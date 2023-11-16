---
title: Debugging Flutter apps
title: 除錯 Flutter 應用
description: How to debug your app using the DevTools suite.
description: 如何使用開發者工具來除錯你的 Flutter 應用。
tags: Flutter測試
keywords: Flutter除錯工具,Flutter開發者工具,Flutter效能除錯,Flutter打斷點
---

<?code-excerpt path-base="testing/debugging"?>

There's a wide variety of tools and features to help debug
Flutter applications. Here are some of the available tools:

有很多工具和特性可以幫助除錯 Flutter 應用程式，如下列舉了一些：

* [DevTools][], a suite of performance and profiling
  tools that run in a browser.

  [開發者工具][DevTools]，是一套執行在瀏覽器的效能及分析工具。

* [Android Studio/IntelliJ][], and [VS Code][]
  (enabled with the Flutter and Dart plugins)
  support a built-in source-level debugger with
  the ability to set breakpoints, step through code,
  and examine values.

  [Android Studio/IntelliJ][] 和 [VS Code][]（藉助 Flutter 和 Dart 外掛）
  支援內建的原始碼偵錯程式，可以設定斷點，單步除錯，檢查數值。

* [Flutter inspector][], a widget inspector available
  in DevTools, and also directly from Android Studio
  and IntelliJ (enabled with the Flutter plugin).
  The inspector allows you to examine a visual
  representation of the widget tree, inspect
  individual widgets and their property values,
  enable the performance overlay, and more.
* If you are looking for a way to use GDB to remotely debug the
  Flutter engine running within an Android app process,
  check out [`flutter_gdb`][].

[`flutter_gdb`]: https://github.com/flutter/engine/blob/main/sky/tools/flutter_gdb

  [Flutter inspector][]，是開發者工具提供的 widget 檢查器，
  也可直接在 Android Studio 和 IntelliJ 中使用（藉助 Flutter 外掛）。
  檢查器可以視覺化展現 widget 樹，檢視單個 widget 及其屬性值，開啟效能圖層，等等。

## DevTools

## 開發者工具

For debugging and profiling apps, DevTools might be
the first tool you reach for. DevTools runs in a
browser and supports a variety of features:

要除錯及分析應用，開發者工具可能是你的首選。
開發者工具執行在瀏覽器，支援以下特性：

* source-level debugger

  原始碼偵錯程式

* widget inspector that displays a visual widget tree,
  and "widget select" mode where you select a widget
  in the app and it drills down to that widget in
  the tree

  widget 檢查器，展示視覺化的 widget 樹；
  “widget select” 模式，在應用中選擇一個 widget，會在 widget 樹直接定位到它的位置。

* memory profiler

  記憶體分析

* timeline view that supports tracing, and importing
  and exporting trace information

  時間線檢視，支援追蹤，匯入及匯出追蹤資訊

* logging view

  日誌檢視

If you run your application in [debug mode][] or
[profile mode][], while it's running you can open
DevTools in the browser to connect to your app.
DevTools doesn't work well with an app compiled to
[release mode][], as the debugging and profiling
information has been stripped away.

如果你在 [debug 模式][debug mode] 或
[profile 模式][profile mode] 執行，
那麼可以在瀏覽器開啟開發者工具連線到你的應用。
開發者工具不能用在以 [release 模式][release mode]
編譯的應用，因為除錯和分析資訊都被刪除了。

