## Android setup (without Android Studio)

## Android 安裝（無需 Android Studio）

### Install Java

### 安裝 Java

```terminal
$ sudo apt update
$ sudo apt install default-jre
$ sudo apt install default-jdk
```

### Install the Android SDKs

### 安裝 Android SDK

Download the [Android SDK tools][] and
select the "Command Line Tools only" option.

下載 [Android SDK tools][]，
然後勾選 “Command Line Tools only” 選項。

Drag and drop the downloaded zip into your Linux Files folder through the
ChromeOS Files app. This moves the file to the home directory,
and is referred to as $TOOLS_PATH going forward (`~/`).

透過 ChromeOS 檔案應用，
把下載好的 zip 檔案拖放到你的 Linux 檔案資料夾中。
這個操作會把檔案移動到主目錄，接下來會被稱為 $TOOLS_PATH（`~/`）。

Unzip the tools and then add it to your path.

解壓縮下載的工具並且將解壓縮的檔案新增到路徑中。

```terminal
$ unzip ~/sdk-tools-linux*
$ export PATH="$PATH:$TOOLS_PATH/tools/bin"
```

Navigate to where you'd like to keep the SDK packages
($PLATFORM_PATH in these snippets) and download the SDK
packages using the sdkmanager tool (version numbers here are
the latest at time of publishing):

將 SDK 包放置到你熟悉的路徑下（對應的路徑變數是 $PLATFORM_PATH）。
然後使用 sdkmanager 工具下載 SDK 包 （這裡的版本號是最新的釋出版本）：

```terminal
$ sdkmanager "build-tools;28.0.3" "emulator" "tools" "platform-tools" "platforms;android-28" "extras;google;google_play_services" "extras;google;webdriver" "system-images;android-28;google_apis_playstore;x86_64"
```

Add the Android platform tools to your path (you should find this where you
ran the sdkmanager command: $PLATFORM_PATH):

將 Android 平台工具新增到你的路徑下
（和 sdkmanager 所處的路徑一致，也就是 $PLATFORM_PATH）：

```terminal
$ export PATH="$PATH:$PLATFORM_PATH/platform-tools"
```

Set the `ANDROID_SDK_ROOT` variable to where you unzipped sdk-tools before (aka
your $TOOLS_PATH):

將 `ANDROID_SDK_ROOT` 設定為你之前解壓縮 sdk-tools 的路徑
（也是你的 $TOOLS_PATH 路徑）：

```terminal
$ export ANDROID_SDK_ROOT="$TOOLS_PATH"
```

Now, run flutter doctor to accept the android-licenses:

現在，你可以執行 flutter doctor 來獲取 android-licenses：

```terminal
$ flutter doctor --android-licenses
```

[Android SDK tools]: {{site.android-dev}}/studio/#downloads
