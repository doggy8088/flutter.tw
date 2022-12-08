---
title: Using the Memory view
title: 使用記憶體檢視 (Memory view)
description: Learn how to use the DevTools memory view.
description: 學習如何使用開發者工具的內容檢視。
tags: Flutter開發工具,DevTools
keywords: 開發者工具,記憶體檢視,Dart
---

## What is it?

## 這是什麼？

Allocated Dart objects created using a class constructor (for
example, by using `new MyClass()` or `MyClass()`) live in a
portion of memory called the heap. The memory in the heap is
managed by the Dart VM (virtual machine).

使用類建構函式建立的 Dart 物件（例如，使用 `new MyClass()` 或 `MyClass()`）
會被分配在稱為 **堆** 的記憶體區域中。堆中的記憶體由 Dart VM（虛擬機器）管理。

## DevTools memory page

## DevTools 記憶體分析頁面

The DevTools Memory page lets you peek at how an isolate is using
memory at a given moment. 

你可以在 DevTools 中的記憶體分析頁面觀察在給定的時段內 isolate 在怎樣使用記憶體。

Memory profiling in DevTools consists of 3 main functions:

DevTools 中的記憶體分析包括 3 個主要功能：

* Charting memory usage statistics and events

  繪製記憶體使用統計資料和事件圖表

* Analysis to view all memory via a heap to detect memory issues
  and inspect objects

  透過檢視堆中的所有記憶體，來檢測記憶體問題和檢查物件

* Monitoring and tracking allocations and
  their stack traces for selected classes.

  分配時監控和追蹤（堆疊追蹤）特定類和物件的分配

{{site.alert.note}}

  **Use [profile mode][] when running your app
  to analyze performance.** Memory usage is not
  indicative of release performance unless your
  application is run in profile mode.
  In general, memory usage is fairly accurate,
  (in relative terms), between debug, release,
  and profile modes. Profile mode might show higher
  absolute memory usage because a service isolate
  is created to profile your application.
  This isolate won't exist in release mode.
  Absolute memory used might also be higher in
  debug versus release mode. In [release mode][],
  work can be computed and optimized ahead of time,
  while in [debug mode][] that same work might have
  to be computed at runtime, requiring more information.

  **分析應用效能時請使用 [效能模式][profile mode]。**
  記憶體使用情況僅在應用程式執行在效能模式下有代表性。
  一般來說，記憶體使用在除錯、生產和效能檔案模式之間都是相當準確的（相對而言）。
  效能模式下可能會展現更高的記憶體佔用，
  因為應用建立了一個單獨的服務 isolate 測算效能指標。
  在生產模式下，服務 isolate 不會存在。
  記憶體使用量在除錯模式下也會比生產模式高。
  在 [生產模式][release mode] 中，工作流以 AOT 方式被處理，
  而 [除錯模式][debug mode] 中部分工作流需要在執行時計算，
  從而需要更多的資訊進行處理。

{{site.alert.end}}

## Charting memory statistics and events

## 繪製記憶體統計資訊和事件圖表

