---
title: Scrollable AlertDialog
title: 可滾動的 AlertDialog (不再棄用)
description: AlertDialog should scroll automatically when it overflows.
description: 當 AlertDialog 邊界溢位時將會自動變為可滾動的。
---

## Summary

## 概述

{{site.alert.note}}

  `AlertDialog.scrollable` is no longer deprecated because there is
  no backwards-compatible way to make `AlertDialog` scrollable by default.
  Instead, the parameter will remain and you can set `scrollable`
  to true if you want a scrollable `AlertDialog`.
  
  因為沒有向後相容的方法使 `AlertDialog` 在預設情況下可滾動，
  所以 `AlertDialog.scrollable` 不再棄用。
  相反，引數將保持不變，如果需要 `AlertDialog` 可滾動，可以將 `scrollable` 設定為 true。
    
{{site.alert.end}}

An `AlertDialog` now scrolls automatically when it overflows.

`AlertDialog` 現在會在繪製溢位時自動變為可滾動狀態。

## Context

## 上下文

Before this change,
when an `AlertDialog` widget’s contents were too tall,
the display overflowed, causing the contents to be clipped.
This resulted in the following issues:

在此更改之前，當 `AlertDialog` widget 中的內容過高時，
會顯示繪製溢位，使內容被剪裁。這會導致以下問題：

* There was no way to view the portion of the content that was clipped.

  無法檢視被剪裁的內容。

* Most alert dialogs have buttons beneath the content to prompt users for
  actions. If the content overflowed, obscuring the buttons,
  users might be unaware of their existence.

  大多數 `AlertDialog` 的內容下方都有按鈕，
  用於提示使用者執行操作。如果內容溢位，遮蓋了按鈕，
  使用者可能不知道它們的存在。

## Description of change

## 更改描述

The previous approach listed the title and content
widgets consecutively in a `Column` widget.

在改動前，可以使用下面的方法在 `Column` widget 中連續地列出標題和內容 widget。

<!-- skip -->
```dart
Column(
  mainAxisSize: MainAxisSize.min,
  crossAxisAlignment: CrossAxisAlignment.stretch,
  children: <Widget>[
    if (title != null)
      Padding(
        padding: titlePadding ?? EdgeInsets.fromLTRB(24.0, 24.0, 24.0, content == null ? 20.0 : 0.0),
        child: DefaultTextStyle(
          style: titleTextStyle ?? dialogTheme.titleTextStyle ?? theme.textTheme.title,
          child: Semantics(
          child: title,
          namesRoute: true,
          container: true,
          ),
        ),
      ),
    if (content != null)
      Flexible(
        child: Padding(
        padding: contentPadding,
        child: DefaultTextStyle(
          style: contentTextStyle ?? dialogTheme.contentTextStyle ?? theme.textTheme.subhead,
          child: content,
        ),
      ),
    ),
    // ...
  ],
);
```

The new approach wraps both widgets in a
`SingleChildScrollView` above the button bar,
making both widgets part of the same scrollable
and exposing the button bar at the bottom of the dialog.

在改動後，它們被包裹在按鈕欄上方的 `SingleChildScrollView` 中，
作為同一個可滾動的模組的一部分，
按鈕欄將顯示在對話方塊底部。

<!-- skip -->
```dart
Column(
  mainAxisSize: MainAxisSize.min,
  crossAxisAlignment: CrossAxisAlignment.stretch,
  children: <Widget>[
    if (title != null || content != null)
      SingleChildScrollView(
        child: Column(
        mainAxisSize: MainAxisSize.min,
        crossAxisAlignment: CrossAxisAlignment.stretch,
         children: <Widget>[
           if (title != null)
             titleWidget,
             if (content != null)
             contentWidget,
         ],
       ),
     ),
   // ...
  ],
),
```

## Migration guide

## 遷移指南

You might see the following issues as a result of this change:

此更改可能會導致以下問題：

**Semantics tests might fail because of the addition of a `SingleChildScrollView`.**
: Manual testing of the `Talkback` and `VoiceOver` features
  show that they still exhibit the same (correct)
  behavior as before.
  
**由於添加了 `SingleChildScrollView`，語義測試可能會失敗。**
<br>對 `Talkback` 和 `VoiceOver` 功能的手動測試表明，
它們仍然表現出與以前相同的（正確的）行為。

