---
title: Hosting native Android and iOS views in your Flutter app with Platform Views
title: 在 Flutter 應用中使用整合平臺視圖託管您的原生 Android 和 iOS 檢視
short-title: Platform-views
description: Learn how to host native Android and iOS views in your Flutter app with Platform Views.
description: 學習如何在 Flutter 應用中使用整合平臺視圖託管您的原生 Android 和 iOS 檢視。
---

本頁面內容已根據平台分為幾個頁面，並將在後期移除，請根據需要導航至不同平台：
- [託管您的原生 Android 檢視][Hosting native Android views]
- [託管您的原生 iOS 檢視][Hosting native iOS views]

[Hosting native Android views]: {{site.url}}/development/platform-integration/android/platform-views
[Hosting native iOS views]: {{site.url}}/development/platform-integration/ios/platform-views

{% comment %}
<?code-excerpt path-base="development/platform_integration"?>

Platform views allow you to embed native views in a Flutter app, so
you can apply transforms, clips, and opacity to the native view
from Dart.

整合平臺視圖（後稱為平臺視圖）允許將原生檢視嵌入到 Flutter 應用中，
所以您可以透過 Dart 將變換、裁剪和不透明度等效果應用到原生檢視。

This allows you, for example, to use the native
Google Maps from the Android and iOS SDKs
directly inside your Flutter app, by using Platform Views.

例如，這使您可以透過使用平臺視圖直接在 Flutter 應用內部
使用 Android 和 iOS SDK 中的 Google Maps。

This page discusses how to host your own native views
within a Flutter app.

本篇文件討論瞭如何在您的 Flutter 應用中託管您的原生檢視。

## Android

Flutter supports two modes: Virtual displays and Hybrid composition.

Flutter 支援兩種整合模式：虛擬顯示模式 (Virtual displays) 和混合整合模式 (Hybrid composition) 。

Which one to use depends on the use case. Let's take a look:

我們應根據具體情況來決定使用哪種模式。讓我們來看看：

* Virtual displays renders the `android.view.View` instance to a texture,
  so it's not embedded within the Android Activity's view hierachy.
  Certain platform interactions such as keyboard handling, and accessibility
  features might not work.

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

To create a platform view on Android, follow these steps:

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
  [外掛遷移指南][plugin migration guide].

{{site.alert.end}}


#### Hybrid composition

#### 混合整合模式

In your Dart file, for example `native_view_example.dart`,
do the following:

在 Dart 檔案中，例如 `native_view_example.dart`，
請執行下列操作：

<ol markdown="1">
<li markdown="1">Add the following imports:

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

<li markdown="1">Implement a `build()` method:

   實現 `build` 方法：

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

更多資訊可以檢視下面的 API 文件：

* [`PlatformViewLink`][]
* [`AndroidViewSurface`][]
* [`PlatformViewsService`][]

#### Virtual Display

In your Dart file, for example `native_view_example.dart`,
do the following:

在 Dart 檔案中，例如 `native_view_example.dart`，
請執行下列操作：

<ol markdown="1">
<li markdown="1">Add the following imports:

   新增下面的匯入程式碼：

<?code-excerpt "lib/platform_views/native_view_example_2.dart (Import)"?>
```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
```
</li>

<li markdown="1">Implement a `build()` method:

   實現 `build` 方法：

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

### On the platform side

### 在平台（Android）端

On the platform side, use the standard
`io.flutter.plugin.platform` package in either Java or Kotlin:

在平臺端，使用 Java 或 Kotlin 中的標準包 `io.flutter.plugin.platform`：

{% samplecode android-platform-views %}
{% sample Kotlin %}

In your native code, implement the following:

在您的原生程式碼中，實現如下方法：

Extend `io.flutter.plugin.platform.PlatformView`
to provide a reference to the `android.view.View`,
For example `NativeView.kt`:

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
for example `NativeViewFactory.kt`:

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
This can be done in an app or a plugin.

最後，註冊這個平臺視圖。
這一步可以在應用中，也可以在外掛中。

For app registration, modify the app's main activity
(for example, `MainActivity.kt`):

