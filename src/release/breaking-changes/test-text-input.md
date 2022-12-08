---
title: TestTextInput state reset
title: 重置 TestTextInput 狀態
description: TestTextInput state is now reset between tests.
description: 測試之間，TestTextInput 的狀態將重置。
---

## Summary

## 概述

The state of a `TestTextInput` instance,
a stub for the system's onscreen keyboard,
is now reset between tests.

現在 `TestTextInput` 例項（系統螢幕鍵盤的存根）的狀態會在測試之間重置。

## Context

## 上下文

The Flutter test framework uses a class called `TestTextInput`
to track and manipulate editing state in a widgets test.
Individual tests can make calls that modify the internal
state of this object, sometimes indirectly (such as
by setting their own handlers on `SystemChannels.textInput`).
Subsequent tests might then check the state of
`WidgetTester.testTextInput` and get unexpected values.

Flutter 測試框架中使用一個名為 `TestTextInput` 的類來追蹤和操作 widgets 測試中的編輯狀態。
個別測試可以透過方法呼叫來修改此物件的內部狀態，
有時是間接的（例如透過 `SystemChannels.textInput` 設定自己的處理程式）。
在其之後的的測試可能會檢查 `WidgetTester.testTextInput` 的狀態，拿到不符合預期的值。

## Description of change

## 更改描述

The state of `WidgetTester.testTextInput`
is now reset before running a `testWidgets` test.

現在 `WidgetTester.testTextInput` 的狀態會在執行 `testWidgets` 測試之前重置。

## Migration guide

## 遷移指南

Tests that relied on dirty state from a previously run
test must be updated. For example, the following test,
from `packages/flutter/test/material/text_field_test.dart`
in the `'Controller can update server'` test,
previously passed because of a combination of dirty state
from previous tests and a failure to actually set state
in cases where it should have been set.

之前依賴於 `WidgetTester.testTextInput` 執行時髒狀態的測試必須更新。
例如 `packages/flutter/test/material/text_field_test.dart`
檔案中的 `'Controller can update server'` 測試。
在這之前，其它測試使得 `WidgetTester.testTextInput` 處於髒狀態，
並且在應該設定狀態的時候設定狀態失敗，所以它通過了測試。

Code before migration:

遷移前的程式碼：

In a `widgetsTest`, before actually changing text on a
text editing widget, this call might have succeeded:

`widgetsTest` 裡，在實際更改文字編輯 widget 上的文字之前，此呼叫可能已成功：

<!-- skip -->
```dart
    expect(tester.testTextInput.editingState['text'], isEmpty);
```

Code after migration:

遷移後的程式碼：

Either remove the call entirely, or consider using the
following to assert that the state hasn't been modified yet:

要麼完全刪除該呼叫，要麼考慮使用以下宣告來確認狀態尚未被修改：

<!-- skip -->
```dart
    expect(tester.testTextInput.editingState, isNull);
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

* [`TestTextInput`][]
* [`WidgetTester`][]

Relevant issue:

相關 issues：

* [Randomize test order to avoid global state][]

Relevant PR:

相關 PR：

* [Reset state between tests][]


[Randomize test order to avoid global state]: {{site.repo.flutter}}/issues/47233
[Reset state between tests]: {{site.repo.flutter}}/pull/47464
[`TestTextInput`]: {{site.api}}/flutter/flutter_test/TestTextInput-class.html
[`WidgetTester`]: {{site.api}}/flutter/flutter_test/WidgetTester-class.html
