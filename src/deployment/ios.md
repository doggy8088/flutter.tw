---
title: Build and release an iOS app
title: 建構和釋出為 iOS 應用
short-title: iOS
description: How to release a Flutter app to the App Store.
description: 如何在 App Store 上釋出一個 Flutter 應用。
short-title: iOS
tags: 釋出, iOS
keywords: 上傳App Store,釋出Flutter應用
---

This guide provides a step-by-step walkthrough of releasing a
Flutter app to the [App Store][appstore] and [TestFlight][].

這個課程將為你提供關於如何將 Flutter App 釋出到
[App Store][appstore_cn] 和 [TestFlight][testflight_cn] 的說明。

## Preliminaries

## 預先準備

Xcode is required to build and release your app. You
must use a device running macOS to follow this guide.

建構和釋出一個 macOS 應用需要使用 Xcode，
你必須要有一個執行著 macOS 系統的裝置來學習本指南文件。

Before beginning the process of releasing your app,
ensure that it meets
Apple's [App Review Guidelines][appreview].

在開始釋出你的 app 的處理序之前，
確保你已經看過了 Apple 的 [App Store 稽核指南][appreview_cn]。

In order to publish your app to the App Store,
you must first enroll in the
[Apple Developer Program][devprogram].
You can read more about the various
membership options in Apple's
[Choosing a Membership][devprogram_membership] guide.

想要釋出你的 app 到 App Store，
你需要註冊 [Apple Developer Program][devprogram_cn]。
你可以在蘋果的 [選擇會員資格(開發者型別)][devprogram_membership_cn]
中檢視到關於多種不同會員型別的選擇。

## Register your app on App Store Connect

## 在 App Store Connect 上註冊你的 App

Manage your app's life cycle on
[App Store Connect][appstoreconnect] (formerly iTunes Connect).
You define your app name and description, add screenshots,
set pricing, and manage releases to the App Store and TestFlight.

[App Store Connect][appstoreconnect_cn]
（曾經的 iTunes Connet）是你將會管理應用生命週期的地方。
你將會定義應用的名稱和描述以及截圖，設定價格，
並管理釋出到 App Store 和 Testflight。

Registering your app involves two steps: registering a unique
Bundle ID, and creating an application record on App Store Connect.

註冊你的 app 需要兩步：登記唯一的套裝 ID（Bundle ID），
並在你的 App Store Connect 中建立一個 app。

For a detailed overview of App Store Connect, see the
[App Store Connect][appstoreconnect_guide] guide.

關於更多 App Store Connect 的細節，
檢視 [App Store Connect][appstoreconnect_guide_cn] 指南。

### Register a Bundle ID

### 登記套裝 ID

Every iOS application is associated with a Bundle ID,
a unique identifier registered with Apple.
To register a Bundle ID for your app, follow these steps:

每一個 iOS 應用都與一個在 Apple 登記的唯一的套裝 ID 關聯。
要為你的應用登記一個套裝 ID，請參考下面的步驟：

1. Open the [App IDs][devportal_appids] page of your developer account.

   在你的開發者帳號頁面開啟 [App IDs][devportal_appids] 頁面。

1. Click **+** to create a new Bundle ID.

   點選 **+** 來建立一個新的套裝 ID。

1. Enter an app name, select **Explicit App ID**, and enter an ID.

   輸入一個 App 名稱，選擇 **Explicit App ID**，然後輸入一個 ID。

1. Select the services your app uses, then click **Continue**.

   選擇你的 App 將要使用的服務，然後點選 **繼續**

1. On the next page, confirm the details and click **Register**
   to register your Bundle ID.

   在下一頁，確認細節並點選 **註冊** 來註冊你的 Bundle ID。

### Create an application record on App Store Connect

### 在 App Store Connect 建立一個應用記錄

Register your app on App Store Connect:

在 App Store Connect 中註冊你的應用：

Next, you'll register your app on App Store Connect:

接下來，你需要在 App Store Connect 註冊你的應用：

1. Open [App Store Connect][appstoreconnect_login] in your browser.

   在你的瀏覽器裡開啟 [App Store Connect][appstoreconnect_login]。

1. On the App Store Connect landing page, click **My Apps**.

   在 App Store Connect 的落地頁，點選 **My Apps**。