要在應用中進行註冊，修改應用的主 Activity （例如：`MainActivity.kt`）：

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

For plugin registration, modify the plugin's main class
(for example, `PlatformViewPlugin.kt`):

要在外掛中進行註冊，修改您外掛的主類（例如：`PlatformViewPlugin.kt`）：

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

Create a factory class that creates an instance of the
`NativeView` created earlier,
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
This can be done in an app or a plugin.

最後，註冊這個平臺視圖。
這一步可以在應用中，也可以在外掛中。

For app registration, modify the app's main activity
(for example, `MainActivity.java`):

要在應用中進行註冊，修改應用的主 Activity
（例如：`MainActivity.java`）：

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

For plugin registration, modify the plugin's main file
(for example, `PlatformViewPlugin.java`):

要在外掛中進行註冊，修改外掛的主類（例如：`PlatformViewPlugin.java`）：

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

Finally, modify your `build.gradle` file to require one of the
minimal Android SDK versions:

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

## iOS

iOS only uses Hybrid composition,
which means that the native
`UIView` is appended to view hierarchy.

iOS 只支援混合整合模式，
這意味著原生的 `UIView` 會被加入檢視層級中。

To create a platform view on iOS, follow these steps:

要在 iOS 中建立平臺視圖，需要如下步驟：

### On the Dart side

### 在 Dart 端

On the Dart side, create a `Widget`
and add the following build implementation,
as shown in the following steps.

在 Dart 端，建立一個 `Widget`
並新增如下的實現，具體如下：

In your Dart file, for example
do the following in `native_view_example.dart`:

在 Dart 檔案中，例如 `native_view_example.dart`，
請執行下列操作：

<ol markdown="1">
<li markdown="1">Add the following imports:

   新增如下匯入程式碼：

<?code-excerpt "lib/platform_views/native_view_example_3.dart (Import)"?>
```dart
import 'package:flutter/widgets.dart';

class IOSCompositionWidget extends StatelessWidget {
  const IOSCompositionWidget({super.key});

  @override
  Widget build(BuildContext context) {
    // This is used in the platform side to register the view.
    const String viewType = '<platform-view-type>';
    // Pass parameters to the platform side.
    final Map<String, dynamic> creationParams = <String, dynamic>{};

    return UiKitView(
      viewType: viewType,
      layoutDirection: TextDirection.ltr,
      creationParams: creationParams,
      creationParamsCodec: const StandardMessageCodec(),
    );
  }
}

class TogetherWidget extends StatelessWidget {
  const TogetherWidget({super.key});

  @override
  Widget build(BuildContext context) {
    // This is used in the platform side to register the view.
    const String viewType = '<platform-view-type>';
    // Pass parameters to the platform side.
    final Map<String, dynamic> creationParams = <String, dynamic>{};

    switch (defaultTargetPlatform) {
      case TargetPlatform.android:
      // return widget on Android.
      case TargetPlatform.iOS:
      // return widget on iOS.
      default:
        throw UnsupportedError('Unsupported platform view');
    }
  }
}
```
</li>

<li markdown="1">Implement a `build()` method:

   實現 `build` 方法：

<?code-excerpt "lib/platform_views/native_view_example_3.dart (iOSCompositionWidget)"?>
```dart
Widget build(BuildContext context) {
  // This is used in the platform side to register the view.
  const String viewType = '<platform-view-type>';
  // Pass parameters to the platform side.
  final Map<String, dynamic> creationParams = <String, dynamic>{};

  return UiKitView(
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
[`UIKitView`][].

更多資訊，請檢視 API 文件：
[`UIKitView`][].

### On the platform side

### 在平台（iOS）端

On the platform side, you use the either Swift or Objective-C:

在平臺端，您可以使用 Swift 或是 Objective-C：

{% samplecode ios-platform-views %}
{% sample Swift %}

Implement the factory and the platform view.
The `FLNativeViewFactory` creates the platform view, and the platform view
provides a reference to the `UIView`. For example, `FLNativeView.swift`:

實現工廠和平臺視圖。
`FLNativeViewFactory` 建立一個關聯了 `UIView` 的平臺視圖。
舉個例子，`FLNativeView.swift`：

```swift
import Flutter
import UIKit

