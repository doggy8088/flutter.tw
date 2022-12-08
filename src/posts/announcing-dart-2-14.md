---
title: Dart 2.14 版現已釋出
toc: true
---

*支援 Apple Silicon，增加了預設的 lint、更好的工具和新的語言功能提高生產力。*

本月，我們釋出了 Dart SDK 2.14 的正式版，新的版本旨在透過獨特的可移植性、生產力和穩健性組合來打造建構應用程式的最佳平台。這一次，我們對 Apple Silicon 提供了更好的支援，並提供了許多增強生產力的功能，例如用於透過程式碼樣式分析在你編寫程式碼時捕獲錯誤的標準 lint 程式碼規則、更快的釋出工具、更好的級聯程式碼格式以及一些小的語言特性更新。

![](https://files.flutter-io.cn/posts/flutter-cn/2021/announcing-dart-2-14/announcing-dart-2-14.png){:width="85%"}

自 Apple 在 2020 年末釋出新的 [Apple 晶片](https://support.apple.com/zh-cn/HT211814 "Apple 晶片") 處理器晶片以來，我們一直致力於更新 Dart SDK 以增加對新處理器上的原生執行支援。所需的更新已經在 dev 渠道中提供了一段時間，過去一個月，beta 渠道也提供了支援，從 Dart 2.14.1 開始，可以在 Dart stable 渠道中使用啦。當你 [下載](https://dart.tw.gh.miniasp.com/get-dart "下載") 一個 macOS 的 SDK 時，確保要選擇 ARM64 選項。請注意，與 Flutter SDK 中捆綁的 Dart SDK 還沒有支援這些改進。

這些支援包括了在 Apple 晶片上執行 SDK 和 Dart 虛擬機器，也支援了在 Apple 晶片上編譯執行的可執行檔案 (使用 [dart compile](https://dart.tw.gh.miniasp.com/tools/dart-compile "dart compile") 命令)。由於 Dart 命令列工具使用原生 Apple 晶片支援，因此它們的啟動速度要快得多。

開發者們通常會更偏向讓程式碼遵循某種風格。這些規則中有許多並不只是風格上的偏好 (比如眾所周知的 tab 與空格的討論)，而且涵蓋了可能導致錯誤或引入錯誤的編碼風格。例如，Dart 風格指南要求對所有控制流結構使用花括號，如 if-else 陳述式。這可以防止經典的「懸空 else」問題，即在有多個巢狀(Nesting)的 if-else 陳述式時存在歧義。另一個例子則是型別推斷，雖然在宣告具有初始值的變數時，使用型別推斷沒有問題，但在 [宣告未初始化的變數](https://dart-lang.github.io/linter/lints/prefer_typing_uninitialized_variables.html "宣告未初始化的變數") 時手動指定型別就相當重要，這樣可以確保型別安全。

保持良好程式碼風格當然也可以選擇某種人工審查的形式，也就是我們熟悉的 code review。但是，在編寫程式碼時執行靜態分析來強制要求執行規則，通常要更為有效。

在 Dart 中，這種靜態分析是高度 [可配置的](https://dart.tw.gh.miniasp.com/guides/language/analysis-options "可配置的")，我們有 [數百條樣式規則](https://dart.tw.gh.miniasp.com/tools/linter-rules "數百條樣式規則") (也稱為 lints)。有了這麼豐富的選項，可能會導致我們不知道選擇要啟用哪個規則。Dart 團隊維護了一個 [Dart 風格指南](https://dart.tw.gh.miniasp.com/guides/language/effective-dart/style "Dart 風格指南")，它描述了我們認為編寫和設計 Dart 程式碼的最佳實踐，在此之前我們還沒有提供正式的官方風格指南的 linter 規則。

許多開發者，也包括 pub.dev 網站的 [評分](https://pub.dev/help/scoring "評分") 引擎都在使用一個名為 [pedantic](https://github.com/google/pedantic "pedantic") 的 lint 規則。然而，pedantic 起源於谷歌內部的 Dart 風格指南，由於歷史原因，它與一般的 Dart 風格指南並不相同。因此，Flutter 框架從未使用過 pedantic 規則集，而是有自己的一套規範性規則。

這聽起來可能有點亂，不過確實如此。但是在這次的版本釋出中，我們很高興地宣佈，我們現在擁有了一套全新的 lint 集合來實現樣式指南，並且 Dart 和 Flutter SDK 已更新為預設情況下將這些規則集用於新專案。這些規則集包括：

- [package:lints/core.yaml](https://github.com/dart-lang/lints/blob/main/lib/core.yaml "package:lints/core.yaml"): Dart 風格指南中的主要規則，我們認為所有 Dart 程式碼都應該遵循該規則。pub.dev 評分引擎已更新，使用這些規則代替 pedantic。
- [package:lints/recommended.yaml](https://github.com/dart-lang/lints/blob/main/lib/recommended.yaml "package:lints/recommended.yaml"): 核心規則，加上額外的推薦規則。這套規則被推薦用於所有一般的 Dart 程式碼。
- [package:flutter_lints/flutter.yaml](https://github.com/flutter/packages/blob/master/packages/flutter_lints/lib/flutter.yaml "package:flutter_lints/flutter.yaml"): 核心和推薦規則，加上額外的 Flutter 專用推薦規則。這套規則被推薦用於所有 Flutter 程式碼。

如果你現有的專案中使用了 pedantic，我們強烈建議你升級到這些新規則集。從 pedantic 升級 [只需幾步](https://github.com/dart-lang/lints#migrating-from-packagepedantic "只需幾步")。

我們對 Dart 格式化器格式化 [級聯](https://dart.tw.gh.miniasp.com/guides/language/language-tour#cascade-notation "級聯") 程式碼的方式進行了一些最佳化。以前，格式化器在某些情況下會產生混亂的格式化。例如，在這個例子中，`doIt()` 是怎麼被呼叫的？

```dart
var result = errorState ? foo : bad..doIt();
```

看上去它總會被 `bad` 呼叫，但實際上級聯適用於整個 ? 表示式，所以級聯是在該表示式的結果上呼叫的，而不僅僅是在 `false` 子句上，新的格式化器會清晰地表明這一點：

```dart
var result = errorState ? foo : bad
 ..doIt();
```

其他的變化還涉及如何對有多個級聯的行進行格式化，以及級聯一般縮排多遠。我們還大大提高了包含級聯的程式碼的格式化速度；在為 [協議緩衝區](https://developers.google.cn/protocol-buffers/docs/reference/dart-generated "協議緩衝區") 產生的 Dart 程式碼中，我們看到格式化速度 <highlight>提高了 10 倍</highlight>。

有關該問題的所有詳細資訊，請參閱和追蹤這個 [Pull Request](https://github.com/dart-lang/dart_style/pull/1033 "Pull Request")。

目前，當你 [釋出](https://dart.tw.gh.miniasp.com/tools/pub/publishing "釋出") 一個 package 到 [pub.dev](https://pub.dev/ "pub.dev 社群儲存庫") 社群儲存庫時，`pub` 便會捕獲該資料夾中的所有檔案，但是會跳過隱藏的檔案 (那些以點 `.` 開頭的檔案) 和 `.gitignore` 中列出的檔案。一些開發者需要能夠控制哪些檔案在 `.gitignore` 的列表之外被忽略。例如，你可能在一個 `tool/` 資料夾裡有一些內部開發工具，你用來維護你的 package，但這些工具與使用你 package 的人沒有關係。

Dart 2.14 中更新的 `pub` 命令支援新的 `.pubignore` 檔案，你可以在其中列出不想上傳到 pub.dev 的檔案。此檔案使用與 `.gitignore` 檔案相同的格式。詳細資訊，請參閱 [package 釋出文件](https://dart.tw.gh.miniasp.com/tools/pub/publishing#what-files-are-published "package 釋出文件")。

雖然 `pub` 可能最常用於管理程式碼的依賴性，但它也有第二個重要用途：提供強大的工具支援。其中一個例子就是 Dart 測試工具，透過 `dart test` 命令使用。這個命令實際上只是 `pub run test:test` 命令的一個包裝，它執行 [package:test](https://github.com/dart-lang/test/blob/master/pkgs/test/bin/test.dart "package:test") 中的測試入口。在呼叫該入口之前，`pub` 首先將其編譯為可以更快地執行的原生代碼。

在 Dart 2.14 之前，對 `pubspec` 的任何更改 (包括與 `package:test` 無關的更改 ) 都會使此測試建構無效，並且你會看到一堆這樣的輸出，其中包含「預編譯可執行檔案」：

```console
$ dart test
Precompiling executable... (11.6s)
Precompiled test:test.
00:01 +1: All tests passed!
```

在 Dart 2.14 中，`pub` 對何時取消建構步驟變得更加智慧，因此只有當版本改變時才會進行建構。此外，我們改進了使用並行化來執行建構步驟的方式，因此該步驟本身會完成得更快。在我們測試的一些 package 上，我們看到它只用了一半的時間。

Dart 2.14 還包含了一些小的語言特性。這一次，我們把重點放在更具體的改進上，這些改進可能只是一些細小的功能，但卻能實現以前不支援的更專業的使用案例。

首先，我們添加了一個新的 [三重移位](https://github.com/dart-lang/language/issues/120 "三重移位") 運算子 (`>>>`)。這類似於現有的移位運算子 (`>>`)，但其中 `>>` 執行算術移位，`>>>` 執行邏輯或無符號移位，其中無論被移位的數字是正數還是負數，零位都會被移入最高有效位。

我們還刪除了對型別引數的舊限制，該限制不允許使用泛型函式型別作為型別引數。以下所有內容在 2.14 之前都是無效的，但現在是允許的：

```dart
late List<T Function<T>(T)> idFunctions;
var callback = [<T>(T value) => value];
late S Function<S extends T Function<T>(T)>(S) f;
```

最後，我們對註解型別做了一個小小的調整 (像 [@Deprecated](https://api.dart.tw.gh.miniasp.com/stable/2.13.4/dart-core/Deprecated-class.html "@Deprecated") 這樣註解在 Dart 程式碼中通常用來捕獲元資料)。以前註解不能傳遞型別引數，所以像 `@TypeHelper<int>(42, "The meaning")` 這樣的程式碼是不允許的。現在這個限制已經被移除。

我們對核心 Dart package 和程式碼庫進行了許多增強，包括：

- `dart:core`: 向 Object 類添加了靜態方法 `hash` 、`hashAll` 和 `hashAllUnordered`。這些可用於以一致的方式組合多個物件的雜湊碼 ([hashAll 範例](https://api.dart.tw.gh.miniasp.com/stable/2.14.0/dart-core/Object/hashAll.html "hashAll 範例"))；
- `dart:core`: 本機 DateTime 類現在可以更好地處理本地時間，而不是精確到一小時的夏令時更改——例如澳大利亞豪勳爵島，它有 30 分鐘的時差偏移；
- [package:ffi](https://pub.dev/packages/ffi "package:ffi"): 添加了對使用 [arena](https://pub.dev/documentation/ffi/latest/ffi/Arena-class.html "arena") 分配器管理記憶體的支援 ([範例](https://github.com/dart-lang/sdk/blob/master/samples/ffi/resource_management/arena_sample.dart "使用 arena 分配器管理記憶體的範例"))。Arenas 是一種 [基於區域的記憶體管理](https://en.wikipedia.org/wiki/Region-based_memory_management "基於區域的記憶體管理") 形式，一旦退出 arena/region 就會自動釋放資源；
- [package:ffigen](https://pub.dev/packages/ffigen "package:ffigen"): 現在支援從 C 型別定義產生 Dart 型別定義。

Dart 2.14 還包含一些較小的、已經 [提前宣佈過的](https://github.com/dart-lang/sdk/blob/master/docs/process/breaking-changes.md "提前宣佈過的破壞性更新") 破壞性更新 (breaking changes)。預計這些變化只會影響一些特別的使用案例，這些破壞性更新如下：

## [#46545](https://github.com/dart-lang/sdk/issues/46545 "#46545"): 取消對 ECMAScript5 的支援

[所有的現代瀏覽器](https://caniuse.com/es6 "所有的現代瀏覽器") 都已支援最新的 ECMAScript 版本，所以兩年前我們 [宣佈了](https://groups.google.com/a/dartlang.org/g/announce/c/x7eDinVT6fM/m/ZSFl2a9tEAAJ "宣佈不再支援 ES5") 一項計劃，不再支援 ECMAScript 5 (ES5)。這使我們能夠利用最新的 ECMAScript 的改進，產生更小的輸出。在 Dart 2.14 中，這項工作已經完成，Dart Web 編譯器不再支援 ES5 了，因此，較舊的瀏覽器 (例如 IE11 ) 將不再支援。

## [#46100](https://github.com/dart-lang/sdk/issues/46100 "#46100"): 棄用 stagehand、dartfmt 和 dart2native

在 2020 年 10 月的 [Dart 2.10 部落格文章中](https://medium.com/dartlang/announcing-dart-2-10-350823952bd5 "Dart 2.10 部落格文章中")，我們宣佈了將所有 Dart CLI 開發人員工具組合成一個單一的組 `dart` 命令工具 (類似於 flutter 命令工具) 的工作。作為這一演變的一部分，Dart 2.14 棄用了以前 `dartfmt` 和 `dart2native` 命令，並停止了 `stagehand`。這些工具在 [統一的 dart 命令工具中](https://dart.tw.gh.miniasp.com/tools/dart-tool "統一的 dart 命令工具中") 都有等價的替代品。

## [#45451](https://github.com/dart-lang/sdk/issues/45451 "#45451"): 棄用 VM 原生擴充

我們已經棄用了 Dart VM 的原生擴充，這是我們從 Dart 程式碼呼叫原生程式碼的舊機制。Dart [FFI](https://dart.tw.gh.miniasp.com/guides/libraries/c-interop "FFI") (外部功能介面) 是我們當前用於這個使用案例的機制，我們正在積極 [發展](https://mp.weixin.qq.com/s/pmfJ3Q8wJ_fM0VTNWeaSqg) 它以使其功能更加強大且易於使用。

我們在 3 月份的 [Dart 2.12]({{site.url}}/posts/announcing-dart-2-12) 版本中推出了健全的空安全。空安全是 Dart 最新的主要生產力特性，旨在幫助你避免空值錯誤，這是一類通常難以發現的錯誤。

自從我們上次更新以來，我們看到現有 package 和應用程式的遷移取得了巨大的進展，以實現空安全 的健全檢查優勢。對於 pub.dev 上的 package，前 250 名的 package 中 100% 都已支援了空安全，前 1000 名中有 94% 都支援。這意味著更多的開發者可以用完全 [健全的空安全](https://dart.tw.gh.miniasp.com/null-safety/unsound-null-safety#sound-and-unsound-null-safety "健全的空安全") 來執行他們的應用。分析顯示，56% 的 `flutter run` 命令以完全健全的方式執行。感謝生態系統中的所有開發者，感謝你們的遷移工作!

包含上述變化的增強型 Dart SDK 已經可以在 Dart 2.14.1 和 [Flutter 2.5]({{site.url}}/posts/whats-new-in-flutter-2-5) SDK 中使用。我們希望你會喜歡這些新的改進和功能。

## 感謝 Dart 社群

另外，我們想借此機會向 Dart 社群表示感謝。透過最近對程式語言調查的一些更新，可以看到 Dart 的勢頭很強勁。備受尊敬的 [RedMonk 排名中](https://redmonk.com/sogrady/2021/08/05/language-rankings-6-21/ "RedMonk 排名中") 提到了 "Dart 的顯著上升"，並首次將 Dart 列入前 20 名。StackOverflow 的 [2021 年開發者綜合調查](https://insights.stackoverflow.com/survey/2021#technology-most-loved-dreaded-and-wanted "2021 年開發者綜合調查") 同樣讓人欣喜。據報道，Dart 是開發人員最喜愛的第七種程式語言。我們真的很高興看到 Dart 平台有持續的增長和發展勢頭。

*感謝 flutter.cn 社群成員 (@AlexV525、@Vadaski、@MeandNi) 以及 Lynn 對本文的審校和貢獻。*
