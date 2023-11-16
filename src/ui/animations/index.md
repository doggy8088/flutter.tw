---
title: Introduction to animations
title: 動畫效果介紹
short-title: Animations
short-title: 動畫
description: How to perform animations in Flutter.
description: 如何使用 Flutter 實現動畫效果。
tags: 使用者介面,Flutter UI,動畫
keywords: 動畫效果實現
---

Well-designed animations make a UI feel more intuitive,
contribute to the slick look and feel of a polished app,
and improve the user experience.
Flutter's animation support makes it easy to implement a variety of
animation types. Many widgets, especially [Material widgets][],
come with the standard motion effects defined in their design spec,
but it's also possible to customize these effects.

精心設計的動畫會使 UI 更生動，
它有助於提升應用程式更精巧的外觀和感覺，
從而改善使用者體驗。
Flutter 讓各種動畫效果的實現變得容易。
在許多 widget 特別是 [Material widgets][] 中，
它們都自帶其設計規範中定義的標準動畫效果，
當然，你也可以客製這些效果。

## Choosing an approach

## 選擇一種實現方式

There are different approaches you can take when creating
animations in Flutter. Which approach is right for you?
To help you decide, check out the video,
[How to choose which Flutter Animation Widget is right for you?][]
(Also published as a [_companion article_][article1].)

在 Flutter 中建立動畫可以有多種不同實現方式。
那麼，究竟哪種才是最適合你的呢？
為了幫助你更好的理解它，你可以觀看下面的影片，
[如何在 Flutter 中選擇合適的動畫 Widget][How to choose which Flutter Animation Widget is right for you?]
（同時也釋出了一篇 [配套文章][article1]。）

<center><iframe width="560" height="315" src="{{site.youtube-site}}/embed/GXIJJkq_H8g" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

(To dive deeper into the decision process,
watch the [Animations in Flutter done right][] video,
presented at Flutter Europe.)

（若想要深入瞭解它的決策流程，
請觀看在 Flutter Europe 社群帳號釋出的
[在 Flutter 中使用動畫的正確選擇][Animations in Flutter done right]）影片。

As shown in the video, the following
decision tree helps you decide what approach
to use when implementing a Flutter animation:

正如影片中說的那樣，
下面的決策樹將幫助你挑選實現 Flutter 動畫的正確方式：

<img src='/assets/images/docs/ui/animations/animation-decision-tree.png'
    alt="The animation decision tree" class="mw-100">

If a pre-packaged implicit animation (the easiest animation
to implement) suits your needs, watch
[Animation basics with implicit animations][].
(Also published as a [_companion article_][article2].)

如果內建的隱含動畫（最簡單的動畫）已經能夠滿足你的需求，
請觀看 [隱含動畫基礎][Animation basics with implicit animations]。
（同時也釋出了一篇 [配套文章][article2]。）

<center><iframe width="560" height="315" src="{{site.youtube-site}}/embed/IVTjpW3W33s" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

To create a custom implicit animation, watch
[Creating your own custom implicit animations with TweenAnimationBuilder][].
(Also published as a [_companion article_][article3].)

要建立一個自訂的隱含動畫，請觀看
[使用 TweenAnimationBuilder 建立獨特的隱含動畫][Creating your own custom implicit animations with TweenAnimationBuilder]。
（同時也釋出了一篇 [配套文章][article3]。）

<center><iframe width="560" height="315" src="{{site.youtube-site}}/embed/6KiPEqzJIKQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

To create an explicit animation (where you control the animation,
rather than letting the framework control it), perhaps
you can use one of the built-in explicit animations classes.
For more information, watch
[Making your first directional animations with
built-in explicit animations][].
(Also published as a [_companion article_][article4].)

要建立顯式動畫（手動控制，而不是讓框架控制），
你可以使用內建的其中一個顯式動畫類來實現。更多有關資訊，請觀看
[使用內建顯式動畫][Making your first directional animations with
built-in explicit animations]。
（同時也釋出了一篇 [配套文章][article4]。）

<center><iframe width="560" height="315" src="{{site.youtube-site}}/embed/CunyH6unILQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

If you need to build an explicit animation from scratch, watch
[Creating custom explicit animations with
AnimatedBuilder and AnimatedWidget][].
(Also published as a [_companion article_][article5].)

如果你需要從頭開始建構顯式動畫，請觀看
[透過 AnimatedBuilder 和 AnimatedWidget 建立一個自訂動畫][Creating custom explicit animations with AnimatedBuilder and AnimatedWidget]。
（同時也釋出了一篇[配套文章][article5]。）

<center><iframe width="560" height="315" src="{{site.youtube-site}}/embed/fneC7t4R_B0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

