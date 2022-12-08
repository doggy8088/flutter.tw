---
title: Visual Studio Code
title: 在 VS Code 裡開發 Flutter 應用
short-title: VS Code
description: How to develop Flutter apps in Visual Studio Code.
description: 如何在 VS Code 裡開發 Flutter 應用。
tags: SDK,Flutter SDK,開發工具
keywords: VS Code,IDE,配置,安裝
---

<ul class="nav nav-tabs" id="ide" role="tablist">
  <li class="nav-item">
    <a class="nav-link" href="{{site.url}}/development/tools/android-studio" role="tab" aria-selected="false">Android Studio 和 IntelliJ</a>
  </li>
  <li class="nav-item">
    <a class="nav-link active" role="tab" aria-selected="true">VS Code</a>
  </li>
</ul>

## Installation and setup

## 安裝和配置

Follow the [Set up an editor][] instructions to
install the Dart and Flutter extensions
(also called plugins).

根據 [編輯工具設定][Set up an editor]
的指引來安裝 Dart 和 Flutter 擴充（也叫做外掛）。

### Updating the extension {#updating}

### 更新擴充程式

Updates to the extensions are shipped on a regular basis.
By default, VS Code automatically updates extensions when
updates are available.

擴充的更新會定期釋出。
預設情況下，當有可用的更新時 VS Code 會自動更新擴充。

To install updates manually:

手動安裝更新：

 1. Click the **Extensions** button in the Side Bar.

    點選側邊欄的 **Extensions** 按鈕。

 1. If the Flutter extension is shown with an available update,
    click the update button and then the reload button.
    
    如果 Flutter 擴充顯示有可用更新，點選更新按鈕，然後重載。

 1. Restart VS Code.

    重啟 VS Code。

## Creating projects

## 建立專案

There are a couple ways to create a new project.

有幾種方式建立一個新專案。

### Creating a new project

### 新建專案

To create a new Flutter project from the Flutter
starter app template:

透過 Flutter 入門應用範本新建 Flutter 專案：

 1. Open the Command Palette
    (`Ctrl`+`Shift`+`P` (`Cmd`+`Shift`+`P` on macOS)).

    開啟命令面板（`Ctrl`+`Shift`+`P` （macOS 用 `Cmd`+`Shift`+`P`））。

 1. Select the **Flutter: New Project** command and press `Enter`.

    選擇 **Flutter: New Project** 命令然後按 `Enter`。

 1. Select **Application** and press `Enter`.

    選擇 **Application** 然後按 `Enter`。

 1. Select a **Project location**.

    選擇 **專案地址**。

 1. Enter your desired **Project name**.

    輸入你想要的 **專案名**。

### Opening a project from existing source code

### 從現有原始碼開啟專案

To open an existing Flutter project:

開啟現有 Flutter 專案：

 1. Click **File > Open** from the main IDE window.
 
    在 IDE 主視窗點選 **File > Open**。
 
 1. Browse to the directory holding your existing
    Flutter source code files.
 
    選擇存放現有 Flutter 原始碼檔案的目錄。
  
 1. Click **Open**.
 
    點選 **Open**。

## Editing code and viewing issues

## 編寫程式碼及檢視問題

The Flutter extension performs code analysis that
enables the following:

Flutter 擴充執行程式碼分析，它提供：

* Syntax highlighting

  語法高亮。
  
* Code completions based on rich type analysis

  基於豐富輸入分析的程式碼自動完成。
  
* Navigating to type declarations
  (**Go to Definition** or `F12`),
  and finding type usages
  (**Find All References** or `Shift`+`F12`)
  
  導航到型別宣告（**Go to Definition** 或 `F12`）
  和查詢型別參考（**Find All References** 或 `Shift`+`F12`）。
  
* Viewing all current source code problems
  (**View > Problems** or `Ctrl`+`Shift`+`M`
  (`Cmd`+`Shift`+`M` on macOS))
  Any analysis issues are shown in the Problems pane:<br>
  ![Problems pane]({{site.url}}/assets/images/docs/tools/vs-code/problems.png){:.mw-100.pt-1}
  
  檢視所有當前程式碼問題（**View > Problems** 或 `Ctrl`+`Shift`+`M` (macOS 用 `Cmd`+`Shift`+`M`)）。
  所有問題分析都會在 Problems 面板展示：<br>
  ![Problems pane]({{site.url}}/assets/images/docs/tools/vs-code/problems.png){:.mw-100.pt-1}

