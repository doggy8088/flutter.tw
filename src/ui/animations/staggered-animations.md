---
title: Staggered animations
title: 交織動畫
description: How to write a staggered animation in Flutter.
description: 如何在 Flutter 中編寫一個交織動畫。
short-title: Staggered
short-title: 交織動畫
tags: 使用者介面,Flutter UI,動畫
keywords: 交織動畫
---

{{site.alert.secondary}}
  <h4 class="no_toc">What you'll learn</h4>

  * A staggered animation consists of sequential or overlapping
    animations.
    
    一個交織動畫由一組序列動畫或重疊動畫所組成。
    
  * To create a staggered animation, use multiple `Animation` objects.
    
    建立一個交織動畫，要用到多個動畫物件
  
  * One `AnimationController` controls all of the `Animation`s.
  
    一個 AnimationController 控制所有動畫。
  
  * Each `Animation` object specifies the animation during an `Interval`.
  
    每個動畫物件在一個間隔時間內指定一個動畫。
    
  * For each property being animated, create a `Tween`.
  
    為每一個要執行動畫的屬性建立一個 Tween
    
{{site.alert.end}}

{{site.alert.secondary}}

  **Terminology:**
  If the concept of tweens or tweening is new to you, see the
  [Animations in Flutter tutorial][].
  
  **術語:**
  如果 tweens 或 tweening 的概念對你來說比較新，請看
  [Flutter 指南中的 Animation][Animations in Flutter tutorial]。
{{site.alert.end}}

Staggered animations are a straightforward concept: visual changes
happen as a series of operations, rather than all at once.
The animation might be purely sequential, with one change occurring after
the next, or it might partially or completely overlap. It might also
have gaps, where no changes occur.

交織動畫是一個簡單的概念：視覺變化是隨著一系列的動作發生，而不是一次性的動作。
動畫可能是純粹順序的，一個改變隨著一個改變發生，
動畫也可能是部分或者全部重疊的。動畫也可能有間隙，沒有變化發生。

This guide shows how to build a staggered animation in Flutter.

本指南展示如何在Flutter中建構交織動畫。

{{site.alert.secondary}}

  <h4 class="no_toc">Examples</h4>
  
  <h4 class="no_toc">例子</h4>

  This guide explains the basic_staggered_animation example.
  You can also refer to a more complex example,
  staggered_pic_selection.

  本指南主要介紹 basic_staggered_animation 範例，您也可參考更復雜的例子：staggered_pic_selection。

  [basic_staggered_animation][]
  <br> Shows a series of sequential and overlapping animations
    of a single widget. Tapping the screen begins an animation
    that changes opacity, size, shape, color, and padding.
    
  [basic_staggered_animation][]
  <br> 展示一個單獨的 widget 的一系列連續和重疊動畫。
  輕觸螢幕開始一個動畫，改變不透明度，大小，形狀、顏色和填充。
  
  [staggered_pic_selection][]
  <br> Shows deleting an image from a list of images displayed
    in one of three sizes. This example uses two
    [animation controllers][]: one for image selection/deselection,
    and one for image deletion. The selection/deselection
    animation is staggered. (To see this effect,
    you might need to increase the `timeDilation` value.)
    Select one of the largest images&mdash;it shrinks as it
    displays a checkmark inside a blue circle.
    Next, select one of the smallest images&mdash;the
    large image expands as the checkmark disappears.
    Before the large image has finished expanding,
    the small image shrinks to display its checkmark.
    This staggered behavior is similar to what you might
    see in Google Photos.
    
  [staggered_pic_selection][]
  <br> 展示從一個以三種大小顯示的圖像列表中刪除一個圖像。
    這個例子使用了兩個 [動畫控制器][animation controllers]：
    一個用於控制圖像的選擇/取消選擇, 一個用於控制刪除圖像.
    選擇/取消選擇的動畫是交織動畫。
    (想看到這種效果，你可能需要增加 `timeDilation` 的數值。)
    選擇最大的一個圖像，它會收縮同時在一個在藍色圓圈裡顯示一個對勾，
    然後，選擇一個最小的圖像，最大的圖像會展開同時對勾消失。
    在最大的圖像結束展開前，最小的圖像會收縮並顯示對勾。
    這個交織行為比較類似於你在 Google Photos 中看到的效果。
{{site.alert.end}}

The following video demonstrates the animation performed by
basic_staggered_animation:

以下影片示範了 basic_staggered_animation 所執行的動畫：

<div class="embedded-video-wrapper">
  <iframe class="embedded-video-wrapper__frame"
    src="//player.bilibili.com/player.html?aid=55796337&cid=97540547&page=1&autoplay=false"
    frameborder="0" allowfullscreen>
  </iframe>
