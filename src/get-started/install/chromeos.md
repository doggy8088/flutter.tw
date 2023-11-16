---
title: ChromeOS install
title: 在 ChromeOS 上安裝和配置 Flutter 開發環境
description: How to install on ChromeOS.
description: 如何在 ChromeOS 上安裝 Flutter。
short-title: ChromeOS
tags: Flutter安裝,Flutter環境搭建
keywords: Flutter ChromsOS,ChromeOS上安裝Flutter,ChromeOS開發
next:
  title: Set up an editor
  titie: 編輯工具設定
  path: /docs/get-started/editor
---

{% assign os = 'linux' -%}

## System requirements

## 系統要求

To install and run Flutter on a Chromebook, your machine
must have [Linux][] enabled from the **Developers** tab of Settings.

要在 Chromebook 上安裝並執行 Flutter，
你的裝置必須在設定的 **開發者** 選項卡中啟用內建的 [Linux][] 環境。

The amount of disk space required varies
depending on which target platforms you enable.
We recommend that you increase the disk size for the
Linux environment from the default of 10GB to 32GB or larger,
to accommodate Android Studio and other tooling.

所需的磁碟空間大小會根據你啟用的目標平台而變化。
我們建議你將 Linux 環境的磁碟大小從預設的 10GB 增大到 32GB 或更大，
以容納 Android Studio 和其他工具。

{% include_relative _get-sdk-chromeos.md %}

{% include_relative _chrome-setup-chromeos.md %}

{% include_relative _android-setup-chromeos.md %}

## Next step

## 下一步

Set up your preferred editor.

編輯器設定。

[Linux (Beta)]: https://support.google.com/chromebook/answer/9145439
[Linux]: https://support.google.com/chromebook/answer/9145439
