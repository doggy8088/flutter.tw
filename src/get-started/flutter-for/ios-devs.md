---
title: Flutter for iOS developers
title: 給 iOS 開發者的 Flutter 指南
description: Learn how to apply iOS developer knowledge when building Flutter apps.
description: 學習如何將 iOS 開發經驗應用到 Flutter 應用開發中。
tags: Flutter課程,Flutter起步,Flutter入門
keywords: Flutter iOS,iOS,用Flutter開發iOS,Cupertino
---

<?code-excerpt path-base="get-started/flutter-for/ios_devs"?>

This document is for iOS developers looking to apply
their existing iOS knowledge to build mobile apps with Flutter.
If you understand the fundamentals of the iOS framework
then you can use this document as a
way to get started learning Flutter development.

這篇文章是為那些想將已有的 iOS 開發經驗運用到 Flutter 開發中的 iOS 開發者所作。
如果你理解 iOS framework 的基本原理，那麼你可以將這篇文章作為學習 Flutter 開發的起點。

{{site.alert.note}}

  To integrate Flutter code into your iOS app, see
  [Add Flutter to existing app][].

  要向你的 Android 應用中整合 Flutter，請參閱
  [向已有應用中新增 Flutter][Add Flutter to existing app]。

{{site.alert.end}}

Before diving into this doc, you might want to watch a 15-minute video from
the [Flutter Youtube channel][] about
the Cupertino package.

在開始本文件之前，建議先瀏覽一下這個 15 分鐘的影片，
瞭解一下 Cupertino package 是什麼吧：

<iframe width="560" height="315" src="https://player.bilibili.com/player.html?aid=55647852&bvid=BV1B4411V79J&cid=97286350&page=1" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Your iOS knowledge and skill set
are highly valuable when building with Flutter,
because Flutter relies on the mobile operating system
for numerous capabilities and configurations.
Flutter is a new way to build UIs for mobile,
but it has a plugin system to communicate
with iOS (and Android) for non-UI tasks.
If you're an expert in iOS development,
you don't have to relearn everything to use Flutter.

你的 iOS 開發技能對於開發 Flutter 而言非常寶貴，
因為 Flutter 也依賴作業系統進行眾多功能和配置。
Flutter 是一種全新的建構移動應用的方式，
同時它也包含了可以與 iOS（和 Android）進行非 UI 通訊的外掛系統。
如果你已經是 iOS 開發專家，那麼你並不需要重新學習 Flutter 的所有內容。

Flutter also already makes a number of adaptations
in the framework for you when running on iOS.
For a list, see [Platform adaptations][].

Flutter 早已對在 iOS 上執行 Flutter 框架作了許多最佳化。
若你需要檢視相關內容，請閱讀 [平台適配][Platform adaptations]。

This document can be used as a cookbook by jumping around
and finding questions that are most relevant to your needs.

你同樣可以將這篇文章當作一份手冊檢視，以便查詢並解決你所遇到的問題。

## Views

## 檢視

### What is the equivalent of a UIView in Flutter?

### `UIView` 相當於 Flutter 中的什麼？

{{site.alert.secondary}}

  How is react-style, or _declarative_,
  programming different from the
  traditional imperative style?
  For a comparison, see [Introduction to declarative UI][].

  響應式或者宣告式的程式設計和傳統的命令式風格有什麼不同？
  作為對比，請查閱 [宣告式 UI 介紹][Introduction to declarative UI]。

{{site.alert.end}}

On iOS, most of what you create in the UI is done using view objects,
which are instances of the `UIView` class.
These can act as containers for other `UIView` classes,
which form your layout.

在 iOS 中，你在 UI 中建立的大部分檢視都是 `UIView` 的例項。
而在構造佈局時，這些檢視也可以作為其他檢視的容器。

In Flutter, the rough equivalent to a `UIView` is a `Widget`.
Widgets don't map exactly to iOS views,
but while you're getting acquainted with how Flutter works
you can think of them as "the way you declare and construct UI".

在 Flutter 中，同 `UIView` 能夠進行類比的就是 `Widget` 了。
但 `Widget` 和 iOS 裡的檢視並不能同等對待，
不過當你想要了解 Flutter 的工作原理時，你可以把它理解為
「宣告和構造 UI 的方法」。

However, these have a few differences to a `UIView`.
To start, widgets have a different lifespan: they are immutable
and only exist until they need to be changed.
Whenever widgets or their state change,
Flutter’s framework creates a new tree of widget instances.
In comparison, an iOS view is not recreated when it changes,
but rather it's a mutable entity that is drawn once
and doesn't redraw until it is invalidated using `setNeedsDisplay()`.

然而，`Widget` 和 `UIView` 還是有著相當一部分區別的。
首先，widget 擁有著不同的生命週期：
整個生命週期內它是不可變的，且只能夠存活到被修改的時候。
一旦 widget 例項或者它的狀態發生了改變，
Flutter 框架就會建立一個新的由 `Widget` 例項構造而成的樹狀結構。
而在 iOS 裡，修改一個檢視並不會導致它重新建立例項，
它作為一個可變物件，只會繪製一次，
只有在發生 `setNeedsDisplay()` 呼叫之後才會發生重繪。

Furthermore, unlike `UIView`, Flutter’s widgets are lightweight,
in part due to their immutability.
Because they aren't views themselves,
and aren't directly drawing anything,
but rather are a description of the UI and its semantics
that get "inflated" into actual view objects under the hood.

還有，和 `UIView` 不同，Flutter 的 widget 是很輕量的，
一部分原因就是源於它的不可變特性。
因為它並不是檢視，也不直接繪製任何內容，
而是作為對 UI 及其特性的一種描述，被「注入」到檢視中去。

Flutter includes the [Material Components][] library.
These are widgets that implement the
[Material Design guidelines][].
Material Design is a flexible design system
[optimized for all platforms][], including iOS.

Flutter 包含了 [Material Components][] 庫。內容都是一些遵循了
[Material Design 設計規範][Material Design guidelines] 的元件。
Material Design 是一種靈活的
[支援全平台][optimized for all platforms]
的設計體系，其中也包括了 iOS。

