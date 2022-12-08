---
title: Report errors to a service
title: 把報錯資訊透過服務上傳
description: How to keep track of errors that users encounter.
description: 如何持續收集報錯資訊。
tags: cookbook, 實用課程
keywords: Flutter收集錯誤資訊
prev:
  title: Work with long lists
  title: 長列表的處理
  path: /docs/cookbook/lists/long-lists
next:
  title: Animate a widget across screens
  title: 跨頁面切換的動效 Widget (Hero animations)
  path: /docs/cookbook/navigation/hero-animations
---

<?code-excerpt path-base="cookbook/maintenance/error_reporting/"?>

While one always tries to create apps that are free of bugs,
they're sure to crop up from time to time.
Since buggy apps lead to unhappy users and customers,
it's important to understand how often your users
experience bugs and where those bugs occur.
That way, you can prioritize the bugs with the
highest impact and work to fix them.

開發者總是試圖創造沒有 bug 的應用，但是 bug 還是會時不時地出現。
這些 bug 會給使用者帶來糟糕的體驗，
所以獲知 bug 發生的位置以及出現的頻率就顯得極為關鍵了。
這樣，你就可以根據 bug 的影響程度優先修復它們。

How can you determine how often your users experiences bugs?
Whenever an error occurs, create a report containing the
error that occurred and the associated stacktrace.
You can then send the report to an error tracking
service, such as [Bugsnag][], Fabric, [Firebase Crashlytics][], [Rollbar][], or Sentry.

如何確定使用者遇到 bug 的頻率呢？
解決方案是：當例外發生時，產生一份日誌，日誌中包含發生的例外及相關的堆疊資訊。
隨後，可以將日誌傳送到例外監控報警平台，比如
[Bugsnag][]、Fabric、[Firebase Crashlytics][Firebase Crashlytics CN]、
[Rollbar][] 或 Sentry。

The error tracking service aggregates all of the crashes your users
experience and groups them together. This allows you to know how often your
app fails and where the users run into trouble.

例外監控報警平台會將上報的崩潰日誌例外資訊聚合並分組歸類，
這樣就可以知道應用程式出現例外的頻率並定位例外發生位置。

In this recipe, learn how to report errors to the
[Sentry][] crash reporting service using
the following steps:

這個章節中，你可以透過以下步驟學習如何把例外資訊
上報給例外監控報警平台 [Sentry][]：

  1. Get a DSN from Sentry.

     從 Sentry 平台獲取 DSN
     
  2. Import the Flutter Sentry package

     匯入 Sentry package

  3. Initialize the Sentry SDK

     初始化 Sentry SDK

  4. Capture errors programmatically

     捕獲並上報例外

## 1. Get a DSN from Sentry

## 1. 從 Sentry 平台獲取 DSN

Before reporting errors to Sentry, you need a "DSN" to uniquely identify
your app with the Sentry.io service.

在向 Sentry 上報例外資訊前，需要在 Sentry.io 上獲取應用的唯一身份標識 DSN。

To get a DSN, use the following steps:

根據以下步驟，獲取 DSN：

  1. [Create an account with Sentry][].

     [建立 Sentry 賬戶][Create an account with Sentry]

  2. Log in to the account.

     登入賬戶

  3. Create a new Flutter project.

     新建一個 Flutter 工程

  4. Copy the code snippet that includes the DSN.

     複製包含 DSN 的程式碼片段

## 2. Import the Sentry package

## 2. 匯入 Sentry 包

Import the [`sentry_flutter`][] package into the app.
The sentry package makes it easier to send
error reports to the Sentry error tracking service.

匯入 [`sentry_flutter`][] package 到應用中，
這個 package 能更方便的將錯誤報告發送給
Sentry 的錯誤追蹤系統。

```yaml
dependencies:
  sentry_flutter: <latest_version>
```

## 3. Initialize the Sentry SDK

## 3. 建立 `SentryClient`

Initialize the SDK to capture different unhandled errors automatically:

初始化 SDK 來自動捕獲不同的未處理的錯誤。

<?code-excerpt "lib/main.dart (InitializeSDK)"?>
```dart
import 'package:flutter/widgets.dart';
import 'package:sentry_flutter/sentry_flutter.dart';

Future<void> main() async {
  await SentryFlutter.init(
    (options) => options.dsn = 'https://example@sentry.io/example',
    appRunner: () => runApp(const MyApp()),
  );
}
```

Alternatively, you can pass the DSN to Flutter using the `dart-define` tag:

另外，你也可以使用 `dart-define` 標記將 DSN 傳遞給 Flutter。

<!-- skip -->
```sh
--dart-define SENTRY_DSN=https://example@sentry.io/example
```

### What does that give me?

### 這番操作都發生了什麼

This is all you need for Sentry to capture unhandled errors in Dart and native layers.  
This includes Swift, Objective-C, C, and C++ on iOS, and Java, Kotlin, C, and C++ on Android.

使用 Sentry 捕獲 Dart 和原生中未處理的錯誤，以上這些操作就足夠了。
這包括 iOS 上的 Swift、Objective-C、C 和 C++，
以及 Android 上的 Java、Kotlin、C 和 C++。

## 4. Capture errors programatically

## 4. 捕獲並上報例外

Besides the automatic error reporting that Sentry generates by
importing and initializing the SDK,
you can use the API to report errors to Sentry:

除了自動初始化 Sentry SDK 來捕獲和上報錯誤之外，
你還可以透過 API 來報告錯誤：

<?code-excerpt "lib/main.dart (CaptureException)"?>
```dart
await Sentry.captureException(exception, stackTrace: stackTrace);
```

For more information, see the [Sentry API][] docs on pub.dev.

更多相關資訊，請參閱 pub.dev 上的 [Sentry API][] 文件。

## Learn more

## 瞭解更多

Extensive documentation about using the Sentry SDK can be found on [Sentry's site][].

更多關於使用 Sentry SDK 的文件可以在 [其官網][Sentry's site] 檢視。

## Complete example

## 完整範例

To view a working example,
see the [Sentry flutter example][] app.

檢視 [Sentry flutter example][] 範例應用，體驗完整流程。


[Sentry flutter example]: {{site.github}}/getsentry/sentry-dart/tree/main/flutter/example
[Create an account with Sentry]: https://sentry.io/signup/
[Bugsnag]: https://www.bugsnag.com/platforms/flutter
[Rollbar]: https://rollbar.com/
[Sentry]: https://sentry.io/welcome/
[`sentry_flutter`]: {{site.pub-pkg}}/sentry_flutter
[Sentry API]: {{site.pub-api}}/sentry_flutter/latest/sentry_flutter/sentry_flutter-library.html
[Sentry's site]: https://docs.sentry.io/platforms/flutter/
[Firebase Crashlytics]: https://firebase.google.com/docs/crashlytics
[Firebase Crashlytics CN]: https://firebase.google.cn/docs/crashlytics
