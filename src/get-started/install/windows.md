---
title: Windows install
title: 在 Windows 作業系統上安裝和配置 Flutter 開發環境
description: How to install on Windows.
description: 如何在 Windows 上安裝 Flutter。
short-title: Windows
tags: Flutter安裝,Flutter環境搭建
keywords: Flutter Windows,Flutter映象,Windows開發Flutter,Windows開發環境配置
next:
  title: Set up an editor
  title: 編輯工具設定
  path: /docs/get-started/editor
---

{% assign os = 'windows' -%}

## System requirements

## 系統配置要求

To install and run Flutter,
your development environment must meet these minimum requirements:

要想安裝和執行 Flutter，你的開發環境至少應該滿足如下的需求：

- **Operating Systems**: Windows 10 or later (64-bit), x86-64 based

  **作業系統**：Windows 10 或更高的版本（基於 x86-64 的 64 位作業系統）。

- **Disk Space**: 1.64 GB (does not include disk space for IDE/tools).

  **磁碟空間**：除安裝 IDE 和一些工具之外還應有至少 1.64 GB 的空間。

- **Settings**: Developer Mode must be enabled on Windows 10/11.

  **設定**: 必須在 Windows 10/11 上啟用開發者模式。

- **Tools**: Flutter depends on these tools being available in your environment.

  **工具**：要讓 Flutter 在你的開發環境中正常使用，依賴於以下的工具：

  - [Windows PowerShell 5.0][] or newer (this is pre-installed with Windows 10)

    [Windows PowerShell 5.0][] 或者更高的版本（Windows 10 中已預裝）

  - [Git for Windows][] 2.x, with the
    **Use Git from the Windows Command Prompt** option.

    [Git for Windows][] 2.x，並且勾選**從 Windows 命令提示符使用 Git** 選項。

     If Git for Windows is already installed,
     make sure you can run `git` commands from the
     command prompt or PowerShell.

     如果 Windows 版的 Git 已經安裝過了，那麼請確保能從命令提示符或者
     PowerShell 中直接執行 git 命令。

{% include_relative _get-sdk-win.md %}

{% include_relative _android-setup.md %}

{% include_relative _windows-desktop-setup.md %}

{% include_relative _web-setup.md %}

## Next step

## 下一步

Set up your preferred editor.

編輯工具設定。

[Git for Windows]: https://git-scm.com/download/win
[Windows PowerShell 5.0]: https://docs.microsoft.com/en-us/powershell/scripting/install/installing-windows-powershell
