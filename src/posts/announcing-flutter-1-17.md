---
title: Flutter 1.17 — 2020 首個穩定版釋出！
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/05/g6bFl3.jpg)

*作者 / Chris Sells, Product Manager, Flutter developer experience*

很高興為大家帶來 Flutter 1.17，這也是我們 2020 年的第一個穩定釋出版本。

今年對每個人來說都是充滿挑戰的一年。我們的目標是每季度釋出一次穩定版，由於我們一直在 [針對新的釋出流程調整基礎設施](https://flutter.cn/posts/flutter-spring-2020-update)，本次版本釋出有所推遲。然而品質依然是我們的第一要務，新的版本釋出流程將提升我們為穩定分支提供高品質熱修復的能力。本次釋出的版本包含大量的修復，自 1.12 穩定版釋出以來，我們已經解決了 6,339 個 Issue，這是史無前例的大進展。而這很大程度上要歸功於我們 [與 Nevercode 的合作](https://blog.codemagic.io/flutter-and-codemagic-join-forces-on-github/)，在使用者提出問題時我們可以更快地響應。截至現在，我們已經解決的 bug 數量超過新增數量，報錯總數減少了大約 800 個。這些 bug 中相當的一部分已透過合併 3,164 個 pull request 進行了修復，它們來自 231 位貢獻者。這些資料都令人振奮，我們也衷心感謝大家在這個特殊時期的勤奮工作和持續貢獻。

如果您想檢視我們在這個版本中合併的所有 pull request，請前往 flutter.cn 瞭解詳情。除了品質方面的改進，我們還設法在這個版本中加入了一些新功能，包括 iOS 系統中的 Metal 支援、全新的 Material 元件、全新的網路追蹤工具等等。這個版本還包括了 Dart 2.8，請閱讀今天釋出的二條文章詳細瞭解。

## **移動端效能和檔案體積最佳化**

效能和記憶體方面的整體最佳化是 1.17 版的重點之一。只需將您的工程升級到使用 Flutter 1.17，您的使用者就能體驗到更快速流暢的動畫、更小巧的應用尺寸，以及更低的記憶體佔用。在預設的導航場景下 (不包含透明圖層內容的導航路徑) 1.17 版的 [速度提升了 20%-37%](https://github.com/flutter/flutter/pull/48900)。簡單 iOS 動畫的 CPU/GPU 佔用可減少高達 40%，具體的減少量取決於硬體 (詳見 [PR 14104](https://github.com/flutter/engine/pull/14104) 和 [PR 13976](https://github.com/flutter/engine/pull/13976))。

得益於我們的多項修復工作，新版本還在應用體積上做出了可觀的改進。比如 Flutter Gallery 範例應用的 Android 版本在去年年底是 9.6MB，現在則是 8.1MB，體積減少了 18.5%。

* 我們做出的多項修復：

	* [https://github.com/dart-lang/sdk/commit/f56b0f690789b6f0e2e5bc1340abf4eba414b7a0](https://github.com/dart-lang/sdk/commit/f56b0f690789b6f0e2e5bc1340abf4eba414b7a0)
	* [https://github.com/dart-lang/sdk/commit/a2bb7301c5795e6b28089a8dc96e6ab5ca798e22](https://github.com/dart-lang/sdk/commit/a2bb7301c5795e6b28089a8dc96e6ab5ca798e22)
	* [https://github.com/dart-lang/sdk/commit/d77f4913a18ecce8c4be95cbaa4299ff1521dc10](https://github.com/dart-lang/sdk/commit/d77f4913a18ecce8c4be95cbaa4299ff1521dc10)
	* [https://github.com/dart-lang/sdk/commit/e2faac751e1ef3707730e6e48f4d8f22ecbf35c3](https://github.com/dart-lang/sdk/commit/e2faac751e1ef3707730e6e48f4d8f22ecbf35c3)
	* [https://github.com/dart-lang/sdk/commit/8e7ffafbafc8203361111ddcafe0e0fcc372edf8](https://github.com/dart-lang/sdk/commit/8e7ffafbafc8203361111ddcafe0e0fcc372edf8)

記憶體使用方面也有明顯的最佳化，例如在快速滾動大型圖片時 [記憶體佔用減少了 70%](https://github.com/flutter/engine/pull/14265)，進而提升效能，具體提升的程度取決於裝置記憶體的大小。

![](https://devrel.andfun.cn/devrel/posts/2021/05/lKp7yG.png)

> △ 測試應用的記憶體佔用量隨著 PR 合併的變化 (豎條越短記憶體佔用越少)

不過最大的效能提升來自在 iOS 系統中實現對 Metal 的支援。

## **Metal 將 iOS 效能提升 50%**

Apple 的 Metal API 使開發者幾乎直接存取底層 GPU，它也是 Apple 推薦使用的圖形 API。在支援 Metal 的 iOS 裝置上，Flutter 將預設使用 Metal，使得應用在絕大多數情況下都執行得更快，[渲染速度平均提升約 50%](https://github.com/flutter/flutter/issues/53768) (具體取決於裝置的工作負載)。

![](https://devrel.andfun.cn/devrel/posts/2021/05/rqHHKL.png)

> △ 測試 iOS 應用在 OpenGL 和 Metal 下的幀渲染時間 (值越低越好)

對於那些不完全支援 Metal 的裝置 (使用 A7 之前的處理器或 iOS 10 之前系統的裝置)，Flutter 會和以前一樣使用 OpenGL，在這些裝置上實現原生渲染速度。更多詳情，請檢視 Flutter wiki 上的 [iOS Metal 常見問題解答](https://github.com/flutter/flutter/wiki/Metal-on-iOS-FAQ)。

## **Material widgets: NavigationRail, DatePicker, 以及更多**

基於多方使用者的反饋，我們也在持續推進 Material 設計系統在 Flutter 中落地。在新版本中我們加入了 [NavigationRail](https://master-api.flutter-io.cn/flutter/material/NavigationRail-class.html)，這是一個新的 widget，提供了響應式的應用導航模型。它由 Google Material Design 團隊 [設計](https://material.io/components/navigation-rail) 並實現。NavigationRail 非常適合可以在移動和桌面裝置之間切換的應用，當您的應用所在的螢幕尺寸增大時，您可以非常容易地從 BottomNavigator 切換成 NavigationRail。

![](https://devrel.andfun.cn/devrel/posts/2021/05/10uOeq.gif)

> △ 新加入的 NavigationRail widget

您可以在 [web_dashboard 範例](https://github.com/flutter/samples/tree/master/experimental/web_dashboard) 或者 [DartPad](https://dartpad.cn/b9c6cd345fd1cff643353c1f4902f888) 中實際體驗 NavigationRail。

除了加入新的 widget 之外，新版本還更新了 Material [DatePicker](https://api.flutter.dev/flutter/material/showDatePicker.html) 以及修正了文字選擇選單 widget 的溢位顯示問題。

![](https://devrel.andfun.cn/devrel/posts/2021/05/B4GHia.gif)

> △ 新的 Material DatePicker widget

DatePicker 基於更新過的 [Material 設計指南](https://material.io/components/pickers/#mobile-pickers) 引入了新的視覺設計，並新增了文字輸入模式。詳情請閱讀 [Material DatePicker 改版文件](https://flutter.dev/go/material-date-picker-redesign)。

![](https://devrel.andfun.cn/devrel/posts/2021/05/TwiEKM.gif)

> △ Android 上新版文字選擇懸浮選單

![](https://devrel.andfun.cn/devrel/posts/2021/05/67UFM4.gif)

> △ iOS 上新版文字選擇懸浮選單

如上圖，當按鈕總長度超過了顯示範圍時，[Android](https://github.com/flutter/flutter/pull/49391) 和 [iOS](https://github.com/flutter/flutter/pull/54140) 中的文字選擇選單保真度也得到了提高。選單文字越長，這項更新越顯著。

另外，[全新的 Animations Package](https://pub.flutter-io.cn/packages/animations)，雖然並沒有被包含進 Flutter 1.17 版本中，但也已同期釋出。它實現了 [新的 Material 動效規範](https://material.io/design/motion/the-motion-system.html)。

![](https://devrel.andfun.cn/devrel/posts/2021/05/cDpbsU.gif)

> △ Animations Package 中的容器轉場動畫

在《[實現動效太難了？試試 Material Design》](https://mp.weixin.qq.com/s/v9QGn4xkEIlQ6DAe_1t7tg) 一文中，Material Design 團隊給出了元件與全屏檢視之間的四種轉場模式: 容器變換、共享軸、淡入淡出和彈出效果。雖然 Flutter 一直就可以實現這些動效，但 Animations Package 讓您可以更輕鬆地實現它們。今天就在應用裡試試這些動效，讓您的使用者們眼前一亮吧！

## **Material 文字縮放: 現代化 Flutter 文字主題**

在新版本中，Flutter 團隊在不破壞現有 Flutter 應用的同時，完成了對 2018 年 [Material Design 規範文字排版](https://material.io/design/typography/the-type-system.html#type-scale) 縮放 (Type Scale) 的實現。2018 年 10 月，我們在 [PR 22330](https://github.com/flutter/flutter/pull/22330) 中添加了對新配置 (而不是新名稱) 的可選支援。現有的文字樣式名稱沒有更改，因為這屬於重大的 API 更改，可能會影響到大多數應用。Flutter 1.17 更新了 TextTheme API，以遵循當前的 Material 規範，但保留了舊的名稱，從而不影響您的程式碼。由於舊的名稱已經被廢棄，您將收到 warning 提示，建議採用新的名稱。

2018 年 Material TextStyle 的名稱和配置彙總如下表所示。

![](https://devrel.andfun.cn/devrel/posts/2021/05/ICBLwp.png)

Material Design 規範中稱為 `body1` 和 `body2` 的 TextStyle 在 Flutter TextTheme API 中被稱為 `bodyText1` 和 `bodyText2`。同樣的，在規範中稱為 `H1-H6` 的 TextStyle，在 TextTheme API 中稱為 `headline1-headline6`。

## **在 Flutter 中使用 Google Fonts**

文字和字型總是密不可分，如果新的 Material 文字縮放實現讓您眼前一亮，那麼您可能也會對新的 [Google Fonts for Flutter v1.0](https://medium.com/flutter/introducing-google-fonts-for-flutter-v-1-0-0-c0e993617118) 頗感興趣。

![](https://devrel.andfun.cn/devrel/posts/2021/05/FJhCZx.gif)

> △ 在 Flutter 應用中輕鬆使用 Google Fonts

開發者可以透過 Google Fonts 在應用中輕鬆嘗試和使用 fonts.google.com 中的字型。當應用準備釋出時，開發者也可以決定是透過 API 來下載字型，還是在應用套件中直接嵌入字型。

## **無障礙功能和國際化**

最後是我們持續關注的課題——無障礙功能。我們認為，讓 Flutter 應用可以服務最為廣泛的受眾是我們的首要任務之一。在新版本中，我們做了全面的工作，對滾動、文字框以及其他輸入 widget 的無障礙功能進行了修復。GitHub 上有我們在這個版本中 [完成修復的無障礙功能完整列表](https://github.com/flutter/flutter)。我們希望開發者能多多測試自己應用的無障礙功能，並且隨著這次版本還發布了一些推薦的 [最佳實踐](https://flutter.dev/docs/development/accessibility-and-localization/accessibility) 供大家參考。

在國際化方面，我們一直在處理一些 [影響三星鍵盤 IME 的問題](https://github.com/flutter/flutter/issues/42273)，這影響了許多東亞語言的文字輸入。現在我們很高興地宣佈這個問題已經解決。各國開發者，尤其是韓國的開發者會在這個版本中發現不少喜人的變化。

## **工具: Dart DevTools 支援 Flutter**

將當前版本的 Dart DevTools 替換成新的 Flutter 版本，這一處理序透過 Flutter 1.17 已經接近完成。如果您想試試新的 Flutter 版 Dart DevTools，請在啟動 DevTools 後點擊右上角的 Beaker (燒瓶) 圖示。

![](https://devrel.andfun.cn/devrel/posts/2021/05/lbMHWp.png)

由 Flutter 實現的 Dart DevTools 預釋出版本帶來了諸多改進，其中全新的 **Network** (網路) 選項卡最為重要。

![](https://devrel.andfun.cn/devrel/posts/2021/05/tpb7Pb.png)

如果您在 Dart DevTools 的預釋出版本中沒有看到 Network 選項卡 (比如，您是透過命令列來使用 DevTools)，可以透過如下命令手動更新它:

```
$ pub global activate devtools
```

在按下 Record (錄製) 按鈕後， Network 選項卡會顯示您的 Flutter 應用的網路流量情況。如果您想在應用啟動時就立即開始監測網路流量，可以在您的 main() 方法中加入這行程式碼:

```
void main() {
    // enable network traffic logging
    HttpClient.enableTimelineLogging = true;
    runApp(MyApp());
}
```

除了 Dart DevTools 的更新之外，這個版本還實驗性地增加了 ["快速啟動" 選項](https://github.com/flutter/flutter/pull/46140)，當您除錯 Android 版 Flutter 應用時，其啟動速度可以提升高達 70%。您可以透過 flutter run --fast-start -d <your Android device> 來啟用這個選項。這個選項會安裝一個只依賴您的外掛程式碼的通用 Android 應用，而不包含任何 Dart 程式碼或資源。這會讓重複執行的 flutter run 命令更快地啟動，因為修改 Dart 程式碼或資源並不需要重新建構 APK。和通常的啟動選項不同，快速啟動選項將您的應用繫結到了一個通用的 Android "容器" 中，實際上並不會在您的裝置上安裝。在一些情況下，比如您使用的外掛訪問了後臺執行的內容，快速啟動選項將不起作用。如果您覺得 Android 除錯的啟動時間漫長得讓人頭疼，不妨試試這個全新的選項吧。

如果您的目標平台是 Android，您會注意到，現在建立新的 Flutter 專案時只提供 AndroidX 選項。[AndroidX](https://developer.android.google.cn/jetpack/androidx) 庫提供了被稱為 [Android Jetpack](https://developer.android.google.cn/jetpack/) 的高階 Android 功能。在上一個版本中，我們不再支援原先的 Android Support Library，轉而將 AndroidX 作為所有新專案的預設選項。在 Flutter 1.17 中，flutter create 命令只有 --androidx 這一個選項。雖然現有的不使用 AndroidX 的 Flutter 應用依然可以編譯，但 [是時候遷移至 AndroidX 了](https://zhuanlan.zhihu.com/p/136351588)。

如果您使用 Android Studio 或 IntelliJ，您會發現 Hot Reload (熱重載) 功能的容錯性更強了。在本次更新之前，如果您的應用出現了任何分析錯誤，Hot Reload 將不會重新載入您的程式碼。如果分析錯誤並不涉及您當前正在執行的程式碼 (比如在單元測試中)，會讓人很崩潰。但透過 [本次更新](https://groups.google.com/forum/m/#!topic/flutter-announce/tTgQcTgqrKg)，Hot Reload 將不再受分析錯誤影響，而取決於 VM 中的編譯錯誤。

如果您想更早地體驗 Android Studio 或 IntelliJ 的 Flutter 外掛中類似的改進，我們現在也為 [IntelliJ 外掛提供了 dev 渠道](https://groups.google.com/forum/m/#!topic/flutter-announce/tTgQcTgqrKg)，您可以選擇加入，以更快獲取到這些更新。這個 dev 渠道的目標是在公開發布新 IDE 整合功能前透過 Flutter 開發者收集反饋。如果您希望嚐鮮並且願意向 Flutter 工具團隊提供早期反饋，請即刻 [加入我們的體驗計劃](https://groups.google.com/forum/m/#!topic/flutter-announce/tTgQcTgqrKg)！

如果您使用的是 Visual Studio Code，我們推薦使用新的 **Dart: List Outdated Packages** 命令來執行新的 pub outdedated 命令。詳情請見 Dart 2.8 釋出 (今日微信二條文章)。

![](https://devrel.andfun.cn/devrel/posts/2021/05/wRDc6Y.png)

這個命令用於釐清依賴項中的版本問題。

最後值得一提的是，現在當 Flutter 崩潰時，工具會提示您上報這個 bug:

![](https://devrel.andfun.cn/devrel/posts/2021/05/mh6Izu.png)

我們的團隊會密切關注這些錯誤報告的嚴重程度和出現頻率，所以當這些提示出現時，請提交 bug 給我們。

## **案例分享: MGM 和 Superformula**

Flutter 的存在是為了實現精美的應用體驗。數字開發商 [Superformula](https://superformula.com/flutter/) 最近就完成了一件精彩的作品: 他們最近與 MGM Resorts (美高梅酒店集團) 合作，完全使用 Flutter [對其移動應用進行了重構](https://www2.mgmresorts.com/app/)。他們反饋道，"在核心產品中引入 Flutter，為我們的客戶和他們的使用者帶來了更快的速度和更高的靈活性，從而為他們帶來了真正的、可衡量的價值。"

![](https://devrel.andfun.cn/devrel/posts/2021/05/004NS7.png)

Superformula 與 MGM Resorts 設計團隊合作，為所有主要的網路、移動和店頭體驗打造了全新的 MGM 設計語言。這個規模不大的團隊使用全新的 Flutter 原始碼庫更快地完成了應用改版，並在兩個應用商店上架，使得 MGM 的預訂轉化率提高了 9%。

## **重要改動 (Breaking Changes)**

和以往一樣，我們會盡量減少每一個 Flutter 新版本中的重要改動，我們會反覆權衡利弊，確保 Flutter 能夠提供直觀、靈活的 API，能夠在新平臺上支援新的開發習慣。在去年的使用者調查中，開發者表示可以接受經過深思熟慮的、能夠改善框架的重要改動。因此，我們正在穩步、審慎地更新 API。Flutter 1.17 中的重要改動包括:

* #42100 在使用 pushReplacement 時使用之前的路徑執行後續動畫
* #45940 棄用 UpdateLiveRegionEvent
* #49389 在高速滾動時推遲影象解碼
* #49391 文字選取溢位 (Android)
* #49771 斷言快取提示未針對空繪圖物件進行設定
* #50318 即時影象快取
* #50354 使用構成要素高度計算選區範圍，確保其維持在可視範圍內
* #50733 在 gen_l10n 中產生訊息查詢
* #51435 從 RouteSettings 中移除 isinitialroute
* #52781 將 mouse_tracking.dart 移動到 rendering

## **小結**

隨著移動端支援的不斷成熟，以及 [Web 端逐步穩定](https://mp.weixin.qq.com/s/NGqF2OTvsV1A2KLiMXE2PQ)，Flutter 有望解決困擾行業幾十年的問題: 如何透過單一程式碼庫構建出多平臺部署的優秀應用？Flutter 成長至今的表現讓我們相信，我們正走在正確的道路上，也期待著您精彩的 Flutter 作品！
