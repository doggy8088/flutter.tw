---
title: Differentiate between ephemeral state and app state
title: 短時 (ephemeral) 和應用 (app) 狀態的區別
short-title: Ephemeral versus app state
short-title: 區分短時和共享狀態
description: How to tell the difference between ephemeral and app state.
description: 介紹短時 (ephemeral) 和應用 (app) 狀態的區別有哪些。
tags: Flutter狀態管理
keywords: 理論,狀態管理思想,介面,動畫狀態,紋理,字型
prev:
  title: Start thinking declaratively
  title: 宣告式的程式設計思維
  path: /docs/development/data-and-backend/state-mgmt/declarative
next:
  title: Simple app state management
  title: 簡單的應用 (app) 狀態管理
  path: /docs/development/data-and-backend/state-mgmt/simple
---

_This doc introduces app state, ephemeral state,
and how you might manage each in a Flutter app._
 
**本文將介紹應用 (app) 狀態、短時 (ephemeral) 狀態，
以及在一個 Flutter 應用中你可以如何應用這兩種狀態。**

In the broadest possible sense, the state of an app is everything that
exists in memory when the app is running. This includes the app's assets,
all the variables that the Flutter framework keeps about the UI,
animation state, textures, fonts, and so on. While this broadest
possible definition of state is valid, it's not very useful for
architecting an app.

廣義上來講，一個應用的狀態就是當這個應用執行時存在於記憶體中的所有內容。
這包括了應用中用到的資源，所有 Flutter 框架中
有關使用者介面、動畫狀態、紋理、字型以及其他等等的變數。
這個對於狀態廣義的定義是有效的，但是它對於建構一個應用來說並不是很有用。

First, you don't even manage some state (like textures).
The framework handles those for you. So a more useful definition of
state is "whatever data you need in order to rebuild your UI at any
moment in time". Second, the state that you _do_ manage yourself can
be separated into two conceptual types: ephemeral state and app state.

首先，你不需要管理一些狀態（例如紋理），框架本身會替你管理。
所以對於狀態的更有用的定義是
“當任何時候你需要重建你的使用者介面時你所需要的資料”。
其次，你需要自己 **管理** 的狀態可以分為兩種概念型別：
短時 (ephemeral) 狀態和應用 (app) 狀態。

## Ephemeral state

## 短時狀態

Ephemeral state (sometimes called _UI state_ or _local state_)
is the state you can neatly contain in a single widget.

短時狀態（有時也稱 **使用者介面 (UI) 狀態** 或者 **區域性狀態**） 
是你可以完全包含在一個獨立 widget 中的狀態。

This is, intentionally, a vague definition, so here are a few examples. 

這是一個有點兒模糊的定義，這裡有幾個例子。

* current page in a [`PageView`][]

  一個 [`PageView`][] 元件中的當前頁面

* current progress of a complex animation

  一個複雜動畫中當前進度

* current selected tab in a `BottomNavigationBar`

  一個 `BottomNavigationBar` 中當前被選中的 tab

Other parts of the widget tree seldom need to access this kind of state.
There is no need to serialize it, and it doesn't change in complex ways.

widget 樹中其他部分不需要存取這種狀態。
不需要去序列化這種狀態，這種狀態也不會以複雜的方式改變。

In other words, there is no need to use state management techniques
(ScopedModel, Redux, etc.) on this kind of state.
All you need is a `StatefulWidget`.

換句話說，不需要使用狀態管理架構（例如 ScopedModel, Redux）去管理這種狀態。
你需要用的只是一個 `StatefulWidget`。

Below, you see how the currently selected item in a bottom navigation bar is
held in the `_index` field of the `_MyHomepageState` class.
In this example, `_index` is ephemeral state.

在下方你可以看到一個底部導航欄中當前被選中的專案是
如何被被儲存在 `_MyHomepageState` 類別的 `_index` 變數中。
在這個例子中，`_index` 是一個短時狀態。

<?code-excerpt "state_mgmt/simple/lib/src/set_state.dart (Ephemeral)" plaster="// ... items ..."?>
```dart
class MyHomepage extends StatefulWidget {
  const MyHomepage({super.key});

  @override
  State<MyHomepage> createState() => _MyHomepageState();
}

class _MyHomepageState extends State<MyHomepage> {
  int _index = 0;

  @override
  Widget build(BuildContext context) {
    return BottomNavigationBar(
      currentIndex: _index,
      onTap: (newIndex) {
        setState(() {
          _index = newIndex;
        });
      },
      // ... items ...
    );
  }
}
```

Here, using `setState()` and a field inside the StatefulWidget's State
class is completely natural. No other part of your app needs to access
`_index`. The variable only changes inside the `MyHomepage` widget.
And, if the user closes and restarts the app,
you don't mind that `_index` resets to zero.

