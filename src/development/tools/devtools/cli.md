---
title: Install and run DevTools from the command line
title: 透過命令列安裝和執行開發者工具
Description: Learn how to install and use DevTools from the command line.
Description: 學習如何透過命令列工具安裝和使用開發者工具。
tags: Flutter開發工具
keywords: 命令列,啟動,開發者工具,DevTools
---

To run Dart DevTools from the CLI, you must have `dart` on your path. Then
you can run the following command to launch DevTools:

要從命令列或終端執行 Dart DevTools，`dart` 必須要能在環境變數中找到。
你可以執行以下命令啟動 DevTools：

```
dart devtools
```

To upgrade DevTools, upgrade your Dart SDK. If a newer Dart SDK
includes a newer version of DevTools, `dart devtools` will automatically
launch this version. If `which dart` points to the Dart SDK included in
your Flutter SDK, then DevTools will be upgraded when you upgrade your
Flutter SDK to a newer version.

你可以透過升級 Dart SDK 來升級 DevTools。
如果新的 Dart SDK 包括了新版本的 DevTools，`dart devtools` 命令會自動啟動新的版本。
如果 `which dart` 指向的是 Flutter SDK 中包含的 Dart SDK，那麼 DevTools 會在
Flutter SDK 更新時一併更新。

When you run DevTools from the command line, you should see output that
looks something like:

在命令列或終端執行 DevTools 時，你會看到類似下方的輸出：

```
Serving DevTools at http://127.0.0.1:9100
```

## Start an application to debug

## 啟動一個應用進行 debug

Next, start an app to connect to.
This can be either a Flutter application
or a Dart command-line application.
The command below specifies a Flutter app:

下一步，啟動並連線一個 app。可以是 Flutter app 或者一個 Dart 命令列應用。
下面這個命令是啟動一個 Flutter app:

```
cd path/to/flutter/app
flutter run
```

You need to have a device connected, or a simulator open,
for `flutter run` to work. Once the app starts, you'll see a
message in your terminal that looks like the following:

執行 `flutter run` 時，你需要連線一個裝置或者模擬器。
當 app 啟動後，你會在命令列中看到如下內容：

```
An Observatory debugger and profiler on iPhone X is available
at: http://127.0.0.1:50976/Swm0bjIe0ks=/
```

Keep note of this URL,
as you will use it to connect your app to DevTools.

記下這個 URL ，待會兒你可以使用它來連線 app 和開發者工具。

## Open DevTools and connect to the target app

## 開啟開發者工具並且連線到目標 app

Once it's set up, using DevTools is as simple as opening a
Chrome browser window and navigating to `http://localhost:9100`.

上述完成後，使用開發者工具就會很簡單，
只需開啟 chrome 並存取 `http://localhost:9100`。

Once DevTools opens, you should see a connect dialog:

當這個網頁開啟後，你會看到一個連結對話方塊：

![Screenshot of a logging view]({{site.url}}/assets/images/docs/tools/devtools/connect_dialog.png){:width="100%"}

Paste the URL you got from running your app (in this example,
`http://127.0.0.1:50976/Swm0bjIe0ks=/`) into the connect dialog
to connect your app to DevTools.

將從啟動 app 時獲得的 URL 連結
（在這個例子裡是 `http://127.0.0.1:50976/Swm0bjIe0ks=/` ）
複製到這個連結對話方塊中來把你的 app 和開發者工具連結起來。

This URL contains a security token, so it's different
for each run of your app. This means that if you stop your
application and re-run it, you need to connect to DevTools
with the new URL.

這個連結包含一個秘鑰 token，所以每次啟動你的 app 時，連結都會改變。
這意味著如果重啟 app 後，你需要用新的 URL 連結來連線開發者工具。
