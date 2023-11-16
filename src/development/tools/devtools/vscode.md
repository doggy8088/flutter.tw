---
title: Install and run DevTools from VS Code
title: 在 VS Code 裡安裝和使用開發者工具
description: Learn how to install and use DevTools from VS Code.
description: 學習如何在 VS Code 裡使用開發者工具。
tags: Flutter開發工具,DevTools
keywords: 開發者工具,VSCode,IDE,外掛安裝,啟動
---

## Install the VS Code extensions

## 安裝 VS Code 外掛

To use the DevTools from VS Code, you need the [Dart extension][].
If you're debugging Flutter applications, you should also install
the [Flutter extension][].

如果你想在 VS Code 中使用開發工具，你就一定需要安裝 [Dart 擴充][Dart extension]。
如果你還想要除錯 Flutter 應用程式，那你還應該安裝 [Flutter 擴充][Flutter extension]。

## Start an application to debug

## 進行除錯應用程式

Start a debug session for your application by opening the root
folder of your project (the one containing `pubspec.yaml`)
in VS Code and clicking **Run > Start Debugging** (`F5`).

透過在 VS Code 中開啟你的專案的根目錄
（包含 `pubspec.yaml`）並點選 **Run > Debugging** (`F5`)，
來開啟除錯會話。

## Launch DevTools

## 啟動開發工具

Once the debug session is active and the application has started,
the **Dart: Open DevTools** command becomes available in the
VS Code command palette:

一旦除錯會話處於活躍且應用程式已開啟，那麼 VS Code 命令控制板中將會顯示 **Dart: Open DevTools**：

![Screenshot showing Open DevTools command]({{site.url}}/assets/images/docs/tools/vs-code/vscode_command.png){:width="100%"}

The first time you run this (and subsequently when the DevTools package
is updated), you are prompted to activate or upgrade DevTools.

當你第一次執行時（以及未來更新開發工具套件時），系統會提醒你啟用或升級開發工具。

![Screenshot showing Active DevTools command]({{site.url}}/assets/images/docs/tools/vs-code/vscode_install_prompt.png){:width="100%"}

Clicking the **Open** button uses `pub global activate` to activate
the DevTools package for you. Next, DevTools launches in your browser and
automatically connects to your debug session.

接下來，開發工具將會在瀏覽器中啟動，並自動連線至你的除錯會話。

![Screenshot showing DevTools in a browser]({{site.url}}/assets/images/docs/tools/vs-code/vscode_show_in_browser.png){:width="100%"}

While DevTools is active, you'll see them in the status bar
of VS Code. If you've closed the browser tab,
you can click the status bar to re-launch your browser, so long
as there's still a suitable Dart/Flutter debugging session available.

當開發工具啟用後，你將可以在 VS Code 的狀態列中看到它們。
如果你已關閉瀏覽器選項卡，只要還有可用的 Dart/Flutter 除錯會話，
你也可以透過單擊狀態列來重新啟動瀏覽器。

![Screenshot showing DevTools in the VS Code status bar]({{site.url}}/assets/images/docs/tools/vs-code/vscode_status_bar.png){:width="100%"}

[Dart extension]: https://marketplace.visualstudio.com/items?itemName=Dart-Code.dart-code
[Flutter extension]: https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter
