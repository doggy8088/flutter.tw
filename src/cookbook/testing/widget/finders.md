---
title: Find widgets
title: 定位到目標 widget
description: How to use the Finder classes for testing widgets.
description: 如何在 widget 測試中使用 Finder 類別。
tags: cookbook, 實用課程, 測試
keywords: 定位 widget,CommonFinders,flutter_test
---

<?code-excerpt path-base="cookbook/testing/widget/finders/"?>

{% assign api = site.api | append: '/flutter' -%}

To locate widgets in a test environment, use the [`Finder`][]
classes. While it's possible to write your own `Finder` classes,
it's generally more convenient to locate widgets using the tools
provided by the [`flutter_test`][] package.

在測試環境下，為了定位 widgets，我們需要用到 [`Finder`][] 類別。
我們可以編寫自己的 `finder` 類，
不過通常使用 [`flutter_test`][] package 提供的工具來定位 widgets 更加方便。

During a `flutter run` session on a widget test, you can also
interactively tap parts of the screen for the Flutter tool to
print the suggested `Finder`.

執行 widget 測試，在執行 `flutter run` 的時候，你還可以與螢幕進行互動點選，
Flutter 工具會輸出建議的 `Finder`。

This recipe looks at the [`find`][] constant provided by
the `flutter_test` package, and demonstrates how
to work with some of the `Finders` it provides.
For a full list of available finders,
see the [`CommonFinders` documentation][].

下面，我們來看看 `flutter_test` package 提供的 [`find`][]
常量並示範如何使用其所提供的 `Finders`。
如需檢視完整的 finders 的列表，
請參閱 [`CommonFinders` 文件][`CommonFinders` documentation]。

If you're unfamiliar with widget testing and the role of
`Finder` classes,
review the [Introduction to widget testing][] recipe.

如果你還不熟悉 Widget 測試和 Finder 類使用方法，
請參閱 [Widget 測試介紹][Introduction to widget testing]。

This recipe uses the following steps:

本課程包含以下步驟：

  1. Find a `Text` widget.
  
     查詢 `Text` Widget 
  
  2. Find a widget with a specific `Key`.
  
     使用具體 `Key` 查詢 Widget
  
  3. Find a specific widget instance.
  
     查詢具體的 Widget 例項
  

### 1. Find a `Text` widget

### 一. 查詢 `Text` widget

In testing, you often need to find widgets that contain specific text.
This is exactly what the `find.text()` method is for. It creates a
`Finder` that searches for widgets that display a specific `String` of text.

在測試中，我們經常需要查詢含有特定文字的 widget。
這正是 `find.text()` 的用途。它會建立一個 `Finder`
來尋找顯示特定文字 `String` 的 widget。

<?code-excerpt "test/finders_test.dart (test1)"?>
```dart
testWidgets('finds a Text widget', (tester) async {
  // Build an App with a Text widget that displays the letter 'H'.
  await tester.pumpWidget(const MaterialApp(
    home: Scaffold(
      body: Text('H'),
    ),
  ));

  // Find a widget that displays the letter 'H'.
  expect(find.text('H'), findsOneWidget);
});
```

### 2. Find a widget with a specific `Key`

### 二. 使用具體 `Key` 查詢 widget

In some cases, you might want to find a widget based on the Key that has been
provided to it. This can be handy if displaying multiple instances of the
same widget. For example, a `ListView` might display several
`Text` widgets that contain the same text.

有時，我們可能想要透過已經提供給 widget 的 Key 來查詢 widget。
這樣在顯示多個相同 widget 實體時會很方便。
比如，我們有一個 `ListView` 列表，
它顯示了數個含有相同文字的 `Text` widgets。

In this case, provide a `Key` to each widget in the list. This allows
an app to uniquely identify a specific widget, making it easier to find
the widget in the test environment.

在這種情況下，我們可以為列表中的每一個 widget 賦予一個 `Key`。
這樣我們就可以唯一識別特定的 widget，
在測試環境中更容易查詢 widget。

<?code-excerpt "test/finders_test.dart (test2)"?>
```dart
testWidgets('finds a widget using a Key', (tester) async {
  // Define the test key.
  const testKey = Key('K');

  // Build a MaterialApp with the testKey.
  await tester.pumpWidget(MaterialApp(key: testKey, home: Container()));

  // Find the MaterialApp widget using the testKey.
  expect(find.byKey(testKey), findsOneWidget);
});
```

### 3. Find a specific widget instance

### 三. 查詢具體的 Widget 例項

Finally, you might be interested in locating a specific instance of a widget.
For example, this can be useful when creating widgets that take a `child`
property and you want to ensure you're rendering the `child` widget.

最後，我們有時會需要查詢 widget 的具體例項。
比如，當建立含有 `child` 屬性的 widget 並需要確保渲染 `child` widget。

<?code-excerpt "test/finders_test.dart (test3)"?>
```dart
testWidgets('finds a specific instance', (tester) async {
  const childWidget = Padding(padding: EdgeInsets.zero);

  // Provide the childWidget to the Container.
  await tester.pumpWidget(Container(child: childWidget));

  // Search for the childWidget in the tree and verify it exists.
  expect(find.byWidget(childWidget), findsOneWidget);
});
```

### Summary

### 總結

The `find` constant provided by the `flutter_test` package provides
several ways to locate widgets in the test environment. This recipe
demonstrated three of these methods, and several more methods exist
for different purposes.

在測試環境下，`flutter_test` 套件提供的 `find` 常量
給了我們多種查詢 widget 的方法。
本篇列舉了三種方法，另外還有一些其他用途的方法。

If the above examples do not work for a particular use-case,
see the [`CommonFinders` documentation][]
to review all available methods.

如果上述範例不適用於一些特殊情況，
請到 [`CommonFinders` 文件][`CommonFinders` documentation] 中檢視更多用法。

### Complete example

### 完整範例

<?code-excerpt "test/finders_test.dart"?>
```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

void main() {
  testWidgets('finds a Text widget', (tester) async {
    // Build an App with a Text widget that displays the letter 'H'.
    await tester.pumpWidget(const MaterialApp(
      home: Scaffold(
        body: Text('H'),
      ),
    ));

    // Find a widget that displays the letter 'H'.
    expect(find.text('H'), findsOneWidget);
  });

  testWidgets('finds a widget using a Key', (tester) async {
    // Define the test key.
    const testKey = Key('K');

    // Build a MaterialApp with the testKey.
    await tester.pumpWidget(MaterialApp(key: testKey, home: Container()));

    // Find the MaterialApp widget using the testKey.
    expect(find.byKey(testKey), findsOneWidget);
  });

  testWidgets('finds a specific instance', (tester) async {
    const childWidget = Padding(padding: EdgeInsets.zero);

    // Provide the childWidget to the Container.
    await tester.pumpWidget(Container(child: childWidget));

    // Search for the childWidget in the tree and verify it exists.
    expect(find.byWidget(childWidget), findsOneWidget);
  });
}
```

[`Finder`]: {{api}}/flutter_test/Finder-class.html
[`CommonFinders` documentation]: {{api}}/flutter_test/CommonFinders-class.html
[`find`]: {{api}}/flutter_test/find-constant.html
[`flutter_test`]: {{api}}/flutter_test/flutter_test-library.html
[Introduction to widget testing]: {{site.url}}/cookbook/testing/widget/introduction
