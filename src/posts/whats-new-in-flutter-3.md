---
title: Flutter 3 更新詳解
toc: true
keywords: Flutter 3, Flutter 3 新特性
description: 深入瞭解最新版本，包括 macOS 和 Linux 平台的穩定支援、多項效能改進等！
image:
    path: https://devrel.andfun.cn/devrel/posts/2022/05/TCPW12.png
---

*作者 / Kevin Jamaul Chisholm, Technical Program Manager for Dart and Flutter at Google*

又到了 Flutter 穩定版釋出時間，我們無比自豪地宣佈推出 [**Flutter 3**](https://flutter.cn/posts/introducing-flutter-3) ！僅 3 個月前，我們宣佈了 [Flutter 對 Windows 的支援](https://flutter.cn/posts/announcing-flutter-for-windows)。現在，我們再次懷著激動的心情宣佈，繼 Windows 之後，Flutter 現已穩定支援 macOS 和 Linux！

我們總計合併了 5,248 條 PR，感謝各位 Flutter 貢獻者的辛勤工作！

此版本中激動人心的升級包括: 更新了 Flutter 對 macOS 和 Linux 的支援，效能得到了顯著提升，針對移動裝置和 web 端的更新，以及諸多其他功能！此外，我們還帶來了關於減少對舊版 Windows 提供支援的訊息，以及幾條重大變更。下面讓我們直奔主題吧！

## **全桌面平台生產就緒**

Linux 和 macOS 平台的支援已進入穩定狀態，內含下列功能:

### **級聯選單和 macOS 系統選單欄支援**

現在您可以使用 `PlatformMenuBar` widget 在 macOS 上建立平台渲染的選單欄，支援插入僅限該平台使用的選單，並控制 macOS 應用選單中的顯示內容。

![△ 級聯選單示意](https://devrel.andfun.cn/devrel/posts/2022/05/QHCvSV.gif)

△ 級聯選單示意

### **完整支援全桌面平台多國文字輸入**

全部三種桌面平台完整支援多國文字輸入，包括使用文字 [輸入法編輯器](https://en.wikipedia.org/wiki/Input_method) (IME) 的語言，如中文、日文和韓文。同時支援第三方輸入法，如搜狗 (Sogou)、谷歌日文輸入法 (Google Japanese Input) 等。

### **全桌面平台無障礙服務**

Flutter 支援 Windows、macOS 和 Linux 平台的無障礙服務，包括螢幕文字閱讀、無障礙導航和顏色反轉等。

### **macOS 平台預設使用通用二進位制檔案**

在 Flutter 3 中，Flutter macOS 桌面應用會被建構為通用二進位制檔案，從而原生支援使用 Intel 處理器的 Mac 電腦和搭載 Apple Silicon 的新裝置。

### **不再支援使用 Windows 7/8 進行開發**

此版本將建議的 Windows 開發版本提升為 Windows 10。雖然我們不會禁止使用舊版本 (Windows 7、Windows 8、Windows 8.1) 進行開發，但由於 [Microsoft 不再支援這些舊版本](https://docs.microsoft.com/en-us/lifecycle/faq/windows)，我們僅會針對這些版本提供有限的測試。儘管我們會盡力為舊版本提供支援，但還是建議您升級版本。

*注意: 在 Windows 7 和 8 上依然可以執行 Flutter 應用，此更改隻影響我們推薦使用的開發環境。*

## **移動端更新**

我們針對移動端的更新包括:

### **支援可摺疊裝置**

Flutter 3 支援可摺疊移動裝置。透過由 Microsoft 牽頭的合作，讓大家可以使用新的功能和 widget 在可摺疊裝置上建立動感、愉悅的體驗。

作為合作的一部分，[MediaQuery](https://api.flutter.dev/flutter/widgets/MediaQuery-class.html) 現在包含一個 [DisplayFeature](https://api.flutter.dev/flutter/dart-ui/DisplayFeature-class.html) 列表，用以描述裝置元件狀態，包括鉸鏈、摺疊狀態和螢幕劉海等。此外，[DisplayFeatureSubScreen](https://api.flutter.dev/flutter/widgets/DisplayFeatureSubScreen-class.html) widget 包含的子 widget 的位置現在不會與 `DisplayFeature` 的邊界重疊，並且已經用於框架的預設對話方塊和彈出視窗，使 Flutter 預設即可動態適應這些元素的位置。

![](https://devrel.andfun.cn/devrel/posts/2022/05/Nqhj5g.png)

非常感謝 Microsoft 團隊。另外我們還要特別鳴謝 [@andreidiaconu](https://github.com/andreidiaconu) 所做出的貢獻！

歡迎大家嘗試[ Surface Duo 模擬器範例](https://docs.microsoft.com/en-us/dual-screen/flutter/samples)，其中包含了 Flutter Gallery 的一個特別派生版本，以便了解 Flutter 在雙屏中的實際執行情況:

### **支援 iOS 可變重新整理率**

Flutter 現已在使用 ProMotion 顯示屏的 iOS 裝置上支援可變重新整理率，包括 iPhone 13 Pro 和 iPad Pro。在這些裝置上 Flutter 應用的渲染重新整理率可達 120 Hz，而之前最高為 60 Hz，這使得滾動等快速動畫的觀感體驗更加流暢。請檢視 [官方文件](https://docs.google.com/document/d/1O-ot6MydAl5pAr_XGnpR-Qq5A5CPDF6edaPu8xQtgCQ/edit?resourcekey=0-LlXeGtGRC67XL4NrGoc91A) 瞭解詳情。

### **簡化 iOS 釋出**

我們為 `flutter build ipa` 命令添加了 [新選項](https://github.com/flutter/flutter/pull/97672)，使得 iOS 應用釋出更加簡便。在您準備好分發至 TestFlight 或 App Store 時，請執行 flutter build ipa 建構 Xcode 歸檔 (.xcarchive 檔案) 和應用軟體包 (.ipa 檔案)。您可選擇新增 --export-method ad-hoc、--export-method development 或 --export-method enterprise 選項。應用軟體包建構完成後，即可透過 [Apple Transport macOS 應用](https://apps.apple.com/us/app/transporter/id1450874784) 將其上傳至 Apple，或使用 xcrun altool 在命令列中完成上傳 (執行 man altool 獲取 App Store Connect API 金鑰驗證說明)。上傳完成後，您的應用即可釋出至 [TestFlight 或 App Store](https://flutter.tw/deployment/ios#release-your-app-to-the-app-store)。在完成應用顯示名稱、應用圖示等初始 [Xcode 專案設定](https://flutter-io.cn/docs/deployment/ios#review-xcode-project-settings) 後，您在釋出應用時就無需再開啟 Xcode 了。

### **Gradle 版本更新**

使用 Flutter 工具建立新專案時，您或許已經注意到，產生的檔案現在使用了最新版本的 Gradle 和 Android Gradle 外掛。對於現有的專案而言，您需要手動將 Gradle 版本升級至 7.4，Android Gradle 外掛版本升級至 7.1.2。

### **停止更新 32 位 iOS/iOS 9/iOS 10**

按照 2022 年 2 月 2.10 穩定版釋出的公告，Flutter 對 32 位 iOS 裝置以及 iOS 9 和 10 的支援即將結束。這一變化會影響到 iPhone 4S、iPhone 5、iPhone 5C 以及 iPad 第二、三、四代裝置。Flutter 3 是最後一個支援上述 iOS 版本和裝置的穩定版本。

如需詳細瞭解此項變更，請參閱 [RFC: 終止對 32 位 iOS 裝置的支援](https://docs.google.com/document/d/1cc5EOsuTlbf4dTDTwmkD3aKjS8XEbVCIqi9BFct9XHM/edit?resourcekey=0-Iv0gXDx7nSDCe3YDfxDKqw)。

## **Web 端更新**

我們針對 web 端的更新包括:

### **影象解碼**

在瀏覽器支援的情況下，Flutter web 現在可以自動檢測並使用 ImageDecoder API。到目前為止，大多數基於 Chrome 的瀏覽器都添加了此 API，如 Chrome、Edge、Opera、Samsung Browser 等。

這個新 API 使用瀏覽器內建的影象編解碼器在主執行緒之外非同步解碼影象。這使得影象解碼速度提高 2 倍，而且完全不會阻塞主執行緒，消除了所有之前由影象引起的卡頓現象。

### **Web 應用的生命週期**

Flutter web 應用的新生命週期 API 提升了靈活性，可實現從託管 HTML 頁面控制 Flutter 應用的載入程式，並支援使用 Lighthouse 分析您的應用的效能表現。這適用於許多使用案例，包括以下常被開發者們提及的場景:

* 啟動畫面。
* 載入指示器。

* 在 Flutter 應用之前顯示的純 HTML 互動式載入頁。

請閱讀官方文件 "[自訂 web 應用初始化](https://flutter-io.cn/docs/development/platform-integration/web/initialization)" 瞭解詳細資訊。

## **工具更新**

我們針對 Flutter 和 Dart 工裝的更新內容包括:

### **Lint package 更新**

Lint package 2.0 版現已釋出:

* [Flutter](https://pub.dev/packages/flutter_lints/versions/2.0.0)
* [Dart](https://pub.dev/packages/lints/versions/2.0.0)

使用 `flutter create` 產生的 Flutter 3 應用將自動啟用 2.0 版 Lint 套件。我們建議大家執行 `flutter pub upgrade --major-versions flutter_lints`，將現有應用、package 和外掛遷移到 2.0 版，以遵循 Flutter 最新、最優的最佳實踐。

Lint 2.0 版中新增的大多數警告都帶有自動修復功能。因此，當您在應用的 `pubspec.yaml` 中升級至最新 package 版本後，即可在程式碼庫中執行 `dart fix --apply` 自動修復大多數 Lint 警告 (某些警告仍需部分手動操作)。對於尚未使用 `package:flutter_lints` 的應用、package 或外掛，建議開發者按照 [遷移指南](https://flutter-io.cn/docs/release/breaking-changes/flutter-lints-package#migration-guide) 遷移至最新版本。

### **效能提升**

感謝開源貢獻者 [knopp](https://github.com/knopp)，區域性重繪已經在支援此功能的 [Android 裝置上實現](https://github.com/flutter/engine/pull/29591)。在我們的本地測試中，此功能在 Pixel 4XL 裝置上將依照 `backdrop_filter_perf` 基準測試的幀網格化時間的平均值、90 百分位值和 99 百分位值縮減了 5 倍。現在，iOS 裝置和較新版本的 Android 裝置上都已實現在單一矩形髒區出現時進行區域性重繪。

我們 [進一步提升](https://github.com/flutter/engine/pull/30957) 了簡單使用案例中不透明度動畫的效能。具體而言，當 `Opacity` widget 只包含單個渲染原語時，通常由 `Opacity` widget 呼叫的 `saveLayer` 方法可以省略。在為此最佳化建構的基準測試中，此使用案例下的網格化時間提升了 [一個數量級](https://flutter-flutter-perf.skia.org/e/?begin=1643063115&end=1644004520&keys=X32827d8819e8271e025f50e77bf2bec0&requestType=0&xbaroffset=27447)。在今後的版本中，我們計劃為更多場景應用此最佳化。

在開源貢獻者 [JsouLiang](https://github.com/JsouLiang) 的努力下，引擎的光柵和介面執行緒在 Android 和 iOS 上的執行優先順序已經高於其他執行緒 (比如 Dart VM 的後臺垃圾回收執行緒)。在我們的基準測試中，這使得幀建構平均時間提速 [約 20%](https://flutter-flutter-perf.skia.org/e/?begin=1644581114&end=1644647407&keys=X3999dc0a0c89054eaa9f66bcff27d882&num_commits=50&request_type=1&xbaroffset=27549)。

在第 3 版釋出之前，光柵快取的准入策略只檢視圖片中繪製運算元的數量 (假設任何具有多個運算元的圖片都應該進入快取)。但這會導致引擎消耗記憶體來快取渲染速度極快的圖片。此版本 [引入新的機制](https://github.com/flutter/engine/pull/31417)，根據所包含繪製運算元的成本來估計影象渲染的複雜性。在我們的效能測試中，使用新機制作為網格快取准入策略可以 [減少記憶體用量](https://flutter-flutter-perf.skia.org/e/?begin=1644790212&end=1646044276&keys=X4c7dd4e4903a38523816c00b31d4d787&requestType=0&xbaroffset=27636)，而不會降低效能。

感謝開源貢獻者 [ColdPaleLight](https://github.com/ColdPaleLight)，他修復了 iOS 上由於 [幀排程 bug](https://github.com/flutter/engine/pull/31513) 而導致少量動畫幀丟失的問題。感謝所有報告此問題並提供掉幀復現影片的每一個人。

### **Impeller**

我們一直致力於解決 iOS 和其他平臺上的早期卡頓問題。在 Flutter 3 中，您可以在 iOS 上預覽一個名為 [Impeller](https://github.com/flutter/engine/tree/main/impeller) 的實驗性渲染後端。Impeller 會在引擎建構時預編譯一組 [較為小巧、簡單的著色器](https://github.com/flutter/flutter/issues/77412)，從而避免在應用執行時編譯，而後者是造成 Flutter 卡頓的主要原因。Impeller 尚未作好投產準備，距離完成也還有一段距離。目前 Impeller 尚未實現 Flutter 的所有功能特性，但我們對它在 [flutter/gallery](https://github.com/flutter/gallery) 應用中實現的保真度和效能感到滿意，並且很高興地在這裡和大家分享開發進度。特別是，在 Gallery 應用的過場動畫中，即便最差的幀速度也比之前快大約 [20 倍](https://flutter-flutter-perf.skia.org/e/?begin=1650297849&end=1651261748&queries=sub_result%3Dworst_frame_rasterizer_time_millis%26test%3Dnew_gallery_impeller_ios__transition_perf%26test%3Dnew_gallery_ios__transition_perf&requestType=0)。

Impeller 可以帶標記在 iOS 上使用。如果您要試用 Impeller，可以傳遞 `--enable-impeller` 標記至 `flutter run`，或將 `Info.plist` 檔案中的 `FLTEnableImpeller` 標記為 `true`。Impeller 的開發會繼續在 Flutter 主渠道進行，我們希望在未來的版本中提供進一步更新。

### **Android 上的內聯廣** **告**

使用 [google_mobile_ads](https://pub.dev/packages/google_mobile_ads) package 時，您應該可以感受到使用者關鍵互動 (如頁面之間的滾動和切換) 的效能有所提升。在新興市場廣為流行的裝置上，這種效能提升尤其明顯。最棒的是，您無需更改任何程式碼！

在具體實現方面，Flutter 現在是非同步組合 Android 檢視 (即通常所說的 [平臺視圖](https://flutter-io.cn/docs/development/platform-integration/platform-views))。這意味著 Flutter 的光柵執行緒無需等待 Android 檢視渲染。現在，Flutter 引擎使用它管理的 OpenGL 紋理將檢視顯示在螢幕上。

## **更多令人興奮的更新**

我們針對 Flutter 生態系統的其他更新包括:

### **Material 3**

Flutter 3 支援新一代 Material Design，即 [Material Design 3](https://m3.material.io/)。Flutter 3 提供 Material 3 的可選支援，包括動態顏色、最新顏色系統和字型等 Material You 功能，還包含許多元件的更新，以及在 Android 12 中引入的新觸控波紋設計和拉伸滾動等全新視覺效果。我們歡迎大家透過全新的 "[將枯燥無味的 Flutter 應用變得生動有趣](https://codelabs.developers.google.com/codelabs/flutter-boring-to-beautiful)" 的 Codelab 來嘗試 Material 3 的功能特性。請參閱 [API 文件](https://api.flutter.dev/flutter/material/ThemeData/useMaterial3.html)，詳細瞭解如何選用上述新功能特性，以及哪些元件支援 Material 3。另請關注 [Material 3 Umbrella issue](https://github.com/flutter/flutter/issues/91605) 瞭解最新開發進展。

### **主題擴充**

藉助 "主題擴充 (Theme extension)"，Flutter 現支援向 Material 庫中的 `ThemeData` 新增任何內容。您現在可以指定 `ThemeData.extensions`，而無需 (在 Dart 中) 擴充 `ThemeData` 並重新實現其 `copyWith`、`lerp` 和其他方法。另外，package 開發者也可以提供 `ThemeExtension`。請參閱 [官方文件](https://docs.google.com/document/d/1LbD4JqBgAfHex02oR3r2jyu9lTBBNBmyec2ovT59Kr8/edit) 瞭解詳情，並檢視 GitHub 上的 [相關範例](https://github.com/guidezpl/flutter/blob/master/examples/api/lib/material/theme/theme_extension.1.dart)。

### **廣告**

我們知道對於釋出商來說，徵求使用者同意對個性化廣告，以及應對 Apple 的 "應用追蹤透明度 (App Tracking Transparency, ATT)" 要求非常重要。

為了支援這些需求，Google 提供了 "使用者訊息平台 (User Messaging Platform, UMP)" SDK，取代了之前的開源 [Consent SDK](https://github.com/googleads/googleads-consent-sdk-ios)。在即將釋出的 Google 移動廣告 SDK (Flutter) 中，我們會增加對 UMP (使用者訊息平台) SDK 的支援，讓釋出商能夠徵求使用者同意。如需瞭解詳情，請在 pub.dev 上檢視 [google_mobile_ads](https://pub.dev/packages/google_mobile_ads) package 頁面。

## **重大變更**

在持續擴充和改進 Flutter 的過程中，我們會盡量把重大變更的數量維持在最低限度。Flutter 3 包含以下重大變更:

* [2.10 版之後移除已棄用的 API](https://flutter-io.cn/docs/release/breaking-changes/2-10-deprecations)
* [頁面切換轉為使用 ZoomPageTransitionsBuilder](https://flutter-io.cn/docs/release/breaking-changes/page-transition-replaced-by-ZoomPageTransitionBuilder)
* [Chips 的 useDeleteButtonTooltip 遷移至 deleteButtonTooltipMessage](https://flutter-io.cn/docs/release/breaking-changes/chip-usedeletebuttontooltip-migration)

如果您正在使用上述 API，請參閱 Flutter.dev 上的 [遷移指南](https://flutter-io.cn/docs/release/breaking-changes)。

## **總結**

按照 Statista 和 SlashData 等分析機構的統計，Flutter 依然是最受歡迎的跨平臺介面工具套件，我們能保持這種地位，社群的貢獻功不可沒，對此，Google Flutter 團隊向大家致以由衷敬意。期待與各位社群成員共同努力，繼續提供由社群驅動的工具，幫助大家為使用者創造出更多令人愉悅的體驗！
