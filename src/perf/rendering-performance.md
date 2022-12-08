---
title: Improving rendering performance
title: 提高渲染效能
description: How to measure and evaluate your app's rendering performance.
description: 如何測量以及評估你的應用渲染效能。
tags: Flutter效能
keywords: 效能測量,建議,常見問題,效能除錯
---

{% include docs/performance.md %}

Rendering animations in your app is one of the most cited
topics of interest when it comes to measuring performance.
Thanks in part to Flutter's Skia engine and its ability
to quickly create and dispose of widgets,
Flutter applications are performant by default,
so you only need to avoid common pitfalls to achieve
excellent performance.

在衡量效能時，應用程式中的渲染動畫一直是最受關注的話題之一。
由於 Flutter 自帶的 Skia 引擎以及它能夠快速建立和處理元件的能力，
Flutter 應用在預設情況下就能保證擁有良好的效能，
因此我們只需避開常見的陷阱就可以獲得出色的效能。

## General advice

## 一些基本的建議

If you see janky (non smooth) animations, make
**sure** that you are profiling performance with an
app built in _profile_ mode.
The default Flutter build creates an app in _debug_ mode,
which is not indicative of release performance.
For information,
see [Flutter's build modes][].

如果看到不穩定（不流暢）的動畫，
請 **確保** 你正在做效能分析的應用是在 **profile** 模式下建構的，
因為預設情況下 Flutter 會在 debug 模式下建立應用，這並不表示應用正式釋出後的效能。
更多資訊，參見 [Flutter 的建構模式][Flutter's build modes]。

A couple common pitfalls:

有幾種常見的陷阱：

* Rebuilding far more of the UI than expected each frame.
  To track widget rebuilds, see [Show performance data][].
  
  每幀重建的 UI 比預期的要多得多。要追蹤元件的重建，
  請參閱 [顯示效能資料][Show performance data]。
  
* Building a large list of children directly, rather than
  using a ListView.
  
  直接建構大量的子元件列表，而不使用 ListView。

For more information on evaluating performance
including information on common pitfalls,
see the following docs:

有關評估效能的更多資料（包括常見缺陷），請參閱以下文件：

* [Performance best practices][]

  [Flutter 應用效能最佳化最佳實踐][Performance best practices]

* [Flutter performance profiling][]

  [Flutter 效能分析][Flutter performance profiling]

## Mobile-only advice

## 純移動應用

Do you see noticeable jank on your mobile app, but only on
the first run of an animation? If so, see
[Reduce shader animation jank on mobile][].

如果移動應用裡遇到一些肉眼可見的卡頓，單只是在第一次執行動畫的時候？
如果是這樣的話，可以檢視這個文件
[減少過移動應用的著色器動畫卡頓][Reduce shader animation jank on mobile]。

[Reduce shader animation jank on mobile]: {{site.url}}/perf/shader

## Web-only advice

## 純 Web 應用

The following series of articles cover what the Flutter Material
team learned when improving performance of the Flutter Gallery
app on the web:

下面的內容是 Flutter Material 團隊在提高
Flutter Gallery Web 應用效能時候總結的經驗：

* [Optimizing performance in Flutter web apps with tree shaking and deferred loading][shaking]

  [透過 tree shaking 和延遲載入來最佳化 Flutter Web 應用的效能 (Optimizing performance in Flutter web apps with 
  tree shaking and deferred loading)][shaking]

* [Improving perceived performance with image placeholders, precaching, and disabled navigation transitions][images]

  [透過使用影象佔位符、預快取和禁用導航效果來提高效能(Improving perceived performance with image placeholders, precaching, and disabled navigation transitions)][images]
  
* [Building performant Flutter widgets][]

  [高效建構 Flutter widgets (Building performant Flutter widgets)][Building performant Flutter widgets]

[Building performant Flutter widgets]: {{site.flutter-medium}}/building-performant-flutter-widgets-3b2558aa08fa
[Flutter's build modes]: {{site.url}}/testing/build-modes
[Flutter performance profiling]: {{site.url}}/perf/ui-performance
[images]: {{site.flutter-medium}}/improving-perceived-performance-with-image-placeholders-precaching-and-disabled-navigation-6b3601087a2b
[Performance best practices]: {{site.url}}/perf/best-practices
[shaking]: {{site.flutter-medium}}/optimizing-performance-in-flutter-web-apps-with-tree-shaking-and-deferred-loading-535fbe3cd674
[Show performance data]: {{site.url}}/development/tools/android-studio#show-performance-data
