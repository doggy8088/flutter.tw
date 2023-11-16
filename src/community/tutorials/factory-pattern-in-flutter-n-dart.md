---
title: 工廠模式家族
toc: true
---

文/ 楊加康，CFUG 社群成員，《Flutter 開發之旅從南到北》作者，小米工程師

在圍繞設計模式的話題中，工廠這個詞頻繁出現，從 **簡單工廠** 模式到 **工廠方法** 模式，再到 **抽象工廠** 模式。工廠名稱含義是製造產品的工業場所，應用在面向物件中，順理成章的成為了比較典型的建立型模式。

![圖源：https://media2.giphy.com/media/3ohjUKYWSqORcgIIsE/giphy.gif](https://files.flutter-io.cn/posts/community/tutorial/images/2022-02-20-1_X-eyz2eZZDho_bFBGBOWEA.gif)

> 從形式上講，工廠可以是一個返回我們想要物件的一個方法/函式，即可以作為建構函式的一種抽象。

本文，就帶大家使用 Dart 理解它們的各自的實現，以及它們之間的關係。

### 簡單工廠 & factory 關鍵字

**簡單工廠模式** 不在 23 種 GoF 設計模式中，卻是我們最常使用的一種程式設計方式。
其中主要涉及到一個特殊的方法，專門用來提供我們想要的例項物件（物件工廠），
我們可以將這個方法放到一個單獨的類 `SimpleFactory` 中，如下：

```dart
class SimpleFactory {

  /// 工廠方法
  static Product createProduct(int type) {
    if (type == 1) {
      return ConcreteProduct1();
    }
    if (type == 2) {
      return ConcreteProduct2();
    }
    return ConcreteProduct();
  }
}
```

我們認為該方法要建立的物件同屬一個 **Product** 類（抽象類別），並透過引數 type 指定要建立具體的物件型別。
Dart 不支援 `interface` 關鍵詞，但我們可以使用 `abstract` 以抽象類別的方式定義介面，
然後各個具體的型別繼承實現它即可：

```dart
/// 抽象類別
abstract class Product {
  String? name;
}

/// 實現類
class ConcreteProduct implements Product {
  @override
  String? name = 'ConcreteProduct';
}

/// 實現類1
class ConcreteProduct1 implements Product {
  @override
  String? name = 'ConcreteProduct1';
}

/// 實現類2
class ConcreteProduct2 implements Product {
  @override
  String? name = 'ConcreteProduct2';
}
```

當我們想要在程式碼中獲取對應的型別物件時，只需要透過這個方法傳入想要的型別值即可，
我們不必關心生產如何被生產以及哪個物件被選擇的具體邏輯：

```dart
void main() {
  final Product product = SimpleFactory.createProduct(1);
  print(product.name); // ConcreteProduct1
}
```

這就是 **簡單工廠模式**。
說到這裡，就不得不提到 Dart 中特有的 **factory** 關鍵詞了。

**factory 關鍵詞** 可以用來修飾 Dart 類別的建構函式，意為 **工廠建構函式**，它能夠讓 **類** 的建構函式天然具有工廠的功能，使用方式如下：

```dart
class Product {
  /// 工廠建構函式（修飾 create 建構函式）
  factory Product.createFactory(int type) {
    if (type == 1) {
      return Product.product1;
    } else if (type == 2) {
      return Product._concrete2();
    }
    return Product._concrete();
  }

  /// 命名建構函式
  Product._concrete() : name = 'concrete';

  /// 命名建構函式1
  Product._concrete1() : name = 'concrete1';

  /// 命名建構函式2
  Product._concrete2() : name = 'concrete2';

  String name;
}
```

**factory** 修飾的建構函式需要返回一個當前類別的物件例項，
我們可以根據引數呼叫對應的建構函式，返回對應的物件例項。

```dart
void main() {
  Product product = Product.createFactory(1);
  print(product.name); // concrete1
}
```

此外，工廠建構函式也並不要求我們每次都必須產生新的物件，
我們也可以在類中預先定義一些物件供工廠建構函式使用，
這樣每次在使用同樣的引數建構物件時，返回的會是同一個物件，
在 [單例模式](https://flutter.tw/community/tutorials/singleton-pattern-in-flutter-n-dart) 的章節中我們已經介紹過：

```dart
class Product {
  /// 工廠建構函式
  factory Product.create(int type) {
    if (type == 1) {
      return product1;
    } else if (type == 2) {
      return product2();
    }
    return Product._concrete();
  }

  static final Product product1 = Product._concrete1();
  static final Product product2 = Product._concrete2();
}
```

**factory** 除了可以修飾命名建構函式外，也可以修飾預設的非命名建構函式，

```dart
class Product {
  factory Product(int type) {
    return Product._concrete(); 
  }

  String? name;
}
```

到這裡為止，工廠建構函式的一個缺點已經凸顯了出來，即使用者並不能直觀的感覺到自己正在使用的是工廠函式。
工廠建構函式的使用方法和普通建構函式沒有區別，但這個建構函式生產的例項相當於是一種單例：

```dart
void main() {
  Product product = Product(1);
  print(product.name); // concrete1
}
```

這樣的用法很容易造成使用者的困擾，因此，我們應當儘量使用特定的
**命名建構函式** 作為工廠建構函式（如上面範例中的 `createFactory`）。

### 工廠方法模式

工廠方法模式同樣也是我們程式設計中最常用到的一種手段。

![抽象工廠 UML，圖源：refactoring.guru](https://files.flutter-io.cn/posts/community/tutorial/images/2022-02-20-2022-02-20-1_yyGj6x9PNJLYiq4miG3mww.png)

在簡單工廠中，它主要服務的物件是客戶，而 **工廠方法** 的使用者與工廠本身的類並不相干，
而工廠方法模式主要服務自身的父類，如下的 `ProductFactory`（類比 UML 中的 Creator）：

```dart
/// 抽象工廠
abstract class ProductFactory {
  /// 抽象工廠方法
  Product factoryMethod();

  /// 業務程式碼
  void dosomthing() {
    Product product = factoryMethod();
    print(product.name);
  }
}
```

在 `ProductFactory` 類中，工廠方法 `factoryMethod` 是抽象方法，
每個子類別都必須重寫這個方法並返回對應不同的 `Product` 物件，
在 `dosomthing()` 方法被呼叫時，就可以根據返回的物件做出不同的響應。
具體使用方法如下：

```dart
/// 具體工廠
class ProductFactory1 extends ProductFactory {
  
  /// 具體工廠方法1
  @override
  Product factoryMethod() {
    return ConcreteProduct1();
  }
}

class ProductFactory2 extends ProductFactory {
  /// 具體工廠方法2
  @override
  Product factoryMethod() {
    return ConcreteProduct2();
  }
}

/// 使用
main() {
  ProductFactory product = ProductFactory1();
  product.dosomthing();	// ConcreteProduct1
}
```

在 Flutter 中，抽象方法有一個非常實用的應用場景。我們在使用 Flutter 開發多端應用時通常需要考慮到多平臺的適配，即在多個平臺中，同樣的操作有時會產生不同的結果/樣式，我們可以將這些不同結果/樣式產生的邏輯放在工廠方法中。

如下，我們定義一個 `DialogFacory`，用作產生不同樣式 Dialog 的工廠：

```dart
abstract class DialogFacory {
  Widget createDialog(BuildContext context);

  Future<void> show(BuildContext context) async {
    final dialog = createDialog(context);
    return showDialog<void>(
      context: context,
      builder: (_) {
        return dialog;
      },
    );
  }
}
```

然後，針對 Android 和 iOS 兩個平台，就可以建立兩個不同樣式的 Dialog 了：

```dart
/// Android 平台
class AndroidAlertDialog extends DialogFactory {

  @override
  Widget createDialog(BuildContext context) {
    return AlertDialog(
      title: Text(getTitle()),
      content: const Text('This is the material-style alert dialog!'),
      actions: <Widget>[
        TextButton(
          onPressed: () {
            Navigator.of(context).pop();
          },
          child: const Text('Close'),
        ),
      ],
    );
  }
}
/// iOS 平台
class IOSAlertDialog extends DialogFactory {
  
  @override
  Widget createDialog(BuildContext context) {
    return CupertinoAlertDialog(
      title: Text(getTitle()),
      content: const Text('This is the cupertino-style alert dialog!'),
      actions: <Widget>[
        CupertinoButton(
          onPressed: () {
            Navigator.of(context).pop();
          },
          child: const Text('Close'),
        ),
      ],
    );
  }
}
```

現在，我們就可以像這樣使用對應的 Dialog 了：

```dart
Future _showCustomDialog(BuildContext context) async {
  final dialog = AndroidAlertDialog();
  // final dialog = IOSAlertDialog();
  await selectedDialog.show(context);
}
```

### 抽象工廠

抽象工廠模式，相較於 **簡單工廠** 和 **工廠方法** 最大的不同是：這兩種模式只生產一種物件，而抽象工廠**生產的是一系列物件**（物件族），而且產生的這一系列物件一定存在某種聯絡。比如 Apple 會生產 **手機**、**平板** 等多個產品，這些產品都屬於 Apple 這個品牌。

如下面這個抽象的工廠類：

```dart
abstract class ElectronicProductFactory {
  Product createComputer();
  
  Product createMobile();

  Product createPad();
  
  // ...
}
```

對於 Apple 來說，我就是生產這類電子產品的工廠，於是可以繼承這個類，實現其中的方法生產各類產品：

```dart
class Apple extends ElectronicProductFactory {

  @override
  Product createComputer() {
    return Mac();
  }

  @override
  Product createMobile() {
    return IPhone();
  }

  @override
  Product createPad() {
    return IPad();
  }
  
  // ...
}
```

同樣地，對於華為、小米等電子產品廠商也可以使用相同的方式表示，這就是抽象工廠模式。

在開發 Flutter 應用中，我們也可以充分利用抽象工廠模式做切合應用的適配，我們可以定義如下這個抽象工廠，用於生產 widget：

```dart
abstract class IWidgetsFactory {
  
  Widget createButton(BuildContext context);
  
  Widget createDialog(BuildContext context);
  
  // ...
}
```

我們的應用通常需要針對各個平台展示不同風格的 widget。因此針對每一個平台，我們都可以實現對應的實現工廠，如下：

```dart
/// Material 風格元件工廠
class MaterialWidgetsFactory extends IWidgetsFactory {
  @override
  Widget createButton(
      BuildContext context, VoidCallback? onPressed, String text) {
    return ElevatedButton(
      child: Text(text),
      onPressed: onPressed,
    );
  }

  @override
  Widget createDialog(BuildContext context, String title, String content) {
    return AlertDialog(title: Text(title), content: Text(content));
  }
  
  /// ...
}

/// Cupertino 風格元件工廠
class CupertinoWidgetsFactory extends IWidgetsFactory {
  @override
  Widget createButton(
    BuildContext context,
    VoidCallback? onPressed,
    String text,
  ) {
    return CupertinoButton(
      child: Text(text),
      onPressed: onPressed,
    );
  }

  @override
  Widget createDialog(BuildContext context, String title, String content) {
    return CupertinoAlertDialog(
      title: Text(title),
      content: Text(content),
    );
  }
  
  // ...
}
```

這樣，在 Android 平臺上我們使用 `MaterialWidgetsFactory`，在 iOS 平臺上使用 `CupertinoWidgetsFactory`，就能使用對應平台的 widget，想要適配更多平臺只需要再繼承 `IWidgetsFactory` 實現對應平台的工廠類即可。

至此，我們可以發現，作為建立型模式，這三類工廠模式主要工作就是以不同的方式建立物件，但他們各有特點：簡單工廠模式抽象的是 **生產物件**，工廠方法模式抽象的是 **類方法**，工廠方法模式抽象的則是 **生產物件的工廠**，如何使用就見仁見智了。

### 拓展閱讀

- [百度百科：工廠方法模式](http://baike.baidu.com/l/JAsmKIAk)
- [百度百科：抽象工廠模式](http://baike.baidu.com/l/j5yzRvW)
- [Mangirdas Kazlauskas：Flutter Design Patterns: Abstract Factory](https://medium.com/flutter-community/flutter-design-patterns-11-abstract-factory-7098112925d8)

## 關於本系列文章

Flutter / Dart 設計模式從南到北（簡稱 Flutter 設計模式）系列內容預計兩週釋出一篇，著重向開發者介紹 Flutter 應用開發中常見的設計模式以及開發方式，旨在推進 Flutter / Dart 語言特性的普及，以及幫助開發者更高效地開發出高品質、可維護的 Flutter 應用。

我很樂意繼續完善本系列中的文章，如果您對本文還有任何疑問或者文章的建議，歡迎向中文社群官方 Github 儲存庫提交 issue 或者直接與我聯絡，我會及時回覆。
