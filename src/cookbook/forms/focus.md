---
title: Focus and text fields
title: 焦點和文字框
description: How focus works with text fields.
description: 文字框的聚焦是如何工作的。
tags: cookbook, 實用課程, 表格互動
keywords: Flutter輸入框
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/forms/focus/"?>

When a text field is selected and accepting input,
it is said to have "focus."
Generally, users shift focus to a text field by tapping,
and developers shift focus to a text field programmatically by
using the tools described in this recipe.

當一個文字框（輸入框）被選中並接受輸入時，
被稱為獲得了“焦點”。通常情況下，使用者能夠透過點選文字框以使其聚焦，
開發人員也可以使用本文所描述的方法來讓文字框得到焦點。

Managing focus is a fundamental tool for creating forms with an intuitive
flow. For example, say you have a search screen with a text field.
When the user navigates to the search screen,
you can set the focus to the text field for the search term.
This allows the user to start typing as soon as the screen
is visible, without needing to manually tap the text field.

管理焦點是一種直觀地建立表單流程的基本方法。
例如，假設我們有一個帶有文字框的搜尋頁面。當用戶導航到搜尋頁面時，
我們可以聚焦文字框的搜尋項。
這將允許使用者在搜尋頁面可見時能夠立即開始輸入，而無需手動點選文字框。

In this recipe, learn how to give the focus
to a text field as soon as it's visible,
as well as how to give focus to a text field
when a button is tapped.

在本文中，我們將學習如何聚焦到文字框上，以及點選按鈕時聚焦文字框。

## Focus a text field as soon as it's visible

## 一旦文字框可見，就將其聚焦

To give focus to a text field as soon as it's visible,
use the `autofocus` property.

為了在文字框可見時將其聚焦，我們可以使用 `autofocus` 屬性。

```dart
TextField(
  autofocus: true,
);
```

For more information on handling input and creating text fields,
see the [Forms][] section of the cookbook.

有關處理輸入和建立文字框的更多資訊，請參閱
[實用課程的 Forms 部分]({{site.url}}/cookbook#forms)。

## Focus a text field when a button is tapped

## 點選按鈕時聚焦文字框

Rather than immediately shifting focus to a specific text field,
you might need to give focus to a text field at a later point in time.
In the real world, you might also need to give focus to a specific
text field in response to an API call or a validation error.
In this example, give focus to a text field after the user
presses a button using the following steps:

我們也可能需要在之後的某個時間點聚焦特定的文字框，而不是立即聚焦它。
在這個例子中，我們將看到在使用者按下按鈕後如何聚焦文字框。
在實際開發中，您還可能需要聚焦特定的文字框以響應 api 呼叫或錯誤校驗。

  1. Create a `FocusNode`.

     建立一個 `FocusNode`

  2. Pass the `FocusNode` to a `TextField`.

     將 `FocusNode` 傳遞給 `TextField`   

  3. Give focus to the `TextField` when a button is tapped.

     透過點選按鈕聚焦 `TextField`

### 1. Create a `FocusNode`

### 1. 建立一個 `FocusNode`

First, create a [`FocusNode`][].
Use the `FocusNode` to identify a specific `TextField` in Flutter's
"focus tree." This allows you to give focus to the `TextField`
in the next steps.

首先，我們需要建立一個 [`FocusNode`][]。
我們將使用 `FocusNode` 來識別 Flutter 的“focus tree”中的特定的 `TextField`。
這將允許我們能夠在接下來的步驟中聚焦 `TextField`。

Since focus nodes are long-lived objects, manage the lifecycle
using a `State` object. Use the following instructions to create
a `FocusNode` instance inside the `initState()` method of a
`State` class, and clean it up in the `dispose()` method:

由於 focus node 是長壽命物件，我們需要使用 `State` 類來管理生命週期。
為此，需要在 `State` 類別的 `initState` 方法中建立 `FocusNode` 例項，
並在 `dispose` 方法中清除它們。

<?code-excerpt "lib/starter.dart (Starter)" remove="return Container();"?>
```dart
// Define a custom Form widget.
class MyCustomForm extends StatefulWidget {
  const MyCustomForm({super.key});

  @override
  State<MyCustomForm> createState() => _MyCustomFormState();
}

// Define a corresponding State class.
// This class holds data related to the form.
class _MyCustomFormState extends State<MyCustomForm> {
  // Define the focus node. To manage the lifecycle, create the FocusNode in
  // the initState method, and clean it up in the dispose method.
  late FocusNode myFocusNode;

  @override
  void initState() {
    super.initState();

    myFocusNode = FocusNode();
  }

  @override
  void dispose() {
    // Clean up the focus node when the Form is disposed.
    myFocusNode.dispose();

    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // Fill this out in the next step.
  }
}
```

### 2. Pass the `FocusNode` to a `TextField`

### 2. 將 `FocusNode` 傳遞給 `TextField` 

Now that you have a `FocusNode`,
pass it to a specific `TextField` in the `build()` method.

現在已經有了 `FocusNode`，我們可以將這個 `TextField` 傳遞給 `build()` 方法。

<?code-excerpt "lib/step2.dart (Build)"?>
```dart
@override
Widget build(BuildContext context) {
  return TextField(
    focusNode: myFocusNode,
  );
}
```

### 3. Give focus to the `TextField` when a button is tapped

### 3. 透過點選按鈕聚焦 `TextField`

Finally, focus the text field when the user taps a floating
action button. Use the [`requestFocus()`][] method to perform
this task.

最後，當用戶點選 floating action button 時，我們將要聚焦文字框！
為此我們將要使用 [`requestFocus()`][] 方法來完成此操作。

<?code-excerpt "lib/step3.dart (FloatingActionButton)" replace="/^floatingActionButton\: //g"?>
```dart
FloatingActionButton(
  // When the button is pressed,
  // give focus to the text field using myFocusNode.
  onPressed: () => myFocusNode.requestFocus(),
),
```

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Text Field Focus',
      home: MyCustomForm(),
    );
  }
}

