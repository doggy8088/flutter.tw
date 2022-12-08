---
title: Retrieve the value of a text field
title: 獲取文字框的輸入值
description: How to retrieve text from a text field.
description: 如何獲取文字框輸入的文字
prev:
  title: Create an expandable FAB
  title: 建立一個點選展開的 FAB
  path: /docs/cookbook/effects/expandable-fab
next:
  title: Focus and text fields
  title: Text Field 上的焦點
  path: /docs/cookbook/forms/focus
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/forms/retrieve_input"?>

{% comment %}
prev:
  title: Handle changes to a text field
  path: /docs/cookbook/forms/text-field-changes
{% endcomment %}

In this recipe,
learn how to retrieve the text a user has entered into a text field
using the following steps:

這個章節講解的是如何獲取文字框的輸入值。

## Directions

## 步驟

  1. Create a `TextEditingController`.
     
     建立一個  `TextEditingController`

  2. Supply the `TextEditingController` to a `TextField`.
     
     把 `TextEditingController` 應用到 `TextField` 上

  3. Display the current value of the text field.

     展示文字框當前值  

## 1. Create a `TextEditingController`

## 1. 建立 `TextEditingController`

To retrieve the text a user has entered into a text field,
create a [`TextEditingController`][]
and supply it to a `TextField` or `TextFormField`.

為了獲取文字框輸入值，需要建立一個 [`TextEditingController`][]。
在後續步驟中，這個 `TextEditingController` 將會被應用到 `TextField` 上。

Once a `TextEditingController` is supplied to a `TextField` or `TextFormField`,
we can use it to retrieve the text a user has typed into that text field.

`TextEditingController` 被應用於 `TextField` 或者 `TextFormField` 後，
就可以使用它來獲取文字框輸入值。

{{site.alert.secondary}}

  **Important:** Call `dispose` of the `TextEditingController` when
  you've finished using it. This ensures that you discard any resources
  used by the object.
  
  **記住:**當不再使用 `TextEditingController` 時，
  請銷燬它以確保相關的資源得到釋放。
  
{{site.alert.end}}

<?code-excerpt "lib/starter.dart (Starter)" remove="return Container();"?>
```dart
// Define a custom Form widget.
class MyCustomForm extends StatefulWidget {
  const MyCustomForm({super.key});

  @override
  State<MyCustomForm> createState() => _MyCustomFormState();
}

// Define a corresponding State class.
// This class holds the data related to the Form.
class _MyCustomFormState extends State<MyCustomForm> {
  // Create a text controller and use it to retrieve the current value
  // of the TextField.
  final myController = TextEditingController();

  @override
  void dispose() {
    // Clean up the controller when the widget is disposed.
    myController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // Fill this out in the next step.
  }
}
```

## 2. Supply the `TextEditingController` to a `TextField`

## 2. 把 `TextEditingController` 應用到 `TextField` 上

Now that you have a `TextEditingController`, wire it up
to a text field using the `controller` property:

建立完 `TextEditingController`，
就可以使用 `controller` 屬性完成 text field 繫結。

<?code-excerpt "lib/step2.dart (TextFieldController)"?>
```dart
return TextField(
  controller: myController,
);
```

## 3. Display the current value of the text field

## 3. 展示文字框當前值

After supplying the `TextEditingController` to the text field,
begin reading values. Use the [`text()`][]
method provided by the `TextEditingController` to retrieve the
String that the user has entered into the text field.

在 `TextEditingController` 作用於文字框後，就可以開始取值了。
透過 `TextEditingController` 提供的 [`text()`][] 方法，
就能夠獲取到文字框輸入值了。

The following code displays an alert dialog with the current
value of the text field when the user taps a floating action button.

在下面的範例中，使用者點選浮層按鈕，
將會觸發彈出一個對話方塊，對話方塊獲取並顯示文字框的當前值。

<?code-excerpt "lib/step3.dart (FloatingActionButton)" replace="/^floatingActionButton\: //g"?>
```dart
FloatingActionButton(
  // When the user presses the button, show an alert dialog containing
  // the text that the user has entered into the text field.
  onPressed: () {
    showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          // Retrieve the text that the user has entered by using the
          // TextEditingController.
          content: Text(myController.text),
        );
      },
    );
  },
  tooltip: 'Show me the value!',
  child: const Icon(Icons.text_fields),
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
// This class holds the data related to the Form.
class _MyCustomFormState extends State<MyCustomForm> {
  // Create a text controller and use it to retrieve the current value
  // of the TextField.
  final myController = TextEditingController();

  @override
  void dispose() {
    // Clean up the controller when the widget is disposed.
    myController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Retrieve Text Input'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: TextField(
          controller: myController,
        ),
      ),
      floatingActionButton: FloatingActionButton(
        // When the user presses the button, show an alert dialog containing
        // the text that the user has entered into the text field.
        onPressed: () {
          showDialog(
            context: context,
            builder: (context) {
              return AlertDialog(
                // Retrieve the text the that user has entered by using the
                // TextEditingController.
                content: Text(myController.text),
              );
            },
          );
        },
        tooltip: 'Show me the value!',
        child: const Icon(Icons.text_fields),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/retrieve-input.gif" alt="獲取文字輸入範例" class="site-mobile-screenshot" />
</noscript>


[`text()`]: {{site.api}}/flutter/widgets/TextEditingController/text.html
[`TextEditingController`]: {{site.api}}/flutter/widgets/TextEditingController-class.html
