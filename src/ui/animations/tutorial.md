---
title: Animations tutorial
title: 課程 | 在 Flutter 應用裡實現動畫效果
short-title: Tutorial
short-title: 課程
description: A tutorial showing how to build explicit animations in Flutter.
description: 如何在 Flutter 中實現動畫效果。
tags: 使用者介面,Flutter UI,動畫
keywords: 課程,實戰,顯式動畫
diff2html: true
---

{% assign api = '{{site.api}}/flutter' -%}
{% capture examples -%} {{site.repo.this}}/tree/{{site.branch}}/examples {%- endcapture -%}

<?code-excerpt path-base="animation"?>

{{site.alert.secondary}}

  <h4 class="no_toc">What you'll learn</h4>

  <h4 class="no_toc">本章內容</h4>

  * How to use the fundamental classes from the
    animation library to add animation to a widget.

    如何使用動畫函式庫中的基本類為 widget 新增動畫。
  
  * When to use `AnimatedWidget` vs. `AnimatedBuilder`.

    `AnimatedWidget` 和 `AnimatedBuilder` 的應用區別。
  
{{site.alert.end}}

This tutorial shows you how to build explicit animations in Flutter.
After introducing some of the essential concepts, classes,
and methods in the animation library, it walks you through 5
animation examples. The examples build on each other,
introducing you to different aspects of the animation library.

本課程將講解如何在 Flutter 中建構顯式動畫。
我們先來介紹一些動畫函式庫中的基本概念，類和方法，
然後列舉五個動畫範例。這些範例互相關聯，
展示了動畫函式庫的不同方面。

The Flutter SDK also provides built-in explicit animations,
such as [`FadeTransition`][], [`SizeTransition`][],
and [`SlideTransition`][]. These simple animations are
triggered by setting a beginning and ending point.
They are simpler to implement
than custom explicit animations, which are described here.

Flutter SDK 也內建了顯式動畫，比如
[`FadeTransition`][]，[`SizeTransition`][] 和 [`SlideTransition`][]。
這些簡單的動畫可以透過設定起點和終點來觸發。
它們比下面介紹的顯式動畫更容易實現。

<a id="concepts"></a>
## Essential animation concepts and classes

## 基本動畫概念和類

{{site.alert.secondary}}

  <h4 class="no_toc">What's the point?</h4>

  <h4 class="no_toc">重點是什麼？</h4>

  * [`Animation`][], a core class in Flutter's animation library,
    interpolates the values used to guide an animation.

    [`Animation`][]，Flutter 動畫函式庫中的核心類，
    插入用於指導動畫的值。

    * An `Animation` object knows the current state of an animation
    (for example, whether it's started, stopped,
    or moving forward or in reverse),
    but doesn't know anything about what appears onscreen.
    
    `Animation` 物件知道動畫目前的狀態
    （例如，是否開始，暫停，前進或倒退），但是對螢幕上顯示的內容一無所知。
    
  * An [`AnimationController`][] manages the `Animation`.
  
    [`AnimationController`][] 管理 `Animation`。
    
  * A [`CurvedAnimation`][] defines progression as a non-linear curve.
  
    [`CurvedAnimation`][] 定義處理序為非線性曲線。
    
  * A [`Tween`][] interpolates between the range of data as used by the
    object being animated.
    For example, a `Tween` might define an interpolation
    from red to blue, or from 0 to 255.

    [`Tween`][] 為動畫物件插入一個範圍值。
    例如，`Tween` 可以定義插入值由紅到藍，或從 0 到 255。

  * Use `Listener`s and `StatusListener`s to monitor
    animation state changes.

    使用 `Listener` 和 `StatusListener` 監視動畫狀態變化。

{{site.alert.end}}

The animation system in Flutter is based on typed
[`Animation`][] objects. Widgets can either incorporate
these animations in their build functions directly by
reading their current value and listening to their state
changes or they can use the animations as the basis of
more elaborate animations that they pass along to
other widgets.

Flutter 中的動畫系統基於型別化的 [`Animation`] [] 物件。
Widgets 既可以透過讀取當前值和監聽狀態變化直接
合併動畫到 build 函式，也可以作為傳遞給其他 widgets
的更精細動畫的基礎。

<a id="animation-class"></a>
### Animation<wbr>\<double>

In Flutter, an `Animation` object knows nothing about what
is onscreen. An `Animation` is an abstract class that
understands its current value and its state (completed or dismissed).
One of the more commonly used animation types is `Animation<double>`.

在 Flutter 中，動畫物件無法獲取螢幕上顯示的內容。
`Animation` 是一個已知當前值和狀態（已完成或已解除）的抽象類別。
一個比較常見的動畫型別是 `Animation<double>`。

An `Animation` object sequentially generates
interpolated numbers between two values over a certain duration.
The output of an `Animation` object might be linear,
a curve, a step function, or any other mapping you can devise.
Depending on how the `Animation` object is controlled,
it could run in reverse, or even switch directions in the
middle.

一個 `Animation` 物件在一段時間內，
持續產生介於兩個值之間的插入值。
這個 `Animation` 物件輸出的可能是直線，
曲線，階梯函式，或者任何自訂的對映。
根據 `Animation` 物件的不同控制方式，
它可以反向執行，或者中途切換方向。

Animations can also interpolate types other than double, such as
`Animation<Color>` or `Animation<Size>`.

動畫還可以插入除 double 以外的型別，
比如 `Animation<Color>` 或者 `Animation<Size>`。

An `Animation` object has state. Its current value is
always available in the `.value` member.

`Animation` 物件具有狀態。
它的當前值在 `.value` 中始終可用。

An `Animation` object knows nothing about rendering or
`build()` functions.

`Animation` 物件與渲染或 `build()` 函式無關。

### Curved&shy;Animation

A [`CurvedAnimation`][] defines the animation's progress
as a non-linear curve.

[`CurvedAnimation`][] 定義動畫處理序為非線性曲線。

<?code-excerpt "animate5/lib/main.dart (CurvedAnimation)"?>
```dart
animation = CurvedAnimation(parent: controller, curve: Curves.easeIn);
```

