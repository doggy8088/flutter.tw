---
title: Flutter Windows 桌面端支援進入穩定版
toc: true
---

Flutter 建立伊始，我們就致力於打造一個能夠建構精美的、可高度客製的、並且可以編譯為機器碼的跨平臺應用解決方案，以充分發揮裝置底層硬體的全部圖形渲染能力。今天，Flutter 對 Windows 生產版本的正式支援是對這一願景實現的重要標誌。它使 Windows 開發者也能享受到行動開發的相同生產力和功能。

![用 Flutter 建構 Windows 桌面應用程式](https://devrel.andfun.cn/devrel/posts/2022/02/908c66b003640.jpg)

Flutter 的目標是在任何平臺上為你提供出色的建構體驗，並且我們想要打造的是：只需要使用同一套核心框架和工具完成這個目標。透過 Flutter，你可以自由打造 **優美** 的使用體驗，使你的品牌和設計脫穎而出；它還擁有 **極高** 的執行速度，因為它會被直接編譯為機器碼；而透過支援有狀態的熱重載功能以提供互動式的體驗，讓你可以在應用執行時直接看到程式碼更改後的結果，從而獲得 **生產力** 提升。Flutter 是開放的，成千上萬的貢獻者參與到核心框架的建構，或是透過 package 和外掛生態系統對其進行擴充。

<highlight>截止目前，已經有近 50 萬個應用程式使用 Flutter 進行建構</highlight>

迄今為止，我們看到這股趨勢已經超出了我們的預期。包括一些大公司例如 [Betterment](https://verygood.ventures/success-stories/betterment "Betterment 使用 Flutter 的案例")、寶馬以及位元組跳動等，還有 Google 內部三十多個團隊都使用了 Flutter。根據 Statista 以及 SlashData 的統計，Flutter 在 2021 年已經成為了最流行的跨端 UI 工具套件。

![](https://devrel.andfun.cn/devrel/posts/2022/02/531f089d347f9.png)

我們自己的資料也能支援這一點，在 2021 年四個季度的開發者調查中，有 92% 的 Flutter 開發者對我們提供的工具表示滿意。（對於另外 8% 的人，我們正在傾聽你的反饋，希望也能得到你的滿意）。

這其中共同的需求之一就是對 Windows 的支援，

> 今天，我們很高興的宣佈，我們此次釋出的 Flutter 穩定版中，已經全面支援建構 Windows 桌面端應用程式了！

## Windows 與 Flutter

幾年前，我們為 Flutter 制定了一個宏大的願景，即從 iOS 和 Android 的移動端應用上擴充到其他平台，其中就包括 Web 端和**桌面端**。

Flutter 的核心部分是跨平臺的：從可移植的硬體加速的 Skia 圖形渲染引擎，到 Flutter 的渲染系統的核心單元，例如動畫、主題、文字輸入以及國際化，Flutter 提供了上百個 widgets。

然而桌面端並不只是移動應用執行在一個更大的螢幕上這麼簡單，它們從設計上來說就很不一樣。從輸入裝置角度來看，桌面端有鍵盤和滑鼠，它們會在顯示器上執行多個可變大小的視窗。而對於輔助功能 (Accessibility)、輸入法、視覺樣式等關鍵內容都有不同的規則約束。並且它們還和底層作業系統中不同的 API 進行整合：桌面應用支援從系統的檔案選擇器到裝置硬體再到 Windows 登錄檔等資料儲存的所有內容。

所以當我們把 Flutter 帶到 Windows 上時，我們也需要為它進行客製。

正如我們對 Android 和 iOS 的支援那樣，對 Windows 的實現也包括了 Dart 框架以及 C++ 的引擎。Windows 與 Flutter 透過承載了 Flutter 引擎的 **嵌入層(Embedder)** 進行通訊，翻譯以及傳送 Windows 都是 **嵌入層** 的職責範圍。Flutter 與 Windows 共同將你的 UI 繪製到螢幕上，處理視窗大小調整和 DPI 更改等事件，並與已有的 Windows (如輸入法編輯器) 配合使用。

![Flutter 在 Windows 平台的架構](https://devrel.andfun.cn/devrel/posts/2022/02/3ccad9b79b6d5.jpg)

在 Windows 上，Flutter 使用了一套完全相同的 Dart 程式碼，但是能夠使用 Windows 的 API。

你的應用能夠使用 Flutter 框架的全部功能，在 Windows 上，它還可以直接透過 Dart 的 C 互操作層或使用用 C++ 編寫的平台外掛與 Win32、COM 和 Windows 執行時 API 進行通訊。我們還適配了許多常用外掛以包含對 Windows 的支援，包括`camera`，`file_picker`和`shared_preferences`。更重要的是，社群已經添加了各種其他 package 對 Windows 的支援，涵蓋了從 Windows 工作列整合到串列埠存取的全部內容。

![目前已經有上百個 package 為透過 Flutter 建構 Windows 應用程式提供了適配。](https://devrel.andfun.cn/devrel/posts/2022/02/8eff674e59263.jpg)

要完全為 Windows 的 UI 進行客製，你也可以使使用案例如像是 [fluent_ui](https://pub.flutter-io.cn/packages/fluent_ui "使用 fluent_ui package 實現具有 Microsoft Fluent 設計美學的設計") 以及 [flutter_acrylic](https://pub.flutter-io.cn/packages/flutter_acrylic "使用 flutter_acrylic package 實現具有 Microsoft Fluent 設計美學的設計") 這樣的 package 創造具有 Microsoft Fluent 設計美學的應用。而且使用 [msix](https://pub.dev/packages/msix "使用 msix 工具打包釋出 Windows 桌面應用程式") 工具能將你的應用包裝進安裝器，這樣就可以上傳到 Windows 上的 Microsoft Store。

總的來說，這促進了在 Window 平臺上建立應用程式。以極快的速度在 Windows 上執行的同時還能轉到其他桌面或者移動應用以及 web 平臺上執行。讓我們來看看到目前為止的一些早期範例：

![一些使用 Flutter 建構的 Windows 應用程式的早期社群範例，特別展示來自深圳團隊的一款應用。](https://devrel.andfun.cn/devrel/posts/2022/02/8f17446cb2052.png)

## Microsoft 與 Flutter

我們曾詢問過 Windows 團隊是否願意分享一些對 Flutter 支援的話。這是微軟公司 Windows 開發者平台副總裁 Kevin Gallo 的評論：

>"我們很高興看到 Flutter 增加了對建立 Windows 應用程式的支援。Windows 是一個開放的平台，我們歡迎所有開發人員。我們很高興看到 Flutter 開發人員將他們的體驗帶到 Windows 上，併發布到 Microsoft Store。對 Windows 的支援是 Flutter 社群的一大步，我們迫不及待地想看看你能為 Windows 帶來哪些令人驚歎的應用！

事實上，許多 Microsoft 的團隊也都為今天的釋出做出了很大貢獻。我們特別想要感謝 Fluent design 團隊對於支援 Windows 上 Flutter 應用的圖示做出的貢獻。他們高品質的 [fluentui_system_icons](https://pub.dev/packages/fluentui_system_icons "Fluent design 團隊開發的 fluentui_system_icons package") package 已經加入到 Flutter Favorite 專案中了。

同時，Microsoft 圍繞 Windows 可及性所做的投入也給我們留下了深刻的印象，非常感謝該團隊的幫助，以確保 Flutter 從第一天起就能夠為螢幕閱讀器提供支援。將無障礙功能視為額外的需求是錯誤的。正如 [Microsoft 包容性設計工具套件](https://www.microsoft.com/design/inclusive "Microsoft 包容性設計工具套件") 中的這張圖片所示，我們必須要關注提供永久、臨時或不同情境下需求的體驗。

![](https://devrel.andfun.cn/devrel/posts/2022/02/a6687ec76191d.jpg)

下面的影片示範了 Flutter 如何整合 Windows 講述人 (Windows Narrator) 功能。出於本影片的目的，我們特意模糊了螢幕，讓你瞭解此功能對需要它的使用者的價值。

插入影片

Windows 講述人是一個為 Windows 打造的螢幕閱讀器，它同樣能夠在 Flutter 應用中良好的執行。

## Windows 開發生態工具集

我們的開發工具合作伙伴們也開始為 Windows 桌面端應用程式開發增加支援，比如：

* [FlutterFlow](https://flutterflow.io/ "低程式碼、拖拽式產生 Flutter 應用的工具 FlutterFlow 官網") 是低程式碼、拖拽式產生 Flutter 應用的工具，FlutterFlow 今天正式宣佈支援了 Windows 平台，同時還宣佈了一些幫助 Flutter 開發者建立桌面應用的很多功能。
* Realm 是一個快速的本地資料儲存服務。[今天釋出的最新版本支援使用 Flutter 建構Windows 桌面端應用程式](https://www.mongodb.com/developer/article/introducing-realm-flutter-sdk "Realm 今天宣佈支援使用 Flutter 建構Windows 桌面端應用程式")，他們使用 Dart FFI 快速存取底層資料庫，並增加了他們對 iOS 和Android 等行動平台的現有支援。
* Nevercode 團隊更新了他們的 [Codemagic CI/CD tool](https://flutterci.com/ "Nevercode 開發的 Codemagic CI/CD 工具") 工具並開始支援 Windows 桌面端，你可以在雲端測試和建構 Windows 應用，並將其釋出到 Microsoft Store。
* [Syncfusion](https://www.syncfusion.com/flutter-widgets "Syncfusion 官網的 Flutter widget 介紹介面") 更新了工具套件以充分發揮在 Windows 平台的優勢。如果你在使用他們提供的服務，你會發現他們的資料視覺化元件，比如樹狀圖、線性規、火花圖表，日曆元件甚至是 PDF 和 Excel 產生元件都已經支援了 Flutter，Syncfusion Flutter widget 是用 Dart 原生建構的。
* 近期，[Rive](https://rive.app/ "建立互動式向量動畫的工具 Rive 官網") 宣佈推出其流行的圖形工具套件的 Windows 版本，它能夠讓設計人員和開發人員建立互動式向量動畫，這些動畫可以使用狀態機即時響應程式碼。即將推出的 Windows 版本的應用程式提供了驚人的效能和更低的記憶體佔用，並將很快將在 Microsoft Store 中提供下載。

![](https://devrel.andfun.cn/devrel/posts/2022/02/7d6b68e9aaec4.jpg)

看到圍繞著 Flutter 的建立的成熟生態，我們非常激動，在你開始使用 Flutter 建構 Windows 應用的時候，我們鼓勵你多嘗試一下生態裡的這些合作伙伴的服務和工具。

## Flutter 2.10 中的 Windows 平台支援

作為 Flutter 2.10 正式版的一部分，Flutter 已經可以為 Windows 平台建構應用程式提供穩定的、具備生產品質的支援，Flutter 2.10 還包含諸多其他新特性和效能改進以及錯誤修復，在今天的另一篇推文中已經詳細介紹。

在未來幾個月裡，我們會發布更多為 macOS 和 Linux 平台提供穩定版支援的訊息，讓你可以透過 Flutter 為更全的桌面平台、Web 平臺和移動端平台建構精美應用！

再次感謝大家對 Flutter 的支援，期待看到你為 Windows 平台建構的精彩應用程式！

![](https://devrel.andfun.cn/devrel/posts/2022/02/56b3f56c8b099.jpg)

- 如果你想將上面這個圖片當作桌面背景，請點選點選這個連結下載原圖: https://files.flutter-io.cn/images/branding/desktop/dash2022_4k.png

## 致謝

- 原文: Announcing Flutter for Windows
- 連結: https://medium.com/flutter/announcing-flutter-for-windows-6979d0d01fed
- 翻譯: Vadaski
- 審校: Luke
- 製圖: Lynn
