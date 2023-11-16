---
title: AndroidX migration
title: 遷移到 AndroidX
description: How to migrate existing Flutter projects to AndroidX.
description: 如何將現有的 Flutter 專案遷移到 AndroidX。
---

{{site.alert.note}}

  You might be directed to this page if Flutter detects
  that your project doesn't use AndroidX.

  如果 Flutter 檢測到你的專案未使用 AndroidX，
  可能會提示引導你檢視此頁面。

{{site.alert.end}}

[AndroidX][] is a major improvement
to the original Android Support Library.

[AndroidX][] 是對原有 Android 支援庫的重大改進。

It provides the `androidx.*` package libraries,
unbundled from the platform API. This means that it
offers backward compatibility and is updated
more frequently than the Android platform.

它提供了 `androidx.*` 庫，並且與平台 API 分離。
這意味著它提供了向後的相容性，
並且更新頻率比 Android 平台更頻繁。

[AndroidX]: {{site.android-dev}}/jetpack/androidx

## Common Questions

## 常見問題

### How do I migrate my existing app, plugin or host-editable module project to AndroidX?

### 如何將我現有的應用程式、外掛或可編輯的模組專案遷移到 AndroidX？

_You will need Android Studio 3.2 or higher.
If you don't have it installed,
you can download the latest version from the
[Android Studio][] site_.

**你需要 Android Studio 3.2 或更高版本。
如果你尚未安裝，
你可以從 [Android Studio][] 網站下載最新版本。**

1. Open Android Studio.

   開啟 Android Studio。

2. Select **Open an existing Android Studio Project**.

   選擇 **Open an existing Android Studio Project**。

3. Open the `android` directory within your app.

   開啟應用程式中的 `android` 目錄。

4. Wait until the project has been synced successfully.
   (This happens automatically once you open the project,
   but if it doesn't, select **Sync Project with Gradle Files**
   from the **File** menu).

   等待專案同步成功。
   （在你開啟專案後會自動同步，如果沒有，
   請從選單中選擇 **File** > **Sync Project with Gradle Files**）。

5. Select **Migrate to AndroidX** from the Refactor menu.

   從選單中選擇 **Refactor** > **Migrate to AndroidX**。

6. If you are asked to backup the project before proceeding,
   check **Backup project as Zip file**, then click **Migrate**.
   Lastly, save the zip file in your location of preference.

   在繼續之前，如果你被要求對專案進行備份，
   請勾選 **Backup project as Zip file**，
   然後點選 **Migrate**。
   最後，將 zip 檔案儲存在你期望的位置。

  <img
      width="500"
      style="border-radius: 12px;"
      src="/assets/images/docs/androidx/migrate_prompt.png"
      class="figure-img img-fluid"
      alt="Select backup project as zip file" />
7. The refactoring preview shows the list of changes.
   Finally, click **Do Refactor**:

   重構預覽顯示了變動的列表，
   最後，點選 **Do Refactor** ：

  <img
      width="600"
      style="border-radius: 12px;"
      src="/assets/images/docs/androidx/do_androidx_refactor.png"
      class="figure-img img-fluid"
      alt="An animation of the bottom-up page transition on Android" />
8. That is it! You successfully migrated your project to AndroidX.

   大功告成！你成功地將專案遷移到了 AndroidX。

Finally, if you migrated a plugin,
publish the new AndroidX version to pub and update
your `CHANGELOG.md` to indicate that this new version
is compatible with AndroidX.

最後，如果你遷移了外掛，
請釋出新的 AndroidX 版本到 pub 並更新你的 `CHANGELOG.md`，
以表明該新版本與 AndroidX 相容。

[Android Studio]: {{site.android-dev}}/studio

### What if I can't use Android Studio?

### 如果我不能使用 Android Studio 怎麼辦？

You can create a new project using the Flutter tool
and then move the Dart code and
assets to the new project.

你可以使用 Flutter 工具建立一個新專案，
然後將原專案的 Dart 程式碼和資原始檔轉移到新專案中。

To create a new project run:

要建立一個新的專案，請執行：

```bash
flutter create -t <project-type> <new-project-path>
```

### Add to app

### 整合到現有應用 (add-to-app)

If your Flutter project is a module type for adding
to an existing Android app, and contains a
`.android` directory, add the following line to `pubspec.yaml`:

如果你的 Flutter 專案是用於整合到現有 Android 應用的模組，
並且包含 `.android` 目錄，
請在 `pubspec.yaml` 中新增以下內容：

```yaml
 module:
   ...
    androidX: true # Add this line.
```

Finally, run `flutter clean`.

最後，執行 `flutter clean`。

If your module contains an `android` directory instead,
then follow the steps in previous section.

如果你的模組包含 `android` 目錄，
那麼請按照上面相關章節的步驟操作。

### How do I know if my project is using AndroidX?

### 如何判斷我的專案中是否在使用 AndroidX？

Starting from Flutter v1.12.13, new projects created with
`flutter create -t <project-type>`
use AndroidX by default.

從 Flutter 1.12.13 版本開始，
用 `flutter create -t <project-type>` 建立的新專案
將預設使用 AndroidX。

Projects created prior to this Flutter version
mustn't depend on any [old build artifact][] or
[old Support Library class][].

在此 Flutter 版本（1.12.13 版本）之前建立的專案
不能依賴於任何 [舊建構工件][old build artifact] 和 
[舊支援庫類][old Support Library class]。

[old build artifact]: {{site.android-dev}}/jetpack/androidx/migrate/artifact-mappings
[old Support Library class]: {{site.android-dev}}/jetpack/androidx/migrate/class-mappings

In an app or module project,
the file `android/gradle.properties`
or `.android/gradle.properties`
must contain:

在應用程式和模組專案中，
`android/gradle.properties` 或 
`.android/gradle.properties` 檔案內
必須包含以下內容： 

```
android.useAndroidX=true
android.enableJetifier=true
```

### What if I don't migrate my app or module to AndroidX?

### 如果我不將應用程式或模組遷移到 AndroidX 會怎麼樣？

Your app might continue to work. However,
combining AndroidX and Support artifacts
is generally not recommended because it can
result in dependency conflicts or
other kind of Gradle failures.
As a result, as more plugins migrate to AndroidX,
plugins depending on Android core libraries are likely
to cause build failures.

你的應用程式也許能繼續使用。
然而，一般不建議將 AndroidX 和 Android Support 結合使用，
因為它可能導致依賴衝突或其他型別的 Gradle 故障。
因此，隨著越來越多的外掛遷移到 AndroidX，
依賴於 Android 核心函式庫的外掛可能會導致建構失敗。

### What if my app is migrated to AndroidX, but not all of the plugins I use?

### 如果我的應用程式已經遷移到了 AndroidX，但我使用的外掛未全部支援 AndroidX 怎麼辦？

The Flutter tool uses Jetifier to automatically
migrate Flutter plugins using the Support Library
to AndroidX, so you can use the same plugins even
if they haven't been migrated to AndroidX yet.

Flutter 工具透過 Jetifier 將
使用支援庫的 Flutter 外掛自動遷移到 AndroidX，
因此，即使這些外掛尚未遷移到 AndroidX，
你仍然可以使用它們。

### I'm having issues migrating to AndroidX

### 我在遷移到 AndroidX 的過程中遇到問題

[Open an issue on GitHub][] and add `[androidx-migration]`
to the title of the issue.

[在 GitHub 提出一個 issue][Open an issue on GitHub] 
並在 issue 的標題中新增 `[androidx-migration]`。

[Open an issue on GitHub]: {{site.repo.flutter}}/issues/new/choose
