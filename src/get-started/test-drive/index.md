---
title: Test drive
title: 開發體驗初探
description: How to create a templated Flutter app and use hot reload.
description: 如何使用 hot reload 建立一個 Flutter 應用模版。
tags: Flutter安裝,Flutter環境搭建
keywords: Flutter IDE,Flutter編輯器,Android Studio,VS Code,Flutter外掛
prev:
  title: Set up an editor
  title: 編輯工具設定
  path: /docs/get-started/editor
next:
  title: Write your first Flutter app
  title: 編寫第一個 Flutter 應用
  path: /docs/get-started/codelab
toc: false
---

This page describes the following tasks:

本篇文章講解以下內容：

1. How to create a new Flutter app from templates.

   如何基於範本建立新的 Flutter 應用。

1. How to run the created Flutter app.

   如何執行建立好的 Flutter 應用。

1. How to use "hot reload" after you make changes to the app.

   如何在應用中使用「熱重載」應用你的更改。

Details for these tasks depend on the integrated development environment
(IDE) you use.

這些任務的細節會根據你使用的 IDE 的不同有所變化。

The first two options listed rely on the Flutter plugin for
the respective IDE.
Visual Studio Code, Android Studio, and IntelliJ IDEA Community,
Educational, and Ultimate editions support Flutter development
through plugins.

前兩個任務依賴於 IDE 上的 Flutter 外掛。
Visual Studio Code、Android Studio、以及 IntelliJ IDEA 的
Community、Educational 和 Ultimate 版本都透過外掛支援 Flutter 開發。

The third option explains how to use an editor of your choice and
the terminal to run the commands.

第三個任務向你解釋瞭如何用你使用的編輯器或者終端來執行命令。

Select your preferred IDE for Flutter apps.

{% comment %} Nav tabs {% endcomment -%}
<ul class="nav nav-tabs" id="editor-setup" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="vscode-tab" href="#vscode" role="tab" aria-controls="vscode" aria-selected="true">Visual Studio Code</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="androidstudio-tab" href="#androidstudio" role="tab" aria-controls="androidstudio" aria-selected="false">Android Studio and IntelliJ</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="terminal-tab" href="#terminal" role="tab" aria-controls="terminal" aria-selected="false">終端 & 文字編輯器</a>
  </li>
</ul>

{% comment %} Tab panes {% endcomment -%}
<div class="tab-content">
  {% include_relative _vscode.md %}
  {% include_relative _androidstudio.md %}
  {% include_relative _terminal.md %}
</div>