</div>

In the video, you see the following animation of a single widget,
which begins as a bordered blue square with slightly rounded corners.
The square runs through changes in the following order:

在這個影片中，你可以看到一個獨立的 widget 的以下動畫，
以一個帶邊框的略微有圓角的藍色矩形開始，
這個矩形會按照以下順序變化：

1. Fades in

   淡出

1. Widens

   擴大

1. Becomes taller while moving upwards

   向上移動同時變得更高

1. Transforms into a bordered circle

   變為一個有邊框的圓圈

1. Changes color to orange

   顏色變為橙色

After running forward, the animation runs in reverse.

向前執行之後， 動畫將反向執行。

{{site.alert.secondary}}

  **New to Flutter?**
  This page assumes you know how to create a layout using Flutter's
  widgets.  For more information, see [Building Layouts in Flutter][].
  
  **剛接觸Flutter？**
  本文假定你已經知道如何使用 Flutter 的 widgets 建立一個佈局。
  更多資訊請看 [Flutter 中的佈局][Building Layouts in Flutter].
  
{{site.alert.end}}

## Basic structure of a staggered animation

## 一個交織動畫的基礎結構

{{site.alert.secondary}}
  <h4 class="no_toc">What's the point?</h4>
  
  <h4 class="no_toc">重點是什麼？</h4>

  * All of the animations are driven by the same
    [`AnimationController`][].

    所有的動畫都是由相同同樣的 [`AnimationController`][] 驅動。

  * Regardless of how long the animation lasts in real time,
    the controller's values must be between 0.0 and 1.0, inclusive.

    無論動畫在真即時間中播放多長時間，控制器的值必須在 0.0 和 1.0 之間，包括 0.0 和 1.0。

  * Each animation has an [`Interval`][]
    between 0.0 and 1.0, inclusive.

    每個動畫都有一個 [`Interval`][]，
    值必須在 0.0 和 1.0 之間，包括 0.0 和 1.0。

  * For each property that animates in an interval, create a
    [`Tween`][]. The `Tween` specifies the start and end
    values for that property.

    對於每一個間隔內產生動畫的屬性，建立一個 [`Tween`][]。
    `Tween` 可以指定該屬性的開始值和結束值。 

  * The `Tween` produces an [`Animation`][]
    object that is managed by the controller.

    `Tween` 產生一個由控制器管理的 [`Animation`][] 物件。

{{site.alert.end}}

{% comment %}
The app is essentially animating a `Container` whose
decoration and size are animated. The `Container`
is within another `Container` whose padding moves the
inner container around and an `Opacity` widget that's
used to fade everything in and out.
{% endcomment %}

The following diagram shows the `Interval`s used in the
[basic_staggered_animation][] example.
You might notice the following characteristics:

下圖展示了在 [basic_staggered_animation]({{site.github}}/flutter/website/tree/master/examples/_animation/basic_staggered_animation) 使用間隔的例子。
你會注意到有以下特點：

* The opacity changes during the first 10% of the timeline.

  透明度在時間軸的前 10% 發生變化。

* A tiny gap occurs between the change in opacity,
  and the change in width.

  透明度的變化和寬度的變化之間有一個很小的間隔。

* Nothing animates during the last 25% of the timeline.

  在時間軸的最後 25% 沒有動畫。

* Increasing the padding makes the widget appear to rise upward.

  增加填充使 widget 看起來向上上升。

* Increasing the border radius to 0.5,
  transforms the square with rounded corners into a circle.
  
  將圓角半徑增加到 0.5，將圓角正方形變成一個圓。
  
* The padding and height changes occur during
  the same exact interval, but they don't have to.
  
  填充和高度的變化發生在相同的時間間隔內，但它們不必這麼做。

![Diagram showing the interval specified for each motion]({{site.url}}/assets/images/docs/ui/animations/StaggeredAnimationIntervals.png)

To set up the animation:

設定這個動畫：

* Create an `AnimationController` that manages all of the
  `Animations`.

  建立一個 `AnimationController` 管理所有的 `Animations`。

* Create a `Tween` for each property being animated.

  為每一個有動畫的屬性建立一個 Tween 
  
  * The `Tween` defines a range of values.
  
    Tween 定義一個值的範圍。
  
  * The `Tween`'s `animate` method requires the
    `parent` controller, and produces an `Animation`
    for that property.
    
    Tween 的 `animate` 方法需要 `parent` 控制器。同時產生一個動畫為這個屬性。
    
