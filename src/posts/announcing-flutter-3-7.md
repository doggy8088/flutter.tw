---
title: Flutter 3.7 正式釋出
toc: true
---

文/ Kevin Chisholm，Flutter & Dart TPM

新年伊始，由 Flutter 3.7 正式版來「打頭陣」！我們與整個 Flutter 社群們繼續在 Flutter 3.7 中優化了框架，包括建立自訂選單欄和層疊式選單、更好的國際化工具支援、新的除錯工具以及其他功能和特性等。

![](https://devrel.andfun.cn/devrel/posts/2023/01/36024c875a435.jpg)

新的穩定版裡，我們在持續改進一些特性，例如全域文字選擇、Impeller 渲染速度、DevTools 以及一直以來都在最佳化的效能。讓我們一起來快速探索 Flutter 3.7 的新特性吧！

## 增強對 Material 3 的支援

在 Flutter 3.7 中，以下的 widget 已經進行了 Material 3 的適配：

-  [`Badge`](https://api.flutter.dev/flutter/material/Badge-class.html)
-  [`BottomAppBar`](https://api.flutter.dev/flutter/material/BottomAppBar-class.html)
-  [`Filled`](https://api.flutter.dev/flutter/material/FilledButton-class.html) and [`Filled Tonal`](https://api.flutter.dev/flutter/material/FilledButton/FilledButton.tonal.html) buttons
-  [`SegmentedButton`](https://api.flutter.dev/flutter/material/SegmentedButton-class.html)
-  [`Checkbox`](https://api.flutter.dev/flutter/material/Checkbox-class.html)
-  [`Divider`](https://api.flutter.dev/flutter/material/Divider-class.html)
-  [`Menus`](https://api.flutter.dev/flutter/material/MenuBar-class.html)
-  [`DropdownMenu`](https://api.flutter.dev/flutter/material/DropdownMenu-class.html)
-  [`Drawer`](https://api.flutter.dev/flutter/material/Drawer-class.html) and [`NavigationDrawer`](https://api.flutter.dev/flutter/material/NavigationDrawer-class.html)
-  [`ProgressIndicator`](https://api.flutter.dev/flutter/material/ProgressIndicator-class.html)
-  [`Radio`](https://api.flutter.dev/flutter/material/Radio-class.html) buttons
-  [`Slider`](https://api.flutter.dev/flutter/material/Slider-class.html)
-  [`SnackBar`](https://api.flutter.dev/flutter/material/SnackBar-class.html)
-  [`TabBar`](https://api.flutter.dev/flutter/material/TabBar-class.html)
-  [`TextFields`](https://api.flutter.dev/flutter/material/TextField-class.html) and [`InputDecorator`](https://api.flutter.dev/flutter/material/InputDecorator-class.html)
-  [`Banner`](https://api.flutter.dev/flutter/widgets/Banner-class.html)

你可以直接在應用中的 [`ThemeData`](https://api.flutter.dev/flutter/material/ThemeData-class.html) 裡設定 [`useMaterial3`](https://api.flutter.dev/flutter/material/ThemeData/useMaterial3.html) 來啟用 Material 3。只有在完整的顏色方案下才能展現出 Material 3 最完整的細節，你可以使用新的 [Material 主題建構器](https://m3.material-io.cn/theme-builder#/custom) 產生你的主題配置，也可以透過 Flutter [`ThemeData`](https://api.flutter.dev/flutter/material/ThemeData-class.html) 構造中的 `colorSchemeSeed` (顏色種子) 自動產生一整套的主題：

```dart
MaterialApp(
  theme: ThemeData(
     useMaterial3: true,
    colorSchemeSeed: Colors.green,
  ),
  // …
);
```

若想了解 Flutter 在 Material 3 上的支援進度，你可以在 GitHub 上檢視 [flutter #91605 號議題](https://github.com/flutter/flutter/issues/91605)。

你也可以嘗試 [Material 3 範例](https://flutter-experimental-m3-demo.web.app/#/)，其中展示了所有主題的特性。

![](https://devrel.andfun.cn/devrel/posts/2023/01/edc093ab9959d.gif)

## 選單欄和級聯選單

Flutter 現在可以建立選單欄和級聯選單了。

在 macOS 上，你可以使用 `PlatformMenuBar` widget 來建立選單欄，你的選單欄將由 macOS 系統來渲染，而不是使用 Flutter。

此外，對於所有其他的平台，你可以定義一個 [Material Design 選單](https://m3.material-io.cn/components/menus/overview)，它提供了級聯選單欄 ([`MenuBar`](https://api.flutter.dev/flutter/material/MenuBar-class.html))，或者使用由 UI 介面元素觸發的  ([`MenuAnchor`](https://api.flutter.dev/flutter/material/MenuAnchor-class.html)) 來建立一個級聯選單。這些選單都是完全可自訂的，其中的選單項可以是自訂的 widget，也可以使用新的選單項 widget: ([`MenuItemButton`](https://api.flutter.dev/flutter/material/MenuItemButton-class.html) 和 [`SubmenuButton`](https://api.flutter.dev/flutter/material/SubmenuButton-class.html))。

![](https://devrel.andfun.cn/devrel/posts/2023/01/f15288d8fe378.png)

## Impeller 預覽版

Flutter 團隊很高興能 [在穩定版渠道上](https://github.com/flutter/engine/tree/main/impeller#try-impeller-in-flutter) 為大家帶來 iOS 平台的 [Impeller 渲染引擎](https://github.com/flutter/engine/tree/main/impeller) 預覽。我們認為 Impeller 的效能已經達到甚至超越了大部分現有應用上的 Skia 渲染。在圖像保真方面，Impeller 也已覆蓋了大部分除極端條件以外的應用場景。我們希望能夠在之後的穩定版本中將 Impeller 作為 iOS 平台的預設渲染引擎，如果你在體驗時有任何問題，請繼續 [在 GitHub 上提交 Impeller 的相關反饋](https://github.com/flutter/flutter/issues)。

儘管我們對 iOS 上 Impeller 滿足現有應用的渲染需求有足夠的自信，但仍然有部分 API 需要進行補充。你可以在 [Flutter wiki 文件](https://github.com/flutter/flutter/wiki/Impeller#status) 上看到目前 Impeller 的進度。使用者及開發者在使用時可能會注意到 Impeller 與 Skia 之間的渲染細節區別，這些區別可能是 BUG，當你遇到時請記得 [提交反饋](https://github.com/flutter/flutter/issues) 幫助我們定位並修復它。

Impeller 的進展飛速離不開社群貢獻者的支援。尤其是 [ColdPaleLight](https://github.com/ColdPaleLight)、[guoguo338](https://github.com/guoguo338)、[JsouLiang](https://github.com/JsouLiang) 以及 [magicianA](https://github.com/magicianA) 這些開發者，在此次釋出版本的 Impeller 的 291 次提交中有 31 次 (>12%) 是他們提交的。非常感謝他們的幫助！

我們也在繼續推進 Vulkan 作為 Impeller 的渲染後端 (在一些老的裝置上會降級到 OpenGL)，但是 Android Impeller 尚未準備好進行公開預覽。我們會在未來的釋出中分享更多正在積極進行的 Impeller 開發處理序，包括桌面和 Web 平台的支援。

若你感興趣，可以關注 GitHub 上的 [Impeller 專案板](https://github.com/orgs/flutter/projects/21) 來跟進開發進度。

## iOS 釋出校驗

當你在建構一個釋出版本的 iOS 應用時，Flutter 會為你提供 [專案設定檢查清單](https://flutter.tw/deployment/ios#review-xcode-project-settings) 來確保你的應用已經準備好釋出到 App Store。

現在 `flutter build ipa` 命令會校驗專案的一部分設定，並且在清單中告知你在釋出前進行更改。

![](https://devrel.andfun.cn/devrel/posts/2023/01/e51530946f812.png)

## 開發者工具更新

在本次釋出中，開發工具也帶來了新的特性和體驗最佳化。DevTools 的記憶體除錯工具已經完成了一輪全面的調整。我們帶來了三個新的選項卡：**Profile**、**Trace** 和 **Diff**，它們包含了先前的所有記憶體除錯功能，也添加了更多利於除錯的操作。現在你可以按照類或者記憶體型別對當前的記憶體分配進行分析，可以在執行時分析哪些程式碼呼叫了哪些部分的記憶體，也可以對比兩個不同時間點的記憶體快照之間的差異來了解記憶體使用的細節。

![](https://devrel.andfun.cn/devrel/posts/2023/01/0498c7b2659ab.png)

以上的這些記憶體特性已經在 [文件](https://flutter.tw/development/tools/devtools/memory) 中進行了介紹，若你感興趣可以前往瞭解更多細節。

效能頁面也有一些值得注意的新功能，該頁面現在在頂部新增了 **Frame Analysis** (幀分析) 選項卡，它能夠提供在 Flutter 中詳細追蹤大量消耗的某些幀和操作的一些建議。

![](https://devrel.andfun.cn/devrel/posts/2023/01/b5ab5668cac60.png)

除了以上的新功能，本次更新還有其他的問題修復和最佳化改進，包括檢視器 (Inspector)、網路記錄器的 CPU 記錄器的問題修復。你可以檢視下面的 DevTools 更新日誌瞭解更多細節。

- [Flutter DevTools 2.17.0 發行註記](https://flutter.tw/development/tools/devtools/release-notes/release-notes-2.17.0)
- [Flutter DevTools 2.18.0 發行註記](https://flutter.tw/development/tools/devtools/release-notes/release-notes-2.18.0)
- [Flutter DevTools 2.19.0 發行註記](https://flutter.tw/development/tools/devtools/release-notes/release-notes-2.19.0)
- [Flutter DevTools 2.20.0 發行註記](https://flutter.tw/development/tools/devtools/release-notes/release-notes-2.20.0)

## 自訂上下文選單

從新版本開始，你可以在 Flutter 應用的任意位置建立自訂的上下文選單，也可以自訂內建的上下文選單。

舉例來說，你可以在使用者選中郵件地址時，為文字框預設的選擇選單新增「傳送郵件」的按鈕 ([程式碼地址](https://github.com/flutter/samples/blob/main/experimental/context_menus/lib/email_button_page.dart))。[`contextMenuBuilder`](https://master-api.flutter.dev/flutter/cupertino/CupertinoTextField/contextMenuBuilder.html) 引數也已經新增到現有包含上下文選單的 widget 中。你可以在 `contextMenuBuilder` 中返回任何你想返回的 widget，也包括平台自適應的上下文選單。

![](https://devrel.andfun.cn/devrel/posts/2023/01/ffb979eb8fa7d.gif)

這一新特性也可以用於文字選擇以外的場景。例如，你可以為一個 `Image` widget 的右鍵和長按操作新增「儲存」按鈕 ([程式碼地址](https://github.com/flutter/samples/blob/main/experimental/context_menus/lib/image_page.dart))。你也可以使用 [`ContextMenuController`](https://api.flutter.dev/flutter/widgets/ContextMenuController-class.html) 在應用內的任意位置展示平台預設或者自訂的上下文選單。

![](https://devrel.andfun.cn/devrel/posts/2023/01/0332c5f7dcc77.gif)

若想檢視完整的範例，前往 [Flutter 範例程式碼儲存庫](https://github.com/flutter/samples/tree/main/experimental/context_menus) 瞭解更多。

## CupertinoListSection 和 CupertinoListTile widget

Cupertino 系列 widget 迎來了兩位新成員： [`CupertinoListSection`](https://github.com/flutter/flutter/pull/78732) 和[`CupertinoListTile`](https://github.com/flutter/flutter/pull/78732)，可用於展示 iOS 風格的滾動列表內容。它們是 Cupertino 版本的 `ListView` 和 `ListTile`。

![](https://devrel.andfun.cn/devrel/posts/2023/01/3835e17a8e8ba.png)

![](https://devrel.andfun.cn/devrel/posts/2023/01/708e171743d27.png)

## 滑動最佳化

此次版本釋出中也包含了眾多 [滑動相關的問題](https://github.com/flutter/flutter/issues?page=1&q=is%3Aissue+is%3Aclosed+closed%3A2022-07-11..2022-11-30+label%3A%22f%3A+scrolling%22+reason%3Acompleted) 修復，包括觸控板的互動最佳化以及在滑動元件中文字選擇時的行為。

值得注意的是，macOS 的應用現在可以透過 [新物理滑動特性](https://github.com/flutter/flutter/pull/108298) 來體驗與其有更高匹配度的滑動體驗。

新的 [`AnimatedGrid`](https://github.com/flutter/flutter/pull/112982) 和 `SliverAnimatedGrid` 可以用於為新增和移除的內容展示動畫。

![](https://devrel.andfun.cn/devrel/posts/2023/01/8f8ba42208951.gif)

最後，我們 [修復了](https://github.com/flutter/flutter/pull/108706) 自 Flutter 遷移至健全的空安全以來的一個問題，該問題影響了所有包含 `itemBuilder` 引數的滑動 widget (例如 `ListView`)。在遷移至空安全時，`itemBuilder` 的型別遷移至了 `IndexedWidgetBuilder`，即不允許返回 `null`，而在以前 `null` 可以用來代表列表已經到了底部等。該引數現已修改為 `NullableIndexedWidgetBuilder`。感謝 @rrousselGit 發現並修復了這個問題！

## 國際化工具和文件

Flutter 對國際化的支援已經煥然一新！我們對 `gen-l10n` 進行了重寫以支援下述特性：

- 描述性的語法錯誤
- 巢狀(Nesting)或多個複數、選擇和佔位的訊息內容

![](https://devrel.andfun.cn/devrel/posts/2023/01/93b17a42072f8.png)

更多內容可以瞭解已經更新的 [Flutter 應用裡的國際化](https://flutter.tw/development/accessibility-and-localization/internationalization) 文件。

## 全域的選擇最佳化

`SelectionArea` 現在已支援鍵盤操作。你可以透過鍵盤快捷鍵 `Shift+→` 等快捷鍵進行選擇。

## 後臺 isolate

現在 [平台通道](https://flutter.tw/development/platform-integration/platform-channels) 可以在 [任意 isolate](https://flutter.tw/development/packages-and-plugins/background-processes) 中進行呼叫。先前平台通道只能在主 isolate 中進行呼叫。最佳化後會讓外掛和混合開發呼叫 isolate 和宿主平台程式碼更加簡單。更多內容可以閱讀 [撰寫平台程式碼](https://flutter.tw/development/platform-integration/platform-channels) 文件以及 [介紹後臺 isolate 通道](https://medium.com/flutter/introducing-background-isolate-channels-7a299609cad8) 文章。

## 文字放大鏡

在 Android 和 iOS 上進行文字選擇時會出現的放大鏡現在也會在 Flutter 中出現了。它已經新增至了所有的文字選擇，但是你也可以透過 [`magnifierConfiguration`](https://api.flutter.dev/flutter/material/TextField/magnifierConfiguration.html) 禁用或者自訂。

![](https://devrel.andfun.cn/devrel/posts/2023/01/c375f8ae339e9.gif)

![](https://devrel.andfun.cn/devrel/posts/2023/01/bde08a1b5a13a.gif)

## 外掛程式碼遷移至 Swift

Apple 整將它們的程式碼遷移至 Swift，我們也希望能為開發者建構 Swift 外掛的範例和指導。[quick_actions](https://pub.dev/packages/quick_actions) 已經從 Objective-C 遷移至了 Swift，也可以作為 Swift 外掛的最佳實踐。如果你對幫助 Flutter 遷移第一方外掛至 Swift 感興趣，請參考 [wiki 中的 Swift 遷移部分](https://github.com/flutter/flutter/wiki/Contributing-to-Plugins-and-Packages#swift-migration-for-1p-plugins)。

**給 iOS 開發者準備的資源**

我們新發布了一系列為 iOS 開發者準備的資源，包括：

- [給 SwiftUI 開發者的 Flutter 指南](https://flutter.tw/get-started/flutter-for/swiftui-devs)
- [給 Swift 開發者的 Dart 指南](https://dart.dev/guides/language/coming-from/swift-to-dart)
- [給 Swift 開發者的 Flutter 併發開發指南](https://flutter.tw/resources/dart-swift-concurrency)
- [將 Flutter 新增到現有的 SwiftUI 應用中](https://flutter.tw/development/add-to-app/ios/add-flutter-screen)
- [為 Flutter 建立多渠道](https://flutter.tw/deployment/flavors) (針對 Android 和 iOS)

## 廢棄 Bitcode

從 Xcode 14 開始，watchOS 和 tvOS 的應用不再需要 bitcode，並且 App Store 也不再接收帶 bitcode 的應用提交。因此，Flutter 也移除了 bitcode 的支援。

Bitcode 在 Flutter 應用中預設是關閉的，所以這也不應該會影響太多開發者的專案。但是，如果你曾經為你的專案手動啟用過 bitcode，請儘快在升級到 Xcode 14 後關閉 bitcode。你可以使用 Xcode 開啟 `ios/Runner.xcworkspace` 找到 **Enable Bitcode** 設定為 **No**，混合開發專案需要在宿主專案中禁用。

![](https://devrel.andfun.cn/devrel/posts/2023/01/bda59d271dcb8.png)

## iOS 平臺視圖應用 BackdropFilter

我們為 iOS 原生檢視添加了可以渲染高斯模糊的特性，現在巢狀(Nesting)在 `BackdropFilter` 中的 `UiKitView` 可以正確的渲染高斯模糊了。

![](https://devrel.andfun.cn/devrel/posts/2023/01/991e1cff34251.png)

你可以檢視相應的 [設計文件](https://files.flutter-io.cn/flutter-design-docs/Flutter_iOS_PlatformView_BackdropFilter.pdf) 瞭解更多。

## 記憶體管理

此次釋出的版本對記憶體管理做了一些改進，這些改進的共同作用是減少由 GC 暫停引起的卡頓、減少由於分配速度和後臺 GC 執行緒引起的 CPU 佔用，並且降低記憶體佔用。

例如，我們擴充了現有手動釋放某些 `dart:ui` Dart 物件的本地資源的實踐。先前在 Dart VM 垃圾回收 Dart 物件前，本地資源都將被 Flutter 引擎持有。透過對使用者應用程式和我們的 benchmarks 分析，我們認為這種策略很多時候無法避免不合適的 GC 和過度使用記憶體。因此在此次更新中 Flutter 引擎添加了 API ，用於顯式釋放由 `Vertices`、`Paragraph` 和 `ImageShader` 物件持有的本地資源。

![](https://devrel.andfun.cn/devrel/posts/2023/01/2435f5771a7c2.png)

在我們遷移到此 API 的 Flutter 框架的 benchmarks 中，將 90% 的幀建構時間減少了 30% 以上，終端使用者將體驗到更流暢的動畫和更少的卡頓。

此外，Flutter 引擎 [不再上報](https://github.com/flutter/engine/pull/35473) Dart VM 中的 GPU 圖像的大小。如上所述，當這些圖像資源不再被需要時已由框架手動釋放，如果這時繼續按照 GPU 記憶體大小的 GC 策略上報至 Dart，會導致不必要的堆記憶體壓力並進一步觸發無效的 GC。類似的方法同樣應用到了 Flutter 引擎中，用於回收 `dart:ui` 原生物件的 [隱含記憶體佔用](https://github.com/flutter/engine/pull/35813)。

![](https://devrel.andfun.cn/devrel/posts/2023/01/9635c0a4f4bf6.png)

在我們的測試中，此更改省去了 widget 建立 GPU 常駐圖像建構幀時的同步 GC 工作。

本次版本釋出中，Flutter 引擎在動態更新應用狀態至 Dart VM 方面有所進步。具體來說，Flutter 現在會使用 Dart VM 中 [RAIL 風格](https://web.dev/rail/) 的 [API](https://github.com/dart-lang/sdk/commit/c6a1eb1b61844b2d733f9e2f4c7754f1920325d7)，讓 [路由轉場時渲染延遲更低](https://github.com/flutter/flutter/pull/110600)，即讓堆記憶體在轉場時保持增長而不是進行 GC，避免造成動畫的卡頓。目前這項改動不會帶來太大的效能最佳化，但未來我們會將這項改進拓展到其他方法上，消除由 GC 帶來的卡頓影響。此外，我們還修復了向 Dart VM 報告 Flutter 引擎已經閒置的 [一處邏輯錯誤](https://github.com/flutter/engine/pull/37737)，也減少了 GC 帶來的卡頓。最後，在 Flutter 檢視不再展示時，也會 [通知 Dart VM](https://github.com/flutter/engine/pull/37539) 進行處理，進一步優化了 Flutter 檢視未顯示時的記憶體佔用。

## 放棄對 macOS 10.11 到 10.13 版本的支援

我們在 [Flutter 3.3 釋出的文章](https://mp.weixin.qq.com/s/-AYFnatRYNARGTxhzSY9Lg) 中提到過，Flutter 將不再支援 macOS 的 10.11 和 10.12 版本，自上個版本釋出以來，透過進一步的 [分析發現](https://github.com/flutter/flutter/issues/114445)，放棄對 macOS 10.13 的支援也不會造成太大影響，帶來的收效卻是可以幫助大幅簡化程式碼庫。這意味著，使用 Flutter 3.7 以及後續版本建構的桌面端應用程式將不能再在 macOS 10.11、10.12、10.13 版本中執行，Flutter 對 macOS 的最低10點要求版本提升至 macOS Mojave 10.14。

至此，Flutter 建構的 iOS 和 macOS 應用都已經包含了 Metal 的支援，OpenGL 後端渲染引擎已經從 iOS 和 macOS 嵌入器層被移除，移除後，壓縮後的 Flutter 引擎體積降低了大約 100KB。

## 將 toImageSync 新增至 dart:ui 中

本次版本釋出，將 `Picture.toImageSync` 和 `Scene.toImageSync` 方法直接加入到了 `dart:ui`，類似於 `Picture.toImage` 以及 `Scene.toImage.` 這樣的非同步方法，Picture.toImageSync 會直接返回一個 `Picture` 轉 Image 的一個控制代碼，並在後臺非同步對 Image 進行光柵化。

當 GPU context 可用時，圖像會在 GPU 中常駐，這意味著與 `toImage` 產生的圖像相比它的繪製速度會更快。(toImage 產生的圖像也可以實現 GPU 常駐，但目前還未實現)。

新的 `toImageSync` API 支援的例子：

- 快速捕捉一張昂貴的網格化圖片，以便跨多幀重複使用。
- 應用在圖片的多路過濾器上
- 應用在自訂著色器上

一個例子是，Flutter 框架現已使用這個 API 以最佳化 Android 上的頁面切換動畫的效能，幾乎減少了幀光柵化一半的時間且減少了卡頓，而且在支援這些重新整理率的機器上動畫可以達到 90 / 120 FPS。

## 自訂著色器支援的改進

本次發行版包含了許多關於 Flutter 對自訂著色器片段的最佳化支援。Flutter SDK 現已內建了一個著色器編譯器，能夠將 `pubspec.yaml` 檔案中列出的 GSGL 著色器編譯為目標平台的正確的平台特定對應的格式。此外，自訂著色器能夠在開發階段方便的執行 hot reload。自訂著色器目前已經在 iOS 上對 Skia 以及 Impeller 都支援了。

我們為社群中分享的範例感到印象深刻，期待能夠未來能有更多關於 Flutter 中的自訂著色器的創意。

- [https://twitter.com/reNotANumber/status/1599717360096620544](https://twitter.com/reNotANumber/status/1599717360096620544)
- [https://twitter.com/reNotANumber/status/1599810391625719810](https://twitter.com/reNotANumber/status/1599810391625719810)
- [https://twitter.com/wolfenrain/status/1600242975937687553](https://twitter.com/wolfenrain/status/1600242975937687553)
- [https://twitter.com/iamjideguru/status/1598308434608283650](https://twitter.com/iamjideguru/status/1598308434608283650)
- [https://twitter.com/rxlabz/status/1609975128758026247](https://twitter.com/rxlabz/status/1609975128758026247)
- [https://twitter.com/RealDevOwl/status/1528357506795421698](https://twitter.com/RealDevOwl/status/1528357506795421698)
- [https://twitter.com/TakRutvik/status/1601380047599808513](https://twitter.com/TakRutvik/status/1601380047599808513)
- [https://twitter.com/wolfenrain/status/1600601043477401606](https://twitter.com/wolfenrain/status/1600601043477401606)

請參閱 [文件網站上的文件](https://flutter.tw/development/ui/advanced/shaders) 以及 pub.dev 上的 
[`flutter_shaders`](https://pub.dev/packages/flutter_shaders) package 瞭解更多。

## 字型資源支援熱重載

在過去，將新的字型資源加入到 `pubspec.yaml` 檔案的時候會需要重新建構應用後才能檢視，不像其他資源可以直接熱重載生效，現如今，字型清單檔案的修改 (包括新增新字型) 後，也可以直接熱重載到應用中立刻可見了。

## 減少 iOS 裝置上動畫效果的卡頓

有兩項重要的來自社群成員 luckysmg 的貢獻，幫助減少了 iOS 裝置上動畫效果的卡頓。特別是在 iOS 手勢互動期間在主執行緒上新增一個虛擬的 `CADisplayLink` 以強制設定最大重新整理率。此外，鍵盤動畫也透過 `CADisplayLink` 設定了與 Flutter 引擎裡 animator 相同的重新整理率。由於新加入了這些變化，使用者可以在 120Hz 的 iOS 裝置上感受到更一致和流暢的動畫效果。

## 結語

還是那句話，如果沒有 Flutter 社群中優秀、熱情貢獻者們，Flutter 不會像現在這樣優秀，在我們未來持續進行的這段旅程中，我們希望你可以知道，沒有你們，我們無法做出這樣的優秀成績。感恩每一位貢獻者！

我們的發展勢頭依舊，請期待未來的更新！

## 致謝

感謝來自 CFUG 社群的 Alex、Luke、迷鹿、鑫磊 對本文的翻譯和審校
