---
title: Flutter for React Native developers
title: 給 React Native 開發者的 Flutter 指南
description: Learn how to apply React Native developer knowledge when building Flutter apps.
description: 學習如何把 React Native 的開發經驗應用到 Flutter 應用的開發中。
tags: Flutter課程,Flutter起步,Flutter入門
keywords: Flutter React Native,React Native,RN轉Flutter
---

<?code-excerpt path-base="get-started/flutter-for/react_native_devs"?>

This document is for React Native (RN) developers looking to apply their
existing RN knowledge to build mobile apps with Flutter. If you understand
the fundamentals of the RN framework then you can use this document as a
way to get started learning Flutter development.

本文面向希望基於現有的 React Native（下文統稱 RN）的知識結構使用
Flutter 開發移動端應用的開發者。
如果你已經對 RN 的框架有所瞭解，
那麼你可以透過這個文件入門 Flutter 開發。

This document can be used as a cookbook by jumping around and finding
questions that are most relevant to your needs.

本文可以當做查詢手冊使用，裡面涉及到的問題基本上可以滿足需求。

## Introduction to Dart for JavaScript Developers

## 針對 JavaScript 開發者的 Dart 介紹

Like React Native, Flutter uses reactive-style views. However, while RN
transpiles to native widgets, Flutter compiles all the way to native code.
Flutter controls each pixel on the screen, which avoids performance problems
caused by the need for a JavaScript bridge.

與 RN 一樣，Flutter 使用響應式風格的介面編寫方式。
然而，RN 需要被轉譯為本地對應的 widget，
而 Flutter 是直接編譯成原生程式碼執行。
Flutter 可以控制螢幕上的每一個畫素，
由此可以避免使用 JavaScript Bridge 導致的效能問題。

Dart is an easy language to learn and offers the following features:

Dart 學習起來非常簡單，包含如下特性：

* Provides an open-source, scalable programming language for building web,
  server, and mobile apps.

  它針對 web 服務和移動應用開發提供了一種開源的，可擴充的程式語言。

* Provides an object-oriented, single inheritance language that uses a C-style
  syntax that is AOT-compiled into native.

  它提供了一種面向物件的單繼承語言，使用 C 語言風格的語法並且可透過 AOT 編譯為原生代碼。

* Transcompiles optionally into JavaScript.

  可轉譯為 JavaScript 程式碼。

* Supports interfaces and abstract classes.

  支援介面和抽象類別。

A few examples of the differences between JavaScript and Dart are described
below.

下面的幾個例子解釋了 JavaScript 和 Dart 的區別。

### Entry point

### 入口函式

JavaScript doesn't have a pre-defined entry
function&mdash;you define the entry point.

JavaScript 並沒有預定義的入口函式。

```js
// JavaScript
function startHere() {
  // Can be used as entry point
}
```

In Dart, every app must have a top-level `main()` function that serves as the
entry point to the app.

在 Dart 裡，每個應用程式必須有一個最最上層的 `main()` 函式，該函式作為應用程式的入口函式。


<?code-excerpt "lib/main.dart (Main)"?>
```dart
/// Dart
void main() {}
```

Try it out in [DartPad][DartPadA].

可以在這裡檢視效果 [DartPad][DartPadA]。

### Printing to the console

### 在控制檯列印輸出

To print to the console in Dart, use `print()`.

在 Dart 中如果需要在控制檯進行輸出，呼叫 `print()`。

```js
// JavaScript
console.log('Hello world!');
```

<?code-excerpt "lib/main.dart (Print)"?>
```dart
/// Dart
print('Hello world!');
```

Try it out in [DartPad][DartPadB].

可以在這裡檢視效果 [DartPad][DartPadB]。

### Variables

### 變數

Dart is type safe&mdash;it uses a combination of static type checking
and runtime checks to ensure that a variable’s value always matches
the variable’s static type. Although types are mandatory,
some type annotations are optional because
Dart performs type inference.

Dart 是型別安全的，它會結合靜態型別檢查和執行時期檢查，
來保證變數的值總是和變數的靜態型別相匹配。
雖然型別是語法要求，有些型別標註也並不是必須要填的，
因為 Dart 使用型別推斷。

#### Creating and assigning variables

#### 建立變數並賦值

In JavaScript, variables cannot be typed.

在 JavaScript 中，變數是無法指定型別的。

In [Dart][], variables must either be explicitly
typed or the type system must infer the proper type automatically.

在 [Dart][] 中，變數可以顯式定義型別，或者型別系統自動判斷變數的型別。

```js
// JavaScript
var name = 'JavaScript';
```

<?code-excerpt "lib/main.dart (Variables)"?>
```dart
/// Dart
/// Both variables are acceptable.
String name = 'dart'; // Explicitly typed as a [String].
var otherName = 'Dart'; // Inferred [String] type.
```

Try it out in [DartPad][DartPadC].

可以在這裡檢視效果 [DartPad][DartPadC]。

