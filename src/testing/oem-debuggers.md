---
title: Using an OEM debugger
title: 使用原生的偵錯程式
short-title: debuggers
short-title: 偵錯程式
description: How to connect an OEM debugger to your running Flutter app.
description: 如何連線各開發平台的偵錯程式除錯 Flutter 應用。
tags: Flutter偵錯程式
keywords: Flutter處理錯誤,平台的偵錯程式,Flutter除錯,Flutter建構模式
---

If you are exclusively writing Flutter apps with
Dart code and not using platform-specific libraries,
or otherwise accessing platform-specific features,
you can debug your code using your IDE's debugger.
Only the first section of this guide, Debugging Dart code,
is relevant for you.

如果你只使用 Dart 語言開發 Flutter 應用，並且不使用特定於平台的的函式庫或者功能，
你可以使用 IDE 的偵錯程式除錯你的程式碼。
只有這篇指南的第一部分「除錯 Dart 程式碼」對你有用。

If you're writing a platform-specific plugin or
using platform-specific libraries written in Swift,
ObjectiveC, Java, or Kotlin, you can debug
that portion of your code using Xcode (for iOS)
or Android Gradle (for Android).
This guide shows you how you can connect _two_
debuggers to your Dart app, one for Dart,
and one for the OEM code.

如果你正在開發特定於平台的的外掛或者使用由
Swift、ObjectiveC、Java 或 Kotlin 語言編寫的特定於平台的函式庫，
你可以使用 Xcode（用於 iOS）或者 Android Gradle（用於 Android）除錯這部分程式碼。
本指南介紹如何將用於 Dart 和用於原生程式碼的 *兩個* 偵錯程式連線到你的 Dart 應用。

## Debugging Dart code

## 除錯 Dart 程式碼

Use your IDE for standard Dart debugging.
These instructions describe Android Studio,
but you can use your preferred IDE with the
Flutter and Dart plugins installed and configured.

你可以使用 IDE 進行一般的 Dart 除錯。以下內容針對 Android Studio 進行說明，
但你也可以使用你喜歡的安裝並配置好 Flutter 和 Dart 外掛的編輯器來進行除錯。

