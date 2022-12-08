---
title: Export fonts from a package
title: 以 package 的方式使用字型
description: How to export fonts from a package.
description: 如何從 package 中匯出字型。
tags: cookbook, 實用課程, 設計
keywords: Flutter使用字型,字型打包,開放字型,谷歌字型
prev:
  title: Display a snackbar
  title: 顯示 SnackBar
  path: /docs/cookbook/design/snackbars
next:
  title: Update the UI based on orientation
  title: 根據螢幕方向更新介面
  path: /docs/cookbook/design/orientation
---

<?code-excerpt path-base="cookbook/design/package_fonts"?>

Rather than declaring a font as part of an app,
you can declare a font as part of a separate package.
This is a convenient way to share the same font across
several different projects,
or for coders publishing their packages to [pub.dev][].
This recipe uses the following steps:

自訂字型，除了可以把字型檔案作為應用的一部分之外，還可以透過 package 的方式使用，
這樣有助於跨專案的字型共享，也可以更方便的釋出到 [pub.dev][]。

## Directions

## 步驟

  1. Add a font to a package.

     將字型新增到 package
  
  2. Add the package and font to the app.
  
     將 package 和字型新增到應用
  
  3. Use the font.
  
     使用字型

{{site.alert.note}}

  Check out the [google_fonts][] package for direct access
  to almost 1000 open-sourced font families.

  檢視 [google_fonts][] 庫，你將可以直接存取近 1000 個開源字型族。
  
{{site.alert.end}}

## 1. Add a font to a package

## 1. 將字型新增到 package

To export a font from a package, you need to import the font files into the
`lib` folder of the package project. You can place font files directly in the
`lib` folder or in a subdirectory, such as `lib/fonts`.

透過 package 的方式使用字型，需要將字型檔案匯入 package 專案的 `lib` 資料夾中。
你既可以將字型檔案直接放在 `lib` 資料夾中，也可以放在子目錄中，例如 `lib/fonts`。

In this example, assume you've got a Flutter library called
`awesome_package` with fonts living in a `lib/fonts` folder.

在此範例中，假設你已有一個名為 `awesome_package` 的 library，
其中包含了 `lib/fonts` 資料夾中的字型資源。

```
awesome_package/
  lib/
    awesome_package.dart
    fonts/
      Raleway-Regular.ttf
      Raleway-Italic.ttf
```

## 2. Add the package and fonts to the app

## 2. 將 package 和字型新增到應用

Now you can use the fonts in the package by
updating the `pubspec.yaml` in the *app's* root directory.

現在你可以使用該 package 以及它提供的字型。
我們來編輯 *應用程式* 根目錄下的 `pubspec.yaml` 檔案。

### Add the package to the app

### 將 package 新增到應用中

```yaml
dependencies:
  awesome_package: <latest_version>
```

### Declare the font assets

### 宣告字型資源

Now that you've imported the package, tell Flutter where to
find the fonts from the `awesome_package`.

現在已經匯入了 package，你需要告之 Flutter
在 `awesome_package` 中的哪裡可以找到字型檔案。

To declare package fonts, prefix the path to the font with
`packages/awesome_package`.
This tells Flutter to look in the `lib` folder
of the package for the font.

要想宣告 package 中的字型，必須在 `packages/awesome_package` 的路徑前加上字型宣告。
這將會讓 Flutter 檢索到 `lib` package 的資料夾中的字型。

```yaml
flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: packages/awesome_package/fonts/Raleway-Regular.ttf
        - asset: packages/awesome_package/fonts/Raleway-Italic.ttf
          style: italic
```

## 3. Use the font

## 3. 使用字型

Use a [`TextStyle`][] to change the appearance of text.
To use package fonts, declare which font you'd like to use and
which package the font belongs to.

你可以使用 [`TextStyle`][] 來更改文字的外觀。
在使用 package 中的字型時，你不僅需要宣告該文字所要使用的字型，
還需要宣告字型所屬的 `package`。

<?code-excerpt "lib/main.dart (TextStyle)"?>
```dart
child: Text(
  'Using the Raleway font from the awesome_package',
  style: TextStyle(
    fontFamily: 'Raleway',
    package: 'awesome_package',
  ),
),
```

## Complete example

## 完整範例

### Fonts

### 字型

The Raleway and RobotoMono fonts were downloaded from
[Google Fonts][].

這裡所使用的 Raleway 和 RobotoMono 字型都是從 [Google Fonts](https://fonts.google.com/) 下載的 。

### `pubspec.yaml`

```yaml
name: package_fonts
description: An example of how to use package fonts with Flutter

dependencies:
  awesome_package:
  flutter:
    sdk: flutter

dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
  fonts:
    - family: Raleway
      fonts:
        - asset: packages/awesome_package/fonts/Raleway-Regular.ttf
        - asset: packages/awesome_package/fonts/Raleway-Italic.ttf
          style: italic
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
    return const MaterialApp(
      title: 'Package Fonts',
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  const MyHomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // The AppBar uses the app-default font.
      appBar: AppBar(title: const Text('Package Fonts')),
      body: const Center(
        // This Text widget uses the Raleway font.
        child: Text(
          'Using the Raleway font from the awesome_package',
          style: TextStyle(
            fontFamily: 'Raleway',
            package: 'awesome_package',
          ),
        ),
      ),
    );
  }
}
```

![Package Fonts Demo]({{site.url}}/assets/images/docs/cookbook/package-fonts.png){:.site-mobile-screenshot}

[Google Fonts]: https://fonts.google.com
[google_fonts]: {{site.pub-pkg}}/google_fonts
[pub.dev]: {{site.pub}}
[`TextStyle`]: {{site.api}}/flutter/painting/TextStyle-class.html
