---
title: Adding a Flutter screen to an Android app
title: 在 Android 應用中新增 Flutter 頁面
short-title: Add a Flutter screen
short-title: 新增一個 Flutter 頁面
description: Learn how to add a single Flutter screen to your existing Android app.
description: 瞭解如何在你現有的 Android 應用中新增單個 Flutter 頁面。
tags: Flutter混合工程,add2app
keywords: Android,Flutter 頁面
---

This guide describes how to add a single Flutter screen to an
existing Android app. A Flutter screen can be added as a normal,
opaque screen, or as a see-through, translucent screen.
Both options are described in this guide.

本指南講述瞭如何在一個現有的 Android 應用中新增單個 Flutter 頁面。
新增到應用中的單個 Flutter 頁面可以是不透明的普通頁面，也可以是透明的頁面。
這兩種頁面的使用都會在本指南中提到。

## Add a normal Flutter screen

## 新增一個普通的 Flutter 頁面

<img src='/assets/images/docs/development/add-to-app/android/add-flutter-screen/add-single-flutter-screen_header.png'
class="mw-100" alt="Add Flutter Screen Header">

### Step 1: Add FlutterActivity to AndroidManifest.xml

### 步驟 1：在 AndroidManifest.xml 中新增 FlutterActivity

Flutter provides [`FlutterActivity`][] to display a Flutter
experience within an Android app. Like any other [`Activity`][],
`FlutterActivity` must be registered in your
`AndroidManifest.xml`. Add the following XML to your
`AndroidManifest.xml` file under your `application` tag:

Flutter 提供了 [`FlutterActivity`][]，用於在 Android 應用內部展示一個 Flutter 的互動介面。
和其他的 [`Activity`][] 一樣，`FlutterActivity` 必須在專案的 `AndroidManifest.xml` 檔案中註冊。
將下邊的 XML 程式碼新增到你的 `AndroidManifest.xml` 檔案中的 `application` 標籤內：

```xml
<activity
  android:name="io.flutter.embedding.android.FlutterActivity"
  android:theme="@style/LaunchTheme"
  android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale|layoutDirection|fontScale|screenLayout|density|uiMode"
  android:hardwareAccelerated="true"
  android:windowSoftInputMode="adjustResize"
  />
```

The reference to `@style/LaunchTheme` can be replaced
by any Android theme that want to apply to your `FlutterActivity`.
The choice of theme dictates the colors applied to
Android's system chrome, like Android's navigation bar, and to
the background color of the `FlutterActivity` just before
the Flutter UI renders itself for the first time.

上述程式碼中的 `@style/LaunchTheme` 可以替換為想要在你的
`FlutterActivity` 中使用的其他 Android 主題。
主題的選擇決定 Android 系統展示框架所使用的顏色，
例如 Android 的導航欄，以及 Flutter UI 自身的第一次渲染前 `FlutterActivity` 的背景色。

### Step 2: Launch FlutterActivity

### 步驟 2：載入 FlutterActivity

With `FlutterActivity` registered in your manifest file,
add code to launch `FlutterActivity` from whatever point
in your app that you'd like. The following example shows
`FlutterActivity` being launched from an `OnClickListener`.

在你的清單檔案中註冊了 `FlutterActivity` 之後，
根據需要，你可以在應用中的任意位置新增開啟 `FlutterActivity` 的程式碼。
下邊的程式碼展示瞭如何在 `OnClickListener` 的點選事件中開啟 `FlutterActivity`。

{{site.alert.note}}

  Make sure to use the following import:

  確保使用如下的陳述式匯入：
  
<!--skip-->
```java
import io.flutter.embedding.android.FlutterActivity;
```

{{site.alert.end}}

{% samplecode default-activity-launch %}
{% sample Java %}
<!--code-excerpt "ExistingActivity.java" title-->
```java
myButton.setOnClickListener(new OnClickListener() {
  @Override
  public void onClick(View v) {
    startActivity(
      FlutterActivity.createDefaultIntent(currentActivity)
    );
  }
});
```
{% sample Kotlin %}
<!--code-excerpt "ExistingActivity.kt" title-->
```kotlin
myButton.setOnClickListener {
  startActivity(
    FlutterActivity.createDefaultIntent(this)
  )
}
```
{% endsamplecode %}

