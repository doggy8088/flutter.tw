---
title: Using the Performance view
title: 使用效能檢視 (Performance view)
description: Learn how to use the DevTools performance view.
description: 學習如何使用開發者工具的效能檢視。
tags: Flutter開發工具,DevTools
keywords: 開發者工具,效能檢視,Dart,效能最佳化
---

{{site.alert.note}}

  The DevTools performance view works for Flutter mobile and desktop apps.
  For web apps, Flutter adds timeline events to the
  performance panel of Chrome DevTools instead. 
  To learn about profiling web apps, check out [Debugging web performance][].

  效能檢視可用於 Flutter 移動端、Flutter 桌面端、Dart 和 Flutter 網頁端應用。
  Flutter 會在 Chrome DevTools 中新增時間線事件。
  若你想了解如何分析 Flutter 網頁應用的效能，請參閱
  [除錯 Web 應用效能][Debugging web performance]。

{{site.alert.end}}

[Debugging web performance]: {{site.url}}/perf/web-performance

The performance page can help you diagnose performance
problems and UI jank in your application.
This page offers timing and performance information
for activity in your application.
It consists of several tools to help you identify
the cause of poor performance in your app:

效能頁面可以幫助您診斷應用中的效能問題和 UI 卡頓。
該頁面提供了應用中活動的時間和效能資訊。
它由幾個工具組成，可幫助您識別應用中效能不佳的原因：

* Flutter frames chart (Flutter apps only)

  Flutter 幀圖表（僅 Flutter 應用）

* Frame analysis tab (Flutter apps only)

  幀分析標籤頁（僅 Flutter 應用）

* Raster stats tab (Flutter apps only)

  光柵統計標籤頁（僅 Flutter 應用）

* Timeline events trace viewer (all native Dart applications)

  時間軸事件追蹤檢視器（所有原生 Dart 應用）

* Advanced debugging tools (Flutter apps only)

  高階除錯工具（僅 Flutter 應用）


{{site.alert.secondary}}

  **Use a [profile build][] of your application to analyze performance.**
  Frame rendering times aren't indicative of release performance
  when running in debug mode. Run your app in profile mode,
  which still preserves useful debugging information.

  **使用 [profile build][] 來分析效能。**
  幀渲染時間在除錯模式下不代表釋出模式的效能。
  請在 profile 模式下執行應用，它仍然保留了有用的除錯資訊。

{{site.alert.end}}

[profile build]: {{site.url}}/testing/build-modes#profile

The performance view also supports importing and exporting of
data snapshots. For more information,
check out the [Import and export][] section.

效能檢視還支援匯入和匯出資料快照。
更多資訊，請檢視 [匯入和匯出][Import and export] 部分。

### What is a frame in Flutter?

### Flutter 中的幀是什麼？

Flutter is designed to render its UI at 60 frames per second
(fps), or 120 fps on devices capable of 120Hz updates.
Each render is called a _frame_.
This means that, approximately every 16ms, the UI updates
to reflect animations or other changes to the UI. A frame
that takes longer than 16ms to render causes jank 
(jerky motion) on the display device.

Flutter 的 UI 設計為每秒渲染 60 幀（fps），或者在支援 120Hz 更新的裝置上為 120 幀。
每次渲染稱為一幀。
這意味著，大約每 16ms，UI 就會更新以反映動畫或 UI 的其他更改。
渲染時間超過 16ms 的幀會導致顯示裝置上的卡頓（抖動）。

## Flutter frames chart

## Flutter 幀圖表

This chart contains Flutter frame information for your application.
Each bar set in the chart represents a single Flutter frame.
The bars are color-coded to highlight the different portions
of work that occur when rendering a Flutter frame: work from
the UI thread and work from the raster thread (previously known
as the GPU thread).

