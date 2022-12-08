---
title: Flutter 1.5 釋出：適用於移動、Web、嵌入式和桌面平台的行動式介面框架
toc: true
---

**作者: Flutter 團隊**

I/O 期間我們迎來 [Flutter 框架](https://flutter.dev/) 的一個重要里程碑，因為我們的開發重點從行動平台擴充到了更廣泛的裝置和機型。在 I/O 大會上，我們釋出了 [Web 版 Flutter 的首個技術預覽版](https://flutter.dev/web)，宣佈 Flutter 將為包括 Google Home Hub 在內的 Google Smart Display 平台提供技術支援，並邁出利用 Chrome 作業系統支援桌面級應用的第一步。

## 從移動裝置到多平臺

長期以來，Flutter 團隊的使命一直是為開發 iOS 和 Android 版移動應用建構最佳框架。我們認為對行動開發作出改進的時機已經成熟，因為現在開發者不得不選擇在兩個平臺上建構相同的應用兩次，或者作出某些妥協以使用跨平臺框架。Flutter 提供了一種最有效的方式，使單個程式碼庫能夠為兩個平台提供美觀、快速和量身客製的體驗，並提高開發者的工作效率。我們很高興能夠看到早期的努力成功催生出目前最熱門的[開源專案](https://github.com/flutter/flutter)之一。

從去年開始著力開發 1.0 版本時，我們就開始嘗試將 Flutter 的範圍擴充到其他平台。這是基於兩方面考慮: 一是 Google 內部團隊越來越依賴於 Flutter，二是 [Dart 平台](https://dart.dev)有提供便捷式體驗的潛力。特別是，已經著手為 Dart 建構 Web 框架以供內部使用的小型團隊啟動了一個探索性專案 (代號為 "Hummingbird")，以評估移植 Flutter 引擎以支援基於標準的 Web 有何技術優勢。

該專案的成效令人驚歎，這在很大程度上要歸功於 Chrome、Firefox 和 Safari 等網路瀏覽器的快速發展。這些瀏覽器廣泛地提供了硬體加速的圖形、動畫和文字，以及較快的 JavaScript 執行速度。在專案剛開始的幾個月內，我們就成功建構了 Flutter 的核心框架原語。不久之後，我們在移動和桌面瀏覽器上運行了示範版本。長期以來，Dart 語言經常用於編譯網頁內容，這證明我們也能在 Web 端執行 Flutter 框架和應用。

與此同時，Flutter 核心專案不斷取得進展，進而推動桌面級應用的發展，其中包括鍵盤和滑鼠等輸入工具、視窗大小調整，以及適用於 Chrome 作業系統應用開發的除錯工具。針對在 Windows、Mac 和 Linux 上執行的桌面級應用，我們嵌入了 Flutter，而這項探索性工作已逐步演變成 Flutter 的核心引擎。

## 適用於所有螢幕的行動式介面框架

![Flutter Mobile, Web, Desktop, and Embedded](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot5-release/flutter-platforms.png){:width="85%"}

毋庸置疑，高效能的行動式介面框架具有巨大的商業潛力。該框架可以透過單個程式碼庫來為各種裝置提供量身客製的出色體驗。

對於創業公司來說，這讓他們能夠透過同一個應用在移動端、Web 端或桌面端接觸使用者。如此一來，他們從一開始就能全面覆蓋所有使用者，而不會受到技術上的限制。對於大型組織來說尤其如此，因為他們能夠使用同一個程式碼庫為所有使用者提供相同的體驗，而這會降低工作複雜度和開發成本，更加專注於提升相關體驗的品質。

實現對移動、桌面和網路應用的支援後，我們肩負更大的使命: **我們要建構最佳框架，以便為所有螢幕開發出色體驗。**

## 適用於 Web 平台的 Flutter

我們即將釋出 Web 版 Flutter 的**首個技術預覽版**。雖然這項技術還在開發中，但我們準備邀請嚐鮮者來試用並提供反饋。對於 Web 版 Flutter，我們的最初設想並不是將其用作文件體驗 (針對其最佳化 HTML ) 的通用替代品；相反，我們打算透過這種有效方式建構高度互動和圖形豐富的內容，從而切實感受到成熟介面框架所帶來的益處。

為了展示 Web 版 Flutter，我們與《紐約時報》合作建構了一個示範版本。《紐約時報》不僅是世界一流的新聞媒體，而且以設計縱橫字謎等益智遊戲而聞名。由於狂熱的解謎玩家希望能在當時使用的任何裝置上玩遊戲，所以《紐約時報》的開發團隊把目光轉向 Flutter，將其作為滿足讀者需求的潛在解決方案。發現能夠利用同一組程式碼存取網頁給他們帶來了巨大裨益。在 Google I/O 大會上，您可以率先了解他們最近更新的 [KENKEN 解謎遊戲](https://www.nytimes.com/games/prototype/kenken)。該遊戲利用同一組程式碼在 Android、iOS、Web、Mac 和 Chrome 作業系統上執行。

![ken-gratulations puzzle](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot5-release/nyt-game.gif){:width="85%"}

以下是《紐約時報》解謎遊戲執行總監 Eric von Coelln 對 Flutter 使用體驗的看法:

> “《紐約時報》縱橫字謎遊戲的單獨訂閱數量已超過 40 萬份，玩這款遊戲已經成為解謎者每天必做的事情。除了縱橫字謎遊戲，我們還開發了數字解謎遊戲，每個月都吸引了超過 200 萬名解謎者。 
> 我們已經開始探索 Flutter，並將其作為快速開發有趣和優質的移動體驗這一挑戰的潛在解決方案。現在，我們能夠在 Web 端釋出遊戲，這使 Flutter 成為在所有使用者平台快速部署內容的更具吸引力的選擇。之前基於 Flash 的 KenKen 遊戲經過更新，能夠提供多平臺的暢玩體驗。今年我們很高興能為解謎者帶來全新體驗。"

由於篇幅有限，我們無法在此一一詳述 Web 版 Flutter。若有興趣，請前往 [Flutter 部落格](https://medium.com/flutter-io/bringing-flutter-to-the-web-904de05f0df0)，閱讀專門介紹 Web 版 Flutter 的文章。

鑑於目前處於早期開發階段，我們非常希望收到您的反饋，瞭解您希望如何使用 Web 版 Flutter。我們希望以效能為重中之重，快速開發程式碼，並與 Flutter 專案的其他部分協調程式碼庫。

## 適用於移動裝置的 Flutter

我們還會升級核心 Flutter 框架，並會在**穩定版 channel 立即提供 Flutter 1.5**。根據開發者的反饋，我們對 [Flutter 1.5](https://medium.com/flutter-io/announcing-flutter-1-5-6e5d7e35b75f) 進行了數百處更改，包括對全新應用商店 iOS SDK 要求、iOS 和材料微件的更新，新增對新裝置型別的引擎支援，以及對具有最新 [UI-as-code](https://medium.com/dartlang/making-dart-a-better-language-for-ui-f1ccaf9f546c) 語言特徵的 Dart 2.3 作出改進。 

隨著框架本身逐漸成熟，我們正在設法建構支援生態系統。Flutter 的架構模型一貫優先考慮小型核心框架，並輔以豐富的軟體包社群。在過去的幾個月，Google 為網頁檢視、Google 地圖和 Firebase ML Vision 提供了產品級品質的軟體套件。我們還將新增對 [應用內支付](https://pub.dev/packages/in_app_purchase) 的初步支援。得益於 2,000 多個適用於 Flutter 的開放原始碼軟體套件，大多數場景均有合適的選擇。 

在今年的 I/O 上，我們宣佈推出一個尤其令人振奮的專案，即 [ML Kit 自訂影象分類器](http://github.com/firebase/mlkit-custom-image-classifier)。該工具利用 Flutter 和 Firebase 建構，可為建立自訂影象分類模型提供基於應用的簡易工作流。您可以使用手機的攝影頭收集訓練資料、邀請他人為您的資料集貢獻素材、觸發模型訓練以及使用訓練過的模型，這些操作都可以在同一個應用中實現。

![Flutter ML Kit: create datasets, collaborate to collect data, train model, run inference](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot5-release/flutter-mlkit.png){:width="85%"}

Flutter 越來越受歡迎，使用人數也越來越多。[有需求的客戶](https://flutter.dev/showcase)不斷增加，其中包括 eBay、Sonos、Square、Capital One、Alibaba 和 Tencent。這些公司正在利用 Flutter 開發應用，並從中發現樂趣！以下是 eBay 的高階開發者 Larry McKenzie 對 Flutter 的看法:

> **“Flutter 執行速度很快！過去需要很多天才能實現的功能，現在只需一天就能完成。過去我們花費很多時間處理的問題，如今再也不會發生了。我們的團隊現在可以專注於建立更出色的使用者體驗和提供相關功能。Flutter 讓我們能夠超越期望！”**

從更廣泛的角度上看，LinkedIn 最近進行的一項研究顯示，根據網站成員在過去一年中所新增的個人資料，Flutter 是軟體工程師中[增長最快的一項技能](https://learning.linkedin.com/blog/tech-tips/the-fastest-growing-skills-among-software-engineers--and-how-to-)。在最近的 2019 年 StackOverflow 開發者調查問卷中，Flutter 被列為 [最受歡迎的開發者框架之一](https://insights.stackoverflow.com/survey/2019#technology-_-most-loved-dreaded-and-wanted-other-frameworks-libraries-and-tools)。

## 適用於桌面平台的 Flutter

Flutter 目前也被用於桌面平台。在過去幾個月，我們一直在研究桌面平台這一 [實驗性專案](https://github.com/google/flutter-desktop-embedding)。但現在該專案逐漸演變成 Flutter 引擎，並將這項工作直接整合到 mainline repo 中。儘管這些目標尚未在生產環境中部署，但我們已釋出早期說明，以便開發 [在 Mac、Windows 和 Linux 上執行的 Flutter 應用](https://github.com/flutter/flutter/wiki/Desktop-shells)。

另一個快速發展的 Flutter 平台是 Chrome 作業系統，每年售出的 Chromebook 多達數百萬台，尤其是在教育領域。無論是執行 Flutter 應用，還是作為開發者平台，Chrome 作業系統都為 Flutter 提供了絕佳環境，因為該系統支援執行 Android 和 Linux 應用。藉助 Chrome 作業系統，您可以使用 Visual Studio Code 或 Android Studio 來開發 Flutter 應用，並在沒有模擬器的情況下使用同一台裝置本機測試和執行應用。您還可以在 Play Store 釋出適用於 Chrome 作業系統的 Flutter 應用，讓數百萬使用者因您的創作而受益。

## 適用於嵌入式裝置的 Flutter

舉例說明 Flutter 便攜性的最後，我們將介紹可嵌入其他裝置的 Flutter。最近我們釋出了一些範例，示範了直接在 Raspberry Pi 等小型裝置上執行 Flutter 的情況。我們還為 Flutter 開發了一個嵌入式 API，以便將其用於家庭和汽車等場景。

Smart Display 作業系統或許是 Flutter 目前已執行的最常見嵌入式平台之一，其為類似於 Google Home Hub 的裝置提供技術支援。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot5-release/flutter-google-home-hub.png){:width="85%"}

目前在 Google 中，Smart Display 平台的部分 Google 自建功能由 Flutter 提供技術支援。Google 助理團隊很高興能夠在接下來的幾個月繼續擴充透過 Flutter 為 Smart Display 建構的各項功能；而今年的目標是利用 Flutter 來驅動整個系統介面。

## 其他資源

開發者經常詢問我們如何完成 Flutter 入門。現在我們很高興地宣佈推出全新的綜合性 Flutter 培訓課程。該課程由 Udemy 上評分最高的 iOS 培訓課程的製作者 The App Brewery 建構。他們的最新課程涵蓋 30 多個小時的 Flutter 內容，其中包括影片、示範和實驗。在 Google 的贊助下，The App Brewery 宣佈推出此課程的限時折扣，原來的零售價為 199 美元，現只需 10 美元。

許多開發者正在利用 Flutter 開發振奮人心的應用。在 Google I/O 大會的籌備階段，我們舉辦了名為 Flutter Create 的挑戰賽，鼓勵開發者使用不超過 5KB 的 Dart 程式碼透過 Flutter 建構內容。我們收到來自世界各地的 750 多個獨特參賽作品，其中一些作品讓我們大開眼界，誰能想到如此少的程式碼竟然能創造出如此精彩的作品。

我們在此宣佈獲勝者，您可前往 flutter.dev/create 檢視獲勝名單。祝賀總冠軍 Zebiao Hu，其將榮獲價值超過 1 萬美元的全載入式 iMac Pro！

<iframe src="//player.bilibili.com/player.html?aid=52416421&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>

Flutter 不再只是一個移動框架，更是一個多平臺框架，可幫助您觸及任何地方的使用者。我們迫不及待地看到您利用 Flutter 在 Web、桌面、移動及其他平臺上建構的內容！
