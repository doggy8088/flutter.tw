---
title: Introduction to widgets
title: Widgets 介紹
description: Learn about Flutter's widgets.
description: 瞭解 Flutter widget 相關的內容。
tags: 使用者介面,Flutter UI,佈局
keywords: Flutter widget,矩形,邊框
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="ui/widgets_intro/"?>

{% assign api = site.api | append: '/flutter' -%}

Flutter widgets are built using a modern framework that takes
inspiration from [React][]. The central idea is that you build
your UI out of widgets. Widgets describe what their view
should look like given their current configuration and state.
When a widget's state changes, the widget rebuilds its description,
which the framework diffs against the previous description in order
to determine the minimal changes needed in the underlying render
tree to transition from one state to the next.

Flutter 從 [React][] 中吸取靈感，透過現代化框架創建出精美的元件。
它的核心思想是用 widget 來建構你的 UI 介面。
Widget 描述了在當前的配置和狀態下檢視所應該呈現的樣子。
當 widget 的狀態改變時，它會重新建構其描述（展示的 UI），
框架則會對比前後變化的不同，
以確定底層渲染樹從一個狀態轉換到下一個狀態所需的最小更改。

{{site.alert.note}}

  If you would like to become better acquainted with Flutter by diving
  into some code, check out [basic layout codelab][],
  [building layouts][],
  and [adding interactivity to your Flutter app][].

  如果你想透過深入瞭解一些程式碼來更好地掌握 Flutter，
  請查閱 [Codelab: Flutter 佈局基礎課程][basic layout codelab]、
  [Flutter 中的佈局][building layouts] 和 
  [為你的 Flutter 應用加入互動體驗][adding interactivity to your Flutter app]
  這三篇文章。

{{site.alert.end}}

## Hello world

The minimal Flutter app simply calls the [`runApp()`][]
function with a widget:

建立一個最小的 Flutter 應用簡單到僅需呼叫 [`runApp()`][] 方法並傳入一個 widget 即可：

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-false:width-100%:height-310px:split-60:ga_id-starting_code
import 'package:flutter/material.dart';

void main() {
  runApp(
    const Center(
      child: Text(
        'Hello, world!',
        textDirection: TextDirection.ltr,
      ),
    ),
  );
}
```

The `runApp()` function takes the given
[`Widget`][] and makes it the root of the widget tree.
In this example, the widget tree consists of two widgets,
the [`Center`][] widget and its child, the [`Text`][] widget.
The framework forces the root widget to cover the screen,
which means the text "Hello, world" ends up centered on screen.
The text direction needs to be specified in this instance;
when the `MaterialApp` widget is used,
this is taken care of for you, as demonstrated later.

`runApp()` 函式會持有傳入的 [`Widget`][]，
並且使它成為 widget 樹中的根節點。
在這個例子中，Widget 樹有兩個 widgets，
[`Center`][] widget 及其子
widget —— [`Text`][] widget。
框架會強制讓根 widget 鋪滿整個螢幕，
也就是說“Hello World”會在螢幕上居中顯示。
在這個例子我們需要指定文字的方向
，當使用 `MaterialApp` widget 時，
你就無需考慮這一點，之後我們會進一步的描述。

When writing an app, you'll commonly author new widgets that
are subclasses of either [`StatelessWidget`][] or [`StatefulWidget`][],
depending on whether your widget manages any state.
A widget's main job is to implement a [`build()`][] function,
which describes the widget in terms of other, lower-level widgets.
The framework builds those widgets in turn until the process
bottoms out in widgets that represent the underlying [`RenderObject`][],
which computes and describes the geometry of the widget.

在寫應用的過程中，取決於是否需要管理狀態，
你通常會建立一個新的元件繼承
[`StatelessWidget`][] 或 [`StatefulWidget`][]。
Widget 的主要工作是實現 [`build()`][] 方法，
該方法根據其它較低級別的 widget 來描述這個 widget。
框架會逐一建構這些 widget，
直到最底層的描述 widget 幾何形狀的
[`RenderObject`][]。

## Basic widgets

## 基礎 widgets

Flutter comes with a suite of powerful basic widgets,
of which the following are commonly used:

Flutter 自帶了一套強大的基礎 widgets，下面列出了一些常用的：

**[`Text`][]**
<br> The `Text` widget lets you create a run of styled text
  within your application.

**[`Text`][]** 
<br>`Text` widget 可以用來在應用內建立帶樣式的文字。

**[`Row`][], [`Column`][]**
<br> These flex widgets let you create flexible layouts in
  both the horizontal (`Row`) and vertical (`Column`) directions.
  The design of these objects is based on the web's
  flexbox layout model.
  
**[`Row`][], [`Column`][]**
<br> 這兩個 flex widgets 可以讓你在水平 (`Row`) 和垂直(`Column`) 
方向建立靈活的佈局。它是基於 web 的 flexbox 佈局模型設計的。

**[`Stack`][]**
<br> Instead of being linearly oriented (either horizontally or vertically),
  a `Stack` widget lets you place widgets on top of each other in paint order.
  You can then use the [`Positioned`][] widget on children of a
  `Stack` to position them relative to the top, right, bottom,
  or left edge of the stack. Stacks are based on the web's
  absolute positioning layout model.
  
**[`Stack`][]**
<br> `Stack` widget 不是線性（水平或垂直）定位的，而是按照繪製順序將 widget 堆疊在一起。
  你可以用 [`Positioned`][] widget 作為`Stack` 的子 widget，
  以相對於 `Stack` 的上，右，下，左來定位它們。
  Stack 是基於 Web 中的絕對位置佈局模型設計的。

**[`Container`][]**
<br> The `Container` widget lets you create a rectangular visual element.
  A container can be decorated with a [`BoxDecoration`][], such as a
  background, a border, or a shadow. A `Container` can also have margins,
  padding, and constraints applied to its size. In addition, a
  `Container` can be transformed in three dimensional space using a matrix.
  
**[`Container`][]**
<br> `Container` widget 可以用來建立一個可見的矩形元素。
   Container 可以使用 [`BoxDecoration`][] 來進行裝飾，如背景，邊框，或陰影等。
   `Container` 還可以設定外邊距、內邊距和尺寸的約束條件等。
   另外，`Container`可以使用矩陣在三維空間進行轉換。

Below are some simple widgets that combine these and other widgets:

下面是一些簡單的 widget，它們結合了上面提到的 widget 和一些其他的 widget：

<?code-excerpt "lib/main_myappbar.dart"?>
```run-dartpad:theme-light:mode-flutter:run-false:width-100%:height-600px:split-60:ga_id-starting_code
import 'package:flutter/material.dart';

