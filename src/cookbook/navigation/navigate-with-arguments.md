---
title: Pass arguments to a named route
title: 給特定的 route 傳參
description: How to pass arguments to a named route.
description: 如何向命名路由傳參。
tags: cookbook, 實用課程, 路由
keywords: 引數傳遞,讀取引數
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/navigation/navigate_with_arguments"?>

The [`Navigator`][] provides the ability to navigate
to a named route from any part of an app using
a common identifier.
In some cases, you might also need to pass arguments to a
named route. For example, you might wish to navigate to the `/user` route and
pass information about the user to that route.

[`Navigator`][] 元件支援透過使用通用識別符號從應用程式的任何地方導航到特定路由。
在某些情況下，你可能還希望能夠傳遞引數給特定路由。
例如，你希望導航到 `/user` 路由並攜帶上使用者資訊。

{{site.alert.note}}

  Named routes are no longer recommended for most
  applications. For more information, see
  [Limitations][] in the [navigation overview][] page.

  針對大多數的應用情況，我們不再推薦使用命名的路由 (Named routes)，
  瞭解更多資訊，請參考 [導航概覽][navigation overview] 中的 [受限情況][Limitations] 部分。

{{site.alert.end}}

[Limitations]: {{site.url}}/ui/navigation#limitations
[navigation overview]: {{site.url}}/ui/navigation

You can accomplish this task using the `arguments` parameter of the
[`Navigator.pushNamed()`][] method. Extract the arguments using the
[`ModalRoute.of()`][] method or inside an [`onGenerateRoute()`][]
function provided to the [`MaterialApp`][] or [`CupertinoApp`][]
constructor.

在 Flutter 中，你能透過提供額外的 `arguments` 給 
[`Navigator.pushNamed()`][] 方法方便地完成這個任務。
透過使用 [`ModalRoute.of()`][] 方法或 
[`MaterialApp`][] 和 [`CupertinoApp`][] 構造器中的 
[`onGenerateRoute()`][] 來獲取引數。

This recipe demonstrates how to pass arguments to a named
route and read the arguments using `ModalRoute.of()`
and `onGenerateRoute()` using the following steps:

這個章節講解的是如何給特定路由傳遞引數並使用
`ModalRoute.of()` 和 `onGenerateRoute()` 來讀取引數。

## Directions

## 步驟

  1. Define the arguments you need to pass.

     定義需要傳遞的引數

  2. Create a widget that extracts the arguments.

     建立元件來獲取引數

  3. Register the widget in the `routes` table.

     把元件註冊到路由表中

  4. Navigate to the widget.

     導航到元件

## 1. Define the arguments you need to pass

## 1. 定義需要傳遞的引數

First, define the arguments you need to pass to the new route.
In this example, pass two pieces of data:
The `title` of the screen and a `message`.

首先，定義需要傳遞給新路由的引數。
在這個範例中我們傳遞了兩個資料：頁面的標題 `title` 和內容 `message`。

To pass both pieces of data, create a class that stores this information.
建立包含 title 和 message 欄位的實體類來同時傳遞這兩個資料。

<?code-excerpt "lib/main.dart (ScreenArguments)"?>
```dart
// You can pass any object to the arguments parameter.
// In this example, create a class that contains both
// a customizable title and message.
class ScreenArguments {
  final String title;
  final String message;

  ScreenArguments(this.title, this.message);
}
```

## 2. Create a widget that extracts the arguments

## 2. 建立元件來獲取引數

Next, create a widget that extracts and displays the
`title` and `message` from the `ScreenArguments`.
To access the `ScreenArguments`,
use the [`ModalRoute.of()`][] method.
This method returns the current route with the arguments.

接著，建立元件，從 `ScreenArguments` 提取 `title` 和 `message` 引數並展示。
為了存取 `ScreenArguments`，可以使用 [`ModalRoute.of()`][] 方法。
這個方法返回的是當前路由及其攜帶的引數。

<?code-excerpt "lib/main.dart (ExtractArgumentsScreen)"?>
```dart
// A Widget that extracts the necessary arguments from
// the ModalRoute.
class ExtractArgumentsScreen extends StatelessWidget {
  const ExtractArgumentsScreen({super.key});

  static const routeName = '/extractArguments';

  @override
  Widget build(BuildContext context) {
    // Extract the arguments from the current ModalRoute
    // settings and cast them as ScreenArguments.
    final args = ModalRoute.of(context)!.settings.arguments as ScreenArguments;

    return Scaffold(
      appBar: AppBar(
        title: Text(args.title),
      ),
      body: Center(
        child: Text(args.message),
      ),
    );
  }
}
```

