---
title: Multiple Flutter screens or views
title: 多個 Flutter 頁面或檢視
short-title: Adding multiple Flutters
short-title: 新增多個 Flutter 引擎
description: How to integrate multiple instances of Flutter engine, screens or views to your application.
description: 如何將多個 Flutter 引擎 (engine)、頁面 (screen) 或檢視 (view) 新增到你的應用中（實驗性）。
---

## Scenarios

## 使用場景

If you're integrating Flutter into an existing app, or gradually migrating an
existing app to use Flutter, you may find yourself wanting to add multiple
Flutter instances to the same project. In particular, this can be useful in the
following scenarios:

如果你正在將 Flutter 整合到現有應用，或者正在將原生應用逐漸遷移到使用 Flutter，
你可能會需要在一個工程中新增多個 Flutter 例項，特別是在下述場景下，
多 Flutter 例項可能更為有用：

* An application where the integrated Flutter screen is not a leaf node of
  the navigation graph, and the navigation stack might be a hybrid mixture of
  native -> Flutter -> native -> Flutter.

  集成了 Flutter 介面的應用，其位置並不在路由棧的葉子節點上，
  且其可能是混合路由棧，即 native -> Flutter -> native -> Flutter。

* A screen where multiple partial screen Flutter views might be integrated
  and visible at once.

  多個 Flutter view 同時整合在同一個頁面上，且同時顯示。

The advantage of using multiple Flutter instances is that each
instance is independent and maintains its own internal navigation
stack, UI, and application states. This simplifies the overall application code's
responsibility for state keeping and improves modularity. More details on the
scenarios motivating the usage of multiple Flutters can be found at
[docs.flutter.dev/go/multiple-flutters][].