1. Click **+** in the top-left corner of the My Apps page,
   then select **New App**.

   在我的 app 頁面的頂部左側 ，點選 **+** ，然後選擇 **New App**。

1. Fill in your app details in the form that appears.
   In the Platforms section, ensure that iOS is checked.
   Since Flutter does not currently support tvOS,
   leave that checkbox unchecked. Click **Create**.

   在出現的表單中填寫你的 app 細節。在平台部分，確保 iOS 被選中。
   由於 Flutter 暫時不支援 tvOS，保持該選項為未選。點選 **Create**。

1. Navigate to the application details for your app and select
   **App Information** from the sidebar.

   跳轉到你的應用詳情，然後從側邊欄選擇 **App Information** 。

1. In the General Information section, select the Bundle ID
   you registered in the preceding step.

   在基礎資訊部分，選擇你在前一步註冊的套裝 ID。

For a detailed overview, see
[Add an app to your account][appstoreconnect_guide_register].

想要獲取更多資訊，可以看這個幫助頁面 [新增 App 至您的帳戶][appstoreconnect_guide_register_cn]。

## Review Xcode project settings

## 檢查 Xcode 專案設定

This step covers reviewing the most important settings
in the Xcode workspace.
For detailed procedures and descriptions, see
[Prepare for app distribution][distributionguide_config].

在這一步，你需要在 Xcode 工作空間檢查絕大多數重要設定。
關於更多的步驟和描述，檢視 [為 App 分發做準備][distributionguide_config]。

Navigate to your target's settings in Xcode:

在 Xcode 中跳轉到你的目標設定：

1. Open the default Xcode workspace in your project by running
   `open ios/Runner.xcworkspace` in a terminal window from your
   Flutter project directory.

   在 Flutter 工程目錄下執行命令 `open ios/Runner.xcworkspace`
   開啟預設的 Xcode workspace。

1. To view your app's settings, select the **Runner** target in the
   Xcode navigator.

   想要看你的 app 設定，在 Xcode 的專案導航欄中選擇 **Runner**。

Verify the most important settings.

接下來，你需要驗證最重要的配置：

In the **Identity** section of the **General** tab:

選擇 **General** 標籤頁，在 **Identity** 部分：

`Display Name`
<br> The display name of your app.

`Display Name`
<br> 應用的名字。

`Bundle Identifier`
<br> The App ID you registered on App Store Connect.

`Bundle Identifier`
<br> 在 App Store Connect 註冊的 App ID。

In the **Signing & Capabilities** tab:

在 **Signing & Capabilities** 部分：

`Automatically manage signing`
<br> Whether Xcode should automatically manage app signing
  and provisioning.  This is set `true` by default, which should
  be sufficient for most apps. For more complex scenarios,
  see the [Code Signing Guide][codesigning_guide].

`Automatically manage signing`
<br> 是否需要 Xcode 自動管理 app 簽名和設定。
這個預設被設定為 `true` ，對於絕大多數 App 來說都是適用的。
對於更復雜的場景，檢視 [程式碼簽名指南][codesigning_guide]。

`Team`
<br> Select the team associated with your registered Apple Developer
  account. If required, select **Add Account...**,
  then update this setting.

`Team`
<br> 選擇關聯到你註冊的 Apple 開發者賬戶的團隊。
如果需要，選擇 **Add Account...**, 然後更新選項。

In the **Deployment** section of the **Build Settings** tab:

在 **Build Settings** 標籤頁的 **Deployment** 部分：

`iOS Deployment Target`
<br> The minimum iOS version that your app supports.
  Flutter supports iOS 11 and later. If your app or plugins
  include Objective-C or Swift code that makes use of APIs newer
  than iOS 11, update this setting to the highest required version.

`iOS Deployment Target`
<br> 設定你的應用可以支援到的最低的 iOS 版本。
Flutter 支援 iOS 11 及其之後的版本，如果你的應用包含了 iOS 11 不支援的
Objective-C 或 Swift 程式碼，請將這裡一併設定為相應所需的最高版本。

The **General** tab of your project settings should resemble
the following:

你專案的 **General** 標籤頁應該看起來像是這樣的：

![Xcode Project Settings]({{site.url}}/assets/images/docs/releaseguide/xcode_settings.png){:width="100%"}

For a detailed overview of app signing, see
[Create, export, and delete signing certificates][appsigning].

