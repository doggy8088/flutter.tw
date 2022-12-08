---
title: Add Flutter to existing app
title: 將 Flutter 整合到現有應用
description: Adding Flutter as a library to an existing Android or iOS app.
description: 將 Flutter 作為 library 整合到現有的 Android 或 iOS 應用。
tags: Flutter混合工程,add2app
keywords: Flutter原生混編,Flutter整合
---

## Add-to-app

## 整合到現有應用

It's sometimes not practical to rewrite your entire application in
Flutter all at once. For those situations,
Flutter can be integrated into your existing
application piecemeal, as a library or module.
That module can then be imported into your Android or iOS
(currently supported platforms) app to render a part of your
app's UI in Flutter. Or, just to run shared Dart logic.

有時候，用 Flutter 一次性重寫整個已有的應用是不切實際的。
對於這些情況，Flutter 可以作為一個函式庫或模組，
整合進現有的應用當中。
模組引入到您的 Android 或 iOS 應用（當前支援的平台）中，
以使用 Flutter 來渲染一部分的 UI，或者僅執行多平臺共享的 Dart 程式碼邏輯。

In a few steps, you can bring the productivity and the expressiveness of
Flutter into your own app.

僅需幾步，你就可以將高效而富有表現力的 Flutter 引入您的應用。

The `add-to-app` feature supports integrating multiple instances of any screen size.
This can help scenarios such as a hybrid navigation stack with mixed
native and Flutter screens, or a page with multiple partial-screen Flutter
views.

Add-to-app 支援將多個 Flutter 例項附加到任意大小的檢視上。
適用於混合棧應用在導航到原生頁面和 Flutter 頁面的情況，
也適用於一個頁面有原生檢視和 Flutter 檢視的情況等混合棧應用。

Having multiple Flutter instances allows each instance to maintain
independent application and UI state while using minimal
memory resources. See more in the [multiple Flutters][] page.

多個 Flutter 例項會幫助每個例項保持獨立的應用和 UI 狀態，
同時使用最少的記憶體資源。請多詳細內容，請參考文件：
[多個 Flutter 例項][multiple Flutters]。 

## Supported features

## 已支援的特性

### Add to Android applications

### 整合到 Android 應用

{% include docs/app-figure.md image="development/add-to-app/android-overview.gif" alt="Add-to-app steps on Android" %}

* Auto-build and import the Flutter module by adding a
  Flutter SDK hook to your Gradle script.

  在 Gradle 指令碼中新增一個自動建構並引入 Flutter 模組的 Flutter SDK 鉤子。

* Build your Flutter module into a generic
  [Android Archive (AAR)][] for integration into your
  own build system and for better Jetifier interoperability
  with AndroidX.

  將 Flutter 模組建構為通用的 [Android Archive (AAR)][Android Archive (AAR)]
  以便整合到您自己的建構系統中，並提高 Jetifier 與 AndroidX 的互操作性；

* [`FlutterEngine`][java-engine] API for starting and persisting
  your Flutter environment independently of attaching a
  [`FlutterActivity`][]/[`FlutterFragment`][] etc.

  [`FlutterEngine`][java-engine] API 用於啟動並持續地為掛載 
  [`FlutterActivity`][] 或 [`FlutterFragment`][] 提供獨立的 Flutter 環境；

* Android Studio Android/Flutter co-editing and module
  creation/import wizard.

  Android Studio 的 Android 與 Flutter 同時編輯，
  以及 Flutter module 建立與匯入嚮導；

* Java and Kotlin host apps are supported.

  支援了 Java 和 Kotlin 為宿主的應用程式；

* Flutter modules can use [Flutter plugins][] to interact
  with the platform.

  Flutter 模組可以透過使用 [Flutter plugins][] 與平台進行互動。

* Support for Flutter debugging and stateful hot reload by
  using `flutter attach` from IDEs or the command line to
  connect to an app that contains Flutter.

  支援透過從 IDE 或命令列中使用 `flutter attach` 
  來實現 Flutter 除錯與有狀態的熱重載。

### Add to iOS applications

### 整合到 iOS 應用

{% include docs/app-figure.md image="development/add-to-app/ios-overview.gif" alt="Add-to-app steps on iOS" %}

* Auto-build and import the Flutter module by adding a Flutter
  SDK hook to your CocoaPods and to your Xcode build phase.

  在 Xcode 的 Build Phase 以及 CocoaPods 中，
  新增一個自動建構並引入 Flutter 模組的 Flutter SDK 鉤子。
  
* Build your Flutter module into a generic [iOS Framework][]
  for integration into your own build system.

  將 Flutter 模組建構為通用的 [iOS Framework][]
  以便整合到您自己的建構系統中；
  
