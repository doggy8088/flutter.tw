---
title: Dart 2.6 正式釋出, 新增利器 dart2native：將 Dart 編譯為自包含的本地可執行檔案
toc: true
---

*作者: Michael Thomsen, Dart & Flutter Product Manager, Google*

Dart 提供了 [豐富多樣的編譯器](https://dart.dev/platforms)，全方位助力開發者為移動裝置和 web 平台最佳化生產程式碼。靈活的編譯器技術讓我們的框架合作伙伴可以針對不同的目標平台，建構各種型別的應用，例如: [Android 和 iOS](https://flutter.dev/docs) 平臺上的 Flutter 應用、[Web 端](https://flutter.dev/web) 和 [桌面端](https://github.com/flutter/flutter/wiki/Desktop-shells) Flutter 應用、[Web 端的 AngularDart 應用](https://angulardart.dev) 以及 [嵌入式裝置](https://mp.weixin.qq.com/s/xVmilQeiveA8XZNU0g668Q) 上的 Google 助手。

今天，我們正式推出 **dart2native**。作為現有編譯器集合的一個擴充，dart2native 可以將 Dart 程式編譯為含有預編譯機器碼的自包含可執行檔案。在它的協助下，您可以使用 Dart 在 **macOS**, **Windows** 或 **Linux** 上建立命令列工具。下方的功能宣傳圖就是 [使用 dart2native 來實現](https://gist.github.com/mit-mit/faec2bfc1d1cef7cd09df917e531c5c0) 的。

![](https://devrel.andfun.cn/devrel/posts/2021/05/H1ZCVn.gif)

## **Dart Native 及 dart2native 編譯器**

Dart 從數年前就開始支援 AOT 執行前編譯，透過 AOT 編譯器，開發者可以提前將 Dart 程式碼編譯為本地機器碼。幾年發展下來，[Dart Native](https://dart.dev/platforms) 技術也日趨成熟。不過，此前這項功能的應用範圍比較侷限，我們僅透過 [Flutter](https://flutter.dev) 向 iOS 和 Android 移動裝置投放了該功能。

引入 dart2native 後，我們將本地編譯支援擴充至 macOS、Windows 和 Linux 三款傳統桌面作業系統。利用 dart2native 建立的可執行檔案屬於自包含檔案，因此在未安裝 Dart SDK 的機器上亦能執行。由於可執行檔案之前已經透過 Dart 的 AOT 編譯器進行過處理，因此幾毫秒後就會開始執行。在編譯為原生代碼的過程中，dart2native 與其它 Dart 編譯器和執行時工具一樣，也可以使用豐富且一致的 [Dart 核心函式庫](https://dart.dev/guides/libraries)。

許多客戶要求我們為桌面作業系統提供 AOT 預編譯支援 (Dart AOT 問題專區 [排名第 6 的功能請求](https://github.com/dart-lang/sdk/issues/36915))，所以，我們很高興終於將這項功能帶到您身邊。

> 如果您之前使用過 dart2aot，那麼，升級至 Dart SDK 2.6 後，您將開始使用 dart2native，它的功能比 dart2aot 更加強大。

## **使用 dart2native 建構命令列應用**

如果您想建構和部署 [基於 Dart 的命令列應用](https://dart.dev/tutorials/server/cmdline)，那麼 dart2native 絕對是您的不二選擇。此類應用通常會使用到 [dart:io](https://api.dart.dev/stable/2.6.1/dart-io/dart-io-library.html) (基礎 I/O), package:[http](https://pub.dev/packages/http) (網路) 以及 package:[args](https://pub.dev/packages/args) (引數解析) 這三個庫。下面讓我們以 "hello, world" (你好，世界) 應用為例，回顧一下如何把應用編譯成可執行檔案:

原始碼 hello.dart

```
main() {
 print(‘Hello Dart developers’);
}
```

編譯 hello.dart 至 hello 可執行檔案:

```
$ dart2native src/hello.dart -o hello
Generated: /Users/mit/hello
```

執行 hello 並測量知執行時間:

```
$ time ./hello
Hello Dart developers
real 0m0.049s
user 0m0.018s
sys 0m0.020s
```

**請注意:** 在短短 49 毫秒內，這條命令就完成了開始、列印到標準輸入、退出整個流程！

之前，我們已經看到有不少 Dart 開發者小試牛刀，嘗試利用 dart2native 開發命令列工具:

* 來自 [SASS](https://sass-lang.com/) (一款深受開發者喜愛的 CSS 擴充工具) 團隊的 Natalie [在 Github 上留言說](https://github.com/dart-lang/sdk/issues/32894#issuecomment-513975562)，自從團隊轉用 dart2native 來編譯基於 Dart 的 SASS 實現後，該實現的效能大幅提高，即使與 LibSass 這款基於 C++ 的實現相比也毫不遜色。
* 來自 Dart DevRel 團隊的 Filip 使用 dart2native 重編譯了自己的網站連結檢查工具 [linkchecker](https://github.com/filiph/linkcheck/)，二次編譯後，小型網站的檢查速度 [提升了整整 27 倍](https://github.com/filiph/linkcheck/issues/7#issuecomment-496308288)。

## **透過 dart:ffi 與 C 程式碼互操作**

原生應用經常會用到由存取由作業系統提供的原生功能。此類系統 API 通常由基於 C 語言的原生庫提供，而 Dart 則允許您透過 dart:ffi (外部函式介面) 與這些庫實現互操作。dart:ffi 是我們新推出的 C 語言互操作機制，首個技術預覽版本已隨 [Dart 2.5](https://mp.weixin.qq.com/s?__biz=MzAwODY4OTk2Mg==&mid=2652050716&idx=1&sn=8f417c47868404bf55f9878f69f8c7e4&scene=21#wechat_redirect) 一同釋出。dart2native 編譯器與 dart:ffi 相容，因此您可以直接在本地建立和編譯需要用到 dart:ffi 的應用。

我們團隊的一名成員最近利用 dart:ffi 建立了一個 [dart_console](https://pub.dev/packages/dart_console) 庫，專門用於控制檯應用的開發工作。該庫涵蓋多種功能，如獲取視窗尺寸、讀取和設定游標位置、顏色管理、讀取鍵盤輸入和控制序列等。出色的 dart:ffi 使用能力讓 Dart 成為一款強大的控制檯應用開發語言。

**不到 500 行 Dart 程式碼就能寫出一個 7MB 大小的程式碼編輯器: kilo**

我們利用 Dart 核心函式庫、dart:ffi 以及 dart_console 庫開發出了幾個十分有趣的控制檯應用。我們 [大約用了 500 行 Dart 程式碼](https://github.com/timsneath/dart_console/blob/master/example/kilo.dart) 編寫了一個名為 Kilo 的控制檯文字編輯器，並將完整版示範封裝至 dart_console 包內。Kilo 的命名源於它的前身 [kilo.c](https://github.com/antirez/kilo/blob/master/kilo.c) —— 一款由不到 1,000 行 C 程式碼實現的簡易文字編輯器。

新推出的 dart2native 編譯器讓打包工作變得十分簡單，我們最終得到了一個 7MB 大小的自包含程式碼編輯器。請檢視下方示範動畫，瞭解 Kilo 的編譯過程，以及編譯後的 Kilo 是如何透過編輯自己的原始碼來修復錯誤的。

![](https://devrel.andfun.cn/devrel/posts/2021/05/frVHPl.gif)

正在編輯自己的原始碼的 Kilo 編輯器 (該編輯器使用 Dart 編寫，並透過 dart2native 編譯為可執行檔案)

## **使用 dart2native 建構服務**

微服務是 dart2native 編譯器另一個潛在用途，比如說: 為前端 Flutter 應用提供支援的後臺服務。近年來，[無伺服器計算](https://en.wikipedia.org/wiki/Serverless_computing) 的應用趨勢持續攀升，越來越多的使用者開始使用執行在無伺服器計算上的服務。這些服務完全交由供應商託管，支援自動擴縮，基礎架構可根據流量從零 (停止執行時) 開始擴容，反向縮容亦可。此外，由於無伺服器平台僅在程式碼執行期間收費，因此可以為開發者大幅削減成本。目前，Google Cloud 已透過 [Cloud Run](https://cloud.google.com/run/) 推出無伺服器計算解決方案。

對於無伺服器後臺而言，快速的服務啟動速度至關重要。過去，我們一般透過 JIT (即時) 編譯器來執行基於 Dart 的服務，但是基於 JIT 的執行有一個明顯的缺點: 程式碼必須先完成編譯和預熱兩個步驟才能開始執行，所以延遲現象十分嚴重。透過把服務程式碼提前編譯為原生代碼，您不但可以避免延遲問題，而且還能夠立即開始執行程式碼。此外，使用原生代碼建立的 Dart 服務屬於自包含應用，所佔用的磁碟資源也比較小，因此對執行容器的體積要求也會大幅降低。Dart 開發者 Paul Mundt 在 [《初試 Dart 微服務》](https://itnext.io/experiments-with-dart-microservices-fa117aa408c7) 這篇文章中，談到了對 dart2native 的一些心得體會: 之前使用 JIT 編譯器時，Docker 映象檔案的體積為 220MB，轉用原生代碼後，檔案體積縮減至 20MB，降幅高達 91%！更多技術細節，請檢視官方文件 [《伺服器端應用》](https://dart.dev/tutorials/server/httpserver) 和 [《包》](https://dart.dev/server/libraries#server-packages)。

## **如何獲取**

您可從 2.6 及以上版本的 Dart SDK 中獲取 dart2native 編譯器。請前往 https://dart.dev/get-dart 下載最新版本的 Dart SDK。安裝完畢後，您可在 bin/ directory 和 PATH 內找到新的編譯器。請前往 Dart.dev 獲取 [更多官方文件](https://dart.dev/tools/dart2native)。

透過 Flutter 獲取 Dart SDK 的開發者請注意: 當前 Flutter 版本對 dart2native 的支援並不完整。在穩定版 Flutter 提供 Dart 2.6 之前，我們建議您前往 [dart.dev/get-dart](https://dart.dev/get-dart) 下載 Dart 2.6 SDK。

## **已知限制**

初始版本的 dart2native 編譯器包含以下已知限制。請前往我們在 Github 上的問題追蹤頁面，為您關注的問題投票，以便告知我們哪些問題對您最為重要。

* 不提供交叉編譯支援 ([Github 問題 28617](https://github.com/dart-lang/sdk/issues/28617)): dart2native 編譯器每次只能為一種執行系統建立機器碼，因此，如需同時為 macOS、Windows 和 Linux 建立可執行檔案，請您分別執行 3 次編譯器。或者，您也可以與支援 3 種作業系統的持續整合 (Continuous Integration) 供應商接洽合作，共同解決這類問題。
* 不提供簽名支援 ([Github 問題 39106](https://github.com/dart-lang/sdk/issues/39106)): 產生的可執行檔案的格式與 codesign 和 signtool 等標準簽名工具不相容。
* 不支援 dart:mirrors 和 dart:developer (請參閱 [Dart 核心函式庫](https://dart.dev/guides/libraries))。

## **Dart 2.6 中的其它變更**

除了 dart2native 編譯器之外，Dart SDK 2.6 另外還引入了多項變更。

作為我們新推出的 [C 語言互操作機制](https://dart.dev/guides/libraries/c-interop)，dart:ffi 的首個技術預覽版本已隨 Dart 2.5 一同釋出。Dart 2.6 推出了全新的 dart:ffi，內含多箇中斷性 (Breaking) API 變更，使得 API 的易用性大幅提升，此外，新版本還提供了更多的型別安全，記憶體存取也更為便利。請閱讀 [《Dart 2.6 變更日誌》](https://github.com/dart-lang/sdk/blob/master/CHANGELOG.md#foreign-function-interface-dartffi)，進一步瞭解詳情。引入這些變更後，dart:ffi 已順利迭代至 Beta 版本，我們預計今後 API 的變更頻率將逐步放緩，整體穩定性也會有所提高。歡迎大家繼續透過 [Dart 問題追蹤器](https://github.com/dart-lang/sdk/issues)，向我們提交反饋。

此外，Dart 2.6 還新增了一款超棒的語言特性: 擴充方法 (Extention methods)。此項功能尚處於預覽階段，在效能最佳化及工具支援方面仍有不足，我們目前正在收尾相關工作，希望能在下個版本的 Dart SDK 中正式推出該功能，屆時將為大家帶去更為詳盡的功能介紹。如果您有興趣瞭解它背後的設計理念，請閱讀 [《Dart 擴充方法的基礎知識》](https://medium.com/dartlang/extension-methods-2d466cd8b308)。

## **下一步**

即刻下載 Dart 2.6 SDK ([https://dart.dev/get-dart](https://dart.dev/get-dart)), 使用 dart2native 建構精彩應用，並向我們提交使用反饋。
