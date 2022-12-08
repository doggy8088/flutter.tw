---
title: Flutter for Xamarin.Forms developers
title: 給 Xamarin.Forms 開發者的 Flutter 指南
description: Learn how to apply Xamarin.Forms developer knowledge when building Flutter apps.
description: 學習如何把 Xamarin.Forms 的開發經驗應用到 Flutter 應用的開發中。
tags: Flutter課程,Flutter起步,Flutter入門
keywords: Flutter Xamarin.Forms,Xamarin.Forms,Xamarin.Forms轉Flutter
---

<?code-excerpt path-base="get-started/flutter-for/xamarin_devs"?>

This document is meant for Xamarin.Forms developers
looking to apply their existing knowledge
to build mobile apps with Flutter.
If you understand the fundamentals of the Xamarin.Forms framework,
then you can use this document as a jump start to Flutter development.

本文件旨在幫助 Xamarin.Forms 開發者利用已有的知識去建構 Flutter 移動應用。
如果你懂得 Xamarin.Forms 框架的基本原理，
那麼你就可以將本文件當作你開始 Flutter 開發的不錯的起點。

Your Android and iOS knowledge and skill set
are valuable when building with Flutter,
because Flutter relies on the native operating system configurations,
similar to how you would configure your native Xamarin.Forms projects.
The Flutter Frameworks is also similar to how you create a single UI,
that is used on multiple platforms.

你的 Android 和 iOS 知識以及技能組合在建構 Flutter 時都是有價值的，
因為 Flutter 依賴的原生系統配置都與你配置 Xamarin.Forms 原生專案時一樣。
Flutter 框架在建立適用於多個平台的單一介面時，與 Xamarin.Forms 是類似的。

This document can be used as a cookbook by jumping around
and finding questions that are most relevant to your needs.

這篇文件可以用作隨時查閱以及答疑解惑的專題手冊。

## Project setup

## 專案設定

### How does the app start?

### app 是如何執行的？

For each platform in Xamarin.Forms,
which creates a new application and starts your app.

對於 Xamarin.Forms 裡的每個平台，你可以呼叫 `LoadApplication` 方法，
建立一個新應用並執行你的應用。

```csharp
LoadApplication(new App());
```

In Flutter, the default main entry point is
`main` where you load your Flutter app.

在 Flutter 中，載入 Flutter app 的預設主入口是 `main`。

<?code-excerpt "lib/main.dart (Main)"?>
```dart
void main() {
  runApp(const MyApp());
}
```

In Xamarin.Forms, you assign a `Page` to the
`MainPage` property in the `Application` class.

在 Xamarin.Forms 中，你會分配一個 `Page` 到
`Application` 類中的 `MainPage` 屬性。

```csharp
public class App : Application
{
    public App()
    {
        MainPage = new ContentPage
        {
            Content = new Label
            {
                Text = "Hello World",
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.Center
            }
        };
    }
}
```

In Flutter, "everything is a widget", even the application itself.
The following example shows `MyApp`, a simple application `Widget`.

在 Flutter 中，「萬物皆 widget」，甚至連應用本身也是。
接下來的範例展示了 `MyApp`，一個簡單的應用 `Widget`。

<?code-excerpt "lib/main.dart (MyApp)"?>
```dart
class MyApp extends StatelessWidget {
  /// This widget is the root of your application.
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const Center(
      child: Text(
        'Hello World!',
        textDirection: TextDirection.ltr,
      ),
    );
  }
}
```

### How do you create a page?

### 如何建立一個頁面？

Xamarin.Forms has many types of pages;
`ContentPage` is the most common.
In Flutter, you specify an application widget that holds your root page.
You can use a [`MaterialApp`][] widget, which supports [Material Design][],
or you can use a [`CupertinoApp`][] widget, which supports an iOS-style app,
or you can use the lower level [`WidgetsApp`][],
which you can customize in any way you want.

Xamarin.Forms 擁有一些不同型別的頁面，`ContentPage` 是最為通用的。
在 Flutter 中，指定一個應用程式 widget 來控制你的根頁面。
你可以使用一個 [`MaterialApp`][] widget，它支援 [Material Design][]；
你也可以使用 [`CupertinoApp`][] widget，它能用來建立 iOS 風格的應用；
或者你也可以使用更底層的 [`WidgetsApp`][]，可供你隨心所欲地客製。

The following code defines the home page, a stateful widget.
In Flutter, all widgets are immutable,
but two types of widgets are supported: _Stateful_ and _Stateless_.
Examples of a stateless widget are titles, icons, or images.

接下來的程式碼定義了一個有狀態的主頁 widget。
在 Flutter 中，所有 widget 都是不可變的，並且包含以下兩種主要的 widget：
**有狀態** 和 **無狀態** widget。
無狀態 widget 的範例都是標題、圖示或圖片。

[`CupertinoApp`]: {{site.api}}/flutter/cupertino/CupertinoApp-class.html
[`MaterialApp`]: {{site.api}}/flutter/material/MaterialApp-class.html
[`WidgetsApp`]: {{site.api}}/flutter/widgets/WidgetsApp-class.html

The following example uses `MaterialApp`,
which holds its root page in the `home` property.

下面的範例使用了 `MaterialApp`，它透過 `home` 屬性中控制根頁面。

<?code-excerpt "lib/page.dart (MyApp)"?>
```dart
class MyApp extends StatelessWidget {
  /// This widget is the root of your application.
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Flutter Demo',
      home: MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```

From here, your actual first page is another `Widget`,
in which you create your state.

在這裡真正的首頁是另一個建立了狀態的 `Widget` (`MyHomePage`)。

A _Stateful_ widget, such as `MyHomePage` below, consists of two parts.
The first part, which is itself immutable, creates a `State` object
that holds the state of the object. The `State` object persists over
the life of the widget.

一個 **有狀態的** widget，例如下面的 MyHomePage，包含兩個部分。
第一部分，是自身不變的 widget，建立一個狀態物件來管理 widget 的狀態。
狀態物件在 widget 的整個生命週期中持續存在。

<?code-excerpt "lib/page.dart (MyHomePage)"?>
```dart
class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}
```

The `State` object implements the `build()` method for the stateful widget.

`State` 物件實現了有狀態 widget 中的 `build()` 方法。

When the state of the widget tree changes, call `setState()`,
which triggers a build of that portion of the UI.
Make sure to call `setState()` only when necessary,
and only on the part of the widget tree that has changed,
or it can result in poor UI performance.

當 widget 樹的狀態發生了改變，將會呼叫 `setState()`
觸發 widget 當中該部分 UI 的建構。
確保只在需要時呼叫 `setState()`，
並且在只有部分 widget 樹發生變化時呼叫，否則會造成糟糕的 UI 表現。

<?code-excerpt "lib/page.dart (MyHomePageState)"?>
```dart
class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        // Take the value from the MyHomePage object that was created by
        // the App.build method, and use it to set the appbar title.
        title: Text(widget.title),
      ),
      body: Center(
        // Center is a layout widget. It takes a single child and positions it
        // in the middle of the parent.
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

In Flutter, the UI (also known as widget tree), is immutable,
meaning you can't change its state once it's built.
You change fields in your `State` class, then call `setState()`
to rebuild the entire widget tree again.

在 Flutter 中的 UI（也就是 widget 樹）是不可變的，
意味著它一旦被建構，你就無法再改變它的狀態。
你可以修改 `State` 類中的欄位，並再次呼叫 `setState` 來重新建構整個 widget 樹。

This way of generating UI is different from Xamarin.Forms,
but there are many benefits to this approach.

這樣產生 UI 的方式不同於 Xamarin.Forms，但是卻帶來了更多好處。

## Views

## 檢視

### What is the equivalent of a Page or Element in Flutter?

### 在 Flutter 中頁面（Page）與元素（Element）的相同的是什麼？

{{site.alert.secondary}}

  How is react-style, or _declarative_, programming different from the
  traditional imperative style?
  For a comparison, see [Introduction to declarative
  UI][].

  響應式或者 **宣告式** 的程式設計和傳統的命令式風格有什麼不同呢？
  作為對比，請查閱 [宣告式 UI 介紹][Introduction to declarative UI]。

{{site.alert.end}}

`ContentPage`, `TabbedPage`, `FlyoutPage` are all types of pages
you might use in a Xamarin.Forms application.
These pages would then hold `Element`s to display the various controls.
In Xamarin.Forms an `Entry` or `Button` are examples of an `Element`.

`ContentPage`、`TabbedPage`、`FlyoutPage`
就是你可以在 Xamarin.Forms 應用程式中使用的全部頁面型別。
這些頁面會控制 `Element` 來顯示各種控制項。
在 Xamarin.Forms 中，`Entry` 或者 `Button` 就是一個 `Element` 的範例。

In Flutter, almost everything is a widget.
A `Page`, called a `Route` in Flutter, is a widget.
Buttons, progress bars, and animation controllers are all widgets.
When building a route, you create a widget tree.

在 Flutter 中，幾乎所有東西都是 widget，
在 Flutter 中被稱作 `Route` 的一個 `Page`，也是一個 widget。
按鈕、進度條、動畫控制器都是 widget。
當建構一個路由時，就會建立一棵 widget 樹。

Flutter includes the [Material Components][] library.
These are widgets that implement the [Material Design guidelines][].
Material Design is a flexible design system
[optimized for all platforms][], including iOS.

Flutter 包含 [Material 元件][Material Components] 庫。
這些都是實現了 [Material Design 指南][Material Design guidelines] 的 widget。
Material Design 是一個靈活的
[針對所有平台][optimized for all platforms]
的設計系統，包括 iOS。

But Flutter is flexible and expressive enough
to implement any design language.
For example, on iOS, you can use the [Cupertino widgets][]
to produce an interface that looks like [Apple's iOS design language][].

不過，Flutter 有足夠靈活和自描述性 (expressive) 去實現任何設計語言。
舉個例子，在 iOS 上，你可以用 [Cupertino widgets][] 來產生一個看起來像
[蘋果 iOS 設計語言][Apple's iOS design language] 的介面。

### How do I update widgets?

### 如何更新 widget？

In Xamarin.Forms, each `Page` or `Element` is a stateful class,
that has properties and methods.
You update your `Element` by updating a property,
and this is propagated down to the native control.

在 Xamarin.Forms 中，每一個 `Page` 或者 `Element` 都是一個有狀態的類，
擁有一些屬性和方法。透過更新一個屬性來更新你的元素，而且這會傳遞到原生控制項。

In Flutter, `Widget`s are immutable and you can't directly update them
by changing a property, instead you have to work with the widget's state.

在 Flutter 中，`widget` 是不可變的，你不可以直接地透過修改一個屬性來更新它們，
而是應該使用 widget 的狀態。

This is where the concept of Stateful vs Stateless widgets comes from.
A `StatelessWidget` is just what it sounds like&mdash;
a widget with no state information.

有狀態 widget 和無狀態 widget 的概念就是出自這裡，
`StatelessWidget` 顧名思義，就是一個沒有狀態資訊的 widget。

`StatelessWidgets` are useful when the part of the user interface
you are describing doesn't depend on anything
other than the configuration information in the object.

當你在描繪使用者介面的一個不依賴除物件中的配置資訊之外任何東西的部分時，
`StatelessWidget` 是有用的。

For example, in Xamarin.Forms, this is similar
to placing an `Image` with your logo.
The logo is not going to change during runtime,
so use a `StatelessWidget` in Flutter.

舉個例子，在 Xamarin.Forms 中，可以輕而易舉地用你的 logo 替換一張 `Image`。
這個 logo 將不會在執行過程中修改，所以在 Flutter 會使用 `StatelessWidget`。

If you want to dynamically change the UI based on data received
after making an HTTP call or a user interaction,
then you have to work with `StatefulWidget`
and tell the Flutter framework that
the widget’s `State` has been updated,
so it can update that widget.

如果你想基於進行了 HTTP 呼叫或者使用者互動後接收到的資料來動態地修改 UI，
你需要使用 `StatefulWidget` 並告訴 Flutter 框架這個 widget 的 `State`
已經被更新了所以它可以更新那個 widget。

The important thing to note here is at the core
both stateless and stateful widgets behave the same.
They rebuild every frame, the difference is
the `StatefulWidget` has a `State` object
that stores state data across frames and restores it.

這裡要記下的重要內容是有狀態和無狀態 widget 的核心行為都是一樣的。
他們重建每個結構，不同的是 `StatefulWidget` 擁有一個
`State` 物件來跨結構儲存狀態資料和恢復它。

If you are in doubt, then always remember this rule: if a widget changes
(because of user interactions, for example) it’s stateful.
However, if a widget reacts to change, the containing parent widget can
still be stateless if it doesn't itself react to change.

如果你有疑惑，那麼就記住這個規則：
如果一個 widget 改變了（例如是因為使用者互動），它就是有狀態的。
相反，如果一個 widget 對修改作出反應，
包含它的父 widget 如果本身沒有對修改作出反應，它就是無狀態的。

The following example shows how to use a `StatelessWidget`.
A common `StatelessWidget` is the `Text` widget.
If you look at the implementation of the `Text` widget
you'll find it subclasses `StatelessWidget`.

接下來的範例展示瞭如何使用一個 `StatelessWidget`。
一個常見的 `StatelessWidget` 是 `Text` widget。
如果你閱讀了 `Text` widget 的實現，你會發現它是 `StatelessWidget` 的子類別。

<?code-excerpt "lib/views.dart (Text)" replace="/return //g"?>
```dart
const Text(
  'I like Flutter!',
  style: TextStyle(fontWeight: FontWeight.bold),
);
```

As you can see, the `Text` widget has no state information associated with it,
it renders what is passed in its constructors and nothing more.

如你所見，`Text` widget 沒有狀態資訊與它關聯，
它只渲染在它的建構函式中呈現的內容。

But, what if you want to make "I Like Flutter" change dynamically,
for example, when clicking a `FloatingActionButton`?

但是，如果你想動態地修改「I Like Flutter」呢？
例如在點選一個 `FloatingActionButton` 時進行修改。

To achieve this, wrap the `Text` widget in a `StatefulWidget`
and update it when the user clicks the button,
as shown in the following example:

為了實現這個目標，你需要將 `Text` widget 放到一個 `StatefulWidget` 中，
並在用使用者點選按鈕時更新它，正如接下來的例子：

<?code-excerpt "lib/views_stateful.dart"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  /// This widget is the root of your application.
  const SampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Sample App',
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  /// Default placeholder text
  String textToShow = 'I Like Flutter';

  void _updateText() {
    setState(() {
      // Update the text
      textToShow = 'Flutter is Awesome!';
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Sample App')),
      body: Center(child: Text(textToShow)),
      floatingActionButton: FloatingActionButton(
        onPressed: _updateText,
        tooltip: 'Update Text',
        child: const Icon(Icons.update),
      ),
    );
  }
}
```

