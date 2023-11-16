---
title: Add a Flutter Fragment to an Android app
title: 向 Android 應用中新增 Flutter Fragment
short-title: Add a Flutter Fragment
short-title: 新增 Flutter Fragment
description: Learn how to add a Flutter Fragment to your existing Android app.
description: 瞭解如何向你現有的 Android 應用新增一個 Flutter Fragment。
tags: Flutter混合工程,add2app
keywords: Android,Flutter Fragment
---

<img src='/assets/images/docs/development/add-to-app/android/add-flutter-fragment/add-flutter-fragment_header.png'
class="mw-100" alt="Add Flutter Fragment Header">

This guide describes how to add a Flutter `Fragment` to an existing
Android app.  In Android, a [`Fragment`][] represents a modular
piece of a larger UI. A `Fragment` might be used to present
a sliding drawer, tabbed content, a page in a `ViewPager`,
or it might simply represent a normal screen in a
single-`Activity` app. Flutter provides a [`FlutterFragment`][]
so that developers can present a Flutter experience any place
that they can use a regular `Fragment`.

本篇指南介紹如何向一個現有的 Android 應用中新增 Flutter `Fragment`。
在 Android 開發中，一個 [`Fragment`][] 代表了一塊較大的模組化 UI。 
`Fragment` 可能被用來展示滑動抽屜、標籤內容和 `ViewPager` 中的頁面，
或者在單 `Activity` 應用中，`Fragment` 可能僅代表正常的螢幕內容。
Flutter 提供了[`FlutterFragment`][]，
以便於開發者們可以在任何使用常規 `Fragment` 的地方呈現 Flutter 的內容。

If an `Activity` is equally applicable for your application needs,
consider [using a `FlutterActivity`][] instead of a
`FlutterFragment`, which is quicker and easier to use.

如果 `Activity` 同樣適用於您的應用需求，
可以考慮 [使用 `FlutterActivity`][using a `FlutterActivity`] 而非 `FlutterFragment`，前者更加快捷易用。

[using a `FlutterActivity`]: {{site.url}}/development/add-to-app/android/add-flutter-screen

`FlutterFragment` allows developers to control the following
details of the Flutter experience within the `Fragment`:

`FlutterFragment` 允許開發者在 `Fragment` 中控制以下 Flutter 的開發細節：

 * Initial Flutter route

   Flutter 初始路由

 * Dart entrypoint to execute

   將要執行的 Dart 入口

 * Opaque vs translucent background

   非透明或者透明的背景

 * Whether `FlutterFragment` should control its surrounding `Activity`

   `FlutterFragment` 是否能控制它外層的 `Activity`

 * Whether a new [`FlutterEngine`][] or a cached `FlutterEngine` should be used

   使用新的還是快取的 [`FlutterEngine`][]

`FlutterFragment` also comes with a number of calls that
must be forwarded from its surrounding `Activity`.
These calls allow Flutter to react appropriately to OS events.

`FlutterFragment` 還提供了一些回呼(Callback)事件，這些回呼(Callback)必須由它所在的 `Activity` 觸發執行。
這些回呼(Callback)允許 Flutter 適時地響應一些系統事件。

All varieties of `FlutterFragment`, and its requirements,
are described in this guide.

這篇指南介紹了 `FlutterFragment` 的所有使用方式和使用要求。 

## Add a `FlutterFragment` to an `Activity` with a new `FlutterEngine`

## 使用新的 `FlutterEngine` 向 `Activity` 中新增 `FlutterFragment`

The first thing to do to use a `FlutterFragment` is to add it to a host
`Activity`.

使用 `FlutterFragment` 的第一步是將其新增進宿主 `Activity`。

To add a `FlutterFragment` to a host `Activity`, instantiate and
attach an instance of `FlutterFragment` in `onCreate()` within the
`Activity`, or at another time that works for your app:

要向宿主 `Activity` 中新增 `FlutterFragment`，
需要在 `Activity` 的 `onCreate()` 或者其它合適的地方，
例項化 `FlutterFragment` 並且與 `Activity` 繫結。

{% samplecode add-fragment %}
{% sample Java %}
<?code-excerpt title="MyActivity.java"?>
```java
public class MyActivity extends FragmentActivity {
    // Define a tag String to represent the FlutterFragment within this
    // Activity's FragmentManager. This value can be whatever you'd like.
    private static final String TAG_FLUTTER_FRAGMENT = "flutter_fragment";

    // Declare a local variable to reference the FlutterFragment so that you
    // can forward calls to it later.
    private FlutterFragment flutterFragment;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Inflate a layout that has a container for your FlutterFragment.
        // For this example, assume that a FrameLayout exists with an ID of
        // R.id.fragment_container.
        setContentView(R.layout.my_activity_layout);

        // Get a reference to the Activity's FragmentManager to add a new
        // FlutterFragment, or find an existing one.
        FragmentManager fragmentManager = getSupportFragmentManager();

        // Attempt to find an existing FlutterFragment,
        // in case this is not the first time that onCreate() was run.
        flutterFragment = (FlutterFragment) fragmentManager
            .findFragmentByTag(TAG_FLUTTER_FRAGMENT);

        // Create and attach a FlutterFragment if one does not exist.
        if (flutterFragment == null) {
            flutterFragment = FlutterFragment.createDefault();

            fragmentManager
                .beginTransaction()
                .add(
                    R.id.fragment_container,
                    flutterFragment,
                    TAG_FLUTTER_FRAGMENT
                )
                .commit();
        }
    }
}
```
{% sample Kotlin %}
<?code-excerpt title="MyActivity.kt"?>
```kotlin
class MyActivity : FragmentActivity() {
  companion object {
    // Define a tag String to represent the FlutterFragment within this
    // Activity's FragmentManager. This value can be whatever you'd like.
    private const val TAG_FLUTTER_FRAGMENT = "flutter_fragment"
  }

  // Declare a local variable to reference the FlutterFragment so that you
  // can forward calls to it later.
  private var flutterFragment: FlutterFragment? = null

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Inflate a layout that has a container for your FlutterFragment. For
    // this example, assume that a FrameLayout exists with an ID of
    // R.id.fragment_container.
    setContentView(R.layout.my_activity_layout)

    // Get a reference to the Activity's FragmentManager to add a new
    // FlutterFragment, or find an existing one.
    val fragmentManager: FragmentManager = supportFragmentManager

    // Attempt to find an existing FlutterFragment, in case this is not the
    // first time that onCreate() was run.
    flutterFragment = fragmentManager
      .findFragmentByTag(TAG_FLUTTER_FRAGMENT) as FlutterFragment?

    // Create and attach a FlutterFragment if one does not exist.
    if (flutterFragment == null) {
      var newFlutterFragment = FlutterFragment.createDefault()
      flutterFragment = newFlutterFragment
      fragmentManager
        .beginTransaction()
        .add(
          R.id.fragment_container,
          newFlutterFragment,
          TAG_FLUTTER_FRAGMENT
        )
        .commit()
    }
  }
}
```
{% endsamplecode %}

The previous code is sufficient to render a Flutter UI
that begins with a call to your `main()` Dart entrypoint,
an initial Flutter route of `/`, and a new `FlutterEngine`.
However, this code is not sufficient to achieve all expected
Flutter behavior. Flutter depends on various OS signals that
must  be forwarded from your host `Activity` to `FlutterFragment`.
These calls are shown in the following example:

上面的程式碼會以 `main()` 為 Dart 入口函式， `/` 為初始路由，
並使用新的 `FlutterEngine`，能夠正確渲染出 Flutter UI。
但是，這些程式碼還無法使 Flutter 如預期一樣完全正常地工作。
Flutter 依賴作業系統的各種訊號，
這些訊號必須透過宿主 `Activity` 傳送到 `FlutterFragment` 中。
下面的範例展示了這些系統回呼(Callback)：