For more information, see [Dart's Type System][].

如果想了解更多相關資訊，請參考 [Dart 的型別系統][Dart's Type System]。

#### Default value

#### 預設值

In JavaScript, uninitialized variables are `undefined`.

在 JavaScript 中，未初始化的變數是 `undefined`。

In Dart, uninitialized variables have an initial value of `null`.
Because numbers are objects in Dart, even uninitialized variables with
numeric types have the value `null`.

在 Dart 中，未初始化的變數會有一個初始值 `null`。
因為數字在 Dart 是物件，甚至未初始化的數字型別的變數也會是 `null`。

{{site.alert.note}}

  As of 2.12, Dart supports [Sound Null Safety][],
  all underlying types are non-nullable by default,
  which must be initialized as a non-nullable value.

  自 Dart 2.12 起，[健全的空安全][Sound Null Safety] 已完全支援，
  所有的基礎型別預設都為非空，在使用時必須要初始化為非空的值。

{{site.alert.end}}

```js
// JavaScript
var name; // == undefined
```

<?code-excerpt "lib/main.dart (Null)"?>
```dart
// Dart
var name; // == null; raises a linter warning
int? x; // == null
```

Try it out in [DartPad][DartPadD].

可以在這裡檢視效果 [DartPad][DartPadD]。

For more information, see the documentation on
[variables][].

如果想了解更多詳細內容，請檢視這個文件 [variables][]。

### Checking for null or zero

### 檢查 null 或者零值

In JavaScript, values of 1 or any non-null objects
are treated as `true` when using the `==` comparison operator.

在 JavaScript 中，1 或者任何非空物件在使用
`==` 比較運運算元時都會被隱含轉換為 `true`。

```js
// JavaScript
var myNull = null;
if (!myNull) {
  console.log('null is treated as false');
}
var zero = 0;
if (!zero) {
  console.log('0 is treated as false');
}
```

In Dart, only the boolean value `true` is treated as true.

在 Dart 中，只有布林型別值 `true` 才是 true。

<?code-excerpt "lib/main.dart (True)"?>
```dart
/// Dart
var myNull;
if (myNull == null) {
  print('use "== null" to check null');
}
var zero = 0;
if (zero == 0) {
  print('use "== 0" to check zero');
}
```

Try it out in [DartPad][DartPadE].

可以在這裡檢視效果 [DartPad][DartPadE]。

### Functions

### 函式

Dart and JavaScript functions are generally similar.
The primary difference is the declaration.

Dart 和 JavaScript 中的函式很相似。最大的區別是宣告格式。

```js
// JavaScript
function fn() {
  return true;
}
```

<?code-excerpt "lib/main.dart (Function)"?>
```dart
/// Dart
/// You can explicitly define the return type.
bool fn() {
  return true;
}
```

Try it out in [DartPad][DartPadF].

可以在這裡檢視效果 [DartPad][DartPadF]。

For more information, see the documentation on
[functions][].

如果想了解更多相關資訊，可以參考 [函式][functions] 介紹。

### Asynchronous programming

### 非同步程式設計

#### Futures

Like JavaScript, Dart supports single-threaded execution. In JavaScript,
the Promise object represents the eventual completion (or failure)
of an asynchronous operation and its resulting value.

Dart 與 JavaScript 類似，同樣是單執行緒模型。
在 JavaScript 中，Promise 物件代表非同步操作的完成或者失敗。

Dart uses [`Future`][] objects to handle this.

Dart 使用 [`Future`][] 物件來實現該機制。

```js
// JavaScript
class Example {
  _getIPAddress() {
    const url = 'https://httpbin.org/ip';
    return fetch(url)
      .then(response => response.json())
      .then(responseJson => {
        const ip = responseJson.origin;
        return ip;
      });
  }
}

function main() {
  const example = new Example();
  example
    ._getIPAddress()
    .then(ip => console.log(ip))
    .catch(error => console.error(error));
}

main();
```

<?code-excerpt "lib/futures.dart"?>
```dart
/// Dart
import 'dart:convert';

import 'package:http/http.dart' as http;

class Example {
  Future<String> _getIPAddress() {
    final url = Uri.https('httpbin.org', '/ip');
    return http.get(url).then((response) {
      String ip = jsonDecode(response.body)['origin'];
      return ip;
    });
  }
}

void main() {
  final example = Example();
  example
      ._getIPAddress()
      .then((ip) => print(ip))
      .catchError((error) => print(error));
}
```

For more information, see the documentation on
[`Future`][] objects.

如果想了解更多相關資訊，請參考 [`Future`][] 的相關文件。

#### `async` and `await`

#### `async` 和 `await`

The `async` function declaration defines an asynchronous function.

`async` 函式宣告定義了一個非同步執行的函式。

In JavaScript, the `async` function returns a `Promise`.
The `await` operator is used to wait for a `Promise`.

在 JavaScript 中， `async` 函式返回一個 `Promise`，
`await` 運運算元用於等待 `Promise`。

```js
// JavaScript
class Example {
  async function _getIPAddress() {
    const url = 'https://httpbin.org/ip';
    const response = await fetch(url);
    const json = await response.json();
    const data = json.origin;
    return data;
  }
}

async function main() {
  const example = new Example();
  try {
    const ip = await example._getIPAddress();
    console.log(ip);
  } catch (error) {
    console.error(error);
  }
}

main();
```

In Dart, an `async` function returns a `Future`,
and the body of the function is scheduled for execution later.
The `await` operator is used to wait for a `Future`.

在 Dart 中，`async` 函式返回一個 `Future`，而函式體會在未來執行，
`await` 運運算元用於等待 `Future`。

<?code-excerpt "lib/async.dart"?>
```dart
// Dart
import 'dart:convert';

import 'package:http/http.dart' as http;

class Example {
  Future<String> _getIPAddress() async {
    final url = Uri.https('httpbin.org', '/ip');
    final response = await http.get(url);
    String ip = jsonDecode(response.body)['origin'];
    return ip;
  }
}

/// An async function returns a `Future`.
/// It can also return `void`, unless you use
/// the `avoid_void_async` lint. In that case,
/// return `Future<void>`.
void main() async {
  final example = Example();
  try {
    final ip = await example._getIPAddress();
    print(ip);
  } catch (error) {
    print(error);
  }
}
```

For more information, see the documentation for [async and await][].

如果想了解更多相關資訊，請參考
[`async` 和 `await` 的相關文件][async and await]。

## The basics

## 基本知識

### How do I create a Flutter app?

### 如何建立一個 Flutter 應用？

To create an app using React Native,
you would run `create-react-native-app` from the command line.

如果要建立 RN 應用，
你需要在命令列裡執行 `create-react-native-app`。

```terminal
$ create-react-native-app <projectname>
```

To create an app in Flutter, do one of the following:

要建立 Flutter 應用，完成下面其中一項即可：

* Use an IDE with the Flutter and Dart plugins installed.

  使用帶有 Flutter 和 Dart 外掛的 IDE。

* Use the `flutter create` command from the command line. Make sure that the
  Flutter SDK is in your PATH.

  在命令列中執行命令 `flutter create`。
  不過要提前確認 Flutter SDK 已經在系統環境變數 PATH 中定義。

```terminal
$ flutter create <projectname>
```

For more information, see [Getting started][], which
walks you through creating a button-click counter app.
Creating a Flutter project builds all the files that you
need to run a sample app on both Android and iOS devices.

如果想要了解更多內容，詳見 [開始使用 Flutter][Getting started]，
在該頁面會手把手教你建立一個點選按鈕進行計數的應用。
建立一個 Flutter 專案就可以建構 Android 和 iOS 裝置上執行應用所需的所有檔案。

### How do I run my app?

### 我如何執行應用呢？

In React Native, you would run `npm run` or `yarn run` from the project
directory.

你可以在 RN 的專案資料夾中執行 `npm run` 或者 `yarn run` 以執行應用。

You can run Flutter apps in a couple of ways:

而想執行 Flutter 應用，你可以透過如下幾個途徑進行操作：

* Use the "run" option in an IDE with the Flutter and Dart plugins.

  在帶有 Flutter 和 Dart 外掛的 IDE 中使用 "run" 選項。

* Use `flutter run` from the project's root directory.

  在專案根目錄執行 `flutter run`。

Your app runs on a connected device, the iOS simulator,
or the Android emulator.

你的應用程式會在已連線的裝置、iOS 模擬器或者 Android 模擬器上執行。

For more information, see the Flutter [Getting Started][]
documentation.

如果想了解更多相關資訊，可以參考 Flutter 的相關文件：
[開始使用 Flutter][Getting started]。

### How do I import widgets?

### 如何匯入 widget 

In React Native, you need to import each required component.

在 RN 中，你需要匯入每一個所需的元件。

```js
// React Native
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
```

In Flutter, to use widgets from the Material Design library,
import the `material.dart` package. To use iOS style widgets,
import the Cupertino library. To use a more basic widget set,
import the Widgets library.
Or, you can write your own widget library and import that.

在 Flutter 中，
如果要使用 Material Design 庫裡的 widget，匯入 `material.dart` package。
如果要使用 iOS 風格的 widget，匯入 Cupertino 庫。
如果要使用更加基本的 widget，匯入 Widgets 庫。
或者，你可以實現自己的 widget 庫並匯入。

<?code-excerpt "lib/imports.dart (Imports)"?>
```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:flutter/widgets.dart';
import 'package:my_widgets/my_widgets.dart';
```

Whichever widget package you import,
Dart pulls in only the widgets that are used in your app.

無論你匯入哪個庫，Dart 僅僅參考你應用中用到的 widget。

For more information, see the [Flutter Widget Catalog][].

如果想了解更多相關資訊，可以參考 [核心 Widget 目錄][Flutter Widget Catalog]。

### What is the equivalent of the React Native "Hello world!" app in Flutter?

### 在 Flutter 裡有沒有類似 React Native 中 "Hello world!" 應用程式？

In React Native, the `HelloWorldApp` class extends `React.Component` and
implements the render method by returning a view component.

在 RN 裡，`HelloWorldApp` 繼承自 `React.Component` 
並且透過返回 view 物件實現了 render 方法。

```js
// React Native
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';

export default class App extends React.Component {
  render() {
    return (
      <View style={styles.container}>
        <Text>Hello world!</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center'
  }
});
```

In Flutter, you can create an identical "Hello world!" app using the
`Center` and `Text` widgets from the core widget library.
The `Center` widget becomes the root of the widget tree and has one child,
the `Text` widget.

在 Flutter 中，你可以使用核心 widget 庫中的
`Center` 和 `Text` widget 建立對應的「Hello world!」應用程式。
`Center` widget 是 widget 樹中的根節點，而且只有 `Text` 一個子 widget。

<?code-excerpt "lib/hello_world.dart"?>
```dart
/// Flutter
import 'package:flutter/material.dart';

void main() {
  runApp(
    const Center(
      child: Text(
        'Hello, world!',
        textDirection: TextDirection.ltr,
      ),
    ),
  );
}
```

The following images show the Android and iOS UI for the basic Flutter
"Hello world!" app.

下面的圖片展示了 Android 和 iOS 中的
基本 Flutter "Hello world!" 應用程式的介面。

{% include docs/android-ios-figure-pair.md image="react-native/hello-world-basic.png" alt="Hello world app" class="border" %}

Now that you've seen the most basic Flutter app, the next section shows how to
take advantage of Flutter's rich widget libraries to create a modern, polished
app.

現在大家已經明白了最基本的 Flutter 應用，
接下來會告訴大家如何利用 Flutter 豐富的
widget 庫來建立主流的華麗的應用程式。

### How do I use widgets and nest them to form a widget tree?

### 我如何使用 widget 並且把它們封裝起來組成一個 widget 樹？

In Flutter, almost everything is a widget.

在 Flutter 中，幾乎任何元素都是 widget。

Widgets are the basic building blocks of an app's user interface.
You compose widgets into a hierarchy, called a widget tree.
Each widget nests inside a parent widget
and inherits properties from its parent.
Even the application object itself is a widget.
There is no separate “application” object.
Instead, the root widget serves this role.

Widget 是建構應用軟體使用者介面的基本元素。
你可以將 widget 按照一定的層次組合，成為 widget 樹。
每個 widget 內嵌在父 widget 中，並且繼承了父 widget 的屬性，
甚至應用程式本身就是一個 widget。
並沒有一個獨立的應用程式物件。
反而根 widget 充當了這個角色。

A widget can define:

一個 widget 可以定義為：

* A structural element—like a button or menu

  類似按鈕或者選單的結構化元素

* A stylistic element—like a font or color scheme

  類似字型或者顏色方案的風格化元素

* An aspect of layout—like padding or alignment

  類似填充區或者對齊元素佈局元素

The following example shows the "Hello world!" app using widgets from the
Material library. In this example, the widget tree is nested inside the
`MaterialApp` root widget.

下面的範例展示了使用 Material 庫裡 widget 實現的「Hello world!」應用程式。
在這個範例中，該 widget 樹是包含在 `MaterialApp` root widget 裡的。

<?code-excerpt "lib/widget_tree.dart"?>
```dart
/// Flutter
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Welcome to Flutter'),
        ),
        body: const Center(
          child: Text('Hello world'),
        ),
      ),
    );
  }
}
```

The following images show "Hello world!" built from Material Design widgets.
You get more functionality for free than in the basic "Hello world!" app.

下面的圖片為大家展示了透過 Material Design widget 所實現的「Hello world!」應用。
你可以獲得比「Hello world!」應用更多的功能。

{% include docs/android-ios-figure-pair.md image="react-native/hello-world.png" alt="Hello world app" %}

When writing an app, you'll use two types of widgets:
[`StatelessWidget`][] or [`StatefulWidget`][].
A `StatelessWidget` is just what it sounds like&mdash;a
widget with no state. A `StatelessWidget` is created once,
and never changes its appearance.
A `StatefulWidget` dynamically changes state based on data
received, or user input.

當編寫應用程式碼的時候，你將用到下述兩種 widget：
[無狀態 widget][`StatelessWidget`]
就像它的名字一樣，是一個沒有狀態的 widget。
無狀態 widget 一旦建立，就不會改變。
而 [有狀態 widget][`StatefulWidget`]
會基於接收到的資料或者使用者輸入的資料動態改變狀態。

The important difference between stateless and stateful
widgets is that `StatefulWidget`s have a `State` object
that stores state data and carries it over
across tree rebuilds, so it's not lost.

無狀態 widget 和有狀態 widget 之間的主要區別，
是有狀態 widget 包含一個 `State` 物件會快取狀態資料，
並且 widget 樹的重建也會攜帶該資料，因此狀態不會丟失。

In simple or basic apps it's easy to nest widgets,
but as the code base gets larger and the app becomes complex,
you should break deeply nested widgets into
functions that return the widget or smaller classes.
Creating separate functions
and widgets allows you to reuse the components within the app.

在簡單的或者基本的應用程式中，封裝 widget 非常簡單，
但是隨著程式碼量的增加並且應用程式的功能變得更加複雜，
你應該將層級複雜的 widget 封裝到函式中或者稍小一些的類別。
建立獨立的函式和 widget 可以讓你更好地複用應用中元件。

### How do I create reusable components?

### 如何建立可複用的元件？

In React Native, you would define a class to create a
reusable component and then use `props` methods to set
or return properties and values of the selected elements.
In the example below, the `CustomCard` class is defined
and then used inside a parent class.

在 RN 中，你可以定義一個類別來建立一個可複用的元件然後
使用 `props` 方法來設定或者返回屬性或者所選元素的值。
在下面的範例中，`CustomCard` 類在父類中被定義和呼叫。

```js
// React Native
class CustomCard extends React.Component {
  render() {
    return (
      <View>
        <Text> Card {this.props.index} </Text>
        <Button
          title="Press"
          onPress={() => this.props.onPress(this.props.index)}
        />
      </View>
    );
  }
}

// Usage
<CustomCard onPress={this.onPress} index={item.key} />
```

In Flutter, define a class to create a custom widget and then reuse the
widget. You can also define and call a function that returns a
reusable widget as shown in the `build` function in the following example.

在 Flutter 中，定義一個類別來建立一個自訂 widget 然後複用這個 widget。
你可以定義並且呼叫函式來返回一個可複用的 widget，
正如下面範例中 `build` 函式所示的那樣。

<?code-excerpt "lib/components.dart (Components)"?>
```dart
/// Flutter
class CustomCard extends StatelessWidget {
  const CustomCard({
    super.key,
    required this.index,
    required this.onPress,
  });

  final int index;
  final void Function() onPress;

  @override
  Widget build(BuildContext context) {
    return Card(
        child: Column(
      children: <Widget>[
        Text('Card $index'),
        TextButton(
          onPressed: onPress,
          child: const Text('Press'),
        ),
      ],
    ));
  }
}

class UseCard extends StatelessWidget {
  const UseCard({super.key, required this.index});

  final int index;

  @override
  Widget build(BuildContext context) {
    /// Usage
    return CustomCard(
      index: index,
      onPress: () {
        print('Card $index');
      },
    );
  }
}
```

In the previous example, the constructor for the `CustomCard`
class uses Dart's curly brace syntax `{ }` to indicate named
[optional parameters][].

在之前的範例，`CustomCard` 類別的建構函式使用 Dart 的
花括號 `{ }` 來表示 [可選引數][optional parameters]。

To require these fields, either remove the curly braces from
the constructor, or add `required` to the constructor.

如果將這些引數設定為必填引數，要麼從建構函式中刪掉曲括號，
或者在建構函式中加上 `required`。

The following screenshots show an example of the reusable
`CustomCard` class.

下面的截圖展示了可複用的 `CustomCard` 類別的範例：

{% include docs/android-ios-figure-pair.md image="react-native/custom-cards.png" alt="Custom cards" class="border" %}

## Project structure and resources

## 專案結構和資源

### Where do I start writing the code?

### 該從哪開始寫程式碼呢？

Start with the `lib/main.dart` file.
It's autogenerated when you create a Flutter app.

從 `main.dart` 檔案開始。
這個檔案會在你建立 Flutter 應用時自動產生。

<?code-excerpt "lib/examples.dart (Main)"?>
```dart
// Dart
void main() {
  print('Hello, this is the main function.');
}
```

In Flutter, the entry point file is
`{project_name}/lib/main.dart` and execution
starts from the `main` function.

在 Flutter 中，入口檔案是 `{專案目錄}/lib/main.dart`
而程式執行是從 `main` 函式開始的。

### How are files structured in a Flutter app?

### Flutter 應用程式中的檔案是如何組織的？

When you create a new Flutter project,
it builds the following directory structure.
You can customize it later, but this is where you start.

當你建立一個新的 Flutter 工程的時候，它會建立如下所示的資料夾結構。
你可以自訂這個結構，不過這是整個開發的起點。

```
┬
└ project_name
  ┬
  ├ android      - Contains Android-specific files.
  ├ build        - Stores iOS and Android build files.
  ├ ios          - Contains iOS-specific files.
  ├ lib          - Contains externally accessible Dart source files.
    ┬
    └ src        - Contains additional source files.
    └ main.dart  - The Flutter entry point and the start of a new app.
                   This is generated automatically when you create a Flutter
                    project.
                   It's where you start writing your Dart code.
  ├ test         - Contains automated test files.
  └ pubspec.yaml - Contains the metadata for the Flutter app.
                   This is equivalent to the package.json file in React Native.
```

```
┬
└ 專案目錄
  ┬
  ├ android      - 包含 Android 相關檔案。
  ├ build        - 儲存 iOS 和 Android 建構檔案。
  ├ ios          - 包含 iOS 相關檔案。
  ├ lib          - 包含外部可存取 Dart 原始檔。
    ┬
    └ src        - 包含附加原始檔。
    └ main.dart  - Flutter 程式入口和新應用程式的起點。
                   當你建立 Flutter 工程的時候會自動產生這些檔案。

                   你從這裡開始寫 Dart 程式碼
  ├ test         - 包含自動測試檔案。
  └ pubspec.yaml - 包含 Flutter 應用程式的元資料。
                   這個檔案相當於 RN 裡的 package.json 檔案。
```

### Where do I put my resources and assets and how do I use them?

### 我該把資原始檔放到哪並且如何呼叫呢？

A Flutter resource or asset is a file that is bundled and deployed
with your app and is accessible at runtime.
Flutter apps can include the following asset types:

一個 Flutter 資源就是打包到你應用程式裡的一個檔案並且在程式執行的時候可以存取。
Flutter 應用程式可以包含下述幾種資源型別：

* Static data such as JSON files

  類似 JSON 檔案的靜態資料

* Configuration files

  配置檔案

* Icons and images (JPEG, PNG, GIF, Animated GIF, WebP, Animated WebP, BMP,
  and WBMP)

  圖示和圖片（JPEG、PNG、GIF、WebP、BMP 和 WBMP）

Flutter uses the `pubspec.yaml` file,
located at the root of your project, to
identify assets required by an app.

Flutter 使用 `pubspec.yaml` 檔案來確定應用程式中的資源。
該檔案在工程的根目錄。

```yaml
flutter:
  assets:
    - assets/my_icon.png
    - assets/background.png
```

The `assets` subsection specifies files that should be included with the app.
Each asset is identified by an explicit path
relative to the `pubspec.yaml` file, where the asset file is located.
The order in which the assets are declared does not matter.
The actual directory used (`assets` in this case) does not matter.
However, while assets can be placed in any app directory, it's a
best practice to place them in the `assets` directory.

`assets` 確定了需要包含在應用程式中的檔案。
每個資源都會在 `pubspec.yaml` 中定義所儲存的相對路徑。
資源定義的順序沒有特殊要求。
實際的資料夾（在這裡指 `assets` ）也沒影響。
但是， 由於資源可以放置於程式的任何目錄，
所以放在 `assets` 資料夾是比較好的。

During a build, Flutter places assets into a special archive
called the *asset bundle*, which apps read from at runtime.
When an asset’s path is specified in the assets' section of `pubspec.yaml`,
the build process looks for any files
with the same name in adjacent subdirectories.
These files are also included in the asset bundle
along with the specified asset. Flutter uses asset variants
when choosing resolution-appropriate images for your app.

在建構期間，Flutter 會將資源放到一個稱為 *asset bundle* 的歸檔檔案中，
應用程式可以在執行時存取該檔案。當一個資源在 `pubspec.yaml` 中被宣告時，
建構處理序會查詢和這個檔案相關的子資料夾路徑，
這些檔案也會被包含在 asset bundle 中。
當你為應用程式選擇和螢幕顯示解析度相關的圖片時，
Flutter 會使用資源變體。

In React Native, you would add a static image by placing the image file
in a source code directory and referencing it.

在 RN 中，你可以在原始碼資料夾中透過新增檔案來
增加一個靜態圖片並且在程式碼中參考它。

```js
<Image source={require('./my-icon.png')} />
```

In Flutter, add a static image to your app
using the `Image.asset` constructor in a widget’s build method.

在 Flutter 中，如果要展示靜態資源圖片，
在 widget 的 build 方法中使用 `Image.asset` 構造即可。

<?code-excerpt "lib/examples.dart (ImageAsset)" replace="/return //g"?>
```dart
Image.asset('assets/background.png');
```

For more information, see [Adding Assets and Images in Flutter][].

如果想了解更多相關資訊，請參考文件
[在 Flutter 中新增資源和圖片][Adding Assets and Images in Flutter]。

### How do I load images over a network?

### 如何在網路中載入圖片？

In React Native, you would specify the `uri` in the
`source` prop of the `Image` component and also provide the
size if needed.

在 RN 中，你可以在 `Image` 的 `source` 屬性中設定 `uri` 和所需的尺寸。

In Flutter, use the `Image.network` constructor to include
an image from a URL.

在 Flutter 中，使用 `Image.network` 建構函式來實現透過地址載入圖片的操作。

<?code-excerpt "lib/examples.dart (ImageNetwork)" replace="/return //g"?>
```dart
Image.network('https://docs.flutter.dev/assets/images/docs/owl.jpg');
```

### How do I install packages and package plugins?

### 我如何安裝相依套件和包外掛？

Flutter supports using shared packages contributed by other developers to the
Flutter and Dart ecosystems. This allows you to quickly build your app without
having to develop everything from scratch. Packages that contain
platform-specific code are known as package plugins.

Flutter 支援使用開發者向 Flutter 和 Dart 生態系統貢獻的 package。
這樣可以使大量開發者快速建構應用程式而無需重複造車輪。
而平台相關的 package 就被稱為外掛。

In React Native, you would use `yarn add {package-name}` or
`npm install --save {package-name}` to install packages
from the command line.

在 RN 中，你可以在命令列中執行 `yarn add {package-name}` 
或者 `npm install --save {package-name}` 來安裝程式碼套件。

In Flutter, install a package using the following instructions:

在 Flutter 中，安裝 package 需要按照如下的步驟：

1. Add the package name and version to the `pubspec.yaml` dependencies section.
   The example below shows how to add the `google_sign_in` Dart package to the
   `pubspec.yaml` file. Check your spaces when working in the YAML file because
   **white space matters**!

   在 `pubspec.yaml` 的 dependencies 區域新增套件名稱和版本。
   下面的例子向大家展示瞭如何將 `google_sign_in`
   的 Dart package 新增到 `pubspec.yaml` 中。
   一定要檢查一下 YAML 檔案中的空格，因為 **空格很重要**!

```yaml
dependencies:
  flutter:
    sdk: flutter
  google_sign_in: ^3.0.3
```

2. Install the package from the command line by using `flutter pub get`.
   If using an IDE, it often runs `flutter pub get` for you, or it might
   prompt you to do so.

   在命令列中輸入 `flutter pub get` 來安裝程式碼套件。
   如果使用 IDE，它自己會執行 `flutter pub get`，
   或者它會提示你是不是要執行該命令。

3. Import the package into your app code as shown below:

   向下面程式碼一樣在程式中參考 package：

<?code-excerpt "lib/examples.dart (PackageImport)"?>
```dart
import 'package:flutter/material.dart';
```

For more information, see [Using Packages][] and
[Developing Packages & Plugins][].

如果想了解更多相關資訊，請參考
[在 Flutter 裡使用 Packages][Using Packages] 和
[Flutter Packages 的開發和提交][Developing Packages & Plugins]。

You can find many packages shared by Flutter developers in the
[Flutter packages][] section of the [pub.dev][].

你可以在 [Flutter packages][] 的 [pub.dev][]
找到開發者們分享的 package。

## Flutter widgets

In Flutter, you build your UI out of widgets that describe what their view
should look like given their current configuration and state.

在 Flutter 中，你可以基於 widget 打造你自己的 UI，
透過 widget 當前的設定和狀態會呈現相應的頁面效果。

Widgets are often composed of many small,
single-purpose widgets that are nested to produce powerful effects.
For example, the `Container` widget consists of
several widgets responsible for layout, painting, positioning, and sizing.
Specifically, the `Container` widget includes the `LimitedBox`,
`ConstrainedBox`, `Align`, `Padding`, `DecoratedBox`, and `Transform` widgets.
Rather than subclassing `Container` to produce a customized effect, you can
compose these and other simple widgets in new and unique ways.

Widget 常常透過很多小的、單一功能的 widget 組成，
透過這樣的封裝往往能夠實現很棒的效果。
比如，`Container` widget 包含多種 widget，分別負責佈局、繪圖、位置變化和尺寸變化。
準確的說，`Container` widget 包括 `LimitedBox`、`ConstrainedBox`、`Align`、
`Padding`、`DecoratedBox` 和 `Transform` widget。
與其繼承 `Container` 來實現自訂效果，
不如直接修改這些 widget 來實現效果。

The `Center` widget is another example of how you can control the layout.
To center a widget, wrap it in a `Center` widget and then use layout
widgets for alignment, row, columns, and grids.
These layout widgets do not have a visual representation of their own.
Instead, their sole purpose is to control some aspect of another
widget’s layout. To understand why a widget renders in a
certain way, it’s often helpful to inspect the neighboring widgets.

`Center` widget 是另一個用於控制佈局的範例。
如果要居中一個 widget，就把它封裝到 `Center` widget 中，
然後使用佈局 widget 來進行對齊行、列和網格。
這些佈局 widget 並不可見。而他們的作用就是控制其它 widget 的佈局。
如果想搞清楚為什麼一個 widget 會有這樣的效果，
有效的方法是研究它臨近的 widget。

For more information, see the [Flutter Technical Overview][].

如果想了解更多相關資訊，請參考 [Flutter 技術概覽][Flutter Technical Overview]。

For more information about the core widgets from the `Widgets` package,
see [Flutter Basic Widgets][],
the [Flutter Widget Catalog][],
or the [Flutter Widget Index][].

如果想了解更多關於 Widgets 套件中的核心 widget，
請參考 [基礎 Flutter Widgets][Flutter Basic Widgets]、
[核心 Widget 目錄][Flutter Widget Catalog]
或是 [Flutter Widget 目錄][Flutter Widget Index]。

## Views

## 檢視

### What is the equivalent of the `View` container?

### 與 `View` 等價容器的是什麼？

In React Native, `View` is a container that supports layout with `Flexbox`,
style, touch handling, and accessibility controls.

在 RN 中， `View` 是支援 `Flexbox` 
佈局、風格化、觸控事件處理和存取性控制的容器。

In Flutter, you can use the core layout widgets in the `Widgets`
library, such as [`Container`][], [`Column`][],
[`Row`][], and [`Center`][].
For more information, see the [Layout Widgets][] catalog.

在 Flutter 中，你可以使用 Widgets 庫中的核心佈局 widget，
比如 [`Container`][]、[`Column`][]、[`Row`][] 和 [`Center`][]。
如果想了解更多相關資訊，請參考 [佈局類 Widgets][Layout Widgets] 目錄。

### What is the equivalent of `FlatList` or `SectionList`?

### 和 `FlatList` 或者 `SectionList` 相對應的是什麼？

A `List` is a scrollable list of components arranged vertically.

`List` 是一個可以滾動的縱向排列的元件列表。

In React Native, `FlatList` or `SectionList` are used to render simple or
sectioned lists.

在 RN 中，`FlatList` 或者 `SectionList` 用於渲染簡單的或者分組的列表。

```js
// React Native
<FlatList
  data={[ ... ]}
  renderItem={({ item }) => <Text>{item.key}</Text>}
/>
```

[`ListView`][] is Flutter's most commonly used scrolling widget.
The default constructor takes an explicit list of children.
[`ListView`][] is most appropriate for a small number of widgets.
For a large or infinite list, use `ListView.builder`,
which builds its children on demand and only builds
those children that are visible.

[`ListView`][] 是 Flutter 最常用的滑動 widget。
預設建構函式需要一個數據列表的引數。
[`ListView`][] 非常適合用於少量子 widget 的列表。
如果列表的元素比較多，可以使用 `ListView.builder`，
它會按需建構子項並且只建立可見的子項。

<?code-excerpt "lib/examples.dart (ListView)"?>
```dart
var data = [
  'Hello',
  'World',
];
return ListView.builder(
  itemCount: data.length,
  itemBuilder: (context, index) {
    return Text(data[index]);
  },
);
```

{% include docs/android-ios-figure-pair.md image="react-native/flatlist.gif" alt="Flat list" class="border" %}

To learn how to implement an infinite scrolling list, see the
[Write Your First Flutter App, Part 1][] codelab.

如果要了解如何實現無限滑動列表，請參考
[編寫你的第一個 Flutter 應用 - 第一部分][Write Your First Flutter App, Part 1]
的 codelab。

### How do I use a Canvas to draw or paint?

### 如何使用 Canvas 繪圖？

In React Native, canvas components aren't present
so third party libraries like `react-native-canvas` are used.

在 RN 中，canvas 元件是不可見的，
所以需要使用類似 `react-native-canvas` 這樣的元件。

```js
// React Native
handleCanvas = canvas => {
  const ctx = canvas.getContext('2d');
  ctx.fillStyle = 'skyblue';
  ctx.beginPath();
  ctx.arc(75, 75, 50, 0, 2 * Math.PI);
  ctx.fillRect(150, 100, 300, 300);
  ctx.stroke();
};

render() {
  return (
    <View>
      <Canvas ref={this.handleCanvas} />
    </View>
  );
}
```

In Flutter, you can use the [`CustomPaint`][]
and [`CustomPainter`][] classes to draw to the canvas.

在 Flutter 中，你可以使用 [`CustomPaint`][]
和 [`CustomPainter`][] 在畫布上進行繪製。

The following example shows how to draw during the paint phase using the
`CustomPaint` widget. It implements the abstract class, `CustomPainter`,
and passes it to `CustomPaint`'s painter property.
`CustomPaint` subclasses must implement the `paint()`
and `shouldRepaint()` methods.

下面的範例程式碼展示瞭如何使用 `CustomPaint` 進行繪圖。
它實現了抽象類別 `CustomPainter`，
然後將它賦值給 `CustomPainter` 的 painter 屬性。
`CustomPainter` 子類別必須實現 `paint` 和 `shouldRepaint` 方法。

<?code-excerpt "lib/examples.dart (CustomPaint)"?>
```dart
class MyCanvasPainter extends CustomPainter {
  const MyCanvasPainter();

  @override
  void paint(Canvas canvas, Size size) {
    final Paint paint = Paint()..color = Colors.amber;
    canvas.drawCircle(const Offset(100.0, 200.0), 40.0, paint);
    final Paint paintRect = Paint()..color = Colors.lightBlue;
    final Rect rect = Rect.fromPoints(
      const Offset(150.0, 300.0),
      const Offset(300.0, 400.0),
    );
    canvas.drawRect(rect, paintRect);
  }

  @override
  bool shouldRepaint(MyCanvasPainter oldDelegate) => false;
}

class MyCanvasWidget extends StatelessWidget {
  const MyCanvasWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return const Scaffold(
      body: CustomPaint(painter: MyCanvasPainter()),
    );
  }
}
```

{% include docs/android-ios-figure-pair.md image="react-native/canvas.png" alt="Canvas" class="border" %}

## Layouts

## 佈局

### How do I use widgets to define layout properties?

### 如何使用 widget 來定義佈局屬性？

In React Native, most of the layout can be done with the props
that are passed to a specific component.
For example, you could use the `style` prop on the `View` component
in order to specify the flexbox properties.
To arrange your components in a column, you would specify a prop such as:
`flexDirection: 'column'`.

在 RN 中，大多數佈局需要透過向指定的元件傳遞屬性引數進行設定。
比如，你可以使用 `View` 的 `style` 來設定 flexbox 屬性。
如果要整理一列的元件，你可以使用如下的屬性設定：`flexDirection: “column”`。

```js
// React Native
<View
  style={%raw%}{{
    flex: 1,
    flexDirection: 'column',
    justifyContent: 'space-between',
    alignItems: 'center'
  }}{%endraw%}
>
```

In Flutter, the layout is primarily defined by widgets
specifically designed to provide layout,
combined with control widgets and their style properties.

在 Flutter 中，佈局主要是由專門的 widget 定義的，
它們同控制類 widget 和樣式屬性一起發揮功能。

For example, the [`Column`][] and [`Row`][] widgets
take an array of children and align them
vertically and horizontally respectively.
A [`Container`][] widget takes a combination of
layout and styling properties, and a
[`Center`][] widget centers its child widgets.

比如，[`Column`][] 和 [`Row`][] widget 接受一個數組的子元素
並且分別按照縱向和橫向進行排列。
[`Container`][] widget 包含佈局和樣式屬性的組合，
[`Center`][] widget 會將其自 widget 也設定居中。

<?code-excerpt "lib/layouts.dart (Column)"?>
```dart
@override
Widget build(BuildContext context) {
  return Center(
    child: Column(
      children: <Widget>[
        Container(
          color: Colors.red,
          width: 100.0,
          height: 100.0,
        ),
        Container(
          color: Colors.blue,
          width: 100.0,
          height: 100.0,
        ),
        Container(
          color: Colors.green,
          width: 100.0,
          height: 100.0,
        ),
      ],
    ),
  );
```

Flutter provides a variety of layout widgets in its core widget library.
For example, [`Padding`][], [`Align`][], and [`Stack`][].

Flutter 在核心 widget 庫中提供多種不同的佈局 widget。
比如 [`Padding`][]、[`Align`][] 和 [`Stack`][]。

For a complete list, see [Layout Widgets][].

要得到完整的 widget 列表，請參考 [Layout Widgets][]。

{% include docs/android-ios-figure-pair.md image="react-native/basic-layout.gif" alt="Layout" class="border" %}

### How do I layer widgets?

### 如何為 widget 分層？

In React Native, components can be layered using `absolute` positioning.

在 RN 中，元件可以透過 `absolute` 劃分層次。

Flutter uses the [`Stack`][]
widget to arrange children widgets in layers.
The widgets can entirely or partially overlap the base widget.

在 Flutter 中使用 [`Stack`][] widget 將子 widget 進行分層。
該 widget 可以將整體或者部分的子 widget 進行分層。

The `Stack` widget positions its children relative to the edges of its box.
This class is useful if you simply want to overlap several children widgets.

`Stack` widget 將子 widget 根據容器的邊界進行佈局。
如果你僅僅想把子 widget 重疊擺放的話，這個 widget 非常合適。

<?code-excerpt "lib/layouts.dart (Stack)"?>
```dart
@override
Widget build(BuildContext context) {
  return Stack(
    alignment: const Alignment(0.6, 0.6),
    children: <Widget>[
      const CircleAvatar(
        backgroundImage: NetworkImage(
          'https://avatars3.githubusercontent.com/u/14101776?v=4',
        ),
      ),
      Container(
        color: Colors.black45,
        child: const Text('Flutter'),
      ),
    ],
  );
```

The previous example uses `Stack` to overlay a Container
(that displays its `Text` on a translucent black background)
on top of a `CircleAvatar`.
The Stack offsets the text using the alignment property
and `Alignment` coordinates.

上面的範例程式碼使用 `Stack` 將一個 Container
（`Text` 顯示在一個半透明的黑色背景上）
覆蓋在一個 `CircleAvatar` 上。
Stack 使用對齊屬性和 Alignment 座標微調文字。

{% include docs/android-ios-figure-pair.md image="react-native/stack.png" alt="Stack" class="border" %}

For more information, see the [`Stack`][] class documentation.

如果想了解更多相關資訊，請參考 [`Stack`][] 類別的文件。

## Styling

## 風格化

### How do I style my components?

### 如何設定元件的風格？

In React Native, inline styling and `stylesheets.create`
are used to style components.

在 RN 中，內聯風格化和 `stylesheets.create` 可以用於設定元件的風格。

```js
// React Native
<View style={styles.container}>
  <Text style={%raw%}{{ fontSize: 32, color: 'cyan', fontWeight: '600' }}{%endraw%}>
    This is a sample text
  </Text>
</View>

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center'
  }
});
```

In Flutter, a `Text` widget can take a `TextStyle` class
for its style property. If you want to use the same text
style in multiple places, you can create a
[`TextStyle`][] class and use it for multiple `Text` widgets.

在 Flutter 中， `Text` widget 可以接受 `TextStyle` 作為它的風格化屬性。
如果你想在不同的場合使用相同的文字風格，
你可以建立一個 [`TextStyle`][] 類，
並且在多個 `Text` widget 中使用它。

<?code-excerpt "lib/examples.dart (TextStyle)"?>
```dart
const TextStyle textStyle = TextStyle(
  color: Colors.cyan,
  fontSize: 32.0,
  fontWeight: FontWeight.w600,
);

return Center(
  child: Column(
    children: const <Widget>[
      Text('Sample text', style: textStyle),
      Padding(
        padding: EdgeInsets.all(20.0),
        child: Icon(
          Icons.lightbulb_outline,
          size: 48.0,
          color: Colors.redAccent,
        ),
      ),
    ],
  ),
);
```

{% include docs/android-ios-figure-pair.md image="react-native/flutterstyling.gif" alt="Styling" class="border" %}

### How do I use `Icons` and `Colors`?

### 我如何使用 `Icons` 和 `Colors` 呢？

React Native doesn't include support for icons
so third party libraries are used.

RN 並不包含預設圖示，所以需要使用第三方庫。

In Flutter, importing the Material library also pulls in the
rich set of [Material icons][] and [colors][].

在 Flutter 中，參考 Material 庫的時候就同時引入了
[Material icons][] 和 [colors][]。

<?code-excerpt "lib/examples.dart (Icon)"?>
```dart
return const Icon(Icons.lightbulb_outline, color: Colors.redAccent);
```

When using the `Icons` class,
make sure to set `uses-material-design: true` in
the project's `pubspec.yaml` file.
This ensures that the `MaterialIcons` font,
which displays the icons, is included in your app.
In general, if you intend to use the Material library,
you should include this line.

當使用 `Icons` 類時，確保在專案的 `pubspec.yaml` 檔案中
設定 `uses-material-design: true`，
這樣保證 `MaterialIcons` 相關字型被包含在你的應用中。
一般來說，如果你想用 Material 庫的話，則需要包含這一行內容。

```yaml
name: my_awesome_application
flutter:
  uses-material-design: true
```

Flutter's [Cupertino (iOS-style)][] package provides high
fidelity widgets for the current iOS design language.
To use the `CupertinoIcons` font,
add a dependency for `cupertino_icons` in your project's 
`pubspec.yaml` file.

Flutter 的 [Cupertino (iOS-style)][] package
為 iOS 設計語言提供高解析度的 widget。
要使用 `CupertinoIcons` 字型，
在專案的 `pubspec.yaml` 檔案中新增
`cupertino_icons` 的依賴即可。

```yaml
name: my_awesome_application
dependencies:
  cupertino_icons: ^0.1.0
```

To globally customize the colors and styles of components,
use `ThemeData` to specify default colors
for various aspects of the theme.
Set the theme property in `MaterialApp` to the `ThemeData` object.
The [`Colors`][] class provides colors
from the Material Design [color palette][].

要在全域範圍內自訂元件的顏色和風格，
使用 `ThemeData` 為不同的主題指定預設顏色。
在 `MaterialApp` 的主題屬性中設定 `ThemeData` 物件。
[`Colors`][] 類提供 Material Design [color palette][]
中所提供的顏色配置。

The following example sets the primary swatch to `blue`
and the text selection to `red`.

下面的範例程式碼將主色調設定為 `blue` 然後文字顏色設定為 `red`。

<?code-excerpt "lib/examples.dart (Swatch)"?>
```dart
class SampleApp extends StatelessWidget {
  const SampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
          primarySwatch: Colors.blue,
          textSelectionTheme:
              const TextSelectionThemeData(selectionColor: Colors.red)),
      home: const SampleAppPage(),
    );
  }
}
```

### How do I add style themes?

### 如何增加風格化主題？

In React Native, common themes are defined for
components in stylesheets and then used in components.

在 React Native，常用主題都定義在樣式層疊表中。

In Flutter, create uniform styling for almost everything
by defining the styling in the [`ThemeData`][]
class and passing it to the theme property in the
[`MaterialApp`][] widget.

在 Flutter 中，為所有元件建立統一風格可以在 [`ThemeData`][] 類中定義，
並將它賦值給 [`MaterialApp`][] 的主題屬性。

<?code-excerpt "lib/examples.dart (Theme)"?>
```dart
@override
Widget build(BuildContext context) {
  return MaterialApp(
    theme: ThemeData(
      primaryColor: Colors.cyan,
      brightness: Brightness.dark,
    ),
    home: const StylingPage(),
  );
}
```

A `Theme` can be applied even without using the `MaterialApp` widget.
The [`Theme`][] widget takes a `ThemeData` in its `data` parameter
and applies the `ThemeData` to all of its children widgets.

`Theme` 可以在不使用 `MaterialApp` widget 的情況下使用。
[`Theme`][] 接受一個 `ThemeData` 引數，
並且將 `ThemeData` 應用於它的全部子 widget。

<?code-excerpt "lib/examples.dart (ThemeData)"?>
```dart
@override
Widget build(BuildContext context) {
  return Theme(
    data: ThemeData(
      primaryColor: Colors.cyan,
      brightness: brightness,
    ),
    child: Scaffold(
      backgroundColor: Theme.of(context).primaryColor,
      //...
    ),
  );
}
```

## State management

## 狀態管理

State is information that can be read synchronously
when a widget is built or information
that might change during the lifetime of a widget.
To manage app state in Flutter,
use a [`StatefulWidget`][] paired with a State object.

當 widget 被建立或者在 widget 的生命週期中有資訊發生改變時所產生的資訊叫做狀態。
要在 Flutter 中管理應用程式的狀態，使用
[`StatefulWidget`][] 和 State 物件。

For more information on ways to approach managing state in Flutter,
see [State management][].

欲知更多關於 Flutter 的狀態管理相關的內容，請參訪
[狀態管理文件][State management] 頁面。

### The StatelessWidget

### 管理 StatelessWidget widget

A `StatelessWidget` in Flutter is a widget
that doesn't require a state change&mdash;
it has no internal state to manage.

`StatelessWidget` 在 Flutter 中是一個不需要狀態改變的 widget，它沒有內部的狀態。

Stateless widgets are useful when the part of the user interface
you are describing does not depend on anything other than the
configuration information in the object itself and the
[`BuildContext`][] in which the widget is inflated.

當你展現給使用者的介面並不依賴其它任何配置資訊並且使用
[`BuildContext`][] 來解析 widget，則需要使用無狀態 widget。

[`AboutDialog`][], [`CircleAvatar`][], and [`Text`][] are examples
of stateless widgets that subclass [`StatelessWidget`][].

[`AboutDialog`][]、[`CircleAvatar`][] 和 [`Text`][] 是
[`StatelessWidget`][] 的子類別，並且是很典型的無狀態 widget。

<?code-excerpt "lib/stateless.dart"?>
```dart
import 'package:flutter/material.dart';

void main() => runApp(
      const MyStatelessWidget(
        text: 'StatelessWidget Example to show immutable data',
      ),
    );

class MyStatelessWidget extends StatelessWidget {
  const MyStatelessWidget({
    super.key,
    required this.text,
  });

  final String text;

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        text,
        textDirection: TextDirection.ltr,
      ),
    );
  }
}
```

The previous example uses the constructor of the `MyStatelessWidget`
class to pass the `text`, which is marked as `final`.
This class extends `StatelessWidget`&mdash;it contains immutable data.

在上面的例子中，你用到了 `MyStatelessWidget` 類別的建構函式來傳遞 `text`。
並且它被標記為 `final`。該類繼承了 `StatelessWidget`，它包含不可變的資料。

The `build` method of a stateless widget is typically called
in only three situations:

無狀態 widget 的 `build` 方法通常只有在三種情況下會被呼叫：

* When the widget is inserted into a tree

  當 widget 被插入到 widget 樹中；

* When the widget's parent changes its configuration

  當 widget 的父 widget 改變了配置；

* When an [`InheritedWidget`][] it depends on, changes

  當所依賴的 [`InheritedWidget`][] 發生了改變。

### The StatefulWidget

### StatefulWidget widget

A [`StatefulWidget`][] is a widget that changes state.
Use the `setState` method to manage the
state changes for a `StatefulWidget`.
A call to `setState()` tells the Flutter
framework that something has changed in a state,
which causes an app to rerun the `build()` method
so that the app can reflect the change.

[`StatefulWidget`][] 是攜帶狀態變化的 widget。
透過呼叫 `setState` 方法可以管理 `StatefulWidget` 的狀態。
當呼叫 `setState()` 的時候，程式會通知 Flutter 框架有狀態發生了改變，
然後會重新執行 `build()` 方法來更新應用的狀態。

_State_ is information that can be read synchronously when a widget
is built and might change during the lifetime of the widget.
It's the responsibility of the widget implementer to ensure that
the state object is promptly notified when the state changes.
Use `StatefulWidget` when a widget can change dynamically.
For example, the state of the widget changes by typing into a form,
or moving a slider.
Or, it can change over time—perhaps a data feed updates the UI.

**狀態** 是在 widget 被建立期間可以被同步讀取的資訊，
並且在 widget 的生命週期中會發生改變。
實現該 widget 的時候要注意保證黨狀態發生改變的時候程式能夠獲得相應的提醒。
當 widget 能夠動態改變的時候，請使用 `StatefulWidget`。
比如，某個 widget 會隨著使用者填寫表單或者移動滑塊的時候發生改變。
亦或者隨著資料源更新的時候發生改變。

[`Checkbox`][], [`Radio`][], [`Slider`][], [`InkWell`][],
[`Form`][], and [`TextField`][]
are examples of stateful widgets that subclass
[`StatefulWidget`][].

[`Checkbox`][]、[`Radio`][]、[`Slider`][]、[`InkWell`][]、
[`Form`][]、和 [`TextField`][] 都是有狀態的 widget，是
[`StatefulWidget`][] 的子類別。

The following example declares a `StatefulWidget`
that requires a `createState()` method.
This method creates the state object that manages the widget's state,
`_MyStatefulWidgetState`.

下面的範例程式碼聲明瞭一個 `StatefulWidget`，需要實現 `createState()` 方法。
該方法建立一個物件來管理 widget 的狀態，也就是 `_MyStatefulWidgetState`。

<?code-excerpt "lib/stateful.dart (StatefulWidget)"?>
```dart
class MyStatefulWidget extends StatefulWidget {
  const MyStatefulWidget({
    super.key,
    required this.title,
  });

  final String title;

  @override
  State<MyStatefulWidget> createState() => _MyStatefulWidgetState();
}
```

The following state class, `_MyStatefulWidgetState`,
implements the `build()` method for the widget.
When the state changes, for example, when the user toggles
the button, `setState()` is called with the new toggle value.
This causes the framework to rebuild this widget in the UI.

下面的狀態類，`_MyStatefulWidgetState`，實現了 `build()` 方法。
當狀態發生改變的時候，比如說使用者點選了開關按鈕，
這時 `setState` 就會被呼叫，並且將新的開關狀態傳進來。
這就會使整體框架重構這個 widget。

<?code-excerpt "lib/stateful.dart (StatefulWidgetState)"?>
```dart
class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  bool showText = true;
  bool toggleState = true;
  Timer? t2;

  void toggleBlinkState() {
    setState(() {
      toggleState = !toggleState;
    });
    if (!toggleState) {
      t2 = Timer.periodic(const Duration(milliseconds: 1000), (t) {
        toggleShowText();
      });
    } else {
      t2?.cancel();
    }
  }

  void toggleShowText() {
    setState(() {
      showText = !showText;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          children: <Widget>[
            if (showText)
              const Text(
                'This execution will be done before you can blink.',
              ),
            Padding(
              padding: const EdgeInsets.only(top: 70.0),
              child: ElevatedButton(
                onPressed: toggleBlinkState,
                child: toggleState
                    ? const Text('Blink')
                    : const Text('Stop Blinking'),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

### What are the StatefulWidget and StatelessWidget best practices?

### StatefulWidget 和 StatelessWidget 的最佳實踐是什麼？

Here are a few things to consider when designing your widget.

下面有一些設計原則供大家參考。

1. Determine whether a widget should be
   a `StatefulWidget` or a `StatelessWidget`.

   確定一個 widget 應該是 `StatefulWidget` 還是 `StatelessWidget`。

In Flutter, widgets are either Stateful or Stateless—depending on whether
they depend on a state change.

在 Flutter 中， widget 要麼是有狀態的，要麼是無狀態的。
這取決於 widget 是否依賴狀態的改變。

* If a widget changes&mdash;the user interacts with it or
  a data feed interrupts the UI, then it’s *Stateful*.

  如果一個 widget 發生了改變，而它所處的使用者介面或者資料中斷了 UI，
  那麼該 widget 就是 **有狀態** 的。

* If a widget is final or immutable, then it's *Stateless*.

  如果一個 widget 是 final 型別或者 immutable 型別的，
  那麼該 widget 是 **無狀態** 的。

2. Determine which object manages the widget's state (for a `StatefulWidget`).

   確定哪個物件來控制 widget 的狀態（針對 `StatefulWidget`）。

In Flutter, there are three primary ways to manage state:

在 Flutter 中，有三種途徑來管理狀態：

* The widget manages its own state

  widget 管理它的自身狀態

* The parent widget manages the widget’s state

  由其父 widget 管理 widget 狀態

* A mix-and-match approach

  透過混搭的方式

When deciding which approach to use, consider the following principles:

當決定了使用哪個途徑後，要考慮下述的幾個原則：

* If the state in question is user data, for example the checked or unchecked
  mode of a checkbox, or the position of a slider, then the state is best managed
  by the parent widget.

  如果狀態資訊是使用者資料，比如複選框是被勾選還是未被勾選，或者滑塊的位置，
  那麼父 widget 會很好的處理當前 widget 的狀態。

* If the state in question is user data,
  for example the checked or unchecked mode of a checkbox,
  or the position of a slider, then the state is best managed
  by the parent widget.

  如果狀態是和外觀效果相關的，比如動畫，那麼 widget 自己會處理狀態的變化。

* When in doubt, let the parent widget manage the child widget's state.

  如果無法確定，那麼父 widget 會處理子 widget 的狀態。

3. Subclass StatefulWidget and State.

   繼承 `StatefulWidget` 和 `State`。

The `MyStatefulWidget` class manages its own state&mdash;it extends
`StatefulWidget`, it overrides the `createState()`
method to create the `State` object,
and the framework calls `createState()` to build the widget.
In this example, `createState()` creates an instance of
`_MyStatefulWidgetState`, which
is implemented in the next best practice.

`MyStatefulWidget` 類管理它自身的狀態&mdash;&mdash
它繼承自 `StatefulWidget`，重寫了 `createState()` 方法。
該方法建立了 `State` 物件，同時框架會呼叫
`createState()` 方法來建構 widget。
在這個例子中，`createState()` 方法建立了一個 
`_MyStatefulWidgetState` 例項。
下面的最佳實踐中也實現了類似的方法。

<?code-excerpt "lib/best_practices.dart (CreateState)" replace="/return const Text\('Hello World!'\);/\/\/.../g"?>
```dart
class MyStatefulWidget extends StatefulWidget {
  const MyStatefulWidget({
    super.key,
    required this.title,
  });

  final String title;
  @override
  State<MyStatefulWidget> createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  @override
  Widget build(BuildContext context) {
    //...
  }
}
```

4. Add the StatefulWidget into the widget tree.

   將 StatefulWidget 新增到 widget 樹中

Add your custom `StatefulWidget` to the widget tree
in the app’s build method.

將你自訂的 `StatefulWidget` 透過應用程式的
build 方法新增到 widget 樹中。

<?code-excerpt "lib/best_practices.dart (UseStatefulWidget)"?>
```dart
class MyStatelessWidget extends StatelessWidget {
  // This widget is the root of your application.
  const MyStatelessWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Flutter Demo',
      home: MyStatefulWidget(title: 'State Change Demo'),
    );
  }
}
```

{% include docs/android-ios-figure-pair.md image="react-native/state-change.gif" alt="State change" class="border" %}

## Props

In React Native, most components can be customized when they are
created with different parameters or properties, called `props`.
These parameters can be used in a child component using `this.props`.

在 RN 中，大多陣列件都可以在建立的時候透過不同的引數或者屬性來自訂，
叫做 `props`。這些引數可以在子元件中透過 `this.props` 進行呼叫。

```js
// React Native
class CustomCard extends React.Component {
  render() {
    return (
      <View>
        <Text> Card {this.props.index} </Text>
        <Button
          title='Press'
          onPress={() => this.props.onPress(this.props.index)}
        />
      </View>
    );
  }
}
class App extends React.Component {

