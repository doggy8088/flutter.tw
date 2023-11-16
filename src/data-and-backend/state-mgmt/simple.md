---
title: Simple app state management
title: 簡單的應用狀態管理
description: A simple form of state management.
description: 一個簡單形式的狀態管理。
tags: Flutter狀態管理
keywords: provider狀態管理
prev:
  title: Ephemeral versus app state
  title: 區域性狀態和全域應用狀態
  path: /docs/data-and-backend/state-mgmt/ephemeral-vs-app
next:
  title: List of approaches
  title: 狀態管理的方法列表
  path: /docs/data-and-backend/state-mgmt/options
---

<?code-excerpt path-base="state_mgmt/simple/"?>

Now that you know about [declarative UI programming][]
and the difference between [ephemeral and app state][],
you are ready to learn about simple app state management.

現在大家已經瞭解了
[宣告式的程式設計思維][declarative UI programming] 和
[短時 (ephemeral) 與應用 (app) 狀態][ephemeral and app state] 
之間的區別，現在可以學習如何管理簡單的全域應用狀態。

On this page, we are going to be using the `provider` package.
If you are new to Flutter and you don't have a strong reason to choose
another approach (Redux, Rx, hooks, etc.), this is probably the approach
you should start with. The `provider` package is easy to understand
and it doesn't use much code.
It also uses concepts that are applicable in every other approach.

在這裡，我們打算使用 `provider` package。如果你是 Flutter 的初學者，
而且也沒有很重要的理由必須選擇別的方式來實現（Redux、Rx、hooks 等等），
那麼這就是你應該入門使用的。`provider` 非常好理解而且不需要寫很多程式碼。
它也會用到一些在其它實現方式中用到的通用概念。

That said, if you have a strong background in
state management from other reactive frameworks,
you can find packages and tutorials listed on the [options page][].

即便如此，如果你已經從其它響應式框架上積累了豐富的狀態管理經驗的話，
那麼可以在 
[狀態 (State) 管理參考][options page]
中找到相關的 package 和課程。

## Our example 

## 範例

<img src='/assets/images/docs/development/data-and-backend/state-mgmt/model-shopper-screencast.gif' alt='An animated gif showing a Flutter app in use. It starts with the user on a login screen. They log in and are taken to the catalog screen, with a list of items. The click on several items, and as they do so, the items are marked as "added". The user clicks on a button and gets taken to the cart view. They see the items there. They go back to the catalog, and the items they bought still show "added". End of animation.' class='site-image-right'>

For illustration, consider the following simple app.

為了示範效果，我們實現下面這個簡單應用。

The app has two separate screens: a catalog,
and a cart (represented by the `MyCatalog`,
and `MyCart` widgets, respectively). It could be a shopping app,
but you can imagine the same structure in a simple social networking
app (replace catalog for "wall" and cart for "favorites").

這個應用有兩個獨立的頁面：一個類別頁面和一個購物車頁面
（分別用 `MyCatalog`，`MyCart` widget 來展示）。
雖然看上去是一個購物應用程式，
但是你也可以和社交網路應用類比
（把類別頁面替換成朋友圈，把購物車替換成關注的人）。

The catalog screen includes a custom app bar (`MyAppBar`)
and a scrolling view of many list items (`MyListItems`).

類別頁面包含一個自訂的 app bar (`MyAppBar`) 
以及一個包含元素列表的可滑動的檢視 (`MyListItems`)。

Here's the app visualized as a widget tree.

這是應用程式對應的視覺化的 widget 樹。

<img src='/assets/images/docs/development/data-and-backend/state-mgmt/simple-widget-tree.png' width="100%" alt="A widget tree with MyApp at the top, and  MyCatalog and MyCart below it. MyCart area leaf nodes, but MyCatalog have two children: MyAppBar and a list of MyListItems.">

{% comment %}
  Source drawing for the png above: https://docs.google.com/drawings/d/1KXxAl_Ctxc-avhR4uE58BXBM6Tyhy0pQMCsSMFHVL_0/edit?zx=y4m1lzbhsrvx
  上面 widget 樹的圖片可以在這個地址找到: https://docs.google.com/drawings/d/1KXxAl_Ctxc-avhR4uE58BXBM6Tyhy0pQMCsSMFHVL_0/edit?zx=y4m1lzbhsrvx
{% endcomment %}

So we have at least 5 subclasses of `Widget`. Many of them need
access to state that "belongs" elsewhere. For example, each
`MyListItem` needs to be able to add itself to the cart.
It might also want to see whether the currently displayed item
is already in the cart.