更多關於 App 簽名新的介紹，檢視文件
[建立, 匯出, 和刪除簽名證書][appsigning]。

## Updating the app's deployment version

## 更新應用的開發版本

If you changed `Deployment Target` in your Xcode project,
open `ios/Flutter/AppframeworkInfo.plist` in your Flutter app
and update the `MinimumOSVersion` value to match.

如果你在 Xcode 工程裡更改了 `Deployment Target`，
你需要開啟 Flutter app 的 `ios/Flutter/AppframeworkInfo.plist`
檔案並修改 `MinimumOSVersion` 值與之匹配。

## Add an app icon

## 新增應用圖示

When a new Flutter app is created, a placeholder icon set is created.
This step covers replacing these placeholder icons with your
app's icons:

當你建立一個新的 Flutter 應用時，則會建立一個預設的圖示。
在這一步，你將使用你自己的圖示替換佔位圖示：

1. Review the [iOS App Icon][appicon] guidelines.

   回顧 [iOS 的 App Icon][appicon] 指南。

1. In the Xcode project navigator, select `Assets.xcassets` in the
   `Runner` folder. Update the placeholder icons with your own app icons.

   在 Xcode 專案導航欄，選擇 `Runner` 目錄中的 `Assets.xcassets`，
   更新佔位圖示為你自己的 app 的圖示。

1. Verify the icon has been replaced by running your app using
   `flutter run`.

   透過執行 `flutter run` 來驗證你的圖示是否已經被替換。

## Create a build archive and upload to App Store Connect

## 建立一個建構歸檔，並上傳到 App Store Connect

During development, you've been building, debugging, and testing
with _debug_ builds. When you're ready to ship your app to users
on the App Store or TestFlight, you need to prepare a _release_ build.

在開發過程中，你將會使用 **debug** 模式來完成建構、除錯並測試。
當你準備好透過 App Store 或 TestFlight 交付你的 app 給使用者時，
你需要準備一個 **release** 建構。

### Update the app's build and version numbers

### 更新應用的建構編號和版本號

The default version number of the app is `1.0.0`.
To update it, navigate to the `pubspec.yaml` file
and update the following line:

預設應用的版本號是 `1.0.0`，如果需要更新這個版本號，
到 `pubspec.yaml` 檔案中更新下面這一行：

```yaml
version: 1.0.0+1
```
The version number is three numbers separated by dots,
such as `1.0.0` in the example above, followed by an optional
build number such as `1` in the example above, separated by a `+`.

版本號是由三個數字並用半形句號 (點) 隔開的，比如上面顯示的 `1.0.0`。
後面是一個可選的建構編號，比如這個例子中使用 `+` 隔開的數字 `1`。

Both the version and the build number can be overridden in
`flutter build ipa` by specifying `--build-name` and `--build-number`,
respectively.

建構名稱和建構編號都可以透過在執行命令 `flutter build ipa`
的時候透過 `--build-name` 和 `--build-number` 覆蓋設定。

In iOS, `build-name` uses `CFBundleShortVersionString`
while `build-number` uses `CFBundleVersion`.
Read more about iOS versioning at [Core Foundation Keys][]
on the Apple Developer's site.

在 iOS 中，`build-name` 對應 `CFBundleShortVersionString`、
`build-number` 對應著 `CFBundleVersion`。
瞭解更多關於 iOS 中的版本資訊，請在 Apple 開發者文件網站檢視
[Core Foundation Keys][] 文件。

You may also override the `pubspec.yaml` build name and number in Xcode:

在 Xcode 中這樣設定，也可以覆蓋 `pubspec.yaml` 中的建構名稱和建構編號：

1. Open `Runner.xcworkspace` in your app's `ios` folder.

   在 `ios` 資料夾中開啟 `Runner.xcworkspace`。

1. Select **Runner** in the Xcode project navigator, then select the
   **Runner** target in the settings view sidebar.

   在 Xcode 專案導航欄中選擇 **Runner**，
   然後在設定介面側邊欄選擇 **Runner** 目標。

1. In the Identity section, update the **Version** to the user-facing
   version number you wish to publish.

   在 Identity 部分，更新 **Version** 為你想要釋出的使用者可見的版本號。

