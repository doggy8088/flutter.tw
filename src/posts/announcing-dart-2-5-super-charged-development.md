---
title: Dart 2.5 正式公佈
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/05/UUKmRn.jpg)

*作者: Michael Thomsen, Dart & Flutter Product Manager, Google*

繼在中國 Google 開發者大會上 [釋出 Flutter 1.9](https://flutter.cn/posts/flutter-news-from-gdd-china-flutter1.9) 之後，今天 Dart 2.5 SDK 穩定版也正式到來，其中包括兩個意義重大的全新功能技術預覽:

* ML Complete，由機器學習 (ML) 驅動的程式碼自動完成功能
* dart:ffi 外部函式介面，用來直接從 Dart 呼叫 C 語言程式碼

另外，Dart 2.5 還增強了對 [常量表達式](https://dart.dev/guides/language/language-tour#final-and-const) 的支援。

這個版本是我們向著針對客戶端最佳化的最佳開發語言願景邁出的又一步，讓開發者可以在任何平台建立高效執行的應用。ML Complete 是我們現有生產力工具元件 ([熱重載](https://flutter.dev/docs/development/tools/hot-reload)、[可自訂靜態分析](https://dart.dev/guides/language/analysis-options) 和 [Dart DevTools](https://flutter.dev/docs/development/tools/devtools/overview) 等) 的強大補充，第二個預覽功能 dart:ffi 則使您能夠在執行 Dart 程式碼的許多作業系統上呼叫現有的原生 API，以及用 C 編寫的現有跨平臺原生程式碼庫。

我們熱切地想要建立最佳的客戶端開發語言。令人振奮的是，新的 [IEEE Spectrum 2019 最佳開發語言](https://spectrum.ieee.org/computing/software/the-top-programming-languages-2019) 評級已於近期出爐，而 Dart 已經躋身其中並位列第 16 名。在 "熱門" 條目下，Dart 則排名第 10，在只考慮移動端開發語言時則排名第 6 (位於 Java、C、C++、C# 和 Swift 之後)。

![](https://devrel.andfun.cn/devrel/posts/2021/05/jpZtih.png)

## **ML Complete: 基於機器學習的程式碼自動完成**

型別化程式語言的核心優勢之一，就是在型別中附帶的資訊使得 IDE / 編輯器能夠在鍵入程式碼時提供強大的程式碼自動完成功能，從而幫助開發者提高效率。透過程式碼自動完成，開發者只需要輸入程式碼的開頭部分即可從提供的選項中進行選擇，從而避免拼寫錯誤，也便於探索各種 API。

但隨著 API 數量的增長，探索 API 也變得愈發困難，因為自動完成功能提供的列表太長，開發者無法按照字母順序去逐一瀏覽。在過去的一年裡，我們一直在努力讓機器學習來解決這個問題。簡單地講，我們透過 [分析 GitHub 上大量開源的 Dart 程式碼](https://console.cloud.google.com/marketplace/details/github/github-repos) 來訓練一個模型，用以分析特定上下文時不同程式碼成員的出現模式。這個基於 [TensorFlow Lite](https://www.tensorflow.org/lite) 打造的模型在被訓練成型後，可以在開發者編寫程式碼時預測接下來需要用到的程式碼內容。這個新功能我們稱之為 ML Complete。以下是使用 Flutter 框架開發新的 MyHome widget 的範例:

![△ 使用 ML Complete 開發 Flutter widget 時的範例](https://devrel.andfun.cn/devrel/posts/2021/05/epDlid.gif)

> △ 使用 ML Complete 開發 Flutter widget 時的範例

讓我們來深入瞭解一下它的執行機制。假設您正在編寫一個小程式來計算從當前時間開始一天後的時間。使用 ML Complete，您將獲得下圖這樣迅捷的開發體驗。

![△ 使用 ML Complete 編寫程式碼的體驗](https://devrel.andfun.cn/devrel/posts/2021/05/tynAjy.gif)

> △ 使用 ML Complete 編寫程式碼的體驗

![△ 不使用 ML Complete 編寫同樣程式碼的體驗](https://devrel.andfun.cn/devrel/posts/2021/05/gpR4if.gif)

> △ 不使用 ML Complete 編寫同樣程式碼的體驗

首先，請注意 ML Complete 會根據開發者輸入的變數名稱 now 自動給出 DateTime.now() 的建議。當第一行輸入完成後，請注意我們在開發者輸入第二個變數名時，也給出了 tomorrow 這個變數名建議。最後，基於 now 這個變數給出了第二個自動完成建議 add(…)。而在上圖的非 ML Complete 體驗中，我們必須手動鍵入 DateTime，而且在鍵入 tomorrow 變數名時沒有自動完成提示，另外 now 的 add(…) 方法在推薦列表更下面的位置才出現。

## **如何試用 ML Complete**

ML Complete 今天推出預覽版。它內置於 Dart 分析器中，因此可用於所有支援 Dart 的編輯器，包括 Android Studio、IntelliJ 和 VS Code。有關如何啟用此預覽功能，以及如何提供反饋和報告問題，請參閱我們的 [反饋 wiki 頁](https://github.com/dart-lang/sdk/wiki/Previewing-Dart-code-completions-powered-by-machine-learning)。

由於該功能仍在預覽中，因此當前 Flutter 和 Dart 穩定版本中的 ML Complete 在效能表現和最佳化細節上會不及我們計劃推出的後續版本。因此，我們建議您試用此功能時臨時使用 [Flutter dev 渠道](https://github.com/flutter/flutter/wiki/Flutter-build-release-channels) 或 [Dart dev 渠道](https://dart.dev/tools/sdk/archive#dev-channel)。

## **dart:ffi 外部函式介面**

許多開發者要求我們為從 Dart 呼叫 C 程式碼提供更好的支援。一個非常明確的訊號，是在 Flutter 問題反饋專區裡 C 語言互操作是 [呼聲最高的功能請求](https://github.com/flutter/flutter/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)，得票數超過 600。這些功能請求背後有許多有趣的使用案例，包括呼叫低階平台 API (如 [stdlib.h](https://pubs.opengroup.org/onlinepubs/009695399/basedefs/stdlib.h.html) 或 [Win32](https://en.wikipedia.org/wiki/Windows_API))，呼叫現有的跨平臺庫以及用 C 語言編寫的實用程式 (如 TensorFlow、Realm 和 SQLite) 等。

目前，直接從 Dart 呼叫 C 的支援僅限於使用 [原生擴充](https://dart.dev/server/c-interop-native-extensions) 與 Dart VM 進行深度整合。或者，Flutter 應用可以透過 [平台通道](https://flutter.dev/docs/development/platform-integration/platform-channels) 呼叫 host，並從那裡來間接呼叫 C。這種兩層的間接呼叫表現並不理想，我們希望提供一種新的機制，能夠提供出色的效能，易於使用，並可以在許多支援 Dart 的平臺和編譯器上執行。

Dart-C 互操作支援兩種主要場景:

* 在作業系統 (OS) 上呼叫基於 C 的系統 API
* 呼叫基於 C 的程式碼庫，該程式碼庫可以基於單個作業系統，也可以是跨平臺的

## **呼叫基於 C 的作業系統 API**

我們來看看第一個互操作場景。我們將呼叫 Linux 命令 system，它可以執行任何系統命令; 傳遞給它的引數實際上是傳遞給了 shell/terminal，並在那裡執行。這個指令的 C 語言頭部如下所示:

```
// C header: int system(const char *command) in stdlib.h
```

任何互操作機制的核心挑戰都是處理兩種語言的語義差異。在 dart:ffi 這裡，Dart 程式碼需要處理好兩件事:

1. C 語言函式及其引數的型別，以及返回型別
2. 與之對應的 Dart 函式及其型別

我們透過定義兩個 typedef 來做到這一點:

```
// C header typedef:
typedef SystemC = ffi.Int32 Function(ffi.Pointer<Utf8> command);

// Dart header typedef:
typedef SystemDart = int Function(ffi.Pointer<Utf8> command);
```

下面我們需要載入程式碼庫，並查詢我們要呼叫的函式。具體做法取決於作業系統，在下面這個例子中，我們使用的是 macOS。

```
// Load `stdlib`. On MacOS this is in libSystem.dylib.
final dylib = ffi.DynamicLibrary.open('/usr/lib/libSystem.dylib');

// Look up the system function.
final systemP = dylib.lookupFunction<SystemC, SystemDart>('system');
```

您可以在 GitHub 上找到可供所有三種作業系統 ([macOS](https://github.com/dart-lang/samples/blob/master/ffi/system-command/macos.dart)、[Windows](https://github.com/dart-lang/samples/blob/master/ffi/system-command/windows.dart)、[Linux](https://github.com/dart-lang/samples/blob/master/ffi/system-command/linux.dart)) 執行的完整範例。

接下來，我們使用與特定作業系統相關的編碼對字串引數進行編碼，呼叫該函式，並再次釋放參數記憶體:

```
// Allocate a pointer to a Utf8 array containing our command.
final cmdP = Utf8.toUtf8('open http://dart.dev');

// Invoke the command.
systemP(cmdP);

// Free the pointer.
cmdP.free();
```

這段程式碼會執行系統命令，使用系統預設瀏覽器開啟 dart.dev 網頁:

![△ 透過 dart:ffi 使用系統 API 開啟預設瀏覽器](https://devrel.andfun.cn/devrel/posts/2021/05/6rNvGJ.gif)

> △ 透過 dart:ffi 使用系統 API 開啟預設瀏覽器

## **呼叫基於 C 的框架和元件**

dart:ffi 的第二個核心用途是呼叫基於 C 的框架和元件。本文前面提及過的基於 ML 的程式碼自動完成功能就是一個具體的例子！它使用 TensorFlow Lite，這是一個基於 C 的 API。使用 dart:ffi 可以讓我們在需要提供程式碼自動完成的所有作業系統上執行 TensorFlow，同時擁有原生 TensorFlow 的高效能。如果您想檢視 Dart 與 TensorFlow 整合的程式碼，請檢視 [這個 repo](https://github.com/dart-lang/tflite_native)。

我們希望呼叫 C 語言程式碼庫的能力能夠對 Flutter 應用帶來助力，比如呼叫 [Realm](https://github.com/realm/realm-core) 或 [SQLite](https://www.sqlite.org/c3ref/intro.html) 等原生代碼庫，我們認為 dart:ffi 可能會為 [Flutter 桌面應用](https://github.com/flutter/flutter/wiki/Desktop-shells) 帶來強大的外掛功能。

## **封裝 API 和程式碼產生器**

您可能已經注意到，在互操作時，描述函式/方法和查詢符號時會有一些不可避免的前置程式設計工作。您可以從 C 語言的標頭檔案產生許多這種樣板程式碼。我們目前專注於做好底層的基礎工作，如果有任何人對這種程式碼產生器器感興趣的話，我們也很樂意與之合作。

## **如何試用 dart:ffi**

dart:ffi 庫今天也已釋出預覽版。由於它仍處於預覽狀態，因此我們建議您使用 [Flutter master 渠道](https://github.com/flutter/flutter/wiki/Flutter-build-release-channels) 或 [Dart dev 渠道](https://dart.dev/tools/sdk/archive#dev-channel)，以更快地獲得我們所做的更改和改進。

請注意，API 可能會在正式版本中有非相容性變化，因為我們會新增一些細節改動，並擴大對常見模式的支援。您可以詳細瞭解我們目前為第一個版本 [規劃的內容](https://github.com/dart-lang/sdk/projects/13)。以下是您應該瞭解的一些限制:

* 該庫不支援巢狀(Nesting)結構、內聯陣列、打包資料或與平台相關的基本型別
* 指標操作的效能不足 (但可以使用 Pointer.asExternalTypedData 作為替代方案)
* 該庫不支援 finalizer (物件即將被垃圾回收時發生的回呼(Callback))

我們的 [C 語言互操作文件](https://dart.dev/guides/libraries/c-interop) 和 [dart:ffi API 文件](https://api.dart.dev/dev/2.6.0-dev.1.0/dart-ffi/dart-ffi-library.html) 收錄了相關的核心概念，並給出了您可以檢視的範例。如果您有任何疑問的話，請在 [Dart FFI 討論組](https://groups.google.com/forum/#!forum/dart-ffi) 中發帖，或直接 [向我們提交您的問題](https://github.com/dart-lang/sdk/issues/new?labels=area-vm,library-ffi)。

## **改進常量表達式**

Dart 長期以來一直支援 [建立 const 變數和值](https://dart.dev/guides/language/language-tour#final-and-const)，由於它們在編譯時為常量，因此具有非常好的效能。在以前的版本中常量表達式的侷限頗多。從 Dart 2.5 開始，我們支援更多定義常量表達式的方法，包括型別轉換以及 [Dart 2.3](https://medium.com/dartlang/announcing-dart-2-3-optimized-for-building-user-interfaces-e84919ca1dff) 中提供的新控制流和集合擴充功能:

```
// Example: these are now valid compile-time constants.
const Object i = 3;
const list = [i as int];
const set = {if (list is List<int>) ...list};
const map = {if (i is int) i: "int"};
```

## **小結**

在接下來的幾個季度，我們將迎來諸多令人振奮的進展: [擴充方法](https://github.com/dart-lang/language/issues/41) 已經進行了大量的研發工作，[非空參考](https://medium.com/dartlang/dart-nullability-syntax-decision-a-b-or-a-b-d827259e34a3) (non-nullable) 也在大力推進中，[更多語言特性](https://github.com/dart-lang/language/projects/1) 也進入了早期規劃。我們還在研究如何改善併發支援，例如在現代智慧手機上更好地使用多核處理器的能力。

預設採用非空參考是一個我們熱切期望推進的語言特性。我們為這個特性制定了一個 [非常龐大的計劃](https://github.com/dart-lang/language/blob/master/accepted/future-releases/)，並且正在進行大量工作。最近新出現的幾種語言從設計一開始就支援非空參考，而大多數既有語言則在版本更新中也添加了 (有限的) 非空支援，但僅限於額外的靜態分析。而 [我們的目標是實現完全的非空參考](https://github.com/dart-lang/language/blob/master/accepted/future-releases/)，簡而言之，這意味著我們的非空將擴充到型別系統的核心，一旦我們的型別系統知道某些東西是非空的，我們就可以完全信任這些資訊，並且我們的後端編譯器就可以全力最佳化這些程式碼。這種穩健性會在很多地方帶來優勢，比如提供一致的 "無例外報錯" 體驗，更最佳化的程式碼體積以及更高的執行時效能。

當我們改進開發語言時，我們總會意識到這些修改給我們的生態系統帶來的負擔。因此，我們也在為現有程式碼提供豐富的遷移工具，並在這方面進行了大量的投入。我們希望這些工具能夠抵消大部分遷移成本。我們還添加了一些支援逐步遷移的語言特性和工具，並將努力在 pub.dev 上遷移我們自己的程式碼和共享程式碼。

請保持對我們的關注，今年還會有更多驚喜到來！
