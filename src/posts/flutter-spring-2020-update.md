---
title: 2020 春季速遞：Flutter 勢頭正盛
toc: true
---

文 / Patrick Sosinski & Tim Sneath

在過去的幾個月中，
Google Flutter 團隊的工程師、產品經理、UX 研究人員、技術作者和開發者關係工程師們
都遭遇了不少的挑戰。
和大家一樣，在這特殊時期，我們也都忙於應對不同以往的日常工作。
不過，Flutter 作為一個開源專案，我們能夠使用各種公開可用的工具繼續進行開發。
在如今的形勢下，我們不但需要適應新的工作環境所帶來的限制和挑戰，
也需要照顧身邊的家人。幸運的是，我們依然在不斷取得進展，
並推進這個春天積壓下來的工作，
更學會了如何身處辦公室之外繼續完善 Flutter！
衷心希望大家也能不斷進步，平安健康。

![flutter-developers](https://files.flutter-io.cn/posts/flutter-cn/2020/flutter-spring-2020-update/flutter-developers.png){:width="85%"}

## Flutter 勢頭正盛

我們繼續見證 Flutter 使用者的快速增長。自發布以來的 16 個月內，已有 **200 萬開發者使用 Flutter** 。即便是在如今這種前所未有的情況下，3 月份也迎來了 10% 的月度增長，現在每個月有近 50 萬開發者使用 Flutter。

這裡還想和大家分享幾個有趣的資料:

* 開發者使用的作業系統：60% 的開發者人使用 Windows 系統進行開發，27% 的人使用 macOS，13% 的人使用 Linux。
* 開發者所在的團隊：35% 的開發者人在初創公司工作，26% 的人是企業開發者，19% 的人是獨立開發者，7% 的人在設計機構工作。
* 開發者使用的 Flutter 版本：78% 的 Flutter 開發者使用穩定版本，11% 使用 beta 版，11% 使用 dev 或 master 版本。
* Flutter 使用者數量排名前五的地區是印度、中國、美國、歐盟和巴西。
* 在 Play Store 中釋出的 Flutter 應用約有 5 萬個，僅在最近一個月就有近 1 萬個應用上傳。
* Flutter 應用中使用最多的 framework package 是 `http`、`shared_preferences`、`intl`、`meta`、`path_provider` 和 `pedantic`。
* Flutter 應用中使用最多的第三方 package 是 `provider`、`rxdart`、`cached_network_image`、`sqflite`、`font_awesome_flutter` 和 `flutter_launcher_icons`。

## Flutter 在企業中的應用

Flutter 的企業使用者增長迅速。我們的研究結果持續表明，
跨平臺建構高度品牌個性化體驗的能力，是大公司選擇 Flutter 的關鍵因素。
最近的一個例子是 [Nubank](https://nubank.com.br/en/)，
它是亞洲以外最大的數字銀行，擁有超過 2,000 萬用戶。
在對各種應用開發選項進行了 
[詳細的調查和分析後](https://cdn.nubank.com.br/mobile/taskforce/nubank-mobile-architecture-task-force-mission-report.pdf)，
Nubank 選擇了 Flutter，並在此後將其前端開發團隊統一在同一個框架內，
這也使得他們能在 iOS 和 Android 上同時釋出新功能。

請觀看他們的 [開發者故事](https://www.bilibili.com/video/BV1zi4y1t77J/)，瞭解他們如何用 Flutter 的優勢改進團隊開發體驗以及打造跨平臺優質應用:

企業使用者普遍要求使用專業元件。我們正在與 [SyncFusion](https://www.syncfusion.com/) 合作，
他們的 Essential Studio 產品現在包含了一系列
[高品質的 Flutter 元件](https://www.syncfusion.com/flutter-widgets)，
包括圖表、PDF 編輯和條碼產生器等。
在 [2020.1 版本](https://www.businesswire.com/news/home/20200421005174/en/Syncfusion-Introduces-New-Flutter-Widgets-Web-Compatibility)
釋出後，他們的所有元件都支援 Web、iOS 和 Android 三個平台，
而且還提供了 [Web 版的示範](https://flutter.syncfusion.com/#/)。

## 版本釋出流程更新

最後，在展望下一個穩定版釋出的同時，我們想分享一些對版本釋出流程的改動，
我們認為這些改動將進一步提高發布的穩定性和可預測性。

現在的釋出流程設計比較簡潔，對維護的要求也比較低。
在我們團隊規模還不大，框架也比較新的時候，這個流程就相當適合 Flutter，
但以目前的規模而言，我們遇到了一些影響 Flutter 貢獻者和開發者的問題，包括:

* 不清楚釋出版本何時建構，因此也不清楚裡面包含哪些程式碼
* 缺乏對分支的測試，導致釋出的熱修復版本出現回退

從 4 月釋出的 Flutter 開始，我們將轉向包含 beta 版和穩定版的分支模式，
並提供穩定期。現在我們將在月初發布 beta 版分支，
並揀選 (cherry-pick)* 修正關鍵問題來 "穩定" 這個版本。
大約每一個季度，當前的 beta 版分支會被提升為穩定版。
如有必要，我們將繼續在這個版本上進行熱修復。
我們的基礎設施現在支援針對分支進行測試，這意味著我們可以驗證揀選修正的問題，
並根據嚴重程度接納一些修復請求。

** 揀選 (cherry-pick): 指從一組既定的物件中挑選出最合適/最佳的。*

我們還藉此機會同步了 Flutter 和 [Dart 的釋出流程和渠道](https://dart.tw.gh.miniasp.com/get-dart#about-release-channels-and-version-strings)。
因此，Dart 現在也增加了一個 beta 版渠道，
並將在未來與 Flutter 同步釋出 
(例如，Flutter 的 beta 版將包含 Dart 的 beta 版)。

如果您已經基於穩定渠道釋出了 Flutter 應用，
我們鼓勵您在 beta 版上測試您的應用，
並在發現問題時上報，以便改善穩定版本的品質。
您也可以按照 Flutter wiki 上新發布的 [Flutter 揀選流程](https://github.com/flutter/flutter/wiki/Flutter-Cherrypick-Process)，
將穩定渠道上的回退或阻塞 bug 的嚴重性進行升級。

我們認為這個新的流程既能強化使用者對釋出品質和可預測性的信心，又能更容易地將熱修復傳遞到穩定渠道。

### 版本命名變更

作為新的分支模式的一部分，我們對版本釋出的命名方式做了一些調整。
完整的技術細節可以在 Flutter wiki 上的 Flutter 建構釋出渠道頁面找到，
其要點如下:

**非穩定的釋出版本** 將在版本字串中用 ".pre" 表示 "預釋出" 。給定一個 "x.y.z-n.m.pr" 的版本字串，每次從 master 建構一個新的 dev 渠道版本，"n" 的數值遞增。

* 1.18.0-1.0.pre <- master 版本升至 1.18 後的第一個 dev 版本
* 1.18.0-2.0.pre <- 從最近的 master 版本構建出的下一個 dev 版本

**Beta 版** 將從上面的 dev 版本構建出來。當我們對其中一個版本進行揀選修正時，"m" 數值會遞增。例如，如果我們將 master 的第 15 個 dev 版本作為 1.18 beta 版，那麼版本編號看起來會是這樣:

* 1.18.0-15.0.pre <- 最初的 beta RC，和進入 dev 的版本相同
* 1.18.0-15.1.pre <- (目前) beta 分支的後續建構，包含揀選結果
* 1.18.0-15.2.pre <- 第二個後續建構

**穩定版** 的版本號為 X.Y.0。後續的熱修復版本，如有必要，將增加補丁號 (X.Y.1、X.Y.2 等)。

* 1.18.0-15.4.pre <- 分支中最後的 beta 版本
* 1.18.0 <- 穩定版本，和 1.18.0-15.4.pre 內容一致
* 1.18.1 <- 1.18.0 的熱修復版本

## 下一步

我們的 **下一個穩定版本** 將採用這個全新的版本模型。

我們之前提到過 [Codepen 對 Flutter 的支援](https://flutter.cn/posts/announcing-codepen-support-for-flutter)，以下是我們最喜歡的幾個作品及其作者:

* [Twitter clone](https://codepen.io/mkiisoft/pen/KKdgdad) (mkiisoft)
* [Generative abstract art](https://codepen.io/rx-labz/pen/WNQoNem) (rxlabz)
* [Chakra animation](https://codepen.io/tahatesser/pen/GRpqbRY) (tahatesser)
* [Rotating carousel](https://codepen.io/joshuadeguzman/pen/jObrzJB) (joshuadeguzman)
* [Nougat animation](https://codepen.io/phillywiggins/pen/gOaPNPY) (phillywiggins)
* [Double pendulum](https://codepen.io/abhilas-csc/pen/qBOZKPj) (abhilas-csc)
 
![codepen support flutter](https://files.flutter-io.cn/posts/flutter-cn/2020/flutter-spring-2020-update/codepen-plus-flutter.png){:width="85%"}

如果您正在尋找 Flutter 學習資源，我們現在在網上提供免費的 Flutter 入門課程。
這個由 Angela Yu 主講的長達 10 小時的課程內含課程和程式碼實驗室，
可以助您開啟 Flutter 之旅。

請您持續關注 Flutter 每週的推送更新。
值此非常時期，請繼續注意保持社交距離，祝大家身體健康！

原文: [Flutter Spring 2020 Update](https://medium.com/flutter/flutter-spring-2020-update-f723d898d7af) /
中文釋出：谷歌開發者公眾號