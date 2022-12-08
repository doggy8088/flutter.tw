---
title: Developing packages & plugins
title: Flutter Packages 的開發和提交
short-title: Developing
short-title: 開發和提交
description: How to write packages and plugins for Flutter.
description: 如何編寫和提交你的 Packages。
tags: Packages,外掛
keywords: 外掛開發,Flutter外掛課程
---

{{site.note.alert}}

  The plugin API has been updated and now supports [federated plugins][] that
  enable separation of different platform implementations. You can also now
  indicate [which platforms a plugin][supported-platforms] supports, for example
  web and macOS.

  外掛 API 現已支援 [聯合外掛][federated plugins]，從而分離在不同平臺上的實現。
  你可以指定 [哪些平台有外掛][supported-platforms] 支援，例如 Web 和 macOS。

  Eventually, the old plugin APIs will be deprecated. In the short term, you
  will see a warning when the framework detects that you are using an old-style
  plugin. For information on how to upgrade your plugin, see [Supporting the new
  Android plugins APIs][].  

  舊的外掛 API 會在將來被廢棄。如果在短期內你仍在使用舊版本的外掛 API，你會看到警告。
  想了解更多關於升級 Android 外掛的內容，請閱讀
  [支援新的 Android 外掛 API][Supporting the new Android plugins APIs]。

{{site.note.end}}

## Package introduction

## Package 介紹

Packages enable the creation of modular code that can be shared easily. A
minimal package consists of the following:

透過使用 package（的模式）可以建立易於共享的模組化程式碼。
一個最基本的 package 由以下內容構成：

**`pubspec.yaml`**
<br> A metadata file that declares the package name,
  version, author, and so on.

**`pubspec.yaml` 檔案**
<br> 用於定義 package 名稱、版本號、作者等其他資訊的元資料檔案。

**`lib`**
<br> The `lib` directory contains the public code in
  the package, minimally a single `<package-name>.dart` file.
  
**`lib` 目錄**
<br> 包含共享程式碼的 `lib` 目錄，
其中至少包含一個 `<package-name>.dart` 檔案。

{{site.alert.note}}

  For a list of dos and don'ts when writing an effective plugin,
  see the Medium article by Mehmet Fidanboylu,
  [Writing a good plugin][].
  
  有關編寫高效外掛的注意事項列表，請參考 Medium 上的文章：
  [Writing a good plugin][]。

{{site.alert.end}}

### Package types {#types}

### Package 類別 {#types}

Packages can contain more than one kind of content:

Package 包含以下幾種類別：

**Dart packages**
<br> General packages written in Dart,
  for example the [`path`][] package.
  Some of these might contain Flutter specific
  functionality and thus have a dependency on the
  Flutter framework, restricting their use to Flutter only,
  for example the [`fluro`][] package.
  
**純 Dart 庫 (Dart packages)**
<br> 用 Dart 編寫的傳統 package，比如 [`path`][]。
  其中一些可能包含 Flutter 的特定功能，因此依賴於 Flutter 框架，
  其使用範圍僅限於 Flutter，比如 [`fluro`][]。

**Plugin packages**
<br> A specialized Dart package that contains an API written in
  Dart code combined with one or more platform-specific
  implementations.

**原生外掛 (Plugin packages)**
<br> 使用 Dart 編寫的，按需使用 Java 或 Kotlin、Objective-C
  或 Swift 分別在 Android 和/或 iOS 平台實現的 package。

  Plugin packages can be written for Android (using Kotlin or Java), iOS (using
  Swift or Objective-C), web, macOS, Windows, or Linux, or any combination
  thereof.

  外掛 package 可以針對 Android（使用 Kotlin 或 Java）、
  iOS（使用 Swift 或 Objective-C）、Web、macOS、Windows 或 Linux，
  又或者它們的各種組合方式，進行編寫。

  A concrete example is the [`url_launcher`][] plugin package.
  To see how to use the `url_launcher` package, and how it
  was extended to implement support for web,
  see the Medium article by Harry Terkelsen,
  [How to Write a Flutter Web Plugin, Part 1][].
  一個較為具體的實現例子是 [`url_launcher`][] 外掛 package。
  想了解如何使用 `url_launcher` package，以及它如何擴充 Web 的實現，
  請閱讀 Medium 上由 Harry Terkelsen 撰寫的文章
  [如何編寫 Flutter Web 外掛，第一部分][How to Write a Flutter Web Plugin, Part 1]。


**FFI Plugin packages**
<br> A specialized Dart package that contains an API written in
  Dart code combined with one or more platform-specific
  implementations that use Dart FFI([Android][Android], [iOS][iOS], [macOS][macOS]).

**FFI 外掛**
<br> 用 Dart 語言編寫針對一個或多個特定平台的 API，
使用 Dart FFI ([Android][Android]、[iOS][iOS]、[macOS][macOS])。

## Developing Dart packages {#dart}

## 開發純 Dart 的 packages {#dart}

The following instructions explain how to write a Flutter
package.

下面會為你介紹如何寫 Flutter package。

### Step 1: Create the package

### 第一步：建立 package

To create a starter Flutter package,
use the `--template=package` flag with `flutter create`:

想要建立初始的 Flutter package，
請使用帶有 `--template=package` 標誌的
`flutter create` 命令：

```terminal
$ flutter create --template=package hello
```

This creates a package project in the `hello`
folder with the following content:

這將在 `hello` 目錄下建立一個 package 專案，其中包含以下內容：

**LICENSE**
<br> A (mostly) empty license text file.

**LICENSE 檔案**
<br> 大機率會是空的一個許可證檔案。

**test/hello_test.dart**
<br> The [unit tests][] for the package.

