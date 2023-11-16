---
title: Writing custom platform-specific code
title: 撰寫雙端平台程式碼（外掛編寫實現）
short-title: Platform-specific code
short-title: 平台相關程式碼
description: Learn how to write custom platform-specific code in your app.
description: 學習如何撰寫原生實現的程式碼。
tags: 平台整合
keywords: Android,iOS,平台程式碼
---

<?code-excerpt path-base="development/platform_integration"?>

This guide describes how to write custom platform-specific code.
Some platform-specific functionality is available
through existing packages;
see [using packages][].

本指南介紹瞭如何編寫自訂的平台相關程式碼，
某些平台相關功能可透過已有的軟體包獲得，具體細節可檢視：
[在 Flutter 裡使用 Packages][using packages]。

[using packages]: {{site.url}}/packages-and-plugins/using-packages

{{site.alert.note}}

  The information in this page is valid for most platforms,
  but platform-specific code for the web generally uses
  [JS interoperability][] or the [`dart:html` library][] instead.

  本頁面的內容適用於大多數平台，
  但 Web 外掛的實現一般都是透過
  [JS 互動][JS interoperability] 或者
  [`dart:html` 庫][`dart:html` library] 實現的。

{{site.alert.end}}

Flutter uses a flexible system that allows you to call
platform-specific APIs in a language that works directly
with those APIs:

Flutter 建構了一個靈活的系統，
你可以呼叫各種相關平台的 API，具體如下所示：

* Kotlin or Java on Android
  
  Android 中的 Java 或 Kotlin API

* Swift or Objective-C on iOS

  iOS 中的 Objective-C 或 Swift API

* C++ on Windows

  Windows 作業系統中的 C++ API

* Objective-C on macOS

  macOS 系統中的 Objective-C

* C on Linux

  Linux 作業系統中的 C

Flutter's builtin platform-specific API support
doesn't rely on code generation,
but rather on a flexible message passing style.
Alternatively, you can use the [Pigeon][pigeon]
package for [sending structured typesafe messages][]
with code generation:

Flutter 內建的平台特定 API 支援不依賴於任何產生程式碼，而是靈活的依賴於傳遞訊息格式。
或者，你也可以使用 [Pigeon][] 這個 package，透過產生程式碼來
[傳送結構化型別安全訊息][sending structured typesafe messages]。

* The Flutter portion of the app sends messages to its _host_,
  the non-Dart portion of the app, over a platform channel.

  應用中的 Flutter 部分透過平台通道向其宿主 (非 Dart 部分) 傳送訊息。

* The _host_ listens on the platform channel, and receives the message.
  It then calls into any number of platform-specific APIs&mdash;using
  the native programming language&mdash;and sends a response back to the
  _client_, the Flutter portion of the app.

  **宿主**監聽平台通道並接收訊息。然後，它使用原生程式語言來呼叫任意數量的相關平台
  API，並將響應傳送回**客戶端**（即應用程式中的 Flutter 部分）。

{{site.alert.note}}

  This guide addresses using the platform channel mechanism
  if you need to use the platform's APIs in a non-Dart language.
  But you can also write platform-specific Dart code
  in your Flutter app by inspecting the
  [`defaultTargetPlatform`][] property.
  [Platform adaptations][] lists some
  platform-specific adaptations that Flutter
  automatically performs for you in the framework.

  本篇課程主要介紹如何在非 Dart 語言中，利用平台通道的機制呼叫平台 API。
  但是當你在 Flutter 應用裡編寫 Dart 程式碼時，你也可以透過判斷 [`defaultTargetPlatform`][]，
  在不同的平臺上執行對應的程式碼。
  [不同平台操作體驗的差異和適配][Platform adaptations] 文件中列舉了部分
  Flutter 框架自動為你處理的平台適配行為。
  
{{site.alert.end}}

[`defaultTargetPlatform`]: {{site.api}}/flutter/foundation/defaultTargetPlatform.html
[pigeon]: {{site.pub-pkg}}/pigeon

## Architectural overview: platform channels {#architecture}

## 架構概述：平台通道 {#architecture}

Messages are passed between the client (UI)
and host (platform) using platform
channels as illustrated in this diagram:

訊息使用平台通道在客戶端（UI）和宿主（平台）之間傳遞，如下圖所示：

![Platform channels architecture]({{site.url}}/assets/images/docs/PlatformChannels.png){:width="100%"}

Messages and responses are passed asynchronously,
to ensure the user interface remains responsive.

訊息和響應以非同步的形式進行傳遞，以確保使用者介面能夠保持響應。

{{site.alert.note}}

  Even though Flutter sends messages to and from Dart asynchronously,
  whenever you invoke a channel method, you must invoke that method on the
  platform's main thread. See the [section on threading][]
  for more information.
  
  Flutter 是透過 Dart 非同步傳送訊息的。
  即便如此，當你呼叫一個平台方法時，也需要在主執行緒上做呼叫。
  在 [這裡][section on threading] 檢視更多。
  
{{site.alert.end}}

On the client side, [`MethodChannel`][] enables sending
messages that correspond to method calls. On the platform side,
`MethodChannel` on Android ([`MethodChannelAndroid`][]) and
`FlutterMethodChannel` on iOS ([`MethodChanneliOS`][])
enable receiving method calls and sending back a
result. These classes allow you to develop a platform plugin
with very little 'boilerplate' code.

客戶端做方法呼叫的時候 [`MethodChannel`][] 會負責響應，
從平台一側來講，Android 系統上使用 [`MethodChannelAndroid`][]、
iOS 系統使用 [`MethodChanneliOS`][] 來
接收和返回來自 `MethodChannel` 的方法呼叫。
在開發平台外掛的時候，可以減少樣板程式碼。

{{site.alert.note}}

  If desired, method calls can also be sent in the reverse direction,
  with the platform acting as client to methods implemented in Dart.
  For a concrete example, check out the [`quick_actions`][] plugin.

  如果需要，方法呼叫也可以反向傳送，
  由平台充當客戶端來呼叫 Dart 實現的方法。
  一個具體的例子是 [`quick_actions`][] 外掛。

{{site.alert.end}}


### Platform channel data types support and codecs {#codec}

### 平台通道資料型別及編解碼器 {#codec}

The standard platform channels use a standard message codec that supports
efficient binary serialization of simple JSON-like values, such as booleans,
numbers, Strings, byte buffers, and Lists and Maps of these
(see [`StandardMessageCodec`][] for details).
The serialization and deserialization of these values to and from
messages happens automatically when you send and receive values.

標準平台通道使用標準訊息編解碼器，它支援簡單的類似 JSON
值的高效二進位制序列化，例如布林值、數字、字串、位元組緩衝區及這些型別的列表和對映
（詳情請參閱 [`StandardMessageCodec`][]）。
當你傳送和接收值時，它會自動對這些值進行序列化和反序列化。

The following table shows how Dart values are received on the
platform side and vice versa:

下表展示瞭如何在平臺端接收 Dart 值，反之亦然：

{% samplecode type-mappings %}
{% sample Java %}
| Dart                       | Java                |
| -------------------------- | ------------------- |
| null                       | null                |
| bool                       | java.lang.Boolean   |
| int                        | java.lang.Integer   |
| int, if 32 bits not enough | java.lang.Long      |
| double                     | java.lang.Double    |
| String                     | java.lang.String    |
| Uint8List                  | byte[]              |
| Int32List                  | int[]               |
| Int64List                  | long[]              |
| Float32List                | float[]             |
| Float64List                | double[]            |
| List                       | java.util.ArrayList |
| Map                        | java.util.HashMap   |

{% sample Kotlin %}
| Dart                       | Kotlin      |
| -------------------------- | ----------- |
| null                       | null        |
| bool                       | Boolean     |
| int                        | Int         |
| int, if 32 bits not enough | Long        |
| double                     | Double      |
| String                     | String      |
| Uint8List                  | ByteArray   |
| Int32List                  | IntArray    |
| Int64List                  | LongArray   |
| Float32List                | FloatArray  |
| Float64List                | DoubleArray |
| List                       | List        |
| Map                        | HashMap     |

