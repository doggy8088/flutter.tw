---
title: "Implicit animations"
title: 隱含動畫
description: "A codelab that uses interactive examples and exercises to teach  how to use Flutter's implicitly animated widgets."
description: "透過互動式範例和練習學習如何在 Flutter 中使用隱含動畫的 widgets。"
tags: 課程, 程式碼實驗室
keywords: 隱含動畫,UI,使用者介面
toc: true
diff2html: true
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
  - defer: true
    url: /assets/js/codelabs/animations_examples.js
---

<?code-excerpt path-base="animation/implicit"?>

Welcome to the implicit animations codelab, where you learn how to use Flutter
widgets that make it easy to create animations for a specific set of properties.

歡迎來到隱含動畫的 codelab，
在這裡您將學到：
如何使用 Flutter widgets 輕鬆地對一組特定屬性建立動畫。

To get the most out of this codelab, you should have basic knowledge about:

為了充分理解該 codelab，您應該具備以下基本知識：

* How to [make a Flutter app].

  如何 [建立一個 Flutter 應用][make a Flutter app]。
  
* How to use [stateful widgets].

  如何使用 [stateful widgets][]。

This codelab covers the following material:

該 codelab 包括以下內容:

* Using `AnimatedOpacity` to create a fade-in effect.

  使用 `AnimatedOpacity` 來建立一個淡入效果。

* Using `AnimatedContainer` to animate transitions in size, color, and margin.

  使用 `AnimatedContainer` 讓尺寸、顏色和邊距產生動畫變換。

* Overview of implicit animations and techniques for using them.

  隱含動畫及其使用方法的概述。

**Estimated time to complete this codelab: 15-30 minutes.**

**完成該 codelab 的時間約為：15-30 分鐘。**

## What are implicit animations?

## 什麼是隱含動畫?

With Flutter's [animation library],
you can add motion and create visual effects
for the widgets in your UI.

透過使用 Flutter 的 [動畫函式庫][animation library]，
您可以為 UI 中的元件新增運動和建立視覺效果。

One widget set in the library manages animations for you.
These widgets are collectively referred to as _implicit animations_,
or _implicitly animated widgets_, deriving their name from the
[ImplicitlyAnimatedWidget] class that they implement.

您可以使用庫中的一套元件來管理動畫，
這些元件統稱為**隱含動畫**或**隱含動畫元件**，
其名稱源於它們都實現了 [ImplicitlyAnimatedWidget][] 類別。

With implicit animations,
you can animate a widget property by setting a target value;
whenever that target value changes,
the widget animates the property from the old value to the new one.
In this way, implicit animations trade control for convenience&mdash;they
manage animation effects so that you don't have to.

使用隱含動畫，您可以透過設定一個目標值，驅動 widget 的屬性進行動畫變換；
每當目標值發生變化時，屬性會從舊值逐漸更新到新值。
透過這種方式，隱含動畫內部實現了動畫控制，從而能夠方便地使用&mdash;
隱含動畫元件會管理動畫效果，使用者不需要再進行額外的處理。

## Example: Fade-in text effect

## 範例：淡入文字效果

The following example shows how to add a fade-in effect to existing UI
using an implicitly animated widget called [AnimatedOpacity].

下面的範例展示瞭如何使用名為 [AnimatedOpacity][] 的隱含動畫 widget，
為已存在的 UI 新增淡入效果。

**The example begins with no animation code**&mdash;it
consists of a [Material App] home screen containing:

**這個範例開始沒有動畫效果**&mdash;
它包含一個由 [Material App][] 組成的主頁面，
有以下內容：

* A photograph of an owl.

  一張貓頭鷹的照片。

* One **Show details** button that does nothing when clicked.

  一個點選時什麼也不做的 **Show details** 按鈕。

* Description text of the owl in the photograph.

  照片中貓頭鷹的描述文字。

### Fade-in (starter code)

### 淡入 (初始程式碼)

Click the **Run** button to run the example:

點選 **Run** 按鈕來執行這個範例：

{% include docs/implicit-animations/fade-in-starter-code.md %}

