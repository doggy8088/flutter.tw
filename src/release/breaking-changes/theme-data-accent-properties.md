---
title: ThemeData's accent properties have been deprecated
title: ThemeData 的 accent 屬性已經被棄用
description: The ThemeData accentColor, accentColorBrightness, accentIconTheme, and accentTextTheme properties have been deprecated.
description: ThemeData 的 accentColor、accentColorBrightness、accentIconTheme 和 accentTextTheme 屬性已經被棄用。
---

## Summary

## 概述

The ThemeData [accentColor][], [accentColorBrightness][], [accentIconTheme][] and
[accentTextTheme][] properties have been deprecated.

主題資訊 ThemeData 中的 [accentColor][]、[accentColorBrightness][]、[accentIconTheme][] 和
[accentTextTheme][] 屬性現已被棄用。 

The [Material Design spec][] no longer specifies or uses an "accent"
color for the Material components. The default values for component
colors are derived from the overall theme's [color scheme][]. The
`ColorScheme`'s [secondary color][] is now typically used instead of
`accentColor` and the [onSecondary color][] is used when a contrasting
color is needed.

[Material 設計規範][Material Design spec] 不再為 Material 元件指定或使用「強調」色。
元件的預設顏色來自整個主題的 [顏色方案][color scheme]。
現在通常使用 `ColorScheme` 的 [secondary color][] 代替 `accentColor`，
並且在需要對比色時使用 [onSecondary 屬性][onSecondary color]。

## Context

## 上下文

This was a small part of the [Material Theme System Updates][] project.

這是 [Material 主題系統升級][Material Theme System Updates] 專案的一部分。

As of Flutter 1.17, the ThemeData accent properties - accentColor,
accentColorBrightness, accentIconTheme, and accentTextTheme - were no
longer used by the Material library. They had been replaced by
dependencies on the theme's [`colorScheme`][] and [`textTheme`][] properties as
part of the long-term goal of making the default configurations of the
material components depend almost exclusively on these two
properties.

從 Flutter 1.17 開始，Material 庫不再使用 ThemeData 的強調屬性 - 
`accentColor`、`accentColorBrightness`、`accentIconTheme` 和 `accentTextTheme`。 
它們已被主題的 [`colorScheme`][] 和 [`textTheme`][] 屬性的依賴關係所取代。
這是 material 元件的預設配置完全依賴這兩個屬性的長期目標中的一部分，

The motivation for these changes is to make the theme system easier to
understand and use. The default colors for all components are to be
defined by the components themselves and based on the color
scheme. The defaults for specific component types can be overridden
with component-specific themes like [`FloatingActionButtonThemeData`][] or
[`CheckBoxTheme`][]. Previously, properties like accentColor were used by a
handful of component types and only in some situations, which made it
difficult to understand the implications of overriding them.

改動的出發點是使主題系統更易於理解和使用。所有元件的預設顏色由元件本身根據顏色方案定義。
特定元件型別的預設配置可以使用特定元件的主題來覆蓋，
例如 [`FloatingActionButtonThemeData`][] 或 [`CheckBoxTheme`][]。
以前，像 `accentColor` 這樣的屬性僅在某些情況下被少陣列件型別使用，
這使得很難理解覆蓋它們的含義。

## Description of change

## 更改描述

The ThemeData accentColor, accentColorBrightness, accentIconTheme and
accentTextTheme properties have been deprecated because the Material
library no longer uses them.

主題資訊 ThemeData 中的 [accentColor][]、[accentColorBrightness][]、[accentIconTheme][] 和
[accentTextTheme][] 屬性現已被棄用，因為 Material 不再使用它們。

## Migration guide

## 遷移指南

### Application theme

### 應用程式主題

[`ThemeData`][] values no longer need to specify accentColor,
accentColorBrightness, accentIconTheme, or accentTextTheme.

[`ThemeData`][] 不再需要指定 accentColor、accentColorBrightness、
accentIconTheme 或 accentTextTheme 屬性。

To configure the appearance of the material components in about the
same way as before, specify the color scheme's secondary color
instead of accentColor.

要以與以前大致相同的方式配置 material 元件的樣式，
請指定配色方案的次要顏色，而不是 `accentColor`。

Code before migration:

遷移前的程式碼：

<!-- skip -->
```dart
MaterialApp(
  theme: ThemeData(accentColor: myColor),
  // ...
);
```

Code after migration:

遷移後的程式碼：

<!-- skip -->
```dart
final ThemeData theme = ThemeData();
MaterialApp(
  theme: theme.copyWith(
    colorScheme: theme.colorScheme.copyWith(secondary: myColor),
  ),
  //...
)
```

### `accentColor`

The closest backwards compatible [`ColorScheme`][] color is
[`ColorScheme.secondary`][]. To hew most closely to the latest Material
Design guidelines one can substitute ColorScheme.primary instead.
If a contrasting color is needed then use [`ColorScheme.onSecondary`][].

最接近 [`ColorScheme`][] 的顏色是 [`ColorScheme.secondary`][]。
為了跟上最新的 Material 設計指南，可以用 `ColorScheme.primary` 代替。
如果需要對比色，使用 [`ColorScheme.onSecondary`][]。

Custom components that used to look up the theme's accentColor, can look up
the `ColorScheme.secondary` instead.

自訂元件中查詢主題的 `accentColor` 屬性可以改為查詢 `ColorScheme.secondary`。

Code before migration:

遷移前的程式碼：

