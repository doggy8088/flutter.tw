---
title: Update the UI based on orientation
title: 根據螢幕方向更新介面
description: Respond to a change in the screen's orientation.
description: 根據螢幕方向自適應介面。
tags: cookbook, 實用課程, 設計
keywords: 螢幕切換,橫屏模式,豎屏模式,自適應
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/design/orientation"?>

In some situations,
you want to update the display of an app when the user
rotates the screen from portrait mode to landscape mode. For example,
the app might show one item after the next in portrait mode,
yet put those same items side-by-side in landscape mode.

一般情況下，一旦一個應用的螢幕方向發生了改變，比如從橫屏變成豎屏，其設計也將跟著更新。
例如，在縱向模式下，我們可能想要依次顯示各個專案，但在橫向模式下，我們會把這些相同的專案並排放置。

In Flutter, you can build different layouts depending
on a given [`Orientation`][].
In this example, build a list that displays two columns in
portrait mode and three columns in landscape mode using the
following steps:

在 Flutter 中，我們可以根據給定的 [`Orientation`][]
建構不同的佈局。本範例中，我們將建構一個列表，
在縱向模式下顯示兩列，在橫向模式下顯示三列。

  1. Build a `GridView` with two columns.
     
     建立一個列的數量為 2 的 `GridView`；
     
  2. Use an `OrientationBuilder` to change the number of columns.
  
     使用 `OrientationBuilder` 更改列數。

## 1. Build a `GridView` with two columns

## 1. 建立一個列的數量為 2 的 `GridView`

First, create a list of items to work with.
Rather than using a normal list,
create a list that displays items in a grid.
For now, create a grid with two columns.

首先，我們需要一個專案的列表來配合完成。
我們需要使用一個在網格中顯示專案的列表而非一個普通的列表。
現在，我們將建立一個包含兩個列的網格。

<?code-excerpt "lib/partials.dart (GridViewCount)"?>
```dart
return GridView.count(
  // A list with 2 columns
  crossAxisCount: 2,
  // ...
);
```

To learn more about working with `GridViews`,
see the [Creating a grid list][] recipe.

要了解有關使用 `GridViews` 的更多資訊，
請參閱這個課程文件：[建立一個網格列表][Creating a grid list]。

## 2. Use an `OrientationBuilder` to change the number of columns

## 2. 使用 `OrientationBuilder` 更改列數。

To determine the app's current `Orientation`, use the
[`OrientationBuilder`][] widget.
The `OrientationBuilder` calculates the current `Orientation` by
comparing the width and height available to the parent widget,
and rebuilds when the size of the parent changes.

為了確定當前的螢幕方向 `Orientation`，我們可以使用 `OrientationBuilder` widget。
[`OrientationBuilder`][] 透過比較父 widget 的可用寬度和高度
來計算當前的 `Orientation`，
並在父視窗大小更改時重建。

Using the `Orientation`, build a list that displays two columns in portrait
mode, or three columns in landscape mode.

使用 `Orientation`，我們可以建構一個列表，
在縱向模式下顯示兩列，在橫向模式下顯示三列。

<?code-excerpt "lib/partials.dart (OrientationBuilder)"?>
```dart
body: OrientationBuilder(
  builder: (context, orientation) {
    return GridView.count(
      // Create a grid with 2 columns in portrait mode,
      // or 3 columns in landscape mode.
      crossAxisCount: orientation == Orientation.portrait ? 2 : 3,
    );
  },
),
```

{{site.alert.note}}

  If you're interested in the orientation of the screen,
  rather than the amount of space available to the parent,
  use `MediaQuery.of(context).orientation` instead of an
  `OrientationBuilder` widget.
  
  如果你只想知道螢幕的方向，可以直接使用 `MediaQuery.of(context).orientation`，
  而不是使用 `OrientationBuilder` widget.

{{site.alert.end}}

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-500px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    const appTitle = 'Orientation Demo';

    return const MaterialApp(
      title: appTitle,
      home: OrientationList(
        title: appTitle,
      ),
    );
  }
}

class OrientationList extends StatelessWidget {
  final String title;

  const OrientationList({super.key, required this.title});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(title)),
      body: OrientationBuilder(
        builder: (context, orientation) {
          return GridView.count(
            // Create a grid with 2 columns in portrait mode, or 3 columns in
            // landscape mode.
            crossAxisCount: orientation == Orientation.portrait ? 2 : 3,
            // Generate 100 widgets that display their index in the List.
            children: List.generate(100, (index) {
              return Center(
                child: Text(
                  'Item $index',
                  style: Theme.of(context).textTheme.displayLarge,
                ),
              );
            }),
          );
        },
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/orientation.gif" alt="螢幕方向適配範例" class="site-mobile-screenshot" />
</noscript>

## Locking device orientation

In the previous section, you learned 
how to adapt the app UI to device orientation changes.

Flutter also allows you to specify the orientations your app supports 
using the values of [`DeviceOrientation`]. You can either:

- Lock the app to a single orientation, like only the `portraitUp` position, or...
- Allow multiple orientations, like both `portraitUp` and `portraitDown`, but not landscape.

In the application `main()` method,
call [`SystemChrome.setPreferredOrientations()`]
with the list of preferred orientations that your app supports.

To lock the device to a single orientation, 
you can pass a list with a single item.

For a list of all the possible values, check out [`DeviceOrientation`].

<?code-excerpt "lib/orientation.dart (PreferredOrientations)"?>
```dart
void main() {
  WidgetsFlutterBinding.ensureInitialized();
  SystemChrome.setPreferredOrientations([
    DeviceOrientation.portraitUp,
  ]);
  runApp(const MyApp());
}
```


[Creating a grid list]: {{site.url}}/cookbook/lists/grid-lists
[`DeviceOrientation`]: {{site.api}}/flutter/services/DeviceOrientation.html
[`OrientationBuilder`]: {{site.api}}/flutter/widgets/OrientationBuilder-class.html
[`Orientation`]: {{site.api}}/flutter/widgets/Orientation.html
[`SystemChrome.setPreferredOrientations()`]: {{site.api}}/flutter/services/SystemChrome/setPreferredOrientations.html
