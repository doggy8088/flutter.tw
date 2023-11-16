---
title: Performance profiling
title: 效能分析
description: How to profile performance for a Flutter app.
description: 本篇將如何測量你的 Flutter 應用的效能。
tags: cookbook, 實用課程, 測試
keywords: 效能最佳化,卡頓,時間軸
---

<?code-excerpt path-base="cookbook/testing/integration/profiling/"?>

When it comes to mobile apps, performance is critical to user experience.
Users expect apps to have smooth scrolling and meaningful animations free of
stuttering or skipped frames, known as "jank." How to ensure that your app
is free of jank on a wide variety of devices?

效能移動應用使用者來說相當重要，使用者希望應用程式有流暢的滾動和優雅的動畫，
不願看到卡頓和掉幀現象。我們如何確保我們的應用程式在各種裝置上不會受到卡頓的影響？

There are two options: first, manually test the app on different devices.
While that approach might work for a smaller app, it becomes more
cumbersome as an app grows in size. Alternatively, run an integration
test that performs a specific task and records a performance timeline.
Then, examine the results to determine whether a specific section of
the app needs to be improved.

以下兩種方式可供選擇：首先，我們可以在不同的裝置對應用程式進行手動測試。
這種方式適用於較小的應用程式，但隨著應用程式擴充性的提升，它將變得更加繁瑣。
另外，我們可以執行整合測試，執行特定任務並記錄效能時間軸。
然後，我們可以檢驗結果，以確定是否需要對我們應用程式的特定部分進行改善。

In this recipe, learn how to write a test that records a performance
timeline while performing a specific task and saves a summary of the
results to a local file.

在本文中，我們將學習如何在執行特定任務時編寫記錄效能時間軸的測試，
並將結果的摘要儲存到本地檔案中。

This recipe uses the following steps:

步驟：

  1. Write a test that scrolls through a list of items.

     編寫一個滾動列表的測試專案；

  2. Record the performance of the app.

     記錄應用程式的效能；

  3. Save the results to disk.

     將結果儲存到磁碟；

  4. Run the test.

     執行測試；

  5. Review the results.

     檢查結果。

### 1. Write a test that scrolls through a list of items

### 1.  編寫一個滾動列表的測試專案

In this recipe, record the performance of an app as it scrolls through a
list of items. To focus on performance profiling, this recipe builds
on the [Scrolling][] recipe in widget tests.

在這一小節，我們將記錄當滾動列表條目時應用程式的效能。
為了專注於效能分析，這一小節在元件測試中
[Scrolling in integration tests（列表滾動整合測試）][Scrolling]
的基礎上進行。

Follow the instructions in that recipe to create an app and write a test to
verify that everything works as expected.

請按照基礎章節的指南新建一個應用程式，編寫一個測試程式。
最終，確保應用程式按預期執行。

### 2. Record the performance of the app

### 2. 記錄應用程式的效能

Next, record the performance of the app as it scrolls through the
list. Perform this task using the [`traceAction()`][]
method provided by the [`IntegrationTestWidgetsFlutterBinding`][] class.

然後，我們需要再應用程式的列表滾動的時候記錄它的效能。
使用 [`IntegrationTestWidgetsFlutterBinding`][] 類中的 [`traceAction()`][] 方法實現這項功能。

This method runs the provided function and records a [`Timeline`][]
with detailed information about the performance of the app. This example
provides a function that scrolls through the list of items,
ensuring that a specific item is displayed. When the function completes,
the `traceAction()` creates a report data `Map` that contains the `Timeline`.

這種方式執行提供的方法，並將應用程式效能的詳細資訊記錄在 [`Timeline`][] 中。
在這個範例中，我們提供一個方法，用以滾動列表的條目並確保指定條目是否被顯示出來。
當方法執行完成的時候，`traceAction()` 會返回一個 `Timeline`。

Specify the `reportKey` when running more than one `traceAction`.
By default all `Timelines` are stored with the key `timeline`,
in this example the `reportKey` is changed to `scrolling_timeline`.

當執行一個以上的 `traceAction` 的時候需要指定 `reportKey`。
預設情況下，所有的 `Timelines` 都會存在 `timeline` 裡，
在這個例子中，`reportKey` 被修改為了 `scrolling_timeline`:

<?code-excerpt "integration_test/scrolling_test.dart (traceAction)"?>
```dart
await binding.traceAction(
  () async {
    // Scroll until the item to be found appears.
    await tester.scrollUntilVisible(
      itemFinder,
      500.0,
      scrollable: listFinder,
    );
  },
  reportKey: 'scrolling_timeline',
);
```

### 3. Save the results to disk

### 3. 將結果儲存到磁碟

Now that you've captured a performance timeline, you need a way to review it.
The `Timeline` object provides detailed information about all of the events
that took place, but it doesn't provide a convenient way to review the results.

我們已經獲取了一個性能時間軸，我們需要一種方式來對它進行檢驗，
`Timeline` 物件提供所有已發生事件的相關詳細資訊，但它不提供快捷方式檢視結果。

Therefore, convert the `Timeline` into a [`TimelineSummary`][].
The `TimelineSummary` can perform two tasks that make it easier
to review the results:

因此，我們可以將 `Timeline` 轉換成 [`TimelineSummary`][]，
`TimelineSummary` 透過執行兩個任務可以使我們更容易的檢查結果：

  1. Writing a json document on disk that summarizes the data contained
     within the `Timeline`. This summary includes information about the
     number of skipped frames, slowest build times, and more.

     將一個 json 檔案寫入磁碟，它包含了 `Timeline` 中包含的資料的摘要。
     此摘要包括掉幀數量，最慢建構時間等的資訊。

  2. Saving the complete `Timeline` as a json file on disk.
     This file can be opened with the Chrome browser's
     tracing tools found at `chrome://tracing`.

     它可以將完整的 `Timeline` 以 json 檔案的形式儲存在磁碟上，
     可以使用 Chrome 瀏覽器的追蹤工具開啟此檔案。
     追蹤工具在這裡: `chrome://tracing`。

To capture the results, create a file named `perf_driver.dart`
in the `test_driver` folder and add the following code:

為了捕獲結果內容，需要在 `test_driver` 資料夾中
新建一個 `perf_driver.dart` 檔案，並加入如下程式碼:

<?code-excerpt "test_driver/perf_driver.dart"?>
```dart
import 'package:flutter_driver/flutter_driver.dart' as driver;
import 'package:integration_test/integration_test_driver.dart';

Future<void> main() {
  return integrationDriver(
    responseDataCallback: (data) async {
      if (data != null) {
        final timeline = driver.Timeline.fromJson(
          data['scrolling_timeline'] as Map<String, dynamic>,
        );

        // Convert the Timeline into a TimelineSummary that's easier to
        // read and understand.
        final summary = driver.TimelineSummary.summarize(timeline);

        // Then, write the entire timeline to disk in a json format.
        // This file can be opened in the Chrome browser's tracing tools
        // found by navigating to chrome://tracing.
        // Optionally, save the summary to disk by setting includeSummary
        // to true
        await summary.writeTimelineToFile(
          'scrolling_timeline',
          pretty: true,
          includeSummary: true,
        );
      }
    },
  );
}
```

The `integrationDriver` function has a `responseDataCallback` 
which you can customize. 
By default, it writes the results to the `integration_response_data.json` file,
but you can customize it to generate a summary like in this example.

你可以自訂 `integrationDriver` 函式的 `responseDataCallback` 方法，
預設情況下，它會將結果寫入 `integration_response_data.json` 檔案，
不過你也可以透過這個例子裡的方法重寫為產生摘要。

### 4. Run the test

### 4. 執行測試

After configuring the test to capture a performance `Timeline` and save a
summary of the results to disk, run the test with the following command:

在我們為了捕獲一個性能 `Timeline` 配置了測試程式碼，並且將結果的摘要儲存在了磁碟上，
我們可以使用以下命令執行測試程式碼：

```
flutter drive \
  --driver=test_driver/perf_driver.dart \
  --target=integration_test/scrolling_test.dart \
  --profile
```

The `--profile` option means to compile the app for the "profile mode" 
rather than the "debug mode", so that the benchmark result is closer to 
what will be experienced by end users. 

`--profile` 命令列選項代表著應用將以 profile 模式 (效能模式) 執行，
這種模式下執行的應用會比 debug 模式更接近終端使用者的體驗。

{{site.alert.note}}

  Run the command with `--no-dds` when running on a mobile device or emulator.
  This option disables the Dart Development Service (DDS), which won't
  be accessible from your computer.

  在真機或者模擬器上執行的時候，加入 `--no-dds` 命令列選項代表著禁用 Dart 開發環境服務 (DDS)，
  此時就無法在電腦上存取。