{{site.alert.important}}

  This page uses an embedded version of [DartPad] to display
  examples and exercises.
  If you see empty boxes instead of DartPads, go to the
  [DartPad troubleshooting page].

  本頁面使用一個嵌入式版本的 [DartPad][] 來顯示範例和進行練習。
  如果您看到的是空白內容，而不是 DartPad，
  請前往 [DartPad 故障排除頁][DartPad troubleshooting page]。

{{site.alert.end}}

### Animate opacity with AnimatedOpacity widget

### 使用 AnimatedOpacity widget 進行透明度動畫

  This section contains a list of steps you can use to add an
  implicit animation to the
  [fade-in starter code]. After the steps, you can also run the
  [fade-in complete] code with the changes already made.
  The steps outline how to use the `AnimatedOpacity`
  widget to add the following animation feature:

  這部分包含在 [淡入初始程式碼][fade-in starter code] 中新增一個隱含動畫一系列步驟。
  完成這些步驟後，您還可以執行 [淡入完成程式碼][fade-in complete]，
  該程式碼已經實現了淡入效果。
  這些步驟概述瞭如何使用 `AnimatedOpacity` widget 來新增以下的動畫特性：

  * The owl's description text remains hidden until the user clicks the
    **Show details** button.
    
    使用者點選 **Show details** 按鈕後，顯示貓頭鷹的描述文字。
    
  * When the user clicks the **Show details** button,
    the owl's description text fades in.

    當用戶點選 **Show details** 按鈕時，貓頭鷹的描述文字淡入。

#### 1. Pick a widget property to animate

#### 1. 選擇要進行動畫的 widget 屬性

To create a fade-in effect, you can animate the `opacity` property using the
`AnimatedOpacity` widget. Change the `Container` widget to an
`AnimatedOpacity` widget:

想要建立淡入效果，您可以使用 `AnimatedOpacity` widget 對 `opacity` 屬性進行動畫。
將 `Container` widget 換成 `AnimatedOpacity` widget：

<?code-excerpt "opacity{1,2}/lib/main.dart"?>
```diff
--- opacity1/lib/main.dart
+++ opacity2/lib/main.dart
@@ -2,6 +2,8 @@
 // Use of this source code is governed by a BSD-style license
 // that can be found in the LICENSE file.

+// ignore_for_file: missing_required_argument
+
 import 'package:flutter/material.dart';

 const owlUrl =
@@ -25,12 +27,14 @@
             style: TextStyle(color: Colors.blueAccent),
           ),
           onPressed: () => {}),
-      Column(
-        children: const [
-          Text('Type: Owl'),
-          Text('Age: 39'),
-          Text('Employment: None'),
-        ],
+      AnimatedOpacity(
+        child: Column(
+          children: const [
+            Text('Type: Owl'),
+            Text('Age: 39'),
+            Text('Employment: None'),
+          ],
+        ),
       )
     ]);
   }
```

{{site.alert.info}}

  You can reference the line numbers in the example code to help track
  where to make these changes.

  您可以根據範例程式碼中的行號檢視修改的位置。

{{site.alert.end}}

#### 2. Initialize a state variable for the animated property

#### 2. 為動畫屬性初始化一個狀態變數

To hide the text before the user clicks **Show details**, set
the starting value for `opacity` to zero:

將 `opacity` 的初始值設定為 0 ，以便在使用者點選 **Show details** 前隱藏文字：

<?code-excerpt "opacity{2,3}/lib/main.dart"?>
```diff
--- opacity2/lib/main.dart
+++ opacity3/lib/main.dart
@@ -2,8 +2,6 @@
 // Use of this source code is governed by a BSD-style license
 // that can be found in the LICENSE file.

-// ignore_for_file: missing_required_argument
-
 import 'package:flutter/material.dart';

 const owlUrl =
@@ -17,6 +15,8 @@
 }

 class _FadeInDemoState extends State<FadeInDemo> {
+  double opacity = 0.0;
+
   @override
   Widget build(BuildContext context) {
     return Column(children: <Widget>[
@@ -28,6 +28,8 @@
           ),
           onPressed: () => {}),
       AnimatedOpacity(
+        duration: const Duration(seconds: 3),
+        opacity: opacity,
         child: Column(
           children: const [
             Text('Type: Owl'),
```

