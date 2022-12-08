---
title: Display a snackbars
title: 顯示 snackbars
short-title: SnackBars
short-title: SnackBars
description: How to implement a SnackBar to display messages.
description: 學習使用 SnackBar 展示訊息。
tags: cookbook, 實用課程, 設計
keywords: Material Design 效果, SnackBar, 滑動刪除
prev:
  title: Add a drawer to a screen
  title: 在螢幕上新增一個 Drawer
  path: /docs/cookbook/design/drawer
next:
  title: Export fonts from a package
  title: 以 package 的方式使用字型
  path: /docs/cookbook/design/package-fonts
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/design/snackbars/"?>

It can be useful to briefly inform your users when certain actions
take place. For example, when a user swipes away a message in a list,
you might want to inform them that the message has been deleted.
You might even want to give them an option to undo the action.

在某些情況下，我們需要用方便且友好的方式告訴使用者發生了什麼。
例如，當用戶滑動刪除列表中的一條訊息時，
我們或許想提醒使用者訊息已經被刪除了，或者除了提醒之外，
我們還可以提供一個撤銷的操作。

In Material Design, this is the job of a [`SnackBar`][].
This recipe implements a snackbar using the following steps:

在 Material Design 中，用一個 [`SnackBar`][] 就可以實現這個需求。
請參見以下步驟：

## Directions

## 步驟

  1. Create a `Scaffold`.

     建立一個  `Scaffold`
     
  2. Display a `SnackBar`.

     顯示一個  `Scaffold`
  
  3. Provide an optional action.
  
     提供一個附加的操作
  

## 1. Create a `Scaffold`

## 1. 建立一個 `Scaffold`

When creating apps that follow the Material Design guidelines,
give your apps a consistent visual structure.
In this example, display the `SnackBar` at the bottom of the screen,
without overlapping other important
widgets, such as the `FloatingActionButton`.

在建立遵循 Material Design 設計規範的應用時，
我們希望應用可以有一個一致的視覺層次結構。
當我們在螢幕的底部顯示一個 `SnackBar` 時，
不能覆蓋其他重要的 widgets，比如 `FloatingActionButton`。

The [`Scaffold`][] widget, from the [material library][],
creates this visual structure and ensures that important
widgets don't overlap.

用 [material library][] 中的 [`Scaffold`][] widget
就可以建立一個一致的視覺層次結構，
並且可以確保其他重要的 widgets 不會被覆蓋。

<?code-excerpt "lib/partial.dart (Scaffold)"?>
```dart
return MaterialApp(
  title: 'SnackBar Demo',
  home: Scaffold(
    appBar: AppBar(
      title: const Text('SnackBar Demo'),
    ),
    body: const SnackBarPage(),
  ),
);
```

## 2. Display a `SnackBar`

## 2. 顯示一個 `SnackBar`

With the `Scaffold` in place, display a `SnackBar`.
First, create a `SnackBar`, then display it using `ScaffoldMessenger`.

有了 `Scaffold`，我們就可以顯示一個 `SnackBar` 了。
首先，我們需要先建立一個 `SnackBar` ，然後使用 `ScaffoldMessenger` 來顯示它。

<?code-excerpt "lib/partial.dart (DisplaySnackBar)"?>
```dart
const snackBar = SnackBar(
  content: Text('Yay! A SnackBar!'),
);

// Find the ScaffoldMessenger in the widget tree
// and use it to show a SnackBar.
ScaffoldMessenger.of(context).showSnackBar(snackBar);
```
{{site.alert.note}}
  To learn more, watch this short Widget of the Week video on the ScaffoldMessenger widget:

  <iframe class="full-width" src="{{site.youtube-site}}/embed/lytQi-slT5Y" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{{site.alert.end}}

## 3. Provide an optional action

## 3. 提供一個附加的操作

You might want to provide an action to the user when
the SnackBar is displayed.
For example, if the user accidentally deletes a message,
they might use an optional action in the SnackBar to recover
the message.

在某些情況下，我們可能想在顯示 SnackBar 的時候給使用者提供一個附加的操作。
比如，當他們意外的刪除了一個訊息，我們可以提供一個撤銷更改的操作。

To achieve this, we can provide an additional `action` to the `SnackBar` widget.

Here's an example of providing
an additional `action` to the `SnackBar` widget:

這個例子裡，我們在建立 `SnackBar` widget 的時候提供一個附加的 `action ` 引數。

<?code-excerpt "lib/main.dart (SnackBarAction)"?>
```dart
final snackBar = SnackBar(
  content: const Text('Yay! A SnackBar!'),
  action: SnackBarAction(
    label: 'Undo',
    onPressed: () {
      // Some code to undo the change.
    },
  ),
);
```

## Interactive example

## 互動式範例

{{site.alert.note}}

  In this example, the SnackBar displays when a user taps a button.
  For more information on working with user input,
  see the [Gestures][] section of the cookbook.
  
  注意: 這個例子是當用戶點選一個按鈕的時候顯示一個 SnackBar。
  更多有關處理使用者輸入的資訊，請查閱實用課程 (Cookbook) 的
  [Gestures][] 部分。
  
{{site.alert.end}}

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(const SnackBarDemo());

class SnackBarDemo extends StatelessWidget {
  const SnackBarDemo({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'SnackBar Demo',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('SnackBar Demo'),
        ),
        body: const SnackBarPage(),
      ),
    );
  }
}

class SnackBarPage extends StatelessWidget {
  const SnackBarPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Center(
      child: ElevatedButton(
        onPressed: () {
          final snackBar = SnackBar(
            content: const Text('Yay! A SnackBar!'),
            action: SnackBarAction(
              label: 'Undo',
              onPressed: () {
                // Some code to undo the change.
              },
            ),
          );

          // Find the ScaffoldMessenger in the widget tree
          // and use it to show a SnackBar.
          ScaffoldMessenger.of(context).showSnackBar(snackBar);
        },
        child: const Text('Show SnackBar'),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/snackbar.gif" alt="SnackBar Demo" class="site-mobile-screenshot" />
</noscript>


[Gestures]: {{site.url}}/cookbook#gestures
[`Scaffold`]: {{site.api}}/flutter/material/Scaffold-class.html
[`SnackBar`]: {{site.api}}/flutter/material/SnackBar-class.html
[material library]: {{site.api}}/flutter/material/material-library.html
