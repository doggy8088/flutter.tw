## Get the Flutter SDK {#get-sdk}

## 獲取 Flutter SDK {#get-sdk}

{% include docs/china-notice.md %}

To install the Flutter SDK on your Linux system,
use one of the following methods.

在使用 Linux 時，你可以透過如下的方式安裝 Flutter。

### Method 1: Install Flutter using snapd

### 方式 1：使用 snapd 安裝 Flutter

This offers the most direct method to install
the Flutter SDK on your Linux system.

使用 snapd 在 Linux 上安裝 Flutter 是最直接的方式。

To learn about using snapd, check [Installing snapd][].

更多使用 snapd 的課程，請檢視 [安裝 snapd][Installing snapd]。

After you install `snapd`, [install Flutter from the Snap Store][] or
run the following command:

安裝 snapd 後，你可以
[透過 Snap 商店來安裝 Flutter][install Flutter from the Snap Store]，
或者透過以下命令安裝：

```terminal
$ sudo snap install flutter --classic
```

{{site.alert.note}}

  After you install Flutter with `snapd`,
  display your Flutter SDK path with the following command:

  安裝 snap 後，可以使用如下命令展示 Flutter SDK 路徑：

  ```terminal
  $ flutter sdk-path
  ```

{{site.alert.end}}

### Method 2: Manual installation

### 方式 2：手動安裝 Flutter

If you aren't using `snapd`, follow these steps to install Flutter.

如果你沒有 `snapd`，那麼你可以透過以下步驟安裝 Flutter。

1. Download the installation bundle for the latest
   {{site.sdk.channel}} release of the Flutter SDK:

   透過下載下面的安裝包以獲得最新 {{site.sdk.channel}} release 版本的 Flutter SDK：

   [(loading...)](#){:.download-latest-link-{{os}}.btn.btn-primary}

   You can find older builds and other release channels in the [SDK archive][].

   對於其他釋出頻道以及更久的建構版本，
   請檢視 [SDK 釋出][SDK archive] 頁面。

1. Extract the downloaded file to a location of your choice:

   將檔案解壓到合適的地方，例如：

    ```terminal
    $ cd ~/development
    $ tar xf ~/Downloads/flutter_{{os}}_vX.X.X-{{site.sdk.channel}}.tar.xz
    ```

1. Add the `flutter` tool to your path:

   將 `flutter` 工具新增到環境變數中：

    ```terminal
    $ export PATH="$PATH:`pwd`/flutter/bin"
    ```

    This command sets your `PATH` environment variable for the current
    terminal window only.
    To add Flutter as permanent part of your path,
    check out [Update your path][].

   用這個命令新增 `PATH` 僅在當前的命令列視窗生效。
   要將 Flutter 永久新增到環境變數中，請參閱
   [更新您的路徑][Update your path]。

1. (Optional) Pre-download development binaries:

   （可選）預下載開發二進位制檔案：

    ```terminal
    $ flutter precache
    ```

    To find additional download options, run `flutter help precache`.

   你可以執行 `flutter help precache` 檢視其他的下載選項。

{{site.alert.note}}

  To update an existing version of Flutter, see [Upgrading Flutter][].

  要更新已有 Flutter版本，請參閱 [升級你的 Flutter][Upgrading Flutter]。

{{site.alert.end}}

### Verify your install with `flutter doctor`

### 執行 `flutter doctor` 校驗安裝

After installing Flutter, run `flutter doctor`.

安裝 Flutter 後，執行 `flutter doctor`：

```terminal
$ flutter doctor
```

This command checks your environment and displays a report in the
terminal window.
Flutter bundles the Dart SDK. You don't need to install Dart.

該命令將檢查你的環境情況並顯示彙報在命令列視窗中。
Dart SDK 已經綁在 Flutter 中了；你無需單獨再安裝 Dart。

To get greater detail on what you need to fix, add the `-v` flag:

若你想了解你需要進一步配置的內容，使用 `-v` 執行：

```terminal
$ flutter doctor -v
```

Review the output for further tasks to perform.
An example would be the text shown in **bold**.

仔細檢查你是否還有需要配置的內容或者要執行的任務（例如加粗顯示的內容）。

The `flutter doctor -v` output might resemble the following:

`flutter doctor -v` 命令可能會包含以下內容：

{% comment %}
Need to use HTML for this code block to get the replacements
and boldface to work.
{% endcomment -%}

<pre>
[-] Android toolchain - develop for Android devices
    • Android SDK at /Users/dash/Library/Android/sdk
    <strong>✗ Android SDK is missing command line tools; download from https://goo.gl/XxQghQ</strong>
    • Try re-installing or updating your Android SDK,
      visit {{site.url}}/setup/#android-setup for detailed instructions.
</pre>

The following sections describe how to perform these tasks
and finish the setup process.

上面的部分描述瞭如何執行這些任務，並完成設定流程。

After completing the outlined tasks,
run the `flutter doctor` command again.

當你已經執行了列出的所有任務後，
再次執行 `flutter doctor` 命令以驗證這些建構是否設定正確。

{% include_relative _analytics.md %}

[Flutter repo]: {{site.repo.flutter}}
[install Flutter from the Snap Store]: https://snapcraft.io/flutter
[Installing snapd]: https://snapcraft.io/docs/installing-snapd
[SDK archive]: {{site.url}}/release/archive
[Update your path]: #update-your-path
[Upgrading Flutter]: {{site.url}}/release/upgrade
