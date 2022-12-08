---
title: Flutter 3 正式釋出
toc: true
keywords: Flutter版本釋出, Flutter正式版
description: Flutter 3 加入了對 macOS 和 Linux 桌面端的穩定版支援！也包括 Firebase、生產力和效能相關的更新。
image:
    path: https://files.flutter-io.cn/posts/flutter-cn/2022/introducing-flutter-3/flutter3_hero.png
---

![](https://files.flutter-io.cn/posts/flutter-cn/2022/introducing-flutter-3/flutter3_hero.png)

_作者 / Tim Sneath, Google Flutter 和 Dart 產品組產品經理_

Flutter 3 實現了 Flutter 以移動端為中心擴充到多平臺的產品規劃，並在今年 I/O 大會的主題演講上正式釋出，此次釋出提供了對 macOS 和 Linux 桌面端的穩定版支援，同時包括 Firebase 整合的改進，增加了與生產力和效能相關的新特性，並對 Apple 晶片提供了支援。

## Flutter 3 的演進

建立 Flutter 的初衷是為了徹底改變應用開發：將 「Web 應用的迭代開發模式」與「硬體加速的圖形渲染」和「畫素級的控制」三者結合——之前只有遊戲才能這麼做。自 Flutter 1.0 Beta 版釋出以來的四年裡，我們逐漸在這些基礎上發展，增加了新的框架功能和新的 widget，與底層平台進行了更深入的整合，還加入了豐富的 package 庫，此外還進行了許多效能和工具等方面的改進。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/introducing-flutter-3/flutter-milestones.png)

