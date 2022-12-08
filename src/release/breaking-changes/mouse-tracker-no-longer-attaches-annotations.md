---
title: MouseTracker no longer attaches annotations
title: MouseTracker 不再附加註釋
description: MouseTracker no longer relies on annotation attachment to perform the mounted-exit check; therefore, all three related methods are removed.
description: MouseTracker 不再依賴註釋附件來執行安裝退出檢查，因此刪除了三個相關方法。
---

## Summary

## 概述

Removed `MouseTracker`'s methods `attachAnnotation`,
`detachAnnotation`, and `isAnnotationAttached`.

`MouseTracker` 的方法 `attachAnnotation`、`detachAnnotation` 和 `isAnnotationAttached` 已被移除。

## Context

## 上下文

Mouse events, such as when a mouse pointer has entered a region,
exited, or is hovering over a region, are detected with the help of
`MouseTrackerAnnotation`s that are placed on interested regions
during the render phase. Upon each update (a new frame or a new event),
`MouseTracker` compares the annotations hovered by the mouse
pointer before and after the update, then dispatches
callbacks accordingly.

在渲染階段，透過在感興趣區域上放置 `MouseTrackerAnnotation` 可以檢測滑鼠事件，
例如滑鼠指標進入、退出或懸停在區域上。每次更新（一個新的幀或事件）時，
`MouseTracker` 會比較更新前後滑鼠指標懸停的註釋，然後執行相應的回呼(Callback)。

