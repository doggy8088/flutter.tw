---
title: Integration testing
title: 整合測試
description: Learn how to write integration tests
description: 學習如何撰寫整合測試
tags: 測試,Flutter Test,整合測試
---

<?code-excerpt path-base="testing/integration_tests/how_to"?>

This page describes how to use the [`integration_test`][] package to run
integration tests. Tests written using this package have the following
properties:

本篇描述瞭如何使用 [`integration_test`][] package 來執行整合測試。
使用該 package 編寫的測試具有以下特性：
 
* Compatibility with the `flutter drive` command,
  for running tests on a physical device or emulator.

  相容 `flutter drive` 指令，
  用於在真機或模擬器上執行測試。

* The ability to be run on [Firebase Test Lab][],
  enabling automated testing on a variety of devices.

  能夠透過 [Firebase Test Lab][] 在各種裝置上進行自動化測試。

* Compatibility with [flutter_test][] APIs,
  enabling tests to be written in a similar style as [widget tests][] 

  相容 [flutter_test][] API，
  能夠使用類似 [widget 測試][widget tests] 的風格進行編寫。

{{site.alert.note}}

  The `integration_test` package is part of the Flutter SDK itself. 
  To use it, make sure that you update your app's pubspec file
  to include this package as one of your `dev_dependencies`. 
  For an example, see the [Project setup](#project-setup) section below.

  Flutter SDK 本身自帶 `integration_test` package。
  要使用它，請確保你的應用程式 pubspec 檔案 `dev_dependencies` 中引入了它。
  請檢視 [專案設定](#project-setup) 的範例。

{{site.alert.end}}

## Overview

## 概覽

**Unit tests, widget tests, and integration tests**

**單元測試、Widget 測試和整合測試**

There are three types of tests that Flutter supports.
A **unit** test verifies the behavior of a method or class.
A **widget test** verifies the behavior of Flutter widgets
without running the app itself. An **integration test** (also
called end-to-end testing or GUI testing) runs the full app.

Flutter 支援三種類型的測試。
**單元測試** 驗證一個方法或類別的行為。
**Widget 測試** 無需執行應用程式就可以驗證 Flutter widget 的行為。
**整合測試**（又名端到端測試或 GUI 測試）執行整個應用程式。

**Hosts and targets**

**宿主和目標裝置**

During development, you are probably writing the code
on a desktop computer, called the **host** machine,
and running the app on a mobile device, browser,
or desktop application, called the **target** device.
(If you are using a web
browser or desktop application,
the host machine is also the target device.)

在開發過程中，**宿主** 就是你的編碼平台，應用程式在 **目標** 裝置上執行，
就像你在臺式電腦上編寫程式碼，並在移動裝置、瀏覽器或者桌面程式上執行你的應用程式，
（如果你使用的是 web 瀏覽器或桌面應用程式，宿主也是目標裝置）

**integration_test**

**integration_test package**

Tests written with the `integration_test` package can:

用 `integration_test` package 編寫的測試可以：
 
1. Run directly on the target device, allowing you to test on
   multiple Android or iOS devices using Firebase Test Lab.

   直接在目標裝置上執行，
   允許使用 Firebase Test Lab 在多個 Android/iOS 裝置上進行測試。

2. Run using `flutter test integration_test`. 

   使用 `flutter test integration_test` 指令執行。

3. Use `flutter_test` APIs, making integration tests more
   like writing [widget tests][].

   使用 `flutter_test` API，
   讓整合測試編寫風格更像 [Widget 測試][widget tests]。

**Migrating from flutter_driver**

**從 flutter_driver 遷移**

Existing projects using `flutter_driver` can be migrated to
`integration_test` by following the [Migrating from flutter_drive][]
guide.

現在還在使用 `flutter_driver` 的專案可以透過以下方式遷移到 `integration_test`，
請檢視 [從 flutter_driver 遷移][Migrating from flutter_drive] 指南。

## Project setup

## 專案設定

Add `integration_test` and `flutter_test` to your `pubspec.yaml` file:

在你的 `pubspec.yaml` 檔案中加入 `integration_test` 和 `flutter_test`：

```console
$ flutter pub add 'dev:flutter_test:{"sdk":"flutter"}'  'dev:integration_test:{"sdk":"flutter"}'
"flutter_test" is already in "dev_dependencies". Will try to update the constraint.
Resolving dependencies... 
  collection 1.17.2 (1.18.0 available)
+ file 6.1.4 (7.0.0 available)
+ flutter_driver 0.0.0 from sdk flutter
+ fuchsia_remote_debug_protocol 0.0.0 from sdk flutter
+ integration_test 0.0.0 from sdk flutter
  material_color_utilities 0.5.0 (0.8.0 available)
  meta 1.9.1 (1.10.0 available)
+ platform 3.1.0 (3.1.2 available)
+ process 4.2.4 (5.0.0 available)
  stack_trace 1.11.0 (1.11.1 available)
  stream_channel 2.1.1 (2.1.2 available)
+ sync_http 0.3.1
  test_api 0.6.0 (0.6.1 available)
+ vm_service 11.7.1 (11.10.0 available)
+ webdriver 3.0.2
Changed 9 dependencies!
```

In your project, create a new directory
`integration_test` with a new file, `<name>_test.dart`:

在你的專案中，建立一個 `integration_test` 目錄，
新目錄中建立一個新檔案 `<name>_test.dart`：

<?code-excerpt "integration_test/counter_test.dart (initial)" plaster="none"?>
```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:how_to/main.dart';
import 'package:integration_test/integration_test.dart';

void main() {
  testWidgets('tap on the floating action button, verify counter',
      (tester) async {
    // Load app widget.
    await tester.pumpWidget(const MyApp());

    // Verify the counter starts at 0.
    expect(find.text('0'), findsOneWidget);

    // Finds the floating action button to tap on.
    final fab = find.byKey(const Key('increment'));

    // Emulate a tap on the floating action button.
    await tester.tap(fab);

    // Trigger a frame.
    await tester.pumpAndSettle();

    // Verify the counter increments by 1.
    expect(find.text('0'), findsNothing);
    expect(find.text('1'), findsOneWidget);
  });
}
```

{{site.alert.note}}

  Note: You should only use `testWidgets`
  to declare your tests, or errors won't
  be reported correctly.

  注意：你只能使用 `testWidgets` 來宣告你的測試，
  否則錯誤將無法正確丟擲。

{{site.alert.end}}

### Directory structure

### 目錄結構

```yaml
lib/
  ...
integration_test/
  foo_test.dart
  bar_test.dart
test/
  # Other unit tests go here.
```

See also:

另見：

 * [integration_test usage][]

## Running using the flutter command

## 使用 Flutter 指令執行測試

These tests can be launched with the
`flutter test` command, where `<DEVICE_ID>`:
is the optional device ID or pattern displayed
in the output of the `flutter devices` command:

這些測試可以用 `flutter test` 指令執行，
其中 `<DEVICE_ID>`：是可選項，可以透過 `flutter devices` 指定裝置 ID 或顯示模式：

```bash
flutter test integration_test/foo_test.dart -d <DEVICE_ID>
```

This runs the tests in `foo_test.dart`. To run all tests in this directory on
the default device, run:

上面的指令將執行 `foo_test.dart` 中的測試。
要在預設裝置上執行該目錄下的所有測試，請執行：

```
flutter test integration_test
```

### Running in a browser 

### 在瀏覽器中執行測試

First, [Download and install ChromeDriver][]
and run it on port 4444:

首先，[下載安裝 ChromeDriver][Download and install ChromeDriver] 並在 4444 埠執行：

```
chromedriver --port=4444
```

To run tests with `flutter drive`, create a new directory containing a new file,
`test_driver/integration_test.dart`:

為了使用 `flutter drive` 進行測試，請建立一個新的目錄，
其中包含一個新的檔案 `test_driver/integration_test.dart`：

<?code-excerpt "test_driver/integration_test.dart"?>
```dart
import 'package:integration_test/integration_test_driver.dart';

Future<void> main() => integrationDriver();
```

Then add `IntegrationTestWidgetsFlutterBinding.ensureInitialized()` in your 
`integration_test/<name>_test.dart` file:

然後將 `IntegrationTestWidgetsFlutterBinding.ensureInitialized()` 新增到
你的 `integration_test/<name>_test.dart` 檔案中：

<?code-excerpt "integration_test/counter_test.dart"?>
```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:how_to/main.dart';
import 'package:integration_test/integration_test.dart';

void main() {
  IntegrationTestWidgetsFlutterBinding.ensureInitialized(); // NEW

  testWidgets('tap on the floating action button, verify counter',
      (tester) async {
    // Load app widget.
    await tester.pumpWidget(const MyApp());

    // Verify the counter starts at 0.
    expect(find.text('0'), findsOneWidget);

    // Finds the floating action button to tap on.
    final fab = find.byKey(const Key('increment'));

    // Emulate a tap on the floating action button.
    await tester.tap(fab);

    // Trigger a frame.
    await tester.pumpAndSettle();

    // Verify the counter increments by 1.
    expect(find.text('0'), findsNothing);
    expect(find.text('1'), findsOneWidget);
  });
}
```

In a separate process, run `flutter_drive`:

最後在一個獨立的處理序中，執行 `flutter_drive`：

```
flutter drive \
  --driver=test_driver/integration_test.dart \
  --target=integration_test/counter_test.dart \
  -d web-server
```

To learn more, see the
[Running Flutter driver tests with web][] wiki page.

瞭解更多資訊，請檢視 [Running Flutter driver tests with web][] 指南。

## Testing on Firebase Test Lab 

## 在 Firebase Test Lab 進行測試

You can use the Firebase Test Lab with both Android
and iOS targets.

你可以透過 Firebase Test Lab 同時使用 Android 和 iOS 目標裝置進行測試。

### Android setup

### Android 設定

Follow the instructions in the [Android Device Testing][]
section of the README.

請遵循 `integration_test` README 中的 [Android Device Testing][] 進行設定。

### iOS setup

### iOS 設定

Follow the instructions in the [iOS Device Testing][]
section of the README.

請遵循 `integration_test` README 中的 [iOS Device Testing][] 進行設定。

### Test Lab project setup

### Firebase Test Lab 專案設定

Go to the [Firebase Console][],
and create a new project if you don't have one
already. Then navigate to **Quality > Test Lab**:

進入 [Firebase 控制檯][Firebase Console],
如果你還沒有新的專案，就建立一個新專案。
然後導航選單到 **Quality > Test Lab**:

<img src='/assets/images/docs/integration-test/test-lab-1.png' class="mw-100" alt="Firebase Test Lab Console">

### Uploading an Android APK

### 上傳 Android APK

Create an APK using Gradle:

使用 Gradle 建構一個 APK：

```bash
pushd android
# flutter build generates files in android/ for building the app
flutter build apk
./gradlew app:assembleAndroidTest
./gradlew app:assembleDebug -Ptarget=integration_test/<name>_test.dart
popd
```

Where `<name>_test.dart` is the file created in the
**Project Setup** section.

上面指令中的 `<name>_test.dart` 是在剛才 **專案設定** 建立的檔案。

{{site.alert.note}}

  To use `--dart-define` with `gradlew`, you must `base64` encode
  all parameters, and pass them to gradle in a comma separated list:

  要在 `gradlew` 中使用 `--dart-define`，你必須對所有引數進行 `base64` 編碼，
  並將它們作為逗號分隔的列表傳遞給 gradle：

  ```bash
  ./gradlew project:task -Pdart-defines="{base64(key=value)},[...]"
  ```
{{site.alert.end}}

Drag the "debug" APK from
`<flutter_project_directory>/build/app/outputs/apk/debug`
into the **Android Robo Test** target on the web page.
This starts a Robo test and allows you to run
other tests:

將 `<flutter_project_directory>/build/app/outputs/apk/debug` 內
打包完成的 "debug" APK 檔案上傳到網頁上的 **Android Robo Test**。
這將啟動一個 Robo 測試，並允許你執行其他測試：

<img src='/assets/images/docs/integration-test/test-lab-2.png' class="mw-100" alt="Firebase Test Lab upload">

Click **Run a test**,
select the **Instrumentation** test type and drag
the following two files:

點選 **Run a test**,
選擇 **Instrumentation** 測試型別，
並上傳以下兩個檔案
 
 * `<flutter_project_directory>/build/app/outputs/apk/debug/<file>.apk`
 * `<flutter_project_directory>/build/app/outputs/apk/androidTest/debug/<file>.apk`

<img src='/assets/images/docs/integration-test/test-lab-3.png' class="mw-100" alt="Firebase Test Lab upload two APKs">

If a failure occurs,
you can view the output by selecting the red icon:

如果發生故障，
你可以透過選擇紅色圖示檢視輸出內容：

<img src='/assets/images/docs/integration-test/test-lab-4.png' class="mw-100" alt="Firebase Test Lab test results">

### Uploading an Android APK from the command line

### 透過指令上傳 Android APK

See the [Firebase Test Lab section of the README][]
for instructions on uploading the APKs from the command line.

關於透過指令上傳 APK 的說明，
請檢視 `integration_test` README 中的 [Firebase Test Lab][Firebase Test Lab section of the README]。

### Uploading Xcode tests

### 上傳 Xcode 測試

See the [Firebase TestLab iOS instructions][]
for details on how to upload the .zip file
to the Firebase TestLab section of the Firebase Console.

瞭解如何將 .zip 檔案上傳到 Firebase 控制檯的 Firebase Test Lab，
請檢視 [Firebase TestLab iOS instructions][]。

### Uploading Xcode tests from the command line

### 透過指令上傳 Xcode 測試

See the [iOS Device Testing][] section in the README
for instructions on how to upload the .zip file
from the command line.

關於如何透過指令上傳 .zip 檔案的說明，
請檢視 `integration_test` README 中的 [iOS Device Testing][]。

[Android Device Testing]: {{site.repo.flutter}}/tree/main/packages/integration_test#android-device-testing
[Download and install ChromeDriver]: https://chromedriver.chromium.org/downloads
[Firebase Console]: http://console.firebase.google.com/
[Firebase Test Lab]: {{site.firebase}}/docs/test-lab
[Firebase Test Lab section of the README]: {{site.repo.flutter}}/tree/main/packages/integration_test#firebase-test-lab
[Firebase TestLab iOS instructions]: {{site.firebase}}/docs/test-lab/ios/firebase-console
[flutter_test]: {{site.api}}/flutter/flutter_test/flutter_test-library.html
[`integration_test`]: {{site.repo.flutter}}/tree/main/packages/integration_test#integration_test
[integration_test usage]: {{site.repo.flutter}}/tree/main/packages/integration_test#usage
[iOS Device Testing]: {{site.repo.flutter}}/tree/main/packages/integration_test#ios-device-testing
[Migrating from flutter_drive]: {{site.url}}/release/breaking-changes/flutter-driver-migration
[Running Flutter driver tests with web]: {{site.repo.flutter}}/wiki/Running-Flutter-Driver-tests-with-Web
[widget tests]: {{site.url}}/testing/overview#widget-tests
