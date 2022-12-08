---
title: Desktop support for Flutter
title: Flutter 桌面支援
description: General information about Flutter support for desktop apps.
description: Flutter 的桌面端支援相關說明和資訊。
toc: true
tags: 文件
keywords: Flutter Desktop, Flutter 桌面版
---

Flutter provides support for compiling
a native Windows, macOS, or Linux desktop app.
Flutter's desktop support also extends to plugins&mdash;you
can install existing plugins that support the Windows,
macOS, or Linux platforms, or you can create your own.

桌面支援可以讓你的 Flutter 程式碼編譯成 Windows、macOS 或 Linux 的原生桌面應用。
Flutter 的桌面支援也允許外掛拓展&mdash;
你可以使用已經支援了 Windows、macOS 或 Linux 平台的外掛，或者建立你自己的外掛來實現功能。

{{site.alert.note}}

  This page covers developing apps for all desktop
  platforms. Once you've read this, you can dive into
  specific platform information at the following links:

  本頁面包含了所有桌面平台的應用開發內容。
  你閱讀之後可以在以下連結中深入檢視特定平台的內容。

  * [Building Windows apps with Flutter][]

    [透過 Flutter 開發 Windows 應用][Building Windows apps with Flutter]

  * [Building macOS apps with Flutter][]

    [透過 Flutter 開發 macOS 應用][Building Windows apps with Flutter]

  * [Building Linux apps with Flutter][]

    [透過 Flutter 開發 Linux 應用][Building Windows apps with Flutter]

{{site.alert.end}}

[Building Windows apps with Flutter]: {{site.url}}/development/platform-integration/windows/building
[Building macOS apps with Flutter]: {{site.url}}/development/platform-integration/macos/building
[Building Linux apps with Flutter]: {{site.url}}/development/platform-integration/linux/building

## Requirements

## 要求

To compile a desktop application,
you must build it **on** the targeted
platform: build a Windows application on Windows,
a macOS application on macOS,
and a Linux application on Linux.

要能夠編譯桌面應用，你必須 **在特定的平台** 上編譯應用:
在 Windows 上建構 Windows 應用，在 macOS 上建構 macOS 應用，
在 Linux 上建構 Linux 應用。

To create a Flutter application with desktop support,
you need the following software:

要建立一個支援桌面的 Flutter 應用，你需要以下的軟體：

* Flutter SDK. See the
  [Flutter SDK][] installation instructions.

  Flutter SDK，檢視 [Flutter SDK][] 安裝說明。

* Optional: An IDE that supports Flutter.
  You can install [Android Studio][], [IntelliJ IDEA][],
  or [Visual Studio Code][] and
  [install the Flutter and Dart plugins][]
  to enable language support and tools for refactoring,
  running, debugging, and reloading your desktop app
  within an editor. See [setting up an editor][]
  for more details.

  可選項：一個支援 Flutter 的 IDE。你可以安裝 [Android Studio][]、
  [IntelliJ IDEA][] 或 [Visual Studio Code][]，並且需要
  [安裝 Flutter 和 Dart 外掛][install the Flutter and Dart plugins]。
  這些外掛可以使 IDE 支援 Dart 語言，也為你提供了一些工具，
  如重構、允許、除錯和重載桌面應用。詳情請檢視 [配置一個編輯器][setting up an editor]。

[Android Studio]: {{site.android-dev}}/studio/install
[Flutter SDK]: {{site.url}}/get-started/install
[install the Flutter and Dart plugins]: {{site.url}}/get-started/editor
[IntelliJ IDEA]: https://www.jetbrains.com/idea/download/
[setting up an editor]: {{site.url}}/get-started/editor
[Visual Studio Code]: {{site.url}}/development/tools/vs-code

### Additional Windows requirements

### Windows 的額外要求

For Windows desktop development,
you need the following in addition to the Flutter SDK:

要開發 Windows 桌面程式，除了 Flutter SDK，你還需要做以下準備:

* [Visual Studio 2022][] or [Visual Studio Build Tools 2022][]
  When installing Visual Studio or only the Build Tools,
  select the "Desktop development with C++" workload,
  including all of its default components,
  to install the necessary C++ toolchain and
  Windows SDK header files.

  [Visual Studio 2022][] 或 [Visual Studio 2022 產生工具][Visual Studio Build Tools 2022]
  在選擇安裝 Visual Studio 時或只安裝產生工具的時候，
  你需要選擇「使用 C++ 的桌面開發」，包括其所有預設元件，
  以安裝必要的 C++ 工具鏈和 Windows SDK 的標頭檔案。

{{site.alert.note}}

  **Visual Studio** is different than Visual Studio _Code_.

  **Visual Studio** 與 Visual Studio **Code** 不同。

{{site.alert.end}}

