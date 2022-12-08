---
title: Flutter 1.12 正式釋出，為這一年畫上圓滿的句號！
description: Flutter 1.12 正式釋出，包括多項效能改進等。
---

*作者 / Chris Sells，Flutter 開發者體驗產品經理*

我們很高興正式推出 Flutter 最新穩定版: Flutter 1.12。自從去年 12 月釋出 Flutter 1.0 以來，這已經是我們釋出的第 5 個穩定版本了。多麼精彩的一年！我們一共解決了 5,303 個報錯，合併了來自 484 位貢獻者的 5,950 份 pull request。我們在 Flutter 引擎和框架中添加了對 Android App Bundles、iOS 13 和 web 的支援，實現了滑鼠與鍵盤事件，釋出了應用內購外掛，融合了[多項](https://github.com/flutter/engine/pull/12385)[重要](https://github.com/flutter/flutter/pull/36482)的[效能](https://github.com/flutter/engine/pull/10182)[改進](https://github.com/flutter/flutter/pull/37275)，還新增了 24 種語言支援和多個 widget。
此外，隨著 Dart DevTools 的釋出，Flutter 開發工具也比之前更為強勁。Dart DevTools 內含 widget 檢查器以及記憶體與 CPU 效能分析工具，而且最佳化後的日誌功能在所有編輯器和 IDE 中都能流暢執行。此外，我們還針對參考型別添加了程式碼包自動匯入功能，加入了 ChromeOS 顯式支援以及 UI Guide，讓您的建構方法更易讀寫，並從排版、配色和可操作性三方面對 Flutter 的錯誤資訊進行了最佳化。

我們每次推出新版本的時候都會反覆強調: 這是一個新的開始。Flutter 1.12 自然也不例外，作為我們到目前為止最大的一次版本更新，1.12 包含了 188 位貢獻者的辛勤付出，解決了 4,571 個報錯，合併了 1,905 份 pull request。與之前釋出的版本類似，本次更新也包含了大量針對 Flutter 框架和開發工具的改進。

**Flutter 框架**

為更好地支援 iOS 13，Flutter 1.12 在視覺效果方面進行了全面更新，其中包括深色模式 (Dark Mode) 完整實現、全新的 Cupertino widget，多項 UX 微調以及增強版 Add-to-App 體驗。

**全面支援 iOS 13 深色模式**

Flutter 1.12 帶來的一個重磅訊息是，我們現已支援 iOS 13 風格的介面和操作。這包括在 Cupertino widget 中對深色模式的全面支援。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/ios-13-dark-mode.png){:width="60%"}

仔細觀察上圖您會發現，如果想要支援深色模式，可不是單單換個背景顏色就大功告成了，必須要讓螢幕上的其它顏色也適應偏暗的色調才行。這些細節處理為開發者帶來了巨大的工作量，但是為了在深色和淺色模式下都能呈現出精美的 Cupertino 風格外觀，這些努力都是值得的。

在 iOS 13 上實現畫素級完美是我們一直在努力的目標，為此，我們在 Flutter 1.12 中新增了 2 個 widget。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/cupertino-context-menu-and-cupertino-sliding-segmented-control.png){:width="60%"}