{{site.alert.note}}

  The [`Curves`][] class defines many commonly used curves,
  or you can create your own. For example:

  [`Curves`][] 類定義了很多常用曲線，
  或者您也可以自訂。例如：

  <?code-excerpt "animate5/lib/main.dart (ShakeCurve)" plaster="none"?>
  {% prettify dart context="html" %}
  import 'dart:math';

  class ShakeCurve extends Curve {
    @override
    double transform(double t) => sin(t * pi * 2);
  }
  {% endprettify %}

  Browse the [`Curves`] documentation for a complete listing
  (with visual previews) of the `Curves` constants that ship with Flutter.

  瀏覽 [`Curves`] 文件以獲取完整的
  （帶有預覽）Flutter 附帶的 `Curves` 常數列表。

{{site.alert.end}}

`CurvedAnimation` and `AnimationController` (described in the next section)
are both of type `Animation<double>`, so you can pass them interchangeably.
The `CurvedAnimation` wraps the object it's modifying&mdash;you
don't subclass `AnimationController` to implement a curve.

`CurvedAnimation` 和 `AnimationController`（下面將會詳細說明）
都是 `Animation<double>` 型別，所以可以互換使用。
`CurvedAnimation` 封裝正在修改的物件 &mdash; 
不需要將 `AnimationController` 分解成子類別來實現曲線。

### Animation&shy;Controller

[`AnimationController`][] is a special `Animation`
object that generates a new value whenever the hardware
is ready for a new frame. By default,
an `AnimationController` linearly produces the numbers
from 0.0 to 1.0 during a given duration.
For example, this code creates an `Animation` object,
but does not start it running:

[`AnimationController`][] 是個特殊的 `Animation` 物件，
每當硬體準備新幀時，他都會產生一個新值。
預設情況下，`AnimationController` 在給定期間內會線性
產生從 0.0 到 1.0 的數字。
例如，這段程式碼建立了一個動畫物件，但是沒有啟動執行。

<?code-excerpt "animate5/lib/main.dart (AnimationController)"?>
```dart
controller =
    AnimationController(duration: const Duration(seconds: 2), vsync: this);
```

`AnimationController` derives from `Animation<double>`, so it can be used
wherever an `Animation` object is needed. However, the `AnimationController`
has additional methods to control the animation. For example, you start
an animation with the `.forward()` method. The generation of numbers is
tied to the screen refresh, so typically 60 numbers are generated per
second. After each number is generated, each `Animation` object calls the
attached `Listener` objects. To create a custom display list for each
child, see [`RepaintBoundary`][].

`AnimationController` 源自於 `Animation<double>`，
所以可以用在任何需要 `Animation` 物件的地方。
但是 `AnimationController` 還有其他方法控制動畫。
例如，使用 `.forward()` 方法啟動動畫。
數字的產生與螢幕重新整理關聯，所以一般來說每秒鐘會產生 60 個數字。
數字產生之後，每個動畫物件都呼叫附加 Listener 物件。
為每個 child 建立自訂顯示列表，
請參考 [`RepaintBoundary`][]。

When creating an `AnimationController`, you pass it a `vsync` argument.
The presence of `vsync` prevents offscreen animations from consuming
unnecessary resources.
You can use your stateful object as the vsync by adding
`SingleTickerProviderStateMixin` to the class definition.
You can see an example of this in [animate1][] on GitHub.

建立 `AnimationController` 的同時，
也賦予了一個 `vsync` 引數。
`vsync` 的存在防止後臺動畫消耗不必要的資源。
您可以透過新增 `SingleTickerProviderStateMixin` 到類定義，
將有狀態的物件用作 vsync。
可參考 GitHub 網站 [animate1]({{examples}}/animation/animate1/lib/main.dart) 
中的範例。

{% comment %}
The `vsync` object ties the ticking of the animation controller to
the visibility of the widget, so that when the animating widget goes
off-screen, the ticking stops, and when the widget is restored, it
starts again (without stopping the clock, so it's as if it had
been ticking the whole time, but without using the CPU.)
To use your custom State object as the `vsync`, include the
`TickerProviderStateMixin` when defining the custom State class.

`vsync` 物件將動畫控制器的時鐘系統和 widget 的可見性連結，
所以當動畫 widget 切換到後臺，時鐘停止；而當 widget 恢復前臺執行，
時鐘重新啟動（這個過程時鐘並不終止，可被認為一直在執行中，
只是不佔用 CPU）。在定義自訂狀態類時，
也可將自訂狀態物件用作 `vsync`，包括 `TickerProviderStateMixin`。
{% endcomment -%}

{{site.alert.note}}

  In some cases, a position might exceed the `AnimationController`'s
  0.0-1.0 range. For example, the `fling()` function
  allows you to provide velocity, force, and position
  (via the Force object). The position can be anything and
  so can be outside of the 0.0 to 1.0 range.
  
  在一些情況下，一個位置可能會超過
  `AnimationController` 的 0.0-1.0 的範圍。
  例如，`fling()` 函式允許提供速度，
  力和位置（透過 Force 物件）。這個位置可以是任意的，
  所以可能會超出 0.0-1.0 的範圍。

  A `CurvedAnimation` can also exceed the 0.0 to 1.0 range,
  even if the `AnimationController` doesn't.
  Depending on the curve selected, the output of
  the `CurvedAnimation` can have a wider range than the input.
  For example, elastic curves such as `Curves.elasticIn`
  significantly overshoots or undershoots the default range.
  
  即使 `AnimationController` 在範圍內，
  `CurvedAnimation` 也可能會出現超出 0.0-1.0 範圍的情況。
  根據所選曲線的不同，`CurvedAnimation` 的輸出範圍可能會超過輸入。
  舉個例子，彈性曲線（比如Curves.elasticIn）
  會明顯超出或低於預設範圍。
{{site.alert.end}}

### Tween

By default, the `AnimationController` object ranges from 0.0 to 1.0.
If you need a different range or a different data type, you can use a
[`Tween`][] to configure an animation to interpolate to a
different range or data type. For example, the
following `Tween` goes from -200.0 to 0.0:

在預設情況下，`AnimationController` 物件的範圍是 0.0-0.1。
如果需要不同的範圍或者不同的資料型別，
可以使用 [`Tween`][] 配置動畫來插入不同的範圍或資料型別。
例如下面的範例中，`Tween` 的範圍是 -200 到 0.0。

<?code-excerpt "animate5/lib/main.dart (tween)"?>
```dart
tween = Tween<double>(begin: -200, end: 0);
```

A `Tween` is a stateless object that takes only `begin` and `end`.
The sole job of a `Tween` is to define a mapping from an
input range to an output range. The input range is commonly
0.0 to 1.0, but that's not a requirement.

`Tween` 是無狀態的物件，只有 `begin` 和 `end`。
`Tween` 的這種單一用途用來定義從輸入範圍到輸出範圍的對映。
輸入範圍一般為 0.0-1.0，但這並不是必須的。

A `Tween` inherits from `Animatable<T>`, not from `Animation<T>`.
An `Animatable`, like `Animation`, doesn't have to output double.
For example, `ColorTween` specifies a progression between two colors.

`Tween` 源自 `Animatable<T>`，而不是 `Animation<T>`。
像動畫這樣的可動畫元素不必重複輸出。
例如，`ColorTween` 指定了兩種顏色之間的過程。

<?code-excerpt "animate5/lib/main.dart (colorTween)"?>
```dart
colorTween = ColorTween(begin: Colors.transparent, end: Colors.black54);
```

A `Tween` object doesn't store any state. Instead, it provides the
[`evaluate(Animation<double> animation)`][] method that uses the 
`transform` function to map the current value of the animation
(between 0.0 and 1.0), to the actual animation value. 

The current value of the `Animation` object can be found in the
`.value` method. The evaluate function also performs some housekeeping,
such as ensuring that begin and end are returned when the
animation values are 0.0 and 1.0, respectively.

`Tween` 物件不儲存任何狀態。
而是提供 `evaluate(Animation<double> animation)` 方法，
將對映函式應用於動畫當前值。
`Animation` 物件的當前值可以在 `.value` 方法中找到。
evaluate 函式還執行一些內部處理內容，
比如確保當動畫值在 0.0 和1.0 時分別返回起始點和終點。

#### Tween.animate

To use a `Tween` object, call `animate()` on the `Tween`,
passing in the controller object. For example,
the following code generates the
integer values from 0 to 255 over the course of 500 ms.

要使用 `Tween` 物件，請在 `Tween` 呼叫 `animate()`，
傳入控制器物件。例如，下面的程式碼在 500 ms 的處理序中
產生 0-255 範圍內的整數值。

<?code-excerpt "animate5/lib/main.dart (IntTween)"?>
```dart
AnimationController controller = AnimationController(
    duration: const Duration(milliseconds: 500), vsync: this);
Animation<int> alpha = IntTween(begin: 0, end: 255).animate(controller);
```

{{site.alert.note}}

  The `animate()` method returns an [`Animation`][],
  not an [`Animatable`][].
  
  `animate()` 方法會返回一個 [`Animation`][]，而不是 [`Animatable`][]。
{{site.alert.end}}

The following example shows a controller, a curve, and a `Tween`:

下面的範例展示了一個控制器，一個曲線，和一個 `Tween`。

<?code-excerpt "animate5/lib/main.dart (IntTween-curve)"?>
```dart
AnimationController controller = AnimationController(
    duration: const Duration(milliseconds: 500), vsync: this);
final Animation<double> curve =
    CurvedAnimation(parent: controller, curve: Curves.easeOut);
Animation<int> alpha = IntTween(begin: 0, end: 255).animate(curve);
```

### Animation notifications

### 動畫通知

An [`Animation`][] object can have `Listener`s and `StatusListener`s,
defined with `addListener()` and `addStatusListener()`.
A `Listener` is called whenever the value of the animation changes.
The most common behavior of a `Listener` is to call `setState()`
to cause a rebuild. A `StatusListener` is called when an animation begins,
ends, moves forward, or moves reverse, as defined by `AnimationStatus`.
The next section has an example of the `addListener()` method,
and [Monitoring the progress of the animation][] shows an
example of `addStatusListener()`.

一個 [`Animation`][] 物件可以有不止一個
`Listener` 和 `StatusListener`，
用 `addListener()` 和 `addStatusListener()` 來定義。
當動畫值改變時呼叫 `Listener`。
`Listener` 最常用的操作是呼叫 `setState()` 進行重建。
當一個動畫開始，結束，前進或後退時，
會呼叫 `StatusListener`，用 `AnimationStatus` 來定義。
下一部分有關於 `addListener()` 方法的範例，
在 [監控動畫過程][Monitoring the progress of the animation]
中也有 `addStatusListener()` 的範例。

---

## Animation examples

## 動畫範例

This section walks you through 5 animation examples.
Each section provides a link to the source code for that example.

這部分列舉了五個動畫範例，
每個範例都提供了原始碼的連結。

### Rendering animations

### 渲染動畫

{{site.alert.secondary}}
  <h4 class="no_toc">What's the point?</h4>

  <h4 class="no_toc">要點</h4>

  * How to add basic animation to a widget using `addListener()` and
    `setState()`.
    
    如何使用 `addListener()` 和 `setState()` 為 widget 新增基礎動畫。
    
  * Every time the Animation generates a new number, the `addListener()`
    function calls `setState()`.
    
    每次動畫產生一個新的數字，`addListener()` 函式就會呼叫 `setState()`。
    
  * How to define an `AnimationController` with the required
    `vsync` parameter.
    
    如何使用所需的 `vsync` 引數定義一個 `AnimationController`。
    
  * Understanding the "`..`" syntax in "`..addListener`",
    also known as Dart's _cascade notation_.
    
    理解 "`..addListener`" 中的 "`..`" 語法，也稱為 Dart 的 _cascade notation_。
    
  * To make a class private, start its name with an underscore (`_`).
  
    如需使類私有，在名字前面新增下劃線（`_`）。
    