{% samplecode forward-activity-calls %}
{% sample Java %}
<?code-excerpt title="MyActivity.java"?>
```java
public class MyActivity extends FragmentActivity {
    @Override
    public void onPostResume() {
        super.onPostResume();
        flutterFragment.onPostResume();
    }

    @Override
    protected void onNewIntent(@NonNull Intent intent) {
        flutterFragment.onNewIntent(intent);
    }

    @Override
    public void onBackPressed() {
        flutterFragment.onBackPressed();
    }

    @Override
    public void onRequestPermissionsResult(
        int requestCode,
        @NonNull String[] permissions,
        @NonNull int[] grantResults
    ) {
        flutterFragment.onRequestPermissionsResult(
            requestCode,
            permissions,
            grantResults
        );
    }

    @Override
    public void onActivityResult(
        int requestCode,
        int resultCode,
        @Nullable Intent data
    ) {
        super.onActivityResult(requestCode, resultCode, data);
        flutterFragment.onActivityResult(
            requestCode,
            resultCode,
            data
        );
    }

    @Override
    public void onUserLeaveHint() {
        flutterFragment.onUserLeaveHint();
    }

    @Override
    public void onTrimMemory(int level) {
        super.onTrimMemory(level);
        flutterFragment.onTrimMemory(level);
    }
}
```
{% sample Kotlin %}
<?code-excerpt title="MyActivity.kt"?>
```kotlin
class MyActivity : FragmentActivity() {
  override fun onPostResume() {
    super.onPostResume()
    flutterFragment!!.onPostResume()
  }

  override fun onNewIntent(@NonNull intent: Intent) {
    flutterFragment!!.onNewIntent(intent)
  }

  override fun onBackPressed() {
    flutterFragment!!.onBackPressed()
  }

  override fun onRequestPermissionsResult(
    requestCode: Int,
    permissions: Array<String?>,
    grantResults: IntArray
  ) {
    flutterFragment!!.onRequestPermissionsResult(
      requestCode,
      permissions,
      grantResults
    )
  }

  override fun onActivityResult(
    requestCode: Int,
    resultCode: Int,
    data: Intent?
  ) {
    super.onActivityResult(requestCode, resultCode, data)
    flutterFragment!!.onActivityResult(
      requestCode,
      resultCode,
      data
    )
  }

  override fun onUserLeaveHint() {
    flutterFragment!!.onUserLeaveHint()
  }

  override fun onTrimMemory(level: Int) {
    super.onTrimMemory(level)
    flutterFragment!!.onTrimMemory(level)
  }
}
```
{% endsamplecode %}

With the OS signals forwarded to Flutter,
your `FlutterFragment` works as expected.
You have now added a `FlutterFragment` to your existing Android app.

隨著 OS 訊號傳遞到 Flutter，
您的 `FlutterFragment` 可以如預期正常工作。
現在可以嘗試將 `FlutterFragment` 新增進您的 Android 應用了。

The simplest integration path uses a new `FlutterEngine`,
which comes with a non-trivial initialization time,
leading to a blank UI until Flutter is
initialized and rendered the first time.
Most of this time overhead can be avoided by using
a cached, pre-warmed `FlutterEngine`, which is discussed next.

使用新的 `FlutterEngine` 是最簡單的整合方式，
但是會存在一段明顯的初始化時間，
此時，在 Flutter 初始化和首次渲染完成之前會出現短暫的白屏。
使用快取、預熱的 `FlutterEngine` 則可以避免上述的大部分耗時，
下面我們將討論這些內容。

## Using a pre-warmed `FlutterEngine`

## 使用預熱的 `FlutterEngine`

By default, a `FlutterFragment` creates its own instance
of a `FlutterEngine`, which requires non-trivial warm-up time.
This means your user sees a blank `Fragment` for a brief moment.
You can mitigate most of this warm-up time by
using an existing, pre-warmed instance of `FlutterEngine`.