For a deeper understanding of just how animations work in Flutter, watch
[Animation deep dive][].
(Also published as a [_companion article_][article6].)

想要更深入的理解動畫在 Flutter 中的工作方式，
請觀看 [深入理解動畫][Animation deep dive]。
（同時也釋出了一篇 [配套文章][article6]。）

<center><iframe width="560" height="315" src="{{site.youtube-site}}/embed/PbcILiN8rbo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

## Codelabs, tutorials, and articles 

## Codelabs, 課程和文章

The following resources are a good place to start learning
the Flutter animation framework. Each of these documents
shows how to write animation code.

透過下面的資源可以很好的學習 Flutter 動畫框架。這些文件循序漸進地講解如何編寫動畫程式碼。

* [Implicit animations codelab][]<br>
  Covers how to use implicit animations
  using step-by-step instructions and interactive examples.

  [隱含動畫 codelab][Implicit animations codelab]<br>
  涵蓋了如何使用隱含動畫的分步說明及互動範例。

* [Animations tutorial][]<br>
  Explains the fundamental classes in the Flutter animation package
  (controllers, Animatable, curves, listeners, builders),
  as it guides you through a progression of tween animations using
  different aspects of the animation APIs.

  [動畫課程][Animations tutorial]<br>
  闡釋了 Flutter 動畫套件中的基本類（控制器，動畫，曲線，監聽器，建構器），
  這些可以幫助您使用不同的動畫 APIs 完成補間動畫。

* [Zero to One with Flutter, part 1][] and [part 2][]<br>
  Medium articles showing how to create an animated chart using tweening.

  [使用 Flutter 從零到一, 第一部分][Zero to One with Flutter, part 1] 和 [第二部分][part 2]<br>
  Medium 文章中有介紹如何使用補間動畫建立圖表動畫。

* [Write your first Flutter app on the web][]<br>
  Codelab demonstrating how to create a form
  that uses animation to show the user's progress
  as they fill in the fields.

  [撰寫你的第一個 Flutter Web 應用][Write your first Flutter app on the web]<br>
  Codelab 示範如何建構一個根據使用者填寫的內容以動畫展示進度的表單。

## Animation types

Generally, animations are either tween- or physics-based.
The following sections explain what these terms mean,
and point you to resources where you can learn more.

動畫分為兩類：補間動畫和基於物理動畫。下面將解釋這些術語的含義，
並幫助您找到更多相關資源。在一些情況下，
我們現有的最佳文件是 Flutter gallery 中的範例程式碼。

### Tween animation

### 補間動畫

Short for _in-betweening_. In a tween animation, the beginning
and ending points are defined, as well as a timeline, and a curve
that defines the timing and speed of the transition.
The framework calculates how to transition from the beginning point
to the end point.

補間動畫是“介於兩者之間”的縮寫。
在補間動畫中，定義了起點和終點以及時間軸，
再定義過渡時間和速度的曲線。然後框架會計算如何從起點過渡到終點。

The documents listed above, such as the [animations
tutorial][] are not about tweening,
specifically, but they use tweens in their examples.

上文列出的文件，比如 
[在 Flutter 應用裡實現動畫效果][animations tutorial] 
並不是特別針對補間動畫的，但是其範例中使用了補間動畫。

### Physics-based animation

### 基於物理基礎的動畫

In physics-based animation, motion is modeled to resemble real-world
behavior. When you toss a ball, for example, where and when it lands
depends on how fast it was tossed and how far it was from the ground.
Similarly, dropping a ball attached to a spring falls
(and bounces) differently than dropping a ball attached to a string.

在基於物理基礎的動畫中，動作是模擬真實世界的行為來進行建模的。
舉個例子，當您拋球時，球落地的時間和位置取決於丟擲的速度和距離地面的高度。
類似地，附在彈簧上的球和附在繩子上的球掉落（和反彈）方式是不一樣的。

* [Animate a widget using a physics simulation][]<br>
  A recipe in the animations section of the Flutter cookbook.

  [使用物理模擬動畫效果][Animate a widget using a physics simulation]<br>
  在 Flutter cookbook 中的動畫課程。

* [Flutter Gallery][]<br>
  Under **Material Components**, the [`Grid`][] example
  demonstrates a fling animation. Select one of the
  images from the grid and zoom in. You can pan the
  image with flinging or dragging gestures.

  [Flutter Gallery][]<br>
  在 **Material 元件** 下，[`Grid`][] 範例示範了一個拋物動畫。
  從網格中選取一個圖像並放大。您可以透過使用投擲和拖動來平移圖像。

* Also see the API documentation for
  [`AnimationController.animateWith`][] and
  [`SpringSimulation`][].

  請參考 API 文件 [`AnimationController.animateWith`][]
  和 [`SpringSimulation`][]。