所以我們有至少 5 個 `Widget` 的子類別。他們中有很多需要存取一些全域的狀態。
比如，`MyListItem` 會被新增到購物車中。
但是它可能需要檢查和自己相同的元素是否已經被新增到購物車中。

This takes us to our first question: where should we put the current
state of the cart?

這裡我們出現了第一個問題：我們把當前購物車的狀態放在哪合適呢？

## Lifting state up

## 提高狀態的層級

In Flutter,
it makes sense to keep the state above the widgets that use it.

在 Flutter 中，有必要將儲存狀態的物件置於 widget 樹中對應 widget 的上層。

Why? In declarative frameworks like Flutter, if you want to change the UI,
you have to rebuild it. There is no easy way to have
`MyCart.updateWith(somethingNew)`. In other words, it's hard to
imperatively change a widget from outside, by calling a method on it.
And even if you could make this work, you would be fighting the
framework instead of letting it help you.

為什麼呢？在類似 Flutter 的宣告式框架中，如果你想要修改 UI，那麼你需要重構它。
並沒有類似 `MyCart.updateWith(somethingNew)` 的簡單呼叫方法。
換言之，你很難透過外部呼叫方法修改一個 widget。
即便你自己實現了這樣的模式，那也是和整個框架不相相容。

```dart
// BAD: DO NOT DO THIS
void myTapHandler() {
  var cartWidget = somehowGetMyCartWidget();
  cartWidget.updateWith(item);
}
```

Even if you get the above code to work,
you would then have to deal
with the following in the `MyCart` widget:

即使你實現了上面的程式碼，也得處理 `MyCart` widget 中的程式碼：

```dart
// BAD: DO NOT DO THIS
Widget build(BuildContext context) {
  return SomeWidget(
    // The initial state of the cart.
  );
}

void updateWith(Item item) {
  // Somehow you need to change the UI from here.
}
```

You would need to take into consideration the current state of the UI
and apply the new data to it. It's hard to avoid bugs this way.

你可能需要考慮當前 UI 的狀態，然後把最新的資料新增進去。但是這樣的方式很難避免出現 bug。

In Flutter, you construct a new widget every time its contents change.
Instead of `MyCart.updateWith(somethingNew)` (a method call)
you use `MyCart(contents)` (a constructor). Because you can only
construct new widgets in the build methods of their parents,
if you want to change `contents`, it needs to live in `MyCart`'s
parent or above.

在 Flutter 中，每次當 widget 內容發生改變的時候，你就需要構造一個新的。
你會呼叫 `MyCart(contents)`（建構函式），
而不是 `MyCart.updateWith(somethingNew)`（呼叫方法）。
因為你只能透過父類別的 build 方法來建構新 widget，
如果你想修改 `contents`，就需要呼叫 `MyCart` 的父類甚至更高一級的類別。

<?code-excerpt "lib/src/provider.dart (myTapHandler)"?>
```dart
// GOOD
void myTapHandler(BuildContext context) {
  var cartModel = somehowGetMyCartModel(context);
  cartModel.add(item);
}
```

Now `MyCart` has only one code path for building any version of the UI.

這裡 `MyCart` 可以在各種版本的 UI 中呼叫同一個程式碼路徑。

<?code-excerpt "lib/src/provider.dart (build)"?>
```dart
// GOOD
Widget build(BuildContext context) {
  var cartModel = somehowGetMyCartModel(context);
  return SomeWidget(
    // Just construct the UI once, using the current state of the cart.
    // ···
  );
}
```

In our example, `contents` needs to live in `MyApp`. Whenever it changes,
it rebuilds `MyCart` from above (more on that later). Because of this,
`MyCart` doesn't need to worry about lifecycle&mdash;it just declares
what to show for any given `contents`. When that changes, the old
`MyCart` widget disappears and is completely replaced by the new one.

在我們的例子中，`contents`會存在於 `MyApp` 的生命週期中。
當它發生改變的時候，它會從上層重構 `MyCart` 。因為這個機制，
所以 `MyCart` 無需考慮生命週期的問題&mdash;它只需要針對
`contents` 宣告所需顯示內容即可。
當內容發生改變的時候，舊的 `MyCart` widget 就會消失，
完全被新的 widget 替代。

