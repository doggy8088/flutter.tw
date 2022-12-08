---
title: Flutter Fix
description: Keep your code up to date with the help of the Flutter Fix feature.
description: 使用 Flutter Fix 幫助您的應用跟進最新的 API 用法。
---

As Flutter continues to evolve, we provide a tool to help you clean up
deprecated APIs from your codebase. The tool ships as part of Flutter, and
suggests changes that you might want to make to your code. The tool is available
from the command line, and is also integrated into the IDE plugins for Android
Studio and Visual Studio Code. 

隨著 Flutter 的不斷髮展，我們提供了一個工具以幫助從程式碼庫中清理已棄用的 API。
該工具作為 Flutter 產品的一部分被提供，也會向你建議可能希望對程式碼進行的更改。
該工具可透過命令列進行呼叫，
也整合到了 Android Studio 和 Visual Studio Code 的 IDE 外掛中。

{{site.alert.tip}}

  These automated updates are called _quick-fixes_ in IntelliJ and Android
  Studio, and _code actions_ in VS Code).

  這種自動更新的功能在 IntelliJ、Android Studio 和 Eclipse 中被稱為 **quick-fixes**，
  在 VS Code 中被稱為 **code actions**。

{{site.alert.end}}

## Applying individual fixes

## 應用單個修復

You can use any supported IDE
to apply a single fix at a time.

你可以使用支援此功能的 IDE 逐個應用修復。

### IntelliJ and Android Studio

### IntelliJ 和 Android Studio

When the analyzer detects a deprecated API,
a light bulb appears on that line of code.
Clicking the light bulb displays the suggested fix
that updates that code to the new API.
Clicking the suggested fix performs the update.

當 analyzer 檢測到已棄用的 API 時，該行程式碼上會出現一個燈泡狀的圖示。
點選燈泡圖示會顯示將程式碼更新為新 API 的修復建議。
點選建議的修復會執行 API 更新操作。

![Screenshot showing suggested change in IntelliJ]({{site.url}}/assets/images/docs/development/tools/flutter-fix-suggestion-intellij.png)<br>
A sample quick-fix in IntelliJ

在 IntelliJ 中使用 quick-fix 的一個案例。

### VS Code

When the analyzer detects a deprecated API,
it presents an error.
You can do any of the following:

當 analyzer 檢測到已棄用的 API 時，
它會提供一個報錯資訊。
你可以執行以下任一操作：

* Hover over the error and then click the
  **Quick Fix** link.
  This presents a filtered list showing
  _only_ fixes.

  將滑鼠懸停在報錯的位置處，然後點選 **Quick Fix** 選項。
  此操作將只顯示修復程式碼的選項。

* Put the caret in the code with the error and click
  the light bulb icon that appears.
  This shows a list of all actions, including
  refactors.

  將游標放在出現錯誤提示的程式碼中，然後點擊出現的燈泡圖示。
  此操作會顯示包括重構在內所有可執行操作的列表。

* Put the caret in the code with the error and
  press the shortcut
  (**Command+.** on Mac, **Control+.** elsewhere)
  This shows a list of all actions, including
  refactors.

  將游標放在出現錯誤提示的程式碼中，然後按快捷鍵
  （mac 上是 **Command+.**，其他平台是 **Control+.** ）。
  此操作會顯示包括重構在內所有可執行操作的列表。

![Screenshot showing suggested change in VS Code]({{site.url}}/assets/images/docs/development/tools/flutter-fix-suggestion-vscode.png)<br>
A sample code action in VS Code

在 VS Code 中使用 code action 的一個案例。

## Applying project-wide fixes

## 對整個工程應用修復

To see or apply changes to an entire project,
you can use the command-line tool, [`dart fix`][].

你可以使用命令列工具 [`dart fix`][] 來檢視或應用整個專案的更改。

This tool has two options:

此工具有兩個可用選項：

* To see a full list of available changes, run
  the following command:

  若要檢視可用更改的完整列表，請執行以下命令:

  ```terminal
  dart fix --dry-run
  ```

* To apply all changes in bulk, run the
  following command:

  若要批次應用所有更改，請執行以下命令:

  ```terminal
  dart fix --apply
  ```

For more information on Flutter deprecations, see
[Deprecation lifetime in Flutter][], a free article
on Flutter's Medium publication.

更多有關 Flutter 廢棄 API 的詳細資訊，請檢視 Medium 上的
[Flutter 廢棄 API 的週期][Deprecation lifetime in Flutter] 文章。


[Deprecation lifetime in Flutter]: {{site.flutter-medium}}/deprecation-lifetime-in-flutter-e4d76ee738ad
[`dart fix`]: {{site.dart-site}}/tools/dart-fix
