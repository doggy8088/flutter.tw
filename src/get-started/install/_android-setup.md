## Android setup

## 設定 Android 開發環境

{{site.alert.note}}

  Flutter relies on a full installation of Android Studio to supply
  its Android platform dependencies. However, you can write your
  Flutter apps in a number of editors; a later step discusses that.

  Flutter 依賴 Android Studio 的全量安裝來為其提供 Android 平台的支援。
  但你也可以使用其他的編輯器來寫 Flutter 應用，接下來的步驟會提到這個問題。

{{site.alert.end}}

### Install Android Studio

### 安裝 Android Studio


 1. Download and install [Android Studio]({{site.android-dev}}/studio).

    下載並安裝 [Android Studio]({{site.android-dev}}/studio)。

 1. Start Android Studio, and go through the 'Android Studio Setup Wizard'.
    This installs the latest Android SDK, Android SDK Command-line Tools,
    and Android SDK Build-Tools, which are required by Flutter
    when developing for Android.

    執行 Android Studio，並進入 'Android Studio Setup Wizard'，
    這會安裝最新的 Android SDK，
    Android SDK Platform-Tools 以及 Android SDK Build-Tools，
    這些都是在開發 Android Flutter 應用時所需要的。

 1. Run `flutter doctor` to confirm that Flutter has located
    your installation of Android Studio. If Flutter cannot locate it,
    run `flutter config --android-studio-dir <directory>` to set the
    directory that Android Studio is installed to.

    執行 `flutter doctor` 確保 Flutter 已經定位到了你的 Android Studio 的安裝位置。
    如果 Flutter 並未定位到，執行 `flutter config --android-studio-dir <directory>`
    設定你的 Android Studio 的安裝目錄。

### Set up your Android device

### 配置 Android 裝置

To prepare to run and test your Flutter app on an Android device,
you need an Android device running Android 4.1 (API level 16) or higher.

在 Android 裝置上執行或測試 Flutter 應用之前，
你需要一個執行 Android 4.1（API 版本 16）或者更高的裝置。

 1. Enable **Developer options** and **USB debugging** on your device.
    Detailed instructions are available in the
    [Android documentation]({{site.android-dev}}/studio/debug/dev-options).

    在裝置上開啟 **Developer options** 和 **USB debugging** 選項，
    你可以在 [Android documentation]({{site.android-dev}}/studio/debug/dev-options) 上檢視更詳細的方法介紹。
 
 1. Windows-only: Install the [Google USB
    Driver]({{site.android-dev}}/studio/run/win-usb).

    如果是在 Windows 平臺上使用，需要安裝 [Google USB Driver]({{site.android-dev}}/studio/run/win-usb)
 
 1. Using a USB cable, plug your phone into your computer. If prompted on your
    device, authorize your computer to access your device.
 
    透過 USB 介面連線手機和電腦，如果在裝置上彈出需要授權彈窗，
    允許授權以便讓電腦能夠存取你的開發裝置。
 
 1. In the terminal, run the `flutter devices` command to verify that
    Flutter recognizes your connected Android device.  By default,
    Flutter uses the version of the Android SDK where your `adb`
    tool is based. If you want Flutter to use a different installation
    of the Android SDK, you must set the `ANDROID_SDK_ROOT` environment
    variable to that installation directory.

    在命令列中，使用 `flutter devices`
    命令來確保 Flutter 能夠識別出你所連線的 Android 裝置。

### Set up the Android emulator

### 配置 Android 模擬器

To prepare to run and test your Flutter app on the Android emulator,
follow these steps:

根據以下步驟來將 Flutter 應用執行或測試於你的 Android 模擬器上：

 1. Enable
    [VM acceleration]({{site.android-dev}}/studio/run/emulator-acceleration#accel-vm)
    on your machine.

    啟用機器上的 
    [VM acceleration]({{site.android-dev}}/studio/run/emulator-acceleration) 選項。
 
 1. Launch **Android Studio**, click the **AVD Manager**
    icon, and select **Create Virtual Device...**

    開啟 **Android Studio**，點選 **AVD Manager** 按鈕，
    選擇 **Create Virtual Device...**

     * In older versions of Android Studio, you should instead
    launch **Android Studio > Tools > Android > AVD Manager** and select
    **Create Virtual Device...**. (The **Android** submenu is only present
    when inside an Android project.)

       在一些舊的 Android Studio 版本里，需要透過
       **Android Studio > Tools > Android > AVD Manager**，
       然後選擇 **Create Virtual Device...** 選項。
       （只有在 Android 專案中才會顯示 **Android** 子選項。）

     * If you do not have a project open, you can choose 
    **Configure > AVD Manager** and select **Create Virtual Device...**

       如果你以及還沒開啟某個專案，你可以選擇
       **Configure > AVD Manager** 然後選擇 **Create Virtual Device** 選項

 1. Choose a device definition and select **Next**.

    選擇相應的裝置並選擇 **Next** 選項。

 1. Select one or more system images for the Android versions you want
    to emulate, and select **Next**.
    An _x86_ or _x86\_64_ image is recommended.

    選擇一個或多個你想要模擬的 Android 版本的系統映象，
    然後選擇 **Next** 選項。推薦選擇 **x86** 或者 **x86\_64** 映象。

 1. Under Emulated Performance, select **Hardware - GLES 2.0** to enable
    [hardware
    acceleration]({{site.android-dev}}/studio/run/emulator-acceleration).

    在 Emulated Performance 下選擇 **Hardware - GLES 2.0** 選項來開啟
    [硬體加速]({{site.android-dev}}/studio/run/emulator-acceleration)。

 1. Verify the AVD configuration is correct, and select **Finish**.
    
    確保 AVD 選項配置正確，並選擇 **Finish** 選項。

    For details on the above steps, see [Managing
    AVDs]({{site.android-dev}}/studio/run/managing-avds).

    想要檢視上述步驟的更多詳細資訊，
    請檢視 [Managing AVDs]({{site.android-dev}}/studio/run/managing-avds) 頁面。

 1. In Android Virtual Device Manager, click **Run** in the toolbar.
    The emulator starts up and displays the default canvas for your
    selected OS version and device.

    在 Android Virtual Device Manager 中，點選工具欄中的 **Run** 選項，
    模擬器會啟動併為你所選擇的系統版本和裝置顯示出相應的介面。

### Agree to Android Licenses

### 同意 Android 協議

Before you can use Flutter, you must agree to the
licenses of the Android SDK platform. This step should be done after
you have installed the tools listed above.

在使用 Flutter 前，你必須同意 Android SDK 平台的協議。
你可以在安裝完上述工具後執行這一步。

 1. Make sure that you have a version of Java 8 installed and that your 
    `JAVA_HOME` environment variable is set to the JDK's folder.

    確保你安裝了 Java 8，並且正確設定了 `JAVA_HOME` 環境變數到 JDK 目錄。

    Android Studio versions 2.2 and higher come with a JDK, so this should
    already be done.

    高於 2.2 版本的 Android Studio 自帶了 JDK，所以應無需手動操作。

 1. Open an elevated console window and run the following command to begin
    signing licenses.

    開啟一個已經提升管理員許可權的終端視窗，執行以下命令進行協議的確認。

    ```terminal
    $ flutter doctor --android-licenses
    ```
 1. Review the terms of each license carefully before agreeing to them.

    仔細閱讀每條協議後同意。

 1. Once you are done agreeing with licenses, run `flutter doctor` again
    to confirm that you are ready to use Flutter.

    當你同意所有協議後，再次執行 `flutter doctor` 以確認是否已經可以正常使用 Flutter。