<img src='/assets/images/docs/development/data-and-backend/state-mgmt/simple-widget-tree-with-cart.png' width="100%" alt="Same widget tree as above, but now we show a small 'cart' badge next to MyApp, and there are two arrows here. One comes from one of the MyListItems to the 'cart', and another one goes from the 'cart' to the MyCart widget.">

{% comment %}
  Source drawing for the png above: https://docs.google.com/drawings/d/1ErMyaX4fwfbIW9ABuPAlHELLGMsU6cdxPDFz_elsS9k/edit?zx=j42inp8903pt
{% endcomment %}

{% comment %}
  上面圖片的資源可以在該連結檢視: https://docs.google.com/drawings/d/1ErMyaX4fwfbIW9ABuPAlHELLGMsU6cdxPDFz_elsS9k/edit?zx=j42inp8903pt
{% endcomment %}

This is what we mean when we say that widgets are immutable.
They don't change&mdash;they get replaced.

這就是我們所說的 widget 是不可變的。因為它們會直接被替換。

Now that we know where to put the state of the cart, let's see how
to access it.

現在我們知道在哪裡放置購物車的狀態，接下來看一下如何讀取該狀態。

## Accessing the state

## 讀取狀態

When a user clicks on one of the items in the catalog,
it's added to the cart. But since the cart lives above `MyListItem`,
how do we do that?

當用戶點選類別頁面中的一個元素，它會被新增到購物車裡。
然而當購物車在 widget 樹中，處於 MyListItem 的層級之上時，又該如何存取狀態呢？

A simple option is to provide a callback that `MyListItem` can call
when it is clicked. Dart's functions are first class objects,
so you can pass them around any way you want. So, inside
`MyCatalog` you can define the following:

一個簡單的實現方法是提供一個回呼(Callback)函式，當 `MyListItem` 被點選的時候可以呼叫。
Dart 的函式都是 first class 物件，所以你可以以任意方式傳遞它們。
所以在 `MyCatalog` 裡你可以使用下面的程式碼：

<?code-excerpt "lib/src/passing_callbacks.dart (methods)"?>
```dart
@override
Widget build(BuildContext context) {
  return SomeWidget(
    // Construct the widget, passing it a reference to the method above.
    MyListItem(myTapCallback),
  );
}

void myTapCallback(Item item) {
  print('user tapped on $item');
}
```

This works okay, but for an app state that you need to modify from
many different places, you'd have to pass around a lot of
callbacks&mdash;which gets old pretty quickly.

這段程式碼是沒問題的，但是對於全域應用狀態來說，你需要在不同的地方進行修改，
可能需要大量傳遞迴調函式&mdash;&mdash;這些回呼(Callback)很快就會過時。

Fortunately, Flutter has mechanisms for widgets to provide data and
services to their descendants (in other words, not just their children,
but any widgets below them). As you would expect from Flutter,
where _Everything is a Widget™_, these mechanisms are just special
kinds of widgets&mdash;`InheritedWidget`, `InheritedNotifier`,
`InheritedModel`, and more. We won't be covering those here,
because they are a bit low-level for what we're trying to do.

幸運的是 Flutter 在 widget 中存在一種機制，能夠為其子孫節點提供資料和服務。
（換言之，不僅僅是它的子節點，所有在它下層的 widget 都可以）。就像你所瞭解的，
Flutter 中的 _Everything is a Widget™_。
這裡的機制也是一種 widget &mdash;`InheritedWidget`, `InheritedNotifier`,
`InheritedModel`等等。我們這裡不會詳細解釋他們，因為這些 widget 都太底層。

Instead, we are going to use a package that works with the low-level
widgets but is simple to use. It's called `provider`.

我們會用一個 package 來和這些底層的 widget 打交道，
就是 `provider` package。

Before working with `provider`,
don't forget to add the dependency on it to your `pubspec.yaml`.

在使用 `provider` 之前，請不要忘記在
`pubspec.yaml` 檔案里加入依賴。

To add the `provider` package as a dependency, run `flutter pub add`:

執行 `flutter pub add` 將 `provider` 新增為依賴：

```terminal
$ flutter pub add provider
```

Now you can `import 'package:provider/provider.dart';`
and start building.

現在可以在程式碼里加入 `import 'package:provider/provider.dart';`
進而開始建構你的應用了/

With `provider`, you don't need to worry about callbacks or
`InheritedWidgets`. But you do need to understand 3 concepts:

`provider` package 中，你無須關心回呼(Callback)或者 `InheritedWidgets`。
但是你需要理解三個概念：

