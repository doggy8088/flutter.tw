---
title: Rebuild optimization for OverlayEntries and Routes
title: OverlayEntries 和 Routes 進行了重建最佳化
description: OverlayEntries only rebuild on explicit state changes.
description: OverlayEntries 僅在顯式狀態更改時重建。
---

## Summary

## 概述

This  optimization improves performance for route transitions,
but it may uncover missing calls to `setState` in your app.

本次最佳化提高了路由切換時的效能，但可能會揭露出你的應用中沒有顯式呼叫 `setState` 的問題。 

## Context

## 上下文

Prior to this change, an `OverlayEntry` would rebuild when
a new opaque entry was added on top of it or removed above it.
These rebuilds were unnecessary because they were not triggered
by a change in state of the affected `OverlayEntry`. This
breaking change optimized how we handle the addition and removal of
`OverlayEntry`s, and removes unnecessary rebuilds
to improve performance.

在此更改之前，當一個新的不透明 `OverlayEntry`（記作 A）被新增到另一個 `OverlayEntry`（記作 B）上，
或者 A 從 B 上移除時，B 將會重新建構。
這些重建是不必要的，因為它們不是由 `OverlayEntry` 內部狀態發生的改變而觸發的。
這個破壞性的改動優化了我們對 `OverlayEntry` 進行新增和移除的場景，
移除了不必要的重建以提高效能。

Since the `Navigator` internally puts each `Route` into an
`OverlayEntry` this change also applies to `Route` transitions:
If an opaque `Route` is pushed on top or removed from above another
`Route`, the `Route`s below the opaque `Route`
no longer rebuilds unnecessarily.

由於 `Navigator` 在內部會把每一個 `Route` 巢狀(Nesting)在 `OverlayEntry` 中，因此這個改動同樣作用於 `Route` 的變換：
如果一個不透明的 `Route` 被新增到棧頂，或是從另一個 `Route` 的上層被移除時，
位於不透明的 `Route` 下面的 `Route` 將不再進行不必要的重建。

## Description of change

## 更改描述

In most cases, this change doesn't require any changes to your code.
However, if your app was erroneously relying on the implicit
rebuilds you may see issues, which can be resolved by wrapping
any state change in a `setState` call.

在大多數情況下，本次最佳化不需要你對程式碼進行任何更改。
然而，如果你的應用錯誤地依賴了隱含重建，你可能會發現問題，
這可以透過呼叫 `setState` 進行狀態的變更來解決。

Furthermore, this change slightly modified the shape of the
widget tree: Prior to this change,
the `OverlayEntry`s were wrapped in a `Stack` widget.
The explicit `Stack` widget was removed from the widget hierarchy.

此外，這一更改略微調整了 widget 樹的層級結構：
在此更改之前，`OverlayEntry` 集合巢狀(Nesting)在 `Stack` 中。
更改後，`Stack` 將從 `widget` 樹結構中移除。

## Migration guide

## 遷移指南

If you're seeing issues after upgrading to a Flutter version
that included this change, audit your code for missing calls to
`setState`. In the example below, assigning the return value of
`Navigator.pushNamed` to `buttonLabel` is
implicitly modifying the state and it should be wrapped in an
explicit `setState` call.

如果你在升級到包含此次更改的 Flutter 版本後遇到了問題，請檢查你的程式碼是否遺漏了 `setState` 的呼叫。
在下面的例子中，`Navigator.pushNamed` 方法非同步執行完後隱含地修改了 `Text` 所展示的字串 `buttonLabel`，
它應該在顯式的 `setState` 中呼叫。
 
Code before migration:

遷移前程式碼：

<!-- skip -->
```dart
class FooState extends State<Foo> {
  String buttonLabel = 'Click Me';
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: () async {
        // Illegal state modification that should be wrapped in setState.
        buttonLabel = await Navigator.pushNamed(context, '/bar');
      },
      child: Text(buttonLabel),
    );
  }
}
```

Code after migration:

遷移後代碼：

<!-- skip -->
```dart
class FooState extends State<Foo> {
  String buttonLabel = 'Click Me';
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: () async {
        final newLabel = await Navigator.pushNamed(context, '/bar');
        setState(() {
          buttonLabel = newLabel;
        });
      },
      child: Text(buttonLabel),
    );
  }
}
```

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

* [`setState`][]
* [`OverlayEntry`][]
* [`Overlay`][]
* [`Navigator`][]
* [`Route`][]
* [`OverlayRoute`][]

Relevant issues:

相關 issues：

* [Issue 45797][]

Relevant PRs:

相關 PR：

* [Do not rebuild Routes when a new opaque Route is pushed on top][]
* [Reland "Do not rebuild Routes when a new opaque Route is pushed on top"][]


[Do not rebuild Routes when a new opaque Route is pushed on top]: {{site.repo.flutter}}/pull/48900
[Issue 45797]: {{site.repo.flutter}}/issues/45797
[`Navigator`]: {{site.api}}/flutter/widgets/Navigator-class.html
[`Overlay`]: {{site.api}}/flutter/widgets/Overlay-class.html
[`OverlayEntry`]: {{site.api}}/flutter/widgets/OverlayEntry-class.html
[`OverlayRoute`]: {{site.api}}/flutter/widgets/OverlayRoute-class.html
[`Route`]: {{site.api}}/flutter/widgets/Route-class.html
[`setState`]: {{site.api}}/flutter/widgets/State/setState.html
[Reland "Do not rebuild Routes when a new opaque Route is pushed on top"]: {{site.repo.flutter}}/pull/49376
