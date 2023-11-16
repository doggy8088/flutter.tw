---
title: Android Studio and IntelliJ
title: 在 Android Studio 或 IntelliJ 裡開發 Flutter 應用
description: >
  How to develop Flutter apps in Android Studio or other IntelliJ products.
description: 如何在 Android Studio 或者其他類 IntelliJ 產品裡開發 Flutter 應用。
tags: SDK,Flutter SDK
keywords: Android Studio,IntelliJ,安裝,檢查更新,建立新專案
---

<ul class="nav nav-tabs" id="ide" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" role="tab" aria-selected="true">Android Studio and IntelliJ</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="{{site.url}}/tools/vs-code" role="tab" aria-selected="false">Visual Studio Code</a>
  </li>
</ul>

## Installation and setup

## 安裝和設定

Follow the [Set up an editor]({{site.url}}/get-started/editor?tab=androidstudio)
instructions to install the Dart and Flutter plugins.

按照 [編輯工具設定]({{site.url}}/get-started/editor?tab=androidstudio)，安裝 Dart 和 Flutter 外掛。

### Updating the plugins {#updating}

### 更新外掛 {#updating}

### 更新外掛

Updates to the plugins are shipped on a regular basis.
You should be prompted in the IDE when an update is available.

外掛的更新會定期釋出，當有更新可用時，你會在 IDE 中收到提示。

To check for updates manually:

手動檢查更新：

 1. Open preferences (**Android Studio > Check for Updates** on macOS,
    **Help > Check for Updates** on Linux).

    開啟設定（在 macOS 中點選 **Android Studio > Check for Updates**，在 Linux 中點選
    **Help > Check for Updates**）。

 1. If `dart` or `flutter` are listed, update them.

	如果存在 `dart` 或 `flutter`，更新他們。

## Creating projects

## 建立專案

You can create a new project in one of several ways.

你可以透過多種方式來建立新專案。

### Creating a new project

### 建立新專案

Creating a new Flutter project from the Flutter starter app template
differs between Android Studio and IntelliJ.

從 Flutter 起始應用範本建立新 Flutter 專案在
Android Studio 和 IntelliJ 中有所不同。

**In Android Studio:**

