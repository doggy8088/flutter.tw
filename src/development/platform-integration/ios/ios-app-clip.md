---
title: Adding an iOS App Clip target
title: 新增 iOS App Clip target
description: How to add an iOS 14 App Clip target to your Flutter project.
description: 如何為您的 Flutter 工程加入 iOS 14 的 App Clip。
---

{{site.alert.important}}

  This experimental preview currently exceeds the 10MB
  uncompressed IPA payload size limit and cannot be
  used in production ([#71098][]).

  目前該實驗測試版本會突破 10MB 的未壓縮 IPA 有效負載的大小限制，
  請勿在生產環境中使用([#71098][])。
  
{{site.alert.end}}

This guide describes how to manually add another
Flutter-rendering iOS App Clip target to your
existing Flutter project or [add-to-app][] project.

這個指南介紹瞭如何手動新增另一個使用 Flutter 來渲染的 iOS App Clip target, 
並將它整合到您現有的 Flutter 專案或 [add-to-app][] 專案。

[#71098]: {{site.repo.flutter}}/issues/71098
[add-to-app]: {{site.url}}/development/add-to-app

{{site.alert.warning}}

  This is an advanced guide and is best intended
  for audience with a working knowledge of iOS development.

  這是一個高階指南，適合具有 iOS 開發工作經驗的讀者。

{{site.alert.end}}

To see a working sample, see the [App Clip sample][] on GitHub.

要檢視完整可用的範例，請參閱 GitHub 上的 [App Clip 範例][App Clip sample]。

[App Clip sample]: {{site.github}}/flutter/samples/tree/master/ios_app_clip

## Step 1 - Open project

## 步驟 1 - 開啟專案

Open your iOS Xcode project, such as
`ios/Runner.xcworkspace` for full-Flutter apps.

開啟您的 iOS Xcode 工程，例如您的純 Flutter 專案中的 `ios/Runner.xcworkspace`。

## Step 2 - Add an App Clip target

## 步驟 2 - 新增一個 App Clip 的 target

**2.1**

Click on your project in the Project Navigator to show
the project settings.

點選您專案的 Project Navigator 來顯示工程設定。

Press **+** at the bottom of the target list to add a new target.

點選 target 列表底部的 **+** 來新增一個新的 target。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/add-target.png" %}

**2.2**

Select the **App Clip** type for your new target.

為您的新 target 選擇 `App Clip` 型別。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/add-app-clip.png" %}

**2.3**

Enter your new target detail in the dialog.

在對話方塊為您的新 target 輸入詳情。

Select `Storyboard` for Interface.

選擇 `Storyboard` 作為介面。

Select `UIKit App Delegate` for Life Cycle.

選擇 `UIKit App Delegate` 作為生命週期。

Select the same language as your original target for **Language**.

選擇與您原來的 target 相同的**程式語言**。

(In other words, to simplify the setup,
don't create a Swift App Clip target for
an Objective-C main target, and vice versa.)

(換句話說，請勿為 Objective-C target 建立 Swift 型別的 App Clip target，
反之亦然，以簡化設定。)

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/app-clip-details.png" %}

**2.4**

In the following dialog,
activate the new scheme for the new target.

在接下來的對話方塊中，
為新的 target 啟用 (activate) 一個新的 scheme。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/activate-scheme.png" %}

<a name="step-3"></a>
## Step 3 - Remove unneeded files

## 步驟 3 - 移除不需要的檔案

**3.1**

In the Project Navigator, in the newly created App Clip group,
delete everything except `Info.plist` and
`<app clip target>.entitlements`.

在專案 Project Navigator 的新建立的 App Clip 組中，
將除了 `Info.plist` 和 `App Clip target.entitlements` 以外的所有內容刪除。

{{site.alert.tip}}

  For add-to-app users, it's up to the reader to decide
  how much of this template to keep to invoke
  `FlutterViewController` or `FlutterEngine` APIs
  from this code later.

  對於 add-to-app 的使用者，
  由讀者決定以後保留多少範本來從此程式碼中呼叫 
  `FlutterViewController` 或 `FlutterEngine` API。

{{site.alert.end}}

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/clean-files.png" %}

Move files to trash.

移動檔案到廢紙簍。

**3.2**

If you don't use the `SceneDelegate.swift` file,
remove the reference to it in the `Info.plist`.

如果您不使用 `SceneDelegate.swift` 檔案，移除在 `Info.plist` 中對應的參考。

Open the `Info.plist` file in the App Clip group.
Delete the entire dictionary entry for
**Application Scene Manifest**.

開啟 App Clip 組中的 `Info.plist`。
刪除 `Application Scene Manifest` 字典條目。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/scene-manifest.png" %}

## Step 4 - Share build configurations

## 步驟 4 -  共享建構配置

This step isn't necessary for add-to-app projects
since add-to-app projects have their custom build
configurations and versions.

對於 add-to-app 專案，此步驟不是必需的，
因為 add-to-app 有自己的自訂建構配置和版本。

**4.1**

Back in the project settings,
select the project entry now rather than any targets.

返回專案設定，現在選擇 Project 條目，
而不是 Targets 裡的任何 target。

In the **Info** tab, under the **Configurations**
expandable group, expand the
**Debug**, **Profile**, and **Release** entries.

在 **Info** 選項卡頁中的 **Configurations** 可擴充組下，
展開 **Debug**、**Profile** 和 **Release** 條目。

For each, select the same value from the drop-down menu
for the App Clip target as the entry selected for the
normal app target.

每一個 App Clip target 的下拉選單的值都應該與常規應用 target 中的值相同。

This gives your App Clip target access to Flutter's
required build settings.

這使您的 App Clip target 可以存取 Flutter 的必需建構設定。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/configuration.png" %}

**4.2**

In the App Clip group's `Info.plist` file, set:

在 App Clip 組的 `Info.plist` 檔案中，設定：

* `Build version string (short)` to `$(FLUTTER_BUILD_NAME)`
* `Bundle version` to `$(FLUTTER_BUILD_NUMBER)`

## Step 5 - Share code and assets

## 步驟 5 - 共享程式碼和資源

### Option 1 - Share everything

### 選項 1 - 共享所有東西

Assuming the intent is to show the same Flutter UI
in the standard app as in the App Clip,
share the same code and assets.

假設您的目標是在 App Clip 中顯示與普通應用相同的 Flutter UI，
並共享相同的程式碼和資源。

For each of the following: `Main.storyboard`, `Assets.xcassets`,
`LaunchScreen.storyboard`, `GeneratedPluginRegistrant.m`, and
`AppDelegate.swift` (and `Supporting Files/main.m` if using Objective-C),
select the file, then in the first tab of the inspector,
also include the App Clip target in the `Target Membership`
checkbox group.

對於以下每一個檔案： `Main.storyboard`、 `Assets.xcassets`、
`LaunchScreen.storyboard`、`GeneratedPluginRegistrant.m` 和 
`AppDelegate.swift`，
（如果您是 Objective-C 還應該包括 `Supporting Files/main.m`）
選擇檔案並且在檢查器中選擇第一個選項卡，
並且在 `Target Membership` 選中 `App Clip`。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/add-target-membership.png"
%}

### Option 2 - Customize Flutter launch for App Clip

### 選項 2 - 為 App Clip 自訂 Flutter 的啟動器

In this case, do not delete everything listed in [Step 3](#step-3).
Instead, use the scaffolding and the [iOS add-to-app APIs][]
to perform a custom launch of Flutter.
For example to show a [custom Flutter route][].

在這個例子中，不需要刪除在 [步驟 3](#step-3) 中的任何東西。
相對的，使用 [iOS add-to-app APIs][] 的範本來自訂 Flutter 啟動器。
可以參考範例 [自訂 Flutter 路由][custom Flutter route]。

[custom Flutter route]: {{site.url}}/development/add-to-app/ios/add-flutter-screen#route
[iOS add-to-app APIs]: {{site.url}}/development/add-to-app/ios/add-flutter-screen

## Step 6 - Add App Clip associated domains

## 步驟 6 - 新增 App Clip 的關聯域名

This is a standard step for App Clip development.
See the [official Apple documentation][].

這是一個 App Clip 開發的標準步驟。
請檢視 [蘋果官方文件][official Apple documentation]。

[official Apple documentation]: {{site.apple-dev}}/documentation/app_clips/creating_an_app_clip_with_xcode#3604097

**6.1**

Open the `<app clip target>.entitlements` file.
Add an `Associated Domains` Array type.
Add a row to the array with `appclips:<your bundle id>`.

開啟 `<app clip target>.entitlements` 檔案。
新增 `Associated Domains` 陣列。
新增一行 `appclips:<your bundle id>` 到陣列中。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/app-clip-entitlements.png"
%}

**6.2**

The same associated domains entitlement needs to be added
to your main app, as well.

同樣的相關域名權利也需要新增到您的主應用程式中。

Copy the `<app clip target>.entitlements` file from your
App Clip group to your main app group and rename it to
the same name as your main target
such as `Runner.entitlements`.

將 `<app clip target>.entitlements` 
檔案從 App Clip 組複製到主應用程式組，
並將其重新命名為與主目標相同的名稱，
例如 `Runner.entitlements`。

Open the file and delete the `Parent Application Identifiers`
entry for the main app's entitlement file
(leave that entry for the App Clip's entitlement file).

開啟檔案並刪除主應用程式授權檔案的 `Parent Application Identifiers` 
條目（將該條目保留為 App Clip 的授權檔案）。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/main-app-entitlements.png"
%}

**6.3**

Back in the project settings, select the main app's target,
open the **Build Settings** tab.
Set the **Code Signing Entitlements** setting to the
relative path of the second entitlements file
created for the main app.

返回專案設定，選擇主應用 target，
開啟 `Build Settings` 選項卡。
設定 `Code Signing Entitlements` 的值為
主應用建立的第二個授權檔案的相對路徑。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/main-app-entitlements-setting.png"
%}

## Step 7 - Integrate Flutter

## 步驟 7 - 整合 Flutter

These steps are not necessary for add-to-app.

add-to-app 不需要這些步驟。

**7.1**

In your App Clip's target's project settings,
open the **Build Settings** tab.

在您的 App Clip 的 target 的專案設定，開啟 `Build Settings` 選項卡。

For setting `Framework Search Paths`, add 2 entries:

為 `Framework Search Paths` 新增兩個內容：

- `$(inherited)`
- `$(PROJECT_DIR)/Flutter`

In other words, the same as the main app target's build settings.

換句話說，與主應用程式 target 的建構設定相同。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/app-clip-framework-search.png"
%}

**7.2**

For the Swift target,
set the `Objective-C Bridging Header`
build setting to `Runner/Runner-Bridging-Header.h`

如果是 Swift target，設定 `Objective-C Bridging Header` 建構配置
為 `Runner/Runner-Bridging-Header.h`。

In other words, the same as the main app target's build settings.

換句話說，與主應用程式 target 的建構設定相同。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/bridge-header.png"
%}

**7.3**

Now open the **Build Phases** tab. Press the **+** sign
and select **New Run Script Phase**.

現在開啟 `Build Phases` 選項卡。點選 `+` 並且選擇 `New Run Script Phase`。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/new-build-phase.png"
%}

Drag that new phase to below the **Dependencies** phase.

拖動新的 phase 到 `Dependencies` phase。

Expand the new phase and add this line to the script content:

展開新 phase 並將以下內容新增到指令碼：

```bash
/bin/sh "$FLUTTER_ROOT/packages/flutter_tools/bin/xcode_backend.sh" build
```

In other words,
the same as the main app target's build phases.

簡單來說，與主應用程式 target 的建構設定相同。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/xcode-backend-build.png"
%}