class MyAppBar extends StatelessWidget {
  const MyAppBar({required this.title, super.key});

  // Fields in a Widget subclass are always marked "final".

  final Widget title;

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 56.0, // in logical pixels
      padding: const EdgeInsets.symmetric(horizontal: 8.0),
      decoration: BoxDecoration(color: Colors.blue[500]),
      // Row is a horizontal, linear layout.
      child: Row(
        children: [
          const IconButton(
            icon: Icon(Icons.menu),
            tooltip: 'Navigation menu',
            onPressed: null, // null disables the button
          ),
          // Expanded expands its child
          // to fill the available space.
          Expanded(
            child: title,
          ),
          const IconButton(
            icon: Icon(Icons.search),
            tooltip: 'Search',
            onPressed: null,
          ),
        ],
      ),
    );
  }
}

class MyScaffold extends StatelessWidget {
  const MyScaffold({super.key});

  @override
  Widget build(BuildContext context) {
    // Material is a conceptual piece
    // of paper on which the UI appears.
    return Material(
      // Column is a vertical, linear layout.
      child: Column(
        children: [
          MyAppBar(
            title: Text(
              'Example title',
              style: Theme.of(context) //
                  .primaryTextTheme
                  .headline6,
            ),
          ),
          const Expanded(
            child: Center(
              child: Text('Hello, world!'),
            ),
          ),
        ],
      ),
    );
  }
}

void main() {
  runApp(
    const MaterialApp(
      title: 'My app', // used by the OS task switcher
      home: SafeArea(
        child: MyScaffold(),
      ),
    ),
  );
}
```

Be sure to have a `uses-material-design: true` entry in the `flutter`
section of your `pubspec.yaml` file. It allows you to use the predefined
set of [Material icons][]. It's generally a good idea to include this line
if you are using the Materials library.

請確認在 `pubspec.yaml` 檔案中 `flutter` 部分有
`uses-material-design: true` 這條，它能讓你使用預置的 [Material icons][]。

```yaml
name: my_app
flutter:
  uses-material-design: true
```

Many Material Design widgets need to be inside of a [`MaterialApp`][]
to display properly, in order to inherit theme data.
Therefore, run the application with a `MaterialApp`.

為了獲得(`MaterialApp`)主題的資料，
許多 Material Design 的 widget 需要在 [`MaterialApp`][] 中才能顯現正常。
因此，請使用 `MaterialApp` 執行應用。

The `MyAppBar` widget creates a [`Container`][] with a height of 56
device-independent pixels with an internal padding of 8 pixels,
both on the left and the right. Inside the container,
`MyAppBar` uses a [`Row`][] layout to organize its children.
The middle child, the `title` widget, is marked as [`Expanded`][],
which means it expands to fill any remaining available space
that hasn't been consumed by the other children.
You can have multiple `Expanded` children and determine the
ratio in which they consume the available space using the
[`flex`][] argument to `Expanded`.

`MyAppBar` widget 建立了一個高 56 獨立畫素，左右內邊距 8 畫素的 [`Container`][]。
在容器內，`MyAppBar` 以 [`Row`][] 佈局來組織它的子元素。
中間的子 widget（`title` widget），被標記為  [`Expanded`][]，
這意味著它會擴充以填充其它子 widget 未使用的可用空間。
你可以定義多個`Expanded` 子 widget，
並使用 [`flex`][] 引數確定它們佔用可用空間的比例。

The `MyScaffold` widget organizes its children in a vertical column.
At the top of the column it places an instance of `MyAppBar`,
passing the app bar a [`Text`][] widget to use as its title.
Passing widgets as arguments to other widgets is a powerful technique
that lets you create generic widgets that can be reused in a wide
variety of ways. Finally, `MyScaffold` uses an
[`Expanded`][] to fill the remaining space with its body,
which consists of a centered message.

`MyScaffold` widget 將其子 widget 組織在垂直列中。
在列的頂部，它放置一個 `MyAppBar` 例項，
並把 [`Text`][] widget 傳給它來作為應用的標題。
把 widget 作為引數傳遞給其他 widget 是一個很強大的技術，
它可以讓你以各種方式建立一些可重用的通用元件。
最後，MyScaffold 使用 [`Expanded`][]
來填充剩餘空間，其中包含一個居中的訊息。

For more information, see [Layouts][].

有關更多資訊，請參閱 [佈局][Layouts]。

## Using Material Components

## 使用 Material 元件

Flutter provides a number of widgets that help you build apps
that follow Material Design. A Material app starts with the
[`MaterialApp`][] widget, which builds a number of useful widgets
at the root of your app, including a [`Navigator`][],
which manages a stack of widgets identified by strings,
also known as "routes". The `Navigator` lets you transition smoothly
between screens of your application. Using the [`MaterialApp`][]
widget is entirely optional but a good practice.

Flutter 提供了許多 widget，可幫助你建構遵循 Material Design 的應用。
Material 應用以 [`MaterialApp`][] widget 開始，
它在你的應用的底層下建構了許多有用的 widget。
這其中包括 [`Navigator`][]，
它管理由字串標識的 widget 棧，也稱為“routes”。
`Navigator` 可以讓你在應用的頁面中平滑的切換。
使用 [`MaterialApp`][] widget 不是必須的，
但這是一個很好的做法。

<?code-excerpt "lib/main_tutorial.dart"?>
```run-dartpad:theme-light:mode-flutter:run-false:width-100%:height-600px:split-60:ga_id-starting_code
import 'package:flutter/material.dart';