class FLNativeViewFactory: NSObject, FlutterPlatformViewFactory {
    private var messenger: FlutterBinaryMessenger

    init(messenger: FlutterBinaryMessenger) {
        self.messenger = messenger
        super.init()
    }

    func create(
        withFrame frame: CGRect,
        viewIdentifier viewId: Int64,
        arguments args: Any?
    ) -> FlutterPlatformView {
        return FLNativeView(
            frame: frame,
            viewIdentifier: viewId,
            arguments: args,
            binaryMessenger: messenger)
    }
}

class FLNativeView: NSObject, FlutterPlatformView {
    private var _view: UIView

    init(
        frame: CGRect,
        viewIdentifier viewId: Int64,
        arguments args: Any?,
        binaryMessenger messenger: FlutterBinaryMessenger?
    ) {
        _view = UIView()
        super.init()
        // iOS views can be created here
        createNativeView(view: _view)
    }

    func view() -> UIView {
        return _view
    }

    func createNativeView(view _view: UIView){
        _view.backgroundColor = UIColor.blue
        let nativeLabel = UILabel()
        nativeLabel.text = "Native text from iOS"
        nativeLabel.textColor = UIColor.white
        nativeLabel.textAlignment = .center
        nativeLabel.frame = CGRect(x: 0, y: 0, width: 180, height: 48.0)
        _view.addSubview(nativeLabel)
    }
}
```

Finally, register the platform view.
This can be done in an app or a plugin.

最後，註冊這個平臺視圖。
這一步可以在應用中，也可以在外掛中。

For app registration,
modify the App's `AppDelegate.swift`:

要在應用中進行註冊，修改應用中的
`AppDelegate.swift`：

```swift
import Flutter
import UIKit

@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
    override func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]?
    ) -> Bool {
        GeneratedPluginRegistrant.register(with: self)

        weak var registrar = self.registrar(forPlugin: "plugin-name")

        let factory = FLNativeViewFactory(messenger: registrar!.messenger())
        self.registrar(forPlugin: "<plugin-name>")!.register(
            factory,
            withId: "<platform-view-type>")
        return super.application(application, didFinishLaunchingWithOptions: launchOptions)
    }
}
```

For plugin registration,
modify the plugin's main file
(for example, `FLPlugin.swift`):

要在外掛中進行註冊，修改外掛的主類（例如 `FLPlugin.swift`）：

```swift
import Flutter
import UIKit

class FLPlugin: NSObject, FlutterPlugin {
    public static func register(with registrar: FlutterPluginRegistrar) {
        let factory = FLNativeViewFactory(messenger: registrar.messenger)
        registrar.register(factory, withId: "<platform-view-type>")
    }
}
```

{% sample Objective-C %}

Add the headers for the factory and the platform view.
For example, `FLNativeView.h`:

在工廠類和平臺視圖的檔案頭部新增以下內容。
用 `FLNativeView.h` 舉例：

```objc
#import <Flutter/Flutter.h>

@interface FLNativeViewFactory : NSObject <FlutterPlatformViewFactory>
- (instancetype)initWithMessenger:(NSObject<FlutterBinaryMessenger>*)messenger;
@end

@interface FLNativeView : NSObject <FlutterPlatformView>

- (instancetype)initWithFrame:(CGRect)frame
               viewIdentifier:(int64_t)viewId
                    arguments:(id _Nullable)args
              binaryMessenger:(NSObject<FlutterBinaryMessenger>*)messenger;

- (UIView*)view;
@end
```

Implement the factory and the platform view.
The `FLNativeViewFactory` creates the platform view,
and the platform view provides a reference to the
`UIView`. For example, `FLNativeView.m`:

實現工廠類和平臺視圖。
`FLNativeViewFactory` 建立一個關聯了 `UIView` 的平臺視圖。
用 `FLNativeView.m` 舉例：

```objc
#import "FLNativeView.h"

@implementation FLNativeViewFactory {
  NSObject<FlutterBinaryMessenger>* _messenger;
}

