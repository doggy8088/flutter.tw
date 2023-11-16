---
title: Tap, drag, and enter text
title: 點選、拖拽事件和文字輸入
description: How to test widgets for user interaction.
description: 如何測試與使用者互動的 widgets。
tags: cookbook, 實用課程, 測試
keywords: 點選測試,拖拽事件測試,文字輸入測試
---

<?code-excerpt path-base="cookbook/testing/widget/tap_drag/"?>

{% assign api = site.api | append: '/flutter' -%}

Many widgets not only display information, but also respond
to user interaction. This includes buttons that can be tapped,
and [`TextField`][] for entering text.

我們建構的大部分 widget 不僅僅需要展示資訊，
還需要響應使用者互動。常見的互動有使用者點選按鈕、
在螢幕上拖動元件和在 [`TextField`][] 中輸入文字。

To test these interactions, you need a way to simulate them
in the test environment. For this purpose, use the
[`WidgetTester`][] library.

為了測試這些互動，我們需要在測試環境中模擬上述場景，
可以使用 [`WidgetTester`][] 庫。

The `WidgetTester` provides methods for entering text,
tapping, and dragging.

`WidgetTester` 提供了文字輸入、點選、拖動的相關方法：

* [`enterText()`][]
* [`tap()`][]
* [`drag()`][]

In many cases, user interactions update the state of the app. In the test
environment, Flutter doesn't automatically rebuild widgets when the state
changes. To ensure that the widget tree is rebuilt after simulating a user
interaction, call the [`pump()`][] or [`pumpAndSettle()`][]
methods provided by the `WidgetTester`.
This recipe uses the following steps:

在很多情況下，使用者互動會更新應用狀態。
在測試環境中，Flutter 在狀態發生改變的時候並不會自動重建 widget。
為了保證模擬使用者互動實現後，widget 樹能重建，一定要呼叫 `WidgetTester` 提供的
[`pump()`][] 或 [`pumpAndSettle()`][]。

### Directions

### 步驟

  1. Create a widget to test.

     建立待測 Widget

  2. Enter text in the text field.

     在文字區輸入文字

  3. Ensure tapping a button adds the todo.

     點選按鈕，增加待辦清單項

  4. Ensure swipe-to-dismiss removes the todo.

     滑動刪除待辦清單項

### 1. Create a widget to test

### 1. 建立待測 Widget

For this example,
create a basic todo app that tests three features:

在這個範例中，我們將會建立一個簡單的待辦清單應用。
其中有三個主要的功能點需要測試：

  1. Entering text into a `TextField`.

     往 `TextField` 中輸入文字

  2. Tapping a `FloatingActionButton` to add the text to a list of todos.

     點選 `FloatingActionButton`，把文字加入到待辦清單列表

  3. Swiping-to-dismiss to remove the item from the list.

     滑動移除列表中的待辦清單項

To keep the focus on testing,
this recipe won't provide a detailed guide on how to build the todo app.
To learn more about how this app is built,
see the relevant recipes:

為了聚焦在測試上，本章節並不會提供如何建構一個待辦清單應用的具體課程。
如果想要知道這個應用是如何建構的，請參考以下章節：

* [Create and style a text field][]

  [文字框的建立和設定][Create and style a text field]

* [Handle taps][]

  [捕獲和處理點選動作][Handle taps]

* [Create a basic list][]

  [基礎列表][Create a basic list]

* [Implement swipe to dismiss][]

  [實現「滑動清除」效果][Implement swipe to dismiss]

<?code-excerpt "test/main_test.dart (TodoList)"?>
```dart
class TodoList extends StatefulWidget {
  const TodoList({super.key});

  @override
  State<TodoList> createState() => _TodoListState();
}

class _TodoListState extends State<TodoList> {
  static const _appTitle = 'Todo List';
  final todos = <String>[];
  final controller = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: _appTitle,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(_appTitle),
        ),
        body: Column(
          children: [
            TextField(
              controller: controller,
            ),
            Expanded(
              child: ListView.builder(
                itemCount: todos.length,
                itemBuilder: (context, index) {
                  final todo = todos[index];

                  return Dismissible(
                    key: Key('$todo$index'),
                    onDismissed: (direction) => todos.removeAt(index),
                    background: Container(color: Colors.red),
                    child: ListTile(title: Text(todo)),
                  );
                },
              ),
            ),
          ],
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () {
            setState(() {
              todos.add(controller.text);
              controller.clear();
            });
          },
          child: const Icon(Icons.add),
        ),
      ),
    );
  }
}
```

### 2. Enter text in the text field

### 2. 在文字區輸入文字

Now that you have a todo app, begin writing the test.
Start by entering text into the `TextField`.

我們有了一個待辦清單項應用以後，就可以開始編寫測試使用案例了。
在本範例中，我們會先測試在文字區輸入文字。

Accomplish this task by:

完成這項任務，需要做到：

  1. Building the widget in the test environment.

     在測試環境建立 Widget

  2. Using the [`enterText()`][]
     method from the `WidgetTester`.

     使用 `WidgetTester` 中的 [`enterText()`][] 方法

<?code-excerpt "test/main_steps.dart (TestWidgetStep2)"?>
```dart
testWidgets('Add and remove a todo', (tester) async {
  // Build the widget
  await tester.pumpWidget(const TodoList());

  // Enter 'hi' into the TextField.
  await tester.enterText(find.byType(TextField), 'hi');
});
```

{{site.alert.note}}

  This recipe builds upon previous widget testing recipes.
  To learn the core concepts of widget testing,
  see the following recipes:

  這個章節的內容建立在前面的 widget 測試的相關章節上，
  請參考如下章節，獲取關於 Widget 測試的更多內容：