預設情況下，`FlutterFragment` 會建立它自己的 `FlutterEngine` 例項，
同時也需要不少的啟動時間。
這就意味著您的使用者會看到短暫的白屏。
透過使用已存在的、預熱的 `FlutterEngine` 就可以大幅度減少啟動的耗時。

To use a pre-warmed `FlutterEngine` in a `FlutterFragment`,
instantiate a `FlutterFragment` with the `withCachedEngine()`
factory method.  

要在 `FlutterFragment` 中使用預熱 `FlutterEngine`，
可以使用工廠方法 `withCachedEngine()` 例項化 `FlutterFragment`。

{% samplecode use-prewarmed-engine %}
{% sample Java %}
<?code-excerpt title="MyApplication.java"?>
```java
// Somewhere in your app, before your FlutterFragment is needed,
// like in the Application class ...
// Instantiate a FlutterEngine.
FlutterEngine flutterEngine = new FlutterEngine(context);

// Start executing Dart code in the FlutterEngine.
flutterEngine.getDartExecutor().executeDartEntrypoint(
    DartEntrypoint.createDefault()
);

// Cache the pre-warmed FlutterEngine to be used later by FlutterFragment.
FlutterEngineCache
  .getInstance()
  .put("my_engine_id", flutterEngine);
```

<?code-excerpt title="MyActivity.java"?>
```java
FlutterFragment.withCachedEngine("my_engine_id").build();
```
{% sample Kotlin %}
<?code-excerpt title="MyApplication.kt"?>
```kotlin
// Somewhere in your app, before your FlutterFragment is needed,
// like in the Application class ...
// Instantiate a FlutterEngine.
val flutterEngine = FlutterEngine(context)

// Start executing Dart code in the FlutterEngine.
flutterEngine.getDartExecutor().executeDartEntrypoint(
    DartEntrypoint.createDefault()
)

// Cache the pre-warmed FlutterEngine to be used later by FlutterFragment.
FlutterEngineCache
  .getInstance()
  .put("my_engine_id", flutterEngine)
```

<?code-excerpt title="MyActivity.java"?>
```kotlin
FlutterFragment.withCachedEngine("my_engine_id").build()
```
{% endsamplecode %}

`FlutterFragment` internally knows about [`FlutterEngineCache`][]
and retrieves the pre-warmed `FlutterEngine` based on the ID
given to `withCachedEngine()`.

`FlutterFragment` 內部可存取 [`FlutterEngineCache`][]，
並且可以根據傳遞給 `withCachedEngine()` 的 ID 獲取預熱的 `FlutterEngine`。

By providing a pre-warmed `FlutterEngine`,
as previously shown, your app renders the
first Flutter frame as quickly as possible.

如上所示，透過提供預熱的 `FlutterEngine`，您的應用將以最快速度渲染出第一幀。

#### Initial route with a cached engine

#### 快取引擎中的初始路由

{% include_relative _initial-route-cached-engine.md %}

## Display a splash screen

## 展示閃屏頁

The initial display of Flutter content requires some wait time,
even if a pre-warmed `FlutterEngine` is used.
To help improve the user experience around
this brief waiting period, Flutter supports the
display of a splash screen (also known as "launch screen") until Flutter
renders its first frame. For instructions about how to show a launch
screen, see the [splash screen guide][].

即使使用了預熱的 `FlutterEngine`，第一次展示 Flutter 的內容仍然需要一些時間。
為了更進一步提升使用者體驗，Flutter 支援在第一幀渲染完成之前展示閃屏頁。
關於如何展示閃屏頁的詳細說明，請參閱這篇 [閃屏頁指南][splash screen guide]。

[splash screen guide]: {{site.url}}/development/ui/advanced/splash-screen

## Run Flutter with a specified initial route

## 指定 Flutter 執行的初始路由

