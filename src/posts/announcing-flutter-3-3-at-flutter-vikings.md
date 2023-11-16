---
title: 祝福 Eric 的下一段旅程，Flutter 3.3 現已釋出
toc: true
keywords: Flutter 3.3, Flutter Vikings, Wonderous, Impeller
description: Flutter 3.3 正式釋出，這個版本這個版本專注於完善和效能改進，以加強 Flutter 3 釋出以來的新特性。
image:
    path: https://devrel.andfun.cn/devrel/posts/2022/09/ubVHzF.jpg
---

Flutter 團隊及社群成員們在美麗的城市挪威奧斯陸向您發來問候，我們正在此參加社群舉辦的 Flutter Vikings 活動，這是一個為期兩天的開發技術交流盛會，雖然線下門票已經售罄，但您還可以透過線上方式檢視本次會議。本週，我們也有很多更新要分享給各位開發者們。

Flutter 的使用量和生態系統都在持續增長，**每天有超過 1,000 款使用 Flutter 的、新的移動應用釋出到 App Store 和 Google Play 商店**，在 Web 端和桌面端的使用也在持續增長。Flutter 生態中目前也有超過 25,000 個以上的 package，這也進一步證明了 Flutter 的成熟和廣泛應用。

![](https://devrel.andfun.cn/devrel/posts/2022/09/ubVHzF.jpg)

今天，我們正式釋出 Flutter 3.3。這個版本專注於完善和效能改進，以加強 Flutter 3 釋出以來的新特性。Flutter 3.3 加入了一些新的元件並修復了一些錯誤以加強對 Material 3 規範的支援，也加入了對 iPad 上使用隨手寫輸入文字的支援、可選擇的文字分組和觸控板支援等對平板電腦和桌面開發者有幫助的新支援。這個版本還包括了 Dart 2.18 的釋出，它為使用 Swift 和 Objective-C 建構的函式庫和程式碼加入了 FFI 的支援。使用這個 Dart 版本建構的應用，將會在桌面端、Web 端和移動端表現出更好的效能。因此我們強力建議您即刻執行命令 flutter upgrade 來升級到最新版。

## **釋出 Wonderous 應用**

我們與 gskinner 的設計團隊通力合作，釋出了一個名為 Wonderous 的應用，旨在向大家展示 Flutter 的強大功能——幫助您構建出高品質、精美的使用者體驗，而它本身就是一個非常精美的應用——從印度阿格拉市令人驚歎的泰姬陵到墨西哥尤卡坦半島上的瑪雅遺址，Wonderous 將世界上一些非常了不起的地方帶到您的手機上，使用影片和圖像來探索這些藝術、歷史和文化的交集。

![](https://devrel.andfun.cn/devrel/posts/2022/09/TDcEt8.jpg)

我們希望您與家人和朋友分享 Wonderous，更重要的是，它還作為一個開源專案供開發者們探索。作為一個真實上線營運的應用，它提供了一個完整且全面的範例，展示了我們希望為中高階開發者帶去靈感和創想的最佳實踐。未來的幾周時間裡，gskinner 團隊將會分享關於這個應用的更多技術細節文章，包括對無障礙的支援、動畫效果以及效能方面的技巧等內容。

## **引入新的圖形引擎: Impeller**

除了上面提到的 3.3 版本的改進內容之外，Flutter 團隊還在努力開發 **下一代的渲染層引擎: Impeller**。

Impeller 是對 Flutter Engine 核心部分的一次重大重寫，使用一個客製的執行時環境來取代 Skia 程式碼，並充分利用現代的硬體加速的圖形 API，如 iOS 上的 Metal 和 Android 上的 Vulkan。Impeller 提供了絲滑的動畫效果，並很大程度提升了各種多平臺 UI 工具套件的 "門檻"。這個效能上的差異是肉眼可見的，使用 Impeller 的應用可以保持 60Hz 或者更快的重新整理率的同時，能夠比以前更進一步地突破界限。最值得注意的是，Impeller 完全消除了對執行時著色器編譯的需要，而執行時著色器編譯是丟幀卡頓的一個常見來源。

雖然 Impeller 現有的功能還不夠完善，並且我們也還在最佳化它的效能，但我們現在正在一個 Google 級的產品應用上做內部測試。如果您在 App Store 下載剛剛提到的 Wonderous for iPhone，您就能提前感受 Impeller 在生產環境中的應用上執行的效果。

**我們正在 iOS 上為 Impeller 做一個早期採納者的預覽版本** ，除了在命令中加入一個啟用 Impeller 引數 (--enable-impeller) 之外，您無需對現有程式碼做任何改動就能啟用它。更多的關於 Impeller 架構以及如何啟用的文件您都可以在我們的 [wiki 頁面](https://github.com/flutter/flutter/wiki/Impeller "Impeller 概覽") 中找到。Impeller 正處於積極開發的狀態，如果您想參與作為早期採納者來使用的話，您需要切換到 Flutter 的 master 釋出渠道來確保使用了最新的程式碼。

我們期待著更多使用 Impeller 的應用出現，同時也非常歡迎使用了 Impeller 的開發者向我們提出當下版本的、可復現的、對應用當前版本效能影響或者保真度失真的 [報告](https://github.com/flutter/flutter/issues/new?assignees=&labels=created+via+performance+template&labels=impeller&template=4_performance_others.md&title=%5BImpeller%5D "提交效能影響或者保真度失真問題報告")。

## **祝福 Eric 的下一段旅程**

最後，我們想要以對 Flutter 的聯合創始人之一、Flutter 工程主管 Eric Seidel 的祝福作為結尾，他將於本月離開谷歌並開啟新的冒險旅程。2015 年的 Dart 開發者峰會上，Eric 首次向全世界介紹 Flutter，當時 Flutter 還木有名字和吉祥物，在 Flutter 的過去和現在的大部分時間裡，Eric 一直在帶領和管理 Flutter 的工程團隊，簡單說，沒有 Eric 就沒有 Flutter。

Eric 是一個天生的創業者，他的 "superpower" ("超能力"，superpower 也是 Eric 最喜歡用的詞語之一) 便是創造和發起新的構想和理念，因此，當 Eric 決定開啟下一段冒險旅程的時候，我們衷心為他祝福。

以 [Flutter 1.0 釋出](https://developers.google.cn/events/flutter-live "Flutter 1.0 釋出") Eric 在當時 Flutter Live 大會上對 Flutter 的願景作為結尾: Flutter 是一個長期主義的賭注，**希望從根本上做出改善並建構一個美好的使用者體驗**。這仍是我們的願景，因為這個結果還沒有最終實現。全球有數百萬開發者信賴 Flutter，Flutter 生態有成千上萬的貢獻者，Google 的 Flutter 團隊也正在蓬勃發展，我們希望您可以繼續加入我們的 Flutter 之旅，謝謝！
