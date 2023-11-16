---
title: An introduction to integration testing
title: 整合測試介紹
description: Learn about integration testing in Flutter.
description: 瞭解 Flutter 中的整合測試。
short-title: Introduction
short-title: 介紹
prev:
  title: Take a picture using the camera
  path: /cookbook/plugins/picture-using-camera
next:
  title: Performance profiling
  path: /cookbook/testing/integration/profiling
---

<?code-excerpt path-base="cookbook/testing/integration/introduction/"?>

Unit tests and widget tests are handy for testing individual classes,
functions, or widgets. However, they generally don't test how
individual pieces work together as a whole, or capture the performance
of an application running on a real device. These tasks are performed
with *integration tests*.

Unit tests 和 Widget tests 在測試獨立的類、函式或者元件時非常方便。
然而，它們並不能夠測試單獨的模組形成的整體或者獲取真實裝置上應用執行狀態。
這些任務需要整合測試 (**integration tests**) 來處理。 

Integration tests are written using the [integration_test][] package, provided
by the SDK. 

整合測試由 SDK 直接提供支援，使用 [integration_test][] 這個 package 實現。

In this recipe, learn how to test a counter app. It demonstrates
how to setup integration tests, how to verify specific text is displayed
by the app, how to tap specific widgets, and how to run integration tests.

在這個章節中，我們將會學習如何去測試一個計數器應用，
包括如何設定整合測試、
如何驗證指定文字能否在應用內正常顯示、
如何模擬點選指定元件和如何執行整合測試。

This recipe uses the following steps:

本課程將包含以下步驟：

  1. Create an app to test.

     建立一個應用用於測試。

  2. Add the `integration_test` dependency.

     新增 `integration_test` 依賴。

  3. Create the test files.

     建立測試檔案。

  4. Write the integration test.

     編寫整合測試。

  5. Run the integration test.

     執行整合測試。

### 1. Create an app to test

### 1. 建立一個應用用於測試

First, create an app for testing. In this example,
test the counter app produced by the `flutter create`
command. This app allows a user to tap on a button
to increase a counter.

首先，我們需要建立一個應用用於測試。
在這個範例中，我們將會測試一個由 `flutter create` 命令建立的計數器應用。
這個應用允許使用者點選按鈕增加計數。

<?code-excerpt "lib/main.dart"?>
```dart
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Counter App',
      home: MyHomePage(title: 'Counter App Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        // Provide a Key to this button. This allows finding this
        // specific button inside the test suite, and tapping it.
        key: const Key('increment'),
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

### 2. Add the `integration_test` dependency

### 2. 新增 `integration_test` 依賴

Next, use the `integration_test` and `flutter_test` packages
to write integration tests. Add these dependencies to the `dev_dependencies`
section of the app's `pubspec.yaml` file.

接著，我們需要用到 `integration_test` 和 `flutter_test` package
來編寫整合測試，把依賴新增到應用`pubspec.yaml` 檔案的
`dev_dependencies` 區域。

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

### 3. Create the test files

### 3. 建立測試檔案

Create a new directory, `integration_test`, with an empty `app_test.dart` file:

建立一個名為 `integration_test` 的新資料夾，
並在資料夾中建立一個空的 `app_test.dart` 檔案： 

```
counter_app/
  lib/
    main.dart
  integration_test/
    app_test.dart
```

### 4. Write the integration test

### 4. 編寫整合測試檔案

Now you can write tests. This involves three steps:

現在我們可以來寫測試檔案了，步驟如下列三項：

  1. Initialize `IntegrationTestWidgetsFlutterBinding`, a singleton service that
     executes tests on a physical device.

     初始化一個單例 `IntegrationTestWidgetsFlutterBinding`，
     這將用於在物理裝置上執行測試；

  2. Interact and tests widgets using the `WidgetTester` class.

     使用 `WidgetTester` 類測試並與 widget 發生互動；

  3. Test the important scenarios.

     測試重要的應用場景。

<?code-excerpt "integration_test/app_test.dart (IntegrationTest)"?>
```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:integration_test/integration_test.dart';
import 'package:introduction/main.dart';

