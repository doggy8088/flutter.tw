---
title: Shader compilation jank
title: 著色器編譯時卡頓
short-title: Shader jank
short-title: 著色器卡頓
description: What is shader jank and how to minimize it.
description: 你會了解到什麼是著色器卡頓以及如何減少它。
tags: Flutter效能
keywords: 卡頓
---

{% include docs/performance.md %}

If the animations on your mobile app appear to be janky,
but only on the first run,
this is likely due to shader compilation.
Flutter's long term solution to
shader compilation jank is [Impeller][],
which is in the stable release for iOS
and in preview behind a flag on Android.

如果你的移動應用程式上的動畫只在首次執行時卡頓，
很可能是著色器編譯引起的。
Flutter 對著色器編譯卡頓的長期解決方案是 [Impeller][]，
在 iOS 上可以直接使用，在 Android 上透過啟用特定標誌來使用。

[Impeller]: {{site.repo.flutter}}/wiki/Impeller

While we work on making Impeller fully production ready,
you can mitigate shader compilation jank by bundling
precompiled shaders with an iOS app.
Unfortunately, this approach doesn't work well on Android
due to precompiled shaders being device or GPU-specific.
The Android hardware ecosystem is large enough that the
GPU-specific precompiled shaders bundled with an application
will work on only a small subset of devices,
and will likely make jank worse on the other devices,
or even create rendering errors.

在我們努力讓 Impeller 為生產做好準備的時候，
你可以嘗試將預編譯的著色器與 iOS 應用繫結在一起來減輕著色器編譯的卡頓。
不幸的是，由於預編譯的著色器是針對裝置或特定 GPU 進行最佳化的，
所以這種方法在 Android 上效果不佳。
Android 的硬體生態系統非常龐大，
因此與應用程式繫結的特定 GPU 預編譯著色器只能在一小部分裝置上執行，
而且很可能會加劇其他裝置上的卡頓問題，甚至引發渲染錯誤。

Also, note that we aren't planning to make
improvements to the developer experience for creating
precompiled shaders described below. Instead,
we are focusing our energies on the more robust
solution to this problem that Impeller offers.

另外，請注意，我們並不打算改進下面描述的建立預編譯著色器。
相反，為了真正解決這個問題，
我們將主要精力集中在 Impeller 提供的更強大的解決方案上。

## What is shader compilation jank?

## 什麼是著色器編譯卡頓？

A shader is a piece of code that runs on a
GPU (graphics processing unit).
When the Skia graphics backend that Flutter uses for rendering
sees a new sequence of draw commands for the first time,
it sometimes generates and compiles a
custom GPU shader for that sequence of commands.
This allows that sequence and potentially similar sequences
to render as fast as possible.

著色器是在 GPU（圖形處理單元）上執行的程式碼。
當 Flutter 渲染的 Skia 圖形後端首次看到新的繪製命令序列時，
它有時會產生和編譯一個自訂的 GPU 著色器用於該命令序列。
使得該序列和潛在類似的序列能夠儘可能快地渲染。

Unfortunately, Skia's shader generation and compilation
happens in sequence with the frame workload.
The compilation could cost up to a few hundred milliseconds
whereas a smooth frame needs to be drawn within 16 milliseconds
for a 60 fps (frame-per-second) display.
Therefore, a compilation could cause tens of frames
to be missed, and drop the fps from 60 to 6.
This is _compilation jank_.
After the compilation is complete,
the animation should be smooth.

然而不幸的是，Skia 著色器產生和編譯的過程與幀的工作是依次進行的。
編譯過程可能需要幾百毫秒的時間，而對於 60 幀/秒 (frame-per-second) 的顯示來說，
一個流暢的幀必須在 16 毫秒內繪製完成。因此，編譯過程可能導致數十幀被丟失，
使幀數從 60 降到 6。這就是所謂的 **編譯卡頓** 。編譯完成之後，動畫應該會變得流暢。

On the other hand, Impeller generates and compiles all
necessary shaders when we build the Flutter Engine.
Therefore apps running on Impeller already have
all the shaders they need, and the shaders can be used
without introducing jank into animations.

另一方面，Impeller 在我們建構 Flutter 引擎時已經產生並編譯了所有必要的著色器。
因此，在 Impeller 上執行的應用程式已經擁有了它們所需的所有著色器，
並且這些著色器不會在動畫中引起卡頓。

