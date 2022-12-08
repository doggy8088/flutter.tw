---
title: Hosting native Android views in your Flutter app with Platform Views
title: "在 Flutter 應用中使用整合平臺視圖託管您的原生 Android 檢視"
short-title: Android platform-views
short-title: Android 平臺視圖
description: Learn how to host native Android views in your Flutter app with Platform Views.
description: 學習如何在 Flutter 應用中使用整合平臺視圖託管您的原生 Android 檢視。
---

<?code-excerpt path-base="development/platform_integration"?>

Platform views allow you to embed native views in a Flutter app,
so you can apply transforms, clips, and opacity to the native view
from Dart.

整合平臺視圖（後稱為平臺視圖）允許將原生檢視嵌入到 Flutter 應用中，
所以您可以透過 Dart 將變換、裁剪和不透明度等效果應用到原生檢視。

This allows you, for example, to use the native
Google Maps from the Android SDK
directly inside your Flutter app.

例如，這使您可以透過使用平臺視圖直接在 Flutter 應用內部
使用 Android 和 iOS SDK 中的 Google Maps。

{{site.alert.note}}

  This page discusses how to host your own native views
  within a Flutter app.
  If you'd like to embed native iOS views in your Flutter app,
  see [Hosting native iOS views][].

  本篇文件討論瞭如何在您的 Flutter 應用中託管您的原生檢視。
  如果你想了解如何嵌入到 iOS 檢視中，閱讀這篇文件：
  [在 Flutter 應用中使用整合平臺視圖託管您的原生 iOS 檢視][Hosting native iOS views]。

{{site.alert.end}}

[Hosting native iOS views]: {{site.url}}/development/platform-integration/ios/platform-views

Flutter supports two modes:
Virtual displays and Hybrid composition.

Flutter 支援兩種整合模式：虛擬顯示模式 (Virtual displays)
和混合整合模式 (Hybrid composition) 。

Which one to use depends on the use case.
Let's take a look:

我們應根據具體情況來決定使用哪種模式。讓我們來看看：

* Virtual displays render the `android.view.View`
  instance to a texture, so it's not embedded within
  the Android Activity's view hierachy.
  Certain platform interactions such as keyboard handling
  and accessibility features might not work.

  虛擬顯示模式會將 `android.view.View` 例項渲染為紋理，
  因此它不會嵌入到 Android Activity 的檢視層次結構中。
  某些平台互動（例如鍵盤處理和輔助功能）可能無法正常工作。

* Hybrid composition appends the native `android.view.View`
  to the view hierarchy. Therefore, keyboard
  handling, and accessibility work out of the box.
  Prior to Android 10, this mode might significantly
  reduce the frame throughput (FPS) of the
  Flutter UI. See [performance][] for more info.

  混合整合模式會將原生的 `android.view.View` 附加到檢視層次結構中。
  因此，鍵盤處理和無障礙功能是開箱即用的。
  在 Android 10 之前，此模式可能會大大降低 Flutter UI 的幀吞吐量 (FPS)。
  有關更多資訊，請參見 [效能][performance] 小節。

[performance]: #performance

To create a platform view on Android,
use the following steps:

在 Android 上建立平臺視圖需要如下的步驟：

### On the Dart side

### 在 Dart 中進行的處理

On the Dart side, create a `Widget`
and add the following build implementation:

在 Dart 端，建立一個 `Widget`
然後新增如下的實現，具體如下：

{{site.alert.warning}}

  For this to work, your plugin or app must use
  Android embedding v2.
  If you haven't updated your plugin, see the
  [plugin migration guide][].

  您的外掛或應用必須使用 Android embedding v2 以確保平臺視圖可用。
  如果您還沒有更新您的外掛，檢視
  [外掛遷移指南][plugin migration guide]。

{{site.alert.end}}

[plugin migration guide]: {{site.url}}/development/platform-integration/android/plugin-api-migration

In your Dart file,
for example `native_view_example.dart`,
use the following instructions:

在 Dart 檔案中，例如 `native_view_example.dart`，
請執行下列操作：

<ol markdown="1">
<li markdown="1">
Add the following imports:

新增下面的匯入程式碼：

<?code-excerpt "lib/platform_views/native_view_example_1.dart (Import)"?>
```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/gestures.dart';
import 'package:flutter/material.dart';
import 'package:flutter/rendering.dart';
import 'package:flutter/services.dart';
```
</li>

<li markdown="1">
Implement a `build()` method:

實現 `build()` 方法：

<?code-excerpt "lib/platform_views/native_view_example_1.dart (HybridCompositionWidget)"?>
```dart
Widget build(BuildContext context) {
  // This is used in the platform side to register the view.
  const String viewType = '<platform-view-type>';
  // Pass parameters to the platform side.
  const Map<String, dynamic> creationParams = <String, dynamic>{};

  return PlatformViewLink(
    viewType: viewType,
    surfaceFactory:
        (context, controller) {
      return AndroidViewSurface(
        controller: controller as AndroidViewController,
        gestureRecognizers: const <Factory<OneSequenceGestureRecognizer>>{},
        hitTestBehavior: PlatformViewHitTestBehavior.opaque,
      );
    },
    onCreatePlatformView: (params) {
      return PlatformViewsService.initSurfaceAndroidView(
        id: params.id,
        viewType: viewType,
        layoutDirection: TextDirection.ltr,
        creationParams: creationParams,
        creationParamsCodec: const StandardMessageCodec(),
        onFocus: () {
          params.onFocusChanged(true);
        },
      )
        ..addOnPlatformViewCreatedListener(params.onPlatformViewCreated)
        ..create();
    },
  );
}
```
</li>
</ol>

For more information, see the API docs for:

更多資訊，查閱 API 文件：

* [`PlatformViewLink`][]
* [`AndroidViewSurface`][]
* [`PlatformViewsService`][]

[`AndroidViewSurface`]: {{site.api}}/flutter/widgets/AndroidViewSurface-class.html
[`PlatformViewLink`]: {{site.api}}/flutter/widgets/PlatformViewLink-class.html
[`PlatformViewsService`]: {{site.api}}/flutter/services/PlatformViewsService-class.html

#### Virtual Display

#### 虛擬顯示模式 (Virtual Display)

In your Dart file,
for example `native_view_example.dart`,
use the following instructions:

在 Dart 檔案中，例如 `native_view_example.dart`，
請執行下列操作：

<ol markdown="1">
<li markdown="1">
Add the following imports:

新增下面的匯入程式碼：

<?code-excerpt "lib/platform_views/native_view_example_2.dart (Import)"?>
```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
```
</li>

<li markdown="1">
Implement a `build()` method:

實現 `build()` 方法：

<?code-excerpt "lib/platform_views/native_view_example_2.dart (VirtualDisplayWidget)"?>
```dart
Widget build(BuildContext context) {
  // This is used in the platform side to register the view.
  const String viewType = '<platform-view-type>';
  // Pass parameters to the platform side.
  final Map<String, dynamic> creationParams = <String, dynamic>{};

  return AndroidView(
    viewType: viewType,
    layoutDirection: TextDirection.ltr,
    creationParams: creationParams,
    creationParamsCodec: const StandardMessageCodec(),
  );
}
```
</li>
</ol>

For more information, see the API docs for:

更多資訊，查閱 API 文件：

* [`AndroidView`][]

[`AndroidView`]: {{site.api}}/flutter/widgets/AndroidView-class.html

### On the platform side

### 在平臺端

On the platform side, use the standard
`io.flutter.plugin.platform` package
in either Java or Kotlin:

在平臺端，使用 Java 或 Kotlin 中的標準包
`io.flutter.plugin.platform`：

{% samplecode android-platform-views %}
{% sample Kotlin %}

In your native code, implement the following:

在您的原生程式碼中，實現如下方法：

Extend `io.flutter.plugin.platform.PlatformView`
to provide a reference to the `android.view.View`,
For example, `NativeView.kt`:

繼承 `io.flutter.plugin.platform.PlatformView`
以提供對 `android.view.View` 的參考，
如 `NativeView.kt` 所示：

```kotlin
package dev.flutter.example

import android.content.Context
import android.graphics.Color
import android.view.View
import android.widget.TextView
import io.flutter.plugin.platform.PlatformView

internal class NativeView(context: Context, id: Int, creationParams: Map<String?, Any?>?) : PlatformView {
    private val textView: TextView

    override fun getView(): View {
        return textView
    }

    override fun dispose() {}

    init {
        textView = TextView(context)
        textView.textSize = 72f
        textView.setBackgroundColor(Color.rgb(255, 255, 255))
        textView.text = "Rendered on a native Android view (id: $id)"
    }
}
```

Create a factory class that creates an instance of the
`NativeView` created earlier,
for example, `NativeViewFactory.kt`:

建立一個用來建立 `NativeView` 的例項的工廠類，
參考 `NativeViewFactory.kt`：

```kotlin
package dev.flutter.example

import android.content.Context
import android.view.View
import io.flutter.plugin.common.StandardMessageCodec
import io.flutter.plugin.platform.PlatformView
import io.flutter.plugin.platform.PlatformViewFactory

class NativeViewFactory : PlatformViewFactory(StandardMessageCodec.INSTANCE) {
    override fun create(context: Context, viewId: Int, args: Any?): PlatformView {
        val creationParams = args as Map<String?, Any?>?
        return NativeView(context, viewId, creationParams)
    }
}
```

Finally, register the platform view.
You can do this in an app or a plugin.

最後，註冊這個平臺視圖。
這一步可以在應用中，也可以在外掛中。

For app registration,
modify the app's main activity
(for example, `MainActivity.kt`):

要在應用中進行註冊，修改應用的主 Activity
(例如：`MainActivity.kt`)：

```kotlin
package dev.flutter.example

import io.flutter.embedding.android.FlutterActivity
import io.flutter.embedding.engine.FlutterEngine

class MainActivity : FlutterActivity() {
    override fun configureFlutterEngine(flutterEngine: FlutterEngine) {
        flutterEngine
                .platformViewsController
                .registry
                .registerViewFactory("<platform-view-type>", NativeViewFactory())
    }
}
```

For plugin registration,
modify the plugin's main class
(for example, `PlatformViewPlugin.kt`):

要在外掛中進行註冊，修改您外掛的主類 
(例如：`PlatformViewPlugin.kt`)：

```kotlin
package dev.flutter.plugin.example

import io.flutter.embedding.engine.plugins.FlutterPlugin
import io.flutter.embedding.engine.plugins.FlutterPlugin.FlutterPluginBinding

class PlatformViewPlugin : FlutterPlugin {
    override fun onAttachedToEngine(binding: FlutterPluginBinding) {
        binding
                .platformViewRegistry
                .registerViewFactory("<platform-view-type>", NativeViewFactory())
    }

    override fun onDetachedFromEngine(binding: FlutterPluginBinding) {}
}
```

{% sample Java %}

In your native code, implement the following:

在您的原生程式碼中，實現如下方法：

Extend `io.flutter.plugin.platform.PlatformView`
to provide a reference to the `android.view.View`,
For example, `NativeView.java`:

繼承 `io.flutter.plugin.platform.PlatformView`
以提供對 `android.view.View` 的參考，
如 `NativeView.java` 所示：

```java
package dev.flutter.example;

import android.content.Context;
import android.graphics.Color;
import android.view.View;
import android.widget.TextView;
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import io.flutter.plugin.platform.PlatformView;
import java.util.Map;

class NativeView implements PlatformView {
   @NonNull private final TextView textView;

    NativeView(@NonNull Context context, int id, @Nullable Map<String, Object> creationParams) {
        textView = new TextView(context);
        textView.setTextSize(72);
        textView.setBackgroundColor(Color.rgb(255, 255, 255));
        textView.setText("Rendered on a native Android view (id: " + id + ")");
    }

    @NonNull
    @Override
    public View getView() {
        return textView;
    }

    @Override
    public void dispose() {}
}
```