## Pre-canned animations

## 預置動畫

If you are using Material widgets, you might check
out the [animations package][] available on pub.dev.
This package contains pre-built animations for
the following commonly used patterns:
`Container` transforms, shared axis transitions,
fade through transitions, and fade transitions.

如果你在使用 Material widgets，
你也許想要看看 pub.dev 上的 [animations package][]。
這個 package 包含了以下內建常用模式：
`Container` 變換、共享軸變化、漸變穿透和漸變變換。

## Common animation patterns

## 常見動畫模式

Most UX or motion designers find that certain
animation patterns are used repeatedly when designing a UI.
This section lists some of the commonly
used animation patterns, and tells you where to learn more.

大多數 UX 或 動效設計師在設計 UI 時都會尋找主要動畫模式。
本章的列表將介紹一些常見的動畫模式，並向你介紹更多學習它們的地方。

### Animated list or grid

### 列表或網格動畫

This pattern involves animating the addition or removal of
elements from a list or grid.

這種模式用於在列表或網格中新增或刪除元素。

* [`AnimatedList` example][]<br>
  This demo, from the [Sample app catalog][], shows how to
  animate adding an element to a list, or removing a selected element.
  The internal Dart list is synced as the user modifies the list using
  the plus (+) and minus (-) buttons.

  [`AnimatedList` example][]<br>
  這個來自 [Sample app catalog][] 的示範展示了
  如何動態新增元素至列表或刪除選定元素。
  當用戶使用 plus (+) 和 minus (-) 按鈕修改列表時，
  會同步到內部 Dart 列表。

### Shared element transition

### 共享元素轉換

In this pattern, the user selects an element&mdash;often an
image&mdash;from the page, and the UI animates the selected element
to a new page with more detail. In Flutter, you can easily implement
shared element transitions between routes (pages)
using the `Hero` widget.

在這個模式中，使用者從頁面中選擇一個元素，通常是圖像，
然後 UI 會在新頁面中為指定元素新增動畫，並產生更多細節。
在 Flutter 中，您可以透過 Hero widget
輕鬆實現路徑（頁面）間的共享元素轉換動畫。

* [Hero animations][]
  How to create two styles of Hero animations:

  [Hero animations][] 如何建立兩種風格的 Hero 動畫：

  * The hero flies from one page to another while changing position
    and size.
    
    當改變位置和大小時，Hero 從一頁飛至另一頁。
    
  * The hero's boundary changes shape, from a circle to a square,
    as its flies from one page to another.
    
    Hero 的邊界改變形狀由圓變方，同時從一頁飛至另一頁。

* [Flutter Gallery][]<br>
  You can build the Gallery app yourself,
  or download it from the Play Store. The [Shrine][]
  demo includes an example of a hero animation.

  [Flutter Gallery][]<br>
  您可以自己自己建立 Gallery 應用程式，或者到 Play 商店中下載。
  [Shrine][] 示範中有關於 Hero 動畫的範例。

* Also see the API documentation for the
  [`Hero`][], [`Navigator`][], and [`PageRoute`][] classes.

  另請參閱 API 文件 [`Hero`][]、[`Navigator`][] 和 [`PageRoute`][] 類別。

### Staggered animation

### 交織動畫

Animations that are broken into smaller motions,
where some of the motion is delayed.
The smaller animations might be sequential,
or might partially or completely overlap.

動畫被分解成較小的動作，其中一些動作被延遲。
這些小動畫可以是連續的，也可以部分或完全重疊。

* [Staggered Animations][]

  [交織動畫][Staggered Animations]

{% comment %}
  Save so I can remember how to add it back later.
  <img src="/assets/images/docs/ic_new_releases_black_24px.svg" alt="this doc is new!"> NEW<br>
{% endcomment -%}

## Other resources

## 其他資源

Learn more about Flutter animations at the following links:

以下連結可以瞭解更多 Flutter 動畫：

* [Animation samples][] from the [Sample app catalog][].

  [範例應用目錄][Sample app catalog] 中的 [動畫範例][Animation samples]

* [Animation recipes][] from the Flutter cookbook.

  Flutter 實用課程 (Cookbook) 中的 [動畫課程][Animation recipes]

* [Animation videos][] from the Flutter YouTube channel.

  Flutter 影片頻道中 [動畫相關的影片][Animation videos]

* [Animations: overview][]<br>
  A look at some of the major classes in the
  animations library, and Flutter's animation architecture.

  [動畫概覽][Animations: overview]<br>
  動畫函式庫中主要類簡介，以及 Flutter 動畫結構。

* [Animation and motion widgets][]<br>
  A catalog of some of the animation widgets
  provided in the Flutter APIs.

  [動畫及動作 Widgets][Animation and motion widgets]<br>
  Flutter APIs 提供的動畫 widgets 目錄。

