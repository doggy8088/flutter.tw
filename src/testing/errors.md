---
title: Handling errors in Flutter
title: 在 Flutter 裡處理錯誤
description: How to control error messages and logging of errors
description: 如何在 Flutter 裡處理和列印錯誤資訊
tags: Flutter測試
keywords: Flutter處理錯誤,Flutter錯誤報告,FlutterError
---

<?code-excerpt path-base="testing/errors"?>

The Flutter framework catches errors that occur during callbacks
triggered by the framework itself, including errors encountered
during the build, layout, and paint phases. Errors that don't occur
within Flutter's callbacks can't be caught by the framework,
but you can handle them by setting up a error handler on the
[`PlatformDispatcher`][].

Flutter 框架可以捕獲執行期間的錯誤，包括建構期間、佈局期間和繪製期間。

All errors caught by Flutter are routed to the
[`FlutterError.onError`][] handler. By default,
this calls [`FlutterError.presentError`][],
which dumps the error to the device logs.
When running from an IDE, the inspector overrides this
behavior so that errors can also be routed to the IDE's
console, allowing you to inspect the
objects mentioned in the message.

所有 Flutter 的錯誤均會被回呼(Callback)方法 [`FlutterError.onError`][] 捕獲。
預設情況下，會呼叫 [`FlutterError.presentError`][] 方法，
並將錯誤轉儲到當前的裝置日誌中。
當從 IDE 執行應用時，檢查器重寫了該方法，
錯誤也被髮送到 IDE 的控制檯，可以在控制檯中檢查出錯的物件。

{{site.alert.note}}

  Consider calling [`FlutterError.presentError`][]
  from your custom error handler in order to see
  the logs in the console as well.

  考慮從自訂錯誤處理程式呼叫 [`FlutterError.presentError`][]
  以檢視控制檯中的日誌。

{{site.alert.end}}

When an error occurs during the build phase,
the [`ErrorWidget.builder`][] callback is
invoked to build the widget that is used
instead of the one that failed. By default,
in debug mode this shows an error message in red,
and in release mode this shows a gray background.

當建構期間發生錯誤時，回呼(Callback)函式 [`ErrorWidget.builder`][] 會被呼叫，
來產生一個新的 widget，用來代替建構失敗的 widget。
預設情況，debug 模式下會顯示一個紅色背景的錯誤頁面，
release 模式下會展示一個灰色背景的空白頁面。

When errors occur without a Flutter callback on the call stack,
they are handled by the `PlatformDispatcher`'s error callback. By default,
this only prints errors and does nothing else.

如果在呼叫堆疊上沒有 Flutter 回呼(Callback)的情況下發生錯誤，
它們由發生區域的 `Zone` 處理。
`Zone` 在預設情況下僅會列印錯誤，而不會執行其他任何操作。

You can customize these behaviors,
typically by setting them to values in
your `void main()` function.

這些回呼(Callback)方法都可以被重寫，通常在 `void main()` 方法中重寫。

Below each error type handling is explained. At the bottom
there's a code snippet which handles all types of errors. Even
though you can just copy-paste the snippet, we recommend you
to first get acquainted with each of the error types.

下面解釋了所有的錯誤捕獲型別。
在最後的程式碼段可以用於處理所有型別的錯誤。
儘管你可以直接複製貼上程式碼段，但我們建議你先了解每種錯誤型別。


## Errors caught by Flutter

## Flutter 導致的錯誤

For example, to make your application quit immediately any time an
error is caught by Flutter in release mode, you could use the
following handler:

例如，如果你想在 release 模式下發生錯誤時立刻關閉應用，
可以使用下面的回呼(Callback)方法:

<?code-excerpt "lib/quit_immediate.dart (Main)"?>
```dart
import 'dart:io';

import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

void main() {
  FlutterError.onError = (details) {
    FlutterError.presentError(details);
    if (kReleaseMode) exit(1);
  };
  runApp(const MyApp());
}

// rest of `flutter create` code...
```
{{site.alert.note}}

  The top-level [`kReleaseMode`][] constant indicates
  whether the app was compiled in release mode.

  最上層的 [`kReleaseMode`][] 常數表示該應用是否在 release 模式下編譯。

{{site.alert.end}}

This handler can also be used to report errors to a logging service.
For more details, see our cookbook chapter for
[reporting errors to a service][].

這個回呼(Callback)方法也可以上報錯誤到日誌服務平台。更多資訊可以檢視文件
[報錯資訊透過服務上傳][reporting errors to a service]。

