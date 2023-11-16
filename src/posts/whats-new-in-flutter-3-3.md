---
title: Flutter 3.3 更新詳解
toc: true
keywords: Flutter 3, Flutter 3 新特性
description: 文字處理、效能提升和其他改進和更新，為開發者們帶來一個更好用的 Flutter 3.3！
image:
    path: https://devrel.andfun.cn/devrel/posts/2022/08/6c1eaf86b5694.png
---

*作者 / Kevin Jamaul Chisholm, Technical Program Manager for Dart and Flutter at Google*

Flutter 3 是我們正式為全平台提供支援的一個重量級里程碑，距離它的釋出僅過去了三個月，今天讓我們有請 Flutter 3.3 正式版！近三個月我們並沒有放慢更新迭代的速度——自 Flutter 3 釋出以來，我們已經為 Flutter 合併了 5687 個拉取請求。

本次更新帶來了 Flutter Web 平台、桌面端平台、文字處理的效能和其他更新內容。

同時我們也會介紹 go\_router package、DevTools \(開發者工具\) 和 VS Code 擴充相關的更新內容。與我們一起閱讀詳細瞭解它們吧！

## 框架更新

### 全域選擇

到現在為止，Flutter 在 Web 上的文字選擇互動仍然沒有達到預期。與 Flutter 應用不同，原生的 Web 應用會將每個節點建構為樹形結構。在傳統的 Web 應用中你可以輕鬆用拖動手勢來選擇網頁上的節點，這在 Flutter Web 應用中無法輕鬆達成。

從今天起，一切都發生了變化。我們引入了 `SelectionArea` widget，它的子 widget 現已可以進行隨意選擇！

