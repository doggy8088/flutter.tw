---
title: Add a Drawer to a screen
title: 在螢幕上新增一個 Drawer
description: How to implement a Material Drawer.
description: 如何實現一個 Material 風格的 Drawer。
tags: cookbook, 實用課程, 設計
keywords: Material Design 效果, Tab效果
prev:
  title: Fade a widget in and out
  title: Widget 的淡入淡出效果
  path: /docs/cookbook/animation/opacity-animation
next:
  title: Displaying SnackBars
  title: 顯示 SnackBars
  path: /docs/cookbook/design/snackbars
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/design/drawer"?>

In apps that use Material Design,
there are two primary options for navigation: tabs and drawers.
When there is insufficient space to support tabs,
drawers provide a handy alternative.

在 Material Design 設計準則裡，主要提供了兩種導航方式：Tab 和 Drawer。
當沒有足夠的空間來支援 tab 導航時，drawer 提供了另一個方便的選擇。

In Flutter, use the [`Drawer`][] widget in combination with a
[`Scaffold`][] to create a layout with a Material Design drawer.
This recipe uses the following steps:

在 Flutter中，我們可以將 [`Drawer`][] widget 與 [`Scaffold`][] 結合使用
來建立一個具有 Material Design 風格的 Drawer 佈局。
請參見如下的步驟：

## Directions

## 步驟

  1. Create a `Scaffold`

     建立一個 `Scaffold`

  2. Add a drawer

     新增一個 drawer

  3. Populate the drawer with items

     向 drawer 中新增內容

  4. Close the drawer programmatically

     透過程式設計關閉 drawer

## 1. Create a `Scaffold`

## 1. 建立一個 `Scaffold`

To add a drawer to the app, wrap it in a [`Scaffold`][] widget.
The `Scaffold` widget provides a consistent visual structure to apps that
follow the Material Design Guidelines.
It also supports special Material Design
components, such as Drawers, AppBars, and SnackBars.

為了嚮應用中新增一個 Drawer，我們需要將其放在 [`Scaffold`][] widget 中。
Scaffold Widget 為遵循 Material 設計守則的應用程式提供了一套統一的視覺化結構。
它同樣支援一些特殊的 Material Design 元件，例如 Drawer，AppBar 和 SnackBar 等。

In this example, create a `Scaffold` with a `drawer`:

在這個例子中，我們想要建立一個帶有 `drawer` 的 `Scaffold`：

<?code-excerpt "lib/drawer.dart (DrawerStart)" replace="/null, //g"?>
```dart
Scaffold(
  drawer: // Add a Drawer here in the next step.
);
```

## 2. Add a drawer

## 2. 新增一個 drawer

Now add a drawer to the `Scaffold`. A drawer can be any widget,
but it's often best to use the `Drawer` widget from the
[material library][],
which adheres to the Material Design spec.

我們現在可以在 `Scaffold` 上新增一個 drawer。雖然 drawer 可以是任何 widget，
但最好還是使用 [Material Library]({{site.api}}/flutter/material/material-library.html)
中的 `Drawer` widget，因為這樣才符合 Material Design 設計規範。

<?code-excerpt "lib/drawer.dart (DrawerEmpty)" replace="/null, //g"?>
```dart
Scaffold(
  drawer: Drawer(
    child: // Populate the Drawer in the next step.
  ),
);
```

## 3. Populate the drawer with items

## 3. 向 drawer 中新增內容

Now that you have a `Drawer` in place, add content to it.
For this example, use a [`ListView`][].
While you could use a `Column` widget,
`ListView` is handy because it allows users to scroll
through the drawer if the
content takes more space than the screen supports.

既然已經有了一個 `Drawer`，我們現在就可以向其中新增內容。
在這個例子中，我們將使用 [`ListView`][]。
雖然你也可以使用 `Column` widget，但是 `ListView` 在這種情況下將是更好的選擇，
因為如果內容所佔用的空間超出了螢幕的話，它將能夠允許使用者進行滾動。

