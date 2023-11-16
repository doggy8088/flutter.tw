---
title: Flutter 2.10 更新詳解
toc: true
---

Flutter 2.10 版已正式釋出！雖然⾃ [上次穩定版本釋出](./whats-new-in-flutter-2-8) 以來還不到兩個⽉，即使在這麼短的時間內，我們也已處理和關閉了 1843 個 Issue，合併了來⾃全球 155 位貢獻者的 1525 個 PR。感謝⼤家在 2021 年末的假期期間的出⾊⼯作。

我們有⼏件令⼈興奮的事情要宣佈，包括 Flutter 對 Windows ⽀持的重⼤更新、⼏項重⼤的效能改進、框架中圖示和顏⾊的新⽀持以及一些⼯具改進等。此外，該版本還包括移除了 dev 渠道的更新、減少對舊版 iOS 的⽀持以及幾個簡短的破壞性改動。讓我們開始吧！

## 使用 Flutter 建構 Windows 桌面應用支援已經進入穩定階段

⾸先，Flutter 2.10 版本帶來了穩定的 Windows ⽀持，無需再透過 `--enable-windows-desktop` 標記來單獨配置 Windows 桌面版應用程式的支援，因為它現在已經預設被啟用！

![](https://devrel.andfun.cn/devrel/posts/2022/02/2b8921d82869b.jpg)

當然，此次穩定版釋出肯定不只是“刪掉”一個標記這麼簡單 ;-) 在 Flutter 2.10 的 Windows 支援中，也包含了對⽂本處理、鍵盤處理和鍵盤快捷鍵的⼴泛改進，以及直接與 Windows 整合的新方式、⽀持命令列引數、全球化⽂本輸⼊和無障礙功能等。

有關 Windows 穩定版釋出的更多資訊，你可以閱讀今天的另一篇推送文章，文章為大家詳述了 Flutter 在 Windows 上的架構，同時說明了有多少 Flutter package 和外掛已經⽀持 Windows。你還可以檢視我們的⼯具和應⽤合作伙伴在 Windows 上使⽤ Flutter 製作的一些⽰例等。

## 引擎的效能改進

