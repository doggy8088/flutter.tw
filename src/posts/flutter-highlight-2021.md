---
title: 回顧 2021 Flutter 精彩時刻
description: 祝願各位 Flutter 開發者們虎年大吉、虎虎生威！
---

![Flutter新年](https://devrel.andfun.cn/devrel/posts/2022/01/6879bbc9b157d.png)

![2](https://devrel.andfun.cn/devrel/posts/2022/01/752a1f9e5ac3f.png)

2021 年，Flutter 正式進入 2.x 系列的正式版釋出，年初的 [Flutter 2 的釋出](./announcing-flutter-2) 打開了一個新的“格局”，為 Flutter 的加入了第五大特色——「**可移植性**」，讓 Flutter 從一個移動框架正式“升級”為一個「可移植框架」，目標是讓 Flutter 應用基本可以不加變動地在多種平臺上執行。透過 Flutter，開發者們可以為任何平台建立美觀、快速且可移植的應用。3 月份，除了 [Web 平台釋出穩定版](https://mp.weixin.qq.com/s/6oSwvPsMy6r4AW90aostiA) 之外，Flutter 也開始向桌面、可摺疊和嵌入式裝置上進行擴充。隨之釋出的 [Dart 2.12 正式版](./announcing-dart-2-12) 提供了健全的空安全和 FFI 的生產環境級支援。Dart 是一個站在 Flutter 背後的“秘密武器”，我們提到的很多 Flutter 的“閃光點”，實際很大程度要歸功於 Dart: 包括我們熟知的有狀態的熱重載 (Stateful Hot-Reload)，以及 Dart FFI 的成熟支撐了 Flutter 2 提到的「可移植性」，以及最新 Flutter 2.8 穩定版中關於效能的提升的部分，都離不開 Dart。

![2.2](https://devrel.andfun.cn/devrel/posts/2022/01/2faa787ee6bae.png)

21 年 5 月下旬的 I/O 大會仍是在線上舉辦，[Flutter 2.2](https://mp.weixin.qq.com/s/tnQ1F7kvrxKrbRs8bSzMmg) 和 [Dart 2.13](https://mp.weixin.qq.com/s/pmfJ3Q8wJ_fM0VTNWeaSqg) 穩定版正式釋出。除了不斷進化的 Web 支援，Flutter 也在行動平台有了很多關於效能方面的改進，也著手從基礎側為 Flutter 增強擴充到其他平台進行改進。Flutter 2.2 著重改進開發體驗，新專案會預設自動啟用健全的空安全
，Flutter 的開發者工具 (DevTools) 並和 IDE 外掛都得以改進和更新，DartPad 也為教學者增加了引導式程式碼體驗。Dart 2.13 對空安全加入了更多更新，並推出了開發者們非常期待的「類型別名」的新特性。

Flutter 生態裡有一個非常重要的角色，就是圍繞著 Flutter 釋出的一些 Dart Package，這些 package 極大程度上幫助了開發者更好的建構 Flutter 應用。FlutterFire 是官方維護的方便 Flutter 應用整合 Firebase 服務的一系列外掛合集，官方也將一批自己維護的外掛移交給社群進行維護，這標誌著 Flutter 已經遠不是隻有 Google 工程團隊在“單打獨鬥”，整個社群都在為 Flutter 進行持續貢獻。除了廣大的開發者社群成員們的貢獻之外，豐田、Canonical、三星、索尼、微軟也都在持續為 Flutter 做出貢獻。Flutter 不只是適用於開發者，也同樣適用於設計師們，Adobe XD 是一款 UI/UX 設計和協作工具，你可以使用 xd-to-flutter 外掛輕鬆的將設計稿轉換為支援空安全的 Flutter 程式碼。I/O 期間，Flutter 團隊還聯合 VGV 團隊釋出並開源了一個 [使用 Flutter 和 Firebase 建構的 Web 應用](https://mp.weixin.qq.com/s/vEtpHNgsNNzl5Bln3Tfr1g) ——「Flutter 照相亭」，作為一個樣例應用，開發者們可以學習應用是如何建構良好的拍照體驗、最佳化 Web 的應用體驗以及使用 Firebase 完成業務邏輯等。

![2.5](https://devrel.andfun.cn/devrel/posts/2022/01/30914f7e0b7fe.png)

21 年 9 月，Flutter 更新到了 [2.5 正式版](./whats-new-in-flutter-2-5)，Dart 也釋出了 [2.14 正式版](./announcing-dart-2-14)，這個版本仍延續了過去的一些工作，繼續進行一些重要的效能和工具改進，也同時加入了對 Material You 的支援等。從這個版本開始，Dart 對 Apple 晶片的支援正式在穩定版渠道推出，同時增加了很多共享的程式碼 lint 工具，pub.dev 上的評分引擎也開始使用其中的部分 lint 規則，package 釋出工具也支援了識別和使用 `.gitignore` 來宣告哪些需要忽略的內容，避免上傳無用的程式碼片段等。隨著 Dart 空安全特性的推出，有很多應用開發者和 package 開發者們已經開始跟進並應用了健全的空安全特性。除了穩步更新 Flutter 移動端的效能和開發體驗，[Flutter Web 也釋出了產品路線圖](./flutter-2022-roadmap)，力求讓 Flutter 應用在 Web 平台的體驗更自然。Flutter 團隊幾乎完全依靠 GitHub 等開源開放平台協作，所有的流程和程式碼改進都是向社群公開透明的，因此我們也在非常努力的推動國內的開發者參與全球社群貢獻，並 [在每次的開發者調查中發聲](https://www.bilibili.com/video/BV1CP4y1V7c9)，很多新特性和改進都會在 GitHub Issue 上釋出並公開徵集反饋，開發者們的反饋和互動越多，Flutter 團隊就越重視，越會投入資源去解決。

從 Play 商店的應用數量看，Flutter 應用數量從 21 年第一季度的 15 萬+ 上升到第二季度的 20 萬+，同時，21 年 4 月到 5 月，Play 商店中有超過八分之一的新應用是採用 Flutter 建構，21 年年末 Play 商店的 Flutter 應用數量對比第二季度幾乎翻番，達到了 37.5 萬+ 款應用。這些數字證明著 Flutter 的流行，以及受到越來越多開發者們的採納和喜愛。開發者們對 Flutter 的採納和喜愛也同時體現在很多開發者報告裡，分析公司 SlashData 的報告裡顯示：Flutter 是如今最流行的跨平臺開發框架，有 45% 的受訪開發者選用；RedMonk 排名中提到“Dart 有顯著上升”；StackOverflow 的開發者調查體現出開發者喜愛的程式語言裡，Dart 排名第七、喜愛的框架裡，Flutter 排名第二；JetBrains 2021 開發者生態系統現狀報告中顯示，在跨平臺移動框架選擇上，Flutter 的受歡迎程度持續增長，已經超越 React Native 成為最流行的跨平臺移動框架。使用 Flutter 的頭部公司和應用也包含了 BMW、ByteDance、滴滴、eBay、Grab、Greggs、貝殼、Norton、Philips Hue、PUBG、SHEIN、trip.com、WeChat 等 (*排名不分先後)，截止 21 年末，全球的 Flutter 開發者數量已達 300 萬以上，Flutter 也從最初的「行動式 UI 工具套件」進化成「一個為環境計算打造的 UI 平台」。

![2.8](https://devrel.andfun.cn/devrel/posts/2022/01/d914e399b45a1.png)

2021 年的最後一次穩定版更新發布的是 [Flutter 2.8](./announcing-flutter-2-8) 和 [Dart 2.15](./announcing-dart-2-15)，這個版本著重關注的是移動端平台的效能，由於重新設計和實現了 Dart 程式語言中 [isolate 的工作方式](https://mp.weixin.qq.com/s/WCvv7DXzWXNRaMtM-0u7pA)，使得 isolate 所消耗的記憶體最高減少了 100 倍，體現在 Flutter 應用裡就是啟動效能和記憶體佔用等效能都有非常大的提升，同時為了方便開發者們除錯應用效能，新版的開發者工具 (DevTools) 加入了一個 Enhace Tracing (增強追蹤) 的功能，用於診斷消耗較大的、引起 UI 卡頓的 Widget 建構、佈局和繪製操作。在 Web 的平臺視圖 (PlatformView) 方面，2.8 版本會複用平臺視圖之前建立的 canvas，提升效能減少滾動卡頓。圍繞 package 生態，這次更新了一系列 Firebase 相關的外掛，以及 Google 移動端廣告 SDK 的外掛，與此同時，你也可以透過 DartPad 工具直接使用部分 Firebase 外掛和其他 package 了。2.8 還將桌面端的支援往前邁出了一大步，官方也在近期向全球開發者徵集 [Flutter 桌面端的應用](https://mp.weixin.qq.com/s/RgUphbNnc6UTD05oppWnWA)，以擴大測試覆蓋率，為 Flutter 桌面端的穩定支援提供更多保障。

![2:4:2022](https://devrel.andfun.cn/devrel/posts/2022/01/b7b3cf3eadea5.png)

回望 2021，Flutter 的發展勢頭正旺、潛力無限，展望 2022，更是信心滿滿。尤其是桌面端的版本推進，馬上就會進行一個新的 Flutter 產品更新發布，這次釋出是針對 Windows 桌面應用程式開發者的，中國時間 2 月 4 號 (正月初四) 釋出，屆時我們也會關注並推送最新的資訊。

農曆新年將至，向各位 Flutter 開發者們拜年了，祝大家虎年大吉、虎虎生威！願所有不好的事情都煙消雲散，願這個世界的秩序可以儘快恢復，我們能自由的去見想見的人並和他們相互擁抱。衷心祝願各位讀者和家人朋友們身體健康、幸福美滿！

我們今年為大家準備了兩款 Flutter 紅包封面，分別是 Dash 虎頭帽和 Dash 迎新春 (迎新春款可透過領紅包頁面領取)，去年的三隻小禿頭也限時返場，祝願各位開發者們虎年紅紅火火，再創佳績！

![](https://devrel.andfun.cn/devrel/posts/2022/01/f9dvuY.jpg)

![虎頭帽封面QRCode](https://devrel.andfun.cn/devrel/posts/2022/01/ehgTJi.jpg)

![](https://devrel.andfun.cn/devrel/posts/2022/01/fbbcc804ac6c7.png)

![Dash 迎新春封面QRCode](https://devrel.andfun.cn/devrel/posts/2022/01/jzJ8QQ.jpg)

![](https://devrel.andfun.cn/devrel/posts/2022/01/3ZiOn9.jpg)

![三小隻封面QRCode](https://devrel.andfun.cn/devrel/posts/2022/01/qnBir2.jpg)