---
title: Animate a page route transition
title: 為頁面切換加入動畫效果
description: How to animate from one page to another.
description: 如何在頁面過渡之間使用動畫。
tags: cookbook, 實用課程, 動畫效果
keywords: 頁面切換效果,自訂路由
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/animation/page_route_animation/"?>

A design language, such as Material, defines standard behaviors when
transitioning between routes (or screens). Sometimes, though, a custom
transition between screens can make an app more unique. To help,
[`PageRouteBuilder`][] provides an [`Animation`] object.
This `Animation` can be used with [`Tween`][] and
[`Curve`][] objects to customize the transition animation.
This recipe shows how to transition between
routes by animating the new route into view from
the bottom of the screen.

在不同路由（或介面）之間進行切換的時候，許多設計語言，
例如 Material 設計，都定義了一些標準行為。
但有時自訂路由會讓 app 看上去更加的獨特。
為了更好的完成這一點，[`PageRouteBuilder`][] 提供了一個 [`Animation`][] 物件。
這個 `Animation` 能夠透過結合
[`Tween`][] 以及 [`Curve`][] 物件來自訂路由轉換動畫。
這篇指南將會展示如何在兩個路由之間切換時使用從螢幕底部動畫出來的路由。

To create a custom page route transition, this recipe uses the following steps:

要建立這個自訂路由動畫，這篇指南使用了以下步驟：

1. Set up a PageRouteBuilder

   搭建一個 PageRouteBuilder

2. Create a `Tween`

   建立一個 `Tween`

3. Add an `AnimatedWidget`

   新增一個 `AnimatedWidget`

4. Use a `CurveTween`

   使用 `CurveTween`

5. Combine the two `Tween`s

   組合這兩個 `Tween`

## 1. Set up a PageRouteBuilder

## 1. 搭建一個 PageRouteBuilder

To start, use a [`PageRouteBuilder`][] to create a [`Route`][].
`PageRouteBuilder` has two callbacks, one to build the content of the route
(`pageBuilder`), and one to build the route's transition (`transitionsBuilder`).

我們從使用一個 [`PageRouteBuilder`][] 來建立一個 [`Route`][]。
`PageRouteBuilder` 有兩個回呼(Callback)，第一個是建立這個路由的內容（`pageBuilder`），
另一個則是建立一個路由的轉換器（`transitionsBuilder`）。

{{site.alert.note}}
  
  The `child` parameter in transitionsBuilder is the widget returned from
  pageBuilder. The `pageBuilder` function is only called the first time the
  route is built. The framework can avoid extra work because `child` stays the
  same throughout the transition.
  
  transitionsBuilder 的 `child` 引數是透過 `pageBuilder` 方法
  來返回一個 transitionsBuilder widget，這個 `pageBuilder` 方法僅會在
  第一次建構路由的時候被呼叫。框架能夠自動避免做額外的工作，因為
  整個過渡期間 `child` 儲存了同一個例項。
  
{{site.alert.end}}

The following example creates two routes: a home route with a "Go!" button, and
a second route titled "Page 2".

下面的範例將會建立兩個路由：一個主頁路由，
包含了 "Go!" 按鈕，還有第二個路由，包含了一個顯示 "Page 2 的標題。

<?code-excerpt "lib/starter.dart (Starter)"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    const MaterialApp(
      home: Page1(),
    ),
  );
}

class Page1 extends StatelessWidget {
  const Page1({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.of(context).push(_createRoute());
          },
          child: const Text('Go!'),
        ),
      ),
    );
  }
}

Route _createRoute() {
  return PageRouteBuilder(
    pageBuilder: (context, animation, secondaryAnimation) => const Page2(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      return child;
    },
  );
}

