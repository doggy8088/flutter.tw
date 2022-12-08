---
title: Dart 2.7 釋出：更安全、更具表現力的 Dart
toc: true
---
上週，我們釋出了 Dart 2.7 SDK 的穩定版本，它可以為開發者提供多項新功能。Dart 語言經過了充實的一年，它是一種針對客戶端最佳化的語言，適用於在任何平臺上開發高效執行的應用。我們今年釋出了 6 個新版本，數十項新功能。我們很欣喜地看到這些功能已經被 Dart 社群廣泛使用。最近的 [GitHub Octoverse](https://octoverse.github.com/) 顯示，根據多個參與方的評估結果，Dart 被認定為 [增長速度最快的程式語言](https://octoverse.github.com/#top-languages) (排名第一)，這一訊息讓我們備受鼓舞。

Dart 2.7 增加了對擴充方法的支援，此外還添加了一個新的程式碼套件，用來處理帶有特殊字元的字串。我們更新了空安全 (已經實現型別安全的可空和非空型別)，還透過 DartPad 帶來了全新的程式碼體驗環境 (而且支援空安全)。在生態系統層級，pub.dev 現在加入了新的點贊 (Like) 功能，使用者們喜歡程式碼包如今更加一目瞭然。Dart 2.7 現在就可以 [從 dart.dev 下載](http://dart.dev/) 並作為 SDK 使用，並且它也包含在 [釋出的 Flutter 1.12 中](https://flutter.cn/posts/announcing-flutter-1-12)。

![](https://devrel.andfun.cn/devrel/posts/2021/05/gw7QIg.png)

## **擴充方法**

Dart 2.7 加入了一個長期以來備受期待的強大新語言功能: 擴充方法。擴充方法可以讓您給任何型別 (包括您無法控制的型別) 新增新功能，並依然享受和常規方法一樣的簡潔輸入體驗以及程式碼自動自動完成功能。

我們來看一個簡單的例子: 如何從為 String 新增解析 int 和 double 的方法。作為應用開發者，我們無法更改 String 類，因為這個類是在 dart:core 程式碼庫中定義的，但是在擴充方法的幫助下，我們就可以親手擴充它！在定義了擴充方法之後，我們就可以在 String 上呼叫新的 parseInt 方法，就如同這個方法是在 String 類中被原生定義的那樣:

```Dart
extension ParseNumbers on String {
  int parseInt() {
    return int.parse(this);
  }

  double parseDouble() {
    return double.parse(this);
  }
}

main() {
  int i = '42'.parseInt();
  print(i);
}
```

**擴充方法是靜態的**

擴充方法是靜態解析、靜態配置的，也就是說，您無法透過動態值來呼叫它們。如下所示，該呼叫在執行時會丟擲例外:

```Dart
  dynamic d = '2';
  d.parseInt();

→ Runtime exception: NoSuchMethodError
```

擴充方法和 Dart 的 [型別推斷](https://dart.dev/guides/language/sound-dart#type-inference) 可以很好地協作，所以在下面這個例子中，變數 "v" 被推斷為 String 類，自然 String 上的擴充方法是可用的:

```Dart
  var v = '1';
  v.parseInt(); // Works!
```

因為擴充方法是靜態解析的，所以它們的速度就和呼叫靜態方法或 helper 方法一樣快，但呼叫語法則要友好很多

**擴充可以擁有型別變數**

因為擴充方法是靜態解析的，所以它們的速度就和呼叫靜態方法或 helper 方法一樣假如我們想在 List 上定義一個擴充，用來獲取序號為偶數的內容列表。那麼我們就會希望讓這個擴充執行在任何型別的列表上，返回和輸入列表相同型別的新列表。為了做到這一點，我們可以把擴充泛型化，並將它的型別引數應用到它擴充的型別和方法裡:

```Dart
extension FancyList<T> on List<T> {
  List<T> get evenElements {
    return <T>[for (int i = 0; i < this.length; i += 2) this[i]];
  }
}
```

**擴充方法是擴充成員**

我們把這個功能稱作 "擴充方法" 是因為，如果您在其他程式語言中使用過相應的語言功能，就會對這個術語感到熟悉。不過在 Dart 中，這個功能更加寬泛: 它還支援使用新的 getter、setter 以及運算子來擴充類別。在上面那個 FancyList 的例子中，evenElements 就是一個 getter。下面則是一個例子，用來展示如何為 String 新增一個用於字串移位的運算子:

```
extension ShiftString on String {
  String operator <<(int shift) {
    return this.substring(shift, this.length) + this.substring(0, shift);
  }
}
```

**來自社群的優秀範例**

我們已經看到 Dart 社群的很多開發者們開始試用擴充方法。下面列出我們見過的幾個優秀範例。

Jeremiah Ogbomo 建立了 [time 程式碼包](https://pub.dev/packages/time)，它在 num (int 和 double 的基底類別) 上使用擴充，從而簡化了 Duration 的建立過程。

```Dart
// Create a Duration via a `minutes` extension on num.
Duration tenMinutes = 10.minutes;

// Create a Duration via an `hours` extension on num.
Duration oneHourThirtyMinutes = 1.5.hours;

// Create a DateTime using a `+` operator extension on DateTime.
final DateTime afterTenMinutes = DateTime.now() + 10.minutes;
```

Marcelo Glasberg 建立了 [i18n (國際化) 程式碼包](https://www.reddit.com/r/FlutterDev/comments/dm288s/dart_extensions_applied_to_i18n_you_have/)，它使用擴充方法來簡化字串的本地化操作:

```
Text('Hello'.i18n) // Displays Hello in English, Hola in Spanish, etc.
```

Simon Leier 建立了 [dartx 程式碼包](https://pub.dev/packages/dartx)，其中包含了多個核心 Dart 型別的擴充，如:

```Dart
var allButFirstAndLast = list.slice(1, -2);   // [1, 2, 3, 4] 
var notBlank = '   .'.isBlank;       // false 
var file = File('some/path/testFile.dart'); 
print(file.name);           // testFile.dart 
print(file.nameWithoutExtension);       // testFile
```

Brian Egan 正在更新廣受歡迎的 RxDart 程式碼套件，使用擴充方法重新定義 API，以便更好地操作流。

## **更安全的字串擷取操作**

Dart 的標準 String 類使用 [UTF-16 編碼](https://en.wikipedia.org/wiki/UTF-16)。這是程式語言的常見選擇，特別是那些需要同時支援裝置本地執行和 Web 端執行的程式語言。

UTF-16 字串通常運作良好，編碼過程對於開發者來說是透明的。然而，在操作字串時，特別是操作那些由使用者輸入的字串時，您可能會發現，某些被使用者認為是字元的東西，和相應的被 UTF-16 編碼系統認為是字元單元的東西，其實並不一致。下面我們來看一個例子，從使用者輸入的字串中擷取前三個字元:

```
var input = ['Resume'];
input.forEach((s) => print(s.substring(0, 3)));

$ dart main.dart
Res
```

目前看來沒有問題；我們打印出了輸入列表中的字串上的前三個字母，結果是 Res。現在我們來想想，假如使用者來自世界上不同的地區，他們輸入的字元中可能包含自己語言特有的符號，比如韓語，他們甚至還會創造性地用表情符號組合來表達出 "簡歷" 的含義:

```
// New longer input list:
var input = ['Resume', 'Résumé', '이력서', '💼📃', 'Currículo'];

$ dart main.dart 
Res
Ré
이력서
💼�
Cur
```

那問題來了。有些字串處理正常，但是 Résumé 和 💼📃 這些 "特殊" 字串呢？先來看 Résumé，為什麼我們的結果字串裡只有兩個字元？再看看 💼📃，這個奇怪的問號又是怎麼回事？這裡的問題涉及到 [Unicode 中的一些不為人知的秘密](https://eev.ee/blog/2015/09/12/dark-corners-of-unicode/)。Résumé 中的字元 é 其實會佔據兩個編碼位: 一個 e，還有一個 "[重音組合符](https://unicode.org/cldr/utility/character.jsp?a=0301)" 。而 [紙捲圖標 📃](https://emojipedia.org/emoji/%F0%9F%93%83/)，確實只佔據一個編碼位，但這個編碼卻是用 U+d83d 和 U+dcc3 代理對 (surrogate pair) 來編碼的。是不是被搞迷糊了？

我們說過，您通常不需要擔心字元和編碼位。如果您要做的只是接收和傳遞完整字串的話，那麼內部編碼系統對您來說就是透明的。但是如果您需要處理字串內部的一些字元，或是需要操控字串的內容，那麼您可能就會遇到麻煩。好訊息是，Dart 2.7 加入了全新的 characters 程式碼包來解決這些問題。這個程式碼包會按照使用者期待的方式處理字串中的字元，這個功能又被叫做 [Unicode 字形群集](https://unicode.org/reports/tr29/#Grapheme_Cluster_Boundaries) (grapheme clusters)。有了 [characters 程式碼包](https://pub.dev/packages/characters)，我們只需稍微改動一下 shortenText() 方法，即可修正程式碼中的錯誤:

```
// Before:
input.forEach((s) => print(s.substring(0, 3)));

// After, using the characters package:
input.forEach((s) => print(s.characters.take(3)));
```

首先，我們要使用便捷的 .characters 擴充方法，從文字中的 String 裡創建出新的 Characters 例項。然後我們就可以使用 take() 方法提取出前 3 個字元。

這個新程式碼套件的技術預覽版已經在 pub.dev 上釋出。很期待聽到大家對這個程式碼套件的反饋。如果您發現了其中的任何問題，請隨時 [告知我們](https://github.com/dart-lang/characters/issues)。

## **空安全預覽**

幾個月前，我們宣佈 [即將在 Dart 中支援空安全](https://medium.com/dartlang/announcing-dart-2-5-super-charged-development-328822024970#0391)，支援安全存取物件，而不會觸發空參考例外。我們為大家帶來空安全靜態分析的預覽。下面我們來看一個頗令人激動的例子:

```Dart
void main() {
  Person('Larry', birthday: DateTime(1973, 03, 26)).describe();
  Person('Sergey').describe();
}

class Person {
  String firstName;
  DateTime birthday;

  Person(this.firstName, {this.birthday});

  void describe() {
    print(firstName);
    int birthyear = birthday?.year;
    print('Born ${DateTime.now().year - birthyear} years ago');
  }
}
```

如果我們執行這段程式碼，它就會在執行第二個 Person 的 describe 方法時崩潰，並丟擲一個空指標例外。因為這個人沒有設定生日。我們在程式設計時犯了一個錯誤: 雖然我們已經預料到有些人的生日是未知的，在構造方法裡中將 "生日" 設為可選，並在 birthday?.year 中有測試空生日，但我們忘了去處理 birthyear 也為空的情況。

現在我們把這段程式碼貼上進我們新推出的 [空安全程式碼體驗環境](https://nullsafety.dartpad.dev)，它是 DartPad 的一個特殊版本，其中包含靜態分析功能 (是空安全功能的子集) 的技術預覽。甚至都不需要執行程式碼，我們就可以看到 3 個問題:

![](https://devrel.andfun.cn/devrel/posts/2021/05/0zBwPq.png)

透過依次修復這些分析錯誤之後，我們就可以盡享空安全帶來的好處了。請在空安全體驗環境中試著做出如下修改 (並 [最終得到空安全程式碼](https://gist.github.com/mit-mit/c210bfb088545e69ba9231ee459615ba)):

1. 宣告 birthday 可空，將 DateTime birthday 修改為 DateTime? birthday
2. 聲明當 birthday 為空時 birthyear 可空，將 int birthyear 修改為 int? birthyear
3. 將第二個 print 呼叫放進空測試中:if (birthyear != null) {...}

希望這個例子可以讓您明白我們想使用空安全功能帶來何種體驗。如前所示，這個體驗環境只是空安全功能中的一部分的早期技術預覽，開發工作仍在進行。我們正在努力在 Dart SDK 中提供空安全功能的第一個 beta 版本。以下是我們準備在 beta 版中推出的內容:

1. 可空和非空參考的完整實現

2. 將空安全整合至 Dart 的型別推斷和 smart promotion (例如，允許在分配或空檢查後安全存取可空變數)
3. 修改 Dart [核心程式碼庫](https://dart.dev/guides/libraries)，使之宣告可空和非可空型別
4. 新增遷移工具，這個工具可以自動完成大部分的程式碼升級操作，協助開發者升級 Dart 應用和程式碼包

此項工作完成後，我們會在 beta 版 SDK 中釋出它，供大家在自己的應用和程式碼套件中使用。我們還準備在新功能實現後對空安全體驗環境進行更新。

我們知道很多開發者都想盡快用上空安全功能，大家可以在自己方便的時候展開程式碼遷移工作，在做好準備之後再使用這項功能。尚未採用這項功能的程式碼庫和程式碼包將可以依賴那些已經採用這項功能的程式碼庫，反之亦然。

在今後的幾個月中，我們還會帶來更多關於空安全的訊息，比如說，我們會提出更加詳細的建議，引導大家為遷移做準備。

## **在 pub.dev 上為程式碼包點贊**

我們還在 pub.dev 上釋出了 "為程式碼包點贊" 功能，方便大家 "親手" 表明自己對程式碼套件的喜愛。如果您想要為一個程式碼包點贊，只需點選程式碼包詳情資訊旁邊的大拇指圖示即可。

![△ pub.dev 程式碼包詳情頁增加了點贊按鈕](https://devrel.andfun.cn/devrel/posts/2021/05/Vw0mJN.png)

> △ pub.dev 程式碼包詳情頁增加了點贊按鈕

目前我們並未考慮在我們的總體評分模型中納入點贊數，但我們計劃在今後的版本中納入這個指標。我們還打算對我們的搜尋介面和列表頁面進行視覺更新，在其中強調程式碼套件的點贊資訊。

## **謝謝大家**

我們代表 Dart 團隊感謝大家，感謝 Dart 社群的所有成員，謝謝您們持續不斷的支援！請繼續向我們提供反饋，並繼續  [參與 Dart 相關討論，繼續融入 Dart 社群](https://dart.dev/community)。很顯然，沒有 Dart 社群的支援，我們不可能完成這個優異的開源專案。

對 Dart 來說，2019 年是激動人心的一年，但我們並不會就此止步。我們為 2020 年制定了雄心勃勃的計劃。我們準備釋出一些功能的穩定版本，這些功能包括 [dart:ffi](https://dart.dev/guides/libraries/c-interop)、[空安全](https://github.com/dart-lang/language/issues/110)，以及其他全新功能。請大家開始使用 Dart 2.7，大家可以 [前往 dart.dev 下載](http://dart.dev)，另外最新發布的 [Flutter 1.12](https://flutter.cn/posts/announcing-flutter-1-12) 中也包含它，最近剛剛經過 [重新設計的 DartPad](https://medium.com/dartlang/a-brand-new-dartpad-dev-with-flutter-support-16fe6027784) 中也包含 Dart 2.7。
