---
title: 藉助 Flutter 順暢地開發多平臺應用
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/04/VdWBeY.png)

Flutter 已於近期釋出了 [Flutter 2](https://flutter.cn/posts/announcing-flutter-2)，Flutter 和 Dart 的產品總監 Tim Sneath 在 2021 年三月上旬舉辦的 [Flutter Engage](https://flutter.cn/posts/flutter-engage-event-recap) 活動中表示，Flutter 致力於成為多平臺 UI 工具套件，為了 "徹底改變開發者眼中的應用建構方式，讓他們從期望打造的體驗來入手進行開發，而無需優先考慮目標平台"。

在最新版釋出前，美觀、快速、開放且高效是 Flutter 的四大關鍵特性，為使用者建構跨平臺應用提供了極大便利。隨著 Flutter 2 的釋出，其又新增了一項關鍵特性: *可移植性*，對於 Flutter 來說，這可謂是一項重大的里程碑式進展，意味著 Flutter 現在可以利用單一程式碼庫，為移動端、web 端、桌面裝置和嵌入式裝置上的原生應用提供穩定支援。Flutter 是首款真正意義上專為環境計算世界而設計的介面平台。

在本文中，我們將探索 "可移植性" 這項關鍵特性對 Flutter 有何意義並討論其支援 web 應用和多屏裝置型別的功能，以及豐田如何將 Flutter 應用至其車輛的資訊娛樂系統。

## **移動應用向 web 應用的跨越**

Flutter 起初是一個用於建構精美 iOS 和 Android 應用的 UI 框架。然而，Flutter 一以貫之的願景是提供可移植的框架，幫助開發者為任意平臺上的使用者建構原生編譯應用。由於幾乎每台裝置都安裝有 web 瀏覽器，Flutter 也自然將下一個目標定為 web 端。

Flutter 2 可為 web 提供具有生產品質的支援，這意味著您可以利用單一程式碼庫，為 iOS、Android 和 web 瀏覽器建構精美的高效能應用。目前的重點是開發 web 應用，即可安裝的漸進式 web 應用 (PWA)，以及內容豐富的互動式單頁應用 (SPA)，而非現在所看到的靜態內容網站。這些可幫助 Flutter 開發者無縫銜接，從當前的移動應用擴充至 web 應用的全新使用者群體。

有了 Flutter 2，就如同為您的應用進行了一次免費升級。Web 只是另一個裝置目標而已，執行 Flutter Create，將自動產生一個帶有索引 HTML 檔案的 web 目錄，可以隨時在您喜愛的瀏覽器中執行。

與移動應用一樣，web 應用也有 2 個編譯器。*dev_compiler* 適用於開發階段，而 *dart2js* 則適用於部署階段。將開發和部署階段的編譯器分開，一方面可最佳化開發週期生產力，另一方面則可最佳化正式應用的效能，使其快速執行。Flutter 支援以原生機器程式碼編譯應用，這意味著您在釋出已完工的應用時，無需再在虛擬機器或 JavaScript 直譯器中完成這步操作。

![](https://devrel.andfun.cn/devrel/posts/2021/04/SjbcQq.png)

Flutter 的 *dev_compiler* 具有與 JIT 編譯相同的智慧功能。因此，該命令只會重新編譯受變動影響的程式碼，並堆積增量，從而確保開發迴圈快速進行。但是，與移動端支援熱重載 (記住應用的狀態) 不同，在 web 應用中，您需要手動啟動熱重載，並重建應用狀態。

就像與以 Flutter 建構的移動應用一樣，外掛的存在使得應用能夠與平台的原生庫通訊。許多 pub.dev 中現有的 Flutter 外掛都已支援 web 應用。在 web 端執行 Flutter 應用時，您可以透過這些外掛存取 JavaScript 庫。如需檢視某一外掛是否受支援，只需前往 pub.dev 並搜尋該外掛即可。外掛下方的標籤會顯示該外掛所支援的平台。

![](https://devrel.andfun.cn/devrel/posts/2021/04/4OS8ol.png)

> 資料來源: [pub.dev/packages?q=url_launcher](https://pub.dev/packages?q=url_launcher)

如果您希望更新某個外掛以使其適用於 web 應用，您可以參閱以下文章，瞭解如何為現有的外掛專案加入平台的支援:

[https://flutter.tw/development/packages-and-plugins/developing-packages#add-support-for-platforms-in-an-existing-plugin-project
](https://flutter.tw/development/packages-and-plugins/developing-packages#add-support-for-platforms-in-an-existing-plugin-project)

雖然您可以針對 web 應用使用與移動應用一致的程式碼，但您還需要針對使用者體驗進行最佳化。您可以使用約束條件新增動態佈局特性，以增強使用者體驗，例如，如果瀏覽器可以提供更多的螢幕空間，您可以將單列布局擴充至雙列。

類似的，您可能還會考慮新增 web 專用的導航功能，例如捲軸，以及滑鼠或鍵盤互動。為了在網頁中提供更好的瀏覽體驗，您還可以隱藏捲軸中的 ListView，並在將滑鼠懸於某個 widget 之上時，顯示滑鼠游標。此外，您還可以使用快捷鍵 widget 在您的應用中新增鍵盤快捷鍵。

如需瞭解詳情，您可以存取 [flutter.cn/web](https://flutter.cn/web)。

### **支援可摺疊裝置和雙屏裝置**

如前所述，Flutter 專為環境計算世界設計而成。現在的螢幕種類繁多，已不僅侷限於移動端、web 端和桌面端螢幕。從可穿戴式裝置到家用裝置、智慧家電，甚至再到可摺疊裝置和雙屏裝置，這些裝置已越來越多地出現在我們日常生活中。使用者可以使用這些裝置創作內容、玩遊戲、看影片、打字、閱讀或瀏覽網頁，既然這些裝置能夠滿足使用者的需求，那麼這些全新的裝置型別就有助於提高生產力。

同時，這些裝置型別意味著您將有機會探索全新的場景和使用者體驗。在兩個螢幕上執行應用，可帶來更多螢幕空間用於顯示內容和與使用者互動。當在兩個螢幕上適配 Flutter 應用時，您可以使用雙屏設計模式，例如列表詳情檢視、配套窗格，或採取其他用於調整應用 UI 的方法。

可摺疊的裝置型別也使得這些裝置中的應用可以和其他應用互相分享內容。例如，為您的應用新增拖放功能後，您可以在並排執行的應用間互相移動內容。

在 [Flutter Engage](https://flutter.cn/posts/flutter-engage-event-recap) 活動中，[Microsoft 宣佈](https://www.bilibili.com/video/bv1g64y117Jo) 正在與 Google 合作，使 Flutter 支援可摺疊裝置。Microsoft 將提供程式碼，使 Flutter 應用把握這些新機會，在 Surface Duo 裝置和三星等製造商生產的裝置上大展拳腳。

有了 [Flutter 2](https://flutter.cn/posts/whats-new-in-flutter-2-0)，所有 Flutter widgets 均將支援可摺疊裝置。例如，在您使用對話方塊時，應用能感知到其位於摺疊裝置上，將內容顯示在右邊或左邊的窗格中。

![](https://devrel.andfun.cn/devrel/posts/2021/04/7msNxm.png)

您也可以使用全新的雙窗格 widget 來放置資源。藉助此 widget，您可以在左側或右側窗格中放置資源。同時，該 widget 也能正確顯示於單螢幕手機或平板類裝置上。雙窗格 widget 可輕鬆支援全新的裝置型別。

## **豐田藉助 Flutter 打造的車載使用者體驗**

[Flutter 2](https://flutter.cn/posts/whats-new-in-flutter-2-0) 可以幫助開發者構建出適用於移動端、web 端、桌面端，甚至是新興裝置型別的精美應用，但這只是 Flutter 靈活性的冰山一角。想要成為真正的可移植性平台，支援釋出應用至客戶所在的任意平台，Flutter 還需要為嵌入式裝置提供支援。豐田已於近日宣佈，其車輛的資訊娛樂系統未來將由 Flutter 提供動力支援，屆時，Flutter 將為全球最大的汽車製造商之一帶來最佳的數字化體驗。

來自豐田北美汽車公司 (Toyota Motor North America) 總工程師 Daniel Hall 在 [Flutter Engage 活動](https://flutter.cn/posts/flutter-engage-event-recap) 中介紹了此次合作以及 [選擇 Flutter 的原因](https://www.bilibili.com/video/bv1g64y117Jo):

* 豐田的客戶期望享受到高效能車載使用者體驗，使之與豐田汽車的整體品質相符。Flutter 的渲染引擎可在受限的環境下提供優異效能，且其自帶的 AOT 編譯功能可為豐田提供其在車載技術中所追求的一致性特色。
* 在 Flutter 的助力下，豐田提供的車載使用者體驗可以與客戶期待在智慧手機中獲得的體驗相媲美。豐田相信 Flutter 的跨平臺機制所包含的觸控機制能使其適應任何執行環境。這種跨平臺機制可幫助豐田規避許多嵌入式系統都會遇到的問題，如效能遲滯，使用者體驗差等。
* 豐田也被 Flutter 的開發者體驗所吸引。雖然豐田只是在針對單一目標平台釋出其應用，但該應用可支援在桌面端進行熱重載並向 iOS 和 Android 系統的平板電腦傳輸內容，這些功能已證明其有助於提升實體和數字使用者體驗。隨著這種更快的迭代週期的出現，豐田可以更早、更頻繁地收集和整合客戶的反饋，有助於創造最佳使用者體驗。

藉助 Flutter 的嵌入器 API，豐田在其由 Linux 驅動的汽車級資訊娛樂系統中發揮了這項技術的優勢。Flutter 引擎架構透過交叉編譯引擎並將其封裝於嵌入器內的方式，使自身得以輕鬆地嵌入目標環境。嵌入器 API 易於使用，可以幫助豐田將 Flutter 應用與車載系統融為一體。

藉助 Dart 的語言設計和 Flutter SDK 的軟體設計，豐田已開發出諸多內部工具，並透過在豐田的設計流程中運用這些工具，使其得以提高 Flutter 的開發者人機工程學。豐田的目標是打造如下的工作流: 利用設計工具產生程式碼並執行，然後立即驗證軟體。例如，Flutter 會將宣告式 UI 和程式碼用作配置，在這種方法的助力下，豐田能夠高效地執行生成於設計過程中的程式碼，而無需經歷複雜且混亂的中間環節。

在開發這些豐田專用工具時，Flutter 的開源原則和規模日益壯大的開發者社群對豐田的成功起到了至關重要的作用。豐田相信，若沒有這種龐大的開放性生態系統的支援，便無法將 Flutter 擴充至自己的車載使用案例中。

豐田將與開源軟體方的合作視為對其車載使用者體驗的一次積極投資，並期待自身可以在開源的 Flutter 社群中有所作為。

## **Flutter: 為所有平台建構精美應用**

Flutter 可支援汽車、web 瀏覽器、膝上型電腦、手機、桌面裝置、平板電腦和智慧家居裝置，可謂是真正意義上的可移植性 UI 工具套件，其內建成熟的 SDK，可以讓您隨時隨地滿足使用者需求。Flutter 現可在您的重點目標平臺上執行，並可支援與您鍾愛的 Google SDK 和服務結合使用。Flutter 集精美而迅捷的使用者體驗與高效的開發環境於一身，使其可以探索內容並進行迭代，Google 提供的所有開原始碼加之遍佈世界的眾多開發者社群，為 Flutter 在過去幾年的指數級發展作出了貢獻。沒有其他平台可與之媲美。
