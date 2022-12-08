---
title: An introduction to unit testing
title: 單元測試介紹
description: How to write unit tests.
description: 如何編寫單元測試。
short-title: Introduction
tags: cookbook, 實用課程, 測試
keywords: 單元測試,test package,flutter_test,IDE
prev:
  title: Performance profiling
  title: 效能測試
  path: /docs/cookbook/testing/integration/profiling
next:
  title: Mock dependencies using Mockito
  title: 使用 Mockito 模擬依賴關係
  path: /docs/cookbook/testing/unit/mocking
---

<?code-excerpt path-base="cookbook/testing/unit/counter_app"?>

How can you ensure that your app continues to work as you
add more features or change existing functionality?
By writing tests.

我們如何保證 app 在增加了新特性或者改變了現有功能之後還能正常工作呢？答案是寫測試！

Unit tests are handy for verifying the behavior of a single function,
method, or class. The [`test`][] package provides the
core framework for writing unit tests, and the [`flutter_test`][]
package provides additional utilities for testing widgets.

使用單元測試可輕鬆地驗證單個函式、方法或類別的行為。
[`test`][]這個 package 提供了寫單測的核心框架，
[`flutter_test`][] package 則提供了額外的功能來測試 widget。

This recipe demonstrates the core features provided by the `test` package
using the following steps:

本課程將會為大家示範 `test` package 的用法，內容如下：

  1. Add the `test` or `flutter_test` dependency.

     將 `test` 或者 `flutter_test` 加入依賴；

  2. Create a test file.

     建立測試檔案；

  3. Create a class to test.

     建立一個要測試的類；

  4. Write a `test` for our class.

     為建立的類寫一個測試；

  5. Combine multiple tests in a `group`.

     整合多個測試到一個 `group`；

  6. Run the tests.

     執行測試。


For more information about the test package,
see the [test package documentation][].

關於 package 測試的更多內容，
可移步至 [test package 的文件][test package documentation]。

## 1. Add the `test` or `flutter_test` dependency

## 1. 新增測試依賴 —— 將 `test` 或者 `flutter_test` 加入依賴檔案

The `test` package provides the core functionality for 
writing tests in Dart. This is the best approach when
writing packages consumed by web, server, and Flutter apps.

如果 Dart package 沒有依賴 Flutter，可以匯入 `test` package。
Test package 提供了編寫測試所需要的核心功能。
當我們寫的 package 需要被 web、伺服器端和 Flutter app 使用時，
這是最佳的方式。

```yaml
dev_dependencies:
  test: <latest_version>
```

## 2. Create a test file

## 2. 建立測試檔案

In this example, create two files: `counter.dart` and `counter_test.dart`.

本例中，我們會建立兩個檔案：`counter.dart` 和 `counter_test.dart`。

The `counter.dart` file contains a class that you want to test and
resides in the `lib` folder. The `counter_test.dart` file contains
the tests themselves and lives inside the `test` folder.

`counter.dart` 檔案包含一個位於 `lib` 資料夾的待測試類，
而位於 `test` 資料夾的 `counter_test.dart` 檔案將包含測試本身。

In general, test files should reside inside a `test` folder
located at the root of your Flutter application or package.
Test files should always end with `_test.dart`,
this is the convention used by the test runner when searching for tests.

通常測試檔案應位於放置在 Flutter 應用或套件的根目錄下的 `test` 資料夾。
測試檔案通常以 `_test.dart` 命名，這是 test runner 尋找測試檔案的慣例。

When you're finished, the folder structure should look like this:

建立完成後，檔案目錄結構如下：

```
counter_app/
  lib/
    counter.dart
  test/
    counter_test.dart
```

## 3. Create a class to test

## 3. 建立一個要測試的類

Next, you need a "unit" to test. Remember: "unit" is another name for a
function, method, or class. For this example, create a `Counter` class
inside the `lib/counter.dart` file. It is responsible for incrementing
and decrementing a `value` starting at `0`.

下一步，我們需要一個「單元」來測試。記住：「單元」是一個抽象的名稱，
它可以表示一個函式、方法或者類別。
本例中，我們會在 `lib/counter.dart` 檔案中建立一個 `Counter` 類別。
它負責增加或減少一個從 `0` 開始的 `value`。

