---
title: "flutter: The Flutter command-line tool"
title: Flutter 命令列文件
description: "The reference page for using 'flutter' in a terminal window."
description: 在終端中使用 "flutter" 命令的指南。
---

The `flutter` command-line tool is how developers (or IDEs on behalf of
developers) interact with Flutter. For Dart related commands,
you can use the [`dart`][] command-line tool.

開發者（或 IDE）可以使用 `flutter` 命令列工具與 Flutter 的相關功能進行互動。
對於 Dart 相關的命令，你可以使用 [`dart`][] 命令列工具。

Here's how you might use the `flutter` tool to create, analyze, test, and run an
app:

以下命令讓你可以使用 `flutter` 命令列工具來建立、分析、測試以及執行一個應用程式：

```terminal
$ flutter create my_app
$ cd my_app
$ flutter analyze
$ flutter test
$ flutter run lib/main.dart
```

To run [`pub`][`dart pub`] commands using the `flutter` tool:

使用 `flutter` 命令列工具執行 [`pub`][`dart pub`] 相關命令：

```terminal
$ flutter pub get
$ flutter pub outdated
$ flutter pub upgrade
```

To view all commands that `flutter` supports:

檢視 `flutter` 所有支援的命令：

```terminal
$ flutter --help --verbose
```

To get the current version of the Flutter SDK, including its framework, engine,
and tools:

獲取當前版本 Flutter SDK 的資訊（包含框架、引擎和相關工具）：

```terminal
$ flutter --version
```

## `flutter` commands

## `flutter` 命令

The following table shows which commands you can use with the `flutter` tool:

下表列舉了你可以使用的 `flutter` 命令：