{% sample Obj-C %}
| Dart                       | Objective-C                                    |
| -------------------------- | ---------------------------------------------- |
| null                       | nil (NSNull when nested)                       |
| bool                       | NSNumber numberWithBool:                       |
| int                        | NSNumber numberWithInt:                        |
| int, if 32 bits not enough | NSNumber numberWithLong:                       |
| double                     | NSNumber numberWithDouble:                     |
| String                     | NSString                                       |
| Uint8List                  | FlutterStandardTypedData typedDataWithBytes:   |
| Int32List                  | FlutterStandardTypedData typedDataWithInt32:   |
| Int64List                  | FlutterStandardTypedData typedDataWithInt64:   |
| Float32List                | FlutterStandardTypedData typedDataWithFloat32: |
| Float64List                | FlutterStandardTypedData typedDataWithFloat64: |
| List                       | NSArray                                        |
| Map                        | NSDictionary                                   |

{% sample Swift %}
| Dart                       | Swift                                   |
| -------------------------- | --------------------------------------- |
| null                       | nil                                     |
| bool                       | NSNumber(value: Bool)                   |
| int                        | NSNumber(value: Int32)                  |
| int, if 32 bits not enough | NSNumber(value: Int)                    |
| double                     | NSNumber(value: Double)                 |
| String                     | String                                  |
| Uint8List                  | FlutterStandardTypedData(bytes: Data)   |
| Int32List                  | FlutterStandardTypedData(int32: Data)   |
| Int64List                  | FlutterStandardTypedData(int64: Data)   |
| Float32List                | FlutterStandardTypedData(float32: Data) |
| Float64List                | FlutterStandardTypedData(float64: Data) |
| List                       | Array                                   |
| Map                        | Dictionary                              |

{% sample C++ %}
| Dart                       | C++                                                      |
| -------------------------- | -------------------------------------------------------- |
| null                       | EncodableValue()                                         |
| bool                       | EncodableValue(bool)                                     |
| int                        | EncodableValue(int32_t)                                  |
| int, if 32 bits not enough | EncodableValue(int64_t)                                  |
| double                     | EncodableValue(double)                                   |
| String                     | EncodableValue(std::string)                              |
| Uint8List                  | EncodableValue(std::vector<uint8_t>)                     |
| Int32List                  | EncodableValue(std::vector<int32_t>)                     |
| Int64List                  | EncodableValue(std::vector<int64_t>)                     |
| Float32List                | EncodableValue(std::vector<float>)                       |
| Float64List                | EncodableValue(std::vector<double>)                      |
| List                       | EncodableValue(std::vector<EncodableValue>)              |
| Map                        | EncodableValue(std::map<EncodableValue, EncodableValue>) |

{% sample C %}
| Dart                       | C (GObject)               |
| -------------------------- | ------------------------- |
| null                       | FlValue()                 |
| bool                       | FlValue(bool)             |
| int                        | FlValue(int64_t)          |
| double                     | FlValue(double)           |
| String                     | FlValue(gchar*)           |
| Uint8List                  | FlValue(uint8_t*)         |
| Int32List                  | FlValue(int32_t*)         |
| Int64List                  | FlValue(int64_t*)         |
| Float32List                | FlValue(float*)           |
| Float64List                | FlValue(double*)          |
| List                       | FlValue(FlValue)          |
| Map                        | FlValue(FlValue, FlValue) |
{% endsamplecode %}

## Example: Calling platform-specific code using platform channels {#example}

## 範例: 透過平台通道呼叫平台的 iOS、Android 和 Windows 程式碼 {#example}

The following code demonstrates how to call
a platform-specific API to retrieve and display
the current battery level.  It uses
the Android `BatteryManager` API,
the iOS `device.batteryLevel` API,
the Windows `GetSystemPowerStatus` API,
and the Linux `UPower` API with a single
platform message, `getBatteryLevel()`.

以下程式碼示範瞭如何呼叫平台相關 API 來檢索並顯示當前的電池電量。
它透過平台訊息 `getBatteryLevel()`
來呼叫 Android 的 `BatteryManager` API、
iOS 的 `device.batteryLevel` API、
以及 indows 上的 `GetSystemPowerStatus`。

The example adds the platform-specific code inside
the main app itself.  If you want to reuse the
platform-specific code for multiple apps,
the project creation step is slightly different
(see [developing packages][plugins]),
but the platform channel code
is still written in the same way.

該範例在主應用程式中新增平台相關程式碼。
如果想要將該程式碼重用於多個應用程式，
那麼專案的建立步驟將略有差異
（檢視 [Flutter Packages 的開發和提交][plugins]），
但平台通道程式碼仍以相同方式編寫。

{{site.alert.note}}

  The full, runnable source-code for this example is
  available in [`/examples/platform_channel/`][]
  for Android with Java, iOS with Objective-C,
  Windows with C++, and Linux with C.
  For iOS with Swift,
  see [`/examples/platform_channel_swift/`][].

  可在 [`/examples/platform_channel/`][] 中獲得使用 Java 實現的
  Android 及使用 Objective-C 實現的 iOS 的該範例完整可執行的程式碼。
  對於用 Swift 實現的 iOS 程式碼，
  請參閱 [`/examples/platform_channel_swift/`][]。

{{site.alert.end}}

### Step 1: Create a new app project {#example-project}

### 第一步：建立一個新的應用專案 {#example-project}

Start by creating a new app:

首先建立一個新的應用：

* In a terminal run: `flutter create batterylevel`

  在終端中執行：`flutter create batterylevel`

By default, our template supports writing Android code using Kotlin,
or iOS code using Swift. To use Java or Objective-C,
use the `-i` and/or `-a` flags:

預設情況下，我們的範本使用 Kotlin 編寫 Android 或使用 Swift 編寫 iOS 程式碼。要使用
Java 或 Objective-C，請使用 `-i` 和/或 `-a` 標誌：

* In a terminal run: `flutter create -i objc -a java batterylevel`

  在終端中執行：`flutter create -i objc -a java batterylevel`

### Step 2: Create the Flutter platform client {#example-client}

### 第二步：建立 Flutter 平臺客戶端 {#example-client}

The app's `State` class holds the current app state.
Extend that to hold the current battery state.

應用程式的 `State` 類保持當前應用的狀態。擴充它以保持當前的電池狀態。

First, construct the channel. Use a `MethodChannel` with a single
platform method that returns the battery level.

首先，建構通道。在返回電池電量的單一平台方法中使用 `MethodChannel`。

The client and host sides of a channel are connected through
a channel name passed in the channel constructor.
All channel names used in a single app must
be unique; prefix the channel name with a unique 'domain
prefix', for example: `samples.flutter.dev/battery`.

通道的客戶端和宿主端透過傳遞給通道建構函式的通道名稱進行連線。
一個應用中所使用的所有通道名稱必須是唯一的；
使用唯一的 **域字首** 為通道名稱新增字首，比如：`samples.flutter.dev/battery`。

<?code-excerpt "lib/platform_channels.dart (Import)"?>
```dart
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
```
<?code-excerpt "lib/platform_channels.dart (MyHomePageState)"?>
```dart
class _MyHomePageState extends State<MyHomePage> {
  static const platform = MethodChannel('samples.flutter.dev/battery');
  // Get battery level.
```

Next, invoke a method on the method channel,
specifying the concrete method to call using
the `String` identifier `getBatteryLevel`.
The call might fail&mdash;for example,
if the platform doesn't support the
platform API (such as when running in a simulator),
so wrap the `invokeMethod` call in a try-catch statement.

接下來，在方法通道上呼叫方法（指定透過 String 識別符號 `getBatteryLevel`
呼叫的具體方法）。呼叫可能會失敗&mdash;比如，如果平台不支援此平台
API（比如在模擬器中執行），所以將 `invokeMethod` 呼叫包裹在 try-catch 陳述式中。

