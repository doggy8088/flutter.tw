---
title: Flutter for Android developers
title: 給 Android 開發者的 Flutter 指南
description: Learn how to apply Android developer knowledge when building Flutter apps.
description: 學習如何把 Android 開發的經驗應用到 Flutter 應用的開發中。
tags: Flutter課程,Flutter起步,Flutter入門
keywords: Flutter Android,安卓,用Flutter開發Android
---

<?code-excerpt path-base="get-started/flutter-for/android_devs"?>

This document is meant for Android developers looking to apply their
existing Android knowledge to build mobile apps with Flutter.
If you understand the fundamentals of the Android framework then you
can use this document as a jump start to Flutter development.

這篇文件旨在幫助 Android 開發者利用
既有的 Android 知識來透過 Flutter 開發移動應用。
如果你瞭解 Android 框架的基本知識，
你就可以使用這篇文件作為 Flutter 開發的快速入門。

{{site.alert.note}}

  To integrate Flutter code into your Android app, see
  [Add Flutter to existing app][].

  要向你的 Android 應用中整合 Flutter 請參閱
  [向已有應用中新增 Flutter][Add Flutter to existing app]。

{{site.alert.end}}

Your Android knowledge and skill set are highly valuable when building with
Flutter, because Flutter relies on the mobile operating system for numerous
capabilities and configurations. Flutter is a new way to build UIs for mobile,
but it has a plugin system to communicate with Android (and iOS) for non-UI
tasks. If you're an expert with Android, you don't have to relearn everything
to use Flutter.

你的 Android 知識和技能對於 Flutter 開發是非常有用的，
因為 Flutter 依賴於 Android 作業系統的多種功能和配置。
Flutter 是一種全新的建構移動介面的方式，
但是它有一套和 Android（以及 iOS）進行非 UI 任務通訊的外掛系統。
如果你是一名 Android 專家，你就不必重新學習所有知識才能使用 Flutter。

This document can be used as a cookbook by jumping around and
finding questions that are most relevant to your needs.

這篇文件可以用作隨時查閱以及答疑解惑的專題手冊。

## Views

## 檢視 (Views)

### What is the equivalent of a View in Flutter?

### 檢視 (View) 在 Flutter 中對應什麼概念？

{{site.alert.secondary}}

  How is react-style, or _declarative_, programming different than the
  traditional imperative style?
  For a comparison, see [Introduction to declarative UI][].

  響應式或者宣告式的程式設計和傳統的命令式風格有什麼不同呢？
  作為對比，請查閱 [宣告式 UI 介紹][Introduction to declarative UI]。

{{site.alert.end}}

In Android, the `View` is the foundation of everything that shows up on the
screen. Buttons, toolbars, and inputs, everything is a View.
In Flutter, the rough equivalent to a `View` is a `Widget`.
Widgets don't map exactly to Android views, but while you're getting
acquainted with how Flutter works you can think of them as
"the way you declare and construct UI".

Android 中的 `View` 是顯示在螢幕上的一切的基礎。
按鈕、工具欄、輸入框以及一切內容都是 `View`。
而 Flutter 中 `View` 的大致對應著 `Widget`。
Widget 並非完全對應於 Android 中的 View，
但是在你熟悉 Flutter 的工作原理的過程中
可以把它們看做「宣告和建構 UI 的方式」。

However, these have a few differences to a `View`. To start, widgets have a
different lifespan: they are immutable and only exist until they need to be
changed. Whenever widgets or their state change, Flutter's framework creates
a new tree of widget instances. In comparison, an Android view is drawn once
and does not redraw until `invalidate` is called.

然而，widget 和 `View` 還是有一些差異。
首先，widget 有著不一樣的生命週期：
它們是不可變的，一旦需要變化則生命週期終止。
任何時候 widget 或它們的狀態變化時，
Flutter 框架都會建立一個新的 widget 樹的例項。
對比來看，一個 Android View 只會繪製一次，
除非呼叫 `invalidate` 才會重繪。

Flutter's widgets are lightweight, in part due to their immutability.
Because they aren't views themselves, and aren't directly drawing anything,
but rather are a description of the UI and its semantics that get "inflated"
into actual view objects under the hood.

Flutter 的 widget 很輕量，部分原因在於它們的不可變性。
因為它們本身既非檢視，也不會直接繪製任何內容，
而是 UI 及其底層建立真正檢視物件的語義的描述。

Flutter includes the [Material Components][] library.
These are widgets that implement the
[Material Design guidelines][]. Material Design is a
flexible design system [optimized for all platforms][],
including iOS.

Flutter 支援 [Material Components][] 庫。
它提供實現了 [Material Design 設計規範][Material Design guidelines] 的 widgets。
Material Design 是一套 [為所有平台最佳化][optimized for all platforms]
（包括 iOS）的靈活的設計系統。