{{site.alert.end}}

So far you've learned how to generate a sequence of numbers over time.
Nothing has been rendered to the screen. To render with an
`Animation` object, store the `Animation` object as a
member of your widget, then use its value to decide how to draw.

目前為止，我們學習瞭如何隨著時間產生數字序列。但螢幕上並未顯示任何內容。
要顯示一個 `Animation` 物件，需將 `Animation` 物件儲存為您的 widget 成員，
然後用它的值來決定如何繪製。

Consider the following app that draws the Flutter logo without animation:

參考下面的應用程式，它沒有使用動畫繪製 Flutter logo。

<?code-excerpt "animate0/lib/main.dart"?>
```dart
import 'package:flutter/material.dart';

void main() => runApp(const LogoApp());

class LogoApp extends StatefulWidget {
  const LogoApp({super.key});

  @override
  State<LogoApp> createState() => _LogoAppState();
}

class _LogoAppState extends State<LogoApp> {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Container(
        margin: const EdgeInsets.symmetric(vertical: 10),
        height: 300,
        width: 300,
        child: const FlutterLogo(),
      ),
    );
  }
}
```

**App source:** [animate0][]

**原始碼:** [animate0][]

The following shows the same code modified to animate the
logo to grow from nothing to full size.
When defining an `AnimationController`, you must pass in a
`vsync` object. The `vsync` parameter is described in the
[`AnimationController` section][].

下面的程式碼是加入動畫效果的，logo 從無到全屏。
當定義 `AnimationController` 時，
必須要使用一個 `vsync` 物件。
在 [`AnimationController` 部分][`AnimationController` section]
會具體介紹 `vsync` 引數。

The changes from the non-animated example are highlighted:

對比無動畫範例，改動部分被突出顯示：

<?code-excerpt "animate{0,1}/lib/main.dart"?>
```diff
--- animate0/lib/main.dart
+++ animate1/lib/main.dart
@@ -9,16 +9,39 @@
   State<LogoApp> createState() => _LogoAppState();
 }

-class _LogoAppState extends State<LogoApp> {
+class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
+  late Animation<double> animation;
+  late AnimationController controller;
+
+  @override
+  void initState() {
+    super.initState();
+    controller =
+        AnimationController(duration: const Duration(seconds: 2), vsync: this);
+    animation = Tween<double>(begin: 0, end: 300).animate(controller)
+      ..addListener(() {
+        setState(() {
+          // The state that has changed here is the animation object's value.
+        });
+      });
+    controller.forward();
+  }
+
   @override
   Widget build(BuildContext context) {
     return Center(
       child: Container(
         margin: const EdgeInsets.symmetric(vertical: 10),
-        height: 300,
-        width: 300,
+        height: animation.value,
+        width: animation.value,
         child: const FlutterLogo(),
       ),
     );
   }
+
+  @override
+  void dispose() {
+    controller.dispose();
+    super.dispose();
+  }
 }
```

**App source:** [animate1][]

**原始碼:** [animate1][]

The `addListener()` function calls `setState()`,
so every time the `Animation` generates a new number,
the current frame is marked dirty, which forces
`build()` to be called again. In `build()`,
the container changes size because its height and
width now use `animation.value` instead of a hardcoded value.
Dispose of the controller when the `State` object is
discarded to prevent memory leaks.

因為`addListener()` 函式呼叫 `setState()`，
所以每次 `Animation` 產生一個新的數字，
當前幀就被標記為 dirty，使得 `build()` 再次被呼叫。
在 `build()` 函式中，container 會改變大小，
因為它的高和寬都讀取 `animation.value`，而不是固定編碼值。
當 `State` 物件銷燬時要清除控制器以防止記憶體溢位。

With these few changes,
you've created your first animation in Flutter!

經過這些小改動，你成功建立了第一個 Flutter 動畫。


{{site.alert.secondary}}

  **Dart language tricks:**
  You might not be familiar with Dart's cascade notation&mdash;the two
  dots in `..addListener()`. This syntax means that the `addListener()`
  method is called with the return value from `animate()`.
  Consider the following example:

  **Dart 語言技巧：**
  你可能對於 Dart 的級聯運運算元（`..addListener()` 中的兩點）不太熟悉。
  這個語法意思是使用 `animate()` 的返回值呼叫  `addListener()` 方法。
  參考下面範例：

  <?code-excerpt "animate1/lib/main.dart (addListener)" replace="/animation.*|\.\.addListener/[!$&!]/g"?>
  {% prettify dart context="html" %}
  [!animation = Tween<double>(begin: 0, end: 300).animate(controller)!]
    [!..addListener!](() {
      // ···
    });
  {% endprettify %}

  This code is equivalent to:

  這段程式碼相當於：

  <?code-excerpt "animate1/lib/main.dart (addListener)" replace="/animation.*/$&;/g; /  \./animation/g; /animation.*/[!$&!]/g"?>
  {% prettify dart context="html" %}
  [!animation = Tween<double>(begin: 0, end: 300).animate(controller);!]
  [!animation.addListener(() {!]
      // ···
    });
  {% endprettify %}

  To learn more about cascades,
  check out [Cascade notation][]
  in the [Dart language documentation][].

  要了解更多級聯運運算元的內容，請參考 [Dart 文件網站][Dart language documentation]
  裡的文件 [Cascade notation][]。
{{site.alert.end}}

###  Simplifying with Animated&shy;Widget

###  使用 Animated&shy;Widget 進行簡化

