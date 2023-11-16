---
title: Handle changes to a text field
title: 響應文字框內容的更改
description: How to detect changes to a text field.
description: 如何檢測文字框內容的更改。
tags: cookbook, 實用課程, 表格互動
keywords: 文字框,傳值
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/forms/text_field_changes/"?>

In some cases, it's useful to run a callback function every time the text
in a text field changes. For example, you might want to build a search
screen with autocomplete functionality where you want to update the
results as the user types.

在某些情境中，我們可能需要在每次文字框的文字內容變化時都呼叫回呼(Callback)函式。
例如，當建構一個有自動填充功能的搜尋頁面時，
我們希望根據使用者輸入的內容來更新返回的結果。

How do you run a callback function every time the text changes?
With Flutter, you have two options:

那麼如何每次在文字內容改變時呼叫回呼(Callback)函式呢？在Flutter中，我們提供了兩種選擇：

  1. Supply an `onChanged()` callback to a `TextField` or a `TextFormField`.

     給 `TextField` 或 `TextFormField` 繫結 `onChanged()` 回呼(Callback)

  2. Use a `TextEditingController`.

     使用 `TextEditingController`

## 1. Supply an `onChanged()` callback to a `TextField` or a `TextFormField`

## 1. 給 `TextField` 或 `TextFormField` 繫結 `onChanged()` 回呼(Callback)

The simplest approach is to supply an [`onChanged()`][] callback to a
[`TextField`][] or a [`TextFormField`][].
Whenever the text changes, the callback is invoked.

最簡單的方法是給 [`TextField`][] 繫結 [`onChanged()`][] 回呼(Callback)。
每當文字內容改變時，回呼(Callback)函式會被觸發。

In this example, print the current value and length of the text field 
to the console every time the text changes.

在下面的範例中，每次 text 的值改變，會在控制檯中打印出當前文字框的值。

It's important to use [characters][] when dealing with user input,
as text may contain complex characters.
This ensures that every character is counted correctly
as they appear to the user.

在處理使用者的輸入內容時，很重要的一點是要使用 [characters][]，
因為使用者的輸入內容可能會包含複雜的字元。
characters 會保證每一個顯示到使用者字元都被計數到。

<?code-excerpt "lib/main.dart (TextField1)"?>
```dart
TextField(
  onChanged: (text) {
    print('First text field: $text (${text.characters.length})');
  },
),
```

## 2. Use a `TextEditingController`

## 2. 使用 `TextEditingController`

A more powerful, but more elaborate approach, is to supply a
[`TextEditingController`][] as the [`controller`][]
property of the `TextField` or a `TextFormField`.

另外一種更強大但是更復雜的方法是繫結 [`TextEditingController`][] 作為 `TextField` 和
 `TextFormField` 的 [`controller`][] 屬性。

To be notified when the text changes, listen to the controller
using the [`addListener()`][] method using the following steps:

你可以透過如下步驟，使用 [`addListener()`][] 方法來監聽控制，
實現在文字更改時收到通知：

  1. Create a `TextEditingController`.

     建立一個 `TextEditingController`

  2. Connect the `TextEditingController` to a text field.

     將 `TextEditingController` 繫結到 text field

  3. Create a function to print the latest value.

     建立一個函式來列印最新值

  4. Listen to the controller for changes.

     監聽控制器的變化
    

### Create a `TextEditingController`

### 建立一個 `TextEditingController`

Create a `TextEditingController`:

建立一個 `TextEditingController`：


<?code-excerpt "lib/main_step1.dart (Step1)" remove="return Container();"?>
```dart
// Define a custom Form widget.
class MyCustomForm extends StatefulWidget {
  const MyCustomForm({super.key});

  @override
  State<MyCustomForm> createState() => _MyCustomFormState();
}

// Define a corresponding State class.
// This class holds data related to the Form.
class _MyCustomFormState extends State<MyCustomForm> {
  // Create a text controller. Later, use it to retrieve the
  // current value of the TextField.
  final myController = TextEditingController();

  @override
  void dispose() {
    // Clean up the controller when the widget is removed from the
    // widget tree.
    myController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // Fill this out in the next step.
  }
}
```

