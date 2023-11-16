---
title: Integrate a Flutter module into your iOS project
title: 將 Flutter module 整合到 iOS 專案
short-title: Integrate Flutter
short-title: 整合 Flutter
description: Learn how to integrate a Flutter module into your existing iOS project.
description: 瞭解如何將 Flutter module 整合到你現有的 iOS 專案中。
tags: Flutter混合工程,add2app
keywords: iOS,專案整合
---

Flutter UI components can be incrementally added into your existing iOS
application as embedded frameworks. There are a few ways to embed Flutter 
in your existing application.

Flutter UI 元件可以漸進式地內嵌到你現有的 iOS 應用中，下面是幾種方法：

1. **Use the CocoaPods dependency manager and installed Flutter SDK.**
  In this case, the `flutter_module` is compiled from
  the source each time the app is built. (Recommended.)

   **使用 CocoaPods 依賴管理器安裝 Flutter SDK**
   使用這種方法，每次建構應用的時候都會從原始碼中編譯 `flutter_module`。(推薦)

1. **Create frameworks for the Flutter engine, your compiled Dart code,
  and all Flutter plugins.** Here, you manually embed the frameworks,
  and update your existing application's build settings in Xcode.
  This can be useful for teams that don't want to require every developer
  to have the Flutter SDK and Cocoapods installed locally.

   **建立一個框架，把 Flutter 引擎、已編譯的 Dart 程式碼和所有 Flutter 外掛都放進去**
   這種方式你可以手動嵌入這個框架，並在 Xcode 中更改現有的應用的建構設定。
   如果不想要求開發團隊的每一位成員都在本地安裝 Flutter SDK 和 Cocoapods，
   這種方式比較適用。

1. **Create frameworks for your compiled Dart code,
  and all Flutter plugins. Use CocoaPods for the Flutter engine.** 
  With this option, embed the frameworks for your application
  and the plugins in Xcode, but distribute the
  Flutter engine as a CocoaPods podspec.
  This is similar to the second option, but it provides
  an alternative to distributing the large Flutter.xcframework.

   **為已編譯的 Dart 程式碼和所有 Flutter 外掛建立一個框架，對 Flutter 引擎使用 CocoaPods 來管理**
   這種方式是將應用內容和外掛作為內嵌的框架，但將 Flutter 引擎作為 CocoaPods podspec 分發。
   這有點類似第二種方式，但是它為分發大型的 Flutter.xcframework 檔案提供了替代方案。

For examples using an app built with UIKit, 
see the iOS directories in the [add_to_app code samples][]. 
For an example using SwiftUI, see the iOS directory in [News Feed App][].

例如使用 UIKit 建構的應用，請參閱
[add_to_app 程式碼範例][add_to_app code samples] 中 iOS 這個目錄。
有關使用 SwiftUI 的範例，請參閱 [News Feed App][] 中的 iOS 目錄。

## System requirements

## 系統要求

Your development environment must meet the
[macOS system requirements for Flutter][]
with [Xcode installed][].
Flutter supports iOS 11 and later.
Additionally, you will need [CocoaPods][]
version 1.10 or later.

你的開發環境必須滿足
[Flutter 對 macOS 系統的版本要求][macOS system requirements for Flutter]
並 [已經安裝 Xcode][Xcode installed]，Flutter 支援 iOS 11 及以上。
此外，你還需要 1.10 或以上版本的 [CocoaPods][]

## Create a Flutter module

## 建立 Flutter module

To embed Flutter into your existing application, 
using any of the methods mentioned above, 
first create a Flutter module.

為了將 Flutter 整合到你的既有應用裡，
參考上面的任意方法先建立一個 Flutter module。

From the command line, run:

在命令列中執行：

```terminal
cd some/path/
flutter create --template module my_flutter
```

A Flutter module project is created at `some/path/my_flutter/`.
If you are using the first method mentioned above, 
the module should be created in the same parent directory 
as your existing iOS app. 

Flutter module 會建立在 `some/path/my_flutter/` 目錄。
如果你使用上述第一種方法，則應在與現有 iOS 應用工程的父目錄中建立這個 Flutter module。

