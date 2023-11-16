---
title: Supporting the new Android plugins APIs
title: 支援新的 Android 的 API
description: How to update a plugin using the old APIs to support the new APIs。
description: 如何升級舊 API 的外掛以支援新的 API。
tags: Packages,外掛
keywords: 遷移,Android平台,開發,新特性
---

<?code-excerpt path-base="development/plugin_api_migration"?>

{{site.alert.note}}

  You might be directed to this page if the framework detects that
  your app uses a plugin based on the old Android APIs.

  如果我們檢測到您的應用專案中使用的外掛正在使用舊的 Android APIs ，
  會提示您前往本頁面。
{{site.alert.end}}

_If you don't write or maintain an Android Flutter plugin,
you can skip this page._

**如果您並非親自開發或維護一個 Flutter 的 Android 外掛，
您可以跳過本頁面。**

As of the 1.12 release,
new plugin APIs are available for the Android platform.
The old APIs based on [`PluginRegistry.Registrar`][]
won't be immediately deprecated,
but we encourage you to migrate to the new APIs based on
[`FlutterPlugin`][].

自 1.12 版本釋出後，
Android 平台已可以使用新的 Android 外掛 API 。
基於 [`PluginRegistry.Registrar`][] 的 API 不會立刻廢棄，
但我們鼓勵您向基於 [`FlutterPlugin`][] 的 API 進行遷移。

The new API has the advantage of providing a cleaner set
of accessors for lifecycle dependent components compared
to the old APIs. For instance
[`PluginRegistry.Registrar.activity()`][] could return null if
Flutter isn't attached to any activities.

相較舊的 API 而言，新版 API 的優點是為生命週期的相關元件提供了
更簡潔清晰的存取方式。例如，在使用舊的 [`PluginRegistry.Registrar.activity()`][]
時，如果 Flutter 尚未附加到任何 activites，可能會返回 null 。

In other words, plugins using the old API may produce undefined
behaviors when embedding Flutter into an Android app.
Most of the [Flutter plugins][] provided by the flutter.dev
team have been migrated already. (Learn how to become a
[verified publisher][] on pub.dev!) For an example of
a plugin that uses the new APIs, see the
[battery plus package][].

換句話說，在使用舊的 API 進行 Flutter 嵌入 Android 應用時，
可能會產生意外的行為。
Flutter 開發團隊提供的大部分 [Flutter 外掛][Flutter plugins] 已經完成了遷移。
（瞭解如何成為 [認證的釋出者][verified publisher]）
作為參考， [battery plus package][] 已經遷移到新版 API 。

## Upgrade steps

## 升級步驟

The following instructions outline the steps for supporting the new API:

以下的步驟簡要說明了如何支援新版 API ：

1. Update the main plugin class (`*Plugin.java`) to implement the
   [`FlutterPlugin`][] interface. For more complex plugins,
   you can separate the `FlutterPlugin` and `MethodCallHandler`
   into two classes. See the next section, [Basic plugin][],
   for more details on accessing app resources with
   the latest version (v2) of embedding.
   <br><br>
   Also, note that the plugin should still contain the static
   `registerWith()` method to remain compatible with apps that
   don't use the v2 Android embedding.
   (See [Upgrading pre 1.12 Android projects][] for details.)
   The easiest thing to do (if possible) is move the logic from
   `registerWith()` into a private method that both
   `registerWith()` and `onAttachedToEngine()` can call.
   Either `registerWith()` or `onAttachedToEngine()` will be called,
   not both.
   <br><br>
   In addition, you should document all non-overridden public members
   within the plugin. In an add-to-app scenario,
   these classes are accessible to a developer and
   require documentation.
   
   在外掛的主類檔案中 (`*Plugin.java`) 實現 [`FlutterPlugin`][] 介面。
   對於稍微複雜的外掛，您可以將 `FlutterPlugin` 與 `MethodCallHandler`
   拆分到不同的類中。如需更多關於如何使用新版 API 獲取資源的內容，請參考下一節
   [基礎外掛][Basic plugin] 。
   <br><br>
   同時需要注意的是，外掛仍需保留靜態的 `registerWith()` 方法，從而適配不相容
   v2 版本嵌入的應用。 (檢視 [Upgrading pre 1.12 Android projects][] 獲取更多資訊)
   <br><br>
   此外，所有不可覆蓋的公開成員都應該使用文件標註。在嵌入開發的場景下，這些可見內容
   通常需要包含文件。

