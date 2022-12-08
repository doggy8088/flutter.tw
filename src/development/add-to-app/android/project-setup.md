---
title: Integrate a Flutter module into your Android project
title: 將 Flutter module 整合到 Android 專案
short-title: Integrate Flutter
short-title: 整合 Flutter
description: Learn how to integrate a Flutter module into your existing Android project.
description: 瞭解如何將 Flutter module 整合到你現有的 Android 專案中。
tags: Flutter混合工程,add2app
keywords: Android,專案整合
---

Flutter can be embedded into your existing Android
application piecemeal, as a source code Gradle
subproject or as AARs.

Flutter 可以作為 Gradle 子專案原始碼或者 AAR 嵌入到現有的 Android 應用程式中。

The integration flow can be done using the Android Studio
IDE with the [Flutter plugin][] or manually.

開發者可以使用帶有 [Flutter 外掛][Flutter plugin] 
的 Android Studio 或手動完成整個整合流程。

{{site.alert.warning}}

  Your existing Android app may support architectures such as `mips`
  or `x86`. Flutter currently [only supports][]
  building ahead-of-time (AOT) compiled libraries
  for `x86_64`, `armeabi-v7a` and `arm64-v8a`.

  你目前現有的 Android 專案可能支援 `mips`  或 `x86` 之類別的架構，
  然而，Flutter [當前僅支援][only supports]
  為 `x86_64`，`armeabi-v7a` 和 `arm64-v8a` 建構預編（AOT）的函式庫。

  Consider using the [`abiFilters`][] Android Gradle
  Plugin API to limit the supported architectures in your APK.
  Doing this avoids a missing `libflutter.so` runtime crash,
  for example:

  可以考慮使用 `abiFilters` 這個 Android Gradle 外掛 API 
  來指定 APK 中支援的架構，從而避免 `libflutter.so` 無法產生而導致應用執行時崩潰，
  具體操作如下：

<!--code-excerpt "MyApp/app/build.gradle" title-->
```gradle
android {
  //...
  defaultConfig {
    ndk {
      // Filter for architectures supported by Flutter.
      abiFilters 'armeabi-v7a', 'arm64-v8a', 'x86_64'
    }
  }
}
```

  The Flutter engine has an `x86` and `x86_64` version.
  When using an emulator in debug Just-In-Time (JIT) mode,
  the Flutter module still runs correctly.

  Flutter 引擎支援 `x86` 和 `x86_64` 的版本，
  在模擬器以 debug 即時編譯 (JIT) 模式執行時，
  Flutter 模組仍可以正常執行。

{{site.alert.end}}

## Using Android Studio

## 使用 Android Studio

The Android Studio IDE is a convenient way of integrating
your Flutter module automatically. With Android Studio,
you can co-edit both your Android code and your Flutter code
in the same project. You can also continue to use your normal
IntelliJ Flutter plugin functionalities such as Dart code
completion, hot reload, and widget inspector.

直接使用 Android Studio 是在現有應用中自動整合 Flutter 模組比較便捷的方法。 
在 Android Studio 中，你可以在一個專案中同時編寫 Android 程式碼和 Flutter 程式碼，
還可以繼續使用各種常用的 IntelliJ Flutter 外掛功能，
例如 Dart 程式碼自動自動完成、熱重載和 widget 檢查器等。

Add-to-app flows with Android Studio are only supported on
Android Studio 3.6 with version 42+ of the [Flutter plugin][]
for IntelliJ. The Android Studio integration also only
supports integrating using a source code Gradle subproject,
rather than using AARs. See below for more details on
the distinction.

只有在 Android Studio 3.6 及以上的版本，配合 42 以上版本的
IntelliJ [Flutter 外掛][Flutter plugin] 才能直接透過 Android Studio 執行整合流程，
並且，Android Studio 目前僅支援以 Gradle 子專案原始碼的方式整合，而不能以 AAR 方式整合。
有關這兩種方式的區別及更多詳細資訊，請參見下文。

