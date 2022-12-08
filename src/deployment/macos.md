---
title: Build and release a macOS app
title: 建構和釋出為 macOS 應用
description: How to release a Flutter app to the macOS App Store.
description: 如何在 macOS 的 App Store 上釋出一個 Flutter 應用。
short-title: macOS
short-title: macOS
---

This guide provides a step-by-step walkthrough of releasing a
Flutter app to the [App Store][appstore].

本課程將指導開發者如何在 [App Store][appstore] 上釋出 Flutter 應用程式。

## Preliminaries

## 預備工作

Before beginning the process of releasing your app,
ensure that it meets
Apple's [App Review Guidelines][appreview].

在開始釋出應用程式之前，
請確保它符合蘋果的 [應用程式審查指南][appreview]。

In order to publish your app to the App Store,
you must first enroll in the
[Apple Developer Program][devprogram].
You can read more about the various
membership options in Apple's
[Choosing a Membership][devprogram_membership] guide.

為了將應用程式釋出到 App Store，
你必須先註冊 [蘋果開發者計劃][devprogram]。
可以在 Apple 的 [選擇會員資格][devprogram_membership] 指南中閱讀更多關於各種會員資格的資訊。

## Register your app on App Store Connect

## 在 App Store Connect 上註冊你的應用程式

Manage your app's life cycle on
[App Store Connect][appstoreconnect] (formerly iTunes Connect).
You define your app name and description, add screenshots,
set pricing, and manage releases to the App Store and TestFlight.

在 [App Store Connect][appstoreconnect]（以前叫 iTunes Connect）上管理應用程式的生命週期。
你可以定義應用程式的名稱和描述、新增螢幕截圖、
設定定價以及管理應用程式商店和 TestFlight 的釋出。

Registering your app involves two steps: registering a unique
Bundle ID, and creating an application record on App Store Connect.

註冊應用程式包括兩個步驟：註冊一個唯一的 Bundle ID，
以及在 App Store Connect 上建立應用程式記錄。

For a detailed overview of App Store Connect, see the
[App Store Connect][appstoreconnect_guide] guide.

有關 App Store Connect 的詳細概述，
請參閱 [App Store Connect 指南][appstoreconnect_guide]。

### Register a Bundle ID

### 註冊 Bundle ID

Every macOS application is associated with a Bundle ID,
a unique identifier registered with Apple.
To register a Bundle ID for your app, follow these steps:

每個 macOS 應用程式都與一個 Bundle ID 關聯，
Bundle ID 是在 Apple 註冊的唯一標識。
要為應用程式註冊 Bundle ID，請執行以下步驟：

1. Open the [App IDs][devportal_appids] page of your developer account.

   開啟開發者帳戶的 [App IDs][devportal_appids] 頁面。
   
1. Click **+** to create a new Bundle ID.

   點選 **+** 建立一個新的 Bundle ID。
   
1. Enter an app name, select **Explicit App ID**, and enter an ID.
   
   輸入應用程式名稱，選擇 **顯式 App ID**，然後輸入 ID。
   
1. Select the services your app uses, then click **Continue**.
   
   選擇應用程式使用的服務，然後點選 **下一步**。
   
1. On the next page, confirm the details and click **Register**
   to register your Bundle ID.
   
   在下一頁中，確認應用的詳細資訊，然後點選 **註冊** 來註冊你的 Bundle ID。

### Create an application record on App Store Connect

### 在 App Store Connect 上建立應用程式記錄

Register your app on App Store Connect:

在 App Store Connect 上註冊你的應用程式：

1. Open [App Store Connect][appstoreconnect_login] in your browser.
  
   在瀏覽器中開啟 [App Store Connect][appstoreconnect_login]。
   
1. On the App Store Connect landing page, click **My Apps**.

   在 App Store Connect 登入頁上，點選 **我的應用程式**。
   
1. Click **+** in the top-left corner of the My Apps page,
   then select **New App**.
   
   點選我的應用程式頁面左上角的 **+**，然後選擇 **新建應用程式**。
   