But Flutter is flexible and expressive enough
to implement any design language.
On iOS, you can use the [Cupertino widgets][]
to produce an interface that looks like
[Apple's iOS design language][].

但是 Flutter 的靈活性和表現力使其能夠適配任何的設計語言。
在 iOS 中，你可以透過 [Cupertino widgets][] 來構造類似於
[Apple iOS 設計語言][Apple's iOS design language] 的介面。

### How do I update widgets?

### 我該如何更新 widget？

To update your views on iOS, you directly mutate them.
In Flutter, widgets are immutable and not updated directly.
Instead, you have to manipulate the widget’s state.

在 iOS 可以直接對檢視進行修改。但是在 Flutter 中，
widget 都是不可變的，所以也不能夠直接對其修改。
所以，你必須透過修改 widget 的 state 來達到更新檢視的目的。

This is where the concept of Stateful vs Stateless widgets
comes in. A `StatelessWidget` is just what it sounds
like&mdash;a widget with no state attached.

這時我們就開始考慮有狀態和無狀態的 widget 的概念。
與字面意思相同的是，`StatelessWidget` 就是一個沒有繫結狀態的 widget。

`StatelessWidgets` are useful when the part of the user interface you are
describing does not depend on anything other than the initial configuration
information in the widget.

當某個 widget 不需要依賴任何別的初始配置來對這個 widget 進行描述時，
`StatelessWidgets` 會是很有用的。

For example, in iOS, this is similar to placing a `UIImageView`
with your logo as the `image`. If the logo is not changing during runtime,
use a `StatelessWidget` in Flutter.

舉個例子，在 iOS 中，你需要把 logo 當作
`image` 並將它放置在 `UIImageView` 中，
如果在執行時這個 logo 不會發生變化，
那麼對應 Flutter 中你應該使用 `StatelessWidget`。

If you want to dynamically change the UI based on data received
after making an HTTP call, use a `StatefulWidget`.
After the HTTP call has completed, tell the Flutter framework
that the widget’s `State` is updated, so it can update the UI.

但是如果你想要根據 HTTP 請求的返回結果動態的修改 UI，
那麼你應該使用 `StatefulWidget`。在 HTTP 請求結束後，
通知 Flutter 更新這個 widget 的 `State`，然後 UI 就會得到更新。

The important difference between stateless and
stateful widgets is that `StatefulWidget`s have a `State` object
that stores state data and carries it over across tree rebuilds,
so it's not lost.

`StatefulWidget` 和 `StatelessWidget` 最重要的區別就是，
`StatefulWidget` 中有一個 `State` 物件，
它用來儲存一些狀態的資訊，並在整個生命週期內保持不變。

If you are in doubt, remember this rule:
if a widget changes outside of the `build` method
(because of runtime user interactions, for example),
it’s stateful.
If the widget never changes, once built, it's stateless.
However, even if a widget is stateful, the containing parent widget
can still be stateless if it isn’t itself reacting to those changes
(or other inputs).

如果你對此還存有疑慮，記住一點：
如果一個 widget 在 `build` 方法之外
（比如執行時下發生使用者點選事件）被修改，
那麼就應該是有狀態的。
如果一個 widget 一旦產生就不再發生改變，那麼它就是無狀態的。
然而，即使一個 widget 是有狀態的，如果不是自身直接響應修改（或別的輸入），
那麼他的父容器也可以是無狀態的。

The following example shows how to use a `StatelessWidget`.
A common `StatelessWidget` is the `Text` widget.
If you look at the implementation of the `Text` widget,
you'll find it subclasses `StatelessWidget`.

下面是如何使用 `StatelessWidget` 的範例。
`Text` 是一個常用的 `StatelessWidget`。
如果你看了 `Text` 的原始碼，就會發現它繼承於 `StatelessWidget`。

<?code-excerpt "lib/text_widget.dart (TextWidget)" replace="/return const //g"?>
```dart
Text(
  'I like Flutter!',
  style: TextStyle(fontWeight: FontWeight.bold),
);
```

If you look at the code above, you might notice that the `Text` widget
carries no explicit state with it. It renders what is passed in its
constructors and nothing more.

看了上面的程式碼，你會注意到 `Text` 沒有攜帶任何狀態，
它只會渲染初始化時傳進來的內容。

But, what if you want to make "I Like Flutter" change dynamically,
for example when clicking a `FloatingActionButton`?

然而，如果你想要在點選一個 `FloatingActionButton` 時
動態地修改文字為「I Like Flutter」，該怎麼做呢？

To achieve this, wrap the `Text` widget in a `StatefulWidget` and
update it when the user clicks the button.

想要實現這個需求，只需要把 `Text` 放到 `StatefulWidget` 中，
並在使用者點選按鈕時更新它即可。

For example:

下面是範例程式碼：

<?code-excerpt "lib/text_widget.dart (StatefulWidget)"?>
```dart

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
  // Default placeholder text
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

### How do I lay out my widgets? Where is my Storyboard?

### 如何對 widget 做佈局？Storyboard 哪去了？

In iOS, you might use a Storyboard file
to organize your views and set constraints,
or you might set your constraints programmatically in your view controllers.
In Flutter, declare your layout in code by composing a widget tree.

在 iOS 開發中，你可能會經常使用 Storyboard 來組織你的檢視，
並直接透過 Storyboard 或者在 ViewController 中透過程式碼來設定約束。
而在 Flutter 中，你要透過程式碼來對 widget 進行組織來形成一個 widget 樹狀結構。

The following example shows how to display a simple widget with padding:

下面的例子展示瞭如何展示一個帶有內邊距的 widget：

<?code-excerpt "lib/layout.dart (SimpleWidget)"?>
```dart
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(title: const Text('Sample App')),
    body: Center(
      child: CupertinoButton(
        onPressed: () {},
        padding: const EdgeInsets.only(left: 10.0, right: 10.0),
        child: const Text('Hello'),
      ),
    ),
  );
}
```

You can add padding to any widget,
which mimics the functionality of constraints in iOS.

你可以為任何 widget 新增 padding，來達到類似在 iOS 中檢視約束的作用。

You can view the layouts that Flutter has to offer
in the [widget catalog][].

你可以在 [widget 目錄][widget catalog] 中
檢視 Flutter 提供的所有 widget 佈局方法。

### How do I add or remove a component from my layout?

### 如何增加或者移除一個元件？

In iOS, you call `addSubview()` on the parent,
or `removeFromSuperview()` on a child view
to dynamically add or remove child views.
In Flutter, because widgets are immutable,
there is no direct equivalent to `addSubview()`.
Instead, you can pass a function to the parent
that returns a widget, and control that child's creation
with a boolean flag.

在 iOS 中，你可以透過呼叫父檢視的 `addSubview()` 方法或者
`removeFromSuperview()` 方法來動態的新增或移除檢視。
在 Flutter 中，因為 widget 是不可變的，所以沒有提供與
`addSubview()` 作用相同的方法。
但是你可以透過向父檢視傳遞一個返回值是 widget 的方法，
並透過一個布林標識來控制子檢視的存在。

The following example shows how to toggle between two widgets
when the user clicks the `FloatingActionButton`:

下面的例子中向你展示瞭如何讓使用者透過點選 `FloatingActionButton`
按鈕來達到在兩個 widget 中切換的目的。

<?code-excerpt "lib/layout.dart (ToggleWidget)"?>
```dart
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
    }

    return CupertinoButton(
      onPressed: () {},
      child: const Text('Toggle Two'),
    );
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

### 如何新增動畫？

In iOS, you create an animation by calling the
`animate(withDuration:animations:)` method on a view.
In Flutter, use the animation library
to wrap widgets inside an animated widget.

在 iOS 裡，你可以使用呼叫檢視的 `animate(withDuration:animations:)` 方法來建立動畫。
在 Flutter 裡，透過使用動畫函式庫將 widget 封裝到動畫 widget 中來實現動畫效果。

In Flutter, use an `AnimationController`, which is an `Animation<double>`
that can pause, seek, stop, and reverse the animation.
It requires a `Ticker` that signals when vsync happens
and produces a linear interpolation
between 0 and 1 on each frame while it's running.
You then create one or more
`Animation`s and attach them to the controller.

在 Flutter 裡，使用 `AnimationController`，
它是一個可以暫停、查詢、停止和反轉動畫的 `Animation<double>` 型別。
它需要一個 `Ticker`，在螢幕重新整理時發出訊號量，
並在執行時對每一幀都產生一個 0~1 的線性插值。
然後你可以建立一個或多個 `Animation`，並把它們新增到控制器中。

For example, you might use `CurvedAnimation`
to implement an animation along an interpolated curve.
In this sense, the controller is the "master" source
of the animation progress
and the `CurvedAnimation` computes the curve
that replaces the controller's default linear motion.
Like widgets, animations in Flutter work with composition.

比如，你可以使用 `CurvedAnimation` 來實現一個曲線翻頁動畫。
這種情況下，控制器就是動畫進度的主要資料源，
而 `CurvedAnimation` 計算曲線並替換控制器的預設線性運動，
和 widget 一樣，在 Flutter 裡動畫也可以複合巢狀(Nesting)。

When building the widget tree you assign the `Animation` to an animated
property of a widget, such as the opacity of a `FadeTransition`,
and tell the controller to start the animation.

當建構一個 widget 樹時，可以將 `Animation` 賦值給 widget 使用者表現動畫能力的屬性，
比如 `FadeTransition` 的 opacity 屬性，然後告訴控制器啟動動畫。

The following example shows how to write a `FadeTransition` that
fades the widget into a logo when you press the `FloatingActionButton`:

下面的範例描述了當你點選 `FloatingActionButton` 時，
如何實現一個檢視漸淡出成 logo 的 `FadeTransition` 效果。