**在 Android Studio 中：**

 1. In the IDE, click **New Flutter Project** from the **Welcome** window or
    **File > New > New Flutter Project** from the main IDE window.

    在 IDE 中，點選 **Welcome** 視窗中的 **New Flutter Project**，或者主視窗中的

 1. Specify the **Flutter SDK path** and click **Next**.

    指定 **Flutter SDK path**，點選 **Next**。

 1. Enter your desired **Project name**, 
    **Description**, and **Project location**.

    輸入你想要的 **Project name**，**Description**
    和 **Project location**。

 1. If you might publish this app, [set the company domain](#note).

    如果打算釋出此應用，需要 [設定公司域名](#note)。

 1. Click **Finish**.

    點選 **Finish**。

**In IntelliJ:**

**在 IntelliJ 中：**

 1. In the IDE, click **New Project** from the **Welcome** window or
    **File > New > Project** from the main IDE window.

    在 IDE 中，點選 **Welcome** 視窗中的 **New Project**，或者主視窗中的
    **File > New > Project**。

 1. Select **Flutter** from the **Generators** list in the left panel

    在左側面板的 **Generators** 列表中選擇 **Flutter**。

 1. Specify the **Flutter SDK path** and click **Next**.

    指定 **Flutter SDK path**，點選 **Next**。

 1. Enter your desired **Project name**,
    **Description**, and **Project location**.

    輸入你想要的 **Project name**，**Description**

 1. If you might publish this app, [set the company domain](#note).

    如果打算釋出此應用，需要 [設定公司域名](#note)。

 1. Click **Finish**.

    點選 **Finish**。

{{site.alert.secondary}}

  <h4 id="note" class="no_toc">Setting the company domain</h4>

  <h4 id="note" class="no_toc">設定公司域名</h4>

  When creating a new app, some Flutter IDE plugins ask for an
  organization name in reverse domain order,
  something like `com.example`. Along with the name of the app,
  this is used as the package name for Android, and the Bundle ID for iOS
  when the app is released. If you think you might ever release this app,
  it is better to specify these now. They cannot be changed once the app
  is released. Your organization name should be unique.

  在建立新應用時，一些 Flutter IDE 外掛需要一個逆序的域名，比如 `com.example`。
  除了程式名外，在應用釋出後，它將作為 Android 應用的套件名稱，以及 iOS 應用的 Bundle ID。
  如果你可能釋出此應用，最好現在就指定好它，應用釋出後將無法更改。你的域名應該是唯一的。

{{site.alert.end}}

### Opening a project from existing source code

### 從現有原始碼建立新專案

To open an existing Flutter project:

開啟現有的 Flutter 專案：

 1. In the IDE, click **Open** from the **Welcome** window, or
    **File > Open** from the main IDE window. 

    在 IDE 中，點選 **Welcome** 視窗中的 **Open**，或者主視窗中的
    **File > Open**。

 1. Browse to the directory holding your existing
    Flutter source code files.

    瀏覽到現有 Flutter 原始碼的檔案目錄。

 1. Click **Open**.

    點選 **Open**。

    {{site.alert.important}}

      Do *not* use the **New > Project from existing sources**
      option for Flutter projects.

      對於 Flutter 專案，請 **不要** 使用 **New > Project from existing sources**。

    {{site.alert.end}}


## Editing code and viewing issues

## 編輯程式碼，和檢視問題

The Flutter plugin performs code analysis that enables the following:

Dart 外掛的程式碼分析，可以做到：

* Syntax highlighting.

  語法高亮顯示。

* Code completions based on rich type analysis.

  基於多種型別分析的程式碼自動完成。

* Navigating to type declarations (**Navigate > Declaration**),
  and finding type usages (**Edit > Find > Find Usages**).

  定位到型別的宣告（**Navigate > Declaration**），
  查詢型別的參考（**Edit > Find > Find Usages**）。

* Viewing all current source code problems
  (**View > Tool Windows > Dart Analysis**).
  Any analysis issues are shown in the Dart Analysis pane:<br>
  ![Dart Analysis pane]({{site.url}}/assets/images/docs/tools/android-studio/dart-analysis.png){:width="90%"}

  檢視當前存在的程式碼問題（**View > Tool Windows > Dart Analysis**），
  所有問題會在 Dart Analysis 視窗中顯示<br>
  ![Dart Analysis 視窗]({{site.url}}/assets/images/docs/tools/android-studio/dart-analysis.png){:width="90%"}

## Running and debugging

## 執行和除錯

{{site.alert.note}}

  You can debug your app in a few ways.
  
  你可以透過如下方式除錯你的應用：

  * Using [DevTools][], a suite of debugging and profiling
    tools that run in a browser
    _and include the Flutter inspector_.

    使用 [開發者工具 (DevTools)][DevTools], 
    執行在瀏覽器裡的一系列除錯和分析工具，
    **也包括 Flutter inspector**。
    
  * Using Android Studio's (or IntelliJ's) built-in debugging
    features, such as the ability to set breakpoints.

    使用 Android Studio（或者 IntelliJ）內建的除錯功能，
    比如設定斷點等。
    
  * Using the Flutter inspector, directly available in
    Android Studio and IntelliJ.

    使用 Flutter inspector, 在 Android Studio 和 IntelliJ 內建。

  The instructions below describe features available in Android
  Studio and IntelliJ. For information on launching DevTools,
  see [Running DevTools from Android Studio][] in the
  [DevTools][] docs.
  
  下面的介紹文章適用於 Android Studio 和 IntelliJ，更多關於開發者工具的資訊，
  請參看文件：[在 Android Studio 上安裝和執行開發者工具][Running DevTools from Android Studio]。
  
{{site.alert.end}}

Running and debugging are controlled from the main toolbar:

在主工具欄，可以執行和除錯程式碼：

![Main IntelliJ toolbar]({{site.url}}/assets/images/docs/tools/android-studio/main-toolbar.png){:width="90%"}

### Selecting a target

### 選擇目標裝置

When a Flutter project is open in the IDE, you should see a set of
Flutter-specific buttons on the right-hand side of the toolbar.

在 IDE 中開啟 Flutter 專案時，你會在工具欄的右側看到一組 Flutter 的特定按鈕。

{{site.alert.note}}

  If the Run and Debug buttons are disabled, and no targets are listed,
  Flutter has not been able to discover any connected iOS or
  Android devices or simulators.
  You need to connect a device, or start a simulator, to proceed.

  如果 Run 和 Debug 按鈕不可用，且未顯示目標裝置，則意味著
  Flutter 未發現任何已連線的 iOS 、Android 裝置或模擬器。
  你需要連線裝置或啟動模擬器才能繼續。

{{site.alert.end}}

 1. Locate the **Flutter Target Selector** drop-down button.
    This shows a list of available targets.

    找到**選擇目標**下拉按鈕，點選它會顯示出可用裝置列表。

 2. Select the target you want your app to be started on.
    When you connect devices, or start simulators,
    additional entries appear.

    選擇你希望啟動應用的裝置。當連線裝置或啟動模擬器時，
    列表中將會加入新選項。

### Run app without breakpoints

### 不使用斷點執行應用

 1. Click the **Play icon** in the toolbar, or invoke **Run > Run**.
    The bottom **Run** pane shows logs output.

    點選工具欄中的 **Play** 按鈕，或選擇 **Run > Run**。
    底部的 **Run** 視窗會有日誌輸出：<br>

### Run app with breakpoints

### 使用斷點執行應用

 1. If desired, set breakpoints in your source code.

    如果需要，在原始碼中設定斷點。

 1. Click the **Debug icon** in the toolbar, or invoke **Run > Debug**.

    點選工具欄中的 **Debug** 按鈕，或選擇 **Run > Debug**。

    * The bottom **Debugger** pane shows Stack Frames and Variables.

      底部的 **Debugger** 視窗會顯示出堆疊和變數資訊。

    * The bottom **Console** pane shows detailed logs output.

      底部的 **Console** 視窗會顯示詳細的日誌輸出。

    * Debugging is based on a default launch configuration.
      To customize this, click the drop-down button to the right
      of the device selector, and select **Edit configuration**.

      除錯基於預設的啟動配置，如果需要自訂，點選**選擇目標**下拉按鈕，
      選擇 **Edit configuration** 進行配置。

## Fast edit and refresh development cycle

## 快速編輯和檢視效果

Flutter offers a best-in-class developer cycle enabling you to see the effect
of your changes almost instantly with the _Stateful Hot Reload_ feature.
To learn more, check out [Hot reload][].

Flutter 有效加快開發週期。使用 **熱重載** 功能，你可以在修改原始碼後，幾乎馬上看到效果。
詳細資訊請查閱 [使用熱重載][Hot reload]。

### Show performance data

### 顯示效能資料

{{site.alert.note}}

  To examine performance issues in Flutter, see the
  [Timeline view][].

  檢查 Flutter 裡的效能問題，請檢視
  [時間線檢視文件][Timeline view]。

{{site.alert.end}}

To view the performance data, including the widget rebuild
information, start the app in **Debug** mode, and then open
the Performance tool window using
**View > Tool Windows > Flutter Performance**.

在 **Debug** 模式下啟動應用後，使用
**View > Tool Windows > Flutter Performance**
開啟效能工具視窗，以檢視效能資料以及 widget 的 rebuild 資訊。

![Flutter performance window]({{site.url}}/assets/images/docs/tools/android-studio/widget-rebuild-info.png){:width="90%"}

To see the stats about which widgets are being rebuilt, and how often,
click **Show widget rebuild information** in the **Performance** pane.
The exact count of the rebuilds for this frame displays in the second
column from the right. For a high number of rebuilds, a yellow spinning
circle displays. The column to the far right shows how many times a
widget was rebuilt since entering the current screen.
For widgets that aren't rebuilt, a solid grey circle displays.
Otherwise, a grey spinning circle displays.

點選 **Performance** 視窗中的 **Show widget rebuild information**，
檢視正在重載的 widget 統計資訊和重載頻率。
右邊第二列顯示了所在框架的重載次數。
如果重載次數過多，會顯示一個黃色旋轉圓圈。
最右一列顯示了進入當前頁面後 widget 的重載次數。
對於未重載的小部件，將顯示一個灰色圓圈，否則將顯示一個灰色旋轉圓圈。

{{site.alert.secondary}}

  The app shown in this screenshot has been designed to deliver
  poor performance, and the rebuild profiler gives you a clue
  about what is happening in the frame that might cause poor
  performance. The widget rebuild profiler is not a diagnostic
  tool, by itself, about poor performance.

  截圖中的應用效能較差，透過重載分析器，你可以找到導致效能差的線索。
  重載分析器不是一個性能診斷工具，但它和效能有關。

{{site.alert.end}}

The purpose of this feature is to make you aware when widgets are
rebuilding&mdash;you might not realize that this is happening when just
looking at the code. If widgets are rebuilding that you didn't expect,
it's probably a sign that you should refactor your code by splitting
up large build methods into multiple widgets.

該功能的目的是讓你瞭解 widget 是何時重載的，只看程式碼的話可能不好發現。
如果 widget 在你預想不到的情況下發生了重載，
說明你可能需要重構程式碼，將大型的建構方法拆分成多個 widget。

This tool can help you debug at least four common performance issues:

該工具可以幫助你除錯至少四個常見的效能問題：

1. The whole screen (or large pieces of it) are built by a single
   StatefulWidget, causing unnecessary UI building. Split up the
   UI into smaller widgets with smaller `build()` functions.

   整個螢幕（或大部分螢幕）由一個 StatefulWidget 構成，導致不必要的 UI 建構。
   可將 UI 拆分成多個具有較輕量 `build()` 方法的 widget。

1. Offscreen widgets are being rebuilt. This can happen, for example,
   when a ListView is nested in a tall Column that extends offscreen.
   Or when the RepaintBoundary is not set for a list that extends
   offscreen, causing the whole list to be redrawn.

   未在螢幕上顯示的 widget 發生了重載。例如，一個延伸到螢幕外的 ListView，
   或者未給延伸到螢幕外的列表設定 RepaintBoundary，會導致重繪整個列表。

1. The `build()` function for an AnimatedBuilder draws a subtree that
   does not need to be animated, causing unnecessary rebuilds of static
   objects.

   AnimatedBuilder 的 `build()` 方法繪製了一個不需要動畫的子樹，
   導致不必要的靜態物件重載。

1. An Opacity widget is placed unnecessarily high in the widget tree.
   Or, an Opacity animation is created by directly manipulating the
   opacity property of the Opacity widget, causing the widget itself
   and its subtree to rebuild.

   一個 Opacity widget 在 widget tree 中使用了一個不必要的高度，
   或者透過直接操作 Opacity widget 的透明屬性建立 Opacity 動畫，
   導致 widget 和它的子樹重載。

You can click on a line in the table to navigate to the line
in the source where the widget is created. As the code runs,
the spinning icons also display in the code pane to help you
visualize which rebuilds are happening.

你可以點選表格中的一行，定位到建立指定 widget 的原始碼位置。
隨著程式碼的執行，旋轉圖示也會在程式碼視窗中顯示，以幫助你觀察正在進行的重載。

Note that numerous rebuilds doesn't necessarily indicate a problem.
Typically you should only worry about excessive rebuilds if you have
already run the app in profile mode and verified that the performance
is not what you want.

大量的重載並不一定表示存在問題。
通常情況下，只有當你透過分析發現效能不理想時，才需要考慮過度重載的問題。

And remember, _the widget rebuild information is only available in
a debug build_. Test the app's performance on a real device in a profile
build, but debug performance issues in a debug build.

記住，**widget 的重載資訊只在 debug 版本中可用**，
在真機上使用分析建構 (profile build) 進行應用效能分析，
使用除錯建構 (debug build) 進行效能問題除錯。

## Editing tips for Flutter code

## Flutter 程式碼編輯提示

If you have additional tips we should share, [let us know][]!

如果你有其他我們應該提供的程式碼提示建議，請 [告訴我們][let us know]!

### Assists & quick fixes

### 程式碼輔助和快速修復

Assists are code changes related to a certain code identifier.
A number of these are available when the cursor is placed on a
Flutter widget identifier, as indicated by the yellow lightbulb icon.
The assist can be invoked by clicking the lightbulb, or by using the
keyboard shortcut (`Alt`+`Enter` on Linux and Windows,
`Option`+`Return` on macOS), as illustrated here:

程式碼輔助功能是特定程式碼識別符號相關的程式碼修改。
當游標放在 Flutter widget 上時，黃色燈泡圖示會指示可用的修改，
可以透過點選燈泡進行修改，
或使用鍵盤快捷鍵
（在 Linux 和 Windows 上使用 `Alt`+`Enter`，在 macOS 上使用 `Option`+`Return`），
如下圖所示：

![IntelliJ editing assists]({{site.url}}/assets/images/docs/tools/android-studio/assists.gif){:width="100%"}

Quick Fixes are similar, only they are shown with a piece of code has an error
and they can assist in correcting it. They are indicated with a red lightbulb.

Quick Fixes 快速修復功能也是類似的，當一段程式碼存在錯誤時，
它會出現並幫助糾正錯誤。它使用紅色燈泡表示。

#### Wrap with new widget assist

#### Widget 巢狀(Nesting)輔助

This can be used when you have a widget that you want to wrap in a surrounding
widget, for example if you want to wrap a widget in a `Row` or `Column`.

當你有一個 widget 需要巢狀(Nesting)在其他 widget 時，可以使用該功能。
例如，需要將 widget 巢狀(Nesting)在 `Row` 或 `Column` 中。

#### Wrap widget list with new widget assist

####  Widget 列表巢狀(Nesting)輔助

Similar to the assist above, but for wrapping an existing list of
widgets rather than an individual widget.

和上面的輔助類似，但它巢狀(Nesting)的是一個 widget 的列表，而不是單個的 widget。

#### Convert child to children assist

#### child 和 children 轉換輔助

Changes a child argument to a children argument,
and wraps the argument value in a list.

將 child 轉換成 children，並且把引數值寫進一個 list。

### Live templates

### 即時範本

Live templates can be used to speed up entering typical code structures.
They are invoked by typing their prefix, and then selecting it in the code
completion window:

即時範本用於增加典型程式碼結構的輸入速度。輸入字首後，在程式碼完成視窗中選擇它：

![IntelliJ live templates]({{site.url}}/assets/images/docs/tools/android-studio/templates.gif){:width="100%"}

The Flutter plugin includes the following templates:

Flutter 外掛包含了以下範本：

* Prefix `stless`: Create a new subclass of `StatelessWidget`.

  字首 `stless`：建立一個 `StatelessWidget` 的子類別。

* Prefix `stful`: Create a new subclass of `StatefulWidget` and
  its associated State subclass.

  字首 `stful`：建立一個 `StatefulWidget` 的子類別，並關聯 State 子類別。

* Prefix `stanim`: Create a new subclass of `StatefulWidget` and its
  associated State subclass, including a field initialized with an
  `AnimationController`.

  字首 `stanim`：建立一個 `StatefulWidget` 的子類別，並關聯 State 子類別，
  包含一個 `AnimationController` 的初始化欄位。

You can also define custom templates in **Settings > Editor > Live Templates**.

你還可以透過 **Settings > Editor > Live Templates** 定義自訂範本。

### Keyboard shortcuts

### 鍵盤快捷鍵

**Hot reload**

**熱重載**

On Linux (keymap _Default for XWin_) and Windows the keyboard shortcuts
are `Control`+`Alt`+`;` and `Control`+`Backslash`.

在 Linux（對映方案預設為 **XWin**）和 Windows 上，
快捷鍵是 `Control`+`Alt`+`;` 和 `Control`+`Backslash`。

On macOS (keymap _Mac OS X 10.5+ copy_) the keyboard shortcuts are
`Command`+`Option` and `Command`+`Backslash`.

在 macOS 上（對映方案 **Mac OS X 10.5+**）上，
快捷鍵是 `Command`+`Option` 和 `Command`+`Backslash`。

Keyboard mappings can be changed in the IDE Preferences/Settings: Select
*Keymap*, then enter _flutter_ into the search box in the upper right corner.
Right click the binding you want to change and _Add Keyboard Shortcut_.

可以在 IDE 的設定中修改快捷鍵：選擇 **Keymap** 後，
在右上角的搜尋框輸入 **flutter**。
右鍵點選你想修改的快捷鍵，點選 **Add Keyboard Shortcut**

![IntelliJ settings keymap]({{site.url}}/assets/images/docs/tools/android-studio/keymap-settings-flutter-plugin.png){:width="100%"}

### Hot reload vs. hot restart

### 熱重載和熱重啟

Hot reload works by injecting updated source code files into the running
Dart VM (Virtual Machine). This includes not only adding new classes,
but also adding methods and fields to existing classes,
and changing existing functions.
A few types of code changes cannot be hot reloaded though:

熱重載的工作原理是將更新後的程式碼注入 Dart VM（虛擬機器）。
不僅包括新增新類，還包括新增方法和欄位到已有的類中。
但有些型別的程式碼是無法被熱重載的：

* Global variable initializers

  全部變數的初始化

* Static field initializers

  靜態變數的初始化

* The `main()` method of the app

  應用的 `main()` 方法

For these changes you can fully restart your application,
without having to end your debugging session. To perform a hot restart,
don't click the Stop button, simply re-click the Run button (if in a run
session) or Debug button (if in a debug session), or shift-click the 'hot
reload' button.

對於這些更改，你無需結束除錯過程而直接熱重啟 (hot restart) 你的應用：
不要點選 Stop 按鈕，只需點選 Run 按鈕（在執行中），或 Debug 按鈕（在除錯中），
或者按住 Shift 鍵點選熱重載按鈕。

## Editing Android code in Android Studio with full IDE support {#android-ide}

## 在 Android Studio 中編輯 Android 程式碼，並獲得完整 IDE 支援 {#android-ide}

Opening the root directory of a Flutter project doesn't expose all the Android
files to the IDE. Flutter apps contain a subdirectory named `android`. If you
open this subdirectory as its own separate project in Android Studio, the IDE
will be able to fully support editing and refactoring all Android files (like
Gradle scripts).

開啟 Flutter 專案的根目錄，並不會在 IDE 中顯示所有的 Android 檔案。
Flutter 應用包含了一個名為 `android` 的子目錄，
如果你在 Android Studio 中將該目錄作為單獨的專案開啟，
則 IDE 將可以完全支援編輯和重構所有的 Android 檔案（比如 Gradle 指令碼檔案）。

If you already have the entire project opened as a Flutter app in Android
Studio, there are two equivalent ways to open the Android files on their own
for editing in the IDE. Before trying this, make sure that you're on the latest
version of Android Studio and the Flutter plugins.

如果你已經在 Android Studio 中將整個專案作為 Flutter 應用開啟，
則有兩種方法可以開啟 Android 檔案，在 IDE 中進行編輯。
在進行操作之前，請確保你使用的是最新版本的 Android Studio 和 Flutter 外掛。

* In the ["project view"][], you should see a subdirectory immediately under
  the root of your flutter app named `android`. Right click on it,
  then select **Flutter > Open Android module in Android Studio**.

  在[“專案檢視 (project view)”]["project view"]中，
  你可以在 flutter 應用的根目錄下看到一個 `android` 的子目錄。
  右鍵點選它，選擇 **Flutter > Open Android module in Android Studio**。

* OR, you can open any of the files under the `android` subdirectory for
  editing. You should then see a "Flutter commands" banner at the top of the
  editor with a link  labeled **Open for Editing in Android Studio**.
  Click that link.

  或者，你也可以開啟 `android` 目錄下的任意檔案進行編輯。
  你會在編輯器的頂部看到一個 "Flutter commands" 的橫幅，
  包含一個 **Open for Editing in Android Studio** 的標籤，點選它。

For both options, Android Studio gives you the option to use separate windows or
to replace the existing window with the new project when opening a second
project. Either option is fine.

這兩種方法，Android Studio 都允許你選擇使用單獨的視窗，
或替換現有視窗開啟新專案，兩種都是可以的。

If you don't already have the Flutter project opened in Android studio,
you can open the Android files as their own project from the start:

如果你還沒在 Android Studio 中開啟 Flutter 專案，你可以一開始就將 Android 檔案作為專案開啟：

1. Click **Open an existing Android Studio Project** on the Welcome
   splash screen, or **File > Open** if Android Studio is already open.

   點選歡迎視窗中的 **Open an existing Android Studio Project**。
   如果 Android Studio 已開啟，也可以點選 **File > Open**。

2. Open the `android` subdirectory immediately under the flutter app root.
   For example if the project is called `flutter_app`,
   open `flutter_app/android`.

   開啟 flutter 應用根目錄下的 `android` 子目錄。
   例如，專案名為 `flutter_app`，則開啟 `flutter_app/android`。

If you haven't run your Flutter app yet, you might see Android Studio report a
build error when you open the `android` project. Run `flutter pub get` in
the app's root directory and rebuild the project by selecting **Build > Make**
to fix it.

如果你還未執行過你的 Flutter 應用，可能會在開啟 `android` 專案時，
看到 Android Studio 建構失敗的報告。執行專案根目錄的 `flutter pub get`，
並透過點選 **Build > Make** 重建專案，可修復該問題。

## Editing Android code in IntelliJ IDEA {#edit-android-code}

## 在 IntelliJ IDEA 中編輯 Android 程式碼 {#android-ide}

To enable editing of Android code in IntelliJ IDEA, you need to configure the
location of the Android SDK:

要在 IntelliJ IDEA 中編輯 Android 程式碼，你需要配置 Android SDK 的位置：

 1. In **Preferences > Plugins**, enable **Android Support** if you
    haven't already.

    在 **Preferences > Plugins** 中，啟用 **Android Support**。

 1. Right-click the **android** folder in the Project view, and select **Open
    Module Settings**.

    在專案檢視中，右鍵點選 **android** 資料夾，然後選擇 **Open
    Module Settings**。

 1. In the **Sources** tab, locate the **Language level** field, and
    select level 8 or later.

    在 **Sources** 選項中，找到 **Language level**，並選擇 level 8 或更高級別。

 1. In the **Dependencies** tab, locate the **Module SDK** field,
    and select an Android SDK. If no SDK is listed, click **New**
    and specify the location of the Android SDK.
    Make sure to select an Android SDK matching the one used by
    Flutter (as reported by `flutter doctor`).

    在 **Dependencies** 選項中，找到 **Module SDK**，並選擇一個 Android SDK。
    如果這裡沒有列出 SDK，點選 **New** 並指定 Android SDK 的位置。
    確保選擇和 Flutter 使用相匹配的 Android SDK（如 `flutter doctor` 中所示）。

 1. Click **OK**.

    點選 **OK**。

## Tips and tricks

## 提示和技巧

* [Flutter IDE cheat sheet, MacOS version][]

  [Flutter IDE 速查表，MacOS 版][Flutter IDE cheat sheet, MacOS version]

* [Flutter IDE cheat sheet, Windows & Linux version][]

  [Flutter IDE 速查表，Windows 和 Linux 版][Flutter IDE cheat sheet, Windows & Linux version]

## Troubleshooting

## 故障排除

### Known issues and feedback

### 已知問題和反饋

Important known issues that might impact your experience are documented
in the [Flutter plugin README][] file.

[Flutter 外掛 README][Flutter plugin README]
檔案中記錄了可能影響你使用體驗的已知重要問題。

All known bugs are tracked in the issue trackers:

所有已知問題都會在問題追蹤器中進行追蹤：

* Flutter plugin: [GitHub issue tracker][]

  Flutter 外掛： [GitHub 問題追蹤][GitHub issue tracker]

* Dart plugin: [JetBrains YouTrack][]

  Dart 外掛: [JetBrains 問題追蹤][JetBrains YouTrack]

We welcome feedback, both on bugs/issues and feature requests.
Prior to filing new issues:

我們歡迎所有的錯誤、問題以及功能反饋。在提交新問題前：

* Do a quick search in the issue trackers to see if the issue is already
  tracked.

  在問題追蹤器總快速搜尋檢視問題是否已存在。

* Make sure you have [updated](#updating) to the most recent version of the
  plugin.

  確保你已經 [更新](#updating) 最新版本的外掛。

When filing new issues, include the output of [`flutter doctor`][].

當你在提交新的 issue 時，確保帶上運行了 [`flutter doctor`][] 命令之後的返回內容。

[DevTools]: {{site.url}}/tools/devtools
[GitHub issue tracker]: {{site.repo.flutter}}-intellij/issues
[JetBrains YouTrack]: https://youtrack.jetbrains.com/issues?q=%23dart%20%23Unresolved
[`flutter doctor`]: {{site.url}}/resources/bug-reports#provide-some-flutter-diagnostics
[Flutter IDE cheat sheet, MacOS version]: {{site.url}}/resources/Flutter-IntelliJ-cheat-sheet-MacOS.pdf
[Flutter IDE cheat sheet, Windows & Linux version]: {{site.url}}/resources/Flutter-IntelliJ-cheat-sheet-WindowsLinux.pdf
[Debugging Flutter apps]: {{site.url}}/testing/debugging
[Flutter plugin README]: {{site.repo.flutter}}-intellij/blob/master/README.md
["project view"]: {{site.android-dev}}/studio/projects/#ProjectView
[let us know]: {{site.repo.this}}/issues/new
[Running DevTools from Android Studio]: {{site.url}}/tools/devtools/android-studio
[Hot reload]: {{site.url}}/tools/hot-reload
[Timeline view]: {{site.url}}/tools/devtools/performance
