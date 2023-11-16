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

You can build apps with Flutter using any text editor or
integrated development environment (IDE)
combined with Flutter's command-line tools.
The Flutter team recommends using an editor that supports
a Flutter extension or plugin, like VS Code and Android Studio.
These plugins provide code completion, syntax highlighting,
widget editing assists, run & debug support, and more.

你可以使用任意文字編輯器，結合我們的命令列工具來開發 Flutter 應用。
Flutter 團隊推薦使用支援 Flutter 外掛的編輯器，以獲取更好的開發體驗。
這些外掛提供了程式碼自動完成、程式碼高亮、widget 輔助編輯的功能，
以及為專案的執行和除錯提供支援等。

You can add a supported plugin for Visual Studio Code,
Android Studio, or IntelliJ IDEA Community, Educational,
and Ultimate editions.
The [Flutter plugin][] _only_ works with
Android Studio and the listed editions of IntelliJ IDEA.

(The [Dart plugin][] supports eight additional JetBrains IDEs.)

[Flutter plugin]: https://plugins.jetbrains.com/plugin/9212-flutter
[Dart plugin]: https://plugins.jetbrains.com/plugin/6351-dart

Follow these procedures to add the Flutter plugin to VS Code,
Android Studio, or IntelliJ.

參考以下步驟為 VS Code、Android Studio 或者 IntelliJ 新增編輯器外掛。

If you choose another IDE, skip ahead to the [next step: Test drive][].

如果你想使用其他的編輯器，請前往
[下一節: 開發體驗初探][next step: Test drive]。

{% comment %} Nav tabs {% endcomment -%}
<ul class="nav nav-tabs" id="editor-setup" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="vscode-tab" href="#vscode" role="tab" aria-controls="vscode" aria-selected="true">Visual Studio Code</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="androidstudio-tab" href="#androidstudio" role="tab" aria-controls="androidstudio" aria-selected="false">Android Studio and IntelliJ</a>
  </li>
</ul>

{% comment %} Tab panes {% endcomment -%}
<div class="tab-content">
<div class="tab-pane active" id="vscode" role="tabpanel" aria-labelledby="vscode-tab" markdown="1">

## Install VS Code

## 安裝 VS Code

[VS Code][] is a code editor to build and debug apps.
With the Flutter extension installed, you can compile, deploy, and debug
Flutter apps.

[VS Code][] 是一個可以執行和除錯 Flutter 的編輯器。
安裝 Flutter 外掛後，你可以編譯、部署及除錯 Flutter 應用。

To install the latest version of VS Code,
follow Microsoft's instructions for the relevant platform:

請參考 Microsoft 針對不同平台的安裝指引來安裝最新版本的 VS Code：

- [Install on macOS][]
- [Install on Windows][]
- [Install on Linux][]

[VS Code]: https://code.visualstudio.com/
[Install on macOS]: https://code.visualstudio.com/docs/setup/mac
[Install on Windows]: https://code.visualstudio.com/docs/setup/windows
[Install on Linux]: https://code.visualstudio.com/docs/setup/linux

## Install the VS Code Flutter extension

## 安裝 VS Code 的 Flutter 擴充

1. Start **VS Code**.

   開啟 **VS Code**。

1. Open a browser and go to the [Flutter extension][] page
   on the Visual Studio Marketplace.

   開啟瀏覽器，存取市場的 [Flutter 外掛][Flutter extension] 頁面。

1. Click **Install**.
   Installing the Flutter extension also installs the Dart extension.

   點選 **Install**，安裝 Flutter 與 Dart 依賴。

[Flutter extension]: https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter

## Validate your VS Code setup

1. Go to **View** <span aria-label="and then">></span> **Output**.

   開啟 **View** <span aria-label="and then">></span> **Output**。

   You can also press <kbd>Ctrl</kbd> / <kbd>Cmd</kbd> +
   <kbd>Shift</kbd> + <kbd>U</kbd>.

   你也可以按下 <kbd>Ctrl</kbd> / <kbd>Cmd</kbd> +
   <kbd>Shift</kbd> + <kbd>U</kbd>。

1. In the dropdown on the upper right of the **Output** panel,
   select **flutter (flutter)**.

   在 **Output** 面板右上角的下拉選單中選擇 **flutter (flutter)**。

