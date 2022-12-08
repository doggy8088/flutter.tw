---
title: Using Themes to share colors and font styles
title: 使用 Themes 統一顏色和字型風格
short-title: Themes
short-title: Themes
description: How to share colors and font styles throughout an app using Themes.
description: 學習如何使用 Themes 統一顏色和字型風格。
tags: cookbook, 實用課程, 設計
keywords: Material Design 效果, Theme, 主題, 全域主題, 自訂
prev:
  title: Update the UI based on orientation
  title: 根據螢幕方向更新介面
  path: /docs/cookbook/design/orientation
next:
  title: Use custom fonts
  title: 使用自訂字型
  path: /docs/cookbook/design/fonts
next:
  title: Work with tabs
  title: 使用 tabs
  path: /docs/cookbook/design/tabs
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/design/themes"?>

To share colors and font styles throughout an app, use themes.
You can either define app-wide themes, or use `Theme` widgets
that define the colors and font styles for a particular part
of the application. In fact,
app-wide themes are just `Theme` widgets created at
the root of an app by the `MaterialApp`.

透過定義 `Theme`，我們可以更好地複用顏色和字型樣式，
從而讓整個 app 的設計看起來更一致。
全域 Theme 會在整個 app 範圍內生效，而區域性 Theme 只作用於特定元素。
其實所謂的全域 Theme 和區域性 Theme 的區別只在於，
全域 Theme 定義在了 app 的 root 處而已。
而 `MaterialApp` 已經事先為你預設了一個全域的 `Theme` Widget。

After defining a Theme, use it within your own widgets. Flutter's
Material widgets also use your Theme to set the background
colors and font styles for AppBars, Buttons, Checkboxes, and more.

在定義一個 `Theme` 之後，我們可以讓它在指定的 widgets
（包括 Flutter 自帶的 Material widgets，
例如 AppBars、Buttons、Checkboxes 等等）中生效。

## Creating an app theme

## 定義一個全域 theme

To share a Theme across an entire app, provide a
[`ThemeData`][] to the `MaterialApp` constructor.

全域 Theme 會影響整個 app 的顏色和字型樣式。
只需要向 `MaterialApp` 構造器傳入 [`ThemeData`][] 即可。

If no `theme` is provided, Flutter creates a default theme for you.

如果沒有放置 `Theme`，Flutter 將會使用預設的樣式。

<?code-excerpt "lib/main.dart (MaterialApp)" replace="/return //g"?>
```dart
MaterialApp(
  title: appName,
  theme: ThemeData(
    // Define the default brightness and colors.
    brightness: Brightness.dark,
    primaryColor: Colors.lightBlue[800],

    // Define the default font family.
    fontFamily: 'Georgia',

    // Define the default `TextTheme`. Use this to specify the default
    // text styling for headlines, titles, bodies of text, and more.
    textTheme: const TextTheme(
      headline1: TextStyle(fontSize: 72.0, fontWeight: FontWeight.bold),
      headline6: TextStyle(fontSize: 36.0, fontStyle: FontStyle.italic),
      bodyText2: TextStyle(fontSize: 14.0, fontFamily: 'Hind'),
    ),
  ),
  home: const MyHomePage(
    title: appName,
  ),
);
```

See the [`ThemeData`][] documentation to see all of
the colors and fonts you can define.

在 [ThemeData]({{site.api}}/flutter/material/ThemeData-class.html) 檢視所有可自訂的顏色和字型樣式。

## Themes for part of an application

## 定義一個區域性 Theme

To override the app-wide theme in part of an application,
wrap a section of the app in a `Theme` widget.

如果我們只想對區域性進行樣式修改，可以建立一個 `Theme` Widget。

There are two ways to approach this: creating a unique `ThemeData`,
or extending the parent theme.

有以下兩種方式：定義一個獨立的 `ThemeData`，或者從父級 Theme 擴充。
下面為你分別介紹。

{{site.alert.note}}

  To learn more, watch this short Widget of the Week video on the Theme widget:

  瞭解更多，請參考下方「每週 Widget」的裡關於 Theme 的短影片：

  <iframe class="full-width" src="{{site.youtube-site}}/embed/oTvQDJOBXmM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{{site.alert.end}}

