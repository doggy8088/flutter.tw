---
title: 靈動的介面卡模式
toc: true
---

文/ 楊加康，CFUG 社群成員，《Flutter 開發之旅從南到北》作者，小米工程師

設計模式系列的前兩篇，分別向大家介紹了一種
[建立性型模式（單例模式）]({{site.main_url}}/community/tutorials/singleton-pattern-in-flutter-n-dart)
和一種 [行為型設計模式（觀察者模式）]({{site.main_url}}/community/tutorials/observer-pattern-in-flutter-n-dart)，
今天我們再來介紹一種結構型設計模式 —— 介面卡模式。

**介面卡模式** (Adapter Design Pattern)，顧名思義，這個模式就是用來做適配的，像一個「粘合劑」一樣。

> 介面卡模式可以將不相容的介面轉換為可相容的介面，讓原本由於介面不相容而不能一起工作的類黏合在一起，最終使他們可以一起工作。

和 [觀察者模式]({{site.url}}/community/tutorials/observer-pattern-in-flutter-n-dart) 中的觀察者與被觀察者類似，介面卡模式中擔任主要角色是 **介面卡 (Adapter)** 和 **被適配者 (Adaptee)**。一個比較典型的例子是，插座轉接頭可以被認為是一種介面卡，可以把本身不相容的介面，透過轉接變得可以一起工作。