{{site.alert.end}}

### 5. Review the results

### 5. 檢查結果

After the test completes successfully, the `build` directory at the root of
the project contains two files:

在測試程式碼執行成功以後，在專案根目錄下的 `build` 資料夾裡包含以下兩個檔案：

  1. `scrolling_summary.timeline_summary.json` contains the summary. Open
     the file with any text editor to review the information contained
     within.  With a more advanced setup, you could save a summary every
     time the test runs and create a graph of the results.

     `scrolling_summary.timeline_summary.json` 包含摘要。
     可以使用任何文字編輯器開啟它並檢視其中包含的資訊。
     透過更進階的設定，我們可以在每次測試時儲存摘要並建立一個結果圖。

  2. `scrolling_timeline.timeline.json` contains the complete timeline data.
     Open the file using the Chrome browser's tracing tools found at
     `chrome://tracing`. The tracing tools provide a
     convenient interface for inspecting the timeline data to discover
     the source of a performance issue.

     `scrolling_timeline.timeline.json` 包含完整的時間軸資料。
     使用 Chorme 瀏覽器的追蹤工具開啟這個檔案。
     追蹤工具在這裡：`chrome://tracing`。
     追蹤工具提供了一個便捷的使用者介面，用以檢測時間軸資料平行處理現其中導致效能問題的源頭。

#### Summary example

#### 摘要的範例

```json
{
  "average_frame_build_time_millis": 4.2592592592592595,
  "worst_frame_build_time_millis": 21.0,
  "missed_frame_build_budget_count": 2,
  "average_frame_rasterizer_time_millis": 5.518518518518518,
  "worst_frame_rasterizer_time_millis": 51.0,
  "missed_frame_rasterizer_budget_count": 10,
  "frame_count": 54,
  "frame_build_times": [
    6874,
    5019,
    3638
  ],
  "frame_rasterizer_times": [
    51955,
    8468,
    3129
  ]
}
```

### Complete example

### 完整範例

**integration_test/scrolling_test.dart**

<?code-excerpt "integration_test/scrolling_test.dart"?>
```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:integration_test/integration_test.dart';

import 'package:scrolling/main.dart';

void main() {
  final binding = IntegrationTestWidgetsFlutterBinding.ensureInitialized();

  testWidgets('Counter increments smoke test', (tester) async {
    // Build our app and trigger a frame.
    await tester.pumpWidget(MyApp(
      items: List<String>.generate(10000, (i) => 'Item $i'),
    ));

    final listFinder = find.byType(Scrollable);
    final itemFinder = find.byKey(const ValueKey('item_50_text'));

    await binding.traceAction(
      () async {
        // Scroll until the item to be found appears.
        await tester.scrollUntilVisible(
          itemFinder,
          500.0,
          scrollable: listFinder,
        );
      },
      reportKey: 'scrolling_timeline',
    );
  });
}
```

**test_driver/perf_driver.dart**

<?code-excerpt "test_driver/perf_driver.dart"?>
```dart
import 'package:flutter_driver/flutter_driver.dart' as driver;
import 'package:integration_test/integration_test_driver.dart';

Future<void> main() {
  return integrationDriver(
    responseDataCallback: (data) async {
      if (data != null) {
        final timeline = driver.Timeline.fromJson(
          data['scrolling_timeline'] as Map<String, dynamic>,
        );

        // Convert the Timeline into a TimelineSummary that's easier to
        // read and understand.
        final summary = driver.TimelineSummary.summarize(timeline);

        // Then, write the entire timeline to disk in a json format.
        // This file can be opened in the Chrome browser's tracing tools
        // found by navigating to chrome://tracing.
        // Optionally, save the summary to disk by setting includeSummary
        // to true
        await summary.writeTimelineToFile(
          'scrolling_timeline',
          pretty: true,
          includeSummary: true,
        );
      }
    },
  );
}
```


[`IntegrationTestWidgetsFlutterBinding`]: {{site.api}}/flutter/package-integration_test_integration_test/IntegrationTestWidgetsFlutterBinding-class.html
[Scrolling]: {{site.url}}/cookbook/testing/widget/scrolling
[`Timeline`]: {{site.api}}/flutter/flutter_driver/Timeline-class.html
[`TimelineSummary`]: {{site.api}}/flutter/flutter_driver/TimelineSummary-class.html
[`traceAction()`]: {{site.api}}/flutter/flutter_driver/FlutterDriver/traceAction.html