{{site.alert.tip}}

  Connect to a physical device when debugging,
  rather than an emulator or simulator,
  which don't support profile mode.
  For more information, see [Flutter's modes][].

  推薦連線到真機進行除錯，而不是使用不支援 profie 建構模式的模擬器或模擬器。
  更多資訊參考 [Flutter 的建構模式][Flutter's modes]。

{{site.alert.end}}

### Dart debugger

### Dart 偵錯程式

* Open your project in Android Studio.
  If you don't have a project yet,
  create one using the instructions in [Test drive][].

  使用 Android Studio 開啟你的專案。如果你還沒有專案，根據 [開發體驗初探][Test drive] 中的說明建立一個。

* Simultaneously bring up the Debug pane and
  run the app in the Console
  view by clicking the bug icon
  (<img src='/assets/images/docs/testing/debugging/oem/debug-run.png' alt='Debug-run icon'>).

  透過單擊除錯圖示 (<img src='/assets/images/docs/testing/debugging/oem/debug-run.png' alt='Debug-run icon'>)
  同時開啟除錯面板並在控制檯中執行應用。

  The first time you launch the app is the slowest.
  You should see the Debug pane appear at the bottom
  of the window that looks something like the following:

  首次執行應用是最慢的，你會發現視窗底部的除錯面板看起來會像這樣：

  <img src='/assets/images/docs/testing/debugging/oem/debug-pane.png' alt='Debug pane'>{:width="100%"}

  You can configure where the debug pane appears,
  or even tear it off to its own
  window using the gear to the right in the Debug pane bar.
  This is true for any inspector in Android Studio.

  你可以設定除錯面板的顯示位置，甚至可以用除錯面板右側的齒輪將其拆分到獨立的視窗。
  對於 Android Studio 中的任何檢查器都是如此。

* Add a breakpoint on the `counter++` line.

  在 `counter++` 這一行上新增斷點。

* In the app, click the **+** button
  (FloatingActionButton, or FAB, for short)
  to increment the counter. The app pauses.

  在應用裡，點選 **+** 按鈕
  （FloatingActionButton，或者簡稱 FAB）來增加數字，應用會暫停。

* The following screenshot shows:

  以下截圖顯示：

  * Breakpoint in the edit pane.

    編輯面板中的斷點。

  * State of the app in the debug pane, when paused at the breakpoint.

    當在斷點處暫停時，在除錯面板中顯示應用的狀態。

  * `this` variable expanded to display its values.

    `this` 變數展開並顯示其值。

  <img src='/assets/images/docs/testing/debugging/oem/debug-pane-action.png' alt='App status when hitting the set breakpoint'>{:width="100%"}

You can step in, out, and over Dart statements, hot reload or resume the app,
and use the debugger in the same way you'd use any debugger.
The **5: Debug** button toggles display of the debug pane.

你可以 step in/out/over Dart 陳述式、熱重載和恢復執行應用、
以及像使用其他偵錯程式一樣來使用 Dart 偵錯程式。
**5: Debug** 按鈕切換除錯面板的顯示。

[Test drive]: {{site.url}}/get-started/test-drive

### Flutter inspector

There are two other features provided by the
Flutter plugin that you might find useful.
The Flutter inspector is a tool for
visualizing and exploring
the Flutter widget tree and helps you:

Flutter 外掛提供了另外兩個可能給你提供幫助的功能。
Flutter inspector 是一個用來視覺化以及檢視 Flutter widget 樹的工具，
並幫助你：

* Understand existing layouts
  
  瞭解現有佈局

* Diagnose layout issues

  診斷佈局問題

Toggle display of the inspector using the
vertical button to the
right of the Android Studio window.

你可以使用 Android Studio 視窗右側的垂直按鈕切換檢查器的顯示。

<img src='/assets/images/docs/testing/debugging/oem/flutter-inspector.png' alt='Flutter inspector'>

### Flutter outline

The Flutter Outline displays the build method in visual form.
Note that this might be different than the widget tree for the
build method. Toggle display of the outline using the vertical
button to the right of the AS window.

Flutter Outline 以可視形式顯示建構方法。注意在建構方法上可能與 widget 樹不同。
你可以使用 Android Studio 視窗右側的垂直按鈕切換 outline 的顯示。

<img src='/assets/images/docs/testing/debugging/oem/flutter-outline.png' alt='screenshot showing the Flutter inspector'>{:width="100%"}

{% comment %}

TODO: Android Tips - How to assign a keyboard shortcut on the Mac?

TODO: Android 提示 - 在 Mac 上如何設定快捷鍵？

{% endcomment %}

The rest of this guide shows how to set up your environment to debug OEM
code. As you'd expect, the process works differently for iOS and Android.

這篇指南剩下的部分介紹瞭如何搭建原生程式碼的除錯環境。
你應該可以想象到，對於 iOS 和 Android 這個過程是不同的。

{% comment %}

Considere moving the info below to a new page.

考慮將下面的說明轉移到新的頁面。

{% endcomment %}

{{site.alert.tip}}

  Become a pro user of Android Studio by installing the **Presentation
  Assistant** plugin. You can find and install this plugin using
  **Preferences** > **Plugins** > **Browsing repositories...** and start entering
  _Presen_ in the search field.

  透過安裝 **Presentation Assistant** 外掛來成為 Android Studio 的專業使用者。
  你可以開啟 **Preferences** > **Plugins** > **Browsing repositories...** 
  並在搜尋框中輸入 **Presen** 來找到並安裝這個外掛。

  Once installed and AS is relaunched, this plugin helps you to become a
  pro user by:

  當你安裝並重啟 Android Studio 之後，
  透過使用以下功能這個外掛可以幫助你成為一個專業使用者：

  * Showing the name and Windows/Linux/Mac shortcuts of any action you
    invoke.

    顯示你執行的任何操作的名字和 Windows/Linux/Mac 的快捷鍵。

  * Allowing you to search and find available actions, settings, docs,
    and so on.

    允許你搜索並找到可用的操作、設定、文件等等。

  * Allowing you to toggle preferences, open up views, or run actions.

    允許你切換首選項，開啟檢視或者執行操作。

  * Allowing you to assign keyboard shortcuts (?? Can't make this work on
    Mac.)

    允許你設定鍵盤快捷鍵。（無法在 Mac 上執行此功能？）

  For example, try this:

  例如，嘗試下這個：

  * While focus is in the Edit pane, enter **command-Shift-A** (Mac) or
    **shift-control-A** (Windows and Linux).
    The plugin simultaneously brings up the Find panel and shows a hint for
    performing this same operation on all three platforms.

    當焦點在編輯面板中時，
    輸入 **command-Shift-A**（Mac）或者 **shift-control-A**（Windows 和 Linux）。
    該外掛會同時顯示「查詢」面板並顯示在所有三個平臺上執行此操作的提示。

    <img src='/assets/images/docs/testing/debugging/oem/presentation-assistant-find-pane.png' alt='Find panel'>{:width="100%"}
    <center>Presentation assistant 的「查詢」面板</center>

    <img src='/assets/images/docs/testing/debugging/oem/presentation-assistant-teaches.png' alt='Find pane'>{:width="100%"}
    <center>Presentation assistant 的在 Mac、Windows 和 Linux 上開啟「查詢」面板的操作提示。</center>

  * Enter _attach_ to see the following:

    輸入 **attach** 顯示以下內容：

    <img src='/assets/images/docs/testing/debugging/oem/presentation-assistant-search-results.png' alt='Find panel'>

  * After an update, you might enter _Flutter_ or _Dart_ to see if
    new actions are available.

    更新之後，你可以輸入 **Flutter** 或者
    **Dart** 來檢視是否有新的可用的操作。

  Hide the Presentation Assistant's Find panel by using **Escape**.

  使用 **Escape** 隱藏 Presentation Assistant 的「查詢」面板。

{{site.alert.end}}


## Debugging with Android Gradle (Android)

## 使用 Android Gradle 除錯（Android）

In order to debug OEM Android code, you need an app that contains
OEM Android code. In this section, you'll learn how to connect
two debuggers to your app: 1) the Dart debugger and,
2) the Android Gradle debugger.