1. Go to **View** <span aria-label="and then">></span>
   **Command Palette...**.

   開啟 **View** <span aria-label="and then">></span> **Command Palette...**。

   You can also press <kbd>Ctrl</kbd> / <kbd>Cmd</kbd> +
   <kbd>Shift</kbd> + <kbd>P</kbd>.

   你也可以按下 <kbd>Ctrl</kbd> / <kbd>Cmd</kbd> +
   <kbd>Shift</kbd> + <kbd>P</kbd>。

1. Type `doctor`.

   輸入 "doctor"。

1. Select the **Flutter: Run Flutter Doctor**.
   Flutter Doctor runs and its response displays in the **Output** panel.

   選擇 **Flutter: Run Flutter Doctor**。
   輸出結果會顯示在 **Output** 面板中。

</div>
<div class="tab-pane" id="androidstudio" role="tabpanel" aria-labelledby="androidstudio-tab" markdown="1">

## Install Android Studio or IntelliJ IDEA

## 安裝 Android Studio 或 IntelliJ IDEA

Android Studio and IntelliJ IDEA offer a complete,
IDE experience once you install the Flutter plugin.

Android Studio 和 IntelliJ IDEA
為 Flutter 提供了一個完整的整合開發環境。

To install the latest version of the following IDEs, follow their instructions:

你可以按照以下指引安裝對應 IDE 的最新版本：

- [Android Studio][]
- [IntelliJ IDEA Community][]
- [IntelliJ IDEA Ultimate][]

## Install the Flutter plugin

## 安裝 Flutter 和 Dart 外掛

The installation instructions vary by platform.

請參考下面不同平台的安裝指南：

### macOS

Use the following instructions for macOS:

安裝過程如下：

1. Start Android Studio or IntelliJ.

   開啟 Android Studio 或 IntelliJ。

1. From the macOS menu bar, go to **Android Studio** (or **IntelliJ**)
   <span aria-label="and then">></span> **Settings...**.

   從 macOS 的選單欄中開啟外掛設定。

   You can also press <kbd>Cmd</kbd> + <kbd>,</kbd>.

   你也可以按下 <kbd>Cmd</kbd> + <kbd>,</kbd>。

   The **Preferences** dialog opens.

   **Preferences** 彈窗會開啟。

1. From the list at the left, select **Plugins**.

   在左側列表中選擇 **Plugins**。

1. From the top of this panel, select **Marketplace**.

   在面板的上方選擇 **Marketplace**。

1. Type `flutter` in the plugins search field.

   在搜尋框中輸入 `flutter`。

1. Select the **Flutter** plugin.

   選擇 **Flutter** 外掛。

1. Click **Install**.

   點選 **Install** 安裝。

1. Click **Yes** when prompted to install the plugin.

   提示安裝時點選確認。如果提示同時安裝 Dart 外掛也點選確認。

1. Click **Restart** when prompted.

   當彈出重新啟動提示時，點選 **Restart**。

### Linux or Windows

### Linux 或者 Windows 平台

Use the following instructions for Linux or Windows:

參考使用下面介紹的步驟：

1. Go to **File** <span aria-label="and then">></span>
   **Settings**.

   開啟 **File** <span aria-label="and then">></span>
   **Settings**。

   You can also press <kbd>Ctrl</kbd> + <kbd>Alt</kbd> +
   <kbd>S</kbd>.

   你也可以按下 <kbd>Cmd</kbd> + <kbd>,</kbd>。

   The **Preferences** dialog opens.

   **Preferences** 彈窗會開啟。

1. From the list at the left, select **Plugins**.

   在左側列表中選擇 **Plugins**。

1. From the top of this panel, select **Marketplace**.

   在面板的上方選擇 **Marketplace**。

1. Type `flutter` in the plugins search field.

   在搜尋框中輸入 `flutter`。

1. Select the **Flutter** plugin.

   選擇 **Flutter** 外掛。

1. Click **Install**.

   點選 **Install** 安裝。

1. Click **Yes** when prompted to install the plugin.

   提示安裝時點選確認。如果提示同時安裝 Dart 外掛也點選確認。

1. Click **Restart** when prompted.

   當彈出重新啟動提示時，點選 **Restart**。

</div>
</div>{% comment %} End: Tab panes. {% endcomment -%}

[Android Studio]: {{site.android-dev}}/studio/install
[IntelliJ IDEA Community]: https://www.jetbrains.com/idea/download/
[IntelliJ IDEA Ultimate]: https://www.jetbrains.com/idea/download/
[next step: Test drive]: {{site.url}}/get-started/test-drive
