---
title: Work with long lists
title: 長列表的處理
description: Use ListView.builder to implement a long or infinite list.
description: 使用 ListView.builder 實現一個長或無限的列表。
tags: cookbook, 實用課程, 列表相關
keywords: 長列表,進階,資料源
prev:
  title: Place a floating app bar above a list
  title: 在列表頂部放置一個浮動的 app bar
  path: /docs/cookbook/lists/floating-app-bar
next:
  title: Report errors to a service
  title: 把報錯資訊透過服務上傳
  path: /docs/cookbook/maintenance/error-reporting
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/lists/long_lists/"?>

The standard [`ListView`][] constructor works well
for small lists. To work with lists that contain
a large number of items, it's best to use the
[`ListView.builder`][] constructor.

標準的 [`ListView`][] 建構函式適用於短列表，對於具有大量列表項的長列表，
需要用 [`ListView.builder`][] 建構函式來建立。

In contrast to the default `ListView` constructor, which requires
creating all items at once, the `ListView.builder()` constructor
creates items as they're scrolled onto the screen.

與標準的 `ListView` 建構函式需要一次性建立所有列表項不同的是，
`ListView.builder` 建構函式只在列表項從螢幕外滑入螢幕時才去建立列表項。

## 1. Create a data source

## 1. 建立資料源

First, you need a data source. For example, your data source
might be a list of messages, search results, or products in a store.
Most of the time, this data comes from the internet or a database.

首先，需要獲取列表的資料源。
例如，資料源可以是訊息集、搜尋結果集或者商店商品集。
大部分情況下，這些資料來自於網路請求或者資料庫獲取。

For this example, generate a list of 10,000 Strings using the
[`List.generate`][] constructor.

在下面的例子，使用 [`List.generate`][] 建構函式產生包含 10,000 個字串的集合。

<?code-excerpt "lib/main.dart (Items)" replace="/^items: //g"?>
```dart
List<String>.generate(10000, (i) => 'Item $i'),
```

## 2. Convert the data source into widgets

## 2. 將資料源渲染成元件

To display the list of strings, render each String as a widget
using `ListView.builder()`.

為了將字串集合展示出來，需要透過 `ListView.builder`
把集合中的每個字串都渲染成元件。

In this example, display each String on its own line.

在下面的例子中，將會把每個字串用單行列表項顯示在列表中。

<?code-excerpt "lib/main.dart (ListView)" replace="/^body: //g;/,$//g"?>
```dart
ListView.builder(
  itemCount: items.length,
  prototypeItem: ListTile(
    title: Text(items.first),
  ),
  itemBuilder: (context, index) {
    return ListTile(
      title: Text(items[index]),
    );
  },
)
```

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() {
  runApp(
    MyApp(
      items: List<String>.generate(10000, (i) => 'Item $i'),
    ),
  );
}

class MyApp extends StatelessWidget {
  final List<String> items;

  const MyApp({super.key, required this.items});

  @override
  Widget build(BuildContext context) {
    const title = 'Long List';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(title),
        ),
        body: ListView.builder(
          itemCount: items.length,
          prototypeItem: ListTile(
            title: Text(items.first),
          ),
          itemBuilder: (context, index) {
            return ListTile(
              title: Text(items[index]),
            );
          },
        ),
      ),
    );
  }
}
```

## Children's extent

To specify each item's extent, you can use either `itemExtent` or `prototypeItem`.
Specifying either is more efficient than letting the children determine their own extent
because the scrolling machinery can make use of the foreknowledge of the children's
extent to save work, for example when the scroll position changes drastically.

<noscript>
  <img src="/assets/images/docs/cookbook/long-lists.gif" alt="Long Lists Demo" class="site-mobile-screenshot" />
</noscript>

[`List.generate`]: {{site.api}}/flutter/dart-core/List/List.generate.html
[`ListView`]: {{site.api}}/flutter/widgets/ListView-class.html
[`ListView.builder`]: {{site.api}}/flutter/widgets/ListView/ListView.builder.html
