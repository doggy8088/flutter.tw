---
title: Build a form with validation
title: 建構一個有驗證判斷的表單
description: How to build a form that validates input.
description: 如何建立一個能夠驗證輸入內容的表單。
tags: cookbook, 實用課程, 表格互動
keywords: 文字框, 驗證
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/forms/validation"?>

Apps often require users to enter information into a text field.
For example, you might require users to log in with an email address
and password combination.

應用程式通常會要求使用者在文字框中輸入資訊。例如，我們可能正在開發一個應用程式，該應用程式就需要使用者輸入郵箱和密碼登入。

To make apps secure and easy to use, check whether the
information the user has provided is valid. If the user has correctly filled
out the form, process the information. If the user submits incorrect
information, display a friendly error message letting them know what went
wrong.

為了讓應用程式更為安全易用，我們通常都需要驗證使用者輸入的資訊是否有效。
如果使用者輸入了正確的資訊，就可以針對該資訊進行後續處理。
如果使用者輸入了錯誤的資訊，就需要在相關的輸入區域展示一條輸入資訊出錯的提示，以便使用者更正輸入。

In this example, learn how to add validation to a form that has
a single text field using the following steps:

你可以透過以下步驟，在下面的例子中學習如何為表單中的文字輸入框加入驗證判斷的功能：

  1. Create a `Form` with a `GlobalKey`.
     
     建立表單 `Form`，並以 `GlobalKey` 作為唯一性標識

  2. Add a `TextFormField` with validation logic.

     新增帶驗證邏輯的 `TextFormField` 到表單中

  3. Create a button to validate and submit the form.
     
     建立按鈕以驗證和提交表單

## 1. Create a `Form` with a `GlobalKey`

## 1. 建立表單 `Form`，並以 `GlobalKey` 作為唯一性標識

First, create a [`Form`][].
The `Form` widget acts as a container for grouping
and validating multiple form fields.

首先，我們需要建立一個表單元件 [`Form`][] 作為容器承載和驗證多個表單域。

When creating the form, provide a [`GlobalKey`][].
This uniquely identifies the `Form`,
and allows validation of the form in a later step.

當我們建立表單 `Form` 的時候，需要提供一個 [`GlobalKey`][]。
`GlobalKey` 唯一標識了這個表單 `Form`，
在後續的表單驗證步驟中，也起到了關鍵的作用。

<?code-excerpt "lib/form.dart"?>
```dart
import 'package:flutter/material.dart';

// Define a custom Form widget.
class MyCustomForm extends StatefulWidget {
  const MyCustomForm({super.key});

  @override
  MyCustomFormState createState() {
    return MyCustomFormState();
  }
}

// Define a corresponding State class.
// This class holds data related to the form.
class MyCustomFormState extends State<MyCustomForm> {
  // Create a global key that uniquely identifies the Form widget
  // and allows validation of the form.
  //
  // Note: This is a `GlobalKey<FormState>`,
  // not a GlobalKey<MyCustomFormState>.
  final _formKey = GlobalKey<FormState>();

  @override
  Widget build(BuildContext context) {
    // Build a Form widget using the _formKey created above.
    return Form(
      key: _formKey,
      child: const Column(
        children: <Widget>[
          // Add TextFormFields and ElevatedButton here.
        ],
      ),
    );
  }
}
```

{{site.alert.tip}}

  Using a `GlobalKey` is the recommended way to access a form.
  However, if you have a more complex widget tree,
  you can use the [`Form.of()`][] method to
  access the form within nested widgets.
  
  一般情況下，推薦使用 `GlobalKey` 來存取一個表單。
  巢狀(Nesting)元件且元件樹比較複雜的情況下，
  可以使用 [`Form.of()`][] 方法存取表單。

{{site.alert.end}}

## 2. Add a `TextFormField` with validation logic

## 2. 新增帶驗證邏輯的 `TextFormField` 到表單中

Although the `Form` is in place,
it doesn't have a way for users to enter text.
That's the job of a [`TextFormField`][].
The `TextFormField` widget renders a material design text field
and can display validation errors when they occur.

儘管在前面步驟中，已經創建出表單 `Form` 了，
但我們此時還需要提供一個 [`TextFormField`][] 讓使用者輸入文字資訊。
`TextFormField` 是遵循 material 設計風格的文字輸入框，
並且能夠在輸入驗證不透過時顯示錯誤提醒。

Validate the input by providing a `validator()` function to the
`TextFormField`. If the user's input isn't valid,
the `validator` function returns a `String` containing
an error message.
If there are no errors, the validator must return null.

透過給 `TextFormField` 加入 `validator()` 函式可以驗證輸入是否正確。
`validator` 函式會校驗使用者輸入的資訊，
如果資訊有誤，會返回包含出錯原因的字串 `String`。
如果資訊無誤，則不返回。

For this example, create a `validator` that ensures the
`TextFormField` isn't empty. If it is empty,
return a friendly error message.