<?code-excerpt "lib/animation.dart"?>
```dart
import 'package:flutter/material.dart';

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  const SampleApp({super.key});

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

class _MyFadeTest extends State<MyFadeTest>
    with SingleTickerProviderStateMixin {
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
  void dispose() {
    controller.dispose();
    super.dispose();
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

關於更多的內容，可以檢視
[Animation 和 Motion widgets][Animation & Motion widgets]，
[Animations 課程][Animations tutorial]，
以及 [Animations 概覽][Animations overview]。

### How do I draw to the screen?

### 如何渲染到螢幕上？

On iOS, you use `CoreGraphics` to draw lines and shapes to the
screen. Flutter has a different API based on the `Canvas` class,
with two other classes that help you draw: `CustomPaint` and `CustomPainter`,
the latter of which implements your algorithm to draw to the canvas.

在 iOS 裡，可以使用 `CoreGraphics` 繪製線條和圖形到螢幕上。
Flutter 裡有一套基於 `Canvas` 實現的 API，有兩個類可以幫助你進行繪製：
`CustomPaint` 和 `CustomPainter`，後者實現了繪製圖形到 canvas 的演算法。

To learn how to implement a signature painter in Flutter,
see Collin's answer on [StackOverflow][].

想要學習在 Flutter 裡如何實現一個畫筆，
可以檢視 Collin 在 [StackOverflow][] 裡的回答。

[StackOverflow]: {{site.so}}/questions/46241071/create-signature-area-for-mobile-app-in-dart-flutter

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
  State<Signature> createState() => SignatureState();
}

class SignatureState extends State<Signature> {
  List<Offset?> _points = <Offset?>[];
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

### 如何設定檢視 widget 的透明度？

On iOS, everything has `.opacity` or `.alpha`.
In Flutter, most of the time you need to
wrap a widget in an `Opacity` widget to accomplish this.

在 iOS 裡，檢視都有一個 `.opacity` 或者 `.alpha` 屬性。
而在 Flutter 裡，大部分時候你都需要封裝 widget 到
`Opacity` widget 中來實現這一功能。

### How do I build custom widgets?

### 如何建構自訂 widget？

In iOS, you typically subclass `UIView`, or use a pre-existing view,
to override and implement methods that achieve the desired behavior.
In Flutter, build a custom widget by [composing][] smaller widgets
(instead of extending them).

在 iOS 裡，你可以直接繼承 `UIView` 或者使用已經存在的檢視，
然後重寫並實現對應的方法來達到想要的效果。
在 Flutter 裡，建構自訂 widget 需要透過 [組合][composing]
一些小的 widget（而不是對它們進行擴充）來實現。

For example, how do you build a `CustomButton`
that takes a label in the constructor?
Create a CustomButton that composes a `ElevatedButton` with a label,
rather than by extending `ElevatedButton`:

例如，應該如何建構一個初始方法中就包含文字標籤的 `CustomButton`？
需要建立一個合成一個 `RaisedButton` 和一個文字標籤的 CustomButton，
而不是繼承 `RaisedButton`：

<?code-excerpt "lib/custom.dart (CustomButton)"?>
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

Then use `CustomButton`,
just as you'd use any other Flutter widget:

與其他 Flutter widget 一樣的用法，
下面我們使用 `CustomButton`：

<?code-excerpt "lib/custom.dart (UseCustomButton)"?>
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

### 如何在兩個頁面之間切換？

In iOS, to travel between view controllers, you can use a
`UINavigationController` that manages the stack of view controllers
to display.

在 iOS 裡，想要在多個 View Controller 中切換，
可以使用 `UINavigationController`
管理 View Controller 構成的棧進行顯示。

Flutter has a similar implementation, using a `Navigator` and
`Routes`. A `Route` is an abstraction for a “screen” or “page” of an app, and
a `Navigator` is a [widget][]
that manages routes. A route roughly maps to a
`UIViewController`. The navigator works in a similar way to the iOS
`UINavigationController`, in that it can `push()` and `pop()`
routes depending on whether you want to navigate to, or back from, a view.

Flutter 中也有類似的實現，使用 `Navigator` 和 `Route`。
一個 `Route` 是應用中螢幕或者頁面的抽象概念，
而一個 `Navigator` 是管多個 `Route` 的 [widget][]。
也可以理解把 `Route` 理解為 `UIViewController`。
而 `Navigator` 的工作方式和 iOS 裡的 `UINavigationController` 類似，
當你想要進入或退出一個新頁面的時候，它也可以進行 `push()` 和 `pop()` 操作。

To navigate between pages, you have a couple options:

想要在不同頁面間跳轉，你有兩個選擇：

* Specify a `Map` of route names.
  
  建構由 route 名稱組成的 `Map`；
  
* Directly navigate to a route.
  
  直接跳轉到一個路由。

The following example builds a `Map`.

下面的範例建構了一個 `Map`：

<?code-excerpt "lib/intent.dart (Map)"?>
```dart
void main() {
  runApp(
    CupertinoApp(
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

Navigate to a route by `push`ing its name to the `Navigator`.

透過把 route 名稱傳遞給 `Navigator` 來實現 `push` 效果。

<?code-excerpt "lib/intent.dart (Push)"?>
```dart
Navigator.of(context).pushNamed('/b');
```

The `Navigator` class handles routing in Flutter and is used to get
a result back from a route that you have pushed on the stack.
This is done by `await`ing on the `Future` returned by `push()`.

`Navigator` 類對 Flutter 中的路由事件做處理，還可以用來獲取入棧之後的路由的結果。
這需要透過 `push()` 返回的 `Future` 中 的 `await` 來實現。

For example, to start a ‘location’ route that lets the user select their
location, you might do the following:

例如，要開啟一個「定位」頁面來讓使用者選擇他們的位置，你需要做如下事情：

<?code-excerpt "lib/intent.dart (PushAwait)"?>
```dart
Object? coordinates = await Navigator.of(context).pushNamed('/location');
```

And then, inside your ‘location’ route, once the user has selected their
location, `pop()` the stack with the result:

然後，在「定位」頁面中，一旦使用者選擇了自己的定位，就 `pop()` 出棧並返回結果。

<?code-excerpt "lib/intent.dart (Pop)"?>
```dart
Navigator.of(context).pop({'lat': 43.821757, 'long': -79.226392});
```

### How do I navigate to another app?

### 如何跳轉到其他應用？

In iOS, to send the user to another application,
you use a specific URL scheme.
For the system level apps, the scheme depends on the app.
To implement this functionality in Flutter,
create a native platform integration, or use an
[existing plugin][], such as [`url_launcher`][].

在 iOS 裡，想要跳轉到其他應用，可以使用特定的 URL scheme。
對於系統級別的應用，scheme 都是取決於應用的。
在 Flutter 裡想要實現這個功能，需要建立原生平台的整合層，
或者使用已經存在的 [外掛][existing plugin]，例如
[`url_launcher`][]。

### How do I pop back to the iOS native ViewController?

### 如何退回到 iOS 原生的 ViewController？

Calling `SystemNavigator.pop()` from your Dart code
invokes the following iOS code:

在 Dart 程式碼中呼叫 `SystemNavigator.pop()` 將會呼叫下面的 iOS 程式碼：

```objc
UIViewController* viewController = [UIApplication sharedApplication].keyWindow.rootViewController;
if ([viewController isKindOfClass:[UINavigationController class]]) {
  [((UINavigationController*)viewController) popViewControllerAnimated:NO];
}
```

If that doesn't do what you want, you can create your own
[platform channel][] to invoke arbitrary iOS code.

如果這不是你需要的功能，你可以建立你自己的
[平台通道][platform channel] 來呼叫對應的 iOS 程式碼。

## Threading & asynchronicity

## 執行緒和非同步

### How do I write asynchronous code?

### 如何編寫非同步程式碼？

Dart has a single-threaded execution model,
with support for `Isolate`s
(a way to run Dart codes on another thread),
an event loop, and asynchronous programming.
Unless you spawn an `Isolate`,
your Dart code runs in the main UI thread and is
driven by an event loop. Flutter’s event loop is
equivalent to the iOS main loop&mdash;that is,
the `Looper` that is attached to the main thread.

Dart 是單執行緒執行模型，支援 `Isolate`
（一種在其他執行緒執行 Dart 程式碼的方法）、事件迴圈和非同步程式設計。
除非生成了 `Isolate`，否則所有 Dart 程式碼將永遠在主執行緒執行，
並由事件迴圈驅動。
Flutter 中的事件迴圈類似於 iOS 中的 main loop&mdash;&mdash;
也就是主執行緒上的 `Looper`。

Dart's single-threaded model doesn't mean you are
required to run everything as a blocking operation
that causes the UI to freeze. Instead,
use the asynchronous facilities that the Dart language provides,
such as `async`/`await`, to perform asynchronous work.

Dart 的單執行緒模型並不意味著你需要以阻塞 UI 的形式來執行程式碼，
相反，你更應該使用 Dart 語言提供的非同步功能，
比如使用 `async`/`await` 來實現非同步操作。

For example, you can run network code without causing the UI to hang by using
`async`/`await` and letting Dart do the heavy lifting:

例如，你可以使用 `async`/`await` 來執行網路程式碼以避免 UI 掛起，
讓 Dart 來完成這個繁重的任務：

<?code-excerpt "lib/async.dart (loadData)"?>
```dart
Future<void> loadData() async {
  final Uri dataURL = Uri.parse('https://jsonplaceholder.typicode.com/posts');
  final http.Response response = await http.get(dataURL);
  setState(() {
    data = jsonDecode(response.body);
  });
}
```

Once the `await`ed network call is done,
update the UI by calling `setState()`,
which triggers a rebuild of the widget sub-tree
and updates the data.

一旦 `await` 等待的網路操作結束，透過呼叫 `setState()` 來更新 UI，
這將會觸發 widget 子樹的重新建構並更新資料。

The following example loads data asynchronously and displays it in a `ListView`:

下面的範例展示瞭如何非同步載入資料，並在 `ListView` 中展示出來：

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
    final Uri dataURL = Uri.parse('https://jsonplaceholder.typicode.com/posts');
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
      appBar: AppBar(
        title: const Text('Sample App'),
      ),
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

Refer to the next section for more information on doing work
in the background, and how Flutter differs from iOS.

更多關於在後台執行任務的資訊，以及 Flutter 和 iOS 的區別，可以參考下一章節。

### How do you move work to a background thread?

### 如何讓你的任務在後台執行緒執行？

Since Flutter is single threaded and runs an event loop (like Node.js), you
don't have to worry about thread management or spawning background threads. If
you're doing I/O-bound work, such as disk access or a network call, then
you can safely use `async`/`await` and you're done. If, on the other
hand, you need to do computationally intensive work that keeps the CPU busy,
you want to move it to an `Isolate` to avoid blocking the event loop.

由於 Flutter 是單執行緒模型，而且執行著一個 event loop（就像 Node.js），
你不需要為執行緒管理或是開啟後臺執行緒操心。
如果你在處理 I/O 操作，例如磁碟存取或網路請求，
那麼你安全地使用 `async`/`await` 就可以了。
但是，如果你需要大量的計算來讓 CPU 保持忙碌狀態，
你需要使用 `Isolate` 來防止阻塞事件迴圈。

For I/O-bound work, declare the function as an `async` function,
and `await` on long-running tasks inside the function:

對於 I/O 操作，把方法宣告為 `async` 方法，然後透過 `await` 來等待非同步方法的執行完成：

<?code-excerpt "lib/async.dart (loadData)"?>
```dart
Future<void> loadData() async {
  final Uri dataURL = Uri.parse('https://jsonplaceholder.typicode.com/posts');
  final http.Response response = await http.get(dataURL);
  setState(() {
    data = jsonDecode(response.body);
  });
}
```

This is how you typically do network or database calls,
which are both I/O operations.

這就是處理網路或資料庫請求等 I/O 操作的經典做法。

However, there are times when you might be processing a large amount of data and
your UI hangs. In Flutter, use `Isolate`s to take advantage of
multiple CPU cores to do long-running or computationally intensive tasks.

然而，有時候你需要處理大量的資料，從而導致 UI 掛起。
在 Flutter 裡，當處理長期執行或者運算密集的任務時，
可以使用 `Isolate` 來發揮出多核 CPU 的優勢。

Isolates are separate execution threads that do not share
any memory with the main execution memory heap.
This means you can’t access variables from the main thread,
or update your UI by calling `setState()`.
Isolates are true to their name, and cannot share memory
(in the form of static fields, for example).

Isolates 是相互隔離的執行執行緒，並不和主執行緒共享記憶體。
這意味著你不能夠存取主執行緒的變數，也不能使用 `setState()` 來更新 UI。
Isolates 正如其字面意思，是不能共享記憶體（例如靜態變量表）的。

The following example shows, in a simple isolate,
how to share data back to the main thread to update the UI.

下面的例子展示了在一個簡單的 isolate 中，如何把資料推到主執行緒上用來更新 UI。

<?code-excerpt "lib/isolates.dart (loadData)"?>
```dart
Future<void> loadData() async {
  final ReceivePort receivePort = ReceivePort();
  await Isolate.spawn(dataLoader, receivePort.sendPort);

  // The 'echo' isolate sends its SendPort as the first message.
  final SendPort sendPort = await receivePort.first as SendPort;

  final List<Map<String, dynamic>> msg = await sendReceive(
    sendPort,
    'https://jsonplaceholder.typicode.com/posts',
  );

  setState(() {
    data = msg;
  });
}

// The entry point for the isolate.
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

在這裡，`dataLoader` 就是執行在獨立執行緒上的 `Isolate`。
在 `Isolate` 中，你可以處理 CPU 密集型任務
（如解析一個龐大的 JSON 檔案），
或者處理複雜的數學運算，比如加密操作或者訊號處理等。

You can run the full example below:

下面是一個完整範例：

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

    // The 'echo' isolate sends its SendPort as the first message.
    final SendPort sendPort = await receivePort.first as SendPort;

    final List<Map<String, dynamic>> msg = await sendReceive(
      sendPort,
      'https://jsonplaceholder.typicode.com/posts',
    );

    setState(() {
      data = msg;
    });
  }

  // The entry point for the isolate.
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
    bool showLoadingDialog = data.isEmpty;

    if (showLoadingDialog) {
      return getProgressDialog();
    } else {
      return getListView();
    }
  }

  Widget getProgressDialog() {
    return const Center(child: CircularProgressIndicator());
  }

  ListView getListView() {
    return ListView.builder(
      itemCount: data.length,
      itemBuilder: (context, position) {
        return getRow(position);
      },
    );
  }

  Widget getRow(int i) {
    return Padding(
      padding: const EdgeInsets.all(10.0),
      child: Text("Row ${data[i]["title"]}"),
    );
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
}
```

### How do I make network requests?

### 如何發起網路請求？

Making a network call in Flutter is easy when you use the popular
[`http` package][]. This abstracts
away a lot of the networking that you might normally implement yourself,
making it simple to make network calls.

在 Flutter 裡，想要構造網路請求十分簡單，直接使用
[`http` 庫][`http` package] 即可。
它把你可能要實現的網路操作進行了抽象封裝，讓處理網路請求變得十分簡單。

To use the `http` package, add it to your dependencies in `pubspec.yaml`:

要使用 `http` 庫，需要在 `pubspec.yaml` 中把它新增為依賴：

```yaml
dependencies:
  http: ^0.13.4
```

To make a network call,
call `await` on the `async` function `http.get()`:

構造網路請求，需要在 `async` 方法 `http.get()` 中呼叫 `await`：

<?code-excerpt "lib/progress.dart (loadData)"?>
```dart
Future<void> loadData() async {
  final Uri dataURL = Uri.parse('https://jsonplaceholder.typicode.com/posts');
  final http.Response response = await http.get(dataURL);
  setState(() {
    data = jsonDecode(response.body);
  });
}
```

### How do I show the progress of a long-running task?

### 展示耗時任務的進度

In iOS, you typically use a `UIProgressView`
while executing a long-running task in the background.

在 iOS 裡，在後台執行耗時任務時，會使用 `UIProgressView`。

In Flutter, use a `ProgressIndicator` widget.
Show the progress programmatically by controlling
when it's rendered through a boolean flag.
Tell Flutter to update its state before your long-running task starts,
and hide it after it ends.

在 Flutter 裡，應該使用 `ProgressIndicator`。
它在渲染時透過一個 boolean flag 來控制是否顯示
進度。在耗時任務開始前，告訴 Flutter 去更新狀態，
並在任務結束後隱藏。

In the example below, the build function is separated into three different
functions. If `showLoadingDialog` is `true`
(when `widgets.length == 0`), then render the `ProgressIndicator`.
Otherwise, render the `ListView` with the data returned from a network call.

在下面的例子中，`build` 函式被分為三個不同的函式。
當 `showLoadingDialog()` 是 `true` 時
（當 `widgets.length == 0`），渲染 `ProgressIndicator`。
否則，使用網路請求返回的資料渲染 `ListView`。

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
    final Uri dataURL = Uri.parse('https://jsonplaceholder.typicode.com/posts');
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

  Widget getRow(int i) {
    return Padding(
      padding: const EdgeInsets.all(10.0),
      child: Text("Row ${data[i]["title"]}"),
    );
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
}
```

## Project structure, localization, dependencies and assets

## 工程結構，本地化，依賴和資源

### How do I include image assets for Flutter? What about multiple resolutions?

### 如何在 Flutter 中引入圖片資源？如何處理多解析度？

While iOS treats images and assets as distinct items,
Flutter apps have only assets. Resources that are
placed in the `Images.xcasset` folder on iOS,
are placed in an assets' folder for Flutter.
As with iOS, assets are any type of file, not just images.
For example, you might have a JSON file located in the `my-assets` folder:

在 iOS 裡，圖片和其他資源會被視為不同的資源分別處理，
而在 Flutter 中只有資源這一個概念。
iOS 裡被放置在 `Images.xcasset` 資料夾的資源，
在 Flutter 中都被放置到了 assets 資料夾中。
和 iOS 一樣，assets 中可以放置任意類別型的檔案，而不僅僅是圖片。
例如，你可以把一個 JSON 檔案放置到 `my-assets` 資料夾中。

```
my-assets/data.json
```

Declare the asset in the `pubspec.yaml` file:

在 `pubspec.yaml` 中宣告 assets：

```yaml
assets:
 - my-assets/data.json
```

And then access it from code using an [`AssetBundle`][]:

然後在程式碼中透過 [`AssetBundle`][] 存取資源:

<?code-excerpt "lib/asset_bundle.dart"?>
```dart
import 'dart:async' show Future;
import 'package:flutter/services.dart' show rootBundle;

Future<String> loadAsset() async {
  return await rootBundle.loadString('my-assets/data.json');
}
```

For images, Flutter follows a simple density-based format like iOS.
Image assets might be `1.0x`, `2.0x`, `3.0x`, or any other multiplier.
The so-called [`devicePixelRatio`][]
expresses the ratio of physical pixels in a single logical pixel.

對於圖片，Flutter 和 iOS 一樣遵循了一個簡單的基於螢幕密度的格式。
Image assets 可能是 `1.0x`，`2.0x`，`3.0x` 或者其他任意的倍數。
而 [`devicePixelRatio`][] 則表達了物理解析度到邏輯解析度的對照比例。

Assets are located in any arbitrary folder&mdash;
Flutter has no predefined folder structure.
You declare the assets (with location) in
the `pubspec.yaml` file, and Flutter picks them up.

Assets 可以放在任何屬性的資料夾中&mdash;Flutter 沒有任何預置的檔案結構。
你需要在 `pubspec.yaml` 中宣告 assets （包括路徑），然後 Flutter 將會識別它們。

For example, to add an image called `my_icon.png` to your Flutter
project, you might decide to store it in a folder arbitrarily called `images`.
Place the base image (1.0x) in the `images` folder, and the
other variants in sub-folders named after the appropriate ratio multiplier:

例如，要新增一個名為 `my_icon.png` 的圖片到你的 Flutter 工程中，
你可以把它儲存在 `images` 資料夾下。
把基礎的圖片（一倍圖）放到 `images` 資料夾下，
然後把其他倍數的圖片放置到對應的比例下的子資料夾中。

```
images/my_icon.png       // Base: 1.0x image
images/2.0x/my_icon.png  // 2.0x image
images/3.0x/my_icon.png  // 3.0x image
```

Next, declare these images in the `pubspec.yaml` file:

接著，在 `pubspec.yaml` 檔案中宣告這些圖片：

```yaml
assets:
 - images/my_icon.png
```

You can now access your images using `AssetImage`:

現在你可以使用 `AssetImage` 存取你的圖片了：

<?code-excerpt "lib/images.dart (AssetImage)"?>
```dart
AssetImage('images/a_dot_burr.jpeg')
```

or directly in an `Image` widget:

或者直接在 `Image` widget 進行使用：

<?code-excerpt "lib/images.dart (Imageasset)"?>
```dart
@override
Widget build(BuildContext context) {
  return Image.asset('images/my_image.png');
}
```

For more details, see
[Adding Assets and Images in Flutter][].

關於更多的細節，請參見文件
[在 Flutter 中新增資源和圖片][Adding Assets and Images in Flutter]。

### Where do I store strings? How do I handle localization?

### 字串儲存在哪裡？如何處理本地化？

Unlike iOS, which has the `Localizable.strings` file,
Flutter doesn't currently have a dedicated system for handling strings.
At the moment, the best practice is to declare your copy text
in a class as static fields and access them from there. For example:

iOS 裡有 `Localizable.strings` 檔案，而 Flutter 則不同，
目前並沒有關於字串的處理系統。
目前，最佳的方案就是在靜態區宣告你的文字，然後進行存取。例如：

<?code-excerpt "lib/string_examples.dart (Strings)"?>
```dart
class Strings {
  static const String welcomeMessage = 'Welcome To Flutter';
}
```

You can access your strings as such:

你可以這樣存取字串：

<?code-excerpt "lib/string_examples.dart (AccessString)" replace="/const //g"?>
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

<?code-excerpt "lib/localizations_example.dart"?>
```dart
import 'package:flutter/material.dart';
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

要使用本地化資源，使用 `Localizations.of()`
方法可以存取提供代理的特定本地化類別。
使用 [`intl_translation`][] 庫解壓翻譯的副本到 [arb][] 檔案，
然後在應用中透過 `intl` 來參考它們。

For further details on internationalization and localization in Flutter,
see the [internationalization guide][], which has sample code
with and without the `intl` package.

關於 Flutter 中國際化和本地化的細節內容，請參看
[Flutter 應用裡的國際化][internationalization guide]，
裡面包含有使用和不使用 `intl` 庫的範例程式碼。

### What is the equivalent of CocoaPods? How do I add dependencies?

### CocoaPods 相當於 Flutter 中的什麼？如何新增依賴？

In iOS, you add dependencies by adding to your `Podfile`. Flutter uses Dart’s
build system and the Pub package manager to handle dependencies. The tools
delegate the building of the native Android and iOS wrapper apps to the
respective build systems.

在 iOS 裡，可以透過 `Podfile` 新增依賴。
而 Flutter 使用 Dart 建構系統和 Pub 包管理器來處理依賴。
這些工具將原生應用的打包任務分發給相應 Android 或 iOS 建構系統。

While there is a Podfile in the iOS folder in your
Flutter project, only use this if you are adding native
dependencies needed for per-platform integration.
In general, use `pubspec.yaml` to declare external dependencies in Flutter.
A good place to find great packages for Flutter is on [pub.dev][].

如果你的 Flutter 專案 iOS 資料夾中存在 Podfile，
那麼請僅在裡面新增原生平台的依賴。總而言之，
在 Flutter 中使用 `pubspec.yaml` 來宣告外部依賴。
你可以透過 [pub.dev][] 來查詢一些優秀的 Flutter 第三方套件。

## ViewControllers

### What is the equivalent to ViewController in Flutter?

### ViewControllers 相當於 Flutter 中的什麼？

In iOS, a `ViewController` represents a portion of user interface,
most commonly used for a screen or section.
These are composed together to build complex user interfaces,
and help scale your application's UI.
In Flutter, this job falls to Widgets.
As mentioned in the Navigation section,
screens in Flutter are represented by Widgets since
"everything is a widget!"
Use a `Navigator` to move between different `Route`s
that represent different screens or pages,
or maybe different states or renderings of the same data.

在 iOS 裡，一個 `ViewController` 是使用者介面的一部分，
通常是作為螢幕或者其中的一部分來使用。
這些組合在一起構成了複雜的使用者介面，並以此對應用的 UI 做不斷的擴充。
在 Flutter 中，這一任務又落到了 Widget 這裡。
就像在導航一章中提到的，Flutter 中的螢幕也是使用 Widgets 表示的，
因為「萬物皆 widget」！。
使用 `Navigator` 在不同的 `Route` 之間切換，
而不同的路由則代表了不同的螢幕或頁面，
或是不同的狀態，也可能是渲染相同的資料。

### How do I listen to iOS lifecycle events?

### 如何監聽 iOS 中的生命週期？

In iOS, you can override methods to the `ViewController`
to capture lifecycle methods for the view itself,
or register lifecycle callbacks in the `AppDelegate`.
In Flutter, you have neither concept, but you can instead
listen to lifecycle events by hooking into
the `WidgetsBinding` observer and listening to
the `didChangeAppLifecycleState()` change event.

在 iOS 裡，可以重寫 `ViewController` 的方法來捕獲自身的生命週期，
或者在 `AppDelegate` 中註冊生命週期的回呼(Callback)。
Flutter 中則沒有這兩個概念，但是你可以透過在
`WidgetsBinding` 的監聽器 (observer) 中新增監聽，
也可以透過監聽 `didChangeAppLifecycleState()` 事件，來實現相應的功能。

The observable lifecycle events are:

可監聽的生命週期事件有：

**`inactive`** 
<br> The application is in an inactive state and is not receiving
user input. This event only works on iOS, as there is no equivalent event on
Android.
  
**`inactive`** 
<br> 應用當前處於不活躍狀態，不接收使用者輸入事件。
  這個事件只在 iOS 上有效，Android 中沒有類似的狀態。
  
**`paused`** 
<br> The application is not currently visible to
the user, is not responding to user input, but is running in the background.

**`paused`** 
<br> 應用當前處於使用者不可見狀態，不接收使用者輸入事件，但仍在後台執行。
  
**`resumed`** 
<br> The application is visible and responding to user input.
  
**`resumed`** 
<br> 應用可見，也響應使用者輸入。
  
**`suspending`** 
<br> The application is suspended momentarily. The iOS platform
has no equivalent event.

**`suspending`**
<br> 應用被掛起，在 iOS 平台沒有這一事件。

For more details on the meaning of these states, see
[`AppLifecycleState` documentation][].

關於這些狀態含義的更多細節，請參看 [AppLifecycleStatus 文件][`AppLifecycleState` documentation]。

## Layouts

## 佈局

### What is the equivalent of UITableView or UICollectionView in Flutter?

### `UITableView` 和 `UICollectionView` 相當於 Flutter 中的什麼？

In iOS, you might show a list in
either a `UITableView` or a `UICollectionView`.
In Flutter, you have a similar implementation using a `ListView`.
In iOS, these views have delegate methods
for deciding the number of rows,
the cell for each index path, and the size of the cells.

在 iOS 裡，你可能使用 `UITableView`
或者 `UICollectionView` 來展示一個列表。
而在 Flutter 裡，你可以使用 `ListView` 來達到類似的實現。
在 iOS 中，你透過 delegate 方法來確定顯示的行數、相應位置的 cell 以及 cell 的尺寸。

Due to Flutter’s immutable widget pattern,
you pass a list of widgets to your `ListView`,
and Flutter takes care of making sure that
scrolling is fast and smooth.

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
  const SampleApp({super.key});

  // This widget is the root of your application.
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
    final List<Widget> widgets = [];
    for (int i = 0; i < 100; i++) {
      widgets.add(Padding(
        padding: const EdgeInsets.all(10.0),
        child: Text('Row $i'),
      ));
    }
    return widgets;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sample App'),
      ),
      body: ListView(children: _getListData()),
    );
  }
}
```

### How do I know which list item is clicked?

### 如何確定列表中被點選的元素？

In iOS, you implement the delegate method, `tableView:didSelectRowAtIndexPath:`.
In Flutter, use the touch handling provided by the passed-in widgets.

在 iOS 裡，可以透過 `tableView:didSelectRowAtIndexPath:` 代理方法來實現。
而在 Flutter 裡，需要透過 widget 傳遞進來的 touch 響應處理來實現。

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
    List<Widget> widgets = [];
    for (int i = 0; i < 100; i++) {
      widgets.add(
        GestureDetector(
          onTap: () {
            developer.log('row tapped');
          },
          child: Padding(
            padding: const EdgeInsets.all(10.0),
            child: Text('Row $i'),
          ),
        ),
      );
    }
    return widgets;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sample App'),
      ),
      body: ListView(children: _getListData()),
    );
  }
}
```

