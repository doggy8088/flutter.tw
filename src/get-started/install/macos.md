---
title: macOS install
title: 在 macOS 上安裝和配置 Flutter 開發環境
description: How to install on macOS.
description: 如何在 macOS 上安裝 Flutter。
short-title: macOS
tags: Flutter安裝,Flutter環境搭建
keywords: Flutter macOS,Flutter鏡像,macOS開發Flutter,macOS開發環境配置
next:
  title: Set up an editor
  title: 編輯工具設定
  path: /docs/get-started/editor
---

{% assign os = 'macos' -%}

## System requirements

## 系統配置要求

To install and run Flutter,
your development environment must meet these minimum requirements:

想要安裝並執行 Flutter，你的開發環境需要最低滿足以下要求：

- **Operating Systems**: macOS, version 10.14 (Mojave) or later.

  **作業系統**：macOS，需要 10.14 (Mojave) 以及以上版本的作業系統

- **Disk Space**: 2.8 GB (does not include disk space for IDE/tools).

  **磁碟空間**：2.8 GB（不包含 IDE 或其餘工具所需要的磁碟空間） 

- **Tools**: Flutter uses `git` for installation and upgrade. We recommend
  installing [Xcode][], which includes `git`, but you can also 
  [install `git` separately][]. 

  **工具**：Flutter 使用 `git` 進行安裝和升級，我們建議您安裝包含了 `git` 的 Xcode，
  或者您也可以 [單獨安裝 `git`][install `git` separately]。

{{site.alert.important}}

  If you're installing on an [Apple Silicon Mac][], you must have the Rosetta
  translation environment available for [some ancillary tools]. 
  You can install this manually by running:

  如果你要在 [Apple 晶片的 Mac 電腦][] 上使用，你還需要安裝
  Rosetta 2 環境因為 [一些輔助工具][some ancillary tools] 仍然需要，
  你可以透過手動執行下面的命令來安裝：
  ```terminal
$ sudo softwareupdate --install-rosetta --agree-to-license
  ```
{{site.alert.end}}

{% include_relative _get-sdk-mac.md %}

{% include_relative _path-mac.md %}

## Platform setup

## 平台配置

macOS supports developing Flutter apps for iOS, Android, macOS itself 
and the web. Complete at least one of the platform setup steps now,
to be able to build and run your first Flutter app.

macOS 系統可以開發支援 iOS、Android、macOS 桌面以及 Web 的 Flutter 應用，
你可以任選一個平台完成初始配置，以此來搭建並執行你的第一個 Flutter 應用。

{% include_relative _ios-setup.md %}

{% include_relative _android-setup.md %}

{% include_relative _macos-desktop-setup.md %}

## Next step

## 下一步

Set up your preferred editor.

編輯器設定。

[Apple Silicon Mac]: https://support.apple.com/en-us/HT211814
[Apple 晶片的 Mac 電腦]: https://support.apple.com/zh-cn/HT211814
[some ancillary tools]: https://github.com/flutter/website/pull/7119#issuecomment-1124537969
[these supplementary notes]: {{site.repo.flutter}}/wiki/Developing-with-Flutter-on-Apple-Silicon
[Xcode]: {{site.apple-dev}}/xcode/
[install `git` separately]: https://git-scm.com/download/mac