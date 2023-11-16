---
title: Dart 2.13 版現已釋出
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/05/mnqClZ.png)

*作者 / Kevin Moore & Michael Thomsen*

Dart 2.13 版現已釋出，其中新增了*類型別名*功能，這是目前使用者呼聲第二高的語言功能。Dart 2.13 還改進了 Dart FFI 以及更好的效能，並且我們還為 Dart 提供了新的官方鏡像。本文將為您奉上 2.12 版中推出的空安全功能的最新資訊，介紹 2.13 版本的新特性，以及 Docker 和 Google Cloud 對 Dart 後端支援的新訊息。另外，還會預告在後續版本中的其他變化。

## **空安全更新**

在今年 3 月份釋出的 [Dart 2.12](https://flutter.cn/posts/announcing-dart-2-12) 中，我們推出了 [健全的空安](https://dart.cn/null-safety) 全功能。空安全可謂是 Dart 最近推出的一項重要功能，旨在幫助您避免空值錯誤 (這類錯誤經常難以發現)，有效提升工作效率。我們希望釋出 package 的開發者能夠及時跟進這項釋出，更新 pub.dev 上分享的 package 以支援空安全。

我們極其欣喜地看到，在釋出後的短短几個月內，空安全就已被廣泛採用，目前 **pub.dev 上前 500 個最受歡迎的 package 中，93% 的 package 已經支援空安全。**在此，謹向如此迅速跟進的所有 package 開發者致以最誠摯的謝意，感謝大家幫助推動整個生態系統不斷向前！

有了這麼多 package 支援空安全，您就可以開始考慮著手將自己的應用遷移到使用空安全的環境。要開始遷移，請首先使用 `dart pub outdated` 檢查應用的依賴項。詳細步驟，請參閱 [空安全遷移指南](https://dart.cn/null-safety/migration-guide#step1-wait)。我們還調整了 `dart create` 和 `flutter create` 範本，現在它們在新的應用程式和 package 中預設啟用空安全。

## **推出類型別名功能**

類型別名是 2.13 版中新增的語言功能，也是廣大開發者翹首以盼的功能，曾在語言問題的反饋中高居 [第二位](https://github.com/dart-lang/language/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)。有了這一功能，開發者就能夠建立函式型別的別名，但不能建立其他任何型別。

利用類型別名您可以為任何現有的型別建立新的名稱，然後將新建立的名稱用在原始型別可以出現的任何地方。建立新名稱並不會真的定義一個新型別，只不過是引入一個簡短的別名而已。該別名甚至能透過型別等同測試:

```Dart
typedef Integer = int;

void main() {
  print(int == Integer); // true
}
```

那麼，類型別名可以怎麼用？一種常見的用法是給某型別指定一個更短或更具描述性的名稱，以便您的程式碼更易於理解和維護。

比如，給 JSON 型別指定別名就是種不錯的用法 (此範例由 GitHub 使用者 [Levi-Lesches](https://github.com/Levi-Lesches) 提供，特此感謝)。在下列範例中，我們可以定義一個新的類型別名 `Json`，它將一個 JSON 文件描述為一個 map，其鍵為 `String`，值為任意值 (使用動態型別)。這樣，當我們定義名為 `fromJson` 的建構函式和 json get函式時，就能使用該 Json 類型別名。

```Dart
typedef Json = Map<String, dynamic>;

class User {
  final String name;
  final int age;

  User.fromJson(Json json) : 
    name = json['name'],
    age = json['age'];

  Json get json => {
    'name': name, 
    'age': age,
  };
}
```

您也可以對指代某個類別的類型別名呼叫建構函式，比如以下範例就非常合規:

```Dart
main() {
  var j = Json();
  j['name'] = 'Michael';
}
```

透過使用類型別名來指代複雜型別，可以讓讀者更容易理解您程式碼的不變數。例如，以下程式碼定義了一個類別型別名來描述鍵值為泛型型別 X、值為型別 `List<X>` 的對映。如果給該型別指定一個具有單一型別引數的名稱，對映的常規結構在程式碼讀者眼中會變得更為清晰。

```Dart
typedef MapToList<X> = Map<X, List<X>>;
void main() {
  MapToList<int> m = {};
  m[7] = [7]; // OK
  m[8] = [2, 2, 2]; // OK
  for (var x in m.keys) {
    print('$x --> ${m[x]}');
  }
}

=>

7 --> [7]
8 --> [2, 2, 2]
```

如果您嘗試使用不匹配的型別，將出現分析錯誤:

```Dart
m[42] = ['The', 'meaning', 'of', 'life']; 


=>

The element type 'String' can't be assigned to the list type 'int'.
```

您甚至可以使用類型別名來重新命名公共庫中的類別。假設現在公共庫中有一個 `PoorlyNamedClass` 類，您想要將它重新命名為 `BetterNamedClass`。如果您只是重新命名該類，那麼您的 API 客戶那邊將會出現突發編譯錯誤。而使用類型別名，則不會出現這一問題，您可以隨意重新命名，只不過要先為舊的類別名稱稱定義一個新的類型別名，再給舊名稱新增幾行 `@Deprecated` 註解。這樣，使用 `PoorlyNamedClass` 的程式碼雖然會出現警告，但仍可繼續編譯並照舊正常執行，讓使用者有時間升級其程式碼。

```Dart
mylibrary.dart:

class BetterNamedClass {}

@Deprecated('Use BetterNamedClass instead')
typedef PoorlyNamedClass = BetterNamedClass;

main.dart


import 'mylibrary.dart';


void main() {
  PoorlyNamedClass p;
}

=>

'PoorlyNamedClass' is deprecated and shouldn't be used. Use BetterNamedClass instead.
```

下面介紹實現 `BetterNamedClass` 和棄用 `PoorlyNamedClass` 的方法 (在一個名為 `mylibrary.dart` 的檔案中)。

```Dart
class BetterNamedClass {...}

@Deprecated('Use BetterNamedClass instead')
typedef PoorlyNamedClass = BetterNamedClass;
```

下面是嘗試使用 `PoorlyNamedClass` 時會發生的情況:

```Dart
import 'mylibrary.dart';
void main() {
 PoorlyNamedClass p;
}
=>
'PoorlyNamedClass' is deprecated and shouldn't be used. Use BetterNamedClass instead.
```

類型別名功能從 Dart 2.13 版開始即可使用，要啟用此功能，需要將您 pubspec 中版本較低的 Dart SDK 約束設定為最低 2.13 版，如下所示:

```
environment:
  sdk: ">=2.13.0 <3.0.0"
```

此功能支援向後相容，這要歸功於 [語言的版本管理](https://dart.cn/guides/language/evolution#language-versioning)。也就是說，SDK 約束版本低於 2.13 的 package 可以安全地參考 2.13 版 package 中定義的類型別名，儘管 2.13 版之前的 package 不能定義其自己的類型別名。

## **Dart 2.13 FFI 的變化**

我們還在 Dart FFI (這是用來呼叫 C 語言程式碼的互操作機制) 中引入了一些新功能。

首先，FFI 現在支援包含內聯陣列 ([#35763](https://github.com/dart-lang/sdk/issues/35763)) 的結構。假設某 C 語言結構具有如下內聯陣列:

```Dart
struct MyStruct {
  uint8_t arr[8];
}
```

現在，只需將包含一個類別型實參的元素型別指定給 `Array`，即可直接將該結構體封裝在 Dart 中，如下所示:

```Dart
class StructInlineArray extends Struct {
  @Array(8)
  external Array<Uint8> arr;
}
```

其次，FFI 現在支援封裝結構體 ([#38158](https://github.com/dart-lang/sdk/issues/38158))。結構體通常都被放置在記憶體中，以便其位於地址邊界內的成員能夠被 CPU 更輕鬆地存取。使用 [封裝結構體](http://www.catb.org/esr/structure-packing/) 時，為了減少整體記憶體佔用量，經常會以平台特有的方式忽略一些填充位元組。藉助新的 `@Packed(<alignment>)` 註解，您可以輕鬆指定填充位元組。例如，下列程式碼建立的結構體就指定其在記憶體中時的位元組對齊為 4。

```Dart
@Packed(4)
class TASKDIALOGCONFIG extends Struct {
  @Uint32()
  external int cbSize;
  @IntPtr()
  external int hwndParent;
  @IntPtr()
  external int hInstance;
  @Uint32()
  external int dwFlags;
…
}
```

## **Dart 2.13 在效能方面的提升**

我們一直在不斷努力降低 Dart 程式碼的應用體量和記憶體佔用量。在大型 Flutter 應用中，經過 AOT 編譯 Dart 程式的元資料的內部結構可能要佔用非常可觀的記憶體。這些元資料的存在大多是為了實現熱重載、互動式除錯，以及格式化可讀堆疊軌跡等功能，這些功能在需要部署的應用中從不會用到。過去幾年來，我們一直在重構 Dart 原生執行時環境，以便儘可能多地消除這種開銷。其中一些改進適用於所有以版本模式建構的 Flutter 應用，而有些則需要使用 [--split-debug-info](https://flutter.cn/docs/perf/app-size#reducing-app-size) 標誌將 AOT 編譯應用中的除錯資訊拆分出來，從而放棄可讀的堆疊軌跡。

Dart 2.13 在記憶體消耗上取得了很大的進步，在使用 `--split-debug-info` 時，程式元資料佔用的空間量降幅顯著。例如，Flutter Gallery 的空間佔用降幅達到 30%: 在 --split-debug-info 模式下，程式元資料在 Dart 2.12 中要佔用 5.7Mb，而在 Dart 2.13 中僅需 3.7Mb。以 Flutter Gallery 應用為例，在 Android 平臺上，包含除錯資訊的釋出 APK 大小為 112.4MB，不包含的情況下大小為 106.7MB (總體積減少了 5%)。該 APK 中包含了大量的資源。僅從 APK 內部的元資料體積來說，從 Dart 2.12 平臺上的 5.7MB 減少至 Dart 2.13 平臺上的 3.7MB (減少了35%！)。

如果對您來說應用體量和記憶體佔用量比較重要，可以使用 [--split-debug-info](https://flutter.cn/docs/perf/app-size#reducing-app-size) 標誌省略除錯資訊。請注意，一旦這麼做，您需要使用 [symbolize 命令](https://flutter.cn/docs/deployment/obfuscate#reading-an-obfuscated-stack-trace) 來重新使堆疊軌跡可讀。

## **Dart 官方 Docker 鏡像釋出以及 Cloud 支援**

Dart 現在在 [官方鏡像](https://docs.docker.com/docker-hub/official_images/) 中可用，雖然 Dart 早已提供了 Docker 鏡像，但為了遵循最佳實踐，這些 [新的 Dart 鏡像](https://hub.docker.com/_/dart) 是由 Docker 進行測試和驗證的。它們還支援 AOT 編譯，可以大大減少建構容器的大小，並且可以在容器環境中提升部署速度——如 [Cloud Run](https://cloud.google.com/run)。

雖然 Dart 始終專注於使 Flutter 等應用框架在每個螢幕上構建出色的介面，但我們意識到，大多數使用者體驗的背後至少有一個託管服務。透過讓 Dart 輕鬆建構後端服務來支援全棧體驗，開發者可以使用與前端 widget 相同的語言和業務邏輯，將他們的應用擴充到雲端。

通常來說，將 Dart 用於 Flutter 應用程式的後端，特別符合 Google 無伺服器管理平台 Cloud Run 的簡單性和可擴充性。這也包括零擴充，意味著當您的後端不處理任何請求時，就不會產產生本。我們與 Google Cloud 團隊合作，提供 [Dart 的函式框架](https://pub.dev/packages/functions_framework)，這是一個 packages、工具和例項的集合，使開發者們能夠輕鬆地編寫 Dart 函式，以取代處理 HTTP 請求和 CloudEvents 的完整伺服器部署。

您可以檢視我們的 [Google Cloud 官方文件](https://dart.cn/server/google-cloud) 以便開始使用。

## **後續更新預告**

在接下來的版本中，還會有一些令人激動的改變。和以往一樣，您可以使用 [language funnel](https://github.com/dart-lang/language/projects/1) 追蹤器留意我們的後續工作。

我們一直努力改進的一個方面是，為 Dart 和 Flutter 定義一組新的 canonical lint。lint 是配置 Dart [靜態分析](https://dart.cn//guides/language/analysis-options) 的一種高效方式，但由於可能有成百上千個 lint 要啟用或禁用，有時可能會難以抉擇。眼下，我們正打算定義兩組要在 Dart 和 Flutter 專案中預設應用的 canonical lint。預計這兩組 lint 將在下一個穩定版中預設啟用。如果您想要提前預覽，請檢視 [lints](https://pub.dev/packages/lints) 和 [flutter_lints](https://pub.dev/packages/flutter_lints) 這兩個 package。

最後，如果您深度嵌套了 Dart VM 執行時環境，請注意，我們打算棄用其現有的機制。我們將用一個基於 Dart FFI 的更快、更靈活的模型取代它 (請參閱追蹤問題 [#45451](https://github.com/dart-lang/sdk/issues/45451))。

## **Dart 2.13 版現已釋出**

Dart 2.13 版現已在 [Dart 2.13](https://dart.cn/get-dart) 和 [Flutter 2.2](https://dart.cn/get-dart) SDK 中推出，此版本新增了類型別名功能，還改進了 FFI。

如果您一直在等待將自己的依賴項遷移到空安全環境的時機，不妨使用 [dart pub outdated](https://dart.cn/null-safety/migration-guide) 再次檢查一下。目前，前 500 個最受歡迎的 package 中，93% 的 package 都已遷移，現在沒準就是您遷移的好時機。在此，謹向那些已經遷移的開發者致以最衷心的感謝！

歡迎試用本指南中介紹的新功能和改進後的功能，並將您使用後的感想告訴我們。
