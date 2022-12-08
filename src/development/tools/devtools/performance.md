---
title: Using the Performance view
title: 使用效能檢視 (Performance view)
description: Learn how to use the DevTools performance view.
description: 學習如何使用開發者工具的效能檢視。
tags: Flutter開發工具,DevTools
keywords: 開發者工具,效能檢視,Dart,效能最佳化
---

{{site.alert.note}}

  The performance view works with Dart CLI and mobile apps only.
  Use Chrome DevTools to [generate timeline events][]
  of a web app.

  效能檢視適用於移動應用和 Dart 命令列工具。
  對於 web 應用程式，請使用 Chrome 自帶的開發者工具
  [產生時間線事件][generate timeline events]。

{{site.alert.end}}

## What is it?

## 它是什麼?

The performance view offers timing and performance information for activity in
your application. It consists of three parts, each increasing in granularity.

效能檢視提供了應用活動的時間線以及效能資訊。它由三個部分組成，且每個部分的粒度都更加細。

* Flutter frames chart (Flutter apps only)

  Flutter 火焰圖（僅支援 Flutter 應用）

* Timeline events chart

  時間線事件圖

* CPU profiler

  CPU 監控

{{site.alert.note}}

  **If you are running a Flutter application, 
  use a profile build to analyze performance.**
  Cpu profiles are not indicative of release performance 
  unless your Flutter application is run in profile mode.

  **對於 Flutter 應用程式，需要使用 profile 建構模式才能使用效能分析**
  如果你希望你的 Flutter 應用程式效能與生產模式下相同
  且希望使用效能分析工具，請使用效能模式。

{{site.alert.end}}

## Flutter frames chart

This chart contains Flutter frame information for your application. Each bar set
in the chart represents a single Flutter frame. The bars are color-coded to
highlight the different portions of work that occur when rendering a Flutter
frame: work from the UI thread and work from the raster thread (previously known
as the GPU thread).

![Screenshot from a performance snapshot]({{site.url}}/assets/images/docs/tools/devtools/performance-flutter-frames-chart.png){:width="100%"}

Selecting a bar from this chart centers the flame chart below on the timeline
events corresponding to the selected Flutter frame. The events are highlighted
with blue brackets.

![Screenshot from a timeline recording]({{site.url}}/assets/images/docs/tools/devtools/performance-timeline-events-chart-selected-frame.png){:width="100%"}

### UI

The UI thread executes Dart code in the Dart VM. This includes
code from your application as well as the Flutter framework.
When your app creates and displays a scene, the UI thread creates
a layer tree, a lightweight object containing device-agnostic
painting commands, and sends the layer tree to the raster thread
to be rendered on the device. Do **not** block this thread.

UI 執行緒執行 Dart VM 中的 Dart 程式碼。
它包括你的應用程式和 Flutter 框架的所有程式碼。
當你建立或開啟一個頁面，
UI 執行緒會建立一個圖層樹和一個輕量級的與裝置無關的繪製指令集，
並把圖層樹交給裝置的 raster（網格）執行緒進行渲染。
**不要** 阻塞這個執行緒。

### Raster

### 網格執行緒

The raster thread (previously known as the GPU thread) executes 
graphics code from the Flutter Engine.
This thread takes the layer tree and displays it by talking to
the GPU (graphic processing unit). You cannot directly access
the raster thread or its data, but if this thread is slow, it's a
result of something you've done in the Dart code. Skia, the
graphics library, runs on this thread.

網格化執行緒（也就是我們之前知道的 GPU 執行緒）執行 Flutter 引擎中圖形相關的程式碼。
這個執行緒透過與 GPU (圖形處理單元) 通訊，獲取圖形樹並顯示它。
你不能直接存取 Raster 執行緒或它的資料，但如果這個執行緒較慢，
那它肯定是由你的 Dart 程式碼引起的。
圖形化庫 Skia 執行在這個執行緒上，有時候也稱它為光柵執行緒。

Sometimes a scene results in a layer tree that is easy to construct,
but expensive to render on the raster thread. In this case, you
need to figure out what your code is doing that is causing
rendering code to be slow. Specific kinds of workloads are more
difficult for the GPU. They might involve unnecessary calls to
`saveLayer()`, intersecting opacities with multiple objects,
and clips or shadows in specific situations.

