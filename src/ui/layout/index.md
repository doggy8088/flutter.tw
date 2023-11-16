---
title: Layouts in Flutter
title: Flutter 中的佈局
short-title: Layout
short-title: 佈局
description: Learn how Flutter's layout mechanism works and how to build a layout.
description: 瞭解 Flutter 的佈局機制和如何建構佈局。
tags: 使用者介面,Flutter UI,佈局
keywords: Flutter佈局核心介紹,核心機制,Flutter佈局
diff2html: true
---

{% assign api = site.api | append: '/flutter' -%}
{% capture code -%} {{site.repo.this}}/tree/{{site.branch}}/src/_includes/code {%- endcapture -%}
{% capture examples -%} {{site.repo.this}}/tree/{{site.branch}}/examples {%- endcapture -%}
{% capture demo -%} {{site.repo.gallery}}/tree/{{site.branch}}/lib/demos {%- endcapture -%}

<?code-excerpt path-base=""?>

<style>dl, dd { margin-bottom: 0; }</style>

{{site.alert.secondary}}

  <h4 class="no_toc">What's the point?</h4>

  <h4 class="no_toc">要點</h4>

  * Widgets are classes used to build UIs.

    Widgets 是用於建構 UI 的類別。

  * Widgets are used for both layout and UI elements.

    Widgets 可以用於佈局和展示 UI 元素。

  * Compose simple widgets to build complex widgets.

    透過組合簡單的 widgets 來建構複雜的 widgets。

{{site.alert.end}}

The core of Flutter's layout mechanism is widgets.
In Flutter, almost everything is a widget&mdash;even
layout models are widgets. The images, icons,
and text that you see in a Flutter app are all widgets.
But things you don't see are also widgets,
such as the rows, columns, and grids that arrange,
constrain, and align the visible widgets.

Flutter 佈局的核心機制是 widgets。
在 Flutter 中，幾乎所有東西都是 widget —— 甚至佈局模型都是 widgets。
你在 Flutter 應用程式中看到的圖像，圖示和文字都是 widgets。
此外不能直接看到的也是 widgets，
例如用來排列、限制和對齊可見 widgets 的行、列和網格。

You create a layout by composing widgets to build more complex widgets.
For example, the first screenshot below shows 3 icons with a label
under each one:

你可以透過組合 widgets 來建構更復雜的 widgets 來建立佈局。
比如，下面第一個截圖上有 3 個圖示，每個圖示下面都有一個標籤：

<div class="row mb-4">
  <div class="col-12 text-center">
    <img src='/assets/images/docs/ui/layout/lakes-icons.png' class="border mt-1 mb-1 mw-100" alt="Sample layout">
    <img src='/assets/images/docs/ui/layout/lakes-icons-visual.png' class="border mt-1 mb-1 mw-100" alt="Sample layout with visual debugging">
  </div>
</div>

The second screenshot displays the visual layout, showing a row of
3 columns where each column contains an icon and a label.

第二個截圖顯示了可視佈局，可以看到有一排三列，
其中每列包含一個圖示和一個標籤。

{{site.alert.note}}

  Most of the screenshots in this tutorial are displayed with
  `debugPaintSizeEnabled` set to true so you can see the visual layout.
  For more information, see
  [Debugging layout issues visually][], a section in
  [Using the Flutter inspector][].
  
  本課程中的大多數截圖都是將 `debugPaintSizeEnabled` 設定為 true 以後的效果，
  因此你可以看到可視佈局。更多資訊可以檢視文件中的 
  [視覺化除錯][Debugging layout issues visually]，
  它是 [除錯 Flutter 應用][Using the Flutter inspector] 中的一節。

{{site.alert.end}}

Here's a diagram of the widget tree for this UI:

以下是這個 UI 的 widget 樹形圖：

<img src='/assets/images/docs/ui/layout/sample-flutter-layout.png' class="mw-100" alt="Node tree">
{:.text-center}

Most of this should look as you might expect, but you might be wondering
about the containers (shown in pink). [`Container`][] is a widget class
that allows you to customize its child widget. Use a `Container` when
you want to add padding, margins, borders, or background color,
to name some of its capabilities.

圖上大部分應該和你預想的一樣，但你可能會疑惑 containers（圖上粉色顯示的）是什麼。
[`Container`][] 是一個 widget，允許你自訂其子 widget。
舉幾個例子，如果要新增 padding、margin、邊框或背景顏色，
你就可以用上 `Container` 了。

In this example, each [`Text`][] widget is placed in a `Container`
to add margins. The entire [`Row`][] is also placed in a
`Container` to add padding around the row.

在這個例子中，每個 [`Text`][] widget 都被放在一個 `Container` 以新增 padding。
整個 [`Row`][] 也被放在一個 `Container` 中，以便新增 padding。

The rest of the UI in this example is controlled by properties.
Set an [`Icon`][]'s color using its `color` property.
Use the `Text.style` property to set the font, its color, weight, and so on.
Columns and rows have properties that allow you to specify how their
children are aligned vertically or horizontally, and how much space
the children should occupy.

這個例子其餘部分的 UI 由屬性控制。透過 [`Icon`][] 的 `color` 屬性來設定它的顏色，
透過 `Text.style` 屬性來設定文字的字型、顏色、字重等等。
列和行有一些屬性可以讓你指定子項垂直或水平的對齊方式以及子項應占用的空間大小。

## Lay out a widget

## 佈局 widget

How do you lay out a single widget in Flutter? This section
shows you how to create and display a simple widget.
It also shows the entire code for a simple Hello World app.

如何在 Flutter 中佈局單個 widget？本節將介紹如何建立和顯示單個 widget。
本節還包括一個簡單的 Hello World app 的完整程式碼。

In Flutter, it takes only a few steps to put text, an icon,
or an image on the screen.

在 Flutter 中，只需幾步就可以在螢幕上顯示文字、圖示或圖像。

### 1. Select a layout widget

### 1. 選擇一個佈局 widget

Choose from a variety of [layout widgets][] based
on how you want to align or constrain the visible widget,
as these characteristics are typically passed on to the
contained widget.

根據你想要對齊或限制可見 widget 的方式從各種
[layout widgets][] 中進行選擇，
因為這些特性通常會傳遞它所給包含的 widget。

This example uses [`Center`][] which centers its content
horizontally and vertically.

本例使用將其內容水平和垂直居中的 [`Center`][]。

### 2. Create a visible widget

### 2. 建立一個可見 widget

For example, create a [`Text`][] widget:

舉個例子，建立一個 [`Text`][] widget：

<?code-excerpt "layout/base/lib/main.dart (text)" replace="/child: //g"?>
```dart
Text('Hello World'),
```

Create an [`Image`][] widget:

建立一個 [`Image`][] widget：

<?code-excerpt "layout/lakes/step5/lib/main.dart (Image-asset)" remove="/width|height/"?>
```dart
Image.asset(
  'images/lake.jpg',
  fit: BoxFit.cover,
),
```

Create an [`Icon`][] widget:

建立一個 [`Icon`][] widget：

<?code-excerpt "layout/lakes/step5/lib/main.dart (Icon)"?>
```dart
Icon(
  Icons.star,
  color: Colors.red[500],
),
```

### 3. Add the visible widget to the layout widget

### 3. 將可見 widget 新增到佈局 widget

<?code-excerpt path-base="layout/base"?>

All layout widgets have either of the following:

所有佈局 widgets 都具有以下任一項：

* A `child` property if they take a single child&mdash;for example,
  `Center` or `Container`

  一個 `child` 屬性，如果它們只包含一個子項 —— 例如 `Center` 和 `Container`

* A `children` property if they take a list of widgets&mdash;for example,
  `Row`, `Column`, `ListView`, or `Stack`.

  一個 `children` 屬性，如果它們包含多個子項 —— 例如 `Row`、`Column`、`ListView` 和 `Stack`

