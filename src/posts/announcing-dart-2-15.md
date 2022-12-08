---
title: Dart 2.15 現已釋出
toc: true
---

*作者 / Michael Thomsen, Dart & Flutter Product Manager, Google*

我們已經正式釋出了 Dart SDK 的 2.15 版本，該版本新增了可快速併發的工作器 isolate、新的建構函式拆分 (tear-off) 語言特性、經過改進的 dart:core 庫列舉支援、package 釋出者相關的新功能，等等。

![](https://devrel.andfun.cn/devrel/posts/2021/12/8CA8pZ.png)

## **工作器 isolate 的快速併發**

如今，幾乎所有現代裝置都使用多核 CPU，可以並行執行多個任務。對於大多數 Dart 程式來說，這些核心的使用情況對開發者而言是透明的: 預設情況下，Dart 執行時系統在單個核心上執行所有的 Dart 程式碼，不過會使用其他核心來執行系統級任務，比如非同步輸入/輸出，包括寫入檔案或者呼叫網路等。

不過您自己的 Dart 程式碼可能也需要併發執行。例如，您可能需要展示一個連續的動畫，同時執行一個長時間執行的任務，比如解析一個大型 JSON 檔案。如果額外任務花了太長時間，就可能會導致介面卡頓或延遲。如果將這些額外的任務移動到另一個單獨的核心，動畫就可以在主執行執行緒上繼續執行而不受干擾。

Dart 的併發模型基於 [isolate](https://dart.dev/guides/language/concurrency)，isolate 是一種相互隔離的獨立執行單元，這是為了避免出現與共享記憶體相關的大量併發程式設計錯誤，如 [資料爭用等競態條件](https://en.wikipedia.org/wiki/Race_condition#In_software)。Dart 透過禁止在 isolate 之間共享任何可變物件來避免這些錯誤，並使用 [訊息傳遞](https://dart.dev/guides/language/concurrency#sending-multiple-messages-between-isolates) 在 isolate 之間交換狀態。在 Dart 2.15 中，我們對 isolate 進行了許多實質性的改進。

我們首先重新設計和實現了 isolate 的工作方式，引入了一個新概念: isolate 組。Isolate 組中的 isolate 共享各種內部資料結構，這些資料結構則表示正在執行的程式。這使得組中的單個 isolate 變得更加輕便。如今，因為不需要初始化程式結構，在現有 isolate 組中啟動額外的 isolate 比之前快 100 多倍，並且產生的 isolate 所消耗的記憶體減少了 10 至 100 倍。

雖然 isolate 組仍然阻止在 isolate 間共享存取可變物件，但由於 isolate 組使用共享堆實現，這也讓其擁有了更多的功能。我們可以將物件從一個 isolate 傳遞到另一個 isolate，這可用於執行返回大量記憶體資料的任務的工作器 isolate。例如，工作器 isolate 透過網路呼叫獲得資料，將該資料解析為大型 JSON 物件圖，然後將這個 JSON 圖返回到主 isolate 中。在推出 Dart 2.15 之前，執行該操作需要深度複製，如果複製花費的時間超過幀預算時間，就會導致介面卡頓。

在 Dart 2.15 中，工作器 isolate 可以呼叫 [`Isolate.exit()`](https://api.dart.cn/stable/2.15.0/dart-isolate/Isolate/exit.html)，將其結果作為引數傳遞。然後，Dart 執行時將包含結果的記憶體資料從工作器 isolate 傳遞到主 isolate 中，無需複製，且主 isolate 可以在固定時間內接收結果。我們已經在 [Flutter 2.8](https://mp.weixin.qq.com/s/22Ylncb3V95MGkMBRSrZoA) 中更新了 [compute()](https://api.flutter-io.cn/flutter/foundation/compute-constant.html) 實用函式，來利用 Isolate.exit()。如果您已經在使用 compute()，那麼在升級到 Flutter 2.8 後，您將自動獲得這些效能提升。

最後，我們還重新設計了 isolate 訊息傳遞機制的實現方式，使得中小型訊息的傳遞速度提高了大約 8 倍。傳送訊息的速度明顯更快，而接收資訊幾乎總是在恆定的時間內完成。另外，我們擴充了 isolate 可以相互發送的物件種類，增加了對函式型別、閉套件和堆疊追蹤物件的支援。請參閱 [SendPort.send()](https://api.dart.cn/stable/2.15.0/dart-isolate/SendPort/send.html) 的 API 文件瞭解詳情。

要了解有關如何使用 isolate 的更多資訊，請參閱我們為 Dart 2.15 新增的官方文件 [Dart 中的併發](https://dart.cn/guides/language/concurrency)，以及更多 [程式碼範例](https://github.com/dart-lang/samples/tree/master/isolates)。

## **新語言特性: 建構函式拆分**

在 Dart 中，您可以使用函式名稱建立一個函式物件，該物件指向另一個物件的函式。在以下範例中，main() 方法的第二行示範了將 `g` 指向 `m.greet` 的語法:

```Dart
class Greeter {
  final String name;
  Greeter(this.name);

  void greet(String who) {
    print('$name says: Hello $who!');
  }
}
void main() {
  final m = Greeter('Michael');
  final g = m.greet; // g holds a function pointer to m.greet.
  g('Leaf'); // Invokes and prints "Michael says: Hello Leaf!"
}
```

在使用 Dart 核心函式庫時，這種函式指標 (也被稱為函式*拆分*) 經常出現。下面是透過傳遞函式指標在 iterable 上呼叫 `foreach()` 的範例:

```Dart
final m = Greeter('Michael');
['Lasse', 'Bob', 'Erik'].forEach(m.greet);
// Prints "Michael says: Hello Lasse!", "Michael says: Hello Bob!",
// "Michael says: Hello Erik!"
```

在之前的版本中，Dart SDK 不支援建立建構函式的拆分 (語言問題 [#216](https://github.com/dart-lang/language/issues/2))。這就有點煩人，因為在許多情況下，例如建構 Flutter 介面時，就需要用到建構函式的拆分。從 Dart 2.15 開始，我們支援這種語法。以下是建構包含三個 `Text` widget 的 `Column` widget 的範例，透過呼叫 `.map()` 將 Text 建構函式的拆分傳遞給 `Column` 的子項。

```Dart
class FruitWidget extends StatelessWidget {
 @override
 Widget build(BuildContext context) {
   return Column(
       children: ['Apple', 'Orange'].map(Text.new).toList());
 }
}
```

`Text.new` 指 `Text` 類別的預設建構函式。您也可以參考命名建構函式，例如 `.map(Text.rich)`。

## **相關語言變化**

在實現建構函式拆分時，我們也藉此機會修復了現有的函式指標功能中的一些不一致問題。現在可以特化泛型方法來建立非泛型方法:

```Dart
T id<T>(T value) => value;
var intId = id<int>; // New in 2.15.
int Function(int) intId = id; // Pre-2.15 workaround.
```

您甚至可以特化一個泛型函式物件來建立一個非泛型函式物件:

```Dart
const fo = id; // Tear off `id`, creating a function object.
const c1 = fo<int>; // New in 2.15; error before.
```

最後，Dart 2.15 清理了涉及泛型的型別字面量:

```Dart
var y = List; // Already supported.
var z = List<int>; // New in 2.15.
var z = typeOf<List<int>>(); // Pre-2.15 workaround.
```

**改進 dart:core 庫中的列舉**

我們為 dart:core 庫的列舉 API 添加了許多最佳化 (語言問題 [#1511](https://github.com/dart-lang/language/issues/1511))。現在您可以透過 `.name` 獲取每個列舉值的 `String` 值:

```Dart
enum MyEnum {
 one, two, three
}
void main() {
 print(MyEnum.one.name);  // Prints "one".
}
```

還可以按名稱查詢列舉值:

```Dart
print(MyEnum.values.byName('two') == MyEnum.two);  // Prints "true".
```

最後，您可以獲得所有名稱-值對的對映:

```Dart
final map = MyEnum.values.asNameMap();
print(map['three'] == MyEnum.three);  // Prints "true".
```

請參閱此 [Flutter PR](https://github.com/flutter/flutter/pull/94496/files) 檢視這些新 API 的使用範例。

## **壓縮指標**

Dart 2.15 增加了對壓縮指標的支援，這樣，如果只需要支援 32 位的地址空間 (最多 4 GB 記憶體)，則 64 位 SDK 可以使用更加節省空間的指標表示形式。壓縮指標顯著減少了記憶體佔用，在對 Google Pay 應用的內部測試中，我們發現 Dart 堆的體積減少了大約 10%。

壓縮指標意味著無法處理 4 GB 以上的可用 RAM，因此該功能只存在於 Dart SDK 的配置選項中，只能在建構 SDK 時由 Dart SDK 的嵌入器啟用。Flutter SDK 2.8 版已為 Android 建構啟用此配置，Flutter 團隊正在考慮在後續版本中 [為 iOS 建構啟用此配置](https://github.com/flutter/flutter/issues/94753)。

## **Dart SDK 中包含 Dart DevTools**

以往 Dart SDK 不提供除錯和效能工具的 [DevTools 套件](https://dart.dev/tools/dart-devtools#using-devtools-with-a-command-line-app)，您需要單獨下載。從 Dart 2.15 開始，下載 Dart SDK 時也會獲取 DevTools，無需進一步的安裝步驟。有關在 Dart 命令列應用中使用 DevTools 的更多資訊，請參閱 [DevTools 文件](https://dart.dev/tools/dart-devtools#)。

## **面向 package 釋出者的新 pub 功能**

Dart 2.15 SDK 在 `dart pub` 開發者命令和 [pub.dev](https://pub.dev) package repo 中還新增了兩個功能。

首先，為 package 釋出者新增了一個安全功能，用於檢測釋出者在 pub package 中意外發布 secret，例如 Cloud 或 CI 憑據。在瞭解到 GitHub repo 中 [每天都有數以千計的 secret 被洩露後](https://www.ndss-symposium.org/wp-content/uploads/2019/02/ndss2019_04B-3_Meli_paper.pdf)，我們便決定新增這個洩露檢測功能。

洩露檢測作為 `dart pub publish` 命令中的預釋出驗證的一部分執行。如果它在即將釋出的檔案中檢測到潛在的 secret，`publish` 命令會退出，而不進行釋出，並列印如下輸出:

```
Publishing my_package 1.0.0 to https://pub.dartlang.org:
Package validation found the following errors:
* line 1, column 1 of lib/key.pem: Potential leak of Private Key detected.
╷
1 │ ┌ - - -BEGIN PRIVATE KEY - - -
2 │ │ H0M6xpM2q+53wmsN/eYLdgtjgBd3DBmHtPilCkiFICXyaA8z9LkJ
3 │ └ - - -END PRIVATE KEY - - -
╵
* line 2, column 23 of lib/my_package.dart: Potential leak of Google OAuth Refresh Token detected.
╷
2 │ final refreshToken = "1//042ys8uoFwZrkCgYIARAAGAQSNwF-L9IrXmFYE-sfKefSpoCnyqEcsHX97Y90KY-p8TPYPPnY2IPgRXdy0QeVw7URuF5u9oUeIF0";
```

在極少數情況下，此項檢測可能會出現誤報，將您實際上打算釋出的內容或檔案標記為潛在洩露。在這些情況下，您可以將檔案新增到 [許可名單](https://dart.cn/tools/pub/pubspec#false_secrets) 中。

其次，我們還為釋出者添加了另一個功能: 撤銷已釋出的 package 版本。當釋出了有問題的 package 版本時，我們通常的建議是釋出一個小幅升級的新版本來修復意外問題。但在極少數情況下，例如您尚未修復這些問題，或是您在原打算只發佈一個次要版本時意外發布了一個主要版本，那麼您就可以使用新的 [package 撤銷功能](https://dart.cn/tools/pub/publishing#retract)，作為最後的補救方法。此功能在 pub.dev 的管理介面中提供:

![](https://devrel.andfun.cn/devrel/posts/2021/12/Cr4RZ4.png)

在 package 版本被撤銷後，pub 客戶端在 `pub get` 或 `pub upgrade` 中將不再解析該版本。如果有開發者已經解析該撤銷的版本 (並存在於他們的 `pubspec.lock` 檔案中)，他們將在下次執行 `pub` 時看到警告:

```
$ dart pub get
Resolving dependencies…
mypkg 0.0.181-buggy (retracted, 0.0.182-fixed available)
Got dependencies!
```

## **檢測雙向 Unicode 字元的安全性分析 (CVE-2021–22567)**

最近發現了一個涉及雙向 Unicode 字元的通用程式語言漏洞 ([CVE-2021–42574](https://nvd.nist.gov/vuln/detail/CVE-2021-42574))。這個漏洞影響了大多數支援 Unicode 的現代程式語言。下面的 Dart 原始碼示範了這個問題:

```Dart
main() {
 final accessLevel = 'user';
 if (accessLevel == 'user .⁦// Check if admin⁩ ⁦') {
   print('You are a regular user.');
 } else {
   print('You are an admin.');
 }
}
```

您可能會認為該程式會打印出 *You are a regular user.*，但實際上它打印出的是 *You are an admin.*！透過使用包含雙向 Unicode 字元的字串，您就可能會造成這一漏洞。這些雙向字元針對在同一行的文字，可以將文字的方向由從左到右更改為從右到左，反之亦然。雙向字元文字在螢幕上的呈現與實際文字內容截然不同。您可以進一步檢視此 [GitHub gist 範例](https://gist.github.com/mit-mit/7dda00ca6278ce7d2555f78d59d9e67b?h=1)。

針對此漏洞的緩解措施包括使用檢測雙向 Unicode 字元的工具 (編輯器、程式碼審查工具等)，以便開發者發現它們，並在知情的情況下使用這些字元。上面提到的 GitHub gist 檔案檢視器便是發現這些字元的工具的一個例子。

Dart 2.15 引入了進一步的緩解措施 ([Dart 安全建議 CVE-2021–22567](https://github.com/dart-lang/sdk/security/advisories/GHSA-8pcp-6qc9-rqmv))。現在，Dart 分析器會掃描雙向 Unicode 字元，並標記對它們的任何使用:

```
$ dart analyze
Analyzing cvetest...                   2.6s
info • bin/cvetest.dart:4:27 • The Unicode code point 'U+202E'
      changes the appearance of text from how it's interpreted
      by the compiler. Try removing the code point or using the
      Unicode escape sequence '\u202E'. •
      text_direction_code_point_in_literal
```

我們建議用 Unicode 轉義序列替換這些字元，這樣它們就可在任何文字編輯器或檢視器中顯示出來。或者，如果您確實正當使用了這些字元，您可以在使用這些字元的程式碼行之前新增覆蓋陳述式來禁用警告:

```
// ignore: text_direction_code_point_in_literal
```

## **使用第三方 pub 伺服器時的 pub.dev 憑據漏洞 (CVE-2021–22568)**

我們也釋出了第二個與 pub.dev 相關的 Dart 安全建議: [CVE-2021–22568](https://github.com/dart-lang/sdk/security/advisories/GHSA-r32f-vhjp-qhj7)。此建議針對可能將 package 釋出到第三方 pub package 伺服器 (例如私人或公司內部 package 伺服器) 的 package 釋出者。僅將 package 釋出到公開 pub.dev repo (標準配置) 的開發者 **不受此漏洞的影響**。

如果您已經將 package 釋出至第三方 repo，那麼漏洞是: 用於在第三方 repo 進行身份驗證的 OAuth2 臨時 (一小時) 存取令牌可能被誤用，以在公開 pub.dev repo 上進行身份驗證。因此惡意的第三方 pub 伺服器可能會使用存取令牌，在 pub.dev 上冒充您，併發布 package。如果您已經將 package 釋出到一個不受信任的第三方 package repo，請考慮審查您的帳號在 pub.dev 公開 package repo 上的所有活動。我們推薦您使用 [pub.dev 活動日誌](https://pub.dev/my-activity-log) 進行檢視。

## **最後**

希望您喜歡 [已經推出](https://dart.cn/get-dart) 的 Dart 2.15 中的新功能。這是我們今年的最後一個版本，我們想借此機會表達我們對美妙的 Dart 生態系統的感謝。感謝大家的寶貴反饋，以及對我們一直以來的支援，感謝大家在過去的一年中在 [pub.dev](https://pub.dev) 上釋出的數千個 package，它們豐富了我們的生態系統。我們迫切期待明年再次投入工作，我們計劃在 2022 年推出很多激動人心的內容。預祝大家新年快樂，好好享受即將到來的假期吧！