Use the returned result to update the user interface state in `_batteryLevel`
inside `setState`.

在 `setState` 中使用返回結果來更新 `_batteryLevel` 內的使用者介面狀態。

<?code-excerpt "lib/platform_channels.dart (GetBattery)"?>
```dart
// Get battery level.
String _batteryLevel = 'Unknown battery level.';

Future<void> _getBatteryLevel() async {
  String batteryLevel;
  try {
    final result = await platform.invokeMethod<int>('getBatteryLevel');
    batteryLevel = 'Battery level at $result % .';
  } on PlatformException catch (e) {
    batteryLevel = "Failed to get battery level: '${e.message}'.";
  }

  setState(() {
    _batteryLevel = batteryLevel;
  });
}
```

Finally, replace the `build` method from the template to
contain a small user interface that displays the battery
state in a string, and a button for refreshing the value.

最後，將範本中的 `build` 方法替換為包含以字串形式
顯示電池狀態、幷包含一個用於重新整理該值的按鈕的小型使用者介面。

<?code-excerpt "lib/platform_channels.dart (Build)"?>
```dart
@override
Widget build(BuildContext context) {
  return Material(
    child: Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: [
          ElevatedButton(
            onPressed: _getBatteryLevel,
            child: const Text('Get Battery Level'),
          ),
          Text(_batteryLevel),
        ],
      ),
    ),
  );
}
```

### Step 3: Add an Android platform-specific implementation

### 步驟 3: 新增 Android 平台的實現

{% samplecode android-channel %}
{% sample Kotlin %}
Start by opening the Android host portion of your Flutter app
in Android Studio:

首先在 Android Studio 中開啟 Flutter 應用的 Android 宿主部分：

1. Start Android Studio

   啟動 Android Studio

1. Select the menu item **File > Open...**

   選擇選單項 **File > Open...**

1. Navigate to the directory holding your Flutter app,
   and select the **android** folder inside it. Click **OK**.

   導航到包含 Flutter 應用的目錄，然後選擇其中的 **android** 資料夾。點選 **OK**。

1. Open the file `MainActivity.kt` located in the **kotlin** folder in the
   Project view.

   在專案檢視中開啟 **kotlin** 資料夾下的 `MainActivity.kt` 檔案。

Inside the `configureFlutterEngine()` method, create a `MethodChannel` and call
`setMethodCallHandler()`. Make sure to use the same channel name as
was used on the Flutter client side.

在 `configureFlutterEngine()` 方法中建立一個 `MethodChannel` 並呼叫
`setMethodCallHandler()`。確保使用的通道名稱與 Flutter 客戶端使用的一致。

<?code-excerpt title="MyActivity.kt"?>
```kotlin
import androidx.annotation.NonNull
import io.flutter.embedding.android.FlutterActivity
import io.flutter.embedding.engine.FlutterEngine
import io.flutter.plugin.common.MethodChannel

class MainActivity: FlutterActivity() {
  private val CHANNEL = "samples.flutter.dev/battery"

  override fun configureFlutterEngine(@NonNull flutterEngine: FlutterEngine) {
    super.configureFlutterEngine(flutterEngine)
    MethodChannel(flutterEngine.dartExecutor.binaryMessenger, CHANNEL).setMethodCallHandler {
      call, result ->
      // This method is invoked on the main thread.
      // TODO
    }
  }
}
```

Add the Android Kotlin code that uses the Android battery APIs to
retrieve the battery level. This code is exactly the same as you
would write in a native Android app.

新增使用 Android battery API 來檢索電池電量的 Android Kotlin 程式碼。該程式碼與你在原生
Android 應用中編寫的程式碼完全相同。

First, add the needed imports at the top of the file:

首先在檔案頭部新增所需的依賴：

<?code-excerpt title="MyActivity.kt"?>
```kotlin
import android.content.Context
import android.content.ContextWrapper
import android.content.Intent
import android.content.IntentFilter
import android.os.BatteryManager
import android.os.Build.VERSION
import android.os.Build.VERSION_CODES
```

Next, add the following method in the `MainActivity` class,
below the `configureFlutterEngine()` method:

然後在 `MainActivity` 類中的 `configureFlutterEngine()` 方法下方新增以下新方法：

<?code-excerpt title="MyActivity.kt"?>
```kotlin
  private fun getBatteryLevel(): Int {
    val batteryLevel: Int
    if (VERSION.SDK_INT >= VERSION_CODES.LOLLIPOP) {
      val batteryManager = getSystemService(Context.BATTERY_SERVICE) as BatteryManager
      batteryLevel = batteryManager.getIntProperty(BatteryManager.BATTERY_PROPERTY_CAPACITY)
    } else {
      val intent = ContextWrapper(applicationContext).registerReceiver(null, IntentFilter(Intent.ACTION_BATTERY_CHANGED))
      batteryLevel = intent!!.getIntExtra(BatteryManager.EXTRA_LEVEL, -1) * 100 / intent.getIntExtra(BatteryManager.EXTRA_SCALE, -1)
    }

    return batteryLevel
  }
```

Finally, complete the `setMethodCallHandler()` method added earlier.
You need to handle a single platform method, `getBatteryLevel()`,
so test for that in the `call` argument.
The implementation of this platform method calls the
Android code written in the previous step, and returns a response for both
the success and error cases using the `result` argument.
If an unknown method is called, report that instead.

最後，完成前面新增的 `onMethodCall()` 方法。
你需要處理單個平台方法 `getBatteryLevel()`，所以在引數 `call` 中對其進行驗證。
該平台方法的實現是呼叫上一步編寫的 Android 程式碼，並使用 `result` 引數來返回成功
和錯誤情況下的響應。如果呼叫了未知方法，則報告該方法。

Remove the following code:

刪除以下程式碼：

<?code-excerpt title="MyActivity.kt"?>
```kotlin
    MethodChannel(flutterEngine.dartExecutor.binaryMessenger, CHANNEL).setMethodCallHandler {
      call, result ->
      // This method is invoked on the main thread.
      // TODO
    }
```

And replace with the following:

並替換成以下內容：

<?code-excerpt title="MyActivity.kt"?>
```kotlin
    MethodChannel(flutterEngine.dartExecutor.binaryMessenger, CHANNEL).setMethodCallHandler {
      // This method is invoked on the main thread.
      call, result ->
      if (call.method == "getBatteryLevel") {
        val batteryLevel = getBatteryLevel()

        if (batteryLevel != -1) {
          result.success(batteryLevel)
        } else {
          result.error("UNAVAILABLE", "Battery level not available.", null)
        }
      } else {
        result.notImplemented()
      }
    }
```

{% sample Java %}
Start by opening the Android host portion of your Flutter app
in Android Studio:

首先在 Android Studio 中開啟 Flutter 應用的 Android 宿主部分：

1. Start Android Studio

   啟動 Android Studio

1. Select the menu item **File > Open...**

   選擇選單項 **File > Open...**

1. Navigate to the directory holding your Flutter app,
   and select the **android** folder inside it. Click **OK**.

   導航到包含 Flutter 應用的目錄，然後選擇其中的 **android** 資料夾。點選 **OK**。

1. Open the `MainActivity.java` file located in the **java** folder in the
   Project view.

   在專案檢視中開啟 **java** 資料夾下的 `MainActivity.java` 檔案。

Next, create a `MethodChannel` and set a `MethodCallHandler`
inside the `configureFlutterEngine()` method.
Make sure to use the same channel name as was used on the
Flutter client side.

接下來，在 `configureFlutterEngine()` 方法中建立一個 `MethodChannel` 並設定一個
`MethodCallHandler`。確保使用的通道名稱與 Flutter 客戶端使用的一致。