Using the **File > New > New Module...** menu in
Android Studio in your existing Android project,
you can either create a new Flutter module to integrate,
or select an existing Flutter module that was created previously.

在 Android Studio 開啟現有的 Android 專案並點選選單按鈕 **File > New > New Module...** ，
這樣就可以創建出一個可以整合的新 Flutter 模組，或者選擇匯入已有的 Flutter 模組。

{% include docs/app-figure.md image="development/add-to-app/android/project-setup/ide-new-module.png" %}

If you create a new module, you can use a wizard to
select the module name, location, and so on.

如果你想建立一個新的 Flutter 模組，則可以直接在嚮導視窗中填寫模組名稱、路徑等資訊。

{% include docs/app-figure.md image="development/add-to-app/android/project-setup/ide-wizard.png" %}

The Android Studio plugin automatically configures your
Android project to add your Flutter module as a dependency,
and your app is ready to build.

此時，Android Studio 外掛就會自動為這個 Android 專案
配置新增 Flutter 模組作為依賴項，這時整合應用就已準備好進行下一步的建構。

{{site.alert.note}}

  To see the changes that were automatically made to your
  Android project by the IDE plugin, consider using
  source control for your Android project before performing
  any steps. A local diff shows the changes.

  如果要檢視 IDE 外掛自動對 Android 專案做了哪些更改，
  可以在執行具體步驟之前對 Android 專案使用程式碼版本控制，
  便可以使用本地 diff 檢視更改內容的具體資訊。

{{site.alert.end}}

{{site.alert.tip}}

  By default, your project's Project pane is probably
  showing the 'Android' view. If you can't see your new
  Flutter files in the Project pane, ensure that
  your Project pane is set to display 'Project Files',
  which shows all files without filtering.

  預設情況下，專案的 Project 視窗中可能會顯示的是 “Android” 檢視，
  如果在 Project 視窗中看不到新建立的 Flutter 檔案，
  可以將 Project 視窗設定為顯示 “Project Files”，
  這時就會顯示所有未過濾的檔案。

{{site.alert.end}}

Your app now includes the Flutter module as a dependency.
You can jump to the [Adding a Flutter screen to an Android app][]
to follow the next steps.

現在，應用程式已經包含了 Flutter 模組作為依賴項，
你可以跳轉至
[向 Android 應用中新增 Flutter 頁面][Adding a Flutter screen to an Android app] 執行後續步驟。

## Manual integration

## 手動整合

To integrate a Flutter module with an existing Android app
manually, without using Flutter's Android Studio plugin,
follow these steps:

如果想要在不使用 Flutter 的 Android Studio 外掛的情況下
手動將 Flutter 模組與現有的 Android 應用整合，可以參考以下步驟：

### Create a Flutter module

### 建立 Flutter 模組

Let's assume that you have an existing Android app at
`some/path/MyApp`, and that you want your Flutter
project as a sibling:

假設你在 `some/path/MyApp` 路徑下已有一個 Android 應用，
並且你希望 Flutter 專案作為同級專案：

```terminal
$ cd some/path/
$ flutter create -t module --org com.example my_flutter
```

This creates a `some/path/my_flutter/` Flutter module project
with some Dart code to get you started and an `.android/`
hidden subfolder. The `.android` folder contains an
Android project that can both help you run a barebones
standalone version of your Flutter module via `flutter run`
and it's also a wrapper that helps bootstrap the Flutter
module an embeddable Android library.

這會建立一個 `some/path/my_flutter/` 的 Flutter 模組專案，
其中包含一些 Dart 程式碼來幫助你入門以及一個隱藏的子資料夾 `.android/`。
`.android` 資料夾包含一個 Android 專案，
該專案不僅可以幫助你透過 `flutter run` 執行這個 Flutter 模組的獨立應用，
而且還可以作為封裝程式來幫助引導 Flutter 模組作為可嵌入的 Android 庫。