**Golden tests might fail.**
: This change might have caused diffs in (previously passing)
  golden tests since the `SingleChildScrollView` now nests both the
  title and content widgets.
  Some Flutter projects have taken to creating semantics tests
  by taking goldens of semantics nodes used in Flutter's debug build.
  
**Golden Test 可能會失敗。** 
<br>由於 `SingleChildScrollView` 現在嵌套了標題和內容 widget，
  因此可能導致 Golden Test 出現不同的結果。
  一些 Flutter 專案已經開始透過獲取 Flutter debug 建構過程中使用的語義節點 goldens 來建立語義測試。

  <br>Any semantics golden updates that reflect the scrolling
  container addition are expected and these diffs should be safe to accept.

  <br>滾動容器上附加的所有語義 golden 變動都是符合預期的，它們不會造成其他後果。

  Sample resulting Semantics tree:
  
  語義樹範例：

```
flutter:        ├─SemanticsNode#30 <-- SingleChildScrollView
flutter:          │ flags: hasImplicitScrolling
flutter:          │ scrollExtentMin: 0.0
flutter:          │ scrollPosition: 0.0
flutter:          │ scrollExtentMax: 0.0
flutter:          │
flutter:          ├─SemanticsNode#31 <-- title
flutter:          │   flags: namesRoute
flutter:          │   label: "Hello"
flutter:          │
flutter:          └─SemanticsNode#32 <-- contents
flutter:              label: "Huge content"
```

**Layout changes might result because of the scroll view.**
: If the dialog was already overflowing,
  this change corrects the problem.
  This layout change is expected.
  
**滾動檢視可能導致佈局更改。**
<br>該變更會修復對話方塊發生繪製溢位的問題。
  這種佈局上的變化是意料之中的。

  <br>A nested `SingleChildScrollView` in `AlertDialog.content`
  should work properly if left in the code,
  but should be removed if unintended, since
  it might cause confusion.
    
  <br>當代碼中有 `SingleChildScrollView` 巢狀(Nesting)在
  `AlertDialog.content` 時，那麼對話方塊應正常展示。
  但如果不是有意為之，則應將其移除，否則可能導致程式碼更為不可讀。
  
Code before migration:

遷移前的程式碼：

<!-- skip -->
```dart
AlertDialog(
  title: Text(
    'Very, very large title that is also scrollable',
    textScaleFactor: 5,
  ),
  content: SingleChildScrollView( // won't be scrollable
    child: Text('Scrollable content', textScaleFactor: 5),
  ),
  actions: <Widget>[
    TextButton(child: Text('Button 1'), onPressed: () {}),
    TextButton(child: Text('Button 2'), onPressed: () {}),
  ],
)
```

Code after migration:

遷移後的程式碼：

<!-- skip -->
```dart
AlertDialog(
  title: Text('Very, very large title', textScaleFactor: 5),
  content: Text('Very, very large content', textScaleFactor: 5),
  actions: <Widget>[
    TextButton(child: Text('Button 1'), onPressed: () {}),
    TextButton(child: Text('Button 2'), onPressed: () {}),
  ],
)
```

## Timeline

## 時間線

Landed in version: 1.16.3<br>
In stable release: 1.17

釋出於版本：1.16.3<br>
釋出於穩定版本：1.17

## References

## 參考文獻

Design doc:

設計文件

* [Scrollable `AlertDialog`][]

API documentation:

API 文件：

* [`AlertDialog`][]

Relevant issue:

相關 issue：

* [Overflow exceptions with maximum accessibility font size][]

Relevant PRs:

相關 PRs：

* [Update to `AlertDialog.scrollable`][]
* [Original attempt to implement scrollable `AlertDialog`][]
* [Revert of original attempt to implement scrollable `AlertDialog`][]


[`AlertDialog`]: {{site.api}}/flutter/material/AlertDialog-class.html
[Original attempt to implement scrollable `AlertDialog`]: {{site.repo.flutter}}/pull/43226
[Overflow exceptions with maximum accessibility font size]: {{site.repo.flutter}}/issues/42696
[Revert of original attempt to implement scrollable `AlertDialog`]: {{site.repo.flutter}}/pull/44003
[Scrollable `AlertDialog`]: {{site.url}}/go/scrollable-alert-dialog
[Update to `AlertDialog.scrollable`]: {{site.repo.flutter}}/pull/45079
