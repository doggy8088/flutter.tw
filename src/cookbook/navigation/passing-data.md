---
title: Send data to a new screen
title: 傳遞資料到新頁面
description: How to pass data to a new route.
description: 如何向新路由傳遞資料。
tags: cookbook, 實用課程, 路由
keywords: 傳參,待辦事項應用
prev:
  title: Return data from a screen
  title: 從一個頁面回傳資料
  path: /docs/cookbook/navigation/returning-data
next:
  title: Delete data on the internet
  title: 刪除網路資料
  path: /docs/cookbook/networking/delete-data
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/navigation/passing_data"?>

Often, you not only want to navigate to a new screen,
but also pass data to the screen as well.
For example, you might want to pass information about
the item that's been tapped.

在開發的過程中，我們經常需要在跳轉到新頁面的時候，
能同時傳遞一些資料。比如，傳遞使用者點選的元素資訊。

Remember: Screens are Just widgets.
In this example, create a list of todos.
When a todo is tapped, navigate to a new screen (widget) that
displays information about the todo.
This recipe uses the following steps:

還記得麼，全屏的介面也只是 widget。在這個例子中，我們會建立一個待辦事項列表，
當某個事項被點選的時候，會跳轉到新的一屏 (widget)，
在新的一屏顯示待辦事項的詳細資訊。

## Directions

## 步驟

  1. Define a todo class.

     定義一個描述待辦事項的資料類

  2. Display a list of todos.

     顯示待辦事項

  3. Create a detail screen that can display information about a todo.

     建立一個顯示待辦事項詳細資訊的介面

  4. Navigate and pass data to the detail screen.

     傳遞資料並跳轉到待辦事項詳細資訊介面

## 1. Define a todo class

## 1. 定義一個描述待辦事項的資料類

First, you need a simple way to represent todos. For this example,
create a class that contains two pieces of data: the title and description.

首先，我們需要一個簡單的方式來描述待辦事項。
我們建立一個類別叫做 `Todo`，包含 `title` 和 `description` 兩個成員變數。

<?code-excerpt "lib/main.dart (Todo)"?>
```dart
class Todo {
  final String title;
  final String description;

  const Todo(this.title, this.description);
}
```

## 2. Create a list of todos

## 2. 建立待辦事項列表

Second, display a list of todos. In this example, generate
20 todos and show them using a ListView.
For more information on working with lists,
see the [Use lists][] recipe.

第二步，我們需要顯示一個待辦事項列表，
產生 20 條待辦事項並用 `ListView` 顯示。
如果你想了解更多關於列表顯示的內容，
請閱讀文件 [`基礎列表`][Use lists]。

### Generate the list of todos

### 產生待辦事項列表

<?code-excerpt "lib/main.dart (Generate)" replace="/^todos:/final todos =/g;/,$/;/g"?>
```dart
final todos = List.generate(
  20,
  (i) => Todo(
    'Todo $i',
    'A description of what needs to be done for Todo $i',
  ),
);
```

### Display the list of todos using a ListView

### 用 `ListView` 顯示待辦事項列表

<?code-excerpt "lib/main_todoscreen.dart (ListViewBuilder)" replace="/^body: //g;/;$//g"?>
```dart
ListView.builder(
  itemCount: todos.length,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text(todos[index].title),
    );
  },
),
```

So far, so good.
This generates 20 todos and displays them in a ListView.

到目前為止，我們生成了 20 條待辦事項，
並用 `ListView` 把它顯示出來了。

## 3. Create a Todo screen to display the list

## 3. 建立一個待辦頁面顯示待辦事件列表

For this, we create a `StatelessWidget`. We call it `TodosScreen`.
Since the contents of this page won't change during runtime,
we'll have to require the list
of todos within the scope of this widget.

為了實現這個，我們要建立一個無狀態的 widget (`StatelessWidget`)，
我們叫它 `TodosScreen`。
因為這個頁面在執行時內容並不會變動，
在這個 widget 的 scope 裡，
我們會把這個待辦事項的陣列設定為必須 (加入 @require 限定符）。

We pass in our `ListView.builder` as body of the widget we're returning to `build()`.
This'll render the list on to the screen for you to get going!

我們把 `ListView.builder` 作為 body 的引數返回給 `build()` 方法，
這將會把列表渲染到螢幕上供你繼續下一步。

<?code-excerpt "lib/main_todoscreen.dart (TodosScreen)"?>
```dart
class TodosScreen extends StatelessWidget {
  // Requiring the list of todos.
  const TodosScreen({super.key, required this.todos});

  final List<Todo> todos;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Todos'),
      ),
      //passing in the ListView.builder
      body: ListView.builder(
        itemCount: todos.length,
        itemBuilder: (context, index) {
          return ListTile(
            title: Text(todos[index].title),
          );
        },
      ),
    );
  }
}
```

With Flutter's default styling, you're good to go without sweating about 
things that you'd like to do later on!

使用 Flutter 自帶的樣式，未來會變得很輕鬆。

## 4. Create a detail screen to display information about a todo

## 4. 建立一個顯示待辦事項詳細資訊的介面

Now, create the second screen. The title of the screen contains the
title of the todo, and the body of the screen shows the description.

現在，我們來建立第二個全屏的介面，
介面的標題是待辦事項的標題，
介面下面顯示待辦事項的描述資訊。

Since the detail screen is a normal `StatelessWidget`,
require the user to enter a `Todo` in the UI.
Then, build the UI using the given todo.

這個介面是一個 `StatelessWidget`，建立的時需要傳遞 `Todo` 物件給它，
它就可以使用傳給他的 `Todo` 物件來建構 UI 。

<?code-excerpt "lib/main.dart (detail)"?>
```dart
class DetailScreen extends StatelessWidget {
  // In the constructor, require a Todo.
  const DetailScreen({super.key, required this.todo});

  // Declare a field that holds the Todo.
  final Todo todo;

  @override
  Widget build(BuildContext context) {
    // Use the Todo to create the UI.
    return Scaffold(
      appBar: AppBar(
        title: Text(todo.title),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Text(todo.description),
      ),
    );
  }
}
```

## 5. Navigate and pass data to the detail screen

## 5. 傳遞資料並跳轉到待辦事項詳細資訊介面

With a `DetailScreen` in place,
you're ready to perform the Navigation.
In this example, navigate to the `DetailScreen` when a user
taps a todo in the list. Pass the todo to the `DetailScreen`.

上面寫完了 `DetailScreen` ，現在該執行介面跳轉啦！
我們想讓使用者在點選列表中的某個待辦事項時跳轉到 `DetailScreen` 介面，
同時能傳遞點選的這條代辦事項物件（`Todo` 物件） 。

To capture the user's tap in the `TodosScreen`, write an [`onTap()`][]
callback for the `ListTile` widget. Within the `onTap()` callback,
use the [`Navigator.push()`][] method.

想要獲取到使用者在 `TodosScreen` 的點選事件，
我們來編寫 `ListTile` widget 的 `onTap()` 回呼(Callback)函式，
繼續使用 [`Navigator.push()`][] 方法。

<?code-excerpt "lib/main.dart (builder)"?>
```dart
body: ListView.builder(
  itemCount: todos.length,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text(todos[index].title),
      // When a user taps the ListTile, navigate to the DetailScreen.
      // Notice that you're not only creating a DetailScreen, you're
      // also passing the current todo through to it.
      onTap: () {
        Navigator.push(
          context,
          MaterialPageRoute(
            builder: (context) => DetailScreen(todo: todos[index]),
          ),
        );
      },
    );
  },
),
```

### Interactive example

### 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

class Todo {
  final String title;
  final String description;

  const Todo(this.title, this.description);
}

void main() {
  runApp(
    MaterialApp(
      title: 'Passing Data',
      home: TodosScreen(
        todos: List.generate(
          20,
          (i) => Todo(
            'Todo $i',
            'A description of what needs to be done for Todo $i',
          ),
        ),
      ),
    ),
  );
}

class TodosScreen extends StatelessWidget {
  const TodosScreen({super.key, required this.todos});

  final List<Todo> todos;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Todos'),
      ),
      body: ListView.builder(
        itemCount: todos.length,
        itemBuilder: (context, index) {
          return ListTile(
            title: Text(todos[index].title),
            // When a user taps the ListTile, navigate to the DetailScreen.
            // Notice that you're not only creating a DetailScreen, you're
            // also passing the current todo through to it.
            onTap: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => DetailScreen(todo: todos[index]),
                ),
              );
            },
          );
        },
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  // In the constructor, require a Todo.
  const DetailScreen({super.key, required this.todo});

  // Declare a field that holds the Todo.
  final Todo todo;

  @override
  Widget build(BuildContext context) {
    // Use the Todo to create the UI.
    return Scaffold(
      appBar: AppBar(
        title: Text(todo.title),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Text(todo.description),
      ),
    );
  }
}
```

## Alternatively, pass the arguments using RouteSettings

## 或者使用 RouteSettings 傳遞引數

Repeat the first two steps.

重複前面兩個步驟。

### Create a detail screen to extract the arguments

### 建立一個詳情頁以提取引數

Next, create a detail screen that extracts and displays
the title and description from the `Todo`. 
To access the `Todo`, use the `ModalRoute.of()` method.
This method returns the current route with the arguments.

接下來，建立一個詳情頁用於提取並顯示來自 `Todo` 頁面的標題和描述資訊。
為了存取 `Todo` 頁面，請使用 `ModalRoute.of()` 方法。
它將會返回帶有引數的當前路由。

<?code-excerpt "lib/main_routesettings.dart (DetailScreen)"?>
```dart
class DetailScreen extends StatelessWidget {
  const DetailScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final todo = ModalRoute.of(context)!.settings.arguments as Todo;