1. (Optional) If your plugin needs an `Activity` reference,
   also implement the [`ActivityAware`][] interface.
   
   （可選）如果您的外掛需要 `Activity` 的參考，請同時實現 [`ActivityAware`][] 介面。

1. (Optional) If your plugin is expected to be held in a
   background Service at any point in time, implement the
   [`ServiceAware`][] interface.
   
   （可選）如果您的外掛需要隨時保持一個後臺 Service ，請實現 [`ServiceAware`][] 介面。

1. Update the example app's `MainActivity.java` to use the
   v2 embedding `FlutterActivity`. For details, see
   [Upgrading pre 1.12 Android projects][].
   You might have to make a public constructor for your plugin class
   if one didn't exist already. For example:
   
   使用 `FlutterActivity` 將範例應用中的 `MainActivity.java`
   遷移到 v2 版本嵌入。
   更多資訊請檢視 [Upgrading pre 1.12 Android projects][] 。
   如果您的外掛類尚不存在，則必須新增一個公有建構函式。例如：

   <?code-excerpt title="MainActivity.java"?>
   ```java
    package io.flutter.plugins.firebasecoreexample;

    import io.flutter.embedding.android.FlutterActivity;
    import io.flutter.embedding.engine.FlutterEngine;
    import io.flutter.plugins.firebase.core.FirebaseCorePlugin;

    public class MainActivity extends FlutterActivity {
      // You can keep this empty class or remove it. Plugins on the new embedding
      // now automatically registers plugins.
    }
    ```

1. (Optional) If you removed `MainActivity.java`, update the
   `<plugin_name>/example/android/app/src/main/AndroidManifest.xml`
   to use `io.flutter.embedding.android.FlutterActivity`.
   For example:
   
   （可選）如果您移除了 `MainActivity.java`，請更新
   `<plugin_name>/example/android/app/src/main/AndroidManifest.xml`
   以使用 `io.flutter.embedding.android.FlutterActivity`。例如：

    <?code-excerpt title="AndroidManifest.xml"?>
    ```xml
     <activity android:name="io.flutter.embedding.android.FlutterActivity"
            android:theme="@style/LaunchTheme"
   android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale|layoutDirection|fontScale"
            android:hardwareAccelerated="true"
            android:exported="true"
            android:windowSoftInputMode="adjustResize">
            <meta-data
                android:name="io.flutter.app.android.SplashScreenUntilFirstFrame"
                android:value="true" />
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    ```

1. (Optional) Create an `EmbeddingV1Activity.java` file
   that uses the v1 embedding for the example project
   in the same folder as `MainActivity` to
   keep testing the v1 embedding's compatibility
   with your plugin. Note that you have to manually
   register all the plugins instead of using
   `GeneratedPluginRegistrant`.  For example:
   
   （可選）在 `MainActivity.java` 同級目錄下建立一個 
   `EmbeddingV1Activity.java` 檔案，使用 v1 版本嵌入以
   持續測試您的專案對 v1 版本嵌入的相容性。例如：

    <?code-excerpt title="EmbeddingV1Activity.java"?>
    ```java
    package io.flutter.plugins.batteryexample;

    import android.os.Bundle;
    import io.flutter.app.FlutterActivity;
    import io.flutter.plugins.battery.BatteryPlugin;

    public class EmbeddingV1Activity extends FlutterActivity {
      @Override
      protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        BatteryPlugin.registerWith(registrarFor("io.flutter.plugins.battery.BatteryPlugin"));
      }
    }
    ```

