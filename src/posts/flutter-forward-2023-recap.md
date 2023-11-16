---
title: 與 Flutter 共創未來 | Flutter Forward 活動精彩回顧
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2023/02/QXD8vj.jpg)

*作者 / Google 開發者框架和語言 (含 Flutter、Dart 和 Go) 產品經理 & 使用者體驗總監 Tim Sneath*

我們很高興可以在 [Flutter Forward 活動](https://flutter.dev/events/flutter-forward "Flutter Forward 活動") 上分享我們對 Flutter 的願景。Flutter Forward 是在肯尼亞內羅畢以線上直播方式舉行的開發者活動，世界各地的開發者能夠親自參與或者遠端相聚，探索 Flutter 的未來發展方向。

Flutter 是一個介面工具套件，它讓應用開發者只需編寫一套程式碼，即可建構移動應用、Web 應用和桌面應用。您可以使用 Flutter 建構**精緻美觀**的應用，螢幕上的每一個畫素盡在掌握。Flutter 具有如下獨特優勢:

* **快速** 。支援硬體加速圖形和原生編譯的機器程式碼，可充分發揮手機或電腦硬體的計算能力。
* **高效** 。支援有狀態熱重載等技術，讓您可立即看到程式碼更改在應用中的實際效果。
* **可移植** 。使用一套原始碼即可部署到多種平台，而不會出現意外情況。
* **開源** 。它是一個完全開源的工具套件，您無需支付許可費，也不用為相關開發工具付費。

事實證明 Flutter 深受歡迎: 迄今為止已有超過 70 萬款使用 Flutter 打造的應用上架。Flutter 的使用者中既有 [志存高遠的小型初創公司](https://flutter.dev/showcase/so-vegan "志存高遠的小型初創公司")，也不乏 [有關鍵需求的大型成熟企業](https://flutter.dev/showcase/credit-agricole "有關鍵需求的大型成熟企業")。Flutter 的價值還讓 Google 內部團隊受益，[Google 課堂](https://edu.google.com/workspace-for-education/classroom/ "Google 課堂") 等團隊藉助 Flutter 為移動和 Web 使用者提供優質解決方案。我們還在其他開發者工具中添加了對 Flutter 的支援，這些工具套件括 [Google Ads](https://developers.google.cn/admob/flutter/quick-start "Google Ads")、[Google Maps](https://codelabs.developers.google.com/codelabs/google-maps-in-flutter#0 "Google Maps")、[Google Pay](https://pub.dev/packages/pay "Google Pay")，當然還有 [Firebase](https://firebase.google.cn/docs/flutter/setup?platform=ios)。

<video controls width="690" height="480" src="https://devrel.andfun.cn/devrel/posts/2023/02/RKj6GN.mp4" poster="https://devrel.andfun.cn/devrel/posts/2023/02/UdtebS.jpg"></video>

△ Google 課堂與 Flutter

> 藉助 Flutter，我們將同一功能的程式碼量縮減了 66%…這意味著各個平臺上的 bug 數量減少，未來的技術負債也更低。
>
>
>
>
>
> Google 課堂軟體工程師 Kenechi Ufondu

Flutter 的第一個版本提供了用於建構 Android 和 iOS 移動應用的介面工具套件。在 [Flutter 3](https://flutter.cn/posts/introducing-flutter-3 "Flutter 3 正式釋出") 中，我們拓展了所支援的平台，不僅穩定支援 [Windows](https://flutter.cn/posts/announcing-flutter-for-windows "Flutter 正式推出 Windows 平台支援")、macOS、Linux 和 [Web](https://mp.weixin.qq.com/s/6oSwvPsMy6r4AW90aostiA "Flutter Web 支援現已進入穩定版")，還新增了對嵌入式平台的支援。在此基礎上，我們最新發布了穩定版本 Flutter 3.7，增加了許多新功能並做出了各種改進，包括引入新的 iOS 渲染引擎、增強對 Material 3 和 iOS 風格元件的支援、改善對國際化的支援、改進後臺處理以及對開發者工具做出更新。

在此次活動中，我們的重點是 ***展望未來***，帶您搶先了解我們對 Flutter 的下一波發力點: 突破性的圖形效能、Web 應用和移動應用的無縫整合、對新興架構的早期支援，以及持續關注開發者體驗。希望我們展示的這些將在未來幾個月內逐步推出的新功能，能讓您對我們描繪的 Flutter 願景充滿期待: 讓 Flutter 成為一個強大的工具套件，助力開發者打造令人賞心悅目的優質使用者體驗，並靈活部署到諸多平臺。

*特別說明: 我們在這裡預先介紹的功能仍處於開發階段，可能會在未來幾個月發生重大變更。在現階段展示這些功能旨在讓早期採納者有機會參與進來、做出貢獻。*

![](https://devrel.andfun.cn/devrel/posts/2023/02/FaQLxh.png)

## **突破性的圖形效能**

長期以來，由於涉及建構抽象層這個難題，跨平臺框架一直需要在視覺方面做出妥協。Flutter 採用了一種不同於大多數框架的方法，藉助自有渲染層，可在所有裝置上提供硬體加速的圖形和一致的視覺外觀。今後，我們將著力發展 *突破性的圖形效能*，擴大 Flutter 在這一領域的現有優勢。

在 Flutter Forward 活動中，我們展示了 Flutter 的下一代渲染引擎 **[Impeller](https://github.com/flutter/flutter/wiki/Impeller "Impeller")** 取得的更多進展。Impeller 專為 Flutter 最佳化，提高圖形管線方面的靈活性和控制力，併為我們帶來了新的機會。Impeller 使用預編譯的著色器，可減少執行時由著色器編譯引起的丟幀，從而實現更加可預測的效能。它利用了 Metal 和 Vulkan 的基元型別，二者分別是 iOS 和 Android 中的現代底層圖形 API。以及有效地運用了併發機制，將同一幀的工作負載分散到多個執行緒中。

![△ Impeller 為 Wonderous 這類要求較高的圖形應用帶來了絲般順滑的效能。Wonderous 是一款帶您探索世界奇觀的精美應用，這裡展示了它的最新版本，可根據不同的裝置和外形規格調整其介面。您可前往 [https://wonderous.app](https://wonderous.app "wonderous.app") 下載該應用。](https://devrel.andfun.cn/devrel/posts/2023/02/a3Dk7v.png)

△ Impeller 為 Wonderous 這類要求較高的圖形應用帶來了絲般順滑的效能。Wonderous 是一款帶您探索世界奇觀的精美應用，這裡展示了它的最新版本，可根據不同的裝置和外形規格調整其介面。您可前往 [https://wonderous.app](https://wonderous.app "wonderous.app") 下載該應用。

除了提供流暢的介面，在某些情況下，Impeller 還可以顯著提高效能。下面這個影片中的 demo 就完美詮釋了這一點。左側是一個使用 SVG 剪下建構的萬花筒應用，該應用使用了當前的預設渲染器。向下滾動頁面時，渲染所用時間會超過每幀的預算，導致效能下降、幀率跌至 7-10 fps。右側是同一個應用，但採用了 Impeller，能以 60 fps 的幀率流暢渲染。

<video controls width="690" height="480" src="https://devrel.andfun.cn/devrel/posts/2023/02/cLx9Wq.mp4" poster="https://devrel.andfun.cn/devrel/posts/2023/02/qPCYaj.jpg"></video>

△ 萬花筒應用範例，展示了使用 Impeller 可提高圖形效能

從頭開始建構帶來的一個好處是，Impeller 的架構可支援全新的使用使用案例。得益於新引入的 **對自訂著色器的支援**，我們已經有了一些 [令人驚歎的新 demo](https://twitter.com/reNotANumber/status/1599717360096620544 "令人驚歎的新 demo")，展示了與 Flutter widget 層次結構的無縫整合。此外，我們並未止步於移動端，還在 Web 端釋出了對自訂著色器的早期支援。現在您可以使用同一套程式碼，在 iOS、Android 和瀏覽器中提供硬體加速的體驗。

![Flutter 現在支援在 Web 端使用畫素著色器，讓您可實現各種炫酷的視覺效果。（圖片提供者: [Erick Ghaumez](https://medium.com/u/21767146c3d4?source=post_page-----b94ce089f49c "Erick Ghaumez")）](https://devrel.andfun.cn/devrel/posts/2023/02/BNbAEb.png)

△ Flutter 現在支援在 Web 端使用畫素著色器，讓您可實現各種炫酷的視覺效果。（圖片提供者: [Erick Ghaumez](https://medium.com/u/21767146c3d4?source=post_page-----b94ce089f49c "Erick Ghaumez")）

另外，我們已經著手開展 **使 Flutter 支援 3D 圖形** 的早期工作。在主題演講中，我們示範了您可以匯入使用 [Blender](https://www.blender.org/ "Blender") 建立的模型，甚至可以利用熱重載技術，在 Blender 中即時迭代模型，然後在正執行的應用中檢視結果。雖然該功能還處於早期階段，但我們對所實現的初始效能以及將 3D 整合到其他 Flutter 體驗中的潛力滿懷期待。

![△ 這個有趣的 Dash demo 展現了 Impeller 讓 Flutter 能夠渲染 3D 圖形。](https://devrel.andfun.cn/devrel/posts/2023/02/kYeQ6b.png)

△ 這個有趣的 Dash demo 展現了 Impeller 讓 Flutter 能夠渲染 3D 圖形。

支援 3D 圖形和自訂著色器後，跨平臺介面工具套件可實現的圖形效能達到了新高度。我們非常期待看到您充分利用這些新推出的功能。

## **Web 應用和移動應用的無縫整合**

儘管您可以完全用 Flutter 和 Dart 編寫應用，但幾乎所有重要的專案都會涉及呼叫原生平台的功能。在 Web 端，Flutter 可能會用作大型應用的一個嵌入式元件；在移動端，應用可能需要呼叫系統 API 或其他語言的程式碼。因此，我們的第二個發力點就是  *Web 應用和移動應用的無縫整合*。

在 Web 端，我們將會推出一項名為 **元素嵌入** 的新功能，可用於將 Flutter 內容新增到任何標準 Web <div> 中。以這種方式嵌入時，Flutter 就變成了一個 Web 元件，能與 Web DOM 良好整合，甚至支援使用 CSS 選擇器和轉換來設定父 Flutter 物件的樣式。

此外，我們還對 [js](http://pub.dev/packages/js "js") 軟體包進行了一些重大更改，以便在 **JavaScript 程式碼和 Dart 程式碼之間實現順暢的互操作性**。利用 js，您可以透過 @JSExport 屬性為 Dart 程式碼中的任何函式添加註釋，然後從 JavaScript 程式碼中進行呼叫。

這兩項新功能相結合，使得 Flutter 可在 Web 端解鎖一些令人期待的新使用案例。我們在 Flutter Forward 活動中展示了一個 [概念驗證 demo](https://flutter-forward-demos.web.app/#/ "概念驗證 demo ")，您可以看到一個嵌入在 HTML 網頁中的簡單 Flutter 應用。使用 CSS，我們可以實現動畫旋轉效果；即使在旋轉時，Flutter 內容也一直可供互動。該 demo 還展示了您可以使用 HTML 按鈕和 JavaScript 事件處理指令碼來改變 Flutter 的狀態，反之亦然。我們認為，這項功能正式推出後，將在使用 Flutter 向現有 Web 應用增加互動方面，帶來更多可能性。

![△ 利用 "元素嵌入" 功能，您可以將 Flutter 嵌入 <div> 元素中並使用 CSS 來設定其樣式。](https://devrel.andfun.cn/devrel/posts/2023/02/tAcjsa.png)

△ 利用 "元素嵌入" 功能，您可以將 Flutter 嵌入 <div> 元素中並使用 CSS 來設定其樣式。

在 Android 和 iOS 端，Flutter 一直支援使用平台渠道與系統 API 整合。透過這些渠道您可使用基於訊息的方法，與用 Kotlin 或 Swift 等語言編寫的程式碼進行通訊。但這仍要求應用開發者通曉多種語言，並需要大量樣板程式碼。

我們正著手研究 **一種新的系統互操作性方法**，以允許直接呼叫庫。在 iOS 端，我們在此前所做 FFI 工作的基礎上實現 C 語言互操作性，添加了對 Swift 和 Objective-C 庫的支援。在 Android 端，我們使用 JNI 橋接到用 Kotlin 編寫的 Jetpack 庫。Dart 中新增了一個命令，可用於自動建立 binding 繫結以實現跨語言互操作，並相應地轉換資料類別。我們希望這項功能推出後，Flutter 開發者能夠呼叫新的 Jetpack 或 iOS 庫，而無需使用外掛或學習不同的 API 語法，同時也極大地減輕外掛開發者的工作。您可以 [檢視我們的範例](https://github.com/flutter/samples/tree/main/experimental/pedometer) 瞭解詳情。

## **對新興架構的早期支援**

得益於 Dart 對諸多處理器架構的廣泛支援，以及高度最佳化的 JavaScript 編譯器，Flutter 已經可以在很多不同型別、不同外形規格的裝置上執行。與此同時，也有一些優秀的新架構逐漸興起，因此我們的第三個發力點就是 *對新興架構的早期支援*。

[WebAssembly](https://webassembly.org/ "WebAssembly") 作為一種平臺中立的二進位制指令格式，如今發展已經日漸成熟，[在現代瀏覽器上獲得了越來越多的支援](https://caniuse.com/wasm "在現代瀏覽器上獲得了越來越多的支援")。令人興奮的是，WebAssembly 使得 Web 平台向 JavaScript 之外的其他語言敞開了大門。近幾個月來，我們與 Chrome 團隊和其他 WebAssembly 合作伙伴攜手，著力於對 Dart 等 [垃圾回收語言的早期支援](https://github.com/WebAssembly/gc "垃圾回收語言的早期支援")。Chrome 的最新開發版中提供了一個 flag 標誌，用於開啟這個新的 WebAssembly 擴充。在 Flutter Forward 活動中，我們公佈了對 **從 Flutter 編譯到 WebAssembly** 的早期支援，這使得我們可以進一步最佳化 Web 端的速度和程式碼體積。

吸引開發者關注的另一種平台架構是 [RISC-V](https://en.wikipedia.org/wiki/RISC-V "RISC-V")，這是一種為廣泛使用而設計的開放式標準指令集架構 (ISA)。Android 團隊最近 [在支援 RISC-V 方面的工作](https://www.youtube.com/watch?v=70O_RmTWP58 "他們在支援 RISC-V 方面所做的工作") 有所進展。我們也很高興地宣佈 **Dart 現在支援 RISC-V**，相關工作的目標是讓 Flutter 能夠在陸續上市的 RISC-V 裝置上執行。儘管生產級 RISC-V 硬體仍處於起步階段，但我們依然在 Flutter Forward 活動中展示了目前為止在 [ClockworkPi DevTerm Kit R-01](https://www.clockworkpi.com/product-page/devterm-kit-r01 "ClockworkPi DevTerm Kit R-01") 上取得的進展，它是一個執行 Linux 的自組裝行動式終端裝置。我們認為在嵌入式場景中，尤其應該支援 RISC-V，Flutter 可以在此情景中為各類需求提供強大的介面工具套件。

![△ 一臺正在執行 Dart 控制檯應用的 ClockworkPi DevTerm R-01 (一款實驗性 RISC-V 電腦)](https://devrel.andfun.cn/devrel/posts/2023/02/6mBeRT.png)

△ 一臺正在執行 Dart 控制檯應用的 ClockworkPi DevTerm R-01 (一款實驗性 RISC-V 電腦)

## **持續關注開發者體驗**

開發者生產力是上述一切的基礎，Flutter 與生俱來的有狀態熱重載等功能賦予了它高效的屬性。我們第四個也是最後一個發力點，就是將 *持續關注 Flutter 和 Dart 開發者體驗*。

一直以來我們都在為推動 Dart 語言的健康發展做出努力。在 Flutter Forward 活動中，我們帶大家搶鮮瞭解了一些重要的新 Dart 語言功能的初步進展。對 **records 和 patterns** 的早期支援現已登陸開發渠道，這兩項新的增強功能協同運用可以發揮出良好效果。

![△ 一個簡單的 records 和 patterns 範例，可返回和接收多個函式引數。](https://devrel.andfun.cn/devrel/posts/2023/02/jJMm1S.jpg)

△ 一個簡單的 records 和 patterns 範例，可返回和接收多個函式引數。

此外，我們還 **正式釋出了 Dart 3**，標誌著我們將健全的空安全引入 Dart 語言的工作交出了圓滿答卷。Dart 3 還移除了其他一些早已棄用的功能，更加現代化。我們已開始釋出 Dart 3 的 Alpha 版本及對應的 Flutter 版本，方便開發者測試軟體套件和應用。如需詳細瞭解 Dart 3，您可以參閱 [Dart 頻道中釋出的博文](https://medium.com/dart-lang/dart-3-alpha-f1458fb9d232 "Dart 頻道中釋出的博文")。

當然，我們也在著力提升 Flutter 的開發者體驗。繼去年我們 [在 I/O 大會上宣佈推出的](https://flutter.cn/posts/announcing-the-flutter-casual-games-toolkit "Flutter 休閒遊戲工具套件釋出") 休閒遊戲工具套件大獲成功之後，我們釋出了 [**新聞工具套件** 的第一個版本](https://medium.com/flutter/announcing-the-flutter-news-toolkit-180a0d32c012 "新聞工具套件的第一個版本")，這將加速新聞釋出商和其他內容提供方的移動應用開發，讓他們無需從頭開始設計應用就能觸達移動端使用者。它包含了打造以新聞報道為中心的應用所需的一切功能，包括導航和搜尋、身份驗證、廣告植入、通知、檔案和訂閱，同時納入了根據 Google 新聞計劃的研究成果確定的最佳實踐。我們要分享三個使用該工具套件建構應用的非洲早期採納者的故事，他們分別是摩洛哥最著名的新聞網站之一 [Hespress](https://www.hespress.com/ "摩洛哥最著名的新聞網站之一 Hespress")、尼日利亞的熱門體育網站 [Bold Sports](https://boldsportsng.com/ "尼日利亞的熱門體育網站 Bold Sports") 以及肯尼亞歷史最悠久的報紙 [《The Standard》](https://www.standardmedia.co.ke/ "肯尼亞歷史最悠久的報紙 《The Standard》")。

<video controls wdith="690" height="480" src="https://devrel.andfun.cn/devrel/posts/2023/02/yHWQPt.mp4" poster="https://devrel.andfun.cn/devrel/posts/2023/02/2xmwUE.jpg"></video>

△ 觀看三個非洲新聞釋出商的故事，瞭解他們作為 Google 新聞工具套件早期採納者的體驗。

## **攜手前行**

衷心希望您能和我們一樣，對 Flutter 的未來發展滿懷期待。我們將繼續著力提升核心開發者體驗，同時進行一些基礎性改進，讓所有人都能順暢地使用 Flutter 打造更出色的體驗。

下圖對我們的發展方向進行了總結:

![](https://devrel.andfun.cn/devrel/posts/2023/02/rmxfey.png)

除了自身擬定的開發方向，我們也很高興看到 Flutter 生態系統繼續蓬勃發展。例如，[FlutterFlow](https://flutterflow.io/ "FlutterFlow") 是一個用於開發原生移動應用的低程式碼建構工具，而 [Widgetbook](http://widgetbook.io/ "Widgetbook") 提供靈活的工具，供設計師和開發者協作開發使用者介面。

最後，很高興藉此機會來到肯尼亞，領略了當地開發者的企業家精神和才華。非洲的 Flutter 社群充滿活力；僅肯尼亞的 Flutter 使用者群組就有超過 1,000 名開發者。我們很高興 Flutter 能夠為非洲的開發者帶來新的機遇，讓他們可以參與到快速增長的應用經濟中。Klasha 就是詮釋這一前景的優秀案例。這家公司藉助 Flutter 快速進入市場，並解決了本地使用者的問題。下面的影片介紹了他們使用 Flutter 的體驗:

<video controls wdith="690" height="480" src="https://devrel.andfun.cn/devrel/posts/2023/02/a4nD2R.mp4" poster="https://devrel.andfun.cn/devrel/posts/2023/02/DrP6nP.jpg"></video>

△ Klasha ([https://klasha.com/](https://klasha.com/ "Klasha")) 的故事以及他們使用 Flutter 的體驗。Klasha 是一家科技初創公司，其宗旨是讓非洲消費者能夠順暢地買到世界各地的商品。
