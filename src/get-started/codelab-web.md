---
title: Write your first Flutter app on the web
title: 編寫你的第一個 Flutter 網頁應用
description: How to create a Flutter web app.
description: 如何建立一個 Flutter 網頁應用。
short-title: Write your first web app
short-title: 編寫你的第一個網頁應用
tags: Flutter安裝,Flutter起步課程
keywords: Flutter Web應用,DartPad,線上課程,零基礎
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="get-started/codelab_web"?>

{{site.alert.tip}}

  This codelab walks you through writing
  your first Flutter app on the web, specifically. 
  You might prefer to try
  [another codelab][first_flutter_codelab] 
  that takes a more generic approach.
  Note that the codelab on this page
  does work on mobile and desktop
  once you download and configure the appropriate tooling.

  這個 codelab 將引導你初步體驗 Flutter 網頁應用開發。
  當然你可能更想去嘗試 [編寫你的第一個 Flutter 應用][first_flutter_codelab]。
  需要注意的是，在一切工具順利安裝的基礎上，
  本頁面上的 codelab 將可以在移動端和桌面端的網頁瀏覽器裡執行。
{{site.alert.end}}

<img src="/assets/images/docs/get-started/sign-up.gif" alt="The web app that you'll be building" class='site-image-right'>

This is a guide to creating your first Flutter **web** app.
If you are familiar with object-oriented programming,
and concepts such as variables, loops, and conditionals,
you can complete this tutorial.
You don't need previous experience with Dart,
mobile, or web programming.

本課程可以幫助你你完成第一個 Flutter Web 應用，
如果你熟悉面對物件、變數、迴圈以及條件判斷等概念，就可以完成本課程，
而無需要 Dart、行動開發和 Web 開發經驗。

## What you'll build
{:.no_toc}

## 內容概覽
{:.no_toc}

You'll implement a simple web app that displays a sign in screen.
The screen contains three text fields:  first name,
last name, and username. As the user fills out the fields,
a progress bar animates along the top of the sign in area.
When all three fields are filled in, the progress bar displays
in green along the full width of the sign in area,
and the **Sign up** button becomes enabled.
Clicking the **Sign up** button causes a welcome screen
to animate in from the bottom of the screen.

你將實現一個只顯示登入頁面的簡單 Web 應用，
這個頁面包含了三個文字輸入框：名字、姓氏和使用者名稱。
當用戶向輸入框輸入內容時，在登入區域頂部顯示一個進度條動畫效果。
當用戶完成輸入時，綠色的進度條將會跟隨著充滿整個登入區域的頂部，
而且 **Sign up** 按鈕狀態變成可點選，
點選 **Sign up** 按鈕從螢幕下方彈出一個歡迎頁面。

The animated GIF shows how the app works at the completion of this lab.

右側的動畫展示了完成該課程後程序的執行效果。

{{site.alert.secondary}}

  <h4 class="no_toc">What you'll learn</h4>

  <h4 class="no_toc">你將學到以下內容：</h4>

  * How to write a Flutter app that looks natural on the web.

    如何使用 Flutter 建構一個原始的 Web 程式。

  * Basic structure of a Flutter app.

    Flutter 程式的基本結構。

  * How to implement a Tween animation.

    如何實現一個補間 (Tween) 動畫。

  * How to implement a stateful widget.

    如何實現一個有狀態 (Stateful) widget 。

  * How to use the debugger to set breakpoints.

    如何使用斷點除錯程式。

{{site.alert.end}}

{{site.alert.secondary}}

  <h4 class="no_toc">What you'll use</h4>

  <h4 class="no_toc">你將用到：</h4>

  You need three pieces of software to complete this lab:

  我們需要下面三個軟體來實現該課程：

  * [Flutter SDK][]
  * [Chrome browser][]

    [Chrome 瀏覽器][Chrome browser]

  * [Text editor or IDE][editor]

    [文字編輯器 或 IDE][editor]

  While developing, run your web app in Chrome
  so you can debug with Dart DevTools.

  在開發過程中，你需要將你的應用在 Chrome 瀏覽器中執行，
  以便使用 Dart DevTools 進行除錯。

{{site.alert.end}}

## Step 0: Get the starter web app

## 第 0 步: 建立初始化 Web 應用

You'll start with a simple web app that we provide for you.

你將從我們為你提供的簡單 Web 應用開始學習。

<ol markdown="1">
<li markdown="1">Enable web development.<br>
啟用 Web 開發。<br>

At the command line, perform the following command to
make sure that you have Flutter installed correctly. 

在命令列觀察輸出內容，你應該可以看到如下類似的內容，
說明 Flutter 安裝的沒問題：

```terminal
$ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel master, 3.4.0-19.0.pre.254, on macOS 12.6 21G115
    darwin-arm64, locale en)
[✓] Android toolchain - develop for Android devices (Android SDK version 33.0.0)
[✓] Xcode - develop for iOS and macOS (Xcode 14.0)
[✓] Chrome - develop for the web
[✓] Android Studio (version 2021.2)
[✓] VS Code (version 1.71.1)
[✓] Connected device (4 available)
[✓] HTTP Host Availability

• No issues found!
```

If you see "flutter: command not found",
then make sure that you have installed the
[Flutter SDK][] and that it's in your path.

如果你看到提示是 "flutter: command not found"，
那麼就需要確保 [Flutter SDK][] 已經正確地安裝，
並且在環境變數中做好了配置。

It's okay if the Android toolchain, Android Studio,
and the Xcode tools aren't installed,
since the app is intended for the web only.
If you later want this app to work on mobile,
you'll need to do additional installation and setup.

如上所示，顯示我們缺少 Android 工具、Android Studio 和 Xcode，
如果我們只用於 Web 開發，這些都不是必要的。
後續如果你想用於移動端開發，你將需要安裝配置這些工具。

</li>

<li markdown="1">List the devices.<br>
To ensure that web _is_ installed,
list the devices available.
You should see something like the following:

查詢裝置列表。<br>
透過查詢裝置列表來驗證已支援 Web 開發。
你將看到如下的類似內容：

``` terminal
$ flutter devices
4 connected devices:

sdk gphone64 arm64 (mobile) • emulator-5554                        •
android-arm64  • Android 13 (API 33) (emulator)
iPhone 14 Pro Max (mobile)  • 45A72BE1-2D4E-4202-9BB3-D6AE2601BEF8 • ios
• com.apple.CoreSimulator.SimRuntime.iOS-16-0 (simulator)
macOS (desktop)             • macos                                •
darwin-arm64   • macOS 12.6 21G115 darwin-arm64
Chrome (web)                • chrome                               •
web-javascript • Google Chrome 105.0.5195.125

```

The **Chrome** device automatically starts Chrome and enables the use 
of the Flutter DevTools tooling.

**Chrome** 瀏覽器會自動啟動並啟用 Flutter 開發者工具。
</li>

<li markdown="1"><t>The starting app is displayed in the following DartPad.</t><t>執行程式將在 DartPad 中顯示。</t>

<?code-excerpt "lib/starter.dart" replace="/\/\* //g;/ \*\///g;/_ignore_for_file/ignore_for_file/g"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-starting_code
{$ begin main.dart $}
import 'package:flutter/material.dart';

void main() => runApp(const SignUpApp());

class SignUpApp extends StatelessWidget {
  const SignUpApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      routes: {
        '/': (context) => const SignUpScreen(),
      },
    );
  }
}

class SignUpScreen extends StatelessWidget {
  const SignUpScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.grey[200],
      body: const Center(
        child: SizedBox(
          width: 400,
          child: Card(
            child: SignUpForm(),
          ),
        ),
      ),
    );
  }
}

class SignUpForm extends StatefulWidget {
  const SignUpForm({super.key});

  @override
  State<SignUpForm> createState() => _SignUpFormState();
}

class _SignUpFormState extends State<SignUpForm> {
  final _firstNameTextController = TextEditingController();
  final _lastNameTextController = TextEditingController();
  final _usernameTextController = TextEditingController();

  double _formProgress = 0;