**test/hello_test.dart 檔案**
<br> Package 的 [單元測試][unit tests] 檔案。

**hello.iml**
<br> A configuration file used by the IntelliJ IDEs.

**hello.iml 檔案**
<br> 由 IntelliJ 產生的配置檔案。

**.gitignore**
<br> A hidden file that tells Git which files or
  folders to ignore in a project.

**.gitignore 檔案**
<br> 告訴 Git 系統應該隱藏哪些檔案或資料夾的一個隱藏檔案。

**.metadata**
<br> A hidden file used by IDEs to track the properties
  of the Flutter project.

**.metadata 檔案**
<br> IDE 用來記錄某個 Flutter 專案屬性的的隱藏檔案。

**pubspec.yaml**
<br> A yaml file containing metadata that specifies
  the package's dependencies. Used by the pub tool.

**pubspec.yaml 檔案**
<br> pub 工具需要使用的，包含 package 依賴的 yaml 格式的檔案。

**README.md**
<br> A starter markdown file that briefly describes
  the package's purpose.

**README.md 檔案**
<br> 起步文件，用於描述 package。

**lib/hello.dart**
<br> A starter app containing Dart code for the package.

**lib/hello.dart 檔案**
<br> package 的 Dart 實現程式碼。

**.idea/modules.xml**, **.idea/workspace.xml**
<br> A hidden folder containing configuration files
  for the IntelliJ IDEs.

**.idea/modules.xml**、**.idea/workspace.xml 檔案**
<br> IntelliJ 的各自配置檔案（包含在 .idea 隱藏資料夾下）。

**CHANGELOG.md**
<br> A (mostly) empty markdown file for tracking
  version changes to the package.

**CHANGELOG.md 檔案**
<br> 又一個大機率為空的文件，用於記錄 package 的版本變更。

### Step 2: Implement the package

### 第二步：實現 package

For pure Dart packages, simply add the functionality
inside the main `lib/<package name>.dart` file,
or in several files in the `lib` directory.

對於純 Dart 庫的 package，只要在 `lib/<package name>.dart` 檔案中新增功能實現，
或在 `lib` 目錄中的多個檔案中新增功能實現。

To test the package, add [unit tests][]
in a `test` directory.

如果要對 package 進行測試，
在 `test` 目錄下新增 [單元測試][unit tests]。

For additional details on how to organize the
package contents,
see the [Dart library package][] documentation.

關於如何組織 package 內容的更多詳細資訊，
請參考 [Dart library package][] 文件。

## Developing plugin packages {#plugin}

## 開發原生外掛型別的 packages {#plugin}

If you want to develop a package that calls into platform-specific APIs, you
need to develop a plugin package.

如果想要開發一個呼叫特定平台 API 的 package，你需要開發一個原生外掛 packgae。

The API is connected to the platform-specific implementation(s) using a
[platform channel][].

它的 API 透過 [平台通道][platform channel] 連線到平台特定的實現。

### Federated plugins

### 聯合外掛

Federated plugins are a way of splitting support for different platforms into
separate packages. So, a federated plugin can use one package for iOS, another
for Android, another for web, and yet another for a car (as an example of an IoT
device). Among other benefits, this approach allows a domain expert to extend an
existing plugin to work for the platform they know best.

Federated plugins (聯合外掛) 是一種將對不同平台的支援分為單獨的軟體套件。
所以，聯合外掛能夠使用針對 iOS、Android、Web 甚至是針對汽車
(例如在 IoT 裝置上)分別使用對應的 package。
除了這些好處之外，它還能夠讓領域專家在他們最瞭解的平臺上擴充現有平台外掛。

A federated plugin requires the following packages:

聯合外掛需要以下 package：

**app-facing package**
<br> The package that plugin users depend on to use the plugin.
  This package specifies the API used by the Flutter app.

**面向應用的 package**
<br> 該 package 是使用者使用外掛的的直接依賴。
  它指定了 Flutter 應用使用的 API。

**platform package(s)**
<br> One or more packages that contain the platform-specific
  implementation code. The app-facing package calls into
  these packages&mdash;they aren't included into an app,
  unless they contain platform-specific functionality
  accessible to the end user.

**平台 package**
<br> 一個或多個包含特定平台程式碼的 package。
  面向應用的 package 會呼叫這些平台 package&mdash;&mdash;
  除非它們帶有一些終端使用者需要的特殊平台功能，否則它們不會包含在應用中。

**platform interface package**
<br> The package that glues the app-facing packing
  to the platform package(s). This package declares an
  interface that any platform package must implement to
  support the app-facing package. Having a single package
  that defines this interface ensures that all platform
  packages implement the same functionality in a uniform way.

**平台介面 package**
<br> 將面向應用的 package 與平台 package 進行整合的 package。
  該 package 會宣告平台 package 需要實現的介面，供面向應用的 package 使用。
  使用單一的平台介面 package 可以確保所有平台 package
  都按照各自的方法實現了統一要求的功能。

#### Endorsed federated plugin

#### 整合的聯合外掛

Ideally, when adding a platform implementation to
a federated plugin, you will coordinate with the package
author to include your implementation.
In this way, the original author _endorses_ your
implementation.

理想情況下，當你在為一個聯合外掛新增某個平台的實現時，
你會與 package 的作者合作，將你的實現納入 package。

For example, say you write a `foobar_windows`
implementation for the (imaginary) `foobar` plugin.
In an endorsed plugin, the original `foobar` author
adds your Windows implementation as a dependency
in the pubspec for the app-facing package.
Then, when a developer includes the `foobar` plugin
in their Flutter app, the Windows implementation,
as well as the other endorsed implementations,
are automatically available to the app.

