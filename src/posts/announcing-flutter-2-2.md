---
title: Flutter 2.2 現已釋出！
toc: true
---

在本次 [Google I/O 2021 大會](https://mp.weixin.qq.com/s/S1GZdQdwcdZKIbBY_FlzJQ)上，我們正式釋出了 [Flutter 2.2](https://flutter.tw/whats-new)。Flutter 2.2 是我們最新版的開源工具套件，可讓開發者立足單個平台建構適合任何裝置的精美應用。Flutter 2.2 是迄今為止最出色的 Flutter 版本。藉助更新後的功能，開發者能更便利地透過應用內購買、支付方式和廣告將其應用變現，還能連線到雲服務和 API 來擴充應用的功能；而藉助工具和語言功能，開發者能夠消除一整類別的錯誤，增強應用效能並縮減軟體包大小。

![](https://devrel.andfun.cn/devrel/posts/2021/05/zC30Hx.png)

## **在 Flutter 2 的基礎上打造而成**

Flutter 2.2 在 Flutter 2 的基礎上打造而成，並將 Flutter 的根基從最初的移動裝置擴充到 web、桌面裝置以及嵌入式裝置。其絕無僅有的設計是為了滿足環境計算世界的需求，因為在這樣的世界中，使用者有各式各樣、尺寸不一的裝置，而使用者期望在其日常生活中的各式裝置上獲得一致的美好體驗。Flutter 2.2 的存在，使得企業、初創團隊和企業家們都可以建立高品質的解決方案，充分發揮潛在市場的潛力，讓目標平台不再成為限制因素，而只需專注於尋找創意靈感。

**Flutter 是目前開發者首選的跨平臺開發框架。**

近期的行動開發者調研結果凸顯了 Flutter 的迅猛發展。分析公司 [SlashData](https://www.slashdata.co/) 的 [Mobile Developer Population Forecast 2021](https://www.slashdata.co/reports/?category=mobile-desktop) (2021 行動開發者人口預測) 報告顯示 Flutter 是如今最流行的跨平臺開發框架，有 45% 的開發者選用，在 2020 年 1 季度至 2021 年 1 季度期間，使用人數增長了 47%。我們自己的資料也印證了這一增長，過去 30 天內，在 Play 商店新上架的應用中，有超過八分之一的應用是使用 Flutter 開發的。

在 I/O 大會上，我們介紹過，如今僅 Play 商店就有超過 20 萬款應用是使用 Flutter 開發的；開發這些應用的公司如騰訊，其即時通訊應用微信 ([WeChat](https://apps.apple.com/us/app/wechat/id414478124)) 在 iOS 和 Android 平台的使用者數超過了 12 億；還有 TikTok 的開創者 [位元組跳動](https://www.bytedance.com/en/products/)，其目前已經使用 Flutter 開發了 70 款不同應用；以及 [BMW](https://www.press.bmwgroup.com/global/article/detail/T0328610EN/the-my-bmw-app:-new-features-and-tech-insights-for-march-2021?language=en)、[SHEIN](https://apps.apple.com/app/id878577184)、[Grab](https://apps.apple.com/app/id647268330) 和 [滴滴](https://play.google.com/store/apps/details?id=com.xiaojukeji.didi.global.customer&hl=None) 等公司。當然，使用 Flutter 的並非只有大公司，部分創意十足的應用就出自一些您可能不曾聽過的公司，比如 [Wombo](https://play.google.com/store/apps/details?id=com.womboai.wombo&hl=None) (一款迅速走紅的唱歌自拍應用)、[Fastly](https://play.google.com/store/apps/details?id=de.fastic.app&hl=None) (一款飲食控制應用) 以及 [Kite](https://play.google.com/store/apps/details?id=com.zerodha.kite3&hl=None) (一款精美的投資交易應用)。

## **介紹 Flutter 2.2**

Flutter 2.2 側重於改進開發體驗，目的是讓您能夠向客戶提供更穩定、效能更好的應用。

現在，預設情況下，新專案會自動啟用健全的空安全。空安全可有效防範空參考例外，讓開發者能夠有一些方式來在自己的程式碼中表示非可空型別。由於 Dart 的實現非常*健全*，編譯器能夠在執行時避免空檢查，從而提升應用的效能。生態系統也緊隨其後，目前已有大約 5,000 個 package 更新支援空安全。

在這一版本中還包含了許多效能改進: 對於 web 應用，我們使用 Service Worker 來提供後臺快取；對於 Android 應用，Flutter 支援延遲載入元件；至於 iOS 應用，我們已在開發工具來對著色器進行預編譯，以便消除或減少首次執行卡頓。我們還向 DevTools 套件中添加了許多新功能，藉此幫您瞭解應用中記憶體分配方式，以及支援第三方工具擴充程式。

此外，我們還在一些重要的領域進行了最佳化，例如提高網路目標的可及性。

我們的工作已不再侷限於 Flutter 的核心。我們還與 Google 其他團隊合作，將 Flutter 與更多技術棧的開發者進行聯動。特別指出的是，我們仍會繼續打造可信賴的服務來幫助開發者負責任地將其應用變現。在此版本中，我們 [新的廣告 SDK](https://developers.google.cn/admob/flutter/quick-start) 也已更新，現在具有空安全設定，並支援自適應橫幅廣告格式。我們還引入了一個新的支付外掛，這款外掛是我們與 Google Pay 團隊合力開發，可用於在 iOS 和 Android 平台處理實物商品的支付事宜。此外，我們也更新了我們的 [應用內購買外掛](https://pub.dev/packages/in_app_purchase)，以及配套的 [codelab](https://codelabs.developers.google.com/codelabs/flutter-in-app-purchases#0)。

在此版本中，作為成就 Flutter 的 "秘密武器"，[Dart](https://dart.tw.gh.miniasp.com/) 也有了更新。Dart 2.13 擴充了對原生互操作性的支援，現在支援在 FFI 中使用陣列和封裝結構體。新的 Dart 版本還支援類型別名，如此一來，不但能提高程式碼可讀性，也讓部分重構工作更為輕鬆。我們將繼續為更廣泛的生態系統增加整合方案，包括 [GitHub Actions](https://github.com/marketplace/actions/setup-dart-sdk) 和針對基於雲的業務邏輯部署最佳化過的官方 [Docker 映像](https://hub.docker.com/_/dart)。

## **不止是 Google 專案**

儘管 Google 仍是 Flutter 專案的主要貢獻者，但我們也欣喜地看到 Flutter 的生態系統在不斷髮展壯大。

![](https://devrel.andfun.cn/devrel/posts/2021/05/vulUJU.png)

近幾個月來，一個特別的增長領域就是支援 Flutter 的平臺和作業系統日益增多。在 [Flutter Engage 活動](https://flutter.cn/posts/flutter-engage-event-recap)中，我們曾宣佈 [Toyota 將把 Flutter 引入其下一代汽車資訊娛樂系統](https://flutter.cn/posts/seamless-multi-platform-app-development-with-flutter)。上個月，Canonical 推出了其首個 [集成了 Flutter 支援的 Ubuntu 版本](https://ubuntu.com/blog/ubuntu-21-04-is-here)，其中集成了 Snap 並支援 Wayland。

兩家新合作伙伴的到來也說明這個生態系統正在不斷髮展: [三星正在將 Flutter 移植到 Tizen](https://github.com/flutter-tizen/flutter-tizen) (採用一個其他開發者也可貢獻內容的開原始碼庫)，[索尼正在主導為嵌入式 Linux 提供解決方案而努力](https://github.com/sony/flutter-embedded-linux)。

設計師們也因為這個專案的開源性質而獲益匪淺，[Adobe 已宣佈推出其 XD to Flutter 外掛更新版](https://flutter.cn/posts/announcing-xd-to-flutter-v2-0)。Adobe XD 為設計師提供了絕佳的試驗和迭代方式，現在，由於加強了對 Flutter 的支援，設計師和開發者可以通力協作，以前所未有的速度將自己的想法付諸實踐。

最後，微軟將繼續與我們合作；除了 Surface 團隊一直在使用 Flutter 開發可摺疊裝置體驗專案外，本週又增加了 [Flutter UWP (Windows 10) Alpha 應用](https://flutter.cn/desktop#windows-uwp)。我們興奮地看到，越來越多的應用在利用 Flutter 內建的平台適應功能來打造可在移動裝置、桌面和 web 等多個平台完美執行的體驗。

## **打造絕佳的體驗**

我們推出 Flutter 的最主要目的是幫助開發者打造絕佳的體驗。我們認為應用開發工作可以更美好，希望可以為您消除在觸及受眾時遇到的傳統障礙，這些理念激勵著我們不斷向前。

我們期待看到您使用 Flutter 建構應用。美國退伍軍人交易部的專案就是一個不錯的應用範例，透過 [影片](https://youtu.be/2S-KkvFuLWs)，您可以瞭解其 Flutter 應用是如何幫助他們為患有創傷後應激障礙 (PTSD) 計程車兵提供康復治療的。

我們在 Flutter 方面所做的工作會在 [Google I/O 大會](https://mp.weixin.qq.com/s/S1GZdQdwcdZKIbBY_FlzJQ)上 [以各種研討會、示範和影片點播的形式](https://events.google.com/io/program/content?4=topic_flutter) 與大家分享。別忘記嘗試我們的 [photo booth web 應用](https://photobooth.flutter.dev)，這個有趣的應用就是用 Flutter 開發的，您可以在那裡與我們的吉祥物 Dash 及其夥伴們合影留念！

![](https://devrel.andfun.cn/devrel/posts/2021/05/r8Qxd4.png)