[CupertinoContextMenu](https://api.flutter.dev/flutter/cupertino/CupertinoContextMenu-class.html) 和 [CupertinoSlidingSegmentedControl](https://api.flutter.dev/flutter/cupertino/CupertinoSlidingSegmentedControl-class.html)

最後，為了讓 Flutter 應用能在 iOS 13 裝置上實現原生級別的介面和操作感受，我們還[提高了捲軸保真度](https://github.com/flutter/flutter/pull/41799)，[提供了自適應對話方塊彈出模式 CupertinoAlertDialog](https://github.com/flutter/flutter/pull/42967)，並在 [CupertinoDatePicker 內添加了最小/最大日期約束](https://github.com/flutter/flutter/pull/44149)。

**Add-to-App 更新**

Add-to-App 功能更新是我們在移動支援方面所做的另一項改進。透過 Add-to-App，開發者可以將 Flutter 整合到現有的 Android 或 iOS 應用中。我們一直在努力簡化整合流程，讓您可以更輕鬆地把 Flutter 程式碼庫新增到應用中，比如說，我們在 Android Studio 中添加了一個全新的 Flutter 模組嚮導。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/flutter-module.png){:width="70%"}

Flutter 1.12 現已正式支援 Add-to-App 功能，允許開發者在應用中新增一個全屏 Flutter 例項。在支援這個功能的同時，我們還: 

-   提高了 API 穩定性，以便在平臺中整合 Java、Kotlin、Objective-C 和 Swift 程式碼，其中包括一套全新的 Android API。請參閱 [Android 專案遷移說明](https://flutter.cn/go/android-project-migration)，瞭解變更細節。
-   支援在內嵌 Flutter 模組中使用外掛。
-   透過 [Android AAR](https://flutter.tw/development/add-to-app/android/project-setup#option-a---depend-on-the-android-archive-aar) 和 [iOS 框架提供額外的整合機制](https://flutter.tw/development/add-to-app/ios/project-setup#option-b---embed-frameworks-in-xcode)，以提高與現有建構系統的相容性。
-   更新了命令列工具、VSCode 和 IntelliJ 外掛中的 "flutter attach" 機制，方便開發者接入正在執行的 Flutter 模組，並進行除錯，使用 DevTools 或者進行熱重載。    

如果您想要體驗 [Add-to-App](https://flutter.tw/development/add-to-app) 功能，請參閱文件或瀏覽我們的[範例專案](https://github.com/flutter/samples/tree/master/experimental/add_to_app)，我們在這些專案中展示了多種整合場景。

**Dart 2.7**

當然，我們在 Flutter 中所做的一切都是建構在 Dart 的基礎上的，所以，如果您還沒有聽過擴充方法和字串安全處理 (包含表情符)，或是想要了解非空型別在空安全方面的最新知識，不妨閱讀[《Dart 2.7 現已釋出》](https://medium.com/dartlang/dart-2-7-a3710ec54e97)進一步瞭解相關資訊。

**在 Flutter 1.12 穩定版之外也同樣精彩**

Flutter 不僅透過穩定渠道釋出了許多全新功能，穩定版之外亦是精彩紛呈，尤其是 Beta 版本的 web 支援以及 Alpha 版本的 macOS 支援。

**Beta 版 web 支援**

Flutter 1.12 master、dev 和 beta 三個渠道所提供的 web 支援均有明顯提升。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/support-for-web.png){:width="95%"}

[Rivet](https://rivet.area120.com/link/flutter) (應用介面如上圖所示) 對 Flutter 在 web 端的表現非常滿意。Rivet 是一個教育專案，旗下的移動版應用已經發布，目前，他們正在使用 Flutter 和 Firebase 建構 web 版本的應用，預計釋出時間為 2020 年 1 月。

請閱讀這篇 [Flutter web 支援文章](https://medium.com/flutter/web-support-for-flutter-goes-beta-35b64a1217c0)，進一步瞭解其他開發者的 Flutter web 故事。

**macOS 端支援進入 alpha 版本**

macOS 桌面支援的進展也很順利，現在已經從技術預覽版迭代至 alpha 版，並透過 master 和 dev 兩個渠道開放下載。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/macos-desktop-support.png){:width="95%"}

上圖展示的是桌面尺寸的[新版 Flutter Gallery](https://flutter.github.io/samples/#/)，經過徹底升級，現在它已經支援 Android、iOS、web 和 macOS。

macOS 端支援的 alpha 版代表著 Flutter 在桌面支援領域的重大進展，其中包括全新的 DataTree、Split 範例 widget、已成功移植至 macOS 的多個外掛、釋出 (release mode) 和分析模式 (profiling mode) 內的建構支援、以及更為簡便的工具體驗。如果您透過 master 和 dev 渠道執行 Flutter，請在 Flutter 系統配置中啟用 macOS 桌面端支援，以便獲取 macOS 工具:

`$ flutter config --enable-macos-desktop`

現在，您只需透過 "flutter create" 命令就能建立一個可在 macOS 平臺上執行的 Flutter 專案，操作步驟和新建一個普通的 Flutter 專案一樣簡單。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/flutter-create.png){:width="95%"}

△ 請注意預設建立的新 macOS 目錄

除了工具支援之外，我們也在一直探索適合桌面級應用的 widget 密度。移動應用需要較大的控制區域才能正常進行觸控操作，但在桌面應用中，使用者更可能會使用滑鼠。為了把 Flutter 帶到桌面，我們現在允許您選擇 widget 密度，以便更好地滿足桌面使用者的需求。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/sample-demonstrating-flutters-implementation-of-the-material-density-guidelines.gif){:width="60%"}

[範例](https://github.com/gspencergoog/density_sample): [Material 視覺密度設計規範](https://material.io/design/layout/applying-density.html)在 Flutter 上的實現 

最後一點，為了改善 Flutter 桌面應用的體驗，我們還在鍵盤導航和鍵盤存取方面做了不少工作，包括:

-   [在按鍵事件中同步修飾鍵資訊](https://github.com/flutter/flutter/pull/43948)
-   [在下拉列表開啟時管理專案選取](https://github.com/flutter/flutter/pull/43722)
-   [為主焦點新增便利存取器](https://github.com/flutter/flutter/pull/43859)
-   [為切換控制項新增鍵盤導航、懸浮和快捷方式](https://github.com/flutter/flutter/pull/43384)
-   [勾選框和單選按鈕](https://github.com/flutter/flutter/pull/43384)
-   [自動滾動以便讓焦點項維持在視野內](https://github.com/flutter/flutter/pull/44965)
-   [基於鍵盤快捷方式的滾動](https://github.com/flutter/flutter/pull/45019)
-   [處理焦點和懸浮的新 widget](https://github.com/flutter/flutter/pull/44867)
-   [重寫複製/貼上和鍵盤選取](https://github.com/flutter/flutter/pull/44130)
-   [下拉列表鍵盤導航](https://github.com/flutter/flutter/pull/42811)
-   [視覺密度支援](https://github.com/flutter/flutter/pull/43547)
-   [新增 macOS 功能鍵支援](https://github.com/flutter/flutter/pull/44410)

除了 Flutter Gallery，我們還推薦大家去看一下[全新的 Photos Search 範例應用](https://github.com/flutter/samples/tree/master/experimental/desktop_photo_search)，我們在其中展示了桌面平台特有的強大功能，包括鍵盤操作、新加入的 widget 視覺密度、新外掛、新 widget 等。

至於 Windows 和 Linux 支援，它們目前還處在技術預覽階段，不過，macOS alpha 版的開發經歷很多值得借鑑的地方，有助於我們之後開展 Windows 和 Linux 的工作。我們近期就會和大家分享關於這兩個平台的最新訊息。如果您想進一步瞭解 Flutter 為 macOS、Windows 和 Linux 等桌面系統提供了哪些支援，請移步 [flutter.cn/desktop](https://flutter.cn/desktop)。

**Flutter 工具**

除了框架和引擎之外，我們在 Flutter 工具方面也有許多內容想與您分享，包括: 提供 Flutter 支援的新版本 DartPad、基於 IntelliJ 的增強版 IDE (內含新功能 Hot UI 的預覽)、強化版 Dart DevTools (內含全新的視覺佈局檢視)、Visual Studio Code 同步多裝置除錯啟用，以及改進版 Android 建構流程，此外，我們提供了更好的差異檢測支援，幫助您檢測已渲染 widget 在不同試執行中差異。

**DartPad 現已支援 Flutter**

如果您還沒用過 [DartPad](https://dartpad.cn/) 的話，不妨現在就上手試試！有了它，您不用安裝任何工具就能體驗 Dart 的功能。此外，最新的 DartPad 還支援 Flutter！

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/dartpad.png){:width="95%"}

全新的 DartPad 活用了 Flutter 的 web 端支援，當您在左側編寫 Flutter 程式碼的時候，右側就會即時執行一個真實的 Flutter (web) 程式。DartPad 的獨特之處在於，您無需安裝任何程式就能立即開始編寫 Flutter 應用。

除了單獨的 DartPad 網站之外，我們還在文件和 codelab (如 "Flutter [佈局基礎課程](https://flutter.tw/codelabs/layout-basics)" 和 "[隱含動畫](https://flutter.tw/codelabs/implicit-animations)") 中加入了帶 Flutter 支援的 DartPad，從而方便大家在瀏覽器中學習 Flutter。如果您想要了解更多關於 DartPad 的資訊，請閱讀《[DartPad.dev 全面升級，現已支援 Flutter](https://medium.com/dartlang/a-brand-new-dartpad-dev-with-flutter-support-16fe6027784)》。

**在 IDE 中透過 Hot UI 直接建構 widget**

如果您在自己的機器上安裝了本地工具 (我們也希望您這麼做)，您會在 Flutter 的 IntelliJ/Android Studio 外掛中找到一個新功能預覽。現在您可以在開發 widget 的過程中，直接在 IDE 裡進行預覽並與之進行互動。

我們把這個功能起名為 Hot UI，它與熱重啟有點類似，當您在程式碼中做出改動的時候，Hot UI 就會直接更新對應的 UI。您也可以和 UI 進行互動 (比如像上圖中這樣更改顏色)，所有改動會直接寫入程式碼。如需啟用 Hot UI 預覽功能，請閱讀 [Github Flutter wiki 頁面上的操作指南](https://github.com/flutter/flutter-intellij/wiki/HotUI-Getting-Started-instructions)。

**使用 Layout Explorer 除錯佈局問題**

無論您是選擇自己手寫程式碼，還是讓 Hot UI 替寫，程式碼中出現問題總是難免的。我們推出 Dart DevTools 工具的目的就是，幫助您找到並修復這些問題。在新版 DevTools 中，我們添加了一個名為 Layout Explorer 的功能，它能夠以視覺化的方式呈現應用的佈局資訊，從而讓檢查器可以更好地發揮功能。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/layout-explorer.gif){:width="95%"}

Layout Explorer 不僅能以視覺化的方式展現正在執行的應用中的 widget 佈局，而且還允許您以互動的方式更改佈局選項。我們希望這個功能預覽可以讓您更容易理解並修正佈局問題。如需啟用這一功能，請參閱 [Layout Explorer 官方文件](https://flutter.tw/development/tools/devtools/inspector#flutter-layout-explorer)。

**多裝置除錯**

建構並除錯 Flutter UI 的工作往往是在同一台裝置上完成的。如果能同時在多臺實體或虛擬的裝置上除錯您的應用，是不是會更好呢？Flutter 在 Visual Code 上提供的多會話除錯支援就能幫您做到這一點。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/multi-device-debugging.png){:width="95%"}

從上面這張圖裡我們可以看到，一個 Flutter 應用正同時執行在 3 個不同的除錯會話中，如果我們在程式碼中做出一處更改，Hot Reload 會確保這個更改反映在所有 3 個應用中。如果我們設定一個斷點，那麼無論哪個應用觸發了相關程式碼，它都會停下來。如果您想中止某一個會話的除錯，也不需要停止所有的會話。請前往 Github wiki 頁面瞭解如何配置這個功能來實現[多裝置除錯](https://github.com/flutter/flutter/wiki/Multi-device-debugging-in-VS-Code)。

**Android 建構改進**

最後一點，為了繼續改進 Android，我們在新版本中解決了一些建構方面的問題。首先，我們讓 Android 建構體驗更加強健了，尤其是使用 Support 庫的外掛與使用 AndroidX 的外掛混用的情況。我們的做法是把 Flutter 團隊的外掛移入 AndroidX，我們[建議大家把應用和外掛也遷移至 AndroidX](https://flutter.tw/development/packages-and-plugins/plugin-api-migration)。對於那些尚未遷移的外掛，如果存在建構問題，我們已經在建構中添加了其它的程式碼路徑，這個路徑使用的是 Android Archive 檔案和 Jetifier。但如此一來建構的速度也減慢了，所以我們不把它作為主要的建構機制，但我們發現，這個做法解決了我們遇到的大約 95% 的建構問題。

我們解決的另一個問題使用 Google 開發的最新程式碼最佳化器 [R8](https://developer.android.google.cn/studio/build/shrink-code) 來替代 ProGuard。在此版本之前，應用開發者必須按照外掛作者提供的指南手動配置 ProGuard 規則。在從新版本開始，外掛可在原始碼中定義規則，R8 會自動根據這些規則進行程式碼最佳化，從而為開發者解決了手動配置的難題。

此外，我們也在一直努力減少 Flutter 的應用體積，現在，供 Android 使用的 Hello, World 應用已經成功 "瘦身" 2.6% (從 3.8MB 降到 3.7MB)。不積跬步，無以至千里！

**Golden 影象測試**

Golden 影象指的是一個主影象檔案，它是 widget、state、應用或其它您選擇捕捉的視覺內容的正確渲染結果。在 Flutter 1.12 中，我們實現了 [GoldenFileComparator](https://api.flutter.dev/flutter/flutter_test/GoldenFileComparator-class.html) 和 [LocalFileComparator](https://api.flutter.dev/flutter/flutter_test/LocalFileComparator-class.html) 類，它們依照畫素而不是位元來進行比較，因此可以徹底消除錯誤的比較結果。這些新的實現強調呈現視覺差異，從而更清楚地展現出 Golden 影象和正在測試中的更新檔案之間的差異。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/golden-image-testing.png){:width="60%"}

在這個例子中，主影象檔案 (上圖) 和測試影象檔案 (下圖) 之間的差異集中在輪廓上，透過差值圖 (中圖) 可以更容易地看出二者之間的差異。

**社群**

除了 Flutter 及其工具方面的工作，Flutter 社群還在不斷地引領 Flutter 前往令人驚奇的新領域！請檢視☟下方影片☟，瞭解一下社群中的開發者們正在做什麼。

擁有這樣一群了不起的開發者，真是 Flutter 社群的一大幸事。您們讓全體 Flutter 團隊成員深感驕傲！

<iframe src="//player.bilibili.com/player.html?aid=86761188&cid=148257761&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="560" height="315"> </iframe>

**Flutter Favorite 程式碼包**

當我們在 2019 年 12 月推出 Flutter 1.0 的時候，pub.dev 上大約有 1,000 個程式碼包支援 Flutter，我們當時認為這個數字很大。現在，這個數字已經增加至 6 倍以上。選擇這麼多，究竟該用哪個程式碼包反而成了一件麻煩事。pub.dev 的綜合評分系統和全新的 [Verified Publishers (釋出者認證)](https://medium.com/dartlang/verified-publishers-98f05466558a) 功能都能幫助大家更好地做出選擇。另外，pub.dev 即將迎來[評分系統](https://medium.com/dartlang/dart-2-7-a3710ec54e97)，進一步為大家提供方便。

我們的使用者仍在不斷要求推出 "官方推薦" 的程式碼套件和外掛。為此，我們很高興地推出 [Flutter Favorite 計劃](https://flutter.tw/development/packages-and-plugins/favorites)。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/the-flutter-favorite-program.png){:width="30%"}

Flutter Favorite 程式碼包 (或外掛) 是我們認為您在建構應用時的第一選擇。這裡的 "我們" 指的是 Flutter Ecosystem Committee (Flutter 生態圈委員會)。委員會成員來自各個地區，由 Google 的 Flutter 團隊和 Flutter 社群共同推選，他們的目標是解決 Flutter 生態圈中存在的各種問題。委員會要做的第一件事就是建立一個高標準基線，並依照這個標準線挑選出合適的程式碼套件。被選中的程式碼套件的作者可以在程式碼包說明文件中使用 Flutter Favorite 徽標。此外，pub.dev 在更新後也會在搜尋結果等位置顯示 Flutter Favorite 徽標。

請大家前往 flutter.cn 網站上的 [Flutter Favorite 頁面](https://flutter.tw/development/packages-and-plugins/favorites)瞭解詳情。您也可以前往 pub.dev 檢視[完整的 Flutter Favorite 程式碼包列表](https://pub.dev/flutter/favorites)。關鍵是，如果您是應用開發者，而且您看到了 Flutter Favorite 徽標，那麼您就可以認為這個程式碼包是可以信賴的。如果您是得到 Flutter Favorite 認證的程式碼包作者，我們非常感謝您對 Flutter 生態圈所做的貢獻。

**來自社群的工具**

談到值得驕傲的貢獻，就不得不說 Flutter 社群打造的眾多出色工具。我們在下面列舉部分成果。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/flutter-device-preview.png){:width="85%"}

[Flutter Device Preview](https://github.com/aloisdeniel/flutter_device_preview)

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/widget-maker.png){:width="85%"}

[Widget Maker](https://www.widgetmaker.dev/)

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/panache.png){:width="85%"}

[Panache](https://rxlabz.github.io/panache/#/)

**工具開發夥伴鳴謝: Nevercode**

Flutter 生態圈不僅包括由社群開發的豐富工具，還有一批很棒的工作開發夥伴。Nevercode 一直以來都是我們的重要合作伙伴之一，他們最新發布的工具提供了許多新功能，其中一項就是名為 [Remote Mac 的 Visual Studio Code 外掛](https://marketplace.visualstudio.com/items?itemName=codemagic.remote-mac)。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot12-release/remote-mac.png){:width="95%"}

Remote Mac 擴充外掛可以讓您直接連上一臺由團隊託管在雲端的 Mac 主機，然後測試 iOS 和 macOS 版本的 Flutter 程式碼。如果您想要了解更多關於 Nevercode 最新產品的資訊，例如對 web 端和 macOS 端 Flutter 的最新支援，全新的企業功能等等，請參閱他們[最新發布的博文](https://blog.codemagic.io/more-professional-capable-accessible/)。

其他工具開發夥伴們的進展也不容錯過，歡迎閱讀《[Flutter: 首個面向環境計算打造的 UI 平台](https://developers.googleblog.com/2019/12/flutter-ui-ambient-computing.html)》瞭解 Supernove、2Dimensions 和 Adobe 最新發布的產品。

**結語**

今年對於 Flutter 來說是非常重要的一年，Flutter 1.12 更是一次意義重大的釋出。本文大致為大家展示了本次釋出版本中的新內容；如果您想要檢視您最喜歡的 pull release，瞭解我們把精力投放在了哪些地方，想要看看每個領域中有多少 pull release，或者看看我們在程式碼和功能上做了哪些改動，歡迎查閱 [Flutter v1.12 釋出說明](https://flutter.tw/development/tools/sdk/release-notes/release-notes-1.12.13)。

我們認為 Flutter 正朝著正確的方向快步前進，希望您也同意我們的看法。擁有這麼多的新功能和新工具之後，您會建構什麼呢？我們拭目以待。

文/ [Flutter Blog](https://medium.com/flutter/announcing-flutter-1-12-what-a-year-22c256ba525d)，
譯/ [谷歌開發者](https://mp.weixin.qq.com/s/sETtUi-J4cxCCbKP_FQkpA)
