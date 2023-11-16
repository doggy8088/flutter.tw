---
title: Networking
title: 網路
description: Internet network calls in Flutter.
description: 在 Flutter 中存取網路
tags: 資料呼叫和後端
keywords: 網路請求
---

## Cross-platform http networking

## 跨平臺 http 網路

The [`http`][] package provides the simplest way to issue http requests. This
package is supported on Android, iOS, macOS, Windows, Linux and the web.

[`http`][] package 提供了 http 請求最簡單易用的方法。
該 package 支援 Android、iOS、macOS、Windows、Linux 以及 Web。

## Platform notes

## 平台說明

Some platforms require additional steps, as detailed below.

部分平台需要額外的步驟，詳見下文。

### Android

Android apps must [declare their use of the internet][declare] in the Android
manifest (`AndroidManifest.xml`):

Android 應用程式必須在 Android manifest (`AndroidManifest.xml`) 中
[宣告使用網路許可權][declare]

```
<manifest xmlns:android...>
 ...
 <uses-permission android:name="android.permission.INTERNET" />
 <application ...
</manifest>
```

### macOS

macOS apps must allow network access in the relevant `*.entitlements` files. 

macOS 應用程式必須在相關 `*.entitlements` 的檔案中允許網路存取。

```
<key>com.apple.security.network.client</key>
<true/>
```

Learn more about [setting up entitlements][].

瞭解更多資訊，請查閱 [設定 entitlements][setting up entitlements]。

[setting up entitlements]: {{site.url}}/platform-integration/macos/building#setting-up-entitlements

## Samples

## 範例

For a practical sample of various networking tasks (incl. fetching data,
WebSockets, and parsing data in the background) see the 
[networking cookbook]({{site.url}}/cookbook#networking).

有關各種網路任務（包括：獲取資料、WebSockets 和後臺解析資料）的
實用範例，請查閱 [網路 cookbook]({{site.url}}/cookbook#networking)。

[declare]: {{site.android-dev}}/training/basics/network-ops/connecting
[`http`]: {{site.pub-pkg}}/http