void main() {
  runApp(
    const MaterialApp(
      title: 'Flutter Tutorial',
      home: TutorialHome(),
    ),
  );
}

class TutorialHome extends StatelessWidget {
  const TutorialHome({super.key});

  @override
  Widget build(BuildContext context) {
    // Scaffold is a layout for
    // the major Material Components.
    return Scaffold(
      appBar: AppBar(
        leading: const IconButton(
          icon: Icon(Icons.menu),
          tooltip: 'Navigation menu',
          onPressed: null,
        ),
        title: const Text('Example title'),
        actions: const [
          IconButton(
            icon: Icon(Icons.search),
            tooltip: 'Search',
            onPressed: null,
          ),
        ],
      ),
      // body is the majority of the screen.
      body: const Center(
        child: Text('Hello, world!'),
      ),
      floatingActionButton: const FloatingActionButton(
        tooltip: 'Add', // used by assistive technologies
        onPressed: null,
        child: Icon(Icons.add),
      ),
    );
  }
}
```

Now that the code has switched from `MyAppBar` and `MyScaffold` to the
[`AppBar`][] and [`Scaffold`][] widgets, and from `material.dart`,
the app is starting to look a bit more Material.
For example, the app bar has a shadow and the title text inherits the
correct styling automatically. A floating action button is also added.

現在我們已經從 `MyAppBar` 和 `MyScaffold` 切換到了 material.dart 中的 
[`AppBar`][] 和 [`Scaffold`][] widget，
我們的應用更“Material”了一些。
例如，標題欄有了陰影，標題文字會自動繼承正確的樣式，此外還添加了一個浮動操作按鈕。

Notice that widgets are passed as arguments to other widgets.
The [`Scaffold`][] widget takes a number of different widgets as
named arguments, each of which are placed in the `Scaffold`
layout in the appropriate place. Similarly, the
[`AppBar`][] widget lets you pass in widgets for the
[`leading`][] widget, and the [`actions`][] of the [`title`][] widget.
This pattern recurs throughout the framework and is something you
might consider when designing your own widgets.

注意，widget 作為引數傳遞給了另外的 widget。
[`Scaffold`][] widget 將許多不同的 widget 作為命名引數，
每個 widget 都放在了 Scofford 佈局中的合適位置。
同樣的，[`AppBar`][] widget 允許我們給 
[`leading`][]、[`title`][] widget 的 [`actions`][] 傳遞 widget。
這種模式在整個框架會中重複出現，在設計自己的 widget 時可以考慮這種模式。

For more information, see [Material Components widgets][].

有關更多資訊，請參閱 [Material 元件][Material Components widgets]。

{{site.alert.note}}

  Material is one of the 2 bundled designs included with Flutter.
  To create an iOS-centric design,
  see the [Cupertino components][] package,
  which has its own versions of [`CupertinoApp`][], and [`CupertinoNavigationBar`][].

  Material 是 Flutter 中兩個自帶的設計之一，
  如果想要以 iOS 為主的設計，可以參考 [Cupertino components][]，
  它有自己版本的 [`CupertinoApp`][] 和 [`CupertinoNavigationBar`][]。

{{site.alert.end}}


## Handling gestures

## 處理手勢

Most applications include some form of user interaction with the system.
The first step in building an interactive application is to detect
input gestures. See how that works by creating a simple button:

大多數應用都需要透過系統來處理一些使用者互動。
建構互動式應用程式的第一步是檢測輸入手勢，這裡透過建立一個簡單的按鈕來了解其工作原理：

<?code-excerpt "lib/main_mybutton.dart"?>
```run-dartpad:theme-light:mode-flutter:run-false:width-100%:height-600px:split-60:ga_id-starting_code
import 'package:flutter/material.dart';

class MyButton extends StatelessWidget {
  const MyButton({super.key});

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        print('MyButton was tapped!');
      },
      child: Container(
        height: 50.0,
        padding: const EdgeInsets.all(8.0),
        margin: const EdgeInsets.symmetric(horizontal: 8.0),
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(5.0),
          color: Colors.lightGreen[500],
        ),
        child: const Center(
          child: Text('Engage'),
        ),
      ),
    );
  }
}