But Flutter is flexible and expressive enough to implement any design language.
For example, on iOS, you can use the [Cupertino widgets][]
to produce an interface that looks like [Apple's iOS design language][].

Flutter 非常靈活、有表達能力，它可以實現任何設計語言。
例如，在 iOS 平臺上，你可以使用 [Cupertino widgets][] 
建立 [Apple 的 iOS 設計語言][Apple's iOS design language] 風格的介面。

### How do I update widgets?

### 如何更新 widgets？

In Android, you update your views by directly mutating them. However,
in Flutter, `Widget`s are immutable and are not updated directly,
instead you have to work with the widget's state.

在 Android 中，你可以直接操作更新 View。
然而在 Flutter 中，`Widget` 是不可變的，
無法被直接更新，你需要操作 Widget 的狀態。

This is where the concept of `Stateful` and `Stateless` widgets comes from.
A `StatelessWidget` is just what it sounds like&mdash;a
widget with no state information.

這就是有狀態 (Stateful) 和無狀態 (Stateless) Widget 概念的來源。
`StatelessWidget` 如其字面意思&mdash;沒有狀態資訊的 Widget。

`StatelessWidgets` are useful when the part of the user interface
you are describing does not depend on anything other than the configuration
information in the object.

`StatelessWidget` 用於你描述的使用者介面的一部分不依賴於
除了物件中的配置資訊以外的任何東西的場景。

For example, in Android, this is similar to placing an `ImageView`
with your logo. The logo is not going to change during runtime,
so use a `StatelessWidget` in Flutter.

例如在 Android 中，這就像顯示一個展示圖示的 `ImageView`。
這個圖示在執行過程中不會改變，所以在 Flutter 中就使用 `StatelessWidget`。

If you want to dynamically change the UI based on data received
after making an HTTP call or user interaction then you have to work
with `StatefulWidget` and tell the Flutter framework that the widget's
`State` has been updated so it can update that widget.

如果你想要根據 HTTP 請求返回的資料或者使用者的互動來動態地更新介面，
那麼你就必須使用 `StatefulWidget`，
並告訴 Flutter 框架 Widget 的 `狀態` (`State`) 更新了，
以便 Flutter 可以更新這個 Widget。

The important thing to note here is at the core both stateless and stateful
widgets behave the same. They rebuild every frame, the difference is the
`StatefulWidget` has a `State` object that stores state data across frames
and restores it.

這裡需要著重注意的是，無狀態和有狀態的 Widget 本質上是行為一致的。
它們每一幀都會重建，不同之處在於 `StatefulWidget`
有一個跨幀儲存和恢復狀態資料的 `State` 物件。

If you are in doubt, then always remember this rule: if a widget changes
(because of user interactions, for example) it's stateful.
However, if a widget reacts to change, the containing parent widget can
still be stateless if it doesn't itself react to change.

如果你有疑問，那麼記住這條規則：
如果一個 Widget 會變化（例如由於使用者互動），它是有狀態的。
然而，如果一個 Widget 響應變化，
它的父 Widget 只要本身不響應變化，就依然是無狀態的。

The following example shows how to use a `StatelessWidget`. A common
`StatelessWidget` is the `Text` widget. If you look at the implementation of
the `Text` widget you'll find that it subclasses `StatelessWidget`.

下面的例子展示瞭如何使用 `StatelessWidget`。
`Text` Widget 是一個普通的 `StatelessWidget`。
如果你檢視 `Text` Widget 的實現，你會發現它繼承自 `StatelessWidget`。

<?code-excerpt "lib/text_widget.dart (TextWidget)" replace="/return const //g"?>
```dart
Text(
  'I like Flutter!',
  style: TextStyle(fontWeight: FontWeight.bold),
);
```

As you can see, the `Text` Widget has no state information associated with it,
it renders what is passed in its constructors and nothing more.

如上所示，這個 `Text` Widget 沒有相關聯的狀態資訊，
它只渲染傳入構造器的資訊，僅此而已。

But, what if you want to make "I Like Flutter" change dynamically, for
example when clicking a `FloatingActionButton`?

但是，假如你想要動態地改變 "I Like Flutter"，
例如當你點選一個 `FloatingActionButton` 的時候，
該怎麼辦呢？

To achieve this, wrap the `Text` widget in a `StatefulWidget` and
update it when the user clicks the button.

為了實現這個效果，將 `Text` Widget 嵌入一個 `StatefulWidget` 中，
並在使用者點選按鈕的時候更新它。

For example:

例如：

<?code-excerpt "lib/text_widget.dart (StatefulWidget)"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  const SampleApp({super.key});
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  // Default placeholder text.
  String textToShow = 'I Like Flutter';

  void _updateText() {
    setState(() {
      // Update the text.
      textToShow = 'Flutter is Awesome!';
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sample App'),
      ),
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

### How do I lay out my widgets? Where is my XML layout file?

### 如何佈局 Widget？我的 XML 佈局檔案在哪裡？

In Android, you write layouts in XML, but in Flutter you write your layouts
with a widget tree.

在 Android 中，你透過 XML 檔案定義佈局，
但是在 Flutter 中，你要透過一個 widget 樹來定義佈局的。

The following example shows how to display a simple widget with padding:

以下範例展示瞭如何顯示一個帶有間距的簡單 widget：

<?code-excerpt "lib/layout.dart (SimpleWidget)"?>
```dart
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: const Text('Sample App'),
    ),
    body: Center(
      child: ElevatedButton(
        style: ElevatedButton.styleFrom(
          padding: const EdgeInsets.only(left: 20, right: 30),
        ),
        onPressed: () {},
        child: const Text('Hello'),
      ),
    ),
  );
}
```

You can view some of the layouts that Flutter has to offer in the
[widget catalog][].

你可以在 [widget 目錄][widget catalog] 中檢視 Flutter 提供的佈局。

### How do I add or remove a component from my layout?

### 如何在佈局中新增或刪除一個元件？

In Android, you call `addChild()` or `removeChild()`
on a parent to dynamically add or remove child views.
In Flutter, because widgets are immutable there is
no direct equivalent to `addChild()`.  Instead,
you can pass a function to the parent that returns a widget,
and control that child's creation with a boolean flag.

在 Android 中，你透過呼叫父 View 的 `addChild()`
或 `removeChild()` 方法動態地新增或者刪除子 View。
在 Flutter 中，由於 Widget 是不可變的，
所以沒有 `addChild()` 的直接對應的方法。
不過，你可以給返回一個 Widget 的父 Widget 傳入一個方法，
並透過布林標記值控制子 Widget 的建立。

For example, here is how you can toggle between two
widgets when you click on a `FloatingActionButton`:

例如，下面就是你可以如何在點選一個
`FloatingActionButton` 時在兩個 widget 之間切換。

<?code-excerpt "lib/layout.dart (ToggleWidget)"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  const SampleApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  // Default value for toggle.
  bool toggle = true;
  void _toggle() {
    setState(() {
      toggle = !toggle;
    });
  }

  Widget _getToggleChild() {
    if (toggle) {
      return const Text('Toggle One');
    } else {
      return ElevatedButton(
        onPressed: () {},
        child: const Text('Toggle Two'),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sample App'),
      ),
      body: Center(
        child: _getToggleChild(),
      ),
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

### Widget 如何實現動畫？

In Android, you either create animations using XML, or call the `animate()`
method on a view. In Flutter, animate widgets using the animation
library by wrapping widgets inside an animated widget.

在 Android 中，你既可以透過 XML 檔案定義動畫，
也可以呼叫 View 物件的 `animate()` 方法。
在 Flutter 裡，則使用動畫函式庫，透過將 Widget
嵌入一個動畫 Widget 的方式實現 Widget 的動畫效果。

In Flutter, use an `AnimationController` which is an `Animation<double>`
that can pause, seek, stop and reverse the animation. It requires a `Ticker`
that signals when vsync happens, and produces a linear interpolation between
0 and 1 on each frame while it's running. You then create one or more
`Animation`s and attach them to the controller.

Flutter 透過 `Animation<double>` 的子類別
`AnimationController` 來暫停、播放、停止以及逆向播放動畫。
它需要一個 `Ticker` 在垂直同步 (vsync) 的時候發出訊號，
並且在執行的時候建立一個介於 0 和 1 之間的線性插值。
然後你就可以建立一個或多個 `Animation`，並將它們繫結到控制器上。

For example, you might use `CurvedAnimation` to implement an animation
along an interpolated curve. In this sense, the controller
is the "master" source of the animation progress and the `CurvedAnimation`
computes the curve that replaces the controller's default linear motion.
Like widgets, animations in Flutter work with composition.

例如，你可以使用 `CurvedAnimation` 來實現一個曲線插值的動畫。
在這種情況下，控制器決定了動畫進度，
`CurvedAnimation` 計算用於替換控制器預設線性動畫的曲線值。
與 Widget 一樣，Flutter 中的動畫效果也可以組合使用。

When building the widget tree you assign the `Animation` to an animated
property of a widget, such as the opacity of a `FadeTransition`, and tell the
controller to start the animation.

在建構 Widget 樹的時候，你需要將 `Animation` 物件賦值給某個 Widget 的動畫屬性，
例如`FadeTransition` 的不透明度屬性，並讓控制器開始動畫。

The following example shows how to write a `FadeTransition` that fades the
widget into a logo when you press the `FloatingActionButton`:

下面的例子展示瞭如何實現一個點選 `FloatingActionButton` 時
將一個 Widget 漸變為一個圖示的 `FadeTransition`：

<?code-excerpt "lib/animation.dart"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const FadeAppTest());
}

class FadeAppTest extends StatelessWidget {
  const FadeAppTest({super.key});
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Fade Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const MyFadeTest(title: 'Fade Demo'),
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
  late AnimationController controller;
  late CurvedAnimation curve;

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
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: FadeTransition(
          opacity: curve,
          child: const FlutterLogo(
            size: 100,
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        tooltip: 'Fade',
        onPressed: () {
          controller.forward();
        },
        child: const Icon(Icons.brush),
      ),
    );
  }
}
```

For more information, see
[Animation & Motion widgets][],
the [Animations tutorial][],
and the [Animations overview][].

獲取更多內容，請檢視 [動畫 Widget][Animation & Motion widgets]、
[動畫指南][Animations tutorial] 以及 [動畫概覽][Animations overview]。

### How do I use a Canvas to draw/paint?

### 如何使用 Canvas 進行繪製？

In Android, you would use the `Canvas` and `Drawable`
to draw images and shapes to the screen.
Flutter has a similar `Canvas` API as well,
since it's based on the same low-level rendering engine, Skia.
As a result, painting to a canvas in Flutter
is a very familiar task for Android developers.

在 Android 中，你可以使用 `Canvas` 和 `Drawable` 將圖片和形狀繪製到螢幕上。
Flutter 也有一個類似於 `Canvas` 的 API，
因為它基於相同的底層渲染引擎 Skia。
因此，在 Flutter 中用畫布 (canvas) 
進行繪製對於 Android 開發者來說是一件非常熟悉的工作。

Flutter has two classes that help you draw to the canvas: `CustomPaint`
and `CustomPainter`,
the latter of which implements your algorithm to draw to the canvas.

Flutter 有兩個幫助你用畫布 (canvas) 進行繪製的類：
`CustomPaint` 和 `CustomPainter`，
後者可以實現自訂的繪製演算法。

To learn how to implement a signature painter in Flutter,
see Collin's answer on [Custom Paint][].

如果想學習在 Flutter 中如何實現一個簽名功能，
可以檢視 Collin 在 [Custom Paint][] 上的回答。

<?code-excerpt "lib/canvas.dart"?>
```dart
import 'package:flutter/material.dart';

void main() => runApp(const MaterialApp(home: DemoApp()));

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
  List<Offset?> _points = <Offset>[];
  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onPanUpdate: (details) {
        setState(() {
          RenderBox? referenceBox = context.findRenderObject() as RenderBox;
          Offset localPosition =
              referenceBox.globalToLocal(details.globalPosition);
          _points = List.from(_points)..add(localPosition);
        });
      },
      onPanEnd: (details) => _points.add(null),
      child: CustomPaint(
        painter: SignaturePainter(_points),
        size: Size.infinite,
      ),
    );
  }
}