An Android app might contain many independent Flutter experiences,
running in different `FlutterFragment`s, with different
`FlutterEngine`s. In these scenarios,
it's common for each Flutter experience to begin with different
initial routes (routes other than `/`).
To facilitate this, `FlutterFragment`'s `Builder`
allows you to specify a desired initial route, as shown:

一個 Android 應用中可能包含很多獨立的 Flutter 介面，
這些介面顯示在不同的 `FlutterFragment` 上，
每個 `FlutterFragment` 的 `FlutterEngine` 也是獨立的。
在這些情況下，每個 Flutter 介面透過不同的初始路由（除 `/` 以外的路由 ）啟動是很正常的。
為此，`FlutterFragment` 的 `Builder` 允許指定一個您希望的初始路由，如下所示：

{% samplecode launch-with-initial-route %}
{% sample Java %}
<?code-excerpt title="MyActivity.java"?>
```java
// With a new FlutterEngine.
FlutterFragment flutterFragment = FlutterFragment.withNewEngine()
    .initialRoute("myInitialRoute/")
    .build();
```
{% sample Kotlin %}
<?code-excerpt title="MyActivity.kt"?>
```kotlin
// With a new FlutterEngine.
val flutterFragment = FlutterFragment.withNewEngine()
    .initialRoute("myInitialRoute/")
    .build()
```
{% endsamplecode %}

{{site.alert.note}}

  `FlutterFragment`'s initial route property has no effect when a pre-warmed
  `FlutterEngine` is used because the pre-warmed `FlutterEngine` already
  chose an initial route. The initial route can be chosen explicitly when
  pre-warming a `FlutterEngine`.

  當使用已預熱的 `FlutterEngine` 建構 `FlutterFragment` 時，指定的初始路由屬性是無效的。
  因為已預熱的 `FlutterEngine` 已經設定好了一個初始路由。
  `FlutterEngine` 僅在預熱之時可以明確地選擇初始路由。

{{site.alert.end}}

## Run Flutter from a specified entrypoint

## 指定 Flutter 執行的入口

Similar to varying initial routes, different
`FlutterFragment`s might want to execute different
Dart entrypoints. In a typical Flutter app, there is only one
Dart entrypoint: `main()`, but you can define other entrypoints.

和變化的初始路由類似，不同的 `FlutterFragment` 可能需要執行不同的 Dart 程式碼入口。
正常的 Flutter 應用中，只會有一個 `main()` 入口，但是您也可以定義不同的入口。

`FlutterFragment` supports specification of the desired
Dart entrypoint to execute for the given Flutter experience.
To specify an entrypoint, build `FlutterFragment`, as shown:

`FlutterFragment` 支援指定需要的 Dart 入口以執行對應的 Flutter 介面。
下面的程式碼展示瞭如何在建構 `FlutterFragment` 時指定一個入口。

{% samplecode launch-with-custom-entrypoint %}
{% sample Java %}
<?code-excerpt title="MyActivity.java"?>
```java
FlutterFragment flutterFragment = FlutterFragment.withNewEngine()
    .dartEntrypoint("mySpecialEntrypoint")
    .build();
```
{% sample Kotlin %}
<?code-excerpt title="MyActivity.kt"?>
```kotlin
val flutterFragment = FlutterFragment.withNewEngine()
    .dartEntrypoint("mySpecialEntrypoint")
    .build()
```
{% endsamplecode %}

The `FlutterFragment` configuration results in the execution
of a Dart entrypoint called `mySpecialEntrypoint()`.
Notice that the parentheses `()` are
not included in the `dartEntrypoint` `String` name.

這裡，`FlutterFragment` 的配置會將 Dart 入口的執行函式設定為 `mySpecialEntrypoint()`。
需要注意的是，括號 `()` 不包含在 `dartEntrypoint` 的 `String` 型別的引數中。

