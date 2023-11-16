---
title: Animate the properties of a container
title: Container 裡的動畫漸變效果
description: How to animate properties of a container using implicit animations.
description: 如何透過控制 container 的屬性以使用隱含動畫。
tags: cookbook, 實用課程, 動畫效果
keywords: Container,動畫,漸變,背景顏色,隱含動畫
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/animation/animated_container/"?>

The [`Container`][] class provides a convenient way
to create a widget with specific properties:
width, height, background color, padding, borders, and more.

[`Container`][] 類提供了一系列實用方法，能夠便捷地創建出一個具有
指定寬度、高度、背景顏色、外邊距和邊框等屬性的 widget。

Simple animations often involve changing these properties over time.
For example,
you might want to animate the background color from grey to green to
indicate that an item has been selected by the user.

簡單的動畫通常會在一段時間內改變這些屬性。
例如你可能想將灰色背景逐漸變為綠色背景來告訴使用者已經選擇了某個專案。

To animate these properties,
Flutter provides the [`AnimatedContainer`][] widget.
Like the `Container` widget, `AnimatedContainer` allows you to define
the width, height, background colors, and more. However, when the
`AnimatedContainer` is rebuilt with new properties, it automatically
animates between the old and new values. In Flutter, these types of
animations are known as "implicit animations."

為了製作這樣的簡單動畫效果，Flutter 提供了
[`AnimatedContainer`][] widget。與 `Container` 一樣，
`AnimatedContainer` 也可以設定它的寬度、高度以及背景顏色等等。
但是 `AnimatedContainer` 在使用新屬性進行重建時，
將會自動在舊值和新值之間產生動畫。
這種動畫在 Flutter 中被稱為“隱含動畫”。

This recipe describes how to use an `AnimatedContainer` to animate the size,
background color, and border radius when the user taps a button
using the following steps:

下面這篇課程將介紹如何使用 `AnimatedContainer` 實現
當用戶點選按鈕時改變它的大小、背景顏色，以及邊框半徑的動畫。

## Directions

## 步驟

  1. Create a StatefulWidget with default properties.

     建立一個擁有預設屬性的 StatefulWidget

  2. Build an `AnimatedContainer` using the properties.

     建立一個使用這些屬性的 `AnimatedContainer`

  3. Start the animation by rebuilding with new properties.

     透過設定新的屬性觸發重建並啟動動畫

## 1. Create a StatefulWidget with default properties

## 1. 建立一個擁有預設屬性的 StatefulWidget

To start, create [`StatefulWidget`][] and [`State`][] classes.
Use the custom State class to define the properties that change over
time. In this example, that includes the width, height, color, and border
radius. You can also define the default value of each property.

首先你需要建立一個
[`StatefulWidget`][] 類和 [`State`][] 類別。
然後在 State 類中定義需要隨時間更改的屬性。
在這個範例中，我們將會改變其寬度、高度、顏色和邊框半徑。
此外，你還可以定義其他預設屬性。

These properties belong to a custom `State` class so they
can be updated when the user taps a button.

但是這些屬性必須定義在 `State` 類中，這樣我們才能在使用者點選按鈕時更新它們。

<?code-excerpt "lib/starter.dart (Starter)" remove="return Container();"?>
```dart
class AnimatedContainerApp extends StatefulWidget {
  const AnimatedContainerApp({super.key});

  @override
  State<AnimatedContainerApp> createState() => _AnimatedContainerAppState();
}

class _AnimatedContainerAppState extends State<AnimatedContainerApp> {
  // Define the various properties with default values. Update these properties
  // when the user taps a FloatingActionButton.
  double _width = 50;
  double _height = 50;
  Color _color = Colors.green;
  BorderRadiusGeometry _borderRadius = BorderRadius.circular(8);

  @override
  Widget build(BuildContext context) {
    // Fill this out in the next steps.
  }
}
```

## 2. Build an `AnimatedContainer` using the properties

## 2. 建立一個使用這些屬性的 AnimatedContainer

Next, build the `AnimatedContainer` using the properties defined in the
previous step. Furthermore, provide a `duration` that defines how long
the animation should run.

接下來，你就可以使用上一步中定義的屬性來建構 `AnimatedContainer`。
此外，你還必須提供一個 `duration` 它將定義這個動畫應該執行多長時間。

