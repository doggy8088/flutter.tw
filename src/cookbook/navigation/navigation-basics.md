---
title: Navigate to a new screen and back
title: 導航到一個新頁面和返回
description: How to navigate between routes.
description: 如何在路由之間進行導航。
tags: cookbook, 實用課程, 路由
keywords: 路由之間的切換
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/navigation/navigation_basics"?>

Most apps contain several screens for displaying different types of
information.
For example, an app might have a screen that displays products.
When the user taps the image of a product, a new screen displays
details about the product.

我們通常會用“屏”來表示應用的不同頁面（介面）。
比如，某個應用有一“屏”展示商品列表，當用戶點選某個商品的圖片，
會跳到新的一“屏”展示商品的詳細資訊。

{{site.alert.secondary}}

  **Terminology**: In Flutter, _screens_ and _pages_ are called _routes_.
  The remainder of this recipe refers to routes.
  
  **術語**: 在 Flutter 中，**屏 (screen)** 和 **頁面 (page)** 都叫做 **路由 (route)**，
  在下文中統稱為“路由 (route)”。
  
{{site.alert.end}}

In Android, a route is equivalent to an Activity.
In iOS, a route is equivalent to a ViewController.
In Flutter, a route is just a widget.

在 Android 開發中，Activity 相當於“路由”，
在 iOS 開發中，ViewController 相當於“路由”。
在 Flutter 中，“路由”也是一個 widget。

This recipe uses the [`Navigator`][] to navigate to a new route.

怎麼樣從一個“路由”跳轉到新的“路由”呢？[`Navigator`][] 類別。

這個課程裡我們使用 [`Navigator`][] 來跳轉到一個新的“路由”：

## Directions

## 步驟

The next few sections show how to navigate between two routes,
using these steps:

下面來展示如何在兩個路由間跳轉，總共分三步：

  1. Create two routes.
  
     建立兩個路由
     
  2. Navigate to the second route using Navigator.push().
     
     用 Navigator.push() 跳轉到第二個路由
     
  3. Return to the first route using Navigator.pop().
  
     用 Navigator.pop() 回退到第一個路由

## 1. Create two routes

## 1. 建立兩個路由

First, create two routes to work with. Since this is a basic example,
each route contains only a single button. Tapping the button on the
first route navigates to the second route. Tapping the button on the
second route returns to the first route.

首先，我們來建立兩個路由。這是個最簡單的例子，每個路由只包含一個按鈕。
點選第一個路由上的按鈕會跳轉到第二個路由，
點選第二個路由上的按鈕，會回退到第一個路由。

First, set up the visual structure:

首先來編寫介面佈局程式碼：

<?code-excerpt "lib/main_step1.dart (FirstSecondRoutes)"?>
```dart
class FirstRoute extends StatelessWidget {
  const FirstRoute({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('First Route'),
      ),
      body: Center(
        child: ElevatedButton(
          child: const Text('Open route'),
          onPressed: () {
            // Navigate to second route when tapped.
          },
        ),
      ),
    );
  }
}

class SecondRoute extends StatelessWidget {
  const SecondRoute({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Second Route'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Navigate back to first route when tapped.
          },
          child: const Text('Go back!'),
        ),
      ),
    );
  }
}
```

## 2. Navigate to the second route using Navigator.push()

## 2. 用 Navigator.push() 跳轉到第二個路由

To switch to a new route, use the [`Navigator.push()`][]
method. The `push()` method adds a `Route` to the stack of routes managed by
the `Navigator`. Where does the `Route` come from?
You can create your own, or use a [`MaterialPageRoute`][],
which is useful because it transitions to the
new route using a platform-specific animation.

使用 [`Navigator.push()`] 方法跳轉到新的路由，
`push()` 方法會新增一個 `Route` 物件到導航器的堆疊上。
那麼這個 `Route` 物件是從哪裡來的呢？
你可以自己實現一個，或者直接使用 [`MaterialPageRoute`][] 類別。
使用 `MaterialPageRoute` 是非常方便的，
框架已經為我們實現了和平台原生類似的切換動畫。 

In the `build()` method of the `FirstRoute` widget,
update the `onPressed()` callback:

在 `FirstRoute` widget 的 `build()` 方法中，
我們來修改  `onPressed()` 回呼(Callback)函式：

<?code-excerpt "lib/main_step2.dart (FirstRouteOnPressed)"?>
```dart
// Within the `FirstRoute` widget
onPressed: () {
  Navigator.push(
    context,
    MaterialPageRoute(builder: (context) => const SecondRoute()),
  );
}
```

## 3. Return to the first route using Navigator.pop()

## 3. 用 Navigator.pop() 回退到第一個路由