Add the `Text` widget to the `Center` widget:

將 `Text` widget 新增進 `Center` widget：

<?code-excerpt "lib/main.dart (centered-text)" replace="/body: //g"?>
```dart
const Center(
  child: Text('Hello World'),
),
```

### 4. Add the layout widget to the page

### 4. 將佈局 widget 新增到頁面

A Flutter app is itself a widget, and most widgets have a [`build()`][]
method. Instantiating and returning a widget in the app's `build()` method
displays the widget.

一個 Flutter app 本身就是一個 widget，
大多數 widgets 都有一個 [`build()`][] 方法，
在 app 的 `build()` 方法中例項化和返回一個 widget 會讓它顯示出來。

#### Material apps

#### 基於 Material 的應用

For a `Material` app, you can use a [`Scaffold`][] widget;
it provides a default banner, background color,
and has API for adding drawers, snack bars, and bottom sheets.
Then you can add the `Center` widget directly to the `body`
property for the home page.

對於 `Material` app，你可以使用 [`Scaffold`][] widget，
它提供預設的 banner 背景顏色，
還有用於新增抽屜、提示條和底部列表彈窗的 API。
你可以將 `Center` widget 直接新增到主頁 `body` 的屬性中。

<?code-excerpt path-base="layout/base"?>
<?code-excerpt "lib/main.dart (MyApp)" title?>
```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter layout demo',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Flutter layout demo'),
        ),
        body: const Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
```

{{site.alert.note}}

  The [Material library][] implements widgets that follow [Material
  Design][] principles. When designing your UI, you can exclusively use
  widgets from the standard [widgets library][], or you can use
  widgets from the Material library. You can mix widgets from both
  libraries, you can customize existing widgets,
  or you can build your own set of custom widgets.

  [Material 庫][Material library] 實現了一些遵循 [Material
  Design][] 原則的 widgets。在設計 UI 時，
  你可以只使用標準 [widgets 庫][widgets library] 中的 widgets，
  也可以使用 Material library 中的 widgets。
  你可以混合來自兩個庫的 widgets，
  可以自訂現有 widgets，也可以建構自己的一組自訂 widgets。

{{site.alert.end}}

#### Non-Material apps

#### 非 Material apps

For a non-Material app, you can add the `Center` widget to the app's
`build()` method:

對於非 Material app，你可以將 `Center` widget 新增到 app 的 `build()` 方法裡：

<?code-excerpt path-base="layout/non_material"?>
<?code-excerpt "lib/main.dart (MyApp)" title?>
```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: const BoxDecoration(color: Colors.white),
      child: const Center(
        child: Text(
          'Hello World',
          textDirection: TextDirection.ltr,
          style: TextStyle(
            fontSize: 32,
            color: Colors.black87,
          ),
        ),
      ),
    );
  }
}
```

By default a non-Material app doesn't include an `AppBar`, title,
or background color. If you want these features in a non-Material app,
you have to build them yourself. This app changes the background
color to white and the text to dark grey to mimic a Material app.

預設情況下，非 Material app 不包含 `AppBar`、標題和背景顏色。
如果你希望在非 Material app 中使用這些功能，則必須自己建構它們。
以上 app 將背景顏色更改為白色，將文字更改為深灰色來模擬一個 Material app。

<div class="row">
<div class="col-md-6" markdown="1">
  That's it! When you run the app, you should see _Hello World_.

  完成! 啟動這個 app，你應該能看到 _Hello World_。

  App source code:

  App 原始碼:

  - [Material app]({{examples}}/layout/base)

  - [Non-Material app]({{examples}}/layout/non_material)

    [非 Material app]({{examples}}/layout/non_material)

</div>
<div class="col-md-6">
  {% include docs/app-figure.md img-class="site-mobile-screenshot border w-75"
      image="ui/layout/hello-world.png" alt="Hello World" %}
</div>
</div>

<hr>

## Lay out multiple widgets vertically and horizontally

## 橫向或縱向佈局多個 widgets

<?code-excerpt path-base=""?>

One of the most common layout patterns is to arrange
widgets vertically or horizontally. You can use a
`Row` widget to arrange widgets horizontally,
and a `Column` widget to arrange widgets vertically.

最常見的佈局模式之一是垂直或水平 widgets。
你可以使用 Row widget 水平排列 widgets，
使用 Column widget 垂直排列 widgets。

{{site.alert.secondary}}

  <h4 class="no_toc">What's the point?</h4>

  <h4 class="no_toc">要點</h4>

  * `Row` and `Column` are two of the most commonly used layout patterns.

    `Row` 和 `Column` 是兩種最常用的佈局模式。

  * `Row` and `Column` each take a list of child widgets.

    `Row` 和 `Column` 每個都有一個子 widgets 列表。

  * A child widget can itself be a `Row`, `Column`,
    or other complex widget.

    一個子 widget 本身可以是 `Row`、`Column` 或其他複雜 widget。

  * You can specify how a `Row` or `Column` aligns its children,
    both vertically and horizontally.

    可以指定 `Row` 或 `Column` 如何在垂直和水平方向上對齊其子項。

  * You can stretch or constrain specific child widgets.

    可以拉伸或限制特定的子 widgets。

  * You can specify how child widgets use the `Row`'s or
    `Column`'s available space.

    可以指定子 widgets 如何佔用 `Row` 或 `Column` 的可用空間。

{{site.alert.end}}

To create a row or column in Flutter, you add a list of children
widgets to a [`Row`][] or [`Column`][] widget. In turn,
each child can itself be a row or column, and so on.
The following example shows how it is possible to nest rows or
columns inside of rows or columns.

要在 Flutter 中建立行或列，可以將子 widgets 列表新增到
[`Row`][] 或 [`Column`][] widget 中。
反過來，每個子項本身可以是一行或一列，依此類推。
以下範例示範瞭如何在行或列中巢狀(Nesting)行或列。

This layout is organized as a `Row`. The row contains two children:
a column on the left, and an image on the right:

這個佈局被組織為 `Row`。這一行包含兩個子項：左側的列和右側的圖像：

<img src='/assets/images/docs/ui/layout/pavlova-diagram.png' class="mw-100"
    alt="Screenshot with callouts showing the row containing two children">

The left column's widget tree nests rows and columns.

左側列的 widget 樹巢狀(Nesting)著行和列。

<img src='/assets/images/docs/ui/layout/pavlova-left-column-diagram.png' class="mw-100"
    alt="Diagram showing a left column broken down to its sub-rows and sub-columns">