1. In the Identity section, update the **Build** identifier to a unique
   build number used to track this build on App Store Connect.
   Each upload requires a unique build number.

   在 Identity 部分，更新 **Build** 標示為一個唯一的 Build 數字，
   用來在 App Store Connect 上追蹤，每一個上傳都需要一個獨立的 Build 數字。

### Create an app bundle

### 建立一個應用套裝 (app bundle)

Run `flutter build ipa` to produce an Xcode build archive (`.xcarchive` file)
in your project's `build/ios/archive/` directory and an App Store app
bundle (`.ipa` file) in `build/ios/ipa`.

執行命令列 `flutter build ipa` 之後會在 `build/ios/archive` 資料夾下產生一個
Xcode 建構歸檔（`.xcarchive` 文件），在 `build/ios/ipa` 資料夾下會產生一個
App Store 銷售套裝檔案（`.ipa` 檔案）。

Consider adding the `--obfuscate` and `--split-debug-info` flags to
[obfuscate your Dart code][] to make it more difficult
to reverse engineer.

可以考慮新增 `--obfuscate` 和 `--split-debug-info` 命令列標記來
[混淆你的 Dart 程式碼][obfuscate your Dart code]，
使應用更難被逆向工程解析。

If you are not distributing to the App Store, you can optionally
choose a different [export method][app_bundle_export_method] by
adding the option `--export-method ad-hoc`,
`--export-method development` or `--export-method enterprise`.

你可以使用不同的 [應用匯出方法][app_bundle_export_method]
而非只能輸出用於 App Store 釋出的應用，可用的命令列引數有
`--export-method ad-hoc`、`--export-method development`
和 `--export-method enterprise`。

{{site.alert.note}}

  On versions of Flutter where `flutter build ipa --export-method` is unavailable,
  open `build/ios/archive/MyApp.xcarchive` and follow the instructions below
  to validate and distribute the app from Xcode.

  在命令 `flutter build ipa --export-method` 無法使用的 Flutter 版本里，
  開啟 `build/ios/archive/MyApp.xcarchive` 檔案並按照下面的說明來驗證和釋出應用。

{{site.alert.end}}

### Upload the app bundle to App Store Connect

### 上傳應用套裝到 App Store Connect

Once the app bundle is created, upload it to
[App Store Connect][appstoreconnect_login] by either:

最後，建立一個建構歸檔並將其上傳到
[App Store Connect][appstoreconnect_login]：

<ol markdown="1">
<li markdown="1">

Install and open the [Apple Transport macOS app][apple_transport_app].
Drag and drop the `build/ios/ipa/*.ipa` app bundle into the app.

安裝並開啟 [Transporter macOS 應用][apple_transport_app]，
將 `build/ios/ipa/*.ipa` 下的應用套裝拖入 Transporter 應用中。

</li>

<li markdown="1">

Or upload the app bundle from the command line by running:

也可以在命令列執行下面的命令將應用套裝上傳：
```bash
xcrun altool --upload-app --type ios -f build/ios/ipa/*.ipa --apiKey your_api_key --apiIssuer your_issuer_id
```
Run `man altool` for details about how to authenticate with the App Store Connect API key.

執行 `man altool` 命令瞭解如何使用 App Store Connect API 金鑰進行認證。

</li>

<li markdown="1">

Or open `build/ios/archive/MyApp.xcarchive` in Xcode.

在 Xcode 中開啟 `build/ios/archive/MyApp.xcarchive`。

Click the **Validate App** button. If any issues are reported,
address them and produce another build. You can reuse the same
build ID until you upload an archive.

點選 **Validate App** 按鈕。如果報告了任何問題，記錄下他們並重新開始一個新的建構。
在你上傳一個歸檔前，可以一直使用同一個 Build ID。

After the archive has been successfully validated, click
**Distribute App**.

當這個歸檔校驗成功以後，點選 **Distribute App**。

{{site.alert.note}}

  When you export your app at the end of **Distribute App**,
  Xcode will create a directory containing
  an IPA of your app and an `ExportOptions.plist` file.
  You can create new IPAs with the same options without launching
  Xcode by running
  `flutter build ipa --export-options-plist=path/to/ExportOptions.plist`.
  See `xcodebuild -h` for details about the keys in this property list.

  當你最後在 **Distribute App** 匯出應用時，Xcode 將會建立一個含有你的應用 IPA 
  以及 `ExportOptions.plist` 檔案的資料夾。
  你可以無需啟動 Xcode，透過運命令
  `flutter build ipa --export-options-plist=path/to/ExportOptions.plist` 
  就可以建立新的 IPA。執行 `xcodebuild -h` 以檢視該屬性列表中 key 的詳細資訊。