class Page2 extends StatelessWidget {
  const Page2({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: const Center(
        child: Text('Page 2'),
      ),
    );
  }
}
```

## 2. Create a Tween

## 2. 建立一個 Tween

To make the new page animate in from the bottom, it should animate from
`Offset(0,1)` to `Offset(0, 0)` (usually defined using the `Offset.zero`
constructor). In this case, the Offset is a 2D vector for the
['FractionalTranslation'][] widget.
Setting the `dy` argument to 1 represents a vertical translation one
full height of the page.

為了使新頁面從底部動畫出來，它應該從 `Offset(0,1)` 到 `Offset(0, 0)` 進行動畫。
（通常我們會使用 `Offset.zero` 構造器。）在這個情況下，
對於 ['FractionalTranslation'][] widget 來說偏移量是一個 2D 向量值。
將 `dy` 引數設為 1，這代表在豎直方向上切換整個頁面的高度。

The `transitionsBuilder` callback has an `animation` parameter. It's an
`Animation<double>` that produces values between 0 and 1. Convert the
Animation<double> into an Animation<Offset> using a Tween:
  
`transitionsBuilder` 的回呼(Callback)有一個 `animation` 引數。
它其實是一個 `Animation<double>`，提供 0 到 1 的值。
使用 Tween 來將 Animation<double> 轉為 Animation<Offset>。

<?code-excerpt "lib/starter.dart (step1)"?>
```dart
transitionsBuilder: (context, animation, secondaryAnimation, child) {
  const begin = Offset(0.0, 1.0);
  const end = Offset.zero;
  final tween = Tween(begin: begin, end: end);
  final offsetAnimation = animation.drive(tween);
  return child;
},
```

## 3. Use an AnimatedWidget

## 3. 使用 AnimatedWidget

Flutter has a set of widgets extending [`AnimatedWidget`][]
that rebuild themselves when the value of the animation changes. For instance,
SlideTransition takes an `Animation<Offset>` and translates its child (using a
FractionalTranslation widget) whenever the value of the animation changes.

Flutter 有一堆繼承自 [`AnimatedWidget`][] 的 widget，
它們能夠在動畫的值發生改變時自動重建自己。
舉個例子，SlideTransition 拿到一個 `Animation<Offset>`
並在動畫改變時使用 FractionalTranslation widget 轉換其子級。

AnimatedWidget Return a [`SlideTransition`][]
with the `Animation<Offset>` and the child widget:

AnimatedWidget 返回了一個 帶有 `Animation<Offset>` 
的 [`SlideTransition`][]，以及 child widget：

<?code-excerpt "lib/starter.dart (step2)"?>
```dart
transitionsBuilder: (context, animation, secondaryAnimation, child) {
  const begin = Offset(0.0, 1.0);
  const end = Offset.zero;
  final tween = Tween(begin: begin, end: end);
  final offsetAnimation = animation.drive(tween);

  return SlideTransition(
    position: offsetAnimation,
    child: child,
  );
},
```

## 4. Use a CurveTween

## 4. 使用 CurveTween

Flutter provides a selection of easing curves that
adjust the rate of the animation over time.
The [`Curves`][] class
provides a predefined set of commonly used curves.
For example, `Curves.easeOut`
makes the animation start quickly and end slowly.

Flutter 提供了一系列緩和曲線，可以調整一段時間內的動畫速率。
[`Curves`][] 類提供了一個提前定義的用法相似的 curves。
例如，`Curves.easeOut` 將會讓動畫開始很快結束很慢。

To use a Curve, create a new [`CurveTween`][]
and pass it a Curve:

要使用 Curve，建立一個 [`CurveTween`][]
並傳一個 Curve：

<?code-excerpt "lib/starter.dart (step3)"?>
```dart
var curve = Curves.ease;
var curveTween = CurveTween(curve: curve);
```

This new Tween still produces values from 0 to 1. In the next step, it will be
combined the `Tween<Offset>` from step 2.

新的 Tween 依然提供 0 到 1 之間的值。
在下一步中，它將會結合第二步中提到的 `Tween<Offset>`。

## 5. Combine the two Tweens

## 5. 結合兩個 Tween

To combine the tweens,
use [`chain()`][]:

為了結合兩個 tween，請使用 [`chain()`][]:

<?code-excerpt "lib/main.dart (Tween)"?>
```dart
const begin = Offset(0.0, 1.0);
const end = Offset.zero;
const curve = Curves.ease;

