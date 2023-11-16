---
title: Flutter Cocoon 已達到 SLSA 2 級標準的要求
toc: true
keywords: Cocoon, SLSA2
description: 
image:
    path: https://devrel.andfun.cn/devrel/posts/2022/10/c4f9568a65b61.jpg
---

文/ Jesse Seales, Dart 和 Flutter 安全工作組工程師

今年年初，我們釋出了 [Flutter 2022 產品路線圖](https://flutter.cn/posts/flutter-2022-roadmap)，其中「基礎設施建設」這部分提到：2022 年 Flutter 團隊將增加對供應鏈的安全的投入，目的是達到符合基礎設施 SLSA 4 級別中描述的要求。4 月份下旬，Dart 團隊與 GitHub 合作，[Dependabot 開始支援 pub.dev 上的 package 版本檢測](https://flutter.cn/posts/pub-beta-support-for-dependabot-version-updates)。

Cocoon 是一個管理 Flutter Infra CI 的工具應用，我們已經實現了提升到 SLSA 2 級標準所要求的內容，將身份識別和存取管理 (Identity and Access Management, IAM) 許可權降低到所需的最低許可權，並對部分應用許可權管理實施了基礎設施即程式碼 (Infrastructure as code, IaC) 的策略。

>> 身份識別和存取管理 (IAM) 是一種安全許可權措施，主要用於存取敏感技術資源時，為不同需要的人授權不同的許可權。基礎設施即程式碼 (IaC) 的核心思路是透過程式碼而不是用手動的方式和流程來管理專案的基礎設施。

## 亮點

Cocoon 不僅可以管理 Flutter Infra CI，還可以將多個 CI 服務與 GitHub 整合，使得團隊在 GitHub 上開發變得更容易。Cocoon 通過了 SLSA 2 級的要求，這意味著 Cocoon 已經解決了 SLSA 1 和 2 級別中所有的安全問題。Google 的開源安全團隊已經驗證並稽核了 Cocoon 具備 SLSA 2 級標準的要求。

![](https://devrel.andfun.cn/devrel/posts/2022/10/99938ae2c6677.png)

我們為 `docs-flutter-dev`、`master-docs-flutter-dev` 和 `flutter-dashboard` 實施了額外的安全強化措施，使用基礎設施即程式碼 (IaC) 系統實現身份識別和存取管理 (IAM)。這幾個專案非常重要，他們為 Flutter 提供開發文件以及 Flutter 建構狀態的儀表盤等。在 IaC 系統的管理下，安全許可權的更改需要改動程式碼，沒有批准則無法進行任何改動。也就是說，安全許可權的改變是要透過版本控制系統來修改程式碼的，並且需要提供改變的理由。現有的 IAM 許可權將會被減弱以遵循最小許可權原則 (Principle of least privilege)。

## 優勢

- 為 Cocoon 實現 SLSA 2 級的要求內容，意味 Cocoon 的供應鏈具有「針對特定威脅的額外抵禦能力」；
- Cocoon 的自動建構流程為 flutter-dashboard 和 auto-submit 提供了原始碼出處和防篡改的建構證明，這有助於加強建構流程中使用多種工具的安全性，如 Google Cloud Platform、Cloudbuild、App Engine 和 Artifact Registry；
- 整體看，我們已經為 Cocoon 解決了所有級別要求中 83% 的內容，並已經敲定了為了滿足每個 SLSA 級別的合規性工作，我們也做好 Cocoon 邁向 SLSA 4 級要求的準備。

## 經驗總結和最佳實踐

1. 透過 Google Cloud Build 服務，我們僅做了相對較小的改動就提升了 Cocoon 建構過程中的供應鏈安全性，因為元資料驗證會在 Cloud Build 過程中自動驗證。
1. 透過程式碼來管理和調整 IAM 許可權會帶來很多額外的好處，並且可以使首次存取許可權的授權變得更簡單。
1. 提高或“升級” SLSA 的不同等級規範有時需要根據應用的建構流程等因素做出不同的投入。在爭取達到最高級別 (SLSA 4) 的過程中會需要做很多不同於其他等級 (比如 SLSA 2 級) 的更改。

## 展望下一步

這將是 Flutter 和 Dart 走向更高 SLSA 級別要求的開始，我們希望可以將其中的收穫實踐到更多的應用中，同時也希望開始為 `flutter/flutter` 這樣更復雜的程式碼庫進行 SLSA 2 級以上的改造工作，同時也希望 Cocoon 應用可以達到更高水平的 SLSA 合規等級。
