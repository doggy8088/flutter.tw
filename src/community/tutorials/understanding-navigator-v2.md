---
title: Flutter Navigator 2.0 指南與原理解析
toc: true
---

文/ 楊加康

Flutter 1.22 釋出後，大家可以發現，
官方對路由相關 API 的改動很大，
設計文件中表示，由於傳統的命令式 API，
如 `Navigator.pop()`、`Navigator.push()` 等介面，
並**沒有給開發者一種靈活的方式去直接管理路由棧**，
甚至覺得已經過時了，一點也不 Flutter。

{{site.alert.note}}

As mentioned by a participant in one of Flutter's user studies, 
the API also feels outdated and not very Flutter-y.

{{site.alert.end}}

而 Navigator 2.0 引入了一套全新的宣告式 API，
全新的實現方式與呼叫方法與以往都截然不同，
在官方推薦的 [Learning Flutter’s new navigation and routing system](https://medium.com/flutter/learning-flutters-new-navigation-and-routing-system-7c9068155ade)
（譯文：[Flutter Navigator 2.0 全面解析](https://mp.weixin.qq.com/s/zGpzJahDSTZDhWqYmkzi5g)）文章中，
許多讀者也表示並不能立即適應 Navigator 2.0 的一些反差。

本文就來帶領讀者們進一步深入 Navigator 2.0 的基本原理，
幫助大家從中探索出最佳的使用方式。

## 為什麼需要新的 API

在探究具體細節之前，我們有必要了解一下 
Flutter 團隊為什麼要不惜這些代價對 Navigator API 做這次的重構，
主要有如下幾點原因。

- **原始 API 中的 `initialRoute` 引數，
  即系統預設的初始頁面，在應用執行後就不能再更改了**。
  這種情況下，如果使用者接收到一個系統通知，
  點選後想要從當前的路由棧狀態 [Main -> Profile -> Settings] 
  重啟切換到新的 [Main -> List -> Detail[id=24] 路由棧，
  舊的 Navigator API 並沒有一種優雅的實現方式實現這種效果。

- **原始的命令式 Navigator API 只提供給了
  開發者一些非常針對性的介面，如 `push()`、`pop()` 等，
  而沒有給出一種更靈活的方式讓我們直接操作路由棧**。
  這種做法其實與 Flutter 理念相違背，
  試想如果我們想要改變某個 widget 的所有子元件
  只需要重建所有子元件並且建立一系列新的 widget 即可，
  而將此概念應用在路由中，
  當應用中存在一系列路由頁面並想要更改時，
  我們只能呼叫 `push()`、`pop()` 這類介面來回操作，
  **這樣的 Flutter 食之無味**。

- **巢狀(Nesting)路由下，手機裝置自帶的回退按鈕
  只能由根 Navigator 響應**。
  在目前的應用中，我們很多場景都需要
  在某個子 tab 內單獨管理一個子路由棧。
  假設有這個場景，使用者在子路由棧中
  做一系列路由操作之後，點選系統回退按鈕，
  消失的將是整個上層的根路由，
  我們當然可以使用某種措施來避免這種狀況，
  但歸咎起來，這也不應該是應用開發者應該考慮的問題。

於是，Navigator 2.0 就肩負著這千里之任來了。

## Navigator 2.0

Navigator 2.0 新增的宣告式 API 主要包含
[Page](https://api.flutter.dev/flutter/widgets/Page-class.html) API、[Router](https://api.flutter.dev/flutter/widgets/Router-class.html) API 兩個部分，
它們各自強大的功能為 Navigator 2.0 提供了強有力的基石，
本節我就帶讀者們看看它們各自的實現細節。

### Page

Page 是 Navigator 2.0 中最常見的類之一，
從名字就能知道它的含義就是 “**頁面**”，
正如 widget 就是 **元件** 一樣，
但 Page 與 Widget 的關係也更加微妙。

與 Flutter 中 Widget、Element、
RenderObject 三棵樹的概念保持一致。
Widget 只儲存元件配置資訊，
框架層內建了一個 `createElement()` 可以建立
與之對應的 Element 例項。
Page 同樣只儲存頁面路由相關資訊，
框架層也存在一個 `createRoute()` 方法
可以建立與之對應的 Route 例項。

![](https://devrel.andfun.cn/devrel/posts/2020/11/bc57589cd7882.png)

English placeholder for the translation toggle tools issue.

Widget 和 Page 中也都有一個 `canUpdate()` 方法，
幫助 Flutter 判斷其是否已更新或改變：

<!--skip-->
```dart
// Page
bool canUpdate(Page<dynamic> other) {
  return other.runtimeType == runtimeType &&
         other.key == key;
}

// Widget
static bool canUpdate(Widget oldWidget, Widget newWidget) {
  return oldWidget.runtimeType == newWidget.runtimeType
      && oldWidget.key == newWidget.key;
}
```

甚至連比較的條件都是 **執行時型別與 key**。

而在程式碼層面，Page 類就繼承自我們在
舊的 Navigator API 用過的 `RouteSettings`：

<!--skip-->
```
abstract class Page<T> extends RouteSettings
```

其中就儲存了包含路由名稱（name，如 "/settings"）和
路由引數 (arguments) 等資訊。

#### pages 引數

在新的 Navigator 元件中，新增了一個 `pages` 引數，
它接受的就是一個 Page 物件列表，如下這段程式碼：

<!--skip-->
```dart
class _MyAppState extends State<MyApp> {
  final pages = [
    MyPage(
      key: Key('/'),
      name: '/',
      builder: (context) => HomeScreen(),
    ),
    MyPage(
      key: Key('/category/5'),
      name: '/category/5',
      builder: (context) => CategoryScreen(id: 5),
    ),
    MyPage(
      key: Key('/item/15'),
      name: '/item/15',
      builder: (context) => ItemScreen(id: 15),
    ),
  ];

  @override
  Widget build(BuildContext context) {
    return //...
      Navigator(
          key: _navigatorKey,
          pages: List.of(pages),
        ),
  }
}
```

此時，執行應用，**Flutter 就會根據這裡 pages 列表中的
所有 Page 物件在底層的路由棧產生對應的 Route 例項**，
即與 pages 對應的三個路由頁面。

應用開啟某個頁面，就表示在 pages 中新增一個 Page 物件，
系統接收到上層的 pages 改變的通知後
就會 **比較新的 pages 與舊的 pages**，
根據比較結果，Flutter 就會在底層路由棧中新產生一個 Route 例項，
這樣一個新的頁面就算開啟成功了。

<!--skip-->
```dart
void addPage(MyPage page) {
  setState(() => pages.add(page));
}
```

Navigator 元件同樣也新增了一個 `onPopPage` 引數，
接受一個回呼(Callback)函式來響應頁面的 pop 事件，
如下面程式碼中的 `_onPopPage` 函式：

<!--skip-->
```dart
class _MyAppState extends State<MyApp> {
  bool _onPopPage(Route<dynamic> route, dynamic result) {
    setState(() => pages.remove(route.settings));
    return route.didPop(result);
  }

  @override
  Widget build(BuildContext context) {
    print('build: $pages');
    return // ...
      Navigator(
        key: _navigatorKey,
        onPopPage: _onPopPage,
        pages: List.of(pages),
      )
  }
}
```

當我們呼叫 `Navigator.pop()` 關閉某個頁面時，
即能觸發這個函式呼叫，
而函式接受到的 route 物件引數就表示需要在 pages 中被移除的頁面，
在這裡，我們順勢更新 pages 列表做移除操作即可。

在 `_onPopPage` 中，如果我們同意關閉該頁面，
則呼叫 `route.didPop(result)`，該函式預設返回 true。

當然，我們也完全可以選擇在接收到通知時不更新 pages 列表，
這完全由我們控制，如下這段程式碼：

<!--skip-->
```dart
bool _onPopPage(Route<dynamic> route, dynamic result) {
  // setState(() => pages.remove(route.settings));
  return route.didPop(result);
}
```

那麼，此時會導致什麼現象？
`route.didPop(result)` 函式被直接觸發，
表示在底層路由棧中彈出該頁面，
這時，Flutter 就會比較
**底層已經關閉了一個頁面的路由棧** 和 **當前 Navigator 中存有的 pages**，
發現不一致，就會按照現有的 pages 將多餘的一個 Page 當做新頁面，
再產生一個 Route 物件，這樣，底層路由棧中的內容
就能隨時保持與上層 pages 資料一致了。

也就是說，**某個頁面是否能夠關閉完全由開發者掌控**，
而不是單純地交給系統的 `Navigator.pop()`。
這裡，如果我們不想關閉某個頁面，
也可以在 `onPopPage` 的回呼(Callback)函式中直接返回 false：

<!--skip-->
```dart
bool _onPopPage(Route<dynamic> route, dynamic result) {
  if (...) {
    return false;
  }
  setState(() => pages.remove(route.settings));
  return route.didPop(result);
}
```

需要注意的是，onPopPage 只響應路由棧最上層頁面的推出，
中間頁面的移除不會呼叫這個回呼(Callback)函式。

**這也合情合理**，如果我們想要移除非最上層頁面，
那麼下次彈出頁面時候，底層路由棧會直接與
新的 pages 列表比較來做出相應改變。

要執行上述完整案例，檢視 [完整程式碼](https://github.com/MeandNi/flutter_navigator_v2/blob/master/lib/pages_example.dart)。

Flutter 框架中預先內建了 `MaterialPage` 和 
`CupertinoPage` 兩種 Page，分別表示 Material 
和 Cupertino 風格下的頁面，
與之前我們常用的 `MaterialPageRoute` 
和 `CupertinoPageRoute` 相呼應，
它們都接受一個 child 元件表示該頁面所要呈現的內容。
例如下面這個例子，我們可以直接在 pages 中
使用 `MaterialPage` 建立頁面：

<!--skip-->
```dart
List<Page> pages = <Page>[
  MaterialPage(
    key: ValueKey('VeggiesListPage'),
    child: VeggiesListScreen(
      veggies: veggies,
      onTapped: _handleVeggieTapped,
    ),
  ),
  if (show404)
    MaterialPage(key: ValueKey('UnknownPage'), child: UnknownScreen())
  else if (_selectedVeggie != null)
    VeggieDetailsPage(veggie: _selectedVeggie)
];
```

我們也可以直接繼承 Page 定義自己的頁面型別，如下：

<!--skip-->
```dart
class MyPage extends Page {
  final Veggie veggie;

  MyPage({
    this.veggie,
  }) : super(key: ValueKey(veggie));

  Route createRoute(BuildContext context) {
    return MaterialPageRoute(
      settings: this,
      builder: (BuildContext context) {
        return VeggieDetailsScreen(veggie: veggie);
      },
    );
  }
}
```

這裡，我們重寫了 `createRoute()` 返回
一個 `MaterialPageRoute` 物件即可。

### Router

Router 是 Navigator 2.0 中新增的另一個非常重要的元件，
繼承自 StatefulWidget，可以管理自己的狀態。

它所管理的狀態就是應用的 **路由狀態**，
結合上節中提到的 Page 的概念，
我們就可以將其中的 pages 看做這裡的路由狀態，當
我們改變 pages 的內容或狀態時，
Router 就會將該狀態分發給子元件，
狀態改變導致子元件重建應用最新的狀態。

所以當 Navigator 作為 Router 的子元件時，
就會天然具有感知路由狀態改變的能力了，如下圖所示：

![](https://devrel.andfun.cn/devrel/posts/2020/11/957f7d75ef977.png)

當用戶點選某個按鈕就會觸發類似下面這個函式的呼叫，
該函式又會導致狀態改變而重建子元件。

<!--skip-->
```dart
void _pushPage() {
  MyRouteDelegate.of(context).push('Route$_counter');
}
```

Navigator 2.0 所強調的宣告式 API 的核心就在於此，
我們操作路由的方式並非再是 push 或者 pop，
而是改變應用的狀態了！
我們需要從觀念上理解宣告式 API 與以往的不同之處。

### Router 代理

Router 要完成上面所說的功能主要需要
透過配置 RouterDelegate（路由代理）實現。

Navigator 2.0 之後，Flutter 也提供了 
MaterialApp 的新建構函式 router 
來幫助我們直接在應用最上層
構造出全域的 Router 元件，使用方式如下：

<!--skip-->
```dart
MaterialApp.router(
  title: 'Flutter Demo',
  theme: ThemeData(
    primarySwatch: Colors.blue,
    visualDensity: VisualDensity.adaptivePlatformDensity,
  ),
  routeInformationParser: MyRouteParser(),
  routerDelegate: delegate,
)
```

該建構函式接受一個 `routerDelegate` 引數，
這裡，就可以傳入我們自己建立的 
`MyRouteDelegate` 物件，具體程式碼如下：

<!--skip-->
```dart
class MyRouteDelegate extends RouterDelegate<String>
    with PopNavigatorRouterDelegateMixin<String>, ChangeNotifier {
  final _stack = <String>[];

  static MyRouteDelegate of(BuildContext context) {
    final delegate = Router.of(context).routerDelegate;
    assert(delegate is MyRouteDelegate, 'Delegate type must match');
    return delegate as MyRouteDelegate;
  }

  MyRouteDelegate({
    @required this.onGenerateRoute,
  });

  // ...
  @override
  Widget build(BuildContext context) {
    print('${describeIdentity(this)}.stack: $_stack');
    return Navigator(
      key: navigatorKey,
      onPopPage: _onPopPage,
      pages: [
        for (final name in _stack)
            MyPage(
              key: ValueKey(name),
              name: name,
              routeFactory: onGenerateRoute,
            ),
      ],
    );
  }
}
```

上面的 MyRouteDelegate 繼承自 RouterDelegate，
內部可以實現它的 `setInitialRoutePath`、`setNewRoutePath`、
`build` 三個方法與 `currentConfiguration` 的 getter 方法，
並且也混入了 `PopNavigatorRouterDelegateMixin` 類，
它的主要作用是響應 Android 裝置的回退按鈕，
而 `ChangeNotifier` 作用便是做事件通知，
下文的 “[實現 RouterDelegate](#實現-routerdelegate)” 中
就會分析這些方法各自的作用。

這裡，我們先看 `MyRouteDelegate.build` 方法，
與上一小節一樣，我們可以透過傳入 `pages` 和 `onPopPage` 引數
建立一個 Navigator 元件返回，
這樣，當 MyRouteDelegate 元件
傳入到 `MaterialApp.router()` 建構函式後，
這裡的 Navigator 就順利成為了 Router 的子元件了。

大部分情況下，一個自訂的路由代理就可以這樣實現完成了。

### Router 事件

在應用開發中，Router 最根本的作用還是監聽各種
來自系統的路由相關事件，包括：

- 首次啟動應用程式時，系統請求的初始路由。
- 監聽來自系統的新 intent，即開啟一個新路由頁面。
- 監聽裝置回退，關閉路由棧中頂部路由。

而要想完整的響應這些事件，
還得為 Router 配置 **RouteNameProvider Delegate** 
和 **BackButtonDispatcher Delegate**。

最初，應用啟動或者開啟新頁面的事件從系統發出時，
**會轉發給應用層一個表示該事件的字串**，
RouteNameParser Delegate 會將該字串傳遞給 RouteNameParser，
進而會解析成一個類別型 `T` 的物件，
型別 `T` 預設為 `RouteSetting`，
其中就會包含傳遞的路由名稱和引數等資訊了。

類似地，使用者點選裝置回退按鈕後，
會將該事件傳遞給 BackButtonDispatcher Delegate。

最終，RouteNameParser 解析的物件資料和 
BackButtonDispatcher Delegate 回退事件都會轉發給
上文中的 RouteDelegate，RouteDelegate 接收到這些事件通知後，
就會執行響應，改變狀態，
從而導致含有 pages 的 Navigator 元件重建，
在應用層中呈現最新的路由狀態。

整個過程可以用下圖表示：

![](https://devrel.andfun.cn/devrel/posts/2020/11/516f15849e0aa.png)

{{site.alert.note}}

需要知道的是，RouteNameProvider Delegate 和 
BackButtonDispatcher Delegate 都有 Flutter 內建的預設實現，
因此，大部分情況下，我們並不需要考慮其中的細節，
此時型別 `T` 預設為 RouteSetting
（與舊的 Navogator API 一致，包含路由資訊）。

{{site.alert.end}}

從以上部分可以看出，
一系列的操作只是將最終事件傳遞給 RouterDelegate 而已，
之後狀態更新等操作都可以由我們自訂的 RouterDelegate 決定。

### 實現 RouterDelegate

正如我們上文說的，Flutter 為 RouteNameProvider Delegate 
和 BackButtonDispatcher Delegate 都提供了預設實現，
而 RouterDelegate 則必須要我們手動實現，
並傳遞給 `MaterialApp.router()` 建構函式才行。

我們可以在這裡完成各種業務相關的操作，
RouteDelegate 本身實現自 Listenable，即可監聽物件，
也可以叫做被觀察者，
每當狀態改變時，觀察者們就能通知它響應該事件，
從而促使 Navigator 元件重建，更新路由狀態。

RouterDelegate 中的路由事件的通知主要由下面幾個函式接收：

- backButtonDispatcher 發出回退按鈕事件時，
會呼叫 RouterDelegate 的 `popRoute()` 方法，
由混入的 PopNavigatorRouterDelegateMixin 實現。
- 發出應用初始路由的通知時，
會呼叫 RouterDelegate 的 `setInitialRoutePath()` 方法，
該方法接受路由名稱作為引數，
預設此方法會直接呼叫 RouterDelegate 的 `setNewRoutePath()` 函式。
- 系統透過 routeNameProvider 發出開啟新路由頁面的通知時，
直接呼叫 `setNewRoutePath()` 方法，
引數就是由 routeNameParser 解析的結果。

因此，我們最終就可以實現如下這樣的 RouterDelegate：

<!--skip-->
```dart
class MyRouteDelegate extends RouterDelegate<String>
    with PopNavigatorRouterDelegateMixin<String>, ChangeNotifier {
  final _stack = <String>[];

  static MyRouteDelegate of(BuildContext context) {
    final delegate = Router.of(context).routerDelegate;
    assert(delegate is MyRouteDelegate, 'Delegate type must match');
    return delegate as MyRouteDelegate;
  }

  MyRouteDelegate({
    @required this.onGenerateRoute,
  });

  final RouteFactory onGenerateRoute;

  @override
  GlobalKey<NavigatorState> navigatorKey = GlobalKey<NavigatorState>();

  @override
  String get currentConfiguration => _stack.isNotEmpty ? _stack.last : null;

  List<String> get stack => List.unmodifiable(_stack);

  void push(String newRoute) {
    _stack.add(newRoute);
    notifyListeners();
  }

  void pop() {
    if (_stack.isNotEmpty) {
      _stack.remove(_stack.last);
    }
    notifyListeners();
  }

  @override
  Future<void> setInitialRoutePath(String configuration) {
    return setNewRoutePath(configuration);
  }

  @override
  Future<void> setNewRoutePath(String configuration) {
    print('setNewRoutePath $configuration');
    _stack
      ..clear()
      ..add(configuration);
    return SynchronousFuture<void>(null);
  }

  bool _onPopPage(Route<dynamic> route, dynamic result) {
    if (_stack.isNotEmpty) {
      if (_stack.last == route.settings.name) {
        _stack.remove(route.settings.name);
        notifyListeners();
      }
    }
    return route.didPop(result);
  }

  @override
  Widget build(BuildContext context) {
    print('${describeIdentity(this)}.stack: $_stack');
    return Navigator(
      key: navigatorKey,
      onPopPage: _onPopPage,
      pages: [
        for (final name in _stack)
            MyPage(
              key: ValueKey(name),
              name: name,
            ),
      ],
    );
  }
}
```

這裡的 `_stack` 表示一個數據集，
每個資料會在 build 函式中創建出一個 MyPage，預設為空。
應用啟動時，會先呼叫這裡的 
`setInitialRoutePath(String configuration)` 方法，
引數為 `/`，此時路由棧就會存在一個首頁了。

完整程式碼，請 [參考這裡](https://github.com/MeandNi/flutter_navigator_v2/blob/master/lib/router_example.dart)。

在子元件中，我們也可以使用 MyRouteDelegate，
透過如下方式開啟或者關閉一個頁面：

<!--skip-->
```dart
MyRouteDelegate.of(context).push('Route$_counter');

MyRouteDelegate.of(context).pop();
```

與 InheritWidget 的性質相同，
這裡會觸發 MyRouteDelegate 中，
我們自訂的 `push()` 和 `pop()` 方法操作宣告的路由棧，
最終通知 Navigator 更新路由狀態。

### 實現 RouteInformationParser

`MaterialApp.router` 除了需要接受
路由代理 `routerDelegate` 這個必要引數外，
還需要同時指定 `routeInformationParser` 引數，如下：

<!--skip-->
```dart
MaterialApp.router(
  title: 'Flutter Demo',
  routeInformationParser: MyRouteParser(), 	// 傳入 MyRouteParser
  routerDelegate: delegate,
)
```

該引數接受一個 RouteInformationParser 物件，
定義該類通常有一個最簡單直接的實現，如下：

<!--skip-->
```dart
class MyRouteParser extends RouteInformationParser<String> {
  @override
  Future<String> parseRouteInformation(RouteInformation routeInformation) {
    return SynchronousFuture(routeInformation.location);
  }

  @override
  RouteInformation restoreRouteInformation(String configuration) {
    return RouteInformation(location: configuration);
  }
}
```

MyRouteParser 繼承自 RouteInformationParser，
並重寫了父類 `parseRouteInformation()` 
和 `restoreRouteInformation()` 兩個方法。

如上文所述，`parseRouteInformation()` 方法的作用就是
接受系統傳遞給我們的路由資訊 routeInformation，
然後，返回轉發給我們之前定義的路由代理 RouterDelegate，
解析後的型別為 RouteInformationParser 的泛型型別，
即這裡的 String。也就是說，
下面這個 RouterDelegate 中 `setNewRoutePath()` 方法的
引數 `configuration` 就是從那裡轉發而來的：

<!--skip-->
```dart
@override
Future<void> setNewRoutePath(String configuration) {
  print('setNewRoutePath $configuration');
  _stack
    ..clear()
    ..add(configuration);
  return SynchronousFuture<void>(null);
}
```

`restoreRouteInformation()` 方法返回一個 
RouteInformation 物件，表示從傳入的 `configuration` 恢復路由資訊。
與 `parseRouteInformation()` 相呼應。

例如，在瀏覽器中，Flutter 應用所在的標籤被關閉，
此時如果我們想要恢復整個頁面的路由棧則需要重寫此方法，

上面 MyRouteParser 的實現，是最簡單的實現方式，
功能就是在 `parseRouteInformation()` 中接受底層的 `routeInformation`，
在 `restoreRouteInformation()` 中恢復上層的 `configuration`。

我們也可以繼續為這兩個方法賦能，
實現更符合業務需求的邏輯，如下這程式碼：

<!--skip-->
```dart
import 'package:flutter/material.dart';
import 'package:flutter_navigator_v2/navigator_v2/model.dart';

class VeggieRouteInformationParser extends RouteInformationParser<VeggieRoutePath> {
  @override
  Future<VeggieRoutePath> parseRouteInformation(
      RouteInformation routeInformation) async {
    print("parseRouteInformation");
    final uri = Uri.parse(routeInformation.location);
    // Handle '/'
    if (uri.pathSegments.length == 0) {
      return VeggieRoutePath.home();
    }

    // Handle '/veggie/:id'
    if (uri.pathSegments.length == 2) {
      if (uri.pathSegments[0] != 'veggie') return VeggieRoutePath.unknown();
      var remaining = uri.pathSegments[1];
      var id = int.tryParse(remaining);
      if (id == null) return VeggieRoutePath.unknown();
      return VeggieRoutePath.details(id);
    }

    // Handle unknown routes
    return VeggieRoutePath.unknown();
  }

  @override
  RouteInformation restoreRouteInformation(VeggieRoutePath path) {
    print("restoreRouteInformation");
    if (path.isUnknown) {
      return RouteInformation(location: '/404');
    }
    if (path.isHomePage) {
      return RouteInformation(location: '/');
    }
    if (path.isDetailsPage) {
      return RouteInformation(location: '/veggie/${path.id}');
    }
    return null;
  }
}
```

這裡的 VeggieRouteInformationParser 
繼承的 RouteInformationParser 泛型型別被指定為了
我們自訂的 VeggieRoutePath，
在 Navigator 2.0 中我們稱這個解析後的形式為 **路由 Model**。

此時 VeggieRouteInformationParser 作用就凸顯出來了，
它在 `parseRouteInformation()` 方法中
接受到系統傳遞過來的 RouteInformation 資訊後就可以將其轉換成
我們上層熟悉的 VeggieRoutePath Model 物件。
VeggieRoutePath 類內容如下：

<!--skip-->
```dart
class VeggieRoutePath {
  final int id;
  final bool isUnknown;

  VeggieRoutePath.home()
      : id = null,
        isUnknown = false;

  VeggieRoutePath.details(this.id) : isUnknown = false;

  VeggieRoutePath.unknown()
      : id = null,
        isUnknown = true;

  bool get isHomePage => id == null;

  bool get isDetailsPage => id != null;
}
```

此時，在 `RouterDelegate<VeggieRoutePath>` 中，
我們就可以根據該物件做路由狀態的更新了。

## 最佳實踐

Navigator 2.0 與以往不同的方面主要體現在，
將路由狀態轉換成了應用本身的狀態，
給了開發者更大的自由與想象空間，
此後，我們可以將路由邏輯及其狀態的管理
與我們的業務邏輯緊密相連，
形成自己的一套方案，
相信這又會是以後 Flutter 體系中一塊大主題。

上述提及的所有程式碼包含三個案例，分別是：

- [pages_example.dart](https://github.com/MeandNi/flutter_navigator_v2/blob/master/lib/pages_example.dart)，Navigator + Page 實現路由狀態管理。
- [router_example.dart](https://github.com/MeandNi/flutter_navigator_v2/blob/master/lib/router_example.dart)，Router + Navigator + Page 實現路由狀態的統一管理
- [水果列表最佳實踐](https://github.com/MeandNi/flutter_navigator_v2/blob/master/lib/)，相對完整的一個案例，包含自訂 RouteInformationParser Model 和路由狀態管理操作。

[範例完整原始碼地址](https://github.com/MeandNi/flutter_navigator_v2)。

## 寫在最後

感謝 [@Vadaski](https://github.com/Vadaski)、[@Alex Li](https://github.com/AlexV525) 對本文的 Review。

如果您對本文還有任何疑問或者文章的建議，
歡迎向我的 Github 中的範例儲存庫提交 issue 
或者透過郵箱與我聯絡，我會及時回覆。

**本文作者：** 楊加康

《Flutter 開發之旅從南到北》作者，現就職於小米瀏覽器團隊。

聯絡方式：yangjiakay@gmail.com
