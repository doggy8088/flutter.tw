---
title: Web FAQ
title: Web 常見問題
description: Some gotchas and differences when writing or running web apps in Flutter.
description: 在 Flutter 中編寫或執行 Web 應用程式時遇到的一些問題，以及 Web 與不同之處。
tags: 平台整合
keywords: Flutter網頁版,常見問題
---

### What scenarios are ideal for Flutter on the web?

### 在 Web 平台使用 Flutter 的場景有哪些？

Not every web page makes sense in Flutter, but we think Flutter is particularly
suited for app-centric experiences:

Flutter 目前並非適用於所有的網頁內容，不過我們主要關注三個應用場景：

* Progressive Web Apps

  漸進式 web 應用 (Progressive web apps, PWA)，兼具 web 的高覆蓋面與桌面應用的強大功能。

* Single Page Apps

  單頁應用 (Single page apps, SPA)，只需一次載入，並與網際網路服務動態互傳資料。

* Existing Flutter mobile apps

  將現有 Flutter 移動應用拓展到 web，在兩個平台共享程式碼。

At this time, Flutter is not suitable for static websites with text-rich
flow-based content. For example, blog articles benefit from the document-centric
model that the web is built around, rather than the app-centric services that a
UI framework like Flutter can deliver. However, you _can_ use Flutter to embed
interactive experiences into these websites.

現在階段，Flutter 不適合具有豐富文字和瀑布流的頁面。
例如，部落格文章等基於流媒體的豐富文字內容，
其受益於網路建構的以文件為中心的模式，
而不是像 Flutter 這樣的 UI 框架可以提供的以應用為中心的服務。
然而，你可以使用 Flutter 將互動式體驗嵌入到這些網站中。

For more information on how you can use Flutter on the web,
see [Web support for Flutter][].

有關如何在 Web 上使用 Flutter 的更多資訊，參考文件：
[Flutter 的 Web 支援][Web support for Flutter]。

### Search Engine Optimization (SEO)

### Flutter Web 應用的 SEO 最佳化

In general, Flutter is geared towards dynamic application experiences. Flutter's
web support is no exception. Flutter web prioritizes performance, fidelity, and
consistency. This means application output does not align with what search
engines need to properly index. For web content that is static or document-like,
we recommend using HTML—just like we do on [flutter.dev]({{site.main-url}}),
[dart.dev]({{site.dart-site}}), and [pub.dev]({{site.pub}}). You should also
consider separating your primary application experience—created in Flutter—from
your landing page, marketing content, and help content—created using
search-engine optimized HTML.

一般情況下，Flutter Web 的目標是建構「動態化」網頁應用。
Flutter 的 Web 端支援會優先考慮和確保效能、保真度和一致性。
這意味著產生的網頁頁面可能不是搜尋引擎「熟悉」的結構化頁面。
對於一些網頁、文件內容，我們建議你使用 HTML 建構，
就像我們為 [Flutter]({{site.main-url}}) 和
[Dart]({{site.dart-site}}) 官網所做的那樣。
你還應該考慮將主要的類應用體驗（使用 Flutter 建構的 Web 網頁）
與首頁、營銷內容以及幫助內容等（使用搜索引擎「熟悉」的 HTML 建構）
進行分離，避免將它們混在一起。

### How do I create an app that also runs on the web?

### 如何建立同時在 Web 上執行的應用？

See [building a web app with Flutter][].

請參見 [使用 Flutter 建構 Web 應用][building a web app with Flutter]。

### Does hot reload work with a web app?

### 我該如何在瀏覽器中重新整理正在執行的應用？

No, but you can use hot restart. Hot restart is a fast way of seeing your
changes without having to relaunch your web app and wait for it to compile and
load. This works similarly to the hot reload feature for Flutter mobile
development. The only difference is that hot reload remembers your state and hot
restart doesn't.

不能，但是可以使用熱重啟 (hot restart)。
熱重啟是可以您的應用快速響應改動的方法，無需等待重新編譯的載入。
它與移動端的熱重載功能類似。唯一的區別是熱重載可以保持應用的狀態。

### How do I restart the app running in the browser?

### 我該如何在瀏覽器中重啟正在執行的應用？

Using the browser's refresh button doesn't work,
but you can enter "R" in the console where
"flutter run -d chrome" is running.

使用瀏覽器的重新整理按鈕不會起作用，
但你可以在執行 `flutter run -d chrome` 的控制檯中
輸入「R」進行重新整理。

### Which web browsers are supported by Flutter?

### 現在有哪些瀏覽器支援 Flutter 了？

Flutter web apps can run on the following browsers:

現在 Flutter web 應用可以執行在以下瀏覽器中：

* Chrome (mobile & desktop)

  Chrome（移動和桌面端）

* Safari (mobile & desktop)

  Safari（移動和桌面端）

* Edge (mobile & desktop)

  Edge（移動和桌面端）

* Firefox (mobile & desktop)

  Firefox（移動和桌面端）

During development, Chrome (on macOS, Windows, and Linux) and Edge (on Windows)
are supported as the default browsers for debugging your app.

在開發階段，Chrome（在 macOS、Windows 以及 Linux）以及
Edge（在 Windows 上）將作為預設瀏覽器用於除錯。

### Can I build, run, and deploy web apps in any of the IDEs?

### 我可以在任意 IDE 中，建構、執行併發布 web 應用嗎？

You can select **Chrome** or **Edge** as the target device in
Android Studio/IntelliJ and VS Code.

你可以在 Android Studio/IntelliJ 和 VS Code 裡選擇使用
**Chrome** 或者 **Edge** 瀏覽器。

The device pulldown should now include the **Chrome (web)**
option for all channels.

裝置下拉列表裡現在應該在所有平台裡都包含了 Chrome (web)。