void main() {
  runApp(
    const MaterialApp(
      home: Scaffold(
        body: Center(
          child: MyButton(),
        ),
      ),
    ),
  );
}
```

The [`GestureDetector`][] widget doesn't have a visual
representation but instead detects gestures made by the
user. When the user taps the [`Container`][],
the `GestureDetector` calls its [`onTap()`][] callback, in this
case printing a message to the console. You can use
`GestureDetector` to detect a variety of input gestures,
including taps, drags, and scales.

[`GestureDetector`][] widget 沒有視覺化的展現，但它能識別使用者的手勢。
當用戶點選 [`Container`][] 時，
`GestureDetector` 會呼叫其 [`onTap()`][] 回呼(Callback)，在這裡會向控制檯列印一條訊息。
你可以使用 `GestureDetector` 檢測各種輸入的手勢，包括點選，拖動和縮放。

Many widgets use a [`GestureDetector`][] to provide
optional callbacks for other widgets. For example, the
[`IconButton`][], [`ElevatedButton`][], and
[`FloatingActionButton`][] widgets have [`onPressed()`][]
callbacks that are triggered when the user taps the widget.

許多 widget 使用 [`GestureDetector`][] 為其他 widget 提供可選的回呼(Callback)。
例如，[`IconButton`][]、[`ElevatedButton`][] 和
[`FloatingActionButton`][] widget 都有 [`onPressed()`][] 回呼(Callback)，
當用戶點選 widget 時就會觸發這些回呼(Callback)。

For more information, see [Gestures in Flutter][].

有關更多資訊，請參閱 [Flutter 中的手勢][Gestures in Flutter]。

## Changing widgets in response to input

## 根據使用者輸入改變 widget

So far, this page has used only stateless widgets.
Stateless widgets receive arguments from their parent widget,
which they store in [`final`][] member variables.
When a widget is asked to [`build()`][], it uses these stored
values to derive new arguments for the widgets it creates.

到目前為止，這個頁面僅使用了無狀態的 widget。
無狀態 widget 接收的引數來自於它的父 widget，
它們儲存在 [`final`][] 成員變數中。
當 widget 需要被 [`build()`][] 時，
就是用這些儲存的變數為建立的 widget 產生新的引數。

In order to build more complex experiences&mdash;for example,
to react in more interesting ways to user input&mdash;applications
typically carry some state. Flutter uses `StatefulWidgets` to capture
this idea. `StatefulWidgets` are special widgets that know how to generate
`State` objects, which are then used to hold state.
Consider this basic example, using the [`ElevatedButton`][] mentioned earlier:

為了建構更復雜的體驗，例如，
以更有趣的方式對使用者輸入做出反應&mdash;應用通常帶有一些狀態。
Flutter 使用 StatefulWidgets 來實現這一想法。
StatefulWidgets 是一種特殊的 widget，
它會產生 State 物件，用於儲存狀態。看看這個基本的例子，
它使用了前面提到的 [`ElevatedButton`][]：

<?code-excerpt "lib/main_counter.dart"?>
```run-dartpad:theme-light:mode-flutter:run-false:width-100%:height-600px:split-60:ga_id-starting_code
import 'package:flutter/material.dart';

class Counter extends StatefulWidget {
  // This class is the configuration for the state.
  // It holds the values (in this case nothing) provided
  // by the parent and used by the build  method of the
  // State. Fields in a Widget subclass are always marked
  // "final".

  const Counter({super.key});

  @override
  State<Counter> createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int _counter = 0;

  void _increment() {
    setState(() {
      // This call to setState tells the Flutter framework
      // that something has changed in this State, which
      // causes it to rerun the build method below so that
      // the display can reflect the updated values. If you
      // change _counter without calling setState(), then
      // the build method won't be called again, and so
      // nothing would appear to happen.
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    // This method is rerun every time setState is called,
    // for instance, as done by the _increment method above.
    // The Flutter framework has been optimized to make
    // rerunning build methods fast, so that you can just
    // rebuild anything that needs updating rather than
    // having to individually changes instances of widgets.
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        ElevatedButton(
          onPressed: _increment,
          child: const Text('Increment'),
        ),
        const SizedBox(width: 16),
        Text('Count: $_counter'),
      ],
    );
  }
}

void main() {
  runApp(
    const MaterialApp(
      home: Scaffold(
        body: Center(
          child: Counter(),
        ),
      ),
    ),
  );
}
```

You might wonder why `StatefulWidget` and `State` are separate objects.
In Flutter, these two types of objects have different life cycles.
`Widgets` are temporary objects, used to construct a presentation of
the application in its current state. `State` objects, on the other
hand, are persistent between calls to
`build()`, allowing them to remember information.

您可能想知道為什麼 StatefulWidget 和 State 是獨立的物件。
在 Flutter 中，這兩種型別的物件具有不同的生命週期。
Widget 是臨時物件，用於構造應用當前狀態的展示。
而 State 物件在呼叫 `build()` 之間是持久的，以此來儲存資訊。

The example above accepts user input and directly uses
the result in its `build()` method.  In more complex applications,
different parts of the widget hierarchy might be
responsible for different concerns; for example, one
widget might present a complex user interface
with the goal of gathering specific information,
such as a date or location, while another widget might
use that information to change the overall presentation.

上面的範例接受使用者輸入並直接在其 `build()` 方法中直接使用結果。
在更復雜的應用中，widget 層次不同的部分可能負責不同的關注點；
例如，一個 widget 可能呈現複雜的使用者介面，
來收集像日期或位置這樣特定的資訊，而另一個 widget 可能使用該資訊來改變整體的展現。

In Flutter, change notifications flow "up" the widget
hierarchy by way of callbacks, while current state flows
"down" to the stateless widgets that do presentation.
The common parent that redirects this flow is the `State`.
The following slightly more complex example shows how
this works in practice:

在 Flutter 中，widget 透過回呼(Callback)得到狀態改變的通知，
同時當前狀態通知給其他 widget 用於顯示。
重新導向這一流程的共同父級是 `State`，
下面稍微複雜的範例顯示了它在實踐中的工作原理：

<?code-excerpt "lib/main_counterdisplay.dart"?>
```run-dartpad:theme-light:mode-flutter:run-false:width-100%:height-600px:split-60:ga_id-starting_code
import 'package:flutter/material.dart';

