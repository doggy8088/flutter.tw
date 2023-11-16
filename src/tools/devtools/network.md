---
title: Using the Network View
title: 使用網路檢視 (Network View)
description: How to use the DevTools network view.
tags: Flutter開發工具,DevTools
keywords: 開發者工具,網路檢視,Dart
---

{{site.alert.note}}

  The network view works with all Flutter and Dart applications.

  網路檢視適用於所有 Flutter 和 Dart 應用程式。

{{site.alert.end}}

## What is it?

## 網路檢視是什麼？

The network view allows you to inspect HTTP, HTTPS, and web socket traffic from
your Dart or Flutter application.

你可以透過網路檢視檢查來自 Dart 或 Flutter 應用程式的
HTTP、HTTPS 和 web socket 的網路流量情況。

![Screenshot of the network screen]({{site.url}}/assets/images/docs/tools/devtools/network_screenshot.png){:width="100%"}

## How to use it

## 如何使用

Network traffic should be recording by default when you open the Network page.
If it is not, click the **Record network traffic** button in the upper left to
begin polling.

當你開啟網路頁時，網路流量應該是預設記錄的。
如果沒有記錄，請點選左上方的 **Resume** 按鈕，
開始記錄網路流量情況。

Select a network request from the table (left) to view details (right). You can
inspect general and timing information about the request, as well as the content
of response and request headers and bodies.

從表格中選中一個網路請求（左邊）來檢視請求細節（右邊）。
你可以檢查關於請求的常規資訊和時間資訊，
包含響應內容、請求標頭、請求體。

### Search and filtering

### 搜尋和過濾

You can use the search and filter controls to find a specific request or filter
requests out of the request table.

你可以使用搜索和過濾來尋找一個特定的請求，
或者單獨在請求表中過濾請求。

![Screenshot of the network screen]({{site.url}}/assets/images/docs/tools/devtools/network_search_and_filter.png)

To apply a filter, press the filter button (right of the search bar). You will
see a filter dialog pop up:

要使用過濾器，請點選過濾器按鈕（搜尋欄右邊）。
你會看見一個過濾查詢對話方塊彈出：

![Screenshot of the network screen]({{site.url}}/assets/images/docs/tools/devtools/network_filter_dialog.png)

The filter query syntax is described in the dialog. You can filter network
requests by the following keys:

過濾查詢的語法在對話方塊中描述。
你可以透過以下語法關鍵詞來過濾網路請求：

* `method`, `m`: this filter corresponds to the value in the "Method" column

  `method`，`m`：該關鍵詞對應過濾 “Method” 列中的值

* `status`, `s`: this filter corresponds to the value in the "Status" column

  `status`，`s`：該關鍵詞對應過濾 “Status” 列中的值

* `type`, `t`: this filter corresponds to the value in the "Type" column

  `type`，`t`：該關鍵詞對應過濾 “Type” 列中的值

Any text that is not paired with an available filter key will be queried against
all categories (method, uri, status, type).

任何沒有與可用的語法關鍵詞對應的文字將會查詢所有類別 (method、uri、status、type)。

Example filter queries:

過濾器查詢範例：

```
my-endpoint m:get t:json s:200
```
```
https s:404
```

## Other resources

## 其他資訊

HTTP and HTTPs requests are also surfaced in the [Timeline][timeline] as
asynchronous timeline events. Viewing network activity in the timeline can be
useful if you want to see how HTTP traffic aligns with other events happening
in your app or in the Flutter framework.

HTTP 和 HTTPs 請求也作為非同步時間線事件出現在 [時間線 (Timeline)][timeline] 中。
如果你想了解 HTTP 流量情況與應用程式或 Flutter 框架中發生的其他事件是否一致，
這種情況下在時間線中檢視網路活動是很有用的。

To learn how to monitor an app's network traffic and inspect
different types of requests using the DevTools,
check out a guided [Network View tutorial][network-tutorial].
The tutorial also uses the view to identify network activity that
causes poor app performance.

要學習如何使用 DevTools 監控應用程式的網路流量以及檢查不同型別的請求，
請查閱 [網路檢視課程][network-tutorial]。
該課程還使用網路檢視來識別導致應用程式效能不佳的網路活動。

[timeline]: {{site.url}}/tools/devtools/performance#timeline-events-tab
[network-tutorial]: {{site.medium}}/@fluttergems/mastering-dart-flutter-devtools-network-view-part-4-of-8-afce2463687c
