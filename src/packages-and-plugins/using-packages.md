---
title: Using packages
title: 在 Flutter 裡使用 Packages
description: How to use packages in your Flutter app.
description: 如何在你的 Flutter 應用裡使用 Packages。
tags: Packages,外掛
keywords: 使用packages,Flutter第三方庫
---

<?code-excerpt path-base="development/plugin_api_migration"?>

Flutter supports using shared packages contributed by other developers
to the Flutter and Dart ecosystems. This allows quickly building
an app without having to develop everything from scratch.

Flutter 支援使用其他開發者向 Flutter 和 Dart 生態系統貢獻的共享 package，
這意味著你可以快速建構應用而不是一切從零開始。

{{site.alert.secondary}}

  **What is the difference between a package
  and a plugin?** A plugin is a _type_ of
  package&mdash;the full designation is _plugin package_,
  which is generally shortened to _plugin_.
  
  **Package 和外掛 (plugin) 有什麼區別呢？**
  外掛 (plugin) 是 package 的一種，全稱是 plugin package，
  我們簡稱為 plugin，中文叫外掛。

  **Packages**
  <br> At a minimum, a Dart package is a directory
    containing a `pubspec.yaml` file. Additionally,
    a package can contain dependencies
    (listed in the pubspec), Dart libraries, apps,
    resources, tests, images, fonts, and examples.
    The [pub.dev][] site lists many packages—developed by Google engineers
    and generous members of the Flutter and Dart community—
    that you can use in your app.
    
  **Packages**
  <br> Dart package 最低要求是包含一個 `pubspec.yaml` 檔案。
    此外，一個 package 可以包含依賴關係 (在 `pubspec.yaml` 檔案裡宣告)、
    Dart 庫、應用、資源、字型、測試、圖片和例子等。
    [pub.dev][] 上列出了很多 package，由 Google 工程師和
    Flutter 和 Dart 社群的開發者開發和釋出，你可以用在自己的應用裡。

  **Plugins**
  <br> A plugin package is a special kind of package that makes
    platform functionality available to the app.
    Plugin packages can be written for Android (using Kotlin or Java),
    iOS (using Swift or Objective-C), web, macOS, Windows, Linux,
    or any combination thereof.
    For example, a plugin might provide Flutter apps
    with the ability to use a device's camera.

  **Plugins**
  <br> 外掛 (plugin package) 是一種特別的 package，特別指
    那些幫助你獲得原生平台特性的 package。
    外掛可以為 Android (使用 Kotlin 或 Java 語言)、
    iOS (使用 Swift 或 Objective-C 語言)、Web、macOS、Windows、Linux 平台，
    或其任意組合的平台編寫。
    比如：某個外掛可以為 Flutter 應用提供使用原生平台的攝影頭的功能。

  <iframe width="560" height="315" src="https://player.bilibili.com/player.html?bvid=BV1dY4y1r7xD&page=1&autoplay=false" title="Bilibili video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{{site.alert.end}}

Existing packages enable many use cases—for example,
making network requests ([`http`][]),
navigation/route handling ([`go_router`][]),
integration with device APIs
([`url_launcher`][] and [`battery_plus`][]),
and using third-party platform SDKs like Firebase
([FlutterFire][]).

現有的 package 支援許多使用場景，例如，網路請求 ([`http`][])，
自訂導航/路由處理 ([`go_router`][])，整合裝置 API（如 [`url_launcher`][] 和
[`battery_plus`][]，以及使用第三方平台的 SDK（如 Firebase 的 ([FlutterFire][])。

To write a new package, see [developing packages][].
To add assets, images, or fonts,
whether stored in files or packages,
see [Adding assets and images][].

如果你正打算開發新的 package，請參閱
[Flutter Packages 的開發和提交][developing packages]。
如果你想新增資源、圖片或字型，無論是儲存在檔案中還是 package 中，
請參閱 [新增資源和圖片][Adding assets and images] 這篇文件。

[Adding assets and images]: {{site.url}}/ui/assets/assets-and-images
[`battery_plus`]: {{site.pub-pkg}}/battery_plus
[developing packages]: {{site.url}}/packages-and-plugins/developing-packages
[FlutterFire]: {{site.github}}/firebase/flutterfire