#### 3. Set the duration of the animation

#### 3. 為動畫設定一個時長

In addition to an `opacity` parameter, `AnimatedOpacity` requires a
[duration] to use for its animation. For this example,
you can start with 2 seconds:

除了 `opacity` 引數以外，`AnimatedOpacity` 還需要為動畫設定 [duration][]。
在下面的例子中，動畫會以兩秒的時長執行：

<?code-excerpt "opacity{3,4}/lib/main.dart"?>
```diff
--- opacity3/lib/main.dart
+++ opacity4/lib/main.dart
@@ -28,7 +28,7 @@
           ),
           onPressed: () => {}),
       AnimatedOpacity(
-        duration: const Duration(seconds: 3),
+        duration: const Duration(seconds: 2),
         opacity: opacity,
         child: Column(
           children: const [
```

#### 4. Set up a trigger for the animation, and choose an end value

#### 4. 為動畫設定一個觸發器，並選擇一個結束值

Configure the animation to trigger when the user clicks the **Show details**
button. To do this, change `opacity` state using the `onPressed()` handler for
`TextlButton`. To make the `FadeInDemo` widget become fully visible when
the user clicks the **Show details** button, use the `onPressed()` handler
to set `opacity` to 1:

當用戶點選 **Show details** 按鈕時，將會觸發動畫。
為了做到這點，我們使用 `TextButton` 的 `onPressed()` 方法，
在呼叫時改變 `opacity` 的狀態值為 1。

<?code-excerpt "opacity{4,5}/lib/main.dart"?>
```diff
--- opacity4/lib/main.dart
+++ opacity5/lib/main.dart
@@ -22,11 +22,14 @@
     return Column(children: <Widget>[
       Image.network(owlUrl),
       TextButton(
-          child: const Text(
-            'Show Details',
-            style: TextStyle(color: Colors.blueAccent),
-          ),
-          onPressed: () => {}),
+        child: const Text(
+          'Show Details',
+          style: TextStyle(color: Colors.blueAccent),
+        ),
+        onPressed: () => setState(() {
+          opacity = 1;
+        }),
+      ),
       AnimatedOpacity(
         duration: const Duration(seconds: 2),
         opacity: opacity,
```

{{site.alert.secondary}}

  Notice that you only need to set the start and end values of `opacity`.
  The `AnimatedOpacity` widget manages everything in between.

  注意：您只需要設定 `opacity` 的開始值和結束值。
  `AnimatedOpacity` widget 會自行處理動畫過程中的一切。

{{site.alert.end}}

### Fade-in (complete)

### 淡入 (完成程式碼)

Here's the example with the completed changes you've made&mdash;run this
example and click the **Show details** button to trigger the animation.

下面的範例是修改後的完成版程式碼&mdash;
執行這個範例，然後點選 **Show details** 按鈕就可以觸發動畫。

{% include docs/implicit-animations/fade-in-complete.md %}

### Putting it all together

### 小結一下

The [Fade-in text effect] example demonstrates the following features
of `AnimatedOpacity`:

[淡入文字效果][Fade-in text effect] 的例子展現了 `AnimatedOpacity` 的特性:

* `AnimatedOpacity` listens for state changes in its `opacity` property.

  `AnimatedOpacity` 會監聽其 `opacity` 屬性的狀態變化。

* Whenever `opacity` changes, `AnimatedOpacity` automatically animates the
  widget's transition to the new value for `opacity`.
  
  當 `opacity` 屬性改變時，
  `AnimatedOpacity` 會自動將 `opacity` 變化到新值，
  同時使 widget 進行動畫跟隨變換。

* `AnimatedOpacity` requires a `duration` parameter to define the time it takes
  to animate the transition between an old `opacity` value and a new one.

  `AnimatedOpacity` 需要一個 `duration` 引數來確定新舊 `opacity` 進行動畫變換的時長。