From the Flutter module directory, you can run the same `flutter`
commands you would in any other Flutter project,
like `flutter run --debug` or `flutter build ios`.
You can also run the module in
[Android Studio/IntelliJ][] or [VS Code][] with
the Flutter and Dart plugins. This project contains a
single-view example version of your module before it's
embedded in your existing application,
which is useful for incrementally
testing the Flutter-only parts of your code.

在這個目錄中，你可以像在其它 Flutter 專案中一樣，執行 `flutter` 命令。
比如 `flutter run --debug` 或者 `flutter build ios`。
同樣，你也可以透過 [Android Studio/IntelliJ][] 或者 [VS Code][]
中的 Flutter 和 Dart 外掛執行這個 module，在整合到現有應用前，
這個專案在 Flutter module 中包含了一個單檢視的範例程式碼，
對 Flutter 側程式碼的測試會有幫助。

### Module organization

### Module 的目錄結構

The `my_flutter` module directory structure is similar to a
normal Flutter application:

在 `my_flutter` module 裡，目錄結構和普通 Flutter 應用類似：

```text
my_flutter/
├── .ios/
│   ├── Runner.xcworkspace
│   └── Flutter/podhelper.rb
├── lib/
│   └── main.dart
├── test/
└── pubspec.yaml
```

Add your Dart code to the `lib/` directory.

新增你的 Dart 程式碼到 `lib/` 目錄。

Add Flutter dependencies to `my_flutter/pubspec.yaml`,
including Flutter packages and plugins.

新增 Flutter 依賴到 `my_flutter/pubspec.yaml`，
包括 Flutter packages 和 plugins。

The `.ios/` hidden subfolder contains an Xcode workspace where
you can run a standalone version of your module.
It is a wrapper project to bootstrap your Flutter code,
and contains helper scripts to facilitate building frameworks or
embedding the module into your existing application with CocoaPods.

`.ios/` 隱藏資料夾包含了一個 Xcode workspace，用於單獨執行你的 Flutter module。
它是一個獨立啟動 Flutter 程式碼的殼工程，並且包含了一個幫助指令碼，
用於編譯 framewroks 或者使用 CocoaPods 將 Flutter module 整合到你的既有應用。

{{site.alert.note}}

  Add custom iOS code to your own existing application's
  project or to a plugin, not to the module's `.ios/`
  directory. Changes made in your module's `.ios/`
  directory don't appear in your existing iOS project
  using the module, and might be overwritten by Flutter.

  iOS 程式碼要新增到你的既有應用或者 Flutter plugin中，
  而不是 Flutter module 的 `.ios/` 目錄下。
  `.ios/` 下的改變不會整合到你的既有應用，
  並且這有可能被 Flutter 重寫。

  Do not source control the `.ios/` directory since it's autogenerated.
  Before building the module on a new machine, run `flutter pub get`
  in the `my_flutter` directory first to regenerate the `.ios/`
  directory before building iOS project using the Flutter module.

  由於 `.ios/` 目錄是自動產生的，因此請勿對其進行版本控制。在新機器上建構 module 時，
  請在使用 Flutter module 建構 iOS 專案之前，
  先於 `my_flutter` 目錄執行 `flutter pub get` 以產生 `.ios/` 目錄。

{{site.alert.end}}

## Embed the Flutter module in your existing application

## 在你的既有應用中整合 Flutter module

After you have developed your Flutter module,
you can embed it using the methods described at the top of the page.

在你的 module 開發完成後，
你就能使用頁面頂部描述的方法將其嵌入到應用中去了。

{{site.alert.note}}

  You can run in Debug mode on a simulator or a real device,
  and Release on a real device. Learn more about
  [Flutter's build modes][build modes of Flutter]

  你可以在模擬機和真機上執行 Debug 模式，
  在真機上執行 Release 模式，瞭解更多，
  請檢視文件 [Flutter 的建構模式][build modes of Flutter]。

  To leverage Flutter debugging functionality 
  such as hot reload, see  [Debugging your add-to-app module][].

  若要嘗試 Flutter 除錯裡類似熱重載等功能，請參閱文件
  [除錯你的 add-to-app 模組][Debugging your add-to-app module]。

{{site.alert.end}}

Using Flutter [increases your app size][].

使用 Flutter 會 [增加應用體積][increases your app size] 。

### Option A - Embed with CocoaPods and the Flutter SDK

### 選項 A - 使用 CocoaPods 和 Flutter SDK 整合

This method requires every developer working on your
project to have a locally installed version of the Flutter SDK.
The Flutter module is compiled from source each time the app is built.
Simply build your application in Xcode to automatically
run the script to embed your Dart and plugin code.
This allows rapid iteration with the most up-to-date
version of your Flutter module without running additional
commands outside of Xcode.

這個方法需要你的專案的所有開發者，都在本地安裝 Flutter SDK。
你的工程在每次建構的的時候，都將會從原始碼裡編譯 Flutter 模組。
只需要在 Xcode 中編譯應用，就可以自動執行指令碼來整合 Dart 程式碼和外掛。
這個方法允許你使用 Flutter module 中的最新程式碼區塊速迭代開發，
而無需在 Xcode 以外執行額外的命令。

The following example assumes that your existing
application and the Flutter module are in sibling
directories. If you have a different directory structure,
you might need to adjust the relative paths.

下面的範例假設你的既有應用和 Flutter module 在相鄰目錄。
如果你有不同的目錄結構，需要適配到對應的路徑。

```text
some/path/
├── my_flutter/
│   └── .ios/
│       └── Flutter/
│         └── podhelper.rb
└── MyApp/
    └── Podfile
```

If your existing application (`MyApp`) doesn't
already have a Podfile, run `pod init` in the  
`MyApp` directory to create one. 
You can find more details on using 
CocoaPods in the [CocoaPods getting started guide][].

如果你的應用下（`MyApp`）還沒有 Podfile，
請執行 `pod init` 來建立一個。
你可以在 [CocoaPods 起步指南][CocoaPods getting started guide]
中瞭解更多。


<ol markdown="1">
<li markdown="1">

Add the following lines to your `Podfile`:

在 `Podfile` 中新增下面程式碼：

<?code-excerpt title="MyApp/Podfile"?>
```ruby
flutter_application_path = '../my_flutter'
load File.join(flutter_application_path, '.ios', 'Flutter', 'podhelper.rb')
```

</li>

<li markdown="1">

For each [Podfile target][] that needs to
embed Flutter, call `install_all_flutter_pods(flutter_application_path)`.

每個需要整合 Flutter 的 [Podfile target][]，
執行 `install_all_flutter_pods(flutter_application_path)`：

<?code-excerpt title="MyApp/Podfile"?>
```ruby
target 'MyApp' do
  install_all_flutter_pods(flutter_application_path)
end
```

</li>

<li markdown="1">

In the `Podfile`'s `post_install` block, call `flutter_post_install(installer)`.

在 `Podfile` 的 `post_install` 部分，呼叫 `flutter_post_install(installer)`。

<?code-excerpt title="MyApp/Podfile"?>
```ruby
post_install do |installer|
  flutter_post_install(installer) if defined?(flutter_post_install)
end
```

{{site.alert.note}}

  The `flutter_post_install` method (added in Flutter 3.1.0),
  adds build settings to support native Apple Silicon `arm64` iOS simulators.
  Include the `if defined?(flutter_post_install)` check to ensure your `Podfile`
  is valid if you are running on older versions of Flutter that don't have this method.

  `flutter_post_install` 方法（Flutter 3.1.0 中新增的）
  增加了原生 Apple Silicon `arm64` iOS 模擬器的支援。
  它包括 `if defined?(flutter_post_install)` 的檢查以確保你的
  `Podfile` 在舊版本的沒有該方法的 Flutter 上也能正常執行。

{{site.alert.end}}

</li>

<li markdown="1">

Run `pod install`.

   執行 `pod install`。

{{site.alert.note}}

  When you change the Flutter plugin dependencies in
  `my_flutter/pubspec.yaml`, run `flutter pub get`
  in your Flutter module directory to refresh the list
  of plugins read by the `podhelper.rb` script.
  Then, run `pod install` again from
  your application at `some/path/MyApp`.

  當你在 `my_flutter/pubspec.yaml` 改變了 Flutter plugin 依賴，
  需要在 Flutter module 目錄執行 `flutter pub get`，
  來更新會被`podhelper.rb` 指令碼用到的 plugin 列表，
  然後再次在你的應用目錄 `some/path/MyApp` 執行 `pod install`.

{{site.alert.end}}

</li>
</ol>

The `podhelper.rb` script embeds your plugins,
`Flutter.framework`, and `App.framework` into your project.

`podhelper.rb` 指令碼會把你的 plugins，
`Flutter.framework`，和 `App.framework` 整合到你的專案中。

Your app's Debug and Release build configurations embed
the Debug or Release [build modes of Flutter][], respectively.
Add a Profile build configuration
to your app to test in profile mode.

你應用的 Debug 和 Release 編譯配置，將會整合相對應的
Debug 或 Release 的 [編譯產物][build modes of Flutter]。
可以增加一個 Profile 編譯配置用於在 profile 模式下測試應用。

{{site.alert.tip}}

  `Flutter.framework` is the bundle for the Flutter engine,
  and `App.framework` is the compiled Dart code for this project.

  `Flutter.framework` 是 Flutter engine 的框架，
  `App.framework` 是你的 Dart 程式碼的編譯產物。

{{site.alert.end}}

Open `MyApp.xcworkspace` in Xcode.
You can now build the project using `⌘B`.

在 Xcode 中開啟 `MyApp.xcworkspace` ，你現在可以使用 `⌘B` 編譯專案了。

### Option B - Embed frameworks in Xcode

### 選項 B - 在 Xcode 中整合 frameworks

Alternatively, you can generate the necessary frameworks
and embed them in your application by manually editing
your existing Xcode project. You might do this if members of your
team can't locally install Flutter SDK and CocoaPods,
or if you don't want to use CocoaPods
as a dependency manager in your existing applications.
You must run `flutter build ios-framework`
every time you make code changes in your Flutter module.

除了上面的方法，你也可以建立必備的 frameworks，手動修改既有 Xcode 專案，將他們整合進去。
當你組內其它成員們不能在本地安裝 Flutter SDK 和 CocoaPods，
或者你不想使用 CocoaPods 作為既有應用的依賴管理時，這種方法會比較合適。
但是每當你在 Flutter module 中改變了程式碼，
都必須執行 `flutter build ios-framework`。

The following example assumes that you want to generate the
frameworks to `some/path/MyApp/Flutter/`.

下面的範例假設你想在 `some/path/MyApp/Flutter/` 目錄下建立 frameworks：

```terminal
flutter build ios-framework --output=some/path/MyApp/Flutter/
```

```text
some/path/MyApp/
└── Flutter/
    ├── Debug/
    │   ├── Flutter.xcframework
    │   ├── App.xcframework
    │   ├── FlutterPluginRegistrant.xcframework (only if you have plugins with iOS platform code)
    │   └── example_plugin.xcframework (each plugin is a separate framework)
    ├── Profile/
    │   ├── Flutter.xcframework
    │   ├── App.xcframework
    │   ├── FlutterPluginRegistrant.xcframework
    │   └── example_plugin.xcframework
    └── Release/
        ├── Flutter.xcframework
        ├── App.xcframework
        ├── FlutterPluginRegistrant.xcframework
        └── example_plugin.xcframework
```

{{site.alert.warning}}

  Always use `Flutter.xcframework` and `App.xcframework`
  from the same directory. Mixing `.xcframework` imports
  from different directories (such as `Profile/Flutter.xcframework`
  with `Debug/App.xcframework`) causes runtime crashes.

  始終使用相同目錄下的 `Flutter.framework` 和 `App.framework`。
  混合使用不同目錄（例如 `Profile/Flutter.framework`
  以及 `Debug/App.framework`）將會導致執行失敗。

{{site.alert.end}}

Link and embed the generated frameworks into your existing
application in Xcode.  There are multiple ways to do
this&mdash;use the method that is best for your project.

在 Xcode 中將產生的 frameworks 整合到你的既有應用中。
例如，你可以在 `some/path/MyApp/Flutter/Release/`
目錄拖拽 frameworks 到 你的應用 target 編譯設定的
General > Frameworks, Libraries, and Embedded Content 下，
然後在 Embed 下拉列表中選擇 "Embed & Sign"。

#### Link on the frameworks

#### 連結到框架

For example, you can drag the frameworks from
`some/path/MyApp/Flutter/Release/` in Finder
into your target's **Build
Settings > Build Phases > Link Binary With Libraries**.

例如，你可以將框架從 Finder 的 `some/path/MyApp/Flutter/Release/`
拖到你的目標專案中，
然後點選以下步驟
build settings > Build Phases > Link Binary With Libraries。

In the target's build settings, add `$(PROJECT_DIR)/Flutter/Release/`
to the **Framework Search Paths** (`FRAMEWORK_SEARCH_PATHS`).

在 target 的編譯設定中的
Framework Search Paths (`FRAMEWORK_SEARCH_PATHS`)
增加 `$(PROJECT_DIR)/Flutter/Release/`。

{% include docs/app-figure.md image="development/add-to-app/ios/project-setup/framework-search-paths.png" alt="Update Framework Search Paths in Xcode" %}

{{site.alert.tip}}

  To use the simulator, you will need to 
  embed the Debug version of the Flutter frameworks in your
  Debug build configuration. To do this 
  you can use `$(PROJECT_DIR)/Flutter/$(CONFIGURATION)`
  in the **Framework Search Paths** (`FRAMEWORK_SEARCH_PATHS`)
  build setting. This embeds the Release frameworks in the Release configuration, 
  and the Debug frameworks in the Debug Configuration.

  若你需要使用模擬器，你需要在你的 Debug 建構配置中嵌入 Debug 環境的 Flutter framework。 
  此時你應該在 **Framework Search Paths** (`FRAMEWORK_SEARCH_PATHS`) 建構設定中使用
  `$(PROJECT_DIR)/Flutter/$(CONFIGURATION)`。
  它會讓不同環境的 Flutter framework 對應地嵌入到不同模式。
  
  You must also open `MyApp.xcodeproj/project.pbxproj` (from Finder) 
  and replace `path = Flutter/Release/example.xcframework;`
  with `path = "Flutter/$(CONFIGURATION)/example.xcframework";`
  for all added frameworks. (Note the added `"`.)

  你需要開啟 `MyApp.xcodeproj/project.pbxproj`（從訪達中開啟）
  將 `path = Flutter/Release/example.xcframework;` 替換為
  `path = "Flutter/$(CONFIGURATION)/example.xcframework";`
  來新增所有的 framework（注意增加的 `"` 雙引號）。

{{site.alert.end}}

#### Embed the frameworks

### 內嵌框架

The generated dynamic frameworks must be embedded
into your app to be loaded at runtime.

產生的動態框架必須嵌入你的應用並且在執行時被載入。

{{site.alert.important}}

  Plugins might produce [static or dynamic frameworks][].
  Static frameworks should be linked on, but never embedded.
  If you embed a static framework into your application,
  your application is not publishable to the App Store
  and fails with a
  **Found an unexpected Mach-O header code** archive error.

  外掛會幫助你產生 [靜態或動態框架][static or dynamic frameworks]。
  靜態框架應該直接連結而不是嵌入。
  如果你在應用中嵌入了靜態框架，
  你的應用將不能釋出到 App Store 並且會得到一個
  **Found an unexpected Mach-O header code** 的 archive error。

{{site.alert.end}}

After linking the frameworks, you should see them in the 
**Frameworks, Libraries, and Embedded Content**
section of your target's **General** settings. 
To embed the dynamic frameworks
select **Embed & Sign**. 

例如，你可以從應用框架組中拖拽框架
（除了 FlutterPluginRegistrant 以及其他的靜態框架）
到你的目標 ' build settings > Build Phases > Embed Frameworks。
然後從下拉選單中選擇 “Embed & Sign”。

They will then appear under **Embed Frameworks** within 
**Build Phases** as follows: 

之後它們將出現在 **Build Phases** 中的 **Embed Frameworks** 內，
如下所示：

{% include docs/app-figure.md image="development/add-to-app/ios/project-setup/embed-xcode.png" alt="Embed frameworks in Xcode" %}

You should now be able to build the project in Xcode using `⌘B`.

你現在可以在 Xcode中使用 `⌘B` 編譯專案。

### Option C - Embed application and plugin frameworks in Xcode and Flutter framework with CocoaPods

### 選項 C -  使用 CocoaPods 在 Xcode 和 Flutter 框架中內嵌應用和外掛框架

Alternatively, instead of distributing the large Flutter.xcframework
to other developers, machines, or continuous integration systems,
you can instead generate Flutter as CocoaPods podspec by adding
the flag `--cocoapods`. This produces a `Flutter.podspec`
instead of an engine Flutter.xcframework.
The App.xcframework and plugin frameworks are generated
as described in Option B.

除了將一個很大的 Flutter.framework 分發給其他開發者、機器或者持續整合 (CI) 系統
之外，你可以加入一個引數 `--cocoapods` 將 Flutter 框架作為一個
CocoaPods 的 podspec 檔案分發。
這將會產生一個 `Flutter.podspec` 檔案而不再產生 Flutter.framework 引擎檔案。
如選項 B 中所說的那樣，它將會產生 App.framework 和外掛框架。

To generate the `Flutter.podspec` and frameworks, run the following 
from the command line in the root of your Flutter module:

要產生 `Flutter.podspec` 和框架，
命令列切換到 Flutter module 根目錄，然後執行以下命令：

```terminal
flutter build ios-framework --cocoapods --output=some/path/MyApp/Flutter/
```

```text
some/path/MyApp/
└── Flutter/
    ├── Debug/
    │   ├── Flutter.podspec
    │   ├── App.xcframework
    │   ├── FlutterPluginRegistrant.xcframework
    │   └── example_plugin.xcframework (each plugin with iOS platform code is a separate framework)
    ├── Profile/
    │   ├── Flutter.podspec
    │   ├── App.xcframework
    │   ├── FlutterPluginRegistrant.xcframework
    │   └── example_plugin.xcframework
    └── Release/
        ├── Flutter.podspec
        ├── App.xcframework
        ├── FlutterPluginRegistrant.xcframework
        └── example_plugin.xcframework
```

Host apps using CocoaPods can add Flutter to their Podfile:

使用 CocoaPods 的宿主應用程式可以將 Flutter 新增到 Podfile 中：

<?code-excerpt title="MyApp/Podfile"?>
```ruby
pod 'Flutter', :podspec => 'some/path/MyApp/Flutter/[build mode]/Flutter.podspec'
```

{{site.alert.note}}

  You must hard code the `[build mode]` value.
  For example, use `Debug` if you need to use
  `flutter attach` and `Release` when you're ready to ship.

  你必須選擇相應的建構模式進行硬編碼，
  將建構模式的值寫在上面指令中的 `[build mode]` 位置。 
  例如，你需要 `flutter attach` 的時候，應該使用 `Debug`，
  在你準備釋出版本的時候，應該使用 `Release`。

{{site.alert.end}}

Link and embed the generated App.xcframework,
FlutterPluginRegistrant.xcframework,
and any plugin frameworks into your existing application
as described in Option B.

如選項 B 所述，將產生的 App.xcframework、
FlutterPluginRegistrant.xcframework 以及
任何外掛框架，連結並嵌入到你現有的應用程式中。

## Local Network Privacy Permissions

## 本地網路隱私許可權

On iOS 14 and higher, enable the Dart multicast DNS
service in the Debug version of your app
to add [debugging functionalities such as hot-reload and
DevTools][] via `flutter attach`.

在 iOS 14 及更高的版本中，
可以在應用程式的 Debug 版本中啟用 Dart 的 多播 DNS 服務 (multicast DNS service)，
透過 `flutter attach` 新增 
[除錯功能，如熱重載和 DevTools][debugging functionalities such as hot-reload and DevTools]。

{{site.alert.warning}}

  This service must not be enabled in the **Release**
  version of your app, or you might experience App Store rejections.

  不可以在應用程式的 **Release** 版本中啟用這項服務，
  否則你很有可能被 App Store 拒絕上架。

{{site.alert.end}}

One way to do this is to maintain a separate copy of your app's Info.plist per
build configuration. The following instructions assume
the default **Debug** and **Release**.
Adjust the names as needed depending on your app's build configurations.

還有一種方式是將每種不同的建構配置，單獨建立對應配置的 Info.plist。
下面的說明假定預設為 **除錯版本 (Debug)** 和 **釋出版本 (Release)**。
根據應用程式建構配置的需要調整名稱。

<ol markdown="1">
<li markdown="1">

Rename your app's **Info.plist** to **Info-Debug.plist**.
Make a copy of it called **Info-Release.plist** and add it to your Xcode project.

將應用程式中的 **Info.plist** 重新命名為 **Info-Debug.plist**，
再複製一個相同的檔案並重命名為 **Info-Release.plist**，
最後將 **Info-Debug.plist**、**Info-Release.plist** 新增到 Xcode 專案中。

{% include docs/app-figure.md image="development/add-to-app/ios/project-setup/info-plists.png" alt="Info-Debug.plist and Info-Release.plist in Xcode" %}

</li>

<li markdown="1">

In **Info-Debug.plist** _only_ add the key `NSBonjourServices`
and set the value to an array with the string `_dartVmService._tcp`.
Note Xcode will display this as "Bonjour services".

在 **Info-Debug.plist** 中 **只** 新增 key `NSBonjourServices`，
並將它的值設定為陣列 (Array)，然後在該陣列中新增 `_dartVmService._tcp` 字串 (String)。

Optionally, add the key `NSLocalNetworkUsageDescription` set to your
desired customized permission dialog text.

可以選擇新增 key `NSLocalNetworkUsageDescription`，
並設定為你自訂的許可權提示對話方塊文字。

{% include docs/app-figure.md image="development/add-to-app/ios/project-setup/debug-plist.png" alt="Info-Debug.plist with additional keys" %}

</li>

<li markdown="1">

In your target's build settings, change the **Info.plist File**
(`INFOPLIST_FILE`) setting path from `path/to/Info.plist` to `path/to/Info-$(CONFIGURATION).plist`.

在 target 建構設定中，將 **Info.plist File** (`INFOPLIST_FILE`) 設定路徑
從 `path/to/Info.plist` 改為 `path/to/Info-$(CONFIGURATION).plist`。

{% include docs/app-figure.md image="development/add-to-app/ios/project-setup/set-plist-build-setting.png" alt="Set INFOPLIST_FILE build setting" %}

This will resolve to the path **Info-Debug.plist** in **Debug** and
**Info-Release.plist** in **Release**.

這個設定將會在 **Debug** 時，使用 **Info-Debug.plist** 的配置，
在 **Release** 時，使用 **Info-Release.plist** 的配置。

{% include docs/app-figure.md image="development/add-to-app/ios/project-setup/plist-build-setting.png" alt="Resolved INFOPLIST_FILE build setting" %}

Alternatively, you can explicitly set the **Debug** path to **Info-Debug.plist**
and the **Release** path to **Info-Release.plist**.

又或者，你可以明確地將 **Debug** 的路徑設定為 **Info-Debug.plist**，
將 **Release** 的路徑設定為 **Info-Release.plist**。

</li>

<li markdown="1">

If the **Info-Release.plist** copy is in your target's **Build Settings > Build Phases > Copy Bundle**
Resources build phase, remove it.

如果 **Info-Release.plist** 在 target 中 **Build Settings > Build Phases > Copy Bundle Resources** 
的時候，請刪除它。

{% include docs/app-figure.md image="development/add-to-app/ios/project-setup/copy-bundle.png" alt="Copy Bundle build phase" %}

The first Flutter screen loaded by your Debug app will now prompt
for local network permission. The permission can also be allowed by enabling
**Settings > Privacy > Local Network > Your App**.

現在 Debug 應用程式會在 Flutter 啟動時提示本地網路許可權。
也可以透過開啟 **設定 > 隱私與安全性 > 本地網路 > 你的應用程式** 來允許該許可權。

{% include docs/app-figure.md image="development/add-to-app/ios/project-setup/network-permission.png" alt="Local network permission dialog" %}

</li>
</ol>

## Apple Silicon (`arm64` Macs)

On an Apple Silicon (M1) Mac, the host app builds for an `arm64` simulator.
While Flutter supports `arm64` simulators, some plugins might not. If you use
one of these plugins, you might see a compilation error like **Undefined symbols
for architecture arm64** and you must exclude `arm64` from the simulator
architectures in your host app.

在使用 Apple Silicon 晶片的 Mac 上 (M1)，宿主應用將針對 `arm64` 架構的模擬器編譯。
儘管 Flutter 支援 `arm64` 的 iOS 模擬器，但一些外掛仍有可能未進行支援。
當你在使用這些外掛時，你會遇到 **Undefined symbols for architecture arm64** 的錯誤，
此時你必須從模擬器支援架構中移除 `arm64`。

In your host app target, find the **Excluded Architectures** (`EXCLUDED_ARCHS`) build setting.
Click the right arrow disclosure indicator icon to expand the available build configurations.
Hover over **Debug** and click the plus icon. Change **Any SDK** to **Any iOS Simulator SDK**.
Add `arm64` to the build settings value.

在宿主應用的 Target 中，找到名為 **Excluded Architectures** (`EXCLUDED_ARCHS`) 的建構設定。
單擊右側的箭頭指示器圖示以展開可用的建構配置。
將滑鼠懸停在 **Debug** 處並單擊加號圖示。將 **Any SDK** 更改為 **Any iOS Simulator SDK**。
然後向建構設定值中新增 `arm64`。

{% include docs/app-figure.md image="development/add-to-app/ios/project-setup/excluded-archs.png" alt="Set conditional EXCLUDED_ARCHS build setting" %}

When done correctly, Xcode will add `"EXCLUDED_ARCHS[sdk=iphonesimulator*]" = arm64;` to your **project.pbxproj** file.

當全部都正確設定後，Xcode 將會向你的 **project.pbxproj** 檔案中新增 `"EXCLUDED_ARCHS[sdk=iphonesimulator*]" = arm64;`。

Repeat for any iOS unit test targets.

然後對全部 iOS 目標再次執行單元測試。

## Development

## 開發

You can now [add a Flutter screen][] to your existing application.

你現在可以 [新增一個 Flutter 頁面][add a Flutter screen] 到你的既有應用中。

[add_to_app code samples]: {{site.github}}/flutter/samples/tree/main/add_to_app
[add a Flutter screen]: {{site.url}}/add-to-app/ios/add-flutter-screen
[Android Studio/IntelliJ]: {{site.url}}/tools/android-studio
[build modes of Flutter]: {{site.url}}/testing/build-modes
[embed the frameworks]: {{site.url}}/add-to-app/ios/project-setup#embed-the-frameworks
[CocoaPods]: https://cocoapods.org/
[CocoaPods getting started guide]: https://guides.cocoapods.org/using/using-cocoapods.html
[debugging functionalities such as hot-reload and DevTools]: {{site.url}}/add-to-app/debugging
[Embed with CocoaPods and Flutter tools]: #option-a---embed-with-cocoapods-and-the-flutter-sdk
[increases your app size]: {{site.url}}/resources/faq#how-big-is-the-flutter-engine
[macOS system requirements for Flutter]: {{site.url}}/get-started/install/macos#system-requirements
[On iOS 14 and higher]: {{site.apple-dev}}/news/?id=0oi77447
[Podfile target]: https://guides.cocoapods.org/syntax/podfile.html#target
[static or dynamic frameworks]: {{site.so}}/questions/32591878/ios-is-it-a-static-or-a-dynamic-framework
[VS Code]: {{site.url}}/tools/vs-code
[XCFrameworks]: {{site.apple-dev}}/documentation/xcode_release_notes/xcode_11_release_notes
[Xcode installed]: {{site.url}}/get-started/install/macos#install-xcode
[News Feed app]: https://github.com/flutter/put-flutter-to-work/tree/022208184ec2623af2d113d13d90e8e1ce722365
[Debugging your add-to-app module]: {{site.url}}/add-to-app/debugging/
