---
title: Flutter for web developers
title: 給 Web 開發者的 Flutter 指南
description: Learn how to apply Web developer knowledge when building Flutter apps.
description: 學習如何把 Web 的開發經驗應用到 Flutter 應用的開發中。
tags: Flutter課程,Flutter起步,Flutter入門
keywords: Flutter Web,iOS,用Flutter開發iOS,Flutter網頁版
css-old: [two_column.css]
---

<?code-excerpt path-base="get-started/flutter-for/web_devs"?>

This page is for users who are familiar with the HTML
and CSS syntax for arranging components of an application's UI.
It maps HTML/CSS code snippets to their Flutter/Dart code equivalents.

本文是為那些熟悉用 HTML 與 CSS 語法來管理應用頁面中元素的開發者準備的。
本文會將 HTML/CSS 程式碼片段替換為等價的 Flutter/Dart 程式碼。

One of the fundamental differences between
designing a web layout and a Flutter layout,
is learning how constraints work,
and how widgets are sized and positioned.
To learn more, see [Understanding constraints][].

在 Web 和 Flutter 的佈局基礎條件中，
**佈局限制、widget 的大小確定和定位** 是重要的區別之一。
想要了解更多，你可以閱讀
[深入理解 Flutter 佈局約束][Understanding constraints]。

The examples assume:

以下的範例基於如下假設：

* The HTML document starts with `<!DOCTYPE html>`, and the CSS box model
  for all HTML elements is set to [`border-box`][],
  for consistency with the Flutter model.

  HTML 檔案以 `<!DOCTYPE html>` 開頭，且為了與 Flutter 模型保持一致，
  所有 HTML 元素的 CSS 盒模型被設定為
  [`border-box`](https://css-tricks.com/box-sizing/)。

  ```css
  {
      box-sizing: border-box;
  }
  ```
* In Flutter, the default styling of the 'Lorem ipsum' text
  is defined by the `bold24Roboto` variable as follows,
  to keep the syntax simple:

  在 Flutter 中，為了保持語法簡潔，
  "Lorem ipsum" 文字的預設樣式由如下 `bold24Roboto` 變數定義：

  <?code-excerpt "lib/main.dart (TextStyle)"?>
  ```dart
  TextStyle bold24Roboto = const TextStyle(
    color: Colors.white,
    fontSize: 24,
    fontWeight: FontWeight.bold,
  );
  ```

{{site.alert.secondary}}

  How is react-style, or _declarative_, programming different from the
  traditional imperative style?
  For a comparison, see [Introduction to declarative
  UI][Introduction to declarative UI].

  React-style 或 **宣告式** 程式設計與傳統的命令式風格有何不同？
  為了對比，請查閱 [宣告式 UI 介紹][Introduction to declarative UI]。

{{site.alert.end}}

## Performing basic layout operations

## 執行基礎佈局操作

The following examples show how to perform the most common UI layout tasks.

以下範例將向你展示如何執行最常見的 UI 佈局操作。

### Styling and aligning text

### 文字樣式與對齊

Font style, size, and other text attributes that CSS handles with the font and
color properties are individual properties of a
[`TextStyle`][]
child of a
[`Text`][] widget.

CSS 所處理的字型樣式、大小以及其他文字屬性，
都是一個 [`Text`][] widget 子元素 [`TextStyle`][] 中單獨的屬性。

For text-align property in CSS that is used for aligning text,
there is a textAlign property of a [`Text`][] widget.

[`Text`][] widget 中的 textAlign 屬性與 CSS 中的
text-align 屬性作用相同，用來控制文字的對齊方向。

In both HTML and Flutter, child elements or widgets
are anchored at the top left, by default.

在 HTML 和 Flutter 中，子元素或者 widget 的位置都預設在左上方。

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  Lorem ipsum
</div>

.grey-box {
    background-color: #e0e0e0; /* grey 300 */
    width: 320px;
    height: 240px;
    [[highlight]]font: 900 24px Georgia;[[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
<?code-excerpt "lib/main.dart (Container)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  color: Colors.grey[300],
  child: const Text(
    'Lorem ipsum',
    style: [[highlight]]TextStyle(
      fontFamily: 'Georgia',
      fontSize: 24,
      fontWeight: FontWeight.bold,
    ),
    [[/highlight]]
    [[highlight]]textAlign: TextAlign.center, [[/highlight]]
  ),
);
{% endprettify %}
</div>

### Setting background color

### 設定背景顏色

In Flutter, you set the background color using the `color` property
or the `decoration` property of a [`Container`][].
However, you cannot supply both, since it would potentially
result in the decoration drawing over the background color.
The `color` property should be preferred
when the background is a simple color.
For other cases, such as gradients or images,
use the `decoration` property.

在 Flutter 中，你可以透過 [`Container`][] 的
`decoration` 或者 `color` 屬性來設定背景顏色。
但是，你不能同時設定這兩個屬性，這有可能導致 `decoration` 覆蓋掉 `color`。
當背景是簡單的顏色時，應首選 `color` 屬性，
對於其他情況漸變或影象等情況，推薦使用 `decoration` 屬性。

The CSS examples use the hex color equivalents to the Material color palette.

CSS 範例使用十六進位制顏色，
這等價於 Material 的調色盤。

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  Lorem ipsum
</div>

.grey-box {
    [[highlight]]background-color: #e0e0e0;[[/highlight]] /* grey 300 */
    width: 320px;
    height: 240px;
    font: 900 24px Roboto;
}
{% endprettify %}
</div>

<div class="righthighlight">
<?code-excerpt "lib/main.dart (Container2)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  [[highlight]]color: Colors.grey[300],
  [[/highlight]]
  child: Text(
    'Lorem ipsum',
    style: bold24Roboto,
  ),
);
{% endprettify %}
</div>

<div class="righthighlight">
<?code-excerpt "lib/main.dart (Container3)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  [[highlight]]decoration: BoxDecoration(
    color: Colors.grey[300],
  ),
  [[/highlight]]
  child: Text(
    'Lorem ipsum',
    style: bold24Roboto,
  ),
);
{% endprettify %}
</div>