{{site.alert.end}}

</li>
</ol>

You can follow the status of your build in the
Activities tab of your app's details page on
[App Store Connect][appstoreconnect_login].

你可以在 [App Store Connect][appstoreconnect_login]
中應用詳情頁面的 Activities 標籤頁檢視你的建構狀態。

You should receive an email within 30 minutes notifying you that
your build has been validated and is available to release to testers
on TestFlight. At this point you can choose whether to release
on TestFlight, or go ahead and release your app to the App Store.

當你的建構已經通過了校驗，可以將你的建構透過 TestFlight
釋出給你的測試人員或直接將其釋出到 App Store 的時候，
你會在 30 分鐘內收到一封信來提醒你。

For more details, see
[Upload an app to App Store Connect][distributionguide_upload].

更多內容可參考 [上傳 App 到 App Store Connect][distributionguide_upload]。

## Create a build archive with Codemagic CLI tools

## 使用 Codemagic 命令列工具建立一個建構歸檔

This step covers creating a build archive and uploading
your build to App Store Connect using Flutter build commands
and [Codemagic CLI Tools][codemagic_cli_tools] executed in a terminal
in the Flutter project directory. This allows you to create a build archive
with full control of distribution certificates in a temporary keychain 
isolated from your login keychain.

該步驟包含了在 Flutter 專案的目錄下，
透過終端使用 Flutter 建構命令和 [Codemagic 命令列工具][codemagic_cli_tools]，
建構歸檔並上傳至 App Store 的課程。
該操作可以讓你完全控制分發證書和臨時鑰匙串，將它們與登入的進行隔離。

<ol markdown="1">
<li markdown="1">

Install the Codemagic CLI tools:

安裝 Codemagic 命令列工具：

```bash
pip3 install codemagic-cli-tools
```

</li>
<li markdown="1">

You'll need to generate an [App Store Connect API Key][appstoreconnect_api_key]
with App Manager access to automate operations with App Store Connect. To make
subsequent commands more concise, set the following environment variables from
the new key: issuer id, key id, and API key file.

你需要使用包含 App 管理許可權的 App Store Connect 帳號，產生一個
[App Store Connect API Key][appstoreconnect_api_key]。
為了使後續的命令更加簡潔，你可以在環境變數中配置這些內容：
issuer id、key id 以及 API key 檔案。

```bash
export APP_STORE_CONNECT_ISSUER_ID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee
export APP_STORE_CONNECT_KEY_IDENTIFIER=ABC1234567
export APP_STORE_CONNECT_PRIVATE_KEY=`cat /path/to/api/key/AuthKey_XXXYYYZZZ.p8`
```

</li>
<li markdown="1">

You need to export or create an iOS Distribution certificate to code sign and package a build archive.

你需要匯出或者建立一個 iOS 分發證書，用來簽名及歸檔建構。

If you have existing [certificates][devportal_certificates], you can export the
private keys by executing the following command for each certificate:

如果你已經有可用的 [證書][devportal_certificates]，你可以使用以下命令匯出私鑰：

```bash
openssl pkcs12 -in <certificate_name>.p12 -nodes -nocerts | openssl rsa -out cert_key
```

Or you can create a new private key by executing the following command:

你也可以使用以下命令建立新的私鑰：

```bash
ssh-keygen -t rsa -b 2048 -m PEM -f cert_key -q -N ""
```

Later, you can have CLI tools automatically create a new iOS Distribution from the private key.

之後你便可以用命令列工具基於這個私鑰建立新的 iOS 分發建構了。

</li>
<li markdown="1">

Set up a new temporary keychain to be used for code signing:

配置一個用於簽名的臨時鑰匙串：

```bash
keychain initialize
```

{{site.alert.secondary}}

**Restore Login Keychain!**
After running `keychain initialize` you **must** run the following:<br>

**記得重置用於登入的鑰匙串！**
執行完 `keychain initialize` 後，**必須** 執行以下命令：<br>

`keychain use-login`

This sets your login keychain as the default to avoid potential
authentication issues with apps on your machine.

