---
title: "Adding an Apple Watch extension"
title: 作為 Apple Watch 外掛
description: "Learn how to add an Apple Watch target to a Flutter app."
description: "學習將 Apple Watch target 新增到 Flutter app"
tags: 平台整合
keywords: Apple Watch,手錶,Flutter嵌入式
---

While you cannot build an Apple Watch app with Flutter,
it is possible to add a native Apple Watch extension to a Flutter app.

雖然您不能使用 Flutter 建構 Apple Watch 應用程式，
但可以向 Flutter 應用程式新增 Apple Watch 的本地擴充。

## Step 1: Enable bitcode in Xcode

## 步驟 1: 在 Xcode 中開啟 bitcode

Apple Watch targets require bitcode to be enabled,
so follow the steps in
[Creating an iOS Bitcode enabled app]({{site.repo.flutter}}/wiki/Creating-an-iOS-Bitcode-enabled-app)
to use bitcode in your app.

Apple Watch target 要求啟用 bitcode，
因此請按照 
[建立支援 bitcode 的 iOS 應用程式]({{site.repo.flutter}}/wiki/Creating-an-iOS-Bitcode-enabled-app) 
中的步驟在您的應用程式中使用 bitcode。

## Step 2: Add an Apple Watch target

## 步驟 2: 新增一個 Apple Watch target

In the menu, select **File > New > Target**. Once the dialog opens, select
**watchOS** at the top and click **Watch App for iOS App**. Click **Next**, 
enter a product name, and select **Enter**.

在選單中，選擇 **File > New > Target**。
當對話方塊開啟後，在頂部選擇 **watchOS** 並且點選 **Watch App for iOS App**。
然後，點選 **Next**，輸入產品名，最後選擇 **Enter**。

![Adding an Apple Watch target]({{site.url}}/assets/images/docs/AppleWatchTarget.png){:width="70%"}

[Creating an iOS Bitcode enabled app]: {{site.repo.flutter}}/wiki/Creating-an-iOS-Bitcode-enabled-app-(experimental)
