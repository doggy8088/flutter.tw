---
title: 在 I/O 看未來 | Flutter 和 Dart 最新進展
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2023/07/21/Adqvxc.png)

*作者 / Google 開發者框架和語言 (含 Flutter、Dart 和 Go) 產品經理/使用者體驗總監 Tim Sneath*

今年的 [Google I/O 大會](https://mp.weixin.qq.com/s/_FdkBIy3GDxSK--ZZosirA) 在位於加利福尼亞州山景城的總部附近舉辦，我們懷著無比激動的心情面向全球直播了這場盛會！

就在三個多月前，我們在肯尼亞內羅畢舉行的 [Flutter Forward 大會](https://flutter.cn/posts/flutter-forward-2023-recap) 上為 Flutter 和 Dart 推出了一個大膽的新路線圖。在此次 I/O 大會上，我們將透過介紹四個主題領域的最新動態，來分享我們在實現這一願景方面取得的進展，這四個主題分別為: 突破性的圖形效能、與 Web 應用和平台的無縫整合、對新興架構的支援，以及對開發者體驗的關注。

![](https://devrel.andfun.cn/devrel/posts/2023/07/21/04KBGl.png)

您可能已經瞭解到，Flutter 是一個介面工具套件，它讓應用開發者只需編寫一套程式碼，即可建構移動應用、Web 應用、桌面應用和嵌入式裝置應用。您可以使用 Flutter 建構精緻**美觀**的應用，完全掌控螢幕上的每一個畫素。Flutter 具有如下獨特優勢:

* **快速** 。支援硬體加速圖形和原生編譯的機器程式碼，可充分利用裝置的各種功能。
* **高效** 。支援有狀態熱重載等技術，讓您可立即看到程式碼更改在應用中的實際效果。
* **可移植** 。使用一套原始碼即可部署到多種平台，而不會出現意外情況。
* **開源** 。它是一個完全開源的工具套件，您無需支付許可費，也不用為相關開發工具付費。

## **Flutter 持續發展**

在 Google 乃至整個行業中，Flutter 的使用量都在持續增長。在 Google，我們的團隊已經在移動、Web 和桌面平臺上部署了 Flutter 應用，範例包括:

* Android 的最新應用 "[Nearby Share" 適配 Windows](https://www.android.com/better-together/nearby-share-app/)。這款應用使用 Flutter 建構，允許在 Windows 和 Android 裝置之間無線分享照片和文件。

* [全新的 Play 管理中心應用](https://play.google.com/store/apps/details?id=com.google.android.apps.playconsole)。這款應用目前已釋出公開 Beta 版，開發者可以透過它檢視應用統計資訊並回復應用評價。

* [Google Cloud 移動應用](https://cloud.google.com/blog/products/management-tools/google-cloud-mobile-app-with-uptime-checks)。這款應用讓您可以透過全新的 Flutter 賦能體驗來監控自己的服務。

* [Google 課堂練習集](https://workspaceupdates.googleblog.com/2023/04/practice-sets-for-google-classroom.html)。這是一款線上新工具，用於建立和分發互動式作業，為學生提供即時反饋和幫助。

我們在 [Flutter Forward 大會上宣佈](https://www.youtube.com/watch?v=JVJF_M9bgj4) 團隊正在使用 Flutter 建構新版本的 Google 課堂移動應用。新版本現已開始在 iOS 上推出，Android 應用更新版本中的新功能也即將推出。此版本的 Google 課堂使用最新的 Flutter 技術，包括我們最新的 Impeller 圖形渲染引擎，可保證介面快速響應、不卡頓。

透過在 Flutter 中重寫 Google 課堂，我們提升了 Google 課堂的效能。該應用的新版本比舊版本的冷啟動時間更短，您可以觀看下面這則對比影片瞭解詳情:

<video controls width="690" height="480" src="https://devrel.andfun.cn/devrel/posts/2023/07/21/469PWC.mp4" poster="https://devrel.andfun.cn/devrel/posts/2023/07/21/ZkFYLQ.png"></video>

我們正在持續投入資源開發 package，以便將您的 Flutter 應用運用到 Google 開發者生態系統的其他方面。這包括對原生廣告的 [Google Ads 支援](https://medium.com/flutter/2023-google-mobile-ads-updates-for-flutter-16b603df9ec9) 進行了重大更新；[新增了 Firebase 對 Windows 平台的支援，並改善了 Firebase 對 Web 的支援](https://github.com/firebase/flutterfire)；同時還新增了對 [更深入的 Android 互操作性](https://io.google/2023/program/2f02692d-9a41-49c0-8786-1a22b7155628/) 的實驗性支援。

由於 Flutter 支援在六大平台 (Android、iOS、Web、Windows、macOS 和 Linux)上使用，**現在已有超過一百萬款已釋出的應用在使用 Flutter**。這些應用來自世界各地，從法國鐵路的火車旅行應用 [SNCF Connect](https://play.google.com/store/apps/details?id=com.vsct.vsc.mobile.horaireetresa.android&hl=en_US&gl=US) 到 Apple App of the Day 獲獎應用 [SO VEGAN](https://apps.apple.com/us/app/so-vegan/id1572826611)；從 [Rive 開發的用於建立動畫圖形的超快桌面應用](https://rive.app/downloads) 到培養親密關係的 [Agapé](https://www.getdailyagape.com/) 移動和平板電腦應用；從採用全新設計的精美 [Global Citizen 應用](https://www.globalcitizen.org/en/content/new-global-citizen-app-impact-activism-every-day/) 到 [最新的 Ubuntu Linux 安裝程式](https://9to5linux.com/first-look-at-ubuntu-23-04s-brand-new-desktop-installer-written-in-flutter)，種類繁多。很高興這些應用讓 Flutter 的價值得到了證明！

## **Impeller 帶來的突破性圖形效能**

我們期待透過 Flutter 為開發者和設計人員提供強大的功能，以實現令人驚歎的圖形體驗。在過去的幾年裡，我們一直在重建圖形渲染架構以提高速度和效能，現在終於取得了成果，我們將這款新引擎命名為 *Impeller*。

> "我們期待透過 Flutter 為開發者和設計人員提供強大的功能，從而實現令人驚歎的圖形體驗。"

自從在 iOS 上引入 Impeller 以來，我們不斷擴大測試範圍並加深與早期採納者的合作，以驗證生產品質並進一步調整效能。隨著現下 Flutter 3.10 的釋出，我們很高興地宣佈: [Impeller 現在將在 iOS 裝置上預設開啟](https://github.com/flutter/flutter/issues/122223)，只需遷移到最新版本的 Flutter 即可大幅提升應用的效能。

與此同時，我們也將注意力轉向為 Android 應用新增預覽支援。正如 iOS 上的 Impeller 使用底層 Metal API 一樣，Android 的 Impeller 實現建立在 [Vulkan](https://www.vulkan.org/) 之上。Vulkan 可提供低階 API 以在底層圖形硬體上進行快速渲染。雖然絕大多數 [現代 Android 裝置](https://developer.android.google.cn/about/dashboards#Vulkan) 都支援 Vulkan，但我們仍將支援較舊裝置機型的向後相容模式。我們將在即將釋出的博文中分享適用於 Android 的 Impeller 的早期預覽版，以及有關 Impeller 技術基礎的更多詳細資訊。

## **與 Web 應用的無縫整合**

正如我們在 [Flutter Forward 大會](https://flutter.cn/posts/flutter-forward-2023-recap) 上介紹的那樣，我們的目標與大多數現有的 Web 框架有所不同。我們為 Web 建構的 Flutter 的實現方式表明，Flutter 明顯 *不是* 為了設計成通用的 Web 框架。已經有很多現有的 Web 框架，如 Angular 和 React，在這一領域表現的非常出色。然而，Flutter 是第一個圍繞 [CanvasKit](https://skia.org/docs/user/modules/canvaskit/) 和 [WebAssembly](https://webassembly.org/) (這兩個技術特別適合用於打造複雜的應用體驗) 等新興 Web 技術進行架構設計的框架。

> "Flutter 是第一個圍繞 CanvasKit 和 WebAssembly 等新興網路技術進行架構設計的框架。"

自最初 [Flutter 推出 Windows 平台支援](https://flutter.cn/posts/announcing-flutter-for-windows) 以來，我們一直在努力提高其效能、可用性和互操作性。

影響感知效能的一個主要因素是 *載入時間*，即從使用者請求頁面到頁面可互動所用的時間間隔。在此版本中，我們取得了飛躍性的進展，這要歸功於在所有瀏覽器上縮減了 CanvasKit 的大小，並對基於 Chromium 的瀏覽器進行了其他方面的最佳化。在 Flutter 3.10 中，CanvasKit 的大小縮減到 1.5MB (之前版本中的大小為 2.7MB)。圖示字型也去除了未使用的字形，在大多數情況下，其大小縮減至原來的百分之一。得益於這些最佳化，我們使用模擬資料線連線將預設計數器應用的載入時間縮短了 42%。

正如 Flutter Forward 大會上預告的那樣，我們現在 [支援在現有 HTML 網頁中嵌入 Flutter 內容](https://flutter-forward-demos.web.app/#/)，而不是讓 Flutter 佔據整個頁面。此外，Flutter 也不需要使用內嵌框架。在 Flutter 3.10 中，我們引入了*元素嵌入*功能。藉助該功能，您可以像在頁面上整合任何其他 CSS 元素一樣整合 Flutter 內容。例如，您可以應用複雜的 CSS 過渡和轉換。若想開始體驗，不妨試試這些使用 [JavaScript](https://github.com/flutter/samples/tree/main/web_embedding/element_embedding_demo) 或將 Flutter 封裝在 [Angular 元件](https://github.com/flutter/samples/tree/main/web_embedding/ng-flutter) 中的範例應用。

Flutter 3.10 繼續專注於發展突破性的圖形效能，還支援 Web 上的 [fragment 著色器](https://docs.flutter.dev/development/ui/advanced/shaders)。自訂著色器可用於提供超出 Flutter SDK 所提供的豐富圖形效果。著色器是一種使用類似於 Dart 的小型語言 (稱為 [GLSL](https://www.khronos.org/opengl/wiki/Core_Language_(GLSL))) 編寫的程式，它會在使用者的 GPU 上執行。如需瞭解更多資訊，請檢視我們 [關於著色器的文件](https://docs.flutter.dev/ui/advanced/shaders) 以及 [新發布的 Codelab](https://codelabs.developers.google.com/codelabs/flutter-next-gen-uis#0)。

## **藉助 WebAssembly 實現對新架構的早期支援**

[WebAssembly](https://webassembly.org/) (通常縮寫為 Wasm) 作為適用於 [現代瀏覽器](https://caniuse.com/wasm) 而無關平台的二進位制指令格式已經日漸成熟。在 Web 應用上，Flutter 一直使用 Wasm 來分發 CanvasKit 執行時，而 Dart 框架和應用程式碼歷來都被編譯為 JavaScript。我們將感興趣的目標從 JavaScript 轉向 Wasm 已經有一段時間了。然而，直到最近，Wasm 仍然缺乏對 Dart 等垃圾回收語言的原生支援。

因此，在過去的一年裡，我們與 WebAssembly 生態系統中的多個團隊合作，將垃圾回收引入標準之中。這是透過名為 [WasmGC](https://github.com/WebAssembly/gc/blob/main/proposals/gc/Overview.md) 的新擴充程式實現的，該擴充程式現在在基於 Chromium 的瀏覽器和 Firefox 瀏覽器中具有近乎穩定的實現。

WebAssembly 具有將原生程式碼的效能帶到 Web 的潛力，這一點讓我們興奮不已。Dart 的 JavaScript 編譯器已在 Google 的數百萬行程式碼中使用，已經生成了執行速度快、最佳化良好的 JavaScript。然而，切換到 Wasm 將為我們提供原生程式碼的效率和 JavaScript 的可移植性，這將進一步提高我們在 Web 上的效能。在一些早期的基準測試中，我們看到執行速度提高到原來的 3 倍，而執行速度的提升會轉化為更豐富的基於 Web 的體驗。此外，藉助 Wasm 我們能夠與用其他語言 (如 Kotlin 和 C++) 編寫的程式碼更輕鬆地整合在一起。

> "WebAssembly 具有將原生程式碼的效能帶到 Web 的潛力，這一點讓我們興奮不已。"

在翹首以盼瀏覽器支援變得更加普遍的同時，我們在預釋出渠道中引入了對將 Flutter 應用編譯為 WebAssembly 的預覽支援。我們希望您能在自己的應用中試用該功能，並儘早與我們分享反饋。如需瞭解詳情，您可以存取 [flutter.dev/wasm](https://flutter.dev/wasm)。

## **對開發者體驗的持續關注**

一方面我們希望透過前面列出的突破性的圖形效能和更豐富的網路支援讓更多使用者滿意，另一方面我們也在這個版本中為提升開發者的速度和效率進行了許多改進。並且 [我們詳細的技術文件記錄了對 Flutter 本身的數百項改進](https://medium.com/flutter/whats-new-in-flutter-3-10-b21db2c38c73)，這將引起目前 Flutter 開發者的極大興趣。

但在這個版本中，對核心開發者體驗最顯著的改進是 **釋出了 [Dart 3](https://dart.cn)，包含在 Flutter 3.10 中**。

Dart 3 完成了為 Dart 生態系統帶來可靠的空安全 (null safety) 的漫長旅程。編寫空安全程式碼可以防止因未經檢查就使用未初始化的值而產生的一整類程式設計 bug。雖然我們從 Dart 2.0 開始就支援空安全程式碼，但現在已經關閉了傳統的 "不安全" 模式。作為一個生態系統，我們已經為此準備了一段時間，排名前 1,000 的 packages 中有 99% 都支援空安全，現在是進行過渡的最佳時機。

> "Dart 3 完成了為 Dart 生態系統帶來可靠的空安全的漫長旅程。"

Dart 3 引入了許多其他新功能，包括記錄、模式和類修飾符，這將提高 Dart 程式碼的可讀性和流暢性。歡迎前往 [Dart 3 部落格](https://medium.com/dartlang/announcing-dart-3-53f065a10635)，瞭解更多資訊和範例。Flutter 本身已經在利用這些新的 Dart 3 功能，因此隨著這些功能的推出，您會看到我們自己的程式碼庫得到了改進。我們相信您會喜歡在自己的程式碼中使用這些功能。

## **SLSA 和軟體供應鏈安全**

在當今時代，我們不得不採取措施來防止對關鍵軟體基礎設施的威脅。因此，除了前面列出的功能外，我們的工程團隊還將安全方面的投入列為 [今年的工作重點](https://medium.com/flutter/flutter-in-2023-strategy-and-roadmap-60efc8d8b0c7)。這項投入涵蓋安全測試、自動化和供應鏈安全。

> "我們的團隊已將安全方面的投入列為工作重點。"

我們將透過開展以下工作，進一步增強企業採用 Flutter 的信心:

* 開源安全基金會 (OpenSSF) [最佳實踐計劃](https://bestpractices.coreinfrastructure.org/en) 是幫助專案遵守安全和漏洞管理最佳實踐的實用基準。很高興地宣佈，我們已經達成了這個計劃合格水平的 [全部合格要求](https://bestpractices.coreinfrastructure.org/en/projects/5631)，並繼續努力朝著符合 [白銀級](https://bestpractices.coreinfrastructure.org/en/projects/5631?criteria_level=1) 和 [黃金級](https://bestpractices.coreinfrastructure.org/en/projects/5631?criteria_level=2) 標準的方向前進。

* Flutter 還在所有 Flutter 關鍵庫上啟用了 [OpenSSF Scorecards](https://securityscorecards.dev/) 和 [Dependabot](https://github.com/dependabot/dependabot-core)。OpenSSF Scorecards 是一種靜態分析工具，用於檢查您的函式庫對最佳實踐的遵守程度，並在這些實踐沒有被遵守時發現問題。Dependabot 可監控專案依賴項中是否存在漏洞，並建立拉取請求以便在必要時更新它們。利用這些工具，Flutter 團隊已經在我們的網站和 Codelab 中發現並解決了 300 多個漏洞。

* Flutter 和 Dart SDK 以及這些 SDK 的釋出工作流程最近已達到 [SLSA L1](https://slsa.dev/spec/v1.0/levels#build-l1) 等級。SLSA (軟體製品的供應鏈等級) 框架可幫助開源專案保持強大的供應鏈安全性。達到 SLSA L1 等級是朝著保護 Flutter 開發者日常使用的工具邁出的重要一步。

* 最後，我們對基礎架構進行了多項安全改進，包括遷移到更安全的建構和測試環境，同時限制對這些環境的存取。此外，我們還改進了 Flutter 框架和引擎工件的日誌記錄和稽核功能，為我們的工件提供了卓越的保護。這些改進讓 Flutter 團隊更深入地瞭解我們產生的工件在建構流程中的處理方式。

## **一個凝聚了開發者全員智慧的開源專案**

此版本中還包含數以千計的其他更新，我們希望這些內容會讓現有的 Flutter 開發者滿意。但值得注意的是，這些貢獻者中有相當一部分是 Google 以外的開發者，貢獻內容包括開發新功能；改進文件；開發軟體套件，將 Flutter 擴充到我們從未想象到的領域；提交可重現的問題報告和功能請求，為我們提供了有關如何改進 Flutter 的新視角。

Flutter 不只是一個 *Google* 專案，而是一個*全員參與*的專案。我們非常感謝社群的多樣性和熱情參與，讓 Flutter 蛻變至此。很高興能與您一起完成這一使命，Flutter 將邁向更加光明的未來！

<video controls width="690" height="480" src="https://devrel.andfun.cn/devrel/posts/2023/07/21/HSILcX.mp4" poster="https://devrel.andfun.cn/devrel/posts/2023/07/21/jUxloC.png"></video>

Betterment 開發者故事: 使用 Flutter 規模化建構理財應用