有時候一個頁面的圖形層樹比較容易建構但 raster 執行緒的渲染卻比較昂貴。
在這種情形下，你需要找出導致渲染變慢的程式碼。
為 GPU 設定特定多種型別的 workload 是相當困難的。
在一些特定的情形下，多個物件的透明度重疊、剪下或陰影，
有可能會導致不必要的 `saveLayer()` 的呼叫。

For more information on profiling, see
[Identifying problems in the GPU graph][GPU graph].

更多詳細資訊，請檢視文件 [定位 GPU 圖表中的問題][GPU graph]。

### Jank (slow frame)

### 卡頓 (Jank)

The frame rendering chart shows jank with a red overlay.
A frame is considered to be janky if it takes more than
~16 ms to complete (for 60 FPS devices). To achieve a frame rendering rate of
60 FPS (frames per second), each frame must render in
~16 ms or less. When this target is missed, you may
experience UI jank or dropped frames.

幀渲染圖表使用紅色圖層顯示幀延時。
如果一幀的渲染時間超過 16ms，則會被認為此幀是延時的，
為了達到幀渲染頻率到 60 FPS (每秒幀數)，
每一幀的渲染時間必須等於或少於 16 ms。
如果沒有達到這個目標，你會發現 UI 不流暢或丟幀。

For more information on how to analyze your app's performance,
see [Flutter performance profiling][].

更多關於效能分析資訊，請檢視文件：[Flutter 效能分析][Flutter performance profiling]。

### Shader compilation

### 著色器渲染

Shader compilation occurs when a shader is first used in your Flutter
app. Frames that perform shader compilation are marked in dark
red:

在 Flutter 應用中，著色器會在初次使用時發生渲染。參與了著色器編譯的建構幀已標記為深紅色：

![Screenshot of shader compilation for a frame]({{site.url}}/assets/images/docs/tools/devtools/shader-compilation-frames-chart.png)

For more information on how to reduce shader compilation jank, see [Reduce
shader compilation jank on mobile][].

想要了解更多關於如何減少著色器快取卡頓的內容，閱讀
[在移動端減少著色器編譯卡頓][Reduce shader compilation jank on mobile]。

## Timeline events chart

## 時間線事件表

The timeline events chart shows all event tracing from your application.
The Flutter framework emits timeline events as it works to build frames, draw
scenes, and track other activity such as HTTP traffic. These events show up here
in the Timeline. You can also send your own Timeline events via the
dart:developer
[Timeline]({{site.api}}/flutter/dart-developer/Timeline-class.html)
and [TimelineTask]({{site.api}}/flutter/dart-developer/TimelineTask-class.html)
APIs.

![Screenshot of timeline events for a frame]({{site.url}}/assets/images/docs/tools/devtools/performance-timeline-events-chart.png){:width="100%"}

The flame chart supports zooming and panning:
* To zoom, scroll up and down with the mouse wheel / trackpad
* To pan horizontally, either click and drag the chart or scroll horizontally
with the mouse wheel / trackpad
* To pan vertically, either click and drag the chart or use **alt + scroll**
* The WASD keys also work for controlling zoom and horizontal scroll position

You can click an event to view CPU profiling information in the CPU profiler
below, described in the next section.

## Enhance tracing 

## 增強的追蹤選項

To view more detailed tracing in the timeline events chart,
use the options in the enhance tracing dropdown:

想要在時間線事件圖表裡檢視更詳細的追蹤內容，請使用增強的追蹤下拉控制項裡的選項：

{{site.alert.note}}

  Frame times may be negatively affected when these options are enabled.

  啟用該選項後，幀建構時間可能會受到影響。

{{site.alert.end}}

![Screenshot of enhance tracing dropdown]({{site.url}}/assets/images/docs/tools/devtools/enhance-tracing.png)

To see the new timeline events,
reproduce the activity in your app that you are interested in tracing,
and then select a frame to inspect the timeline.

你可以重複操作你想要追蹤的行為來檢視新的時間線事件，
操作後可以在時間線中選擇一個建構幀進行檢視。

### Track widget builds

### 追蹤 widget 的建構

To see the build() method events in the timeline,
enable the Track Widget Builds option.
The name of the widget is shown in the timeline event.

