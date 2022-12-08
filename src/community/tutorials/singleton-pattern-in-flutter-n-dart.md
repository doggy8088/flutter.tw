---
title: Flutter(able) 的單例模式
toc: true
---

文/ 楊加康，CFUG 社群成員，《Flutter 開發之旅從南到北》作者，小米工程師

**單例設計模式**（Singleton Design Pattern）理解起來非常簡單。

> 一個類別只允許建立一個例項，那這個類就是一個單例類，這種設計模式就叫作單例設計模式，簡稱單例模式。

作為最簡單的一種設計模式之一，對於單例本身的概念，大家一看就能明白，但在某些情況下也很容易使用不恰當。相比其他語言，Dart 和 Flutter 中的單例模式也不盡相同，本篇文章我們就一起探究看看它在 Dart 和 Flutter 中的應用。

![](https://files.flutter-io.cn/posts/community/tutorial/images/2021-07-29-%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F.png)

## Flutter(able) 的單例模式

一般來說，要在程式碼中使用單例模式，結構上會有下面這些約定俗成的要求：

- 單例類（Singleton）中包含一個參考自身類別的靜態屬性例項（instance），且能自行建立這個例項。
- 該例項只能透過靜態方法 `getInstance()` 存取。
- 類建構函式通常沒有引數，且被標記為私有，確保不能從類外部例項化該類別。

![單例設計模式 UML 圖，圖源：https://www.uml-diagrams.org/class-reference.html](https://files.flutter-io.cn/posts/community/tutorial/images/2021-07-29-1_CqdIf_w0sOciElKyfam3fQ.png)

遵循以上這些要求，我們就不難能用 Dart 寫出一個普通的單例模式：

```dart
class Singleton {
  static Singleton _instance;
  
  // 私有的命名建構函式
  Singleton._internal();
  
  static Singleton getInstance() {
    if (_instance == null) {
      _instance = Singleton._internal();
    }
    
    return _instance;
  }
}
```

同時，在實現單例模式時，也需要考慮如下幾點，以防在使用過程中出現問題：

- 是否需要延遲載入，即類例項只在第一次需要時建立。
- 是否執行緒安全，在 Java、C++ 等多執行緒語言中需要考慮到多執行緒的併發問題。由於 Dart 是單執行緒模型的語言，所有的程式碼通常都執行在同一個 isolate 中，因此不需要考慮執行緒安全的問題。
- 在某些情況下，單例模式會被認為是一種 **反模式**，因為它違反了 SOLID 原則中的單一責任原則，單例類自己控制了自己的建立和生命週期，且單例模式一般沒有介面，擴充困難。
- 單例模式的使用會影響到程式碼的可測試性。如果單例類依賴比較重的外部資源，比如 DB，我們在寫單元測試的時候，希望能透過 mock 的方式將它替換掉。而單例類這種硬編碼式的使用方式，導致無法實現 mock 替換。

在實際編碼過程中，單例模式常見應用有：

- 全域日誌的 Logger 類、應用全域的配置資料物件類，單業務管理類別。
- 建立例項時佔用資源較多，或例項化耗時較長的類別。
- 等等...

## Dart 化

如上文所說的，Dart 語言作為單執行緒模型的語言，實現單例模式時，我們本身已經可以不用再去考慮 **執行緒安全** 的問題了。Dart 的很多其他特性也依然可以幫助到我們實現更加 Dart 化的單例。

使用 **getter** 運運算元，可以打破單例模式中既定的，一定要寫一個 `getInstance()` 靜態方法的規則，簡化我們必須要寫的模版化程式碼，如下的 `get instance`:

```dart
class Singleton {
  static Singleton _instance;
  static get instance {
    if (_instance == null) {
      _instance = Singleton._internal();
    }
    
    return _instance;
  }
  
  Singleton._internal();
}
```

Dart 的 getter 的使用方式與普通方法大致相同，只是呼叫者不再需要使用括號，這樣，我們在使用時就可以直接使用如下方式拿到這個單例物件：

```dart
final singleton = Singleton.instance;
```

而 Dart 中特有的 **工廠建構函式**（factory constructor）也原生具備了 **不必每次都去建立新的類例項** 的特性，將這個特性利用起來，我們就可以寫出更優雅的 Dart(able) 單例模式了，如下：

```dart
class Singleton {
  static Singleton _instance;
  
  Singleton._internal();
  
  // 工廠建構函式
  factory Singleton() {
    if (_instance == null) {
      _instance = Singleton._internal();
    }
    
    return _instance;
  }
}
```

這裡我們不再使用 **getter** 運運算元額外提供一個函式，而是將單例物件的產生交給工廠建構函式，此時，工廠建構函式僅在第一次需要時建立 `_instance`，並之後每次返回相同的例項。這時，我們就可以像下面這樣使用普通建構函式的方式獲取到單例了：

```dart
final singleton = Singleton();
```

如果你還掌握了 Dart 空安全及箭頭函式等特性，那麼還可以使用另一種方式進一步精簡程式碼，寫出像下面這樣 Dart 風味十足的程式碼：

```dart
class Singleton {
  static Singleton _instance;

  Singleton._internal() {
    _instance = this;
  }

  factory Singleton() => _instance ?? Singleton._internal();
}
```

這裡，使用 `??` 作為 `_instance` 例項的判空運運算元，如果為空則呼叫建構函式例項化否則直接返回，也可以達到單例的效果。

以上，Dart 單例中延遲載入的無不是使用判空來實現的（`if (_instance == null)` 或 `??`），但是在 Dart 空安全特性裡還有一個非常重要的運運算元 `late ` ，它在語言層面就實現了例項的延遲載入，如下面這個例子：

```dart
class Singleton {
  Singleton._internal();
  
  factory Singleton() => _instance;
  
  static late final Singleton _instance = Singleton._internal();
}
```

被標記為 `late ` 的變數 `_instance` 的初始化操作將會延遲到欄位首次被存取時執行，而不是在類載入時就初始化。這樣，Dart 語言特有的單例模式的實現方式就這麼產生了。

## Flutter 化

說到工廠建構函式/空安全運運算元等 Dart 語法上的特性，Flutter 應用中的例子已經屢見不鮮了， 但光看單例模式的定義，我們還必須聯想到 Flutter 中另一個非常重要的 widget，那就是 InheritedWidget。

如果你已經是一個 Flutter 小能手，或者已經看過《Flutter 開發之旅從南到北》和之前的文章的話，一定已經對他的作用有了清晰的認識了。

InheritedWidget 狀態可遺傳的特性可以幫助我們很方便的實現父子元件之間的資料傳遞，同時，它也可以作為狀態管理中的 **資料儲存庫**，作為整個應用的資料狀態統一儲存的地方。

![圖源《Flutter 開發之旅從南到北》—— 第九章 圖 9.4](https://files.flutter-io.cn/posts/community/tutorial/images/2021-07-29-%E6%95%B0%E6%8D%AE%E4%BB%93%E5%BA%93-3243985.svg)

上面程式碼中，我們透過繼承 InheritedWidget 就實現了自己的可遺傳元件 `_InheritedStateContainer`，其中的 `data` 變量表示全域狀態資料，**在這裡就可以被認為是整個應用的一個單例物件**。

`_InheritedStateContainer` 還接受 `child` 引數作為它的子元件，`child` 表示的所以子元件們就都能夠以某種方式得到 `data` 這個單一的全域資料了。

約定俗成地，Flutter 原始碼經常會提供一些 `of` 方法（類比 `getInstance()`）作為幫助我們拿到全域資料的輔助函式。

以 Flutter 中典型的 Theme 物件為例。我們通常會在應用的根元件 `MaterialApp` 中建立 `ThemeData` 物件作為應用統一的主題樣式物件：

```dart
MaterialApp(
  title: 'Flutter Demo',
  theme: ThemeData(
    primarySwatch: Colors.blue,
    visualDensity: VisualDensity.adaptivePlatformDensity,
  ),
  home: MyHomePage(title: 'Flutter Demo Home Page'),
);
```

在其他任意的元件中，我們可以使用 `Theme.of(context)` 拿到該物件了，且這個物件全域唯一。如下所示，我們可以將該 `ThemeData` 物件中的 `primaryColor` 應用在 `Text` 中：

```dart
// 使用全域文字樣式
Text(
  'Flutter',
  style: TextStyle(color: Theme.of(context).primaryColor),
)
```

這個角度來看，InheritedWidget 完全可以被我們看作是最原生、最 Flutter 的單例應用了。

## 本文小結

本篇文章，我們經歷了從實現普通單例到應用 **getter 運運算元** 的 Dart 單例，到使用 **工廠建構函式** Dart 單例，再到使用了 **工廠建構函式 + 空安全語法 + 箭頭函式** 的 Dart 單例，最後結合對 InheritedWidget 概念的理解，看到了 Flutter 中特有的單例模式，算是每一步都走了一遍。但學習設計模式的重點還是在於實際應用，希望大家今後在實際工程中能將這些概念用起來，如果你想更進一步理解 Dart 中的單例模式，可以參閱「**拓展閱讀**」學習更多，希望對你有幫助。

![單例模式從南到北](https://files.flutter-io.cn/posts/community/tutorial/images/2021-07-29-%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F.svg)

## 拓展閱讀

- 圖書 [《Flutter 開發之旅從南到北》](https://item.jd.com/12757223.html)—— 第 2 章、第 9 章
- [單例模式](https://c.biancheng.net/view/1338.html)
- [Dart 空安全](https://dart.cn/null-safety)
- [延遲初始化](https://dart.cn/null-safety/understanding-null-safety#lazy-initialization)

## 關於本系列文章

Flutter / Dart 設計模式從南到北（簡稱 Flutter 設計模式）系列內容預計兩週釋出一篇，著重向開發者介紹 Flutter 應用開發中常見的設計模式以及開發方式，旨在推進 Flutter / Dart 語言特性的普及，以及幫助開發者更高效地開發出高品質、可維護的 Flutter 應用。
