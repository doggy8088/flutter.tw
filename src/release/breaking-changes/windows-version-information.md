---
title: Migrate a Windows project to set version information
title: 遷移 Windows 專案以支援設定版本資訊
description: How to update a Windows project to set version information
description: 如何遷移 Windows 專案以支援設定版本資訊
---

Flutter 3.3 added support for setting the Windows app's version from
the `pubspec.yaml` file or through the `--build-name` and `--build-number`
build arguments. For more information, refer to the
[Build and release a Windows app][] documentation.

Flutter 3.3 新增了一項功能支援，
你可以透過 `pubspec.yaml` 檔案或者
使用 `--build-name` 和 `--build-number` 指令建構引數
來設定 Windows 應用程式的版本。
更多資訊，請參閱 [建構和釋出為 Windows 應用][Build and release a Windows app] 文件。

Projects created before Flutter version 3.3 need to be migrated
to support versioning.

在 Flutter 3.3 版本之前建立的專案需要透過遷移來支援版本管理。

## Migration steps

## 遷移步驟

Your project can be updated using these steps:

你的專案可以透過這些步驟進行遷移：

1. Verify you are on Flutter version 3.3 or newer using `flutter --version`

   使用 `flutter --version` 指令，確認你的 Flutter 是 3.3 或更高的版本

2. If needed, use `flutter upgrade` to update to the latest version of the
   Flutter SDK

   如果可以的話，請使用 `flutter upgrade` 指令，更新 Flutter SDK 到最新版本

3. Backup your project, possibly using git or some other version control system

   備份你的專案，請儘量使用 git 或一些其他的版本控制系統

4. Delete the `windows/runner/CMakeLists.txt` and `windows/runner/Runner.rc`
   files

   確保備份完成後，刪除 `windows/runner/CMakeLists.txt` 和 `windows/runner/Runner.rc` 檔案

5. Run `flutter create --platforms=windows .`

   執行 `flutter create --platforms=windows .` 指令

6. Review the changes to your `windows/runner/CMakeLists.txt` and
   `windows/runner/Runner.rc` files

   檢查 `windows/runner/CMakeLists.txt` 和 `windows/runner/Runner.rc` 檔案的改動

7. Verify your app builds using `flutter build windows`

   執行 `flutter build windows` 指令，驗證應用程式的建構情況

{{site.alert.note}}

  Follow the [run loop migration guide][] if the build fails
  with the following error message:

  如果建構失敗並顯示以下報錯資訊，
  請遵循 [Windows 事件迴圈遷移指南][run loop migration guide] 來解決問題：

  ```
  flutter_window.obj : error LNK2019: unresolved external symbol "public: void __cdecl RunLoop::RegisterFlutterInstance(class flutter::FlutterEngine *)" (?RegisterFlutterInstance@RunLoop@@QEAAXPEAVFlutterEngine@flutter@@@Z) referenced in function "protected: virtual bool __cdecl FlutterWindow::OnCreate(void)" (?OnCreate@FlutterWindow@@MEAA_NXZ)
  ```

{{site.alert.end}}

## Example

## 範例

[PR 721][] shows the migration work for the
[Flutter Gallery][] app.

[PR 721][] 展示了 [Flutter Gallery][] 應用程式的遷移工作。

[Build and release a Windows app]: {{site.url}}/deployment/windows#updating-the-apps-version-number
[run loop migration guide]: {{site.url}}/release/breaking-changes/windows-run-loop
[PR 721]: {{site.github}}/flutter/gallery/pull/721/files
[Flutter Gallery]: https://gallery.flutter.dev/