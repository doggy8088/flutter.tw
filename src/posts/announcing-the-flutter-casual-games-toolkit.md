---
title: 一起看 I/O | Flutter 休閒遊戲工具套件釋出
toc: true
keywords: 遊戲工具套件
description: 幫助您的遊戲從概念走向成功釋出的入門套件。
image:
    path: https://devrel.andfun.cn/devrel/posts/2022/05/dSsVED.png
---

![](https://devrel.andfun.cn/devrel/posts/2022/05/dSsVED.png)

*作者 / Zoey Fan, Product Manager for Flutter, Google*

對於大多數開發者來說，Flutter 是一個應用框架。但利用 Flutter 提供的硬體加速圖形支援，圍繞休閒遊戲開發的社群也在不斷壯大。

在過去的一年裡，已經有數千款 Flutter 遊戲釋出。拿遊戲公司 Lotum 來舉例，該公司旗下頗具人氣的文字解謎遊戲 [4 Pics 1 Word](https://flutter.dev/showcase/lotum "Flutter 案例: 4 Pics 1 Word") 最近就已經用 Flutter 完全重新編寫。[Flame](https://flame-engine.org/ "基於 Flutter 一款開源遊戲引擎 Flame Engine 主頁") 則是一款基於 Flutter 建構的、由社群驅動的開源遊戲引擎，其貢獻者和使用者也一直在穩步增長。

![△ Flutter 開發者建構的手機遊戲](https://devrel.andfun.cn/devrel/posts/2022/05/7PH1QO.png)

△ Flutter 開發者建構的手機遊戲

為了瞭解我們如何才能為大家提供更好的支援，我們 [採訪了幾位](https://medium.com/flutter/perspectives-from-early-adopters-of-flutter-as-a-game-development-tool-f95fb3406d51 "將 Flutter 用於休閒遊戲的早期採納者的訪談") 已經成功建構併發布 Flutter 移動端遊戲的開發者。我們詢問他們喜歡用 Flutter 建立遊戲的原因，他們的回答給出了下面幾個原因:

* Flutter 簡單易學，可以輕鬆用於建構使用者介面和休閒遊戲
* Flutter 允許開發者檢視框架原始碼 (不存在 "黑盒")，並能完全控制畫布
* Flutter 擁有開放的生態系統，開發者可以找到 (並使用) 許多有用的 package、外掛和開發庫
* Flutter 是可移植的，只需寫一次程式碼，遊戲就預設可以編譯為多平臺版本

同時，參與我們調研的開發者有提到，建立 Flutter 遊戲的最大挑戰是尋找用於入門的資源和學習材料，以及與平台遊戲服務進行整合。為了解決這些需求，我們釋出了新的入門工具套件，以加速您的遊戲開發處理序。

[Flutter 休閒遊戲工具套件](http://flutter.dev/games "Flutter 休閒遊戲工具套件") (Casual Game Toolkit) 提供了專門的範本 (由 [Filip Hracek](https://github.com/filiph "Filip Hracek 的 GitHub 個人頁面") 開發)，您可以用它來建構自己的遊戲。這個啟動專案提供了預先建構完畢的各種您可能用得著的「零部件」，包括主選單、設定頁面、聲音支援等，最有趣的部分仍然留給您: 建構遊戲！

![△ 在移動裝置上執行的井字棋遊戲](https://devrel.andfun.cn/devrel/posts/2022/05/qmT3RE.png)

△ 在移動裝置上執行的井字棋遊戲

## **影片課程**

如果想要上手開發遊戲，請檢視我們準備的關於如何使用遊戲範本的 [影片課程](https://www.bilibili.com/video/BV12Y4y1z7m9/ "影片課程：使用 Flutter 開發遊戲")。它為您提供了建立一個 [井字棋遊戲](https://github.com/filiph/tictactoe "井字棋遊戲的 GitHub 開源儲存庫地址") (您也可從 [iOS](https://apps.apple.com/us/app/tic-tac-toe-puzzle-game/id1611729977 "井字棋遊戲 iOS 版下載地址")/[Android](https://play.google.com/store/apps/details?id=dev.flutter.tictactoe "井字棋遊戲 Android 版下載地址") 應用商店下載這個遊戲) 的逐步教學。如果您想要了解 Filip 在開發這款遊戲時經歷的所有細節，歡迎閱讀他的 [遊戲開發日誌](https://files.flutter-io.cn/posts/flutter-cn/2022/announcing-the-flutter-casual-games-toolkit/flutter-game-sample-devlog/ "Filip 的遊戲開發日誌")。

## 預整合的服務

除了在遊戲中可能需要用到的常見使用者介面和功能元素之外，您還會得到遊戲開發所需的關鍵服務的預整合模組和範例程式碼。例如，遊戲範本中就集成了 Apple Game Center 和 Google Play Games Services，因此您可以輕鬆地實現排行榜和成就等功能。

如果您打算透過遊戲盈利，範本還使用了 [Google 移動廣告 SDK](https://pub.dev/packages/google_mobile_ads "Flutter package: Google 移動廣告 SDK")，並告訴您如何實現廣告樣本。該範本還使用了 [應用內購買](https://pub.dev/packages/in_app_purchase "Flutter package: 應用內購買") package，可讓您在遊戲中為玩家提供額外的內容，如高階體驗、數字商品和訂閱等。

最後，遊戲範本還包含 [Firebase Crashlytics](https://pub.dev/packages/firebase_crashlytics "Flutter package: Firebase Crashlytics")，讓您可以獲得更多關於遊戲中可能發生的崩潰和錯誤的洞察。該遊戲範本的所有原始碼都可以在 GitHub 上的 [Flutter 範例 repo](https://github.com/flutter/samples/tree/master/game_template#readme "Flutter 範例程式碼儲存庫") 中找到。

## **Flutter 遊戲 Discord 頻道**

Flutter 的優勢之一，在於它是充滿溫度、樂於助人的社群。如果您想要與其他 Flutter 遊戲開發者聊天、提問和分享最佳實踐，請加入 r/FlutterDev Discord 伺服器上的 [遊戲開發專屬頻道](https://discord.gg/WY5NwwjBQz "Flutter Discord 遊戲開發專屬頻道加入連結")！非常感謝我們的社群成員 @Miyoyo 幫助建立這個社群空間來支援 Flutter 遊戲開發者！(順便說一下，如果您已經是 r/FlutterDev 社群的成員，可以透過 [連結](https://discord.com/channels/420324994703163402/964110538986651658 "Flutter Discord 遊戲開發專屬頻道連結") 直接加入)。

## **Google Ads 和 Cloud 贊助額度**

如果您的遊戲需要 Cloud 或 Firebase 服務，或者您想使用 Ads 向更多使用者推廣您的遊戲，您可以獲得由 [Google Ads](https://ads.google.cn/intl/en_us/home/flutter/#!/ "Google Ads 額度贊助頁面") 和 [Cloud](https://cloud.google.com/free "Google Cloud 額度贊助頁面") 團隊提供的高達 900 美元的贊助額度 (受條款限制)！

## **由移動端開始，走向更廣闊舞臺**

從過去的研究中我們瞭解到，現在的 Flutter 遊戲大多是休閒手機遊戲，所以我們在設計 Flutter 休閒遊戲工具套件時優先考慮了移動場景。

但這並不意味著您的創意應該侷限於行動平台。事實上，剛才提到的 [井字棋遊戲](https://github.com/filiph/tictactoe "井字棋遊戲開原始碼儲存庫地址") 除了在 [web 端執行](https://filiph.github.io/tictactoe/ "井字棋遊戲 Web 端執行網址") 外，也能在桌面端執行！

![△ 在 web 端和桌面端執行的井字棋遊戲](https://devrel.andfun.cn/devrel/posts/2022/05/GwGAuu.png)

△ 在 web 端和桌面端執行的井字棋遊戲

我們最近還在 DartPad 上增加了對社群驅動的遊戲引擎 Flame 的支援，因此您可以在 DartPad 上探索 [用 Flame 建構遊戲](https://dartpad.cn/?id=3e52ca7b51ba15f989ad880b8b3314a2 "DartPad 直接體驗用 Flame 建構遊戲")，而無需下載 SDK。另外，由 Very Good Ventures (VGV) 編寫，並在 Google I/O 大會上推出的 web 端 [彈球遊戲](https://pinball.flutter.dev/ "Flutter 彈球遊戲")，就是使用 Flame 引擎在 Flutter 中建構的！如果您想要了解該彈球遊戲是如何建立的，請關注我們近期的推送或直接 [閱讀其程式碼](https://github.com/flutter/pinball "Flutter 彈球遊戲開原始碼")。

![△ Web 端的彈球遊戲](https://devrel.andfun.cn/devrel/posts/2022/05/tzNjv7.png)

△ Web 端的彈球遊戲

遊戲開發是 Flutter 涉足的全新且令人興奮的場景！展望未來，我們希望增加更多的 Codelab 和其他資源，來幫助您開發遊戲。這是我們第一次嘗試在這方面為您提供更多便利，我們充分知曉還有許多地方需要改進。我們最近開始與社群成員 [@wolfenrain](https://github.com/wolfenrain "@wolfenrain 的 GitHub 頁面") 合作，對遊戲相關的問題進行分流。如果您希望 Flutter SDK 提供更好的產品功能，請在 GitHub 上提交 issue (或對現有 issue 進行投票)。

## **Flutter 遊戲開發**

請檢視專門的 [遊戲開發頁面](http://flutter.dev/games "使用 Flutter 進行遊戲開發頁面") 以瞭解更多關於上述資源的詳細資訊，您還可以找到 [文件連結](http://docs.flutter.cn/docs/resources/games-toolkit "Flutter 遊戲開發參考文件")，以及 Flutter 社群中游戲開發專家推薦的程式碼庫、package 和工具的參考資訊。

自 Flutter 1.0 釋出以來，大家不斷用精彩的應用為我們帶來驚喜，現在我們迫不及待地想看到您將用 Flutter 打造出多麼令人興奮的遊戲了！