You'll implement some of Pavlova's layout code in
[Nesting rows and columns](#nesting-rows-and-columns).

你將在 [巢狀(Nesting)行和列](#nesting-rows-and-columns) 中實現蛋糕介紹範例的一些佈局程式碼。

{{site.alert.note}}

  `Row` and `Column` are basic primitive widgets for horizontal
  and vertical layouts&mdash;these low-level widgets allow for maximum
  customization. Flutter also offers specialized, higher level widgets
  that might be sufficient for your needs. For example,
  instead of `Row` you might prefer [`ListTile`][],
  an easy-to-use widget with properties for leading and trailing icons,
  and up to 3 lines of text.  Instead of Column, you might prefer
  [`ListView`][], a column-like layout that automatically scrolls
  if its content is too long to fit the available space.
  For more information, see [Common layout widgets][].

  Row 和 Column 是水平和垂直佈局的基本原始 widgets —— 這些低階 widgets 允許最大程度的自訂。
  Flutter 還提供專門的、更高級別的 widgets，可能可以直接滿足需求。
  例如，和 Row 相比你可能更喜歡 [`ListTile`][]，
  這是一個易於使用的 widget，有屬性可以設定頭尾圖示，最多可以顯示 3 行文字；
  和 Column 相比你也可能更喜歡 [`ListView`][]，
  這是一種類似於列的佈局，但如果其內容太長導致可用空間不夠容納時會自動滾動。
  更多資訊可以檢視 [通用佈局 widgets][Common layout widgets]。

{{site.alert.end}}

### Aligning widgets

### 對齊 widgets

You control how a row or column aligns its children using the
`mainAxisAlignment` and `crossAxisAlignment` properties.
For a row, the main axis runs horizontally and the cross axis runs
vertically. For a column, the main axis runs vertically and the cross
axis runs horizontally.

你可以使用 `mainAxisAlignment` 和 `crossAxisAlignment`
屬性控制行或列如何對齊其子項。
對於一行來說，主軸水平延伸，交叉軸垂直延伸。
對於一列來說，主軸垂直延伸，交叉軸水平延伸。

<div class="mb-2 text-center">
  <img src='/assets/images/docs/ui/layout/row-diagram.png' class="mb-2 mw-100"
      alt="Diagram showing the main axis and cross axis for a row">
  <img src='/assets/images/docs/ui/layout/column-diagram.png' class="mb-2 mr-2 ml-2 mw-100"
      alt="Diagram showing the main axis and cross axis for a column">
</div>

The [`MainAxisAlignment`][] and [`CrossAxisAlignment`][]
enums offer a variety of constants for controlling alignment.

[`MainAxisAlignment`][] 和 [`CrossAxisAlignment`][] 這兩個列舉
提供了很多用於控制對齊的常量。

{{site.alert.note}}

  When you add images to your project,
  you need to update the `pubspec.yaml` file to access
  them&mdash;this example uses `Image.asset` to display
  the images.  For more information, see this example's
  [`pubspec.yaml` file][] or [Adding assets and images][].
  You don't need to do this if you're referencing online
  images using `Image.network`.

  當你將圖像新增到專案中時，你需要更新 `pubspec.yaml` 檔案來存取它們
  —— 本例使用 `Image.asset` 來顯示圖像。
  更多資訊可以檢視本例的 [pubspec.yaml 檔案][`pubspec.yaml` file]，
  或文件：[新增資源和圖片][Adding assets and images]。
  如果你正在使用 `Image.network` 參考線上圖像，則不需要這些操作。

{{site.alert.end}}

In the following example, each of the 3 images is 100 pixels wide.
The render box (in this case, the entire screen)
is more than 300 pixels wide, so setting the main axis
alignment to `spaceEvenly` divides the free horizontal
space evenly between, before, and after each image.

在以下範例中，3 個圖像每個都是是 100 畫素寬。
渲染框（在本例中是整個螢幕）寬度超過 300 畫素，
因此設定主軸對齊方式為 `spaceEvenly` 會將
空餘空間在每個圖像之間、之前和之後均勻地劃分。

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/row_column/lib/main.dart (Row)" replace="/Row/[!$&!]/g"?>
  {% prettify dart context="html" %}
  [!Row!](
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      Image.asset('images/pic1.jpg'),
      Image.asset('images/pic2.jpg'),
      Image.asset('images/pic3.jpg'),
    ],
  );
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  <img src='/assets/images/docs/ui/layout/row-spaceevenly-visual.png' class="mw-100" alt="Row with 3 evenly spaced images">

  **App source:** [row_column]({{examples}}/layout/row_column)

  **App 原始碼:** [row_column]({{examples}}/layout/row_column)
</div>
</div>

Columns work the same way as rows. The following example shows a column
of 3 images, each is 100 pixels high. The height of the render box
(in this case, the entire screen) is more than 300 pixels, so
setting the main axis alignment to `spaceEvenly` divides the free vertical
space evenly between, above, and below each image.

列的工作方式與行的工作方式相同。
以下範例展示了包含 3 個圖像的列，每個圖像的高度為 100 畫素。
渲染框（在本例中是整個螢幕）高度超過 300 畫素，
因此設定主軸對齊方式為 `spaceEvenly` 會將空餘空間
在每個圖像之間、之上和之下均勻地劃分。

<div class="row">
<div class="col-lg-8" markdown="1">
  <?code-excerpt "layout/row_column/lib/main.dart (Column)" replace="/Column/[!$&!]/g"?>
  {% prettify dart context="html" %}
  [!Column!](
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      Image.asset('images/pic1.jpg'),
      Image.asset('images/pic2.jpg'),
      Image.asset('images/pic3.jpg'),
    ],
  );
  {% endprettify %}

  **App source:** [row_column]({{examples}}/layout/row_column)

  **App 原始碼:** [row_column]({{examples}}/layout/row_column)
</div>
<div class="col-lg-4 text-center">
  <img src='/assets/images/docs/ui/layout/column-visual.png' class="mb-4" height="250px"
      alt="Column showing 3 images spaced evenly">
</div>
</div>

### Sizing widgets

### 調整 widgets 大小

When a layout is too large to fit a device, a yellow
and black striped pattern appears along the affected edge.
Here is an [example][sizing] of a row that is too wide:

當某個佈局太大而超出螢幕時，受影響的邊緣會出現黃色和黑色條紋圖案。
這裡有一個行太寬的 [例子][sizing]：

<img src='/assets/images/docs/ui/layout/layout-too-large.png' class="mw-100" alt="Overly-wide row">
{:.text-center}

Widgets can be sized to fit within a row or column by using the
[`Expanded`][] widget. To fix the previous example where the
row of images is too wide for its render box,
wrap each image with an `Expanded` widget.

透過使用 [`Expanded`][] widget，可以調整 widgets 的大小以適合行或列。
要修復上一個圖像行對其渲染框來說太寬的範例，
可以用 `Expanded` widget 把每個圖像包起來。

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/sizing/lib/main.dart (expanded-images)" replace="/Expanded/[!$&!]/g"?>
  {% prettify dart context="html" %}
  Row(
    crossAxisAlignment: CrossAxisAlignment.center,
    children: [
      [!Expanded!](
        child: Image.asset('images/pic1.jpg'),
      ),
      [!Expanded!](
        child: Image.asset('images/pic2.jpg'),
      ),
      [!Expanded!](
        child: Image.asset('images/pic3.jpg'),
      ),
    ],
  );
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  <img src='/assets/images/docs/ui/layout/row-expanded-2-visual.png' class="mw-100"
      alt="Row of 3 images that are too wide, but each is constrained to take only 1/3 of the space">

  **App source:** [sizing]({{examples}}/layout/sizing)

  **App 原始碼:** [sizing]({{examples}}/layout/sizing)
</div>
</div>

Perhaps you want a widget to occupy twice as much space as its
siblings. For this, use the `Expanded` widget `flex` property,
an integer that determines the flex factor for a widget.
The default flex factor is 1. The following code sets
the flex factor of the middle image to 2:

也許你想要一個 widget 佔用的空間是兄弟項的兩倍。
為了達到這個效果，可以使用 `Expanded` widget 的 `flex` 屬性，
這是一個用來確定 widget 的彈性係數的整數。
預設的彈性係數為 1，以下程式碼將中間圖像的彈性係數設定為 2：

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/sizing/lib/main.dart (expanded-images-with-flex)" replace="/flex.*/[!$&!]/g"?>
  {% prettify dart context="html" %}
  Row(
    crossAxisAlignment: CrossAxisAlignment.center,
    children: [
      Expanded(
        child: Image.asset('images/pic1.jpg'),
      ),
      Expanded(
        [!flex: 2,!]
        child: Image.asset('images/pic2.jpg'),
      ),
      Expanded(
        child: Image.asset('images/pic3.jpg'),
      ),
    ],
  );
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  <img src='/assets/images/docs/ui/layout/row-expanded-visual.png' class="mw-100"
      alt="Row of 3 images with the middle image twice as wide as the others">

  **App source:** [sizing]({{examples}}/layout/sizing)

  **App 原始碼:** [sizing]({{examples}}/layout/sizing)
