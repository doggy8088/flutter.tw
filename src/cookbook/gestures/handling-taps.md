---
title: Handle taps
title: 捕獲和處理點選動作
description: How to handle tapping and dragging.
description: 如何處理點選和拖拽。
tags: cookbook, 實用課程, 手勢操作
keywords: 互動,點選,拖動,snackbar
prev:
  title: Add Material touch ripples
  title: 新增點按漣漪效果 (Material Design)
  path: /docs/cookbook/gestures/ripples
next:
  title: Implement swipe to dismiss
  title: 實現「滑動清除」效果
  path: /docs/cookbook/gestures/dismissible
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/gestures/handling_taps/"?>

You not only want to display information to users,
you want users to interact with your app.
Use the [`GestureDetector`][] widget to respond
to fundamental actions, such as tapping and dragging.

我們的 app 不僅要把資訊展示給使用者，還要和使用者進行互動。
怎麼響應使用者的點選，拖動等操作行為呢？
——使用 [`GestureDetector`][] Widget。

{{site.alert.note}}

  To learn more, watch this short Widget of the Week video on the GestureDetector widget:

  瞭解更多，請參考下方「每週 Widget」的裡關於 GestureDetector 的短影片：

  <iframe class="full-width" src="{{site.youtube-site}}/embed/WhVXkCFPmK4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{{site.alert.end}}

This recipe shows how to make a custom button that shows
a snackbar when tapped with the following steps:

你可以透過以下步驟來實現一個按鈕，
當用戶點選的時候顯示 snackbar 訊息：

  1. Create the button.

     建立一個按鈕。

  2. Wrap it in a `GestureDetector` that an `onTap()` callback.

     用 `GestureDetector` 包裹按鈕，並傳入 `onTap` 回呼(Callback)函式。

<?code-excerpt "lib/main.dart (GestureDetector)" replace="/return //g;/;$//g"?>
```dart
// The GestureDetector wraps the button.
GestureDetector(
  // When the child is tapped, show a snackbar.
  onTap: () {
    const snackBar = SnackBar(content: Text('Tap'));

    ScaffoldMessenger.of(context).showSnackBar(snackBar);
  },
  // The custom button
  child: Container(
    padding: const EdgeInsets.all(12.0),
    decoration: BoxDecoration(
      color: Colors.lightBlue,
      borderRadius: BorderRadius.circular(8.0),
    ),
    child: const Text('My Button'),
  ),
)
```

## Notes

## 注意

  1. For information on adding the Material ripple effect to your
     button, see the [Add Material touch ripples][] recipe.
      
     如果你想新增點按漣漪效果 (Material Design) 請參考文章 
     [新增點按漣漪效果 (Material Design)][Add Material touch ripples]。

  2. Although this example creates a custom button,
     Flutter includes a handful of button implementations, such as:
     [`ElevatedButton`][], [`TextButton`][], and
     [`CupertinoButton`][].

     這裡為了說明原理，我們建立了自訂的按鈕，
     其實 Flutter 已經為我們準備了很多現成的按鈕供我們使用，比如：
     [`ElevatedButton`][]、[`TextButton`][] 和 [`CupertinoButton`][]。

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
    const title = 'Gesture Demo';

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
    // The GestureDetector wraps the button.
    return GestureDetector(
      // When the child is tapped, show a snackbar.
      onTap: () {
        const snackBar = SnackBar(content: Text('Tap'));

        ScaffoldMessenger.of(context).showSnackBar(snackBar);
      },
      // The custom button
      child: Container(
        padding: const EdgeInsets.all(12.0),
        decoration: BoxDecoration(
          color: Colors.lightBlue,
          borderRadius: BorderRadius.circular(8.0),
        ),
        child: const Text('My Button'),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/handling-taps.gif" alt="Handle taps demo" class="site-mobile-screenshot" />
</noscript>


[Add Material touch ripples]: {{site.url}}/cookbook/gestures/ripples
[`CupertinoButton`]: {{site.api}}/flutter/cupertino/CupertinoButton-class.html
[`TextButton`]: {{site.api}}/flutter/material/TextButton-class.html
[`GestureDetector`]: {{site.api}}/flutter/widgets/GestureDetector-class.html
[`ElevatedButton`]: {{site.api}}/flutter/material/ElevatedButton-class.html
