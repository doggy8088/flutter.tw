---
title: Building a web application with Flutter
title: 建構 Flutter Web 應用
short-title: Web development
description: Instructions for creating a Flutter app for the web.
description: 針對 Web 平台建構 Flutter 應用的指南。
---

This page covers the following steps for getting started with web support:

本頁面將通過幾個步驟讓你開啟 Web 平台的支援：

* Configure the `flutter` tool for web support.

  配置 `flutter` 工具開啟 Web 支援。

* Create a new project with web support.

  建立支援 Web 的新專案。

* Run a new project with web support.

  執行支援 Web 的新專案。

* Build an app with web support.

  建構支援 Web 的新專案。

* Add web support to an existing project.

  為現有專案新增 Web 支援。

## Requirements

## 要求

To create a Flutter app with web support,
you need the following software:

要建立支援 Web 平台的 Flutter 應用，你需要以下這些軟體內容：

* Flutter SDK. See the
  [Flutter SDK][] installation instructions.

  Flutter SDK。檢視 [Flutter SDK][] 安裝指導。

* [Chrome][]; debugging a web app requires
  the Chrome browser.

  [Chrome][]：用於除錯 Web 應用。

* Optional: An IDE that supports Flutter.
  You can install [Visual Studio Code][],
  [Android Studio][], [IntelliJ IDEA][].
  Also [install the Flutter and Dart plugins][]
  to enable language support and tools for refactoring,
  running, debugging, and reloading your web app
  within an editor. See [setting up an editor][]
  for more details.

  可選：用於 Flutter 開發的 IDE。
  你可以安裝 [Android Studio][]、[IntelliJ IDEA][]
  或者 [Visual Studio Code][]，並且
  [安裝 Flutter 和 Dart 外掛][install the Flutter and Dart plugins]
  以啟用語言支援、用於執行/除錯/重構的開發工具，
  以及在 IDE 中重載你的 Web 應用。
  檢視 [編輯工具設定][setting up an editor] 瞭解更多。

[Android Studio]: https://developer.android.com/studio
[IntelliJ IDEA]: https://www.jetbrains.com/idea/
[Visual Studio Code]: https://code.visualstudio.com/


For more information, see the [web FAQ][].

更多內容請閱讀 [Web 平台常見問題][Web FAQ]。

## Create a new project with web support

## 建立支援 Web 的新專案

You can use the following steps
to create a new project with web support.

你可以依照以下步驟建立支援 Web 的新專案。

### Set up

### 準備工作

Run the following commands to use the latest version of the Flutter SDK:

執行以下命令以使用最新版本的 Flutter SDK：

```terminal
$ flutter channel stable
$ flutter upgrade
```

{{site.alert.warning}}

  Running `flutter channel stable` replaces your current version of Flutter
  with the stable version and can take time if your connection is slow.
  After this, running `flutter upgrade` upgrades your install to the latest
 `stable`.  Returning to another channel (beta or master) requires calling
 `flutter channel <channel>` explicitly.

  執行 `flutter channel stable` 會替換你正在使用的 Flutter 版本為穩定版本，
  如果你的網路狀況不好，可能會花費較多的時間。
  執行完成後，再執行 `flutter upgrade` 會升級到最新的 `stable` 版本。
  如果你需要切換回其他渠道的 Flutter 版本（beta 或 master），
  你需要執行 `flutter channel <渠道>`。

{{site.alert.end}}

If Chrome is installed,
the `flutter devices` command outputs a `Chrome` device
that opens the Chrome browser with your app running,
and a `Web Server` that provides the URL serving the app.

如果 Chrome 已經安裝，`flutter devices` 命令會輸出一個
`Chrome` 裝置，在執行應用時會使用它啟動應用；
以及一個將應用提供在指定 URL 的 `Web Server`。

```terminal
$ flutter devices
1 connected device:

Chrome (web) • chrome • web-javascript • Google Chrome 88.0.4324.150
```

In your IDE, you should see **Chrome (web)** in the device pulldown.

在你的 IDE 中，你可以在裝置下拉列表中看到 **Chrome (web)**。

### Create and run

### 建立並執行

Creating a new project with web support is no different
than [creating a new Flutter project][] for other platforms.

建立支援 Web 的新專案的步驟與
[開發體驗初探][creating a new Flutter project]
中建立其他平台應用的步驟相差無幾。

#### IDE

#### 編輯器 (IDE)