[`go_router`]: {{site.pub-pkg}}/go_router
[`http`]: {{site.url}}/cookbook/networking/fetch-data
[pub.dev]: {{site.pub}}
[`url_launcher`]: {{site.pub-pkg}}/url_launcher

## Using packages

## 使用 package

The following section describes how to use
existing published packages.

下面的內容將為你描述如何使用已經發布了的 packages。

### Searching for packages

### 搜尋 package

Packages are published to [pub.dev][].

Package 會被髮布到 [pub.dev][] 網站上。

The [Flutter landing page][] on pub.dev displays
top packages that are compatible with Flutter
(those that declare dependencies generally compatible with Flutter),
and supports searching among all published packages.

Pub 網站上的 [Flutter 頁面][Flutter landing page] 
展示了與 Flutter 相容的 package（即宣告的依賴通常與 Flutter 相容），
並且所有已釋出的 package 都支援搜尋。

The [Flutter Favorites][] page on pub.dev lists
the plugins and packages that have been identified as
packages you should first consider using when writing
your app. For more information on what it means to
be a Flutter Favorite, see the
[Flutter Favorites program][].

Pub.dev 上的 [Flutter Favorites][] 頁面列出了一系列編寫應用時
可以首先考慮使用的外掛和 package，關於這個專案的更多資訊，請
檢視 [Flutter Favorites 專案][Flutter Favorites program] 頁面。

You can also browse the packages on pub.dev by filtering
on [Android][], [iOS][], [web][],
[Linux][], [Windows][], [macOS][],
or any combination thereof.

在 pub.dev 網站上你可以同時過濾出適合
[Android][]、[iOS][]、[Web][web]、[Linux][]、
[Windows][] 或 [macOS][] 的外掛，
你也可以透過複選框，過濾出組合結果（適配一個或者多個平台）。

[Android]: {{site.pub-pkg}}?q=sdk%3Aflutter+platform%3Aandroid
[Flutter Favorites]: {{site.pub}}/flutter/favorites
[Flutter Favorites program]: {{site.url}}/packages-and-plugins/favorites
[Flutter landing page]: {{site.pub}}/flutter
[Linux]: {{site.pub-pkgs}}?q=sdk%3Aflutter+platform%3Alinux
[iOS]: {{site.pub-pkg}}?q=sdk%3Aflutter+platform%3Aios
[macOS]: {{site.pub-pkg}}?q=sdk%3Aflutter+platform%3Amacos
[web]: {{site.pub-pkg}}?q=sdk%3Aflutter+platform%3Aweb
[Windows]: {{site.pub-pkg}}?q=sdk%3Aflutter+platform%3Awindows

### Adding a package dependency to an app

### 將 package 依賴新增到應用

To add the package, `css_colors`, to an app:

要將 package 'css_colors' 新增到應用：

1. Depend on it

   新增依賴

   * Open the `pubspec.yaml` file located inside the app folder,
     and add `css_colors:` under `dependencies`.

     開啟應用資料夾下的 `pubspec.yaml` 檔案，
     然後在 `pubspec.yaml` 下新增 `css_colors:`。

1. Install it

   安裝

   * From the terminal: Run `flutter pub get`<br/>

     在命令列中執行：`flutter pub get`<br/>

   **OR**

   **或者**

   * From VS Code: Click **Get Packages** located in right side of the action
     ribbon at the top of `pubspec.yaml` indicated by the Download icon.

     在 VS Code 中點選位於 `pubspec.yaml` 檔案頂部操作功能區右側的 **Get Packages**

   * From Android Studio/IntelliJ: Click **Packages get** in the action
     ribbon at the top of `pubspec.yaml`.

     在 Android Studio/IntelliJ 中點選 `pubspec.yaml` 檔案頂部操作功能區的 **Packages get**

1. Import it

   匯入
   
   * Add a corresponding `import` statement in the Dart code.

     在 Dart 程式碼中新增相關的 `import` 陳述式。

1. Stop and restart the app, if necessary

   如果有必要，停止並重啟應用

   * If the package brings platform-specific code
     (Kotlin/Java for Android, Swift/Objective-C for iOS),
     that code must be built into your app.
     Hot reload and hot restart only update the Dart code,
     so a full restart of the app might be required to avoid
     errors like `MissingPluginException` when using the package.
     
     如果 package 內有特定平台的程式碼（Android 的 Java/Kotlin,
     iOS 的 Swift/Objective-C），程式碼必須內建到你的應用內。
     熱重載和熱重啟只對 package 的 Dart 程式碼執行此操作，
     所以你需要完全重啟應用以避免使用 package 時
     出現 `MissingPluginException` 錯誤。
     
