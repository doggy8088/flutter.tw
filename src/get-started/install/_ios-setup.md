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

 1. Configure the Xcode command-line tools to use the
    newly-installed version of Xcode by
    running the following from the command line:

    配置 Xcode 命令列工具以使用新安裝的 Xcode 版本。
    從命令列中執行以下命令：

    ```terminal
    $ sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
    $ sudo xcodebuild -runFirstLaunch
    ```

    This is the correct path for most cases,
    when you want to use the latest version of Xcode.
    If you need to use a different version,
    specify that path instead.

    當你安裝了最新版本的 Xcode，大部分情況下，上面的路徑都是一樣的。
    但如果你安裝了不同版本的 Xcode，你可能要更改一下上述命令中的路徑。

1. Make sure the Xcode license agreement is signed by
    either opening Xcode once and confirming or running
    `sudo xcodebuild -license` from the command line.
 
    執行一次 Xcode 或者透過輸入命令 `sudo xcodebuild -license`
    來確保已經同意 Xcode 的許可協議。

Versions older than the latest stable version may still work,
but are not recommended for Flutter development.

舊版本可能也能夠正常工作，但是不建議在 Flutter 開發環境中使用。
舊版本的 Xcode 不支援定位程式碼，還可能無法正常工作。

With Xcode, you’ll be able to run Flutter apps on
an iOS device or on the simulator.

安裝了 Xcode 之後，你就可以在 iOS 真機或者模擬器上執行 Flutter 應用了。

### Set up the iOS simulator

### 配置 iOS 模擬器

To prepare to run and test your Flutter app on the iOS simulator,
follow these steps:

如果想要在 iOS 模擬器中執行和測試 Flutter 應用，按照以下步驟即可：

 1. On your Mac, find the Simulator via Spotlight or
    by using the following command:

    在你的 Mac 中，透過 Spotlight 或者以下命令來執行模擬器：

    ```terminal
    $ open -a Simulator
    ```

 2. Make sure your simulator is using a 64-bit device
    (iPhone 5s or later).  You can check the device by viewing the settings in
    the simulator's **Hardware > Device** or **File > Open Simulator** menus.

    透過模擬器選單中的 **Hardware > Device** 或者 **File > Open Simulator** 
    選項檢查當前模擬器是否是 64 位機型（iPhone 5S 或之後的機型）。

 3. Depending on your development machine's screen size,
    simulated high-screen-density iOS devices
    might overflow your screen. Grab the corner of the
    simulator and drag it to change the scale. You can also
    use the **Window > Physical Size** or **Window > Pixel Accurate**
    options if your computer's resolution is high enough.

    根據你當前開發機器的螢幕尺寸，
    模擬器模擬出來的高密度螢幕的裝置可能會溢位你的螢幕，
    你可以調整模擬器的邊角來拖動改變比例，
    如果你的開發機解析度很高的話，也可以透過選單中的
    **Window > Physical Size** or **Window > Pixel Accurate**
    選項來更改模擬器的縮放比例。
    * 如果你只用 Xcode 版本低於 9.1，調整模擬器比例的選單選項應該是
    **Window > Scale**。

### Create and run a simple Flutter app

### 建立並執行一個簡單的 Flutter 應用

To create your first Flutter app and test your setup,
follow these steps:

透過以下步驟來建立你的第一個 Flutter 應用並進行測試：

 1. Create a new Flutter app by running the following from the
    command line:

    透過執行以下命令來建立一個新的 Flutter 應用：
 
    ```terminal
    $ flutter create my_app
    ```

 2. A `my_app` directory is created, containing Flutter's starter app.
    Enter this directory:

    上述命令建立了一個 `my_app` 的目錄，
    包含了 Flutter 初始的應用模版，切換路徑到這個目錄內：
 
    ```terminal
    $ cd my_app
    ```

 3. To launch the app in the Simulator,
    ensure that the Simulator is running and enter:

    確保模擬器已經處於執行狀態，輸入以下命令來啟動應用：

    ```terminal
    $ flutter run
    ```

### Deploy to iOS devices

### 部署到 iOS 裝置

To deploy your Flutter app to a physical iPhone or iPad
you'll need to set up physical device deployment in Xcode
and an Apple Developer account. If your app is using Flutter plugins,
you will also need the third-party CocoaPods dependency manager.

如果你想把 Flutter 應用部署到  iPhone 或 iPad 上，
你還需要一些別的工具和一個 Apple 開發者帳號。
另外，你還需要在 Xcode 上針對你的機器做一些設定。

<ol markdown="1">

<li markdown="1">

<a name="trust"></a>
The first time you use an attached physical device for iOS
development, you need to trust both your Mac and the
Development Certificate on that device.
On iOS 16 and higher you must also enable [Developer Mode][].

Select **Trust** in the dialog prompt when
first connecting the iOS device to your Mac.

![Trust Mac][]{:.mw-100}

