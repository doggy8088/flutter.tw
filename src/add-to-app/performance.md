---
title: Load sequence, performance, and memory
title: 控制載入順序，最佳化效能與記憶體
description: What are the steps involved when showing a Flutter UI.
description: 介紹在顯示一個 Flutter 介面時有哪些步驟。
tags: Flutter混合工程,add2app
keywords: 效能最佳化
---

This page describes the breakdown of the steps involved
to show a Flutter UI. Knowing this, you can make better,
more informed decisions about when to pre-warm the Flutter engine,
which operations are possible at which stage,
and the latency and memory costs of those operations.

本頁面描述了展示一個 Flutter UI 的分解步驟。
知道了這一點，您可以就何時對 Flutter 引擎進行預熱，
在哪個階段可以進行哪些操作，
以及這些操作的潛在問題和記憶體成本做出更好、更明智的決策。

## Loading Flutter

## 載入 Flutter

Android and iOS apps (the two supported platforms for
integrating into existing apps), full Flutter apps,
and add-to-app patterns have a similar sequence of
conceptual loading steps when displaying the Flutter UI.

在展示 Flutter UI 時，
Android 與 iOS 應用（用於整合到現有應用的兩個受支援的平台），
純 Flutter 應用，以及 add-to-app 的模式，在概念上的載入步驟順序相似。

### Finding the Flutter resources

### 查詢 Flutter 資源

Flutter's engine runtime and your application's compiled
Dart code are both bundled as shared libraries on Android
and iOS. The first step of loading Flutter is to find those
resources in your .apk/.ipa/.app (along with other Flutter
assets such as images, fonts, and JIT code, if applicable).

Flutter 的引擎執行時和應用已編譯的 Dart 程式碼都被打包為 Android 和 iOS 上的共享庫。
載入 Flutter 的第一步是在 .apk、.ipa 或 .app 中查詢這些資源
（以及其他 Flutter 資源，例如圖像和字型，假如適用的話還有 JIT 程式碼）。

This happens when you construct a `FlutterEngine` for the
first time on both **[Android][android-engine]**
and **[iOS][ios-engine]** APIs.

當您首次在 **[Android][android-engine]** 和 **[iOS][ios-engine]** 
上呼叫 API 建構 `FlutterEngine` 時，就會發生這種情況。

### Loading the Flutter library

### 載入 Flutter 庫

After it's found, the engine's shared libraries are memory loaded
once per process.

找到後，引擎的共享庫將在每個處理序中載入一次記憶體。

On **Android**, this also happens when the
[`FlutterEngine`][android-engine] is constructed because the
JNI connectors need to reference the Flutter C++ library.
On **iOS**, this happens when the
[`FlutterEngine`][ios-engine] is first run,
such as by running [`runWithEntrypoint:`][].

在 **Android** 上，當建構 
[`FlutterEngine`][android-engine] 
時也會發生這種情況，因為 JNI 聯結器需要參考 Flutter C++ 庫。
在 **iOS** 上，這是在首次執行 
[`FlutterEngine`][ios-engine] 時發生的，
例如執行 [`runWithEntrypoint:`][]。

### Starting the Dart VM

### 啟動 Dart VM

The Dart runtime is responsible for managing Dart memory and
concurrency for your Dart code. In JIT mode,
it's additionally responsible for compiling
the Dart source code into machine code during runtime.

Dart 執行時負責管理 Dart 程式碼的 Dart 記憶體與非同步。
在 JIT 模式下，它還負責在執行時將 Dart 原始碼編譯為機器碼。

A single Dart runtime exists per application session on
Android and iOS.

在 Android 和 iOS 上，每個應用程式會話都存在一個 Dart 執行時。

A one-time Dart VM start is done when constructing the
[`FlutterEngine`][android-engine] for the first time on
**Android** and when [running a Dart entrypoint][ios-engine]
for the first time on **iOS**.

在 **Android** 上首次建構 
[`FlutterEngine`][android-engine]，
以及在 **iOS** 上首次
[執行 Dart 入口][ios-engine]
時，將完成一次 Dart VM 啟動。

At this point, your Dart code's [snapshot][]
is also loaded into memory from your application's files.

此時，您的 Dart 程式碼的 [snapshot][] 
也將從應用程式的檔案載入到記憶體中。

This is a generic process that also occurs if you used the
[Dart SDK][] directly, without the Flutter engine.

即使您直接使用 [Dart SDK][]而沒 Flutter 引擎，也會這樣執行，這是一個通用的過程。

The Dart VM never shuts down after it's started.

Dart VM 啟動後永遠不會關閉。

### Creating and running a Dart Isolate

### 建立並執行一個 Dart Isolate

