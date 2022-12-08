---
title: 歡迎體驗 Beta 版 Flutter web 支援
toc: true
---

*作者 / Mariam Hasnany, 產品經理, Flutter*

我們很高興宣佈 Flutter web 支援已順利迭代至 Beta 版本！

## **為什麼我們要把 Flutter 帶向 web 平台？**

開發者經常需要建構可以在移動和 web 兩個平臺上執行的應用。我們想要幫助您設計和構建出自己想要的效果，將心中的奇思妙想變為現實，讓您知道 Flutter 能夠讓您的應用在任何地方流暢執行！開發者們都希望學習一套可在多個平臺上靈活使用的技術。Flutter web 支援允許您使用同一套程式碼，更快地釋出功能，並且在不同裝置上實現一致的使用者體驗。此外，強大的 web 端 Dart 編譯器和便攜性出眾的 Flutter 框架可大大簡化您的開發工作，讓您使用 Flutter 輕鬆打造出精美的互動式 web 體驗。

## **比預覽版更出色**

我們在今年的 I/O 大會上釋出了 web 支援的 [技術預覽版](https://mp.weixin.qq.com/s/trNhd1CI1gBBDtmVGdeI6g)，並在 7 月推出了 [早期使用者計劃](https://medium.com/flutter/flutter-for-web-early-adopter-program-now-open-9f1fb146e4c4)，這段時間以來，我們一直在努力改進 web 支援，力求更好地滿足來自 Google 內部團隊和外部開發者的需求。

**Beta 版能為您帶來什麼？**

隨著 Flutter 1.12 的釋出，Flutter web 支援也從技術預覽版順利迭代至 beta 版。當您在 beta 渠道內啟用 web 支援後，新建立的 Flutter 專案不僅包含了 Android 和 iOS 應用，其中新增的 web / directory 還提供了豐富的工具，幫助您在瀏覽器中編譯和運行同一個專案。

隨著 Flutter web 支援逐漸穩定，它已經準備好迎接新的挑戰，歡迎更具探索精神的開發者們前來開啟更多的使用場景。目前，團隊的開發工作已進入下一個階段，我們將繼續引入變更，並從無障礙支援、測試覆蓋等多個方面進一步最佳化 web 支援。

## **探索使用場景**

在 Flutter web 支援的開發過程中，我們尤為關注部分使用場景。我們認為這些場景特別能夠體現 Flutter 的優勢所在。我們相信現有的功能已足夠幫助開發者建構豐富的互動式 web 體驗。在與早期使用者合作的過程中，我們還針對下列使用場景驗證與優化了 web 支援。

**多平臺互連的獨立應用**

Flutter 允許開發者使用同一套程式碼實現移動和 web 的跨平臺體驗。我們的一位早期使用者 [Journey](https://startyourjourney.io/) 就利用 Flutter 開發了一個多平臺應用。

![](https://devrel.andfun.cn/devrel/posts/2021/05/vMoskP.png)

> △ 社交應用 Journey 最近剛釋出了一個使用 Flutter 開發的跨平臺應用

Journey 的創始人 Luke O’Brien 給予了 Flutter 很高的評價: "我大概從 4 個月前開始開發 Journey，起初我只想做一個 Android 版本的最簡可行產品。沒想到後來竟然發現了 Flutter，當時我就覺得它特別棒，於是我就決定試著用一下。這絕對是我做過最正確的決定。Flutter 為我們縮短了至少一半的開發時間，現在我們已經發布了 Android、iOS 和 web 三個版本的應用，使用者增長量多了整整一倍。從萌生想法到產品落地，Flutter 在這個過程中起到了十分關鍵的作用。"

**內嵌式互動內容**

第二個使用場景在網站中內嵌一個小程式。這類程式通常功能豐富且以資料為中心，但是它們並不需要提供導航服務或者其它應用級別的複雜功能，比如說，您可以為現有網站新增一個新的汽車配置器、填字遊戲或者互動式的視覺化資料，這些都是該場景下比較典型的使用案例。我們的另一位早期使用者 [AEI Studio](https://studio.aei.dev/showcase/) 就開發了一系列聊天機器人並在 web 版的聊天對話方塊中內嵌了 Flutter，從而實現了動畫，鍵盤文字輸入等多項互動功能。

![](https://devrel.andfun.cn/devrel/posts/2021/05/2bGkk6.png)

> △ Weatherbot 是 AEI Studio 旗下的一款聊天機器人，

它的網頁版對話方塊就內嵌了 Flutter

**精簡版 Lite 應用**

儘管 Flutter 本身的移動端執行時 (mobile runtime) 現可提供更為流暢的體驗，但有時候使用者依舊會覺得安裝步驟太繁瑣，因而放棄嘗試新應用。如果為現有的 Flutter 應用配備一個輕量級 web 體驗，豈不兩全其美？大部分使用者依舊集中在移動版上，精簡版 web 應用則透過相同的工具、框架、UI 元件和業務邏輯提供相關功能，只不過會在豐富程度上進行適當縮減。

**輔助應用**

輔助應用是指用 Flutter 建構的，且為主要的移動版應用提供支援的 web 體驗。比如說，您可以使用 Flutter 建構一個 web 應用，管理員或內部使用者可以透過這個應用為現有的 Flutter 移動版應用進行內容創作或後臺管理。儘管此類 web 應用相對較為獨立，但它依舊可以複用移動應用的大量程式碼。

## **找到適合您的外掛！**

外掛是 Flutter 一個比較獨特的優勢，開發者可以透過外掛存取執行平台的原生庫。當您在 web 平臺上執行 Flutter 應用時，您可以呼叫現有 JS 庫中的所有內容。我們在後台完成了所有 JS 互操作程式碼，確保這些外掛在移動和 web 平臺上均能正常工作。我們已成功實現了請求最多的幾個外掛，讓它們能夠在 web 應用和原生應用中提供一致的使用體驗。從現在開始，您也可以像 [Ben Hagan](https://github.com/cbenhagen) 和 [Hadrien Lejard](https://github.com/lejard-h) 一樣 [編寫自己的外掛](https://medium.com/flutter/how-to-write-a-flutter-web-plugin-5e26c689ea1)，他們曾分別為 video_player 和 sentry 包開發過外掛。下列幾個包均有更新:

* [shared_preferences](https://pub.dev/packages/shared_preferences)
* [firebase_core](https://pub.dev/packages/firebase_core)
* [firebase_auth](https://pub.dev/packages/firebase_auth)
* [google_sign_in](https://pub.dev/packages/google_sign_in)
* [url_launcher](https://pub.dev/packages/url_launcher)
* [video_player](https://pub.dev/packages/video_player)
* [sentry](https://pub.dev/packages/sentry)

此外，我們還為 pub.dev 包庫添加了新平台的標籤管理和過濾功能。

首先，我們在每個套件的詳情頁面中列明瞭它所支援的平台，讓開發者可以更輕鬆地判別這個包是否提供 web 支援。

![](https://devrel.andfun.cn/devrel/posts/2021/05/taG3uH.png)

> △ pub.dev 套件的詳情頁面顯示了關於 SDK 和平台相容性的標籤

搜尋 UI 也新增了若干過濾器，方便開發者找到提供 web 支援的套件。我們基於新的 [平台清單標籤](https://flutter.dev/docs/development/packages-and-plugins/developing-packages#plugin-platforms) 開發了該功能，您可透過 Flutter 1.12 獲取這些標籤。

![](https://devrel.andfun.cn/devrel/posts/2021/05/UxFyg9.png)

> △ pub.dev 的搜尋 UI 添加了對 SDK 和平台的過濾器支援

## **向穩定版邁進**

我們在 Beta 版中取得了一定的進展，但仍有許多工作亟待完成。除了進一步提高 web 支援的效能表現之外，我們也在積極推進無障礙體驗、瀏覽器相容性等方面的工作。

**無障礙**

Flutter 透過 Android TalkBack 和 iOS VoiceOver 為移動瀏覽器提供無障礙支援。目前，我們已經為多個平台的輔助技術實現了多項特性，其中包括: UI 遍歷、遍歷順序以及豐富的 UI 互動提示，例如可點選區域、標籤、可編輯區域、漸增操作、圖片、活動區域和可檢查專案。此外，我們還在研究如何為桌面端瀏覽器新增讀屏支援。

**瀏覽器支援**

Flutter 的支援範圍已從一開始的行動平台擴充至桌面平台，使得桌面應用的 UX 設計風格也可透過 Flutter 框架實現。Flutter 將繼續最佳化對桌面瀏覽器的支援，併為使用者提供更加流暢的無縫體驗。接下來，我們計劃為 Chrome、Edge、Firefox 和 Safari 四款瀏覽器 (移動版和桌面版) 提供支援和測試。

**測試覆蓋**

從技術預覽版開始，我們就一直在擴大框架和 Flutter web 引擎的測試覆蓋範圍。Chrome 目前已支援執行自動測試，Safari 仍需手動進行測試。我們還有許多測試工作需要完成，未經測試的場景可能會出現迴歸錯誤。

**體驗、貢獻與分享**

歡迎各位開發者立即上手體驗 Flutter web 支援！請前往 [flutter.dev/web](https://flutter.dev/web) 開啟您的 web 之旅，盡情探索範例、文件等精彩內容。如果您之前已經使用過 web 支援，不妨嘗試一下我們的 [beta 渠道](https://github.com/flutter/flutter/wiki/Flutter-build-release-channels)。

Flutter 外掛的數量已突破 1,800 個，但是其中大部分僅提供 iOS 或 Android 支援。如果您想為 web 支援貢獻自己的一份力量，幫助我們縮小 web 與行動平台之間的距離，歡迎您為現有外掛新增 web 支援或開發新的外掛。請閱讀 [《Flutter web 外掛開發指南》](https://medium.com/flutter/how-to-write-a-flutter-web-plugin-5e26c689ea1) 進一步瞭解技術細節與開發方法。

## **結語**

我們已透過 beta 渠道開放了最新的 Flutter web 支援，與此同時，我們離最終穩定版的釋出也越來越近，希望您能喜歡這個 beta 版本，同時感受到我們對 Flutter 的熱情與付出。

歡迎大家向我們 [提交反饋](https://flutter.dev/community) 並參與 #Flutter 話題討論，與我們分享您在 Flutter 方面的專案進展。期待您透過 Flutter 打造出精美的互動式 web 體驗！