### Adding a package dependency to an app using `flutter pub add`

### 使用命令 `flutter pub add` 新增一個 package 依賴

To add the package, `css_colors`, to an app:

將 `css_colors` 這個 package 新增到工程中：

1. Issue the command while being inside the project directory

   在專案根目錄執行命令

   * `flutter pub add css_colors`

1. Import it

   匯入

   * Add a corresponding `import` statement in the Dart code.
   
     在 Dart 程式碼中新增相應的 `import` 陳述式

1. Stop and restart the app, if necessary

   如果有必要，停止並重啟應用

   * If the package brings platform-specific code
     (Kotlin/Java for Android, Swift/Objective-C for iOS),
     that code must be built into your app.
     Hot reload and hot restart only update the Dart code,
     so a full restart of the app might be required to avoid
     errors like `MissingPluginException` when using the package.
     
     如果 package 內有特定平台的程式碼（Android 的 Java/Kotlin,
     iOS 的 Swift/Objective-C），程式碼必須內建到你的應用內。
     熱重載和熱重啟只對 package 的 Dart 程式碼執行此操作，
     所以你需要完全重啟應用以避免使用 package 時
     出現 `MissingPluginException` 錯誤。
     
### Removing a package dependency to an app using `flutter pub remove`

### 使用 `flutter pub remove` 命令移除一個 package 依賴

To remove the package, `css_colors`, to an app:

要將 package 'css_colors' 從工程中移除：

1. Issue the command while being inside the project directory

   在專案根目錄執行命令

   * `flutter pub remove css_colors`

The [Installing tab][],
available on any package page on pub.dev,
is a handy reference for these steps.

對於這些步驟，Pub 上任何 package 頁面的
[Installing tab][] 選項卡都是一個很方便的參考。

For a complete example,
see the [css_colors example][] below.

完整範例，參閱下面的
[css_colors 範例][css_colors example]。

[css_colors example]: #css-example
[Installing tab]: {{site.pub-pkg}}/css_colors/install

### Conflict resolution

### 衝突解決

Suppose you want to use `some_package` and
`another_package` in an app,
and both of these depend on `url_launcher`,
but in different versions.
That causes a potential conflict.
The best way to avoid this is for package authors to use
[version ranges][] rather than specific versions when
specifying dependencies.

假設你想在應用中使用 `some_package` 和
`other_package`，並且它們依賴於不同版本的 `url_launcher`。
於是我們便有了潛在的衝突。避免這種情況的最好方法是 package
的作者在指定依賴項時使用 [版本範圍][version ranges] 而非特定版本。

```yaml
dependencies:
  url_launcher: ^5.4.0    # Good, any version >= 5.4.0 but < 6.0.0
  image_picker: '5.4.3'   # Not so good, only version 5.4.3 works.
```

If `some_package` declares the dependencies above
and `another_package` declares a compatible
`url_launcher` dependency like `'5.4.6'` or
`^5.5.0`, pub resolves the issue automatically.
Platform-specific dependencies on
[Gradle modules][] and/or [CocoaPods][]
are solved in a similar way.

如果 `some_package` 聲明瞭以上依賴，
並且 `another_package` 聲明瞭一個相容的
`url_launcher` 依賴項，如 `'5.4.6'` 或 `^5.5.0`，
pub 能夠自動解決衝突問題。
[Gradle modules][] 和 [CocoaPods][]
也是用類似的方式解決平台依賴的。

Even if `some_package` and `another_package`
declare incompatible versions for `url_launcher`,
they might actually use `url_launcher` in
compatible ways. In this situation,
the conflict can be resolved by adding
a dependency override declaration to the app's
`pubspec.yaml` file, forcing the use of a particular version.

即使 `some_package` 和 `another_package` 聲明瞭不相容的 `url_launcher`
版本，它們實際上仍可能以相容的方式使用 `url_launcher`。在這種情況下，可在
`pubspec.yaml` 檔案中新增一個依賴覆蓋宣告來強制使用特定版本，從而處理衝突。

For example, to force the use of `url_launcher` version `5.4.0`,
make the following changes to the app's `pubspec.yaml` file:

為了強制使用版本為 `5.4.0` 的 `url_launcher`，
你可以對應用的 `pubspec.yaml` 檔案做如下更改：

```yaml
dependencies:
  some_package:
  another_package:
dependency_overrides:
  url_launcher: '5.4.0'
```

If the conflicting dependency is not itself a package,
but an Android-specific library like `guava`,
the dependency override declaration must be added to
Gradle build logic instead.

如果依賴衝突項不是 package 自身，而是如 `guava` 這樣特定於 Android 的函式庫，
那麼依賴的覆蓋宣告必須新增到 Gradle 的建構邏輯中。

To force the use of `guava` version `28.0`, make the following
changes to the app's `android/build.gradle` file:

為了強制使用版本為 `28.0` 的 `guava`，
你可以對 `android/build.gradle` 檔案做如下更改：

```groovy
configurations.all {
    resolutionStrategy {
        force 'com.google.guava:guava:28.0-android'
    }
}
```

CocoaPods doesn't currently offer dependency
override functionality.

CocoaPods 目前尚不提供依賴項覆蓋功能。

[CocoaPods]: https://guides.cocoapods.org/syntax/podspec.html#dependency
[Gradle modules]: https://docs.gradle.org/current/userguide/declaring_dependencies.html
[version ranges]: {{site.dart-site}}/tools/pub/dependencies#version-constraints

## Developing new packages

## 開發新的 package

If no package exists for your specific use case,
you can [write a custom package][].

如果某個 package 不適用於你的特定需求，
你可以 [開發新的自訂 package][write a custom package]。

[write a custom package]: {{site.url}}/packages-and-plugins/developing-packages

## Managing package dependencies and versions

## 管理 package 的依賴和版本

To minimize the risk of version collisions,
specify a version range in the `pubspec.yaml` file.

為了使版本衝突的風險最小化，請在 `pubspec.yaml` 檔案中指定一個版本範圍。

### Package versions

### Package 版本

All packages have a version number, specified in the
package's `pubspec.yaml` file. The current version of a package
is displayed next to its name (for example,
see the [`url_launcher`][] package), as
well as a list of all prior versions
(see [`url_launcher` versions][]).

所有 package 都有一個版本號，在它們的 `pubspec.yaml` 檔案中指定。
當前的 package 版本會在其名稱旁邊顯示當前版本號。
（例如，參閱 [`url_launcher`][] package）以及所有先前版本的列表：
[url_launcher 版本列表][`url_launcher` versions]。

To ensure that the app doesn't break when a
package is updated,
specify a version range using one of the
following formats:

為了確保在更新 package 的時候你的應用不會崩潰，
我們建議使用以下格式之一來指定版本範圍：

* Range constraints: Specify a minimum and maximum version. For example:

  範圍限制：指定一個最小和最大的版本號，例如：

  ```yaml
  dependencies:
    url_launcher: '>=5.4.0 <6.0.0'
  ```

* Range constraints with [*caret syntax*][]
  are similar to regular range constraints:

  使用 [*caret語法*][*caret syntax*] 的範圍約束與常規的範圍約束類似：
  
  ```yaml
  dependencies:
    collection: '^5.4.0'
  ```

For additional details,
see the [package versioning guide][].

瞭解更詳細的資訊，參閱 [Pub 版本管理指南][package versioning guide]。

[*caret syntax*]: {{site.dart-site}}/tools/pub/dependencies#caret-syntax
[package versioning guide]: {{site.dart-site}}/tools/pub/versioning
[`url_launcher` versions]: {{site.pub-pkg}}/url_launcher/versions

### Updating package dependencies

### 更新 package 依賴

When running `flutter pub get` (**Packages get** in IntelliJ
or Android Studio) for the first time after adding a package,
When running `flutter pub get` 
for the first time after adding a package,
Flutter saves the concrete package version found in the `pubspec.lock`
[lockfile][]. This ensures that you get the same version again
if you, or another developer on your team, run `flutter pub get`.

當你新增一個 package 後首次執行
`flutter pub get`（IntelliJ 或 Android Studio 中的 **Packages Get**），
Flutter 將會儲存在 `pubspec.lock` [lockfile][] 中找到的具體 package 版本。
這將確保當你或者團隊中其他開發者執行
`flutter pub get` 後能得到相同版本的 package。