為了除錯原生程式碼，你需要一個包含 Android 原生程式碼的應用。
在本節中，你將學會如何連線兩個偵錯程式到你的應用：
1）Dart 偵錯程式，和 2）Android Gradle 偵錯程式。

* Create a basic Flutter app.
  
  建立一個基本的 Flutter 應用。

* Replace `lib/main.dart` with the following example code from the
  [`url_launcher`][] package:

  替換 `lib/main.dart` 為來自 
  [`url_launcher`]({{site.pub}}/packages/url_launcher) 套件的以下範例程式碼：

{% prettify dart %}
// Copyright 2017 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

import 'dart:async';

import 'package:flutter/material.dart';
import 'package:url_launcher/url_launcher.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'URL Launcher',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(title: 'URL Launcher'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  Future<void> _launched;

  Future<void> _launchInBrowser(String url) async {
    if (await canLaunch(url)) {
      await launch(url, forceSafariVC: false, forceWebView: false);
    } else {
      throw 'Could not launch $url';
    }
  }

  Future<void> _launchInWebViewOrVC(String url) async {
    if (await canLaunch(url)) {
      await launch(url, forceSafariVC: true, forceWebView: true);
    } else {
      throw 'Could not launch $url';
    }
  }

  Widget _launchStatus(BuildContext context, AsyncSnapshot<void> snapshot) {
    if (snapshot.hasError) {
      return Text('Error: ${snapshot.error}');
    } else {
      return Text('');
    }
  }

  @override
  Widget build(BuildContext context) {
    String toLaunch = 'https://flutter.dev';
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Padding(
              padding: EdgeInsets.all(16.0),
              child: Text(toLaunch),
            ),
            ElevatedButton(
              onPressed: () => setState(() {
                    _launched = _launchInBrowser(toLaunch);
                  }),
              child: Text('Launch in browser'),
            ),
            Padding(padding: EdgeInsets.all(16.0)),
            ElevatedButton(
              onPressed: () => setState(() {
                    _launched = _launchInWebViewOrVC(toLaunch);
                  }),
              child: Text('Launch in app'),
            ),
            Padding(padding: EdgeInsets.all(16.0)),
            FutureBuilder<void>(future: _launched, builder: _launchStatus),
          ],
        ),
      ),
    );
  }
}
{% endprettify %}