class CounterDisplay extends StatelessWidget {
  const CounterDisplay({required this.count, super.key});

  final int count;

  @override
  Widget build(BuildContext context) {
    return Text('Count: $count');
  }
}

class CounterIncrementor extends StatelessWidget {
  const CounterIncrementor({required this.onPressed, super.key});

  final VoidCallback onPressed;

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      child: const Text('Increment'),
    );
  }
}

class Counter extends StatefulWidget {
  const Counter({super.key});

  @override
  State<Counter> createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int _counter = 0;

  void _increment() {
    setState(() {
      ++_counter;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        CounterIncrementor(onPressed: _increment),
        const SizedBox(width: 16),
        CounterDisplay(count: _counter),
      ],
    );
  }
}

void main() {
  runApp(
    const MaterialApp(
      home: Scaffold(
        body: Center(
          child: Counter(),
        ),
      ),
    ),
  );
}
```

Notice the creation of two new stateless widgets,
cleanly separating the concerns of _displaying_ the counter
(`CounterDisplay`) and _changing_ the counter (`CounterIncrementor`).
Although the net result is the same as the previous example,
the separation of responsibility allows greater complexity to
be encapsulated in the individual widgets,
while maintaining simplicity in the parent.

注意建立兩個新的無狀態 widget 的方式，
它清楚地分離了 **顯示** 計數器（`CounterDisplay`）和 
**改變** 計數器（`CounterIncrementor`）。
儘管最終結果與前面的範例相同，
但是責任的分離將更大的複雜性封裝在各個 widget 中，保證了父級的簡單性。

For more information, see:

有關更多資訊，請參閱：

* [`StatefulWidget`][]
  
  [API 文件: StatefulWidget][`StatefulWidget`]

* [`setState()`][]

  [API 文件: State.setState][`setState()`]

## Bringing it all together

## 整合在一起

What follows is a more complete example that brings together
these concepts: A hypothetical shopping application displays various
products offered for sale, and maintains a shopping cart for
intended purchases. Start by defining the presentation class,
`ShoppingListItem`:

下面是一個更完整的範例，彙集了上面介紹的概念：
假定一個購物應用顯示各種出售的產品，並在購物車中維護想購買的物品。
首先定義一個用於展示的類，`ShoppingListItem`：

<?code-excerpt "lib/main_shoppingitem.dart"?>
```run-dartpad:theme-light:mode-flutter:run-false:width-100%:height-600px:split-60:ga_id-starting_code
import 'package:flutter/material.dart';

class Product {
  const Product({required this.name});

  final String name;
}

typedef CartChangedCallback = Function(Product product, bool inCart);

class ShoppingListItem extends StatelessWidget {
  ShoppingListItem({
    required this.product,
    required this.inCart,
    required this.onCartChanged,
  }) : super(key: ObjectKey(product));

  final Product product;
  final bool inCart;
  final CartChangedCallback onCartChanged;

  Color _getColor(BuildContext context) {
    // The theme depends on the BuildContext because different
    // parts of the tree can have different themes.
    // The BuildContext indicates where the build is
    // taking place and therefore which theme to use.

    return inCart //
        ? Colors.black54
        : Theme.of(context).primaryColor;
  }

  TextStyle? _getTextStyle(BuildContext context) {
    if (!inCart) return null;

    return const TextStyle(
      color: Colors.black54,
      decoration: TextDecoration.lineThrough,
    );
  }

  @override
  Widget build(BuildContext context) {
    return ListTile(
      onTap: () {
        onCartChanged(product, inCart);
      },
      leading: CircleAvatar(
        backgroundColor: _getColor(context),
        child: Text(product.name[0]),
      ),
      title: Text(product.name, style: _getTextStyle(context)),
    );
  }
}