  onPress = index => {
    console.log('Card ', index);
  };

  render() {
    return (
      <View>
        <FlatList
          data={[ ... ]}
          renderItem={({ item }) => (
            <CustomCard onPress={this.onPress} index={item.key} />
          )}
        />
      </View>
    );
  }
}
```

In Flutter, you assign a local variable or function marked
`final` with the property received in the parameterized constructor.

在 Flutter 中，你可以將建構函式中的引數值賦值
給標記為 `final` 的本地變數或者函式。

<?code-excerpt "lib/components.dart (Components)"?>
```dart
/// Flutter
class CustomCard extends StatelessWidget {
  const CustomCard({
    super.key,
    required this.index,
    required this.onPress,
  });

  final int index;
  final void Function() onPress;

  @override
  Widget build(BuildContext context) {
    return Card(
        child: Column(
      children: <Widget>[
        Text('Card $index'),
        TextButton(
          onPressed: onPress,
          child: const Text('Press'),
        ),
      ],
    ));
  }
}

class UseCard extends StatelessWidget {
  const UseCard({super.key, required this.index});

  final int index;

  @override
  Widget build(BuildContext context) {
    /// Usage
    return CustomCard(
      index: index,
      onPress: () {
        print('Card $index');
      },
    );
  }
}
```

{% include docs/android-ios-figure-pair.md image="react-native/modular.png" alt="Cards" class="border" %}

## Local storage

## 本地儲存

If you don't need to store a lot of data, and it doesn't require
structure, you can use `shared_preferences` which allows you to
read and write persistent key-value pairs of primitive data
types: booleans, floats, ints, longs, and strings.

如果你不需要在本地儲存太多資料同時也不需要儲存結構化資料，
那麼你可以使用 `shared_preferences`，透過它來讀寫一些原始資料型別鍵值對，
資料型別包括布林、浮點、整數、長精度和字串。

### How do I store persistent key-value pairs that are global to the app?

### 如何儲存在應用程式中全域有效的鍵值對？

In React Native, you use the `setItem` and `getItem` functions
of the `AsyncStorage` component to store and retrieve data
that is persistent and global to the app.

在 React Native，可以使用 `AsyncStorage` 中的
`setItem` 和 `getItem` 函式來儲存和讀取應用程式中的全域資料。

```js
// React Native
await AsyncStorage.setItem( 'counterkey', json.stringify(++this.state.counter));
AsyncStorage.getItem('counterkey').then(value => {
  if (value != null) {
    this.setState({ counter: value });
  }
});
```

In Flutter, use the [`shared_preferences`][] plugin to
store and retrieve key-value data that is persistent and global
to the app. The `shared_preferences` plugin wraps
`NSUserDefaults` on iOS and `SharedPreferences` on Android,
providing a persistent store for simple data.
To use the plugin,
add `shared_preferences` as a dependency in the `pubspec.yaml`
file then import the package in your Dart file.

在 Flutter 中，使用 [`shared_preferences`][] 外掛來
儲存和存取應用程式內全域有效的鍵值對資料。
`shared_preferences` 外掛封裝了 iOS 中的 `NSUserDefaults` 和
Android 中的 `SharedPreferences` 來實現簡單資料的持續儲存。
如果要使用該外掛，可以在 `pubspec.yaml` 中新增依賴
`shared_preferences`，然後在 Dart 檔案中參考包即可。

```yaml
dependencies:
  flutter:
    sdk: flutter
  shared_preferences: ^2.0.13