If you use DevTools for profiling, make sure to
run your application in [profile mode][]. Otherwise,
the main output that appears on your profile are the
debug asserts verifying the framework's various invariants
(see [Debug mode assertions](#debug-mode-assertions)).

如果你要用開發者工具分析應用，需確保使用 [效能模式][profile mode]。
否則，分析的主要輸出將會是用於驗證框架中各種不變式的除錯斷言
（檢視 [debug 模式斷言](#debug-mode-assertions)）。

![GIF showing DevTools features]({{site.url}}/assets/images/docs/tools/devtools/inspector.gif){:width="100%"}

For more information, see the
[DevTools][] documentation.

想獲取更多資訊，請檢視 [開發者工具][DevTools] 文件。

## Setting breakpoints

## 設定斷點

You can set breakpoints directly in your IDE/editor
(such as [Android Studio/IntelliJ][] and [VS Code][]),
in the [DevTools debugger][],
or [programmatically][breakpoints].

要設定斷點，可以直接在 IDE 或編輯器
（比如 [Android Studio/IntelliJ][] 和 [VS Code][]）、
[開發者工具偵錯程式][DevTools debugger] 設定，
或者 [透過編碼的方式設定][breakpoints]。

## The Dart analyzer

## Dart 分析器

If you're using a [Flutter enabled IDE/editor][],
the Dart analyzer is already checking your code
and looking for possible mistakes.

如果你使用的是 [Flutter 推薦的 IDE 或編輯器][Flutter enabled IDE/editor]，
則自帶的 Dart 分析器預設會檢查程式碼，平行處理現可能的錯誤。

If you run from the command line,
test your code with `flutter analyze`.

如果你使用命令列，則可以使用 `flutter analyze` 檢查程式碼。

The Dart analyzer makes heavy use of type annotations that
you put in your code to help track problems down.
You are encouraged to use them everywhere (avoiding `var`,
untyped arguments, untyped list literals, and so on)
as this is the quickest and least painful way of tracking
down problems.

Dart 分析器非常依賴你在程式碼中新增的型別註解，以幫助追蹤問題。
建議您在各個地方都加上註解（避免 `var`，無型別引數，無型別 list 字面量，等等），
因為這是追蹤問題最快且最不痛苦的方式。

For more information, see [Using the Dart analyzer][].

想獲取更多資訊，請檢視 [使用 Dart 分析器][Using the Dart analyzer]。

## Logging

## 日誌

Another useful debugging tool is logging.
You set logging up [programmatically][logging]
then view the output in the DevTools
[logging view][], or in the console.

另一個有用的除錯工具是日誌。
透過 [編碼][logging] 配置日誌，然後在開發者工具中的
[日誌檢視][logging view] 或控制檯檢視輸出。

## Debugging application layers

## 除錯應用層

Flutter was designed with a layered architecture that includes
widget, rendering, and painting layers. For links to more
information and videos, see [The Framework architecture][] on the
[GitHub wiki][], and the community article, [The Layer Cake][].

Flutter 採用分層架構，包括 widget、渲染和繪製等層。
想獲取更多資訊和影片，請檢視 [GitHub wiki][] 上的
[The Framework architecture][]，和社群文章 [The Layer Cake][]。

The Flutter widget inspector provides a visual representation
of the widget tree, but if you want a greater level of detail,
or you want a verbose text-based dump of the widget,
layer, or render trees, see
[Debug flags: application layers][]
in the [Debugging Flutter apps programmatically][] page.

Flutter widget 檢查器提供了 widget 樹的視覺展現，如果你想要更多細節，
或關於 wiget、層級或渲染樹的詳盡文字轉儲，請檢視 
[新增輸出程式碼的方式除錯 Flutter 應用][Debugging Flutter apps programmatically] 
頁面的 [除錯標誌：應用層][Debug flags: application layers] 部分。

## Debug mode assertions

## Debug 模式斷言

During development, you are highly encouraged to use Flutter's
[debug mode][]. This is the default if you use bug icon in
Android Studio, or `flutter run` at the command line.
Some tools support assert statements through the
command-line flag `--enable-asserts`.

在開發過程中，強烈建議您使用 Flutter 的 [debug 模式][debug mode]。
如果你是用 Android Studio 的 bug 圖示執行，或者在命令列執行 `flutter run`，則預設會使用 debug 模式。
有些工具透過 `--enable-assets` 命令列標誌可以支援斷言陳述式。

In this mode, Dart assert statements are enabled,
and the Flutter framework evaluates the argument
to each assert statement encountered during execution,
throwing an exception if the result is false.
This allows developers to enable or disable invariant
checking, such that the associated performance cost
is only paid during debugging sessions.

在此模式，Dart 斷言陳述式被開啟，
Flutter 框架在執行時會計算每一個遇到的斷言陳述式的引數，
當結果是 false 時丟擲例外。
如此一來，開發者可以控制不變式檢查的開啟或關閉，
相應的效能損耗將只發生在除錯期間。

When an invariant is violated, it's reported to the
console, with some context information to help track
down the source of the problem.

有不變式被違反時，它會被報告給控制檯，並攜帶一些幫助追蹤問題源的上下文資訊。

For more information, check out [Assert][] in the
[Dart language documentation][].

想獲取更多資訊，請檢視 [Dart 文件網站][Dart language documentation]
中的 [斷言][Assert] 部分。

## Debugging animations

## 除錯動畫

The easiest way to debug animations is to slow them down.
The [Flutter inspector][] provides a **Slow Animations** button,
or you can [slow the animations programmatically][].

除錯動畫最簡單的方法是讓它們變慢。
[Flutter inspector][] 提供一個 **放慢動畫**(Slow Animations)
的按鈕，你也可以 [在程式碼中放慢動畫][slow the animations programmatically]。

For more information on debugging janky (non-smooth)
applications, see [Flutter performance profiling][].

想獲取更多關於除錯動畫卡頓的資訊，請檢視
[Flutter 效能分析][Flutter performance profiling]。

## Measuring app startup time

## 測量應用啟動時間

To gather detailed information about the time it takes for your
Flutter app to start, you can run the `flutter run` command
with the `trace-startup` and `profile` options.

要收集有關 Flutter 應用程式啟動所需時間的詳細資訊，
可以在執行 `flutter run` 時使用 `trace-startup` 和 `profile` 選項。

```
$ flutter run --trace-startup --profile
```

The trace output is saved as a JSON file called `start_up_info.json`
under the `build` directory of your Flutter project.
The output lists the elapsed time from app startup to these trace
events (captured in microseconds):

追蹤輸出被儲存到 Flutter 工程目錄在 `build` 目錄下，
一個名為 `start_up_info.json` 的 JSON 檔案中。
輸出列出了從應用程式啟動到這些追蹤事件（以微秒捕獲）所用的時間：

+ Time to enter the Flutter engine code.
  
  進入 Flutter 引擎時
  
+ Time to render the first frame of the app.

  展示應用第一幀時

+ Time to initialize the Flutter framework.

  初始化Flutter框架時

+ Time to complete the Flutter framework initialization.

  完成Flutter框架初始化時

For example:

例如：

```
{
  "engineEnterTimestampMicros": 96025565262,
  "timeToFirstFrameMicros": 2171978,
  "timeToFrameworkInitMicros": 514585,
  "timeAfterFrameworkInitMicros": 1657393
}
```

## Tracing Dart code

## 追蹤 Dart 程式碼效能

To perform a performance trace,
you can use the DevTools [Timeline view][].
The Timeline view also supports importing
and exporting trace files. For more
information, see the [Timeline view][] docs.

要進行效能追蹤，你可以使用開發者工具的
[時間線檢視][Timeline view]。
時間線檢視還支援匯入和匯出追蹤檔案。
想要獲取更多資訊，請檢視 [時間線檢視][Timeline view]。

You can also
[perform traces programmatically][],
though these traces can't be imported
into DevTool's Timeline view.

你也可以 [在程式碼中追蹤][perform traces programmatically]，
不過這些追蹤資訊無法匯入到開發者模式的時間線檢視。

Be sure to use run your app in [profile mode][]
before tracing to ensure that the runtime performance
characteristics closely matches that of your final product.

追蹤時請確保在 [效能模式][profile mode] 執行應用，
這樣才能保證執行時效能特徵同你最終產品高度一致。

## Performance overlay

## 效能圖層

To get a graphical view of the performance of your application,
turn on the performance overlay. You can do this in the
by clicking the **Performance Overlay** button in the
[Flutter inspector][].

要圖形化展現你應用的效能，可以開啟效能圖層。
你可以在 [Flutter inspector][] 中點選 **Performance Overlay** 按鈕。

You can also turn on the overlay [programmatically][overlay].

你也可以 [在程式碼中][overlay] 開啟該圖層。

For information on how to interpret the graphs in the overlay,
see [The performance overlay][] in
the [Flutter performance profiling][] guide.

關於如何解析圖層中的圖形，請檢視
[Flutter 效能分析][Flutter performance profiling]
中的 [效能圖層][The performance overlay] 部分。

## Debug flags

## 除錯標誌

In most cases, you won't need to use the debug flags
directly, as you'll find the most useful debugging
functionality in the [DevTools][] suite. But if you
prefer to use the debug flags directly, see
[Debug flags: performance][] in the
[Debugging Flutter apps programmatically][] page.

大部分情況，你不需要直接使用除錯標誌，因為可以在
[開發者工具][DevTools] 找到最有用的除錯功能。
但是如果你偏好直接使用除錯標誌，請檢視
[新增輸出程式碼的方式除錯 Flutter 應用][Debugging Flutter apps programmatically]
中的 [除錯標誌：效能][Debug flags: performance] 部分。

## Common problems

## 常見問題

The following is a problem that some have encountered on MacOS.

下面是一些在 macOS 上遇到的問題。

### "Too many open files" exception (MacOS)

### "控制代碼數超出系統限制" 例外 (macOS)

The default limit for Mac OS on how many files it can have open at a
time is rather low.  If you run into this limit,
increase the number of available
file handlers using the `ulimit` command:

mac OS 在同一時間可以開啟多少控制代碼的預設限制數相當低。如果你達到這個極限，
可以用 `ulimit` 命令增加可用控制代碼的數量：

```
ulimit -S -n 2048
```

If you use Travis or Cirrus for testing, increase the number of
available file handlers that they can open by adding the same line to
flutter/.travis.yml, or flutter/.cirrus.yml, respectively.

如果您使用 Travis 或 Cirrus 進行測試，
請透過在 flutter/.travis.yml 或 flutter/.cirrus.yml 
中增加同樣的命令來增加它們可以開啟的控制代碼數量。

### Widgets marked const that should be equal to each other, aren't

### 被標記為 const 的相同 Widget 應被視為同一物件，然而卻並沒有

In debug mode, you might find that two `const` widgets that should to all
appearances be equal (because of Dart's constant deduplication) are not.

在 debug 模式下，（由於 Dart 的常量去重策略）你也許會發現兩個 `const` 的 widget 長得並不完全一樣。

For example, this code should print 1:

例如，下面的程式碼應該列印 1：

<?code-excerpt "lib/main.dart (Syntax)"?>
```dart
print(<Widget>{
  // this is the syntax for a Set<Widget> literal
  const SizedBox(),
  const SizedBox(),
}.length);
```

It should print 1 (rather than 2) because the two constants are the same and sets
coalesce duplicate values (and indeed the analyzer complains that
"Two elements in a set literal shouldn't be equal"). As expected, in release
builds, it does print 1. However, in debug builds it prints 2. This is because the
flutter tool injects the source location of Widget constructors into the code at compile
time, so the code is effectively:

這段程式碼應該列印 1（而不是 2），這是由於兩個常量相同且在同一個 set 中（實際上分析器抱怨
“集合文字中的兩個元素不應相等”）。正如我們所期待的那樣，在 release 模式下建構的時候，它確實列印了 1。
然而，在 debug 模式下它卻列印了 2。這是由於 Flutter tool 在編譯期向 Widget 的構造器注入了源位置，
所以下面的程式碼有效：

<?code-excerpt "lib/main.dart (SyntaxExplain)"?>
```dart
print(<Widget>{
  const SizedBox(/* location: Location(file: 'foo.dart', line: 12) */),
  const SizedBox(/* location: Location(file: 'foo.dart', line: 13) */),
}.length);
```

This results in the instances being different, and so they are not deduplicated by the set.
We use this injected information to make the error messages clearer when
a widget is involved in an exception, by reporting where the relevant widget was created.
Unfortunately, it has the visible side-effect of making otherwise-identical constants be
different at compile time.

上面的程式碼在結果中的例項不同，故它們在 set 中並沒有重複。
我們使用注入資訊彙報相關 widget 的建立資訊，
使得 widget 出現例外時錯誤訊息會更加清晰。
不幸的是，它會導致相同常量在編譯期變為不同例項。

To disable this behavior, pass `--no-track-widget-creation` to the `flutter run` command.
With that flag set, the code above prints "1" in debug and release builds, and error messages
include a message saying that they cannot provide all the information that they would otherwise
be able to provide if widget creation tracking was enabled.

要關閉此行為，請在執行 `flutter run` 命令的同時傳 `--no-track-widget-creation`。
有了這個標記，程式碼將會在 debug 和 release 模式下列印 1，
而錯誤訊息這邊會有一條訊息說，
除非開啟 widget 建立追蹤器，否則我們將無法提供完整的資訊。

See also:

你也可以檢視：

 * Our documentation on [how the Widget Inspector uses widget creation tracking][].

   文件 [Widget Inspector 是如何使用 widget 建立追蹤的][how the
   Widget Inspector uses widget creation tracking]。

 * [WidgetInspectorService.isWidgetCreationTracked][].

 * The `_Location` class in [widget_inspector.dart][].

   [widget_inspector.dart][] 中的 `_Location` 類別。

 * The [kernel transform that implements this feature][].

   [實現該功能的核心轉換][kernel transform that implements this feature]。

## Other resources

## 其他資源

You might find the following docs useful:

以下是其他一些有用的文件：

* [Performance best practices][]

  [效能最佳化最佳實踐][Performance best practices]

* [Flutter performance profiling][]

  [Flutter 效能分析][Flutter performance profiling]

* [Use a native debugger][]

  [使用原生的偵錯程式][Use a native debugger]

* [Flutter's modes][]

  [Flutter 建構模式][Flutter's modes]

* [Debugging Flutter apps programmatically][]

  [新增輸出程式碼的方式除錯 Flutter 應用][Debugging Flutter apps programmatically]

* [DevTools][]

  [開發者工具][DevTools]

* [Android Studio/IntelliJ][]

* [VS Code][]


[Flutter enabled IDE/editor]: {{site.url}}/get-started/editor

[Debugging Flutter apps programmatically]: {{site.url}}/testing/code-debugging
[perform traces programmatically]: {{site.url}}/testing/code-debugging#tracing-dart-code-performance
[Debug flags: application layers]: {{site.url}}/testing/code-debugging#debug-flags-application-layers
[Debug flags: performance]: {{site.url}}/testing/code-debugging#debug-flags-performance
[slow the animations programmatically]: {{site.url}}/testing/code-debugging#debugging-animations
[breakpoints]: {{site.url}}/testing/code-debugging#setting-breakpoints
[logging]: {{site.url}}/testing/code-debugging#logging
[Flutter's modes]: {{site.url}}/testing/build-modes
[Flutter performance profiling]: {{site.url}}/perf/ui-performance
[Performance best practices]: {{site.url}}/perf/best-practices
[Use a native debugger]: {{site.url}}/testing/native-debugging

[The Layer Cake]: {{site.medium}}/flutter-community/the-layer-cake-widgets-elements-renderobjects-7644c3142401

[GitHub wiki]: {{site.repo.flutter}}/wiki/
[Using the Dart analyzer]: {{site.repo.flutter}}/wiki/Using-the-Dart-analyzer
[The Framework architecture]: {{site.repo.flutter}}/wiki/The-Framework-architecture

[Android Studio/IntelliJ]: {{site.url}}/tools/android-studio#run-app-with-breakpoints
[VS Code]: {{site.url}}/tools/vs-code#run-app-with-breakpoints
[DevTools]: {{site.url}}/tools/devtools
[Flutter inspector]: {{site.url}}/tools/devtools/inspector
[DevTools debugger]: {{site.url}}/tools/devtools/debugger
[logging view]: {{site.url}}/tools/devtools/logging
[Timeline view]: {{site.url}}/tools/devtools/performance
[The performance overlay]: {{site.url}}/perf/ui-performance#the-performance-overlay
[Flutter performance profiling]: {{site.url}}/perf/ui-performance
[overlay]: {{site.url}}/testing/code-debugging#performance-overlay
[debug mode]: {{site.url}}/testing/build-modes#debug
[profile mode]: {{site.url}}/testing/build-modes#profile
[release mode]: {{site.url}}/testing/build-modes#release
[how the Widget Inspector uses widget creation tracking]: {{site.url}}/tools/devtools/inspector#track-widget-creation

[Assert]: {{site.dart-site}}/language/control-flow#assert
[Dart language documentation]: {{site.dart-site}}/language

[WidgetInspectorService.isWidgetCreationTracked]: {{site.api}}/flutter/widgets/WidgetInspectorService/isWidgetCreationTracked.html
[widget_inspector.dart]: {{site.repo.flutter}}/blob/master/packages/flutter/lib/src/widgets/widget_inspector.dart
[kernel transform that implements this feature]: {{site.github}}/dart-lang/sdk/blob/main/pkg/kernel/lib/transformations/track_widget_constructor_locations.dart
