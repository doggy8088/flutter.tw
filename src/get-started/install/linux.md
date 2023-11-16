---
title: Linux Installation Guide
title: 在 Linux 作業系統上安裝和配置 Flutter 開發環境
description: Learn how to install Flutter on Linux.
description: 學習如何在 Linux 上安裝 Flutter。
short-title: Linux
tags: Flutter安裝,Flutter環境搭建
keywords: Flutter Linux,Flutter macOS,Flutter鏡像,Linux開發Flutter
next:
  title: Set up an editor
  title: 編輯工具設定
  path: /docs/get-started/editor
---

{% assign os = 'linux' -%}

## System requirements

## 系統配置要求

To install and run Flutter,
your Linux development environment needs to meet these minimum requirements:

要想安裝和執行 Flutter，你的開發環境至少應該滿足如下的需求：

- **Operating System**: Linux (64-bit)

  **作業系統**: Linux (64 位)

- **Disk Space**: At least 1.6 GB (excluding disk space for IDE/tools).

  **磁碟空間**: 1.6 GB (不包含安裝 IDE 和其他工具的空間)

- **Tools**: Flutter relies on these command-line tools:

  **命令工具**: Flutter 需要以下命令列工具：

  - `bash`
  - `curl`
  - `file`
  - `git` 2.x
  - `mkdir`
  - `rm`
  - `unzip`
  - `which`
  - `xz-utils`
  - `zip`

- **Shared libraries**: To utilize the `flutter test` command,
  your environment needs the library `libGLU.so.1`.
  The `mesa` packages provide this library:
  `libglu1-mesa` on Ubuntu/Debian and `mesa-libGLU` on Fedora.

  **公用庫**: Flutter 的 `test` 命令需要 `libGLU.so.1` 庫。
  `mesa` 套件已經包含了這個庫：
  在 Ubuntu/Debian 上是 `libglu1-mesa`，在 Fedora 上是 `mesa-libGLU`。

{% include_relative _get-sdk-linux.md %}

{% include_relative _path-linux-chromeos.md %}

{% include_relative _android-setup.md %}

{% include_relative _linux-desktop-setup.md %}

## Next step

## 下一步

Set up your preferred editor.

編輯器設定。