|---------+--------------------------------+-----------------------------------|
| <t>Command</t><t>命令</t> | <t>Example of use</t><t>範例</t> | <t>More information</t><t>描述</t> |
|---------------------------|---------------------------------|------------------------------------|
| analyze | `flutter analyze -d <DEVICE_ID>`     | Analyzes the project's Dart source code.<br>Use instead of [`dart analyze`][]. |
| analyze | `flutter analyze -d <DEVICE_ID>`     | 分析專案的 Dart 原始碼。<br>該命令用來替代 [`dart analyze`][]。 |
| assemble | `flutter assemble -o <DIRECTORY>` | Assemble and build flutter resources. |
| assemble | `flutter assemble -o <DIRECTORY>` | 組建和建構 Flutter 資源。 |
| attach | `flutter attach -d <DEVICE_ID>` | Attach to a running application. |
| attach | `flutter attach -d <DEVICE_ID>` | 連線到執行中的應用程式。 |
| bash-completion | `flutter bash-completion` | Output command line shell completion setup scripts. |
| bash-completion | `flutter bash-completion` | 輸出 Shell 命令列設定的指令碼。 |
| build | `flutter build <DIRECTORY>` | Flutter build commands. |
| build | `flutter build <DIRECTORY>` | Flutter 建構命令。 |
| channel | `flutter channel <CHANNEL_NAME>` | List or switch flutter channels. |
| channel | `flutter channel <CHANNEL_NAME>` | 列出或切換 Flutter 的渠道分支 |
| clean | `flutter clean` | Delete the `build/` and `.dart_tool/` directories. |
| clean | `flutter clean` | 刪除 `build/` 和 `.dart_tool/` 目錄。 |
| config | `flutter config --build-dir=<DIRECTORY>` | Configure Flutter settings. To remove a setting, configure it to an empty string. |
| config | `flutter config --build-dir=<DIRECTORY>` | 設定 Flutter 配置項。如果你需要刪除一個配置項，請將該配置項的值置空。 |
| create  | `flutter create <DIRECTORY>`      | Creates a new project. |
| create  | `flutter create <DIRECTORY>`      | 建立一個新專案。 |
| custom-devices  | `flutter custom-devices list` | Add, delete, list, and reset custom devices. |
| custom-devices  | `flutter custom-devices list` | 新增、刪除、列出或重置客製的裝置。 |
| devices | `flutter devices -d <DEVICE_ID>` | List all connected devices. |
| devices | `flutter devices -d <DEVICE_ID>` | 列出所有連線的裝置。 |
| doctor | `flutter doctor` | Show information about the installed tooling. |
| doctor | `flutter doctor` | 顯示相關已安裝工具的資訊。 |
| downgrade | `flutter downgrade` | Downgrade Flutter to the last active version for the current channel. |
| downgrade | `flutter downgrade` | 將 Flutter 降級到當前渠道分支的上一個有效版本。 |
| drive | `flutter drive` | Runs Flutter Driver tests for the current project. |
| drive | `flutter drive` | 運行當前專案的 Flutter 測試。 |
| emulators | `flutter emulators` | List, launch and create emulators. |
| emulators | `flutter emulators` | 列出、啟動或建立模擬器。 |
| gen-l10n | `flutter gen-l10n <DIRECTORY>` | Generate localizations for the Flutter project. |
| gen-l10n | `flutter gen-l10n <DIRECTORY>` | 為 Flutter 專案產生 l10n 本地化。 |
| install | `flutter install -d <DEVICE_ID>` | Install a Flutter app on an attached device. |
| install | `flutter install -d <DEVICE_ID>` | 在連線的裝置上安裝 Flutter 應用程式。 |
| logs | `flutter logs` | Show log output for running Flutter apps. |
| logs | `flutter logs` | 顯示執行中的 Flutter 應用程式的日誌內容。 |
| precache | `flutter precache <ARGUMENTS>` | Populates the Flutter tool's cache of binary artifacts. |
| precache | `flutter precache <ARGUMENTS>` | 下載並預快取 Flutter 相關平台工具的二進位制檔案 |
| pub     | `flutter pub <PUB_COMMAND>`       | Works with packages.<br>Use instead of [`dart pub`][]. |
| pub     | `flutter pub <PUB_COMMAND>`       | package 的相關操作命令。<br>該命令用來替代 [`dart pub`][]。 |
| run     | `flutter run <DART_FILE>`         | Runs a Flutter program. |
| run     | `flutter run <DART_FILE>`         | 執行 Flutter 應用程式。 |
| screenshot | `flutter screenshot` | Take a screenshot of a Flutter app from a connected device. |
| screenshot | `flutter screenshot` | 在連線的裝置上對 Flutter 應用程式進行截圖。 |
| symbolize | `flutter symbolize --input=<STACK_TRACK_FILE>` | Symbolize a stack trace from the AOT compiled flutter application. |
| symbolize | `flutter symbolize --input=<STACK_TRACK_FILE>` | 讀取並解析 Flutter 應用程式中 AOT 編譯的堆疊追蹤資訊。 |
| test    | `flutter test [<DIRECTORY|DART_FILE>]` | Runs tests in this package.<br>Use instead of [`dart test`][`dart test`]. |
| test    | `flutter test [<DIRECTORY|DART_FILE>]` | 執行測試<br>該命令用來替代 [`dart test`][]。 |
| upgrade | `flutter upgrade` | Upgrade your copy of Flutter. |
| upgrade | `flutter upgrade` | 升級你的 Flutter 版本。 |
{:.table .table-striped .nowrap}

For additional help on any of the commands, enter `flutter help <command>`
or follow the links in the **More information** column.
You can also get details on `pub` commands — for example,
`flutter help pub outdated`.

對於 flutter 命令的其他幫助資訊，請輸入 `flutter help <command>` 獲取，
或根據上面表格 **描述** 欄內的連結，獲取對應命令的相關資訊。
你還可以獲取關於 `pub` 命令的幫助資訊 —— 例如，
`flutter help pub outdated`。

[`dart`]: {{site.dart-site}}/tools/dart-tool
[`dart analyze`]: {{site.dart-site}}/tools/dart-analyze
[`dart format`]: {{site.dart-site}}/tools/dart-format
[`dart pub`]: {{site.dart-site}}/tools/dart-pub
[`dart test`]: {{site.dart-site}}/tools/dart-test
