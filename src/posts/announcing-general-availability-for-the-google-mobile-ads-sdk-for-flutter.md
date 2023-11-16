---
title: 適用於 Flutter 的 Google 移動廣告 SDK 正式版現已釋出
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/12/yWS0zM.png)

*作者 / Zoey Fan，Flutter 產品經理*

應用變現有多種方法: 透過實體企業的店面接受付款、提供訂閱或應用內購買，或者直接在應用中投放廣告。經過六個月的 beta 測試期，我們很高興能夠推出 [Google 移動廣告 SDK (Flutter)](https://pub.flutter-io.cn/packages/google_mobile_ads) 正式版。這對需要應用內廣告的 Flutter 應用來說是個好訊息！

## **支援的廣告格式**

適用於 Flutter 的 Google 移動廣告 (GMA) SDK 可在 iOS 和 Android 上執行，支援載入和顯示所有的 [Google 移動廣告格式](https://developers.google.cn/admob/flutter/quick-start)，包括：

**橫幅廣告 (Banner Ads):** 在應用佈局中出現的矩形廣告。在使用者與應用互動時，這些廣告可以錨定在螢幕的頂部或底部，也可以嵌入到使用者滾動的內容中。除了標準固定尺寸的橫幅廣告，我們還支援自適應的橫幅廣告，它會基於裝置的寬度和高度選擇最佳的橫幅尺寸。

**插頁式廣告 (Interstitial Ads):** 覆蓋應用整個螢幕的全屏廣告。這些廣告適合放置在應用的自然停頓點或轉場中。

**激勵影片廣告 (Rewarded Video Ads):** 一種激勵廣告單元，讓使用者透過與影片廣告、試玩廣告、或參與問卷調查進行互動來換取應用內商品的獎勵。這是最受遊戲開發者歡迎的廣告格式之一。

**原生廣告 (Native Ads):** 一種高度可客製的格式，可用來設計匹配應用內容外觀和特質的廣告。

**應用開屏廣告 (App Open Ads):** 一種讓移動應用載入體驗得以變現的廣告格式。當用戶開啟或切換回應用時，會展示開屏廣告。

![](https://devrel.andfun.cn/devrel/posts/2021/12/6UpIut.png)

## **Google AdMob 和 Ad Manager**

我們和 Google Ads 團隊合作開發了這個外掛，作為 Flutter 開發者的官方廣告解決方案。Flutter GMA SDK 透過一個外掛整合了對 **Google AdMob 和 Google Ad Manager** 的支援。

如果您不熟悉 Google 的廣告服務，您可能不知道 [AdMob](https://admob.google.cn/intl/zh-CN_cn/home/) 和 [Ad Manager](https://admanager.google.com/intl/zh-CN_cn/home/) 是兩種不同的產品，具有不同的變現功能。AdMob 平台專為想要透過廣告獲利並獲得切實可行的洞察以發展應用業務的行動開發者設計。Ad Manager 平台專為擁有大量直銷或多種庫存型別的釋出商設計。

[Flutter GMA SDK](https://pub.flutter-io.cn/packages/google_mobile_ads) 統一了這些產品，透過在 iOS 和 Android 平台共享程式碼來滿足您的變現需求，且無需在需求增長時重寫程式碼。

![](https://devrel.andfun.cn/devrel/posts/2021/12/VGvTlQ.png)

## **中介和競價**

此版本還包括一個新中介功能的預覽，以幫助您最佳化廣告效果。[中介](https://developers.google.cn/admob/flutter/mediation/get-started) (Mediation) 可以幫助您在一個地方統一管理用於向您的應用投放廣告的多個廣告源。除了 Google 的廣告投放需求，中介還可以讓您展示來自非 Google 廣告網路的廣告。透過中介功能，您可以將傳入的廣告請求傳送給多個廣告源，並找出最佳的可用來源以滿足請求。除了傳統的中介功能，該外掛還支援競價，讓廣告源透過即時競價來滿足您的廣告請求。這可以確保您從廣告展示中獲取最高的收入。

Flutter GMA SDK 為 iOS 和 Android 應用提供相同的廣告功能。您可以使用相同的工具來管理廣告計劃，追蹤廣告效果，等等。

## **早期適配的開發者**

我們一直在與一些早期適配的開發者合作，他們熱切希望在我們的正式版釋出之前就開始使用 Flutter 提供的廣告支援。其中一位開發者是 Lotum，他們最近重新編寫了其廣受好評的文字遊戲 "[4 Pics 1 Word](https://play.google.com/store/apps/details?id=de.lotum.whatsinthefoto.us&hl=en_US&gl=US)"，這是一款在 50 個國家/地區獲得巨大成功的應用，僅 Android 版本的安裝量就超過 5,000 萬。他們選擇使用 Flutter 來進行重寫，用 Flutter GMA SDK 來展示插頁式廣告和激勵影片。

該應用的 Flutter 開發者 Petra Langenbacher 如此說道:

> 多年來，我們一直在打磨和最佳化我們的應用，我們曾擔心大規模重寫會影響我們的收入或使用者群。但是我們驚喜地發現，情況並不是這樣: 我們能夠在不造成任何負面影響的前提下做出這些改變！

感謝 Lotum 和所有其他早期適配的開發者為我們提供的寶貴反饋。非常感謝大家為初始版本提供的幫助。

## **其他變現功能**

除了廣告，Flutter 也提供了應用變現的其他方法。例如，[應用內購買](https://pub.flutter-io.cn/packages/in_app_purchase) (In-App Purchase) 外掛可以讓您在應用中提供額外內容，包括高階服務、數字商品和訂閱。Flutter 的 [Pay](https://pub.flutter-io.cn/packages/pay) 外掛可以讓您的應用在 Android 裝置上整合 Google Pay 以及在 iOS 上整合 Apple Pay，從而讓您快速輕鬆地支援這兩個平台，為您的使用者打造順暢的付款體驗，來購買日用品、零售商品和食品外賣等。

要了解 Ads 的詳情或其他變現功能，請前往我們 [最近更新的網站](https://flutter.cn/monetization) 檢視範例、Codelab 和文件。

建構 Flutter 應用僅僅只是開始。我們希望您能善加利用這一系列變現功能，透過 Flutter 打造成功的業務！
