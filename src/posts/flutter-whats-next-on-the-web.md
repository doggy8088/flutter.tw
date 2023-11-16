---
title: Flutter Web 近期的重要更新
toc: true
---

_我們的釋出重點優先關注在效能、開發者體驗以及 Web 整合上_

2021 年 3 月 [Flutter Web 支援已進入穩定版]({{site.url}}/posts/flutter-web-support-hits-the-stable-milestone)，那麼，下一步是什麼？

在我們對使用者調研的研究中發現，有超過 10％ 的 Flutter 專案目標是釋出到 Web 平台。因此，目前我們會專注於提升第一個 Web 穩定版的品質，並使更多開發者能夠將 Flutter Web 應用投入到生產環境。

根據 Flutter 每個季度的調查結果和我們透過 Issue、開發者面談、社交媒體渠道中聽取到的資訊，我們確立了這些優先事項。我們剛剛收到了第三季度的調查結果，大家的反饋與我們的計劃不謀而合。

本文提供了詳細的路線圖和每個優先事項對應的工作計劃。有些特性可能會跨越幾個季度的時間來完成，使用放大鏡符號 🔍 標識的特性代表它們仍需進一步調查之後才能給出一個解決方案。

讓 Flutter 應用在 Web 上表現得自然是一件很重要的事，這包括諸如滾動行為、文字功能、閃屏、超連結、搜尋引擎最佳化和其他 Web 應用特有的功能。

## RTL 文字

此前，Flutter 沒有在 Web 上對從右到左 (RTL) 的語言 (如阿拉伯語和希伯來語) 提供完整的支援。雖然框架本身支援 RTL 文字，但 Web 引擎忽略了 LTR 和 RTL 之間的區別，從而產生了未定義的行為。

近期釋出的 [Flutter 2.5 穩定版]({{site.url}}/posts/whats-new-in-flutter-2-5) 增加了對 RTL 的基礎支援，Flutter Web 應用已經支援了 RTL 語言的所有主要場景。大部分與基礎支援相關的問題已經得到了解決，並且我們正在計劃修復剩餘的問題。