```

<?code-excerpt "lib/examples.dart (SharedPrefs)"?>
```dart
import 'package:shared_preferences/shared_preferences.dart';
```

To implement persistent data, use the setter methods
provided by the `SharedPreferences` class.
Setter methods are available for various primitive
types, such as `setInt`, `setBool`, and `setString`.
To read data, use the appropriate getter method provided
by the `SharedPreferences` class. For each
setter there is a corresponding getter method,
for example, `getInt`, `getBool`, and `getString`.

要實現持久資料儲存，使用 `SharedPreferences` 類提供的 setter 方法即可。
Setter 方法適用於多種原始型別資料，
比如 `setInt`、`setBool`、和 `setString`。
要讀取資料，使用 `SharedPreferences` 類中相應的 getter 方法。
每一個 setter 方法都有對應的 getter 方法，
比如，`getInt`、`getBool` 和 `getString`。

<?code-excerpt "lib/examples.dart (SharedPrefsUpdate)"?>
```dart
Future<void> updateCounter() async {
  final prefs = await SharedPreferences.getInstance();
  int? counter = prefs.getInt('counter');
  if (counter is int) {
    await prefs.setInt('counter', ++counter);
  }
  setState(() {
    _counter = counter;
  });
}
```

## Routing

## 路徑

Most apps contain several screens for displaying different
types of information. For example, you might have a product
screen that displays images where users could tap on a product
image to get more information about the product on a new screen.

大多數應用都會包含多個頁面來顯示不同型別的資料。
比如，你有一個頁面展示商品列表，
使用者可以透過點選其中的任意一個商品，
在另外一個頁面檢視該商品的詳細資訊。

In Android, new screens are new Activities.
In iOS, new screens are new ViewControllers. In Flutter,
screens are just Widgets! And to navigate to new
screens in Flutter, use the Navigator widget.

在 Android 中，新的頁面是 Activity。 在 iOS 中，
新的頁面是 ViewController。在 Flutter 中，頁面也只是 widget，
如果在 Flutter 中要切換頁面，使用 Navigator widget 即可。

### How do I navigate between screens?

### 如何在頁面之間進行切換？

In React Native, there are three main navigators:
StackNavigator, TabNavigator, and DrawerNavigator.
Each provides a way to configure and define the screens.

在 RN 中，有三種主要的導航 widget ：
StackNavigator、TabNavigator 和 DrawerNavigator。
每個都提供了配置和定義頁面的方法。

```js
// React Native
const MyApp = TabNavigator(
  { Home: { screen: HomeScreen }, Notifications: { screen: tabNavScreen } },
  { tabBarOptions: { activeTintColor: '#e91e63' } }
);
const SimpleApp = StackNavigator({
  Home: { screen: MyApp },
  stackScreen: { screen: StackScreen }
});
export default (MyApp1 = DrawerNavigator({
  Home: {
    screen: SimpleApp
  },
  Screen2: {
    screen: drawerScreen
  }
}));
```

In Flutter, there are two main widgets used to navigate between screens:

在 Flutter 中，有兩種主要的 widget 實現頁面之間的切換：

* A [`Route`][] is an abstraction for an app screen or page.

  [`Route`][] 是應用程式頁面的一個抽象類別。

* A [`Navigator`][] is a widget that manages routes.

  [`Navigator`][] 是管理頁面路徑的 widget。

A `Navigator` is defined as a widget that manages a set of child
widgets with a stack discipline. The navigator manages a stack
of `Route` objects and provides methods for managing the stack,
like [`Navigator.push`][] and [`Navigator.pop`][].
A list of routes might be specified in the [`MaterialApp`][] widget,
or they might be built on the fly, for example, in hero animations.
The following example specifies named routes in the `MaterialApp` widget.

`Navigator` 以堆疊的方式管理子 widget。
它的堆疊裡儲存的是 `Route` 物件，並且提供方法管理整個堆疊，
比如 [`Navigator.push`][] 和 [`Navigator.pop`][]。
路徑列表需要在 [`MaterialApp`][] 中指定。
或者在頁面切換的時候進行建構，比如 hero 動畫。
下面的例子在 `MaterialApp` widget 中指定了頁面切換路徑。

{{site.alert.note}}

  Named routes are no longer recommended for most
  applications. For more information, see
  [Limitations][] in the [navigation overview][] page.

  針對大多數的應用情況，我們不再推薦使用命名的路由 (Named routes)，
  瞭解更多資訊，請參考導航概覽中的 [受限情況][Limitations] 部分。

{{site.alert.note}}

[Limitations]: {{site.url}}/development/ui/navigation#limitations
[navigation overview]: {{site.url}}/development/ui/navigation

<?code-excerpt "lib/navigation.dart (Navigator)"?>
```dart
class NavigationApp extends StatelessWidget {
  // This widget is the root of your application.
  const NavigationApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      //...
      routes: <String, WidgetBuilder>{
        '/a': (context) => const UsualNavScreen(),
        '/b': (context) => const DrawerNavScreen(),
      },
      //...
    );
  }
}
```

To navigate to a named route, the [`Navigator.of()`][]
method is used to specify the `BuildContext`
(a handle to the location of a widget in the widget tree).
The name of the route is passed to the `pushNamed` function to
navigate to the specified route.

要切換到一個已命名的路徑，[`Navigator.of()`][] 方法被用於
指定 `BuildContext`（該物件可以定位到 widget 樹中的一個具體的 widget）。
路徑的名稱傳遞到 `pushNamed` 函式來切換至指定的路徑。

<?code-excerpt "lib/navigation.dart (PushNamed)"?>
```dart
Navigator.of(context).pushNamed('/a');
```

You can also use the push method of `Navigator` which
adds the given [`Route`][] to the history of the
navigator that most tightly encloses the given [`BuildContext`][],
and transitions to it. In the following example,
the [`MaterialPageRoute`][] widget is a modal route that
replaces the entire screen with a platform-adaptive
transition. It takes a [`WidgetBuilder`][] as a required parameter.

你可以使用 `Navigator` 中的 push 方法新增
[`Route`][] 到 navigator 的歷史佇列中，
其中包含 [`BuildContext`][] 並且可以切換到指定頁面。
在下面的例子中，[`MaterialPageRoute`][] widget 是一個模式化路徑，
可以將整個頁面透過平台自適應切換方式進行切換。
它需要一個 [`WidgetBuilder`][] 引數。

<?code-excerpt "lib/navigation.dart (NavigatorPush)"?>
```dart
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => const UsualNavScreen(),
  ),
);
```

### How do I use tab navigation and drawer navigation?

### 如何使用 tab 導航和 drawer 導航？ 

In Material Design apps, there are two primary options
for Flutter navigation: tabs and drawers.
When there is insufficient space to support tabs, drawers
provide a good alternative.

在 Material Design 應用程式中，
Flutter 的導航形式主要有兩種：tab 和 drawer。
如果沒有足夠的 widget 可以容納 tab，drawer 就是個不錯的選擇。

#### Tab navigation

#### Tab 導航

In React Native, `createBottomTabNavigator`
and `TabNavigation` are used to
show tabs and for tab navigation.

在 RN 中，`createBottomTabNavigator`
和 `TabNavigation` 用來顯示 tab 和 tab 導航。

```js
// React Native
import { createBottomTabNavigator } from 'react-navigation';

