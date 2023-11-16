---
title: GitHub 供應鏈安全已支援 Dart 開發者生態
toc: true
keywords: GitHub, Dart, 供應鏈安全
description: 透過 Dart 和 GitHub 團隊的共同努力，自 10 月 7 日起，GitHub 的 Advisory Database (安全諮詢資料庫)、Dependency Graph (依賴項關係圖) 和 Dependabot (依賴更新機器人) 開始支援 Dart 開發者生態，這也意味著 GitHub 為 Dart 和 Flutter 應用的供應鏈安全提供了全面支援。
image:
    path: https://devrel.andfun.cn/devrel/posts/2022/10/39fdff31f85d9.png
---

文/ Michael Thomsen，Dart 產品經理

透過 Dart 和 GitHub 團隊的共同努力，自 10 月 7 日起，GitHub 的 Advisory Database (安全諮詢資料庫)、Dependency Graph (依賴項關係圖) 和 Dependabot (依賴更新機器人) 開始支援 Dart 開發者生態，這也意味著 GitHub 為 Dart 和 Flutter 應用的供應鏈安全提供了全面支援:

- GitHub 的 Advisory Database (安全諮詢資料庫) 為漏洞報告者和專案維護者之間提供了一個協作平台，漏洞報告者和專案維護者可以共同合作，在漏洞被公開之前私密討論並修復漏洞。
- Dependency Graph (依賴項關係圖) 主要是分析 Dart / Flutter 專案的 pubspec.yaml 和 pubspec.lock 檔案來確定專案依賴關係。
- Dependabot 是 GitHub 收購併免費開放的一個檢測依賴項安全性的工具，一旦你依賴的 Dart package 版本發現新漏洞時，Dependabot 就可以發出通知並自動建立拉取請求 (Pull Request)，將 package 版本升級到沒有漏洞的版本。檢視過往推文: [Dependabot 開始支援 pub package 版本檢測](https://flutter.cn/posts/pub-beta-support-for-dependabot-version-updates) 瞭解更多。

Dart 產品經理 Michael Thomsen 表示：透過與 GitHub 團隊的合作，Dart 開發者們可以在新的漏洞影響到客戶之前發現和解決問題；GitHub 的高階產品經理 Courtney Claessens 也提到說，在供應鏈安全側全面支援 Dart，不僅是對開源社群、開發者的支援，更能夠幫助數百萬使用 Dart 應用的使用者們。

![Dependabot 會檢測 package 的更新並建立拉取請求以請求更新到最新版本](https://devrel.andfun.cn/devrel/posts/2022/10/18646812c0ce1.jpg)

![Dependency Graph (依賴項關係圖) 會展示出專案所依賴的其他 package](https://devrel.andfun.cn/devrel/posts/2022/10/813aca34d90a0.jpg)

### 釋出 package 到 pub.dev 的安全最佳實踐

作為 package 開發者或維護者，當你將 package 釋出到 pub.dev 的時候，這裡有兩條最佳實踐的建議:
1. 使用 GitHub 的安全公告功能在你的程式碼儲存庫中建立新的安全公告，GitHub 會將這個納入其 Advisory Database (安全諮詢資料庫) 中。
1. 為你的 GitHub 程式碼儲存庫配置安全策略，詳細說明使用者可以用什麼樣的方式報告安全問題。

![為釋出到 pub.dev 上的 package 建立安全公告 (Ecosystem 選擇 Pub)](https://devrel.andfun.cn/devrel/posts/2022/10/ebd6c66d2ea67.jpg)

上述提到的這些安全策略和功能均已面向所有使用者釋出 (私有儲存庫也只需要加入一點的額外配置)，快去試試吧，保護自己的程式碼安全，刻不容緩。

### 延伸閱讀

- [在程式碼儲存庫中建立新的安全公告](https://docs.github.com/cn/code-security/repository-security-advisories/creating-a-repository-security-advisory)
- [關於安全漏洞的協調披露](https://docs.github.com/cn/code-security/repository-security-advisories/about-coordinated-disclosure-of-security-vulnerabilities)