[Visual Studio 2022]: https://visualstudio.microsoft.com/downloads/
[Visual Studio Build Tools 2022]: https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022

### Additional macOS requirements

### macOS 的額外要求

For macOS desktop development,
you need the following in addition to the Flutter SDK:

要開發 macOS 桌面程式，除了 Flutter SDK，你還需要做以下準備:

* [Xcode][]

* [CocoaPods][] if you use plugins

  如果使用外掛，需要安裝 [CocoaPods][]

[CocoaPods]: https://cocoapods.org/
[Xcode]: {{site.apple-dev}}/xcode/

### Additional Linux requirements

### Linux 的額外要求

For Linux desktop development,
you need the following in addition to the Flutter SDK:

要開發 Linux 桌面程式，除了 Flutter SDK，你還需要做以下準備:

* [Clang][]
* [CMake][]
* [GTK development headers][]
* [Ninja build][]
* [pkg-config][]
* [liblzma-dev][] This dependency may be required

One easy way to install the Flutter SDK along with the necessary
dependencies is by using [snapd][].
For more information, see [Installing snapd][].

安裝 Flutter SDK 和這些依賴，最簡單方式的方式是使用 [snapd][]。
更多詳細資訊，可以檢視 [安裝 snapd][Installing snapd]。

Once you have `snapd`, you can install Flutter
using the [Snap Store][], or at the command line:

安裝 `snapd` 後，你就可以使用 [Snap Store][] 安裝 Flutter，
也可以在命令列進行安裝:

```terminal
$ sudo snap install flutter --classic
```

Alternatively, if you prefer not to use `snapd`,
you can use the following command:

如果你在使用的 Linux 發行版上無法使用 `snapd`，你可以使用下面的命令列:

```terminal
$ sudo apt-get install clang cmake ninja-build pkg-config libgtk-3-dev liblzma-dev
```

[Clang]: https://clang.llvm.org/
[CMake]: https://cmake.org/
[GTK development headers]: https://developer.gnome.org/gtk3/3.2/gtk-getting-started.html
[Installing snapd]: https://snapcraft.io/docs/installing-snapd
[Ninja build]: https://ninja-build.org/
[pkg-config]: https://www.freedesktop.org/wiki/Software/pkg-config/
[liblzma-dev]: https://packages.debian.org/sid/liblzma-dev
[Snap Store]: https://snapcraft.io/store
[snapd]: https://snapcraft.io/flutter

## Create a new project

## 建立一個新專案

You can use the following steps
to create a new project with desktop support.

你可以透過下列步驟，來建立一個支援桌面的新專案。

### Set up

### 配置步驟

On Windows, desktop support is enabled on
Flutter 2.10 or higher. On macOS and Linux,
desktop support is enabled on Flutter 3 or higher.

Flutter 2.10 以及更高版本中加入了對 Windows 作業系統的桌面端支援。
mac OS 和 Linux 的桌面端支援需要使用 Flutter 3 及更高版本。

You might run `flutter doctor` to see if
there are any unresolved issues.
You should see a checkmark for each successfully
configured area. It should look something like
the following on Windows,
with an entry for "develop for Windows":

你也可以執行 `flutter doctor` 來檢視是否存在未解決的問題。
每一個成功的配置都有一個對勾，比如在 Windows 上你可能會看到如下內容:

```terminal
C:\> flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 3.0.0, on Microsoft Windows [Version 10.0.19044.1706], locale en-US)
[✓] Chrome - develop for the web
[✓] Visual Studio - develop for Windows (Visual Studio Professional 2022 17.2.0)
[✓] VS Code (version 1.67.2)
[✓] Connected device (3 available)
[✓] HTTP Host Availability

• No issues found!
```

On macOS, look for a line like this:

在 macOS 上，你可能會看到如下內容:

```terminal
[✓] Xcode - develop for iOS and macOS
```

On Linux, look for a line like this:

Linux 平台，你可能會看到如下內容：

```terminal
[✓] Linux toolchain - develop for Linux desktop
```

If `flutter doctor` finds problems or missing components
for a platform that you don't want to develop for,
you can ignore those warnings. Or you can disable the
platform altogether using the `flutter config` command,
for example:

在執行 `flutter doctor` 命令時如果發現有不需要支援的平台的問題或者元件缺少報錯等提示，
你可以忽略這些警告，或者使用 `flutter config` 命令來禁用這個平台，比如：

```terminal
$ flutter config --no-enable-ios
```

Other available flags:

其他可用的引數：

* `--no-enable-windows-desktop`
* `--no-enable-linux-desktop`
* `--no-enable-macos-desktop`
* `--no-enable-web`
* `--no-enable-android`
* `--no-enable-ios`

After enabling desktop support,
restart your IDE so that it can detect the new device.