</div>
</div>

[sizing]: {{examples}}/layout/sizing

### Packing widgets

### 組合 widgets

By default, a row or column occupies as much space along its main axis
as possible, but if you want to pack the children closely together,
set its `mainAxisSize` to `MainAxisSize.min`. The following example
uses this property to pack the star icons together.

預設情況下，行或列沿其主軸會佔用盡可能多的空間，
但如果要將子項緊密組合在一起，
請將其 `mainAxisSize` 設定為 `MainAxisSize.min`。
以下範例使用此屬性將星形圖示組合在一起。

<div class="row">
<div class="col-lg-8">
  <?code-excerpt "layout/pavlova/lib/main.dart (stars)" replace="/mainAxisSize.*/[!$&!]/g; /\w+ \w+ = //g; /;//g"?>
  {% prettify dart context="html" %}
  Row(
    [!mainAxisSize: MainAxisSize.min,!]
    children: [
      Icon(Icons.star, color: Colors.green[500]),
      Icon(Icons.star, color: Colors.green[500]),
      Icon(Icons.star, color: Colors.green[500]),
      const Icon(Icons.star, color: Colors.black),
      const Icon(Icons.star, color: Colors.black),
    ],
  )
  {% endprettify %}
</div>
<div class="col-lg-4" markdown="1">
  <img src='/assets/images/docs/ui/layout/packed.png' class="border mw-100"
      alt="Row of 5 stars, packed together in the middle of the row">

  **App source:** [pavlova]({{examples}}/layout/pavlova)

  **App 原始碼:** [pavlova]({{examples}}/layout/pavlova)
</div>
</div>

### Nesting rows and columns

### 巢狀(Nesting)行和列

The layout framework allows you to nest rows and columns
inside of rows and columns as deeply as you need.
Let's look at the code for the outlined
section of the following layout:

佈局框架允許你根據需要在行和列內巢狀(Nesting)行和列。
讓我們看看以下佈局的概述部分的程式碼：

<img src='/assets/images/docs/ui/layout/pavlova-large-annotated.png' class="border mw-100"
    alt="Screenshot of the pavlova app, with the ratings and icon rows outlined in red">
{:.text-center}

The outlined section is implemented as two rows. The ratings row contains
five stars and the number of reviews. The icons row contains three
columns of icons and text.

概述的部分實現為兩行，評級一行包含五顆星和評論的數量，
圖示一行包含由圖示與文字組成的三列。

The widget tree for the ratings row:

以下是評級行的 widget 樹形圖：

<img src='/assets/images/docs/ui/layout/widget-tree-pavlova-rating-row.png' class="mw-100" alt="Ratings row widget tree">
{:.text-center}

The `ratings` variable creates a row containing a smaller row
of 5 star icons, and text:

`ratings` 變數建立了一個行，
其中包含較小的由 5 個星形圖示和文字組成的一行：

<?code-excerpt "layout/pavlova/lib/main.dart (ratings)" replace="/ratings/[!$&!]/g"?>
```dart
var stars = Row(
  mainAxisSize: MainAxisSize.min,
  children: [
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.green[500]),
    const Icon(Icons.star, color: Colors.black),
    const Icon(Icons.star, color: Colors.black),
  ],
);

final [!ratings!] = Container(
  padding: const EdgeInsets.all(20),
  child: Row(
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      stars,
      const Text(
        '170 Reviews',
        style: TextStyle(
          color: Colors.black,
          fontWeight: FontWeight.w800,
          fontFamily: 'Roboto',
          letterSpacing: 0.5,
          fontSize: 20,
        ),
      ),
    ],
  ),
);
```

{{site.alert.tip}}

  To minimize the visual confusion that can result from
  heavily nested layout code, implement pieces of the UI
  in variables and functions.

  為了最大限度地減少高度巢狀(Nesting)的佈局程式碼可能導致的視覺混亂，
  可以在變數和函式中實現 UI 的各個部分。

{{site.alert.end}}

The icons row, below the ratings row, contains 3 columns;
each column contains an icon and two lines of text,
as you can see in its widget tree:

評級行下方的圖示行包含 3 列，每列包含一個圖示和兩行文字，
你可以在其 widget 樹中看到：

<img src='/assets/images/docs/ui/layout/widget-tree-pavlova-icon-row.png' class="mw-100" alt="Icon widget tree">
{:.text-center}

The `iconList` variable defines the icons row:

`iconList` 變數定義了圖示行:

<?code-excerpt "layout/pavlova/lib/main.dart (iconList)" replace="/iconList/[!$&!]/g"?>
```dart
const descTextStyle = TextStyle(
  color: Colors.black,
  fontWeight: FontWeight.w800,
  fontFamily: 'Roboto',
  letterSpacing: 0.5,
  fontSize: 18,
  height: 2,
);

// DefaultTextStyle.merge() allows you to create a default text
// style that is inherited by its child and all subsequent children.
final [!iconList!] = DefaultTextStyle.merge(
  style: descTextStyle,
  child: Container(
    padding: const EdgeInsets.all(20),
    child: Row(
      mainAxisAlignment: MainAxisAlignment.spaceEvenly,
      children: [
        Column(
          children: [
            Icon(Icons.kitchen, color: Colors.green[500]),
            const Text('PREP:'),
            const Text('25 min'),
          ],
        ),
        Column(
          children: [
            Icon(Icons.timer, color: Colors.green[500]),
            const Text('COOK:'),
            const Text('1 hr'),
          ],
        ),
        Column(
          children: [
            Icon(Icons.restaurant, color: Colors.green[500]),
            const Text('FEEDS:'),
            const Text('4-6'),
          ],
        ),
      ],
    ),
  ),
);
```

The `leftColumn` variable contains the ratings and icons rows,
as well as the title and text that describes the Pavlova:

`leftColumn` 變數包含評級和圖示行，
以及蛋糕介紹的標題和文字：

<?code-excerpt "layout/pavlova/lib/main.dart (leftColumn)" replace="/leftColumn/[!$&!]/g"?>
```dart
final [!leftColumn!] = Container(
  padding: const EdgeInsets.fromLTRB(20, 30, 20, 20),
  child: Column(
    children: [
      titleText,
      subTitle,
      ratings,
      iconList,
    ],
  ),
);
```

The left column is placed in a `SizedBox` to constrain its width.
Finally, the UI is constructed with the entire row (containing the
left column and the image) inside a `Card`.

左列放置在 `Container` 中以限制其寬度。最後，UI 由 `Card` 內的整行（包含左列和圖像）構成。

The [Pavlova image][] is from [Pixabay][].
You can embed an image from the net using `Image.network()` but,
for this example, the image is saved to an images directory in the project,
added to the [pubspec file][], and accessed using `Images.asset()`.
For more information, see [Adding assets and images][].

[蛋糕圖片][Pavlova image] 來自 [Pixabay][] 網站。
你可以使用 `Image.network()` 從網路上參考圖像，
但是在本例圖像將儲存到專案中的一個圖像目錄中，
新增到 [pubspec 檔案][pubspec file]，並使用 `Images.asset()` 存取。
更多資訊可以檢視文件中關於 [新增資源和圖片][Adding assets and images] 這一章。

<?code-excerpt "layout/pavlova/lib/main.dart (body)"?>
```dart
body: Center(
  child: Container(
    margin: const EdgeInsets.fromLTRB(0, 40, 0, 30),
    height: 600,
    child: Card(
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          SizedBox(
            width: 440,
            child: leftColumn,
          ),
          mainImage,
        ],
      ),
    ),
  ),
),
```

