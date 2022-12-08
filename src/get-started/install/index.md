---
title: Install
title: 安裝和環境配置
description: Install Flutter and get started. Downloads available for Windows, macOS, Linux, and Chrome OS operating systems.
description: Flutter安裝和上手起步課程, 下載 Windows、macOS、Linux 和 Chrome OS 系統的 Flutter SDK。
tags: Flutter安裝,Flutter環境搭建
keywords: Flutter Windows,Flutter Linux,Flutter macOS,Flutter映象,Flutter使用課程
next:
  title: Set up an editor
  title: 編輯工具設定
  path: /docs/get-started/editor
os-list: [Windows, macOS, Linux, "Chrome OS"]
---

Select the operating system on which you are installing Flutter:

你想把 Flutter 安裝在哪個作業系統呢？

<div class="card-deck mb-8">
{% for os in page.os-list %}
  <a class="card" id="install-{{os | remove: ' ' | downcase}}" href="/get-started/install/{{os | remove: ' ' | downcase}}">
    <div class="card-body">
      <header class="card-title text-center m-0">
        <span class="d-block h1">
          {% assign icon = os | downcase %}
          {% if icon == 'macos' %}
            <i class="fab fa-apple"></i>
          {% else %}
            <i class="fab fa-{{icon}}"></i>
          {% endif %}
        </span>
        <span class="text-muted text-nowrap">{{os}}</span>
      </header>
    </div>
  </a>
{% endfor %}
</div>

{{site.alert.important}}

  If you're in China, first read [Using Flutter in China][].
  
  如果你在中國的網路環境下使用 Flutter，請先看一下
  [這篇文章][Using Flutter in China]，檢視是否需要對網路環境進行特別設定。

{{site.alert.end}}

[Using Flutter in China]: {{site.main_url}}/community/china