- (instancetype)initWithMessenger:(NSObject<FlutterBinaryMessenger>*)messenger {
  self = [super init];
  if (self) {
    _messenger = messenger;
  }
  return self;
}

- (NSObject<FlutterPlatformView>*)createWithFrame:(CGRect)frame
                                   viewIdentifier:(int64_t)viewId
                                        arguments:(id _Nullable)args {
  return [[FLNativeView alloc] initWithFrame:frame
                              viewIdentifier:viewId
                                   arguments:args
                             binaryMessenger:_messenger];
}

@end

@implementation FLNativeView {
   UIView *_view;
}

- (instancetype)initWithFrame:(CGRect)frame
               viewIdentifier:(int64_t)viewId
                    arguments:(id _Nullable)args
              binaryMessenger:(NSObject<FlutterBinaryMessenger>*)messenger {
  if (self = [super init]) {
    _view = [[UIView alloc] init];
  }
  return self;
}

- (UIView*)view {
  return _view;
}

@end
```

Finally, register the platform view.
This can be done in an app or a plugin.

最後，註冊這個平臺視圖。
這一步可以在應用中，也可以在外掛中。

For app registration, modify the App's `AppDelegate.m`:

要在應用中進行註冊，修改應用中的 `AppDelegate.m`：

```objc
#import "AppDelegate.h"
#import "FLNativeView.h"
#import "GeneratedPluginRegistrant.h"

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application
    didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  [GeneratedPluginRegistrant registerWithRegistry:self];

   NSObject<FlutterPluginRegistrar>* registrar =
      [self registrarForPlugin:@"plugin-name"];

  FLNativeViewFactory* factory =
      [[FLNativeViewFactory alloc] initWithMessenger:registrar.messenger];

  [[self registrarForPlugin:@"<plugin-name>"] registerViewFactory:factory
                                                          withId:@"<platform-view-type>"];
  return [super application:application didFinishLaunchingWithOptions:launchOptions];
}

@end
```

For plugin registration,
modify the main plugin file (for example, `FLPlugin.m`):

要在外掛中進行註冊，修改外掛主檔案（例如 `FLPlugin.m`）：

```objc
#import <Flutter/Flutter.h>
#import "FLNativeView.h"

@interface FLPlugin : NSObject<FlutterPlugin>
@end

@implementation FLPlugin

+ (void)registerWithRegistrar:(NSObject<FlutterPluginRegistrar>*)registrar {
  FLNativeViewFactory* factory =
      [[FLNativeViewFactory alloc] initWithMessenger:registrar.messenger];
  [registrar registerViewFactory:factory withId:@"<platform-view-type>"];
}

