---
title: 新版 Flutter 中文開發者網站釋出
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/12/VAmVaC.jpg){:max-width="90%"}

Develop as One，2021 年 Google 開發者大會 (Google Developer Summit) 於上個月順利舉辦，包含眾多最新 Google 技術產品更新的線上演講，乾貨滿滿。

![](https://devrel.andfun.cn/devrel/posts/2021/12/QIL1S6.jpg)

在 11 月 16 日上午進行的主題演講中提到，目前國內的開發者數量有 500 萬以上，約佔全球開發者數量的五分之一，中國的遊戲開發商在頭部的海外遊戲市場份額超過 23%，排名第一，應用和遊戲開發仍是一個值得持續投入並且擁有更廣闊前景空間的市場。

Flutter 在這其中也扮演了相當重要的角色。首先是頭部廠商採納，我們看到**位元組跳動有 70 多款應用正在使用 Flutter**，騰訊在其 **企業微信** 和 **PUBG 手遊**上也使用 Flutter 建構新的功能，以及今年早些時候提到的貝殼找房也在 **用 Flutter 建構一些核心功能**。其次，不斷完善的 Flutter 產品已經成為了開發者們不可或缺的「秘密武器」——3 年前正式釋出的時候，我們將其定義為「行動式的 UI 工具套件」，到現在進化成「**一個為環境計算打造的 UI 平台**」，開發者們只需要寫一套程式碼，便可為所有螢幕建構精美應用，包括移動端、Web 端、桌面端和嵌入式裝置平台。最後，**全球的 Flutter 開發者數量達 300 萬以上**，當你開始寫下 Flutter 的第一行 Hello World! 的時候，你也就加入成為全球大家庭中的一員。

在此次開發者大會上，Flutter 團隊也公佈和回顧了一些近期的更新，包括：

* Flutter 的桌面端支援在近期所做出了的大量改進，下一步計劃是 Windows 端的無障礙功能最佳化和改進；
* Flutter Web 的 **CanvasKit 渲染器穩定版已釋出**，可以透過 `flutter run -d chrome --web-renderer canvaskit` 使用；
* Flutter 版的 **谷歌移動廣告外掛正式釋出 1.0 版本** (支援空安全)，支援加入 AdMob 和 Ad Manager 廣告，並支援各種廣告形式，包括橫幅廣告、插頁式廣告、激勵影片廣告和原生廣告。正式版也加入了廣告中介的 beta 支援，透過 pub 命令 `flutter pub add google_mobile_ads` 即可開始使用；
* 介紹和回顧了其他有關使用 Flutter 進行盈利的外掛，包括使用應用內購買的 `in_app_purchase` 外掛、整合使用多種付款方式的 `pay` 外掛，都能幫助你更好的透過 Flutter 盈利。

## DartPad 和 Flutter 網站更新

DartPad 是一個開源的線上編譯和執行 Flutter / Dart 程式碼的平台，它可以幫助你 **方便的學習和實驗 Dart 程式語言特性**，也可以將程式的最小可復現程式碼 **分享給其他開發者一起交流討論**，我們也製作了大量可互動的 Codelab，其中就內嵌了 DartPad，讓你學習起來更方便、更直觀、更有趣。

Pub.dev 上的 Dart package 和外掛是 Flutter 生態中非常重要的組成部分，這些開箱即用的外掛可以大大提高開發效率。今天，我們 **正式為 DartPad 加入了 package 的支援**，第一階段我們支援了一組比較流行的 package，可以在 DartPad 右下角的圖示上看到：

![DartPad 支援匯入 package 了](https://devrel.andfun.cn/devrel/posts/2021/12/pMrooB.jpg)

如果你想提名更多希望我們支援的 package，可以在 DartPad 的 Issue 區提出或者點贊讓我們看到：https://github.com/dart-lang/dart-pad/issues
與此同時，由中國 Flutter 社群維護的 DartPad.cn 也支援了這項新的功能，國內的開發者可以使用 DartPad.cn 存取和體驗。

![Flutter 中文開發者網站 - flutter.cn](https://devrel.andfun.cn/devrel/posts/2021/12/VAmVaC.jpg)

Flutter 網站也在今天正式釋出全新的設計，新版的網站更專注從視覺和文字上突出 Flutter 的特性，包括在各種平臺上使用 Flutter 建構應用的優勢以及上手文件和參考資料、Flutter 開發的學習資料合集，也介紹了 Flutter 生態上的優勢和強大的社群，還有關於透過 Flutter 盈利的課程和其他開發者的成功故事等。

在中國社群成員們的共同努力下，我們於近期「清掃」了一批沒有翻譯的文件，伴隨官網此次改版，我們也同時跟進發布了中文版的本地化頁面（確切的說是比 flutter.dev 更早釋出了 ;-P）點選閱讀原文直達。希望我們為大家提供的這些中文開發者資源能夠幫助你更好的使用 Flutter。

## Google 開發者大會上的 Flutter 演講影片

在本次大會在主題演講中，Flutter 產品經理 Zoey 為大家帶來了 Flutter 相關的更新和介紹（從影片的 51 分 20 秒 開始）:

<iframe width="560" height="315" frameborder="0" src="https://v.qq.com/txp/iframe/player.html?vid=p0041ce57pe" allowFullScreen="true"></iframe>

也有四個與 Flutter 相關的專題演講：

1. Google 產品經理 Kevin Moore: 使用 Flutter 建構自適應跨平臺應用

<iframe width="560" height="315" frameborder="0" src="https://v.qq.com/txp/iframe/player.html?vid=f0041nmx1l2" allowFullScreen="true"></iframe>

Flutter 現已支援移動、桌面、Web 等多達 6 個平台。瞭解相關最佳實踐，讓您的應用在良好適配每個平台的同時，實現最大程度的程式碼複用。

2. Google 軟體工程師錢冠霖: Flutter 的 Deferred Components

<iframe width="560" height="315" frameborder="0" src="https://v.qq.com/txp/iframe/player.html?vid=h0041b043ig" allowFullScreen="true"></iframe>

應用體積是開發者最大的顧慮之一。學習如何使用 Flutter 的延遲載入元件，讓您實現在執行時下載預編譯的程式碼和資源。

3. Google 軟體工程師 Justin McCandless: 利用延遲載入提升 Flutter 應用效能

<iframe width="560" height="315" frameborder="0" src="https://v.qq.com/txp/iframe/player.html?vid=m0041hexsmo" allowFullScreen="true"></iframe>

瞭解如何透過延遲載入內容來最佳化效能，它的侷限性，以及 Flutter 如何讓您預設即可實現更流暢的使用者體驗。

4. Google 軟體工程師 Bob Nystrom: 為何要使用「空安全」

<iframe width="560" height="315" frameborder="0" src="https://v.qq.com/txp/iframe/player.html?vid=l0041a7jxo6" allowFullScreen="true"></iframe>

「空安全」是一項新推出的基本語言功能，由多人共同開發。在這裡，其中一位開發者將向您介紹如何利用該功能改善 Dart 程式碼編寫體驗、防止出錯以及怎樣產生更小且更快的二進位制檔案。

感謝 Flutter 團隊和社群成員們的幫助，希望 Flutter 可以更好的助力你獲得成功！