After the Dart runtime is initialized,
the Flutter engine's usage of the Dart
runtime is the next step.

在初始化 Dart 執行時之後，下一步就是 Flutter 引擎對 Dart 執行時的使用。

This is done by starting a [Dart `Isolate`][] in the Dart runtime.
The isolate is Dart's container for memory and threads.
A number of [auxiliary threads][] on the host platform are
also created at this point to support the isolate, such
as a thread for offloading GPU handling and another for image decoding.

這是透過在 Dart 執行時中啟動 
[Dart `Isolate`][] 來完成的。
isolate 是 Dart 的記憶體和執行緒容器。 
此時在宿主平臺上還建立了許多
[輔助執行緒][auxiliary threads] 來支援 isolate，
例如用於解除 GPU 處理的執行緒和用於圖像解碼的執行緒。

One isolate exists per `FlutterEngine` instance, and multiple isolates
can be hosted by the same Dart VM.

每個 `FlutterEngine` 例項都存在一個 isolate，並且同一個 Dart VM 可以承載多個 isolate。

On **Android**, this happens when you call
[`DartExecutor.executeDartEntrypoint()`][]
on a `FlutterEngine` instance.

在 **Android** 上，當您在 `FlutterEngine` 例項上呼叫 
[`DartExecutor.executeDartEntrypoint()`][] 時，就會發生這種情況。

On **iOS**, this happens when you call [`runWithEntrypoint:`][]
on a `FlutterEngine`.

在 **iOS** 上，當您對 `FlutterEngine` 例項呼叫 [`runWithEntrypoint:`][]時會發生這種情況。

At this point, your Dart code's selected entrypoint
(the `main()` function of your Dart library's `main.dart` file,
by default) is executed. If you called the
Flutter function [`runApp()`][] in your `main()` function,
then your Flutter app or your library's widget tree is also created
and built. If you need to prevent certain functionalities from executing
in your Flutter code, then the `AppLifecycleState.detached`
enum value indicates that the `FlutterEngine` isn't attached
to any UI components such as a `FlutterViewController`
on iOS or a `FlutterActivity` on Android.

此時，Dart 程式碼會執行預設的入口點方法 (預設是 `main.dart` 檔案的 `main()` 方法) ，
如果你在 `main()` 方法中呼叫 Flutter 的 [`runApp()`][] 方法，則你的 Flutter 應用
或庫的 widget 樹將會建立並建構。如果你需要阻止某些功能在 Flutter 程式碼中執行，
則需要使用列舉值 `AppLifecycleState.detached` 表示其不繫結在任何 UI 元件上。

### Attaching a UI to the Flutter engine

### 將 UI 掛載到 Flutter 引擎

A standard, full Flutter app moves to reach this state as
soon as the app is launched.

啟動後不久，一個標準的完整的 Flutter 應用程式便會達到此狀態。

In an add-to-app scenario,
this happens when you attach a `FlutterEngine`
to a UI component such as by calling [`startActivity()`][]
with an [`Intent`][] built using [`FlutterActivity.withCachedEngine()`][]
on **Android**. Or, by presenting a [`FlutterViewController`][]
initialized by using [`initWithEngine: nibName: bundle:`][]
on **iOS**.

在 add-to-app 的場景中，
例如透過在 **Android** 上使用 [`FlutterActivity.withCachedEngine()`][] 
方法建構的 [`Intent`][]，
呼叫 [`startActivity()`][] 時，
或者，在 **iOS** 上呼叫 [`initWithEngine: nibName: bundle:`][]，
展示例項化的 [`FlutterViewController`][]，
都會將 `FlutterEngine` 掛載到 UI 元件。

This is also the case if a Flutter UI component was launched without
pre-warming a `FlutterEngine` such as with
[`FlutterActivity.createDefaultIntent()`][] on **Android**,
or with [`FlutterViewController initWithProject: nibName: bundle:`][]
on **iOS**. An implicit `FlutterEngine` is created in these cases.

如果在沒有啟動 Flutter UI 元件的情況下也是如此,
例如在 **Android** 上使用 [`FlutterActivity.createDefaultIntent()`][] 
或在 **iOS** 上使用 [`FlutterViewController initWithProject: nibName: bundle:`][] 
預熱一個 `FlutterEngine`。
在這些情況下，將建立一個隱含的 `FlutterEngine`。

Behind the scene, both platform's UI components provide the
`FlutterEngine` with a rendering surface such as a
[`Surface`][] on **Android** or a [CAEAGLLayer][] or [CAMetalLayer][]
on **iOS**.

