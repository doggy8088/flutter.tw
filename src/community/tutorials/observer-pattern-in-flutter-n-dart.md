---
title: 流動的觀察者模式
toc: true
---

文/ 楊加康，CFUG 社群成員，《Flutter 開發之旅從南到北》作者，小米工程師

**觀察者模式**，又稱釋出訂閱模式，是一種行為設計模式——你可以定義一種訂閱機制，可在物件事件發生時通知多個 **觀察** 該物件的其他物件。

>> 觀察者模式定義了一種一對多的依賴關係，讓多個觀察者物件同時監聽某一個主題物件。
>>
>> 這個主題物件在狀態上發生變化時，會通知所有觀察者物件，讓它們能夠自動更新自己。

![](https://files.flutter-io.cn/posts/community/tutorial/images/2021-08-14-ObserverPattern.png)

從定義中，不難發現，**觀察者** 與 **被觀察者 / 釋出者** 是這個模式中最重要的組成元素。

![](https://files.flutter-io.cn/posts/community/tutorial/images/2021-08-14-Observer1.png)

微信的公眾號可以被視為生活中最典型的觀察者模式的例子。如果你訂閱了「Flutter社群」，每當 Flutter 社群釋出文章時，就會給你及其他訂閱者推送這個訊息，這其中你就是 **觀察者**，公眾號「Flutter社群」就是 **被觀察者 (Observable) 或釋出者 (Subject)**。

觀察者模式經常被應用在這類事件處理系統中，從概念上理解，被觀察者也經常被稱作是 **事件流 (stream of events)** 或者說是 **事件流的來源 (stream source of events)**，而觀察者相當於 **事件接收器 (sinks of events)**。

同時，觀察者模式也是實現 **響應式程式設計** 的基礎，RxDart、EventBus 等庫都是觀察者模式下的產物。

## 面向物件

面向物件中，觀察者和和釋出者 (被觀察者) 分別對應兩個類 (Observer 和 Subject) 的物件。

![觀察者模式 UML 圖，圖源維基百科](https://files.flutter-io.cn/posts/community/tutorial/images/2021-08-04-2880px-Observer_w_update.svg.png)

釋出類 (Subject) 中通常會有提供給每個物件訂閱或取消訂閱釋出者事件流的 **訂閱機制**，包括：

1. 一個用於儲存訂閱者物件參考的列表成員變數；
2. 幾個用於新增或刪除該列表中訂閱者的公有方法。

```dart
// 被觀察者
class Subject {
  List<Observer> _observers;
  Subject([List<Observer> observers]) {
    _observers = observers ?? [];
  }

  // 註冊觀察者
  void registerObserver(Observer observer) {
    _observers.add(observer);
  }
  
  // 解註冊觀察者
  void unregisterObserver(Observer observer) {
    _observers.remove(observer)
  }

  // 通知觀察者
  void notifyobservers(Notification notification) {
    for (var observer in _observers) {
      observer.notify(notification);
    }
  }
}
```

此時，每當事件發生，它只需遍歷訂閱者並呼叫其物件的特定通知方法即可 (如上面程式碼中的 `notifyobservers` 方法) 。

實際應用中，一個釋出者通常會對應多個訂閱者，且釋出者與訂閱者應當遵循面向物件的開發設計原則，因此：

1. 為了避免耦合，訂閱者們必須實現同樣的介面；
2. 釋出者僅透過該介面與訂閱者互動，介面方法可以宣告引數， 這樣釋出者在發出通知時就能傳遞一些上下文資料 (如下面程式碼中的 notification 物件) 。

```dart
// 觀察者
class Observer {
  String name;
  
  Observer(this.name);

  void notify(Notification notification) {
    print("[${notification.timestamp.toIso8601String()}] Hey $name, ${notification.message}!");
  }
}
```

這樣，我們可以得出如下這樣用 Dart 語言實現的觀察者模式了，下面是一個簡單的應用：

```dart
// 具體的被觀察者 CoffeeMaker
// 每當 Coffee 製作完成發出通知給觀察者。
class CoffeeMaker extends Subject {
  CoffeeMaker([List<Observer> observers]) : super(observers);
  
  void brew() {
    print("Brewing the coffee...");
    notifyobservers(Notification.forNow("coffee's done"));
  }
}

void main() {
  var me = Observer("Tyler");
  var mrCoffee = CoffeeMaker(List.from([me]));
  var myWife = Observer("Kate");
  mrCoffee.registerObserver(myWife);
  mrCoffee.brew();
}
```

這裡的 CoffeeMaker 繼承自 Subject，作為一個具體的釋出類，`brew()` 方法是其內部，每當咖啡製作完成後，用於通知其他各個觀察者的方法。上面程式碼中，我們在 `mrCoffee` 這台咖啡機上註冊了 `myWife` 這一個觀察者，`mrCoffee.brew();` 觸發後，`myWife` 內部的 `notify`  方法就會被呼叫。

觀察者模式很好的實現了他們兩者之間釋出訂閱的關係，在實際應用中，被觀察者正在處理的事件很可能是非同步的，而作為觀察者不必顯示的去阻塞等待事件的完成，而是由被觀察者通知，當事件完成後，再將事件主動地「推」給關心這個事件的觀察者。與之相對的，有一類觀察者也會使用後臺執行緒時刻輪詢地監聽著其關心的主題事件，這個話題我們暫不展開。

觀察者模式使用不慎的話，也很容易出現傳說中的 **失效監聽器** 問題，導致記憶體洩漏，因為在基本實現中，被觀察者依然持有觀察者的強參考，如果事件中途，被觀察者已經不存在時或不再關心此事件，就會導致觀察者無法被回收，因此，我們在這種情況下應當在被觀察中做好取消訂閱的機制，及時釋放無用的資源。

## Dart

Stream 可以被看作是 Dart 語言原生支援的觀察者模式的典型模型之一，它本身是 `Dart:async` 套件中一個用於非同步操作的類，響應式程式設計庫 RxDart 也是基於 Stream 封裝而成的。

從概念上講，我們可以將 Stream 看做是一個可以連線兩端的傳送帶，作為開發者，我們可以在傳送帶的一端放入資料，Stream 就會將這些資料傳送到另一端。

![](https://files.flutter-io.cn/posts/community/tutorial/images/2021-08-12-stream.svg)

和現實中的情況類似，如果傳送帶的另一端沒有人接受資料，這些資料就會被程式丟棄，因此，我們通常會在傳送到尾端安排一個接收資料的物件，在響應式程式設計中，它被稱為資料的觀察者。

![](https://files.flutter-io.cn/posts/community/tutorial/images/2021-08-12-stream4.svg)

如果說上文 Dart 面向物件中，觀察者和被觀察者兩者的關係是在儘量保持低耦合的情況下而形成的，相對獨立。那麼在響應式程式設計中，它們的關係就是變得更加緊密的 **上游與下游** 的關係。

因為 Stream，顧名思義，就是「流」的含義，被觀察者在流的入口產生事件，觀察者則在流的出口等待資料或事件的到來。

![](https://files.flutter-io.cn/posts/community/tutorial/images/2021-08-14-Observer2.png)

在這套流程裡，觀察者的 **訂閱** 與被觀察者的 **事件釋出** 等一系列操作都直接在 Stream 或者說是框架內部完成的。

Dart 中，我們可以使用 StreamController 來建立流：

```dart
var controller = new StreamController<int>();

controller.add(1); // 將資料放入流中
```

如上面程式碼所示，建立 `StreamController` 時必須指定泛型型別來定義可以加入 `Stream` 的資料物件，上面的 `controller` 可以接受 `int` 型別的資料，我們使用它的 `add` 方法就可以將資料放入到它的傳送帶中。

如果我們直接執行上面的兩行程式碼，最終並不會不到任何結果，因為我們還沒有為傳送帶設定接收資料的物件：

```dart
var controller = new StreamController<int>();

controller.stream.listen((item) => print(item)); // 資料觀察者函式

controller.add(1);
controller.add(2);
controller.add(3);
```

上面的程式碼中，我們透過呼叫 StreamController 內部的 stream 物件的 listen 方法，就可以為 controller 物件新增監聽這個 Stream 事件的觀察者，這個方法接受一個回呼(Callback)函式，這個回呼(Callback)函式又接受一個我們在 `new StreamController<int>()` 泛型中宣告的資料物件作為引數。

這時，每當我們再次透過 `add` 方法將資料放入傳送帶後，就會通知觀察者，呼叫這個函式，並將傳遞的資料打印出來：

```dart
1
2
3
```

另外，我們也可以使觀察者在某個時間段後停止監聽 Stream 中傳遞的資料，在上面程式碼中的 `listen` 函式會返回一個  `StreamSubscription` 型別的訂閱物件，當我們呼叫它的 `.cancel()` 後就會釋放這個觀察者，不再接收資料：

```dart
var controller = new StreamController<String>();

StreamSubscription subscription = controller.stream.listen((item) => print(item));

controller.add(1);
controller.add(2);
controller.add(3);

await Future.delayed(Duration(milliseconds: 500));

subscription.cancel();
```

## Flutter

### ChangeNotifier

ChangeNotifier 大概是 Flutter 中實現觀察者模式最典型的例子了，它實現自 Listenable，內部維護一個 `_listeners` 列表用來存放觀察者，並實現了 `addListener`、`removeListener` 等方法來完成其內部的訂閱機制：

```dart
class ChangeNotifier implements Listenable {
  LinkedList<_ListenerEntry>? _listeners = LinkedList<_ListenerEntry>();

  @protected
  bool get hasListeners {
    return _listeners!.isNotEmpty;
  }
  
  @override
  void addListener(VoidCallback listener) {
    _listeners!.add(_ListenerEntry(listener));
  }

  @override
  void removeListener(VoidCallback listener) {
    for (final _ListenerEntry entry in _listeners!) {
      if (entry.listener == listener) {
        entry.unlink();
        return;
      }
    }
  }

  @mustCallSuper
  void dispose() {
    _listeners = null;
  }

  @protected
  @visibleForTesting
  void notifyListeners() {
    if (_listeners!.isEmpty)
      return;
    final List<_ListenerEntry> localListeners = List<_ListenerEntry>.from(_listeners!);
    for (final _ListenerEntry entry in localListeners) {
      try {
        if (entry.list != null)
          entry.listener();
      } catch (exception, stack) {
        // ...
      }
    }
  }
}
```

在實際使用時，我們只需要繼承 ChangeNotifier 便能具備這種訂閱機制，如下這個 CartModel 類：

```dart
class CartModel extends ChangeNotifier {
  final List<Item> _items = [];

  UnmodifiableListView<Item> get items => UnmodifiableListView(_items);

  int get totalPrice => _items.length * 42;

  void add(Item item) {
    _items.add(item);
    notifyListeners();
  }

  void removeAll() {
    _items.clear();
    notifyListeners();
  }
}
```

`CartModel` 內部維護一個 `_items` 陣列，`add`、`removeAll` 方法時提供給外部操作該陣列的介面，每當 `_items` 改變則會呼叫 `notifyListeners()` 通知它的所有觀察者。

`ChangeNotifier` 作為 `flutter:foundation` 中最基礎的類，不依賴其他任何上層的類，測試起來也非常簡單，我們可以針對 `CartModel` 做一個簡單的單元測試：

```dart
test('adding item increases total cost', () {
  final cart = CartModel();
  final startingPrice = cart.totalPrice;
  cart.addListener(() {
    expect(cart.totalPrice, greaterThan(startingPrice));
  });
  cart.add(Item('Dash'));
});
```

這裡，當我們呼叫 `cart.add(Item('Dash'));` 後，就是會觸發觀察者函式的呼叫，實現一種由資料的改變驅動事件執行的機制。

Flutter 應用中最傳統的狀態管理方案是使用有狀態 widget 的 `setState` 的方法，這種方式暴露出來的問題是，大型應用中的 widget 樹會非常複雜，每當狀態更新呼叫 `setState` 時，則會牽一髮而動全身，重建所有子樹，使效能大打折扣。

那麼，當將 `ChangeNotifier` 觀察者模式應用在狀態管理方案中時，便能解決這個問題。設想讓每一個最小元件充當觀察者，觀察應用的狀態，每當狀態改變時即驅動該區域性小元件更新，是不是就能達到這種目的。我們常用 provider 庫就應用了這個原理。

provider 內部提供了一個 ChangeNotifierProvider widget，可以向其子元件暴露一個 ChangeNotifier 例項 (被觀察者) ：

```dart
void main() {
  runApp(
    ChangeNotifierProvider(
      create: (context) => CartModel(),
      child: const MyApp(),
    ),
  );
}
```

在子元件中，只需要使用 Consumer widget 註冊觀察者元件，就能接收到 `CartModel` 內部資料更新的通知：

```dart
return Consumer<CartModel>(
  builder: (context, cart, child) {
    return Text("Total price: ${cart.totalPrice}");
  },
);
```

這裡，使用 Consumer 必須指定要觀察的 ChangeNotifier 型別，我們要存取 `CartModel` 那麼就寫上 `Consumer<CartModel>`，builder 最為 Consumer 唯一一個必要引數，用來建構展示在頁面中的子元件。

當 `ChangeNotifier` 發生變化的時候會呼叫 builder 這個函式。 (換言之，當呼叫 `CartModel` 的  `notifyListeners()` 方法時，所有相關的 `Consumer` widget 的 builder 方法都會被呼叫。) ，重建子樹，達到區域性更新狀態的目的。

### Navigator

路由是在 Flutter 應用中常去討論的話題，在整個應用執行過程中，路由操作也都需要被時刻關注著，它是我們瞭解使用者行為的一種有效的方式。Flutter 提供了一套很方便的觀察者模式的模型幫助我們實現這個功要求。

Flutter 中每個 Navigator 物件都接受一個 NavigatorObserver 物件的陣列，在實際開發過程中，我們可以透過根元件 `MaterialApp` (或 `CupertinoPageRoute`)  的 `navigatorObservers` 屬性傳遞給根 Navigator 元件，用於觀察根 Navigator 的路由行為，這一組 NavigatorObserver 物件就是一系列的路由觀察者。

```dart
 Widget build(BuildContext context) {
    return new MaterialApp(
      navigatorObservers: [new MyNavigatorObserver()],
      home: new Scaffold(
        body: new MyPage(),
      ),
    );
  }
```

路由觀察者們統一繼承自 RouteObserver，範型型別為 PageRoute，這時，它就能監聽 CupertinoPageRoute 和 MaterialPageRoute 兩種型別的路由了：

```dart
class MyRouteObserver extends RouteObserver<PageRoute<dynamic>> {

  // 監聽導航器的 push 操作
  @override
  void didPush(Route<dynamic> route, Route<dynamic> previousRoute) {
    super.didPush(route, previousRoute);
    if (previousRoute is PageRoute && route is PageRoute) {
      print('${previousRoute.settings.name} => ${route.settings.name}');
    }
  }

  // 監聽導航器的 replace 操作
  @override
  void didReplace({Route<dynamic> newRoute, Route<dynamic> oldRoute}) {
    super.didReplace(newRoute: newRoute, oldRoute: oldRoute);
    if (newRoute is PageRoute) {
      print('${oldRoute.settings.name} => ${oldRoute.settings.name}');
    }
  }

  // 監聽導航器的 pop 操作
  @override
  void didPop(Route<dynamic> route, Route<dynamic> previousRoute) {
    super.didPop(route, previousRoute);
    if (previousRoute is PageRoute && route is PageRoute) {
      print('${route.settings.name} => ${previousRoute.settings.name}');
    }
  }
}
```

在我們做實際路由操作，呼叫 `Navigator` 的 `pop`，`push` 等方法時，就會按照慣例遍歷呼叫這些觀察者物件對應的方法：

```dart
 Future<T> push<T extends Object>(Route<T> route) {
  // ...
  for (NavigatorObserver observer in widget.observers)
    observer.didPush(route, oldRoute);
	// ...
}
```

這樣，觀察者模式在 Flutter 路由中又完成了這個非常重要的任務。

## 本文小結

本文內容到這裡就結束了，觀察者模式的場景例子數不勝數，在實際開發中，我們也會經常需要使用到，但我們要記住的是設計模式的運用並不是套用模版，而是要根據實際場景找到最合適的解決方案。

對於行為型模式來說，觀察者模式將被觀察者與觀察者這兩件事物抽象出來，實現了程式碼上的解藕，在實際場景中，觀察者可能是關心某種狀態的元件，監聽某個事件的監聽器等等，整體的設計也會變得更加直觀，希望大家能在以後的開發中多多使用。

## 拓展閱讀

- [《Flutter開發之旅從南到北》](https://item.jd.com/12757223.html) —— 第8章 路由管理 & 第9章 狀態管理
- [觀察者模式 wikipedia](https://en.wikipedia.org/wiki/Observer_pattern)
- [Design Patterns in Dart](https://scottt2.github.io/design-patterns-in-dart/observer/)
- [什麼是Stream](https://juejin.cn/post/6844903686737494023)
- [簡單的應用狀態管理](https://flutter.tw/data-and-backend/state-mgmt/simple)

## 關於本系列文章

Flutter / Dart 設計模式從南到北 (簡稱 Flutter 設計模式) 系列內容由 CFUG 社群成員、《Flutter 開發之旅從南到北》作者、小米工程師楊加康撰寫併發布在 Flutter 社群公眾號和 flutter.cn 網站的社群課程節目。

本系列預計兩週釋出一篇，著重向開發者介紹 Flutter 應用開發中常見的設計模式以及開發方式，旨在推進 Flutter / Dart 語言特性的普及，以及幫助開發者更高效地開發出高品質、可維護的 Flutter 應用。