### How do I lay out my widgets? What is the equivalent of an XAML file?

### 我該如何佈局我的 widget 呢？什麼東西可以等價於一個 XAML 檔案？

In Xamarin.Forms, most developers write layouts in XAML,
though sometimes in C#.
In Flutter, you write your layouts with a widget tree in code.

在 Xamarin.Forms 中，大部分開發者用 XAML 寫佈局，有時也會用到 C#。
在 Flutter 中編碼一棵 widget 樹來編寫佈局。

The following example shows how to display a simple widget with padding:

接下來的範例展示如何顯示一個簡單的帶內邊距的 widget：

<?code-excerpt "lib/padding.dart (Padding)"?>
```dart
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(title: const Text('Sample App')),
    body: Center(
      child: ElevatedButton(
        style: ElevatedButton.styleFrom(
          padding: const EdgeInsets.only(left: 20.0, right: 30.0),
        ),
        onPressed: () {},
        child: const Text('Hello'),
      ),
    ),
  );
}
```

You can view the layouts that Flutter has to offer in the
[widget catalog][].

你可以檢視 Flutter 在 [widget 目錄][widget catalog] 中提供的佈局。

### How do I add or remove an Element from my layout?

### 如何從佈局中新增或移除一個元素?

In Xamarin.Forms, you had to remove or add an `Element` in code.
This involved either setting the `Content` property or calling
`Add()` or `Remove()` if it was a list.

在 Xamarin.Forms 中，你需要在程式碼中移除或新增一個 `Element`。
如果是一個列表，這將會涉及設定 `Content` 屬性
或者呼叫 `Add()` 或者 `Remove()` 方法。

In Flutter, because widgets are immutable there is no direct equivalent.
Instead, you can pass a function to the parent that returns a widget,
and control that child's creation with a boolean flag.

在 Flutter 中，因為 widget 都是不可變的，所以沒有直接對等的東西。
但是你可以將一個建構 widget 的函式傳遞給父級，
並用布林值控制它的子 widget 的建立。

The following example shows how to toggle between two widgets
when the user clicks the `FloatingActionButton`:

下面的範例展示當用戶點選 `FloatingActionButton` 時，
如何在兩個 widget 之間切換。

<?code-excerpt "lib/views.dart (AddRemoveElement)"?>
```dart
class SampleApp extends StatelessWidget {
  /// This widget is the root of your application.
  const SampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Sample App',
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  /// Default value for toggle
  bool toggle = true;
  void _toggle() {
    setState(() {
      toggle = !toggle;
    });
  }

  Widget _getToggleChild() {
    if (toggle) {
      return const Text('Toggle One');
    }
    return CupertinoButton(
      onPressed: () {},
      child: const Text('Toggle Two'),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Sample App')),
      body: Center(child: _getToggleChild()),
      floatingActionButton: FloatingActionButton(
        onPressed: _toggle,
        tooltip: 'Update Text',
        child: const Icon(Icons.update),
      ),
    );
  }
}
```

### How do I animate a widget?

### 如何讓一個 widget 動起來？

In Xamarin.Forms, you create simple animations using ViewExtensions that
include methods such as `FadeTo` and `TranslateTo`.
You would use these methods on a view
to perform the required animations.

在 Xamarin.Forms 中，你可以利用 `FadeTo` 和 `TranslateTo`
等檢視擴充方法（ViewExtensions）來建立簡單的動畫。
你需要在一個檢視中使用這些方法來執行需要的動畫。

```xml
<Image Source="{Binding MyImage}" x:Name="myImage" />
```

Then in code behind, or a behavior, this would fade in the image,
over a 1-second period.

在後面的程式碼或一個動作中，這個會在 1 秒內淡入這張影象。

```csharp
myImage.FadeTo(0, 1000);
```

In Flutter, you animate widgets using the animation library
by wrapping widgets inside an animated widget.
Use an `AnimationController`, which is an `Animation<double>`
that can pause, seek, stop and reverse the animation.
It requires a `Ticker` that signals when vsync happens,
and produces a linear interpolation between 0 and 1
on each frame while it's running.
You then create one or more`Animation`s and attach them to the controller.

Flutter 透過 `Animation<double>` 的子類別
`AnimationController` 來暫停、播放、停止以及逆向播放動畫。
它需要一個 `Ticker` 在垂直同步 (vsync) 的時候發出訊號，
並且在執行的時候建立一個介於 0 和 1 之間的線性插值。
然後你就可以建立一個或多個 `Animation`，並將它們繫結到控制器上。

For example, you might use `CurvedAnimation`
to implement an animation along an interpolated curve.
In this sense, the controller is the "master" source of the animation progress
and the `CurvedAnimation` computes the curve
that replaces the controller's default linear motion.
Like widgets, animations in Flutter work with composition.

例如，你可以使用 `CurvedAnimation` 來實現一個曲線插值的動畫。
在這種情況下，控制器決定了動畫進度，
`CurvedAnimation` 計算用於替換控制器預設線性動畫的曲線值。
與 Widget 一樣，Flutter 中的動畫效果也可以組合使用。

When building the widget tree, you assign the `Animation`
to an animated property of a widget,
such as the opacity of a `FadeTransition`,
and tell the controller to start the animation.

在建構 Widget 樹的時候，你需要將 `Animation` 物件賦值給某個 Widget 的動畫屬性，
例如 `FadeTransition` 的不透明度屬性，並讓控制器開始動畫。

The following example shows how to write a `FadeTransition` that fades
the widget into a logo when you press the `FloatingActionButton`:

下面的例子展示瞭如何實現一個點選 `FloatingActionButton` 時
將一個 Widget 漸變為一個圖示的 `FadeTransition`：

<?code-excerpt "lib/animation.dart"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const FadeAppTest());
}

class FadeAppTest extends StatelessWidget {
  /// This widget is the root of your application.
  const FadeAppTest({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Fade Demo',
      home: MyFadeTest(title: 'Fade Demo'),
    );
  }
}

class MyFadeTest extends StatefulWidget {
  const MyFadeTest({super.key, required this.title});

  final String title;

  @override
  State<MyFadeTest> createState() => _MyFadeTest();
}

class _MyFadeTest extends State<MyFadeTest> with TickerProviderStateMixin {
  late final AnimationController controller;
  late final CurvedAnimation curve;

  @override
  void initState() {
    super.initState();
    controller = AnimationController(
      duration: const Duration(milliseconds: 2000),
      vsync: this,
    );
    curve = CurvedAnimation(
      parent: controller,
      curve: Curves.easeIn,
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(widget.title)),
      body: Center(
        child: FadeTransition(
          opacity: curve,
          child: const FlutterLogo(size: 100.0),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          controller.forward();
        },
        tooltip: 'Fade',
        child: const Icon(Icons.brush),
      ),
    );
  }
}
```

For more information, see [Animation & Motion widgets][],
the [Animations tutorial][], and the [Animations overview][].

獲取更多內容，請檢視 [動畫 & 運動 Widget][Animation & Motion widgets]、
[動畫指南][Animations tutorial] 以及 [動畫概覽][Animations overview]。

### How do I draw/paint on the screen?

### 如何在螢幕上繪圖？

Xamarin.Forms never had a built-in way to draw directly on the screen.
Many would use SkiaSharp, if they needed a custom image drawn. In Flutter,
you have direct access to the Skia Canvas and can easily draw on screen.

Xamarin.Forms 從來沒有任何內建的方法來直接在螢幕上繪圖。
如果他們需要一個自訂影象繪製，大多數使用 SkiaSharp。
在 Flutter 中，你可以直接存取 Skia 畫布（Skia Canvas）
方便地在螢幕上繪圖。

Flutter has two classes that help you draw to the canvas: `CustomPaint`
and `CustomPainter`, the latter of which implements your algorithm to draw to
the canvas.

Flutter 有兩個幫助你用畫布 (canvas) 進行繪製的類：
`CustomPaint` 和 `CustomPainter`，
後者可以實現自訂的繪製演算法。

To learn how to implement a signature painter in Flutter,
see Collin's answer on [StackOverflow][].

如果想學習在 Flutter 中如何實現一個簽名功能，
可以檢視 Collin 在 [StackOverflow][] 上的回答。

[StackOverflow]: {{site.so}}/questions/46241071/create-signature-area-for-mobile-app-in-dart-flutter

<?code-excerpt "lib/draw.dart"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MaterialApp(home: DemoApp()));
}

class DemoApp extends StatelessWidget {
  const DemoApp({super.key});

  @override
  Widget build(BuildContext context) => const Scaffold(body: Signature());
}

class Signature extends StatefulWidget {
  const Signature({super.key});

  @override
  SignatureState createState() => SignatureState();
}

class SignatureState extends State<Signature> {
  List<Offset?> _points = <Offset?>[];

  void _onPanUpdate(DragUpdateDetails details) {
    setState(() {
      final RenderBox referenceBox = context.findRenderObject() as RenderBox;
      final Offset localPosition = referenceBox.globalToLocal(
        details.globalPosition,
      );
      _points = List.from(_points)..add(localPosition);
    });
  }

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onPanUpdate: _onPanUpdate,
      onPanEnd: (details) => _points.add(null),
      child: CustomPaint(
        painter: SignaturePainter(_points),
        size: Size.infinite,
      ),
    );
  }
}

class SignaturePainter extends CustomPainter {
  const SignaturePainter(this.points);

  final List<Offset?> points;

  @override
  void paint(Canvas canvas, Size size) {
    final Paint paint = Paint()
      ..color = Colors.black
      ..strokeCap = StrokeCap.round
      ..strokeWidth = 5.0;
    for (int i = 0; i < points.length - 1; i++) {
      if (points[i] != null && points[i + 1] != null) {
        canvas.drawLine(points[i]!, points[i + 1]!, paint);
      }
    }
  }

  @override
  bool shouldRepaint(SignaturePainter oldDelegate) =>
      oldDelegate.points != points;
}
```

### Where is the widget's opacity?

### widget 的不透明度在哪裡？

On Xamarin.Forms, all `VisualElement`s have an Opacity.
In Flutter, you need to wrap a widget in an
[`Opacity` widget][] to accomplish this.

Xamarin.Forms 上，所有 `VisualElement` 都有不透明度的屬性。
在 Flutter 中，你需要將 widget 放到一個
[不透明度 widget][`Opacity` widget] 來實現。

### How do I build custom widgets?

### 如何建構一個自訂 widget ？

In Xamarin.Forms, you typically subclass `VisualElement`,
or use a pre-existing `VisualElement`, to override and
implement methods that achieve the desired behavior.

