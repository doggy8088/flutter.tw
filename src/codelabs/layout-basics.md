---
title: "Basic Flutter layout concepts"
title: "Flutter 佈局基礎課程"
description: "A codelab that teaches basic Flutter layout concepts through DartPad examples and exercises."
description: "使用 DartPad2 工具教你如何建構 Flutter 佈局"
tags: 課程, 程式碼實驗室
keywords: Flutter佈局課程
toc: true
js:
- defer: true
  url: https://dartpad.cn/inject_embed.dart.js
---

Welcome to the Flutter layout codelab,
where you learn how to build a Flutter UI without
downloading and installing Flutter or Dart!

歡迎來到 Flutter 佈局 codelab！
你將在這裡學到如何建構 Flutter UI，
更棒的是這一切都不需要安裝 Flutter 或者 Dart！

{% include docs/dartpad-troubleshooting.md %}

Flutter is different from other frameworks because its UI
is built in code, not (for example) in an XML file or similar.
Widgets are the basic building blocks of a Flutter UI.
As you progress through this codelab,
you'll learn that almost everything in Flutter is a widget.
A widget is an immutable object that describes a specific part of a UI.
You'll also learn that Flutter widgets are composable, meaning
that you can combine existing widgets to make more sophisticated widgets.
At the end of this codelab,
you'll get to apply what you've learned
into building a Flutter UI that displays a business card.

Flutter 與其他框架有著明顯的差異，
原因在於它使用程式碼來建構 UI，而不是 XML 或其他東西。
其中，widget 是建構 Flutter UI 的基本單元。
當你逐漸深入這個 codelab，你將會發現在 Flutter 中幾乎所有的東西都是 widget。
widget 是一個不會改變的物件，它是 UI 中一個特定部分的描述。
你還會學到 Flutter 的 widget 非常容易組合，
這意味著你能夠透過組合已有的 widgets 來創造更多複雜的 widgets。
到這篇文章的最後，你會運用這裡所學的知識建構一個顯示名片的 Flutter UI。

**Estimated time to complete this codelab: 45-60 minutes.**

**本 codelab 的預期完成時間約為 45 - 60 分鐘**

## Row and Column classes

## Row 和 Column 類

`Row` and `Column` are classes that contain and lay out widgets.
Widgets inside of a `Row` or `Column` are called *children*,
and `Row` and `Column` are referred to as *parents*.
`Row` lays out its widgets horizontally,
and `Column` lays out its widgets vertically.

`Row` 和 `Column` 是兩個用來容納和佈局 widgets 的類別。
在它們內部的 widgets 我們稱為 *children*，
`Row` 和 `Column` 就作為它們的父級。
`Row` 將會讓 widgets 水平排列，而 Column 則會讓其豎直排列。

#### Example: Creating a Column
{:.no_toc}

#### 範例：建立一個 Column
{:.no_toc}

{{site.alert.secondary}}
{:.no_toc}
  The following example displays the differences between
  a `Row` and `Column`.

  **1.** Click the **Run** button.

  **1.** 點選 **執行** 按鈕。

  **2.** In the code, change the `Row` to a `Column`, and run again.

  **2.** 在這段程式碼中，將 `Row` 改為 `Column` 並再次執行。

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        BlueBox(),
        BlueBox(),
        BlueBox(),
      ],
    );
  }
}

class BlueBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 50,
      height: 50,
      decoration: BoxDecoration(
        color: Colors.blue,
        border: Border.all(),
      ),
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.ltr,
      child: Container(
        color: const Color(0xffeeeeee),
        child: Center(
          child: Container(
            color: const Color(0xffcccccc),
            child: MyWidget(),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;
  
  runApp(MyApp());

  final controller = LiveWidgetController(WidgetsBinding.instance);

  final columns = controller.widgetList(find.byType(Column));

  if (columns.isEmpty) {
    _result(false, ['The Row contains three BlueBox widgets and lays them out horizontally.']);
    return;
  }

  if (columns.length > 1) {
    _result(false, ['Found ${columns.length} Rows, rather than just one.']);
    return;
  }

  final column = columns.first as Column;

  if (column.children.length != 3 || column.children.any((w) => w is! BlueBox)) {
    _result(false, ['Row/Column should contain three children, all BlueBox widgets.']);
    return;
  }

  _result(true, ['The Column contains three BlueBox widgets and lays them out vertically.']);
}
{$ end test.dart $}
```

## Axis size and alignment

## 軸大小和對齊方式

So far, the `BlueBox` widgets have been squished together
(either to the left or at the top of the UI Output).
You can change how the `BlueBox` widgets are spaced
out using the axis size and alignment properties.

至此，`BlueBox` widget 已經在一起被壓扁了 (在介面的左邊或者上面 )。
你可以透過軸大小和對齊屬性來改變 `BlueBox` widget 的間距。

### mainAxisSize property

### mainAxisSize 屬性

`Row` and `Column` occupy different main axes.
A `Row`'s main axis is horizontal,
and a `Column`'s main axis is vertical.
The `mainAxisSize` property determines how much
space a `Row` and `Column` can occupy on their main axes.
The `mainAxisSize` property has two possible values:

`Row` 和 `Column`分別佔據了不同的主軸。`Row` 的主軸是水平的。
 `mainAxisSize` 決定了 `Row` 和 `Column` 能夠在主軸上佔據多大空間。
`mainAxisSize` 有兩個可選屬性：

`MainAxisSize.max`
<br> `Row` and `Column` occupy all of the space on their main axes.
  If the combined width of their children is
  less than the total space on their main axes,
  their children are laid out with extra space.

`MainAxisSize.max`
<br>  `Row` 和 `Column` 佔據它們主軸上所有空間。
  如果子 widget 的總寬度小於主軸上的空間，它們就會充滿剩餘的空間。

`MainAxisSize.min`
<br> `Row` and `Column` only occupy enough space on their main axes
  for their children. Their children are laid out without extra space
  and at the middle of their main axes.

`MainAxisSize.min`
<br>  `Row` 和 `Column` 僅佔據它的 children 在主軸上所需的空間，
  它的 children 在主軸之間將沒有額外空間。

{{site.alert.tip}}

  `MainAxisSize.max` is the `mainAxisSize` property's default value.
  If you don't specify another value,
  the default value is used,
  as shown in the previous example.

  `mainAxisSize` 預設為 `MainAxisSize.max`。
  如果你不特別指定其他的值，就會使用預設值，就像我們前一個樣例中展示的那樣。

{{site.alert.end}}

#### Example: Modifying axis size
{:.no_toc}

#### 範例：自訂軸大小
{:.no_toc}

{{site.alert.secondary}}
{:.no_toc}

  The following example explicitly sets `mainAxisSize`
  to its default value, `MainAxisSize.max`.

  下面的範例將會特別指定 `mainAxisSize` 為其預設值 `MainAxisSize.max`。

  **1.** Click the **Run** button.

  **1.** 點選 **執行** 按鈕。

  **2.** Change `MainAxisSize.max` to `MainAxisSize.min`,
         and run again.

  **2.** 將 `MainAxisSize.max` 改為 `MainAxisSize.min`，並再次執行。 

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisSize: MainAxisSize.max,
      children: [
        BlueBox(),
        BlueBox(),
        BlueBox(),
      ],
    );
  }
}

class BlueBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 50,
      height: 50,
      decoration: BoxDecoration(
        color: Colors.blue,
        border: Border.all(),
      ),
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.ltr,
      child: Container(
        color: const Color(0xffeeeeee),
        child: Center(
          child: Container(
            color: const Color(0xffcccccc),
            child: MyWidget(),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();

  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });

  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  final rows = controller.widgetList(find.byType(Row));

  if (rows.isEmpty) {
    _result(false, ['Couldn\'t find Row!']);
    return;
  }

  if (rows.length > 1) {
    _result(false, ['Found ${rows.length} Rows, rather than just one.']);
    return;
  }

  final row = rows.first as Row;

  if (row.mainAxisSize != MainAxisSize.min) {
    _result(false, ['Row lays out the BlueBox widgets with extra space. Change MainAxisSize.max to MainAxisSize.min']);
    return;
  }

  if (row.children.length != 3 || row.children.any((w) => w is! BlueBox)) {
    _result(false, ['There should only be three children, all BlueBox widgets.']);
    return;
  }

  _result(true, ['Row lays out the BlueBox widgets without extra space, and the BlueBox widgets are positioned at the middle of Row\'s main axis.']);
}
{$ end test.dart $}
```

### mainAxisAlignment property

### mainAxisAlignment 屬性 

When `mainAxisSize` is set to `MainAxisSize.max`,
`Row` and `Column` might lay out their children with extra space.
The `mainAxisAlignment` property determines how `Row` and `Column`
can position their children in that extra space.
`mainAxisAlignment` has six possible values:

當 `mainAxisSize` 被設為 `MainAxisSize.max`,
`Row` 和 `Column` 將會使用額外空間來對齊它的 children。
`mainAxisAlignment` 屬性決定了 `Row` 和 `Column` 將會在額外空間中如何對齊它的 children。
`mainAxisAlignment` 有以下六個可選屬性：

`MainAxisAlignment.start`
<br> Positions children near the beginning of the main axis. 
  (Left for `Row`, top for `Column`)

`MainAxisAlignment.start`
<br> 將其 children 從主軸起點處開始對齊。 (`Row` 的起點在左邊，`Column` 的起點在頂部 )

`MainAxisAlignment.end`
<br> Positions children near the end of the main axis. 
  (Right for `Row`, bottom for `Column`)

`MainAxisAlignment.end`
<br> 將其 children 從主軸終點處開始對齊。 (`Row` 的終點在右邊，`Column` 的終點在底部 )

`MainAxisAlignment.center`
<br> Positions children at the middle of the main axis.

`MainAxisAlignment.center`
<br> 將其 children 置於主軸中心。

`MainAxisAlignment.spaceBetween`
<br> Divides the extra space evenly between children.

`MainAxisAlignment.spaceBetween`
<br> 在 children 之間平均分配額外空間。

`MainAxisAlignment.spaceEvenly`
<br> Divides the extra space evenly between children
  and before and after the children.
  
`MainAxisAlignment.spaceEvenly`
<br> 在 children 之間，以及第一個 children 之前和最後一個 children 之後，平均分配額外空間。

`MainAxisAlignment.spaceAround`
<br> Similar to `MainAxisAlignment.spaceEvenly`,
  but reduces the space before the first
  child and after the last child
  to half of the width between the children.

`MainAxisAlignment.spaceAround`
<br> 與 `MainAxisAlignment.spaceEvenly` 相似，
  但在第一個 child 之前以及最後一個孩子之後減少了一半的空間，
  讓其 children 之間寬度縮減一半。

#### Example: Modifying main axis alignment
{:.no_toc}

#### 範例：自訂主軸對齊方式
{:.no_toc}

{{site.alert.secondary}}

  The following example explicitly sets
  `mainAxisAlignment` to its default value,
  `MainAxisAlignment.start`.

  下面的範例將特別指定 `mainAxisAlignment` 為其預設值，
  `MainAxisAlignment.start`。

  **1.** Click the **Run** button.

  **1.** 點選**執行**按鈕。

  **2.** Change `MainAxisAlignment.start` to
         `MainAxisAlignment.end`, and run again.

  **2.** 將 `MainAxisAlignment.start` 改為 `MainAxisAlignment.end`，然後再次執行。

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.start,
      children: [
        BlueBox(),
        BlueBox(),
        BlueBox(),
      ],
    );
  }
}

class BlueBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 50,
      height: 50,
      decoration: BoxDecoration(
        color: Colors.blue,
        border: Border.all(),
      ),
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.ltr,
      child: Container(
        color: const Color(0xffeeeeee),
        child: Center(
          child: Container(
            color: const Color(0xffcccccc),
            child: MyWidget(),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();

  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });

  await completer.future;
  final controller = LiveWidgetController(WidgetsBinding.instance);

  final rows = controller.widgetList(find.byType(Row));

  if (rows.isEmpty) {
    _result(false, ['Couldn\'t find a Row!']);
    return;
  }

  if (rows.length > 1) {
    _result(false, ['Found ${rows.length} Rows, rather than just one.']);
    return;
  }

  final row = rows.first as Row;

  if (row.mainAxisSize != MainAxisSize.max) {
    _result(false, ['It\'s best to leave the mainAxisSize set to MainAxisSize.max, so there\'s space for the alignments to take effect.']);
    return;
  }

  if (row.children.length != 3 || row.children.any((w) => w is! BlueBox)) {
    _result(false, ['The Row should have three children, all BlueBox widgets.']);
    return;
  }

  if (row.mainAxisAlignment == MainAxisAlignment.start) {
    _result(false, ['MainAxisAlignment.start positions the BlueBox widgets on the left of the main axis. Change the value to MainAxisAlignment.end.']);
  } else if (row.mainAxisAlignment == MainAxisAlignment.end) {
    _result(true, ['MainAxisAlignment.end positions the BlueBox widgets on the right of the main axis.']);
  } else if (row.mainAxisAlignment == MainAxisAlignment.center) {
    _result(true, ['MainAxisAlignment.center positions the BlueBox widgets at the middle of the main axis.']);
  } else if (row.mainAxisAlignment == MainAxisAlignment.spaceBetween) {
    _result(true, ['The extra space is divided between the BlueBox widgets.']);
  } else if (row.mainAxisAlignment == MainAxisAlignment.spaceEvenly) {
    _result(true, ['The extra space is divided evenly between the BlueBox widgets and before and after them.']);
  } else if (row.mainAxisAlignment == MainAxisAlignment.spaceAround) {
    _result(true, ['Similar to MainAxisAlignment.spaceEvenly, but reduces half of the space before the first BlueBox widget and after the last BlueBox widget to half of the width between the BlueBox widgets.']);
  }
}
{$ end test.dart $}
```
{{site.alert.tip}}

  Before moving to the next section,
  change `MainAxisAlignment.end` to another value.

  在閱讀下一個小節之前，將 `MainAxisAlignment.end` 換成其他值試試看。

{{site.alert.end}}

### crossAxisAlignment property

### crossAxisAlignment 屬性

The `crossAxisAlignment` property determines
how `Row` and `Column` can position their children
on their cross axes.
A `Row`'s cross axis is vertical,
and a `Column`'s cross axis is horizontal.
The `crossAxisAlignment` property has five possible values:

`crossAxisAlignment` 屬性決定了 `Row` 和 `Column`
能夠如何在其橫軸上定位 children。
`Row` 的橫軸是豎直的，而 `Column` 則是水平的，
`crossAxisAlignment` 屬性有五個可選屬性：

`CrossAxisAlignment.start`
<br> Positions children near the start of the cross axis. (Top for `Row`, Left for `Column`)

`CrossAxisAlignment.start`
<br> 將 children 放置在交叉軸的靠前位置（即 `Row` 佈局的頂部，`Column` 佈局的左側）。

`CrossAxisAlignment.end`
<br> Positions children near the end of the cross axis. (Bottom for `Row`, Right for `Column`)

`CrossAxisAlignment.end`
<br> 將 children 放置在交叉軸的靠後位置（即 `Row` 佈局的底部，`Column` 佈局的右側）。

`CrossAxisAlignment.center`
<br> Positions children at the middle of the cross axis. (Middle for `Row`, Center for `Column`)

`CrossAxisAlignment.center`
<br> 將 children 放置在交叉軸的中心位置（即 `Row` 佈局和 `Column` 佈局的中間）。

`CrossAxisAlignment.stretch`
<br> Stretches children across the cross axis. 
  (Top-to-bottom for `Row`, left-to-right for `Column`)

`CrossAxisAlignment.stretch`
<br> 使 children 在交叉軸上進行拉伸填充（即 `Row` 佈局是縱向的拉伸，`Column` 佈局是橫向的拉伸）。

`CrossAxisAlignment.baseline`
<br> Aligns children by their character baselines.
  (`Text` class only, and requires that the
  `textBaseline` property is set to
  `TextBaseline.alphabetic`.  See the
  [Text widget](#text-widget) section for an example.)

`CrossAxisAlignment.baseline`
<br> 根據 children 的基線對子節點。
   (僅限`Text`類，並要求 `textBaseline` 屬性設定為
  `TextBaseline.alphabetic`。
  在 [Text widget](#text-widget) 小節中檢視範例。

#### Example: Modifying cross axis alignment
{:.no_toc}

#### 範例：自訂橫軸對齊方式
{:.no_toc}

{{site.alert.secondary}}

  The following example explicitly sets `crossAxisAlignment`
  to its default value, `CrossAxisAlignment.center`.

  下面的範例將特別指定 `crossAxisAlignment` 為其預設值，
   `CrossAxisAlignment.center`。

  To demonstrate cross axis alignment,
  `mainAxisAlignment` is set to
 `MainAxisAlignment.spaceAround`,
  and `Row` now contains a `BiggerBlueBox` widget
  that is taller than the `BlueBox` widgets.

  為了示範橫軸對齊方式，`mainAxisAlignment` 被設為
  `MainAxisAlignment.spaceAround`，`Row` 現在包含一個比
  “BlueBox” widget 更高的 `BiggerBlueBox` widget。

  **1.** Click the **Run** button.

  **1.** 點選**執行**按鈕

  **2.** Change `CrossAxisAlignment.center` to
         `CrossAxisAlignment.start`, and run again.
  **2.** 將 `CrossAxisAlignment.center` 改為 
   `CrossAxisAlignment.start`，並再次執行。

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.spaceAround,
      crossAxisAlignment: CrossAxisAlignment.center,
      children: [
        BlueBox(),
        BiggerBlueBox(),
        BlueBox(),
      ],
    );
  }
}

class BlueBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 50,
      height: 50,
      decoration: BoxDecoration(
        color: Colors.blue,
        border: Border.all(),
      ),
    );
  }
}

class BiggerBlueBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 50,
      height: 100,
      decoration: BoxDecoration(
        color: Colors.blue,
        border: Border.all(),
      ),
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.ltr,
      child: Container(
        color: const Color(0xffeeeeee),
        child: Center(
          child: Container(
            color: const Color(0xffcccccc),
            child: MyWidget(),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  final rows = controller.widgetList(find.byType(Row));

  if (rows.isEmpty) {
    _result(false, ['Couldn\'t find a Row!']);
    return;
  }

  if (rows.length > 1) {
    _result(false, ['Found ${rows.length} Rows, rather than just one.']);
    return;
  }

  final row = rows.first as Row;

  if (row.children.length != 3 || row.children.any((w) => w is! BlueBox && w is! BiggerBlueBox)) {
    _result(false, ['The Row should have three children, all BlueBox or BiggerBlueBox widgets.']);
    return;
  }

  if (row.crossAxisAlignment == CrossAxisAlignment.start) {
    _result(true, ['The BlueBox and BiggerBlueBox widgets are positioned at the top of the cross axis.']);
  } else if (row.crossAxisAlignment == CrossAxisAlignment.end) {
    _result(true, ['The BlueBox and BiggerBlueBox widgets are positioned at the bottom of the cross axis']);
  } else if (row.crossAxisAlignment == CrossAxisAlignment.center) {
    _result(false, ['The widgets are positioned at the middle of the cross axis. Change CrossAxisAlignment.center to CrossAxisAlignment.start.']);
  } else if (row.crossAxisAlignment == CrossAxisAlignment.stretch) {
    _result(true, ['The BlueBox and BiggerBlueBox widgets are stretched across the cross axis. Change the Row to a Column, and run again.']);
  } else if(row.crossAxisAlignment == CrossAxisAlignment.baseline) {
    _result(false, ['Couldn\t find a text class.']);
  }
}
{$ end test.dart $}
```
{{site.alert.tip}}

  Before moving to the next section,
  change `CrossAxisAlignment.start` to another value.

  在閱讀下一個小節之前，將 `CrossAxisAlignment.start` 改為其他值試試。

{{site.alert.end}}

## Flexible widget

As you've seen, the `mainAxisAlignment` and
`crossAxisAlignment` properties determine
how `Row` and `Column` position widgets along both axes.
`Row` and `Column` first lay out widgets of a fixed size.
Fixed size widgets are considered *inflexible* because
they can't resize themselves after they've been laid out.

正如你所看到，`mainAxisAlignment` 和 `crossAxisAlignment` 屬性
決定了 `Row` 和 `Column`在各個軸上如何佈局 widget。
`Row` 和 `Column` 首先佈置固定大小的 widget。
固定大小的小部件被認為是 **不靈活的** 因為它們佈局後無法自我調整大小。

The `Flexible` widget wraps a widget,
so the widget becomes resizable.
When the `Flexible` widget wraps a widget,
the widget becomes the `Flexible` widget's child
and is considered *flexible*.
After inflexible widgets are laid out,
the widgets are resized according to their
`flex` and `fit` properties:

`Flexible` widget 包裹一個 widget 讓這個 widget 變得可以調整大小。
當 `Flexible` widget 包裹 widget 時，
這個 widget 就成為 `Flexible` widget 的子節點，
並被視為 *flexible* 的。在佈置固定大小的 widget 後，
Flex 的 widget 根據其 `flex` 和 `fit` 屬性調整大小：

`flex`
<br> Compares itself against other `flex`
  properties before determining what fraction of the
  total remaining space each `Flexible` widget receives.

`flex`
<br> 將自身的 `flex` 因子與其他的比較，以決定自身佔剩餘空間的比例。

`fit`
<br> Determines whether a `Flexible` widget
  fills all of its extra space.

`fit`
<br> 決定 `Flexible` 的 widget 是否能夠填充所有剩餘空間。 

#### Example: Changing fit properties
{:.no_toc}

#### 範例：改變 fit 屬性
{:.no_toc}

{{site.alert.secondary}}

  The following example demonstrates the `fit` property,
  which can have one of two values:

  下面的範例示範了 `fit` 屬性，它可以使用這兩個值之一：

  `FlexFit.loose`
  <br> The widget's preferred size is used. (Default)

  `FlexFit.loose`
  <br> 使用 widget 的自身作為首選大小。 (預設情況下)

  `FlexFit.tight`
  <br> Forces the widget to fill all of its extra space.

  `FlexFit.tight`
  <br> 強制 widget 充滿所有剩餘空間。

  In this example, change the `fit` properties to
  make the `Flexible` widgets fill the extra space.

  在這個範例中，改變 `fit` 屬性使 `Flexible` widgets
  能夠填充剩餘空間。

  **1.** Click the **Run** button.

  **1.** 點選**執行**按鈕

  **2.** Change both `fit` values to `FlexFit.tight`,
         and run again.

  **2.** 將所有 `fit` 的值設為 `FlexFit.tight`，並再次執行。

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        BlueBox(),
        Flexible(
          fit: FlexFit.loose,
          flex: 1,
          child: BlueBox(),
        ),
        Flexible(
          fit: FlexFit.loose,
          flex: 1,
          child: BlueBox(),
        ),
      ],
    );
  }
}

class BlueBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 50,
      height: 50,
      decoration: BoxDecoration(
        color: Colors.blue,
        border: Border.all(),
      ),
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.ltr,
      child: Container(
        color: const Color(0xffeeeeee),
        child: Center(
          child: Container(
            color: const Color(0xffcccccc),
            child: MyWidget(),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  final rows = controller.widgetList(find.byType(Row));

  if (rows.isEmpty) {
    _result(false, ['Couldn\'t find a Row!']);
    return;
  }

  if (rows.length > 1) {
    _result(false, ['Found ${rows.length} Rows, rather than just one.']);
    return;
  }

  final row = rows.first as Row;

  if (row.mainAxisSize != MainAxisSize.max) {
    _result(false, ['It\'s best to leave the mainAxisSize set to MainAxisSize.max, so there\'s space for the alignments to take effect.']);
    return;
  }
  
  if (row.children.length != 3) {
    _result(false, ['The Row should have three children, all BlueBox or Flexible widgets.']);
    return;
  }

  if (row.children[0] is! BlueBox) {
    _result(false, ['Row\'s first child should be a BlueBox.']);
    return;
  }

  if (row.children[1] is! Flexible) {
    _result(false, ['Row\'s second child should be a Flexible class.']);
    return;
  }

  if (row.children[2] is! Flexible) {
    _result(false, ['Row\'s third child should be a Flexible class.']);
    return;
  }

  final flexibleWidget = row.children[2] as Flexible;
  
  if (flexibleWidget.child is! BlueBox) {
    _result(false, ['The Flexible classes should have BlueBox widgets as their children.']);
    return;
  }

  if (flexibleWidget.fit != FlexFit.tight) {
    _result(false, ['The fit properties set the Flexible widgets to their preferred size. Change both fit values to FlexFit.tight.']);
    return;
  }

  _result(true, ['The Flexible widgets now occupy the space determined by their flex values.']);
}
{$ end test.dart $}
```

#### Example: Testing flex values
{:.no_toc}

#### 範例：測試 flex 值
{:.no_toc}

{{site.alert.secondary}}

  In the following example,
  `Row` contains one `BlueBox` widget
  and two `Flexible` widgets that wrap two
  `BlueBox` widgets.  The `Flexible` widgets
  contain `flex` properties with `flex`
  values set to 1 (the default value).

  在下面這個例子中，`Row` 包含了一個 `BlueBox` widget 和
  兩個 `Flexible` widgets 包裹的 `BlueBox` widget。
  `Flexible` widgets 包含了 `flex` 屬性，並將其值設為 1。 (預設值 )

  When `flex` properties are compared against one another,
  the ratio between their `flex` values determines
  what fraction of the total remaining space each
  `Flexible` widget receives.

  當 `flex` 屬性互相比較時，它們的 `flex` 值的比率決定了 `Flexible` widget
  自身所佔剩餘空間的比例。

  ```dart
  remainingSpace * (flex / totalOfAllFlexValues)
  ```

  In this example, the sum of the `flex` values (2),
  determines that both `Flexible` widgets receive
  half of the total remaining space.
  The `BlueBox` widget (or fixed-size widget)
  remains the same size.

  在這個例子中，`flex` 值的總和為 (2 )，這決定了每個 `Flexible` widgets 
  都能分到總剩餘空間的一半空間。`BlueBox` widget (或是 fixed-size widget )得到了相同的大小。

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        BlueBox(),
        Flexible(
          fit: FlexFit.tight,
          flex: 1,
          child: BlueBox(),
        ),
        Flexible(
          fit: FlexFit.tight,
          flex: 1,
          child: BlueBox(),
        ),
      ],
    );
  }
}

class BlueBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 50,
      height: 50,
      decoration: BoxDecoration(
        color: Colors.blue,
        border: Border.all(),
      ),
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.ltr,
      child: Container(
        color: const Color(0xffeeeeee),
        child: Center(
          child: Container(
            color: const Color(0xffcccccc),
            child: MyWidget(),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  final rows = controller.widgetList(find.byType(Row));

  if (rows.isEmpty) {
    _result(false, ['Couldn\'t find a Row!']);
    return;
  }

  if (rows.length > 1) {
    _result(false, ['Found ${rows.length} Rows, rather than just one.']);
    return;
  }

  final row = rows.first as Row;

  if (row.children.length != 3) {
    _result(false, ['The Row should have three children, all BlueBlox or Flexible widgets.']);
    return;
  }

  if (row.children[0] is! BlueBox) {
    _result(false, ['The Row\'s first child should be a BlueBox widget.']);
    return;
  }

  if (row.children[1] is! Flexible) {
    _result(false, ['The Row\'s second child should be a Flexible widget.']);
    return;
  }

  if (row.children[2] is! Flexible) {
    _result(false, ['The Row\'s third child should be a Flexible widget.']);
    return;
  }

  final flexibleWidget = row.children[1] as Flexible;
  
  if (flexibleWidget.child is! BlueBox) {
    _result(false, ['The Flexible should have a BlueBox widget as its child.']);
    return;
  }

  if (flexibleWidget.flex != 1) {
    _result(false, ['Notice how the flex properties divide the extra space between the two Flexible widgets.']);
    return;
  }

  _result(true, ['Both Flexible widgets receive half of the total remaining space.']);
}
{$ end test.dart $}
```
{{site.alert.tip}}

  Before moving to the next example,
  try changing the `flex` properties to other values,
  such as 2 and 1.

  在閱讀下個樣例之前，嘗試將 `flex` 屬性轉換為其他值，例如 2 和 1。

{{site.alert.end}}

## Expanded widget

Similar to `Flexible`, the `Expanded` widget can
wrap a widget and force the widget to fill extra space.

`Expanded` widget 能夠包裹一個 widget 並強制其填滿剩餘空間，
與 `Flexible` 非常相似。

{{site.alert.tip}}
  
  **What's the difference between Flexible and Expanded?**
  Use `Flexible` to resize widgets in a `Row` or `Column`.
  That way, you can adjust a child widget's spacing
  while keeping its size in relation to its parent widget.
  `Expanded` changes the constraints of a child widget,
  so it fills any empty space.

  **Flexible 和 Expanded 有何不同呢？**
  使用 `Flexible` 在  `Row` 或 `Column` 中重新調整 widgets 的大小。
  這樣，你就可以調整子 widget 的間距同時保持其相對於父 widget  的大小。
  `Expanded`改變子視窗小部件的約束， 所以它會填自動完成部空白空間。

{{site.alert.end}}

#### Example: Filling extra space
{:.no_toc}

#### 範例：填補額外空間
{:.no_toc}

{{site.alert.secondary}}

  The following example demonstrates how the
  `Expanded` widget forces its child widget to
  fill extra space.
  
  下面的例子示範了 `Expanded` widget 是如何
  強制其子 widget 填滿空間的。

  **1.** Click the **Run** button.
         
         點選**執行**按鈕 

  **2.** Wrap the second `BlueBox` widget in an `Expanded` widget.
  
         在第二個 `BlueBox` widget 外包裹一個 `Expanded` widget。

  For example:

  ```dart
  Expanded(child: BlueBox(),),
  ```
  **3.** Select the **Format** button to properly format the code,
         and run again.
         
         選擇 **Format** 按鈕格式化程式碼，然後再次執行。
{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        BlueBox(),
        BlueBox(),
        BlueBox(),
      ],
    );
  }
}

class BlueBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 50,
      height: 50,
      decoration: BoxDecoration(
        color: Colors.blue,
        border: Border.all(),
      ),
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.ltr,
      child: Container(
        color: const Color(0xffeeeeee),
        child: Center(
          child: Container(
            color: const Color(0xffcccccc),
            child: MyWidget(),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  final rows = controller.widgetList(find.byType(Row));

  if (rows.isEmpty) {
    _result(false, ['Couldn\'t find a Row!']);
    return;
  }

  if (rows.length > 1) {
    _result(false, ['Found ${rows.length} Rows, rather than just one.']);
    return;
  }

  final row = rows.first as Row;
  
  if (row.children.length != 3) {
    _result(false, ['The Row should have three children, all BlueBox widgets.']);
    return;
  }

   if (row.children[0] is! BlueBox) {
    _result(false, ['The Row\'s first child should be a BlueBox widget.']);
    return;
  }

  if (row.children[1] is! Expanded) {
    _result(false, ['Notice how Row contains extra space on its main axis. Wrap the second BlueBox widget in an Expanded widget.']);
    return;
  }

  if (row.children[2] is! BlueBox) {
    _result(false, ['The Row\'s third child should be a Flexible widget.']);
    return;
  }
  
  _result(true, ['Expanded forces second BlueBox widget to fill the extra space.']);
}
{$ end test.dart $}
```

## SizedBox widget

The `SizedBox` widget can be used in one of two ways when
creating exact dimensions.
When `SizedBox` wraps a widget, it resizes the widget
using the `height` and `width` properties.
When it doesn't wrap a widget,
it uses the `height` and `width` properties to
create empty space.

`SizedBox` widget 的兩種用途之一就是建立精確的尺寸。
當 `SizedBox` 包裹了一個 widget 時，
它會使用 `height` 和 `width` 調整其大小。
如果它沒有包裹 widget，
它可以使用`height`和`width`屬性創造空的空間。

#### Example: Resizing a widget
{:.no_toc}

#### 範例：調整一個 widget
{:.no_toc}

{{site.alert.secondary}}

  The following example wraps the middle `BlueBox` widget inside of a
  `SizedBox` widget and sets the `BlueBox`'s width to 100 logical pixels.

  下面的範例使用 `SizedBox` widget 包裹了中間的 `BlueBox` widget，
  並將 `BlueBox` 的寬度設為 100 邏輯畫素。

  **1.** Click the **Run** button.

  **1.** 點選**執行**按鈕

  **2.** Add a `height` property equal to 100 logical pixels
         inside the `SizedBox` widget, and run again.

  **2.** 將 `SizedBox` widget 中的 `height` 設為 100 邏輯畫素，並重新執行。

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisSize: MainAxisSize.max,
      children: [
        BlueBox(),
        SizedBox(
          width: 100,
          child: BlueBox(),
        ),
        BlueBox(),
      ],
    );
  }
}

class BlueBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 50,
      height: 50,
      decoration: BoxDecoration(
        color: Colors.blue,
        border: Border.all(),
      ),
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.ltr,
      child: Container(
        color: const Color(0xffeeeeee),
        child: Center(
          child: Container(
            color: const Color(0xffcccccc),
            child: MyWidget(),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  final rows = controller.widgetList(find.byType(Row));

  if (rows.isEmpty) {
    _result(false, ['Couldn\'t find a Row!']);
    return;
  }

  if (rows.length > 1) {
    _result(false, ['Found ${rows.length} Rows, rather than just one.']);
    return;
  }

  final row = rows.first as Row;

  if (row.mainAxisSize != MainAxisSize.max) {
    _result(false, ['It\'s best to leave the mainAxisSize set to MainAxisSize.max, so there\'s space for the alignments to take effect.']);
    return;
  }
  
  
  if (row.children.length != 3) {
    _result(false, ['The Row should end up with three children.']);
    return;
  }

  if (row.children[0] is! BlueBox) {
    _result(false, ['The Row\'s first child should be a BlueBox widget.']);
    return;
  }

  if (row.children[1] is! SizedBox) {
    _result(false, ['The Row\'s second child should be a SizedBox widget.']);
    return;
  }

  if (row.children[2] is! BlueBox) {
    _result(false, ['The Row\'s third child should be a BlueBox widget.']);
    return;
  }

  final sizedBox = row.children[1] as SizedBox;
  
  if (sizedBox.width != 100) {
    _result(false, ['The SizedBox should have a width of 100.']);
    return;
  }
  
  if (sizedBox.height != 100) {
    _result(false, ['The SizedBox widget resizes the BlueBox widget to 100 logical pixels wide. Add a height property inside SizedBox equal to 100 logical pixels.']);
    return;
  }
  
  _result(true, ['The SizedBox widget resizes the BlueBox widget to 100 logical pixels wide and tall.']);
}
{$ end test.dart $}
```

#### Example: Creating space
{:.no_toc}

#### 範例：建立空間
{:.no_toc}

{{site.alert.secondary}}

  The following example contains three `BlueBox` widgets
  and one `SizedBox` widget that separates the first
  and second `BlueBox` widgets. The `SizedBox` widget
  contains a `width` property equal to 50 logical pixels.

  下面的範例將會包含三個 `BlueBox` widget，
  其中 第一個和第二個 `BlueBox` widget 包裹 `SizedBox`。
  `SizedBox` widget 的 `width` 設為 50 邏輯畫素。

  **1.** Click the **Run** button.

  **1.** 點選**執行**按鈕

  **2.** Create more space by adding another
         `SizedBox` widget (25 logical pixels wide)
         between the second and third `BlueBox` widgets,
         and run again.
  **2.** 透過在第二個和第三個 `BlueBox` widget 之間新增另一個
  `SizedBox` widget (寬 25 邏輯畫素) 以建立更多空間。 

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        BlueBox(),
        const SizedBox(width: 50),
        BlueBox(),
        BlueBox(),
      ],
    );
  }
}

class BlueBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 50,
      height: 50,
      decoration: BoxDecoration(
        color: Colors.blue,
        border: Border.all(),
      ),
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.ltr,
      child: Container(
        color: const Color(0xffeeeeee),
        child: Center(
          child: Container(
            color: const Color(0xffcccccc),
            child: MyWidget(),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  final rows = controller.widgetList(find.byType(Row));

  if (rows.isEmpty) {
    _result(false, ['Couldn\'t find a Row!']);
    return;
  }

  if (rows.length > 1) {
    _result(false, ['Found ${rows.length} Rows, rather than just one.']);
    return;
  }

  final row = rows.first as Row;

  if (row.mainAxisSize != MainAxisSize.max) {
    _result(false, ['It\'s best to leave the mainAxisSize set to MainAxisSize.max, so there\'s space for the alignments to take effect.']);
    return;
  }
  
  if (row.mainAxisAlignment == MainAxisAlignment.spaceAround 
      || row.mainAxisAlignment == MainAxisAlignment.spaceBetween 
      || row.mainAxisAlignment == MainAxisAlignment.spaceEvenly) {
    _result(false, ['It\'s best to use MainAxisAlignment.start, MainAxisAlignment.end, or MainAxisAlignment.center to see how the SizedBox widgets work in a Row.']);
    return;
  }
  
  if (row.children.length != 5) {
    _result(false, ['The SizedBox widget creates space at 50 logical pixels wide. Add another SizedBox class between the second and third BlueBox widgets with a width property equal to 25 logical pixels.']);
    return;
  }

  if (row.children[0] is! BlueBox) {
    _result(false, ['The Row\'s first child should be a BlueBox widget.']);
    return;
  }

  if (row.children[1] is! SizedBox) {
    _result(false, ['The Row\'s second child should be a SizedBox widget.']);
    return;
  }

  if (row.children[2] is! BlueBox) {
    _result(false, ['The Row\'s third child should be a BlueBox widget.']);
    return;
  }

  if (row.children[3] is! SizedBox) {
    _result(false, ['The Row\'s fourth child should be a SizedBox widget.']);
    return;  
  }
  
   if (row.children[4] is! BlueBox) {
    _result(false, ['The Row\'s fifth child should be a BlueBox widget.']);
    return;
  }
  
  final sizedBox = row.children[1] as SizedBox;
  
  if (sizedBox.width != 50) {
    _result(false, ['The SizedBox should have a width of 50.']);
    return;
  }
  
  final sizedBox2 = row.children[3] as SizedBox;
  
  if (sizedBox2.width != 25) {
    _result(false, ['SizedBox should have a width of 25.']);
    return;
  }
  
  _result(true, ['The SizedBox widgets create space between the BlueBox widgets, one space at 50 logical pixels and one at 25 logical pixels.']);
}
{$ end test.dart $}
```

## Spacer widget

Similar to `SizedBox`, the `Spacer` widget also
can create space between widgets.

與 `SizedBox` 相似，`Spacer` widget 也能在 widgets 之間建立空間。

{{site.alert.tip}}

  **What's the difference between SizedBox and Spacer?**
  Use `Spacer` when you want to create space using a `flex` property.
  Use `SizedBox` when you want to create space
  using a specific number of logical pixels.
  **SizedBox 和 Spacer 有何不同？**
  如果你想用 `flex` 屬性建立一段空間，請使用 `Spacer`。
  如果你想建立一個擁有特定邏輯畫素值的空間，請使用 `SizedBox`。

{{site.alert.end}}

#### Example: Creating more space
{:.no_toc}

#### Example：建立更多空間
{:.no_toc}

{{site.alert.secondary}}

  The following example separates the first two
  `BlueBox` widgets using a `Spacer` widget with
  a `flex` value of 1.

  下面的範例使用 `flex` 值為 1 的 `Spacer` widget，
  分隔最初的兩個 `BlueBox` widget。

  **1.** Click the **Run** button.

  **1.** 點選**執行**按鈕

  **2.** Add another `Spacer` widget (also with a `flex` value of 1)
         between the second and third `BlueBox` widgets.

  **2.** 在第二個和第三個 `BlueBox` widget 之間新增另一個 `Spacer` widget。
      (flex 值仍然為 1)

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        BlueBox(),
        const Spacer(flex: 1),
        BlueBox(),
        BlueBox(),
      ],
    );
  }
}

class BlueBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 50,
      height: 50,
      decoration: BoxDecoration(
        color: Colors.blue,
        border: Border.all(),
      ),
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.ltr,
      child: Container(
        color: const Color(0xffeeeeee),
        child: Center(
          child: Container(
            color: const Color(0xffcccccc),
            child: MyWidget(),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  final rows = controller.widgetList(find.byType(Row));

  if (rows.isEmpty) {
    _result(false, ['Couldn\'t find a Row!']);
    return;
  }

  if (rows.length > 1) {
    _result(false, ['Found ${rows.length} Rows, rather than just one.']);
    return;
  }

  final row = rows.first as Row;

  if (row.mainAxisSize != MainAxisSize.max) {
    _result(false, ['It\'s best to leave the mainAxisSize set to MainAxisSize.max, so there\'s space for the alignments to take effect.']);
    return;
  }
  
  if (row.mainAxisAlignment == MainAxisAlignment.spaceAround 
      || row.mainAxisAlignment == MainAxisAlignment.spaceBetween 
      || row.mainAxisAlignment == MainAxisAlignment.spaceEvenly) {
    _result(false, ['It\'s best to use MainAxisAlignment.start, MainAxisAlignment.end, or MainAxisAlignment.center to see how the SizedBox widgets work in a Row.']);
    return;
  }
  
  if (row.children.length != 5) {
    _result(false, ['What do you think would happen if you added another Spacer widget with a flex value of 1 between the second and third BlueBox widgets?']);
    return;
  }

  if (row.children[0] is! BlueBox ||
     row.children[1] is! Spacer ||
     row.children[2] is! BlueBox ||
     row.children[3] is! Spacer ||
     row.children[4] is! BlueBox) {
    _result(false, ['Not quite. Row should contain five children in this order: BlueBox, Spacer, BlueBox, Spacer, BlueBox.']);
    return;
  }
  
    final spacer = row.children[3] as Spacer;
  
    if (spacer.flex != 1) {
    _result(false, ['The Spacer class should have a flex equal to 1.']);
    return;
  }
  
  _result(true, ['Both Spacer widgets create equal amounts of space between all three BlueBox widgets.']);
}
{$ end test.dart $}
```

## Text widget

The `Text` widget displays text and can be configured
for different fonts, sizes, and colors.

`Text` widget 不僅能夠顯示文字，並能夠配置不同的字型，大小和顏色。

#### Example: Aligning text
{:.no_toc}

#### 範例：文字對齊
{:.no_toc}

{{site.alert.secondary}}
  
  The following example displays "Hey!" three times,
  but at different font sizes and in different colors.
  `Row` specifies the `crossAxisAlignment`
  and `textBaseline` properties.

  下面的範例將會顯示三次"Hey!"，但是使用不同的字型和不同顏色。
  特別指定 `Row` 的 `crossAxisAlignment` 和 `textBaseline` 屬性。

  **1.** Click the **Run** button.

  **1.** 點選**執行**按鈕

  **2.** Change `CrossAxisAlignment.center` to
         `CrossAxisAlignment.baseline`, and run again.

  **2.** 將 `CrossAxisAlignment.center` 改為 `CrossAxisAlignment.baseline`，
   然後再次執行。

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      crossAxisAlignment: CrossAxisAlignment.center,
      textBaseline: TextBaseline.alphabetic,
      children: const [
        Text(
          'Hey!',
          style: TextStyle(
            fontSize: 30,
            fontFamily: 'Futura',
            color: Colors.blue,
          ),
        ),
        Text(
          'Hey!',
          style: TextStyle(
            fontSize: 50,
            fontFamily: 'Futura',
            color: Colors.green,
          ),
        ),
        Text(
          'Hey!',
          style: TextStyle(
            fontSize: 40,
            fontFamily: 'Futura',
            color: Colors.red,
          ),
        ),
      ],
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.ltr,
      child: Container(
        color: const Color(0xffeeeeee),
        child: Center(
          child: Container(
            color: const Color(0xffcccccc),
            child: MyWidget(),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  final rows = controller.widgetList(find.byType(Row));

  if (rows.isEmpty) {
    _result(false, ['Couldn\'t find a Row!']);
    return;
  }

  if (rows.length > 1) {
    _result(false, ['Found ${rows.length} Rows, rather than just one.']);
    return;
  }

  final row = rows.first as Row;

  if (row.mainAxisSize != MainAxisSize.max) {
    _result(false, ['It\'s best to leave the mainAxisSize set to MainAxisSize.max, so there\'s space for the alignments to take effect.']);
    return;
  }
  
  if (row.children.length != 3 || row.children.any((w) => w is! Text)) {
    _result(false, ['The Row should have three children, all Text widgets.']);
    return;
  }

  if (row.textBaseline == null) {
    _result(false, ['To use CrossAxisAlignment.baseline, you need to set the Row\'s textBaseline property.']);
    return;
  } 
  
  if (row.crossAxisAlignment != CrossAxisAlignment.baseline) {
    _result(false, ['The Text widgets are positioned at the middle of the cross axis. Change CrossAxisAlignment.center to CrossAxisAlignment.baseline.']);
		return;
  }  

  _result(true, ['The Text widgets are now aligned by their character baselines.']);
  
}
{$ end test.dart $}
```

## Icon widget

The `Icon` widget displays a graphical symbol
that represents an aspect of the UI.
Flutter is preloaded with icon packages for
[Material][] and [Cupertino][] applications.

`Icon` widget 能夠顯示圖形符號，這代表了 UI 的一個方面。
Flutter 將會為 [Material][] 和 [Cupertino][]
的應用提前載入 icon packages。

#### Example: Creating an Icon
{:.no_toc}

#### 範例：建立一個 Icon
{:.no_toc}

{{site.alert.secondary}}

  The following example displays the widget `Icons.widget`
  from the [Material Icon library][] in red and blue.

  下面的範例顯示了來自 [Material Icon library][] 
  的紅藍 `Icons.widget` widget。

  **1.** Click the **Run** button.

  **1.** 點選 **執行** 按鈕

  **2.** Add another `Icon` from the
         [Material Icon library][]
         with a size of 50.

  **2.** 新增另一個來自 [Material Icon library][]
   的 `Icon` 並將其大小設為 50。

  **3.** Give the `Icon` a color of `Colors.amber` from the
         [Material Color palette][], and run again.

  **3.** 給 `Icon` 設定一個來自 [Material Color palette][] 的
   `Colors.amber` 顏色，然後再次執行。

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      crossAxisAlignment: CrossAxisAlignment.center,
      textBaseline: TextBaseline.alphabetic,
      children: const [
        Icon(
          Icons.widgets,
          size: 50,
          color: Colors.blue,
        ),
        Icon(
          Icons.widgets,
          size: 50,
          color: Colors.red,
        ),
      ],
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.ltr,
      child: Container(
        color: const Color(0xffeeeeee),
        child: Center(
          child: Container(
            color: const Color(0xffcccccc),
            child: MyWidget(),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  final rows = controller.widgetList(find.byType(Row));

  if (rows.isEmpty) {
    _result(false, ['Couldn\'t find a Row!']);
    return;
  }

  if (rows.length > 1) {
    _result(false, ['Found ${rows.length} Rows, rather than just one.']);
    return;
  }

  final row = rows.first as Row;

  if (row.mainAxisSize != MainAxisSize.max) {
    _result(false, ['It\'s best to leave the mainAxisSize set to MainAxisSize.max, so there\'s space for the alignments to       take effect.']);
    return;
  }
  
    if (row.children.length != 3 || row.children.any((w) => w is! Icon)) {
    _result(false, ['Row should have three children, all Icon widgets.']);
    return;
  }

    final icon = row.children[2] as Icon;
  
  if (icon.color != Colors.amber) {
    _result(false, ['Add a third Icon. Give the Icon a size of 50 and a color of Colors.amber.']);
    return;
  }
  
  _result(true, ['The code displays three Icons in blue, red, and amber.']);
  
}
{$ end test.dart $}
```

## Image widget

The `Image` widget displays an image.
You either can reference images using a URL,
or you can include images inside your app package.
Since DartPad can't package an image,
the following example uses an image from the network.

`Image` widget 顯示了一張圖片。你還能夠直接參考圖片 URL，
或是你的應用 package 中的圖片。但是由於 DartPad 無法參考包圖片，
所以下面的範例將會使用網路上的圖片。  

#### Example: Displaying an image
{:.no_toc}

#### 範例：顯示一張圖片
{:.no_toc}

{{site.alert.secondary}}

  The following example displays an image that's
  stored remotely on [GitHub][].
  The `Image.network` method takes a string
  parameter that contains an image's URL.

  下面的範例將會顯示一張儲存在遠端 [GitHub][] 上的圖片。
  `Image.network` 方法接收一個含有圖片 url 的字串。

  In this example, `Image.network` contains a non-working URL.

  在這個範例中，`Image.network` 包含了一個無法存取的 URL。
  
  In this example, `Image.network` contains a non-working URL.

  **1.** Click the **Run** button.

  **1.** 點選 **執行** 按鈕

  **2.** Change the non-working URL to the actual URL:

  **2.** 將那個無法存取的 URL 更換成可以存取的 URL:

  `https://raw.githubusercontent.com/flutter/website/main/examples/layout/sizing/images/pic1.jpg`

  **3.** Then change `pic1.jpg` to `pic2.jpg` or `pic3.jpg`,
         and run again.

  **3.** 然後將 `pic3.jpg` 改為 `pic1.jpg` 或 `pic2.jpg`，然後重新執行。

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        Image.network('[Place an image link here!]'),
      ],
    );
  }
}
{$ end main.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.ltr,
      child: Container(
        color: const Color(0xffeeeeee),
        child: Center(
          child: Container(
            color: const Color(0xffcccccc),
            child: MyWidget(),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  final rows = controller.widgetList(find.byType(Row));

  if (rows.isEmpty) {
    _result(false, ['Couldn\'t find a Row!']);
    return;
  }

  if (rows.length > 1) {
    _result(false, ['Found ${rows.length} Rows, rather than just one.']);
    return;
  }

  final row = rows.first as Row;

  if (row.mainAxisSize != MainAxisSize.max) {
    _result(false, ['It\'s best to leave the mainAxisSize set to MainAxisSize.max, so there\'s space for the alignments to take effect.']);
    return;
  }
 
}
{$ end test.dart $}
```

## Putting it all together

## 綜合練習

You're almost at the end of this codelab.
If you'd like to test your knowledge of the
techniques that you've learned, why not apply
those skills into building a Flutter UI that
displays a business card!

你就要完成這個 codelab 了！如果你想要檢驗你剛學的知識，
為何不講這些結合起來，建構一個顯示名片的 Flutter UI 呢！

 ![Completed business card]({{site.url}}/assets/images/docs/codelab/layout/businesscarddisplay1.png){:width="400px"}{:.text-center}

You'll break down Flutter's layout into parts,
which is how you'd create a Flutter UI in the real world.

你將會把 Flutter 的佈局分解成幾個部分，這就是如何在實際開發中建構 Flutter UI 方式！

In [Part 1](#part-1),
you'll implement a `Column` that contains the name and title.
Then you'll wrap the `Column` in a `Row` that contains the icon,
which is positioned to the left of the name and title.

在[第一部分](#part-1), 你將會實現包含姓名和標題的 `Column`。
然後你將會在 `Column` 包裹一個含有 icon 的 `Row`，
它將會被放在姓名和標題的左邊。

 ![Completed business card]({{site.url}}/assets/images/docs/codelab/layout/businesscarddisplay2.png){:width="400px"}{:.text-center}

In [Part 2](#part-2), you'll wrap the `Row` in a `Column`,
so the code contains a `Column` within a `Row` within a `Column`.
Then you'll tweak the outermost `Column`'s layout,
so it looks nice.
Finally, you'll add the contact information
to the outermost `Column`'s list of children,
so it's displayed below the name, title, and icon.

在[第二部分](#part-2)中，你將會在 `Row` 外包裹一個 `Column`，
所以你的程式碼中就包含了
一個 Column (Row (Column) )。然後你將調整最外面的`Column`的佈局，
所以它看起來不錯。最後，您將新增聯絡資訊到最外面的`Column`的 children 中，
所以它將顯示在名稱，標題和圖示下方。

 ![Completed business card]({{site.url}}/assets/images/docs/codelab/layout/businesscarddisplay3.png){:width="400px"}{:.text-center}

In [Part 3](#part-3), you'll finish building
the business card display by adding four more icons,
which are positioned below the contact information.

在[第三部分](#part-3)，你將會完成添加了更多圖示的名片，
它們會被放在聯絡資訊的下方。

 ![Completed business card]({{site.url}}/assets/images/docs/codelab/layout/businesscarddisplay4.png){:width="400px"}{:.text-center}

### Part 1
{:.no_toc}

### 第一部分
{:.no_toc}


#### Exercise: Create the name and title
{:.no_toc}

#### 練習：建立 name 和 title
{:.no_toc}

{{site.alert.secondary}}

  Implement a `Column` that contains two text widgets:

  實現含有兩個 Text widget 的 `Column`：

<ul markdown="1">
  <li markdown="1">
  The first `Text` widget has the name `Flutter McFlutter` and
  the `style` property set to `Theme.of(context).textTheme.headlineSmall`.

  第一個 `Text` widget 有一個叫做 `Flutter McFlutter` 的名字，
  並將其 `style` 屬性設為 `Theme.of(context).textTheme.headlineSmall`。
  </li>
  <li markdown="1">
  The second `Text` widget contains the title `Experienced App Developer`.

  第二個 `Text` widget 包含了標題 `Experienced Developer`。
  </li>
</ul>

  For the `Column`,
  set `mainAxisSize` to `MainAxisSize.min`
  and `crossAxisAlignment` to `CrossAxisAlignment.start`.

  對於這個 `Column`，將其 `mainAxisSize` 設為 `MainAxisSize.min`，
  `crossAxisAlignment` 設為 `CrossAxisAlignment.start`。

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    TODO('Begin implementing the Column here.');
  }
}
{$ end main.dart $}
{$ begin solution.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          'Flutter McFlutter', 
          style: Theme.of(context).textTheme.headlineSmall,
        ),
        const Text('Experienced App Developer'),
      ],
    );
  }
}
{$ end solution.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        scaffoldBackgroundColor: const Color(0xffeeeeee),
        textTheme: const TextTheme(
          bodyMedium: TextStyle(
            fontSize: 16,
          ),
        ),
      ),
      home: Scaffold(
        body: Padding(
          padding: const EdgeInsets.all(16),
          child: Center(
            child: Container(
              decoration: BoxDecoration(
                color: const Color(0xffffffff),
                border: Border.all(),
                boxShadow: const [
                  BoxShadow(
                    blurRadius: 10,
                    color: Color(0x80000000),
                  ),
                ],
              ),
              padding: const EdgeInsets.all(8),
              child: MyWidget(),
            ),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  // Check MyWidget starts with one Column

  final myWidgetElement = controller.element(find.byType(MyWidget));

  final myWidgetChildElements = <Element>[];
  myWidgetElement.visitChildElements((e) => myWidgetChildElements.add(e));

  if (myWidgetChildElements.length != 1 ||
      myWidgetChildElements[0].widget is! Column) {
    _result(false, ['The root widget in MyWidget\'s build method should be a Column.']);
    return;
  }

  // Check Column has correct properties

  final innerColumnElement = myWidgetChildElements[0];
  final innerColumnWidget = innerColumnElement.widget as Column;

  if (innerColumnWidget.crossAxisAlignment != CrossAxisAlignment.start) {
    _result(false, ['The Column that contains the name and title should use CrossAxisAlignment.start as its CrossAxisAlignment value.']);
    return;
  }

  if (innerColumnWidget.mainAxisSize != MainAxisSize.min) {
    _result(false, ['The Column that contains the name and title should use MainAxisSize.min as its MainAxisSize value.']);
    return;
  }

  // Check inner Column has two Text children

  if (innerColumnWidget.children.any((w) => w is! Text)) {
    _result(false, ['The Column that contains the name and title should have two children, both Text widgets.']);
    return;
  }

  // Check first Text has headline style

  final nameText = innerColumnWidget.children[0] as Text;

  if (nameText.style?.fontSize != 24) {
    _result(false, ['The Text widget for the name should use the "headlineSmall" textStyle.']);
    return;
  }

  _result(true);
}
{$ end test.dart $}
```

#### Exercise: Wrap the Column in a Row
{:.no_toc}

#### 練習：在 Column 外包裹一個 Row
{:.no_toc}

{{site.alert.secondary}}

  Wrap the `Column` you implemented in a
  `Row` that contains the following widgets:

  你將在下面的 widgets 中實現 `Row` 包裹一個 `Column`：ßß

<ul markdown="1">
  <li markdown="1">
  An `Icon` widget set to `Icons.account_circle`
  and with a size of 50 pixels.

  將一個 `Icon` widget 設為 `Icons.account_circle`，
  並設為 50 畫素大小。
  </li>
  <li markdown="1">
  A `Padding` widget that creates a space of 8
  pixels around the `Icon` widget.

  在 `Icon` widget外包裹一個 `Padding` widget 以在其周圍建立 8 畫素的空間。

  To do this, you can specify `const EdgeInsets.all(8.0)`
  for the `padding` property.

  為了完成這個，你可以指定其 `padding` 屬性為 `const EdgeInsets.all(8.0)`。

  The `Row` should look like this:

  這個 `Row` 看上去會像這樣：
  </li>
</ul>

  ```dart
     Row(
       children: [
        Padding(
          padding: const EdgeInsets.all(8),
          child: Icon(Icons.account_circle, size: 50),
        ),
        Column( ... ), // <--- The Column you first implemented
      ],
     );
  ```
{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          'Flutter McFlutter',
          style: Theme.of(context).textTheme.headlineSmall,
        ),
        const Text('Experienced App Developer'),
      ],
    );
  }
}
{$ end main.dart $}
{$ begin solution.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        const Padding(
          padding: EdgeInsets.all(8),
          child: Icon(Icons.account_circle, size: 50),
        ),
        Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Flutter McFlutter',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const Text('Experienced App Developer'),
          ],
        ),
      ],
    );
  }
}
{$ end solution.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        scaffoldBackgroundColor: const Color(0xffeeeeee),
        textTheme: const TextTheme(
          bodyMedium: TextStyle(
            fontSize: 16,
          ),
        ),
      ),
      home: Scaffold(
        body: Padding(
          padding: const EdgeInsets.all(16),
          child: Center(
            child: Container(
              decoration: BoxDecoration(
                color: const Color(0xffffffff),
                border: Border.all(),
                boxShadow: const [
                  BoxShadow(
                    blurRadius: 10,
                    color: Color(0x80000000),
                  ),
                ],
              ),
              padding: const EdgeInsets.all(8.0),
              child: MyWidget(),
            ),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  // Check MyWidget starts with one Column

  final myWidgetElement = controller.element(find.byType(MyWidget));

  final myWidgetChildElements = <Element>[];
  myWidgetElement.visitChildElements((e) => myWidgetChildElements.add(e));

  if (myWidgetChildElements.length != 1 ||
      myWidgetChildElements[0].widget is! Row) {
    _result(false, ['The root widget in MyWidget\'s build method should be a Column.']);
    return;
  }

  // Check first Row has two children: Padding and Column

  final firstRowElement = myWidgetChildElements[0];

  final firstRowChildElements = <Element>[];
  firstRowElement.visitChildElements((e) => firstRowChildElements.add(e));

  if (firstRowChildElements.length != 2 ||
      firstRowChildElements[0].widget is! Padding ||
      firstRowChildElements[1].widget is! Column) {
    _result(false, ['The first Row should have two children: first a Padding, and then a Column.']);
    return;
  }

  // Check Padding has correct padding

  final paddingElement = firstRowChildElements[0];

  if ((paddingElement.widget as Padding).padding != const EdgeInsets.all(8)) {
    _result(false, ['The Padding widget in the first Row should have a padding of 8.']);
    return;
  }

  // Check Padding has an Icon as its child

  final paddingChildren = <Element>[];
  paddingElement.visitChildElements((e) => paddingChildren.add(e));

  if (paddingChildren.length != 1 || paddingChildren[0].widget is! Icon) {
    _result(false, ['The Padding widget in the first Row should have an Icon as its child.']);
    return;
  }

  // Check icon has a size of 50

  if ((paddingChildren[0].widget as Icon).size != 50) {
    _result(false, ['The Icon in the top-left corner should have a size of 50.']);
    return;
  }

  // Check inner Column has correct properties

  final innerColumnElement = firstRowChildElements[1];
  final innerColumnWidget = innerColumnElement.widget as Column;

  if (innerColumnWidget.crossAxisAlignment != CrossAxisAlignment.start) {
    _result(false, ['The Column for the name and title should use CrossAxisAlignment.start as its crosAxisAlignment.']);
    return;
  }

  if (innerColumnWidget.mainAxisSize != MainAxisSize.min) {
    _result(false, ['The Column for the name and title should use MainAxisSize.min as its mainAxisSize.']);
    return;
  }

  // Check inner Column has two Text children

  if (innerColumnWidget.children.any((w) => w is! Text)) {
    _result(false, ['The Column for the name and title should have two children, both Text widgets.']);
    return;
  }

  // Check first Text has headline style

  final nameText = innerColumnWidget.children[0] as Text;

  if (nameText.style?.fontSize != 24) {
    _result(false, ['The Text widget for the name should use the "headlineSmall" textStyle.']);
    return;
  }

  _result(true);
}
{$ end test.dart $}
```

### Part 2
{:.no_toc}

### 第二部分
{:.no_toc}

#### Exercise: Tweak the layout
{:.no_toc}

#### 練習：調整佈局
{:.no_toc}

{{site.alert.secondary}}

  Wrap the `Row` in a `Column` that has a `mainAxisSize`
  property set to `MainAxisSize.min` and a
  `crossAxisAlignment` property set to `CrossAxisAlignment.stretch`.
  The `Column` contains the following widgets:

  將 `Column` 包裹在 `Row` 外面，並將其 `mainAxisSize` 屬性設為 `MainAxisSize.min`，
  `crossAxisAlignment` 屬性設為 `CrossAxisAlignment.stretch`。
  這個 `Column` 將包含以下 widgets：

  * A `SizedBox` widget with a height of 8.

    一個高度為 8 的 `SizedBox` widget。

  * An empty `Row` where you'll add the contact information in
    a later step.

    一個空的 `Row`，你等會將用它來新增聯絡方式。

  * A second `SizedBox` widget with a height of 16.

    第二個高度為 16 的 `SizedBox` widget。

  * A second empty `Row` where you'll add
    four icons (Part 3).

    第二個空的 `Row`，你將會新增四個圖示。 (第三部分)

  The `Column`'s list of widgets should be formatted as follows,
  so the contact information and icons are displayed below the
  name and title:
  
  `Column` widget 的列表格式應該如下一樣，
  聯絡資訊和圖示顯示在名稱和頭銜下方：

  ```dart

     ],
    ), // <--- Closing parenthesis for the Row
    SizedBox(),
    Row(), // First empty Row
    SizedBox(),
    Row(), // Second empty Row
   ],
  ); // <--- Closing parenthesis for the Column that wraps the Row

  ```

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        const Padding(
          padding: EdgeInsets.all(8.0),
          child: Icon(Icons.account_circle, size: 50),
        ),
        Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Flutter McFlutter',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const Text('Experienced App Developer'),
          ],
        ),
      ],
    );
  }
}
{$ end main.dart $}
{$ begin solution.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: [
        Row(
          children: [
            const Padding(
              padding: EdgeInsets.all(8.0),
              child: Icon(Icons.account_circle, size: 50),
            ),
            Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              mainAxisSize: MainAxisSize.min,
              children: [
                Text(
                  'Flutter McFlutter',
                  style: Theme.of(context).textTheme.headlineSmall,
                ),
                const Text('Experienced App Developer'),
              ],
            ),
          ],
        ),
        const SizedBox(height: 8),
        Row(),
        const SizedBox(height: 16),
        Row(),
      ],
    );
  }
}
{$ end solution.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        scaffoldBackgroundColor: const Color(0xffeeeeee),
        textTheme: const TextTheme(
          bodyMedium: TextStyle(
            fontSize: 16,
          ),
        ),
      ),
      home: Scaffold(
        body: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Center(
            child: Container(
              decoration: BoxDecoration(
                color: const Color(0xffffffff),
                border: Border.all(),
                boxShadow: const [
                  BoxShadow(
                    blurRadius: 10,
                    color: Color(0x80000000),
                  ),
                ],
              ),
              padding: const EdgeInsets.all(8.0),
              child: MyWidget(),
            ),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  // Check MyWidget starts with one Column

  final myWidgetElement = controller.element(find.byType(MyWidget));

  final myWidgetChildElements = <Element>[];
  myWidgetElement.visitChildElements((e) => myWidgetChildElements.add(e));

  if (myWidgetChildElements.length != 1 ||
      myWidgetChildElements[0].widget is! Column) {
    _result(false, ['The root widget in MyWidget\'s build method should be a Column.']);
    return;
  }

  // Check outermost Column has 5 correct children.

  final outerColumnElement = myWidgetChildElements[0];
  final outerColumnChildWidgets =
      (outerColumnElement.widget as Column).children;
  final outerColumnChildElements = <Element>[];
  outerColumnElement.visitChildElements((e) => outerColumnChildElements.add(e));

  if (outerColumnChildWidgets.length != 5 ||
      outerColumnChildWidgets[0] is! Row ||
      outerColumnChildWidgets[1] is! SizedBox ||
      outerColumnChildWidgets[2] is! Row ||
      outerColumnChildWidgets[3] is! SizedBox ||
      outerColumnChildWidgets[4] is! Row) {
    _result(false, ['The children of the outermost Column should be [Row, SizedBox, Row, SizedBox, Row] in that order.']);
    return;
  }

  // Check outermost Column's properties

  if ((outerColumnElement.widget as Column).mainAxisSize != MainAxisSize.min) {
    _result(false, ['The outermost Column should use MainAxisSize.min for its mainAxisSize.']);
    return;
  }

  if ((outerColumnElement.widget as Column).crossAxisAlignment !=
      CrossAxisAlignment.stretch) {
    _result(false, ['The outermost Column should use CrossAxisAlignment.stretch for its crossAxisAlignment.']);
    return;
  }

  // Check first Row has two children: Padding and Column

  final firstRowElement = outerColumnChildElements
      .firstWhere((e) => e.widget == outerColumnChildWidgets[0]);

  final firstRowChildElements = <Element>[];
  firstRowElement.visitChildElements((e) => firstRowChildElements.add(e));

  if (firstRowChildElements.length != 2 ||
      firstRowChildElements[0].widget is! Padding ||
      firstRowChildElements[1].widget is! Column) {
    _result(false, ['The first Row should have two children: first a Padding, and then a Column.']);
    return;
  }

  // Check Padding has correct padding

  final paddingElement = firstRowChildElements[0];

  if ((paddingElement.widget as Padding).padding != const EdgeInsets.all(8)) {
    _result(false, ['The Padding widget in the first Row should have a padding of 8.']);
    return;
  }

  // Check Padding has an Icon as its child

  final paddingChildren = <Element>[];
  paddingElement.visitChildElements((e) => paddingChildren.add(e));

  if (paddingChildren.length != 1 || paddingChildren[0].widget is! Icon) {
    _result(false, ['The Padding widget in the first Row should have an Icon as its child.']);
    return;
  }

  // Check icon has a size of 50

  if ((paddingChildren[0].widget as Icon).size != 50) {
    _result(false, ['The Icon in the top-left corner should have a size of 50.']);
    return;
  }

  // Check inner Column has correct properties

  final innerColumnElement = firstRowChildElements[1];
  final innerColumnWidget = innerColumnElement.widget as Column;

  if (innerColumnWidget.crossAxisAlignment != CrossAxisAlignment.start) {
    _result(false, ['The Column for the name and title should use CrossAxisAlignment.start as its crosAxisAlignment.']);
    return;
  }

  if (innerColumnWidget.mainAxisSize != MainAxisSize.min) {
    _result(false, ['The Column for the name and title should use MainAxisSize.min as its mainAxisSize.']);
    return;
  }

  // Check inner Column has two Text children

  if (innerColumnWidget.children.any((w) => w is! Text)) {
    _result(false, ['The Column for the name and title should have two children, both Text widgets.']);
    return;
  }

  // Check first Text has headline style

  final nameText = innerColumnWidget.children[0] as Text;

  if (nameText.style?.fontSize != 24) {
    _result(false, ['The Text widget for the name should use the "headlineSmall" textStyle.']);
    return;
  }

  // Check first SizedBox has correct properties

  final firstSizedBoxElement = outerColumnChildElements
      .firstWhere((e) => e.widget == outerColumnChildWidgets[1]);

  if ((firstSizedBoxElement.widget as SizedBox).height != 8) {
    _result(false, ['The SizedBox before the first empty Row should have a height of 8.']);
    return;
  }

  // Check second SizedBox has correct properties

  final secondSizedBoxElement = outerColumnChildElements
      .firstWhere((e) => e.widget == outerColumnChildWidgets[3]);

  if ((secondSizedBoxElement.widget as SizedBox).height != 16) {
    _result(false, ['The SizedBox between the first and second empty Rows should have a height of 16.']);
    return;
  }

  _result(true);
}
{$ end test.dart $}
```

