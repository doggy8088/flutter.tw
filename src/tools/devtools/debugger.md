---
title: Using the debugger
title: 使用偵錯程式工具
description: How to use DevTools' source-level debugger.
description: 學習如何使用開發者工具裡的偵錯程式。
tags: Flutter開發工具,DevTools
keywords: 除錯,debug,設定斷點,單步除錯
---

{{site.alert.note}}

  DevTools hides the Debugger tab if the app was launched
  from VS Code because VS Code has a built-in debugger.

  如果應用是從 VS Code 啟動的，開發工具會隱藏偵錯程式標籤頁，
  因為 VS Code 有內建的偵錯程式。
{{site.alert.end}}

## Getting started

## 開始使用

DevTools includes a full source-level debugger, supporting
breakpoints, stepping, and variable inspection.

開發工具中包含了一個完整的原始碼級偵錯程式，支援斷點、單步除錯以及變數檢視。

{{site.alert.note}}

  The debugger works with all Flutter and Dart applications.
  If you are looking for a way to use GDB to remotely debug the
  Flutter engine running within an Android app process,
  check out [`flutter_gdb`][].

  偵錯程式可以用於所有的 Flutter 和 Dart 應用。
  如果你想要使用 GDB 遠端除錯執行在 Android 應用處理序中的 Flutter 引擎，
  請檢視 [`flutter_gdb`][]。
{{site.alert.end}}

[`flutter_gdb`]: https://github.com/flutter/engine/blob/main/sky/tools/flutter_gdb

When you open the debugger tab, you should see the source for the main
entry-point for your app loaded in the debugger.

當你開啟偵錯程式標籤頁時，你應該會看到你的應用入口點的原始碼已經載入到了偵錯程式中。

In order to browse around more of your application sources, click **Libraries**
(top right) or press <kbd>Ctrl</kbd> / <kbd>Cmd</kbd> + <kbd>P</kbd>.
This opens the libraries window and allows you
to search for other source files.

為了瀏覽更多的應用原始碼，點選 **Libraries**（右上角）或者使用快捷鍵 `⌘ + P` / `ctrl + P`。
這會開啟庫視窗並允許你搜索其他原始檔。

![Screenshot of the debugger tab]({{site.url}}/assets/images/docs/tools/devtools/debugger_screenshot.png){:width="100%"}

## Setting breakpoints

## 設定斷點

To set a breakpoint, click the left margin (the line number ruler)
in the source area. Clicking once sets a breakpoint, which should
also show up in the **Breakpoints** area on the left. Clicking
again removes the breakpoint.

可以點選原始碼區左邊空白（行數展示欄內）來設定斷點。
單擊一次就設定了一個斷點，並且也會在 **Breakpoints** 區域展示出來。
再次單擊則取消斷點。

## The call stack and variable areas

## 呼叫棧和變數區

When your application encounters a breakpoint, it pauses there,
and the DevTools debugger shows the paused execution location
in the source area. In addition, the `Call stack` and `Variables`
areas populate with the current call stack for the paused isolate,
and the local variables for the selected frame. Selecting other
frames in the `Call stack` area changes the contents of the variables.

當應用執行到某個斷點時，就會在此處暫停，偵錯程式也會在原始碼區顯示當前暫停的位置。
此外，`Call stack` 和 `Variables` 區域也會顯示暫停時的呼叫棧以及選中幀的本地變數。
在 `Call stack` 選擇其他的幀可以改變變數區的內容。

Within the `Variables` area, you can inspect individual objects by
toggling them open to see their fields. Hovering over an object
in the `Variables` area calls `toString()` for that object and
displays the result.

在 `Variables` 內，可以透過點選物件展開檢視其內容來檢視獨立的物件。
指標停在 `Variables` 區域的物件上時會呼叫該物件的 `toString()` 方法並展示結果。

## Stepping through source code

## 單步除錯原始碼

When paused, the three stepping buttons become active.

三個單步除錯按鈕在暫停後會變為可用狀態。

* Use **Step in** to step into a method invocation, stopping at
  the first executable line in that invoked method.
  
  使用 **Step in** 來進入被呼叫的方法，在遇到方法內的第一行可執行程式碼時結束。

* Use **Step over** to step over a method invocation;
  this steps through source lines in the current method.
  
  使用 **Step over** 直接執行某個方法呼叫而不進入內部；該按鈕在當前方法內按行執行。
  
* Use **Step out** to step out of the current method,
  without stopping at any intermediary lines.
  
  使用 **Step out** 來跳出當前方法，這種方式會直接執行完所有當前方法內的陳述式。

In addition, the **Resume** button continues regular
execution of the application.

另外，**Resume** 按鈕的作用是恢復應用的正常執行。

## Console output

## 命令列輸出

Console output for the running app (stdout and stderr) is 
displayed in the console, below the source code area.
You can also see the output in the [Logging view][].

執行中應用的命令列輸出（stdout 和 stderr）會在命令列中輸出，
該區域在原始碼區下方。[Logging view][] 中也可以看到相應輸出。

## Breaking on exceptions

## 例外跳出

To adjust the stop-on-exceptions behavior, toggle the
**Ignore** dropdown at the top of the debugger view.

請在偵錯程式檢視頂部切換 **Ignore** 下拉選單來
適配例外跳出的行為。

Breaking on unhandled excepts only pauses execution if the
breakpoint is considered uncaught by the application code.
Breaking on all exceptions causes the debugger to pause
whether or not the breakpoint was caught by application code.

Break on unhandled exceptions：只在斷點被認為應用內程式碼無法捕獲時暫停執行。
Breaking on all exceptions：無論是否被捕獲都會暫停執行。

## Known issues

## 已知問題

When performing a hot restart for a Flutter application,
user breakpoints are cleared.

當 Flutter 應用執行熱重載時，使用者的斷點會被清除。

[Logging view]: {{site.url}}/tools/devtools/logging

## Other resources

## 其他資源

For more information on debugging and profiling, see the
[Debugging][] page.

存取 [Debugging][] 頁面來獲取更多關於偵錯程式和效能分析的資訊。

[Debugging]: {{site.url}}/testing/debugging