1. Fill in your app details in the form that appears.
   In the Platforms section, ensure that macOS is checked.
   Since Flutter does not currently support tvOS,
   leave that checkbox unchecked. Click **Create**.
   
   在表單中填寫應用程式詳細資訊。
   在平台部分，請確保選中了 macOS。
   由於 Flutter 目前不支援 tvOS，所以不要選中該項。點選 **建立**。
   
1. Navigate to the application details for your app and select
   **App Information** from the sidebar.

   從側邊欄中選擇 **應用程式資訊**，可以檢視應用程式的詳細資訊。
   
1. In the General Information section, select the Bundle ID
   you registered in the preceding step.
   
   在常規資訊中，選擇在上一步中註冊的 Bundle ID。

For a detailed overview,
see [Add an app to your account][appstoreconnect_guide_register].

更詳細的介紹，請參閱 [將應用程式新增到您的帳戶][appstoreconnect_guide_register]]

## Review Xcode project settings

## 檢查 Xcode 專案設定

This step covers reviewing the most important settings
in the Xcode workspace.
For detailed procedures and descriptions, see
[Prepare for app distribution][distributionguide_config].

這一步包括檢查 Xcode 工作區中最重要的設定。
更詳細的過程和說明，請參閱 [準備應用程式分發][distributionguide_config]。

Navigate to your target's settings in Xcode:

在 Xcode 中配置目標：

1. In Xcode, open `Runner.xcworkspace` in your app's `macos` folder.

   在 Xcode 中，開啟應用程式 `macos` 資料夾中的 `Runner.xcworkspace`。

1. To view your app's settings, select the **Runner** project in the Xcode
   project navigator. Then, in the main view sidebar, select the **Runner**
   target.
   
   要檢視應用程式的設定，請在 Xcode 導航欄中選擇 **Runner** 專案。
   然後，在主檢視側欄中，選擇 **Runner** 目標。
      
1. Select the **General** tab.

   選擇 **General（常規）** 選項。

Verify the most important settings.

確認最重要的設定。

In the **Identity** section:

在 **Identity（標識）** 部分：

`App Category`
<br> The app category under which your app will be listed on the Mac App Store. This cannot be none.

`App Category（應用類別）`
<br> 你的應用將出現在 Mac App Store 中的哪個類別，此項不能為空。

`Bundle Identifier` 
<br> The App ID you registered on App Store Connect.

`Bundle Identifier`
<br> 你在 App Store Connect 註冊的應用程式 ID。

In the **Deployment info** section:

在 **Deployment info（部署資訊）** 部分：

`Deployment Target`
<br> The minimum macOS version that your app supports. Flutter supports macOS 10.11 and later.

`Deployment Target（部署目標）`
<br> 應用程式支援的最低 macOS 版本。Flutter 支援 macOS 10.11 及更高版本。


In the **Signing & Capabilities** section:

在 **Signing & Capabilities（簽名和功能）** 部分：

`Automatically manage signing`
<br> Whether Xcode should automatically manage app signing
  and provisioning.  This is set `true` by default, which should
  be sufficient for most apps. For more complex scenarios,
  see the [Code Signing Guide][codesigning_guide].

`Automatically manage signing（自動管理簽名）`
<br> Xcode 是否自動管理應用程式簽名和配置。
  預設為 `true`，這對於大多數應用程式來說應該足夠。
  更復雜的場景，請參閱 [程式碼簽名指南][codesigning_guide]。

`Team`
<br> Select the team associated with your registered Apple Developer
  account. If required, select **Add Account...**,
  then update this setting.

`Team（團隊）`
<br> 選擇與你註冊的 Apple 開發者帳戶關聯的團隊。
  如果需要，請選擇 **Add Account...（新增賬戶...）**，然後更新此設定。

The **General** tab of your project settings should resemble
the following:

專案設定的 **General（常規）** 選項應類似於以下內容：

![Xcode Project Settings]({{site.url}}/assets/images/docs/releaseguide/macos_xcode_settings.png){:width="100%"}

For a detailed overview of app signing, see
[Create, export, and delete signing certificates][appsigning].

有關應用程式簽名的詳細概述，請參閱 [建立、匯出和刪除簽名證書][appsigning]。