#### Exercise: Enter contact information
{:.no_toc}

#### 練習：輸入聯絡資訊
{:.no_toc}

{{site.alert.secondary}}

  Enter two `Text` widgets inside the first empty `Row` :

  在第一個空的 `Row` 中，新增兩個 `Text` widget。

<ul markdown="1">
  <li markdown="1">
  The first `Text` widget contains the address `123 Main Street`.

  第一個 `Text` widget 包含了 `123 Main Street` 的地址。
  </li>
  <li markdown="1">
  The second `Text` widget contains the phone number `(415) 555-0198`.

  第二個 `Text` widget 包含了電話號 `(415) 555-0198`。
  </li>
</ul>

  For the first empty `Row`,
  set the `mainAxisAlignment` property to
  `MainAxisAlignment.spaceBetween`.

  對於第一個空的 `Row`，將 `mainAxisAlignment`
  屬性設為 `MainAxisAlignment.spaceBetween`。

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: [
        Row(
          children: [
            const Padding(
              padding: EdgeInsets.all(8.0),
              child: Icon(Icons.account_circle, size: 50),
            ),
            Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              mainAxisSize: MainAxisSize.min,
              children: [
                Text(
                  'Flutter McFlutter',
                  style: Theme.of(context).textTheme.headlineSmall,
                ),
                const Text('Experienced App Developer'),
              ],
            ),
          ],
        ),
        const SizedBox(height: 8),
        Row(
          children: const [],
        ),
        const SizedBox(height: 16),
        Row(
          children: const [],
        ),
      ],
    );
  }
}
{$ end main.dart $}
{$ begin solution.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: [
        Row(
          children: [
            const Padding(
              padding: EdgeInsets.all(8.0),
              child: Icon(Icons.account_circle, size: 50),
            ),
            Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              mainAxisSize: MainAxisSize.min,
              children: [
                Text(
                  'Flutter McFlutter',
                  style: Theme.of(context).textTheme.headlineSmall,
                ),
                const Text('Experienced App Developer'),
              ],
            ),
          ],
        ),
        const SizedBox(height: 8),
        Row(
          mainAxisAlignment: MainAxisAlignment.spaceBetween,
          children: const [
            Text(
              '123 Main Street',
            ),
            Text(
              '(415) 555-0198',
            ),
          ],
        ),
        const SizedBox(height: 16),
        Row(
          children: const [],
        ),
      ],
    );
  }
}
{$ end solution.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        scaffoldBackgroundColor: const Color(0xffeeeeee),
        textTheme: const TextTheme(
          bodyMedium: TextStyle(
            fontSize: 16,
          ),
        ),
      ),
      home: Scaffold(
        body: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Center(
            child: Container(
              decoration: BoxDecoration(
                color: const Color(0xffffffff),
                border: Border.all(),
                boxShadow: const [
                  BoxShadow(
                    blurRadius: 10,
                    color: Color(0x80000000),
                  ),
                ],
              ),
              padding: const EdgeInsets.all(8.0),
              child: MyWidget(),
            ),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  // Check MyWidget starts with one Column

  final myWidgetElement = controller.element(find.byType(MyWidget));

  final myWidgetChildElements = <Element>[];
  myWidgetElement.visitChildElements((e) => myWidgetChildElements.add(e));

  if (myWidgetChildElements.length != 1 ||
      myWidgetChildElements[0].widget is! Column) {
    _result(false, ['The root widget in MyWidget\'s build method should be a Column.']);
    return;
  }

  // Check outermost Column has 5 correct children.

  final outerColumnElement = myWidgetChildElements[0];
  final outerColumnChildWidgets =
      (outerColumnElement.widget as Column).children;
  final outerColumnChildElements = <Element>[];
  outerColumnElement.visitChildElements((e) => outerColumnChildElements.add(e));

  if (outerColumnChildWidgets.length != 5 ||
      outerColumnChildWidgets[0] is! Row ||
      outerColumnChildWidgets[1] is! SizedBox ||
      outerColumnChildWidgets[2] is! Row ||
      outerColumnChildWidgets[3] is! SizedBox ||
      outerColumnChildWidgets[4] is! Row) {
    _result(false, ['The children of the outermost Column should be [Row, SizedBox, Row, SizedBox, Row] in that order.']);
    return;
  }

  // Check outermost Column's properties

  if ((outerColumnElement.widget as Column).mainAxisSize != MainAxisSize.min) {
    _result(false, ['The outermost Column should use MainAxisSize.min for its mainAxisSize.']);
    return;
  }

  if ((outerColumnElement.widget as Column).crossAxisAlignment !=
      CrossAxisAlignment.stretch) {
    _result(false, ['The outermost Column should use CrossAxisAlignment.stretch for its crossAxisAlignment.']);
    return;
  }

  // Check first Row has two children: Padding and Column

  final firstRowElement = outerColumnChildElements
      .firstWhere((e) => e.widget == outerColumnChildWidgets[0]);

  final firstRowChildElements = <Element>[];
  firstRowElement.visitChildElements((e) => firstRowChildElements.add(e));

  if (firstRowChildElements.length != 2 ||
      firstRowChildElements[0].widget is! Padding ||
      firstRowChildElements[1].widget is! Column) {
    _result(false, ['The first Row should have two children: first a Padding, and then a Column.']);
    return;
  }

  // Check Padding has correct padding

  final paddingElement = firstRowChildElements[0];

  if ((paddingElement.widget as Padding).padding != const EdgeInsets.all(8)) {
    _result(false, ['The Padding widget in the first Row should have a padding of 8.']);
    return;
  }

  // Check Padding has an Icon as its child

  final paddingChildren = <Element>[];
  paddingElement.visitChildElements((e) => paddingChildren.add(e));

  if (paddingChildren.length != 1 || paddingChildren[0].widget is! Icon) {
    _result(false, ['The Padding widget in the first Row should have an Icon as its child.']);
    return;
  }

  // Check icon has a size of 50

  if ((paddingChildren[0].widget as Icon).size != 50) {
    _result(false, ['The Icon in the top-left corner should have a size of 50.']);
    return;
  }

  // Check inner Column has correct properties

  final innerColumnElement = firstRowChildElements[1];
  final innerColumnWidget = innerColumnElement.widget as Column;

  if (innerColumnWidget.crossAxisAlignment != CrossAxisAlignment.start) {
    _result(false, ['The Column for the name and title should use CrossAxisAlignment.start as its crosAxisAlignment.']);
    return;
  }

  if (innerColumnWidget.mainAxisSize != MainAxisSize.min) {
    _result(false, ['The Column for the name and title should use MainAxisSize.min as its mainAxisSize.']);
    return;
  }

  // Check inner Column has two Text children

  if (innerColumnWidget.children.any((w) => w is! Text)) {
    _result(false, ['The Column for the name and title should have two children, both Text widgets.']);
    return;
  }

  // Check first Text has headline style

  final nameText = innerColumnWidget.children[0] as Text;

  if (nameText.style?.fontSize != 24) {
    _result(false, ['The Text widget for the name should use the "headlineSmall" textStyle.']);
    return;
  }

  // Check first SizedBox has correct properties

  final firstSizedBoxElement = outerColumnChildElements
      .firstWhere((e) => e.widget == outerColumnChildWidgets[1]);

  if ((firstSizedBoxElement.widget as SizedBox).height != 8) {
    _result(false, ['The SizedBox before the first empty Row widget should have a height of 8.']);
    return;
  }

  // Check second Row has two Text children

  final secondRowElement = outerColumnChildElements
      .firstWhere((e) => e.widget == outerColumnChildWidgets[2]);

  final secondRowChildElements = <Element>[];
  secondRowElement.visitChildElements((e) => secondRowChildElements.add(e));

  if (secondRowChildElements.length != 2 ||
      secondRowChildElements.any((e) => e.widget is! Text)) {
    _result(false, ['The first empty Row widget should have two children, both Text widgets.']);
    return;
  }

  // Check second Row has correct properties

  if ((secondRowElement.widget as Row).mainAxisAlignment !=
      MainAxisAlignment.spaceBetween) {
    _result(false, ['The first empty Row widget should use MainAxisAlignment.spaceBetween as its MainAxisAlignment value.']);
    return;
  }

  // Check second SizedBox has correct properties

  final secondSizedBoxElement = outerColumnChildElements
      .firstWhere((e) => e.widget == outerColumnChildWidgets[3]);

  if ((secondSizedBoxElement.widget as SizedBox).height != 16) {
    _result(false, ['The SizedBox between the first and second empty Row widgets should have a height of 16.']);
    return;
  }

  _result(true);
}
{$ end test.dart $}
```

### Part 3
{:.no_toc}

### 第三部分
{:.no_toc}

#### Exercise: Add four icons
{:.no_toc}

#### 練習：新增四個圖示
{:.no_toc}

{{site.alert.secondary}}

  Enter the following `Icon` widgets inside the second empty `Row`:

  在第二個空的 `Row` 中新增以下四個 `Icon` widget。

  * `Icons.accessibility`
  * `Icons.timer`
  * `Icons.phone_android`
  * `Icons.phone_iphone`

  For the second empty `Row`,
  set the `mainAxisAlignment` property to
  `MainAxisAlignment.spaceAround`.

  對於第二個空的 `Row`，將其 `mainAxisAlignment` 
  屬性設為 `MainAxisAlignment.spaceAround`。

{{site.alert.end}}

```run-dartpad:theme-dark:mode-flutter:width-100%:height-400px:split-60
{$ begin main.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: [
        Row(
          children: [
            const Padding(
              padding: EdgeInsets.all(8.0),
              child: Icon(Icons.account_circle, size: 50),
            ),
            Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              mainAxisSize: MainAxisSize.min,
              children: [
                Text(
                  'Flutter McFlutter',
                  style: Theme.of(context).textTheme.headlineSmall,
                ),
                const Text('Experienced App Developer'),
              ],
            ),
          ],
        ),
        const SizedBox(height: 8),
        Row(
          mainAxisAlignment: MainAxisAlignment.spaceBetween,
          children: const [
            Text('123 Main Street'),
            Text('415-555-0198'),
          ],
        ),
        const SizedBox(height: 16),
        Row(
          children: const [],
        ),
      ],
    );
  }
}
{$ end main.dart $}
{$ begin solution.dart $}
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: [
        Row(
          children: [
            const Padding(
              padding: EdgeInsets.all(8.0),
              child: Icon(Icons.account_circle, size: 50),
            ),
            Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              mainAxisSize: MainAxisSize.min,
              children: [
                Text(
                  'Flutter McFlutter',
                  style: Theme.of(context).textTheme.headlineSmall,
                ),
                const Text('Experienced App Developer'),
              ],
            ),
          ],
        ),
        const SizedBox(height: 8),
        Row(
          mainAxisAlignment: MainAxisAlignment.spaceBetween,
          children: const [
            Text(
              '123 Main Street',
            ),
            Text(
              '(415) 555-0198',
            ),
          ],
        ),
        const SizedBox(height: 16),
        Row(
          mainAxisAlignment: MainAxisAlignment.spaceAround,
          children: const [
            Icon(Icons.accessibility),
            Icon(Icons.timer),
            Icon(Icons.phone_android),
            Icon(Icons.phone_iphone),
          ],
        ),
      ],
    );
  }
}
{$ end solution.dart $}
{$ begin test.dart $}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        scaffoldBackgroundColor: const Color(0xffeeeeee),
        textTheme: const TextTheme(
          bodyMedium: TextStyle(
            fontSize: 16,
          ),
        ),
      ),
      home: Scaffold(
        body: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Center(
            child: Container(
              decoration: BoxDecoration(
                color: const Color(0xffffffff),
                border: Border.all(),
                boxShadow: const [
                  BoxShadow(
                    blurRadius: 10,
                    color: Color(0x80000000),
                  ),
                ],
              ),
              padding: const EdgeInsets.all(8.0),
              child: MyWidget(),
            ),
          ),
        ),
      ),
    );
  }
}