<?code-excerpt title="MainActivity.java"?>
```java
import androidx.annotation.NonNull;
import io.flutter.embedding.android.FlutterActivity;
import io.flutter.embedding.engine.FlutterEngine;
import io.flutter.plugin.common.MethodChannel;

public class MainActivity extends FlutterActivity {
  private static final String CHANNEL = "samples.flutter.dev/battery";

  @Override
  public void configureFlutterEngine(@NonNull FlutterEngine flutterEngine) {
  super.configureFlutterEngine(flutterEngine);
    new MethodChannel(flutterEngine.getDartExecutor().getBinaryMessenger(), CHANNEL)
        .setMethodCallHandler(
          (call, result) -> {
            // This method is invoked on the main thread.
            // TODO
          }
        );
  }
}
```

Add the Android Java code that uses the Android battery APIs to
retrieve the battery level. This code is exactly the same as you
would write in a native Android app.

新增使用 Android battery API 來檢索電池電量的 Android Java 程式碼。
該程式碼與你在原生 Android 應用中編寫的程式碼完全相同。

First, add the needed imports at the top of the file:

首先在檔案頭部新增所需的依賴：

<?code-excerpt title="MainActivity.java"?>
```java
import android.content.ContextWrapper;
import android.content.Intent;
import android.content.IntentFilter;
import android.os.BatteryManager;
import android.os.Build.VERSION;
import android.os.Build.VERSION_CODES;
import android.os.Bundle;
```

Then add the following as a new method in the activity class,
below the `configureFlutterEngine()` method:

然後在 Activity 類中的 `onCreate()` 方法下方新增以下新方法：

<?code-excerpt title="MainActivity.java"?>
```java
  private int getBatteryLevel() {
    int batteryLevel = -1;
    if (VERSION.SDK_INT >= VERSION_CODES.LOLLIPOP) {
      BatteryManager batteryManager = (BatteryManager) getSystemService(BATTERY_SERVICE);
      batteryLevel = batteryManager.getIntProperty(BatteryManager.BATTERY_PROPERTY_CAPACITY);
    } else {
      Intent intent = new ContextWrapper(getApplicationContext()).
          registerReceiver(null, new IntentFilter(Intent.ACTION_BATTERY_CHANGED));
      batteryLevel = (intent.getIntExtra(BatteryManager.EXTRA_LEVEL, -1) * 100) /
          intent.getIntExtra(BatteryManager.EXTRA_SCALE, -1);
    }

    return batteryLevel;
  }
```

Finally, complete the `setMethodCallHandler()` method added earlier.
You need to handle a single platform method, `getBatteryLevel()`,
so test for that in the `call` argument. The implementation of
this platform method calls the Android code written
in the previous step, and returns a response for both
the success and error cases using the `result` argument.
If an unknown method is called, report that instead.

最後，完成前面新增的 `onMethodCall()` 方法，
你需要處理單個平台方法 `getBatteryLevel()`，所以在引數 `call` 中對其進行驗證。
該平台方法的實現是呼叫上一步編寫的 Android 程式碼，並使用 `result` 引數來返回
成功和錯誤情況下的響應。如果呼叫了未知方法，則報告該方法。

Remove the following code:

移除以下程式碼：

<?code-excerpt title="MainActivity.java"?>
```java
          (call, result) -> {
            // This method is invoked on the main thread.
            // TODO
          }
```

And replace with the following:

並替換成以下內容：

<?code-excerpt title="MainActivity.java"?>
```java
          (call, result) -> {
            // This method is invoked on the main thread.
            if (call.method.equals("getBatteryLevel")) {
              int batteryLevel = getBatteryLevel();

              if (batteryLevel != -1) {
                result.success(batteryLevel);
              } else {
                result.error("UNAVAILABLE", "Battery level not available.", null);
              }
            } else {
              result.notImplemented();
            }
          }
```
{% endsamplecode %}

You should now be able to run the app on Android. If using the Android
Emulator, set the battery level in the Extended Controls panel
accessible from the **...** button in the toolbar.

現在你應該可以在 Android 中執行該應用。如果使用了 Android
模擬器，請在**擴充控制項**面板中設定電池電量，可從工具欄中的 **...** 按鈕存取。

### Step 4: Add an iOS platform-specific implementation

### 步驟 4：新增 iOS 平台的實現

{% samplecode ios-channel %}
{% sample Swift %}
Start by opening the iOS host portion of your Flutter app in Xcode:

首先在 Xcode 中開啟 Flutter 應用的 iOS 宿主部分：

1. Start Xcode.

   啟動 Xcode

1. Select the menu item **File > Open...**.

   選擇選單項 **File > Open...**

1. Navigate to the directory holding your Flutter app, and select the **ios**
folder inside it. Click **OK**.

   導航到包含 Flutter 應用的目錄，然後選擇其中的 **ios** 資料夾。點選 **OK**。

Add support for Swift in the standard template setup that uses Objective-C:

在使用 Objective-C 的標準範本設定中新增對 Swift 的支援：

1. **Expand Runner > Runner** in the Project navigator.

   在專案導航中展開 **Expand Runner > Runner**。

1. Open the file `AppDelegate.swift` located under **Runner > Runner**
   in the Project navigator.
   
   開啟專案導航 `Runner > Runner` 下的 `AppDelegate.swift` 檔案。

Override the `application:didFinishLaunchingWithOptions:` function and create
a `FlutterMethodChannel` tied to the channel name
`samples.flutter.dev/battery`:

重寫 `application:didFinishLaunchingWithOptions:` 方法，
然後建立一個 `FlutterMethodChannel` 繫結到名字為
`samples.flutter.dev/battery` 名稱的 channel：

<?code-excerpt title="AppDelegate.swift"?>
```swift
@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
  override func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

    let controller : FlutterViewController = window?.rootViewController as! FlutterViewController
    let batteryChannel = FlutterMethodChannel(name: "samples.flutter.dev/battery",
                                              binaryMessenger: controller.binaryMessenger)
    batteryChannel.setMethodCallHandler({
      (call: FlutterMethodCall, result: @escaping FlutterResult) -> Void in
      // This method is invoked on the UI thread.
      // Handle battery messages.
    })

    GeneratedPluginRegistrant.register(with: self)
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }
}
```

Next, add the iOS Swift code that uses the iOS battery APIs to retrieve
the battery level. This code is exactly the same as you
would write in a native iOS app.

然後，新增 iOS Swift 程式碼，使用電池相關的 API 獲取電量。
這裡的程式碼和你寫原生 iOS 程式碼別無二致。

Add the following as a new method at the bottom of `AppDelegate.swift`:

在 `AppDelegate.swift` 末尾新增以下新的方法：

<?code-excerpt title="AppDelegate.swift"?>
```swift
private func receiveBatteryLevel(result: FlutterResult) {
  let device = UIDevice.current
  device.isBatteryMonitoringEnabled = true
  if device.batteryState == UIDevice.BatteryState.unknown {
    result(FlutterError(code: "UNAVAILABLE",
                        message: "Battery level not available.",
                        details: nil))
  } else {
    result(Int(device.batteryLevel * 100))
  }
}
```

Finally, complete the `setMethodCallHandler()` method added earlier.
You need to handle a single platform method, `getBatteryLevel()`,
so test for that in the `call` argument.
The implementation of this platform method calls
the iOS code written in the previous step. If an unknown method
is called, report that instead.

最後，完成前面新增的 `setMethodCallHandler()` 方法。
你需要處理單個平台方法 `getBatteryLevel()`，
所以在引數 `call` 中對其進行驗證。
該平台方法的實現是呼叫上一步編寫的 iOS 程式碼。
如果呼叫了未知方法，則報告該方法。

<?code-excerpt title="AppDelegate.swift"?>
```swift
batteryChannel.setMethodCallHandler({
  [weak self] (call: FlutterMethodCall, result: FlutterResult) -> Void in
  // This method is invoked on the UI thread.
  guard call.method == "getBatteryLevel" else {
    result(FlutterMethodNotImplemented)
    return
  }
  self?.receiveBatteryLevel(result: result)
})
```

{% sample Objective-C %}
Start by opening the iOS host portion of the Flutter app in Xcode:

首先在 Xcode 中開啟 Flutter 應用的 iOS 宿主部分：

1. Start Xcode.

   啟動 Xcode

1. Select the menu item **File > Open...**.

   選擇選單項 **File > Open...**

1. Navigate to the directory holding your Flutter app,
   and select the **ios** folder inside it. Click **OK**.
   
   導航到包含 Flutter 應用的目錄，然後選擇其中的 **ios** 資料夾。點選 **OK**。

1. Make sure the Xcode projects builds without errors.

   確保 Xcode 專案建構沒有錯誤。

1. Open the file `AppDelegate.m`, located under **Runner > Runner**
   in the Project navigator.
   
   開啟專案導航 **Runner > Runner** 下的 `AppDelegate.m` 檔案。

Create a `FlutterMethodChannel` and add a handler inside the `application
didFinishLaunchingWithOptions:` method.
Make sure to use the same channel name
as was used on the Flutter client side.

在 `application didFinishLaunchingWithOptions:` 方法中
建立一個 `FlutterMethodChannel` 並新增一個處理程式。
確保使用的通道名稱與 Flutter 客戶端使用的一致。

<?code-excerpt title="AppDelegate.m"?>
```objectivec
#import <Flutter/Flutter.h>
#import "GeneratedPluginRegistrant.h"

@implementation AppDelegate
- (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions {
  FlutterViewController* controller = (FlutterViewController*)self.window.rootViewController;

  FlutterMethodChannel* batteryChannel = [FlutterMethodChannel
                                          methodChannelWithName:@"samples.flutter.dev/battery"
                                          binaryMessenger:controller.binaryMessenger];

  [batteryChannel setMethodCallHandler:^(FlutterMethodCall* call, FlutterResult result) {
    // This method is invoked on the UI thread.
    // TODO
  }];

  [GeneratedPluginRegistrant registerWithRegistry:self];
  return [super application:application didFinishLaunchingWithOptions:launchOptions];
}
```

Next, add the iOS ObjectiveC code that uses the iOS battery APIs to
retrieve the battery level. This code is exactly the same as you
would write in a native iOS app.

接下來新增使用 iOS battery API 來檢索電池電量的 iOS Objective-C 程式碼。
該程式碼與你在原生 iOS 應用中編寫的程式碼完全相同。

Add the following method in the `AppDelegate` class, just before `@end`:

在 `AppDelegate` 類中的 `@end` 之前新增以下方法：

<?code-excerpt title="AppDelegate.m"?>
```objectivec
- (int)getBatteryLevel {
  UIDevice* device = UIDevice.currentDevice;
  device.batteryMonitoringEnabled = YES;
  if (device.batteryState == UIDeviceBatteryStateUnknown) {
    return -1;
  } else {
    return (int)(device.batteryLevel * 100);
  }
}
```

Finally, complete the `setMethodCallHandler()` method added earlier.
You need to handle a single platform method, `getBatteryLevel()`,
so test for that in the `call` argument. The implementation of
this platform method calls the iOS code written in the previous step,
and returns a response for both the success and error cases using
the `result` argument. If an unknown method is called, report that instead.

最後，完成前面新增的 `setMethodCallHandler()` 方法。
你需要處理單個平台方法 `getBatteryLevel()`，所以在引數`call` 中對其進行驗證。
該平台方法的實現是呼叫上一步編寫的 iOS 程式碼，並使用 `result` 引數來返回成功
和錯誤情況下的響應。如果呼叫了未知方法，則報告該方法。

<?code-excerpt title="AppDelegate.m"?>
```objectivec
__weak typeof(self) weakSelf = self;
[batteryChannel setMethodCallHandler:^(FlutterMethodCall* call, FlutterResult result) {
  // This method is invoked on the UI thread.
  if ([@"getBatteryLevel" isEqualToString:call.method]) {
    int batteryLevel = [weakSelf getBatteryLevel];

    if (batteryLevel == -1) {
      result([FlutterError errorWithCode:@"UNAVAILABLE"
                                 message:@"Battery level not available."
                                 details:nil]);
    } else {
      result(@(batteryLevel));
    }
  } else {
    result(FlutterMethodNotImplemented);
  }
}];
```
{% endsamplecode %}

You should now be able to run the app on iOS.
If using the iOS Simulator,
note that it doesn't support battery APIs,
and the app displays 'Battery level not available'.

現在你應該可以在 iOS 中執行該應用。
如果使用了 iOS 模擬器（注意它並不支援 battery API），
應用則會顯示 'battery info unavailable'。

### Step 5: Add a Windows platform-specific implementation

### 第五步：新增 Windows 平台特定實現

Start by opening the Windows host portion of your Flutter app in Visual Studio:

首先在 Visual Studio 中開啟你 Flutter 應用 Windows 的 host 部分：
 
1. Run `flutter build windows` in your project directory once to generate
   the Visual Studio solution file.

   在你專案的目錄夾下執行一次 `flutter build windows` 以產生 Visual Studio solution 檔案。

1. Start Visual Studio.

   啟動 Visual Studio。

1. Select **Open a project or solution**.

   選擇 **Open a project or solution**

1. Navigate to the directory holding your Flutter app, then into the **build**
   folder, then the **windows** folder, then select the `batterylevel.sln` file.
   Click **Open**.

   導航至含有你 Flutter 應用的目錄下，然後進入 **build** 資料夾，然後是 **windows** 資料夾，
   然後選擇 `batterylevel.sln` 檔案，點選 **Open**。

Add the C++ implementation of the platform channel method:

然後新增 platform channel 方法的 c++ 實現：

1. Expand **batterylevel > Source Files** in the Solution Explorer.

   在 Solution 瀏覽器中展開 **batterylevel > Source Files**

1. Open the file `flutter_window.cpp`.

   開啟 `flutter_window.cpp`。
  
First, add the necessary includes to the top of the file, just
after `#include "flutter_window.h"`:

首先，在檔案的最頂部新增必要的參考，在 `#include "flutter_window.h"` 下面寫上就行：

<?code-excerpt title="flutter_window.cpp"?>
```cpp
#include <flutter/event_channel.h>
#include <flutter/event_sink.h>
#include <flutter/event_stream_handler_functions.h>
#include <flutter/method_channel.h>
#include <flutter/standard_method_codec.h>
#include <windows.h>

#include <memory>
```

Edit the `FlutterWindow::OnCreate` method and create
a `flutter::MethodChannel` tied to the channel name
`samples.flutter.dev/battery`:

編輯 `FlutterWindow::OnCreate` 方法，然後建立一個 `flutter::MethodChannel`
繫結 `samples.flutter.dev/battery` 名字：

<?code-excerpt title="flutter_window.cpp"?>
```cpp
bool FlutterWindow::OnCreate() {
  // ...
  RegisterPlugins(flutter_controller_->engine());

  flutter::MethodChannel<> channel(
      flutter_controller_->engine()->messenger(), "samples.flutter.dev/battery",
      &flutter::StandardMethodCodec::GetInstance());
  channel.SetMethodCallHandler(
      [](const flutter::MethodCall<>& call,
         std::unique_ptr<flutter::MethodResult<>> result) {
        // TODO
      });

  SetChildContent(flutter_controller_->view()->GetNativeWindow());
  return true;
}
```

Next, add the C++ code that uses the Windows battery APIs to
retrieve the battery level. This code is exactly the same as
you would write in a native Windows application.

接下來新增使用 Windows battery API 來檢索電池電量的程式碼。該程式碼與你在原生
Windows 應用中編寫程式碼別無二致。

Add the following as a new function at the top of
`flutter_window.cpp` just after the `#include` section:

在 `flutter_window.cpp` 頂部新增下面的新方法，在 `#include` 下面新增：

<?code-excerpt title="flutter_window.cpp"?>
```cpp
static int GetBatteryLevel() {
  SYSTEM_POWER_STATUS status;
  if (GetSystemPowerStatus(&status) == 0 || status.BatteryLifePercent == 255) {
    return -1;
  }
  return status.BatteryLifePercent;
}
```