void main() {
  runApp(
    MaterialApp(
      home: Scaffold(
        body: Center(
          child: ShoppingListItem(
            product: const Product(name: 'Chips'),
            inCart: true,
            onCartChanged: (product, inCart) {},
          ),
        ),
      ),
    ),
  );
}
```

The `ShoppingListItem` widget follows a common pattern
for stateless widgets.  It stores the values it receives
in its constructor in [`final`][] member variables,
which it then uses during its [`build()`][] function.
For example, the `inCart` boolean toggles between two visual
appearances: one that uses the primary color from the current
theme, and another that uses gray.

`ShoppingListItem` widget 遵循無狀態 widget 的通用模式。
它將建構函式中接受到的值儲存在 [`final`][] 成員變數中，
然後在 [`build()`][] 函式中使用它們。
例如，`inCart` 布林值使兩種樣式進行切換：
一個使用當前主題的主要顏色，另一個使用灰色。

When the user taps the list item, the widget doesn't modify
its `inCart` value directly. Instead, the widget calls the
`onCartChanged` function it received from its parent widget.
This pattern lets you store state higher in the widget
hierarchy, which causes the state to persist for longer periods of time.
In the extreme, the state stored on the widget passed to
[`runApp()`][] persists for the lifetime of the
application.

當用戶點選列表中的一項，widget 不會直接改變 `inCart` 的值，
而是透過呼叫從父 widget 接收到的 `onCartChanged` 函式。
這種方式可以在元件的生命週期中儲存狀態更長久，
從而使狀態持久化。甚至，widget 傳給 [`runApp()`][] 
的狀態可以持久到整個應用的生命週期。

When the parent receives the `onCartChanged` callback,
the parent updates its internal state, which triggers
the parent to rebuild and create a new instance
of `ShoppingListItem` with the new `inCart` value.
Although the parent creates a new instance of
`ShoppingListItem` when it rebuilds, that operation is cheap
because the framework compares the newly built widgets with the previously
built widgets and applies only the differences to the underlying
[`RenderObject`][].

當父級接收到 `onCartChanged` 回呼(Callback)時，父級會更新其內部狀態，
從而觸發父級重建並使用新的 `inCart` 值來建立新的 `ShoppingListItem` 例項。
儘管父級在重建時會建立 `ShoppingListItem` 的新例項，
但是由於框架會將新建構的 widget 與先前建構的 widget 進行比較，
僅將差異應用於底層的 [`RenderObject`][]，這種代價是很小的。

Here's an example parent widget that stores mutable state:

這裡有一個範例展示父元件是如何儲存可變狀態：

<?code-excerpt "lib/main_shoppinglist.dart"?>
```run-dartpad:theme-light:mode-flutter:run-false:width-100%:height-600px:split-60:ga_id-starting_code
import 'package:flutter/material.dart';

class Product {
  const Product({required this.name});

  final String name;
}

typedef CartChangedCallback = Function(Product product, bool inCart);

class ShoppingListItem extends StatelessWidget {
  ShoppingListItem({
    required this.product,
    required this.inCart,
    required this.onCartChanged,
  }) : super(key: ObjectKey(product));

  final Product product;
  final bool inCart;
  final CartChangedCallback onCartChanged;

  Color _getColor(BuildContext context) {
    // The theme depends on the BuildContext because different
    // parts of the tree can have different themes.
    // The BuildContext indicates where the build is
    // taking place and therefore which theme to use.

    return inCart //
        ? Colors.black54
        : Theme.of(context).primaryColor;
  }

  TextStyle? _getTextStyle(BuildContext context) {
    if (!inCart) return null;

    return const TextStyle(
      color: Colors.black54,
      decoration: TextDecoration.lineThrough,
    );
  }

  @override
  Widget build(BuildContext context) {
    return ListTile(
      onTap: () {
        onCartChanged(product, inCart);
      },
      leading: CircleAvatar(
        backgroundColor: _getColor(context),
        child: Text(product.name[0]),
      ),
      title: Text(
        product.name,
        style: _getTextStyle(context),
      ),
    );
  }
}

class ShoppingList extends StatefulWidget {
  const ShoppingList({required this.products, super.key});

  final List<Product> products;

  // The framework calls createState the first time
  // a widget appears at a given location in the tree.
  // If the parent rebuilds and uses the same type of
  // widget (with the same key), the framework re-uses
  // the State object instead of creating a new State object.

  @override
  State<ShoppingList> createState() => _ShoppingListState();
}

class _ShoppingListState extends State<ShoppingList> {
  final _shoppingCart = <Product>{};