在 Xamarin.Forms 中，通常派生 `VisualElement`
或使用一個已有的 `VisualElement` ，來重寫和實現所需行為的方法。

In Flutter, build a custom widget by
[composing][]
smaller widgets (instead of extending them).
It is somewhat similar to implementing a custom control
based off a `Grid` with numerous `VisualElement`s added in,
while extending with custom logic.

在 Flutter 中，透過 [組合][composing]
更小的 Widget 來建立自訂 Widget（而不是繼承它們）。
這有點類似於基於 `Grid` 實現自訂控制項，
其中添加了大量 `VisualElement`，同時使用自訂邏輯進行擴充。

For example, how do you build a `CustomButton`
that takes a label in the constructor?
Create a CustomButton that composes a `ElevatedButton`
with a label, rather than by extending `ElevatedButton`:

舉例來說，你該如何建立一個在構造器接收標籤引數的 `CustomButton`？
你要組合 `RaisedButton` 和一個標籤來建立自訂按鈕，
而不是繼承 `RaisedButton`：

<?code-excerpt "lib/custom_button.dart (CustomButton)"?>
```dart
class CustomButton extends StatelessWidget {
  const CustomButton(this.label, {super.key});

  final String label;

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: () {},
      child: Text(label),
    );
  }
}
```

Then use `CustomButton`, just as you'd use any other Flutter widget:

然後就像使用其它 Flutter Widget 一樣使用 `CustomButton`：

<?code-excerpt "lib/custom_button.dart (UseCustomButton)"?>
```dart
@override
Widget build(BuildContext context) {
  return const Center(
    child: CustomButton('Hello'),
  );
}
```

## Navigation

## 導航

### How do I navigate between pages?

### 如何在頁面之間導航？

In Xamarin.Forms, the `NavigationPage` class
provides a hierarchical navigation experience
where the user is able to navigate through pages,
forwards and backwards.

在 Xamarin.Forms 中，`NavigationPage` 類提供了一個
階級式的導航方式，讓使用者可以在頁面之間來回進行跳轉。

Flutter has a similar implementation,
using a `Navigator` and `Routes`.
A `Route` is an abstraction for a `Page` of an app,
and a `Navigator` is a [widget][] that manages routes.

Flutter 也有類似的實現，使用 `Navigator` 和 `Route`。
`Route` 是應用程式裡 `Page` 的抽象，
而 `Navigator` 是用於管理路由的 [widget][]。

A route roughly maps to a `Page`.
The navigator works in a similar way to the Xamarin.Forms `NavigationPage`,
in that it can `push()` and `pop()` routes depending on
whether you want to navigate to, or back from, a view.

一個路由大致上對映到一個 `Page`。
`Navigator` 的工作方式類似於 Xamarin.Forms 的 `NavigationPage`，
在裡面可以 `push()` 和 `pop()` 路由，
取決於你是否想導航到一個檢視，或者從它返回。

To navigate between pages, you have a couple options:

你有多種不同的方式在頁面間導航：

* Specify a `Map` of route names. (MaterialApp)

  定義一個 route 名字的 `Map`。(MaterialApp)

* Directly navigate to a route. (WidgetApp)

  直接導航到一個 route。(WidgetApp)

The following example builds a Map.

接下來建構一個對映的範例。

<?code-excerpt "lib/navigation.dart (Main)"?>
```dart
void main() {
  runApp(
    MaterialApp(
      home: const MyAppHome(), // becomes the route named '/'
      routes: <String, WidgetBuilder>{
        '/a': (context) => const MyPage(title: 'page A'),
        '/b': (context) => const MyPage(title: 'page B'),
        '/c': (context) => const MyPage(title: 'page C'),
      },
    ),
  );
}
```

Navigate to a route by pushing its name to the `Navigator`.

透過路由名 **壓棧** (`push`) 到 `Navigator` 中來跳轉到這個 route。

<?code-excerpt "lib/navigation.dart (PushNamed)"?>
```dart
Navigator.of(context).pushNamed('/b');
```

The `Navigator` is a stack that manages your app's routes.
Pushing a route to the stack moves to that route.
Popping a route from the stack, returns to the previous route.
This is done by awaiting on the `Future` returned by `push()`.

`Navigator` 管理應用程式的路由堆疊。
把一個路由推入堆疊可以導航到這個路由，
而從堆疊彈出一個路由可以返回到前一個路由。
這是透過 `await` 被 `push()` 返回的 `Future` 來完成的。

`async`/`await` is very similar to the .NET implementation
and is explained in more detail in [Async UI][].

`async`/`await` 與 .NET 的實現非常類似，在 [Async UI][] 中有更詳盡的解釋。

For example, to start a `location` route
that lets the user select their location,
you might do the following:

舉個例子，想要讓使用者選擇他們的定位的 `定位 (location)` 路由，你需要以下步驟：

<?code-excerpt "lib/navigation.dart (await)"?>
```dart
Object? coordinates = await Navigator.of(context).pushNamed('/location');
```

And then, inside your ‘location’ route, once the user has selected their
location, pop the stack with the result:

然後，在你的「定位」路由裡，使用者選擇他們的定位後，透過 `pop()` 路由堆疊來返回結果。

<?code-excerpt "lib/navigation.dart (PopLocation)"?>
```dart
Navigator.of(context).pop({'lat': 43.821757, 'long': -79.226392});
```

### How do I navigate to another app?

### 如何導航到其它應用程式？

In Xamarin.Forms, to send the user to another application,
you use a specific URI scheme, using `Device.OpenUrl("mailto://")`.

在 Xamarin.Forms 中，需要用指定的 URI 協議並使用
`Device.OpenUrl("mailto://")` 跳轉到其它應用程式。

To implement this functionality in Flutter,
create a native platform integration, or use an [existing plugin][],
such as[`url_launcher`][], available with many other packages on [pub.dev][].