在後台，這兩個平台的UI元件都為 `FlutterEngine` 提供了渲染層，
例如 **Android** 上的 [`Surface`][] 或 **iOS** 上的 [CAEAGLLayer][]
或 [CAMetalLayer][]。

At this point, the [`Layer`][] tree generated by your Flutter
program, per frame, is converted into
OpenGL (or Vulkan or Metal) GPU instructions.

此時，您的 Flutter 程式產生的 [`Layer`][]
樹將轉換為 OpenGL（或 Vulkan 或 Metal）GPU 指令。

## Memory and latency

## 記憶體和延遲

Showing a Flutter UI has a non-trivial latency cost.
This cost can be lessened by starting the Flutter engine
ahead of time.

顯示 Flutter UI 會耗費不少時間。提前啟動 Flutter 引擎可以降低時間開銷。

The most relevant choice for add-to-app scenarios is for you
to decide when to pre-load a `FlutterEngine`
(that is, to load the Flutter library, start the Dart VM,
and run entrypoint in an isolate), and what the memory and latency
cost is of that pre-warm. You also need to know how the pre-warm
affects the memory and latency cost of rendering a first Flutter
frame when the UI component is subsequently attached
to that `FlutterEngine`.

對於 add-to-app 的場景，預熱相應的選擇是，
讓您決定什麼時候預載入 `FlutterEngine`
（即載入 Flutter 庫，啟動 Dart VM 並在 isolate 中執行入口點），
以及確定記憶體的佔用與時間開銷。
您還需要知道，在將 UI 元件隨後掛載到該 `FlutterEngine` 時，
預熱會如何影響 Flutter 渲染首幀的記憶體和時間開銷。

As of Flutter v1.10.3, and testing on a low-end 2015 class device
in release-AOT mode, pre-warming the `FlutterEngine` costs:

用 Flutter v1.10.3 版本，在 2015 年的低端裝置上測試，release AOT 模式下，預熱 `FlutterEngine` 的開銷：

* 42 MB and 1530 ms to prewarm on **Android**.
  330 ms of it is a blocking call on the main thread.

  **Android** 平台需要 42 MB 記憶體，耗費 1530 毫秒。主執行緒阻塞了 330 毫秒；
  
* 22 MB and 860 ms to prewarm on **iOS**.
  260 ms of it is a blocking call on the main thread.


  **iOS** 平台需要 22 MB 記憶體，耗費 860 毫秒。主執行緒阻塞了 260 毫秒。

A Flutter UI can be attached during the pre-warm.
The remaining time is joined to the time-to-first-frame latency.

Flutter 使用者介面可以在預熱期間被載入。所需時間與渲染出首幀的時間有關。

Memory-wise, a cost sample (variable,
depending on the use case) could be:

在記憶體方面的開銷（具體根據使用情況而定）可能是：

* ~4 MB OS's memory usage for creating pthreads.
  
  約 4 MB 系統記憶體用於建立 pthread；
  
* ~10 MB GPU driver memory.
  
  約 10 MB 是 GPU 驅動記憶體；
  
* ~1 MB for Dart runtime-managed memory.
  
  約 1 MB 是用於 Dart 執行時管理的記憶體；
  
* ~5 MB for Dart-loaded font maps.
  
  約 5 MB 用於 Dart 載入的字型對映。

Latency-wise,
a cost sample (variable, depending on the use case) could be:

在時間方面的開銷（具體根據使用情況而定）可能是：

* ~20 ms to collect the Flutter assets from the application package.
  
  約 20 毫秒用於從應用套件中收集 Flutter 資源；

* ~15 ms to dlopen the Flutter engine library.
  
  約 15 毫秒用於 dlopen 載入 Flutter 引擎的函式庫；
  
* ~200 ms to create the Dart VM and load the AOT snapshot.
  
  約 200 毫秒用於建立 Dart VM 並載入 AOT snapshot；
  
* ~200 ms to load Flutter-dependent fonts and assets.
  
  約 200 毫秒用於載入 Flutter 依賴的字型和資源；
  
* ~400 ms to run the entrypoint, create the first widget tree,
  and compile the needed GPU shader programs.
  
  約 400 毫秒來執行應用入口，建立第一個 widget 樹，並編譯所需的 GPU 著色器程式。

The `FlutterEngine` should be pre-warmed late enough to delay the
memory consumption needed but early enough to avoid combining the
Flutter engine start-up time with the first frame latency of
showing Flutter.

應該對 `FlutterEngine` 進行預熱，不應過於提早，以延遲記憶體佔用，
但又要避免 Flutter 引擎初始化的時機與顯示 Flutter 的首幀的時機趕在一起。

The exact timing depends on the app's structure and heuristics.
An example would be to load the Flutter engine in the screen
before the screen is drawn by Flutter.