  void _handleCartChanged(Product product, bool inCart) {
    setState(() {
      // When a user changes what's in the cart, you need
      // to change _shoppingCart inside a setState call to
      // trigger a rebuild.
      // The framework then calls build, below,
      // which updates the visual appearance of the app.

      if (!inCart) {
        _shoppingCart.add(product);
      } else {
        _shoppingCart.remove(product);
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Shopping List'),
      ),
      body: ListView(
        padding: const EdgeInsets.symmetric(vertical: 8.0),
        children: widget.products.map((product) {
          return ShoppingListItem(
            product: product,
            inCart: _shoppingCart.contains(product),
            onCartChanged: _handleCartChanged,
          );
        }).toList(),
      ),
    );
  }
}

void main() {
  runApp(const MaterialApp(
    title: 'Shopping App',
    home: ShoppingList(
      products: [
        Product(name: 'Eggs'),
        Product(name: 'Flour'),
        Product(name: 'Chocolate chips'),
      ],
    ),
  ));
}
```

The `ShoppingList` class extends [`StatefulWidget`][],
which means this widget stores mutable state.
When the `ShoppingList` widget is first inserted
into the tree, the framework calls the [`createState()`][] function
to create a fresh instance of `_ShoppingListState` to associate with that
location in the tree. (Notice that subclasses of
[`State`][] are typically named with leading underscores to
indicate that they are private implementation details.)
When this widget's parent rebuilds, the parent creates a new instance
of `ShoppingList`, but the framework reuses the `_ShoppingListState`
instance that is already in the tree rather than calling
`createState` again.

`ShoppingList` 類繼承自 [`StatefulWidget`][]，
這意味著這個 widget 儲存著可變狀態。
當 `ShoppingList` 首次插入到 widget 樹中時，
框架呼叫 [`createState()`][] 函式來建立 `_ShoppingListState`
的新例項，以與樹中的該位置相關聯。
（注意，[`State`][] 的子類別通常以下劃線開頭進行命名，
表示它們的實現細節是私有的）
當該 widget 的父 widget 重建時，
父 widget 首先會建立一個 `ShoppingList` 的例項，
但是框架會複用之前建立的 `_ShoppingListState`，
而不會重新呼叫 `createState`。

To access properties of the current `ShoppingList`,
the `_ShoppingListState` can use its [`widget`][] property.
If the parent rebuilds and creates a new `ShoppingList`,
the `_ShoppingListState` rebuilds with the new widget value.
If you wish to be notified when the `widget` property changes,
override the [`didUpdateWidget()`][] function, which is passed
an `oldWidget` to let you compare the old widget with
the current widget.

為了訪問當前 `ShoppingList` 的屬性，
`_ShoppingListState` 可以使用它的 [`widget`][] 屬性。
當父元件重建一個新的 `ShoppingList` 時，
`_ShoppingListState` 會使用新的 [`widget`][] 值來建立。
如果希望在 [`widget`][] 屬性更改時收到通知，
則可以重寫 [`didUpdateWidget()`][] 函式，
該函式將 `oldWidget` 作為引數傳遞，
以便將 `oldWidget` 與當前 widget 進行比較。

When handling the `onCartChanged` callback, the `_ShoppingListState`
mutates its internal state by either adding or removing a product from
`_shoppingCart`. To signal to the framework that it changed its internal
state, it wraps those calls in a [`setState()`][] call.
Calling `setState` marks this widget as dirty and schedules it to be rebuilt
the next time your app needs to update the screen.
If you forget to call `setState` when modifying the internal
state of a widget, the framework won't know your widget is
dirty and might not call the widget's [`build()`][] function,
which means the user interface might not update to reflect
the changed state.  By managing state in this way,
you don't need to write separate code for creating and
updating child widgets. Instead, you simply implement the `build`
function, which handles both situations.

當處理 `onCartChanged` 回呼(Callback)時，`_ShoppingListState` 
透過增加或刪除 `_shoppingCart` 中的產品來改變其內部狀態。
為了通知框架它改變了它的內部狀態，
需要呼叫 [`setState()`][]，將該 widget 標記為「dirty」（髒標記），
並且計劃在下次應用需要更新螢幕時重新建構它。
如果在修改 widget 的內部狀態後忘記呼叫 setState，
框架將不知道這個 widget 是「dirty」（髒標記），
並且可能不會呼叫 widget 的 [`build()`][] 方法，
這意味著使用者介面可能不會更新以展示新的狀態。
透過以這種方式管理狀態，你不需要編寫用於建立和更新子 widget 的單獨程式碼。
相反，你只需實現 build 函式，它可以處理這兩種情況。

## Responding to widget lifecycle events

## 響應 widget 的生命週期事件

After calling [`createState()`][] on the `StatefulWidget`,
the framework inserts the new state object into the tree and
then calls [`initState()`][] on the state object.
A subclass of [`State`][] can override `initState` to do work
that needs to happen just once. For example, override `initState`
to configure animations or to subscribe to platform services.
Implementations of `initState` are required to start
by calling `super.initState`.

在 StatefulWidget 上呼叫 [`createState()`][] 之後，
框架將新的狀態物件插入到樹中，
然後在狀態物件上呼叫 [`initState()`][]。
[`State`][] 的子類別可以重寫 `initState` 來完成只需要發生一次的工作。
例如，重寫 `initState` 來配置動畫或訂閱平台服務。
實現 `initState` 需要呼叫父類別的 `super.initState` 方法來開始。

When a state object is no longer needed,
the framework calls [`dispose()`][] on the state object.
Override the `dispose` function to do cleanup work.
For example, override `dispose` to cancel timers or to
unsubscribe from platform services. Implementations of
`dispose` typically end by calling `super.dispose`.

當不再需要狀態物件時，框架會呼叫狀態物件上的 [`dispose()`][] 方法。
可以重寫`dispose` 方法來清理狀態。
例如，重寫 `dispose` 以取消計時器或取消訂閱平台服務。
實現 `dispose` 時通常透過呼叫 `super.dispose` 來結束。

For more information, see [`State`][].

有關更多資訊，請參閱 [`State`][]。

## Keys

Use keys to control which widgets the framework matches up
with other widgets when a widget rebuilds. By default, the
framework matches widgets in the current and previous build
according to their [`runtimeType`][] and the order in which they appear.
With keys, the framework requires that the two widgets have
the same [`key`][] as well as the same `runtimeType`.

使用 key 可以控制框架在 widget 重建時與哪些其他 widget 進行匹配。
預設情況下，框架根據它們的 [`runtimeType`][]
以及它們的顯示順序來匹配。使用 key 時，
框架要求兩個 widget 具有相同的 [`key`][] 和 `runtimeType`。

Keys are most useful in widgets that build many instances of
the same type of widget. For example, the `ShoppingList` widget,
which builds just enough `ShoppingListItem` instances to
fill its visible region:

Key 在建構相同型別 widget 的多個例項時很有用。
例如，`ShoppingList` widget，它只建構剛剛好足夠的 `ShoppingListItem` 例項來填充其可見區域：

 * Without keys, the first entry in the current build
   would always sync with the first entry in the previous build,
   even if, semantically, the first entry in the list just
   scrolled off screen and is no longer visible in the viewport.

   如果沒有 key，當前建構中的第一個條目將始終與前一個建構中的第一個條目同步，
   在語義上，列表中的第一個條目如果滾動出螢幕，那麼它應該不會再在視窗中可見。

 * By assigning each entry in the list a "semantic" key,
   the infinite list can be more efficient because the
   framework syncs entries with matching semantic keys
   and therefore similar (or identical) visual appearances.
   Moreover, syncing the entries semantically means that
   state retained in stateful child widgets remains attached
   to the same semantic entry rather than the entry in the
   same numerical position in the viewport.

   透過給列表中的每個條目分配為“語義” key，無限列表可以更高效，
   因為框架將透過相匹配的語義 key 來同步條目，
   並因此具有相似（或相同）的可視外觀。
   此外，語義上同步條目意味著在有狀態子 widget 中，
   保留的狀態將附加到相同的語義條目上，而不是附加到相同數字位置上的條目。

For more information, see the [`Key`][] API.

有關更多資訊，請參閱 [`Key`][] API。

## Global keys

## 全域 key

Use global keys to uniquely identify child widgets.
Global keys must be globally unique across the entire
widget hierarchy, unlike local keys which need
only be unique among siblings. Because they are
globally unique, a global key can be used to
retrieve the state associated with a widget.

全域 key 可以用來標識唯一子 widget。
全域 key 在整個 widget 結構中必須是全域唯一的，
而不像本地 key 只需要在兄弟 widget 中唯一。
由於它們是全域唯一的，因此可以使用全域 key 來檢索與 widget 關聯的狀態。

For more information, see the [`GlobalKey`][] API.

有關更多資訊，請參閱 [`GlobalKey`][] API。

[`actions`]: {{api}}/material/AppBar-class.html#actions
[adding interactivity to your Flutter app]: {{site.url}}/development/ui/interactive
[`AppBar`]: {{api}}/material/AppBar-class.html
[basic layout codelab]: {{site.url}}/codelabs/layout-basics
[`BoxDecoration`]: {{api}}/painting/BoxDecoration-class.html
[`build()`]: {{api}}/widgets/StatelessWidget/build.html
[building layouts]: {{site.url}}/development/ui/layout
[`Center`]: {{api}}/widgets/Center-class.html
[`Column`]: {{api}}/widgets/Column-class.html
[`Container`]: {{api}}/widgets/Container-class.html
[`createState()`]: {{api}}/widgets/StatefulWidget-class.html#createState
[Cupertino components]: {{site.url}}/development/ui/widgets/cupertino
[`CupertinoApp`]: {{api}}/cupertino/CupertinoApp-class.html
[`CupertinoNavigationBar`]: {{api}}/cupertino/CupertinoNavigationBar-class.html
[`didUpdateWidget()`]: {{api}}/widgets/State-class.html#didUpdateWidget
[`dispose()`]: {{api}}/widgets/State-class.html#dispose
[`Expanded`]: {{api}}/widgets/Expanded-class.html
[`final`]: {{site.dart-site}}/guides/language/language-tour#final-and-const
[`flex`]: {{api}}/widgets/Expanded-class.html#flex
[`FloatingActionButton`]: {{api}}/material/FloatingActionButton-class.html
[Gestures in Flutter]: {{site.url}}/development/ui/advanced/gestures
[`GestureDetector`]: {{api}}/widgets/GestureDetector-class.html
[`GlobalKey`]: {{api}}/widgets/GlobalKey-class.html
[`IconButton`]: {{api}}/material/IconButton-class.html
[`initState()`]: {{api}}/widgets/State-class.html#initState
[`key`]: {{api}}/widgets/Widget-class.html#key
[`Key`]: {{api}}/foundation/Key-class.html
[Layouts]: {{site.url}}/development/ui/widgets/layout
[`leading`]: {{api}}/material/AppBar-class.html#leading
[Material Components widgets]: {{site.url}}/development/ui/widgets/material
[Material icons]: https://design.google.com/icons/
[`MaterialApp`]: {{api}}/material/MaterialApp-class.html
[`Navigator`]: {{api}}/widgets/Navigator-class.html
[`onPressed()`]: {{api}}/material/ElevatedButton-class.html#onPressed
[`onTap()`]: {{api}}/widgets/GestureDetector-class.html#onTap
[`Positioned`]: {{api}}/widgets/Positioned-class.html
[`ElevatedButton`]: {{api}}/material/ElevatedButton-class.html
[React]: https://reactjs.org
[`RenderObject`]: {{api}}/rendering/RenderObject-class.html
[`Row`]: {{api}}/widgets/Row-class.html
[`runApp()`]: {{api}}/widgets/runApp.html
[`runtimeType`]: {{api}}/widgets/Widget-class.html#runtimeType
[`Scaffold`]: {{api}}/material/Scaffold-class.html
[`setState()`]: {{api}}/widgets/State/setState.html
[`Stack`]: {{api}}/widgets/Stack-class.html
[`State`]: {{api}}/widgets/State-class.html
[`StatefulWidget`]: {{api}}/widgets/StatefulWidget-class.html
[`StatelessWidget`]: {{api}}/widgets/StatelessWidget-class.html
[`Text`]: {{api}}/widgets/Text-class.html
[`title`]: {{api}}/material/AppBar-class.html#title
[`widget`]: {{api}}/widgets/State-class.html#widget
[`Widget`]: {{api}}/widgets/Widget-class.html
