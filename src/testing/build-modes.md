---
title: Flutter's build modes
title: Flutter 的建構模式選擇
description: Describes Flutter's build modes and when you should use debug, release, or profile mode.
description: 在使用 Flutter 建構的時候，你應該選擇哪種模式呢？Debug、Release、或者是 Profile 模式。
tags: Flutter測試
keywords: Flutter的建構模式,Flutter debug模式,Flutter release模式,Flutter profile模式
---

The Flutter tooling supports three modes when compiling your app,
and a headless mode for testing.
This doc explains the three modes and tells you when to use which.
For more information on headless testing, see
[Unit testing.]({{site.url}}/docs/testing#unit-tests)

Flutter 支援三種模式編譯 app，也支援使用 headless 模式來測試。
這篇文件解釋了這三種模式，並且告訴你什麼時候應該使用哪種模式。
關於 headless 測試的更多資訊，可以檢視 [單元測試]({{site.url}}/docs/testing#unit-tests)。

You choose a compilation mode depending on where you are in
the development cycle. Are you debugging your code? Do you
need profiling information? Are you ready to deploy your app?

選擇哪種編譯模式取決於你處於哪個開發週期中。
是除錯程式碼階段，還是需要效能最佳化分析，抑或是準備部署你的應用了呢？

A quick summary for when to use which mode is as follows:

快速簡要介紹下列三種建構模式：

* Use [debug](#debug) mode during development,
  when you want to use [hot reload][].

  開發過程中，需要使用 [熱重載][hot reload] 功能，
  請選擇 [debug](#debug) 建構模式；
  
* Use [profile](#profile) mode when you want to analyze
  performance.

  當你需要分析效能的時候，
  選擇使用 [profile](#profile) 建構模式；
  
* Use [release](#release) mode when you are ready to release
  your app.

  釋出應用的時候，需要選擇使用 [release](#release) 建構模式。

The rest of the page goes into more detail about these modes.
For information on headless testing, see the [Flutter wiki][].

下文詳細解釋了每種模式以及何時使用它，獲得更多資訊，或者瞭解無頭模式的測試，
請參考 [Flutter wiki][] 文件。

## Debug

## 除錯模式

In _debug mode_, the app is set up for debugging on the physical
device, emulator, or simulator.

在 **Debug 模式**下，app 可以被安裝在物理裝置、模擬器或者模擬器上進行除錯。

Debug mode for mobile apps mean that:

Debug 模式意味著：

* [Assertions][] are enabled.
   
  [斷點]({{site.dart-site}}/guides/language/language-tour#assert) 是開啟的。

* Service extensions are enabled.

  服務擴充是開啟的。

* Compilation is optimized for fast development and run cycles
  (but not for execution speed, binary size, or deployment).

  針對快速開發和執行週期進行了編譯最佳化（但不是針對執行速度、二進位制檔案大小或者部署）。
  
* Debugging is enabled, and tools supporting source level debugging
  (such as [DevTools][]) can connect to the process.

  除錯開啟，類似 [開發者工具][DevTools] 等除錯工具可以連線到處理序裡。

Debug mode for a web app means that:

在 Web 平臺下的除錯模式意味著：

* The build is _not_ minified and tree shaking has _not_ been 
  performed.

  本次建構 **沒有** 最小化資源並且整個建構 **沒有** 最佳化效能。
  
* The app is compiled with the [dartdevc][] compiler for
  easier debugging.

  為了簡化除錯，這個 Web 應用使用了 [dartdevc][] 編譯器。

By default, `flutter run` compiles to debug mode.
Your IDE supports this mode. Android Studio,
for example, provides a **Run > Debug...** menu option,
as well as a green bug icon overlayed with a small triangle
on the project page.

預設情況下，執行 `flutter run` 會使用 Debug 模式。
你的 IDE 也支援這些模式。例如，Android Studio 提供了 **Run > Debug...** 選單選項，
而且在專案面板中還有一個三角形的綠色執行按鈕圖示
（選單選項中會顯示相應圖示的圖片）。

{{site.alert.note}}

  * Hot reload works _only_ in debug mode.

    熱重載功能**僅能**在除錯模式下執行；
  
  * The emulator and simulator execute _only_ in debug mode.

    模擬器和模擬器**僅能**在除錯模式下執行；

  * Application performance can be janky in debug mode.
    Measure performance in [profile](#profile)
    mode on an actual device.

    在除錯模型下，應用的效能可能會掉幀或者卡頓，
    [profile](#profile) 模式下會更接近真機效能。

{{site.alert.end}}

## Release

## Release 模式

Use _release mode_ for deploying the app, when you want maximum
optimization and minimal footprint size. For mobile, release mode
(which is not supported on the simulator or emulator), means that:

當你想要最大的最佳化以及最小的佔用空間時，
就使用 **Release 模式**來部署 app 吧。
release 模式是不支援模擬器或者模擬器的，
使用 **Release 模式**意味著：

* Assertions are disabled.

  斷點是不可用的。

* Debugging information is stripped out.

  除錯資訊是不可見的。

* Debugging is disabled.

  除錯是禁用的。

* Compilation is optimized for fast startup, fast execution,
  and small package sizes.
  
  編譯針對快速啟動、快速執行和小的 package 的大小進行了最佳化。

* Service extensions are disabled.

  服務擴充是禁用的。
  
Release mode for a web app means that:

在 Web 平台的 Release 模式意味著：

* The build is minified and tree shaking has been performed.

  這次建構資源已經被壓縮，並且效能得以最佳化。
  
* The app is compiled with the [dart2js][] compiler for
  best performance.

  這個 Web 應用透過 [dart2js][] 編譯器建構，以確保更優秀的效能。

The command `flutter run --release` compiles to release mode.
Your IDE supports this mode. Android Studio, for example,
provides a **Run > Run...** menu option, as well as a triangular
green run button icon on the project page.
(The menu item shows a pic of the corresponding icon.)

`flutter run --release` 命令會使用 Release 模式來進行編譯。
你的 IDE 同樣也支援這個模式。
例如，Android Studio 提供了 **Run > Run...** 選單選項，
而且在專案面板中還有一個被小三角覆蓋的綠色蟲子圖示。
（選單選項中會顯示相應圖示的圖片）

You can compile to release mode for a specific target
with `flutter build <target>`. For a list of supported targets,
use `flutter help build`.

你可以使用 `flutter build <target>` 針對特定目標編譯 release 模式。請使用 `flutter help build` 檢視支援的目標列表。

You can also compile to release mode with `flutter build --release`.

你也可以透過 `flutter build --release` 命令來使用 release 模式。

For more information, see the docs on releasing
[iOS][] and [Android][] apps.

你也可以執行 `flutter build` 命令使用 Release 模式來編譯。
更多詳細資訊，可以參閱釋出 [iOS][] 和 [Android][] app 的文件。

## Profile

## Profile 模式

In _profile mode_, some debugging ability is maintained&mdash;enough
to profile your app's performance. Profile mode is disabled on
the emulator and simulator, because their behavior is not representative
of real performance. On mobile, profile mode is similar to release mode,
with the following differences:

在 **profile** 模式下，一些除錯能力是被保留的&mdash;足夠分析你的 app 效能。
在模擬器和模擬器上，Profile 模式是不可用的，
因為他們的行為不能代表真實的效能。
profile 模式和 release 類似，但有以下不同：

* Some service extensions, such as the one that enables the performance
  overlay, are enabled.
  
  一些服務擴充是啟用的。例如，支援 performance overlay。

* Tracing is enabled, and tools supporting source-level debugging
  (such as [DevTools][]) can connect to the process.

  Tracing 是啟用的，一些除錯工具，比如 [開發者工具][DevTools] 可以連線到處理序裡。
  
Profile mode for a web app means that:

在 Web 平台的 Profile 模式意味著：

* The build is _not_ minified but tree shaking has been performed.
 
  資原始檔沒有被壓縮，但是整體效能已經最佳化 

* The app is compiled with the [dart2js][] compiler.
 
  這個 Web 應用透過 [dart2js][] 編譯器建構 

Your IDE supports this mode. Android Studio, for example,
provides a **Run > Profile...** menu option.
The command `flutter run --profile` compiles to profile mode.

`flutter run --profile` 命令是使用 Profile 模式來編譯的。
你的 IDE 也是支援這個模式的。例如，Android Studio 提供了 **Run > Profile...** 選單選項。

{{site.alert.note}}

  Use the [DevTools][] suite to profile your app's performance.
  
  可以使用 [開發者工具][DevTools] 來測試應用效能。
{{site.alert.end}}

For more information on the build modes, see
[Flutter's build modes][].

關於這些模式的更多資訊，可以檢視 [Flutter wiki][] 中的 
[Flutter's build modes][] 文件。

[Android]: {{site.url}}/deployment/android
[Assertions]: {{site.dart-site}}/guides/language/language-tour#assert
[dart2js]: {{site.dart-site}}/tools/dart2js
[dartdevc]: {{site.dart-site}}/tools/dartdevc
[DevTools]: {{site.url}}/development/tools/devtools
[Flutter wiki]: {{site.repo.flutter}}/wiki/Flutter's-modes
[Flutter's build modes]: {{site.repo.flutter}}/wiki/Flutter%27s-modes
[hot reload]: {{site.url}}/development/tools/hot-reload
[iOS]: {{site.url}}/deployment/ios