假設你開發了 `foobar_windows` 外掛，用於對應 `foobar` 外掛的實現。
在整合的聯合外掛裡，`foobar` 的原作者會將你的 Windows
實現作為依賴新增在 pubspec 檔案中，供面向應用的 package 呼叫。
而後在開發者使用 `foobar` 外掛時，Windows
及已包含的其他平台的實現就自動可用了。

#### Non-endorsed federated plugin

#### 未整合的聯合外掛

If you can't, for whatever reason, get your implementation
added by the original plugin author, then your plugin
is _not_ endorsed. A developer can still use your
implementation, but must manually add the plugin
to the app's pubspec file. So, the developer
must include both the `foobar` dependency _and_
the `foobar_windows` dependency in order to achieve
full functionality.

如果你的實現出於某些原因無法被原作者整合，
那麼你的外掛屬於 **未整合** 的聯合外掛。
開發者仍然可以使用你的實現，但是必須手動在 pubspec 檔案裡新增參考。
意味著開發者需要同時參考 `foobar` **和** `foobar_windows` 依賴，
才能使用對應平台的完整功能。

For more information on federated plugins,
why they are useful, and how they are
implemented, see the Medium article by Harry Terkelsen,
[How To Write a Flutter Web Plugin, Part 2][].

有關聯合外掛的更多資訊、它為什麼非常強大，以及如何實現聯合外掛，
你可以閱讀 Harry Terkelsen 在 Medium 撰寫的
[如何撰寫 Flutter Web 外掛，第 2 部分][How To Write a Flutter Web Plugin, Part 2]。

### Specifying a plugin's supported platforms {#plugin-platforms}

### 指定一個外掛支援的平台 {#plugin-platforms}

Plugins can specify the platforms they support by adding keys to the `platforms`
map in the `pubspec.yaml` file. For example, the following pubspec file shows
the `flutter:` map for the `hello` plugin, which supports only iOS and Android:

外掛可以透過向 `pubspec.yaml` 中的 `platforms` map 
新增 keys 來指定其支援的平台。
例如，以下是 `hello` 外掛的 `flutter:` map，
它僅支援 Android 和 iOS：

```yaml
flutter:
  plugin:
    platforms:
      android:
        package: com.example.hello
        pluginClass: HelloPlugin
      ios:
        pluginClass: HelloPlugin

environment:
  sdk: ">=2.1.0 <3.0.0"
  # Flutter versions prior to 1.12 did not support the
  # flutter.plugin.platforms map.
  flutter: ">=1.12.0"
```

When adding plugin implementations for more platforms, the `platforms` map
should be updated accordingly. For example, here's the map in the pubspec file
for the `hello` plugin, when updated to add support for macOS and web:

當為更多平臺新增外掛實現時，應相應地更新 `platforms` map，
例如這是支援 Android、iOS、macOS 和 web 的 `hello` 外掛的 map：

```yaml
flutter:
  plugin:
    platforms:
      android:
        package: com.example.hello
        pluginClass: HelloPlugin
      ios:
        pluginClass: HelloPlugin
      macos:
        pluginClass: HelloPlugin
      web:
        pluginClass: HelloPlugin
        fileName: hello_web.dart

environment:
  sdk: ">=2.1.0 <3.0.0"
  # Flutter versions prior to 1.12 did not support the
  # flutter.plugin.platforms map.
  flutter: ">=1.12.0"
```

#### Federated platform packages

#### 聯合平台 package

A platform package uses the same format, but includes an `implements` entry
indicating which app-facing package it is an implementation for. For example,
a `hello_windows` plugin containing the Windows implementation for `hello`
would have the following `flutter:` map:

平台 package 有著同樣的格式，但會包含 `implements` 入口，
用於指明 package 實現的平台。
例如，實現了 `hello` package 的 Windows 平台的 `hello_windows` 外掛，
會在 `flutter:` 對映下包含以下內容：

```yaml
flutter:
  plugin:
    implements: hello
    platforms:
      windows:
        pluginClass: HelloPlugin
```

#### Endorsed implementations

#### 認可的實現

An app facing package can endorse a platform package by adding a
dependency on it, and including it as a `default_package` in the
`platforms:` map. If the `hello` plugin above endorsed `hello_windows`,
it would look like this:

提供給 App 專案使用的 package
可以透過在 `platform:` 對映下宣告 `default_package`，
認可一個平台實現外掛。
如果 `hello` 外掛認可了 `hello_windows`，它看起來會是這樣：

```yaml
flutter:
  plugin:
    platforms:
      android:
        package: com.example.hello
        pluginClass: HelloPlugin
      ios:
        pluginClass: HelloPlugin
      windows:
        default_package: hello_windows

dependencies:
  hello_windows: ^1.0.0
```

Note that as shown here, an app-facing package can have
some platforms implementated within the package, and others in
endorsed federated implementations.

注意如上所示，面向 App 專案的 package 可能已經包含了某些平台的實現，
同時也有認可的其他平台的實現。

### Step 1: Create the package

### 第一步：建立 package

To create a plugin package, use the `--template=plugin`
flag with `flutter create`.

想要建立原生外掛 package，
請使用帶有 `--template=plugin` 標誌的 `flutter create` 命令。

Use the `--platforms=` option followed by a comma separated list to specify the
platforms that the plugin supports. Available platforms are: `android`, `ios`,
`web`, `linux`, `macos`, and `windows`. If no platforms are specified, the
resulting project doesn't support any platforms.

你可以使用 `--platforms=` 命令列選項指定外掛支援的平台，
後面引數是用逗號分隔的列表。
可用的平台有：`android`、`ios`、`web`、`linux`、`macos` 和 `windows`。
如果沒有指定平台，則產生的專案不支援任何平台。