const MyApp = TabNavigator(
  { Home: { screen: HomeScreen }, Notifications: { screen: tabNavScreen } },
  { tabBarOptions: { activeTintColor: '#e91e63' } }
);
```

Flutter provides several specialized widgets for drawer and tab navigation:

Flutter 針對 drawer 和 tab 導航提供幾種專用的 widget：

* [`TabController`][]&mdash;Coordinates
  the tab selection between a TabBar and a TabBarView.

  [`TabController`][]&mdash;&mdash;將 tab 與 TabBar 和 TabBarView 結合起來使用。

* [`TabBar`][]&mdash;Displays
  a horizontal row of tabs.

  [`TabBar`][]&mdash;&mdash;水平顯示一行 tab。

* [`Tab`][]&mdash;Creates
  a material design TabBar tab.

  [`Tab`][]&mdash;&mdash;建立一個 material design 風格的 TabBar 中的 tab。

* [`TabBarView`][]&mdash;Displays
  the widget that corresponds to the currently selected tab.

  [`TabBarView`][]&mdash;&mdash;顯示目前所選 tab 所對應的 widget。

<?code-excerpt "lib/navigation.dart (TabNav)"?>
```dart
class _MyAppState extends State<MyApp> with SingleTickerProviderStateMixin {
  late TabController controller = TabController(length: 2, vsync: this);

