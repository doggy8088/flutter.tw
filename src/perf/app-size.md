---
title: Measuring your app's size
title: 測量你的應用體積
description: How to measure app size for iOS and Android.
description: 如何測量你的應用在 Android 以及 iOS 上的大小。
tags: Flutter效能
keywords: 包大小,減少Flutter包體積,Flutter瘦身
---

Many developers are concerned with the size of their compiled app. As the APK,
app bundle, or IPA version of a Flutter app is self-contained and holds all the
code and assets needed to run the app, its size can be a concern. The larger an
app, the more space it requires on a device, the longer it takes to download,
and it may break the limit of useful features like Android instant apps.

許多開發者都會關注應用編譯後的大小。Flutter 應用編譯出的 APK、app bundle 和 IPA 均持有
應用執行需要的所有程式碼和資源，是完全獨立的。一個應用越大，在裝置上佔用的空間就越多，下載時間就越長，
還可能超出 Android 即時應用等實用功能的限制。

## Debug builds are not representative

## 除錯版本不具有代表性

By default, launching your app with `flutter run`,
or by clicking the **Play** button in your IDE
(as used in [Test drive][] and
[Write your first Flutter app][]),
generates a _debug_ build of the Flutter app.
The app size of a debug build is large due to
the debugging overhead that allows for hot reload
and source-level debugging. As such, it is not representative of a production
app end users download.

預設情況下，使用 `flutter run` 命令啟動應用，或者點選 IDE 的 **Play** 按鈕
（如 [開發體驗初探][Test drive] 和 [編寫第一個 Flutter 應用][Write your first Flutter app] 中所使用的），
會產生 Flutter 應用的 **除錯** 版本。除錯版本體積很大，用於熱重載和原始碼除錯。
因此，它不能代表使用者最終下載的正式版本的應用。

## Checking the total size

## 檢查總大小

A default release build, such as one created by `flutter build apk` or
`flutter build ios`, is built to conveniently assemble your upload package
to the Play Store and App Store. As such, they're also not representative of
your end-users' download size. The stores generally reprocess and split
your upload package to target the specific downloader and the downloader's
hardware, such as filtering for assets targeting the phone's DPI, filtering
native libraries targeting the phone's CPU architecture.

由 `flutter build apk` 或 `flutter build ios` 等產生的預設發行版本，
是為了方便在 Play 商店和 App Store 上組裝你上傳的應用套件。
因此，它們也無法代表你的使用者最終下載的大小。
應用商店通常會針對不同的下載程式及其硬體，重新處理和拆分你上傳的應用套件，
例如根據手機的 DPI 過濾資源、根據手機的 CPU 架構過濾原生庫。 

### Estimating total size

### 估算總大小

To get the closest approximate size on each platform, use the following
instructions.

請使用以下指南，獲取各個平臺下最接近的估算大小。

#### Android

