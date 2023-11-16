---
title: Build and release an Android app
title: 建構和釋出為 Android 應用
description: How to prepare for and release an Android app to the Play store.
description: 如何打包把 App 釋出到 Play 商店。
short-title: Android
tags: 釋出, Android
keywords: 上傳Google商店,釋出Flutter應用
---

During a typical development cycle,
you test an app using `flutter run` at the command line,
or by using the **Run** and **Debug**
options in your IDE. By default,
Flutter builds a _debug_ version of your app.

在一般的開發過程中，我們可以使用 `flutter run` 命令，
或者 IntelliJ 工具欄中的 **Run** 和 **Debug** 來測試 app。
這時候，Flutter 預設會為我們建構 app 的**除錯**版本。

When you're ready to prepare a _release_ version of your app,
for example to [publish to the Google Play Store][play],
this page can help. Before publishing,
you might want to put some finishing touches on your app.
This page covers the following topics:

當想要釋出 app 時，比如 [釋出到 Google Play Store][play]，
可以按照以下步驟來準備 Android 平台的 **釋出** 版本。
本頁面的內容包含如下主題：

* [Adding a launcher icon](#adding-a-launcher-icon)

  [新增啟動畫標](#adding-a-launcher-icon)

* [Enabling Material Components](#enabling-material-components)

  [啟用 Material 元件](#enabling-material-components)

* [Signing the app](#signing-the-app)

  [建立一個金鑰庫](#signing-the-app)

* [Shrinking your code with R8](#shrinking-your-code-with-r8)

  [使用 R8 縮小你的程式碼體積](#shrinking-your-code-with-r8)

* [Enabling multidex support](#enabling-multidex-support)

  [啟用 MultiDex](#enabling-multidex-support)

* [Reviewing the app manifest](#reviewing-the-app-manifest)

  [檢查 app manifest 檔案](#reviewing-the-app-manifest)

* [Reviewing the build configuration](#reviewing-the-gradle-build-configuration)

  [檢查建構配置](#reviewing-the-gradle-build-configuration)

* [Building the app for release](#building-the-app-for-release)

  [為釋出建構應用](#building-the-app-for-release)

* [Publishing to the Google Play Store](#publishing-to-the-google-play-store)

  [釋出到 Google Play Store](#publishing-to-the-google-play-store)

* [Updating the app's version number](#updating-the-apps-version-number)

  [更新應用版本號](#updating-the-apps-version-number)

* [Android release FAQ](#android-release-faq)

  [Android 釋出常見問題](#android-release-faq)

{{site.alert.note}}

   Throughout this page, `[project]` refers to
   the directory that your application is in. While following
   these instructions, substitute `[project]` with 
   your app's directory.

   在整個頁面中，`[project]` 是指
   您的應用所處的目錄。同時關注
   這些說明，替換 `[project]` 為
   您的應用的目錄。

{{site.alert.end}}

## Adding a launcher icon

## 新增啟動畫標

When a new Flutter app is created, it has a default launcher icon.
To customize this icon, you might want to check out the
[flutter_launcher_icons][] package.

當我們建立一個新的 Flutter app 的時候，它會有一個預設的啟動畫標。
要自訂這個圖示，可以參考使用 [flutter_launcher_icons][] 這個 package。

Alternatively, you can do it manually using the following steps:

或者，如果我們想手動操作，可以參考以下方法：

1. Review the [Material Design product
   icons][launchericons] guidelines for icon design.

   檢視 [Material Design Product Icons][launchericons] 指南中圖示設計部分。

1. In the `[project]/android/app/src/main/res/` directory,
   place your icon files in folders named using
   [configuration qualifiers][].
   The default `mipmap-` folders demonstrate the correct
   naming convention.

   在 `<app dir>/android/app/src/main/res/` 目錄下，
   把我們的圖示檔案放在以 [配置限定符][configuration qualifiers] 命名的資料夾中。
   類似預設的 `mipmap-` 資料夾這樣的命名方式。

1. In `AndroidManifest.xml`, update the
   [`application`][applicationtag] tag's `android:icon`
   attribute to reference icons from the previous
   step (for example,
   `<application android:icon="@mipmap/ic_launcher" ...`).

   在 `AndroidManifest.xml` 中，更新 [`application`][applicationtag] 標籤中的
   `android:icon` 屬性來參考上一步驟中我們自己的圖示檔案
   (例如，`<application android:icon="@mipmap/ic_launcher" ...`)。

1. To verify that the icon has been replaced,
   run your app and inspect the app icon in the Launcher.

   用 `flutter run` 執行 app，檢查啟動程式中的 app 圖示
   是否已經替換成我們自己的圖示檔案。

## Enabling Material Components

## 啟用 Material 元件

If your app uses [Platform Views][], you might want to enable
Material Components by following the steps described in the
[Getting Started guide for Android][].

如果你的應用使用了 [平臺視圖 (Platform Views)][Platform Views]，
你可能要透過 [Android 平台的入門指南文件][Getting Started guide for Android]
中的步驟使用 Material 元件：

For example:

舉個例子：

1. Add the dependency on Android's Material in `<my-app>/android/app/build.gradle`:

   在 `<my-app>/android/app/build.gradle` 檔案中新增
   Android Material 元件依賴：

```groovy
dependencies {
    // ...
    implementation 'com.google.android.material:material:<version>'
    // ...
}
```

To find out the latest version, visit [Google Maven][].

檢視最新的版本，請存取 [Google Maven 儲存庫][Google Maven]。

2. Set the light theme in `<my-app>/android/app/src/main/res/values/styles.xml`:

   在 `<my-app>/android/app/src/main/res/values/styles.xml` 檔案中設定亮色主題：

```diff
-<style name="NormalTheme" parent="@android:style/Theme.Light.NoTitleBar">
+<style name="NormalTheme" parent="Theme.MaterialComponents.Light.NoActionBar">
```

3. Set the dark theme in `<my-app>/android/app/src/main/res/values-night/styles.xml`

```diff
-<style name="NormalTheme" parent="@android:style/Theme.Black.NoTitleBar">
+<style name="NormalTheme" parent="Theme.MaterialComponents.DayNight.NoActionBar">
```

<a id="signing-the-app"></a>
## Sign the app

<a id="signing-the-app"></a>
## 為 app 簽名

To publish on the Play Store, you need to
sign your app with a digital certificate.

為了將你的應用釋出到 Play 商店，
你需要給你的應用進行數字簽名。

Android uses two signing keys: _upload_ and _app signing_.

Android 應用需要兩個簽名：**上傳簽名** 和 **應用簽名**。

* Developers upload an `.aab` or `.apk` file signed with
  an _upload key_ to the Play Store.

  開發者上傳到 Play Store 的 `.aab` 或 `.apk` 需要有上傳簽名。

* The end-users download the `.apk` file signed with an _app signing key_.

  終端使用者下載的 `.apk` 檔案需要有 **應用簽名**。

To create your app signing key, use Play App Signing
as described in the [official Play Store documentation][].

請參考 [Play Store 的官方文件][official Play Store documentation]
來建立你的應用簽名。

To sign your app, use the following instructions.

參考以下步驟對你的應用進行簽名。

### Create an upload keystore

### 建立一個用於上傳的金鑰庫

If you have an existing keystore, skip to the next step.
If not, create one using one of the following methods:

如果你已經有一個金鑰庫了，可以直接跳到下一步，
如果還沒有，需要參考下面的方式建立一個：

1. Follow the [Android Studio key generation steps]({{site.android-dev}}/studio/publish/app-signing#generate-key)

   參考文件 [在 Android Studio 上為你的應用簽名]({{site.android-dev}}/studio/publish/app-signing#sign-apk)。

1. Run the following command at the command line:

   在命令列視窗執行如下的命令：

   On macOS or Linux, use the following command:

   在 macOS 或者 Linux 系統上，執行下面的程式碼：

   ```terminal
   keytool -genkey -v -keystore ~/upload-keystore.jks -keyalg RSA \
           -keysize 2048 -validity 10000 -alias upload
   ```

   On Windows, use the following command in PowerShell:

   在 Windows 系統上，在 PoweShell 內執行以下程式碼：

   ```powershell
   keytool -genkey -v -keystore %userprofile%\upload-keystore.jks ^
           -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 ^
           -alias upload
   ```

   This command stores the `upload-keystore.jks` file in your home
   directory. If you want to store it elsewhere, change
   the argument you pass to the `-keystore` parameter.
   **However, keep the `keystore` file private;
   don't check it into public source control!**

   該命令將會把 `upload-keystore.jks` 檔案儲存在你的主資料夾中。
   如果你想要儲存在其他地方，請透過指定 `-keystore` 傳入引數。
   **注意，請保證這個檔案的私有性，不要將它提交到公共的程式碼管理空間**。

   {{site.alert.note}}

     * The `keytool` command might not be in your path&mdash;it's
       part of Java, which is installed as part of
       Android Studio.  For the concrete path,
       run `flutter doctor -v` and locate the path printed after
       'Java binary at:'. Then use that fully qualified path
       replacing `java` (at the end) with `keytool`.
       If your path includes space-separated names,
       such as `Program Files`, use platform-appropriate
       notation for the names. For example, on Mac/Linux
       use `Program\ Files`, and on Windows use
       `"Program Files"`.

       `keytool` 可能不在我們的系統路徑中。
       它是 Java 的一部分，在安裝 Android Studio 的時候會被一起安裝。
       執行 `flutter doctor -v`，'Java binary at:' 之後打印出來的就是它的路徑，
       然後用 `java` 來替換以上命令中的 `keytool`，並加上 `keytool` 的完整路徑即可。
       如果檔案路徑包含空格，類似 `Program Files` 這樣的，請使用平台允許的命名規則。
       例如，在 Mac/Linux 上使用 `Program\ Files`，而在 Windows 上可以使用
       `"Program Files"`。

      * The `-storetype JKS` tag is only required for Java 9
       or newer. As of the Java 9 release,
       the keystore type defaults to PKS12.

       只有 Java 9 或更高版本才需要 `-storetype JKS` 標籤。
       從 Java 9 版本開始，keystore 型別預設為 PKS12。

   {{site.alert.end}}

### Reference the keystore from the app

### 從 app 中參考金鑰庫

Create a file named `[project]/android/key.properties`
that contains a reference to your keystore.
Don't include the angle brackets (`< >`).
They indicate that the text serves as a placeholder for your values.

建立一個名為 `[project]/android/key.properties` 的檔案，
它包含了金鑰庫位置的定義。
在替換內容時請去除 `< >` 括號：

```properties
storePassword=<password-from-previous-step>
keyPassword=<password-from-previous-step>
keyAlias=upload
storeFile=<keystore-file-location>
```

The `storeFile` might be located at
`/Users/<user name>/upload-keystore.jks` on macOS
or `C:\\Users\\<user name>\\upload-keystore.jks` on Windows.

`storeFile` 金鑰路徑在 macOS 上類似於 `/Users/<user name>/upload-keystore.jks`，
在 Windows 上類似於 `C:\\Users\\<user name>\\upload-keystore.jks`。

{{site.alert.warning}}

  Keep the `key.properties` file private;
  don't check it into public source control.

（再次）請保證這個檔案的私有性，不要將它提交到公共的程式碼管理空間。

{{site.alert.end}}

### Configure signing in gradle

### 在 gradle 中配置簽名

Configure gradle to use your upload key when building your app in release mode
by editing the `[project]/android/app/build.gradle` file.

在以 release 模式下建構你的應用時，修改 `[project]/android/app/build.gradle`
檔案，以透過 gradle 配置你的上傳金鑰。


<ol markdown="1">
<li markdown="1"> Add the keystore information from your properties file before the `android` block:

 在 `android` 程式碼塊之前將你 properties 檔案的金鑰庫資訊新增進去：

```
   def keystoreProperties = new Properties()
   def keystorePropertiesFile = rootProject.file('key.properties')
   if (keystorePropertiesFile.exists()) {
       keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
   }

   android {
         ...
   }
```
   
   Load the `key.properties` file into the `keystoreProperties` object.

   將 `key.properties` 檔案載入到 `keystoreProperties` 物件中。

</li>

<li markdown="1"> Find the `buildTypes` block:

 找到 `buildTypes` 程式碼塊：

```
   buildTypes {
       release {
           // TODO: Add your own signing config for the release build.
           // Signing with the debug keys for now,
           // so `flutter run --release` works.
           signingConfig signingConfigs.debug
       }
   }
```

   And replace it with the following signing configuration info:

   將其替換為我們的配置內容：

```
   signingConfigs {
       release {
           keyAlias keystoreProperties['keyAlias']
           keyPassword keystoreProperties['keyPassword']
           storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
           storePassword keystoreProperties['storePassword']
       }
   }
   buildTypes {
       release {
           signingConfig signingConfigs.release
       }
   }
```

</li>
</ol>

Release builds of your app will now be signed automatically.

現在我們 app 的釋出版本就會被自動簽名了。

{{site.alert.note}}
  You might need to run `flutter clean` after changing the gradle file.
  This prevents cached builds from affecting the signing process.

  當你更改 gradle 檔案後，也許需要執行一下 `flutter clean`。
  這將防止快取的版本影響簽名過程。

{{site.alert.end}}

For more information on signing your app, check out
[Sign your app][] on developer.android.com.

有關應用簽名的更多資訊，請檢視 developer.android.com 的
[為您的應用設定簽名][Sign your app]。

## Shrinking your code with R8

## 使用 R8 壓縮你的程式碼

[R8][] is the new code shrinker from Google, and it's enabled by default
when you build a release APK or AAB. To disable R8, pass the `--no-shrink`
flag to `flutter build apk` or `flutter build appbundle`.

[R8][] 是谷歌推出的最新程式碼壓縮器，
當你打包 release 版本的 APK 或者 AAB 時會預設開啟。
要關閉 R8，請執行 `flutter build apk` 或
`flutter build appbundle` 時加上 `--no-shrink` 引數。

{{site.alert.note}}

  Obfuscation and minification can considerably extend compile time
  of the Android application.

  混淆和壓縮會大大地延長 Android 應用的編譯時間。

{{site.alert.end}}

## Enabling multidex support

## 啟用 multidex 支援

When writing large apps or making use of large plugins,
you might encounter Android's dex limit of 64k methods
when targeting a minimum API of 20 or below.
This might also be encountered when running debug versions of your app
using `flutter run` that does not have shrinking enabled.

當你在編寫較大的應用或使用體量較大的外掛時，
你可能會在最低的 API 目標版本低於 20 時，
遇到 Android 的 dex 的 64k 方法數限制問題。
當 `flutter run` 以除錯模式執行應用時，
由於縮減機制沒有執行，該問題也有可能發生。

Flutter tool supports easily enabling multidex. The simplest way is to
opt into multidex support when prompted. The tool detects multidex build errors
and asks before making changes to your Android project.
Opting in allows Flutter to automatically depend on
`androidx.multidex:multidex` and use a generated
`FlutterMultiDexApplication` as the project's application.

When you try to build and run your app with the **Run** and **Debug**
options in your IDE, your build might fail with the following message:

<img src='/assets/images/docs/deployment/android/ide-build-failure-multidex.png' width="100%" alt='screenshot of build failure because Multidex support is required'>

To enable multidex from the command line,
run `flutter run --debug` and select an Android device:

<img src='/assets/images/docs/deployment/android/cli-select-device.png' width="100%" alt='screenshot of selecting an Android device'>

When prompted, enter `y`.
The Flutter tool enables multidex support and retries the build:

<img src='/assets/images/docs/deployment/android/cli-multidex-added-build.png' width="100%" alt='screenshot of a successful build after adding multidex'>

Flutter 工具支援以便捷的方式啟用 multidex 支援。
當工具提示你需要支援時，跟隨工具的指示進行調整，是最快的方式。
Flutter 工具會檢測 multidex 的建構錯誤，並提示你是否要更改 Android 專案。
在同意的情況下，專案會自動依賴 `androidx.multidex:multidex`，
並且讓專案的 `Application` 繼承於 `FlutterMultiDexApplication`。

{{site.alert.note}}

  Multidex support is natively included when targeting
  Android SDK 21 or later. However, we don't recommend
  targeting API 21+ purely to resolve the multidex issue
  as this might inadvertently exclude users running older devices.

  在設定了目標 Android SDK 版本為 21 和以上時，其已經包含了 Multidex 的原生支援。
  不過，我們不建議為了解決對 Multidex 的支援而將目標 SDK 設定為 21+，
  這可能會無意中忽略掉那些執行著舊裝置的使用者。

{{site.alert.end}}

You might also choose to manually support multidex by following Android's guides
and modifying your project's Android directory configuration.
A [multidex keep file][multidex-keep] must be specified to include:

你也可以根據 Android 的指南，手動配置你的 Android 專案以支援 multidex。
請務必指定 [multidex keep 檔案][multidex-keep] 以包含以下內容：

```
io/flutter/embedding/engine/loader/FlutterLoader.class
io/flutter/util/PathUtils.class
```

Also, include any other classes used in app startup.
For more detailed guidance on adding multidex support manually,
check out the official [Android documentation][multidex-docs].

同時也要包含所有在應用啟動時載入的其他類別。
參考 [multidex 文件][multidex-docs]
瞭解更詳細的手動適配指南。

## Reviewing the app manifest

## 檢查 app manifest 檔案

Review the default [App Manifest][manifest] file, `AndroidManifest.xml`.
This file is located in `[project]/android/app/src/main`.
Verify the following values:

檢查位於 `[project]/android/app/src/main` 的預設 [App Manifest][manifest]
檔案 `AndroidManifest.xml`，並確認各個值都設定正確，特別是：

`application`
<br> Edit the `android:label` in the
     [`application`][applicationtag] tag to reflect
     the final name of the app.

`application`
<br> 編輯 [`application`][applicationtag]
     標籤中的 `android:label` 來設定 app 的最終名字。

`uses-permission`
<br> Add the `android.permission.INTERNET`
  [permission][permissiontag] if your application code needs Internet
  access. The standard template doesn't include this tag but allows
  Internet access during development to enable communication between
  Flutter tools and a running app.

`uses-permission`
<br>
  如果你的程式碼需要網際網路互動，請加入 `android.permission.INTERNET`
  [許可權標籤][permissiontag]。
  標準開發模版裡並未加入這個許可權（但是 Flutter debug 模版加入了這個許可權），
  加入這個許可權是為了允許 Flutter 工具和正在執行的 app 之間的通訊。

## Reviewing the Gradle build configuration

Review the default [Gradle build file][gradlebuild]
(`build.gradle`, located in `[project]/android/app`),
to verify that the values are correct.

檢查位於 `[project]/android/app` 的
預設 [Gradle 建構檔案][gradlebuild] (`build.gradle`)
並確認各個值都設定正確：

#### Under the `defaultConfig` block

#### 在 `defaultConfig` 配置中

`applicationId`
<br> Specify the final, unique [application ID][].

`applicationId`
<br> 指定唯一的 [應用 ID][application ID]。

`minSdkVersion`
<br> Specify the [minimum API level][] on which you designed the app to run.
  Defaults to `flutter.minSdkVersion`.

`minSdkVersion`
<br> 指定應用適配的 [最低 SDK API 版本][minimum API level]。
  預設為 `flutter.minSdkVersion`。

`targetSdkVersion`
<br> Specify the target API level on which on which you designed the app to run.
  Defaults to `flutter.targetSdkVersion`.

`targetSdkVersion`
<br> 指定應用適配的目標 SDK 版本。
  預設為 `flutter.targetSdkVersion`。

`versionCode`
<br> A positive integer used as an [internal version number][].
  This number is used only to determine whether one version is more recent
  than another, with higher numbers indicating more recent versions.
  This version isn't shown to users.

`versionCode`
<br> 用於 [內部版本號][internal version number] 的正整數。
  該數字僅用於比較兩個版本間數字較大的為更新版本。
  該版本不會對使用者展示。

`versionName`
<br> A string used as the version number shown to users.
  This setting can be specified as a raw string or as
  a reference to a string resource.

`versionName`
<br> 向用戶展示的版本號。
  該欄位必須設定為原始字串或字串資源的參考。

`buildToolsVersion`
: The Gradle plugin specifies the default version of the
  build tools that your project uses.
  You can use this option to specify a different version of the build tools.

`buildToolsVersion`
<br> 指定你的專案使用的建構工具的版本。
  你也可以手動指定不同的建構工具的版本。

#### Under the `android` block

#### 在 `android` 配置中

`compileSdkVersion`
<br> Specify the API level Gradle should use to compile your app.
  Defaults to `flutter.compileSdkVersion`.

`compileSdkVersion`
<br> 指定 Gradle 用於編譯應用的 API 版本。
  預設為 `flutter.compileSdkVersion`。

For more information, check out the module-level build
section in the [Gradle build file][gradlebuild].

更多資訊可以參考 [Gradle 建構檔案][gradlebuild]
文件中模組級建構的部分。

## Building the app for release

## 建構生產版本應用

You have two possible release formats when publishing to
the Play Store.

當要釋出到 Play Store 時，你有兩種釋出方式的選擇：

* App bundle (preferred)

  App bundle (推薦）

* APK

{{site.alert.note}}

  The Google Play Store prefers the app bundle format.
  For more information, check out
  [About Android App Bundles][bundle].

  Google Play 更推薦使用 app bundle 格式的應用，
  更多資訊可以參考 [Android App Bundle][bundle]。
{{site.alert.end}}

### Build an app bundle

### 建構一個 app bundle

This section describes how to build a release app bundle.
If you completed the signing steps,
the app bundle will be signed.
At this point, you might consider [obfuscating your Dart code][]
to make it more difficult to reverse engineer. Obfuscating
your code involves adding a couple flags to your build command,
and maintaining additional files to de-obfuscate stack traces.

這個部分描述瞭如何建構一個釋出的 app bundle。
如果在前面的部分已經完成了簽名步驟，釋出的 bundle 會被簽名。
這時你也許想要 [混淆你的 Dart 程式碼][obfuscating your Dart code] 以加大反編譯難度。
混淆你的程式碼需要在 build 的時候新增一些標誌，並維護其他檔案以消除反編譯的堆疊追蹤。

From the command line:

使用如下命令：

1. Enter `cd [project]`<br>

   執行 `cd [project]`。

1. Run `flutter build appbundle`<br>
   (Running `flutter build` defaults to a release build.)

   執行 `flutter build appbundle`。
   (執行 `flutter build` 預設建構一個釋出版本。)

The release bundle for your app is created at
`[project]/build/app/outputs/bundle/release/app.aab`.

你的應用的 release bundle 會被建立到
`<app dir>/build/app/outputs/bundle/release/app.aab`.

By default, the app bundle contains your Dart code and the Flutter
runtime compiled for [armeabi-v7a][] (ARM 32-bit), [arm64-v8a][]
(ARM 64-bit), and [x86-64][] (x86 64-bit).

此 app bundle 會預設地包含為
[armeabi-v7a][] (ARM 32-bit)、[arm64-v8a][] (ARM 64-bit)
以及 [x86-64][] (x86 64-bit) 編譯的 Dart 和 Fluter 執行時程式碼。

### Test the app bundle

### 測試 app bundle

An app bundle can be tested in multiple ways.
This section describes two.

一個 app bundle 可以用多種方法測試，這裡介紹兩種。

#### Offline using the bundle tool

#### 離線使用 bundle tool

1. If you haven't done so already, download `bundletool` from the
   [GitHub repository][].

   如果你還沒準備好，可以從 [GitHub 儲存庫][GitHub repository] 下載 `bundletool`。

2. [Generate a set of APKs][apk-set] from your app bundle.

   從你的 app bundle [產生 APKs][apk-set]。

3. [Deploy the APKs][apk-deploy] to connected devices.

   [將這 APKs 部署到][apk-deploy] 已連線的裝置。

#### Online using Google Play

#### 線上使用 Google Play

1. Upload your bundle to Google Play to test it.
   You can use the internal test track,
   or the alpha or beta channels to test the bundle before
   releasing it in production.

   上傳你的 bundle 到 Google Play 去測試它。
   或者在正式釋出之前用 alpha 或 beta 頻道去測試。

2. Follow [these steps to upload your bundle][upload-bundle]
   to the Play Store.

   按照 [這些步驟把你的 bundle][upload-bundle] 上傳到 Play Store。

### Build an APK

### 建構一個 APK

Although app bundles are preferred over APKs, there are stores
that don't yet support app bundles. In this case, build a release
APK for each target ABI (Application Binary Interface).

雖然 app bundle 比 APKs 更被推薦使用，
但是有一些 Store 目前還不支援 app bundle方式。
這種情況下，要為各種目標
ABI (Application Binary Interface) 分別建構釋出的 APK 檔案。

If you completed the signing steps, the APK will be signed.
At this point, you might consider [obfuscating your Dart code][]
to make it more difficult to reverse engineer. Obfuscating
your code involves adding a couple flags to your build command.

如果你完成簽名步驟，APK 就被簽名了。
這時你也許想要 [混淆你的 Dart 程式碼][obfuscating your Dart code] 以加大反編譯難度。
混淆你的程式碼需要在建構時新增一些引數。

From the command line:

使用如下命令：

1. Enter `cd [project]`.

   輸入命令 `cd [project]`。

1. Run `flutter build apk --split-per-abi`.

   (The `flutter build` command defaults to `--release`.)

   執行 `flutter build apk --split-per-abi`<br>
   （`flutter build` 預設帶有 `--release` 引數。）

This command results in three APK files:

這個命令會產生如下三個 APK 檔案

* `[project]/build/app/outputs/apk/release/app-armeabi-v7a-release.apk`
* `[project]/build/app/outputs/apk/release/app-arm64-v8a-release.apk`
* `[project]/build/app/outputs/apk/release/app-x86_64-release.apk`

Removing the `--split-per-abi` flag results in a fat APK that contains
your code compiled for _all_ the target ABIs. Such APKs are larger in
size than their split counterparts, causing the user to download
native binaries that are not applicable to their device's architecture.

如果移除 `--split-per-abi` 將會產生一個包含 **所有** 目標 ABI 的 fat APK 檔案。
這種 APK 檔案將會在比單獨建構的 APK 檔案尺寸要大，
會導致使用者下載一些不適用於其裝置架構的二進位制檔案。

### Install an APK on a device

### 在裝置上安裝 APK 檔案

Follow these steps to install the APK on a connected Android device.

按照如下這些步驟，將前一步中構建出來的 APK 安裝到 Android 裝置上。

From the command line:

使用如下命令：

1. Connect your Android device to your computer with a USB cable.

   用 USB 線將 Android 裝置連線到電腦上；

1. Enter `cd [project]`.

   輸入命令 `cd [project]`；

1. Run `flutter install`.

   執行 `flutter install`。

## Publishing to the Google Play Store

## 釋出到 Google Play Store

For detailed instructions on publishing your app to the Google Play Store,
check out the [Google Play launch][play] documentation.

要了解如何釋出一個 app 到 Google Play Store，
可以參考 [Google Play 釋出文件][play]。

## Updating the app's version number

## 更新應用版本號

The default version number of the app is `1.0.0`.
To update it, navigate to the `pubspec.yaml` file
and update the following line:

每個應用預設的初始版本號是 `1.0.0`。若要更新它，
請轉到 `pubspec.yaml` 檔案並更新以下內容：

`version: 1.0.0+1`

The version number is three numbers separated by dots,
such as `1.0.0` in the example above, followed by an optional
build number such as `1` in the example above, separated by a `+`.

版本號由三個點分隔的數字組成，例如上面範例中的 `1.0.0`。然後是可選的
建構號，例如上面範例中的 `1`，以 `+` 分隔。

Both the version and the build number can be overridden in Flutter's
build by specifying `--build-name` and `--build-number`, respectively.

版本號與建構號都可以在 Flutter 打包時分別使用
`--build-name` 和 `--build-number` 重新指定。

In Android, `build-name` is used as `versionName` while
`build-number` used as `versionCode`. For more information,
check out [Version your app][] in the Android documentation.

在 Android 中，`build-number` 被用作 `versionCode`，
`build-name` 將作為 `versionName` 使用。
更多資訊請參考 Android 文件中的 [為你的應用新增版本][Version your app]。

When you rebuild the app for Android, any updates in the version number
from the pubspec file will update the `versionName` and `versionCode` 
in the `local.properties` file.

當重新建構 Android 應用後，任何在 pubspec 檔案所做的版本號更新，
都將會更新 `local.properties` 檔案中的
`versionName` 和 `versionCode`。

## Android release FAQ

## Android 釋出常見問題

Here are some commonly asked questions about deployment for
Android apps.

這裡是一些關於 Android 應用釋出的常見問題。

### When should I build app bundles versus APKs?

### 我應該什麼時候建構 app bundles 而不是 APKs?

The Google Play Store recommends that you deploy app bundles
over APKs because they allow a more efficient delivery of the
application to your users. However, if you're distributing
your application by means other than the Play Store,
an APK might be your only option.

Google Play Store 相對於 APKs 更建議你釋出 app bundles，
因為那樣應用會更有效率地交付給你的使用者。
但是，如果你想將應用釋出到其他的應用商店，APK可能是唯一選項。

### What is a fat APK?

### 什麼是 fat APK?

A [fat APK][] is a single APK that contains binaries for multiple
ABIs embedded within it. This has the benefit that the single APK
runs on multiple architectures and thus has wider compatibility,
but it has the drawback that its file size is much larger,
causing users to download and store more bytes when installing
your application. When building APKs instead of app bundles,
it is strongly recommended to build split APKs,
as described in [build an APK](#build-an-apk) using the
`--split-per-abi` flag.

一個 [fat APK][] 是一個包含了支援多個 ABI 架構的 APK 檔案。
這樣做的好處是單個 APK 可以執行在多個架構上，因此
具有更廣泛的相容性。但同時缺點就是檔案體積會比較大，
導致使用者在安裝你的應用時會下載和儲存更多的位元組。
當建構 APK 而不是 app bundles 時強烈建議分開建構 APK，
如 [build an APK](#build-an-apk) 所描述的那樣，
使用 `--split-per-abi` 指令。

### What are the supported target architectures?

### 哪些目標架構是被支援的?

When building your application in release mode,
Flutter apps can be compiled for [armeabi-v7a][] (ARM 32-bit),
[arm64-v8a][] (ARM 64-bit), and [x86-64][] (x86 64-bit).
Flutter supports building for x86 Android through ARM emulation.

當使用 release 模式建構你的應用時,
Flutter app 可以基於 [armeabi-v7a][] (ARM 32 位)、
[arm64-v8a][] (ARM 64 位) 以及 [x86-64][] (x86 64 位) 被編譯。
Flutter 目前支援透過 ARM 模擬 x86 Android。

### How do I sign the app bundle created by `flutter build appbundle`?

### 如何為一個使用 `flutter build appbundle` 建立的 app bundle 簽名？

See [Signing the app](#signing-the-app).

請檢視 [建立一個金鑰庫](#signing-the-app)。

### How do I build a release from within Android Studio?

### 如何使用 Android Studio 建構一個釋出？

In Android Studio, open the existing `android/`
folder under your app's folder. Then,
select **build.gradle (Module: app)** in the project panel:

在Android Studio中, 開啟你的 app 資料夾下的 `android/`
資料夾. 然後在專案面板中選擇 **build.gradle (Module: app)** :

<img src='/assets/images/docs/deployment/android/gradle-script-menu.png' width="100%" alt='screenshot of gradle build script menu'>

Next, select the build variant. Click **Build > Select Build Variant**
in the main menu. Select any of the variants in the **Build Variants**
panel (debug is the default):

接下來，選擇建構變體。在主選單中點選 **Build > Select Build Variant**。
從 **Build Variants** 面板中選擇任意一個變體（預設是 debug）。

<img src='/assets/images/docs/deployment/android/build-variant-menu.png' width="100%" alt='screenshot of build variant menu'>

The resulting app bundle or APK files are located in
`build/app/outputs` within your app's folder.

產生的 app bundle 或 APK 檔案會在你的 app 所在資料夾下的 `build/app/outputs` 資料夾下。

{% comment %}

### Are there any special considerations with add-to-app?

### 在混合應用中是否有特殊考慮之處？

{% endcomment %}


[apk-deploy]: {{site.android-dev}}/studio/command-line/bundletool#deploy_with_bundletool
[apk-set]: {{site.android-dev}}/studio/command-line/bundletool#generate_apks
[application ID]: {{site.android-dev}}/studio/build/application-id
[applicationtag]: {{site.android-dev}}/guide/topics/manifest/application-element
[arm64-v8a]: {{site.android-dev}}/ndk/guides/abis#arm64-v8a
[armeabi-v7a]: {{site.android-dev}}/ndk/guides/abis#v7a
[bundle]: {{site.android-dev}}/guide/app-bundle
[configuration qualifiers]: {{site.android-dev}}/guide/topics/resources/providing-resources#AlternativeResources
[fat APK]: https://en.wikipedia.org/wiki/Fat_binary
[flutter_launcher_icons]: {{site.pub}}/packages/flutter_launcher_icons
[Getting Started guide for Android]: {{site.material}}/develop/android/mdc-android
[GitHub repository]: {{site.github}}/google/bundletool/releases/latest
[Google Maven]: https://maven.google.com/web/index.html#com.google.android.material:material
[gradlebuild]: {{site.android-dev}}/studio/build/#module-level
[internal version number]: {{site.android-dev}}/studio/publish/versioning
[launchericons]: {{site.material}}/styles/icons
[manifest]: {{site.android-dev}}/guide/topics/manifest/manifest-intro
[minimum API level]: {{site.android-dev}}/studio/publish/versioning#minsdk
[multidex-docs]: {{site.android-dev}}/studio/build/multidex
[multidex-keep]: {{site.android-dev}}/studio/build/multidex#keep
[obfuscating your Dart code]: {{site.url}}/deployment/obfuscate
[official Play Store documentation]: https://support.google.com/googleplay/android-developer/answer/7384423?hl=en
[official Play Store documentation Zh Lang]: https://support.google.com/googleplay/android-developer/answer/7384423?hl=zh_CN
[permissiontag]: {{site.android-dev}}/guide/topics/manifest/uses-permission-element
[Platform Views]: {{site.url}}/platform-integration/android/platform-views
[play]: {{site.android-dev}}/distribute
[R8]: {{site.android-dev}}/studio/build/shrink-code
[Sign your app]: https://developer.android.com/studio/publish/app-signing.html#generate-key
[upload-bundle]: {{site.android-dev}}/studio/publish/upload-bundle
[Version your app]: {{site.android-dev}}/studio/publish/versioning
[x86-64]: {{site.android-dev}}/ndk/guides/abis#86-64
