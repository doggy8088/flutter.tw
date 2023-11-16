---
title: Performance FAQ
title: 效能常見問題和回答
description: Frequently asked questions about Flutter performance
description: 關於 Flutter 效能的常見問題
---

This page collects some frequently asked questions
about evaluating and debugging Flutter's performance.

本篇收集了一些關於評估和除錯 Flutter 效能的常見問題。

* Which performance dashboards have metrics that are related to Flutter?

  哪些效能儀表盤有 Flutter 相關的指標？

  * [Flutter dashboard on appspot][]

    [在 Appspot 上的 Flutter 儀表盤][Flutter dashboard on appspot]

  * [Flutter Skia dashboard][]

    [Flutter Skia 儀表盤][Flutter Skia dashboard]

  * [Flutter Engine Skia dashboard][]

    [Flutter Engine Skia 儀表盤][Flutter Engine Skia dashboard]

[Flutter dashboard on appspot]: https://flutter-dashboard.appspot.com/
[Flutter engine Skia dashboard]: https://flutter-engine-perf.skia.org/t/?subset=regressions
[Flutter Skia dashboard]: https://flutter-flutter-perf.skia.org/t/?subset=regressions

* How do I add a benchmark to Flutter?

  我如何向 Flutter 新增一個基準測試？

  * [How to write a render speed test for Flutter][speed-test]

    [如何編寫 Flutter 的渲染速度測試][speed-test]

  * [How to write a memory test for Flutter][memory-test]

    [如何編寫 Flutter 的記憶體測試][memory-test]

[memory-test]: {{site.repo.flutter}}/wiki/How-to-write-a-memory-test-for-Flutter
[speed-test]: {{site.repo.flutter}}/wiki/How-to-write-a-render-speed-test-for-Flutter