### How do I dynamically update a ListView?

### 如何動態更新 `ListView`？

In iOS, you update the data for the list view, and notify the table or
collection view using the `reloadData` method.

在 iOS 裡，可以更新列表的資料，然後透過呼叫 `reloadData` 方法
來通知 tableView 或者 collectionView。

In Flutter, if you update the list of widgets inside a `setState()`,
you quickly see that your data doesn't change visually.
This is because when `setState()` is called,
the Flutter rendering engine looks at the widget tree
to see if anything has changed.
When it gets to your `ListView`, it performs an `==` check,
and determines that the two `ListView`s are the same.
Nothing has changed, so no update is required.

在 Flutter 裡，如果你在 `setState()` 中更新了 widget 列表，
你會發現展示的資料並不會立刻更新。
這是因為當 `setState()` 被呼叫時，Flutter 的渲染引擎回去檢索 widget 樹是否有改變。
當它獲取到 `ListView`，會進行 `==` 判斷，
然後發現兩個 `ListView` 是相等的。
此時沒有改變，也就不會進行更新。

For a simple way to update your `ListView`,
create a new `List` inside of `setState()`,
and copy the data from the old list to the new list.
While this approach is simple,
it is not recommended for large data sets,
as shown in the next example.

一個更新 `ListView` 的簡單方法就是，
在 `setState()` 建立一個新的 `List`，
然後複製舊列表中的所有資料到新列表。
這樣雖然簡單，但是像下面範例一樣資料量很大時，並不推薦這樣做。

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
        padding: const EdgeInsets.all(10.0),
        child: Text('Row $i'),
      ),
    );
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
}
```

The recommended, efficient,
and effective way to build a list uses a `ListView.Builder`.
This method is great when you have a dynamic
list or a list with very large amounts of data.

一個推薦的、高效且有效的方法就是使用 `ListView.Builder` 來建構列表。
當你的資料量很大，且需要建構動態列表時，這個方法會非常好用。

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

  Widget getRow(int i) {
    return GestureDetector(
      onTap: () {
        setState(() {
          widgets.add(getRow(widgets.length));
          developer.log('row $i');
        });
      },
      child: Padding(
        padding: const EdgeInsets.all(10.0),
        child: Text('Row $i'),
      ),
    );
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
}
```

