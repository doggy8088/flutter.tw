---
title: Using the Logging view
title: 使用日誌檢視 (Logging view)
description: Learn how to use the DevTools logging view.
description: 學習如何使用開發者工具的日誌檢視。
tags: Flutter開發工具,DevTools
keywords: 開發者工具,日誌檢視,Dart
---

{{site.alert.note}}

  The logging view works with all Flutter and Dart applications.

  日誌檢視可用於所有 Flutter 和 Dart 應用。
  
{{site.alert.end}}

## What is it?

## 簡介

The logging view displays events from the Dart runtime,
application frameworks (like Flutter), and application-level
logging events.

日誌檢視展示 Dart 執行時和應用框架（比如 Flutter）的事件，
以及應用級日誌。

## Standard logging events

## 標準日誌事件

By default, the logging view shows:

預設情況下，日誌檢視會展示：

* Garbage collection events from the Dart runtime

  Dart 執行時的垃圾回收事件

* Flutter framework events, like frame creation events

  Flutter 框架事件，比如建立幀的事件

* `stdout` and `stderr` from applications

  應用的 `stdout` 和 `stderr` 輸出

* Custom logging events from applications

  應用的自訂日誌事件

![日誌檢視的截圖]({{site.url}}/assets/images/docs/tools/devtools/logging_log_entries.png){:width="100%"}

## Logging from your application

## 應用日誌

To implement logging in your code,
see the [Logging][] section in the
[Debugging Flutter apps programmatically][]
page.

要在程式碼中輸出日誌，請檢視 
[新增輸出程式碼的方式除錯 Flutter 應用][Debugging Flutter apps programmatically]
頁面的 [日誌][Logging] 部分。

## Clearing logs

## 清理日誌

To clear the log entries in the logging view,
click the **Clear logs** button.

要清理日誌檢視的日誌記錄，請點選 **Clear logs**（清理日誌）按鈕。

[Logging]: {{site.url}}/testing/code-debugging#logging
[Debugging Flutter apps programmatically]: {{site.url}}/testing/code-debugging

## Other resources

To learn about different methods of logging
and how to effectively use DevTools to
analyze and debug Flutter apps faster,
check out a guided [Logging View tutorial][logging-tutorial].

[logging-tutorial]: {{site.medium}}/@fluttergems/mastering-dart-flutter-devtools-logging-view-part-5-of-8-b634f3a3af26