{{site.alert.note}}

  Add custom Android code to your own existing
  application's project or a plugin,
  not to the module in `.android/`.
  Changes made in your module's `.android/`
  directory won't appear in your existing Android
  project using the module.

  將自己的 Android 程式碼新增到你現有應用程式的專案或外掛中，
  而不是新增到 `.android/` 中的模組。在模組的 `.android/` 目錄中
  所做的任何更改並不會顯示在使用該模組的現有 Android 專案中。

  Do not source control the `.android/` directory
  since it's autogenerated. Before building the
  module on a new machine, run `flutter pub get`
  in the `my_flutter` directory first to regenerate
  the `.android/` directory before building the
  Android project using the Flutter module.

  由於 `.android/` 目錄是自動產生的，因此不需要對它的程式碼進行版本控制，
  在新機器上建構模組之前，可以先在 `my_flutter` 目錄中
  執行 `flutter pub get` 來重新產生 `.android/` 目錄，
  然後再使用 Flutter 模組建構 Android 專案。

{{site.alert.end}}

{{site.alert.note}}

  To avoid Dex merging issues, `flutter.androidPackage` should not be identical to your host app's package name.
  
  為了避免 Dex 合併出現問題，`flutter.androidPackage` 不應與應用的套件名稱相同。
{{site.alert.end}}

{{site.alert.note}}
  To avoid Dex merging issues, `flutter.androidPackage` should not be identical to your host app's package name
{{site.alert.end}}

{{site.alert.note}}

  To avoid Dex merging issues, `flutter.androidPackage` should not be identical to your host app's package name

  要避免 Dex 合併衝突問題，**請勿**將你的宿主工程名設為 `flutter.androidPackage`。

{{site.alert.end}}

### Java 8 requirement

### 引入 Java 8

The Flutter Android engine uses Java 8 features.

Flutter Android 引擎需要使用到 Java 8 中的新特性。

Before attempting to connect your Flutter module project
to your host Android app, ensure that your host Android
app declares the following source compatibility within your
app's `build.gradle` file, under the `android { }`
block, such as:

在嘗試將 Flutter 模組專案整合到宿主 Android 應用之前，
請先確保宿主 Android 應用的 build.gradle 檔案的  `android { }` 塊中
聲明瞭以下源相容性，例如：

<!--code-excerpt "MyApp/app/build.gradle" title-->

```gradle
android {
  //...
  compileOptions {
    sourceCompatibility 1.8
    targetCompatibility 1.8
  }
}
```

### Add the Flutter module as a dependency

### 將 Flutter module 作為依賴項

Next, add the Flutter module as a dependency of your
existing app in Gradle. There are two ways to achieve this.
The AAR mechanism creates generic Android AARs as
intermediaries that packages your Flutter module.
This is good when your downstream app builders don't
want to have the Flutter SDK installed. But,
it adds one more build step if you build frequently.

接下來，將 Flutter 模組新增為 Gradle 中宿主應用程式的依賴項。
主要有兩種方法實現。 AAR 機制可以為每個 Flutter 模組
建立 Android AAR 作為依賴媒介。當你的宿主應用程式開發者
不想安裝 Flutter SDK 時，這是一個很好方案，但是，如果你想要經常編譯，
那麼每次都需要重新編譯一次，該步驟不可避免。

The source code subproject mechanism is a convenient
one-click build process, but requires the Flutter SDK.
This is the mechanism used by the Android Studio IDE plugin.

直接將 Flutter 模組的原始碼作為子專案的依賴機制是一種便捷的一鍵式建構方案，
但此時需要另外安裝 Flutter SDK，這是目前 Android Studio IDE 外掛使用的機制。

#### Option A - Depend on the Android Archive (AAR)

#### 方案 A - 依賴 Android Archive (AAR)