此版本的 Flutter 包括由社群成員 [knopp](https://github.com/knopp "Flutter 社群成員 Matej 的 GitHub 主頁") 提供的 **繪製髒區管理** 的初步⽀持，他為 [iOS/Metal 上的單個髒區域啟⽤了選擇性重繪](https://github.com/flutter/engine/pull/28801 "iOS/Metal 上的單個髒區域啟⽤了選擇性重繪")。這一變化將一些基準測試中九十分位和九十九分位的光柵化時間減少了一個數量級，並將這些基準測試中的 GPU 利⽤率從 90% 以上降低到了 10% 以下。

![最佳化後的 Skia 渲染基準測試資料 1](https://devrel.andfun.cn/devrel/posts/2022/02/fa73031f55ece.jpg)

我們希望在未來的版本中，[將選擇性重繪的優勢引入到其他平台](https://github.com/flutter/engine/pull/29591 "將選擇性重繪的優勢引入到其他平台")。

在 [Flutter 2.8](./whats-new-in-flutter-2-8)。而在 Flutter 2.10 中，我們已經開始使⽤它 (`DisplayList`) 進行最佳化。例如，**[一種常見不透明層的情況現在已經以更高效的方式實現](https://github.com/flutter/engine/pull/29775 "一種常見不透明層的情況現以更高效的方式實現")**。即使在最壞的情況下，在我們基準測試中每幀光柵化時間也下降到了先前的三分之一以下。

![最佳化後的 Skia 渲染基準測試資料 2](https://devrel.andfun.cn/devrel/posts/2022/02/7ef05091b9321.jpg)

隨著我們繼續開發記錄格式，將來會繼續將該最佳化擴充到更多的場景。

在 profile 和 release 模式下，Dart 程式碼將以 AOT 方式編譯。這段程式碼的輕量和高效的關鍵來源於整個程式的型別流分析，它解鎖了許多編譯器最佳化和激進的搖樹最佳化 (tree-shaking)。但由於型別流分析必須涵蓋整個程式，因此可能會有些消耗效能。新版本帶來了 [更快的型別流分析實現](https://dart.googlesource.com/sdk.git/+/e698500693603374ecc409e158f36c25bff45b12 "Flutter 新版本帶來了更快的型別流分析實現")。在我們的基準測試中，Flutter 應⽤的總體建構時間下降了約 10%。

![最佳化後的建構時間](https://devrel.andfun.cn/devrel/posts/2022/02/869bf3b91639b.jpg)

與往常一樣，效能增強、減少記憶體使⽤和減少延遲是 Flutter 團隊的⾸要任務。期待未來版本的進一步改進。

## iOS 平台更新

除了效能改進之外，我們還新增並增強了一些特定平台的功能。[luckysmg](https://github.com/luckysmg "luckysmg") 就為我們帶來了一項 [iOS 上新的增強功能——更流暢的鍵盤動畫](https://github.com/flutter/engine/pull/29281 "iOS 上新的增強功能——更流暢的鍵盤動畫")，它會⾃動應用在你的應用中。

![iOS 鍵盤過渡動畫](https://devrel.andfun.cn/devrel/posts/2022/02/6bd64fc2cc49f.gif)


除此以外，我們還透過修復一些 [邊緣情況崩潰提⾼了 iOS 相機外掛的穩定性](https://github.com/flutter/plugins/pull/4619 "邊緣情況崩潰提⾼了 iOS 相機外掛的穩定性")。

最後，**我們為 64 位架構的 iOS 系統加入減少記憶體使⽤的新功能：[壓縮指標](https://github.com/flutter/engine/pull/30077 "我們為 64 位架構的 iOS 系統加入減少記憶體使⽤的新功能: 壓縮指標")。**

64 位架構將指標表⽰為 4 位元組大小的資料結構。當你有大量物件時，指標本⾝佔⽤的空間會增加應⽤整體的記憶體佔用，特別是如果你的應⽤更龐大更復雜時，這些應⽤有更多的 GC 抖動。然而，iOS 應⽤不太可能有⾜夠的物件來佔⽤大部分的 32 位地址空間（20 億個物件），更不⽤說龐⼤的 64 位地址空間（900 億個物件）了。

Dart 2.15 中引入了壓縮指標的功能，在這次釋出的 Flutter 版本中，我們使⽤這項功能來減少 64 位 iOS 應⽤的記憶體使⽤量。你可以檢視 [Dart 2.15 部落格⽂章](./announcing-dart-2-15) 瞭解詳細資訊。

與此同時，Dart 2.16 穩定版也正式釋出，我們將在稍微晚些的時間釋出 Dart 2.16 的更新內容，敬請關注。

## Android 平台更新

Flutter 2.10 版本還包含許多針對 Android 平台的改進。現在在預設情況下，當你建立新應⽤時，**Flutter 預設⽀持最新版本的 Android**，即 Android 12（API 級別 31）。此外，在此版本中，我們啟⽤了 [multidex](https://developer.android.google.cn/studio/build/multidex "為你的建構啟⽤ multidex") ⾃動⽀持。如果您的應⽤⽀持低於 21 的 Android SDK 版本，並且超過了 64K 的 dex ⽅法數限制，只需將 `--multidex` 引數傳遞給 `flutter build appbundle` 或 `flutter build apk` 命令，你的應⽤就會增加 multidex 的⽀持。

最後，在我們收到開發者們對於 Gradle 丟擲讓人費解的錯誤訊息的反饋後，我們調整了 Flutter 命令列工具，現在它將 **為常⻅問題提供解決方法**。例如，如果你向你的應⽤添加了一個外掛，該外掛需要您提⾼最低支援的 Android SDK 版本，你現在會在錯誤資訊中看到「Flutter Fix」的建議。

![Gradle 錯誤提示](https://devrel.andfun.cn/devrel/posts/2022/02/37d95f749a27e.jpg)

我們將繼續為常⻅錯誤訊息新增更多解決方法的建議，並希望獲得你對其他錯誤訊息的反饋，這些錯誤訊息將顯著幫助開發者處理同類問題。

## Web 平台更新

此版本還包含對 Web 平台的一些改進。例如，在先前的版本中，當滑鼠拖動到多行文字框的邊緣時，它不會正確地跟隨滾動。在此版本中，當選擇游標拖出了文字框時，文字框會進行滾動，瀏覽並選中對應的文字內容。此行為同時適⽤於 Web 平臺和桌⾯端。

![在 Web 平台選中並拖拽 TextField 游標](https://devrel.andfun.cn/devrel/posts/2022/02/516ce9d6c26e3.gif)

Flutter 2.10 還包括對 Web 平台的另一項顯著改進，我們也一直在尋求減少將 Flutter 應用執行到 Web 平台的開銷，在先前的版本中，每次我們想要將原⽣ HTML 的 widget 引⼊ Flutter 應⽤時，我們都需要一個覆蓋層 (Overlay) 作為我們對 Web 的平臺視圖 (Platform view) ⽀持的一部分。這些疊加層中的每一個都⽀持⾃定義繪製，但同時也代表了一定數量的效能開銷。如果你的應⽤中有⼤量原⽣ HTML 的 widget (例如連結)，那這將造成非常大效能開銷。在這個版本中我們為 Web 平台建構了一個新的「⾮繪製的平臺視圖」，已經基本上消除了這種開銷。我們已經 [將這種最佳化](https://github.com/flutter/plugins/pull/4578 "使用⾮繪製的平臺視圖最佳化") 應用到 [Link widget](https://pub.flutter-io.cn/documentation/url_launcher/latest/link/Link-class.html "Link widget API 文件") 中，這意味著如果您的 Flutter Web 應⽤中有⼤量的連結，它們將不再產生額外的效能消耗。隨著時間的推移，我們會將此最佳化應⽤到其他的 widget。

## Material 3

此版本是向 Material 3 過渡的開始，其中包括 [從單一種⼦顏⾊⽣成整個配⾊⽅案](https://github.com/flutter/flutter/pull/93463 "從單一種⼦顏⾊⽣成整個配⾊⽅案") 的能力。

你可以使用任意顏色來建立新的 `ColorScheme` 型別：

```dart
final lightScheme = ColorScheme.fromSeed(seedColor: Colors.green);
final darkScheme = ColorScheme.fromSeed(seedColor: Colors.green, brightness: Brightness.dark);
```

`ThemeData` 的建構函式還有一個新的 `colorSchemeSeed` 引數，可以直接從顏色⽣成主題的配⾊⽅案：

```dart 
final lightTheme = ThemeData(colorSchemeSeed: Colors.orange, ...); 
final darkTheme = ThemeData(colorSchemeSeed: Colors.orange, brightness: Brightness.dark, ...); 
```

此外，此版本包括了 **`ThemeData.useMaterial3` 的引數，它可以將 widget 切換到新的 Material 3 外觀**。

最後，我們添加了 [1028 個新的 Material 圖示](https://github.com/flutter/flutter/pull/95007 "添加了 1028 個新 Material 圖示的 PR")。

![1028 個圖示的部分預覽](https://devrel.andfun.cn/devrel/posts/2022/02/8f784a556bba2.jpg)

你可以在 [這個 issue 中](https://github.com/flutter/flutter/issues/91605 "在這裡跟進 Material 3 的接入情況") 跟進 Material 3 的接入情況，並隨時留下你的意見反饋。 

## 整合測試改進

2020 年 12 ⽉，我們宣佈了使⽤ `integration_test` 進行端到端測試的新⽅法，檢視中文文件 [整合測試介紹](https://flutter.cn/docs/cookbook/testing/integration/introduction "中文文件: 整合測試介紹") 瞭解更多。這個新的 package 取代了 `flutter_driver` 作為進行整合測試的推薦⽅式，提供了新功能，如 Firebase 測試實驗室⽀持以及對 Web 和桌⾯的⽀持。

從那時起，我們對整合測試進行了進一步改進，包括 **將 `integration_test` package 內建在 Flutter SDK 中**，使其更容易與您的應⽤整合。現在，我們撰寫了一份新的遷移指南，幫助你 [從 flutter_driver 測試遷移到 integration_test](https://flutter.cn/docs/testing/integration-tests/migration "從 flutter_driver 測試遷移到 integration_test")。

[現有的⽂檔](https://flutter.cn/docs/testing/integration-tests "整合測試說明⽂檔")、[範例](https://github.com/flutter/samples/tree/master/testing_app "整合測試範例") 和 [codelab](https://codelabs.developers.google.com/codelabs/flutter-app-testing "整合測試的 codelab") 也已針對 `integration_test` 進行了更新。如果你還沒有在 Flutter 應⽤上使⽤ `integration_test`，那麼從現在就開始吧！

## DevTools

在這個版本中，我們也在 Flutter DevTools 上做了一些改進⼯作，包括從命令列直接使⽤ DevTools 的簡易功能。現在你⽆需使⽤ `pub global activate` 來下載和執行最新版本的 devtools，只需簡單地使⽤ `dart devtools` 獲取與你正在使⽤的 Flutter 版本一致的最新版本即可。

我們還進行了一些 [可⽤性更新](https://github.com/flutter/devtools/pull/3493 "為 DevTools 加入可⽤性更新的 PR")，包括 [改進在偵錯程式變數窗格中檢查⼤型列表和對映](https://github.com/flutter/devtools/pull/3497 "改進在偵錯程式變數窗格中檢查⼤型列表和對映") 的⽀持（感謝 [Elliott](https://github.com/elliette "Elliott 的 GitHub 主頁")）。

![在 DevTools 中檢視大型列表和對映](https://devrel.andfun.cn/devrel/posts/2022/02/aaff408a25f10.gif)

最後，我們即將釋出年度 DevTools 問卷調查！請提供你的反饋並幫助我們改善你的開發體驗。

![該調查提⽰將在 2 ⽉中旬的某個時間直接顯⽰在 DevTools 中，敬請參與並踴躍反饋！](https://devrel.andfun.cn/devrel/posts/2022/02/2f802a49c7f72.jpg)

## VSCode 改進

Flutter 的 Visual Studio Code 擴充也獲得了許多增強功能，包括 **在程式碼的更多位置預覽顏色** 以及 [可更新顏色程式碼的顏⾊選擇器](https://github.com/Dart-Code/Dart-Code/issues/3240 "VS Code 中加入了可更新顏色程式碼的顏⾊選擇器")。

![VSCode 的 Flutter 顏色選擇器](https://devrel.andfun.cn/devrel/posts/2022/02/bcc4017326ad4.gif)

此外，如果你想成為 VSCode 的 Dart 和 Flutter 擴充的預釋出版本的測試⼈員，你可以 [在你的擴充設定中切換到預釋出版本](https://github.com/Dart-Code/Dart-Code/issues/3729 "將 Dart 和 Flutter 擴充切換到預釋出版本")。

![使用預釋出版本的外掛](https://devrel.andfun.cn/devrel/posts/2022/02/dc881f95347ef.jpg)

你可以在 flutter-announce 郵件列表的 [這一篇](https://groups.google.com/g/flutter-announce/c/lR-yn1s9HKk "flutter-announce 郵件列表中關於 VS Code 外掛改進的文章") 閱讀有關此更新的詳細資訊。

## 移除 dev 版本釋出渠道

在 Flutter 2.8 版本中，我們宣佈我們正在努力移除 dev 版本釋出渠道，以簡化你的選擇並減少研發的開銷。在這個版本中，我們已經完成了這項⼯作，包括：

- 更新 Flutter ⼯具以幫助將開發⼈員遷移出開發頻道 
- 更新 wiki 對於各個渠道的說明和承諾
- 更新棄⽤政策
- 從 DartPad、預提交測試和⽹站中刪除 dev 渠道的⽀持

Dev 渠道現已被徹底移除。如果我們漏了一些沒有移除的位置，請告訴我們。

## 對 iOS 9.3.6 的⽀持進入尾聲

由於我們實驗室中⽬標裝置的使⽤減少和維護難度增加，我們正在 [調整對於 iOS 9.3.6 的支援](https://files.flutter-io.cn/flutter-design-docs/RFC_Move_32-bit_iOS_to_Best_Effort_Tier.pdf "RFC 文件: 調整對於 iOS 9.3.6 的支援")，[從「⽀持」到「盡力⽽為」](https://flutter.cn/docs/development/tools/sdk/release-notes/supported-platforms "對於 iOS 9.3.6 的支援從「⽀持」到「盡力⽽為」")。這意味著對 iOS 9.3.6 的⽀持和對 32 位 iOS 裝置的⽀持將僅透過編碼實踐、Ad-Hoc 和社群測試來維護了。 

我們預計在 2022 年第三季度的 Flutter 穩定版本中放棄對 32 位 iOS 裝置以及 iOS 版本 9 和 10 的 ⽀持。這意味著基於穩定的 Flutter SDK 建構的應⽤將不再在 32 位 iOS 裝置上執行，並且 Flutter ⽀持的最低 iOS 版本將增加到 iOS 11。

## 破壞性改動

我們還努力在每個版本和此版本中減少破壞性改動，儘管我們還沒有完全歸零，但我們會繼續努力！

- [建構 Flutter 應用的 Kotlin 版本應高於 1.5.31](https://flutter.cn/docs/release/breaking-changes/kotlin-version "建構 Flutter 應用的 Kotlin 版本應高於 1.5.31")
- [Flutter 2.5 之後移除的已棄用的 API](https://flutter.cn/docs/release/breaking-changes/2-5-deprecations "Flutter 2.5 之後移除的已棄用的 API")
- [Web 上的原始圖像使用正確的來源和顏色](https://flutter.cn/docs/release/breaking-changes/raw-images-on-web-uses-correct-origin-and-colors "Web 上的原始圖像使用正確的來源和顏色")
- [Apple Pencil 隨手寫 TextInputClient 變動](https://flutter.cn/docs/release/breaking-changes/scribble-text-input-client "Apple Pencil 隨手寫 TextInputClient 變動")

如果您仍在使⽤這些 API，可以閱讀 [flutter.cn 上的遷移指南](https://flutter.cn/docs/release/breaking-changes "flutter.cn 上的遷移指南")。一如既往，⾮常感謝社群貢獻測試，幫助我們識別這些破壞性改動。

## 總結

謹代表 Google Flutter 團隊的所有成員向大家說一聲謝謝，感謝你成為社群的一員！有了社群的幫助，Flutter 才能成為最受歡迎的跨平臺 UI 工具。儘管對 Windows 的穩定⽀持才剛剛開始，但我們已經開始期待我們將共同建構的一切！

## 致謝

- 原文: What’s New in Flutter 2.10
- 連結: https://medium.com/flutter/whats-new-in-flutter-2-10-5aafb0314b12
- 翻譯: Alex
- 審校: Vadaski / Luke
- 製圖: Lynn
