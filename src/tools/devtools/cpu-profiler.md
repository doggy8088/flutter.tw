---
title: Using the CPU profiler view
title: 使用 CPU 探測檢視
description: Learn how to use the DevTools CPU profiler view.
---

{{site.alert.note}}

  The CPU profiler view works with Dart CLI and mobile apps only.
  Use Chrome DevTools to [analyze performance][]
  of a web app.

  CPU 探測檢視僅能在 Dart CLI 以及移動應用中使用。
  請使用 Chrome DevTools [測量][analyze performance] web 應用的效能。

{{site.alert.end}}

The CPU profiler view allows you to record and profile a
session from your Dart or Flutter application.
The profiler can help you solve performance problems
or generally understand your app's CPU activity.
The Dart VM collects CPU samples
(a snapshot of the CPU call stack at a single point in time)
and sends the data to DevTools for visualization.
By aggregating many CPU samples together,
the profiler can help you understand where the CPU
spends most of its time.

藉助 CPU 探測檢視，你可以記錄並測量來自 Dart 或 Flutter 應用的會話。
探測器可以幫助你解決效能問題，或者更好地理解應用的 CPU 活動。
Dart VM 收集 CPU 樣本（在單個時間點上 CPU 呼叫棧的快照），
並將資料傳送給 DevTools 以進行視覺化。
透過聚合多個 CPU 樣本，探測器可以幫助你瞭解 CPU 的大部分時間都花在了哪裡。

{{site.alert.note}}

  **If you are running a Flutter application,
  use a profile build to analyze performance.**
  CPU profiles are not indicative of release performance
  unless your Flutter application is run in profile mode.

  **如果你正在執行 Flutter 應用，請在 profile 模式下測量效能。**
  CPU 探測器無法測量 release 模式下的效能，你應該使用 profile 模式建構應用。

{{site.alert.end}}

{% include_relative _profiler.md %}

[analyze performance]: {{site.developers}}/web/tools/chrome-devtools/evaluate-performance/
  
## Other resources

## 其他資源
  
To learn how to use DevTools to analyze
the CPU usage of a compute-intensive Mandelbrot app,
check out a guided [CPU Profiler View tutorial][profiler-tutorial].
Also, learn how to analyze CPU usage when the app
uses isolates for parallel computing.

要了解如何使用 DevTools 分析計算密集型 Mandelbrot 應用的 CPU 使用情況，
請檢視 [CPU 探測檢視課程][profiler-tutorial]。
此外，還可以瞭解應用在使用隔離區進行平行計算時的 CPU 使用情況。

[profiler-tutorial]: {{site.medium}}/@fluttergems/mastering-dart-flutter-devtools-cpu-profiler-view-part-6-of-8-31e24eae6bf8
