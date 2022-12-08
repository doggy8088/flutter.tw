---
title: Set up an editor
title: 編輯工具設定
description: Configuring an IDE for Flutter.
description: 為 Flutter 配置 IDE 環境。
tags: Flutter安裝,Flutter起步課程
keywords: Flutter編輯工具,IDE配置
prev:
  title: Install
  title: 安裝和環境配置
  path: /docs/get-started/install
next:
  title: Test drive
  title: 開發體驗初探
  path: /docs/get-started/test-drive
toc: false
---

You can build apps with Flutter using any text editor
combined with Flutter's command-line tools.
However, we recommend using one of our editor
plugins for an even better experience.
These plugins provide you with code completion, syntax
highlighting, widget editing assists, run & debug support, and more.

你可以使用任意文字編輯器，結合我們的命令列工具來開發 Flutter 應用。
然而，我們推薦使用我們的編輯器外掛以獲取更好的開發體驗。這些外掛提供了程式碼自動完成、
程式碼高亮、widget 輔助編輯的功能，以及為專案的執行和除錯提供支援等。

Use the following steps to add an editor plugin for VS Code,
Android Studio, IntelliJ, or Emacs.
If you want to use a different editor,
that's OK, skip ahead to the [next step: Test drive][].

參考以下步驟為 VS Code、Android Studio、IntelliJ 或者 Emacs 新增編輯器外掛。
如果你想使用其他的編輯器，請直接開啟
[下一節: 開發體驗初探][next step: Test drive]，
來檢視使用其他文字編輯器配合命令列工具來建立和執行 Flutter 應用。

{% comment %} Nav tabs {% endcomment -%}
<ul class="nav nav-tabs" id="editor-setup" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="vscode-tab" href="#vscode" role="tab" aria-controls="vscode" aria-selected="true">Visual Studio Code</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="androidstudio-tab" href="#androidstudio" role="tab" aria-controls="androidstudio" aria-selected="false">Android Studio 和 IntelliJ</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="emacs-tab" href="#emacs" role="tab" aria-controls="emacs" aria-selected="false">Emacs</a>
  </li>
</ul>

{% comment %} Tab panes {% endcomment -%}
<div class="tab-content">
<div class="tab-pane active" id="vscode" role="tabpanel" aria-labelledby="vscode-tab" markdown="1">

## Install VS Code

## 安裝 VS Code

VS Code is a lightweight editor with complete Flutter
app execution and debug support.

VS Code 是一個可以執行和除錯 Flutter 的輕量級編輯器。

* [VS Code][], latest stable version

  [VS Code][]，最新穩定版本

## Install the Flutter and Dart plugins

## 安裝 Flutter 和 Dart 外掛

 1. Start VS Code.

    開啟 VS Code。

 1. Invoke **View > Command Palette...**.

    開啟 **View > Command Palette...**。

 1. Type "install", and select
    **Extensions: Install Extensions**.

    輸入「install」，然後選擇
    **Extensions: Install Extensions**。

 1. Type "flutter" in the extensions search field,
    select **Flutter** in the list, and click **Install**.
    This also installs the required Dart plugin.

    在擴充搜尋輸入框中輸入「flutter」，然後在列表中選擇 **Flutter** 並單擊 **Install**。
    此過程中會自動安裝必需的 Dart 外掛。

 1. Click **Reload to Activate** to reload VS Code.

    點選 **Reload to Activate** 以重新啟動 VS Code。

## Validate your setup with the Flutter Doctor

## 透過 Flutter Doctor 命令驗證是否安裝成功

 1. Invoke **View > Command Palette...**.

    開啟 **View > Command Palette...**。

 1. Type "doctor", and select the
    **Flutter: Run Flutter Doctor**.

    輸入 "doctor"，選擇
    **Flutter: Run Flutter Doctor**。

 1. Review the output in the **OUTPUT** pane for any issues.
    Make sure to select Flutter from the dropdown
    in the different Output Options.

    開啟 **OUTPUT** 面板檢視是否有錯誤，
    確保在不同的輸出選項 (Output Options) 
    的下拉列表中選擇了 Flutter。

</div>
<div class="tab-pane" id="androidstudio" role="tabpanel" aria-labelledby="androidstudio-tab" markdown="1">

## Install Android Studio

## 安裝 Android Studio

Android Studio offers a complete,
integrated IDE experience for Flutter.

Android Studio 為 Flutter 提供了一個完整的整合開發環境。

* [Android Studio][], version 2020.3.1 (Arctic Fox) or later

  [Android Studio](https://developer.android.google.cn/studio)，2020.3.1 (Arctic Fox) 或之後的版本

Alternatively, you can also use IntelliJ:

同時, 你也可以使用 IntelliJ：

* [IntelliJ IDEA Community][], version 2021.2 or later

  [IntelliJ IDEA Community][]，2021.2 或之後的版本

* [IntelliJ IDEA Ultimate][], version 2021.2 or later

  [IntelliJ IDEA Ultimate][]，2021.2 或之後的版本

## Install the Flutter and Dart plugins

## 安裝 Flutter 和 Dart 外掛

The installation instructions vary by platform.

請參考下面不同平台的安裝指南：

### Mac

Use the following instructions for macos:

安裝過程如下：

 1.  Start Android Studio.
    
     開啟 Android Studio。

 1. Open plugin preferences (**Preferences > Plugins** as of
     v3.6.3.0 or later).

    開啟外掛設定（在 v3.6.3.0 以上的系統開啟 **Preferences > Plugins**）。

 1. Select the Flutter plugin and
     click **Install**.

    然後選擇 Flutter 外掛並點選 **安裝**。

 1. Click **Yes** when prompted to install the Dart plugin.

    當彈出安裝 Dart 外掛提示時，點選 **Yes**。

 1. Click **Restart** when prompted.

    當彈出重新啟動提示時，點選 **Restart**。

### Linux or Windows

### Linux 或者 Windows 平台

Use the following instructions for Linux or Windows:

參考使用下面介紹的步驟：

   1. Open plugin preferences (**File > Settings > Plugins**).

      開啟外掛偏好設定 (位於 **File > Settings > Plugins**)

   1. Select **Marketplace**,  select the Flutter plugin and click
      **Install**.

      選擇 **Marketplace (擴充商店)**，選擇 Flutter plugin
      然後點選 **Install (安裝)**。

</div>
<div class="tab-pane" id="emacs" role="tabpanel" aria-labelledby="emacs-tab" markdown="1">

## Install Emacs

## 安裝 Emacs 編輯器

Emacs is a lightweight editor with support for Flutter and Dart.

Emacs 是一個輕量級的編輯器，支援 Flutter 和 Dart。

* [Emacs][], latest stable version

  最新版本的 [Emacs][] 編輯器。

## Install the lsp-dart package

## 安裝 lsp-dart 這個 package

For information on how to install and use the package,
see the [lsp-dart documentation][].

關於如何安裝和使用 lsp-dart 這個 package，可以檢視
[lsp-dart 文件][lsp-dart documentation]。

</div>
</div>{% comment %} End: Tab panes. {% endcomment -%}


[Android Studio]: {{site.android-dev}}/studio
[IntelliJ IDEA Community]: https://www.jetbrains.com/idea/download/
[IntelliJ IDEA Ultimate]: https://www.jetbrains.com/idea/download/
[next step: Test drive]: {{site.url}}/get-started/test-drive
[VS Code]: https://code.visualstudio.com/
[Emacs]: https://www.gnu.org/software/emacs/download.html
[lsp-dart documentation]: https://emacs-lsp.github.io/lsp-dart/