This option packages your Flutter library as a generic local
Maven repository composed of AARs and POMs artifacts.
This option allows your team to build the host app without
installing the Flutter SDK. You can then distribute the
artifacts from a local or remote repository.

這種方式會將 Flutter 庫打包成由 AAR 和 POM artifacts 組成的本地 Maven 儲存庫。
這種方案可以使你的團隊不需要安裝 Flutter SDK 即可編譯宿主應用。
之後，你可以從本地或遠端儲存庫中分發更新 artifacts。

Let's assume you built a Flutter module at
`some/path/my_flutter`, and then run:

假設你在 `some/path/my_flutter` 下建構 Flutter 模組，執行如下命令：

```terminal
$ cd some/path/my_flutter
$ flutter build aar
```

Then, follow the on-screen instructions to integrate.

然後，根據螢幕上的提示完成整合操作。

{% include docs/app-figure.md image="development/add-to-app/android/project-setup/build-aar-instructions.png" %}

More specifically, this command creates
(by default all debug/profile/release modes)
a [local repository][], with the following files:

詳細地說，該命令應用於建立（預設情況下建立 debug/profile/release 所有模式）本地儲存庫，
主要包含以下檔案：

```text
build/host/outputs/repo
└── com
    └── example
        └── my_flutter
            ├── flutter_release
            │   ├── 1.0
            │   │   ├── flutter_release-1.0.aar
            │   │   ├── flutter_release-1.0.aar.md5
            │   │   ├── flutter_release-1.0.aar.sha1
            │   │   ├── flutter_release-1.0.pom
            │   │   ├── flutter_release-1.0.pom.md5
            │   │   └── flutter_release-1.0.pom.sha1
            │   ├── maven-metadata.xml
            │   ├── maven-metadata.xml.md5
            │   └── maven-metadata.xml.sha1
            ├── flutter_profile
            │   ├── ...
            └── flutter_debug
                └── ...
```

To depend on the AAR, the host app must be able
to find these files.

要依賴 AAR，宿主應用必須能夠找到這些檔案。

To do that, edit `app/build.gradle` in your host app
so that it includes the local repository and the dependency:

為此，需要在宿主應用程式中修改 `app/build.gradle` 檔案，
使其包含本地儲存庫和上述依賴項：

<!--code-excerpt "MyApp/app/build.gradle" title-->
```gradle
android {
  // ...
}

repositories {
  maven {
    url 'some/path/my_flutter/build/host/outputs/repo'
    // This is relative to the location of the build.gradle file
    // if using a relative path.
  }
  maven {
    url 'https://storage.googleapis.com/download.flutter.io'
  }
}

dependencies {
  // ...
  debugImplementation 'com.example.flutter_module:flutter_debug:1.0'
  profileImplementation 'com.example.flutter_module:flutter_profile:1.0'
  releaseImplementation 'com.example.flutter_module:flutter_release:1.0'
}
```

{{site.alert.important}}

  If you're located in China, use a mirror site such as
  `https://[a mirror site]/download.flutter.io` rather than the
  `storage.googleapis.com` domain directly. See our
  [Using Flutter in China][] page for information on mirrors.

  在國內，需要使用映象站點（如直接使用 `https://[a mirror site]/download.flutter.io` 代替 `storage.googleapis.com`）。 
  有關映象的詳細資訊，參見 [在中國網路環境下使用 Flutter][Using Flutter in China] 頁面。

{{site.alert.end}}

{{site.alert.tip}}

  You can also build an AAR for your Flutter module in Android Studio using
  the `Build > Flutter > Build AAR` menu.

  你也可以直接點選 Android Studio 選單中的 `Build > Flutter > Build AAR` 
  為 Flutter 模組建構 AAR。

{% include docs/app-figure.md image="development/add-to-app/android/project-setup/ide-build-aar.png" %}
{{site.alert.end}}

  Your app now includes the Flutter module as a dependency.
  You can follow the next steps in the
  [Adding a Flutter screen to an Android app][].