{{site.alert.secondary}}
  <h4 class="no_toc">What's the point?</h4>

  <h4 class="no_toc">要點</h4>

  * How to use the [`AnimatedWidget`][] helper class
    (instead of `addListener()`
    and `setState()`) to create a widget that animates.
    
    如何使用 [`AnimatedWidget`][] 幫助類
   （代替 `addListener()` 和 `setState()`）建立動畫 widget。
    
  * Use `AnimatedWidget` to create a widget that performs
    a reusable animation.
    To separate the transition from the widget, use an
    `AnimatedBuilder`, as shown in the
    [Refactoring with AnimatedBuilder][] section.
    
    利用 `AnimatedWidget` 建立一個可以執行重複使用動畫的 widget。
    如需區分 widget 過渡，可以使用 `AnimatedBuilder`，你可以在
    [使用 AnimatedBuilder 進行重構][Refactoring with AnimatedBuilder]
    部分查閱更多資訊。
    
  * Examples of `AnimatedWidget`s in the Flutter API:
    `AnimatedBuilder`, `AnimatedModalBarrier`,
    `DecoratedBoxTransition`, `FadeTransition`,
    `PositionedTransition`, `RelativePositionedTransition`,
    `RotationTransition`, `ScaleTransition`,
    `SizeTransition`, `SlideTransition`.
    
    Flutter API 中的 `AnimatedWidget`：`AnimatedBuilder`,
    `RotationTransition`, `ScaleTransition`, `SizeTransition`, `SlideTransition`。
{{site.alert.end}}

The `AnimatedWidget` base class allows you to separate out
the core widget code from the animation code.
`AnimatedWidget` doesn't need to maintain a `State`
object to hold the animation. Add the following `AnimatedLogo` class:

`AnimatedWidget` 基本類可以從動畫程式碼中區分出核心 widget 程式碼。
`AnimatedWidget` 不需要保持 `State` 物件來 hold 動畫。
可以新增下面的 `AnimatedLogo` 類：

<?code-excerpt path-base="animation/animate2"?>
<?code-excerpt "lib/main.dart (AnimatedLogo)" title?>
```dart
class AnimatedLogo extends AnimatedWidget {
  const AnimatedLogo({super.key, required Animation<double> animation})
      : super(listenable: animation);

  @override
  Widget build(BuildContext context) {
    final animation = listenable as Animation<double>;
    return Center(
      child: Container(
        margin: const EdgeInsets.symmetric(vertical: 10),
        height: animation.value,
        width: animation.value,
        child: const FlutterLogo(),
      ),
    );
  }
}
```
<?code-excerpt path-base="animation"?>

`AnimatedLogo` uses the current value of the `animation`
when drawing itself.

在繪製時，`AnimatedLogo` 會讀取 `animation` 當前值。

The `LogoApp` still manages the `AnimationController` and the `Tween`,
and it passes the `Animation` object to `AnimatedLogo`:

`LogoApp` 持續控制 `AnimationController` 和 `Tween`，
並將 `Animation` 物件傳給 `AnimatedLogo`：

<?code-excerpt "animate{1,2}/lib/main.dart" from="class _LogoAppState" diff-u="6"?>
```diff
--- animate1/lib/main.dart
+++ animate2/lib/main.dart
@@ -1,10 +1,28 @@
 import 'package:flutter/material.dart';

 void main() => runApp(const LogoApp());

+class AnimatedLogo extends AnimatedWidget {
+  const AnimatedLogo({super.key, required Animation<double> animation})
+      : super(listenable: animation);
+
+  @override
+  Widget build(BuildContext context) {
+    final animation = listenable as Animation<double>;
+    return Center(
+      child: Container(
+        margin: const EdgeInsets.symmetric(vertical: 10),
+        height: animation.value,
+        width: animation.value,
+        child: const FlutterLogo(),
+      ),
+    );
+  }
+}
+
 class LogoApp extends StatefulWidget {
   const LogoApp({super.key});

   @override
   State<LogoApp> createState() => _LogoAppState();
 }
@@ -15,32 +33,18 @@

   @override
   void initState() {
     super.initState();
     controller =
         AnimationController(duration: const Duration(seconds: 2), vsync: this);
-    animation = Tween<double>(begin: 0, end: 300).animate(controller)
-      ..addListener(() {
-        setState(() {
-          // The state that has changed here is the animation object's value.
-        });
-      });
+    animation = Tween<double>(begin: 0, end: 300).animate(controller);
     controller.forward();
   }

   @override
-  Widget build(BuildContext context) {
-    return Center(
-      child: Container(
-        margin: const EdgeInsets.symmetric(vertical: 10),
-        height: animation.value,
-        width: animation.value,
-        child: const FlutterLogo(),
-      ),
-    );
-  }
+  Widget build(BuildContext context) => AnimatedLogo(animation: animation);

   @override
   void dispose() {
     controller.dispose();
     super.dispose();
   }
```

**App source:** [animate2][]

**原始碼：** [animate2][]

<a id="monitoring"></a>
### Monitoring the progress of the animation

### 監控動畫過程

{{site.alert.secondary}}

  <h4 class="no_toc">What's the point?</h4>

  <h4 class="no_toc">要點</h4>

  * Use `addStatusListener()` for notifications of changes
    to the animation's state, such as starting, stopping,
    or reversing direction.
    
    使用 `addStatusListener()` 作為動畫狀態的變更提示，比如開始，結束，或改變方向。
    
  * Run an animation in an infinite loop by reversing direction when
    the animation has either completed or returned to its starting state.
    
    透過在動畫完成或返回起始狀態時改變方向，使動畫無限迴圈。

{{site.alert.end}}

It's often helpful to know when an animation changes state,
such as finishing, moving forward, or reversing.
You can get notifications for this with `addStatusListener()`.
The following code modifies the previous example so that
it listens for a state change and prints an update.
The highlighted line shows the change:

瞭解動畫何時改變狀態通常是很有用的，比如完成，前進或後退。
可以透過 `addStatusListener()` 來獲得提示。
下面是之前範例修改後的程式碼，這樣就可以監聽狀態的改變和更新。
修改部分會突出顯示：

<?code-excerpt "animate3/lib/main.dart (print state)" plaster="none" replace="/\/\/ (\.\..*)/$1;/g; /\.\..*/[!$&!]/g; /\n  }/$&\n  \/\/ .../g"?>
```dart
class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
  late Animation<double> animation;
  late AnimationController controller;

  @override
  void initState() {
    super.initState();
    controller =
        AnimationController(duration: const Duration(seconds: 2), vsync: this);
    animation = Tween<double>(begin: 0, end: 300).animate(controller)
      [!..addStatusListener((status) => print('$status'));!]
    controller.forward();
  }
  // ...
}
```

