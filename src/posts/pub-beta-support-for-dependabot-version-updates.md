---
title: Dependabot 開始支援 pub package 版本檢測
toc: true
---

![](https://files.flutter-io.cn/posts/flutter-cn/2022/pub-beta-support-for-dependabot-version-updates/pub-dependabot-hero.png)

今年年初，我們釋出了 [Flutter 2022 產品路線圖](https://flutter.cn/posts/flutter-2022-roadmap)，其中「基礎設施建設」這部分提到：2022 年 Flutter 團隊將增加對供應鏈的安全的投入，目的是達到符合基礎設施 SLSA 4 級別中描述的要求。

## 供應鏈安全

大多數開源專案依賴了數百個開源依賴項<sup>[[1]](https://go2.gdsub.com/ospd "GitHub 文件 - 供應鏈安全: 大多數開源專案依賴了數百個開源依賴項")</sup>，隨著更多開源專案被更廣泛地使用，這些依賴項給其使用者們帶來了安全隱患：如果我們使用的開源專案的依賴項受到攻擊和破壞該怎麼辦？這將讓你的直接使用者 (軟體的最終使用者) 蒙受供應鏈攻擊帶來的風險。

供應鏈攻擊是一種新興的、針對軟體開發者和供應商的安全威脅<sup>[[2]](https://go2.gdsub.com/scad "Microsoft 文件 - 供應鏈攻擊: 供應鏈攻擊是一種新興的、針對軟體開發者和供應商的安全威脅")</sup>，攻擊者會透過尋找不安全的網路協議、不受保護的伺服器以及不安全的程式碼等安全漏洞並更改程式碼，導致使用它的程式在建構和更新的過程中被加入隱藏的惡意程式碼。

為了確保依賴項供應鏈的安全性，開發者們需要確保所有依賴項和工具定期更新到最新穩定版本，因為這些新的版本通常會包含最新功能和已知漏洞的安全修補程式。依賴項包括依賴的程式碼、使用的二進位制檔案、使用的工具以及其他元件等<sup>[[3]](https://docs.microsoft.com/zh-cn/nuget/concepts/security-best-practices#dependency-versions "Microsoft 文件 - 關於安全軟體供應鏈的最佳做法")</sup>。在 GitHub 上託管的開原始碼可以使用 GitHub 提供的程式碼掃描功能來查詢專案中的安全漏洞和錯誤，並使用 Dependabot 維護專案的依賴項，以確保專案自動更新到依賴項的最新版本。

## 啟用 package 版本檢測

對 Dart package 的支援可以回溯至 19 年 4 月初，當時的 Flutter 剛剛釋出到 [v1.2 穩定版](https://flutter.cn/posts/launching-flutter-12-at-mobile-world)；同年 5 月，Dependabot 被 GitHub 收購併免費為開發者提供服務。有一位社群成員在 [dependabot-core#2166](https://github.com/dependabot/dependabot-core/issues/2166 "GitHub 議題: dependabot-core#2166") 這個議題 (issue) 中發起提問，希望 Dependabot 加入對 Flutter / Dart 的支援。透過 Dependabot 與 Dart 團隊的共同努力，包括後期為 Flutter 命令列工具加入了一些對 package 版本檢測等功能，最後在今年 3 月 22 日開啟封閉測試，並於 4 月 5 日進行公開的 beta 測試。

目前，Dependabot 版本更新對 pub package 生態的支援已進入測試階段，開發者們可以使用 Dependabot 為他們的 Flutter 或 Dart 專案加入 package 的更新檢測。目前僅支援已釋出到 pub.dev 網站上的 package。

如果想在自己的 Dart 或 Flutter 儲存庫測試 Dependabot 版本更新檢測，需要建立 `.github/dependabot.yml` 檔案並新增如下內容：

```yaml
version: 2
enable-beta-ecosystems: true
updates:
  - package-ecosystem: "pub"
    directory: "/"
    schedule:
      interval: "weekly"
```

請確保這兩個引數設定: `package-ecosystem: "pub"` 以及 `enable-beta-ecosystems: true`。

## 已知限制

目前的 pub package 版本檢測支援仍處於測試階段，幷包含了一些已知的限制，例如 [Dependabot 安全更新](https://docs.github.com/cn/code-security/dependabot/dependabot-security-updates/about-dependabot-security-updates "GitHub 文件 - Dependabot 安全更新功能說明") 目前尚不支援，將在未來發布的版本中加入對其的支援。

其他已知的限制：

- 不支援更新以 git 方式參考的依賴
- 如果在 dependabot 中配置忽略新版本檢測，將不會進行任何更新
- 不支援檢測私享和自訂 pub 釋出的 package

開發者們可以在 [GitHub 官方的反饋討論](https://github.com/github/feedback/discussions/14200 "提出反饋: Dependabot 開始支援 pub package 檢測") 裡提出建議或 vote +1，也可以在 [dependabot-core](https://go2.gdsub.com/pub-issues-dependabot "在 dependabot-core 儲存庫提交關於 pub 支援的議題") 儲存庫提交議題來幫助團隊排查問題。

## 致謝

- 訊息源: GitHub 部落格 - [pub beta support for Dependabot version updates](https://github.blog/changelog/2022-04-05-pub-beta-support-for-dependabot-version-updates/)
- 編輯: CFUG 本地化團隊 Alex, Luke
- 製圖: Lynn
