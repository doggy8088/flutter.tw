---
title: Flutter performance profiling
title: Flutter 效能分析
subtitle: Where to look when your Flutter app drops frames in the UI.
subtitle: 找出你的 Flutter 應用 UI 在哪裡掉幀了
description: Diagnosing UI performance issues in Flutter.
description: 診斷 Flutter 裡的 UI 效能問題。
tags: Flutter效能
keywords: 效能分析,效能除錯工具,開發者工具,60fps,120fps,profile mode
---

{% include docs/performance.md %}

{{site.alert.secondary}}

  <h4 class="no_toc">What you'll learn</h4>

  <h4 class="no_toc">你將學到</h4>

  * Flutter aims to provide 60 frames per second (fps) performance,
    or 120 fps performance on devices capable of 120Hz updates.
    
    Flutter 的目標是提供 60 幀每秒 (fps) 的效能，
    或者是在可以達到 120 Hz 的裝置上提供 120 fps 的效能。
    
  * For 60fps, frames need to render approximately every 16ms.
  
    對於 60 fps 來說，需要在約每 16 ms 的時候渲染一幀。
    
  * Jank occurs when the UI doesn't render smoothly. For example,
    every so often, a frame takes 10 times longer to render,
    so it gets dropped, and the animation visibly jerks.
    
    當 UI 渲染不流暢的時候，卡頓就隨之產生了。
    舉例來說，如果一幀花了 10 倍的時間來渲染，這幀就會被丟棄，動畫看起來就會卡。
    
{{site.alert.end}}

It's been said that "a _fast_ app is great,
but a _smooth_ app is even better."
If your app isn't rendering smoothly,
how do you fix it? Where do you begin?
This guide shows you where to start,
steps to take, and tools that can help.

有句話叫「**快**的應用固然很好，但**流暢**的應用則更好。」
如果你的應用渲染並不流暢，該怎麼處理呢？
從哪裡著手呢？本文展示了應該從哪裡著手，步驟以及可以提供幫助的工具。

{{site.alert.note}}
  * An app's performance is determined by more than one measure.
    Performance sometimes refers to raw speed, but also to the UI's
    smoothness and lack of stutter. Other examples of performance
    include I/O or network speed. This page primarily focuses on the
    second type of performance (UI smoothness), but you can use most
    of the same tools to diagnose other performance problems.

    應用的效能不只是由一次測量 (measure) 決定的。效能有時取決於原生速度，
    同時也取決於 UI 的流暢性，不卡頓。其他效能指標還包括 I/O 或者網速。
    本文主要聚焦於第二種效能（UI 流暢性），
    但其中的大多數工具也能被用來分析其他效能問題。

  * To perform tracing inside your Dart code, see [Tracing Dart code][]
    in the [Debugging][] page.

    分析 Dart 程式碼中的效能問題，
    可以參考 [除錯 Flutter 應用][Debugging] 頁下的
    [追蹤 Dart 程式碼效能][Tracing Dart code]。

{{site.alert.end}}

[Debugging]: {{site.url}}/testing/debugging
[Tracing Dart code]: {{site.url}}/testing/debugging#tracing-dart-code

## Diagnosing performance problems

## 分析效能問題

To diagnose an app with performance problems, you'll enable
the performance overlay to look at the UI and raster threads.
(The raster thread was previously known as the GPU thread.)
Before you begin, you want to make sure that you're running in
[profile mode][], and that you're not using an emulator.
For best results, you might choose the slowest device that
your users might use.

分析應用的效能問題需要開啟效能監控圖層 (performance overlay) 
來觀察 UI 和 GPU 執行緒。在此之前，要確保是在 [分析模式][profile mode] 下執行，
而且當前裝置不是虛擬機器。使用使用者可能採用的最慢裝置來獲取最佳結果。

[profile mode]: {{site.url}}/testing/build-modes#profile

### Connect to a physical device

### 連線到物理裝置

Almost all performance debugging for Flutter applications
should be conducted on a physical Android or iOS device,
with your Flutter application running in [profile mode][].
Using debug mode, or running apps on simulators
or emulators, is generally not indicative of the final
behavior of release mode builds.
_You should consider checking performance
on the slowest device that your users might reasonably use._

