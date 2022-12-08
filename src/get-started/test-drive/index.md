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

This page describes how to create a new Flutter app from templates, run it,
and experience "hot reload" after you make changes to the app.

本頁面講解如何透過範本實現一個 Flutter 應用，
執行並且在修改程式之後觸發“熱重載 (hot reload)”功能。

Select your development tool of choice for writing, building, and running Flutter apps.

選擇你用於編寫、編譯、執行 Flutter 應用的開發環境吧。

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


[Install]: {{site.url}}/get-started/install
[Main IntelliJ toolbar]: {{site.url}}/assets/images/docs/tools/android-studio/main-toolbar.png
[Managing AVDs]: {{site.android-dev}}/studio/run/managing-avds
[Material Components]: {{site.material}}/guidelines
