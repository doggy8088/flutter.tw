---
title: Build and release a web app
title: 建構和釋出為 Web 應用
description: How to prepare for and release a web app.
description: 如何打包併發布到 Web 平台。
short-title: Web
tags: 釋出, Web
keywords: 釋出Flutter應用為Web應用
---

During a typical development cycle,
you test an app using `flutter run -d chrome`
(for example) at the command line.
This builds a _debug_ version of your app.

（例如）在典型的開發過程中，
你可以在命令列使用 `flutter run -d chrome` 命令測試應用程式。
這會構建出 **debug** 版本的應用。

This page helps you prepare a _release_ version
of your app and covers the following topics:

本頁面會幫助你建構 **release** 版本的應用，其囊括瞭如下主題：

* [Handling images on the web](#handling-images-on-the-web)

  [處理 Web 中的圖片](#handling-images-on-the-web)

* [Choosing a web renderer](#choosing-a-web-renderer)

  [選擇 Web 渲染器](#choosing-a-web-renderer)

* [Minification](#minification)

  [壓縮](#minification)

* [Building the app for release](#building-the-app-for-release)

  [建構用於釋出的應用](#building-the-app-for-release)

* [Deploying to the web](#deploying-to-the-web)

  [釋出到 Web 上](#deploying-to-the-web)

## Handling images on the web

## 處理 Web 中的圖片

The web supports the standard `Image` widget to display images.
However, because web browsers are built to run untrusted code safely,
there are certain limitations in what you can do with images compared
to mobile and desktop platforms.

Web 支援標準的 `Image` widget 來顯示圖片。 
但是，由於 Web 瀏覽器需要安全地執行不受信任的程式碼，
因此與移動和桌面平台相比，影象處理方面存在某些限制。

For more information, see [Displaying images on the web][].

更多資訊，請參閱 [在 Web 中展示圖片][Displaying images on the web].

## Choosing a web renderer

## 選擇 Web 渲染器

By default, the `flutter build` and `flutter run` commands
use the `auto` choice for the web renderer. This means that
your app runs with the HTML renderer on mobile browsers and
CanvasKit on desktop browsers. This is our recommended combination
to optimize for the characteristics of each platform.

預設情況下，`flutter build` 和 `flutter run` 命令對 Web 渲染器使用 `auto` 引數。 
這意味著您的應用程式在移動瀏覽器上會與 HTML 渲染器一起執行，
而在桌面瀏覽器上與 CanvasKit 一起執行。
這是我們推薦的組合方式，能夠針對每個平台特性最佳化。

For more information, see [Web renderers][].

更多資訊，請參閱 [Web 渲染器][Web renderers].

## Minification

## 混淆並壓縮程式碼

Minification is handled for you when you
create a release build.

當你建立了一個 release 版本時，便已經壓縮了程式碼。

A debug build of a web app is not minified and
tree shaking has not been performed.

Debug 模式建構的 Web 應用沒有被壓縮，且 Tree-shaking 沒有執行。

A profile build is not minified and tree shaking
has been performed.

Profile 模式建構的 Web 應用沒有被壓縮，但 Tree-shaking 執行了。

A release build is both minified and tree shaking
has been performed.

Release 模式建構的 Web 應用被壓縮了，並且 Tree-shaking 執行了。

## Building the app for release

## 建構用於釋出的應用

Build the app for deployment using the
`flutter build web` command.
You can also choose which renderer to use
by using the `--web-renderer` option (See [Web renderers][]).
This generates the app, including the assets,
and places the files into the `/build/web`
directory of the project.

使用 `flutter build web` 命令建構應用程式以進行部署。
你也可以透過使用 `--web-renderer` 自行選擇渲染方式。（請檢視 [網頁渲染器][Web renderers]）
這將產生包括資源的應用程式，並將檔案放入專案的 `/build/web` 目錄中。

The release build of a simple app has the
following structure:

一般的應用程式的 release 版本具有以下結構：

```none
/build/web
  assets
    AssetManifest.json
    FontManifest.json
    NOTICES
    fonts
      MaterialIcons-Regular.ttf
      <other font files>
    <image files>
  index.html
  main.dart.js
  main.dart.js.map
```

Launch a web server (for example,
`python -m http.server 8000`,
or by using the [dhttpd][] package),
and open the /build/web directory. Navigate to
`localhost:8000` in your browser
(given the python SimpleHTTPServer example)
to view the release version of your app.

啟動 Web 伺服器（例如，`python -m SimpleHTTPServer 8000`，或使用
[dhttpd][] package），然後開啟 /build/web 目錄。
在瀏覽器中存取 `localhost:8000`（前文用 Python 啟動的伺服器）
以檢視應用程式的 release 版本。

## Embedding a Flutter app into an HTML page

## 將 Flutter 應用內嵌到一個 HTML 頁面裡

You can embed a Flutter web app,
as you would embed other content,
in an [`iframe`][] tag of an HTML file.
In the following example, replace "URL"
with the location of your HTML page:

你可以使用 [`iframe`][] 標籤將 Flutter web 應用
內嵌到一個網頁裡。
請參照下面的例子，將 URL 替換成實際的地址：

```html
<iframe src="URL"></iframe>
```

## Deploying to the web

## 部署到 Web 端

When you are ready to deploy your app,
upload the release bundle
to Firebase, the cloud, or a similar service.
Here are a few possibilities, but there are
many others:

等你準備好部署應用時，將 release 包上傳到 Firebase、雲或者是類似服務上：

* [Firebase Hosting][]
* [GitHub Pages][]
* [Google Cloud Hosting][]

## PWA Support

As of release 1.20, the Flutter template for web apps includes support
for the core features needed for an installable, offline-capable PWA app.
Flutter-based PWAs can be installed in the same way as any other web-based
PWA; the settings signaling that your Flutter app is a PWA are provided by
`manifest.json`, which is produced by `flutter create` in the `web` directory.

從 1.20 版開始，用於 Web 應用程式的 Flutter 範本包括了對可安裝且
具有離線功能的 PWA 應用程式所需的核心功能的支援。 
基於 Flutter 的 PWA 的安裝方式與其他基於 Web 的 PWA 基本相同；
由 `manifest.json` 提供的配置資訊可以宣告您的 Flutter 應用程式是 PWA，
該檔案可以在 `web` 目錄中使用 `Flutter create` 命令產生。

PWA support remains a work in progress,
so please [give us feedback][] if you see something that doesn’t look right.

對 PWA 的支援仍在進行中，因此，如果您發現不正確的地方，
歡迎 [給予我們反饋][give us feedback]。

[dhttpd]: {{site.pub}}/packages/dhttpd
[Displaying images on the web]: {{site.url}}/development/platform-integration/web-images
[Firebase Hosting]: {{site.firebase}}/docs/hosting
[GitHub Pages]: https://pages.github.com/
[give us feedback]: {{site.repo.flutter}}/issues/new?title=%5Bweb%5D:+%3Cdescribe+issue+here%3E&labels=%E2%98%B8+platform-web&body=Describe+your+issue+and+include+the+command+you%27re+running,+flutter_web%20version,+browser+version
[Google Cloud Hosting]: https://cloud.google.com/solutions/web-hosting
[`iframe`]: https://html.com/tags/iframe/
[Web renderers]: {{site.url}}/development/tools/web-renderers