{{site.alert.secondary}}

  Note that Implicit animations can only animate properties of a parent
  `StatefulWidget`, so this example begins with the `FadeInDemo` widget that
  extends `StatefulWidget`.

  注意：隱含動畫只能在父節點是 `StatefulWidget` 時以動畫變換屬性。
  因此，本範例是從繼承 `StatefulWidget` 的 `FadeInDemo` widget 開始的。

  Notice also that `AnimatedOpacity` animates a single property: `opacity`.
  Some implicitly animated widgets can animate many properties, as the following
  example illustrates.

  還請注意：`AnimatedOpacity` 只能將一個 `opacity` 屬性進行動畫。
  一些隱含動畫 widget 可以同時動畫變換多個屬性，如下面的範例所示。

{{site.alert.end}}

## Example: Shape-shifting effect

## 範例：形狀變化效果

The following example shows how to use the [AnimatedContainer] widget to
animate multiple properties (`margin`, `borderRadius`, and `color`) with
different types (`double` and `Color`).

下面的範例將展示如何使用 [AnimatedContainer][] widget
讓多個不同型別（`double` 和 `Color`）的屬性
（`margin`、`borderRadius` 和 `color`）同時進行動畫變換。

**The example begins with no animation code**&mdash;it starts with a
[Material App] home screen that contains:

**這個範例開始沒有動畫效果**&mdash;
它以一個由 [Material App][] 組成的主頁面開始，
有以下內容：

* A `Container` with `borderRadius`, `margin`, and `color` properties that are
  different each time you run the example.
  
  一個有 `margin`、`borderRadius`、和 `color` 屬性的 `Container`，
  這些屬性每次執行時的值都不同。
  
* A **Change** button that does nothing when clicked.

  一個點選時什麼都不做的 **Change** 按鈕。

### Shape-shifting (starter code)

### 形狀變化 (初始程式碼)

Click the **Run** button to run the example:

點選 **Run** 按鈕來執行這個範例：

{% include docs/implicit-animations/shape-shifting-starter-code.md %}

### Animate color, borderRadius, and margin with AnimatedContainer

### 使用 AnimatedContainer 將 color、borderRadius、和 margin 進行動畫變換

  This section contains a list of steps you can use to add an
  implicit animation to the [shape-shifting starter code].
  After the steps, you can also run the
  [shape-shifting complete] example with the changes already made.

  這部分包含在 [形狀變化初始程式碼][shape-shifting starter code] 中
  新增一個隱含動畫的一系列步驟。
  完成這些步驟後，您還可以執行 [形狀變化完成程式碼][shape-shifting complete]，
  該程式碼已經實現了淡入效果。

In the [shape-shifting starter code],
each property in the `Container` widget (`color`,
`borderRadius`, and `margin`)
is assigned a value by an associated function (`randomColor()`,
`randomBorderRadius()`, and `randomMargin()` respectively).
By using an `AnimatedContainer` widget,
you can refactor this code to do the following:

在 [形狀變化初始程式碼][shape-shifting starter code] 中
每個 `Container` widget 的屬性（`color`、`borderRadius` 和 `margin`）
都由一個相關的函式賦值
（ 分別是 `randomColor()`、`randomBorderRadius()` 和 `randomMargin()`）。 
您可以使用 `AnimatedContainer` widget 重構這段程式碼，
來完成以下的效果:

* Generate new values for `color`, `borderRadius`,
  and `margin` whenever the user clicks the **Change** button.
  
  每當使用者點選 **Change** 按鈕時，
  `color`、`borderRadius` 和 `margin` 都會產生一個新值。
  
* Animate the transition to the new values for `color`,
  `borderRadius`, and `margin` whenever they are set.
  
  每當 `color`、`borderRadius` 和 `margin` 被設定時，
  都會進行動畫變換到新的值。
  
#### 1. Add an implicit animation

#### 1. 新增一個隱含動畫

Change the `Container` widget to an `AnimatedContainer` widget:

將 `Container` widget 換成 `AnimatedContainer` widget：

<?code-excerpt "container{1,2}/lib/main.dart"?>
```diff
--- container1/lib/main.dart
+++ container2/lib/main.dart
@@ -2,6 +2,8 @@
 // Use of this source code is governed by a BSD-style license
 // that can be found in the LICENSE file.

+// ignore_for_file: missing_required_argument
+
 import 'dart:math';

 import 'package:flutter/material.dart';
@@ -47,7 +49,7 @@
             SizedBox(
               width: 128,
               height: 128,
-              child: Container(
+              child: AnimatedContainer(
                 margin: EdgeInsets.all(margin),
                 decoration: BoxDecoration(
                   color: color,
```
{{site.alert.info}}

  You can reference the line numbers in the example code to help track where to
  make these changes in [shape-shifting starter code]

  您可以根據範例程式碼中的行號，
  檢視 [形狀變化初始程式碼][shape-shifting starter code] 裡修改的位置。

{{site.alert.end}}

#### 2. Set starting values for animated properties

#### 2. 為動畫屬性設定初始值

`AnimatedContainer` automatically animates between old and new values of
its properties when they change. Create a `change()` method that defines the
behavior triggered when the user clicks the **Change** button.
The `change()` method can use `setState()` to set new values
for the `color`, `borderRadius`, and `margin` state variables:

當屬性的新舊值發生變化時，`AnimatedContainer` 會自動在新舊值之間產生動畫效果。
透過建立一個 `change()` 方法，我們將定義當用戶點選 **Change** 按鈕時觸發變更的行為。
`change()` 方法可以使用 `setState()` 
為 `color`、`borderRadius` 和 `margin` 狀態變數設定新值：

<?code-excerpt "container{2,3}/lib/main.dart"?>
```diff
--- container2/lib/main.dart
+++ container3/lib/main.dart
@@ -40,6 +40,14 @@
     margin = randomMargin();
   }

+  void change() {
+    setState(() {
+      color = randomColor();
+      borderRadius = randomBorderRadius();
+      margin = randomMargin();
+    });
+  }
+
   @override
   Widget build(BuildContext context) {
     return Scaffold(
```

#### 3. Set up a trigger for the animation

#### 3. 為動畫設定觸發器

To set the animation to trigger whenever the user presses the **Change** button,
invoke the `change()` method in the `onPressed()` handler:

每當使用者點選 **Change** 按鈕時觸發動畫，
呼叫 `onPressed()` 處理器的 `change()` 方法：

<?code-excerpt "container{3,4}/lib/main.dart"?>
```diff
--- container3/lib/main.dart
+++ container4/lib/main.dart
@@ -67,7 +67,7 @@
             ),
             ElevatedButton(
               child: const Text('change'),
-              onPressed: () => {},
+              onPressed: () => change(),
             ),
           ],
         ),
```

#### 4. Set duration

#### 4. 設定時長

Finally, set the `duration` of the animation that powers the transition
between the old and new values:

在最後，設定新舊值之間變換的時長引數 `duration`：

<?code-excerpt "container{4,5}/lib/main.dart"?>
```diff
--- container4/lib/main.dart
+++ container5/lib/main.dart
@@ -2,12 +2,12 @@
 // Use of this source code is governed by a BSD-style license
 // that can be found in the LICENSE file.

-// ignore_for_file: missing_required_argument
-
 import 'dart:math';

 import 'package:flutter/material.dart';

+const _duration = Duration(milliseconds: 400);
+
 double randomBorderRadius() {
   return Random().nextDouble() * 64;
 }
@@ -63,6 +63,7 @@
                   color: color,
                   borderRadius: BorderRadius.circular(borderRadius),
                 ),
+                duration: _duration,
               ),
             ),
             ElevatedButton(
```

### Shape-shifting (complete)

### 形狀變化 (完成程式碼)

Here’s the example with the completed changes you’ve made&mdash;run the code
and click the **Change** button to trigger the animation. Notice that each time
you click the **Change** button, the shape animates to its new values
for `margin`, `borderRadius`, and `color`.

