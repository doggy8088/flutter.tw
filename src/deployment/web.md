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

* [Building the app for release](#building-the-app-for-release)

  [建構正式版本的應用](#building-the-app-for-release)

* [Deploying to the web](#deploying-to-the-web)

  [部署應用到 Web](#deploying-to-the-web)

* [Deploying to Firebase Hosting](#deploying-to-firebase-hosting)

  [部署到 Firebase 主機](#deploying-to-firebase-hosting)

* [Handling images on the web](#handling-images-on-the-web)

  [處理 Web 中的圖片](#handling-images-on-the-web)

* [Choosing a web renderer](#choosing-a-web-renderer)

  [選擇 Web 渲染器](#choosing-a-web-renderer)

* [Minification](#minification)

  [壓縮](#minification)

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
    packages
      cupertino_icons
        assets
          CupertinoIcons.ttf
    shaders
      ink_sparkle.frag
  canvaskit
    canvaskit.js
    canvaskit.wasm
    profiling
      canvaskit.js
      canvaskit.wasm
  favicon.png
  flutter.js
  flutter_service_worker.js
  index.html
  main.dart.js
  manifest.json
  version.json
```

{{site.alert.note}}
  The `canvaskit` directory and its contents are only present when the
  CanvasKit renderer is selected—not when the HTML renderer is selected.
{{site.alert.end}}

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

## Deploying to the web

When you are ready to deploy your app,
upload the release bundle
to Firebase, the cloud, or a similar service.
Here are a few possibilities, but there are
many others:

* [Firebase Hosting][]
* [GitHub Pages][]
* [Google Cloud Hosting][]

## Deploying to Firebase Hosting
You can use the Firebase CLI to build and release your Flutter app with Firebase
Hosting.

### Before you begin
To get started, [install or update][install-firebase-cli] the Firebase CLI:

```
npm install -g firebase-tools
```

### Initialize Firebase

1. Enable the web frameworks preview to the [Firebase framework-aware CLI][]:

    ```
    firebase experiments:enable webframeworks
    ```

2. In an empty directory or an existing Flutter project, run the initialization
command:

    ```
    firebase init hosting
    ```

3. Answer `yes` when asked if you want to use a web framework.

4. If you're in an empty directory,
    you'll be asked to choose your web framework. Choose `Flutter Web`.

5. Choose your hosting source directory; this could be an existing flutter app.

6. Select a region to host your files.

7. Choose whether to set up automatic builds and deploys with GitHub.

8. Deploy the app to Firebase Hosting:

    ```terminal
    firebase deploy
    ```

    Running this command automatically runs `flutter build web --release`,
    so you don't have to build your app in a separate step.

To learn more, visit the official [Firebase Hosting][] documentation for
Flutter on the web.

## Handling images on the web

The web supports the standard `Image` widget to display images.
By design, web browsers run untrusted code without harming the host computer.
This limits what you can do with images compared to mobile and desktop platforms.

For more information, see [Displaying images on the web][].

## Choosing a web renderer

By default, the `flutter build` and `flutter run` commands
use the `auto` choice for the web renderer. This means that
your app runs with the HTML renderer on mobile browsers and
CanvasKit on desktop browsers. We recommend this combination
to optimize for the characteristics of each platform.

For more information, see [Web renderers][].

## Minification

Minification is handled for you when you
create a release build.

| Type of web app build | Code minified? | Tree shaking performed? |
|-----------------------|----------------|-------------------------|
| debug                 | No             | No                      |
| profile               | No             | Yes                     |
| release               | Yes            | Yes                     |

## Embedding a Flutter app into an HTML page

## 將 Flutter 應用內嵌到一個 HTML 頁面裡

### `hostElement`

### 使用 `hostElement`

_Added in Flutter 3.10_<br>
You can embed a Flutter web app into
any HTML element of your web page, with `flutter.js` and the `hostElement`
engine initialization parameter.

** 在 Flutter 3.10 中新增** <br>
你可以使用 `flutter.js` 和 `hostElement` 引擎初始化引數將 
Flutter Web 應用嵌入到 Web 頁面的任何 HTML 元素中。

To tell Flutter web in which element to render, use the `hostElement` parameter of the `initializeEngine`
function:

要告訴 Flutter Web 在哪個元素中呈現，
請使用 `initializeEngine` 函式的 `hostElement` 引數：

```html
<html>
  <head>
    <!-- ... -->
    <script src="flutter.js" defer></script>
  </head>
  <body>

    <!-- Ensure your flutter target is present on the page... -->
    <div id="flutter_host">Loading...</div>

    <script>
      window.addEventListener("load", function (ev) {
        _flutter.loader.loadEntrypoint({
          onEntrypointLoaded: async function(engineInitializer) {
            let appRunner = await engineInitializer.initializeEngine({
              // Pass a reference to "div#flutter_host" into the Flutter engine.
              hostElement: document.querySelector("#flutter_host")
            });
            await appRunner.runApp();
          }
        });
      });
    </script>
  </body>
</html>
```

To learn more, check out [Customizing web app initialization][customizing-web-init].

要了解更多，請檢視 [自訂 Web 應用的初始化][customizing-web-init]。

### Iframe

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
so please [give us feedback][] if you see something that doesn't look right.

對 PWA 的支援仍在進行中，因此，如果您發現不正確的地方，
歡迎 [給予我們反饋][give us feedback]。

[dhttpd]: {{site.pub}}/packages/dhttpd
[Displaying images on the web]: {{site.url}}/platform-integration/web/web-images
[Firebase Hosting]: {{site.firebase}}/docs/hosting/frameworks/flutter
[Firebase framework-aware CLI]: {{site.firebase}}/docs/hosting/frameworks/frameworks-overview
[install-firebase-cli]: https://firebase.google.com/docs/cli#install_the_firebase_cli
[GitHub Pages]: https://pages.github.com/
[give us feedback]: {{site.repo.flutter}}/issues/new?title=%5Bweb%5D:+%3Cdescribe+issue+here%3E&labels=%E2%98%B8+platform-web&body=Describe+your+issue+and+include+the+command+you%27re+running,+flutter_web%20version,+browser+version
[Google Cloud Hosting]: https://cloud.google.com/solutions/web-hosting
[`iframe`]: https://html.com/tags/iframe/
[Web renderers]: {{site.url}}/platform-integration/web/renderers
[customizing-web-init]: {{site.url}}/platform-integration/web/initialization