class SignaturePainter extends CustomPainter {
  SignaturePainter(this.points);
  final List<Offset?> points;
  @override
  void paint(Canvas canvas, Size size) {
    var paint = Paint()
      ..color = Colors.black
      ..strokeCap = StrokeCap.round
      ..strokeWidth = 5;
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

[Custom Paint]: {{site.so}}/questions/46241071/create-signature-area-for-mobile-app-in-dart-flutter

### How do I build custom widgets?

### 如何建立自訂 Widget？

In Android, you typically subclass `View`, or use a pre-existing view,
to override and implement methods that achieve the desired behavior.

在 Android 中，一般透過繼承 `View` 類，
或者使用已有的檢視類，再重載或實現以達到特定效果的方法。

In Flutter, build a custom widget by [composing][]
smaller widgets (instead of extending them).
It is somewhat similar to implementing a custom `ViewGroup`
in Android, where all the building blocks are already existing,
but you provide a different behavior&mdash;for example,
custom layout logic.

在 Flutter 中，透過 [組合][composing] 
更小的 Widget 來建立自訂 Widget（而不是繼承它們）。
這和 Android 中實現一個自訂的 `ViewGroup` 有些類似，
所有的建構 UI 的模組程式碼都已存在，
不過由你提供不同的行為&mdash;例如，自訂佈局 (layout) 邏輯。

For example, how do you build a `CustomButton` that takes a label in
the constructor? Create a CustomButton that composes a `ElevatedButton` with
a label, rather than by extending `ElevatedButton`:

舉例來說，你該如何建立一個在構造器接收標籤引數的 `CustomButton`？
你要組合 `ElevatedButton` 和一個標籤來建立自訂按鈕，
而不是繼承 `ElevatedButton`：

<?code-excerpt "lib/custom.dart (CustomButton)"?>
```dart
class CustomButton extends StatelessWidget {
  final String label;

  const CustomButton(this.label, {super.key});

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

<?code-excerpt "lib/custom.dart (UseCustomButton)"?>
```dart
@override
Widget build(BuildContext context) {
  return const Center(
    child: CustomButton('Hello'),
  );
}
```

## Intents

## 意圖 (Intents)

### What is the equivalent of an Intent in Flutter?

### `Intent` 在 Flutter 中的對應概念是什麼？

In Android, there are two main use cases for `Intent`s: navigating between
Activities, and communicating with components. Flutter, on the other hand,
does not have the concept of intents, although you can still start intents
through native integrations (using [a plugin][]).

在 Android 中，`Intent` 主要有兩個使用場景：
在 Activity 之前進行導航，以及元件間通訊。
Flutter 卻沒有 intent 這樣的概念，
但是你依然可以透過原生整合 ([外掛]({{site.pub}}/packages/android_intent))
來啟動 intent。

Flutter doesn't really have a direct equivalent to activities and fragments;
rather, in Flutter you navigate between screens, using a `Navigator` and
`Route`s, all within the same `Activity`.

Flutter 實際上並沒有 Activity 和 Fragment 的對應概念。
在 Flutter 中你需要使用 `Navigator` 和 `Route`
在同一個 `Activity` 內的不同介面間進行跳轉。

A `Route` is an abstraction for a "screen" or "page" of an app, and a
`Navigator` is a widget that manages routes. A route roughly maps to an
`Activity`, but it does not carry the same meaning. A navigator can push
and pop routes to move from screen to screen. Navigators work like a stack
on which you can `push()` new routes you want to navigate to, and from
which you can `pop()` routes when you want to "go back".

`Route` 是應用內螢幕和頁面的抽象，`Navigator` 是管理路徑 route 的工具。
一個 route 物件大致對應於一個 `Activity`，但是它的含義是不一樣的。
Navigator 可以透過對 route 進行壓棧和彈棧操作實現頁面的跳轉。
Navigator 的工作原理和棧相似，你可以將想要跳轉到的 route 壓棧 (`push()`)，
想要返回的時候將 route 出棧 (`pop()`)。

In Android, you declare your activities inside the app's `AndroidManifest.xml`.

在 Android 中，在應用的 `AndroidManifest.xml` 檔案中宣告 Activity。

In Flutter, you have a couple options to navigate between pages:

在 Flutter 中，你有多種不同的方式在頁面間導航：

* Specify a `Map` of route names. (using `MaterialApp`)

  定義一個 route 名字的 `Map`。(MaterialApp)

* Directly navigate to a route. (using `WidgetsApp`)

  直接導航到一個 route。(WidgetApp)

The following example builds a Map.

下面的例子建立了一個 Map。

<?code-excerpt "lib/intent.dart (Map)"?>
```dart
void main() {
  runApp(MaterialApp(
    home: const MyAppHome(), // Becomes the route named '/'.
    routes: <String, WidgetBuilder>{
      '/a': (context) => const MyPage(title: 'page A'),
      '/b': (context) => const MyPage(title: 'page B'),
      '/c': (context) => const MyPage(title: 'page C'),
    },
  ));
}
```

Navigate to a route by `push`ing its name to the `Navigator`.

透過路由名 **壓棧** (`push`) 到 `Navigator` 中來跳轉到這個 route。

<?code-excerpt "lib/intent.dart (Push)"?>
```dart
Navigator.of(context).pushNamed('/b');
```

The other popular use-case for `Intent`s is to call external components such
as a Camera or File picker. For this, you would need to create a native platform
integration (or use an [existing plugin][]).

`Intent` 的另一種常見的使用場景是呼叫外部的元件，例如相機或檔案選擇器。
對於這種情況，你需要建立一個原生平台外掛
（或者使用 [已有的外掛][existing plugin]）。

To learn how to build a native platform integration,
see [developing packages and plugins][].

想要學習如何建立一個原生平台整合，
請檢視 [開發套件和外掛][developing packages and plugins]。

### How do I handle incoming intents from external applications in Flutter?

### 在 Flutter 中應該如何處理從外部應用接收到的 intent？

Flutter can handle incoming intents from Android by directly talking to the
Android layer and requesting the data that was shared.

Flutter 可以透過直接和 Android 層通訊並請求分享的資料來處理接收到的 Android intent。

The following example registers a text share intent filter on the native
activity that runs our Flutter code, so other apps can share text with
our Flutter app.

下面的例子中，執行 Flutter 程式碼的原生 Activity
註冊了一個文字分享的 intent 過濾器，
這樣其它應用就可以和 Flutter 應用分享文字了。

The basic flow implies that we first handle the shared text data on the
Android native side (in our `Activity`), and then wait until Flutter requests
for the data to provide it using a `MethodChannel`.

從以上流程可以得知，我們首先在 Android 原生層面
（在我們的 `Activity` 中）處理分享的文字資料，
然後 Flutter 再透過使用 `MethodChannel` 獲取這個資料。

First, register the intent filter for all intents in `AndroidManifest.xml`:

首先，在 `AndroidManifest.xml` 中註冊 intent 過濾器：

```xml
<activity
  android:name=".MainActivity"
  android:launchMode="singleTop"
  android:theme="@style/LaunchTheme"
  android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale|layoutDirection"
  android:hardwareAccelerated="true"
  android:windowSoftInputMode="adjustResize">
  <!-- ... -->
  <intent-filter>
    <action android:name="android.intent.action.SEND" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:mimeType="text/plain" />
  </intent-filter>
</activity>
```

Then in `MainActivity`, handle the intent, extract the text that was
shared from the intent, and hold onto it. When Flutter is ready to process,
it requests the data using a platform channel, and it's sent
across from the native side:

接著在 `MainActivity` 中處理 intent，提取出其它 intent 分享的文字並儲存。
當 Flutter 準備好處理的時候，它會使用一個平台通道請求資料，
資料便會從原生端傳送過來：

```java
package com.example.shared;

import android.content.Intent;
import android.os.Bundle;

import androidx.annotation.NonNull;

import io.flutter.plugin.common.MethodChannel;
import io.flutter.embedding.android.FlutterActivity;
import io.flutter.embedding.engine.FlutterEngine;
import io.flutter.plugins.GeneratedPluginRegistrant;

public class MainActivity extends FlutterActivity {

  private String sharedText;
  private static final String CHANNEL = "app.channel.shared.data";

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Intent intent = getIntent();
    String action = intent.getAction();
    String type = intent.getType();

    if (Intent.ACTION_SEND.equals(action) && type != null) {
      if ("text/plain".equals(type)) {
        handleSendText(intent); // Handle text being sent
      }
    }
  }

  @Override
  public void configureFlutterEngine(@NonNull FlutterEngine flutterEngine) {
      GeneratedPluginRegistrant.registerWith(flutterEngine);

      new MethodChannel(flutterEngine.getDartExecutor().getBinaryMessenger(), CHANNEL)
              .setMethodCallHandler(
                      (call, result) -> {
                          if (call.method.contentEquals("getSharedText")) {
                              result.success(sharedText);
                              sharedText = null;
                          }
                      }
              );
  }

  void handleSendText(Intent intent) {
    sharedText = intent.getStringExtra(Intent.EXTRA_TEXT);
  }
}
```

Finally, request the data from the Flutter side
when the widget is rendered:

最後，當 widget 渲染的時候，從 Flutter 這端請求資料：

<?code-excerpt "lib/request_data.dart"?>
```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  const SampleApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample Shared App Handler',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  static const platform = MethodChannel('app.channel.shared.data');
  String dataShared = 'No data';

  @override
  void initState() {
    super.initState();
    getSharedText();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(body: Center(child: Text(dataShared)));
  }

  Future<void> getSharedText() async {
    var sharedData = await platform.invokeMethod('getSharedText');
    if (sharedData != null) {
      setState(() {
        dataShared = sharedData;
      });
    }
  }
}
```

### What is the equivalent of startActivityForResult()?

### `startActivityForResult()` 的對應方法是什麼？

The `Navigator` class handles routing in Flutter and is used to get
a result back from a route that you have pushed on the stack.
This is done by `await`ing on the `Future` returned by `push()`.

`Navigator` 類負責 Flutter 的導航，並用來接收被壓棧的 route 的返回值。
這是透過在 `push()` 後返回的 `Future` 上 `await` 來實現的。

For example, to start a location route that lets the user select
their location, you could do the following:

例如，要開啟一個讓使用者選擇位置的路由，你可以這樣做：

<?code-excerpt "lib/intent.dart (PushAwait)"?>
```dart
Object? coordinates = await Navigator.of(context).pushNamed('/location');
```

And then, inside your location route, once the user has selected their location
you can `pop` the stack with the result:

然後，在你的位置的路由內，一旦使用者選擇了位置，你就可以出棧 (`pop`) 並返回結果：

<?code-excerpt "lib/intent.dart (Pop)"?>
```dart
Navigator.of(context).pop({'lat': 43.821757, 'long': -79.226392});
```

## Async UI

## 非同步 UI

### What is the equivalent of runOnUiThread() in Flutter?

### `runOnUiThread()` 在 Flutter 中的對應方法是什麼？

Dart has a single-threaded execution model, with support for `Isolate`s
(a way to run Dart code on another thread), an event loop, and
asynchronous programming. Unless you spawn an `Isolate`, your Dart code
runs in the main UI thread and is driven by an event loop. Flutter's event
loop is equivalent to Android's main `Looper`&mdash;that is, the `Looper` that
is attached to the main thread.

Dart 有一個單執行緒執行的模型，同時也支援 `Isolate`
（在另一個執行緒執行 Dart 程式碼的方法），它是一個事件迴圈和非同步程式設計方式。
除非你建立一個 `Isolate`，否則你的 Dart 程式碼會執行在主 UI 執行緒，
並被一個事件迴圈所驅動。Flutter 的事件迴圈對應於 Android 裡的
主 `Looper`&mdash;&mdash;也即繫結到主執行緒上的 `Looper`。

Dart's single-threaded model doesn't mean you need to run everything as a
blocking operation that causes the UI to freeze. Unlike Android, which
requires you to keep the main thread free at all times, in Flutter,
use the asynchronous facilities that the Dart language provides, such as
`async`/`await`, to perform asynchronous work. You might be familiar with
the `async`/`await` paradigm if you've used it in C#, Javascript, or if you
have used Kotlin's coroutines.

Dart 的單執行緒模型並不意味著你需要以會導致 UI 凍結的
阻塞操作的方式來執行所有程式碼。
不同於 Android 中需要你時刻保持主執行緒空閒，
在 Flutter 中，可以使用 Dart 語言提供的非同步工具，
例如 `async`/`await` 來執行非同步任務。
如果你使用過 C# 或者 Javascript 中的 `async`/`await` 正規化，
或者 Kotlin 中的協程，你應該對它比較熟悉。

For example, you can run network code without causing the UI to hang by
using `async`/`await` and letting Dart do the heavy lifting:

例如，你可以透過使用 `async`/`await`
來執行網路程式碼而且不會導致 UI 掛起，
同時讓 Dart 來處理背後的繁重細節：

<?code-excerpt "lib/async.dart (loadData)"?>
```dart
Future<void> loadData() async {
  var dataURL = Uri.parse('https://jsonplaceholder.typicode.com/posts');
  http.Response response = await http.get(dataURL);
  setState(() {
    widgets = jsonDecode(response.body);
  });
}
```

Once the `await`ed network call is done, update the UI by calling `setState()`,
which triggers a rebuild of the widget sub-tree and updates the data.

一旦用 `await` 修飾的網路操作完成，再呼叫 `setState()` 更新 UI，
這會觸發 widget 子樹的重建並更新資料。

The following example loads data asynchronously and displays it in a `ListView`:

下面的例子展示了非同步載入資料並將之展示在 `ListView` 內：

<?code-excerpt "lib/async.dart"?>
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
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();
    loadData();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sample App'),
      ),
      body: ListView.builder(
        itemCount: widgets.length,
        itemBuilder: (context, position) {
          return getRow(position);
        },
      ),
    );
  }

  Widget getRow(int i) {
    return Padding(
      padding: const EdgeInsets.all(10),
      child: Text("Row ${widgets[i]["title"]}"),
    );
  }

  Future<void> loadData() async {
    var dataURL = Uri.parse('https://jsonplaceholder.typicode.com/posts');
    http.Response response = await http.get(dataURL);
    setState(() {
      widgets = jsonDecode(response.body);
    });
  }
}
```

Refer to the next section for more information on doing work in the
background, and how Flutter differs from Android.

參考下一節內容獲取更多關於後臺任務以及 Flutter 與 Android 的差異的資訊。

### How do you move work to a background thread?

### 如何將任務轉移到後臺執行緒？

In Android, when you want to access a network resource you would typically
move to a background thread and do the work, as to not block the main thread,
and avoid ANRs. For example, you might be using an `AsyncTask`, a `LiveData`,
an `IntentService`, a `JobScheduler` job, or an RxJava pipeline with a
scheduler that works on background threads.

在 Android 中，當你想要存取一個網路資源卻又不想阻塞主執行緒並避免 ANR 的時候，
你一般會將任務放到一個後臺執行緒中執行。
例如，你可以使用 `AsyncTask`、`LiveData`、`IntentService`、`JobScheduler`
任務或者透過 RxJava 的管道用排程器將任務切換到後臺執行緒中。

Since Flutter is single threaded and runs an event loop (like Node.js), you
don't have to worry about thread management or spawning background threads. If
you're doing I/O-bound work, such as disk access or a network call, then
you can safely use `async`/`await` and you're all set. If, on the other
hand, you need to do computationally intensive work that keeps the CPU busy,
you want to move it to an `Isolate` to avoid blocking the event loop, like
you would keep _any_ sort of work out of the main thread in Android.