你的應用程式現在添加了 Flutter 模組作為依賴項，下面，
你可以按照 [向 Android 應用中新增 Flutter 頁面][Adding a Flutter screen to an Android app] 中的後續步驟繼續操作。

#### Option B - Depend on the module's source code

#### 方案 B - 依賴模組的原始碼

This option enables a one-step build for both your
Android project and Flutter project. This option is
convenient when you work on both parts simultaneously
and rapidly iterate, but your team must install the
Flutter SDK to build the host app.

該方式可以使你的 Android 專案和 Flutter 專案能夠同步一鍵式建構。
當你需要同時在這兩個專案中進行快速迭代時，這種方案非常方便，
但是此時，你的團隊必須安裝 Flutter SDK 才能建構宿主應用程式。

{{site.alert.tip}}

  By default, the host app provides the `:app` Gradle project.
  To change the name of this project, set
  `flutter.hostAppProjectName` in the Flutter module's
  `gradle.properties` file. Finally, include this project
  in the host app's `settings.gradle` file mentioned below.

  預設情況下，宿主應用程式已經提供了 Gradle 專案 `:app`。 
  要更改該專案的名稱，可以在 Flutter 模組的 `gradle.properties` 檔案中
  設定 `flutter.hostAppProjectName`。最後，將該專案新增到
  下面提到的宿主應用的 settings.gradle 檔案中。

{{site.alert.end}}

Include the Flutter module as a subproject in the host app's
`settings.gradle`:

將 Flutter 模組作為子專案新增到宿主應用的 `settings.gradle` 中：

<!--code-excerpt "MyApp/settings.gradle" title-->
```groovy
// Include the host app project.
include ':app'                                    // assumed existing content
setBinding(new Binding([gradle: this]))                                // new
evaluate(new File(                                                     // new
  settingsDir.parentFile,                                              // new
  'my_flutter/.android/include_flutter.groovy'                         // new
))                                                                     // new
```

Assuming `my_flutter` is a sibling to `MyApp`.

假設 `my_flutter` 和 `MyApp` 是同級目錄。

The binding and script evaluation allows the Flutter
module to `include` itself (as `:flutter`) and any
Flutter plugins used by the module (as `:package_info`,
`:video_player`, etc) in the evaluation context of
your `settings.gradle`.

binding 和 evaluation 指令碼可以使 Flutter 模組將其自身（如 `:flutter`）和
該模組使用的所有 Flutter 外掛（如 `:package_info`，`:video_player` 等）
都包含在 `settings.gradle` 的評估的上下文中。

Introduce an `implementation` dependency on the Flutter
module from your app:

在你的應用中引入對 Flutter 模組的依賴：

<!--code-excerpt "MyApp/app/build.gradle" title-->

```groovy
dependencies {
  implementation project(':flutter')
}
```

Your app now includes the Flutter module as a dependency.
You can follow the next steps in the [Adding a Flutter screen to an Android app][].

此時，你的應用程式已將 Flutter 模組新增為依賴項，
下面，你可以按照 
[向 Android 應用中新增 Flutter 頁面][Adding a Flutter screen to an Android app] 
中的後續步驟繼續操作。


[`abiFilters`]: https://developer.android.com/reference/tools/gradle-api/4.2/com/android/build/api/dsl/Ndk#abiFilters:kotlin.collections.MutableSet
[Adding a Flutter screen to an Android app]: {{site.url}}/development/add-to-app/android/add-flutter-screen
[Flutter plugin]: https://plugins.jetbrains.com/plugin/9212-flutter
[local repository]: https://docs.gradle.org/current/userguide/declaring_repositories.html#sub:maven_local
[only supports]: {{site.url}}/resources/faq#what-devices-and-os-versions-does-flutter-run-on
[Using Flutter in China]: {{site.url}}/community/china
