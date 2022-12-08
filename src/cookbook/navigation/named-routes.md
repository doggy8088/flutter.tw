---
title: Navigate with named routes
title: 導航到對應名稱的 routes 裡
description: How to implement named routes for navigating between screens.
description: 如何實現用於導航的命名路由。
tags: cookbook, 實用課程, 路由
keywords: 頁面跳轉
prev:
  title: Navigate to a new screen and back
  title: 導航到一個新頁面和返回
  path: /docs/cookbook/navigation/navigation-basics
next:
  title: Pass arguments to a named route
  title: 給特定的 route 傳參
  path: /docs/cookbook/navigation/navigate-with-arguments
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/navigation/named_routes"?>

{{site.alert.note}}
  Named routes are no longer recommended for most
  applications. For more information, see
  [Limitations][] in the [navigation overview][] page.
{{site.alert.end}}

[Limitations]: {{site.url}}/development/ui/navigation#limitations
[navigation overview]: {{site.url}}/development/ui/navigation

In the [Navigate to a new screen and back][] recipe,
you learned how to navigate to a new screen by creating a new route and
pushing it to the [`Navigator`][].

在 [導航到一個新頁面和返回][Navigate to a new screen and back] 一節中，
我們透過建立一個新的路由並將它推到 [`Navigator`][] 
類中學習到了如何導航到新的一個介面 (screen)。

However, if you need to navigate to the same screen in many parts
of your app, this approach can result in code duplication.
The solution is to define a _named route_,
and use the named route for navigation.

然而，如果我們需要在應用的很多地方導航到同一介面，
這樣做就會導致程式碼重複。
在這種情況下，定義 **命名路由 (named route)** 
並使用它進行導航就會非常方便。

To work with named routes,
use the [`Navigator.pushNamed()`][] function.
This example replicates the functionality from the original recipe,
demonstrating how to use named routes using the following steps:

要使用命名路由，我們可以使用 [`Navigator.pushNamed()`][] 方法。
下面的例子展示如何使用“命名路由”來實現前一節中的功能。

## Directions

## 步驟

  1. Create two screens.
  
     建立兩個介面

  2. Define the routes.

     定義路由

  3. Navigate to the second screen using `Navigator.pushNamed()`.

     使用 `Navigator.pushNamed()` 跳轉到第二個介面

  4. Return to the first screen using `Navigator.pop()`.

     使用 `Navigator.pop()` 返回到第一個介面

## 1. Create two screens

## 1. 建立兩個介面

First, create two screens to work with. The first screen contains a
button that navigates to the second screen. The second screen contains a
button that navigates back to the first.

首先，我們需要兩個介面來開始。
第一個介面將包含一個跳轉到第二個介面的按鈕，
第二個介面將包含一個跳轉回第一個介面的按鈕。

<?code-excerpt "lib/main_original.dart"?>
```dart
import 'package:flutter/material.dart';

class FirstScreen extends StatelessWidget {
  const FirstScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('First Screen'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Navigate to the second screen when tapped.
          },
          child: const Text('Launch screen'),
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  const SecondScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Second Screen'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Navigate back to first screen when tapped.
          },
          child: const Text('Go back!'),
        ),
      ),
    );
  }
}
```

## 2. Define the routes

## 2. 定義路由

Next, define the routes by providing additional properties
to the [`MaterialApp`][] constructor: the `initialRoute`
and the `routes` themselves.

接下來，我們需要透過為 [`MaterialApp`][] 的建構函式額外的屬性：
`initialRoute` 和 `routes` 來定義我們的路由。

The `initialRoute` property defines which route the app should start with.
The `routes` property defines the available named routes and the widgets
to build when navigating to those routes.

`initialRoute` 屬性定義了應用應該從哪個路由啟動。
`routes` 屬性定義了所有可用的命名路由，
以及當我們跳轉到這些路由時應該建構的 widgets。

