---
title: Hero animations
title: 主動畫 (Hero animations)
description: How to animate a widget to fly between two screens.
description: 如何讓 widget 動畫飛躍兩個螢幕。
short-title: Hero
tags: 使用者介面,Flutter UI,動畫
keywords: 共享元素過渡,補間動畫
---

{{site.alert.secondary}}

  <h4 class="no_toc">What you’ll learn</h4>

  <h4 class="no_toc">你將會在這裡學到：</h4>

  * The _hero_ refers to the widget that flies between screens.
  
    **Hero** 指的是在螢幕間飛躍的 widget。
    
  * Create a hero animation using Flutter's Hero widget.
  
    用 Flutter's Hero widget 建立 hero 動畫。
    
  * Fly the hero from one screen to another.
  
    使 hero 從原頁面飛至新頁面。
    
  * Animate the transformation of a hero's shape from circular to
    rectangular while flying it from one screen to another.
    
    當 hero 從原頁面飛至新頁面時，使 hero 的形狀由圓形動態過渡為正方形。
    
  * The Hero widget in Flutter implements a style of animation
    commonly known as _shared element transitions_ or
    _shared element animations._
    
    Flutter 中的 Hero widget 實現的動畫型別也稱為 **共享元素過渡** 或 **共享元素動畫**。
    
{{site.alert.end}}

You've probably seen hero animations many times. For example, a screen displays
a list of thumbnails representing items for sale.  Selecting an item flies it to
a new screen, containing more details and a "Buy" button. Flying an image from
one screen to another is called a _hero animation_ in Flutter, though the same
motion is sometimes referred to as a _shared element transition_.

你可能經常遇到 hero 動畫。比如，頁面上顯示的代售商品列表。
選擇一件商品後，應用會跳轉至包含更多細節以及“購買”按鈕的新頁面。
在 Flutter 中，影象從當前頁面轉到另一個頁面稱為 **hero 動畫**，
相同的動作有時也被稱為 **共享元素過渡**。

You might want to watch this one-minute video introducing the Hero widget:

下面的一分鐘影片介紹了 Hero widget：

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=55794187&cid=97537277&page=1" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This guide demonstrates how to build standard hero animations, and hero
animations that transform the image from a circular shape to a square shape
during flight.

這個指南示範瞭如何建立標準 hero 動畫，以及 hero 動畫如何在飛行過程中將影象形狀由圓形變成正方形。

{{site.alert.secondary}}

  **Examples**: This guide provides examples of each hero animation style at
  the following links.

  **範例**: 這個指南在下面的連結中為每種型別的 hero 動畫提供範例。

  * [Standard hero animation code][]

    [標準 hero 動畫程式碼][Standard hero animation code]

  * [Radial hero animation code][]

    [徑向 hero 動畫程式碼][Radial hero animation code]

{{site.alert.end}}

{{site.alert.secondary}}

  **New to Flutter?**

  **剛接觸 Flutter？**

  This page assumes you know how to create a layout
  using Flutter’s widgets. For more information, see
  [Building Layouts in Flutter][].

  這部分假定您已經瞭解如何使用 Flutter 的 widget 建立佈局。
  更多資訊請參考文件 [Flutter 中的佈局][Building Layouts in Flutter]。

{{site.alert.end}}

{{site.alert.secondary}}

  **Terminology:**

  **術語：**  

  A [_Route_][] describes a page or screen
  in a Flutter app.

  在 Flutter app 中，[**Route**][_Route_] 用來描述一個頁面。

{{site.alert.end}}

You can create this animation in Flutter with Hero widgets.
As the hero animates from the source to the destination route,
the destination route (minus the hero) fades into view.
Typically, heroes are small parts of the UI, like images,
that both routes have in common. From the user's perspective
the hero "flies" between the routes. This guide shows how
to create the following hero animations:

您可以在 Flutter 中使用 Hero widgets 建立這個動畫。
當 hero 動畫從原頁面到目標頁面，目標頁面（減去 hero）淡入視野。
可以說，heroes 是 UI 的一小部分，就像影象，兩個頁面有共同之處。
從使用者的角度來說，hero 在頁面間「飛翔」。
本指南展示如何建立如下 hero 動畫：

**Standard hero animations**<br>

**標準 hero 動畫**<br>

A _standard hero animation_ flies the hero from one route to a new route,
usually landing at a different location and with a different size.

一個 **標準 hero 動畫** 使 hero 從一頁飛至新頁面，通常以不同大小到達不同的目的地。