- [對文字編輯實現 RTL 支援](https://github.com/flutter/flutter/issues/32239) (已修復)
- [Web 上的 RTL 文字渲染故障](https://github.com/flutter/flutter/issues/69396) (已修復)
- [帶有 TextDirection.rtl 的 TextField 在 Web 上游標錯位](https://github.com/flutter/flutter/issues/78550) (計劃)
- [在 RTL 應用程式中，RichText 中的 InlineSpans 重疊了](https://github.com/flutter/flutter/issues/82136) (已修復)

## 滾動行為

雖然 Flutter 2 對滾動做出了 [一些改進](https://github.com/flutter/flutter/pulls?q=is%3Apr+is%3Aclosed+is%3Amerged+label%3A%22f%3A+scrolling%22)，來支援桌面瀏覽器上的滾動行為 (如捲軸支援)，但在瀏覽器或執行 Web 應用的作業系統上，滾動行為依然在某些情況下沒有達到預期。

雖然其中一些行為依賴於 Flutter 的桌面端支援，但我們計劃在這個路線圖中，解決一些物理滾動屬性和捲軸的問題，問題如下所示。我們還計劃展開對觸控板支援的研究。

- [PageScrollPhysics 出現奇怪的行為](https://github.com/flutter/flutter/issues/35687)
- [在 (無限) 列表 Widget 中沒有捲軸](https://github.com/flutter/flutter/issues/41434)
- [Mac 桌面的滾動物理應該保持範圍](https://github.com/flutter/flutter/issues/85579)
- [讓捲軸避免阻塞 Sliver 和媒體查詢的內邊距](https://github.com/flutter/flutter/issues/13253)
- [MaterialScrollBehavior.buildScrollbar 需要更新](https://github.com/flutter/flutter/issues/87739)

🔍 研究對 [觸控板的支援](https://github.com/flutter/flutter/issues/23604)

## 應用載入 API

有些 Web 應用傾向於在載入或進行一些自訂體驗的時候，透過閃屏頁、載入指示器或落地頁來實現這個體驗。目前在 Flutter Web 應用內並沒有顯示正在載入或實現自訂體驗的簡單方法，並且當用於渲染的 CanvasKit 體積較大時，這將會變成比較棘手的問題。

我們正專注於為應用啟動週期提供一個顯式 API，可以用來預裝應用，控制應用的載入週期，並建立閃屏頁或載入指示器。

- [增加在 Web 上對閃屏頁的支援](https://github.com/flutter/flutter/issues/48468)

無障礙是我們的首要任務之一；我們旨在為您提供必需的工具以建構可存取的 Web 應用，且應用在最常見的螢幕閱讀器上執行良好。

Flutter 2.2 對無障礙支援進行了極大的改進。從那時開始，我們就聽到了一些終端使用者的擔憂，他們試圖用螢幕閱讀器 (如 [JAWS](https://www.freedomscientific.com/products/software/jaws/)) 來瀏覽他們的 Web 應用。

在該路線圖中，我們將專注於桌面瀏覽器 [支援的螢幕閱讀器](https://flutter.cn/docs/development/accessibility-and-localization/accessibility#screen-readers) 的問題，同時，我們還將繼續研究如何提高我們整體無障礙支援。

- [使用Enter鍵時未能觸發按鈕的點選](https://github.com/flutter/flutter/issues/83812)
- [方向鍵和 B 鍵不更新螢幕閱讀器的焦點](https://github.com/flutter/flutter/issues/83809)

我們將始終優先考慮效能，以改善 Flutter Web 應用的使用者體驗。目前我們的主要目標是改善滾動卡頓，並加快 Web 應用的初始載入速度。

## 滾動時的卡頓

我們近期的季度調查資料顯示，滾動卡頓是首要的效能問題報告之一。我們的目標是無論在手機上使用手勢，還是在桌面上使用滑鼠 / 鍵盤，都確保 Flutter Web 應用能流暢滾動，但這也取決於 Web 應用期望使用者滾動內容的型別和數量。

在未來幾個月裡，我們將專注於改善由於圖像解碼造成的卡頓，也同時將繼續研究滾動的效能問題，以找到我們可以改善的其他使用案例。

- [將圖像解碼轉移到 Web Worker](https://github.com/flutter/flutter/issues/63397)
- 降低 [在 CanvasKit 渲染引擎中使平臺視圖的成本](https://github.com/flutter/flutter/issues/71884)

## 捆綁 CanvasKit (離線支援)

目前，用 CanvasKit 渲染的 Flutter Web 應用需要額外的手動步驟，才能作為漸進式 Web 應用 (PWA) 離線工作。為了在離線模式下完全作為 PWA 工作，並確保應用符合嚴格的內容安全策略，我們需要捆綁 CanvasKit 和備選字型。

我們將首先捆綁 CanvasKit，然後捆綁字型，並新增必要的工具以啟用離線模式。

- [CanvasKit 的後端不應該依賴於不穩定的谷歌字型](https://github.com/flutter/flutter/issues/85793)
- [支援捆綁的 CanvasKit 而不是透過 CDN](https://github.com/flutter/flutter/issues/70101)

## CanvasKit 的下載大小

CanvasKit 的效能優於基於 DOM 的方法，因此它是我們在桌面瀏覽器上的預設渲染器。然而，下載應用所需的時間會影響初始載入效能 (以及在 Web 上執行的 Flutter 應用的 Lighthouse 得分)。

在該路線圖中，我們將研究如何減少 CanvasKit 的下載大小，以努力提高初始載入效能。我們希望確保終端使用者的裝置或瀏覽器不會處理大量的有效載荷。

- 🔍 [改善 CanvasKit 的下載大小](https://github.com/flutter/flutter/issues/89616)
- 🔍 [找到一個大小合適的表情符號備選字型](https://github.com/flutter/flutter/issues/76248)

提升瀏覽器程式碼整合的能力，有利於 Web 平台優勢的發揮。Flutter Web 應用有兩種方式與 HTML 整合。1) 在 Flutter Web 應用中使用 HTML 平臺視圖，或 2) 將 Flutter 作為內容集嵌入現有的 Web 應用 (類似於 Web 的附加應用)。目前前者已經可用且需要改進，而後者將是一個新功能，需要進一步設計和開發。

## 用自訂元素嵌入 (add2app)

今天，將 Flutter Web 應用嵌入現有網站 / Web 應用的唯一方法是透過 iframe。雖然這在某些場景下是可行的，但對於那些慢慢將其 Web 應用遷移到 Flutter 的開發者來說，這並不是一個理想解決方案。

在該路線圖中，我們將研究並設計一款客製的解決方案，使你能夠嵌入 Flutter Web 應用，類似於在移動端 add2app 的場景。

🔍 [渲染自訂元素內的應用](https://github.com/flutter/flutter/issues/32329)

Flutter 的生態系統包括了用於開發的功能，但目前仍然缺乏 Web 的功能支援，如外掛、除錯、熱重載等。為了讓你在 Web 端有一個良好的開發者體驗，我們將繼續縮減移動端和 Web 端之間的差距。

## 相機外掛

自最初的 Web 穩定版本釋出以來，相機外掛一直是呼聲最高的外掛之一；許多人發現，在將 Flutter 移動應用引入 Web 時，同步差距是一個主要的問題。

在 [Flutter 2.5 穩定版]({{site.url}}/posts/whats-new-in-flutter-2-5) ，我們提供了這個[外掛](https://pub.dev/packages/camera_web)的早期版本，可以初始化相機，顯示相機預覽，並拍攝照片。我們收到反饋後將會對這個外掛進行改進。

- 為 [Web 增加相機支援](https://github.com/flutter/flutter/issues/45297) (已修復，一些 PR 待定)。

以上是對目前我們在 Web 上路線圖的概述，[GitHub 問題列表](https://github.com/flutter/flutter/issues?q=is%3Aopen+is%3Aissue+label%3Aplatform-web+) 依然是我們正在處理的問題來源。
我們可能會根據瞭解到的情況和開發者們的反饋來增加、延長或推遲功能。

我們非常重視開發者們的反饋，並感謝大家一如既往的支援。

_感謝 flutter.cn 社群成員 (@AlexV525、@Vadaski、@MeandNi) 以及 Lynn 對本文的審校和貢獻。_
