---
title: Use themes to share colors and font styles
title: 使用 Themes 統一顏色和字型風格
short-title: Themes
short-title: 主題
description: How to share colors and font styles throughout an app using Themes.
description: 學習如何使用 Themes 統一顏色和字型風格。
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/design/themes"?>

{{site.alert.note}}

  This recipe uses Flutter's support for [Material 3][] and
  the [google_fonts][] package.

  本文內容使用了 Flutter 的 [Material 3][]
  以及 [google_fonts][] package 的支援。

{{site.alert.end}}

[Material 3]: {{site.url}}/ui/design/material
[google_fonts]: {{site.pub-pkg}}/google_fonts

To share colors and font styles throughout an app, use themes.

你可以使用主題來全域應用顏色和文字樣式。

You can define app-wide themes.
You can extend a theme to change a theme style for one component.
Each theme defines the colors, type style, and other parameters
applicable for the type of Material component.

你可以定義應用全域的主題。
你也可以為某一個元件單獨繼承一個特定的主題。
每個主題都可以各自訂顏色、文字樣式和其他 Material 配置引數。

Flutter applies styling in the following order:

Flutter 會按以下順序應用樣式：

1. Styles applied to the specific widget.

   針對特定 widget 的樣式。

1. Themes that override the immediate parent theme.

   重載的繼承主題的樣式。

1. Main theme for the entire app.

   應用的總體樣式。

After you define a `Theme`, use it within your own widgets.
Flutter's Material widgets use your theme to set the background
colors and font styles for app bars, buttons, checkboxes, and more.

在定義一個 `Theme` 之後，我們可以讓它在指定的 widgets，
包括 Flutter 自帶的 Material widgets，例如
AppBars、Buttons、Checkboxes 等 widget 中生效。

## Create an app theme

To share a `Theme` across your entire app, set the `theme` property
to your `MaterialApp` constructor.
This property takes a [`ThemeData`][] instance.

全域 Theme 會影響整個 app 的顏色和字型樣式。
只需要向 `MaterialApp` 構造器傳入 [`ThemeData`][] 即可。

To enable Material 3, set the [`useMaterial3`][] property
to `true` in the `ThemeData` constructor.

要使用 Material 3，設定 `ThemeData` 中的
[`useMaterial3`][] 屬性為 `true` 即可。

If you don't specify a theme in the constructor,
Flutter creates a default theme for you.

如果沒有手動配置主題，Flutter 將會使用預設的樣式。

[`useMaterial3`]: {{site.api}}/flutter/material/ThemeData/useMaterial3.html

<?code-excerpt "lib/main.dart (MaterialApp)" replace="/return //g"?>
```dart
MaterialApp(
  title: appName,
  theme: ThemeData(
    useMaterial3: true,

    // Define the default brightness and colors.
    colorScheme: ColorScheme.fromSeed(
      seedColor: Colors.purple,
      // ···
      brightness: Brightness.dark,
    ),

    // Define the default `TextTheme`. Use this to specify the default
    // text styling for headlines, titles, bodies of text, and more.
    textTheme: TextTheme(
      displayLarge: const TextStyle(
        fontSize: 72,
        fontWeight: FontWeight.bold,
      ),
      // ···
      titleLarge: GoogleFonts.oswald(
        fontSize: 30,
        fontStyle: FontStyle.italic,
      ),
      bodyMedium: GoogleFonts.merriweather(),
      displaySmall: GoogleFonts.pacifico(),
    ),
  ),
  home: const MyHomePage(
    title: appName,
  ),
);
```

Most instances of `ThemeData` set values for the following two properties. These properties affect the entire app.

大部分 `ThemeData` 例項會設定以下兩個屬性。它們會影響大部分樣式屬性。

1. [`colorScheme`][] defines the colors.

   [`colorScheme`][] 定義了顏色。

1. [`textTheme`][] defines text styling.

   [`textTheme`][] 定義了文字樣式。

[`colorScheme`]: {{site.api}}/flutter/material/ThemeData/colorScheme.html
[`textTheme`]: {{site.api}}/flutter/material/ThemeData/textTheme.html

To learn what colors, fonts, and other properties, you can define,
check out the [`ThemeData`][] documentation.

你可以在 [`ThemeData`][] 文件中檢視所有可自訂的顏色和字型樣式。

## Apply a theme

## 應用指定的主題

To apply your new theme, use the `Theme.of(context)` method 
when specifying a widget's styling properties.
These can include, but are not limited to, `style` and `color`.

要想應用你的主題，使用 `Theme.of(context)` 方法來指定 widget 的樣式屬性。
其包括但不限於樣式和顏色。

The `Theme.of(context)` method looks up the widget tree and retrieves
the nearest `Theme` in the tree.
If you have a standalone `Theme`, that's applied.
If not, Flutter applies the app's theme.

`Theme.of(context)` 會查詢 widget 樹，並返回其中最近的 `Theme`。
所以他會優先返回我們之前定義過的一個獨立的 `Theme`，
如果找不到，它會返回全域主題。

In the following example, the `Container` constructor uses this technique to set its `color`.

