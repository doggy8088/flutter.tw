---
title: Debug your add-to-app module
title: 在混合開發模式下進行除錯
short-title: Debugging
short-title: 除錯應用
description: How to run, debug, and hot reload your add-to-app Flutter module.
description: 如何在使用 Flutter module 的混合應用執行除錯以及熱重載。
tags: Flutter混合工程,add2app
keywords: 工程除錯,VS Code
---

Once you've integrated the Flutter module to your project and used Flutter's
platform APIs to run the Flutter engine and/or UI,
you can then build and run your Android or iOS app the same way
you run normal Android or iOS apps.

當你將 Flutter 模組整合到專案中並使用 Flutter 的平台 APIs 來執行 Flutter 引擎和/或 UI 時，
你可以與平時執行 Android 或 iOS 應用程式一樣，建構和執行你的應用。

However, Flutter is now powering the UI in places where you're showing a
`FlutterActivity` or `FlutterViewController`.

但就目前而言，Flutter 需要在 `FlutterActivity` 或 `FlutterViewController` 中展示 UI 內容。

### Debugging

### 除錯

You might be used to having your suite of favorite Flutter debugging tools
available to you automatically when running `flutter run` or an equivalent
command from an IDE. But you can also use all your Flutter
[debugging functionalities][] such as hot reload, performance
overlays, DevTools, and setting breakpoints in add-to-app scenarios.

你可能習慣於在 IDE 中執行 `flutter run` 或者等效的快捷命令，
它會自動啟動你喜愛的 Flutter 除錯工具。
同樣的，你也可以使用所有 Flutter 的 [除錯功能][debugging functionalities]，
例如熱重載、效能除錯、DevTools 以及在混合開發的場景中設定斷點。

These functionalities are provided by the `flutter attach` mechanism.
`flutter attach` can be initiated through different pathways,
such as through the SDK's CLI tools,
through VS Code or IntelliJ/Android Studio.

這些功能由 `flutter attach` 機制提供。
`flutter attach` 可以透過不同的路徑啟動，
例如透過 SDK 中的命令列工具、VS Code 或者 IntelliJ/Android Studio。

`flutter attach` can connect as soon as you run your `FlutterEngine`, and
remains attached until your `FlutterEngine` is disposed. But you can invoke
`flutter attach` before starting your engine. `flutter attach` waits for
the next available Dart VM that is hosted by your engine.

`flutter attach` 可以在你執行 `FlutterEngine` 時立即進行連線，
並在 `FlutterEngine` 被釋放之前一直保持連線。
你可以在啟動引擎之前執行 `flutter attach`，
它將等待下一個由引擎持有的 Dart VM 進行連線。

#### Terminal

#### 終端

Run `flutter attach` or `flutter attach -d deviceId` to attach from the terminal.

在終端執行 `flutter attach` 或者 `flutter attach -d deviceId` 來連線你的應用。 

{% include docs/app-figure.md image="development/add-to-app/debugging/cli-attach.png" caption="flutter attach via terminal" %}

#### VS Code

{% include docs/debug/debug-flow-ios.md %}
{% include docs/debug/vscode-flutter-attach-json.md %}

#### IntelliJ / Android Studio

Select the device on which the Flutter module runs so `flutter attach` filters for the right start signals.

選擇要執行 Flutter 模組的裝置，然後點選右邊的 `flutter attach` 按鈕。

{% include docs/app-figure.md image="development/add-to-app/debugging/intellij-attach.png" caption="flutter attach via IntelliJ" %}

[debugging functionalities]: {{site.url}}/testing/debugging

### Wireless debugging

You can debug your app wirelessly on an iOS or Android device
using `flutter attach`.

#### iOS

On iOS, you must follow the steps below:

<ol markdown="1">
<li markdown="1">

Ensure that your device is wirelessly connected to Xcode
as described in the [iOS setup guide][].

</li>
<li markdown="1">

Open **Xcode > Product > Scheme > Edit Scheme**

</li>
<li markdown="1">

Select the **Arguments** tab

</li>
<li markdown="1">

Add either `--vm-service-host=0.0.0.0` for IPv4, 
or `--vm-service-host=::0` for IPv6 as a launch argument

You can determine if you're on an IPv6 network by opening your Mac's 
**Settings > Wi-Fi > Details (of the network you're connected to) > TCP/IP** 
and check to see if there is an **IPv6 address** section.

<img src="/assets/images/docs/development/add-to-app/debugging/wireless-port.png" alt="Check the box that says 'connect via network' from the devices and simulators page">

</li>
</ol>

#### Android

Ensure that your device is wirelessly connected to Android Studio 
as described in the [Android setup guide][].

[iOS setup guide]: {{site.url}}/get-started/install/macos#ios-setup
[Android setup guide]: {{site.url}}/get-started/install/macos#set-up-your-android-device