This ensures that your Flutter Dart code is compiled
when running the App Clip target.

這可以確保在執行 App Clip target 時編譯 Flutter 的 Dart 程式碼。

**7.4**

Press the **+** sign and select **New Run Script Phase** again.
Leave it as the last phase.

再次點選 `+` 並且選擇 `New Run Script Phase`。
這是最後一個 phase。

This time, add:

這次新增如下內容：

```bash
/bin/sh "$FLUTTER_ROOT/packages/flutter_tools/bin/xcode_backend.sh" embed_and_thin
```

In other words,
the same as the main app target's build phases.

簡單來說，與主應用程式 target 的建構設定相同。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/xcode-backend-embed.png"
%}

This ensures that your Flutter app and engine are embedded
into the App Clip bundle.

這將確保您的 Flutter 應用程式和引擎嵌入到 App Clip bundle 中。

## Step 8 - Disable Bitcode

In the App Clip target's **Build Settings** tab,
set the **Enable Bitcode** setting to No. On Xcode 14
and higher bitcode has been deprecated and this step is unnecessary.

在 App Clip target 的 `Build Settings` 選項卡中，
將 `Enable Bitcode` 設定設定為 No。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/bitcode.png"
%}

## Step 9 - Integrate plugins

## Step 9 - 整合外掛