隨著 Flutter 逐漸產品走向成熟，越來越多的人開始用它來建構應用。今天，有超過 50 萬個應用使用 Flutter 釋出。來自 data.ai 等研究公司的分析以及公眾的評價都可以表明，Flutter 正被許多細分領域的客戶所使用。其使用場景非常廣泛，從微信 (WeChat) 等社交應用，到 Betterment 和 Nubank 等金融和銀行類應用，再到 SHEIN 和 trip.com 等商旅應用以及 Fastic、[Tabcorp](https://auspreneur.com.au/tabcorp-adopts-googles-flutter-platform/ "Tabcorp 使用 Google Flutter 平台進行建構") 等生活方式類別的應用，還有 [My BMW](https://www.press.bmwgroup.com/global/article/detail/T0328610EN/the-my-bmw-app:-new-features-and-tech-insights-for-march-2021 "My BMW 應用: 2021 年 3 月的新特性更新和技術說明") 此類別的硬體連線類應用，最後是巴西政府等公共機構的官方應用等等，都有 Flutter 在大放異彩。

<highlight>現在已有超過 50 萬個應用使用 Flutter 建構。</highlight>

開發者告訴我們，Flutter 幫助他們在更多的平臺上更快地構建出了精美的應用。我們最新的使用者研究也表明:

- 91\% 的開發者認為 Flutter 縮短了建構和釋出應用的時間。
- 85\% 的開發者認為 Flutter 使他們的應用比以前更加精美。
- 85\% 的開發者認為 Flutter 使他們的應用能在更多平臺上釋出。

Sonos 在 [最近的一篇部落格文章](https://tech-blog.sonos.com/posts/renovating-setup-with-flutter/ "最近的一篇部落格文章") 中討論了他們對設定體驗的改版工作，其中著重強調了上述的第二點：

> 毫不誇張地說，Flutter 為我們帶來了一種 "高階感"，這與我們團隊之前提供的任何東西都不一樣。對我們的設計師來說，最重要的是，他們可以輕鬆地建構新的使用者介面，這意味著我們團隊在面對各種設計規格時，會更少說 "做不到"，而更多地直接進行迭代。如果您覺得我們的說法有道理，我們推薦您試試 Flutter —— 至少我們非常慶幸當初選擇了它。

## 歡迎來到 Flutter 3

Flutter 3 正式釋出，至此，Flutter 的跨平臺之旅邁入高潮。有了 Flutter 3，您就可以僅用一套程式碼庫，為 6 個平台建構精美的體驗。它為開發者提供了無與倫比的生產力，並使初創團隊從第一天起就能將新的想法投放到完全可用的市場中。

在以前的版本中，我們已經在 iOS 和 Android 平台之外增加了 [Web 端](https://mp.weixin.qq.com/s/6oSwvPsMy6r4AW90aostiA) 和 [Windows 平台](https://mp.weixin.qq.com/s/GSsym9zSYusZkrNLI2a6fQ) 的支援，現在，**Flutter 3 增加了對 macOS 和 Linux 應用的穩定支援**。增加平台支援需要的不僅僅是渲染畫素這麼簡單: 還包括對新的輸入和互動模型的支援、編譯和建構的支援、無障礙和國際化支援，以及特定平台的整合。我們的目標是，讓您能夠靈活地充分利用底層作業系統，同時根據您的選擇儘可能多地共享使用者介面和邏輯。

在 macOS 上，我們已經投入資源支援 Intel 處理器和 Apple 晶片，並提供 [通用二進位制 (Universal Binary) 檔案](https://developer.apple.com/documentation/apple-silicon/building-a-universal-macos-binary "Apple 開發者文件: 建構通用 mac OS 二進位制檔案")) 的支援，使應用能夠打包成在兩種架構上都能原生執行的可執行檔案。在 Linux 上，Canonical 和 Google 已經合作為開發者們帶來了高度整合的最佳開發工具。

[Superlist](https://superlist.com/ "團隊生產力提高工具 Superlist") 是一個很好的例子，告訴大家 Flutter 是如何幫助您實現精美桌面體驗的，它今天推出了 Beta 版本供大家體驗。Superlist 提供了超強的協作功能，透過一個嶄新的應用將列表、任務和自由形式的內容結合在一起，將待辦事項和個人計劃等功能打磨得煥然一新。Superlist 團隊選擇 Flutter，是因為它能夠提供快速且高度品牌化的桌面體驗。我們認為，他們迄今為止的進展表明這是一個非常明智的選擇。

Flutter 3 還對許多基本要素進行了改進，提高了效能，強化了對 Material You 的支援，並進一步提升了生產力。

除了上面提到的內容外，在這個版本中，Flutter 已經完全支援在 [Apple 晶片](https://support.apple.com/zh-cn/HT211814 "搭載 Apple 晶片的 Mac 電腦") 上進行原生開發。雖然自 M1 處理器誕生以來，Flutter 就一直與搭載 M1 的蘋果裝置相容，但 Flutter 現在充分利用了 [Dart 對 Apple Silicon 的支援](https://mp.weixin.qq.com/s/mogv7U94WdZQ5Wd_S0kXLg)，在搭載 M1 的裝置上實現了更快的編譯，並支援 macOS 應用的通用二進位制檔案。

在這個版本中，我們對 [Material Design 3](https://m3.material-io.cn "Material Design 3 主頁") 所做的支援工作已基本完成。開發者們現在可以盡情使用這套自適應性強、跨平臺的設計系統，包括其動態顏色方案和經過更新的視覺元件。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/introducing-flutter-3/m3-support.png)

我們也將在近期釋出更詳細的技術文章，並在其中更多介紹這部分內容，以及 Flutter 3 的許多其他新功能。

Flutter 由 Dart 程式語言提供強有力的支援，這是一種用於多平臺開發的高生產力、可移植的語言。我們在這個釋出週期中對 Dart 的改進工作包括: 列舉支援成員變數、改進的超類引數繼承，以及更為靈活的命名引數相關的新的語言特性；同時為 `package:lints` 開啟了 2.x 版本，這是一套官方的 lint 規則，是根據我們總結的 Dart 最佳實踐整合而成的一個 lint 規則集；與此同時，我們也更新了核心函式庫的 API 文件，為其帶來了豐富的範例程式碼。並且，為了改善平台整合特性，我們在 Flutter 外掛中提供了一個新的模版，使用 `dart:ffi` 與原生平台進行 C 語言的互操作、對 RISC-V 指令集提供實驗性支援，以及對 macOS 和 Windows 可執行檔案的簽名支援。如果想要了解 Dart 2.17 中所有新改進的細節情況，請關注將在近期釋出的文章。

## Firebase 和 Flutter

當然，建構應用不僅僅是建構使用者介面框架。應用釋出者需要一套全面的工具，來幫助他們建構、釋出和營運自己的應用，包括認證、資料儲存、雲端功能和裝置測試等服務。目前已有多種服務支援 Flutter，包括 [Sentry](https://docs.sentry.io/platforms/flutter/ "Sentry 文件: Flutter 平台整合")、[AppWrite](https://appwrite.io/docs/getting-started-for-flutter "AppWrite 文件: 在 Appwrite 平臺中使用 Flutter") 和 [AWS Amplify](https://docs.amplify.aws/start/q/integration/flutter/ "AWS Amplify 文件: Flutter 整合")。

Firebase 是由 Google 提供的應用服務平台。[SlashData 的開發者基準研究](https://www.slashdata.co/developer-program-benchmarking/ "SlashData 的開發者基準研究") 顯示，62\% 的 Flutter 開發者在其應用中使用 Firebase。因此，在過去的幾個版本中，我們一直在與 Firebase 合作對兩者的整合進行擴充和改良，並將其打造成 Flutter 的首選整合服務。這包括將 Flutter 的 Firebase 外掛升級到 1.0，增加更好的文件和工具，以及提供像 [FlutterFire UI](https://pub.flutter-io.cn/packages/flutterfire_ui "FlutterFire UI package 頁面") 這樣的新 widget，為開發者提供可重用的認證和使用者資料介面。

今天，我們宣佈 Flutter 和 Firebase 的整合已成為 Firebase 產品核心的一部分並得到完全的支援。我們已將原始碼和文件轉移到 Firebase 的主 repo 和網站中。Firebase 對 Flutter 的支援將和 Android 和 iOS 端的支援同步發展。

此外，我們還進行了重大改進，以支援 Flutter 應用使用 [Crashlytics](https://firebase.google.cn/docs/crashlytics "Firebase Crashlytics 產品主頁") (這是 Firebase 中很受歡迎的即時崩潰報告服務)。隨著 Flutter Crashlytics 外掛的更新，您可以使用與 iOS 和 Android 開發者相同的功能集來即時追蹤致命錯誤。其中包括重要警報和指標，如 "無崩潰使用者"，幫助您保持應用的穩定性。Crashlytics 的分析管線已經升級，以改善 Flutter 崩潰的聚合處理，使其能更快地分級、優先處理和修復問題。最後，我們簡化了外掛的設定過程，因此您只需要幾個步驟就可以直接透過 Dart 程式碼設定並使用 Crashlytics。

## Flutter 休閒遊戲工具套件

對於大多數開發者來說，Flutter 是一個應用框架。但是，利用 Flutter 提供的硬體圖形加速支援和 Flame 等開源遊戲引擎，圍繞休閒遊戲開發而建立的社群也在不斷壯大。我們希望讓休閒遊戲開發者更容易上手，所以在今天的 I/O 大會上，我們釋出了 [Flutter 休閒遊戲工具套件](https://flutter.dev/games "Flutter 休閒遊戲工具套件") \(Casual Game Toolkit\)，它提供了範本、最佳實踐入門套件，還為您準備了可用於廣告和雲服務的贊助額度。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/introducing-flutter-3/announcing-flutter-casual-game-toolkit.png)

雖然 Flutter 並不是為高強度的 3D 動作遊戲而設計的，但如今，一部分這類遊戲在獨立於遊戲場景之外的使用者介面部分也開始使用 Flutter 來實現，包括 PUBG Mobile 這樣擁有數億使用者的流行遊戲。在 I/O 大會上，我們想看看我們能把技術推進到什麼程度，所以我們建立了一個有趣的彈球遊戲，它使用到了 Firebase 和 Flutter 的 web 端支援。I/O Pinball 彈球遊戲提供了一個客製桌面，圍繞著 Google 最受歡迎的四個吉祥物進行設計，它們分別是: Flutter 的 Dash、Firebase 的 Sparky、Android 機器人和 Chrome 恐龍，您可以在這個遊戲中與他人一較高下。我們希望透過這種有趣方式展示 Flutter 的多功能性。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/introducing-flutter-3/io-pinball.png)

## Flutter: 由 Google 支援，由社群驅動

我們喜歡 Flutter 的一點是，它不僅僅是 Google 的產品——它是一個「屬於所有人」的產品。開源意味著我們都可以參與其中，並與它的成功息息相關。您可以貢獻新的程式碼或文件，建立 package 來為核心框架賦予新的超能力，編寫教導他人的書籍和培訓課程，還可以幫助組織活動和使用者社群，等等。

為了展示社群的精彩，我們最近與 DevPost 合作，贊助了 Puzzle Hack 挑戰，讓開發者們透過 Flutter 來演繹經典的滑動拼圖遊戲，以展示他們的技能。這場活動呈現了 web、桌面和移動端完美結合的場景:  現在大家都可以透過瀏覽器或應用商店玩到這些遊戲。  

感謝您對 Flutter 的支援，歡迎來到 Flutter 3！