<!-- skip -->
```dart
Color myColor = Theme.of(context).accentColor;
```

Code after migration:

遷移後的程式碼：

<!-- skip -->
```dart
Color myColor = Theme.of(context).colorScheme.secondary;
```

### `accentColorBrightness`

The static [`ThemeData.estimateBrightnessForColor()`][] method can be used
to compute the brightness of any color.

靜態方法 [`ThemeData.estimateBrightnessForColor()`][] 可用於計算任何顏色的亮度。

### `accentTextTheme`

This was white [`TextStyle`]s for dark themes, black
TextStyles for light themes. In most cases textTheme can be used
instead. A common idiom was to refer to one TextStyle from
accentTextTheme, since the text style's color was guaranteed to contrast
well with the accent color (now `ColorScheme.secondaryColor`).  To get
the same result now, specify the text style's color as
`ColorScheme.onSecondary`:

此屬性在深色主題中代表白色的 [`TextStyle`]，黑色主題中代表淺色的 TextStyles。
大多數情況下， 可以使用 `textTheme`。
一個常見的用法是參考 `accentTextTheme` 中的 `TextStyle`，
因為文字樣式的顏色要與強調顏色（現在是 `ColorScheme.secondaryColor`）形成鮮明的對比。
現在要獲得相同的結果，請將文字樣式的顏色指定為 `ColorScheme.secondaryColor`：

Code before migration:

遷移前的程式碼：

<!-- skip -->
```dart
TextStyle style = Theme.of(context).accentTextTheme.headline1;
```

Code after migration:

遷移後的程式碼：

<!-- skip -->
```dart
final ThemeData theme = Theme.of(context);
TextStyle style = theme.textTheme.headline1.copyWith(
  color: theme.colorScheme.onSecondary,
)
```

### `accentIconTheme`

This property had only been used to configure the color of icons
within a [`FloatingActionButton`][]. It's now possible to configure the icon
color directly or with the [`FloatingActionButtonThemeData`][]. See
[FloatingActionButton and ThemeData's accent properties][].

此屬性僅用於配置 [`FloatingActionButton`][] 中圖示的顏色。
現在可以直接使用 [`FloatingActionButtonThemeData`][] 配置圖示顏色。
參閱 [FloatingActionButton 和 ThemeData 的強調屬性][FloatingActionButton and ThemeData's accent properties]。

## Timeline

## 時間軸

Landed in version: 2.3.0-0.1.pre<br>
In stable release: 2.5

釋出於版本：2.3.0-0.1.pre<br>
釋出於穩定版本：2.5

## References

## 參考文獻

API documentation:

API 文件：

* [`ColorScheme`][]
* [`FloatingActionButton`][]
* [`FloatingActionButtonThemeData`][]
* [`TextStyle`][]
* [`TextTheme`][]
* [`Theme`][]
* [`ThemeData`][]

Relevant issues:

相關 issues：

* [Issue #56918][]

Relevant PRs:

相關 PR：

* [PR #81336][]

Other:

其他：

* [Material Theme System Updates][]


[accentColor]: {{site.api}}/flutter/material/ThemeData/accentColor.html
[accentColorBrightness]: {{site.api}}/flutter/material/ThemeData/accentColorBrightness.html
[accentIconTheme]: {{site.api}}/flutter/material/ThemeData/accentIconTheme.html
[accentTextTheme]: {{site.api}}/flutter/material/ThemeData/accentTextTheme.html
[`CheckboxTheme`]: {{site.api}}/flutter/material/CheckboxTheme-class.html
[color scheme]: {{site.api}}/flutter/material/ThemeData/colorScheme.html
[`colorScheme`]: {{site.api}}/flutter/material/ThemeData/colorScheme.html
[`colorScheme.onSecondary`]: {{site.api}}/flutter/material/ColorScheme/onSecondary.html
[`colorScheme.secondary`]: {{site.api}}/flutter/material/ColorScheme/secondary.html
[`ColorScheme`]: {{site.api}}/flutter/material/ColorScheme-class.html
[Issue #56918]: {{site.repo.flutter}}/issues/56918
[FloatingActionButton and ThemeData's accent properties]: {{site.url}}/release/breaking-changes/fab-theme-data-accent-properties
[`FloatingActionButton`]: {{site.api}}/flutter/material/FloatingActionButton-class.html
[`FloatingActionButtonTheme`]: {{site.api}}/flutter/material/FloatingActionButtonTheme-class.html
[`FloatingActionButtonThemeData`]: {{site.api}}/flutter/material/FloatingActionButtonThemeData-class.html
[Material Design spec]: {{site.material}}/design/color
[Material Theme System Updates]: {{site.url}}/go/material-theme-system-updates
[secondary color]: {{site.api}}/flutter/material/ColorScheme/secondary.html
[onSecondary color]: {{site.api}}/flutter/material/ColorScheme/onSecondary.html
[PR #81336]: {{site.repo.flutter}}/pull/81336
[`TextStyle`]: {{site.api}}/flutter/painting/TextStyle-class.html
[`textTheme`]: {{site.api}}/flutter/material/ThemeData/textTheme.html
[`TextTheme`]: {{site.api}}/flutter/material/TextTheme-class.html
[`Theme`]: {{site.api}}/flutter/material/Theme-class.html
[`ThemeData`]: {{site.api}}/flutter/material/ThemeData-class.html
[`ThemeData.estimateBrightnessForColor()`]: {{site.api}}/flutter/material/ThemeData/estimateBrightnessForColor.html
