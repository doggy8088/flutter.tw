{% if os == 'linux' -%}
  {% assign unzip = 'tar xf' -%}
  {% assign file_ext = '.tar.xz' -%}
{% else -%}
  {% assign unzip = 'unzip' -%}
  {% assign file_ext = '.zip' -%}
{% endif -%}

## Get the Flutter SDK {#get-sdk}

## 獲取 Flutter SDK {#get-sdk}

 1. Download the following installation bundle to get the latest
    {{site.sdk.channel}} release of the Flutter SDK:

    下載下面的安裝包以獲得最新的 Flutter SDK 的 {{site.sdk.channel}}釋出套件。

    [(loading...)](#){:.download-latest-link-{{os}}.btn.btn-primary}

    For other release channels, and older builds,
    see the [SDK releases][] page.

    對於其他釋出頻道，以及更早的版本，
    請檢視 [SDK 釋出][SDK releases] 頁面。

 1. In the Files app, drag-and-drop the downloaded file from "Downloads"
    to "Linux Files" to access Flutter from your Linux container.

    在檔案管理應用中，將下載好的檔案從 "Downloads" 拖拽到 "Linux Files" 中，以便能夠從 Linux container 中存取到 Flutter。

 1. Extract the file in the desired location, for example:

    將檔案解壓到合適的地方，例如：

    {% comment %}
      Our JS also updates the filename in this template, but it doesn't include the terminal formatting:

      {% prettify shell %}
      $ cd ~/development
      $ {{unzip}} ~/Downloads/[[download-latest-link-filename]]flutter_{{os}}_vX.X.X-{{site.sdk.channel}}{{file_ext}}[[/end]]
      {% endprettify %}
    {% endcomment -%}

    ```terminal
    $ cd ~/development
    $ {{unzip}} ~/Downloads/flutter_{{os}}_vX.X.X-{{site.sdk.channel}}{{file_ext}}
    ```

    If you don't want to install a fixed version of the installation bundle, 
    you can skip steps 1 and 2. 
    Instead, get the source code from the [Flutter repo][]
    on GitHub with the following command:

    如果你不想安裝安裝套件的補丁，你可以跳過步驟 1 或 步驟 2，
    直接獲取 Github 上 [Flutter 儲存庫][Flutter repo] 的原始碼並執行以下命令：

    ```terminal
    $ git clone https://github.com/flutter/flutter.git
    ```

    You can also change branches or tags as needed.
    For example, to get just the stable version:

    你也可以按你的需要切換分支或者tag。
    例如，你可以使用 stable 版本的分支：

    ```terminal
    $ git clone https://github.com/flutter/flutter.git -b stable
    ```

 1. Add the `flutter` tool to your path:

    將 `flutter` 工具新增到環境變數中：

    ```terminal
    $ export PATH="$PATH:`pwd`/flutter/bin"
    ```

    This command sets your `PATH` variable for the
    _current_ terminal window only.
    To permanently add Flutter to your path, see
    [Update your path][].

    用這個命令新增 `PATH` 僅在當前的命令列視窗生效。
    要將 Flutter 永久新增到環境變數中，請參閱
    [更新環境變數值][Update your path]。

 1. Optionally, pre-download development binaries:

    可選步驟，提前下載二進位制開發檔案：

    The `flutter` tool downloads platform-specific development binaries as
    needed. For scenarios where pre-downloading these artifacts is preferable
    (for example, in hermetic build environments,
    or with intermittent network availability), iOS
    and Android binaries can be downloaded ahead of time by running:

    `flutter` 工具將下載所需的平台特殊開發二進位制檔案。
    對於預下載這些工件更好的做法是（例如，在系統建構環境中，
    網路可能出現不通暢的問題），透過執行下面命令提前下載 iOS
    和 Android 的二進位制檔案：

    ```terminal
    $ flutter precache
    ```

    For additional download options, see `flutter help precache`.

    對於這些可選的下載項，請參考 `flutter help precache`。

You are now ready to run Flutter commands!

你現在可以執行 Flutter 命令了！

{{site.alert.note}}

  To update an existing version of Flutter, see
  [Upgrading Flutter][].

  要更新已有 Flutter版本，請參閱 [升級你的 Flutter][Upgrading Flutter]。

{{site.alert.end}}

### Run flutter doctor

### 執行 flutter doctor

Run the following command to see if there are any dependencies you need to
install to complete the setup (for verbose output, add the `-v` flag):

執行以下命令以檢視是否還有缺失的依賴需要安裝，你需要安裝這些依賴以完成設定
（要看到詳細輸出，請新增 `-v` 標識）：

```terminal
$ flutter doctor
```

This command checks your environment and displays a report to the terminal
window. The Dart SDK is bundled with Flutter; it is not necessary to install
Dart separately. Check the output carefully for other software you might
need to install or further tasks to perform (shown in **bold** text).

該命令將檢查你的環境情況並顯示彙報在命令列視窗中。
Dart SDK 已經綁在 Flutter 中了；你無需單獨再安裝 Dart。
仔細檢查你是否還有需要安裝的東西，或者要執行的任務（在該文字中提示的）。

For example:

例如：

<pre>
[-] Android toolchain - develop for Android devices
    • Android SDK at /Users/obiwan/Library/Android/sdk
    <strong>✗ Android SDK is missing command line tools; download from https://goo.gl/XxQghQ</strong>
    • Try re-installing or updating your Android SDK,
      visit {{site.url}}/setup/#android-setup for detailed instructions.
</pre>

The following sections describe how to perform these tasks and finish the setup
process.

上面的部分描述瞭如何執行這些任務，並完成設定流程。

Once you have installed any missing dependencies, run the `flutter doctor`
command again to verify that you’ve set everything up correctly.

當你已經安裝了全部缺失的依賴之後，請再次執行 `flutter doctor` 命令，
以驗證你是否是真的全部正確設定完畢了。

{% include_relative _analytics.md %}

[Flutter repo]: {{site.repo.flutter}}
[Installing snapd]: https://snapcraft.io/docs/installing-snapd
[SDK releases]: {{site.url}}/development/tools/sdk/releases
[Snap Store]: https://snapcraft.io/store
[snapd]: https://snapcraft.io/flutter
[Update your path]: #update-your-path
[Upgrading Flutter]: {{site.url}}/development/tools/sdk/upgrading
