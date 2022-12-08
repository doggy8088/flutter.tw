---
title: Flutter 1.22 釋出 — 支援 iOS 14 和 Android 11，以及更多新功能
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/05/1s8wc0.jpg)

*作者 / Chris Sells, Product Manager, Flutter developer experience*

我們很高興推出支援 iOS 14 和 Android 11 的最新版 Flutter。Flutter 1.22 基於之前的版本更上一層樓，繼續助力開發者基於一個程式碼庫為多個平台打造快速、美觀的使用者體驗。我們的季度穩定版本中囊括了最新功能、效能改進和問題修復，且適用於多樣的生產環境。

由於本季度是新版移動作業系統的釋出季，因此這一版本側重於確保 Flutter 能夠有效支援 Android 11 和 iOS 14。針對這兩種作業系統的更新涉及大量的底層工作，以符合最新版 SDK 的規範，並確保所有功能都能透過我們廣泛的測試套件中的測試。針對 iOS 14，此版本包含了對新版 Xcode 12 和新圖示的支援，以及對 iOS 14 的新功能輕 App (App Clips) 的預覽支援。針對 Android 11，此更新支援新的螢幕型別 (如挖孔屏和瀑布屏)，以及在調出軟鍵盤時可以提供更流暢的動畫效果。

此版本的釋出距我們 1.20 版本的釋出僅有兩個月時間，因此釋出間隔比大多數版本都短。但在如此有限的時間內，我們解決了 3,024 個議題 (issue)，並且合併了來自 197 位貢獻者的 1,944 個 PR。其中有 114 位 (58%) 來自社群，並貢獻了 271 個 PR。[a14n](https://github.com/a14n) 是最突出的個人貢獻者，他再次以 20 個 PR 登上我們的傑出貢獻者名單榜首，他貢獻的大多數 PR 旨在支援 Flutter 中的空安全 (敬請期待更多即將釋出的更新)。

除了對新版移動作業系統的支援以外，我們還有很多其他訊息要與您分享，包括針對 Android 呼聲最高的功能之一 "狀態恢復" 的預覽、一整套新的 Material 按鈕、可與熱重載配合使用的全新國際化與本地化支援、新版 Navigator、穩定版 Platform Views (Google Maps 和 WebView 外掛的基礎)，以及可以在程式碼中設定的開關，用以改善高重新整理率顯示屏裝置上的滾動效果。我們還提供了一款新工具，可用於剖析應用大小並確保您所建構的外掛僅支援您要支援的平台。

## **支援 iOS 14**

每當廠商釋出新版移動作業系統時，我們都會對其進行徹底測試，以查詢會影響 Flutter 及其工具的相容性問題或變更。

為了適配 iOS 14，我們對 Flutter 進行了多項更改，以確保其功能可以滿足開發者的需求:

* Xcode 12 需要 iOS 9.0 或更高版本，因此我們的預設範本的預設系統版本從 8.0 提高到 9.0
* iOS 14 帶來的崩潰和字型渲染問題已在 Flutter 1.22 中得到修復
* 自 Flutter 1.20.4 起，部署到物理裝置的問題已得到解決
* 應用存取剪貼簿時會顯示相關使用通知，這項新策略在 Flutter 應用中曾引發虛假通知，該問題已在 Flutter 1.20.4 中得到修復
* 禁止在 iOS 14 裝置上執行除錯應用，但在除錯過程中執行應用除外
* 針對本地除錯的 Flutter 應用，有關網路安全的新策略會使 iOS 14 顯示一次性確認對話方塊 (僅針對開發中的應用，不適用於已釋出的 Flutter 應用)

綜上所述，要使您的 Flutter 應用適配 iOS 14，我們強烈建議您立即使用 Flutter 1.22 重新建構應用，然後將其釋出至 App Store，以確保您的 iOS 14 使用者獲得最佳體驗。

有關 Flutter 適配 iOS 14 的更多詳細資訊，包括一些 Add-to-App (將 Flutter 整合至現有應用)、深連結和通知考量因素，請參閱 [Flutter 的 iOS 14 文件](https://flutter.tw/development/ios-14)。

希望我們在工具和 SDK 支援方面所做的工作能夠幫助您專注於最關鍵的任務: 充分利用 iOS 14 的新特性。

其中一項特性就是更新了對 iOS 新版 SF Symbols 字型的支援，為此，我們也相應地更新了 [cupertino_icon package](https://pub.dev/packages/cupertino_icons)。將 cupertino_icons 依賴項更新到新的 1.0 主版本後，現有的 CupertinoIcons 呼叫將自動對映為新樣式。如果將 cupertino_icons 1.0 與 Flutter 1.22 結合使用，那麼透過 [CupertinoIcons API](https://api.flutter.dev/flutter/cupertino/CupertinoIcons-class.html)，您將能夠獲得約 900 個新圖示。

![](https://devrel.andfun.cn/devrel/posts/2021/05/xYace4.png)

您可以在 [cupertino_icons](https://flutter.github.io/cupertino_icons/) 預覽頁面中檢視完整的圖示列表，也可以參閱 flutter.dev 上的 [遷移詳情頁](https://flutter.tw/release/breaking-changes/cupertino-icons-1.0.0)。

針對 iOS 14，您可以使用 Flutter 嘗試的另一項功能是 [App Clips](https://developer.apple.com/app-clips/)，它是 iOS 14 的一項新增功能，支援以快速、免安裝的方式執行 10MB 以下的輕量版本應用。在 Flutter 1.22 版中，我們提供了使用 Flutter 建構輕 App 的功能預覽。

![](https://devrel.andfun.cn/devrel/posts/2021/05/ZHcXNl.png)

> △ 使用 Flutter 建構的輕 App 體驗

有關如何使用 Flutter 建構輕 App 的更多詳細資訊，請參閱技 [術文件](https://flutter.tw/development/platform-integration/ios-app-clip)。您也可以參考這一簡單的 [範例專案](https://github.com/flutter/samples/tree/master/ios_app_clip)。

## **Android 11**

此版本 Flutter 的釋出與 [Android 11 的釋出](https://zhuanlan.zhihu.com/p/228405882) 時間也剛好契合。我們更新了 Flutter 框架和引擎，以支援最新版 Android 系統中引入的兩項新特性。

第一，Flutter 現在可以提供安全邊襯區，以支援 Android 裝置的異形螢幕，包括劉海屏、挖孔屏和瀑布屏。

![](https://devrel.andfun.cn/devrel/posts/2021/05/opMphv.png)

透過使用 [MediaQuery](https://api.flutter.dev/flutter/widgets/MediaQuery-class.html) 和 [SafeArea](https://api.flutter.dev/flutter/widgets/SafeArea-class.html) API，您可以確保將活動的介面和互動元素放置在裝置顯示屏的無遮擋區域內。另外，您需要避免在瀑布屏邊緣區域內設定手勢檢測，從而避免誤觸問題。

第二，Flutter 應用內顯示軟體鍵盤時的動畫已經可以與 Android 11 實現同步。

![](https://devrel.andfun.cn/devrel/posts/2021/05/uLpxxk.gif)

> △ 請注意左側懸浮操作按鈕 (FAB) 的同步位移

Issue [#19279](https://github.com/flutter/flutter/issues/19279) 由來己久，即系統鍵盤的顯示/隱藏動畫與 Flutter 的邊襯區一直無法同步。這一問題在 Android 11 中已得到解決。

這裡做一則關於 Android 嵌入 API 的說明。去年，隨著 Flutter 1.12 版的釋出，我們推出了一套適用於 Android 系統的新版 Flutter 引擎和 Flutter 外掛 API。我們打造了這些 API 的 v2 版本，以更好地支援 Android 平台的 Add-to-App 使用者。一年後，我們有超過 80% 的 Android 外掛使用了新版 Android API。自 1.22 起，我們將棄用較舊的 v1 版本 API。

如果您仍在使用 Android v1 API，那麼將存在如下問題:

* 新建立的外掛將不再適配 v1 API
* Flutter 工具的 —no-enable-android-embedding-v2 配置標記已移除，現已作為預設行為
* 仍在使用 v1 API 的舊版應用在建構時會顯示棄用警告，並提供 [支援新版 Android 外掛 API 的文件](https://flutter.tw/development/packages-and-plugins/plugin-api-migration) 入口。

同時，如果您仍有基於 v1 Android API 的 Flutter 應用，該應用仍然可以執行。但是，您可能會逐漸遇到僅適用於 v2 API 的新版外掛，v1 Android API 則無法使用。有關更多詳細資訊，請參閱 [重大變更文件](https://flutter.tw/release/breaking-changes/android-v1-embedding-create-deprecation)。

## **更加豐富的按鈕樣式**

![](https://flutter.tw/release/breaking-changes/android-v1-embedding-create-deprecation)

> △ 一整套新的 Material Design 按鈕

現有的 Flutter 按鈕雖然美觀，但 [並不易用](https://files.flutter-io.cn/flutter-design-docs/material-button-system-updates.pdf)，尤其是在需要自訂主題時。此外，Material 規範也已經擴充包含了許多全新樣式的按鈕。

為使 Flutter 緊隨 Material 指南的變更步伐，我們很高興地宣佈 Flutter 1.22 中加入了一整套全新的按鈕。

相比就地嘗試和改進現有的按鈕類及其主題，[這個 PR](https://github.com/flutter/flutter/pull/59702) 引入了新的替換按鈕 widget 和主題。這項提議不但使我們免於因維護既有類而踏入向後相容性的迷宮，還使 Flutter 遵守了 [Material Design 指南](https://material.io/components/buttons/) 中對按鈕元件的全新命名規則。

![](https://devrel.andfun.cn/devrel/posts/2021/05/Hm9x4W.png)

新主題遵循 Flutter 最近對新 Material widget 採用的 "規範化" 模式。如果您想透過範例來上手體驗，可以參見 DartPad 中的這個絕佳 [範例](https://dartpad.cn/e560e1c2e4455ad53aac245079ccdcf2)。這並不屬於重大變更，因為 FlatButton、OutlineButton、RaisedButton、ButtonBar、ButtonBarTheme 和 ButtonTheme的語義並未發生改變。您可以根據喜好將舊按鈕與新按鈕混合搭配使用。

## **新的國際化和本地化支援**

自發布以來，Flutter 就提供了對應用進行國際化 (i18n) 和本地化 (l10n) 所需的核心功能。但在最新版本中，我們將推薦的最佳做法直接整合進了工具當中，甚至實現了熱重載支援，可以在您新增新的本地化資訊時更新您的應用。

![](https://devrel.andfun.cn/devrel/posts/2021/05/8QezMC.gif)

如果您想了解有關 Flutter 本地化支援的更多詳細資訊，包括本地化的訊息，以及含引數、日期、數字和貨幣的訊息，請閱讀 Flutter [國際化使用者指南](https://files.flutter-io.cn/flutter-design-docs/i18n-user-guide.pdf)。

此外，如果您對國際化和本地化感興趣，那麼您可能同樣有興趣瞭解舊的純文字 ASCII 無法支援的字串，例如 Unicode 和表情符號 (emoji)。最近，Dart 團隊釋出了 characters package，可以幫助開發者處理 Unicode (擴充) 字形集。這個 package 可解決諸如如何將類似於 "A 🇬🇧 text in English" 的字串正確地縮短至前 15 個字元一類別的問題。使用 String 類時，縮短的結果是 "A 🇬🇧 text in"，僅含 12 個使用者可感知字元。而使用 [characters package](https://pub.dev/packages/characters) 則會得到正確的結果: "A 🇬🇧 text in Eng"。

基於 [PR59267](https://github.com/flutter/flutter/pull/59267)，Flutter 將使用 characters package 來正確處理這些複雜字元。例如，當使用具有 maxLength 限制的 TextField 時，諸如 👨‍👩‍👦 ‍‍ 的字元現在可以正確地計為單個字元。另外，基於 [PR59620](https://github.com/flutter/flutter/pull/59620)，characters package 在 Flutter 所在的專案中均自動可用，無需手動新增。希望這種方案可以使處理來自所有語言區域的各種字串變得更加容易。有關 characters package 的更多詳細資訊，請參閱《[如何正確處理 Dart 字串](https://medium.com/dartlang/dart-string-manipulation-done-right-5abd0668ba3e)》。

## **可正式使用的 Google Maps 和 WebView 外掛**

Flutter 團隊在將某項功能標記為 "生產就緒" 前會非常謹慎，並親自完成全面的測試。對於 [google_maps_flutter](https://pub.dev/packages/google_maps_flutter) 和 [webview_flutter](https://pub.dev/packages/webview_flutter) 外掛而言，決斷因素一直是底層的 [Platform Views](http://flutter.tw/development/platform-integration/platform-views) 實現，該實現支援將 Android 和 iOS 系統的原生介面元件託管在 Flutter 應用中。我們很高興地宣佈，我們在此版本的 Flutter 中對框架的基礎進行了充分強化，足以將這兩個外掛標記為 "生產就緒"。

![](https://devrel.andfun.cn/devrel/posts/2021/05/8kusLr.gif)

> △ 託管 flutter.dev 的 webview_flutter 外掛

在 Flutter 1.22 中，我們新增了備選的 Platform Views 實現，修復了 [Android 檢視所有已知的鍵盤和無障礙問題](https://github.com/flutter/flutter/issues/61133)。此外，該版本還支援 Android API 19 及更高級別 (此前曾要求 API 級別 20)。我們還在 iOS 上進行了執行緒改進，使 Platform Views 更為高效可靠 (並且您不再需要將 `io.flutter.embedded_views_preview` 標記新增到 iOS Info.plist 中)。

[webview_flutter](https://pub.dev/packages/webview_flutter) 外掛支援新的 Android Platform Views 模式，但目前需要 [手動啟用](https://github.com/flutter/plugins/blob/master/packages/webview_flutter/README.md#android)。當該功能在更廣泛的社群中得到更多使用後，我們會在未來版本中預設啟用它。

Google Maps 和 WebView 外掛已經從 Platform Views 的改進中受益。如果您想使用 Platform Views 來託管 iOS 或 Android 中的原生介面元件，可以前往 [這裡](https://flutter.tw/development/platform-integration/platform-views) 瞭解相關方法。

## **Navigator 2.0**

如果您曾在 Flutter 應用中使用過 [導航](https://flutter.tw/development/ui/navigation)，那麼您可能已經注意到核心資料結構 (即使用者所瀏覽的頁面堆疊) 是對您隱藏的。要對其進行管理，您需呼叫 Navigator.pop() 或 Navigator.push()。舉例來說，假設您想在首頁上顯示一系列 widget，使用者點按一個 widget 後轉到呈現該顏色的詳細資訊頁面 (如下圖所示)。

![](https://devrel.andfun.cn/devrel/posts/2021/05/XmAMxs.png)

這兩個螢幕可以採用如下方式實現:

```Dart
class ColorListScreen extends StatelessWidget {
 final List<Color> colors;
 final void Function(Color color) onTapped;
 ColorListScreen({this.colors, this.onTapped});
 
 @override
 Widget build(BuildContext context) => Scaffold(
       appBar: AppBar(title: Text('Colors')),
       body: Column(
         children: [
           // you can see and decide on every color in this list
           for (final color in colors)
             Expanded(
               child: GestureDetector(
                 child: Container(color: color),
                 onTap: () => onTapped(color),
               ),
             )
         ],
       ),
     );
}
 
class ColorScreen extends StatelessWidget {
 final Color color;
 const ColorScreen({this.color});
 
 @override
 Widget build(BuildContext context) => Scaffold(
       appBar: AppBar(title: Text('Color')),
       body: Container(color: color),
     );
}
```

使用最簡單的 Navigator 1.0 做法，即可透過看起來非常簡單的方式在這兩個螢幕之間導航:

```Dart
class _ColorAppState extends State<ColorApp> {
 List<Color> _colors = [Colors.red, Colors.green, Colors.blue];
 
 @override
 Widget build(BuildContext context) => MaterialApp(
       title: 'Color App',
       home: Builder(
         builder: (context) => ColorListScreen(
           colors: _colors,
           // the Navigator manages the list of pages itself; you can only push and pop
           onTapped: (color) => Navigator.push(
             context,
             MaterialPageRoute(builder: (context) => ColorScreen(color: color)),
           ),
         ),
       ),
     );
}
```

只需呼叫 `Navigator.push()` 即可將某一頁面推入首頁的上方，從而建立包含兩個頁面的堆疊。但與透過 `ColorListScreen` 的 build 方法建立的一系列 `Containers` 不同的是，該堆疊將對您隱藏。而且由於堆疊被隱藏，在其他場景中將難以對其進行管理，例如處理由原生嵌入、網址或 Android intent 等提供的初始路由中包含的深連結。管理同一頁面的不同排列形成的巢狀(Nesting)路由時也十分困難。

透過使頁面堆疊可見，Navigator 2.0 有效解決了上述乃至更多問題。以下是在相同的 `ColorListScreen` 和 `ColorScreen` 之間導航的新範例:

```
class _ColorAppState extends State<ColorApp> {
 Color _selectedColor;
 List<Color> _colors = [Colors.red, Colors.green, Colors.blue];
 
 @override
 Widget build(BuildContext context) => MaterialApp(
       title: 'Color App',
       home: Navigator(
           // you can see and decide on every page in this list
         pages: [
           MaterialPage(
             child: ColorListScreen(
               colors: _colors,
               onTapped: (color) => setState(() => _selectedColor = color),
             ),
           ),
           if (_selectedColor != null) MaterialPage(child: ColorScreen(color: _selectedColor)),
         ],
         onPopPage: (route, result) {
           if (!route.didPop(result)) return false;
           setState(() => _selectedColor = null);
           return true;
         },
       ),
     );
}
```

該應用顯式建立了 `Navigator`，併為其提供了代表完整堆疊的頁面列表。我們建立一個空的 `_selectedColor` 來表示尚未選擇任何顏色，因此我們最初不顯示 `ColorScreen`。當用戶選擇一種顏色時，我們將照常呼叫 s`etState()` 以向 Flutter 指示您需要再次呼叫 `build()` 方法，進而建立頂部為 `ColorScreen` 的堆疊。

您可以在 `OnPopPage` 回呼(Callback)中更新狀態，例如，如果使用者執行了 pop (彈出棧頂元素) 操作，則表示他們已 "取消選擇" 當前顏色，因此我們不再需要顯示該頁面。

Navigator 2.0 看起來與 Flutter 的其餘部分相似，這種設計是有意為之——此版本為宣告式導航，而不同於 Navigator 1.0 版本的命令式導航。這種設計旨在統一導航與 Flutter 的其餘部分之間的模型，以便在解決問題的同時新增新的功能。實際上，本例較為簡略，只能對 Navigator 2.0 提供粗淺的說明。如需瞭解詳細資訊，強烈推薦您參閱有關 Flutter 中的宣告式導航和路由的這篇 [文章](https://medium.com/flutter/learning-flutters-new-navigation-and-routing-system-7c9068155ade)。

另外，您對 Navigator 1.0 的現有使用仍將有效，在短期內不會被移除。如果您喜歡該模型，則可以一如既往地使用該版本。但如果您試用了 Navigator 2.0，我們相信您會喜歡上這一全新版本。

## **預覽: 適用於 Android 的狀態恢復**

此版本支援 [Android 的狀態恢復](https://developer.android.google.cn/topic/libraries/architecture/saving-states) (State Restoration)，歡迎大家試用這一新增功能。這是我們的 [熱門功能之一](https://github.com/flutter/flutter/issues/6827)，獲得了 217 次點贊！

對於不熟悉狀態恢復需求的使用者: 移動作業系統可能會終止後臺應用以回收資源供前臺應用使用。發生這種情況時，作業系統會通知該應用即將被終止以便其快速儲存介面狀態，從而在使用者切換回該應用時能夠恢復。正確實現該操作後，既能為使用者提供無縫體驗，也可以更好地利用裝置資源。Flutter 此前尚不支援狀態恢復，因為在不具備框架支援的情況下很難正確地實現狀態恢復。正因如此，我們很高興能夠在 Android 平臺中提供此功能的基礎實現。

下面是一個恢復 Flutter Counter 應用狀態的非常簡單的 [範例](https://api.flutter.dev/flutter/widgets/RestorationMixin-mixin.html#widgets.RestorationMixin.1):

```
class CounterState extends State<RestorableCounter> with RestorationMixin {
    @override
    String get restorationId => widget.restorationId;

    RestorableInt _counter = RestorableInt(0);

    @override
    void restoreState(RestorationBucket oldBucket) => registerForRestoration(_counter, 'count');

    void _incrementCounter() => setState(() => _counter.value++);

    @override
    Widget build(BuildContext context) => Scaffold(
        body: Center(child: Text('${_counter.value}')),
        floatingActionButton: FloatingActionButton(onPressed: _incrementCounter),
    );
}
```

簡而言之，每個 widget 都有一個儲存分割槽，該儲存分割槽使用唯一 ID 註冊到 [RestorationMixin](https://api.flutter.dev/flutter/widgets/RestorationMixin-mixin.html)。透過使用 [RestorableProperty](https://master-api.flutter.dev/flutter/widgets/RestorableProperty-class.html) 型別 (如此例使用的 RestorableInt) 儲存特定於介面的資料，並向狀態恢復功能註冊該資料，即可在 Android 終止應用前自動儲存資料，並在應用恢復執行時恢復資料，就是這麼簡單。所有以 Restoration* 型別儲存的資料 (如 RestorableInt、RestorableString 和 RestorableTextEditingController，不勝列舉) 都能被恢復。而且，如果我們未能覆蓋您想要恢復的型別，您也可以透過擴充 [RestorableProperty<T>](https://api.flutter.dev/flutter/widgets/RestorableProperty-class.html) 來建立自己的型別。

![](https://devrel.andfun.cn/devrel/posts/2021/05/ZUSUID.png)

為了自動測試狀態恢復，我們 [為 WidgetTester 添加了一個新的 restartAndRestore API](https://api.flutter.dev/flutter/flutter_test/WidgetTester/restartAndRestore.html)。要進行手動測試，最簡單的方法是在 Android 裝置上啟動已啟用狀態恢復功能的 Flutter 應用，在 Android 的開發者設定中開啟 "Don’t keep activities"，執行 Flutter 應用並將其置於後臺，然後再切換回該應用。此時，Android 已終止並恢復您的應用，屆時您可以檢查是否一切正常。

雖然我們很高興為您提供狀態恢復的預覽版本，但還有更多工作待完成。例如，狀態恢復不僅適用於 Android，iOS 應用也將從中受益。此外，我們也在致力於更新 Flutter widget，以在恢復時保持其狀態。我們已經在 Scrollable 類中提供了支援，例如 ListView 和 SingleChildScrollView (用於記住使用者的滾動位置) 和 TextFields (用於恢復使用者輸入的文字)，並且我們計劃將這些支援擴充至其他 widget。

但是，我們目前尚未為導航 (包括 Navigator 1.0 和 2.0) 新增關鍵恢復支援，這也是我們暫時稱其為預覽版本的原因。這意味著應用暫時無法恢復使用者所在位置。該功能的測試版即將在 Flutter 的下一個穩定版本中正式釋出。

## **預覽: 在輸入頻率與顯示重新整理率不匹配情況下提供平滑的滾動效果**

Flutter 團隊與 Google 內部合作伙伴密切合作，在輸入頻率與顯示重新整理率不同的情況下顯著提高了滾動效能。例如，Pixel 4 輸入頻率為 120hz，而顯示屏重新整理率為 90hz。在滾動螢幕畫面時，這種不匹配會導致效能下降。使用新的 resamplingEnabled 標記，您可以利用我們在 Flutter 中實現的效能改進解決此問題:

```
void main() {
    GestureBinding.instance.resamplingEnabled = true;
    run(MyApp());
}
```

啟用此標記後，滾動螢幕畫面時的掉幀率最多可以降低 97%，具體取決於所涉及的頻率差。在確定這一配置能夠提供最佳體驗後，我們計劃在未來的版本中預設啟用此標記。

## **全新的統一 Dart 開發者工具**

與往常一樣，對 Flutter 的更新不僅限於引擎和框架，還包括工具方面的更新。Flutter 1.22 中包含新版 Dart (2.10)，以及非常實用的新 dart 命令列工具。

Dart 之前有許多小巧的開發者工具 (例如，用於格式化的 dartfmt 和用於程式碼分析的 `dartanalyzer`)。Dart 2.10 中新提供了統一的 dart 開發者工具，與 flutter 工具非常相似。

![](https://devrel.andfun.cn/devrel/posts/2021/05/ngscrA.png)

自新版 Flutter 1.22 SDK 起，<flutter-sdk>/bin 資料夾 (可能在您的 PATH 位置) 中將同時包含 flutter 和 dart 命令。有關更多詳細資訊，請參閱 [Dart 2.10](https://medium.com/dartlang/announcing-dart-2-10-350823952bd5) 的釋出文章。

## **應用體積分析工具**

Flutter 1.22 還帶來了新的輸出應用體積分析的實用工具。此工具可以幫助分析 Flutter 並判斷應用尺寸構成是否隨時間變化。

您可以透過將 `--analyze-size` 標記傳遞給以下任何命令來使用該工具收集分析所需的資料:

* flutter build apk

* flutter build appbundle

* flutter build ios

* flutter build linux

* flutter build macos

* flutter build windows

在建構 Flutter 輸出工件時使用此標記將列印工件大小及組成的摘要。其中包括原生程式碼、資源，甚至包括已編譯的 Dart 程式碼的軟體包級細分情況。

![](https://devrel.andfun.cn/devrel/posts/2021/05/C8RTHd.png)

> △ Flutter Gallery 釋出版本 APK 的細分範例

此摘要有助於快速識別應用軟體套件中的尺寸熱點。此外，收集到的資料還可作為 JSON 檔案用於 Dart DevTools，您可以按照 [Flutter 文件上的說明](https://flutter.tw/development/tools/devtools/app-size) 進一步瀏覽應用的內容、查明大小問題以及比較兩個不同 JSON 檔案之間的差異。載入 JSON 檔案後會出現一個介面，其中提供了應用尺寸情況的樹狀圖。

![](https://devrel.andfun.cn/devrel/posts/2021/05/GedErZ.png)

> △ Dart DevTools 中的 APK 細分範例

更多有關您可以使用應用尺寸工具執行的操作，請參閱文件: [使用應用尺寸工具](https://flutter.tw/development/tools/devtools/app-size)。

## **預覽: 在 DevTools 中更新了 Network 頁面**

此版本中的另一項 DevTools 預覽功能是，使用者能夠在 **Network** 標籤中檢視 HTTP 和 HTTPs 響應正文。

![](https://devrel.andfun.cn/devrel/posts/2021/05/xx7qFb.png)

要啟用此功能，請確保透過 flutter channel dev 和 flutter upgrade 命令進入 Flutter dev 渠道。

此外，對於網路流量較大的應用，我們提供了搜尋和篩選功能。

![](https://devrel.andfun.cn/devrel/posts/2021/05/10HBMU.png)

有關 **Network** 標籤的文件，請參閱 [使用網路檢視  (Network View)](https://flutter.tw/development/tools/devtools/network) 的說明。

## **IntelliJ 中託管的 DevTools Inspector 標籤**

一段時間以來，我們一直在維護著某些 Flutter 工具的兩個副本，例如 IntelliJ 中的 **Inspector** 窗格和 Dart DevTools 中的 **Inspector** 標籤。這不僅降低開發速度，因為我們必須維護兩個程式碼庫，也造成某些功能 (例如 Layout Explorer) 尚未能納入到 IntelliJ 外掛當中。因此，為了同時解決這兩個問題，我們現在提供直接在 IntelliJ 內部託管 Dart DevTools 的 Inspector 標籤的功能。

![](https://devrel.andfun.cn/devrel/posts/2021/05/AWpL8g.png)

請注意新出現的 Layout Explorer，您可以在程式碼旁使用該功能。要啟用此選項，請使用 **Preferences** > **Languages & Frameworks** > **Flutter** > **Enable embedded DevTools inspector**。

## **Visual Studio Code 中的輸出連結得到改進**

對於所有 Flutter 開發者而言，在終端或堆疊分析結果中檢視錯誤輸出並繼續解決問題都是一項必不可少的例行工作。在 Visual Studio Code 的最新版本 Flutter 擴充中，現已能夠正確解析這些連結，使您可以直接在輸出結果中使用它們。

![](https://devrel.andfun.cn/devrel/posts/2021/05/krDNo3.png)

這項功能看似微不足道，卻已獲得了非常積極的初步反饋。

與往常一樣，我們在工具方面的變更不勝列舉，謹建議您參閱以下發布內容來了解相關詳細資訊:

* [Dart DevTools - 0.9.0](https://groups.google.com/g/flutter-announce/c/UxMv8MzE_uo/m/ED539pi2AAAJ)
* [Dart DevTools - 0.9.1](https://groups.google.com/g/flutter-announce/c/y27h86ATFJM)
* [Dart DevTools - 0.9.3](https://groups.google.com/g/flutter-announce/c/24LppkXdMtM)
* [Flutter IntelliJ Plugin M48.1 版本](https://groups.google.com/g/flutter-announce/c/nvgDi3RLAUE/m/Fx4Ze0vrBAAJ)
* [Flutter IntelliJ Plugin M49 版本](https://groups.google.com/g/flutter-announce/c/-ZMKRIBRtGU)
* [Flutter IntelliJ Plugin M50 版本](https://groups.google.com/g/flutter-announce/c/u0zU6zv3o44/m/2y0JsX1_AwAJ)
* [VS Code 擴充程式 v3.14.0](https://groups.google.com/g/flutter-announce/c/8e8e-ZrgySY)
* [VS Code 擴充程式 v3.15.0](https://dartcode.org/releases/v3-15/)

## **精彩案例: EasyA**

[EasyA](https://easya.io/) 是一款訂閱應用，為適齡學生搭建了透過即時通訊與優秀教師溝通的平台，該應用使用 Flutter 編寫。這款應用近日被 Apple 推薦為 [每日精選應用](https://apps.apple.com/gb/story/id1527472788)。

![](https://devrel.andfun.cn/devrel/posts/2021/05/IvM3sF.png)

> 今年年初，學校紛紛啟動了線上課程，我們需要快速釋出這款教學應用來為學生提供幫助。Flutter 驚人的開發速度使我們能夠針對 iOS 和 Android 平台實現優秀的設計，而且還支援釋出到 web 平台，這有助於我們及時應對停課的局面！這在以前是無法想象的。由於 Flutter 支援同時針對所有三個平台進行開發，我們能夠高效地共享程式碼，並最為充分地利用我們的小型開發團隊。
>
>
>
>
>
> — Phil Kwok，EasyA 聯合創始人

## **重大變更**

與往常一樣，我們試圖將重大變更的數量降至最低。Flutter 1.22 版本的重大變更列表如下:

* [56413](https://github.com/flutter/flutter/pull/56413) - [如果指定的 Rect 已經可見，則阻止 viewport.showOnScreen 滾動視窗](https://docs.google.com/document/d/1BZhxy176uUnqOCnXdnHM1XetS9mw9WIyUAOE-dgVdUM/edit?usp=sharing)。
* [62395](https://github.com/flutter/flutter/pull/62395) - [gen_l10n] 預設生成合成軟體套件。
* [62588](https://github.com/flutter/flutter/pull/62588) - 進一步減少建構路線。

## **總結**

Flutter 1.22 穩定版在上一版本釋出之後很快問世，其中包含了諸多新特性，本篇文章未能一一列舉。我們希望此版本可以幫助您為 iOS 和 Android 開發出優秀的應用，我們對您將在應用商店中釋出的產品充滿期待！感謝您的支援——Flutter，為開發者而打造。