確切的時間取決於應用的結構與不斷試探的結果。
一個範例是在 Flutter 繪製螢幕之前將 Flutter 引擎載入到螢幕中。

Given an engine pre-warm, the first frame cost on UI attach is:

引擎預熱後，載入 UI 首幀的成本為：

* 320 ms on **Android** and an additional 12 MB
  (highly dependent on the screen's physical pixel size).
  
  在 **Android** 上為 320 毫秒，另外需要 12 MB 的記憶體（在很大程度上取決於螢幕的物理畫素大小）；
  
* 200 ms on **iOS** and an additional 16 MB
  (highly dependent on the screen's physical pixel size).

  在 **iOS** 上為 200 毫秒，另外需要 16 MB 的記憶體（在很大程度上取決於螢幕的物理畫素大小）。

Memory-wise, the cost is primarily the graphical memory buffer used for
rendering and is dependent on the screen size.

在記憶體方面，開銷主要用於渲染的圖形記憶體緩衝區，並且取決於螢幕大小。

Latency-wise, the cost is primarily waiting for the OS callback to provide
Flutter with a rendering surface and compiling the remaining shader programs
that are not pre-emptively predictable. This is a one-time cost.

在時間方面，開銷主要是在等待系統回呼(Callback)，為 Flutter 提供渲染層，
並編譯其餘無法預先預測的著色器程式。這是一次性的開銷。

When the Flutter UI component is released, the UI-related memory is freed.
This doesn't affect the Flutter state, which lives in the `FlutterEngine`
(unless the `FlutterEngine` is also released).

釋放 Flutter UI 元件後，將釋放與 UI 相關的記憶體。
這不會影響 Flutter 狀態（除非也釋放了 `FlutterEngine`），
狀態位於 `FlutterEngine` 中。

For performance details on creating more than one `FlutterEngine`,
see [multiple Flutters][].

關於建立多個 `FlutterEngine` 對效能影響的詳細情況，請參考文件：
[多個 Flutter 例項][multiple Flutters]。 

[android-engine]: {{site.api}}/javadoc/io/flutter/embedding/engine/FlutterEngine.html
[auxiliary threads]: {{site.repo.flutter}}/wiki/The-Engine-architecture#threading
[CAEAGLLayer]: {{site.apple-dev}}/documentation/quartzcore/caeagllayer
[CAMetalLayer]: {{site.apple-dev}}/documentation/quartzcore/cametallayer
[Dart `Isolate`]: {{site.dart.api}}/stable/dart-isolate/Isolate-class.html
[Dart SDK]: {{site.dart-site}}/tools/sdk
[`DartExecutor.executeDartEntrypoint()`]: {{site.api}}/javadoc/io/flutter/embedding/engine/dart/DartExecutor.html#executeDartEntrypoint-io.flutter.embedding.engine.dart.DartExecutor.DartEntrypoint-
[`FlutterActivity.createDefaultIntent()`]: {{site.api}}/javadoc/io/flutter/embedding/android/FlutterActivity.html#createDefaultIntent-android.content.Context-
[`FlutterActivity.withCachedEngine()`]: {{site.api}}/javadoc/io/flutter/embedding/android/FlutterActivity.html#withCachedEngine-java.lang.String-
[`FlutterViewController`]: {{site.api}}/objcdoc/Classes/FlutterViewController.html
[`FlutterViewController initWithProject: nibName: bundle:`]: {{site.api}}/objcdoc/Classes/FlutterViewController.html#/c:objc(cs)FlutterViewController(im)initWithProject:nibName:bundle:
[`initWithEngine: nibName: bundle:`]: {{site.api}}/objcdoc/Classes/FlutterViewController.html#/c:objc(cs)FlutterViewController(im)initWithEngine:nibName:bundle:
[`Intent`]: {{site.android-dev}}/reference/android/content/Intent.html
[ios-engine]: {{site.api}}/objcdoc/Classes/FlutterEngine.html
[`Layer`]: {{site.api}}/flutter/rendering/Layer-class.html
[multiple Flutters]: {{site.url}}/add-to-app/multiple-flutters
[`runApp()`]: {{site.api}}/flutter/widgets/runApp.html
[`runWithEntrypoint:`]: {{site.api}}/objcdoc/Classes/FlutterEngine.html#/c:objc(cs)FlutterEngine(im)runWithEntrypoint:
[snapshot]: {{site.github}}/dart-lang/sdk/wiki/Snapshots
[`startActivity()`]: {{site.android-dev}}/reference/android/content/Context.html#startActivity(android.content.Intent
[`Surface`]: {{site.android-dev}}/reference/android/view/Surface
