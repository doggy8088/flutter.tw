---
title: 位元組跳動的多平臺綻放秘訣 | Flutter 開發者故事
toc: true
keywords: Flutter開發者故事, 位元組跳動
description: 更高效、更靈活、更精美，以及更多樣的產品研發，需要什麼樣的開發工具？看位元組跳動分享他們的故事。
image:
    path: https://devrel.andfun.cn/devrel/posts/2022/05/3o1bOf.png
---

位元組跳動旗下營運著一系列成功的使用者產品、企業應用以及服務，覆蓋資訊、教育、娛樂等不同領域。隨著產品陣容的不斷髮展，傳統的原生雙平臺開發已經難以滿足團隊更高效、更靈活、更精美，以及更多樣的產品研發需求。Google 首次釋出 Flutter 的時候，團隊就果斷決定用手上的 iOS 和 Android 應用來測試其跨平臺開發能力，而 Flutter 也用不遜於原生框架的效能表現和極高的生產力迅速征服了團隊成員們的心。

<iframe src="//player.bilibili.com/player.html?aid=596724784&bvid=BV1xB4y197Tc&cid=722160670&page=1&autoplay=false" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="690" height="480"> </iframe>


> Flutter 擁有令人難以置信的超高效率，與原生雙端開發相比，Flutter 為我們的團隊節省了大約 1/3 的開發時間。
> 
> —— 董巖, 位元組跳動 Flutter Infra 團隊負責人

## **多平臺出擊，擁抱更多機遇**

在更多的平臺上覆蓋更多使用者一直是位元組跳動產品團隊的目標之一。比如在中國非常受歡迎的短影片娛樂應用 "抖音火山版"，就讓 iOS 和 Android 平台的使用者都可以透過分享短影片來展示他們的愛好、技能與日常，且擁有非常一致的體驗。

![△ 抖音火山版](https://devrel.andfun.cn/devrel/posts/2022/05/3xkCSz.gif)

△ 抖音火山版

在 2B 領域，位元組跳動則有一款支援企業協同辦公的一站式應用 Lark，透過音視訊會議、文件共享、及一系列專案管理與能效工具來支援團隊的即時通訊需求。其中，Lark 的移動版本使用了 Flutter 來實現跨平臺開發。

![△ 協同辦公一站式應用 Lark](https://devrel.andfun.cn/devrel/posts/2022/05/slsqWB.gif)

△ 協同辦公一站式應用 Lark

## **開發更高效，釋放團隊創意**

[熱重載 (Hot reload)](https://flutter.tw/development/tools/hot-reload) 一直是 Flutter 最令開發者們稱道的開發特性之一，讓工程師不再需要一遍遍地等待漫長的編譯，從而高效除錯程式碼，快速將 UI 設計師的想法變為現實。

為了讓開發者們能最大限度利用各個平台的原生底層功能，Flutter 還提供了外部功能介面 [FFI](https://flutter.tw/development/platform-integration/c-interop)。這使得 Flutter 應用可以直接呼叫 C++ 層程式碼，提高與硬體互動的效率，同時實現端上深度學習模型的部署。

"極課錯題印表機" 和 "極課閱卷大師" 兩款智慧裝置上搭載的應用都使用 Flutter 開發，前者用於放置在校園內供學生們自助列印錯題，後者則可以幫助老師智慧識別學生的答卷以及統計分數。藉助 Flutter，團隊得以為超過 200 所學校的師生快速完成裝置軟體的開發以及迭代。

![△ 智慧裝置 "極課錯題印表機" 中的應用使用 Flutter 開發](https://devrel.andfun.cn/devrel/posts/2022/05/oMNaBD.gif)

△ 智慧裝置 "極課錯題印表機" 中的應用使用 Flutter 開發

有些時候，Flutter 打造的作品本身也能進一步提升團隊的工作效率。比如位元組跳動最有趣的 Flutter web 應用之一 Alchemy，這是一款服務於抖音內容創作的內部工具，為 PGC 與 UGC 使用者提供便捷有趣的素材產生工具與海量素材，並批次產出受版權保護的設計物料。

![△ Alchemy](https://devrel.andfun.cn/devrel/posts/2022/05/7AhByp.gif)

△ Alchemy

## **畫素級精美，收穫使用者芳心**

Flutter 對應用螢幕渲染的精確控制能力，讓團隊得以放開手腳打磨產品的每一個畫素。"Lemon8" 就憑藉優良的使用者體驗榮登 2021 年日本地區 Google Play 年度應用榜單，這是一款興趣種草社群應用，專注於時尚、美容、美食、旅遊、居家、健身、藝術、戶外、攝影等內容領域。多樣的興趣圈層對應用的一致認可，離不開 Flutter 對畫布強大的控制力，以及團隊的精心雕琢。

![△ Lemon8](https://devrel.andfun.cn/devrel/posts/2022/05/wbj080.gif)

△ Lemon8

## **多平臺開發解決方案**

> Flutter 是我們公司當下重要的多平臺開發解決方案。我們從最初的幾個應用開始，逐漸發展到現在的 90 多個應用，面向包括移動端、web 端、桌面端，以及嵌入式裝置等多個平台。我們致力於推動 Flutter 在位元組跳動的深度應用，並對核心框架作出貢獻。
>
> —— 王瑩，位元組跳動 Flutter Infra 引擎技術負責人

移動端醫療服務應用 "小荷健康"、推薦優質房地產內容和資訊的應用 "幸福裡"、團隊虛擬辦公工具 "Coze"，以及程式設計師和產品經理們熟悉的社群平台 "掘金"……位元組跳動如今使用 Flutter 開發的應用已經超過 90 款，全部由位元組跳動的 800 多名 Flutter 開發者和 Flutter Infra 團隊共同支援。

在如此廣泛地採用 Flutter 的同時，團隊自然也對 Flutter 開放的社群讚歎不已: 來自世界各地的開發者們都在為社群貢獻程式碼、編寫 package、完善文件和製作課程，而總有讓您撓頭的問題能在社群中找到答案。

團隊也在為 Flutter 核心框架作出自己的貢獻。比如提升核心框架在 [iOS](https://github.com/flutter/engine/pull/17366) 和 [Android](https://github.com/flutter/engine/pull/30924) 上的穩定性，[最佳化 RasterCache](https://github.com/flutter/engine/pull/31892)、[PlatformView](https://github.com/flutter/engine/pull/27662)、[多引擎](https://github.com/flutter/engine/pull/17366)、[執行緒排程](https://github.com/flutter/engine/pull/30605)、[x86 桌面系統](https://github.com/flutter/engine/pull/30417) 和 [Web](https://github.com/flutter/website/pull/3296) 等功能，以及為 Flutter 的 [DevTools](https://github.com/flutter/engine/pull/30538)、[空安全問題](https://github.com/flutter/engine/pull/30145) 提供修復等等。

隨著 [Flutter 3](https://flutter.cn/posts/introducing-flutter-3) 的釋出，相信位元組跳動團隊會創造出更多、更美好的多平臺本地化應用，讓我們拭目以待！