由於 Flutter 是單執行緒並且執行一個事件迴圈（類似 Node.js），
你無須擔心執行緒的管理以及後臺執行緒的建立。
如果你在執行和 I/O 繫結的任務，例如儲存存取或者網路請求，
那麼你可以安全地使用 `async`/`await`，
並無後顧之憂。再例如，你需要執行消耗 CPU 的計算密集型工作，
那麼你可以將其轉移到一個 `Isolate` 上以避免阻塞事件迴圈，
就像你在 Android 中會將任何任務放到主執行緒之外一樣。 

For I/O-bound work, declare the function as an `async` function,
and `await` on long-running tasks inside the function:

對於和 I/O 繫結的任務，將方法宣告為 `async` 方法，
並在方法內 `await` 一個長時間執行的任務：

<?code-excerpt "lib/async.dart (loadData)"?>
```dart
Future<void> loadData() async {
  var dataURL = Uri.parse('https://jsonplaceholder.typicode.com/posts');
  http.Response response = await http.get(dataURL);
  setState(() {
    widgets = jsonDecode(response.body);
  });
}
```

This is how you would typically do network or database calls, which are both
I/O operations.

這就是你一般應該如何執行網路和資料庫操作，它們都屬於 I/O 操作。

On Android, when you extend `AsyncTask`, you typically override 3 methods,
`onPreExecute()`, `doInBackground()` and `onPostExecute()`. There is no
equivalent in Flutter, since you `await` on a long running function, and
Dart's event loop takes care of the rest.

在 Android 中，當你繼承 `AsyncTask` 的時候，
你一般會覆寫三個方法：
`onPreExecute()`、`doInBackground()` 和 `onPostExecute()`。
Flutter 中沒有對應的 API，你只需要 `await` 一個耗時方法呼叫， 
Dart 的事件迴圈就會幫你處理剩下的事情。

However, there are times when you might be processing a large amount of data and
your UI hangs. In Flutter, use `Isolate`s to take advantage of
multiple CPU cores to do long-running or computationally intensive tasks.

然而，有時候你可能需要處理大量的資料並掛起你的 UI。
在 Flutter 中，可以透過使用 `Isolate`
來利用多核處理器的優勢執行耗時或計算密集的任務。

Isolates are separate execution threads that do not share any memory
with the main execution memory heap. This means you can't access variables from
the main thread, or update your UI by calling `setState()`.
Unlike Android threads,
Isolates are true to their name, and cannot share memory
(in the form of static fields, for example).

`Isolate` 是獨立執行的執行緒，不會和主執行記憶體堆分享記憶體。
這意味著你無法存取主執行緒的變數，或者呼叫 `setState()` 更新 UI。
與 Android 中執行緒的概念不同，isolate 如其名所示，
它們無法分享記憶體（例如透過靜態變數的形式）。

The following example shows, in a simple isolate, how to share data back to
the main thread to update the UI.

下面的例子展示了一個簡單的 isolate 是如何
將資料分享給主執行緒來更新 UI 的。

<?code-excerpt "lib/isolates.dart (loadData)"?>
```dart
Future<void> loadData() async {
  ReceivePort receivePort = ReceivePort();
  await Isolate.spawn(dataLoader, receivePort.sendPort);

  // The 'echo' isolate sends its SendPort as the first message.
  SendPort sendPort = await receivePort.first;

  List msg = await sendReceive(
    sendPort,
    'https://jsonplaceholder.typicode.com/posts',
  );

  setState(() {
    widgets = msg;
  });
}

// The entry point for the isolate.
static Future<void> dataLoader(SendPort sendPort) async {
  // Open the ReceivePort for incoming messages.
  ReceivePort port = ReceivePort();

  // Notify any other isolates what port this isolate listens to.
  sendPort.send(port.sendPort);

  await for (var msg in port) {
    String data = msg[0];
    SendPort replyTo = msg[1];

    String dataURL = data;
    http.Response response = await http.get(Uri.parse(dataURL));
    // Lots of JSON to parse
    replyTo.send(jsonDecode(response.body));
  }
}

Future sendReceive(SendPort port, msg) {
  ReceivePort response = ReceivePort();
  port.send([msg, response.sendPort]);
  return response.first;
}
```

Here, `dataLoader()` is the `Isolate` that runs in its own separate
execution thread.  In the isolate you can perform more CPU intensive
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
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();
    loadData();
  }

  Widget getBody() {
    bool showLoadingDialog = widgets.isEmpty;
    if (showLoadingDialog) {
      return getProgressDialog();
    } else {
      return getListView();
    }
  }

  Widget getProgressDialog() {
    return const Center(child: CircularProgressIndicator());
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sample App'),
      ),
      body: getBody(),
    );
  }

  ListView getListView() {
    return ListView.builder(
      itemCount: widgets.length,
      itemBuilder: (context, position) {
        return getRow(position);
      },
    );
  }

  Widget getRow(int i) {
    return Padding(
      padding: const EdgeInsets.all(10),
      child: Text("Row ${widgets[i]["title"]}"),
    );
  }

  Future<void> loadData() async {
    ReceivePort receivePort = ReceivePort();
    await Isolate.spawn(dataLoader, receivePort.sendPort);

    // The 'echo' isolate sends its SendPort as the first message.
    SendPort sendPort = await receivePort.first;

    List msg = await sendReceive(
      sendPort,
      'https://jsonplaceholder.typicode.com/posts',
    );

    setState(() {
      widgets = msg;
    });
  }

  // The entry point for the isolate.
  static Future<void> dataLoader(SendPort sendPort) async {
    // Open the ReceivePort for incoming messages.
    ReceivePort port = ReceivePort();

    // Notify any other isolates what port this isolate listens to.
    sendPort.send(port.sendPort);

    await for (var msg in port) {
      String data = msg[0];
      SendPort replyTo = msg[1];

      String dataURL = data;
      http.Response response = await http.get(Uri.parse(dataURL));
      // Lots of JSON to parse
      replyTo.send(jsonDecode(response.body));
    }
  }

  Future sendReceive(SendPort port, msg) {
    ReceivePort response = ReceivePort();
    port.send([msg, response.sendPort]);
    return response.first;
  }
}
```

### What is the equivalent of OkHttp on Flutter?

### OkHttp 在 Flutter 中對應什麼庫？

Making a network call in Flutter is easy when you use the
popular [`http` package][].

Flutter 中使用流行的 [`http` package][] 進行網路請求是很簡單的。

While the http package doesn't have every feature found in OkHttp,
it abstracts away much of the networking that you would normally implement
yourself, making it a simple way to make network calls.

雖然 http 包沒有 OkHttp 中的所有功能，
但是它抽象了很多通常你會自己實現的網路功能，
這使其本身在執行網路請求時簡單易用。

To add the `http` package as a dependency, run `flutter pub add`:

要使用 `http`，請在 `pubspec.yaml` 檔案中新增依賴：

```terminal
$ flutter pub add http
```

To make a network call, call `await` on the `async` function `http.get()`:

如果要發起一個網路請求，
在非同步 (`async`) 方法 `http.get()` 上呼叫 `await` 即可：

<?code-excerpt "lib/network.dart"?>
```dart
import 'dart:developer' as developer;
import 'package:http/http.dart' as http;

Future<void> loadData() async {
  var dataURL = Uri.parse('https://jsonplaceholder.typicode.com/posts');
  http.Response response = await http.get(dataURL);
  developer.log(response.body);
}
```

### How do I show the progress for a long-running task?

### 如何為耗時任務顯示進度？

In Android you would typically show a `ProgressBar` view in your UI while
executing a long running task on a background thread.

在 Android 中你通常會在後台執行一個耗時任務的時候
顯示一個 `ProgressBar` 在介面上。

In Flutter, use a `ProgressIndicator` widget.
Show the progress programmatically by controlling when it's rendered
through a boolean flag. Tell Flutter to update its state before your
long-running task starts, and hide it after it ends.

在 Flutter 中，我們使用 `ProgressIndicator` widget。
透過程式碼邏輯使用一個布林標記值控制進度條的渲染。

In the following example, the build function is separated into three different
functions. If `showLoadingDialog` is `true` (when `widgets.isEmpty`),
then render the `ProgressIndicator`. Otherwise, render the
`ListView` with the data returned from a network call.

在下面的例子中，build 方法被拆分成三個不同的方法。
如果 `showLoadingDialog()` 返回 `true`（當 `widgets.length == 0`），
渲染 `ProgressIndicator`。否則，在 `ListView` 裡渲染網路請求返回的資料。

<?code-excerpt "lib/progress.dart"?>
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
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();
    loadData();
  }

  Widget getBody() {
    bool showLoadingDialog = widgets.isEmpty;
    if (showLoadingDialog) {
      return getProgressDialog();
    } else {
      return getListView();
    }
  }

  Widget getProgressDialog() {
    return const Center(child: CircularProgressIndicator());
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sample App'),
      ),
      body: getBody(),
    );
  }

  ListView getListView() {
    return ListView.builder(
      itemCount: widgets.length,
      itemBuilder: (context, position) {
        return getRow(position);
      },
    );
  }

  Widget getRow(int i) {
    return Padding(
      padding: const EdgeInsets.all(10),
      child: Text("Row ${widgets[i]["title"]}"),
    );
  }

  Future<void> loadData() async {
    var dataURL = Uri.parse('https://jsonplaceholder.typicode.com/posts');
    http.Response response = await http.get(dataURL);
    setState(() {
      widgets = jsonDecode(response.body);
    });
  }
}
```

## Project structure & resources

## 工程結構和資原始檔

### Where do I store my resolution-dependent image files?

### 在哪裡放置解析度相關的圖片檔案？

While Android treats resources and assets as distinct items,
Flutter apps have only assets. All resources that would live
in the `res/drawable-*` folders on Android,
are placed in an assets folder for Flutter.

雖然 Android 區分對待資原始檔 (resources) 和資產檔案 (assets)，
但是 Flutter 應用只有資產檔案 (assets)。
所有原本在 Android 中應該放在 `res/drawable-*` 資料夾中的資原始檔，
在 Flutter 中都放在一個 assets 資料夾中。

Flutter follows a simple density-based format like iOS.
Assets might be `1.0x`, `2.0x`, `3.0x`, or any other multiplier.
Flutter doesn't have `dp`s but there are logical pixels,
which are basically the same as device-independent pixels.
Flutter's [`devicePixelRatio`][] expresses the ratio
of physical pixels in a single logical pixel.

