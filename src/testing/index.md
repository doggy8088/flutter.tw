---
title: Testing Flutter apps
title: 測試 Flutter 應用
description: Learn more about the different types of testing and how to write them.
description: 學習不同型別的測試以及如何編寫它們。
tags: Flutter測試
keywords: 單元測試,Widget 測試,元件測試,整合測試,程式碼覆蓋率
---

The more features your app has, the harder it is to test manually.
Automated tests help ensure that your app performs correctly before
you publish it, while retaining your feature and bug fix velocity.

通常一個應用的功能越多，手工測試就越困難。
自動化測試在釋出之前執行，有助於保證我們應用的穩定性和功能的完整性，
並且可以快速修復問題。

{{site.alert.note}}

  For hands-on practice of testing Flutter apps, see the
  [How to test a Flutter app][] codelab.
  
  動手試試看測試你的 Flutter 應用？
  請關注 codelab：[如何測試一個 Flutter 應用][How to test a Flutter app]。
{{site.alert.end}}

Automated testing falls into a few categories:

自動化測試可分為以下幾類：

* A [_unit test_](#unit-tests) tests a single function, method, or class.
    
  [**單元測試**](#unit-tests) 測試單一的函式，方法或類別。

* A [_widget test_](#widget-tests) (in other UI frameworks referred to
  as _component test_) tests a single widget.

  [**Widget 測試**](#widget-tests)（在其他 UI 框架中指 **元件測試**）測試單一的 widget 。

* An [_integration test_](#integration-tests)
  tests a complete app or a large part of an app.

  [**整合測試**](#integration-tests) 測試一個完整的應用或者一個應用的大部分功能。
  
Generally speaking, a well-tested app has many unit and widget tests,
tracked by [code coverage][], plus enough integration tests
to cover all the important use cases. This advice is based on
the fact that there are trade-offs between different kinds of testing,
seen below.

一般來說，在自動化測試方面做的比較好的應用會有許多單元測試和 widget 測試，
並且使用 [程式碼覆蓋率][code coverage] 進行追蹤，
還會有足夠的整合測試來覆蓋所有的重要使用場景。
這樣做是因為不同型別的測試之間需要權衡，如下所示：

|                      | <t>Unit</t><t>單元測試</t> | <t>Widget</t><t>Widget 測試</t> | <t>Integration</t><t>整合測試</t> |
|----------------------|--------|--------|-------------|
| **Confidence**       | Low    | Higher | Highest     |
| **置信度**            | 低    | 較高 | 最高     |
| **Maintenance cost** | Low    | Higher | Highest     |
| **維護成本**           | 低    | 較高 | 最高     |
| **Dependencies**     | Few    | More   | Most        |
| **依賴程度**              | 少    | 較多   | 最多        |
| **Execution speed**  | Quick  | Slower | Slow     |
| **執行速度**           | 快  | 較慢 | 慢     |
{:.table.table-striped} 


## Unit tests

## 單元測試

A _unit test_ tests a single function, method, or class.
The goal of a unit test is to verify the correctness of a
unit of logic under a variety of conditions.
External dependencies of the unit under test are generally
[mocked out]({{site.url}}/cookbook/testing/unit/mocking).
Unit tests generally don't read from or write
to disk, render to screen, or receive user actions from
outside the process running the test.
For more information regarding unit tests, 
you can view the following recipes 
or run `flutter test --help` in your terminal.

**單元測試** 測試單一的函式，方法或類別。
單元測試的目標是驗證邏輯單元在各種條件下的正確性。
被測試單元的外部依賴通常需要 [模擬]({{site.url}}/cookbook/testing/unit/mocking)。
單元測試通常不會讀寫磁碟，將資料渲染到螢幕，也不會從執行測試處理序的外部去接收使用者的操作。
你可以在終端執行 `flutter test --help` 命令獲得更多有關單元測試的幫助：

{{site.alert.note}}

  If you're writing unit tests for code that
  uses plugins, see
  [Plugins in Flutter tests][].

  如果你在為 plugin 編寫單元測試，
  請檢視 [如何測試 Flutter Plugin][Plugins in Flutter tests]。

{{site.alert.end}}

[Plugins in Flutter tests]: {{site.url}}/development/packages-and-plugins/plugins-in-tests

### Recipes

### 更多資訊

{% include docs/testing-toc.md type='unit' %}

## Widget tests

## Widget測試

A _widget test_ (in other UI frameworks referred to as _component test_)
tests a single widget. The goal of a widget test is to verify that the
widget's UI looks and interacts as expected. Testing a widget involves
multiple classes and requires a test environment that provides the
appropriate widget lifecycle context.

**Widget 測試**（在其他 UI 框架中指 **元件測試**）是用來測試單一的 widget，
widget 測試的目標是驗證 widget 的 UI 表現和互動行為是否符合預期。
測試一個 widget 涉及多個類，並且測試環境需要提供具有 widget 生命週期的上下文。

For example, the Widget being tested should be able to receive and
respond to user actions and events, perform layout, and instantiate child
widgets. A widget test is therefore more comprehensive than a unit test.
However, like a unit test, a widget test's environment is replaced with
an implementation much simpler than a full-blown UI system.

例如，被測試的 widget 可以接收和響應使用者操作和事件，進行佈局並例項化子 widget。
所以，widget 測試比單元測試更全面。
但是，就像單元測試一樣，widget 測試環境實現上比成熟的 UI 系統簡單得多。

### Recipes

### 更多資訊

{% include docs/testing-toc.md type='widget' %}

## Integration tests

## 整合測試

An _integration test_ tests a complete app or a large part of an app.
The goal of an integration test is to verify that all the widgets
and services being tested work together as expected.
Furthermore, you can use integration
tests to verify your app's performance.

**整合測試** 測試一個完整的應用或者一個應用的大部分功能。
整合測試的目標是驗證正在測試的所有 widget 和服務是否按照預期的方式一起工作。
此外，還可以使用整合測試來驗證應用的效能。

Generally, an _integration test_ runs on a real device or an OS emulator,
such as iOS Simulator or Android Emulator.
The app under test is typically isolated
from the test driver code to avoid skewing the results.

通常情況下，一個 **整合測試** 執行在真機或 OS 模擬器上，
如 iOS 模擬器 (iOS Simulator) 或 Android 模擬器 (Android Emulator) 。
測試中的應用通常與測試驅動程式程式碼隔離，以避免結果出現偏差。

For more information on how to write integration tests, see the [integration
testing page][].

更多關於如何編寫整合測試的相關資訊，請參閱[整合測試文件][integration testing page]。

### Recipes

### 更多資訊

{% include docs/testing-toc.md type='integration' %}

## Continuous integration services

## 持續整合服務

Continuous integration (CI) services allow you to run your
tests automatically when pushing new code changes.
This provides timely feedback on whether the code
changes work as expected and do not introduce bugs.

持續整合 (CI) 服務允許我們在推送新程式碼（程式碼變更）時自動執行測試。
當代碼變更後，會立即收到關於程式碼是否
仍按預期工作、是否引入新問題的反饋。

For information on running tests on various continuous
integration services, see the following:

有關各種持續整合服務的資訊，
參考如下：

* [Continuous delivery using fastlane with Flutter][]
 
  [Flutter 裡的持續部署][Continuous delivery using fastlane with Flutter]

* [Test Flutter apps on Appcircle][]
 
  [使用 Appcircle 測試 Flutter 應用][Test Flutter apps on Appcircle]

* [Test Flutter apps on Travis][]

  [使用 Travis 測試 Flutter 應用][Test Flutter apps on Travis]

* [Test Flutter apps on Cirrus][]

  [使用 Cirrus 測試 Flutter 應用][Test Flutter apps on Cirrus]

* [Codemagic CI/CD for Flutter][]
 
  [使用 Codemagic 進行 Flutter 持續整合/持續交付][Codemagic CI/CD for Flutter]

* [Flutter CI/CD with Bitrise][]

  [使用 Bitrise 進行 Flutter 持續整合/持續交付][Flutter CI/CD with Bitrise]


[code coverage]: https://en.wikipedia.org/wiki/Code_coverage
[Codemagic CI/CD for Flutter]: https://blog.codemagic.io/getting-started-with-codemagic/
[Continuous delivery using fastlane with Flutter]: {{site.url}}/deployment/cd#fastlane
[Flutter CI/CD with Bitrise]: https://devcenter.bitrise.io/getting-started/getting-started-with-flutter-apps/
[How to test a Flutter app]: {{site.codelabs}}/codelabs/flutter-app-testing
[mocked out]: {{site.url}}/cookbook/testing/unit/mocking
[Test Flutter apps on Appcircle]: https://appcircle.io/blog/guide-to-automated-mobile-ci-cd-for-flutter-projects-with-appcircle/#testing-the-flutter-app
[Test Flutter apps on Cirrus]: https://cirrus-ci.org/examples/#flutter
[Test Flutter apps on Travis]: {{site.flutter-medium}}/test-flutter-apps-on-travis-3fd5142ecd8c
[integration testing page]: {{site.url}}/testing/integration-tests
