## iOS setup

## 設定 iOS 開發環境

### Install Xcode

### 安裝 Xcode

To develop Flutter apps for iOS, you need a Mac with Xcode installed.

開發 iOS 平臺上的 Flutter 應用，你需要一個安裝了 Xcode 的 Mac 裝置。

 1. Install the latest stable version of Xcode
    (using [web download][] or the [Mac App Store][]).

    透過 [直接下載][web download] 或者透過 [Mac App Store][]
    來安裝最新穩定版 Xcode；

 1. To configure the Xcode command-line tools to use the
    installed version, run the following commands.

    配置 Xcode 命令列工具以使用新安裝的 Xcode 版本。
    從命令列中執行以下命令：

    ```terminal
    $ sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
    $ sudo xcodebuild -runFirstLaunch
    ```

    To use the latest version of Xcode, use this path.
    If you need to use a different version, specify that path instead.

    當你安裝了最新版本的 Xcode，大部分情況下，上面的路徑都是一樣的。
    但如果你安裝了不同版本的 Xcode，你可能要更改一下上述命令中的路徑。

 1. Sign the Xcode license agreement.
    To sign the SLA, either:

    同意 Xcode 的許可協議：

    {: type="a"}
    1. Open Xcode and confirm.

       執行一次 Xcode。

    1. Open the Terminal and run:

       在終端輸入命令並執行：

       ```terminal
       $ sudo xcodebuild -license
       ```

Versions older than the latest stable version might still work,
but are not recommended for Flutter development.

舊版本可能也能夠正常工作，但是不建議在 Flutter 開發環境中使用。
舊版本的 Xcode 不支援定位程式碼，還可能無法正常工作。

With Xcode, you can run Flutter apps on an iOS device or on the simulator.

安裝了 Xcode 之後，你就可以在 iOS 真機或者模擬器上執行 Flutter 應用了。

### Set up the iOS simulator

### 配置 iOS 模擬器

To prepare to run and test your Flutter app on the iOS simulator,
follow this procedure.