The previous example assumes that your Dart entrypoint
is called `main()`, and your initial Flutter route is '/'.
The Dart entrypoint can't be changed using `Intent`,
but the initial route can be changed using `Intent`.
The following example demonstrates how to launch a
`FlutterActivity` that initially renders a custom
route in Flutter.

上述的例子假定了你的 Dart 程式碼入口是呼叫 `main()`，並且你的 Flutter 初始路由是 '/'。
Dart 程式碼入口不能透過 `Intent` 改變，但是初始路由可以透過 `Intent` 來修改。
下面的例子講解了如何開啟一個自訂 Flutter 初始路由的 `FlutterActivity`。

{% samplecode custom-activity-launch %}
{% sample Java %}
<!--code-excerpt "ExistingActivity.java" title-->
```java
myButton.addOnClickListener(new OnClickListener() {
  @Override
  public void onClick(View v) {
    startActivity(
      FlutterActivity
        .withNewEngine()
        .initialRoute("/my_route")
        .build(currentActivity)
      );
  }
});
```
{% sample Kotlin %}
<!--code-excerpt "ExistingActivity.kt" title-->
```kotlin
myButton.setOnClickListener {
  startActivity(
    FlutterActivity
      .withNewEngine()
      .initialRoute("/my_route")
      .build(this)
  )
}
```
{% endsamplecode %}

Replace `"/my_route"` with your desired initial route.

可以用你想要的初始路由替換掉 `"/my_route"`。

The use of the `withNewEngine()` factory method
configures a `FlutterActivity` that internally create
its own [`FlutterEngine`][] instance. This comes with a
non-trivial initialization time. The alternative approach
is to instruct `FlutterActivity` to use a pre-warmed,
cached `FlutterEngine`, which minimizes Flutter's
initialization time. That approach is discussed next.

工廠方法 `withNewEngine()` 可以用於配置一個 `FlutterActivity`，
它會在內部建立一個屬於自己的 `FlutterEngine` 例項，這會有一個明顯的初始化時間。
另外一種可選的做法是讓
`FlutterActivity` 使用一個預熱且快取的 `FlutterEngine`，
這可以最小化 Flutter 初始化的時間。
這種方式接下來會討論到。

### Step 3: (Optional) Use a cached FlutterEngine

### 步驟 3：（可選）使用快取的 FlutterEngine

Every `FlutterActivity` creates its own `FlutterEngine`
by default. Each `FlutterEngine` has a non-trivial
warm-up time. This means that launching a standard
`FlutterActivity` comes with a brief delay before your Flutter
experience becomes visible. To minimize this delay,
you can warm up a `FlutterEngine` before arriving at
your `FlutterActivity`, and then you can use
your pre-warmed `FlutterEngine` instead.

每一個 `FlutterActivity` 預設會建立它自己的 `FlutterEngine`。
每一個 `FlutterEngine` 會有一個明顯的預熱時間。
這意味著載入一個標準的 `FlutterActivity` 時，
在你的 Flutter 互動頁面可見之前會有一個短暫的延遲。
想要最小化這個延遲時間，你可以在抵達你的 `FlutterActivity` 之前，
初始化一個 `FlutterEngine`，然後使用這個已經預熱好的 `FlutterEngine`。

To pre-warm a `FlutterEngine`, find a reasonable
location in your app to instantiate a `FlutterEngine`.
The following example arbitrarily pre-warms a
`FlutterEngine` in the `Application` class:

要預熱一個 `FlutterEngine`，
可以在你的應用中找一個合理的地方例項化一個 `FlutterEngine`。
下面的這個例子是在 `Application` 類中預先初始化一個 `FlutterEngine`：

