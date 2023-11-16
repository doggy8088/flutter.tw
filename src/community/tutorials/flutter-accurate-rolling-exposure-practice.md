---
title: 實現一個精準滑動埋點
toc: true
---

文/ Vadaski，CFUG 社群成員，滴滴國際化研發工程師

## 前言

今天的這篇文章要介紹的內容，是我們經常會用到的一個場景：**埋點**。
為了對行為特徵的資料進行量化分析，最佳化產品，
我們常常需要在特定的時機上報資料埋點，想必大家都對它比較熟悉。
而曝光埋點則是其中的一個高頻使用場景。

## 🥲 滑動埋點的痛

在 Flutter 中，我們通常會在 `initState` 這個生命週期上報曝光埋點，這在一般的使用場景下當然是沒有問題的。然而在滑動場景下這個解決方案就不 work 了，我們來看看。

![listview_track.gif](https://files.flutter-io.cn/posts/community/tutorial/images/listview_track.gif)

很明顯，我們把本來沒有展示的 widget 也給打印出來了。如果這樣做，埋點上報不準確，將會給業務帶來不可恢復的損失。

## 🤯 ScrollView 載入機制

為什麼會出現這種情況呢？在查閱了原始碼之後，我們發現所有的 `ScrollView` 都是在一個可視區域 `Viewport` 當中進行繪製，為了讓滑動更加流暢，通常 `ScrollView` 都會在可視區域之外載入一部分，也就是 `cacheExtent`。落入該快取區域的專案即使在螢幕上尚不可見，也會進行佈局。這時候 `initState` 就被執行了。 `ListView` 作為 `ScrollView` 的子類同樣也使用了這個機制。

那麼很自然我們能夠想到一個最簡單的解決方案：把預載入機制給禁用掉不就可以了嘛。


``` dart
ListView.builder(
  cacheExtent: 0,
  itemCount: 40,
  itemBuilder: (context, index) {
    return Item(index: index);
  },
),
```

![no_cache_extent.gif](https://files.flutter-io.cn/posts/community/tutorial/images/no_cache_extent.gif)

好了，本文到此結束，你學會了嗎。😏

## 🤔 新的問題

開個玩笑，相信大家很容易就能夠聯想到，這樣做大機率會產生效能問題。在我們真實業務中，會考慮到支援的最差的裝置效能，以及業務的複雜性，肯定不是這樣簡單的取消掉預載入就能夠解決的。

在做測試的時候，會發現如果去掉快取機制，平均幀率會下降 5-10 幀左右，
還是在比較好的一加手機上的測試結果，這當然是不能接受的。
(更何況本身在 1.x 版本 的 Flutter 下 ListView 效能就有一些問題。)

所以我們想要的是一套 `Flutter` 上的高準確率的使用者行為埋點方案，而且不要影響到 `ScrollView` 的效能。

## 🤨 破局

想清楚了需求，就有了一半的思路。在我們查閱了業界現有的資料後，發現閒魚技術已經分享了一個比較好的解題思路：[# 揭秘！一個高準確率的Flutter埋點框架如何設計](https://juejin.cn/post/6844903864479514631#comment)。
奈何這個方案也沒有開源的計劃，那就只有自己來寫一個吧。這個問題應該如何解呢？

在前面我們提到過，每一個 `ScrollView` 都會有一個自己的 `ViewPort` 來決定自己的繪製範圍，這個 `ViewPort` 最後會產生一個 `RenderObjectElement`，這樣就可以單獨渲染這個區域，把影響返回控制到最小。那麼問題現在就變成了我們想要計算一個 Item 什麼時候進入到 ViewPort 中。

**一個複雜的問題需要把它抽象成更簡單的問題然後逐步求解**，我們不妨先把 item 看成一個點，看看要計算一個 Item 是否在 Viewport 內需要哪些資訊。

很容易能夠想到和滑動的偏移量 (Scroll Offset)，以及 Viewport 在滑動方向上的長度 (Viewport Length)，
還有 item 自身的資訊，也就是當前 item 距離滑動起始點的距離 (Exposure Offset) 相關。

![簡易關鍵變數.jpg](https://files.flutter-io.cn/posts/community/tutorial/images/simple_key_variable.jpg)

想象一下滑動的樣子，一個 Item 從 `ViewPort` 的右邊滑入，進入 `ViewPort`，被使用者看到，然後再從 `ViewPort` 的左邊劃出，這一系列過程。我們可以把這個過程抽象為下面的四個狀態：
- **Item 在 `ViewPort` 右側不可視範圍內**：(Scroll Offset + ViewPort Length < Exposure Offset)
- **Item 進入 `ViewPort` 右側**：(Scroll Offset + ViewPort Length > Exposure Offset)
- **Item 在 ViewPort 中**
- **Item 在 `ViewPort` 左側不可視範圍內**：(Exposure Offset < Scroll Offset)


對於從左邊劃入右邊則是這幾個狀態：

- **Item 在 `ViewPort` 左側不可視範圍內**：(Exposure Offset < Scroll Offset)
- **Item 進入 `ViewPort` 左側**：(Exposure Offset > Scroll Offset)
- **Item 在 ViewPort 中**
- **Item 在 `ViewPort` 右側不可視範圍內**：(Scroll Offset + ViewPort Length < Exposure Offset)

透過觀察可以發現，Item 從左邊劃入和從右邊劃入它的判斷時機是不一樣的，所以我們需要區分兩種滑動情況。

下面我們把 Item 自身的寬度 (Item Width）也帶上，再使用上面得出的結論來進行計算。

> 我們這裡暫時認為 Item 完全劃入 ViewPort 才算一次曝光。

![關鍵變數.jpg](https://files.flutter-io.cn/posts/community/tutorial/images/key_variable.jpg)

- **Item 在 `ViewPort` 右側不可視範圍內**：(Scroll Offset + ViewPort Length < Exposure Offset)
- **Item 進入 `ViewPort` 右側**：(Scroll Offset + ViewPort Length > Exposure Offset)
- **Item 在 ViewPort 中**
- **Item 在 `ViewPort` 左側不可視範圍內**：(Exposure Offset + Item Width < Scroll Offset)


對於從左邊劃入右邊則是這幾個狀態：

- **Item 在 `ViewPort` 左側不可視範圍內**：(Exposure Offset + Item Width < Scroll Offset)
- **Item 進入 `ViewPort` 左側**：(Exposure Offset + Item Width > Scroll Offset)
- **Item 在 ViewPort 中**
- **Item 在 `ViewPort` 右側不可視範圍內**：(Scroll Offset + ViewPort Length < Exposure Offset)

## 🧩 如何獲取這些資訊

知道了解法之後，接下來就只需要尋找這些拼圖的碎片就行了。

### Item 大小資訊

這塊比較簡單，我們都知道可以透過 Widget 的 `BuildContext` 拿到它所對應的 `RenderObject`，透過它去拿當前 Item 的長度和寬度。

``` dart
// 這裡命名為曝光坑位的大小，對於不同滑動方向，我們需要用不同方向的長度。
final exposurePitSize = (context.findRenderObject() as RenderBox).size;
```
這裡的 context 是我們想要判斷是否曝光的 Item 的 context，如果你對這個概念還不太清楚，可以去看看這篇 [深入理解BuildContext](https://juejin.cn/post/6844903777565147150)。

> 注意：不是每個 `Widget` 都會建立一個 `RenderObject`，只有 `RenderObjectWidget` 才會建立 `RenderObject`。 `ListView` 會預設幫每一個 Item 新增一個 `RepaintBoundary`，這個 `Widget` 是一個 `SingleChildRenderObjectWidget`，所以每一個 Item 其實都會有一個它所對應的 `RenderObject`。 

``` dart
// SliverChildListDelegate 的 build 方法
if (addRepaintBoundaries) child = RepaintBoundary(child: child);
```
### ViewPort 大小資訊

我們在進行曝光判斷的時候，肯定是在每一個 Item 中進行的，而 `ViewPort` 則是存在於 `ListView` 這一層級，所以我們需要從祖先的節點中找到它，幸運的是，Flutter 已經為我們提供了這個方法。
``` dart
static RenderAbstractViewport? of(RenderObject? object) {
  while (object != null) {
    if (object is RenderAbstractViewport)
      return object;
    object = object.parent as RenderObject?;
  }
  return null;
}
```
我們剛剛已經拿到了 Item 對應的渲染物件，`RenderAbstractViewport.of` 可以透過這個 `RenderObject` 向上尋找祖先節點，直到發現離它最近一個節點的 `RenderAbstractViewport` 就能拿到我們想要的 `ViewPort` 資訊了。

``` dart
Size? getViewPortSize(BuildContext context) {
  final RenderObject? box = context.findRenderObject();
  final RenderAbstractViewport? viewport = RenderAbstractViewport.of(box);
  assert(() {
    if (viewport != null) {
      debugPrint('Please make sure you have a `ScrollView` in ancestor');
      return false;
    }
    return true;
  });
  final Size? size = viewport?.paintBounds.size;
  return size;
}
```
### Item 相對 ViewPort 的滑動起始點的距離

在 `RenderAbstractViewport` 的另一個方法 `getOffsetToReveal`，中，我們可以獲得當前的 `RenderObject` 相對於這個 ViewPort 滑動的起始位置。

``` dart
double getExposureOffset(BuildContext context) {
  final RenderObject? box = context.findRenderObject();
  final RenderAbstractViewport? viewport = RenderAbstractViewport.of(box);

  if (viewport == null || box == null || !box.attached) {
    return 0.0;
  }

  // box 為當前 Item 的 RenderObject
  // alignment 為 0 的時候獲得距離起點的相對偏移量
  // 為 1 的時候獲得距離終點的相對偏移量。
  final RevealedOffset offsetRevealToTop =
      viewport.getOffsetToReveal(box, 0.0, rect: Rect.zero);
  return offsetRevealToTop.offset;
}
```

### 滑動距離

要獲得滑動距離通常有兩種方式：
- 透過 `ScrollController` 獲得。
- 利用 Scrollable Widget 的 `Notification` 機制。

每次編寫程式碼的時候都必須得寫 `ScrollController` 看上去有些麻煩，所以我們選擇了`Notification` 這種方式。(它也更加通用)

#### Scroll Notification

Scrollable Widget 將會向其其祖先通知有關滾動變化資訊，而這些資訊能夠使用 `NotificationListener` 來捕獲到。目前有下面幾種 `Notification`:

- `ScrollStartNotification`：滾動開始時發起 `Notification`。
- `ScrollUpdateNotification`：滾動進行時不斷髮起 `Notification`。(頻率很高)
- `ScrollEndNotification`：滾動結束時發起 `Notification`。
- `UserScrollNotification`：當用戶改變滾動方向時，發起通知。(通常在不同方向的 ScrollView 互相巢狀(Nesting)時會出現)

我們這裡使用 `NotificationListener` 來獲取 滑動的資訊。

``` dart
Widget buildNotificationWidget(BuildContext context, Widget child) {
  return NotificationListener<ScrollNotification>(
    onNotification: (scrollNotification) {
      // 這裡就能獲取到滾動資訊
    },
    child: ScrollView,
  );
}
```

#### 解決資訊共享問題

看到這裡，似乎我們要的拼圖都湊齊了，但是總感覺哪裡不對勁？🧐

如果你敏銳的話，想必已經發現我們現在這樣的設計根本沒法在一個地方拿到全部資訊。

![資料獲取位置不一致.jpg](https://files.flutter-io.cn/posts/community/tutorial/images/tree.jpg)

Scroll Notification 僅會向祖先節點發起 Notification 通知，也就是說，我們在 Item 層級是拿不到的！

如果我們想要在 Item 中進行埋點曝光判定，就必須要獲取到更高的祖先節點中的 scrollNotification。

當然解法肯定有很多，共享狀態的方法在狀態管理中是一個常見的 Case，但是為了滑動埋點曝光就引入一個狀態管理庫似乎有些得不償失，所以還不如使用 Flutter 最原始的 Inherit 機制來實現資料的共享。

##### 什麼是 Inherit 機制

要理解 Inherit 機制，首先你需要了解 Flutter 的三棵樹，
這個網上的解釋文章已經有很多了，我就不再贅述，
感興趣的可以看看 [迷鹿](https://juejin.cn/user/4309694831660711)
的這篇 [Widget、Element、Render是如何形成樹結構？](https://juejin.cn/post/6921493845330886670)。

簡單來說，Inherit 機制是一種能夠在 Flutter 中自頂向下共享資料的方式，我們知道 Flutter 是透過樹形結構來建構檢視的，而其中的 `InheritedWidget` 則是能夠讓它的資料能夠被所有子節點中的 Widget 存取到。

它的原理也是很簡單，每個 Element 都持有了一個叫做 `Map<Type, InheritedElement>? _inheritedWidgets` 的 `Map` 的參考，當我們的 Element 在掛載到 Element Tree 的時候 (執行 `mount` 操作的時候會呼叫 `_updateInheritance`)，將會把 parent 中儲存的 `_InheritedWidget` 參考自己也給留一份。

``` dart
void _updateInheritance() {
  assert(_lifecycleState == _ElementLifecycle.active);
  _inheritedWidgets = _parent?._inheritedWidgets;
}
```

而 `InheritedWidget` 建立的 Element 則會在 mount 的時候把自己給塞到這個 map 當中，這樣就完成了自頂向下的資料共享了。

```
@override
void _updateInheritance() {
  assert(_lifecycleState == _ElementLifecycle.active);
  final Map<Type, InheritedElement>? incomingWidgets = _parent?._inheritedWidgets;
  if (incomingWidgets != null)
    _inheritedWidgets = HashMap<Type, InheritedElement>.from(incomingWidgets);
  else
    _inheritedWidgets = HashMap<Type, InheritedElement>();
  _inheritedWidgets![widget.runtimeType] = this;
}
```

基於此，我們就可以完成對於滑動埋點曝光的計算了，可喜可賀。

## 拿來吧你

像我們這樣有經驗的開發者，看到這樣好的文章，第一時間那一定是想要~~自己實踐一下~~

> 直接拿來吧你

所以為了各位寶貴的 (滑水/嘮嗑/帶娃/...) 時間，這款滑動埋點方案已經登陸了 [Pub 儲存庫](https://pub.flutter-io.cn/packages/flutter_exposure)，各位可以放心食用了。

目前已經支援的有：

- 懶曝光模式：僅當滾動結束時再曝光。
- 曝光比例：可以控制 Item 展現多大的範圍算是一次曝光。
- 追蹤 Item 何時離開可視範圍：可以獲取到曝光時長。
- 支援所有 ScrollView：包括 `ListView`、`GridView`、`CustomScrollView` 等等。

這個專案我會一直維護下去 (畢竟自己也要用)，
如果你想了解該專案的最新進展，
可以關注該專案的 [GitHub](https://github.com/Vadaski/flutter_exposure)，
或者有需要增加的功能需求，也歡迎透過 [郵箱](mailto:xinlei966@gmail.com) 與我聯絡～

Pub 地址：https://pub.flutter-io.cn/packages/flutter_exposure

Github 地址：https://github.com/Vadaski/flutter_exposure

郵箱：xinlei966@gmail.com

## 寫在最後

這個解決方案其實是在去年公司裡就用到了，一直沒有來得及開源。
在這裡也感謝 [閒魚技術](https://juejin.cn/post/6955304605190357005) 提供的寶貴思路，
最近湊了一些零零碎碎的時間把它給完成了，把趁著國慶第一天寫完了這篇文章，
希望大家能透過我的分享有一點點收穫～

我是鑫磊，和你一起快樂學習 Flutter 的工程師，大家國慶快樂，我們之後再見👋