{% comment %}
RegEx removes the trailing comma
{% endcomment %}
<?code-excerpt "lib/main.dart (MaterialApp)" replace="/,$//g"?>
```dart
MaterialApp(
  title: 'Named Routes Demo',
  // Start the app with the "/" named route. In this case, the app starts
  // on the FirstScreen widget.
  initialRoute: '/',
  routes: {
    // When navigating to the "/" route, build the FirstScreen widget.
    '/': (context) => const FirstScreen(),
    // When navigating to the "/second" route, build the SecondScreen widget.
    '/second': (context) => const SecondScreen(),
  },
)
```

{{site.alert.warning}}

  When using `initialRoute`, **don't** define a `home` property.

  當使用 `initialRoute` 時，需要確保你沒有同時定義 `home` 屬性。
{{site.alert.end}}

## 3. Navigate to the second screen

## 3. 跳轉到第二個介面

With the widgets and routes in place, trigger navigation by using the
[`Navigator.pushNamed()`][] method.
This tells Flutter to build the widget defined in the
`routes` table and launch the screen.

準備好了 Widgets 和路由，我們就可以開始進行頁面跳轉。
在這裡，我們將使用 [`Navigator.pushNamed()`][] 函式。
它會告訴 Flutter 去建構我們在 `routes` 表中定義的 widget 並啟動該介面。

In the `build()` method of the `FirstScreen` widget, update the `onPressed()`
callback:

在 `FirstScreen` widget 的 `build()` 方法中，
我們將更新 `onPressed()` 回呼(Callback)：

{% comment %}
RegEx removes the trailing comma
{% endcomment %}
<?code-excerpt "lib/main.dart (PushNamed)" replace="/,$//g"?>
```dart
// Within the `FirstScreen` widget
onPressed: () {
  // Navigate to the second screen using a named route.
  Navigator.pushNamed(context, '/second');
}
```

## 4. Return to the first screen

## 4. 返回到第一個介面

To navigate back to the first screen, use the
[`Navigator.pop()`][] function.

為了能夠跳轉回第一個頁面，
我們可以使用 [`Navigator.pop()`][] 方法。

{% comment %}
RegEx removes the trailing comma
{% endcomment %}
<?code-excerpt "lib/main.dart (Pop)" replace="/,$//g"?>
```dart
// Within the SecondScreen widget
onPressed: () {
  // Navigate back to the first screen by popping the current route
  // off the stack.
  Navigator.pop(context);
}
```

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(
      title: 'Named Routes Demo',
      // Start the app with the "/" named route. In this case, the app starts
      // on the FirstScreen widget.
      initialRoute: '/',
      routes: {
        // When navigating to the "/" route, build the FirstScreen widget.
        '/': (context) => const FirstScreen(),
        // When navigating to the "/second" route, build the SecondScreen widget.
        '/second': (context) => const SecondScreen(),
      },
    ),
  );
}

class FirstScreen extends StatelessWidget {
  const FirstScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('First Screen'),
      ),
      body: Center(
        child: ElevatedButton(
          // Within the `FirstScreen` widget
          onPressed: () {
            // Navigate to the second screen using a named route.
            Navigator.pushNamed(context, '/second');
          },
          child: const Text('Launch screen'),
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  const SecondScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Second Screen'),
      ),
      body: Center(
        child: ElevatedButton(
          // Within the SecondScreen widget
          onPressed: () {
            // Navigate back to the first screen by popping the current route
            // off the stack.
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


[`MaterialApp`]: {{site.api}}/flutter/material/MaterialApp-class.html
[Navigate to a new screen and back]: {{site.url}}/cookbook/navigation/navigation-basics
[`Navigator`]: {{site.api}}/flutter/widgets/Navigator-class.html
[`Navigator.pop()`]: {{site.api}}/flutter/widgets/Navigator/pop.html
[`Navigator.pushNamed()`]: {{site.api}}/flutter/widgets/Navigator/pushNamed.html