Create a factory class that creates an
instance of the `NativeView` created earlier,
for example, `NativeViewFactory.java`:

建立一個用來建立 `NativeView` 的例項的工廠類，
參考 `NativeViewFactory.java`：

```java
package dev.flutter.example;

import android.content.Context;
import androidx.annotation.Nullable;
import androidx.annotation.NonNull;
import io.flutter.plugin.common.StandardMessageCodec;
import io.flutter.plugin.platform.PlatformView;
import io.flutter.plugin.platform.PlatformViewFactory;
import java.util.Map;

class NativeViewFactory extends PlatformViewFactory {

  NativeViewFactory() {
    super(StandardMessageCodec.INSTANCE);
  }

  @NonNull
  @Override
  public PlatformView create(@NonNull Context context, int id, @Nullable Object args) {
    final Map<String, Object> creationParams = (Map<String, Object>) args;
    return new NativeView(context, id, creationParams);
  }
}
```

Finally, register the platform view.
You can do this in an app or a plugin.

最後，註冊這個平臺視圖。
這一步可以在應用中，也可以在外掛中。

For app registration,
modify the app's main activity
(for example, `MainActivity.java`):

要在應用中進行註冊，修改應用的主 Activity
(例如：`MainActivity.java`):

```java
package dev.flutter.example;

import androidx.annotation.NonNull;
import io.flutter.embedding.android.FlutterActivity;
import io.flutter.embedding.engine.FlutterEngine;

public class MainActivity extends FlutterActivity {
    @Override
    public void configureFlutterEngine(@NonNull FlutterEngine flutterEngine) {
        flutterEngine
            .getPlatformViewsController()
            .getRegistry()
            .registerViewFactory("<platform-view-type>", new NativeViewFactory());
    }
}
```

For plugin registration,
modify the plugin's main file
(for example, `PlatformViewPlugin.java`):

要在外掛中進行註冊，
修改外掛的主類 (例如：`PlatformViewPlugin.java`):

```java
package dev.flutter.plugin.example;

import androidx.annotation.NonNull;
import io.flutter.embedding.engine.plugins.FlutterPlugin;

public class PlatformViewPlugin implements FlutterPlugin {
  @Override
  public void onAttachedToEngine(@NonNull FlutterPluginBinding binding) {
    binding
        .getPlatformViewRegistry()
        .registerViewFactory("<platform-view-type>", new NativeViewFactory());
  }

  @Override
  public void onDetachedFromEngine(@NonNull FlutterPluginBinding binding) {}
}
```
{% endsamplecode %}

For more information, see the API docs for:

更多資訊，請檢視 API 文件：

* [`FlutterPlugin`][]
* [`PlatformViewRegistry`][]
* [`PlatformViewFactory`][]
* [`PlatformView`][]

[`FlutterPlugin`]: {{site.api}}/javadoc/io/flutter/embedding/engine/plugins/FlutterPlugin.html
[`PlatformView`]: {{site.api}}/javadoc/io/flutter/plugin/platform/PlatformView.html
[`PlatformViewFactory`]: {{site.api}}/javadoc/io/flutter/plugin/platform/PlatformViewFactory.html
[`PlatformViewRegistry`]: {{site.api}}/javadoc/io/flutter/plugin/platform/PlatformViewRegistry.html

Finally, modify your `build.gradle` file
to require one of the minimal Android SDK versions:

最後，修改您的 `build.gradle` 檔案
來滿足 Android SDK 最低版本的要求：

```gradle
android {
    defaultConfig {
        minSdkVersion 19 // if using Hybrid composition.
        minSdkVersion 20 // if using Virtual display.
    }
}
```

[`AndroidViewSurface`]: {{site.api}}/flutter/widgets/AndroidViewSurface-class.html

{% include docs/platform-view-perf.md %}

