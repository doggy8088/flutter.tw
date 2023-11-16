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

For a video introduction to DevTools, check out
the following deep dive and use case walkthrough:

<iframe width="560" height="315" src="https://www.youtube.com/embed/_EYk-E29edo" title="YouTube video player - Dive in to DevTools" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
[Dive in to DevTools](https://www.youtube.com/watch?v=_EYk-E29edo)

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

  檢視正在執行的 Flutter 或 Dart 的命令列應用程式相關的常規日誌和診斷資訊。

* Analyze code and app size.

  分析程式碼和應用的大小

We expect you to use DevTools in conjunction with
your existing IDE or command-line based development workflow.

我們希望您將開發工具與現有的 IDE 或基於命令列的開發流程結合起來使用。

<a id="install-devtools"></a>
## How do I install DevTools?

## 如何安裝開發工具？

See the [VS Code][], [Android Studio/IntelliJ][], or
[command line][] pages for installation instructions.

請參考 [VS Code][], [Android Studio/IntelliJ][], 或者
[命令列][command line] 頁面來安裝開發工具。

## Troubleshooting some standard issues

## 一些常見問題的解決方案

**Question**: My app looks janky or stutters.
  How do I fix it?

**問題**: 我的應用程式看起來很卡頓或者有明顯的延遲，
我該如何解決？

**Answer**: Performance issues can cause [UI frames][]
  to be janky and/or slow down some operations.

**解決方案**: 效能問題可能會導致 [UI frames][] 卡頓，
或者導致某些操作變慢。

  1. To detect which code impacts concrete late frames,
     start at [Performance > Timeline][].

     從 [效能 > 時間軸][Performance > Timeline] 開始，
     檢測哪些程式碼影響了具體的延遲幀。

  2. To learn which code takes the most CPU time in
     the background, use the [CPU profiler][].

     使用 [CPU profiler][] 來觀察和學習
     哪些程式碼在後台佔用了大量的 CPU 時間。

For more information, check out the
[Performance][] page.

更多資訊，請查閱 [效能][Performance] 頁面。

**Question**: I see a lot of garbage collection (GC) events occurring.
  Is this a problem?

**問題**: 我看到了很多垃圾回收 (GC) 事件的發生，
這是一個問題嗎？

**Answer**: Frequent GC events might display on
  the DevTools > Memory > Memory chart. In most cases,
  it's not a problem.

**解決方案**: 在開發工具 > 記憶體 > 記憶體圖表中，
頻繁的 GC 事件可能會顯示出來。在大多數情況下，
這個問題不大。

If your app has frequent background activity with some idle time,
Flutter might use that opportunity to collect the created objects
without performance impact.

如果你的應用有頻繁的後臺活動和一些空閒時間，
Flutter 可能會利用這個機會來收集建立的物件，而不會影響效能。

[CPU profiler]: {{site.url}}/tools/devtools/cpu-profiler
[Performance]: {{site.url}}/perf
[Performance > Timeline]: {{site.url}}/tools/devtools/performance#timeline-events-tab
[UI frames]: {{site.url}}/perf/ui-performance



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

果你希望知道更多如何在命令列下使用開發者工具 (DevTools) 的話，
請參考這個頁面 [Dart 開發者工具]({{site.dart-site}}/tools/dart-devtools).

[Android Studio/IntelliJ]: {{site.url}}/tools/devtools/android-studio
[VS Code]: {{site.url}}/tools/devtools/vscode
[command line]: {{site.url}}/tools/devtools/cli
[DevTools issue tracker]: {{site.github}}/flutter/devtools/issues
[Debugging]: {{site.url}}/testing/debugging
[Other resources]: {{site.url}}/testing/debugging#other-resources