## Configuring the app's name, bundle identifier and copyright

## 配置應用名稱、Bundle ID 和版權資訊

The configuration for the product identifiers are centralized 
in `macos/Runner/Configs/AppInfo.xcconfig`. For the app's name,
set `PRODUCT_NAME`, for the copyright set `PRODUCT_COPYRIGHT`,
and finally set `PRODUCT_BUNDLE_IDENTIFIER` for the app's
bundle identifier.

參考標識的配置集中在 `macos/Runner/Configs/AppInfo.xcconfig` 檔案中。
想修改應用名稱，設定 `PRODUCT_NAME`；想修改版權資訊，設定 `PRODUCT_COPYRIGHT`；
想修改 Bundle ID，設定 `PRODUCT_BUNDLE_IDENTIFIER`。

## Updating the app's version number

## 更新應用程式的版本號

The default version number of the app is `1.0.0`.
To update it, navigate to the `pubspec.yaml` file
and update the following line:

應用程式的預設版本號為 `1.0.0`。
如需更新版本號，在 `pubspec.yaml` 檔案中更新以下位置：

`version: 1.0.0+1`

The version number is three numbers separated by dots,
such as `1.0.0` in the example above, followed by an optional
build number such as `1` in the example above, separated by a `+`.

版本號是三個用點分隔的數字，如上面範例中的 `1.0.0`，
後面用 `+` 分隔的是可選的內部版本號，如上面範例中的 `1`。

Both the version and the build number may be overridden in Flutter's
build by specifying `--build-name` and `--build-number`,
respectively.

版本號和內部版本號都可以在 Flutter 建構時，
透過指定 `--build name` 和 `--build number` 進行覆蓋。

In macOS, `build-name` uses `CFBundleShortVersionString`
while `build-number` uses `CFBundleVersion`.
Read more about iOS versioning at [Core Foundation Keys][]
on the Apple Developer's site.

在 macOS 中，`build-name` 使用 `CFBundleShortVersionString`，
而 `build-number` 使用 `CFBundleVersion`。
在蘋果開發者的網站上，檢視更多關於 iOS 版本的 [Core Foundation Keys][Core Foundation Keys] 。

## Add an app icon

## 新增應用程式圖示

When a new Flutter app is created, a placeholder icon set is created.
This step covers replacing these placeholder icons with your
app's icons:

建立一個新的 Flutter 應用程式時，會建立一個佔位圖示集。
此步驟包含如何用應用程式的圖示替換這些佔位圖示：

1. Review the [macOS App Icon][appicon] guidelines.

   檢視 [macOS 應用程式圖示][appicon] 指南。
   
1. In the Xcode project navigator, select `Assets.xcassets` in the
   `Runner` folder. Update the placeholder icons with your own app icons.
   
   在 Xcode 專案導航欄的 `Runner` 資料夾中選擇 `Assets.xcassets`。
   用你自己的應用程式圖示更新佔位圖示。
   
1. Verify the icon has been replaced by running your app using
   `flutter run -d macos`.
   
   使用 `flutter run -d macos` 執行應用程式，驗證圖示是否已被替換。

## Create a build archive with Xcode

## 建立建構存檔

This step covers creating a build archive and uploading
your build to App Store Connect using Xcode.

此步驟包含建立建構存檔並將其上傳到 App Store Connect。

During development, you've been building, debugging, and testing
with _debug_ builds. When you're ready to ship your app to users
on the App Store or TestFlight, you need to prepare a _release_ build.
At this point, you might consider [obfuscating your Dart code][]
to make it more difficult to reverse engineer. Obfuscating
your code involves adding a couple flags to your build command.

在開發時，你已經完成了在 **debug** 模式下的應用建構、除錯和測試。
當你準備好在 App Store 或 TestFlight 上向用戶釋出應用時，你需要準備一個 **release** 版產物。
此時，你可以考慮 [混淆你的 Dart 程式碼][obfuscating your Dart code] 讓逆向工程變得更加困難。
混淆你的程式碼需要向建構命令新增兩個標誌。

In Xcode, configure the app version and build:

在 Xcode 中，配置應用程式版本和內部版本：

1. Open `Runner.xcworkspace` in your app's `macos` folder. To do this from
   the command line, run the following command from the base directory of your
   application project.

   開啟 `macos` 資料夾中的 `Runner.xcworkspace` 工程專案，若要在命令列中這樣做
   就切換到工程的資料夾下執行下面的命令:

   ```console
   open macos/Runner.xcworkspace
   ```

1. Select **Runner** in the Xcode project navigator, then select the
   **Runner** target in the settings view sidebar.
   
   在 Xcode 專案導航欄中選擇 **Runner**，然後在設定側欄中選擇 **Runner** 目標。
   
1. In the Identity section, update the **Version** to the user-facing
   version number you wish to publish.
   
   在標識部分，將 **Version（版本）** 更新為要釋出的版本號。
   
1. In the Identity section, update the **Build** identifier to a unique
   build number used to track this build on App Store Connect.
   Each upload requires a unique build number.
   
   在標識部分，將 **Build identifier（建構標識）**
   更新為在 App Store Connect 上可以追蹤此產生的唯一產生串。
   每次上傳都需要一個唯一的建構標識。

Finally, create a build archive and upload it to App Store Connect:

最後，建立一個建構歸檔並將其上傳到 App Store Connect：

1. Create a release Archive of your application. From the base directory of
   your application project, run the following.

   為你的應用建立一個釋出歸檔，命令列切換到你的工程目錄，
   執行下面的命令：

   ```console
   flutter build macos
   ```
1. Open Xcode and select **Product > Archive** to open the archive created
   in the previous step.

   開啟 Xcode 並選擇 **Product > Archive**，開啟上個步驟產生的歸檔檔案；

1. Click the **Validate App** button. If any issues are reported,
   address them and produce another build. You can reuse the same
   build ID until you upload an archive.

   點選 **Validate App** 按鈕。如果報告了任何問題，請嘗試解決並再次建構。
   在上傳歸檔之前，可以重用相同的建構 ID。

1. After the archive has been successfully validated, click
   **Distribute App**. You can follow the status of your build in the
   Activities tab of your app's details page on
   [App Store Connect][appstoreconnect_login].

   成功驗證歸檔後，點選 **Distribute App**。
   你可以在 [App Store Connect][appstoreconnect_login]
   上的應用程式詳細資訊頁的活動標籤下檢視建構狀態。

You should receive an email within 30 minutes notifying you that
your build has been validated and is available to release to testers
on TestFlight. At this point you can choose whether to release
on TestFlight, or go ahead and release your app to the App Store.

你應該會在 30 分鐘內收到一封郵件。告知你的建構已經過驗證，
可以在 TestFlight 上釋出給測試人員。
此時，你可以選擇在 TestFlight 上釋出，或者繼續將應用程式釋出到應用程式商店。

For more details, see
[Upload an app to App Store Connect][distributionguide_upload].

更多詳細資訊，請參閱 
[將應用程式上傳到 App Store Connect][distributionguide_upload]。

## Create a build archive with Codemagic CLI tools

## 使用 Codemagic 命令列工具建立一個建構歸檔

This step covers creating a build archive and uploading
your build to App Store Connect using Flutter build commands 
and [Codemagic CLI Tools][codemagic_cli_tools] executed in a terminal
in the Flutter project directory.

下面的步驟，我們會介紹在 Flutter 應用的工程目錄下執行
Flutter 建構命令和 [Codemagic 命令列工具][codemagic_cli_tools]，
建立一個建構歸檔並將其上傳至 App Store Connect。

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

你需要產生一個具有 App Manager 存取許可權的 App Store Connect API 金鑰，
以方便對 App Store Connect 進行自動化操作。為了使後續的命令更簡潔，
請設定下面的環境變數：發行者 ID、金鑰 ID、API 金鑰檔案：

```bash
export APP_STORE_CONNECT_ISSUER_ID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee
export APP_STORE_CONNECT_KEY_IDENTIFIER=ABC1234567
export APP_STORE_CONNECT_PRIVATE_KEY=`cat /path/to/api/key/AuthKey_XXXYYYZZZ.p8`
```