### Centering components

### 居中元素

A [`Center`][] widget
centers its child both horizontally and vertically.

一個 [`Center`][] widget 可以將它的子 widget 同時以水平和垂直方向居中。

To accomplish a similar effect in CSS, the parent element uses either a flex
or table-cell display behavior. The examples on this page show the flex
behavior.

要用 CSS 實現相似的效果，父元素需要使用一個 flex
或者 table-cell 顯示佈局。本節範例使用的是 flex 佈局。

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  Lorem ipsum
</div>

.grey-box {
    background-color: #e0e0e0; /* grey 300 */
    width: 320px;
    height: 240px;
    font: 900 24px Roboto;
    [[highlight]]display: flex;
    align-items: center;
    justify-content: center;[[/highlight]]
}
{% endprettify %}
</div>

<div class="righthighlight">
<?code-excerpt "lib/main.dart (Center)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  color: Colors.grey[300],
  child: [[highlight]]Center(
    child: [[/highlight]]Text(
      'Lorem ipsum',
      style: bold24Roboto,
    ),
  ),
);
{% endprettify %}
</div>

### Setting container width

### 設定容器寬度

To specify the width of a [`Container`][]
widget, use its `width` property.
This is a fixed width, unlike the CSS max-width property
that adjusts the container width up to a maximum value.
To mimic that effect in Flutter,
use the `constraints` property of the Container.
Create a new [`BoxConstraints`][] widget with a `minWidth` or `maxWidth`.

要指定一個 [`Container`][] widget 的寬度，請使用它的 `width` 屬性。
與 CSS 中的 max-width 屬性用於指定容器可調整的寬度最大值不同的是，
這裡指定的是一個固定寬度。
要在 Flutter 中模擬該效果，可以使用 Container 的 `constraints` 屬性。
新建一個帶有 `minWidth` 和 `maxWidth` 屬性的 [`BoxConstraints`][] widget。

For nested Containers, if the parent’s width is less than the child’s width,
the child Container sizes itself to match the parent.

