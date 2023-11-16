## Get the Flutter SDK

## 獲取 Flutter SDK

{% include docs/china-notice.md %}

{% include_relative _help-link.md location='win-get-sdk' %}

 1. Download the following installation bundle to get the latest
    {{site.sdk.channel}} release of the Flutter SDK:

    點選下方的安裝套件，獲取 {{site.sdk.channel}} 發行通道的 Flutter SDK 最新版本：

    [(loading...)](#){:.download-latest-link-{{os}}.btn.btn-primary}

    For other release channels, and older builds,
    heck out the [SDK archive][].

    要檢視其他發行通道和以往的版本，請參閱
    [SDK 版本列表][SDK archive] 頁面。

 1. Extract the zip file and place the contained `flutter`
    in the desired installation location for the Flutter SDK
    (for example, `%USERPROFILE%\flutter`, `D:\dev\flutter`).

    將壓縮包解壓，然後把其中的 `flutter` 目錄整個放在
    你想放置 Flutter SDK 的路徑中（例如 `%USERPROFILE%\flutter` 或者 `D:\dev\flutter`）。

{{site.alert.warning}}

  Do not install Flutter to a path that contains special
  characters or spaces.

  請勿將 Flutter 有特殊字元或空格的路徑下。

{{site.alert.end}}

{{site.alert.warning}}

  Do not install Flutter in a directory like `C:\Program Files\` that requires
  elevated privileges.

  請勿將 Flutter 安裝在需要高許可權的資料夾內，例如 `C:\Program Files\`。

{{site.alert.end}}

You are now ready to run Flutter commands in the Flutter Console.

現在你可以在控制檯當中使用 Flutter 的命令了。

[Flutter repo]: {{site.repo.flutter}}

### Update your path

### 更新 path 環境變數

{% include_relative _help-link.md location='win-path' section='#unable-to-find-the-flutter-command' %}

If you wish to run Flutter commands in the regular Windows console,
take these steps to add Flutter to the `PATH` environment variable:

如果你想在 Windows 控制檯中執行 Flutter 命令，需要按照下面的步驟來將 Flutter 的
執行檔案路徑加入到 `PATH` 環境變數。

* From the Start search bar, enter 'env'
  and select **Edit environment variables for your account**.

  在開始選單的搜尋功能鍵入「env」，然後選擇 **編輯系統環境變數**。

* Under **User variables** check if there is an entry called **Path**:

  在 **使用者變數** 一欄中，檢查是否有 **Path** 這個條目：

  * If the entry exists, append the full path to `flutter\bin` using
    `;` as a separator from existing values.

    如果存在這個條目，以 `;` 分隔已有的內容，加入 `flutter\bin` 目錄的完整路徑。

  * If the entry doesn't exist,
    create a new user variable named `Path` with
    the full path to `flutter\bin` as its value.

    如果不存在的話，在使用者環境變數中建立一個新的 `Path` 變數，
    然後將 `flutter\bin` 所在的完整路徑作為新變數的值。

You have to close and reopen any existing console windows
for these changes to take effect.

你需要重新開啟已經開啟的命令列提示符視窗，
這樣下次啟動命令提示符時，才能存取到剛才修改的變數。

{% include docs/dart-tool-win.md %}

### Run `flutter doctor`

### 執行 `flutter doctor`

{% include_relative _help-link.md location='win-doctor' %}

From a console window that has the Flutter directory in the
path (see above), run the following command to see if there
are any platform dependencies you need to complete the setup:

在將 `Path` 變數更新後，開啟一個新的控制檯視窗，然後執行下面的命令。
如果它提示有任何的平台相關依賴，那麼你就需要按照指示完成這些配置：

```batchfile
C:\src\flutter>flutter doctor
```

This command checks your environment and displays a report of the status
of your Flutter installation. Check the output carefully for other
software you might need to install or further tasks to perform
(shown in **bold** text).

上述命令會檢查你的現有環境，並將檢測結果以報告形式呈現出來。
仔細閱讀它顯示的內容，檢查是否有尚未安裝的軟體
或是有其他的步驟需要完成（通常會以**粗體**呈現）。

For example:

例如：

<pre>
[-] Android toolchain - develop for Android devices
    • Android SDK at D:\Android\sdk
    <strong>✗ Android SDK is missing command line tools; download from https://goo.gl/XxQghQ</strong>
    • Try re-installing or updating your Android SDK,
      visit {{site.url}}/setup/#android-setup for detailed instructions.
</pre>

The following sections describe how to perform these tasks and
finish the setup process. Once you have installed any missing
dependencies, you can run the `flutter doctor` command again to
verify that you've set everything up correctly.

下面的章節介紹了對缺失的內容進行配置的方法。
每當您安裝了任何一個的依賴項，
就可以隨時執行 `flutter doctor` 來檢查是否正確配置了所有內容。

{{site.alert.note}}

  If `flutter doctor` returns that either the Flutter plugin
  or Dart plugin of Android Studio are not installed, move
  on to [Set up an editor][] to resolve this issue.

  如果 `flutter doctor` 提示 Android Studio 的
  Flutter 或者 Dart 外掛尚未安裝，請移步文件
  [編輯器設定][Set up an editor] 查閱如何解決這個問題。

{{site.alert.end}}

{% include_relative _analytics.md %}


[Flutter repo]: {{site.repo.flutter}}
[SDK archive]: {{site.url}}/release/archive
[Set up an editor]: {{site.url}}/get-started/editor?tab=androidstudio