void main() {
  IntegrationTestWidgetsFlutterBinding.ensureInitialized();

  group('end-to-end test', () {
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
      expect(find.text('1'), findsOneWidget);
    });
  });
}
```

### 5. Run the integration test

### 5. 執行整合測試

The process of running the integration tests varies depending on the platform
you are testing against. You can test against a mobile platform or the web.

整合測試的執行情況會根據需要進行測試的平台不同而不盡相同，
你可以針對行動平台或者 Web 平台進行測試。

#### 5a. Mobile

#### 5a. 行動平台

To test on a real iOS / Android device, first connect the device and run the
following command from the root of the project:

在 iOS 或 Android 平台進行真機測試的時候，
首先需要連線裝置並在工程的根目錄執行下面的命令：

```terminal
flutter test integration_test/app_test.dart
```

Or, you can specify the directory to run all integration tests:

或者你可以在指定目錄下執行所有的整合測試：

```terminal
flutter test integration_test
```

This command runs the app and integration tests on the target device. For more
information, see the [Integration testing][] page.

這個命令可以在目標裝置上執行應用並執行整合測試，更多相關資訊，
請參閱文件：[整合測試][Integration testing] 頁面。

#### 5b. Web

#### 5b. Web 平台

<!--
TODO(ryjohn): Add back after other WebDriver versions are supported:
https://github.com/flutter/flutter/issues/90158

To test for web,
determine which browser you want to test against
and download the corresponding web driver:

  * Chrome: [Download ChromeDriver][]
  * Firefox: [Download GeckoDriver][]
  * Safari: Safari can only be tested on a Mac;
    the SafariDriver is already installed on Mac machines.
  * Edge [Download EdgeDriver][]
-->

To get started testing in a web browser, [Download ChromeDriver][].

在網頁瀏覽器裡開始進行整合測試，首先要下載 [ChromeDriver][Download ChromeDriver]。

Next, create a new directory named `test_driver` containing a new file
named `integration_test.dart`:

接下來，新建一個資料夾，命名為 `test_driver`，幷包含一個新的檔案，命名為
`integration_test.dart`。

<?code-excerpt "test_driver/integration_test.dart"?>
```dart
import 'package:integration_test/integration_test_driver.dart';

Future<void> main() => integrationDriver();
```

Launch `chromedriver` as follows: 

執行 `chromedriver`，執行如下命令：

```terminal
chromedriver --port=4444
```

From the root of the project, run the following command:

在工程的根目錄下，執行如下命令：

```terminal
flutter drive \
  --driver=test_driver/integration_test.dart \
  --target=integration_test/app_test.dart \
  -d chrome
```

For a headless testing experience, you can also run `flutter drive` 
with `web-server` as the target device identifier as follows:

如需 Headless 測試體驗，你同樣可以執行 `flutter drive` 命令，
並加入 `web-server` 作為目標裝置，參考如下命令：

```terminal
flutter drive \
  --driver=test_driver/integration_test.dart \
  --target=integration_test/app_test.dart \
  -d web-server
```

[Download ChromeDriver]: https://chromedriver.chromium.org/downloads
[Download EdgeDriver]: https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/
[Download GeckoDriver]: {{site.github}}/mozilla/geckodriver/releases
[flutter_driver]: {{site.api}}/flutter/flutter_driver/flutter_driver-library.html
[integration_test]: {{site.repo.flutter}}/tree/main/packages/integration_test
[Integration testing]: {{site.url}}/testing/integration-tests
[`SerializableFinders`]: {{site.api}}/flutter/flutter_driver/CommonFinders-class.html
[`ValueKey`]: {{site.api}}/flutter/foundation/ValueKey-class.html
