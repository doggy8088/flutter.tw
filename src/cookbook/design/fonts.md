---
title: Use a custom font
title: 使用自訂字型
description: How to use custom fonts.
description: 學習如何在 Flutter 裡使用自訂字型
tags: cookbook, 實用課程, 設計
keywords: Material Design 效果, 字型, 自訂字型, Flutter使用谷歌字型
---

<?code-excerpt path-base="cookbook/design/fonts/"?>

Although Android and iOS offer high quality system fonts,
one of the most common requests from designers is for custom fonts.
For example, you might have a custom-built font from a designer,
or perhaps you downloaded a font from [Google Fonts][].

儘管 Android 和 iOS 已經提供了一套高品質系統字型，
然而通常設計師還是會要求使用自訂字型。
例如，你可能需要使用設計師提供的自訂字型，或者從
[Google Fonts][] 下載的字型。

{{site.alert.note}}

  Check out the [google_fonts][] package for direct access
  to over 1,000 open-sourced font families.

  檢視 [google_fonts][] 庫，
  你將可以直接存取 1000 個以上的開源字型。

  <iframe class="full-width" src="{{site.youtube-site}}/embed/8Vzv2CdbEY0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{{site.alert.end}}

{{site.alert.note}}

  For another approach to using custom fonts, 
  especially if you want to re-use one font over multiple projects, 
  see [Export fonts from a package][].

  這裡還有另一個關於使用自訂字型的課程，
  如果你想要在多專案中重用一份字型的情況下，
  請參考 [將字型匯出到 package][Export fonts from a package]。

{{site.alert.end}}

Flutter works with custom fonts and you can apply a custom
font across an entire app or to individual widgets.
This recipe creates an app that uses custom fonts with
the following steps:

Flutter 可以很方便的使用自訂字型，不僅能夠將其用於整個應用裡，
還可以用在某個單獨的 widget 中。
請參照下面的步驟使用自訂字型：

  1. Import the font files.

     匯入字型檔案。

  2. Declare the font in the pubspec.

     在 `pubspec.yaml` 中宣告字型。

  3. Set a font as the default.

     設定預設字型。

  4. Use a font in a specific widget.
  
     將字型用於特定 widget。

## 1. Import the font files

## 1. 匯入字型檔案

To work with a font, import the font files into the project.
It's common practice to put font files in a `fonts` or `assets`
folder at the root of a Flutter project.

要使用字型，你需要將字型檔案匯入到專案中。
常見的做法是將字型檔案放在專案根目錄下的 `fonts` 或者 `assets` 資料夾中。

For example, to import the Raleway and Roboto Mono font
files into a project, the folder structure might look like this:

例如，如果你想要在專案中匯入 Raleway 和 Roboto Mono 字型，
資料夾結構會像下面這樣：

```
awesome_app/
  fonts/
    Raleway-Regular.ttf
    Raleway-Italic.ttf
    RobotoMono-Regular.ttf
    RobotoMono-Bold.ttf
```

### Supported font formats

### 已支援的字型格式

Flutter supports the following font formats:

Flutter 支援以下的字型格式：

* `.ttc`
* `.ttf`
* `.otf`

Flutter does not support `.woff` and `.woff2` fonts for all platforms.

Flutter 在所有平臺上均尚未支援 `.woff` 和 `.woff2` 字型。

## 2. Declare the font in the pubspec

## 2. 在 `pubspec.yaml` 中宣告字型

Once you've identified a font, tell Flutter where to find it.
You can do this by including a font definition in the `pubspec.yaml` file.

現在你已經有一個字型可以使用，接下來則需要告訴 Flutter 它在哪。
你可以在 `pubspec.yaml` 中像下面這樣宣告：

```yaml
flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: fonts/Raleway-Regular.ttf
        - asset: fonts/Raleway-Italic.ttf
          style: italic
    - family: RobotoMono
      fonts:
        - asset: fonts/RobotoMono-Regular.ttf
        - asset: fonts/RobotoMono-Bold.ttf
          weight: 700
```

### `pubspec.yaml` option definitions

