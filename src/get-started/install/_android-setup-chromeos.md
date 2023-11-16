## Configuring Android app support

## Android 設定

{{site.alert.note}}

  Flutter relies on a full installation of Android Studio to supply
  its Android platform dependencies. However, you can write your
  Flutter apps in a number of editors; a later step discusses that.

  Flutter 需要完整安裝 Android Studio 才能支援其 Android 平台的依賴。
  然而，你也可以在許多其他編輯器上編寫 Flutter 應用，之後我們將會提到它。

{{site.alert.end}}

### Install Android Studio

### 安裝 Android Studio

 1. Download and install [Android Studio]({{site.android-dev}}/studio/install#chrome-os).

    下載並安裝 [Android Studio]({{site.android-dev}}/studio/install#chrome-os)。

 1. Start Android Studio, and go through the 'Android Studio Setup Wizard'.
    This installs the latest Android SDK, platform tools and build tooling
    that are required by Flutter when developing for Android.

    啟動 Android Studio，並前往 'Android Studio Setup Wizard'，
    這將會幫你安裝最新版本的 Android SDK，Android SDK 命令列工具，
    以及 Android SDK 建構工具，等一系列你在建構 Android 應用時會需要用到的元件。

 1. From the welcome dialog, choose **More Actions -> SDK Manager**.
    From the SDK Tools tab, select
    **Android SDK Command-line Tools (latest)**
    to install additional necessary tooling.

    從歡迎對話方塊中，選擇 **More Actions -> SDK Manager**。
    在 SDK 工具標籤頁中，選擇
    **Android SDK Command-line Tools (latest)**
    來安裝額外的必要工具。

 1. Accept Android licenses.

    同意 Android 許可協議（Android licenses）。

 ```terminal
$ flutter doctor --android-licenses
```

### Deploy to your Chromebook

### 部署到 Chromebook

To deploy apps directly to your Chromebook, you need to do the following:

要在 Chromebook 上直接部署應用程式，需要執行以下操作：

 1. [Enable ADB][] in Settings. Note that this requires you to reboot your
    device once.

    在設定中 [啟用 ADB][Enable ADB]。
    請注意，這需要你重啟一次裝置。

 1. In the Terminal, run `flutter devices`. If prompted, authorize access to
    the Android container. Verify that `flutter devices` lists your ChromeOS
    device as a recognized device.

    在終端執行 `flutter devices`。如果出現提示，
    請授權存取 Android 容器。
    驗證 `flutter devices` 是否將 ChromeOS 裝置列為可識別裝置。

### Set up your Android device

### 設定你的 Android 裝置

To prepare to run and test your Flutter app on an attached device,
you need an Android device running Android 5.0 (API level 21) or higher.

在 Android 裝置上執行或測試你的 Flutter 之前，
需要確保 Android 裝置執行在 4.1（API 級別 16）或者更高的版本。

 1. Enable **Developer options** and **USB debugging** on your device.
    Detailed instructions are available in the
    [Android documentation]({{site.android-dev}}/studio/debug/dev-options).

    在你的裝置上啟動**開發者選項**以及 **USB 除錯**工具。
    詳細步驟請檢視 [Android 文件]({{site.android-dev}}/studio/debug/dev-options)。

 1. Using a USB cable, plug your phone into your computer.
    On your Chromebook, you might see a notification for
    "USB device detected". Click on "Connect to Linux".
    If prompted on your Android device,
    authorize your computer to access your device.

    透過 USB 資料線連線你的手機與電腦。
    在 Chromebook 上，你可能會看到 "USB device detected"（USB 裝置已連線）的通知。
    如果你的 Android 裝置上出現點選 "Connect to Linux"（連線到 Linux）的提示，
    請授權計算機存取你的裝置。

 1. In the terminal, run the `flutter devices` command to verify
    that Flutter recognizes your connected Android device.
    By default, Flutter uses the version of the
    Android SDK where your `adb` tool is based.
    If you want Flutter to use a different installation
    of the Android SDK, you must set the `ANDROID_SDK_ROOT`
    environment variable to that installation directory.

    在命令列執行 `flutter devices` 命令以驗證 Flutter 能夠識別你的 Android 裝置連線。
    預設情況下，flutter 使用基於 `adb` 工具的 Android SDK 版本。
    如果你想要 Flutter 執行並安裝在不同的 Android SDK 中的話，
    你必須將 `ANDROID_SDK_ROOT` 環境變數設定為該 SDK 的安裝目錄。

[Enable ADB]: https://support.google.com/chromebook/answer/9770692
