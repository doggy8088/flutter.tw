## Get the Flutter SDK {#get-sdk}

## 獲取 Flutter SDK {#get-sdk}

{% include docs/china-notice.md %}

 1. Download the following installation bundle to get the latest
    {{site.sdk.channel}} release of the Flutter SDK:

    下載以下安裝包來獲取最新的 {{site.sdk.channel}} Flutter SDK：

    |Intel | | <span class="apple-silicon">Apple 晶片</span> |
    |------| | ---------------|
    |[(loading...)](#){:.download-latest-link-{{os}}.btn.btn-primary} | | [(loading...)](#){:.download-latest-link-{{os}}-arm64.apple-silicon.btn.btn-primary} |

    <br>
    For other release channels, and older builds,
    check out the [SDK archive][].

    想要獲取到其他版本的安裝套件，請參閱 [SDK 版本列表][SDK archive] 頁面。

    <div class="apple-silicon">
    {{site.alert.tip}}

      To determine whether your Mac uses an Apple silicon processor,
      refer to [Mac computers with Apple silicon]{:target="_blank"}
      on apple.com

      若要確定你的 Mac 是否是使用了 Apple 晶片處理器，請查閱蘋果官網的說明頁面: [搭載 Apple 晶片的 Mac 電腦][]。

    {{site.alert.end}}</div>

 1. Extract the file in the desired location. For example:

    將檔案解壓到目標路徑, 比如:

    {% comment %}
      Our JS also updates the filename in this template,
      but it doesn't include the terminal formatting:

      ```terminal
      $ cd ~/development
      $ unzip ~/Downloads/[[download-latest-link-filename]]flutter_{{os}}_vX.X.X-{{site.sdk.channel}}.zip[[/end]]
      ```
    {% endcomment -%}

    ```terminal
    $ cd ~/development
    $ unzip ~/Downloads/flutter_{{os}}_vX.X.X-{{site.sdk.channel}}.zip
    ```

 1. Add the `flutter` tool to your path:

    配置 `flutter` 的 PATH 環境變數：

    ```terminal
    $ export PATH="$PATH:`pwd`/flutter/bin"
    ```

    This command sets your `PATH` variable for the
    _current_ terminal window only.
    To permanently add Flutter to your path,
    check out [Update your path][].

    這個命令配置了 `PATH` 環境變數，且只會在你 **當前** 命令列視窗中生效。
    如果想讓它永久生效，請檢視 [更新 PATH 環境變數][Update your path]。

You are now ready to run Flutter commands!

現在你可以愉快地執行 Flutter 的命令列啦！

{{site.alert.note}}

  To update an existing version of Flutter,
  check out [Upgrading Flutter][].

  如果想要升級當前的 Flutter 版本，可以檢視 [升級 Flutter][Upgrading Flutter]。

{{site.alert.end}}

### Run flutter doctor

### 執行 flutter doctor 命令

Run the following command to see if there are any
dependencies you need to install to complete the setup
(for verbose output, add the `-v` flag):

透過執行以下命令來檢視當前環境是否需要安裝其他的依賴
（如果想檢視更詳細的輸出，增加一個 `-v` 引數即可）：

```terminal
$ flutter doctor
```

This command checks your environment and displays
a report to the terminal window.
The Dart SDK is bundled with Flutter;
it isn't necessary to install Dart separately.
Check the output carefully for other software you might
need to install or further tasks to perform
(shown in **bold** text).

這個命令會檢查你當前的配置環境，並在命令列視窗中產生一份報告。
安裝 Flutter 會附帶安裝 Dart SDK，所以不需要再對 Dart 進行單獨安裝。
你需要仔細閱讀上述命令產生的報告，看看別漏了一些需要安裝的依賴，
或者需要之後執行的命令（這個會以 **加粗的文字** 顯示出來）。

For example:

比如你可能會看到下面這樣的輸出：

<pre>
[-] Android toolchain - develop for Android devices
    • Android SDK at /Users/dash/Library/Android/sdk
    <strong>✗ Android SDK is missing command line tools; download from https://goo.gl/XxQghQ</strong>
    • Try re-installing or updating your Android SDK,
      visit {{site.url}}/setup/#android-setup for detailed instructions.
</pre>

The following sections describe how to perform these tasks
and finish the setup process.

之後的部分會向你描述如果執行這些命令來完成整體的配置過程。

Once you have installed any missing dependencies,
run the `flutter doctor` command again
to verify that you've set everything up correctly.

### Downloading straight from GitHub instead of using an archive

### 直接從 Github 上（而不是歸檔）下載

_This is only suggested for advanced use cases._

**該建議僅適用於高階使用案例**

You can also use git directly instead of downloading
the prepared archive. For example,
to download the stable branch:

你也可以不從歸檔，而是用 Git 直接下載。
例如，可以執行下方的命令，以下載穩定分支的 SDK：

```terminal
$ git clone https://github.com/flutter/flutter.git -b stable
```

[Update your path][], and run `flutter doctor`.
This lets you know if there are other dependencies
you need to install to use Flutter (such as the Android SDK).

[更新環境變數][Update your path]，並執行 `flutter doctor`。
這個命令將會告訴你，是否還缺少執行 Flutter 所需要安裝的其他依賴項（例如 Android SDK）。

If you didn't use the archive,
Flutter downloads necessary development binaries as they
are needed (if you used the archive,
they are included in the download). You might want to
pre-download these development binaries
(for example, you might do this when setting
up hermetic build environments,
or if you only have intermittent network availability).
To do so, run the following command:

如果你不使用歸檔，Flutter 將會下載必要的開發二進位制檔案（如果你使用的歸檔，那麼這些檔案已經包含在內了）。
你也許會想要提前下載這些開發二進位制檔案（例如，您可能希望設定系統建構環境，或是您的網路可用性不佳）。
那麼你可以執行以下命令：

```terminal
$ flutter precache
```

For additional download options, check out `flutter help precache`.

更多額外下載選項，請參閱 `flutter help precache`。

{% include_relative _analytics.md %}

[Flutter repo]: {{site.repo.flutter}}
[SDK archive]: {{site.url}}/release/archive
[Snap Store]: https://snapcraft.io/store
[snapd]: https://snapcraft.io/flutter
[Update your path]: #update-your-path
[Upgrading Flutter]: {{site.url}}/release/upgrade
[Mac computers with Apple silicon]: https://support.apple.com/en-us/HT211814
[搭載 Apple 晶片的 Mac 電腦]: https://support.apple.com/zh-cn/HT211814