* Add the `url_launcher` dependency to the pubspec file,
  and run flutter pub get:

  新增 `url_launcher` 依賴到 pubspec 檔案，並執行 `flutter pub get`：

```yaml
name: flutter_app
description: A new Flutter application.
version: 1.0.0+1

dependencies:
  flutter:
    sdk: flutter

  url_launcher: ^3.0.3
  cupertino_icons: ^0.1.2

dev_dependencies:
  flutter_test:
    sdk: flutter
```

[`url_launcher`]: {{site.pub}}/packages/url_launcher

* Click the debug icon
  (<img src='/assets/images/docs/testing/debugging/oem/debug-run.png' alt='Debug-run icon'>)
  to simultaneously bring up the Debug pane and launch the app.
  Wait for the app to launch on the device, and for the debug pane to
  indicate **Connected**.
  (This can take a minute the first time but is faster for subsequent
   launches.) The app contains two buttons: 1) **Launch in browser**
   opens flutter.dev in your phone's default browser and 2) **Launch
   in app** opens flutter.dev within your app.

   點選除錯按鈕
   (<img src='/assets/images/docs/testing/debugging/oem/debug-run.png' alt='Debug-run icon'>)
   來同時開啟除錯面板並啟動應用。
   等待應用在裝置上啟動並在除錯面板中顯示 **Connected**。
   （第一次可能需要一分鐘，但是之後的啟動會變快。）
   應用包含兩個按鈕：
   1）**Launch in browser** 在你的手機預設瀏覽器中開啟 flutter.dev 網站 
   2）**Launch in app** 在你的應用中開啟 flutter.dev 網站。

  <img src='/assets/images/docs/testing/debugging/oem/launch-flutter-dev.png' alt='screenshot containing two buttons for opening flutter.dev'>

* Click the **Attach debugger to Android process** button (
  <img src='/assets/images/docs/testing/debugging/oem/attach-process-button.png' alt='looks like a rectangle superimposed with a tiny green bug'> )

  點選 **Attach debugger to Android process** 按鈕
  <img src='/assets/images/docs/testing/debugging/oem/attach-process-button.png' alt='looks like a rectangle superimposed with a tiny green bug'> )

{{site.alert.tip}}

  If this button doesn't appear in the Projects menu bar, make sure that
  you are inside a Flutter project but <em>not a Flutter plugin</em>.

  如果這個按鈕沒有顯示在 Projects 選單欄上，
  確定你正在使用的是 Flutter 專案而<em>不是 Flutter 外掛</em>。

{{site.alert.end}}

* From the process dialog, you should see an entry for each connected device.
  Select **show all processes** to display available processes for each
  device.

  從處理序對話方塊中，你應該可以看到每一個裝置的入口。
  選擇 **show all processes** 來顯示每個裝置可用的處理序。

* Choose the process you want to attach to. In this case, it's the
  `com.google.clickcount`
   (or <strong>com.<em>company</em>.<em>app_name</em></strong>)
   process for the Motorola Moto G.

  選擇你想附加到的處理序。
  在這個例子中是 Motorola Moto G 的 `com.google.clickcount`
  （或 <strong>com.<em>company</em>.<em>app_name</em></strong>）處理序。
  
  <img src='/assets/images/docs/testing/debugging/oem/choose-process-dialog.png' alt='screenshot containing two buttons for opening flutter.dev'>{:width="100%"}

* In the debug pane, you should now see a tab for **Android Debugger**.

  在除錯面板中，你現在應該可以看到一個 **Android Debugger** 標籤頁。