Definitive evidence for the presence of shader compilation jank
is to set `GrGLProgramBuilder::finalize` in the tracing
with `--trace-skia` enabled.
The following screenshot shows an example timeline tracing.

要獲得更加確切的著色器編譯卡頓存在的證據，
你可以在 `--trace-skia` 開啟時檢視追蹤檔案中的
`GrGLProgramBuilder::finalize`。
下面的截圖展示了一個 timeline 追蹤的範例。

![A tracing screenshot verifying jank]({{site.url}}/assets/images/docs/perf/render/tracing.png){:width="100%"}

## What do we mean by "first run"?

## 如何定義「首次執行」？

On iOS, "first run" means that the user might see
jank when an animation first occurs every time
the user opens the app from scratch.

在 iOS 上來說，
「首次執行」意味著使用者可能在每次開啟應用後，
在動畫首次載入時都會出現卡頓。

## How to use SkSL warmup

## 如何使用 SkSL 預熱

Flutter provides command line tools
for app developers to collect shaders that might be needed
for end-users in the SkSL (Skia Shader Language) format.
The SkSL shaders can then be packaged into the app,
and get warmed up (pre-compiled) when an end-user first
opens the app, thereby reducing the compilation
jank in later animations.
Use the following instructions to collect
and package the SkSL shaders:

Flutter 為應用開發者提供了一個命令列工具以收集終端使用者在
SkSL（Skia 著色器語言）進行格式化處理中需要用到的著色器。
SkSL 著色器可以被打包進應用，並提前進行預熱（預編譯），
這樣當終端使用者第一次開啟應用時，就能夠減少動畫的編譯掉幀了。
使用下面的指令收集並打包 SkSL 的著色器：

<ol markdown="1">
<li markdown="1">Run the app with `--cache-sksl` turned on
    to capture shaders in SkSL:

​    開啟 `--cache-sksl` 執行你的應用以捕獲 SkSL 中的著色器：

```terminal
flutter run --profile --cache-sksl
```

If the same app has been previously run
without `--cache-sksl`, then the
`--purge-persistent-cache` flag might be needed:

如果這個相同的應用之前執行的時候沒有使用 `--cache-sksl`，
你需要加上 `--purge-persistent-cache` 標誌：

```terminal
flutter run --profile --cache-sksl --purge-persistent-cache
```

This flag removes older non-SkSL shader caches that
could interfere with SkSL shader capturing.
It also purges the SkSL shaders so use it *only* on the first
`--cache-sksl` run.

這個標誌將會刪除可能干擾 SkSL 的較舊的非 SkSL 著色器快取捕獲的著色器。 
它還清除了 SkSL 著色器，因此*僅*在第一次使用 `--cache-sksl` 執行。

</li>

<li markdown="1"> Play with the app to trigger as many animations
    as needed; particularly those with compilation jank.

   儘可能多觸發應用的動畫，特別是那些會引起編譯卡頓的。
</li>

<li markdown="1"> Press `M` at the command line of `flutter run` to
    write the captured SkSL shaders into a file named something like
   `flutter_01.sksl.json`.
   For best results,
   capture SkSL shaders on an actual iOS device.
   A shader captured on a simulator isn't likely to work correctly
   on actual hardware.

   在執行 `flutter run` 命令後行按下 `M` 鍵以捕獲 SkSL 著色器到一個類別似
   `flutter_01.sksl.json` 的檔案中。為了達到最好的效果，最好是能夠在
   iOS 真機上抓取 SkSL 著色器，在模擬器上的抓取通常會是無效的。
</li>

<li markdown="1"> Build the app with SkSL warm-up using the following,
    as appropriate:

    在下面的命令中選擇合適的建構帶有 SkSL 預熱的應用：

```terminal
flutter build ios --bundle-sksl-path flutter_01.sksl.json
```

If it's built for a driver test like `test_driver/app.dart`,
make sure to also specify `--target=test_driver/app.dart`
(for example, `flutter build ios --bundle-sksl-path
flutter_01.sksl.json --target=test_driver/app.dart`).

如果它會建構一個類別似 `test_driver/app.dart` 的驅動測試，
請確保指定 `--target=test_driver/app.dart`。
（例如 `flutter build ios --bundle-sksl-path flutter_01.sksl.json --target=test_driver/app.dart`）

