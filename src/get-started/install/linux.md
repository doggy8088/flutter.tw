---
title: Linux install
title: 在 Linux 作業系統上安裝和配置 Flutter 開發環境
description: How to install on Linux.
description: 如何在 Linux 上安裝 Flutter。
short-title: Linux
tags: Flutter安裝,Flutter環境搭建
keywords: Flutter Linux,Flutter macOS,Flutter映象,Linux開發Flutter
next:
  title: Set up an editor
  title: 編輯工具設定
  path: /docs/get-started/editor
---

{% assign os = 'linux' -%}

## System requirements

## 系統配置要求

To install and run Flutter,
your development environment must meet these minimum requirements:

要想安裝和執行 Flutter，你的開發環境至少應該滿足如下的需求：

- **Operating Systems**: Linux (64-bit)

  **作業系統**: Linux (64 位)

- **Disk Space**: 600 MB (does not include disk space for IDE/tools).

  **磁碟空間**: 600MB (不包含安裝 IDE 和其他工具的空間)

- **Tools**: Flutter depends on these command-line tools being available
  in your environment.
 
  **命令工具**: Flutter 需要你的開發環境中已經配置了以下命令列工具。

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

- **Shared libraries**: Flutter `test` command depends on this library
  being available in your environment.

  **公用庫**: Flutter 的 `test` 命令需要你的系統安裝或存在如下的公用庫。

  - `libGLU.so.1` - provided by mesa packages such as `libglu1-mesa` on
     Ubuntu/Debian and `mesa-libGLU` on Fedora.

    `libGLU.so.1` - 由 mesa 套件 (packages) 提供，
    比如 Ubuntu/Debian 系統下的 `libglu1-mesa`，
    以及 Fedora 系統下的 `mesa-libGLU`。

{% include_relative _get-sdk-linux.md %}

{% include_relative _path-linux-chromeos.md %}

{% include_relative _android-setup.md %}

{% include_relative _linux-desktop-setup.md %}

{% include_relative _web-setup.md %}

## Next step

## 下一步

Set up your preferred editor.

編輯器設定。