Finally, complete the `setMethodCallHandler()` method added earlier.
You need to handle a single platform method, `getBatteryLevel()`,
so test for that in the `call` argument.
The implementation of this platform method calls
the Windows code written in the previous step. If an unknown method
is called, report that instead.

最後，完成 `setMethodCallHandler()` 方法。
你可以在這裡處理平台方法，`getBatteryLevel()`，
然後可以在 `call` 引數中進行測試。
這個平台方法呼叫的實現，在之前的步驟中已經完成了。
如果呼叫了一個未知的，請報告它。
  
Remove the following code:

移除下面的程式碼：

<?code-excerpt title="flutter_window.cpp"?>
```cpp
  channel.SetMethodCallHandler(
      [](const flutter::MethodCall<>& call,
         std::unique_ptr<flutter::MethodResult<>> result) {
        // TODO
      });
```

And replace with the following:

然後替換為這個：

<?code-excerpt title="flutter_window.cpp"?>
```cpp
  channel.SetMethodCallHandler(
      [](const flutter::MethodCall<>& call,
         std::unique_ptr<flutter::MethodResult<>> result) {
        if (call.method_name() == "getBatteryLevel") {
          int battery_level = GetBatteryLevel();
          if (battery_level != -1) {
            result->Success(battery_level);
          } else {
            result->Error("UNAVAILABLE", "Battery level not available.");
          }
        } else {
          result->NotImplemented();
        }
      });
```

You should now be able to run the application on Windows.
If your device doesn't have a battery,
it displays 'Battery level not available'.
  
### Step 6: Add a macOS platform-specific implementation

Start by opening the macOS host portion of your Flutter app in Xcode:

1. Start Xcode.

1. Select the menu item **File > Open...**.

1. Navigate to the directory holding your Flutter app, and select the **macos**
folder inside it. Click **OK**.

Add the Swift implementation of the platform channel method:

1. **Expand Runner > Runner** in the Project navigator.

1. Open the file `MainFlutterWindow.swift` located under **Runner > Runner**
   in the Project navigator.

First, add the necessary import to the top of the file, just after
`import FlutterMacOS`:

<?code-excerpt title="MainFlutterWindow.swift"?>
```swift
import IOKit.ps
```

Create a `FlutterMethodChannel` tied to the channel name
`samples.flutter.dev/battery` in the `awakeFromNib` method:

<?code-excerpt title="MainFlutterWindow.swift"?>
```swift
  override func awakeFromNib() {
    // ...
    self.setFrame(windowFrame, display: true)
  
    let batteryChannel = FlutterMethodChannel(
      name: "samples.flutter.dev/battery",
      binaryMessenger: flutterViewController.engine.binaryMessenger)
    batteryChannel.setMethodCallHandler { (call, result) in
      // This method is invoked on the UI thread.
      // Handle battery messages.
    }

    RegisterGeneratedPlugins(registry: flutterViewController)

    super.awakeFromNib()
  }
}
```

Next, add the macOS Swift code that uses the IOKit battery APIs to retrieve
the battery level. This code is exactly the same as you
would write in a native macOS app.

Add the following as a new method at the bottom of `MainFlutterWindow.swift`:

<?code-excerpt title="MainFlutterWindow.swift"?>
```swift
private func getBatteryLevel() -> Int? {
  let info = IOPSCopyPowerSourcesInfo().takeRetainedValue()
  let sources: Array<CFTypeRef> = IOPSCopyPowerSourcesList(info).takeRetainedValue() as Array
  if let source = sources.first {
    let description =
      IOPSGetPowerSourceDescription(info, source).takeUnretainedValue() as! [String: AnyObject]
    if let level = description[kIOPSCurrentCapacityKey] as? Int {
      return level
    }
  }
  return nil
}
```

Finally, complete the `setMethodCallHandler` method added earlier.
You need to handle a single platform method, `getBatteryLevel()`,
so test for that in the `call` argument.
The implementation of this platform method calls
the macOS code written in the previous step. If an unknown method
is called, report that instead.

<?code-excerpt title="MainFlutterWindow.swift"?>
```swift
batteryChannel.setMethodCallHandler { (call, result) in
  switch call.method {
  case "getBatteryLevel":
    guard let level = getBatteryLevel() else {
      result(
        FlutterError(
          code: "UNAVAILABLE",
          message: "Battery level not available",
          details: nil))
     return
    }
    result(level)
  default:
    result(FlutterMethodNotImplemented)
  }
}
```

You should now be able to run the application on macOS.
If your device doesn't have a battery,
it displays 'Battery level not available'.

### Step 7: Add a Linux platform-specific implementation
  
For this example you need to install the `upower` developer headers.
This is likely available from your distribution, for example with:
```sh
sudo apt install libupower-glib-dev
```

Start by opening the Linux host portion of your Flutter app in the editor
of your choice. The instructions below are for Visual Studio Code with the
"C/C++" and "CMake" extensions installed, but can be adjusted for other IDEs.

1. Launch Visual Studio Code.

1. Open the **linux** directory inside your project.

1. Choose **Yes** in the prompt asking: `Would you like to configure project "linux"?`.
   This enables C++ autocomplete.

1. Open the file `my_application.cc`.
  
First, add the necessary includes to the top of the file, just
after `#include <flutter_linux/flutter_linux.h`:

<?code-excerpt title="my_application.cc"?>
```c
#include <math.h>
#include <upower.h>
```

Add an `FlMethodChannel` to the `_MyApplication` struct:

<?code-excerpt title="my_application.cc"?>
```c
struct _MyApplication {
  GtkApplication parent_instance;
  char** dart_entrypoint_arguments;
  FlMethodChannel* battery_channel;
};
```

Make sure to clean it up in `my_application_dispose`:

<?code-excerpt title="my_application.cc"?>
```c
static void my_application_dispose(GObject* object) {
  MyApplication* self = MY_APPLICATION(object);
  g_clear_pointer(&self->dart_entrypoint_arguments, g_strfreev);
  g_clear_object(&self->battery_channel);
  G_OBJECT_CLASS(my_application_parent_class)->dispose(object);
}
```

Edit the `my_application_activate` method and initialize
`battery_channel` using the channel name
`samples.flutter.dev/battery`, just after the call to
`fl_register_plugins`:

<?code-excerpt title="my_application.cc"?>
```c
static void my_application_activate(GApplication* application) {
  // ...
  fl_register_plugins(FL_PLUGIN_REGISTRY(self->view));

  g_autoptr(FlStandardMethodCodec) codec = fl_standard_method_codec_new();
  self->battery_channel = fl_method_channel_new(
      fl_engine_get_binary_messenger(fl_view_get_engine(view)),
      "samples.flutter.dev/battery", FL_METHOD_CODEC(codec));
  fl_method_channel_set_method_call_handler(
      self->battery_channel, battery_method_call_handler, self, nullptr);

  gtk_widget_grab_focus(GTK_WIDGET(self->view));
}
```

Next, add the C code that uses the Linux battery APIs to
retrieve the battery level. This code is exactly the same as
you would write in a native Linux application.

Add the following as a new function at the top of
`my_application.cc` just after the `G_DEFINE_TYPE` line:

<?code-excerpt title="my_application.cc"?>
```c
static FlMethodResponse* get_battery_level() {
  // Find the first available battery and report that.
  g_autoptr(UpClient) up_client = up_client_new();
  g_autoptr(GPtrArray) devices = up_client_get_devices2(up_client);
  if (devices->len == 0) {
    return FL_METHOD_RESPONSE(fl_method_error_response_new(
        "UNAVAILABLE", "Device does not have a battery.", nullptr));
  }

  UpDevice* device = (UpDevice*)(g_ptr_array_index(devices, 0));
  double percentage = 0;
  g_object_get(device, "percentage", &percentage, nullptr);

  g_autoptr(FlValue) result =
      fl_value_new_int(static_cast<int64_t>(round(percentage)));
  return FL_METHOD_RESPONSE(fl_method_success_response_new(result));
}
```

