---
title: Building macOS apps with Flutter
title: 使用 Flutter 開發 macOS 應用
description: Platform-specific considerations for building for macOS with Flutter.
description: 使用 Flutter 建構 macOS 應用時，平台側的一些關注點
toc: true
short-title: macOS development
short-title: macOS 開發
---

This page discusses considerations unique to building
macOS apps with Flutter, including shell integration
and distribution of macOS apps through the Apple Store.

本文章討論了使用 Flutter 建構 macOS 應用的特有考慮因素，
包括 shell 整合和在 Apple Store 上分發應用。

## Integrating with macOS look and feel

## 適應 macOS 的外觀及介面風格

While you can use any visual style or theme you choose
to build a macOS app, you might want to adapt your app
to more fully align with the macOS look and feel.
Flutter includes the [Cupertino] widget set,
which provides a set of widgets for
the current iOS design language.
Many of these widgets, including sliders,
switches and segmented controls,
are also appropriate for use on macOS.

儘管你可以選用任何樣式或主題來建構 macOS 應用，
但是也許你會更希望應用的介面風格儘可能地對齊 macOS 的設計語言。
Flutter 提供了一套符合當前 iOS 設計風格語言的 [Cupertino][] 元件集。
其中許多的元件，例如滑塊 (Sliders)，開關 (Switches)，
分段控制器 (Segmented controls)，在 macOS 上依然適用。

Alternatively, you might find the [macos_ui][]
package a good fit for your needs.
This package provides widgets and themes that
implement the macOS design language,
including a `MacosWindow` frame and scaffold,
toolbars, pulldown and
pop-up buttons, and modal dialogs.

此外，[macos_ui][] package 同樣能滿足你的需求。
此 package 提供了採用 macOS 設計語言的元件和主題，
包括一個 `MacosWindow` 框架、腳手架 (Scaffold)、
工具欄 (Toolbar)、下拉和彈出按鈕以及模態 (modal) 對話方塊。

[Cupertino]: {{site.url}}/ui/widgets/cupertino
[macos_ui]: {{site.pub}}/packages/macos_ui

## Building macOS apps

## 建構 macOS 應用

To distribute your macOS application, you can either
[distribute it through the macOS App Store][],
or you can distribute the `.app` itself,
perhaps from your own website.
As of macOS 10.14.5, you need to notarize
your macOS application before distributing
it outside of the macOS App Store.

你既可透過 [macOS 的 App Store][distribute it through the macOS App Store]，
也可直接在你的網站提供 `.app` 程式檔案下載，以分發你的應用。
對於 macOS 10.14.5 及之後的版本，
在外部分發 macOS 應用之前，你需要對其進行公證。

The first step in both of the above processes
involves working with your application inside of Xcode.
To be able to compile your application from inside of
Xcode you first need to build the application for release
using the `flutter build` command, then open the
Flutter macOS Runner application.

無論採用上方何種方案，你都需要在 Xcode 中對應用進行處理。
首先你需要使用 `flutter build` 命令建構釋出版本的應用，
然後在 Xcode 中開啟 Flutter macOS 目錄下 Runner (Runner.xcworkspace)，
才能在 Xcode 中編譯你的應用。

```bash
flutter build macos
open macos/Runner.xcworkspace
```