{% samplecode prewarm-engine %}
{% sample Java %}
<!--code-excerpt "MyApplication.java" title-->
```java
public class MyApplication extends Application {
  public FlutterEngine flutterEngine;
  
  @Override
  public void onCreate() {
    super.onCreate();
    // Instantiate a FlutterEngine.
    flutterEngine = new FlutterEngine(this);

    // Start executing Dart code to pre-warm the FlutterEngine.
    flutterEngine.getDartExecutor().executeDartEntrypoint(
      DartEntrypoint.createDefault()
    );

    // Cache the FlutterEngine to be used by FlutterActivity.
    FlutterEngineCache
      .getInstance()
      .put("my_engine_id", flutterEngine);
  }
}
```
{% sample Kotlin %}
<!--code-excerpt "MyApplication.kt" title-->
```kotlin
class MyApplication : Application() {
  lateinit var flutterEngine : FlutterEngine

  override fun onCreate() {
    super.onCreate()

    // Instantiate a FlutterEngine.
    flutterEngine = FlutterEngine(this)

    // Start executing Dart code to pre-warm the FlutterEngine.
    flutterEngine.dartExecutor.executeDartEntrypoint(
      DartExecutor.DartEntrypoint.createDefault()
    )

    // Cache the FlutterEngine to be used by FlutterActivity.
    FlutterEngineCache
      .getInstance()
      .put("my_engine_id", flutterEngine)
  }
}
```
{% endsamplecode %}

The ID passed to the [`FlutterEngineCache`][] can be whatever you want.
Make sure that you pass the same ID to any `FlutterActivity`
or [`FlutterFragment`][] that should use the cached `FlutterEngine`.
Using `FlutterActivity` with a cached `FlutterEngine`
is discussed next.

傳給 [`FlutterEngineCache`][] 的 ID 可以是你想要的任何名稱。
確保 `FlutterActivity` 或 [`FlutterFragment`][] 在使用快取的
`FlutterEngine` 時，傳遞了同樣的 ID。
基於快取的 `FlutterEngine` 來使用 `FlutterActivity` 會在後續討論到。

{{site.alert.note}}

  To warm up a `FlutterEngine`, you must execute a Dart
  entrypoint. Keep in mind that the moment
  `executeDartEntrypoint()` is invoked,
  your Dart entrypoint method begins executing.
  If your Dart entrypoint invokes `runApp()`
  to run a Flutter app, then your Flutter app behaves as if it
  were running in a window of zero size until this
  `FlutterEngine` is attached to a `FlutterActivity`,
  `FlutterFragment`, or `FlutterView`. Make sure that your app
  behaves appropriately between the time you warm it up and
  the time you display Flutter content.

  要預熱一個 `FlutterEngine`，你必須執行一個 Dart 入口。
  切記當 `executeDartEntrypoint()` 方法呼叫時，你的 Dart 入口方法就會開始執行。
  如果你的 Dart 入口呼叫了 `runApp()` 來執行一個 Flutter 應用，
  那麼你的 Flutter 應用會像是執行在一個大小為零的視窗中，
  直至 `FlutterEngine` 附屬到一個 `FlutterActivity`，`FlutterFragment` 或 `FlutterView`。
  請確保你的應用在開始預熱到你展示 Flutter 內容中間的這段時間裡表現正常。
  
{{site.alert.end}}

With a pre-warmed, cached `FlutterEngine`, you now need
to instruct your `FlutterActivity` to use the cached
`FlutterEngine` instead of creating a new one.
To accomplish this, use `FlutterActivity`'s `withCachedEngine()`
builder:

要使用預熱且快取的 `FlutterEngine` 時，
讓你的 `FlutterActivity` 從快取中獲取 `FlutterEngine`，而不是建立一個新的。
可以使用 `FlutterActivity` 的 `withCachedEngine()` 方法來實現：

{% samplecode cached-engine-activity-launch %}
{% sample Java %}
<!--code-excerpt "ExistingActivity.java" title-->
```java
myButton.addOnClickListener(new OnClickListener() {
  @Override
  public void onClick(View v) {
    startActivity(
      FlutterActivity
        .withCachedEngine("my_engine_id")
        .build(currentActivity)
      );
  }
});
```
{% sample Kotlin %}
<!--code-excerpt "ExistingActivity.kt" title-->
```kotlin
myButton.setOnClickListener {
  startActivity(
    FlutterActivity
      .withCachedEngine("my_engine_id")
      .build(this)
  )
}
```
{% endsamplecode %}