## Running and debugging

## 執行和除錯

{{site.alert.note}}

  You can debug your app in a couple of ways.

  有多種方式能夠除錯你的應用

  * Using [DevTools][], a suite of debugging and profiling
    tools that run in a browser. DevTools replaces the previous
    browser-based profiling tool, Observatory, and includes
    functionality previously only available to Android Studio
    and IntelliJ, such as the Flutter inspector.

    使用 [DevTools][]，它是一個執行在瀏覽器中的
    除錯以及效能測試工具集。DevTools 取代了前一代基於瀏覽器的效能測試工具 Observatory，
    它包含了以前僅適用於 Android Studio 和 IntelliJ 的功能，例如 Flutter inspector。

  * Using VS Code's built-in debugging features,
    such as setting breakpoints.

    使用 VS Code 的內建除錯功能，例如設定斷點。

  The instructions below describe features available in VS Code.
  For information on using launching DevTools, see
  [Running DevTools from VS Code][] in the [DevTools][] docs.

  以下說明描述了 VS Code 可用的功能。更多使用 DevTools 的詳細資訊請參考
  [DevTools][] 中的 [Running DevTools from VS Code][] 文件。
  
{{site.alert.end}}

Start debugging by clicking **Run > Start Debugging**
from the main IDE window, or press `F5`.

在 IDE 主視窗點選 **Run > Start Debugging** 或按 `F5` 開啟除錯。

### Selecting a target device

### 選擇目標裝置

When a Flutter project is open in VS Code,
you should see a set of Flutter specific entries in the status bar,
including a Flutter SDK version and a
device name (or the message **No Devices**):<br>

當一個 Flutter 專案在 VS Code 中開啟，
你會在狀態列看到一些 Flutter 特有項，
包括 Flutter SDK 版本和裝置名稱（或者**無裝置**資訊）：<br>

![VS Code status bar][]{:.mw-100.pt-1}

{{site.alert.note}}

  * If you do not see a Flutter version number or device info,
    your project might not have been detected as a Flutter project.
    Ensure that the folder that contains your `pubspec.yaml` is
    inside a VS Code **Workspace Folder**.

    如果你沒看到 Flutter 版本號或者裝置資訊，
    你的專案可能不被識別為一個 Flutter 專案。
    請確認 VS Code **Workspace Folder** 的目錄中是否含有 `pubspec.yaml`。

  * If the status bar reads **No Devices**, Flutter has not been
    able to discover any connected iOS or Android devices or simulators.
    You need to connect a device, or start a simulator or emulator,
    to proceed.

    如果狀態列顯示 **無裝置** 表明 Flutter 
    沒有發現任何已連線的 IOS、Android 或者模擬器。
    你需要連線裝置或者啟動模擬器。

{{site.alert.end}}

The Flutter extension automatically selects the last device connected.
However, if you have multiple devices/simulators connected, click
**device** in the status bar to see a pick-list
at the top of the screen. Select the device you want to use for
running or debugging.

Flutter 擴充會自動選擇上次連線的裝置。
然而，如果你有多個裝置/模擬器連線，
點選狀態列的 **device** 檢視螢幕頂部的選擇列表。
選擇你要用來執行或除錯的裝置。

{{site.alert.secondary}}

  **Are you developing for macOS or iOS remotely using
  Visual Studio Code Remote?** If so, you might need to manually
  unlock the keychain. For more information, see this
  [question on StackExchange][].

  **你是否使用 VSCode Remote 遠端開發 macOS 或 iOS 應用？**
  如果是，你可能需要手動解鎖 iOS 金鑰。
  更多資訊請檢視 [StackExchange 上的問題解答][question on StackExchange]。

[question on StackExchange]: https://superuser.com/questions/270095/when-i-ssh-into-os-x-i-dont-have-my-keychain-when-i-use-terminal-i-do/363840#363840

{{site.alert.end}}

### Run app without breakpoints

### 無斷點執行

 1. Click **Run > Start Without Debugging** in the
    main IDE window, or press `Ctrl`+`F5`.
    The status bar turns orange to show you are in a debug session.<br>
    ![Debug console]({{site.url}}/assets/images/docs/tools/vs-code/debug_console.png){:.mw-100.pt-1}

    在 IDE 主視窗點選 **Run > Start Without Debugging**，
    或者按 `Ctrl`+`F5`，狀態列變橙色說明你正處於除錯模式。<br>
    ![Debug console]({{site.url}}/assets/images/docs/tools/vs-code/debug_console.png){:.mw-100.pt-1}
    