加入了桌面端支援之後，請重啟你的 IDE，然後 IDE 就能檢測到新的裝置了。

### Create and run

### 建立和執行

Creating a new project with desktop support is no different
than [creating a new Flutter project][] for other platforms.

建立一個桌面支援的新專案，與在其他平台
[建立新的 Flutter 專案][creating a new Flutter project]
沒什麼不同的地方。

Once you've configured your environment for desktop
support, you can create and run a desktop application
either in the IDE or from the command line.

一旦配置好了桌面支援的環境，
你可以透過 IDE 或命令列建立和執行桌面程式。

[creating a new Flutter project]: {{site.url}}/get-started/test-drive

#### Using an IDE

#### 使用 IDE

After you've configured your environment to support
desktop, make sure you restart the IDE if it was
already running.

在你配置好桌面支援的環境後，記得重啟已經在執行的 IDE。

Create a new application in your IDE and it automatically
creates iOS, Android, web, and desktop versions of your app.
From the device pulldown, select **windows (desktop)**,
**macOS (desktop)**, or **linux (desktop)**
and run your application to see it launch on the desktop.

在你的 IDE 中建立新應用時，它會自動建立 iOS、 Android 和應用的桌面版本。
從裝置的下拉選項中，選擇 **windows (desktop)**、
**macOS (desktop)** 或 **linux (desktop)**
然後執行你的應用，就會看到應用在桌面啟動。

[web support]: {{site.url}}/get-started/web

#### From the command line

#### 使用命令列

To create a new application that includes desktop support
(in addition to mobile and web support), run the following commands,
substituting `my_app` with the name of your project:

想要建立一個包含桌面支援的新應用（除了支援移動和 Web），請執行下面的命令，
將 `my_app` 替換成你專案的名稱：

```terminal
$ flutter create my_app
$ cd my_app
```

To launch your application from the command line,
enter one of the following commands from the top
of the package:

想要從命令列啟動你的應用，可以在根目錄執行以下命令之一：

```terminal
C:\> flutter run -d windows
$ flutter run -d macos
$ flutter run -d linux
```

{{site.alert.note}}
If you do not supply the `-d` flag, `flutter run` lists
the available targets to choose from.
{{site.alert.end}}

## Build a release app

## 建立 release 版本的應用

To generate a release build,
run one of the following commands:

要產生 release 版本，可以執行以下命令之一：

```terminal
PS C:\> flutter build windows
$ flutter build macos
$ flutter build linux
```

## Add desktop support to an existing Flutter app

## 為已有的應用新增桌面支援

To add desktop support to an existing Flutter project,
run the following command in a terminal from the
root project directory:

想為已有的 Flutter 專案新增桌面支援，你可以從專案根目錄在控制檯執行下面命令：

```terminal
$ flutter create --platforms=windows,macos,linux .
```

This adds the necessary desktop files and directories
to your existing Flutter project.
To add only specific desktop platforms,
change the `platforms` list to include only
the platform(s) you want to add.

這將會在你的 Flutter 桌面專案中新增必要的已有檔案和資料夾。
如果需要只新增特定平台桌面端的支援，修改 `platforms` 的值
為你想要支援的平台即可。

## Plugin support

## 外掛支援

Flutter on the desktop supports using and creating plugins.
To use a plugin that supports desktop,
follow the steps for plugins in [using packages][].
Flutter automatically adds the necessary native code
to your project, as with any other platform.

Flutter 在桌面支援中使用和建立外掛。
使用支援桌面端的外掛，可以根據文件
[在 Flutter 裡使用 Packages][using packages]
中描述的內容進行操作。
Flutter 會像在其他平臺中一樣的操作，
自動將需要的原生平台程式碼加入到你的工程中。

### Writing a plugin

### 編寫一個外掛

When you start building your own plugins,
you’ll want to keep federation in mind.
Federation is the ability to define several
different packages, each targeted at a
different set of platforms, brought together
into a single plugin for ease of use by developers.
For example, the Windows implementation of the
`url_launcher` is really `url_launcher_windows`,
but a Flutter developer can simply add the
`url_launcher` package to their `pubspec.yaml`
as a dependency and the build process pulls in
the correct implementation based on the target platform.
Federation is handy because different teams with
different expertise can build plugin implementations
for different platforms.
You can add a new platform implementation to any
endorsed federated plugin on pub.dev,
so long as you coordinate this effort with the
original plugin author.