When using the `withCachedEngine()` factory method,
pass the same ID that you used when caching the desired
`FlutterEngine`.

當使用 `withCachedEngine()` 方法時，傳遞你快取對應 `FlutterEngine` 時用的相同 ID。

Now, when you launch `FlutterActivity`,
there is significantly less delay in
the display of Flutter content.

現在，當你載入 `FlutterActivity` 時，在展示 Flutter 內容前的延遲會明顯降低。

{{site.alert.note}}

  When using a cached `FlutterEngine`, that `FlutterEngine` outlives any
  `FlutterActivity` or `FlutterFragment` that displays it. Keep in
  mind that Dart code begins executing as soon as you pre-warm the
  `FlutterEngine`, and continues executing after the destruction of your
  `FlutterActivity`/`FlutterFragment`. To stop executing and clear resources,
  obtain your `FlutterEngine` from the `FlutterEngineCache` and destroy the
  `FlutterEngine` with `FlutterEngine.destroy()`.

  當使用一個快取的 `FlutterEngine` 時，
  `FlutterEngine` 會比展示它的 `FlutterActivity` 或 `FlutterFragment` 存活得更久。
  切記，Dart 程式碼會在你預熱 `FlutterEngine` 時就開始執行，
  並且在你的 `FlutterActivity` 或 `FlutterFragment` 銷燬後繼續執行。
  要停止程式碼執行和清理相關資源，可以從 `FlutterEngineCache` 中獲取你的 `FlutterEngine`，
  然後使用 `FlutterEngine.destroy()` 來銷燬 `FlutterEngine`。

{{site.alert.end}}

{{site.alert.note}}

  Runtime performance isn't the only reason that you might
  pre-warm and cache a `FlutterEngine`.
  A pre-warmed `FlutterEngine` executes Dart code independent
  from a `FlutterActivity`, which allows such a `FlutterEngine`
  to be used to execute arbitrary Dart code at any moment.
  Non-UI application logic can be executed in a `FlutterEngine`,
  like networking and data caching, and in background behavior
  within a `Service` or elsewhere. When using a `FlutterEngine`
  to execute behavior in the background, be sure to adhere to all
  Android restrictions on background execution.

  執行時的效能考量並不是你會預熱和快取一個 `FlutterEngine` 的唯一原因。
  一個預熱的 `FlutterEngine` 會獨立於 `FlutterActivity` 執行 Dart 程式碼，
  即一個 `FlutterEngine` 可以在任意時刻用於執行任意程式碼。
  非 UI 的應用邏輯可以在 `FlutterEngine` 中執行，
  例如網路請求和資料快取，以及在 `Service` 中或其他地方的後臺行為。
  當使用 `FlutterEngine` 在後台執行任務時，確保滿足 Android 對於後臺執行的所有限制。

{{site.alert.end}}

{{site.alert.note}}

  Flutter's debug/release builds have drastically different
  performance characteristics. To evaluate the performance
  of Flutter, use a release build.

  Flutter 的 debug 與 release 建構體現了完全不同的效能。
  評估 Flutter 的效能表現時，請使用 release 建構版本。

{{site.alert.end}}

#### Initial route with a cached engine

#### 為快取的 FlutterEngine 設定初始路由

{% include_relative _initial-route-cached-engine.md %}

## Add a translucent Flutter screen

## 新增一個透明主題的 FlutterActivity

<img src='/assets/images/docs/development/add-to-app/android/add-flutter-screen/add-single-flutter-screen-transparent_header.png'
class="mw-100" alt="Add Flutter Screen With Translucency Header">

Most full-screen Flutter experiences are opaque.
However, some apps would like to deploy a Flutter
screen that looks like a modal, for example,
a dialog or bottom sheet. Flutter supports translucent
`FlutterActivity`s out of the box.

大部分的全屏 Flutter 互動頁面是不透明的。
但是，一些應用可能會發佈一個類似模態框的 Flutter 頁面，
例如，一個對話方塊或者底部工作表。
Flutter 預設支援透明的 `FlutterActivity`。