    // Use the Todo to create the UI.
    return Scaffold(
      appBar: AppBar(
        title: Text(todo.title),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Text(todo.description),
      ),
    );
  }
}
```

### Navigate and pass the arguments to the detail screen

### 導航並向詳情頁傳遞引數

Finally, navigate to the `DetailScreen` when a user taps
a `ListTile` widget using `Navigator.push()`.
Pass the arguments as part of the [`RouteSettings`][].
The `DetailScreen` extracts these arguments.

最後，當用戶點選 `ListTile` widget 時，
使用 `Navigator.push()` 導航到 `DetailScreen`。
將引數作為 [`RouteSettings`][] 的一部分進行傳遞，
`DetailScreen` 將會提取這些引數。

<?code-excerpt "lib/main_routesettings.dart (builder)" replace="/^body: //g;/,$//g"?>
```dart
ListView.builder(
  itemCount: todos.length,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text(todos[index].title),
      // When a user taps the ListTile, navigate to the DetailScreen.
      // Notice that you're not only creating a DetailScreen, you're
      // also passing the current todo through to it.
      onTap: () {
        Navigator.push(
          context,
          MaterialPageRoute(
            builder: (context) => const DetailScreen(),
            // Pass the arguments as part of the RouteSettings. The
            // DetailScreen reads the arguments from these settings.
            settings: RouteSettings(
              arguments: todos[index],
            ),
          ),
        );
      },
    );
  },
)
```

### Complete example

### 完整範例

<?code-excerpt "lib/main_routesettings.dart"?>
```dart
import 'package:flutter/material.dart';

class Todo {
  final String title;
  final String description;

  const Todo(this.title, this.description);
}

void main() {
  runApp(
    MaterialApp(
      title: 'Passing Data',
      home: TodosScreen(
        todos: List.generate(
          20,
          (i) => Todo(
            'Todo $i',
            'A description of what needs to be done for Todo $i',
          ),
        ),
      ),
    ),
  );
}

class TodosScreen extends StatelessWidget {
  const TodosScreen({super.key, required this.todos});

  final List<Todo> todos;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Todos'),
      ),
      body: ListView.builder(
        itemCount: todos.length,
        itemBuilder: (context, index) {
          return ListTile(
            title: Text(todos[index].title),
            // When a user taps the ListTile, navigate to the DetailScreen.
            // Notice that you're not only creating a DetailScreen, you're
            // also passing the current todo through to it.
            onTap: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => const DetailScreen(),
                  // Pass the arguments as part of the RouteSettings. The
                  // DetailScreen reads the arguments from these settings.
                  settings: RouteSettings(
                    arguments: todos[index],
                  ),
                ),
              );
            },
          );
        },
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  const DetailScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final todo = ModalRoute.of(context)!.settings.arguments as Todo;

    // Use the Todo to create the UI.
    return Scaffold(
      appBar: AppBar(
        title: Text(todo.title),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Text(todo.description),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/passing-data.gif" alt="Passing Data Demo" class="site-mobile-screenshot" />
</noscript>


[`ModalRoute.of()`]: {{site.api}}/flutter/widgets/ModalRoute/of.html
[`Navigator.push()`]: {{site.api}}/flutter/widgets/Navigator/push.html
[`onTap()`]: {{site.api}}/flutter/material/ListTile/onTap.html
[`RouteSettings`]: {{site.api}}/flutter/widgets/RouteSettings-class.html
[Use lists]: {{site.url}}/cookbook/lists/basic-list
