---
title: 使用 Flutter 加速應用開發
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/04/c1e6141049825.gif)

*作者 / Larry McKenzie*

> 本文由 eBay 技術負責人 Larry Mckenzie 和 Corey Sprague 撰寫。您可以收聽他們在 Google [Apps, Games & Insights 播客](https://zhuanlan.zhihu.com/p/337692560) 中的討論，詳細瞭解如何使用 Flutter 建構應用。

建構原生應用本來就不輕鬆，如果需要在不到一年的時間內構建出一款應用，同時還要滿足目標受眾的嚴格要求，就更加困難了。我們在建構 eBay Motors 應用時，就遇到了這樣的挑戰。但在 Google 的介面工具套件 Flutter 的幫助下，我們得以快速針對 Android 和 iOS 平台提供優質且一致的體驗。

## **eBay Motors**

過去十幾年來，eBay 在汽車市場的業務一直十分活躍。但在幾年前，我們的領導層發現了一個機遇: 我們可以在 eBay 平台提供客製化的車輛買賣服務。購買汽車不同於大多數線上購物: 購車是一項較為重大的消費，而且銷售過程中存在一些特殊的問題需要考慮。eBay 傾向於上架經典車型、稀有車型、改裝車等獨特品類別的車輛。eBay賣家往往想要為自己的愛車找到真正懂得欣賞和珍惜的買家。很多 eBay 買賣雙方都在尋找更加個性化、更為獨特的服務，所以我們想為這些使用者建構一款特殊的應用——eBay Motors 應用便應運而生。

## **目標受眾**

eBay Motors 的目標受眾是廣大汽車愛好者。儘管愛好者們每天都會關注汽車，但可能幾年才會買一次車。因此，打造一款能夠讓他們願意定期使用的應用並不容易。開發團隊著重研究了這些人的使用動機，以及他們希望與其他汽車愛好者互動的方式。

買家可能正在物色下一輛車，或者只是隨便看看或尋找靈感。在考慮購買車輛時，買家希望在決定購買之前充分獲取相關資訊。他們希望能夠從各個角度檢視車輛 - 內飾、外觀、發動機、傳動系統。他們想了解車況，也想了解賣家。當前車主的用車情況怎樣？他們每天都開車嗎？車輛是否在修理廠維修拋光過？車輛是每年行駛一次，只在車展上展出嗎？我們想建構一種可以充分滿足這些需求的體驗。

開發團隊在與賣家談論出售車輛方面的問題時，發現賣家明顯格外關心買家是誰，而並非總是關心價格。一些賣家表示，如果他們覺得買家真的會悉心呵護他們的愛車，那麼降價出售並非不可接受。這種態度與傳統汽車市場大相徑庭，在傳統汽車市場中，賣家總是希望能夠以最高價格出售車輛。

在調查中，人們認為買賣雙方的關係並沒有在交易達成後就結束。賣家希望能夠追蹤愛車情況，並繼續與他們的買家交流。買家同樣希望保持聯絡，以便詢問: "嗨，我想改裝這裡，但我發現你之前改裝過其他部位。可以給我講講具體情況嗎？" 因此，我們希望建構一些功能，使買賣雙方形成一個不僅僅只是討論交易的社群。

## **選擇 Flutter**

在開發這款應用時，eBay Motors 的領導層為開發團隊提供了很大的自主權。唯一的要求是: 這款應用必須在一年內完成開發。根據我們的調查，顯然這款應用必須包含 eBay 使用者所期望的全部功能: 鉅細無遺的商品詳情頁、拍賣、訊息功能、搜尋等。開發團隊還積極地添加了有助於培養活躍社群的功能。這意味著我們的工作範圍十分龐雜，我們認為，如果依然沿用兩支獨立平臺開發團隊的模式，將無法在截止日期前完成任務。

顯然，我們需要一種新的應用開發方法。開發團隊之前對跨平臺開發工具進行過一些評估，但並沒有找到令我們滿意的解決方案。

但在這個專案開始時，Flutter 1.0 釋出了。Flutter 是 Google 的介面工具套件，可基於單個程式碼庫為移動、網路和桌面端建構富有吸引力的原生編譯應用。這看起來很有前景，所以我們的開發團隊開始了更加詳細的調查。

對 Flutter 進行徹底的評估後，開發團隊對其相當滿意。短短几周內，開發團隊便確信 Flutter 是開發這款應用的最佳選擇。

eBay Motors 之前組建了兩支開發團隊，分別採用不同的工作方式、工作安排和程式設計風格。在決定使用 Flutter 時，必須弄清楚應該如何協調兩支開發團隊。由於時間緊迫，因此開發團隊在如何建構應用方面必須達成一致。所有人都秉持著相同的目標 - 打造高品質的產品、快速交付並超越預期。

兩支團隊開會討論了所有分歧，儘管雙方各有讓步，但最終結果非常協調，這也為順利釋出產品做好了充分準備。

兩支團隊都缺乏 Flutter 和 Dart 的使用經驗，但官方提供的指引非常詳盡，學習起來非常簡單。

## **首個版本**

我們在 2019 年 3 月收到了第一份產品需求——我們需要在三個月內向我們的 CEO 交付包含可用交易體驗的 Beta 版應用。

第一個版本需求因為時間緊張，迫使團隊大幅削減應用的功能。我們知道，即便在 Flutter 的幫助下，我們也不可能一次性解決所有問題——無法構建出包含所有買賣功能的完整社群和完善的訊息功能。我們必須制定合理的開發策略，確定需要優先開發的重點功能，而非同時開發所有功能。團隊透過討論削減哪些功能，總結出了大量關注重點。這樣我們就確定了研究重點，以及隨之而來的開發重點。每個微小變化都會解鎖下一個變化，使我們共同朝著里程碑一步步邁進。我們積極地關注問題，專注地解決使用者希望我們處理的細節。這一過程困難且常常凌亂不堪，但在 Flutter 的強力助推下，我們以意想不到的方式加速追趕上了一個個任務節點。每到達一個節點，團隊都會為之振奮。

實現首個里程碑之後，我們還要攻克多個後續里程碑，而 Flutter 使得我們能夠一次性解決問題，並繼續向前邁進。我們的產品從交付給 CEO 的 Beta 版發展為 eBay 內測 Beta 版，供數千名 eBay 使用者使用。一個月後，我們的產品進入了公開 Beta 版測試階段。終於，我們於 2019 年 12 月底正式釋出了 iOS 和 Android 版應用。

對於 eBay Motors 而言，每個時刻都意義非凡。開發團隊也透過這些時刻一步步地建立了信心。將產品交付給新使用者、獲得反饋，並進行更正和調整，我們可謂 "邊做邊學"。朝著目標快速地交付小規模迭代，也有助於我們的產品團隊取得成功。即使應用的功能和複雜程度不斷提高，Flutter 也讓我們能夠持續快速地交付產品。

由於在釋出時大幅削減了應用功能，導致此時我們並沒有聊天和社群功能。因此，在 2020 年 1 月，開發團隊著手從零開始建構我們的社群，並在釋出後的幾個月內增加了 50% 的應用功能。

## **Flutter 的影響**

在剛開始採用 Flutter 時，我們只是將其視為一種支援共享程式碼並節省時間的工程工具。但 Flutter 幫助開發團隊解決了許多我們並未期待它能解決的問題。在某些情況下，它甚至解決了我們未知的問題。

我們希望確保使用者介面在各個平台之間的一致性: 將使用者體驗重點凝聚在 eBay Motors 品牌上，而非迎合特定平台的設計語言。但是，產品必須保留所在平台的具體行為，例如滾動的物理效果以及導航機制。幸運的是，Flutter 以開箱即用的方式使這些問題迎刃而解。這意味著設計師可以省去很多工作，因為他們只需要製作一份與平台無關的設計即可。

在產品需求方面，我們希望平台之間不存在差異。由於每項需求只需實現一次，所以減輕了我們不少的負擔。我們開會的次數減少了，事半功倍。

隨著開發團隊對 Flutter 的瞭解日益加深，我們意識到，程式碼共享可謂意義非凡。在我們的程式碼庫中，我們在 Android 和 iOS 之間共享了 98.6% 的程式碼。只有大約 0.5% 的程式碼為平台特定的原生程式碼。其餘部分包括我們的持續整合流水線、自動化工具和開發者支援。

在開發過程中，透過共享程式碼可以節省大量資金，而在測試方面也同樣如此。在開發團隊曾使用過的所有平台當中，Flutter 在測試方面是佼佼者之一。它的測試功能幫助我們加快了進度，並讓我們在釋出產品時信心十足。我們從一開始就採取了強制執行 100% 程式碼覆蓋率的策略，Flutter 讓我們能夠輕鬆實現這一目標。

隨著應用進入生產階段，節省資金的優勢也在持續體現。由於我們不必單獨處理 iOS 或 Android 錯誤，因此支援內容的可預測性要高得多。開發團隊可以將新的版本釋出到更具包容性的 Android 測試版渠道，之後再充滿信心地釋出 iOS 版本。由於只需建構和支援一個應用，我們能夠節省大量資金。

## **Flutter 入門技巧**

如果是初上手 Flutter，在解決移動應用中的某些問題的時候，必須擺脫先入為主的觀念，因為 Flutter 所使用的正規化與傳統 iOS 和 Android 截然不同。當您理解了 Flutter 所用的應用建構方式後，一切都將水到渠成。最好的學習方法是花些時間研究 Flutter 程式碼庫。

Flutter 的一大優點就是開源。您可以檢視 Flutter 團隊是如何建構每個元件的，從中可以收穫很多有價值的資訊。

Flutter 社群也非常活躍。如果您需要 Flutter 框架尚未提供的解決方案，那麼很可能已經有人建構瞭解決這個問題的 package。我們鼓勵您的團隊探索並查詢所需資訊，也歡迎您貢獻自己的專業知識並積極擁抱開源文化。

## **Flutter 的前景**

在 eBay Motors 剛開始使用 Flutter 時，還鮮有大型公司公開使用該框架: 它當時僅是一款小眾工具套件，似乎還缺乏實戰檢驗。時至今日，這個框架勢頭愈發強勁，很多公司向其敞開了懷抱。令人興奮的是，Flutter 提供了最為現代且最為優秀的應用開發體驗。而最棒的一點是，它仍在不斷地發展與改進。

隨著 Flutter 的發展，Dart 得到了大量的資源投入。Swift 和 Kotlin 中諸如空安全等一些必不可少的特性得到了積極的引入。看到 Flutter 的持續發展，以及這種發展為建構 Flutter 應用所帶來的改進，我們感到十分喜悅。無論公司體量大小，當大家逐漸意識到 Flutter 可以幫助他們基於單個程式碼庫開發桌面、web 和移動端應用，自然也會有更多團隊採用 Flutter。

## **結語**

雖然 eBay Motors 開發團隊的所有成員都擁有原生 iOS 或原生 Android 開發背景，但在用過 Flutter 之後，所有人都不約而同地喜歡上了它。Flutter 的開發者體驗明顯好於以往: 它可以利用熱重載功能讓開發者在進行編碼時就獲得快速反饋，而且由於開發流程的順暢，使得問題能夠快速得到解決，這些都是非常有價值的優勢。在兩年的使用過程中，驚喜無處不在。我們常能聽到同事大喊: "我愛 Flutter！我剛發現了一個新的妙招！"

對於我們這樣規模的團隊，如果沒有選擇 Flutter，就沒辦法完成 eBay Motors 應用的開發。它讓我們能夠將資源集中到一起，這是分別建構兩個平台應用的模式所無法實現的。Flutter 是幫助我們加快開發處理序的加速器。

![](https://devrel.andfun.cn/devrel/posts/2021/04/3ac77f1134a10.gif)

更多 Google Play 開發者播客節目，請移步《[Apps, Games & Insights 播客節目合輯](https://www.ximalaya.com/keji/34766927/)》，瞭解不同領域的開發者透過多種視角與主題，探討海外市場開發與發行的經驗心得。

您對使用 Flutter 建構應用有何想法？歡迎在評論區分享您的評論。