### Creating unique `ThemeData`

### 定義一個獨立的 `ThemeData`

If you don't want to inherit any application colors or font styles,
create a `ThemeData()` instance and pass that to the `Theme` widget.

如果不想從任何全域 Theme 繼承樣式，
我們可以建立一個 `ThemeData()` 例項，然後把它傳給 `Theme` widget：

<?code-excerpt "lib/theme.dart (Theme)"?>
```dart
Theme(
  // Create a unique theme with `ThemeData`
  data: ThemeData(
    splashColor: Colors.yellow,
  ),
  child: FloatingActionButton(
    onPressed: () {},
    child: const Icon(Icons.add),
  ),
);
```

### Extending the parent theme

### 從父級 Theme 擴充

Rather than overriding everything, it often makes sense to extend the parent
theme. You can handle this by using the [`copyWith()`][] method.

相比從頭開始定義一套樣式，從父級 Theme 擴充可能更常規一些，使用 
[`copyWith()`][] 方法即可。

<?code-excerpt "lib/theme.dart (ThemeCopyWith)"?>
```dart
Theme(
  // Find and extend the parent theme using `copyWith`. See the next
  // section for more info on `Theme.of`.
  data: Theme.of(context).copyWith(splashColor: Colors.yellow),
  child: const FloatingActionButton(
    onPressed: null,
    child: Icon(Icons.add),
  ),
);
```

## Using a Theme

## 使用定義好的 Theme

Now that you've defined a theme, use it within the widgets' `build()`
methods by using the `Theme.of(context)` method.

現在我們定義好了一個 theme，接下來我們該使用它了！
在我們 widget 的 `build` 方法中呼叫 `Theme.of(context)` 函式，
可以讓這些主題樣式生效。

The `Theme.of(context)` method looks up the widget tree and returns
the nearest `Theme` in the tree. If you have a standalone
`Theme` defined above your widget, that's returned.
If not, the app's theme is returned.

`Theme.of(context)` 會查詢 widget 樹，並返回其中最近的 `Theme`。
所以他會優先返回我們之前定義過的一個獨立的 `Theme`，
如果找不到，它會返回全域 theme。

In fact, the `FloatingActionButton` uses this technique to find the
`accentColor`.

實際上，`FloatingActionButton` 就是使用這種方式來定義自己的 `accentColor` 的。

<?code-excerpt "lib/main.dart (Container)" replace="/^child: //g"?>
```dart
Container(
  color: Theme.of(context).colorScheme.secondary,
  child: Text(
    'Text with a background color',
    style: Theme.of(context).textTheme.headline6,
  ),
),
```

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    const appName = 'Custom Themes';

    return MaterialApp(
      title: appName,
      theme: ThemeData(
        // Define the default brightness and colors.
        brightness: Brightness.dark,
        primaryColor: Colors.lightBlue[800],

        // Define the default font family.
        fontFamily: 'Georgia',

        // Define the default `TextTheme`. Use this to specify the default
        // text styling for headlines, titles, bodies of text, and more.
        textTheme: const TextTheme(
          headline1: TextStyle(fontSize: 72.0, fontWeight: FontWeight.bold),
          headline6: TextStyle(fontSize: 36.0, fontStyle: FontStyle.italic),
          bodyText2: TextStyle(fontSize: 14.0, fontFamily: 'Hind'),
        ),
      ),
      home: const MyHomePage(
        title: appName,
      ),
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
      body: Center(
        child: Container(
          color: Theme.of(context).colorScheme.secondary,
          child: Text(
            'Text with a background color',
            style: Theme.of(context).textTheme.headline6,
          ),
        ),
      ),
      floatingActionButton: Theme(
        data: Theme.of(context).copyWith(splashColor: Colors.yellow),
        child: FloatingActionButton(
          onPressed: () {},
          child: const Icon(Icons.add),
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/themes.png" alt="Themes Demo" class="site-mobile-screenshot" />
</noscript>


[`copyWith()`]: {{site.api}}/flutter/material/ThemeData/copyWith.html
[`ThemeData`]: {{site.api}}/flutter/material/ThemeData-class.html