1.  Add `<meta-data android:name="flutterEmbedding" android:value="2"/>`
    to the `<plugin_name>/example/android/app/src/main/AndroidManifest.xml`.
    This sets the example app to use the v2 embedding.
    
    將 `<meta-data android:name="flutterEmbedding" android:value="2"/>`
    新增至 `<plugin_name>/example/android/app/src/main/AndroidManifest.xml` 。
    這會讓範例應用使用 v2 版本的嵌入。

1. (Optional) If you created an `EmbeddingV1Activity`
   in the previous step, add the `EmbeddingV1Activity` to the
   `<plugin_name>/example/android/app/src/main/AndroidManifest.xml` file.
   For example:
   
   （可選）如果上步您建立了 `EmbeddingV1Activity` ，
   將 `EmbeddingV1Activity`
   新增至 `<plugin_name>/example/android/app/src/main/AndroidManifest.xml` 檔案。
   例如：

    <?code-excerpt title="AndroidManifest.xml"?>
    ```xml
    <activity
        android:name=".EmbeddingV1Activity"
        android:theme="@style/LaunchTheme"
            android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale|layoutDirection|fontScale"
        android:hardwareAccelerated="true"
        android:exported="true"
        android:windowSoftInputMode="adjustResize">
    </activity>
    ```

## Testing your plugin

## 測試您的外掛

The remaining steps address testing your plugin, which we encourage,
but aren't required.

剩下的步驟讓您可以測試您的外掛，我們鼓勵您這樣做，但這並不是必需的。

1. Update `<plugin_name>/example/android/app/build.gradle`
   to replace references to `android.support.test` with `androidx.test`:
   
   替換 `<plugin_name>/example/android/app/build.gradle` 檔案中
   `android.support.test` 的參考為 `androidx.test` ：

    <?code-excerpt title="build.gradle"?>
    ```groovy
    defaultConfig {
      ...
      testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
      ...
    }
    ```

    <?code-excerpt title="build.gradle"?>
    ```groovy
    dependencies {
    ...
    androidTestImplementation 'androidx.test:runner:1.2.0'
    androidTestImplementation 'androidx.test:rules:1.2.0'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.2.0'
    ...
    }
    ```

1. Add tests files for `MainActivity` and `EmbeddingV1Activity`
   in `<plugin_name>/example/android/app/src/androidTest/java/<plugin_path>/`.
   You will need to create these directories. For example:
   
   在 `<plugin_name>/example/android/app/src/androidTest/java/<plugin_path>/` 
   路徑下新增針對 `MainActivity` 和 `EmbeddingV1Activity` 的測試檔案，
   並且您需要建立該目錄。例如：

    <?code-excerpt title="MainActivityTest.java"?>
    ```java
    package io.flutter.plugins.firebase.core;

    import androidx.test.rule.ActivityTestRule;
    import io.flutter.plugins.firebasecoreexample.MainActivity;
    import org.junit.Rule;
    import org.junit.runner.RunWith;

    @RunWith(FlutterRunner.class)
    public class MainActivityTest {
      // Replace `MainActivity` with `io.flutter.embedding.android.FlutterActivity` if you removed `MainActivity`.
      @Rule public ActivityTestRule<MainActivity> rule = new ActivityTestRule<>(MainActivity.class);
    }
    ```

    <?code-excerpt title="EmbeddingV1ActivityTest.java"?>
    ```java
    package io.flutter.plugins.firebase.core;

    import androidx.test.rule.ActivityTestRule;
    import io.flutter.plugins.firebasecoreexample.EmbeddingV1Activity;
    import org.junit.Rule;
    import org.junit.runner.RunWith;

    @RunWith(FlutterRunner.class)
    public class EmbeddingV1ActivityTest {
      @Rule
      public ActivityTestRule<EmbeddingV1Activity> rule =
          new ActivityTestRule<>(EmbeddingV1Activity.class);
    }
    ```

1. Add `integration_test` and `flutter_driver` dev_dependencies to
   `<plugin_name>/pubspec.yaml` and
   `<plugin_name>/example/pubspec.yaml`.
   
   在 `<plugin_name>/pubspec.yaml` 和
   `<plugin_name>/example/pubspec.yaml` 中的
   dev_dependencies 下新增 `e2e` 和 `flutter_driver`。

    <?code-excerpt title="pubspec.yaml"?>
    ```yaml
    integration_test:
      sdk: flutter
    flutter_driver:
      sdk: flutter
    ```

1. Update minimum Flutter version of environment in
   `<plugin_name>/pubspec.yaml`. All plugins moving
   forward will set the minimum version to 1.12.13+hotfix.6
   which is the minimum version for which we can guarantee support.
   For example:
   
   更新 `<plugin_name>/pubspec.yaml` 中 Flutter 版本的最低限制。
   所有已遷移的外掛都將會設定最低版本為我們保證支援的最低版本
   1.12.13+hotfix.6。例如：

    <?code-excerpt title="pubspec.yaml"?>
    ```yaml
    environment:
      sdk: ">=2.16.1 <3.0.0"
      flutter: ">=1.17.0"
    ```

1. Create a simple test in `<plugin_name>/test/<plugin_name>_test.dart`.
   For the purpose of testing the PR that adds the v2 embedding support,
   we're trying to test some very basic functionality of the plugin.
   This is a smoke test to ensure that the plugin properly registers
   with the new embedder. For example:
   
   在 `<plugin_name>/test/<plugin_name>_e2e.dart` 中建立一個簡單的測試。
   為了測試添加了 v2 版本嵌入支援的 PR，我們將嘗試測試一些外掛的基礎功能。
   這是一個確保外掛正確註冊到新的嵌入器的煙霧測試。例如：

    <?code-excerpt "lib/test.dart (Test)"?>
    ```dart
    import 'package:flutter_test/flutter_test.dart';
    import 'package:integration_test/integration_test.dart';

    void main() {
      IntegrationTestWidgetsFlutterBinding.ensureInitialized();

      testWidgets('Can get battery level', (tester) async {
        final Battery battery = Battery();
        final int batteryLevel = await battery.batteryLevel;
        expect(batteryLevel, isNotNull);
      });
    }
    ```

1. Test run the `integration_test` tests locally. In a terminal,
   do the following:
   
   本地執行 e2e 測試。在終端中執行以下內容：

    ```terminal
    flutter test integration_test/app_test.dart
    ```

## Basic plugin

## 基礎外掛

To get started with a Flutter Android plugin in code,
start by implementing `FlutterPlugin`.

要開始開發一個新的 Flutter Android 外掛，請從 `FlutterPlugin` 的實現開始。

```java
public class MyPlugin implements FlutterPlugin {
  @Override
  public void onAttachedToEngine(@NonNull FlutterPluginBinding binding) {
    // TODO: your plugin is now attached to a Flutter experience.
  }

  @Override
  public void onDetachedFromEngine(@NonNull FlutterPluginBinding binding) {
    // TODO: your plugin is no longer attached to a Flutter experience.
  }
}
```

As shown above, your plugin might (or might not)
be associated with a given Flutter experience at
any given moment in time.
You should take care to initialize your plugin's behavior
in `onAttachedToEngine()`, and then cleanup your plugin's
references in `onDetachedFromEngine()`.

如上述程式碼所示，您的外掛在任意時刻都可能與 Flutter 的體驗有關或無關。
您需要特別注意，在 `onAttachedToEngine()` 進行初始化，並且在
`onDetachedFromEngine()` 中進行清理外掛的各種參考。

The FlutterPluginBinding provides your plugin with a few
important references:

FlutterPluginBinding 為您的外掛提供了幾個重要的參考：

**binding.getFlutterEngine()**
<br> Returns the `FlutterEngine` that your plugin is attached to,
  providing access to components like the `DartExecutor`,
  `FlutterRenderer`, and more.

**binding.getFlutterEngine()**
<br> 返回外掛附加到的 `FlutterEngine` ，提供了諸如 `DartExecutor` 、
  `FlutterRenderer` 等內容的獲取。

**binding.getApplicationContext()**
<br> Returns the Android application's `Context` for the running app.

**binding.getApplicationContext()**
<br> 返回當前執行的安卓應用的 Application `Context` 。

## UI/Activity plugin

## UI/Activity 外掛

If your plugin needs to interact with the UI,
such as requesting permissions, or altering Android UI chrome,
then you need to take additional steps to define your plugin.
You must implement the `ActivityAware` interface.

如果您的外掛需要與 UI 進行互動，例如請求許可權或更改 Android UI ，
那麼您就需要一些附加步驟來建構您的外掛。
您必須實現 `ActivityAware` 介面。

```java
public class MyPlugin implements FlutterPlugin, ActivityAware {
  //...normal plugin behavior is hidden...

  @Override
  public void onAttachedToActivity(ActivityPluginBinding activityPluginBinding) {
    // TODO: your plugin is now attached to an Activity
  }

  @Override
  public void onDetachedFromActivityForConfigChanges() {
    // TODO: the Activity your plugin was attached to was
    // destroyed to change configuration.
    // This call will be followed by onReattachedToActivityForConfigChanges().
  }

  @Override
  public void onReattachedToActivityForConfigChanges(ActivityPluginBinding activityPluginBinding) {
    // TODO: your plugin is now attached to a new Activity
    // after a configuration change.
  }

  @Override
  public void onDetachedFromActivity() {
    // TODO: your plugin is no longer associated with an Activity.
    // Clean up references.
  }
}
```

To interact with an `Activity`, your `ActivityAware` plugin must
implement appropriate behavior at 4 stages. First, your plugin
is attached to an `Activity`. You can access that `Activity` and
a number of its callbacks through the provided `ActivityPluginBinding`.

若需要與 `Activity` 互動，您已經實現 `ActivityAware` 的外掛需要
在 4 個不同的階段實現不同的行為。
首先，確保您的外掛已經附加至 `Activity` 。
您可以透過提供的 `ActivityPluginBinding` 獲取到 `Activity` 及一些回呼(Callback)。

Since `Activity`s can be destroyed during configuration changes,
you must clean up any references to the given `Activity` in
`onDetachedFromActivityForConfigChanges()`,
and then re-establish those references in
`onReattachedToActivityForConfigChanges()`.

由於 `Activity` 有可能在配置變化時被銷燬，您必須在
`onDetachedFromActivityForConfigChanges()` 方法中
清理所有與 `Activity` 有關的參考，接著在 
`onReattachedToActivityForConfigChanges()` 中重新建立它們。

Finally, in `onDetachedFromActivity()` your plugin should clean
up all references related to `Activity` behavior and return to
a non-UI configuration.

最後，在 `onDetachedFromActivity()` 中清理所有與 `Activity`
有關的參考並返回與 UI 無關的配置。


[`ActivityAware`]: {{site.api}}/javadoc/io/flutter/embedding/engine/plugins/activity/ActivityAware.html
[Basic plugin]: #basic-plugin
[battery plus package]: {{site.github}}/fluttercommunity/plus_plugins/tree/main/packages/battery_plus/battery_plus
[Flutter plugins]: {{site.pub}}/flutter/packages
[`FlutterPlugin`]: {{site.api}}/javadoc/io/flutter/embedding/engine/plugins/FlutterPlugin.html
[`PluginRegistry.Registrar`]: {{site.api}}/javadoc/io/flutter/plugin/common/PluginRegistry.Registrar.html
[`PluginRegistry.Registrar.activity()`]: {{site.api}}/javadoc/io/flutter/plugin/common/PluginRegistry.Registrar.html#activity--
[`ServiceAware`]: {{site.api}}/javadoc/io/flutter/embedding/engine/plugins/service/ServiceAware.html
[Upgrading pre 1.12 Android projects]: {{site.github}}/flutter/flutter/wiki/Upgrading-pre-1.12-Android-projects
[verified publisher]: {{site.dart-site}}/tools/pub/verified-publishers
