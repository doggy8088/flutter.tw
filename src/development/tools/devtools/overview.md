---
title: DevTools
title: 開發者工具概覽
description: How to use the DevTools with Flutter.
description: 學習如何在 Flutter 裡使用開發者工具。
---

## What is DevTools?

## 開發工具是什麼？

DevTools is a suite of performance and debugging tools
for Dart and Flutter.

開發工具是一套 Dart 和 Flutter 的效能除錯工具。
目前已經“行進”到 Beta 版本了，但仍在正在持續開發中。

![Dart DevTools Screens]({{site.url}}/assets/images/docs/tools/devtools/dart-devtools.gif){:width="100%"}

## What can I do with DevTools?

## 我可以用開發工具來做什麼？

Here are some of the things you can do with DevTools:

下面列出了一些可以用開發工具來實現的操作：

* Inspect the UI layout and state of a Flutter app.

  檢查 Flutter 應用程式的 UI 元件佈局和狀態；

* Diagnose UI jank performance issues in a Flutter app.

  在 Flutter 應用程式中診斷 UI 效能過低的問題；
  
* CPU profiling for a Flutter or Dart app.

  Flutter 和 Dart 應用的 CPU 效能檢測；

* Network profiling for a Flutter app.

  為 Flutter 應用進行網路效能檢測；

* Source-level debugging of a Flutter or Dart app.

  為 Flutter 或 Dart 應用進行原始碼級的除錯；

* Debug memory issues in a Flutter or Dart
  command-line app.

  在 Flutter 或 Dart 命令列應用中測試記憶體問題；

* View general log and diagnostics information
  about a running Flutter or Dart
  command-line app.
* Analyze code and app size.

  檢視正在執行的 Flutter 或 Dart 的命令列應用程式相關的常規日誌和診斷資訊。

We expect you to use DevTools in conjunction with
your existing IDE or command-line based development workflow.

我們希望您將開發工具與現有的 IDE 或基於命令列的開發流程結合起來使用。

<a name="install-devtools"></a>
## How do I install DevTools?

## 如何安裝開發工具？

See the [Android Studio/IntelliJ][], [VS Code][], or
[command line][] pages for installation instructions.

檢視 [Android Studio/IntelliJ][]、[VS Code][] 或
[命令列][command line] 頁面獲取更多安裝指導。

## Providing feedback

## 提交反饋

Please give DevTools a try, provide feedback, and file issues
in the [DevTools issue tracker][]. Thanks!

請在 [開發者工具 issue 追蹤器][DevTools issue tracker] 中嘗試使用開發工具，並提交反饋和檔案 issue。

## Other resources

## 其他資源

For more information on debugging and profiling
Flutter apps, see the [Debugging][] page and,
in particular, its list of [other resources][].

關於除錯、分析 Flutter 應用程式的更多詳細，
請查閱 [除錯][Debugging] 頁面，尤其是
[其他資源][other resources] 列表。

For more information on using DevTools with Dart command-line apps, see the 
[DevTools documentation on dart.dev]({{site.dart-site}}/tools/dart-devtools).

如果你希望知道更多如何在命令列下使用開發者工具 (DevTools) 的話，
請參考這個頁面 [Dart 開發者工具]({{site.dart-site}}/tools/dart-devtools).

[Android Studio/IntelliJ]: {{site.url}}/development/tools/devtools/android-studio
[VS Code]: {{site.url}}/development/tools/devtools/vscode
[command line]: {{site.url}}/development/tools/devtools/cli
[DevTools issue tracker]: {{site.github}}/flutter/devtools/issues
[Debugging]: {{site.url}}/testing/debugging
[Other resources]: {{site.url}}/testing/debugging#other-resources