Populate the `ListView` with a [`DrawerHeader`][]
and two [`ListTile`][] widgets.
For more information on working with Lists,
see the [list recipes][].

我們將使用 [`DrawerHeader`][] 和兩個 [`ListTile`][] widget 填充 `ListView`。
有關使用 List 的更多資訊，請參閱實用課程中的 [list recipes][]。

<?code-excerpt "lib/drawer.dart (DrawerListView)"?>
```dart
Drawer(
  // Add a ListView to the drawer. This ensures the user can scroll
  // through the options in the drawer if there isn't enough vertical
  // space to fit everything.
  child: ListView(
    // Important: Remove any padding from the ListView.
    padding: EdgeInsets.zero,
    children: [
      const DrawerHeader(
        decoration: BoxDecoration(
          color: Colors.blue,
        ),
        child: Text('Drawer Header'),
      ),
      ListTile(
        title: const Text('Item 1'),
        onTap: () {
          // Update the state of the app.
          // ...
        },
      ),
      ListTile(
        title: const Text('Item 2'),
        onTap: () {
          // Update the state of the app.
          // ...
        },
      ),
    ],
  ),
);
```

## 4. Close the drawer programmatically

## 4. 透過程式設計關閉 drawer

After a user taps an item, you might want to close the drawer.
You can do this by using the [`Navigator`][].

我們經常需要在使用者點選某個專案後就將 Drawer 關掉。
那麼怎樣才能做到這一點呢？請試試看 [`Navigator`][]。

When a user opens the drawer, Flutter adds the drawer to the navigation
stack. Therefore, to close the drawer, call `Navigator.pop(context)`.

當用戶開啟 Drawer 時，Flutter 會將 drawer widget 覆蓋在當前的導航堆疊上。
因此，要關閉 drawer，我們可以透過呼叫 `Navigator.pop(context)` 來實現。

<?code-excerpt "lib/main.dart (CloseDrawer)"?>
```dart
ListTile(
  title: const Text('Item 1'),
  onTap: () {
    // Update the state of the app
    // ...
    // Then close the drawer
    Navigator.pop(context);
  },
),
```

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  static const appTitle = 'Drawer Demo';

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: appTitle,
      home: MyHomePage(title: appTitle),
    );
  }
}

class MyHomePage extends StatelessWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(title)),
      body: const Center(
        child: Text('My Page!'),
      ),
      drawer: Drawer(
        // Add a ListView to the drawer. This ensures the user can scroll
        // through the options in the drawer if there isn't enough vertical
        // space to fit everything.
        child: ListView(
          // Important: Remove any padding from the ListView.
          padding: EdgeInsets.zero,
          children: [
            const DrawerHeader(
              decoration: BoxDecoration(
                color: Colors.blue,
              ),
              child: Text('Drawer Header'),
            ),
            ListTile(
              title: const Text('Item 1'),
              onTap: () {
                // Update the state of the app
                // ...
                // Then close the drawer
                Navigator.pop(context);
              },
            ),
            ListTile(
              title: const Text('Item 2'),
              onTap: () {
                // Update the state of the app
                // ...
                // Then close the drawer
                Navigator.pop(context);
              },
            ),
          ],
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/drawer.png" alt="抽屜範例" class="site-mobile-screenshot" />
</noscript>


[`Drawer`]: {{site.api}}/flutter/material/Drawer-class.html
[`DrawerHeader`]: {{site.api}}/flutter/material/DrawerHeader-class.html
[list recipes]: {{site.url}}/cookbook#lists
[`ListTile`]: {{site.api}}/flutter/material/ListTile-class.html
[`ListView`]: {{site.api}}/flutter/widgets/ListView-class.html
[material library]: {{site.api}}/flutter/material/material-library.html
[`Navigator`]: {{site.api}}/flutter/widgets/Navigator-class.html
[`Scaffold`]: {{site.api}}/flutter/material/Scaffold-class.html