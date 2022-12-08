---
title: Dart 2.16 現已釋出
toc: true
---

*文 / Michael Thomsen, Dart 產品經理*

## Dart 2.16 正式釋出

Dart 2.16 正式版已於上週釋出。儘管沒有新的語言特性加入，但本次版本釋出包含了數個問題修復 (包括對安全漏洞的修復)，釋出 Dart package 的時候也可以指定支援的平台，pub.dev 網站也更新了全新的搜尋介面。

![](https://devrel.andfun.cn/devrel/posts/2022/02/d7ae376ac7664.png)

與 [Flutter 2.10](https://mp.weixin.qq.com/s/FgMu6-O_wMkwxp2yxiW2Ew) 一同釋出的 Dart 2.16，仍然在將舊的命令列工具遷移到新的 dart 命令列工具。這個版本中，`dartdoc`和 `dartanalyzer` 已被棄用，分別對應新的命令是 `dart doc` 和 `dart analyze`。`dartdoc`、`dartanalyzer` 和 `pub` 這三個命令計劃在 Dart 2.17 中徹底移除。

| 歷史命令                            | 替代的 dart 命令 | 棄用版本 | 停用版本 |
|-------------------------------------|------------------|-------------|-----------------|
| stagehand                           | dart create    | 2.14        | 2.14           |
| dartfmt                             | dart format      | 2.14        | 2.15            |
| dart2native                         | dart compile exe | 2.14        | 2.15            |
| dart2js                             | dart compile js  | 2.17        | 2.18            |
| dartdevc                            | none             | 2.17        | 2.18            |
| dartanalyzer                        | dart analyze     | 2.16        | 2.17            |
| package:analyzer_cli                        | dart analyze     | 2.16        | 2.17            |
| dartdoc                             | dart doc       | 2.16        | 2.17            |
| pub                                 | dart pub         | 2.15        | 2.17            |

檢視所有計劃棄用的命令的更多說明，請參考 [Dart SDK 儲存庫的 Issue #46100](https://github.com/dart-lang/sdk/issues/46100 "Dart SDK 儲存庫的 Issue #46100")。 

Dart 2.16 也包含了一個安全漏洞的修復，以及兩個破壞性改動：

- `dart:io` 中的 HttpClient API 現在可以設定 `authroization`、`www-authenticate`、`cookie`和`cookie2`這些請求頭資訊。在 Dart 2.16 之前，SDK 中重新導向邏輯的實現有一個漏洞，當跨域重新導向發生時，這些請求頭 (可能包含敏感資訊) 會被髮送，在 Dart 2.16 中這些請求頭已被移除，你可以閱讀 [CVE-2022–0451](https://github.com/dart-lang/sdk/security/advisories/GHSA-c8mh-jj22-xg5h "CVE-2022–0451") 瞭解更多細節。
- `dart:io`中的 `Directory.rename` API 調整了在 Windows 平臺上的行為：與目標名稱一致的目錄不會被刪除 (issue [#47653](https://github.com/dart-lang/sdk/issues/47653 "Dart SDK 儲存庫的 Issue #47653"))。
- 在 Dart 1.x 中遺留的 `Platform.packageRoot` 和 `Isolate.packageRoot` 已被移除 (issue [#47769](https://github.com/dart-lang/sdk/issues/47769 "Dart SDK 儲存庫的 Issue #47769"))，它們在 Dart 2.x 中不起作用。

想要了解更多關於 Dart 2.16 的改動，可以查閱 [更新日誌](https://github.com/dart-lang/sdk/blob/master/CHANGELOG.md#2160 "Dart 2.16 詳細更新日誌")。

## 在 pub.dev 上宣告 package 支援的平台

Dart 的設計思想包含了可移植性，因此我們會盡量讓程式碼能夠在更多平臺上執行。然而，開發者們偶爾可能會在 pub.dev 上建立或分享僅為一個或幾個平台設計的 package。你可能有一個依賴於特定作業系統上 API 的 package，或者它使用了像 `dart:ffi` 這類只能在 native 平台而不能在 Web 平臺上使用的函式庫等。

在 Dart 2.16 版本中，你可以在 pubspec 手動宣告 package 支援的平台。例如，如果你的 package 只支援 Windows 和 macOS，那麼就可以在 `pubspec.yaml` 這樣宣告:

```
name: mypackage  
version: 1.0.0

platforms:  
  windows:  
  macos:

dependencies:
```

當你正在開發一個 Dart package，而它支援的平台與 pub.dev 自動識別的不同時，可以在新的 `platforms` 標籤處手動宣告。如果你正在開發和分享包含特定平台程式碼的 Flutter 外掛 (例如，Kotlin 或 Swift)，[Flutter plugin 標籤](https://flutter.tw/development/packages-and-plugins/developing-packages#plugin-platforms "Flutter plugin 標籤") 可以指定支援的平台。

## pub.dev 新的搜尋體驗

響應開發者的請求，我們對於在 pub.dev 上的搜尋提供了更好的支援。此次更改的主要目的是幫助你區分和搜尋支援的平台。以下是新的搜尋介面預覽:

![Pub.dev 的搜尋介面，側邊欄包含 Platforms、SDKs 和高階選項](https://devrel.andfun.cn/devrel/posts/2022/02/35e594d3f2d5c.jpg)

新的搜尋介面在左側有一個搜尋篩選欄，你可以用它限制搜尋範圍:

- **Platforms (平台)**: 選擇一個或多個平台，搜尋結果僅會包含支援所選平台的 package；
- **SDKs**: 選擇 Dart 或 Flutter 將結果限制為支援 Dart SDK 或 Flutter SDK 的 package；
- **Advanced (高階選項)**: 額外的搜尋選項，例如篩選出被標記為 Flutter Favorite 的 packages。

## 空安全進展

從一年前我們在 [Dart 2.12](https://mp.weixin.qq.com/s/OA0bTnR9o4eN_hyxTqaayA) 中正式釋出了健全的空安全開始，社群以及 Dart 生態向空安全遷移的速度令人震撼：截止到上週，排名前 250 的 package 已經 100% 支援了空安全，排名前 1000 的 pacakge 也已經有 96% 的比例支援了空安全！謝謝所有為空安全貢獻的 package 作者們！

應用遷移到空安全的遷移進度也十分樂觀，這裡指應用以及其所有的依賴都支援空安全。根據我們的統計，71% 的 Flutter tool 都執行在健全的空安全模式下了，如果你還沒有開始，現在已經可以行動啦！

## 即將到來的內容

我們希望 pub.dev 的搜尋介面更新對你有幫助，同時歡迎隨時 [提出反饋](https://github.com/dart-lang/pub-dev/issues/ "向 pub.deb 提出建議和反饋")。2022 年二季度我們計劃釋出下一個 Dart SDK 版本，並且，我們還在跟進一些 [令人興奮的語言特性](https://github.com/dart-lang/language/projects/1 "Dart 語言新特性計劃看板")，希望能在今年晚些時候釋出。

## 致謝
- 原文: Dart 2.16: Improved tooling and platform handling
- 連結: https://medium.com/dartlang/dart-2-16-improved-tooling-and-platform-handling-dd87abd6bad1
- 翻譯 / 審校: CFUG 團隊 Alex、加康、迷鹿
- 製圖: Lynn
