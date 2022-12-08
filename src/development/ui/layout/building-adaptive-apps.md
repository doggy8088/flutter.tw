---
title: Building adaptive apps
title: 建構自適應的應用
description: Some considerations and instructions on how to build adaptive apps to run on a variety of platforms.
description: 針對多樣化的平台建構自適應的應用的重點和指南。
---

<?code-excerpt path-base="ui/layout/adaptive_app_demos"?>

## Overview

## 概覽

Flutter provides new opportunities to build apps that can
run on mobile, desktop, and the web from a single codebase.
However, with these opportunities, come new challenges.
You want your app to feel familiar to users,
adapting to each platform by maximizing usability and
ensuring a comfortable and seamless experience.
That is, you need to build apps that are not just
multiplatform, but are fully platform adaptive.

Flutter 為在移動端、桌面端和 Web 端使用同樣的程式碼建構應用創造了新的機會。
伴隨著機會而來的，是新的挑戰。
你可能會希望你的應用既能在儘可能複用的情況下自適應多個平台，
又能保證流暢且無縫的體驗，還可以讓使用者保持一致的使用習慣。
這樣的應用不僅僅是為了多個平台而建構的，它能完全地自適應平台的變化。

There are many considerations for developing platform-adaptive
apps, but they fall into three major categories:

在建構平台自適應的應用時，有眾多的考量因素，總的來說分為以下幾類：