Use the `--org` option to specify your organization,
using reverse domain name notation. This value is used
in various package and bundle identifiers in the
generated plugin code.

使用 `--org` 選項，以反向域名錶示法來指定你的組織。
該值用於產生的 Android 及 iOS 程式碼。

Use the `-a` option to specify the language for android or the `-i` option to
specify the language for ios. Please choose **one** of the following:

使用 `-a` 選項指定 Android 的語言，或使用 `-i` 選項指定 iOS 的語言。 
請選擇以下 **任一項**：

```terminal
$ flutter create --org com.example --template=plugin --platforms=android,ios -a kotlin hello
```
```terminal
$ flutter create --org com.example --template=plugin --platforms=android,ios -a java hello
```
```terminal
$ flutter create --org com.example --template=plugin --platforms=android,ios -i objc hello
```
```terminal
$ flutter create --org com.example --template=plugin --platforms=android,ios -i swift hello
```

This creates a plugin project in the `hello` folder
with the following specialized content:

這將在 `hello` 目錄下建立一個外掛專案，其中包含以下內容：

**`lib/hello.dart`**
<br> The Dart API for the plugin.

**`lib/hello.dart` 檔案**
<br> Dart 外掛 API 實現。

**`android/src/main/java/com/example/hello/HelloPlugin.kt`**
<br> The Android platform-specific implementation of the plugin API
  in Kotlin.

**`android/src/main/java/com/example/hello/HelloPlugin.kt` 檔案**
<br> Android 平台原生外掛 API 實現（使用 Kotlin 程式語言）。

**`ios/Classes/HelloPlugin.m`**
<br> The iOS-platform specific implementation of the plugin API
  in Objective-C.

**`ios/Classes/HelloPlugin.m` 檔案**
<br> iOS 平台原生外掛 API 實現（使用 Objective-C 程式語言）。

**`example/`**
<br> A Flutter app that depends on the plugin,
  and illustrates how to use it.

**`example/` 檔案**
<br> 一個依賴於該外掛並說明了如何使用它的 Flutter 應用。

By default, the plugin project uses Swift for iOS code and
Kotlin for Android code. If you prefer Objective-C or Java,
you can specify the iOS language using `-i` and the
Android language using `-a`. For example:

預設情況下，外掛專案中 iOS 程式碼使用 Swift 編寫，
Android 程式碼使用 Kotlin 編寫。
如果你更喜歡 Objective-C 或 Java，
你可以透過 `-i` 指定 iOS 所使用的語言和/或
使用`-a` 指定 Android 所使用的語言。比如：

```terminal
$ flutter create --template=plugin --platforms=android,ios -i objc hello
```
```terminal
$ flutter create --template=plugin --platforms=android,ios -a java hello
```

### Step 2: Implement the package {#edit-plugin-package}

### 第二步：實現 package {#edit-plugin-package}

As a plugin package contains code for several platforms
written in several programming languages,
some specific steps are needed to ensure a smooth experience.

由於原生外掛型別的 package 包含了使用多種程式語言編寫的多個平台程式碼，
因此需要一些特定步驟來保證體驗的流暢性。

#### Step 2a: Define the package API (.dart)

#### 步驟 2a：定義 package API（.dart）

The API of the plugin package is defined in Dart code.
Open the main `hello/` folder in your favorite [Flutter editor][].
Locate the file `lib/hello.dart`.

原生外掛型別 package 的 API 在 Dart 程式碼中要首先定義好，
使用你鍾愛的 [Flutter 編輯器][Flutter editor]，
開啟 `hello` 主目錄，並找到 `lib/hello.dart` 檔案。

#### Step 2b: Add Android platform code (.kt/.java)

#### 步驟 2b：新增 Android 平台程式碼（.kt/.java）

We recommend you edit the Android code using Android Studio.

我們建議你使用 Android Studio 來編輯 Android 程式碼。

Then use the following steps:

接下來進行如下步驟：

1. Launch Android Studio.

   啟動 Android Studio；
   
1. Select **Open an existing Android Studio Project** 
   in the **Welcome to Android Studio** dialog,
   or select **File > Open** from the menu,
   and select the `hello/example/android/build.gradle` file.

   在 Android Studio 的歡迎選單 (**Welcome to Android Studio**) 對話方塊中
   選擇開啟現有的 Android Studio 專案
   (**Open an existing Android Studio Project**)，
   或在選單中選擇 **File > Open**，
   然後選擇 `hello/example/android/build.gradle` 檔案；

1. In the **Gradle Sync** dialog, select **OK**.

    在**Gradle Sync** 對話方塊中，選擇 **OK**；
   
1. In the **Android Gradle Plugin Update** dialog,
   select **Don't remind me again for this project**.

   在“Android Gradle Plugin Update”對話方塊中，
   選擇“Don't remind me again for this project”。

The Android platform code of your plugin is located in
`hello/java/com.example.hello/HelloPlugin`.

外掛中與 Android 系統徐相關的程式碼在
`hello/java/com.example.hello/HelloPlugin` 這個檔案裡。

