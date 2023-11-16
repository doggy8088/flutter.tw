---
title: FAQ
title: 常見問題與解答
description: Frequently asked questions and answers about Flutter.
description: 與 Flutter 相關的常見問題與解答
tags: Flutter參考資料
keywords: Flutter常見問題和答案,Flutter的優勢
---

## Introduction

## 簡介

This page collects some common questions asked about
Flutter. You might also check out the following
specialized FAQs:

本頁面收集了關於 Flutter 一些大家常見問題的解答。
你可能還會想要檢視下面一些特別的答疑：

* [Web FAQ][]
* [Performance FAQ][]

[Web FAQ]: {{site.url}}/platform-integration/web/faq
[Performance FAQ]: {{site.url}}/perf/faq

### What is Flutter?

### 什麼是 Flutter？

Flutter is Google's portable UI toolkit for crafting beautiful,
natively compiled applications for mobile, web,
and desktop from a single codebase.
Flutter works with existing code,
is used by developers and organizations around
the world, and is free and open source.

Flutter 是 Google 的行動式 UI 工具套件，
幫助你在移動、web、桌面端創造高品質的絕妙原生體驗的應用。
Flutter 可以和世界上的開發人員和開發組織廣泛使用的那些現存程式碼一起使用，
並且是開源的、免費的。

### Who is Flutter for?

### 哪些人會用到 Flutter？

For users, Flutter makes beautiful apps come to life.

對於使用者來說，Flutter 將美妙的應用帶到了生活中。

For developers, Flutter lowers the bar to entry for building apps.
It speeds app development and reduces the cost and complexity
of app production across platforms.

對於開發者來說，Flutter 降低了應用開發的入門門檻。
它加速了應用開發的過程，減少了跨平臺開發的成本以及複雜度。

For designers, Flutter provides a canvas for
high-end user experiences. Fast Company described
Flutter as [one of the top design ideas of the decade][] for
its ability to turn concepts into production code
without the compromises imposed by typical frameworks.
It also acts as a productive prototyping tool
with drag-and-drop tools like [FlutterFlow][]
and web-based IDEs like [Zapp!][].

對於設計師來說，Flutter 提供了一個能夠實現高保真度使用者體驗的畫布。
Fast 公司評價 Flutter 是
[一個設計靈感的源泉][one of the top design ideas of the decade]，
提供了將概念轉換為生產程式碼的能力，卻沒有典型的框架強加的妥協。
Flutter 同時也是一個能提高生產力的原型工具，
例如可拖入檔案載入的 [FlutterFlow][] 和基於 Web 的 IDE [Zapp!][]。

For engineering managers and businesses,
Flutter allows the unification of app
developers into a single _mobile, web,
and desktop app team_, building branded
apps for multiple platforms out of a single codebase.
Flutter speeds feature development and synchronizes
release schedules across the entire customer base.

對於工程主管以及僱主來說，Flutter 可以將不同平台的應用開發者，
統一為一個 **移動端、前端和桌面端應用程式** 的團隊，共同建立品牌，
並在單個程式碼庫中打造的多個平台的應用程式。
Flutter 加速了跨平臺下開發以及同步釋出處理序的開發進度。

[FlutterFlow]: https://flutterflow.io/
[Zapp!]: https://zapp.run/
[one of the top design ideas of the decade]: https://www.fastcompany.com/90442092/the-14-most-important-design-ideas-of-the-decade-according-to-the-experts

### How much development experience do I need to use Flutter?

### 我需要擁有怎樣的開發經驗才能使用 Flutter？

Flutter is approachable to programmers familiar with
object-oriented concepts (classes, methods, variables,
etc) and imperative programming concepts (loops,
conditionals, etc).

如果您熟悉面向物件概念 (類、方法、變數等) 和指令式程式設計概念 (迴圈、條件等) ，
您會發現 Flutter 很容易上手。

We have seen people with very little programming
experience learn and use Flutter for prototyping
and app development.

就我們親歷過的例子來說，
程式設計經驗並不豐富的人們一樣可以學習並使用 Flutter 進行原型設計和應用開發。

### What kinds of apps can I build with Flutter?

### 我可以用 Flutter 建構怎樣的應用？

Flutter is designed to support mobile apps that run
on both Android and iOS, as well as interactive apps
that you want to run on your web pages or on the desktop.

Flutter 設計為了讓移動應用能夠執行在 Android 與 iOS，
以及在 web 和桌面端執行可互動式的應用。

Apps that need to deliver highly branded designs
are particularly well suited for Flutter.
However, you can also create pixel-perfect experiences
that match the Android and iOS design languages with Flutter.

如果您的應用強烈需要表達出品牌個性，Flutter 會非常適合。
不過，即便您想要打造的應用看起來像是股票平台那樣複雜，
也可以使用 Flutter 來建構。

Flutter's [package ecosystem][] supports a wide
variety of hardware (such as camera, GPS, network,
and storage) and services (such as payments, cloud
storage, authentication, and [ads][]).

Flutter 的 [軟體包生態][package ecosystem] 支援絕大多數硬體
（包括攝影頭、GPS、網路以及儲存）以及服務
（例如支付、雲儲存、驗證以及 [廣告][ads]）。

[ads]: {{site.main-url}}/monetization
[package ecosystem]: {{site.pub}}/flutter

### Who makes Flutter?

### 誰創造了 Flutter？

Flutter is an open source project,
with contributions from Google and other
companies and individuals.

Flutter 是一個開源專案，由 Google 和開發社群共同創造。

### Who uses Flutter?

### 誰在使用 Flutter？

Developers inside and outside of Google use
Flutter to build beautiful natively-compiled
apps for iOS and Android. To learn about some
of these apps, visit the [showcase][].

Google 內部和外部的開發者使用 Flutter 為 Android 和 iOS 建構精美的原生應用。
您可以存取 [案例頁面][showcase] 來了解一些知名的開發者 / 組織。

[showcase]: {{site.main-url}}/showcase

### What makes Flutter unique?

### Flutter 有哪些獨到之處？

Flutter is different than most other options
for building mobile apps because it doesn't rely
on web browser technology nor the set of widgets
that ship with each device. Instead, Flutter uses
its own high-performance rendering engine to draw widgets.

Flutter 與大多數用來建構移動應用的工具不同，因為它既不使用 WebView，
也不使用裝置附帶的 OEM Widget，而是使用自己的高效能渲染引擎來繪製 Widget。

In addition, Flutter is different because it only
has a thin layer of C/C++ code. Flutter implements
most of its system (compositing, gestures, animation,
framework, widgets, etc) in [Dart][] (a modern,
concise, object-oriented language) that developers
can easily approach read, change, replace, or remove.
This gives developers tremendous control over the system,
as well as significantly lowers the bar to approachability
for the majority of the system.

Flutter 與其它工具的不同之處還在於，它只有一層簡潔的 C/C++ 程式碼，
在這之上，Flutter 使用 [Dart][] (一種現代化的、簡潔的物件導向語言)
實現其大部分系統功能 (佈局、手勢、動畫、框架、Widget 等)，
這種語言使得開發者可以輕鬆地進行閱讀、更改、替換或刪除。
這些特性都為開發者提供了巨大的系統控制權限，同時顯著降低了存取大多數系統功能的門檻。

[Dart]: {{site.dart-site}}/

### Should I build my next production app with Flutter?

### 我需要使用 Flutter 來建構我的下一個應用嗎？

[Flutter 1][] was launched on Dec 4th, 2018,
[Flutter 2][] on March 3rd, 2021, and
[Flutter 3][] on May 10th, 2023.
As of May 2023, over _one million_ apps have shipped using
Flutter to many hundreds of millions of devices.
Check out some sample apps in the [showcase][].

[Flutter 1][] 於 2018 年 12 月推出，
[Flutter 2][] 於 2021 年 3 月 3 日釋出，
[Flutter 3][] 於 2023 年 5 月 10 日釋出。
至今為止，成千上萬使用了 Flutter 的應用已經被安裝到了數億台裝置中。
請透過成功 [案例頁面][showcase] 瞭解知名開發者們的成果。

Flutter ships updates on a roughly-quarterly
cadence that improve stability and performance
and address commonly-requested user features.

Flutter 進行了高品質的持續交付更新，優化了穩定性、效能以及一些常見的使用者需求。

[Flutter 1]: {{site.google-blog}}/2018/12/flutter-10-googles-portable-ui-toolkit.html
[Flutter 2]: {{site.google-blog}}/2021/03/announcing-flutter-2.html
[Flutter 3]: {{site.google-blog}}/flutter/introducing-flutter-3-5eb69151622f

## What does Flutter provide?

## Flutter 能夠為我們提供什麼？

### What is inside the Flutter SDK?

### Flutter SDK 裡有什麼？

Flutter includes:

Flutter 包括了：

* Heavily optimized, mobile-first 2D rendering engine
  with excellent support for text

  針對移動應用深度最佳化的 2D 渲染引擎，具備出色的文字支援功能

* Modern react-style framework

  現代響應式風格框架

* Rich set of widgets implementing Material Design and iOS-style

  Material Design 風格及 iOS 風格豐富的 widget 元件

* APIs for unit and integration tests

  用於單元測試和整合測試的 API

* Interop and plugin APIs to connect to the system and 3rd-party SDKs

  原生平台互動性和外掛 API 可以連線系統及第三方 SDK

* Headless test runner for running tests on Windows, Linux, and Mac

  Headless 測試執行器，用於在 Windows、Linux 和 Mac 上執行測試

* [Flutter DevTools][] (also called Dart DevTools)
  for testing, debugging, and profiling your app

  用於測試、除錯和分析你的應用程式的 [Flutter DevTools][]  

* Command-line tools for creating, building, testing, and
  compiling your apps

  命令列工具，用於建立、開發、測試和編譯你的應用程式

[Dart DevTools]: {{site.url}}/tools/devtools

### Does Flutter work with any editors or IDEs?

### 用 Flutter 開發時可以使用哪些編輯器或 IDE？

We provide plugins for [VS Code][],
[Android Studio][], and [IntelliJ IDEA][].
See [editor configuration][] for setup details,
and [VS Code][] and [Android Studio/IntelliJ][]
for tips on how to use the plugins.

可以透過外掛的方式使用
[Android Studio][]、[IntelliJ IDEA][] 和 [VS Code][]
開發 Flutter 應用。請參閱 [邊界配置][editor configuration] 以瞭解如何初始化，
以及 [Android Studio/IntelliJ][] 和 [VS Code][] 如何使用 plugin 的小提示。

