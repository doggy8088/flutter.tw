---
title: Add Material touch ripples
title: 新增點按漣漪效果 (Material Design)
description: How to implement ripple animations.
description: 如何實現漣漪動畫。
tags: cookbook, 實用課程, 手勢操作
keywords: 動畫,漣漪效果
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/gestures/ripples/"?>

Widgets that follow the Material Design guidelines display
a ripple animation when tapped.

當我們在開發遵循 Material Design 規範應用的時候，
我們可能會需要為某個 widgets 的點選加入漣漪效果。

Flutter provides the [`InkWell`][]
widget to perform this effect.
Create a ripple effect using the following steps:

Flutter 提供了 [`InkWell`][] widget 來實現這個功能。
你可以透過以下步驟實現漣漪效果：

  1. Create a widget that supports tap.

     建立一個想要點選的 widget；

  2. Wrap it in an `InkWell` widget to manage tap callbacks and
     ripple animations.

     用 `InkWell` widget 包裹它，並設定回呼(Callback)函式，
     就可以顯示漣漪動畫了。

<?code-excerpt "lib/main.dart (InkWell)" replace="/return //g;/;$//g"?>
```dart
// The InkWell wraps the custom flat button widget.
InkWell(
  // When the user taps the button, show a snackbar.
  onTap: () {
    ScaffoldMessenger.of(context).showSnackBar(const SnackBar(
      content: Text('Tap'),
    ));
  },
  child: const Padding(
    padding: EdgeInsets.all(12),
    child: Text('Flat Button'),
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
    const title = 'InkWell Demo';

    return const MaterialApp(
      title: title,
      home: MyHomePage(title: title),
    );
  }
}

class MyHomePage extends StatelessWidget {
  final String title;

  const MyHomePage({super.key, required this.title});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: const Center(
        child: MyButton(),
      ),
    );
  }
}

class MyButton extends StatelessWidget {
  const MyButton({super.key});

  @override
  Widget build(BuildContext context) {
    // The InkWell wraps the custom flat button widget.
    return InkWell(
      // When the user taps the button, show a snackbar.
      onTap: () {
        ScaffoldMessenger.of(context).showSnackBar(const SnackBar(
          content: Text('Tap'),
        ));
      },
      child: const Padding(
        padding: EdgeInsets.all(12),
        child: Text('Flat Button'),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/ripples.gif" alt="Ripples Demo" class="site-mobile-screenshot" />
</noscript>


[`InkWell`]: {{site.api}}/flutter/material/InkWell-class.html