// Define a custom Form widget.
class MyCustomForm extends StatefulWidget {
  const MyCustomForm({super.key});

  @override
  State<MyCustomForm> createState() => _MyCustomFormState();
}

// Define a corresponding State class.
// This class holds data related to the form.
class _MyCustomFormState extends State<MyCustomForm> {
  // Define the focus node. To manage the lifecycle, create the FocusNode in
  // the initState method, and clean it up in the dispose method.
  late FocusNode myFocusNode;

  @override
  void initState() {
    super.initState();

    myFocusNode = FocusNode();
  }

  @override
  void dispose() {
    // Clean up the focus node when the Form is disposed.
    myFocusNode.dispose();

    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Text Field Focus'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            // The first text field is focused on as soon as the app starts.
            const TextField(
              autofocus: true,
            ),
            // The second text field is focused on when a user taps the
            // FloatingActionButton.
            TextField(
              focusNode: myFocusNode,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        // When the button is pressed,
        // give focus to the text field using myFocusNode.
        onPressed: () => myFocusNode.requestFocus(),
        tooltip: 'Focus Second Text Field',
        child: const Icon(Icons.edit),
      ), // This trailing comma makes auto-formatting nicer for build methods.
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/focus.gif" alt="Text Field Focus Demo" class="site-mobile-screenshot" />
</noscript>


[fix has landed]: {{site.repo.flutter}}/pull/50372
[`FocusNode`]: {{site.api}}/flutter/widgets/FocusNode-class.html
[Forms]: {{site.url}}/cookbook#forms
[flutter/flutter@bf551a3]: {{site.repo.flutter}}/commit/bf551a31fe7ef45c854a219686b6837400bfd94c
[Issue 52221]: {{site.repo.flutter}}/issues/52221
[`requestFocus()`]: {{site.api}}/flutter/widgets/FocusNode/requestFocus.html
[workaround]: {{site.repo.flutter}}/issues/52221#issuecomment-598244655