Follow the Google [Play Console's instructions][] for checking app download and
install sizes.

根據 Google 的 [Play 控制檯說明][Play Console's instructions] 來檢視應用的下載大小。

Produce an upload package for your application:

產生你的應用的上傳包：

```terminal
flutter build appbundle
```

Log into your [Google Play Console][]. Upload your application binary by drag
dropping the .aab file.

登入你的 [Google Play 控制檯][Google Play Console]。
透過拖放 .abb 檔案來上傳應用的二進位制檔案。

View the application's download and install size in the **Android vitals** ->
**App size** tab.

在 **Android vitals** -> **App size** 選項卡中檢視應用的下載和安裝大小。

{% include docs/app-figure.md image="perf/vital-size.png" alt="App size tab in Google Play Console" %}

The download size is calculated based on an XXXHDPI (~640dpi) device on an
arm64-v8a architecture. Your end users' download sizes may vary depending on
their hardware.

該下載大小是基於 XXXHDPI (~640dpi) 且架構為 arm64-v8a 的裝置來計算的。
使用者最終的下載大小可能因硬體而異。

The top tab has a toggle for download size and install size. The page also
contains optimization tips further below.

頂部選項卡有一個切換下載大小和安裝大小的開關。該頁面還包含了進一步的最佳化提示。

#### iOS

Create an [Xcode App Size Report][].

建立一份 [Xcode 應用大小報告][Xcode App Size Report]。

First, by configuring the app version and build as described in the
[iOS create build archive instructions][].

首先，參照 [iOS 建立建構歸檔指南][iOS create build archive instructions]，
配置應用的版本，並開始建構。

Then:

然後：

1. Run `flutter build ipa --export-method development`.

   執行命令 `flutter build ipa --export-method development`。

1. Run `open build/ios/archive/*.xcarchive` to open the archive in Xcode.
   執行命令 `open build/ios/archive/*.xcarchive` 開啟 Xcode 產生的歸檔檔案。

1. Click **Distribute App**.

   點選 **Distribute App**。

1. Select a method of distribution. **Development** is the simplest if you don't
   intend to distribute the application.
   
   選擇一種釋出方式。如果你不打算釋出該應用，**Development** 模式是最簡單的。
   
1. In **App Thinning**, select 'all compatible device variants'.

   在 **App Thinning** 中，選擇「all compatible device variants」。

1. Select **Strip Swift symbols**.

   選擇 **Strip Swift symbols**。

Sign and export the IPA. The exported directory contains
`App Thinning Size Report.txt` with details about your projected
application size on different devices and versions of iOS.

簽名並匯出 IPA 套件，匯出目錄中有一個 `App Thinning Size Report.txt` 檔案，
其中記錄了在不同裝置和 iOS 版本上預估的應用程式大小的詳細資訊。

The App Size Report for the default demo app in Flutter 1.17 shows:

Flutter 1.17 上的預設 demo app 的應用大小報告顯示如下：

```
Variant: Runner-7433FC8E-1DF4-4299-A7E8-E00768671BEB.ipa
Supported variant descriptors: [device: iPhone12,1, os-version: 13.0] and [device: iPhone11,8, os-version: 13.0]
App + On Demand Resources size: 5.4 MB compressed, 13.7 MB uncompressed
App size: 5.4 MB compressed, 13.7 MB uncompressed
On Demand Resources size: Zero KB compressed, Zero KB uncompressed
```

In this example, the app has an approximate
download size of 5.4 MB and an approximate
install size of 13.7 MB on an iPhone12,1 ([Model ID / Hardware
number][] for iPhone 11)
and iPhone11,8 (iPhone XR) running iOS 13.0.

在這個例子中，裝置 iPhone12,1（iPhone 11 的 [Model ID / Hardware number][]）
和 iPhone11,8 (iPhone XR) 執行在 iOS 13.0 版本下時，
下載大小約為 5.4 MB，安裝大小約為 13.7 MB。

To measure an iOS app exactly,
you have to upload a release IPA to Apple’s
App Store Connect ([instructions][])
and obtain the size report from there.
IPAs are commonly larger than APKs as explained
in [How big is the Flutter engine?][], a
section in the Flutter [FAQ][].

想要精確測量一個 iOS 應用的體積，你需要先將一個發行版本的 IPA 包上傳至 
App Store Connect（[簡介][instructions]），再獲取它的大小報告。
IPA 包一般都比 APK 包要大，這在 Flutter [FAQ][] 中的 
[Flutter 引擎有多大？][How big is the Flutter engine?] 一節中已經闡述過了。

## Breaking down the size

## 大小拆分

Starting in Flutter version 1.22 and DevTools version 0.9.1,
a size analysis tool is included to help developers understand the breakdown
of the release build of their application.

從 Flutter 1.22 和 DevTools 0.9.1 版本開始，包含了一個大小分析工具，
幫助開發者瞭解和拆分應用的發行版本。

{{site.alert.warning}}

  As stated in the [checking total size](#checking-the-total-size) section
  above, an upload package is not representative of your end users' download
  size. Be aware that redundant native library architectures and asset densities
  seen in the breakdown tool can be filtered by the Play Store and App Store.
  
  正如 [檢查總大小](#checking-the-total-size) 一節所述，
  上傳套件的大小並不代表使用者最終的下載大小。請注意，拆分工具中顯示的冗餘的原生庫結構和資源密度，
  都可以透過 Play 商店和 App Store 過濾。
  
{{site.alert.end}}

The size analysis tool is invoked by passing the `--analyze-size` flag when
building:

該大小分析工具透過在建構時新增 `--analyze-size` 標記來呼叫：

- `flutter build apk --analyze-size`
- `flutter build appbundle --analyze-size`
- `flutter build ios --analyze-size`
- `flutter build linux --analyze-size`
- `flutter build macos --analyze-size`
- `flutter build windows --analyze-size`

This build is different from a standard release build in two ways.

這種建構模式和標準的發行建構相比，有以下兩方面的區別：

1. The tool compiles Dart in a way that records code size usage of Dart
   packages.
   
   該工具編譯 Dart 時，記錄了 Dart 套件的程式碼大小使用情況。
   
2. The tool displays a high level summary of the size breakdown
   in the terminal, and leaves a `*-code-size-analysis_*.json` file for more
   detailed analysis in DevTools.
   
   該工具在終端上展示了大小拆分的摘要資訊，並在 DevTools 中生成了一個
   `*-code-size-analysis_*.json` 檔案，用於進行更詳細的分析。

In addition to analyzing a single build, two builds can also be diffed by
loading two `*-code-size-analysis_*.json` files into DevTools. See
[DevTools documentation][] for details.

除了分析單個建構，你還可以在 DevTools 中載入兩個 `*-code-size-analysis_*.json`
檔案比較差異。詳情請閱讀 [DevTools 文件][DevTools documentation]。

{% include docs/app-figure.md image="perf/size-summary.png" alt="Size summary of an Android application in terminal" %}

Through the summary, you can get a quick idea of the size usage per category
(such as asset, native code, Flutter libraries, etc). The compiled Dart
native library is further broken down by package for quick analysis.

透過總結，你可以快速瞭解每種型別（例如資源、原生程式碼、Flutter 庫等）的大小使用情況。
編譯後的 Dart 原生庫會按包進一步拆分，以便快速分析。

{{site.alert.warning}}
  This tool on iOS creates a .app rather than an IPA. Use this tool to
  evaluate the relative size of the .app's content. To get
  a closer estimate of the download size, reference the
  [Estimating total size](#estimating-total-size) section above.

  在 iOS 上，該工具會建立一個 .app 檔案，而不是一個 IPA 包檔案。
  使用該工具可以評估 .app 內容的相對大小。為了獲取更準確的下載大小的估算值，
  請參考上面的 [估算總大小](#estimating-total-size) 一節。
  
{{site.alert.end}}

### Deeper analysis in DevTools

### 在 DevTools 中深入分析

The `*-code-size-analysis_*.json` file produced above can be further
analyzed in deeper detail in DevTools where a tree or a treemap view can
break down the contents of the application into the individual file level and
up to function level for the Dart AOT artifact.

上面產生的 `*-code-size-analysis_*.json` 檔案可以在 DevTools 中進一步深入分析，
樹和樹狀圖可以將應用內容分割至單檔案級別，也可以達到 Dart AOT 產物的函式級別。

This can be done by `flutter pub global run devtools`, selecting
`Open app size tool` and uploading the JSON file.

可以透過 `flutter pub global run devtools` 開啟 DevTools，
選擇 `Open app size tool`，然後上傳 JSON 檔案。

{% include docs/app-figure.md image="perf/devtools-size.png" alt="Example breakdown of app in DevTools" %}

For further information on using the DevTools app size tool, see
[DevTools documentation][].

更多關於 DevTools 中應用大小工具的使用，請看
[DevTools 文件][DevTools documentation]。

## Reducing app size

## 減少應用大小

When building a release version of your app,
consider using the `--split-debug-info` tag.
This tag can dramatically reduce code size.
For an example of using this tag, see
[Obfuscating Dart code][].

當建構應用的發行版本時，考慮使用 `--split-debug-info` 標記。
該標記會顯著減少程式碼量。關於使用此標記的範例，
請檢視文件 [Obfuscating Dart code][]。

Some other things you can do to make your app smaller are:

其他減少應用大小的方式：

* Remove unused resources

  刪除無用的資源

* Minimize resource imported from libraries

  儘量減少從庫中引入的資源

* Compress PNG and JPEG files

  壓縮 PNG 和 JPEG 檔案

[FAQ]: {{site.url}}/resources/faq
[How big is the Flutter engine?]: {{site.url}}/resources/faq#how-big-is-the-flutter-engine
[instructions]: {{site.url}}/deployment/ios
[Xcode App Size Report]: {{site.apple-dev}}/documentation/xcode/reducing_your_app_s_size#3458589
[iOS create build archive instructions]: {{site.url}}/deployment/ios#update-the-apps-build-and-version-numbers
[Model ID / Hardware number]: https://en.wikipedia.org/wiki/List_of_iOS_devices#Models
[Obfuscating Dart code]: {{site.url}}/deployment/obfuscate
[Test drive]: {{site.url}}/get-started/test-drive
[Write your first Flutter app]: {{site.url}}/get-started/codelab
[Play Console's instructions]: https://support.google.com/googleplay/android-developer/answer/9302563?hl=en
[Google Play Console]: https://play.google.com/apps/publish/
[DevTools documentation]: {{site.url}}/development/tools/devtools/app-size
