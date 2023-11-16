---
title: Create and style a text field
title: 文字框的建立和設定
description: How to implement a text field.
description: 如何實現一個文字框。
tags: cookbook, 實用課程, 表格互動
keywords: 文字框,實現,Flutter搜尋框
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/forms/text_input/"?>

Text fields allow users to type text into an app.
They are used to build forms,
send messages, create search experiences, and more.
In this recipe, explore how to create and style text fields.

文字框作為一個接收使用者輸入的元件，被廣泛應用於表單建構、即時通訊、搜尋等場景中。

Flutter provides two text fields:
[`TextField`][] and [`TextFormField`][].

Flutter 提供了兩個開箱即用的文字框元件：
[`TextField`][] 和 [`TextFormField`][]。

## `TextField`

## `文字框`

[`TextField`][] is the most commonly used text input widget.

[`TextField`][] 是最常用的文字輸入元件。

By default, a `TextField` is decorated with an underline.
You can add a label, icon, inline hint text, and error text by supplying an
[`InputDecoration`][] as the [`decoration`][]
property of the `TextField`.
To remove the decoration entirely (including the
underline and the space reserved for the label),
set the `decoration` to null.

`TextField` 元件的預設樣式是帶有下劃線的裝飾樣式。
如果需要自訂裝飾樣式（新增標籤、圖示、提示文字和錯誤文字），
可以將 [`InputDecoration`][] 應用到 `TextField` 的 [`decoration`][] 屬性上。
如果需要完全移除下劃線和標籤預留空間，可以將 `decoration` 屬性設定為 null。

<?code-excerpt "lib/main.dart (TextField)" replace="/^child\: //g"?>
```dart
TextField(
  decoration: InputDecoration(
    border: OutlineInputBorder(),
    hintText: 'Enter a search term',
  ),
),
```

To retrieve the value when it changes,
see the [Handle changes to a text field][] recipe.

要在內容發生變化時收到此內容，請參見 
[響應文字框內容的更改][Handle changes to a text field]。

## `TextFormField`

## `表單文字框`

[`TextFormField`][] wraps a `TextField` and integrates it
with the enclosing [`Form`][].
This provides additional functionality,
such as validation and integration with other
[`FormField`][] widgets.

[`TextFormField`][] 內部封裝了一個 `TextField`
並被整合在表單元件 [`Form`][] 中。
如果需要對文字輸入進行驗證或者需要與其他表單元件 [`FormField`][] 互動聯動，
可以考慮使用 `TextFormField`。

<?code-excerpt "lib/main.dart (TextFormField)" replace="/^child\: //g"?>
```dart
TextFormField(
  decoration: const InputDecoration(
    border: UnderlineInputBorder(),
    labelText: 'Enter your username',
  ),
),
```

## Interactive example

<?code-excerpt "lib/main.dart" replace="/^child\: //g"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    const appTitle = 'Form Styling Demo';
    return MaterialApp(
      title: appTitle,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(appTitle),
        ),
        body: const MyCustomForm(),
      ),
    );
  }
}

class MyCustomForm extends StatelessWidget {
  const MyCustomForm({super.key});

  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: <Widget>[
        const Padding(
          padding: EdgeInsets.symmetric(horizontal: 8, vertical: 16),
          child: TextField(
            decoration: InputDecoration(
              border: OutlineInputBorder(),
              hintText: 'Enter a search term',
            ),
          ),
        ),
        Padding(
          padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 16),
          child: TextFormField(
            decoration: const InputDecoration(
              border: UnderlineInputBorder(),
              labelText: 'Enter your username',
            ),
          ),
        ),
      ],
    );
  }
}
```

For more information on input validation, see the
[Building a form with validation][] recipe.

檢視章節 [建構一個有驗證判斷的表單][] 獲取更多關於輸入驗證的內容。

[Building a form with validation]: {{site.url}}/cookbook/forms/validation/
[建構一個有驗證判斷的表單]: {{site.url}}/cookbook/forms/validation/
[`decoration`]: {{site.api}}/flutter/material/TextField/decoration.html
[`Form`]: {{site.api}}/flutter/widgets/Form-class.html
[`FormField`]: {{site.api}}/flutter/widgets/FormField-class.html
[Handle changes to a text field]: {{site.url}}/cookbook/forms/text-field-changes/
[`InputDecoration`]: {{site.api}}/flutter/material/InputDecoration-class.html
[`TextField`]: {{site.api}}/flutter/material/TextField-class.html
[`TextFormField`]: {{site.api}}/flutter/material/TextFormField-class.html
