---
title: "Flutter and the pubspec file"
title: "Pubspec 檔案的 Flutter 部分"
description: "Describes the Flutter-only fields in the pubspec file."
description: "描述了 pubspec 檔案中與 Flutter 相關的部分。"
---

{{site.alert.note}}

  This page is primarily aimed at folks who write
  Flutter apps. If you write packages or plugins,
  (perhaps you want to create a federated plugin),
  you should check out the
  [Developing packages and plugins][] page.

  本頁主要是針對編寫 Flutter 應用程式的人。
  如果你寫 package 或外掛，（也許你想建立一個聯合外掛），
  你應該檢視 [開發 package 和外掛][Developing packages and plugins] 頁面。

{{site.alert.end}}

Every Flutter project includes a `pubspec.yaml` file,
often referred to as _the pubspec_.
A basic pubspec is generated when you create
a new Flutter project. It's located at the top
of the project tree and contains metadata about
the project that the Dart and Flutter tooling
needs to know. The pubspec is written in
[YAML][], which is human readable, but be aware
that _white space (tabs v spaces) matters_.

每個 Flutter 專案都包含一個 `pubspec.yaml` 檔案，通常被稱為 **pubspec**。
當你建立一個新的 Flutter 專案時，會產生一個基本的 pubspec。
它位於專案的根目錄，包含 Dart 和 Flutter 工具需要了解的專案元資料。
pubspec 是用 [YAML][] 寫的，它具有可讀性，
但要注意 **縮排符號（製表符和空格）很重要**。

[YAML]: https://yaml.org/

The pubspec file specifies dependencies
that the project requires, such as particular packages
(and their versions), fonts, or image files.
It also specifies other requirements, such as
dependencies on developer packages (like
testing or mocking packages), or particular
constraints on the version of the Flutter SDK.

pubspec 檔案指定了專案所需的依賴，
如特定的 package（及其版本）、字型或影象檔案。
它還指定了其他配置，如對開發者 package 的依賴（如測試或模擬 package），
或對 Flutter SDK 版本的特殊限制。

Fields common to both Dart and Flutter projects
are described in [the pubspec file][] on [dart.dev][].
This page lists _Flutter-specific_ fields
that are only valid for a Flutter project.

Dart 和 Flutter 專案共有的欄位在
[dart.dev][] 的 [pubspec 檔案][the pubspec file] 中描述。
本頁列出了只對 Flutter 專案有效的 **Flutter 特定的** 欄位。

{{site.alert.note}}

  The first time you build your project, it
  creates a `pubspec.lock` file that contains
  specific versions of the included packages.
  This ensures that you get the same version
  the next time the project is built.

  在你第一次建構你的專案時，它會建立一個 `pubspec.lock` 檔案，
  其中包含了所匯入 package 的特定版本。
  這可以確保你在下次建構專案時得到相同的版本。

{{site.alert.end}}

[the pubspec file]: {{site.dart-site}}/tools/pub/pubspec
[dart.dev]: {{site.dart-site}}

When you create a new project with the
`flutter create` command (or by using the
equivalent button in your IDE), it creates
a pubspec for a basic Flutter app.

當你用 `flutter create` 命令建立一個新專案時
（或透過使用你的 IDE 中的相應按鈕），
它會為每一個 Flutter 應用程式建立 pubspec。

Here is an example of a Flutter project pubspec file.
The Flutter only fields are highlighted.

下面是一個 Flutter 專案的 pubspec 檔案的範例。
只有 Flutter 可用的欄位會高亮顯示。

<div class="righthighlight">
{% prettify dart %}
name: <project name>
description: A new Flutter project.

publish_to: 'none'

version: 1.0.0+1

environment:
  sdk: ">=2.7.0 <3.0.0"

dependencies:
  [[highlight]]flutter:[[/highlight]]       # Required for every Flutter project
    [[highlight]]sdk: flutter[[/highlight]] # Required for every Flutter project

  [[highlight]]cupertino_icons: ^1.0.2[[/highlight]] # Only required if you use Cupertino (iOS style) icons

dev_dependencies:
  [[highlight]]flutter_test:[[/highlight]]
    [[highlight]]sdk: flutter[[/highlight]] # Required for a Flutter project that includes tests

