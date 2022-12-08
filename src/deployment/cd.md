---
title: Continuous delivery with Flutter
title: Flutter 裡的持續部署
description: How to automate continuous building and releasing of your Flutter app.
description: 如何自動的持續編譯建構和釋出你的 Flutter 應用。
tags: CI/CD,持續部署,釋出應用
keywords: fastlane
---

Follow continuous delivery best practices with Flutter to make sure your
application is delivered to your beta testers and validated on a frequent basis
without resorting to manual workflows.

透過 Flutter 持續交付的最佳實踐，
確保您的應用程式交付給您的 Beta 版本測試人員並能夠頻繁予以驗證，
而無需藉助手動工作流程。

## CI/CD Options

## CI/CD 選擇

There are a number of continuous integration (CI) and continuous delivery (CD)
options available to help automate the delivery of your application.

有許多持續整合 (CI) 和持續交付 (CD) 的工具，幫助自動釋出你的應用。

### All-in-one options with built-in Flutter functionality

### 內建 Flutter 的多合一 (All-in-one) 選擇：

* [Codemagic][]
* [Bitrise][]
* [Appcircle][]

### Integrating fastlane with existing workflows

### 使用 Fastlane 與現有工作流程整合

You can use fastlane with the following tooling:

你可以透過下面的工具使用 fastlane：

* [GitHub Actions][]
    * Example: Flutter Gallery's [Github Actions workflows][]

      範例：Flutter Galley 的 [GitHub Actions 工作流][Github Actions workflows]
    
    * Example: [Github Action in Flutter Project][]

      範例：[適用於 Flutter 專案的 GitHub Actions][Github Action in Flutter Project]

* [Cirrus][]
* [Travis][]
* [GitLab][]
* [CircleCI][]
    * [Building and deploying Flutter apps with Fastlane][]

This guide shows how to set up fastlane and then integrate it with 
your existing testing and continuous integration (CI) workflows. 
For more information, see "Integrating fastlane with existing workflow".

這份指南展示瞭如何讓設定 fastlane 以及
將其整合到現有應用的測試和持續整合 (CI) 工作流當中去。
更多相關的內容，請參考上面這部分的內容。

## fastlane

[fastlane][] is an open-source tool suite to automate releases and deployments 
for your app.

[fastlane][] 是一個開源工具套件，幫助你自動的打包正式版以及部署你的應用。

### Local setup

### 本地設定

It's recommended that you test the build and deployment process locally before
migrating to a cloud-based system. You could also choose to perform continuous
delivery from a local machine.

建議在遷移到基於雲計算的系統之前，先在本地測試其建構和部署流程。
您還可以使用本地機器執行連續交付。

1. Install fastlane `gem install fastlane` or `brew install fastlane`.
Visit the [fastlane docs][fastlane] for more info.

   安裝 fastlane `gem install fastlane` 或 `brew install fastlane`。
   存取 [fastlane docs][fastlane] 以獲得更多資訊。

1. Create an environment variable named `FLUTTER_ROOT`,
    and set it to the root directory of your Flutter SDK.
    (This is required for the scripts that deploy for iOS.)

   建立一個名為 `FLUTTER_ROOT` 的環境變數，並將其設定為 Flutter SDK 的根目錄。（這是為 iOS 部署的指令碼所必需的。）
    
1. Create your Flutter project, and when ready, make sure that your project builds via

   建立您的 Flutter 專案，準備就緒後，確保透過如下途徑建構專案：

    * ![Android]({{site.url}}/assets/images/docs/cd/android.png) `flutter build appbundle`;
    * ![iOS]({{site.url}}/assets/images/docs/cd/ios.png) `flutter build ipa`.

1. Initialize the fastlane projects for each platform.

   初始化各平台的 fastlane 專案：
   
    * ![Android]({{site.url}}/assets/images/docs/cd/android.png) In your `[project]/android`
    directory, run `fastlane init`.
      
      ![Android]({{site.url}}/assets/images/docs/cd/android.png)：在 `[project]/android` 目錄中，
      執行 `fastlane init` 命令。
    
    * ![iOS]({{site.url}}/assets/images/docs/cd/ios.png) In your `[project]/ios` directory,
    run `fastlane init`.
       
      ![iOS]({{site.url}}/assets/images/docs/cd/ios.png)：
      在 `[project]/ios` 目錄下，執行 `fastlane init` 命令。
    