Running this code produces this output:

執行這段程式碼，得到如下結果：

```console
AnimationStatus.forward
AnimationStatus.completed
```

Next, use `addStatusListener()` to reverse the animation
at the beginning or the end. This creates a "breathing" effect:

下一步，在起始或結束時，使用 `addStatusListener()` 反轉動畫。
製造“呼吸”效果：

<?code-excerpt "animate{2,3}/lib/main.dart" to="/^   }/" diff-u="4"?>
```diff
--- animate2/lib/main.dart
+++ animate3/lib/main.dart
@@ -35,7 +35,15 @@
   void initState() {
     super.initState();
     controller =
         AnimationController(duration: const Duration(seconds: 2), vsync: this);
-    animation = Tween<double>(begin: 0, end: 300).animate(controller);
+    animation = Tween<double>(begin: 0, end: 300).animate(controller)
+      ..addStatusListener((status) {
+        if (status == AnimationStatus.completed) {
+          controller.reverse();
+        } else if (status == AnimationStatus.dismissed) {
+          controller.forward();
+        }
+      })
+      ..addStatusListener((status) => print('$status'));
     controller.forward();
   }
```

**App source:** [animate3][]

**原始碼：** [animate3][]

### Refactoring with AnimatedBuilder

### 使用 AnimatedBuilder 進行重構

{{site.alert.secondary}}

  <h4 class="no_toc">What's the point?</h4>

  <h4 class="no_toc">要點</h4>

  * An [`AnimatedBuilder`][] understands how to render the transition.
  
    [`AnimatedBuilder`][] 知道如何渲染過渡效果
    
  * An `AnimatedBuilder` doesn't know how to render the widget, nor does it
    manage the Animation object.
    
    但 `AnimatedBuilder` 不會渲染 widget，也不會控制動畫物件。
    
  * Use `AnimatedBuilder` to describe an animation as
    part of a build method for another widget.
    If you simply want to define a widget with a reusable
    animation, use an `AnimatedWidget`, as shown in
    the [Simplifying with AnimatedWidget][] section.
    
    使用 `AnimatedBuilder` 描述一個動畫是其他 widget 建構方法的一部分。
    如果只是單純需要用可重複使用的動畫定義一個 widget，
    可參考文件：[簡單使用 AnimatedWidget][Simplifying with AnimatedWidget]。
    
  * Examples of `AnimatedBuilders` in the Flutter API: `BottomSheet`,
    `ExpansionTile`, `PopupMenu`, `ProgressIndicator`,
    `RefreshIndicator`, `Scaffold`, `SnackBar`, `TabBar`,
    `TextField`.
    
    Flutter API 中 `AnimatedBuilders`：`BottomSheet`, `ExpansionTile`,
    `PopupMenu`, `ProgressIndicator`, `RefreshIndicator`, `Scaffold`, `SnackBar`, `TabBar`,
    `TextField`。

{{site.alert.end}}

One problem with the code in the [animate3][] example,
is that changing the animation required changing the widget
that renders the logo. A better solution
is to separate responsibilities into different classes:

[animate3][] 範例程式碼中有個問題，
就是改變動畫需要改變渲染 logo 的widget。
較好的解決辦法是，將任務區分到不同類裡：

* Render the logo

  渲染 logo
  
* Define the `Animation` object

  定義動畫物件
  
* Render the transition

  渲染過渡效果

You can accomplish this separation with the help of the
`AnimatedBuilder` class. An `AnimatedBuilder` is a
separate class in the render tree. Like `AnimatedWidget`,
`AnimatedBuilder` automatically listens to notifications
from the `Animation` object, and marks the widget tree
dirty as necessary, so you don't need to call `addListener()`.

您可以使用 `AnimatedBuilder` 類方法來完成分配。
`AnimatedBuilder` 作為渲染樹的一個單獨類別。
像 `AnimatedWidget`，`AnimatedBuilder` 自動監聽動畫物件提示，
並在必要時在 widget 樹中標出，
所以這時不需要呼叫 `addListener()`。

The widget tree for the [animate4][]
example looks like this:

應用於 [animate4][] 範例的 widget 樹長這樣：

<img src="/assets/images/docs/ui/AnimatedBuilder-WidgetTree.png"
    alt="AnimatedBuilder widget tree" class="d-block mx-auto" width="160px">

Starting from the bottom of the widget tree, the code for rendering
the logo is straightforward:

從 widget 樹底部開始，渲染 logo 的程式碼很容易：

<?code-excerpt "animate4/lib/main.dart (LogoWidget)"?>
```dart
class LogoWidget extends StatelessWidget {
  const LogoWidget({super.key});

  // Leave out the height and width so it fills the animating parent
  @override
  Widget build(BuildContext context) {
    return Container(
      margin: const EdgeInsets.symmetric(vertical: 10),
      child: const FlutterLogo(),
    );
  }
}
```

The middle three blocks in the diagram are all created in the
`build()` method in `GrowTransition`, shown below.
The `GrowTransition` widget itself is stateless and holds
the set of final variables necessary to define the transition animation.
The build() function creates and returns the `AnimatedBuilder`,
which takes the (`Anonymous` builder) method and the
`LogoWidget` object as parameters. The work of rendering the
transition actually happens in the (`Anonymous` builder)
method, which creates a `Container` of the appropriate size
to force the `LogoWidget` to shrink to fit.

圖表中間的三部分都是用 `GrowTransition` 中的 `build()` 方法建立的，
如下。 `GrowTransition` widget 本身是無狀態的，
而且擁有定義過渡動畫所需的一系列最終變數。
build() 函式建立並返回 `AnimatedBuilder`，
`AnimatedBuilder` 使用（`Anonymous` builder）
方法並將 LogoWidget 物件作為引數。
渲染過渡效果實際上是在（`Anonymous` builder）方法中完成的，
該方法建立一個適當大小 `Container` 強制 `LogoWidget` 配合。