當你開始建構自己的外掛時，你需要記住聯合。聯合是定義幾個不同套件的能力，
其中每個包都針對不同的平台，將它們合併到一個外掛中，這樣方便開發人員使用。
比如，Windows 實現的 `url_launcher`，實際是透過 `url_launcher_windows` 完成的，
但是 Flutter 開發者可以在 `pubspec.yaml` 中，
簡單地新增 `url_launcher` 包作為依賴，在建構過程中會基於目標平台引入正確的實現。
聯合非常方便，因為具有不同專長的不同團隊，可以為不同的平台建構相應的外掛實現。
與原外掛作者協調之後，你可以為 pub.dev 上任何聯合外掛新增新的平台實現。

For more information, including information
about endorsed plugins, see the following resources:

想要了解更多資訊，包括關於已支援的外掛資訊，請參閱以下資源:

* [Developing packages and plugins][], particularly the
  [Federated plugins][] section.

  [開發套件和外掛][Developing packages and plugins]，特別是 [聯合外掛][Federated plugins] 部分。

* [How to write a Flutter web plugin, part 2][],
  covers the structure of federated plugins and
  contains information applicable to desktop
  plugins.

  [如何寫一個 Flutter web 外掛，第 2 部分][How to write a Flutter web plugin, part 2]，
  介紹聯合外掛的結構，幷包含適用於桌面外掛的資訊。

* [Modern Flutter Plugin Development][] covers
  recent enhancements to Flutter's plugin support.

  [現代 Flutter 外掛開發][Modern Flutter Plugin Development]
  介紹了最近對 Flutter 外掛支援的增強。

[using packages]: {{site.url}}/development/packages-and-plugins/using-packages
[Developing packages and plugins]: {{site.url}}/development/packages-and-plugins/developing-packages
[Federated plugins]: {{site.url}}/development/packages-and-plugins/developing-packages#federated-plugins
[How to write a Flutter web plugin, part 2]: {{site.flutter-medium}}/how-to-write-a-flutter-web-plugin-part-2-afdddb69ece6
[Modern Flutter Plugin Development]: {{site.flutter-medium}}/modern-flutter-plugin-development-4c3ee015cf5a

## Samples and codelabs

## Codelab 和 Flutter 文件

[Write a Flutter desktop application][]
<br> A codelab that walks you through building
a desktop application that integrates the GitHub
GraphQL API with your Flutter app.

[建構一個 Flutter 桌面程式][Write a Flutter desktop application]
<br> 這個 codelab 會引導你透過使用 Flutter，
來建構一個整合 GitHub GraphQL API 的桌面應用。

You can run the following samples as desktop apps,
as well as download and inspect the source code to
learn more about Flutter desktop support.

你可以執行下面的桌面應用案例，也可以下載並閱讀原始碼，
以瞭解更多關於 Flutter 桌面支援的資訊。

Flutter Gallery [running web app][], [repo][flutter-gallery-repo]
<br> A samples project hosted on GitHub to help developers
  evaluate and use Flutter. The Gallery consists of a
  collection of Material design widgets, behaviors,
  and vignettes implemented with Flutter.
  You can clone the project and run Gallery as a desktop app
  by following the instructions provided in the [README][].

Flutter Gallery [線上執行的 Web 應用][running web app]，[原始碼儲存庫地址][flutter-gallery-repo]
<br> 一個託管在 GitHub 上案實體目，可以用來幫助開發人員評估和使用 Flutter。
  Gallery 的構成部分有：Material design widgets 的集合、
  behaviors 和用 Flutter 實現的 vignettes。
  你可以複製該專案，並按照 [README][] 中的說明，將 Gallery 作為桌面應用程式執行。

Flokk [announcement blogpost][gskinner-flokk-blogpost], [repo][gskinner-flokk-repo]
<br> A Google contacts manager that integrates with GitHub and Twitter.
  It syncs with your Google account, imports your contacts,
  and allows you to manage them.

Flokk [官宣文章][gskinner-flokk-blogpost]，[原始碼儲存庫地址][gskinner-flokk-repo]
<br> 一款集成了 GitHub 和 Twitter 的谷歌聯絡人管理器應用。
  可以從你的 Google 賬戶同步資料，匯入聯絡人資訊，並管理它們。

[Photo Search app][]
<br> A sample application built as a desktop application that
  uses desktop-supported plugins.

[圖片搜尋應用][Photo Search app]
<br> 使用支援桌面端的外掛建構的一個桌面應用案例。

[Photo Search app]: {{site.repo.organization}}/samples/tree/main/desktop_photo_search
[running web app]: {{site.gallery}}
[flutter-gallery-repo]: {{site.repo.gallery}}
[README]: {{site.repo.gallery}}#readme
[gskinner-flokk-repo]: {{site.github}}/gskinnerTeam/flokk
[gskinner-flokk-blogpost]: https://blog.gskinner.com/archives/2020/09/flokk-how-we-built-a-desktop-app-using-flutter.html
[Write a Flutter desktop application]: {{site.codelabs}}/codelabs/flutter-github-client