{{site.alert.note}}

  `FlutterFragment`'s Dart entrypoint property has no effect
  when a pre-warmed `FlutterEngine` is used because the
  pre-warmed `FlutterEngine` already executed a Dart entrypoint.
  The Dart entrypoint can be chosen explicitly when pre-warming
  a `FlutterEngine`.

  當使用已預熱的 `FlutterEngine` 建構 `FlutterFragment` 時，指定的 Dart 入口是無效的。
  因為已預熱的 `FlutterEngine` 已經執行了一個入口函式。
  只有 `FlutterEngine` 在預熱之時是可以明確選擇入口的。

{{site.alert.end}}

## Control `FlutterFragment`'s render mode

## 控制 `FlutterFragment` 的渲染模式

`FlutterFragment` can either use a `SurfaceView` to render its
Flutter content, or it can use a `TextureView`.
The default is `SurfaceView`, which is significantly
better for performance than `TextureView`. However, `SurfaceView`
can't be interleaved in the middle of an Android `View` hierarchy.
A `SurfaceView` must either be the bottommost `View` in the hierarchy,
or the topmost `View` in the hierarchy.
Additionally, on Android versions before Android N,
`SurfaceView`s can't be animated because their layout and rendering
aren't synchronized with the rest of the `View` hierarchy.
If either of these use cases are requirements for your app,
then you need to use `TextureView` instead of `SurfaceView`.
Select a `TextureView` by building a `FlutterFragment` with a
`texture` `RenderMode`:

`FlutterFragment` 可以選擇使用 `SurfaceView` 或者 `TextureView` 來渲染其內容。
預設配置的 `SurfaceView` 在效能上明顯好於 `TextureView`。
然而，`SurfaceView` 無法插入到 Android 的 `View` 層級之中。
`SurfaceView` 在檢視層級中必須是最底層的 `View` 或者最最上層的 `View`。
此外，在 Android N 之前，`SurfaceView` 無法用於製作動畫，
因為它們的佈局和渲染無法和檢視層級中的其它 `View` 同步。
如果上述這些使用案例之一在您的應用需求之中，您需要使用 `TextureView` 替換 `SurfaceView`。
要選擇 `TextureView`，可以在建構 `FlutterFragment` 時指定 `RenderMode` 為 `texture`：

{% samplecode launch-with-rendermode %}
{% sample Java %}
<?code-excerpt title="MyActivity.java"?>
```java
// With a new FlutterEngine.
FlutterFragment flutterFragment = FlutterFragment.withNewEngine()
    .renderMode(FlutterView.RenderMode.texture)
    .build();

// With a cached FlutterEngine.
FlutterFragment flutterFragment = FlutterFragment.withCachedEngine("my_engine_id")
    .renderMode(FlutterView.RenderMode.texture)
    .build();
```
{% sample Kotlin %}
<?code-excerpt title="MyActivity.kt"?>
```kotlin
// With a new FlutterEngine.
val flutterFragment = FlutterFragment.withNewEngine()
    .renderMode(FlutterView.RenderMode.texture)
    .build()

// With a cached FlutterEngine.
val flutterFragment = FlutterFragment.withCachedEngine("my_engine_id")
    .renderMode(FlutterView.RenderMode.texture)
    .build()
```
{% endsamplecode %}

Using the configuration shown, the resulting `FlutterFragment`
renders its UI to a `TextureView`.

使用上面展示的程式碼配置，`FlutterFragment` 可以將它的 UI 渲染為 `TextureView`。

## Display a `FlutterFragment` with transparency

## 展示透明的 `FlutterFragment`