* Specify the interval on the `Animation`'s `curve` property.

  指定動畫的 “curve” 屬性的間隔

When the controlling animation's value changes,
the new animation's value changes, triggering the UI to update.

當控制動畫的值發生變化時，新動畫的值也隨之變化值更改，觸發 UI 更新。

The following code creates a tween for the `width` property.
It builds a [`CurvedAnimation`][],
specifying an eased curve. See [`Curves`][] for
other available pre-defined animation curves.

下面的程式碼為 `width` 屬性建立了一個 tween。
它建立了一個 [`CurvedAnimation`][], 指定一個 eased curve。
其他更多的預定的動畫曲線請看 [`Curves`][]。

```dart
width = Tween<double>(
  begin: 50.0,
  end: 150.0,
).animate(
  CurvedAnimation(
    parent: controller,
    curve: const Interval(
      0.125,
      0.250,
      curve: Curves.ease,
    ),
  ),
),
```

The `begin` and `end` values don't have to be doubles.

`begin` 和 `end` 的值不一定是 doubles。

The following code builds the tween for the `borderRadius` property
(which controls the roundness of the square's corners),
using `BorderRadius.circular()`.

下面的程式碼為 `borderRadius` 屬性建立一個 tween（控制矩形的圓角半徑），使用 `BorderRadius.circular()`。

```dart
borderRadius = BorderRadiusTween(
  begin: BorderRadius.circular(4),
  end: BorderRadius.circular(75),
).animate(
  CurvedAnimation(
    parent: controller,
    curve: const Interval(
      0.375,
      0.500,
      curve: Curves.ease,
    ),
  ),
),
```

### Complete staggered animation

### 完整的交織動畫

Like all interactive widgets, the complete animation consists
of a widget pair: a stateless and a stateful widget.

像所有可互動的 widgets 一樣，完整的動畫包括一對 widget：一個無狀態 widget 和一個有狀態的 widget。

The stateless widget specifies the `Tween`s,
defines the `Animation` objects, and provides a `build()` function
responsible for building the animating portion of the widget tree.

無狀態 widget 指定 Tweens，定義動畫物件，提供一個 `build()` 方法，負責建構 widget 樹的動畫部分。


The stateful widget creates the controller, plays the animation,
and builds the non-animating portion of the widget tree.
The animation begins when a tap is detected anywhere in the screen.

有狀態 widget 建立控制器，播放動畫， 同時建構 widget 樹的非動畫部分。
當在螢幕上檢測到一個點選時，動畫開始。

[Full code for basic_staggered_animation's main.dart][]

### Stateless widget: StaggerAnimation

### 無狀態的 widget: StaggerAnimation

In the stateless widget, `StaggerAnimation`,
the `build()` function instantiates an
[`AnimatedBuilder`][]&mdash;a general purpose widget for building
animations. The `AnimatedBuilder`
builds a widget and configures it using the `Tweens`' current values.
The example creates a function named `_buildAnimation()` (which performs
the actual UI updates), and assigns it to its `builder` property.
AnimatedBuilder listens to notifications from the animation controller,
marking the widget tree dirty as values change.
For each tick of the animation, the values are updated,
resulting in a call to `_buildAnimation()`.

在無狀態 widget 中，`StaggerAnimation`，the `build()` 函式例項化了一個
[`AnimatedBuilder`][]&mdash;一個用於建構動畫的通用 widget。
`AnimatedBuilder` 建構一個 widget 並使用 Tweens 的當前值配置它。
這個例子建立一個名為 `_buildAnimation()` （實際更新 UI）的方法，
並將其分配給其 `builder` 屬性。`AnimatedBuilder` 監聽來自動畫控制器的通知，
當值發生更改時，將 widget 樹標記為 dirty。
對於動畫的每一個標記，值都會更新，導致呼叫 `_buildAnimation()`。


{% prettify dart %}
[!class StaggerAnimation extends StatelessWidget!] {
  StaggerAnimation({super.key, required this.controller}) :

    // Each animation defined here transforms its value during the subset
    // of the controller's duration defined by the animation's interval.
    // For example the opacity animation transforms its value during
    // the first 10% of the controller's duration.

    [!opacity = Tween<double>!](
      begin: 0.0,
      end: 1.0,
    ).animate(
      CurvedAnimation(
        parent: controller,
        curve: const Interval(
          0.0,
          0.100,
          curve: Curves.ease,
        ),
      ),
    ),

    // ... Other tween definitions ...
    );

  [!final AnimationController controller;!]
  [!final Animation<double> opacity;!]
  [!final Animation<double> width;!]
  [!final Animation<double> height;!]
  [!final Animation<EdgeInsets> padding;!]
  [!final Animation<BorderRadius?> borderRadius;!]
  [!final Animation<Color?> color;!]

  // This function is called each time the controller "ticks" a new frame.
  // When it runs, all of the animation's values will have been
  // updated to reflect the controller's current value.
  [!Widget _buildAnimation(BuildContext context, Widget? child)!] {
    return Container(
      padding: padding.value,
      alignment: Alignment.bottomCenter,
      child: Opacity(
        opacity: opacity.value,
        child: Container(
          width: width.value,
          height: height.value,
          decoration: BoxDecoration(
            color: color.value,
            border: Border.all(
              color: Colors.indigo[300]!,
              width: 3,
            ),
            borderRadius: borderRadius.value,
          ),
        ),
      ),
    );
  }

  @override
  [!Widget build(BuildContext context)!] {
    return [!AnimatedBuilder!](
      [!builder: _buildAnimation!],
      animation: controller,
    );
  }
}
{% endprettify %}

### Stateful widget: StaggerDemo

### 有狀態的 widget: StaggerDemo

The stateful widget, `StaggerDemo`, creates the `AnimationController`
(the one who rules them all), specifying a 2000 ms duration. It plays
the animation, and builds the non-animating portion of the widget tree.
The animation begins when a tap is detected in the screen.
The animation runs forward, then backward.

有狀態的 widget, StaggerDemo， 建立 AnimationController（控制所有動畫的控制器），
設定一個 2000 毫秒的週期。控制器播放一個動畫，然後在 widget 樹上建立一個無動畫的部分。
當在螢幕上檢測到一個點選時，動畫開始。動畫向前執行，然後向後執行。

{% prettify dart %}
[!class StaggerDemo extends StatefulWidget!] {
  @override
  State<StaggerDemo> createState() => _StaggerDemoState();
}

class _StaggerDemoState extends State<StaggerDemo>
    with TickerProviderStateMixin {
  late AnimationController _controller;

  @override
  void initState() {
    super.initState();

    _controller = AnimationController(
      duration: const Duration(milliseconds: 2000),
      vsync: this,
    );
  }

  // ...Boilerplate...

  [!Future<void> _playAnimation() async!] {
    try {
      [!await _controller.forward().orCancel;!]
      [!await _controller.reverse().orCancel;!]
    } on TickerCanceled {
      // The animation got canceled, probably because it was disposed of.
    }
  }

  @override
  [!Widget build(BuildContext context)!] {
    timeDilation = 10.0; // 1.0 is normal animation speed.
    return Scaffold(
      appBar: AppBar(
        title: const Text('Staggered Animation'),
      ),
      body: GestureDetector(
        behavior: HitTestBehavior.opaque,
        onTap: () {
          _playAnimation();
        },
        child: Center(
          child: Container(
            width: 300,
            height: 300,
            decoration: BoxDecoration(
              color: Colors.black.withOpacity(0.1),
              border: Border.all(
                color:  Colors.black.withOpacity(0.5),
              ),
            ),
            child: StaggerAnimation(controller: _controller.view),
          ),
        ),
      ),
    );
  }
}
{% endprettify %}


[`Animation`]: {{site.api}}/flutter/animation/Animation-class.html
[animation controllers]: {{site.api}}/flutter/animation/AnimationController-class.html
[`AnimationController`]: {{site.api}}/flutter/animation/AnimationController-class.html
[`AnimatedBuilder`]: {{site.api}}/flutter/widgets/AnimatedBuilder-class.html
[Animations in Flutter tutorial]: {{site.url}}/ui/animations/tutorial
[basic_staggered_animation]: {{site.repo.this}}/tree/{{site.branch}}/examples/_animation/basic_staggered_animation
[Building Layouts in Flutter]: {{site.url}}/ui/layout
[staggered_pic_selection]: {{site.repo.this}}/tree/{{site.branch}}/examples/_animation/staggered_pic_selection
[`CurvedAnimation`]: {{site.api}}/flutter/animation/CurvedAnimation-class.html
[`Curves`]: {{site.api}}/flutter/animation/Curves-class.html
[flutter_sequence_animation]: {{site.pub}}/packages/flutter_sequence_animation
[Full code for basic_staggered_animation's main.dart]: {{site.repo.this}}/tree/{{site.branch}}/examples/_animation/basic_staggered_animation/main.dart
[`Interval`]: {{site.api}}/flutter/animation/Interval-class.html
[Material motion spec]: {{site.material}}/styles/motion/overview
[pub.dev]: {{site.pub}}/packages
[`Tween`]: {{site.api}}/flutter/animation/Tween-class.html