@end
```

{% endsamplecode %}

For more information, see the API docs for:

更多資訊，請檢視 API 文件：

* [`FlutterPlatformViewFactory`][]
* [`FlutterPlatformView`][]
* [`PlatformView`][]

## Putting it together

## 整合

When implementing the `build()` method in Dart,
you can use [`defaultTargetPlatform`][]
to detect the platform, and decide what widget to use:

在 Dart 中實現 `build()` 方法時，您可以使用 [`defaultTargetPlatform`][] 
來檢測當前的平台，並且決定如何使用這個 widget：

<?code-excerpt "lib/platform_views/native_view_example_3.dart (TogetherWidget)"?>
```dart
Widget build(BuildContext context) {
  // This is used in the platform side to register the view.
  const String viewType = '<platform-view-type>';
  // Pass parameters to the platform side.
  final Map<String, dynamic> creationParams = <String, dynamic>{};

  switch (defaultTargetPlatform) {
    case TargetPlatform.android:
    // return widget on Android.
    case TargetPlatform.iOS:
    // return widget on iOS.
    default:
      throw UnsupportedError('Unsupported platform view');
  }
}
```

## Performance

## 效能

Platform views in Flutter come with performance trade-offs.

在 Flutter 中使用平臺視圖時，效能會有所折衷。

For example, in a typical Flutter app, the Flutter UI is composed
on a dedicated raster thread. This allows Flutter apps to be fast,
as the main platform thread is rarely blocked.

例如，在典型的 Flutter 應用中，Flutter UI 是在專用網格執行緒上組成的。
由於平台的主執行緒很少被阻塞，因此 Flutter 應用程式可以快速執行。

While a platform view is rendered with Hybrid composition, the Flutter
UI is composed from the platform thread, which competes with other
tasks like handling OS or plugin messages.

使用混合整合模式渲染平臺視圖時，
Flutter UI 由平台執行緒完成，與其他執行緒一起競爭，
例如：處理系統或外掛訊息等任務。

Prior to Android 10, Hybrid composition copies each Flutter frame
out of the graphic memory into main memory, and then copies it back
to a GPU texture. In Android 10 or above, the graphics memory is
copied twice. As this copy happens per frame, the performance of
the entire Flutter UI may be impacted.

在 Android 10 之前，
混合整合模式將每個 Flutter 幀從視訊記憶體中複製到主記憶體中，
然後再將其複製回 GPU 紋理中。
在 Android 10 或更高版本中，視訊記憶體會被複制兩次。
由於每幀都會進行一次複製，因此可能會影響整個 Flutter UI 的效能。

Virtual display, on the other hand, makes each pixel of the native view
flow through additional intermediate graphic buffers, which cost graphic
memory and drawing performance.

另一方面，虛擬顯示模式使平臺視圖的每個畫素
流經附加的中間圖形緩衝區，這會浪費視訊記憶體和繪圖效能。

For complex cases, there are some techniques that can be used to mitigate
these issues.

對於複雜的情況，可以使用一些技巧來緩解這些問題。

For example, you could use a placeholder texture while an animation is
happening in Dart. In other words, if an animation is slow while a
platform view is rendered, then consider taking a screenshot of the
native view and rendering it as a texture.

例如，當 Dart 中發生動畫時，您可以使用佔位符紋理。
換句話說，如果在渲染平臺視圖時動畫很慢，
請考慮對原生檢視進行截圖，並將其渲染為紋理。

For more information, see:

更多訊息，檢視：

* [`TextureLayer`][]
* [`TextureRegistry`][]
* [`FlutterTextureRegistry`][]

[`AndroidView`]: {{site.api}}/flutter/widgets/AndroidView-class.html
[`AndroidViewSurface`]: {{site.api}}/flutter/widgets/AndroidViewSurface-class.html
[`defaultTargetPlatform`]: {{site.api}}/flutter/foundation/defaultTargetPlatform.html
[`FlutterPlatformView`]: {{site.api}}/objcdoc/Protocols/FlutterPlatformView.html
[`FlutterPlatformViewFactory`]: {{site.api}}/objcdoc/Protocols/FlutterPlatformViewFactory.html
[`FlutterPlugin`]: {{site.api}}/javadoc/io/flutter/embedding/engine/plugins/FlutterPlugin.html
[`FlutterTextureRegistry`]: {{site.api}}/objcdoc/Protocols/FlutterTextureRegistry.html
[performance]: #performance
[plugin migration guide]: {{site.url}}/development/packages-and-plugins/plugin-api-migration
[`PlatformView`]: {{site.api}}/javadoc/io/flutter/plugin/platform/PlatformView.html
[`PlatformViewFactory`]: {{site.api}}/javadoc/io/flutter/plugin/platform/PlatformViewFactory.html
[`PlatformViewLink`]: {{site.api}}/flutter/widgets/PlatformViewLink-class.html
[`PlatformViewRegistry`]: {{site.api}}/javadoc/io/flutter/plugin/platform/PlatformViewRegistry.html
[`PlatformViewsService`]: {{site.api}}/flutter/services/PlatformViewsService-class.html
[`UIKitView`]: {{site.api}}/flutter/widgets/UiKitView-class.html
[`TextureLayer`]: {{site.api}}/flutter/rendering/TextureLayer-class.html
[`TextureRegistry`]: {{site.api}}/javadoc/io/flutter/view/TextureRegistry.html
[version 1.22.2]: {{site.repo.flutter}}/wiki/Hotfixes-to-the-Stable-Channel#1222--october-16-2020

{% endcomment %}