使用多個 Flutter 例項的優勢在於，每一個例項互相獨立，各自維護路由棧、UI 和應用狀態。
這簡化了應用程式整體的狀態保持考慮，並且進一步模組化。
瞭解更多關於多個 Flutter 使用的動機和場景，請檢視
RFC 文件: [Multiple Flutters](https://files.flutter-io.cn/flutter-design-docs/Multiple_Flutters.pdf)。

Flutter 2 and above are optimized for this scenario, with a low incremental
memory cost (~180kB) for adding additional Flutter instances. This fixed cost
reduction allows the multiple Flutter instance pattern to be used more liberally
in your add-to-app integration.

Flutter 2 以及以上的版本針對多 Flutter 例項進行了最佳化，
額外增加的 Flutter 例項只會增加約 180K 的記憶體佔用，
這種「固定成本」的降低，可以幫助你更輕鬆的將 Flutter 加入到現有應用 (add-to-app)。

## Components

## 元件

The primary API for adding multiple Flutter instances on both Android and iOS
is based on a new `FlutterEngineGroup` class ([Android API][], [iOS API][])
to construct `FlutterEngine`s, rather than the `FlutterEngine`
constructors used previously.

在 Android 和 iOS 上新增多個 Flutter 例項的主要 API
是基於新的 `FlutterEngineGroup` 類 ([Android API][], [iOS API][])
來建立 `FlutterEngine` 的，而不是透過以前的 `FlutterEngine` 構造。

Whereas the `FlutterEngine` API was direct and easier to consume, the
`FlutterEngine` spawned from the same `FlutterEngineGroup` have the performance
advantage of sharing many of the common, reusable resources such as the GPU
context, font metrics, and isolate group snapshot, leading to a faster initial
rendering latency and lower memory footprint.

儘管 `FlutterEngine` API 的用法簡潔明瞭，
但從 `FlutterEngineGroup` 產生的 `FlutterEngine` 具有常用共享資源
（例如 GPU 上下文、字型度量和隔離執行緒的快照）的效能優勢，
從而加快首次渲染的速度、降低延遲並降低記憶體佔用。

* `FlutterEngine`s spawned from `FlutterEngineGroup` can be used to
   connect to UI classes like [`FlutterActivity`][] or [`FlutterViewController`][]
   in the same way as normally constructed cached `FlutterEngine`s.

  由 `FlutterEngineGroup` 產生的 `FlutterEngine` 可以用來關聯 UI 相關的類，
  例如 [`FlutterActivity`][] 或 [`FlutterViewController`][]，
  與通常構造快取的 `FlutterEngine` 類似。

* The first `FlutterEngine` spawned from the `FlutterEngineGroup` doesn't need
  to continue surviving in order for subsequent `FlutterEngine`s to share
  resources as long as there's at least 1 living `FlutterEngine` at all
  times.

  第一個 `FlutterEngineGroup` 產生的 `FlutterEngine` 不需要持續保活，
  只要有 1 個可用的 `FlutterEngine`，就可以隨時在各個 `FlutterEngine` 之間共享資源。

* Creating the very first `FlutterEngine` from a `FlutterEngineGroup` has
  the same [performance characteristics][] as constructing a
  `FlutterEngine` using the constructors did previously.

  透過 `FlutterEngineGroup` 產生的首個 `FlutterEngine` 與使用先前的構造方法構造的
  `FlutterEngine` 有相同的[效能特徵][performance characteristics]。

* When all `FlutterEngine`s from a `FlutterEngineGroup` are destroyed, the next
  `FlutterEngine` created has the same performance characteristics as the very
  first engine.

  當所有由 `FlutterEngineGroup` 構造的 `FlutterEngine` 都被銷燬後，
  下一個建立的 `FlutterEngine` 與首個創造的效能特徵相同。

* The `FlutterEngineGroup` itself doesn't need to live beyond all of the spawned
  engines. Destroying the `FlutterEngineGroup` doesn't affect existing spawned
  `FlutterEngine`s but does remove the ability to spawn additional
  `FlutterEngine`s that share resources with existing spawned engines.

  `FlutterEngineGroup` 本身不需要持續保活。
  將其銷燬後，已產生的 `FlutterEngine` 不受影響，
  但無法繼續在現有共享的基礎上建立新引擎。

## Communication

## 例項之間相互通訊

Communication between Flutter instances is handled using [platform channels][]
(or [Pigeon][]) through the host platform. To see our roadmap on communication,
or other planned work on enhancing multiple Flutter instances, see 
[Issue 72009][].

多個 Flutter 例項之間相互通訊可以透過 [平台通道][platform channels] 或者 [Pigeon][] 進行。
可以在 [Issue 72009][] 裡查閱我們關於多 Flutter 例項通訊和增強功能計劃的路線圖。

## Samples

## 範例

You can find a sample demonstrating how to use `FlutterEngineGroup`
on both Android and iOS on [GitHub][].

您可以在 [GitHub 儲存庫][GitHub] 上找到在 Android 和 iOS 上
使用 `FlutterEngineGroup` 的範例。

{% include docs/app-figure.md image="development/add-to-app/multiple-flutters-sample.gif" alt="A sample demonstrating multiple-Flutters" %}

[GitHub]: {{site.repo.samples}}/tree/main/add_to_app/multiple_flutters
[`FlutterActivity`]: {{site.api}}/javadoc/io/flutter/embedding/android/FlutterActivity.html
[`FlutterViewController`]: {{site.api}}/objcdoc/Classes/FlutterViewController.html
[performance characteristics]: {{site.url}}/development/add-to-app/performance
[docs.flutter.dev/go/multiple-flutters]: {{site.url}}/go/multiple-flutters
[Issue 72009]: {{site.repo.flutter}}/issues/72009
[Pigeon]: {{site.pub}}/packages/pigeon
[platform channels]: {{site.url}}/development/platform-integration/platform-channels
[Android API]: https://cs.opensource.google/flutter/engine/+/master:shell/platform/android/io/flutter/embedding/engine/FlutterEngineGroup.java
[iOS API]: https://cs.opensource.google/flutter/engine/+/master:shell/platform/darwin/ios/framework/Headers/FlutterEngineGroup.h