The `MouseTracker` class, which manages the state of mouse pointers,
used to require `MouseRegion` to attach annotations when mounted,
and detach annotations when unmounted.
This was used by `MouseTracker` to perform the
_mounted-exit check_ (for example, `MouseRegion.onExit`
must not be called if the exit was caused by the unmounting
of the widget), in order to prevent calling `setState`
of an unmounted widget and throwing exceptions (explained
in detail in [Issue #44631][]).

`MouseTracker` 類管理滑鼠指標的狀態，要求 `MouseRegion` 在掛載（mounted）時附加註釋，
在移除（unmounted）時分離註釋。`MouseTracker` 使用它來執行 **掛載退出檢查**。
（例如，如果是因移除 widget 引起的退出，則不呼叫 `MouseRegion.onExit`），
防止移除 widget 時呼叫 `setState` 引發的例外（詳情可見 [Issue #44631][]）。

This mechanism has been replaced by making `MouseRegion`
a stateful widget, so that it can perform the mounted-exit
check by itself by blocking the callback when unmounted.
Therefore, these methods have been removed, and `MouseTracker`
no longer tracks all annotations on the screen.

這種機制已經被 `MouseRegion` 變為一個有狀態的 widget 所取代，
它可以透過在解除安裝時阻止回呼(Callback)來自行執行掛載退出檢查。
因此，這些方法已被刪除，`MouseTracker` 不再追蹤螢幕上的所有註釋。

## Description of change

## 更改描述

The `MouseTracker` class has removed three methods related
to attaching annotations:

`MouseTracker` 類刪除了三個與附加註釋相關的方法：

```diff
 class MouseTracker extends ChangeNotifier {
   // ...
-  void attachAnnotation(MouseTrackerAnnotation annotation) {/* ... */}

-  void detachAnnotation(MouseTrackerAnnotation annotation) {/* ... */}

-  @visibleForTesting
-  bool isAnnotationAttached(MouseTrackerAnnotation annotation) {/* ... */}
 }
```

`RenderMouseRegion` and `MouseTrackerAnnotation` no longer perform the
mounted-exit check, while `MouseRegion` still does.

`RenderMouseRegion` 和 `MouseTrackerAnnotation` 不再執行掛載退出檢查，在 `MouseRegion` 中仍然執行。

## Migration guide

## 遷移指南

Calls to `MouseTracker.attachAnnotation` and
`detachAnnotation` should be removed with little to no impact:

刪除 `MouseTracker.attachAnnotation` 和 `detachAnnotation` 的呼叫，這幾乎沒有影響：

* Uses of `MouseRegion` should not be affected at all.

  `MouseRegion` 的使用應該完全不受影響。
  
* If your code directly uses `RenderMouseRegion` or
  `MouseTrackerAnnotation`, be aware that `onExit`
  is now called when the exit is caused by events that used
  to call `MouseTracker.detachAnnotation`.
  This should not be a problem if no states are involved,
  otherwise you might want to add the mounted-exit check,
  especially if the callback is leaked so that outer
  widgets might call `setState` in it. For example:

  如果你的程式碼直接使用 `RenderMouseRegion` 或 `MouseTrackerAnnotation`，
  請注意現在 `onExit` 的回呼(Callback)會在 `MouseTracker.detachAnnotation` 事件引起的退出時觸發。 
  如果不涉及任何狀態，應該不會有問題，
  否則你可能希望添加掛載的退出檢查，尤其是在回呼(Callback)洩漏時，
  以便外部 widget 可以呼叫 `setState`。例如：
  
Code before migration:

遷移前的程式碼：

<!-- skip -->
```dart
class MyMouseRegion extends SingleChildRenderObjectWidget {
  const MyMouseRegion({this.onHoverChange});

  final ValueChanged<bool> onHoverChange;

  @override
  RenderMouseRegion createRenderObject(BuildContext context) {
    return RenderMouseRegion(
      onEnter: (_) { onHoverChange(true); },
      onExit: (_) { onHoverChange(false); },
    );
  }

  @override
  void updateRenderObject(BuildContext context, RenderMouseRegion renderObject) {
    renderObject
      ..onEnter = (_) { onHoverChange(true); }
      ..onExit = (_) { onHoverChange(false); };
  }
}
```

Code after migration:

遷移後的程式碼：

<!-- skip -->
```dart
class MyMouseRegion extends SingleChildRenderObjectWidget {
  const MyMouseRegion({this.onHoverChange});

  final ValueChanged<bool> onHoverChange;

  @override
  RenderMouseRegion createRenderObject(BuildContext context) {
    return RenderMouseRegion(
      onEnter: (_) { onHoverChange(true); },
      onExit: (_) { onHoverChange(false); },
    );
  }

  @override
  void updateRenderObject(BuildContext context, RenderMouseRegion renderObject) {
    renderObject
      ..onEnter = (_) { onHoverChange(true); }
      ..onExit = (_) { onHoverChange(false); };
  }

  @override
  void didUnmountRenderObject(RenderMouseRegion renderObject) {
    renderObject
      ..onExit = onHoverChange == null ? null : (_) {};
  }
}
```

Calls to `MouseTracker.isAnnotationAttached` must be removed.
This feature is no longer technically possible,
since annotations are no longer tracked.
If you somehow need this feature, please submit an issue.

必須刪除 `MouseTracker.isAnnotationAttached` 的呼叫。
由於註釋不再被追蹤，因此該功能在技術上不再可行。如果你需要此功能，請提交 issue。

## Timeline

## 時間軸

Landed in version: 1.15.4<br>
In stable release: 1.17

釋出於版本：1.15.4<br>
釋出於穩定版本：1.17

## References

## 參考文獻

API documentation:

API 文件：

* [`MouseRegion`][]
* [`MouseTracker`][]
* [`MouseTrackerAnnotation`][]
* [`RenderMouseRegion`][]

Relevant PRs:

相關 PR：

* [MouseTracker no longer requires annotations attached][],
  which made the change
* [Improve MouseTracker lifecycle: Move checks to post-frame][],
  which first introduced the mounted-exit change,
  explained at _The change to onExit_.


[Improve MouseTracker lifecycle: Move checks to post-frame]: {{site.repo.flutter}}/issues/44631
[Issue #44631]: {{site.repo.flutter}}/pull/44631)
[`MouseRegion`]: {{site.api}}/flutter/widgets/MouseRegion-class.html
[`MouseTracker`]: {{site.api}}/flutter/gestures/MouseTracker-class.html
[MouseTracker no longer requires annotations attached]: {{site.repo.flutter}}/issues/48453
[`MouseTrackerAnnotation`]: {{site.api}}/flutter/gestures/MouseTrackerAnnotation-class.html
[`RenderMouseRegion`]: {{site.api}}/flutter/rendering/RenderMouseRegion-class.html
