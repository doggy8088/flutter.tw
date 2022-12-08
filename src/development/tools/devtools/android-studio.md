---
title: Install and run DevTools from Android Studio
title: 在 Android Studio 上安裝和執行開發者工具
description: Learn how to install and use DevTools from Android Studio.
description: 學習如何在 Android Studio 上使用開發者工具。
tags: Flutter開發工具,DevTools
keywords: Android Studio,安卓,除錯
---

## Install the Flutter plugin

## 安裝 Flutter 外掛

Install the Flutter plugin if you don't already have it installed.
This can be done using the normal **Plugins** page in the IntelliJ
and Android Studio settings. Once that page is open,
you can search the marketplace for the Flutter plugin.

請在第一次使用前安裝 Flutter 外掛，它可以在 Intellij 的 **Plugins** 
或 Android Studio 的設定中開啟，
頁面開啟後便可以在 marketPlace 中找到 Flutter 外掛。

## Start an app to debug

## 開始除錯一個應用

To open DevTools, you first need to run a Flutter app. 
This can be accomplished by opening a Flutter project,
ensuring that you have a device connected,
and clicking the **Run** or **Debug** toolbar buttons.

需要在啟動除錯工具前開啟一個 Flutter 應用，
它可以透過開啟 Flutter 專案實現，
首先要確保你已經完成裝置的連線，
然後只需點選工具欄的 **Run** 或 **Debug** 按鈕。

## Launch DevTools from the toolbar/menu

## 從工具欄/選單啟動除錯工具

Once an app is running,
you can start DevTools using one of the following:

應用啟動後，你可以透過以下幾種方式執行除錯工具：

* Select the **Open DevTools** toolbar action in the Run view.

  執行介面下，在工具欄選擇 **Open DevTools**（啟動除錯工具）。
  
* Select the **Open DevTools** toolbar action in the Debug view.
  (if debugging)
  
  除錯介面下，在工具欄選擇 **Open DevTools**。
  
* Select the **Open DevTools** action from the **More Actions** 
  menu in the Flutter Inspector view.

  Flutter Inspector 裡，在 **More Actions**
  的選單中選擇 **Open DevTools**。

![screenshot of Open DevTools button]({{site.url}}/assets/images/docs/tools/devtools/android_studio_open_devtools.png){:width="100%"}

## Launch DevTools from an action

## 從事件 (action) 中啟動除錯工具

You can also open DevTools from an IntelliJ action.
Open the **Find Action...** dialog
(on a Mac, press `Command+Shift+A)`, and search for the
**Open DevTools** action. When you select that action,
DevTools is installed (if it isn't already), the DevTools server
launches, and a browser instance opens pointing to the DevTools app.

你同樣可以在 IntelliJ 中執行除錯工具，
先開啟 **Find Action...** 對話方塊（Mac 上可以同時按下 'Command+Shift+A')，
然後查詢 **Open DevTools** 選項。
在除錯工具已安裝的前提下，相關服務會啟動，同時開啟指向待除錯應用的瀏覽器例項。

When opened with an IntelliJ action, DevTools is not connected
to a Flutter app. You'll need to provide a service protocol port
for a currently running app. You can do this using the inline
**Connect to a running app** dialog.

除錯工具在 IntelliJ 事件啟動時並不會直接連線到 Flutter 應用，
因此當前應用的服務協議埠是必須的，
你可以在 **Connect to a running app** 的對話方塊中開啟。
