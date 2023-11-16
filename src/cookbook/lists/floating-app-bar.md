---
title: Place a floating app bar above a list
title: 在列表頂部放置一個浮動的 app bar
description: How to place a floating app bar above a list.
description: 如何在列表頂部放置一個浮動的 app bar。
tags: cookbook, 實用課程, 列表相關
keywords: 列表客製,頂部,搜尋框,固定,隱藏搜尋框
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/lists/floating_app_bar/"?>

To make it easier for users to view a list of items,
you might want to hide the app bar as the user scrolls down the list.
This is especially true if your app displays a "tall"
app bar that occupies a lot of vertical space.

為了方便使用者檢視列表，你可能希望在使用者向下滾動列表時隱藏 app bar，
尤其在你的 app bar 特別高，導致它佔據了很多豎向空間的時候。

Typically, you create an app bar by providing an `appBar` property to the
`Scaffold` widget. This creates a fixed app bar that always remains above
the `body` of the `Scaffold`.

一般情況下，你可以透過給 `Scaffold` 元件設定一個 `appBar` 屬性來
建立一個 app bar。這個 app bar 會始終固定在 `Scaffold` 元件的 `body` 上方。

Moving the app bar from a `Scaffold` widget into a
[`CustomScrollView`][] allows you to create an app bar
that scrolls offscreen as you scroll through a
list of items contained inside the `CustomScrollView`.

把 app bar 從 `Scaffold` 元件挪到一個 [`CustomScrollView`][] 裡，
可以讓你建立一個隨著你滑動 `CustomScrollView`
裡列表的同時在螢幕外自動隨之滾動的 app bar。

This recipe demonstrates how to use a `CustomScrollView` to display a list of
items with an app bar on top that scrolls offscreen as the user scrolls
down the list using the following steps:

下面這篇課程將介紹如何透過 `CustomScrollView` 
來產生一個帶有隨著使用者滑動列表同時會在螢幕外隨之滾動的 app bar 的列表。

  1. Create a `CustomScrollView`.

     建立一個 `CustomScrollView`

  2. Use `SliverAppBar` to add a floating app bar.

     透過 `SliverAppBar` 來新增一個浮動的 app bar

  3. Add a list of items using a `SliverList`.

     透過 `SliverList` 來新增列表

## 1. Create a `CustomScrollView`

## 1. 建立一個 `CustomScrollView`

To create a floating app bar, place the app bar inside a
`CustomScrollView` that also contains the list of items.
This synchronizes the scroll position of the app bar and the list of items.
You might think of the `CustomScrollView` widget as a `ListView`
that allows you to mix and match different types of scrollable lists
and widgets together.

要建立一個浮動的 app bar，你需要將 app bar 
放在一個包含列表的 `CustomScrollView` 裡，
這會同步 app bar 和列表的滾動位置。
你可以把 `CustomScrollView` 元件當成一個
可以讓你把不同型別的可滾動列表和元件混合匹配在一起的 `ListView`。

The scrollable lists and widgets provided to the
`CustomScrollView` are known as _slivers_. There are several types
of slivers, such as `SliverList`, `SliverGridList`, and `SliverAppBar`.
In fact, the `ListView` and `GridView` widgets use the `SliverList` and
`SliverGrid` widgets to implement scrolling.

可以放在 `CustomScrollView` 裡的可滾動列表和元件
我們稱之為 **slivers**。有幾種型別的 slivers，
比如 `SliverList`、`SliverGridList` 和 `SliverAppBar`。
實際上，`ListView` 和 `GridView` 元件底層
使用的就是 `SliverList` 和 `SliverGrid`。

For this example, create a `CustomScrollView` that contains a
`SliverAppBar` and a `SliverList`. In addition, remove any app bars
that you provide to the `Scaffold` widget.

以下例子示範了建立一個包含 `SliverAppBar` 和 `SliverList`
的 `CustomScrollView`。
另外你需要刪除你之前可能設定在 `Scaffold` 元件上的 app bar！

<?code-excerpt "lib/starter.dart (CustomScrollView)" replace="/^return //g"?>
```dart
Scaffold(
  // No appBar property provided, only the body.
  body: CustomScrollView(
      // Add the app bar and list of items as slivers in the next steps.
      slivers: <Widget>[]),
);
```

### 2. Use `SliverAppBar` to add a floating app bar

### 2. 使用 `SliverAppBar` 來新增一個浮動的 app bar

Next, add an app bar to the [`CustomScrollView`][].
Flutter provides the [`SliverAppBar`][] widget which,
much like the normal `AppBar` widget,
uses the `SliverAppBar` to display a title,
tabs, images and more.

接下來為 [`CustomScrollView`][] 新增一個 app bar。
Flutter 提供開箱即用的 [`SliverAppBar`][] 元件，
與普通的 `AppBar` 元件非常相似，
你可以使用 `SliverAppBar` 來顯示標題、標籤、圖像等內容。

However, the `SliverAppBar` also gives you the ability to create a "floating"
app bar that scrolls offscreen as the user scrolls down the list.
Furthermore, you can configure the `SliverAppBar` to shrink and
expand as the user scrolls.

同時，`SliverAppBar` 元件也提供一種建立 “浮動” app bar 的能力，
當用戶向下滾動列表時，app bar 會隨之在螢幕外滾動。
此外，你可以配置 `SliverAppBar` 在使用者滾動時縮小或展開。

To create this effect:

要達到這個效果：

  1. Start with an app bar that displays only a title.

     先建立一個只顯示標題的 app bar
    
  2. Set the `floating` property to `true`.
     This allows users to quickly reveal the app bar when
     they scroll up the list.
     
     將 `floating` 屬性設為 `true`，
     這使使用者在向上滾動列表時能快速顯示 app bar。

  3. Add a `flexibleSpace` widget that fills the available
     `expandedHeight`.

     新增一個 `flexibleSpace` 元件，
     這個元件將填充可用的 `expandedHeight`。

<?code-excerpt "lib/step2.dart (SliverAppBar)" replace="/^body: //g;/,$//g"?>
```dart
CustomScrollView(
  slivers: [
    // Add the app bar to the CustomScrollView.
    const SliverAppBar(
      // Provide a standard title.
      title: Text(title),
      // Allows the user to reveal the app bar if they begin scrolling
      // back up the list of items.
      floating: true,
      // Display a placeholder widget to visualize the shrinking size.
      flexibleSpace: Placeholder(),
      // Make the initial height of the SliverAppBar larger than normal.
      expandedHeight: 200,
    ),
  ],
)
```

{{site.alert.tip}}

  Play around with the
  [various properties you can pass to the `SliverAppBar` widget][],
  and use hot reload to see the results. For example, use an `Image`
  widget for the `flexibleSpace` property to create a background image that
  shrinks in size as it's scrolled offscreen.
  
  試試 [`SliverAppBar` 支援的各種屬性][various properties you can pass to the `SliverAppBar` widget]，
  並使用熱重載來檢視結果。例如，你可以給 `flexibleSpace`
  提供一個 `Image` widget 來建立一個背景圖像，當它在螢幕外滾動時會縮小尺寸。
{{site.alert.end}}


### 3. Add a list of items using a `SliverList`

### 3. 使用 `SliverList` 來新增一個列表

Now that you have the app bar in place, add a list of items to the
`CustomScrollView`. You have two options: a [`SliverList`][]
or a [`SliverGrid`][].  If you need to display a list of items one after the other,
use the `SliverList` widget.
If you need to display a grid list, use the `SliverGrid` widget.

現在你已經建立好一個 app bar，接下來應該給 `CustomScrollView` 新增一個列表。
你有兩種選擇：選擇 [`SliverList`][] 或者 [`SliverGrid`][]。
如果你需要一個一個往下排地顯示列表中的內容，應該用 `SliverList` 元件。
如果需要網格狀地顯示列表中的內容，應該用 `SliverGrid` 元件。

The `SliverList` and `SliverGrid` widgets take one required parameter: a
[`SliverChildDelegate`][], which provides a list
of widgets to `SliverList` or `SliverGrid`.
For example, the [`SliverChildBuilderDelegate`][]
allows you to create a list of items that are built lazily as you scroll,
just like the `ListView.builder` widget.

`SliverList` 和 `SliverGrid` 元件都需要一個必要引數：[`SliverChildDelegate`][]。
雖然聽起來很花哨，但它只是用來給列表元件
`SliverList` 或 `SliverGrid` 提供一個代理。
例如，[`SliverChildBuilderDelegate`][] 允許你建立一組
可以在滾動時延遲載入的列表項，就和 `ListView.builder` 元件差不多。

<?code-excerpt "lib/main.dart (SliverList)" replace="/,$//g"?>
```dart
// Next, create a SliverList
SliverList(
  // Use a delegate to build items as they're scrolled on screen.
  delegate: SliverChildBuilderDelegate(
    // The builder function returns a ListTile with a title that
    // displays the index of the current item.
    (context, index) => ListTile(title: Text('Item #$index')),
    // Builds 1000 ListTiles
    childCount: 1000,
  ),
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
    const title = 'Floating App Bar';

    return MaterialApp(
      title: title,
      home: Scaffold(
        // No appbar provided to the Scaffold, only a body with a
        // CustomScrollView.
        body: CustomScrollView(
          slivers: [
            // Add the app bar to the CustomScrollView.
            const SliverAppBar(
              // Provide a standard title.
              title: Text(title),
              // Allows the user to reveal the app bar if they begin scrolling
              // back up the list of items.
              floating: true,
              // Display a placeholder widget to visualize the shrinking size.
              flexibleSpace: Placeholder(),
              // Make the initial height of the SliverAppBar larger than normal.
              expandedHeight: 200,
            ),
            // Next, create a SliverList
            SliverList(
              // Use a delegate to build items as they're scrolled on screen.
              delegate: SliverChildBuilderDelegate(
                // The builder function returns a ListTile with a title that
                // displays the index of the current item.
                (context, index) => ListTile(title: Text('Item #$index')),
                // Builds 1000 ListTiles
                childCount: 1000,
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/floating-app-bar.gif" alt="Use list demo" class="site-mobile-screenshot"/> 
</noscript>


[`CustomScrollView`]: {{site.api}}/flutter/widgets/CustomScrollView-class.html
[`SliverAppBar`]: {{site.api}}/flutter/material/SliverAppBar-class.html
[`SliverChildBuilderDelegate`]: {{site.api}}/flutter/widgets/SliverChildBuilderDelegate-class.html
[`SliverChildDelegate`]: {{site.api}}/flutter/widgets/SliverChildDelegate-class.html
[`SliverGrid`]: {{site.api}}/flutter/widgets/SliverGrid-class.html
[`SliverList`]: {{site.api}}/flutter/widgets/SliverList-class.html
[various properties you can pass to the `SliverAppBar` widget]: {{site.api}}/flutter/material/SliverAppBar/SliverAppBar.html
