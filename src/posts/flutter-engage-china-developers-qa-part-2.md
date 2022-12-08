---
title: Flutter Engage China 開發者常見問題解答 — 下篇
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/04/14dcfcd1d2a09.jpg)

再次感謝大家對 [Flutter Engage China 活動](https://flutter.cn/posts/flutter-engage-china-event-recap) 的關注和積極參與！我們在活動前後收到了很多來自開發者的反饋和問題，Flutter 團隊和演講嘉賓在直播 Q&A 環節中也針對部分問題在第一時間給出回覆。現在我們將一些開發者關心的問題和回覆整理出來分享給大家，希望對您有所幫助。由於問題數量較多，我們分為上下兩期釋出，上期的內容已經發布，歡迎大家 [前往回顧](https://flutter.cn/posts/flutter-engage-china-developers-qa-part-1)。您也可以觀看 Flutter Engage China 影片回顧精彩內容:

[**➡️點選這裡檢視直播回顧影片⬅️**](https://www.bilibili.com/medialist/play/ml1214246458/BV1hh411D7mV)

* Bilibili 影片合集連結 [https://www.bilibili.com/medialist/play/ml1214246458/BV1hh411D7mV](https://www.bilibili.com/medialist/play/ml1214246458/BV1hh411D7mV)


## **為什麼 Flutter SDK 整合的內容這麼少，寫個簡單的控制項都需要依賴第三方外掛，不能多擴充一下控制項功能嗎？**

*回答者: 楊天航 (Chris)，Google Flutter 團隊工程師*

在一開始決定哪些內容應該整合到 SDK 裡的時候，我們主要考慮這麼三個因素:

* **降低二進位制檔案尺寸** 。
* **希望框架裡的 API 在不同平臺上比較統一** 。所以如果我們在框架或引擎里加入一些控制項的功能的話，會讓我們無法去統一 API。
* **讓 Flutter 框架非常模組化** ，中間有很多模組，很多層級，這樣我們可以很容易地進行替換。假如我們把影片播放器控制項建立在 Flutter 引擎和框架裡，那就很難避免大部分的 Flutter 應用只能選擇我們提供的影片播放器 API 的局面。這樣其實很不方便，尤其對於那些有不同需求的應用。如果是以現在外掛的形式提供影片播放器的話，大家都可以找到合適的解決方案。不同的團隊也可以建立自己的外掛，這無疑會讓大家有更多選擇的餘地。

## **Flutter 包體積如何縮減？**

*回答者: 董韜，Google Flutter 使用者體驗研究負責人*

我們一直在針對這個問題做最佳化工作。套件的精簡主要包括兩個方面的工作: 第一個方面是 Flutter 團隊為全球所有使用者做的全域最佳化。第二個方面是每個 App 團隊在自己的 App 內部做的最佳化。

從第一個方面來講，我們去年對大的編譯過程做了很多最佳化，主要是在 iOS 端。iOS 端產物的體積已經有了比較顯著的縮小。目前來看，想要進一步縮小包產物的話，需要更多的由開發者根據自己 App 的實際情況做一些取捨，以及對 Flutter 引擎做一定的客製。

我們最近也推出了一款工具，在 Dart DevTools 裡面，叫做 [Code Size Analysis](https://flutter.cn/docs/development/tools/devtools/app-size#analysis-tab)，這個工具可以幫大家視覺化包裡具體有哪些內容，每個內容佔用了多少空間。有的時候您可能會發現，有一些資源或者有一些依賴的函式庫佔用了過多的空間。這個工具也會幫您解析 Flutter 引擎裡面具體的產物是什麼，如果您有計劃去對 Flutter 引擎進行客製的話，這些都會是很好的參考資料。

*回答者: 袁輝輝，位元組跳動 Flutter 技術負責人*

我簡單從 App 團隊的角度來做一些補充。App 團隊想縮小包體積的話，有三個可以做的最佳化。一個是壓縮，主要是我們在程式碼端和資料端，可以對資料做一些壓縮。二是裁剪，您可以看看哪些模組是不使用的，比如說您是國內使用的 App，那就並不需要國際化的一些功能模組，就可以裁剪掉。三是系統級的最佳化，比如說大的編譯由 O3 變 Oz，或者是做指令集頭部的一些精簡，把 code source map 這樣的東西去掉，以及做一些混淆，到了線上之後再通過後台平台還原回來，能使用的手段其實很多的。

我們接下來會加大對外的技術輸出，後續我們會把這些技巧再梳理一下分享給大家。

## **類似微博那種大量音訊、影片，以及文字和圖片混排導致的效能問題該怎樣解決？**

*回答者: 劉森森，阿里巴巴 UC 客戶端團隊*

這個問題其實可以理解為在這種複雜卡片的場景下，在列表做慣性滾動的時候出現的一些效能問題。這個問題非常典型，實際上在應用層有很多最佳化手段可以去做。

首先我們可以使用 Flutter 提供的效能分析工具來定位問題，去發現是否在寫法上面存在著一些可以最佳化的地方。比如在列表滾動的時候，是否存在 widget 建構的次數過多，或者建構的層級過深的問題，導致 UI 執行緒出現卡頓。我們也可以使用 Flutter 提供的 [RepaintBoundary](https://api.flutter-io.cn/flutter/widgets/RepaintBoundary-class.html)，來減少重繪的範圍。並且檢查是否使用到了 ClipPath 或者 BackdropFilter 這種可能會對 Raster 執行緒造成一定影響的操作，是否可以儘量去避免。

在最佳化完寫法後，可以去評估使用 SurfaceView 作為 FlutterView 渲染的實現。SurfaceView 從原理上來講比 TextureView 的效能更好，因為它有獨立的渲染管線。從實際測試的效果來看，SurfaceView 的幀率平均有 2~4 幀的提升。它的問題可能存在於混合開發的場景，由於它跟 AndroidView 不相容，會導致黑屏等相容性問題。我的建議是，如果是開發一個完整的 Flutter App 的話，優先選用 SurfaceView；如果做混合棧開發的話，需要評估 SurfaceView 是否存在導致相容性問題的場景，然後儘量去使用 SurfaceView。

如果前兩步都沒有發現問題的話，可以使用我分享的分幀渲染的思路，也可以減少 UI 和 Raster 執行緒的繪製耗時。

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=247451014&bvid=BV1wv411h7Ni&cid=318242333&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

回到引擎層面上，UC 這邊做的一些最佳化也會以 PR 的形式回饋到社群，大家可以關注我們的進展。

## **教育培訓領域的應用適合使用 Flutter 嗎？**

*回答者: 袁輝輝，位元組跳動 Flutter 技術負責人*

教育這一塊的話，我們在位元組內部其實用 Flutter 比較多的就是我們的教育領域產品。我有了解到，很多公司會針對一款教育產品，按照課程進行拆分，比如數學、英語、思維，都會拆分出一個個獨立的 App，這個開發的工作量是非常大的。在剛進入一個新的行業時，人員招聘可能一下子跟不上來，這個時候其實我們會很在乎一個技術能不能讓研發效率實現快速提升，這時 Flutter 就是一個比較好的選擇。

昨天我還跟我們公司外的教育公司聊到 Flutter。他們現在有一款大的 App，想要拆成很多個教育子方向的 App，可能會裂變成 3 個、5 個甚至 10 個，但是他們團隊可能 Android 和 iOS 端加在一起也只有幾個開發小夥伴。聊下來的結果就是，Flutter 是一個很好的切入方式。我覺得不光是教育行業，其實對於一些新 (領域) 的 App，如果您比較注重研發效率，用 Flutter 都是一個不錯的選擇。

## **高延遲渲染管線如何分幀，是否使用 Isolate 分執行緒？**

*回答者: 劉森森，阿里巴巴 UC 客戶端團隊*

高延遲渲染管線，其實它的分幀還是以 16 毫秒為週期輸出的，就是一個 Vsync 週期，這一點是沒有改變的。實現起來的話，主要是把原本必須在一個 Vsync 週期完成的任務增大到了兩個 Vsync 週期，由於 UI 執行緒和 Raster 執行緒是並行的，從而增大了輸出的吞吐量，Raster 執行緒始終是每一個 Vsync 週期輸出一幀。大家可以結合 Android 的 DoubleBuffer 去理解，會更容易一些。在執行緒方面，我們沒有新建，而是在原本的 UI 執行緒和 Raster 執行緒上去操作的。這些改動只是在引擎層，上層不用去做什麼。

## **請問 Flutter 在汽車系統上做了哪些工作？豐田採用 Flutter 開發的車機系統會及時開源嗎？**

*回答者: 樊舟穎 (Zoey)，Google Flutter 產品經理*

上次在 [Flutter Engage 活動](https://flutter.cn/posts/announcing-flutter-2) 上，我們邀請豐田來講解了一些他們現在 Flutter 的部署規劃。豐田現在還沒有公佈具體的計劃，以及是否開源。如果大家有這個需求的話，我們會和豐田聊一聊，向他們提出這方面的建議。但我們不是豐田，不能代表他們來做出決定，如果他們後續有相關的開源計劃的話，我們會和大家分享。

. . .

以上就是 Flutter Engage China 開發者常見問題解答 (下篇) 的內容，大家也可以隨時回顧之前釋出的 [上篇內容](https://flutter.cn/posts/flutter-engage-china-developers-qa-part-1)。如果您有任何疑問或者建議，歡迎大家 [GitHub](https://github.com/flutter) 積極分享您的反饋與想法.