<?code-excerpt "lib/counter.dart"?>
```dart
class Counter {
  int value = 0;

  void increment() => value++;

  void decrement() => value--;
}
```

**Note:** For simplicity, this tutorial does not follow the "Test Driven
Development" approach. If you're more comfortable with that style of
development, you can always go that route.

**注意：** 為了簡化內容，本課程沒有遵守「測試驅動開發」的寫法。
如果你擅長那種開發模式，當然可以選擇用那種方式來寫。

## 4. Write a test for our class

## 4. 為建立的類寫一個測試

Inside the `counter_test.dart` file, write the first unit test. Tests are
defined using the top-level `test` function, and you can check if the results
are correct by using the top-level `expect` function.
Both of these functions come from the `test` package.

我們將在 `counter_test.dart` 檔案中寫第一個測試。
使用最上層函式 `test` 來定義，
可透過執行最上層函式 `expect` 來檢查結果正確與否。
這兩個函式都來自 `test` 這個 package。

<?code-excerpt "test/counter_test.dart"?>
```dart
// Import the test package and Counter class
import 'package:counter_app/counter.dart';
import 'package:test/test.dart';

void main() {
  test('Counter value should be incremented', () {
    final counter = Counter();

    counter.increment();

    expect(counter.value, 1);
  });
}
```

## 5. Combine multiple tests in a `group`

## 5. 整合多個測試到一個 `group`

If you have several tests that are related to one another,
combine them using the `group` function provided by the `test` package.

如果多個測試之間互相關聯，
可以使用 `test` 這個 package 提供的 `group` 函式將他們整合到一起。

<?code-excerpt "test/group.dart"?>
```dart
import 'package:counter_app/counter.dart';
import 'package:test/test.dart';

void main() {
  group('Counter', () {
    test('value should start at 0', () {
      expect(Counter().value, 0);
    });

    test('value should be incremented', () {
      final counter = Counter();

      counter.increment();

      expect(counter.value, 1);
    });

    test('value should be decremented', () {
      final counter = Counter();

      counter.decrement();

      expect(counter.value, -1);
    });
  });
}
```

## 6. Run the tests

## 6. 執行測試

Now that you have a `Counter` class with tests in place,
you can run the tests.

現在 `Counter` 類和它的測試都有了，開始執行測試！

### Run tests using IntelliJ or VSCode

### 用 IntelliJ 或 VSCode 執行測試

The Flutter plugins for IntelliJ and VSCode support running tests.
This is often the best option while writing tests because it provides the
fastest feedback loop as well as the ability to set breakpoints.

IntelliJ 和 VSCode 的 Flutter 外掛支援執行測試。
用這種方式執行測試是最好的，因為它可以提供最快的反饋閉環，
而且還支援斷點除錯。


  * **IntelliJ**

    1. Open the `counter_test.dart` file

       開啟檔案 `counter_test.dart`

    2. Select the `Run` menu

       選擇選單 `Run`

    3. Click the `Run 'tests in counter_test.dart'` option

       點選選項 `Run 'tests in counter_test.dart'`

    4. *Alternatively, use the appropriate keyboard shortcut
       for your platform.*

       *或者，也可以使用系統快捷鍵!*

  * **VSCode**

    1. Open the `counter_test.dart` file

       開啟檔案 `counter_test.dart`

    2. Select the `Run` menu

       在選單裡選擇 `Run`

    3. Click the `Start Debugging` option

       點選選項 `Start Debugging`

    4. *Alternatively, use the appropriate keyboard shortcut
       for your platform.*

       *或者，也可以使用系統快捷鍵!*

### Run tests in a terminal

### 在終端執行測試

You can also use a terminal to run the tests by executing the following
command from the root of the project:

我們也可以開啟終端，在工程根目錄輸入以下命令來執行測試：

```
flutter test test/counter_test.dart
```

For more options regarding unit tests, you can execute this command:

你可以執行下面的命令獲得更多有關單元測試的幫助：

```
flutter test --help
```

[`flutter_test`]: {{site.api}}/flutter/flutter_test/flutter_test-library.html
[`test`]: {{site.pub-pkg}}/test
[test package documentation]: {{site.pub}}/packages/test