{{site.alert.tip}}

  The Pavlova example runs best horizontally on a wide device,
  such as a tablet.  If you are running this example in the iOS simulator,
  you can select a different device using the **Hardware > Device** menu.
  For this example, we recommend the iPad Pro.
  You can change its orientation to landscape mode using
  **Hardware > Rotate**. You can also change the size of the
  simulator window (without changing the number of logical pixels)
  using **Window > Scale**.

  蛋糕介紹的例子在寬屏裝置（如平板電腦）上橫向執行效果最佳。
  如果在 iOS 模擬器中執行這個範例，
  可以使用 **Hardware > Device** 選單選擇不同的裝置。
  在本例中，我們推薦 iPad Pro。
  你可以使用 **Hardware > Rotate** 將其方向更改為橫向模式。
  你還可以使用 **Window > Scale** 更改模擬器視窗的大小
  （不改變邏輯畫素的數量）。

{{site.alert.end}}

**App source:** [pavlova]({{examples}}/layout/pavlova)

**App 原始碼:** [pavlova]({{examples}}/layout/pavlova)

[Pavlova image]: https://pixabay.com/en/photos/pavlova
[蛋糕圖片]: https://pixabay.com/en/photos/pavlova
[Pixabay]: https://pixabay.com/en/photos/pavlova

<hr>

## Common layout widgets

## 通用佈局 widgets

Flutter has a rich library of layout widgets.
Here are a few of those most commonly used.
The intent is to get you up and running as quickly as possible,
rather than overwhelm you with a complete list.
For information on other available widgets,
refer to the [Widget catalog][],
or use the Search box in the [API reference docs][].
Also, the widget pages in the API docs often make suggestions
about similar widgets that might better suit your needs.

Flutter 有一個豐富的佈局 widget 儲存庫，裡面有很多經常會用到的佈局 widget。
目的是為了讓你更快的上手，而不是被一個完整的列表嚇跑。
關於其他有用的 widget 的資訊，可以參考 [Widget 目錄][Widget catalog]，
或者使用 [API 參考文件][API reference docs] 中的搜尋框。
而且，API 文件中的 widget 頁面中經常會給出一些
關於相似的 widget 哪個會更適合你的建議。

The following widgets fall into two categories: standard widgets
from the [widgets library][], and specialized widgets from the
[Material library][]. Any app can use the widgets library but
only Material apps can use the Material Components library.

下面的 widget 會分為兩類：[widgets 庫][widgets library] 中的標準 widgets 和
[Material 庫][Material library] 中的 widgets。
任何 app 都可以使用 widget 庫，
但是 Material 庫中的元件只能在 Material app 中使用。

### Standard widgets

### 標準 widgets