Instead of creating a `ListView`, create a `ListView.builder`
that takes two key parameters: the initial length of the list,
and an `ItemBuilder` function.

和建立 `ListView` 不同，建立 `ListView.Builder` 需要兩個關鍵引數：
初始化列表長度和 `ItemBuilder` 函式。

The `ItemBuilder` function is similar to the `cellForItemAt` delegate method
in an iOS table or collection view, as it takes a position, and returns the
cell you want rendered at that position.

`ItemBuilder` 函式和 iOS 裡 tableView 和 collectionView 的
`cellForItemAt` 方法類似，它接收位置引數，然後返回想要在該位置渲染的 cell。

Finally, but most importantly, notice that the `onTap()` function
doesn't recreate the list anymore, but instead `.add`s to it.

最後，也是最重要的，注意 `onTap()` 方法並沒有重新建立列表，
而是使用 `.add` 方法進行新增。

### What is the equivalent of a ScrollView in Flutter?

### `ScrollView` 相當於 Flutter 中的什麼？

In iOS, you wrap your views in a `ScrollView` that allows a user to scroll
your content if needed.

在 iOS 裡，把檢視放在 `ScrollView` 裡來允許使用者在需要時滾動內容。

In Flutter the easiest way to do this is using the `ListView` widget. This
acts as both a `ScrollView` and an iOS `TableView`, as you can layout widgets
in a vertical format.