此圖表包含了應用的幀資訊。
圖表中每組條形圖代表每一幀。
這些條形圖以顏色區分渲染幀時進行的不同工作：
UI 執行緒和光柵執行緒（以前稱為 GPU 執行緒）。

This chart contains Flutter frame timing information for your
application. Each pair of bars in the chart represents a single
Flutter frame. Selecting a frame from this chart updates the data
that is displayed below in the [Frame analysis](#frame-analysis-tab) tab
or the [Timeline events](#timeline-events-tab) tab.
(As of [DevTools 2.23.1][], the [Raster stats](#raster-stats-tab)
is a standalone feature without data per frame).

此圖表在時間線上顯示應用的幀資訊。
圖表中每組條形圖代表每一幀。
從圖表選中一幀，
就會更新下面 [幀分析](#frame-analysis-tab) 標籤頁
或 [時間線事件](#timeline-events-tab) 標籤頁中顯示的資料。
（從 [DevTools 2.23.1][] 開始，[光柵統計](#raster-stats-tab) 是一個獨立的功能，沒有每幀的資料）。

[DevTools 2.23.1]: {{site.url}}/tools/devtools/release-notes/release-notes-2.23.1

The flutter frames chart updates when new frames
are drawn in your app. To pause updates to this chart,
click the pause button to the right of the chart.
This chart can be collapsed to provide more viewing space
for data below by clicking the **Flutter frames** button above the chart. 

在應用程式執行過程中繪製新的幀時，會更新 Flutter 幀圖表。
點選圖表右側的暫停按鈕就可以暫停圖表的更新，
點選圖表上方的 **Flutter frames** 按鈕，
可以將此圖表摺疊起來，為下面的資料提供更多的觀察空間。

![Screenshot of a Flutter frames chart]({{site.url}}/assets/images/docs/tools/devtools/flutter-frames-chart.png)

The pair of bars representing each Flutter frame are color-coded
to highlight the different portions of work that occur when rendering
a Flutter frame: work from the UI thread and work from the raster thread
(previously known as the GPU thread).

每一組條形圖以顏色區分，突出顯示渲染每一幀時進行的不同工作：
來自 UI 執行緒和光柵執行緒（以前稱為 GPU 執行緒）的工作。

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
並把圖層樹交給裝置的光柵執行緒進行渲染。
**不要** 阻塞這個執行緒。

### Raster

### 光柵執行緒 (Raster)

The raster thread (previously known as the GPU thread)
executes graphics code from the Flutter Engine.
This thread takes the layer tree and displays it by talking to
the GPU (graphic processing unit). You can't directly access
the raster thread or its data, but if this thread is slow,
it's a result of something you've done in the Dart code.
Skia, the graphics library, runs on this thread.
[Impeller][] (in the stable channel for iOS)
also uses this thread.

[Impeller]: {{site.url}}/perf/impeller

光柵執行緒（也就是我們之前知道的 GPU 執行緒）執行 Flutter 引擎中圖形相關的程式碼。
這個執行緒透過與 GPU (圖形處理單元) 通訊，獲取圖形樹並顯示它。
你不能直接存取光柵執行緒或它的資料，但如果這個執行緒較慢，
那它肯定是由你的 Dart 程式碼引起的。
圖形化庫 Skia 執行在這個執行緒上。
[Impeller][]（目前處於預覽階段）也將執行在這個執行緒上。

Sometimes a scene results in a layer tree that is easy to construct,
but expensive to render on the raster thread. In this case, you
need to figure out what your code is doing that is causing
rendering code to be slow. Specific kinds of workloads are more
difficult for the GPU. They might involve unnecessary calls to
`saveLayer()`, intersecting opacities with multiple objects,
and clips or shadows in specific situations.

有時候一個頁面的圖形層樹比較容易建構但光柵執行緒的渲染卻比較昂貴。
在這種情形下，你需要找出導致渲染變慢的程式碼。
為 GPU 設定特定多種型別的 workload 是相當困難的。
在一些特定的情形下，多個物件的透明度重疊、剪下或陰影，
有可能會導致不必要的 `saveLayer()` 的呼叫。

For more information on profiling, check out
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
check out [Flutter performance profiling][].

更多關於效能分析資訊，請檢視文件：[Flutter 效能分析][Flutter performance profiling]。

### Shader compilation

### 著色器渲染

Shader compilation occurs when a shader is first used in your Flutter
app. Frames that perform shader compilation are marked in dark
red:

在 Flutter 應用中，著色器會在初次使用時發生渲染。參與了著色器編譯的建構幀已標記為深紅色：

![Screenshot of shader compilation for a frame]({{site.url}}/assets/images/docs/tools/devtools/shader-compilation-frames-chart.png)

For more information on how to reduce shader compilation jank,
check out [Reduce shader compilation jank on mobile][].

想要了解更多關於如何減少著色器快取卡頓的內容，閱讀
[在移動端減少著色器編譯卡頓][Reduce shader compilation jank on mobile]。

## Frame analysis tab

## 幀分析標籤頁

Selecting a janky frame (slow, colored in red)
from the Flutter frames chart above shows debugging hints
in the Frame analysis tab. These hints help you diagnose
jank in your app, and notify you of any expensive operations
that we have detected that might have contributed to the slow frame time.

從上面的 Flutter 幀圖表中選擇一個延時的幀（紅色），
會在幀分析標籤頁中顯示除錯提示。
這些提示可以幫助你診斷應用中的卡頓，
並通知你任何昂貴的操作，我們檢測到這些操作可能會導致幀延時。

![Screenshot of the frame analysis tab]({{site.url}}/assets/images/docs/tools/devtools/frame-analysis-tab.png)

## Raster stats tab

## 光柵統計標籤頁

{{site.alert.note}}

  For best results, this tool should be used with
  the Impeller rendering engine. When using Skia,
  the raster stats reported might be inconsistent
  due to the timing of when shaders are compiled.

  為了獲得最佳的效果，該工具應該和 Impeller 渲染引擎一起使用。
  當使用 Skia 時，由於著色器編譯的時間不同，
  光柵統計報告的資料可能會存在差異。

{{site.alert.end}}

If you have Flutter frames that are janking with
slow raster thread times, this tool might be able
to help you diagnose the source of the slow performance.
To generate raster stats:

如果幀的卡頓來自光柵執行緒，
這個工具也許能夠幫助你診斷效能緩慢的原因。
產生光柵統計的步驟：

1. Navigate to the screen in your app where you are seeing
   raster thread jank.

   在應用程式中導航到你看見光柵執行緒卡頓的畫面。

2. Click **Take Snapshot**.

   點選 **Take Snapshot** 產生快照。

3. View different layers and their respective rendering times.

   檢視不同圖層和它們各自的渲染時間。

If you see an expensive layer, find the Dart code in your app
that is producing this layer and investigate further.
You can make changes to your code, hot reload,
and take new snapshots to see if the performance of a layer
was improved by your change.

如果你看到一個圖層特別耗時，請找到應用程式中產生這個圖層的 Dart 程式碼
並一步調查原因。
你可以對程式碼進行修改、熱重載和產生新的快照，
看看圖層的效能是否因你的修改而得到改善。

![Screenshot of the raster stats tab]({{site.url}}/assets/images/docs/tools/devtools/raster-stats-tab.png)

## Timeline events tab

## 時間線事件表

The timeline events chart shows all event tracing from your application.
The Flutter framework emits timeline events as it works to build frames,
draw scenes, and track other activity such as HTTP request timings
and garbage collection. These events show up here in the Timeline.
You can also send your own Timeline events using the dart:developer
[`Timeline`][] and [`TimelineTask`][] APIs.

時間線事件圖表顯示了應用程式的所有事件追蹤。
Flutter 底層框架在建構幀、繪製場景和
追蹤其他活動（如 HTTP 請求時間和垃圾回收）時，會發出時間線事件。
這些事件會在時間線中顯示出來。
你也可以使用 dart:developer [`Timeline`][] 和 [`TimelineTask`][] API 傳送
你自己的時間線事件。

[`Timeline`]: {{site.api}}/flutter/dart-developer/Timeline-class.html
[`TimelineTask`]: {{site.api}}/flutter/dart-developer/TimelineTask-class.html

![Screenshot of a timeline events tab]({{site.url}}/assets/images/docs/tools/devtools/timeline-events-tab.png)

For help with navigating and using the trace viewer,
click the **?** button at the top right of the timeline
events tab bar. To refresh the timeline with new events from
your application, click the refresh button
(also in the upper right corner of the tab controls).

關於導航和使用追蹤檢視器的幫助，
請點選時間線事件標籤欄右上方的 **?** 按鈕。
要使用應用程式中的新事件，請單擊重新整理按鈕（也位於選項卡的右上角）。

## Advanced debugging tools

## 高階除錯工具

### Enhance tracing 

### 增強的追蹤選項

To view more detailed tracing in the timeline events chart,
use the options in the enhance tracing dropdown:

想要在時間線事件圖表裡檢視更詳細的追蹤內容，請使用增強的追蹤下拉控制項裡的選項：

{{site.alert.note}}

  Frame times might be negatively affected when these options are enabled.

  啟用該選項後，幀建構時間可能會受到影響。

{{site.alert.end}}

![Screenshot of enhanced tracing options]({{site.url}}/assets/images/docs/tools/devtools/enhanced-tracing.png)

To see the new timeline events, reproduce the activity
in your app that you are interested in tracing,
and then select a frame to inspect the timeline.

你可以重複操作你想要追蹤的行為來檢視新的時間線事件，
操作後可以在時間線中選擇一個建構幀進行檢視。

### Track widget builds

### 追蹤 widget 的建構

To see the `build()` method events in the timeline,
enable the **Track Widget Builds** option.
The name of the widget is shown in the timeline event.

想要在時間線中檢視 `build()` 方法的事件，啟用 **Track Widget Builds** 選項，
時間線中將出現 widget 對應名稱的事件。

![Screenshot of track widget builds]({{site.url}}/assets/images/docs/tools/devtools/track-widget-builds.png)

### Track layouts

### 追蹤佈局

To see render object layout events in the timeline,
enable the **Track Layouts** option:

想要在時間線中檢視 `RenderObject` 佈局建構的事件，啟用 Track Layouts 選項：

![Screenshot of track layouts]({{site.url}}/assets/images/docs/tools/devtools/track-layouts.png)

### Track paints

### 追蹤繪製

To see render object paint events in the timeline,
enable the **Track Paints** option:

想要在時間線中檢視 `RenderObject` 的繪製事件，
啟用 **Track Paints** 選項：

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
如果光柵執行緒的時間消耗有顯著降低，
那麼你禁用的效果的濫用可能是導致卡頓的主要原因。

**Render Clip layers**
<br> Disable this option to check whether excessive use of clipping
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

## Other resources

## 其他資源

To learn how to monitor an app's performance and
detect jank using DevTools, check out a guided
[Performance View tutorial][performance-tutorial].

想要學習如何使用 DevTools 監控應用的效能和檢測卡頓，
可以閱讀 [效能檢視課程][performance-tutorial]。

[GPU graph]: {{site.url}}/perf/ui-performance#identifying-problems-in-the-gpu-graph
[Flutter performance profiling]: {{site.url}}/perf/ui-performance
[Reduce shader compilation jank on mobile]: {{site.url}}/perf/shader
[Import and export]: #import-and-export
[performance-tutorial]: {{site.medium}}/@fluttergems/mastering-dart-flutter-devtools-performance-view-part-8-of-8-4ae762f91230
