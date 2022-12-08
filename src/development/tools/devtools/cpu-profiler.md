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

## What is it?

## 它是什麼？

The CPU profiler view allows you to record and profile a
session from your Dart or Flutter application.

CPU 探測檢視能夠測量並記錄你的 Dart 或 Flutter 應用的片段。

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
