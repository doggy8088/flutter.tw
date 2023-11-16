## Get the Flutter SDK {#get-sdk}

## 獲取 Flutter SDK {#get-sdk}

{% include docs/china-notice.md %}

 1. Install the core development tools needed for Flutter:

    安裝 Flutter 所需的核心開發工具：

    ```terminal
    $ sudo apt install clang cmake ninja-build pkg-config libgtk-3-dev
    ```

    This downloads the compiler toolchain needed
    to compile apps for ChromeOS.

    這會下載用於編譯 ChromeOS 應用所需的編譯器工具鏈。

 1. Download Flutter from the [Flutter repo][]
    on GitHub with the following command in your home directory:

    從 GitHub 的 [Flutter repo][] 下載 Flutter，
    在你的主目錄使用以下命令：

    ```terminal
    $ git clone https://github.com/flutter/flutter.git -b stable
    ```

 1. Add the `flutter` tool to your path:

    將 `flutter` 工具新增到環境變數中：

    ```terminal
    $ echo PATH="$PATH:`pwd`/flutter/bin" >> ~/.profile
    $ source ~/.profile
    ```

You are now ready to run Flutter commands!

你現在已經準備好執行 Flutter 命令了！

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
    <strong>✗ Unable to locate Android SDK.
    Install Android Studio from:
    https://developer.android.com/studio/index.html</strong>
</pre>

The following sections describe how to perform these tasks and finish the setup
process.

上面的部分描述瞭如何執行這些任務，並完成設定流程。

Once you have installed any missing dependencies, run the `flutter doctor`
command again to verify that you've set everything up correctly.

當你已經安裝了全部缺失的依賴之後，請再次執行 `flutter doctor` 命令，
以驗證你是否是真的全部正確設定完畢了。

{% include_relative _analytics.md %}

[Flutter repo]: {{site.repo.flutter}}