</li>
<li markdown="1">

You need to export or create a Mac App Distribution and a Mac Installer
Distribution certificate to perform code signing and package a build archive.

你需要匯出或者建立 Mac App Distribution 和 Mac Installer Distribution 證書，
以便與執行程式碼簽名以及打包建構歸檔。

If you have existing [certificates][devportal_certificates], you can export the
private keys by executing the following command for each certificate:

對於已有的 [證書][devportal_certificates]，你可以選擇透過下嗎的命令
來匯出私鑰：

```bash
openssl pkcs12 -in <certificate_name>.p12 -nodes -nocerts | openssl rsa -out cert_key
```

Or you can create a new private key by executing the following command:

或者透過以下命令建立一個新的私鑰：

```bash
ssh-keygen -t rsa -b 2048 -m PEM -f cert_key -q -N ""
```

Later, you can have CLI tools automatically create a new Mac App Distribution and
Mac Installer Distribution certificate. You can use the same private key for
each new certificate.

之後，你可以讓命令列工具自動建立新的
Mac App Distribution 和 Mac Installer Distribution 證書，
每個新的證書都可以使用相同的私鑰。

</li>
<li markdown="1">

Fetch the code signing files from App Store Connect:

從 App Store Connect 獲取需要程式碼簽名的檔案：

```bash
app-store-connect fetch-signing-files YOUR.APP.BUNDLE_ID \
    --platform MAC_OS \
    --type MAC_APP_STORE \
    --certificate-key=@file:/path/to/cert_key \
    --create
```

Where `cert_key` is either your exported Mac App Distribution certificate private key
or a new private key which automatically generates a new certificate.

上面程式碼裡的 `cert_key` 是你已匯出的或者新產生的
Mac App Distribution 證書私鑰。

</li>
<li markdown="1">

If you do not have a Mac Installer Distribution certificate,
you can create a new certificate by executing the following:

如果你還沒有 Mac Installer Distribution 證書，
透過執行下面的命令列可以產生一個：

```bash
app-store-connect create-certificate \
    --type MAC_INSTALLER_DISTRIBUTION \
    --certificate-key=@file:/path/to/cert_key \
    --save
```

Use `cert_key` of the private key you created earlier.

使用你之前建立的私鑰的 `cert_key`。

</li>
<li markdown="1">

Fetch the Mac Installer Distribution certificates:

獲取 Mac 安裝程式分發證書：

```bash
app-store-connect list-certificates \
    --type MAC_INSTALLER_DISTRIBUTION \
    --certificate-key=@file:/path/to/cert_key \
    --save
```

</li>
<li markdown="1">

Set up a new temporary keychain to be used for code signing:

設定用於程式碼簽名的新臨時鑰匙串：

```bash
keychain initialize
```

{{site.alert.secondary}}
  **Restore Login Keychain!**
  After running `keychain initialize` you **must** run the following:<br>

  **恢復登入鑰匙串！**
   執行 `keychain initialize` 後，你 **必須** 執行以下命令：<br>

  `keychain use-login`

  This sets your login keychain as the default to avoid potential
  authentication issues with apps on your machine.

  這會將你的登入鑰匙串設定為預設值，
  以避免你機器上的應用程式出現潛在的身份驗證問題。

{{site.alert.end}}

</li>
<li markdown="1">

Now add the fetched certificates to your keychain:

現在將獲取的證書新增到你的鑰匙串中：

```bash
keychain add-certificates
```

</li>
<li markdown="1">

Update the Xcode project settings to use fetched code signing profiles:

更新 Xcode 專案設定以使用獲取的程式碼簽名配置檔案：

```bash
xcode-project use-profiles
```

</li>

<li markdown="1">

Install Flutter dependencies:

安裝 Flutter 依賴項：

```bash
flutter packages pub get
```

</li>
<li markdown="1">

Install CocoaPods dependencies:

安裝 CocoaPods 依賴項：

```bash
find . -name "Podfile" -execdir pod install \;
```

</li>
<li markdown="1">

Build the Flutter macOS project:

建構 Flutter macOS 專案：