To upgrade to a new version of the package,
for example to use new features in that package,
run `flutter pub upgrade`
to retrieve the highest available version of the package
that is allowed by the version constraint specified in
`pubspec.yaml`.
Note that this is a different command from
`flutter upgrade` or `flutter update-packages`,
which both update Flutter itself.

如果你想升級到 package 的最新版本，比如使用 package 的最新特性，
請執行 `flutter pub upgrade`。
這將檢索你在 `pubspec.yaml` 檔案中指定的版本約束所允許的最高可用版本。
請注意，`flutter upgrade` 與 `flutter update-packages` 是兩個
不同的命令，但它們都會更新 Flutter。

[lockfile]: {{site.dart-site}}/tools/pub/glossary#lockfile

### Dependencies on unpublished packages

### 依賴未釋出的 package

Packages can be used even when not published on pub.dev.
For private packages, or for packages not ready for publishing,
additional dependency options are available:

即使未在 Pub site 上釋出，也可以使用 package。
對於不用於公開發布的私有外掛，
或者尚未準備好釋出的 package，可以使用其他依賴選項。

**Path dependency**
<br> A Flutter app can depend on a package via a file system
  `path:` dependency. The path can be either relative or absolute.
  Relative paths are evaluated relative to the directory
  containing `pubspec.yaml`. For example, to depend on a
  package, packageA, located in a directory next to the app,
  use the following syntax:

**Path 依賴**
<br> Flutter 應用可以透過檔案系統 `path:` 依賴而依賴於外掛。
  路徑可以是相對的，也可以是絕對的。
  例如，要依賴位於應用相鄰目錄中的外掛 `plugin1`，可以使用以下語法：

  ```yaml
    dependencies:
    packageA:
      path: ../packageA/
  
  ```

**Git dependency**
<br> You can also depend on a package stored in a Git repository.
  If the package is located at the root of the repo,
  use the following syntax:

**Git 依賴**
<br> 你也可以依賴儲存在 Git 儲存庫中的 package，
  如果 package 位於儲存庫的根目錄，可以使用以下語法：
  
  ```yaml
    dependencies:
      packageA:
        git:
          url: https://github.com/flutter/packageA.git
  ```

**Git dependency using SSH**
<br> If the repository is private and you can connect to it using SSH,
  depend on the package by using the repo's SSH url:

**透過 SSH 依賴 Git package**
<br> 如果你需要透過 SSH 連線私有的儲存庫，你可以用 SSH 連結依賴對應的 package：

  ```yaml
    dependencies:
      packageA:
        git:
          url: git@github.com:flutter/packageA.git
  ```

**Git dependency on a package in a folder**
<br> Pub assumes the package is located in
  the root of the Git repository. If that isn't
  the case, specify the location with the `path` argument.
  For example:

**Git 依賴於資料夾中的 package **
<br> 預設情況下，pub 會預設假定 package 位於 Git 儲存庫的根目錄。
  如果不是這種情況，你可以使用 `path` 引數指定位置，例如：
  
  ```yaml
  dependencies:
    packageA:
      git:
        url: https://github.com/flutter/packages.git
        path: packages/packageA
  ```

  Finally, use the `ref` argument to pin the dependency to a
  specific git commit, branch, or tag. For more details, see
  [Package dependencies][].

  最後，你可以使用 `ref` 引數將依賴固定到 git 特定的 commit、branch 或者 tag。
  更多詳細資訊，請參閱 [Package dependencies][]。

[Package dependencies]: {{site.dart-site}}/tools/pub/dependencies

## Examples

## 例子

The following examples walk through the necessary steps for
using packages.

下面的範例將介紹使用 packages 的一些必要步驟。

### Example: Using the CSS Colors package {#css-example}

### 例子：使用 CSS Colors package {#css-example}

The [`css_colors`][] package
defines color constants for CSS colors, so use the constants
wherever the Flutter framework expects the `Color` type.

[`css_colors`][] package 為 CSS 顏色定義顏色常量，
允許你在 Flutter 框架中任何需要 `Color` 型別的地方使用它們。

To use this package:

要使用這個 package：

1. Create a new project called `cssdemo`.

   建立一個名為 `cssdemo` 的新專案