### `pubspec.yaml` 選項的定義

The `family` determines the name of the font, which you use in the
[`fontFamily`][] property of a [`TextStyle`][] object.

`family` 屬性決定了字型的名稱，
你將會在 [`TextStyle`][] 的 [`fontFamily`][] 屬性中用到。

The `asset` is a path to the font file,
relative to the `pubspec.yaml` file.
These files contain the outlines for the glyphs in the font.
When building the app,
these files are included in the app's asset bundle.

`asset` 是字型檔案對於 `pubspec.yaml` 檔案的相對路徑。
這些檔案包含了字型中字形的輪廓。
建構應用時，這些檔案將會被包含在應用程式的資源套件中。

A single font can reference many different files with different
outline weights and styles:

單個字型可以參考多個不同輪廓字重及風格的檔案：

  * The `weight` property specifies the weight of the outlines in
    the file as an integer multiple of 100, between 100 and 900.
    These values correspond to the [`FontWeight`][] and can be used in the
    [`fontWeight`][fontWeight property] property of a [`TextStyle`][] object. 
    For example, if you want to use the `RobotoMono-Bold` font defined above, 
    you would set `fontWeight` to `FontWeight.w700` in your `TextStyle`.

    `weight` 屬性指定了檔案中字型輪廓的字重為 100 的整數倍，
    並且範圍在 100 和 900 之間。
    這些值對應 [`FontWeight`][fontWeight property] 並能夠在 [`TextStyle`][] 
    物件的 [`fontWeight`][] 屬性上使用。
    例如，如果你想使用上面定義的 `RobotoMono-Bold` 字型，
    你可以在 `TextStyle` 中將 `fontWeight` 設定為 `FontWeight.w700`。
    
    Note that defining the `weight` property does not
    override the actual weight of the font. You would not be able to
    access `RobotoMono-Bold` with `FontWeight.w100`, even if its `weight`
    was set to 100.

    需要注意的是，定義 `weight` 屬性不會覆蓋字型的實際粗細。
    你無法使用 `FontWeight.w100` 存取 `RobotoMono-Bold`，
    即使其 `weight` 設定為了 100。

  * The `style` property specifies whether the outlines in the file are
    `italic` or `normal`. 
    These values correspond to the [`FontStyle`][] and can be used in the
    [`fontStyle`][fontStyle property] property of a [`TextStyle`][] object. 
    For example, if you want to use the `Raleway-Italic` font defined above, 
    you would set `fontStyle` to `FontStyle.italic` in your `TextStyle`.

    `style` 屬性指定檔案中字型的輪廓是否為 `italic` 或 `normal`。
    這些值對應 [`FontStyle`][fontStyle property] 並能夠在 [`TextStyle`][]
    物件的 [`fontStyle`][] 屬性上使用。例如，如果你想使用上面定義的 
    `Raleway-Italic` 字型，你可以在 `TextStyle` 中
    將 `fontStyle` 設定為 `FontStyle.italic`。
    
    Note that defining the `style` property does not
    override the actual style of the font; You would not be able to
    access `Raleway-Italic` with `FontStyle.normal`, even if its `style`
    was set to normal.

    需要注意的是，定義 `style` 屬性不會更改字型的實際樣式；
    你無法使用 `FontStyle.normal` 存取 `Raleway-Italic，
    即使其 `style` 設定為了 normal。

## 3. Set a font as the default

## 3. 設定預設字型

You have two options for how to apply fonts to text: as the default font
or only within specific widgets.

關於如何應用這些字型，你有兩種選擇：
將其設為預設字型，或者僅在某些特定 widget 中使用。

To use a font as the default, set the `fontFamily` property
as part of the app's `theme`. The value provided to
`fontFamily` must match the `family`
name declared in the `pubspec.yaml`.

如果你想要設為預設字型，
請將 `fontFamily` 設為應用（全域）`theme` 的屬性的一部分。
提供的 `fontFamily` 的值必須與 `pubspec.yaml` 中宣告的名稱相匹配。

<?code-excerpt "lib/main.dart (MaterialApp)"?>
```dart
return MaterialApp(
  title: 'Custom Fonts',
  // Set Raleway as the default app font.
  theme: ThemeData(fontFamily: 'Raleway'),
  home: const MyHomePage(),
);
```

For more information on themes,
see the [Using Themes to share colors and font styles][] recipe.

有關主題的更多資訊，請參閱文件：
[使用 Themes 統一顏色和字型風格][Using Themes to share colors and font styles]。

## 4. Use the font in a specific widget

## 4. 將字型用於特定 Widget

If you want to apply the font to a specific widget,
such as a `Text` widget,
provide a [`TextStyle`][] to the widget.

如果你希望在特定 Widget（例如 `Text` Widget）中使用該字型，可以透過
[`TextStyle`][] 中進行指定。

In this example, apply the RobotoMono font to a single `Text` widget.
Once again, the `fontFamily` must match the `family` name declared in the
`pubspec.yaml`.

在這個例子中，我們將在一個 `Text` Widget 上使用 RobotoMono 字型。
同樣的，這裡的 fontFamily 的值必須與 pubspec.yaml 中宣告的值相匹配。

<?code-excerpt "lib/main.dart (Text)"?>
```dart
child: Text(
  'Roboto Mono sample',
  style: TextStyle(fontFamily: 'RobotoMono'),
),
```

### TextStyle

### 字型樣式

If a [`TextStyle`][] object specifies a weight
or style for which there is no exact font file,
the engine uses one of the more generic files
for the font and attempts to extrapolate outlines
for the requested weight and style.

如若 [`TextStyle`][] 指定的字型樣式缺少相應的字型檔案，
Engine 則會使用一個更加通用的字型檔案，
並嘗試推斷所請求的字型 weight 和樣式的輪廓。

## Complete example

## 完整範例

### Fonts

### 字型

The Raleway and RobotoMono fonts were downloaded from
[Google Fonts][].

Raleway 和 RobotoMono 字型是從 [Google Fonts][] 下載的。

### `pubspec.yaml`

```yaml
name: custom_fonts
description: An example of how to use custom fonts with Flutter

