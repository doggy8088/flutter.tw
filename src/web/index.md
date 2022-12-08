---
title: Web support for Flutter
title: Flutter 正式支援 Web 平台
description: "Details of how Flutter supports the creation of web experiences."
description: "有關 Flutter 如何支援建立 Web 體驗的詳細資訊。"
tags: 平台
keywords: Flutter web, web跨端
---

Flutter’s web support delivers the same experiences on the web as on mobile.
Building on the portability of Dart, the power of the web platform and the
flexibility of the Flutter framework, you can now build apps for iOS, Android,
and the browser from the same codebase. You can compile existing Flutter code
written in Dart into a web experience because it is exactly the same Flutter
framework and **web** is just another device target for your app.

<img src="/assets/images/docs/arch-overview/web-arch.png"
     alt="Flutter architecture for web"
     width="100%">

Adding web support to Flutter involved implementing Flutter's
core drawing layer on top of standard browser APIs, in addition
to compiling Dart to JavaScript, instead of the ARM machine code that
is used for mobile applications. Using a combination of DOM, Canvas, 
and WebAssembly, Flutter can provide a portable, high-quality,
and performant user experience across modern browsers.
We implemented the core drawing layer completely in Dart
and used Dart's optimized JavaScript compiler to compile the
Flutter core and framework along with your application
into a single, minified source file that can be deployed to
any web server.

向 Flutter 新增 Web 支援涉及在標準瀏覽器 API
之上實現 Flutter 的核心繪圖層。
透過結合使用 DOM、Canvas 和 CSS，Web 支援旨在為現代瀏覽器提供
便攜、高品質和高效能的使用者體驗。
我們在 Dart 中完全實現了這個核心繪圖層，
並使用 Dart 的最佳化過的 JavaScript 編譯器將 Flutter 核心與框架，
同你的應用程式一起編譯成一個可以部署到任何 Web 伺服器的簡化原始檔。

While you can do a lot on the web,
Flutter’s web support is most valuable in the
following scenarios:

**A [Progressive Web Application][] built with Flutter**
: Flutter delivers high-quality PWAs that are integrated with a user’s
  environment, including installation, offline support, and tailored UX.

**Single Page Application**
: Flutter’s web support enables complex standalone web apps that are rich with
  graphics and interactive content to reach end users on a wide variety of
  devices.

**Existing mobile applications**
: Web support for Flutter provides a browser-based delivery model for existing
  Flutter mobile apps.

Not every HTML scenario is ideally suited for Flutter at this time. 
For example, text-rich, flow-based, static content such as blog articles
benefit from the document-centric model that the web is built around,
rather than the app-centric services that a UI framework like Flutter
can deliver. However, you _can_ use Flutter to embed interactive
experiences into these websites.

For a glimpse into how to migrate your mobile app to web, see
the following video:

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=246950294&bvid=BV1Jv411h7x6&cid=305745348&page=1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<a name="web"></a>
## Resources

## 關於 web 支援的說明

The following resources can help you get started:

以下資源可以幫助你入門：

* To add web support to an existing app, or to create a
  new app that includes web support, see
  [Building a web application with Flutter][].
  
  要向現有應用新增Web支援，或建立一個包含 Web 支援的新應用，
  請參閱 [使用 Flutter 建構 Web 應用][Building a web application with Flutter]。
  
* To learn about Flutter's different web renderers (HTML and CanvasKit), see 
  [Web renderers][]

  瞭解更多關於 Flutter web 渲染器 (HTML and CanvasKit) 的不同之處，請查閱[網頁渲染器][Web renderers]。

* To learn how to create a responsive Flutter
  app, see [Creating responsive apps][].
  
  想了解如何建立響應式 Flutter 應用，請參閱 [建立響應式應用][Creating responsive apps]。
  
* To view commonly asked questions and answers, see the
  [web FAQ][].
  
  要瀏覽常見問題和答案，請參閱 [Web 常見問題][web FAQ]。
  
* To see code examples,
  check out the [web samples for Flutter][].

  要檢視程式碼例項，請檢視
  [Web 平台範例程式碼][web samples for Flutter]。

* To see a Flutter web app demo, check out the [Flutter Gallery][].

  檢視 Flutter web 應用的範例，可以試試看 [Flutter Gallery][]。

* To learn about deploying a web app, see
  [Preparing an app for web release][].
  
  要了解關於釋出 Web 應用的資訊，請參閱 [Preparing an app for web release][]。
  
* [File an issue][] on the main Flutter repo.

  請從 [File an issue][] 向 Flutter 主儲存庫提一個 issue。

* You can chat and ask web-related questions on the
  **#help** channel on [Discord][].

  你可以在 [Discord][] 的 **#help** 頻道諮詢 web 相關的問題。

---


[Building a web application with Flutter]: {{site.url}}/get-started/web
[Creating responsive apps]: {{site.url}}/development/ui/layout/adaptive-responsive
[Discord]: https://discordapp.com/invite/yeZ6s7k
[file an issue]: https://goo.gle/flutter_web_issue
[Flutter Gallery]: {{site.gallery}}
[main Flutter repo]: {{site.repo.flutter}}
[Preparing an app for web release]: {{site.url}}/deployment/web
[Progressive Web Application]: https://web.dev/progressive-web-apps/
[web FAQ]: {{site.url}}/development/platform-integration/web
[web samples for Flutter]: https://flutter.github.io/samples/#?platform=web
[Web renderers]: {{site.url}}/development/tools/web-renderers
