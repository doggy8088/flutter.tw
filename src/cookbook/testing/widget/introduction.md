---
title: An introduction to widget testing
title: Widget 測試介紹
description: Learn more about widget testing in Flutter.
description: 瞭解更多 Flutter 的 widget 測試。
short-title: Introduction
tags: cookbook, 實用課程, 測試
keywords: widget測試,Matcher使用
prev:
  title: Mock dependencies using Mockito
  title: 使用 Mockito 模擬依賴關係
  path: /docs/cookbook/testing/unit/mocking
next:
  title: Find widgets
  title: 定位到目標 widgets
  path: /docs/cookbook/testing/widget/finders
---

<?code-excerpt path-base="cookbook/testing/widget/introduction/"?>

{% assign api = site.api | append: '/flutter' -%}

In the [introduction to unit testing][] recipe,
you learned how to test Dart classes using the `test` package.
To test widget classes, you need a few additional tools provided by the
[`flutter_test`][] package, which ships with the Flutter SDK.

在 [單元測試介紹][introduction to unit testing] 部分，
我們學習了使用 `test` 這個 package 測試 Dart 類別的方法。
為了測試 widget 類，我們需要使用 [`flutter_test`][] package 提供的額外工具，
這些工具是跟 Flutter SDK 一起釋出的。

The `flutter_test` package provides the following tools for
testing widgets:

`flutter_test` package 提供了以下工具用於 widget 的測試：

  * The [`WidgetTester`][] allows building and interacting
    with widgets in a test environment.
  
    [`WidgetTester`][]，使用該工具可在測試環境下建立 widget 並與其互動。
  
  * The [`testWidgets()`][] function automatically
    creates a new `WidgetTester` for each test case,
    and is used in place of the normal `test()` function.
  
    [`testWidgets()`][] 函式，此函式會自動為每個測試建立一個 `WidgetTester`，
    用來代替普通的 `test` 函式。
     
  * The [`Finder`][] classes allow searching for widgets
    in the test environment.
  
    [`Finder`][] 類，可以方便我們在測試環境下查詢 widgets。
    
  * Widget-specific [`Matcher`][] constants help verify
   whether a `Finder` locates a widget or
    multiple widgets in the test environment.
  
    Widget-specific [`Matcher`][] 常量，
    該常量在測試環境下幫助我們驗證 `Finder` 是否定位到一個或多個 widgets。

If this sounds overwhelming, don't worry. Learn how all of these pieces fit
together throughout this recipe, which uses the following steps:

如果覺得太複雜，別擔心！讓我們透過下面這些步驟把這些內容整合起來。

### Directions

### 步驟：

  1. Add the `flutter_test` dependency.
  
     新增一個 `flutter_test` 依賴
     
  2. Create a widget to test.
  
     建立一個測試用的 widget
  
  3. Create a `testWidgets` test.
  
     建立一個 `testWidgets` 測試方法
     
  4. Build the widget using the `WidgetTester`.
  
     使用 `WidgetTester` 建立 widget
     
  5. Search for the widget using a `Finder`.
  
     使用 `Finder` 查詢 widget
     
  6. Verify the widget using a `Matcher`.
  
     使用 `Matcher` 驗證 widget 是否正常工作

### 1. Add the `flutter_test` dependency

### 一. 新增一個 `flutter_test` 依賴

Before writing tests, include the `flutter_test`
dependency in the `dev_dependencies` section of the `pubspec.yaml` file.
If creating a new Flutter project with the command line tools or
a code editor, this dependency should already be in place.

我們開始編寫測試之前，需要先給 `pubspec.yaml` 檔案的 `dev_dependencies` 段
新增 `flutter_test` 依賴。如果使用命令列或編譯器新建一個 Flutter 專案，
那麼依賴已經預設添加了。

```yaml
dev_dependencies:
  flutter_test:
    sdk: flutter
```

### 2. Create a widget to test

### 二. 建立一個測試用的 Widget

Next, create a widget for testing. For this recipe,
create a widget that displays a `title` and `message`.

接下來，我們需要建立一個可以測試的 widget！在此例中，
我們建立了一個 widget 顯示一個 `標題 (title)` 和 `資訊 (message)`。

<!-- skip -->
<?code-excerpt "test/main_test.dart (widget)"?>
```dart
class MyWidget extends StatelessWidget {
  const MyWidget({
    super.key,
    required this.title,
    required this.message,
  });

  final String title;
  final String message;

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: Center(
          child: Text(message),
        ),
      ),
    );
  }
}
```

### 3. Create a `testWidgets` test

### 三. 建立一個 `testWidgets` 測試方法

With a widget to test, begin by writing your first test.
Use the [`testWidgets()`][] function provided by the
`flutter_test` package to define a test.
The `testWidgets` function allows you to define a
widget test and creates a `WidgetTester` to work with.

現在我們有了一個可以測試的 widget，可以開始編寫第一個測試了！
第一步，我們用 `flutter_test` 這個 package 提供的
[`testWidgets()`][] 函式定義一個測試。
`testWidgets` 函式可以定義一個 widget 測試並建立一個可以使用的 `WidgetTester`。

