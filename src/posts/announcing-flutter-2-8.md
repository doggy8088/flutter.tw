---
title: Flutter 2.8 正式釋出
toc: true
---

文/ Tim Sneath，Flutter & Dart 產品經理

Flutter 已經更新到 2.8 正式版，釋出了多項新特性和改進以不斷改善移動和 Web 端的開發體驗，同時也正在將桌面端的支援推向穩定版。

![](https://files.flutter-io.cn/posts/flutter-cn/2021/announcing-flutter-2-8/flutter-2-8-hero.png)

Flutter 為應用開發帶來了革新：只要一套程式碼庫，即可建構、測試和釋出適用於移動、Web、桌面和嵌入式平台的精美應用——開發者只需專注於他們希望建構的產品和服務，而無需首要考慮釋出到哪些平台；作為一個高效能、高生產力的開發框架，Flutter 也可以幫助開發者們縮短產品開發週期；一套程式碼庫，針對多個平台。

## 新特性和改進: 更快速、更高效

這次正式版主要關注的是 **行動平台效能**。理想情況下，出色的效能應該是「標配」，但在實踐中，所有大型或複雜業務的應用都需要針對硬體和系統 API 庫進行最佳化。這包括但不僅限於比如應用啟動，可能會受限於網路頻寬和其他程式碼初始化的消耗，記憶體消耗，可能會受制於部分記憶體有限的裝置，以及圖形渲染效能等。我們也一直在藉助內部的大型應用比如 Google Pay 對 Flutter 的使用來提高 Flutter 的效能表現，並提供更好的工具來除錯和分析應用效能。為你的工程升級到 Flutter 2.8 正式版，你的應用應該會有更少的記憶體佔用以及更快的啟動速度。

最新的更新也包含了更方便的 **應用接入後端服務** 的特性，比如使用 Firebase 和 Google Cloud。我們也為應用可以加入 Google Ads 提供了穩定版的支援，並對相機外掛和 Web 外掛內嵌提供了大量更新。與此同時釋出的還有 Dart 2.15 正式版，增加了對併發效能的重大改進，也添加了新的語言特性，比如建構函式拆分和列舉型別的增強，也進行了效能最佳化，使得執行時記憶體降低了 10%。

![現在你的應用可以透過使用一個簡單的 Flutter widget 來完成多平臺使用者登入功能了](https://files.flutter-io.cn/posts/flutter-cn/2021/announcing-flutter-2-8/sign-in-widget.png)

另一個重要話題和資源投入是「提升開發者效率」，得力於 Flutter 的有狀態熱重載 (stateful hot reload) 等特性，我們始終專注於為開發者建立一個緊湊的內部迴圈迭代流程，我們正在開始探索封裝出一些更進階的功能讓開發者們更快速和高效的使用，你也能在未來的版本里看到我們針對這個目標的改進。比如在此次正式版釋出裡，我們添加了一個使用 Firebase 處理身份驗證的 widget，使用的時候無須擔心任何特殊的使用案例，比如兩步驗證、密碼重置的操作，也不用操心使用 Google、Apple、Twitter 和 Facebook 帳號登入時的複雜情況。將這些特性和服務直接建構在 Flutter 的核心基礎中，將有可能為應用開發帶來革新，將高效開發和低程式碼方案相結合，應用在 Flutter 這個靈活和強大的 UI 框架上。

## 使用基於 Flutter 的遊戲開發框架 Flame

對於大部分開發者來說，Flutter 是一個應用框架。不過使用 Flutter 進行休閒小遊戲開發的生態也在不斷髮展，這些小遊戲藉助 Flutter 實現硬體圖形加速。

今天我們也同時慶祝 Flame 框架 (flame-engine.org) 的 1.0 版正式釋出，這是一個使用基於 Flutter 的模組化 2D 遊戲引擎，Flame 提供了快速建構遊戲所需要的全部內容，除了遊戲迴圈 (game loop) 之外，也提供了核心元素比如元件系統 (Flame 裡稱之為 FCS)、精靈動畫和圖像、碰撞檢測、世界相機、效果系統以及手勢和輸入支援等。

Flame 是模組化的，它也可以使用其他庫或 package 進行擴充，比如使用 River 進行動畫效果處理、使用 audioplayers 這個 package 進行音樂播放和聲音特效，使用 Forge2D (一個類別似 Box2D 的物理引擎)、Tiled (瓦片地圖編輯器)、Fire Atlas (Spritesheet 和精靈動畫編輯器) 等。Flame 以及其廣泛的生態系統，共同為休閒或者 2D 遊戲提供了一套強大的服務。


![三款使用 Flame 建構的遊戲範例：Tomb Toad、Gravity Runner 和 Bonfire](https://files.flutter-io.cn/posts/flutter-cn/2021/announcing-flutter-2-8/flame-game-demo.png)

Flame 由 [Blue Fire 團隊](https://dev.to/blue-fire/fireslime-is-now-blue-fire-405g "Blue Fire 團隊") 創造，這是一個專注於為 Flutter 和 Dart 建構開源 package 和外掛的團隊。我們很高興能與他們一起合作，如果你對遊戲開發感興趣，我們鼓勵你去試試看 Flame。

## Flutter 的發展勢頭正旺

Flutter 的持續增長、發展勢頭以及工具和生態的繁榮的令人感嘆。今年的 I/O 大會上，我們注意到 Play 商店以及擁有超過 20 萬款應用使用了 Flutter，六個月後，這個數字幾近翻了一番，達到 37.5 萬+ 款！

![在所有螢幕上大放異彩，Flutter 支援 Android、iOS、iPadOS、Web、Windows、macOS 和 Linux](https://files.flutter-io.cn/posts/flutter-cn/2021/announcing-flutter-2-8/flutter-platform.png)


不僅在 Android 平台，據獨立移動分析公司 [AppAnnie](https://www.appannie.com/cn/ "AppAnnie") 的資訊，iOS 平台頭部品牌和大型應用諸如 [BMW](https://itunes.apple.com/app/id1519457734 "BMW")、[eBay](https://itunes.apple.com/app/id1456156090 "eBay")、[WeChat](https://apps.apple.com/us/app/wechat/id414478124 "WeChat")、[SHEIN](https://apps.apple.com/us/app/shein-online-fashion/id878577184 "SHEIN")、[Philips Hue](https://apps.apple.com/app/id1055281310 "Philips Hue")、 [Norton](https://apps.apple.com/app/id1278474169 "Norton")、[trip.com](https://apps.apple.com/app/id681752345 "trip.com") 和 [Greggs](https://apps.apple.com/gb/app/greggs/id1098233626 "Greggs") 裡也都使用了 Flutter。在 Web 平台，我們也透過一些類似 [FlutterFlow](https://flutterflow.io/ "FlutterFlow") 和 [Rive](https://rive.app/ "Rive") 等設計工具為應用帶去更好的體驗。桌面端，Ubuntu 的工程團隊也正繼續使用 Flutter 建構各種新的體驗，包括新的安裝程式和韌體更新程式。甚至包括 [絕地求生](https://apps.apple.com/us/app/pubg-mobile-arcane/id1330123889 "絕地求生") 這樣的大型遊戲，也稱 Flutter 能夠很好的適應各種 UI 螢幕。

生態系統的建構絕非一日之事，由各個機構和社群獨立調研得出：[Statista](https://www.statista.com/statistics/869224/worldwide-software-developer-working-hours/ "Statista")、[JetBrains](https://www.jetbrains.com/lp/devecosystem-2021/miscellaneous/#Technology_which-cross-platform-mobile-frameworks-do-you-use-two-years "JetBrains")、[SlashData](https://www.slashdata.co/reports/?category=mobile-desktop "SlashData") 和 [Stack Overflow](https://insights.stackoverflow.com/trends?tags=flutter%2Creact-native%2Ccordova%2Cxamarin "Stack Overflow")，Flutter 現在已經成為最受歡迎的多平臺工具套件，這同時離不開日益增長的 package 和外掛庫的生態以及各種工具集的支援。

## 回首和展望

這一年同樣艱難，而且我們的工程團隊也一直在忙碌。除了把 2.8 推入正式版本，我們還重寫了開發者工具，推出空安全和 Web 支援，完成了原生程式碼的 FFI 支援，加入了對 Material You 的支援，並努力提升效能和品質。我們將近解決和關閉了總共 2 萬個 issue，也 [更新了新版的 Flutter 網站](https://mp.weixin.qq.com/s/JOm2-TBh4m3nJZKWbfjoug)。過去的幾個月我們花費了大量的精力來整理我們的工程基礎建設，用以提高工程師的生產力以及擴大測試範圍等。

展望 2022，首先是希望能夠真正走出去與大家見面，我們也正在向核心開發者體驗方面投入更多，比如增強程式語言，文件更新以及抽象出更多高階功能，讓 Flutter 可以更易於建構複雜應用，我們還將把桌面端支援推進到穩定版本，並進一步增加 Web 端的特性。除此之外，我們還計劃與擴大其他平台的互操作性以適配更新的螢幕。我們一直在路上！

## 懷念和致敬

我們想把 Flutter 2.8 版本獻給社群的 Kevin Gray，他是一名來自 VGV 團隊的工程師，但是 [於一周之前不幸去世](https://verygood.ventures/blog/remembering-our-friend-and-teammate-kevin-gray "於一周之前不幸去世")。一開始 Kevin 就對 Flutter 的成功做了很多重要貢獻，他是很多早期 Flutter demo 背後的開發者，包括 Flutter 的第一個客戶 Hamilton 應用，用 Flutter 開發了第一個桌面示範 slides 的應用，開發了第一個在 Google I/O 主題演講中展示的 Flutter 應用。Kevin 是一位有才華、有愛心、風趣和善良的人，我們在公開紀念他，並讓所有人都知道他的影響，如果沒有他，Flutter 將不會是現在的這個樣子。我們想念你，謝謝你為 Flutter 做出的一切。

Kevin 一直在支援一項公益事業「國際計劃 (Plan International)」，CFUG 社群以 Kevin 的名義向這個專案捐助 $280，以感謝他對 Flutter 的支援和貢獻。





























