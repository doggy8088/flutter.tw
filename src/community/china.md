---
title: Using Flutter in China
title: 在中國網路環境下使用 Flutter
description: Where to find a version of the Flutter site that is localized to Simplified Chinese.
description: 如果你需要在中國網路環境下使用 Flutter，請查閱此頁面。
toc: true
---

{% assign path = 'flutter_infra_release/releases/stable/windows/flutter_windows_v1.0.0-stable.zip' -%}
{% comment %}
The Flutter community has made a Simplified Chinese version of the
Flutter website available at
[https://flutter.cn](https://flutter.cn).
{% endcomment %}

歡迎你來到由中國 Flutter 社群維護的中文 Flutter 資源網站：[https://flutter.cn](https://flutter.cn)

{% comment %}
If you’d like to install Flutter using an [installation
bundle]({{site.url}}/development/tools/sdk/releases),
you can replace the domain of the original URL with a trusted mirror
to speed it up. For example:
{% endcomment %}

如果你需要下載 [Flutter SDK 的獨立打包檔案]({{site.url}}/development/tools/sdk/releases)，
你可以將下載連結字首替換為你信任的映象連結。

* 原始連結:<br>
  [`https://storage.googleapis.com/{{path}}`](https://storage.googleapis.com/{{path}})
* 映象之後的連結:<br>
  [`https://storage.flutter-io.cn/{{path}}`](https://storage.flutter-io.cn/{{path}})

{% comment %}
You must also set two environment variables to upgrade Flutter and use the pub
package repository in China. Instructions are below.
{% endcomment %}

同時為了正常升級 Flutter 和透過 pub package 命令獲取 packages，你需要設定如下兩個環境變數，
設定方式如下：

{{site.alert.important}}

{% comment %}
  Use mirror sites only if you _trust_ the provider.
  The Flutter team cannot verify their reliability or security.
{% endcomment %}  
  使用任意映象網站的時候，你必須確保你 **信任** 你的映象提供者。
  Flutter 團隊無法確保他們的安全性。
  
{{site.alert.end}}
{% comment %}
## Configuring Flutter to use a mirror site
{% endcomment %}
## 為 Flutter 設定映象配置

{% comment %}
If you’re installing or using Flutter in China, it may be helpful to use
a trustworthy local mirror site that hosts Flutter’s dependencies.
To instruct the Flutter tool to use an alternate storage location,
you need to set two environment variables, `PUB_HOSTED_URL` and
`FLUTTER_STORAGE_BASE_URL`, before running the `flutter` command.
{% endcomment %}

如果你在國內使用 Flutter，那麼你可能需要找一個與官方同步的可信的映象站點，
幫助你的 Flutter 命令列工具到該映象站點下載其所需的資源。
你需要為此設定兩個環境變數：`PUB_HOSTED_URL` 和 `FLUTTER_STORAGE_BASE_URL`，
然後再執行 Flutter 命令列工具。

{% comment %}
Taking MacOS or Linux as an example, here are the first few steps in
the setup process for using a mirror site. Run the following in a Bash
shell from the directory where you wish to store your local Flutter clone:
{% endcomment %}

以 macOS 或者與 Linux 相近的系統為例，這裡有以下步驟幫助你設定映象。
在系統終端裡執行如下命令設定環境變數，並透過 GitHub 檢出 Flutter SDK：

```terminal
$ export PUB_HOSTED_URL=https://pub.flutter-io.cn
$ export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
$ git clone -b dev {{site.repo.flutter}}.git
$ export PATH="$PWD/flutter/bin:$PATH"
$ cd ./flutter
$ flutter doctor
```

{% comment %}
After these steps, you should be able to continue
[setting up Flutter]({{site.url}}/get-started/editor) normally.
From here on, packages fetched by `flutter pub get` are
downloaded from `flutter-io.cn` in any shell where `PUB_HOSTED_URL`
and `FLUTTER_STORAGE_BASE_URL` are set.
{% endcomment %}

如上步驟設定之後，你可以繼續進行 Flutter 安裝的下一步：[編輯工具設定]({{site.url}}/get-started/editor)，
在這兩個環境變數（`PUB_HOSTED_URL` 和 `FLUTTER_STORAGE_BASE_URL`）設定過後，
未來透過命令 `flutter pub get` 獲取 packages 的時候，網路請求將會透過
`flutter-io.cn` 提供的映象進行。

{% comment %}
The `flutter-io.cn` server is a provisional mirror for Flutter
dependencies and packages maintained by [GDG China]().
The Flutter team cannot guarantee long-term availability of this service.
You’re free to use other mirrors if they become available. If you’re
interested in setting up your own mirror in China, contact
[flutter-dev@googlegroups.com](mailto:flutter-dev@googlegroups.com)
for assistance.
{% endcomment %}

`flutter-io.cn` 所提供的映象由中國的 Flutter 開發者社群提供和維護，
Flutter 團隊無法保證其的長期穩定運作，你也可以自由使用其他可信的機構提供的映象服務。

{% comment %}
## Community-run mirror sites
{% endcomment %}

## 社群執行的映象站點

如下列表為目前在國內提供映象的社群以及其映象配置，
由於映象的實現方式有所不同，可能會導致資料的滯後等問題。
我們製作了一個 [映象可用性監控頁面](https://stats.uptimerobot.com/JZK3ZTql79) 供參考。

### Flutter 社群

社群主映象，採用多種方式同步 Flutter 開發者資源（推薦）。
有任何映象相關的問題，請與我們
[反饋映象問題](https://github.com/cfug/flutter.cn/issues)，
中國映象儲存由 [七牛雲](https://sensors.qiniu.com/t/n9Q) 提供服務。

```terminal
$ export PUB_HOSTED_URL=https://pub.flutter-io.cn
$ export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

### 上海交大 Linux 使用者組

使用全量同步方式建立 Flutter 映象，配置了完善的回源
(flutter-io.cn 主映象和 GCS 站源) 策略（推薦），
有任何映象相關的問題，請向上海交大 Linux 使用者組
[反饋映象問題](https://github.com/sjtug/mirror-requests/issues/new?labels=bug&template=2-bug-report.md)。

檢視幫助文件：
[Flutter 映象安裝幫助](https://mirrors.sjtug.sjtu.edu.cn/docs/flutter_infra)，
[Pub 映象安裝幫助](https://mirrors.sjtug.sjtu.edu.cn/docs/dart-pub)。

```terminal
$ export PUB_HOSTED_URL=https://mirrors.sjtug.sjtu.edu.cn/dart-pub
$ export FLUTTER_STORAGE_BASE_URL=https://mirrors.sjtug.sjtu.edu.cn
```

### 清華大學 TUNA 協會

採取自訂指令碼定時主動抓取策略，並配置了完善的回源
(flutter-io.cn 主映象和 GCS 站源) 策略（推薦），
有任何映象相關的問題（包含 TUNA、OpenTUNA），請向清華大學 TUNA 協會
[反饋映象問題](https://github.com/tuna/issues/issues)。

檢視幫助文件：
[Flutter 映象安裝幫助](https://mirrors.tuna.tsinghua.edu.cn/help/flutter/)，
[Pub 映象安裝幫助](https://mirrors.tuna.tsinghua.edu.cn/help/dart-pub/)。

```terminal
$ export PUB_HOSTED_URL=https://mirrors.tuna.tsinghua.edu.cn/dart-pub
$ export FLUTTER_STORAGE_BASE_URL=https://mirrors.tuna.tsinghua.edu.cn/flutter
```

#### OpenTUNA

資料策略與 TUNA 映象一致、由清華 TUNA 協會執行維護，
[OpenTUNA](https://mirrors.tuna.tsinghua.edu.cn/news/opentuna-mirror/) 
映象透過 CloudFront CDN 進行分發。

**已知問題**：Pub API 與預期返回值不一致，可能造成請求無效 (2021/6/8)。

```terminal
$ export PUB_HOSTED_URL=https://opentuna.cn/dart-pub
$ export FLUTTER_STORAGE_BASE_URL=https://opentuna.cn/flutter
```

#### CNNIC

基於 TUNA 協會的映象服務，資料策略和內容與 TUNA 一致，
透過非教育網的域名存取（建議選擇 TUNA）。
暫無反饋渠道，可嘗試 TUNA 反饋渠道。

```terminal
$ export PUB_HOSTED_URL=http://mirrors.cnnic.cn/dart-pub
$ export FLUTTER_STORAGE_BASE_URL=http://mirrors.cnnic.cn/flutter
```

### 騰訊雲開源映象站

使用 TUNA 開源的指令碼每天凌晨 0 - 2 點執行同步，未配置回源策略。
使用騰訊雲伺服器的使用者，可將源域名從 mirrors.cloud.tencent.com 修改為
mirrors.tencentyun.com，使用內網流量不佔用公網流量。
有任何映象問題，請透過郵件 (mirrors@tencent.com) 向騰訊雲開源映象站反饋。

**已知問題**：
- Flutter Storage 已經從 `flutter_infra` 調整為 `flutter_infra_release`，
  騰訊雲開源映象並未對此做調整，可能會在更新的 Flutter 版本里無法請求到完整資料 (2021/6/8)。 
- 根據反饋 ([#1135](https://github.com/cfug/flutter.cn/issues/1135))，
  騰訊雲對 CIPD 的支援尚不確定是否完整。

```terminal
$ export PUB_HOSTED_URL=https://mirrors.cloud.tencent.com/dart-pub
$ export FLUTTER_STORAGE_BASE_URL=https://mirrors.cloud.tencent.com/flutter
```

### 其他已知問題

- 所有 Flutter 映象目前均不支援/也不應支援上傳 packages 到 pub.dev 網站。
  這個過程通常需要登陸谷歌帳號，而這將是一個無法繞開且複雜的挑戰。
- [上海大學的映象](https://mirrornews.shuosc.org/p/6d7146f9.html) 
  暫時只允許校記憶體取，故暫未展示，感謝上海大學 Linux 使用者組的同學。
- 騰訊雲開源映象站使用 TUNA 開源指令碼製作，每天同步一次，
  經測試，其資料延遲較大並尚未配置有效的回源策略，有待於社群成員進一步驗證。
- 任何其他與映象相關的問題，請透過
  [Issue 向我們反饋](https://github.com/cfug/flutter.cn/issues/new?template=mirrors_issue.md&title=%5Bmirrors%5D)。
- 部分映象的問題已經特別標識，待映象修復之後移除。

## 致謝

本頁面列出的映象由提供者分別維護，我們確保上述列出的映象提供方不會對資料進行惡意修改，
因為國內網路情況的複雜性和特殊性，我們無法保證映象長期穩定性和存取速度，請諒解。

如果在映象使用中有任何問題，歡迎透過郵件與我們聯絡：cfug-dev@googlegroups.com。
非常感謝所有幫助 Flutter 在國內維護社群基礎設施建設資源的社群和公司，檢視
[詳細緻謝名單]({{site.url}}/about)。
