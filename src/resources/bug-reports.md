---
title: Create useful bug reports
title: 如何有效提出 Bug
description: >
  Where to file bug reports and enhancement requests for 
  flutter and the website.
description: 在哪裡提出 bug 最有效呢？
tags: Flutter參考資料
keywords: 提出錯誤,Flutter除錯資訊,Flutter反饋指南,Flutter doctor
---

The instructions in this document detail the current steps
required to provide the most actionable bug reports for
crashes and other bad behavior. Each step is optional but
will greatly improve how quickly issues are diagnosed and addressed.
We appreciate your effort in sending us as much feedback as possible.

本文透過每個詳細的步驟，介紹瞭如何提供最具可操作性的 bug 報告，以更好地解決崩潰和其他不良行為。
雖然每個步驟都是可選的，但它們能夠顯著提高診斷及解決問題的速度。
我們希望你能夠儘可能多地反饋錯誤報告，我們由衷地感謝你對此做出的努力。

## Create an issue on GitHub

## 在 Github 上提出 issue

* To report a Flutter crash or bug,
  [create an issue in the flutter/flutter project][Flutter issue].

  報告 Flutter 的崩潰或 bug，
  [請在 flutter/flutter 專案中提出 issue][Flutter issue]。

* To report a problem with the website,
  [create an issue in the flutter/website project][Website issue].

  報告文件網站的問題，
  [請在 flutter/website 專案中提出 issue][Website issue]。

## Provide a minimal reproducible code sample

## 提供最小可復現的程式碼樣本

Create a minimal Flutter app that shows the problem you are facing,
and paste it into the GitHub issue.

建立一個最小可執行的 Flutter 應用程式，復現你所面臨的問題，
並且將這些程式碼貼上到 GitHub issue 中。

To create it you can use `flutter create bug` command and update
the `main.dart` file.

為了更好地除錯完成最小可復現的程式碼，
我們建議你可以使用 `flutter create bug` 命令建立新專案，
並在 `main.dart` 檔案中復現問題。

Alternatively, you can use [DartPad][], which is capable
of creating and running small Flutter apps.

另外，你還可以使用 [DartPad][]，
它能夠建立和執行小型 Flutter 應用程式。

If your problem goes out of what can be placed in a single file, for example
you have a problem with native channels, you can upload the full code of
the reproduction into a separate repository and link it.

如果你復現的問題由多個程式碼檔案組成，
例如，你有一個關於本地通道的問題，
你可以把這些程式碼檔案完整上傳到網路，
並在 issue 中附上鍊接地址。

## Provide some Flutter diagnostics

## 提供一些 Flutter 的診斷

* Run `flutter doctor -v` in your project directory and paste
  the results into the GitHub issue:

  在你的專案目錄中執行 `flutter doctor -v`，
  並將結果貼上到 GitHub issue 中：

```none
[✓] Flutter (Channel stable, 1.22.3, on Mac OS X 10.15.7 19H2, locale en-US)
    • Flutter version 1.22.3 at /Users/me/projects/flutter
    • Framework revision 8874f21e79 (5 days ago), 2020-10-29 14:14:35 -0700
    • Engine revision a1440ca392
    • Dart version 2.10.3

[✓] Android toolchain - develop for Android devices (Android SDK version 29.0.2)
    • Android SDK at /Users/me/Library/Android/sdk
    • Platform android-30, build-tools 29.0.2
    • Java binary at: /Applications/Android Studio.app/Contents/jre/jdk/Contents/Home/bin/java
    • Java version OpenJDK Runtime Environment (build 1.8.0_242-release-1644-b3-6222593)
    • All Android licenses accepted.

[✓] Xcode - develop for iOS and macOS (Xcode 12.2)
    • Xcode at /Applications/Xcode.app/Contents/Developer
    • Xcode 12.2, Build version 12B5035g
    • CocoaPods version 1.9.3

[✓] Android Studio (version 4.0)
    • Android Studio at /Applications/Android Studio.app/Contents
    • Flutter plugin version 50.0.1
    • Dart plugin version 193.7547
    • Java version OpenJDK Runtime Environment (build 1.8.0_242-release-1644-b3-6222593)

[✓] VS Code (version 1.50.1)
    • VS Code at /Applications/Visual Studio Code.app/Contents
    • Flutter extension version 3.13.2

[✓] Connected device (1 available)
    • iPhone (mobile) • 00000000-0000000000000000 • ios • iOS 14.0
```

## Run the command in verbose mode

## 在詳細模式 (verbose mode) 下執行命令

Follow these steps only if your issue is related to the
`flutter` tool.

當你遇到的問題與 `flutter` 工具相關時，
你才需要遵循以下步驟。

* All Flutter commands accept the `--verbose` flag.
  If attached to the issue, the output from this command
  might aid in diagnosing the problem.

  所有 Flutter 命令都接受 `--verbose` 標誌。
  如果將這個命令的輸出附在 issue 上，可能有助於診斷問題。

* Attach the results of the command to the GitHub issue.

  將該命令的結果附在 GitHub issue 上。

![flutter verbose]({{site.url}}/assets/images/docs/verbose_flag.png){:width="100%"}

## Provide the most recent logs

## 提供最新的日誌

* Logs for the currently connected device are accessed
  using `flutter logs`.

  使用 `flutter logs` 命令訪問當前連線裝置的日誌。

* If the crash is reproducible, clear the logs
  (⌘ + k on Mac), reproduce the crash and copy the
  newly generated logs into a file attached to the bug report.

  如果崩潰的情況是可以復現的，
  請你先清除當前的日誌（Mac 上的 ⌘ + k），
  然後復現崩潰的情況，
  最後將新產生的日誌複製到 bug 報告中。

* If you are getting exceptions thrown by the framework,
  include all the output between and including the dashed
  lines of the first such exception.

  如果你得到了由底層框架丟擲的例外，
  請你提供從第一個例外開始的所有輸出內容（包括虛線之間的內容）。

![flutter logs]({{site.url}}/assets/images/docs/logs.png){:width="100%"}

## Provide the crash report

## 提供崩潰報告

* When the iOS simulator crashes,
  a crash report is generated in `~/Library/Logs/DiagnosticReports/`.

  在 iOS 模擬器中崩潰時，
  會在 `~/Library/Logs/DiagnosticReports/` 中產生一份崩潰報告。

* When an iOS device crashes,
  a crash report is generated in `~/Library/Logs/CrashReporter/MobileDevice`.

  在 iOS 真機中崩潰時，
  會在 `~/Library/Logs/CrashReporter/MobileDevice` 中產生一份崩潰報告。

* Find the report corresponding to the crash (usually the latest)
  and attach it to the GitHub issue.

  找到與崩潰相對應的報告（通常是最新的）
  並將其附加到 GitHub issue 上。

![crash report]({{site.url}}/assets/images/docs/crash_reports.png){:width="100%"}


[DartPad]: {{site.dartpad}}
[Flutter issue]: {{site.repo.flutter}}/issues/new/choose
[Website issue]: {{site.repo.this}}/issues/new/choose