One tricky point in the code below is that the child looks
like it's specified twice. What's happening is that the
outer reference of child is passed to `AnimatedBuilder`,
which passes it to the anonymous closure, which then uses
that object as its child. The net result is that the
`AnimatedBuilder` is inserted in between the two widgets
in the render tree.

在下面這段程式碼中，一個比較棘手的問題是 child 看起來被指定了兩次。
其實是 child 的外部參照被傳遞給了 `AnimatedBuilder`，
再傳遞給匿名閉套件，然後用作 child 的物件。
最終結果就是 `AnimatedBuilder` 被插入渲染樹的兩個 widgets 中間。

<?code-excerpt "animate4/lib/main.dart (GrowTransition)"?>
```dart
class GrowTransition extends StatelessWidget {
  const GrowTransition(
      {required this.child, required this.animation, super.key});

  final Widget child;
  final Animation<double> animation;

  @override
  Widget build(BuildContext context) {
    return Center(
      child: AnimatedBuilder(
        animation: animation,
        builder: (context, child) {
          return SizedBox(
            height: animation.value,
            width: animation.value,
            child: child,
          );
        },
        child: child,
      ),
    );
  }
}
```

Finally, the code to initialize the animation looks very
similar to the [animate2][] example. The `initState()`
method creates an `AnimationController` and a `Tween`,
then binds them with `animate()`. The magic happens in
the `build()` method, which returns a `GrowTransition`
object with a `LogoWidget` as a child, and an animation object to
drive the transition. These are the three elements listed
in the bullet points above.

最後，初始動畫的程式碼看起來很像 [animate2][] 的範例。
`initState()` 方法建立了 `AnimationController` 和 `Tween`，
然後用 `animate()` 繫結它們。
神奇的是 `build()` 方法，
它返回一個以`LogoWidget` 為 child 的  `GrowTransition` 物件，
和一個驅動過渡的動畫物件。上面列出了三個主要因素。

<?code-excerpt "animate{2,4}/lib/main.dart" from="class _LogoAppState" diff-u="10"?>
```diff
--- animate2/lib/main.dart
+++ animate4/lib/main.dart
@@ -1,27 +1,47 @@
 import 'package:flutter/material.dart';

 void main() => runApp(const LogoApp());

-class AnimatedLogo extends AnimatedWidget {
-  const AnimatedLogo({super.key, required Animation<double> animation})
-      : super(listenable: animation);
+class LogoWidget extends StatelessWidget {
+  const LogoWidget({super.key});
+
+  // Leave out the height and width so it fills the animating parent
+  @override
+  Widget build(BuildContext context) {
+    return Container(
+      margin: const EdgeInsets.symmetric(vertical: 10),
+      child: const FlutterLogo(),
+    );
+  }
+}
+
+class GrowTransition extends StatelessWidget {
+  const GrowTransition(
+      {required this.child, required this.animation, super.key});
+
+  final Widget child;
+  final Animation<double> animation;

   @override
   Widget build(BuildContext context) {
-    final animation = listenable as Animation<double>;
     return Center(
-      child: Container(
-        margin: const EdgeInsets.symmetric(vertical: 10),
-        height: animation.value,
-        width: animation.value,
-        child: const FlutterLogo(),
+      child: AnimatedBuilder(
+        animation: animation,
+        builder: (context, child) {
+          return SizedBox(
+            height: animation.value,
+            width: animation.value,
+            child: child,
+          );
+        },
+        child: child,
       ),
     );
   }
 }

 class LogoApp extends StatefulWidget {
   const LogoApp({super.key});

   @override
   State<LogoApp> createState() => _LogoAppState();
@@ -34,18 +54,23 @@
   @override
   void initState() {
     super.initState();
     controller =
         AnimationController(duration: const Duration(seconds: 2), vsync: this);
     animation = Tween<double>(begin: 0, end: 300).animate(controller);
     controller.forward();
   }

   @override
-  Widget build(BuildContext context) => AnimatedLogo(animation: animation);
+  Widget build(BuildContext context) {
+    return GrowTransition(
+      animation: animation,
+      child: const LogoWidget(),
+    );
+  }

   @override
   void dispose() {
     controller.dispose();
     super.dispose();
   }
 }
```

**App source:** [animate4][]

**原始碼：** [animate4][]

### Simultaneous animations

### 同步動畫

{{site.alert.secondary}}
  <h4 class="no_toc">What's the point?</h4>

  <h4 class="no_toc">重點提醒</h4>

  * The [`Curves`][] class defines an array of
    commonly used curves that you can
    use with a [`CurvedAnimation`][].
    
    [`Curves`][] 類定義了一列常用的曲線，您可以配合 [`CurvedAnimation`][] 來使用。
    
{{site.alert.end}}

In this section, you'll build on the example from
[monitoring the progress of the animation][]
([animate3][]), which used `AnimatedWidget`
to animate in and out continuously. Consider the case
where you want to animate in and out while the
opacity animates from transparent to opaque.

