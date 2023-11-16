---
title: Fade a widget in and out
title: Widget 的淡入淡出效果
description: How to fade a widget in and out.
description: 如何淡入淡出一個 widget。
tags: cookbook, 實用課程, 動畫效果
keywords: 淡入淡出效果,隱藏元素,使用者體驗
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/animation/opacity_animation/"?>

UI developers often need to show and hide elements on screen.
However, quickly popping elements on and off the screen can
feel jarring to end users. Instead,
fade elements in and out with an opacity animation to create
a smooth experience.

在實現 UI 設計時，我們經常需要在螢幕上顯示或隱藏各種元素。
如若這個過程只是讓某個元素快速地出現或者消失，使用者們肯定不買帳。
我們一般會使用不透明動畫讓元素淡入淡出，以創建出更加流暢的使用者體驗。

The [`AnimatedOpacity`][] widget makes it easy to perform opacity
animations. This recipe uses the following steps:

在 Flutter 中，你可以使用 [`AnimatedOpacity`][] widget 來完成這個效果，
請參見下面的步驟：

  1. Create a box to fade in and out.

     建立一個用來淡入淡出的方框。

  2. Define a `StatefulWidget`.

     定義一個 `StatefulWidget`。

  3. Display a button that toggles the visibility.

     顯示一個用於切換可見狀態的按鈕

  4. Fade the box in and out.

     淡入淡出方框。

## 1. Create a box to fade in and out

## 1. 建立一個用來淡入淡出的方框

First, create something to fade in and out. For this example,
draw a green box on screen.

首先是建立一個來淡入淡出的東西。在這個範例中，你將在螢幕上繪製一個綠色的方框。

<?code-excerpt "lib/main.dart (Container)" replace="/^child: //g;/,$//g"?>
```dart
Container(
  width: 200,
  height: 200,
  color: Colors.green,
)
```

## 2. Define a `StatefulWidget`

## 2. 定義一個 `StatefulWidget`

Now that you have a green box to animate,
you need a way to know whether the box should be visible.
To accomplish this, use a [`StatefulWidget`][].

我們要對這個綠色的方框進行動畫。
那麼為了表示這個方框的狀態是否可見，你需要使用 [`StatefulWidget`][]。

A `StatefulWidget` is a class that creates a `State` object.
The `State` object holds some data about the app and provides a way to
update that data. When updating the data,
you can also ask Flutter to rebuild the UI with those changes.

`StatefulWidget` 是一個類別，它將會建立一個 `State` 物件。
而這個 `State` 物件將包含與這個應用相關的一些資料，並且能夠更新它們。
當你更新資料時，可以讓Flutter使用這些更改去重建使用者介面。

In this case, you have one piece of data:
a boolean representing whether the button is visible.

在這個範例中，我們將使用一個布林值來表示其是否可見。

To construct a `StatefulWidget`, create two classes: A
`StatefulWidget` and a corresponding `State` class.
Pro tip: The Flutter plugins for Android Studio and VSCode include
the `stful` snippet to quickly generate this code.

要構造一個 `StatefulWidget`，
你需要建立兩個類：一個 `StatefulWidget` 類以及與其對應的 `State` 類別。
小提示：Android Studio 和 VSCode 的 Flutter 外掛都包含了 `stful` 片段，
能夠快速產生該程式碼。

<?code-excerpt "lib/starter.dart (Starter)" remove="return Container();"?>
```dart
// The StatefulWidget's job is to take data and create a State class.
// In this case, the widget takes a title, and creates a _MyHomePageState.
class MyHomePage extends StatefulWidget {
  final String title;

  const MyHomePage({
    super.key,
    required this.title,
  });

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

// The State class is responsible for two things: holding some data you can
// update and building the UI using that data.
class _MyHomePageState extends State<MyHomePage> {
  // Whether the green box should be visible.
  bool _visible = true;

  @override
  Widget build(BuildContext context) {
    // The green box goes here with some other Widgets.
  }
}
```

## 3. Display a button that toggles the visibility

## 3. 顯示一個用於切換可見狀態的按鈕

Now that you have some data to determine whether the green box
should be visible, you need a way to update that data.
In this example, if the box is visible, hide it.
If the box is hidden, show it.

現在你已經有了一些資料能夠決定這個綠色方框是否可見，
但是還需要一個方法來改變這些資料。
在這個例子中，我們想讓方框在顯示與隱藏之間切換。