* In the project pane, expand
  <nobr><strong><em>app_name</em> > android > app > src > main >
  java > io.flutter plugins</strong></nobr>.
  Double click **GeneratedProjectRegistrant** to open the
  Java code in the edit pane.

  在專案面板，展開 <strong><em>app_name</em> > android > app > src > main > java > io.flutter plugins</strong>。
  雙擊 **GeneratedProjectRegistrant** 在編輯面板中開啟 Java 程式碼。

Both the Dart and OEM debuggers are interacting with the same process.
User either, or both, to set breakpoints, examine stack, resume execution...
In other words, debug!

Dart 和原生偵錯程式都在與同一個處理序互動。
使用其中一個或者同時使用兩個來設定斷點、檢查堆疊、恢復執行……
換句話說，除錯！

  <img src='/assets/images/docs/testing/debugging/oem/dart-debugger.png' alt='screenshot of Android Studio in the Dart debug pane.'>{:width="100%"}
  <br><center>Dart 除錯面板和 `lib/main.dart` 中的兩個斷點。</center>

  <center>Dart 除錯面板和 `lib/main.dart` 中的兩個斷點。</center>

  <img src='/assets/images/docs/testing/debugging/oem/android-debugger.png' alt='screenshot of Android Studio in the Android debug pane.'>{:width="100%"}
  <br><center>Android 除錯面板和 `GeneratedPluginRegistrant.java` 中的一個斷點。
  透過單擊除錯面板頂部的相應偵錯程式，在偵錯程式之間進行切換。</center>

## Debugging with Xcode (iOS)

## 使用 Xcode 除錯（iOS）

In order to debug OEM iOS code, you need an app that contains OEM iOS code.
In this section, you'll learn how to connect two debuggers to your app:
1) the Dart debugger and, 2) the Xcode debugger.

為了除錯原生 iOS 程式碼，你需要一個包含原生 iOS 程式碼的應用。
在本節中，你將學會如何連線兩個偵錯程式到你的應用：
1）Dart 偵錯程式
2）Xcode 偵錯程式。

[PENDING]

## Resources

## 資源

The following resources have more information on debugging Flutter,
iOS, and Android:

下面的資源包含更多關於 Flutter、iOS 和 Android 除錯的資訊。

### Flutter

* [Debugging Flutter apps][]
 
  [除錯 Flutter 應用][Debugging Flutter apps]

* [Flutter inspector][], as well as the general
  [DevTools][] docs.

  [開發者工具][DevTools] 裡的 [Flutter inspector][]

* [Performance Profiling][]

  [Flutter 效能分析][Performance profiling]

[Debugging Flutter apps]: {{site.url}}/testing/debugging
[Performance profiling]: {{site.url}}/perf/ui-performance

### Android

You can find the following debugging resources on
[developer.android.com][].

你可以在 [developer.android.com]({{site.android-dev}}) 找到下列的除錯資源。

* [Debug your app][]

  [除錯你的應用][Debug your app]

* [Android Debug Bridge (adb)][]

  [Android 除錯橋 (adb)][Android Debug Bridge (adb)]

### iOS

You can find the following debugging resources on
[developer.apple.com][].

你可以在 [developer.apple.com](https://developer.apple.com) 找到下列的除錯資源。

* [Debugging][]
 
  [除錯](https://developer.apple.com/cn/support/debugging/)

* [Instruments Help][]

[Android Debug Bridge (adb)]: {{site.android-dev}}/studio/command-line/adb
[Debug your app]: {{site.android-dev}}/studio/debug
[Debugging]: {{site.apple-dev}}/support/debugging/
[developer.android.com]: {{site.android-dev}}
[developer.apple.com]: {{site.apple-dev}}
[DevTools]: {{site.url}}/development/tools/devtools
[Flutter inspector]: {{site.url}}/development/tools/devtools/inspector
[Flutter's modes]: {{site.url}}/testing/build-modes
[Instruments Help]: https://help.apple.com/instruments/mac/current/