Once inside of Xcode, follow either Apple's
[documentation on notarizing macOS Applications][], or
[on distributing an application through the App Store][].
You should also read through the
[macOS-specific support](#entitlements-and-the-app-sandbox)
section below to understand how entitlements,
the App Sandbox, and the Hardened Runtime
impact your distributable application.

在 Xcode 中開啟應用後，請參考 Apple 官網指南
[在分發前對 macOS 軟體進行公證][documentation on notarizing macOS Applications]
或 [使用 App Store 分發應用程式][on distributing an application through the App Store]。
除此之外，你還需要閱讀 [授權和應用沙盒](#entitlements-and-the-app-sandbox) 部分，
瞭解授權機制、沙盒和強化版執行時會如何影響分發的應用程式。

[Build and release a macOS app][] provides a more detailed
step-by-step walkthrough of releasing a Flutter app to the
App Store.

[建構和釋出為 macOS 應用][Build and release a macOS app] 文件提供了將 Flutter 應用釋出至 App Store 的詳細步驟。

[distribute it through the macOS App Store]: {{site.apple-dev}}/macos/submit/
[documentation on notarizing macOS Applications]:{{site.apple-dev}}/documentation/xcode/notarizing_macos_software_before_distribution
[on distributing an application through the App Store]: https://help.apple.com/xcode/mac/current/#/dev067853c94
[Build and release a macOS app]: {{site.url}}/deployment/macos

## Entitlements and the App Sandbox

## 授權和應用沙盒

macOS builds are configured by default to be signed,
and sandboxed with App Sandbox.
This means that if you want to confer specific
capabilities or services on your macOS app,
such as the following:

預設情況下，macOS 建構已簽名並使用 App Sandbox 進行沙盒化。
這意味著如果你想要在 macOS 應用中配置特定的功能或服務，例如：

* Accessing the internet

  存取網路

* Capturing movies and images from the built-in camera

  使用內建相機拍攝圖片或影片

* Accessing files

  存取檔案

Then you must set up specific _entitlements_ in Xcode.
The following section tells you how to do this.

你必須要在 Xcode 中指定 **授權檔案 (entitlements)**。
接下來的章節，會告訴你如何實現。

### Setting up entitlements

### 配置授權

Managing sandbox settings is done in the
`macos/Runner/*.entitlements` files. When editing
these files, you shouldn't remove the original
`Runner-DebugProfile.entitlements` exceptions
(that support incoming network connections and JIT),
as they're necessary for the `debug` and `profile`
modes to function correctly.

`macos/Runner/*.entitlements` 檔案管理了沙盒的相關配置。
當你編輯這些檔案時，請不要刪除原始的 `Runner-DebugProfile.entitlements`
（支援傳入網路連線和 JIT），因為除錯和效能模式的正常執行需要它們。

If you're used to managing entitlement files through
the **Xcode capabilities UI**, be aware that the capabilities
editor updates only one of the two files or,
in some cases, it creates a whole new entitlements
file and switches the project to use it for all configurations.
Either scenario causes issues. We recommend that you
edit the files directly. Unless you have a very specific
reason, you should always make identical changes to both files.

如果你習慣於透過 **Xcode capabilities（功能）介面** 管理授權檔案，
請注意它僅會更新兩個檔案中的一個。
而在某些情況下，它會建立一個新的授權檔案，並使用其切換專案的所有配置。
這兩種情況都會導致一些問題，因此我們建議你直接編輯這些檔案。
除非有明確的需求，否則你應該同步修改這兩個檔案。

If you keep the App Sandbox enabled (which is required if you
plan to distribute your application in the [App Store][]),
you need to manage entitlements for your application
when you add certain plugins or other native functionality.
For instance, using the [`file_chooser`][] plugin
requires adding either the
`com.apple.security.files.user-selected.read-only` or
`com.apple.security.files.user-selected.read-write` entitlement.
Another common entitlement is
`com.apple.security.network.client`,
which you must add if you make any network requests.

如果你打算在 [App Store][] 分發你的應用，你需要啟用應用沙盒功能，
此時如果你需要新增某些外掛或其他原生功能，則需編輯你的應用授權。
例如，使用 [`file_chooser`][] 外掛需要新增 `com.apple.security.files.user-selected.read-only`
或 `com.apple.security.files.user-selected.read-write` 授權。
另一個常見的授權為 `com.apple.security.network.client`，是你的應用存取網路所必需的。

Without the `com.apple.security.network.client` entitlement,
for example, network requests fail with a message such as:

如果沒有 `com.apple.security.network.client` 授權，
則網路請求會失敗並返回如下的資訊：

```terminal
flutter: SocketException: Connection failed
(OS Error: Operation not permitted, errno = 1),
address = example.com, port = 443
```

{{site.alert.secondary}}

  **Important:** The `com.apple.security.network.server`
  entitlement, which allows incoming network connections,
  is enabled by default only for `debug` and `profile`
  builds to enable communications between Flutter tools
  and a running app. If you need to allow incoming
  network requests in your application,
  you must add the `com.apple.security.network.server`
  entitlement to `Runner-Release.entitlements` as well,
  otherwise your application will work correctly for debug or
  profile testing, but will fail with release builds.

  **重點：** 預設情況下，允許傳入網路請求的 `com.apple.security.network.server` 授權，
  只在除錯和效能模式下啟用，
  這是為了讓 Flutter tools 能和執行中的應用進行通訊。
  如果你需要允許應用的傳入網路請求，則必須在 `Runner-Release.entitlements` 中新增
  `com.apple.security.network.server` 授權，
  否則你的應用僅可在除錯和效能模式下正常工作，在釋出版本中則無法正常使用。

{{site.alert.end}}

For more information on these topics,
see [App Sandbox][] and [Entitlements][]
on the Apple Developer site.

你可以閱讀 Apple 開發者官網的 [應用沙盒][App Sandbox] 和
[授權][Entitlements] 文件，以獲取關於此章節的更多資訊。

[App Sandbox]: {{site.apple-dev}}/documentation/security/app_sandbox
[App Store]: {{site.apple-dev}}/app-store/submissions/
[Entitlements]: {{site.apple-dev}}/documentation/bundleresources/entitlements
[`file_chooser`]: {{site.github}}/google/flutter-desktop-embedding/tree/master/plugins/file_chooser

## Hardened Runtime

## 強化版執行時

If you choose to distribute your application outside
of the App Store, you need to notarize your application
for compatibility with macOS 10.15+.
This requires enabling the Hardened Runtime option.
Once you have enabled it, you need a valid signing
certificate in order to build.

如果你不打算使用 App Store 分發應用，
你則需要對你的應用進行公證，以與 macOS 10.15+ 相容。
這便要求啟用強化版執行時。
一旦啟用，你就需要一個有效的簽名證書來編譯建構應用。

By default, the entitlements file allows JIT for
debug builds but, as with App Sandbox, you might
need to manage other entitlements.
If you have both App Sandbox and Hardened
Runtime enabled, you might need to add multiple
entitlements for the same resource.
For instance, microphone access would require both
`com.apple.security.device.audio-input` (for Hardened Runtime)
and `com.apple.security.device.microphone` (for App Sandbox).

預設情況下，授權檔案支援 JIT 除錯建構，
但是在使用應用沙盒時，你可能需要管理其他授權檔案。
如果你同時啟用應用沙盒和強化版執行時，你可能需要為同一資源新增多個授權。
例如，麥克風存取許可權同時要求 `com.apple.security.device.audio-input` (用於強化版執行時)
和 `com.apple.security.device.microphone` (用於應用沙盒)。

For more information on this topic,
see [Hardened Runtime][] on the Apple Developer site.

你可以閱讀 Apple 開發者官網的 [強化版執行時][Hardened Runtime] 文件，
以獲取關於此章節的更多資訊。

[Hardened Runtime]: {{site.apple-dev}}/documentation/security/hardened_runtime
