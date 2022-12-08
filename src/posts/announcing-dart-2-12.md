---
title: Dart 2.12 現已釋出
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/03/6b1a0818de0a5.png)

*作者 / Michael Thomsen*

Dart 2.12 現已釋出，其中包含 [健全的空安全](https://dart.tw.gh.miniasp.com/null-safety) 和 [Dart FFI](https://dart.tw.gh.miniasp.com/guides/libraries/c-interop) 的穩定版。空安全是我們最新主打的一項生產力強化功能，意在幫助您規避空值錯誤，以前這種錯誤通常很難被發現，您可以觀看下面這支影片瞭解詳情。FFI 則是一種互操作機制，支援呼叫以 C 語言編寫的既有程式碼，例如呼叫 Windows [Win32 API](https://pub.dev/packages/win32)。歡迎大家即刻開始使用 Dart 2.12。

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=415467484&bvid=BV1GV411Y7sW&cid=262014735&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

## **Dart 平台的獨特功能**

在詳細瞭解健全空安全和 FFI 之前，我們先來討論一下它們在哪些方面契合了我們對 Dart 平台的期望。程式語言往往有很多類似的功能，例如，很多語言都支援面向物件的程式設計或在 web 上執行。真正將各個語言區分開來的，是其獨特的功能組合。

![](https://devrel.andfun.cn/devrel/posts/2021/03/6773481aed985.jpg)

Dart 具有橫跨三個維度的獨特功能組合:

* **可移植性:** 高效的編譯器可針對裝置產生 x86 和 ARM 機器程式碼，並針對 web 產生最佳化的 JavaScript。同時相容移動裝置、桌面 PC、應用後端等多種 [目標平台](https://dart.tw.gh.miniasp.com/overview#platform)。大量的開發庫和 package 提供了可在所有平臺上使用的一致的 API，進一步降低了開發者建立真正多平臺應用的成本。

* **高生產力:** Dart 平台支援熱重載，因此可在原生裝置和 web 上實現快速迭代開發。此外，Dart 還提供了豐富的結構，如 isolates 和 async/await 等，用以處理和實現常見的併發和事件驅動的應用模式。

* **穩健:** Dart 的健全空安全型別系統可以在開發過程中就捕捉到錯誤。整個平台擁有極好的可擴充性和可靠性，已經被大量且多樣的應用在累計超過十年的生產環境中實戰檢驗過，其中包括 Google 的一些關鍵業務應用，如 Google Ads 和 Google Assistant 等。

健全空安全增強了型別系統的穩健性，同時提高了效能。藉助 Dart FFI，您可以獲得更強的可移植性，同時沿用由 C 語言編寫的既有程式碼，在處理對效能要求極為嚴苛的任務時，可以盡情使用經過精心最佳化的 C 語言程式碼。

## **健全的空安全**

自 [Dart 2.0](https://medium.com/dartlang/announcing-dart-2-80ba01f43b6) 中引入健全型別系統以來，Dart 語言中最重大的新增內容便是健全空安全。空安全進一步增強了型別系統，讓您能夠捕捉到空值錯誤，此類錯誤經常導致應用崩潰。啟用空安全後，您就可以在開發過程中捕捉到空值錯誤，避免應用在生產環境中發生崩潰。

健全空安全的設計圍繞一套核心原則展開。您可以閱讀 [官方文件](https://dart.tw.gh.miniasp.com/null-safety#null-safety-principles) 瞭解這些原則對開發者的影響。

### **預設不可空: 從根本改變型別系統**

在空安全出現之前，開發者面臨的核心挑戰在於無法區分預期收到空值的程式碼和不接受空值的程式碼。幾個月前，我們在 Flutter 的 master 渠道中發現了一個錯誤，多個 flutter 工具命令在特定計算機配置下會發生崩潰，並觸發空值錯誤: `The method '>=' was called on null`。問題出自如下程式碼:

```
final int major = version?.major;
final int minor = version?.minor;
if (globals.platform.isMacOS) {
 // plugin path of Android Studio changed after version 4.1.
 if (major >= 4 && minor >= 1) {
 ...
```

您發現錯誤了嗎？由於 `version` 可能為空，所以 `major` 和 `minor` 也可能為空。如果單獨檢查此處程式碼，這一錯誤似乎並不難發現。但實際上，即使經過了嚴格的程式碼審查過程 (如 Flutter repo 所採用的程式碼審查流程)，也總是難免有這樣的漏網之魚。在啟用空安全後，靜態分析能夠立即捕捉到這一問題 (如下圖)。您可以 [在 DartPad 中親自上手體驗](https://dartpad.dev/0e9797be7488d8ec6c3fca92b7f2740f?null_safety=true)。

![△ IDE 中的分析結果](https://devrel.andfun.cn/devrel/posts/2021/03/7260c6d9d0a2a.png)

△ IDE 中的分析結果

這只是一個非常簡單的錯誤。我們早期在 Google 內部的程式碼中使用空安全時，捕捉到的複雜錯誤遠多於此。其中一些是多年前就已經發現的 bug，但在透過空安全進行額外的靜態檢查前，很多團隊都未能找到原因。

* 內部團隊發現，他們經常檢查表示式中是否存在空值，而這些表示式永遠不可能為空。這個問題在使用 [protobuf](https://developers.google.cn/protocol-buffers) 的程式碼中最常見，其中可選欄位在未經設定時會返回一個預設值，而且永不為空。這會導致程式碼混淆預設值和空值，並錯誤地檢查預設條件。
* Google Pay 團隊在他們的 Flutter 程式碼中發現了一些 bug，在嘗試存取 Widget 上下文之外的 Flutter State 物件時會出錯。在採用空安全之前，這些物件會返回 null 並掩蓋錯誤；在採用空安全之後，健全分析確定這些屬性永遠不可能為空，並會給出分析錯誤。
* Flutter 團隊發現了一個 bug: 如果在 Window.render() 中向 scene 引數傳遞空值，則 Flutter 引擎可能會崩潰。在向空安全遷移的過程中，他們添加了一個提示，[將 Scene 標記為不可空](https://github.com/cbracken/engine/blob/bad869e229a8a02cad6e63d12e80807b33b5c12f/lib/ui/window.dart#L1069)，即可輕鬆防止空值可能引發的應用崩潰。

### **在預設不可空的前提下工作**

[啟用空安全](https://dart.tw.gh.miniasp.com/null-safety#enable-null-safety) 後，宣告變數的基礎方法會發生變化，因為預設型別不可為空:

```
// 在空安全的 Dart 中，以下均不可為空
var i = 42; // Inferred to be an int.
String name = getFileName();
final b = Foo();
```

如果您想要建立可能同時包含值或 null 的變數，則需要在宣告變數時在型別後面顯式新增 ? 字尾:

```
// aNullableInt 可以為整型或 null
int? aNullableInt = null;
```

空安全的實現很穩健，並提供豐富的靜態流程分析，方便開發者輕鬆處理可空型別。例如，區域變數在進行空值檢查後，Dart 會將其型別從可空提升為非空:

```
int definitelyInt(int? aNullableInt) {
 if (aNullableInt == null) {
   return 0;
 }
 // aNullableInt 現在會被提示為非空 int
 return aNullableInt;
}
```

我們還添加了一個新的關鍵字，required。當一個命名的引數被標記為 required (在 Flutter widget API 中經常出現)，而呼叫者忘記提供該引數時，就會發生如下分析錯誤:

![](https://devrel.andfun.cn/devrel/posts/2021/03/17e1a588495f7.png)

### **漸進遷移至空安全**

空安全對於我們的型別系統而言是一項根本性的改變，因此如果我們執意強制所有開發者採用，勢必會造成嚴重的混亂。因此，我們想讓**您自行決定**合適的遷移時機，空安全將是一項可選特性: 在做好準備之前，您可以在無需強制啟用空安全的情況下使用 Dart 2.12。您甚至可以在尚未啟用空安全的應用或 package 中依賴已啟用空安全的 package。

為了幫助您將現有程式碼遷移至空安全，我們提供了遷移工具和 [遷移指南](https://dart.tw.gh.miniasp.com/null-safety/migration-guide)。該工具會首先分析您所有的程式碼，然後您可以互動式地檢視工具推斷出的可空屬性，如果您不同意工具得出的結論，則可以新增可空性提示以更改推斷。新增遷移提示可能會大幅提升遷移品質。

![](https://devrel.andfun.cn/devrel/posts/2021/03/e4f5172c2403a.png)

目前，在預設情況下，使用 [dart create](https://dart.tw.gh.miniasp.com/tools/dart-tool) 和 [flutter create](https://flutter.tw/reference/flutter-cli) 新建立的 package 和應用中不會啟用健全空安全。在大部分生態系統完成遷移後，我們預計將在後續的穩定版本中預設啟用。您可以透過 `dart migrate` 在新建立的 package 或應用中輕鬆 [啟用空安全](https://dart.tw.gh.miniasp.com/null-safety#create)。

### **Dart 生態系統的空安全遷移狀態**

去年，我們提供了健全空安全的數個預覽版和 Beta 版，旨在為生態系統提供首批支援空安全的 package。這項工作非常重要，我們建議大家 [有序遷移至健全空安全](https://dart.tw.gh.miniasp.com/null-safety/migration-guide#step1-wait)，也就是說，在所有依賴項遷移完成之前，最好不要遷移自己的 package 或應用。

我們已釋出由 [Dart](https://pub.dev/packages?q=publisher%3Adart.dev&sort=popularity&null-safe=1)、[Flutter](https://pub.dev/packages?q=publisher%3Aflutter.dev&sort=popularity&null-safe=1)、[Firebase](https://pub.dev/packages?q=publisher%3Afirebase.google.com&sort=popularity&null-safe=1) 和 [Material](https://pub.dev/packages?q=publisher%3Amaterial.io&sort=popularity&null-safe=1) 團隊所提供的數百個 package 的空安全版本。令人驚喜的是，Dart 和 Flutter 生態系統對此也予以巨大的支援，pub.dev 現在共有 1,000 多個 package 支援空安全。而且重要的是，最受歡迎的 package 已率先完成遷移，截止到 Dart 2.12 釋出時，前 100 個最受歡迎的 package 中已有 98 個支援空安全，而在前 250 和前 500 的 package 中，支援空安全的比例則為 78% 和 57%。我們希望在接下來的幾周，pub.dev 上能夠出現更多支援空安全的 package。[我們的分析](https://github.com/dart-lang/sdk/wiki/Null-safety-migration-status) 表明，pub.dev 上的絕大多數 package 已經可以 [開始遷移](https://dart.tw.gh.miniasp.com/null-safety/migration-guide)。

### **充分健全的空安全的優勢**

完成遷移後，您的專案就處於健全的空安全模式下了。這意味著 Dart 能夠完全確保具有不可空型別的表示式不為空。當 Dart 分析完您的程式碼並確定某個變數不可為空時，該變數將始終不可為空。Dart 與 Swift 都擁有健全的空安全，但有些程式語言在這方面仍有待改進。

Dart 的健全空安全還暗含另一項備受期待的優勢: 您的程式可以更小、更快。由於 Dart 能夠確保不可為空的變數絕不為空，因此可以 [實現最佳化](https://medium.com/dartlang/dart-and-the-performance-benefits-of-sound-types-6ceedd5b6cdc)。例如，Dart 的執行前 (ahead-of-time, AOT) 編譯器可以產生更小更快的原生程式碼，因為當其知道變數不為空時，便不再需要新增空值檢查了。

## **Dart FFI: 整合 Dart 與 C 語言程式碼庫**

您可以透過 Dart FFI 呼叫 C 語言編寫的既有程式碼庫，從而增強可移植性，還可以透過精心打磨的 C 程式碼完成對效能要求極為嚴苛的任務。從 Dart 2.12 起，[Dart FFI](https://dart.tw.gh.miniasp.com/guides/libraries/c-interop) 已結束 Beta 測試階段，現已進入穩定狀態，可以用於生產環境。我們還添加了一些新功能，包括巢狀(Nesting)結構和按值傳遞結構。

### **按值傳遞結構**

在 C 語言中，結構可透過參考和值進行傳遞。FFI 以前僅支援按參考傳遞結構，但從 Dart 2.12 開始，也支援按值傳遞。下方的簡單範例中，兩個 C 函式使用參考和值完成傳遞:

```
struct Link {
  double value;
  Link* next;
};

void MoveByReference(Link* link) {
  link->value = link->value + 10.0;
}

Coord MoveByValue(Link link) {
  link.value = link.value + 10.0;
  return link;
}
```

### **巢狀(Nesting)結構**

C API 通常使用巢狀(Nesting)結構，這種結構本身也包含結構，比如以下範例:

```
struct Wheel {
 int spokes;
};

struct Bike {
 struct Wheel front;
 struct Wheel rear;
 int buildYear;
};
```

從 Dart 2.12 起，FFI 將支援巢狀(Nesting)結構。

### **API 改動**

作為 FFI 穩定版釋出內容的一部分，並且為了支援上述功能，我們做了一些小幅的 API 改動。

現在不允許建立空結構 (重要改動參照 [#44622](https://github.com/dart-lang/sdk/issues/44622))，並會給出棄用警告。您可以使用一個新的型別 Opaque 來表示空結構。dart:ffi 函式 sizeOf、elementAt 和 ref 現在需要編譯時的型別引數 (重要改動參照 [#44621](https://github.com/dart-lang/sdk/issues/44621))。因為在 package:ffi 中增加了新的便利函式，所以在常見的情況下，無需額外新增關於分配和釋放記憶體的範本程式碼:

```
// 分配一個 Utf8 陣列，使用 Dart 字串填充，然後傳遞給 C 方法並轉換結果，最後釋放 arg
//
// API 變更前：
final pointer = allocate<Int8>(count: 10);
free(pointer);
final arg = Utf8.toUtf8('Michael');
var result = helloWorldInC(arg);
print(Utf8.fromUtf8(result);
free(arg);
// API 變更後：
final pointer = calloc<Int8>(10);
calloc.free(pointer);
final arg = 'Michael'.toNativeUtf8();
var result = helloWorldInC(arg);
print(result.toDartString);
calloc.free(arg);
```

### **自動產生 FFI 繫結**

對於大型的 API 介面，編寫與 C 程式碼整合的 Dart 繫結極其耗時。為減輕這一負擔，我們為大家準備了繫結產生器，可以透過 C 標頭檔案自動建立 FFI 封裝程式碼，[歡迎試用](https://pub.dev/packages/ffigen)。

### **FFI 路線圖**

核心 FFI 平台完成後，我們的工作重心將轉向基於核心平台擴充 FFI 功能集。我們正在研究的一些功能包括:

* ABI 特定資料型別，如 int、long、size_t ([#36140](https://github.com/dart-lang/sdk/issues/36140))
* 結構中的內聯陣列 ([#35763](https://github.com/dart-lang/sdk/issues/35763))
* Packed 結構 ([#38158](https://github.com/dart-lang/sdk/issues/38158))
* 聯合型別 ([#38491](https://github.com/dart-lang/sdk/issues/38491))
* 對 Dart 開放終結方法 (finalizer) ([#35770](https://github.com/dart-lang/sdk/issues/35770)，請注意，您現在可以透過 C 語言使用終結方法)

### **FFI 使用範例**

在過去的幾個月中，我們看到大家在使用 Dart FFI 整合一系列基於 C 語言的 API 時，發掘出了許多有創意的用法。下面介紹幾個範例:

* [open_file](https://pub.dev/packages/open_file) 是一個用於在多個平台開啟檔案的 API，使用 FFI 在 Windows、macOS 和 Linux 上呼叫作業系統原生 API。https://pub.dev/packages/open_file
* [win32](https://pub.dev/packages/win32) 封裝了最常用的 Win32 API，便於從 Dart 直接呼叫各種 Windows API。
* [objectbox](https://pub.dev/packages/objectbox) 是一個快速資料庫，底層由 C 語言實現。
* [tflite_flutter](https://pub.dev/packages/tflite_flutter) 使用 FFI 封裝了 TensorFlow Lite API。

## **Dart 語言的未來計劃**

健全空安全是這幾年我們對 Dart 語言做出的最大改變。接下來，我們將繼續穩步改進 Dart 語言和平台。下面簡單介紹一些我們在 [語言設計規劃](https://github.com/dart-lang/language/projects/1) 中實驗的內容:

* **類型別名** ([#65](https://github.com/dart-lang/language/issues/65)): 將建立類型別名的功能擴充到非函式型別。例如，您可以建立一個 typedef 並將其用作變數型別:

```
typedef IntList = List<int>;
IntList il = [1,2,3];
```

* **無符號右移運算子** ([#120](https://github.com/dart-lang/language/issues/120)): 新增新的 >>> 運算子，便於對整數執行無符號移位操作。此運算子可完全重寫。

* **通用元資料註解** ([#1297](https://github.com/dart-lang/language/issues/1297)): 擴充元資料註解，以同時支援含型別引數的註解。

* **靜態超程式設計** ([#1482](https://github.com/dart-lang/language/issues/1482)): 支援靜態超程式設計，即編譯期間產生新 Dart 原始碼的 Dart 程式，與 Rust 宏 ([macro](https://doc.rust-lang.org/book/ch19-06-macros.html)) 和 Swift 函式建構器 ([function builder](https://github.com/apple/swift-evolution/blob/9992cf3c11c2d5e0ea20bee98657d93902d5b174/proposals/XXXX-function-builders.md)) 類似。該功能目前仍處於早期探索階段，但我們認為它可能會開啟全新使用案例的大門，打破現在依賴程式碼產生器的僵局。

## **Dart 2.12 現已釋出**

歡迎大家下載 [Dart 2.12](https://dart.tw.gh.miniasp.com/get-dart) 和 [Flutter 2.0](https://flutter.tw/get-started/) SDK，即刻開始使用 Dart 2.12，盡情體驗健全空安全和穩定版 FFI。請大家閱覽 [Dart](https://dart.tw.gh.miniasp.com/null-safety#known-issues) 和 [Flutter](https://flutter.tw/null-safety#known-issues) 的已知空安全問題。如果您發現其他任何問題，請在 [Dart 問題追蹤頁](https://github.com/dart-lang/sdk/issues) 中報告給我們。

如果您已在 [pub.dev](https://pub.dev/) 上釋出了 package，請立即參閱 [遷移指南](https://dart.tw.gh.miniasp.com/null-safety/migration-guide)，瞭解如何遷移至健全空安全。遷移有助於依賴您的 package 的其他 package 和應用完成遷移。我們在此向已經完成遷移的開發者們表示感謝！

歡迎大家與我們分享自己的健全空安全和 FFI 體驗，我們評論區見！