Finally, add the `battery_method_call_handler` function referenced
in the earlier call to `fl_method_channel_set_method_call_handler`.
You need to handle a single platform method, `getBatteryLevel`,
so test for that in the `method_call` argument.
The implementation of this function calls
the Linux code written in the previous step. If an unknown method
is called, report that instead.
  
Add the following code after the `get_battery_level` function:

<?code-excerpt title="flutter_window.cpp"?>
```c
static void battery_method_call_handler(FlMethodChannel* channel,
                                        FlMethodCall* method_call,
                                        gpointer user_data) {
  g_autoptr(FlMethodResponse) response = nullptr;
  if (strcmp(fl_method_call_get_name(method_call), "getBatteryLevel") == 0) {
    response = get_battery_level();
  } else {
    response = FL_METHOD_RESPONSE(fl_method_not_implemented_response_new());
  }

  g_autoptr(GError) error = nullptr;
  if (!fl_method_call_respond(method_call, response, &error)) {
    g_warning("Failed to send response: %s", error->message);
  }
}
```

You should now be able to run the application on Linux.
If your device doesn't have a battery,
it displays 'Battery level not available'.

你現在應該可以在 Windows 上執行應用了。
如果你的裝置沒有電池的話，
它會提示 'Battery level not available'。

## Typesafe platform channels using Pigeon {#pigeon}

## 透過 Pigeon 獲得型別安全的通道 {#pigeon}

The previous example uses `MethodChannel`
to communicate between the host and client,
which isn't typesafe. Calling and receiving
messages depends on the host and client declaring
the same arguments and datatypes in order for messages to work.
You can use the [Pigeon][pigeon] package as
an alternative to `MethodChannel`
to generate code that sends messages in a
structured, typesafe manner.

在之前的範例中，我們使用 `MethodChannel` 在 host 和 client 之間進行通訊，
然而這並不是型別安全的。為了正確通訊，
呼叫/接收訊息取決於 host 和 client 宣告相同的引數和資料型別。
[Pigeon][] 套件可以用作 `MethodChannel` 的替代品，
它將產生以結構化型別安全方式傳送訊息的程式碼。

With [Pigeon][pigeon], the messaging protocol is defined
in a subset of Dart that then generates messaging
code for Android, iOS, macOS, or Windows. You can find a more complete
example and more information on the [`pigeon`][pigeon]
page on pub.dev.

在 [Pigeon][] 中，訊息介面在 Dart 中進行定義，
然後它將產生對應的 Android 以及 iOS 的程式碼。
更復雜的例子以及更多資訊盡在 [`pigeon`][pigeon]。

Using [Pigeon][pigeon] eliminates the need to match
strings between host and client
for the names and datatypes of messages.
It supports: nested classes, grouping
messages into APIs, generation of
asynchronous wrapper code and sending messages
in either direction. The generated code is readable
and guarantees there are no conflicts between
multiple clients of different versions.
Supported languages are Objective-C, Java, Kotlin, C++,
and Swift (with Objective-C interop).

使用 [Pigeon][] 消除了在主機和客戶端之間
匹配字串的需要訊息的名稱和資料型別。
它支援：巢狀(Nesting)類，訊息轉換為 API，產生非同步包裝程式碼併發送訊息。
產生的程式碼具有相當的可讀性並保證在不同版本的多個客戶端之間沒有衝突。
支援 Objective-C，Java，Kotlin 和 Swift（透過 Objective-C 互操作）語言。

### Pigeon example

### Pigeon 範例

**Pigeon file:**
  
**Pigeon 檔案:**

<?code-excerpt "lib/pigeon_source.dart (Search)"?>
```dart
import 'package:pigeon/pigeon.dart';

class SearchRequest {
  final String query;

  SearchRequest({required this.query});
}

class SearchReply {
  final String result;

  SearchReply({required this.result});
}

@HostApi()
abstract class Api {
  @async
  SearchReply search(SearchRequest request);
}
```

**Dart usage:**
  
**Dart 用法:**

<?code-excerpt "lib/use_pigeon.dart (UseApi)"?>
```dart
import 'generated_pigeon.dart';

Future<void> onClick() async {
  SearchRequest request = SearchRequest(query: 'test');
  Api api = SomeApi();
  SearchReply reply = await api.search(request);
  print('reply: ${reply.result}');
}
```

## Separate platform-specific code from UI code {#separate}

## 從 UI 程式碼中分離平台相關程式碼 {#separate}

If you expect to use your platform-specific code
in multiple Flutter apps, you might consider
separating the code into a platform plugin located
in a directory outside your main application.
See [developing packages][] for details.

如果你想要在多個 Flutter 應用中使用你的平台相關程式碼，
則將程式碼分離為位於主應用目錄之外的平台外掛會很有用。相關細節檢視
[Flutter Packages 的開發和提交][developing packages]。

## Publish platform-specific code as a package {#publish}

## 將平台相關程式碼作為 Package 進行提交 {#publish}

To share your platform-specific code with other developers
in the Flutter ecosystem, see [publishing packages][].

與 Flutter 生態中的其他開發者共享你的平台相關程式碼，
可檢視 [提交 package][publishing packages]。

## Custom channels and codecs

## 自訂通道和編解碼器

Besides the above mentioned `MethodChannel`,
you can also use the more basic
[`BasicMessageChannel`][], which supports basic,
asynchronous message passing using a custom message codec.
You can also use the specialized [`BinaryCodec`][],
[`StringCodec`][], and [`JSONMessageCodec`][]
classes, or create your own codec.

除了上面提到的 `MethodChannel`，你還可以使用更基礎的
[`BasicMessageChannel`][]，
它支援使用自訂的訊息編解碼器進行基本的非同步訊息傳遞。你還可以使用專門的
[`BinaryCodec`][]、[`StringCodec`][] 和
[`JSONMessageCodec`][] 類，或建立自己的編解碼器。

You might also check out an example of a custom codec
in the [`cloud_firestore`][] plugin,
which is able to serialize and deserialize many more
types than the default types.

您還可以在 [`cloud_firestore`][] 外掛中檢視自訂編解碼器的範例，
該外掛可以序列化和反序列化比預設型別更多的型別。

## Channels and platform threading

## 通道和平台執行緒

When invoking channels on the platform side destined for Flutter,
invoke them on the platform's main thread.
When invoking channels in Flutter destined for the platform side,
either invoke them from any `Isolate` that is the root
`Isolate`, _or_ that is registered as a background `Isolate`.
The handlers for the platform side can execute on the platform's main thread
or they can execute on a background thread if using a Task Queue.
You can invoke the platform side handlers asynchronously
and on any thread.

目標平台向 Flutter 發起 channel 呼叫的時候，需要在對應平台的主執行緒執行。
同樣的，在 Flutter 向目標平台發起 channel 呼叫的時候，需要在根 `Isolate` 中執行。
對應平台側的 handler 既可以在平台的主執行緒執行，也可以透過事件迴圈在後台執行。
對應平台側 handler 的返回值可以在任意執行緒非同步執行。

{{site.alert.note}}

  On Android, the platform's main thread is sometimes
  called the "main thread", but it is technically defined
  as [the UI thread][]. Annotate methods that need
  to be run on the UI thread with `@UiThread`.
  On iOS, this thread is officially
  referred to as [the main thread][].

  在 Android 平臺上時，平台的 main 執行緒有時候被叫做主執行緒，
  但是它在技術上被看作 [UI 執行緒][the UI thread]。
  被 `@UiThread` 註解標記的方法需要在 UI 執行緒上執行。
  在 iOS 上，這個執行緒被官方標記為[主執行緒][the main thread]。

{{site.alert.end}}

### Using plugins and channels from background isolates

Plugins and channels can be used by any `Isolate`, but that `Isolate` has to be
a root `Isolate` (the one created by Flutter) or registered as a background
`Isolate` for a root `Isolate`.