* The [animation library][] in the [Flutter API documentation][]<br>
  The animation API for the Flutter framework. This link
  takes you to a technical overview page for the library.

  [Flutter API 文件][Flutter API documentation] 中的 [動畫函式庫][animation library]<br>
  Flutter 的動畫 API。此連結將帶你進入動畫函式庫的概述頁。

[Animate a widget using a physics simulation]: {{site.url}}/cookbook/animation/physics-simulation
[`AnimatedList` example]: https://flutter.github.io/samples/animations.html
[Animation and motion widgets]: {{site.url}}/ui/widgets/animation
[Animation basics with implicit animations]: {{site.youtube-site}}/watch?v=IVTjpW3W33s&list=PLjxrf2q8roU2v6UqYlt_KPaXlnjbYySua&index=1
[Animation deep dive]: {{site.youtube-site}}/watch?v=PbcILiN8rbo&list=PLjxrf2q8roU2v6UqYlt_KPaXlnjbYySua&index=5
[animation library]: {{site.api}}/flutter/animation/animation-library.html
[Animation recipes]: {{site.url}}/cookbook/animation
[Animation samples]: {{site.github}}/flutter/samples/tree/main/animations#animation-samples
[Animation videos]: {{site.social.youtube}}/search?query=animation
[Animations in Flutter done right]: {{site.youtube-site}}/watch?v=wnARLByOtKA&t=3s
[Animations: overview]: {{site.url}}/ui/animations/overview
[animations package]: {{site.pub}}/packages/animations
[Animations tutorial]: {{site.url}}/ui/animations/tutorial
[`AnimationController.animateWith`]: {{site.api}}/flutter/animation/AnimationController/animateWith.html
[article1]: {{site.flutter-medium}}/how-to-choose-which-flutter-animation-widget-is-right-for-you-79ecfb7e72b5
[article2]: {{site.flutter-medium}}/flutter-animation-basics-with-implicit-animations-95db481c5916
[article3]: {{site.flutter-medium}}/custom-implicit-animations-in-flutter-with-tweenanimationbuilder-c76540b47185
[article4]: {{site.flutter-medium}}/directional-animations-with-built-in-explicit-animations-3e7c5e6fbbd7
[article5]: {{site.flutter-medium}}/when-should-i-useanimatedbuilder-or-animatedwidget-57ecae0959e8
[article6]: {{site.flutter-medium}}/animation-deep-dive-39d3ffea111f
[Creating your own custom implicit animations with TweenAnimationBuilder]: {{site.youtube-site}}/watch?v=6KiPEqzJIKQ&feature=youtu.be
[Creating custom explicit animations with AnimatedBuilder and AnimatedWidget]: {{site.youtube-site}}/watch?v=fneC7t4R_B0&list=PLjxrf2q8roU2v6UqYlt_KPaXlnjbYySua&index=4
[Flutter API documentation]: {{site.api}}
[Flutter Gallery]: {{site.repo.gallery}}
[`Grid`]: {{site.repo.gallery}}/blob/main/lib/demos/material/grid_list_demo.dart
[`Hero`]: {{site.api}}/flutter/widgets/Hero-class.html
[Hero animations]: {{site.url}}/ui/animations/hero-animations
[How to choose which Flutter Animation Widget is right for you?]: {{site.youtube-site}}/watch?v=GXIJJkq_H8g
[Implicit animations codelab]: {{site.url}}/codelabs/implicit-animations
[Making your first directional animations with built-in explicit animations]: {{site.youtube-site}}/watch?v=CunyH6unILQ&list=PLjxrf2q8roU2v6UqYlt_KPaXlnjbYySua&index=3
[Material widgets]: {{site.url}}/ui/widgets/material
[`Navigator`]: {{site.api}}/flutter/widgets/Navigator-class.html
[`PageRoute`]: {{site.api}}/flutter/widgets/PageRoute-class.html
[part 2]: {{site.medium}}/dartlang/zero-to-one-with-flutter-part-two-5aa2f06655cb
[Sample app catalog]: https://flutter.github.io/samples
[Shrine]: {{site.repo.gallery}}/tree/main/lib/studies/shrine
[`SpringSimulation`]: {{site.api}}/flutter/physics/SpringSimulation-class.html
[Staggered Animations]: {{site.url}}/ui/animations/staggered-animations
[Step 7 (Animate your app)]: {{site.codelabs}}/codelabs/flutter/#6
[Write your first Flutter app on the web]: {{site.url}}/get-started/codelab-web
[Zero to One with Flutter, part 1]: {{site.medium}}/dartlang/zero-to-one-with-flutter-43b13fd7b354
