---
title: Building layouts
title: 佈局建構課程
short-title: Tutorial
short-title: 佈局課程
description: Learn how to build a layout.
description: 學習如何在 Flutter 裡建構佈局。
tags: 使用者介面,Flutter UI,佈局
keywords: 佈局課程,自動換行
diff2html: true
---

{% assign api = '{{site.api}}/flutter' -%}
{% capture examples -%} {{site.repo.this}}/tree/{{site.branch}}/examples {%- endcapture -%}
{% assign rawExFile = 'https://raw.githubusercontent.com/flutter/website/main/examples' -%}
{% capture demo -%} {{site.repo.flutter}}/tree/{{site.branch}}/examples/flutter_gallery/lib/demo {%- endcapture -%}

<style>dl, dd { margin-bottom: 0; }</style>

{{site.alert.secondary}}
  <h4 class="no_toc">What you'll learn</h4>

  <h4 class="no_toc">你將會學習到</h4>

  * How Flutter's layout mechanism works.

    Flutter 的佈局機制是如何工作的。

  * How to lay out widgets vertically and horizontally.

    如何豎直或者水平地對 widgets 進行佈局。

  * How to build a Flutter layout.

    如何建構一個 Flutter 佈局。

{{site.alert.end}}

This is a guide to building layouts in Flutter.
You'll build the layout for the following app:

這是一份如何在 Flutter 中建構佈局的指南。你將為如下 app 建立佈局：

{% include docs/app-figure.md img-class="site-mobile-screenshot border"
    image="ui/layout/lakes.jpg" caption="The finished app" %}

This guide then takes a step back to explain Flutter's
approach to layout, and shows how to place a single widget
on the screen. After a discussion of how to lay widgets
out horizontally and vertically, some of the most common
layout widgets are covered.

這份指南之前溯源一步解釋了 Flutter 中的佈局方式，以及展示瞭如何在螢幕中放置單個 widget。
經過了如何水平以及豎直放置 widgets 的討論之後，一些最常使用的 widgets 都涉及到了。