[[highlight]]flutter:[[/highlight]]

  [[highlight]]uses-material-design: true[[/highlight]] # Required if you use the Material icon font

  [[highlight]]assets:[[/highlight]]  # Lists assets, such as image files
    [[highlight]]- images/a_dot_burr.jpeg[[/highlight]]
    [[highlight]]- images/a_dot_ham.jpeg[[/highlight]]

  [[highlight]]fonts:[[/highlight]]              # Required if your app uses custom fonts
    [[highlight]]- family: Schyler[[/highlight]]
      [[highlight]]fonts:[[/highlight]]
        [[highlight]]- asset: fonts/Schyler-Regular.ttf[[/highlight]]
        [[highlight]]- asset: fonts/Schyler-Italic.ttf[[/highlight]]
          [[highlight]]style: italic[[/highlight]]
    [[highlight]]- family: Trajan Pro[[/highlight]]
      [[highlight]]fonts:[[/highlight]]
        [[highlight]]- asset: fonts/TrajanPro.ttf[[/highlight]]
        [[highlight]]- asset: fonts/TrajanPro_Bold.ttf[[/highlight]]
          [[highlight]]weight: 700[[/highlight]]
{% endprettify %}
</div>

## Assets

## 資源

Common types of assets include static data
(for example, JSON files), configuration files,
icons, and images (JPEG, WebP, GIF,
animated WebP/GIF, PNG, BMP, and WBMP).

常見的資源包括靜態資料（例如 JSON 檔案）、
配置檔案、圖示和影象（JPEG、WebP、GIF、
動畫 WebP/GIF、PNG、BMP 和 WBMP）。

Besides listing the images that are included in the
app package, an image asset can also refer to one or more
resolution-specific "variants". For more information,
see the [resolution aware][] section of the
[Assets and images][] page.
For information on adding assets from package
dependencies, see the
[asset images in package dependencies][]
section in the same page.

除了列出應用 package 中包含的圖片，
一個圖片資源還可以參考一個或多個特定解析度的「變體」。
想要了解更多資訊，請參閱 [資源和影象][Assets and images]
頁面的 [解析度相關][resolution aware] 部分。
關於從 package 的依賴關係中新增資源的資訊，
見同一頁的 [package 依賴關係中的圖片資源][asset images in package dependencies] 部分。

[Assets and images]: {{site.url}}/development/ui/assets-and-images
[asset images in package dependencies]: {{site.url}}/development/ui/assets-and-images#from-packages
[resolution aware]: {{site.url}}/development/ui/assets-and-images#resolution-aware

## Fonts

## 字型

As shown in the above example,
each entry in the fonts section should have a
`family` key with the font family name,
and a `fonts` key with a list specifying the
asset and other descriptors for the font.

如上例所示，字型部分的每個條目都應該有一個
包含字型家族名稱的 `family` 鍵，
以及一個包含指定字型的資源和其他描述符的 `fonts` 鍵。

For examples of using fonts
see the [Use a custom font][] and
[Export fonts from a package][] recipes in the
[Flutter cookbook][].

關於使用字型的例子，請參見 [Flutter 實用課程][Flutter cookbook] 中的
[使用自訂字型][Use a custom font] 和
[從 package 中匯出字型][Export fonts from a package] 課程。

[Export fonts from a package]: {{site.url}}/cookbook/design/package-fonts
[Flutter cookbook]: {{site.url}}/cookbook
[Use a custom font]: {{site.url}}/cookbook/design/fonts

## More information

## 更多資訊

For more information on packages, plugins,
and pubspec files, see the following:

要檢視更多有關 package、外掛和 pubspec 的資訊，
請參考下面文件：

* [Creating packages][] on dart.dev

  dart.dev 上介紹的 [建立 package][Creating packages]

* [Glossary of package terms][] on dart.dev

  dart.dev 上介紹的 [package 的術語表][Glossary of package terms]
  
* [Package dependencies][] on dart.dev
  
  dart.dev 上介紹的 [package 的依賴][Package dependencies]
  
* [Using packages][]
  
  dart.dev 上介紹的 [使用 package][Using packages]

* [What not to commit][] on dart.dev

  dart.dev 上介紹的 [避擴音交的內容][What not to commit]

[Creating packages]: {{site.dart-site}}/guides/libraries/create-library-packages
[Developing packages and plugins]: {{site.url}}/development/packages-and-plugins/developing-packages
[Federated plugins]: {{site.url}}/development/packages-and-plugins/developing-packages#federated-plugins
[Glossary of package terms]: {{site.dart-site}}/tools/pub/glossary
[Package dependencies]: {{site.dart-site}}/tools/pub/dependencies
[Using packages]: {{site.url}}/development/packages-and-plugins/using-packages
[What not to commit]: {{site.dart-site}}/guides/libraries/private-files#pubspeclock
