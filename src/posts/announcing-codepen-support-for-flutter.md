---
title: CodePen 現已支援 Flutter
description: 我們很高興宣佈 CodePen 現已提供 Flutter 支援。
---

文/ Zoey Fan, Flutter 產品經理

我們很高興宣佈 [CodePen](http://codepen.io/) 現已提供
[Flutter 支援](https://codepen.io/flutter)。
作為世界領先的社交開發環境，CodePen 為數百萬開發者和設計師
帶去了出眾的前端開發與社交體驗。
長久以來，CodePen 一直是網頁開發者們共享設計方案、前沿技術與創意靈感的交流平台。
在引入 Flutter 支援後，CodePen 將惠及更多的開發者，
為他們提供學習、分享和推廣創意的絕佳機會。

> " CodePen 一直是 Flutter 和 Dart 語言的忠實粉絲。我們很高興看到 Flutter 對移動和 web 平台的支援。Flutter 社群的發展速度令人驚歎，為此，我們特別為 Flutter 客製了一款 CodePen 編輯器。Flutter 現已成為 CodePen 大家庭的重要成員之一。十分期待各位開發者在 CodePen 上創造出色的 Flutter 作品。"
—— CodePen 聯合創始人 Alex Vazquez

自誕生第一天起，Flutter 就一直是開發者們揮灑靈感的創意平台。
成長至今，Flutter 的設計功能也收穫了越來越多的認可，
例如，我們在 Flutter Interact 大會上宣佈與 Adobe 和 SuperNova 結成合作夥伴；
Flutter 被 Fast Company 評選為
[十年來最具影響力的設計理念](https://www.fastcompany.com/90442092/the-14-most-important-design-ideas-of-the-decade-according-to-the-experts )
之一等等。
現在，基於 CodePen 的 Flutter 編譯環境也與大家見面了。
創意工作者們可透過 CodePen 尋找 Flutter 藝術靈感，
打造獨具魅力的 Flutter 作品，並向全世界展現別出心裁的 Flutter 創意！

CodePen 上的 Flutter 編輯器和 DartPad 採用的是同一套後端服務，
即 [dart-services](https://github.com/dart-lang/dart-services)。
[DartPad](https://dartpad.cn) 是一款由 Flutter 及 Dart 團隊聯合開發的線上編輯器，
它已在近期的一次更新中添加了 Flutter 支援。
在建構 DartPad 的過程中，我們希望把它設計成一款實用的學習工具，
用於幫助開發者掌握 Flutter 和 Dart，
並與其他人分享程式碼片段。此外，我們還特別開源了dart-services，
這樣一來，CodePen 或其它網站便能根據特定情況對其進行修改，
從而滿足新場景或使用者的客製需求。

DartPad 允許您在程式碼中快速測試想法或與其他開發者分享程式碼片段，
此外，它對於重現 (和提交) 錯誤也十分有用。
CodePen 除了擁有這些功能之外，還讓您有機會加入活力滿滿的設計師社群。
您可以在社群裡分享、評論、推廣和嘗試您的設計創意，
並收穫其他設計師的建議與反饋。
如果以人類大腦來作類比，
CodePen 上的 Flutter 編輯器相當於激發創意表達與設計靈感的 "右腦"，
而 DartPad 則更像是您的 "左腦"，
它會在您需要快速測試想法或者用程式碼實現某個技術概念的時候發揮作用。

![codepen-demo](https://files.flutter-io.cn/posts/flutter-cn/2020/announcing-codepen-support-for-flutter/codepen-demo.gif){:width="95%"}

## CodePen 上的 Flutter 編輯器

接下來，我們將帶您快速瞭解一下 CodePen 上的 Flutter 編輯器。
您可以透過以下兩種方式建立一個新的 Flutter 畫筆 
(CodePen 將程式碼片段命名為 "Pen", 即 "畫筆"): 
[從零開始](https://codepen.io/pen/editor/flutter) 或者
[基於現有範本](https://codepen.io/topic/flutter/templates)。
十分感謝 Flutter 社群的積極貢獻，
包括 @aednlaxer, @ayushnishad, @diegoveloper, @divyanshub024, @egorbelibov, @gskinnerTeam, @mkiisoft, @orestesgaolin, @SlaxXxX 
在內的眾多傑出開發者為我們提供了豐富的精美範本。

首先為您介紹的是 ["GooeyEdge" 範本](https://codepen.io/zoeyfan/pen/ExVaXGK)。
正如下圖所示，介面的左邊是 Flutter 程式碼，
右邊則是 Flutter 網頁的輸出樣式。
您只需用滑鼠拖動素材邊框，便可檢視該設計的互動效果。

![GooeyEdge-demo](https://files.flutter-io.cn/posts/flutter-cn/2020/announcing-codepen-support-for-flutter/Gooey-edge-animation.gif){:width="95%"}

另外，您還可以對 Flutter 程式碼進行修改並檢視相應的輸出效果。
例如，如果我們把頁面控制圖示的顏色從 "白色" 改成 "藍色" (第 326 行程式碼)，
短短几秒鐘後，您便能看到更新後的顏色。
CodePen 會為您自動重新編譯修改後的程式碼。
您只需更新一下程式碼，等待幾秒鐘，就可以看到新的輸出。

![頁面控制圖示的顏色變為藍色](https://files.flutter-io.cn/posts/flutter-cn/2020/announcing-codepen-support-for-flutter/page-control-indicator.png){:width="95%"}

接下來讓我們看看如果出現語法錯誤會發生什麼。
假設我不小心刪除了第一行程式碼末尾的分號，編輯器會立即顯示一條紅色的警告資訊，
提醒我程式碼中含有語法錯誤。這些警告資訊可幫助您輕鬆地發現並更正錯誤。

![出現語法錯誤時的警告資訊](https://files.flutter-io.cn/posts/flutter-cn/2020/announcing-codepen-support-for-flutter/debug-codepen.png)

## 社交功能

CodePen 最值得稱道的就是其豐富的社交和社群功能。
當您建立新畫筆或者發現社群內的 Flutter 畫筆後，
您可以儲存、收藏、新增到合集、分享至社交平台，
甚至可以透過復刻專案以建立自己的版本。

![社交功能](https://files.flutter-io.cn/posts/flutter-cn/2020/announcing-codepen-support-for-flutter/social1.png){:width="90%"}

![社群分享](https://files.flutter-io.cn/posts/flutter-cn/2020/announcing-codepen-support-for-flutter/social2.png){:width="90%"}

## 上手體驗

我們希望 [CodePen](https://codepen.io/flutter) 上的 Flutter 編輯器能夠成為您玩轉創意的新天地，
讓您盡情建構和展示 Flutter 動畫、想法、插圖等豐富內容。

原文: [Announcing CodePen support for Flutter](https://medium.com/flutter/announcing-codepen-support-for-flutter-bb346406fe50) /
中文釋出：谷歌開發者公眾號