To make your `FlutterActivity` translucent,
make the following changes to the regular process of
creating and launching a `FlutterActivity`.

要將你的 `FlutterActivity` 設定為透明，
在建立和載入 `FlutterActivity` 的常規步驟中做如下的變更。

### Step 1: Use a theme with translucency

### 步驟 1：使用透明的主題

Android requires a special theme property for `Activity`s that render
with a translucent background. Create or update an Android theme with the
following property:

Android 需要一個特殊的主題屬性來讓 `Activity` 以一個透明的背景渲染。
使用如下屬性來建立或者修改一個 Android 主題：

```xml
<style name="MyTheme" parent="@style/MyParentTheme">
  <item name="android:windowIsTranslucent">true</item>
</style>
```

Then, apply the translucent theme to your `FlutterActivity`.

然後，將透明主題應用到你的 `FlutterActivity`。

```xml
<activity
  android:name="io.flutter.embedding.android.FlutterActivity"
  android:theme="@style/MyTheme"
  android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale|layoutDirection|fontScale|screenLayout|density|uiMode"
  android:hardwareAccelerated="true"
  android:windowSoftInputMode="adjustResize"
  />
```

Your `FlutterActivity` now supports translucency.
Next, you need to launch your `FlutterActivity`
with explicit transparency support.

現在你的 `FlutterActivity` 已經支援透明化。
下一步，你需要在開啟 `FlutterActivity` 時顯式啟用透明模式。

### Step 2: Start FlutterActivity with transparency

### 步驟 2：啟動透明的 FlutterActivity

To launch your `FlutterActivity` with a transparent background,
pass the appropriate `BackgroundMode` to the `IntentBuilder`:

要開啟透明背景的 `FlutterActivity`，需要把對應的 `BackgroundMode` 傳遞給 `IntentBuilder`：

{% samplecode transparent-activity-launch %}
{% sample Java %}
<!--code-excerpt "ExistingActivity.java" title-->
```java
// Using a new FlutterEngine.
startActivity(
  FlutterActivity
    .withNewEngine()
    .backgroundMode(FlutterActivityLaunchConfigs.BackgroundMode.transparent)
    .build(context)
);

// Using a cached FlutterEngine.
startActivity(
  FlutterActivity
    .withCachedEngine("my_engine_id")
    .backgroundMode(FlutterActivityLaunchConfigs.BackgroundMode.transparent)
    .build(context)
);
```

{% sample Kotlin %}
<!--code-excerpt "ExistingActivity.kt" title-->
```kotlin
// Using a new FlutterEngine.
startActivity(
  FlutterActivity
    .withNewEngine()
    .backgroundMode(FlutterActivityLaunchConfigs.BackgroundMode.transparent)
    .build(this)
);

// Using a cached FlutterEngine.
startActivity(
  FlutterActivity
    .withCachedEngine("my_engine_id")
    .backgroundMode(FlutterActivityLaunchConfigs.BackgroundMode.transparent)
    .build(this)
);
```
{% endsamplecode %}

You now have a `FlutterActivity` with a transparent background.

現在你的 `FlutterAcivity` 的背景已經是透明的了。

{{site.alert.note}}

  Make sure that your Flutter content also includes a
  translucent background. If your Flutter UI paints a
  solid background color, then it still appears as
  though your `FlutterActivity` has an opaque background.

  確保你的 Flutter 內容也有一個透明的背景。
  如果你的 Flutter UI 繪製了一個特定的背景顏色，
  那麼你的 `FlutterActivity` 依舊看起來會是有一個不透明的背景。

{{site.alert.end}}

[`FlutterActivity`]: {{site.api}}/javadoc/io/flutter/embedding/android/FlutterActivity.html
[`Activity`]: https://developer.android.com/reference/android/app/Activity
[`FlutterEngine`]: {{site.api}}/javadoc/io/flutter/embedding/engine/FlutterEngine.html
[`FlutterEngineCache`]: {{site.api}}/javadoc/io/flutter/embedding/engine/FlutterEngineCache.html
[`FlutterFragment`]: {{site.api}}/javadoc/io/flutter/embedding/android/FlutterFragment.html
