---
title: AndroidX Migration
title: 遷移到 AndroidX
description: How to migrate existing Flutter projects to AndroidX.
description: 如何將現有的 Flutter 專案遷移到 AndroidX。
tags: Flutter開發,部署
keywords: AndoridX, Android Studio, Flutter 裡使用 AndroidX
---

{{site.alert.note}}

  You might be directed to this page if Flutter detects that your project
  doesn't use AndroidX.

  如果 Flutter 監測到你的專案中未使用到 AndroidX，那麼你會跳轉到此頁面。
{{site.alert.end}}

[AndroidX]({{site.android-dev}}/jetpack/androidx) is a major improvement
to the original Android Support Library.

[AndroidX]({{site.android-dev}}/jetpack/androidx) 是針對 Android 原生支援庫的重大改進。

It provides the `androidx.*` package libraries, unbundled from the platform API.
This means that it offers backward compatibility and is updated more frequently
than the Android platform.

其提供了套件名稱為 `androidx.*`，且並未與平台 API 關聯的類別庫，
這意味著它提供了向後的相容性，以及比 Android 平台更頻繁的更新。

## Common Questions

## 常見問題

### How do I migrate my existing app, plugin or host-editable module project to AndroidX?

### 如何將現有的應用程式、外掛，或者可編輯的模組專案遷移至 AndroidX?

_You will need Android Studio 3.2 or higher. If you don’t have it installed,
you can download the latest version from the
[Android Studio]({{site.android-dev}}/studio) site_.

**你需要 Android Studio 3.2 或其更高的版本。
若尚未安裝，可從 [Android Studio]({{site.android-dev}}/studio) 頁面
下載最新的版本。**

1. Open Android Studio.

   開啟 Android Studio。

2. Select **Open an existing Android Studio Project**.

   選中 **Open an existing Android Studio Project** 。

3. Open the `android` directory within your app.

   在你的應用路徑中開啟 `android` 目錄。

4. Wait until the project has been synced successfully.

   等待專案直到其同步成功。

  (This happens automatically once you open the project, but if it doesn’t,
   select **Sync Project with Gradle Files** from the **File** menu).

  （一旦開啟專案，同步就會自動建構，若沒有自動建構，
  請從 **File** 選單中選中 **Sync Project with Gradle Files**）。

5. Select **Migrate to AndroidX** from the Refactor menu.

   從 **Refactor** 選單中選擇 **Migrate to AndroidX** 。

6. If you are asked to backup the project before proceeding,
   check **Backup project as Zip file**, then click **Migrate**. Lastly, save
   the zip file in your location of preference. <br/>

   在繼續之前，若被要求對專案進行備份，選中 **Backup project as Zip file** ，
   然後單擊 **Migrate** ，最終將 zip 檔案儲存在你喜歡的路徑下。<br/>

  <img
      width="500"
      style="border-radius: 12px;"
      src="/assets/images/docs/androidx/migrate_prompt.png"
      class="figure-img img-fluid"
      alt="Select backup project as zip file" />
7. The refactoring preview shows the list of changes. Finally, click **Do Refactor**:

   重構預覽展示了變動的列表，最後，單擊 **Do Refactor** ：

  <img
      width="600"
      style="border-radius: 12px;"
      src="/assets/images/docs/androidx/do_androidx_refactor.png"
      class="figure-img img-fluid"
      alt="An animation of the bottom-up page transition on Android" />
      
8. That is it! You successfully migrated your project to AndroidX.

   大功告成！你已成功將專案遷移到 AndroidX。

Finally, if you migrated a plugin, publish the new AndroidX version to pub and update
your `CHANGELOG.md` to indicate that this new version is compatible with AndroidX.

最後，如果你對外掛進行了遷移，
請釋出新的 AndroidX 版本到 pub 並更新的 `CHANGELOG.md`，以指明該版本與 AndroidX 相容。

### What if I can't use Android Studio?

### 若無法使用 Android Studio 怎麼辦？

You can create a new project using the Flutter tool and then move the Dart code and
assets to the new project.

你可以使用 Flutter 工具建立一個新專案，
然後將 Dart 程式碼和資原始檔移動到新的專案中。

To create a new project run:

要建立一個新的專案，請執行：

```bash
flutter create -t <project-type> <new-project-path>
```

### Add to App

### 新增至應用

If your Flutter project is a module type for adding to an existing Android app, and
contains a `.android` directory, add the following line to `pubspec.yaml`:

若的 Flutter 專案型別是用於新增至現有 Android 應用的模組，
並且包含 `.android` 目錄，則將下述程式碼新增至 `pubspec.yaml` 中。

```yaml
 module:
   ...
    androidX: true # Add this line.
```

Finally, run `flutter clean`.

最後，執行 `flutter clean` 。

If your module contains an `android` directory instead, then follow the
steps in previous section.

若你的模組中包含一個 `android` 目錄，請按照上一節中的步驟執行。

### How do I know if my project is using AndroidX?

### 如何判斷我的專案中是否使用了 AndroidX？

Starting from Flutter v1.12.13, new projects created with `flutter create -t <project-type>`
use AndroidX by default.

自 Flutter 1.12.13 版本之後，使用 `flutter create -t <project-type>`
命令列建立的 Flutter 專案將會預設使用 AndroidX。

Projects created prior to this Flutter version must not depend on any
[old build artifact]({{site.android-dev}}/jetpack/androidx/migrate/artifact-mappings) or
[old Support Library class]({{site.android-dev}}/jetpack/androidx/migrate/class-mappings).

在此 Flutter 版本（1.12.13）之前建立的專案不能依賴任何
[工件對映]({{site.android-dev}}/jetpack/androidx/migrate/artifact-mappings)
和 [類對映]({{site.android-dev}}/jetpack/androidx/migrate/class-mappings)。

In an app or module project, the file `android/gradle.properties` or `.android/gradle.properties`
must contain:

`android/gradle.properties` 或 `.android/gradle.properties` 檔案中需要包含下述程式碼：

```
android.useAndroidX=true
android.enableJetifier=true
```

### What if I don’t migrate my app or module to AndroidX?

### 若不將應用程式或模組遷移至 AndroidX 將會怎樣？

Your app may continue to work. However, combining AndroidX and Support artifacts
is generally not recommended because it can result in dependency conflicts or
other kind of Gradle failures. As a result, as more plugins migrate to AndroidX,
plugins depending on Android core libraries are likely to cause build failures.

你的應用程式也許能繼續執行。
然而，通常不建議將 AndroidX 和 Support 元件結合起來使用，
因為這會導致依賴關係衝突或者 Gradle 的其它型別失敗。

### What if my app is migrated to AndroidX, but not all of the plugins I use?

### 若我將應用遷移到了 AndroidX，但我使用到的外掛沒有全部支援 AndroidX 怎麼辦？

The Flutter tool uses Jetifier to automatically migrate Flutter plugins using
the Support Library to AndroidX, so you can use the same plugins even if they
haven’t been migrated to AndroidX yet.

Flutter 工具使用 Jetifier 將支援庫中的 Flutter 外掛自動遷移到 AndroidX，
因此，即使你尚未將其遷移到 AndroidX ，你也可以使用相同的外掛。

### I'm having issues migrating to AndroidX

### 在遷移至 AndroidX 的過程中遇到了問題

[Open an issue on GitHub]({{site.repo.flutter}}/issues/new/choose)
and add `[androidx-migration]` to the title of the issue.

[在 GitHub 上建立一個問題]({{site.github}}/flutter/flutter/issues/new/choose)
併為其新增一個 `[androidx-migration]` 標題。

