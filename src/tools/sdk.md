---
title: Flutter SDK overview
title: Flutter SDK 概覽
short-title: Flutter SDK
short-title: Flutter SDK
description: Flutter libraries and command-line tools.
description: Flutter 庫和命令列工具。
---

The Flutter SDK has the packages and command-line tools that you need to develop
Flutter apps across platforms. To get the Flutter SDK, see [Install][].

Flutter SDK 擁有你在 Flutter 跨平臺應用程式所需的 package 和命令列工具。
請前往 [安裝][Install] 頁面瞭解如何安裝 Flutter SDK。

## What's in the Flutter SDK

## Flutter SDK 中的內容

The following is available through the Flutter SDK:

以下是 Flutter SDK 提供的內容：

* [Dart SDK][]
* Heavily optimized, mobile-first 2D rendering engine with
  excellent support for text

  針對移動應用深度最佳化的 2D 渲染引擎，具備出色的文字支援功能

* Modern react-style framework

  現代響應式風格框架

* Rich set of widgets implementing Material Design and iOS styles

  Material Design 風格及 iOS 風格豐富的 widget 元件

* APIs for unit and integration tests

  用於單元測試和整合測試的 API

* Interop and plugin APIs to connect to the system and 3rd-party SDKs

  原生平台互動性和外掛 API 可以連線系統及第三方 SDK 

* Headless test runner for running tests on Windows, Linux, and Mac

  Headless 測試執行器，用於在 Windows、Linux 和 Mac 上執行測試

* [Dart DevTools][] for testing, debugging, and profiling your app

  用於測試、除錯和分析你的應用程式的 [Dart DevTools][]

* `flutter` and `dart` command-line tools for creating, building, testing,
  and compiling your apps

  `flutter` 和 `dart` 命令列工具，用於建立、開發、測試和編譯你的應用程式

Note: For more information about the Flutter SDK, see its
[README file][].

說明：關於 Flutter SDK 的更多資訊，
請查閱 [README 檔案][README file]。

## `flutter` command-line tool

## `flutter` 命令列工具

The [`flutter` CLI tool][] (`flutter/bin/flutter`) is how developers
(or IDEs on behalf of developers) interact with Flutter.

開發者（或使用的 IDE）使用 [`flutter` CLI 工具][`flutter` CLI tool] (`flutter/bin/flutter`)
與 Flutter 的相關功能進行互動。

## `dart` command-line tool

## `dart` 命令列工具

The [`dart` CLI tool][] is available with the Flutter SDK at `flutter/bin/dart`.

Flutter SDK 中提供了 [`dart` CLI 工具][`dart` CLI tool] (`flutter/bin/dart`)。

[Dart DevTools]: {{site.url}}/tools/devtools
[Dart SDK]: {{site.dart-site}}/tools/sdk
[`dart` CLI tool]: {{site.dart-site}}/tools/dart-tool
[`flutter` CLI tool]: {{site.url}}/reference/flutter-cli
[Install]: {{site.url}}/get-started/install
[README file]: {{site.repo.flutter}}/blob/master/README.md