## Define a custom error widget for build phase errors

## 自訂一個 ErrorWidget 以展示 build 時的錯誤

To define a customized error widget that displays whenever
the builder fails to build a widget, use [`MaterialApp.builder`][].

定義一個自訂的 error widget，以當 builder 建構 widget 失敗時顯示，
請使用 [`MaterialApp.builder`][]。

<?code-excerpt "lib/excerpts.dart (CustomError)"?>
```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      builder: (context, widget) {
        Widget error = const Text('...rendering error...');
        if (widget is Scaffold || widget is Navigator) {
          error = Scaffold(body: Center(child: error));
        }
        ErrorWidget.builder = (errorDetails) => error;
        if (widget != null) return widget;
        throw ('widget is null');
      },
    );
  }
}
```

## Errors not caught by Flutter

## 未被 Flutter 捕獲的錯誤

Consider an `onPressed` callback that invokes an asynchronous function,
such as `MethodChannel.invokeMethod` (or pretty much any plugin).
For example:

假設一個 `onPressed` 回呼(Callback)呼叫了非同步方法，例如 `MethodChannel.invokeMethod`
（或者其他 plugin 的方法）：

<?code-excerpt "lib/excerpts.dart (OnPressed)" replace="/return //g;/\;//g"?>
```dart
OutlinedButton(
  child: const Text('Click me!'),
  onPressed: () async {
    const channel = MethodChannel('crashy-custom-channel')
    await channel.invokeMethod('blah')
  },
)
```

If `invokeMethod` throws an error, it won't be forwarded to `FlutterError.onError`.
Instead, it's forwarded to the `PlatformDispatcher`.

如果 `invokeMethod` 丟擲了錯誤，它不會傳遞至 `FlutterError.onError`，
而是直接進入 `runApp` 的 `Zone`。

To catch such an error, use [`PlatformDispatcher.instance.onError`][].

如果你想捕獲這樣的錯誤，請使用 [`PlatformDispatcher.instance.onError`][]。

<?code-excerpt "lib/excerpts.dart (CatchError)"?>
```dart
import 'package:flutter/material.dart';
import 'dart:ui';

void main() {
  MyBackend myBackend = MyBackend();
  PlatformDispatcher.instance.onError = (error, stack) {
    myBackend.sendError(error, stack);
    return true;
  };
  runApp(const MyApp());
}
```

## Handling all types of errors

## 處理所有型別的錯誤

Say you want to exit application on any exception and to display
a custom error widget whenever a widget building fails - you can base
your errors handling on next code snippet:

如果你想在例外丟擲時退出應用，並在 build 錯誤時展示自訂的 ErrorWidget，
你可以在下面的程式碼片段的基礎上客製你的處理：

<?code-excerpt "lib/main.dart (Main)"?>
```dart
import 'package:flutter/material.dart';
import 'dart:ui';

Future<void> main() async {
  await myErrorsHandler.initialize();
  FlutterError.onError = (details) {
    FlutterError.presentError(details);
    myErrorsHandler.onErrorDetails(details);
  };
  PlatformDispatcher.instance.onError = (error, stack) {
    myErrorsHandler.onError(error, stack);
    return true;
  };
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      builder: (context, widget) {
        Widget error = const Text('...rendering error...');
        if (widget is Scaffold || widget is Navigator) {
          error = Scaffold(body: Center(child: error));
        }
        ErrorWidget.builder = (errorDetails) => error;
        if (widget != null) return widget;
        throw ('widget is null');
      },
    );
  }
}
```

[`ErrorWidget.builder`]: {{site.api}}/flutter/widgets/ErrorWidget/builder.html
[`FlutterError.onError`]: {{site.api}}/flutter/foundation/FlutterError/onError.html
[`FlutterError.presentError`]: {{site.api}}/flutter/foundation/FlutterError/presentError.html
[`kReleaseMode`]:  {{site.api}}/flutter/foundation/kReleaseMode-constant.html
[`MaterialApp.builder`]: {{site.api}}/flutter/material/MaterialApp/builder.html
[reporting errors to a service]: {{site.url}}/cookbook/maintenance/error-reporting
[`PlatformDispatcher.instance.onError`]: {{site.api}}/flutter/dart-ui/PlatformDispatcher/onError.html
[`PlatformDispatcher`]: {{site.api}}/flutter/dart-ui/PlatformDispatcher-class.html