* [Layout](#building-adaptive-layouts)

  [佈局](#building-adaptive-layouts)

* [Input](#input)

  [輸入](#input)

* [Idioms and norms](#idioms-and-norms)

  [平台行為習慣與規範](#idioms-and-norms)

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=421723399&bvid=BV1i3411878z&cid=442392038&page=1" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This page covers all three categories in detail
using code snippets to illustrate the concepts.
If you’d like to see how these concepts come together,
check out the [Flokk][] and [Folio][] examples that
were built using the concepts described here.

指南將透過程式碼片段，詳細說明三個類別的概念。
若你想了解這些概念的實際落地情況，可以參考 [Flokk][] 和 [Folio][] 範例。

[Flokk]: {{site.github}}/gskinnerTeam/flokk
[Folio]: {{site.github}}/gskinnerTeam/flutter-folio

Original demo code for adaptive app development techniques from [flutter-adaptive-demo](https://github.com/gskinnerTeam/flutter-adaptive-demo).

## Building adaptive layouts

## 建構自適應的佈局

One of the first things you must consider when bringing
your app to multiple platforms is how to adapt
it to the various sizes and shapes of the screens that
it will run on. 

在建構多平臺的應用時，首要考慮的是如何針對不同大小的裝置進行尺寸適配。

### Layout widgets

### 佈局 widgets

If you've been building apps or websites,
you're probably familiar with creating responsive interfaces.
Luckily for Flutter developers,
there are a large set of widgets to make this easier. 

如果你已經開發過應用或網站，那你可能已經熟悉如何建構自適應的介面。
好訊息是，對於 Flutter 開發者而言，有非常多的 widgets 讓建構更為簡單。

Some of Flutter's most useful layout widgets include:

Flutter 中最有用的部分佈局 widgets 包括：

**Single child**

**單子級 (Single child)**

* [`Align`][]&mdash;Aligns a child within itself.
  It takes a double value between -1 and 1,
  for both the vertical and horizontal alignment.

  [`Align`][]&mdash;&mdash;讓子級在其內部進行對齊。
  可使用 -1 至 1 之間的任意值在垂直和水平方向上進行對齊。

* [`AspectRatio`][]&mdash;Attempts to size the
  child to a specific aspect ratio.

  [`AspectRatio`][]&mdash;&mdash;嘗試讓子級以指定的比例進行佈局。

* [`ConstrainedBox`][]&mdash;Imposes size constraints on its child,
  offering control over the minimum or maximum size.

  [`ConstrainedBox`][]&mdash;&mdash;對子級施加尺寸限制，可以控制最小和最大的尺寸。

* [`CustomSingleChildLayout`][]&mdash;Uses a delegate function
  to position a single child. The delegate can determine
  the layout constraints and positioning for the child.

  [`CustomSingleChildLayout`][]&mdash;&mdash;使用代理方法對單個子級進行定位。
  代理方法可以為子級確定佈局限制和定位。

* [`Expanded`][] and [`Flexible`][]&mdash;Allows a child of a
  `Row` or `Column` to shrink or grow to fill any available space.

  [`Expanded`] 和 [`Flexible`][]&mdash;&mdash;允許
  `Row` 或 `Column` 的子級填充剩餘空間或者儘可能地小。

* [`FractionallySizedBox`][]&mdash;Sizes its child to a fraction
  of the available space.

  [`FractionallySizedBox`][]&mdash;&mdash;基於剩餘空間的比例限定子級的大小。

* [`LayoutBuilder`][]&mdash;Builds a widget that can reflow
  itself based on its parents size.

  [`LayoutBuilder`][]&mdash;&mdash;讓子級可以基於父級的尺寸重新調整其佈局。

* [`SingleChildScrollView`][]&mdash;Adds scrolling to a single child.
  Often used with a `Row` or `Column`.

  [`SingleChildScrollView`][]&mdash;&mdash;為單一的子級新增滾動。
  通常配合 `Row` 或 `Column` 進行使用。

**Multichild**

**多子級 (Multi child)**

* [`Column`][], [`Row`][], and [`Flex`][]&mdash;Lays out children
  in a single horizontal or vertical run.
  Both `Column` and `Row` extend the `Flex` widget.

  [`Column`][]、[`Row`][] 和 [`Flex`][]&mdash;&mdash;
  在同一水平線或垂直線上放置所有子級。
  `Column` 和 `Row` 都繼承了 `Flex` widget。

* [`CustomMultiChildLayout`][]&mdash;Uses a delegate function to
  position multiple children during the layout phase.

  [`CustomMultiChildLayout`][]&mdash;&mdash;
  在佈局過程中使用代理方法對多個子級進行定位。

* [`Flow`][]&mdash;Similar to `CustomMultiChildLayout`,
  but more efficient because it’s performed during the
  paint phase rather than the layout phase.

  [`Flow`][]&mdash;&mdash;相對於 `CustomMultiChildLayout`
  更高效的佈局方式。在繪製過程中使用代理方法對多個子級進行定位。

* [`ListView`][], [`GridView`][], and
  [`CustomScrollView`][]&mdash;Provides scrollable
  lists of children.

  [`ListView`][]、[`GridView`][] 和 [`CustomScrollView`][]&mdash;&mdash;
  為所有子級增加滾動支援。

* [`Stack`][]&mdash;Layers and positions multiple children
  relative to the edges of the `Stack`.
  Functions similarly to position-fixed in CSS.

  [`Stack`][]&mdash;&mdash;基於 `Stack` 的邊界對多個子級進行放置和定位。
  與 CSS 中的 `position: fixed` 功能類似。

* [`Table`][]&mdash;Uses a classic table layout algorithm for
  its children, combining multiple rows and columns.

  [`Table`][]&mdash;&mdash;使用經典的表格佈局演算法，可以組合多列和多行。

* [`Wrap`][]&mdash;Displays its children in multiple horizontal
  or vertical runs.

  [`Wrap`][]&mdash;&mdash;將子級順序顯示在多行或多列內。

To see more available widgets and example code, see
[Layout widgets][].

檢視 [佈局 widgets][Layout widgets] 瞭解更多的 widgets 和程式碼範例。

[`Align`]: {{site.api}}/flutter/widgets/Align-class.html
[`AspectRatio`]: {{site.api}}/flutter/widgets/AspectRatio-class.html
[`Column`]: {{site.api}}/flutter/widgets/Column-class.html
[`ConstrainedBox`]: {{site.api}}/flutter/widgets/ConstrainedBox-class.html
[`CustomMultiChildLayout`]: {{site.api}}/flutter/widgets/CustomMultiChildLayout-class.html
[`CustomScrollView`]: {{site.api}}/flutter/widgets/CustomScrollView-class.html
[`CustomSingleChildLayout`]: {{site.api}}/flutter/widgets/CustomSingleChildLayout-class.html
[`Expanded`]: {{site.api}}/flutter/widgets/Expanded-class.html
[`Flex`]: {{site.api}}/flutter/widgets/Flex-class.html
[`Flexible`]: {{site.api}}/flutter/widgets/Flexible-class.html
[`Flow`]: {{site.api}}/flutter/widgets/Flow-class.html
[`FractionallySizedBox`]: {{site.api}}/flutter/widgets/FractionallySizedBox-class.html
[`GridView`]: {{site.api}}/flutter/widgets/GridView-class.html
[Layout widgets]: {{site.url}}/development/ui/widgets/layout
[`LayoutBuilder`]: {{site.api}}/flutter/widgets/LayoutBuilder-class.html
[`ListView`]: {{site.api}}/flutter/widgets/ListView-class.html
[`Row`]: {{site.api}}/flutter/widgets/Row-class.html
[`SingleChildScrollView`]: {{site.api}}/flutter/widgets/SingleChildScrollView-class.html
[`Stack`]: {{site.api}}/flutter/widgets/Stack-class.html
[`Table`]: {{site.api}}/flutter/widgets/Table-class.html
[`Wrap`]: {{site.api}}/flutter/widgets/Wrap-class.html

### Visual density

### 視覺密度

Different input devices offer various levels of precision,
which necessitate differently sized hit areas.
Flutter's `VisualDensity` class makes it easy to adjust the
density of your views across the entire application,
for example, by making a button larger
(and therefore easier to tap) on a touch device.

不同的裝置會提供不同級別的顯示密度，使得操作的命中區域也要隨之變化。
Flutter 的 `VisualDensity` 類可以讓你快速地調整整個應用的檢視密度，
比如在可觸控裝置上放大一個按鈕（使其更容易被點選）。

不同的裝置會提供不同級別的顯示密度，使得操作的命中區域也要隨之變化。
Flutter 的 `VisualDensity` 類可以讓你快速地調整整個應用的檢視密度，
比如在可觸控裝置上放大一個按鈕（使其更容易被點選）。

When you change the `VisualDensity` for your `MaterialApp`,
`MaterialComponents` that support it animate their densities
to match. By default, both horizontal and vertical densities
are set to 0.0, but you can set the densities to any negative
or positive value that you want. By switching between different
densities, you can easily adjust your UI:

在你改變 `MaterialApp` 的 `VisualDensity` 時，
已支援 `VisualDensity` 的 `MaterialComponents`
會以動畫過渡的形式改變其自身的密度。
水平和垂直方向的密度預設都為 0.0，你可以將它設定為任意的正負值，
這樣就可以透過調整密度輕鬆地調整你的 UI：

![Adaptive scaffold]({{site.url}}/assets/images/docs/development/ui/layout/adaptive_scaffold.gif){:width="100%"}

To set a custom visual density, inject the density into
your `MaterialApp` theme:

若想使用自訂的視覺密度，請在你的 `MaterialApp` 的主題中進行設定：

<?code-excerpt "lib/main.dart (VisualDensity)"?>
```dart
double densityAmt = touchMode ? 0.0 : -1.0;
VisualDensity density =
    VisualDensity(horizontal: densityAmt, vertical: densityAmt);
return MaterialApp(
  theme: ThemeData(visualDensity: density),
  home: MainAppScaffold(),
  debugShowCheckedModeBanner: false,
);
```

To use `VisualDensity` inside your own views,
you can look it up: 

若想在你的檢視中使用 `VisualDensity`，你可以向上查詢：

<?code-excerpt "lib/pages/adaptive_reflow_page.dart (VisualDensityOwnView)"?>
```dart
VisualDensity density = Theme.of(context).visualDensity;
```

Not only does the container react automatically to changes
in density, it also animates when it changes.
This ties together your custom components,
along with the built-in components,
for a smooth transition effect across the app.

在密度變化時，容器不僅能自動地對其做出反應，
還會結合動畫進行過渡變化。
所有的元件都會聯絡在一起，使整個應用平滑過渡。

As shown, `VisualDensity` is unit-less,
so it can mean different things to different views.
In this example, 1 density unit equals 6 pixels,
but this is totally up to your views to decide.
The fact that it is unit-less makes it quite versatile,
and it should work in most contexts. 

我們可以看到，`VisualDensity` 是沒有單位的，
所以在不同的檢視上可能有不同的含義。
在以上的例子中，1 個單位的密度等同於 6 個邏輯畫素。
具體的處理完全由你的檢視自行決定。
無單位的設計讓它可以處理通用情況，
能在大部分的場景下使用。

It’s worth noting that the Material Components generally
use a value of around 4 logical pixels for each
visual density unit. For more information about the
supported components, see [`VisualDensity`][] API.
For more information about density principles in general,
see the [Material Design guide][]. 

值得注意的是，在 Material 的元件中，1 個單位的視覺密度通常等於 4 個邏輯畫素。
你可以檢視 [`VisualDensity`][] API 文件瞭解更多支援視覺密度的元件。
若想了解視覺密度的通用原則，請檢視 [Material Design 指南][Material Design guide]。

[Material Design guide]: {{site.material}}/design/layout/applying-density.html#usage
[`VisualDensity`]: {{site.api}}/flutter/material/VisualDensity-class.html

### Contextual layout

### 基於上下文的佈局

If you need more than density changes and can't find a
widget that does what you need, you can take a more
procedural approach to adjust parameters, calculate sizes,
swap widgets, or completely restructure your UI to suit
a particular form factor. 

如果你需要的不僅是密度的變化，並且沒有找到一個滿足需求的 widget，
那麼你可以使用程式碼進行更細化的控制、計算尺寸、切換 widgets
或是完全重新建構你的 UI 適配對應的外形結構。

#### Screen-based breakpoints

#### 基於螢幕大小的分界點

The simplest form of procedural layouts uses
screen-based breakpoints. In Flutter,
this can be done with the `MediaQuery` API.
There are no hard and fast rules for the sizes to use
here, but these are general values: 

最簡單的程式碼控制佈局方式是基於螢幕尺寸來定義分界點。
在 Flutter 中，你可以使用 `MediaQuery` API 實現這些分界點。
具體需要使用的大小並沒有作出硬性規定，下方是一些通用的值：

<?code-excerpt "lib/global/device_size.dart (FormFactor)"?>
```dart
class FormFactor {
  static double desktop = 900;
  static double tablet = 600;
  static double handset = 300;
}
```

Using breakpoints, you can set up a simple system
to determine the device type:

使用分界點可以讓你透過簡單的判斷快速確定裝置的型別：

<?code-excerpt "lib/global/device_size.dart (getFormFactor)"?>
```dart
ScreenType getFormFactor(BuildContext context) {
  // Use .shortestSide to detect device type regardless of orientation
  double deviceWidth = MediaQuery.of(context).size.shortestSide;
  if (deviceWidth > FormFactor.desktop) return ScreenType.Desktop;
  if (deviceWidth > FormFactor.tablet) return ScreenType.Tablet;
  if (deviceWidth > FormFactor.handset) return ScreenType.Handset;
  return ScreenType.Watch;
}
```

As an alternative, you could abstract it more
and define it in terms of small to large:

又或者，你可以對大小型別進行更深層次的抽象，並且按照從小到大的方式定義：

<?code-excerpt "lib/global/device_size.dart (ScreenSize)"?>
```dart
enum ScreenSize { Small, Normal, Large, ExtraLarge }

ScreenSize getSize(BuildContext context) {
  double deviceWidth = MediaQuery.of(context).size.shortestSide;
  if (deviceWidth > 900) return ScreenSize.ExtraLarge;
  if (deviceWidth > 600) return ScreenSize.Large;
  if (deviceWidth > 300) return ScreenSize.Normal;
  return ScreenSize.Small;
}
```
 
Screen-based breakpoints are best used for making
top-level decisions in your app. Changing things like
visual density, paddings, or font-sizes are best when
defined on a global basis. 

使用基於螢幕大小的分界點的最佳場景，是在應用的最上層進行尺寸決策。
在需要改變視覺密度、邊距或者字型大小時，定義全域的基數是最好的方式。

You can also use screen-based breakpoints to reflow your
top-level widget trees. For example, you could switch
from a vertical to a horizontal layout when the user isn’t on a handset:

你也可以利用分界點重新組織最上層的 widget 結構。
例如，你可以判斷使用者是否使用手持裝置，來切換垂直或水平的佈局：

<?code-excerpt "lib/global/device_size.dart (MediaQuery)"?>
```dart
bool isHandset = MediaQuery.of(context).size.width < 600;
return Flex(
    children: [Text('Foo'), Text('Bar'), Text('Baz')],
    direction: isHandset ? Axis.vertical : Axis.horizontal);
```

In another widget,
you might swap some of the children completely: 

在其他的 widget 中，你也可以切換部分子級 widget：

<?code-excerpt "lib/global/device_size.dart (WidgetSwap)"?>
```dart
Widget foo = Row(
  children: [
    ...isHandset ? _getHandsetChildren() : _getNormalChildren(),
  ],
);
```

#### Use LayoutBuilder for extra flexibility

#### 使用 LayoutBuilder 提升佈局靈活性

Even though checking total screen size is great for
full-screen pages or making global layout decisions,
it’s often not ideal for nested subviews.
Often, subviews have their own internal breakpoints
and care only about the space that they have available to render.

儘管對於全屏頁面或者全域的佈局決策而言，判斷整個螢幕大小非常有效，
但對於內嵌的子檢視而言，並不一定是合理的方案。
子檢視通常有自己的分界點，並且只關心它們可用的渲染空間。

The simplest way to handle this in Flutter is using the
[`LayoutBuilder`][] class. `LayoutBuilder` allows a
widget to respond to incoming local size constraints,
which can make the widget more versatile than if it
depended on a global value.

在 Flutter 內處理這類場景最簡單的做法是使用 [`LayoutBuilder`][]。
`LayoutBuilder` 讓 widget 可以根據其父級的限制進行調整，
相比依賴全域的尺寸限制而言更為通用。

The previous example could be rewritten using `LayoutBuilder`:

之前的範例可以使用 `LayoutBuilder` 重寫：

<?code-excerpt "lib/widgets/extra_widget_excerpts.dart (LayoutBuilder)"?>
```dart
Widget foo = LayoutBuilder(
    builder: (context, constraints) {
  bool useVerticalLayout = constraints.maxWidth < 400.0;
  return Flex(
    children: [
      Text('Hello'),
      Text('World'),
    ],
    direction: useVerticalLayout ? Axis.vertical : Axis.horizontal,
  );
});
```

This widget can now be composed within a side panel,
dialog, or even a full-screen view,
and adapt its layout to whatever space is provided.

現在這個 widget 可以組裝在側邊面板、彈框又或是全屏檢視中，
並且根據尺寸自適應佈局。

#### Device segmentation

#### 裝置細分

There are times when you want to make layout decisions
based on the actual platform you’re running on,
regardless of size. For example, when building a
custom title bar, you might need to check the operating
system type and tweak the layout of your title bar, so
it doesn’t get covered by the native window buttons. 

有時你可能需要根據實際執行的平台進行佈局處理，而不是基於大小。
例如，在建構自訂的標題欄時，
你可能需要判斷裝置的平台來處理佈局，以防被原生視窗的按鈕遮擋。

To determine which combination of platforms you’re on,
you can use the [`Platform`][] API along with the `kIsWeb` value:

想判斷應用當前所處的平台，你可以使用 [`Platform`][] API 和 `kIsWeb` 組合進行判斷：

[`Platform`]: {{site.api}}/flutter/package-platform_platform/Platform-class.html

<?code-excerpt "lib/global/device_type.dart (Platforms)"?>
```dart
bool get isMobileDevice => !kIsWeb && (Platform.isIOS || Platform.isAndroid);
bool get isDesktopDevice =>
    !kIsWeb && (Platform.isMacOS || Platform.isWindows || Platform.isLinux);
bool get isMobileDeviceOrWeb => kIsWeb || isMobileDevice;
bool get isDesktopDeviceOrWeb => kIsWeb || isDesktopDevice;
```

The `Platform` API can’t be accessed from web builds without 
throwing an exception, because the `dart.io` package is not
supported on the web target. As a result, this code checks 
for web first, and because of short-circuiting, Dart will 
never call `Platform` on web targets.

在建構 Web 平台應用時，由於 `dart.io` package 不支援 Web 平台，
導致使用 `Platform` API 時會例外。
所以在上面的程式碼中，會首先判斷是否在 Web 平台，
基於這個條件，在 Web 平臺上永遠不會呼叫 `Platform` API。

### Single source of truth for styling

### 使用單一來源控制樣式

You’ll probably find it easier to maintain your views
if you create a single source of truth for styling values
like padding, spacing, corner shape, font sizes, and so on.
This can be done easily with some helper classes:

使用單一的來源對樣式進行維護，可以讓你更簡便地控制邊距、間距、圓角、字型等樣式值。
你可以利用一些幫助類進行實現：

<?code-excerpt "lib/global/device_type.dart (Styling)"?>
```dart
class Insets {
  static const double xsmall = 3;
  static const double small = 4;
  static const double medium = 5;
  static const double large = 10;
  static const double extraLarge = 20;
  // etc
}

class Fonts {
  static const String raleway = 'Raleway';
  // etc
}

class TextStyles {
  static const TextStyle raleway = const TextStyle(
    fontFamily: Fonts.raleway,
  );
  static TextStyle buttonText1 =
      TextStyle(fontWeight: FontWeight.bold, fontSize: 14);
  static TextStyle buttonText2 =
      TextStyle(fontWeight: FontWeight.normal, fontSize: 11);
  static TextStyle h1 = TextStyle(fontWeight: FontWeight.bold, fontSize: 22);
  static TextStyle h2 = TextStyle(fontWeight: FontWeight.bold, fontSize: 16);
  static late TextStyle body1 = raleway.copyWith(color: Color(0xFF42A5F5));
  // etc
}
```

These constants can then be used in place of hard-coded numeric values:

這些常量可以用來替代硬編碼的值：

<?code-excerpt "lib/global/device_type.dart (UseConstants)"?>
```dart
return Padding(
  padding: EdgeInsets.all(Insets.small),
  child: Text('Hello!', style: TextStyles.body1),
);
```

With all views referencing the same shared-design system rules,
they tend to look better and more consistent.
Making a change or adjusting a value for a specific platform
can be done in a single place, instead of using an error-prone
search and replace. Using shared rules has the added benefit
of helping enforce consistency on the design side.

由於所有的檢視都參考了相同設計系統的規範，它們通常看起來更一致且更順暢。
與其進行容易出錯的搜尋替換，你可以將平台對應樣式值的修改集中在一處。
使用共享的規則也對設計的一致性有所幫助。

Some common design system categories that can be represented
this way are: 

常見的設計型別裡，如下這些類別可以以這樣的方式進行組織：

* Animation timings 

  動畫時間

* Sizes and breakpoints

  尺寸大小和分界點

* Insets and paddings

  遮蓋和內邊距區域

* Corner radius

  圓角

* Shadows

  陰影

* Strokes 

  筆畫

* Font families, sizes, and styles

  字體系列、大小和樣式

Like most rules, there are exceptions:
one-off values that are used nowhere else in the app.
There is little point in cluttering up the styling rules
with these values, but it’s worth considering if they
should be derived from an existing value (for example,
`padding + 1.0`). You should also watch for reuse or duplication
of the same semantic values. Those values should likely be
added to the global styling ruleset.

當然，上述的例子也有一些例外：在應用中只使用了一次的值。
將這些值放在樣式規則裡屬實無用之舉，
但可以考慮它們是否能從現有的值延伸（例如 `padding + 1.0`）。
你也可以留意一些有著相同意義複用的值，這些值也許可以新增到全域的樣式規則裡。

### Design to the strengths of each form factor

### 針對不同外形螢幕的特性進行設計

Beyond screen size, you should also spend time
considering the unique strengths and weaknesses
of different form factors. It isn’t always ideal
for your multiplatform app to offer identical
functionality everywhere. Consider whether it makes
sense to focus on specific capabilities,
or even remove certain features, on some device categories.

除了螢幕尺寸以外，你也應當花時間，針對各種不同外形螢幕的優劣點進行設計。
支援多平臺的應用，並不能在所有的裝置上都提供理想的體驗。
實際開發時，可以考慮某些特定的功能是否合理，也可以考慮在某些平臺上移除特定的功能。

For example, mobile devices are portable and have cameras,
but they aren't well suited for detailed creative work.
With this in mind, you might focus more on capturing content
and tagging it with location data for a mobile UI,
but focus on organizing or manipulating that content
for a tablet or desktop UI.

舉個例子，移動裝置是十分便攜的，一般還配有攝影頭，但它們並不適合深度的內容創作工作。
基於這個前提，你的應用可以更側重於內容捕獲，並使用位置資訊對其進行標記，配上移動端的介面，
而另一方面，在平板和桌面介面上專注於組織和操作產出的內容。

Another example is leveraging the web's extremely low barrier
for sharing. If you’re deploying a web app,
decide which deep links to support,
and design your navigation routes with those in mind.

另一個例子是充分利用 Web 平台的快速分享能力。如果你正在部署 Web 應用，
可以考慮哪些頁面會使用 deep link，並根據配置來設計應用的導航。

The key takeaway here is to think about what each
platform does best and see if there are unique capabilities
you can leverage.

此處的關鍵點在於，如何發揮每個平台的長處，尋找平台可以利用的特有功能。

### Use desktop build targets for rapid testing

### 透過建構桌面應用程式進行快速測試

One of the most effective ways to test adaptive
interfaces is to take advantage of the desktop build targets. 

測試自適應介面的最快方式，是利用桌面端快速進行建構。

When running on a desktop, you can easily resize the window
while the app is running to preview various screen sizes.
This, combined with hot reload, can greatly accelerate the
development of a responsive UI.

在桌面上執行應用時，你可以在應用執行時輕易地改變視窗的大小，預覽多種尺寸的佈局。
配上熱重載，能極大程度地加快響應式開發的速度。

![Adaptive scaffold 2]({{site.url}}/assets/images/docs/development/ui/layout/adaptive_scaffold2.gif){:width="100%"}

### Solve touch first

### 優先處理觸控操作

Building a great touch UI can often be more difficult
than a traditional desktop UI due, in part,
to the lack of input accelerators like right-click,
scroll wheel, or keyboard shortcuts. 

在移動端建構優良的觸控互動式 UI 通常比傳統的桌面端更為困難，
因為它缺少類似右鍵單擊、滾輪或鍵盤快捷鍵這樣的快速輸入裝置。

One way to approach this challenge is to focus initially
on a great touch-oriented UI. You can still do most of
your testing using the desktop target for its iteration speed.
But, remember to switch frequently to a mobile device to
verify that everything feels right. 

在一開始就專注於提升觸控體驗的 UI，足以應對這樣的挑戰。
你依舊可以使用桌面端來提高你的開發效率，但要記得時不時切換回移動端，
驗證開發的內容是否正常。

After you have the touch interface polished, you can tweak
the visual density for mouse users, and then layer on all
the additional inputs. Approach these other inputs as
accelerator—alternatives that make a task faster.
The important thing to consider is what a user expects
when using a particular input device,
and work to reflect that in your app.

完善了觸控介面後，你可以調整面向滑鼠使用者的視覺密度，然後對所有的輸入裝置進行分層。
這些輸入裝置應當作為加快你的應用使用速度的途徑。
在這裡需要考慮的應當是使用者對於應用體驗的期望，並在應用中合理地實現這些期望。

## Input

## 輸入

Of course, it isn’t enough to just adapt how your app looks,
you also have to support varying user inputs.
The mouse and keyboard introduce input types beyond those
found on a touch device—like scroll wheel, right-click,
hover interactions, tab traversal, and keyboard shortcuts.

當然，應用只適配了介面是遠遠不夠的，你還需要適配各種使用者的輸入操作。
滑鼠和鍵盤提供了觸控裝置不具備的輸入方式，
例如滾輪、右鍵點選、懸停互動、Tab 遍歷切換和鍵盤快捷鍵。

### Scroll wheel

### 滾輪

Scrolling widgets like `ScrollView` or `ListView`
support the scroll wheel by default, and because
almost every scrollable custom widget is built
using one of these, it works with them as well.

像 `ScrollView` 和 `ListView` 這樣的滾動 widget 預設支援滾輪行為，
而大部分可滾動的自訂 widget 都是基於它們建構的，所以也同樣支援。

If you need to implement custom scroll behavior,
you can use the [`Listener`][] widget, which lets you
customize how your UI reacts to the scroll wheel.

如果你需要實現自訂的滑動行為，可以使用 [`Listener`][] widget，
透過它你可以完全自訂 UI 如何響應滾輪行為。

<?code-excerpt "lib/widgets/extra_widget_excerpts.dart (PointerScroll)"?>
```dart
return Listener(
    onPointerSignal: (event) {
      if (event is PointerScrollEvent) print(event.scrollDelta.dy);
    },
    child: ListView());
```

[`Listener`]: {{site.api}}/flutter/widgets/Listener-class.html

### Tab traversal and focus interactions

### Tab 遍歷切換和焦點互動

Users with physical keyboards expect that they can use
the tab key to quickly navigate your application,
and users with motor or vision differences often rely
completely on keyboard navigation.

使用鍵盤的使用者，可能會希望透過 Tab 鍵在應用中快速導航，
特別是對有動效和視覺障礙的使用者，他們幾乎完全依賴於鍵盤導航。

There are two considerations for tab interactions:
how focus moves from widget to widget, known as traversal,
and the visual highlight shown when a widget is focused.

在考慮 Tab 遍歷切換時，有兩點需要注意：
焦點如何在 widget 之間遍歷，以及 widget 聚焦時的突出顯示。

Most built-in components, like buttons and text fields,
support traversal and highlights by default.
If you have your own widget that you want included in
traversal, you can use the [`FocusableActionDetector`][] widget
to create your own controls. It combines the functionality
of [`Actions`][], [`Shortcuts`][], [`MouseRegion`][], and
[`Focus`][] widgets to create a detector that defines actions
and key bindings, and provides callbacks for handling focus
and hover highlights.

大部分內建的元件，類似於按鈕和輸入框，都預設支援遍歷和高亮。
如果你想讓自己的 widget 包含在遍歷中，你可以利用 [`FocusableActionDetector`][] 進行控制。
它將 [`Actions`][]、[`Shortcuts`][]、[`MouseRegion`][] 和 [`Focus`][]
的能力進行了整合，創建出一個可以定義行為和鍵位繫結，並且提供聚焦和懸浮高亮事件回呼(Callback)的 widget。

<?code-excerpt "lib/pages/focus_examples_page.dart (_BasicActionDetectorState)"?>
```dart
class _BasicActionDetectorState extends State<BasicActionDetector> {
  bool _hasFocus = false;
  @override
  Widget build(BuildContext context) {
    return FocusableActionDetector(
      onFocusChange: (value) => setState(() => _hasFocus = value),
      actions: <Type, Action<Intent>>{
        ActivateIntent: CallbackAction<Intent>(onInvoke: (intent) {
          print('Enter or Space was pressed!');
          return null;
        }),
      },
      child: Stack(
        clipBehavior: Clip.none,
        children: [
          FlutterLogo(size: 100),
          // Position focus in the negative margin for a cool effect
          if (_hasFocus)
            Positioned(
                left: -4,
                top: -4,
                bottom: -4,
                right: -4,
                child: _roundedBorder())
        ],
      ),
    );
  }
}
```

[`Actions`]: {{site.api}}/flutter/widgets/Actions-class.html
[`Focus`]: {{site.api}}/flutter/widgets/Focus-class.html
[`FocusableActionDetector`]: {{site.api}}/flutter/widgets/FocusableActionDetector-class.html
[`MouseRegion`]: {{site.api}}/flutter/widgets/MouseRegion-class.html
[`Shortcuts`]: {{site.api}}/flutter/widgets/Shortcuts-class.html

#### Controlling traversal order

#### 控制遍歷的順序

To get more control over the order that
widgets are focused on when the user presses tab,
you can use [`FocusTraversalGroup`][] to define sections
of the tree that should be treated as a group when tabbing. 

想要控制使用者按下 Tab 鍵時的 widget 切換順序，
你可以使用 [`FocusTraversalGroup`][] 來指定樹中的區域，作為切換時的組別。

For example, you might to tab through all the fields in
a form before tabbing to the submit button:

例如，你可能想要使用者逐個切換所有的輸入框，最後再切換到提交按鈕：

<?code-excerpt "lib/pages/focus_examples_page.dart (FocusTraversalGroup)"?>
```dart
return Column(children: [
  FocusTraversalGroup(
    child: MyFormWithMultipleColumnsAndRows(),
  ),
  SubmitButton(),
]);
```

Flutter has several built-in ways to traverse widgets and groups,
defaulting to the `ReadingOrderTraversalPolicy` class.
This class usually works well, but it’s possible to modify this
using another predefined `TraversalPolicy` class or by creating
a custom policy.

Flutter 有幾種內建的方法對 widget 和組別進行遍歷，預設使用的是
`ReadingOrderTraversalPolicy` 類別。這個類通常可以正常使用，
你也可以建立另一個 `TraversalPolicy` 或建立一個自訂的規則，
對它進行定義。

[`FocusTraversalGroup`]: {{site.api}}/flutter/widgets/FocusTraversalGroup-class.html

### Keyboard accelerators

### 提升使用者操作速度的鍵盤

In addition to tab traversal, desktop and web users are accustomed
to having various keyboard shortcuts bound to specific actions.
Whether it’s the `Delete` key for quick deletions or
`Control+N` for a new document, be sure to consider the different
accelerators your users expect. The keyboard is a powerful
input tool, so try to squeeze as much efficiency from it as you can.
Your users will appreciate it!

除了使用 Tab 遍歷元素以外，桌面和 Web 使用者還習慣將為各種操作繫結鍵盤快捷鍵。
無論是 `Delete` 鍵進行快速刪除，還是 `Control+N` 新建文件，
你都需要認真考慮使用者對這些操作的期望。
鍵盤是非常強力的輸入工具，所以請儘可能讓它發揮最大的作用和效果。
使用者會給予高度評價。

Keyboard accelerators can be accomplished in a few ways in Flutter
depending on your goals.

根據目標的不同，在 Flutter 中可以通過幾種方式實現利用鍵盤提升使用者操作速度。

If you have a single widget like a `TextField` or a `Button` that
already has a focus node, you can wrap it in a
[`RawKeyboardListener`][] and listen for keyboard events:

如果你已經有一個包含焦點的 widget，例如 `TextField` 或者 `Button`，
你可以巢狀(Nesting)一個 [`RawKeyboardListener`][] 監聽鍵盤事件：

<?code-excerpt "lib/pages/focus_examples_page.dart (FocusRawKeyboardListener)"?>
```dart
  @override
  Widget build(BuildContext context) {
    return Focus(
      onKey: (node, event) {
        if (event is RawKeyDownEvent) {
          print(event.logicalKey);
        }
        return KeyEventResult.ignored;
      },
      child: ConstrainedBox(
        constraints: BoxConstraints(maxWidth: 400),
        child: TextField(
          decoration: InputDecoration(
            border: OutlineInputBorder(),
          ),
        ),
      ),
    );
  }
}
```

If you’d like to apply a set of keyboard shortcuts to a
large section of the tree, you can use the [`Shortcuts`][] widget:

如果你想將一組鍵盤快捷鍵應用到更大範圍的 widget，你可以使用 [`Shortcuts`][] widget：

<?code-excerpt "lib/widgets/extra_widget_excerpts.dart (Shortcuts)"?>
```dart
// Define a class for each type of shortcut action you want
class CreateNewItemIntent extends Intent {
  const CreateNewItemIntent();
}

Widget build(BuildContext context) {
  return Shortcuts(
    // Bind intents to key combinations
    shortcuts: <ShortcutActivator, Intent>{
      SingleActivator(LogicalKeyboardKey.keyN, control: true):
          CreateNewItemIntent(),
    },
    child: Actions(
      // Bind intents to an actual method in your code
      actions: <Type, Action<Intent>>{
        CreateNewItemIntent: CallbackAction<CreateNewItemIntent>(
            onInvoke: (intent) => _createNewItem()),
      },
      // Your sub-tree must be wrapped in a focusNode, so it can take focus.
      child: Focus(
        autofocus: true,
        child: Container(),
      ),
    ),
  );
}
```

The [`Shortcuts`][] widget is useful because it only
allows shortcuts to be fired when this widget tree
or one of its children has focus and is visible.

[`Shortcuts`][] widget 非常有用，
因為它會讓 widget 樹的這一分支或它的子級僅在有焦點且可見時觸發快捷方式。

The final option is a global listener. This listener
can be used for always-on, app-wide shortcuts or for
panels that can accept shortcuts whenever they're visible
(regardless of their focus state). Adding global listeners
is easy with [`RawKeyboard`][]:

最後，你還可以全域新增監聽。這樣的監聽可以用於始終需要監聽，且為應用全域的快捷鍵，
或是在任何時候（無論是否已聚焦）都接收快捷鍵的部分。
使用 [`RawKeyboard`][] 新增全域監聽非常簡單：

<?code-excerpt "lib/widgets/extra_widget_excerpts.dart (RawKeyboard)"?>
```dart
void initState() {
  super.initState();
  RawKeyboard.instance.addListener(_handleKey);
}

@override
void dispose() {
  RawKeyboard.instance.removeListener(_handleKey);
  super.dispose();
}
```

To check key combinations with the global listener,
you can use the `RawKeyboard.instance.keysPressed` map.
For example, a method like the following can check whether any
of the provided keys are being held down:

要想在全域監聽中判斷組合按鍵，你可以使用
`RawKeyboard.instance.keysPressed` 這個 Map 進行判斷。
例如下面這個方法，可以判斷是否已經按下了指定的按鍵：

<?code-excerpt "lib/widgets/extra_widget_excerpts.dart (KeysPressed)"?>
```dart
static bool isKeyDown(Set<LogicalKeyboardKey> keys) {
  return keys.intersection(RawKeyboard.instance.keysPressed).isNotEmpty;
}
```

Putting these two things together,
you can fire an action when `Shift+N` is pressed:

將它們合併判斷，你就可以在 `Shift+N` 同時按下時觸發行為：

<?code-excerpt "lib/widgets/extra_widget_excerpts.dart (HandleKey)"?>
```dart
void _handleKey(event) {
  if (event is RawKeyDownEvent) {
    bool isShiftDown = isKeyDown({
      LogicalKeyboardKey.shiftLeft,
      LogicalKeyboardKey.shiftRight,
    });
    if (isShiftDown && event.logicalKey == LogicalKeyboardKey.keyN) {
      _createNewItem();
    }
  }
}
```

One note of caution when using the static listener,
is that you often need to disable it when the user
is typing in a field or when the widget it’s associated with
is hidden from view.
Unlike with `Shortcuts` or `RawKeyboardListener`,
this is your responsibility to manage. This can be especially
important when you’re binding a Delete/Backspace accelerator for
`Delete`, but then have child `TextFields` that the user
might be typing in.

使用靜態的監聽時有一件值得注意的事情，當用戶在輸入框中輸入內容，
或關聯的 widget 從檢視中隱藏時，通常需要禁用監聽。
與 `Shortcuts` 和 `RawKeyboardListener` 不同，你需要自己對它們進行管理。
當你在為 `Delete` 鍵建構一個刪除或退格行為的監聽時，需要尤其注意，
因為使用者可能會在 `TextField` 中輸入內容時受到影響。

[`RawKeyboard`]: {{site.api}}/flutter/services/RawKeyboard-class.html
[`RawKeyboardListener`]: {{site.api}}/flutter/widgets/RawKeyboardListener-class.html

### Mouse enter, exit, and hover

### 滑鼠進入、移出和懸停事件

On desktop, it’s common to change the mouse cursor
to indicate the functionality about the content the
mouse is hovering over. For example, you usually see
a hand cursor when you hover over a button,
or an `I` cursor when you hover over text.

在桌面平臺上，常會在滑鼠懸停在內容上時，改變游標以表明不同的功能用途。
例如，你會在滑鼠懸停的按鈕上看到手指游標，或是在懸停的文字上看到一個 `I`。

The Material Component set has built-in support
for your standard button and text cursors.
To change the cursor from within your own widgets,
use [`MouseRegion`][]:

Material 系列元件內建了對標準的按鈕和文字的游標支援。
你可以使用 [`MouseRegion`][] 在你自己的 widget 上改變游標。

<?code-excerpt "lib/pages/focus_examples_page.dart (MouseRegion)"?>
```dart
// Show hand cursor
return MouseRegion(
  cursor: SystemMouseCursors.click,
  // Request focus when clicked
  child: GestureDetector(
    onTap: () {
      Focus.of(context).requestFocus();
      _submit();
    },
    child: Logo(showBorder: hasFocus),
  ),
);
```

`MouseRegion` is also useful for creating custom
rollover and hover effects:

`MouseRegion` 對於建立自訂翻轉和懸停效果也很有用：

<?code-excerpt "lib/pages/focus_examples_page.dart (MouseOver)"?>
```dart
return MouseRegion(
  onEnter: (_) => setState(() => _isMouseOver = true),
  onExit: (_) => setState(() => _isMouseOver = false),
  onHover: (e) => print(e.localPosition),
  child: Container(
    height: 500,
    color: _isMouseOver ? Colors.blue : Colors.black,
  ),
);
```


[`MouseRegions`]: {{site.api}}/flutter/widgets/MouseRegion-class.html


## Idioms and norms

## 平台行為習慣與規範

The final area to consider for adaptive apps is platform standards.
Each platform has its own idioms and norms;
these nominal or de facto standards inform user expectations
of how an application should behave. Thanks, in part to the web,
users are accustomed to more customized experiences,
but reflecting these platform standards can still provide
significant benefits:

最後，我們需要為自適應應用考慮平台標準。
每個平台都有其不同的行為習慣與規範，
這些名義和事實上的標準將操作應用的方法告知了使用者。
在當下網路如此便利的時代，使用者更傾向於更加個性化的體驗，
但是提供這些平台標準，依然可以帶來一些顯著的好處：

* **Reduce cognitive load**&mdash;By matching the user's
  existing mental model, accomplishing tasks becomes intuitive,
  which requires less thinking,
  boosts productivity, and reduces frustrations.

  **減少認知學習成本**&mdash;&mdash;與使用者期望的互動進行匹配，
  讓使用者更直接地完成操作，而無需過多地思考，從而提高生產力，減少其中的頓挫感。

* **Build trust**&mdash;Users can become wary or suspicious
  when applications don't adhere to their expectations.
  Conversely, a UI that feels familiar can build user trust
  and can help improve the perception of quality.
  This often has the added benefit of better app store
  ratings&mdash;something we can all appreciate! 

  **建立與使用者之間的信任**&mdash;&mdash;在應用的互動表現不如預期時，
  使用者會逐漸對應用本身產生懷疑。相反，使用讓使用者感到熟悉的 UI，
  可以快速地建立應用與使用者之間的信任，讓使用者提高對應用品質的評價。
  同時這也會讓應用商店的評級更為可觀&mdash;&mdash;皆大歡喜。

### Consider expected behavior on each platform

### 考慮每個平台的預期互動行為

The first step is to spend some time considering what
the expected appearance, presentation, or behavior is on this platform.
Try to forget any limitations of your current implementation,
and just envision the ideal user experience.
Work backwards from there.

考慮的第一步，是花一些時間思考應用在這個平臺上期望的外觀、表現或者行為。
試著將當前能否實現的限制拋諸腦後，僅針對理想的使用者體驗進行逆向思考。

Another way to think about this is to ask,
"How would a user of this platform expect to achieve this goal?"
Then, try to envision how that would work in your app
without any compromises.

另一種思考方式，是向自己提問：「該平台的使用者要想完成這個操作，需要什麼樣的互動？」
接著開始設想如何在應用內正常且無妥協地實現它。

This can be difficult if you aren’t a regular user of the platform.
You might be unaware of the specific idioms and can easily miss
them completely. For example, a lifetime Android user will
likely be unaware of platform conventions on iOS,
and the same holds true for macOS, Linux, and Windows.
These differences might be subtle to you,
but be painfully obvious to an experienced user. 

如果你本身不是這個平台的常用使用者，這項工作就有一定的難度。
某些特定的行為和習慣，很容易會被你完全忽略。
例如，一位一直使用 Android 的使用者很有可能不清楚 iOS 平台的約定，
同樣還有 macOS、Linux 和 Windows。
對於身為開發者的你來說，這些差異可能微乎其微，
但對於有經驗的使用者來說是顯而易見的。

#### Find a platform advocate

#### 尋找一位平台的實際使用者（倡導者）

If possible, assign someone as an advocate for each platform.
Ideally, your advocate uses the platform as their primary device,
and can offer the perspective of a highly opinionated user.
To reduce the number of people, combine roles.
Have one advocate for Windows and Android,
one for Linux and the web, and one for Mac and iOS.

最好為每一種適配平台指定一位負責人。
理想情況下，負責人以他們熟悉的平台為主，提供他們對平台特有的看法和意見。
若想減少人員，兼顧角色，可以安排一位支援 Windows 和 Android，
一位支援 Linux 和 Web，一位支援 Mac 和 iOS。

The goal is to have constant, informed feedback so the app
feels great on each platform. Advocates should be encouraged
to be quite picky, calling out anything they feel differs from
typical applications on their device. A simple example is how
the default button in a dialog is typically on the left on Mac
and Linux, but is on the right on Windows.
Details like that are easy to miss if you aren't using a platform
on a regular basis.

這樣做的目的是為了得到持續且有效的反饋，讓應用在每個平臺上都能表現良好。
負責人應該以挑剔的角度對平台實現進行把關。一個非常簡單的例子是在對話方塊裡，
對話方塊本身按鈕的預設位置在 Mac 和 Linux 上通常位於左側，而在 Windows 上位於右側。
如果你不是平台的常用使用者，通常會錯過這樣的細節。

{{site.alert.secondary}}

  **Important**: Advocates don’t need to be developers or
  even full-time team members. They can be designers,
  stakeholders, or external testers that are provided
  with regular builds. 

  **重要**：負責人不需要是開發者或者一直在開發的團隊成員，
  可以是設計師、利益相關的人士或是外部的版本定期測試人員。

{{site.alert.end}}

#### Stay unique

#### 保持應用的獨特

Conforming to expected behaviors doesn't mean that your app
needs to use default components or styling.
Many of the most popular multiplatform apps have very distinct
and opinionated UIs including custom buttons, context menus,
and title bars.

應用並不一定需要預設的元件或樣式來保證其符合期望的行為。
許多非常流行的多平臺應用都有自成一派的 UI，包括自訂按鈕、選項選單和標題欄等。

The more you can consolidate styling and behavior across platforms,
the easier development and testing will be. 
The trick is to balance creating a unique experience with a
strong identity, while respecting the norms of each platform.

跨平臺樣式內容越多，開發和測試就越輕鬆。
在建構你的使用者體驗時，要注意平衡對它們的選擇，同時還要尊重各個平台的規範。

### Common idioms and norms to consider

### 需要考慮的常見平台行為習慣與規範

Take a quick look at a few specific norms and idioms
you might want to consider, and how you could approach
them in Flutter.

讓我們來快速瀏覽一下你可能需要考慮的規範和習慣，
瞭解一下在 Flutter 中如何實現它們。

#### Scrollbar appearance and behavior

#### 捲軸的外觀和行為

Desktop and mobile users expect scrollbars,
but they expect them to behave differently on different platforms.
Mobile users expect smaller scrollbars that only appear
while scrolling, whereas desktop users generally expect
omnipresent, larger scrollbars that they can click or drag. 

無論是桌面端還是移動端的使用者，都需要捲軸，
但他們對不同平台所期待的行為是不一樣的。
移動端的使用者希望捲軸小一些，只在滾動時出現，
而桌面端的使用者一般想要更大且一直顯示的捲軸，同時可以點選和拖動。

Flutter comes with a built-in `Scrollbar` widget that already
has support for adaptive colors and sizes according to the
current platform. The one tweak you might want to make is to
toggle `alwaysShown` when on a desktop platform:

Flutter 內建了 `Scrollbar` widget，會根據當前所在的平台自適應顏色和大小。
你可能會需要調整 `alwaysShown` 以在桌面平臺上一直顯示捲軸：

<?code-excerpt "lib/pages/adaptive_grid_page.dart (ScrollbarAlwaysShown)"?>
```dart
return Scrollbar(
  thumbVisibility: DeviceType.isDesktop,
  controller: _scrollController,
  child: GridView.count(
      controller: _scrollController,
      padding: EdgeInsets.all(Insets.extraLarge),
      childAspectRatio: 1,
      crossAxisCount: colCount,
      children: listChildren),
);
```

This subtle attention to detail can make your app feel more
comfortable on a given platform.

對這些細節的把握，可以讓你的應用在對應平臺上體驗更為良好。

#### Multi-select

#### 多選

Dealing with multi-select within a list is another area
with subtle differences across platforms: 

跨平臺的另一個存在差異的地方，是如何處理列表中的多選：

<?code-excerpt "lib/widgets/extra_widget_excerpts.dart (MultiSelectShift)"?>
```dart
static bool get isSpanSelectModifierDown =>
    isKeyDown({LogicalKeyboardKey.shiftLeft, LogicalKeyboardKey.shiftRight});
```

To perform a platform-aware check for control or command,
you can write something like this: 

要想監測不同平台的 Control 或 Command 鍵，你可以編寫以下的程式碼：

<?code-excerpt "lib/widgets/extra_widget_excerpts.dart (MultiSelectModifierDown)"?>
```dart
static bool get isMultiSelectModifierDown {
  bool isDown = false;
  if (Platform.isMacOS) {
    isDown = isKeyDown(
        {LogicalKeyboardKey.metaLeft, LogicalKeyboardKey.metaRight});
  } else {
    isDown = isKeyDown(
        {LogicalKeyboardKey.controlLeft, LogicalKeyboardKey.controlRight});
  }
  return isDown;
}
```

A final consideration for keyboard users is the **Select All** action.
If you have a large list of items of selectable items,
many of your keyboard users will expect that they can use
`Control+A` to select all the items. 

最後一項針對鍵盤使用者需要考慮的是 **全選** 操作。
如果你的列表裡有很多的可選擇內容，
可能你的許多使用者也會希望能使用 `Control+A` 選中所有內容。

##### Touch devices

##### 觸屏裝置

On touch devices, multi-selection is typically simplified,
with the expected behavior being similar to having the
`isMultiSelectModifier` down on the desktop.
You can select or deselect items using a single tap,
and will usually have a button to **Select All** or
**Clear** the current selection. 

在觸屏裝置上，多選操作通常會被簡化，
與在桌面上按下了 `isMultiSelectModifier`（多選按鈕）的行為類似。

How you handle multi-selection on different devices depends
on your specific use cases, but the important thing is to
make sure that you're offering each platform the best
interaction model possible.

在不同裝置上處理多選操作，取決於你的使用案例是否有區分，
但更重要的是為各個平台提供最好的互動模式。

#### Selectable text

#### 可選中的文字

A common expectation on the web (and to a lesser extent desktop)
is that most visible text can be selected with the mouse cursor.
When text is not selectable,
users on the web tend to have an adverse reaction.

對於 Web 平台（以及小部分的桌面平台）而言，大部分能看到的文字都是可以使用滑鼠選擇的。
如果不能選擇，使用者可能會感到不正常。

Luckily, this is easy to support with the [`SelectableText`][] widget: 

幸運的是，使用 [`SelectableText`][] 就可以很簡單地支援選擇：

<?code-excerpt "lib/widgets/extra_widget_excerpts.dart (SelectableText)"?>
```dart
return SelectableText('Select me!');
```

To support rich text, then use `TextSpan`: 

可以用 `TextSpan` 支援富文字：

<?code-excerpt "lib/widgets/extra_widget_excerpts.dart (RichTextSpan)"?>
```dart
return SelectableText.rich(
  TextSpan(
    children: [
      TextSpan(text: 'Hello'),
      TextSpan(text: 'Bold', style: TextStyle(fontWeight: FontWeight.bold)),
    ],
  ),
);
```

[`SelectableText`]: {{site.api}}/flutter/material/SelectableText-class.html

#### Title bars

#### 標題欄

On modern desktop applications, it’s common to customize
the title bar of your app window, adding a logo for
stronger branding or contextual controls to help save
vertical space in your main UI. 

在現代的桌面應用程式中，經常會有客製應用視窗的標題欄、新增 Logo 或者其他控制的需求，
能節省介面對於垂直空間的佔用。

![Samples of title bars]({{site.url}}/assets/images/docs/development/ui/layout/titlebar.png)

This isn't supported directly in Flutter, but you can use the
[`bits_dojo`][] package to disable the native title bars,
and replace them with your own.

Flutter 並沒有內建這樣的支援，但是你可以使用 [`bits_dojo`][] package
禁用標題欄，並且替換成自己的。

This package lets you add whatever widgets you want to the
`TitleBar` because it uses pure Flutter widgets under the hood.
This makes it easy to adapt the title bar as you navigate
to different sections of the app.

你可以利用這個 package 將任意 widget 應用在標題欄上，
因為它是基於 Flutter 的 widget 進行設定的。
如此一來，當你在應用內各個地方瀏覽時，標題欄都能以非常便捷的方式進行適配。

[`bits_dojo`]: {{site.github}}/bitsdojo/bitsdojo_window

#### Context menus and tooltips

#### 上下文選單和提示

On desktop, there are several interactions that
manifest as a widget shown in an overlay,
but with differences in how they’re triggered, dismissed,
and positioned:

在桌面平臺上，通常有幾種在疊加層中顯示的互動元件，它們各自有不同的觸發、關閉和定位方式：

* **Context menu**&mdash;Typically triggered by a right-click,
  a context menu is positioned close to the mouse,
  and is dismissed by clicking anywhere,
  selecting an option from the menu, or clicking outside it.

  **上下文選單**&mdash;&mdash;通常在右鍵單擊時顯示，上下文選單會顯示在滑鼠點選位置的附近，
  可以點選任意位置關閉、點選選項關閉或點選外部區域關閉。

* **Tooltip**&mdash;Typically triggered by hovering for
  200-400ms over an interactive element,
  a tooltip is usually anchored to a widget
  (as opposed to the mouse position) and is dismissed
  when the mouse cursor leaves that widget.

  **提示**&mdash;&mdash;提示通常會在互動元素上懸停 200-400 毫秒後出現，
  一般會錨定在 widget 上（與滑鼠位置相反），並在滑鼠移出元素後消失。

* **Popup panel (also known as flyout)**&mdash;Similar to a tooltip,
  a popup panel is usually anchored to a widget.
  The main difference is that panels are most often
  shown on a tap event, and they usually don’t hide
  themselves when the cursor leaves.
  Instead, panels are typically dismissed by clicking
  outside the panel or by pressing a **Close** or **Submit** button.

  **懸浮面板（浮出控制項）**&mdash;&mdash;懸浮面板與提示類似，通常會錨定在 widget 上。
  它與提示的區別是一般會在點選事件觸發時顯示，並且在滑鼠移出時不會自動消失。
  通常來說，點選外部區域或者 **關閉** 或 **提交** 按鈕時會關閉懸浮面板。

To show basic tooltips in Flutter,
use the built-in [`Tooltip`][] widget:

若你想在 Flutter 中顯示一個簡單的提示，
你可以使用 [`Tooltip`][] widget：

<?code-excerpt "lib/widgets/extra_widget_excerpts.dart (Tooltip)"?>
```dart
return const Tooltip(
  message: 'I am a Tooltip',
  child: Text('Hover over the text to show a tooltip.'),
);
```

Flutter also provides built-in context menus when editing
or selecting text. 

Flutter 同時也為編輯和選擇文字提供了內建的上下文選單。

To show more advanced tooltips, popup panels,
or create custom context menus,
you either use one of the available packages,
or build it yourself using a `Stack` or `Overlay`.

若你想顯示更進階的提示、懸浮面板或自訂的上下文選單，
你可以使用已有的 package，或利用 `Stack` 和 `Overlay` 進行建構。

Some available packages include: 

可以使用的 package 包括：

* [`context_menus`][]
* [`anchored_popups`][]
* [`flutter_portal`][]
* [`super_tooltip`][]
* [`custom_pop_up_menu`][]

While these controls can be valuable for touch users as accelerators,
they are essential for mouse users. These users expect
to right-click things, edit content in place,
and hover for more information. Failing to meet those expectations
can lead to disappointed users, or at least,
a feeling that something isn’t quite right.

儘管這些控制對於觸控使用者來說只是一種增強，但對於桌面使用者而言，它們是必不可少的。
桌面使用者會期望能夠右鍵點選其中一些內容，當場進行編輯，懸浮時檢視更多資訊。
若你的應用並不包含這類互動，相關的使用者群體可能會感到有些失望，或是認為某些地方不合理。

[`anchored_popups`]: {{site.pub}}/packages/anchored_popups
[`context_menus`]: {{site.pub}}/packages/context_menus
[`custom_pop_up_menu`]: {{site.pub}}/packages/custom_pop_up_menu
[`flutter_portal`]: {{site.pub}}/packages/flutter_portal
[`super_tooltip`]: {{site.pub}}/packages/super_tooltip
[`Tooltip`]: {{site.api}}/flutter/material/Tooltip-class.html

#### Horizontal button order

#### 按鈕的水平排列

On Windows, when presenting a row of buttons,
the confirmation button is placed at the start of
the row (left side). On all other platforms,
it’s the opposite. The confirmation button is
placed at the end of the row (right side). 

在 Windows 上展示一行按鈕時，確認按鈕會在一行的起始位置（左側）。
而在其他平臺上，則是完全相反的，確認按鈕顯示在末尾位置（右側）。

This can be easily handled in Flutter using the
`TextDirection` property on `Row`: 

在 Flutter 裡你可以很輕鬆地修改 `Row` 的 `TextDirection` 來達到這個效果：

<?code-excerpt "lib/widgets/ok_cancel_dialog.dart (RowTextDirection)"?>
```dart
TextDirection btnDirection =
    DeviceType.isWindows ? TextDirection.rtl : TextDirection.ltr;
return Row(
  children: [
    Spacer(),
    Row(
      textDirection: btnDirection,
      children: [
        DialogButton(
            label: 'Cancel',
            onPressed: () => Navigator.pop(context, false)),
        DialogButton(
            label: 'Ok', onPressed: () => Navigator.pop(context, true)),
      ],
    ),
  ],
);
```

![Sample of embedded image]({{site.url}}/assets/images/docs/development/ui/layout/embed_image1.png)

![Sample of embedded image]({{site.url}}/assets/images/docs/development/ui/layout/embed_image2.png)

#### Menu bar

#### 選單欄

Another common pattern on desktop apps is the menu bar.
On Windows and Linux, this menu lives as part of the Chrome title bar,
whereas on macOS, it’s located along the top of the primary screen. 

桌面平台有另一種常見的內容：選單欄。
在 Windows 和 Linux 上，Chrome 的選單欄整合在標題欄內，
而在 macOS 上，選單欄在主螢幕的頂部。

Currently, you can specify custom menu bar entries using
a prototype plugin, but it’s expected that this functionality will
eventually be integrated into the main SDK.

目前你可以使用一個原型外掛來指定選單欄的入口，
我們希望這個功能最終能合併到 SDK 中。

It’s worth mentioning that on Windows and Linux,
you can’t combine a custom title bar with a menu bar.
When you create a custom title bar,
you’re replacing the native one completely,
which means you also lose the integrated native menu bar.

值得一提的是，在 Windows 和 Linux 上，你無法將自訂的標題欄與選單欄整合在一起。
在建構自訂的標題欄時，實際上是替換了整個原生的標題欄，
意味著你也同時失去了原生的選單欄。

If you need both a custom title bar and a menu bar,
you can achieve that by implementing it in Flutter,
similar to a custom context menu.

如果你同時需要自訂的標題欄和選單欄，
你可以使用 Flutter 進行實現，類似於自訂的上下文選單。

#### Drag and drop

#### 拖放（拖動和放置）

One of the core interactions for both touch-based and
pointer-based inputs is drag and drop. Although this
interaction is expected for both types of input,
there are important differences to think about when
it comes to scrolling lists of draggable items.

拖放是基於觸控和指標的互動的一項核心。
雖然這兩種互動型別都需要拖放，但是在滑動整個包含可拖拽元素的列表時，
仍然需要考慮其中的差異。

Generally speaking, touch users expect to see drag handles
to differentiate draggable areas from scrollable ones,
or alternatively, to initiate a drag by using a long
press gesture. This is because scrolling and dragging
are both sharing a single finger for input.

一般來說，觸屏使用者希望看到可拖動的手柄，以區分拖動和滾動的範圍，
或者透過長按操作來進行拖動。
這是由於滑動和拖動操作都是由一個觸控點完成的。

Mouse users have more input options. They can use a wheel
or scrollbar to scroll, which generally eliminates the need
for dedicated drag handles. If you look at the macOS
Finder or Windows Explorer, you’ll see that they work
this way: you just select an item and start dragging.

滑鼠使用者有著不止一種輸入方式。他們可以使用滾輪和滑動條進行滑動，
這樣便不再專門需要操作手柄進行指示操作。
如果你使用過 macOS 的訪達和 Windows 的資源管理器，
你會看到它們在選中一個元素後，就可以開始拖動。

In Flutter, you can implement drag and drop in many
ways. Discussing specific implementations is outside
the scope of this article, but some high level options
are: 

在 Flutter 中，你可以用多種方式實現拖放。
但是我們不在本篇文章中討論這個話題，以下是一些更進階的選項：

在 Flutter 中，你可以用多種方式實現拖放。
但是我們不在本篇文章中討論這個話題，以下是一些更進階的選項：

* Use the [`Draggable`][] and [`DragTarget`][] APIs
  directly for a custom look and feel.

  使用 [`Draggable`][] 和 [`DragTarget`][] API 客製介面和互動。

* Hook into `onPan` gesture events,
  and move an object yourself within a parent `Stack`.

  監聽 `onPan` 手勢事件，利用 `Stack` 移動物件。

* Use one of the [pre-made list packages][] on pub.dev.  

  使用 pub.dev 上一些 [預先實現的 package][pre-made list packages]。

[`Draggable`]: {{site.api}}/flutter/widgets/Draggable-class.html
[`DragTarget`]: {{site.api}}/flutter/widgets/DragTarget-class.html
[pre-made list packages]: {{site.pub}}/packages?q=reorderable+list

### Educate yourself on basic usability principles

### 自身做到熟悉基本的可用性原則

Of course, this page doesn’t constitute an exhaustive list
of the things you might consider. The more operating systems,
form factors, and input devices you support,
the more difficult it becomes to spec out every permutation in design. 

當然，這篇文章並不代表你僅需要考慮這些內容。
針對平台設計的規範，會隨著你適配的平台、裝置外形和輸入裝置數量的增加而變得更為複雜。

Taking time to learn basic usability principles as a
developer empowers you to make better decisions,
reduces back-and-forth iterations with design during production,
and results in improved productivity with better outcomes.

作為開發人員，你應當花一些時間學習基本的可用性原則，幫助你做出更好的決策，
減少由設計細節帶來的返工時間消耗，從而提升自己的生產力，產出更好的結果。

Here are some resources to get you started: 

你可以從下列的資源開始學習：

* [Material guidelines on responsive UI layout][]

  [Material 響應式介面佈局指南][Material guidelines on responsive UI layout]

* [Material design for large screens][]

  [大屏裝置的 Material Design][Material design for large screens]

* [Build high quality apps (Android)][]

  [建構高品質的應用 (Android)][Build high quality apps (Android)]

* [UI design do's and don'ts (Apple)][]

  [UI 設計的注意事項 (Apple)][UI design do's and don'ts (Apple)]

* [Human interface guidelines (Apple)][]

  [人機介面指南 (Apple)][Human interface guidelines (Apple)]

* [Responsive design techniques (Microsoft)][]

  [響應式設計技術 (Microsoft)][Responsive design techniques (Microsoft)]

* [Machine sizes and breakpoints (Microsoft)][]

  [螢幕大小和分界點][Machine sizes and breakpoints (Microsoft)]

[Build high quality apps (Android)]: {{site.android-dev}}/quality
[Human interface guidelines (Apple)]: {{site.apple-dev}}/design/human-interface-guidelines/
[Material design for large screens]: {{site.material}}/blog/material-design-for-large-screens
[Machine sizes and breakpoints (Microsoft)]: https://docs.microsoft.com/en-us/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design
[Material guidelines on responsive UI layout]: {{site.material}}/archive/guidelines/layout/responsive-ui.html
[Responsive design techniques (Microsoft)]: https://docs.microsoft.com/en-us/windows/uwp/design/layout/responsive-design
[UI design do's and don'ts (Apple)]: {{site.apple-dev}}/design/tips/