Alternatively, you can use the `flutter` command
from a terminal, along with one
of the many editors that support [editing Dart][].

您也可以在命令列中使用 `flutter` 命令，
並配合能編輯 [Dart 語言的編輯器][editing Dart] 進行開發。


[Android Studio]: {{site.android-dev}}/studio
[Android Studio/IntelliJ]: {{site.url}}/tools/android-studio
[editing Dart]: {{site.dart-site}}/tools
[editor configuration]: {{site.url}}/get-started/editor
[IntelliJ IDEA]: https://www.jetbrains.com/idea/
[VS Code]: https://code.visualstudio.com/

### Does Flutter come with a framework?

### Flutter 裡存在開發框架嗎？

Yes! Flutter ships with a modern react-style framework.
Flutter's framework is designed to be layered and
customizable (and optional). Developers can choose to
use only parts of the framework, or even replace
upper layers of the framework entirely.

是的，Flutter 自帶了現代化的開發框架，靈感正是來自 React。
Flutter 的框架旨在實現分層、可客製 (以及靈活的開發選項)。
開發者可以選擇僅使用框架的一部分，或是使用另外的框架。

### Does Flutter come with widgets?

### Flutter 裡存在 Widget 嗎？

Yes! Flutter ships with a set of
[high-quality Material Design and Cupertino
(iOS-style) widgets][widgets], layouts, and themes.
Of course, these widgets are only a starting point.
Flutter is designed to make it easy to create your own
widgets, or customize the existing widgets.

是的，Flutter 自帶了一套
[高品質的 Material Design 和 Cupertino (iOS 風格) Widget][widgets]、
佈局和主題。當然，這些 Widget 只是一個起點。
Flutter 的設計目的就是，讓您輕鬆建立自己的 Widget，或是客製現有的 Widget。

[widgets]: {{site.url}}/ui/widgets

### Does Flutter support Material Design?

### Flutter 支援 Material Design 嗎？

Yes! The Flutter and Material teams collaborate closely,
and Material is fully supported. For more information,
check out the Material 2 and Material 3 widgets
in the [widget catalog][].

是的，Flutter 和 Material 團隊密切合作，完全支援 Material Theming。
你可以透過 [widget 目錄][widget catalog]
瞭解支援 Material 2 和 3 的 widget。

[widget catalog]: {{site.url}}/ui/widgets/material

### Does Flutter come with a testing framework?

### Flutter 帶有測試框架嗎？

Yes, Flutter provides APIs for writing unit and
integration tests. Learn more about [testing with Flutter][].

是的，Flutter 提供用於編寫單元和整合測試的 API。
瞭解更多有關 Flutter 測試的資訊請檢視
[測試 Flutter 應用][testing with Flutter]。

We use our own testing capabilities to test our SDK,
and we measure our [test coverage][] on every commit.

我們使用自己的測試功能來測試我們的 SDK，
每次提交程式碼前我們都會測量提交的 [測試覆蓋率][test coverage]。

[test coverage]: https://coveralls.io/github/flutter/flutter?branch=master
[testing with Flutter]: {{site.url}}/testing/overview

### Does Flutter come with debugging tools?

### Flutter 是否附帶除錯工具？

Yes, Flutter comes with [Flutter DevTools][] (also
called Dart DevTools). For more information, see
[Debugging with Flutter][] and the [Flutter DevTools][] docs.

Flutter 本身附帶了 [除錯工具][Flutter DevTools]（也稱為 Dart DevTools）。
你可以在 [除錯 Flutter][Debugging with Flutter]
和 [Flutter DevTools][] 文件中瞭解更多資訊。


[Debugging with Flutter]: {{site.url}}/testing/debugging
[Flutter DevTools]: {{site.url}}/tools/devtools/overview

### Does Flutter come with a dependency injection framework?

### Flutter 是否帶有依賴注入 (dependency injection) 的框架？

We don't ship with an opinionated solution,
but there are a variety of packages that offer
dependency injection and service location,
such as [injectable][], [get_it][], [kiwi][], and [riverpod][].

我們並沒有提供相關解決方案，
但是這裡有許多套件提供了依賴注入或服務定位的能力，
例如 [injectable][]、[get_it][]、[kiwi][] 和 [riverpod][]。


[get_it]: {{site.pub}}/packages/get_it
[injectable]: {{site.pub}}/packages/injectable
[kiwi]: {{site.pub}}/packages/kiwi
[riverpod]: {{site.pub}}/packages/riverpod

## Technology

## 技術篇

### What technology is Flutter built with?

### Flutter 是使用什麼技術建構的？

Flutter is built with C, C++, Dart,
Skia (a 2D rendering engine),
and [Impeller][] (the default rendering engine on iOS).
See this [architecture diagram][]
for a better picture of the main components.
For a more detailed description of the layered architecture
of Flutter, read the [architectural overview][].

Flutter 使用 C、C++、Dart、Skia (2D 渲染引擎)
以及 [Impeller][] (iOS 預設的渲染引擎) 建構。
您可以參閱下面這張 [架構圖][architecture diagram] 來理解其主要建構。
若您需要了解 Flutter 的分層架構，請閱讀 [架構概覽][architectural overview]。

[architectural overview]: {{site.url}}/resources/architectural-overview
[architecture diagram]: https://docs.google.com/presentation/d/1cw7A4HbvM_Abv320rVgPVGiUP2msVs7tfGbkgdrTy0I/edit#slide=id.gbb3c3233b_0_162
[Impeller]: {{site.url}}/perf/impeller

### How does Flutter run my code on Android? {#run-android}

### Flutter 如何在 Android 上執行我的程式碼？

The engine's C and C++ code are compiled with Android's NDK.
The Dart code (both the SDK's and yours)
are ahead-of-time (AOT) compiled into native, ARM, and x86
libraries. Those libraries are included in a "runner"
Android project, and the whole thing is built into an `.apk`.
When launched, the app loads the Flutter library.
Any rendering, input, or event handling, and so on,
is delegated to the compiled Flutter and app code.
This is similar to the way many game engines work.

引擎的 C 和 C++ 程式碼使用 Android 的 NDK 編譯。
Dart 程式碼 (SDK 的和您寫的) 都是預先
(ahead-of-time, AOT) 編譯成本地 ARM 及 x86 庫。
這些庫被包含在一個 Android "runner" 專案中，然後整套內容被編譯成一個 APK。
當應用啟動時，它會載入 Flutter 庫。
任何渲染、輸入或事件處理等都會 delegate 給編譯好的 Flutter 和應用程式碼。
這個工作機制與很多遊戲引擎頗為相似。

During debug mode, Flutter uses a virtual machine (VM)
to run its code in order to enable stateful hot reload,
a feature that lets you make changes to your running code
without recompilation. You'll see a "debug" banner in
the top right-hand corner of your app when running in this mode,
to remind you that performance is not characteristic of
the finished release app.

除錯模式時，Flutter 使用虛擬機器 (VM) 來執行 Dart 程式碼
（因此這時會顯示 "Debug" 字樣，以提醒開發者速度會稍微變慢)，
這樣便可以啟用有狀態熱重載 (Stateful Hot Reload)，
它能夠讓你無需重新編譯整個應用就能看到程式碼變更帶來的變化。
當執行該模式時，你可以看到一個 “debug” banner 在你應用的右上角。
請記住，這時的效能並不是最終釋出應用時的效能。

### How does Flutter run my code on iOS? {#run-ios}

### Flutter 如何在 iOS 上執行我的程式碼？{#run-ios}