* [Introduction to widget testing][]

  [Widget 測試介紹][Introduction to widget testing]

* [Finding widgets in a widget test][]

  [定位到目標 Widgets][Finding widgets in a widget test][

{{site.alert.end}}

### 3. Ensure tapping a button adds the todo

### 3. 點選按鈕，增加待辦清單項

After entering text into the `TextField`, ensure that tapping
the `FloatingActionButton` adds the item to the list.

在 `TextField` 中輸入文字後，需要確保能夠點選 `FloatingActionButton`，
將文字作為清單項加入列表中。

This involves three steps:

這包含了三個步驟：

 1. Tap the add button using the [`tap()`][] method.

    使用 [`tap()`][] 方法模擬點選按鈕

 2. Rebuild the widget after the state has changed using the
    [`pump()`][] method.

    使用 [`pump()`][] 方法確保應用狀態發生改變時可以重建 widget

 3. Ensure that the list item appears on screen.

    確保列表清單項展現在螢幕上

<?code-excerpt "test/main_steps.dart (TestWidgetStep3)"?>
```dart
testWidgets('Add and remove a todo', (tester) async {
  // Enter text code...

  // Tap the add button.
  await tester.tap(find.byType(FloatingActionButton));

  // Rebuild the widget after the state has changed.
  await tester.pump();

  // Expect to find the item on screen.
  expect(find.text('hi'), findsOneWidget);
});
```

### 4. Ensure swipe-to-dismiss removes the todo

### 4. 滑動刪除待辦清單項

Finally, ensure that performing a swipe-to-dismiss action on the todo
item removes it from the list. This involves three steps:

最後，我們需要確保滑動刪除的操作能夠正常地從列表中移除清單項。
這包含了三個步驟：

  1. Use the [`drag()`][]
     method to perform a swipe-to-dismiss action.

     使用  [`drag()`][] 方法模擬滑動刪除操作。

  2. Use the [`pumpAndSettle()`][]
     method to continually rebuild the widget tree until the dismiss
     animation is complete.

     使用 [`pumpAndSettle()`][] 方法使 widget 樹保持重建更新，
     直到消除的動畫完成。

  3. Ensure that the item no longer appears on screen.

     確保上述清單項不會再出現在螢幕上

<?code-excerpt "test/main_steps.dart (TestWidgetStep4)"?>
```dart
testWidgets('Add and remove a todo', (tester) async {
  // Enter text and add the item...

  // Swipe the item to dismiss it.
  await tester.drag(find.byType(Dismissible), const Offset(500, 0));

  // Build the widget until the dismiss animation ends.
  await tester.pumpAndSettle();

  // Ensure that the item is no longer on screen.
  expect(find.text('hi'), findsNothing);
});
```

### Complete example

### 完整範例

<?code-excerpt "test/main_test.dart"?>
```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

void main() {
  testWidgets('Add and remove a todo', (tester) async {
    // Build the widget.
    await tester.pumpWidget(const TodoList());

    // Enter 'hi' into the TextField.
    await tester.enterText(find.byType(TextField), 'hi');

    // Tap the add button.
    await tester.tap(find.byType(FloatingActionButton));

    // Rebuild the widget with the new item.
    await tester.pump();

    // Expect to find the item on screen.
    expect(find.text('hi'), findsOneWidget);

    // Swipe the item to dismiss it.
    await tester.drag(find.byType(Dismissible), const Offset(500, 0));

    // Build the widget until the dismiss animation ends.
    await tester.pumpAndSettle();

    // Ensure that the item is no longer on screen.
    expect(find.text('hi'), findsNothing);
  });
}

class TodoList extends StatefulWidget {
  const TodoList({super.key});

  @override
  State<TodoList> createState() => _TodoListState();
}

class _TodoListState extends State<TodoList> {
  static const _appTitle = 'Todo List';
  final todos = <String>[];
  final controller = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: _appTitle,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(_appTitle),
        ),
        body: Column(
          children: [
            TextField(
              controller: controller,
            ),
            Expanded(
              child: ListView.builder(
                itemCount: todos.length,
                itemBuilder: (context, index) {
                  final todo = todos[index];

                  return Dismissible(
                    key: Key('$todo$index'),
                    onDismissed: (direction) => todos.removeAt(index),
                    background: Container(color: Colors.red),
                    child: ListTile(title: Text(todo)),
                  );
                },
              ),
            ),
          ],
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () {
            setState(() {
              todos.add(controller.text);
              controller.clear();
            });
          },
          child: const Icon(Icons.add),
        ),
      ),
    );
  }
}
```

[Create a basic list]: {{site.url}}/cookbook/lists/basic-list
[Create and style a text field]: {{site.url}}/cookbook/forms/text-input
[`drag()`]: {{api}}/flutter_test/WidgetController/drag.html
[`enterText()`]: {{api}}/flutter_test/WidgetTester/enterText.html
[Finding widgets in a widget test]: {{site.url}}/cookbook/testing/widget/finders
[Handle taps]: {{site.url}}/cookbook/gestures/handling-taps
[Implement swipe to dismiss]: {{site.url}}/cookbook/gestures/dismissible
[Introduction to widget testing]: {{site.url}}/cookbook/testing/widget/introduction
[`pump()`]: {{api}}/flutter_test/WidgetTester/pump.html
[`pumpAndSettle()`]: {{api}}/flutter_test/WidgetTester/pumpAndSettle.html
[`tap()`]: {{api}}/flutter_test/WidgetController/tap.html
[`TextField`]: {{api}}/material/TextField-class.html
[`WidgetTester`]: {{api}}/flutter_test/WidgetTester-class.html