在下面的例子中，`Container` 的顏色使用的就是指定主題（上層）的顏色。

<?code-excerpt "lib/main.dart (Container)" replace="/^child: //g"?>
```dart
Container(
  padding: const EdgeInsets.symmetric(
    horizontal: 12,
    vertical: 12,
  ),
  color: Theme.of(context).colorScheme.primary,
  child: Text(
    'Text with a background color',
    // ···
    style: Theme.of(context).textTheme.bodyMedium!.copyWith(
          color: Theme.of(context).colorScheme.onPrimary,
        ),
  ),
),
```

## Override a theme

To override the overall theme in part of an app,
wrap that section of the app in a `Theme` widget.

你可以用 `Theme` widget 巢狀(Nesting)想要改變主題的部分以進行主題重載。

You can override a theme in two ways:

以下是兩種重載主題的方法：

1. Create a unique `ThemeData` instance.

   構造一個不一樣的 `ThemeData` 例項。

2. Extend the parent theme.

   繼承上層主題。

### Set a unique `ThemeData` instance

If you want a component of your app to ignore the overall theme,
create a `ThemeData` instance.
Pass that instance to the `Theme` widget.

如果不想從任何全域 Theme 繼承樣式，
我們可以建立一個 `ThemeData()` 例項，
然後把它傳給 `Theme` widget：

<?code-excerpt "lib/main.dart (Theme)"?>
```dart
Theme(
  // Create a unique theme with `ThemeData`.
  data: ThemeData(
    colorScheme: ColorScheme.fromSeed(
      seedColor: Colors.pink,
    ),
  ),
  child: FloatingActionButton(
    onPressed: () {},
    child: const Icon(Icons.add),
  ),
);
```

### Extend the parent theme

Instead of overriding everything, consider extending the parent theme.
To extend a theme, use the [`copyWith()`][] method.

相比從頭開始定義一套樣式，從上層 Theme 擴充可能更常規一些，
使用 [`copyWith()`][] 方法即可。

<?code-excerpt "lib/main.dart (ThemeCopyWith)"?>
```dart
Theme(
  // Find and extend the parent theme using `copyWith`.
  // To learn more, check out the section on `Theme.of`.
  data: Theme.of(context).copyWith(
    colorScheme: ColorScheme.fromSeed(
      seedColor: Colors.pink,
    ),
  ),
  child: const FloatingActionButton(
    onPressed: null,
    child: Icon(Icons.add),
  ),
);
```

## Watch a video on `Theme`

## 觀看 `Theme` 的相關影片

To learn more, watch this short Widget of the Week video on the `Theme` widget:

想要了解更多，你可以觀看 Widget of the Week 中關於 `Theme` 的短影片：

<iframe class="full-width" src="{{site.youtube-site}}/embed/oTvQDJOBXmM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Try an interactive example

## 互動式範例

<?code-excerpt "lib/main.dart (FullApp)"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';
// Include the Google Fonts package to provide more text format options
// https://pub.dev/packages/google_fonts
import 'package:google_fonts/google_fonts.dart';

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
        useMaterial3: true,

        // Define the default brightness and colors.
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.purple,
          // TRY THIS: Change to "Brightness.light"
          //           and see that all colors change
          //           to better contrast a light background.
          brightness: Brightness.dark,
        ),

        // Define the default `TextTheme`. Use this to specify the default
        // text styling for headlines, titles, bodies of text, and more.
        textTheme: TextTheme(
          displayLarge: const TextStyle(
            fontSize: 72,
            fontWeight: FontWeight.bold,
          ),
          // TRY THIS: Change one of the GoogleFonts
          //           to "lato", "poppins", or "lora".
          //           The title uses "titleLarge"
          //           and the middle text uses "bodyMedium".
          titleLarge: GoogleFonts.oswald(
            fontSize: 30,
            fontStyle: FontStyle.italic,
          ),
          bodyMedium: GoogleFonts.merriweather(),
          displaySmall: GoogleFonts.pacifico(),
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
        title: Text(title,
            style: Theme.of(context).textTheme.titleLarge!.copyWith(
                  color: Theme.of(context).colorScheme.onSecondary,
                )),
        backgroundColor: Theme.of(context).colorScheme.secondary,
      ),
      body: Center(
        child: Container(
          padding: const EdgeInsets.symmetric(
            horizontal: 12,
            vertical: 12,
          ),
          color: Theme.of(context).colorScheme.primary,
          child: Text(
            'Text with a background color',
            // TRY THIS: Change the Text value
            //           or change the Theme.of(context).textTheme
            //           to "displayLarge" or "displaySmall".
            style: Theme.of(context).textTheme.bodyMedium!.copyWith(
                  color: Theme.of(context).colorScheme.onPrimary,
                ),
          ),
        ),
      ),
      floatingActionButton: Theme(
        data: Theme.of(context).copyWith(
          // TRY THIS: Change the seedColor to "Colors.red" or
          //           "Colors.blue".
          colorScheme: ColorScheme.fromSeed(
            seedColor: Colors.pink,
            brightness: Brightness.dark,
          ),
        ),
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
[`Theme`]: {{site.api}}/flutter/material/Theme-class.html