  @override
  Widget build(BuildContext context) {
    return TabBar(
      controller: controller,
      tabs: const <Tab>[
        Tab(icon: Icon(Icons.person)),
        Tab(icon: Icon(Icons.email)),
      ],
    );
  }
}
```

A `TabController` is required to coordinate the tab selection
between a `TabBar` and a `TabBarView`.
The `TabController` constructor `length` argument is the total
number of tabs. A `TickerProvider` is required to trigger
the notification whenever a frame triggers a state change.
The `TickerProvider` is `vsync`. Pass the
`vsync: this` argument to the `TabController` constructor
whenever you create a new `TabController`.

要將 tab 選項與 `TabBar` 和 `TabBarView` 結合起來使用就需要 `TabController`。
`TabController` 的建構函式中的 `length` 引數定義了 tab 的總數。
當狀態變化時，需要使用 `TickerProvider` 來觸發通知。
`TickerProvider` 是 `vsync`，當你需要建立新的 `TabController` 時，
將 `vsync: this` 作為建構函式的引數即可。

The [`TickerProvider`][] is an interface implemented
by classes that can vend [`Ticker`][] objects.
Tickers can be used by any object that must be notified whenever a
frame triggers, but they're most commonly used indirectly via an
[`AnimationController`][]. `AnimationController`s
need a `TickerProvider` to obtain their `Ticker`.
If you are creating an AnimationController from a State,
then you can use the [`TickerProviderStateMixin`][]
or [`SingleTickerProviderStateMixin`][]
classes to obtain a suitable `TickerProvider`.

[`TickerProvider`][] 介面可以用於產生 [`Ticker`][] 物件。
當有物件被觸發通知後會用到 Tickers，
不過它通常都是被 [`AnimationController`][] 間接呼叫。
`AnimationController` 需要 `TickerProvider` 來獲得對應的 `Ticker`。 
如果你透過 State 建立了一個 AnimationController，
那麼你就可以使用 [`TickerProviderStateMixin`][]
或者 [`SingleTickerProviderStateMixin`][] 
來獲得對應的 `TickerProvider`。

The [`Scaffold`][] widget wraps a new `TabBar` widget and
creates two tabs. The `TabBarView` widget
is passed as the `body` parameter of the `Scaffold` widget.
All screens corresponding to the `TabBar` widget’s tabs are
children to the `TabBarView` widget along with the same `TabController`.

[`Scaffold`][] 封裝了一個新的 `TabBar` widget，其中包含兩個 tab。
`TabBarView` 作為 `body` 引數傳遞到 `Scaffold` 中。
所有和 `TabBar` 中的 tab 相關的頁面均是 `TabBarView` 的子 widget，
並且都對應同一個 `TabController`。

<?code-excerpt "lib/navigation.dart (NavigationHomePageState)"?>
```dart
class _NavigationHomePageState extends State<NavigationHomePage>
    with SingleTickerProviderStateMixin {
  late TabController controller = TabController(length: 2, vsync: this);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        bottomNavigationBar: Material(
          color: Colors.blue,
          child: TabBar(
            tabs: const <Tab>[
              Tab(
                icon: Icon(Icons.person),
              ),
              Tab(
                icon: Icon(Icons.email),
              ),
            ],
            controller: controller,
          ),
        ),
        body: TabBarView(
          controller: controller,
          children: const <Widget>[HomeScreen(), TabScreen()],
        ));
  }
}
```

#### Drawer navigation

#### Drawer 導航

In React Native, import the needed react-navigation packages and then use
`createDrawerNavigator` and `DrawerNavigation`.

在 RN 中，匯入所需的 react-navigation 套件，
然後使用 `createDrawerNavigator` 和 `DrawerNavigation` 實現。

```js
// React Native
export default (MyApp1 = DrawerNavigator({
  Home: {
    screen: SimpleApp
  },
  Screen2: {
    screen: drawerScreen
  }
}));
```

In Flutter, we can use the `Drawer` widget in combination with a
`Scaffold` to create a layout with a Material Design drawer.
To add a `Drawer` to an app, wrap it in a `Scaffold` widget.
The `Scaffold` widget provides a consistent
visual structure to apps that follow the
[Material Design][] guidelines. It also supports
special Material Design components,
such as `Drawers`, `AppBars`, and `SnackBars`.

在 Flutter 中，我們可以結合 `Drawer` 和 `Scaffold`
一起使用來實現 Material Design 風格的 drawer 佈局。
如果要在應用程式中新增 `Drawer`， 可以將它封裝在 `Scaffold` widget 中。
`Scaffold` widget 提供了一種一致的介面風格，
它遵循 [Material Design][] 的設計原則。
同時它還支援一些特殊的 Material Design 元件，
比如 `Drawers`，`AppBars`， 和 `SnackBars`。

The `Drawer` widget is a Material Design panel that slides
in horizontally from the edge of a `Scaffold` to show navigation
links in an application. You can
provide a [`ElevatedButton`][], a [`Text`][] widget,
or a list of items to display as the child to the `Drawer` widget.
In the following example, the [`ListTile`][]
widget provides the navigation on tap.

`Drawer` 就是一個 Material Design 窗格，
它可以從 `Scaffold` 邊緣水平滑動顯示應用程式的導航選項。
你可以在裡面新增 [`ElevatedButton`][] 或 [`Text`][] 。
或者新增一個列表的元素作為 `Drawer` 的子 widget。
在下面的例子中，[`ListTile`][] 提供了點選導航。

<?code-excerpt "lib/examples.dart (Drawer)"?>
```dart
@override
Widget build(BuildContext context) {
  return Drawer(
    elevation: 20.0,
    child: ListTile(
      leading: const Icon(Icons.change_history),
      title: const Text('Screen2'),
      onTap: () {
        Navigator.of(context).pushNamed('/b');
      },
    ),
  );
}
```

The `Scaffold` widget also includes an `AppBar` widget that automatically
displays an appropriate IconButton to show the `Drawer` when a Drawer is
available in the `Scaffold`. The `Scaffold` automatically handles the
edge-swipe gesture to show the `Drawer`.

`Scaffold` 還包含一個 `AppBar`。
它會自動顯示一個圖示按鈕來表明 `Scaffold` 中有一個`Drawer`。
`Scaffold` 會自動處理邊緣的滑動手勢來顯示 `Drawer`。

<?code-excerpt "lib/examples.dart (Scaffold)"?>
```dart
@override
Widget build(BuildContext context) {
  return Scaffold(
    drawer: Drawer(
      elevation: 20.0,
      child: ListTile(
        leading: const Icon(Icons.change_history),
        title: const Text('Screen2'),
        onTap: () {
          Navigator.of(context).pushNamed('/b');
        },
      ),
    ),
    appBar: AppBar(title: const Text('Home')),
    body: Container(),
  );
}
```

{% include docs/android-ios-figure-pair.md image="react-native/navigation.gif" alt="Navigation" class="border" %}

## Gesture detection and touch event handling

## 手勢檢測和觸控事件處理

To listen for and respond to gestures,
Flutter supports taps, drags, and scaling.
The gesture system in Flutter has two separate layers.
The first layer includes raw pointer events,
which describe the location and movement of pointers,
(such as touches, mice, and styli movements), across the screen.
The second layer includes gestures,
which describe semantic actions
that consist of one or more pointer movements.

Flutter 支援點選、拖拽和縮放手勢來監聽和相應手勢操作。
Flutter 中的手勢處理有兩個獨立的層。第一層是指標事件，
指標事件定義了指標在螢幕上的位置和動作，比如觸控、滑鼠和觸控筆。
第二層指手勢，主要是語義層面的動作，裡面包含一種或者多種指標動作。

### How do I add a click or press listeners to a widget?

### 如何為 widget 新增點選或者按壓的監聽器？

In React Native, listeners are added to components
using `PanResponder` or the `Touchable` components.

在 RN 中，使用 `PanResponder` 或者 `Touchable` 元件來新增監聽器。

```js
// React Native
<TouchableOpacity
  onPress={() => {
    console.log('Press');
  }}
  onLongPress={() => {
    console.log('Long Press');
  }}
>
  <Text>Tap or Long Press</Text>
</TouchableOpacity>
```

For more complex gestures and combining several touches into
a single gesture, [`PanResponder`][] is used.

對於更加複雜手勢以及將多個觸控新增到單獨的一個手勢中，
可以使用 [`PanResponder`][]。

```js
// React Native
class App extends Component {

  componentWillMount() {
    this._panResponder = PanResponder.create({
      onMoveShouldSetPanResponder: (event, gestureState) =>
        !!getDirection(gestureState),
      onPanResponderMove: (event, gestureState) => true,
      onPanResponderRelease: (event, gestureState) => {
        const drag = getDirection(gestureState);
      },
      onPanResponderTerminationRequest: (event, gestureState) => true
    });
  }

  render() {
    return (
      <View style={styles.container} {...this._panResponder.panHandlers}>
        <View style={styles.center}>
          <Text>Swipe Horizontally or Vertically</Text>
        </View>
      </View>
    );
  }
}
```

In Flutter, to add a click (or press) listener to a widget,
use a button or a touchable widget that has an `onPress: field`.
Or, add gesture detection to any widget by wrapping it
in a [`GestureDetector`][].

在 Flutter 中，要為 widget 新增點選或者按壓監聽器，
使用帶有 `onPress: field` 的按鈕或者可觸控 widget 即可。
或者，用任何 widget 封裝 [`GestureDetector`][]，在其中新增手勢檢測。

<?code-excerpt "lib/examples.dart (GestureDetector)"?>
```dart
@override
Widget build(BuildContext context) {
  return GestureDetector(
    child: Scaffold(
      appBar: AppBar(title: const Text('Gestures')),
      body: Center(
          child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: const <Widget>[
          Text('Tap, Long Press, Swipe Horizontally or Vertically'),
        ],
      )),
    ),
    onTap: () {
      print('Tapped');
    },
    onLongPress: () {
      print('Long Pressed');
    },
    onVerticalDragEnd: (value) {
      print('Swiped Vertically');
    },
    onHorizontalDragEnd: (value) {
      print('Swiped Horizontally');
    },
  );
}
```

For more information, including a list of
Flutter `GestureDetector` callbacks,
see the [GestureDetector class][].

如果想要了解更多詳細內容，包括 Flutter 的
`GestureDetector` 回呼(Callback)函式的列表，
請檢視頁面 [GestureDetector 類][GestureDetector class]。

[GestureDetector class]: {{site.api}}/flutter/widgets/GestureDetector-class.html#instance-properties

{% include docs/android-ios-figure-pair.md image="react-native/flutter-gestures.gif" alt="Gestures" class="border" %}

## Making HTTP network requests

## 發起 HTTP 網路請求

Fetching data from the internet is common for most apps. And in Flutter,
the `http` package provides the simplest way to fetch data from the internet.

對於大多數應用程式來說都需要從網際網路上獲取資料。
在 Flutter 中，`http` 套件提供了從網際網路獲取資料的最簡單的途徑。

### How do I fetch data from API calls?

### 如何透過 API 呼叫來獲得資料呢？

React Native provides the Fetch API for networking—you make a fetch request
and then receive the response to get the data.

RN 提供 Fetch API 實現網路程式設計，
你可以發起請求，然後接收響應來獲得資料。

```js
// React Native
_getIPAddress = () => {
  fetch('https://httpbin.org/ip')
    .then(response => response.json())
    .then(responseJson => {
      this.setState({ _ipAddress: responseJson.origin });
    })
    .catch(error => {
      console.error(error);
    });
};
```

Flutter uses the `http` package. To install the `http` package,
add it to the dependencies' section of our pubspec.yaml.

Flutter 使用 `http` package。如果要安裝 `http` package，
將它新增到 pubspec.yaml 的 dependencies 部分。

```yaml
dependencies:
  flutter:
    sdk: flutter
  http: <latest_version>
```

Flutter uses the [`dart:io`][] core HTTP support client.
To create an HTTP Client, import `dart:io`.

Flutter 使用 [`dart:io`][] 提供核心的 HTTP 客戶端支援，
要建立一個 HTTP 客戶端，參考 `dart:io`。

<?code-excerpt "lib/examples.dart (ImportDartIO)"?>
```dart
import 'dart:io';
```

The client supports the following HTTP operations:
GET, POST, PUT, and DELETE.

客戶端支援如下所列的 HTTP 操作：GET, POST, PUT 和 DELETE。

<?code-excerpt "lib/examples.dart (HTTP)"?>
```dart
final url = Uri.parse('https://httpbin.org/ip');
final httpClient = HttpClient();

Future<void> getIPAddress() async {
  final request = await httpClient.getUrl(url);
  final response = await request.close();
  final responseBody = await response.transform(utf8.decoder).join();
  final String ip = jsonDecode(responseBody)['origin'];
  setState(() {
    _ipAddress = ip;
  });
}
```

{% include docs/android-ios-figure-pair.md image="react-native/api-calls.gif" alt="API calls" class="border" %}

## Form input

## 輸入表單

Text fields allow users to type text into your app so they can be
used to build forms, messaging apps, search experiences, and more.
Flutter provides two core text field widgets:
[`TextField`][] and [`TextFormField`][].

TextField 用於在應用程式中輸入文字，這樣就可以實現建立
表單、短訊息應用、搜尋框等等功能。Flutter 提供兩個核心文字輸入 widget：
[`TextField`][] 和 [`TextFormField`][].

### How do I use text field widgets?

### 如何使用文字輸入 widget ？

In React Native, to enter text you use a `TextInput` component to show a text
input box and then use the callback to store the value in a variable.

在 RN 裡，可以使用 `TextInput` 元件來輸入文字，
它會顯示一個輸入框，然後透過回呼(Callback)函式來傳遞輸入值。

```js
// React Native
<TextInput
  placeholder="Enter your Password"
  onChangeText={password => this.setState({ password })}
 />
<Button title="Submit" onPress={this.validate} />
```

In Flutter, use the [`TextEditingController`][]
class to manage a `TextField` widget.
Whenever the text field is modified,
the controller notifies its listeners.

在 Flutter 中，使用 [`TextEditingController`][]
類來管理 `TextField` widget。
當用戶修改文字的時候，controller 會通知監聽器。

Listeners read the text and selection properties to
learn what the user typed into the field.
You can access the text in `TextField`
by the `text` property of the controller.

監聽器讀取文字和選項屬性來獲知使用者所輸入的內容。
你可以透過 `TextField` 中的 `text`
屬性獲得使用者輸入的文字資料。

<?code-excerpt "lib/examples.dart (TextEditingController)"?>
```dart
final TextEditingController _controller = TextEditingController();

