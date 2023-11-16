---
title: Code formatting
title: 程式碼格式化
description: >
    Flutter's code formatter formats your code
    following recommended style guidelines.
description: Flutter 的程式碼格式化工具以及推薦的程式碼格式。
tags: SDK,Flutter SDK
keywords: 程式碼格式化,dartfmt,程式碼審查,code review
---


While your code might follow any preferred style&mdash;in our
experience&mdash;teams of developers might find it more productive to:

每個人都有自己喜歡的程式碼樣式。但是根據我們的經驗，下面這些做法可以提高團隊的開發效率：

* Have a single, shared style, and

  團隊使用單一，共享的程式碼樣式。

* Enforce this style through automatic formatting.
  
  透過自動格式化程式碼來保證統一的程式碼樣式。

The alternative is often tiring formatting debates during code reviews,
where time might be better spent on code behavior rather than code style.

如果沒有統一的程式碼樣式，當進行程式碼審查的時候，
可能會為了一些樣式的問題而進行爭論從而浪費時間。
程式碼審查最好把時間花在程式碼的行為上，而不是程式碼的樣式上。

## Automatically formatting code in VS Code

## 在 VS Code 中自動格式化程式碼

Install the `Flutter` extension (see
[Editor setup]({{site.url}}/get-started/editor))
to get automatic formatting of code in VS Code.

在 VS Code 中安裝 `Flutter` 擴充
（見章節 [編輯工具設定]({{site.url}}/get-started/editor)）來進行程式碼的自動格式化。

To automatically format the code in the current source code window,
right-click in the code window and select `Format Document`.
You can add a keyboard shortcut to this VS Code **Preferences**.

格式化當前視窗中程式碼的方法是先在程式碼視窗中單擊右鍵，
然後選擇 `Format Document` 選項即可。
也可以在 VS Code 的偏好設定裡面增加快捷鍵，
然後使用快捷鍵操作。

To automatically format code whenever you save a file, set the
`editor.formatOnSave` setting to `true`.

將 `editor.formatOnSave` 設定成 `true`，
可以在儲存檔案的時候自動進行程式碼格式化。

## Automatically formatting code in Android Studio and IntelliJ

Install the `Dart` plugin (see
[Editor setup]({{site.url}}/get-started/editor))
to get automatic formatting of code in Android Studio and IntelliJ.
To format your code in the current source code window:

在 Android Studio / IntelliJ 中安裝 `Dart` 外掛
（見章節 [編輯工具設定]({{site.url}}/get-started/editor))
來進行程式碼的自動格式化。
在當前程式碼視窗中格式化程式碼的方法是：

* In macOS,
  press <kbd>Cmd</kbd> + <kbd>Option</kbd> + <kbd>L</kbd>.

  在 Mac 系統裡使用 <kbd>Cmd</kbd> + <kbd>Option</kbd> + <kbd>L</kbd>。

* In Windows and Linux,
  press <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>L</kbd>.

  在 Windows 和 Linux 系統裡使用
  <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>L</kbd>。

Android Studio and IntelliJ also provide a checkbox named
**Format code on save** on the Flutter page in **Preferences**
on macOS or **Settings** on Windows and Linux.
This option corrects formatting in the current file when you save it.

Android Studio 和 IntelliJ 為 Flutter 頁面提供了一個選項，
即“在儲存的時候格式化程式碼”—— `Format code on save`，
要開啟這個，可以在 Windows 和 Linux 下選擇設定、在 macOS 下選擇偏好設定。
這樣在每次儲存程式碼的時候就會自動格式化當前檔案。

## Automatically formatting code with the `dart` command

## 使用 `dart` 命令自動格式化程式碼

To correct code formatting in the command line interface (CLI),
run the `dart format` command:

我們也可以在命令列介面（CLI）中使用 `dart format` 命令，
進行程式碼的自動格式化。

```terminal
$ dart format path1 path2 [...]
```

## Using trailing commas

## 末尾處新增逗號

Flutter code often involves building fairly deep tree-shaped data structures,
for example in a `build` method. To get good automatic formatting,
we recommend you adopt the optional *trailing commas*.
The guideline for adding a trailing comma is simple: Always
add a trailing comma at the end of a parameter list in
functions, methods, and constructors where you care about
keeping the formatting you crafted.
This helps the automatic formatter to insert an appropriate
amount of line breaks for Flutter-style code.

Flutter 程式碼經常會建構一定深度的樹形資料結構，如在 `build` 方法中。
為了有更好的自動格式化效果，我們推薦在末尾處新增逗號，
儘管也可以不這樣做。
規則也比較簡單：總是在函式、普通方法、構造方法引數列表的末尾處新增逗號。
這樣做會使格式化工具自動插入一些換行符，
使程式碼更具有 Flutter 風格。

Here is an example of automatically formatted code *with* trailing commas:

自動格式化的時候，末尾處 **加入** 逗號的例子：

![Automatically formatted code with trailing commas]({{site.url}}/assets/images/docs/tools/android-studio/trailing-comma-with.png){:width="100%"}

![末尾處有逗號進行程式碼自動格式化的情況 (Automatically formatted code with trailing commas)]({{site.url}}/assets/images/docs/tools/android-studio/trailing-comma-with.png){:width="100%"}

And the same code automatically formatted code *without* trailing commas:

同樣的程式碼在進行自動格式化的時候，末尾處沒有逗號的例子：

![Automatically formatted code without trailing commas]({{site.url}}/assets/images/docs/tools/android-studio/trailing-comma-without.png){:width="100%"}

![末尾處沒有逗號進行程式碼自動格式化的情況 (Automatically formatted code without trailing commas)]({{site.url}}/assets/images/docs/tools/android-studio/trailing-comma-without.png){:width="100%"}
