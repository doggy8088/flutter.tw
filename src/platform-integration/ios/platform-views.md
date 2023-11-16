---
title: Hosting native iOS views in your Flutter app with Platform Views
title: "在 Flutter 應用中使用整合平臺視圖託管您的原生 iOS 檢視"
short-title: iOS platform-views
short-title: iOS 平臺視圖
description: Learn how to host native iOS views in your Flutter app with Platform Views.
description: 學習如何在 Flutter 應用中使用整合平臺視圖託管您的原生  iOS 檢視。
---

<?code-excerpt path-base="development/platform_integration"?>

Platform views allow you to embed native views in a Flutter app,
so you can apply transforms, clips, and opacity to the native view
from Dart.

整合平臺視圖 (後稱為平臺視圖) 允許將原生檢視嵌入到 Flutter 應用中，
所以您可以透過 Dart 將變換、裁剪和不透明度等效果應用到原生檢視。

This allows you, for example, to use the native
Google Maps from the Android and iOS SDKs
directly inside your Flutter app.

例如，這使您可以透過使用平臺視圖直接在 Flutter 應用內部
使用 Android 和 iOS SDK 中的 Google Maps。

{{site.alert.note}}

  This page discusses how to host your own native iOS views
  within a Flutter app.
  If you'd like to embed native Android views
  in your Flutter app,
  see [Hosting native Android views][].

  本篇文件討論瞭如何在您的 Flutter 應用中託管您的 iOS 原生檢視。
  如果你想了解如何嵌入到 Android 檢視中，閱讀這篇文件：
  [在 Flutter 應用中使用整合平臺視圖託管您的原生 Android 檢視][Hosting native Android views]。

{{site.alert.end}}

[Hosting native Android views]: {{site.url}}/platform-integration/android/platform-views


iOS only uses Hybrid composition,
which means that the native
`UIView` is appended to the view hierarchy.

iOS 只支援混合整合模式，
這意味著原生的 `UIView` 會被加入檢視層級中。

To create a platform view on iOS,
use the following instructions:

要在 iOS 中建立平臺視圖，需要如下步驟：

### On the Dart side

### 在 Dart 端

On the Dart side, create a `Widget`
and add the build implementation,
as shown in the following steps.

在 Dart 端，建立一個 `Widget`
並新增如下的實現，具體如下：

In the Dart widget file, make changes similar to those 
shown in `native_view_example.dart`:

在 Dart 檔案中，例如 `native_view_example.dart`，
請執行下列操作：

<ol markdown="1">
<li markdown="1">Add the following imports:

新增如下匯入程式碼：

<?code-excerpt "lib/platform_views/native_view_example_3.dart (import)"?>
```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/services.dart';
```
</li>

<li markdown="1">Implement a `build()` method:

實現 `build()` 方法：

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

更多資訊，請檢視 API 文件：[`UIKitView`][]。

[`UIKitView`]: {{site.api}}/flutter/widgets/UiKitView-class.html

### On the platform side

### 在平臺端

On the platform side, use either Swift or Objective-C:

在平臺端，您可以使用 Swift 或是 Objective-C：

{% samplecode ios-platform-views %}
{% sample Swift %}

Implement the factory and the platform view.
The `FLNativeViewFactory` creates the platform view,
and the platform view provides a reference to the `UIView`.
For example, `FLNativeView.swift`:

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

    /// Implementing this method is only necessary when the `arguments` in `createWithFrame` is not `nil`.
    public func createArgsCodec() -> FlutterMessageCodec & NSObjectProtocol {
          return FlutterStandardMessageCodec.sharedInstance()
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
`AppDelegate.swift`:

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

要在外掛中進行註冊，修改外掛的主類
(例如 `FLPlugin.swift`):

```swift
import Flutter
import UIKit

class FLPlugin: NSObject, FlutterPlugin {
    public static func register(with registrar: FlutterPluginRegistrar) {
        let factory = FLNativeViewFactory(messenger: registrar.messenger())
        registrar.register(factory, withId: "<platform-view-type>")
    }
}
```

{% sample Objective-C %}

In Objective-C, add the headers for the factory and the platform view.
For example, as shown in `FLNativeView.h`:

使用 Objective-C 時，你需要在工廠類和平臺視圖的檔案頭部新增以下內容。
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

/// Implementing this method is only necessary when the `arguments` in `createWithFrame` is not `nil`.
- (NSObject<FlutterMessageCodec>*)createArgsCodec {
    return [FlutterStandardMessageCodec sharedInstance];
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

For app registration,
modify the App's `AppDelegate.m`:

要在應用中進行註冊，
修改應用中的 `AppDelegate.m`：

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
modify the main plugin file
(for example, `FLPlugin.m`):

要在外掛中進行註冊，修改外掛主檔案
(例如 `FLPlugin.m`):

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

[`FlutterPlatformView`]: {{site.api}}/objcdoc/Protocols/FlutterPlatformView.html
[`FlutterPlatformViewFactory`]: {{site.api}}/objcdoc/Protocols/FlutterPlatformViewFactory.html
[`PlatformView`]: {{site.api}}/javadoc/io/flutter/plugin/platform/PlatformView.html

## Putting it together

## 整合起來

When implementing the `build()` method in Dart,
you can use [`defaultTargetPlatform`][]
to detect the platform, and decide which widget to use:

在 Dart 中實現 `build()` 方法時，
您可以使用 [`defaultTargetPlatform`][] 
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
Platform views in Flutter come with performance trade-offs.

For example, in a typical Flutter app, the Flutter UI is 
composed on a dedicated raster thread. 
This allows Flutter apps to be fast, 
as the main platform thread is rarely blocked.

When a platform view is rendered with hybrid composition, 
the Flutter UI is composed from the platform thread. 
The platform thread competes with other tasks 
like handling OS or plugin messages.

When an iOS PlatformView is on screen, the screen refresh rate is 
capped at 80fps to avoid rendering janks.

For complex cases, there are some techniques that can be used 
to mitigate performance issues.

For example, you could use a placeholder texture while an 
animation is happening in Dart. 
In other words, if an animation is slow while a platform view is rendered, 
then consider taking a screenshot of the native view and rendering it as a texture.

## Composition limitations
There are some limitations when composing iOS Platform Views.

- The [`ShaderMask`][] and [`ColorFiltered`][] widgets are not supported.
- The [`BackdropFilter`][] widget is supported, 
but there are some limitations on how it can be used. 
For more details, check out the [iOS Platform View Backdrop Filter Blur design doc][design-doc].

[`ShaderMask`]: {{site.api}}/flutter/foundation/ShaderMask.html
[`ColorFiltered`]: {{site.api}}/flutter/foundation/ColorFiltered.html
[`BackdropFilter`]: {{site.api}}/flutter/foundation/BackdropFilter.html
[`defaultTargetPlatform`]: {{site.api}}/flutter/foundation/defaultTargetPlatform.html
[design-doc]: {{site.main-url}}/go/ios-platformview-backdrop-filter-blur