The following video (recorded at slow speed) shows a typical example. 
Tapping the flippers in the center of the route flies them to the 
upper left corner of a new, blue route, at a smaller size. 
Tapping the flippers in the blue route (orusing the device's back-to-previous-route gesture) flies the flippers back to
the original route.

下面的影片（慢放）示範了一個典型範例。點選頁面中間的 flippers，
它將飛至一個新的藍色頁面的左上角，並縮小。
點選藍色頁面中的 flippers（或者使用裝置的回到前頁手勢），
它將返回原頁面。

<div class="embedded-video-wrapper">
  <iframe class="embedded-video-wrapper__frame"
    src="//player.bilibili.com/player.html?aid=55796337&cid=97540547&page=3"
    frameborder="0" allowfullscreen>
  </iframe>
</div>

<br>**Radial hero animations**<br>

**徑向 hero 動畫**<br>

In _radial hero animation_, as the hero flies between routes
its shape appears to change from circular to rectangular.

在 **徑向 hero 動畫** 中，隨著 hero 在頁面間飛翔，
它的形狀也會有圓形變成矩形。

The following video (recorded at slow speed),
shows an example of a radial hero animation. At the start, a
row of three circular images appears at the bottom of the route.
Tapping any of the circular images flies that image to a new route
that displays it with a square shape.
Tapping the square image flies the hero back to
the original route, displayed with a circular shape.

下面的影片（慢放）示範了一個徑向 hero 動畫的範例。
開始，一排三個圓形的影象在頁面底部。點選任意圓形影象，
其飛至新頁面，並變成正方形。
點選正方形影象，hero 返回至原頁面，並變回圓形。

<div class="embedded-video-wrapper">
  <iframe class="embedded-video-wrapper__frame"
    src="//player.bilibili.com/player.html?aid=55796337&cid=97540547&page=1"
    frameborder="0" allowfullscreen>
  </iframe>
</div>

<br>Before moving to the sections specific to
[standard](#standard-hero-animations)
or [radial](#radial-hero-animations) hero animations,
read [basic structure of a hero animation](#basic-structure)
to learn how to structure hero animation code,
and [behind the scenes](#behind-the-scenes) to understand
how Flutter performs a hero animation.

在學習 [標準](#standard-hero-animations) 或 
[徑向](#radial-hero-animations) hero 動畫之前，
請閱讀 [hero 動畫基本結構](#basic-structure) 來學習如何建構 hero 動畫程式碼，
以及 [幕後](#behind-the-scenes) 來了解 Flutter 如何顯示一個 hero 動畫。

<a name="basic-structure"></a>
## Basic structure of a hero animation

## hero 動畫基本結構

{{site.alert.secondary}}

  <h4 class="no_toc">What's the point?</h4>

  <h4 class="no_toc">要點列表</h4>

  * Use two hero widgets in different routes but with matching tags to
    implement the animation.

    在不同頁面分別使用兩個 hero widgets，同時使用配對的標籤來實現動畫。

  * The Navigator manages a stack containing the app’s routes.

    Navigator 管理含有 app 頁面的堆疊。

  * Pushing a route on or popping a route from the Navigator’s stack
    triggers the animation.

    推送一個頁面或彈出一個 Navigator 堆疊中的頁面會觸發動畫。

  * The Flutter framework calculates a rectangle tween,
    [`RectTween`][] that defines the hero's boundary
    as it flies from the source to the destination route.
    During its flight, the hero is moved to
    an application overlay, so that it appears on top of both routes.

    Flutter 框架設定了一個 [RectTween 類][`RectTween`]，
    用來定義 hero 從原頁面飛至目標頁面的邊界。
    在飛翔過程中，hero 移動到一個應用圖層，這樣它可以在兩個頁面上方顯示。

{{site.alert.end}}

{{site.alert.secondary}}

  **Terminology:**

  **術語：**

  If the concept of tweens or tweening is new to you, see the
  [Animations in Flutter tutorial][].

  如果您不瞭解 tween 或者 tweening 的概念，請參考課程
  [在 Flutter 應用裡實現動畫效果][Animations in Flutter tutorial]。

{{site.alert.end}}

Hero animations are implemented using two [`Hero`][]
widgets: one describing the widget in the source route,
and another describing the widget in the destination route.
From the user’s point of view, the hero appears to be shared, and
only the programmer needs to understand this implementation detail.

Hero 動畫需要使用兩個 [`Hero`][] widgets 來實現：
一個用來在原頁面中描述 widget，另一個在目標頁面中描述 widget。
從使用者角度來說，hero 似乎是分享的，只有程式設計師需要了解實施細節。

Hero animation code has the following structure:

Hero 動畫程式碼有如下結構：

1. Define a starting Hero widget, referred to as the _source
   hero_. The hero specifies its graphical representation
   (typically an image), and an identifying tag, and is in
   the currently displayed widget tree as defined by the source route.

   定義一個起始 Hero widget，被稱為 **source hero**。
   該 hero 指定圖形表示（通常是影象），以及識別標籤，
   並且在由原頁面定義的當前顯示的 widget 樹中。

1. Define an ending Hero widget, referred to as the _destination hero_.
   This hero also specifies its graphical representation,
   and the same tag as the source hero.
   It's **essential that both hero widgets are created with
   the same tag**, typically an object that represents the
   underlying data. For best results, the heroes should have
   virtually identical widget trees.

   定義一個截至 Hero widget，被稱為 **destination hero**。
   該 hero 也指定圖形表示，並與 source hero 使用同樣的標籤。
   **這是基本，兩個 hero widgets 要建立相同的標籤**，通常是代表基礎資料的物件。
   為了獲得最佳效果，heroes 應該有幾乎完全相同的 widget 樹。

1. Create a route that contains the destination hero.
   The destination route defines the widget tree that exists
   at the end of the animation.

   建立一個含有 destination hero 的頁面。
   目標頁面定義了動畫結束時應有的 widget 樹。

1. Trigger the animation by pushing the destination route on the
   Navigator's stack. The Navigator push and pop operations trigger
   a hero animation for each pair of heroes with matching tags in
   the source and destination routes.

   透過推送目標頁面到 Navigator 堆疊來觸發動畫。
   Navigator 推送並彈出操作觸發原頁面和目標頁面中含有
   配對標籤 heroes 的 hero 動畫。

Flutter calculates the tween that animates the Hero's bounds from
the starting point to the endpoint (interpolating size and position),
and performs the animation in an overlay.

Flutter 設定了 tween 用來界定 Hero
從起點到終點的界限（插入大小和位置），並在圖層上執行動畫。

The next section describes Flutter's process in greater detail.

下一章節將更詳細地介紹 Flutter 的過程。

## Behind the scenes

## 幕後

The following describes how Flutter performs the
transition from one route to another.

下面將介紹 Flutter 如何執行一個頁面到另一頁面的過渡。

<img src='/assets/images/docs/ui/animations/hero-transition-0.png'
    alt="在過渡之前 source hero 出現在原頁面中"
    class="mw-100">

Before transition, the source hero waits in the source 
route's widget tree. The destination route does not yet exist, 
and the overlay is empty.

過渡前，source hero 在原頁面的 widget 樹中等待。
而目標頁面此時並不存在，圖層也是空的。

---

<img src='/assets/images/docs/ui/animations/hero-transition-1.png'
    alt="過渡開始"
    class="mw-100">

Pushing a route to the `Navigator` triggers the animation. 
At t=0.0, Flutter does the following:

推送一個頁面到 Navigator 來觸發動畫。t=0.0 時，Flutter 執行如下動作：

* Calculates the destination hero's path, offscreen,
  using the curved motion as described in the Material
  motion spec. Flutter now knows where the hero ends up.

  使用 Material motion spec 中介紹的曲線運動計算 
  destination hero 路徑，後臺執行。
  Flutter 限制知道 hero 應在何處終止。

* Places the destination hero in the overlay,
  at the same location and size as the _source_ hero.
  Adding a hero to the overlay changes its Z-order so that it
  appears on top of all routes.

  將 destination hero 放到圖層，與 **source** hero 相同的位置和大小。
  新增一個 hero 到圖層改變其 Z 軸的順序，
  這樣才可以出現在所有頁面的上面。

* Moves the source hero offscreen.

  將 source hero 移至後臺執行。

---

<img src='/assets/images/docs/ui/animations/hero-transition-2.png'
    alt="The hero flies in the overlay to its final position and size (hero 飛入圖層到達其最終位置和大小)"
    class="mw-100">

As the hero flies, its rectangular bounds are animated using
[Tween&lt;Rect&gt;][], specified in Hero's
[`createRectTween`][] property.
By default, Flutter uses an instance of
[`MaterialRectArcTween`][], which animates the
rectangle's opposing corners along a curved path.
(See [Radial hero animations][] for an example
that uses a different Tween animation.)

hero 飛翔時，它的矩形邊界使用 Hero 的 
[`createRectTween`][] 屬性中特定的 [Tween&lt;Rect&gt;][] 進行動畫。
預設情況下，Flutter 使用 [`MaterialRectArcTween`][] 的範例，
它沿著一個曲線路徑設定矩形對角動畫。
（參考 [徑向 hero 動畫][Radial hero animations]，
該範例使用了不同的補間動畫）

---

<img src='/assets/images/docs/ui/animations/hero-transition-3.png'
    alt="When the transition is complete, the hero is moved from the overlay to the destination route (當過渡完成時，hero 從圖層移動到目的頁面)"
    class="mw-100">

When the flight completes:

當飛翔完成時：

* Flutter moves the hero widget from the overlay to
  the destination route. The overlay is now empty.

  Flutter 將 hero widget 從圖層移動到目標頁面。圖層現在是空的。

* The destination hero appears in its final position
  in the destination route.

  destination hero 出現在目標圖層的最終位置。

* The source hero is restored to its route.

  source hero 被儲存到原頁面中。

---

Popping the route performs the same process,
animating the hero back to its size
and location in the source route.

彈出的頁面執行同樣的過程，hero 動畫回到原頁面並回復原來大小和位置。

### Essential classes

### 基本類

The examples in this guide use the following classes to
implement hero animations:

本指南中的範例使用瞭如下類來實現 hero 動畫：

[`Hero`][]
<br> The widget that flies from the source to the destination route.
  Define one Hero for the source route and another for the
  destination route, and assign each the same tag.
  Flutter animates pairs of heroes with matching tags.

[`Hero`][]
<br> 從原頁面飛到目標頁面的 widget。
  定義一個原頁面的 Hero 和另一個目標頁面的 Hero，並設定相同的標籤。
  Flutter 為成對的含有匹配標籤的 heroes 設定動畫。

[`Inkwell`][]
<br> Specifies what happens when tapping the hero.
  The InkWell's `onTap()` method builds the new route and pushes it
  to the Navigator's stack.

[`Inkwell`][]
<br> 指定點選 hero 時發生什麼。
  InkWell 的 `onTap()` 方法可以建立新頁面並推送至 Navigator 的堆疊。

[`Navigator`][]
<br> The Navigator manages a stack of routes. Pushing a route on or
  popping a route from the Navigator's stack triggers the animation.

[`Navigator`][]
<br> Navigator 管理一個頁面堆疊。推送或彈出 Navigator 堆疊中的頁面觸發動畫。

[`Route`][]
<br> Specifies a screen or page. Most apps, beyond the most basic,
  have multiple routes.

[`Route`][]
<br> 指定螢幕或頁面。除最基本的應用程式外，大部分含有多頁面。

## Standard hero animations

## 標準 hero 動畫

{{site.alert.secondary}}

  <h4 class="no_toc">What's the point?</h4>

  <h4 class="no_toc">要點</h4>

  * Specify a route using `MaterialPageRoute`, `CupertinoPageRoute`, or
    build a custom route using
    `PageRouteBuilder`. The examples in this section use MaterialPageRoute.
    
    使用 MaterialPageRoute，CupertinoPageRoute 指定頁面，
    或使用 PageRouteBuilder 建立自訂頁面。本章節範例使用的時 MaterialPageRoute。
  
  * Change the size of the image at the end of the transition by
    wrapping the destination's image in a `SizedBox`.
    
    在過渡的最後，透過在 `SizedBox` 中裹挾目標影象來改變影象大小。
    
  * Change the location of the image by placing the destination's
    image in a layout widget. These examples use `Container`.
    
    透過在佈局 widget 中放置目標影象來改變影象位置。這些範例中使用 `Container`。
    
{{site.alert.end}}

<a name="standard-hero-animation-code"></a>

{{site.alert.secondary}}

  **Standard hero animation code**

  **標準 hero 動畫程式碼**

  Each of the following examples demonstrates flying an image from one
  route to another. This guide describes the first example.

  下面的每個範例都示範了一個圖形從一頁面飛至另一頁面。本指南詳細介紹第一個例子。

  [hero_animation][]
  <br> Encapsulates the hero code in a custom PhotoHero widget.
    Animates the hero's motion along a curved path,
    as described in the Material motion spec.

  [hero_animation][]
  <br> 將 hero 程式碼封裝到自訂的 PhotoHero widget 中。
    沿 Material motion spec 中介紹的曲線路徑設定 hero 動作的動畫。

  [basic_hero_animation][]
  <br> Uses the hero widget directly.
    This more basic example, provided for your reference, isn't
    described in this guide.
    
  [basic_hero_animation][]
  <br> 直接使用 hero widget。
    這個基礎範例僅供參考，本指南中無詳細介紹。

{{site.alert.end}}

### What's going on?

### 然後呢？

Flying an image from one route to another is easy to implement
using Flutter's hero widget. When using `MaterialPageRoute`
to specify the new route, the image flies along a curved path,
as described by the [Material Design motion spec][].

使用 Flutter 的 hero widget 可以輕鬆實現影象由一個頁面飛至另一個。
當使用 MaterialPageRoute 指定新頁面時，
影象將沿 [Material Design motion spec][] 中介紹的曲線路徑飛翔。

[Create a new Flutter example][] and
update it using the files from the [hero_animation][].

[建立一個新的 Flutter 範例][Create a new Flutter example]
並使用來自 [hero_animation][] 的檔案更新。

To run the example:

執行範例：

* Tap on the home route’s photo to fly the image to a new route
  showing the same photo at a different location and scale.
  
  點選主頁的圖片使影象飛至新頁面並在不同位置以不同規格顯示相同圖片。

* Return to the previous route by tapping the image, or by using the
  device’s back-to-the-previous-route gesture.
  
  點選影象或使用裝置的回到前頁手勢返回之前頁面。
  
* You can slow the transition further using the `timeDilation`
  property.
  
  可以使用 `timeDilation` 屬性來減緩過渡。

### PhotoHero class

### PhotoHero 類

The custom PhotoHero class maintains the hero, 
and its size, image, and behavior when tapped. 
The PhotoHero builds the following widget tree:

自訂的 PhotoHero 類保留了 hero 以及其大小，影象，和點選時的動作。PhotoHero 建立如下 widget 樹：

<div class="text-center mb-4">
  <img src='/assets/images/docs/ui/animations/photohero-class.png'
      alt="PhotoHero class widget tree"
      class="mw-100">
</div>

Here's the code:

程式碼如下：

{% prettify dart %}
class PhotoHero extends StatelessWidget {
  const PhotoHero({ Key key, this.photo, this.onTap, this.width }) : super(key: key);

  final String photo;
  final VoidCallback onTap;
  final double width;

  Widget build(BuildContext context) {
    return SizedBox(
      width: width,
      child: Hero(
        tag: photo,
        child: Material(
          color: Colors.transparent,
          child: InkWell(
            onTap: onTap,
            child: Image.asset(
              photo,
              fit: BoxFit.contain,
            ),
          ),
        ),
      ),
    );
  }
}
{% endprettify %}

Key information:

重要資訊：

* The starting route is implicitly pushed by `MaterialApp` when
  `HeroAnimation` is provided as the app's home property.
  
  當 `HeroAnimation` 作為應用程式的主頁屬性時，
  起始頁面由 `MaterialApp` 隱含推送。
  
* An `InkWell` wraps the image, making it trivial to add a tap
  gesture to the both the source and destination heroes.
  
  `InkWell` 裹挾影象，使得為 source hero 和
  destination hero 新增點選動作變得簡單。
  
* Defining the Material widget with a transparent color
  enables the image to "pop out" of the background as it
  flies to its destination.

  用透明色定義 Material widget 使圖片在飛至目標頁時可以從背景中“彈出”。

* The `SizedBox` specifies the hero's size at the start and
  end of the animation.
  
  `SizedBox` 指定動畫起始和結束時 hero 的大小。
  
* Setting the Image's `fit` property to `BoxFit.contain`,
  ensures that the image is as large as possible during the
  transition without changing its aspect ratio.
  
  設定影象的 `fit` 屬性到 `BoxFit.contain`，
  可以確保在過渡過程中儘可能放大，且不改變長寬比例。

### HeroAnimation class

### HeroAnimation 類

The `HeroAnimation` class creates the source and destination
PhotoHeroes, and sets up the transition.

HeroAnimation 類可以建立 source PhotoHero 和 destination PhotoHero，並建立過渡。

Here's the code:

程式碼如下：

{% prettify dart %}
class HeroAnimation extends StatelessWidget {
  Widget build(BuildContext context) {
    [[highlight]]timeDilation = 5.0; // 1.0 means normal animation speed.[[/highlight]]

    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic Hero Animation'),
      ),
      body: Center(
        [[highlight]]child: PhotoHero([[/highlight]]
          photo: 'images/flippers-alpha.png',
          width: 300.0,
          [[highlight]]onTap: ()[[/highlight]] {
            [[highlight]]Navigator.of(context).push(MaterialPageRoute<void>([[/highlight]]
              [[highlight]]builder: (BuildContext context)[[/highlight]] {
                return Scaffold(
                  appBar: AppBar(
                    title: const Text('Flippers Page'),
                  ),
                  body: Container(
                    // The blue background emphasizes that it's a new route.
                    color: Colors.lightBlueAccent,
                    padding: const EdgeInsets.all(16.0),
                    alignment: Alignment.topLeft,
                    [[highlight]]child: PhotoHero([[/highlight]]
                      photo: 'images/flippers-alpha.png',
                      width: 100.0,
                      [[highlight]]onTap: ()[[/highlight]] {
                        [[highlight]]Navigator.of(context).pop();[[/highlight]]
                      },
                    ),
                  ),
                );
              }
            ));
          },
        ),
      ),
    );
  }
}
{% endprettify %}

Key information:

重要資訊：

* When the user taps the `InkWell` containing the source hero,
  the code creates the destination route using `MaterialPageRoute`.
  Pushing the destination route to the `Navigator`’s stack triggers
  the animation.
  
  當用戶點選含有 source hero 的 `InkWell` 時，程式碼使用 `MaterialPageRoute` 
  產生目標頁面。並將目標頁面推送至 `Navigator` 堆疊，觸發動畫。
  
* The `Container` positions the `PhotoHero` in the destination
  route's top-left corner, below the `AppBar`.
  
  `Container` 將 `PhotoHero` 置於目標頁面左上角，AppBar 的下方。
  
* The `onTap()` method for the destination `PhotoHero`
  pops the `Navigator`’s stack, triggering the animation
  that flies the `Hero` back to the original route.
  
  目標頁 `PhotoHero` 的 `onTap()` 函式會彈出 `Navigator` 的堆疊，觸發動畫 Hero 飛回至原頁面。
  
* Use the `timeDilation` property to slow the transition
  while debugging.

  在除錯時，可以使用 `timeDilation` 屬性來減緩過渡。

---

## Radial hero animations

## 徑向 hero 動畫

{{site.alert.secondary}}

  <h4 class="no_toc">What's the point?</h4>

  <h4 class="no_toc">要點</h4>

  * A _radial transformation_ animates a circular shape into a square
    shape.
    
    **徑向過渡** 是由圓形變成正方形的過渡動畫。
    
  * A radial _hero_ animation performs a radial transformation while
    flying the hero from the source route to the destination route.
    
    徑向 **hero** 動畫在 hero 從原頁面飛至目標頁面時，執行徑向過渡。
    
  * MaterialRectCenter&shy;Arc&shy;Tween defines the tween animation.
  
    MaterialRectCenter&shy;Arc&shy;Tween 定義了補間動畫。
  
  * Build the destination route using `PageRouteBuilder`.
  
    使用 PageRouteBuilder 建立目標頁。
    
{{site.alert.end}}

Flying a hero from one route to another as it transforms
from a circular shape to a rectangular shape is a slick
effect that you can implement using Hero widgets.
To accomplish this, the code animates the intersection of
two clip shapes: a circle and a square.
Throughout the animation, the circle clip (and the image)
scales from `minRadius` to `maxRadius`, while the square
clip maintains constant size. At the same time,
the image flies from its position in the source route to its
position in the destination route. For visual examples
of this transition, see [Radial transformation][]
in the Material motion spec.

hero 從一個頁面飛至另一頁的同時由圓形過渡到矩形，
這是一個滑入效果，可使用 Hero widgets 來實現。
要做到這一點，程式碼需要動畫兩個剪裁形狀的交叉：
一個圓形和一個正方形。整個動畫中，圓形剪裁（和圖片）由
`minRadius` 縮放到 `maxRadius`，而正方形剪裁保持大小不變。
同時，影象從原頁面飛至目標頁面的相同位置。
這個過渡的效果範例，
請參見 Material motion spec 中的 [徑向過渡][Radial transformation]。

This animation might seem complex (and it is), but you can **customize the
provided example to your needs.** The heavy lifting is done for you.

這個動畫看起來複雜，但是您可以**根據自身需要自訂範例**。
艱鉅的工作已為您完成。

<a name="radial-hero-animation-code"></a>
{{site.alert.secondary}}

  **Radial hero animation code**

  **徑向 hero 動畫程式碼**

  Each of the following examples demonstrates a radial hero animation.
  This guide describes the first example.

  下面的每個範例都示範了一個徑向 hero 動畫。
  本指南詳細介紹第一個範例。

  [radial_hero_animation][]
  <br> A radial hero animation as described in the Material motion spec.

  [radial_hero_animation][]
  <br> Material motion spec 中詳細介紹了這個徑向 hero 動畫。

  [basic_radial_hero_animation][]
  <br> The simplest example of a radial hero animation. The destination
    route has no Scaffold, Card, Column, or Text.
    This basic example, provided for your reference, isn't
    described in this guide.

  [basic_radial_hero_animation][]
  <br> 徑向 hero 動畫最簡單的範例。
    目標頁面沒有 Scaffold, Card, Column, 或 Text。
    這個基本範例僅供參考，本指南不詳述。


  [radial_hero_animation_animate<wbr>_rectclip][]
  <br> Extends radial_hero_animaton by also animating the size of the
    rectangular clip. This more advanced example,
    provided for your reference, isn't described in this guide.
    
  [radial_hero_animation_animate<wbr>_rectclip][]
  <br> 透過動畫矩形剪裁大小，擴充徑向 hero 動畫。
    這個高階範例亦供參考，本指南不詳述。

{{site.alert.end}}

{{site.alert.secondary}}

  **Pro tip:**
  
  **技巧：**
  
  The radial hero animation involves intersecting a round shape with
  a square shape. This can be hard to see, even when slowing
  the animation with `timeDilation`, so you might consider enabling
  the [`debugPaintSizeEnabled`][] flag during development.
  
  徑向 hero 動畫設計圓形和正方形的交叉。
  這個很難看出來，即使使用 `timeDilation` 來減慢動畫。
  所以在開發時，可以考慮啟用 Flutter 的 [`debugPaintSizeEnabled`][] 這個 flag。

{{site.alert.end}}

### What's going on?

### 然後呢？

The following diagram shows the clipped image at the beginning
(`t = 0.0`), and the end (`t = 1.0`) of the animation.

下面的圖表顯示了在動畫起始（`t = 0.0`）和結束（`t = 1.0`）時的剪裁影象。

<img src='/assets/images/docs/ui/animations/radial-hero-animation.png'
    alt="Radial transformation from beginning to end"
    class="mw-100">

The blue gradient (representing the image), indicates where the clip
shapes intersect. At the beginning of the transition,
the result of the intersection is a circular clip ([`ClipOval`][]).
During the transformation, the `ClipOval` scales from `minRadius`
to `maxRadius` while the [ClipRect][] maintains a constant size.
At the end of the transition the intersection of the circular and
rectangular clips yield a rectangle that's the same size as the hero
widget. In other words, at the end of the transition the image is no
longer clipped.

藍色漸變（代表影象），表明剪裁形狀交叉的位置。
在過渡的開始，交叉的結果是圓形剪裁 ([`ClipOval`][])。
在過渡過程中，ClipOval 由 `minRadius` 縮放至 `maxRadius`，
[ClipRect][] 則保持原尺寸。
在過渡結束時，圓形和矩形剪裁的交集產生一個與 hero widget 相同大小的矩形。
也就是說，在過渡結束時，圖片已不再被剪裁。

[Create a new Flutter example][] and
update it using the files from the
[radial_hero_animation][] GitHub directory.

[建立一個新的 Flutter 範例][Create a new Flutter example] 並使用來自
[radial_hero_animation][] 的檔案更新。

To run the example:

執行範例：

* Tap on one of the three circular thumbnails to animate the image
  to a larger square positioned in the middle of a new route that
  obscures the original route.

  點選三個圓形縮圖中的任意一個，
  使影象變成位於新頁面中間的一個較大的正方形，且覆蓋原頁面。

* Return to the previous route by tapping the image, or by using the
  device’s back-to-the-previous-route gesture.

  點選圖片或使用裝置的返回手勢，返回之前頁面。

* You can slow the transition further using the `timeDilation`
  property.

  可以使用 `timeDilation` 屬性來減緩過渡。

### Photo class

### Photo 類

The `Photo` class builds the widget tree that holds the image:

Photo 類建立儲存影象的 widget 樹：

{% prettify dart %}
class Photo extends StatelessWidget {
  Photo({ Key key, this.photo, this.color, this.onTap }) : super(key: key);

  final String photo;
  final Color color;
  final VoidCallback onTap;

  Widget build(BuildContext context) {
    return [[highlight]]Material([[/highlight]]
      // Slightly opaque color appears where the image has transparency.
      [[highlight]]color: Theme.of(context).primaryColor.withOpacity(0.25),[[/highlight]]
      child: [[highlight]]InkWell([[/highlight]]
        onTap: [[highlight]]onTap,[[/highlight]]
        child: [[highlight]]Image.asset([[/highlight]]
            photo,
            fit: BoxFit.contain,
          )
      ),
    );
  }
}
{% endprettify %}

Key information:

重要資訊：

* The `Inkwell` captures the tap gesture.
  The calling function passes the `onTap()` function to the
  `Photo`'s constructor.

  `Inkwell` 捕捉點選動作。呼叫函式將 `onTap()` 函式傳遞給 Photo 的建構函式。

* During flight, the `InkWell` draws its splash on its 
  first Material ancestor.

  飛翔過程中，InkWell 的飛濺效果會出現在它第一個 Material 祖先上。

* The Material widget has a slightly opaque color, so the
  transparent portions of the image are rendered with color.
  This ensures that the circle-to-square transition is easy to see,
  even for images with transparency.

  Material widget 有輕微不透明色，所以影象的透明部分會被渲染上顏色。
  這確保了圓形到正方形過渡，即使是透明的影象依然清晰可見。

* The `Photo` class does not include the `Hero` in its widget tree.
  For the animation to work, the hero
  wraps the `RadialExpansion` widget.

  Photo 類別的 widget 樹中並不包含 Hero。
  為了使動畫執行，hero需要包裹 `RadialExpansion` widget。

### RadialExpansion class

### RadialExpansion 類

The `RadialExpansion` widget, the core of the demo, builds the
widget tree that clips the image during the transition.
The clipped shape results from the intersection of a circular clip
(that grows during flight),
with a rectangular clip (that remains a constant size throughout).

RadialExpansion widget，demo 的核心，建立過渡過程中剪裁影象的 widget 樹。
剪裁的形狀來自於圓形剪裁（飛翔過程中增長）
和矩形剪裁（自始至終保持一致大小）的交集。

To do this, it builds the following widget tree:

為此，它建立了如下 widget 樹：

<div class="text-center mb-4">
  <img src='/assets/images/docs/ui/animations/radial-expansion-class.png'
      alt="RadialExpansion widget tree" class="mw-100">
</div>

Here's the code:

程式碼如下：

{% prettify dart %}
class RadialExpansion extends StatelessWidget {
  RadialExpansion({
    Key key,
    this.maxRadius,
    this.child,
  }) : [[highlight]]clipRectSize = 2.0 * (maxRadius / math.sqrt2),[[/highlight]]
       super(key: key);

  final double maxRadius;
  final clipRectSize;
  final Widget child;

  @override
  Widget build(BuildContext context) {
    return [[highlight]]ClipOval([[/highlight]]
      child: [[highlight]]Center([[/highlight]]
        child: [[highlight]]SizedBox([[/highlight]]
          width: clipRectSize,
          height: clipRectSize,
          child: [[highlight]]ClipRect([[/highlight]]
            child: [[highlight]]child,[[/highlight]]  // Photo
          ),
        ),
      ),
    );
  }
}
{% endprettify %}

Key information:

重要資訊：

- The hero wraps the `RadialExpansion` widget.

  hero 包裹 `RadialExpansion` widget。

- As the hero flies, its size changes and, 
  because it constrains its child's size, 
  the `RadialExpansion` widget changes size to match.
  
  hero 飛翔時會改變大小，因為它限制了 child 的大小，
  所以 `RadialExpansion` widget 會改變大小以匹配。
  
- The `RadialExpansion` animation is created by two overlapping clips.

  `RadialExpansion` 動畫由兩個重疊的剪裁建立。

- The example defines the tweening interpolation using
  [`MaterialRectCenterArcTween`][].
  The default flight path for a hero animation
  interpolates the tweens using the corners of the heroes.
  This approach affects the hero's aspect ratio during
  the radial transformation, so the new flight path uses
  `MaterialRectCenterArcTween` to interpolate the tweens using the
  center point of each hero.

  這個範例用 [`MaterialRectCenterArcTween`][] 定義了補間插值。
  hero 動畫的預設飛翔路徑，利用 heroes 的角插值補間。
  這個方法會影響到徑向過渡時 hero 的長寬比例，
  所以新的飛翔路徑使用 `MaterialRectCenterArcTween` 方法，
  利用每個 hero 的中心點來插值補間。

  Here's the code:

  程式碼如下：

  <!-- skip -->
  {% prettify dart %}
  static RectTween _createRectTween(Rect begin, Rect end) {
    return MaterialRectCenterArcTween(begin: begin, end: end);
  }
  {% endprettify %}

  The hero's flight path still follows an arc,
  but the image's aspect ratio remains constant.

  Hero 的飛行路線仍然是一個弧線，
  但影象的長寬比將保持不變。

[Animations in Flutter tutorial]: {{site.url}}/development/ui/animations/tutorial
[basic_hero_animation]: {{site.repo.this}}/tree/{{site.branch}}/examples/_animation/basic_hero_animation/
[basic_radial_hero_animation]: {{site.repo.this}}/tree/{{site.branch}}/examples/_animation/basic_radial_hero_animation
[Building Layouts in Flutter]: {{site.url}}/development/ui/layout
[`ClipOval`]: {{site.api}}/flutter/widgets/ClipOval-class.html
[ClipRect]: {{site.api}}/flutter/widgets/ClipRect-class.html
[Create a new Flutter example]: {{site.url}}/get-started/test-drive
[`createRectTween`]: {{site.api}}/flutter/widgets/CreateRectTween.html
[`debugPaintSizeEnabled`]: {{site.url}}/testing/code-debugging#debug-flags-layout
[`Hero`]: {{site.api}}/flutter/widgets/Hero-class.html
[hero_animation]: {{site.repo.this}}/tree/{{site.branch}}/examples/_animation/hero_animation/
[`Inkwell`]: {{site.api}}/flutter/material/InkWell-class.html
[Material Design motion spec]: {{site.material}}/design/motion/understanding-motion.html#principles
[`MaterialRectArcTween`]: {{site.api}}/flutter/material/MaterialRectArcTween-class.html
[`MaterialRectCenterArcTween`]: {{site.api}}/flutter/material/MaterialRectCenterArcTween-class.html
[`Navigator`]: {{site.api}}/flutter/widgets/Navigator-class.html
[Radial hero animation code]: #radial-hero-animation-code
[radial_hero_animation]: {{site.repo.this}}/tree/{{site.branch}}/examples/_animation/radial_hero_animation
[radial_hero_animation_animate<wbr>_rectclip]: {{site.repo.this}}/tree/{{site.branch}}/examples/_animation/radial_hero_animation_animate_rectclip
[Radial hero animations]: #radial-hero-animations
[Radial transformation]: https://web.archive.org/web/20180223140424/https://material.io/guidelines/motion/transforming-material.html
[`RectTween`]: {{site.api}}/flutter/animation/RectTween-class.html
[_Route_]: {{site.url}}/cookbook/navigation/navigation-basics
[`Route`]: {{site.api}}/flutter/widgets/Route-class.html
[Standard hero animation code]: #standard-hero-animation-code
[Tween&lt;Rect&gt;]: {{site.api}}/flutter/animation/Tween-class.html
[watch this issue]: {{site.repo.flutter}}/issues/10667