想要在時間線中檢視 `build()` 方法的事件，啟用 Track Widget Builds 選項。
時間線中將出現 widget 對應名稱的事件。

![Screenshot of track widget builds]({{site.url}}/assets/images/docs/tools/devtools/track-widget-builds.png)

### Track layouts

### 追蹤佈局

To see render object layout events in the timeline,
enable the Track Layouts option:

想要在時間線中檢視 `RenderObject` 佈局建構的事件，啟用 Track Layouts 選項：

![Screenshot of track layouts]({{site.url}}/assets/images/docs/tools/devtools/track-layouts.png)

### Track paints

### 追蹤繪製

To see render object paint events in the timeline,
enable the Track Paints option:

想要在時間線中檢視 `RenderObject` 的繪製事件，啟用 Track Paints 選項：

![Screenshot of track paints]({{site.url}}/assets/images/docs/tools/devtools/track-paints.png)

## More debugging options

## 更多除錯選項

To diagnose performance problems related to rendering layers,
toggle off a rendering layer.
These options are enabled by default.

想要診斷渲染圖層相關的問題，請先關閉渲染層。
下述的選項將會預設啟動。

To see the effects on your app's performance,
reproduce the activity in your app.
Then select the new frames in the frames chart
to inspect the timeline events
with the layers disabled.
If Raster time has significantly decreased,
excessive use of the effects you disabled might be contributing
to the jank you saw in your app.

想要檢視你的應用的效能影響，請嘗試以相同的操作重現效能問題。
在渲染層關閉的情況下，於建構幀圖表裡選擇一個新的建構幀，
檢視它的時間線細節。
如果 Raster 執行緒的時間消耗有顯著降低，
那麼你禁用的效果的濫用可能是導致卡頓的主要原因。

**Render Clip layers**
<br> Disable this option  to check whether excessive use of clipping
     is affecting performance.
     If performance improves with this option disabled,
     try to reduce the use of clipping effects in your app.

**渲染裁剪的圖層**
<br> 禁用該選項來檢查已使用的裁剪圖層是否影響了效能。
     如果禁用後效能有顯著提升，請嘗試減少你的應用中裁剪效果的使用。

**Render Opacity layers**
<br> Disable this option to check whether
     excessive use of opacity effects are affecting performance.
     If performance improves with this option disabled,
     try to reduce the use of opacity effects in your app.

**渲染透明度圖層**
<br> 禁用該選項來檢查已使用的透明度圖層是否影響了效能。
     如果禁用後效能有顯著提升，請嘗試減少你的應用中透明度效果的使用。

**Render Physical Shape layers**
<br> Disable this option to check whether excessive
     use of physical modeling effects are affecting performance,
     such as shadows or elevation.
     If performance improves with this option disabled,
     try to reduce the use of physical modeling effects in your app.

**渲染物理形狀圖層**
<br> 禁用該選項來檢查已使用的物理形狀圖層是否影響了效能，例如陰影和背景特效。
     如果禁用後效能有顯著提升，請嘗試減少你的應用中物理效果的使用。

![Screenshot of more debugging options]({{site.url}}/assets/images/docs/tools/devtools/more-debugging-options.png)

## Import and export

## 匯入匯出

DevTools supports importing and exporting performance snapshots.
Clicking the export button (upper-right corner above the
frame rendering chart) downloads a snapshot of the current data on the
performance page. To import a performance snapshot, you can drag and drop the
snapshot into DevTools from any page. **Note that DevTools only
supports importing files that were originally exported from DevTools.**

DevTools 支援匯入和匯出時間線快照。單擊 export 按鈕
(幀渲染圖表右上角) 下載當前時間線的快照。
要匯入時間線快照，可以從任何頁面拖放快照到 DevTools。
提示：DevTools 僅支援匯入 DevTools 匯出的原始檔。

[generate timeline events]: {{site.developers}}/web/tools/chrome-devtools/evaluate-performance/performance-reference
[GPU graph]: {{site.url}}/perf/ui-performance#identifying-problems-in-the-gpu-graph
[Flutter performance profiling]: {{site.url}}/perf/ui-performance
[Reduce shader compilation jank on mobile]: {{site.url}}/perf/shader
[Import and export]: #import-and-export