@override
Widget build(BuildContext context) {
  return Column(children: [
    TextField(
      controller: _controller,
      decoration: const InputDecoration(
        hintText: 'Type something',
        labelText: 'Text Field',
      ),
    ),
    ElevatedButton(
      child: const Text('Submit'),
      onPressed: () {
        showDialog(
            context: context,
            builder: (context) {
              return AlertDialog(
                title: const Text('Alert'),
                content: Text('You typed ${_controller.text}'),
              );
            });
      },
    ),
  ]);
}
```

In this example, when a user clicks on the submit button an alert dialog
displays the current text entered in the text field.
This is achieved using an [`AlertDialog`][]
widget that displays the alert message, and the text from
the `TextField` is accessed by the `text` property of the
[`TextEditingController`][].

在這個例子中，當用戶點選提交按鈕的時候，會彈出視窗顯示當前輸入的文字內容。
可以使用 [`alertDialog`][] widget 顯示提示資訊，
`TextField` 的文字透過 `text` 屬性來獲得，
該屬性屬於 [`TextEditingController`][]。

### How do I use Form widgets?

### 如何使用 Form widget 呢？

In Flutter, use the [`Form`][] widget where
[`TextFormField`][] widgets along with the submit
button are passed as children.
The `TextFormField` widget has a parameter called
[`onSaved`][] that takes a callback and executes
when the form is saved. A `FormState`
object is used to save, reset, or validate
each `FormField` that is a descendant of this `Form`.
To obtain the `FormState`, you can use `Form.of()`
with a context whose ancestor is the `Form`,
or pass a `GlobalKey` to the `Form` constructor and call
`GlobalKey.currentState()`.

在 Flutter 中，當需要使用帶有提交按鈕和 [`TextFormField`][]
元件的複合 widget 時，就會用到 [`Form`][]。
`TextFormField` 內含一個 [`onSaved`][] 引數，
它可以設定一個回呼(Callback)函式，當表單儲存的時候會回呼(Callback)該函式。
`FormState` 用於儲存、重置或者驗證 `Form` 內含的每個 `FormField`。
你可以透過將當前表單的 context 屬性賦值給 `Form.of` 來獲得 `FormState`。
或者在表單的構造函數里使用 `GlobalKey`，
然後呼叫 `GlobalKey.currentState` 來獲得 `FormState`。

<?code-excerpt "lib/examples.dart (FormState)"?>
```dart
@override
Widget build(BuildContext context) {
  return Form(
    key: formKey,
    child: Column(
      children: <Widget>[
        TextFormField(
          validator: (value) {
            if (value != null && value.contains('@')) {
              return null;
            }
            return 'Not a valid email.';
          },
          onSaved: (val) {
            _email = val;
          },
          decoration: const InputDecoration(
            hintText: 'Enter your email',
            labelText: 'Email',
          ),
        ),
        ElevatedButton(
          onPressed: _submit,
          child: const Text('Login'),
        ),
      ],
    ),
  );
}
```

The following example shows how `Form.save()` and `formKey`
(which is a `GlobalKey`), are used to save the form on submit.

下面的範例程式碼展示了 `Form.save()` 和 `formKey`
（使用 `GlobalKey`）如何被用於表單提交的。

<?code-excerpt "lib/examples.dart (FormSubmit)"?>
```dart
void _submit() {
  final form = formKey.currentState;
  if (form != null && form.validate()) {
    form.save();
    showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
            title: const Text('Alert'),
            content: Text('Email: $_email, password: $_password'));
      },
    );
  }
}
```

{% include docs/android-ios-figure-pair.md image="react-native/input-fields.gif" alt="Input" class="border" %}

## Platform-specific code

## 平台相關程式碼

When building a cross-platform app, you want to re-use as much code as
possible across platforms. However, scenarios might arise where it
makes sense for the code to be different depending on the OS.
This requires a separate implementation by declaring a specific platform.

當建構跨平臺應用程式的時候，你會盡量多地複用程式碼。
然而，根據不同的應用場景，程式碼會根據平台的不同有所變化。
這就需要提前宣告具體的平台來進行獨立的實現。

In React Native, the following implementation would be used:

在 RN 中，下面的實現程式碼會被用到：

```js
// React Native
if (Platform.OS === 'ios') {
  return 'iOS';
} else if (Platform.OS === 'android') {
  return 'android';
} else {
  return 'not recognised';
}
```

In Flutter, use the following implementation:

而在 Flutter 中，則是下面這樣的實現：

<?code-excerpt "lib/examples.dart (Platform)"?>
```dart
final platform = Theme.of(context).platform;
if (platform == TargetPlatform.iOS) {
  return 'iOS';
}
if (platform == TargetPlatform.android) {
  return 'android';
}
if (platform == TargetPlatform.fuchsia) {
  return 'fuchsia';
}
return 'not recognized ';
```

## Debugging

## 除錯

### What tools can I use to debug my app in Flutter?

### 應該使用什麼工具除錯我的 Flutter 應用？

Use the [DevTools][] suite for debugging Flutter or Dart apps.

請使用 [DevTools][] 除錯你的 Flutter 和 Dart 應用。

DevTools includes support for profiling, examining the heap,
inspecting the widget tree, logging diagnostics, debugging,
observing executed lines of code, debugging memory leaks and memory
fragmentation. For more information, see the
[DevTools][] documentation.

開發者工具套件含了 profiling 建構、檢查堆疊、
檢視 widget 樹、診斷資訊記錄、除錯、
執行程式碼行觀察、除錯記憶體洩漏和記憶體碎片等。
有關更多資訊，請參閱 [DevTools][] 文件。

### How do I perform a hot reload?

### 如何進行熱重載？

Flutter’s Stateful Hot Reload feature helps you quickly and easily experiment,
build UIs, add features, and fix bugs. Instead of recompiling your app
every time you make a change, you can hot reload your app instantly.
The app is updated to reflect your change,
and the current state of the app is preserved.

Flutter 的熱重載特性可以幫助你快速便捷地實驗、建構 UI 和各種特性以及修復 bug。
每次修改程式碼以後，你只需直接熱重載你的應用程式即可，而無需重新進行編譯。
應用程式會根據你的修改進行相應的更新，而程式原有的狀態則會被保留。

In React Native,
the shortcut is ⌘R for the iOS Simulator and tapping R twice on
Android emulators.

在 RN 中，iOS 模擬器對應的快捷鍵是 ⌘R，
對應 Android 模擬器的快捷鍵是點選兩次 R。

In Flutter, If you are using IntelliJ IDE or Android Studio,
you can select Save All (⌘s/ctrl-s), or you can click the
Hot Reload button on the toolbar. If you
are running the app at the command line using `flutter run`,
type `r` in the Terminal window.
You can also perform a full restart by typing `R` in the
Terminal window.

在 Flutter 中，如果你使用的是 IntelliJ 或者 Android Studio，
可以使用 Save All (⌘s/ctrl-s)，或者可以點選工具欄上的 Hot Reload 按鈕。
如果你是在命令列裡使用 `flutter run` 命令執行的程式，在窗口裡輸入 `r` 即可。
也可以輸入 `R` 進行徹底的重啟。

### How do I access the in-app developer menu?

### 如何開啟程式裡的開發者選單？

In React Native, the developer menu can be accessed by shaking your device: ⌘D
for the iOS Simulator or ⌘M for Android emulator.

在 RN 中，開發者選單可以透過搖動裝置開啟：
對於 iOS 模擬器的快捷鍵是 ⌘D 而 Android 模擬器的快捷鍵是 ⌘M。

In Flutter, if you are using an IDE, you can use the IDE tools. If you start
your application using `flutter run` you can also access the menu by typing `h`
in the terminal window, or type the following shortcuts:

在 Flutter 中，如果你使用 IDE，那麼可以直接使用 IDE 工具。
如果你是透過命令列執行 `flutter run` 來啟動應用程式的，
你可以在命令列視窗透過輸入 `h` 來開啟選單，或者參考下面的快捷鍵說明：

<div class="table-wrapper" markdown="1">
| Action| Terminal Shortcut| Debug functions and properties|
| :------- | :------: | :------ |
| 功能| 命令列快捷鍵| 除錯功能和屬性|
| Widget hierarchy of the app| `w`| debugDumpApp()|
| 應用程式的 widget 層級| `w`| debugDumpApp()|
| Rendering tree of the app | `t`| debugDumpRenderTree()|
| 渲染程式的 widget 樹 | `t`| debugDumpRenderTree()|
| Layers| `L`| debugDumpLayerTree()|
| 層| `L`| debugDumpLayerTree()|
| Accessibility | `S` (traversal order) or<br>`U` (inverse hit test order)|debugDumpSemantics()|
| 無障礙 | `S` (遍歷順序) 或者<br>`U` (反轉點選測試順序)|debugDumpSemantics()|
| To toggle the widget inspector | `i` | WidgetsApp. showWidgetInspectorOverride|
| 開啟或者關閉 widget 視窗 | `i` | WidgetsApp. showWidgetInspectorOverride|
| To toggle the display of construction lines| `p` | debugPaintSizeEnabled|
| 顯示或者隱藏框架線條| `p` | debugPaintSizeEnabled|
| To simulate different operating systems| `o` | defaultTargetPlatform|
| 模擬不同的作業系統| `o` | defaultTargetPlatform|
| To display the performance overlay | `P` | WidgetsApp. showPerformanceOverlay|
| 疊加顯示效能引數| `P` | WidgetsApp. showPerformanceOverlay|
| To save a screenshot to flutter. png| `s` ||
| 將截圖儲存為 flutter.png| `s` ||
| To quit| `q` ||
| 退出| `q` ||
{:.table.table-striped}
</div>

## Animation

## 動畫

Well-designed animation makes a UI feel intuitive,
contributes to the look and feel of a polished app,
and improves the user experience.
Flutter’s animation support makes it easy
to implement simple and complex animations.
The Flutter SDK includes many Material Design widgets
that include standard motion effects,
and you can easily customize these effects
to personalize your app.

精美的動畫效果會使得 UI 更加直觀，可以提升整體視覺效果，
使應用顯得更加精緻，從而提升使用者體驗。
Flutter 的動畫框架使得開發者能夠更方便地實現簡單和複雜的動畫。
Flutter SDK 含有很多 Material Design widget。
其中已經包括了標準的動畫效果，你可以很方便地自訂這些效果。

In React Native, Animated APIs are used to create animations.

在 RN 中，動畫 API 用於建立動畫。

In Flutter, use the [`Animation`][]
class and the [`AnimationController`][] class.
`Animation` is an abstract class that understands its
current value and its state (completed or dismissed).
The `AnimationController` class lets you
play an animation forward or in reverse,
or stop animation and set the animation
to a specific value to customize the motion.

在 Flutter 中，使用 [`Animation`][] 類和
[`AnimationController`][] 類實現動畫。
`Animation` 是抽象類別，內含其當前的值和它的狀態（已完成或者已取消）。
`AnimationController` 類可以正向或者反向播放動畫或者
停止動畫以及為動畫設定特定值來自訂動畫。

### How do I add a simple fade-in animation?

### 如何新增一個簡單的淡入動畫效果？

In the React Native example below, an animated component,
`FadeInView` is created using the Animated API.
The initial opacity state, final state, and the
duration over which the transition occurs are defined.
The animation component is added inside the `Animated` component,
the opacity state `fadeAnim` is mapped
to the opacity of the `Text` component that we want to animate,
and then, `start()` is called to start the animation.

在下面的 React Native 範例中，有一個動畫元件，也就是 `FadeInView`，
它是使用 Animated API 建立的。定義了初始的不透明狀態，
最終狀態和動畫切換之間的時間間隔。在 `Animated` 中添加了動畫元件，
不透明狀態 `fadeAnim` 對映到我們想要新增動畫效果的文字元件上，
然後在開始動畫的時候呼叫 `start()`。

```js
// React Native
class FadeInView extends React.Component {
  state = {
    fadeAnim: new Animated.Value(0) // Initial value for opacity: 0
  };
  componentDidMount() {
    Animated.timing(this.state.fadeAnim, {
      toValue: 1,
      duration: 10000
    }).start();
  }
  render() {
    return (
      <Animated.View style={%raw%}{{...this.props.style, opacity: this.state.fadeAnim }}{%endraw%} >
        {this.props.children}
      </Animated.View>
    );
  }
}
    ...
<FadeInView>
  <Text> Fading in </Text>
</FadeInView>
    ...
```

To create the same animation in Flutter, create an
[`AnimationController`][] object named `controller`
and specify the duration. By default, an `AnimationController`
linearly produces values that range from 0.0 to 1.0,
during a given duration. The animation controller generates a new value
whenever the device running your app is ready to display a new frame.
Typically, this rate is around 60 values per second.

要在 Flutter 中實現相同的動畫效果，
建立一個 [`AnimationController`][] 物件，
叫它 `controller`，並且指定時間間隔。
在預設配置下，`AnimationController`
會在給定時間間隔線性地產生從 0.0 到 1.0 的數值。
當你的程式可以顯示新一幀畫面的時候，`AnimationController` 會產生一個新的值。
通常這個頻率為每秒 60 次。

When defining an `AnimationController`,
you must pass in a `vsync` object.
The presence of `vsync` prevents offscreen
animations from consuming unnecessary resources.
You can use your stateful object as the `vsync` by adding
`TickerProviderStateMixin` to the class definition.
An `AnimationController` needs a TickerProvider,
which is configured using the `vsync` argument on the constructor.

當定義 `AnimationController` 的時候，你必須傳入一個 `vsync` 物件。
`vsync` 會防止螢幕顯示區域之外的動畫消耗不必要的資源。
你可以透過新增 `TickerProviderStateMixin` 到類定義中來使用有狀態的物件。
`AnimationController` 需要傳入一個 TickerProvider，
它是透過構造函數里的 `vsync` 引數進行配置的。

A [`Tween`][] describes the interpolation between a
beginning and ending value or the mapping from an input
range to an output range. To use a `Tween` object
with an animation, call the `Tween` object's `animate()`
method and pass it the `Animation` object that you want to modify.

[`Tween`][] 定義了起始和結束值之間或者輸入段到輸出段之間的過渡。
如果要在動畫中使用 `Tween` 物件，呼叫 `Tween` 物件的 `animate` 方法，
然後把它賦給你要修改的 `Animation` 物件。

For this example, a [`FadeTransition`][]
widget is used and the `opacity` property is
mapped to the `animation` object.

在這個例子中，用到了 [`FadeTransition`][] widget，
它的 `opacity` 屬性對映到了 `animation` 物件上。

To start the animation, use `controller.forward()`.
Other operations can also be performed using the
controller such as `fling()` or `repeat()`.
For this example, the [`FlutterLogo`][]
widget is used inside the `FadeTransition` widget.

要開始動畫，使用 `controller.forward()`。
其它的操作也可以使用控制器裡的方法，
比如 `fling()` 或者 `repeat()`。
這個例子裡，[`FlutterLogo`][] widget 
被用於 `FadeTransition` widget 中。

<?code-excerpt "lib/animation.dart"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const Center(child: LogoFade()));
}

class LogoFade extends StatefulWidget {
  const LogoFade({super.key});

  @override
  State<LogoFade> createState() => _LogoFadeState();
}

class _LogoFadeState extends State<LogoFade>
    with SingleTickerProviderStateMixin {
  late Animation<double> animation;
  late AnimationController controller;

  @override
  void initState() {
    super.initState();
    controller = AnimationController(
      duration: const Duration(milliseconds: 3000),
      vsync: this,
    );
    final CurvedAnimation curve = CurvedAnimation(
      parent: controller,
      curve: Curves.easeIn,
    );
    animation = Tween(begin: 0.0, end: 1.0).animate(curve);
    controller.forward();
  }

  @override
  void dispose() {
    controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return FadeTransition(
      opacity: animation,
      child: const SizedBox(
        height: 300.0,
        width: 300.0,
        child: FlutterLogo(),
      ),
    );
  }
}
```

{% include docs/android-ios-figure-pair.md image="react-native/flutter-fade.gif" alt="Flutter fade" class="border" %}

### How do I add swipe animation to cards?

### 如何為卡片新增滑動動畫呢？

In React Native, either the `PanResponder` or
third-party libraries are used for swipe animation.

在 RN 中，無論  `PanResponder` 或者第三方庫都可被用於滑動動畫。

In Flutter, to add a swipe animation, use the
[`Dismissible`][] widget and nest the child widgets.

在 Flutter 中，要新增滑動動畫，
使用 [`Dismissible`][] widget 封裝其它子 widget 即可。

<?code-excerpt "lib/examples.dart (Dismissable)"?>
```dart
return Dismissible(
  key: Key(widget.key.toString()),
  onDismissed: (dismissDirection) {
    cards.removeLast();
  },
  child: Container(
      //...
      ),
);
```

{% include docs/android-ios-figure-pair.md image="react-native/card-swipe.gif" alt="Card swipe" class="border" %}

## React Native and Flutter widget equivalent components

## React Native 和 Flutter widget 對等的元件

The following table lists commonly-used React Native
components mapped to the corresponding Flutter widget
and common widget properties.

下面的表格列舉了通用的 React Native 元件與對應的
Flutter widget 和通用的 widget 屬性。