var tween = Tween(begin: begin, end: end).chain(CurveTween(curve: curve));
```

Then use this tween by passing it to `animation.drive()`. This creates a new
`Animation<Offset>` that can be given to the `SlideTransition` widget:

它們透過把這個 tween 傳遞給 `animation.drive()` 
來建立一個新的 `Animation<Offset>`，
然後你就能把它傳給 `SlideTransition` widget：

<?code-excerpt "lib/main.dart (SlideTransition)"?>
```dart
return SlideTransition(
  position: animation.drive(tween),
  child: child,
);
```

This new Tween (or Animatable) produces `Offset` values by first evaluating the
`CurveTween`, then evaluating the `Tween<Offset>.` When the animation runs, the
values are computed in this order:

這個新的 Tween（或者是能夠動畫的東西）透過評估 `CurveTween` 來提供 `Offset`，
然後評估 `Tween<Offset>`。當動畫執行時，值都被這條命令計算出：

1. The animation (provided to the transitionsBuilder callback) produces values
   from 0 to 1.
   
   這個動畫提供了從 0 到 1 的值。（透過 transitionsBuilder 的值提供）
   
2. The CurveTween maps those values to new values between 0 and 1 based on its
   curve.
   
   這個 CurveTween 根據其將這些值對映到介於 0 和 1 之間的新曲線值。
   
3. The `Tween<Offset>` maps the `double` values to `Offset` values.

   `Tween<Offset>` 將 `double` 值對映為 `Offset` 值。

Another way to create an `Animation<Offset>` with an easing curve is to use a
`CurvedAnimation`:

使用緩動曲線建立 `Animation<Offset>`
的另一種方法是使用 `CurvedAnimation`：

<?code-excerpt "lib/starter.dart (step4)" replace="/,$//g"?>
```dart
transitionsBuilder: (context, animation, secondaryAnimation, child) {
  const begin = Offset(0.0, 1.0);
  const end = Offset.zero;
  const curve = Curves.ease;

  final tween = Tween(begin: begin, end: end);
  final curvedAnimation = CurvedAnimation(
    parent: animation,
    curve: curve,
  );

  return SlideTransition(
    position: tween.animate(curvedAnimation),
    child: child,
  );
}
```

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() {
  runApp(
    const MaterialApp(
      home: Page1(),
    ),
  );
}

class Page1 extends StatelessWidget {
  const Page1({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.of(context).push(_createRoute());
          },
          child: const Text('Go!'),
        ),
      ),
    );
  }
}

Route _createRoute() {
  return PageRouteBuilder(
    pageBuilder: (context, animation, secondaryAnimation) => const Page2(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      const begin = Offset(0.0, 1.0);
      const end = Offset.zero;
      const curve = Curves.ease;

      var tween = Tween(begin: begin, end: end).chain(CurveTween(curve: curve));

      return SlideTransition(
        position: animation.drive(tween),
        child: child,
      );
    },
  );
}

class Page2 extends StatelessWidget {
  const Page2({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: const Center(
        child: Text('Page 2'),
      ),
    );
  }
}
```
<noscript>
  <img src="/assets/images/docs/cookbook/page-route-animation.gif" alt="Demo showing a custom page route transition animating up from the bottom of the screen" class="site-mobile-screenshot" />
</noscript>


[`AnimatedWidget`]: {{site.api}}/flutter/widgets/AnimatedWidget-class.html
[`Animation`]: {{site.api}}/flutter/animation/Animation-class.html
[`chain()`]: {{site.api}}/flutter/animation/Animatable/chain.html
[`Curve`]: {{site.api}}/flutter/animation/Curve-class.html
[`Curves`]: {{site.api}}/flutter/animation/Curves-class.html
[`CurveTween`]: {{site.api}}/flutter/animation/CurveTween-class.html
['FractionalTranslation']: {{site.api}}/flutter/widgets/FractionalTranslation-class.html
[`PageRouteBuilder`]: {{site.api}}/flutter/widgets/PageRouteBuilder-class.html
[`Route`]: {{site.api}}/flutter/widgets/Route-class.html
[`SlideTransition`]: {{site.api}}/flutter/widgets/SlideTransition-class.html
[`Tween`]: {{site.api}}/flutter/animation/Tween-class.html
