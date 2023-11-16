---
title: 所見即所得 — Adobe XD 的 Flutter 外掛
description: 我們很高興宣佈 CodePen 現已提供 Flutter 支援。
---

文/ Tim Sneath, 谷歌 Flutter & Dart 產品經理

Flutter 希望成為任您揮灑創意的畫布。
憑藉在 iOS 和 Android 上的原生效能體驗、
對螢幕上每個畫素點的精確控制
以及透過有狀態熱重載實現的快速迭代能力，
我們希望釋放設計師和開發者的潛力，
打造不受技術限制的精美體驗。

在去年的 Flutter Interact 大會中，我們將焦點轉向了那些使用 Flutter 進行探索和實驗的創作者們。比如數字藝術家 Robert Felker，他使用 Flutter 透過產生演算法 (generative algorithm) 建構了 [空靈的視覺效果和形式](https://v.youku.com/v_show/id_XNDQ2ODg0OTYxMg==.html)。我們還介紹了來自 [gskinner](https://flutter.gskinner.com/) 等創意機構的作品，他們用一件件充滿創意的作品，展示了 Flutter 在表現形式方面的無限可能。另外，我們也瞭解到 Adobe 在 Flutter 方面的投入: 他們展示了一款 Adobe XD 外掛的早期原型，讓大家可以直接從 XD 中匯出 Flutter 程式碼。

![generative-artwork](https://files.flutter-io.cn/posts/flutter-cn/2020/announcing-adobe-xd-support-for-flutter/generative-artwork.png){:width="90%"}

△ Flutter 為創意提供了極富表現力的舞臺，創造者可以盡情呈現優美、原生的體驗，且不再受傳統技術的束縛。(由 Flutter 繪製的產生藝術，Robert Felker 作品)

今天 **我們很高興和 Adobe 共同宣佈，Adobe XD Flutter 匯出外掛現在正式開放早期體驗** ，歡迎大家踴躍參與測試。[Adobe XD](https://www.adobe.com/products/xd.html) 是一款 UI/UX 設計和協作工具，幫助團隊創造和分享網站、應用、語音介面以及遊戲等內容的設計方案。作為業界知名的 [Adobe Creative Cloud](https://www.adobe.com/creativecloud.html) 套件中的一員，XD 讓創作者們可以將向量繪圖、文字、圖像、小互動和動畫資源共冶一爐，打造出可以互動的原型，來預覽軟體產品實際的執行效果。隨著 Flutter 匯出功能的加入，XD 原型現在可以在幾分鐘內轉變成可用的 Flutter 程式碼，創意想法和產品開發的間隔被進一步縮短。Adobe XD 支援 Windows 和 macOS 系統，並且提供了[免費的入門計劃](https://www.adobe.com/products/xd/compare-plans.html)，方便大家快速上手。

![flutter-plugin](https://files.flutter-io.cn/posts/flutter-cn/2020/announcing-adobe-xd-support-for-flutter/flutter-plugin.png){:width="90%"}

△ 使用外掛即可輕鬆從 Adobe XD 匯出到 Flutter

## 從 Adobe XD 中匯出 Flutter 程式碼

在 XD 中使用 Flutter 外掛很簡單。您可以匯出一個單獨的繪圖物件或元件，也可以匯出整個畫板 (artboard)。具體做法如下:

首先，安裝 Flutter 匯出外掛。在 Adobe XD 中，選擇 *外掛* > *發現外掛* ( *Plugins* > *Discover Plugins* )，然後搜尋 Flutter。安裝完成後，選擇 *外掛* > *Flutter* > *UI 面板* ( *Plugins* > *Flutter* > *UI Panel* )，即可顯示上圖中的 UI 面板。

現在將 [adobe_xd package](https://pub.dev/packages/adobe_xd) 新增到您的 Flutter 專案中，只需將其包含在您的 pubspec.yaml 檔案中即可。這個 package 提供了幫助函式，用來減少產生的 XD 程式碼中的樣板程式碼。

要匯出單個元素，只需選擇您想匯出至 Flutter 的單個 widget，然後點選 UI 面板中的 *複製所選項* ( *Copy Selected* ) 按鈕。這會將元素對應的 Dart 程式碼複製到您的剪貼簿中，您可以基於這些程式碼打造有狀態或無狀態的 widget:

![flutter-plugin](https://files.flutter-io.cn/posts/flutter-cn/2020/announcing-adobe-xd-support-for-flutter/export-demo.png){:width="90%"}

△ 匯出的程式碼可以整合進現有的專案中，而且更新時不需要調整其他檔案

另一種方法是匯出整個專案。假設您已經有了一個 Flutter 應用，並且您想把內容新增到這個應用裡 (包括 pubspec.yaml 中對 adobe_xd package 的參考)，您只需從 UI 面板中選擇 *外掛* > *Flutter* > *匯出全部 Widget* ( *Plugins* > *Flutter* > *Export All Widgets* )，然後設定想要的附加選項即可。

這個操作會在專案的 lib/ 子資料夾中建立一系列的類，您可以直接使用。您也可以繼續調整 XD 原型，然後用 ⇧⌘F (在 Windows 上是 Ctrl+Shift+F) 再次匯出，如果您在 Visual Studio Code 中打開了 Dart 的 "[在 Save Watcher 上使用熱重載](https://dartcode.org/docs/settings/#dartpreviewhotreloadonsavewatcher)" 選項，那麼當您重新匯出 widget 時，您的應用將自動重新載入它們。

![flutter-plugin](https://files.flutter-io.cn/posts/flutter-cn/2020/announcing-adobe-xd-support-for-flutter/live-demo.png){:width="90%"}

△ 從 XD 快速轉出程式碼的功能，使得從原型到應用之間的路徑又多了一條

作為早期體驗的預覽版，這個外掛現在也有一些限制，請閱讀 [釋出說明](https://github.com/AdobeXD/xd-to-flutter-plugin/blob/master/README.md#using-this-plugin) 瞭解詳情。值得注意的是，響應式佈局目前還不能使用，尚需等待新的 XD API 完成。不過請放心，當這些新功能上線時，您會自動獲得外掛更新。


> Adobe 致力於幫助那些設計和打造應用的團隊，簡化讓他們頗為困擾的設計-開發流程。今天我們很高興推出這個全新工具的早期體驗版，它誕生自我們與 Flutter 的合作，旨在消除設計-開發流程中含糊的溝通環節，提高決策速度，便於團隊更快地將新體驗推向市場。
—— Vijay Vachani, Adobe Creative Cloud 平台與生態資深總監

請存取 Adobe 的 [XD Flutter 匯出外掛頁面](https://github.com/AdobeXD/xd-to-flutter-plugin) 瞭解更多資訊。我們期待看到您用它建立的作品！

原文：[Announcing Adobe XD support for Flutter](https://medium.com/flutter/announcing-adobe-xd-support-for-flutter-4b3dd55ff40e) 
中文釋出：谷歌開發者公眾號