* ChangeNotifier
* ChangeNotifierProvider
* Consumer


## ChangeNotifier

`ChangeNotifier` is a simple class included in the Flutter SDK which provides
change notification to its listeners. In other words, if something is
a `ChangeNotifier`, you can subscribe to its changes. (It is a form of
Observable, for those familiar with the term.)

`ChangeNotifier` 是 Flutter SDK 中的一個簡單的類別。它用於向監聽器傳送通知。
換言之，如果被定義為 `ChangeNotifier`，你可以訂閱它的狀態變化。
（這和大家所熟悉的觀察者模式相類似）。

In `provider`, `ChangeNotifier` is one way to encapsulate your application
state. For very simple apps, you get by with a single `ChangeNotifier`.
In complex ones, you'll have several models, and therefore several
`ChangeNotifiers`. (You don't need to use `ChangeNotifier` with `provider`
at all, but it's an easy class to work with.)

在 `provider` 中，`ChangeNotifier` 是一種能夠封裝應用程式狀態的方法。
對於特別簡單的程式，你可以透過一個 `ChangeNotifier` 來滿足全部需求。
在相對複雜的應用中，由於會有多個模型，所以可能會有多個 `ChangeNotifier`。
(不是必須得把 `ChangeNotifier` 和 `provider` 結合起來用，
不過它確實是一個特別簡單的類)。

In our shopping app example, we want to manage the state of the cart in a
`ChangeNotifier`. We create a new class that extends it, like so:

在我們的購物應用範例中，我們打算用 `ChangeNotifier` 來管理購物車的狀態。
我們建立一個新類，繼承它，像下面這樣：

<?code-excerpt "lib/src/provider.dart (model)" replace="/ChangeNotifier/[!$&!]/g;/notifyListeners/[!$&!]/g"?>
```dart
class CartModel extends [!ChangeNotifier!] {
  /// Internal, private state of the cart.
  final List<Item> _items = [];

  /// An unmodifiable view of the items in the cart.
  UnmodifiableListView<Item> get items => UnmodifiableListView(_items);

  /// The current total price of all items (assuming all items cost $42).
  int get totalPrice => _items.length * 42;

  /// Adds [item] to cart. This and [removeAll] are the only ways to modify the
  /// cart from the outside.
  void add(Item item) {
    _items.add(item);
    // This call tells the widgets that are listening to this model to rebuild.
    [!notifyListeners!]();
  }

  /// Removes all items from the cart.
  void removeAll() {
    _items.clear();
    // This call tells the widgets that are listening to this model to rebuild.
    [!notifyListeners!]();
  }
}
```

The only code that is specific to `ChangeNotifier` is the call
to `notifyListeners()`. Call this method any time the model changes in a way
that might change your app's UI. Everything else in `CartModel` is the
model itself and its business logic.

唯一一行和 `ChangeNotifier` 相關的程式碼就是呼叫 `notifyListeners()`。
當模型發生改變並且需要更新 UI 的時候可以呼叫該方法。
而剩下的程式碼就是 `CartModel` 和它本身的業務邏輯。

`ChangeNotifier` is part of `flutter:foundation` and doesn't depend on
any higher-level classes in Flutter. It's easily testable (you don't even need
to use [widget testing][] for it). For example,
here's a simple unit test of `CartModel`:

`ChangeNotifier` 是 `flutter:foundation` 的一部分，
而且不依賴 Flutter 中任何高級別類別。測試起來非常簡單
（你都不需要使用 [widget 測試][widget testing]）。
比如，這裡有一個針對 `CartModel` 簡單的單元測試：

<?code-excerpt "test/model_test.dart (test)"?>
```dart
test('adding item increases total cost', () {
  final cart = CartModel();
  final startingPrice = cart.totalPrice;
  var i = 0;
  cart.addListener(() {
    expect(cart.totalPrice, greaterThan(startingPrice));
    i++;
  });
  cart.add(Item('Dash'));
  expect(i, 1);
});
```


## ChangeNotifierProvider

`ChangeNotifierProvider` is the widget that provides an instance of
a `ChangeNotifier` to its descendants. It comes from the `provider` package.

`ChangeNotifierProvider` widget 可以向其子孫節點暴露一個 `ChangeNotifier` 例項。
它屬於 `provider` package。

We already know where to put `ChangeNotifierProvider`: above the widgets that
need to access it. In the case of `CartModel`, that means somewhere
above both `MyCart` and `MyCatalog`.

