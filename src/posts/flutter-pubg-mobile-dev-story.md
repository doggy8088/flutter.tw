---
title: 藉助 Flutter 跨平臺特性連線 10 億玩家 | Flutter 開發者故事
toc: true
keywords: Flutter開發者故事, PUBG MOBILE
description: PUBG MOBILE 遊戲自帶的社群功能豐富，使用者群龐大，如何確保各個平台的玩家都能有最棒的體驗？
image:
    path: https://devrel.andfun.cn/devrel/posts/2023/02/lVFv2g.jpg
---

> _由光子工作室及 Krafton 聯合研發的 PUBG MOBILE 依然保持著_[_極高的人氣_](https://101.dev/t/google-play-2022/823)_，目前全球有 10 億玩家，日活躍 5,000 萬 (不包括中國大陸地區)。從遊戲策劃伊始，團隊就打算為各個平台的玩家們打造功能完善的社群模組。_

在 PUBG MOBILE 中，玩家們被空投到一個荒島上，孤身奮戰或與隊友合作，努力與對手周旋，倖存到最後贏得勝利。想要在這個遊戲裡 "吃雞"，玩家的敏捷反應和大局意識都很重要——而對負責開發遊戲內社群模組的團隊來說，這兩點正好也是他們成功的秘訣。

<iframe width="690" height="480" src="//player.bilibili.com/player.html?aid=393503978&bvid=BV1sd4y1H7G7&cid=985504987&page=1&autoplay=false" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

△ PUBG MOBILE 如何連線 10 億玩家 | Flutter 開發者故事
    
**全平台一盤棋，大局意識很重要**

負責 PUBG MOBILE 遊戲社群模組的團隊規模並不是很大，但服務的玩家群體卻十分龐大，這也讓團隊對任何能提高效率的開發技術都十分敏感。2020 年下半年，當團隊著手為社群模組尋找解決方案時，就在著眼尋找合適的 [跨平臺解決方案](https://flutter.dev/multi-platform): 他們需要讓社群模組能很方便地覆蓋 Android 和 iOS 平台。

> _我們測試的很多解決方案都存在這樣那樣的限制，但這些限制在 Flutter 中則不存在: 即便在引入了複雜的業務邏輯後，Flutter 的效能表現仍然十分優秀。_
> 
> 胡明春，PUBG MOBILE 開發團隊高階工程師

![](https://devrel.andfun.cn/devrel/posts/2023/02/SvqNgV.gif)

△ Flutter 打造的社群模組一直能保持穩定的幀率

團隊選擇 Flutter 的另一個原因是它能很方便地 [和現有的遊戲進行整合](https://flutter.tw/development/add-to-app)。這也讓社群模組在開發層面不至於和其他模組 "高度耦合"，讓團隊能專注打造社群功能本身。

> _Flutter 可以很輕鬆地和現有的遊戲進行整合，基本上照著官方文件操作就行，用不了多少時間。_
> 
> 張海鵬，PUBG MOBILE 開發團隊高階工程師

**大幅降低程式碼量，敏捷開發很重要**

"一次編寫，到處執行" 是 Flutter 跨平臺特性帶來的 "福利" 之一。除去一些平台特定的功能外，團隊可以**只用一個程式碼庫就覆蓋 Android 和 iOS 兩個平台**。這樣做的另一個好處是能**確保平台之間功能的一致性**，在高強度迭代時這個好處更是非常重要。

> _自從採用 Flutter 後，我們發現可以將前端所需的 **開發工作量減少 80%**！_
> 
> 程建，PUBG MOBILE 開發團隊高階工程師

![](https://devrel.andfun.cn/devrel/posts/2023/02/Dqcz3s.png)

△ 一次編寫，到處執行

如果正在閱讀本文的讀者還沒有體驗過 Flutter 帶來的這些優勢，那也許還會心存顧慮: Flutter 好學嗎？

正好我們也能從團隊的反饋中找到答案:

> _我們在 Java、Kotlin 和 Objective-C 方面有著堅實的基礎，這讓 Dart 語言的學習成本很低。_
> 
> 張海鵬，PUBG MOBILE 開發團隊高階工程師

**用跨平臺連線更多玩家**

> _遊戲中的社群模組一直很受使用者歡迎。統計資料表明，每月有近千萬玩家使用社群模組在遊戲中分享螢幕錄影等內容。_
> 
> 汪增灝，PUBG MOBILE 開發團隊高階產品經理

![](https://devrel.andfun.cn/devrel/posts/2023/02/rZVjEk.gif)

△ PUBG MOBILE 豐富的社群功能由 Flutter 打造

展示戰利品、時裝、精彩錄屏……玩家們一次次精彩的對抗，也是一段段難忘的時光。社群功能的存在，是讓這些時光得以留存，讓玩家們彼此相連，讓快樂從一個人傳遞給更多的人。

節省程式碼、平台統一、效能優秀、易於上手，則是 Flutter 帶給開發者們的快樂。跨平臺的遊戲社群體驗，毫無疑問是 PUBG MOBILE 遊戲在成功路上的重要一環。

"用上 Flutter，今晚吃雞！"