幾乎全部的 Flutter 應用效能除錯都應該在真實的 Android 或者
iOS 裝置上以 [分析模式][profile mode] 進行。
通常來說，除錯模式或者是模擬器上執行的應用的效能指標和釋出模式的表現並不相同。
**應該考慮在使用者使用的最慢的裝置上檢查效能。**

{{site.alert.secondary}}

  <h4 class="no_toc" markdown="1">**Why you should run on a real device:**</h4>
  
  <h4 class="no_toc" markdown="1">**為什麼應該在真機上執行：**</h4>

* Simulators and emulators don't use the same hardware, so their
  performance characteristics are different&mdash;some operations are
  faster on simulators than real devices, and some are slower.

  各種模擬器使用的硬體並不相同，因此效能也不同&mdash;模擬器上的
  一些操作會比真機快，而另一些操作則會比真機慢。

* Debug mode enables additional checks (such as asserts) that don't run
  in profile or release builds, and these checks can be expensive.
  
  除錯模式相比分析模式或者釋出編譯來說，
  增加了額外的檢查（例如斷言），這些檢查可能相當耗費資源。
  
* Debug mode also executes code in a different way than release mode.
  The debug build compiles the Dart code "just in time" (JIT) as the
  app runs, but profile and release builds are pre-compiled to native
  instructions (also called "ahead of time", or AOT) before the app is
  loaded onto the device. JIT can cause the app to pause for JIT
  compilation, which itself can cause jank.
  
  除錯模式和釋出模式程式碼執行的方式也是不同的。
  除錯編譯採用的是“just in time” (JIT) 模式執行應用，
  而分析和釋出模式則是預編譯到本地指令（“ahead of time”，或者叫 AOT）
  之後再載入到裝置中。JIT本身的編譯就可能導致應用暫停，從而導致卡頓。

{{site.alert.end}}

### Run in profile mode

### 在分析模式執行

Flutter's profile mode compiles and launches your application
almost identically to release mode, but with just enough additional
functionality to allow debugging performance problems.
For example, profile mode provides tracing information to the
profiling tools.

除了一些除錯效能問題所必須的額外方法，
Flutter 的分析模式和釋出模式的編譯和執行基本相同。
例如，分析模式為分析工具提供了追蹤資訊。

{{site.alert.note}}

  DevTools can't connect to a Flutter web app running
  in profile mode. Use Chrome DevTools to
  [generate timeline events][] for a web app.

  DevTools 無法連線到以效能模式執行的 Flutter 應用。
  你需要使用 Chrome 的 DevTools 來為 Flutter Web 應用
  [產生時間線事件][generate timeline events]。

{{site.alert.end}}

[generate timeline events]: {{site.developers}}/web/tools/chrome-devtools/evaluate-performance/performance-reference


Launch the app in profile mode as follows:

使用分析模式執行應用的方法：

* In VS Code, open your `launch.json` file, and set the
  `flutterMode` property to `profile`
  (when done profiling, change it back to `release` or `debug`):

  在 VS Code 中，開啟 `launch.json` 檔案，
  設定 `flutterMode` 屬性為 `profile`（當分析完成後，改回 `release` 或者 `debug`）：

  ```json
  "configurations": [
    {
      "name": "Flutter",
      "request": "launch",
      "type": "dart",
      "flutterMode": "profile"
    }
  ]
  ```
* In Android Studio and IntelliJ, use the
  **Run > Flutter Run main.dart in Profile Mode** menu item.

  在 Android Studio 和 IntelliJ 使用
  **Run > Flutter Run main.dart in Profile Mode** 選項。

* From the command line, use the `--profile` flag:

  命令列使用 `--profile` 引數執行：

  ```terminal
  $ flutter run --profile
  ```