下面的範例是修改後的完成版程式碼&mdash;
執行這個範例，然後點選 **Change** 按鈕就可以觸發動畫。
注意：每次您點選 **Change** 按鈕，
形狀的 `margin`、`borderRadius` 和 `color` 都會進行動畫變化到新的值。

{% include docs/implicit-animations/shape-shifting-complete.md %}

### Using animation curves

### 使用動畫曲線

The preceding examples show how implicit animations allow you to animate
changes in values for specific widget properties, and how the
`duration` parameter allows you to set the amount of time an
animation takes to complete. Implicit animations also allow you to
control changes to __the rate__ of an animation within the `duration`.
The parameter you use to define this change in rate is [curve].

前面的範例展示出，如何讓您透過隱含動畫對特定的 widget 屬性值進行動畫變化，
以及如何透過 `duration` 引數設定動畫完成所需的時間。
隱含動畫還允許您在 `duration` 時長內控制動畫的 **速率** 變化。
用來定義這種速率變化的引數是 [curve][]。

The preceding examples do not specify a `curve`,
so the implicit animations apply a [linear animation curve] by default.
Add a `curve` parameter to the [shape-shifting complete]
and watch how the animation changes when you pass the
[easeInOutBack] constant for `curve`:

前面的例子中沒有指定 `curve`，
所以隱含動畫預設使用 [線性動畫曲線][linear animation curve]。
在 [形狀變化完成程式碼][shape-shifting complete] 中新增一個 `curve` 引數，
然後當您將常量 [easeInOutBack][] 傳遞給 `curve` 時，
觀察動畫的變化：

<?code-excerpt "container{5,6}/lib/main.dart"?>
```diff
--- container5/lib/main.dart
+++ container6/lib/main.dart
@@ -64,6 +64,7 @@
                   borderRadius: BorderRadius.circular(borderRadius),
                 ),
                 duration: _duration,
+                curve: Curves.easeInOutBack,
               ),
             ),
             ElevatedButton(
```

Now that you have passed `easeInOutBack` as the value for `curve` to
`AnimatedContainer`, notice that the rates of change for `margin`,
`borderRadius`, and `color` follow the curve defined by the
`easeInOutBack` curve:

現在您已經將 `easeInOutBack` 作為 `curve` 的值傳遞給了 `AnimatedContainer`，
注意：`margin`、`borderRadius` 和 `color` 的變化速率
遵循 `easeInOutBack` 所定義的曲線:

<div id="animation_1_play_button_"></div>
<video id="animation_1" style="width:464px; height:192px;" loop="">
  <source src="{{site.flutter-assets}}/animation/curve_ease_in_out_back.mp4" type="video/mp4">
</video>

{{site.alert.secondary}}

  The `easeInOutBack` constant is only one of many that you can
  pass for the `curve` parameter. Explore the
  [list of curve constants] to discover more ways
  to use `curve` to modify the look and feel of your animations.

  常量 `easeInOutBack` 只是您可以傳遞給 `curve` 引數的眾多常量之一。
  探索一下 [曲線常量列表][list of curve constants]，
  您就可以發現更多使用 `curve` 來改變動畫感觀的方式。

{{site.alert.end}}

### Putting it all together

### 小結一下

The [shape-shifting complete] example animates transitions between values for
`margin`, `borderRadius`, and `color` properties.
Note that `AnimatedContainer` animates changes to any of its properties,
including those you didn't use such as `padding`, `transform`,
and even `child` and `alignment`!
The [shape-shifting complete] example builds upon [fade-in complete] by showing
additional capabilities of implicit animations:

[形狀變化完成程式碼][shape-shifting complete] 範例
對 `margin`、`borderRadius` 和 `color` 屬性值進行了動畫變換。
注意：`AnimatedContainer` 可以對它的任意屬性進行動畫改變，
包括那些您沒有使用的屬性，比如 `padding`、`transform`，
甚至是 `child` 和 `alignment`!
這個 [形狀變化完成程式碼][shape-shifting complete] 的範例
建立在 [漸變完成程式碼][fade-in complete] 的基礎上，
展現出隱含動畫的額外功能:

* Some implicit animations (for example,
  `AnimatedOpacity`) only animate a single
  property, while others (like `AnimatedContainer`)
  can animate many properties.
  
  一些隱含動畫（比如 `AnimatedOpacity`）只能對一個屬性值進行動畫變換，
  然而有些（比如 `AnimatedContainer`）可以同時變換多個屬性。
  
* Implicit animations automatically animate between the old and
  new values of properties when they change using the provided
  `curve` and `duration`.
  
  隱含動畫會在新舊屬性值變換時，
  自動使用提供的 `curve` 和 `duration` 進行動畫變換。
  
* If you do not specify a `curve`,
  implicit animations default to a [linear curve].

  如果您沒有指定 `curve`，隱含動畫的曲線會預設使用 [線性曲線][linear curve]。

## What's next?

## 下一個是什麼？

Congratulations, you've finished the codelab! If you'd like to learn more,
here are some suggestions for where to go next:

恭喜，您已經完成了這個 codelab！
如果您想要了解更多，這裡有一些其他文章的推薦：

* Try the [animations tutorial].

  嘗試一下 [動畫課程][animations tutorial]。

* Learn about [hero animations] and [staggered animations].

  學習 [hero 動畫][hero animations] 和 [staggered 動畫][staggered animations]。

* Checkout the [animation library].

  檢視更多 [動畫函式庫][animation library] 的資訊。

* Try another [codelab].

  嘗試一下其他的 [codelab][]。

<iframe src="https://g.forms.cn/forms/d/e/1FAIpQLSfTcB884FuPXukPEEewU5pgphZyF2Ue0pOWoIHvRp-4D-xYjw/viewform?embedded=true" width="100%" height="1726" frameborder="0" marginheight="0" marginwidth="0">Loading…</iframe>

[AnimatedContainer]: {{site.api}}/flutter/widgets/AnimatedContainer-class.html
[AnimatedOpacity]: {{site.api}}/flutter/widgets/AnimatedOpacity-class.html
[animation library]: {{site.api}}/flutter/animation/animation-library.html
[animations tutorial]: {{site.url}}/development/ui/animations/tutorial
[codelab]: {{site.url}}/codelabs
[curve]: {{site.api}}/flutter/animation/Curve-class.html
[DartPad troubleshooting page]: {{site.dart-site}}/tools/dartpad/troubleshoot
[DartPad]: {{site.dartpad}}
[duration]: {{site.api}}/flutter/widgets/ImplicitlyAnimatedWidget/duration.html
[easeInOutBack]: {{site.api}}/flutter/animation/Curves/easeInOutBack-constant.html
[fade-in complete]: #fade-in-complete
[fade-in starter code]: #fade-in-starter-code
[Fade-in text effect]: #example-fade-in-text-effect
[hero animations]: {{site.url}}/development/ui/animations/hero-animations
[ImplicitlyAnimatedWidget]: {{site.api}}/flutter/widgets/ImplicitlyAnimatedWidget-class.html
[linear animation curve]: {{site.api}}/flutter/animation/Curves/linear-constant.html
[linear curve]: {{site.api}}/flutter/animation/Curves/linear-constant.html
[list of common implicitly animated widgets]: {{site.api}}/flutter/widgets/ImplicitlyAnimatedWidget-class.html
[list of curve constants]: {{site.api}}/flutter/animation/Curves-class.html
[make a Flutter app]: {{site.codelabs}}/codelabs/first-flutter-app-pt1/
[Material App]: {{site.api}}/flutter/material/MaterialApp-class.html
[shape-shifting complete]: #shape-shifting-complete
[Shape-shifting effect]: #example-shape-shifting-effect
[shape-shifting starter code]: #shape-shifting-starter-code
[staggered animations]: {{site.url}}/development/ui/animations/staggered-animations
[stateful widgets]: {{site.url}}/development/ui/interactive#stateful-and-stateless-widgets