At the top-level, when the **Memory** tab is selected,
memory statistics from the VM are collected and displayed
in the two overview charts (Dart memory and Android only).
The collected statistics include general memory usage,
such as total heap used, external heap size,
maximum heap capacity, and Resident Set Size (RSS).
As you interact with your application,
various events are detected and collected in the same timeline
as the memory statistics, such as memory GC (garabage collection),
Flutter events, and user-fired events
(using the `dart:developer` package).
All of these collected statistics and events
are displayed in charts.
For more information, see [Memory anatomy](#memory-anatomy).

當選中頂部 tab 中的 **Memory（記憶體）**選項時，
DevTools 將收集來自 VM 的記憶體統計資料。
這些統計資料顯示在兩個概覽圖（Dart 記憶體以及 Android 圖表）中，
記錄了一般記憶體的使用情況，
例如使用的總堆、外部堆、最大堆容量、常駐集大小（RSS）。
當你與應用程式互動時，會觸發各種事件，
例如記憶體 GC（垃圾回收）、Flutter 事件、使用者觸發的事件
（使用 `dart:developer` 包）與記憶體統計資料會被記錄在同一時間線中。
所有這些收集的統計資料和事件都顯示在圖表中，詳見 [記憶體剖析](#memory-anatomy)。

## Analysis and snapshots

## 分析和快照

A snapshot is a complete view of
all objects in the Dart memory heap.
Each time a snapshot is taken, a complex
and time consuming analysis
is performed over the collected memory data.
The memory profiler attempts to identify any memory patterns
that might cause leaks or lead to application crashes.
For example, loading large assets for images displayed
as thumbnails&mdash;memory usage can be improved by loading smaller
assets or adjusting the `cacheWidth` and `cacheHeight`
to decode an image to a smaller size.
This reduces the memory burden on the `ImageCache`.
For more information, see [Analysis tab](#analysis-tab).

快照是 Dart 記憶體堆中所有物件最複雜和最耗時的完整檢視。
每次儲存快照時，都會對收集的記憶體資料進行分析。
該分析會嘗試識別任何可能導致應用程式記憶體洩漏或崩潰的記憶體模式。
例如，為縮圖大小的圖片載入大型資源是低效的，
可以透過載入較小的資源或調整 cacheWidth/cacheHeight，
減小圖片的解碼大小，降低 ImageCache 的記憶體使用，以此提高記憶體使用率。
你可以在 [Analysis 選項](#analysis-tab) 中檢視分析捕捉到的問題。

## Allocations and tracking

## 分配和追蹤

Monitoring all allocations involves directly interacting
with DevTools and your application for a specific period
of time; this tells you how many objects and bytes were
allocated in that timeframe, as well as tracking
all of the places in your code where a particular
class was allocated.
This information is available under the **Allocations**
tab of the Memory profiler and is a fairly fast computation
with less overhead than using a snapshot.

在你感興趣的時間週期內，監控所有直接與 DevTools 或應用程式互動時涉及到的記憶體分配。
你可以瞭解分配了多少物件、多少位元組，或者追蹤程式碼中特定類別的所有分配位置。
此資訊可在記憶體分析頁面的 **Allocations（分配）** 選項下檢視，
與使用快照相比，它的速度相當快，開銷也更小。

Monitoring allocations and resetting accumulators
helps to analyze the accumulator counts (the number of
objects or bytes allocated), in a short timeframe.
If you suspect that your application is leaking memory,
or has other bugs relating to memory allocation,
use the accumulators to understand the rate
of allocation. Additionally, you can track
allocations of a few specific classes.
This feature isolates the exact location in your
code when memory is allocated.
For more information, see [Allocation tab](#allocation-tab).

監控分配和重置累計資料，有助於在點選重置和監視追蹤之間的短時間內分析累計資料（分配的物件數或位元組數）。
如果你懷疑應用程式發生了記憶體洩漏或存在與記憶體分配相關的其他錯誤，累計資料可以幫助你瞭解記憶體分配的速率。
此外，追蹤幾個特定類別的能力，可能會減慢應用程式的執行。VM 在呼叫類別的建構函式（分配）時記錄堆疊追蹤。
可以在分配記憶體時找到程式碼中的確切位置。請參閱 [Allocation 選項](#allocation-tab)。

{{site.alert.secondary}}

  **Pro tip**: When tracking is enabled for a class,
  the VM records a stack trace for each call to the
  class constructor. Recording this stack trace
  can be expensive, so we don't recommend tracking
  too many classes at once.

  **高階提示**：當開啟對某一個類別的追蹤時，
  VM 會記錄類構造的每一次呼叫堆疊。
  記錄這些堆疊較為消耗效能，
  所以此處不推薦同時追蹤過多類別。

{{site.alert.end}}

## Memory anatomy

## 記憶體剖析圖

## 記憶體剖析圖

A timeseries graph is used to visualize the state
of the Flutter memory at successive intervals of time.
Each data point on the chart corresponds to the timestamp
(x-axis) of measured quantities (y-axis) of the heap.
For example, usage, capacity, external, garbage
collection, and resident set size are captured.

時間序列圖用於顯示連續時間間隔的 Flutter 記憶體狀態。
圖表上的每個資料點表示相應時間戳（x 軸）下堆的測量值（y 軸），
例如，使用率、容量、外部記憶體、垃圾收集和常駐集大小。

![Screenshot of a memory anatomy page]({{site.url}}/assets/images/docs/tools/devtools/memory_chart_anatomy.png){:width="100%"}

### Events pane

### 事件窗格

The event timeline displays Dart VM and DevTools events
on a shared timeline. These events can be snapshots
(manual and auto), Dart VM GCs, user requested GCs,
or monitor and accumulator reset actions.

同一個時間軸上會顯示 Dart VM 和 DevTools 事件。
這些事件包含快照（手動和自動）、Dart VM 自動 GC、手動 GC
或者監控和累計資料的重置操作。

![Screenshot of DevTools events]({{site.url}}/assets/images/docs/tools/devtools/memory_eventtimeline.png)

This chart displays DevTools events (such as manual GC, VM GC,
snapshot, monitor Allocations **Track** and **Reset** of
accumulators button clicks) in relation to the memory chart timeline.
Clicking over the markers in the **Event** timeline displays
a hover card of the time when the event occurred.
This might help identify when a memory leak has
possibly occurred in the timeline (x-axis).

此圖表顯示與記憶體圖表時間線相關的 DevTools 事件
（如手動 GC、VM GC、快照、監控分配 **追蹤** 和 **重置** 累計資料按鈕單擊）。
點選事件時間線中的標記，將顯示事件發生時間的懸浮窗。
這可能有助於判斷時間軸（x 軸）中何時發生記憶體洩漏。

![Screenshot of the event timeline legend]({{site.url}}/assets/images/docs/tools/devtools/memory_eventtimeline_legend.png)

The following legend shows the symbol for each
DevTools event and its meaning:

下面是圖例中每個 DevTools 事件的符號及其含義:

**Snapshot**  

**Snapshot (手動快照)**
![User snapshot]({{site.url}}/assets/images/docs/tools/devtools/memory_eventtimeline_snapshot.png){:width="17px"}

User initiated snapshot&mdash;all memory
information is collected and an analysis is performed.

使用者主動儲存的快照，可以收集所有記憶體資訊並進行分析。

**Auto-Snapshot**  

**Auto-Snapshot (自動快照)**
![Auto snapshot]({{site.url}}/assets/images/docs/tools/devtools/memory_eventtimeline_auto_snapshot.png){:width="18px"}

DevTools initiated a snapshot after detecting
that memory has grown by 40% or more from its
last measurement. This can help to quickly detect
memory spikes in your Flutter application for later
analysis, and is the same information collected
in a manual snapshot.

當檢測到記憶體比以前的大小增加了 40% 或更多時
DevTools 會自動儲存一個快照。
可以用於快速檢測 Flutter 應用程式中的記憶體峰值，
以供後續分析，它與手動快照中記錄的內容一致。

**Track**  

**Track (追蹤)**
![Monitor]({{site.url}}/assets/images/docs/tools/devtools/memory_eventtimeline_monitor.png){:width="17px"}

Collects the current state of all active classes, the
number of instances, and the byte size of all instances.
In addition, the deltas indicate the change in the
accumulators since the last **Reset**.

收集當前所有處於活動狀態的類別的狀態例項數和所有例項的位元組大小。
此外，變化值是自上次按下 **Reset (重置)** 按鈕以來累計資料的變化。

**Reset**  

**Reset (重置)**
![Reset]({{site.url}}/assets/images/docs/tools/devtools/memory_eventtimeline_reset_monitor.png){:width="18px"}

When both the instance and byte accumulators
were reset to zero.

例項和位元組的累計資料都重置為零時。

**User Initiated GC**  

**User Initiated GC (使用者手動 GC)**
![GC]({{site.url}}/assets/images/docs/tools/devtools/memory_eventtimeline_gc.png){:width="18px"}

User initiated request to the VM to perform a
garbage collection on memory. This is not a guarantee
that GC occured&mdash;it's only a request.

使用者向 VM 請求執行記憶體 GC。僅向 VM 申請，不一定立刻執行。

**VM GC**  

**VM GC (VM 自動 GC)**  
![VM GC]({{site.url}}/assets/images/docs/tools/devtools/memory_eventtimeline_vmgc.png){:width="11px"}

GC (VM garbage collection) has occurred and has freed
up space that is no longer used. For more information on
how Dart performs garbage collection, see
[Don't Fear the Garbage Collector][].

VM 自動執行 GC，釋放不再使用的空間。
更多 Dart 是如何執行垃圾收集的資訊，
參閱 [不要擔心 GC][Don't Fear the Garbage Collector]。

**User and Flutter Event**  

**User and Flutter Event (使用者和 Flutter 事件)**

The dark magenta triangle displayed in the event pane indicates
that "Multiple Flutter or User Events" occurred at this timestamp.

深色三角形表示在這個時間點的「多個 Flutter 或使用者事件」。

![Aggregate Events]({{site.url}}/assets/images/docs/tools/devtools/memory_multi_events.png){:width="25px"}

The lighter magenta triangle indicates that
"One Flutter or User Event" occurred at this timestamp.

淺色三角形表示在這個時間點的「一次 Flutter 或使用者事件」。

![Single Events]({{site.url}}/assets/images/docs/tools/devtools/memory_one_event.png){:width="23px"}

To view the events, click on the triangle to display a hover card.
Expand the events at the bottom of the hover card
to display all events for that timestamp.

表示在此時間戳處僅收到一個事件。要檢視事件，點選三角形將顯示懸浮窗，
展開浮窗底部，將顯示該時間戳的所有事件。

Displayed below the events pane is the [memory chart](#memory-overview-chart)
and the [Android memory chart](#android-chart). The android-memory chart is
specific to an Android app, and it shows Android ADB meminfo from an
ADB app summary.

事件窗格下方顯示的是 [記憶體圖表](#memory-overview-chart) 和 [Android 記憶體圖表](#android-chart)。
只有 Android 應用程式才會展示，它顯示了 ADB 應用程式摘要中的 Android ADB 記憶體資訊。

### Adding user custom events to the timeline

### 新增自訂事件到時間線

Sometimes it might be difficult to correlate the
actions in your Flutter application code and the
collected memory statistics and events charted in
the **Memory** timeline chart.
To learn precisely what your app is doing to the heap,
you can inject your own events into the **Memory Profile** timeline.

有時可能很難將 Flutter 應用程式程式碼中的操作與記憶體時間線圖中繪製的記憶體資料或者事件關聯起來。
你可以將自訂的事件傳送到 **記憶體分析** 時間線中，來了解程式碼中發生了什麼。
這會幫助你瞭解應用程式在 Dart/Flutter 框架中的記憶體使用情況（堆）。

Post your own custom events by using the `postEvent`
method in the `dart:developer` package.
The event name must be prefixed with
**DevTools.Event_**.
For example, **DevTools.Event_**_MyEventName_

使用 dart:developer 套件的 `postEvent` 方法釋出你的自訂事件。
需要注意的是，事件名稱的字首必須為 **DevTools.Event_**，然後附加事件的名稱。
例如 **DevTools.Event_**_MyEventName_。

To use this feature,
add the following import to your code:

使用時你需要在程式碼中新增下方的匯入資訊：

<!--skip-->
```dart
import 'dart:developer' as developer;
```

Also add a method to post custom events to the
Memory timeline:

以及將自訂事件釋出到記憶體時間線的方法：

<!--skip-->
```dart
void devToolsPostEvent(String eventName, Map<String, Object> eventData) {
  developer.postEvent('DevTools.Event_$eventName', eventData);
}
```

To post an event from your code,
call the `devToolsPostEvent` method.
For example, in your function,
`recordLoadedImage`, you could post the
`MyImages` event to the memory timeline as follows:

要從程式碼中釋出事件，可以呼叫 devToolsPostEvent。
例如，在函式 recordLoadedImage 中，
可以透過 method（recordLoadedImage）以及 param（URL）引數
將 `MyImages` 事件釋出到記憶體（事件）時間線。

<!--skip-->
```dart
Widget recordLoadedImage(ImageChunkEvent imageChunkEvent, String imageUrl) {
 
  // Record the event in the memory event pane.
  devToolsPostEvent('MyImages', { 'method': 'recordLoadedImage', 'param': imageUrl });

  if (imageChunkEvent == null) return null;

  //...
  }
```

Clicking the aggregated event triangle in the event pane
dispays a hover card with the details of all events.
The following example displays two custom events at the
timestamp 04:36:21 with the event name `MyFirstApp`,
and the two `eventData` entries (`method` and `param`),
are displayed with their values: 

點選事件窗格中的多事件三角形，將顯示包含所有事件詳細資訊的懸浮窗，
例如，在 04:36:21 時刻的兩個自訂事件，事件名稱為「MyFirstApp」，
兩個欄位 method 和 param 顯示事件的對應值：

![Hover Card Custom Events]({{site.url}}/assets/images/docs/tools/devtools/memory_hover_events.png)

Scrolling the events displays the following:

滑動事件顯示：

![Custom Events Details]({{site.url}}/assets/images/docs/tools/devtools/memory_events_detail.png)

## Memory overview chart

## 記憶體概覽圖

The memory overview chart is a timeseries graph of collected
memory statistics. It visually presents the state of the
Dart or Flutter heap and Dart's or Flutter's native memory
over time.

採集的記憶體資料的時間序列圖，用於觀察隨時間變化的 Dart/Flutter 堆和本機記憶體的狀態。

The chart's x-axis is a timeline of events (timeseries).
The data plotted in the y-axis all has a timestamp
when the data was collected. In other words,
it shows the polled state (capacity, used, external,
RSS (resident set size), and GC (garbage collection))
of the memory every 500 ms. This helps provide a live
appearance on the state of the memory as the application is running.

圖表的 x 軸是事件的時間線（時間序列）。在 y 軸上繪製的資料在收集資料時都有時間戳。
換句話說，這會顯示每隔 500 毫秒記憶體的狀態（容量、已用記憶體、外部記憶體、常駐集大小和 GC）。
顯示應用程式執行即時的記憶體狀態。

Clicking the **Legend** button displays the collected
measurements and symbols and colors used to display the data.

點選圖例按鈕會顯示採集的測量值和用於顯示資料的符號或顏色。

![Screenshot of a memory anatomy page]({{site.url}}/assets/images/docs/tools/devtools/memory_chart_anatomy.png){:width="100%"}

The **Memory Size Scale** y-axis automatically adjusts to the
range of data collected in the current visible chart range.

Y 軸上 **記憶體大小刻度** 會自動調整到當前圖表收集的資料範圍內。

The quantities plotted on the y-axis are as follows:

y 軸上繪製的資料包含：

**Dart/Flutter Heap**
<br> Objects (Dart/Flutter objects) in the heap.

**Dart/Flutter Heap（Dart/Flutter 堆）**
<br> 堆中的物件 (Dart/Flutter 物件)。

**Dart/Flutter Native**
<br> Memory that is not in the Dart/Flutter heap but
  is still part of the total memory footprint.
  Objects in this memory would be native objects
  (for example, from a memory read from a file,
  or a decoded image). The native objects are exposed
  to the Dart VM from the native OS (such as Android,
  Linux, Windows, iOS) using a Dart embedder.
  The embedder creates a Dart wrapper with a finalizer,
  allowing Dart code to communicate with these native resources.
  Flutter has an embedder for Android and iOS.
  For more information, see [Dart on the Server][server]
  or [Custom Flutter Engine Embedders][embedder].

**Dart/Flutter Native（Dart/Flutter 原生物件）**
<br> 不在 Dart/Flutter 堆中，但仍然佔用總記憶體的一部分。
  該記憶體中儲存了原生物件（例如，檔案讀取或者圖片解碼的所佔用的記憶體）。
  原生物件透過 Dart 嵌入層，從原生作業系統
  （如 Android、Linux、Windows、iOS）暴露給 Dart VM。
  嵌入層使用 finalizer 建立一個 Dart 包裝類，允許 Dart 程式碼與這些原生資源通訊。
  Flutter 有一個用於 Android 和 iOS 的嵌入層。更多資訊，
  參閱 [伺服器端應用 Dart][server] 或 [自訂 Flutter 引擎嵌入層][embedder]。

**Timeline**
<br> The timestamps of all collected memory statistics
  and events at a particular point in time (timestamp).

**Timeline（時間線）**
<br> 在特定時間點採集的所有記憶體統計資料和事件的時間戳（時間戳）。

**Raster Cache**
<br> The size of the Flutter engine's raster cache layer(s)
  or picture(s), while performing the final rendering
  after compositing.  For more information,
  see [Flutter Architectural Overview][architecture]
  and [DevTools Performance][performance].

**Raster Cache（光柵快取）**
<br> 合成後執行最終渲染時顫振引擎光柵快取層或圖片的光柵快取大小。
  詳情參閱 [Flutter 架構概覽][architecture]
  和 [DevTools 效能檢視][performance]。

**Allocated**
<br> The current capacity of the heap is typically slightly
  larger than the total size of all heap objects.

**Allocated（已分配記憶體）**
<br> 堆當前的容量，通常略大於所有堆物件的總大小。

**RSS - Resident Set Size**
<br> The resident set size displays the
  amount of memory for a process.
  It doesn't include memory that is swapped out.
  It includes memory from shared libraries that are
  loaded, as well as all stack and heap memory.

**RSS - Resident Set Size（常駐集）**
<br> 常駐集大小顯示處理序的記憶體量。
  包含載入的共享庫中的記憶體，以及所有堆疊和堆記憶體，不包含互動的記憶體。

For more information, see [Dart VM internals][vm].

有關更多資訊，請參閱 [Dart 虛擬機器結構][vm]。

### Hover card

### 懸浮窗

Clicking a chart displays a vertical yellow line where
the click occurred on the x-axis (Timestamp).
A hover card displays the collected information:

點選圖表會在 X 軸（時間戳）上顯示一條垂直黃線，
同時顯示帶有所收集資訊的懸浮窗：

![Screenshot of the basic memory chart]({{site.url}}/assets/images/docs/tools/devtools/memory_basic_chart.png)

**Memory Events**
<br> Memory Events recorded in the Event Pane.
  This includes VM GC, User Initiated GC,
  User Initiated Snapshot, Auto-Snapshot,
  Allocation Monitoring, and Reset of Accumulators

**Memory Events（記憶體事件）** 事件視窗中記錄的記憶體事件，
<br> 例如虛擬機器自動 GC、使用者啟動的 GC、使用者儲存的快照、自動快照、分配監控和重置累加資料。

**Dart / Flutter Memory**
<br> Displays collected data Capacity, Used, External, RSS,
  Raster Cache (pictures/layers)

**Dart / Flutter Memory（Dart / Flutter 記憶體）**
<br> 收集的資料容量、已用資料、外部資料、RSS、光柵快取（影象/圖層）。

**Flutter and User Events**
<br> Includes extension events,
  such as `Flutter.ImageSizesForFrame`,
  user custom events. For more information,
  see [Events](#events-pane).

**Flutter and User Events（Flutter 和自訂的事件）**
<br> 擴充事件，例如 `Flutter.ImageSizesForFrame`，
  使用者自訂事件。參閱 [事件](#events-pane)。

Aggregate events, as the name implies,
collects all the events nearest a particular
timestamp (tick) and displays the events to the x-axis'
closest tick.

顧名思義，集合事件收集了最接近特定時間戳（tick）的所有事件，並將事件顯示到 x 軸最近的位置。

If more than one event was collected at this timestamp,
a dark magenta triangle is displayed with the aggregate
list of events. The aggregate events collects all the events
nearest a particular timestamp (tick) and displays the events
on the x-axis near the closest tick.
Expanding an event displays the values for that event:

![Aggregate Events]({{site.url}}/assets/images/docs/tools/devtools/memory_multi_events.png){:width="25px"}

如果此時間戳處收集了多個事件，則會顯示一個深色三角形，包含事件集合的列表。
集合事件收集了最接近特定時間戳 (tick) 的所有事件，並將事件顯示到 x 軸最近的位置。
展開將顯示每個事件的更多資訊：

If only one event is collected,
a lighter magenta triangle color is
displayed with the single event values:

如果只收集了一個事件，則會顯示一個淺色三角形，並顯示單個事件資訊：

![Single Events]({{site.url}}/assets/images/docs/tools/devtools/memory_one_event.png){:width="23px"}

If the Android memory chart is displayed,
then the collected Android data is separated
into **Dart / Flutter Memory** and
**Flutter and User Events** categories:

如果顯示 Android 記憶體圖表，則 Android 部分採集的資料將顯示在
「Dart / Flutter 記憶體」和「Flutter 和自訂的事件」之間。例如：

![Hovercard of Android chart is visible]({{site.url}}/assets/images/docs/tools/devtools/memory_android_hovercard.png)

### Android chart

### Android 圖表

When connected to an Android app,
DevTools collects Android's ADB (Android Debug Bridge)
meminfo from an ADB app summary (polled every 500 ms).
This meminfo section is most interesting at a high-level.
If you collect this info from the ADB tool,
it would similar to the following:

當連線到 Android 應用程式時，DevTools 會從 ADB 連線的應用程式摘要中（每 500 毫秒一次）
收集 Android 的 ADB（Android 除錯通道）記憶體資訊。這部分記憶體資訊非常有趣。
如果你從 ADB 工具中採集此資訊，它將是這樣的：

```sh
> adb shell dumpsys meminfo io.flutter.demo.gallery -d

 App Summary
                       Pss(KB)
                       -------
           Java Heap:     5192
         Native Heap:    11992
                Code:     2132
               Stack:       60
            Graphics:    53700
       Private Other:    42800
              System:    84493
 
               TOTAL:   200369       TOTAL SWAP PSS:    82168
```

This chart is another timeseries graph of the state
of Android memory as the application is running.
The quantities plotted on the y-axis are the above values
(Java Heap, Native Heap, Code size, Stack size,
Graphics stack, System size and total).

此圖表是應用程式執行時 Android 記憶體狀態的另一個時間序列圖。
上述值會被繪製到 y 軸上
（Java 堆、原生堆、程式碼大小、堆疊大小、圖形堆疊、系統大小和總大小）。

Clicking a timestamp (x-position) displays all data points
collected for that time period:

點選時間戳（x 位置）將顯示該時間段內收集的所有資料點。

![Screenshot of Android Memory Chart]({{site.url}}/assets/images/docs/tools/devtools/memory_android.png)

The hover card displays the values of all
collected Android memory data.

懸浮窗將顯示所有采集到的 Android 記憶體資料。

**Time**  
The timestamp for the current data values
collected&mdash;see descriptions below.

**Time (時間戳)**
記憶體資料的採集時刻&mdash;&mdash;請參閱以下的說明。

**Total**  
The total memory in use. Total memory is comprised of
several different categories, all of which are plotted
along the y-axis. These categories are described below.

**Total (總計大小)**
已使用的總記憶體大小，由幾類不同的記憶體組成，
所有類別都沿著 y 軸繪製，如下所述。

**Other**  
Other memory usage corresponds to the ‘Private Other’
field from ADB. This is memory used by the app that the
system isn't sure how to categorize. Note: The Other trace
is a combination of Other and System (shared and system
memory usage), and corresponds to ‘System’ field from ADB.

**其他 (Other)**
「其他」使用情況對應於 ADB 的「Private Other（其他私有）」部分，
這是系統不確定如何分類別的記憶體。
注意：這一部分是「其他」和「系統」
（共享和系統記憶體使用- 對應 ADB 的 System（系統）部分）的組合。

**Code**  
Code memory usage corresponds to the ‘Code’ field from ADB.
This is memory that your app uses for static
code and resources, such as dex byte code,

**Code (程式碼)**
「程式碼」使用情況對應於 ADB 的 「Code（程式碼）」部分。
這是應用程式中靜態程式碼和資源的記憶體，
如 dex 位元組碼、最佳化或編譯的 dex 程式碼、.so 庫和字型。

**Native Heap**  
Native Heap usage corresponds to the ‘Native Heap’ field
from ADB. This is memory from objects allocated from C or
C++ code. Even if you're not using C++ in your app, you might
see some native memory used here because the Android framework
uses native memory to handle various tasks on your behalf. Some
examples of these tasks are handling image assets and other
graphics&mdash;even though the code you've written is in Java or Kotlin.

**Native Heap (本地堆記憶體)**
「本地堆」使用情況對應於 ADB 中的 「Native Heap（本地堆）」部分。
這是從 C 或 C++ 程式碼分配的物件的記憶體。即使你的應用程式中沒有使用 C++，
也可以看到本地記憶體的使用，因為 Android 框架使用本地記憶體來處理各種任務。
比如，用來處理影象資源和其他圖形，即使你編寫的程式碼是 Java 或 Kotlin。

**Java Heap**  
Java Heap usage corresponds to the ‘Java Heap’ field from ADB.
This is memory from objects allocated from Java or Kotlin code.

**Java Heap (Java 堆記憶體)**
「Java 堆」使用情況對應於 ADB 的「Java Heap（Java 堆）」部分。
這是 Java 或 Kotlin 程式碼分配的物件的記憶體。

**Stack**  
Stack usage corresponds to the ‘Stack’ field from ADB.
This is memory used by both native and Java stacks
in your app and usually relates to how many threads
your app is running.

**Stack (堆疊)**
「堆疊」使用情況對應於 ADB 的 「Stack（堆疊）」部分。
這是應用程式中本地和 Java 堆疊使用的記憶體。
通常與應用程式正在執行的執行緒數有關。

**Graphics**  
Graphics usage corresponds to the ‘Graphics’ field from ADB.
This is memory used for graphics buffer queues to
display pixels on the screen, including GL surfaces,
GL textures, and so on. Note: This is memory shared
with the CPU&mdash;not dedicated GPU memory.

**Graphics (圖形)**
「圖形」使用情況對應於 ADB 的 「Graphics（圖形）」部分。
這是用於圖形緩衝佇列在螢幕上顯示畫素的記憶體，包括 GL 曲面、GL 紋理等。
注意：這是與 CPU 共享的記憶體，而不是專用的 GPU 記憶體。

## Memory controls

## 記憶體控制

At the top of the memory page, above the charts,
are several buttons and dropdowns that control
how memory data is displayed:

在記憶體頁面頂部的圖表上方，有幾個按鈕和下拉列表，用於控制記憶體資料的顯示方式。

![Screenshot of a memory controls]({{site.url}}/assets/images/docs/tools/devtools/memory_controls.png){:width="100%"}

**Pause**  
Pauses the memory overview chart to allow inspecting
the currently plotted data. Incoming memory data is still received;
notice the Range selector continues to grow to the right.

**Pause (暫停)**
暫停記憶體概覽圖表的變化，可以檢查已展示的資料，
但仍會接收到傳入的記憶體資料。注意，範圍選擇器會繼續向右增長。

**Resume**  
Resumes the memory overview chart so that it's live, displaying the
current time and the latest memory statistics.

**Resume (恢復)**
恢復記憶體概覽圖表，使其處於活動狀態，
顯示當前時間和最新記憶體統計資料。

**Clear**
Clears all collected data from the memory profiler.

**Clear (清除)**
清除記憶體監控中收集的所有資料。

**Display**
The duration of the x-axis. For example, if this dropdown
is set to "Display 5 minutes", memory data from the last
5 minutes is displayed.

**Display (顯示範圍)**
x 軸的顯示區間。例如，將此下拉列表設定為「顯示 5 分鐘」，
則顯示最近 5 分鐘的記憶體資料。

\- Display 1 Minute  

   顯示最近 1 分鐘

\- Display 5 Minutes  

   顯示最近 5 分鐘

\- Display 10 Minutes  

   顯示最近 10 分鐘

\- Display All Minutes (slider disabled)

   顯示所有時間 - 禁用滑動

**Source**  
Source can be either **Live Feed**, which pulls data from the
connected Flutter app, or one of the available offline data
files, which are created by clicking **Export**.

**資料來源 (Source)**
資料來源可以是 **Live Feed**（從連線的 Flutter 應用程式中獲取即時資料），
也可以是透過點選 **匯出** 建立的本地資料檔案。

**Android Memory**  
Displays or hides the Android Memory Chart.

**Android Memory (Android 記憶體)**
顯示或隱藏 Android 記憶體圖表。

**GC**  
Initiates a garbage collection&mdash;a compaction of the heap.

**GC (垃圾回收)**
啟動垃圾回收&mdash;&mdash;壓縮堆空間。

**Export**  
Saves collected data for the Event Timeline, Memory Overview Chart,
and Android Overview Chart. Files saved are displayed under the
**Source** dropdown. Selecting a file loads the offline data.

**Export (匯出)**
為事件時間線、記憶體概覽圖和 Android 概覽圖儲存已收集的資料。
儲存的檔案顯示在「Source（資料來源）」下拉列表中。選擇檔案將載入資料。

## Memory actions

## 記憶體相關操作

Below the memory charts (Event Timeline, Memory Overview, and Android Overview
charts), are interactive actions used to collect and analyze information about
memory usage while using the application that DevTools is connected.
Two tabs are displayed:

記憶體圖表（事件時間線、記憶體概覽和 Android 概覽圖表）下方有兩個 tab,
分別用於分析和收集 DevTools 所連線到的應用程式的記憶體使用情況。

![Two Tabs Memory Actions]({{site.url}}/assets/images/docs/tools/devtools/memory_two_tabs.png)

### Analysis tab

### Analysis 選項

The **Analysis** tab collects memory snapshots, both user initiated and
auto-collected by DevTools (when DevTools detects memory spikes).
Each snapshot is analyzed.

**分析選項** 會收集由使用者手動儲存和 DevTools 檢測到記憶體峰值時自動儲存的記憶體快照。
每個快照都會被分析並產生分析結果。

### Analysis actions

### Analysis 選項下的操作

The actions available for Analysis are:

analysis 選項下的操作包括：

![Screenshot of a memory actions]({{site.url}}/assets/images/docs/tools/devtools/memory_analysis_actions.png)

**Snapshot**
<br> Clicking the **Snapshot** button makes a request to the
  Dart VM to collect the current state of memory. The
  memory objects can be sorted by attributes, such as class
  name, size, and allocated instances.
  For more information, see [Snapshot classes](#snapshots)).

**Snapshot（快照）**
<br> 點選 Snapshot 按鈕向 Dart VM 發出請求，以儲存記憶體當前的狀態。
  記憶體物件可以按屬性排序，如類別名稱、大小、分配的例項（參見 [快照類](#snapshots)）。

**Treemap**
<br> If the **Treemap** switch is on, the snapshot displays currently
  active memory objects, the last snapshot, and memory in a high-level
  view as a tree map.

**Treemap（樹形圖）**
<br> 如果 Treemap 開關開啟，
  快照將以樹形圖的形式在高階檢視中顯示當前活動的物件、
  最後一個快照和記憶體。（詳情待定）

**Group By**
<br> A dropdown to select how data is grouped, which can either be by
  instance or by class name.

**Group By（分組方式）**
<br> 下拉列表選擇資料的分組方式，可以按例項或按類別名稱分組。

**Collapse All**
<br> Collapse all nodes in the tree table.

**Collapse All（摺疊全部）**
<br> 摺疊樹表中的所有節點。

**Expand All**
<br> Expand all nodes in the tree table.

**Expand All（展開全部）**
<br> 展開樹表中的所有節點。

### Analysis and snapshots

### 分析和快照

All analyses and snapshots are displayed in a table tree:

所有分析和快照都顯示在樹表檢視中：

![Two Tabs Memory Actions]({{site.url}}/assets/images/docs/tools/devtools/memory_table_tree_view.png){:width="100%"}

The snapshots are grouped by library, and then by class.
Each class displays the list of known instances for that class.

快照按庫分組，在庫中按類分組，每個類將顯示該類別的已知例項列表。

A snapshot is a complete view of all memory objects
at a particular point in time. You can navigate in
the tree to a class and its instances (if the constructor
was called to create an instance). If instances exist,
expanding the class displays all live instances (objects).
Clicking an instance of a class brings up the memory inspector.

快照是特定時間點上所有記憶體物件的完整檢視。
在樹中導航到類及其例項（呼叫建構函式建立的例項）。
如果例項存在，擴充類將顯示所有活動例項（物件）。
點選一個類別的例項，將在樹表的右側顯示記憶體的檢查資訊。

![Two Tabs Memory Actions]({{site.url}}/assets/images/docs/tools/devtools/memory_navigate_inspect.png){:width="100%"}

## Snapshots

## 快照

![The snapshot button]({{site.url}}/assets/images/docs/tools/devtools/memory_snapshot.png)

Clicking the **Snapshot** button shows the current state of the heap
and displays all active classes and their instances: 

點選 **Snapshot（快照）** 按鈕顯示堆的所有活動的類及其例項的當前狀態。

![Screenshot of the snapshot classes]({{site.url}}/assets/images/docs/tools/devtools/memory_snapshot_tree.png){:width="100%"}

This pane shows classes allocated in the heap, all instances for a class,
and the ability to inspect a particular instance.

此視窗顯示堆中分配的類、類別的所有例項並且可以檢查某個特定的例項的資訊。

In addition, a snapshot can automatically occur when DevTools notices a
spike in memory usage (when detecting growth of 40% or greater).

此外，當 DevTools 檢測到所用記憶體出現峰值（記憶體增長 > 40%）時，會自動產生快照。

Every snapshot, whether manual or automatic, generates an
analysis of the snapshot. For example, analysis might indicate
that image problems have occurred. In the future, other
common Flutter coding issues will be flagged, such as problems
with fonts, files, or JSON.

每個快照（手動或自動）都將產生快照的分析。例如，可能發生的組映像問題。
將來，其他可能引起記憶體問題的常見 Flutter 編碼問題
（例如字型、檔案、JSON 等）也會加入到分析中。

**Tree View of Memory**  
The tree table view displays outstanding memory events (user
requested snapshots, automatic snapshots, snapshot analyses,
memory allocation monitoring).

**Tree View of Memory (記憶體樹檢視)**
樹表檢視顯示關鍵的記憶體事件（使用者請求的快照、自動快照、快照分析、記憶體分配監控）。

**Memory Inspector**  
Displays either the contents of an analysis, snapshot, or
monitoring based on the currently selected row in the tree view.

**Memory Inspector (記憶體檢查器)**
根據樹狀圖中當前選定的行顯示其分析、快照或監視的內容。

Snapshots have major tree nodes:

快照具有主要的樹節點：

**External**  
Memory that is not in the Dart heap but is still part
of the total memory footprint. Objects in external memory would be
native objects (for example, from a memory read from a file,
or a decoded image). The native objects are exposed to the Dart
VM from the native OS (such as Android, Linux, Windows, iOS)
using a Dart embedder. The embedder creates a Dart wrapper with
a finalizer, allowing Dart code to communicate with these native
resources. Flutter has an embedder for Android and iOS.
For more information, see [Dart on the Server][server] or
[Custom Flutter Engine Embedders][embedder].

**External (外部記憶體)**
不在 Dart 堆中的記憶體但仍然佔用總記憶體一部分的記憶體。
外部記憶體中的物件可以是原生物件（例如，檔案讀取或者圖片解碼的所佔用的記憶體）。
原生物件透過 Dart 嵌入層，從原生作業系統（如 Android、Linux、Windows、iOS）暴露給 Dart VM。
嵌入層使用 finalizer 建立一個 Dart 包裝類，允許 Dart 程式碼與這些原生資源通訊。
Flutter 有一個用於 Android 和 iOS 的嵌入層。
更多資訊，參閱 [伺服器端應用 Dart][server] 或 [自訂 Flutter 引擎嵌入層][embedder]。

**Filtered**  
Displays the packages being filtered.

**Filtered (篩選)**
篩選項中包含了篩選過的 package。

**Packages**  
User packages used by the application and
Src&mdash;the empty Dart package.

**Packages**  
應用使用的使用者 package 和 Src&mdash;&mdash;空的 Dart package。

Under each node are class nodes, an aggregate of the
objects allocated to this class.
Clicking a class name displays all the instances in a class.
Clicking an instance inspects the fields and values of that
instance.

上述每個節點下都是類節點，是分配給該類別的物件的集合。
點選類別名稱將顯示該類別的所有例項。單擊某個例項將顯示其檢查資訊（欄位和值）。

## Inspecting a class instance in a snapshot

## 檢查快照中的類例項

Expanding a class displays the active instances for that class.
Clicking a particular instance displays the type and value of
the fields for that instance:

展開類將顯示該類別的活動的例項。點選某個特定例項將顯示該例項欄位的型別和值。

![Screenshot of the inspecting an instance]({{site.url}}/assets/images/docs/tools/devtools/memory_inspector.png)

## Analysis of a snapshot

## 快照分析

Every snapshot creates a corresponding **Analyzed** entry under the
**Analysis** node (the analyzed date/time corresponds to the matching
snapshot date/time):

每個快照都會在分析節點下建立相應的分析內容（分析的時間對應快照的產生時間）。

![Screenshot of a snapshot analysis]({{site.url}}/assets/images/docs/tools/devtools/memory_analysis.png)

Currently, analysis looks for common problems with images, such as
loading large files instead of scaled thumbnails, or not using a
`ListBuilder` to manage images in a list, and so on.

目前，分析查詢圖片的常見問題。 例如，載入大檔案而不是縮放的縮圖，
沒使用 ListBuilder 管理列表中的圖片等。

The analysis pulls all image-related classes and instances from
a snapshot and organizes the data in one place. This saves you
the pain of having to search through all the classes and inspect the
instances to find the image-related classes.

該分析從快照中提取所有與圖片相關的類和例項，並將資料組織在一個位置。
這樣我們不必搜尋和了解哪些類與圖片相關，並檢查其例項。

In the above analysis, the raw images are located in the **External**
portion of memory, as `_Int32List` (or `_Int64List` for newer phones).
DevTools organizes the images by size into buckets. In this example,
eleven images are 10K-50K, one image is 10M-50M,
seven images are 1M-10M and four images are greater than 50M.
That's a grand total of over 500M to render thumbnail-sized
images on a phone. Obviously, this should be improved!

在上圖的分析中，原始圖片位於外部記憶體的 _Int32List
（或較新手機的 _Int64List）部分，根據例項大小分類到 Buckets 中。
可以看出，圖片大小為 10K-50K 的有 11 張，
10M-50M 有 1 張，1M-10M 有 7 張，大於 50M 的有 4 張。
這個應用程式中共有超過 500M 的圖片在手機上渲染為小圖。

## Allocation tab

## Allocation 選項

The **Allocation** tab monitors the instances of all classes,
reporting the number of objects allocated,
and the number of bytes consumed by all objects.
The numbers are displayed in absolute totals as well
as in accumulated totals. Initially, the accumulated values
(the number of objects and their size in bytes) are
equal to the initial totals at the time of the first
monitor request. The accumulators can be reset to zero at
any time such that the next monitor request returns the
accumlated values since the last reset.

**Allocation** 選項可以監控所有類別的例項，
報告分配的物件數和所有物件消耗的位元組數。
數字以絕對總數和累計總數顯示。
一開始，累計資料（物件數量和位元組大小）
等於第一次發起監控請求時的初始總數。
可以隨時將累計資料重置為零，
這樣下次監控請求將返回自上次重置以來的累計值。

Additionally, a small set of classes can track the allocation of each instance
of a class. The tracking captures a stack trace when the constructor was
called. The overhead to track these allocations is expensive (slow)
therefore tracking should be used sparingly.

此外，一小組類可以追蹤類別的每個例項的記憶體分配。
呼叫建構函式時，監控器可以捕獲到對應的堆疊。
但追蹤這些分配效能代價很大（緩慢的），因此不要頻繁使用追蹤。

### Allocation actions

### 記憶體分配操作

You can perform the following Allocation actions:

你可以進行如下的記憶體分配操作:

![Screenshot of a memory actions]({{site.url}}/assets/images/docs/tools/devtools/memory_allocations_actions.png){:width="100%"}

**Track**  
Records and monitors the number of instances
and size of all instances in bytes. Clicking
the **Track** button populates a table with
instance allocation data. For each instance in
the allocation table, the **Delta** column reflects
the number of memory allocations since the last reset.

**Track (追蹤)**
記錄和監控所有例項的數量和大小（以位元組為單位）。
點選「Track（追蹤）」按鈕，例項分配資料會顯示在下方的表格中。對於表中的每個例項，
「Delta（增加量）」列表示自上次重置資料以來記憶體的分配大小。

**Reset**  
Resets the accumulator counts (**Delta** columns) for each
instance in the allocation table. The next time the **Monitor**
button is pressed, the **Delta** columns populate with
the new instances and sizes since the last reset.

**Reset (重置)**
重置表中每個例項的累計資料（增加量列）。
下次按下 **Monitor（監控）** 按鈕時，
增加量列將顯示自上次重置以來新分配的例項和所佔空間。

**Search**  
The **Search** field is enabled when the instance allocation data
exists. Typing, or selecting a name from the dropdown,
navigates to that class name in the table.

**Search (搜尋)**
搜尋功能 **Search** 會存在例項分配資料時可用。
輸入或從下拉列表中選擇名稱將跳轉到表中對應的類別。

**Filter**  
When clicked, displays a dialog box listing libraries and class names
that you can select.

**Filter (篩選)**
點選時彈出一個包含所有已展示的函式庫和類別名稱的對話方塊。

### Allocation view

### 分配檢視

Allocations are displayed in a table view that lists the
classes available to the connected application:

已連線的應用程式中所有可用的類別的分配資訊會顯示錶中：

![Two Tabs Memory Actions]({{site.url}}/assets/images/docs/tools/devtools/memory_allocations_overview.png){:width="100%"}

Each row displays the class name, the number of instances and bytes
allocated, with deltas for each (the count since the last reset).

每行顯示類別名稱、例項數和對應分配的增加（自上次重置以來的累計資料）的位元組數。

**Track with Stack Trace**  
If enabled, records the stack trace when the instance
is created (when the class constructor is called).

**Track with Stack Trace (堆疊追蹤)**
如果啟用，則在呼叫類建構函式建立例項時記錄堆疊。

**Class Name**  
Class allocations monitored.

**Class Name (類別名稱)**
監控的類別名稱

**Total Instances**  
Total number of active instances for the class.

**Total Instances (例項總計數量)**
類活動例項總數。

**Delta Instances**  
An accumulator indicating a change to the instance count.
When **Reset** is pressed, the accumulators reset to zero;
then each time the **Track** button is pressed,
the data in the table is updated.

**Delta Instances (例項的增加數量)**
由重置按鈕控制的例項累計資料。
當按下 **Reset（重置）** 時，累計資料重置為零，
然後每次按下 **Track（追蹤）** 按鈕時，當前總計和增加數量都會更新。

**Total Bytes**  
Total number of bytes allocated to all instances of the class.

**總位元組數 (Total Bytes)**
為類別的所有例項分配的總位元組數。

**Delta Bytes**  
An accumulator indicating a change to the number of
instance bytes created. When **Reset** is pressed,
the accumulators reset to zero; then each time the
**Track** button is pressed, the data in the table
is updated.

**Delta Bytes (增加的位元組數)**
由重置按鈕控制的例項位元組變化累計資料。
當按下 **Reset（重置）** 時，累計資料重置為零，
然後每次按下 **Track（追蹤）** 按鈕時，當前總計和增加數量都會更新。

**Timestamp of Last Track**  
The timestamp when the **Track** button was pressed.

**Timestamp of Last Track (上一次啟用追蹤的時間)**
按下 **Track（追蹤）** 按鈕的時間。

**Change Bubble**  
A small bubble indicating that data in the table has
been updated.

**Change Bubble (更改氣泡)**
小氣泡，表示已經在表中記錄並更新的更改。

For more information, see [Allocation tracking](#allocation-tracking).

更多資訊，請參閱 [分配追蹤](#allocation-tracking)。

### Managing the objects and statistics in the heap (Monitor Allocations)

### 管理堆中的物件和統計資料（分配監控）

This feature can help you find memory leaks. Here are some of the
available buttons:

該功能可以幫助你找到記憶體洩露。
以下是一些可用的按鈕：

![The Monitor Allocations button]({{site.url}}/assets/images/docs/tools/devtools/memory_monitor_allocations.png)

Clicking the allocation **Track** button monitors the total
number of instances and bytes allocated for a class.
In addition, two accumulators are provided for instances and bytes
allocated. You can reset these values to zero by pressing
the **Reset Accumulators** button. The mechanism is useful
for finding memory leaks.

點選「Track（追蹤）」按鈕可以監控類分配的例項總數和位元組總數。
此外，為例項的分配維護兩個累計資料（總計數量和增加數量），使用者可以透過操作（按下重置按鈕）重置為零。
該機制對於查詢記憶體洩漏非常有用。

![Reset Accumulators button]({{site.url}}/assets/images/docs/tools/devtools/memory_reset.png)

Pressing the **Reset** button resets the accumulators for all classes
to zero and a "monitor reset" event is sent to the
Event Timeline. Clicking the **Reset** button again resets both
accumulators to zero.

按下 **Reset（重置）** 按鈕時，所有類別的累計資料都會重置為零。當發生重置時，
事件時間線上會出現「監控重置」事件。再次點選 **Reset（重置）** 按鈕將兩個累計資料重置為零。

**Classes**  
Active classes in the heap.

**Classes (類)**
堆中的活動的類別。

**Instances column**  
Total active objects (instances) for all classes in the heap.

**Instances column (例項列)**
堆中所有類別的活動物件（例項）總數。

**Delta column**  
Total number of instances since last **Reset** was pressed.
Clicking the **Reset** button initializes the accumulated
(Delta) instances of a class. This is useful for finding memory leaks.

**Delta column (數量增加列)**
自上次按下「Reset（重置）」後所有例項的累計數量。
點選重置按鈕重置類例項的累計數量。這對於查詢記憶體洩漏非常有用。

**Bytes column**  
Total bytes consumed for all instances of a class in the heap.

**Bytes column (位元組列)**
堆中類別的所有例項的總位元組數。

**Delta column**  
The number of bytes allocated since the **Reset** was last pressed.
Clicking the **Reset** button initializes the accumulated (Delta) bytes for
all instances of a class. This is useful for finding memory leaks.

**Delta column (位元組增加列)**
自上次按下「Reset（重置）」後所有例項的累計位元組數。
點選重置按鈕重置類例項的累計位元組數。這對於查詢記憶體洩漏非常有用。

## Allocation tracking

## 分配追蹤

In addition to tracking the number of objects and bytes consumed
for all instances of a class, a stack trace can be recorded when a
class's constructor is called. This can help narrow where allocations
might have gone astray.
To do this, enable the **Track** checkbox for a class. For example:

除了追蹤一個類別的所有例項的物件數和位元組數外，還可以在呼叫類別的建構函式時記錄堆疊，
來幫助縮小分配可能出錯的範圍。為此，請選中類別的追蹤 (**Track**) 複選框，例如：

![Enable Stack Trace Tracking]({{site.url}}/assets/images/docs/tools/devtools/memory_enable_stacktrace.png){:width="100%"}

Interact with your application. When you want to view the
instances allocation press the **Track** button again
to update the count for the instances being tracked.
For example, 118 instances of `ObjectWithUniqueId`
are being tracked in the following screenshot.
Expanding the instances tracked displays all 
instances and the timestamp when each instance was created:

在你與應用程式互動，想要檢視例項分配時，再次按下「Track（追蹤）」按鈕。
這將更新正在追蹤的例項的數量，例如下圖中的 118。
展開追蹤的例項將顯示所有例項以及建立每個例項的時間，例如：

![Class Tracking]({{site.url}}/assets/images/docs/tools/devtools/memory_tracking.png){:width="100%"}

Selecting an instance displays the call stack at the time the
class's constructor was called and the instance was allocated:

選擇例項將顯示呼叫類別的建構函式（已分配）時的堆疊，例如：

![Call Stack]({{site.url}}/assets/images/docs/tools/devtools/memory_tracking_callstack.png){:width="100%"}

## Filtering, searching, and auto-complete

## 篩選、搜尋和自動自動完成

Both the **Analysis** and **Allocations** tabs support
searching and filtering. Begin typing name of the class
you'd like to find (for example, `ObjectWithUniqueId`)
to display a list that matches the characters
typed so far. The first item in the list is highlighted:

「Analysis（分析）」和「Allocations（分配）」選項都支援搜尋和篩選。
輸入要查詢的類別的名稱，例如，`ObjectWithUniqueId`
將返回與與輸入字元匹配的列表。列表中的第一項高亮顯示。

**ENTER**  
Selects the highlighted line (GlobalObjectKey) and
navigates to the row with that class name in the
active **Snapshot** table or the **Allocations** table.

**Enter鍵**
選擇高亮顯示的行（GlobalObjectKey）
跳轉到當前樹表檢視 **快照** 或表 **分配** 中該類別名稱對應的行。

**UP/DOWN arrows**  
Rotates through the list of possible matches highlighting
the next item in the list.

**上/下箭頭**
在可能匹配的列表中跳轉，突出顯示列表中的下一項。

**ESCAPE**  
Clears and cancels all searching.

**ESC 鍵**
清除並取消所有搜尋。

![Searching]({{site.url}}/assets/images/docs/tools/devtools/memory_search_1.png)

Typing more characters to further narrow down the list of possible
class names. For example, typing **Obje** displays:

輸入更多字元來縮小類別名稱範圍，例如鍵入 **Obje** 顯示：

![Narrower Search]({{site.url}}/assets/images/docs/tools/devtools/memory_search_2.png)

Finally, typing **ObjectW** displays the exact match:

最後，輸入 **ObjectW** 顯示精確匹配：

![Narrowed Search]({{site.url}}/assets/images/docs/tools/devtools/memory_search_3.png)

### Filtering

### 篩選

Filtering allows you to move libraries and classes from the main list
to a **Filter** group. This help reduce the number of classes visible that are
less important while profiling memory:

篩選用於將庫和類從主列表（表）移動到篩選組，幫助減少在分析記憶體時不太重要的可見類別的數量。

![Filtering]({{site.url}}/assets/images/docs/tools/devtools/memory_filtering.png){:width="100%"}

**Hide Private Classes**  
Class names prefix with an underscore.

**Hide Private Classes (隱藏私有類)**
字首帶有下劃線類別的類別。

**Hide Classes with No Instances**  
Classes never constructed are filtered.

**Hide Classes with No Instances (隱藏沒有例項的類)**
從未構造的類將被過濾。

**Hide Libraries with No Instances**  
All classes in a library never constructed
the library is filtered.

**Hide Libraries with No Instances (隱藏沒有例項的函式庫)**  
庫中所有類都從未被例項化過，將會被隱藏

**Hide Libraries or Packages**  
List of all libraries used in your application
are displayed. By default the libraries enabled
above are filtered out (for example, dart:*,
package:flutter*, and so on).
The libraries filtered out can
be enabled if you are interested in Dart core
libraries and classes or the Flutter framework.

**Hide Libraries or Packages (隱藏庫或 package)**
顯示應用程式中使用的所有庫的列表。預設情況下啟用，
系統相關庫將被過濾掉（例如：`dart:*`、`package:flutter*`）。
如果你對 Dart 核心函式庫和類或 Flutter 框架感興趣，可以關閉自動過濾掉的函式庫。

### Setting

### 設定

The Memory profiler has its own settings dialog:

記憶體分析器有自己的設定對話方塊：

![Settings]({{site.url}}/assets/images/docs/tools/devtools/memory_settings.png){:width="100%"}

**Collect Android Memory Statistics using ADB**  
By default, if DevTools is connected to your
application through an Android device or emulator,
Android memory statistics are not collected.
Collecting with ADB can be expensive and might hide
performance issues in your app.

**使用 ADB 收集 Android 記憶體統計資訊**
預設情況下，如果 DevTools 透過 Android 裝置或模擬器連線到應用，
則不會收集 Android 記憶體統計資訊。
使用 ADB 收集開銷很大，並且可能會隱藏應用程式中的效能問題。

**Display Data in Units (B, K, MB, GB)**  
By default, data displayed in the hover card
is scaled using units instead of raw values.
Turning this off displays the raw numbers.
For example, 125M displays as 125,235,712

**以單位（B、K、MB、GB）顯示資料**
預設情況下，懸浮窗中顯示的資料使用單位而不是原始值。
關閉此選項將顯示原始數字，例如 125M 將顯示為 125,235,712。

**Enable advanced memory settings**  
If enabled, the GC button is available to
ask the VM to garbage collect memory (manually).
This manual GC is only a request to the VM. The
VM might decide to do no compaction, some compaction,
or complete compaction of the heap.

**啟用高階記憶體設定**
如果啟用，將顯示 GC 按鈕，請求虛擬機器（手動）垃圾收集記憶體。
此手動 GC 只是對虛擬機器的請求。
虛擬機器可能不壓縮、部分壓縮或完全壓縮堆。

## Memory problem case study

## 記憶體問題案例學習

A memory leak study using large network images is available
on GitHub. You can follow the step-by-step instructions
on using the Devtools memory profiler to detect the
memory problem and fix it. For more information,
see the [memory leak case study][case_study].

使用大量網路圖片導致記憶體洩漏的學習，以及有關使用 DevTools 記憶體分析器、
檢測記憶體問題和修復問題的分步說明，請參閱 [案例研究][case_study]。

## Glossary of VM terms

## VM 虛擬機器術語表

The following computer science terms will help you better
understand how your application uses memory.

以下是一些計算機科學概念，
它們將幫助你更好地理解應用程式如何使用記憶體。

**Garbage collection (GC)**  
GC is the process of searching the
heap to locate, and reclaim, regions of "dead" memory&mdash;memory
that is no longer being used by an application. This process
allows the memory to be re-used and minimizes the risk of an
application running out of memory, causing it to crash. Garbage
collection is performed automatically by the Dart VM. In DevTools,
you can perform garbage collection on demand by clicking the
GC button.

**垃圾回收 (GC)**
GC 是搜尋堆以定位和回收應用程式不再使用的記憶體區域的過程。
這個過程允許重新使用記憶體，將應用程式由於記憶體不足導致的崩潰風險降至最低。
GC 由 Dart VM 自動執行。在 DevTools 中，
你可以透過點選 GC 按鈕按需執行垃圾回收。

**Heap**  
Dart objects that are dynamically allocated live in a portion of
memory called _the heap_. An object allocated from the heap is freed
(eligible for garbage collection) when nothing points to it,
or when the application terminates. When nothing points to an
object, it is considered to be dead. When an object is pointed
to by another object, it is live.

**堆**
動態分配的 Dart 物件存在於稱為堆的記憶體部分中。當沒有任何物件指向它，
或者當應用程式退出時。在堆中分配的物件將被回收（符合 GC 的條件）。
當沒有任何東西指向某個物件時，它不處於存活狀態。
當一個物件被另一個物件指向時，它是存活的。

**Isolates**  
Dart supports concurrent execution by way of isolates,
which you can think of processes without the overhead.
Each isolate has its own memory and code that can't be
affected by any other isolate. For more information,
see [The Event Loop and Dart][event-loop].

**Isolates**  
Dart 透過 Isolates 的方式支援併發，你可以認為這樣的過程沒有開銷。
每個 Isolates 都有自己的記憶體和程式碼，不受任何其他 Isolates 的影響。
更多資訊，請參見 [事件迴圈和 Dart][event-loop]。

**Memory leak** 
A memory leak occurs when an object is live
(meaning that another object points to it), but it is not being
used (so it shouldn't have any references from other objects).
Such an object can't be garbage collected, so it takes up space
in the heap and contributes to memory fragmentation.
Memory leaks put unnecessary pressure on the VM and can be
difficult to debug.

**記憶體洩漏**
當一個物件處於存活狀態（意味著有其它物件指向它），
但它沒有被使用時（因此它不應該有來自其他任何物件的參考），就會發生記憶體洩漏。
這樣的物件不能被垃圾收集，因此它會佔用堆中的空間並導致記憶體碎片。
記憶體洩漏會給虛擬機器帶來不必要的壓力，並且可能很難除錯。

**Virtual machine (VM)**  
The Dart virtual machine is a piece of
software that directly executes Dart code.

**VM 虛擬機器**
Dart 虛擬機器是一種直接執行 Dart 程式碼的軟體。

[architecture]: {{site.url}}/resources/architectural-overview
[performance]: {{site.url}}/development/tools/devtools/performance
[server]: https://dart-lang.github.io/server/server.html
[embedder]: {{site.repo.flutter}}/wiki/Custom-Flutter-Engine-Embedders
[vm]: https://mrale.ph/dartvm/
[event-loop]: {{site.dart-site}}/articles/archive/event-loop
[profile mode]: {{site.url}}/testing/build-modes#profile
[release mode]: {{site.url}}/testing/build-modes#release
[debug mode]: {{site.url}}/testing/build-modes#debug
[Don't Fear the Garbage Collector]: {{site.flutter-medium}}/flutter-dont-fear-the-garbage-collector-d69b3ff1ca30
[case_study]: {{site.repo.organization}}/devtools/tree/master/case_study/memory_leaks/images_1