</li>

<li markdown="1"> Test the newly built app.
</li>
</ol>

Alternatively, you can write some integration tests to
automate the first three steps using a single command.
For example:

或者，你可以編寫一些整合測試來
使用一個命令自動執行前三個步驟。
例如：

```terminal
flutter drive --profile --cache-sksl --write-sksl-on-exit flutter_01.sksl.json -t test_driver/app.dart
```

With such [integration tests][],
you can easily and reliably get the
new SkSLs when the app code changes,
or when Flutter upgrades.
Such tests can also be used to verify the performance change
before and after the SkSL warm-up.
Even better, you can put those tests into a
CI (continuous integration) system so the
SkSLs are generated and tested automatically over the lifetime of an app.

使用這樣的 [整合測試][integration tests]，無論是程式碼發生改變或者 Flutter 更新了，
你都可以輕鬆獲得可靠的著色器快取。
這些測試也被用於驗證開啟著色器預熱前後的效能變化上。
更好的做法是，你可以把這些測試放進 CI（持續整合）系統上，
這樣就能在每次應用釋出前自動產生並測試著色器快取了。

[integration tests]: {{site.url}}/cookbook/testing/integration/introduction

{{site.alert.note}}

  The integration_test package is now the recommended way
  to write integration tests. Refer to the
  [Integration testing]({{site.url}}/testing/integration-tests/)
  page for details.

  整合測試（integration_test）package，現在已經成為編寫整合測試首推的 package。
  請在 [整合測試]({{site.url}}/testing/integration-tests/) 頁面上檢視詳情。

{{site.alert.end}}

Take the original version of [Flutter Gallery][] as an example.
The CI system is set up to generate SkSLs for every Flutter commit,
and verifies the performance, in the [`transitions_perf_test.dart`][] test.
For more details,
check out the [`flutter_gallery_sksl_warmup__transition_perf`][] and
[`flutter_gallery_sksl_warmup__transition_perf_e2e_ios32`][] tasks.

就拿原始版本的 [Flutter Gallery][] 舉例。
我們讓 CI 系統在每次 Flutter commit 後都產生著色器快取，
並在 [`transitions_perf_test.dart`][] 中驗證效能。
更多詳細資訊請檢視 [Flutter Gallery sksl 預熱過渡效能驗證][`flutter_gallery_sksl_warmup__transition_perf`]，
以及 [Flutter Gallery sksl 預熱過渡在 iOS_32 上的效能驗證][`flutter_gallery_sksl_warmup__transition_perf_e2e_ios32`]。

[Flutter Gallery]: {{site.repo.flutter}}/tree/main/dev/integration_tests/flutter_gallery
[`flutter_gallery_sksl_warmup__transition_perf`]: {{site.repo.flutter}}/blob/master/dev/devicelab/bin/tasks/flutter_gallery_sksl_warmup__transition_perf.dart
[`flutter_gallery_sksl_warmup__transition_perf_e2e_ios32`]: {{site.repo.flutter}}/blob/master/dev/devicelab/bin/tasks/flutter_gallery_sksl_warmup__transition_perf_e2e_ios32.dart
[`transitions_perf_test.dart`]: {{site.repo.flutter}}/blob/master/dev/integration_tests/flutter_gallery/test_driver/transitions_perf_test.dart

The worst frame rasterization time is a useful metric from
such integration tests to indicate the severity of shader
compilation jank.
For instance,
the steps above reduce Flutter gallery's shader compilation
jank and speeds up its worst frame rasterization time on a
Moto G4 from ~90 ms to ~40 ms. On iPhone 4s,
it's reduced from ~300 ms to ~80 ms. That leads to the visual
difference as illustrated in the beginning of this article.

在這種這種整合測試中，
最差的幀光柵化時間是一個很好的指標來衡量
著色器編譯卡頓的嚴重性。 
例如，上述步驟減少了 Flutter gallery 應用的著色器編譯卡頓，
並減少了它在 Moto G4 手機上的最差的幀光柵化時間，從 ~90 ms 減少到 ~40 ms。
在 iPhone 4s 上，它從 ~300 ms 減少到 ~80 ms。 
這種視覺差異如同本文開頭所示一樣。

