---
title: Flutter 2 正式釋出！
toc: true
---

3月4日凌晨，**Flutter 2** 正式釋出: 開發者為*任何*平台建立美觀、快速且可移植應用的能力得以更上一層樓。透過 Flutter 2，您可以使用相同的程式碼庫為五種作業系統建構原生應用: iOS、Android、Windows、macOS 和 Linux；以及為 Chrome、Firefox、Safari 和 Edge 等瀏覽器打造 web 體驗。Flutter 甚至可以嵌入到汽車、電視和智慧家電，為環境計算提供最普適、可移植的體驗。

![](https://devrel.andfun.cn/devrel/posts/2021/03/0939e5e734a5c.png)

我們的目標是*從根本上改變*開發者的應用建構思路，讓體驗先於平台。Flutter 可以讓您盡情雕琢**精美**的應用體驗，暢快表達自己的品牌和設計風格。Flutter 可以將您的原始碼編譯為機器碼，並**快速**執行，同時藉助有狀態熱重載功能為您帶來了解釋環境的**高效**生產力，讓您可以在應用執行時做出更改並立即檢視結果。而且 Flutter 是**開源**的，有成千上萬的貢獻者在為核心框架添磚加瓦，並圍繞其打造了包含海量 package 的生態系統。

![](https://devrel.andfun.cn/devrel/posts/2021/03/f090329f06af7.png)

在 [釋出](https://flutter.dev/) 的 Flutter 2 中，我們將 Flutter 從移動框架擴充為**可移植**框架，讓您的應用基本可以不加變動地在多種平臺上執行。僅 Google Play 商店就已有**超過 15 萬款 Flutter 應用**，而現在應用可以在無需重寫的情況下部署到桌面裝置和 web 平台，可以說每款應用都透過 Flutter 2 得到 "免費升級"。

世界各地的使用者都在使用 Flutter，包括 [WeChat](https://apps.apple.com/us/app/wechat/id414478124)、[Grab](https://play.google.com/store/apps/details?id=com.grabtaxi.passenger)、[Yandex Go](https://play.google.com/store/apps/details?id=ru.yandex.taxi)、[Nubank](https://play.google.com/store/apps/details?id=com.nu.production)、[Sonos](https://apps.apple.com/us/app/sonos/id1488977981)、[Fastic](https://apps.apple.com/us/app/fastic-fasting-app/id1459260306)、[Betterment](https://play.google.com/store/apps/details?id=com.betterment) 和 [realtor.com](https://apps.apple.com/US/app/id336698281?mt=8) 等熱門應用。Flutter 在 Google 也是備受依賴的重要工具，我們有一千多名工程師正在使用 Dart 和 Flutter 建構應用。許多產品已經發布，包括 Stadia、Google One 和 Google Nest Hub。

![](https://devrel.andfun.cn/devrel/posts/2021/03/458afcd4aaecf.png)

幾個月前，[Google Pay 改用 Flutter](https://mp.weixin.qq.com/s/2y3dWbA4uZbwfTx_bOGdSQ) 打造其旗艦移動應用，在生產力和品質上取得重大進步。透過統一程式碼庫，團隊消除了平台之間的功能差異，精簡了超過 50 萬行程式碼。Google Pay 還表示，其工程師效率大幅提高，技術債務顯著減少，並在 iOS 和 Android 系統上統一了安全審查和實驗等釋出流程。

## Flutter web 支援

也許 Flutter 2 帶來的最重大的進展是**在 web 平臺達到了穩定版**。

Web 的 [早期基礎以文件為中心](https://tools.ietf.org/html/rfc1866)。但 web 平台經過發展，所包含的平台 API 也越發豐富，已實現高度複雜的應用，支援硬體加速 2D 和 3D 圖形以及靈活的佈局和繪畫 API。Flutter 的 web 支援建立在這些創新的基礎上，提供了**一個以應用為中心** **的框架**，能充分展現現代 web 的所有優勢。

這次釋出帶來的初始版本特別關注三種應用場景:

* **漸進式 web 應用 (Progressive web apps, PWA)** ，兼具 web 的高覆蓋面與桌面應用的強大功能。
* **單頁應用 (Single page apps, SPA)** ，只需載入一次即可與網際網路服務互傳資料。
* **將現有 Flutter 移動應用帶到 web** ，使兩種體驗共享程式碼。

在過去幾個月準備釋出穩定版 web 支援的過程中，效能最佳化方面也取得了許多進展，增加了由 [WebAssembly](https://webassembly.org/) 建構的由 [CanvasKit](https://skia.org/user/modules/canvaskit) 驅動的新渲染引擎。[Flutter Plasma](https://flutterplasma.dev) 是由社群成員 [Felix Blaschke](https://felixblaschke.medium.com/) 建構的示範，展示了使用 Dart 和 Flutter 建構複雜 web 圖形體驗的便利性，而且這些體驗也可以在桌面或移動裝置上原生執行。

我們不斷擴充 Flutter，力求為大家提供 web 平台最棒的功能。最近幾個月，我們增加了文字自動填充、位址列 URL 和路由控制以及 PWA 清單。由於桌面瀏覽器與移動瀏覽器同樣重要，我們添加了互動式捲軸和鍵盤快捷鍵，提升了桌面模式下的預設內容密度，併為 Windows、macOS 和 Chrome OS 增加了螢幕閱讀器無障礙功能支援。

目前已經出現了一批使用 Flutter 建構的 web 應用範例。在教育工作者中，[iRobot](https://www.irobot.com/) 以其廣受歡迎的 [Root 教育機器人](https://www.irobot.com/Root) 而聞名。Flutter 在 web 平台對生產環境的支援，使 iRobot 能夠將現有的 [教育程式設計環境](https://edu.irobot.com/what-we-offer/irobot-coding) 搬到 web 上，從而覆蓋到了 Chromebook 以及其他以瀏覽器為主要體驗的裝置。iRobot 的 [博文](https://edu.irobot.com/the-latest/building-a-coding-experience-for-all) 詳盡闡述了選擇 Flutter 的理由以及使用情況。

![](https://devrel.andfun.cn/devrel/posts/2021/03/3bbba28798746.png)

另一個例子是 Rive，他們為設計師打造的強大工具可以將建立的自訂動畫釋出到任意平台。其 [新版 web 應用](https://rive.app/) 完 全由 Flutter 建構，現已進入 Beta 階段，這也是 web 平台最能展現 Flutter 特色的體驗之一。

![](https://devrel.andfun.cn/devrel/posts/2021/03/384a963e53119.png)

您可以從 [Flutter web 釋出文章](https://medium.com/flutter/web-post-d6b84e83b425) 中瞭解更多資訊。

## 桌面、可摺疊和嵌入式裝置上的 Flutter 2

除了傳統的移動裝置和 web 之外，Flutter 正越來越多地覆蓋到其他型別裝置。[Flutter Engage](https://zhuanlan.zhihu.com/p/355036482) 主題演講中重點介紹了三位合作伙伴，以展示 Flutter 的可移植性。

第一位合作伙伴，**Canonical** 與我們聯手將 Flutter 帶到桌面，Canonical 工程師貢獻的程式碼使得 Flutter 開發者能在 Linux 上進行開發和部署。在活動中，Ubuntu 團隊展示了使用 Flutter 重寫的新安裝程式的早期示範版。對 Canonical 來說，在多種硬體配置上提供穩如磐石且美妙的體驗至關重要。未來，**Flutter 將成為 Canonical 打造桌面和移動應用的首選**。

![](https://devrel.andfun.cn/devrel/posts/2021/03/74e068704f2a5.png)

第二位合作伙伴 **Microsoft** 正在繼續擴大對 Flutter 的支援。除了 [持續與我們合作](https://github.com/flutter/flutter/issues/14967#issuecomment-787678757) 為 Flutter 提供高品質的 Windows 支援外，Microsoft 還發布了對 Flutter 引擎的貢獻: 支援新興的可摺疊 Android 裝置。這些裝置帶來了新的設計模式，應用可以擴充內容，或者利用雙屏特性提供視窗並排體驗。Surface 工程團隊在一篇 [博文](https://devblogs.microsoft.com/surface-duo/flutter-dual-screen-foldable/) 中展示了其工作成果，並邀請大家加入他們的行列，共同完成適用於 Surface Duo 和其他裝置的高品質解決方案。

![](https://devrel.andfun.cn/devrel/posts/2021/03/781360ee27de7.png)

第三位合作伙伴，全球暢銷汽車製造商之一 **Toyota** 宣佈，計劃建構由 Flutter 驅動的資訊娛樂系統，打造最佳的汽車數字體驗。使用 Flutter 標誌著車載軟體的開發方式向著未來邁進了一大步。Toyota 之所以選擇 Flutter，是因為其高效能和體驗的一致性，快速迭代的能力和極高的開發者工效，以及智慧手機級別的觸控體驗機制。透過使用 Flutter 的嵌入器 API，Toyota 能夠根據車載系統的獨特需求對 Flutter 進行客製。

![](https://devrel.andfun.cn/devrel/posts/2021/03/8f5850ecec2bd.png)

我們很榮幸與 Toyota 等合作伙伴繼續深入協作，將 Flutter 帶到汽車、電視和其他嵌入式裝置中，在未來幾個月我們會為大家帶來更多這方面的範例。

## 持續發展的 Flutter 生態系統

目前，Flutter 和 Dart 有超過 15,000 個 package: 包括 [Amazon](https://pub.dev/publishers/aws-amplify.com/packages)、[Microsoft](https://pub.dev/publishers/microsoft.com/packages)、[Adobe](https://pub.dev/publishers/adobe.com/packages)、[Alibaba](https://pub.dev/publishers/community.opensource.alibaba.com/packages)、[eBay](https://pub.dev/publishers/ebay.com/packages) 和 [Square](https://pub.dev/packages/square_in_app_payments) 等企業 package，[Lottie](https://pub.dev/packages/lottie)、[Sentry](https://pub.dev/packages/sentry_flutter) 和 [SVG](https://pub.dev/packages/flutter_svg) 等關鍵 package，以及 [sign_in_with_apple](https://pub.dev/packages/sign_in_with_apple)、[google_fonts](https://pub.dev/packages/google_fonts)、[geolocator](https://pub.dev/packages/geolocator) 和 [sqflite](https://pub.dev/packages/sqflite) 等 [Flutter Favorite](https://flutter.tw/development/packages-and-plugins/favorites) package。

我們也為 Flutter 帶來了 [Google Mobile Ads ](https://pub.dev/packages/google_mobile_ads) Beta 版，這款全新 SDK 透過 AdMob 和 AdManager 提供多種廣告格式，包括橫幅式、插頁式、原生和激勵影片廣告。我們之前已經邀請了一些主要客戶 (比如拉丁美洲最大的獨立藝術家音樂平台 [Sua Música](https://www.suamusica.com.br/)) 先行體驗此 SDK，現在計劃開放 Google Mobile Ads 供更多的 Flutter 開發者採用。

![圖片](https://devrel.andfun.cn/devrel/posts/2021/03/59e21f0feea5d.png)

我們還更新了 [幾項核心 Firebase 服務的 Flutter 外掛](https://firebase.flutter.dev/): Authentication、Cloud Firestore、Cloud Functions、Cloud Messaging、Cloud Storage 和 Crashlytics，包括對健全空安全的支援以及對 Cloud Messaging package 的全面改版。

## Dart: Flutter 背後的秘訣

如前所述，Flutter 2 可以移植到許多不同的平臺和裝置上。之所以能輕鬆過渡到支援 web、桌面和嵌入式裝置，這在很大程度上要歸功於 [Dart](https://dart.tw.gh.miniasp.com)，它是 Google 為多平臺開發最佳化的程式語言。

Dart 為建構應用提供了一套獨特的功能:

* **無意外的可移植性** ，編譯器可為移動和桌面裝置產生高效能的 Intel 和 ARM 機器程式碼，併為 web 輸出嚴密最佳化過的 JavaScript。相同的 Flutter 框架原始碼可編譯到所有這些目標平台。
* 在桌面和移動裝置上進行**有狀態熱重載的迭代開發**，以及為現代介面程式設計的非同步、併發模式設計的語言結構。
* 全平台一致的 **Google 級效能**，健全空安全保證了執行時以及開發時的空約束。

沒有其他語言可以同時提供這些功能，也許這就解釋了為什麼 Dart 能成為 [GitHub 上發展最快的語言](https://madnight.github.io/githut/#/pull_requests/2020/4) 之一。

我們同期釋出的 Dart 2.12 是自 2.0 以來最重大的版本更新，支援**健全的空安全**。健全的空安全能掃除令人頭疼的空參考例外，除非開發者明確允許，否則型別在開發時和執行時不可能包含空值。最重要的是，此功能並非重要改動 (breaking change): 您可以按照自己的節奏將空安全逐步新增到程式碼中，我們也準備好了遷移工具，您可以在準備好之後使用它完成遷移。

這一版本的更新還包括: [FFI 的穩定版本](https://dart.tw.gh.miniasp.com/guides/libraries/c-interop)，讓您可以編寫出高效能的程式碼與基於 C 語言的 API 進行互操作；使用 Flutter 編寫的 [新的整合開發者和效能剖析器工具](https://flutter.tw/development/tools/devtools/overview)；以及許多效能改進和尺寸最佳化，只需重新編譯即可讓程式碼得到長足的改進。如需瞭解詳細資訊，請檢視 [Dart 2.12 釋出文章](https://medium.com/dartlang/announcing-dart-2-12-499a6e689c87)。

## 即刻體驗 Flutter 2

在介紹 Flutter 2 時，本文由於篇幅限制難免掛一漏萬。事實上，被合併的 PR (pull request) 清單就有足足 200 頁！請閱讀 [Flutter 2 技術博文](https://medium.com/@csells_18027/fe8e95ecc65)，裡面介紹了更多新功能和效能改進，我們認為會讓 Flutter 開發者感到滿意，也請大家立即下載 Flutter 2 開始體驗。

![](https://devrel.andfun.cn/devrel/posts/2021/03/46794d1264d83.png)

我們還為大家準備了一款全新的範例應用，[Flutter Folio](https://flutterfolio.com)，由我們與加拿大埃德蒙頓的獲獎設計團隊 [gskinner](https://gskinner.com/) 合作完成，這個範例展示了我們剛剛提到的一切內容。Flutter Folio 是一款能在您所有的裝置上執行的剪貼簿應用。在小螢幕上體驗時會強調展示內容；而在大螢幕上體驗時則支援以桌面和平板電腦的習慣用法進行內容編輯；web 體驗則著重強調分享。這些客製化的體驗都共享相同的開原始碼庫，可供您自由瀏覽。

![](https://devrel.andfun.cn/devrel/posts/2021/03/7ef1cc3624d4e.png)

如果您尚未嘗試過 Flutter，我們相信它將為您的應用開發體驗帶來巨大的提升。Flutter，一個開源工具套件，讓您透過單一程式碼庫為移動、桌面、web 和嵌入式裝置打造美觀、快速的應用，讓您即便是在面對 Google 和廣大使用者的苛刻需求時也能遊刃有餘。

而且 Flutter 是免費和開源的。您會使用 Flutter 2 建構怎樣精彩的應用呢？我們拭目以待！