<?code-excerpt "lib/main.dart (AnimatedContainer)" replace="/^child: //g;/,$//g"?>
```dart
AnimatedContainer(
  // Use the properties stored in the State class.
  width: _width,
  height: _height,
  decoration: BoxDecoration(
    color: _color,
    borderRadius: _borderRadius,
  ),
  // Define how long the animation should take.
  duration: const Duration(seconds: 1),
  // Provide an optional curve to make the animation feel smoother.
  curve: Curves.fastOutSlowIn,
)
```

## 3. Start the animation by rebuilding with new properties

## 3. 透過設定新的屬性觸發重建並啟動動畫

Finally, start the animation by rebuilding the
`AnimatedContainer` with the new properties.
How to trigger a rebuild?
Use the [`setState()`][] method.

最後將設定新的屬性觸發 `AnimatedContainer` 重建並啟動動畫。那麼如何觸發重建呢？
當我們提到 `StatefulWidgets` 時，[`setState()`][] 就行了。

Add a button to the app. When the user taps the button, update
the properties with a new width, height, background color and border radius
inside a call to `setState()`.

在這個例子中，我們給應用添加了一個按鈕。
當用戶點選按鈕時，將會呼叫 `setState`
去重新整理它的寬度、高度、背景顏色和邊框半徑等屬性。

A real app typically transitions between fixed values (for example,
from a grey to a green background). For this app,
generate new values each time the user taps the button.

實際專案通常只會在某些固定值之間進行轉換（例如從灰色背景過渡到綠色背景）。
在這個應用中，每次使用者點選按鈕都會產生新的值。

<?code-excerpt "lib/main.dart (FAB)" replace="/^floatingActionButton: //g;/,$//g"?>
```dart
FloatingActionButton(
  // When the user taps the button
  onPressed: () {
    // Use setState to rebuild the widget with new values.
    setState(() {
      // Create a random number generator.
      final random = Random();

      // Generate a random width and height.
      _width = random.nextInt(300).toDouble();
      _height = random.nextInt(300).toDouble();

      // Generate a random color.
      _color = Color.fromRGBO(
        random.nextInt(256),
        random.nextInt(256),
        random.nextInt(256),
        1,
      );

      // Generate a random border radius.
      _borderRadius =
          BorderRadius.circular(random.nextInt(100).toDouble());
    });
  },
  child: const Icon(Icons.play_arrow),
)
```

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'dart:math';

import 'package:flutter/material.dart';

void main() => runApp(const AnimatedContainerApp());

class AnimatedContainerApp extends StatefulWidget {
  const AnimatedContainerApp({super.key});

  @override
  State<AnimatedContainerApp> createState() => _AnimatedContainerAppState();
}

class _AnimatedContainerAppState extends State<AnimatedContainerApp> {
  // Define the various properties with default values. Update these properties
  // when the user taps a FloatingActionButton.
  double _width = 50;
  double _height = 50;
  Color _color = Colors.green;
  BorderRadiusGeometry _borderRadius = BorderRadius.circular(8);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('AnimatedContainer Demo'),
        ),
        body: Center(
          child: AnimatedContainer(
            // Use the properties stored in the State class.
            width: _width,
            height: _height,
            decoration: BoxDecoration(
              color: _color,
              borderRadius: _borderRadius,
            ),
            // Define how long the animation should take.
            duration: const Duration(seconds: 1),
            // Provide an optional curve to make the animation feel smoother.
            curve: Curves.fastOutSlowIn,
          ),
        ),
        floatingActionButton: FloatingActionButton(
          // When the user taps the button
          onPressed: () {
            // Use setState to rebuild the widget with new values.
            setState(() {
              // Create a random number generator.
              final random = Random();

              // Generate a random width and height.
              _width = random.nextInt(300).toDouble();
              _height = random.nextInt(300).toDouble();

              // Generate a random color.
              _color = Color.fromRGBO(
                random.nextInt(256),
                random.nextInt(256),
                random.nextInt(256),
                1,
              );

              // Generate a random border radius.
              _borderRadius =
                  BorderRadius.circular(random.nextInt(100).toDouble());
            });
          },
          child: const Icon(Icons.play_arrow),
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/animated-container.gif" alt="這個 AnimatedContainer demo 展示了一個透過動畫改變顏色、邊框半徑、放大和縮小的盒子" class="site-mobile-screenshot" />
</noscript>


[`AnimatedContainer`]: {{site.api}}/flutter/widgets/AnimatedContainer-class.html
[`Container`]: {{site.api}}/flutter/widgets/Container-class.html
[`setState()`]: {{site.api}}/flutter/widgets/State/setState.html
[`State`]: {{site.api}}/flutter/widgets/State-class.html
[`StatefulWidget`]: {{site.api}}/flutter/widgets/StatefulWidget-class.html