Flutter 遵循一個簡單的類似 iOS 的密度相關的格式。
檔案可以是一倍 (`1x`)、兩倍 (`2x`)、三倍 (`3x`) 或其它的任意倍數。
Flutter 沒有 `dp` 單位，但是有邏輯畫素尺寸，基本和裝置無關的畫素尺寸是一樣的。
名稱為 [`devicePixelRatio`][] 的尺寸表示在單一邏輯畫素標準下裝置物理畫素的比例。

The equivalent to Android's density buckets are:

與 Android 的密度分類別的對照表如下：

 Android density qualifier | Flutter pixel ratio
 --- | ---
Android 密度修飾符 | Flutter 畫素比例
 `ldpi` | `0.75x`
 `mdpi` | `1.0x`
 `hdpi` | `1.5x`
 `xhdpi` | `2.0x`
 `xxhdpi` | `3.0x`
 `xxxhdpi` | `4.0x`

Assets are located in any arbitrary folder&mdash;Flutter
has no predefined folder structure.
You declare the assets (with location) in
the `pubspec.yaml` file, and Flutter picks them up.

檔案放置於任意資料夾中&mdash;&mdash;Flutter 沒有預先定義好的資料夾結構。
你在 `pubspec.yaml` 檔案中定義檔案（包括位置資訊），Flutter 負責找到它們。

Assets stored in the native asset folder are
accessed on the native side using Android's `AssetManager`:

檔案放置於原生端的 asset 資料夾中，
所以可以被原生端的 `AssetManager` 存取：

```kotlin
val flutterAssetStream = assetManager.open("flutter_assets/assets/my_flutter_asset.png")
```

Flutter can't access native resources or assets.

Flutter 依然無法存取原生資原始檔 (resources)，
也無法存取原生資產檔案 (assets)。

To add a new image asset called `my_icon.png` to our Flutter project,
for example, and deciding that it should live in a folder we
arbitrarily called `images`, you would put the base image (1.0x)
in the `images` folder, and all the other variants in sub-folders
called with the appropriate ratio multiplier:

如果你要向 Flutter 專案中新增一個新的叫 `my_icon.png` 的圖片資源，
並且將其放入我們隨便起名的叫做 `images` 的資料夾中，
你需要將基礎圖片(1.0x)放在 `images` 資料夾中，
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

You can then access your images using `AssetImage`:

然後你就可以使用 `AssetImage` 存取你的圖片了：

<?code-excerpt "lib/images.dart (AssetImage)"?>
```dart
AssetImage('images/my_icon.jpeg')
```

or directly in an `Image` widget:

或者透過 `Image` widget 直接存取：

<?code-excerpt "lib/images.dart (Imageasset)"?>
```dart
@override
Widget build(BuildContext context) {
  return Image.asset('images/my_image.png');
}
```

### Where do I store strings? How do I handle localization?

### 字串儲存在哪裡？如何處理本地化？

Flutter currently doesn't have a dedicated resources-like system for strings.
At the moment, the best practice is to hold your copy text in a class as
static fields and accessing them from there. For example:

Flutter 當下並沒有一個特定的管理字串的資源管理系統。
目前來講，最好的辦法是將字串作為靜態域存放在類中，
並透過類存取它們。例如：

<?code-excerpt "lib/string_examples.dart (Strings)"?>
```dart
class Strings {
  static String welcomeMessage = 'Welcome To Flutter';
}
```

Then in your code, you can access your strings as such:

接著在你們的程式碼中，你可以這樣存取你的字串：

<?code-excerpt "lib/string_examples.dart (AccessString)"?>
```dart
Text(Strings.welcomeMessage);
```

Flutter has basic support for accessibility on Android,
though this feature is a work in progress.

Flutter 在 Android 上提供無障礙的基本支援，但是這個功能當下仍在開發。

Flutter developers are encouraged to use the
[intl package][] for internationalization and localization.

我們鼓勵 Flutter 開發者使用 [intl 包][intl package] 進行國際化和本地化。

### What is the equivalent of a Gradle file? How do I add dependencies?

### Gradle 檔案的對應物是什麼？我該如何新增依賴？

In Android, you add dependencies by adding to your Gradle build script.
Flutter uses Dart's own build system, and the Pub package manager.
The tools delegate the building of the native Android and iOS
wrapper apps to the respective build systems.

在 Android 中，你在 Gradle 建構指令碼中新增依賴。Flutter 使用 Dart 自己的建構系統以及 Pub 包管理器。
建構工具會將原生 Android 和 iOS 殼應用的建構代理給對應的建構系統。

While there are Gradle files under the `android` folder in your
Flutter project, only use these if you are adding native
dependencies needed for per-platform integration.
In general, use `pubspec.yaml` to declare
external dependencies to use in Flutter.
A good place to find Flutter packages is [pub.dev][].

雖然在你的 Flutter 專案的 `android` 資料夾下有 Gradle 檔案，
但是它們只用於給對應平台的整合新增原生依賴。
一般來說，在 `pubspec.yaml` 檔案中定義在 Flutter 裡使用的外部依賴。
[pub.dev][] 是查詢 Flutter packages 的好地方。

## Activities and fragments

## Activity 和 Fragment

### What are the equivalent of activities and fragments in Flutter?

### Activity 和 Fragment 在 Flutter 中的對應概念是什麼？

In Android, an `Activity` represents a single focused thing the user can do.
A `Fragment` represents a behavior or a portion of user interface.
Fragments are a way to modularize your code, compose sophisticated
user interfaces for larger screens, and help scale your application UI.
In Flutter, both of these concepts fall under the umbrella of `Widget`s.

在 Android 中，一個 `Activity` 代表使用者可以完成的一件獨立任務。
一個 `Fragment` 代表一個行為或者使用者介面的一部分。
Fragment 用於模組化你的程式碼，為大屏組合複雜的使用者介面，並適配應用的介面。
在 Flutter 中，這兩個概念都對應於 `Widget`。

To learn more about the UI for building Activities and Fragements,
see the community-contributed Medium article,
[Flutter for Android Developers: How to design Activity UI in Flutter][].

如果要學習更多的關於 Activity 和 Fragment 建立介面的內容，請閱讀社群貢獻的 Medium 文章，
[給 Android 開發者的 Flutter 指南：如何在 Flutter 中設計一個 Activity 介面]
[Flutter for Android Developers: How to design Activity UI in Flutter]。

As mentioned in the [Intents][] section,
screens in Flutter are represented by `Widget`s since everything is
a widget in Flutter. Use a `Navigator` to move between different
`Route`s that represent different screens or pages,
or perhaps different states or renderings of the same data.

就如在 [Intents][] 部分所提到的，Flutter 中的介面都是以 `Widget` 表示的，
因為 Flutter 中一切皆為 Widget。你使用 `Navigator` 在表示不同螢幕或頁面，
或者僅僅是相同資料的不同狀態和渲染的各個 `Route` 之間進行導航。

### How do I listen to Android activity lifecycle events?

### 如何監聽 Android Activity 的生命週期事件？

In Android, you can override methods from the `Activity` to capture lifecycle
methods for the activity itself, or register `ActivityLifecycleCallbacks` on
the `Application`. In Flutter, you have neither concept, but you can instead
listen to lifecycle events by hooking into the `WidgetsBinding` observer and
listening to the `didChangeAppLifecycleState()` change event.

在 Android 中，你可以覆寫 `Activity` 的生命週期方法來監聽其生命週期，
也可以在 `Application` 上註冊 `ActivityLifecycleCallbacks`。
在 Flutter 中，這兩種方法都沒有，但是你可以透過繫結 `WidgetsBinding`
觀察者並監聽 `didChangeAppLifecycleState()` 的變化事件來監聽生命週期。

The observable lifecycle events are:

可以被觀察的生命週期事件有：

* `detached` — The application is still hosted on a flutter engine but is detached from any host views.

  `detached` — 應用依然保留 flutter engine，但是全部宿主 view 均已脫離。

* `inactive` — The application is in an inactive state and is not receiving user
  input.

  `inactive` — 應用處於非活躍狀態並且不接收使用者輸入。

* `paused` — The application is not currently visible to the user,
  not responding to user input, and running in the background.
  This is equivalent to `onPause()` in Android.

  `paused` — 應用當前對使用者不可見，無法響應使用者輸入，並執行在後台。
  這個事件對應於 Android 中的 `onPause()`；

* `resumed` — The application is visible and responding to user input.
  This is equivalent to `onPostResume()` in Android.

  `resumed` — 應用對使用者可見並且可以響應使用者的輸入。
  這個事件對應於 Android 中的 `onPostResume()`；


For more details on the meaning of these states, see the
[`AppLifecycleStatus` documentation][].

想要了解這些狀態含義的更多細節，請檢視
[`AppLifecycleStatus` 文件][`AppLifecycleStatus` documentation]。

As you might have noticed, only a small minority of the Activity
lifecycle events are available; while `FlutterActivity` does
capture almost all the activity lifecycle events internally and
send them over to the Flutter engine, they're mostly shielded
away from you. Flutter takes care of starting and stopping the
engine for you, and there is little reason for needing to
observe the activity lifecycle on the Flutter side in most cases.
If you need to observe the lifecycle to acquire or release any
native resources, you should likely be doing it from the native side,
at any rate.

你可能已經注意到，只有一小部分的 Activity 生命週期事件是可用的，
雖然 `FlutterActivity` 在內部捕獲了幾乎所有的 Activity 生命週期事件
並將它們傳送給 Flutter 引擎，但是它們大部分都向你遮蔽了。
Flutter 為你管理引擎的啟動和停止，
在大部分情況下幾乎沒有理由要在 Flutter 一端監聽 Activity 的生命週期。
如果你需要透過監聽生命週期來獲取或釋放原生的資源，
無論如何都應該在原生端做這件事。

Here's an example of how to observe the lifecycle status of the
containing activity:

下面的例子展示瞭如何監聽容器 Activity 的生命週期狀態：

<?code-excerpt "lib/lifecycle.dart"?>
```dart
import 'package:flutter/widgets.dart';

class LifecycleWatcher extends StatefulWidget {
  const LifecycleWatcher({super.key});

  @override
  State<LifecycleWatcher> createState() => _LifecycleWatcherState();
}

class _LifecycleWatcherState extends State<LifecycleWatcher>
    with WidgetsBindingObserver {
  AppLifecycleState? _lastLifecycleState;

  @override
  void initState() {
    super.initState();
    WidgetsBinding.instance.addObserver(this);
  }

  @override
  void dispose() {
    WidgetsBinding.instance.removeObserver(this);
    super.dispose();
  }

  @override
  void didChangeAppLifecycleState(AppLifecycleState state) {
    setState(() {
      _lastLifecycleState = state;
    });
  }

  @override
  Widget build(BuildContext context) {
    if (_lastLifecycleState == null) {
      return const Text(
        'This widget has not observed any lifecycle changes.',
        textDirection: TextDirection.ltr,
      );
    }

    return Text(
      'The most recent lifecycle state this widget observed was: $_lastLifecycleState.',
      textDirection: TextDirection.ltr,
    );
  }
}

void main() {
  runApp(const Center(child: LifecycleWatcher()));
}
```

