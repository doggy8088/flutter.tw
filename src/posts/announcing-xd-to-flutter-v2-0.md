---
title: XD to Flutter 2.0 現已釋出！
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/05/JEUpoL.jpg)

Flutter 是 Google 的開源 UI 工具套件。利用它，只需一套程式碼庫，就能開發出適合移動裝置、桌面裝置、嵌入式裝置以及 web 等多個平台的精美應用。過去幾年，對於想要打造多平臺應用的開發者來說，Flutter 已成為他們的首選。但設計師們需要的是一個視覺工具來建立原型和建構 Flutter UI，而不是精雕細琢 Dart 原始碼，於是 [XD to Flutter](https://github.com/AdobeXD/xd-to-flutter-plugin) 應運而生！

此外掛的首個預覽版推出已近一年。這期間，我們一直在透過一些小更新對它進行最佳化和改進；如今，繼去年夏天 1.0 版的重大發布之後，我們推出了此外掛的 2.0 版以配合 Flutter 2 的釋出。

## **等等，什麼是 XD to Flutter？**

顧名思義，[XD to Flutter](https://youtu.be/raG7NjM0p0k) 外掛是一個功能強大、易於使用的工具，可將您的 [Adobe XD](https://www.adobe.com/products/xd.html) 設計匯出為簡潔、有效的 Flutter 程式碼。您可以從自己的設計中複製特定視覺元素的程式碼，匯出各種可再利用的 Widgets，甚至可以將一些檢視整個匯出。

這意味著，利用 XD to Flutter 外掛，您只需點選一個按鈕，即可讓您的設計在任意裝置上執行。此外掛雖不能為您完成整個應用的編碼，但可讓您有個極好的開端。

XD to Flutter 由 [gskinner](https://gskinner.com/) 與 Adobe 聯合開發，作為 Adobe XD 本身的外掛釋出，因此，您可將它用於您正在打造的任何現有的 Adobe XD 設計。

## **太棒了！有哪些新功能？**

最初發布的 XD to Flutter 版本非常有助於輸出設計中所有不同的視覺元素，比如向量圖形、圖片、富文字、背景模糊效果、混合模式、陰影及其他類似元素，只可惜輸出結果是靜態的，且不夠靈活。

儘管抓取圖示或文字樣式不費什麼功夫，但我們仍希望它能發揮更大功效。XD 不僅可讓設計師建立動態 UI，還提供有自適應佈局、可滾動區域、堆疊及網格之類別的工具；我們希望此外掛能支援上述每一種功能，而在 2.0 版中，我們取得了許多進展。

**響應式調整尺寸**

XD to Flutter 支援 XD 的響應式佈局功能，您可將元素 "固定" 在其所屬父元素內，並精確控制其調整尺寸的方式。

![Adobe XD 中的響應式設計](https://devrel.andfun.cn/devrel/posts/2021/05/2oeEuB.png)

> Adobe XD 中的響應式設計

![Flutter 中的響應式設計](https://devrel.andfun.cn/devrel/posts/2021/05/T0Kwo6.gif)

> Flutter 中的響應式設計

Flutter 使用 [adobe_xd](https://pub.dev/packages/adobe_xd) 開源軟體套件中的自訂 Pinned 佈局 Widget 實現了這一功能，開發者可直接在其專案中使用。

![Pinned Widget 程式碼範例](https://devrel.andfun.cn/devrel/posts/2021/05/nzmZOr.jpg)

> Pinned Widget 程式碼範例

**堆疊和滾動組**

"堆疊" 和 "滾動組" 讓開發者可以運用一些新的方式來在 Adobe XD 中動態佈局螢幕內容。透過 XD 中的 "堆疊"，您可以將一堆形形色色的元素以橫向或縱向列表排列，元素之間可以有不同的間距；相較於與其同名的 `Stack` Widget，"堆疊" 更類似於 Flutter 中的 `Flex` Widget。

如您所料，利用 "滾動組"，可直接在您的設計內定義一個區域來橫向或縱向滾動一大組內容。

XD to Flutter 2.0 版對這些功能都支援，可將它們轉換成常見的 Flutter Widget (`Column`、`Row` 以及 `SingleChildScrollView`)。您甚至可以將某個堆疊放入一個滾動組內，從而輕鬆建立一個滾動項列表。

![XD (左) 和 Flutter (右) 中的堆疊和滾動組](https://devrel.andfun.cn/devrel/posts/2021/05/GRBLhW.gif)

> XD (左) 和 Flutter (右) 中的堆疊和滾動組

**內邊距和背景元素**

另一項新功能是背景元素，也就是說，您可以指定一個視覺元素作為另一組元素的背景。背景元素可以和內邊距配對使用，以界定背景的邊緣與其內容之間的距離。

Flutter 匯出工具使用 `Stack` Widget 將背景元素分層放置在內容之後，而後者則放置在一個 `Padding` Widget 內。

![XD (左) 和 Flutter 中的內邊距和背景](https://devrel.andfun.cn/devrel/posts/2021/05/RIaeJC.gif)

> XD (左) 和 Flutter 中的內邊距和背景

**Flutter 2 和空安全機制**

正是有了上述佈局功能，才能成就響應式更強的 UI，也增強了 Flutter 2 對桌面裝置和 web 等平台的支援。

Flutter 2 還引入了健全的空安全機制 ，這一語言特性可幫助開發者先行捕捉不可為空的變數卻為空的問題，避免其在應用中引發問題。XD to Flutter 2.0 版包含一個新設定 "Export Null Safe Code" (匯出空安全程式碼)；匯出時選中該設定，可確保產生的程式碼未來可用。

![](https://devrel.andfun.cn/devrel/posts/2021/05/mEFck0.png)

> "Export Null Safe Code" 設定和輸出

**聽起來不錯啊！怎樣開始使用？**

無論您是要使用它來複制某個構思精妙的漸變效果的程式碼，還是要匯出完全響應式、引數化、互動式的 Widget，都非常簡單，不過是加入到成千上萬已經在使用 XD to Flutter 外掛的創意專業人才大軍而已。

您只需從 Adobe XD 的 "Plugin" (外掛) 選單選擇 "Browse Plugins…" (瀏覽外掛)，然後搜尋 "Flutter" (奇怪的是，搜尋 "XD to Flutter" 不起作用)，或前往 [adobe.com/go/xd_to_flutter](https://adobe.com/go/xd_to_flutter)，即可安裝此外掛。

![](https://devrel.andfun.cn/devrel/posts/2021/05/r9kl7p.png)

安裝好後，從外掛面板中開啟 XD to Flutter 面板，點按 "Need help" (需要幫助？) 連結，可查閱 [plugin documentation](https://github.com/AdobeXD/xd-to-flutter-plugin/blob/master/README.md) (外掛幫助文件)。

我們一直專注於打造可在任何平臺上實際執行的精美應用，Flutter 2 就是我們在這一框架上邁出的可喜一步。[gskinner](https://gskinner.com/) 的各團隊非常開心能夠與 Adobe 和 Google 合作，共同確保 XD to Flutter 進一步簡化將設計轉換成可執行產品的過程。