To handle this, display a button. When a user presses the button,
flip the boolean from true to false, or false to true.
Make this change using [`setState()`][],
which is a method on the `State` class.
This tells Flutter to rebuild the widget.

為此你將使用一個按鈕——當用戶按下按鈕時，
資料將會在 true 和 false 之間進行切換。
為了使改變生效，你需要使用 `State` 類中的 [`setState()`][] 方法，
這會使 Flutter 重建這個 widget。

For more information on working with user input,
see the [Gestures][] section of the cookbook.

注意：如果你想要了解更多與使用者輸入相關的資料，
請參閱 Cookbook 的 [Gestures][] 部分。

<?code-excerpt "lib/main.dart (FAB)" replace="/^floatingActionButton: //g;/,$//g"?>
```dart
FloatingActionButton(
  onPressed: () {
    // Call setState. This tells Flutter to rebuild the
    // UI with the changes.
    setState(() {
      _visible = !_visible;
    });
  },
  tooltip: 'Toggle Opacity',
  child: const Icon(Icons.flip),
)
```

## 4. Fade the box in and out

## 4. 淡入淡出方框

You have a green box on screen and a button to toggle the visibility
to `true` or `false`. How to fade the box in and out? With an
[`AnimatedOpacity`][] widget.

現在你的螢幕上已經有一個綠色的方框，
以及一個可以透過改變 `true` 或 `false` 來切換方框可見性的按鈕。
那麼該如何讓方框淡入淡出呢？答案是使用 [`AnimatedOpacity`][] widget。

The `AnimatedOpacity` widget requires three arguments:

`AnimatedOpacity` 小部件需要傳入三個引數：

  * `opacity`: A value from 0.0 (invisible) to 1.0 (fully visible).

    `opacity`：它的取值範圍從 0.0（不可見）到 1.0（完全可見）。

  * `duration`: How long the animation should take to complete.

    `duration`：代表這個動畫需要持續多長時間。

  * `child`: The widget to animate. In this case, the green box.

    `child`：需要進行動畫的小部件。在這個例子中就是那個綠色的方框。

<?code-excerpt "lib/main.dart (AnimatedOpacity)" replace="/^child: //g;/,$//g"?>
```dart
AnimatedOpacity(
  // If the widget is visible, animate to 0.0 (invisible).
  // If the widget is hidden, animate to 1.0 (fully visible).
  opacity: _visible ? 1.0 : 0.0,
  duration: const Duration(milliseconds: 500),
  // The green box must be a child of the AnimatedOpacity widget.
  child: Container(
    width: 200,
    height: 200,
    color: Colors.green,
  ),
)
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
    const appTitle = 'Opacity Demo';
    return const MaterialApp(
      title: appTitle,
      home: MyHomePage(title: appTitle),
    );
  }
}

// The StatefulWidget's job is to take data and create a State class.
// In this case, the widget takes a title, and creates a _MyHomePageState.
class MyHomePage extends StatefulWidget {
  const MyHomePage({
    super.key,
    required this.title,
  });

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

// The State class is responsible for two things: holding some data you can
// update and building the UI using that data.
class _MyHomePageState extends State<MyHomePage> {
  // Whether the green box should be visible
  bool _visible = true;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: AnimatedOpacity(
          // If the widget is visible, animate to 0.0 (invisible).
          // If the widget is hidden, animate to 1.0 (fully visible).
          opacity: _visible ? 1.0 : 0.0,
          duration: const Duration(milliseconds: 500),
          // The green box must be a child of the AnimatedOpacity widget.
          child: Container(
            width: 200,
            height: 200,
            color: Colors.green,
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          // Call setState. This tells Flutter to rebuild the
          // UI with the changes.
          setState(() {
            _visible = !_visible;
          });
        },
        tooltip: 'Toggle Opacity',
        child: const Icon(Icons.flip),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/fade-in-out.gif" alt="Fade In and Out Demo" class="site-mobile-screenshot" />
</noscript>

[`AnimatedOpacity`]: {{site.api}}/flutter/widgets/AnimatedOpacity-class.html
[Gestures]: {{site.url}}/cookbook#gestures
[`StatefulWidget`]: {{site.api}}/flutter/widgets/StatefulWidget-class.html
[`setState()`]: {{site.api}}/flutter/widgets/State/setState.html
