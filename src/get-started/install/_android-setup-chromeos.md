## Android setup

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

 1. Download and install [Android Studio]({{site.android-dev}}/studio).

    下載並安裝 [Android Studio]({{site.android-dev}}/studio)。

 1. Start Android Studio, and go through the 'Android Studio Setup Wizard'.
    This installs the latest Android SDK, Android SDK Command-line Tools,
    and Android SDK Build-Tools, which are required by Flutter
    when developing for Android.

    啟動 Android Studio，並前往 'Android Studio Setup Wizard'，
    這將會幫你安裝最新版本的 Android SDK，Android SDK 命令列工具，
    以及 Android SDK 建構工具，等一系列你在建構 Android 應用時會需要用到的元件。

 1. Accept Android licenses.

    同意 Android 許可協議（Android licenses）。

 ```terminal
$ flutter doctor --android-licenses
```

### Set up your Android device

### 設定你的 Android 裝置

To prepare to run and test your Flutter app on an Android device,
you need an Android device running Android 4.1 (API level 16) or higher.

在 Android 裝置上執行或測試你的 Flutter 之前，
需要確保 Android 裝置執行在 4.1（API 級別 16）或者更高的版本。

 1. Enable **Developer options** and **USB debugging** on your device.
    Detailed instructions are available in the
    [Android documentation]({{site.android-dev}}/studio/debug/dev-options).

    在你的裝置上啟動**開發者選項**以及 **USB 除錯**工具。
    詳細步驟請檢視 [Android 文件]({{site.android-dev}}/studio/debug/dev-options)。

 1. Using a USB cable, plug your phone into your computer. On your Chromebook,
    you may see a notification for "USB device detected". Click on "Connect
    to Linux" If prompted on your Android device, authorize your computer
    to access your device. 

    透過 USB 資料線連線你的手機與電腦。
    在 Chromebook 上，你可能會看到 "USB device detected"（USB 裝置已連線）的通知。
    如果你的 Android 裝置上出現點選 "Connect to Linux"（連線到 Linux）的提示，
    請授權計算機存取你的裝置。

 1. In the terminal, run the `flutter devices` command to verify that
    Flutter recognizes your connected Android device.  By default,
    Flutter uses the version of the Android SDK where your `adb`
    tool is based. If you want Flutter to use a different installation
    of the Android SDK, you must set the `ANDROID_SDK_ROOT` environment
    variable to that installation directory.

    在命令列執行 `flutter devices` 命令以驗證 Flutter 能夠識別你的 Android 裝置連線。
    預設情況下，flutter 使用基於 `adb` 工具的 Android SDK 版本。
    如果你想要 Flutter 執行並安裝在不同的 Android SDK 中的話，
    你必須將 `ANDROID_SDK_ROOT` 環境變數設定為該 SDK 的安裝目錄。

### Deploy to your Chromebook

### 在 Chromebook 上部署

With the latest version of Chrome OS, you no longer need to put your
device into developer mode to push apps to your Chrome OS device.

在最新版本的 Chrome OS 中，你不再需要將裝置置為開發者模式，就可以將你的應用部署到 Chrome OS 裝置中。

 1. [Enable ADB][] in Settings. Note that this will require you to reboot your
    device once. 

    在設定中[開啟 ADB][Enable ADB]。注意，這將會需要你重啟一次電腦。

 1. In the Terminal, run `flutter devices`. If prompted, authorize access to
    the Android container. Verify that `flutter devices` lists your Chrome
    OS device as a recognized device.

    在終端中執行 `flutter devices`。如果出現提示，請授權存取
    安卓容器。透過 `flutter devices` 驗證是否列出了您的 Chrome
    作業系統裝置作為識別的裝置。

[Enable ADB]: https://support.google.com/chromebook/answer/9770692
