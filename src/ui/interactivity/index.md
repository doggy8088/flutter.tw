---
title: Add interactivity to your Flutter app
title: 為你的 Flutter 應用加入互動體驗
description: How to implement a stateful widget that responds to taps.
description: 如何實現一個能夠響應點選事件的有狀態 widget。
short-title: Interactivity
short-title: 互動性
tags: 使用者介面,Flutter UI,佈局
keywords: 互動,Flutter互動,有狀態的widget,無狀態,StatefulWidget,狀態管理
diff2html: true
---

{% capture examples -%} {{site.repo.this}}/tree/{{site.branch}}/examples {%- endcapture -%}

{{site.alert.secondary}}

  <h4 class="no_toc">What you'll learn</h4>

  <h4 class="no_toc">你會學到什麼</h4>

  * How to respond to taps.

    如何響應點選。

  * How to create a custom widget.

    如何建立自訂 widget。

  * The difference between stateless and stateful widgets.

    無狀態和有狀態 widget 之間的區別。

{{site.alert.end}}

How do you modify your app to make it react to user input?
In this tutorial, you'll add interactivity to an app that
contains only non-interactive widgets.
Specifically, you'll modify an icon to make it tappable
by creating a custom stateful widget that manages two
stateless widgets.

如何修改您的應用程式以使其對使用者輸入做出反應？
在本課程中，您將為僅包含非互動式 widget 的應用程式新增互動性。
具體來說，您將透過建立一個管理兩個無狀態 widget 的自訂有狀態 widget，
修改一個圖示實現使其可點選。

The [building layouts tutorial][] showed you how to create
the layout for the following screenshot.

[建構佈局課程][building layouts tutorial] 中展示瞭如何建構下面截圖所示的佈局。

{% include docs/app-figure.md img-class="site-mobile-screenshot border"
    image="ui/layout/lakes.jpg" caption="The layout tutorial app" %}

When the app first launches, the star is solid red,
indicating that this lake has previously been favorited.
The number next to the star indicates that 41
people have favorited this lake. After completing this tutorial,
tapping the star removes its favorited status,
replacing the solid star with an outline and
decreasing the count. Tapping again favorites the lake,
drawing a solid star and increasing the count.

當應用第一次啟動時，這個星形圖示是實心紅色，表明這個湖以前已經被收藏過了。
星號旁邊的數字表示 41 個人已經收藏了此湖。
完成本課程後，點選星形圖示將取消收藏狀態，
然後用輪廓線的星形圖示代替實心的，並減少計數。
再次點選會重新收藏，並增加計數。

<img src='/assets/images/docs/ui/favorited-not-favorited.png' class="mw-100" alt="The custom widget you'll create" width="200px">
{:.text-center}

To accomplish this, you'll create a single custom widget
that includes both the star and the count,
which are themselves widgets. Tapping the star changes state
for both widgets, so the same widget should manage both.

為了實現這個，您將建立一個包含星形圖示和計數的自訂 widget，它們都是 widget。 
因為點選星形圖示會更改這兩個 widget 的狀態，
所以同一個 widget 應該同時管理這兩個 widget。