The following example shows how to register a background `Isolate` in order to
use a plugin from a background `Isolate`.

```dart
import 'package:flutter/services.dart';
import 'package:shared_preferences/shared_preferences.dart';

void _isolateMain(RootIsolateToken rootIsolateToken) async {
  BackgroundIsolateBinaryMessenger.ensureInitialized(rootIsolateToken);
  SharedPreferences sharedPreferences = await SharedPreferences.getInstance();
  print(sharedPreferences.getBool('isDebug'));
}

void main() {
  RootIsolateToken rootIsolateToken = RootIsolateToken.instance!;
  Isolate.spawn(_isolateMain, rootIsolateToken);
}
```

### Executing channel handlers on background threads

### 在後台執行緒中執行 channel 的 handlers

In order for a channel's platform side handler to
execute on a background thread, you must use the
Task Queue API. Currently this feature is only
supported on iOS and Android.

要在 channel 對應的平台側的後臺中執行 handler，需要使用 Task Queue API。
當前該功能僅支援在 iOS 和 Android。

In Java:

對應的 Java 程式碼：

```java
@Override
public void onAttachedToEngine(@NonNull FlutterPluginBinding binding) {
  BinaryMessenger messenger = binding.getBinaryMessenger();
  BinaryMessenger.TaskQueue taskQueue =
      messenger.makeBackgroundTaskQueue();
  channel =
      new MethodChannel(
          messenger,
          "com.example.foo",
          StandardMethodCodec.INSTANCE,
          taskQueue);
  channel.setMethodCallHandler(this);
}
```

In Kotlin:

Kotlin 版本：

```kotlin
override fun onAttachedToEngine(@NonNull flutterPluginBinding: FlutterPlugin.FlutterPluginBinding) {
  val taskQueue =
      flutterPluginBinding.binaryMessenger.makeBackgroundTaskQueue()
  channel = MethodChannel(flutterPluginBinding.binaryMessenger,
                          "com.example.foo",
                          StandardMethodCodec.INSTANCE,
                          taskQueue)
  channel.setMethodCallHandler(this)
}
```

In Swift:

Swift 版本：

{{site.alert.note}}

  In release 2.10, the Task Queue API is only available on the `master` channel
  for iOS.

  在 2.10 的釋出中，若要在 iOS 上使用 Task Queue API，只能切換到 `master` 渠道。

{{site.alert.end}}

```swift
public static func register(with registrar: FlutterPluginRegistrar) {
  let taskQueue = registrar.messenger.makeBackgroundTaskQueue()
  let channel = FlutterMethodChannel(name: "com.example.foo",
                                     binaryMessenger: registrar.messenger(),
                                     codec: FlutterStandardMethodCodec.sharedInstance,
                                     taskQueue: taskQueue)
  let instance = MyPlugin()
  registrar.addMethodCallDelegate(instance, channel: channel)
}
```

In Objective-C:

Objective-C 版本：

{{site.alert.note}}

  In release 2.10, the Task Queue API is only available on the `master` channel
  for iOS.

  在 2.10 的釋出中，若要在 iOS 上使用 Task Queue API，只能切換到 `master` 渠道。

{{site.alert.end}}

```objc
+ (void)registerWithRegistrar:(NSObject<FlutterPluginRegistrar>*)registrar {
  NSObject<FlutterTaskQueue>* taskQueue =
      [[registrar messenger] makeBackgroundTaskQueue];
  FlutterMethodChannel* channel =
      [FlutterMethodChannel methodChannelWithName:@"com.example.foo"
                                  binaryMessenger:[registrar messenger]
                                            codec:[FlutterStandardMethodCodec sharedInstance]
                                        taskQueue:taskQueue];
  MyPlugin* instance = [[MyPlugin alloc] init];
  [registrar addMethodCallDelegate:instance channel:channel];
}
```


### Jumping to the UI thread in Android

### 跳轉到 Android 中的 UI 執行緒

To comply with channels' UI thread requirement,
you might need to jump from a background thread
to Android's UI thread to execute a channel method.
In Android, you can accomplish this by `post()`ing a
`Runnable` to Android's UI thread `Looper`,
which causes the `Runnable` to execute on the
main thread at the next opportunity.

為了符合通道跳轉到 Android UI 執行緒的要求，
你可能需要從後臺執行緒跳轉到 Android 的 UI 執行緒以執行通道的方法。
在 Android 中的實現方式是：
在一個叫 `Looper` 的 Android UI 執行緒裡 `post()` 一個 `Runnable`。
這能使得 `Runnable` 在下一次機會時在主執行緒上執行。

In Java:

Java 程式碼：

```java
new Handler(Looper.getMainLooper()).post(new Runnable() {
  @Override
  public void run() {
    // Call the desired channel message here.
  }
});
```

In Kotlin:

Kotlin 程式碼：

```kotlin
Handler(Looper.getMainLooper()).post {
  // Call the desired channel message here.
}
```

### Jumping to the main thread in iOS

### 跳轉到 iOS 中的主執行緒

To comply with channel's main thread requirement,
you might need to jump from a background thread to
iOS's main thread to execute a channel method.
You can accomplish this in iOS by executing a
[block][] on the main [dispatch queue][]:

為了符合通道跳轉到 iOS 主執行緒的要求，
您可能需要從後臺執行緒跳轉到 iOS 的主執行緒來執行通道方法。
在iOS中，這是透過在主 [dispatch queue][]上執行 [block][]來實現的：

In Objective-C:

Objective-C 程式碼：

```objectivec
dispatch_async(dispatch_get_main_queue(), ^{
  // Call the desired channel message here.
});
```

In Swift:

Swift 程式碼：

```swift
DispatchQueue.main.async {
  // Call the desired channel message here.
}
```

[`BasicMessageChannel`]: {{site.api}}/flutter/services/BasicMessageChannel-class.html
[`BinaryCodec`]: {{site.api}}/flutter/services/BinaryCodec-class.html
[block]: {{site.apple-dev}}/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithBlocks/WorkingwithBlocks.html
[`cloud_firestore`]: {{site.github}}/FirebaseExtended/flutterfire/blob/master/packages/cloud_firestore/cloud_firestore_platform_interface/lib/src/method_channel/utils/firestore_message_codec.dart
[`dart:html` library]: {{site.dart.api}}/dart-html/dart-html-library.html
[developing packages]: {{site.url}}/packages-and-plugins/developing-packages
[plugins]: {{site.url}}/packages-and-plugins/developing-packages#plugin
[dispatch queue]: {{site.apple-dev}}/documentation/dispatch/dispatchqueue
[`/examples/platform_channel/`]: {{site.repo.flutter}}/tree/main/examples/platform_channel
[`/examples/platform_channel_swift/`]: {{site.repo.flutter}}/tree/main/examples/platform_channel_swift
[JS interoperability]: {{site.dart-site}}/web/js-interop
[`JSONMessageCodec`]: {{site.api}}/flutter/services/JSONMessageCodec-class.html
[`MethodChannel`]: {{site.api}}/flutter/services/MethodChannel-class.html
[`MethodChannelAndroid`]: {{site.api}}/javadoc/io/flutter/plugin/common/MethodChannel.html
[`MethodChanneliOS`]: {{site.api}}/objcdoc/Classes/FlutterMethodChannel.html
[Platform adaptations]: {{site.url}}/platform-integration/platform-adaptations
[publishing packages]: {{site.url}}/packages-and-plugins/developing-packages#publish
[`quick_actions`]: {{site.pub}}/packages/quick_actions
[section on threading]: #channels-and-platform-threading
[`StandardMessageCodec`]: {{site.api}}/flutter/services/StandardMessageCodec-class.html
[`StringCodec`]: {{site.api}}/flutter/services/StringCodec-class.html
[the main thread]: {{site.apple-dev}}/documentation/uikit?language=objc
[the UI thread]: {{site.android-dev}}/guide/components/processes-and-threads#Threads
[sending structured typesafe messages]: #pigeon