{{site.alert.warning}}

  CocoaPods version 1.10.0.beta.1 or higher is required
  to run Flutter apps with plugins.

  執行帶有外掛的 Flutter 應用程式需要 CocoaPods 1.10.0.beta.1 或更高版本。
  
{{site.alert.end}}

**9.1**

Open the `Podfile` for your Flutter project
or add-to-app host project.

在您的 Flutter 專案
或是 add-to-app 的宿主專案中開啟 `Podfile` 檔案。

For full-Flutter apps, replace the following section:

如果是完整的 Flutter 專案，替換下面這段程式碼：

```ruby
target 'Runner' do
  use_frameworks!
  use_modular_headers!

  flutter_install_all_ios_pods File.dirname(File.realpath(__FILE__))
end
```

with:

為：

```ruby
use_frameworks!
use_modular_headers!
flutter_install_all_ios_pods File.dirname(File.realpath(__FILE__))

target 'Runner'
target '<name of your App Clip target>'
```

At the top of the file,
also uncomment `platform :ios, '11.0'` and set the
version to the lowest of the two target's iOS
Deployment Target.

在檔案的開始，需要把 `platform :ios, '9.0'` 的註釋解除，
並且為您的 2 個 target 設定最低可執行的 iOS 系統版本。

For add-to-app, add to:

如果是 add-to-app，緊跟下面的程式碼：

```ruby
target 'MyApp' do
  install_all_flutter_pods(flutter_application_path)
end
```

with:

新增：

```ruby
target 'MyApp' do
  install_all_flutter_pods(flutter_application_path)
end

target '<name of your App Clip target>'
  install_all_flutter_pods(flutter_application_path)
end
```

**9.2**

From the command line,
enter your Flutter project directory
and then install the pod:

在命令列中，目前工作目錄需要是您的 Flutter 專案目錄，然後安裝 pod：

```terminal
cd ios
pod install
```

## Run

## 執行

You can now run your App Clip target from Xcode by
selecting your App Clip target from the scheme drop-down,
selecting an iOS 14 device and pressing run.

您現在可以在 Xcode 的 scheme 下拉中選擇並執行您的 App Clip target 了，
選擇一個 iOS 14 的裝置並點選執行。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/run-select.png"
%}

To test launching an App Clip from the beginning,
also consult Apple's doc on
[Testing Your App Clip's Launch Experience][testing].

要從頭測試 App Clip 的啟動，您也可以檢視蘋果公司的文件
[測試您的 App Clip 的啟動體驗][testing]。

[testing]: {{site.apple-dev}}/documentation/app_clips/testing_your_app_clip_s_launch_experience

## Debugging, hot reload

## 除錯和熱重載

Unfortunately `flutter attach` cannot auto-discover the Flutter session
in an App Clip due to networking permission restrictions.

不幸的是，由於網路許可權的原因，`flutter attach` 
無法在 App clip 中自動發現 Flutter 會話。

In order to debug your App Clip and use functionalities
like hot reload, you must look for the Observatory URI
from the console output in Xcode after running.
[PENDING: Is this still true?]

為了除錯 App clip 並使用諸如熱重新載入之類別的功能，
必須在執行應用後從 Xcode 中的控制檯輸出中查詢 Observatory URI。

{% include docs/app-figure.md
image="development/platform-integration/ios-app-clip/observatory-uri.png"
%}

You must then copy paste it back into the
`flutter attach` command to connect.

您需要複製貼上它們到 `flutter attach` 來連線。

For example:

例如：

```terminal
flutter attach --debug-uri <copied URI>
```