<div class="table-wrapper" markdown="1">
| React Native 元件   | Flutter Widget      | 描述                                                     |
| ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| [`Button`](https://facebook.github.io/react-native/docs/button.html)                        | [`ElevatedButton`][]                           | 基礎的凸起按鈕                                                                           |
|                                                                                           |  onPressed [required]                                                                                        | 該回調函式在當按鈕被點選的時候被觸發。                                                         |
|                                                                                           | Child                                                                              | 按鈕的標籤                                                                     |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [`Button`](https://facebook.github.io/react-native/docs/button.html)                        | [`TextButton`][]                               | 基礎的扁平化按鈕.                                                                                                         |
|                                                                                           |  onPressed [required]                                                                                        | 當按鈕被點選的時候觸發該回調函式。                                                          |
|                                                                                           | Child             | 按鈕的標籤                                                                                                     |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [`ScrollView`](https://facebook.github.io/react-native/docs/scrollview.html)                | [`ListView`][]                                    | 一個可滑動的縱向排列的 widget 列表。|
||        children                                                                              | 	( <Widget\> [ ])  要顯示的子 widget 列表
||controller |[ [`ScrollController`][] ] 可用於控制滑動 widget 的物件
||itemExtent|[ double ] 如果非空，那麼強制所有子 widget 在滑動方向上增加給定的距離
||scroll Direction|[ [`Axis`][] ] 滑動頁面的滑動軸
||                                                                                                            |                                                                                                                                        |
| [`FlatList`](https://facebook.github.io/react-native/docs/flatlist.html)                    | [`ListView.builder`][]               | 根據需要建立的一組 widget 的建構函式。
||itemBuilder [required] |[[`IndexedWidgetBuilder`][]] 根據需要建立子 widget。當元素序號大於等於零並且小於佇列元素總數時，該回調函式會被呼叫。
||itemCount |[ int ] 優化了 ListView 對於最大滑動範圍的預估能力。
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [`Image`](https://facebook.github.io/react-native/docs/image.html)                         | [`Image`][]                                           | 顯示圖片的 widget。                                                                                                      |
|                                                                                           |  image [required]                                                                                          | 要顯示的圖片                                                                                                             |
|                                                                                           | Image. asset                                                                                                | 有多個建構函式可以用於指定圖片。                                                |
|                                                                                           | width, height, color, alignment                                                                            | 圖片的風格和佈局。                                                                                                      |
|                                                                                           | fit                                                                                                        | 將圖片內嵌到佈局對應的空間裡。                                                                           |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [`Modal`](https://facebook.github.io/react-native/docs/modal.html)                          | [`ModalRoute`][]                                | 避免和之前路徑交叉的路徑。                                          |
|                                                                                           | animation                                                                                                  | 路徑切換的動畫和之前路徑向前切換的動畫。                                         |
|                                                                                           |                                                                                                            |                                                                                                                                        |
|  [`ActivityIndicator`](https://facebook.github.io/react-native/docs/activityindicator.html) | [`LinearProgressIndicator`][] | 一個進度條 widget。                                                                                       |
|                                                                                           | strokeWidth                                                                                                | 圓形線條的寬度。                                                                                        |
|                                                                                           | backgroundColor                                                                                            | 指示進度的背景色。預設是當前主題的 `ThemeData.backgroundColor`。                         |
|                                                                                           |                                                                                                            |                                                                                                                                        |
|  [`ActivityIndicator`](https://facebook.github.io/react-native/docs/activityindicator.html) | [`LinearProgressIndicator`][]     | 一個水平條形的進度條。                                                                                          |
|                                                                                           | value                                                                                                      | 進度條的進度值。                                                                                            |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [`RefreshControl`](https://facebook.github.io/react-native/docs/refreshcontrol.html)        | [`RefreshIndicator`][]                   | 支援 Material 中滑動重新整理的 widget  |
|                                                                                           | color                                                                                                      | 進度指示的前景色。                                                                                      |
|                                                                                           | onRefresh                                                                                                  | 	當用戶拖拽重新整理指示器想要重新整理的時候會呼叫該函式。  |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [`View`](https://facebook.github.io/react-native/docs/view.html)                            | [`Container`][]                                  | 封裝子 widget 的 widget。                                                                                                               |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [`View`](https://facebook.github.io/react-native/docs/view.html)                            | [`Column`][]                                        | 將子 widget 縱向排列的 widget。                                                                            |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [`View`](https://facebook.github.io/react-native/docs/view.html)                            | [`Row`][]                                              | 將子 widget 橫向排列的 widget。                                                                              |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [`View`](https://facebook.github.io/react-native/docs/view.html)                            | [`Center`][]                                        | 將子 widget 放置於中央的 widget。                                                      |                                                                                                            |                                                                                                                                        |
| [`View`](https://facebook.github.io/react-native/docs/view.html)                            | [`Padding`][]                                      | 將子 widget 按照給定的間隔進行排列的 widget。                                                                 |
|                                                                                           | padding [required]                                                                                         | [ EdgeInsets ] 子 widget 間隔。
|||
| [`TouchableOpacity`](https://facebook.github.io/react-native/docs/touchableopacity.html)    | [`GestureDetector`][]                      | 檢測手勢的 widget。                                                                                                                     |
|                                                                                           | onTap                                                                                                      | 當點選的時候會呼叫。                                                                                                 |
|                                                                                           | onDoubleTap                                                                                                | 當兩次點選的時候會呼叫。
|||
| [`TextInput`](https://facebook.github.io/react-native/docs/textinput.html)                | [`TextInput`][]                                   | 呼叫系統文字輸入的介面。                                                 |
|                                                                                           | controller                                                                                                 | [ [`TextEditingController`][] ] 用於獲取或者修改文字。
|||
| [`Text`](https://facebook.github.io/react-native/docs/text.html)                          | [`Text`][]                                            | 以單一的樣式顯示文字的文字 widget。                                                                                                                                                                       |
|                                                                                         | data                                                                                                      | [ String ] 要顯示的文字。                                                                                                                                                                         |
|                                                                                         | textDirection                                                                                             | [ [`TextAlign`][] ] 文字的方向。        |
|                                                                                         |                                                                                                           |                                                                                                                                                                                                              |
| [`Switch`](https://facebook.github.io/react-native/docs/switch.html)                      | [`Switch`][]                                      | Material Design 樣式的開關。                                           |
|                                                                                         | value [required]                                                                                          | [ boolean ] 開關的開啟或者閉合狀態。                                                                                                                                                        |
|                                                                                         | onChanged [required]                                                                                      | [ callback ] 當用戶點選開關的時候呼叫。                                                                 |
|                                                                                         |                                                                                                           |                                                                                                                                                                                                              |
| [`Slider`](https://facebook.github.io/react-native/docs/slider.html)                      | [`Slider`][]                                      |選擇一個範圍的值。                                                                                                                      |
|                                                                                         | value [required]                                                                                          | [ double ] 當前滑動器的值。                                      |
|                                                                                         | onChanged [required]                                                                                      | 當用戶為滑動器選擇了新的值時會呼叫           |
{:.table.table-striped}
</div>


[`AboutDialog`]: {{site.api}}/flutter/material/AboutDialog-class.html
[Adding Assets and Images in Flutter]: {{site.url}}/development/ui/assets-and-images
[`AlertDialog`]: {{site.api}}/flutter/material/AlertDialog-class.html
[`Align`]: {{site.api}}/flutter/widgets/Align-class.html
[`Animation`]: {{site.api}}/flutter/animation/Animation-class.html
[`AnimationController`]: {{site.api}}/flutter/animation/AnimationController-class.html
[async and await]: {{site.dart-site}}/guides/language/language-tour#asynchrony-support
[`Axis`]: {{site.api}}/flutter/painting/Axis-class.html
[`BuildContext`]: {{site.api}}/flutter/widgets/BuildContext-class.html
[`Center`]: {{site.api}}/flutter/widgets/Center-class.html
[color palette]: {{site.material}}/design/color
[colors]: {{site.api}}/flutter/material/Colors-class.html
[`Colors`]: {{site.api}}/flutter/material/Colors-class.html
[`Column`]: {{site.api}}/flutter/widgets/Column-class.html
[`Container`]: {{site.api}}/flutter/widgets/Container-class.html
[`Checkbox`]: {{site.api}}/flutter/material/Checkbox-class.html
[`CircleAvatar`]: {{site.api}}/flutter/material/CircleAvatar-class.html
[`CircularProgressIndicator`]: {{site.api}}/flutter/material/CircularProgressIndicator-class.html
[Cupertino (iOS-style)]: {{site.url}}/development/ui/widgets/cupertino
[`CustomPaint`]: {{site.api}}/flutter/widgets/CustomPaint-class.html
[`CustomPainter`]: {{site.api}}/flutter/rendering/CustomPainter-class.html
[Dart]: {{site.dart-site}}/dart-2
[Dart's Type System]: {{site.dart-site}}/guides/language/sound-dart
[Sound Null Safety]: {{site.dart-site}}/null-safety
[`dart:io`]: {{site.api}}/flutter/dart-io/dart-io-library.html
[DartPadA]: {{site.dartpad}}/0df636e00f348bdec2bc1c8ebc7daeb1
[DartPadB]: {{site.dartpad}}/cf9e652f77636224d3e37d96dcf238e5
[DartPadC]: {{site.dartpad}}/3f4625c16e05eec396d6046883739612
[DartPadD]: {{site.dartpad}}/57ec21faa8b6fe2326ffd74e9781a2c7
[DartPadE]: {{site.dartpad}}/c85038ad677963cb6dc943eb1a0b72e6
[DartPadF]: {{site.dartpad}}/5454e8bfadf3000179d19b9bc6be9918
[Developing Packages & Plugins]: {{site.url}}/development/packages-and-plugins/developing-packages
[DevTools]: {{site.url}}/development/tools/devtools
[`Dismissible`]: {{site.api}}/flutter/widgets/Dismissible-class.html
[`FadeTransition`]: {{site.api}}/flutter/widgets/FadeTransition-class.html
[Flutter packages]: {{site.pub}}/flutter/
[Flutter Architectural Overview]: {{site.url}}/resources/architectural-overview
[Flutter Basic Widgets]: {{site.url}}/development/ui/widgets/basics
[Flutter Technical Overview]: {{site.url}}/resources/architectural-overview
[Flutter Widget Catalog]: {{site.url}}/development/ui/widgets
[Flutter Widget Index]: {{site.url}}/reference/widgets
[`FlutterLogo`]: {{site.api}}/flutter/material/FlutterLogo-class.html
[`Form`]: {{site.api}}/flutter/widgets/Form-class.html
[`TextButton`]: {{site.api}}/flutter/material/TextButton-class.html
[functions]: {{site.dart-site}}/guides/language/language-tour#functions
[`Future`]: {{site.dart-site}}/tutorials/language/futures
[`GestureDetector`]: {{site.api}}/flutter/widgets/GestureDetector-class.html
[Getting started]: {{site.url}}/get-started
[`Image`]: {{site.api}}/flutter/widgets/Image-class.html
[`IndexedWidgetBuilder`]: {{site.api}}/flutter/widgets/IndexedWidgetBuilder.html
[`InheritedWidget`]: {{site.api}}/flutter/widgets/InheritedWidget-class.html
[`InkWell`]: {{site.api}}/flutter/material/InkWell-class.html
[Layout Widgets]: {{site.url}}/development/ui/widgets/layout
[`LinearProgressIndicator`]: {{site.api}}/flutter/material/LinearProgressIndicator-class.html
[`ListTile`]: {{site.api}}/flutter/material/ListTile-class.html
[`ListView`]: {{site.api}}/flutter/widgets/ListView-class.html
[`ListView.builder`]: {{site.api}}/flutter/widgets/ListView/ListView.builder.html
[Material Design]: {{site.material}}/design
[Material icons]: {{site.api}}/flutter/material/Icons-class.html
[`MaterialApp`]: {{site.api}}/flutter/material/MaterialApp-class.html
[`MaterialPageRoute`]: {{site.api}}/flutter/material/MaterialPageRoute-class.html
[`ModalRoute`]: {{site.api}}/flutter/widgets/ModalRoute-class.html
[`Navigator`]: {{site.api}}/flutter/widgets/Navigator-class.html
[`Navigator.of()`]: {{site.api}}/flutter/widgets/Navigator/of.html
[`Navigator.pop`]: {{site.api}}/flutter/widgets/Navigator/pop.html
[`Navigator.push`]: {{site.api}}/flutter/widgets/Navigator/push.html
[`onSaved`]: {{site.api}}/flutter/widgets/FormField/onSaved.html
[optional parameters]: {{site.dart-site}}/guides/language/language-tour#optional-parameters
[`Padding`]: {{site.api}}/flutter/widgets/Padding-class.html
[`PanResponder`]: https://facebook.github.io/react-native/docs/panresponder.html
[pub.dev]: {{site.pub}}
[`Radio`]: {{site.api}}/flutter/material/Radio-class.html
[`ElevatedButton`]: {{site.api}}/flutter/material/ElevatedButton-class.html
[`RefreshIndicator`]: {{site.api}}/flutter/material/RefreshIndicator-class.html
[`Route`]: {{site.api}}/flutter/widgets/Route-class.html
[`Row`]: {{site.api}}/flutter/widgets/Row-class.html
[`Scaffold`]: {{site.api}}/flutter/material/Scaffold-class.html
[`ScrollController`]: {{site.api}}/flutter/widgets/ScrollController-class.html
[`shared_preferences`]: {{site.repo.plugins}}/tree/main/packages/shared_preferences
[`SingleTickerProviderStateMixin`]: {{site.api}}/flutter/widgets/SingleTickerProviderStateMixin-mixin.html
[`Slider`]: {{site.api}}/flutter/material/Slider-class.html
[`Stack`]: {{site.api}}/flutter/widgets/Stack-class.html
[State management]: {{site.url}}/development/data-and-backend/state-mgmt
[`StatefulWidget`]: {{site.api}}/flutter/widgets/StatefulWidget-class.html
[`StatelessWidget`]: {{site.api}}/flutter/widgets/StatelessWidget-class.html
[`Switch`]: {{site.api}}/flutter/material/Switch-class.html
[`Tab`]: {{site.api}}/flutter/material/Tab-class.html
[`TabBar`]: {{site.api}}/flutter/material/TabBar-class.html
[`TabBarView`]: {{site.api}}/flutter/material/TabBarView-class.html
[`TabController`]: {{site.api}}/flutter/material/TabController-class.html
[`Text`]: {{site.api}}/flutter/widgets/Text-class.html
[`TextAlign`]: {{site.api}}/flutter/dart-ui/TextAlign-class.html
[`TextEditingController`]: {{site.api}}/flutter/widgets/TextEditingController-class.html
[`TextField`]: {{site.api}}/flutter/material/TextField-class.html
[`TextFormField`]: {{site.api}}/flutter/material/TextFormField-class.html
[`TextInput`]: {{site.api}}/flutter/services/TextInput-class.html
[`TextStyle`]: {{site.api}}/flutter/dart-ui/TextStyle-class.html
[`Theme`]: {{site.api}}/flutter/material/Theme-class.html
[`ThemeData`]: {{site.api}}/flutter/material/ThemeData-class.html
[`Ticker`]: {{site.api}}/flutter/scheduler/Ticker-class.html
[`TickerProvider`]: {{site.api}}/flutter/scheduler/TickerProvider-class.html
[`TickerProviderStateMixin`]: {{site.api}}/flutter/widgets/TickerProviderStateMixin-mixin.html
[`Tween`]: {{site.api}}/flutter/animation/Tween-class.html
[Using Packages]: {{site.url}}/development/packages-and-plugins/using-packages
[variables]: {{site.dart-site}}/guides/language/language-tour#variables
[`WidgetBuilder`]: {{site.api}}/flutter/widgets/WidgetBuilder.html
[Write Your First Flutter App, Part 1]: {{site.codelabs}}/codelabs/first-flutter-app-pt1