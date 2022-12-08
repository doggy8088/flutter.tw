---
title: Flutter Engage China 開發者常見問題解答 — 上篇
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/04/84565efef718e.jpg)

再次感謝大家對 [Flutter Engage China 活動](https://flutter.cn/posts/flutter-engage-china-event-recap) 的關注和積極參與！我們在活動前後收到了很多來自開發者的反饋和問題，Flutter 團隊和演講嘉賓在直播 Q&A 環節中也針對部分問題在第一時間給出回覆。現在我們將一些開發者關心的問題和回覆整理出來分享給大家，希望對您有所幫助。由於問題數量較多，我們會分為上下兩期釋出。您也可以觀看 Flutter Engage China 影片回顧精彩內容:

**➡️[點選這裡觀看直播回顧影片](https://www.bilibili.com/medialist/play/ml1214246458/BV1hh411D7mV)⬅️**


## **如何更好地解決跟平台硬體互動的問題？**

*回答者: 於瀟，Google Flutter 移動端團隊負責人*

和硬體 API 互動最好的方法是透過 [平台通道](https://flutter.cn/docs/development/platform-integration/platform-channels) (Platform Channel) 和平台的 SDK 進行互動。在這之上也有 [Pigeon](https://pub.flutter-io.cn/packages/pigeon) 之類別的外掛可供大家使用，它可以產生針對所有語言的終點 (endpoint)，從而簡化平台通道程式碼的編寫過程。

如果硬體有 C++ 的驅動的話，也可以使用 [外部函式介面](https://flutter.cn/docs/development/platform-integration/c-interop) (FFI)，透過 FFI 呼叫 C++ 的 SDK 來使用硬體層的能力。

## **Flutter 在開發效率方面有哪些優勢？**

*回答者: 劉森森，阿里巴巴 UC 客戶端團隊*

Flutter 的開發效率很高，從阿里巴巴和 UC 多個團隊的實際應用情況來看，主要體現在以下幾點:

* **Flutter 具備非常良好的研發體驗** ，可以說是非常接近 web 了: 包括提供了裝置的熱重載功能，在 IDE 裡也能提供視覺化佈局，這些都非常便於開發和迭代。
* **Flutter 內建了很多符合現代化 UI 設計的 widget** ，使得開發團隊能很容易地開發出 UI 表現力很好的 Flutter 頁面。
* **Flutter 跨平臺的一致性非常強大** 。

從開發者們實際落地使用的效果來看，單客戶端的開發者使用 Flutter 就可以覆蓋多端的研發任務。在 UC 團隊裡，前端的開發者也能深度參與 Flutter 的業務開發。另外 Flutter 在互操作上也很靈活，比如提供了 FFI、平台通道、外接紋理等做法，讓開發團隊可以更容易地為 Flutter 提供現有的原生元件，為元件研發的速度帶來了非常大的提升。

在團隊正式使用 Flutter 之前，建議大家把 Flutter 融入到現有的研發工作流程裡，包括開發、測試、整合等各個階段，併為開發者們進行一些常規的 Flutter 技能培訓，這些做法可以大幅提升 Flutter 落地的效率。

總的來說，Flutter 對比傳統的 Android 和 iOS 研發效率有質的飛躍。

## **Flutter 適合做遊戲開發嗎？**

*回答者: 樊舟穎 (Zoey)，Google Flutter 產品經理*

Flutter 的初衷並不是為遊戲而創作的，也就是說，遊戲的應用場景並不是 Flutter 一開始所考慮的。但是我們在過去的一年到一年半里，發現確實有越來越多的遊戲開發者開始使用 Flutter，而且在 Play Store 裡獲得了非常大的下載量。

就我們觀察到的情況而言，強調效能和影象表現，或者需要用到很多重力感應等特殊元件的遊戲大部分還是會用 Unity 這樣的引擎來開發。但是一些休閒遊戲，比如字謎、猜圖、連連看，我們有看到越來越多的人在使用 Flutter 開發。

後續我們會透過使用者問卷調查，瞭解遊戲開發者使用 Flutter 的話主要有哪些需求。

## **Flutter 1 升級到 2 後有很多 API 都過時了，有好的方法快速從 1 轉 2 麼？**

*回答者: 董韜，Google Flutter 使用者體驗研究負責人*

Flutter 2.0 版本的釋出是一個比較主要的升級，從 release note 來看有大概 12 個重大變更 (breaking changes)。我們團隊對 API 的更改一直抱持非常謹慎的態度。畢竟我們也瞭解開發者需要去更新到新的版本，這個過程中會碰到一些障礙。但我們也會不斷改進 API，來讓 Flutter 本身更加好用、易用。

如果在升級 Flutter 的過程中碰到 API 變更的問題，這裡有兩個建議:

* 請考慮使用我們在 2.0 版本中同時釋出的 [Flutter Fix](https://flutter.cn/docs/development/tools/flutter-fix) 工具。這個工具有命令列，也有和 IDE 進行整合，可以幫助大家更方便地自動進行 API 升級。
* 建議大家放心及時地升級 Flutter 版本。如果您從一個相鄰的版本升級到一個新的版本，那麼需要更改的程式碼一般是比較少的。

## **Flutter 的渲染預設不是平台的主執行緒，那在渲染的時候會不會由於執行緒優先順序問題而影響流暢度？如果出現流暢度問題，該如何解決？**

*回答者: 於瀟，Google Flutter 移動端團隊負責人*

執行緒優先順序確實有可能導致問題。在 Android 上我們是有改變 "背景" 執行緒的優先順序的，但在 iOS 上目前還沒有。請繼續關注我們在這方面的探討: [https://github.com/flutter/flutter/issues/65752](https://github.com/flutter/flutter/issues/65752)

## **除了 Ubuntu，其他 Linux 版本 (如 Red Hat) 要客製開發底層解析 Flutter 引擎嗎？**

*回答者: 董韜，Google Flutter 使用者體驗研究負責人*

Flutter 對 Linux 的支援其實是不僅限於 Ubuntu 的。只要您使用的 Linux 是 64 位的，就可以安裝和使用 Flutter 去開發 App。具體可以參考 [Flutter 官方的 Linux 安裝說明](https://flutter.cn/docs/get-started/install/linux)。

Ubuntu 則更方便了一些，提供了一個官方的 Flutter 安裝工具。另外 Ubuntu 也開始用 Flutter 開發一些內建的 App。

## **希望官方外掛儘快解決現有的 issue，期待 Flutter 生態越來越好！**

*回答者: 楊天航 (Chris)，Google Flutter 團隊工程師*

謝謝提出這個問題的同學。我們在去年的時候，由於各種原因，把主要的工作重點放在了基礎設施的建設上，包括穩定性這些方面。今年我們的工作重點之一就是解決 issue，包括對 pull request 的稽核等。大概在一個月前，我們大大增加了每週整理 issue 和 PR 的時間，增加為去年的 4 倍以上。

另外，我們目前還在跟一些公司和團隊進行合作，比如 Baseflow，Invertase，以及和社群進一步加大合作，從而進一步提高 Flutter 外掛的品質。

・・・

以上就是 Flutter Engage China 開發者常見問題解答 (上篇) 的內容，也請大家繼續關注下篇內容。