If you want a "big picture" understanding of the layout mechanism,
start with [Flutter's approach to layout][].

如果你想對佈局機制有個"全域"的理解，可以先從 [Flutter 中的佈局]({{site.url}}/development/ui/layout) 開始.

## Step 0: Create the app base code

## 第一步: 建立 app 基礎程式碼

Make sure to [set up][] your environment,
then do the following:

確保你已經 [安裝和配置][set up] 好了你的環境，
然後做如下步驟：

<?code-excerpt path-base="layout/base"?>

 1. [Create a new Flutter app][new-flutter-app].

    [建立一個新的 Flutter 應用][new-flutter-app]

 2. Replace the contents in `lib/main.dart` with the following code:

    用下面的程式碼來替換你的 `lib/main.dart` 檔案:

    <?code-excerpt "lib/main.dart (all)" title?>
    ```dart
    import 'package:flutter/material.dart';

    void main() => runApp(const MyApp());

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

## Step 1: Diagram the layout

## 第一步: 對佈局進行圖形分解

The first step is to break the layout down to its basic elements:

第一步需要將佈局分解成它的各個基礎元素：

* Identify the rows and columns.

  識別出它的行和列。

* Does the layout include a grid?

  這個佈局是否包含網格佈局？

* Are there overlapping elements?

  是否有重疊的元素？

* Does the UI need tabs?

  介面是否需要選項卡？

* Notice areas that require alignment, padding, or borders.

  留意需要對齊、內間距、或者邊界的區域。

First, identify the larger elements. In this example,
four elements are arranged into a column: an image, two rows, and a block of text.  

首先，識別出稍大的元素。在這個例子中，四個元素排成一列：一個圖像，兩個行區域，和一個文字區域。

{% include docs/app-figure.md img-class="site-mobile-screenshot border"
    image="ui/layout/lakes-column-elts.png" caption="Column elements (circled in red)" %}

Next, diagram each row. The first row, called the Title
section, has 3 children: a column of text, a star icon,
and a number. Its first child, the column, contains 2 lines of text.
That first column takes a lot of space, so it must be wrapped in an
Expanded widget.

接著，對每一行進行圖解。第一行，也就是標題區域，
有三個子元素：一個文字列，一個星形圖示，和一個數字。
它的第一個子元素，文字列，包含兩行文字。
第一列佔據大量空間，因此它應當被封裝在一個 Expanded widget 當中。

{% include docs/app-figure.md image="ui/layout/title-section-parts.png" alt="Title section" %}

The second row, called the Button section, also has
3 children: each child is a column that contains an icon and text.

第二行，也就是按鈕區域，同樣有三個子元素：
每個子元素是一個包含圖示和文字的列。

{% include docs/app-figure.md image="ui/layout/button-section-diagram.png" alt="Button section" %}

Once the layout has been diagrammed, it's easiest to
take a bottom-up approach to implementing it.
To minimize the visual confusion of deeply nested layout code,
place some of the implementation in variables and functions.

一旦圖解好佈局，採取自下而上的方法來實現它就變得尤為輕鬆了。
為了最大程度減少，深層巢狀(Nesting)的佈局程式碼帶來的視覺混亂，
需要用一些變數和函式來替代某些實現。

## Step 2: Implement the title row

## 第二步: 實現標題行

<?code-excerpt path-base="layout/lakes/step2"?>

First, you'll build the left column in the title section.
Add the following code at the top of the `build()`
method of the `MyApp` class:

首先，你可以建構標題部分左側列。新增如下程式碼到 `MyApp` 類別的 `build()` 方法內頂部。

<?code-excerpt "lib/main.dart (titleSection)" title?>
```dart
Widget titleSection = Container(
  padding: const EdgeInsets.all(32),
  child: Row(
    children: [
      Expanded(
        /*1*/
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            /*2*/
            Container(
              padding: const EdgeInsets.only(bottom: 8),
              child: const Text(
                'Oeschinen Lake Campground',
                style: TextStyle(
                  fontWeight: FontWeight.bold,
                ),
              ),
            ),
            Text(
              'Kandersteg, Switzerland',
              style: TextStyle(
                color: Colors.grey[500],
              ),
            ),
          ],
        ),
      ),
      /*3*/
      Icon(
        Icons.star,
        color: Colors.red[500],
      ),
      const Text('41'),
    ],
  ),
);
```

{:.numbered-code-notes}
 1. Putting a `Column` inside an `Expanded` widget stretches
    the column to use all remaining free space in the row.
    Setting the `crossAxisAlignment` property to
    `CrossAxisAlignment.start` positions the column at
    the start of the row.

    將 Column 元素放到 Expanded widget 中可以拉伸該列，以利用該行中所有剩餘的閒置空間。
    設定 `crossAxisAlignment` 屬性值為 `CrossAxisAlignment.start`，
    這會將該列放置在行的起始位置。

 2. Putting the first row of text inside a `Container`
    enables you to add padding. The second child in the
    `Column`, also text, displays as grey.

    將第一行文字放入 Container 容器中使得你可以增加內間距。
    列中的第二個子元素，同樣為文字，顯示為灰色。

 3. The last two items in the title row are a star icon,
    painted red, and the text "41". The entire row is in
    a `Container` and padded along each edge by 32 pixels.
    Add the title section to the app body like this:

    標題行中的最後兩項是一個紅色星形圖示，和文字"41"。
    整行都在一個 Container 容器佈局中，
    而且每條邊都有 32 畫素的內間距。

Add the title section to the app body like this:

如下新增標題部分到 app body 中：

<?code-excerpt path-base="layout/lakes"?>
<?code-excerpt "{../base,step2}/lib/main.dart" from="return MaterialApp"?>
```diff
--- ../base/lib/main.dart
+++ step2/lib/main.dart
@@ -14,11 +48,13 @@
     return MaterialApp(
       title: 'Flutter layout demo',
       home: Scaffold(
         appBar: AppBar(
           title: const Text('Flutter layout demo'),
         ),
-        body: const Center(
-          child: Text('Hello World'),
+        body: Column(
+          children: [
+            titleSection,
+          ],
         ),
       ),
     );
```

{{site.alert.tip}}
  * When pasting code into your app, indentation can
    become skewed. You can fix this in your Flutter editor
    using the [automatic reformatting support][].

    在貼上程式碼到你的 app 中時，行首縮排可能會發生偏移。
    你可以透過使用 [程式碼自動格式化][automatic reformatting support] 來修復這個問題。

  - For a faster development experience,
    try Flutter's [hot reload][] feature.

    為了獲得更便捷的開發體驗，請嘗試 Flutter 的 [熱重載][hot reload] 功能。

  - If you have problems, compare your code to [`lib/main.dart`][].

    如果你有任何問題，可以將你的程式碼與 [`lib/main.dart`][] 比對.

{{site.alert.end}}

## Step 3: Implement the button row

## 第三步: 實現按鈕行

<?code-excerpt path-base="layout/lakes/step3"?>

The button section contains 3 columns that use the same
layout&mdash;an icon over a row of text.
The columns in this row are evenly spaced,
and the text and icons are painted with the primary color.

按鈕區域包含三列使用相同佈局－一行文字上面一個圖示。
此行的各列被等間隙放置，文字和圖示被著以初始色。

Since the code for building each column is almost identical,
create a private helper method named `buildButtonColumn()`,
which takes a color, an `Icon` and `Text`,
and returns a column with its widgets painted in the given color.

由於建構每列的程式碼基本相同，
因此可以建立一個名為
`buildButtonColumn()` 的私有輔助函式，
以顏色、圖示和文字為入參，
返回一個以指定顏色繪製自身
widgets 的一個 column 列物件。

<?code-excerpt "lib/main.dart (_buildButtonColumn)" title?>
```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    // ···
  }

  Column _buildButtonColumn(Color color, IconData icon, String label) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        Icon(icon, color: color),
        Container(
          margin: const EdgeInsets.only(top: 8),
          child: Text(
            label,
            style: TextStyle(
              fontSize: 12,
              fontWeight: FontWeight.w400,
              color: color,
            ),
          ),
        ),
      ],
    );
  }
}
```

The function adds the icon directly to the column.
The text is inside a `Container` with a top-only margin,
separating the text from the icon.

這個函式直接將圖示新增到這列裡。
文字在以一個僅有上間距的 Container 容器中，
使得文字與圖示分隔開。

Build the row containing these columns by calling the
function and passing the color, `Icon`, and text specific
to that column. Align the columns along the main axis
using `MainAxisAlignment.spaceEvenly` to arrange the
free space evenly before, between, and after each column.
Add the following code just below the
`titleSection` declaration inside the `build()` method:

透過呼叫函式並傳遞針對某列的顏色，`Icon` 圖示和文字，
來建構包含這些列的行。
然後在行的主軸方向透過使用 `MainAxisAlignment.spaceEvenly`，
將剩餘的空間均分到每列各自的前後及中間。
只需在 `build()` 方法中的 `titleSection`
宣告下新增如下程式碼：

<?code-excerpt "lib/main.dart (buttonSection)" title?>
```dart
Color color = Theme.of(context).primaryColor;