```bash
flutter build macos --release
```

</li>
<li markdown="1">

Package the app:

打包應用程式：

```bash
APP_NAME=$(find $(pwd) -name "*.app")
PACKAGE_NAME=$(basename "$APP_NAME" .app).pkg
xcrun productbuild --component "$APP_NAME" /Applications/ unsigned.pkg

INSTALLER_CERT_NAME=$(keychain list-certificates \
          | jq '[.[]
            | select(.common_name
            | contains("Mac Developer Installer"))
            | .common_name][0]' \
          | xargs)
xcrun productsign --sign "$INSTALLER_CERT_NAME" unsigned.pkg "$PACKAGE_NAME"
rm -f unsigned.pkg 
```

</li>
<li markdown="1">

Publish the packaged app to App Store Connect:

將打套件的應用釋出到 App Store Connect：

```bash
app-store-connect publish \
    --path "$PACKAGE_NAME"
```

</li>
<li markdown="1">

As mentioned earlier, don't forget to set your login keychain
as the default to avoid authentication issues
with apps on your machine:

如前所述，不要忘記將你的登入鑰匙串設定為預設設定，
以避免你機器上的應用程式出現身份驗證問題：

```bash
keychain use-login
```

</li>
</ol>

## Distribute to registered devices

## 分發到已註冊的裝置

[TestFlight][] is not available for distributing macOS apps
to internal and external testers. See [distribution guide][distributionguide_macos] 
to prepare an archive for distribution to designated Mac computers.

[TestFlight][] 不可用於向內部和外部的測試人員分發 macOS 應用。
請參閱 [分發指南][distributionguide_macos]，
準備一個歸檔檔案，以便分發到指定的 Mac 裝置。

## Release your app to the App Store

## 將應用程式釋出到應用程式商店

When you're ready to release your app to the world,
follow these steps to submit your app for review and
release to the App Store:

當你準備向全世界釋出應用程式時，
請按照以下步驟提交應用程式以供審閱併發布到應用程式商店：

1. Select **Pricing and Availability** from the sidebar of your app's
   application details page on
   [App Store Connect][appstoreconnect_login] and complete the
   required information.
   
   在 [App Store Connect][appstoreconnect_login] 
   應用程式詳情頁的側欄中選擇 **定價和可用性**，並完善相關資訊。
   
1. Select the status from the sidebar. If this is the first
   release of this app, its status is
   **1.0 Prepare for Submission**. Complete all required fields.
   
   從側邊欄中選擇狀態。
   如果這是應用程式的第一個版本，
   則其狀態為 **1.0 準備提交**。填寫所有必填欄位。
     
1. Click **Submit for Review**.

   點選 **提交稽核**。

Apple notifies you when their app review process is complete.
Your app is released according to the instructions you
specified in the **Version Release** section.

蘋果會在你的應用程式稽核完成後通知你。
應用程式是按照你在 **版本釋出** 說明中釋出的。

For more details, see
[Distribute an app through the App Store][distributionguide_submit].

更多詳細資訊，請參閱 [透過應用程式商店分發應用程式][distributionguide_submit].

## Troubleshooting

## 故障排除

The [Distribute your app][distributionguide] guide provides a
detailed overview of the process of releasing an app to the App Store.

[分發你的應用程式][distributionguide] 指南詳細概述了將應用程式釋出到應用商店的過程。

[appicon]: {{site.apple-dev}}/design/human-interface-guidelines/macos/icons-and-images/app-icon/
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
[distributionguide]: https://help.apple.com/xcode/mac/current/#/dev8b4250b57
[distributionguide_config]: https://help.apple.com/xcode/mac/current/#/dev91fe7130a
[distributionguide_macos]: https://help.apple.com/xcode/mac/current/#/dev295cc0fae
[distributionguide_submit]: https://help.apple.com/xcode/mac/current/#/dev067853c94
[distributionguide_upload]: https://help.apple.com/xcode/mac/current/#/dev442d7f2ca
[obfuscating your Dart code]: {{site.url}}/deployment/obfuscate
[TestFlight]: {{site.apple-dev}}/testflight/
