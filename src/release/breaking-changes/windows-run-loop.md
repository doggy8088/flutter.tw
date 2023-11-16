---
title: Migrate a Windows project to the idiomatic run loop
title: 將工程遷移到更有利於 Windows 的事件迴圈系統
description: How to update a Windows project to use the idiomatic run loop
description: 學習如何將你的 Windows 工程升級為使用更好的 Windows 事件迴圈策略
---

Flutter 2.5 replaced Windows apps' run loop with an idiomatic
Windows message pump to reduce CPU usage.

Flutter 2.5 開始使用更有利於 Windows 的事件迴圈系統來降低 CPU 使用率。

Projects created before Flutter version 2.5 need to be
migrated to get this improvement. You should follow the
migration steps below if the `windows/runner/run_loop.h`
file exists in your project.

在 Flutter 2.5 版本之前建立的專案需要進行遷移來獲得這種改進。
如果你的專案中存在 `windows/runner/run_loop.h` 檔案，
你應該遵循下面的遷移步驟。

## Migration steps

## 遷移步驟

{{site.alert.note}}

  As part of this migration, you must recreate your Windows project,
  which clobbers any custom changes to the
  files in the `windows/runner` folder.  The following steps
  include instructions for this scenario.

  作為遷移步驟之一，你必須重新建立你的 Windows 專案，
  這將會抹除掉 `windows/runner` 資料夾中所有檔案的自訂修改。
  下面的步驟將會對這種情況進行說明。

{{site.alert.end}}

Your project can be updated using these steps:

你的專案可以透過這些步驟進行遷移：

1. Verify you are on Flutter version 2.5 or newer using `flutter --version`

   使用 `flutter --version` 指令，確認你的 Flutter 是 2.5 或更高的版本；

2. If needed, use `flutter upgrade` to update to the latest version of the
   Flutter SDK

   如果可以的話，使用 `flutter upgrade` 指令，更新 Flutter SDK 到最新版本；

3. Backup your project with git (or your preferred version control system),
   since you need to reapply any local changes you've made (if any) to your
   project in a later step

   使用 git（或你習慣的版本控制系統）備份你的專案，
   因為你需要在後面的步驟中，
   重新應用之前在本地做的所有自訂修改（如果有的話）；

4. Delete all files under the `windows/runner` folder

   確保備份完成後，刪除 `windows/runner` 資料夾下所有檔案；

5. Run `flutter create --platforms=windows .` to recreate the Windows project

   執行 `flutter create --platforms=windows .` 指令，重建 Windows 專案；

6. Review the changes to files in the `windows/runner` folder

   檢查 `windows/runner` 資料夾中檔案的改動；

7. Reapply any custom changes made to the files in the
   `windows/runner` folder prior to this migration

   重新應用你備份的 `windows/runner` 資料夾中所有檔案之前做的自訂修改；

8. Verify that your app builds using `flutter build windows`

   使用 `flutter build windows` 指令，驗證應用程式的建構情況。