## Layouts

## 佈局

### What is the equivalent of a LinearLayout?

### LinearLayout 的對應概念是什麼？

In Android, a LinearLayout is used to lay your widgets out
linearly&mdash;either horizontally or vertically.
In Flutter, use the Row or Column
widgets to achieve the same result.

在 Android 中，LinearLayout 用於線性佈局 widget 的&mdash;&mdash;水平或者垂直。
在 Flutter 中，使用 Row 或者 Column Widget 來實現相同的效果。

If you notice the two code samples are identical with the exception of the
"Row" and "Column" widget. The children are the same and this feature can be
exploited to develop rich layouts that can change overtime with the same
children.

如果你注意看的話，會發現下面的兩段程式碼除了
`Row` 和 `Column` widget 以外是一模一樣的。
它們的子級是一樣的，
而這個特性可以被充分利用來開發包含有相同的子級，
但是會隨時間改變的複雜佈局。

<?code-excerpt "lib/layout.dart (Row)"?>
```dart
@override
Widget build(BuildContext context) {
  return const Row(
    mainAxisAlignment: MainAxisAlignment.center,
    children: <Widget>[
      Text('Row One'),
      Text('Row Two'),
      Text('Row Three'),
      Text('Row Four'),
    ],
  );
}
```

<?code-excerpt "lib/layout.dart (Column)"?>
```dart
@override
Widget build(BuildContext context) {
  return const Column(
    mainAxisAlignment: MainAxisAlignment.center,
    children: <Widget>[
      Text('Column One'),
      Text('Column Two'),
      Text('Column Three'),
      Text('Column Four'),
    ],
  );
}
```

To learn more about building linear layouts,
see the community-contributed Medium article
[Flutter for Android Developers: How to design LinearLayout in Flutter][].

如果想學習更多的建構線性佈局的內容，請閱讀社群貢獻的 Medium 文章 
[給 Android 開發者的 Flutter 指南：如何在 Flutter 中設計線性佈局]
[Flutter for Android Developers: How to design LinearLayout in Flutter]。

### What is the equivalent of a RelativeLayout?

### RelativeLayout 的對應概念是什麼？

A RelativeLayout lays your widgets out relative to each other. In
Flutter, there are a few ways to achieve the same result.

RelativeLayout 透過 Widget 的相互位置對它們進行佈局。在 Flutter 中，
有幾種實現相同效果的方法。

You can achieve the result of a RelativeLayout by using a combination of
Column, Row, and Stack widgets. You can specify rules for the widgets
constructors on how the children are laid out relative to the parent.

你可以透過組合使用 Column、Row 和 Stack Widget 實現 RelativeLayout 的效果。
你還可以在 Widget 構造器內宣告孩子相對父親的佈局規則。

For a good example of building a RelativeLayout in Flutter,
see Collin's answer on [StackOverflow][].

Collin 在 [StackOverflow][] 上的回答是一個在 Flutter 中建構相對佈局的好例子。

### What is the equivalent of a ScrollView?

### ScrollView 的對應概念是什麼？

In Android, use a ScrollView to lay out your widgets&mdash;if the user's
device has a smaller screen than your content, it scrolls.

在 Android 中，使用 ScrollView 佈局 widget&mdash;&mdash;
如果使用者的裝置螢幕比應用的內容區域小，使用者可以滑動內容。

In Flutter, the easiest way to do this is using the ListView widget.
This might seem like overkill coming from Android,
but in Flutter a ListView widget is
both a ScrollView and an Android ListView.

在 Flutter 中，實現這個功能的最簡單的方法是使用 ListView widget。
從 Android 的角度看，這樣做可能是殺雞用牛刀了，
但是 Flutter 中 ListView widget 既是一個 ScrollView，
也是一個 Android 中的 ListView。

<?code-excerpt "lib/layout.dart (ListView)"?>
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

### 在 Flutter 中如何處理螢幕旋轉？

FlutterView handles the config change if AndroidManifest.xml contains:

FlutterView 會處理配置的變化，前提條件是在 AndroidManifest.xml 檔案中聲明瞭：

```yaml
android:configChanges="orientation|screenSize"
```

## Gesture detection and touch event handling

## 手勢監聽和觸控事件處理

### How do I add an onClick listener to a widget in Flutter?

### Flutter 中如何為一個 Widget 新增點選監聽器？

In Android, you can attach onClick to views such as button by calling
the method 'setOnClickListener'.

在 Android 中，你可以透過呼叫 `setOnClickListener`
方法在按鈕這樣的 View 上新增點選監聽器。

In Flutter there are two ways of adding touch listeners:

在 Flutter 中有兩種新增觸控監聽器的方法：

 1. If the Widget supports event detection, pass a function to it and handle it
    in the function. For example, the ElevatedButton has an `onPressed` parameter:

    如果 Widget 支援事件監聽，那麼向它傳入一個方法並在方法中處理事件。
    例如，RaisedButton 有一個 `onPressed` 引數：

  <?code-excerpt "lib/events.dart (onPressed)"?>
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

 2. If the Widget doesn't support event detection, wrap the
    widget in a GestureDetector and pass a function to the `onTap` parameter.

    如果 Widget 不支援事件監聽，將 Widget 包裝進一個 GestureDetector 中
    並向 `onTap` 引數傳入一個方法。

  <?code-excerpt "lib/events.dart (onTap)"?>
  ```dart
  class SampleTapApp extends StatelessWidget {
    const SampleTapApp({super.key});

    @override
    Widget build(BuildContext context) {
      return Scaffold(
        body: Center(
          child: GestureDetector(
            onTap: () {
              developer.log('tap');
            },
            child: const FlutterLogo(
              size: 200,
            ),
          ),
        ),
      );
    }
  }
  ```

### How do I handle other gestures on widgets?

### 如何處理 Widget 上的其它手勢？

Using the GestureDetector, you can listen to a wide range of Gestures such as:

使用 GestureDetector 可以監聽非常多的手勢，例如：

* Tap

  * `onTapDown` - A pointer that might cause a tap has contacted the screen at a
     particular location.

    `onTapDown` - 一個可能產生點選事件的指標觸控到螢幕的特定位置。

  * `onTapUp` - A pointer that triggers a tap has stopped contacting the
     screen at a particular location.

    `onTapUp` - 一個產生了點選事件的指標停止觸控式螢幕幕的特定位置。

  * `onTap` - A tap has occurred.
   
    `onTap` - 一個已經發生的點選事件。

  * `onTapCancel` - The pointer that previously triggered the `onTapDown` won't
     cause a tap.

    `onTapCancel` - 之前觸發了 `onTapDown` 事件的指標不會產生點選事件。

* Double tap

  * `onDoubleTap` - The user tapped the screen at the same location twice in
     quick succession.

    `onDoubleTap` - 使用者在螢幕同一位置連續快速地點選兩次。

* Long press

  * `onLongPress` - A pointer has remained in contact with the screen at
    the same location for a long period of time.

    `onLongPress` - 指標在螢幕的同一位置保持了一段較長時間的觸控狀態。

* Vertical drag

  * `onVerticalDragStart` - A pointer has contacted the screen and
    might begin to move vertically.

    `onVerticalDragStart` - 指標已經觸控式螢幕幕並可能開始垂直移動。

  * `onVerticalDragUpdate` - A pointer in contact with the screen
    has moved further in the vertical direction.

    `onVerticalDragUpdate` - 觸控式螢幕幕的指標在垂直方向移動了更多的距離。

  * `onVerticalDragEnd` - A pointer that was previously in contact with the
    screen and moving vertically is no longer in contact with the screen and was
    moving at a specific velocity when it stopped contacting the screen.

    `onVerticalDragEnd` - 之前和螢幕接觸並垂直移動的指標不再繼續和螢幕接觸，
    並且在和螢幕停止接觸的時候以一定的速度移動。

* Horizontal drag

  * `onHorizontalDragStart` - A pointer has contacted the screen and might begin
    to move horizontally.

    `onHorizontalDragStart` - 指標已經觸控式螢幕幕並可能開始水平移動。

  * `onHorizontalDragUpdate` - A pointer in contact with the screen
    has moved further in the horizontal direction.

    `onHorizontalDragUpdate` - 觸控式螢幕幕的指標在水平方向移動了更多的距離。

  * `onHorizontalDragEnd` - A pointer that was previously in contact with the
    screen and moving horizontally is no longer in contact with the
    screen and was moving at a specific velocity when it stopped
    contacting the screen.

    `onHorizontalDragEnd` - 之前和螢幕接觸並水平移動的指標不再繼續和螢幕接觸，
    並且在和螢幕停止接觸的時候以一定的速度移動。

The following example shows a `GestureDetector`
that rotates the Flutter logo on a double tap:

下面的例子展示了一個實現了雙擊旋轉 Flutter 標誌的 `GestureDetector`：

<?code-excerpt "lib/events.dart (SampleApp)"?>
```dart
class SampleApp extends StatefulWidget {
  const SampleApp({super.key});

  @override
  State<SampleApp> createState() => _SampleAppState();
}

class _SampleAppState extends State<SampleApp>
    with SingleTickerProviderStateMixin {
  late AnimationController controller;
  late CurvedAnimation curve;

  @override
  void initState() {
    super.initState();
    controller = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 2000),
    );
    curve = CurvedAnimation(
      parent: controller,
      curve: Curves.easeIn,
    );
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
            child: const FlutterLogo(
              size: 200,
            ),
          ),
        ),
      ),
    );
  }
}
```

## Listviews & adapters

## 列表檢視和介面卡

### What is the alternative to a ListView in Flutter?

### ListView 在 Flutter 中的對應概念是什麼？

The equivalent to a ListView in Flutter is … a ListView!

Flutter 中 ListView 的對應概念仍然是...ListView！

In an Android ListView, you create an adapter and pass it into the
ListView, which renders each row with what your adapter returns. However, you
have to make sure you recycle your rows, otherwise, you get all sorts of crazy
visual glitches and memory issues.

使用 Android 的 ListView 時，建立一個 adapter 並將其傳給 ListView，
ListView 渲染 adapter 返回的每一行內容。
然後，你需要確保回收了每一行檢視，否則，你會遇到各種奇怪的介面和記憶體問題。

Due to Flutter's immutable widget pattern, you pass a list of
widgets to your ListView, and Flutter takes care of making sure
that scrolling is fast and smooth.

因為 Flutter widget 不可變的特點，你需要向 ListView 傳入一組 widget，
Flutter 會保證滑動的快速順暢。