對巢狀(Nesting)的 Container 來說，如果其父元素寬度小於子元素寬度，
則子元素會調整尺寸以匹配父元素大小。

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  <div class="red-box">
    Lorem ipsum
  </div>
</div>

.grey-box {
    background-color: #e0e0e0; /* grey 300 */
    [[highlight]]width: 320px;[[/highlight]]
    height: 240px;
    font: 900 24px Roboto;
    display: flex;
    align-items: center;
    justify-content: center;
}
.red-box {
    background-color: #ef5350; /* red 400 */
    padding: 16px;
    color: #ffffff;
    [[highlight]]width: 100%;
    max-width: 240px;[[/highlight]]
}
{% endprettify %}
</div>

<div class="righthighlight">
<?code-excerpt "lib/main.dart (Nested)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  [[highlight]]width: 320,
  [[/highlight]]
  height: 240,
  color: Colors.grey[300],
  child: Center(
    child: Container(
      // red box
      [[highlight]]width: 240,
      [[/highlight]]// max-width is 240
      padding: const EdgeInsets.all(16),
      decoration: BoxDecoration(
        color: Colors.red[400],
      ),
      child: Text(
        'Lorem ipsum',
        style: bold24Roboto,
      ),
    ),
  ),
);
{% endprettify %}
</div>

## Manipulating position and size

## 操控位置與大小

The following examples show how to perform more complex operations
on widget position, size, and background.

以下範例將展示如何對 widget 的位置、大小以及背景進行更復雜的操作。

### Setting absolute position

### 設定絕對位置

By default, widgets are positioned relative to their parent.

預設情況下，widget 相對於其父元素定位。

To specify an absolute position for a widget as x-y coordinates,
nest it in a [`Positioned`][] widget that is,
in turn, nested in a [`Stack`][] widget.

想要透過 x-y 座標指定一個 widget 的絕對位置，
可以把它放在一個 [`Positioned`][] widget 中，
而 `Positioned` 則需被放在一個 [`Stack`][] widget 中。

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  <div class="red-box">
    Lorem ipsum
  </div>
</div>

.grey-box {
    [[highlight]]position: relative;[[/highlight]]
    background-color: #e0e0e0; /* grey 300 */
    width: 320px;
    height: 240px;
    font: 900 24px Roboto;
}
.red-box {
    background-color: #ef5350; /* red 400 */
    padding: 16px;
    color: #ffffff;
    [[highlight]]position: absolute;
    top: 24px;
    left: 24px;[[/highlight]]
}
{% endprettify %}
</div>

<div class="righthighlight">
<?code-excerpt "lib/main.dart (Absolute)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  color: Colors.grey[300],
  [[highlight]]child: Stack(
    children: [[/highlight]][
      Positioned(
        // red box
        [[highlight]]left: 24,
        top: 24,
        [[/highlight]]
        child: Container(
          padding: const EdgeInsets.all(16),
          decoration: BoxDecoration(
            color: Colors.red[400],
          ),
          child: Text(
            'Lorem ipsum',
            style: bold24Roboto,
          ),
        ),
      ),
    ],
  ),
);
{% endprettify %}
</div>

### Rotating components

### 旋轉元素

To rotate a widget, nest it in a [`Transform`][] widget.
Use the `Transform` widget’s `alignment` and `origin` properties
to specify the transform origin (fulcrum) in relative and absolute terms,
respectively.

想要旋轉一個 widget，請將它放在 [`Transform`][] widget 中。
使用 `Transform` widget 的 `alignment` 和 `origin`
屬性分別來指定轉換原點（支點）的相對和絕對位置資訊。

For a simple 2D rotation, in which the widget is rotated on the Z axis,
create a new [`Matrix4`][] identity object
and use its `rotateZ()` method to specify the rotation factor
using radians (degrees × π / 180).

對於簡單的 2D 旋轉，例如在 Z 軸上旋轉的，
建立一個新的 [`Matrix4`][] 物件，
並使用它的 `rotateZ()` 方法以弧度單位
（角度 × π / 180）指定旋轉系數。

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  <div class="red-box">
    Lorem ipsum
  </div>
</div>

