---
title: iOS debugging
title: iOS 平台除錯
description: iOS-specific debugging techniques for Flutter apps
description: 用於 Flutter 應用程式的 iOS 專用除錯技巧
---

Due to security around
[local network permissions in iOS 14 or later][],
you must accept a permission dialog box to enable
Flutter debugging functionalities such as hot-reload
and DevTools.

由於 [iOS 14 或更高版本中本地網路許可權][local network permissions in iOS 14 or later] 
的安全限制，你必須在以下許可權的對話方塊中允許許可權來啟用 Flutter 除錯功能，
如熱重載和 DevTools。

![Screenshot of "allow network connections" dialog]({{site.url}}/assets/images/docs/development/device-connect.png)

This affects debug and profile builds only and won't
appear in release builds. You can also allow this
permission by enabling
**Settings > Privacy > Local Network > Your App**.

這隻會出現在 debug 和 profile 建構中，
不會出現在 release 建構中。
你也可以透過開啟 **設定 > 隱私與安全性 > 本地網路 > 你的應用程式** 來允許該許可權。

[local network permissions in iOS 14 or later]: {{site.apple-dev}}/news/?id=0oi77447