![](https://files.flutter-io.cn/posts/flutter-cn/2022/whats-new-in-flutter-3-3/image6.gif)

只需使用 `SelectionArea` 包裹住路由顯示的內容 \(例如 `Scaffold`\)，Flutter 會替你處理好一切，你便可以享受到這項強力的新特性。

想要更全面深入地瞭解這個絕妙的新功能，請存取 [SelectionArea API 頁面](https://api.flutter.dev/flutter/material/SelectionArea-class.html)。

### 觸控板操作

Flutter 3.3 優化了針對觸控板的支援。Flutter 不僅提供了更豐富且順滑的控制，同時也減少了幾種特定情況的誤觸。若你想了解誤觸的範例，你可以檢視 [Flutter 實用課程](https://flutter.tw/cookbook) 頁面。將頁面滾動到底部的 DartPad，並跟隨以下步驟進行操作：

1.  縮小視窗讓上半部分出現捲軸
2.  將指標懸停在上半部分
3.  使用觸控板進行滾動
4.  在 Flutter 3.3 以前，使用觸控板滾動會拖動元素，因為 Flutter 將模擬的手勢事件進行了下發
5.  從 Flutter 3.3 開始，使用觸控板滾動會正確地滾動列表，因為 Flutter 會傳遞「滾動」事件，卡片不會識別這些事件，而列表會進行對應的處理

想了解更多資訊，請存取 [Flutter 觸控板手勢](<https://files.flutter-io.cn/flutter-design-docs/Flutter_Trackpad_Gestures_(PUBLICLY%20SHARED).pdf) 的設計文件，並且檢視以下的拉取請求：

* PR 89944: [在框架中支援觸控板手勢](https://github.com/flutter/flutter/pull/89944)
* PR 31591: [iPad 上的觸控版手勢](https://github.com/flutter/engine/pull/31591)
* PR 34060: [ChromeOS/Android 觸控板手勢](https://github.com/flutter/engine/pull/34060)
* PR 31594: [Win32 的觸控板手勢](https://github.com/flutter/engine/pull/31594)
* PR 31592: [Linux 的觸控板手勢](https://github.com/flutter/engine/pull/31592)
* PR 31593: [Mac 上的觸控板手勢](https://github.com/flutter/engine/pull/31593)

### 隨手寫功能

感謝來自社群成員 [fbcouch](https://github.com/fbcouch) 的出彩貢獻。Flutter 現在支援在 iPadOS 上使用 Apple Pencil 進行 [隨手寫](http://support.apple.com/zh-cn/guide/ipad/ipad355ab2a7/ipados) 輸入。這項功能已預設在 `CupertinoTextField`、`TextField` 和 `EditableText` 上啟用。只需要將 Flutter 升級到 3.3 就可以為你的使用者帶來這項新功能。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/whats-new-in-flutter-3-3/image7.gif)

### 文字輸入

為了最佳化富文字編輯的支援，本次更新我們增加了從底層平台的 `TextInputPlugin` 接收更加精細化的更新的能力。以前 `TextInputClient` 只能傳遞新的編輯狀態，而不能細分新舊狀態之間的變化量，`TextEditingDelta` 和 `DeltaTextInputClient` 填充了這部分的資訊差。透過存取這些變化量，你可以為輸入區域建構自訂的樣式，這個區域會在你輸入時展開和收縮。想要了解更多資訊，你可以檢視 [富文字編輯器範例](https://flutter.github.io/samples/rich_text_editor.html)。

## Material Design 3 支援

Flutter 團隊持續地在整合更多 Material Design 3 的元件到 Flutter 中。本次更新包括了[IconButton](https://api.flutter.dev/flutter/material/IconButton-class.html) 的中等和擴大樣式。

想要追蹤 Material Design 3 的整合進度，你可以在 GitHub 上檢視 [將 Material 3 帶到 Flutter](https://github.com/flutter/flutter/issues/91605)。

### IconButton 範例

![](https://files.flutter-io.cn/posts/flutter-cn/2022/whats-new-in-flutter-3-3/image9.png)

### Chip 範例

![](https://files.flutter-io.cn/posts/flutter-cn/2022/whats-new-in-flutter-3-3/image5.png)

### 中型和大型 AppBar 範例

![](https://files.flutter-io.cn/posts/flutter-cn/2022/whats-new-in-flutter-3-3/image8.gif)

![](https://files.flutter-io.cn/posts/flutter-cn/2022/whats-new-in-flutter-3-3/image3.gif)

## 桌面端平台

### Windows

在先前建構 Windows 桌面應用時，應用的版本只能在檔案中進行設定。這樣的行為與其他平臺上設定版本的行為並不一致。

現在 Windows 桌面應用的版本可以透過 `pubspec.yaml` 和建構引數進行設定。它有助於當你的應用推送了更新時，在應用中為你的使用者提供應用更新功能。

想要了解更多關於設定 Windows 桌面應用版本的內容，請檢視 [文件](https://flutter.tw/deployment/windows%23updating-the-apps-version-number)。Flutter 3.3 前建立的專案需要手動進行調整才能使用這項功能。

## Packages 更新

### go\_router 釋出

當你的應用包含複雜的導航需求時，它可能會讓你暈頭轉向。為了擴充 Flutter 的導航 API，團隊釋出了新版本的 [go\_router package](https://pub.dev/packages/go_router)，讓你在所有平台的路由邏輯設計變得更加簡潔。

go\_router package 由 Flutter 團隊進行維護，透過宣告式和基於 URL 的 API 讓導航和 deep links 的處理變得更加輕鬆。最新的 5.0 版本讓應用可以透過非同步程式碼進行重新導向，其中還包含了一些 [破壞性改動](https://files.flutter-io.cn/flutter-design-docs/[Draft]Go_Router%205.0%20migration%20guide%20(PUBLICLY%20SHARED).docx)。

更多內容請檢視官方文件：[路由和導航](http://flutter.tw/development/ui/navigation)。

## VS Code 外掛增強

VS Code 的 Flutter 擴充也帶來了新增依賴的更新。你可以使用 `Dart: Add Dependency` 命令加上逗號一次性新增多個依賴。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/whats-new-in-flutter-3-3/image1.gif)

你可以檢視以下內容瞭解自上一個 Flutter 穩定版本釋出以來所有 VS Code 的 Flutter 外掛的更新：

* [VS Code extensions v3.46](https://groups.google.com/g/flutter-announce/c/u1iSDMtKMVg)
* [VS Code extensions v3.44](https://groups.google.com/g/flutter-announce/c/x4m9o93-Dng)
* [VS Code extensions v3.42](https://groups.google.com/g/flutter-announce/c/45Wsk5pISx4)

## Flutter 開發者工具更新

自上次 Flutter 釋出穩定版以來，DevTools 同樣也包含數次更新，包括資料表格展示的使用者體驗和效能的提升，還有在滾動事件的長列表時減少卡頓 \([#4175](https://github.com/flutter/devtools/pull/4175)。

以下是自 Flutter 3.0 以來 DevTools 各個版本更新的公告內容：

* [Flutter DevTools 2.16.0 發行註記](https://flutter.tw/development/tools/devtools/release-notes/release-notes-2.16.0)
* [Flutter DevTools 2.15.0 發行註記](https://flutter.tw/development/tools/devtools/release-notes/release-notes-2.15.0)
* [Flutter DevTools 2.14.0 發行註記](https://flutter.tw/development/tools/devtools/release-notes/release-notes-2.14.0)

## 效能改進

### Raster 快取改善

本次更新提升了載入資源圖片的效能，減少了圖片資料的複製和 Dart 垃圾回收 \(GC\) 的壓力。先前在載入資源圖片時，`ImageProvider` 需要複製多次壓縮的資料。首先，開啟圖片時資料會被複製至原生的堆記憶體並向 Dart 暴露出結構陣列。然後，資料會在結構陣列轉換至內建儲存的 `ui.ImmutableBuffer` 時被再次複製。

隨著 [新增的 ui.ImmutableBuffer.fromAsset 的引入](https://github.com/flutter/engine/pull/32999)。這個載入過程同時也會更加快速，因為它會繞過之前方法通道所需的額外排程的開銷。特別是在我們的基準測試中，圖片的載入速度提升為原先的 2 倍左右。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/whats-new-in-flutter-3-3/image2.png)

更多相關資訊，請檢視官方文件：[新增 ImageProvider.loadBuffer](https://flutter.tw/release/breaking-changes/image-provider-load-buffer)。

## 框架穩定性

### 禁用 iOS 記憶體指標壓縮

在 Flutter 2.10 穩定版的釋出中，我們為 iOS 啟用了 Dart 的記憶體指標壓縮最佳化。但是，Yeatse 在 GitHub 上[提醒我們這項最佳化中包含了我們並未預料到的後果](https://github.com/flutter/flutter/issues/105183)。Dart 透過為堆保持一個大的虛擬記憶體來實現指標壓縮。由於 iOS 上允許的總虛擬記憶體少於其他平台，因此其他例如 Flutter 外掛之類別的元件可持有的虛擬記憶體便減少了。

雖然禁用了指標壓縮會增加 Dart 物件消費的記憶體，但是它也恢復了 Flutter 應用可用的非 Dart 部分的記憶體，總體來說是更合適的方案。

應用可以增加最大虛擬記憶體的分配量，但這項操作僅在較新的 iOS 版本上可用，並不適用於其他 Flutter 支援的 iOS 裝置版本。當我們能夠在所有位置使用這項最佳化時，我們會重新進行評估。

## API 改進

### PlatformDispatcher.onError

在先前的版本中，你需要手動配置一個自訂的 Zone 來捕獲應用的所有例外和錯誤。然而，自訂的 Zone 並不適用於 Dart 核心函式庫中的一些最佳化，會減慢應用的啟動時間。在本次更新中，你可以透過設定 PlatformDispatcher.onError 回呼(Callback)來捕獲所有的錯誤和例外，代替自訂的 Zone。更多內容請檢視已經更新的官方文件：[在 Flutter 裡處理錯誤](https://flutter.tw/testing/errors)。

### FragmentProgram 更新

用 GLSL  編寫的並且在 `pubspec.yaml` 的 `shader:` 部分宣告的片段著色器 \(Fragment shader\) 現在會自動編譯成引擎可以正確識別的格式，並且自動繫結為應用的資源。有了這項改動，開發者無需再使用三方工具編譯著色器。在未來，引擎的 FragmentProgram API 可能只能接受來自 Flutter 的工具建構。目前我們還沒應用這項更改，但如 [FragmentProgram API 改進支援的設計文件](https://files.flutter-io.cn/flutter-design-docs/FragmentProgram_Support_Improvements%20(PUBLICLY_SHARED).pdf) 中所計劃的，有可能在未來實行。

想要了解更多內容，你可以檢視這個 [Flutter 著色器範例](https://github.com/zanderso/fragment_shader_example)。

### 佈局小數處理

在先前的版本中，Flutter 引擎會將合成層精準地對齊畫素，用於提升 Flutter 在舊款 iPhone \(32 位\) 上的渲染效能。而在我們新增桌面平台的支援後，我們注意到這項操作會導致肉眼可見的抖動，因為桌面平台的是裝置畫素比通常會更低。例如在較低的 DPR 裝置上，提示會在漸入時產生的明顯抖動。在確定更新的 iPhone 裝置並不需要這項最佳化後，我們已[從 Flutter 引擎中將其移除](https://github.com/flutter/flutter/issues/103909)，來改善桌面端的渲染保真度。

此外我們還發現，將這些畫素對齊移除後，先前在黃金鏡像測試 \(golden image test\) 時候出現的細微渲染差異也變得更穩定了。

### 停止支援 32 位 iOS

在我們釋出 Flutter 3.0 時曾經提到，由於使用量的減少，3.0 版本是最後一個支援 32 位 iOS 裝置以及 iOS 9 和 10 的版本。這個改動將會影響  iPhone 4S、iPhone 5、 iPhone 5C 以及第 2、3、4 代 iPad 裝置。Flutter 3.3 穩定版以及之後的穩定版將不再支援 32 位 iOS 裝置以及 iOS 9 \& 10。這意味著使用 Flutter 3.3 及之後建構的應用將不能再上述裝置上執行。

### macOS 10.11 和 10.12 的支援進入尾聲

在即將到來的 2022 第四季度的正式版發行計劃中，我們將放棄對 macOS 版本 10.11 和 10.12 的支援。這意味著在此之後的 Flutter SDK 穩定版將不能在這些版本上執行，Flutter 最低支援的 macOS 版將上升為 10.13 High Sierra。

### 停止支援 Bitcode

[即將釋出的 Xcode 14 將不再支援提交含有 Bitcode 的 iOS 應用](https://developer.apple.com/documentation/xcode-release-notes/xcode-14-release-notes)，這個版本的 Xcode 會對開啟了 bitcode 的專案發出警告。因此 Flutter 將會在未來的穩定發行版中移除對 bitcode 的支援。我們不希望影響到很多的開發者，因此預設情況下，Flutter 將不會開啟 bitcode。然而，如果你手動在 Xcode 專案中開啟了 bitcode，請儘快在升級到 Xcode 14 之後關閉它。

你可以開啟 `ios/Runner.xcworkspace` 並在 build setting 中將 Enable Bitcode 設定為 No 以關閉它。混合開發應用可以在宿主工程的 Xcode 專案中關閉它。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/whats-new-in-flutter-3-3/image4.png)

你可以查閱 [Apple 文件](https://help.apple.com/xcode/mac/11.0/index.html?localePath%3Den.lproj%23/devde46df08a) 瞭解更多關於 bitcode 分發的內容。

## 結語

Google Flutter 團隊非常感謝社群中的每一位成員們為了讓 Flutter 的體驗變得更好所付出的所有努力，我們期待在已完成的工作上繼續努力，並且我們始終重視我們最重要最寶貴的財富——社群中的每一位成員！

---

> **原文連結**: 
>
> https://medium.com/flutter/whats-new-in-flutter-3-3-893c7b9af1ff
>
> **本地化**: CFUG 團隊: @AlexV525、@chenglu、@Vadaski、@Nayuta403
>
> **中文連結**: https://flutter.cn/posts/whats-new-in-flutter-3-3