在這裡，使用 `setState()` 和一個在有狀態 widget 的 State 類中的變數是很自然的。
你的 app 中的其他部分不需要存取 `_index`。
這個變數只會在 `MyHomepage` widget 中改變。
而且，如果使用者關閉並重啟這個 app，你不會介意 `_index` 重置回 0.

## App state

## 應用狀態

State that is not ephemeral,
that you want to share across many parts of your app,
and that you want to keep between user sessions,
is what we call application state
(sometimes also called shared state).

如果你想在你的應用中的多個部分之間共享一個非短時的狀態，
並且在使用者會話期間保留這個狀態，
我們稱之為應用狀態（有時也稱共享狀態）。

Examples of application state:

應用狀態的一些例子：

* User preferences

  使用者選項

* Login info

  登入資訊

* Notifications in a social networking app

  一個社交應用中的通知

* The shopping cart in an e-commerce app

  一個電商應用中的購物車

* Read/unread state of articles in a news app

  一個新聞應用中的文章已讀/未讀狀態

For managing app state, you'll want to research your options.
Your choice depends on the complexity and nature of your app,
your team's previous experience, and many other aspects. Read on.

為了管理應用狀態，你需要研究你的選項。你的選擇取決於你的應用的複雜度和限制，
你的團隊之前的經驗以及其他方面。請繼續閱讀。

## There is no clear-cut rule

## 沒有明確的規則

To be clear, you _can_ use `State` and `setState()` to manage all of
the state in your app. In fact, the Flutter team does this in many
simple app samples (including the starter app that you get with every
`flutter create`).

需要說明的是，你 **可以** 使用 `State` 和 `setState()` 管理你的應用中的所有狀態。
實際上Flutter團隊在很多簡單的範例程式
（包括你每次使用 `flutter create` 命令建立的初始應用）中正是這麼做的。

It goes the other way, too. For example, you might decide that&mdash;in
the context of your particular app&mdash;the selected tab in a bottom
navigation bar is _not_ ephemeral state. You might need to change it
from outside the class, keep it between sessions, and so on.
In that case, the `_index` variable is app state.

也可以用另外一種方式。比如，在一個特定的應用中，
你可以指定底部導航欄中被選中的專案 **不是** 一個短時狀態。
你可能需要在底部導航欄類別的外部來改變這個值，並在對話期間保留它。
在種情況下 `_index` 就是一個應用狀態。 

There is no clear-cut, universal rule to distinguish
whether a particular variable is ephemeral or app state.
Sometimes, you'll have to refactor one into another.
For example, you'll start with some clearly ephemeral state,
but as your application grows in features,
it might need to be moved to app state.

沒有一個明確、普遍的規則來區分一個變數屬於短時狀態還是應用狀態，
有時你不得不在此之間重構。比如，剛開始你認為一些狀態是短時狀態，
但隨著應用不斷增加功能，有些狀態需要被改變為應用狀態。

For that reason, take the following diagram with a large grain of salt:

因此，請有保留地遵循以下這張流程圖：

<img src='/assets/images/docs/development/data-and-backend/state-mgmt/ephemeral-vs-app-state.png' width="100%" alt="A flow chart. Start with 'Data'. 'Who needs it?'. Three options: 'Most widgets', 'Some widgets' and 'Single widget'. The first two options both lead to 'App state'. The 'Single widget' option leads to 'Ephemeral state'.">

{% comment %}
Source drawing for the png above: : https://docs.google.com/drawings/d/1p5Bvuagin9DZH8bNrpGfpQQvKwLartYhIvD0WKGa64k/edit?usp=sharing
{% endcomment %}

When asked about React's setState versus Redux's store, the author of Redux,
Dan Abramov, replied:

當我們就 React 的 setState 和 Redux 的 Store 
哪個好這個問題問 Redux 的作者 Dan Abramov 時, 他如此回答:

> "The rule of thumb is: [Do whatever is less awkward][]."
>
> "經驗原則是: [選擇能夠減少麻煩的方式][Do whatever is less awkward]"

In summary, there are two conceptual types of state in any Flutter app.
Ephemeral state can be implemented using `State` and `setState()`,
and is often local to a single widget. The rest is your app state.
Both types have their place in any Flutter app, and the split between
the two depends on your own preference and the complexity of the app.

總之，在任何 Flutter 應用中都存在兩種概念型別的狀態，
短時狀態經常被用於一個單獨 widget 的本地狀態，
通常使用 `State` 和 `setState()` 來實現。
其他的是你的應用應用狀態，在任何一個 Flutter 應用中這兩種狀態都有自己的位置。
如何劃分這兩種狀態取決於你的偏好以及應用的複雜度。

[Do whatever is less awkward]: {{site.github}}/reduxjs/redux/issues/1287#issuecomment-175351978
[`PageView`]: {{site.api}}/flutter/widgets/PageView-class.html
