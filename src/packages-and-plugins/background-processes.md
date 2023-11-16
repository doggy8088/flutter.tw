---
title: Background processes
title: 後臺處理序
description: Where to find more information on implementing background processes in Flutter.
description: 可以找到更多關於 Flutter 中後臺處理序實現的資訊的地方
tags: Packages,外掛
keywords: isolate,多執行緒,排程器
---

Have you ever wanted to execute Dart code in the
background—even if your app wasn't the currently active app?
Perhaps you wanted to implement a process that watches the time,
or that catches camera movement.
In Flutter, you can execute Dart code in the background.

當你的應用被切換到後臺時，是否仍希望它在後台可以執行一些業務邏輯？
在 Flutter 裡，你可以在應用被切換到後臺時執行一些程式碼邏輯。

The mechanism for this feature involves setting up an isolate.
_Isolates_ are Dart's model for multithreading,
though an isolate differs from a conventional thread
in that it doesn't share memory with the main program.
You'll set up your isolate for background execution using
callbacks and a callback dispatcher.

這個功能的機制主要是設定一個 isolate。**isolate** 是 Dart 中的多執行緒模型，
不過其與傳統執行緒的不同之處在於它不與主處理序共享記憶體。
你可以使用回呼(Callback)和回呼(Callback)排程器來設定 isolate，
從而使應用被切換進後臺時仍能執行一些業務。

Additionally, the [WorkManager][] plugin enables persistent background processing 
that keeps tasks scheduled through app restarts and system reboots. 

此外，[WorkManager][] 外掛可以實現持久化的後臺處理序，
應用和系統重啟之後還可以繼續執行計劃任務。

For more information and a geofencing example that uses background
execution of Dart code, see the Medium article by Ben Konyi,
[Executing Dart in the Background with Flutter Plugins and
Geofencing][background-processes].  At the end of this article,
you'll find links to example code, and relevant documentation for Dart,
iOS, and Android.

有關在後台處理序中使用 Dart 程式碼的 geofencing 案例，
你可以查閱釋出在 Flutter on Medium 上的一篇文章：
[Executing Dart in the Background with Flutter Plugins and Geofencing][background-processes]。
在這篇文章的最後，你可以找到範例程式碼的連結，
以及相關的 Dart、iOS 和 Android 文件。
 
[background-processes]: {{site.flutter-medium}}/executing-dart-in-the-background-with-flutter-plugins-and-geofencing-2b3e40a1a124
[WorkManager]: {{site.pub-pkg}}/workmanager 
