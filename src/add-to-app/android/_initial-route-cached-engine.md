The concept of an initial route is available when configuring a
`FlutterActivity` or a `FlutterFragment` with a new `FlutterEngine`.
However, `FlutterActivity` and `FlutterFragment` don't offer the
concept of an initial route when using a cached engine.
This is because a cached engine is expected to already be
running Dart code, which means it's too late to configure the
initial route.

當配置一個使用新 `FlutterEngine` 的 `FlutterActivity` 或者 `FlutterFragment` 時，
會使用到初始路由的概念。但是，使用快取中的 Flutter 引擎時，
`FlutterActivity` 或者 `FlutterFragment` 則沒有涉及初始路由的概念。
這是因為被快取的引擎理論上已經執行了 Dart 程式碼，在這時配置初始路由已經太遲了。

Developers that would like their cached engine to begin
with a custom initial route can configure their cached
`FlutterEngine` to use a custom initial route just before
executing the Dart entrypoint. The following example
demonstrates the use of an initial route with a cached engine:

開發者如果想要讓快取中的引擎從自訂的初始路由開始執行，
那麼可以執行 Dart 入口前，為快取的 `FlutterEngine` 配置自訂的初始路由。
如下面這個例子：

{% samplecode cached-engine-with-initial-route %}
{% sample Java %}
<?code-excerpt title="MyApplication.java"?>
```java
public class MyApplication extends Application {
  @Override
  public void onCreate() {
    super.onCreate();
    // Instantiate a FlutterEngine.
    flutterEngine = new FlutterEngine(this);
    // Configure an initial route.
    flutterEngine.getNavigationChannel().setInitialRoute("your/route/here");
    // Start executing Dart code to pre-warm the FlutterEngine.
    flutterEngine.getDartExecutor().executeDartEntrypoint(
      DartEntrypoint.createDefault()
    );
    // Cache the FlutterEngine to be used by FlutterActivity or FlutterFragment.
    FlutterEngineCache
      .getInstance()
      .put("my_engine_id", flutterEngine);
  }
}
```
{% sample Kotlin %}
<?code-excerpt title="MyApplication.kt"?>
```kotlin
class MyApplication : Application() {
  lateinit var flutterEngine : FlutterEngine
  override fun onCreate() {
    super.onCreate()
    // Instantiate a FlutterEngine.
    flutterEngine = FlutterEngine(this)
    // Configure an initial route.
    flutterEngine.navigationChannel.setInitialRoute("your/route/here");
    // Start executing Dart code to pre-warm the FlutterEngine.
    flutterEngine.dartExecutor.executeDartEntrypoint(
      DartExecutor.DartEntrypoint.createDefault()
    )
    // Cache the FlutterEngine to be used by FlutterActivity or FlutterFragment.
    FlutterEngineCache
      .getInstance()
      .put("my_engine_id", flutterEngine)
  }
}
```
{% endsamplecode %}

By setting the initial route of the navigation channel, the associated
`FlutterEngine` displays the desired route upon initial execution of the
`runApp()` Dart function.

透過設定導航通道中的初始路由，
會讓關聯的 `FlutterEngine` 在 `runApp()` 方法首次執行後，
展示已配置的路由頁面。

Changing the initial route property of the navigation channel
after the initial execution of `runApp()` has no effect.
Developers who would like to use the same `FlutterEngine`
between different `Activity`s and `Fragment`s and switch
the route between those displays need to setup a method channel and
explicitly instruct their Dart code to change `Navigator` routes.

在 `runApp()` 的首次執行之後，修改導航通道中的初始路由屬性是不會生效的。
想要在不同的 `Activity` 和 `Fragment` 之間使用同一個 `FlutterEngine`，
並且在其展示時切換不同的路由，開發者需要設定一個方法通道，
來明確地通知他們的 Dart 程式碼切換 `Navigator` 路由。