.grey-box {
    background-color: #e0e0e0; /* grey 300 */
    width: 320px;
    height: 240px;
    font: 900 24px Roboto;
    display: flex;
    align-items: center;
    justify-content: center;
}
.red-box {
    background-color: #ef5350; /* red 400 */
    padding: 16px;
    color: #ffffff;
    [[highlight]]transform: rotate(15deg);[[/highlight]]
}
{% endprettify %}
</div>

<div class="righthighlight">
<?code-excerpt "lib/main.dart (Rotating)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  color: Colors.grey[300],
  child: Center(
    child: [[highlight]]Transform(
      alignment: Alignment.center,
      transform: Matrix4.identity()..rotateZ(15 * 3.1415927 / 180),
      child: [[/highlight]]Container(
        // red box
        padding: const EdgeInsets.all(16),
        decoration: BoxDecoration(
          color: Colors.red[400],
        ),
        child: Text(
          'Lorem ipsum',
          style: bold24Roboto,
          textAlign: TextAlign.center,
        ),
      ),
    ),
  ),
);
{% endprettify %}
</div>

### Scaling components

### 縮放元素

To scale a widget up or down, nest it in a [`Transform`][] widget.
Use the Transform widget’s `alignment` and `origin` properties
to specify the transform origin (fulcrum) in relative or absolute terms,
respectively.

想要縮放一個 widget，請同樣將它放在一個 [`Transform`][] widget 中。
使用 `Transform` widget 的 `alignment` 和 `origin`
屬性分別來指定縮放原點（支點）的相對和絕對資訊。

For a simple scaling operation along the x-axis,
create a new [`Matrix4`][] identity object
and use its `scale()` method to specify the scaling factor.

對於沿 x 軸的簡單縮放操作，新建一個 [`Matrix4`][] 物件
並用它的 `scale()` 方法來指定縮放因係數。

When you scale a parent widget, its child widgets are scaled accordingly.

當你縮放一個父 widget 時，它的子 widget 也會相應被縮放。
<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  <div class="red-box">
    Lorem ipsum
  </div>
</div>

.grey-box {
    background-color: #e0e0e0; /* grey 300 */
    width: 320px;
    height: 240px;
    font: 900 24px Roboto;
    display: flex;
    align-items: center;
    justify-content: center;
}
.red-box {
    background-color: #ef5350; /* red 400 */
    padding: 16px;
    color: #ffffff;
    [[highlight]]transform: scale(1.5);[[/highlight]]
}
{% endprettify %}
</div>

<div class="righthighlight">
<?code-excerpt "lib/main.dart (Scaling)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  color: Colors.grey[300],
  child: Center(
    child: [[highlight]]Transform(
      alignment: Alignment.center,
      transform: Matrix4.identity()..scale(1.5),
      child: [[/highlight]]Container(
        // red box
        padding: const EdgeInsets.all(16),
        decoration: BoxDecoration(
          color: Colors.red[400],
        ),
        child: Text(
          'Lorem ipsum',
          style: bold24Roboto,
          textAlign: TextAlign.center,
        ),
      ),
    ),
  ),
);
{% endprettify %}
</div>

### Applying a linear gradient

### 應用線性變換

To apply a linear gradient to a widget's background, nest it in a
[`Container`][]
widget.  Then use the Container widget’s `decoration` property to create a
[`BoxDecoration`][]
object, and use BoxDecoration's `gradient` property to transform the
background
fill.

想要將線性顏色漸變在 widget 的背景上應用，請將它巢狀(Nesting)在一個 [`Container`][] widget 中。
接著將一個 [`BoxDecoration`][] 物件傳遞至 `Container` 的 `decoration`，
然後使用 `BoxDecoration` 的 `gradient` 屬性來變換背景填充內容。

The gradient “angle” is based on the Alignment (x, y) values:

變換「角度」基於 `Alignment (x, y)` 取值來定：

* If the beginning and ending x values are equal, the gradient is vertical
(0° | 180°).

  如果開始和結束的 x 值相同，變換將是垂直的 (0° | 180°)。

* If the beginning and ending y values are equal, the gradient is horizontal
(90° | 270°).

  如果開始和結束的 y 值相同，變換將是水平的 (90° | 270°)。

