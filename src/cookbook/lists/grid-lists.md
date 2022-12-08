---
title: Create a grid list
title: 建立一個網格列表
description: How to implement a grid list.
description: 如何實現一個網格列表。
prev:
  title: Create a horizontal list
  title: 建立一個水平滑動的列表
  path: /docs/cookbook/lists/horizontal-list
next:
  title: Create lists with different types of items
  title: 建立擁有不同列表項的列表
  path: /docs/cookbook/lists/mixed-list
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/lists/grid_lists"?>

In some cases, you might want to display your items as a grid rather than
a normal list of items that come one after the next.
For this task, use the [`GridView`][] widget.

有時候，你可能希望用網格來展示內容，而不是一條接著一條的普通列表來展示。
在本文當中，我們將採用 [`GridView`][] widget。

The simplest way to get started using grids is by using the
[`GridView.count()`][] constructor,
because it allows you to specify how many rows or columns you'd like.

用網格展示資料最簡單的方式，
就是透過使用 [`GridView.count()`][] 構造方法，
因為它允許我們指定有多少行多少列。

To visualize how `GridView` works, 
generate a list of 100 widgets that display their index in the list.

為了幫助我們想象 `GridView` 是如何工作的，
在這個例子中，我們將建立一個包含有 100 個 widget 的 List，
每個 Widget 將展示它在 List 中的索引。

<?code-excerpt "lib/main.dart (GridView)" replace="/^body\: //g"?>
```dart
GridView.count(
  // Create a grid with 2 columns. If you change the scrollDirection to
  // horizontal, this produces 2 rows.
  crossAxisCount: 2,
  // Generate 100 widgets that display their index in the List.
  children: List.generate(100, (index) {
    return Center(
      child: Text(
        'Item $index',
        style: Theme.of(context).textTheme.headline5,
      ),
    );
  }),
),
```

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    const title = 'Grid List';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(title),
        ),
        body: GridView.count(
          // Create a grid with 2 columns. If you change the scrollDirection to
          // horizontal, this produces 2 rows.
          crossAxisCount: 2,
          // Generate 100 widgets that display their index in the List.
          children: List.generate(100, (index) {
            return Center(
              child: Text(
                'Item $index',
                style: Theme.of(context).textTheme.headline5,
              ),
            );
          }),
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/grid-list.gif" alt="Grid List Demo" class="site-mobile-screenshot" />
</noscript>

[`GridView`]: {{site.api}}/flutter/widgets/GridView-class.html
[`GridView.count()`]: {{site.api}}/flutter/widgets/GridView/GridView.count.html
