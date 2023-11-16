---
title: Dart 2.17 正式釋出
toc: true
keywords: Dart正式版, Dart2.17
description: Dart 程式語言圍繞生產力和平台整合相關的更新一覽。
image:
    path: https://files.flutter-io.cn/posts/flutter-cn/2022/dart-2-17/dart-2-17-hero.png
    width: 2000
    height: 850
---
*文/ Michael Thomsen, Google Dart 團隊產品經理，2022 年 5 月 12 日發表於 Dart 官方部落格*

隨著 [Flutter 3]({{site.main-url}}/posts/introducing-flutter-3) 在本次 I/O 大會的釋出，我們也同時正式釋出了 Dart 2.17 穩定版 SDK。這個版本的釋出是圍繞著我們的核心主題建構的，即：領先的生產力和平台可移植性。

Dart 2.17 提供了新的語言特性：**列舉支援成員變數、改進的超類引數繼承，以及更為靈活的命名引數**。我們同時為 `package:lints` 開啟了 2.x 版本，這是一套官方的 lint 規則，是根據我們總結的 Dart 最佳實踐整合而成的一個 lint 規則集。與此同時，我們也更新了核心函式庫的 API 文件，為其帶來了豐富的範例程式碼。並且，為了改善平台整合特性，我們在 Flutter 外掛中提供了一個新的模版，使用 `dart:ffi` 與原生平台進行 C 語言的互操作、對 RISC-V 指令集提供實驗性支援，以及對 macOS 和 Windows 可執行檔案的簽名支援。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/dart-2-17/dart-2-17-hero.png)

## 程式語言新特性助力生產力提升

我們一直在持續地改進 Dart 程式語言，不斷新增新特性以及改進現有的特性，以助力開發者們工作效率的提升。Dart 2.17 增加了對列舉成員變數的支援，優化了在建構函式中使用命名引數的方式，並且開始使用繼承超類別的引數以減少冗長和重複的程式碼。

### 增強的支援成員變數的列舉

列舉非常適合表示一組離散的狀態。例如，我們可以將水描述為 `enum Water { frozen, lukewarm, boiling }`。但如果我們想在 `enum` 上新增一些方法，例如，將每個狀態轉換為溫度，並支援將 `enum` 轉換為 `String`，該怎麼辦？或許我們可以使用擴充方法來新增一個 `waterToTemp()` 方法，但我們必須時刻注意它與 `enum` 的同步。對於 `String` 我們希望覆寫 `toString()` 方法，但它不支援這麼做。

在 Dart 2.17 中現已支援列舉型別的成員變數。這意味著我們可以新增儲存狀態的欄位、設定狀態的建構函式、具有功能的方法，甚至覆寫現有的方法。社群中許多開發者一直有這樣的需求，這是我們在 Dart 程式語言儲存庫的問題追蹤中 [投票排名第三的問題](https://github.com/dart-lang/language/issues?q=is%3Aissue+sort%3Areactions-%2B1-desc+ "Dart 程式語言 GitHub 儲存庫議題按參與度情況排序")。

繼續拿 `Water` 舉例，我們可以新增一個儲存溫度的 `int` 欄位，並新增接收 `int` 的預設建構函式：

```dart
enum Water 
  const Water(this.tempInFahrenheit);

  final int tempInFahrenheit;
}
```

為了確保在建立列舉時建構函式被正常呼叫，我們需要為每一個列舉值附以顯式的呼叫：

```dart
enum Water {
  frozen(32),
  lukewarm(100),
  boiling(212);
}
```

想要支援從列舉轉換為 `String`，我們可以很簡單地覆寫 `toString` 方法，因為 `enums` 也繼承自 `Object`：

```dart
@override
String toString() => "The $name water is $tempInFahrenheit F.";
```

如此一來，你就有了一個可以輕鬆例項化完整功能的列舉類，並且可以在任意位置呼叫方法：

```dart
void main() {
  print(Water.frozen); // 列印內容為 “The frozen water is 32 F.”
}
```

這兩種方法的完整範例如下所示，有了這些改動，新版本的程式碼更易於閱讀和維護。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/dart-2-17/extensions-vs-enum.png)