1. Open `pubspec.yaml`, and add the `css-colors` dependency:

   開啟 `pubspec.yaml`，並新增依賴 `css-colors`：
   
   ```yaml
   dependencies:
     flutter:
       sdk: flutter
   ```
   with:

   替換為：

   ```
   dependencies:
     flutter:
       sdk: flutter
     css_colors: ^1.0.0
   ```

1. Run `flutter pub get` in the terminal,
   or click **Get Packages** in VS Code.

   在命令列中執行 `flutter packages get`，
   或者點選 Intellij 中的 **Packages get**

1. Open `lib/main.dart` and replace its full contents with:

   開啟 `lib/main.dart` 並將其全部內容替換為：

    <?code-excerpt "lib/css_colors.dart (CssColors)"?>
    ```dart
    import 'package:css_colors/css_colors.dart';
    import 'package:flutter/material.dart';

    void main() {
      runApp(const MyApp());
    }

    class MyApp extends StatelessWidget {
      const MyApp({super.key});

      @override
      Widget build(BuildContext context) {
        return const MaterialApp(
          home: DemoPage(),
        );
      }
    }

    class DemoPage extends StatelessWidget {
      const DemoPage({super.key});

      @override
      Widget build(BuildContext context) {
        return Scaffold(body: Container(color: CSSColors.orange));
      }
    }
    ```


[`css_colors`]: {{site.pub-pkg}}/css_colors

1. Run the app. The app's background should now be orange.

   執行應用。當你點選 'Show Flutter homepage' 時，你將看到手機預設瀏覽器開啟並出現 Flutter 主頁。
   
### Example: Using the url_launcher package to launch the browser {#url-example}

### 例子：使用 url_launcher package 來開啟瀏覽器 {#url-example}

The [`url_launcher`][] plugin package enables opening
the default browser on the mobile platform to display
a given URL, and is supported on Android, iOS, web,
Windows, Linux, and macos.
This package is a special Dart package called a
_plugin package_ (or _plugin_),
which includes platform-specific code.

[`url_launcher`][] 外掛可以讓你在移動平臺上開啟預設瀏覽器以顯示給定的 URL。
它示範了 package 如何也可能包含特定於平台的程式碼
我們將這一類包含各平台不同程式碼的 package 稱為 **外掛 package** 或者 **外掛**。

To use this plugin:

要使用這個外掛：

1. Create a new project called `launchdemo`.

   新建一個名為 `lauchdemo` 的新專案；

1. Open `pubspec.yaml`, and add the `url_launcher` dependency:

   開啟 `pubspec.yaml`，然後新增 `url_launcher` 的依賴：
   
   ```yaml
   dependencies:
     flutter:
       sdk: flutter
     url_launcher: ^5.4.0
   ```

1. Run `flutter pub get` in the terminal,
   or click **Get Packages get** in VS Code.

   在命令列中執行 `flutter packages get`，或者
   點選 Intellij 或 Android Studio 中的 **Packages get**；

1. Open `lib/main.dart` and replace its full contents with the
   following:

   開啟 `lib/main.dart` 並將其全部內容替換為：

    <?code-excerpt "lib/url_launcher.dart (UrlLauncher)"?>
    ```dart
    import 'package:flutter/material.dart';
    import 'package:path/path.dart' as p;
    import 'package:url_launcher/url_launcher.dart';

    void main() {
      runApp(const MyApp());
    }

    class MyApp extends StatelessWidget {
      const MyApp({super.key});

      @override
      Widget build(BuildContext context) {
        return const MaterialApp(
          home: DemoPage(),
        );
      }
    }

    class DemoPage extends StatelessWidget {
      const DemoPage({super.key});

      void launchURL() {
        launchUrl(p.toUri('https://flutter.dev'));
      }

      @override
      Widget build(BuildContext context) {
        return Scaffold(
          body: Center(
            child: ElevatedButton(
              onPressed: launchURL,
              child: const Text('Show Flutter homepage'),
            ),
          ),
        );
      }
    }
    ```

1. Run the app (or stop and restart it, if it was already running
   before adding the plugin). Click **Show Flutter homepage**.
   You should see the default browser open on the device,
   displaying the homepage for flutter.dev.

   執行應用（如果你的應用在新增外掛之前已經執行，請停止並重啟應用）。
   當你點選 **Show Flutter homepage** 時，
   你將看到手機預設瀏覽器開啟並出現 Flutter 主頁。

