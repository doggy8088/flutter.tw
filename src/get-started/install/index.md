---
title: Install
title: 安裝和環境配置
description: Install Flutter and get started. Downloads available for Windows, macOS, Linux, and ChromeOS operating systems.
description: Flutter安裝和上手起步課程, 下載 Windows、macOS、Linux 和 ChromeOS 系統的 Flutter SDK。
tags: Flutter安裝,Flutter環境搭建
keywords: Flutter Windows,Flutter Linux,Flutter macOS,Flutter鏡像,Flutter使用課程
next:
  title: Set up an editor
  title: 編輯工具設定
  path: /docs/get-started/editor
os-list: [Windows, macOS, Linux, ChromeOS]
---

Select the operating system on which you are installing Flutter:

你想把 Flutter 安裝在哪個作業系統呢？

<div class="card-deck mb-8">
{% for os in page.os-list %}
  <a class="card" id="install-{{os | remove: ' ' | downcase}}" href="{{site.url}}/get-started/install/{{os | remove: ' ' | downcase}}">
    <div class="card-body">
      <header class="card-title text-center m-0">
        <span class="d-block h1">
          {% assign icon = os | downcase -%}
            <img src="/assets/images/docs/brand-svg/{{icon}}.svg" width="72" height="72" aria-hidden="true" alt="{{os}} logo"> 
        </span>
        <span class="text-muted text-nowrap">{{os}}</span>
      </header>
    </div>
  </a>
{% endfor %}
</div>

{% include docs/china-notice.md %}
