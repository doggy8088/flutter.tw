---
title: Flutter 2022 產品路線圖釋出
toc: true
---

為了提高產品透明度，每年年初 Flutter 團隊都會發布今年度的產品路線圖，以幫助使用 Flutter 的團隊和開發者們根據這些優先事項制定計劃。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/flutter-roadmap-2022/flutter-2022-roadmap.jpg)

2022 年 Flutter 團隊將重點透過關注以下幾個領域和方向針對產品進行研發和改進，包括開發者體驗、桌面端、Web 端、框架和引擎、Dart 程式語言、卡頓，並計劃於今年停止對 32 位 iOS 裝置的支援，並增加對軟體供應鏈安全方面的投入，以達到 SLSA 4 級 (使用者可以高度確信該軟體沒有被篡改)。

## 重點關注

### 開發者體驗

作為一款面向開發者的工具，我們最關注的就是開發者體驗。我們的目標是建立一款開發者們熱愛的 SDK，這將會在很多方面有所體現。包括建立實現通用業務場景的 widget、釐清現有的 API 並引入新的 API 以更便捷的方式實現常見的設計模式、改善錯誤資訊提示、改進開發者工具和 IDE 外掛、建立新的 Lint 規則、修復框架和引擎的 bug、改進 API 文件、建立更有用的範例程式碼，以及在 Web 上實現熱重載 (Hot Reload) 和改進 Dart-to-JS 場景的堆疊追蹤等。

### 桌面端

2022 年我們計劃將 Flutter 的桌面端支援推進到穩定版本。我們會把重點放在測試上，並在平台可用之後進行公佈——首先是 [Windows 平台](https://github.com/flutter/flutter/projects/209)，然後是 [Linux 平台](https://github.com/flutter/flutter/projects/216)，然後是 [macOS 平台](https://github.com/flutter/flutter/projects/215)。這項工作的重要部分是擴增迴歸測試套件，以讓我們有充分的信心將 Flutter 帶到桌面端平台而無需破壞現有的程式碼。

### Web 端

2022 年我們計劃提升 Flutter Web 的效能、外掛品質、無障礙特性和多瀏覽器一致性的體驗，與此同時，我們也在計劃讓 Flutter 應用更方便的嵌入其他頁面。

### 框架和引擎

為了提高 Android 平台的還原度，我們將 [更新 Material 庫以支援 Material Design 3](https://github.com/flutter/flutter/issues/91605)。也為了提高 Web 平台的還原度，我們計劃實現跨元件的文字選擇，這些更新都不侷限於某個平台。

我們計劃提升不同平臺上的文字編輯體驗，例如提高桌面端文字編輯協議的還原度，以及 iPadOS 上手勢識別的整合。

對於桌面和 Web 端，我們將提供選單 (包括上下文選單和選單欄) 的解決方案，包括與作業系統相關的整合 (特別是與 macOS 相關)。

最後，我們計劃嘗試支援基於單一 Isolate 渲染到多個視窗的特性，這個特性受到桌面端平台的啟發，但其應用可以不僅限於桌面端平台。

### Dart 程式語言

2022 年我們計劃放緩並以穩定的速度發展 Dart 程式語言特性，我們預計會給 Dart 程式語言引入一個新的特性，有可能是靜態超程式設計 (static metaprogramming)，我們將根據對這個特性對 Dart 程式語言的改善做出決定，也同時會對 Dart 程式語言進行改進，包括可能改進 package 匯入的語法等。

我們還計劃擴增 Dart 程式語言的編譯工具鏈以支援編譯為 Wasm，不過這可能會要看 WasmGC 規範的支援時間。

### 卡頓

[2021 年我們著手解決了很多關於卡頓的問題](https://files.flutter-io.cn/flutter-design-docs/Jank_in_Flutter.pdf)，但最後的結論是，我們可能需要完全重新思考該如何使用著色器 (shader)，正因如此，我們也一直在重寫圖形渲染的後端。2022 年，我們打算將 iOS 上的 Flutter 遷移到這個新的架構上，然後根據在這方面的經驗，將這個方案移植到其他平台。此外，我們還將實現其他效能方面的改進以及效能自省特性，例如 [新的 DisplayList 系統](https://github.com/flutter/flutter/issues/85737) 所實現的功能。

## 計劃棄用

我們計劃在 2022 年放棄對 32 位 iOS 裝置的支援，詳見 [RFC 文件](https://files.flutter-io.cn/flutter-design-docs/RFC_Move_32-bit_iOS_to_Best_Effort_Tier.pdf)。

## 基礎設施建設

2022 年我們將增加對供應鏈的安全的投入，目的是達到符合基礎設施 SLSA 4 級別中描述的要求。

>> 近年來，軟體供應鏈安全 (Supply Chain Security) 成為常被提及的話題，因為軟體開發的複雜性，在原始碼、建構、釋出等過程中都可能會存在很多威脅。一些不懷好意的人可能會透過釋出二次打包後的開發工具或一些命名相似的軟體套件，或在一些依賴的軟體套件中加入惡意程式碼等方式對應用的供應鏈安全產生威脅。
>> 
>> SLSA (Supply-chain Levels for Software Artifacts) 是一個針對軟體供應鏈的安全規範框架，目的是為了防止軟體被篡改、提高完整性，其內部版本由 Google 自 2013 年開始使用，v0.1 版於 2021 年 9 月中旬公開發布，由 Intel、Linux 基金會、VMWare、Google、CNCF 等多家機構組成的委員會共同領導，SLSA 1 為最基礎的要求等級，SLSA 4 為最高等級要求，瞭解更多請存取 slsa.dev 網站。

希望這份 2022 產品路線圖可以更好的幫助你建構 Flutter 應用，讓你的業務更上一層樓！

*Flutter 產品路線圖原文: https://github.com/flutter/flutter/wiki/Roadmap*