By default, `FlutterFragment` renders with an opaque background,
using a `SurfaceView`. (See "Control `FlutterFragment`'s render
mode.") That background is black for any pixels that aren't
 painted by Flutter. Rendering with an opaque background is
the preferred rendering mode for performance reasons.
Flutter rendering with transparency on Android negatively
affects performance. However, there are many designs that
require transparent pixels in the Flutter experience that
show through to the underlying Android UI. For this reason,
Flutter supports translucency in a `FlutterFragment`.

預設情況下，`FlutterFragment` 使用 `SurfaceView` 渲染且背景不透明。
（參考「控制 `FlutterFragment` 的渲染模式」）任何未經 Flutter 繪製的畫素在背景中都是黑色的。
出於效能方面的考慮，我們優先選擇使用不透明的背景進行渲染。
渲染透明的 Flutter 介面在 Android 平臺上會產生效能方面的負面影響。
但是許多設計都需要 Flutter 介面中包含透明的畫素以顯示底層的 Android UI。
因此，Flutter 支援 `FlutterFragment` 半透明。

{{site.alert.note}}

  Both `SurfaceView` and `TextureView` support transparency.
  However, when a `SurfaceView` is instructed to render with
  transparency, it positions itself at a higher z-index than
  all other Android `View`s, which means it appears
  above all other `View`s. This is a limitation of `SurfaceView`.
  If it's acceptable to render your Flutter experience on top
  of all other content, then `FlutterFragment`'s default
  `RenderMode` of `surface` is the `RenderMode` that you
  should use. However, if you need to display Android `View`s both
  above and below your Flutter experience, then you must specify a
  `RenderMode` of `texture`.
  See "Control `FlutterFragment`'s render mode"
  for information about controlling the `RenderMode`.

  `SurfaceView` 和 `TextureView` 都支援透明。
  但是當 `SurfaceView` 以透明模式渲染時，它的 Z 軸高度會超過其它所有 Android `View` ，
  這意味著 `SurfaceView` 會展示在其它所有 `View` 之上。
  這是 `SurfaceView` 自身的限制。
  如果可以接受您的 Flutter 內容渲染在其它所有內容之上，
  應該使用預設的 `surface` 作為 `FlutterFragment` 的 `RenderMode` 的配置。
  但是如果需要在 Flutter 內容的上方和下方展示 Android `View`，
  您必須指定 `RenderMode` 為 `texture`。

{{site.alert.end}}

To enable transparency for a `FlutterFragment`,
build it with the following configuration:

要啟動一個透明的 `FlutterFragment`，可以使用以下方式進行建構：

{% samplecode launch-with-transparency %}
{% sample Java %}
<?code-excerpt title="MyActivity.java"?>
```java
// Using a new FlutterEngine.
FlutterFragment flutterFragment = FlutterFragment.withNewEngine()
    .transparencyMode(FlutterView.TransparencyMode.transparent)
    .build();

// Using a cached FlutterEngine.
FlutterFragment flutterFragment = FlutterFragment.withCachedEngine("my_engine_id")
    .transparencyMode(FlutterView.TransparencyMode.transparent)
    .build();
```
{% sample Kotlin %}
<?code-excerpt title="MyActivity.kt"?>
```kotlin
// Using a new FlutterEngine.
val flutterFragment = FlutterFragment.withNewEngine()
    .transparencyMode(FlutterView.TransparencyMode.transparent)
    .build()

// Using a cached FlutterEngine.
val flutterFragment = FlutterFragment.withCachedEngine("my_engine_id")
    .transparencyMode(FlutterView.TransparencyMode.transparent)
    .build()
```
{% endsamplecode %}

## The relationship between `FlutterFragment` and its `Activity`

## `FlutterFragment` 與其 `Activity` 之間的關係

Some apps choose to use `Fragment`s as entire Android screens.
In these apps, it would be reasonable for a `Fragment` to
control system chrome like Android's status bar,
navigation bar, and orientation.

一些應用選擇使用 `Fragment` 作為整個 Android 螢幕內容。
在這些應用裡，`Fragment` 可能會需要控制一些系統屬性，
例如 Android 的狀態列、導航欄以及螢幕方向。

<img src='/assets/images/docs/development/add-to-app/android/add-flutter-fragment/add-flutter-fragment_fullscreen.png'
 class="mw-100" alt="Fullscreen Flutter">

In other apps, `Fragment`s are used to represent only
a portion of a UI. A `FlutterFragment` might be used to
implement the inside of a drawer, a video player,
or a single card. In these situations, it would be
inappropriate for the `FlutterFragment` to affect
Android's system chrome because there are other UI
pieces within the same `Window`.

在其它應用中，`Fragment` 通常只是整個 UI 的一部分。
`FlutterFragment` 可能用於實現抽屜、影片播放器或卡片的內容。
在這些情況下，`FlutterFragment` 就不應當影響 Android 的系統屬性，
因為同一個 `Window` 中還有其它 UI 元件。

<img src='/assets/images/docs/development/add-to-app/android/add-flutter-fragment/add-flutter-fragment_partial-ui.png'
 class="mw-100" alt="Flutter as Partial UI">

`FlutterFragment` comes with a concept that helps
differentiate between the case when a `FlutterFragment`
should be able to control its host `Activity`, and the
cases when a `FlutterFragment` should only affect its
own behavior. To prevent a `FlutterFragment` from
exposing its `Activity` to Flutter plugins, and to
prevent Flutter from controlling the `Activity`'s system UI,
use the `shouldAttachEngineToActivity()` method in
`FlutterFragment`'s `Builder`, as shown:

`FlutterFragment` 自身包含一種特性，
可以用於決定 `FlutterFragment` 是否應該控制宿主 `Activity`，或者隻影響自身行為。
要預防 `FlutterFragment` 將其 `Activity` 暴露給 Flutter 外掛，
以免 Flutter 控制 `Activity` 的系統 UI，
可以使用 `FlutterFragment`  的 `Builder` 中的 `shouldAttachEngineToActivity()` 方法。
如下所示：

{% samplecode attach-to-activity %}
{% sample Java %}
<?code-excerpt title="MyActivity.java"?>
```java
// Using a new FlutterEngine.
FlutterFragment flutterFragment = FlutterFragment.withNewEngine()
    .shouldAttachEngineToActivity(false)
    .build();

// Using a cached FlutterEngine.
FlutterFragment flutterFragment = FlutterFragment.withCachedEngine("my_engine_id")
    .shouldAttachEngineToActivity(false)
    .build();
```
{% sample Kotlin %}
<?code-excerpt title="MyActivity.kt"?>
```kotlin
// Using a new FlutterEngine.
val flutterFragment = FlutterFragment.withNewEngine()
    .shouldAttachEngineToActivity(false)
    .build()

// Using a cached FlutterEngine.
val flutterFragment = FlutterFragment.withCachedEngine("my_engine_id")
    .shouldAttachEngineToActivity(false)
    .build()
```
{% endsamplecode %}

Passing `false` to the `shouldAttachEngineToActivity()`
`Builder` method prevents Flutter from interacting with
the surrounding `Activity`. The default value is `true`,
which allows Flutter and Flutter plugins to interact with the
surrounding `Activity`.

傳遞 `false` 給 `Builder` 的 `shouldAttachEngineToActivity()` 方法，
可防止 Flutter 與所屬的 `Activity` 互動。
預設值為 `true`，此時允許 Flutter 和 Flutter 外掛與 `Activity` 互動。

{{site.alert.note}}

  Some plugins might expect or require an `Activity` reference.
  Ensure that none of your plugins require an `Activity`
  before you disable access.

  一些外掛可能期望或確實需要一個 `Activity` 的參考。
  所以在禁用 `Activity` 的存取許可權之前，請確保沒有外掛需要。

{{site.alert.end}}

[`Fragment`]: https://developer.android.com/guide/components/fragments
[`FlutterFragment`]: {{site.api}}/javadoc/io/flutter/embedding/android/FlutterFragment.html
[using a `FlutterActivity`]: {{site.url}}/add-to-app/android/add-flutter-screen
[`FlutterEngine`]: {{site.api}}/javadoc/io/flutter/embedding/engine/FlutterEngine.html
[`FlutterEngineCache`]: {{site.api}}/javadoc/io/flutter/embedding/engine/FlutterEngineCache.html
[splash screen guide]: {{site.url}}/platform-integration/android/splash-screen