<?code-excerpt "lib/listview.dart"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  const SampleApp({super.key});
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sample App'),
      ),
      body: ListView(children: _getListData()),
    );
  }

  List<Widget> _getListData() {
    List<Widget> widgets = [];
    for (int i = 0; i < 100; i++) {
      widgets.add(Padding(
        padding: const EdgeInsets.all(10),
        child: Text('Row $i'),
      ));
    }
    return widgets;
  }
}
```

### How do I know which list item is clicked on?

### 如何知道點選了哪個列表項？

In Android, the ListView has a method to find out which item was clicked,
'onItemClickListener'.
In Flutter, use the touch handling provided by the passed-in widgets.

在 Android 中，ListView 有一個可以幫助你定位哪個列表項被點選了的方法 `onItemClickListener`。
在 Flutter 中，則使用傳入 widget 的觸控監聽。

<?code-excerpt "lib/list_item_tapped.dart"?>
```dart
import 'dart:developer' as developer;

import 'package:flutter/material.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  const SampleApp({super.key});
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  const SampleAppPage({super.key});

  @override
  State<SampleAppPage> createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sample App'),
      ),
      body: ListView(children: _getListData()),
    );
  }

  List<Widget> _getListData() {
    List<Widget> widgets = [];
    for (int i = 0; i < 100; i++) {
      widgets.add(
        GestureDetector(
          onTap: () {
            developer.log('row tapped');
          },
          child: Padding(
            padding: const EdgeInsets.all(10),
            child: Text('Row $i'),
          ),
        ),
      );
    }
    return widgets;
  }
}
```

### How do I update ListView's dynamically?

### 如何動態更新 ListView？

On Android, you update the adapter and call `notifyDataSetChanged`.

在 Android 中，你需要更新 adapter 並呼叫 `notifyDataSetChanged`。

In Flutter, if you were to update the list of widgets inside a `setState()`,
you would quickly see that your data did not change visually.
This is because when `setState()` is called, the Flutter rendering engine
looks at the widget tree to see if anything has changed. When it gets to your
`ListView`, it performs a `==` check, and determines that the two
`ListView`s are the same. Nothing has changed, so no update is required.

在 Flutter 中，如果你準備在 `setState()` 裡更新一組 widget，
你很快會發現你的資料並沒有更新到介面上。
這是因為當 `setState()` 被呼叫的時候，
Flutter 渲染引擎會檢視 Widget 樹是否有任何更改。
當引擎檢查到 `ListView`，他會執行 `==` 檢查，並判斷兩個 `ListView` 是一樣的。
沒有任何更改，所以也就不需要更新。

For a simple way to update your `ListView`, create a new `List` inside of
`setState()`, and copy the data from the old list to the new list.
While this approach is simple, it is not recommended for large data sets,
as shown in the next example.

更新 `ListView` 的一個簡單方法是，
在 `setState()` 裡建立一個新的 `List`，並將資料從舊列表複製到新列表。
雖然這個方法很簡單，就如下面例子所示，但是並不推薦在大資料集的時候使用。

<?code-excerpt "lib/listview_dynamic.dart"?>
```dart
import 'dart:developer' as developer;

import 'package:flutter/material.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  const SampleApp({super.key});
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const SampleAppPage(),
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

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sample App'),
      ),
      body: ListView(children: widgets),
    );
  }

  Widget getRow(int i) {
    return GestureDetector(
      onTap: () {
        setState(() {
          widgets = List.from(widgets);
          widgets.add(getRow(widgets.length));
          developer.log('row $i');
        });
      },
      child: Padding(
        padding: const EdgeInsets.all(10),
        child: Text('Row $i'),
      ),
    );
  }
}
```

The recommended, efficient, and effective way to build a list uses a
`ListView.Builder`. This method is great when you have a dynamic
`List` or a `List` with very large amounts of data. This is essentially
the equivalent of RecyclerView on Android, which automatically
recycles list elements for you:

推薦的高效且有效的建立一個列表的方法是使用 ListView.Builder。這個方法非常適用於
動態列表或者擁有大量資料的列表。這基本上就是 Android 裡的 RecyclerView，會為你
自動回收列表項：

<?code-excerpt "lib/listview_builder.dart"?>
```dart
import 'dart:developer' as developer;

import 'package:flutter/material.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  const SampleApp({super.key});
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const SampleAppPage(),
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

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sample App'),
      ),
      body: ListView.builder(
        itemCount: widgets.length,
        itemBuilder: (context, position) {
          return getRow(position);
        },
      ),
    );
  }

  Widget getRow(int i) {
    return GestureDetector(
      onTap: () {
        setState(() {
          widgets.add(getRow(widgets.length));
          developer.log('row $i');
        });
      },
      child: Padding(
        padding: const EdgeInsets.all(10),
        child: Text('Row $i'),
      ),
    );
  }
}
```

Instead of creating a "ListView", create a
`ListView.builder` that takes two key parameters: the
initial length of the list, and an `ItemBuilder` function.

不用建立一個 "ListView"，而是建立接收兩個引數的 ListView.Builder，
兩個引數分別是列表的初始長度和一個 `ItemBuilder` 方法。

The `ItemBuilder` function is similar to the `getView`
function in an Android adapter; it takes a position,
and returns the row you want rendered at that position.

ItemBuilder 方法和 Android adapter 裡的 `getView` 方法類似；
它透過位置返回你期望在 這個位置渲染的列表項。

Finally, but most importantly, notice that the `onTap()` function
doesn't recreate the list anymore, but instead `.add`s to it.

最後也是最重要的一條，需要注意 `onTap()` 方法不再重建列表項，但是會執行 `.add` 操作。

## Working with text

## 文字處理

### How do I set custom fonts on my Text widgets?

### 如何為 Text Widget 設定自訂字型？

In Android SDK (as of Android O), you create a Font resource file and
pass it into the FontFamily param for your TextView.

在 Android SDK 中（從 Android O 開始），你可以建立一個字型資原始檔並將其
傳給 TextView 的 FontFamily 引數。

In Flutter, place the font file in a folder and reference it in the
`pubspec.yaml` file, similar to how you import images.

在 Flutter 中，將字型檔案放入一個資料夾，
並在 `pubspec.yaml` 檔案中參考它，和匯入圖片一樣。

```yaml
fonts:
   - family: MyCustomFont
     fonts:
       - asset: fonts/MyCustomFont.ttf
       - style: italic
```

Then assign the font to your `Text` widget:

然後將字型賦值給你的 `Text` Widget：

<?code-excerpt "lib/text.dart (CustomFont)"?>
```dart
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: const Text('Sample App'),
    ),
    body: const Center(
      child: Text(
        'This is a custom font text',
        style: TextStyle(fontFamily: 'MyCustomFont'),
      ),
    ),
  );
}
```

### How do I style my Text widgets?

### 如何更改 Text Widget 的樣式？

Along with fonts, you can customize other styling elements on a `Text` widget.
The style parameter of a `Text` widget takes a `TextStyle` object, where you can
customize many parameters, such as:

除了字型，你還可以自訂 `Text` Widget 的其它樣式元素。
`Text` Widget 的樣式引數接收一個 `TextStyle` 物件，
你可以在這個物件裡自訂很多引數，例如：

* color
* decoration
* decorationColor
* decorationStyle
* fontFamily
* fontSize
* fontStyle
* fontWeight
* hashCode
* height
* inherit
* letterSpacing
* textBaseline
* wordSpacing

## Form input

## 表單輸入

For more information on using Forms,
see [Retrieve the value of a text field][],
from the [Flutter cookbook][].

如果需要更多使用表單的資訊，請檢視
[Flutter Cookbook][Flutter Cookbook] 中的
[檢索一個文字欄位的值][Retrieve the value of a text field]。

### What is the equivalent of a "hint" on an Input?

### Input 的「提示」 (hint) 的對應概念是什麼？

In Flutter, you can easily show a "hint" or a placeholder text for your input by
adding an InputDecoration object to the decoration constructor parameter for
the Text Widget.

在 Flutter 中，你可以簡單地透過向 Text Widget 構造器的 decoration 引數
傳入一個 InputDecoration 物件來為輸入框展示一個「提示」或佔位文字。

<?code-excerpt "lib/form.dart (InputHint)" replace="/return const //g;/;//g"?>
```dart
Center(
  child: TextField(
    decoration: InputDecoration(hintText: 'This is a hint'),
  ),
)
```

### How do I show validation errors?

### 如何顯示驗證錯誤的資訊？

Just as you would with a "hint", pass an InputDecoration object
to the decoration constructor for the Text widget.

就像上面實現「提示」功能一樣，像 Text Widget 構造方法的 decoration 引數傳入
一個 InputDecoration 物件。

However, you don't want to start off by showing an error.
Instead, when the user has entered invalid data,
update the state, and pass a new `InputDecoration` object.

然而，你並不想一開始就顯示錯誤資訊。相反，當用戶輸入了無效的資訊後，
更新狀態並傳入一個新的 `InputDecoration` 物件。

<?code-excerpt "lib/validation_errors.dart"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const SampleApp());
}

class SampleApp extends StatelessWidget {
  const SampleApp({super.key});
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const SampleAppPage(),
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

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sample App'),
      ),
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

  String? _getErrorText() {
    return _errorText;
  }

  bool isEmail(String em) {
    String emailRegexp =
        r'^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|'
        r'(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|'
        r'(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$';

    RegExp regExp = RegExp(emailRegexp);

    return regExp.hasMatch(em);
  }
}
```


## Flutter plugins

## Flutter 外掛

### How do I access the GPS sensor?

### 如何使用 GPS 感測器？

Use the [`geolocator`][] community plugin.

使用 [`geolocator`][] 社群外掛。

### How do I access the camera?

### 如何使用相機？

The [`image_picker`][] plugin is popular
for accessing the camera.

[`image_picker`][] 外掛被常用於相機功能的使用。

### How do I log in with Facebook?

### 如何使用 Facebook 登入？

To Log in with Facebook, use the
[`flutter_facebook_login`][] community plugin.

使用 [`flutter_facebook_login`][] 社群外掛實現
Facebook 登入功能。

### How do I use Firebase features?

### 如何使用 Firebase 的功能？

Most Firebase functions are covered by
[first party plugins][].
These plugins are first-party integrations,
maintained by the Flutter team:

[官方外掛][first party plugins] 提供了 Firebase 的大多數功能。
這些外掛都是由 Flutter 團隊維護的官方整合外掛：

 * [`google_mobile_ads`][] for Google Mobile Ads for Flutter

   [`google_mobile_ads`][] 提供 Google Mobile Ads 功能

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

 * [`flutter_firebase_ui`][] for quick Firebase Auth integrations
   (Facebook, Google, Twitter and email)

   [`flutter_firebase_ui`][] 提供快速的 Firebase Auth 整合功能 (Facebook, Google, Twitter 和 email)

 * [`cloud_firestore`][] for Firebase Cloud Firestore
  
   [`cloud_firestore`][] 提供 Firebase Cloud Firestore 功能