* [`FlutterEngine`][ios-engine] API for starting and persisting
  your Flutter environment independently of attaching a
  [`FlutterViewController`][].

  [`FlutterEngine`][ios-engine] API 用於啟動並持續地為掛載
  [`FlutterViewController`][] 以提供獨立的 Flutter 環境；
  
* Objective-C and Swift host apps supported.

  支援了 Objective-C 和 Swift 為宿主的應用程式；
  
* Flutter modules can use [Flutter plugins][] to interact
  with the platform.

  Flutter 模組可以透過使用 [Flutter plugins][] 與平台進行互動；
  
- Support for Flutter debugging and stateful hot reload by
  using `flutter attach` from IDEs or the command line to
  connect to an app that contains Flutter.

  支援透過從 IDE 或命令列中使用 `flutter attach` 
  來實現 Flutter 除錯與有狀態的熱重載。

See our [add-to-app GitHub Samples repository][]
for sample projects in Android and iOS that import
a Flutter module for UI.

檢視 [add-to-app GitHub 範例儲存庫](https://github.com/flutter/samples/tree/master/experimental/add_to_app)
中在 iOS 和 Android 平臺上引入 Flutter module 的範例專案。 

## Get started

## 開始

To get started, see our project integration guide for
Android and iOS:

第一步，檢視以下工程整合指南

<div class="card-deck mb-8">
  <a class="card" href="{{site.url}}/development/add-to-app/android/project-setup">
    <div class="card-body">
      <header class="card-title text-center m-0">
        Android
      </header>
    </div>
  </a>
  <a class="card" href="{{site.url}}/development/add-to-app/ios/project-setup">
    <div class="card-body">
      <header class="card-title text-center m-0">
        iOS
      </header>
    </div>
  </a>
</div>

## API usage

## API 用法

After Flutter is integrated into your project,
see our API usage guides at the following links:

將 Flutter 整合進您的工程後，可以檢視以下 API 使用指南

<div class="card-deck mb-8">
  <a class="card" href="{{site.url}}/development/add-to-app/android/add-flutter-screen">
    <div class="card-body">
      <header class="card-title text-center m-0">
        Android
      </header>
    </div>
  </a>
  <a class="card" href="{{site.url}}/development/add-to-app/ios/add-flutter-screen">
    <div class="card-body">
      <header class="card-title text-center m-0">
        iOS
      </header>
    </div>
  </a>
</div>

## Limitations

## 已知限制

* Packing multiple Flutter libraries into an
  application isn't supported.

  不支援將多個 Flutter 庫（Flutter 模組）同時打包進一個應用程式。

* Plugins used in add-to-app on Android should migrate
  to the [new Android plugin APIs][Android plugin APIs], based on [`FlutterPlugin`].

  基於 Android add-to-app 的外掛必須遷移至基於
  [`FlutterPlugin`][] 的 [新版 Android 外掛 API][Android plugin APIs]

* Plugins that don't support `FlutterPlugin` might have unexpected
  behaviors if they make assumptions that are untenable in add-to-app
  (such as assuming that a Flutter `Activity` is always present).

  不支援 `FlutterPlugin` 的外掛如果在 add-to-app 進行一些不合理的假設
  （例如假設 Flutter 的 `Activity` 始終存在），可能會出現意外行為。

* On Android, the Flutter module only supports AndroidX applications.

  Android 平台的 Flutter 模組僅支援適配了 AndroidX 的應用。

[add-to-app GitHub Samples repository]: {{site.github}}/flutter/samples/tree/main/add_to_app
[Android Archive (AAR)]: {{site.android-dev}}/studio/projects/android-library
[Android plugin APIs]: {{site.url}}/development/platform-integration/android/plugin-api-migration
[Flutter plugins]: {{site.pub}}/flutter
[`FlutterActivity`]: {{site.api}}/javadoc/io/flutter/embedding/android/FlutterActivity.html
[java-engine]: {{site.api}}/javadoc/io/flutter/embedding/engine/FlutterEngine.html
[ios-engine]: {{site.api}}/objcdoc/Classes/FlutterEngine.html
[FlutterFire]: {{site.github}}/FirebaseExtended/flutterfire/tree/master/packages
[`FlutterFragment`]: {{site.api}}/javadoc/io/flutter/embedding/android/FlutterFragment.html
[`FlutterPlugin`]: {{site.api}}/javadoc/io/flutter/embedding/engine/plugins/FlutterPlugin.html
[`FlutterViewController`]: {{site.api}}/objcdoc/Classes/FlutterViewController.html
[iOS Framework]: {{site.apple-dev}}/library/archive/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html
[maintained by the Flutter team]: {{site.repo.plugins}}/tree/main/packages
[migrated to the V2 plugins APIs]: {{site.url}}/development/platform-integration/android/plugin-api-migration
[multiple Flutters]: {{site.url}}/development/add-to-app/multiple-flutters