在 Flutter 中，最簡單的辦法就是使用 `ListView` widget。
它和 iOS 中的 `ScrollView` 以及 `TableView` 表現一致，
也可以給它的子 widget 做垂直排版。

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

For more detailed docs on how to lay out widgets in Flutter,
see the [layout tutorial][].

關於 Flutter 中佈局的更多細節，請參看 [佈局課程][layout tutorial]。

## Gesture detection and touch event handling

## 手勢檢測與 touch 事件處理

### How do I add a click listener to a widget in Flutter?

### 如何給 Flutter 的 widget 新增點選事件？

In iOS, you attach a `GestureRecognizer` to a view to handle
click events. In Flutter, there are two ways of adding touch listeners:

在 iOS 裡，透過把 `GestureRecognizer` 繫結給 UIView 來處理點選事件。
在 Flutter 中，有兩種方法來新增事件監聽：

1. If the widget supports event detection, pass a function to it,
   and handle the event in the function. For example, the
   `ElevatedButton` widget has an `onPressed` parameter:

   如果 widget 本身支援事件檢測，則直接傳遞處理函式給它。
   例如，`ElevatedButton` 擁有一個 `onPressed` 引數：

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

2. If the Widget doesn't support event detection,
   wrap the widget in a GestureDetector and pass a function
   to the `onTap` parameter.

   如果 widget 本身不支援事件檢測，那麼把它封裝到一個 GestureDetector 中，
   並給它的 `onTap` 引數傳遞一個函式：

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
              size: 200.0,
            ),
          ),
        ),
      );
    }
  }
   ```

### How do I handle other gestures on widgets?

### 如何處理 widget 的其他手勢？

Using `GestureDetector` you can listen to a wide range of gestures such as:

你可以使用 `GestureDetector` 來監聽更多的手勢，例如：

* **Tapping**

  **單擊事件**

  **`onTapDown`**
  <br> A pointer that might cause a tap has contacted the screen at a
    particular location.

  **`onTapDown`**
  <br> 使用者在特定區域發生點觸螢幕的一個即時操作。

  **`onTapUp`**
  <br> A pointer that triggers a tap has stopped contacting the
    screen at a particular location.

  **`onTapUp`**
  <br> 使用者在特定區域發生觸控抬起的一個即時操作。

  **`onTap`**
  <br> A tap has occurred.

  **`onTap`**
  <br> 從點觸螢幕之後到觸控抬起之間的單擊操作。

  **`onTapCancel`**
  <br> The pointer that previously triggered the `onTapDown` won't
    cause a tap.

  **`onTapCancel`**
  <br>使用者在之前觸發了 `onTapDown` 時間，但未觸發 tap 事件。

* **Double tapping**

  雙擊事件

  **`onDoubleTap`**
  <br> The user tapped the screen at the same location twice in
    quick succession.

  **`onDoubleTap`**
  <br>使用者在同一位置發生快速點選螢幕兩次的操作。

* **Long pressing**

  長按事件

  **`onLongPress`**
  <br> A pointer has remained in contact with the screen at the same
    location for a long period of time.

  **`onLongPress`**
  <br> 使用者在同一位置長時間觸控式螢幕幕的操作。

* **Vertical dragging**

  垂直拖動事件

  **`onVerticalDragStart`**
  <br> A pointer has contacted the screen and might begin to
    move vertically.

  **`onVerticalDragStart`**
  <br> 使用者手指接觸螢幕，並且將要進行垂直移動事件。

  **`onVerticalDragUpdate`**
  <br> A pointer in contact with the screen
    has moved further in the vertical direction.

  **`onVerticalDragUpdate`**
  <br>使用者手指接觸螢幕，已經開始垂直移動，且會持續進行移動。

  **`onVerticalDragEnd`**
  <br> A pointer that was previously in contact with the
    screen and moving vertically is no longer in contact
    with the screen and was moving at a specific velocity
    when it stopped contacting the screen.

  **`onVerticalDragEnd`**
  <br>使用者之前手指接觸了螢幕併發生了垂直移動操作，
  並且停止接觸前還在以一定的速率移動。

* **Horizontal dragging**

  水平拖動事件

  **`onHorizontalDragStart`**
  <br> A pointer has contacted the screen and might begin
    to move horizontally.
    
  **`onHorizontalDragStart`**
  <br>使用者手指接觸螢幕，並且將要進行水平移動事件。
    
  **`onHorizontalDragUpdate`**
  <br> A pointer in contact with the screen
    has moved further in the horizontal direction.
    
  **`onHorizontalDragUpdate`**
  <br>使用者手指接觸螢幕，已經開始水平移動，且會持續進行移動。
    
  **`onHorizontalDragEnd`**
  <br> A pointer that was previously in contact with the
    screen and moving horizontally is no longer in contact with the screen.
    
  **`onHorizontalDragEnd`**
  <br>使用者之前手指接觸了螢幕併發生了水平移動操作，並且停止接觸前還在以一定的速率移動。

下面的範例展示了 `GestureDetector` 是如何實現雙擊時旋轉 Flutter 的 logo 的：

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
              size: 200.0,
            ),
          ),
        ),
      ),
    );
  }
}
```

## Theming and text

## 主題和文字

### How do I theme an app?

### 如何設定應用主題？

Out of the box, Flutter comes with a beautiful implementation
of Material Design, which takes care of a lot of styling and
theming needs that you would typically do.

Flutter 實現了一套漂亮的 Material Design 元件，
而且開箱可用，它提供了許多常用的樣式和主題。

To take full advantage of Material Components in your app,
declare a top-level widget, `MaterialApp`,
as the entry point to your application.
`MaterialApp` is a convenience widget that wraps a number
of widgets that are commonly required for applications
implementing Material Design.
It builds upon a `WidgetsApp` by adding Material specific functionality.

為了充分發揮應用中 Material Components 的優勢，
Flutter 提供了一個最上層的 `MaterialApp` widget 來作為你的應用入口。
`MaterialApp` 是一個封裝了大量常用 Material Design 元件的 widget。
它基於 `WidgetsApp` 添加了 Material 的相關功能。

But Flutter is flexible and expressive enough to implement
any design language. On iOS, you can use the
[Cupertino library][] to produce an interface that adheres to the
[Human Interface Guidelines][].
For the full set of these widgets,
see the [Cupertino widgets][] gallery.

但是 Flutter 有足夠的靈活性和表現力來實現任何設計語言。
在 iOS 上，可以使用 [Cupertino 庫][Cupertino library] 來
製作遵循 [人機介面指南][Human Interface Guidelines] 的介面。
關於這些 widget 的全部集合，
可以參看 [Cupertino widgets][] 合集。

You can also use a `WidgetsApp` as your app widget,
which provides some of the same functionality,
but is not as rich as `MaterialApp`.

