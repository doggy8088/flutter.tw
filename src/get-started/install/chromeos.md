---
title: Chrome OS install
title: 在 Chrome OS 上安裝和配置 Flutter 開發環境
description: How to install on Chrome OS.
description: 如何在 Chrome OS 上安裝 Flutter。
short-title: Chrome OS
tags: Flutter安裝,Flutter環境搭建
keywords: Flutter ChromsOS,ChromeOS上安裝Flutter,ChromeOS開發
next:
  title: Set up an editor
  titie: 編輯工具設定
  path: /docs/get-started/editor
---

{% assign os = 'linux' -%}

## System requirements

## 系統要求

To install and run Flutter, your development environment
must meet these minimum requirements:

要安裝並執行 Flutter，你的開發環境必須滿足如下最低配置要求：

* **Operating Systems**: Chrome OS (64-bit) with [Linux (Beta)][] turned on

  **作業系統**：64 位的 Chrome OS 系統（需開啟 [Linux (Beta)][] 功能）

* **Disk Space**: 600 MB (does not include disk space for IDE/tools).

  **磁碟空間**：600 MB （不包括 IDE/tools 所佔的空間）

* **Tools**: Flutter depends on these command-line
  tools being available in your environment.

  **工具**：Flutter 需要環境中安裝如下命令列工具

  * `bash`
  * `curl`
  * `git` 2.x
  * `mkdir`
  * `rm`
  * `unzip`
  * `which`
  * `xz-utils`

* **Shared libraries**: Flutter `test` command depends on
  this library being available in your environment.

  **共享庫**：Flutter 的 `test` 命令依賴下面的函式庫

  * `libGLU.so.1` - provided by mesa packages such as `libglu1-mesa` on
     Ubuntu/Debian

    `libGLU.so.1` - 由 mesa 套件提供，比如在 Ubuntu/Debian
    系統上對應的包是 `libglu1-mesa`

{% include_relative _get-sdk-chromeos.md %}

{% include_relative _path-linux-chromeos.md %}

{% include_relative _android-setup-chromeos.md %}

## Next step

## 下一步

Set up your preferred editor.

編輯器設定。

## Flutter & Chrome OS tips & tricks

## Flutter 和 Chrome OS 的小技巧

For the current versions of Chrome OS, only certain ports from
Crostini are exposed to the rest of the environments.
Here’s an example of how to launch
Flutter DevTools for an Android app with ports
that will work:

對於當前版本的 Chrome OS，只有 Crostini 的幾個埠對環境開放。
下面這個範例講解了如何在可用埠上啟動 Flutter DevTools:

```terminal
$ flutter pub global run devtools --port 8000
$ cd path/to/your/app
$ flutter run --observatory-port=8080
```

Then, navigate to http://127.0.0.1:8000/#
in your Chrome browser and enter the URL to your
application. The last `flutter run` command you
just ran should output a URL similar to the format
of `http://127.0.0.1:8080/auth_code=/`. Use this URL
and select "Connect" to start the Flutter DevTools
for your Android app.

然後在你的 Chrome 瀏覽器裡開啟 URL: http://127.0.0.1:8000/#，
上面最後一個 `flutter run` 命令會輸出一個類別似
`http://127.0.0.1:8080/auth_code=/` 的 URL，
使用這個 URL 並選擇「Connect」來啟動
適用於 Android 應用的 Flutter DevTools。

#### Flutter Chrome OS lint analysis

#### Chrome OS 上的 Flutter lint 分析

The Flutter team is adding Chrome OS specific
Lint Analysis checks that are available to make
sure that the app that you're building is going
to work well on Chrome OS. It looks for things
like required hardware in your Android Manifest
that aren’t available on Chrome OS devices,
permissions that imply requests for unsupported
hardware, as well as other properties or code
that would bring a lesser experience on these devices.

Flutter 團隊在 Chrome OS 中添加了 Lint Analysis 檢查，
用於保證所建構的應用程式在 Chrome OS 上執行正常。
它會檢查在 AndroidManifest 裡是否存在所需的硬體是 Chrome OS 裝置上所不支援的，
是否向不支援的硬體請求了許可權，以及是否存在會降低體驗效果的程式碼。

To activate these,
you need to create a new analysis_options.yaml file to include these options.

要啟用上述的功能特性，你需要建立一個新的或者更新你現有的
analysis_options.yaml 檔案，使其包含如下選項：

(If you have an existing analysis_options.yaml file, you can update it)

（如果你已經有一個 analysis_options.yaml，直接更新它的內容即可）

```yaml
include: package:flutter/analysis_options_user.yaml
analyzer:
 optional-checks:
   chrome-os-manifest-checks
```

To run these from the command line, use the following command:

從命令列執行下面的內容：

```terminal
$ flutter analyze
```

Sample output for this command might look like:

執行後的輸出如下：

```terminal
Analyzing ...
warning • This hardware feature is not supported on Chrome OS •
android/app/src/main/AndroidManifest.xml:4:33 • unsupported_chrome_os_hardware
```

This functionality is still under development,
but check back for instructions on how you can make
this functionality work with your Chrome OS
targeted Flutter app.

目前該功能仍然處於開發階段，不過你可以在未來根據本文件的內容
在 Chrome OS 上開發 Flutter 應用程式。

[Linux (Beta)]: https://support.google.com/chromebook/answer/9145439
