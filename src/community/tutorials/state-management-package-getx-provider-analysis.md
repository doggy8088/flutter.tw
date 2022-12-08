---
title: Flutter 狀態管理框架 Provider 和 Get 分析
toc: true
---

文/ Nayuta，CFUG 社群

狀態管理一直是 Flutter 開發中一個火熱的話題。談到狀態管理框架，社群也有諸如有以
[Get](https://pub.dev/packages/get)、[Provider](https://pub.dev/packages/provider)
為代表的多種方案，它們有各自的優缺點。
面對這麼多的選擇，你可能會想：「我需要使用狀態管理麼？哪種框架更適合我？」
本文將從作者的實際開發經驗出發，分析狀態管理解決的問題以及思路，希望能幫助你做出選擇。

## 為什麼需要狀態管理？

首先，為什麼需要狀態管理？
根據筆者的經驗，這是因為 Flutter 基於
[**宣告式**](https://flutter.tw/resources/architectural-overview#reactive-user-interfaces) 建構 UI ，
使用狀態管理的目的之一就是解決「宣告式」開發帶來的問題。

「宣告式」開發是一種區別於傳原生的方式，所以我們沒有在原生開發中聽到過狀態管理，那如何理解「宣告式」開發呢？

### 「宣告式」VS「命令式」分析

以最經典的的計數器例子分析：

![透過計數器 app 理解 Flutter 的「宣告式」和「命令式」](https://files.flutter-io.cn/posts/community/tutorial/images/image-20220417103627166.jpg)

如上圖所示：點選右下角按鈕，顯示的文字數字加一。
Android 中可以這麼實現：當右下角按鈕點中時，
拿到 `TextView` 的物件，手動設定其展示的文字。

實現程式碼如下：

```java
// 一、定義展示的內容
private int mCount =0;
 
// 二、中間展示數字的控制項 TextView
private TextView mTvCount;
 
// 三、關聯 TextView 與 xml 中的元件
mTvCount = findViewById(R.id.tv_count)
 
// 四、點選按鈕控制組件更新
private void increase( ){ 
	mCount++;
	mTvCounter.setText(mCount.toString()); 
}

```

而在 Flutter 中，我們只需要使變數增加之後呼叫 `setState((){})` 即可。`setState` 會重新整理整個頁面，使得中間展示的值進行變更。

```dart
// 一、宣告變數
int _counter =0; 

// 二、展示變數 
Text('$_counter')

//  三、變數增加，更新介面
setState(() {
   _counter++; 
});
```

可以發現，Flutter 中只對 `_counter` 屬性進行了修改，並沒有對 Text 元件進行任何的操作，整個介面隨著狀態的改變而改變。

所以在 Flutter 中有這麼一種說法: **UI = f(state)**:

![](https://files.flutter-io.cn/posts/community/tutorial/images/2022-05-03-ui-equals-function-of-state.jpg)

上面的例子中，狀態 (state) 就是 `_counter` 的值，呼叫 `setState` 驅動 `f` build 方法產生新的 UI。

那麼，宣告式有哪些優勢，並帶來了哪些問題呢？

**優勢: 讓開發者擺脫元件的繁瑣控制，聚焦於狀態處理**

習慣 Flutter 開發之後，回到原生平臺開發，你會發現當多個元件之間相互關聯時，對於 View 的控制非常麻煩。

而在 Flutter 中我們只需要處理好狀態即可 (複雜度轉移到了狀態 -> UI 的對映，也就是 Widget 的建構)。包括 Jetpack Compose、Swift 等技術的最新發展，也是在朝著「宣告式」的方向演進。

**宣告式開發帶來的問題**

沒有使用狀態管理，直接「宣告式」開發的時候，遇到的問題總結有三個:
1. 邏輯和頁面 UI 耦合，導致無法複用/單元測試、修改混亂等
1. 難以跨元件 (跨頁面) 存取資料
1. 無法輕鬆的控制重新整理範圍 (頁面 setState 的變化會導致全域頁面的變化)

接下來，我先帶領大家逐個瞭解這些問題，下一章向大家詳細描述狀態管理框架如何解決這些問題。

**1) 邏輯和頁面 UI 耦合，導致無法複用/單元測試、修改混亂等**

![](https://files.flutter-io.cn/posts/community/tutorial/images/image-20220416153119414.jpg)

一開始業務不復雜的時候，所有的程式碼都直接寫到 widget 中，隨著業務迭代，
檔案越來越大，其他開發者很難直觀地明白裡面的業務邏輯。
並且一些通用邏輯，例如網路請求狀態的處理、分頁等，在不同的頁面來回貼上。

這個問題在原生上同樣存在，後面也衍生了諸如 MVP 設計模式的思路去解決。

**2) 難以跨元件 (跨頁面) 存取資料**

![](https://files.flutter-io.cn/posts/community/tutorial/images/image-20220416152601484.jpg)

第二點在於跨元件互動，比如在 Widget 結構中，
一個子元件想要展示父元件中的 `name` 欄位，
可能需要層層進行傳遞。

又或者是要在兩個頁面之間共享篩選資料，
並沒有一個很優雅的機制去解決這種跨頁面的資料存取。

**3) 無法輕鬆的控制重新整理範圍 (頁面 setState 的變化會導致全域頁面的變化)**

最後一個問題也是上面提到的優點，很多場景我們只是部分狀態的修改，例如按鈕的顏色。
但是整個頁面的 `setState` 會使得其他不需要變化的地方也進行重建，
帶來不必要的開銷。

## Provider、Get 狀態管理框架設計分析

Flutter 中狀態管理框架的核心在於這三個問題的解決思路，
下面一起看看 Provider、Get 是如何解決的：

### 解決邏輯和頁面 UI 耦合問題

傳統的原生開發同樣存在這個問題，Activity 檔案也可能隨著迭代變得難以維護，
這個問題可以透過 MVP 模式進行解耦。

簡單來說就是將 View 中的邏輯程式碼抽離到 Presenter 層，
View 只負責檢視的建構。

![](https://files.flutter-io.cn/posts/community/tutorial/images/image-20220416152955696.jpg)

這也是 Flutter 中幾乎所有狀態管理框架的解決思路，
上圖的 Presenter 你可以認為是 Get 中的 `GetController`、
Provider 中的 `ChangeNotifier` 或者 Bloc 中的 `Bloc`。
值得一提的是，具體做法上 Flutter 和原生 MVP 框架有所不同。

我們知道在經典 MVP 模式中，
一般 View 和 Presenter 以介面定義自身行為 (action)，
**相互持有介面進行呼叫** 。

![](https://files.flutter-io.cn/posts/community/tutorial/images/image-20220416153312721.jpg)

但 Flutter 中不太適合這麼做，
從 Presenter → View 關係上 View 在 Flutter 中對應 Widget，
但在 Flutter 中 Widget 只是使用者宣告 UI 的配置，
直接控制 Widget 例項並不是好的做法。

而在從 View → Presenter 的關係上，
Widget 可以確實可以直接持有 Presenter，
但是這樣又會帶來難以資料通訊的問題。

這一點不同狀態管理框架的解決思路不一樣，從實現上他們可以分為兩大類：

- 透過 **Flutter 樹機制** 解決，例如 Provider；
- 透過 **依賴注入**，例如 Get。

**1) 透過 Flutter 樹機制處理 V → P 的獲取**

![](https://files.flutter-io.cn/posts/community/tutorial/images/2022-05-03-three-trees-on-flutter.jpg)

```dart
abstract class Element implements BuildContext { 
	/// 當前 Element 的父節點
	Element? _parent; 
}

abstract class BuildContext {
	/// 查詢父節點中的T型別的State
	T findAncestorState0fType<T extends State>( );

	/// 遍歷子元素的element物件
	void visitChildElements(ElementVisitor visitor);

	/// 查詢父節點中的T型別的 InheritedWidget 例如 MediaQuery 等
	T dependOnInheritedWidget0fExactType<T extends InheritedWidget>({ 
		Object aspect });
	……
} 
```
<center> Element 實現了父類 BuildContext 中操作樹結構的方法 </center>

我們知道 Flutter 中存在三棵樹，Widget、Element 和 RenderObject。
所謂的 **Widget 樹其實只是我們描述元件巢狀(Nesting)關係的一種說法，是一種虛擬的結構**。
但 Element 和 RenderObject 在執行時實際存在，
可以看到 Element 元件中包含了 `_parent` 屬性，存放其父節點。
而它實現了 `BuildContext` 介面，包含了諸多對於樹結構操作的方法，
例如 `findAncestorStateOfType`，向上查詢父節點；
`visitChildElements` 遍歷子節點。

在一開始的例子中，我們可以透過 `context.findAncestorStateOfType`
一層一層地向上查詢到需要的 Element 物件，
獲取 Widget 或者 State 後即可取出需要的變數。

![](https://files.flutter-io.cn/posts/community/tutorial/images/image-20220416154300160.jpg)

provider 也是藉助了這樣的機制，完成了 View -> Presenter 的獲取。
透過 `Provider.of` 獲取最上層 Provider 元件中的 Present 物件。
顯然，所有 Provider 以下的 Widget 節點，
都可以透過自身的 context 存取到 Provider 中的 Presenter，
很好地解決了跨元件的通訊問題。

**2) 透過依賴注入的方式解決 V → P**

樹機制很不錯，但依賴於 context，這一點有時很讓人抓狂。
我們知道 Dart 是一種單執行緒的模型，
所以不存在多執行緒下對於物件存取的競態問題。
基於此 Get 藉助一個全域單例的 Map 儲存物件。
透過依賴注入的方式，實現了對 Presenter 層的獲取。
這樣在任意的類中都可以獲取到 Presenter。

![](https://files.flutter-io.cn/posts/community/tutorial/images/image-20220416154732460.jpg)

這個 Map 對應的 key 是 `runtimeType` + `tag`，
其中 tag 是可選引數，而 value 對應 `Object`，
也就是說我們可以存入任何型別的物件，並且在任意位置獲取。

### 解決難以跨元件 (跨頁面) 存取資料的問題

這個問題其實和上一部分的思考基本類似，所以我們可以總結一下兩種方案特點：

![](https://files.flutter-io.cn/posts/community/tutorial/images/image-20220416154955957.jpg)

**Provider**
* 依賴樹機制，必須基於 context
* 提供了子元件存取上層的能力

**Get**
* 全域單例，任意位置可以存取
* 存在型別重複，記憶體回收問題

### 解決高層級 setState 引起不必要重新整理的問題

最後就是我們提到的高層級 `setState` 引起不必要重新整理的問題，
Flutter 透過採用觀察者模式解決，其關鍵在於兩步：
1. 觀察者去訂閱被觀察的物件；
1. 被觀察的物件通知觀察者。

![](https://files.flutter-io.cn/posts/community/tutorial/images/2022-05-03-2-steps-of-the-observer-mode.jpg)

系統也提供了 `ValueNotifier` 等元件的實現：

```dart
/// 宣告可能變化的資料
ValueNotifier<int> _statusNotifier = ValueNotifier(0); 

ValueListenableBuilder<int>(
	// 建立與 _statusNotifier 的繫結關係 
	valueListenable: _statusNotifier, 
	builder: (c, data, _) {
		return Text('$data'); 
})

///資料變化驅動 ValueListenableBuilder 區域性重新整理 
_statusNotifier.value += 1;

```

瞭解到最基礎的觀察者模式後，看看不同框架中提供的元件：

比如 Provider 中提供了 `ChangeNotifierProvider`:

```dart
class Counter extend ChangeNotifier { 
	int count = 0;

	/// 呼叫此方法更新所有觀察節點
	void increment() {
		count++;
		notifyListeners(); 
	}
}

void main() { 
	runApp(
		ChangeNotifierProvider(
			///  返回一個實現 ChangeNotifier 介面的物件 
			create: (_) => Counter(),
			child: const MyApp( ), 
		),
	);
 }

///  子節點透過 Consumer 獲取 Counter 物件 
Consumer<Counter>(
	builder:(_, counter, _) => Text(counter.count.toString()) 

```

還是之前計數器的例子，這裡 `Counter` 繼承了
`ChangeNotifier` 透過最上層的 Provider 進行儲存。
子節點透過 Consumer 即可獲取例項，
呼叫了 `increment` 方法之後，只有對應的 Text 元件進行變化。

同樣的功能，在 Get 中，
只需要提前呼叫 `Get.put` 方法儲存 `Counter` 物件，
為 `GetBuilder` 元件指定 `Counter` 作為泛型。
因為 Get 基於單例，所以 `GetBuilder` 可以直接透過泛型獲取到存入的物件，
並在 builder 方法中暴露。這樣 `Counter` 便與元件建立了監聽關係，
之後 `Counter` 的變動，只會驅動以它作為泛型的 `GetBuilder` 元件更新。

```dart
class Counter extends GetxController { 
	int count = 0;

	void increase() { 
		count++;
		update(); 
	}
}

/// 提前進行儲存
final counter = Get.put(Counter( )); 

/// 直接透過泛型獲取儲存好的例項
GetBuilder<Counter>(
	builder: (Counter counter) => Text('${counter.count}') ); 

```

## 實踐中的常見問題

在使用這些框架過程中，可能會遇到以下的問題：

### Provider 中 context 層級過高

```dart
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Provider(
      create: (_) => const Count(),
      child: MaterialApp(
        home: Scaffold(
          body: Center(child: Text('${Provider.of<Counter>(context).count}')),
        ),
      ),
    );
  }
}
```

![](https://files.flutter-io.cn/posts/community/tutorial/images/2022-05-03-provider-level.jpg)

如程式碼所示，當我們直接將 Provider 與元件巢狀(Nesting)於同一層級時，
這時程式碼中的 `Provider.of(context)` 執行時丟擲 `ProviderNotFoundException`。
因為此處我們使用的 context 來自於 MyApp，
但 Provider 的 element 節點位於 MyApp 的下方，
所以 `Provider.of(context)` 無法獲取到 Provider 節點。
這個問題可以有兩種改法，如下方程式碼所示：

**改法 1: 透過巢狀(Nesting) Builder 元件，使用子節點的 context 存取:**

```dart
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Provider(
      create: (_) => const Count(),
      child: MaterialApp(
        home: Scaffold(
          body: Center(
            child: Builder(builder: (builderContext) {
              return Text('${Provider.of<Counter>(builderContext).count}');
            }),
          ),
        ),
      ),
    );
  }
}
```

**改法 2: 將 Provider 提至最上層:**

```dart
void main() {
  runApp(
    Provider(
      create: (_) => Counter(),
      child: const MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Center(child: Text('${Provider.of<Counter>(context).count}')),
      ),
    );
  }
}
```

### Get 由於全域單例帶來的問題

正如前面提到 Get 透過全域單例，預設以 `runtimeType` 為 key 進行物件的儲存，
部分場景可能獲取到的物件不符合預期，例如商品詳情頁之間跳轉。
由於不同的詳情頁例項對應的是同一 Class，即 `runtimeType` 相同。
如果不新增 tag 引數，在某個頁面呼叫 `Get.find` 會獲取到其它頁面已經儲存過的物件。
同時 Get 中一定要注意考慮到物件的回收，不然很有可能引起記憶體洩漏。
要麼手動在頁面 `dispose` 的時候做 `delete` 操作，
要麼完全使用 Get 中提供的元件，例如 `GetBuilder`，
它會在 `dispose` 中釋放。

![](https://files.flutter-io.cn/posts/community/tutorial/images/2022-05-03-getx-runtimetype.jpg)

`GetBuilder` 中在 `dispose` 階段進行回收:

```dart
@override
void dispose() {
  super.dispose();
  widget.dispose?.call(this);
  if (_isCreator! || widget.assignId) {
    if (widget.autoRemove && GetInstance().isRegistered<T>(tag: widget.tag)) {
      GetInstance().delete<T>(tag: widget.tag);
    }
  }

  _remove?.call();

  controller = null;
  _isCreator = null;
  _remove = null;
  _filter = null;
}

```

## Get 與 Provider 優缺點總結

透過本文，我向大家介紹了狀態管理的必要性、它解決了 Flutter 開發中的哪些問題以及是如何解決的，
與此同時，我也為大家總結了在實踐中常見的問題等，看到這裡你可能還會有些疑惑，到底是否需要使用狀態管理？

在我看來，框架是為了解決問題而存在。所以這取決於你是否也在經歷一開始提出的那些問題。
如果有，那麼你可以嘗試使用狀態管理解決；如果沒有，則沒必要過度設計，為了使用而使用。

其次，如果使用狀態管理，那麼 Get 和 Provider 哪個更好？

這兩個框架各有優缺點，我認為如果你或者你的團隊剛接觸 Flutter，
使用 Provider 能幫助你們更快理解 Flutter 的核心機制。
而如果已經對 Flutter 的原理有了解，Get 豐富的功能和簡潔的 API，
則能幫助你很好地提高開發效率。