Future<void> main() async {
  final completer = Completer<void>();
  
  runApp(MyApp());

  WidgetsFlutterBinding.ensureInitialized()
      .addPostFrameCallback((timestamp) async {
    completer.complete();
  });
  
  await completer.future;

  final controller = LiveWidgetController(WidgetsBinding.instance);

  // Check MyWidget starts with one Column

  final myWidgetElement = controller.element(find.byType(MyWidget));

  final myWidgetChildElements = <Element>[];
  myWidgetElement.visitChildElements((e) => myWidgetChildElements.add(e));

  if (myWidgetChildElements.length != 1 ||
      myWidgetChildElements[0].widget is! Column) {
    _result(false, ['The root widget in MyWidget\'s build method should be a Column.']);
    return;
  }

  // Check outermost Column has 5 correct children.

  final outerColumnElement = myWidgetChildElements[0];
  final outerColumnChildWidgets =
      (outerColumnElement.widget as Column).children;
  final outerColumnChildElements = <Element>[];
  outerColumnElement.visitChildElements((e) => outerColumnChildElements.add(e));

  if (outerColumnChildWidgets.length != 5 ||
      outerColumnChildWidgets[0] is! Row ||
      outerColumnChildWidgets[1] is! SizedBox ||
      outerColumnChildWidgets[2] is! Row ||
      outerColumnChildWidgets[3] is! SizedBox ||
      outerColumnChildWidgets[4] is! Row) {
    _result(false, ['The children of the outermost Column should be [Row, SizedBox, Row, SizedBox, Row] in that order.']);
    return;
  }

  // Check outermost Column's properties

  if ((outerColumnElement.widget as Column).mainAxisSize != MainAxisSize.min) {
    _result(false, ['The outermost Column should use MainAxisSize.min for its mainAxisSize.']);
    return;
  }

  if ((outerColumnElement.widget as Column).crossAxisAlignment !=
      CrossAxisAlignment.stretch) {
    _result(false, ['The outermost Column should use CrossAxisAlignment.stretch for its crossAxisAlignment.']);
    return;
  }

  // Check first Row has two children: Padding and Column

  final firstRowElement = outerColumnChildElements
      .firstWhere((e) => e.widget == outerColumnChildWidgets[0]);

  final firstRowChildElements = <Element>[];
  firstRowElement.visitChildElements((e) => firstRowChildElements.add(e));

  if (firstRowChildElements.length != 2 ||
      firstRowChildElements[0].widget is! Padding ||
      firstRowChildElements[1].widget is! Column) {
    _result(false, ['The first Row should have two children: first a Padding, and then a Column.']);
    return;
  }

  // Check Padding has correct padding

  final paddingElement = firstRowChildElements[0];

  if ((paddingElement.widget as Padding).padding != const EdgeInsets.all(8)) {
    _result(false, ['The Padding widget in the first Row should have a padding of 8.']);
    return;
  }

  // Check Padding has an Icon as its child

  final paddingChildren = <Element>[];
  paddingElement.visitChildElements((e) => paddingChildren.add(e));

  if (paddingChildren.length != 1 || paddingChildren[0].widget is! Icon) {
    _result(false, ['The Padding widget in the first Row should have an Icon as its child.']);
    return;
  }

  // Check icon has a size of 50

  if ((paddingChildren[0].widget as Icon).size != 50) {
    _result(false, ['The Icon in the top-left corner should have a size of 50.']);
    return;
  }

  // Check inner Column has correct properties

  final innerColumnElement = firstRowChildElements[1];
  final innerColumnWidget = innerColumnElement.widget as Column;

  if (innerColumnWidget.crossAxisAlignment != CrossAxisAlignment.start) {
    _result(false, ['The Column for the name and title should use CrossAxisAlignment.start as its crosAxisAlignment.']);
    return;
  }

  // Check inner Column has two Text children

  if (innerColumnWidget.children.any((w) => w is! Text)) {
    _result(false, ['The Column for the name and title should have two children, both Text widgets.']);
    return;
  }

  if (innerColumnWidget.mainAxisSize != MainAxisSize.min) {
    _result(false, ['The Column for the name and title should use MainAxisSize.min as its mainAxisSize.']);
    return;
  }

  // Check first Text has headline style

  final nameText = innerColumnWidget.children[0] as Text;

  if (nameText.style?.fontSize != 24) {
    _result(false, ['The Text widget for the name should use the "headlineSmall" textStyle.']);
    return;
  }

  // Check first SizedBox has correct properties

  final firstSizedBoxElement = outerColumnChildElements
      .firstWhere((e) => e.widget == outerColumnChildWidgets[1]);

  if ((firstSizedBoxElement.widget as SizedBox).height != 8) {
    _result(false, ['The SizedBox before the first empty Row widget should have a height of 8.']);
    return;
  }

  // Check second Row has two Text children

  final secondRowElement = outerColumnChildElements
      .firstWhere((e) => e.widget == outerColumnChildWidgets[2]);

  final secondRowChildElements = <Element>[];
  secondRowElement.visitChildElements((e) => secondRowChildElements.add(e));

  if (secondRowChildElements.length != 2 ||
      secondRowChildElements.any((e) => e.widget is! Text)) {
    _result(false, ['The first Row widget should have two children, both Text widgets.']);
    return;
  }

  // Check second Row has correct properties

  if ((secondRowElement.widget as Row).mainAxisAlignment !=
      MainAxisAlignment.spaceBetween) {
    _result(false, ['The first Row widget should use MainAxisAlignment.spaceBetween as its mainAxisAlignment.']);
    return;
  }

  // Check second SizedBox has correct properties

  final secondSizedBoxElement = outerColumnChildElements
      .firstWhere((e) => e.widget == outerColumnChildWidgets[3]);

  if ((secondSizedBoxElement.widget as SizedBox).height != 16) {
    _result(false, ['The SizedBox between the first and second Row widgets should have a height of 16.']);
    return;
  }

  // Check second empty Row has four Icon children

  final thirdRowElement = outerColumnChildElements
      .firstWhere((e) => e.widget == outerColumnChildWidgets[4]);

  final thirdRowChildElements = <Element>[];
  thirdRowElement.visitChildElements((e) => thirdRowChildElements.add(e));

  if (thirdRowChildElements.length != 4 ||
      thirdRowChildElements.any((e) => e.widget is! Icon)) {
    _result(false, ['The second empty Row widget should have four children, all Icon widgets.']);
    return;
  }

  // Check second empty Row has correct properties

  if ((thirdRowElement.widget as Row).mainAxisAlignment !=
      MainAxisAlignment.spaceAround) {
    _result(false, ['The second empty Row widget should use MainAxisAlignment.spaceAround as its MainAxisAligment value.']);
    return;
  }

  _result(true);
}
{$ end test.dart $}
```

## What's next?

## 下一步是什麼？

Congratulations, you've finished this codelab!
If you'd like to know more about Flutter,
here are a few suggestions for resources worth exploring:

恭喜你，已經完成了這個 codelab！
如果你想要了解關於 Flutter 的更多資訊，這裡有些值得探索的資源要推薦給你：

* Learn more about layouts in Flutter by
  visiting the [Building layouts][] page.

  在 [Building layouts][] 頁面中學習關於 Flutter 的佈局。

* Check out the [sample apps][].

  檢視 [sample apps][]。

* Visit [Flutter's YouTube channel][],
  where you can watch a variety videos from
  videos that focus on individual widgets
  to videos of developers building apps.

  存取 [Flutter's YouTube channel][]，你將能夠觀看大量
  專注於獨立的 widget 以及開發者如何建構應用的影片。

You can download Flutter from the [install][] page.

你可以在 [安裝][install] 頁面中下載 Flutter。




[Building layouts]: {{site.url}}/ui/layout
[Cupertino]: {{site.api}}/flutter/cupertino/CupertinoApp-class.html
[DartPad issue]: {{site.github}}/dart-lang/dart-pad/issues/new
[Flutter's YouTube channel]: {{site.social.youtube}}
[GitHub]: {{site.repo.this}}/tree/{{site.branch}}/examples/layout/sizing/images
[install]: {{site.url}}/get-started/install
[Material]: {{site.api}}/flutter/material/MaterialApp-class.html
[Material Color palette]: {{site.api}}/flutter/material/Colors-class.html
[Material Icon library]: {{site.api}}/flutter/material/Icons-class.html
[sample apps]: {{site.github}}/flutter/samples
