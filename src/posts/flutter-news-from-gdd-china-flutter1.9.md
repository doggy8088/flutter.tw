---
title: Flutter 1.9 正式釋出
toc: true
---

 
![Google Developer Days taking place in China](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot9-release/flutter1-9-gdd-keynote.jpg){:width="95%"}

本週對 [Flutter](http://flutter.cn) 意義非凡。Google 面向中國開發者舉辦的重量級年度盛會——[中國 Google 開發者大會](https://events.google.cn/intl/en/developerdays2019/) 於今日正式拉開帷幕。在主題演講環節，Flutter 團隊宣佈推出最新穩定版: Flutter 1.9。這是 Flutter 迄今為止最大的一次版本更新，100 餘位貢獻者提交共計超過 1,500 份 pull request。Flutter 1.9 引入的新特性與更新涵蓋範圍廣泛，包括 macOS Catalina 和 iOS 13 支援、工具支援最佳化、多項 Dart 語言新特性以及全新的 Material widget。

團隊還在會上宣佈了另一個具有里程碑意義的重磅訊息: **Flutter web 支援現已成功合併到 Flutter 的主 repo**，自此以後，開發者只需使用同一套基準程式碼，便可為行動平台、桌面端和網頁端開發應用。此外，團隊也分享了來自全球網際網路公司 [騰訊](https://www.youtube.com/watch?v=DVGIBU109nI&feature=youtu.be) 的成功案例，讓現場觀眾體驗了一把 Flutter 的蓬勃活力，親眼見證越來越多的應用正在透過 Flutter 締造精彩。

接下來，讓我們一起回顧一下 Flutter 要聞。首先，給大家介紹一下 Flutter 1.9 帶來了哪些重要的更新內容。

## macOS Catalina 和 iOS 13 支援

蘋果將在近期內推出新版本的 macOS 作業系統 Catalina，為此，團隊付出了巨大努力，以確保 Flutter 做好升級準備，順利適配新平台。比如說，我們進一步優化了端到端的工具體驗，保證 Flutter 工具能夠與 Xcode 妥善協作，助力開發者面向 Catalina 開發出優質應用，具體最佳化項包括：為新的 Xcode 建構系統提供支援、全工具鏈啟用 64 位支援、簡化平台依賴項等。

此外，隨著 iOS 13 即將面世，團隊也在積極推進相關的支援工作，以確保您的 Flutter 應用在新款 iPhone 裝置上保持美觀的介面。Flutter 1.9 實現了 [iOS 13 的拖曳式工具欄功能](https://github.com/flutter/flutter/pull/35829)，允許長按與從右往左拖動兩項操作，並且為 [觸感反饋](https://github.com/flutter/flutter/pull/37724) 提供了支援。不少開發者已經提交了 [pull request](https://github.com/flutter/flutter/issues/35541)，希望 Flutter 支援 iOS 夜間模式，團隊目前已開始著手解決這方面的需求，爭取儘早推出解決方案。 

最後，最新版本的開發建構允許您啟用 [Bitcode 實驗性支援](https://github.com/flutter/flutter/wiki/Creating-an-iOS-Bitcode-enabled-app)。Bitcode 是蘋果新新增的一個編譯特性，開啟 Bitcode 功能後，開發者只需在編譯環節上傳與平台無關的 Intermediate Representation (中間檔案) 即可。以 Bitcode 的形式上傳應用後，蘋果可以在後期直接對二進位制檔案進行最佳化，免除了開發者二次上傳的麻煩，與此同時，這也為 Flutter 開啟了更多的使用場景，比如說為 watchOS 和 tvOS 等要求上傳 Bitcode 檔案的平台提供支援。

## 全新的 Material widget

Flutter 1.9 也對 [Material](https://material-io.cn/) 元件和特性進行了升級。作為一款全球頂尖的開源設計系統，Material 提供了豐富多彩、靈活易操作的視覺元素，助力開發者在多個平台實現高互動性的使用者體驗。

在 Flutter 1.9 中，我們新添加了若干 widget, 其中包括 ToggleButtons 和 ColorFiltered。

![Flutter ToggleButtons Demo](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot9-release/image1phone.gif){:width="45%"}

![Flutter ColorFilter Demo](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot9-release/image2phone.gif){:width="45%"}

ToggleButtons widget 可將同一行的多個 ToggleButton widget 組合到一起，其中每個 widget 各自又由一組圖示和文字 widget 構成。透過這種組合，開發者將得到一組外觀與行為完全可自訂的按鈕。它能為您的應用按鈕實現更加多元化的設計——不論是單選還是多選，選擇至少一個或是零個，尖角還是圓角、粗邊或細邊，圖示或文字——ToggleButtons widget 全都可以滿足。請檢視 [ToggleButtons 範例](https://github.com/csells/flutter_toggle_buttons)，瞭解以上需求的具體實現。

正如上文右圖所示，ColorFiltered widget 允許您更改子 widget 樹的顏色，這與利用演算法 (部分演算法見上圖範例) 給圖片重新上色差不多。該 widget 能夠幫您處理許多使用案例，例如: 向用戶提供更好的色彩無障礙服務等等。請檢視 [ColorFiltered 範例](https://github.com/csells/flutter_color_filter)，瞭解該 widget 的工作細節。

## 全球語言支援

我們還新增了南非語 (Afrikaans)、祖魯語 (Zulu) 等 24 種語言的支援。

![Table of languages supported](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot9-release/i18n.png){:width="95%"}

## Dart 2.5 釋出

如要保障流暢的端到端 Flutter 開發體驗，僅僅憑藉強勁的特性是不夠的，底層語言也至關重要。在 Flutter 1.9 釋出之際，我們也推出了最新版本的 Dart 語言——[Dart 2.5](https://medium.com/dartlang/dart-2-5-release-328822024970)，內含預釋出版本的 Dart: FFI ( 外部函式介面 )，它可用於實現 Dart 語言與 C 語言之間的互操作 (interop)，以及由機器學習驅動的 IDE/ 編輯器程式碼自動完成功能。更多技術細節，請閱讀 [Dart 2.5 釋出說明](https://medium.com/dartlang/dart-2-5-release-328822024970)（請關注微信公眾號後續推文）。

## 工具鏈最佳化

從 Flutter 1.9 開始，iOS 新專案預設使用 Swift 語言，而非 Objective-C；Android 新專案則預設使用 Kotlin，而非 Java。由於許多 [Flutter package](https://pub.dev/) 使用 Swift 編寫，因此，一旦將 Swift 設定為預設語言後，開發者便無需再為啟用預設設定的應用手動新增套件。Swift 5 實現了 ABI 穩定，而且 [蘋果在近期幾個系統版本中也為應用瘦身做了許多工作](https://developer.apple.com/documentation/xcode_release_notes/xcode_10_2_release_notes/swift_5_release_notes_for_xcode_10_2#3138038)，因此 12.2 或更高版本的 iOS 系統將不再包含用於 Swift 的動態連結庫，從而大幅縮小了 Swift 應用的體積。 

考慮到 Android Studio 新專案現在已經預設採用 Kotlin 作為開發語言了，因此，很自然地，我們就想著再往前邁一步，把所有 Android 專案的預設語言統一為 Kotlin。flutter CLI 工具、[IntelliJ/Android Studio](https://plugins.jetbrains.com/plugin/9212-flutter) 和 [VS Code](https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter) 外掛均預設啟用這些選項，不過，如有需要，您可隨時切換回之前的 Objective-C 或 Java 語言。

此外，我們也在一直改善 Flutter 應用中的錯誤資訊品質。最佳化之後，資訊的可讀性、簡潔性和可操作性均有明顯提升。

![Flutter error message](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot9-release/fluttererrormessage.png){:width="95%"}

該專案由 Flutter 使用者體驗團隊負責牽頭，如果您想了解更多有關結構化錯誤顯示的內容，請閱讀 [更精準更簡潔: Flutter 改進錯誤資訊提示](https://medium.com/flutter/improving-flutters-error-messages-e098513cecf9)。我們才剛剛開始採用這些新格式，預計未來將有更多錯誤資訊會以結構化的形式呈現。

## 在 web 平台執行 Flutter

最後，我們很高興地宣佈，flutter_web 這個 [repo](https://github.com/flutter/flutter_web) 已經完成了自己的使命，現在所有的 web 支援已經合併到 flutter 的 [主 repo](https://github.com/flutter/flutter)！這意味著，如果您透過 master 或 dev 渠道安裝最新版本的 Flutter 建構，您只需要執行 flutter run -d chrome 就可以使用最新的試驗版本 Flutter 來開發 web 應用。

在您建立專案時，Flutter 會透過一個最小的 web/index.html 檔案來產生一個 web 執行引擎 (web runner)，其中 web/index.html 檔案主要用於自舉 (bootstrap) 基於 web 編譯的 Flutter 程式碼，有了這檔案後，您可使用 Flutter CLI 工具或 IDE 中的 Flutter 外掛來編輯或運行針對 web 平臺開發的 Flutter 應用。

![screenshot of VS Code with web support enabled for Flutter](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot9-release/vscode.png){:width="95%"}

上圖為啟用了 Flutter web 支援的 VS Code 介面截圖。請注意 web/index.html 檔案和頂部的下拉列表允許您選擇 Chrome 作為目標裝置。儘管 Flutter 的 web 支援還不成熟，但是我們在 Flutter 1.9 中朝著正確的方向邁進了一大步。

我們在今年 7 月底啟動了一項 [早期體驗計劃](https://medium.com/flutter/flutter-for-web-early-adopter-program-now-open-9f1fb146e4c4)，其主要目的是加快 web 版 Flutter 應用的落地速度。獲選開發者能得到必要協助，在接下來的 6 到 12 個月內在 web 環境中推出生產版本的 Flutter 應用。非常感謝大家的踴躍報名，我們收到了超過 1,000 份範例與申請，但由於名額有限，對無法參加的開發者，我們深表遺憾。不過，目前 web 支援已正式合併到 Flutter 框架中，因此，每一位開發者都有機會利用 Flutter web 支援構建出精美的應用。

我們的開發者已經建構了許多實用的 web 工具，下面就簡單介紹一下 Flutter Widget Livebook 和 Panache。

![Flutter Widget Livebook](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot9-release/communityexperiment1.png){:width="45%"}

![Panache](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot9-release/communityexperiment2.png){:width="45%"}

[Flutter Widget Livebook](https://flutter-widget-livebook.blankapp.org/) 是一個在網頁上展示 widget 執行效果的網站，它使用 Flutter 開發，並直接執行在網頁上。[Panache](https://rxlabz.github.io/panache_web/) 則是一款為 Flutter 建立主題的工具，您可以下載建立好的主題，然後將其直接新增到程式碼中。

歡迎大家嘗試更新後的實驗性 web 支援，並向我們 [分享您的使用感受](https://github.com/flutter/flutter/issues)。

## 社群

Flutter 驚人的成長速度和採用率讓我們倍感欣慰。在 Google 內部，有超過 20 個專案正在穩步推進中，凝集著數千位工程師的辛勤付出，其中有部分專案已成功落地，其餘的則尚在開發階段。在本週的 Google 開發者大會上，我們已經與大家分享了全球網際網路巨頭騰訊的成功經驗，介紹了騰訊是如何把 Flutter 靈活地運用到越來越多的產品中，歡迎收看下方影片，瞭解更多。
Bilibili 影片連結 https://www.bilibili.com/video/av67230699/

<iframe src="//player.bilibili.com/player.html?aid=67230699&cid=116573649&page=1&autoplay=false" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

現在，讓我們插個輕鬆的話題，邀請您參加一個有趣的小遊戲。請找到您手邊的 Google Assistant 裝置，然後對它說 “OK Google. Talk to Flutter Widget Quiz.” (OK Google, [為我接通 Flutter Widget 問答挑戰賽](https://assistant.google.com/services/a/uid/000000f3a4034e91))。十分感謝 Flutter 社群對這份小測試的傾情貢獻，期待各位小夥伴的精彩表現:

![Flutter Widget Quiz](https://devrel.andfun.cn/devrel/posts/2021/02/36cc2facf9626.png){:width="80%"}

## 結語

十分感謝 Flutter 開發者社群一路以來對我們的支援。來自全球各地的貢獻者們為 Flutter 1.9 的釋出投入了巨大的熱情與努力，透過部落格、文章、應用釋出、報錯、程式碼貢獻等多種渠道為我們提供幫助。如果您想了解 Flutter 1.9 的升級方法，學習如何修復遷移過程中可能出現的問題，請參閱 [Flutter 1.9.1 版本說明](https://github.com/flutter/flutter/wiki/Release-Notes-Flutter-1.9.1)。

Flutter 1.9 提供了超級豐富的新功能等您前來體驗，比如，新的 [dart:ffi](https://medium.com/dartlang/announcing-dart-2-5-super-charged-development-328822024970) 或者 [基於機器學習的程式碼自動完成功能](https://flutter.dev/web)，透過自己愛用且順手的開發工具探索在 web 平台執行 Flutter、Catalina 與 iOS 13 支援、兩款新出的 widget ([ToggleButtons](https://github.com/csells/flutter_toggle_buttons) 和 [ColorFiltered](https://github.com/csells/flutter_color_filter))，以及趣味滿滿的 [Flutter Widget 知識問答挑戰賽](https://assistant.google.com/services/a/uid/000000f3a4034e91)。

Flutter 1.9 已至，您將建構怎樣的精彩？