![介面卡模式示意圖，圖源網路](https://files.flutter-io.cn/posts/community/tutorial/images/2021-09-05-002.jpeg)

在程式碼世界中，也有很多介面不適配的場景，如我們引入了一個第三方庫後，發現它其中的類實現與我們現有程式碼並不相容，需要一個 Adapter 類做一層轉換才行。另外，相較於直接接觸原始的程式碼實現，這種模式下，客戶端僅僅依賴介面卡類，對於程式碼複用和維護性也多了一層保障。

## 類介面卡與物件介面卡

![介面卡模式 UML 圖](https://files.flutter-io.cn/posts/community/tutorial/images/2021-09-05-1_2oBi8WnJT31i2E-KaW0rhw.png)

介面卡模式有兩種實現方式：**類介面卡** 和 **物件介面卡**。其中，類介面卡使用繼承關係來實現，而物件介面卡使用組合關係來實現。具體的程式碼實現如下所示。

```dart
/// 被適配者
class Adaptee {
  String concreteOperator() {
    return 'Adaptee';
  }
}

abstract class ITarget {
  String operator();
}

/// 物件介面卡
class ObjectAdapter implements ITarget {
  var adaptee = Adaptee();

  String operator() {
    return adaptee.concreteOperator();
  }
}

/// 類介面卡
class ClassAdapter extends Adaptee {
  String operator() {
    return super.concreteOperator();
  }
}
```

`ITarget` 表示要轉化成的介面，是一個規範化的介面定義。
Dart 本身不支援關鍵詞 `interface`，因此我們可以建立一個沒有預設實現的抽象類別代替。

需要被適配的 `Adaptee` 表示一組不相容 `ITarget` 介面定義的類或介面，`ObjectAdapter` 和 `ClassAdapter` 兩種介面卡分別用不同的方式將 `Adaptee` 轉化成了符合 `ITarget` 介面定義的介面。而在客戶端使用時只需要依賴 `ITarget` 即可完成對 `Adaptee` 的適配。

```dart
class Client {
  Client(this.adapter);

  final ITarget adapter;

  operator() {
    var result = adapter.operator();
    assert(result == 'Adaptee');
  }
}
```

關於類介面卡與物件介面卡：

- 如果希望你一個介面卡可以同時適配多個不同的類，則單繼承機制的 Dart 語言無法使用 **類介面卡** 實現這種一對多的介面卡。
- 如果 `Adaptee` 介面很多，而且 `Adaptee` 和 `ITarget` 介面定義大部分都相同，那我們推薦使用類介面卡，因為 可以充分將繼承的程式碼複用作用利用起來。
- 大部分場景下，我們推薦使用 **物件介面卡** 的方式實現介面卡模式，因為 **繼承** 在很多情況下容易被 **濫用** 並造成 **層級過多** 的現象，而 **組合** 更加靈活。

## 實現

在程式碼應用中，介面卡模式典型的例子是 Android 中的 ListView，在 Android 中，ListView 作為一個展示列表的 UI 元件，它的主要作用是將使用者交給它的 Item View 以列表形式展示出來，然而描述 View 的形式卻多種多樣，可以是 Android 中 XML 佈局，也可以是以 Java 程式碼中自訂 View 的形式提供，甚至可以是自訂的一套規則，實現自己的 UI 描述語言。

本身，XML 或者其他描述語言對於 ListView 是不可知的，所以，在 ListView 和它們之前介入一個介面卡就可以有效的解決這個問題，介面卡的作用就是將這些形式轉換成 Item View 以適應 Listview。

在 Flutter 中，這種形式的模式很容易實現，例如，我們想自訂一個可以展示蔬果列表的元件 `VeggieList`：

```dart
class VeggieList extends StatefulWidget {
  @override
  _VeggieListState createState() => _VeggieListState();
}

class _VeggieListState extends State<VeggieList> {

  final List<Veggie> veggies = [];

  @override
  Widget build(BuildContext context) {
    return veggies.isEmpty
        ? Text('無水果')
        : Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: <Widget>[
        for (var veggie in veggies)
          ListTile(title: Text('${veggie.name}')))
      ],
    );
  }
}
```

這個元件主要關心的是 `veggies`，一個提供一組 `Veggie` 物件的陣列。

`veggies` 資料源並不統一，可能來自多個不同的介面，可能在雲端，也可能是本地的假資料，並且不同的介面提供的資料格式也可能不相同，可能是 xml 或者是 json。

```dart
/// 返回 json 資料格式的介面
class JsonVeggiesApi {
  final String _veggiesJson = '''
  {
    "veggies": [
      {
        "name": "apple (JSON)",
      },
      {
        "name": "banana (JSON)",
      },
    ]
  }
  ''';

  String getVeggiesJson() {
    return _veggiesJson;
  }
}

/// 返回 xml 資料格式的介面
class XmlVeggiesApi {
  final String _contactsXml = '''
  <?xml version="1.0"?>
  <veggies>
    <veggie>
      <name>apple (XML)</name>
    </veggie>
    <veggie>
      <name>banana (XML)</name>
    </veggie>
  </veggies>
  ''';

  String getVeggiesXml() {
    return _contactsXml;
  }
}
```

這些介面顯然不能直接應用在 `VeggieList` 中展示，因此，需要做一些適配工作，適配的目的就是將這些資料轉換成 `Veggie` 物件的陣列，因此我們可以定義如下這個介面：

```dart
abstract class IVeggiesAdapter {
  List<Veggie> getVeggies();
}
```

其中的 `getVeggies` 方法返回的就是 `VeggieList` 元件需要的 `Veggie` 物件陣列。

建立介面卡時，只需要實現這個介面，然後組合目標需要被適配的類做介面轉換即可，例如下面的 `JsonnVeggiesAdapter`，專門負責將 `JsonVeggiesApi` 轉換為相容 `VeggieList` 的介面卡：

```dart
class JsonnVeggiesAdapter implements IVeggiesAdapter {
  final JsonVeggiesApi _api = JsonVeggiesApi();

  @override
  List<Veggie> getVeggies() {
    final veggiesJson = _api.getVeggiesJson();
    final veggiesList = _parseContactsJson(veggiesJson);

    return veggiesList;
  }

  List<Veggie> _parseContactsJson(String contactsJson) {
    final contactsMap = json.decode(contactsJson) as Map<String, dynamic>;
    final contactsJsonList = contactsMap['contacts'] as List;
    final contactsList = contactsJsonList.map((json) {
      final contactJson = json as Map<String, dynamic>;

      return Veggie(
        name: contactJson['name'] as String,
      );
    }).toList();

    return contactsList;
  }
}
```

最終，在使用到 `VeggieList` 時，注入 `JsonnVeggiesAdapter` 這個介面卡就可以將原本不相容的 `JsonVeggiesApi` 中的資料展示出來了：

```dart
class AdapterExample extends StatelessWidget {
  const AdapterExample();

  @override
  Widget build(BuildContext context) {
    return ScrollConfiguration(
      behavior: const ScrollBehavior(),
      child: SingleChildScrollView(
        child: VeggieList(
          adapter: JsonnVeggiesAdapter(),
        ),
      ),
    );
  }
}


/// 最終的 VeggieList
class VeggieList extends StatefulWidget {
  final IVeggiesAdapter adapter;

  const VeggieList({
    @required this.adapter,
  });

  @override
  _VeggieListState createState() => _VeggieListState();
}

class _VeggieListState extends State<VeggieList> {

  final List<Veggie> veggies = [];

  void _getVeggies() {
    setState(() {
      veggies.addAll(widget.adapter.getVeggies());
    });
  }

  @override
  Widget build(BuildContext context) {
    return veggies.isEmpty
        ? Text('無水果',)
        : Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: <Widget>[
        for (var veggie in veggies)
          ListTile(title: Text('${veggie.name}')))
      ],
    );
  }
}
```

同理，不同介面來源的資料都可以透過介面卡實現 `IVeggiesAdapter` 介面與 `VeggieList` 做相容。

## Flutter

在應用中，我們經常會使用到 **`CustomScrollView`** 建立擁有自訂滾動效果的元件，而 `CustomScrollView` 只允許包含 `sliver` 系列元件 (`SliverAppBar`、`SliverList`、`SliverPersistentHeader` 等) ，如果想包含普通的元件，必然需要使用 `SliverToBoxAdapter`：

```dart

return MaterialApp(
  home: CustomScrollView(
    controller: scrollController,
    slivers: <Widget>[
      SliverAppBar(),
      SliverToBoxAdapter(
        child: Container(
          height: 100.0,
        ),
      ),
    ],
  ),
);
```

這裡，將 `Container` 放入 `SliverToBoxAdapter` 中便可以在 `CustomScrollView` 展示出來了。

我們認為普通的 widget 是不相容 `CustomScrollView` 的，`SliverToBoxAdapter` 在其中就扮演了介面卡的角色。它使用 **類介面卡** 的方式，將 `SingleChildRenderObjectWidget` 中 `createRenderObject` 介面重寫轉換成可以包含 `RenderBox` (對應一般 widget 的 `RenderObject`) 的 `RenderSliver` (對應 sliver 系列 widget 的 `RenderObject`)，即這裡的 `RenderSliverToBoxAdapter`：

```dart
class SliverToBoxAdapter extends SingleChildRenderObjectWidget {
  /// Creates a sliver that contains a single box widget.
  const SliverToBoxAdapter({
    Key? key,
    Widget? child,
  }) : super(key: key, child: child);

  @override
  RenderSliverToBoxAdapter createRenderObject(BuildContext context) => RenderSliverToBoxAdapter();
}
```

## 拓展閱讀

- 介面卡模式：https://refactoringguru.cn/design-patterns/adapter
- 組合優於繼承：https://time.geekbang.org/column/article/169593
- Flutter Sliver：https://juejin.cn/post/6844903901720739848

## 關於本系列文章

Flutter / Dart 設計模式從南到北 (簡稱 Flutter 設計模式) 系列內容由 CFUG 社群成員、《Flutter 開發之旅從南到北》作者、小米工程師楊加康撰寫併發布在 Flutter 社群公眾號和 flutter.cn 網站的社群課程節目。

本系列內容旨在推進 Flutter / Dart 語言特性的普及，幫助開發者更高效地開發出高品質、可維護的 Flutter 應用。如果你對本文還有任何疑問或者文章的建議，歡迎向中文社群官方 GitHub 儲存庫 (cfug/flutter.cn) 提交 Issue 或者直接與我聯絡 (yangjiakay@gmail.com)。
