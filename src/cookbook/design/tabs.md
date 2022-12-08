---
title: Work with tabs
title: 使用 tabs
description: How to implement tabs in a layout.
description: 如何實現 tab 佈局。
tags: cookbook, 實用課程, 設計
keywords: Material Design 效果, 標籤頁佈局, tabs
prev:
  title: Use themes to share colors and font styles
  title: 使用 Themes 統一顏色和字型風格
  path: /docs/cookbook/design/themes
next:
  title: Create a download button
  title: 建立一個下載按鈕
  path: /docs/cookbook/effects/download-button
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/design/tabs/"?>

Working with tabs is a common pattern in apps that follow the
Material Design guidelines.
Flutter includes a convenient way to create tab layouts as part of
the [material library][].

在 Material Design 設計準則裡，tabs 是一種常用的佈局模型。
Flutter 自帶的 [Material 庫][material library] 
可以幫助開發者們非常便捷的建立 tab 佈局。

{{site.alert.note}}

  To create tabs in a Cupertino app, see the
  [Building a Cupertino app with Flutter][] codelab.
  
  建立一個使用 tabs 佈局、Cupertino 風格的 iOS 應用，
  請參見這個 codelab:
  [使用 Flutter 建構 iOS 風格的應用][Building a Cupertino app with Flutter]。
  
{{site.alert.end}}

This recipe creates a tabbed example using the following steps;

這份課程將幫助你建立一個 tabs 佈局範例，請參見如下步驟：

## Directions

## 步驟

  1. Create a `TabController`.
     
     建立 `TabController`
  
  2. Create the tabs.
    
     建立 tabs
     
  3. Create content for each tab.
    
     為每個 tab 建立內容

## 1. Create a `TabController`

## 1. 建立 `TabController`

For tabs to work, you need to keep the selected tab and content
sections in sync.
This is the job of the [`TabController`][].

為了使所選的 tab 與它所對應的內容能夠同步變化，
需要用 [`TabController`][] 進行控制。

Either create a `TabController` manually,
or automatically by using a [`DefaultTabController`][] widget.

我們既可以手動建立一個 `TabController` ，也能夠直接使用
[`DefaultTabController`][] widget。

Using `DefaultTabController` is the simplest option, since it
creates a `TabController` and makes it available to all descendant widgets.

最簡單的選擇是使用 `DefaultTabController` widget，
因為它能夠創建出一個可供所有子 widgets 使用的 `TabController`。

<?code-excerpt "lib/partials.dart (TabController)"?>
```dart
return MaterialApp(
  home: DefaultTabController(
    length: 3,
    child: Scaffold(),
  ),
);
```

## 2. Create the tabs

## 2. 建立 tabs 

When a tab is selected, it needs to display content.
You can create tabs using the [`TabBar`][] widget.
In this example, create a `TabBar` with three
[`Tab`][] widgets and place it within an [`AppBar`][].

現在我們已經成功建立了 `TabController`，
接下來就可以用 [`TabBar`][] widget
來建立 tabs。下面這個範例建立了包含三組
[`Tab`][] widget 的 `TabBar`（一個），
並把它放置於 [`AppBar`][] widget 中。

<?code-excerpt "lib/partials.dart (Tabs)"?>
```dart
return MaterialApp(
  home: DefaultTabController(
    length: 3,
    child: Scaffold(
      appBar: AppBar(
        bottom: const TabBar(
          tabs: [
            Tab(icon: Icon(Icons.directions_car)),
            Tab(icon: Icon(Icons.directions_transit)),
            Tab(icon: Icon(Icons.directions_bike)),
          ],
        ),
      ),
    ),
  ),
);
```

By default, the `TabBar` looks up the widget tree for the nearest
`DefaultTabController`. If you're manually creating a `TabController`,
pass it to the `TabBar`.

`TabBar` 預設將會在 Widget 樹中向上尋找離它最近的一個
`DefaultTabController` 節點作為自己的 `TabController`。
如果您想手動建立 `TabController`，
那麼您必須將它作為引數傳給 `TabBar`。

## 3. Create content for each tab

## 3. 為每個 tab 建立內容

Now that you have tabs, display content when a tab is selected.
For this purpose, use the [`TabBarView`][] widget.

現在我們已經成功建立了一些 tabs，
接下來要實現的是 tab 被選中時顯示其對應的內容。
為了實現這個效果，我們將使用
[`TabBarView`][] widget。

{{site.alert.note}}

  Order is important and must correspond to the order of the tabs in the
  `TabBar`.
  
  `TabBarView` 中內容的順序很重要，它必須與 `TabBar` 中 tab 的順序相對應！

{{site.alert.end}}

<?code-excerpt "lib/main.dart (TabBarView)"?>
```dart
body: const TabBarView(
  children: [
    Icon(Icons.directions_car),
    Icon(Icons.directions_transit),
    Icon(Icons.directions_bike),
  ],
),
```

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() {
  runApp(const TabBarDemo());
}

class TabBarDemo extends StatelessWidget {
  const TabBarDemo({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: DefaultTabController(
        length: 3,
        child: Scaffold(
          appBar: AppBar(
            bottom: const TabBar(
              tabs: [
                Tab(icon: Icon(Icons.directions_car)),
                Tab(icon: Icon(Icons.directions_transit)),
                Tab(icon: Icon(Icons.directions_bike)),
              ],
            ),
            title: const Text('Tabs Demo'),
          ),
          body: const TabBarView(
            children: [
              Icon(Icons.directions_car),
              Icon(Icons.directions_transit),
              Icon(Icons.directions_bike),
            ],
          ),
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/tabs.gif" alt="Tabs Demo" class="site-mobile-screenshot" />
</noscript>


[`AppBar`]: {{site.api}}/flutter/material/AppBar-class.html
[Building a Cupertino app with Flutter]: {{site.codelabs}}/codelabs/flutter-cupertino
[`DefaultTabController`]: {{site.api}}/flutter/material/DefaultTabController-class.html
[material library]: {{site.api}}/flutter/material/material-library.html
[`Tab`]: {{site.api}}/flutter/material/Tab-class.html
[`TabBar`]: {{site.api}}/flutter/material/TabBar-class.html
[`TabBarView`]: {{site.api}}/flutter/material/TabBarView-class.html
[`TabController`]: {{site.api}}/flutter/material/TabController-class.html