You can get right to touching the code in
[Step 2: Subclass StatefulWidget](#step-2).
If you want to try different ways of managing state,
skip to [Managing state][].

您可以直接檢視 [第二步: 建立 StatefulWidget 的子類別](#step-2)。
如果您想嘗試不同的管理狀態方式，
請跳至 [狀態管理][Managing state]。

## Stateful and stateless widgets

## 有狀態和無狀態的 widgets

A widget is either stateful or stateless. If a widget can
change&mdash;when a user interacts with it,
for example&mdash;it's stateful.

有些 widgets 是有狀態的, 有些是無狀態的。
如果使用者與 widget 互動，widget 會發生變化，
那麼它就是 **有狀態的**。

A _stateless_ widget never changes.
[`Icon`][], [`IconButton`][], and [`Text`][] are
examples of stateless widgets. Stateless widgets
subclass [`StatelessWidget`][].

**無狀態的** widget 自身無法改變。
[`Icon`][]、[`IconButton`][] 和 [`Text`][]
都是無狀態 widget，它們都是 [`StatelessWidget`][] 的子類別。

A _stateful_ widget is dynamic: for example,
it can change its appearance in response to events
triggered by user interactions or when it receives data.
[`Checkbox`][], [`Radio`][], [`Slider`][],
[`InkWell`][], [`Form`][], and [`TextField`][]
are examples of stateful widgets. Stateful widgets
subclass [`StatefulWidget`][].

而 **有狀態的** widget 自身是可動態改變的（基於State）。
例如，可以透過與使用者的互動或是隨著資料的改變
而導致外觀形態的變化。
[`Checkbox`][]、[`Radio`][]、[`Slider`][]、
[`InkWell`][]、[`Form`][] 和 [`TextField`][] 
都是有狀態 widget，它們都是 [`StatefulWidget`][] 的子類別。

A widget's state is stored in a [`State`][] object,
separating the widget's state from its appearance.
The state consists of values that can change, like a
slider's current value or whether a checkbox is checked.
When the widget's state changes,
the state object calls `setState()`,
telling the framework to redraw the widget.

一個 widget 的狀態儲存在一個 [`State`][] 物件中，它和 widget 的顯示分離。
Widget 的狀態是一些可以更改的值，如一個滑動條的當前值或一個複選框是否被選中。
當 widget 狀態改變時，State 物件呼叫 `setState()`，告訴框架去重繪 widget。

## Creating a stateful widget

## 建立一個有狀態的 widget

{{site.alert.secondary}}

  <h4 class="no_toc">What's the point?</h4>

  <h4 class="no_toc">重點是什麼？</h4>

  * A stateful widget is implemented by two classes:
    a subclass of `StatefulWidget` and a subclass of `State`.

    實現一個有狀態 widget 需要建立兩個類：
    一個 `StatefulWidget` 的子類別和一個 `State` 的子類別。

  * The state class contains the widget's mutable state and
    the widget's `build()` method.

    State 類包含該 widget 的可變狀態並定義該 widget 的 `build()` 方法。

  * When the widget's state changes, the state object calls
    `setState()`, telling the framework to redraw the widget.

    當 widget 狀態改變時，State 物件呼叫 `setState()`，
    告訴框架去重繪 widget。

{{site.alert.end}}

In this section, you'll create a custom stateful widget.
You'll replace two stateless widgets&mdash;the solid red
star and the numeric count next to it&mdash;with a single
custom stateful widget that manages a row with two
children widgets: an `IconButton` and `Text`.

在本節中，您將建立一個自訂有狀態的 widget。
您將使用一個自訂有狀態 widget 來替換兩個無狀態 widget&mdash;&mdash;
紅色實心星形圖示和其旁邊的數字計數&mdash;&mdash;
該 widget 用兩個子 widget 管理一行 `IconButton` 和 `Text`。

Implementing a custom stateful widget requires creating two classes:

實現一個自訂的有狀態 widget 需要建立兩個類：

* A subclass of `StatefulWidget` that defines the widget.

  一個 StatefulWidget 的子類別，用來定義一個 widget 類別。

* A subclass of `State` that contains the state for that
  widget and defines the widget's `build()` method.

  一個 `State` 的子類別，包含該widget狀態並定義該 widget 的 `build()` 方法。

This section shows you how to build a stateful widget,
called `FavoriteWidget`, for the lakes app.
After setting up, your first step is choosing how state is
managed for `FavoriteWidget`.

這一節展示如何為 Lakes 應用程式建構一個名為
`FavoriteWidget` 的 StatefulWidget。
第一步是選擇如何管理 `FavoriteWidget` 的狀態。

### Step 0: Get ready

### 步驟 0: 開始

If you've already built the app in the
[building layouts tutorial (step 6)][],
skip to the next section.

如果你已經在 [建構佈局課程（第 6 步）][building layouts tutorial (step 6)]
中成功建立了應用程式，你可以跳過下面的部分。

 1. Make sure you've [set up][] your environment.

    確保你已經 [設定][set up] 好了你的環境。

 1. [Create a new Flutter app][new-flutter-app].

    [建立一個新的 Flutter 應用][new-flutter-app]。

 1. Replace the `lib/main.dart` file with [`main.dart`][].

    用 GitHub 上的 [`main.dart`][] 替換 `lib/main.dart` 檔案。

 1. Replace the `pubspec.yaml` file with [`pubspec.yaml`][].

    用 GitHub 上的 [`pubspec.yaml`][] 替換 `pubspec.yaml` 檔案。

 1. Create an `images` directory in your project, and add
    [`lake.jpg`][].

    在你的工程中建立一個 `images` 資料夾，並新增 [`lake.jpg`][]。

Once you have a connected and enabled device,
or you've launched the [iOS simulator][]
(part of the Flutter install) or the
[Android emulator][] (part of the Android Studio
install), you are good to go!

如果你有一個連線並可用的裝置，或者你已經啟動了
[iOS 模擬器][iOS simulator] 或者 [Android 模擬器][Android emulator]
（Flutter 安裝部分介紹過），你就可以開始了！

<a id="step-1"></a>
### Step 1: Decide which object manages the widget's state

### Step 1: 決定哪個物件管理 widget 的狀態

A widget's state can be managed in several ways,
but in our example the widget itself,
`FavoriteWidget`, will manage its own state.
In this example, toggling the star is an isolated
action that doesn't affect the parent widget or the rest of
the UI, so the widget can handle its state internally.

一個 widget 的狀態可以透過多種方式進行管理，
但在我們的範例中，widget 本身
&mdash;&mdash;`FavoriteWidget`&mdash;&mdash;
將管理自己的狀態。
在這個例子中，切換星形圖示是一個獨立的操作，
不會影響父視窗 widget 或其他使用者介面，
因此該 widget 可以在內部處理它自己的狀態。

Learn more about the separation of widget and state,
and how state might be managed, in [Managing state][].

你可以在 [狀態管理][Managing state] 中瞭解更多
關於 widget 和狀態的分離以及如何管理狀態的資訊。

<a id="step-2"></a>
### Step 2: Subclass StatefulWidget

### Step 2: 建立 StatefulWidget 的子類別

The `FavoriteWidget` class manages its own state,
so it overrides `createState()` to create a `State`
object. The framework calls `createState()`
when it wants to build the widget.
In this example, `createState()` returns an
instance of `_FavoriteWidgetState`,
which you'll implement in the next step.

`FavoriteWidget` 類管理自己的狀態，
因此它透過重寫 `createState()` 來建立狀態物件。
框架會在建構 widget 時呼叫 `createState()`。
在這個例子中，`createState()` 建立 `_FavoriteWidgetState` 的例項，
您將在下一步中實現該例項。

<?code-excerpt path-base="layout/lakes/interactive"?>
<?code-excerpt "lib/main.dart (FavoriteWidget)" title?>
```dart
class FavoriteWidget extends StatefulWidget {
  const FavoriteWidget({super.key});

  @override
  State<FavoriteWidget> createState() => _FavoriteWidgetState();
}
```

{{site.alert.note}}

  Members or classes that start with an underscore
  (`_`) are private. For more information,
  see [Libraries and imports][], a section in the
  [Dart language documentation][].
  
  以下劃線（`_`）開頭的成員或類是私有的。
  有關更多資訊，請參閱 [Dart 文件網站][Dart language documentation]
  中的文件 [Libraries and imports][]。

  [Dart language tour]: {{site.dart-site}}/guides/language/language-tour
  [Libraries and visibility]: {{site.dart-site}}/guides/language/language-tour#libraries-and-visibility

{{site.alert.end}}

<a id="step-3"></a>
### Step 3: Subclass State

### Step 3: 建立 State 的子類別

The `_FavoriteWidgetState` class stores the mutable data
that can change over the lifetime of the widget.
When the app first launches, the UI displays a solid
red star, indicating that the lake has "favorite" status,
along with 41 likes. These values are stored in the
`_isFavorited` and `_favoriteCount` fields:

`_FavoriteWidgetState` 類儲存可變資訊；
可以在 widget 的生命週期內改變邏輯和內部狀態。
當應用第一次啟動時，使用者介面顯示一個紅色實心的星星形圖示，
表明該湖已經被收藏，並有 41 個「喜歡」。
狀態物件儲存這些資訊在 `_isFavorited` 和 `_favoriteCount` 變數中。

<?code-excerpt "lib/main.dart (_FavoriteWidgetState fields)" replace="/(bool|int) .*/[!$&!]/g" title?>
```dart
class _FavoriteWidgetState extends State<FavoriteWidget> {
  [!bool _isFavorited = true;!]
  [!int _favoriteCount = 41;!]

  // ···
}
```

The class also defines a `build()` method,
which creates a row containing a red `IconButton`,
and `Text`.  You use [`IconButton`][] (instead of `Icon`)
because it has an `onPressed` property that defines
the callback function (`_toggleFavorite`) for handling a tap.
You'll define the callback function next.

狀態物件也定義了 `build()` 方法。這個 `build()` 方法
建立一個包含紅色 `IconButton` 和 `Text` 的行。
該 widget 使用 [`IconButton`][]（而不是 `Icon`），
因為它具有一個 `onPressed` 屬性，
該屬性定義了處理點選的回呼(Callback)方法 (`_toggleFavorite`)。
你將會在接下來的步驟中嘗試定義它。

<?code-excerpt "lib/main.dart (_FavoriteWidgetState build)" replace="/build|icon.*|onPressed.*|child: Text.*/[!$&!]/g" title?>
```dart
class _FavoriteWidgetState extends State<FavoriteWidget> {
  // ···
  @override
  Widget [!build!](BuildContext context) {
    return Row(
      mainAxisSize: MainAxisSize.min,
      children: [
        Container(
          padding: const EdgeInsets.all(0),
          child: IconButton(
            padding: const EdgeInsets.all(0),
            alignment: Alignment.centerRight,
            [!icon: (_isFavorited!]
                ? const Icon(Icons.star)
                : const Icon(Icons.star_border)),
            color: Colors.red[500],
            [!onPressed: _toggleFavorite,!]
          ),
        ),
        SizedBox(
          width: 18,
          child: SizedBox(
            [!child: Text('$_favoriteCount'),!]
          ),
        ),
      ],
    );
  }
}
```

{{site.alert.tip}}

  Placing the `Text` in a [`SizedBox`][] and setting its
  width prevents a discernible "jump" when the text changes
  between the values of 40 and 41 &mdash; a jump would
  otherwise occur because those values have different widths.

  當 `Text` 在 40 和 41 之間變化時，將文字放在 [`SizedBox`][] 中
  並設定其寬度可防止出現明顯的「跳躍」，因為這些值具有不同的寬度。

{{site.alert.end}}

The `_toggleFavorite()` method, which is called when the
`IconButton` is pressed, calls `setState()`.
Calling `setState()` is critical, because this
tells the framework that the widget's state has
changed and that the widget should be redrawn.
The function argument to `setState()` toggles the
UI between these two states:

按下 `IconButton` 時會呼叫 `_toggleFavorite()` 方法，
然後它會呼叫 `setState()`。
呼叫 `setState()` 是至關重要的，因為這告訴框架，
widget 的狀態已經改變，應該重繪。
`setState()` 在如下兩種狀態中切換 UI：

* A `star` icon and the number 41

  實心的星形圖示和數字 41

* A `star_border` icon and the number 40

  輪廓線的星形圖示 `star_border` 和數字 40 之間切換 UI

<?code-excerpt "lib/main.dart (_toggleFavorite)"?>
```dart
void _toggleFavorite() {
  setState(() {
    if (_isFavorited) {
      _favoriteCount -= 1;
      _isFavorited = false;
    } else {
      _favoriteCount += 1;
      _isFavorited = true;
    }
  });
}
```

<a id="step-4"></a>
### Step 4: Plug the stateful widget into the widget tree

### Step 4: 將有 stateful widget 插入 widget 樹中

Add your custom stateful widget to the widget tree in
the app's `build()` method. First, locate the code that
creates the `Icon` and `Text`, and delete it.
In the same location, create the stateful widget:

將您自訂 stateful widget 在 `build()` 方法中新增到 widget 樹中。
首先，找到建立 `Icon` 和 `Text` 的程式碼，並刪除它，
在相同的位置建立有狀態的 widget：

<?code-excerpt path-base=""?>
<?code-excerpt "layout/lakes/{step6,interactive}/lib/main.dart" remove="*3*" from="class MyApp" to="/^[ ]+\);$/"?>
```diff
--- layout/lakes/step6/lib/main.dart
+++ layout/lakes/interactive/lib/main.dart
@@ -10,2 +5,2 @@
 class MyApp extends StatelessWidget {
   const MyApp({super.key});
@@ -40,11 +35,7 @@
               ],
             ),
           ),
-          Icon(
-            Icons.star,
-            color: Colors.red[500],
-          ),
-          const Text('41'),
+          const FavoriteWidget(),
         ],
       ),
     );
```

That's it! When you hot reload the app,
the star icon should now respond to taps.

就是這樣！當您熱重載應用後，星形圖示就會響應點選了。


### Problems?

### 有問題?

If you can't get your code to run, look in your
IDE for possible errors.  [Debugging Flutter apps][] might help.
If you still can't find the problem,
check your code against the interactive lakes example on GitHub.

如果您的程式碼無法執行，請在 IDE 中查詢可能的錯誤。
[除錯 Flutter 應用程式][Debugging Flutter apps] 可能會有所幫助。
如果仍然無法找到問題，請根據 GitHub 上的範例檢查程式碼。

{% comment %}
TODO: replace the following links with tabbed code panes.
{% endcomment -%}

* [`lib/main.dart`]({{site.repo.this}}/tree/{{site.branch}}/examples/layout/lakes/interactive/lib/main.dart)
* [`pubspec.yaml`]({{site.repo.this}}/tree/{{site.branch}}/examples/layout/lakes/interactive/pubspec.yaml)
* [`lakes.jpg`]({{site.repo.this}}/tree/{{site.branch}}/examples/layout/lakes/interactive/images/lake.jpg)

If you still have questions, refer to any one of the developer
[community][] channels.

如果您仍有問題，可以諮詢 [社群][community] 中的任何一位開發者。

---

The rest of this page covers several ways a widget's state can
be managed, and lists other available interactive widgets.

本頁面的其餘部分介紹了可以管理 widget 狀態的幾種方式，
並列出了其他可用的可互動的 widget。

## Managing state

## 狀態管理

{{site.alert.secondary}}

  <h4 class="no_toc">What's the point?</h4>

  <h4 class="no_toc">重點是什麼？</h4>

  * There are different approaches for managing state.

    有多種方法可以管理狀態。

  * You, as the widget designer, choose which approach to use.

    您作為 widget 的設計者，需要選擇使用何種管理方法。

  * If in doubt, start by managing state in the parent widget.

    如果不是很清楚時，就在父 widget 中管理狀態。

{{site.alert.end}}


Who manages the stateful widget's state? The widget itself?
The parent widget?  Both? Another object?
The answer is... it depends. There are several valid ways
to make your widget interactive. You, as the widget designer,
make the decision based on how you expect your widget to be used.
Here are the most common ways to manage state:

誰管理著 stateful widget 的狀態？widget 本身？
父 widget？雙方？另一個物件？答案是......
這取決於實際情況。有幾種有效的方法可以給你的 widget 加入互動。
作為 widget 設計師，你可以基於你所期待的表現 widget 的方式來做決定。
以下是一些管理狀態的最常見的方法：

* [The widget manages its own state](#self-managed)

  [widget 管理自己的狀態](#self-managed)

* [The parent manages the widget's state](#parent-managed)

  [父 widget 管理此 widget 的狀態](#parent-managed)
  
* [A mix-and-match approach](#mix-and-match)
 
  [混搭管理](#mix-and-match)

How do you decide which approach to use?
The following principles should help you decide:

如何決定使用哪種管理方法？以下原則可以幫助您決定：

* If the state in question is user data,
  for example the checked or unchecked
  mode of a checkbox, or the position of a slider,
  then the state is best managed by the parent widget.

  如果狀態是使用者資料，如複選框的選中狀態、滑塊的位置，
  則該狀態最好由父 widget 管理。

* If the state in question is aesthetic,
  for example an animation, then the
  state is best managed by the widget itself.
  
  如果所討論的狀態是有關介面外觀效果的，
  例如動畫，那麼狀態最好由 widget 本身來管理。

If in doubt, start by managing state in the parent widget.

如果有疑問，首選是在父 widget 中管理狀態。

We'll give examples of the different ways of managing state
by creating three simple examples: TapboxA, TapboxB,
and TapboxC. The examples all work similarly&mdash;each
creates a container that, when tapped, toggles between a
green or grey box. The `_active` boolean determines the
color: green for active or grey for inactive.

我們將透過建立三個簡單範例來舉例說明管理狀態的不同方式：
TapboxA、TapboxB 和 TapboxC。
這些例子功能是相似的&mdash;&mdash;
每建立一個容器，當點選時，在綠色或灰色框之間切換。
`_active` 確定顏色：綠色為 true，灰色為 false。

<div class="row mb-4">
  <div class="col-12 text-center">
    <img src='/assets/images/docs/ui/tapbox-active-state.png' class="border mt-1 mb-1 mw-100" width="150px" alt="Active state">
    <img src='/assets/images/docs/ui/tapbox-inactive-state.png' class="border mt-1 mb-1 mw-100" width="150px" alt="Inactive state">
  </div>
</div>

These examples use [`GestureDetector`][] to capture activity
on the `Container`.

這些範例使用 [`GestureDetector`][] 捕獲 `Container` 上的使用者動作。

<a id="self-managed"></a>
### The widget manages its own state

### widget 管理自己的狀態

Sometimes it makes the most sense for the widget
to manage its state internally. For example,
[`ListView`][] automatically scrolls when its
content exceeds the render box. Most developers
using `ListView` don't want to manage `ListView`'s
scrolling behavior, so `ListView` itself manages its scroll offset.

有時，widget 在內部管理其狀態是最好的。
例如，當 [`ListView`][] 的內容超過渲染框時，
ListView 自動滾動。大多數使用 `ListView`
的開發人員不想管理 `ListView` 的滾動行為，
因此 `ListView` 本身管理其滾動偏移量。

The `_TapboxAState` class:

`_TapboxAState` 類:

* Manages state for `TapboxA`.

  管理 `TapboxA` 的狀態。

* Defines the `_active` boolean which determines the
  box's current color.

  定義布林值 `_active` 確定盒子的當前顏色。

* Defines the `_handleTap()` function, which updates
  `_active` when the box is tapped and calls the
  `setState()` function to update the UI.

  定義 `_handleTap()` 函式，該函式在點選該盒子時更新
  `_active`，並呼叫 `setState()` 更新 UI。

* Implements all interactive behavior for the widget.

  實現 widget 的所有互動式行為。

<?code-excerpt path-base="development/ui/interactive/"?>

<?code-excerpt "lib/self_managed.dart"?>
```dart
import 'package:flutter/material.dart';

// TapboxA manages its own state.

//------------------------- TapboxA ----------------------------------

class TapboxA extends StatefulWidget {
  const TapboxA({super.key});

  @override
  State<TapboxA> createState() => _TapboxAState();
}

class _TapboxAState extends State<TapboxA> {
  bool _active = false;

  void _handleTap() {
    setState(() {
      _active = !_active;
    });
  }

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: _handleTap,
      child: Container(
        width: 200,
        height: 200,
        decoration: BoxDecoration(
          color: _active ? Colors.lightGreen[700] : Colors.grey[600],
        ),
        child: Center(
          child: Text(
            _active ? 'Active' : 'Inactive',
            style: const TextStyle(fontSize: 32, color: Colors.white),
          ),
        ),
      ),
    );
  }
}

//------------------------- MyApp ----------------------------------

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Flutter Demo'),
        ),
        body: const Center(
          child: TapboxA(),
        ),
      ),
    );
  }
}
```

<hr>

<a id="parent-managed"></a>
### The parent widget manages the widget's state

### 父 widget 管理 widget 的 state

Often it makes the most sense for the parent widget
to manage the state and tell its child widget when to update.
For example, [`IconButton`][] allows you to treat
an icon as a tappable button. `IconButton` is a
stateless widget because we decided that the parent
widget needs to know whether the button has been tapped,
so it can take appropriate action.

一般來說父 widget 管理狀態並告訴其子 widget 何時更新通常是最合適的。
例如，[`IconButton`][] 允許您將圖示視為可點按的按鈕。
`IconButton` 是一個無狀態 widget，
因為我們認為父 widget 需要知道該按鈕是否被點選來採取相應的處理。

In the following example, TapboxB exports its state
to its parent through a callback. Because TapboxB
doesn't manage any state, it subclasses StatelessWidget.

在以下範例中，TapboxB 透過回呼(Callback)將其狀態到其父類別。
由於 TapboxB 不管理任何狀態，因此它繼承自 StatelessWidget。

The ParentWidgetState class:

ParentWidgetState 類：

* Manages the `_active` state for TapboxB.

  為 TapboxB 管理 `_active` 狀態；

* Implements `_handleTapboxChanged()`,
  the method called when the box is tapped.

  實現 `_handleTapboxChanged()`，當盒子被點選時呼叫的方法；

* When the state changes, calls `setState()`
  to update the UI.

  當狀態改變時，呼叫 `setState()` 更新 UI。

The TapboxB class:

TapboxB 類：

* Extends StatelessWidget because all state is handled by its parent.

  繼承 StatelessWidget 類，因為所有狀態都由其父 widget 處理；

* When a tap is detected, it notifies the parent.

  當檢測到點選時，它會通知父 widget。

<?code-excerpt "lib/parent_managed.dart"?>
```dart
import 'package:flutter/material.dart';

// ParentWidget manages the state for TapboxB.

//------------------------ ParentWidget --------------------------------

class ParentWidget extends StatefulWidget {
  const ParentWidget({super.key});

  @override
  State<ParentWidget> createState() => _ParentWidgetState();
}

class _ParentWidgetState extends State<ParentWidget> {
  bool _active = false;

  void _handleTapboxChanged(bool newValue) {
    setState(() {
      _active = newValue;
    });
  }

  @override
  Widget build(BuildContext context) {
    return SizedBox(
      child: TapboxB(
        active: _active,
        onChanged: _handleTapboxChanged,
      ),
    );
  }
}

//------------------------- TapboxB ----------------------------------

class TapboxB extends StatelessWidget {
  const TapboxB({
    super.key,
    this.active = false,
    required this.onChanged,
  });

  final bool active;
  final ValueChanged<bool> onChanged;

  void _handleTap() {
    onChanged(!active);
  }

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: _handleTap,
      child: Container(
        width: 200,
        height: 200,
        decoration: BoxDecoration(
          color: active ? Colors.lightGreen[700] : Colors.grey[600],
        ),
        child: Center(
          child: Text(
            active ? 'Active' : 'Inactive',
            style: const TextStyle(fontSize: 32, color: Colors.white),
          ),
        ),
      ),
    );
  }
}
```

<hr>

<a id="mix-and-match"></a>
### A mix-and-match approach

### 混搭管理

For some widgets, a mix-and-match approach makes
the most sense. In this scenario, the stateful widget
manages some of the state, and the parent widget
manages other aspects of the state.

對於一些 widget 來說，混搭管理的方法最合適的。
在這種情況下，有狀態的 widget 自己管理一些狀態，
同時父 widget 管理其他方面的狀態。

In the `TapboxC` example, on tap down,
a dark green border appears around the box. On tap up,
the border disappears and the box's color changes. `TapboxC`
exports its `_active` state to its parent but manages its
`_highlight` state internally. This example has two `State`
objects, `_ParentWidgetState` and `_TapboxCState`.

在 `TapboxC` 範例中，點選時，
盒子的周圍會出現一個深綠色的邊框。
點選時，邊框消失，盒子的顏色改變。
`TapboxC` 將其 `_active` 狀態匯出到其父 widget 中，
但在內部管理其 `_highlight` 狀態。
這個例子有兩個狀態物件 `_ParentWidgetState` 和 `_TapboxCState`。

The `_ParentWidgetState` object:

`_ParentWidgetState` 物件：

* Manages the `_active` state.

  管理`_active` 狀態。

* Implements `_handleTapboxChanged()`,
  the method called when the box is tapped.

  實現 `_handleTapboxChanged()`，此方法在盒子被點選時呼叫。

* Calls `setState()` to update the UI when a tap
  occurs and the `_active` state changes.

  當點選盒子並且 `_active` 狀態改變時呼叫 `setState()` 來更新 UI。

The `_TapboxCState` object:

`_TapboxCState` 物件:

* Manages the `_highlight` state.

  管理 `_highlight` state。

* The `GestureDetector` listens to all tap events.
  As the user taps down, it adds the highlight
  (implemented as a dark green border). As the user releases the
  tap, it removes the highlight.

  `GestureDetector` 監聽所有 tap 事件。
  當用戶點下時，它新增高亮（深綠色邊框）；
  當用戶釋放時，會移除高亮。  

* Calls `setState()` to update the UI on tap down,
  tap up, or tap cancel, and the `_highlight` state changes.

  當按下、抬起、或者取消點選時更新 `_highlight` 狀態，
  呼叫 `setState()` 更新UI。

* On a tap event, passes that state change to the parent widget to take
  appropriate action using the [`widget`][] property.

  當點選時，[`widget`][] 屬性將狀態的改變傳遞給父 widget 並進行合適的操作。

<?code-excerpt "lib/mixed.dart"?>
```dart
import 'package:flutter/material.dart';

//---------------------------- ParentWidget ----------------------------

class ParentWidget extends StatefulWidget {
  const ParentWidget({super.key});

  @override
  State<ParentWidget> createState() => _ParentWidgetState();
}

class _ParentWidgetState extends State<ParentWidget> {
  bool _active = false;

  void _handleTapboxChanged(bool newValue) {
    setState(() {
      _active = newValue;
    });
  }

  @override
  Widget build(BuildContext context) {
    return SizedBox(
      child: TapboxC(
        active: _active,
        onChanged: _handleTapboxChanged,
      ),
    );
  }
}

//----------------------------- TapboxC ------------------------------

class TapboxC extends StatefulWidget {
  const TapboxC({
    super.key,
    this.active = false,
    required this.onChanged,
  });

  final bool active;
  final ValueChanged<bool> onChanged;

  @override
  State<TapboxC> createState() => _TapboxCState();
}

class _TapboxCState extends State<TapboxC> {
  bool _highlight = false;

  void _handleTapDown(TapDownDetails details) {
    setState(() {
      _highlight = true;
    });
  }

  void _handleTapUp(TapUpDetails details) {
    setState(() {
      _highlight = false;
    });
  }

  void _handleTapCancel() {
    setState(() {
      _highlight = false;
    });
  }

  void _handleTap() {
    widget.onChanged(!widget.active);
  }

  @override
  Widget build(BuildContext context) {
    // This example adds a green border on tap down.
    // On tap up, the square changes to the opposite state.
    return GestureDetector(
      onTapDown: _handleTapDown, // Handle the tap events in the order that
      onTapUp: _handleTapUp, // they occur: down, up, tap, cancel
      onTap: _handleTap,
      onTapCancel: _handleTapCancel,
      child: Container(
        width: 200,
        height: 200,
        decoration: BoxDecoration(
          color: widget.active ? Colors.lightGreen[700] : Colors.grey[600],
          border: _highlight
              ? Border.all(
                  color: Colors.teal[700]!,
                  width: 10,
                )
              : null,
        ),
        child: Center(
          child: Text(widget.active ? 'Active' : 'Inactive',
              style: const TextStyle(fontSize: 32, color: Colors.white)),
        ),
      ),
    );
  }
}
```

An alternate implementation might have exported the highlight
state to the parent while keeping the active state internal,
but if you asked someone to use that tap box,
they'd probably complain that it doesn't make much sense.
The developer cares whether the box is active.
The developer probably doesn't care how the highlighting
is managed, and prefers that the tap box handles those
details.

另一種實現可能會將高亮狀態匯出到父 widget，
同時保持 active 狀態為內部，
但如果你要求某人使用該 TapBox，他們可能會抱怨說沒有多大意義。
開發人員只會關心該框是否處於活動狀態。
開發人員可能不在乎高亮顯示是如何管理的，
並且傾向於讓 TapBox 處理這些細節。

<hr>

## Other interactive widgets

## 其他互動式 widgets

Flutter offers a variety of buttons and similar interactive widgets.
Most of these widgets implement the [Material Design guidelines][],
which define a set of components with an opinionated UI.

Flutter 提供各種按鈕和類似的互動式 widget。
這些 widget 中的大多數都實現了 [Material Design guidelines][]，
它們定義了一組具有質感的 UI 元件。

If you prefer, you can use [`GestureDetector`][] to build
interactivity into any custom widget.
You can find examples of `GestureDetector` in
[Managing state][]. Learn more about the `GestureDetector`
in [Handle taps][], a recipe in the [Flutter cookbook][].

如果你願意，你可以使用 [`GestureDetector`][]
來給任何自訂 widget 新增互動性。
你可以在 [管理狀態][Managing state] 中找到 `GestureDetector` 的範例。
同時你也可以在 [Flutter cookbook][] 的 [處理點選][Handle taps]
中學習更多關於 `GestureDetector` 的內容。

{{site.alert.tip}}

  Flutter also provides a set of iOS-style widgets called
  [`Cupertino`][].

  Flutter 還提供了一組名為 [`Cupertino`][] 的 iOS 風格的小部件。

{{site.alert.end}}

When you need interactivity, it's easiest to use one of
the prefabricated widgets. Here's a partial list:

當你需要互動性時，最容易的是使用預製的 widget。
這是預置 widget 部分列表:

### Standard widgets

### 標準 widgets

* [`Form`][]
* [`FormField`][]

### Material Components

### Material 元件

* [`Checkbox`][]
* [`DropdownButton`][]
* [`TextButton`][]
* [`FloatingActionButton`][]
* [`IconButton`][]
* [`Radio`][]
* [`ElevatedButton`][]
* [`Slider`][]
* [`Switch`][]
* [`TextField`][]

## Resources

## 資源

The following resources might help when adding interactivity
to your app.

以下資源可能會在給您的應用新增互動的時候有所幫助。

[Gestures][], a section in the [Flutter cookbook][].

[手勢][Gestures]，[Flutter 實用課程][Flutter cookbook] 裡的一個小節。

[Handling gestures][]
<br> How to create a button and make it respond to input.

[處理手勢][Handling gestures]
<br> 如何建立一個按鈕並使其響應使用者動作。

[Gestures in Flutter][]
<br> A description of Flutter's gesture mechanism.

[點選、拖動和其他手勢][Gestures in Flutter]
<br> Flutter 手勢機制的描述。

[Flutter API documentation][]
<br> Reference documentation for all of the Flutter libraries.

[Flutter API 文件][Flutter API documentation]
<br> 所有 Flutter API 的參考文件。

Flutter Gallery [running app][], [repo][]
<br> Demo app showcasing many Material components and
  other Flutter features.

[Flutter Gallery][running app] 應用，[程式碼儲存庫][repo]
<br> 一個 Demo 應用程式，展示了許多 Material 和其他 Flutter 功能。

[Flutter's Layered Design][] (video)
<br> This video includes information about state and
  stateless widgets.  Presented by Google engineer, Ian Hickson.

[Flutter 的分層設計][Flutter's Layered Design CN] (影片)
<br> 此影片包含有關有狀態和無狀態 widget 的資訊。
由 Google 工程師 Ian Hickson 講解。


[Android emulator]: {{site.url}}/get-started/install/windows#set-up-the-android-emulator
[`Checkbox`]: {{site.api}}/flutter/material/Checkbox-class.html
[`Cupertino`]: {{site.api}}/flutter/cupertino/cupertino-library.html
[Dart language documentation]: {{site.dart-site}}/language
[Debugging Flutter apps]: {{site.url}}/testing/debugging
[`DropdownButton`]: {{site.api}}/flutter/material/DropdownButton-class.html
[`TextButton`]: {{site.api}}/flutter/material/TextButton-class.html
[`FloatingActionButton`]: {{site.api}}/flutter/material/FloatingActionButton-class.html
[Flutter API documentation]: {{site.api}}
[Flutter cookbook]: {{site.url}}/cookbook
[Flutter's Layered Design CN]: https://www.bilibili.com/video/BV1b441157vV
[Flutter's Layered Design]: {{site.youtube-site}}/watch?v=dkyY9WCGMi0
[`FormField`]: {{site.api}}/flutter/widgets/FormField-class.html
[`Form`]: {{site.api}}/flutter/widgets/Form-class.html
[`foundation` library]: {{site.api}}/flutter/foundation/foundation-library.html
[`GestureDetector`]: {{site.api}}/flutter/widgets/GestureDetector-class.html
[Gestures]: {{site.url}}/cookbook/gestures
[Gestures in Flutter]: {{site.url}}/ui/interactivity/gestures
[Handling gestures]: {{site.url}}/ui#handling-gestures
[new-flutter-app]: {{site.url}}/get-started/test-drive
[`IconButton`]: {{site.api}}/flutter/material/IconButton-class.html
[`Icon`]: {{site.api}}/flutter/widgets/Icon-class.html
[`InkWell`]: {{site.api}}/flutter/material/InkWell-class.html
[iOS simulator]: {{site.url}}/get-started/install/macos#set-up-the-ios-simulator
[building layouts tutorial]: {{site.url}}/ui/layout/tutorial
[building layouts tutorial (step 6)]: {{site.url}}/ui/layout/tutorial#step-6-final-touch
[community]: {{site.main-url}}/community
[Handle taps]: {{site.url}}/cookbook/gestures/handling-taps
[`lake.jpg`]: {{examples}}/layout/lakes/step6/images/lake.jpg
[Libraries and imports]: {{site.dart-site}}/language/libraries
[`ListView`]: {{site.api}}/flutter/widgets/ListView-class.html
[`main.dart`]: {{examples}}/layout/lakes/step6/lib/main.dart
[Managing state]: #managing-state
[Material Design guidelines]: {{site.material}}/styles
[`meta.dart`]: {{site.pub}}/packages/meta
[`pubspec.yaml`]: {{examples}}/layout/lakes/step6/pubspec.yaml
[`Radio`]: {{site.api}}/flutter/material/Radio-class.html
[`ElevatedButton`]: {{site.api}}/flutter/material/ElevatedButton-class.html
[repo]: {{site.repo.gallery}}
[running app]: {{site.gallery}}
[set up]: {{site.url}}/get-started/install
[`SizedBox`]: {{site.api}}/flutter/widgets/SizedBox-class.html
[`Slider`]: {{site.api}}/flutter/material/Slider-class.html
[`State`]: {{site.api}}/flutter/widgets/State-class.html
[`StatefulWidget`]: {{site.api}}/flutter/widgets/StatefulWidget-class.html
[`StatelessWidget`]: {{site.api}}/flutter/widgets/StatelessWidget-class.html
[`Switch`]: {{site.api}}/flutter/material/Switch-class.html
[`TextField`]: {{site.api}}/flutter/material/TextField-class.html
[`Text`]: {{site.api}}/flutter/widgets/Text-class.html
[`widget`]: {{site.api}}/flutter/widgets/State/widget.html