### 超類別的初始化構造

當你的程式碼存在型別繼承關係時，一個常見的做法是將一些建構函式引數傳遞給超類別的建構函式。為此子類別需要 1) 在其建構函式中列出每個引數 2) 使用這些引數呼叫超類別的建構函式。這導致了大量的程式碼重複，使程式碼難以閱讀和維護。

幾位 Dart 社群成員幫助 Dart 實現了這項語言目標。半年前，GitHub 使用者 [@roy-sianez](https://github.com/roy-sianez "GitHub 使用者主頁: @roy-sianez") 提交了一個 [語言問題](https://github.com/dart-lang/language/issues/1855 "Dart 程式語言 GitHub 儲存庫議題 #1855")。他的建議類似於 GitHub 使用者 [@apps-transround](https://github.com/apps-transround "GitHub 使用者主頁: @apps-transround") 先前的 [建議](https://github.com/dart-lang/language/issues/493#issuecomment-879624528 "Dart 程式語言 GitHub 儲存庫議題 #493")：也許我們可以透過引入一個新的方式來表示在超類中指定了一個引數，來解決這個問題。我們認為這是一個好主意，因此已將其實現並新增到了 Dart 2.17 版本中。從以下範例中可以看出，這與 Flutter widget 的程式碼有很強的相關性。實際上當我們將這項特性應用到 Flutter 框架時，我們看到框架總共減少了 [近兩千行程式碼](https://github.com/flutter/flutter/pull/100905/files "Flutter 框架 GitHub 儲存庫拉取請求 #100905")！

![](https://files.flutter-io.cn/posts/flutter-cn/2022/dart-2-17/manual-forwarding-vs-supper-parameters.png)

### 可在任意引數位置使用命名引數

最後，我們改進了方法呼叫時命名引數的方式。在此次更新之前，命名引數的呼叫必須出現在普通引數列表的後面。當你想要提升程式碼可讀性，希望將命名引數寫在靠前的位置但它無法工作時，會覺得非常惆悵。例如下方 `List<T>.generate` 建構函式的呼叫。此次更新之前 `growable` 引數必須放在最後，這會導致這個引數很容易被可能有很多內容的構造引數所影響而錯過。現在你可以根據自己的喜好對它們進行排序，你可以先使用命名引數，最後使用產生器引數。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/dart-2-17/names-agrs-order-changing.png)

更多有關這三項改進的範例，請參閱我們更新的 [列舉](https://github.com/dart-lang/samples/blob/master/enhanced_enums/lib/members.dart "Dart 程式語言特性改進範例程式碼: 列舉")、[超類別的初始化構造](https://github.com/dart-lang/samples/blob/master/parameters/lib/super_initalizer.dart "Dart 程式語言特性改進範例程式碼: 超類別的初始化構造") 和 [命名引數](https://github.com/dart-lang/samples/blob/master/parameters/lib/named_parameters.dart "Dart 程式語言特性改進範例程式碼: 命名引數") 範例程式碼。

## 生產力工具改進

回到生產力的主題，我們圍繞生產力對核心工具進行了一些改進。

在 Dart 2.14 中，我們引入了 `package:lints`，它與 Dart 分析器一起工作以防止你編寫錯誤的程式碼，並使用更規範的規則審查你的 Dart 程式碼。之後分析器中又新增了許多程式碼提示規則，我們對其進行了仔細分類，並從中選擇了 [10 條新的用於所有 Dart 程式碼的程式碼提示規則](https://github.com/dart-lang/lints/blob/main/CHANGELOG.md#200 "10 條新的用於所有 Dart 程式碼的程式碼提示規則") ，以及 [2 條新的專門用於 Flutter 程式碼的程式碼提示規則](https://github.com/flutter/packages/blob/master/packages/flutter_lints/CHANGELOG.md#200 "2 條新的專門用於 Flutter 程式碼的程式碼提示規則")。它們包括確保你匯入的 package 中有正確地在你 pubspec 檔案中宣告、防止濫用對型別引數的空檢查以及確保子屬性格式一致的程式碼提示規則。你可以簡單地使用命令升級到新的 `lints` package：

- 對 Dart package 可以使用:
  `dart pub upgrade —-major-versions lints`
- 對 Flutter package 可以使用:
  `flutter pub upgrade —-major-versions flutter_lints`

`SecureSockets` 通常用於啟用使用 TLS 和 SSL 保護的 TCP 套接字連線。在 Dart 2.17 之前，因為沒有辦法檢查安全資料流量，在開發過程中除錯這些加密連線變得十分棘手。現在我們添加了對指定 `keyLog` 檔案的支援，指定後，當與伺服器交換新的 TLS 金鑰時，[NSS 金鑰日誌格式](https://firefox-source-docs.mozilla.org/security/nss/legacy/key_log_format/index.html "NSS 金鑰日誌格式") 中的一行文字將附加到檔案中。這將使網路流量分析工具 (例如 [Wireshark](https://gitlab.com/wireshark/wireshark/-/wikis/TLS#tls-decryption "使用 Wireshark 解密 TLS 資料")) 能夠解密透過套接字傳送的內容。更多詳細資訊，請參閱`SecureSocket.connect()` 的 [API 文件](https://api.dart.tw.gh.miniasp.com/stable/2.17.0/dart-io/SecureSocket/connect.html "SecureSocket 類別的 connect API 文件")。

`dart doc` 產生的 API 文件是大多數 Dart 開發者學習新 API 的重要內容之一。雖然我們的核心庫 API長期以來都有豐富的文字描述，但許多開發者告訴我們，他們更喜歡透過閱讀範例程式碼來學習 API。在 Dart 2.17 中，我們檢查了所有主要的核心庫，為瀏覽量排名的前 200 個頁面添加了詳實的範例程式碼。你可以對比 `dart:convert` 在 [Dart 2.16](https://api.dart.tw.gh.miniasp.com/stable/2.16.2/dart-convert/dart-convert-library.html "Dart 2.16 版的 dart:convert API 文件") 和 [2.17](https://api.dart.tw.gh.miniasp.com/stable/2.17.0/dart-convert/dart-convert-library.html "Dart 2.17 版的 dart:convert API 文件") 的文件頁面檢視這些改變，希望這些改變可以幫助你更好地使用 API 文件。

助力生產力的提高不僅是做加法，做減法也同樣重要，我們清理了一些堆積的內容，並刪除了 SDK 裡已棄用的的 API，這將幫助我們保持更小的程式碼體積，這對新上手的開發者們尤為重要。為此，我們從 `dart:io` 庫中刪除了 [231 行已棄用的程式碼](https://dart-review.googlesource.com/c/sdk/+/236840 "Dart SDK 中棄用了 231 行程式碼")。如果你仍在使用這些已棄用的 API，你可以使用 `dart fix` 進行修復和替換。我們還在繼續努力刪除 [已棄用的 Dart CLI 工具](https://github.com/dart-lang/sdk/issues/46100 "Dart SDK 中已棄用的命令列工具工具")，本次更新刪除了 `dartdoc` 工具 (使用`dart doc` 代替) 和 `pub` 工具 (使用 `dart pub` 或 `flutter pub` 代替)。

## 擴大平台整合和支援

第二個核心主題是平台整合和支援。Dart 是一種真正的多平臺語言。雖然我們已經支援 [大量的平台](https://dart.tw.gh.miniasp.com/overview#platform "Dart 程式語言開發者文件: Dart 支援的平台")，但我們仍在不斷拓展新平台，以確保你可以與每個受支援的平台深度整合，同時也關注更新興的平台。

我們 [與 C 語言或原生程式碼互操作](https://dart.tw.gh.miniasp.com/guides/libraries/c-interop "與 C 語言或原生程式碼互操作") 的核心機制——Dart FFI，是一種將 Dart 程式碼與現有原生平台程式碼整合的流行方式。在 Flutter 上，FFI 是建構使用宿主平台原生 API (例如 Windows win32 API) 外掛的好方法。在 Dart 2.17 和 Flutter 3 中，我們向 `flutter` 工具添加了 FFI 的範本，現在你可以輕鬆地建立 FFI 外掛，這些外掛具有透過 `dart:ffi` 呼叫原生程式碼支援的 Dart API。詳細資訊請參閱開發者文件 [開發 package 和外掛](https://flutter.tw/development/packages-and-plugins/developing-packages#dart-only-platform-implementations "Flutter 開發者文件: Packages 和外掛的開發和提交") 頁面。

FFI 現在支援特定於 ABI 的型別，可以在具有特定 [ABI (應用程式二進位制介面)](https://baike.baidu.com/item/ABI/10912305 "百度百科詞條: ABI (應用程式二進位制介面)") 型別的平臺上使用 FFI。例如，現在你可以使用 `Long` (C 語言中的 `long`) 正確表示具有特定於 ABI 大小的長整數，由於 CPU 架構的區別，結果可能是 32 位或 64 位。有關支援型別的完整列表，請參閱 [AbiSpecificInteger API 頁面](https://api.dart.tw.gh.miniasp.com/stable/2.17.0/dart-ffi/AbiSpecificInteger-class.html "dart:ffi 庫 AbiSpecificInteger 類別的 API 文件頁面") 中的 "Implementers" 列表。

在使用 Dart FFI 與原生平台深度整合時，有時需要將 Dart 分配的記憶體或其他資源 (埠、檔案等) 的清理行為與原生程式碼對齊。長期以來，這個問題都十分棘手，因為 Dart 是一種會自動處理垃圾回收清理行為的語言。在 Dart 2.17 中，我們透過引入 Finalizer 的概念解決了這個問題，它包括一個 `Finalizable` 標記介面，用於「標記」不應過早終結或丟棄的物件，以及一個 `NativeFinalizer` 可以附加到 Dart 物件上，當物件即將被垃圾回收時提供回呼(Callback)執行。Finalizer 讓原生程式碼和 Dart 程式碼中同時執行清理。更多詳細資訊請參閱 [NativeFinalizer API 文件](https://api.dart.tw.gh.miniasp.com/stable/2.17.0/dart-ffi/NativeFinalizer-class.html "dart:ffi 庫 NativeFinalizer 類別的 API 文件頁面") 中的描述和範例，或 [WeakReferences](https://api.dart.tw.gh.miniasp.com/stable/2.17.0/dart-core/WeakReference-class.html "dart:core 庫 WeakReferences 類別的 API 文件頁面") 以及 [Finalizer](https://api.dart.tw.gh.miniasp.com/stable/2.17.0/dart-core/Finalizer-class.html "dart:core 庫 Finalizer 類別的 API 文件頁面") 在 Dart 程式碼中的類似支援。

將 Dart 編譯為本機程式碼的支援，也是使 Flutter 應用具有出色的啟動效能和快速渲染的核心。除此之外，你還可以使用 `dart compile` 編譯 Dart 檔案為可執行檔案。這些可執行檔案可以在任何機器上獨立執行，無需安裝 Dart SDK。Dart 2.17 中的另一個新功能是支援對可執行檔案進行簽名，產生的產物可以在經常需要簽名的 Windows 和 macOS 上進行部署。

我們還保持在新興的平臺前沿，繼續擴大我們所支援的平台集。[RISC-V](https://riscv.org/about/ "RISC-V 中國主頁") 是一個全新的指令集體系。RISC-V International 是一家全球性的非盈利組織，擁有 RISC-V 規範，使得指令集自由開放。這仍然是一個新興的平台，但我們對其潛力感到興奮，因此我們的 `2.17.0–266.1.beta` Linux 版本包含了對它的實驗性支援。我們希望能夠聽到你的反饋，你可以 [提出問題](https://github.com/dart-lang/sdk/issues "在 GitHub 上向 Dart 團隊提出問題") 或 [分享](https://groups.google.com/a/dartlang.org/g/misc "加入 Dart 郵件群組並分享你的體驗") 你的體驗！

## 開始使用 Dart 2.17！

我們希望 Dart 的 2.17 正式版能打動你並能助力你提高工作效率，也同時能夠把你的應用帶去更多的平台。即刻下載 Dart 2.17 並開始使用，也安裝使用 Flutter 3，使用內建的 Dart SDK。