This test verifies that `MyWidget` displays a given title and message.
It is titled accordingly, and it will be populated in the next section.

我們的測試會驗證 `MyWidget` 是否顯示給定的標題和資訊。

<!-- skip -->
<?code-excerpt "test/main_step3_test.dart (main)"?>
```dart
void main() {
  // Define a test. The TestWidgets function also provides a WidgetTester
  // to work with. The WidgetTester allows you to build and interact
  // with widgets in the test environment.
  testWidgets('MyWidget has a title and message', (tester) async {
    // Test code goes here.
  });
}
```

### 4. Build the widget using the `WidgetTester`

### 四. 使用 `WidgetTester` 建立 Widget

Next, build `MyWidget` inside the test environment by using the
[`pumpWidget()`][] method provided by `WidgetTester`.
The `pumpWidget` method builds and renders the provided widget.

下一步，為了在測試環境中建立 `MyWidget`，我們可以使用 `WidgetTester` 提供的
[`pumpWidget()`][]方法，`pumpWidget` 方法會建立並渲染我們提供的 widget。

Create a `MyWidget` instance that displays "T" as the title
and "M" as the message.

在這個範例中，我們將建立一個顯示標題“T”和資訊“M”的 `MyWidget` 範例。

<!-- skip -->
<?code-excerpt "test/main_step4_test.dart (main)"?>
```dart
void main() {
  testWidgets('MyWidget has a title and message', (tester) async {
    // Create the widget by telling the tester to build it.
    await tester.pumpWidget(const MyWidget(title: 'T', message: 'M'));
  });
}
```

#### Notes about the pump() methods

#### pump() 方法相關提示

After the initial call to `pumpWidget()`, the `WidgetTester` provides
additional ways to rebuild the same widget. This is useful if you're
working with a `StatefulWidget` or animations.

初次呼叫 `pumpWidget()` 之後，`WidgetTester` 會提供其他方式來重建相同的 widget。
這對使用 `StatefulWidget` 或者動畫會非常有用。

For example, tapping a button calls `setState()`, but Flutter won't
automatically rebuild your widget in the test environment.
Use one of the following methods to ask Flutter to rebuild the widget.

例如，如果我們點選呼叫 `setState()` 的按鈕，在測試環境中，Flutter 
並不會自動重建你的 widget。我們需要用以下列舉的方法來讓 Flutter
再一次建立我們的 widget。

[`tester.pump(Duration duration)`][]
<br> Schedules a frame and triggers a rebuild of the widget.
  If a `Duration` is specified, it advances the clock by
  that amount and schedules a frame. It does not schedule
  multiple frames even if the duration is longer than a
  single frame.

[`tester.pump(Duration duration)`][]
<br> 排程一幀以觸發 widget 的重建。
  若指定了 `Duration`，那麼則會讓 clock 經過該時長之後排程一幀。
  它並不會因為持續時間大於一幀的時間而排程多幀。

{{site.alert.note}}

  To kick off the animation, you need to call `pump()`
  once (with no duration specified) to start the ticker.
  Without it, the animation does not start.

  要開始播放動畫，則需要呼叫一次 `pump()`（不指定 duration）以啟動 ticker。
  如果沒有這一步驟，動畫將不會執行。
  
{{site.alert.end}}

[`tester.pumpAndSettle()`][]
<br> Repeatedly calls `pump()` with the given duration until
  there are no longer any frames scheduled.
  This essentially waits for all animations to complete.
  
[`tester.pumpAndSettle()`][]
<br> 在給定期間內不斷重複呼叫 pump 直到完成所有繪製幀。
一般需要等到所有動畫全部完成。

These methods provide fine-grained control over the build lifecycle,
which is particularly useful while testing.

這些方法在建構週期中保證細粒度控制，這在測試中非常有用。

### 5. Search for our widget using a `Finder`

### 五. 使用 `Finder` 查詢 widget

With a widget in the test environment, search
through the widget tree for the `title` and `message`
Text widgets using a `Finder`. This allows verification that
the widgets are being displayed correctly.

現在讓我們在測試環境中建立 widget。我們需要用 `Finder` 透過 widget 樹來查詢
`標題` 和 `資訊` Text widgets，這樣可以驗證這些 Widgets 是否正確顯示。

For this purpose, use the top-level [`find()`][]
method provided by the `flutter_test` package to create the `Finders`.
Since you know you're looking for `Text` widgets, use the
[`find.text()`][] method.

為了實現這個目的，我們使用 `flutter_test` 這個 package 提供的最上層
[`find()`][] 方法來建立我們的 `Finders`。因為我們要查詢的是 `Text` widgets，
所以可以使用 [`find.text()`][] 方法。

For more information about `Finder` classes, see the
[Finding widgets in a widget test][] recipe.

關於 `Finder` classes 的更多資訊，
請參閱 [定位到目標 Widgets][Finding widgets in a widget test] 章節。

<!-- skip -->
<?code-excerpt "test/main_step5_test.dart (main)"?>
```dart
void main() {
  testWidgets('MyWidget has a title and message', (tester) async {
    await tester.pumpWidget(const MyWidget(title: 'T', message: 'M'));

    // Create the Finders.
    final titleFinder = find.text('T');
    final messageFinder = find.text('M');
  });
}
```