  @override
  Widget build(BuildContext context) {
    return Form(
      child: Column(
        mainAxisSize: MainAxisSize.min,
        children: [
          LinearProgressIndicator(value: _formProgress),
          Text('Sign up', style: Theme.of(context).textTheme.headlineMedium),
          Padding(
            padding: const EdgeInsets.all(8),
            child: TextFormField(
              controller: _firstNameTextController,
              decoration: const InputDecoration(hintText: 'First name'),
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(8),
            child: TextFormField(
              controller: _lastNameTextController,
              decoration: const InputDecoration(hintText: 'Last name'),
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(8),
            child: TextFormField(
              controller: _usernameTextController,
              decoration: const InputDecoration(hintText: 'Username'),
            ),
          ),
          TextButton(
            style: ButtonStyle(
              foregroundColor: MaterialStateProperty.resolveWith((states) {
                return states.contains(MaterialState.disabled)
                    ? null
                    : Colors.white;
              }),
              backgroundColor: MaterialStateProperty.resolveWith((states) {
                return states.contains(MaterialState.disabled)
                    ? null
                    : Colors.blue;
              }),
            ),
            onPressed: null,
            child: const Text('Sign up'),
          ),
        ],
      ),
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
// Avoid warning on "double _formProgress = 0;"
//ignore_for_file: prefer_final_fields
{$ end test.dart $}
```

{{site.alert.important}}

  This page uses an embedded version of [DartPad][]
  to display examples and exercises.
  If you see empty boxes instead of DartPads,
  go to the [DartPad troubleshooting page][].

  本頁面使用了嵌入式 [DartPad] 來顯示和練習範例。
  如果你看到的是空白頁面，請轉到 [DartPad troubleshooting page][]

{{site.alert.end}}
</li>

<li markdown="1">Run the example.<br>
Click the **Run** button to run the example.
Note that you can type into the text fields,
but the **Sign up** button is disabled.

執行程式碼範例。<br>
點選 **Run** 按鈕來執行範例程式碼。
你就可以在文字框中輸入內容，但是 **Sign up** 按鈕是禁用狀態的。

</li>

<li markdown="1">Copy the code.<br>
Click the clipboard icon in the upper right of the
code pane to copy the Dart code to your clipboard.

複製程式碼。<br>
點選程式碼區域右上角的複製圖示複製 Dart 程式碼。

</li>

<li markdown="1">Create a new Flutter project.<br>
From your IDE, editor, or at the command line,
[create a new Flutter project][] and name it `signin_example`.

建立一個新的 Flutter 工程。<br>
使用 IDE、編輯器或者命令列，建立一個名稱為 `signin_example` 的新專案，
更多內容可以參考文件 [Flutter 開發體驗初探][create a new Flutter project]。

</li>

<li markdown="1">Replace the contents of `lib/main.dart`
                 with the contents of the clipboard.<br>

使用上面我們複製的內容替換 `lib/main.dart` 檔案的內容。<br>

</li>
</ol>

### Observations
{:.no_toc}

### 觀察和分析
{:.no_toc}

* The entire code for this example lives in the
  `lib/main.dart` file.

  完整的範例程式碼都位於 `lib/main.dart` 檔案中。

* If you know Java, the Dart language should feel very familiar.

  如果你瞭解 Java ，那 Dart 也會給你一種熟悉的感覺。

* All of the app's UI is created in Dart code.
  For more information, see [Introduction to declarative UI][].

  應用程式的所有的 UI 的都是透過 Dart 建構的。
  你可以透過文件
  [宣告式 UI 介紹][Introduction to declarative UI] 瞭解到更多的資訊。

* The app's UI adheres to [Material Design][],
  a visual design language that runs on any device or platform.
  You can customize the Material Design widgets,
  but if you prefer something else,
  Flutter also offers the Cupertino widget library,
  which implements the current iOS design language.
  Or you can create your own custom widget library.

  應用的 UI 遵循 [Material Design][] 的設計規範，
  這是一種在任何裝置和平台都可以執行的視覺化設計語言。
  而且你也有其他選擇，Flutter 也提供了一款 iOS 設計風格的 Cupertino widget 庫。
  當然你也可以建立自己的自訂 widget 庫。

* In Flutter, almost everything is a [Widget][].
  Even the app itself is a widget.
  The app's UI can be described as a widget tree.

  在 Flutter 的世界，萬物皆 [Widget][]，甚至連應用本身都是 widget。
  應用的 UI 可以看作為 widget 樹。

## Step 1: Show the Welcome screen

## 第 1 步：顯示歡迎頁面

The `SignUpForm` class is a stateful widget.
This simply means that the widget stores information
that can change, such as user input, or data from a feed.
Since widgets themselves are immutable
(can't be modified once created),
Flutter stores state information in a companion class,
called the `State` class. In this lab,
all of your edits will be made to the private
`_SignUpFormState` class.

`SignUpForm` 類是一個 Stateful widget。這代表著 widget 的儲存資訊可動態改變，
例如使用者輸入，或者傳遞的資料。由於 widget 本身是不可變的（一旦建立不可修改），
所有 Flutter 的狀態資訊儲存在一種叫 `State` 的附加類中。
在這個程式碼範例中，所有的編輯將在一個 `_SignUpFormState` 的私有類中實現。

{{site.alert.secondary}}

  <h4 class="no_toc">Fun fact</h4>
The Dart compiler enforces privacy for any identifier
prefixed with an underscore. For more information,
see the [Effective Dart Style Guide][].

  <h4 class="no_toc">有趣的事</h4>
Dart 編譯器會將任何帶有下劃線字首標識的視為私有。可查閱
[Dart 文件 —— 高效 Dart 語言指南：程式碼風格][Effective Dart Style Guide]
獲取更多資訊。

{{site.alert.end}}

First, in your `lib/main.dart` file,
add the following class definition for the
`WelcomeScreen` widget after the `SignUpScreen` class:

首先，在 `lib/main.dart` 檔案中，在 `SignUpScreen` 類後面
新增下面 `WelcomeScreen` widget 的定義類：

<?code-excerpt "lib/step1.dart (WelcomeScreen)"?>
```dart
class WelcomeScreen extends StatelessWidget {
  const WelcomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text(
          'Welcome!',
          style: Theme.of(context).textTheme.displayMedium,
        ),
      ),
    );
  }
}
```

Next, you will enable the button to display the screen
and create a method to display it.

接下來，你需要建立一個顯示方法，然後使用按鈕透過方法控制頁面的顯示。

<ol markdown="1">

<li markdown="1"> Locate the `build()` method for the
`_SignUpFormState` class. This is the part of the code
that builds the SignUp button.
Notice how the button is defined:
It's a `TextButton` with a blue background,
white text that says **Sign up** and, when pressed,
does nothing.

找到 `_SignUpFormState` 類別的 `build()` 方法。
這部分程式碼是用來建構註冊按鈕的。注意，按鈕是如何定義：
它是一個背景為藍色， **Sign up** 文字為白色的 `TextButton` 按鈕，
當我們點選它時，並未執行任何操作。

</li>

<li markdown="1">Update the `onPressed` property.<br>
Change the `onPressed` property to call the (non-existent)
method that will display the welcome screen.

修改按鈕的 `onPressed` 屬性。<br>
將按鈕的 `onPressed` 屬性改為呼叫顯示歡迎頁面的方法（該方法在下一步建立）。

Change `onPressed: null` to the following:

將 `onPressed: null` 改為以下內容：

<?code-excerpt "lib/step1.dart (onPressed)"?>
```dart
onPressed: _showWelcomeScreen,
```
</li>

<li markdown="1">Add the `_showWelcomeScreen` method.<br>
Fix the error reported by the analyzer that `_showWelcomeScreen`
is not defined. Directly above the `build()` method,
add the following function:

新增 `_showWelcomeScreen` 方法。<br>
修復上述程式碼導致的編譯器提示錯誤：
`_showWelcomeScreen` is not defined.
（未定義 `_showWelcomeScreen`）。
在 `build()` 方法上方新增下面的方法：

<?code-excerpt "lib/step1.dart (showWelcomeScreen)"?>
```dart
void _showWelcomeScreen() {
  Navigator.of(context).pushNamed('/welcome');
}
```
</li>

<li markdown="1">Add the `/welcome` route.<br>
Create the connection to show the new screen.
In the `build()` method for `SignUpApp`,
add the following route below `'/'`:

新增 `/welcome` 頁面路由。<br>
為新的頁面新增跳轉路由。在 `SignUpApp` 類別的 `build()` 方法中，
在 `'/'` 下面新增如下路由：

<?code-excerpt "lib/step1.dart (WelcomeRoute)"?>
```dart
'/welcome': (context) => const WelcomeScreen(),
```
</li>

<li markdown="1">Run the app.<br>
The **Sign up** button should now be enabled.
Click it to bring up the welcome screen.
Note how it animates in from the bottom.
You get that behavior for free.

執行該應用程式。<br>
**Sign up** 按鈕現在應該可以點選了。
單擊註冊按鈕跳轉到歡迎頁面。
注意，歡迎頁面顯示是有一個從底部彈出的動畫。
你可以很簡單的實現它。

</li>

</ol>

### Observations
{:.no_toc}

### 觀察和分析
{:.no_toc}

* The `_showWelcomeScreen()` function is used in the `build()`
  method as a callback function. Callback functions are often
  used in Dart code and, in this case, this means
  "call this method when the button is pressed".

  `_showWelcomeScreen()` 函式被當成回呼(Callback)函式在 `build()` 方法中被呼叫。
  在 Dart 中你會經常使用回呼(Callback)函式，在這裡意味著“點選按鈕時呼叫該方法”。

* The `const` keyword in front of the constructor is very
  important. When Flutter encounters a constant widget, it
  short-circuits most of the rebuilding work under the hood 
  making the rendering more efficient.

  建構函式前面的 `const` 關鍵字至關重要，
  當 Flutter 遇到一個靜態 widget 時，
  它就會縮短引擎下的大部分重建工作，從而提高渲染效率。

* Flutter has only one `Navigator` object.
  This widget manages Flutter's screens
  (also called _routes_ or _pages_) inside a stack.
  The screen at the top of the stack is the view that
  is currently displayed. Pushing a new screen to this
  stack switches the display to that new screen.
  This is why the `_showWelcomeScreen` function pushes
  the `WelcomeScreen` onto the Navigator's stack.
  The user clicks the button and, voila,
  the welcome screen appears. Likewise,
  calling `pop()` on the `Navigator` returns to the
  previous screen. Because Flutter's navigation is
  integrated into the browser's navigation,
  this happens implicitly when clicking the browser's
  back arrow button.

  Flutter 中僅存在一個 `Navigator` 物件。
  這個 widget 用來管理 Flutter 堆疊中的頁面
  （也可以被稱為路由 (**routes**) 或者頁面管理器 (**pages**)）。
  當前顯示的頁面是堆疊中最上面的頁面，
  透過往堆疊中 push 新的頁面來切換新的頁面。
  這也是 `_showWelcomeScreen` 函式向
  `Navigator` 堆疊中新增 `WelcomeScreen` 頁面的原因。
  使用者點選按鈕，然後出現歡迎頁面。
  同樣，可以透過呼叫 `Navigator` 的 `pop()` 方法來返回上一個頁面。
  因為 Flutter 的 navigation 已經整合到瀏覽器的導航中，
  所以當點選瀏覽器的返回箭頭也會返回到上一個頁面。

## Step 2: Enable sign in progress tracking

## 第 2 步： 實現輸入進度監聽

This sign in screen has three fields.
Next, you will enable the ability to track the
user's progress on filling in the form fields,
and update the app's UI when the form is complete.

在這個頁面有三個文字框。
下一步，我們將實現監聽使用者輸入表單的進度，
並且在表單完成後更新應用的 UI 。

{{site.alert.note}}

  This example does **not** validate the accuracy of the user input.
  That is something you can add later using form validation, if you like.

  這個簡單的範例並未對使用者的輸入進行準確性驗證。
  如果需要，你可以後面自己新增表單驗證。

{{site.alert.end}}

<ol markdown="1">
<li markdown="1">Add a method to update `_formProgress`.
In the `_SignUpFormState` class, add a new method called
`_updateFormProgress()`:

新增一個用於更新進度 `_formProgress` 屬性的方法。
在 `_SignUpFormState` 類，新增一個名為 `_updateFormProgress()` 的新方法：

<?code-excerpt "lib/step2.dart (updateFormProgress)"?>
```dart
void _updateFormProgress() {
  var progress = 0.0;
  final controllers = [
    _firstNameTextController,
    _lastNameTextController,
    _usernameTextController
  ];

  for (final controller in controllers) {
    if (controller.value.text.isNotEmpty) {
      progress += 1 / controllers.length;
    }
  }

  setState(() {
    _formProgress = progress;
  });
}
```

This method updates the `_formProgress` field based on the
the number of non-empty text fields.

這個方法根據非空輸入框的數量來更新 `_formProgress` 屬性。

</li>

<li markdown="1">Call `_updateFormProgress` when the form
changes.<br>
In the `build()` method of the `_SignUpFormState` class,
add a callback to the `Form` widget's `onChanged` argument.
Add the code below marked as NEW:

表單改變時呼叫 `_updateFormProgress` 方法。<br>
在 `_SignUpFormState` 類別的 `build()` 方法中，
為 `Form` widget 的 `onChanged` 引數添加回調函式。
注意註釋為 NEW 的那行新新增的程式碼：

<?code-excerpt "lib/step2.dart (onChanged)"?>
```dart
return Form(
  onChanged: _updateFormProgress, // NEW
  child: Column(
```
</li>

<li markdown="1">Update the `onPressed` property (again).<br>
In `step 1`, you modified the `onPressed` property for the
**Sign up** button to display the welcome screen.
Now, update that button to display the welcome
screen only when the form is completely filled in:

再次更改按鈕的 `onPressed` 屬性。<br>
還記得我們在第一步中，我們透過修改 `onPressed` 屬性實現了
點選 **Sign up** 按鈕跳轉到歡迎頁面嗎？
現在，將它改成只有完成表單輸入時才可以點選按鈕跳轉到歡迎頁面。

<?code-excerpt "lib/step2.dart (onPressed)"?>
```dart
TextButton(
  style: ButtonStyle(
    foregroundColor: MaterialStateProperty.resolveWith((states) {
      return states.contains(MaterialState.disabled)
          ? null
          : Colors.white;
    }),
    backgroundColor: MaterialStateProperty.resolveWith((states) {
      return states.contains(MaterialState.disabled)
          ? null
          : Colors.blue;
    }),
  ),
  onPressed:
      _formProgress == 1 ? _showWelcomeScreen : null, // UPDATED
  child: const Text('Sign up'),
),
```
</li>

<li markdown="1">Run the app.<br>
The **Sign up** button is initially disabled,
but becomes enabled when all three text fields contain
(any) text.

執行應用。<br>
剛開啟頁面時 **Sign up** 按鈕是禁用狀態，
當為三個欄位輸入內容（任意內容）時將會變成可點選狀態。

</li>
</ol>

### Observations
{:.no_toc}

### 觀察和分析
{:.no_toc}

* Calling a widget's `setState()` method tells Flutter that the
  widget needs to be updated on screen.
  The framework then disposes of the previous immutable widget
  (and its children), creates a new one
  (with its accompanying child widget tree),
  and renders it to screen. For this to work seamlessly,
  Flutter needs to be fast.
  The new widget tree must be created and rendered to screen
  in less than 1/60th of a second to create a smooth visual
  transition—especially for an animation.
  Luckily Flutter *is* fast.

  呼叫 widget 的 `setState()` 方法通知 Flutter 頁面上的 widget 需要重新建構。
  框架將銷燬之前的不可變 widget （上面說過 widget 一旦建立不可更改）（包含它的子級 widget） ，
  然後建立一個新的 widget （包含他的子級 widget 樹）並將新的 widget 渲染到頁面上。
  為了使應用執行順暢， Flutter 需要快速的銷燬和建立 widget。
  新建立的 widget 必須在不到 1/60 秒的時間渲染到頁面上，才能建立一個流暢的動畫效果。
  幸運的是 Flutter 就是這麼**這麼快**。當然如果你願意的話，也可以使用文字編輯器。

* The `progress` field is defined as a floating value,
  and is updated in the `_updateFormProgress` method.
  When all three fields are filled in, `_formProgress` is set to 1.0.
  When `_formProgress` is set to 1.0, the `onPressed` callback is set to the
  `_showWelcomeScreen` method. Now that its `onPressed` argument is non-null, the button is enabled.
  Like most Material Design buttons in Flutter,
  [TextButton][]s are disabled by default if their `onPressed` and `onLongPress` callbacks are null.
  `_showWelcomeScreen` method. The button is enabled when it's `onPressed`
  argument is non-null.

  `progress` 屬性定義為浮點值，並在 `_updateFormProgress` 方法中更新。
  當三個輸入框都被輸入後， `_formProgress` 設定為 1.0 。
  當 `_formProgress` 設定為 1.0 後，
  `onPressed` 的回呼(Callback)函式將設定為 `_showWelcomeScreen` 方法。
  當 `onPressed` 引數變為非空時按鈕將會變成可點選。
  所有的 [TextButton][] 在 `onPressed` 和 `onLongPress` 回呼(Callback)為空時，預設也是無法點選的，
  與 Flutter 中其他 Material Design 的按鈕一致。

* Notice that the `_updateFormProgress` passes a function to `setState()`.
  This is called an anonymous
  function and has the following syntax:

  請注意, `_updateFormProgress` 是透過傳遞一個函式呼叫 `setState()` 。
  這種被稱為匿名函式,語法如下所示:

  ```dart
  methodName(() {...});
  ```
  Where `methodName` is a named function that takes an anonymous
  callback function as an argument.

  名為 `methodName` 的函式把匿名回呼(Callback)函式作為引數。

* The Dart syntax in the last step that displays the
  welcome screen is:

  最後一步顯示歡迎頁面的 Dart 語法如下所示:

  <?code-excerpt "lib/step2.dart (ternary)" replace="/, \/\/ UPDATED//g"?>
  ```dart
  _formProgress == 1 ? _showWelcomeScreen : null
  ```
  This is a Dart conditional assignment and has the syntax:
  `condition ? expression1 : expression2`.
  If the expression `_formProgress == 1` is true, the entire expression results
  in the value on the left hand side of the `:`, which is the
  `_showWelcomeScreen` method in this case.

  Dart 三目運算語法如下: `condition ? expression1 : expression2` 。
  如果 `_formProgress == 1` 是正確的，則會取 `:` 左側的值，在這個範例中會取
  `_showWelcomeScreen` 方法。

## Step 2.5: Launch Dart DevTools

## 第 2.5 步：啟動 Dart 開發者工具

How do you debug a Flutter web app?
It's not too different from debugging any Flutter app.
You want to use [Dart DevTools][]!
(Not to be confused with Chrome DevTools.)

如何除錯 Flutter Web 應用？所有的 Flutter 應用除錯方法沒有很大的區別。
你應該使用 [Dart DevTools][]！（不要和 Chrome 開發者工具搞混淆了）

Our app currently has no bugs, but let's check it out anyway.
The following instructions for launching DevTools applies to any workflow,
but there is a short cut if you're using IntelliJ.
See the tip at the end of this section for more information.

雖然我們的應用現在沒有 bug ，但是我們依然來驗證一下。
下面的指引講明瞭 DevTools 使用的場景，
如果你使用的是 IntelliJ 編輯器則會有更好的方式。
可以透過檢視文件末尾的提示資訊獲取更多的資訊。

<ol markdown="1">
<li markdown="1">Run the app.<br>
If your app isn't currently running, launch it.
Select the **Chrome** device from the pull down
and launch it from your IDE or,
from the command line, use `flutter run -d chrome`,

執行應用。<br>
如果應用未啟動，啟動應用。
從下拉選項中選擇 **Chrome** 裝置然後使用 IDE 啟動，
或者在命令列中使用 `flutter run -d chrome` ，

</li>

<li markdown="1">Get the web socket info for DevTools.<br>
At the command line, or in the IDE,
you should see a message stating something like the following:

獲取開發者工具 （DevTools） 的 socket 資訊。<br>
在命令列或者 IDE 中你應該可以看下如下所示內容的資訊：

<pre>
Launching lib/main.dart on Chrome in debug mode...
Building application for the web...                                11.7s
Attempting to connect to browser instance..
Debug service listening on <b>ws://127.0.0.1:54998/pJqWWxNv92s=</b>
</pre>

Copy the address of the debug service, shown in bold.
You will need that to launch DevTools.

複製粗體顯示的除錯服務的地址，你可以用這個地址啟動 DevTools 。

</li>

<li markdown="1">Ensure that DevTools is installed.<br>
Do you have [DevTools installed][]?
If you are using an IDE,
make sure you have the Flutter and Dart plugins set up,
as described in the [VS Code][] and
[Android Studio and IntelliJ][] pages.
If you are working at the command line,
launch the DevTools server as explained in the
[DevTools command line][] page.

確認開發工具已被安裝。<br>
你是否 [已經安裝 DevTools 了呢][DevTools installed]？
如果你使用的是編輯器 (IDE) ，先確認已經用 [VS Code][] 和
 [Android Studio and IntelliJ][] 文件描述的方式安裝 Flutter
和 Dart 外掛。如果你使用的是命令列的方式，用 [DevTools command line][]
文件說明的方式啟動開發者工具服務 （DevTools server）。

</li>

<li markdown="1">Connect to DevTools.<br>
When DevTools launches, you should see something
like the following:

連線到 DevTools。<br>
當 DevTools 啟動時，你應該會看到如下類似的內容：

```terminal
Serving DevTools at http://127.0.0.1:9100
```
Go to this URL in a Chrome browser. You should see the DevTools
launch screen. It should look like the following:

在 Chrome 瀏覽器中開啟上面 URL，你應該可以看到 DevTools 執行頁面。
如下所示：

{% indent %}
  ![Screenshot of the DevTools launch screen]({{site.url}}/assets/images/docs/get-started/devtools-launch-screen.png){:width="100%"}
{% endindent %}
</li>

<li markdown="1">Connect to running app.<br>
Under **Connect to a running site**,
paste the ws location that you copied in step 2,
and click Connect. You should now see Dart DevTools
running successfully in your Chrome browser:

連線到執行的應用。<br>
在 **Connect to a running site** 下面貼上你在上面第 2 步中複製的 ws 地址，
然後點選連線。現在你應該可以看到 Dart DevTools 成功的執行在你的 Chrome 瀏覽器中，
如下所示：

{% indent %}
  ![Screenshot of DevTools running screen]({{site.url}}/assets/images/docs/get-started/devtools-running.png){:width="100%"}
{% endindent %}

Congratulations, you are now running Dart DevTools!

恭喜，你已經成功執行 Dart 開發者工具！

</li>
</ol>

{{site.alert.note}}

  This is not the only way to launch DevTools.
  If you are using IntelliJ,
  you can open DevTools by going to
  **Flutter Inspector** -> **More Actions** -> **Open DevTools**:

  這不是啟動開發者工具的唯一方式。
  如果你使用的是 IntelliJ 編輯器，
  你可以透過 **Flutter Inspector** -> **More Actions** -> **Open DevTools**
  的方式啟動開發者工具，如下所示：

  {% indent %}
  ![Screenshot of Flutter inspector with DevTools menu]({{site.url}}/assets/images/docs/get-started/intellij-devtools.png){:width="100%"}
  {% endindent %}

{{site.alert.end}}

<ol markdown="1">
<li markdown="1">Set a breakpoint.<br>
Now that you have DevTools running,
select the **Debugger** tab in the blue bar along the top.
The debugger pane appears and, in the lower left,
see a list of libraries used in the example.
Select `lib/main.dart` to display your Dart code
in the center pane.

設定斷點。<br>
現在你以前啟動了開發者工具，在上面的藍色工具欄中選擇 **Debugger** 選項。
在左下角出現除錯面板，可以檢視範例中使用的類別庫。
選擇 `lib/main.dart` 將在頁面中間顯示 Dart 程式碼。

{% indent %}
  ![Screenshot of the DevTools debugger]({{site.url}}/assets/images/docs/get-started/devtools-debugging.png){:width="100%"}
{% endindent %}
</li>

<li markdown="1">Set a breakpoint.<br>
In the Dart code,
scroll down to where `progress` is updated:

設定斷點。<br>
在 Dart 程式碼中，向下拉找到被修改的 `progress`，如下所示：

<?code-excerpt "lib/step2.dart (forLoop)"?>
```dart
for (final controller in controllers) {
  if (controller.value.text.isNotEmpty) {
    progress += 1 / controllers.length;
  }
}
```

Place a breakpoint on the line with the for loop by clicking to the
left of the line number. The breakpoint now appears
in the **Breakpoints** section to the left of the window.

在 for迴圈行的行數前面單擊設定斷點。
這個斷點將顯示在視窗左側的 **Breakpoints** 欄中。

</li>

<li markdown="1">Trigger the breakpoint.<br>
In the running app, click one of the text fields to gain focus.
The app hits the breakpoint and pauses.
In the DevTools screen, you can see on the left
the value of `progress`, which is 0. This is to be expected,
since none of the fields are filled in.
Step through the for loop to see
the program execution.

觸發斷點。<br>
在正在執行的應用中，點選任意一個輸入框獲取焦點。
應用會遇到斷點並暫停。
在開發者工具頁面，你可以在左側看到 `progress` 的值是 0 。
這是正常的，因為你沒有輸入任何內容，
遍歷 for迴圈觀察應用的執行。

</li>

<li markdown="1">Resume the app.<br>
Resume the app by clicking the green **Resume**
button in the DevTools window.

恢復應用程式。<br>
在開發者工具視窗點選綠色的 **Resume** 按鈕來恢復應用程式。

</li>

<li markdown="1">Delete the breakpoint.<br>
Delete the breakpoint by clicking it again, and resume the app.

刪除斷點。<br>
再次點選斷點來刪除斷點和恢復程式。

</li>
</ol>

This gives you a tiny glimpse of what is possible using DevTools,
but there is lots more! For more information,
see the [DevTools documentation][].

這裡只是粗略的介紹開發者工具的使用方式，還有更多沒有講到。
請參考 [DevTools 文件][DevTools documentation] 學習更多的內容。


## Step 3: Add animation for sign in progress

## 第3步：為輸入進度新增動畫效果

It's time to add animation! In this final step,
you'll create the animation for the
`LinearProgressIndicator` at the top of the sign in
area. The animation has the following behavior:

是時候新增動畫效果了！在最後一步，我們將在登入區域上方建立一個進度條動畫，
特效如下所述：

* When the app starts,
  a tiny red bar appears across the top of the sign in area.

  剛啟動時，登入區域的頂部顯示一條紅色的進度條。

* When one text field contains text,
  the red bar turns orange and animates 0.15
  of the way across the sign in area.

  當一個文字框被鍵入內容時，進度條從紅色變成橙色，
  並且進度條前進到距登入區域頂部 1/3 的位置。

* When two text fields contain text,
  the orange bar turns yellow and animates half
  of the way across the sign in area.

  當第二個文字框被鍵入內容時，進度條從橙色變為黃色，
  並且進度條前進到距登入區域頂部 2/3 的位置。

* When all three text fields contain text,
  the orange bar turns green and animates all the
  way across the sign in area.
  Also, the **Sign up** button becomes enabled.

  當三個文字框全部被輸入內容時，進度條從橙色變成綠色，並且逐漸充滿整個登入區域頂部。
  除此之外， **Sign up** 按鈕的狀態也變成可點選。

<ol markdown="1">
<li markdown="1">Add an `AnimatedProgressIndicator`.<br>
At the bottom of the file, add this widget:

新增進度條動畫效果 (`AnimatedProgressIndicator`)<br>
在檔案的下面，新增下面的 widget：

<?code-excerpt "lib/step3.dart (AnimatedProgressIndicator)"?>
```dart
class AnimatedProgressIndicator extends StatefulWidget {
  final double value;

  const AnimatedProgressIndicator({
    super.key,
    required this.value,
  });

  @override
  State<AnimatedProgressIndicator> createState() {
    return _AnimatedProgressIndicatorState();
  }
}

class _AnimatedProgressIndicatorState extends State<AnimatedProgressIndicator>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<Color?> _colorAnimation;
  late Animation<double> _curveAnimation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(milliseconds: 1200),
      vsync: this,
    );

    final colorTween = TweenSequence([
      TweenSequenceItem(
        tween: ColorTween(begin: Colors.red, end: Colors.orange),
        weight: 1,
      ),
      TweenSequenceItem(
        tween: ColorTween(begin: Colors.orange, end: Colors.yellow),
        weight: 1,
      ),
      TweenSequenceItem(
        tween: ColorTween(begin: Colors.yellow, end: Colors.green),
        weight: 1,
      ),
    ]);

    _colorAnimation = _controller.drive(colorTween);
    _curveAnimation = _controller.drive(CurveTween(curve: Curves.easeIn));
  }

  @override
  void didUpdateWidget(oldWidget) {
    super.didUpdateWidget(oldWidget);
    _controller.animateTo(widget.value);
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _controller,
      builder: (context, child) => LinearProgressIndicator(
        value: _curveAnimation.value,
        valueColor: _colorAnimation,
        backgroundColor: _colorAnimation.value?.withOpacity(0.4),
      ),
    );
  }
}
```

The [`didUpdateWidget`][] function updates
the `AnimatedProgressIndicatorState` whenever
`AnimatedProgressIndicator` changes.

[`didUpdateWidget`][] 方法會在 `AnimatedProgressIndicator` 變化時更新
`AnimatedProgressIndicatorState`。
</li>

<li markdown="1">Use the new `AnimatedProgressIndicator`.<br>
Then, replace the `LinearProgressIndicator` in the `Form`
with this new `AnimatedProgressIndicator`:

使用新的進度條。<br>
然後，使用新的 `AnimatedProgressIndicator` widget 替換表單中的 `LinearProgressIndicator`
 widget，如下所示：

<?code-excerpt "lib/step3.dart (UseAnimatedProgressIndicator)"?>
```dart
child: Column(
  mainAxisSize: MainAxisSize.min,
  children: [
    AnimatedProgressIndicator(value: _formProgress), // NEW
    Text('Sign up', style: Theme.of(context).textTheme.headlineMedium),
    Padding(
```

This widget uses an `AnimatedBuilder` to animate the
progress indicator to the latest value.

該 widget 使用 `AnimatedBuilder` 為最新值實現了進度的動畫顯示。

</li>

<li markdown="1">Run the app.<br>
Type anything into the three fields to verify that
the animation works, and that clicking the
**Sign up** button brings up the **Welcome** screen.

執行應用。<br>
在三個輸入框中輸入任意值來驗證動畫效果是否正常顯示，
然後點選 **Sign up** 按鈕將彈出歡迎頁面。

</li>
</ol>

### Complete sample

### 完整的範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-starting_code
import 'package:flutter/material.dart';

void main() => runApp(const SignUpApp());

class SignUpApp extends StatelessWidget {
  const SignUpApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      routes: {
        '/': (context) => const SignUpScreen(),
        '/welcome': (context) => const WelcomeScreen(),
      },
    );
  }
}

class SignUpScreen extends StatelessWidget {
  const SignUpScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.grey[200],
      body: const Center(
        child: SizedBox(
          width: 400,
          child: Card(
            child: SignUpForm(),
          ),
        ),
      ),
    );
  }
}

class WelcomeScreen extends StatelessWidget {
  const WelcomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text(
          'Welcome!',
          style: Theme.of(context).textTheme.displayMedium,
        ),
      ),
    );
  }
}

class SignUpForm extends StatefulWidget {
  const SignUpForm({super.key});

  @override
  State<SignUpForm> createState() => _SignUpFormState();
}

class _SignUpFormState extends State<SignUpForm> {
  final _firstNameTextController = TextEditingController();
  final _lastNameTextController = TextEditingController();
  final _usernameTextController = TextEditingController();

  double _formProgress = 0;

  void _updateFormProgress() {
    var progress = 0.0;
    final controllers = [
      _firstNameTextController,
      _lastNameTextController,
      _usernameTextController
    ];

    for (final controller in controllers) {
      if (controller.value.text.isNotEmpty) {
        progress += 1 / controllers.length;
      }
    }

    setState(() {
      _formProgress = progress;
    });
  }

  void _showWelcomeScreen() {
    Navigator.of(context).pushNamed('/welcome');
  }

  @override
  Widget build(BuildContext context) {
    return Form(
      onChanged: _updateFormProgress,
      child: Column(
        mainAxisSize: MainAxisSize.min,
        children: [
          AnimatedProgressIndicator(value: _formProgress),
          Text('Sign up', style: Theme.of(context).textTheme.headlineMedium),
          Padding(
            padding: const EdgeInsets.all(8),
            child: TextFormField(
              controller: _firstNameTextController,
              decoration: const InputDecoration(hintText: 'First name'),
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(8),
            child: TextFormField(
              controller: _lastNameTextController,
              decoration: const InputDecoration(hintText: 'Last name'),
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(8),
            child: TextFormField(
              controller: _usernameTextController,
              decoration: const InputDecoration(hintText: 'Username'),
            ),
          ),
          TextButton(
            style: ButtonStyle(
              foregroundColor: MaterialStateProperty.resolveWith((states) {
                return states.contains(MaterialState.disabled)
                    ? null
                    : Colors.white;
              }),
              backgroundColor: MaterialStateProperty.resolveWith((states) {
                return states.contains(MaterialState.disabled)
                    ? null
                    : Colors.blue;
              }),
            ),
            onPressed: _formProgress == 1 ? _showWelcomeScreen : null,
            child: const Text('Sign up'),
          ),
        ],
      ),
    );
  }
}

class AnimatedProgressIndicator extends StatefulWidget {
  final double value;

  const AnimatedProgressIndicator({
    super.key,
    required this.value,
  });

  @override
  State<AnimatedProgressIndicator> createState() {
    return _AnimatedProgressIndicatorState();
  }
}

class _AnimatedProgressIndicatorState extends State<AnimatedProgressIndicator>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<Color?> _colorAnimation;
  late Animation<double> _curveAnimation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(milliseconds: 1200),
      vsync: this,
    );

    final colorTween = TweenSequence([
      TweenSequenceItem(
        tween: ColorTween(begin: Colors.red, end: Colors.orange),
        weight: 1,
      ),
      TweenSequenceItem(
        tween: ColorTween(begin: Colors.orange, end: Colors.yellow),
        weight: 1,
      ),
      TweenSequenceItem(
        tween: ColorTween(begin: Colors.yellow, end: Colors.green),
        weight: 1,
      ),
    ]);

    _colorAnimation = _controller.drive(colorTween);
    _curveAnimation = _controller.drive(CurveTween(curve: Curves.easeIn));
  }

  @override
  void didUpdateWidget(oldWidget) {
    super.didUpdateWidget(oldWidget);
    _controller.animateTo(widget.value);
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _controller,
      builder: (context, child) => LinearProgressIndicator(
        value: _curveAnimation.value,
        valueColor: _colorAnimation,
        backgroundColor: _colorAnimation.value?.withOpacity(0.4),
      ),
    );
  }
}
```

### Observations
{:.no_toc}

### 觀察和分析
{:.no_toc}

* You can use an `AnimationController` to run any animation.

  你可以使用 `AnimationController` 控制任何動畫效果。

* `AnimatedBuilder` rebuilds the widget tree when the value
  of an `Animation` changes.

  當 `Animation` 的值改變時 `AnimatedBuilder` 將重新建構 widget 樹。

* Using a `Tween`, you can interpolate between almost any value,
  in this case, `Color`.

  使用動畫 `Tween` ，你還可以使用很多值，像這個範例中的 `Color`。

## What next?

## 下一步，我們該做什麼？

Congratulations!
You have created your first web app using Flutter!

恭喜！
你已經使用 Flutter 建立了第一個 Web 應用！

If you'd like to continue playing with this example,
perhaps you could add form validation.
For advice on how to do this,
see the [Building a form with validation][]
recipe in the [Flutter cookbook][].

如果你想繼續完善這個範例，或許你可以新增表單驗證。
如何繼續的建議，請參考 [Flutter cookbook][] 中的
[Building a form with validation][]

For more information on Flutter web apps,
Dart DevTools, or Flutter animations, see the following:

有關 Web 應用、Dart 開發者工具以及 Flutter 動畫的更多資訊，
請參考下面文件：

* [Animation docs][]
* [Dart DevTools][]
* [Implicit animations][] codelab
* [Web samples][]


[Android Studio and IntelliJ]: {{site.url}}/tools/devtools/android-studio
[Animation docs]: {{site.url}}/ui/animations
[Building a form with validation]: {{site.url}}/cookbook/forms/validation
[Building a web application with Flutter]: {{site.url}}/platform-integration/web/building
[Chrome browser]: https://www.google.com/chrome/?brand=CHBD&gclid=CjwKCAiAws7uBRAkEiwAMlbZjlVMZCxJDGAHjoSpoI_3z_HczSbgbMka5c9Z521R89cDoBM3zAluJRoCdCEQAvD_BwE&gclsrc=aw.ds
[create a new Flutter project]: {{site.url}}/get-started/test-drive
[Dart DevTools]: {{site.url}}/tools/devtools/overview
[DartPad]: {{site.dartpad}}
[DevTools command line]: {{site.url}}/tools/devtools/cli
[DevTools documentation]: {{site.url}}/tools/devtools
[DevTools installed]: {{site.url}}/tools/devtools/overview#how-do-i-install-devtools
[DartPad troubleshooting page]: {{site.dart-site}}/tools/dartpad/troubleshoot
[`didUpdateWidget`]: {{site.api}}/flutter/widgets/State/didUpdateWidget.html
[editor]: {{site.url}}/get-started/editor
[Effective Dart Style Guide]: {{site.dart-site}}/guides/language/effective-dart/style#dont-use-a-leading-underscore-for-identifiers-that-arent-private
[Flutter cookbook]: {{site.url}}/cookbook
[Flutter SDK]: {{site.url}}/get-started/install
[Implicit animations]: {{site.url}}/codelabs/implicit-animations
[Introduction to declarative UI]: {{site.url}}/get-started/flutter-for/declarative
[Material Design]: {{site.material}}/get-started
[TextButton]: {{site.api}}/flutter/material/TextButton-class.html
[VS Code]: {{site.url}}/tools/devtools/vscode
[Web samples]: {{site.github}}/flutter/samples/tree/main/web
[Widget]: {{site.api}}/flutter/widgets/Widget-class.html
[first_flutter_codelab]: {{site.url}}/get-started/codelab