我們已經知道了該把 `ChangeNotifierProvider` 放在什麼位置：
在需要存取它的 widget 之上。在 `CartModel` 裡，
也就意味著將它置於 `MyCart` 和 `MyCatalog` 之上。

You don't want to place `ChangeNotifierProvider` higher than necessary
(because you don't want to pollute the scope). But in our case,
the only widget that is on top of both `MyCart` and `MyCatalog` is `MyApp`.

你肯定不願意把 `ChangeNotifierProvider` 
放的級別太高（因為你不希望破壞整個結構）。
但是在我們這裡的例子中，`MyCart` 和 `MyCatalog` 之上只有 `MyApp`。

<?code-excerpt "lib/main.dart (main)" replace="/ChangeNotifierProvider/[!$&!]/g"?>
```dart
void main() {
  runApp(
    [!ChangeNotifierProvider!](
      create: (context) => CartModel(),
      child: const MyApp(),
    ),
  );
}
```

Note that we're defining a builder that creates a new instance
of `CartModel`. `ChangeNotifierProvider` is smart enough _not_ to rebuild
`CartModel` unless absolutely necessary. It also automatically calls
`dispose()` on `CartModel` when the instance is no longer needed.

請注意我們定義了一個 builder 來建立一個 `CartModel` 的例項。
`ChangeNotifierProvider` 非常聰明，它 **不會** 重複例項化 `CartModel`，
除非在個別場景下。如果該例項已經不會再被呼叫，
`ChangeNotifierProvider` 也會自動呼叫 `CartModel` 的 `dispose()` 方法。

If you want to provide more than one class, you can use `MultiProvider`:

如果你想提供更多狀態，可以使用 `MultiProvider`：

<?code-excerpt "lib/main.dart (multi-provider-main)" replace="/multiProviderMain/main/g;/MultiProvider/[!$&!]/g"?>
```dart
void main() {
  runApp(
    [!MultiProvider!](
      providers: [
        ChangeNotifierProvider(create: (context) => CartModel()),
        Provider(create: (context) => SomeOtherClass()),
      ],
      child: const MyApp(),
    ),
  );
}
```

## Consumer

Now that `CartModel` is provided to widgets in our app through the
`ChangeNotifierProvider` declaration at the top, we can start using it.

現在 `CartModel` 已經透過 `ChangeNotifierProvider` 在應用中與 widget 相關聯。
我們可以開始呼叫它了。

This is done through the `Consumer` widget.

完成這一步需要透過 `Consumer` widget。

<?code-excerpt "lib/src/provider.dart (descendant)" replace="/Consumer/[!$&!]/g"?>
```dart
return [!Consumer!]<CartModel>(
  builder: (context, cart, child) {
    return Text('Total price: ${cart.totalPrice}');
  },
);
```

We must specify the type of the model that we want to access.
In this case, we want `CartModel`, so we write
`Consumer<CartModel>`. If you don't specify the generic (`<CartModel>`),
the `provider` package won't be able to help you. `provider` is based on types,
and without the type, it doesn't know what you want.

我們必須指定要存取的模型型別。在這個範例中，我們要存取 `CartModel` 那麼就寫上 `Consumer<CartModel>`。

The only required argument of the `Consumer` widget
is the builder. Builder is a function that is called whenever the
`ChangeNotifier` changes. (In other words, when you call `notifyListeners()`
in your model, all the builder methods of all the corresponding
`Consumer` widgets are called.)

`Consumer`  widget 唯一必須的引數就是 builder。 當 `ChangeNotifier`
發生變化的時候會呼叫 builder 這個函式。
（換言之，當你在模型中呼叫 `notifyListeners()` 時，
所有相關的 `Consumer` widget 的 builder 方法都會被呼叫。）

The builder is called with three arguments. The first one is `context`,
which you also get in every build method.

builder 在被呼叫的時候會用到三個引數。第一個是 `context`。
在每個 build 方法中都能找到這個引數。

The second argument of the builder function is the instance of
the `ChangeNotifier`. It's what we were asking for in the first place.
You can use the data in the model to define what the UI should look like
at any given point.

builder 函式的第二個引數是 `ChangeNotifier` 的例項。
它是我們最開始就能得到的例項。你可以透過該例項定義 UI 的內容。

The third argument is `child`, which is there for optimization.
If you have a large widget subtree under your `Consumer`
that _doesn't_ change when the model changes, you can construct it
once and get it through the builder.

第三個引數是 `child`，用於最佳化目的。如果 `Consumer` 下面有一個龐大的子樹，
當模型發生改變的時候，該子樹 **並不會** 改變，
那麼你就可以僅僅建立它一次，然後透過 builder 獲得該例項。

<?code-excerpt "lib/src/performance.dart (child)" replace="/\bchild\b/[!$&!]/g"?>
```dart
return Consumer<CartModel>(
  builder: (context, cart, [!child!]) => Stack(
    children: [
      // Use SomeExpensiveWidget here, without rebuilding every time.
      if ([!child!] != null) [!child!],
      Text('Total price: ${cart.totalPrice}'),
    ],
  ),
  // Build the expensive widget here.
  [!child!]: const SomeExpensiveWidget(),
);
```

It is best practice to put your `Consumer` widgets as deep in the tree
as possible. You don't want to rebuild large portions of the UI
just because some detail somewhere changed.

最好能把 `Consumer` 放在 widget 樹儘量低的位置上。
你總不希望 UI 上任何一點小變化就全盤重新建構 widget 吧。

<?code-excerpt "lib/src/performance.dart (nonLeafDescendant)"?>
```dart
// DON'T DO THIS
return Consumer<CartModel>(
  builder: (context, cart, child) {
    return HumongousWidget(
      // ...
      child: AnotherMonstrousWidget(
        // ...
        child: Text('Total price: ${cart.totalPrice}'),
      ),
    );
  },
);
```

Instead:

換成：

<?code-excerpt "lib/src/performance.dart (leafDescendant)"?>
```dart
// DO THIS
return HumongousWidget(
  // ...
  child: AnotherMonstrousWidget(
    // ...
    child: Consumer<CartModel>(
      builder: (context, cart, child) {
        return Text('Total price: ${cart.totalPrice}');
      },
    ),
  ),
);
```

### Provider.of

Sometimes, you don't really need the _data_ in the model to change the
UI but you still need to access it. For example, a `ClearCart`
button wants to allow the user to remove everything from the cart.
It doesn't need to display the contents of the cart,
it just needs to call the `clear()` method.

有的時候你不需要模型中的 **資料** 來改變 UI，但是你可能還是需要存取該資料。
比如，`ClearCart` 按鈕能夠清空購物車的所有商品。
它不需要顯示購物車裡的內容，只需要呼叫 `clear()` 方法。

We could use `Consumer<CartModel>` for this,
but that would be wasteful. We'd be asking the framework to
rebuild a widget that doesn't need to be rebuilt.

我們可以使用 `Consumer<CartModel>` 來實現這個效果，
不過這麼實現有點浪費。因為我們讓整體框架重構了一個無需重構的 widget。

For this use case, we can use `Provider.of`,
with the `listen` parameter set to `false`.

所以這裡我們可以使用 `Provider.of`，
並且將 `listen` 設定為 `false`。

<?code-excerpt "lib/src/performance.dart (nonRebuilding)" replace="/listen: false/[!$&!]/g"?>
```dart
Provider.of<CartModel>(context, [!listen: false!]).removeAll();
```

Using the above line in a build method won't cause this widget to
rebuild when `notifyListeners` is called.

在 build 方法中使用上面的程式碼，當 `notifyListeners` 被呼叫的時候，
並不會使 widget 被重構。


## Putting it all together

## 把程式碼整合在一起

You can [check out the example][] covered in this article.
If you want something simpler,
see what the simple Counter app looks like when
[built with `provider`][].

你可以在文章中 [檢視這個範例][check out the example]。
如果你想參考稍微簡單一點的範例，可以看看 Counter 應用程式是如何
[基於 `provider` 實現的][built with `provider`]。

By following along with these articles, you've greatly 
improved your ability to create state-based applications. 
Try building an application with `provider` yourself to 
master these skills. 

透過跟著這些文章的學習，你已經大大提高了
建立一個包含狀態管理應用的能力。
試著自己用 `provider` 建構一個應用來掌握這些技能吧！


[built with `provider`]: {{site.github}}/flutter/samples/tree/main/provider_counter
[check out the example]: {{site.github}}/flutter/samples/tree/main/provider_shopper
[declarative UI programming]: {{site.url}}/data-and-backend/state-mgmt/declarative
[ephemeral and app state]: {{site.url}}/data-and-backend/state-mgmt/ephemeral-vs-app
[options page]: {{site.url}}/data-and-backend/state-mgmt/options
[widget testing]: {{site.url}}/testing/overview#widget-tests