dependencies:
  flutter:
    sdk: flutter

dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: fonts/Raleway-Regular.ttf
        - asset: fonts/Raleway-Italic.ttf
          style: italic
    - family: RobotoMono
      fonts:
        - asset: fonts/RobotoMono-Regular.ttf
        - asset: fonts/RobotoMono-Bold.ttf
          weight: 700
  uses-material-design: true
```

### `main.dart`

<?code-excerpt "lib/main.dart"?>
```dart
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Custom Fonts',
      // Set Raleway as the default app font.
      theme: ThemeData(fontFamily: 'Raleway'),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  const MyHomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // The AppBar uses the app-default Raleway font.
      appBar: AppBar(title: const Text('Custom Fonts')),
      body: const Center(
        // This Text widget uses the RobotoMono font.
        child: Text(
          'Roboto Mono sample',
          style: TextStyle(fontFamily: 'RobotoMono'),
        ),
      ),
    );
  }
}
```

![Custom Fonts Demo]({{site.url}}/assets/images/docs/cookbook/fonts.png){:.site-mobile-screenshot}


[Export fonts from a package]: {{site.url}}/cookbook/design/package-fonts
[`fontFamily`]: {{site.api}}/flutter/painting/TextStyle/fontFamily.html
[fontStyle property]: {{site.api}}/flutter/painting/TextStyle/fontStyle.html
[`FontStyle`]: {{site.api}}/flutter/dart-ui/FontStyle.html
[fontWeight property]: {{site.api}}/flutter/painting/TextStyle/fontWeight.html
[`FontWeight`]: {{site.api}}/flutter/dart-ui/FontWeight-class.html
[Google Fonts]: https://fonts.google.com
[google_fonts]: {{site.pub-pkg}}/google_fonts
[`TextStyle`]: {{site.api}}/flutter/painting/TextStyle-class.html
[Using Themes to share colors and font styles]: {{site.url}}/cookbook/design/themes
