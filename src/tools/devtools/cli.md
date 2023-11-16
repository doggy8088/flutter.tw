---
title: Install and run DevTools from the command line
title: 透過命令列安裝和執行開發者工具
description: Learn how to install and use DevTools from the command line.
description: 學習如何透過命令列工具安裝和使用開發者工具。
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
An Observatory debugger and profiler on macOS is available at:
http://127.0.0.1:52129/QjqebSY4lQ8=/
The Flutter DevTools debugger and profiler on macOS is available at:
http://127.0.0.1:9100?uri=http://127.0.0.1:52129/QjqebSY4lQ8=/
```

Open the DevTools instance connected to your app
by opening the second link in Chrome.

在 Chrome 瀏覽器中開啟第二個連結，
啟動連結到你應用的開發者工具例項。

This URL contains a security token, 
so it's different for each run of your app. 
This means that if you stop your application and re-run it, 
you need to connect to DevTools again with the new URL.

這個連結包含一個安全認證的 token，
所以每次啟動你的 app 時，連結都會改變。
這意味著如果重啟 app 後，
你需要用新的 URL 連結來連線開發者工具。

## Connect to a new app instance

## 連結到一個新的應用例項

If your app stops running
or you opened DevTools manually,
you should see a **Connect** dialog:

如果應用已經停止執行，或者你是手動開啟的開發者工具，
你應該可以看到一個 **Connect** 對話方塊。

![Screenshot of the DevTools connect dialog]({{site.url}}/assets/images/docs/tools/devtools/connect_dialog.png){:width="100%"}

You can manually connect DevTools to a new app instance
by copying the Observatory link you got from running your app,
such as `http://127.0.0.1:52129/QjqebSY4lQ8=/`
and pasting it into the connect dialog.

你可以將 Observatory 連結貼入到輸入框中，
手動將開發者工具連結到新的應用例項中去，
類似: `http://127.0.0.1:52129/QjqebSY4lQ8=/`。