The engine's C and C++ code are compiled with LLVM.
The Dart code (both the SDK's and yours)
are ahead-of-time (AOT) compiled into a native, ARM library.
That library is included in a "runner" iOS project,
and the whole thing is built into an `.ipa`.
When launched, the app loads the Flutter library.
Any rendering, input or event handling, and so on,
are delegated to the compiled Flutter and app code.
This is similar to the way many game engines work.

引擎的 C 和 C++ 程式碼使用 LLVM 編譯。Dart 程式碼 (SDK 的和您的)
都是預先 (ahead-of-time, AOT) 編譯成本地 ARM 庫。
這些庫被包含在一個 iOS "runner" 專案中，
然後整套內容被編譯成一個 `.ipa`。當應用啟動時，
它會載入 Flutter 庫。任何渲染、輸入或事件處理等都會
代理給編譯好的 Flutter 和應用程式碼。
這個工作機制與很多遊戲引擎頗為相似。

During debug mode, Flutter uses a virtual machine (VM)
to run its code in order to enable stateful hot reload,
a feature that lets you make changes to your
running code without recompilation. You'll see
a "debug" banner in the top right-hand corner of
your app when running in this mode, to remind you that
performance is not characteristic of the finished release app.

在 debug 模式下，Flutter 使用虛擬環境 (VM) 來執行程式碼，
使其可以保持狀態並且可以熱重載，
這個功能讓你可以在無需重新編譯的前提下立刻看到新程式碼生效的結果。
在 debug 模式下，你會看到右上角有一個 "debug" 的橫幅條，
它會提醒你 debug 模式下的效能並不代表 release 模式的效能。

### Does Flutter use my operating system's built-in platform widgets?

### Flutter 是否會使用作業系統內建的 widget？

No. Instead, Flutter provides a set of widgets
(including Material Design and Cupertino (iOS-styled) widgets),
managed and rendered by Flutter's framework and engine.
You can browse a [catalog of Flutter's widgets][].

不會。相反，Flutter 自己提供了一套 widget
(包括 Material Design 和 iOS 風格的 Cupertino widget)，
由 Flutter 的框架和引擎負責管理和渲染。
你可以在這裡瀏覽 [Flutter widget 目錄][catalog of Flutter's widgets]。

We believe that the end result is higher quality apps.
If we reused the built-in platform widgets,
the quality and performance of Flutter apps would be limited
by the flexibility and quality of those widgets.

我們希望最終能夠產生出更高品質的應用。
如果我們直接使用 OEM 自帶的 widget，
那麼 Flutter 應用的品質和效能將受到這些 widget 品質的限制。

In Android, for example, there's a hard-coded set
of gestures and fixed rules for disambiguating them.
In Flutter, you can write your own gesture recognizer
that is a first class participant in the [gesture system][].
Moreover, two widgets authored by different people can
coordinate to disambiguate gestures.

例如，在 Android 中，有一組硬編碼的手勢和固定的計算規則來區別它們。
在 Flutter 中，您可以編寫自己的手勢識別器，
它在 [手勢系統][gesture system] 中擁有最高的優先順序。
此外，由不同人創作的兩個 widget 可以進行協調，以便消除手勢的歧義。

Modern app design trends point towards designers and
users wanting more motion-rich UIs and brand-first designs.
In order to achieve that level of customized, beautiful design,
Flutter is architectured to drive pixels instead
of the built-in widgets.

如今的應用設計趨勢表明，很多設計師和使用者都需要動效豐富的 UI，同時富有品牌表現力。
為了實現這種級別的美學客製化設計，Flutter 在架構上就會傾向於直接驅動畫素，
而不是交給 OEM widget 來處理。

By using the same renderer, framework, and set of widgets,
it's easier to publish for multiple platforms from the same
codebase, without having to do careful and costly planning
to align different feature sets and API characteristics.

由於使用相同的渲染器、框架和 widget，
就意味著您能更加輕鬆地同時釋出 iOS 和 Android 版本應用，
而無需耗費精力和成本來規劃和同步兩套獨立的程式碼庫和功能集。

By using a single language, a single framework,
and a single set of libraries for all of your code
(regardless if your UI is different for each platform or not),
we also aim to help lower app development and maintenance costs.

另外，使用單一的語言、單個框架和同一組適用於所有 UI 的函式庫
（無論您的 UI 在每個移動平臺上都各有不同還是基本一致），
也有助於幫助您降低應用開發和維護成本。


[catalog of Flutter's widgets]: {{site.url}}/ui/widgets
[gesture system]: {{site.url}}/ui/interactivity/gestures

### What happens when my mobile OS updates and introduces new widgets?

### 我的移動 OS 更新並加入新的 widget 時會怎麼樣？

The Flutter team watches the adoption and demand for new mobile
widgets from iOS and Android, and aims to work with the community
to build support for new widgets. This work might come in the form
of lower-level framework features, new composable widgets,
or new widget implementations.

Flutter 團隊密切關注來自 iOS 和 Android 的 widget 使用和需求情況，
且會與社群合作，對新的 widget 提供建構支援。
這些支援可能會以這些形式來提供給開發者: 
較低層級的框架功能、新的可編輯組合的 widget，或全新的 widget 實現。

Flutter's layered architecture is designed to support numerous
widget libraries, and we encourage and support the community in
building and maintaining widget libraries.

Flutter 的分層架構旨在支援眾多 widget 庫，我們鼓勵並支援社群建構和維護 widget 庫。

### What happens when my mobile OS updates and introduces new platform capabilities?

### 我的移動 OS 更新並加入新的平台功能時會怎麼樣？

Flutter's interop and plugin system is designed to allow
developers to access new mobile OS features and capabilities
immediately. Developers don't have to wait for the Flutter team
to expose the new mobile OS capability.

Flutter 的互操作 (interop) 和外掛 (plugin)
系統旨在使開發者能夠立即存取新的移動作業系統特性和功能。
開發者不必等待 Flutter 團隊提供新系統功能的存取介面，而是自己第一時間即可使用。

### What operating systems can I use to build a Flutter app?

### 我能使用哪些作業系統開發 Flutter 應用？

Flutter supports development using Linux, macOS, ChromeOS,
and Windows.

Flutter 支援使用 Linux、Mac 和 Windows 進行開發。

### What language is Flutter written in?

### Flutter 是用哪種語言寫成的？

[Dart][], a fast-growing modern language optimized
for client apps. The underlying graphics framework
and the Dart virtual machine are implemented in C/C++.

[Dart][] 是一個現代化高度發展，併為終端應用專門最佳化的語言。
底層圖形框架和 Dart 虛擬機器在 C/C++ 中實現。

### Why did Flutter choose to use Dart?

### Flutter 為什麼選擇使用 Dart？

During the initial development phase,
the Flutter team looked at a lot of
languages and runtimes, and ultimately
adopted Dart for the framework and widgets.
Flutter used four primary dimensions for evaluation,
and considered the needs of framework authors,
developers, and end users. We found many languages
met some requirements, but Dart scored highly on
all of our evaluation dimensions and met all our
requirements and criteria.

在最初的開發階段，Flutter 團隊調研了很多開發語言和執行時，
最終在框架和小部件中採用了 Dart。
Flutter 主要基於四個維度進行評估，並同時考慮了框架作者、開發人員和終端使用者的需求。
我們發現許多語言都滿足了一些要求，但 Dart 在我們所有的評估維度上都獲得了高分，
並且滿足了我們的所有要求和標準。

Dart runtimes and compilers support the combination of
two critical features for Flutter: a JIT-based fast
development cycle that allows for shape changing and
stateful hot reloads in a language with types,
plus an Ahead-of-Time compiler that emits efficient
ARM code for fast startup and predictable performance of
production deployments.

Dart 執行時和編譯器支援 Flutter 的兩個關鍵功能的組合：
基於 JIT 的高效開發，允許在具有型別的語言中進行形參更改，以及保持狀態的熱重載；
還有 AOT 編譯器，可產出高效的 ARM 程式碼，為生產部署帶來快速啟動和可觀的效能。

In addition, we have the opportunity to work closely
with the Dart community, which is actively investing
resources in improving Dart for use in Flutter. For
example, when we adopted Dart,
the language didn't have an ahead-of-time
toolchain for producing native binaries,
which is instrumental in achieving predictable,
high performance, but now the language does because the Dart team
built it for Flutter. Similarly, the Dart VM has
previously been optimized for throughput but the
team is now optimizing the VM for latency, which is more
important for Flutter's workload.

此外，我們還有幸與 Dart 社群展開了密切合作，Dart 社群積極投入資源改進 Dart，以便在 Flutter 中更易使用。例如，當我們採用 Dart 時，該語言還沒有用於產生原生二進位制檔案的 AOT 工具鏈，這些工具有助於實現穩定的高效能表現，但在 Dart 團隊為 Flutter 建構了這些工具後，這個缺失已經不復存在了。同樣，Dart VM 之前是針對吞吐量進行的最佳化，但團隊現在正在針對延遲進行最佳化，這對於解決 Flutter 的工作負載更為重要。

Dart scores highly for us on the following primary criteria:

在評估時，Dart 在以下主要標準上得分很高：

_Developer productivity_
<br> One of Flutter's main value propositions is that it
  saves engineering resources by letting developers
  create apps for both iOS and Android with the same codebase.
  Using a highly productive language accelerates
  developers further and makes Flutter more attractive.
  This was very important to both our framework team as
  well as our developers. The majority of Flutter
  is built in the same language we give to our users,
  so we need to stay productive at 100k's lines of code,
  without sacrificing approachability or
  readability of the framework and widgets for our developers.

**開發人員生產力**
<br> Flutter 的主要價值之一，是透過讓開發人員用同一套程式碼，
  建立適用於 iOS 和 Android 的應用而節省開發資源。
  使用高生產力的語言加速開發，並提升 Flutter 的吸引力。
  這對於我們的框架團隊和開發人員都很重要。
  Flutter 本身的大部分內容所用的語言都和我們提供給使用者的一樣，
  所以我們要讓十萬行程式碼保持生產力，
  而不會犧牲框架和部件對我們開發人員的可達性和可讀性。

_Object-orientation_
<br> For Flutter, we want a language that's suited to
  Flutter's problem domain: creating visual user experiences.
  The industry has multiple decades of experience building
  user interface frameworks in object-oriented languages.
  While we could use a non-object-oriented language,
  this would mean reinventing the wheel to solve several
  hard problems. Plus, the vast majority of developers
  have experience with object-oriented development,
  making it easier to learn how to develop with Flutter.

**面向物件**
<br> 對於 Flutter 而言，我們需要一種適合建立視覺化使用者體驗的語言。
  這個領域中沉澱了數十年的面向物件建構 UI 框架的經驗。
  雖然我們可以使用非物件導向語言，但這意味著，
  為了解決幾個難題，我們要 "重新發明輪子"。
  此外，絕大多數開發者都擁有面向物件開發的經驗，
  因此可以更輕鬆地學習如何使用 Flutter 進行開發。

_Predictable, high performance_
<br> With Flutter, we want to empower developers to create fast,
  fluid user experiences. In order to achieve that, we need to
  be able to run a significant amount of end-developer code
  during every animation frame. That means we need a language
  that both delivers high performance and predictable
  performance, without periodic pauses that would cause
  dropped frames.

**穩定可期的高效能表現**
<br> 我們希望開發者能夠透過 Flutter 建立快速而流暢的使用者體驗。
  為了實現這一點，我們需要能夠在每個動畫幀期間執行大量的最終開發者程式碼。
  這意味著我們需要的語言一方面既要擁有高效能，
  另一方面又需要避免因週期性的中斷而影響幀率，即 "可期性"。

_Fast allocation_
<br> The Flutter framework uses a functional-style flow that
  depends heavily on the underlying memory allocator
  efficiently handling small, short-lived allocations.
  This style was developed in languages with this
  property and doesn't work efficiently in languages
  that lack this facility.

**快速記憶體分配**
<br> Flutter 框架使用的函式式流程，
很大程度上依賴於下層的記憶體分配器高效地對小型的、短生命週期的內容進行記憶體分配。
這個流程是使用支援這種分配機制的語言進行開發的，
在缺少這個機制的語言中無法有效運作。

### Can Flutter run any Dart code?

### Flutter 可以執行任意的 Dart 程式碼嗎？

Flutter can run Dart code that doesn't directly or
transitively import `dart:mirrors` or `dart:html`.

Flutter 可以執行那些沒有直接或間接匯入了
`dart:mirrors` 或 `dart:html` 的函式庫。

### How big is the Flutter engine?

### Flutter 引擎有多大？

In March 2021, we measured the download size of a
[minimal Flutter app][] (no Material Components,
just a single `Center` widget, built with `flutter build
apk --split-per-abi`), bundled and compressed as a release APK,
to be approximately 4.3 MB for ARM32, and 4.8 MB for ARM64.

2021 年 3 月，我們實測了一個
[最簡版本的 Flutter 應用][minimal Flutter app]
（即不含 Material 元件，只包含一個使用 `flutter build apk --split-per-abi`
建構的 `Center` widget 的 app）
壓縮且 Bundle 一個 release 的 APK，
ARM64 下是 4.6 MB，ARM32 下是 4.3 MB。

On ARM32, the core engine is approximately 3.4 MB
(compressed), the framework + app code is approximately
765 KB (compressed), the LICENSE file is 58 KB
(compressed), and necessary Java code (`classes.dex`)
is 120 KB (compressed).

在 ARM32 下，核心的引擎大約佔 3.4 MB，框架和應用的程式碼大約是 765 KB，
許可證檔案大約是 58 KB，必要的 Java 程式碼（classes.dex）是 120 KB。
上述資料均為經過壓縮處理之後的大小。

In ARM64, the core engine is approximately 4.0 MB 
(compressed), the framework + app code is approximately
659 KB (compressed), the LICENSE file is 58 KB
(compressed), and necessary Java code (`classes.dex`)
is 120 KB (compressed).

在 ARM64 下，核心的引擎大約佔 4.0 MB，框架和應用的程式碼大約是 659 KB，
許可證檔案大約是 58 KB，必要的 Java 程式碼（classes.dex）是 120 KB。
上述資料均為經過壓縮處理之後的大小。

These numbers were measured using [apkanalyzer][],
which is also [built into Android Studio][].

這些數字是由 [AndroidStudio][built into Android Studio]
內建的 [apkanalyzer][] 實測得出。

On iOS, a release IPA of the same app has a download
size of 10.9 MB on an iPhone X, as reported by Apple's
App Store Connect. The IPA is larger than the APK mainly
because Apple encrypts binaries within the IPA, making the
compression less efficient (see the
[iOS App Store Specific Considerations][]
section of Apple's [QA1795][]).

在 iOS 平臺上，跟據 App Store Connect 的資料，
同一應用的釋出 IPA 在 iPhone X 上的下載檔案體積為 10.9 MB。
IPA 比 APK 大，主要是因為 Apple 加密了 IPA 中的二進位制檔案，
使得壓縮效率降低。
（可以檢視 [iOS App Store Specific Considerations][]
中 [QA1795][] 關於加密的部分）

{{site.alert.note}}

  The release engine binary used to include LLVM IR (bitcode).
  However, Apple [deprecated bitcode in Xcode 14][] and removed support,
  so it has been removed from the Flutter 3.7 release.

  釋出版本的引擎二進位制檔案曾經包含 LLVM IR（bitcode）。
  但是，Apple 已在 [Xcode 14 中廢棄][deprecated bitcode in Xcode 14]
  並移除了對 bitcode 的支援，因此，
  我們也在 Flutter 3.7 版本中移除了 bitcode。

{{site.alert.end}}

Of course, we recommend that you measure your own app.
To do that, see [Measuring your app's size][].

當然，您的實際情況可能跟我們所說的有所不同，我們建議您測量自己的應用的體積。
想要測量應用體積，請檢視 [測量你的應用體積][Measuring your app's size]。


[apkanalyzer]: {{site.android-dev}}/studio/command-line/apkanalyzer
[built into Android Studio]: {{site.android-dev}}/studio/build/apk-analyzer
[deprecated bitcode in Xcode 14]: {{site.apple-dev}}/documentation/xcode-release-notes/xcode-14-release-notes
[iOS App Store Specific Considerations]: {{site.apple-dev}}/library/archive/qa/qa1795/_index.html#//apple_ref/doc/uid/DTS40014195-CH1-APP_STORE_CONSIDERATIONS
[Measuring your app's size]: {{site.url}}/perf/app-size
[minimal Flutter app]: {{site.repo.flutter}}/tree/75228a59dacc24f617272f7759677e242bbf74ec/examples/hello_world
[QA1795]: {{site.apple-dev}}/library/archive/qa/qa1795/_index.html

### How does Flutter define a pixel?

Flutter uses logical pixels,
and often refers to them merely as "pixels".
Flutter's [`devicePixelRatio`][] expresses the ratio
between physical pixels and logical CSS pixels. 

[`devicePixelRatio`]: {{site.api}}/flutter/dart-html/Window/devicePixelRatio.html

## Capabilities

## 賦能

### What kind of app performance can I expect?

### Flutter 應用會擁有怎樣的效能表現？

You can expect excellent performance. Flutter is
designed to help developers easily achieve a constant 60fps.
Flutter apps run using natively compiled code&mdash;no
interpreters are involved.
This means that Flutter apps start quickly.

Flutter 應用會有很出色的效能。
Flutter 設計的目標就是幫助開發者輕鬆實現 60fps 的穩定幀率。
Flutter 應用透過本地編譯的程式碼執行&mdash;&mdash;不涉及解釋過程。
這也意味著 Flutter 應用啟動會非常快捷。

### What kind of developer cycles can I expect? How long between edit and refresh? {#hot-reload}

### 開發 Flutter 時的操作週期有多長？修改程式碼和看到介面內容更新之間會隔多久？

Flutter implements a _hot reload_ developer cycle. You can expect
sub-second reload times, on a device or an emulator/simulator.

Flutter 使用的是熱重載式的開發操作週期。
您在實機或者模擬器上都能實現亞秒級的修改和更新速度。

Flutter's hot reload is _stateful_ so the app state
is retained after a reload. This means you can quickly iterate
on a screen deeply nested in your app, without starting
from the home screen after every reload.

另外，Flutter 的熱重載是有狀態的 (stateful)，
這意味著重新載入後 app 的狀態會被保留。
這樣即使您修改的介面在應用很深的位置，
重載後您也能直接看到修改後的該介面，
而無需從應用首頁開始重新操作。

### How is _hot reload_ different from _hot restart_?

### 熱重載 **hot reload** 相比較熱重啟 **hot restart** 的區別在哪裡？

Hot reload works by injecting updated source code files
into the running Dart VM (Virtual Machine). This doesn't
only add new classes, but also adds methods and fields
to existing classes, and changes existing functions.
Hot restart resets the state to the app's initial state.

透過將更新的原始碼檔案注入到正在執行的 Dart VM（虛擬機器）中來進行熱重載。
這不僅會新增新的類，還會向現有的類中新增方法和欄位，並更改現有的函式。
熱重啟後會將狀態重置為應用程式的初始狀態。

For more information, see [Hot reload][].

更多關於熱重載的詳細資訊，請檢視文件：[使用熱重載][Hot reload]。


[Hot reload]: {{site.url}}/tools/hot-reload

### Where can I deploy my Flutter app?

### 我能把 Flutter 應用部署到哪裡？

You can compile and deploy your Flutter app to iOS, Android,
[web][], and [desktop][].

您可以將 Flutter 應用編譯並部署到 iOS 和 Android 平台，
亦可部署到 [web][] 平台以及 [桌面端][desktop]。


[desktop]: {{site.url}}/platform-integration/desktop
[web]: {{site.url}}/platform-integration/web

### What devices and OS versions does Flutter run on?

### Flutter 可以執行在哪些裝置，哪些作業系統版本上？

* We support and test running Flutter on a variety
  of low-end to high-end platforms.  For a detailed list
  of the platforms on which we test, see 
  the list of [supported platforms][].

  我們會為各種從低端到高階的平台進行支援並且加入測試。
  您可以檢視 [已支援的平台][supported platforms] 以瞭解已測試的平台列表。

* Flutter supports building ahead-of-time (AOT) compiled libraries
  for `x86_64`, `armeabi-v7a`, and `arm64-v8a`.

  Flutter 支援在 `x86_64`、`armeabi-v7a` 和 `arm64-v8a`
  架構下建構為 ahead-of-time (AOT) 庫。

* Apps built for ARMv7 or ARM64 run fine (using ARM emulation)
  on many x86 Android devices.

  為 ARMv7 或 ARM64 建構的應用在很多
  x86 Android 裝置上執行良好 (使用 ARM 模擬器)。

* We support developing Flutter apps on a range of platforms.
  See the system requirements listed under each
  [development operating system][install].

  我們支援在不同的平臺上開發 Flutter 應用，
  請參閱 [不同作業系統下安裝 Flutter 的方法文件][install] 瞭解更多。


[install]: {{site.url}}/get-started/install
[supported platforms]: {{site.url}}/reference/supported-platforms

### Does Flutter run on the web?

### Flutter 能在 Web 上執行嗎？

Yes, web support is available in the stable channel.
For more details, check out the [web instructions][].

可以的，目前 stable channel 已經支援 web 平台了。
你可以將已有的 Flutter 程式碼編譯在 web 執行。
更多詳細資訊，請參閱 [Web 介紹][web instructions]。

[web instructions]: {{site.url}}/platform-integration/web/building

### Can I use Flutter to build desktop apps?

### 我能使用 Flutter 建構桌面應用嗎？

Yes, desktop support is in stable for Windows,
macOS, and Linux.

可以，Flutter 的桌面端穩定版支援已經適用於
Windows、macOS 和 Linux 啦。

### Can I use Flutter inside of my existing native app?

### 我能在我現有的原生應用裡使用 Flutter 嗎？

Yes, learn more in the [add-to-app][] section of our website.

是的，你可以在我們網上內的 [混合應用][add-to-app] 章節中學習。

[add-to-app]: {{site.url}}/add-to-app

### Can I access platform services and APIs like sensors and local storage?

### 我能存取感測器、本地儲存之類別的平台服務和 API 嗎？

Yes. Flutter gives developers out-of-the-box access to _some_
platform-specific services and APIs from the operating system.
However, we want to avoid the "lowest common denominator" problem
with most cross-platform APIs, so we don't intend to build
cross-platform APIs for all native services and APIs.

可以。Flutter 預設即為開發者提供了作業系統中 **一些** 平台專屬服務和 API 的操作入口。
但是，我們希望避免大多數跨平臺 API 的“最小公約數”問題，
因此我們不打算為所有本地服務和 API 建構跨平臺的操作 API。

A number of platform services and APIs have
[ready-made packages][] available on pub.dev.
Using an existing package [is easy][].

很多平臺服務和 API 都在 Pub 站點中提供了
[現成的程式碼包][ready-made packages]，
我們可以根據 [說明][is easy] 使用它們，非常方便。

Finally, we encourage developers to use Flutter's
asynchronous message passing system to create your
own integrations with [platform and third-party APIs][].
Developers can expose as much or as little of the
platform APIs as they need, and build layers of
abstractions that are a best fit for their project.

最後，我們鼓勵開發者使用 Flutter 的非同步訊息傳遞系統來創建出
[自己的平台][platform and third-party APIs] 與第三方 API 的整合方案。
開發者可以根據需要公開儘可能多 (或者儘可能少) 的平台 API，
並建構最適合其專案的抽象層。


[is easy]: {{site.url}}/packages-and-plugins/using-packages
[platform and third-party APIs]: {{site.url}}/platform-integration/platform-channels
[ready-made packages]: {{site.pub}}/flutter/

### Can I extend and customize the bundled widgets?

### 我能對自帶的 widget 進行擴充和客製嗎？

Absolutely. Flutter's widget system was designed
to be easily customizable.

當然可以。Flutter widget 系統的設計思路就是讓開發者可以輕鬆客製。

Rather than having each widget provide a large number of parameters,
Flutter embraces composition. Widgets are built out of smaller
widgets that you can reuse and combine in novel ways to make
custom widgets. For example, rather than subclassing a generic
button widget, `ElevatedButton` combines a Material widget with a
`GestureDetector` widget. The Material widget provides the visual
design and the `GestureDetector` widget provides the
interaction design.

Flutter 沒有讓每個 widget 都提供大量引數，而是採用了組合的方式。
較大的 widget 是用較小的 widget 組合構建出來的，
您可以重複使用它們，並以新穎的方式對其加以組合，從而產生自訂的 widget。
例如，`RaisedButton` 沒有繼承自一個通用按鈕 widget，
而是將 Material widget 與 `GestureDetector` widget 組合在一起。
Material widget 負責視覺呈現，`GestureDetector` widget 則實現其互動。

To create a button with a custom visual design, you can combine
widgets that implement your visual design with a `GestureDetector`,
which provides the interaction design. For example,
`CupertinoButton` follows this approach and combines a
`GestureDetector` with several other widgets that implement its
visual design.

如果您想要建立自訂設計的按鈕，
可以將負責視覺呈現的 widget 與提供互動的 `GestureDetector` 組合起來使用。
例如，`CupertinoButton` 就採用了這種方法，
將 `GestureDetector` 與其他幾個負責表現視覺的 widget 進行組合。

Composition gives you maximum control over the visual and
interaction design of your widgets while also allowing a
large amount of code reuse. In the framework, we've decomposed
complex widgets to pieces that separately implement
the visual, interaction, and motion design. You can remix
these widgets however you like to make your own custom
widgets that have full range of expression.

這種組合策略使您可以最大限度地控制 widget 的視覺化和互動邏輯，
同時重複利用大量程式碼。在框架中，
我們將複雜的 widget 分解為實現視覺、互動和動效的各部分。
您可以按照自己喜歡的方式重新組合這些 widget，
從而製作出自訂 widget 來完整傳達出您的設計意圖。

### Why would I want to share layout code across iOS and Android?

### 我為什麼要在 iOS 和 Android 應用間共享佈局程式碼？

You can choose to implement different app layouts for
iOS and Android. Developers are free to check the mobile OS
at runtime and render different layouts,
though we find this practice to be rare.

您可以選擇為 iOS 和 Android 應用實現不同的佈局。
開發者可以在執行時期檢查移動作業系統的種類，並根據作業系統呈現不同的佈局，
但我們發現這種做法比較少見。

More and more, we see mobile app layouts and designs evolving
to be more brand-driven and unified across platforms.
This implies a strong motivation to share layout and UI
code across iOS and Android.

我們發現移動應用佈局和設計正在不斷髮展，更趨於品牌設計的訴求，
而且跨平臺之間的呈現逐漸趨同。
這意味著不少開發者會有很強的動力在 iOS 和 Android 上共享佈局和 UI 程式碼。

The brand identity and customization of the app's
aesthetic design is now becoming more important than
strictly adhering to traditional platform aesthetics.
For example, app designs often require custom fonts, colors,
shapes, motion, and more in order to clearly convey their
brand identity.

如今，在應用美學設計中，品牌表達和客製比嚴格遵循平台自己的美學更為重要。
例如，應用設計通常需要自訂字型、顏色、形狀、動效等，
以便清楚地傳達出其品牌獨有的特性。

We also see common layout patterns deployed across
iOS and Android. For example, the "bottom nav bar"
pattern can now be naturally found across iOS and Android.
There seems to be a convergence of design ideas
across mobile platforms.

我們還發現，很多應用都在 iOS 和 Android 上採用了通用的佈局模式。
例如，您現在可以在 iOS 和 Android 上很方便地找到“底部導航”設計模式。
移動平臺上的設計理念似乎正在趨於一致。

### Can I interop with my mobile platform's default programming language?

### 我能與移動平臺上的預設程式語言進行互操作嗎？

Yes, Flutter supports calling into the platform,
including integrating with Java or Kotlin code on Android,
and ObjectiveC or Swift code on iOS.
This is enabled by a flexible message passing style
where a Flutter app might send and receive messages
to the mobile platform using a [`BasicMessageChannel`][].

可以，Flutter 支援呼叫 (包括整合) Android 上的 Java 或者 Kotlin 程式碼，
或者 iOS 上的 ObjectiveC 或 Swift 程式碼。
這是透過靈活的訊息傳遞方式實現的，
Flutter 應用可以使用 [`BasicMessageChannel`][] 向移動平臺收發訊息。

Learn more about accessing platform and third-party services
in Flutter with [platform channels][].

如果你想了解有關平台通道的更多資訊，可以查閱 [platform channels][] 相關文件。

Here is an [example project][] that shows how to use a
platform channel to access battery state information on
iOS and Android.

你也可以透過這個 [範例專案][example project]，
學習如何使用平台通道存取 iOS 和 Android 上的電池狀態資訊。


[`BasicMessageChannel`]: {{site.api}}/flutter/services/BasicMessageChannel-class.html
[example project]: {{site.repo.flutter}}/tree/main/examples/platform_channel
[platform channels]: {{site.url}}/platform-integration/platform-channels

### Does Flutter come with a reflection / mirrors system?

### Flutter 包含反射 / 鏡像系統嗎？

No. Dart includes `dart:mirrors`,
which provides type reflection. But since
Flutter apps are pre-compiled for production,
and binary size is always a concern with mobile apps,
this library is unavailable for Flutter apps.

不支援，Dart 的確是含有 `dart:mirrors` 庫，能夠提供型別反射。
但是由於 Flutter 應用已經針對最終產物進行了預編譯，
並且控制二進位制內容體積始終是現代移動應用需要面對的一個問題，
所以我們禁用了 dart:mirrors。

Using static analysis we can strip out anything that isn't
used ("tree shaking"). If you import a huge Dart library
but only use a self-contained two-line method,
then you only pay the cost of the two-line method,
even if that Dart library itself imports dozens and
dozens of other libraries. This guarantee is only secure
if Dart can identify the code path at compile time.
To date, we've found other approaches for specific needs
that offer a better trade-off, such as code generation.

使用靜態型別分析系統，我們可以移除任何不會用到的東西（"得益於 tree shaking 機制"）。
如果你匯入了一個巨大的 Dart 庫，但僅僅用到了一個其中實現的一個兩行的函式，
那麼你只需要付出這兩行函式的代價，即便是這個 Dart 庫中匯入了非常多非常多的函式庫。
此保證僅 Dart 可以在編譯期安全識別程式碼路徑的情況下。
目前，我們已找到其他滿足特定需求的方法以提供更好的平衡，如程式碼產生器。

### How do I do international&shy;ization (i18n), localization (l10n), and accessibility (a11y) in Flutter?

### 我應該如何在 Flutter 實現國際化 (internationalization, i18n)、本地化 (localization, l10n) 和可及性 (accessibility, a11y) ？

Learn more about i18n and l10n in the
[internationalization tutorial][].

關於國際化和本地化，請檢視課程：
[Flutter 應用裡的國際化][internationalization tutorial]。

Learn more about a11y in the
[accessibility documentation][].

關於可及性 / 無障礙使用，請檢視文件：[無障礙][accessibility documentation]。


[accessibility documentation]: {{site.url}}/ui/accessibility-and-internationalization/accessibility
[internationalization tutorial]: {{site.url}}/ui/accessibility-and-internationalization/internationalization

### How do I write parallel and/or concurrent apps for Flutter?

### 我如何為 Flutter 開發並行 (parallel) 和/或併發 (concurrent) 應用？

Flutter supports isolates. Isolates are separate heaps in
Flutter's VM, and they are able to run in parallel
(usually implemented as separate threads). Isolates
communicate by sending and receiving asynchronous messages.

Flutter 支援 isolate，一個個的 isolate 是 Flutter VM 裡彼此獨立的堆 (heap)，
可以並行執行 (通常以獨立執行緒的形式實現)。
Isolate 之間透過非同步收發訊息來進行通訊。

Check out an [example of using isolates with Flutter][].

你可以點選連結檢視 [在 Flutter 中使用 isolate 的範例][example of using isolates with Flutter]。

[example of using isolates with Flutter]: {{site.repo.flutter}}/blob/master/examples/layers/services/isolate.dart

### Can I run Dart code in the background of a Flutter app?

### 我能在 Flutter 應用後臺執行 Dart 程式碼嗎？

Yes, you can run Dart code in a background process on both
iOS and Android. For more information, see the free Medium article
[Executing Dart in the Background with Flutter Plugins and Geofencing][backgnd].

可以，你可以在 iOS 和 Android 後臺處理序中執行 Dart 程式碼。
有關更多資訊，你可以檢視在 Medium 上的文章：
[使用 Flutter 外掛和 Geofencing 在後台執行 Dart 程式碼][backgnd]。

[backgnd]: {{site.flutter-medium}}/executing-dart-in-the-background-with-flutter-plugins-and-geofencing-2b3e40a1a124

### Can I use JSON/XML/<wbr>protobuffers (and so on) with Flutter?

### 我在 Flutter 裡能使用 JSON/XML/protobuffers 等內容嗎？

Absolutely. There are libraries on
[pub.dev][] for JSON, XML, protobufs,
and many other utilities and formats.

當然可以。[Pub 站點][pub.dev] 提供了很多這樣的程式碼庫，
包括 JSON, XML, protobufs 以及很多其他內容格式。

For a detailed writeup on using JSON with Flutter,
check out the [JSON tutorial][].

有關在 Flutter 中使用 JSON 的詳細介紹，你可以檢視
[使用 JSON 的課程][JSON tutorial]。

[JSON tutorial]: {{site.url}}/data-and-backend/serialization/json
[pub.dev]: {{site.pub}}

### Can I build 3D (OpenGL) apps with Flutter?

### 我能用 Flutter 建構 3D (OpenGL) 應用嗎？

Today we don't support 3D using OpenGL ES or similar.
We have long-term plans to expose an optimized 3D API,
but right now we're focused on 2D.

我們暫不支援透過 OpenGL ES 或類似的機制實現 3D。
在 3D API 方面我們有一個長期的計劃，但目前我們專注於呈現 2D。

### Why is my APK or IPA so big?

### 我的 APK 或 IPA 為什麼這麼大？

Usually, assets including images, sound files, fonts, and so on,
are the bulk of an APK or IPA. Various tools in the
Android and iOS ecosystems can help you understand
what's inside of your APK or IPA.

通常，圖像、聲音檔案、字型等資源在 APK 或 IPA 裡佔據了相當的比重。
Android 和 iOS 生態系統中有很多工具可以幫助您瞭解 APK 或 IPA 中的各種內容的比重情況。

Also, be sure to create a _release build_
of your APK or IPA with the Flutter tools.
A release build is usually _much_ smaller
than a _debug build_.

此外，請務必使用 Flutter 工具建立 APK 或 IPA 的_釋出版本_。
釋出版本的體積通常_遠_小於_除錯_版本。

Learn more about creating a
[release build of your Android app][],
and creating a [release build of your iOS app][].
Also, check out [Measuring your app's size][].

如果你想學習更多有關如何釋出版本的課程，可以檢視
[打套件和釋出到 Android 平台][release build of your Android app]
以及 [打套件和釋出到 iOS 平台][release build of your iOS app]。
同時，檢視 [測量你的應用體積][Measuring your app's size]。


[release build of your Android app]: {{site.url}}/deployment/android
[release build of your iOS app]: {{site.url}}/deployment/ios

### Do Flutter apps run on Chromebooks?

### Flutter 應用能在 Chromebook 上執行嗎？

We have seen Flutter apps run on some Chromebooks.
We are tracking [issues related to running Flutter on
Chromebooks][].

我們注意到已經有 Flutter 應用執行在某些 Chromebook 上了。
針對在 Chromebook 上執行 Flutter 的情況，
我們有進行持續的追蹤，你可以檢視
[Flutter 執行在 Chromebook 上的問題追蹤][issues related
to running Flutter on Chromebooks]
來獲得相關資訊。

[issues related to running Flutter on Chromebooks]: {{site.repo.flutter}}/labels/platform-arc

### Is Flutter ABI compatible?

### Flutter 是否提供 ABI 支援？

Flutter and Dart don't offer application binary interface (ABI)
compatibility. Offering ABI compatability is not a current
goal for Flutter or Dart.

Flutter 和 Dart 尚未提供且目前不會提供應用二進位制介面 (ABI) 的支援。

## Framework

## 框架

### Why is the build() method on State, rather than StatefulWidget?

### 為什麼 build() 方法被放在 State 上，而不是 StatefulWidget 上？

Putting a `Widget build(BuildContext context)` method on `State`
rather putting a `Widget build(BuildContext context, State state)`
method on `StatefulWidget` gives developers more flexibility when
subclassing `StatefulWidget`. You can read a more
[detailed discussion on the API docs for `State.build`][].

將 Widget 的 build(BuildContext context) 方法放在 `State` 上，
而不是將 `Widget build(BuildContext context, State state)`
方法放在 `StatefulWidget` 上，
這個策略能讓開發者在繼承 `StatefulWidget` 時提供更多的靈活性，
你可以在 API 文件中檢視 
[關於 State.build 的討論][detailed discussion
on the API docs for `State.build`]。

[detailed discussion on the API docs for `State.build`]: {{site.api}}/flutter/widgets/State/build.html

### Where is Flutter's markup language? Why doesn't Flutter have a markup syntax?

### Flutter 怎麼沒有標記語言 (markup language) 和語法？

Flutter UIs are built with an imperative, object-oriented
language (Dart, the same language used to build Flutter's
framework). Flutter doesn't ship with a declarative markup.

Flutter 的 UI 由指令式的物件導向語言建構，也就是 Dart。
它也是 Flutter 框架的編寫語言。Flutter 本身並不包含宣告式的標記語言。

We found that UIs dynamically built with code allow for
more flexibility. For example, we have found it difficult
for a rigid markup system to express and produce
customized widgets with bespoke behaviors.

我們發現將 UI 交給程式碼來動態建構會帶來更多的靈活性。
比如，我們發現固化的標記語言系統很難表達一個從視覺到行為都完全客製的 widget。

We have also found that our "code-first" approach better allows
for features like hot reload and dynamic environment adaptations.

另外，“程式碼優先”的開發也使得熱重載以及動態環境適配等特效能更好地得以實現。

It's possible to create a custom language that is then
converted to widgets on the fly. Because build methods
are "just code", they can do anything,
including interpreting markup and turning it into widgets.

從根本上來講，創造出一種能動態轉化成 widget 的語言是可能的，
畢竟建構方法說到底也還是程式碼，他們能做的事情很多，
自然也包括將標記語言轉化成 widget。

### My app has a Debug banner/ribbon in the upper right. Why am I seeing that?

### 我的應用執行時在右上角有一個 Debug 的標識，為什麼？

By default, the `flutter run` command uses the
debug build configuration.

預設情況下，flutter run 指令會使用 debug 編譯配置。

The debug configuration runs your Dart code in a VM (Virtual Machine)
enabling a fast development cycle with [hot reload][]
(release builds are compiled using the standard [Android][]
and [iOS][] toolchains).

Debug 編譯配置會在一個 VM (Virtual Machine) 裡執行您的 Dart 程式碼，
從而提供更快速的開發操作週期，如 [熱重載][hot reload]。
（如果是編譯釋出版本的話，則會使用 [Android][] 和 [iOS][] 標準的工具鏈。）

The debug configuration also checks all asserts, which helps
you catch errors early during development, but imposes a
runtime cost. The "Debug" banner indicates that these checks
are enabled. You can run your app without these checks by
using either the `--profile` or `--release` flag to `flutter run`.

Debug 編譯配置也會檢查所有的斷言 (assert)，這會幫助您在開發時更早地發現錯誤，
但這也會加大執行時的開銷。您看到的 Debug 標識是告訴您這些檢查目前是開啟的狀態。
您可以透過在執行 `flutter run` 時附加 `--profile`
或者 `--release` 來跳過這些檢查。

If your IDE uses the Flutter plugin,
you can launch the app in profile or release mode.
For VS Code, use the **Run > Start debugging**
or **Run > Run without debugging** menu entries.
For IntelliJ, use the menu entries
**Run > Flutter Run in Profile Mode** or **Release Mode**.

如果您在使用 Flutter 的 IDE 外掛，
您就可以在 profile 或者 release 模式下啟動應用，
對於 VS Code 而言，你可以使用 **Run > Start debugging** 或者
**Run > Run without debugging** 選單選項。
對於 IntelliJ 而言，你可以使用 **Run > Flutter run in Profile Mode** 或者
**Release Mode** 選單選項。

[Android]: #run-android
[hot reload]: #hot-reload
[iOS]: #run-ios

### What programming paradigm does Flutter's framework use?

### Flutter 框架採用了哪些程式設計正規化？

Flutter is a multi-paradigm programming environment.
Many programming techniques developed over the past few decades
are used in Flutter. We use each one where we believe
the strengths of the technique make it particularly well-suited.
In no particular order:

Flutter 是一個多正規化的程式設計環境。過去幾十年中許多程式設計技術都有在 Flutter 中使用。
我們在選擇正規化時會考慮其適用性進行綜合性的決策。以下列出的正規化不分先後：

**Composition**
<br> The primary paradigm used by Flutter is that of using
  small objects with narrow scopes of behavior, composed together to
  obtain more complicated effects, sometimes called
  _aggressive composition_. Most widgets in the Flutter widget
  library are built in this way. For example, the Material
  [`TextButton`][] class is built using
  an [`IconTheme`][], an [`InkWell`][], a [`Padding`][],
  a [`Center`][], a [`Material`][],
  an [`AnimatedDefaultTextStyle`][], and a [`ConstrainedBox`][].
  The [`InkWell`][] is built using a [`GestureDetector`][].
  The [`Material`][] is built using an
  [`AnimatedDefaultTextStyle`][],
  a [`NotificationListener`][], and an [`AnimatedPhysicalModel`][].
  And so on. It's widgets all the way down.

**組合 (composition)** 
<br> 這也是 Flutter 的主要開發正規化，將簡單的、行為有限的小物件進行組合，
  從而實現更復雜的效果。
  絕大多數 Flutter widget 都是用這種方法建構的。
  比如 Material [`TextButton`][] 類是基於
  [`IconTheme`][]、[`InkWell`][]、[`Padding`][]、[`Center`][]、
  [`Material`][]、[`AnimatedDefaultTextStyle`][]
  以及 [`ConstrainedBox`][] 組合而成的。
  而 [`InkWell`][] 則是由 [`GestureDetector`][] 組成，
  [`Material`][] 則是由
  [`AnimatedDefaultTextStyle`][]、[`NotificationListener`][]
  和 [`AnimatedPhysicalModel`][] 組成。
  如此等等。

**Functional programming**
<br> Entire applications can be built with only
  [`StatelessWidget`][]s, which are essentially functions that
  describe how arguments map to other functions, bottoming out
  in primitives that compute layouts or paint graphics.
  (Such applications can't easily have state,
  so are typically non-interactive.) For example, the [`Icon`][]
  widget is essentially a function that maps its arguments
  ([`color`][], [`icon`][], [`size`][]) into layout primitives.
  Additionally, heavy use is made of immutable data structures,
  including the entire [`Widget`][] class
  hierarchy as well as numerous supporting classes such as
  [`Rect`][] and [`TextStyle`][]. On a smaller scale, Dart's
  [`Iterable`][] API, which makes heavy use of the functional
  style (map, reduce, where, etc), is frequently used to process
  lists of values in the framework.

**函數語言程式設計 (functional programming)** 
<br> 整個應用都可以只用 [`StatelessWidget`][] 來建構，它本質上就是一些方法，
  用來描述如何將引數傳送給其他方法，以及在佈局區域內計算佈局以及繪製圖像。
  當然這樣的應用一般也不會包含狀態，所以通常也無法進行互動。
  比如，[`Icon`][] widget 就只是一個將其元素 
  （[顏色][`color`]、[圖示][`icon`]、[尺寸][`size`]）羅列在佈局區域內的方法。
  另外，當這個正規化被重度使用時，則會使用不可變的資料結構，
  如整個 [`Widget`][] 類及其派生，以及一些輔助類，如 [`Rect`][] 和 [`TextStyle`][]。
  另外，從一個較小的尺度來看的話，
  Dart 的 [`Iterable`][] API 也重度使用了這個正規化 (如 map, reduce, where 等方法），
  它在框架中經常被用來處理一系列的值。

**Event-driven programming**
<br> User interactions are represented by event objects
  that are dispatched to callbacks registered with event handlers.
  Screen updates are triggered by a similar callback mechanism. The
  [`Listenable`][] class, which is used as the basis of the
  animation system, formalizes a subscription model for events
  with multiple listeners.

**事件驅動程式設計 (event-driven programming)** 
<br> 使用者的互動操作被包裝成事件物件，
  這些物件傳送給被各個 event handler 註冊的回呼(Callback)方法。
  螢幕內容的更新使用的也是類似的回呼(Callback)機制。
  比如，做為動畫系統建構基礎的 [`Listenable`][] 類，
  就採用了包含多個事件監聽者的訂閱模型。

**Class-based object-oriented programming**
<br> Most of the APIs of the framework are built using classes
  with inheritance. We use an approach whereby we define
  very high-level APIs in our base classes, then specialize
  them iteratively in subclasses. For example,
  our render objects have a base class ([`RenderObject`][])
  that is agnostic regarding the coordinate system,
  and then we have a subclass ([`RenderBox`][])
  that introduces the opinion that the geometry should be based
  on the Cartesian coordinate system (x/width and y/height).
  
**面向類程式設計 (class-based programming，是面向物件程式設計的一種方式)** 
<br> 框架內絕大多數的 API 是由包含各種繼承關係的類來組成的。
  我們在基本類中定義較高級別的 API，然後在其子類別中對這些 API 進行特化處理。
  比如，我們的渲染物件就有一個基本類 [`RenderObject`][])，
  它對座標系的細節並不關心，
  但它的子類別 [`RenderBox`][]) 就引入了笛卡爾座標系的概念
  （x/y座標值，以及寬度高度的概念）。

**Prototype-based object-oriented programming**
<br> The [`ScrollPhysics`][] class chains instances to compose
  the physics that apply to scrolling dynamically at runtime.
  This lets the system compose, for example, paging physics
  with platform-specific physics, without the platform having to be
  selected at compile time.

**原型程式設計 (prototype-based programming，同樣是面向物件程式設計的一種方式)** 
<br> [`ScrollPhysics`][] 類在執行時動態連結那些會組成滾動邏輯的例項。
  這就使得系統無需在編譯時提前選擇平台的情況下，
  也能組合出符合平台特性的頁面滾動效果。

**Imperative programming**
<br> Straightforward imperative programming, usually
  paired with state encapsulated within an object,
  is used where it provides the most intuitive solution.
  For example, tests are written in an imperative style,
  first describing the situation under test, then listing
  the invariants that the test must match, then advancing
  the clock or inserting events as necessary for the test.

**指令式程式設計 (imperative programming)** 
<br> 簡單直白的指令式程式設計，通常和物件內封裝的狀態 (state) 搭配使用，
  這種正規化能提供最符合直覺的解法。
  比如，測試就是使用指令式程式設計實現的，首先描述出測試的環境，
  然後給出測試需要滿足的定量，最後開始步進，
  或者根據測試需要插入事件。

**Reactive programming**
<br> The widget and element trees are sometimes described as
  reactive, because new inputs provided in a widget's
  constructor are immediately propagated as changes to
  lower-level widgets by the widget's build method, and
  changes made in the lower widgets (for example,
  in response to user input) propagate back up the tree
  using event handlers. Aspects of both functional-reactive and
  imperative-reactive are present in the framework,
  depending on the needs of the widgets. Widgets with build
  methods that consist of just an expression describing how
  the widget reacts to changes in its configuration are functional
  reactive widgets (for example, the Material [`Divider`][] class).
  Widgets whose build methods construct a list of children
  over several statements, describing how the widget reacts
  to changes in its configuration, are imperative reactive
  widgets (for example, the [`Chip`][] class).

**響應式程式設計 (reactive programming)** 
<br> Widget 和元素樹有時候被描述為響應式的，
  因為隨 widget 構造方法引入的新輸入會隨著其 build 方法
  傳播給更低等級的 widget；
  而底層 widget 中出現的修改 (如響應使用者的輸入) 也會
  沿著結構樹透過 event handler 向上傳播。
  在整個框架中，函式-響應式以及指令-響應式的實現都有出現，
  具體取決於 widget 的功能需求。
  Widget 的 build 方法如果只是包含其針對變化如何響應的表示式的話，
  就是函式-響應式 widget (如 Material [`Divider`][] 類)。
  如果 widget 的 build 方法包含一系列構造子元素的表示式，
  用於描述該 widget 如何響應變化的話，
  那它就是指令響應式 widget (如 [`Chip`][] 類)。

**Declarative programming**
<br> The build methods of widgets are often a single
  expression with multiple levels of nested constructors,
  written using a strictly declarative subset of Dart.
  Such nested expressions could be mechanically transformed
  to or from any suitably expressive markup language.
  For example, the [`UserAccountsDrawerHeader`][]
  widget has a long build method (20+ lines),
  consisting of a single nested expression.
  This can also be combined with the imperative style to build UIs
  that would be harder to describe in a pure-declarative approach.
  
**宣告式程式設計 (declarative programming)** 
<br> Widget 的 build 方法通常都是一個單一表達式，
  它包含多級巢狀(Nesting)的建構函式，且使用 Dart 嚴格的宣告式子集編寫。
  這些巢狀(Nesting)的表示式可以與合適的標記語言互相轉換。
  比如，[`UserAccountsDrawerHeader`][] 這個 widget
  就有一個很長的 build 方法 (20 多行)，由一個巢狀(Nesting)的表示式構成。
  這種正規化也可以和指令式混合使用，以實現某些很難用純宣告式的方法實現的 UI。

**Generic programming**
<br> Types can be used to help developers catch programming
  errors early. The Flutter framework uses generic programming to
  help in this regard. For example, the [`State`][]
  class is parameterized in terms of the type of its
  associated widget, so that the Dart analyzer can catch
  mismatches of states and widgets. Similarly, the
  [`GlobalKey`][] class takes a type parameter so that it
  can access a remote widget's state in a type-safe manner
  (using runtime checking), the [`Route`][] interface is
  parameterized with the type that it is expected to use when
  [popped][], and collections such as [`List`][]s, [`Map`][]s,
  and [`Set`][]s are all parameterized so that mismatched
  elements can be caught early either during analysis or at
  runtime during debugging.

**泛型程式設計 (generic programming)** 
<br> 型別可以幫助開發者更早地抓到錯誤，
  基於這一點，Flutter 框架也採用了泛型開發。
  比如，[`State`][] 類就是如此，其關聯的 widget 就是類別型引數，
  如此一來 Dart 分析器就能捕獲到 state 和 widget 不匹配的情況。
  類似的，[`GlobalKey`][] 類就接受一個類別型引數，
  從而型別安全地存取一個 widget 的 state (會使用執行時期檢查)。
  [`Route`][] 介面也在被 [popped][] 時接受型別引數，
  另外 [`List`][], [`Map`][], [`Set`][] 這些集合也都如此，
  這樣就可以在分析或者執行時儘早發現型別不匹配的錯誤。

**Concurrent programming**
<br> Flutter makes heavy use of [`Future`][]s and other
  asynchronous APIs. For example, the animation system reports
  when an animation is finished by completing a future.
  The image loading system similarly uses futures to report
  when a load is complete.

**併發 (concurrent programming)** 
<br> Flutter 大量使用諸如 [`Future`][] 等非同步 API。
  比如，動畫系統就會在動畫執行完 future 時進行事件告知。
  同樣的，圖片載入系統也會使用 future 在載入完畢時進行告知。

**Constraint programming**
<br> The layout system in Flutter uses a weak form of
  constraint programming to determine the geometry of a scene.
  Constraints (for example, for cartesian boxes, a minimum and
  maximum width and a minimum and maximum height)
  are passed from parent to child, and the child selects a resulting
  geometry (for example, for cartesian boxes, a size,
  specifically a width and a height) that fulfills those constraints.
  By using this technique, Flutter can usually
  lay out an entire scene with a single pass.

**約束程式設計 (constraint programming)**
<br>Flutter 的佈局系統使用了約束程式設計的簡化形態來描述一個場景的幾何性質。
  約束值 (比如一個笛卡爾矩形允許的最大 / 最小寬高值) 會從父元素傳遞給子元素，
  子元素最終選擇一個能滿足上面所有約束條件的最終尺寸。
  這種做法也使得 Flutter 能不依賴太多輸入的情況下快速完成一個全新的佈局。


[`AnimatedDefaultTextStyle`]: {{site.api}}/flutter/widgets/AnimatedDefaultTextStyle-class.html
[`AnimatedPhysicalModel`]: {{site.api}}/flutter/widgets/AnimatedPhysicalModel-class.html
[`Center`]: {{site.api}}/flutter/widgets/Center-class.html
[`Chip`]: {{site.api}}/flutter/material/Chip-class.html
[`color`]: {{site.api}}/flutter/widgets/Icon/color.html
[`ConstrainedBox`]: {{site.api}}/flutter/widgets/ConstrainedBox-class.html
[`Divider`]: {{site.api}}/flutter/material/Divider-class.html
[`Future`]: {{site.api}}/flutter/dart-async/Future-class.html
[`GestureDetector`]: {{site.api}}/flutter/widgets/GestureDetector-class.html
[`GlobalKey`]: {{site.api}}/flutter/widgets/GlobalKey-class.html
[`icon`]: {{site.api}}/flutter/widgets/Icon/icon.html
[`Icon`]: {{site.api}}/flutter/widgets/Icon-class.html
[`IconTheme`]: {{site.api}}/flutter/widgets/IconTheme-class.html
[`InkWell`]: {{site.api}}/flutter/material/InkWell-class.html
[`Iterable`]: {{site.api}}/flutter/dart-core/Iterable-class.html
[`List`]: {{site.api}}/flutter/dart-core/List-class.html
[`Listenable`]: {{site.api}}/flutter/foundation/Listenable-class.html
[`Map`]: {{site.api}}/flutter/dart-core/Map-class.html
[`Material`]: {{site.api}}/flutter/material/Material-class.html
[`NotificationListener`]: {{site.api}}/flutter/widgets/NotificationListener-class.html
[`Padding`]: {{site.api}}/flutter/widgets/Padding-class.html
[popped]: {{site.api}}/flutter/widgets/Navigator/pop.html
[`Rect`]: {{site.api}}/flutter/dart-ui/Rect-class.html
[`RenderBox`]: {{site.api}}/flutter/rendering/RenderBox-class.html
[`RenderObject`]: {{site.api}}/flutter/rendering/RenderObject-class.html
[`Route`]: {{site.api}}/flutter/widgets/Route-class.html
[`ScrollPhysics`]: {{site.api}}/flutter/widgets/ScrollPhysics-class.html
[`Set`]: {{site.api}}/flutter/dart-core/Set-class.html
[`size`]: {{site.api}}/flutter/widgets/Icon/size.html
[`State`]: {{site.api}}/flutter/widgets/State-class.html
[`StatelessWidget`]: {{site.api}}/flutter/widgets/StatelessWidget-class.html
[`TextButton`]: {{site.api}}/flutter/material/TextButton-class.html
[`TextStyle`]: {{site.api}}/flutter/painting/TextStyle-class.html
[`UserAccountsDrawerHeader`]: {{site.api}}/flutter/material/UserAccountsDrawerHeader-class.html
[`Widget`]: {{site.api}}/flutter/widgets/Widget-class.html

## Project

## 工程管理

### Where can I get support?

### 我該如何獲得技術支援？

If you think you've encountered a bug, file it in our
[issue tracker][]. You might also use
[Stack Overflow][] for "HOWTO" type questions.
For discussions, join our mailing list at
[{{site.email}}][] or seek us out on [Discord][].

For more information, see our [Community][] page.

如果您覺得遇到 bug 了，請提交至我們的 [問題追蹤入口][issue tracker]。
我們也鼓勵您在 [Stack Overflow][] 中多多使用
“如何 (how to) ...“來搜尋解答。
如果您希望直接與我們溝通，
請使用我們的官方郵件地址 [{{site.email}}][]
或在 [Discord][] 上向我們提問。 


[Community]: {{site.main-url}}/community
[Discord]: https://discord.gg/N7Yshp4
[issue tracker]: {{site.repo.flutter}}/issues
[{{site.email}}]: mailto:{{site.email}}
[Stack Overflow]: {{site.so}}/tags/flutter

### How do I get involved?

### 我如何投入到 Flutter 開發社群？

Flutter is open source, and we encourage you to contribute.
You can start by simply filing issues for feature requests
and bugs in our [issue tracker][].

Flutter 是開源的，我們鼓勵您為此做出自己的貢獻。
您可以透過 [問題追蹤入口][issue tracker] 來提交功能需求或者 bug 報告。

We recommend that you join our mailing list at
[{{site.email}}][] and let us know how you're
using Flutter and what you'd like to do with it.

我們也希望您加入我們的郵件討論 [{{site.email}}][]，
告訴我們您是如何使用 Flutter 的，以及打算用 Flutter 開發什麼。

If you're interested in contributing code, you can start
by reading our [Contributing guide][], and check out our
list of [easy starter issues][].

如果您打算為 Flutter 貢獻程式碼，請先閱讀 [程式碼貢獻指南][Contributing guide]，
然後從 [簡單待修復問題][easy starter issues] 列表中尋找力所能及的問題開始入手。

Finally, you can connect with helpful Flutter communities.
For more information, see the [Community][] page.

最後，您可以與各個 Flutter 社群保持聯絡，
更多相關資訊，請查閱我們的 [社群][Community] 頁面。

[Contributing guide]: {{site.repo.flutter}}/blob/master/CONTRIBUTING.md
[easy starter issues]: {{site.repo.flutter}}/issues?q=is%3Aopen+is%3Aissue+label%3A%22easy+fix%22

### Is Flutter open source?

### Flutter 是開源的嗎？

Yes, Flutter is open source technology.
You can find the project on [GitHub][].

是的，Flutter 是開源的。您可以在 [GitHub][] 上獲取到它。

[GitHub]: {{site.repo.flutter}}

### Which software license(s) apply to Flutter and its dependencies?

### Flutter 以及其依存專案使用的是哪種軟體許可協議？

Flutter includes two components: an engine that ships as a
dynamically linked binary, and the Dart framework as a separate
binary that the engine loads. The engine uses multiple software
components with many dependencies; view the complete list
in its [license file][].

Flutter 包含兩個部分：
一個使用動態連結二進位制檔案發行的引擎，以及引擎載入的 Dart 框架二進位制檔案。
引擎使用了很多軟體元件，且包含許多依存內容。
完整的說明和依存清單請檢視引擎的 [許可協議][license file]。

The framework is entirely self-contained and requires
[only one license][].

框架部分則自成一體，且 [只有一份簡單的許可協議][only one license]。

In addition, any Dart packages you use might have their
own license requirements.

另外，您使用的其他 Dart 程式碼包可能有其獨有的許可協議。

[license file]: https://raw.githubusercontent.com/flutter/engine/master/sky/packages/sky_engine/LICENSE
[only one license]: {{site.repo.flutter}}/blob/master/LICENSE

### How can I determine the licenses my Flutter application needs to show?

### 我如何確定我的 Flutter 應用該顯示哪些許可協議？

There's an API to find the list of licenses you need to show:

您可以使用 API 來確定需要顯示的許可協議。

* If your application has a [`Drawer`][], add an
  [`AboutListTile`][].

  如果您的應用使用了 [`Drawer`][]，則新增一個 [`AboutListTile`][]。

* If your application doesn't have a Drawer but does use the
  Material Components library, call either [`showAboutDialog`][]
  or [`showLicensePage`][].

  如果您的應用不包含 Drawer 但使用了 Material 元件庫，
  請呼叫 [`showAboutDialog`][] 或者 [`showLicensePage`][]。

* For a more custom approach, you can get the raw licenses from the
  [`LicenseRegistry`][].

  對於更加客製的場合，您可以使用
  [`LicenseRegistry`][] 獲得原始的許可內容。


[`AboutListTile`]: {{site.api}}/flutter/material/AboutListTile-class.html
[`Drawer`]: {{site.api}}/flutter/material/Drawer-class.html
[`LicenseRegistry`]: {{site.api}}/flutter/foundation/LicenseRegistry-class.html
[`showAboutDialog`]: {{site.api}}/flutter/material/showAboutDialog.html
[`showLicensePage`]: {{site.api}}/flutter/material/showLicensePage.html

### Who works on Flutter?

### 目前有哪些人在開發 Flutter？

We all do! Flutter is an open source project.
Currently, the bulk of the development is done
by engineers at Google. If you're excited about Flutter,
we encourage you to join the community and
[contribute to Flutter][]!

我們都在參與 Flutter 開發！
我們都知道 Flutter 是一個開源專案。
目前 Flutter 中的大部分都是由 Google 的工程師來開發。
如果您喜歡 Flutter 的話，我們希望您加入開發者社群並
[做出貢獻][contribute to Flutter]！

[contribute to Flutter]: {{site.repo.flutter}}/blob/master/CONTRIBUTING.md

### What are Flutter's guiding principles?

### Flutter 有哪些指導原則？

We believe the following:

我們相信：

* In order to reach every potential user,
  developers need to target multiple mobile platforms.

  為了觸達每一位潛在使用者，開發者需要針對多個行動平台釋出自己的應用。

* HTML and WebViews as they exist today make it challenging to
  consistently hit high frame rates and deliver
  high-fidelity experiences, due to automatic behavior (scrolling,
  layout) and legacy support.

  目前常用的 HTML 和 WebView 由於一些預設的互動響應
  （滾動、佈局等）以及向後相容等問題，很難獲得穩定的高幀率和精確的設計體驗。

* Today, it's too costly to build the same app multiple times: it
  requires different teams, different code bases,
  different workflows, different tools, etc.

  目前，開發同一個應用的不同平臺版本成本很高：
  這意味著不同的團隊、不同的程式碼庫、不同的工作流程以及不同的工具，等等。

* Developers want an easier, better way to use a single codebase to
  build mobile apps for multiple target platforms,
  and they don't want to sacrifice quality, control, or performance.

  開發者需要一個簡單的、更好的方法來使用同一套程式碼庫開發應用的不同平臺版本。
  而且他們不希望在品質、細節和功能控制以及效能上有任何妥協。

We are focused on three things:

我們目前集中於以下三件事：

_Control_
<br> Developers deserve access to, and control over,
  all layers of the system. Which leads to:

**功能控制**
<br> 開發者應該能存取到系統所有層級的功能，且能獲得全面的控制權。這也意味著：

_Performance_ 
<br> Users deserve perfectly fluid, responsive,
  jank-free apps. Which leads to:

**效能表現**
<br> 使用者應該獲得流暢、響應迅捷且沒有卡頓的應用。這也意味著：

_Fidelity_
<br> Everyone deserves precise, beautiful, delightful
  app experiences.

**精確實現**
<br> 每一個人都應該獲得精確、優美且富有表現力的移動應用體驗。

### Will Apple reject my Flutter app?

### Apple 會拒絕我的 Flutter 應用嗎？

We can't speak for Apple, but their App Store contains
many apps built with framework technologies such as Flutter.
Indeed, Flutter uses the same fundamental architectural
model as Unity, the engine that powers many of the
most popular games on the Apple store.

我們無法代 Apple 發言，但已經有很多使用類似 Flutter 的其他技術開發的應用。
實際上，Flutter 與 Unity 使用了近乎一致的底層架構模型，
Apple store 中最著名的遊戲也是使用它的引擎開發的。

Apple has frequently featured well-designed apps
that are built with Flutter,
including [Hamilton][Hamilton for iOS] and [Reflectly][].

Apple 最近評選的最佳設計應用也是使用 Flutter 開發的，
其中包括 [Hamilton][Hamilton for iOS] 和 [Reflectly][]。

As with any app submitted to the Apple store,
apps built with Flutter should follow Apple's
[guidelines][] for App Store submission.

任何提交到 Apple store 的 Flutter 應用都應該遵守
Apple 的 [規範][guidelines]。

[guidelines]: {{site.apple-dev}}/app-store/review/guidelines/
[Hamilton for iOS]: https://itunes.apple.com/us/app/hamilton-the-official-app/id1255231054?mt=8
[Reflectly]: https://apps.apple.com/us/app/reflectly-journal-ai-diary/id1241229134
