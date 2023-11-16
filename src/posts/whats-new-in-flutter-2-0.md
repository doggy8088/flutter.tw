---
title: 無限可能 — Flutter 2 重點更新一覽
toc: true
---

我們非常高興在本週釋出了 [Flutter 2](https://mp.weixin.qq.com/s/tJe2ScLgKWFTybpBtDl2TA)。自 Flutter 1.0 釋出至今已有兩年多的時間，在如此短暫的時間內，我們解決了 24,541 個 issue，合併了來自 765 個貢獻者的 17,039 個 PR。自九月釋出 [Flutter 1.22](https://mp.weixin.qq.com/s/k71z0Kihuz34Ol2O7j1WrA) 以來，我們已解決 5,807 個 issue，合併了來自 298 個貢獻者的 4,091 個 PR。在此特別感謝廣大的貢獻者，將業餘時間慷慨地投入到 Flutter 專案的最佳化中。Flutter 2 的傑出貢獻者有 [xu-baolin](https://github.com/xu-baolin) (貢獻了 46 個 PR)、[a14n](https://github.com/a14n) (貢獻了 32 個 PR；專注於為 Flutter 引入空安全) 和 [hamdikahloun](https://github.com/hamdikahloun) (貢獻了 20 個 PR；優化了一系列 Flutter 外掛)。然而，為 Flutter 專案做出貢獻的不只有開發者，還有我們負責評審 1,525 個 PR 的眾多 PR 評審員，其中包括 [hamdikahloun](https://github.com/hamdikahloun) (身兼數職！)、[CareF](https://github.com/CareF) 和 [YazeedAlKhalaf](https://github.com/YazeedAlKhalaf) (年僅 16 歲！) 等人。Flutter 是真正的社群合力之作，如果沒有問題反饋者、PR 貢獻者和程式碼評審員，版本 2 便無法問世，是你們帶來了這一新版本。

Flutter 2 的釋出也帶來了許多振奮人心的內容。您可以閱讀圖文《[Flutter 2 正式釋出！](https://mp.weixin.qq.com/s/tJe2ScLgKWFTybpBtDl2TA)》，快速瞭解 Flutter 2 和 [Dart 2.12](https://medium.com/dartlang/announcing-dart-2-12-499a6e689c87) 中的新增功能，以及我們的客戶及合作伙伴是如何使用 Flutter 2 的。我們將在下週的圖文中為大家詳細介紹如何在生產環境中使用 [Flutter web 穩定版](https://medium.com/flutter/web-post-d6b84e83b425) 併發揮其優勢。

下面我們一起來詳細瞭解下 Flutter 2 的新增功能吧！

## Web

從現在開始，Flutter web 的支援已經從 Beta 版過渡到穩定階段。隨著首個穩定版本的釋出，Flutter 透過對 web 平台的支援，將程式碼的複用性提升到了一個新高度。現在，當您平穩執行 Flutter 應用時，可以將 web 作為一個新的適配方向。

![](https://devrel.andfun.cn/devrel/posts/2021/03/6547ec1e75755.png)

> 作為一家致力於透過數字化實現卓越營運的現代移動虛擬網路營運商，[Moi Mobiili](https://www.moi.fi/) 選擇使用 Flutter 建構其 Mun Moi 客戶經理應用，並在近期釋出了其 web 版應用。

藉助 web 平台的諸多優勢，Flutter 為建構豐富的互動式 web 應用奠定了基礎。我們的首要重點是提升效能並最佳化渲染。除了 HTML 渲染引擎外，我們還新增了一個基於 CanvasKit 的渲染引擎，以及一些如 [Link Widget](https://pub.flutter-io.cn/documentation/url_launcher/latest/link/Link-class.html) 等特定於 web 的功能，以確保您的應用能夠像 web 應用一樣在瀏覽器中穩定執行。

有關此 [Flutter web 穩定版](https://medium.com/flutter/web-post-d6b84e83b425) 的更多詳情，請參閱 Flutter web 釋出文章:

## 空安全

健全的空安全是對 Dart 語言的重要補充，它透過區分可空型別和不可空型別來進一步加強型別系統。這使開發者能夠防止 null error 崩潰，這也是導致應用崩潰的常見原因。透過將空檢查合併到型別系統中，可以在開發期間捕獲這些錯誤，從而防止生產環境中的崩潰。在包含 Dart 2.12 的 Flutter 2 中，健全的空安全得到了充分的支援。要了解更多細節，請參閱 [Dart 2.12 釋出文章](https://medium.com/dartlang/announcing-dart-2-12-499a6e689c87):

pub.dev 已經發布了 [超過 1,000 個空安全 package](https://pub.flutter-io.cn/packages?q=&null-safe=1)，包括來自 [Dart](https://pub.flutter-io.cn/packages?q=publisher%3Adart.dev&sort=popularity&null-safe=1)、[Flutter](https://pub.flutter-io.cn/packages?q=publisher%3Aflutter.dev&sort=popularity&null-safe=1)、[Firebase](https://pub.flutter-io.cn/packages?q=publisher%3Afirebase.google.com&sort=popularity&null-safe=1) 和 [Material](https://pub.flutter-io.cn/packages?q=publisher%3Amaterial.io&sort=popularity&null-safe=1) 團隊釋出的數百個 package。如果您也是 package 的作者，請參閱 [遷移指南](https://dart.cn/null-safety/migration-guide) 並著手遷移事項。

## 桌面裝置

在新版本中，Flutter 對桌面裝置的支援已經進入穩定版本的前期準備階段。也就是說，您現在可以嘗試將其作為您 Flutter 應用的部署目標: 把它看作最終穩定版釋出前的預覽，最終穩定版本將於今年晚些時候釋出。

Flutter 桌面版經過一系列大大小小的最佳化，才達到現在的品質。我們率先從文字編輯入手，確保其在每個支援平臺上都能令 [文字選擇點](https://github.com/flutter/flutter/pull/71756) 等基本功能獲得如原生般的順暢執行體驗，並能夠 [在鍵盤事件經過處理後將其取消](https://github.com/flutter/flutter/issues/33521)。在滑鼠輸入方面，我們也已確保，當處理觸控輸入以及在 [Material](https://github.com/flutter/flutter/pull/74286) 和 [Cupertino](https://github.com/flutter/flutter/pull/73882) 設計語言的 TextField 和 TextFormField 中新增內建上下文選單，並 [在 ReorderableListView 上提供抓取控點](https://github.com/flutter/flutter/pull/74299) 時，使用高精度定點裝置的拖放事件將立即開始，沒有任何延遲。另外，內建的上下文選單已經新增至以 Material 和 Cupertino 為設計語言的 TextField 及 TextFormField widget 中，ReorderableListView widget 添加了抓取控點功能。

![](https://devrel.andfun.cn/devrel/posts/2021/03/a362da9373a9e.gif)

> ReorderableListView 現已支援抓取控點，便於滑鼠輕鬆拖動

開發者可以使用 ReorderableListView 輕鬆移動專案，但使用者需要長按專案以啟用拖動操作。該設計在移動裝置上很合理，但很少有桌面平台的使用者會想到用滑鼠長按某個專案來移動它，所以新版本提供了一個適用於滑鼠或觸控輸入的抓取控點。按照同樣的思路，對於因平台而異的慣用功能，新版本提供了一個 [經過升級的捲軸](https://github.com/flutter/flutter/pull/71664)，可使其與桌面平台完美適配。

![](https://devrel.andfun.cn/devrel/posts/2021/03/8f412222d985f.gif)

> 新版本中的捲軸 widget 已適配桌面平台

更新後的 Scrollbar Widget 為桌面平台提供了預期的相同互動功能，例如支援拇指拖動、點選捲軸空白區域進行上下翻頁，以及將滑鼠懸停在捲軸的任何部位以顯示一個軌道等。此外，由於捲軸可以透過 [新增的 ScrollbarTheme](https://api.flutter.dev/flutter/material/ScrollbarTheme-class.html) 類設定主題，您可以根據您的應用對其外觀和感覺進行個性化設計。

對於其他桌面平台的特定功能，本版本還為 Flutter 應用啟用了命令列引數處理，這樣一來，您可以透過在 Windows 檔案資源管理器中雙擊滑鼠等簡單操作來開啟應用中的檔案。此外，我們還努力使 [Windows](https://github.com/flutter/engine/pull/23701) 和 [macOS](https://github.com/flutter/engine/pull/23924) 上的大小調整操作變得更加流暢，並面向全球使用者推出了輸入法 (IME) 支援。

![](https://devrel.andfun.cn/devrel/posts/2021/03/62d569de4af28.gif)

> Flutter 桌面現支援輸入法直接輸入

我們還提供了更新的 [文件](https://flutter.cn/desktop#create-a-new-project)，以指導您做好準備，將應用部署到相應的作業系統商店中。您不妨參考一下，如發現有任何遺漏，請反饋給我們。

試用 Flutter 桌面 Beta 版時，您可以按需切換到 beta 渠道來進行存取，並按照 [Flutter 文件上的說明](https://flutter.cn/desktop#create-a-new-project) 設定目標平台的配置標記。此外，我們還在穩定渠道中新增了 Beta 版的快照。當您使用 "flutter config" 啟用其中一個桌面配置設定 (如 enable-macos-desktop 時)，您可以直接嘗試使用桌面 Beta 版功能，而無需再經歷前往 Beta 渠道、獲取完整的 Beta 版及建構工具等漫長的過程。您不妨親自嘗試一下，或把桌面支援作為一個簡單的 "Flutter 模擬器"，非常好用。

然而，如果您選擇繼續透過穩定渠道使用桌面 Beta 版，那麼您將無法像切換到 Beta 或開發渠道時那樣快速獲取新功能或錯誤修復，因此，如果您以 Windows、macOS 或 Linux 為明確目標，我們建議您切換到更新速度更快的渠道。

在開發穩定版 Flutter 桌面支援過程中，我們深知未來還有很多工作要做，包括支援原生最上層選單整合、提供如同獨立平台一樣的文字編輯體驗和無障礙功能支援，以及一般漏洞修復和效能增強。如果您認為桌面平台在投入生產之前仍然有一些地方需要完善，望 [不吝賜教](http://github.com/flutter/flutter/issues)！

## 平台自適應應用範例: Flutter Folio

現在 Flutter 已可在三個平台 (Android、iOS 和 web) 上為生產環境的應用提供支援，還有三個平台仍處於測試階段 (Windows、macOS 和 Linux)，那麼問題來了: 如何開發一款應用，可以良好適應多種不同螢幕規格 (大、中、小螢幕)、不同輸入模式 (觸控、鍵盤和滑鼠) 和不同慣用裝置 (移動、web 和桌面)？為了給自己以及各地的廣大 Flutter 開發者解決這個問題，[Flutter Folio 應用應運而生](https://youtu.be/x4xZkdlADWo)。

您可將 Folio 視作一個簡單的範例應用，幫助您在多個平臺上利用單一程式碼庫良好地執行應用。"良好執行" 是指它能在大、中、小螢幕上正常顯示，並能利用觸控、鍵盤和滑鼠輸入，還可適應不同平台的風格，例如使用 web 上的連結、使用桌面裝置上的選單等等。我們將此類應用稱為 "平台自適應應用"，因為這類應用能良好地適應所執行的任何平台。

如需檢視如何使應用自適應平台，請參閱 [Folio 的原始碼](https://github.com/gskinnerTeam/flutter-folio)。未來，希望能有更為深入地探討此主題的文件和 codelabs 出現。您還可以閱讀和觀看 Aloïs Deniel 關於該主題的 [博文和影片](https://aloisdeniel.com/#/posts/adaptative-ui)。

## Google Mobile Ads 釋出 Beta 版

除了釋出 Flutter 桌面 Beta 版外，我們也非常高興地釋出了 Google Mobile Ads SDK for Flutter 的公開 Beta 版。這是一個全新外掛，除了原有的疊加格式 (疊加橫幅、插頁和激勵影片廣告)，我們還在其中新增了內聯橫幅及原生廣告。另外，我們還在此外掛中提供了 Ad Manager 和 Admob 支援，無論您是何種規模的釋出商，這款外掛都能滿足您的需求。

![](https://devrel.andfun.cn/devrel/posts/2021/03/2d7174f7a14d8.jpg)

在公開發布 Beta 之前，我們邀請了一些客戶參與試用這款外掛。許多客戶都使用了這些新增的廣告格式成功地釋出了應用。例如，Sua Musica (拉丁美洲最大的獨立藝術家音樂平台，擁有超過 15,000 位認證音樂人和一千萬活躍使用者) 使用了 Google Mobile Ads SDK for Flutter 外掛釋出了新的應用。其廣告展示量增加了 350%，點選率增加了 43%，千次展示收益上漲了 13%。

您此刻就可使用該 [外掛]([https://pub.flutter-io.cn/packages/google_mobile_ads](https://pub.flutter-io.cn/packages/google_mobile_ads)) 了。在剛結束的 [Flutter Engage](https://zhuanlan.zhihu.com/p/355036482) 上，Andrew 和 Zoey 圍繞《[如何透過 Flutter 應用獲利](https://v.qq.com/x/page/a323184ybj7.html)》介紹了一些 Flutter 應用創收策略，以及如何在 Flutter 應用中載入廣告，您可以檢視下方影片瞭解詳細內容。此外，我們在 flutter.dev 上建立了一個新的 [Ads 頁面](https://flutter.cn/ads)，方便您查詢所需資源，如 [外掛使用指南](https://developers.google.cn/admob/flutter)，[內聯橫幅和原生廣告](https://codelabs.developers.google.com/codelabs/admob-inline-ads-in-flutter) codelab，以及[疊加橫幅，插頁和激勵影片廣告](https://codelabs.developers.google.com/codelabs/admob-ads-in-flutter#0) codelab，歡迎您隨時檢視。更多資訊請檢視 [如何透過 Flutter 應用獲利](https://v.qq.com/x/page/a323184ybj7.html) 影片。

## 新增 iOS 功能

在不斷提高對其他平台支援的同時，我們並沒有將 iOS 拋諸腦後。事實上，新版本提供了 178 個與 iOS 相關的合併 PR，其中包括將狀態恢復 (State Restoration) 引入 iOS 的 [23495](https://github.com/flutter/engine/pull/23495)，應開發者需求——不用開啟 Xcode 就可以直接從命令列建立 IPA 的 [67781](https://github.com/flutter/flutter/pull/67781)，以及更新 CocoaPods 版本以配合最新工具的 [69809](https://github.com/flutter/flutter/pull/69809)。此外，我們還在 Cupertino 設計語言實現中添加了一些 iOS 小部件。

新增的 [CupertinoSearchTextField](https://api.flutter.cn/flutter/cupertino/CupertinoSearchTextField-class.html) 為 iOS 提供了搜尋欄 UI。

![](https://devrel.andfun.cn/devrel/posts/2021/03/a62ac8b91dd58.jpg)

[CupertinoFormSection](https://api.flutter.cn/flutter/cupertino/CupertinoFormSection-class.html)、[CupertinoFormRow](https://api.flutter.cn/flutter/cupertino/CupertinoFormRow-class.html) 和 [CupertinoTextFormFieldRow](https://api.flutter.cn/flutter/cupertino/CupertinoTextFormFieldRow-class.html) 等 widgets 則利用 iOS 的視覺美學簡化了驗證表單欄位的產生。

![](https://devrel.andfun.cn/devrel/posts/2021/03/2c7bc09f2c6c5.png)

除了為 iOS 新增功能以外，我們也在持續尋求 iOS 和 Flutter 在著色器和動畫方面的 [整體效能最佳化](https://github.com/flutter/flutter/issues/60267#issuecomment-762786388)。iOS 仍然是 Flutter 的首要平台，我們將繼續致力於為大家帶來重要的新功能和效能提升。

## 新增 Widget: Autocomplete 和 ScaffoldMessenger

新版 Flutter 新增了兩個 Widget，分別是 [AutocompleteCore](https://github.com/flutter/flutter/pull/62927) 和 ScaffoldMessenger。AutocompleteCore 是在您的 Flutter 應用中實現自動自動完成功能所需的基礎功能。

![](https://devrel.andfun.cn/devrel/posts/2021/03/e40d0f6eccfcd.gif)

開發者對為 Flutter 增加 Autocomplete 功能的呼聲很高，所以我們在新版本中提供了此功能。您現在即可使用，如果您想了解該功能的設計理念，請參閱 [設計文件](https://docs.google.com/document/d/1fV4FDNdcza1ITU7hlgweCDUZdWyCqd-rjz_J7K2KkfY/)。

同樣，[ScaffoldMessenger](https://github.com/flutter/flutter/pull/64101) 可用於處理許多與訊息提示 Snackbar 相關的問題，例如，它可以輕鬆建立 Snackbar 訊息以響應 AppBar 操作、建立可在 Scaffold 轉換之間持久儲存的 Snackbar 訊息，並能夠在非同步操作完成時顯示 Snackbar 訊息，即使使用者已導航至使用不同 Scaffold 的頁面時也不例外。

![](https://devrel.andfun.cn/devrel/posts/2021/03/aed84cb7b2ce7.gif)

您只需寫一行程式碼，即可將所有這些更加便捷的新功能收入囊中。從現在開始，您可以使用此行程式碼來顯示您的 Snackbar 訊息:

```
final messenger = ScaffoldMessenger.of(context);
messenger.showSnackBar(SnackBar(content: Text(‘I can fly.’)));
```

如您所想，其中原理不止於此，您可以觀看 Kate Lovett 釋出的 [關於 ScaffoldMessenger 的精彩影片](https://www.youtube.com/watch?v=sYG7HAGu_Eg) 瞭解詳情。

## 透過 "新增到應用" 功能建立多個 Flutter

在和許多 Flutter 開發者聊天的過程中，我們得知大多數人並非是從零開發一個全新應用，而是會透過將 Flutter 新增到現有的 iOS 和 Android 應用中來進行使用。我們將此功能稱為 [混合程式設計 (Add-to-App)](https://flutter.cn/docs/development/add-to-app)，您可以透過這種方法，在保留現有原生程式碼庫的同時，在兩個行動平台間重複使用 Flutter 程式碼。然而，我們有時聽到採用此方法的開發者們表示，他們不知如何擺脫只能將第一個畫面整合到 Flutter 的限制。Flutter 和原生頁面交織導致導航狀態難以維護，而且在檢視級別整合多個 Flutter 會佔用大量記憶體。

過去，額外 Flutter 例項的記憶體佔用量與第一個 Flutter 例項相同。在 Flutter 2 中，我們將建立額外 Flutter 引擎的靜態記憶體佔用量降低了約 99%，使每個例項的佔用量大約為 180kB。

![](https://devrel.andfun.cn/devrel/posts/2021/03/81b71e6e83d2d.gif)

提供該支援的新增 API 目前尚處於預覽狀態，在 [我們的文件裡](https://flutter.cn/docs/development/add-to-app/multiple-flutters)，您可以找到透過使用這種新模式的說明和 [範例專案](https://github.com/flutter/samples/tree/master/add_to_app/multiple_flutters)。隨著這一變化的出現，我們強烈建議您在原生應用中建立多個 Flutter 引擎例項。

## Flutter Fix

當任何成熟的框架聚集了擁有龐大程式碼庫的使用者時，我們往往需要避免對框架 API 進行任何更改，以避免破壞日益增多的程式碼。隨著超過 50 萬的 Flutter 開發者分佈在越來越多的平台，Flutter 2 很快就會踏入這一行列。然而，隨著時間的推移，為了持續改進 Flutter，我們希望能夠對 API 進行重大更改。現在的問題是，如何在不影響開發者的前提下繼續改進 Flutter API。

我們為此推出了 [Flutter Fix](https://flutter.cn/docs/development/tools/flutter-fix)。

Flutter Fix 是一系列功能的組合。首先，我們為 dart 命令列工具新增了一個名為 "dart fix" 的命令列選項，您可藉此尋找棄用 API 列表的所在位置，並瞭解如何更新呼叫這些 API 的程式碼。其次，Flutter Fix 本身就是個列表，自版本 2 開始便與 Flutter SDK 繫結。另外，Flutter Fix 也是一組針對 Visual Studio Code、IntelliJ 和 Android Studio IDE 的新 Flutter 擴充程式，您可藉此找到已棄用 API 的相同列表，單擊滑鼠，輕點旁邊的小燈泡圖示即可更改程式碼，完成快速修復。

舉個例子，比如您的應用具有下面一行程式碼:

![](https://devrel.andfun.cn/devrel/posts/2021/03/8c4cb573969e2.jpg)

> 使用已棄用的函式建立 Flutter widget

因為這個函式已經棄用，請使用下面的引數代替:

![](https://devrel.andfun.cn/devrel/posts/2021/03/78d1c0bbae193.jpg)

> 替換已棄用的函式並建立 Flutter widget

即使您熟悉和了解很多 Flutter 中已棄用的內容，程式碼中需要修改的內容越多，就越難修復所有的內容，也就越容易出現錯誤。人類並不擅長這種重複性的工作，但計算機不一樣。透過下面的命令，您可以看到我們如何在您的整個專案中進行問題修復:

```
$ dart fix --dry-run
```

如想批次應用它們，您亦可以透過以下程式碼輕鬆實現:

```
$ dart fix --apply
```

或者，如果您希望以互動方式在您喜歡的 IDE 中應用這些修復，也可以實現。

![](https://devrel.andfun.cn/devrel/posts/2021/03/a12a132d45d16.png)

多年來，我們一直在標記已棄用的舊 API，現在我們制定了一個策略，明確 [何時真正移除棄用的 API](https://medium.com/flutter/deprecation-lifetime-in-flutter-e4d76ee738ad)，而我們率先將其應用到了 Flutter 2 之中。儘管我們尚未捕捉到所有棄用 API，並將其以資料形式提供給 Flutter Fix，但我們會不斷從之前棄用的 API 中獲取更多內容，並在未來持續加入新的重大更改。我們的目標是盡全力將 Flutter API 打造的盡善盡美，同時保持程式碼的及時更新。

## Flutter DevTools

為了明確 DevTools 是用於除錯 Flutter 應用的工具，現在我們已將其命名為 Flutter DevTools。此外，我們還做了很多工作，讓其可以成為與 Flutter 2 成熟度與品質相匹配的版本。

其中有一個新功能，可在您尚未啟動 Flutter DevTools 2 時幫您鎖定問題，那就是您的 IDE 能夠發現常見的例外，並在 DevTools 中提出這個例外，以助您開展除錯。例如，下面顯示您的應用中丟擲了一個溢位例外，系統在 Visual Studio Code 中提供了一個在 DevTools 中除錯此問題的選項。

![](https://devrel.andfun.cn/devrel/posts/2021/03/608e8fe5872ec.png)

> Flutter IDE 擴充的溢位例外提示通知

按下該按鈕，您即可利用 DevTools 中的 Flutter Inspector 檢查引發問題的 Widget，以便進行修復。我們今天的操作只是為了解決溢位例外，但這種處理方法適用於 DevTools 可以解決的各種常見例外。

在 DevTools 開始執行後，您可透過標籤上的新錯誤標識幫助自己追蹤應用中出現的具體問題。

![](https://devrel.andfun.cn/devrel/posts/2021/03/0dd3177d5686b.png)

> DevTools 中的紅點可以幫助提醒應用中存在的錯誤部分

DevTools 的另一個新功能是能夠輕鬆發現所顯示的解析度低於其實際解析度的圖像，這有助於追蹤應用過大和記憶體佔用過多等情況。若要啟用此功能，請在 Flutter Inspector 中啟用 Invert Oversized Images。

![](https://devrel.andfun.cn/devrel/posts/2021/03/045ae1f47d21f.png)

> 啟用 "反轉超大尺寸圖像 (Invert Oversized Images)" 選項，以突出顯示例外圖像

現在，當圖像的實際解析度明顯大於其顯示大小時，系統就會將其倒置，以便您在應用中輕鬆找到它。

![](https://devrel.andfun.cn/devrel/posts/2021/03/4dc22b9b72fb5.png)

> "反轉超大圖像" 的操作範例

此外，為響應大量使用者的要求，除了在 Flutter Inspector 的 Layout Explorer 中顯示有關彈性佈局的詳細資訊外，我們還添加了顯示固定佈局的功能，可便於您除錯各種佈局。

![](https://devrel.andfun.cn/devrel/posts/2021/03/b11256af35d94.png)

> 新的 Layout Explorer 顯示了 fixed 和 flex layout 的佈局細節

其功能還遠不止如此。以下是對 Flutter DevTools 2 其他一些新增功能的總結:

* 為 Flutter 幀圖添加了平均 FPS 資訊和易用性改進
* 用紅色的錯誤標籤在網路分析器中呼叫失敗的網路請求
* 新的記憶體檢視圖表更快、更小、更簡單易用，其中包含用於在特定時間描述活動的懸浮卡片
* 在 "Logging (日誌庫)" 選項卡中新增了搜尋和篩選功能
* 從 DevTools 啟動之前開始追蹤日誌，以便在啟動後可以看到完整的日誌記錄
* 將 "Performance" 檢視重新命名為 "CPU Profiler"，以便更清楚地表示其功能
* 為 CPU Profiler 幀圖添加了時間網格
* 將 "Timeline" 檢視重新命名為 "Performance"，以便更清楚地表示其功能

當然這並不是全部。想了解所有相關更改，建議您查閱下列公告:

* [DevTools 0.9.4](https://groups.google.com/g/flutter-announce/c/mx_hBxuXM9Q/m/Kjy9dqS3AAAJ)
* [DevTools 0.9.5](https://groups.google.com/g/flutter-announce/c/mNqTNPUwBKw/m/_1qyXwTBAQAJ)
* [DevTools 0.9.6](https://groups.google.com/g/flutter-announce/c/Ta5HR39P3go/m/2a43w7iSCwAJ)
* [DevTools 0.9.7](https://groups.google.com/g/flutter-announce/c/IJ97oJ2HpxM/m/909J9Kl8AQAJ)
* [DevTools 2.0](https://groups.google.com/g/flutter-announce/c/0xQhJR4nQbI)

## Android Studio/IntelliJ Extension

我們也為 IntelliJ 系列 IDE 的 Flutter 外掛添加了一些適用於 Flutter 2 的新功能。首先，我們在其中新增了一個專案嚮導，該向導與 IntelliJ 中的新嚮導風格一致。

![](https://devrel.andfun.cn/devrel/posts/2021/03/e239fc16e88ab.png)

![](https://devrel.andfun.cn/devrel/posts/2021/03/41eb9c6d72ccc.png)

此外，如果您正在 Linux 上使用 IntelliJ 或 Android Studio，針對 [安裝自 Snap Store 的 Flutter SDK](https://snapcraft.io/flutter) 進行程式設計，那麼系統便會將 Flutter snap 路徑新增到已知的 SDK 路徑列表中。這使得 Flutter snap 使用者可以更輕鬆地在 "Settings (設定)" 中配置 Flutter SDK。感謝 Marcus Tomlinson 對此作出的貢獻！

![](https://devrel.andfun.cn/devrel/posts/2021/03/231b9293873b0.png)

> 透過 Snap 安裝 Flutter SDK，可以更輕鬆的在 Linux 上使用 Android Studio

您可以從最近更新公告中瞭解更多有用資訊:

* [IntelliJ Plugin M51](https://groups.google.com/g/flutter-announce/c/w65rD73R83Q/m/gV5p0qf2AAAJ)
* [IntelliJ Plugin M52](https://groups.google.com/g/flutter-announce/c/tQqqMOIg6V0/m/wj7Kbv4-AgAJ)
* [IntelliJ Plugin M53](https://groups.google.com/g/flutter-announce/c/V335xbsPWUs/m/14LSp05kAQAJ)
* [IntelliJ Plugin M54](https://groups.google.com/g/flutter-announce/c/-jYDrwG7PmA)

## Visual Studio Code 擴充

適用於 Visual Studio Code 的 Flutter 擴充也針對 Flutter 2 進行了最佳化，我們首先引入了一些測試增強功能，例如重新執行失敗測試的能力。

![](https://devrel.andfun.cn/devrel/posts/2021/03/7b6ddc9b0f723.png)

經過兩年的逐步發展，對 Dart 的 LSP (語言伺服器協議) 支援已經成為在 Flutter 擴充中將 Dart 分析器整合到 Visual Studio Code 中的預設方式。LSP 支援為 Flutter 開發帶來了許多改進，包括在當前的 Dart 檔案中應用特定的所有修復，以及能夠自動完成程式碼以產生完整函式呼叫 (包括括號和所需引數) 的能力。

![](https://devrel.andfun.cn/devrel/posts/2021/03/d2ba55aae416d.gif)

![](https://devrel.andfun.cn/devrel/posts/2021/03/dcd04901b23b0.gif)

LSP 支援不僅限於 Dart，它還支援 pubspec.yaml 及 analysis_options.yaml 檔案中的程式碼自動完成。

![](https://devrel.andfun.cn/devrel/posts/2021/03/dcd04901b23b0.gif)

這僅僅是近期 Visual Studio Code 適用於 Flutter 的部分擴充更新。您可以閱讀下列公告，瞭解全部更新內容:

* [Visual Studio Code 外掛 v3.16](https://dartcode.org/releases/v3-16/)
* [Visual Studio Code 外掛 v3.17](https://dartcode.org/releases/v3-17/)
* [Visual Studio Code 外掛 v3.18](https://dartcode.org/releases/v3-18/)
* [Visual Studio Code 外掛 v3.19](https://dartcode.org/releases/v3-19/)
* [Visual Studio Code 外掛 v3.20](https://dartcode.org/releases/v3-20/)

## DartPad 升級到支援 Flutter 2

如果不提 DartPad，那我們的工具更新就不能算完整，DartPad 現已更新並支援 Flutter 2。

![](https://devrel.andfun.cn/devrel/posts/2021/03/20cfd09d4ec0a.png)

> DartPad 已經升級到支援 Flutter 2 了

現在，無需離開喜歡的瀏覽器，您就可以體驗新的 Flutter 空安全版本。

## 生態系統更新

Flutter 開發體驗不僅包括框架和工具；還包括為 Flutter 應用提供的各種軟體套件和外掛。自上一次 Flutter 穩定版本釋出以來，這方面也發生了很多變化。例如，我們已在攝影頭和影片播放器外掛之間合併了將近 30 個 PR，從而大大提高了兩者的品質。如果您在過去使用這兩種產品時曾遇到過問題，那麼您應該再嘗試一次，您會發現它們比以前更加強大。

另外，如果您是 Firebase 使用者，我非常高興地宣佈熱門外掛的產品品質已經得到了提高並可投入生產，同時我們還為這些外掛提供了空安全支援以及針對 Android、iOS、web 和 macOS 的 [全套參考文件和常用課程](http://firebase.flutter.dev)。這些外掛包括:

* Core
* Authentication
* Cloud Firestore
* Cloud Functions
* Cloud Messaging
* Cloud Storage
* Crashlytics

如果您正在尋找應用的崩潰報告，您可以考慮使用 Sentry，其已經發布了 [適用於 Flutter 應用的新 SDK](https://blog.sentry.io/2021/03/03/with-flutter-and-sentry-you-can-put-all-your-eggs-in-one-repo/)。

![Sentry 崩潰報告工具已經支援 Flutter](https://devrel.andfun.cn/devrel/posts/2021/03/7d5f73ad3037b.jpg)

> Sentry 崩潰報告工具已經支援 Flutter

在 Flutter 應用中使用 Sentry 的 SDK，您可以在 Android、iOS 或原生平臺上發生錯誤時收到即時通知。查閱 [Sentry 文件](https://docs.sentry.io/platforms/flutter/) 瞭解更多詳細資訊。

另外，如果您尚未了解過 [Flutter 社群中的 "Plus" 外掛](http://plus.fluttercommunity.dev/)，您也可著手嘗試。Flutter 團隊最初開發的許多熱門外掛均由此衍生而來，我們已在 Plus 外掛中添加了空安全支援和對其它平台的支援，並已著手開始解決 flutter/plugins 庫中的相應問題。這些外掛包括:

* Android Alarm+
* Android Intent+
* Battery+
* Connectivity+
* Device Info+
* Network Info+
* Package Info+
* Sensors+
* Share+

目前，與 Flutter 相容的 package 和外掛數量超過 15,000 個，這會讓人很難找到那些值得優先考慮的軟體套件和外掛。因此，我們釋出了 Pub 分值 (靜態分析評分)、人氣排名、喜愛度，若軟體包質量出眾，我們會為其打上 "[Flutter Favorite](https://flutter.cn/docs/development/packages-and-plugins/favorites)" 的特殊標記。為與 Flutter 2 適配，我們已在 Favorite 列表中添加了幾個新軟體包:

* animated_text_kit
* bottom_navy_bar
* chopper
* font_awesome_flutter
* flutter_local_notifications
* just_audio

祝賀這些軟體套件的作者！如果您尚未了解這些軟體包或 [列表中的其它軟體包](https://pub.flutter-io.cn/flutter/favorites)，建議您著手開始瞭解。

最後同樣也是很重要的一點，如果軟體包作者或使用者有興趣瞭解最新版本的軟體包是否適用於最新版本的 Flutter，可以檢視 Codemagic 的新 pub.green 網站以瞭解詳情。

![](https://devrel.andfun.cn/devrel/posts/2021/03/59ffdf68da593.jpg)

Codemagic 釋出了一個網站 pub.green 用來展示 package 和近期 Flutter 版本的相容性結果

您可在 [pub.green](http://pub.green/) 網站上測試 pub.dev 上可用的 Flutter 和 Dart 軟體與不同 Flutter 版本的相容性。瞭解更多資訊，推薦查閱 [CodeMagic 團隊的公告博文](https://blog.codemagic.io/pub-green/)。

## 重大變更

我們為 Flutter 2 做了如下重大變更，您可利用 "dart fix" 命令自動緩解其中的許多內容:

* [61366 繼續對 clipBehavior 進行重大變更](https://github.com/flutter/flutter/pull/61366)
* [66700 將預設 FittedBox 的 clipBehavior 更改為無](https://github.com/flutter/flutter/pull/66700)
* [68905 從 Cupertino 顏色 API 中移除 nullOk 引數](https://github.com/flutter/flutter/pull/68905)
* [69808 從 Scaffold.of 和 ScaffoldMessenger.of 中移除 nullOk 引數，為兩者建立 maybeOf](https://github.com/flutter/flutter/pull/68908)
* [68910 從 Router.of 中移除 nullOk 引數，使其返回非空值](https://github.com/flutter/flutter/pull/68910)
* [68911 在 Localizations 中加入 maybeLocaleOf 方法](https://github.com/flutter/flutter/pull/68911)
* [68736 從Media.queryOf 中移除 nullOK 引數](https://github.com/flutter/flutter/pull/68736)
* [68917 從 Focus.of, FocusTraversalOrder.of 和 FocusTraversalGroup.of 中移除 nullOK](https://github.com/flutter/flutter/pull/68917)
* [68921 從 Shortcuts.of, Actions.find, and Actions.handler 中移除 nullOK](https://github.com/flutter/flutter/pull/68921)
* [68925 從 AnimatedList.of 和 SliverAnimatedList.of 中移除 nullOK](https://github.com/flutter/flutter/pull/68925)
* [69620 從 BuildContext 中刪除已棄用的方法](https://github.com/flutter/flutter/pull/69620)
* [72017 刪除已棄用的 CupertinoTextThemeData.brightness](https://github.com/flutter/flutter/pull/72017)
* [72395 刪除已棄用的 [PointerEnterEvent, PointerExitEvent].fromHoverEvent](https://github.com/flutter/flutter/pull/72395)
* [72532 刪除已棄用的 showDialog.child](https://github.com/flutter/flutter/pull/72532)
* [72890 刪除已棄用的 Scaffold.resizeToAvoidBottomPadding](https://github.com/flutter/flutter/pull/72890)
* [72893 刪除已棄用的 WidgetsBinding.[deferFirstFrameReport, allowFirstFrameReport]](https://github.com/flutter/flutter/pull/72893)
* [72901 刪除已棄用的 StatefulElement.inheritFromElement](https://github.com/flutter/flutter/pull/72901)
* [72903 刪除已棄用的元素方法](https://github.com/flutter/flutter/pull/72903)
* [73604 刪除已棄用的 CupertinoDialog](https://github.com/flutter/flutter/pull/73604)
* [73745 從 Cupertino[Sliver]NavigationBar 中刪除已棄用的 actionsForegroundColor](https://github.com/flutter/flutter/pull/73745)
* [73746 刪除已棄用的 ButtonTheme.bar](https://github.com/flutter/flutter/pull/73746)
* [73747 刪除 Span 棄用項](https://github.com/flutter/flutter/pull/73747)
* [73748 刪除已棄用的 RenderView.scheduleInitialFrame](https://github.com/flutter/flutter/pull/73748)
* [73749 刪除已棄用的 Layer.findAll](https://github.com/flutter/flutter/pull/73749)
* [75657 在 Localizations.localeOf 移除殘留的 nullOK 引數](https://github.com/flutter/flutter/pull/74657)
* [74680 從 Actions.invoke 移除 nullOK 引數，並新增 Actions.maybeInvoke 方法](https://github.com/flutter/flutter/pull/74680)

## 總結

最後，我想代表 Google Flutter 團隊全體成員向開發者們說一句謝謝！感謝你們在過去的兩年裡推出了超過 15 萬款 Flutter 應用，我們整理了一些團隊喜歡的 Flutter 應用，在 [Flutter Engage 主題演講](https://zhuanlan.zhihu.com/p/355036482) 上播放了這段影片，獻給各位開發者和社群。

![](https://devrel.andfun.cn/devrel/posts/2021/03/638471b80bffd.jpg)

如果沒有各位開發者們對自己建構的 Flutter 應用的持續支援和熱忱，這個具備全球活力的開發者社群就不可能成為現實，我們非常期待看到您接下來的作品！
