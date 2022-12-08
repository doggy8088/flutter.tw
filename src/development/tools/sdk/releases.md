---
title: Flutter SDK releases
title: Flutter SDK 版本列表
short-title: Releases
short-title: 版本列表
description: All current Flutter SDK releases, both stable, and master.
description: 所有 Flutter SDK 的版本列表，包括穩定版和主分支。
tags: 下載,SDK下載,Flutter版本
keywords: 建構渠道,Flutter SDK,SDK,中國映象
toc: false
---

<style>
.scrollable-table {
  overflow-y: scroll;
  max-height: 20rem;
}
</style>

The {{site.sdk.channel | capitalize }} channel contains the
most stable Flutter builds. See [Flutter’s channels][] for details.

Flutter 的 {{site.sdk.channel | capitalize }} channel 是相對穩定的釋出版本，
查閱這個文件瞭解更多：[Flutter 的建構（釋出）渠道 channels][Flutter’s channels]。

{% comment %} Nav tabs {% endcomment -%}
<ul class="nav nav-tabs" id="editor-setup" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="windows-tab" href="#windows" role="tab" aria-controls="windows" aria-selected="true">Windows</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="macos-tab" href="#macos" role="tab" aria-controls="macos" aria-selected="false">macOS</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="linux-tab" href="#linux" role="tab" aria-controls="linux" aria-selected="false">Linux</a>
  </li>
</ul>

{% comment %} Tab panes {% endcomment -%}
<div id="sdk-archives" class="tab-content">
{% include_relative _os.md os="Windows" %}
{% include_relative _os.md os="macOS" %}
{% include_relative _os.md os="Linux" %}
</div>

## Master channel

Installation bundles are not available for master.
However, you can get the SDK directly from
[GitHub repo][] by cloning the master channel,
and then triggering a download of the SDK dependencies:

我們並沒有對 master channel 的提供打包下載，
不過，你可以透過 `git clone` 我們在 
[Github 上 repo]({{site.repo.flutter}}) 的 master 分支來使用。

```terminal
$ git clone -b master https://github.com/flutter/flutter.git
$ ./flutter/bin/flutter --version
```

For additional details on how our installation bundles are structured,
see [Installation bundles][].

關於安裝包結構的更多資訊，請檢視這個頁面：
[Flutter 安裝包結構][Installation bundles]。

We will post a Weibo message with each Flutter releases and the merged PR,
please follow us on Weibo: [Flutter Community](https://weibo.com/u/6723427904)!

每次新版本釋出以及 Flutter 主 repo 有新 PR 合併的時候，我們會在社群微博上釋出一條資訊，歡迎關注
[Flutter社群](https://weibo.com/u/6723427904) 微博帳號！

[Flutter Spring 2020 Update]: {{site.flutter-medium}}/flutter-spring-2020-update-f723d898d7af
[Flutter’s channels]: {{site.repo.flutter}}/wiki/Flutter-build-release-channels
[Installation bundles]: {{site.repo.flutter}}/wiki/Flutter-Installation-Bundles
[GitHub repo]: {{site.repo.flutter}}
[Installation bundles]: {{site.repo.flutter}}/wiki/Flutter-Installation-Bundles