在下面的例項中，我們會在 `TextFormField` 中加入一個 `validator` 驗證函式，
它的功能是判斷使用者輸入的文字是否為空，
如果為空，就返回「請輸入文字」的友情提示。

<?code-excerpt "lib/main.dart (TextFormField)"?>
```dart
TextFormField(
  // The validator receives the text that the user has entered.
  validator: (value) {
    if (value == null || value.isEmpty) {
      return 'Please enter some text';
    }
    return null;
  },
),
```

## 3. Create a button to validate and submit the form

## 3. 建立按鈕以驗證和提交表單

Now that you have a form with a text field,
provide a button that the user can tap to submit the information.

在建立完表單以及文字框後，還需要提供一個按鈕讓使用者提交表單。

When the user attempts to submit the form, check if the form is valid.
If it is, display a success message.
If it isn't (the text field has no content) display the error message.

當用戶提交表單後，我們會預先檢查表單資訊是否有效。
如果文字框有內容，表單有效，則會顯示正確資訊。
如果文字框沒有輸入任何內容，表單無效，會在文字框區域展示錯誤提示。

<?code-excerpt "lib/main.dart (ElevatedButton)" replace="/^child\: //g"?>
```dart
ElevatedButton(
  onPressed: () {
    // Validate returns true if the form is valid, or false otherwise.
    if (_formKey.currentState!.validate()) {
      // If the form is valid, display a snackbar. In the real world,
      // you'd often call a server or save the information in a database.
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Processing Data')),
      );
    }
  },
  child: const Text('Submit'),
),
```

### How does this work?

### 實現原理

To validate the form, use the `_formKey` created in
step 1. You can use the `_formKey.currentState()`
method to access the [`FormState`][],
which is automatically created by Flutter when building a `Form`.

為了驗證表單，我們需要使用到步驟 1 中的 `_formKey`。
使用 `_formKey.currentState()` 方法去存取 [`FormState`][]，
而 `FormState` 是在建立表單 `Form` 時 Flutter 自動產生的。

The `FormState` class contains the `validate()` method.
When the `validate()` method is called, it runs the `validator()`
function for each text field in the form.
If everything looks good, the `validate()` method returns `true`.
If any text field contains errors, the `validate()` method
rebuilds the form to display any error messages and returns `false`.

`FormState` 類包含了 `validate()` 方法。當 `validate()` 方法被呼叫的時候，
會遍歷執行表單中所有文字框的 `validator()` 函式。
如果所有 `validator()` 函式驗證都透過，`validate()` 方法返回 `true`。
如果有某個文字框驗證不透過，就會在那個文字框區域顯示錯誤提示，
同時 `validate()` 方法返回 `false`。

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
    const appTitle = 'Form Validation Demo';

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

// Create a Form widget.
class MyCustomForm extends StatefulWidget {
  const MyCustomForm({super.key});

  @override
  MyCustomFormState createState() {
    return MyCustomFormState();
  }
}

// Create a corresponding State class.
// This class holds data related to the form.
class MyCustomFormState extends State<MyCustomForm> {
  // Create a global key that uniquely identifies the Form widget
  // and allows validation of the form.
  //
  // Note: This is a GlobalKey<FormState>,
  // not a GlobalKey<MyCustomFormState>.
  final _formKey = GlobalKey<FormState>();

  @override
  Widget build(BuildContext context) {
    // Build a Form widget using the _formKey created above.
    return Form(
      key: _formKey,
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          TextFormField(
            // The validator receives the text that the user has entered.
            validator: (value) {
              if (value == null || value.isEmpty) {
                return 'Please enter some text';
              }
              return null;
            },
          ),
          Padding(
            padding: const EdgeInsets.symmetric(vertical: 16),
            child: ElevatedButton(
              onPressed: () {
                // Validate returns true if the form is valid, or false otherwise.
                if (_formKey.currentState!.validate()) {
                  // If the form is valid, display a snackbar. In the real world,
                  // you'd often call a server or save the information in a database.
                  ScaffoldMessenger.of(context).showSnackBar(
                    const SnackBar(content: Text('Processing Data')),
                  );
                }
              },
              child: const Text('Submit'),
            ),
          ),
        ],
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/form-validation.gif" alt="表單驗證範例" class="site-mobile-screenshot" />
</noscript>

To learn how to retrieve these values, check out the
[Retrieve the value of a text field][] recipe.

想要了解更多關於如何獲取這些值的內容，你可以參考
[獲取文字輸入框的值][Retrieve the value of a text field] 部分。

[Retrieve the value of a text field]: {{site.url}}/cookbook/forms/retrieve-input
[`Form`]: {{site.api}}/flutter/widgets/Form-class.html
[`Form.of()`]: {{site.api}}/flutter/widgets/Form/of.html
[`FormState`]: {{site.api}}/flutter/widgets/FormState-class.html
[`GlobalKey`]: {{site.api}}/flutter/widgets/GlobalKey-class.html
[`TextFormField`]: {{site.api}}/flutter/material/TextFormField-class.html