{{site.alert.note}}

  Remember to dispose of the `TextEditingController` when it's no
  longer needed. This ensures that you discard any resources used
  by the object.

  請在 `TextEditingController` 使用完畢時將其 `dispose` ，
  從而確保所有被這個物件所使用的資源被釋放。

{{site.alert.end}}


### Connect the `TextEditingController` to a text field

### 給 text field 繫結 `TextEditingController`

Supply the `TextEditingController` to either a `TextField`
or a `TextFormField`. Once you wire these two classes together,
you can begin listening for changes to the text field.

`TextEditingController` 必須繫結到 `TextField` 或者是 `TextFormField` 才能被正常的使用。
一旦繫結，就能夠開始監聽文字框的變化。

<?code-excerpt "lib/main.dart (TextField2)"?>
```dart
TextField(
  controller: myController,
),
```

### Create a function to print the latest value

### 建立一個列印當前值的方法

You need a function to run every time the text changes.
Create a method in the `_MyCustomFormState` class that prints
out the current value of the text field.

現在，我們需要一個每當表單項變化都會執行的函式。
在下面的範例中，我們會在 `_MyCustomFormState` 類中建立一個方法，
實現打印出文字框當前值。


<?code-excerpt "lib/main.dart (printLatestValue)"?>
```dart
void _printLatestValue() {
  final text = myController.text;
  print('Second text field: $text (${text.characters.length})');
}
```

### Listen to the controller for changes

### 監聽控制器的變化

Finally, listen to the `TextEditingController` and call the
`_printLatestValue()` method when the text changes. Use the
[`addListener()`][] method for this purpose.

最後，需要監聽 `TextEditingController` 並且在 text 值變化時執行
`_printLatestValue()` 方法。
我們需要使用 [`addListener()`][] 方法來實現這個功能。

Begin listening for changes when the
`_MyCustomFormState` class is initialized,
and stop listening when the `_MyCustomFormState` is disposed.

下面的範例會在類 `_MyCustomFormState` 初始化的時候開始監聽變化，dispose 時停止監聽。

<?code-excerpt "lib/main.dart (initState)"?>
```dart
@override
void initState() {
  super.initState();

  // Start listening to changes.
  myController.addListener(_printLatestValue);
}
```

<?code-excerpt "lib/main.dart (dispose)"?>
```dart
@override
void dispose() {
  // Clean up the controller when the widget is removed from the widget tree.
  // This also removes the _printLatestValue listener.
  myController.dispose();
  super.dispose();
}
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
      title: 'Retrieve Text Input',
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
// This class holds data related to the Form.
class _MyCustomFormState extends State<MyCustomForm> {
  // Create a text controller and use it to retrieve the current value
  // of the TextField.
  final myController = TextEditingController();

  @override
  void initState() {
    super.initState();

    // Start listening to changes.
    myController.addListener(_printLatestValue);
  }

  @override
  void dispose() {
    // Clean up the controller when the widget is removed from the widget tree.
    // This also removes the _printLatestValue listener.
    myController.dispose();
    super.dispose();
  }

  void _printLatestValue() {
    final text = myController.text;
    print('Second text field: $text (${text.characters.length})');
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Retrieve Text Input'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            TextField(
              onChanged: (text) {
                print('First text field: $text (${text.characters.length})');
              },
            ),
            TextField(
              controller: myController,
            ),
          ],
        ),
      ),
    );
  }
}
```

[`addListener()`]: {{site.api}}/flutter/foundation/ChangeNotifier/addListener.html
[`controller`]: {{site.api}}/flutter/material/TextField/controller.html
[`onChanged()`]: {{site.api}}/flutter/material/TextField/onChanged.html
[`TextField`]: {{site.api}}/flutter/material/TextField-class.html
[`TextEditingController`]: {{site.api}}/flutter/widgets/TextEditingController-class.html
[`TextFormField`]: {{site.api}}/flutter/material/TextFormField-class.html
[characters]: {{site.pub}}/packages/characters