* [`Container`](#container): Adds padding, margins, borders,
  background color, or other decorations to a widget.

  [`Container`](#container)：向 widget 增加 
  padding、margins、borders、background color 或者其他的“裝飾”。

* [`GridView`](#gridview): Lays widgets out as a scrollable grid.

  [`GridView`](#gridview)：將 widget 展示為一個可滾動的網格。

* [`ListView`](#listview): Lays widgets out as a scrollable list.

  [`ListView`](#listview)：將 widget 展示為一個可滾動的列表。

* [`Stack`](#stack): Overlaps a widget on top of another.

  [`Stack`](#stack)：將 widget 覆蓋在另一個的上面。

### Material widgets

* [`Card`](#card): Organizes related info into a box with
  rounded corners and a drop shadow.

  [`Card`](#card)：將相關資訊整理到一個有圓角和陰影的盒子中。

* [`ListTile`](#listtile): Organizes up to 3 lines of text,
  and optional leading and trailing icons, into a row.

  [`ListTile`](#listtile)：將最多三行的文字、
  可選的導語以及後面的圖示組織在一行中。

### Container

Many layouts make liberal use of [`Container`][]s to separate
widgets using padding, or to add borders or margins.
You can change the device's background by placing the
entire layout into a `Container` and changing its background
color or image.

許多佈局都可以隨意的用 [`Container`][]，它可以將使用了 padding 或者
增加了 borders/margins 的 widget 分開。
你可以透過將整個佈局放到一個 `Container` 中，
並且改變它的背景色或者圖片，來改變裝置的背景。

<div class="row">
<div class="col-lg-6" markdown="1">
  <h4 class="no_toc">Summary (Container)</h4>

  <h4 class="no_toc">摘要 (Container)</h4>

  * Add padding, margins, borders

    增加 padding、margins、borders

  * Change background color or image

    改變背景色或者圖片

  * Contains a single child widget, but that child can be a Row,
    Column, or even the root of a widget tree

    只包含一個子 widget，但是這個子 widget 可以是行、列或者是 widget 樹的根 widget

</div>
<div class="col-lg-6 text-center">
  <img src='/assets/images/docs/ui/layout/margin-padding-border.png' class="mb-4 mw-100"
      width="230px"
      alt="Diagram showing: margin, border, padding, and content">
</div>
</div>

#### Examples (Container)
{:.no_toc}

#### 範例 (Container)
{:.no_toc}

This layout consists of a column with two rows, each containing
2 images. A [`Container`][] is used to change the background color
of the column to a lighter grey.

這個佈局包含一個有兩行的列，每行有兩張圖片。
[`Container`][] 用來將列的背景色變為淺灰色。

<div class="row">
<div class="col-lg-7">
  <?code-excerpt "layout/container/lib/main.dart (column)" replace="/\bContainer/[!$&!]/g;"?>
  {% prettify dart context="html" %}
  Widget _buildImageColumn() {
    return [!Container!](
      decoration: const BoxDecoration(
        color: Colors.black26,
      ),
      child: Column(
        children: [
          _buildImageRow(1),
          _buildImageRow(3),
        ],
      ),
    );
  }
  {% endprettify %}
</div>
<div class="col-lg-5 text-center">
  <img src='/assets/images/docs/ui/layout/container.png' class="mb-4 mw-100" width="230px"
      alt="Screenshot showing 2 rows, each containing 2 images">
</div>
</div>

A `Container` is also used to add a rounded border and margins
to each image:

`Container` 還用來為每個圖片新增圓角和外邊距：

<?code-excerpt "layout/container/lib/main.dart (row)" replace="/\bContainer/[!$&!]/g;"?>
```dart
Widget _buildDecoratedImage(int imageIndex) => Expanded(
      child: [!Container!](
        decoration: BoxDecoration(
          border: Border.all(width: 10, color: Colors.black38),
          borderRadius: const BorderRadius.all(Radius.circular(8)),
        ),
        margin: const EdgeInsets.all(4),
        child: Image.asset('images/pic$imageIndex.jpg'),
      ),
    );

Widget _buildImageRow(int imageIndex) => Row(
      children: [
        _buildDecoratedImage(imageIndex),
        _buildDecoratedImage(imageIndex + 1),
      ],
    );
```

You can find more `Container` examples in the [tutorial][]
and the Flutter Gallery ([running app][], [repo][]).

你可以在 [佈局建構課程][tutorial] 和 [Flutter Gallery][running app]
中可以發現更多關於 `Container` 的例子。

**App source:** [container]({{examples}}/layout/container)

**App 原始碼:** [container]({{examples}}/layout/container)

<hr>

### GridView

Use [`GridView`][] to lay widgets out as a two-dimensional
list. `GridView` provides two pre-fabricated lists,
or you can build your own custom grid. When a `GridView`
detects that its contents are too long to fit the render box,
it automatically scrolls.

使用 [`GridView`][] 將 widget 作為二維列表展示。
`GridView` 提供兩個預製的列表，或者你可以自訂網格。
當 `GridView` 檢測到內容太長而無法適應渲染盒時，
它就會自動支援滾動。

#### Summary (GridView)
{:.no_toc}

#### 摘要 (GridView)
{:.no_toc}

* Lays widgets out in a grid

  在網格中使用 widget

* Detects when the column content exceeds the render box
  and automatically provides scrolling

  當列的內容超出渲染容器的時候，它會自動支援滾動。

* Build your own custom grid, or use one of the provided grids:

  建立自訂的網格，或者使用下面提供的網格的其中一個：

  * `GridView.count` allows you to specify the number of columns

    `GridView.count` 允許你制定列的數量

  * `GridView.extent` allows you to specify the maximum pixel
  width of a tile
  
    `GridView.extent` 允許你制定單元格的最大寬度

{% comment %}
* Use `MediaQuery.of(context).orientation` to create a grid
  that changes its layout depending on whether the device
  is in landscape or portrait mode.
  
  使用 `MediaQuery.of(context).orientation` 建立一個網格，
  它會根據裝置處於寬屏模式還是豎屏模式來改變佈局。

{% endcomment %}

{{site.alert.note}}
  When displaying a two-dimensional list where it's important which
  row and column a cell occupies (for example,
  it's the entry in the "calorie" column for the "avocado" row), use
  [`Table`][] or [`DataTable`][].
  
  當展示二維列表中的單元格所在的行和列的位置很重要的
  （例如，它是 “calorie” 行和 “avocado” 列的條目）的時候，
  使用 [`Table`][] 或者 [`DataTable`][].。

{{site.alert.end}}

#### Examples (GridView)
{:.no_toc}

#### 範例 (GridView)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  <img src='/assets/images/docs/ui/layout/gridview-extent.png' class="mw-100" alt="A 3-column grid of photos">
  {:.text-center}

  Uses `GridView.extent` to create a grid with tiles a maximum
  150 pixels wide.

  使用 `GridView.extent` 建立一個最大寬度為 150 畫素的網格。

  **App source:** [grid_and_list]({{examples}}/layout/grid_and_list)

  **App 原始碼：** [grid_and_list]({{examples}}/layout/grid_and_list)

</div>
<div class="col-lg-6" markdown="1">
  <img src='/assets/images/docs/ui/layout/gridview-count-flutter-gallery.png' class="mw-100"
      alt="A 2 column grid with footers">
  {:.text-center}

  Uses `GridView.count` to create a grid that's 2 tiles
  wide in portrait mode, and 3 tiles wide in landscape mode.
  The titles are created by setting the `footer` property for
  each [`GridTile`][].

  使用 `GridView.count` 建立一個網格，它在豎屏模式下有兩行，在橫屏模式下有三行。
  可以透過為每個 [`GridTile`][] 設定 `footer` 屬性來建立標題。

  **Dart code:** [grid_list_demo.dart]({{demo}}/material/grid_list_demo.dart)
  from the [Flutter Gallery][repo]

  **Dart 程式碼：** [Flutter Gallery][repo] 中的
  [grid_list_demo.dart]({{demo}}/material/grid_list_demo.dart)

</div>
</div>

<?code-excerpt "layout/grid_and_list/lib/main.dart (grid)" replace="/\GridView/[!$&!]/g;"?>
```dart
Widget _buildGrid() => [!GridView!].extent(
    maxCrossAxisExtent: 150,
    padding: const EdgeInsets.all(4),
    mainAxisSpacing: 4,
    crossAxisSpacing: 4,
    children: _buildGridTileList(30));

// The images are saved with names pic0.jpg, pic1.jpg...pic29.jpg.
// The List.generate() constructor allows an easy way to create
// a list when objects have a predictable naming pattern.
List<Container> _buildGridTileList(int count) => List.generate(
    count, (i) => Container(child: Image.asset('images/pic$i.jpg')));
```

<hr>

### ListView

[`ListView`][], a column-like widget, automatically
provides scrolling when its content is too long for
its render box.

[`ListView`]({{api}}/widgets/ListView-class.html)，一個和列很相似的 widget，
當內容長於自己的渲染盒時，就會自動支援滾動。

#### Summary (ListView)
{:.no_toc}

#### 摘要 (ListView)
{:.no_toc}

* A specialized [`Column`][] for organizing a list of boxes

  一個用來組織盒子中列表的專用 [`Column`][]

* Can be laid out horizontally or vertically

  可以水平或者垂直佈局

* Detects when its content won't fit and provides scrolling

  當監測到空間不足時，會提供滾動

* Less configurable than `Column`, but easier to use and
  supports scrolling

  比 `Column` 的配置少，使用更容易，並且支援滾動

#### Examples (ListView)
{:.no_toc}

#### 範例 (ListView)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  <img src='/assets/images/docs/ui/layout/listview.png' class="border mw-100"
      alt="ListView containing movie theaters and restaurants">
  {:.text-center}

  Uses `ListView` to display a list of businesses using
  `ListTile`s. A `Divider` separates the theaters from
  the restaurants.

  使用 `ListView` 的業務列表，它使用了多個 `ListTile`。`Divider` 將餐廳從劇院中分隔開。

  **App source:** [grid_and_list]({{examples}}/layout/grid_and_list)

  **App 原始碼：** [grid_and_list]({{examples}}/layout/grid_and_list)

</div>
<div class="col-lg-6" markdown="1">
  <img src='/assets/images/docs/ui/layout/listview-flutter-gallery.png' class="border mw-100"
      alt="ListView containing shades of blue">
  {:.text-center}

  Uses `ListView` to display the [`Colors`][] from
  the [Material Design palette][]
  for a particular color family.

  使用 `ListView` 展示特定顏色系列 
  [Material Design 調色盤][Material Design palette] 中的 [`Colors`][]。

  **Dart code:** [colors_demo.dart]({{demo}}/reference/colors_demo.dart) from the
  [Flutter Gallery][repo]

  **Dart 程式碼：** [Flutter Gallery][repo] 中的
  [colors_demo.dart]({{demo}}/reference/colors_demo.dart)。
  
</div>
</div>

<?code-excerpt "layout/grid_and_list/lib/main.dart (list)" replace="/\ListView/[!$&!]/g;"?>
```dart
Widget _buildList() {
  return [!ListView!](
    children: [
      _tile('CineArts at the Empire', '85 W Portal Ave', Icons.theaters),
      _tile('The Castro Theater', '429 Castro St', Icons.theaters),
      _tile('Alamo Drafthouse Cinema', '2550 Mission St', Icons.theaters),
      _tile('Roxie Theater', '3117 16th St', Icons.theaters),
      _tile('United Artists Stonestown Twin', '501 Buckingham Way',
          Icons.theaters),
      _tile('AMC Metreon 16', '135 4th St #3000', Icons.theaters),
      const Divider(),
      _tile('K\'s Kitchen', '757 Monterey Blvd', Icons.restaurant),
      _tile('Emmy\'s Restaurant', '1923 Ocean Ave', Icons.restaurant),
      _tile('Chaiya Thai Restaurant', '272 Claremont Blvd', Icons.restaurant),
      _tile('La Ciccia', '291 30th St', Icons.restaurant),
    ],
  );
}

ListTile _tile(String title, String subtitle, IconData icon) {
  return ListTile(
    title: Text(title,
        style: const TextStyle(
          fontWeight: FontWeight.w500,
          fontSize: 20,
        )),
    subtitle: Text(subtitle),
    leading: Icon(
      icon,
      color: Colors.blue[500],
    ),
  );
}
```

<hr>

### Stack

Use [`Stack`][] to arrange widgets on top of a base
widget&mdash;often an image. The widgets can completely
or partially overlap the base widget.

可以使用 [`Stack`][] 在基礎 widget（通常是圖片） 上排列 widget，
widget 可以完全或者部分覆蓋基礎 widget。

#### Summary (Stack)
{:.no_toc}

#### 摘要 (Stack)
{:.no_toc}

* Use for widgets that overlap another widget

  用於覆蓋另一個 widget

* The first widget in the list of children is the base widget;
  subsequent children are overlaid on top of that base widget

  子列表中的第一個 widget 是基礎 widget；後面的子項覆蓋在基礎 widget 的頂部

* A `Stack`'s content can't scroll

  `Stack` 的內容是無法滾動的

* You can choose to clip children that exceed the render box

  你可以剪下掉超出渲染框的子項

#### Examples (Stack)
{:.no_toc}

#### 範例 (Stack)
{:.no_toc}

<div class="row">
<div class="col-lg-7" markdown="1">
  <img src='/assets/images/docs/ui/layout/stack.png' class="mw-100" width="200px" alt="Circular avatar image with a label">
  {:.text-center}

  Uses `Stack` to overlay a `Container`
  (that displays its `Text` on a translucent
  black background) on top of a `CircleAvatar`.
  The `Stack` offsets the text using the `alignment` property and
  `Alignment`s.

  在 `CircleAvatar` 的上面使用 `Stack` 覆蓋 `Container`
  （在透明的黑色背景上展示它的 `Text`）。
  `Stack` 使用 `alignment` 屬性和 `Alignment` 讓文字偏移。

  **App source:** [card_and_stack]({{examples}}/layout/card_and_stack)

  **App 原始碼：** [card_and_stack]({{examples}}/layout/card_and_stack)

</div>
<div class="col-lg-5" markdown="1">
  <img src='/assets/images/docs/ui/layout/stack-flutter-gallery.png' class="mw-100" alt="An image with a icon overlaid on top">
  {:.text-center}

  Uses `Stack` to overlay an icon on top of an image.

  使用 `Stack` 將漸變疊加到圖片的頂部，
  漸變可以將工具欄的圖示和圖片區分開來。

  **Dart code:** [bottom_navigation_demo.dart]({{demo}}/material/bottom_navigation_demo.dart)
  from the [Flutter Gallery][repo]

  **Dart 程式碼：** [Flutter Gallery][repo] 中的
  [bottom_navigation_demo.dart]({{demo}}/material/bottom_navigation_demo.dart)。

</div>
</div>

<?code-excerpt "layout/card_and_stack/lib/main.dart (Stack)" replace="/\bStack/[!$&!]/g;"?>
```dart
Widget _buildStack() {
  return [!Stack!](
    alignment: const Alignment(0.6, 0.6),
    children: [
      const CircleAvatar(
        backgroundImage: AssetImage('images/pic.jpg'),
        radius: 100,
      ),
      Container(
        decoration: const BoxDecoration(
          color: Colors.black45,
        ),
        child: const Text(
          'Mia B',
          style: TextStyle(
            fontSize: 20,
            fontWeight: FontWeight.bold,
            color: Colors.white,
          ),
        ),
      ),
    ],
  );
}
```

<hr>

### Card

A [`Card`][], from the [Material library][],
contains related nuggets of information and can
be composed from almost any widget, but is often used with
[`ListTile`][]. `Card` has a single child,
but its child can be a column, row, list, grid,
or other widget that supports multiple children.
By default, a `Card` shrinks its size to 0 by 0 pixels.
You can use [`SizedBox`][] to constrain the size of a card.

[Material 庫][Material library] 中的 [`Card`][] 包含相關有價值的資訊，
幾乎可以由任何 widget 組成，但是通常和 [`ListTile`][] 一起使用。
`Card` 只有一個子項，
這個子項可以是列、行、列表、網格或者其他支援多個子項的 widget。
預設情況下，`Card` 的大小是 0x0 畫素。
你可以使用 [`SizedBox`][] 控制 card 的大小。

In Flutter, a `Card` features slightly rounded corners
and a drop shadow, giving it a 3D effect.
Changing a `Card`'s `elevation` property allows you to control
the drop shadow effect. Setting the elevation to 24,
for example, visually lifts the `Card` further from the
surface and causes the shadow to become more dispersed.
For a list of supported elevation values, see [Elevation][] in the
[Material guidelines][Material Design].
Specifying an unsupported value disables the drop shadow entirely.

在 Flutter 中，`Card` 有輕微的圓角和陰影來使它具有 3D 效果。
改變 `Card` 的 `elevation` 屬性可以控制陰影效果。
例如，把 elevation 設定為 24，可以從視覺上更多的把 `Card` 抬離表面，
使陰影變得更加分散。關於支援的 elevation 的值的列表，
可以檢視 [Material guidelines][Material Design] 中的 [Elevation][]。
使用不支援的值則會使陰影無效。

#### Summary (Card)
{:.no_toc}

#### 摘要 (Card)
{:.no_toc}

* Implements a [Material card][]

  實現一個 [Material card][]

* Used for presenting related nuggets of information

  用於呈現相關有價值的資訊

* Accepts a single child, but that child can be a `Row`,
  `Column`, or other widget that holds a list of children

  接收單個子項，但是子項可以是 `Row`、`Column` 或者其他
  可以包含列表子項的 widget

* Displayed with rounded corners and a drop shadow

  顯示圓角和陰影

* A `Card`'s content can't scroll

  `Card` 的內容無法滾動

* From the [Material library][]

  來自 [Material 庫][Material library]

#### Examples (Card)
{:.no_toc}

#### 範例 (Card)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  <img src='/assets/images/docs/ui/layout/card.png' class="mw-100" alt="Card containing 3 ListTiles">
  {:.text-center}

  A `Card` containing 3 ListTiles and sized by wrapping
  it with a `SizedBox`. A `Divider` separates the first
  and second `ListTiles`.

  包含 3 個 ListTile 的 `Card`，並且透過被 `SizedBox` 包住來調整大小。
  `Divider` 分隔了第一個和第二個 `ListTiles`。

  **App source:** [card_and_stack]({{examples}}/layout/card_and_stack)

  **App 原始碼：** [card_and_stack]({{examples}}/layout/card_and_stack)

</div>
<div class="col-lg-6" markdown="1">
  <img src='/assets/images/docs/ui/layout/card-flutter-gallery.png' class="mw-100"
      alt="Card containing an image, text and buttons">
  {:.text-center}

  A `Card` containing an image and text.

  包含圖片和文字的 `Card`。

  **Dart code:** [cards_demo.dart]({{demo}}/material/cards_demo.dart)
  from the [Flutter Gallery][repo]

  **Dart 程式碼：** [Flutter Gallery][repo] 中的
  [cards_demo.dart]({{demo}}/material/cards_demo.dart)

</div>
</div>

<?code-excerpt "layout/card_and_stack/lib/main.dart (Card)" replace="/\bCard/[!$&!]/g;"?>
```dart
Widget _buildCard() {
  return SizedBox(
    height: 210,
    child: [!Card!](
      child: Column(
        children: [
          ListTile(
            title: const Text(
              '1625 Main Street',
              style: TextStyle(fontWeight: FontWeight.w500),
            ),
            subtitle: const Text('My City, CA 99984'),
            leading: Icon(
              Icons.restaurant_menu,
              color: Colors.blue[500],
            ),
          ),
          const Divider(),
          ListTile(
            title: const Text(
              '(408) 555-1212',
              style: TextStyle(fontWeight: FontWeight.w500),
            ),
            leading: Icon(
              Icons.contact_phone,
              color: Colors.blue[500],
            ),
          ),
          ListTile(
            title: const Text('costa@example.com'),
            leading: Icon(
              Icons.contact_mail,
              color: Colors.blue[500],
            ),
          ),
        ],
      ),
    ),
  );
}
```
<hr>

### ListTile

Use [`ListTile`][], a specialized row widget from the
[Material library][], for an easy way to create a row
containing up to 3 lines of text and optional leading
and trailing icons. `ListTile` is most commonly used in
[`Card`][] or [`ListView`][], but can be used elsewhere.

[`ListTile`][] 是 [Material 庫][Material library] 中專用的行 widget，
它可以很輕鬆的建立一個包含三行文字以及可選的行前和行尾圖示的行。
`ListTile` 在 [`Card`][] 或者 [`ListView`][] 中最常用，但是也可以在別處使用。

#### Summary (ListTile)
{:.no_toc}

#### 摘要 (ListTile)
{:.no_toc}

* A specialized row that contains up to 3 lines of text and
  optional icons

  一個可以包含最多 3 行文字和可選的圖示的專用的行

* Less configurable than `Row`, but easier to use

  比 `Row` 更少的配置，更容易使用

* From the [Material library][]

  來自 [Material 庫][Material library]

#### Examples (ListTile)
{:.no_toc}

#### 範例 (ListTile)
{:.no_toc}

<div class="row">
<div class="col-lg-6" markdown="1">
  <img src='/assets/images/docs/ui/layout/card.png' class="mw-100" alt="Card containing 3 ListTiles">
  {:.text-center}

  A `Card` containing 3 `ListTile`s.

  包含 3 個 `ListTiles` 的 `Card`。

  **App source:** [card_and_stack]({{examples}}/layout/card_and_stack)

  **App 原始碼：** [card_and_stack]({{examples}}/layout/card_and_stack)

</div>
<div class="col-lg-6" markdown="1">
  <img src='/assets/images/docs/ui/layout/listtile-flutter-gallery.png' class="border mw-100" height="200px"
      alt="4 ListTiles, each containing a leading avatar">
  {:.text-center}

  Uses `ListTile` with leading widgets.<br>

  leading wiget 使用 `ListTile`。

  **Dart code:** [list_demo.dart]({{demo}}/material/list_demo.dart)
  from the [Flutter Gallery][repo]

  **Dart 程式碼：** [Flutter Gallery][repo] 中的
  [list_demo.dart]({{demo}}/material/list_demo.dart)。

</div>
</div>

<hr>

## Constraints

To fully understand Flutter's layout system, you need
to learn how Flutter positions and sizes
the components in a layout. For more information,
see [Understanding constraints][].

## Videos

## 影片

The following videos, part of the
[Flutter in Focus][] series,
explain `Stateless` and `Stateful` widgets.

下面的影片是 [Flutter in Focus][] 系列的一部分，
解釋了 Stateless 和 Stateful 的 widget。

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=55794591&cid=97538062&page=1&autoplay=false" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[Flutter in Focus playlist]({{site.youtube-site}}/watch?v=wgTBLj7rMPM&list=PLjxrf2q8roU2HdJQDjJzOeO6J3FoFLWr2)

---

Each episode of the
[Widget of the Week series]({{site.youtube-site}}/playlist?list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG)
focuses on a widget. Several of them includes layout widgets.

[每週 Widget 系列](https://www.bilibili.com/video/av55795672)
的每一集都會介紹一個 widget。其中也包括一些佈局的 widget。

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=55795672&cid=97539385&page=1&autoplay=false" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[Flutter Widget of the Week playlist]({{site.youtube-site}}/watch?v=yI-8QHpGIP4&index=5&list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG)

## Other resources

## 其他資源

The following resources might help when writing layout code.

當寫佈局程式碼時，下面的資源可能會幫助到你。

* [Layout tutorial][]
<br> Learn how to build a layout.
  
  [Layout 課程][Layout tutorial]
<br> 學習如何建構佈局。

* [Widget catalog][]
<br> Describes many of the widgets available in Flutter.
  
  [核心 Widget 目錄][Widget catalog]<br>
  描述了 Flutter 中很多可用的 widget。

* [HTML/CSS Analogs in Flutter][]
<br> For those familiar with web programming,
  this page maps HTML/CSS functionality to Flutter features.

  [給 Web 開發者的 Flutter 指南][HTML/CSS Analogs in Flutter]
<br> 對那些熟悉 web 開發的人來說，這頁將 HTML/CSS 的功能對映到 Flutter 特性上。

* Flutter Gallery [running app][], [repo][]
<br> Demo app showcasing many Material Design widgets and other
  Flutter features.

  Flutter Gallery [應用][running app]和 [程式碼儲存庫][repo]
<br> Demo app 展示了很多 Material Design widget 和其他的 Flutter 特性。

* [API reference docs][]
<br> Reference documentation for all of the Flutter libraries.
  
  [Flutter API 文件][API reference docs]
<br> 所有 Flutter 庫的參考文件。

* [Adding assets and images][]
<br> Explains how to add images and other assets to your app's package.

  [在 Flutter 中新增資源和圖片][Adding assets and images]
<br> 解釋在你的 app 中如何新增圖片和其他資源。

* [Zero to One with Flutter][]
<br> One person's experience writing his first Flutter app.

  [Flutter 從 0 到 1][Zero to One with Flutter]
<br> 一位開發者第一次寫 Flutter app 的經驗分享文章。


[Adding assets and images]: {{site.url}}/ui/assets-and-images
[API reference docs]: {{api}}
[`build()`]: {{api}}/widgets/StatelessWidget/build.html
[`Card`]: {{api}}/material/Card-class.html
[`Center`]: {{api}}/widgets/Center-class.html
[`Column`]: {{api}}/widgets/Column-class.html
[Common layout widgets]: #common-layout-widgets
[`Colors`]: {{api}}/material/Colors-class.html
[`Container`]: {{api}}/widgets/Container-class.html
[`CrossAxisAlignment`]: {{api}}/rendering/CrossAxisAlignment.html
[`DataTable`]: {{api}}/material/DataTable-class.html
[Elevation]: {{site.material}}/styles/elevation
[`Expanded`]: {{api}}/widgets/Expanded-class.html
[Flutter in Focus]: {{site.youtube-site}}/watch?v=wgTBLj7rMPM&list=PLjxrf2q8roU2HdJQDjJzOeO6J3FoFLWr2
[`GridView`]: {{api}}/widgets/GridView-class.html
[`GridTile`]: {{api}}/material/GridTile-class.html
[HTML/CSS Analogs in Flutter]: {{site.url}}/get-started/flutter-for/web-devs
[`Icon`]: {{api}}/material/Icons-class.html
[`Image`]: {{api}}/widgets/Image-class.html
[Layout tutorial]: {{site.url}}/ui/layout/tutorial
[layout widgets]: {{site.url}}/ui/widgets/layout
[`ListTile`]: {{api}}/material/ListTile-class.html
[`ListView`]: {{api}}/widgets/ListView-class.html
[`MainAxisAlignment`]: {{api}}/rendering/MainAxisAlignment.html
[Material card]: {{site.material}}/components/cards
[Material Design]: {{site.material}}/styles
[Material Design palette]: {{site.material2}}/design/color/the-color-system.html#tools-for-picking-colors
[Material library]: {{api}}/material/material-library.html
[pubspec file]: {{examples}}/layout/pavlova/pubspec.yaml
[`pubspec.yaml` file]: {{examples}}/layout/row_column/pubspec.yaml
[repo]: {{site.repo.gallery}}/tree/main
[`Row`]: {{api}}/widgets/Row-class.html
[running app]: {{site.gallery}}
[`Scaffold`]: {{api}}/material/Scaffold-class.html
[`SizedBox`]: {{api}}/widgets/SizedBox-class.html
[`Stack`]: {{api}}/widgets/Stack-class.html
[`Table`]: {{api}}/widgets/Table-class.html
[`Text`]: {{api}}/widgets/Text-class.html
[tutorial]: {{site.url}}/ui/layout/tutorial
[widgets library]: {{api}}/widgets/widgets-library.html
[Widget catalog]: {{site.url}}/ui/widgets
[Debugging layout issues visually]: {{site.url}}/tools/devtools/inspector#debugging-layout-issues-visually
[Understanding constraints]: {{site.url}}/ui/layout/constraints
[Using the Flutter inspector]: {{site.url}}/tools/devtools/inspector
[Widget of the Week series]: {{site.youtube-site}}/playlist?list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG
[Zero to One with Flutter]: {{site.medium}}/@mravn/zero-to-one-with-flutter-43b13fd7b354