#### Vertical gradient

#### 垂直變換

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  <div class="red-box">
    Lorem ipsum
  </div>
</div>

.grey-box {
    background-color: #e0e0e0; /* grey 300 */
    width: 320px;
    height: 240px;
    font: 900 24px Roboto;
    display: flex;
    align-items: center;
    justify-content: center;
}
.red-box {
    padding: 16px;
    color: #ffffff;
    [[highlight]]background: linear-gradient(180deg, #ef5350, rgba(0, 0, 0, 0) 80%);[[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
<?code-excerpt "lib/main.dart (Gradient)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  color: Colors.grey[300],
  child: Center(
    child: Container(
      // red box
      [[highlight]]decoration: const BoxDecoration(
        gradient: LinearGradient(
          begin: Alignment.topCenter,
          end: Alignment(0.0, 0.6),
          colors: <Color>[
            Color(0xffef5350),
            Color(0x00ef5350),
          ],
        ),
      ),
      [[/highlight]]
      padding: const EdgeInsets.all(16),
      child: Text(
        'Lorem ipsum',
        style: bold24Roboto,
      ),
    ),
  ),
);
{% endprettify %}
</div>

#### Horizontal gradient

#### 水平變換

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  <div class="red-box">
    Lorem ipsum
  </div>
</div>

.grey-box {
    background-color: #e0e0e0; /* grey 300 */
    width: 320px;
    height: 240px;
    font: 900 24px Roboto;
    display: flex;
    align-items: center;
    justify-content: center;
}
.red-box {
    padding: 16px;
    color: #ffffff;
    [[highlight]]background: linear-gradient(90deg, #ef5350, rgba(0, 0, 0, 0) 80%);[[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
<?code-excerpt "lib/main.dart (HorizontalGradient)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  color: Colors.grey[300],
  child: Center(
    child: Container(
      // red box
      padding: const EdgeInsets.all(16),
      [[highlight]]decoration: const BoxDecoration(
        gradient: LinearGradient(
          begin: Alignment(-1.0, 0.0),
          end: Alignment(0.6, 0.0),
          colors: <Color>[
            Color(0xffef5350),
            Color(0x00ef5350),
          ],
        ),
      ),
      [[/highlight]]
      child: Text(
        'Lorem ipsum',
        style: bold24Roboto,
      ),
    ),
  ),
);
{% endprettify %}
</div>

## Manipulating shapes

## 操控圖形

The following examples show how to make and customize shapes.

以下範例將展示如何新建和自訂圖形。

### Rounding corners

### 圓角

To round the corners of a rectangular shape,
use the `borderRadius` property of a [`BoxDecoration`][] object.
Create a new [`BorderRadius`][]
object that specifies the radius for rounding each corner.

想要在矩形上實現圓角，請用 [`BoxDecoration`][] 物件的 `borderRadius` 屬性。
新建一個 [`BorderRadius`][] 物件來指定各個圓角的半徑大小。

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  <div class="red-box">
    Lorem ipsum
  </div>
</div>

.grey-box {
    background-color: #e0e0e0; /* grey 300 */
    width: 320px;
    height: 240px;
    font: 900 24px Roboto;
    display: flex;
    align-items: center;
    justify-content: center;
}
.red-box {
    background-color: #ef5350; /* red 400 */
    padding: 16px;
    color: #ffffff;
    [[highlight]]border-radius: 8px;[[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
<?code-excerpt "lib/main.dart (RoundCorners)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  color: Colors.grey[300],
  child: Center(
    child: Container(
      // red circle
      padding: const EdgeInsets.all(16),
      decoration: BoxDecoration(
        color: Colors.red[400],
        [[highlight]]borderRadius: const BorderRadius.all(
          Radius.circular(8),
        ), [[/highlight]]
      ),
      child: Text(
        'Lorem ipsum',
        style: bold24Roboto,
      ),
    ),
  ),
);
{% endprettify %}
</div>

### Adding box shadows

### 為盒子新增陰影 (box shadows)

In CSS you can specify shadow offset and blur in shorthand,
using the box-shadow property. This example shows two box shadows,
with properties:

在 CSS 中你可以透過 box-shadow 屬性快速指定陰影偏移與模糊範圍。
本例展示了兩個盒陰影的屬性設定：

* `xOffset: 0px, yOffset: 2px, blur: 4px, color: black @80% alpha`
* `xOffset: 0px, yOffset: 06x, blur: 20px, color: black @50% alpha`

In Flutter, each property and value is specified separately.
Use the `boxShadow` property of `BoxDecoration` to create a list of
[`BoxShadow`][] widgets. You can define one or multiple
`BoxShadow` widgets, which can be stacked
to customize the shadow depth, color, and so on.

在 Flutter 中，每個屬性與其取值都是單獨指定的。
請使用 BoxDecoration 的 `boxShadow` 
屬性來產生一系列 [`BoxShadow`][] widget。
你可以定義一個或多個 `BoxShadow` widget，
這些 widget 共同用於設定陰影深度、顏色等等。

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  <div class="red-box">
    Lorem ipsum
  </div>
</div>

.grey-box {
    background-color: #e0e0e0; /* grey 300 */
    width: 320px;
    height: 240px;
    font: 900 24px Roboto;
    display: flex;
    align-items: center;
    justify-content: center;
}
.red-box {
    background-color: #ef5350; /* red 400 */
    padding: 16px;
    color: #ffffff;
    [[highlight]]box-shadow: 0 2px 4px rgba(0, 0, 0, 0.8),
              0 6px 20px rgba(0, 0, 0, 0.5);[[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
<?code-excerpt "lib/main.dart (BoxShadow)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  margin: const EdgeInsets.only(bottom: 16),
  decoration: BoxDecoration(
    color: Colors.grey[300],
  ),
  child: Center(
    child: Container(
      // red box
      padding: const EdgeInsets.all(16),
      decoration: BoxDecoration(
        color: Colors.red[400],
        [[highlight]]boxShadow: const <BoxShadow>[
          BoxShadow(
            color: Color(0xcc000000),
            offset: Offset(0, 2),
            blurRadius: 4,
          ),
          BoxShadow(
            color: Color(0x80000000),
            offset: Offset(0, 6),
            blurRadius: 20,
          ),
        ], [[/highlight]]
      ),
      child: Text(
        'Lorem ipsum',
        style: bold24Roboto,
      ),
    ),
  ),
);
{% endprettify %}
</div>

### Making circles and ellipses

### 產生圓與橢圓

Making a circle in CSS requires a workaround of applying a border-radius of
50% to all four sides of a rectangle, though there are
[basic shapes][].

儘管 CSS 中有 [基礎圖形][basic shapes]，
用 CSS 產生圓可以用一個變通方案，
即將矩形的四邊 border-radius 均設成 50%。

While this approach is supported
with the `borderRadius` property of [`BoxDecoration`][],
Flutter provides a `shape` property
with [`BoxShape` enum][] for this purpose.

雖然 [`BoxDecoration`][] 的 `borderRadius` 屬性支援這樣設定，
Flutter 提供了一個 `shape` 屬性用於實現同樣的目的，
它的型別是 [`BoxShape` 列舉][`BoxShape` enum]。

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  <div class="red-circle">
    Lorem ipsum
  </div>
</div>

.grey-box {
    background-color: #e0e0e0; /* grey 300 */
    width: 320px;
    height: 240px;
    font: 900 24px Roboto;
    display: flex;
    align-items: center;
    justify-content: center;
}
.red-circle {
    background-color: #ef5350; /* red 400 */
    padding: 16px;
    color: #ffffff;
    [[highlight]]text-align: center;
    width: 160px;
    height: 160px;
    border-radius: 50%;[[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
<?code-excerpt "lib/main.dart (Circle)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  color: Colors.grey[300],
  child: Center(
    child: Container(
      // red circle
      decoration: BoxDecoration(
        color: Colors.red[400],
        [[highlight]]shape: BoxShape.circle, [[/highlight]]
      ),
      padding: const EdgeInsets.all(16),
      [[highlight]]width: 160,
      height: 160,
      [[/highlight]]
      child: Text(
        'Lorem ipsum',
        style: bold24Roboto,
        [[highlight]]textAlign: TextAlign.center, [[/highlight]]
      ),
    ),
  ),
);
{% endprettify %}
</div>

## Manipulating text

## 操控文字

The following examples show how to specify fonts and other
text attributes. They also show how to transform text strings,
customize spacing, and create excerpts.

以下範例展示瞭如何設定字型和其他文字屬性。
它們同時還展示瞭如何變換文字字元、自訂間距以及產生摘要。

### Adjusting text spacing

### 文字間距調整

In CSS, you specify the amount of white space
between each letter or word by giving a length value
for the letter-spacing and word-spacing properties, respectively.
The amount of space can be in px, pt, cm, em, etc.

在 CSS 中你可以透過分別設定 letter-spacing 和 word-spacing 屬性，
來指定每個字母以及每個單詞間的空白距離。
距離的單位可以是 px、pt、cm、em 等。

In Flutter, you specify white space as logical pixels
(negative values are allowed)
for the `letterSpacing` and `wordSpacing` properties
of a [`TextStyle`][] child of a `Text` widget.

在 Flutter 中，你可以將 `Text` widget 的 [`TextStyle`][] 屬性中
`letterSpacing` 與 `wordSpacing` 設定為邏輯畫素（允許負值）。

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  <div class="red-box">
    Lorem ipsum
  </div>
</div>

.grey-box {
    background-color: #e0e0e0; /* grey 300 */
    width: 320px;
    height: 240px;
    font: 900 24px Roboto;
    display: flex;
    align-items: center;
    justify-content: center;
}
.red-box {
    background-color: #ef5350; /* red 400 */
    padding: 16px;
    color: #ffffff;
    [[highlight]]letter-spacing: 4px;[[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
<?code-excerpt "lib/main.dart (TextSpacing)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  color: Colors.grey[300],
  child: Center(
    child: Container(
      // red box
      padding: const EdgeInsets.all(16),
      decoration: BoxDecoration(
        color: Colors.red[400],
      ),
      child: const Text(
        'Lorem ipsum',
        style: TextStyle(
          color: Colors.white,
          fontSize: 24,
          fontWeight: FontWeight.w900,
          [[highlight]]letterSpacing: 4, [[/highlight]]
        ),
      ),
    ),
  ),
);
{% endprettify %}
</div>

### Making inline formatting changes

### 內聯樣式更改

A [`Text`][] widget lets you display text
with some formatting characteristics.
To display text that uses multiple styles
(in this example, a single word with emphasis),
use a [`RichText`][] widget instead.
Its `text` property can specify one or more
[`TextSpan`][] objects that can be individually styled.

一個 [`Text`][] widget 允許你展示同一類樣式的文字。
為了展現具有多種樣式（本例中，是一個帶重音的單詞）的文字，
你需要改用 [`RichText`][] widget。
它的 `text` 屬性可以設定一個或多個可單獨設定樣式的 [`TextSpan`][]。

In the following example, "Lorem" is in a `TextSpan`
with the default (inherited) text styling,
and "ipsum" is in a separate `TextSpan` with custom styling.

在接下來的範例中，「Lorem」位於 `TextSpan` 中，具有預設（繼承）文字樣式，
「ipsum」位於具有自訂樣式、單獨的一個 `TextSpan` 中。

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  <div class="red-box">
    [[highlight]]Lorem <em>ipsum</em>[[/highlight]]
  </div>
</div>

.grey-box {
    background-color: #e0e0e0; /* grey 300 */
    width: 320px;
    height: 240px;
    [[highlight]]font: 900 24px Roboto;[[/highlight]]
    display: flex;
    align-items: center;
    justify-content: center;
}
.red-box {
    background-color: #ef5350; /* red 400 */
    padding: 16px;
    color: #ffffff;
}
[[highlight]].red-box em {
    font: 300 48px Roboto;
    font-style: italic;
}[[/highlight]]
{% endprettify %}
</div>
<div class="righthighlight">
<?code-excerpt "lib/main.dart (InlineFormatting)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  color: Colors.grey[300],
  child: Center(
    child: Container(
      // red box
      decoration: BoxDecoration(
        color: Colors.red[400],
      ),
      padding: const EdgeInsets.all(16),
      child: [[highlight]]RichText(
        text: TextSpan(
          style: bold24Roboto,
          children: const <TextSpan>[
            TextSpan(text: 'Lorem '),
            TextSpan(
              text: 'ipsum',
              style: TextStyle(
                fontWeight: FontWeight.w300,
                fontStyle: FontStyle.italic,
                fontSize: 48,
              ),
            ),
          ],
        ),
      ), [[/highlight]]
    ),
  ),
);
{% endprettify %}
</div>

### Creating text excerpts

### 產生文字摘要

An excerpt displays the initial line(s) of text in a paragraph,
and handles the overflow text, often using an ellipsis.

一個摘要會展示一個段落中文字的初始行內容，
並常用省略號處理溢位的文字內容。

In Flutter, use the `maxLines` property of a [`Text`][] widget
to specify the number of lines to include in the excerpt,
and the `overflow` property for handling overflow text.

在 Flutter 中，你可以使用 [`Text`][] widget 的
`maxLines` 屬性來指定包含在摘要中的行數，
以及 `overflow` 屬性來處理溢位文字。

<div class="lefthighlight">
{% prettify css %}
<div class="grey-box">
  <div class="red-box">
    Lorem ipsum dolor sit amet, consec etur
  </div>
</div>

.grey-box {
    background-color: #e0e0e0; /* grey 300 */
    width: 320px;
    height: 240px;
    font: 900 24px Roboto;
    display: flex;
    align-items: center;
    justify-content: center;
}
.red-box {
    background-color: #ef5350; /* red 400 */
    padding: 16px;
    color: #ffffff;
    [[highlight]]overflow: hidden;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;[[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
<?code-excerpt "lib/main.dart (TextExcerpt)" replace="/\/\*//g;/\*\/ *//g"?>
{% prettify dart %}
final container = Container(
  // grey box
  width: 320,
  height: 240,
  color: Colors.grey[300],
  child: Center(
    child: Container(
      // red box
      decoration: BoxDecoration(
        color: Colors.red[400],
      ),
      padding: const EdgeInsets.all(16),
      child: Text(
        'Lorem ipsum dolor sit amet, consec etur',
        style: bold24Roboto,
        [[highlight]]overflow: TextOverflow.ellipsis,
        maxLines: 1, [[/highlight]]
      ),
    ),
  ),
);
{% endprettify %}
</div>
<div class="end-examples"></div>


[basic shapes]: https://developer.mozilla.org/en-US/docs/Web/CSS/basic-shape
[`border-box`]: https://css-tricks.com/box-sizing/
[`BorderRadius`]: {{site.api}}/flutter/painting/BorderRadius-class.html
[`BoxDecoration`]: {{site.api}}/flutter/painting/BoxDecoration-class.html
[`BoxConstraints`]: {{site.api}}/flutter/rendering/BoxConstraints-class.html
[`BoxShape` enum]: {{site.api}}/flutter/painting/BoxShape.html
[`BoxShadow`]: {{site.api}}/flutter/painting/BoxShadow-class.html
[`Center`]: {{site.api}}/flutter/widgets/Center-class.html
[`Container`]: {{site.api}}/flutter/widgets/Container-class.html
[Introduction to declarative UI]: {{site.url}}/get-started/flutter-for/declarative
[`Matrix4`]: {{site.api}}/flutter/vector_math_64/Matrix4-class.html
[`Positioned`]: {{site.api}}/flutter/widgets/Positioned-class.html
[`RichText`]: {{site.api}}/flutter/widgets/RichText-class.html
[`Stack`]: {{site.api}}/flutter/widgets/Stack-class.html
[`Text`]: {{site.api}}/flutter/widgets/Text-class.html
[`TextSpan`]: {{site.api}}/flutter/painting/TextSpan-class.html
[`TextStyle`]: {{site.api}}/flutter/painting/TextStyle-class.html
[`Transform`]: {{site.api}}/flutter/widgets/Transform-class.html
[Understanding constraints]: {{site.url}}/development/ui/layout/constraints