How do you close the second route and return to the first?
By using the [`Navigator.pop()`][] method.
The `pop()` method removes the current `Route` from the stack of
routes managed by the `Navigator`.

怎麼關閉第二個路由回退到第一個呢? 
使用 [`Navigator.pop()`][] 方法，
`pop()` 方法會從導航器堆疊上移除 `Route` 物件。

To implement a return to the original route, update the `onPressed()`
callback in the `SecondRoute` widget:

實現返回第一個頁面，更新 `SecondRoute` widget
的 `onPressed()` 方法回呼(Callback)。

<?code-excerpt "lib/main_step2.dart (SecondRouteOnPressed)"?>
```dart
// Within the SecondRoute widget
onPressed: () {
  Navigator.pop(context);
}
```

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() {
  runApp(const MaterialApp(
    title: 'Navigation Basics',
    home: FirstRoute(),
  ));
}

class FirstRoute extends StatelessWidget {
  const FirstRoute({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('First Route'),
      ),
      body: Center(
        child: ElevatedButton(
          child: const Text('Open route'),
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => const SecondRoute()),
            );
          },
        ),
      ),
    );
  }
}

class SecondRoute extends StatelessWidget {
  const SecondRoute({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Second Route'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: const Text('Go back!'),
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/navigation-basics.gif" alt="Navigation Basics Demo" class="site-mobile-screenshot" />
</noscript>

## Navigation with CupertinoPageRoute

In the previous example you learned how to navigate between screens
using the [`MaterialPageRoute`][] from [Material Components][].
However, in Flutter you are not limited to Material design language,
instead, you also have access to [Cupertino][] (iOS-style) widgets.

Implementing navigation with Cupertino widgets follows the same steps
as when using [`MaterialPageRoute`][], 
but instead you use [`CupertinoPageRoute`][]
which provides an iOS-style transition animation.

In the following example, these widgets have been replaced:

- [`MaterialApp`][] replaced by [`CupertinoApp`].
- [`Scaffold`][] replaced by [`CupertinoPageScaffold`][].
- [`ElevatedButton`][] replaced by [`CupertinoButton`][].

This way, the example follows the current iOS design language.

{{site.alert.secondary}}
  You don't need to replace all Material widgets with Cupertino versions
  to use [`CupertinoPageRoute`][]
  since Flutter allows you to mix and match Material and Cupertino widgets
  depending on your needs.
{{site.alert.end}}

<?code-excerpt "lib/main_cupertino.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/cupertino.dart';

void main() {
  runApp(const CupertinoApp(
    title: 'Navigation Basics',
    home: FirstRoute(),
  ));
}

class FirstRoute extends StatelessWidget {
  const FirstRoute({super.key});

  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      navigationBar: const CupertinoNavigationBar(
        middle: Text('First Route'),
      ),
      child: Center(
        child: CupertinoButton(
          child: const Text('Open route'),
          onPressed: () {
            Navigator.push(
              context,
              CupertinoPageRoute(builder: (context) => const SecondRoute()),
            );
          },
        ),
      ),
    );
  }
}

class SecondRoute extends StatelessWidget {
  const SecondRoute({super.key});

  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      navigationBar: const CupertinoNavigationBar(
        middle: Text('Second Route'),
      ),
      child: Center(
        child: CupertinoButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: const Text('Go back!'),
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/navigation-basics-cupertino.gif" alt="Navigation Basics Cupertino Demo" class="site-mobile-screenshot" />
</noscript>

[Cupertino]: {{site.docs}}/ui/widgets/cupertino
[Material Components]: {{site.docs}}/ui/widgets/material
[`CupertinoApp`]: {{site.api}}/flutter/cupertino/CupertinoApp-class.html
[`CupertinoButton`]: {{site.api}}/flutter/cupertino/CupertinoButton-class.html
[`CupertinoPageRoute`]: {{site.api}}/flutter/cupertino/CupertinoPageRoute-class.html
[`CupertinoPageScaffold`]: {{site.api}}/flutter/cupertino/CupertinoPageScaffold-class.html
[`ElevatedButton`]: {{site.api}}/flutter/material/ElevatedButton-class.html
[`MaterialApp`]: {{site.api}}/flutter/material/MaterialApp-class.html
[`MaterialPageRoute`]: {{site.api}}/flutter/material/MaterialPageRoute-class.html
[`Navigator.pop()`]: {{site.api}}/flutter/widgets/Navigator/pop.html
[`Navigator.push()`]: {{site.api}}/flutter/widgets/Navigator/push.html
[`Navigator`]: {{site.api}}/flutter/widgets/Navigator-class.html
[`Scaffold`]: {{site.api}}/flutter/material/Scaffold-class.html