在這部分內容中，您會根據 [監控動畫過程](#monitoring) ([animate3][]) 建立範例，
該範例將使用 `AnimatedWidget` 持續進行動畫。
可以用在需要對透明度進行從透明到不透明動畫處理的情況。

{{site.alert.note}}

  This example shows how to use multiple tweens on the same animation
  controller, where each tween manages a different effect in
  the animation. It is for illustrative purposes only.
  If you were tweening opacity and size in production code,
  you'd probably use [`FadeTransition`][] and [`SizeTransition`][]
  instead.
  
  這個範例展示瞭如何在同一個動畫控制器中使用複合補間動畫，
  每個補間動畫控制一個動畫的不同效果。
  僅用於說明目的。如果您需要在程式碼中加入漸變不透明度和尺寸效果，
  可能需要用 [`FadeTransition`][] 和 [`SizeTransition`][] 來代替。
  
{{site.alert.end}}

Each tween manages an aspect of the animation. For example:

每個補間動畫控制一個動畫的不同方面，例如：

<?code-excerpt "animate5/lib/main.dart (tweens)" plaster="none"?>
```dart
controller =
    AnimationController(duration: const Duration(seconds: 2), vsync: this);
sizeAnimation = Tween<double>(begin: 0, end: 300).animate(controller);
opacityAnimation = Tween<double>(begin: 0.1, end: 1).animate(controller);
```

You can get the size with `sizeAnimation.value` and the opacity
with `opacityAnimation.value`, but the constructor for `AnimatedWidget`
only takes a single `Animation` object. To solve this problem,
the example creates its own `Tween` objects and explicitly calculates the
values.

透過 `sizeAnimation.value` 我們可以得到尺寸，
透過 `opacityAnimation.value` 可以得到不透明度，
但是 `AnimatedWidget` 的建構函式唯讀取單一的 `Animation` 物件。
為了解決這個問題，該範例建立了一個 `Tween` 物件並計算確切值。

Change `AnimatedLogo` to encapsulate its own `Tween` objects,
and its `build()` method calls `Tween.evaluate()`
on the parent's animation object to calculate
the required size and opacity values.
The following code shows the changes with highlights:

修改 `AnimatedLogo` 來封裝其 `Tween` 物件，
以及其 `build()` 方法在母動畫物件上呼叫
`Tween.evaluate()` 來計算所需的尺寸和不透明度值。
下面的程式碼中將這些改動突出顯示：

<?code-excerpt "animate5/lib/main.dart (diff)" replace="/(static final|child: Opacity|opacity:|_sizeTween\.|CurvedAnimation).*/[!$&!]/g"?>
```dart
class AnimatedLogo extends AnimatedWidget {
  const AnimatedLogo({super.key, required Animation<double> animation})
      : super(listenable: animation);

  // Make the Tweens static because they don't change.
  [!static final _opacityTween = Tween<double>(begin: 0.1, end: 1);!]
  [!static final _sizeTween = Tween<double>(begin: 0, end: 300);!]

  @override
  Widget build(BuildContext context) {
    final animation = listenable as Animation<double>;
    return Center(
      [!child: Opacity(!]
        [!opacity: _opacityTween.evaluate(animation),!]
        child: Container(
          margin: const EdgeInsets.symmetric(vertical: 10),
          height: [!_sizeTween.evaluate(animation),!]
          width: [!_sizeTween.evaluate(animation),!]
          child: const FlutterLogo(),
        ),
      ),
    );
  }
}

class LogoApp extends StatefulWidget {
  const LogoApp({super.key});

  @override
  State<LogoApp> createState() => _LogoAppState();
}

class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
  late Animation<double> animation;
  late AnimationController controller;

  @override
  void initState() {
    super.initState();
    controller =
        AnimationController(duration: const Duration(seconds: 2), vsync: this);
    animation = [!CurvedAnimation(parent: controller, curve: Curves.easeIn)!]
      ..addStatusListener((status) {
        if (status == AnimationStatus.completed) {
          controller.reverse();
        } else if (status == AnimationStatus.dismissed) {
          controller.forward();
        }
      });
    controller.forward();
  }

  @override
  Widget build(BuildContext context) => AnimatedLogo(animation: animation);

  @override
  void dispose() {
    controller.dispose();
    super.dispose();
  }
}
```

**App source:** [animate5][]

**原始碼：** [animate5][]

## Next steps

## 下面的步驟

This tutorial gives you a foundation for creating animations in
Flutter using `Tweens`, but there are many other classes to explore.
You might investigate the specialized `Tween` classes,
animations specific to Material Design,
`ReverseAnimation`,
shared element transitions (also known as Hero animations),
physics simulations and `fling()` methods.
See the [animations landing page][]
for the latest available documents and examples.

本指南是在 Flutter 中應用 `Tweens` 建立動畫的基礎介紹，
還有很多其他類可供探索。
比如指定 `Tween` 類，Material Design 特有的動畫，
`ReverseAnimation`，共享元素過渡（也稱為 Hero 動畫），
物理模擬和 `fling()` 方法。
關於最新的文件和範例可參見 [動畫效果介紹][animations landing page]。

[animate0]: {{examples}}/animation/animate0
[animate1]: {{examples}}/animation/animate1
[animate2]: {{examples}}/animation/animate2
[animate3]: {{examples}}/animation/animate3
[animate4]: {{examples}}/animation/animate4
[animate5]: {{examples}}/animation/animate5
[`AnimatedWidget`]: {{site.api}}/flutter/widgets/AnimatedWidget-class.html
[`Animatable`]: {{site.api}}/flutter/animation/Animatable-class.html
[`Animation`]: {{site.api}}/flutter/animation/Animation-class.html
[`AnimatedBuilder`]: {{site.api}}/flutter/widgets/AnimatedBuilder-class.html
[animations landing page]: {{site.url}}/ui/animations
[`AnimationController`]: {{site.api}}/flutter/animation/AnimationController-class.html
[`AnimationController` section]: #animationcontroller
[`Curves`]: {{site.api}}/flutter/animation/Curves-class.html
[`CurvedAnimation`]: {{site.api}}/flutter/animation/CurvedAnimation-class.html
[Cascade notation]: {{site.dart-site}}/language/operators#cascade-notation
[Dart language documentation]: {{site.dart-site}}/language
[`evaluate(Animation<double> animation)`]: {{site.api}}/flutter/animation/Animation/value.html
[`FadeTransition`]: {{site.api}}/flutter/widgets/FadeTransition-class.html
[Monitoring the progress of the animation]: #monitoring
[Refactoring with AnimatedBuilder]: #refactoring-with-animatedbuilder
[`RepaintBoundary`]: {{site.api}}/flutter/widgets/RepaintBoundary-class.html
[`SlideTransition`]: {{site.api}}/flutter/widgets/SlideTransition-class.html
[Simplifying with AnimatedWidget]: #simplifying-with-animatedwidget
[`SizeTransition`]: {{site.api}}/flutter/widgets/SizeTransition-class.html
[`Tween`]: {{site.api}}/flutter/animation/Tween-class.html