Then, go to the Settings app on the iOS device,
select **General > Device Management**
and trust your Certificate.
For first time users, you might need to select
**General > Profiles > Device Management** instead.
On iOS 16 and higher, navigate back to the top level
of the Settings app, select **Privacy & Security > Developer Mode**,
and toggle Developer Mode on.

</li>

<li markdown="1">

You can skip this step if your apps do not depend on
[Flutter plugins][] with native iOS code.
[Install and set up CocoaPods][] by running the following commands:

如果你的應用不依賴 [Flutter plugins][] 與原生 iOS 程式碼互動，你可以跳過這一步。
透過執行以下命令 [安裝和設定CocoaPods][Install and set up CocoaPods]。

```terminal
$ sudo gem install cocoapods
```
{{site.alert.note}}

  The default version of Ruby requires `sudo` to install the CocoaPods gem.
  If you are using a Ruby Version manager, you may need to run without `sudo`.

  Ruby 的預設版本需要 root 許可權 `sudo` 來安裝 CocoaPods gem，
  如果你使用的是 Ruby Version 管理器，可能就無需 root 許可權。

  Additionally, if you are installing on an [Apple Silicon Mac][],
  run the command:

  除此之外，如果你是在 Apple 晶片的 Mac 上安裝，則需要執行下面的命令：

  ```terminal
  $ sudo gem uninstall ffi && sudo gem install ffi -- --enable-libffi-alloc
  ```

{{site.alert.end}}

</li>

<li markdown="1">

Follow the Xcode signing flow to provision your project:

按照下面 Xcode 簽名流程來配置你的專案:

   {: type="a"}
   1. Open the default Xcode workspace in your project by
      running `open ios/Runner.xcworkspace` in a terminal
      window from your Flutter project directory.

      透過在命令列中於你當前 Flutter 專案目錄下執行
      `open ios/Runner.xcworkspace` 命令來開啟預設的 Xcode 工程。

   1. Select the device you intend to deploy to in the device
      drop-down menu next to the run button.

      在執行按鈕的下拉列表裡選擇你想要部署到的裝置；

   1. Select the `Runner` project in the left navigation panel.

      在左側的導航面板中選擇 `Runner` 專案；

   1. In the `Runner` target settings page,
      make sure your Development Team is selected
      under **Signing & Capabilities > Team**.

      在 `Runner` 專案的設定頁面，請確保勾選你的開發團隊。
      在不同的 Xcode 版本里，這一部分的操作介面不同：

      When you select a team,
      Xcode creates and downloads a Development Certificate,
      registers your device with your account,
      and creates and downloads a provisioning profile (if needed).

      當選擇了一個團隊之後，Xcode 會建立和下載一個開發證書，
      並在你的賬戶裡為你的設備註冊，
      並在需要的時候建立和下載一個配置檔案。

      * To start your first iOS development project,
        you might need to sign into
        Xcode with your Apple ID. ![Xcode account add][]{:.mw-100}
        Development and testing is supported for any Apple ID.
        Enrolling in the Apple Developer Program is required to
        distribute your app to the App Store.
        For details about membership types,
        see [Choosing a Membership][].

        在開始你的第一個 iOS 專案開發之前，
        你需要先在 Xcode 中登陸你的 Apple 開發者帳號
        ![Xcode account add][]{:.mw-100}
        任何 Apple ID 都可以進行開發和測試。
        如果想將應用上架 App Store，你需要加入 Apple Developer Program，
        你可以在 [Choosing a Membership][] 頁面中檢視詳細的說明。

      * If automatic signing fails in Xcode, verify that the project's
        **General > Identity > Bundle Identifier** value is unique.

        如果 Xcode 的自動簽名失敗了，你可以檢查以下專案中
        **General > Identity > Bundle Identifier** 裡的值是否是唯一的。

        ![Check the app's Bundle ID][]{:.mw-100}

</li>

<li markdown="1">

Start your app by running `flutter run`
or clicking the Run button in Xcode.

執行 `flutter run` 命令，或者在 Xcode 裡點選執行，
來執行你的應用。

</li>
</ol>

[Check the app's Bundle ID]: {{site.url}}/assets/images/docs/setup/xcode-unique-bundle-id.png
[Choosing a Membership]: {{site.apple-dev}}/support/compare-memberships
[Mac App Store]: https://itunes.apple.com/us/app/xcode/id497799835
[Flutter plugins]: {{site.url}}/development/packages-and-plugins/developing-packages#types
[Install and set up CocoaPods]: https://guides.cocoapods.org/using/getting-started.html#installation
[Trust Mac]: {{site.url}}/assets/images/docs/setup/trust-computer.png
[web download]: {{site.apple-dev}}/xcode/
[Xcode account add]: {{site.url}}/assets/images/docs/setup/xcode-account.png
[Apple Silicon Mac]: https://support.apple.com/en-us/HT211814
[Developer Mode]: https://developer.apple.com/documentation/xcode/enabling-developer-mode-on-a-device
