---
title: Flutter 1.7 版正式釋出
toc: true
---

文 / Tim Sneath，谷歌 Dart & Flutter 產品組產品經理

今天，我們非常高興地向大家宣佈又一個正式版本的釋出 —— Flutter 1.7，這是繼上次 I/O 時眾多重要功能釋出以來的一次小更新。Flutter 1.7 包含了對 AndroidX 的支援，滿足了 Play 商店近期對應用提出的要求，包含了一些新的和增強過的元件，修復了開發者們提出的 bug 等。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot7-release/announcing-1-7.png){:width="95%"}

如果你已經安裝，並使用預設穩定建構渠道 (stable channel) 的 Flutter，要升級到 1.7 版本，只需要執行 `flutter upgrade` 即可。同時，你可以在 [這個文件裡]({{site.url}}/get-started/install) 檢視如何新安裝 Flutter。

## 支援 AndroidX

[AndroidX](https://developer.android.google.cn/jetpack/androidx)  是 Android 團隊用於在 Jetpack 中開發、測試、打套件和釋出庫以及對其進行版本控制的開源專案，幫助 Android 應用透過最新的元件保持更新而無需犧牲向後相容性。目前 AndroidX 已經穩定，很多 Flutter packages 已經更新和支援它，Flutter 現在可以支援 [建立一個 AndroidX 專案 (new Flutter project with AndroidX)](https://github.com/flutter/flutter/pull/31028) 了，這也減少了與 Android 生態系統整合所你需要做的工作。

當建立 Flutter 專案的時候，你可以透過新增 `--androidx` 來確保產生的專案檔案支援 AndroidX，
更多關於將專案遷移到 AndroidX 的相關資訊，
請存取 [官方文件]({{site.url}}/development/androidx-migration#how-do-i-migrate-my-existing-app-plugin-or-host-editable-module-project-to-androidx) 上的說明。
我們也在積極努力為使用了 AndroidX 和 Android 混合庫的應用帶去 AndroidX 或 Jetifier 的支援，
也會將其作為 add-to-app 的中的一項來支援，接下來的文章中會為大家帶來更多相關的內容。

## 支援 Android App Bundles 和 64 位的 Android 應用

從 2019 年 8 月 1 日開始，為了 target 到 Android Pie 版本，開發者們在 Google Play 上釋出的應用 [必須支援 64 位架構](https://developer.android.google.cn/distribute/best-practices/develop/64-bit)。Flutter 一直都支援產生 64 位的 Android 應用，在 1.7 版本里，我們加入了對 [Android App Bundles](https://developer.android.google.cn/guide/app-bundle) 的支援，開發者們可以在一次提交裡同時 target 到 64 位和 32 位。可透過閱讀 [這篇文件]({{site.url}}/deployment/android) 瞭解到如何分別產生 32 位和 64 位到應用等更多內容。

## 新一批的 widget 和框架的增強功能

我們希望你的應用在任何平臺上都可以看起來平滑自然，我們會持續在平台相關的 widgets 上投入。

如下所示了一個名為 [RangeSlider](https://github.com/flutter/flutter/pull/31681) 的 widget，幫助你在單個滑塊兒上選擇一組值：

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot7-release/rangeslider-widget.gif){:width="95%"}

RangeSlider widget 支援連續或者分散的效果

[更新之後](https://github.com/flutter/flutter/pull/31275) 的 [SnackBar](https://github.com/flutter/flutter/pull/31275) 支援了最新的 Material 規範，文件裡增加了許多 [範例程式碼](https://github.com/flutter/flutter/pull/34679) 。

[Cupertino]({{site.url}}/development/ui/widgets/cupertino) 是用來建構精美的 iOS 體驗的 widgets 庫，我們對其進行了大量的更新。特別提出的是，我們提高了 [CupertinoPicker](https://github.com/flutter/flutter/pull/31464) 和  [CupertinoDateTimePicker](https://github.com/flutter/flutter/pull/31464) widget 的保真度，並增加了對非英語語言本地化的支援。

我們提升了 iOS 上的 [文字選擇和編輯體驗]({{site.url}}/resources/platform-adaptations#text-editing)。此外，我們新增了一個 [範例](https://github.com/flutter/samples/tree/master/platform_design)，關於如何使用同一份程式碼庫，調整不同平台的操作體驗和適配。

文字渲染有了很大的提升，支援了豐富的 [排版樣式](https://api.flutter.dev/flutter/painting/TextStyle/fontFeatures.html)：包括數字表格式對齊、舊式風格數字 (tabular and old-style numbers)、斜線零 (slashed zeros)、樣式集 (stylistic sets)，如這個範例應用截圖所示：

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot7-release/openType-font.png){:width="95%"}

有了 OpenType 的字型支援，你可以用 Flutter 進行復雜的文字排版了

最後，我們加入了對 [遊戲控制器](https://github.com/flutter/flutter/pull/33868) 的支援，會有更好玩的應用出現嗎？

## 初心不忘

整個團隊付出很多努力推出了 Flutter 1.7 正式版，我們解決了開發者們在 GitHub 上提出的 [1250 多個問題](https://github.com/flutter/flutter/issues?q=is%3Aissue+is%3Aclosed+closed%3A2019-04-22..2019-06-21+sort%3Areactions-%2B1-desc) 。

隨著 Flutter 的快速增長，我們看到大家向我們報告了很多新的問題。為了保證專案過程的透明，我們一直在透過 GitHub 執行著這一套錯誤報告系統，但一些相對較小的專案，目前這個流程工作的並不是非常順利。雖然我們在不想關 issue 關閉上有一些新的進展，但是過去幾個月我們的 issue 還是增長的非常明顯。我們也在努力增加這方面的資源配置，可以幫助我們更快的區分 bug，關閉及合併相同的 issue，以及將一些提問引導到 [StackOverflow](https://stackoverflow.com/questions/tagged/flutter)。

在近期的開發者調查裡，很多開發者希望我們在文件和錯誤資訊方面有更持續的投入。一個關鍵部分是能夠在 VSCode 和 Android Studio 裡更結構化的輸出錯誤資訊，我們已經在著手 [這方面的工作](https://github.com/flutter/flutter/pull/34684)。

我們也修復了崩潰率最高的 bug，Flutter 工具的寫許可權問題。Flutter 現在可以更優雅的處理寫許可權導致的崩潰問題，會又一個明晰的指示關於如何解決。

文件方面，我們會持續增加範例程式碼。與此同時，你也可以透過 Flutter create 命令直接建立範例文件，如下是命令：

```flutter create --sample=widgets.Form.1 mysample```

如果透過這種方式建立範例，你將在文件中的 Sample in the App 這一欄看到：

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot7-release/sample-at-docs.png){:width="95%"}

我們也會持續把每週 Flutter widgets 影片嵌入到文件中，在開發者們瀏覽各種 widget 的時候可以得到更全面的理解。

還有一些幕後的設施建設工作正在進行，以便 Flutter app 更好的在 macOS 和 Windows 平台執行。比如支援一些較為重要的平台操作，比如右鍵和一些特別的平臺基建工作（比如 [MSBuild](https://docs.microsoft.com/en-us/visualstudio/msbuild/msbuild?view=vs-2019) 等）。不過，這些非行動平台的支援目前還沒有在穩定建構渠道 (stable channel) 釋出。

最後，當你在蘋果電腦上開發 Flutter 應用的時候，我們支援了 [新的 Xcode 建構系統](https://github.com/flutter/flutter/pull/33684)，這個對新的應用是預設開啟的，也同時方便 [支援現有的應用](https://github.com/flutter/flutter/issues/20685#issuecomment-509731873)。

## 不斷壯大的 Flutter 社群

一如既往，我們非常高興看到 Flutter 在受眾群體和應用場景上繼續持續增長，同時我們也欣賞各種不同的 Flutter 使用方式。自 I/O 以來，Flutter 團隊致力於全球範圍內的各項活動：從中國的 [GMTC](https://gmtc2019.geekbang.org/) 到紐約和墨西哥的交流會和演講等，面對面對大家交流 Flutter 應用開發是一件特別棒的事情。


之前我們提到過 [Reflectly](https://www.forbes.com/sites/heatherfarmbrough/2018/05/01/reflectly-wants-to-be-an-adidas-of-the-mind/#572291294204)，它是一個丹麥的公司，他們在 iOS 和 Android 平臺開發了非常有吸引力的應用程式。他們的應用程式被美國 iPhone 應用商店評為當日最佳應用。這也證明了 Flutter 的真正潛力遠遠超過實現體驗流暢的應用（同時可以幫助開發者獲得成功）。

<iframe src="//player.bilibili.com/player.html?aid=56686514&cid=99031924&page=1&autoplay=false" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="560" height="315"> </iframe>

在柏林的  [WeAreDevelopers](https://events.wearedevelopers.com/) 大會中，[BMW 釋出了他們基於 Flutter 的應用](https://youtu.be/80pRyn7fZRk?t=1234)，目前已經在開發中。下面這段描述來自 Guy Duncan，他是 BMW 集團互聯公司的 CTO：

> 透過結合 Dart 和 Flutter，我們實現了第一個真正跨平臺的移動工具套件；我們認為它打破了原有的遊戲規則，可以平衡數字互動和物聯網的功能特性。
> 透過使用主流的工具鏈、自動化工具和現代化的程式設計模式，我們可以最佳化迴圈時延、安全性、商業應用特性的推送成本。

除了應用程式，整個開源社群所涉及的眾多 [資源](https://flutterx.com/)，[外掛](https://pub.dev/flutter)， [Flutter 社群活動](https://flutterevents.com/) 和 [Meetup](https://www.meetup.com/topics/flutter/) 也使得 Flutter 變得格外生機勃勃。
我們會持續關注大家基於 Flutter 所實現的各種有趣的應用，同時也非常榮幸和大家一起分享其中的樂趣。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-1dot7-release/flutter-bag.jpeg){:width="95%"}

Photo credit: [@damian2048](https://twitter.com/damian2048)