Widget buttonSection = Row(
  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
  children: [
    _buildButtonColumn(color, Icons.call, 'CALL'),
    _buildButtonColumn(color, Icons.near_me, 'ROUTE'),
    _buildButtonColumn(color, Icons.share, 'SHARE'),
  ],
);
```

Add the button section to the body:

新增按鈕部分到 body 屬性中去：

<?code-excerpt path-base="layout/lakes"?>
<?code-excerpt "{step2,step3}/lib/main.dart" from="return MaterialApp" to="}"?>
```diff
--- step2/lib/main.dart
+++ step3/lib/main.dart
@@ -48,3 +59,3 @@
     return MaterialApp(
       title: 'Flutter layout demo',
       home: Scaffold(
@@ -54,8 +65,9 @@
         body: Column(
           children: [
             titleSection,
+            buttonSection,
           ],
         ),
       ),
     );
   }
```

## Step 4: Implement the text section

## 第四步: 實現文字區域

<?code-excerpt path-base="layout/lakes/step4"?>

Define the text section as a variable. Put the text
in a `Container` and add padding along each edge.
Add the following code just below the `buttonSection`
declaration:

將文字區域定義為一個變數，
將文字放置到一個 Container 容器中，
然後為每條邊新增內邊距。
只需在 `buttonSection` 宣告下新增如下程式碼：

<?code-excerpt "lib/main.dart (textSection)" title?>
```dart
Widget textSection = Container(
  padding: const EdgeInsets.all(32),
  child: const Text(
    'Lake Oeschinen lies at the foot of the Blüemlisalp in the Bernese '
    'Alps. Situated 1,578 meters above sea level, it is one of the '
    'larger Alpine Lakes. A gondola ride from Kandersteg, followed by a '
    'half-hour walk through pastures and pine forest, leads you to the '
    'lake, which warms to 20 degrees Celsius in the summer. Activities '
    'enjoyed here include rowing, and riding the summer toboggan run.',
    softWrap: true,
  ),
);
```

By setting `softwrap` to true, text lines will fill the column width before
wrapping at a word boundary.

透過設定 `softwrap` 為 true，
文字將在填充滿列寬後在單詞邊界處自動換行。

Add the text section to the body:

新增文字部分到 body 屬性：

<?code-excerpt path-base="layout/lakes"?>
<?code-excerpt "{step3,step4}/lib/main.dart" from="return MaterialApp"?>
```diff
--- step3/lib/main.dart
+++ step4/lib/main.dart
@@ -59,3 +72,3 @@
     return MaterialApp(
       title: 'Flutter layout demo',
       home: Scaffold(
@@ -66,6 +79,7 @@
           children: [
             titleSection,
             buttonSection,
+            textSection,
           ],
         ),
       ),
