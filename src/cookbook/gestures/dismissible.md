---
title: Implement swipe to dismiss
title: 實現「滑動清除」效果
description: How to implement swiping to dismiss or delete.
description: 如何實現滑動取消或清除效果。
tags: cookbook, 實用課程, 手勢
keywords: 滑動清除
diff2html: true
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/gestures/dismissible"?>

The "swipe to dismiss" pattern is common in many mobile apps.
For example, when writing an email app,
you might want to allow a user to swipe away
email messages to delete them from a list.

“滑動清除”在許多移動應用中都很常見。
比如，我們在寫一個郵件應用，我們會想讓使用者能夠滑動刪除列表中的郵件訊息。
使用者操作時，我們可能需要把這封郵件從收件箱移動到垃圾箱。

Flutter makes this task easy by providing the
[`Dismissible`][] widget.
Learn how to implement swipe to dismiss with the following steps:

Flutter 提供了 [`Dismissible`][] Widget 來輕鬆地實現這個需求。
我們一起看一下如下的步驟：

## Directions

## 步驟

  1. Create a list of items.

     建立專案列表

  2. Wrap each item in a `Dismissible` widget.

     把每一項打包成一個 `Dismissible` Widget

  3. Provide "leave behind" indicators.

     提供“滯留”提示

## 1. Create a list of items

## 1. 建立專案列表

First, create a list of items. For detailed
instructions on how to create a list,
follow the [Working with long lists][] recipe.

首先，我們建立一個列表，列表項是能夠滑動清除的。
至於如何建立列表的更多細節，請參考
[長列表的處理][Working with long lists] 文件。

### Create a data source

### 建立一個數據源

In this example,
you want 20 sample items to work with.
To keep it simple, generate a list of strings.

在我們的例子中，我們需要 20 個樣本項來實現列表。
為簡單起見，我們會產生一個字串列表。

<?code-excerpt "lib/main.dart (Items)"?>
```dart
final items = List<String>.generate(20, (i) => 'Item ${i + 1}');
```

### Convert the data source into a list

### 將資料源轉換成一個 List

Display each item in the list on screen. Users won't
be able to swipe these items away just yet.

首先，我們簡單地在螢幕上展示列表中的每一項，
使用者現在還無法滑動清除它們。

<?code-excerpt "lib/step1.dart (ListView)" replace="/^body: //g;/,$//g"?>
```dart
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text(items[index]),
    );
  },
)
```

## 2. Wrap each item in a Dismissible widget

## 2. 把每一項打包一個 Dismissible Widget

In this step,
give users the ability to swipe an item off the list by using the
[`Dismissible`][] widget.

在這個步驟中，使用者可以透過使用 [`Dismissible`][] 來刪除列表中的某項。

After the user has swiped away the item,
remove the item from the list and display a snackbar.
In a real app, you might need to perform more complex logic,
such as removing the item from a web service or database.

在使用者將某一項滑出屏幕後，我們需要將那一項從列表中刪除並顯示一個 Snackbar。
在真實的應用中，你可能需要執行更復雜的邏輯，比如從網頁服務或資料庫中刪除此項。

Update the `itemBuilder()` function to return a `Dismissible` widget:

我們可以透過更新 `itemBuilder()` 函式來返回一個 `Dismissible` widget:

<?code-excerpt "lib/step2.dart (Dismissible)"?>
```dart
itemBuilder: (context, index) {
  final item = items[index];
  return Dismissible(
    // Each Dismissible must contain a Key. Keys allow Flutter to
    // uniquely identify widgets.
    key: Key(item),
    // Provide a function that tells the app
    // what to do after an item has been swiped away.
    onDismissed: (direction) {
      // Remove the item from the data source.
      setState(() {
        items.removeAt(index);
      });

      // Then show a snackbar.
      ScaffoldMessenger.of(context)
          .showSnackBar(SnackBar(content: Text('$item dismissed')));
    },
    child: ListTile(
      title: Text(item),
    ),
  );
},
```

## 3. Provide "leave behind" indicators

## 3. 提供“滯留”提示

As it stands,
the app allows users to swipe items off the list, but it doesn't
give a visual indication of what happens when they do.
To provide a cue that items are removed,
display a "leave behind" indicator as they
swipe the item off the screen. In this case,
the indicator is a red background.

顧名思義，我們的應用允許使用者將列表項滑出列表，
但是應用可能沒有向用戶給出視覺提示，告訴他們操作時發生了什麼。
要給出提示，表明我們正在刪除列表項，就需要在他們將列表項
滑出螢幕的時候，展示一個“滯留”提示。這個例子中，我們使用了一個紅色背景。

To add the indicator,
provide a `background` parameter to the `Dismissible`.

出於這個目的，我們為 `Dismissible`
設定了一個 `background` 引數。

<?code-excerpt "lib/{step2,main}.dart (Dismissible)"?>
```diff
--- lib/step2.dart (Dismissible)
+++ lib/main.dart (Dismissible)
@@ -16,6 +16,8 @@
       ScaffoldMessenger.of(context)
           .showSnackBar(SnackBar(content: Text('$item dismissed')));
     },
+    // Show a red background as the item is swiped away.
+    background: Container(color: Colors.red),
     child: ListTile(
       title: Text(item),
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

// MyApp is a StatefulWidget. This allows updating the state of the
// widget when an item is removed.
class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  MyAppState createState() {
    return MyAppState();
  }
}

class MyAppState extends State<MyApp> {
  final items = List<String>.generate(20, (i) => 'Item ${i + 1}');

  @override
  Widget build(BuildContext context) {
    const title = 'Dismissing Items';

    return MaterialApp(
      title: title,
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: Scaffold(
        appBar: AppBar(
          title: const Text(title),
        ),
        body: ListView.builder(
          itemCount: items.length,
          itemBuilder: (context, index) {
            final item = items[index];
            return Dismissible(
              // Each Dismissible must contain a Key. Keys allow Flutter to
              // uniquely identify widgets.
              key: Key(item),
              // Provide a function that tells the app
              // what to do after an item has been swiped away.
              onDismissed: (direction) {
                // Remove the item from the data source.
                setState(() {
                  items.removeAt(index);
                });

                // Then show a snackbar.
                ScaffoldMessenger.of(context)
                    .showSnackBar(SnackBar(content: Text('$item dismissed')));
              },
              // Show a red background as the item is swiped away.
              background: Container(color: Colors.red),
              child: ListTile(
                title: Text(item),
              ),
            );
          },
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/dismissible.gif" alt="Dismissible Demo" class="site-mobile-screenshot" />
</noscript>


[`Dismissible`]: {{site.api}}/flutter/widgets/Dismissible-class.html
[Working with long lists]: {{site.url}}/cookbook/lists/long-lists