1. Edit the `Appfile`s to ensure they have adequate metadata for your app.
   
   編輯 `Appfile` 以確保它有應用程式的基本資料配置：

    * ![Android]({{site.url}}/assets/images/docs/cd/android.png) Check that `package_name` in
    `[project]/android/fastlane/Appfile` matches your package name in AndroidManifest.xml.
    
      ![Android]({{site.url}}/assets/images/docs/cd/android.png) 檢查在 `[project]/android/fastlane/Appfile` 
      檔案中的 `package_name` 是否匹配在 AndroidManifest.xml 中的套件名稱。
    
    * ![iOS]({{site.url}}/assets/images/docs/cd/ios.png) Check that `app_identifier` in
    `[project]/ios/fastlane/Appfile` also matches Info.plist's bundle identifier. Fill in
    `apple_id`, `itc_team_id`, `team_id` with your respective account info.
    
      ![iOS]({{site.url}}/assets/images/docs/cd/ios.png) 檢查在 `[project]/ios/fastlane/Appfile` 中的
      `app_identifier` 是否匹配 Info.plist 檔案中的 bundle identifier。
      將相應的 `apple_id`、`itc_team_id` 和 `team_id` 輸入進去。
    
1. Set up your local login credentials for the stores.

   設定應用商店的本地登入憑據。
   
    * ![Android]({{site.url}}/assets/images/docs/cd/android.png) Follow the [Supply setup steps][]
    and ensure that `fastlane supply init` successfully syncs data from your
    Play Store console. _Treat the .json file like your password and do not check
    it into any public source control repositories._
    
      ![Android]({{site.url}}/assets/images/docs/cd/android.png) 按照 [Supply setup steps][] 文件操作，
      並且確保 `fastlane supply init` 成功同步了
      你在 Google Play 商店控制檯中的資料。
      **.json 檔案與密碼一樣重要，切勿將其公開在任何公共原始碼控制儲存庫。**
    
    * ![iOS]({{site.url}}/assets/images/docs/cd/ios.png) Your iTunes Connect username is already
    in your `Appfile`'s `apple_id` field. Set the `FASTLANE_PASSWORD` shell
    environment variable with your iTunes Connect password. Otherwise, you'll be
    prompted when uploading to iTunes/TestFlight.
    
      ![iOS]({{site.url}}//assets/images/docs/cd/ios.png)  iTunes Connect 使用者名稱已經存在於您的
      `Appfile` 的 `apple_id` 欄位中，
      你需要將你的 iTunes 密碼設定到 `FASTLANE_PASSWORD` 這個環境變數裡。
      否則，上傳到 iTunes/TestFlight時會提示你。
    
1. Set up code signing.

   設定程式碼簽名：

    * ![Android]({{site.url}}/assets/images/docs/cd/android.png) Follow the [Android app signing steps][].
    
      ![Android]({{site.url}}/assets/images/docs/cd/android.png) 參考文件 [為應用簽名][Android app signing steps]。

    * ![iOS]({{site.url}}/assets/images/docs/cd/ios.png) On iOS, create and sign using a
      distribution certificate instead of a development certificate when you're
      ready to test and deploy using TestFlight or App Store.
    
      ![iOS]({{site.url}}/assets/images/docs/cd/ios.png) 在iOS上，
      當您準備使用 TestFlight 或 App Store 進行測試和部署時，
      使用分發證書而不是開發證書進行建立和簽名。
      
        * Create and download a distribution certificate in your
          [Apple Developer Account console][].
        
          在 [Apple Developer Account console][] 建立並下載一個分發證書。
        
        * `open [project]/ios/Runner.xcworkspace/` and select the distribution
        certificate in your target's settings pane.
        
          開啟 `[project]/ios/Runner.xcworkspace/` 在你的專案設定裡選擇一個分發證書。
          
1. Create a `Fastfile` script for each platform.

   給每個不同的平台建立一個 `Fastfile` 指令碼。
   
    * ![Android]({{site.url}}/assets/images/docs/cd/android.png) On Android, follow the
      [fastlane Android beta deployment guide][].
      Your edit could be as simple as adding a `lane` that calls
      `upload_to_play_store`.
      Set the `aab` argument to `../build/app/outputs/bundle/release/app-release.aab`
      to use the app bundle `flutter build` already built.
    
      ![Android]({{site.url}}/assets/images/docs/cd/android.png) 在 Android 上按照
      [fastlane Android beta deployment guide][] 指引操作。
      你可以簡單的編輯一下檔案，加一個名叫 `upload_to_play_store` 的 `lane`。
      為了使用 `flutter build` 命令編譯 `aab`，
      要把 `apk` 引數設定為 `../build/app/outputs/bundle/release/app-release.aab`。
    
    * ![iOS]({{site.url}}/assets/images/docs/cd/ios.png) On iOS, follow the
      [fastlane iOS beta deployment guide][].
      You can specify the archive path to avoid rebuilding the project. For example:

      ![iOS]({{site.url}}/assets/images/docs/cd/ios.png) 在 iOS 上，按照
      [fastlane iOS beta 部署指南][fastlane iOS beta deployment guide] 指引操作。
      你可以指定 archive 的路徑以避免重複建構。例如：

      ```ruby
      build_app(
        skip_build_archive: true,
        archive_path: "../build/ios/archive/Runner.xcarchive",
      )
      upload_to_testflight
      ```

You're now ready to perform deployments locally or migrate the deployment
process to a continuous integration (CI) system.

你現在已準備好在本地執行部署或將部署過程遷移到持續整合（CI）系統。

### Running deployment locally

### 在本地執行部署

1. Build the release mode app.
   
   建構釋出模式的應用：

    * ![Android]({{site.url}}/assets/images/docs/cd/android.png) `flutter build appbundle`.
    * ![iOS]({{site.url}}/assets/images/docs/cd/ios.png) `flutter build ipa`.

1. Run the Fastfile script on each platform.

   在每個平臺上執行 Fastfile 指令碼。
   
    * ![Android]({{site.url}}/assets/images/docs/cd/android.png) `cd android` then
    `fastlane [name of the lane you created]`.
    * ![iOS]({{site.url}}/assets/images/docs/cd/ios.png) `cd ios` then
    `fastlane [name of the lane you created]`.

### Cloud build and deploy setup

### 雲建構和部署設定

First, follow the local setup section described in 'Local setup' to make sure
the process works before migrating onto a cloud system like Travis.

首先，按照“本地設定”中描述的本地設定部分，確保在遷移到 Travis 等雲系統之前，該過程有效。

The main thing to consider is that since cloud instances are ephemeral and
untrusted, you won't be leaving your credentials like your Play Store service
account JSON or your iTunes distribution certificate on the server.

需要考慮的主要事項是，由於雲實例是短暫且不可信的，
因此你不能在伺服器上保留你的憑據，
如 Play Store 服務帳戶 JSON 或 iTunes 分發證書。

Continuous Integration (CI) systems generally support encrypted environment 
variables to store private data. You can pass these environment variables 
using `--dart-define MY_VAR=MY_VALUE` while building the app.

持續整合 (CI) 系統通常支援加密的環境變數來儲存私有資料。
你可以使用 `--dart-define MY_VAR=MY_VALUE` 在建構應用時傳遞環境變數。

**Take precaution not to re-echo those variable values back onto the console in
your test scripts**. Those variables are also not available in pull requests
until they're merged to ensure that malicious actors cannot create a pull
request that prints these secrets out. Be careful with interactions with these
secrets in pull requests that you accept and merge.

**採取預防措施，不要在測試指令碼中將這些變數值重新回顯到控制檯。**
在合併之前，這些變數在拉取請求中也不可用，
以確保惡意行為者無法建立列印這些金鑰的拉取請求。
在接受和合並的 pull 請求中，請注意與這些金鑰。

1. Make login credentials ephemeral.

   暫時性登入憑據。
   
    * ![Android]({{site.url}}/assets/images/docs/cd/android.png) On Android:
    
      ![Android]({{site.url}}/assets/images/docs/cd/android.png 在 Android 上：
      
        * Remove the `json_key_file` field from `Appfile` and store the string
          content of the JSON in your CI system's encrypted variable. 
          Read the environment variable directly in your `Fastfile`.
          
          從 `Appfile` 中刪除 `json_key_file` 並將其儲存在 CI 系統的加密變數裡。
          從 `Fastfile` 中直接讀取這些環境變數。

          ```
          upload_to_play_store(
            ...
            json_key_data: ENV['<variable name>']
          )
          ```
        * Serialize your upload key (for example, using base64) and save it as
          an encrypted environment variable. You can deserialize it on your CI
          system during the install phase with
          
          序列化您的上傳金鑰（例如，使用 base64）並將其另存為加密環境變數。
          可以可以在安裝階段在 CI 系統上對其進行反序列化
          
          ```bash
          echo "$PLAY_STORE_UPLOAD_KEY" | base64 --decode > [path to your upload keystore]
          ```
     * ![iOS]({{site.url}}/assets/images/docs/cd/ios.png) On iOS:
    
       ![iOS]({{site.url}}/assets/images/docs/cd/ios.png) 在 iOS 上:
      
        * Move the local environment variable `FASTLANE_PASSWORD` to use
        encrypted environment variables on the CI system.
        
          將本地環境變數 `FASTLANE_PASSWORD` 轉而使用 CI 系統的加密的環境變數。
        
        * The CI system needs access to your distribution certificate. fastlane's
        [Match][] system is
        recommended to synchronize your certificates across machines.
        
          CI 系統需要有許可權拿到你的分發證書。建議使用fastlane 的 [Match][] 系統在不同的機器上同步你的證書。

2. It's recommended to use a Gemfile instead of using an indeterministic
`gem install fastlane` on the CI system each time to ensure the fastlane
dependencies are stable and reproducible between local and cloud machines. However, this step is optional.
   
   建議每次使用 Gemfile 而不是 `gem install fastlane` 以避免其在 CI 系統上使用的不確定性 ，
   以確保 fastlane 依賴關係在本地和雲計算機之間穩定且可重現。但是，此步驟是可選的。

    * In both your `[project]/android` and `[project]/ios` folders, create a
    `Gemfile` containing the following content:
    
      在 `[project]/android` 和 `[project]/ios` 資料夾中，建立一個 `Gemfile` 包含以下內容：
      
      ```
      source "https://rubygems.org"
      gem "fastlane"
      ```
    * In both directories, run `bundle update` and check both `Gemfile` and
    `Gemfile.lock` into source control.
    
      在兩個目錄中，執行 `bundle update` 並將兩者的 `Gemfile` 和 `Gemfile.lock` 檔案納入原始碼管理。
      
    * When running locally, use `bundle exec fastlane` instead of `fastlane`.
    
      當你在本地執行的時候,請使用 `bundle exec fastlane` 而不是 `fastlane`。

3. Create the CI test script such as `.travis.yml` or `.cirrus.yml` in your
   repository root.

   在你的儲存庫根目錄建立一個 CI 測試指令碼，
   例如: `.travis.yml` 或 `.cirrus.yml`。

    * See [fastlane CI documentation][] for CI specific setup.

      有關特定於 CI 的設定，請參見 [fastlane CI 文件][fastlane CI documentation]。

    * Shard your script to run on both Linux and macOS platforms.

      分開你的指令碼以便能在 Linux 和 macOS 兩個平台執行。

    * During the setup phase of the CI task, do the following:

      在 CI 的設定階段，執行下列內容：

         * Ensure Bundler is available using `gem install bundler`.

           透過執行 `gem install bundler` 確保 Bundler 可用。

         * Run `bundle install` in `[project]/android` or `[project]/ios`.

           在 `[project]/android` 或 `[project]/ios` 目錄下分別執行 `bundle install`命令。

         * Make sure the Flutter SDK is available and set in `PATH`.
         
           確保 Flutter SDK 已經正確了設定在了 `PATH` 環境變數中。

         * For Android, ensure the Android SDK is available and the `ANDROID_SDK_ROOT`
           path is set.

           在 Android 平臺上，請確保已經設定正確的 `ANDROID_SDK_ROOT` 環境變數。

         * For iOS, you may have to specify a dependency on Xcode (for example
           `osx_image: xcode9.2`).
           
           在 iOS 平臺上，你需要為 Xcode 指定依賴 (比如: `osx_image: xcode9.2`)

    * In the script phase of the CI task:
    
      在 CI 任務的指令碼階段：
      
         * Run `flutter build appbundle` or
           `flutter build ios --release --no-codesign`,
           depending on the platform.
         
           根據平台的不同可以執行 `flutter build appbundle` 或者
           `flutter build ios --release --no-codesign`。
   
         * `cd android` or `cd ios`.
         
           然後執行 `cd android` 或 `cd ios` 命令。
           
         * `bundle exec fastlane [name of the lane]`.
         
           最後執行 `bundle exec fastlane [name of the lane]` 命令。

## Xcode Cloud

[Xcode Cloud][] is a continuous integration and delivery service for building,
testing, and distributing apps and frameworks for Apple platforms.

[Xcode Cloud][] 是一項為分發 Apple 平台的持續建構整合並交付，以及測試分發的服務。

### Requirements

### 需要準備的

* Xcode 13.4.1 or higher.

  Xcode 13.4.1 或更高版本。

* Be enrolled in the [Apple Developer Program][].

  加入 [Apple 開發者計劃][Apple Developer Program]。

### Custom build script

### 自訂建構指令碼

Xcode Cloud recognizes [custom build scripts][] that can be 
used to perform additional tasks at a designated time. It also includes a set
of [predefined environment variables][], such as `$CI_WORKSPACE`, which is the
location of your cloned repository.

Xcode Cloud 可以識別 [自訂的建構指令碼][custom build scripts] 用於在特定階段執行額外的任務。
同時它還包含了一系列 [預定義的環境變數][predefined environment variables]，
例如用於你 clone 儲存庫地址的 `$CI_WORKSPACE`。

{{site.alert.note}}

  The temporary build environment that Xcode Cloud uses includes tools that are
  part of macOS and Xcode&mdash;for example, Python&mdash;and additionally Homebrew to
  support installing third-party dependencies and tools.

  Xcode Cloud 使用的臨時建構環境工具是 macOS 和 Xcode 的一部分（例如 Python），
  另外還有 Homebrew 以支援安裝第三方依賴項和工具。

{{site.alert.end}}

#### Post-clone script

#### Post-clone 指令碼

Leverage the post-clone custom build script that runs after
Xcode Cloud clones your Git repository using the following instructions:

利用 post-clone 執行的自訂建構指令碼
Xcode Cloud 按照以下說明複製你的 Git 儲存庫：

Create a file at `ios/ci_scripts/ci_post_clone.sh` and add the content below.

在 `ios/ci_scripts/ci_post_clone.sh` 建立一個檔案，然後新增下面的內容。

<?code-excerpt "deployment/xcode_cloud/ci_post_clone.sh"?>
```sh
#!/bin/sh

# The default execution directory of this script is the ci_scripts directory.
cd $CI_WORKSPACE # change working directory to the root of your cloned repo.

# Install Flutter using git.
git clone https://github.com/flutter/flutter.git --depth 1 -b stable $HOME/flutter
export PATH="$PATH:$HOME/flutter/bin"

# Install Flutter artifacts for iOS (--ios), or macOS (--macos) platforms.
flutter precache --ios

# Install Flutter dependencies.
flutter pub get

# Install CocoaPods using Homebrew.
HOMEBREW_NO_AUTO_UPDATE=1 # disable homebrew's automatic updates.
brew install cocoapods

# Install CocoaPods dependencies.
cd ios && pod install # run `pod install` in the `ios` directory.

exit 0
```

This file should be added to your git repository and marked as executable.

該檔案需要加入 git 儲存庫管理，並給予可執行許可權。

```terminal
$ git add --chmod=+x ios/ci_scripts/ci_post_clone.sh
```

### Workflow configuration

### 工作流配置

An [Xcode Cloud workflow][] defines the steps performed in the CI/CD process
when your workflow is triggered.

[Xcode Cloud workflow][] 定義了你工作流觸發時 CI/CD 處理處理序的執行步驟。

{{site.alert.note}}

  This requires that your project is already initialized with Git
  and linked to a remote repository.

  你需要保證專案已經進行 Git 初始化，並關聯到遠端儲存庫。

{{site.alert.end}}

To create a new workflow in Xcode, use the following instructions:

要在 Xcode 中建立一個工作流，請參考以下步驟：

1. Choose **Product > Xcode Cloud > Create Workflow** to open the
   **Create Workflow** sheet.

   選擇 **Product > Xcode Cloud > Create Workflow** 以開啟 **Create Workflow** 選單。

2. Select the product (app) that the workflow should be attached to, then click
   the **Next** button.

   選擇工作流需要作用的生產應用，然後點選 **Next** 按鈕。

3. The next sheet displays an overview of the default workflow provided by Xcode,
    and can be customized by clicking the **Edit Workflow** button.

   下一步，選單將會展示一個 Xcode 提供的預設工作流的浮層，
   然後可以透過點選 **Edit Workflow** 按鈕進行客製。

#### Branch changes

#### 變更分支

By default Xcode suggests the Branch Changes condition that starts a new build
for every change to your Git repository’s default branch.

預設 Xcode 建議每次分支變更後都為你儲存庫的預設分支開始一個全新的建構。

For your app's iOS variant, it's reasonable that you would want Xcode Cloud to
trigger your workflow after you've made changes to your flutter packages, or
modified either the Dart or iOS source files within the `lib\` and `ios\`
directories.

對於你應用的 iOS 變體，你通常會希望 Xcode Cloud 在對你的
Flutter packages 修改了 `lib\` 中的 Dart 或 `ios\` 中的 iOS 原始檔目錄之後，
觸發你的工作流。

This can be achieved by using the following Files and Folders conditions:

這可以透過使用下列檔案和資料夾條件來實現：

![Xcode Workflow Branch Changes]({{site.url}}/assets/images/docs/releaseguide/xcode_workflow_branch_changes.png){:width="100%"}

### Next build number

### 下次建構的建構版本數字

Xcode Cloud defaults the build number for new workflows to `1` and increments
it per successful build. If you're using an existing app with a higher build
number, you'll need to configure Xcode Cloud to use the correct build number
for it's builds by simply specifying the `Next Build Number` in your iteration.

Xcode Cloud 對於新的工作流來說預設的建構版本數字是 `1`，
然後在每次成功建構後遞增。如果你已經在一個已有應用中，
使用了一個更高的建構版本數字，你需要配置 Xcode Cloud 使用正確的建構版本數字，
只需要簡單透過指定 `Next Build Number` 用於迭代即可。

Check out [Setting the next build number for Xcode Cloud builds][] for more
information.

你可以在 [設定 Xcode Cloud 建構下一次的建構版本數字][Setting the next build number for Xcode Cloud builds]
檢視更多資訊。

[Android app signing steps]: {{site.url}}/deployment/android#signing-the-app
[Appcircle]: https://appcircle.io/blog/guide-to-automated-mobile-ci-cd-for-flutter-projects-with-appcircle/
[Apple Developer Account console]: {{site.apple-dev}}/account/ios/certificate/
[Bitrise]: https://devcenter.bitrise.io/getting-started/getting-started-with-flutter-apps/
[CI Options and Examples]: #reference-and-examples
[Cirrus]: https://cirrus-ci.org
[Cirrus script]: {{site.repo.flutter}}/blob/master/.cirrus.yml
[Codemagic]: https://blog.codemagic.io/getting-started-with-codemagic/
[fastlane]: https://docs.fastlane.tools
[fastlane Android beta deployment guide]: https://docs.fastlane.tools/getting-started/android/beta-deployment/
[fastlane CI documentation]: https://docs.fastlane.tools/best-practices/continuous-integration
[fastlane iOS beta deployment guide]: https://docs.fastlane.tools/getting-started/ios/beta-deployment/
[Flutter Gallery Project]: {{site.repo.gallery}}
[Github Action in Flutter Project]: {{site.github}}/nabilnalakath/flutter-githubaction
[GitHub Actions]: {{site.github}}/features/actions
[Github Actions workflows]: {{site.repo.gallery}}/tree/main/.github/workflows
[GitLab]: https://docs.gitlab.com/ee/ci/README.html#doc-nav
[CircleCI]: https://circleci.com
[Building and deploying Flutter apps with Fastlane]: https://circleci.com/blog/deploy-flutter-android
[Match]: https://docs.fastlane.tools/actions/match/
[Supply setup steps]: https://docs.fastlane.tools/getting-started/android/setup/#setting-up-supply
[Travis]: https://travis-ci.org/
[Apple Developer Program]: https://developer.apple.com/programs
[Xcode Cloud]: https://developer.apple.com/xcode-cloud
[Xcode Cloud workflow]: https://developer.apple.com/documentation/xcode/xcode-cloud-workflow-reference
[custom build scripts]: https://developer.apple.com/documentation/xcode/writing-custom-build-scripts
[predefined environment variables]: https://developer.apple.com/documentation/xcode/environment-variable-reference
[Setting the next build number for Xcode Cloud builds]: https://developer.apple.com/documentation/xcode/setting-the-next-build-number-for-xcode-cloud-builds#Set-the-next-build-number-to-a-custom-value
