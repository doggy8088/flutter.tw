---
title: Flutter Web 支援現已進入穩定版
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/03/90973fe9e93f5.png)

*作者 / Mariam Hasnany, Product Manager, Flutter*

我們對 Flutter 的願景是成為一個可移植的 UI 框架，在全平臺上建構精美的應用體驗。做為 [Flutter 2](https://zhuanlan.zhihu.com/p/355308562) 釋出內容的一部分，Flutter 的 web 支援已經抵達穩定版里程碑。

Flutter 的首個版本支援 iOS 和 Android，開發者們已經用它在移動應用商店釋出了超過 15 萬個應用。現在，隨著 web 支援的加入，這些應用可以觸達更廣泛的受眾，同時也開闢了在 web 上建立互動體驗的新途徑。

在此次初始版本的 web 支援中，我們主要關注三個應用場景:

* **漸進式 web 應用 (Progressive web apps, PWA)**，兼具 web 的高覆蓋面與桌面應用的強大功能。
* **單頁應用 (Single page apps, SPA)**，只需一次載入，並與網際網路服務動態互傳資料。
* **將現有 Flutter 移動應用拓展到 web**，在兩個平台共享程式碼。

這篇文章介紹了我們迄今為止的工作成果，並分享了幾個案例，意在幫助開發者在自己的應用中活用 Flutter 對 web 的支援。

![](https://devrel.andfun.cn/devrel/posts/2021/03/80f8ccc90315b.png)

> △ [iRobot Education](https://edu.irobot.com/the-latest/building-a-coding-experience-for-all) 使用 Flutter 開發了 [iRobot Coding](https://code.irobot.com/) 應用，透過此 web 應用向大眾提供程式設計學習體驗

## **Web 之旅**

如今的 web 平台比以往任何時候都要豐富多彩，開發者可以使用的工具套件括: [硬體加速的 2D 和 3D 圖形](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API)，[離線功能和安裝體驗](https://web.dev/progressive-web-apps/)，以及 [對底層作業系統和硬體的存取](https://web.dev/fugu-status/) 等。在 web 這個底層平臺上已經建立起了 [種類](https://vuejs.org/)[繁多](https://angular.io/)[的](https://flask.palletsprojects.com/)[框架](https://reactjs.org/)，因此，開發者在建立 web 應用時擁有極大的靈活性。

Flutter 是用 [Dart](https://dart.cn/) 編寫的，而 Dart 能編譯成 JavaScript，所以我們的下一步自然就是要探討支援 web 平台的可能性。這符合我們的願景，也就是提供一個可移植的框架，方便您在任何能描繪畫素的地方構建出精美的 UI。

我們的方法是，建立一個在所有平臺上都能使用的一致的工具套件 (而不是建立兩個有著各種微妙差異的獨立框架)，以確保開發者的程式碼執行時不會出現意外。

![](https://devrel.andfun.cn/devrel/posts/2021/03/edb077dfd7dda.png)

Flutter 框架由 [一系列層結構](https://flutter.cn/docs/resources/technical-overview#layer-cakes-are-delicious) 組成，其中包含:

* **框架**，用於為 widget、動畫和手勢等常見的習慣用法提供抽象
* **引擎**，使用公開的系統 API 在目標裝置上進行渲染

框架本身採用 Dart 編寫，大約 70 萬行 Flutter 框架核心程式碼在所有平臺上相同: 包括移動端、桌面端和現在的 web 端。對於您的程式碼來說也是這樣，我們使用 Dart 開發編譯器 ([dartdevc](https://dart.cn/tools/dartdevc)) 或 Dart 部署編譯器 ([dart2js](https://dart.cn/tools/dart2js)) 將您的程式碼編譯成 JavaScript，這些 JavaScript 程式碼可以託管在伺服器上。

由於 Dart 擁有將 Flutter 框架 (以及開發者的應用程式碼) 編譯成 JavaScript 的能力，我們對 web 的支援工作就變成了用對映 web 平台 API 的程式碼來取代移動應用所使用的底層 C++ 渲染引擎。Flutter 並不會簡單地將 widget 移植為 HTML 裡的等價元件，Flutter 的 web 引擎為開發者提供了兩種渲染器: 一個是針對尺寸和相容性進行最佳化的 HTML 渲染器，另一個則是使用 WebAssembly 和 WebGL 透過 Skia 繪圖命令向瀏覽器畫布進行渲染的 CanvasKit 渲染器。

我們對 Flutter 的要求是，提供一種針對 web 平台進行開發的新方式，在現有基礎上提供新見解，為所有人提供更棒的 web 體驗。

## **釋出生產環境可用的穩定版本**

自從 [web 支援的測試版](https://mp.weixin.qq.com/s/LyAtbiYYM6PEbxLoLD7OYA) 在一年前釋出以來，我們已經瞭解了很多早期採用者的使用情況，並已與部分客戶合作，他們現在已經將自己的 Flutter web 應用投入生產。

在此期間，我們對架構進行了重大改進，增加了一些功能，以便擴充和最佳化 Flutter 的 web 支援，新增內容主要集中在四個方面: **效能**、**web 專屬功能**、**桌面硬體適配**，以及**外掛**。

![](https://devrel.andfun.cn/devrel/posts/2021/03/f76550f48d532.png)

### **效能**

自推出早期版本至今，效能是提升最顯著的。在開發過程中，我們對 web 上各種渲染技術的效能和準確性特徵有了更深入的瞭解。

我們最早的工作是基於 DOM 的 HTML。在這種渲染模式中，Flutter 的 web 引擎會將每個產生的 Flutter 場景轉換為 HTML、CSS 或 Canvas，並以 HTML 元素樹的形式在頁面上渲染為一幀。雖然 HTML 渲染器能夠最大限度地相容各種瀏覽器，且其程式碼體積較小，但 HTML 渲染器的重繪效能不太適合 Rive (使用 Flutter 建構而成，用於建立動態圖像的協作工具) 這種圖形密集型應用。

![](https://devrel.andfun.cn/devrel/posts/2021/03/3b7b18f434f16.png)

> △ [Rive](https://rive.app/) 是一款建立自訂動畫的工具，該團隊已使用 web 版 Flutter 重新建構應用，併發布了測試版

為了提供高效渲染密集圖形所需的保真度，我們開始嘗試使用 [CanvasKit](https://skia.org/user/modules/canvaskit)，它可使用 [WebAssembly](https://webassembly.org/) 和 [WebGL](https://www.khronos.org/webgl/) 透過 Skia 繪製命令在瀏覽器中進行渲染。我們發現 CanvasKit 渲染器的效能、保真度和準確度都更加理想，請看 Flutter 社群中才華橫溢的德國開發者 [Felix Blaschke](https://github.com/felixblaschke) 的 [Flutter Plasma](https://flutterplasma.dev/) 示範——用 CanvasKit 創造的驚豔特效。

![](https://devrel.andfun.cn/devrel/posts/2021/03/c68e7a88df8ad.png)

> △ [Flutter Plasma](https://flutterplasma.dev/) 是由 Felix Blaschke 建立的示範，可在 Safari、Firefox、Edge 和 Chrome 上執行

不同的渲染器在不同場景下各有優勢，因此 Flutter 同時支援以下兩種渲染模式:

* **HTML 渲染器**: 結合了 HTML 元素、CSS、Canvas 和 SVG。該渲染模式的下載檔案體積較小。
* **CanvasKit 渲染器**: 渲染效果與 Flutter 移動和桌面端完全一致，效能更好，widget 密度更高，但增加了約 2MB 的下載檔案體積。

為了針對每個裝置的特性最佳化您的 Flutter web 應用，渲染模式預設設定為自動。這意味著您的應用將在移動瀏覽器上使用 HTML 渲染器執行，在桌面瀏覽器上使用 CanvasKit 渲染器執行。

您還可以使用 --web-renderer html 或 --web-renderer canvaskit 來明確選擇使用何種渲染器。如需瞭解詳細資訊，請參閱 [官方文件](https://flutter.cn/docs/development/tools/web-renderers)。

### **Web 專屬功能**

在瀏覽器中執行的 Flutter 應用給人的感覺應該像 web 應用一樣。所以我們為 Flutter 添加了一些功能，幫助您發揮 web 的優勢。

Web 有很多優勢，尤其是在全球的覆蓋率。將您現有的 Flutter 應用帶到 web 上的原因之一就是接觸應用商店以外的使用者。為了做到這一點，我們添加了 [自訂 URL 策略](https://flutter.cn/docs/development/ui/navigation/url-strategies)，以確保您的使用者只需點選 URL，就可以從任何地方存取您的應用。有了這個功能，您就可以控制位址列中顯示的 URL，以及您的應用在 web 上的路由。

![](https://devrel.andfun.cn/devrel/posts/2021/03/abd6aee377a66.png)

> △ Flutter Plasma 示範的 Showroom 頁面，實際上就是一個基於 Flutter 自訂 URL 策略的 [url_strategy](https://pub.flutter-io.cn/packages/url_strategy) 外掛範例

當用戶在 web 上導航時，超連結也至關重要。url_launcher package 中的一個新的 [link](https://pub.flutter-io.cn/documentation/url_launcher/latest/link/Link-class.html) widget 使使用者能夠透過深連結直達您應用內的錨點或外部網站。您可以在相關的 widget 上使用 link，包括按鈕、內聯文字、圖像，並指定連結是在同一個標籤頁還是新標籤頁中開啟。

對於任何應用來說，文字渲染都是不可或缺的。開發文字佈局系統，是建構 Flutter web 支援所面臨的重大挑戰之一。由於 web 缺乏直接文字佈局 API，Flutter 必須透過觸發 [layout()](https://api.flutter.cn/flutter/dart-ui/Paragraph/layout.html) 來對 [Parapraph](https://api.flutter.cn/flutter/dart-ui/Paragraph-class.html) 執行各種測量操作。有時，這些測量的代價相當高昂，所以我們添加了 [基於 Canvas 的文字測量](https://github.com/flutter/flutter/issues/33523)，此測量方式可同時支援純文字與富文字。現在，Flutter 可以在 web 上高效地完成精細測量，進而完成正確的繪製任務，比如正確地高亮顯示所選文字。

與文字進行互動同樣重要，其重要性不亞於快速準確地渲染文字。透過 SelectableText 和 EditableText widget，您不僅可以選中 Flutter web 應用中的文字，還可以執行復制貼上操作。此外，表單文字欄位現已支援 [自動填充](https://api.flutter.cn/flutter/widgets/AutofillGroup-class.html)，瀏覽器能夠儲存資料以便將來填充使用。

Flutter 2 特別適合實現漸進式 web 應用 (PWA)。我們建議開發者使用 PWA，透過 Chrome 的 [Project Fugu](https://web.dev/fugu-status/)，以安全和可信的方式，彌合移動端和 web 端應用之間的差異。

![](https://devrel.andfun.cn/devrel/posts/2021/03/0c26286d00f1c.png)

> △ 發票管理應用 [Invoice Ninja](https://www.invoiceninja.com/) 推出的 PWA 應用與他們現有的 Flutter 移動應用使用相同的程式碼庫

在建立 Flutter Web 應用時，我們會提供 PWA web [清單檔案](https://developer.mozilla.org/en-US/docs/Web/Manifest)，以及用來設定 service worker (工作執行緒) 的程式碼。清單檔案提供了關於應用應該如何執行的元資料，包括圖示和應用標題等資訊。[Service workers](https://developers.google.cn/web/ilt/pwa/introduction-to-service-worker) 可以實現資源的快取和應用的離線執行。當您在瀏覽器中以 PWA 的形式執行 Flutter 應用時，您可以將其作為移動或桌面應用安裝到您的裝置上。

### **適配各類桌面裝置**

儘管瀏覽器的形態大小各異，我們都希望提供美好的 Flutter web 體驗。由於 Flutter 最初是為移動應用設計而成，因此 Flutter web 應用已經對移動瀏覽器的手勢和滾動物理效果提供了很好的支援。但桌面瀏覽器 UI 的呈現和使用有所不同，所以我們對 Flutter 進行了相應的更新。

比如，使用者希望應用在桌面瀏覽器中執行時能夠顯示捲軸，以便透過滑鼠或鍵盤進行控制。我們為桌面裝置添加了 [可自訂的互動式捲軸](https://files.flutter-io.cn/flutter-design-docs/Updating_Scrollbars_(PUBLICLY_SHARED).docx)，這意味著我們可為捲軸使用 [主題](https://api.flutter.cn/flutter/material/ScrollbarTheme-class.html)，顯示捲軸軌道，而且還可以拖動滑塊。我們還擴充了 [PrimaryScrollController](https://api.flutter.cn/flutter/widgets/PrimaryScrollController-class.html)，便於使用者 [使用鍵盤快捷鍵進行滾動](https://files.flutter-io.cn/flutter-design-docs/Fallback_ScrollAction_(PUBLICLY_SHARED).docx)，也省去了您使用自訂滾動檢視的工作。

![](https://devrel.andfun.cn/devrel/posts/2021/03/72812bd5afb8b.jpg)

> △ [Spica Technologies](https://spicatech.co.uk/) 為 [Zurich Insurance](https://www.zurich.com/) 建構的物業管理解決方案，這是用 Flutter web 為商務和桌面裝置使用者建構應用的傑出範例

此外，由於滑鼠指標能進行互動的內容密度大於觸控裝置，我們提升了 [預設內容密度](https://github.com/flutter/flutter/issues/43350)。我們還在框架中添加了支援各種平台的 [系統滑鼠游標](https://github.com/flutter/flutter/issues/60641) 合集。

最後，為讓 Flutter web 支援所有使用者，我們還擴充了 Flutter 的 web 語義功能來支援 Windows、macOS 和 chromeOS 系統上的無障礙功能。為了在 web 上實現無障礙體驗，我們在 [RenderObject](https://api.flutter.dev/flutter/rendering/RenderObject-class.html) DOM 樹之外平行生成了一個類別似的 DOM 樹，叫 [SemanticsNode](https://api.flutter.dev/flutter/semantics/SemanticsNode-class.html) 樹。SemanticsNode 樹可將標記、操作、標籤和其他語義屬性轉換成 [ARIA 屬性](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)。現在，您可以透過 [Narrator](https://support.microsoft.com/en-us/windows/complete-guide-to-narrator-e4397a0d-ef4f-b386-d8ae-c172f109bdb1)、[VoiceOver](https://www.apple.com/accessibility/vision/)、[TalkBack](https://support.google.com/accessibility/android/answer/6007100?hl=en) 或 [ChromeVox](https://support.google.com/chromebook/answer/7031755?hl=en#:~:text=You%20can%20turn%20ChromeVox%20on,then%20ChromeVox%20will%20start%20speaking.) 螢幕閱讀器來使用 Flutter web 應用。

### **Flutter web 對外掛的支援**

最後，我們為那些最常用的外掛帶來了 web 支援，使您能夠將自己的 Flutter 應用帶到 web 平台。藉助 [Flutter 外掛](https://pub.flutter-io.cn/)，您的程式碼可與所執行平台的原生開發庫進行互操作。在 web 上執行 Flutter 應用時，您可以透過外掛存取現有的 JavaScript 庫。

自測試版釋出以來，我們在社群的幫助下為以下外掛添加了 web 支援:

* [image_picker](https://pub.flutter-io.cn/packages/image_picker_for_web)
* [google_maps](https://pub.flutter-io.cn/packages/google_maps)
* [firebase_analytics](https://pub.flutter-io.cn/packages/firebase_analytics)
* [firebase_storage](https://pub.flutter-io.cn/packages/firebase_storage)
* [connectivity](https://pub.flutter-io.cn/packages/experimental_connectivity_web)
* [cloud_firestore](https://pub.flutter-io.cn/packages/cloud_firestore)
* [cloud_functions](https://pub.flutter-io.cn/packages/cloud_functions)
* [cross_file](https://pub.flutter-io.cn/packages/cross_file)

## **展望未來**

幾年前，我們還沒辦法在 web 上以可接受的品質和效能提供 Flutter。然而，web 新技術的出現和平台的不斷進步，使我們得以盡情釋放底層裝置的潛力。在支援 web 之後，Flutter 得以涵蓋網際網路上的每一臺裝置，讓使用者在所有現代瀏覽器和裝置上都能獲得一致的體驗。

這個版本的相當一部分內容來自早期 web 使用者的反饋資訊和社群提交的 issue，這裡我們要再次感謝大家的貢獻！今後，我們的首要目標是快速處理大家的反饋，並及時解決 issue，以便大家專注於在所有目標平臺上釋出高品質的 Flutter 應用。

![](https://devrel.andfun.cn/devrel/posts/2021/03/7e72a89aa6bc5.png)

> △ [Moi Mobiili](https://www.moi.fi/) 是一家現代移動虛擬網路營運商，最近使用 Flutter 推出了他們的 web 應用

效能的提升永無止境。我們的目標是減少程式碼體積，提高幀率表現 (fps)。如今，每個 Flutter web 應用都只會下載它必需的引擎程式碼。我們正在研究快取部分邏輯的可能性，以減少啟動時間和下載檔案體積。我們最近在 Flutter Gallery 示範應用中嘗試使用延遲庫來減少程式碼體積，相信很快就能同大家分享我們的進展。

在未來幾個月內，我們還準備繼續完善下列領域:

* 雖然 CanvasKit 很穩定，但還有一些邊界使用案例存在問題，比如特殊字元的 [字型回退](https://github.com/flutter/flutter/issues/74741)，以及對 [跨域資源共享 (CORS) ](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) 圖像的相應支援等。
* PWA 目前 [只快取了資源的一個子集](https://github.com/flutter/flutter/issues/75861)，完全的離線支援仍然需要 [額外的手動步驟](https://github.com/flutter/flutter/issues/70101)，才能正常適配 CanvasKit。
* 文字渲染和功能，比如對樣式設定較為複雜的文字的選取，仍是我們要繼續努力解決的功能之一。
* 我們也會繼續努力改善外掛生態系統，讓 Google 釋出的 package 在移動端和 web 端更加統一。

![](https://devrel.andfun.cn/devrel/posts/2021/03/be293ff0f6829.png)

> △ [Simplebet](https://simplebet.io/) 透過 Flutter 的 web 支援，在 Fanduel 現有的移動應用套件中建構了高度互動的嵌入式 NFL 和 NBA 投注體驗

## **即刻開始使用 Flutter web**

藉助 Dart 的可移植性、Web 平台的強大功能，以及 Flutter 框架的靈活性，您現在可以用同一套程式碼庫，建構用於 iOS、Android 以及瀏覽器的應用。

如果您已經開發了 Flutter web 應用，現在就可以在 [穩定渠道](https://github.com/flutter/flutter/wiki/Flutter-build-release-channels#stable) 中進行建構。如果您剛開始學習建構 Flutter web 應用，請移步官方文件存取我們的 [入門 codelab 課程](https://flutter.cn/docs/get-started/codelab-web)，以及 Flutter Engage 上的 [web 演講](https://www.bilibili.com/video/BV1Jv411h7x6)。建構 web 應用時，如果您發現了任何問題，請隨時 [前往 GitHub 提交給我們](https://goo.gle/flutter_web_issue)。

我們非常期待看到您使用 Flutter web 所建構的精彩應用！