### Run app with breakpoints

### 斷點執行

 1. If desired, set breakpoints in your source code.

    如果需要，在原始碼中設定斷點。

 1. Click **Run > Start Debugging** in the main IDE window,
    or press `F5`.

    在 IDE 主視窗點選 **Run > Start Debugging** 或按 `F5`。

    * The left **Debug Sidebar** shows stack frames and variables.

      左側的**除錯側邊欄**顯示堆疊幀和變數。

    * The bottom **Debug Console** pane shows detailed logging output.

      底部的**除錯控制檯**面板顯示輸出的日誌詳情。

    * Debugging is based on a default launch configuration.
      To customize, click the cog at the top of the
      **Debug Sidebar** to create a `launch.json` file.
      You can then modify the values.

      除錯基於預設的配置。
      也可以透過點選**除錯側邊欄**頂部的齒輪建立 `launch.json` 檔案自訂除錯。
      你可以修改裡面的值。

### Run app in debug, profile, or release mode

### 以 除錯 (debug)、效能 (profile) 或釋出 (release) 模式執行應用

Flutter offers many different build modes to run your app in. 
You can read more about them in [Flutter's build modes][].

Flutter 提供了很多種不同的建構模式執行你的應用，
更多內容請參考文件 [Flutter 的建構模式][Flutter's build modes]。

 1. Open the `launch.json` file in VS Code.
    
    開啟 VS Code 裡的 `launch.json` 檔案

    If you do not have a `launch.json` file, go to 
    the **Run** view in VS Code and click **create a launch.json file**.
    
    如果你沒有 `launch.json` 檔案，請到 VS Code 的 **Run** 檢視，
    點選 **create a launch.json file** 建立。
    
 1. In the `configurations` section, change the `flutterMode` property to 
 the build mode you want to target. 

    在 `configurations` 部分，修改 `flutterMode` 屬性值為你想要的建構模式即可。
    
     * For example, if you want to run in debug mode, 
     your `launch.json` might look like this: 
     
       舉個例子，如果你希望在除錯模式下執行，
       你的 `launch.json` 檔案應該類似下面這樣：
       
     ```json
      "configurations": [
       {
         "name": "Flutter",
         "request": "launch",
         "type": "dart",
         "flutterMode": "debug"
       }
     ]
     ```
 1. Run the app through the **Run** view. 
 
    在 **Run** 視圖裡執行你的應用。


## Fast edit and refresh development cycle

## 快速編輯和重新整理開發週期

Flutter offers a best-in-class developer cycle enabling you
to see the effect of your changes almost instantly with the
_Stateful Hot Reload_ feature. See
[Using hot reload](hot-reload) for details.

Flutter 提供一流的開發週期，透過 **Stateful Hot Reload** 
特性使你在幾乎修改程式碼的同時就能看到變化。
詳情請看 [使用熱重載](hot-reload)。

## Advanced debugging

## 進階除錯

You might find the following advanced debugging tips useful:

以下的除錯指南可能會對你有幫助：

### Debugging visual layout issues

### 視覺化佈局問題除錯

During a debug session,
several additional debugging commands are added to the
[Command Palette][] and to the [Flutter inspector][].
When space is limited, the icon is used as the visual
version of the label.

在除錯會話期間，[命令面板][Command Palette] 和 [Flutter inspector][]
會新增一些額外的除錯命令，包括：

<dl markdown="1">
<dt markdown="1"><t><b>Toggle Baseline Painting</b></t><t><b>切換 Baseline 繪製</b></t> ![Baseline painting icon]({{site.url}}/assets/images/docs/tools/devtools/paint-baselines-icon.png){:width="20px"}</dt>
<dd><p>Causes each RenderBox to paint a line at each of its baselines.</p><p>每個 RenderBox 在底部繪製一條線。</p></dd>
<dt markdown="1"><t><b>Toggle Repaint Rainbow</b></t><t><b>切換重繪 Rainbow</b></t> ![Repaint rainbow icon]({{site.url}}/assets/images/docs/tools/devtools/repaint-rainbow-icon.png){:width="20px"}</dt>
<dd><p>Shows rotating colors on layers when repainting.</p><p>重新繪製時在圖層上改變顏色。</p></dd>
<dt markdown="1"><t><b>Toggle Slow Animations</b></t><t><b>切換慢模式橫幅</b></t> ![Slow animations icon]({{site.url}}/assets/images/docs/tools/devtools/slow-animations-icon.png){:width="20px"}</dt>
<dd><t>Slows down animations to enable visual inspection.</t><t>減慢動畫以啟用視覺檢查。</t>
</dd>
<dt markdown="1"><t><b>Toggle Debug Mode Banner</b></t> <t><b>切換 debug 模式橫幅顯示</b></t> ![Debug mode banner icon]({{site.url}}/assets/images/docs/tools/devtools/debug-mode-banner-icon.png){:width="20px"}</dt>
<dd><p>Hides the debug mode banner even when running a debug build.</p><p>在執行除錯建構時隱藏 debug 模式的橫幅 (banner)。</p></dd>
</dl>

### Debugging external libraries

### 除錯外部庫

By default, debugging an external library is disabled
in the Flutter extension. To enable:

預設情況下，Flutter 擴充禁止除錯外部庫。啟用步驟：

1. Select **Settings > Extensions > Dart Configuration**.
   
   選擇 **Settings > Extensions > Dart Configuration**

2. Check the `Debug External Libraries` option.

   勾選 `Debug External Libraries` 選項。

## Editing tips for Flutter code

## Flutter 程式碼編輯提示

If you have additional tips we should share, [let us know][]!

如果你有其他我們應該提供的程式碼提示建議，請 [告訴我們][let us know]！

### Assists & quick fixes

### 程式碼輔助和快速修復

Assists are code changes related to a certain code identifier.
A number of these are available when the cursor is placed on a
Flutter widget identifier, as indicated by the yellow lightbulb icon.
The assist can be invoked by clicking the lightbulb, or by using the
keyboard shortcut `Ctrl`+`.` (`Cmd`+`.` on Mac), as illustrated here:

程式碼輔助功能是特定程式碼識別符號相關的程式碼修改。
當游標放在 Flutter widget 上時，黃色燈泡圖示會指示可用的修改，
可以透過點選燈泡進行修改，
或者使用快捷鍵 `Ctrl`+`.` (macOS 用 `Cmd`+`.`)，
如圖所示：

![Code assists]({{site.url}}/assets/images/docs/tools/vs-code/assists.png){:width="467px"}

Quick fixes are similar,
only they are shown with a piece of code has an error and they
can assist in correcting it.

快速修復跟輔助類似，當一段程式碼有錯誤並且可以輔助修正時才會顯示。

**Wrap with new widget assist**

**Widget 巢狀(Nesting)輔助**

  This can be used when you have a widget that you want to wrap
  in a surrounding widget, for example if you want to wrap a
  widget in a `Row` or `Column`.

  當你有個 widget 想包裝進一個容器 widget 時，
  例如你想把 widget 放入一個 `Row` 或者 `Column`。

**Wrap widget list with new widget assist**

**Widget 列表巢狀(Nesting)輔助**

  Similar to the assist above, but for wrapping an existing
  list of widgets rather than an individual widget.

  和上面的輔助類似，但它巢狀(Nesting)的是一個 widget 的列表，
  而不是單個的 widget。

**Convert child to children assist**

**child 和 children 轉換輔助**

  Changes a child argument to a children argument,
  and wraps the argument value in a list.

  將 child 轉換成 children，並且把引數值寫進一個 list。

**Convert StatelessWidget to StatefulWidget assist**
<br> Changes the implementation of a `StatelessWidget` to that of
  a `StatefulWidget`, by creating the `State` class and moving
  the code there.

**StatelessWidget 到 StatefulWidget 的轉換**
<br> 建立 `State` 類並將程式碼移動過去，
  可以將 `StatelessWidget` 的實現更改為 `StatefulWidget`。

### Snippets

### 程式碼片段

Snippets can be used to speed up entering typical code structures.
They are invoked by typing their prefix,
and then selecting from the code completion window:

程式碼片段可以用來加速輸入通用型別程式碼段。
透過輸入字首來呼叫，然後從程式碼完成視窗中選擇：

![Snippets]({{site.url}}/assets/images/docs/tools/vs-code/snippets.png){:width="100%"}

The Flutter extension includes the following snippets:

Flutter 擴充包含以下片段：

* Prefix `stless`: Create a new subclass of `StatelessWidget`.

  字首 `stless`：建立一個 `StatelessWidget` 子類別。
  
* Prefix `stful`: Create a new subclass of `StatefulWidget`
  and its associated State subclass.
  
  字首 `stful`：建立一個 `StatefulWidget` 子類別，並且和 State 子類別關聯。
  
* Prefix `stanim`: Create a new subclass of `StatefulWidget`,
  and its associated State subclass including a field initialized
  with an `AnimationController`.
  
  字首 `stanim`：建立一個 `StatefulWidget` 子類別，並且把包含欄位初始化的 State 子類別和一個 `AnimationController` 關聯。

You can also define custom snippets by executing
**Configure User Snippets** from the [Command Palette][].

你也可以透過在 [命令面板][Command Palette]
執行 **Configure User Snippets** 來自訂片段。

### Keyboard shortcuts

### 鍵盤快捷鍵

**Hot reload**
<br> During a debug session, clicking the **Hot Reload** button on the
  **Debug Toolbar**, or pressing `Ctrl`+`F5`
  (`Cmd`+`F5` on macOS) performs a hot reload.

**熱重載**
<br> 除錯期間，在 **除錯工具欄** 點選 **熱重載 (Hot Reload)** 按鈕，
  或者按 `Ctrl`+`F5`（macOS 用 `Cmd`+`F5`）執行熱重載。

  Keyboard mappings can be changed by executing the
  **Open Keyboard Shortcuts** command from the [Command Palette][].

  鍵位繫結可以在 [命令面板][Command Palette] 中使用
  **Open Keyboard Shortcuts** 命令進行調整。

### Hot reload vs. hot restart

### 熱重載和熱重啟

Hot reload works by injecting updated source code files into the
running Dart VM (Virtual Machine). This includes not only
adding new classes, but also adding methods and fields to
existing classes, and changing existing functions.
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

For these changes, fully restart your application without
having to end your debugging session. To perform a hot restart,
run the **Flutter: Hot Restart** command from the [Command Palette][],
or press `Ctrl`+`Shift`+`F5`(`Cmd`+`Shift`+`F5` on macOS).

對於這些更改，你無需結束除錯過程而直接熱重啟 (hot restart) 你的應用。
要執行熱重啟，執行 [命令面板][Command Palette] 的 **Flutter：熱重啟**命令，
或者按 ``Ctrl`+`Shift`+`F5 (在 macOS 上使用 `Cmd`+`Shift`+`F5`)。

## Troubleshooting

## 故障排除

### Known issues and feedback

### 已知問題和反饋

All known bugs are tracked in the issue tracker:
[Dart and Flutter extensions GitHub issue tracker][issue tracker].

所有已知 bug 在這個 issue 列表中記錄：
[Dart 和 Flutter 擴充 GitHub issue 追蹤][issue tracker]。

We welcome feedback,
both on bugs/issues and feature requests.
Prior to filing new issues:

我們非常歡迎 bugs/issues 和特性請求的反饋。在提交新 issue 之前：

* Do a quick search in the issue trackers to see if the
  issue is already tracked.
  
  在 issue 列表中查詢看該問題是否已被記錄。
  
* Make sure you are [up to date](#updating) with the most recent
  version of the plugin.

  確保你已經 [更新](#updating) 最新版本外掛。

When filing new issues, include [flutter doctor][] output.

提交新 issue 時，請包含 [flutter doctor][] 輸出。

[Command Palette]: https://code.visualstudio.com/docs/getstarted/userinterface#_command-palette
[DevTools]: {{site.url}}/development/tools/devtools
[flutter doctor]: {{site.url}}/resources/bug-reports/#provide-some-flutter-diagnostics
[Flutter inspector]: {{site.url}}/development/tools/devtools/inspector
[Flutter's build modes]: {{site.url}}/testing/build-modes
[let us know]: {{site.repo.this}}/issues/new
[issue tracker]: {{site.github}}/Dart-Code/Dart-Code/issues
[Running DevTools from VS Code]: {{site.url}}/development/tools/devtools/vscode
[Set up an editor]: {{site.url}}/get-started/editor?tab=vscode
[VS Code status bar]: {{site.url}}/assets/images/docs/tools/vs-code/device_status_bar.png