如果想要在 iOS 模擬器中執行和測試 Flutter 應用，按照以下步驟即可：

 1. If using Xcode 15 or greater, download and install the iOS Simulator 
    by running the following command:

    如果使用 Xcode 15 或更高的版本，
    請執行以下指令下載並安裝 iOS 模擬器：

    ```terminal
    $ xcodebuild -downloadPlatform iOS
    ```

    If you want to use a different method of downloading and installing the 
    iOS Simulator, check out 
    [Apple's documentation on installing Simulators][] for more options.

    如果你想使用其他方式下載和安裝 iOS 模擬器，請查閱 
    [Apple 文件中的安裝模擬器][Apple's documentation on installing Simulators] 
    瞭解更多方案。

 1. To start the Simulator, run the following command:

    執行以下指令啟動模擬器：

    ```terminal
    $ open -a Simulator
    ```

 1. Set your Simulator to use a 64-bit device (iPhone 5s or later).

    將模擬器設定使用為 64 位裝置（iPhone 5s 或更新的裝置）。

    - From Xcode, choose a simulator device type. Go to
      **Product** <span aria-label="and then">></span>
      **Destination** <span aria-label="and then">></span>
      Choose your target device.

      透過 Xcode 選擇模擬器裝置型別。
      開啟 **Product** <span aria-label="and then">></span>
      **Destination** <span aria-label="and then">></span> 
      選擇目標裝置。

    - From the Simulator app, go to
      **File** <span aria-label="and then">></span>
      **Open Simulator** <span aria-label="and then">></span>
      Choose your target iOS device

      透過 Simulator app 選擇模擬器裝置。
      開啟 **File** <span aria-label="and then">></span>
      **Open Simulator** <span aria-label="and then">></span> 
      選擇目標裝置。

    - To check the device version in the Simulator,
      open the **Settings** app <span aria-label="and then">></span>
      **General** <span aria-label="and then">></span>
      **About**.

      在模擬器中檢查裝置版本。
      開啟 **Settings** app <span aria-label="and then">></span>
      **General** <span aria-label="and then">></span>
      **About**。

 1. The simulated high-screen density iOS devices might overflow your screen.
    If that appears true on your Mac, change the presented size in the
    Simulator app

    模擬的高解析度 iOS 裝置可能會溢位你的螢幕。
    如果在 Mac 上出現這種情況，
    請在 Simulator app 中更改顯示大小。

    - To display the Simulator at a small size, go to
      **Window** <span aria-label="and then">></span>
      **Physical Size** or<br>press <kbd>Cmd</kbd> + <kbd>1</kbd>.

      以小尺寸顯示模擬器，
      設定 **Window** <span aria-label="and then">></span>
      **Physical Size** 或者<br>快捷鍵 <kbd>Cmd</kbd> + <kbd>1</kbd>。

    - To display the Simulator at a moderate size, go to
      **Window** <span aria-label="and then">></span>
      **Point Accurate** or<br>press <kbd>Cmd</kbd> + <kbd>2</kbd>.

      以中尺寸顯示模擬器，
      設定 **Window** <span aria-label="and then">></span>
      **Point Accurate** 或者<br>快捷鍵 <kbd>Cmd</kbd> + <kbd>2</kbd>。

    - To display the Simulator at an HD representation, go to
      **Window** <span aria-label="and then">></span>
      **Pixel Accurate** or<br>press <kbd>Cmd</kbd> + <kbd>3</kbd>.
      _The Simulator defaults to this size._

      以 HD 高畫質顯示模擬器，
      設定 **Window** <span aria-label="and then">></span>
      **Pixel Accurate** 或者<br>快捷鍵 <kbd>Cmd</kbd> + <kbd>3</kbd>。

    - The Simulator defaults to **Fit Screen**.
      If you need to return to that size, go to
      **Window** <span aria-label="and then">></span>
      **Fit Screen** or press <kbd>Cmd</kbd> + <kbd>4</kbd>.

      模擬器預設為 **Fit Screen（適應螢幕）**。
      如果你需要恢復到該尺寸的設定，
      設定 **Window** <span aria-label="and then">></span>
      **Fit Screen** 或者<br>快捷鍵 <kbd>Cmd</kbd> + <kbd>4</kbd>。

### Deploy to physical iOS devices

### 部署到 iOS 真機

To deploy your Flutter app to a physical iPhone or iPad,
you need to do the following:

你需要執行以下操作，
將 Flutter 應用程式部署到 iPhone 或 iPad 真機上：

- Create an [Apple Developer][] account.

  建立 [Apple Developer][] 賬戶。

- Set up physical device deployment in Xcode.

  在 Xcode 中設定真機裝置部署。

- Create a development provisioning profile to self-sign certificates.

  建立開發配置檔案 (Provisioning Profile)，
  並自行簽署證書 (Signing Certificate)。

- Install the third-party CocoaPods dependency manager
  if your app uses Flutter plugins.

  如果你的應用程式使用 Flutter 外掛，
  請安裝第三方 CocoaPods 依賴管理器。

#### Create your Apple ID and Apple Developer account

#### 建立 Apple ID 和 Apple Developer 賬戶

To test deploying to a physical iOS device, you need an Apple ID.

測試部署到 iOS 真機，你需要一個 Apple ID。

To distribute your app to the App Store,
you must enroll in the Apple Developer Program.

要在 App Store 釋出你的應用程式，
你必須註冊 Apple Developer Program。

If you only need to test deploying your app,
complete the first step and move on to the next section.

如果你只需要測試部署應用程式，
請完成第 1 步後繼續下一節。

1. If you don't have an [Apple ID][], create one.

   如果你沒有 [Apple ID][]，請建立一個。

1. If you haven't enrolled in the [Apple Developer][] program, enroll now.

   如果你未註冊 [Apple Developer][] program，請立即註冊。

   To learn more about membership types,
   check out [Choosing a Membership][].

   瞭解有關會員型別的更多資訊，
   請查閱 [選擇會員資格][Choosing a Membership]。

[Apple ID]: https://support.apple.com/en-us/HT204316

#### Attach your physical iOS device to your Mac {#attach}

#### 將 iOS 真機連線到 Mac {#attach}

Configure your physical iOS device to connect to Xcode.

配置你的 iOS 真機連線到 Xcode。

1. Attach your iOS device to the USB port on your Mac.

   將 iOS 裝置連線到 Mac 的 USB 埠。

1. On first connecting your iOS device to your Mac,
   your iOS device displays the **Trust this computer?** dialog.

   首次將 iOS 裝置連線到 Mac 時，
   你的 iOS 裝置會顯示 **信任這臺電腦嗎？** 的對話方塊。

1. Click **Trust**.

   點選 **信任**。

   ![Trust Mac][]{:.mw-100}

1. When prompted, unlock your iOS device.

   出現提示時，解鎖你的 iOS 裝置。

#### Enable Developer Mode on iOS 16 or later
{:.no_toc}

#### 在 iOS 16 或更高版本上啟用開發者模式
{:.no_toc}

Starting with iOS 16, Apple requires you to enable **[Developer Mode][]**
to protect against malicious software.
Enable Developer Mode before deploying to a device running iOS 16 or later.

從 iOS 16 開始，Apple 要求你啟用 **[開發者模式][Developer Mode]**，
以防止惡意軟體。
在部署到 iOS 16 或更高版本的裝置之前，請啟用開發者模式。

1. Tap on **Settings** <span aria-label="and then">></span>
   **Privacy & Security** <span aria-label="and then">></span>
   **Developer Mode**.

   點選 **設定** <span aria-label="and then">></span>
   **隱私與安全性** <span aria-label="and then">></span>
   **開發者模式**。

1. Tap to toggle **Developer Mode** to **On**.

   將 **開發者模式** 切換為 **開啟**。

1. Tap **Restart**.

   點選 **重新啟動**。

1. After the iOS device restarts, unlock your iOS device.

   重新啟動 iOS 裝置後，解鎖 iOS 裝置。

1. When the **Turn on Developer Mode?** dialog appears, tap **Turn On**.

   當出現 **開啟開發者模式嗎？** 對話方塊時，點選 **開啟**。

   The dialog explains that Developer Mode requires reducing the security
   of the iOS device.

   對話方塊會提示開發者模式會降低 iOS 裝置的安全性。
   
1. Unlock your iOS device.

   解鎖你的 iOS 裝置。

#### Enable developer code signing certificates

#### 啟用開發者程式碼簽名證書 (signing certificates)

To deploy to a physical iOS device, you need to establish trust with your
Mac and the iOS device.
This requires you to load signed developer certificates to your iOS device.
To sign an app in Xcode,
you need to create a development provisioning profile.

在部署到 iOS 真機前，你需要在 Mac 與 iOS 裝置之間建立信任。
這需要將簽名的開發者證書載入到 iOS 裝置上。
在 Xcode 中籤署應用程式，
你需要建立一個開發者配置檔案 (Provisioning Profile)。

Follow the Xcode signing flow to provision your project.

請按照 Xcode 簽名流程配置你的專案。

1. Open Xcode.

   開啟 Xcode。

1. Sign in to Xcode with your Apple ID.

   使用 Apple ID 登入 Xcode。

   {: type="a"}
   1. Go to **Xcode** <span aria-label="and then">></span>
      **Settings...*

      選擇 **Xcode** <span aria-label="and then">></span>
      **Settings...*

   1. Click **Accounts**.

      點選 **Accounts**。

   1. Click **+**.

      點選 **+**。

   1. Select **Apple ID** and click **Continue**.

      選擇 **Apple ID** 並點選 **Continue**。

   1. When prompted, enter your **Apple ID** and **Password**.

      出現提示時，請輸入你的 **Apple ID** 和 **Password**。

   1. Close the **Settings** dialog.

      關閉 **Settings** 對話方塊。

   Development and testing supports any Apple ID.

   開發和測試支援任意 Apple ID。

1. Go to **File** <span aria-label="and then">></span> **Open...**

   開啟 **File** <span aria-label="and then">></span> **Open...**

   You can also press <kbd>Cmd</kbd> + <kbd>O</kbd>.

   你還可以使用快捷鍵 <kbd>Cmd</kbd> + <kbd>O</kbd>。

1. Navigate to your Flutter project directory.

   導航至 Flutter 專案目錄。

1. Open the default Xcode workspace in your project: `ios/Runner.xcworkspace`.

   開啟專案中預設的 Xcode workspace：`ios/Runner.xcworkspace`。

1. Select the physical iOS device you intend to deploy to in the device
   drop-down menu to the right of the run button.

   在執行按鈕右側的裝置下拉選單中選擇你要部署的 iOS 真機。

   It should appear under the **iOS devices** heading.

   它應該出現在 **iOS devices** 標題下方。

1. In the left navigation panel under **Targets**, select **Runner**.

   在左側導航面板的 **Targets** 下，選擇 **Runner**。

1. In the **Runner** settings pane, click **Signing & Capabilities**.

   在 **Runner** 設定窗內，點選 **Signing & Capabilities**。

1. Select **All** at the top.

   選擇頂部的 **All**。

1. Select **Automatically manage signing**.

   選擇 **Automatically manage signing**。

1. Select a team from the **Team** dropdown menu.

   從 **Team** 下拉選單中選擇一個團隊。

   Teams are created in the **App Store Connect** section of your
   [Apple Developer Account][] page.
   If you have not created a team, you can choose a _personal team_.

   團隊是在 [Apple Developer Account][] 頁面的 **App Store Connect** 建立的。
   如果你尚未建立團隊，可以選擇 _個人團隊 (personal team)_。

   The **Team** dropdown displays that option as **Your Name (Personal Team)**.

   **Team** 下拉選單中會顯示名為 **你的名稱 (Personal Team)** 的選項。

   ![Xcode account add][]{:.mw-100}

   After you select a team, Xcode performs the following tasks:

   選擇團隊後，Xcode 會執行以下工作。

   {: type="a"}
   1. Creates and downloads a Development Certificate

      建立並下載開發證書

   1. Registers your device with your account,

      將設備註冊到你的賬戶

   1. Creates and downloads a provisioning profile if needed

      根據需要建立並下載配置檔案 (Provisioning Profile)

If automatic signing fails in Xcode, verify that the project's
**General** <span aria-label="and then">></span>
**Identity** <span aria-label="and then">></span>
**Bundle Identifier** value is unique.

如果在 Xcode 中自動簽名失敗，請檢查專案的 **General** <span aria-label="and then">></span>
**Identity** <span aria-label="and then">></span>
**Bundle Identifier** 值是否唯一。

![Check the app's Bundle ID][]{:.mw-100}

#### Enable trust of your Mac and iOS device {#trust}

#### 啟用 Mac 和 iOS 裝置之間的信任 {#trust}

When you attach your physical iOS device for the first time,
enable trust for both your Mac and the Development Certificate
on the iOS device.

首次連線 iOS 真機時，
為你的 Mac 和 iOS 裝置上的開發證書啟用信任。

##### Agree to trust your Mac

##### 同意信任你的 Mac

You should enabled trust of your Mac on your iOS device when
you [attached the device to your Mac](#attach).

將你的裝置連線到 Mac 時，需要 [啟用 iOS 裝置對 Mac 的信任](#attach)。

##### Enable developer certificate for your iOS devices

##### 為 iOS 裝置啟用開發者證書

Enabling certificates varies in different versions of iOS.

在不同版本的 iOS 中，啟用證書的方式也不盡相同。

{% comment %} Nav tabs {% endcomment -%}
<ul class="nav nav-tabs" id="ios-versions" role="tablist">
    <li class="nav-item">
        <a class="nav-link" id="ios14-tab" href="#ios14" role="tab" aria-controls="ios14" aria-selected="true">iOS 14</a>
    </li>
    <li class="nav-item">
        <a class="nav-link" id="ios15-tab" href="#ios15" role="tab" aria-controls="ios15" aria-selected="false">iOS 15</a>
    </li>
    <li class="nav-item">
        <a class="nav-link active" id="ios16-tab" href="#ios16" role="tab" aria-controls="ios16" aria-selected="false">iOS 16 or later</a>
    </li>
</ul>

{% comment %} Tab panes {% endcomment -%}
<div class="tab-content">

<div class="tab-pane" id="ios14" role="tabpanel" aria-labelledby="ios14-tab" markdown="1">

1. Open the **Settings** app on the iOS device.

   開啟 iOS 裝置上的 **設定**。

1. Tap on **General** <span aria-label="and then">></span>
   **Profiles & Device Management**.

   點選 **通用** <span aria-label="and then">></span>
   **裝置管理**。

1. Tap to toggle your Certificate to **Enable**

   點選你的證書切換為 **啟用**。

</div>

<div class="tab-pane" id="ios15" role="tabpanel" aria-labelledby="ios15-tab" markdown="1">

1. Open the **Settings** app on the iOS device.

   開啟 iOS 裝置上的 **設定**。

1. Tap on **General** <span aria-label="and then">></span>
    **VPN & Device Management**.

   點選 **通用** <span aria-label="and then">></span>
   **VPN 與 裝置管理**。

1. Tap to toggle your Certificate to **Enable**.

   點選你的證書切換為 **啟用**。

</div>

<div class="tab-pane active" id="ios16" role="tabpanel" aria-labelledby="ios16-tab" markdown="1">

1. Open the **Settings** app on the iOS device.

   開啟 iOS 裝置上的 **設定**。

1. Tap on **General** <span aria-label="and then">></span>
    **VPN & Device Management**.

   點選 **通用** <span aria-label="and then">></span>
   **VPN 與 裝置管理**。

1. Under the **Developer App** heading, you should find your certificate.

   在 **開發者應用** 標題下，你需要找到你的證書。

1. Tap your Certificate.

   點選你的證書。

1. Tap **Trust "\<certificate\>"**.

   點選 **信任 "\<certificate\>"**。

1. When the dialog displays, tap **Trust**.

   顯示對話方塊時，點選 **信任**。

</div>
</div>{% comment %} End: Tab panes. {% endcomment -%}

If prompted, enter your Mac password into the
**codesign wants to access key...** dialog and tap **Always Allow**.

如果出現提示，請在 **codesign 想要存取金鑰...** 的對話方塊中輸入 Mac 密碼，
然後點選 **始終允許**。

#### Optional deployment procedures

#### 可選的部署步驟

You can skip these procedures. They enable additional debugging features.

你可以跳過這些步驟。
它們可以啟用更多除錯功能。

##### Set up wireless debugging on your iOS device

##### 在 iOS 裝置上設定無線除錯

Follow this procedure to debug your device using a Wi-Fi connection.

按照以下步驟使用 Wi-Fi 連線除錯裝置。

To use wireless debugging:

使用無線除錯：

- Connect your iOS device to the same network as your macOS device.

  將 iOS 裝置連線到與 macOS 裝置相同的網路。

- Set a passcode for your iOS device.

  為 iOS 裝置設定密碼。

After you connect your iOS device to your Mac:

將 iOS 裝置連線到 Mac 後：

1. Open **Xcode**.

   開啟 **Xcode**。

1. Go to **Window** <span aria-label="and then">></span>
   **Devices and Simulators**.

   選擇 **Window** <span aria-label="and then">></span>
   **Devices and Simulators**。

   You can also press <kbd>Shift</kbd> + <kbd>Cmd</kbd> + <kbd>2</kbd>.

   你還可以使用快捷鍵 <kbd>Shift</kbd> + <kbd>Cmd</kbd> + <kbd>2</kbd>。

1. Select your iOS device.

   選擇你的 iOS 裝置。

1. Select **Connect via Network**.

   選擇 **Connect via Network**。

1. Once the network icon appears next to the device name,
   unplug your iOS device from your Mac.

   一旦裝置名稱旁邊出現網路圖示，
   請將 iOS 裝置從 Mac 拔下。

If you don't see your device listed when using `flutter run`,
extend the timeout. The timeout defaults to 10 seconds.
To extend the timeout, change the value to an integer greater than 10.

如果在使用 `flutter run` 時沒有看到裝置列表，請延長超時時間。
超時預設為 10 秒。
要延長超時時間，請將值改為大於 10 的整數。

```terminal
flutter run --device-timeout 60
```

###### Learn more about wireless debugging

###### 進一步瞭解無線除錯

- To learn more, check out
  [Apple's documentation on pairing a wireless device with Xcode][].

  瞭解更多資訊，請查閱 
  [Apple 文件中的 pairing a wireless device with Xcode][Apple's documentation on pairing a wireless device with Xcode]。

- To troubleshoot, check out [Apple's Developer Forums][].

  要排除故障，請存取 [Apple's Developer Forums][]。

- To learn how to configure wireless debugging with `flutter attach`,
  check out [Debugging your add-to-app module][].

  要了解如何使用 `flutter attach` 配置無線除錯，
  請查閱 [在混合開發模式下進行除錯][Debugging your add-to-app module]。


##### Install CocoaPods

##### 安裝 CocoaPods

Follow this procedure if your apps depend on [Flutter plugins][]
with native iOS code.

如果你的應用程式依賴於帶有原生 iOS 程式碼的 [Flutter 外掛][Flutter plugins]，
請遵循此步驟。

To [Install and set up CocoaPods][], run the following commands:

請執行以下指令，[安裝並設定 CocoaPods][Install and set up CocoaPods]：

```terminal
$ sudo gem install cocoapods
```

{{site.alert.note}}

  The default version of Ruby requires `sudo` to install the CocoaPods gem.
  If you are using a Ruby Version manager, you might need to run without `sudo`.

  Ruby 的預設版本需要 root 許可權 `sudo` 來安裝 CocoaPods gem，
  如果你使用的是 Ruby Version 管理器，可能就無需 root 許可權。

  Additionally, if you are installing on an [Apple Silicon Mac][],
  run the command:

  除此之外，如果你是在 Apple 晶片的 Mac 上安裝，則需要執行下面的命令：

  ```terminal
  $ sudo gem uninstall ffi && sudo gem install ffi -- --enable-libffi-alloc
  ```

{{site.alert.end}}

[Check the app's Bundle ID]: {{site.url}}/assets/images/docs/setup/xcode-unique-bundle-id.png
[Choosing a Membership]: {{site.apple-dev}}/support/compare-memberships
[Mac App Store]: https://itunes.apple.com/us/app/xcode/id497799835
[Flutter plugins]: {{site.url}}/packages-and-plugins/developing-packages#types
[Install and set up CocoaPods]: https://guides.cocoapods.org/using/getting-started.html#installation
[Trust Mac]: {{site.url}}/assets/images/docs/setup/trust-computer.png
[web download]: {{site.apple-dev}}/xcode/
[Xcode account add]: {{site.url}}/assets/images/docs/setup/xcode-account.png
[Apple Silicon Mac]: https://support.apple.com/en-us/HT211814
[Developer Mode]: {{site.apple-dev}}/documentation/xcode/enabling-developer-mode-on-a-device
[Apple's Developer Forums]: {{site.apple-dev}}/forums/
[Debugging your add-to-app module]: {{site.url}}/add-to-app/debugging/#wireless-debugging
[Apple's documentation on pairing a wireless device with Xcode]: https://help.apple.com/xcode/mac/9.0/index.html?localePath=en.lproj#/devbc48d1bad
[Apple Developer]: {{site.apple-dev}}/programs/
[Apple Developer Account]: {{site.apple-dev}}/account
[Apple's documentation on installing Simulators]: {{site.apple-dev}}/documentation/xcode/installing-additional-simulator-runtimes