Create a new app in your IDE and it automatically
creates iOS, Android, [desktop][], and web versions of your app.
From the device pulldown, select **Chrome (web)**
and run your app to see it launch in Chrome.

在 IDE 中建立新應用時，會自動包含 iOS、Android 和 Web 支援。
（如果你啟用了 [桌面平台支援][desktop]，也會一併支援。）
在裝置下拉列表裡，選擇 **Chrome (web)** 執行你的應用，
它會在 Chrome 中啟動。

#### Command line

#### 命令列

To create a new app that includes web support
(in addition to mobile support), run the following commands,
substituting `my_app` with the name of your project:

執行以下命令建立支援 Web 平台的應用（包括移動端），
你可以調整 `myapp` 為你的專案名稱：

```terminal
$ flutter create my_app
$ cd my_app
```

To serve your app from `localhost` in Chrome,
enter the following from the top of the package:

想要使用 Chrome 在 `localhost` 存取到你的應用，
在應用的根目錄執行以下命令：

```terminal
$ flutter run -d chrome
```
{{site.alert.note}}

  If there aren't any other connected devices,
  the `-d chrome` is optional.

  如果你沒有連線任何裝置，那麼 `-d chrome` 引數是可選的。

{{site.alert.end}}

The `flutter run` command launches the application using the
[development compiler] in a Chrome browser.

`flutter run` 命令會在 Chrome 中使用 [開發編譯器][development compiler]。

{{site.alert.warning}}

  **Hot reload is not supported in a web browser**
  Currently, Flutter supports **hot restart**,
  but not **hot reload** in a web browser.

  **Web 瀏覽器不支援熱重載。**
  目前為止，Flutter 僅支援在 Web 瀏覽器中 **熱重啟**，
  不支援 **熱重載**。

{{site.alert.end}}

### Build

### 建構

Run the following command to generate a release build:

執行以下命令產生 release 版本的建構：

```terminal
$ flutter build web
```

A release build uses [dart2js][]
(instead of the [development compiler][])
to produce a single JavaScript file `main.dart.js`.
You can create a release build using release mode
(`flutter run --release`) or by using `flutter build web`.
This populates a `build/web` directory
with built files, including an `assets` directory,
which need to be served together.

釋出版本的建構會使用 [dart2js][]
（而不是 [開發編譯器][development compiler]）
來產生單獨的 `main.dart.js` 檔案。
你可以透過執行 release 模式 (`flutter run --release`)
或執行 `flutter build web` 來建構 release 模式的應用。
它們會在 `build/web` 目錄下產生建構的檔案，
包括需要一併提供的 `assets` 資料夾。

You can also include `--web-renderer html`  or `--web-renderer canvaskit` to
select between the HTML or CanvasKit renderers, respectively. For more
information, see [Web renderers][].

同時，你可以使用 `--web-renderer html` 或者 `--web-renderer canvaskit`
分別切換 HTML 或 CanvasKit 渲染器。
想要了解更多資訊，你可以閱讀 [Web 渲染器][Web renderers]。

For more information, see
[Build and release a web app][].

想要了解更多 Web 建構的內容，你可以閱讀
[建構和釋出為 Web 應用][Build and release a web app]。

## Add web support to an existing app

## 為現有專案新增 Web 支援

To add web support to an existing project
created using a previous version of Flutter,
run the following command
from your project's top-level directory:

要為現有的專案新增 Web 支援，請在專案的根目錄下執行：

```terminal
$ flutter create --platforms web .
```

[Build and release a web app]: {{site.url}}/deployment/web
[creating a new Flutter project]: {{site.url}}/get-started/test-drive
[dart2js]: {{site.dart-site}}/tools/dart2js
[desktop]: {{site.url}}/platform-integration/desktop
[development compiler]: {{site.dart-site}}/tools/dartdevc
[file an issue]: {{site.repo.flutter}}/issues/new?title=[web]:+%3Cdescribe+issue+here%3E&labels=%E2%98%B8+platform-web&body=Describe+your+issue+and+include+the+command+you%27re+running,+flutter_web%20version,+browser+version
[install the Flutter and Dart plugins]: {{site.url}}/get-started/editor
[setting up an editor]: {{site.url}}/get-started/editor
[web FAQ]: {{site.url}}/platform-integration/web/faq
[Chrome]: https://www.google.com/chrome/
[Flutter SDK]: {{site.url}}/get-started/install
[Web renderers]: {{site.url}}/platform-integration/web/renderers