* What are some tools for capturing and analyzing performance
  metrics?

  有哪些捕捉和分析效能指標的工具？

  * [Dart DevTools]({{site.url}}/tools/devtools)
  * [Apple instruments](https://en.wikipedia.org/wiki/Instruments_(software))
  * [Linux perf](https://en.wikipedia.org/wiki/Perf_(Linux))
  * [Chrome tracing (enter `about:tracing` in your
    Chrome URL field)][tracing]

    [Chrome tracing（在瀏覽器位址列輸入 `about:tracing`）][tracing]

  * [Android systrace (`adb systrace`)][systrace]
  * [Fuchsia `fx traceutil`][traceutil]
  * [Perfetto](https://ui.perfetto.dev/)
  * [speedscope](https://www.speedscope.app/)

[systrace]: https://developer.android.com/studio/profile/systrace
[tracing]: https://www.chromium.org/developers/how-tos/trace-event-profiling-tool
[traceutil]: https://fuchsia.dev/fuchsia-src/development/tracing/usage-guide

* My Flutter app looks janky or stutters. How do I fix it?

  我的 Flutter 應用程式很卡頓，效能很糟糕。
  怎麼樣才能修復這些問題？

  * [Improving rendering performance][]

    [提高渲染效能][Improving rendering performance]

[Improving rendering performance]: {{site.url}}/perf/rendering-performance

* What are some costly performance operations that I need
  to be careful with?

  有哪些特別消耗效能的操作是我需要注意的？

  * [`Opacity`][], [`Clip.antiAliasWithSaveLayer`][],
     or anything that triggers [`saveLayer`][]

    [`Opacity`][]、[`Clip.antiAliasWithSaveLayer`][] 
    或任何能觸發 [`saveLayer`][] 的操作。

  * [`ImageFilter`][]
  * Also see [Performance best practices][]

    查閱 [效能最佳化最佳實踐][Performance best practices]

[`Clip.antiAliasWithSaveLayer`]: {{site.api}}/flutter/dart-ui/Clip.html#antiAliasWithSaveLayer
[`ImageFilter`]: {{site.api}}/flutter/dart-ui/ImageFilter-class.html
[`Opacity`]: {{site.api}}/flutter/widgets/Opacity-class.html
[Performance best practices]: {{site.url}}/perf/best-practices
[`savelayer`]: {{site.api}}/flutter/dart-ui/Canvas/saveLayer.html

* How do I tell which widgets in my Flutter app are rebuilt
  in each frame?

  我如何才能知道 Flutter 應用程式中哪些 widget 在哪一幀中被重新建構？

  * Set [`debugProfileBuildsEnabled`][] true in
    [widgets/debug.dart][debug.dart].

    將 [widgets/debug.dart][debug.dart] 
    中的 [`debugProfileBuildsEnabled`][] 設定為 true。

  * Alternatively, change the `performRebuild` function in
    [widgets/framework.dart][framework.dart] to ignore
    `debugProfileBuildsEnabled` and always call
    `Timeline.startSync(...)/finish`.

    或者，改變 [widgets/framework.dart][framework.dart] 中的
    `performRebuild` 函式，忽略 `debugProfileBuildsEnabled`，
    並始終呼叫 `Timeline.startSync(...)/finish`。

  * If you use IntelliJ, a GUI view of this data is available.
    Select **Track widget rebuilds**,
    and your IDE displays which the widgets rebuild.

    如果你使用 IntelliJ，就可以看見這些資料的檢視。
    在 IntelliJ 的 Flutter Performance 工具中
    勾選 **Track widget rebuilds**，
    你就能在 IDE 中直觀地看見哪些 widget 進行了重建。

[`debugProfileBuildsEnabled`]: {{site.api}}/flutter/widgets/debugProfileBuildsEnabled.html
[debug.dart]: {{site.repo.flutter}}/blob/master/packages/flutter/lib/src/widgets/debug.dart
[framework.dart]: {{site.repo.flutter}}/blob/master/packages/flutter/lib/src/widgets/framework.dart

* How do I query the target frames per second (of the display)?

  我如何查詢顯示器的重新整理率？

  * [Get the display refresh rate][]

    [獲取顯示器重新整理率][Get the display refresh rate]

[Get the display refresh rate]: {{site.repo.flutter}}/wiki/Engine-specific-Service-Protocol-extensions#get-the-display-refresh-rate-_fluttergetdisplayrefreshrate

* How to solve my app's poor animations caused by an expensive
  Dart async function call that is blocking the UI thread?

  如何解決我的應用程式因高消耗的 Dart 非同步函式呼叫
  阻塞了 UI 執行緒，而導致動畫效果不佳？

  * Spawn another isolate using the [`compute()`][] method,
    as demonstrated in [Parse JSON in the background][] cookbook.

    使用 [`compute()`][] 方法產生另一個 isolate，
    例如 [在後台處理 JSON 資料解析][Parse JSON in the background] 實用課程 (Cookbook) 中所示範的。

[`compute()`]: {{site.api}}/flutter/foundation/compute-constant.html
[Parse JSON in the background]: {{site.url}}/cookbook/networking/background-parsing

* How do I determine my Flutter app's package size that a
  user will download?

  我如何確定使用者下載的 Flutter 應用程式套件的體積？

  * See [Measuring your app's size][]

    請查閱 [測量你的應用體積][Measuring your app's size]

[Measuring your app's size]: {{site.url}}/perf/app-size

* How do I see the breakdown of the Flutter engine size?

  我如何才能看到 Flutter engine 內架構細分的體積？

  * Visit the [binary size dashboard][], and replace the git
    hash in the URL with a recent commit hash from
    [GitHub engine repository commits][].

    存取 [binary size dashboard][]，
    用 [GitHub 上 engine 儲存庫][GitHub engine repository commits] 最近提交的 hash 值替換 URL 中的 hash 值。

[binary size dashboard]: https://storage.googleapis.com/flutter_infra_release/flutter/241c87ad800beeab545ab867354d4683d5bfb6ce/android-arm-release/sizes/index.html
[GitHub engine repository commits]: {{site.github}}/flutter/engine/commits

* How can I take a screenshot of an app that is running and export it
  as a SKP file?

  我如何才能對正在執行的應用程式進行截圖，
  並將其匯出 SKP 檔案？

  * Run `flutter screenshot --type=skia --observatory-uri=...`

    執行 `flutter screenshot --type=skia --observatory-uri=...` 命令

  * Note a known issue viewing screenshots:

    注意，一個檢視截圖的已知問題：

    * [Issue 21237][]: Doesn't record images in real devices.

      [Issue 21237][]: 無法在真機中截圖。

  * To analyze and visualize the SKP file,
    check out the [Skia WASM debugger][].

    分析和視覺化 SKP 檔案，請使用 [Skia WASM debugger][]。

[Issue 21237]: {{site.repo.flutter}}/issues/21237
[Skia WASM debugger]: https://debugger.skia.org/

* How do I retrieve the shader persistent cache from a device?

  如何從裝置上讀取著色器持久化快取？

  * On Android, you can do the following:

    在 Android 上，你可以進行以下操作：

    ```terminal
    adb shell
    run-as <com.your_app_package_name>
    cp <your_folder> <some_public_folder, e.g., /sdcard> -r
    adb pull <some_public_folder/your_folder>
    ```

* How do I perform a trace in Fuchsia?

  我如何在 Fuchsia 中進行追蹤？

  * See [Fuchsia tracing guidelines][traceutil]

    請查閱 [Fuchsia 追蹤指南][traceutil]