你也可以使用 `WidgetApp` 來做為應用入口，
它提供了一部分類似的功能介面，但是不如 `MaterialApp` 強大。

To customize the colors and styles of any child components,
pass a `ThemeData` object to the `MaterialApp` widget.
For example, in the code below,
the primary swatch is set to blue and divider color is grey.

定義所有子元件顏色和樣式，可以直接傳遞
`ThemeData` 物件給 `MaterialApp` widget。
例如在下面的程式碼中，primary swatch 被設定為藍色，
而分割線被被設定為灰色。

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
        primarySwatch: Colors.blue,
        dividerColor: Colors.grey,
      ),
      home: const SampleAppPage(),
    );
  }
}
```

### How do I set custom fonts on my Text widgets?

### 如何給 `Text` widget 設定自訂字型？

In iOS, you import any `ttf` font files into your project
and create a reference in the `info.plist` file.
In Flutter, place the font file in a folder
and reference it in the `pubspec.yaml` file,
similar to how you import images.

在 iOS 裡，可以在專案中引入任何的 `ttf` 字型檔案，
並在 `info.plist` 檔案中宣告並進行參考。
在 Flutter 裡，把字型放到一個資料夾中，
然後在 `pubspec.yaml` 檔案中參考它，就和參考圖片一樣。

```yaml
fonts:
  - family: MyCustomFont
    fonts:
      - asset: fonts/MyCustomFont.ttf
      - style: italic
```

Then assign the font to your `Text` widget:

然後在 `Text` widget 中指定字型：

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

### 如何設定 `Text` widget 的樣式？

Along with fonts, you can customize other styling elements on a `Text` widget.
The style parameter of a `Text` widget takes a `TextStyle` object,
where you can customize many parameters, such as:

除了字型以外，你也可以自訂 `Text` widget 的其他樣式。
`Text` widget 接收一個 `TextStyle` 物件的引數，例如：

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

### How do forms work in Flutter? How do I retrieve user input?

### Flutter 中如何使用表單？如何獲取到使用者的輸入？

Given how Flutter uses immutable widgets with a separate state, you might be
wondering how user input fits into the picture. On iOS, you usually
query the widgets for their current values when it's time to submit the
user input, or action on it. How does that work in Flutter?

我們知道 Flutter 使用的是不可變而且狀態分離的 widget，
你可能會好奇這種情況下如何處理使用者的輸入。
在 iOS 上，一般會在提交資料時查詢當前元件的數值或動作。
那麼在 Flutter 中會怎麼樣呢？

In practice forms are handled, like everything in Flutter, by specialized
widgets. If you have a `TextField` or a `TextFormField`, you can supply a
[`TextEditingController`][]
to retrieve user input:

和 Flutter 的其他部分一樣，表單處理要透過特定的 widget 來實現。
如果你有一個 `TextField` 或者 `TextFormField`，
你可以透過 [`TextEditingController`][] 來
獲取使用者的輸入：

<?code-excerpt "lib/form.dart (MyFormState)"?>
```dart
class _MyFormState extends State<MyForm> {
  // Create a text controller and use it to retrieve the current value.
  // of the TextField!
  final myController = TextEditingController();

