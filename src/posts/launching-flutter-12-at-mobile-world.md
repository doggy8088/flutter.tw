---
title: 首個穩定更新版 — Flutter 1.2 釋出
toc: true
---

*由 Flutter 團隊釋出*

2019 [世界行動通訊大會](https://www.mwcbarcelona.com/session/flutter-google-toolkit-for-building-mobile-experiences/) (MWC 大會) 於 2 月 27 日在巴塞羅那順利拉開帷幕。值此移動盛會，Flutter 團隊宣佈正式推出 Flutter 1.2。
其實，這個大會對 Flutter 有著特別的紀念意義，因為 Flutter 的首個 beta 測試版正是在去年的 MWC 大會上與大家見面的，自此以後，Flutter 的發展速度遠[超我們的想象](http://sotagtrends.com/?tags=[ionic-framework,react-native,flutter,xamarin]&relative=false)。
如今我們再次聚首 MWC 大會，釋出 Flutter 穩定版本的首個更新，以此慶祝 Flutter 誕生一週年。

![announced-12-at-mwc19](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot2-release/announced-12-at-mwc19.png){:width="85%"}

## Flutter 1.2

作為 Flutter 1.0 之後的首次更新， Flutter 1.2 圍繞以下點進行了重點最佳化與改進:

-   提升核心框架的穩定性、效能和品質
-   改進現有 widget 視覺效果和功能
-   為 Flutter 開發者提供全新的基於 Web 的除錯工具

自 Flutter 1.0 釋出已經過去幾個月了，我們在這段時間內集中精力改進了測試和程式碼基礎框架，解決了此前積壓的 pull requests，並全面提升了框架的品質與效能。
有興趣的開發者們可以前往 Flutter wiki 頁面，[檢視完整的 pull requests 列表](https://github.com/flutter/flutter/wiki/Release-Notes---Changes-in-1.2.0)。此外，我們還在這次更新中加強了對 Swahili 等新 UI 設計語言的支援。

我們將繼續改進 Material 和 Cupertino 系列的 widgets，為開發者提供更加靈活的 Material 設計體驗，並持續在 iOS 裝置上繼續交付完美的畫素保真度。為此，我們添加了對[浮動游標文字編輯](https://github.com/flutter/flutter/pull/25384)的支援，
並且對許多細節進行了進一步最佳化 (例如，我們更新了文字編輯游標在 iOS 裝置上的繪製方式，以便真實呈現動畫和繪圖順序)。
受 [Robert Penner 作品](http://robertpenner.com/easing/)的啟發，我們擴充了動畫緩動函式的支援範圍。此外，Flutter 1.2 還引入了全新的鍵盤事件和滑鼠懸停支援，以作好準備為桌面級作業系統提供深層支援。

與此同時，Flutter 外掛團隊也在積極展開針對 Flutter 1.2 釋出的相關最佳化工作，
主要負責實現 [應用內購買](https://github.com/flutter/plugins/tree/master/packages/in_app_purchase) 支援，以及修復[影片播放器 (video player)](https://pub.dartlang.org/packages/video_player)、[webview](https://pub.dartlang.org/packages/webview_flutter) 和 [地圖 (maps)](https://pub.dartlang.org/packages/google_maps_flutter) 中的一些錯誤。
另外，我們還合併了一個來自 [Intuit 工程師提交的 pull request](https://github.com/flutter/flutter/pull/24440)，在 Flutter 中添加了 [Android App Bundles](https://developer.android.com/guide/app-bundle/) 支援。
Android App Bundles 是一種新的封裝格式，它能有效減小應用的體積並啟動應用動態交付等新特性。

最後，Flutter 1.2 還包含了 Dart 2.2 SDK，此項更新為程式碼編譯帶來了顯著的效能提升，
並且為初始化集合提供了新語言支援。更多資訊，請閱讀[《Dart 2.2 釋出說明》](https://medium.com/dartlang/announcing-dart-2-2-faster-native-code-set-literal-support-7e2ab19cc86d)。

特別說明: 有些讀者或許會好奇為什麼這個版本的編號是 1.2，請允許我在這裡稍作解釋。
我們的目標是大概每個月向 "測試版” 渠道釋出 1.x 版本的 Flutter，
然後每季度向 “穩定版” 渠道釋出可在生產環境下使用的更新版本。
上個月釋出的 1.1 是測試版本，因此 1.2 是我們的首個穩定更新版本。

## 新的除錯工具

每位開發者都有著不同的技術背景，偏愛的程式設計工具和編輯器也不盡相同。
為此，Flutter 添加了多種工具支援，其中包括 Android Studio 和 Visual Studio Code 的 一級支援，以及支援命令列建構工具，這也就意味著開發者需要更加靈活的除錯和執行時期檢查工具。

所以我們在釋出 Flutter 1.2 的同時，還帶來了全新的[基於 Web 的除錯工具套件](https://flutter.github.io/devtools/)，目的是幫助您更好地分析與除錯應用效能。
這些工具支援與 Visual Studio Code 和 Android Studio 的擴充程式及載入項一同安裝，並且提供多種功能：

-   Widget 檢查器: 對 Flutter 用於渲染的樹狀分級結構實現視覺化和直觀的探索；
-   時間線檢視: 可幫助您逐幀診斷自己的應用，並識別可能造成應用動畫 “卡頓” 的渲染和計算問題；
-   原始碼級偵錯程式: 支援單步執行程式碼，設定斷點並檢查呼叫堆疊；
-   日誌記錄檢視: 顯示應用所記錄的活動以及網路、框架和垃圾回收等事件。

![flutter-devtools-preview](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot2-release/flutter-devtools-preview.png){:width="85%"}

為了給 Flutter 和 Dart 開發者創造更好的開發體驗，我們將進一步加大對基於 web 的除錯工具的投入。此外，隨著 web 整合技術的不斷髮展，我們還計劃將這些服務直接新增到 Visual Studio Code 等工具中。

## 下一步工作

釋出 Flutter 1.0 之後，除了日常開發工作之外，我們還規劃了 [Flutter 2019 產品路線圖](https://github.com/flutter/flutter/wiki/Roadmap)，從中您會發現我們未來仍很多工作要做。

2019 年的一個工作重點是將 Flutter 的應用範圍擴充到行動平台之外。我們在 Flutter Live 上啟動了 [Hummingbird 計劃](https://youtu.be/5SZZfpkVhwk?list=PLOU2XLYxmsILq4ysYNWXq5TOGLgYDJgVD&t=175)，加快推進 Flutter 在 Web 端的發展。我們會接下來的幾個月裡公佈該專案的初步技術成果，請大家拭目以待！另外，我們還計劃將 Flutter 引入到桌面開發中。因此，除了上述框架層面的開發工作之外，我們還會透過 [Flutter 跨平臺桌面應用計劃 (Flutter Desktop Embedding Project)](https://github.com/google/flutter-desktop-embedding) 幫助各位開發者在 Windows 和 Mac 等作業系統上封裝和部署應用。

## Flutter Create: 您能使用 5K 的 Dart 程式碼做些什麼？

[Flutter Create 挑戰賽]({{site.url}}/create)將從本週起開始接收報名，你敢來參加嗎？參賽者需要利用 Flutter 建構充滿創意和趣味的精美應用，並把這一切全部濃縮到 5K 的 Dart 程式碼裡。5K 並不多，按照普通 MP3 格式的標準來算，差不多相當於三分之一秒的音樂。但我們敢說，有了 Flutter 的幫助，即使是使用如此少量的程式碼，您也能製作出令人大開眼界的應用。

![flutter-create-contest](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot2-release/flutter-create-contest-heroimg.png){:width="85%"}

挑戰賽將於 4 月 7 日結束，因此您將有幾周的時間來構建出色應用。我們準備了一些很棒的獎品，其中包括一臺搭載 14 核處理器和 128GB 記憶體的[頂配版 iMac Pro 工作站](https://www.apple.com/imac-pro/specs/)，價值超過 10,000 美元！我們將在 [Google I/O 大會](https://events.google.com/io/)上宣佈獲勝者名單，並且還會在此期間開展多個 Flutter 演講、Codelab 課程和活動，敬請期待！

## 結語

Flutter 現已進入 Github Top 20 軟體庫，與此同時，Flutter 全球社群也在以驚人的速度蓬勃發展，為世界各地的開發者正帶去獨特的程式設計樂趣——[印度清奈的開發者聚會](https://twitter.com/Nikkitagandhi/status/1099745911985467392)，[尼日利亞哈科特港的報道](https://twitter.com/Zfinix1/status/1079892033060392962)，[丹麥哥本哈根的應用](https://twitter.com/koorankka/status/1098579826355642368)，以及[美國紐約的孵化工作室](https://www.hotreload.io/) —— 從中我們可以清楚地看到 Flutter 正在成為一種全球現象，而這一切都離不開您的貢獻！Flutter 作為行動開發領域一股不容小覷的新生力量，不僅為開發者贏得了[億萬使用者](https://play.google.com/store/apps/details?id=com.alibaba.intl.android.apps.poseidon)，還[幫助創業者把理念推向市場](https://play.google.com/store/apps/details?id=com.kissaan.gomitra)。我們非常高興看到您擁有如此多的創意，也希望能夠幫助您使用 Flutter 來呈現這些創意。

![flutter-deep-dive-srmu](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot2-release/flutter-deep-dive-srmu.jpg){:width="85%"}

*在印度 SRM 大學參加 Flutter 高階研討會的與會者*

我們最近還在 YouTube 網站上專門為 Flutter 開設了一個新頻道。歡迎前來 [flutter.dev/youtube](https://flutter.dev/youtube) 進行訂閱觀看！
這個頻道包含了大家非常喜愛的一些影片合集如 [Boring Flutter Development Show](https://www.youtube.com/playlist?list=PLjxrf2q8roU3ahJVrSgAnPjzkpGmL9Czl)、[Widget of the Week](https://www.youtube.com/playlist?list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG) 和 [Flutter in Focus](https://www.youtube.com/playlist?list=PLjxrf2q8roU2HdJQDjJzOeO6J3FoFLWr2)，
同時也歡迎前來學習 [Dream11 是如何使用 Flutter 的](https://youtu.be/lCeRZhoqEP8) ，以及 [其他的開發者故事](https://www.youtube.com/playlist?list=PLjxrf2q8roU33POuWi4bK0zvDpAHK6759)等。

![welcome-to-flutter-yt-channel](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot2-release/welcome-to-flutter-yt-channel.png){:width="85%"}
