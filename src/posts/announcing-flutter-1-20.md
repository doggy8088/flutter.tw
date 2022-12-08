---
title: Flutter 1.20 正式釋出
description: 效能改進、移動端自動填充、全新 widget 以及更多內容！
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2020/08/bbbf90037ad95.png)

作者 / Chris Sells, Product Manager, Flutter developer experience

我們對 Flutter 的願景是提供一個可移植的工具套件，讓您無論在任何螢幕上都能隨心所欲地繪製畫素，打造出美好的體驗。每次更新，我們都著力確保 Flutter 能夠在所有支援的平臺上執行流暢、介面美觀、開發高效而且保持開放性。透過穩定版渠道釋出的 Flutter 1.20 在上述四個方面都進展頗多。

首先是執行流暢，我們從最底層的渲染引擎到 Dart 語言本身都實現了多項效能改進。

為了讓您構建出更美觀的 Flutter 應用，此版本提供了多項介面改進，包括期待已久的自動填充支援、支援平移和縮放的新 widget 佈局方式、新的滑鼠游標支援、舊版本中人氣 Material widget（如時間和日期選擇器）的更新，以及為桌面和移動端 Flutter 應用中的關於（About）介面帶來了全新的響應式許可（license）展示頁面。

為了進一步提高開發效率，我們更新了 [Visual Studio Code 的 Flutter 擴充](https://dartcode.org/)，將 Dart DevTools 直整合進 IDE，在您移動檔案時會幫您自動更新匯入陳述式，並提供了一組新的元資料用於建構您自己的工具。

得益於 Flutter 的開放性和來自社群的出色貢獻，此版本包含了全球 359 名貢獻者（其中包括來自 Flutter 社群的 270 名貢獻者）的 3,029 個合併 PR，關閉了 5,485 個 issue。因此本次更新的 Flutter 版本也是目前為止擁有最多貢獻者的版本。在這裡特別感謝在社群中貢獻了 28 個 PR的 [CareF](https://github.com/CareF)，貢獻了 26 個 PR 的 [AyushBherwani1998](https://github.com/AyushBherwani1998)（包括他用於 Google Summer of Code 專案的 10 個 Flutter 範例），以及貢獻了 13 個 PR 的 [a14n](https://github.com/a14n)（其中一大部分用於支援 Flutter 的空安全性，有關該主題的更多資訊即將到來！）。Flutter 的誕生離不開社群貢獻者們的廣泛支援。謝謝大家！

Flutter 每一個新版本的釋出都伴隨著使用量的增長和更迅猛的發展態勢。事實上，在 4 月份我們曾 [報道過](https://flutter.cn/posts/flutter-spring-2020-update) Google Play 商店中 Flutter 應用的數量已經達到 50,000，月度新增應用數量峰值更是高達 10,000。現在，短短三個月後，Google Play 中的 Flutter 應用數量已經超過 90,000。增長最快的當屬印度，那裡已經是 Flutter 開發者最多的地區，開發者數量在過去六個月中翻了一番，這與 [Google 在該地區增加的投資](https://www.businessinsider.com/google-alphabet-india-health-agriculture-education-tech-ai-sundar-pichai-2020-7) 密切相關。最後，如果沒有 Dart 語言，Flutter 也不會成為現在的 Flutter。這裡分享一個好訊息：在 IEEE 的 [開發語言榜單](https://spectrum.ieee.org/static/interactive-the-top-programming-languages-2020) 中，Dart 相比去年上升了 4 位，在榜單的前 50 種語言中排名第 12。

## Flutter 和 Dart 的效能改進

Flutter 團隊一直在尋找縮減應用大小和延遲的新方法。對於大小，[此版本修復了在進行圖示字型搖樹（tree-shaking）操作時的工具效能問題](https://github.com/flutter/flutter/pull/55417)，並在您建構非 web 應用時預設進行 [字型搖樹操作](https://github.com/flutter/flutter/pull/56633)。圖示字型搖樹操作會移除應用中未使用的圖示，從而縮減其大小。在對 Flutter Gallery 應用進行該操作後，我們發現 [應用大小縮減了 100kb](https://github.com/flutter/flutter/pull/49737)。現在，在建構移動版應用的釋出版本時該操作會預設執行。目前僅限於 TrueType 字型，但在未來版本中將取消這個限制。

此版本帶來的另一項效能改進是使用預熱階段減少動畫初始顯示時的卡頓。以下為卡頓改進的動畫範例（半速播放）。

![](https://devrel.andfun.cn/devrel/posts/2020/08/455f666234c9d.gif)

△ 使用和不使用 SkSL 預熱的動畫

如果 Flutter 應用在首次執行時的動畫出現卡頓，那麼 Skia Shading Language 著色器將在應用建構中提供預編譯，將速度提高 2 倍以上。
如果您想使用此高階功能，請參見 flutter 文件中的 [SkSL 預熱頁面](https://flutter.cn/docs/perf/shader)。

最後，在針對桌面環境的最佳化中，我們進一步完善了對滑鼠的支援。在此版本，我們 [重構了滑鼠點選測試系統](https://github.com/flutter/flutter/pull/59883)，帶來了許多曾因效能問題受阻的架構優勢。在基於 web 的微型基準測試中，重構使效能提高了多達 15 倍！這意味著，您可以在保證效能的前提下，獲得更好、更一致、更準確的點選測試結果：實現雙贏！

有了更好、更快、更強大的滑鼠點選測試，我們又增加了滑鼠游標支援，這也是桌面端最受期待的功能之一。一些常用的 widget 將預設顯示主流游標，您也可以從支援的游標列表中指定其他游標。

![](https://devrel.andfun.cn/devrel/posts/2020/08/86c0af8ecf3d3.gif)

△ 滑鼠在 Android 既有的 widget 上懸停時切換顯示游標

此版本 Flutter 基於 2.9 版 Dart 建構，採用基於狀態的全新雙通 UTF-8 解碼器，解碼原語在 Dart VM 中最佳化，部分利用了 SIMD 指令。UTF-8 是目前網際網路上最常用的字元編碼方法。在接收大型網路響應時，快速對其進行解碼至關重要。在 UTF-8 解碼基準測試中，我們在低端 ARM 裝置上測得的效能得到了全面改進：英語文字提升至近 200%，中文文字提升至 400%。

## 移動端文字欄位自動填充

一段時間以來，呼聲最高的 Flutter 功能之一就是為 Flutter 應用中的文字自動填充提供 Android 和 iOS 的底層支援。透過 [PR 52126](https://github.com/flutter/flutter/pull/52126)，我們很高興地宣佈該支援已經實現，如果作業系統已經蒐集到可供自動填充的資訊，您的使用者無需再重新輸入了。

![](https://devrel.andfun.cn/devrel/posts/2020/08/04693616a125a.gif)

△ 自動填充

再告訴您一個好訊息，我們已經開始著手在 web 端實現這一功能。

## 用於常見互動模式的全新 widget

此版本引入了一個新的 widget：InteractiveViewer。InteractiveViewer 旨在為您的應用建構常見互動，如平移、縮放和拖放，甚至在可調節大小的視窗中也可實現這些互動，請參見下面這個 [簡單的圍棋範例](https://github.com/justinmc/flutter-go)。

![](https://devrel.andfun.cn/devrel/posts/2020/08/1926f95f7fd40.gif)

△　InteractiveViewer 的縮放、平移、調整大小與拖放

請檢視 [API 文件](https://api.flutter.cn/flutter/widgets/InteractiveViewer-class.html)，瞭解如何將 InteractiveViewer 整合到您自己的應用中，您也可以在 DartPad 中快速進行體驗。另外，如果您想了解 InteractiveViewer 的設計和開發經歷，可以觀看 ChicagoFlutter 釋出的[演講影片](https://www.youtube.com/watch?v=ChFa0A72Uto)。

有興趣在 Flutter 應用中加入更多類似 InteractiveViewer 的互動？歡迎瞭解一下我們在這一版本 [對拖放功能所做的增強](https://github.com/monkeyswarm/DragTargetDetailsExample)。具體來說，如果您想知道拖拽的“放置”操作發生在目標 widget（始終對 Draggable 物件可用）上的精確位置，現在您可以透過 DragTarget 的 onAcceptDetails 方法獲得該資訊。

![](https://devrel.andfun.cn/devrel/posts/2020/08/89b6c425a5767.gif)

△ 接收拖放目標詳情資訊示範

您可以透過這個 [範例](https://github.com/monkeyswarm/DragTargetDetailsExample)瞭解詳細資訊，未來版本還將在拖動過程中提供這些資訊，以便 DragTarget 在拖動操作期間更輕鬆地提供視覺化的更新。

## Material Slider / RangeSlider / TimePicker / DatePicker Widget 更新：

除了新新增的 widget，此版本還包含許多既有 widget 的更新，以匹配 [最新的 Material 指南](https://material-io.cn/components/sliders)。其中包括 Slider 和 RangeSlider。更多資訊參見 [Slider widget 的更新](https://medium.com/flutter/whats-new-with-the-slider-widget-ce48a22611a3)。

![](https://devrel.andfun.cn/devrel/posts/2020/08/163a04b7ec35d.png)

△ 新版 Material Slider

![](https://devrel.andfun.cn/devrel/posts/2020/08/4b39c88d13982.png)

△ 新版 Material RangeSlider

更新的 DatePicker 新添了緊湊型設計以及對日期範圍的支援。

![](https://devrel.andfun.cn/devrel/posts/2020/08/9b72841cf6b9a.gif)

△ 新版 DatePicker

最後，TimePicker 也有了全新的視覺風格。

![](https://devrel.andfun.cn/devrel/posts/2020/08/aa761b870a116.png)

△ 新版 TimePicker

如果您想上手操作，請試試 [使用 Flutter 建構的趣味網路示範](https://flutter-time-picker.firebaseapp.com/#/)。

## 響應式許可頁面

此版本的另一個更新是 AboutDialog 中提供的新的響應式許可頁面。

![](https://devrel.andfun.cn/devrel/posts/2020/08/f46d9adbfbdba.png)

△ 新的許可頁面

社群貢獻者 [TonicArtos](https://github.com/TonicArtos) 的 [PR 57588](https://github.com/flutter/flutter/pull/57588) 遵循 Material 指南進行更新，外觀更加精美，更易於導航，並且在平板電腦、桌面裝置和手機上都一樣好用。謝謝 TonicArtos！由於每個 Flutter 應用都應顯示其所用 package 的許可，如此一來每個 Flutter 應用都獲得了改進！

## 釋出外掛所需的新 pubspec.yaml 格式

當然，Flutter 不僅是 widget (在 Flutter 裡: Everything is a Widget)，它也是一個工具。今天這個版本中帶來的更新實在太多，無法一一提及。下面是一些亮點：

首先是一則宣告：如果您是 Flutter 外掛作者，釋出外掛時將不再支援使用舊的 pubspec.yaml 格式。在使用舊格式檔案執行 pub publish 時會收到以下錯誤訊息：

![](https://devrel.andfun.cn/devrel/posts/2020/08/439e46e8de177.png)

△ 外掛釋出時使用舊 pubspec 格式後收到的錯誤訊息

舊格式不能指定外掛支援的平台，自 Flutter 1.12 起已棄用。現在，釋出新的外掛或更新外掛時需要使用 [新的 pubspec.yaml 格式](https://flutter.cn/docs/development/packages-and-plugins/developing-packages#plugin-platforms)。

對於外掛的使用者，開發工具在當下和可預見的將來仍然能理解舊 pubspec 格式。在可預見的將來，pub.dev 上所有使用舊 pubspec.yaml 格式的既有外掛可繼續在 Flutter 應用中使用。

## 功能預覽：在 Visual Studio Code 中嵌入 Dart DevTools

此版本最大的工具更新是 Visual Studio Code 擴充，它提供了一項新功能的預覽，使您能夠將 Dart DevTools 介面直接嵌入程式設計工作區。

![](https://devrel.andfun.cn/devrel/posts/2020/08/1bb7686b6aee2.png)

△ 預覽功能：在 Visual Studio Code 中嵌入 Dart DevTools 的 Layout Explorer

使用新的 `dart.previewEmbeddedDevTools` 設定啟用此功能。在上面的螢幕截圖中，Flutter Widget Inspector 直接嵌入 Visual Studio Code，但是啟用新設定後，您可以使用狀態列上的 Dart DevTools 選單嵌入其他您偏好的頁面。

![](https://devrel.andfun.cn/devrel/posts/2020/08/e483d4839aed8.png)

透過此選單選擇要顯示的頁面。

![](https://devrel.andfun.cn/devrel/posts/2020/08/3a9be4183f989.gif)

該功能仍處於預覽狀態，如果您遇到任何問題，請 [在這裡提交反饋](https://github.com/Dart-Code/Dart-Code/issues)。

## 網路監測功能更新

最新版本 Dart DevTools 帶有更新的 Network 頁面，可以實現網路套接字分析。

![](https://devrel.andfun.cn/devrel/posts/2020/08/f2570ba7c41dd.png)

△ Dart DevTools 的 Network 頁面上的套接字連線時間、狀態和內容型別

現在，Network 頁面添加了應用進行網路呼叫的時間、狀態和內容型別等資訊。詳細資訊介面也有額外改進，以提供 websocket 或 http 請求中資料的概覽。我們還為這個頁面制定了更多計劃，包括加入 HTTP 請求/響應正文和 gRPC 流量監測。

## 在檔案重新命名時更新匯入陳述式

Visual Studio Code 的另一個新功能是當檔案被移動或重新命名時自動更新匯入陳述式。

![](https://devrel.andfun.cn/devrel/posts/2020/08/3a9be4183f989.gif)

△ 在 Visual Studio Code 中移動 Dart 檔案會更新匯入陳述式

該功能目前僅適用於單個檔案，暫不支援多個檔案或資料夾（即將到來！）。

## 面向每個工具製造者的工具元資料

還有一項為 Flutter 工具開發者提供的更新。我們在 GitHub 上建立了一個新專案，來捕獲和釋出有關 Flutter 框架本身的元資料。它提供以下機器可讀資料檔案：

* 當前所有 Flutter widget 的 [目錄](https://github.com/flutter/tools_metadata/blob/master/resources/catalog/widgets.json)（有 395 個！）
* Flutter 框架中 [顏色名稱到顏色值的對映](https://github.com/flutter/tools_metadata/tree/master/resources/colors)，支援 Material 和 Cupertino 顏色集
* Material 和 Cupertino 圖示的 [圖示元資料](https://github.com/flutter/tools_metadata/tree/master/resources/icons)，包括圖示名稱和預覽圖示

這與我們在 Android Studio / IntelliJ 和 VS Code 擴充中的元資料相同；我們認為這對您建構自己的工具會有所幫助。實際上，此元資料使 IntelliJ 系列 IDE 的功能可以顯示 Flutter 程式碼中使用的顏色：

![](https://devrel.andfun.cn/devrel/posts/2020/08/ca7d24a599cd8.png)

與此相關的是 IntelliJ 和 Android Studio 中的一項新功能，該功能可為 Color.fromARGB() 和 Color.fromRGBO() 顯示色塊：

![](https://devrel.andfun.cn/devrel/posts/2020/08/5c8d5b3dcd271.png)

特別感謝 [dratushnyy](https://github.com/dratushnyy) 在 GitHub 上為 IntelliJ 中的顏色預覽做出的貢獻！

## 平台互操作的型別安全平台通道

為了回應外掛作者在使用者調研中的普遍需求，最近，我們一直以 [外掛](https://flutter.cn/docs/development/packages-and-plugins/developing-packages) 和 [Add-to-App](https://flutter.cn/docs/development/add-to-app) （部分使用了 Flutter 的應用）為物件，探求如何才能讓 Flutter 與宿主平台之間的通訊更安全、更輕鬆。為了滿足這一需求，我們建立了命令列工具 [Pigeon](https://pub.flutter-io.cn/packages/pigeon)，使用 Dart 語法在平台通道上產生型別安全的訊息程式碼，無需新增其他執行時依賴項。您無需在平台通道上手動匹配方法字串和序列化引數，就可以呼叫 Java/Objective-C/Kotlin/Swift 類方法，並透過直接呼叫 Dart 方法傳遞非原始型別資料物件（反之亦然）。

![](https://devrel.andfun.cn/devrel/posts/2020/08/607007baf455d.png)

Pigeon 雖然處於預釋出階段，但已經足夠成熟，我們已經將其用於 [video_player](https://pub.flutter-io.cn/packages/video_player) 外掛。如果您有興趣測試 Pigeon 供自己使用，請參見更新的 [平台通道文件](https://flutter.cn/docs/development/platform-integration/platform-channels#pigeon) 以及此 [範例專案](https://github.com/flutter/samples/tree/master/add_to_app/flutter_module_books)。

## 還有眾多工具更新，不勝列舉

截至 Flutter 1.20 釋出，眾多工具的版本也全新亮相，我們無法在此列出所有內容，請檢視它們的更新公告：

* [VS Code 擴充 v3.13](https://groups.google.com/g/flutter-announce/c/TlN12RemsYw)
* [VS Code 擴充 v3.12](https://groups.google.com/g/flutter-announce/c/8tSufvaRJUg)
* [VS Code 擴充 v3.11](https://groups.google.com/g/flutter-announce/c/gM0bqO7NFA0)
* [Flutter IntelliJ 外掛 M46 版](https://groups.google.com/g/flutter-announce/c/8C2v2ueXjts)
* [Flutter IntelliJ 外掛 M47 版](https://groups.google.com/g/flutter-announce/c/6SF3PG_XB8g/m/6mAY7eC_AAAJ)
* [Flutter IntelliJ 外掛 M48 版](https://groups.google.com/g/flutter-announce/c/i9NTk5o9rZQ)
* [我們用 Flutter 寫了一套全新的 Flutter 開發者工具](https://mp.weixin.qq.com/s/4mcFo3z8DhCDkEMX7IPmww)

## 重要改動 (Breaking Changes)

與往常一樣，我們盡力將重要改動（breaking changes）的數量維持在較低水平。以下是 Flutter 1.20 版本中的重要改動列表。

* [55336](https://github.com/flutter/flutter/pull/55336)：將 tabSemanticsLabel 新增到 CupertinoLocalizations - 遷移 [指南 PR](https://flutter.cn/docs/release/breaking-changes/cupertino-tab-bar-localizations)
* [55977](https://github.com/flutter/flutter/pull/55977)：[將 clipBehavior 新增至具有 clipRect 的 widget](https://files.flutter-io.cn/flutter-design-docs/Clip_Behavior.docx)
* [55998](https://github.com/flutter/flutter/pull/55998)：[為 Navigator 的 TransitionDelegate 新加入了 isWaitingForExitingDecision 判斷。](https://groups.google.com/forum/#!searchin/flutter-announce/55998%7Csort:date/flutter-announce/yoq2VGi94q8/8pTsRL28AQAJ)	
* [56582](https://github.com/flutter/flutter/pull/56582)：[更新 Cupertino 中的 Tab 語義，使其與 Material 相同](https://flutter.cn/docs/release/breaking-changes/cupertino-tab-bar-localizations#migration-guide)
* [57065](https://github.com/flutter/flutter/pull/57065)：移除 NestedScrollView 重疊管理條中被棄用的子引數	
* [58392](https://github.com/flutter/flutter/pull/58392)：確保在 iOS 裡的系統行為一致性，為 CupertinoActivityIndicator 加入 progress 引數

## 總結

希望您和我們一樣喜愛這一版本。從很多角度來看，這都是 Flutter 迄今為止規模最大的版本釋出。其中包含效能的顯著提升、新增並更新了許多 widget，以及對工具做出的諸多改進，考慮到文章篇幅我們只能著重介紹部分亮點。我們要向大家致謝，感謝不斷壯大的社群貢獻者群體，讓每一個 Flutter 版本都比先前功能更豐富、執行更流暢、效能更強大。敬請期待更多內容，包括 [空安全](http://dart.cn/null-safety)支援、新版本的 Ads、Maps 和 WebView 外掛，以及正在建構的更多工具支援。（也歡迎大家閱讀 Bob Nystrom 的文章以深入 [瞭解空安全](https://dart.cn/null-safety/understanding-null-safety)）

Flutter 和工具已經全新升級，您會打造出怎樣精彩的 Flutter 作品呢？
