---
title: Flutter 2.5 更新詳解
toc: true
---

![](https://gglh6.g.forms.cn/YoLrEC3CimENH3yNhDhyTM27tDkMsiEtEemrKlXfgP7IlFnJFP58KCS7-AAb0QgddmUiziQK3eGs8BuYADBp6-TeKJ01C88ww83c80PhJP_x6i1GdUvtU_1YqRUSJ-HTf3pwQtSN=s0)

Flutter 2.5 正式版已於上週正式釋出！這是一次重要的版本更新，也是 Flutter 釋出歷史上各項統計資料排名第二的版本。我們關閉了 4600 個 Issue，合併了 3932 個 PR，它們分別來自 252 個貢獻者和 216 個稽核者。回顧去年 -- 我們收到來自 1337 個貢獻者提交的 21072 個 PR，其中有 15172 個被合併。在詳述本次更新的內容之前，我們想強調，Flutter 的首要工作始終是高品質交付開發者們所需要功能。

Flutter 2.5 帶來了一些重要的效能和工具改進，以幫助開發者們追蹤應用中的效能問題。同時，加入了一些新的功能，包括對 Android 的全屏支援、 對 Material You (也稱 v3) 的更多支援、對文字編輯的更新以支援切換鍵盤快捷鍵、在 Widget Inspector 中檢視 widget 詳情、在 Visual Studio Code 專案中新增依賴關係的新支援、從 IntelliJ / Android Studio 的測試執行中獲得測試覆蓋率資訊的新支援，以及一個更貼近 Flutter 應用在真實的使用場景下的應用範本等。這個版本充滿了令人興奮的新更新，讓我們開始介紹吧！

該版本進行了一些效能上的改進：首先是一項用於從離線訓練執行中連線 Metal 著色器預編譯的 PR ([#25644](https://github.com/flutter/engine/pull/25644 "#25644"))，這將最壞情況下的光柵化時間減少了 2/3 (如我們的基準測試所示)，將第 99 百分位的幀時間減少了一半。我們在減少 iOS 卡頓方面取得了持續性的進展，這也是在這條道路上邁出的另一步。然而，著色器預熱只是卡頓的一個來源。在該版本以前，處理來自網路、檔案系統、外掛或其他 isolate 的非同步事件可能導致動畫中斷，這是另一個卡頓的來源。在該版本中我們對 UI Isolate 的事件迴圈的排程策略 ([#25789](https://github.com/flutter/engine/pull/25789 "#25789")) 進行了改進，現在幀處理優先於其他非同步事件的處理，在我們的測試中，其導致的卡頓已經被消除。

![調整前後處理非同步事件造成的幀建構滯後時間](https://gglh6.g.forms.cn/sfegQdXcNLdBLnybZjacGhJwUuGErq5Jp4MQt3BStGm-EDysCKkD9uaWyVM2NCMVb81-TljxLW7-KEENUTsqMSWayhfLRX7yIrgYTU5kXH-s8otMzg2ffj-u5B6GV0j3-npcCMPg=s0)

另一個原因是垃圾回收 (GC) 會暫停 UI 執行緒來回收記憶體。在該版本以前，一些影象的記憶體只能在 Dart VM 執行 GC 時以較慢的速度進行回收。在早期版本中，常用的做法是 Flutter 引擎會向 Dart VM 提示影象記憶體可以透過 GC 回收，理論上可以讓記憶體回收更為及時。不幸的是，在實踐中這造成了過多的回收，而且記憶體有時仍然不能被快速回收，導致無法避免在記憶體有限的裝置上出現低記憶體的情況。在現在的版本中，未使用的影象的記憶體會盡可能快速地進行回收 ([#26219](https://github.com/flutter/engine/pull/26219 "#26219")、[#82883](https://github.com/flutter/flutter/pull/82883 "#82883")、[#84740](https://github.com/flutter/flutter/pull/84740 "#84740"))，這大大減少了 GC 的次數。

![修復前和修復後的 GC 次數，以儘快回收未使用的大影象記憶體](https://gglh6.g.forms.cn/aYmIHpHERjkTHh_ymdkgeyQRh_qXN72rlIAHSvFxAEoBaejKtM5u82GNAadbS8FH8JFe2WvZdtSmJVoqw0b_lUmMpCWKkFQh9fOuTFcsmyveFzesGlkHDDuuPc-H_7gcWrrsvNS0=s0)

舉個例子，在我們的一個測試中，播放了一個 20 秒的 GIF 動畫，GC 的次數從需要 400 次下降到只需 4 次。更少的主要 GC，意味著更少的涉及影象出現和消失的動畫卡頓，更少的 CPU 和電量消耗。

Flutter 2.5 的另一項效能改進是優化了 iOS 上 Dart 和 Objective-C/Swift、Android 上 Dart 和 Java/Kotlin 之間相互通訊的延遲。作為 [調整訊息通道](https://files.flutter-io.cn/flutter-design-docs/2021_Platform_Channel%20Performance_Tuneup.pdf "調整訊息通道") 的一部分，我們從訊息編解碼器中移除了不必要的複製，在不同內容的大小和裝置上減少了高達 50% 的延遲 (詳見 [#25988](https://github.com/flutter/engine/pull/25988 "#25988")、[#26331](https://github.com/flutter/engine/pull/26331 "#26331"))。

![調整前後的 iOS 訊息延遲](https://gglh6.g.forms.cn/gc-YrPSIwirzTDVJ6FqGK5Lugn1YsA_l5h8shX8W5ROi_2doeJO8h-835CDX4wsDBAyjMwQGp-Ytr9dBLnswV6xMOWWJJzAAkupJ62-Gy0IRxk-9bbtuq0r4OdshSzuAmTwS6oEI=s0)

更多詳情，請你參閱 Aaron Clarke 的文章：[提高 Flutter 中的平台通道效能](https://medium.com/flutter/improving-platform-channel-performance-in-flutter-e5b4e5df04af "提高 Flutter 中的平台通道效能")。

如果你要建構 iOS 應用，我們還有最後一項效能更新：在該版本中，使用 Apple Silicon M1 Mac 建構的 Flutter 應用可以直接在 ARM 架構的 iOS 模擬器 ([#pull/85642](https://github.com/flutter/flutter/pull/85642 "#pull/85642")) 上執行。這意味著無需使用 Rosetta 對 Intel x86_64 指令和 ARM 進行轉換，這提升了 iOS 應用測試的效能，並規避了一些微妙的 Rosetta 問題 ([#74970](https://github.com/flutter/flutter/issues/74970#issuecomment-858170914 "#74970")、[#79641](https://github.com/flutter/flutter/issues/79641 "#79641"))。這是 Flutter 在全面支援 Apple Silicon 的路程上邁出的又一步，敬請繼續關注。

當然，沒有 Dart 語言和它的執行時環境，就不會有現在的 Flutter，它建立在 Dart 語言和 runtime 之上。Flutter 2.5 同時帶來了 Dart 2.14。[新發布的 Dart 版本](https://medium.com/@mit.mit/announcing-dart-2-14-b48b9bb2fb67 "新發布的 Dart 版本") 不僅帶來了新的格式化使 [級聯](https://dart.tw.gh.miniasp.com/guides/language/language-tour%23cascade-notation "級聯") 操作更加清晰，還帶來了支援忽略檔案的新 pub 命令工具，以及新的語言功能 (包括傳說中的無符號右移運運算元的迴歸)。此外，這個版本帶來了一套新的 Dart 和 Flutter 專案之間共享的標準程式碼規範提示，開箱即用，這也是 Dart 2.14 最精彩的部分。

![](https://gglh6.g.forms.cn/HKlMxP0-H-E-E39GOaThjkrQXfE21owI2dPCHiDzNskmDJe81eomRBqQrYeV7jhYYFqcD9gOotVMJYjrYdAir0eqs_feJD4PQcj1k1sb7kPMQtHBpZcQVXn6tYjsGdIm92O7HOh3=s0)

`flutter create` 開箱即有一個 analysis_options.yaml 檔案，預先使用了推薦的 Flutter lint。

當你建立一個新的 Dart 或 Flutter 專案時，你不僅可以使用這些規範，而且 [只需要幾個步驟](https://flutter.tw/release/breaking-changes/flutter-lints-package#migration-guide "只需要幾個步驟") 就可以將這種相同的分析新增到你現有的應用中。關於這些規範的細節、新的語言功能和更多內容，請查閱：[Dart](https://medium.com/dartlang/announcing-dart-2-14-b48b9bb2fb67 "Dart") [2.14](https://medium.com/dartlang/announcing-dart-2-14-b48b9bb2fb67 "2.14") [釋出](https://medium.com/dartlang/announcing-dart-2-14-b48b9bb2fb67 "釋出")。

Flutter 2.5 版本對框架進行了一些修復和改進。[我們修復了關於 Android 全屏模式的一系列相關問題](https://github.com/flutter/flutter/pull/81303 "我們修復了關於 Android 全屏模式的一系列相關問題")，該 Issue 獲得了上百個點贊，全屏選項包括向後傾斜、沉浸模式、粘性沉浸模式和邊到邊四種。這一變化還增加了一種方法用來監聽其他模式下的全屏變化。例如，如果使用者在使用應用時，改變了系統介面的全屏模式，開發者現在可以透過程式碼讓應用重新變為全屏，或執行其他操作。

![新的 Android 邊到邊模式：正常模式 (左)，邊到邊模式 (中)，帶有自訂 SystemUIOverlayStyle 的邊到邊 (右)](https://gglh6.g.forms.cn/lgpnggVQ-nZ3DDzVapq5KLwF4n65RT4AKL5CqNgPdiEhItRwQIAPHswdzM6Gso4_eZdKh7Vpau-Q7dKKePF_NL_uD0fWNj40kXoPb6m1tUoN1gitLaEwEE6NM93ldIw040IU1Ah5=s0)

在這個版本中，我們繼續建立對新的 Material You (又稱 v3) 規範的支援，包括對懸浮按鈕尺寸和主題的更新 ([#86441](https://github.com/flutter/flutter/pull/86441 "#86441"))，以及一個新的 `MaterialState.scrolledUnder` 狀態，你可以透過 PR ([#79999](https://github.com/flutter/flutter/pull/79999 "#79999")) 中的範例程式碼看到它的效果。

![Material You 中新的 FAB 尺寸](https://gglh6.g.forms.cn/74UEjhhQqiY0Aiy0RJ6JLFfVOKSweMDuzk92mSdN7de_UE-Y5AlOno1HlOeqnjZAItC1zcyl-KO9J9GAapTFzb_o-8F04AVC6kTR23-cInBrYQGAn4V4tVEfl03pbTyVJRTF6Osi=s0)

![新的 MaterialState.scrolledUnder 狀態](https://gglh6.g.forms.cn/B65Zt6C7aiA7XWaZ8k4kvd_p5SJ29NE7kl89eMMNE8hidGBNydsAPI0rcVk9kM3ECfsQR26VGY8TRi7BdnxVW5Dq0wQOTzpknXcjsWCtg_T-SzoJiA3DY0_tPPUUxXGWepTLmJz2=s0)

當我們討論滾動時，另一個改進是增加了額外的滾動指標通知 ([#85221](https://github.com/flutter/flutter/pull/85221 "#85221")、[#85499](https://github.com/flutter/flutter/pull/85499 "#85499"))，即使使用者沒有滾動，也會提供可滾動區域的通知。例如，如下範例展示了捲軸根據 ListView 的實際大小而適時出現或消失的效果。

![新的滾動指標通知，使捲軸自動出現和消失。](https://gglh6.g.forms.cn/WxBebxGYC1nJdPctbw4vYuy_fVI-KmS9YsU0jrXjZ3BLkvwxEb1LoM65aKjN0HUaJmmrf8w83T29q_NzZiH6bUvCe1K9NeEVPQkCVHYaHntrutLprfBo6-LYT-u8c9OuSad7VhYu=s0)

在這種情況下，你不需要寫任何程式碼，就可以捕獲 [ScrollMetricNotification](https://master-api.flutter.dev/flutter/widgets/ScrollMetricsNotification-class.html "ScrollMetricNotification") 的變化。特別感謝社群貢獻者 [xu-baolin](https://github.com/xu-baolin "xu-baolin")，他在這方面做了大量工作，並提出了一個很好的解決方案。

社群的另一傑出貢獻是為 `ScaffoldMessenger` 增加了 Material 橫幅的支援。在 [Flutter 2.0](https://medium.com/flutter/whats-new-in-flutter-2-0-fe8e95ecc65 "Flutter 2.0") 中新增的 `ScaffoldMessenger`，它提供了一種強大的方式，在螢幕底部顯示 SnackBars 以向用戶提供通知。在 Flutter 2.5 中，現在你可以在 Scaffold 頂部新增一個橫幅，在使用者將其關閉之前，它將一直保持在原位。

![你可以透過呼叫 ScaffoldMessenger 的 showMaterialBanner 方法觸發這種行為](https://gglh6.g.forms.cn/r0-lBXgPf0gr0mqiW9H3p3t9w7_4kD5SzVuLIv7ElmhlmzJeZNbckDlg0yJW2Kp0hwswa7NGerIWCOZ-StOjRFRNx5cuf1aIaAUCsV0IOle8lMJPxM0nSEOasLAHmGAuMnRAPgCu=s0)

[橫幅的 Material 指南](https://material-io.cn/components/banners "橫幅的 Material 指南") 規定你的應用一次只能顯示一個橫幅，所以如果你的應用多次呼叫 `showMaterialBanner`，`ScaffoldMessenger` 將持有一個佇列，在前一個橫幅被關閉時顯示下一個新的橫幅。感謝 [Calamity210](https://github.com/Calamity210 "Calamity210") 為 Flutter 的 Material 支援提供了有力補充。

在 Flutter 2.0 及其新文字編輯功能的基礎上，我們在這個版本中添加了如文字選擇器、攔截覆寫任何鍵盤事件，以及覆寫文字編輯的鍵盤快捷方式的能力 ([#85381](https://github.com/flutter/flutter/pull/85381 "#85381"))。如果你想讓 Ctrl - A 做一些自訂操作，而不是選擇所有文字，你可以自行定義。[DefaultTextEditingShortcuts](https://github.com/flutter/flutter/blob/b524270af147847f64fa296cf6eed3633ffe683d/packages/flutter/lib/src/widgets/default_text_editing_shortcuts.dart#L163 "DefaultTextEditingShortcuts") 類包含了 Flutter 在每個平臺上支援的每個鍵盤快捷方式的列表。如果你想覆寫其中的關聯，請使用 Flutter 現有的 [Shortcuts](https://api.flutter.dev/flutter/widgets/Shortcuts-class.html "Shortcuts") widget，將任一快捷鍵重新對映到現有或自訂的意圖，您可以將該 widget 放置在你想要覆寫的地方。範例請參閱：[API 文件](https://api.flutter.dev/flutter/widgets/DefaultTextEditingShortcuts-class.html "API 文件")。

另一個得到大量改進的外掛是 [camera 外掛](https://pub.dev/packages/camera "camera 外掛")：

- [3795](https://github.com/flutter/plugins/pull/3795 "3795") \[camera\] android-rework 第 1 部分：支援 Android 相機功能的基礎類
- [3796](https://github.com/flutter/plugins/pull/3796 "3796") \[camera\] android-rework 第 2 部分：Android 自動對焦功能
- [3797](https://github.com/flutter/plugins/pull/3797 "3797") \[camera\] android-rework 第 3 部分：Android 曝光相關功能
- [3798](https://github.com/flutter/plugins/pull/3798 "3798") \[camera\] android-rework 第 4 部分：Android 閃光燈和變焦功能
- [3799](https://github.com/flutter/plugins/pull/3799 "3799") \[camera\] android-rework 第 5 部分：Android FPS 範圍、解析度和感測器方向功能
- [4039](https://github.com/flutter/plugins/pull/4039 "4039") \[camera\] android-rework 第 6 部分：Android 曝光和對焦點功能
- [4052](https://github.com/flutter/plugins/pull/4052 "4052") \[camera\] android-rework 第 7 部分：Android 降噪功能
- [4054](https://github.com/flutter/plugins/pull/4054 "4054") \[camera\] android-rework 第 8 部分：最終實現的支援模組
- [4010](https://github.com/flutter/plugins/pull/4010 "4010") \[camera\] 在 iOS 上不觸發平放時的裝置方向
- [4158](https://github.com/flutter/plugins/pull/4158 "4158") \[camera\] 修復 iOS 上設定焦點和曝光點的座標旋轉
- [4197](https://github.com/flutter/plugins/pull/4197_ "4197") \[camera\] 修復相機預覽在裝置方向改變時不總是重建的問題
- [3992](https://github.com/flutter/plugins/pull/3992 "3992") \[camera\] 防止在設定不支援的 FocusMode 時崩潰
- [4151](https://github.com/flutter/plugins/pull/4151 "4151") \[camera\] 引入 camera_web package

在 [image_picker 外掛](https://pub.dev/packages/image_picker "image_picker 外掛") 上也做了很多工作，專注於端到端的相機體驗。

- [3898](https://github.com/flutter/plugins/pull/3898 "3898") \[image_picker\] 影象採集器修復相機裝置
- [3956](https://github.com/flutter/plugins/pull/3956 "3956") \[image_picker\] 在 Android 中將相機捕捉的儲存位置改為內部快取，以符合新的 Google Play 儲存要求
- [4001](https://github.com/flutter/plugins/pull/4001 "4001") \[image_picker\] 刪除了多餘的相機許可權請求
- [4019](https://github.com/flutter/plugins/pull/4019 "4019") \[image_picker\] 修復當相機作為源時的旋轉問題

這些工作改善了 Android 的相機和 `image_picker` 外掛的功能和健壯性。此外，你也許會注意到 [camera 外掛](https://pub.dev/packages/camera_web "camera 外掛") 的 Web 功能已處於預覽階段 ([#4151](https://github.com/flutter/plugins/pull/4151 "#4151"))。這個預覽版提供了對檢視相機預覽、拍攝照片、使用閃光燈和變焦控制的基本支援，所有這些都可以在 Web 上進行。它目前不是一個 [被認可的聯合外掛](https://flutter.tw/development/packages-and-plugins/developing-packages#endorsed-federated-plugin "被認可的聯合外掛")，因此在配置中，你需要明確這個外掛僅能夠在 Web 應用中 [新增使用](https://pub.dev/packages/camera_web/install "新增使用")。

最初的 Android 相機重構工作是由 [acoutts](https://github.com/acoutts "acoutts") 貢獻完成的。`camera` 和 `image_picker` 的工作是由 [Baseflow](https://www.baseflow.com/open-source/flutter "Baseflow") 完成的，這是一家專門從事 Flutter 的諮詢公司，因其 [在 pub.dev 上的 package ](https://pub.dev/publishers/baseflow.com/packages "在 pub.dev 上的 package") 而聞名。`camera_web` 的工作主要由 [Very Good Ventures](https://verygood.ventures/ "Very Good Ventures") 完成，這是一家位於美國的 Flutter 諮詢公司。非常感謝你們對 Flutter 社群的貢獻！

另一個有價值的社群貢獻是由 fluttercommunity.dev 組織做出的，他們的代表作是 [](https://plus.fluttercommunity.dev/_3yzKulu_dTTYflhO) [plus 系列外掛](https://plus.fluttercommunity.dev/ "plus 系列外掛")。隨著新的 Flutter 版本釋出，之前由 Flutter 官方團隊維護的外掛現在 “交接” 給了 fluttercommunity.dev 組織，在每個外掛下方都會有下面類似的提示：

![](https://gglh6.g.forms.cn/tD8xTpW_n2e9beQeVht1RJSxwBr9ZeMbwPFOy9zfbjp26TzyKR4FAFjbiuwzE2HkdVm8ycv76l_0j1bo24WhuvSjrh3wcMF2bC85OBpCvoQvDpMFE4n1gLrJOdSn_wPZp0Rxrs6x=s0)

此外，由於這些外掛不再積極維護，所以我們已取消了它們的 Flutter Favorite 標記。如果你還沒有遷移到 plus 系列外掛，我們建議你按照以下的表格進行對照遷移。

![](https://gglh6.g.forms.cn/Um8hjP4VXFLcmVGwwXqB9kDqifZoTLIcXf2z5KZYaMm0RBttCg-b1YoTMZ4SlxaRACKHEPgphTJ255Cw8bdU_ArQkrkpBGXZ2jX2Mcf2FobNylC8VHMnSbTZH-kh9ZMmrrZqpY5w=s0)

Flutter 2.5 對 Flutter DevTools 進行了大量的改進。首先是在 DevTools 中增加了對引擎更新的支援 ([#26205](https://github.com/flutter/engine/pull/26205 "#26205")、[#26233](https://github.com/flutter/engine/pull/26233 "#26233")、[#26237](https://github.com/flutter/engine/pull/26237 "#26237")、[#26970](https://github.com/flutter/engine/pull/26970 "#26970")、[#27074](https://github.com/flutter/engine/pull/27074 "#27074")、[#26617](https://github.com/flutter/engine/pull/26617 "#26617"))。其中一組更新使 Flutter 能夠更好地將追蹤事件與特定的幀聯絡起來，這有助於開發人員確定一個幀可能會超出預算的原因。你可以在 DevTools 框架圖中看到這一點，該圖表重構之後已經支援了即時展示；當你的應用正在渲染時，它們的資料會被填入該圖中。從這個圖表中選擇一個建構幀，就可以跳轉到該幀的時間線事件。

![](https://gglh6.g.forms.cn/j3HBk0HsjqGsxNN_U-cs_vZNBnUboY2n0ZIkikwmHdQOz12Vrn3TaIZlLzgxkj4CAnv6tWFYf0a-juf4JHJt-l6u924cA47n7RdnB4vNA1DZ3OUxRSqrh4VylaMJJf-4J-3zs-qH=s0)

Flutter 引擎現在也能識別時間線中的著色器編譯事件。Flutter DevTools 使用這些事件來幫助您診斷應用中的著色器編譯缺陷。

![DevTools 檢測到因著色器編譯而丟失的建構幀](https://gglh6.g.forms.cn/RFhqkY-_vYhPNn4BD9c14NqAti5EKTYmhXvl6tN45a_R6eCAfAicUU9YT_b70ZfuFsxN68UaPy5M0fPM3tpddXVw_kZ1Tw2-QGot_NSa6xFC2wtp3ghsL0ohEuIfA-j2_xi1rYcb=s0)

有了這個新功能，DevTools 可以檢測到你因著色器編譯而丟失的建構幀，以幫助你修復這個問題。如果你希望像首次執行應用一樣，使用 `flutter run` 命令並加上 `--purge-persistent-cache` 這個標記。這將清除著色器的快取，以確保你重現使用者在「首次執行」或「重新開啟」 (iOS) 應用時看到的效果。此功能仍在開發中，所以請將您發現的 [問題或改進建議](https://github.com/flutter/devtools/issues "問題或改進建議") 提交給我們，以幫助發現和改進著色器編譯工具。

此外，當你追蹤應用中的 CPU 效能問題時，可能已經淹沒在了來自 Dart 和 Flutter 庫或引擎的原生程式碼的剖析資料中。如果你想關閉這些資料以專注自己的程式碼，那麼你可以使用新的 CPU Profiler 功能 ([#3236](https://github.com/flutter/devtools/pull/3236 "#3236"))，使你能夠隱藏來自任何這些源的 Profiler 資訊。

![](https://gglh6.g.forms.cn/ayyGLnegxZ-ZKcwHL3v9Co7MO6WrfDExU2oVC3QshvWdo6BKn5IaUdtR0aw69Ltf87Zieqae46OEZOnbrxRhIs1ETmAeeB1G0RRdkXGJ7bCIEHlCcRrn6zbamYpCkexyZPQ6Q8np=s0)

對於你沒有過濾掉的類別，它們現在已經用顏色進行了分類 ([#3310](https://github.com/flutter/devtools/pull/3310 "#3310")、[#3324](https://github.com/flutter/devtools/pull/3324 "#3324"))，這樣你就可以很清晰地看到 CPU 火焰圖內容對應的部分。

![彩色的火焰圖，識別應用中的應用 vs. 原生 vs. Dart vs. Flutter 程式碼活動](https://gglh6.g.forms.cn/asJ8kGDi5H3BhU7tdAoaA_HVuBhqpFQAS51AaqxQZCit64SGUkD-A7XPcLhBHCs2wKCJH6bOPTBWMQlvmXikT-EcPL1Sr3wSTlwrZHaZWWYe5OSbdjGScU4qcAH81xPBtHIFQNPp=s0)

效能可能並不是你唯一想要除錯的內容。新版本的 DevTools 帶有對 Widget Inspector 的更新，當你將滑鼠懸停在 widget 上時，可以評估物件、檢視屬性、widget 狀態等等。

![](https://gglh6.g.forms.cn/j1K1fVwrpRwC82J5W5RvViiwiXPkm1DOdWu6E2DmXfhu3rYCOuZ80k9oKPx9af1hdvtRng7m04_oQfWTauz15cXrWn6zZ7l8zSeNcL3g6K3Aqe00xD5o2vbLxBFYj1RlYXDxPFzo=s0)

而且當你選擇一個 widget 時，它會在新的 Widget Inspector Console 自動彈出，在那裡你可以自由探索 widget 的屬性。

![](https://gglh6.g.forms.cn/rqzYV63V3EPFy1VAouAEWxHB8kyijkX-movIxTp4-77woc7hfFnt4xiOs7Aq35LQhrHkzM1_DRDVuUTs66ipUZ1IXPlx1qIFdbsKxN8KVBKRjdZOVdC11nTGrJYx21drRa62WACj=s0)

當在斷點處暫停時，你也可以在控制檯執行表示式。

除了新功能，Widget Inspector 也進行了改頭換面。為了使 DevTools 成為理解和除錯 Flutter 應用的最佳工具，我們與芬蘭的創意技術機構 [Codemate 合作](https://codemate.com/ "Codemate 合作")，進行了一些更新。

![DevTools 調整後提供了更易於使用的使用者體驗](https://gglh6.g.forms.cn/CjA8kBKJqNbdFMUU4THyN1agx3Ys_5gnYHEMPRLNKXCw73kg4i5HM5YKTMhIG6CQYCE_s71EqddWnQ3CBa9VUGvnYqddX3JuO7UIxOyWCR4KTI434ik6dJWOay-qGZ2jk9Uz5pvD=s0)

如上圖所示，你可以看到以下變化：

- 更好地傳達除錯切換按鈕的作用 —— 這些按鈕有新的圖示、面向任務的標籤，以及豐富的工具提示 (用於描述它們的功能和何時使用它們)。每個工具提示都進一步連結到了該功能的詳細文件。
- 更容易查詢和定位感興趣的 widget——Flutter 框架中經常使用的 widget 現在已在 Inspector 左側的 widget 樹檢視中作為圖示常駐。它們已經根據其類別使用顏色進行了分類別。例如，佈局 widget 顯示為藍色，內容 widget 顯示為綠色。此外，每個文字 widget 現在會顯示其內容預覽。
- 更一致的 Layout Explorer 和 widget 樹的顏色方案 —— 現在更容易從 Layout Explorer 和 widget 樹中識別出相同 widget。例如，如上圖所示中的「Column」widget 在 Layout Explorer 中是藍色背景，在 widget 樹檢視中也有一個藍色圖示。

我們很樂意聽到你對這些更新產生的任何 [問題和改進建議](https://github.com/flutter/devtools/issues "問題和改進建議") 以確保 DevTools 高效地執行。這些亮點僅是開始。關於 DevTools 在 Flutter 這個版本中的全部新內容，請查閱以下版本說明：

- [Flutter DevTools 2.3.2 版本說明](https://groups.google.com/g/flutter-announce/c/LSNbc2rKIjQ/m/7Y7cWgO2CQAJ "Flutter DevTools 2.3.2 版本說明")
- [Flutter DevTools 2.4.0 版本說明](https://groups.google.com/g/flutter-announce/c/qenYe5LuHb8/m/tkWcBCVaAQAJ "Flutter DevTools 2.4.0 版本說明")
- [Flutter DevTools 2.6.0 版本說明](https://groups.google.com/g/flutter-announce/c/yBEZOWdV9nc/m/KCX3m2BpCAAJ "Flutter DevTools 2.6.0 版本說明")

IntelliJ / Android Studio 的 Flutter 外掛在這個版本中也有一些改進，首先改進是執行整合測試的能力 ([#5459](https://github.com/flutter/flutter-intellij/pull/5459 "#5459"))。整合測試是在裝置上執行的整個應用測試，在 integration_test 目錄下執行，並使用與 widget 單元測試相同的 `testWidgets()` 功能。

![在 IntelliJ / Android Studio 中對 Flutter 應用進行整合測試](https://gglh6.g.forms.cn/ZqPzOQNznRhaLdiHOwfxVQSkBQjI9wkTFe-Ia4t0I_TjiwgrTURkb2vfUd4gTSFpey3oaPACcmHYcSzXXjxeU1jhVyBz9Toe0ygUEfS39fd66TB3Yg2lwtwLcFIGyrFFm7fLRR1P=s0)

要在您的專案中新增整合測試，請 [遵循 flutter.dev 上的說明](https://flutter.tw/testing/integration-tests "遵循 flutter.dev 上的說明")。要將測試與 IntelliJ 或 Android Studio 連線，請新增一個執行配置，啟動整合測試，並連線一個裝置供測試使用。執行配置可以讓你在執行測試的同時，設定斷點、步進等。

此外，Flutter 的最新 IntelliJ / Android Studio 系列外掛允許您檢視單元測試和整合測試執行的覆蓋率資訊。您可以透過「debug」按鈕旁邊的工具欄按鈕來存取這個資訊：

![](https://gglh6.g.forms.cn/emcKys6WDwSlUct4O-HwggDbXOFKj8VdXd7QPnbsf51q8m8LZarOHXGSCxoNpZwUYLOHU8QG_99BPW_bAzkCceea2fRoVsiRx5hbefMf5sLxmOgEJo0uB0ZtJN7a5FGHblwh1GpH=s0)

覆蓋率資訊將以紅色和綠色的矩形顯示在編輯視窗左側的空隙中。在這個例子裡，第 9 - 13 行被測試覆蓋，但第 3 和 4 行沒有被測試。

![](https://gglh6.g.forms.cn/kJyY1pox9TwRBohaGJR-p9frbyfCRvV6ldPRD7BK6Ie0g0rVAG5r_tNg94ipXkShy_nwaRmiyH4ZLD_imR8YZfN0oHD-9F2OsN5T_X1ZglJSkVc_i2msrKL9EIQfrdUsUUCIhrjl=s0)

最新版本還包括預覽來自 pub.dev 的 package 中使用的圖示的新功能，這些 package 是圍繞 TrueType 字型檔案建構的 ([#5504](https://github.com/flutter/flutter-intellij/pull/5504 "#5504")、[#5595](https://github.com/flutter/flutter-intellij/pull/5595 "#5595")、[#5677](https://github.com/flutter/flutter-intellij/pull/5677 "#5677")、[#5704](https://github.com/flutter/flutter-intellij/pull/5704 "#5704"))，正如 Material 和 `Cupertino` 圖示支援預覽一樣。

![IntelliJ / Android Studio 中的圖示預覽](https://gglh6.g.forms.cn/gqM1Sij4nxD3wd7CMCapkYAl3wrIQRnXjInjCaPv7Of8TUgixR94KBA9P0QclPodwWYfthq5i4vUMaXJoF12bXU2WmPPsXo2cEfNBnPn36SqPqJI0TsEB7aNYS9IvA9VayElr3Hb=s0)

要啟用圖示預覽，你需要告訴該外掛你正在使用哪些 package。在外掛的設定 / 偏好頁面有一個新的文字欄位。

![](https://gglh6.g.forms.cn/I87uIo9Htez8sAB28Ec-yavKndT2Jx_wGERQ_q_VwRO_mEgp3IHqtWoAg8s-01eviyLbsMrSy1mpJAtWuPya_xGs-NQHhIu1QuTO153XBmOjPBjqYz0qJz2MQficIbe5LKPhuTpg=s0)

注意，這對定義為類中靜態常量的圖示有效，如螢幕截圖中的範例程式碼所示。它不會對錶達式起作用，例如 `LineIcons.addressBook()` 或 `LineIcons.values['code']`。如果你是一個圖示 package 的作者，而這個圖示 package 並不適合這個功能，請 [建立一個 Issue](https://github.com/flutter/flutter-intellij/issues "建立一個 Issue") 進行反饋。

更多有關 Flutter 的 IntelliJ / Android Studio 外掛的更新資訊，請參閱下列釋出說明：

- [Flutter IntelliJ Plugin M57 釋出](https://groups.google.com/g/flutter-announce/c/nZPj0uIW3h4/m/2Xnx8KQtAwAJ "Flutter IntelliJ Plugin M57 釋出")
- [Flutter IntelliJ Plugin M58 釋出](https://groups.google.com/g/flutter-announce/c/WJUH0m6cu-U/m/_n0RltLFAAAJ "Flutter IntelliJ Plugin M58 釋出")
- [Flutter IntelliJ Plugin M59 釋出](https://groups.google.com/g/flutter-announce/c/CNzqxtybpBA/m/nSu7QabMAQAJ "Flutter IntelliJ Plugin M59 釋出")
- [Flutter IntelliJ Plugin M60 釋出](https://groups.google.com/g/flutter-announce/c/qc40yulxvAg/m/B_1HuGmoBQAJ "Flutter IntelliJ Plugin M60 釋出")

Flutter 的 Visual Studio Code 外掛在這個版本中也得到了改進，首先是兩個新的命令「Dart: Add Dependency」和「Dart: Add Dev Dependency」 ([#3306](https://github.com/Dart-Code/Dart-Code/issues/3306 "#3306")、[#3474](https://github.com/Dart-Code/Dart-Code/issues/3474 "#3474"))。

![在 Visual Studio Code 中新增 Dart 依賴](https://gglh6.g.forms.cn/zkRnoDFdsyVea3ukAArVqKLXz7u49QSi3ufrV3nlNpRSVxzY8XVcTQUjuf-iXS94nFetuyt4mVhxMpHylb0A2W6_jOPbm3YTKoO0ZFbSe_iYmrND3yk3eKx8baImfrtZeiVsgjn5=s0)

這些命令提供的功能與已經提供了一段時間的 [Jeroen Meijer 的 Pubspec Assist 外掛](https://marketplace.visualstudio.com/items?itemName=jeroen-meijer.pubspec-assist "Jeroen Meijer 的 Pubspec Assist 外掛") 類似。這些新命令開箱即用，提供了一個從 pub.dev 定期獲取的 package 的型別過濾列表。

你可能還對「Fix All」命令感興趣 ([#3445](https://github.com/Dart-Code/Dart-Code/issues/3445 "#3445")、[#3469](https://github.com/Dart-Code/Dart-Code/issues/3469 "#3469"))，該命令對 Dart 檔案可用，可以在一個步驟中修復所有與 [dart fix](https://dart.tw.gh.miniasp.com/tools/dart-fix "dart fix") 相同的問題。

![使用 Flutter Fix 規則來修復您程式碼中的所有已知問題](https://gglh6.g.forms.cn/zhC59kp2HFl_SK7jvmrFE_z6qrGxBNq1h8X3rjYiEKl4oRaTLF29P6lY1Mmh7F6tiERWHOd26jOKsc_kDDLWDeaEyToCSkI_0I75h4bk4av6jl4Ao013B11Bp6jaiQD5_5xQ-097=s0)

你也可以在 VS Code 中，透過在 `editor.codeActionsOnSave` 中新增 `source.fixAll` 來設定為儲存時執行。

又或者如果你想嘗試一下預覽功能，你可以啟用 `dart.previewVsCodeTestRunner` 設定，看到 Dart 和 Flutter 測試透過新的 Visual Studio Code 測試執行器執行。

![使用新的 Visual Studio Code 測試執行器測試你的 Dart 和 Flutter 程式碼](https://gglh6.g.forms.cn/lUZmN0Qnnrmatl12GQUmTHOFYLT92N-etnOYgGt2p4DZgZzEXVW-HuSKYer6ZeodMtG0zs8u3u3L5bvA_OEcSpkEsVBruU_dgzB1jffeepyxHa0rlisonTqYO2AWhiL4s1RxGvJo=s0)

Visual Studio Code 測試執行器看起來與當前的 Dart 和 Flutter 測試執行器有些不同，它會在不同的會話中顯示結果。Visual Studio Code 測試執行器還在編輯介面的左側增加了新的間距圖示 (Gutter icon)，顯示測試的執行結果狀態，可以點選它來執行測試 (或右鍵點選上下文選單)。

![](https://gglh6.g.forms.cn/OEFD6Au4bXaGrKy1XVn5TEYGP-y3m3Kt1h52trrwVYLec0ShiaESW3-ymgosGuAOqpn_KudJH-Xa_IOGYRj9g-nvF6Qn5CEiZSB6CEdvSa76c0Sbx2FNx5-JNjSXTuejGjNIlcJG=s0)

在之後的版本，現有的 Dart 和 Flutter 測試執行器將被移除，而採用新的 Visual Studio Code 測試執行器。

而這僅僅是 Visual Studio Code 外掛新功能和修正的冰山一角。所有詳情，請查閱下列釋出說明：

- [v3.26](https://dartcode.org/releases/v3-26/ "v3.26") VS Code Test Runner 整合，Flutter 建立設定，...
- [v3.25](https://dartcode.org/releases/v3-25/ "v3.25") 額外的依賴性管理改進，在檔案 / 儲存時修復所有，...
- [v3.24](https://dartcode.org/releases/v3-24/ "v3.24") 依賴關係樹的改進，更容易啟動配置，編輯器的改進
- [v3.23](https://dartcode.org/releases/v3-23/ "v3.23") 配置檔案模式的改進，改進依賴關係樹，改進 LSP

在以前的 Flutter 版本中，你可能會被那些你不希望處理的例外所困擾，你可能希望它們觸發偵錯程式並找出它們的源頭，但卻發現 Flutter 框架沒有讓例外透過來觸發偵錯程式中的「未處理的例外」處理程式。在這個版本中，偵錯程式現在可以正確地中斷未處理的例外，而以前這些例外只是被框架捕獲 ([#17007](https://github.com/flutter/flutter/issues/17007 "#17007"))。這改善了除錯的體驗，偵錯程式現在可以直接指向例外在程式碼中的丟擲行，而不是指向框架深處的一個隨機位置。與之相關的一個新功能是你能夠決定 FutureBuilder 是否應該重新丟擲或隱藏錯誤 ([#84308](https://github.com/flutter/flutter/pull/84308 "#84308"))。這應該會給你提供更多的例外，以幫助你追蹤 Flutter 應用中的問題。

自從 Flutter 誕生以來，就有了 Counter 應用範本，它有很多優點：它展示了 Dart 語言的很多特性，示範了幾個關鍵的 Flutter 概念，而且它足夠小，即使有很多解釋性的註釋，也能裝進一個檔案。然而，它並未對 Flutter 應用的實際使用場景提供一個特別好的展示。在這個版本中，你可以透過以下命令建立一個新的範本 ([#83530](https://github.com/flutter/flutter/pull/83530 "#83530"))。

`$ flutter create -t skeleton my_app`

![新的 Flutter Skeleton 範本示範](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-5/flutter-skeleton-demo.gif)

新的 Skeleton 範本，可產生包含兩頁的列表檢視 Flutter 應用 (帶詳細檢視)，並遵循社群最佳實踐。它的開發經過了大量的內部和外部評審，以提供一個更好的基礎來建構一個達到產品級品質的應用。它支援以下功能：

- 使用 ChangeNotifier 來協調多個小工具
- 預設情況下，使用 arb 檔案產生本地化。
- 包括一個範例影象，併為影象資源建立了 1x、2x 和 3x 資料夾。
- 使用「功能優先」的資料夾組織方式
- 支援 shared_preference
- 支援淺色和深色主題設計
- 支援多頁之間的導航

隨著時間的推移和 Flutter 最佳實踐的發展，希望這個新範本也能隨之發展。

如果你正在開發一個外掛而不是一個應用，你可能會對 Pigeon 1.0 版本感興趣。Pigeon 是一個程式碼產生器工具，用於產生 Flutter 和其宿主平台之間型別安全的互動程式碼。你可以定義外掛的 API 描述，併為 Dart 與 Java / Objective-C / Kotlin / Swift 產生範本程式碼。

![Pigeon 產生的範例程式碼](https://gglh6.g.forms.cn/uzONXOqW9s2Vmw05F_ZFdAV_ys59HRphftET69Ra_KHy0qvkTAbZUxNhpWCXvWXPUgGvZQyJ1gmAFLrBpxq1LRHbJG5yEmI9vs8fSppLYAlc_sa-pKsP-xSdZWxhWF7PqSh6WiEL=s0)

Pigeon 已經應用在 Flutter 團隊的一些外掛中。這個版本提供了更多有用的錯誤資訊，增加了對泛型、原始資料型別作為引數和返回型別以及多引數的支援，在未來它會被更廣泛地使用。如果你想在自己的外掛或 add-to-app 的專案中使用 Pigeon，請查閱 [pigeon 外掛頁面](https://pub.dev/packages/pigeon "pigeon 外掛頁面") 找到更多資訊。

以下是 Flutter 2.5 版本中的破壞性改動：

- [預設的裝置拖動和滾動](https://flutter.tw/release/breaking-changes/default-scroll-behavior-drag "預設的裝置拖動和滾動")
- [v2.2 版後刪除了廢棄的 API](https://flutter.tw/release/breaking-changes/2-2-deprecations "v2.2 版後刪除了廢棄的 API")
- [Package 介紹: flutter_lints](https://flutter.tw/release/breaking-changes/flutter-lints-package "Package 介紹: flutter_lints")
- [ThemeData 的 accent 屬性已被棄用](https://flutter.tw/release/breaking-changes/theme-data-accent-properties "ThemeData 的 accent 屬性已被棄用")
- [手勢識別器清理](https://flutter.tw/release/breaking-changes/gesture-recognizer-add-allowed-pointer "手勢識別器清理")
- [將 AnimationSheetBuilder.display 替換為 collate](https://flutter.tw/release/breaking-changes/animation-sheet-builder-display "將 AnimationSheetBuilder.display 替換為 collate")
- [使用 HTML 插槽在 Web 中渲染平臺視圖](https://flutter.tw/release/breaking-changes/platform-views-using-html-slots-web "使用 HTML 插槽在 Web 中渲染平臺視圖")
- [將 LogicalKeySet 遷移至 SingleActivator](https://github.com/flutter/flutter/pull/80756_yWRVR6Y "將 LogicalKeySet 遷移至 SingleActivator")

瞭解自 1.17 版本以來完整的破壞性改動列表，請參閱：[Flutter 文件網站](https://flutter.tw/release/breaking-changes "Flutter 文件網站")。

隨著我們繼續更新 Flutter Fix (可在 IDE 中使用，也可透過 `dart fix` 命令使用)，我們總共應用了 157 條規則，來遷移受破壞性改動以及任何棄用影響的程式碼。一如既往，我們非常感謝社群 [提供的測試](https://github.com/flutter/tests/blob/master/README.md "提供的測試")，幫助我們識別了這些破壞性改動。如需瞭解更多，請查閱： [Flutter 破壞性改動政策](https://github.com/flutter/flutter/wiki/Tree-hygiene#handling-breaking-changes "Flutter 破壞性改動政策")。

另外，隨著 Flutter 2.5 的釋出，我們將放棄對 iOS 8 的支援，[正如 2020 年 9 月宣佈](https://flutter.cn/go/rfc-ios8-deprecation "正如 2020 年 9 月宣佈") 的那樣。放棄對市場份額不足 1% 的 iOS 8 的支援，使 Flutter 團隊能夠專注於使用範圍更廣的新平台。棄用意味著這些平台可能可以正常使用 Flutter，但我們不會在這些平臺上測試新版本的 Flutter 或外掛。您可以在 [Flutter 文件網站](https://flutter.tw/development/tools/sdk/release-notes/supported-platforms "Flutter 文件網站") 上看到 [目前 Flutter 支援的平台列表](https://flutter.tw/development/tools/sdk/release-notes/supported-platforms "目前 Flutter 支援的平台列表")。

最後，一如既往地感謝世界各地的 Flutter 社群組織和社群成員們，是社群讓這一切成為可能。在本次更新中貢獻和稽核 1000 多個 PR 的數百位開發者，因為有你們每個人的努力才成就了本次的成果。讓我們攜手共同努力，為世界各地的開發者共同轉變應用的開發流程，讓開發者們可以從一個程式碼庫中交付更多應用、更快開發、部署到更多你所關心的平台。

敬請關注 Flutter 團隊的更多更新，這一年還沒有結束，拭目以待！

*感謝 flutter.cn 社群成員 (@AlexV525、@Vadaski、@MeandNi) 以及 Yuan 和 Lynn 對本文的審校和貢獻。*
