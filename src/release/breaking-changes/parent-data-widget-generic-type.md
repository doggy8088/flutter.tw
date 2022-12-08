---
title: The generic type of ParentDataWidget changed to ParentData
title: ParentDataWidget 的泛型已更改為 ParentData。
description: The ParentDataWidget is now bound to the ParentData type.
description: 現在 ParentDataWidget 已繫結為 ParentData 型別。
---

## Summary

## 概述

The generic type of `ParentDataWidget` has changed from
`RenderObjectWidget` to `ParentData`.

`ParentDataWidget` 的泛型已由 `RenderObjectWidget` 更改為 `ParentData`。

## Context

## 上下文

Prior to this change, a `ParentDataWidget` was bound
to a specific `RenderObjectWidget` type as ancestor.
For example, a `Positioned` widget could only be used
within a `Stack` widget. With this change,
a `ParentDataWidget` can be used with any
`RenderObjectWidget` type as ancestor as long as
the `RenderObject` of said `RenderObjectWidget`
sets up the correct `ParentData` type. In this new world,
the `Positioned` widget can be reused with a hypothetical
new `SuperStack` widget.

在此次更改之前，`ParentDataWidget` 是作為祖先節點被繫結至特定的 `RenderObjectWidget` 型別的。
例如：`Positioned` 部件只能在 `Stack` 部件中使用。
透過此次更改，只要上述 `RenderObjectWidget` 的 `RenderObject`
設定了正確的 `ParentData` 型別，
`ParentDataWidget` 就可以作為任意 `RenderObjectWidget` 型別的祖先節點和其一起使用。
透過這種新的方式，`Positioned` 部件就可以透過假想的 `SuperStack` 新部件而被重用了。

## Description of change

## 更改描述

The generic type argument of `ParentDataWidget`
has changed from `RenderObjectWidget` to `ParentData`,
and a new debug property, `debugTypicalAncestorWidgetClass`,
is added to `ParentDataWidget`.
The latter is used for error messages to give users a
better idea of the context a given `ParentDataWidget`
is supposed to be used in.

`ParentDataWidget` 的泛型引數已由 `RenderObjectWidget` 更改為 `ParentData`，
並添加了一個新的除錯屬性 `debugTypicalAncestorWidgetClass`。
後者用於展示錯誤資訊，讓使用者更好地理解 `ParentDataWidget` 中應該使用的上下文。

## Migration guide

## 遷移指南

You must migrate your code as described in this section
if you're subclassing or implementing `ParentDataWidget`.
If you do, the analyzer shows the following warnings when you
upgrade to the Flutter version that includes this change:

如果您繼承或實現了 `ParentDataWidget`，則必須按照本節所述遷移您的程式碼。
當您升級至包含了此更改的 Flutter 版本時，分析器會展示如下警告資訊：

```none
  error • Missing concrete implementation of 'getter ParentDataWidget.debugTypicalAncestorWidgetClass' • lib/main.dart:114:7 • non_abstract_class_inherits_abstract_member
  error • 'FrogJar' doesn't extend 'ParentData' • lib/main.dart:114:41 • type_argument_not_matching_bounds
```

Code before migration:

遷移前程式碼：

<!-- skip -->
```dart
class FrogSize extends ParentDataWidget<FrogJar> {
  FrogSize({
    Key key,
    @required this.size,
    @required Widget child,
  }) : assert(child != null),
        assert(size != null),
        super(key: key, child: child);

  final Size size;

  @override
  void applyParentData(RenderObject renderObject) {
    final FrogJarParentData parentData = renderObject.parentData;
    if (parentData.size != size) {
      parentData.size = size;
      final RenderFrogJar targetParent = renderObject.parent;
      targetParent.markNeedsLayout();
    }
  }
}

class FrogJarParentData extends ParentData {
  Size size;
}

class FrogJar extends RenderObjectWidget {
  // ...
}
```

Code after migration:

遷移後代碼：

<!-- skip -->
```dart
class FrogSize extends ParentDataWidget<FrogJarParentData> { // FrogJar changed to FrogJarParentData
  FrogSize({
    Key key,
    @required this.size,
    @required Widget child,
  }) : assert(child != null),
        assert(size != null),
        super(key: key, child: child);

  final Size size;

  @override
  void applyParentData(RenderObject renderObject) {
    final FrogJarParentData parentData = renderObject.parentData;
    if (parentData.size != size) {
      parentData.size = size;
      final RenderFrogJar targetParent = renderObject.parent;
      targetParent.markNeedsLayout();
    }
  }

  @override
  Type get debugTypicalAncestorWidgetClass => FrogJar; // Newly added
}
```

The generic type of the `ParentDataWidget` superclass
changes from `FrogJar` (a `RenderObjectWidget`) to
`FrogJarParentData` (the `ParentData` type that
`FrogSize.applyParentData` wants to operate on).
Additionally, the new `debugTypicalAncestorWidgetClass`
is implemented for this `ParentDataWidget` subclass.
It returns the type of a typical ancestor `RenderObjectWidget`
for this `ParentDataWidget`. Most of the time,
you just want to return the old generic type here
(`FrogJar` in this example).

父類 `ParentDataWidget` 的泛型從 `FrogJar`（一個 `RenderObjectWidget`）更改為 `FrogJarParentData`
（`FrogSize.applyParentData` 所操作的 `ParentData` 型別）。
除此之外，子類別 `ParentDataWidget` 實現了新的 `debugTypicalAncestorWidgetClass`。
它給 `ParentDataWidget` 返回了一個典型的 `RenderObjectWidget` 型別的祖先節點。
大多數時候，您只是想在此處返回舊的泛型（在這個範例中就是 `FrogJar`）。

## Timeline

## 時間軸

Landed in version: 1.16.3<br>
In stable release: 1.17

釋出於版本：1.16.3<br>
釋出於穩定版本：1.17

## References

## 參考文獻

API documentation:

API 文件：

* [`ParentDataWidget`][]

Relevant PR:

相關 PR：

* [Make ParentDataWidget usable with different ancestor RenderObjectWidget types][]

[Make ParentDataWidget usable with different ancestor RenderObjectWidget types]: {{site.repo.flutter}}/pull/48541
[`ParentDataWidget`]: {{site.api}}/flutter/widgets/ParentDataWidget-class.html