```

## Step 5: Implement the image section

## 第五步: 實現圖片區域

Three of the four column elements are now complete,
leaving only the image. Add the image file to the example:

四個列元素中的三個已經完成了，只剩下圖片部分了。如下新增圖片檔案到範例工程中：

* Create an `images` directory at the top of the project.
* Add [`lake.jpg`][].

  新增 [`lake.jpg`][]

{{site.alert.info}}

  Note that `wget` doesn't work for saving this binary file.
  The original image is [available online][] under a
  Creative Commons license, but it's large and slow to fetch.
  
  注意 `wget` 不能儲存二進位制檔案。
  原始的圖片雖然可以在 Creative Commons 許可下 [線上獲取][available online]，
  但是檔案較大，下載緩慢。
  
{{site.alert.end}}

* Update the `pubspec.yaml` file to include an `assets` tag.
  This makes the image available to your code.

  更新 `pubspec.yaml` 檔案，新增一個 `assets` 標籤。
  這使得在你的程式碼中可以存取到該圖片。

  <?code-excerpt "{step4,step5}/pubspec.yaml"?>
  ```diff
  --- step4/pubspec.yaml
  +++ step5/pubspec.yaml
  @@ -19,3 +19,5 @@

   flutter:
     uses-material-design: true
  +  assets:
  +    - images/lake.jpg
  ```
{{site.alert.tip}}
  * Note that `pubspec.yaml` is case sensitive,
    so write `assets:` and the image URL
    as shown above.

    請注意，`pubspec.yaml` 區分大小寫，
    因此請編寫如上所示的 `assets:` 和圖像 URL。

  * The pubspec file is also sensitive to white
    space, so use proper indentation.

    pubspec 檔案對空格也很敏感，因此請使用適當的縮排。

  * You might need to restart the running program
    (either on the simulator or a connected device) for the
    pubspec changes to take effect.

    你可能需要重新啟動正在執行的程式（在模擬器或連線的裝置上）
    才能使 pubspec 更改生效。

{{site.alert.end}}

Now you can reference the image from your code:

現在你可以在你的程式碼中參考該圖片了：

<?code-excerpt "{step4,step5}/lib/main.dart"?>
```diff
--- step4/lib/main.dart
+++ step5/lib/main.dart
@@ -77,6 +77,12 @@
         ),
         body: Column(
           children: [
+            Image.asset(
+              'images/lake.jpg',
+              width: 600,
+              height: 240,
+              fit: BoxFit.cover,
+            ),
             titleSection,
             buttonSection,
             textSection,
```

`BoxFit.cover` tells the framework that the image should
be as small as possible but cover its entire render box.

`BoxFit.cover` 告訴系統圖片應當儘可能等比縮小到剛好能夠覆蓋住整個渲染 box。

## Step 6: Final touch

## 第六步: 最終的收尾

In this final step, arrange all of the elements in a
`ListView`, rather than a `Column`, because a
`ListView` supports app body scrolling when the app is run
on a small device.

在最後的步驟中，需要在一個 `ListView` 中排列好所有的元素，而不是在一個 `Column` 中，
因為當 app 執行在某個小裝置上時，`ListView` 支援 app body 的滾動。

<?code-excerpt "{step5,step6}/lib/main.dart" diff-u="6" from="return MaterialApp"?>
```diff
--- step5/lib/main.dart
+++ step6/lib/main.dart
@@ -72,13 +77,13 @@
     return MaterialApp(
       title: 'Flutter layout demo',
       home: Scaffold(
         appBar: AppBar(
           title: const Text('Flutter layout demo'),
         ),
-        body: Column(
+        body: ListView(
           children: [
             Image.asset(
               'images/lake.jpg',
               width: 600,
               height: 240,
               fit: BoxFit.cover,
```

**Dart code:** [`main.dart`][]<br>
**Image:** [images][]<br>
**Pubspec:** [`pubspec.yaml`][]<br>

That's it! When you hot reload the app,
you should see the same app layout as
the screenshot at the top of this page.

大功告成！當你熱載入 app 時，你應當可以看到和本頁開頭截圖一樣的 app 佈局了。

You can add interactivity to this layout by following
[Adding Interactivity to Your Flutter App][].

你可以參考文件
[為你的 Flutter 應用加入互動體驗][Adding Interactivity to Your Flutter App] 
來給這個佈局增加互動。

[Adding Interactivity to Your Flutter App]: {{site.url}}/ui/interactivity
[automatic reformatting support]: {{site.url}}/tools/formatting
[available online]: https://images.unsplash.com/photo-1471115853179-bb1d604434e0?dpr=1&amp;auto=format&amp;fit=crop&amp;w=767&amp;h=583&amp;q=80&amp;cs=tinysrgb&amp;crop=
[Flutter's approach to layout]: {{site.url}}/ui/layout
[new-flutter-app]: {{site.url}}/get-started/test-drive
[images]: {{examples}}/layout/lakes/step6/images
[`lake.jpg`]: {{rawExFile}}/layout/lakes/step5/images/lake.jpg
[`lib/main.dart`]: {{examples}}/layout/lakes/step2/lib/main.dart
[hot reload]: {{site.url}}/tools/hot-reload
[`main.dart`]: {{examples}}/layout/lakes/step6/lib/main.dart
[`pubspec.yaml`]: {{examples}}/layout/lakes/step6/pubspec.yaml
[set up]: {{site.url}}/get-started/install
