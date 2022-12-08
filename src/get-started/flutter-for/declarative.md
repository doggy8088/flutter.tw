---
title: Introduction to declarative UI
title: 宣告式 UI 介紹
short-title: Declarative UI
short-title: 宣告式 UI
description: Explains the difference between a declarative and imperative programming style.
description: 描述和解釋宣告式和指令式程式設計思想的差異。
tags: Flutter課程,Flutter起步,Flutter入門
keywords: 概覽,宣告式程式設計,響應式程式設計,UI框架
---

<?code-excerpt path-base="get-started/flutter-for/declarative"?>

_This introduction describes the conceptual difference between the
declarative style used by Flutter, and the imperative style used by
many other UI frameworks._

**這篇介紹描述了 Flutter 所使用的宣告式 UI 和許多其他 UI 框架所使用的命令式 UI 的概念性差異**

## Why a declarative UI?

## 為什麼是宣告式 UI？

Frameworks from Win32 to web to Android and iOS typically use an imperative
style of UI programming. This might be the style you’re most familiar
with&mdash;where you manually construct a full-functioned UI entity,
such as a UIView or equivalent, and later mutate it using methods and
setters when the UI changes.

從 Win32 到 Web 再到 Android 和 iOS，
框架通常使用一種命令式的程式設計風格來完成 UI 程式設計。
這可能是你最熟悉的風格&mdash;&mdash;手動建構一個全功能的 UI 例項，
比如一個 UIView 或其他類似的內容，
在隨後 UI 發生變化時，使用方法或 Setter 修改它。

In order to lighten the burden on developers from having to program how to
transition between various UI states, Flutter, by contrast,
lets the developer describe the current UI state and leaves the
transitioning to the framework.

為了減輕開發人員的負擔，無需編寫如何在不同的 UI 狀態之間進行切換的程式碼，
Flutter 相反，讓開發人員描述當前的 UI 狀態，並將轉換交給框架。

This, however, requires a slight shift in thinking for how to manipulate UI.

然而，這需要稍微改變下如何操作 UI 的思考方式。

## How to change UI in a declarative framework

## 如何在命令式框架中修改 UI

Consider a simplified example below:

思考像下面這樣一個簡單的例子:

<img src="/assets/images/docs/declarativeUIchanges.png" alt="View B (contained by view A) morphs from containing two views, c1 and c2, to containing only view c3">

In the imperative style, you would typically go to ViewB’s owner
and retrieve the instance `b` using selectors or with `findViewById` or similar,
and invoke mutations on it (and implicitly invalidate it). For example:

在命令式風格中，你通常需要使用選擇器 `findViewById` 
或類似函式獲取到 ViewB 的例項 `b` 和所有權，
並呼叫相關的修改的方法（並隱含的使其失效）。例如：

```java
// Imperative style
b.setColor(red)
b.clearChildren()
ViewC c3 = new ViewC(...)
b.add(c3)
```

You might also need to replicate this configuration in the constructor of
ViewB since the source of truth for the UI might outlive instance `b` itself.

由於 UI 真實的來源可能比例項 `b` 本身的存活週期更長，
你可能還需要在 ViewB 的建構函式中複製此配置。

In the declarative style, view configurations (such as Flutter’s Widgets)
are immutable and are only lightweight "blueprints". To change the UI,
a widget triggers a rebuild on itself (most commonly by calling `setState()`
on StatefulWidgets in Flutter) and constructs a new Widget subtree.

在宣告式風格中，檢視配置（如 Flutter 的 Widget ）是不可變的，
它只是輕量的「藍圖」。要改變 UI，widget 會在自身上觸發重建
（在 Flutter 中最常見的方法是在 `StatefulWidget` 上呼叫 `setState()`）
並構造一個新的 Widget 子樹。

<?code-excerpt "lib/main.dart (declarative)"?>
```dart
// Declarative style
return ViewB(
  color: red,
  child: const ViewC(),
);
```

Here, rather than mutating an old instance `b` when the UI changes,
Flutter constructs new Widget instances. The framework manages many of the
responsibilities of a traditional UI object (such as maintaining the
state of the layout) behind the scenes with RenderObjects.
RenderObjects persist between frames and Flutter’s lightweight Widgets
tell the framework to mutate the RenderObjects between states.
The Flutter framework handles the rest.

在這裡，當用戶介面發生變化時，Flutter 不會修改舊的例項 `b`，
而是構造新的 widget 例項。
框架使用 `RenderObject` 管理傳統 UI 物件的職責（比如維護佈局的狀態）。
`RenderObject` 在幀之間保持不變，
Flutter 的輕量級 widget 通知框架在狀態之間修改 `RenderObject`，
Flutter 框架則處理其餘部分。
