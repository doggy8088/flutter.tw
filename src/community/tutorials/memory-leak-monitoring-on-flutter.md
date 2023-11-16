---
title: Flutter 上的記憶體洩漏監控
toc: true
---

文/ 戚耿鑫，快手應用研發平台組 Flutter 團隊

## 一、前言

Flutter 所使用的 Dart 語言具有垃圾回收機制，有垃圾回收就避免不了會記憶體洩漏。
在 Android 平臺上有個記憶體洩漏檢測工具 [LeakCanary](https://github.com/square/leakcanary)，
它可以方便地在 debug 環境下檢測當前頁面是否洩漏。
本文將會帶你實現一個 Flutter 可用的 LeakCanary，
並講述我是怎麼用該工具檢測出了 1.9.1 Framework 上的兩個洩漏。

## 二、Dart 中的弱參考

在具有垃圾回收的語言中，弱參考是檢測物件是否洩漏的一個好方式。
我們只需弱參考觀測物件，等待下次 **Full GC**，
如果 GC 之後物件為 `null`，說明被回收了，
如果不為 `null` 就可能是洩漏了。

Dart 語言中也有著弱參考，它叫 `Expando<T>` ，
看下它的 API：

<!-- skip -->
```dart
class Expando<T> {
  external T operator [](Object object);
  external void operator []=(Object object, T value);
}
```

你可能會好奇上述程式碼弱參考體現在哪裡呢？
其實是在 `expando[key]=value` 這個賦值陳述式上。
`Expando` 會以弱參考的方式持有 `key`，這裡就是弱參考的地方。

那麼問題來了，這個 `Expando` 弱參考持有的是 `key`，
但是本身又沒有提供 `getKey()` 這樣的 API，
我們就無從下手去得知 `key` 這個物件是否被回收了。

為了解決這個問題，我們來看下 `Expando` 的具體實現，
具體的程式碼在 [expando_path.dart](https://github.com/dart-lang/sdk/blob/master/sdk/lib/_internal/vm/lib/expando_patch.dart)：

<!-- skip -->
```dart
@path
class Expando<T> {
  // ...
  T operator [](Objet object) {
    var mask = _size - 1;
    var idx = object._identityHashCode & mask;
    // sdk 是把 key 放到了一個 _data 陣列內，這個 wp 是個 _WeakProperty
    var wp = _data[idx];

    // ... 省略部分程式碼
    return wp.value;
    // ... 省略部分程式碼
  }
}
```

{{site.alert.warning}}

此 patch 程式碼不適用於 Web 平台

{{site.alert.end}}

我們可以發現這個 `key` 物件是放到了 `_data` 陣列內，
用了一個 `_WeakProperty` 來包裹，那麼這個 `_WeakProperty` 就是關鍵類了，
看下它實現，程式碼在 [weak_property.dart](https://github.com/dart-lang/sdk/blob/master/sdk_nnbd/lib/_internal/vm/lib/weak_property.dart)：

<!-- skip -->
```dart
@pragma("vm:entry-point")
class _WeakProperty {

  get key => _getKey();
  // ... 省略部分程式碼
  _getKey() native "WeakProperty_getKey";
  // ... 省略部分程式碼
}
```

這個類有我們想要的 `key`，可以用於判斷物件是否還在。

怎麼獲取這種私有屬性和變數呢？
Flutter 中的 Dart 是不支援反射的（為了最佳化打包體積，關閉了反射），
有沒有其他辦法來獲取到這種私有屬性呢？

答案肯定是 **“有”**，為了解決上述問題，
我來向大家介紹一個 Dart 自帶的服務——
**Dart VM Service**。

## 三、Dart vm_service

[Dart VM Service](https://github.com/dart-lang/sdk/blob/master/runtime/vm/service/service.md) （後面簡稱 `vm_service`）
是 Dart 虛擬機器內部提供的一套 Web 服務，資料傳輸協議是 JSON-RPC 2.0。
不過我們並不需要要自己去實現資料請求解析，
官方已經寫好了一個可用的 Dart SDK 給我們用：[`vm_service`](https://pub.dev/packages/vm_service)。

### `ObjRef`, `Obj` 和 `id` 的作用

先介紹 `vm_service` 中的核心內容：**`ObjRef`、`Obj`、`id`**

`vm_service` 返回的資料主要分為兩大類，
`ObjRef`（參考型別） 和 `Obj`（物件例項型別）。
其中 `Obj` 完整的包含了 `ObjRef` 的資料，
並在其基礎上增加了額外資訊
（`ObjRef` 只包含了一些基本資訊，例如：`id`，`name` 等）。

基本所有的 `API` 返回的資料都是 `ObjRef`，
當 `ObjRef` 裡面的資訊滿足不了你的時候，
再呼叫 `getObject(,,,)`來獲取 `Obj`。

**關於 `id`：** `Obj` 和 `ObjRef` 都含有 `id`，
這個 `id` 是物件例項在 `vm_service` 裡面的一個識別符號，
`vm_service` 幾乎所有的 API 都需要透過 `id` 來操作，
比如：`getInstance(isolateId, classId, ...)`、
`getIsolate(isolateId)`、
`getObject(isolateId, objectId, ...)`。

### 如何使用 `vm_service` 服務

`vm_service` 在啟動的時候會在本地開啟一個 `WebSocket` 服務，
服務 URI 可以在對應的平臺中獲得：

- Android 在 `FlutterJNI.getObservatoryUri()` 中；
- iOS 在 `FlutterEngine.observatoryUrl` 中。

有了 URI 之後我們就可以使用 `vm_service` 的服務了，
官方有一個幫我們寫好的 SDK: [vm_service](https://pub.dev/packages/vm_service) ，
直接使用內部的 `vmServiceConnectUri` 就可以獲得一個可用的 `VmService` 物件。

{{site.alert.note}}

`vmServiceConnectUri` 的引數需要是一個 `ws://` 協議的 URI，
預設獲取的是 `http` 協議，
需要藉助 `convertToWebSocketUrl`方法轉化一下。

{{site.alert.end}}

## 四、洩漏檢測實現

有了 `vm_service` 之後，我們就可以用它來彌補 `Expando` 的不足了。
按照之前的分析，我們要獲 `Expando` 的私有欄位 `_data`，
這裡可以使用 [getObject(isolateId, objectId)](https://github.com/dart-lang/sdk/blob/master/runtime/vm/service/service.md#getobject) API，
它的返回值是 [Instance](https://github.com/dart-lang/sdk/blob/master/runtime/vm/service/service.md#instance)，
內部的 `fields` 欄位儲存了當前物件的所有屬性。
這樣我們就可以遍歷屬性獲取到 `_data`，來達到反射的效果。

現在的問題是 API 引數中的 `isoateId` 和 `objectId` 是什麼呢？
根據我前面介紹的 `id` 相關內容，它是物件在 `vm_serive` 中的識別符號。
也就是我們只有透過 `vm_service` 才可以獲取到這兩個引數。

### `IsolateId` 的獲取

`Isolate`（隔離區）是 Dart 裡面的一個非常重要的概念，
**基本上**一個 `isolate` 相當於一個執行緒，
但是和我們平常接觸的執行緒不同的是：不同 `isolate` 之間的記憶體不共享。

因為有了上述特性，我們在查詢物件的時候也要帶上 `isolateId`。
透過 `vm_service` 的 `getVM()` API 可以獲取到虛擬機器物件資料，
再透過 `isolates` 欄位可以獲取到當前虛擬機器所有的 `isolate`。

那麼怎麼篩選出我們想要的 `isolate` 呢？
這裡簡單起見只篩選主 `isolate`，
這部分的篩選可以檢視 [dev_tools](https://github.com/flutter/devtools) 
的原始碼: [service_manager.dart#\_initSelectedIsolate](https://github.com/flutter/devtools/blob/v0.2.5/packages/devtools_app/lib/src/service_manager.dart#L450) 函式。

### `ObjectId` 的獲取

我們要獲取的 `objectId` 就是 `expando` 在 `vm_service` 中的 `id`，
這裡可以把問題擴充下：

**如何獲取指定物件在 `vm_service` 中的 `id`？**

這個問題比較麻煩，`vm_service` 中沒有例項物件和 `id` 轉換的 API，
有個 `getInstance(isolateId, classId, limit)` 的 API，
可以獲取某個 `classId` 的所有子類別例項，
先不說如何獲取到想要的 `classId`，
此 API 的效能和 `limit` 都讓人擔憂。

沒有好辦法了嗎？其實我們可以
藉助 Library 的 **最上層函式（直接寫在當前檔案，不在類中，例如 main 函式）** 
來實現該功能。

{{site.alert.note}}

簡單說明下 Library 是什麼東西，
Dart 中的分包管理是根據 Library 來的，
同一個 Library 內的類別名稱不能重複，
一般情況下一個 `.dart` 檔案就是一個 Library，
當然也有例外，比如：part of 和 export。
{{site.alert.end}}

`vm_service` 有個 [invoke(isolateId, targetId, selector, argumentIds)](https://github.com/dart-lang/sdk/blob/master/runtime/vm/service/service.md#invoke) API，
可以用來執行某個常規函式
（`getter`、`setter`、建構函式、私有函式屬於非常規函式），
其中如果 `targetId` 是 Library 的 `id`，那麼 `invoke`
執行的就是 Library 的最上層函式。

有了 `invoke` Library 最上層函式的路徑，
就可以用它實現物件轉 `id` 了，程式碼如下：

<!-- skip -->
```dart
int _key = 0;
/// 最上層函式，必須常規方法，產生 key 用
String generateNewKey() {
  return "${++_key}";
}

Map<String, dynamic> _objCache = Map();
/// 最上層函式，根據 key 返回指定物件
dynamic keyToObj(String key) {
  return _objCache[key];
}

/// 物件轉 id
String obj2Id(VMService service, dynamic obj) async {

  // 找到 isolateId。這裡的方法就是前面講的 isolateId 獲取方法
  String isolateId = findMainIsolateId();
  // 找到當前 Library。這裡可以遍歷 isolate 的 libraries 欄位
  // 根據 uri 篩選出當前 Library 即可，具體不展開了
  String libraryId = findLibraryId();

  // 用 vm service 執行 generateNewKey 函式
  InstanceRef keyRef = await service.invoke(
    isolateId,
    libraryId,
    "generateNewKey",
    // 無引數，所以是空陣列
    []
  );
  // 獲取 keyRef 的 String 值
  // 這是唯一一個能把 ObjRef 型別轉為數值的 api
  String key = keyRef.valueAsString;

  _objCache[key] = obj;
  try {
    // 呼叫 keyToObj 最上層函式，傳入 key，獲取 obj
    InstanceRef valueRef = await service.invoke(
      isolateId,
      libraryId,
      "keyToObj",
      // 這裡注意，vm_service 需要的是 id，不是值
      [keyRef.id]
    )
    // 這裡的 id 就是 obj 對應的 id
    return valueRef.id;
  } finally {
    _objCache.remove(key);
  }
  return null;
}
```

### 物件洩漏判斷

現在我們已經可以獲取到 `expando` 例項在
`vm_service` 中的 `id` 了，接下來就簡單了。

先透過 `vm_service` 獲取到 `Instance`，
遍歷裡面的 `fields` 屬性，找到 `_data` 欄位
（注意 `_data` 是 `ObjRef` 型別），
用同樣的辦法把 `_data` 欄位轉成 `Instance` 型別
（`_data` 是個陣列，`Obj` 裡面有陣列的 child 資訊）。

遍歷 `_data` 欄位，如果都是 `null`，
表明我們觀測的 `key` 物件已經被釋放了。
如果 `item` 不為 `null`，
再次把 `item` 轉為 `Instance` 物件，
取它的 `propertyKey`
（因為 item 是 `_WeakProperty` 型別，
`Instance` 裡面特地為 `_WeakProperty` 開了這個欄位）。

### 強制 GC

文章開頭說到，如果要判斷物件是否洩漏，
需要在 Full GC 之後判斷弱參考是否還在。
有沒有辦法手動觸發 GC 呢？

答案是有的，`vm_service` 雖然沒有強制 GC 的 API，
但是 Dev Tools 的記憶體圖示右上角有個 GC 的按鈕，
我們仿照著它來操作就行！
Dev Tools 是呼叫了 `vm_service` 的 
[getAllocationProfile(isolateId, gc: true)](https://github.com/dart-lang/sdk/blob/master/runtime/vm/service/service.md#getallocationprofile) API 
來實現手動 GC 的。

至於這個 API 觸發的是不是 FULL GC，
並沒有說明，我測試觸發的都是 FULL GC，
如果要確定在 FULL GC 之後檢測洩漏，
可以監聽 gc 事件流，
`vm_service` 提供了該功能。

至此為止，我們已經可以實現洩漏的監控，
而且可以獲取到洩漏目標在 `vm_serive` 中的 `id` 了，
下面就開始獲取分析洩漏路徑。

## 五、獲取洩漏路徑

關於洩漏路徑的獲取，
`vm_service` 提供了一個 API 叫 [getRetainingPath(isolateId, objectId, limit)](https://github.com/dart-lang/sdk/blob/master/runtime/vm/service/service.md#getretainingpath)。
直接使用此 API 就可以獲取到洩漏物件到 GC Roots 的參考鏈資訊，
是不是感覺很簡單？不過光這樣可不行，
因為它有以下幾個坑點：

### Expando 持有問題

如果在執行 `getRetainingPath` 的時候，
洩漏物件被 `expando` 持有的話會產生以下兩個問題

- 因為該 API 返回的參考鏈只有一條，
  返回的參考鏈會經過 `expando`，導致無法獲取真正的洩漏節點資訊；
- 在 ARM 裝置上會出現 native crash，
  具體錯誤出現在 utf8 字元解碼上。

此問題很好解決，注意下在前面洩漏檢測完之後，
釋放掉 `expando` 就行。

### `id` 過期問題

`Instance` 型別的 `id` 和 `Class`、`Library`、`Isolate`
這種 `id` 不一樣，是會過期的。
`vm_service` 中對於此類臨時 `id` 的快取容量預設大小是 `8192`，
是一個迴圈佇列。

因為此問題的存在，我們在檢測到洩漏的時候，
不能只儲存洩漏物件的 `id`，需要儲存原物件，
而且不能強參考持有物件。
所以這裡我們還是需要使用 `expando`
來儲存我們檢測到的洩漏物件，
等到需要分析洩漏路徑的時候，
再把物件專為 `id`。

## 六、1.9.1 Framework 上的記憶體洩漏

完成了洩漏檢測和路徑獲取之後，
得到了一個簡陋的 leakcanary 工具。
當我在 1.9.1 版本的 Framework 下測試此工具的時候發現，
**我觀測一個頁面它就洩漏一個頁面！！！**

{{site.alert.note}}

透過 dev_tools dump 出來的物件來看，的確洩漏了！
{{site.alert.end}}

也就是 1.9.1 Framework 裡面存在著洩漏，
而且此洩漏會洩漏整個頁面。

接下來開始排查洩漏原因，這裡就碰到一個問題：
洩漏路徑太長：`getRetainingPath` 返回的鏈路長度有 300+，
排查了一下午也沒有找到問題根源。

結論：直接根據 `vm_service` 返回的資料是很難分析問題來源的，
需要對洩漏路徑的資訊二次處理下。

### 如何縮短參考鏈

首先看下洩漏路徑為什麼會這麼長，
透過觀測返回的鏈路後發現，
絕大部分的節點都是 Flutter UI 元件節點
（例如：`widget`、`element`、`state`、`renderObject`）。

也就是說參考鏈經過了 Flutter 的 widget tree，
熟悉 Flutter 的開發者應該都知道，
Flutter 的 widget tree 的層次是非常深的。
既然參考鏈長的原因是因為包含了 widget tree，
而且 widget tree 基本都是成塊出現的，
那我們只要把參考鏈中的節點根據型別來分類、聚合，
就可以大幅縮短洩漏路徑了。

#### 分類

根據 Flutter 的元件型別，將節點分為以下幾種型別：

- `element`：對應 `Element` 節點；
- `widget`：對應 `Widget` 節點；
- `renderObject`：對應 `RenderObject`節點；
- `state`：對應 `State<T extends StatefulWdget>` 節點；
- `collection`：對應集合型別節點，例如：`List`、`Map`、`Set`；
- other：對應其他節點。

#### 聚合

節點的分類做好了之後，就可以把相同型別的節點聚合一下。
這裡提下我的聚合方式：

把 `collection` 型別的節點看成了連線節點，
相鄰的相同節點合併到一個集合內，
如果兩個相同型別的集合中間是透過 `collection` 節點相連的，
就繼續把這兩個集合合併成一個集合，遞迴進行。

> 透過 **分類-聚合** 的處理後，原先 300+ 的鏈路長度，可以縮短為 100+。

繼續排查 1.9.1 Framework 的洩漏問題，
路徑雖然縮短了，可以找到問題大致出現在 `FocusManager` 節點上！
但是具體問題還是難以定位，主要有以下兩點：

- **參考鏈節點缺少程式碼位置**：因為 `RetainingObject` 資料中只有 `parentField`、`parentIndex` 和 `parentKey` 三個欄位來表示當前物件參考下一個物件的資訊，透過該資訊找程式碼位置效率低下；
- **無法知道當前 Flutter 元件節點的資訊**：比如 `Text` 的文字資訊，`element` 所在的 widget 是啥，state 的生命週期狀態，當前元件屬於哪個頁面，等等。

介於上述兩個痛點，還需要對洩漏節點的資訊做擴充處理：

- **程式碼位置**：節點的參考程式碼位置其實只需要解析 `parentField` 就行，透過 `vm_serive` 解析 `class`，取內部的 `field`，找到對應的 `script` 等資訊。此方法可以獲取到原始碼；
- **元件節點資訊**：Flutter 的 UI 元件都是繼承自 `Diagnosticable`，也就是隻要是 `Diagnosticable` 型別的節點都可獲取到非常詳細的資訊（dev_tools 除錯時候，元件樹資訊就是透過 `Diagnosticable.debugFillProperties` 方法獲取的）。除了這個還需要擴充當前元件所在 route 的資訊，這個很重要，判斷元件所在頁面用。

### 排查 1.9.1 Framework 洩漏根源

透過上述的種種最佳化後，我得到了下面這個工具，
在兩個 `_InkResponseState` 節點中發現了問題：

![](https://files.flutter-io.cn/posts/community/tutorial/images/GMbQYt.png){:width="70%"}

洩漏路徑中有兩個 `_InkResponseState` 節點所屬的 route 資訊不同，
表明這兩個節點在兩個不同的頁面中。
頂部 `_InkResponseState` 的描述資訊顯示 `lifecycle not mounted`，
說明元件已經銷燬了，但是還是被 `FocusManager` 參考著！
問題出現在這，來看下這部分程式碼

![](https://files.flutter-io.cn/posts/community/tutorial/images/GMbq7d.png){:width="90%"}

程式碼中可以明顯的看到 `addListener` 時候
對 `StatefulWidget` 的生命週期理解錯誤。
`didChangeDependencies` 是會多次呼叫的，
`dispose` 只會呼叫一次，
所以這裡就會出現 `listener` 移除不乾淨的情況。

修復了上述洩漏之後，發現還有一處洩漏。
排查後發現洩漏源在 `TransitionRoute` 中：

![](https://files.flutter-io.cn/posts/community/tutorial/images/toDJAS.jpg){:width="90%"}

當開啟一個新頁面的時候，
該頁面的 `Route`（也就是程式碼中的 `nextRoute`）
會被前一個頁面的 `animation` 所持有，
如果頁面跳轉都是 `TransitionRoute`，
那麼所有的 `Route` 都會洩漏！

{{site.alert.note}}

好訊息是以上洩漏都在 1.12 版本之後修復了。
{{site.alert.end}}

修復完上述兩個洩漏之後，
再次測試，`Route` 和 `Widget` 都可以回收了，
至此 1.9.1 Framework 排查完畢。

---

**本文作者：** 戚耿鑫

現就職於快手應用研發平台組 Flutter 團隊，負責 APM 方向開發研究。
從 2018 年開始接觸 Flutter，
在 Flutter 混合棧、工程化落地、UI 元件等方面有大量經驗。

聯絡方式：qigengxin@kuaishou.com