For more information on the different modes,
see [Flutter's build modes][].

關於不同模式的更多資訊，請參考文件：
[Flutter 的建構模式選擇][Flutter's build modes]。

You'll begin by opening DevTools and viewing
the performance overlay, as discussed in the next section.

下面我們會從開啟 DevTools、檢視效能圖層開始講述。

[Flutter's build modes]: {{site.url}}/testing/build-modes

## Launch DevTools

## 執行 DevTools

DevTools provides features like profiling, examining the heap,
displaying code coverage, enabling the performance overlay,
and a step-by-step debugger.
DevTools' [Timeline view][] allows you to investigate the
UI performance of your application on a frame-by-frame basis.

Dart DevTool 提供諸如效能分析、堆測試以及顯示程式碼覆蓋率等功能。
DevTool 的 [Timeline] 介面可以讓開發者逐幀分析應用的 UI 效能。

Once your app is running in profile mode,
[launch DevTools][].

一旦你的應用程式在分析模式下執行，
即 [執行 DevTools][launch DevTools]。

[launch DevTools]: {{site.url}}/tools/devtools
[Timeline view]: {{site.url}}/tools/devtools/performance

## The performance overlay

## 效能圖層

The performance overlay displays statistics in two graphs
that show where time is being spent in your app. If the UI
is janky (skipping frames), these graphs help you figure out why.
The graphs display on top of your running app, but they aren't
drawn like a normal widget&mdash;the Flutter engine itself
paints the overlay and only minimally impacts performance.
Each graph represents the last 300 frames for that thread.

效能圖層用兩張圖表顯示應用的耗時資訊。如果 UI 產生了卡頓（跳幀），
這些圖表可以幫助分析原因。圖表在當前應用的最上層展示，
但並不是用普通的 widget 方式繪製的&mdash;Flutter 
引擎自身繪製了該圖層來儘可能減少對效能的影響。
每一張圖表都代表當前執行緒的最近 300 幀表現。

This section describes how to enable the performance overlay
and use it to diagnose the cause of jank in your application.
The following screenshot shows the performance overlay running
on the Flutter Gallery example:

本節闡述如何開啟效能圖層並用其來分析應用中卡頓的原因。
下面的截圖展示了 Flutter Gallery 範例的效能圖層：

![Screenshot of overlay showing zero jank]({{site.url}}/assets/images/docs/tools/devtools/performance-overlay-green.png)

<br>Performance overlay showing the raster thread (top),
and UI thread (bottom).<br>The vertical green bars
represent the current frame.

<br>raster 執行緒的效能情況在上面，UI 執行緒顯示在下面。
<br>垂直的綠色條條代表的是當前幀。

## Interpreting the graphs

## 圖表解釋

The top graph (marked "GPU") shows the time spent by 
the raster thread, the bottom one graph shows the time 
spent by the UI thread.
The white lines across the graphs show 16ms increments
along the vertical axis; if the graph ever goes over one
of these lines then you are running at less than 60Hz.
The horizontal axis represents frames. The graph is
only updated when your application paints,
so if it's idle the graph stops moving.

最頂部(標誌了 “GPU”）的圖形表示 raster 執行緒所花費的時間，
底部的圖表顯示了 UI 執行緒所花費的時間。
橫跨圖表中的白線代表了 16 ms 內沿豎軸的增量；
如果這些線在圖表中都沒有超過它的話，
說明你的執行幀率低於 60 Hz。而橫軸則表示幀。
只有當你的應用繪製時這個圖表才會更新，所以如果它空閒的話，圖表就不會動。

The overlay should always be viewed in [profile mode][],
since [debug mode][] performance is intentionally sacrificed
in exchange for expensive asserts that are intended to aid
development, and thus the results are misleading.

這個浮層只應在 [分析模式][profile mode] 中使用，因為在 
[除錯模式][debug mode] 下有意犧牲了效能來換取昂貴的斷言以幫助開發，
所以這時候的結果會有誤導性。

Each frame should be created and displayed within 1/60th of
a second (approximately 16ms). A frame exceeding this limit
(in either graph) fails to display, resulting in jank,
and a vertical red bar appears in one or both of the graphs.
If a red bar appears in the UI graph, the Dart code is too
expensive. If a red vertical bar appears in the GPU graph,
the scene is too complicated to render quickly.

每一幀都應該在 1/60 秒（大約 16 ms）內建立並顯示。
如果有一幀超時（任意圖像）而無法顯示，就導致了卡頓，
圖表之一就會展示出來一個紅色豎條。
如果是在 UI 圖表出現了紅色豎條，則表明 Dart 程式碼消耗了大量資源。
而如果紅色豎條是在 GPU 圖表出現的，
意味著場景太複雜導致無法快速渲染。

![Screenshot of performance overlay showing jank with red bars]({{site.url}}/assets/images/docs/tools/devtools/performance-overlay-jank.png)

<br>The vertical red bars indicate that the current frame is
expensive to both render and paint.<br>When both graphs
display red, start by diagnosing the UI thread.

<br>紅色豎條表明當前幀的渲染和繪製都很耗時。<br>
當兩張圖表都是紅色時，就要開始對 UI 執行緒 (Dart VM) 進行診斷了。

[debug mode]: {{site.url}}/testing/build-modes#debug

## Flutter's threads

## Flutter 的執行緒

Flutter uses several threads to do its work, though
only two of the threads are shown in the overlay.
All of your Dart code runs on the UI thread.
Although you have no direct access to any other thread,
your actions on the UI thread have performance consequences
on other threads.

Flutter 使用多個執行緒來完成其必要的工作，圖層中僅展示了其中兩個執行緒。 
您寫的所有 Dart 程式碼都在 UI 執行緒上執行。儘管您沒有直接存取其他執行緒的許可權，
但是您對 UI 執行緒的操作會對其他執行緒產生效能影響。

<dl markdown="1">
<dt>
<p markdown="1">**Platform thread**</p>
<p markdown="1">**平台執行緒**</p>
</dt>
<dd markdown="1">
<p markdown="1">The platform's main thread. Plugin code runs here.
    For more information, see the [UIKit][] documentation for iOS,
    or the [MainThread][] documentation for Android.
    This thread is not shown in the performance overlay.</p>
<p markdown="1">平台執行緒實際上就是主執行緒。Plugin 的程式碼將會在這裡執行。
    想要了解更多資訊，請參閱 Android 的 [MainThread][] 
    以及 iOS 的 [UIKit][] 文件。</p>
</dd>
<dt>
<p markdown="1">**UI thread**</p>
<p markdown="1">**UI 執行緒**</p>
</dt>
<dd markdown="1">
<p markdown="1">The UI thread executes Dart code in the Dart VM.
    This thread includes code that you wrote, and code executed by
    Flutter's framework on your app's behalf.
    When your app creates and displays a scene, the UI thread creates
    a _layer tree_, a lightweight object containing device-agnostic
    painting commands, and sends the layer tree to the raster thread to
    be rendered on the device. _Don't block this thread!_
    Shown in the bottom row of the performance overlay.</p>
<p markdown="1">UI 執行緒在 Dart VM 中執行 Dart 程式碼。
    該執行緒包括開發者寫下的程式碼和 Flutter 框架根據應用行為產生的程式碼。
    當應用建立和展示場景的時候，UI 執行緒首先建立一個 **圖層樹（layer tree）** ，
    一個包含裝置無關的渲染命令的輕量物件，
    並將圖層樹傳送到 GPU 執行緒來渲染到裝置上。
    **不要阻塞這個執行緒！** 在效能圖層的最低欄展示該執行緒。</p>
</dd>
<dt>
<p markdown="1">**Raster thread** (previously known as the GPU thread)</p>
<p markdown="1">**Raster 執行緒**（以前叫 GPU 執行緒）</p>
</dt>
<dd markdown="1">
<p markdown="1">The raster thread takes the layer tree and displays
    it by talking to the GPU (graphic processing unit).
    You cannot directly access the raster thread or its data but,
    if this thread is slow, it's a result of something you've done
    in the Dart code. Skia and Impeller, the graphics libraries,
    run on this thread.
    Shown in the top row of the performance overlay.
    This thread was previously known as the "GPU thread" because it
    rasterizes for the GPU. But it is running on the CPU.
    We renamed it to "raster thread" because many developers wrongly
    (but understandably)
    assumed the thread runs on the GPU unit.
<p markdown="1">raster 執行緒拿到 layer tree，並將它交給 GPU（圖形處理單元）。
    你無法直接與 GPU 執行緒或其資料通訊，
    但如果該執行緒變慢，一定是開發者 Dart 程式碼中的某處導致的。
    圖形庫 Skia 在該執行緒執行，並在效能圖層的最頂欄顯示該執行緒。
    這個執行緒之前被叫做「GPU 執行緒」，因為它為 GPU 進行網格化，
    但我們重新將它命名為「raster 執行緒」，
    這是因為許多開發者錯誤的（但是能理解）認為該執行緒執行在 GPU 單元。</p>
</dd>
<dt>
<p markdown="1">**I/O thread**</p>
<p markdown="1">**I/O執行緒**</p>
</dt>
<dd markdown="1">
<p markdown="1">Performs expensive tasks (mostly I/O) that would
    otherwise block either the UI or raster threads.
    This thread is not shown in the performance overlay.</p>
<p markdown="1">執行昂貴的操作（常見的有 I/O）以避免阻塞 UI 或者 raster 執行緒。
    這個執行緒將不會顯示在 performance overlay 上。</p>
</dd>
</dl>
    
For links to more information and videos,
see [The Framework architecture][] on the
[GitHub wiki][], and the community article,
[The Layer Cake][].

你可以在 [GitHub wiki][] 上的框架結構 ([The Framework architecture][]) 一文中
瞭解更多資訊和一些影片內容，
另外你可以在我們的社群中檢視文章 [The Layer Cake][]。

[GitHub wiki]: {{site.repo.flutter}}/wiki/
[MainThread]: {{site.android-dev}}/reference/android/support/annotation/MainThread
[The Framework architecture]: {{site.repo.flutter}}/wiki/The-Framework-architecture
[The Layer Cake]: {{site.medium}}/flutter-community/the-layer-cake-widgets-elements-renderobjects-7644c3142401
[UIKit]: {{site.apple-dev}}/documentation/uikit

### Displaying the performance overlay

### 顯示效能圖層

You can toggle display of the performance overlay as follows:

你可以用如下方法顯示效能圖層：

* Using the Flutter inspector

  使用 Flutter Inspector
  
* From the command line

  從命令列啟動
   
* Programmatically

  寫入程式碼

#### Using the Flutter inspector

#### 使用 Flutter inspector

The easiest way to enable the PerformanceOverlay widget is
from the Flutter inspector, which is available in the
[Inspector view][] in [DevTools][]. Simply click the
**Performance Overlay** button to toggle the overlay
on your running app.

開啟 PerformanceOverlay widget 最簡單的方法是 IDE 中 
Flutter 外掛提供的 Flutter inspector，
你可以在 [開發者工具][DevTools] 的 
[使用 Flutter inspector 工具][Inspector view] 中找到。
只需單擊 **Performance Overlay** 按鈕，
即可在正在執行的應用程式上切換圖層。

[Inspector view]: {{site.url}}/tools/devtools/inspector

#### From the command line

#### 命令列

Toggle the performance overlay using the **P** key from
the command line.

使用 **P** 引數觸發效能圖層。

#### Programmatically

#### 程式碼控制

To enable the overlay programmatically, see
[Performance overlay][], a section in the
[Debugging Flutter apps programmatically][] page.

若要以程式設計的方式啟用效能圖層，請參考
[以程式設計方式除錯應用][Debugging Flutter apps programmatically] 文件的
[效能圖層][Performance overlay] 章節。

[Debugging Flutter apps programmatically]: {{site.url}}/testing/code-debugging
[Performance overlay]: {{site.url}}/testing/code-debugging#performance-overlay

## Identifying problems in the UI graph

## 定位 UI 圖表中的問題

If the performance overlay shows red in the UI graph,
start by profiling the Dart VM, even if the GPU graph
also shows red.

如果效能圖層的 UI 圖表顯示紅色，
就要從分析 Dart VM 開始著手了，
即使 GPU 圖表同樣顯示紅色。

## Identifying problems in the GPU graph

## 定位 GPU 圖表中的問題

Sometimes a scene results in a layer tree that is easy to construct,
but expensive to render on the raster thread. When this happens,
the UI graph has no red, but the GPU graph shows red.
In this case, you'll need to figure out what your code is doing
that is causing rendering code to be slow. Specific kinds of workloads
are more difficult for the GPU. They might involve unnecessary calls
to [`saveLayer`][], intersecting opacities with multiple objects,
and clips or shadows in specific situations.

有些情況下介面的圖層樹構造起來雖然容易，
但在 raster 執行緒下渲染卻很耗時。
這種情況發生時，UI 圖表沒有紅色，但 GPU 圖表會顯示紅色。
這時需要找出程式碼中導致渲染緩慢的原因。
特定型別的負載對 GPU 來說會更加複雜。
可能包括不必要的對 [`saveLayer`][] 的呼叫，
許多物件間的複雜操作，還可能是特定情形下的裁剪或者陰影。

If you suspect that the source of the slowness is during an animation,
click the **Slow Animations** button in the Flutter inspector
to slow animations down by 5x.
If you want more control on the speed, you can also do this
[programmatically][].

如果推斷的原因是動畫中的卡頓的話，
可以點選 Flutter inspector 中的 **Slow Animations** 按鈕，來使動畫速度減慢 5 倍。
如果你想從更多方面控制動畫速度，你可以參考 [programmatically][]。

Is the slowness on the first frame, or on the whole animation?
If it's the whole animation, is clipping causing the slow down?
Maybe there's an alternative way of drawing the scene that doesn't
use clipping. For example, overlay opaque corners onto a square
instead of clipping to a rounded rectangle.
If it's a static scene that's being faded, rotated, or otherwise
manipulated, a [`RepaintBoundary`][] might help.

卡頓是第一幀發生的還是貫穿整個動畫過程呢？
如果是整個動畫過程的話，會是裁剪導致的嗎？
也許有可以替代裁剪的方法來繪製場景。
比如說，不透明圖層的長方形中用尖角來取代圓角裁剪。
如果是一個靜態場景的淡入、旋轉或者其他操作，
可以嘗試使用重繪邊界 ([`RepaintBoundary`][])。

[programmatically]: {{site.url}}/testing/code-debugging#debugging-animations
[`RepaintBoundary`]: {{site.api}}/flutter/widgets/RepaintBoundary-class.html
[`saveLayer`]: {{site.api}}/flutter/dart-ui/Canvas/saveLayer.html

#### Checking for offscreen layers

#### 檢查螢幕之外的檢視

The [`saveLayer`][] method is one of the most expensive methods in
the Flutter framework. It's useful when applying post-processing
to the scene, but it can slow your app and should be avoided if
you don't need it.  Even if you don't call `saveLayer` explicitly,
implicit calls might happen on your behalf. You can check whether
your scene is using `saveLayer` with the
[`PerformanceOverlayLayer.checkerboardOffscreenLayers`][] switch.

儲存圖層 ([`saveLayer`][]) 方法是 Flutter 框架中最重量的操作之一。
更新螢幕時這個方法很有用，但它可能使應用變慢，
如果不是必須的話，應該避免使用這個方法。
即便沒有明確地呼叫 `saveLayer`，也可能在其他操作中間接呼叫了該方法。
可以使用棋盤畫面以外的層
([`PerformanceOverlayLayer.checkerboardOffscreenLayers`][]) 
開關來檢查場景是否使用了 `saveLayer`。

{% comment %}
[TODO: Document disabling the graphs and checkerboardRasterCacheImages.
Flutter inspector doesn't seem to support this?]
{% endcomment %}

Once the switch is enabled, run the app and look for any images
that are outlined with a flickering box. The box flickers from
frame to frame if a new frame is being rendered.  For example,
perhaps you have a group of objects with opacities that are rendered
using `saveLayer`. In this case, it's probably more performant to
apply an opacity to each individual widget, rather than a parent
widget higher up in the widget tree. The same goes for
other potentially expensive operations, such as clipping or shadows.

開啟開關之後，執行應用並檢查是否有圖像的輪廓閃爍。
如果有新的幀渲染的話，容器就會閃爍。
舉個例子，也許有一組物件的透明度要使用 `saveLayer` 來渲染。
在這種情況下，相比透過 widget 樹中高層次的父 widget 操作，
單獨對每個 widget 來應用透明度可能效能會更好。
其他可能大量消耗資源的操作也同理，比如裁剪或者陰影。

{{site.alert.note}}

  Opacity, clipping, and shadows are not, in themselves,
  a bad idea. However, applying them to the top of the
  widget tree might cause extra calls to `saveLayer`,
  and needless processing.
  
  透明度、裁剪以及陰影它們本身並不是個糟糕的主意。
  然而對 widget 樹最上層 widget 的操作可能導致額外對
  `saveLayer` 的呼叫以及無用的處理。
  
{{site.alert.end}}

When you encounter calls to `saveLayer`,
ask yourself these questions:

當遇到對 `saveLayer` 的呼叫時，先問問自己：

* Does the app need this effect?

  應用是否需要這個效果？
  
* Can any of these calls be eliminated?

  可以減少呼叫麼？
  
* Can I apply the same effect to an individual element instead of a group?

  可以對單獨元素操作而不是一組元素麼？

[`PerformanceOverlayLayer.checkerboardOffscreenLayers`]: {{site.api}}/flutter/rendering/PerformanceOverlayLayer/checkerboardOffscreenLayers.html

#### Checking for non-cached images

#### 檢查沒有快取的圖像

Caching an image with [`RepaintBoundary`][] is good,
_when it makes sense_.

使用重繪邊界 ([`RepaintBoundary`][]) 來快取圖片是個好主意，**當需要的時候。**

One of the most expensive operations,
from a resource perspective,
is rendering a texture using an image file.
First, the compressed image
is fetched from persistent storage.
The image is decompressed into host memory (GPU memory),
and transferred to device memory (RAM).

從資源的角度看，最重量級的操作之一是用圖像檔案來渲染紋理。
首先，需要從持久儲存中取出壓縮圖像，
然後解壓縮到宿主儲存中（GPU 儲存），再傳輸到裝置儲存器中　(RAM) 。

In other words, image I/O can be expensive.
The cache provides snapshots of complex hierarchies so
they are easier to render in subsequent frames.
_Because raster cache entries are expensive to
construct and take up loads of GPU memory,
cache images only where absolutely necessary._

也就是說，圖像的 I/O 操作是重量級的。
快取提供了複雜層次的快照，這樣就可以方便地渲染到隨後的幀中。
**因為光柵快取入口的建構需要大量資源，同時增加了 GPU 儲存的負載，所以只在必須時才快取圖片。**

You can see which images are being cached by enabling the
[`PerformanceOverlayLayer.checkerboardRasterCacheImages`][] switch.

開啟覆蓋層效能 棋盤格光柵快取圖像
([`PerformanceOverlayLayer.checkerboardRasterCacheImages`][]) 開關可以檢查哪些圖片被快取了。

{% comment %}
[TODO: Document how to do this, either via UI or programmatically.
At this point, disable the graphs and checkerboardOffScreenLayers.]
{% endcomment %}

Run the app and look for images rendered with a randomly colored
checkerboard, indicating that the image is cached.
As you interact with the scene, the checkerboarded images
should remain constant&mdash;you don't want to see flickering,
which would indicate that the cached image is being re-cached.

執行應用來檢視使用隨機顏色網格渲染的圖像，標識被快取的圖像。
當和場景互動時，
網格里的圖片應該是靜止的&mdash;代表重新快取圖片的閃爍檢視不應該出現。

In most cases, you want to see checkerboards on static images,
but not on non-static images.  If a static image isn't cached,
you can cache it by placing it into a [`RepaintBoundary`][]
widget. Though the engine might still ignore a repaint
boundary if it thinks the image isn't complex enough.

大多數情況下，開發者都希望在網格里看到的是靜態圖片，而不是非靜態圖片。
如果靜態圖片沒有被快取，可以將其放到重繪邊界
([`RepaintBoundary`][]) widget 中來快取。
雖然引擎也可能忽略 repaint boundary，如果它認為圖像還不夠複雜的話。

[`PerformanceOverlayLayer.checkerboardRasterCacheImages`]: {{site.api}}/flutter/rendering/PerformanceOverlayLayer/checkerboardRasterCacheImages.html

### Viewing the widget rebuild profiler

### 檢視 widget 重建效能

The Flutter framework is designed to make it hard to create
applications that are not 60fps and smooth. Often, if you have jank,
it's because there is a simple bug causing more of the UI to be
rebuilt each frame than required. The Widget rebuild profiler
helps you debug and fix performance problems due to these sorts
of bugs.

Flutter 框架的設計使得建構達不到 60 fps 流暢度的應用變得困難。
通常情況下如果卡頓，就是因為每一幀被重建的 UI 比需求更多的簡單 bug。
Widget rebuild profiler 可以幫助除錯和修復這些問題引起的 bug。

You can view the widget rebuilt counts for the current screen and
frame in the Flutter plugin for Android Studio and IntelliJ.
For details on how to do this, see [Show performance data][]

可以檢視 widget inspector 中當前螢幕和幀下的 widget 重建數量。
瞭解細節，可以參考 [顯示效能資料][Show performance data]。

[Show performance data]: {{site.url}}/tools/android-studio#show-performance-data

## Benchmarking

## 評分

You can measure and track your app's performance by writing
benchmark tests. The Flutter Driver library provides support
for benchmarking. Using this integration test framework,
you can generate metrics to track the following:

可以透過編寫評分測試來測量和追蹤應用的效能。
Flutter Driver 庫提供了對評分的支援。
基於這套測試框架就可以產生以下幾項的測試標準：

* Jank

  卡頓
  
* Download size
  
  下載大小
  
* Battery efficiency
  
  電池效能
  
* Startup time

  啟動時間

Tracking these benchmarks allows you to be informed when a
regression is introduced that adversely affects performance.

追蹤這些評分可以在迴歸測試中瞭解對效能的不利影響。

For more information, check out [Integration testing][].

瞭解更多，請參考 [測試 Flutter 應用][Integration testing]。

[Integration testing]: {{site.url}}/testing/integration-tests

## Other resources

## 更多資源

The following resources provide more information on using
Flutter's tools and debugging in Flutter:

以下連結提供了關於 Flutter 工具的使用和 Flutter 除錯的更多資訊：

* [Debugging][]

  [除錯 Flutter 應用][Debugging]
  
* [Flutter inspector][]

* [Flutter inspector talk][], presented at DartConf 2018
  
  [Flutter Inspector talk][], 一個在 DartConf 2018 大會的演講
  
* [Why Flutter Uses Dart][], an article on Hackernoon

  Hackernoon 專欄的一篇文章 [為什麼 Flutter 使用 Dart][Why Flutter Uses Dart]
  
* [Why Flutter uses Dart][video], a video on the Flutter channel
 
  Flutter YouTube 頻道的一個影片：[為什麼 Flutter 使用 Dart 的][video]

* [DevTools][devtools]: performance tooling for Dart and Flutter apps

  [Dart 開發者工具][devtools]: Dart 和 Flutter 應用的開發者效能除錯工具；
  
* [Flutter API][] docs, particularly the [`PerformanceOverlay`][] class,
  and the [dart:developer][] package

  [Flutter API][] 文件, 特別是 [`PerformanceOverlay`][] 這個類
  和 [dart:developer][] 這個 package。

[dart:developer]: {{site.api}}/flutter/dart-developer/dart-developer-library.html
[devtools]: {{site.url}}/tools/devtools
[Flutter API]: {{site.api}}
[Flutter inspector]: {{site.url}}/tools/devtools/inspector
[Flutter inspector talk]: {{site.youtube-site}}/watch?v=JIcmJNT9DNI
[`PerformanceOverlay`]: {{site.api}}/flutter/widgets/PerformanceOverlay-class.html
[video]: https://www.bilibili.com/video/BV1t54y1m7Qr/
[Why Flutter Uses Dart]: https://hackernoon.com/why-flutter-uses-dart-dd635a054ebf