  @override
  void dispose() {
    // Clean up the controller when disposing of the Widget.
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
        // text the user has typed into our text field.
        onPressed: () {
          showDialog(
            context: context,
            builder: (context) {
              return AlertDialog(
                // Retrieve the text the user has typed in using our
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

你在 [Flutter Cookbook][] 的
[獲取文字框的輸入值][Retrieve the value of a text field] 
課程中可以找到更多的相關內容以及詳細的程式碼列表。

### What is the equivalent of a placeholder in a text field?

### TextField 中的 placeholder 相當於什麼？

In Flutter, you can easily show a "hint" or a placeholder text
for your field by adding an `InputDecoration` object
to the decoration constructor parameter for the `Text` widget:

在 Flutter 裡，透過向 `Text` widget 傳遞一個 `InputDecoration` 物件，
你可以很簡單地顯示文字框的提示資訊，或是 placeholder。

<?code-excerpt "lib/form.dart (InputHint)" replace="/return const //g;/;//g"?>
```dart
Center(
  child: TextField(
    decoration: InputDecoration(hintText: 'This is a hint'),
  ),
)
```

### How do I show validation errors?

### 如何展示驗證錯誤資訊？

Just as you would with a "hint", pass an `InputDecoration` object
to the decoration constructor for the `Text` widget.

就和顯示提示資訊一樣，你可以透過向 `Text` widget
傳遞一個 `InputDecoration` 來實現。

However, you don't want to start off by showing an error.
Instead, when the user has entered invalid data,
update the state, and pass a new `InputDecoration` object.

然而，你並不想在一開始就顯示錯誤資訊。相反，在使用者輸入非法資料後，
應該更新狀態，並傳遞一個新的 `InputDecoration` 物件。

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

  bool isEmail(String em) {
    String emailRegexp =
        r'^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|'
        r'(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|'
        r'(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$';

    RegExp regExp = RegExp(emailRegexp);

    return regExp.hasMatch(em);
  }

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
            errorText: _errorText,
          ),
        ),
      ),
    );
  }
}
```

## Interacting with hardware, third party services and the platform

## 和硬體、第三方服務以及系統平台互動

### How do I interact with the platform, and with platform native code?

### 如何與系統平台以及平台原生程式碼進行互動？

Flutter doesn't run code directly on the underlying platform; rather, the Dart code
that makes up a Flutter app is run natively on the device, "sidestepping" the SDK
provided by the platform. That means, for example, when you perform a network request
in Dart, it runs directly in the Dart context. You don't use the Android or iOS
APIs you normally take advantage of when writing native apps. Your Flutter
app is still hosted in a native app's `ViewController` as a view, but you don't
have direct access to the `ViewController` itself, or the native framework.

Flutter 並不直接在平臺上執行程式碼，而是以 Dart 程式碼的方式原生運行於裝置之上，
這算是繞過了平台的 SDK 的限制。
這意味著，例如，你用 Dart 發起了一個網路請求，它會直接在 Dart 的上下文中執行。
你不需要呼叫寫 iOS 或者 Android 原生應用時常用的 API 介面。
你的 Flutter 應用仍舊被原生平台的 `ViewController` 當做一個 view 來管理，
但是你不能夠直接存取 `ViewController` 自身或是對應的原生框架。

This doesn't mean Flutter apps cannot interact with those native APIs,
or with any native code you have.
Flutter provides the [platform channel][],
which communicates and exchanges data with the
`ViewController` that hosts your Flutter view.
Platform channels are essentially an asynchronous messaging mechanism
that bridge the Dart code with the host `ViewController`
and the iOS framework it runs on.
You can use platform channels to execute a method on the native side,
or to retrieve some data from the device's sensors, for example.

這並不意味著 Flutter 應用不能夠和原生 API，或是原生程式碼進行互動。
Flutter 提供了用來和宿主 `ViewController`
通訊和交換資料的 [平台通道][platform channel]。
平台通道本質上是一個橋接了 Dart 程式碼與宿主 `ViewController`
和 iOS 框架的非同步通訊模型。
你可以透過平台通道來執行原生程式碼的方法，
或者獲取裝置的感測器資訊等資料。

In addition to directly using platform channels,
you can use a variety of pre-made [plugins][]
that encapsulate the native and Dart code for a specific goal.
For example, you can use a plugin to access the camera roll
and the device camera directly from Flutter,
without having to write your own integration.
Plugins are found on [pub.dev],
Dart and Flutter's open source package repository.
Some packages might support native integrations on iOS,
Android, web, or all of the above.

除了直接使用 platform channels 之外，也可以使用一系列包含了原生程式碼和 Dart程式碼，
實現了特定功能的現有 [外掛][plugins]。
例如，你在 Flutter 中可以直接使用外掛來存取相簿或是裝置攝影頭，而不需要自己重新整合。
[pub.dev][] 是一個 Dart 和 Flutter 的開源包儲存庫，你可以在這裡找到需要的外掛。
有些包可能支援整合 iOS 或 Android，或兩者皆有。

If you can't find a plugin on pub.dev that fits your needs,
you can [write your own][] and [publish it on pub.dev][].

如果你在 Pub 找不到自己需要的套件，你可以自己寫一個，
相關資訊可以查閱 [Flutter Packages 的開發和提交][write your own]，
並且你可以將其 [釋出到 Pub 上][publish it on pub.dev]。

### How do I access the GPS sensor?

### 如何存取 GPS 感測器？

Use the [`geolocator`][] community plugin.

使用 [`geolocator`]({{site.pub-pkg}}/geolocator) 外掛，這一外掛由社群提供。

### How do I access the camera?

### 如何存取相機？

The [`camera`][] plugin is popular for accessing the camera.

[`camera`][] 是常用的存取相機的外掛。

### How do I log in with Facebook?

### 如何使用 Facebook 登入？

To log in with Facebook, use the
[`flutter_facebook_login`][] community plugin.

登入 Facebook 可以使用 [`flutter_facebook_login`][] 外掛。

### How do I use Firebase features?

### 如何整合 Firebase 的功能？

Most Firebase functions are covered by [first party plugins][].
These plugins are first-party integrations,
maintained by the Flutter team:

大多數的 Firebase 特性都在
[官方維護的外掛][first party plugins] 中實現了。
這些外掛由 Flutter 官方團隊維護：

 * [`firebase_admob`][] for Firebase AdMob

   搭配 [`firebase_admob`][] 外掛來使用 Firebase AdMob

 * [`firebase_analytics`][] for Firebase Analytics

   搭配 [`firebase_analytics`][] 外掛來使用 Firebase Analytics

 * [`firebase_auth`][] for Firebase Auth
   
   搭配 [`firebase_auth`][] 外掛來使用 Firebase Auth

 * [`firebase_core`][] for Firebase's Core package
   
   搭配 [`firebase_core`][] 外掛來使用 Firebase 核心函式庫

 * [`firebase_database`][] for Firebase RTDB
   
   搭配 [`firebase_database`][] 外掛來使用 Firebase RTDB

 * [`firebase_storage`][] for Firebase Cloud Storage

   搭配 [`firebase_storage`][] 外掛來使用 Firebase Cloud Storage

 * [`firebase_messaging`][] for Firebase Messaging (FCM)

   搭配 [`firebase_messaging`][] 外掛來使用 Firebase Messaging (FCM)

 * [`cloud_firestore`][] for Firebase Cloud Firestore
   
   搭配 [`cloud_firestore`][] 外掛來使用 Firebase Cloud Firestore


You can also find some third-party Firebase plugins on pub.dev that
cover areas not directly covered by the first-party plugins.

在 pub.dev 上你也可以找到一些第三方的 Firebase 外掛，
主要實現了官方外掛沒有直接實現的功能。

### How do I build my own custom native integrations?

### 如何建構自己的外掛？

If there is platform-specific functionality that Flutter or its community
Plugins are missing, you can build your own following the
[developing packages and plugins][] page.

如果有一些 Flutter 和遺漏的平台特性，可以
根據 [developing packages and plugins][] 建構
自己的外掛。

Flutter's plugin architecture, in a nutshell, is much like using an Event bus in
Android: you fire off a message and let the receiver process and emit a result
back to you. In this case, the receiver is code running on the native side
on Android or iOS.

Flutter 的外掛結構，簡單來說，更像是 Android 中的事件匯流排 (EventBus)：
你可以傳送一個訊息，並讓接收者處理並反饋結果給你。
這種情況下，接收者就是在 iOS 或 Android 的原生程式碼。

## Databases and local storage

## 資料庫和本地儲存

### How do I access UserDefault in Flutter?

### Flutter 中如何存取 UserDefaults？

In iOS, you can store a collection of key-value pairs
using a property list, known as the `UserDefaults`.

在 iOS 裡，可以使用屬性列表儲存一個鍵值對的集合，
也就是我們所說的 `UserDefaults`。

In Flutter, access equivalent functionality
using the [Shared Preferences plugin][].
This plugin wraps the functionality of both
`UserDefaults` and the Android equivalent, `SharedPreferences`.

在 Flutter 裡，可以使用
[Shared Preferences 外掛][Shared Preferences plugin]
來實現相同的功能。
這個外掛封裝了 `UserDefaults`
以及 Android 裡類似的 `SharedPreferences`。

### What is the equivalent to CoreData in Flutter?

### CoreData 相當於 Flutter 中的什麼？

In iOS, you can use CoreData to store structured data. This is simply a
layer on top of an SQL database, making it easier to make queries that
relate to your models.

在 iOS 裡，你可以使用 CoreData 來儲存結構化的資料。
這是一個基於 SQL 資料庫的上層封裝，可以使關聯模型的查詢變得更加簡單。

In Flutter, access this functionality using the [SQFlite][] plugin.

在 Flutter 裡，可以使用 [SQFlite][] 外掛來實現這個功能。

## Debugging

### What tools can I use to debug my app in Flutter?

### 應該使用什麼工具除錯我的 Flutter 應用？

Use the [DevTools][] suite for debugging Flutter or Dart apps.

請使用 [開發者工具][DevTools] debug 你的 Flutter 和 Dart 應用。

DevTools includes support for profiling, examining the heap,
inspecting the widget tree, logging diagnostics,
debugging, observing executed lines of code,
debugging memory leaks and memory fragmentation.
For more information, see the [DevTools][] documentation.

開發者工具套件含了 profiling 建構、檢查堆疊、
檢視 widget 樹、診斷資訊記錄、除錯、執行程式碼行觀察、
除錯記憶體洩漏和記憶體碎片等。
有關更多資訊，請參閱 [開發者工具][DevTools] 文件。

## Notifications

## 通知

### How do I set up push notifications?

### 如何設定推送通知？

In iOS, you need to register your app on the developer portal to allow
push notifications.

在 iOS 裡，你需要向開發者中心註冊來允許推送通知。

In Flutter, access this functionality using the
`firebase_messaging` plugin.

在 Flutter 裡，使用 `firebase_messaging` 外掛來實現這個功能。

For more information on using the Firebase Cloud Messaging API, see the
[`firebase_messaging`][]
plugin documentation.

關於 Firebase Cloud Messaging API 的更多資訊，
可以檢視 [`firebase_messaging`][] 外掛文件。

[Add Flutter to existing app]: {{site.url}}/development/add-to-app
[Adding Assets and Images in Flutter]: {{site.url}}/development/ui/assets-and-images
[Animation & Motion widgets]: {{site.url}}/development/ui/widgets/animation
[Animations overview]: {{site.url}}/development/ui/animations
[Animations tutorial]: {{site.url}}/development/ui/animations/tutorial
[Apple's iOS design language]: {{site.apple-dev}}/design/resources
[`AppLifecycleState` documentation]: {{site.api}}/flutter/dart-ui/AppLifecycleState.html
[arb]: {{site.github}}/googlei18n/app-resource-bundle
[`AssetBundle`]: {{site.api}}/flutter/services/AssetBundle-class.html
[`cloud_firestore`]: {{site.pub-pkg}}/cloud_firestore
[composing]: {{site.url}}/resources/architectural-overview#composition
[Cupertino library]: {{site.api}}/flutter/cupertino/cupertino-library.html
[Cupertino widgets]: {{site.url}}/development/ui/widgets/cupertino
[developing packages and plugins]: {{site.url}}/development/packages-and-plugins/developing-packages
[`devicePixelRatio`]: {{site.api}}/flutter/dart-ui/FlutterView/devicePixelRatio.html
[DevTools]: {{site.url}}/development/tools/devtools
[existing plugin]: {{site.pub}}/flutter
[`firebase_admob`]: {{site.pub-pkg}}/firebase_admob
[`firebase_analytics`]: {{site.pub-pkg}}/firebase_analytics
[`firebase_auth`]: {{site.pub-pkg}}/firebase_auth
[`firebase_core`]: {{site.pub-pkg}}/firebase_core
[`firebase_database`]: {{site.pub-pkg}}/firebase_database
[`firebase_messaging`]: {{site.pub-pkg}}/firebase_messaging
[`firebase_storage`]: {{site.pub-pkg}}/firebase_storage
[first party plugins]: {{site.pub}}/flutter/packages?q=firebase
[`flutter_facebook_login`]: {{site.pub-pkg}}/flutter_facebook_login
[Flutter cookbook]: {{site.url}}/cookbook
[Flutter Youtube channel]: {{site.youtube-site}}/flutterdev
[`geolocator`]: {{site.pub-pkg}}/geolocator
[`http` package]: {{site.pub-pkg}}/http
[Human Interface Guidelines]: {{site.apple-dev}}/ios/human-interface-guidelines/overview/themes/
[`camera`]: {{site.pub-pkg}}/camera
[internationalization guide]: {{site.url}}/development/accessibility-and-localization/internationalization
[`intl`]: {{site.pub-pkg}}/intl
[`intl_translation`]: {{site.pub-pkg}}/intl_translation
[Introduction to declarative UI]: {{site.url}}/get-started/flutter-for/declarative
[layout tutorial]: {{site.url}}/development/ui/widgets/layout
[`Localizations`]: {{site.api}}/flutter/widgets/Localizations-class.html
[Material Components]: {{site.material}}/develop/flutter/
[Material Design guidelines]: {{site.material}}/design/
[optimized for all platforms]: {{site.material}}/design/platform-guidance/cross-platform-adaptation.html#cross-platform-guidelines
[Platform adaptations]: {{site.url}}/resources/platform-adaptations
[platform channel]: {{site.url}}/development/platform-integration/platform-channels
[plugins]: {{site.url}}/development/packages-and-plugins/using-packages
[pub.dev]: {{site.pub}}/flutter/packages
[publish it on pub.dev]: {{site.url}}/development/packages-and-plugins/developing-packages#publish
[Retrieve the value of a text field]: {{site.url}}/cookbook/forms/retrieve-input
[Shared Preferences plugin]: {{site.pub-pkg}}/shared_preferences
[SQFlite]: {{site.pub-pkg}}/sqflite
[`TextEditingController`]: {{site.api}}/flutter/widgets/TextEditingController-class.html
[`url_launcher`]: {{site.pub-pkg}}/url_launcher
[widget]: {{site.url}}/resources/architectural-overview#widgets
[widget catalog]: {{site.url}}/development/ui/widgets/layout
[`Window.locale`]: {{site.api}}/flutter/dart-ui/Window/locale.html
[write your own]: {{site.url}}/development/packages-and-plugins/developing-packages