## 3. Register the widget in the `routes` table

## 3. 把元件註冊到路由表中

Next, add an entry to the `routes` provided to the `MaterialApp` widget. The
`routes` define which widget should be created based on the name of the route.

然後，在 `MaterialApp` 的路由表 `routes` 中增加一個入口，
路由表 `routes` 會根據路由的名稱來決定需要建立哪個路由。 

{% comment %}
RegEx removes the return statement and adds the closing parenthesis at the end
{% endcomment %}
<?code-excerpt "lib/main.dart (Routes)" replace="/return //g;/$/\n)/g"?>
```dart
MaterialApp(
  routes: {
    ExtractArgumentsScreen.routeName: (context) =>
        const ExtractArgumentsScreen(),
  },
)
```


## 4. Navigate to the widget

## 4. 導航到元件

Finally, navigate to the `ExtractArgumentsScreen`
when a user taps a button using [`Navigator.pushNamed()`][].
Provide the arguments to the route via the `arguments` property. The
`ExtractArgumentsScreen` extracts the `title` and `message` from these
arguments.

最後，在使用者點選按鈕後導航到 `ExtractArgumentsScreen`。
在 [`Navigator.pushNamed()`][] 方法的 `arguments` 屬性裡提供需要傳遞的引數。
隨後，`ExtractArgumentsScreen` 就可以從引數中提取 `title` 和 `message`。

<?code-excerpt "lib/main.dart (PushNamed)"?>
```dart
// A button that navigates to a named route.
// The named route extracts the arguments
// by itself.
ElevatedButton(
  onPressed: () {
    // When the user taps the button,
    // navigate to a named route and
    // provide the arguments as an optional
    // parameter.
    Navigator.pushNamed(
      context,
      ExtractArgumentsScreen.routeName,
      arguments: ScreenArguments(
        'Extract Arguments Screen',
        'This message is extracted in the build method.',
      ),
    );
  },
  child: const Text('Navigate to screen that extracts arguments'),
),
```

## Alternatively, extract the arguments using `onGenerateRoute`

## 此外，還可以使用 `onGenerateRoute` 提取引數

Instead of extracting the arguments directly inside the widget, you can also
extract the arguments inside an [`onGenerateRoute()`][]
function and pass them to a widget.

除了直接從元件裡提取引數，你也可以透過 [`onGenerateRoute()`][] 函式提取引數，
然後把引數傳遞給元件。

The `onGenerateRoute()` function creates the correct route based on the given
[`RouteSettings`][].

`onGenerateRoute()` 函式會基於給定的 [`RouteSettings`][] 來建立正確的路由。

{% comment %}
RegEx removes the return statement, removed "routes" property and adds the closing parenthesis at the end
{% endcomment %}
<?code-excerpt "lib/main.dart (OnGenerateRoute)" replace="/^return //g;/  routes:((.)*\n){4}//g;/$/\n)/g"?>
```dart
MaterialApp(
  // Provide a function to handle named routes.
  // Use this function to identify the named
  // route being pushed, and create the correct
  // Screen.
  onGenerateRoute: (settings) {
    // If you push the PassArguments route
    if (settings.name == PassArgumentsScreen.routeName) {
      // Cast the arguments to the correct
      // type: ScreenArguments.
      final args = settings.arguments as ScreenArguments;

      // Then, extract the required data from
      // the arguments and pass the data to the
      // correct screen.
      return MaterialPageRoute(
        builder: (context) {
          return PassArgumentsScreen(
            title: args.title,
            message: args.message,
          );
        },
      );
    }
    // The code only supports
    // PassArgumentsScreen.routeName right now.
    // Other values need to be implemented if we
    // add them. The assertion here will help remind
    // us of that higher up in the call stack, since
    // this assertion would otherwise fire somewhere
    // in the framework.
    assert(false, 'Need to implement ${settings.name}');
    return null;
  },
)
```

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      routes: {
        ExtractArgumentsScreen.routeName: (context) =>
            const ExtractArgumentsScreen(),
      },
      // Provide a function to handle named routes.
      // Use this function to identify the named
      // route being pushed, and create the correct
      // Screen.
      onGenerateRoute: (settings) {
        // If you push the PassArguments route
        if (settings.name == PassArgumentsScreen.routeName) {
          // Cast the arguments to the correct
          // type: ScreenArguments.
          final args = settings.arguments as ScreenArguments;

          // Then, extract the required data from
          // the arguments and pass the data to the
          // correct screen.
          return MaterialPageRoute(
            builder: (context) {
              return PassArgumentsScreen(
                title: args.title,
                message: args.message,
              );
            },
          );
        }
        // The code only supports
        // PassArgumentsScreen.routeName right now.
        // Other values need to be implemented if we
        // add them. The assertion here will help remind
        // us of that higher up in the call stack, since
        // this assertion would otherwise fire somewhere
        // in the framework.
        assert(false, 'Need to implement ${settings.name}');
        return null;
      },
      title: 'Navigation with Arguments',
      home: const HomeScreen(),
    );
  }
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Screen'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            // A button that navigates to a named route.
            // The named route extracts the arguments
            // by itself.
            ElevatedButton(
              onPressed: () {
                // When the user taps the button,
                // navigate to a named route and
                // provide the arguments as an optional
                // parameter.
                Navigator.pushNamed(
                  context,
                  ExtractArgumentsScreen.routeName,
                  arguments: ScreenArguments(
                    'Extract Arguments Screen',
                    'This message is extracted in the build method.',
                  ),
                );
              },
              child: const Text('Navigate to screen that extracts arguments'),
            ),
            // A button that navigates to a named route.
            // For this route, extract the arguments in
            // the onGenerateRoute function and pass them
            // to the screen.
            ElevatedButton(
              onPressed: () {
                // When the user taps the button, navigate
                // to a named route and provide the arguments
                // as an optional parameter.
                Navigator.pushNamed(
                  context,
                  PassArgumentsScreen.routeName,
                  arguments: ScreenArguments(
                    'Accept Arguments Screen',
                    'This message is extracted in the onGenerateRoute '
                        'function.',
                  ),
                );
              },
              child: const Text('Navigate to a named that accepts arguments'),
            ),
          ],
        ),
      ),
    );
  }
}

// A Widget that extracts the necessary arguments from
// the ModalRoute.
class ExtractArgumentsScreen extends StatelessWidget {
  const ExtractArgumentsScreen({super.key});

  static const routeName = '/extractArguments';

  @override
  Widget build(BuildContext context) {
    // Extract the arguments from the current ModalRoute
    // settings and cast them as ScreenArguments.
    final args = ModalRoute.of(context)!.settings.arguments as ScreenArguments;

    return Scaffold(
      appBar: AppBar(
        title: Text(args.title),
      ),
      body: Center(
        child: Text(args.message),
      ),
    );
  }
}

// A Widget that accepts the necessary arguments via the
// constructor.
class PassArgumentsScreen extends StatelessWidget {
  static const routeName = '/passArguments';

  final String title;
  final String message;

  // This Widget accepts the arguments as constructor
  // parameters. It does not extract the arguments from
  // the ModalRoute.
  //
  // The arguments are extracted by the onGenerateRoute
  // function provided to the MaterialApp widget.
  const PassArgumentsScreen({
    super.key,
    required this.title,
    required this.message,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: Center(
        child: Text(message),
      ),
    );
  }
}

// You can pass any object to the arguments parameter.
// In this example, create a class that contains both
// a customizable title and message.
class ScreenArguments {
  final String title;
  final String message;

  ScreenArguments(this.title, this.message);
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/navigate-with-arguments.gif" alt="Demonstrates navigating to different routes with arguments" class="site-mobile-screenshot" />
</noscript>


[`CupertinoApp`]: {{site.api}}/flutter/cupertino/CupertinoApp-class.html
[`MaterialApp`]: {{site.api}}/flutter/material/MaterialApp-class.html
[`ModalRoute.of()`]: {{site.api}}/flutter/widgets/ModalRoute/of.html
[`Navigator`]: {{site.api}}/flutter/widgets/Navigator-class.html
[`Navigator.pushNamed()`]: {{site.api}}/flutter/widgets/Navigator/pushNamed.html
[`onGenerateRoute()`]: {{site.api}}/flutter/widgets/WidgetsApp/onGenerateRoute.html
[`RouteSettings`]: {{site.api}}/flutter/widgets/RouteSettings-class.html