該命令會將登入的鑰匙串設為預設，以避免一些裝置上潛在的認證問題。

{{site.alert.end}}

</li>
<li markdown="1">

Fetch the code signing files from App Store Connect:

從 App Store Connect 上獲取簽名檔案：

```bash
app-store-connect fetch-signing-files $(xcode-project detect-bundle-id) \
    --platform IOS \
    --type IOS_APP_STORE \
    --certificate-key=@file:/path/to/cert_key \
    --create
```

Where `cert_key` is either your exported iOS Distribution certificate private key
or a new private key which automatically generates a new certificate. The certificate
will be created from the private key if it doesn't exist in App Store Connect.

其中，`cert_key` 是你匯出的 iOS 分發證書的私鑰，或者是自動產生新證書的新私鑰。
如果 App Store Connect 中不存在這個證書，將會透過私鑰建立。

</li>
<li markdown="1">

Now add the fetched certificates to your keychain:

將獲取到的證書新增到你的鑰匙串中：

```bash
keychain add-certificates
```

</li>
<li markdown="1">

Update the Xcode project settings to use fetched code signing profiles:

更新 Xcode 專案設定，使用獲取到的簽名配置：

```bash
xcode-project use-profiles
```

</li>
<li markdown="1">

Install Flutter dependencies:

獲取 Flutter 依賴：

```bash
flutter packages pub get
```

</li>
<li markdown="1">

Install CocoaPods dependencies:

獲取 CocoaPods 依賴：

```bash
find . -name "Podfile" -execdir pod install \;
```

</li>
<li markdown="1">

Build the Flutter the iOS project:

建構 Flutter 的 iOS 專案：

```bash
flutter build ipa --release \
    --export-options-plist=$HOME/export_options.plist
```

Note that `export_options.plist` is the output of the `xcode-project use-profiles` command.

注意 `export_options.plist` 路徑來源於 `xcode-project use-profiles` 命令的輸出。

</li>
<li markdown="1">

Publish the app to App Store Connect:

將應用釋出到 App Store Connect：

```bash
app-store-connect publish \
    --path $(find $(pwd) -name "*.ipa")
```

</li>
<li markdown="1">

As mentioned earlier, don't forget to set your login keychain
as the default to avoid authentication issues
with apps on your machine:

如前文所示，記得將你的登入鑰匙串設定為預設，
避免裝置出現認證問題：

```bash
keychain use-login
```

</li>
</ol>

You should receive an email within 30 minutes notifying you that
your build has been validated and is available to release to testers
on TestFlight. At this point you can choose whether to release
on TestFlight, or go ahead and release your app to the App Store.

在 30 分鐘內，你會收到一封郵件，提醒你建構已驗證，可以在 TestFlight 上釋出。
這時你可以選擇在 TestFlight 上釋出，或是直接在 App Store 上釋出。

## Release your app on TestFlight

## 在 TestFlight 釋出你的應用

[TestFlight][] allows developers to push their apps
to internal and external testers. This optional step
covers releasing your build on TestFlight.

1. Navigate to the TestFlight tab of your app's application
   details page on [App Store Connect][appstoreconnect_login].

   在 [App Store Connect][appstoreconnect_login]中，
   你的應用的詳情頁面跳轉到 TestFlight Tab。

1. Select **Internal Testing** in the sidebar.

   在側邊欄選擇 **Internal Testing**。

1. Select the build to publish to testers, then click **Save**.

   選擇要釋出給測試人員的建構，然後點選 **儲存**。

1. Add the email addresses of any internal testers.
   You can add additional internal users in the **Users and Roles**
   page of App Store Connect,
   available from the dropdown menu at the top of the page.

   為每一個內部測試人員新增郵件。你可以在 App Store Connect 的
   **使用者與角色** 頁面新增額外的內部使用者，
   他們將會出現在頁面頂部的下拉選單中。

For more details, see [Distribute an app using TestFlight]
[distributionguide_testflight].

關於更多資訊，請檢視
[使用 TestFlight 分發應用 (Distribute an app using TestFlight
(iOS, tvOS, watchOS))][distributionguide_testflight]。

## Release your app to the App Store

## 在 App Store 釋出你的應用

When you're ready to release your app to the world,
follow these steps to submit your app for review and
release to the App Store:

當你準備釋出你的 app 到這個世界時，跟隨下面的步驟，來提交你的 App 去稽核，並將其釋出到 App Store。

1. Select **Pricing and Availability** from the sidebar of your app's
   application details page on
   [App Store Connect][appstoreconnect_login] and complete the
   required information.

   從你的 app 在 [App Store Connect][appstoreconnect_login]
   的頁面中的側邊欄中選擇 **Pricing and Availability**，然後完善所有的必填資訊。

1. Select the status from the sidebar. If this is the first
   release of this app, its status is
   **1.0 Prepare for Submission**. Complete all required fields.

   從側邊欄選擇狀態。如果這是第一次釋出這個 App，
   這個狀態將會是 **1.0 Prepare for Submission**，填寫所有需要填寫的區域。

1. Click **Submit for Review**.

   點選 **提交稽核**

Apple notifies you when their app review process is complete.
Your app is released according to the instructions you
specified in the **Version Release** section.

Apple 將會在他們的稽核過程結束後提醒你。
你的 app 將會根據 **Version Release** 部分的介紹進行釋出。

For more details, see
[Distribute an app through the App Store][distributionguide_submit].

關於更多細節，檢視 [透過 App Store 分發一個 App][distributionguide_submit].

## Troubleshooting

## 故障排除

The [Distribute your app][distributionguide] guide provides a
detailed overview of the process of releasing an app to the App Store.

[分發你的應用][distributionguide] 指南，
提供了詳細的釋出應用到 App Store 過程的內容。


[appicon]: {{site.apple-dev}}/ios/human-interface-guidelines/icons-and-images/app-icon/
[appreview]: {{site.apple-dev}}/app-store/review/
[appsigning]: https://help.apple.com/xcode/mac/current/#/dev154b28f09
[appstore]: {{site.apple-dev}}/app-store/submissions/
[appstoreconnect]: {{site.apple-dev}}/support/app-store-connect/
[appstoreconnect_api_key]: https://appstoreconnect.apple.com/access/api
[appstoreconnect_guide]: {{site.apple-dev}}/support/app-store-connect/
[appstoreconnect_guide_register]: https://help.apple.com/app-store-connect/#/dev2cd126805
[appstoreconnect_login]: https://appstoreconnect.apple.com/
[codemagic_cli_tools]: {{site.github}}/codemagic-ci-cd/cli-tools
[codesigning_guide]: {{site.apple-dev}}/library/content/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html
[Core Foundation Keys]: {{site.apple-dev}}/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html
[devportal_appids]: {{site.apple-dev}}/account/ios/identifier/bundle
[devportal_certificates]: {{site.apple-dev}}/account/resources/certificates
[devprogram]: {{site.apple-dev}}/programs/
[devprogram_membership]: {{site.apple-dev}}/support/compare-memberships/
[distributionguide]: https://help.apple.com/xcode/mac/current/#/devac02c5ab8
[distributionguide_config]: https://help.apple.com/xcode/mac/current/#/dev91fe7130a
[distributionguide_submit]: https://help.apple.com/xcode/mac/current/#/dev067853c94
[distributionguide_testflight]: https://help.apple.com/xcode/mac/current/#/dev2539d985f
[distributionguide_upload]: https://help.apple.com/xcode/mac/current/#/dev442d7f2ca
[obfuscate your Dart code]: {{site.url}}/deployment/obfuscate
[TestFlight]: {{site.apple-dev}}/testflight/
[appreview_cn]: https://developer.apple.com/cn/app-store/review/
[appstore_cn]: https://developer.apple.com/cn/app-store/submissions/
[devprogram_cn]: https://developer.apple.com/cn/programs/
[devprogram_membership_cn]: https://developer.apple.com/cn/support/compare-memberships/
[appstoreconnect_cn]: https://developer.apple.com/cn/support/app-store-connect/
[appstoreconnect_guide_cn]: https://developer.apple.com/cn/support/app-store-connect/
[appstoreconnect_guide_register_cn]: https://help.apple.com/app-store-connect/?lang=zh-cn/dev2cd126805
[testflight_cn]: https://developer.apple.com/cn/testflight/
[app_bundle_export_method]: https://help.apple.com/xcode/mac/current/#/dev31de635e5
[apple_transport_app]: https://apps.apple.com/us/app/transporter/id1450874784
