---
title: Performance metrics
title: 效能指標
description: Flutter metrics, and which tools and APIs are used to get them
description: Flutter 效能指標與能獲取它們的 API 和工具
---

* Startup time to the first frame

  第一幀的啟動時間

  * Check the time when
    [WidgetsBinding.instance.firstFrameRasterized][firstFrameRasterized] 
    is true.
    
    當 [WidgetsBinding.instance.firstFrameRasterized][firstFrameRasterized] 為 true 時檢視耗時。
    
  * See the
    [perf dashboard](https://flutter-flutter-perf.skia.org/e/?queries=sub_result%3DtimeToFirstFrameRasterizedMicros).
    
    檢視 [效能資料看板](https://flutter-flutter-perf.skia.org/e/?queries=sub_result%3DtimeToFirstFrameRasterizedMicros)。
    
* Frame buildDuration, rasterDuration, and totalSpan

  一幀的建構時間，網格化時間，以及總時間
  
  * See [`FrameTiming`]({{site.api}}/flutter/dart-ui/FrameTiming-class.html)
    in the API docs.
    
    在 API 文件中檢視 [`FrameTiming`]({{site.api}}/flutter/dart-ui/FrameTiming-class.html.) 。
    
* Statistics of frame `buildDuration` (`*_frame_build_time_millis`)

  一幀的建構時間 `buildDuration` (`*_frame_build_time_millis`)

  * We recommend monitoring four stats: average, 90th percentile, 99th
    percentile, and worst frame build time.
    
    我們建議監測四個資料：平均值、90 分位值、99 分位值和最差幀建構時間。
    
  * See, for example, [metrics][transition_build] for the 
    `flutter_gallery__transition_perf` test.

    例如，檢視 `flutter_gallery__transition_perf` 測試案例中的 [建構資料][transition_build] 。
    
* Statistics of frame `rasterDuration` (`*_frame_build_time_millis`)

  一幀的網格化時間 `rasterDuration` (`*_frame_build_time_millis`)
  
  * We recommend monitoring four stats: average, 90th percentile, 99th
    percentile, and worst frame build time.
  
    我們建議監測四個資料：平均值、90 分位值、99 分位值和最差幀建構時間。    
    
  * See, for example, [metrics][transition_raster] for the 
    `flutter_gallery__transition_perf` test.

    例如，檢視 `flutter_gallery__transition_perf` 測試案例中的 [網格化資料][transition_build] 。

* CPU/GPU usage (a good approximation for energy use)

  CPU/GPU 的使用情況（一個可以近似衡量功耗的指標）

  * The usage is currently only available through trace events. See
    [profiling_summarizer.dart][profiling_summarizer].
    
    該資料目前僅能透過追蹤事件獲取。檢視 [profiling_summarizer.dart][profiling_summarizer] 。
    
  * See [metrics][cpu_gpu] for the `simple_animation_perf_ios` test.
  
    檢視 `simple_animation_perf_ios` 測試案例中的 [CPU/GPU 資料][cpu_gpu]。

* release_size_bytes to approximately measure the size of a Flutter app

  release_size_bytes 對 Flutter 應用程式的大小進行估算

  * See the [basic_material_app_android][], [basic_material_app_ios][],
    [hello_world_android][], [hello_world_ios][], [flutter_gallery_android][],
    and [flutter_gallery_ios][] tests.
    
    檢視 [basic_material_app_android][]、[basic_material_app_ios][]、[hello_world_android][]、[hello_world_ios][]、[flutter_gallery_android][] 和
    [flutter_gallery_ios][] 測試案例。
    
  * See [metrics][size_perf] in the dashboard.
  
    檢視資料看板中的 [體積大小][size_perf] 資料。
  
  * For info on how to measure the size more accurately,
    see the [app size]({{site.url}}/perf/app-size) page.

    有關如何更精確的測量應用體積資訊，檢視 [應用體積]({{site.url}}/perf/app-size) 頁面。

For a complete list of performance metrics Flutter measures per commit, visit 
the following sites, click **Query**, and filter the **test** and 
**sub_result** fields:

如果你想獲取完整的 Flutter 效能指標列表，存取以下的站點，點選 **Query** ，
然後選擇 **test** 和 **sub_result** ：

  * [https://flutter-flutter-perf.skia.org/e/](https://flutter-flutter-perf.skia.org/e/)
  * [https://flutter-engine-perf.skia.org/e/](https://flutter-engine-perf.skia.org/e/)

[firstFrameRasterized]: {{site.api}}/flutter/widgets/WidgetsBinding/firstFrameRasterized.html
[transition_build]: https://flutter-flutter-perf.skia.org/e/?queries=sub_result%3D90th_percentile_frame_build_time_millis%26sub_result%3D99th_percentile_frame_build_time_millis%26sub_result%3Daverage_frame_build_time_millis%26sub_result%3Dworst_frame_build_time_millis%26test%3Dflutter_gallery__transition_perf
[transition_raster]: https://flutter-flutter-perf.skia.org/e/?queries=sub_result%3D90th_percentile_frame_rasterizer_time_millis%26sub_result%3D99th_percentile_frame_rasterizer_time_millis%26sub_result%3Daverage_frame_rasterizer_time_millis%26sub_result%3Dworst_frame_rasterizer_time_millis%26test%3Dflutter_gallery__transition_perf
[profiling_summarizer]: {{site.repo.flutter}}/blob/master/packages/flutter_driver/lib/src/driver/profiling_summarizer.dart
[cpu_gpu]: https://flutter-flutter-perf.skia.org/e/?queries=sub_result%3Daverage_cpu_usage%26sub_result%3Daverage_gpu_usage%26test%3Dsimple_animation_perf_ios
[basic_material_app_android]: {{site.repo.flutter}}/blob/master/dev/devicelab/bin/tasks/basic_material_app_android__compile.dart
[basic_material_app_ios]: {{site.repo.flutter}}/blob/master/dev/devicelab/bin/tasks/basic_material_app_ios__compile.dart
[hello_world_android]: {{site.repo.flutter}}/blob/master/dev/devicelab/bin/tasks/hello_world_android__compile.dart
[hello_world_ios]: {{site.repo.flutter}}/blob/master/dev/devicelab/bin/tasks/hello_world_ios__compile.dart
[flutter_gallery_android]: {{site.repo.flutter}}/blob/master/dev/devicelab/bin/tasks/flutter_gallery_android__compile.dart
[flutter_gallery_ios]: {{site.repo.flutter}}/blob/master/dev/devicelab/bin/tasks/flutter_gallery_ios__compile.dart
[size_perf]: https://flutter-flutter-perf.skia.org/e/?queries=sub_result%3Drelease_size_bytes%26test%3Dbasic_material_app_android__compile%26test%3Dbasic_material_app_ios__compile%26test%3Dhello_world_android__compile%26test%3Dhello_world_ios__compile%26test%3Dflutter_gallery_ios__compile%26test%3Dflutter_gallery_android__compile