### 6. Verify the widget using a `Matcher`

### 六. 使用 `Matcher` 驗證 widget 是否正常工作

Finally, verify the title and message `Text` widgets appear on screen
using the `Matcher` constants provided by `flutter_test`.
`Matcher` classes are a core part of the `test` package,
and provide a common way to verify a given
value meets expectations.

最後，讓我們來用 `flutter_test` 提供的 `Matcher` 常量驗證 `Text` widgets
顯示的標題和資訊。`Matcher` 類是 `test` 包裡的核心部分，
它提供一種通用方法來驗證給定值是否符合我們的預期。

Ensure that the widgets appear on screen exactly one time.
For this purpose, use the [`findsOneWidget`][] `Matcher`.

在這個範例中，我們要確保 widget 只在螢幕中出現一次。
因此，可以使用 [`findsOneWidget`][] `Matcher`。

<!-- skip -->
<?code-excerpt "test/main_step6_test.dart (main)"?>
```dart
void main() {
  testWidgets('MyWidget has a title and message', (tester) async {
    await tester.pumpWidget(const MyWidget(title: 'T', message: 'M'));
    final titleFinder = find.text('T');
    final messageFinder = find.text('M');

    // Use the `findsOneWidget` matcher provided by flutter_test to verify
    // that the Text widgets appear exactly once in the widget tree.
    expect(titleFinder, findsOneWidget);
    expect(messageFinder, findsOneWidget);
  });
}
```

#### Additional Matchers

#### 其他的 Matchers

In addition to `findsOneWidget`, `flutter_test` provides additional
matchers for common cases.

除了 `findsOneWidget`，`flutter_test` 還為常見情況提供了其他的 matchers。

[`findsNothing`][]
<br> Verifies that no widgets are found.

[`findsNothing`][]
<br> 驗證沒有可被查詢的 widgets。

[`findsWidgets`][]
<br> Verifies that one or more widgets are found.

[`findsWidgets`][]
<br> 驗證一個或多個 widgets 被找到。

[`findsNWidgets`][]
<br> Verifies that a specific number of widgets are found.

[`findsNWidgets`][]
<br> 驗證特定數量的 widgets 被找到。

[`matchesGoldenFile`][]
<br> Verifies that a widget's rendering matches a particular bitmap image ("golden file" testing).

[`matchesGoldenFile`][]
<br> 驗證渲染的 widget 是否與特定的影象匹配（「目標檔案」測試）。

### Complete example

### 完整範例

<?code-excerpt "test/main_test.dart"?>
```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

void main() {
  // Define a test. The TestWidgets function also provides a WidgetTester
  // to work with. The WidgetTester allows building and interacting
  // with widgets in the test environment.
  testWidgets('MyWidget has a title and message', (tester) async {
    // Create the widget by telling the tester to build it.
    await tester.pumpWidget(const MyWidget(title: 'T', message: 'M'));

    // Create the Finders.
    final titleFinder = find.text('T');
    final messageFinder = find.text('M');

    // Use the `findsOneWidget` matcher provided by flutter_test to
    // verify that the Text widgets appear exactly once in the widget tree.
    expect(titleFinder, findsOneWidget);
    expect(messageFinder, findsOneWidget);
  });
}

class MyWidget extends StatelessWidget {
  const MyWidget({
    super.key,
    required this.title,
    required this.message,
  });

  final String title;
  final String message;

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: Center(
          child: Text(message),
        ),
      ),
    );
  }
}
```


[`find()`]: {{api}}/flutter_test/find-constant.html
[`find.text()`]: {{api}}/flutter_test/CommonFinders/text.html
[`findsNothing`]: {{api}}/flutter_test/findsNothing-constant.html
[`findsOneWidget`]: {{api}}/flutter_test/findsOneWidget-constant.html
[`findsNWidgets`]: {{api}}/flutter_test/findsNWidgets.html
[`findsWidgets`]: {{api}}/flutter_test/findsWidgets-constant.html
[`matchesGoldenFile`]: {{api}}/flutter_test/matchesGoldenFile.html
[`Finder`]: {{api}}/flutter_test/Finder-class.html
[Finding widgets in a widget test]: {{site.url}}/cookbook/testing/widget/finders
[`flutter_test`]: {{api}}/flutter_test/flutter_test-library.html
[introduction to unit testing]: {{site.url}}/cookbook/testing/unit/introduction
[`Matcher`]: {{api}}/package-matcher_matcher/Matcher-class.html
[`pumpWidget()`]: {{api}}/flutter_test/WidgetTester/pumpWidget.html
[`tester.pump(Duration duration)`]: {{api}}/flutter_test/TestWidgetsFlutterBinding/pump.html
[`tester.pumpAndSettle()`]: {{api}}/flutter_test/WidgetTester/pumpAndSettle.html
[`testWidgets()`]: {{api}}/flutter_test/testWidgets.html
[`WidgetTester`]: {{api}}/flutter_test/WidgetTester-class.html