在 Flutter 裡想要實現這個功能，需要建立原生平台的整合層，
或者使用已經存在的 [外掛][existing plugin]，例如
[`url_launcher`][]，可與在 [[pub.dev][] 上的許多其他包一起使用。。

## Async UI

## 非同步 UI

### What is the equivalent of Device.BeginOnMainThread() in Flutter?

### 在 Flutter 中有什麼是跟 Device.BeginOnMainThread() 方法是相等的？

Dart has a single-threaded execution model,
with support for `Isolate`s (a way to run Dart codes on another thread),
an event loop, and asynchronous programming.
Unless you spawn an `Isolate`,
your Dart code runs in the main UI thread
and is driven by an event loop.

Dart 有一個單執行緒執行的模型，同時也支援 `Isolate`
（在另一個執行緒執行 Dart 程式碼的方法），它是一個事件迴圈和非同步程式設計方式。
除非你建立一個 `Isolate`，否則你的 Dart 程式碼會執行在主 UI 執行緒，
並被一個事件迴圈所驅動。

Dart's single-threaded model doesn't mean you need to run everything
as a blocking operation that causes the UI to freeze.
Much like Xamarin.Forms, you need to keep the UI thread free.
You would use `async`/`await` to perform tasks,
where you must wait for the response.

Dart 的單執行緒模型並不意味著你需要以會導致 UI 凍結的
阻塞操作的方式來執行所有程式碼。
與 Xamarin.Forms 一樣，UI 執行緒應該儘可能地保持空閒。
你將使用 `async`/`wait` 來執行任務，其中必須等待響應。

In Flutter, use the asynchronous facilities that the Dart language provides,
also named `async`/`await`, to perform asynchronous work.
This is very similar to C# and should be very easy to use
for any Xamarin.Forms developer.

在 Flutter 中，可以使用 Dart 語言提供的非同步工具，
例如 `async`/`await` 來執行非同步任務。
這跟 C# 很像，並且對於 Xamarin.Forms 開發者來說應該是非常容易使用的。

For example, you can run network code without causing the UI to hang by
using `async`/`await` and letting Dart do the heavy lifting:

例如，你可以透過使用 `async`/`await`
來執行網路程式碼而且不會導致 UI 掛起，
同時讓 Dart 來處理背後的繁重細節：

<?code-excerpt "lib/data.dart (loadData)"?>
```dart
Future<void> loadData() async {
  final Uri dataURL = Uri.parse(
    'https://jsonplaceholder.typicode.com/posts',
  );
  final http.Response response = await http.get(dataURL);
  setState(() {
    data = jsonDecode(response.body);
  });
}
```

Once the awaited network call is done,
update the UI by calling `setState()`,
which triggers a rebuild of the widget subtree and updates the data.

一旦用 `await` 修飾的網路操作完成，再呼叫 `setState()` 更新 UI，
這會觸發 widget 子樹的重建並更新資料。

The following example loads data asynchronously
and displays it in a `ListView`:

下面的例子展示了非同步載入資料並將之展示在 `ListView` 內：

<?code-excerpt "lib/data.dart"?>
```dart
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  const SampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Sample App',
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List<Map<String, dynamic>> data = <Map<String, dynamic>>[];

  @override
  void initState() {
    super.initState();
    loadData();
  }

  Future<void> loadData() async {
    final Uri dataURL = Uri.parse(
      'https://jsonplaceholder.typicode.com/posts',
    );
    final http.Response response = await http.get(dataURL);
    setState(() {
      data = jsonDecode(response.body);
    });
  }

  Widget getRow(int index) {
    return Padding(
      padding: const EdgeInsets.all(10.0),
      child: Text('Row ${data[index]['title']}'),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Sample App')),
      body: ListView.builder(
        itemCount: data.length,
        itemBuilder: (context, index) {
          return getRow(index);
        },
      ),
    );
  }
}
```

Refer to the next section for more information
on doing work in the background,
and how Flutter differs from Android.

參考下一節內容獲取更多關於後臺任務以及 Flutter 與 Android 的差異的資訊。

### How do you move work to a background thread?

### 如何將工作轉移到後臺執行緒？

Since Flutter is single threaded and runs an event loop,
you don't have to worry about thread management
or spawning background threads.
This is very similar to Xamarin.Forms.
If you're doing I/O-bound work, such as disk access or a network call,
then you can safely use `async`/`await` and you're all set.

因為 Flutter 是單執行緒的，並且持有事件迴圈，
所以你不必擔心執行緒管理或產生後臺執行緒。
這一點與 Xamarin.Forms 非常相似。如果你正在做 I/O 密集型的工作，比如磁碟存取或網路呼叫，
那麼你可以安全地使用 `async`/`await`，這樣就一切就緒了。

If, on the other hand, you need to do computationally intensive work
that keeps the CPU busy,
you want to move it to an `Isolate` to avoid blocking the event loop,
like you would keep _any_ sort of work out of the main thread.
This is similar to when you move things to a different
thread via `Task.Run()` in Xamarin.Forms.

另一方面，如果你需要執行消耗 CPU 的計算密集型工作，
那麼你可以將其轉移到一個 `Isolate` 上以避免阻塞事件迴圈，
就像你會將任何任務放到主執行緒之外一樣。
這類似於透過 Xamarin.Forms 中的 `Task.Run()` 將內容移動到另一個執行緒。

For I/O-bound work, declare the function as an `async` function,
and `await` on long-running tasks inside the function:

對於和 I/O 繫結的任務，將方法宣告為 `async` 方法，
並在方法內 `await` 一個長時間執行的任務：

<?code-excerpt "lib/data.dart (loadData)"?>
```dart
Future<void> loadData() async {
  final Uri dataURL = Uri.parse(
    'https://jsonplaceholder.typicode.com/posts',
  );
  final http.Response response = await http.get(dataURL);
  setState(() {
    data = jsonDecode(response.body);
  });
}
```

This is how you would typically do network or database calls,
which are both I/O operations.

這是你通常執行網路或資料庫呼叫的方式，它們都屬於 I/O 操作。

However, there are times when you might be processing
a large amount of data and your UI hangs.
In Flutter, use `Isolate`s to take advantage of multiple CPU cores
to do long-running or computationally intensive tasks.

然而，有時候你可能需要處理大量的資料並掛起你的 UI。
在 Flutter 中，可以透過使用 `Isolate`
來利用多核處理器的優勢執行耗時或計算密集的任務。

Isolates are separate execution threads that
do not share any memory with the main execution memory heap.
This is a difference between `Task.Run()`.
This means you can’t access variables from the main thread,
or update your UI by calling `setState()`.

Isolate 是獨立執行的執行緒，不會和主執行記憶體堆分享記憶體。
這是與 `Task.Run()` 的區別。
這意味著你無法存取主執行緒的變數，或者呼叫 `setState()` 更新 UI。

The following example shows, in a simple isolate,
how to share data back to the main thread to update the UI.

下面的例子展示了一個簡單的 Isolate 是如何
將資料分享給主執行緒來更新 UI 的。

<?code-excerpt "lib/isolates.dart (SimpleIsolate)"?>
```dart
Future<void> loadData() async {
  final ReceivePort receivePort = ReceivePort();
  await Isolate.spawn(dataLoader, receivePort.sendPort);

  // The 'echo' isolate sends its SendPort as the first message
  final SendPort sendPort = await receivePort.first as SendPort;
  final List<Map<String, dynamic>> msg = await sendReceive(
    sendPort,
    'https://jsonplaceholder.typicode.com/posts',
  );
  setState(() {
    data = msg;
  });
}

// The entry point for the isolate
static Future<void> dataLoader(SendPort sendPort) async {
  // Open the ReceivePort for incoming messages.
  final ReceivePort port = ReceivePort();

  // Notify any other isolates what port this isolate listens to.
  sendPort.send(port.sendPort);
  await for (final dynamic msg in port) {
    final String url = msg[0] as String;
    final SendPort replyTo = msg[1] as SendPort;

    final Uri dataURL = Uri.parse(url);
    final http.Response response = await http.get(dataURL);
    // Lots of JSON to parse
    replyTo.send(jsonDecode(response.body) as List<Map<String, dynamic>>);
  }
}

Future<List<Map<String, dynamic>>> sendReceive(SendPort port, String msg) {
  final ReceivePort response = ReceivePort();
  port.send(<dynamic>[msg, response.sendPort]);
  return response.first as Future<List<Map<String, dynamic>>>;
}
```

Here, `dataLoader()` is the `Isolate` that runs in
its own separate execution thread.
In the isolate, you can perform more CPU intensive
processing (parsing a big JSON, for example),
or perform computationally intensive math,
such as encryption or signal processing.

這裡的 `dataLoader()` 就是執行在自己獨立執行執行緒內的 `Isolate`。
在 Isolate 中你可以執行更多的 CPU 密集型操作
（例如解析一個大的 JSON 資料），
或者執行計算密集型的數學運算，例如加密或訊號處理。

You can run the full example below:

你可以執行下面這個完整的例子：

<?code-excerpt "lib/isolates.dart"?>
```dart
import 'dart:async';
import 'dart:convert';
import 'dart:isolate';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  const SampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Sample App',
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List<Map<String, dynamic>> data = <Map<String, dynamic>>[];

  @override
  void initState() {
    super.initState();
    loadData();
  }

  bool get showLoadingDialog => data.isEmpty;

  Future<void> loadData() async {
    final ReceivePort receivePort = ReceivePort();
    await Isolate.spawn(dataLoader, receivePort.sendPort);

    // The 'echo' isolate sends its SendPort as the first message
    final SendPort sendPort = await receivePort.first as SendPort;
    final List<Map<String, dynamic>> msg = await sendReceive(
      sendPort,
      'https://jsonplaceholder.typicode.com/posts',
    );
    setState(() {
      data = msg;
    });
  }

  // The entry point for the isolate
  static Future<void> dataLoader(SendPort sendPort) async {
    // Open the ReceivePort for incoming messages.
    final ReceivePort port = ReceivePort();

    // Notify any other isolates what port this isolate listens to.
    sendPort.send(port.sendPort);
    await for (final dynamic msg in port) {
      final String url = msg[0] as String;
      final SendPort replyTo = msg[1] as SendPort;

      final Uri dataURL = Uri.parse(url);
      final http.Response response = await http.get(dataURL);
      // Lots of JSON to parse
      replyTo.send(jsonDecode(response.body) as List<Map<String, dynamic>>);
    }
  }

  Future<List<Map<String, dynamic>>> sendReceive(SendPort port, String msg) {
    final ReceivePort response = ReceivePort();
    port.send(<dynamic>[msg, response.sendPort]);
    return response.first as Future<List<Map<String, dynamic>>>;
  }

  Widget getBody() {
    if (showLoadingDialog) {
      return getProgressDialog();
    }
    return getListView();
  }

  Widget getProgressDialog() {
    return const Center(child: CircularProgressIndicator());
  }

  ListView getListView() {
    return ListView.builder(
      itemCount: data.length,
      itemBuilder: (context, index) {
        return getRow(index);
      },
    );
  }

  Widget getRow(int index) {
    return Padding(
      padding: const EdgeInsets.all(10.0),
      child: Text('Row ${data[index]['title']}'),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Sample App')),
      body: getBody(),
    );
  }
}
```

### How do I make network requests?

### 如何傳送一個網路請求？

In Xamarin.Forms you would use `HttpClient`.
Making a network call in Flutter is easy
when you use the popular [`http` package][].
This abstracts away a lot of the networking
that you might normally implement yourself,
making it simple to make network calls.

在 Xamarin.Forms 中，你可以使用 `HttpClient`。
在 Flutter 中，你可以使用流行的
[`http` package][] package 輕鬆進行網路呼叫。
它抽象了很多通常你會自己實現的網路功能，
這使其本身在執行網路請求時簡單易用。

To use the `http` package, add it to your dependencies in `pubspec.yaml`:

要使用 `http`，請在 `pubspec.yaml` 檔案中新增依賴：

```yaml
dependencies:
  http: ^0.13.4
```

To make a network request,
call `await` on the `async` function `http.get()`:

如果要發起一個網路請求，
在非同步 (`async`) 方法 `http.get()` 上呼叫 `await` 即可：

<?code-excerpt "lib/data.dart (loadData)"?>
```dart
Future<void> loadData() async {
  final Uri dataURL = Uri.parse(
    'https://jsonplaceholder.typicode.com/posts',
  );
  final http.Response response = await http.get(dataURL);
  setState(() {
    data = jsonDecode(response.body);
  });
}
```

### How do I show the progress for a long-running task?

### 如何為耗時任務顯示進度？

In Xamarin.Forms you would typically create a loading indicator,
either directly in XAML or through a 3rd party plugin such as AcrDialogs.

在 Xamarin.Forms 中常會建立一個載入指示器，
可以直接在 XAML 中建立，也可以透過第三方外掛建立，比如 AcrDialogs。

In Flutter, use a `ProgressIndicator` widget.
Show the progress programmatically by controlling
when it's rendered through a boolean flag.
Tell Flutter to update its state before your long-running task starts,
and hide it after it ends.

在 Flutter 中，我們使用 `ProgressIndicator` widget。
透過程式碼邏輯使用一個布林標記值控制進度條的渲染。
告訴 Flutter 在長時間執行的任務開始之前更新狀態，並在結束後將其隱藏。

In the example below, the build function is separated into three different
functions. If `showLoadingDialog` is `true`
(when `widgets.length == 0`), then render the `ProgressIndicator`.
Otherwise, render the `ListView` with the data returned from a network call.

在下面的例子中，build 方法被拆分成三個不同的方法。
如果 `showLoadingDialog()` 返回 `true`（當 `widgets.length == 0`），
渲染 `ProgressIndicator`。否則，在 `ListView` 裡渲染網路請求返回的資料。

<?code-excerpt "lib/loading.dart"?>
```dart
import 'dart:async';
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  const SampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Sample App',
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List<Map<String, dynamic>> data = <Map<String, dynamic>>[];

  @override
  void initState() {
    super.initState();
    loadData();
  }

  bool get showLoadingDialog => data.isEmpty;

  Future<void> loadData() async {
    final Uri dataURL = Uri.parse(
      'https://jsonplaceholder.typicode.com/posts',
    );
    final http.Response response = await http.get(dataURL);
    setState(() {
      data = jsonDecode(response.body);
    });
  }

  Widget getBody() {
    if (showLoadingDialog) {
      return getProgressDialog();
    }
    return getListView();
  }

  Widget getProgressDialog() {
    return const Center(child: CircularProgressIndicator());
  }

  ListView getListView() {
    return ListView.builder(
      itemCount: data.length,
      itemBuilder: (context, index) {
        return getRow(index);
      },
    );
  }

  Widget getRow(int index) {
    return Padding(
      padding: const EdgeInsets.all(10.0),
      child: Text('Row ${data[index]['title']}'),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Sample App')),
      body: getBody(),
    );
  }
}
```

## Project structure & resources

## 工程結構和資原始檔

### Where do I store my image files?

### 在哪裡放置解析度相關的圖片檔案？

Xamarin.Forms has no platform independent way of storing images,
you had to place images in the iOS `xcasset` folder,
or on Android in the various `drawable` folders.

Xamarin.Forms 沒有獨立於平台的儲存影象的方法，
你必須放置圖片在 iOS 的 `xcasset` 資料夾,
或 Android 的 `drawable` 資料夾中。

While Android and iOS treat resources and assets as distinct items,
Flutter apps have only assets.
All resources that would live in the
`Resources/drawable-*` folders on Android,
are placed in an assets' folder for Flutter.

Android 和 iOS 將資源 (resources) 和資產 (assets) 視為不同的專案，
但是 Flutter 應用只有資產檔案 (assets)。
所有原本在 Android 中應該放在 `res/drawable-*` 資料夾中的資原始檔，
在 Flutter 中都放在一個 assets 資料夾中。

Flutter follows a simple density-based format like iOS.
Assets might be `1.0x`, `2.0x`, `3.0x`, or any other multiplier.
Flutter doesn't have `dp`s but there are logical pixels,
which are basically the same as device-independent pixels.
The so-called [`devicePixelRatio`][] expresses the ratio
of physical pixels in a single logical pixel.

Flutter 遵循一個簡單的類似 iOS 的密度相關的格式。
檔案可以是一倍 (`1x`)、兩倍 (`2x`)、三倍 (`3x`) 或其它的任意倍數。
Flutter 沒有 `dp` 單位，但是有邏輯畫素尺寸，基本和裝置無關的畫素尺寸是一樣的。
名稱為 [`devicePixelRatio`][] 的尺寸表示在單一邏輯畫素標準下裝置物理畫素的比例。

The equivalent to Android's density buckets are:

與 Android 的密度分類別的對照表如下：

| Android density qualifier | Flutter pixel ratio |
|---------------------------|---------------------|
| `ldpi`                    | `0.75x`             |
| `mdpi`                    | `1.0x`              |
| `hdpi`                    | `1.5x`              |
 | `xhdpi`                   | `2.0x`              |
| `xxhdpi`                  | `3.0x`              |
| `xxxhdpi`                 | `4.0x`              |

Assets are located in any arbitrary folder&mdash;
Flutter has no predefined folder structure.
You declare the assets (with location)
in the `pubspec.yaml` file, and Flutter picks them up.

檔案放置於任意資料夾中&mdash;&mdash;Flutter 沒有預先定義好的資料夾結構。
你在 `pubspec.yaml` 檔案中定義檔案（包括位置資訊），Flutter 負責找到它們。

To add a new image asset called `my_icon.png` to our Flutter project,
for example, and deciding that it should live in a folder we
arbitrarily called `images`, you would put the base image (1.0x)
in the `images` folder, and all the other variants in sub-folders
called with the appropriate ratio multiplier:

如果你要向 Flutter 專案中新增一個新的叫 `my_icon.png` 的圖片資源，
並且將其放入我們隨便起名的叫做 `images` 的資料夾中，
你需要將基礎圖片 (1.0x) 放在 `images` 資料夾中，
並將其它倍數的圖片放入以特定倍數作為名稱的子資料夾中：

```
images/my_icon.png       // Base: 1.0x image
images/2.0x/my_icon.png  // 2.0x image
images/3.0x/my_icon.png  // 3.0x image
```

Next, you'll need to declare these images in your `pubspec.yaml` file:

接下來，你需要在 `pubspec.yaml` 檔案中定義這些圖片：

```yaml
assets:
 - images/my_icon.jpeg
```

You can directly access your images in an `Image.asset` widget:

然後你就可以使用 `Image.asset` 存取你的圖片了：

<?code-excerpt "lib/images.dart (ImageAsset)"?>
```dart
@override
Widget build(BuildContext context) {
  return Image.asset('images/my_icon.png');
}
```

or using `AssetImage`:

或者透過 `AssetImage` widget 直接存取：

<?code-excerpt "lib/images.dart (AssetImage)"?>
```dart
@override
Widget build(BuildContext context) {
  return const Image(
    image: AssetImage('images/my_image.png'),
  );
}
```

More detailed information can be found in [Adding assets and images][].

更多詳盡的資訊可以在 [在 Flutter 中新增資產和影象][Adding assets and images] 中找到。

### Where do I store strings? How do I handle localization?

### 字串儲存在哪裡？如何處理本地化？

Unlike .NET which has `resx` files,
Flutter doesn't currently have a dedicated system for handling strings.
At the moment, the best practice is to declare your copy text
in a class as static fields and access them from there. For example:

與 .NET 擁有 `resx` 檔案不同，
Flutter 當下並沒有一個特定的管理字串的資源管理系統。
目前來講，最好的辦法是將字串作為靜態域存放在類中，
並透過類存取它們。例如：

<?code-excerpt "lib/strings.dart (StringsClass)"?>
```dart
class Strings {
  static const String welcomeMessage = 'Welcome To Flutter';
}
```

You can access your strings as such:

接著在你們的程式碼中，你可以這樣存取你的字串：

<?code-excerpt "lib/strings.dart (AccessString)" replace="/return const //g"?>
```dart
Text(Strings.welcomeMessage);
```

By default, Flutter only supports US English for its strings.
If you need to add support for other languages,
include the `flutter_localizations` package.
You might also need to add Dart's [`intl`][]
package to use i10n machinery, such as date/time formatting.

預設情況下，Flutter 只支援美式英語的本地化字串。
如果你需要新增其他語言支援，請引入 `flutter_localizations` 庫。
同時你可能還需要新增 [`intl`][] package 來使用國際化機制，
比如日期和時間的格式化等。

```yaml
dependencies:
  flutter_localizations:
    sdk: flutter
  intl: '^0.17.0'
```

To use the `flutter_localizations` package,
specify the `localizationsDelegates` and
`supportedLocales` on the app widget:

若你想使用 `flutter_localizations` package，
指定應用的 `localizationsDelegates` 和
`supportedLocales`。

<?code-excerpt "lib/strings.dart (Localization)"?>
```dart
import 'package:flutter_localizations/flutter_localizations.dart';

class MyWidget extends StatelessWidget {
  const MyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      localizationsDelegates: <LocalizationsDelegate<dynamic>>[
        // Add app-specific localization delegate[s] here
        GlobalMaterialLocalizations.delegate,
        GlobalWidgetsLocalizations.delegate,
      ],
      supportedLocales: <Locale>[
        Locale('en', 'US'), // English
        Locale('he', 'IL'), // Hebrew
        // ... other locales the app supports
      ],
    );
  }
}
```

The delegates contain the actual localized values,
while the `supportedLocales` defines which locales the app supports.
The above example uses a `MaterialApp`,
so it has both a `GlobalWidgetsLocalizations`
for the base widgets localized values,
and a `MaterialWidgetsLocalizations` for the Material widgets localizations.
If you use `WidgetsApp` for your app, you don't need the latter.
Note that these two delegates contain "default" values,
but you'll need to provide one or more delegates
for your own app's localizable copy,
if you want those to be localized too.

`supportedLocales` 指定了應用支援的語言，
而這些 delegates 則包含了實際的本地化內容。
上面的範例使用了一個 `MaterialApp`，
所以它既使用了處理基礎 widget 本地化的 `GlobalWidgetsLocalizations`，
也使用了處理 Material widget 本地化的 `MaterialWidgetsLocalizations`。
如果你在應用中使用的是 `WidgetApp`，就不需要後者了。
注意，這兩個 delegates 雖然都包含了「預設」值，
但是如果你想要實現本地化，
就必須在本地提供一個或多個 delegates 的實現副本。

When initialized, the `WidgetsApp` (or `MaterialApp`)
creates a [`Localizations`][] widget for you,
with the delegates you specify.
The current locale for the device is always accessible
from the `Localizations` widget from the current context
(in the form of a `Locale` object), or using the [`Window.locale`][].

當初始化的時候，`WidgetsApp`（或 `MaterialApp`）會根據
你提供的 delegates 建立一個 [`Localizations`][] widget。
`Localizations` widget 可以隨時從當前上下文中中獲取裝置所用的語言，
也可以使用 [`Window.locale`][]。

To access localized resources, use the `Localizations.of()` method
to access a specific localizations class that is provided by a given delegate.
Use the [`intl_translation`][] package to extract translatable copy
to [arb][] files for translating, and importing them back into the app
for using them with `intl`.

要使用本地化資源，使用 `Localizations.of()` 方法可以存取提供代理的特定本地化類別。
使用 [`intl_translation`][] 庫解壓翻譯
的副本到 [arb][] 檔案，
然後在應用中透過 `intl` 來參考它們。

For further details on internationalization and localization in Flutter,
see the [internationalization guide][], which has sample code
with and without the `intl` package.

關於 Flutter 中國際化和本地化的細節內容，請參看
[Flutter 應用裡的國際化][internationalization guide]，
裡面包含有使用和不使用 `intl` 庫的範例程式碼。

### Where is my project file?

### 我的專案檔案在哪裡？

In Xamarin.Forms you will have a `csproj` file.
The closest equivalent in Flutter is pubspec.yaml,
which contains package dependencies and various project details.
Similar to .NET Standard,
files within the same directory are considered part of the project.

Xamarin.Forms 中有一個 `csproj` 檔案。
在 Flutter 中最接近的它的是 pubspec.yaml，
其中包含套件相依項和各種專案細節。
與 .NET Standard 類似，相同目錄中的檔案被認為是專案的一部分。

### What is the equivalent of Nuget? How do I add dependencies?

### Nuget 的等價物是什麼？如何新增依賴項？

In the .NET ecosystem, native Xamarin projects and Xamarin.Forms projects
had access to Nuget and the built-in package management system.
Flutter apps contain a native Android app, native iOS app and Flutter app.

在 .NET 生態系統中，原生 Xamarin 專案和
Xamarin.Forms 專案都可以存取 Nuget 和內建的包管理系統。
Flutter 應用程式預設包含一個原生 Android 應用程式、
原生 iOS 應用程式和 Flutter 應用程式。

In Android, you add dependencies by adding to your Gradle build script.
In iOS, you add dependencies by adding to your `Podfile`.

在 Android 中，你可以透過向 Gradle 新增建構指令碼來新增依賴項。
而在 iOS 中，你可以透過新增到 `Podfile` 來新增依賴項。

Flutter uses Dart's own build system, and the Pub package manager.
The tools delegate the building of the native Android and iOS wrapper apps
to the respective build systems.

Flutter 使用 Dart 自己的建構系統和 Pub 包管理器。
這些工具將原生 Android 和 iOS 封裝應用程式的建構委託給各自的建構系統。

In general, use `pubspec.yaml` to declare
external dependencies to use in Flutter.
A good place to find Flutter packages is on [pub.dev][].

通常你會在 Flutter 中使用 `pubspec.yaml` 來宣告外部依賴。
你可以透過 [pub.dev][] 來查詢一些優秀的 Flutter 第三方套件。

## Application lifecycle

## 應用程式生命週期

### How do I listen to application lifecycle events?

### 如何偵聽應用程式的生命週期事件？

In Xamarin.Forms, you have an `Application`
that contains `OnStart`, `OnResume` and `OnSleep`.
In Flutter, you can instead listen to similar lifecycle events
by hooking into the `WidgetsBinding` observer and listening to
the `didChangeAppLifecycleState()` change event.

在 Xamarin.Forms 中，你會有一個包含
`OnStart`、`OnResume` 和 `OnSleep` 的 `Application`。
在 Flutter 中，你可以透過在 `WidgetsBinding`
的監聽器 (observer) 中新增監聽，
也可以透過監聽 `didChangeAppLifecycleState()` 事件，
來實現相應的功能。

The observable lifecycle events are:

可監聽的生命週期事件有：

**`inactive`**
: The application is in an inactive state and is not receiving user input.
  This event is iOS only.

**`inactive`**
: 應用當前處於不活躍狀態，不接收使用者輸入事件。
  這個事件只在 iOS 上有效。

**`paused`**
: The application is not currently visible to the user,
  is not responding to user input, but is running in the background.

**`paused`**
: 應用當前處於使用者不可見狀態，不接收使用者輸入事件，但仍在後台執行。

**`resumed`**
: The application is visible and responding to user input.

**`resumed`**
: 應用可見，同時響應使用者輸入。

**`suspending`**
: The application is suspended momentarily.
  This event is Android only.

**`suspending`**
: 應用被掛起。這個事件只在 Android 上有效。

For more details on the meaning of these states,
see the [`AppLifecycleStatus` documentation][].

有關這些狀態的含義的更多細節，可參考
[`AppLifecycleStatus` 文件][`AppLifecycleStatus` documentation]。

[`AppLifecycleStatus` documentation]: {{site.api}}/flutter/dart-ui/AppLifecycleState.html

## Layouts

## 佈局

### What is the equivalent of a StackLayout?

### 什麼東西與 StackLayout 等效?

In Xamarin.Forms you can create a `StackLayout`
with an `Orientation` of horizontal or vertical.
Flutter has a similar approach,
however you would use the `Row` or `Column` widgets.

在 Xamarin.Forms 中，可以建立一個帶水平或垂直方向
`Orientation` 的 `StackLayout` 。
Flutter 也有類似的方法，不過你將使用的是
`Row` 或 `Column` widget。

If you notice the two code samples are identical
except the `Row` and `Column` widget.
The children are the same and this feature
can be exploited to develop rich layouts
that can change overtime with the same children.

你可能會注意到除了 `Row` 和 `Column` widget 之外，
這兩個程式碼範例是相同的。
這些子元素是相同的，可以利用這個特性開發豐富的佈局，
這些佈局可以隨著時間的推移而改變。

<?code-excerpt "lib/layouts.dart (Row)"?>
```dart
@override
Widget build(BuildContext context) {
  return Row(
    mainAxisAlignment: MainAxisAlignment.center,
    children: const <Widget>[
      Text('Row One'),
      Text('Row Two'),
      Text('Row Three'),
      Text('Row Four'),
    ],
  );
}
```

<?code-excerpt "lib/layouts.dart (Column)"?>
```dart
@override
Widget build(BuildContext context) {
  return Column(
    mainAxisAlignment: MainAxisAlignment.center,
    children: const <Widget>[
      Text('Column One'),
      Text('Column Two'),
      Text('Column Three'),
      Text('Column Four'),
    ],
  );
```

### What is the equivalent of a Grid?

### 什麼東西與網格（Grid） 等價?

The closest equivalent of a `Grid` would be a `GridView`.
This is much more powerful than what you are used to in Xamarin.Forms.
A `GridView` provides automatic scrolling when the
content exceeds its viewable space.

與 `Grid` 最接近的對等項是 `GridView`。
這比你在 Xamarin.Forms 中習慣使用的功能強大得多。
`GridView` 在內容超出其可視空間時自動滾動。

<?code-excerpt "lib/layouts.dart (Grid)"?>
```dart
@override
Widget build(BuildContext context) {
  return GridView.count(
    // Create a grid with 2 columns. If you change the scrollDirection to
    // horizontal, this would produce 2 rows.
    crossAxisCount: 2,
    // Generate 100 widgets that display their index in the list.
    children: List<Widget>.generate(
      100,
      (index) {
        return Center(
          child: Text(
            'Item $index',
            style: Theme.of(context).textTheme.headlineMedium,
          ),
        );
      },
    ),
  );
}
```

You might have used a `Grid` in Xamarin.Forms
to implement widgets that overlay other widgets.
In Flutter, you accomplish this with the `Stack` widget.

你可能在 Xamarin.Forms 中使用 `Grid` 來實現覆蓋其他 widget 的 widget。
在 Flutter 中，你可以使用 `Stack` widget 來完成這一操作。

This sample creates two icons that overlap each other.

這個範例建立了兩個相互重疊的圖示。

<?code-excerpt "lib/layouts.dart (Stack)"?>
```dart
@override
Widget build(BuildContext context) {
  return Stack(
    children: const <Widget>[
      Icon(
        Icons.add_box,
        size: 24.0,
        color: Colors.black,
      ),
      Positioned(
        left: 10.0,
        child: Icon(
          Icons.add_circle,
          size: 24.0,
          color: Colors.black,
        ),
      ),
    ],
  );
}
```

### What is the equivalent of a ScrollView?

### 有什麼等同於 ScrollView ？

In Xamarin.Forms, a `ScrollView` wraps around a `VisualElement`,
and if the content is larger than the device screen, it scrolls.

在 Xamarin.Forms 中，`ScrollView` 封裝了 `VisualElement`，
如果內容大於裝置螢幕，它就會滾動。

In Flutter, the closest match is the `SingleChildScrollView` widget.
You simply fill the Widget with the content that you want to be scrollable.

在 Flutter 中，最接近的是 `SingleChildScrollView` widget。
你只需用想要可滾動的內容來填充 widget。

<?code-excerpt "lib/layouts.dart (ScrollView)"?>
```dart
@override
Widget build(BuildContext context) {
  return const SingleChildScrollView(
    child: Text('Long Content'),
  );
}
```

If you have many items you want to wrap in a scroll,
even of different `Widget` types, you might want to use a `ListView`.
This might seem like overkill, but in Flutter this is
far more optimized and less intensive than a Xamarin.Forms `ListView`,
which is backing on to platform specific controls.

如果你想在捲軸中包含許多項，即使是不同的`Widget`型別，
也可以使用 `ListView`。
這可能看起來有點大材小用，但在 Flutter 中，
它比 Xamarin.Forms 的回到平台特定控制項的 `ListView`
更為最佳化且靈活。

<?code-excerpt "lib/layouts.dart (ListView)"?>
```dart
@override
Widget build(BuildContext context) {
  return ListView(
    children: const <Widget>[
      Text('Row One'),
      Text('Row Two'),
      Text('Row Three'),
      Text('Row Four'),
    ],
  );
}
```

### How do I handle landscape transitions in Flutter?

### 在 Flutter 中如何處理橫向過渡 ?

Landscape transitions can be handled automatically by setting the
`configChanges` property in the AndroidManifest.xml:

透過在 AndroidManifest.xml 中設定 `configChanges` 屬性，可以自動處理橫向轉換。

```xml
<activity android:configChanges="orientation|screenSize" />
```

## Gesture detection and touch event handling

## 手勢檢測和觸控事件處理

### How do I add GestureRecognizers to a widget in Flutter?

### 如何在 Flutter 中向 widget 新增手勢識別器?

In Xamarin.Forms, `Element`s might contain a click event you can attach to.
Many elements also contain a `Command` that is tied to this event.
Alternatively you would use the `TapGestureRecognizer`.
In Flutter there are two very similar ways:

在 Xamarin.Forms 中，`Element` 可能包含一個可供附加 (attach) 的單擊事件。
許多元素還包含一個與此事件關聯的 `Command`。
你也可以使用 `TapGestureRecognizer`。
而在 Flutter 中有兩種非常相似的方式：

1. If the widget supports event detection, pass a function to it and
   handle it in the function. For example, the ElevatedButton has an
   `onPressed` parameter:

   如果 Widget 支援事件監聽，那麼向它傳入一個方法並在方法中處理事件。
   例如，RaisedButton 有一個 `onPressed` 引數：

   <?code-excerpt "lib/gestures.dart (ElevatedButton)"?>
   ```dart
   @override
   Widget build(BuildContext context) {
     return ElevatedButton(
       onPressed: () {
         developer.log('click');
       },
       child: const Text('Button'),
     );
   }
   ```

2. If the widget doesn't support event detection, wrap the
   widget in a `GestureDetector` and pass a function
   to the `onTap` parameter.

   如果 Widget 不支援事件監聽，將 Widget 包裝進一個 GestureDetector 中
   並向 `onTap` 引數傳入一個方法。

   <?code-excerpt "lib/gestures.dart (GestureDetector)"?>
   ```dart
   class SampleApp extends StatelessWidget {
     const SampleApp({super.key});

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         body: Center(
           child: GestureDetector(
             onTap: () {
               developer.log('tap');
             },
             child: const FlutterLogo(size: 200.0),
           ),
         ),
       );
     }
   }
   ```

### How do I handle other gestures on widgets?

### 我如何處理 widget 上的其他手勢？

In Xamarin.Forms you would add a `GestureRecognizer` to the `View`.
You would normally be limited to `TapGestureRecognizer`,
`PinchGestureRecognizer`, `PanGestureRecognizer`, `SwipeGestureRecognizer`,
`DragGestureRecognizer` and `DropGestureRecognizer` unless you built your own.

在 Xamarin.Forms 中你可以在 `VisualElement`
中新增一個 `GestureRecognizer`。
你通常只能使用
`TapGestureRecognizer`、`PinchGestureRecognizer`、`PanGestureRecognizer`、，
`SwipeGestureRecognizer`、`DragGestureRecognizer` 和 `DropGestureRecognizer`，
除非你建構了自己的實現。

In Flutter, using the GestureDetector,
you can listen to a wide range of Gestures such as:

在 Flutter 中，使用手勢檢測器，你可以監聽到各種各樣的手勢，比如:

* Tap

  單擊

`onTapDown`
: A pointer that might cause a tap
  has contacted the screen at a particular location.

`onTapDown`
: 當指尖在特定位置與螢幕接觸產生點選事件。

`onTapUp`
: A pointer that triggers a tap
  has stopped contacting the screen at a particular location.

`onTapUp`
: 當指尖觸發的點選事件已經停止在特定位置與螢幕接觸。

`onTap`
: A tap has occurred.

`onTap`
: 一個點選事件已經發生。

`onTapCancel`
: The pointer that previously triggered the `onTapDown`
  won't cause a tap.

`onTapCancel`
: 觸發了 `onTapDown` 事件之後的指尖沒有導致點選事件。

* Double tap

  雙擊

`onDoubleTap`
: The user tapped the screen at the same location twice
  in quick succession.

`onDoubleTap`
: 使用者在同一位置連續快速點選螢幕兩次。

* Long press

  長按

`onLongPress`
: A pointer has remained in contact with the screen
  at the same location for a long period of time.

`onLongPress`
: 指尖長時間保持與螢幕在同一位置的接觸。

* Vertical drag

  垂直拖動

`onVerticalDragStart`
: A pointer has contacted the screen and might begin to move vertically.

`onVerticalDragStart`
: 指尖與螢幕接觸後，可能開始垂直移動。

`onVerticalDragUpdate`
: A pointer in contact with the screen
  has moved further in the vertical direction.

`onVerticalDragUpdate`
: 指尖與螢幕接觸並在垂直方向上移動得更遠。

`onVerticalDragEnd`
: A pointer that was previously in contact with the
  screen and moving vertically is no longer in contact
  with the screen and was moving at a specific velocity
  when it stopped contacting the screen.

`onVerticalDragEnd`
: 指尖在之前與螢幕接觸並垂直移動，
  當不再與螢幕接觸時觸發這個事件。
  當它停止與螢幕接觸時，它會以特定的速度移動。

* Horizontal drag

  水平拖動

`onHorizontalDragStart`
: A pointer has contacted the screen and might begin to move horizontally.

`onHorizontalDragStart`
: 指尖與螢幕接觸，開始水平移動時觸發。

`onHorizontalDragUpdate`
: A pointer in contact with the screen
  has moved further in the horizontal direction.

`onHorizontalDragUpdate`
: 指尖與螢幕接觸並在水平方向上移動得更遠。

`onHorizontalDragEnd`
: A pointer that was previously in contact with the
  screen and moving horizontally is no longer in contact
  with the screen and was moving at a specific velocity
  when it stopped contacting the screen.

`onHorizontalDragEnd`
: 指尖在之前與螢幕接觸並水平移動，當不再與螢幕接觸時會觸發這個事件。
  當它停止與螢幕接觸時，
  它正在以特定的速度移動。

The following example shows a `GestureDetector`
that rotates the Flutter logo on a double tap:

下面的例子展示了一個實現了雙擊旋轉 Flutter 標誌的 `GestureDetector`：

<?code-excerpt "lib/gestures.dart (RotatingFlutterDetector)"?>
```dart
class RotatingFlutterDetector extends StatefulWidget {
  const RotatingFlutterDetector({super.key});

  @override
  State<RotatingFlutterDetector> createState() =>
      _RotatingFlutterDetectorState();
}

class _RotatingFlutterDetectorState extends State<RotatingFlutterDetector>
    with SingleTickerProviderStateMixin {
  late final AnimationController controller;
  late final CurvedAnimation curve;

  @override
  void initState() {
    super.initState();
    controller = AnimationController(
      duration: const Duration(milliseconds: 2000),
      vsync: this,
    );
    curve = CurvedAnimation(parent: controller, curve: Curves.easeIn);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: GestureDetector(
          onDoubleTap: () {
            if (controller.isCompleted) {
              controller.reverse();
            } else {
              controller.forward();
            }
          },
          child: RotationTransition(
            turns: curve,
            child: const FlutterLogo(size: 200.0),
          ),
        ),
      ),
    );
  }
}
```

## Listviews and adapters

## 列表檢視和介面卡

### What is the equivalent to a ListView in Flutter?

### 在 Flutter 中，與列表檢視等價的是什麼？

The equivalent to a `ListView` in Flutter is … a `ListView`!

在 Flutter 中與 `ListView` 等價的是……一個 `ListView`！

In a Xamarin.Forms `ListView`, you create a `ViewCell`
and possibly a `DataTemplateSelector`and pass it into the `ListView`,
which renders each row with what your
`DataTemplateSelector` or `ViewCell` returns.
However, you often have to make sure you turn on Cell Recycling
otherwise you will run into memory issues and slow scrolling speeds.

在一個 Xamarin.Forms 的 `ListView` 中，
你可以建立一個 `ViewCell` 或者 `DataTemplateSelector`，
並將其傳遞到 `ListView` 中，該檢視將用你的 `DataTemplateSelector`
或者 `ViewCell` 的返回資料渲染每一行。
但是，你通常必須確保開啟單元格回收，否則會遇到記憶體問題和會使滾動速度變慢。

Due to Flutter's immutable widget pattern,
you pass a list of widgets to your `ListView`,
and Flutter takes care of making sure that scrolling is fast and smooth.

由於 Flutter 中 widget 的不可變特性，
你需要向 `ListView` 傳遞一個 widget 列表，
Flutter 會確保滾動快速而流暢。

<?code-excerpt "lib/listview.dart"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  /// This widget is the root of your application.
  const SampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Sample App',
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatelessWidget {
  const SampleAppPage({super.key});

  List<Widget> _getListData() {
    return List<Widget>.generate(
      100,
      (index) => Padding(
        padding: const EdgeInsets.all(10.0),
        child: Text('Row $index'),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Sample App')),
      body: ListView(children: _getListData()),
    );
  }
}
```

### How do I know which list item has been clicked?

### 如何確定列表中被點選的元素？

In Xamarin.Forms, the ListView has an `ItemTapped` method
to find out which item was clicked.
There are many other techniques you might have used
such as checking when `SelectedItem` or `EventToCommand`
behaviors change.

在 Xamarin.Forms 中，ListView 擁有一個
`ItemTapped` 方法能找出哪個列表項被單擊了。
還有其他一些方法，比如檢查 `SelectedItem`
或 `EventToCommand` 的行為何時會發生更改。

In Flutter, use the touch handling provided by the passed-in widgets.

而在 Flutter 裡，需要透過 widget 傳遞進來的 touch 響應處理來實現。

<?code-excerpt "lib/listview_item_clicked.dart"?>
```dart
import 'dart:developer' as developer;
import 'package:flutter/material.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  const SampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Sample App',
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List<Widget> _getListData() {
    return List<Widget>.generate(
      100,
      (index) => GestureDetector(
        onTap: () {
          developer.log('Row $index tapped');
        },
        child: Padding(
          padding: const EdgeInsets.all(10.0),
          child: Text('Row $index'),
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Sample App')),
      body: ListView(children: _getListData()),
    );
  }
}
```

### How do I update a ListView dynamically?

### 如何動態更新 ListView ?

In Xamarin.Forms, if you bound the
`ItemsSource` property to an `ObservableCollection`,
you would just update the list in your ViewModel.
Alternatively, you could assign a new `List` to the `ItemSource` property.

在 Xamarin.Forms 中，如果將 `ItemsSource` 屬性繫結到一個
`ObservableCollection`，就只需要更新檢視模型中的列表。
另一種方法是，你可以給屬性 `ItemsSource` 分配一個新的 `List 。

In Flutter, things work a little differently.
If you update the list of widgets inside a `setState()` method,
you would quickly see that your data did not change visually.
This is because when `setState()` is called,
the Flutter rendering engine looks at the widget tree
to see if anything has changed.
When it gets to your `ListView`, it performs a `==` check,
and determines that the two `ListView`s are the same.
Nothing has changed, so no update is required.

在 Flutter 中，情況略有不同。
如果你在 `setState()` 中更新了 widget 列表，
你會發現展示的資料並不會立刻更新。
這是因為當 `setState()` 被呼叫時，
Flutter 的渲染引擎回去檢索 widget 樹是否有改變。
當它獲取到 `ListView`，會進行 `==` 判斷，
然後發現兩個 `ListView` 是相等的。
此時沒有改變，也就不會進行更新。

For a simple way to update your `ListView`,
create a new `List` inside of `setState()`,
and copy the data from the old list to the new list.
While this approach is simple, it is not recommended for large data sets,
as shown in the next example.

一個更新 `ListView` 的簡單方法就是，在 `setState()` 建立一個新的 `List`，
然後複製舊列表中的所有資料到新列表。
這樣雖然簡單，但是像下面範例一樣資料量很大時，並不推薦這樣做。

<?code-excerpt "lib/dynamic_listview.dart"?>
```dart
import 'dart:developer' as developer;
import 'package:flutter/material.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  /// This widget is the root of your application.
  const SampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Sample App',
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List<Widget> widgets = <Widget>[];

  @override
  void initState() {
    super.initState();
    for (int i = 0; i < 100; i++) {
      widgets.add(getRow(i));
    }
  }

  Widget getRow(int index) {
    return GestureDetector(
      onTap: () {
        setState(() {
          widgets = List<Widget>.from(widgets);
          widgets.add(getRow(widgets.length));
          developer.log('Row $index');
        });
      },
      child: Padding(
        padding: const EdgeInsets.all(10.0),
        child: Text('Row $index'),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Sample App')),
      body: ListView(children: widgets),
    );
  }
}
```

The recommended, efficient, and effective way to build a list
uses a `ListView.Builder`.
This method is great when you have a dynamic list
or a list with very large amounts of data.
This is essentially the equivalent of RecyclerView on Android,
which automatically recycles list elements for you:

一個推薦的、高效且有效的方法就是使用 `ListView.Builder` 來建構列表。
當你的資料量很大，且需要建構動態列表時，這個方法會非常好用。
這基本上相當於 Android 上的 `RecyclerView`，它會自動回收列表元素：

<?code-excerpt "lib/listview_builder.dart"?>
```dart
import 'dart:developer' as developer;
import 'package:flutter/material.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  /// This widget is the root of your application.
  const SampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Sample App',
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List<Widget> widgets = [];

  @override
  void initState() {
    super.initState();
    for (int i = 0; i < 100; i++) {
      widgets.add(getRow(i));
    }
  }

  Widget getRow(int index) {
    return GestureDetector(
      onTap: () {
        setState(() {
          widgets.add(getRow(widgets.length));
          developer.log('Row $index');
        });
      },
      child: Padding(
        padding: const EdgeInsets.all(10.0),
        child: Text('Row $index'),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Sample App')),
      body: ListView.builder(
        itemCount: widgets.length,
        itemBuilder: (context, index) {
          return getRow(index);
        },
      ),
    );
  }
}
```

Instead of creating a `ListView`, create a `ListView.builder`
that takes two key parameters: the initial length of the list,
and an item builder function.

與建立 `ListView` 不同，建立 `ListView.Builder` 需要兩個關鍵引數：
初始化列表長度和 item 建構函式。

The item builder function is similar to the `getView` function
in an Android adapter; it takes a position,
and returns the row you want rendered at that position.

Item 建構函式類似於 Android 介面卡中的 `getView` 函式；
它接受一個位置，並返回你希望的在該位置呈現的行。

Finally, but most importantly, notice that the `onTap()` function
doesn't recreate the list anymore, but instead adds to it.

最後且最重要的是，要注意 `onTap()` 函式不再重新建立列表，
而是用 `.add` 新增給它的。

For more information, see
[Write your first Flutter app, part 1][]
and [Write your first Flutter app, part 2][].

更多資訊，請存取
[編寫你的第一個 Flutter 應用程式，第 1 部分][Write your first Flutter app, part 1]
和 [編寫你的第一個 Flutter 應用程式，第 2 部分][Write your first Flutter app, part 2]。

## Working with text

## 文字處理

### How do I set custom fonts on my text widgets?

### 如何在文字 `Text` widget 上設定自訂字型?

In Xamarin.Forms, you would have to add a custom font in each native project.
Then, in your `Element` you would assign this font name
to the `FontFamily` attribute using `filename#fontname`
and just `fontname` for iOS.

在 Xamarin.Forms 中，你必須在每個原生專案中新增自訂字型。
然後在你的 `Element` 中，你會使用 `filename#fontname`
給 `FontFamily` 屬性分配字型名，在 iOS 中使用 `fontname` 。

In Flutter, place the font file in a folder and reference it
in the `pubspec.yaml` file, similar to how you import images.

在 Flutter 中，你可以將字型檔案放在一個資料夾中，
並在 `pubspec.yaml` 中參考它，這跟匯入影象的方式類似。

```yaml
fonts:
  - family: MyCustomFont
    fonts:
      - asset: fonts/MyCustomFont.ttf
      - style: italic
```

Then assign the font to your `Text` widget:

然後將字型賦值給你的 `Text` Widget：

<?code-excerpt "lib/strings.dart (CustomFont)"?>
```dart
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(title: const Text('Sample App')),
    body: const Center(
      child: Text(
        'This is a custom font text',
        style: TextStyle(fontFamily: 'MyCustomFont'),
      ),
    ),
  );
}
```

### How do I style my text widgets?

### 如何更改 · Widget 的樣式？

Along with fonts, you can customize other styling elements on a `Text` widget.
The style parameter of a `Text` widget takes a `TextStyle` object,
where you can customize many parameters, such as:

除了字型，你還可以自訂 `Text` Widget 的其它樣式元素。
`Text` Widget 的樣式引數接收一個 `TextStyle` 物件，
你可以在這個物件裡自訂很多引數，例如：

* `color`
* `decoration`
* `decorationColor`
* `decorationStyle`
* `fontFamily`
* `fontSize`
* `fontStyle`
* `fontWeight`
* `hashCode`
* `height`
* `inherit`
* `letterSpacing`
* `textBaseline`
* `wordSpacing`

## Form input

## 表單輸入

### How do I retrieve user input?

### 如何檢索使用者輸入？

Xamarin.Forms `element`s allow you to directly query the `element`
to determine the state of its properties,
or whether it's bound to a property in a `ViewModel`.

Xamarin.Forms 的 `element` 允許你直接查詢 `element` 來確定它的任何屬性的狀態，
或者它被繫結到 `ViewModel` 中的屬性。

Retrieving information in Flutter is handled by specialized widgets
and is different from how you are used to.
If you have a `TextField`or a `TextFormField`,
you can supply a [`TextEditingController`][]
to retrieve user input:

在 Flutter 中檢索資訊是由專門的 widget 處理的，這是跟原來的習慣不同的。
如果你有一個 `TextField` 或 `TextFormField`，你可以提供一個 
[`TextEditingController`][] 來檢索使用者輸入:

<?code-excerpt "lib/form.dart"?>
```dart
import 'package:flutter/material.dart';

class MyForm extends StatefulWidget {
  const MyForm({super.key});

  @override
  State<MyForm> createState() => _MyFormState();
}

class _MyFormState extends State<MyForm> {
  /// Create a text controller and use it to retrieve the current value
  /// of the TextField.
  final TextEditingController myController = TextEditingController();

  @override
  void dispose() {
    // Clean up the controller when disposing of the widget.
    myController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Retrieve Text Input')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: TextField(controller: myController),
      ),
      floatingActionButton: FloatingActionButton(
        // When the user presses the button, show an alert dialog with the
        // text that the user has typed into our text field.
        onPressed: () {
          showDialog(
            context: context,
            builder: (context) {
              return AlertDialog(
                // Retrieve the text that the user has entered using the
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

You can find more information and the full code listing in
[Retrieve the value of a text field][],
from the [Flutter cookbook][].

你可以在 [Flutter 實用課程][Flutter cookbook] 中的
[獲取文字框的輸入值][Retrieve the value of a text field]
找到更多的資訊和完整的程式碼清單。

### What is the equivalent of a Placeholder on an Entry?

### 在入口的佔位符 (Placeholder) 與什麼等價？

In Xamarin.Forms, some `Elements` support a `Placeholder` property
that you can assign a value to. For example:

在 Xamarin.Forms 中，一些 `Element` 支援設定 `Placeholder` 屬性。如：

```xml
  <Entry Placeholder="This is a hint">
```

In Flutter, you can easily show a "hint" or a placeholder text
for your input by adding an `InputDecoration` object
to the `decoration` constructor parameter for the text widget.

在 Flutter 中，透過在文字 widget 的裝飾器建構函式引數中新增
`InputDecoration` 物件，可以輕鬆地為輸入顯示「提示」或佔位符文字。

<?code-excerpt "lib/input_decoration.dart (HintText)" replace="/child: //g"?>
```dart
TextField(
  decoration: InputDecoration(hintText: 'This is a hint'),
),
```

### How do I show validation errors?

### 如何顯示驗證錯誤的資訊？

With Xamarin.Forms, if you wished to provide a visual hint of a
validation error, you would need to create new properties and
`VisualElement`s surrounding the `Element`s that had validation errors.

使用 Xamarin.Forms 時，如果你希望提供驗證錯誤的視覺化提示，
則需要建立新屬性和 `VisualElement` 來包圍具有驗證錯誤的元素。

In Flutter, you pass through an InputDecoration object to the
decoration constructor for the text widget.

在 Flutter 中，我們將 `InputDecoration` 物件傳遞給文字 widget 的裝飾器建構函式。

However, you don't want to start off by showing an error.
Instead, when the user has entered invalid data,
update the state, and pass a new `InputDecoration` object.

然而，你並不想一開始就顯示錯誤資訊。相反，當用戶輸入了無效的資訊後，
更新狀態並傳入一個新的 `InputDecoration` 物件。

<?code-excerpt "lib/validation.dart"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  /// This widget is the root of your application.
  const SampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Sample App',
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  String? _errorText;

  String? _getErrorText() {
    return _errorText;
  }

  bool isEmail(String em) {
    const String emailRegexp =
        r'^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|'
        r'(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|'
        r'(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$';
    final RegExp regExp = RegExp(emailRegexp);
    return regExp.hasMatch(em);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Sample App')),
      body: Center(
        child: TextField(
          onSubmitted: (text) {
            setState(() {
              if (!isEmail(text)) {
                _errorText = 'Error: This is not an email';
              } else {
                _errorText = null;
              }
            });
          },
          decoration: InputDecoration(
            hintText: 'This is a hint',
            errorText: _getErrorText(),
          ),
        ),
      ),
    );
  }
}
```

## Flutter plugins

## Flutter 外掛

## Interacting with hardware, third party services, and the platform

## 與硬體、第三方服務和平台互動

### How do I interact with the platform, and with platform native code?

### 應該如何與平台以及平台原生程式碼互動?

Flutter doesn't run code directly on the underlying platform;
rather, the Dart code that makes up a Flutter app is run natively
on the device, "sidestepping" the SDK provided by the platform.
That means, for example, when you perform a network request in Dart,
it runs directly in the Dart context.
You don't use the Android or iOS APIs
you normally take advantage of when writing native apps.
Your Flutter app is still hosted in a native app's
`ViewController` or `Activity` as a view,
but you don't have direct access to this, or the native framework.

Flutter 不直接在底層平臺上執行程式碼。
相反，構成一個 Flutter 應用程式的 Dart 程式碼
是在裝置上原生執行的，「繞開」了平台提供的 SDK。
這意味著當你在 Dart 中執行網路請求時，
它將直接執行在 Dart 上下文中。
在編寫原生應用程式時，你通常不會使用 Android 或 iOS 的 API。
Flutter 應用程式仍然作為檢視駐留在原生應用程式的
`ViewController` 或 `Activity` 中，但你不能直接存取這個或原生框架。

This doesn't mean Flutter apps can't interact with those native APIs,
or with any native code you have. Flutter provides [platform channels][]
that communicate and exchange data with the
`ViewController` or `Activity` that hosts your Flutter view.
Platform channels are essentially an asynchronous messaging mechanism
that bridges the Dart code with the host `ViewController`
or `Activity` and the iOS or Android framework it runs on.
You can use platform channels to execute a method on the native side,
or to retrieve some data from the device's sensors, for example.

這並不意味著 Flutter 應用程式不能與這些原生 API 或你自己的任何原生程式碼互動。
Flutter 提供了 [平台通道][platform channels]
用於與託管 Flutter 檢視的 `ViewController` 或 `Activity` 通訊和交換資料。
平台通道本質上是一個非同步訊息傳遞機制，
它將 Dart 程式碼與 `ViewController` 或 `Activity` 宿主
以及它所執行的 iOS 或 Android 框架橋接起來。
例如，你可以使用平台通道在原生端執行一個方法，或者從裝置的感測器檢索一些資料。

In addition to directly using platform channels,
you can use a variety of pre-made [plugins][]
that encapsulate the native and Dart code for a specific goal.
For example, you can use a plugin to access
the camera roll and the device camera directly from Flutter,
without having to write your own integration.
Plugins are found on [pub.dev][],
Dart and Flutter's open source package repository.
Some packages might support native integrations on iOS,
or Android, or both.

除了直接使用平台通道外，你還可以使用各種預製 [外掛][plugins]，
它們封裝了針對特定目標的原生程式碼和 Dart 程式碼。
例如，你可以使用外掛直接從 Flutter 存取裝置相機，而無需編寫自己的整合。
外掛可以在 [pub.dev][]、Dart 和 Flutter 的開源 package 儲存庫中找到。
有些包可能支援 iOS 上的本地整合，有些支援 Android，還有兩者都兼而有之的。

If you can't find a plugin on pub.dev that fits your needs,
you can [write your own][], and [publish it on pub.dev][].

如果在 Pub 上找不到適合你需求的外掛，你可以
[編寫自己的外掛][write your own] 並
[在 Pub 上釋出][publish it on pub.dev]。

### How do I access the GPS sensor?

### 如何存取 GPS 感測器?

Use the [`geolocator`][] community plugin.

使用 [`geolocator`][] 社群外掛.

### How do I access the camera?

### 如何存取照相機？

The [`camera`][] plugin is popular for accessing the camera.

[`camera`][] 外掛被常用於相機功能的使用。

### How do I log in with Facebook?

### 如何透過 Facebook 登入？

To log in with Facebook, use the

[`flutter_facebook_login`][] community plugin.

使用 [`flutter_facebook_login`][] 社群外掛實現
Facebook 登入功能。

### How do I use Firebase features?

### 如何使用 Firebase 特性？

Most Firebase functions are covered by
[first party plugins][].
These plugins are first-party integrations, maintained by the Flutter team:

[官方外掛][first party plugins] 提供了 Firebase 的大多數功能。
這些外掛都是由 Flutter 團隊維護的官方整合外掛：

* [`firebase_admob`][] for Firebase AdMob

  [`firebase_admob`][] 提供 Firebase AdMob 功能

* [`firebase_analytics`][] for Firebase Analytics

  [`firebase_analytics`][] 提供 Firebase Analytics 功能

* [`firebase_auth`][] for Firebase Auth

  [`firebase_auth`][] 提供 Firebase Auth 功能

* [`firebase_database`][] for Firebase RTDB

  [`firebase_database`][] 提供 Firebase RTDB 功能

* [`firebase_storage`][] for Firebase Cloud Storage

  [`firebase_storage`][] 提供 Firebase Cloud Storage 功能

* [`firebase_messaging`][] for Firebase Messaging (FCM)

  [`firebase_messaging`][] 提供 Firebase Messaging (FCM) 功能

* [`flutter_firebase_ui`][] for quick Firebase Auth integrations (Facebook, Google, Twitter and email)

  [`flutter_firebase_ui`][] 提供快速的 Firebase Auth 整合功能 (Facebook, Google, Twitter 和 email)

* [`cloud_firestore`][] for Firebase Cloud Firestore

  [`cloud_firestore`][] 提供 Firebase Cloud Firestore 功能

You can also find some third-party Firebase plugins on pub.dev
that cover areas not directly covered by the first-party plugins.

你可以在 [Pub](https://pub.flutter-io.cn/flutter) 網站上查詢一些官方外掛
沒有直接支援的功能的第三方 Firebase 外掛。

### How do I build my own custom native integrations?

### 如何建構自訂的原生整合？

If there is platform-specific functionality that Flutter
or its community plugins are missing,
you can build your own following the
[developing packages and plugins][] page.

如果有 Flutter 官方或社群第三方外掛沒有涵蓋的平台特定的功能，
你可以參考 [開發套件和外掛][developing packages and plugins]
文件建立自己的外掛。

Flutter's plugin architecture, in a nutshell,
is much like using an Event bus in Android:
you fire off a message and let the receiver process and emit a result
back to you. In this case, the receiver is code running on the native side
on Android or iOS.

簡單地說，Flutter 的外掛架構很像在 Android 中使用事件匯流排：
你發出一條訊息，讓接收方處理並向你發回一個結果。
在這種情況下，接收方是執行在 Android 或 iOS 上的原生程式碼。

## Themes (Styles)

## 主題（樣式）

### How do I theme my app?

### 如何對應用使用主題？

Flutter comes with a beautiful, built-in implementation of Material Design,
which handles much of the styling and theming needs
that you would typically do.

Flutter 附帶了一個內建的漂亮的 Material Design 實現，
它處理了許多你通常會做的樣式和主題需求。

Xamarin.Forms does have a global `ResourceDictionary`
where you can share styles across your app.
Alternatively, there is Theme support currently in preview.

Xamarin.Forms 確實有一個全域的 `ResourceDictionary`，
可以為你的應用程式共享樣式。另外，預覽版目前還支援主題。

In Flutter, you declare themes in the top level widget.

在 Flutter 中，你可以在最最上層 widget 中宣告主題。

To take full advantage of Material Components in your app,
you can declare a top level widget `MaterialApp`
as the entry point to your application.
`MaterialApp` is a convenience widget
that wraps a number of widgets that are commonly required
for applications implementing Material Design.
It builds upon a `WidgetsApp` by adding Material-specific functionality.

為了在應用中利用好 Material 元件，
你可以在應用中宣告一個最上層 Widget `MaterialApp` 作為入口。
MaterialApp 是一個包裝了一系列 Widget 的為你給予便利的 Widget，
而這些 Widget 通常是實現 Material Design 的應用所必須的。
它基於 WidgetsApp 並添加了 Material 相關的功能。

You can also use a `WidgetsApp` as your app widget,
which provides some of the same functionality,
but is not as rich as `MaterialApp`.

你也可以使用 `WidgetApp` 作為應用的 Widget，它會提供一些相同的功能，
但是不如 `MaterialApp` 提供的功能豐富。

To customize the colors and styles of any child components,
pass a `ThemeData` object to the `MaterialApp` widget.
For example, in the following code,
the primary swatch is set to blue and text selection color is red.

如果要自訂任意子元件的顏色或者樣式，
給 `MaterialApp` Widget 傳入一個 `ThemeData` 物件即可。
例如，在下面的程式碼中，主色調設定為藍色，文字選中顏色設定為紅色。

<?code-excerpt "lib/theme.dart (Theme)"?>
```dart
class SampleApp extends StatelessWidget {
  /// This widget is the root of your application.
  const SampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        textSelectionTheme:
            const TextSelectionThemeData(selectionColor: Colors.red),
      ),
      home: const SampleAppPage(),
    );
  }
}
```

## Databases and local storage

## 資料庫與本地儲存

### How do I access shared preferences or UserDefaults?

### 如何存取共享首選項或使用者預設值?

Xamarin.Forms developers will likely be familiar with the
`Xam.Plugins.Settings` plugin.

Xamarin.Forms 開發者可能會熟悉 `Xam.Plugins.Settings` 外掛。

In Flutter, access equivalent functionality using the
[`shared_preferences`][] plugin. This plugin wraps the
functionality of both `UserDefaults` and the Android
equivalent, `SharedPreferences`.

在 Flutter 中，使用 [`shared_preferences` 外掛][`shared_preferences`]
就可以存取相同的功能。這個外掛封裝了 `UserDefaults`
和 Android 平臺上的 `SharedPreferences`。

### How do I access SQLite in Flutter?

### 在 Flutter 中如何存取 SQLite

In Xamarin.Forms most applications would use the `sqlite-net-pcl`
plugin to access SQLite databases.

在 Xamarin.Forms 中大多數應用會使用 `sqlite-net-pcl` 外掛來存取 SQLite 資料庫。

In Flutter, access this functionality using the
[`sqflite`][] plugin.

在 Flutter 中，使用 [SQFlite][`sqflite`] 外掛來存取這個功能。

## Debugging

## 除錯

### What tools can I use to debug my app in Flutter?

### 我可以使用什麼工具除錯我的 Flutter 應用？

Use the [DevTools][] suite for debugging Flutter or Dart apps.

請使用 [開發者工具][DevTools] 除錯你的 Flutter 和 Dart 應用。

DevTools includes support for profiling, examining the heap,
inspecting the widget tree, logging diagnostics, debugging,
observing executed lines of code,
debugging memory leaks and memory fragmentation.
For more information, see the [DevTools][] documentation.

開發者工具套件含了效能工具、檢查堆疊、檢視 widget 樹、診斷資訊記錄、除錯、
執行程式碼行觀察、除錯記憶體洩漏和記憶體碎片等。
有關更多資訊，請參閱 [開發者工具][DevTools] 文件。

## Notifications

## 通知

### How do I set up push notifications?

### 如何設定通知推送?

In Android, you use Firebase Cloud Messaging to setup
push notifications for your app.

在 Android 中，你可以使用 Firebase Cloud Messaging 來為應用設定推送通知。

In Flutter, access this functionality using the
[`firebase_messaging`][] plugin.
For more information on using the Firebase Cloud Messaging API, see the
[`firebase_messaging`][]
plugin documentation.

在 Flutter 中，則使用 [`firebase_messaging`][] 外掛實現此功能。
想要獲得更多關於使用 Firebase Cloud Messaging API 的資訊，
請查閱 [`firebase_messaging`][] 外掛文件。

[Adding assets and images]: {{site.url}}/development/ui/assets-and-images
[Animation & Motion widgets]: {{site.url}}/development/ui/widgets/animation
[Animations overview]: {{site.url}}/development/ui/animations
[Animations tutorial]: {{site.url}}/development/ui/animations/tutorial
[Apple's iOS design language]: {{site.apple-dev}}/design/resources/
[arb]: {{site.github}}/google/app-resource-bundle
[Async UI]: #async-ui
[`cloud_firestore`]: {{site.pub}}/packages/cloud_firestore
[composing]: {{site.url}}/resources/architectural-overview#composition
[Cupertino widgets]: {{site.url}}/development/ui/widgets/cupertino
[`devicePixelRatio`]: {{site.api}}/flutter/dart-ui/FlutterView/devicePixelRatio.html
[developing packages and plugins]: {{site.url}}/development/packages-and-plugins/developing-packages
[DevTools]: {{site.url}}/development/tools/devtools/overview
[existing plugin]: {{site.pub}}/flutter
[`firebase_admob`]: {{site.pub}}/packages/firebase_admob
[`firebase_analytics`]: {{site.pub}}/packages/firebase_analytics
[`firebase_auth`]: {{site.pub}}/packages/firebase_auth
[`firebase_database`]: {{site.pub}}/packages/firebase_database
[`firebase_messaging`]: {{site.pub}}/packages/firebase_messaging
[`firebase_storage`]: {{site.pub}}/packages/firebase_storage
[first party plugins]: {{site.pub}}/flutter/packages?q=firebase
[Flutter cookbook]: {{site.url}}/cookbook
[`flutter_facebook_login`]: {{site.pub}}/packages/flutter_facebook_login
[`flutter_firebase_ui`]: {{site.pub}}/packages/flutter_firebase_ui
[`geolocator`]: {{site.pub}}/packages/geolocator
[`camera`]: {{site.pub-pkg}}/camera
[`http` package]: {{site.pub}}/packages/http
[internationalization guide]: {{site.url}}/development/accessibility-and-localization/internationalization
[`intl`]: {{site.pub}}/packages/intl
[`intl_translation`]: {{site.pub}}/packages/intl_translation
[Introduction to declarative UI]: {{site.url}}/get-started/flutter-for/declarative
[`Localizations`]: {{site.api}}/flutter/widgets/Localizations-class.html
[Material Components]: {{site.url}}/development/ui/widgets/material
[Material Design]: {{site.material}}/design
[Material Design guidelines]: {{site.material}}/design
[`Opacity` widget]: {{site.api}}/flutter/widgets/Opacity-class.html
[optimized for all platforms]: {{site.material}}/design/platform-guidance/cross-platform-adaptation.html#cross-platform-guidelines
[platform channels]: {{site.url}}/development/platform-integration/platform-channels
[plugins]: {{site.url}}/development/packages-and-plugins/using-packages
[pub.dev]: {{site.pub}}
[publish it on pub.dev]: {{site.url}}/development/packages-and-plugins/developing-packages#publish
[Retrieve the value of a text field]: {{site.url}}/cookbook/forms/retrieve-input
[`shared_preferences`]: {{site.pub}}/packages/shared_preferences
[`sqflite`]: {{site.pub}}/packages/sqflite
[`TextEditingController`]: {{site.api}}/flutter/widgets/TextEditingController-class.html
[`url_launcher`]: {{site.pub}}/packages/url_launcher
[widget]: {{site.url}}/resources/architectural-overview#widgets
[widget catalog]: {{site.url}}/development/ui/widgets/layout
[`Window.locale`]: {{site.api}}/flutter/dart-ui/Window/locale.html
[Write your first Flutter app, part 1]: {{site.codelabs}}/codelabs/first-flutter-app-pt1
[Write your first Flutter app, part 2]: {{site.codelabs}}/codelabs/first-flutter-app-pt2
[write your own]: {{site.url}}/development/packages-and-plugins/developing-packages
