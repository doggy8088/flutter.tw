---
title: Handle scrolling
title: 模擬滾動操作
description: How to handle scrolling in a widget test.
description: 如何在 widget 測試中模擬滾動操作。
---

<?code-excerpt path-base="cookbook/testing/widget/scrolling/"?>

Many apps feature lists of content,
from email clients to music apps and beyond.
To verify that lists contain the expected content
using widget tests,
you need a way to scroll through lists to search for particular items.

許多應用程式（電子郵件客戶端、音樂應用程式等）都具有內容列表功能。
要使用 widget 測試來驗證列表是否包含預期內容，
你需要一種方法來滾動列表以檢索指定的專案。

To scroll through lists via integration tests,
use the methods provided by the [`WidgetTester`][] class,
which is included in the [`flutter_test`][] package:

請在整合測試中使用 [`WidgetTester`][] 類提供的方法測試滾動列表，
該類包含在 [`flutter_test`][] package 中：

In this recipe, learn how to scroll through a list of items to
verify a specific widget is being displayed, and discuss the pros on cons of
different approaches. 

在本指南中，你將學習如何滾動列表來驗證指定的 widget 是否正在顯示，
還會了解到不同做法的利弊。

This recipe uses the following steps:

本指南會採用以下步驟：

1. Create an app with a list of items.

   建立帶有列表的應用程式。

2. Write a test that scrolls through the list.

   編寫滾動列表的測試。

3. Run the test.

   執行測試。

### 1. Create an app with a list of items

### 1、建立帶有列表的應用程式

This recipe builds an app that shows a long list of items.
To keep this recipe focused on testing, use the app created in the
[Work with long lists][] recipe.
If you're unsure of how to work with long lists,
see that recipe for an introduction.

本章節建立了一個顯示長列表的應用程式。
為了專注於測試的編寫，
這裡使用 [長列表的處理][Work with long lists] 指南中建立的應用程式。
如果你不確定如何使用長列表，請查閱該指南。

Add keys to the widgets you want to interact with
inside the integration tests.

請在需要互動的 widget 上新增 key 以便
進行整合測試。

<?code-excerpt "lib/main.dart"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp(
    items: List<String>.generate(10000, (i) => 'Item $i'),
  ));
}

class MyApp extends StatelessWidget {
  final List<String> items;

  const MyApp({super.key, required this.items});

  @override
  Widget build(BuildContext context) {
    const title = 'Long List';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(title),
        ),
        body: ListView.builder(
          // Add a key to the ListView. This makes it possible to
          // find the list and scroll through it in the tests.
          key: const Key('long_list'),
          itemCount: items.length,
          itemBuilder: (context, index) {
            return ListTile(
              title: Text(
                items[index],
                // Add a key to the Text widget for each item. This makes
                // it possible to look for a particular item in the list
                // and verify that the text is correct
                key: Key('item_${index}_text'),
              ),
            );
          },
        ),
      ),
    );
  }
}
```


### 2. Write a test that scrolls through the list

### 2、編寫滾動列表的測試

Now, you can write a test. In this example, scroll through the list of items and
verify that a particular item exists in the list. The [`WidgetTester`][] class
provides the [`scrollUntilVisible()`][] method, which scrolls through a list
until a specific widget is visible. This is useful because the height of the
items in the list can change depending on the device.

現在，你可以編寫一個測試：滾動列表並驗證列表中是否存在指定的專案。
使用 [`WidgetTester`][] 類提供的 [`scrollUntilVisible()`][] 方法，
它可以滾動列表，直到某個指定的 widget 可見為止。
這個方法非常有用，
因為列表中專案的高度會根據裝置的不同而變化。

Rather than assuming that you know the height of all the items
in a list, or that a particular widget is rendered on all devices,
the `scrollUntilVisible()` method repeatedly scrolls through
a list of items until it finds what it's looking for.

`scrollUntilVisible()` 方法只會緩慢地滾動列表，直至找到指定的內容。
它不會假定你知道列表中所有專案的高度，
也不會假定指定的 widget 會在所有裝置上渲染。

The following code shows how to use the `scrollUntilVisible()` method
to look through the list for a particular item. This code lives in a
file called `test/widget_test.dart`.

以下程式碼展示瞭如何使用 `scrollUntilVisible()` 方法
在列表中查詢到指定的專案。
這段程式碼位於 `test/widget_test.dart` 檔案中。

<?code-excerpt "test/widget_test.dart (ScrollWidgetTest)"?>
```dart

// This is a basic Flutter widget test.
//
// To perform an interaction with a widget in your test, use the WidgetTester
// utility that Flutter provides. For example, you can send tap and scroll
// gestures. You can also use WidgetTester to find child widgets in the widget
// tree, read text, and verify that the values of widget properties are correct.

import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

import 'package:scrolling/main.dart';

void main() {
  testWidgets('finds a deep item in a long list', (tester) async {
    // Build our app and trigger a frame.
    await tester.pumpWidget(MyApp(
      items: List<String>.generate(10000, (i) => 'Item $i'),
    ));

    final listFinder = find.byType(Scrollable);
    final itemFinder = find.byKey(const ValueKey('item_50_text'));

    // Scroll until the item to be found appears.
    await tester.scrollUntilVisible(
      itemFinder,
      500.0,
      scrollable: listFinder,
    );

    // Verify that the item contains the correct text.
    expect(itemFinder, findsOneWidget);
  });
}
```

### 3. Run the test

### 3、執行測試

Run the test using the following command from the root of the project:

在專案根目錄下使用以下指令執行測試：

```
flutter test test/widget_test.dart
```

[`flutter_test`]: {{site.api}}/flutter/flutter_test/flutter_test-library.html
[`WidgetTester`]: {{site.api}}/flutter/flutter_test/WidgetTester-class.html
[`ListView.builder`]: {{site.api}}/flutter/widgets/ListView/ListView.builder.html
[`scrollUntilVisible()`]: {{site.api}}/flutter/flutter_test/WidgetController/scrollUntilVisible.html
[Work with long lists]: {{site.url}}/cookbook/lists/long-lists