You can also find some third-party Firebase plugins on
pub.dev that cover areas not directly covered by the
first-party plugins.

你可以在 [Pub](https://pub.dev) 網站上查詢一些官方外掛
沒有直接支援的功能的第三方 Firebase 外掛。

### How do I build my own custom native integrations?

### 如何建立自己的自訂原生整合外掛？

If there is platform-specific functionality that Flutter
or its community Plugins are missing,
you can build your own following the
[developing packages and plugins][] page.

如果有 Flutter 官方或社群第三方外掛沒有涵蓋的平台特定的功能，
你可以參考 [開發套件和外掛][developing packages and plugins]
文件建立自己的外掛。

Flutter's plugin architecture, in a nutshell, is much like using an Event bus in
Android: you fire off a message and let the receiver process and emit a result
back to you. In this case, the receiver is code running on the native side
on Android or iOS.

Flutter 的外掛架構，簡而言之，和 Android 中的事件匯流排的使用非常相似：
你傳送一個訊息，並讓接受者處理並返回一個結果給你。
在這種情況下，接受者是執行在 Android 或 iOS 原生端的程式碼。

### How do I use the NDK in my Flutter application?

### 如何在 Flutter 應用中使用 NDK？

If you use the NDK in your current Android application and want your Flutter
application to take advantage of your native libraries then it's possible by
building a custom plugin.

如果你在現有的 Android 應用中使用 NDK，
並且希望你的 Flutter 應用可以利用你的 native 庫，
你可以透過建立一個自訂外掛實現。

Your custom plugin first talks to your Android app, where you call your
`native` functions over JNI. Once a response is ready,
send a message back to Flutter and render the result.

你的自訂外掛首先和你的 Android 應用通訊，Android 應用會透過 JNI 呼叫 `native` 方法。
一旦有返回值，就可以向 Flutter 傳送回一個訊息並渲染結果。

Calling native code directly from Flutter is currently not supported.

暫時還不支援從 Flutter 中直接呼叫 native 程式碼。

## Themes

## 主題(Themes)

### How do I theme my app?

### 如何對應用使用主題？

Out of the box, Flutter comes with a beautiful implementation of Material
Design, which takes care of a lot of styling and theming needs that you would
typically do. Unlike Android where you declare themes in XML and then assign it
to your application using AndroidManifest.xml, in Flutter you declare themes
in the top level widget.

Flutter 提供開箱即用的優美的 Material Design 實現，
可以滿足你通常需要的各種樣式和主題的需求。
不同於 Android 中你在 XML 檔案中定義主題並在
AndroidManifest.xml 中將其賦值給你的應用，
Flutter 中是在最上層 Widget 上宣告主題。

To take full advantage of Material Components in your app, you can declare a top
level widget `MaterialApp` as the entry point to your application. MaterialApp
is a convenience widget that wraps a number of widgets that are commonly
required for applications implementing Material Design.
It builds upon a WidgetsApp by adding Material specific functionality.

為了在應用中利用好 Material 元件，
你可以在應用中宣告一個最上層 Widget `MaterialApp` 作為入口。
MaterialApp 是一個包裝了一系列 Widget 的為你給予便利的 Widget，
而這些 Widget 通常是實現 Material Design 的應用所必須的。
它基於 WidgetsApp 並添加了 Material 相關的功能。

You can also use a `WidgetsApp` as your app widget, which provides some of the
same functionality, but is not as rich as `MaterialApp`.

你也可以使用 `WidgetApp` 作為應用的 Widget，它會提供一些相同的功能，
但是不如 `MaterialApp` 提供的功能豐富。

To customize the colors and styles of any child components, pass a
`ThemeData` object to the `MaterialApp` widget. For example, in the code below,
the color scheme from seed is set to deepPurple and text selection color is red.

如果要自訂任意子元件的顏色或者樣式，
給 `MaterialApp` Widget 傳入一個 `ThemeData` 物件即可。
例如，在下面的程式碼中，主色調設定為深紫色，文字選中顏色設定為紅色。

<?code-excerpt "lib/theme.dart (Theme)"?>
```dart
import 'package:flutter/material.dart';

class SampleApp extends StatelessWidget {
  const SampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        textSelectionTheme:
            const TextSelectionThemeData(selectionColor: Colors.red),
      ),
      home: const SampleAppPage(),
    );
  }
}
```


## Databases and local storage

## 資料庫和本地儲存

### How do I access Shared Preferences?

### 如何使用 Shared Preferences？

In Android, you can store a small collection of key-value pairs using
the SharedPreferences API.

在 Android 中，你可以使用 SharedPreferences API 來儲存少量的鍵值對。

In Flutter, access this functionality using the
[Shared_Preferences plugin][].
This plugin wraps the functionality of both
Shared Preferences and NSUserDefaults (the iOS equivalent).

在 Flutter 中，使用 [Shared_Preferences 外掛][Shared_Preferences plugin] 實現此功能。
這個外掛同時包裝了 Shared Preferences 和 NSUserDefaults（iOS 平台對應 API）的功能。

<?code-excerpt "lib/shared_prefs.dart"?>
```dart
import 'dart:async';
import 'package:flutter/material.dart';

import 'package:shared_preferences/shared_preferences.dart';

void main() {
  runApp(
    const MaterialApp(
      home: Scaffold(
        body: Center(
          child: ElevatedButton(
            onPressed: _incrementCounter,
            child: Text('Increment Counter'),
          ),
        ),
      ),
    ),
  );
}

Future<void> _incrementCounter() async {
  SharedPreferences prefs = await SharedPreferences.getInstance();
  int counter = (prefs.getInt('counter') ?? 0) + 1;
  await prefs.setInt('counter', counter);
}
```

### How do I access SQLite in Flutter?

### 在 Flutter 中如何使用 SQLite？

In Android, you use SQLite to store structured data that you can query
using SQL.

在 Android 中，你會使用 SQLite 來儲存可以透過 SQL 進行查詢的結構化資料。

In Flutter, access this functionality using the
[SQFlite][] plugin.

在 Flutter 中，使用 [SQFlite][] 外掛實現此功能。

## Debugging

## 除錯

### What tools can I use to debug my app in Flutter?

### 我可以使用什麼工具除錯我的 Flutter 應用？

Use the [DevTools][] suite for debugging Flutter or Dart apps.

請使用 [開發者工具][DevTools] 除錯你的 Flutter 和 Dart 應用。

DevTools includes support for profiling, examining the heap,
inspecting the widget tree, logging diagnostics, debugging,
observing executed lines of code, debugging memory leaks and memory
fragmentation. For more information, see the
[DevTools][] documentation.

開發者工具套件含了效能工具、檢查堆疊、檢視 widget 樹、診斷資訊記錄、除錯、
執行程式碼行觀察、除錯記憶體洩漏和記憶體碎片等。
有關更多資訊，請參閱 [開發者工具][DevTools] 文件。

## Notifications

## 通知

### How do I set up push notifications?

### 如何設定推送通知？

In Android, you use Firebase Cloud Messaging to setup push
notifications for your app.

在 Android 中，你可以使用 Firebase Cloud Messaging 來為應用設定推送通知。

In Flutter, access this functionality using the
[Firebase Messaging][] plugin.
For more information on using the Firebase Cloud Messaging API,
see the [`firebase_messaging`][] plugin documentation.

在 Flutter 中，則使用 [Firebase Messaging][] 外掛實現此功能。
想要獲得更多關於使用 Firebase Cloud Messaging API 的資訊，
請查閱 [`firebase_messaging`][] 外掛文件。

[Add Flutter to existing app]: {{site.url}}/add-to-app
[Animation & Motion widgets]: {{site.url}}/ui/widgets/animation
[Animations tutorial]: {{site.url}}/ui/animations/tutorial
[Animations overview]: {{site.url}}/ui/animations
[`AppLifecycleStatus` documentation]: {{site.api}}/flutter/dart-ui/AppLifecycleState.html
[Apple's iOS design language]: {{site.apple-dev}}/design/resources/
[`cloud_firestore`]: {{site.pub}}/packages/cloud_firestore
[composing]: {{site.url}}/resources/architectural-overview#composition
[Cupertino widgets]: {{site.url}}/ui/widgets/cupertino
[developing packages and plugins]: {{site.url}}/packages-and-plugins/developing-packages
[`devicePixelRatio`]: {{site.api}}/flutter/dart-ui/FlutterView/devicePixelRatio.html
[DevTools]: {{site.url}}/tools/devtools
[existing plugin]: {{site.pub}}/flutter/
[`flutter_facebook_login`]: {{site.pub}}/packages/flutter_facebook_login
[`google_mobile_ads`]: {{site.pub}}/packages/google_mobile_ads
[`firebase_analytics`]: {{site.pub}}/packages/firebase_analytics
[`firebase_auth`]: {{site.pub}}/packages/firebase_auth
[`firebase_database`]: {{site.pub}}/packages/firebase_database
[`firebase_messaging`]: {{site.pub}}/packages/firebase_messaging
[`firebase_storage`]: {{site.pub}}/packages/firebase_storage
[`flutter_firebase_ui`]: {{site.pub}}/packages/flutter_firebase_ui
[Firebase Messaging]: {{site.github}}/FirebaseExtended/flutterfire/tree/master/packages/firebase_messaging
[first party plugins]: {{site.pub}}/flutter/packages?q=firebase
[Flutter cookbook]: {{site.url}}/cookbook
[Flutter for Android Developers: How to design LinearLayout in Flutter]: https://proandroiddev.com/flutter-for-android-developers-how-to-design-linearlayout-in-flutter-5d819c0ddf1a
[Flutter for Android Developers: How to design Activity UI in Flutter]: https://blog.usejournal.com/flutter-for-android-developers-how-to-design-activity-ui-in-flutter-4bf7b0de1e48
[`geolocator`]: {{site.pub}}/packages/geolocator
[`http` package]: {{site.pub}}/packages/http
[`image_picker`]: {{site.pub}}/packages/image_picker
[Intents]: #what-is-the-equivalent-of-an-intent-in-flutter
[intl package]: {{site.pub}}/packages/intl
[Introduction to declarative UI]: {{site.url}}/get-started/flutter-for/declarative
[Material Components]: {{site.material}}/develop/flutter
[Material Design guidelines]: {{site.material}}/styles
[optimized for all platforms]: {{site.material}}/develop
[a plugin]: {{site.pub}}/packages/android_intent
[pub.dev]: {{site.pub}}/flutter/packages/
[Retrieve the value of a text field]: {{site.url}}/cookbook/forms/retrieve-input
[Shared_Preferences plugin]: {{site.pub}}/packages/shared_preferences
[SQFlite]: {{site.pub}}/packages/sqflite
[StackOverflow]: {{site.so}}/questions/44396075/equivalent-of-relativelayout-in-flutter
[widget catalog]: {{site.url}}/ui/widgets/layout