You can run the example app from Android Studio by
pressing the run (&#9654;) button.

你可以在 Android Studio 中點選執行 &#9654; 按鈕來執行範例程式。

#### Step 2c: Add iOS platform code (.swift/.h+.m)

#### 步驟 2c：新增 iOS 平台程式碼（.swift/.h+.m）

We recommend you edit the iOS code using Xcode.

我們建議你使用 Xcode 來編輯 iOS 程式碼。

Before editing the iOS platform code in Xcode,
first make sure that the code has been built at least once
(in other words, run the example app from your IDE/editor,
or in a terminal execute
`cd hello/example; flutter build ios --no-codesign`).

使用 Xcode 編輯 iOS 平台程式碼之前，首先確保程式碼至少被建構過一次
（即從 IDE/編輯器執行範例程式，或在終端中執行以下命令：
`cd hello/example; flutter build ios --no-codesign`）。

Then use the following steps:

接下來執行下面步驟：

1. Launch Xcode.

   啟動 Xcode
   
1. Select **File > Open**, and select the
   `hello/example/ios/Runner.xcworkspace` file.

   選擇“File > Open”，
   然後選擇 `hello/example/ios/Runner.xcworkspace` 檔案。

The iOS platform code for your plugin is located in
`Pods/Development Pods/hello/../../example/ios/.symlinks/plugins/hello/ios/Classes`
in the Project Navigator.

外掛的 iOS 平台程式碼位於專案導航中的這個位置：
`Pods/Development Pods/hello/../../example/ios/.symlinks/plugins/hello/ios/Classes`。

You can run the example app by pressing the run (&#9654;) button.

你可以點選執行 &#9654; 按鈕來執行範例程式。

#### Step 2d: Connect the API and the platform code

#### 步驟 2d：關聯 API 和平台程式碼

Finally, you need to connect the API written in Dart code with
the platform-specific implementations.
This is done using a [platform channel][],
or through the interfaces defined in a platform
interface package.

最後，你需要將 Dart 編寫的 API 程式碼與特定平台的實現相互關聯。
這是透過 [平台通道][platform channel] 完成的。

### Add support for platforms in an existing plugin project

### 為現有的外掛專案加入平台的支援

To add support for specific platforms to an existing plugin project, run `flutter create` with
the `--template=plugin` flag again in the project directory.
For example, to add web support in an existing plugin, run:

要在現有的外掛專案中新增對特定平台的支援，
請在專案目錄執行 `flutter create` 命令，並加入 `--template=plugin`。
例如，要對現有的外掛專案新增 Web 支援，請執行以下命令。

```terminal
$ flutter create --template=plugin --platforms=web .
```

If this command displays a message about updating the `pubspec.yaml` file,
follow the provided instructions.

如果這個命令返回了一個關於需要更新 `pubspec.yaml` 檔案的提醒，
請按照提示的說明進行操作。

### Dart platform implementations

### Dart 的平台實現

In many cases, non-web platform implementations only use the
platform-specific implementation language, as shown above. However,
platform implementations can also use platform-specific Dart as well.

在很多場景中，非 web 平台的實現僅僅使用了上述的平台特定語言。
然而，Dart 也是平台特定的語言之一。

{{site.alert.note}}

  The examples below only apply to non-web platforms. Web
  plugin implementations are always written in Dart, and use
  `pluginClass` and `fileName` for their Dart implementations
  as shown above.

  下方的例子僅適用於非 web 平台。
  Web 平台的外掛是用 Dart 編寫的，
  透過 `pluginClass` 和 `fileName` 來指定實現。

{{site.alert.end}}

#### Dart-only platform implementations

#### 純 Dart 平台的實現

In some cases, some platforms can be
implemented entirely in Dart (for example, using FFI).
For a Dart-only platform implementation on a platform other than web,
replace the `pluginClass` in pubspec.yaml with a `dartPluginClass`.
Here is the `hello_windows` example above modified for a
Dart-only implementation:

如先前描述，通常外掛會使用第二種語言，實現對應平台的功能。
然而，在某些場景下，部分平台可能會完全使用 Dart 進行實現（例如使用 FFI）。
若需要僅 Dart 的平台實現，你可以將 pubspec.yaml 裡的
`pluginClass` 替換為 `dartPluginClass`。
下面是 `hello_windows` 範例替換為僅 Dart 實現的程式碼：

```yaml
flutter:
  plugin:
    implements: hello
    platforms:
      windows:
        dartPluginClass: HelloPluginWindows
```

In this version you would have no C++ Windows code, and would instead
subclass the `hello` plugin's Dart platform interface class with a
`HelloPluginWindows` class that includes a static `registerWith()` method.
This method will be called during startup, and can be used to register the
Dart implementation:

在這樣的模式下，外掛內不包含 Windows 的 C++ 程式碼，
它將繼承 `hello` 外掛的 Dart 平台介面，使用包含靜態 `registerWith()`
方法的 `HelloPluginWindows` 類進行實現。
該方法會在啟動時呼叫，用於註冊 Dart 實現：

```dart
class HelloPluginWindows extends HelloPluginPlatform {
  /// Registers this class as the default instance of [HelloPluginPlatform].
  static void registerWith() {
    HelloPluginPlatform.instance = HelloPluginWindows();
  }
```

#### Hybrid platform implementations

#### 混合平台的實現

Platform implementations can also use both Dart and a platform-specific
language. For example, a plugin could use a different platform channel
for each platform so that the channels can be customized per platform.

平台實現可能同時會使用 Dart 以及某個特定平台的語言。
例如，plugin 可能會在不同平台使用不同的 platform channel，
這樣 channel 就可以根據不同平台進行客製。

A hybrid implementation uses both of the registration systems
described above. Here is the `hello_windows` example above modified for a
hybrid implementation:

就和之前說的那樣，混合實現將會使用多種註冊方式。
這裡有一個使用混合實現的 `hello_windows` 範例:

```yaml
flutter:
  plugin:
    implements: hello
    platforms:
      windows:
        dartPluginClass: HelloPluginWindows
        pluginClass: HelloPlugin
```

The Dart `HelloPluginWindows` class would use the `registerWith()`
shown above for Dart-only implementations, while the C++ `HelloPlugin`
class would be the same as in a C++-only implementation.

Dart 類 `HelloPluginWindows` 會使用 `registerWith()` 方法做純 Dart 的實現，
`HelloPlugin` 類則用來做純 C++ 程式碼的實現。

### Testing your plugin

### 測試你的外掛

We encourage you test your plugin with automated tests, to ensure that
functionality does not regress as you make changes to your code. For more
information, see [Testing your plugin][], a section in [Supporting the new
Android plugins APIs][].

我們鼓勵您使用自動化測試來測試您的外掛，以確保程式碼在修改時候功能保持完整。
更多資訊，請參見文件
[支援新的 Android 的 API][Supporting the new Android plugins APIs]
中關於 [測試你的外掛][Testing your plugin] 這個小節。

## Developing FFI plugin packages {#plugin-ffi}

If you want to develop a package that calls into native APIs using
Dart's FFI, you need to develop an FFI plugin package.

Both FFI plugin packages and (non-FFI) plugin packages support
bundling native code, but FFI plugin packages do not support
method channels and do include method channel registration code.
If you want to implement a plugin that uses both method channels
and FFI, use a (non-FFI) plugin. You can chose per platform to
use an FFI or (non-FFI) plugin.

FFI plugin packages were introduced in Flutter 3.0, if you're
targeting older Flutter versions, you can use a (non-FFI) plugin.

### Step 1: Create the package

To create a starter FFI plugin package,
use the `--template=plugin_ffi` flag with `flutter create`:

```terminal
$ flutter create --template=plugin_ffi hello
```

This creates a FFI plugin project in the `hello`
folder with the following specialized content:

**lib**: The Dart code that defines the API of the plugin, and which
calls into the native code using `dart:ffi`.

**src**: The native source code, and a `CmakeFile.txt` file for building
that source code into a dynamic library.

**platform folders** (`android`, `ios`, `windows`, etc.): The build files
for building and bundling the native code library with the platform application.

### Step 2: Building and bundling native code

The `pubspec.yaml` specifies FFI plugins as follows:

```yaml
  plugin:
    platforms:
      some_platform:
        ffiPlugin: true
```

This configuration invokes the native build for the various target platforms
and bundles the binaries in Flutter applications using these FFI plugins.

This can be combined with `dartPluginClass`, such as when FFI is used for the
implementation of one platform in a federated plugin:

```yaml
  plugin:
    implements: some_other_plugin
    platforms:
      some_platform:
        dartPluginClass: SomeClass
        ffiPlugin: true
```

A plugin can have both FFI and method channels:

```yaml
  plugin:
    platforms:
      some_platform:
        pluginClass: SomeName
        ffiPlugin: true
```

The native build systems that are invoked by FFI (and method channels) plugins are:

* For Android: Gradle, which invokes the Android NDK for native builds.
  * See the documentation in `android/build.gradle`.
* For iOS and MacOS: Xcode, via CocoaPods.
  * See the documentation in `ios/hello.podspec`.
  * See the documentation in `macos/hello.podspec`.
* For Linux and Windows: CMake.
  * See the documentation in `linux/CMakeLists.txt`.
  * See the documentation in `windows/CMakeLists.txt`.

### Step 3: Binding to native code

To use the native code, bindings in Dart are needed.

To avoid writing these by hand, they are generated from the header file
(`src/hello.h`) by [`package:ffigen`][].
Regenerate the bindings by running:

```terminal
$  flutter pub run ffigen --config ffigen.yaml
```

### Step 4: Invoking native code

Very short-running native functions can be directly invoked from any isolate.
For an example, see `sum` in `lib/hello.dart`.

Longer-running functions should be invoked on a [helper isolate][] to avoid
dropping frames in Flutter applications.
For an example, see `sumAsync` in `lib/hello.dart`.

## Adding documentation

## 新增文件

It is recommended practice to add the following documentation
to all packages:

建議將下列文件新增到所有 package 中：

1. A `README.md` file that introduces the package

   `README.md` 檔案用來對 package 進行介紹
   
1. A `CHANGELOG.md` file that documents changes in each version
 
   `CHANGELOG.md` 檔案用來記錄每個版本的更改

1. A [`LICENSE`] file containing the terms under which the package
   is licensed

   [`LICENSE`][] 檔案用來闡述 package 的許可條款
   
1. API documentation for all public APIs (see below for details)

   API 文件包含所有的公共 API（詳情參見下文）

### API documentation

### API 文件

When you publish a package,
API documentation is automatically generated and
published to pub.dev/documentation.
For example, see the docs for [`device_info`][].

當你提交一個 package 時，會自動產生 API 文件並將其提交到 
pub.dev/documentation，範例請參見 [`device_info`][] 文件。


If you wish to generate API documentation locally on
your development machine, use the following commands:

如果你希望在本地開發環境中產生 API 文件，可以使用以下命令：

<ol markdown="1">
<li markdown="1"><t>Change directory to the location of your package:</t><t>將當前工作目錄切換到 package 所在目錄：</t>

```terminal
cd ~/dev/mypackage
```
</li>

<li markdown="1"><t>Tell the documentation tool where the
    Flutter SDK is located (change the following commands to reflect
    where you placed it):</t><t>告知文件工具 Flutter SDK 所在位置（請自行更改 Flutter SDK 該在的位置）</t>

```terminal
   export FLUTTER_ROOT=~/dev/flutter  # on macOS or Linux (適用於 macOS 或 Linux 作業系統)

   set FLUTTER_ROOT=~/dev/flutter     # on Windows (適用於 Windows 作業系統)
```
</li>

<li markdown="1"><t>Run the `dartdoc` tool
    (included as part of the Flutter SDK), as follows:</t><t>執行 `dartdoc` 工具（已經包含到 Flutter SDK 了）：</t>

```terminal
   $FLUTTER_ROOT/bin/cache/dart-sdk/bin/dartdoc   # on macOS or Linux (適用於 macOS 或 Linux 作業系統)

   %FLUTTER_ROOT%\bin\cache\dart-sdk\bin\dartdoc  # on Windows (適用於 Windows 作業系統)
```
</li>
</ol>

For tips on how to write API documentation, see
[Effective Dart Documentation][].

關於如何編寫 API 文件的建議，請參閱 
[高效 Dart 指南][Effective Dart Documentation]。

### Adding licenses to the LICENSE file

### 將許可證新增到 LICENSE 檔案中

Individual licenses inside each LICENSE file should
be separated by 80 hyphens on their own on a line.

每個 LICENSE 檔案中的各個許可證應由 80 個短線字元組成的線段進行分割。

If a LICENSE file contains more than one component license,
then each component license must start with the names of the
packages to which the component license applies,
with each package name on its own line, and the
list of package names separated from the actual
license text by a blank line.
(The packages need not match the names of the pub package.
For example, a package might itself contain code from
multiple third-party sources, and might need to include
a license for each one.)

如果 LICENSE 檔案中包含多個元件許可證，那麼每個元件許可證必須以其所在 package
的名稱開始，每個 package 名稱單獨一行顯示，並且 package
名稱列表與實際許可證內容由空行隔開。（package 名稱則需與 pub package 相匹配。
比如，一個 package 可能包含多個第三方程式碼，並且可能需要為每個 package 新增許可證。）

The following example shows a well-organized license file:

如下是一些優秀的許可證檔案：

```none
package_1

<some license text>

--------------------------------------------------------------------------------
package_2

<some license text>
```

Here is another example of a well-organized license file:

這些也是可以的：

```none
package_1

<some license text>

--------------------------------------------------------------------------------
package_1
package_2

<some license text>
```

Here is an example of a poorly-organized license file:

這些是一些不太好的範例：

```none
<some license text>

--------------------------------------------------------------------------------
<some license text>
```

Another example of a poorly-organized license file:

這也是一些不太好的範例：

```
package_1

<some license text>
--------------------------------------------------------------------------------
<some license text>
```

## Publishing your package {#publish}

## 提交 package {#publish}

{{site.alert.tip}}

  Have you noticed that some of the packages and plugins
  on pub.dev are designated as [Flutter Favorites][]?
  These are the packages published by verified developers
  and are identified as the packages and plugins you
  should first consider using when writing your app.
  To learn more,
  see the [Flutter Favorites program][].
  
  你是否注意到一些 package 和外掛旁邊的 [Flutter Favorites][] 標識？
  這是官方挑選出的、由認證的開發者釋出的 packages，
  並建議 Flutter 開發者們需要使用時首要考慮的 package。
  瞭解更多 [Flutter Favorites 專案][Flutter Favorites program]。

{{site.alert.end}}

Once you have implemented a package, you can publish it on
[pub.dev][], so that other developers can easily use it.

一旦完成了 package 的實現，你便可以將其提交到 [pub.dev][]
上，以便其他開發者可以輕鬆地使用它。

Prior to publishing, make sure to review the `pubspec.yaml`,
`README.md`, and `CHANGELOG.md` files to make sure their
content is complete and correct. Also, to improve the
quality and usability of your package (and to make it
more likely to achieve the status of a Flutter Favorite),
consider including the following items:

釋出你的 package 之前，確保檢查了這幾個檔案：`pubspec.yaml`、`README.md` 和
`CHANGELOG.md`，確保它們完整且正確，另外，為了提高 package 的可用性，
可以考慮加入如下的內容：

* Diverse code usage 

  程式碼的範例用法
  
* Screenshots, animated gifs, or videos

  螢幕截圖，GIF 動畫或者影片
 
* A link to the corresponding code repository

  程式碼庫的正確指向連結

Next, run the publish command in `dry-run` mode
to see if everything passes analysis:

接下來，執行 dry-run 命令以檢驗是否所有內容都通過了分析：

```terminal
$ flutter pub publish --dry-run
```

The next step is publishing to pub.dev,
but be sure that you are ready because
[publishing is forever][]:

最後一步是釋出，請注意：[釋出是永久性][publishing is forever] 的，
執行以下提交命令：

```terminal
$ flutter pub publish
```
{{site.note.alert}}
設定了中國映象的開發者們請注意：
目前所存在的映象都不能（也不應該）進行 package 的上傳。
如果你設定了映象，執行上述釋出程式碼可能會造成釋出失敗。
網路設定好後，無需取消中文映象，執行下述程式碼可直接上傳：

```terminal
$ flutter pub publish --server=https://pub.dartlang.org
```
{{site.note.end}}

For more details on publishing, see the
[publishing docs][] on dart.dev.

有關提交的詳細資訊，請查閱關於 Pub 站點的 [提交文件][publishing docs]。

## Handling package interdependencies {#dependencies}

## Package 依賴處理 {#dependencies}

If you are developing a package `hello` that depends on
the Dart API exposed by another package, you need to add
that package to the `dependencies` section of your
`pubspec.yaml` file. The code below makes the Dart API
of the `url_launcher` plugin available to `hello`:

如果你正在開發的 `hello` 依賴於另外一個 package 所公開的 Dart API，
你需要將該 package 新增到檔案 `pubspec.yaml` 的 `dependencies` 段中。
以下程式碼使得外掛 `url_launcher` 的 Dart API 在 `hello` 中可用：

```yaml
dependencies:
  url_launcher: ^5.0.0
```

You can now `import 'package:url_launcher/url_launcher.dart'`
and `launch(someUrl)` in the Dart code of `hello`.

現在你可以在 `hello` 的 Dart 程式碼中使用
`import 'package:url_launcher/url_launcher.dart'` 和 `launch(someUrl)`。

This is no different from how you include packages in
Flutter apps or any other Dart project.

這與你在 Flutter 應用或其他任何 Dart 專案中引入 package 的方式沒什麼區別。

But if `hello` happens to be a _plugin_ package
whose platform-specific code needs access
to the platform-specific APIs exposed by `url_launcher`,
you also need to add suitable dependency declarations
to your platform-specific build files, as shown below.

但碰巧 `hello` 是一個 **原生外掛** package，其特定的平台程式碼如果需要存取 `url_launcher`
所公開的平台特定 API，那麼還需要為特定平台的建構檔案新增適當的依賴說明，如下所示：

### Android

The following example sets a dependency for
`url_launcher` in `hello/android/build.gradle`:

在 `hello/android/build.gradle` 檔案中為 `url_launcher` 外掛設定依賴關係。

```groovy
android {
    // lines skipped
    dependencies {
        compileOnly rootProject.findProject(":url_launcher")
    }
}
```

You can now `import io.flutter.plugins.urllauncher.UrlLauncherPlugin`
and access the `UrlLauncherPlugin`
class in the source code at `hello/android/src`.

現在你可以在 `hello/android/src` 目錄下的原始碼檔案中使用
`import io.flutter.plugins.urllauncher.UrlLauncherPlugin`
並存取檔案 `UrlLauncherPlugin`。

For more information on `build.gradle` files, see the
[Gradle Documentation][] on build scripts.

如果希望瞭解更多有關 `build.gradle` 檔案更多的資訊，請參閱
[Gradle 文件][Gradle Documentation] 瞭解建構指令碼。

### iOS

The following example sets a dependency for
`url_launcher` in `hello/ios/hello.podspec`:

在 `hello/ios/hello.podspec` 檔案中為 `url_launcher` 外掛設定依賴關係。

```ruby
Pod::Spec.new do |s|
  # lines skipped
  s.dependency 'url_launcher'
```
You can now `#import "UrlLauncherPlugin.h"` and
access the `UrlLauncherPlugin` class in the source code
at `hello/ios/Classes`.

現在你可以在 `hello/ios/Classes` 目錄下的原始碼檔案中使用 `#import "UrlLauncherPlugin.h"` 
並存取 `UrlLauncherPlugin` 這個類了。

For additional details on `.podspec` files, see the
[CocoaPods Documentation][] on them.

如果希望瞭解更多有關 `.podspec` 檔案更多的資訊，請參閱
[CocoaPods 文件][CocoaPods Documentation] 瞭解。

### Web

All web dependencies are handled by the `pubspec.yaml`
file like any other Dart package.

與其他的 Dart package 一樣，
所有的 Web 依賴都由檔案 `pubspec.yaml` 來處理。

{% comment %}
<!-- Remove until we have better text. -->
### MacOS

PENDING
{% endcomment %}

[CocoaPods Documentation]: https://guides.cocoapods.org/syntax/podspec.html
[Dart library package]: {{site.dart-site}}/guides/libraries/create-library-packages
[`device_info`]: {{site.pub-api}}/device_info/latest
[Effective Dart Documentation]: {{site.dart-site}}/guides/language/effective-dart/documentation
[federated plugins]: #federated-plugins
[Android]: {{site.url}}/development/platform-integration/android/c-interop
[iOS]: {{site.url}}/development/platform-integration/ios/c-interop
[macOS]: {{site.url}}/development/platform-integration/macos/c-interop
[`fluro`]: {{site.pub}}/packages/fluro
[Flutter editor]: {{site.url}}/get-started/editor
[Flutter Favorites]: {{site.pub}}/flutter/favorites
[Flutter Favorites program]: {{site.url}}/development/packages-and-plugins/favorites
[Gradle Documentation]: https://docs.gradle.org/current/userguide/tutorial_using_tasks.html
[helper isolate]: {{site.dart-site}}/guides/language/concurrency#background-workers
[How to Write a Flutter Web Plugin, Part 1]: {{site.flutter-medium}}/how-to-write-a-flutter-web-plugin-5e26c689ea1
[How To Write a Flutter Web Plugin, Part 2]: {{site.flutter-medium}}/how-to-write-a-flutter-web-plugin-part-2-afdddb69ece6
[issue #33302]: {{site.repo.flutter}}/issues/33302
[`LICENSE`]: #adding-licenses-to-the-license-file
[`path`]: {{site.pub}}/packages/path
[`package:ffigen`]: {{site.pub}}/packages/ffigen
[platform channel]: {{site.url}}/development/platform-integration/platform-channels
[pub.dev]: {{site.pub}}
[publishing docs]: {{site.dart-site}}/tools/pub/publishing
[publishing is forever]: {{site.dart-site}}/tools/pub/publishing#publishing-is-forever
[Supporting the new Android plugins APIs]: {{site.url}}/development/packages-and-plugins/plugin-api-migration
[supported-platforms]: #plugin-platforms
[test your plugin]: #testing-your-plugin
[Testing your plugin]: {{site.url}}/development/platform-integration/android/plugin-api-migration#testing-your-plugin
[unit tests]: {{site.url}}/testing#unit-tests
[`url_launcher`]: {{site.pub}}/packages/url_launcher
[Writing a good plugin]: {{site.flutter-medium}}/writing-a-good-flutter-plugin-1a561b986c9c
