---
title: Create lists with different types of items
title: 建立擁有不同列表項的列表
description: How to implement a list that contains different types of assets.
description: 如何實現一個包含不同型別資源的列表。
tags: cookbook, 實用課程, 列表相關
keywords: 列表進階
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/lists/mixed_list/"?>

You might need to create lists that display different types of content.
For example, you might be working on a list that shows a heading
followed by a few items related to the heading, followed by another heading,
and so on.

我們經常需要建立展示不同型別內容的列表。
比方說，我們可能在開發一個列表，它顯示一個標題，
後跟一些與標題相關的專案，然後是另一個標題，依此類推。

Here's how you can create such a structure with Flutter:

你可以透過以下步驟，用 Flutter 建立這樣的結構：

  1. Create a data source with different types of items.

     建立一個擁有不同型別專案的資料源

  2. Convert the data source into a list of widgets.

     將資料源的資料轉換成列表 widget

## 1. Create a data source with different types of items

## 1. 建立一個具有不同型別專案的資料源

### Types of items

### 專案的型別

To represent different types of items in a list, define
a class for each type of item.

為了表示 List 中不同型別的項，我們需要為每一個類別型的專案定義一個類別。

In this example, create an app that shows a header followed by five
messages. Therefore, create three classes: `ListItem`, `HeadingItem`,
and `MessageItem`.

在這個例子中，我們將製作一個展示了標題，後面有五條訊息的應用。
因此，我們將建立三個類：`ListItem`、`HeadingItem` 和 `MessageItem`。

<?code-excerpt "lib/main.dart (ListItem)"?>
```dart
/// The base class for the different types of items the list can contain.
abstract class ListItem {
  /// The title line to show in a list item.
  Widget buildTitle(BuildContext context);

  /// The subtitle line, if any, to show in a list item.
  Widget buildSubtitle(BuildContext context);
}

/// A ListItem that contains data to display a heading.
class HeadingItem implements ListItem {
  final String heading;

  HeadingItem(this.heading);

  @override
  Widget buildTitle(BuildContext context) {
    return Text(
      heading,
      style: Theme.of(context).textTheme.headlineSmall,
    );
  }

  @override
  Widget buildSubtitle(BuildContext context) => const SizedBox.shrink();
}

/// A ListItem that contains data to display a message.
class MessageItem implements ListItem {
  final String sender;
  final String body;

  MessageItem(this.sender, this.body);

  @override
  Widget buildTitle(BuildContext context) => Text(sender);

  @override
  Widget buildSubtitle(BuildContext context) => Text(body);
}
```

### Create a list of items

### 建立專案的 List

Most of the time, you would fetch data from the internet or a local
database and convert that data into a list of items.

大部分時候，我們從網路或本地資料庫獲取資料，並將資料轉換成一個專案列表。

For this example, generate a list of items to work with. The list
contains a header followed by five messages. Each message has one
of 3 types: `ListItem`, `HeadingItem`, or `MessageItem`.

對於這個例子來說，我們將產生一個要使用的專案列表。
這個列表將包含一個標題，後跟五條訊息。
每條訊息都屬於以下三種類型中的一種：
`ListItem`、`HeadingItem`，或者是 `MessageItem`。

<?code-excerpt "lib/main.dart (Items)" replace="/^items:/final items =/g;/,$/;/g"?>
```dart
final items = List<ListItem>.generate(
  1000,
  (i) => i % 6 == 0
      ? HeadingItem('Heading $i')
      : MessageItem('Sender $i', 'Message body $i'),
);
```

## 2. Convert the data source into a list of widgets

## 2. 將資料源的資料轉換成列表 widget

To convert each item into a widget,
use the [`ListView.builder()`][] constructor.

為了把每一個專案轉換成 widget，
我們將採用 [`ListView.builder()`][] 構造方法。

In general, provide a builder function that checks for what type
of item you're dealing with, and returns the appropriate widget
for that type of item.

通常，我們需要提供一個 builder 函式來確定我們正在處理的專案型別，
並返回該型別專案的相應 widget。

<?code-excerpt "lib/main.dart (builder)" replace="/^body: //g;/,$//g"?>
```dart
ListView.builder(
  // Let the ListView know how many items it needs to build.
  itemCount: items.length,
  // Provide a builder function. This is where the magic happens.
  // Convert each item into a widget based on the type of item it is.
  itemBuilder: (context, index) {
    final item = items[index];

    return ListTile(
      title: item.buildTitle(context),
      subtitle: item.buildSubtitle(context),
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
      items: List<ListItem>.generate(
        1000,
        (i) => i % 6 == 0
            ? HeadingItem('Heading $i')
            : MessageItem('Sender $i', 'Message body $i'),
      ),
    ),
  );
}

class MyApp extends StatelessWidget {
  final List<ListItem> items;

  const MyApp({super.key, required this.items});

  @override
  Widget build(BuildContext context) {
    const title = 'Mixed List';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(title),
        ),
        body: ListView.builder(
          // Let the ListView know how many items it needs to build.
          itemCount: items.length,
          // Provide a builder function. This is where the magic happens.
          // Convert each item into a widget based on the type of item it is.
          itemBuilder: (context, index) {
            final item = items[index];

            return ListTile(
              title: item.buildTitle(context),
              subtitle: item.buildSubtitle(context),
            );
          },
        ),
      ),
    );
  }
}

/// The base class for the different types of items the list can contain.
abstract class ListItem {
  /// The title line to show in a list item.
  Widget buildTitle(BuildContext context);

  /// The subtitle line, if any, to show in a list item.
  Widget buildSubtitle(BuildContext context);
}

/// A ListItem that contains data to display a heading.
class HeadingItem implements ListItem {
  final String heading;

  HeadingItem(this.heading);

  @override
  Widget buildTitle(BuildContext context) {
    return Text(
      heading,
      style: Theme.of(context).textTheme.headlineSmall,
    );
  }

  @override
  Widget buildSubtitle(BuildContext context) => const SizedBox.shrink();
}

/// A ListItem that contains data to display a message.
class MessageItem implements ListItem {
  final String sender;
  final String body;

  MessageItem(this.sender, this.body);

  @override
  Widget buildTitle(BuildContext context) => Text(sender);

  @override
  Widget buildSubtitle(BuildContext context) => Text(body);
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/mixed-list.png" alt="Mixed list demo" class="site-mobile-screenshot" />
</noscript>


[`ListView.builder()`]: {{site.api}}/flutter/widgets/ListView/ListView.builder.html