### How do I build a responsive app for the web?

### 我該如何建構響應式 web 應用？

See [Creating responsive apps][].

請參閱 [建立響應式應用][Creating responsive apps]。

### Can I use `dart:io` with a web app?

### 我能在 Web 應用中使用 `dart:io` 這個 package 嗎？

No. The file system is not accessible from the browser.
For network functionality, use the [`http`][]
package. Note that security works somewhat
differently because the browser (and not the app)
controls the headers on an HTTP request.

不行。檔案系統在瀏覽器中是無法存取的。
對於網路功能來說，請使用 [`http`][] package。
請注意，安全方面的工作有所不同，
因為瀏覽器（而不是應用程式）控制 HTTP 請求上的標頭。

### How do I handle web-specific imports?

### 我應該如何處理一個 Web 平台特定的匯入？

Some plugins require platform-specific imports, particularly if they use the
file system, which is not accessible from the browser. To use these plugins
in your app, see the [documentation for conditional imports][]
on [dart.dev]({{site.dart-site}}).

部分外掛需要在特定平台匯入庫或者檔案，尤其是當使用瀏覽器無法存取的檔案系統時。
若要在你的應用裡使用這些外掛，請參閱 [dart.cn]({{site.dart-site}})：
[選擇性的匯入][documentation for conditional imports]。

### Does Flutter web support concurrency?

### Flutter Web 是否支援併發？

Dart's concurrency support via [isolates][]
is not currently supported in Flutter web.

Dart 透過 [isolates][] 機制實現併發，
目前在 Web 中尚未支援。

Flutter web apps can potentially work around this
by using [web workers][],
although no such support is built in.

Flutter Web 沒有內建併發的支援，
但你可以嘗試透過 [web workers][] 來解決這個問題。

### How do I embed a Flutter web app in a web page?

### 我該如何把一個 Flutter web 應用嵌入到一個網頁中？

You can embed a Flutter web app,
as you would embed other content,
in an [`iframe`][] tag of an HTML file.
In the following example, replace "URL"
with the location of your hosted HTML page:

你可以透過下面這個例子，以 [`iframe`][] 來內嵌，
把 URL 替換成託管 Flutter Web 的頁面 URL：

```html
<iframe src="URL"></iframe>
```

If you encounter problems, please [file an issue][].

如果你遇到問題，請 [提交一個 issue][file an issue] 給我們。

### How do I debug a web app?

### 我該如何除錯一個 web 應用？

Use [Flutter DevTools][] for the following tasks:

使用 [Flutter DevTools][] 來嘗試如下工作：

* [Debugging][]

  [除錯][Debugging]

* [Logging][]

  [檢視日誌][Logging]

* [Running Flutter inspector][]

  [執行 Flutter inspector][Running Flutter inspector]

Use [Chrome DevTools][] for the following tasks:

使用 [Chrome DevTools][] 來嘗試如下工作：

* [Generating event timeline][]

  [產生事件的時間線][Generating event timeline]

* [Analyzing performance][]&mdash;make sure to use a
  profile build

  [分析效能][Analyzing performance]&mdash;&mdash;請確保應用使用的是 profile 建構

### How do I test a web app?

### 我該如何測試 Web 應用？

Use [widget tests][] or integration tests. To learn more about
running integration tests in a browser, see the [Integration testing][] page.

使用常規的 [widget tests][]，瞭解更多關於如何在瀏覽器裡使用整合測試，請檢視
[整合測試][Integration testing] 文件頁面。

### How do I deploy a web app?

### 我該如何部署 Web 應用？

See [Preparing a web app for release][].

請參閱 [打包併發布到 Web 平台][Preparing a web app for release]。

### Does `Platform.is` work on the web?

### `Platform.is` API 現在可用嗎？

Not currently.

目前還不行。

[Analyzing performance]: {{site.developers}}/web/tools/chrome-devtools/evaluate-performance
[building a web app with Flutter]: {{site.url}}/platform-integration/web/building
[Chrome DevTools]: {{site.developers}}/web/tools/chrome-devtools
[Creating responsive apps]: {{site.url}}/ui/layout/responsive/adaptive-responsive
[Debugging]: {{site.url}}/tools/devtools/debugger
[file an issue]: {{site.repo.flutter}}/issues/new?title=[web]:+%3Cdescribe+issue+here%3E&labels=%E2%98%B8+platform-web&body=Describe+your+issue+and+include+the+command+you%27re+running,+flutter_web%20version,+browser+version
[Flutter DevTools]: {{site.url}}/tools/devtools/overview
[Generating event timeline]: {{site.developers}}/web/tools/chrome-devtools/evaluate-performance/performance-reference
[`http`]: {{site.pub}}/packages/http
[`iframe`]: https://html.com/tags/iframe/
[isolates]: {{site.dart-site}}/guides/language/concurrency
[Issue 32248]: {{site.repo.flutter}}/issues/32248
[Logging]: {{site.url}}/tools/devtools/logging
[Preparing a web app for release]: {{site.url}}/deployment/web
[Running Flutter inspector]: {{site.url}}/tools/devtools/inspector
[Upgrading from package:flutter_web to the Flutter SDK]: {{site.repo.flutter}}/wiki/Upgrading-from-package:flutter_web-to-the-Flutter-SDK
[widget tests]: {{site.url}}/testing/overview#widget-tests
[Web support for Flutter]: {{site.url}}/platform-integration/web
[web workers]: https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers
[run your web apps in any supported browser]: {{site.url}}/platform-integration/web/building#create-and-run
[Integration testing]: {{site.url}}/testing/integration-tests#running-in-a-browser
[documentation for conditional imports]: {{site.dart-site}}/guides/libraries/create-library-packages#conditionally-importing-and-exporting-library-files
