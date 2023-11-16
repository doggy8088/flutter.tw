---
title: Mock dependencies using Mockito
title: 使用 Mockito 模擬依賴關係
description: >
  Use the Mockito package to mimic the behavior of services for testing.
description: 使用 Mockito package 在測試中模擬伺服器端行為。
short-title: Mocking
tags: cookbook, 實用課程, 測試
keywords: 依賴關係測試,Mockito,flaky test
---

<?code-excerpt path-base="cookbook/testing/unit/mocking"?>

Sometimes, unit tests might depend on classes that fetch data from live
web services or databases. This is inconvenient for a few reasons:

某些情況下，單元測試可能會依賴需要從線上 Web 服務或資料庫中獲取資料的類別。
這樣會帶來一些不便，原因如下： 

  * Calling live services or databases slows down test execution.

    存取線上服務或資料庫會拖慢測試執行效率。

  * A passing test might start failing if a web service or database returns
    unexpected results. This is known as a "flaky test."

    原本可以透過的測試可能會失敗，
    因為 Web 服務或資料庫可能會返回不符合預期的結果。
    這種情況被稱作“flaky test”。

  * It is difficult to test all possible success and failure scenarios
    by using a live web service or database.
    
    使用線上 web 服務或資料庫來測試很難覆蓋全所有可能成功或失敗的場景。

Therefore, rather than relying on a live web service or database,
you can "mock" these dependencies. Mocks allow emulating a live
web service or database and return specific results depending
on the situation.

因此，最好不要依賴線上 web 服務或資料庫，我們可以把這些依賴“模擬（mock）”出來 。
模擬（Mocks）允許我們仿造一個線上服務或資料庫，並且可以根據條件返回特定結果。

Generally speaking, you can mock dependencies by creating an alternative
implementation of a class. Write these alternative implementations by
hand or make use of the [Mockito package][] as a shortcut.

通常來說，可以透過建立類別的另一種實現來模擬（mock）這種依賴。
類別的另一種實現可以手寫，也可以藉助 [Mockito 包]({{site.pub-pkg}}/mockito)，
後者簡單一些。

This recipe demonstrates the basics of mocking with the
Mockito package using the following steps:

本篇課程介紹了 Mockito 套件的基本用法，可以參考以下步驟：

## Directions

## 使用步驟

  1. Add the package dependencies.

     新增 `mockito` 和 `test` 依賴

  2. Create a function to test.

     建立一個要測試的函式

  3. Create a test file with a mock `http.Client`.

     建立一個模擬了 `http.Client` 的測試檔案

  4. Write a test for each condition.

     給每一個條件寫一個測試

  5. Run the tests.

     執行這些測試

For more information, see the [Mockito package][] documentation.

更多資訊可以查閱 [Mockito][Mockito package] package 的官方文件。

## 1. Add the package dependencies

## 1. 新增 package 依賴

To use the `mockito` package, add it to the
`pubspec.yaml` file along with the `flutter_test` dependency in the
`dev_dependencies` section.

為了使用 mockito 套件，首先將其和 `flutter_test`
的依賴一起新增到 `pubspec.yaml` 檔案的 `dev_dependencies` 部分：

This example also uses the `http` package,
so define that dependency in the `dependencies` section.

本例中還使用了 `http` 套件，需要新增到 `dependencies` 部分：

`mockito: 5.0.0` supports Dart's null safety thanks to code generation.
To run the required code generation, add the `build_runner` dependency
in the `dev_dependencies` section.

感謝程式碼產生器，`mockito: 5.0.0` 已經支援了 Dart 的空安全。
要執行所需的程式碼產生器工具，請將 `build_runner` 依賴新增到 `dev_dependencies` 專案下。

To add the dependencies, run `flutter pub add`:

執行 `flutter pub add` 新增依賴：

```terminal
$ flutter pub add http dev:mockito dev:build_runner
```

## 2. Create a function to test

## 2. 建立一個要測試的函式

In this example, unit test the `fetchAlbum` function from the
[Fetch data from the internet][] recipe.
To test this function, make two changes:

本例中，我們要對 [獲取網路資料][Fetch data from the internet]
章節的 `fetchAlbum` 函式進行單元測試。
為了便於測試，我們需要做兩個改動：

  1. Provide an `http.Client` to the function. This allows providing the
     correct `http.Client` depending on the situation.
     For Flutter and server-side projects, provide an `http.IOClient`.
     For Browser apps, provide an `http.BrowserClient`.
     For tests, provide a mock `http.Client`.
  
     給函式提供一個 `http.Client`。這樣的話我們可以在不同情形下提供相應的
     `http.Client` 例項。如果是 Flutter 以及伺服器端專案，
     可以提供 `http.IOClient `。如果是瀏覽器應用，
     可以提供 `http.BrowserClient`。為了測試，
     我們要提供一個模擬的 `http.Client`。

  2. Use the provided `client` to fetch data from the internet,
     rather than the static `http.get()` method, which is difficult to mock.

     使用上面提供的 `client` 來請求網路資料，
     不要用 `http.get()` 這個靜態方法，因為它比較難以模擬。

The function should now look like this:

函式經過改動之後：

<?code-excerpt "lib/main.dart (fetchAlbum)"?>
```dart
Future<Album> fetchAlbum(http.Client client) async {
  final response = await client
      .get(Uri.parse('https://jsonplaceholder.typicode.com/albums/1'));

  if (response.statusCode == 200) {
    // If the server did return a 200 OK response,
    // then parse the JSON.
    return Album.fromJson(jsonDecode(response.body) as Map<String, dynamic>);
  } else {
    // If the server did not return a 200 OK response,
    // then throw an exception.
    throw Exception('Failed to load album');
  }
}
```

In your app code, you can provide an `http.Client` to the `fetchAlbum` method 
directly with `fetchAlbum(http.Client())`. `http.Client()` creates a default
`http.Client`.

## 3. Create a test file with a mock `http.Client`

## 3. 建立一個模擬了 http.Client 的測試檔案

Next, create a test file.

接下來，建立一個測試檔案。

Following the advice in the [Introduction to unit testing][] recipe,
create a file called `fetch_album_test.dart` in the root `test` folder.

遵循 [單元測試介紹][Introduction to unit testing] 章節的建議，
我們在根目錄下的 `test` 資料夾中建立一個名字為 `fetch_post_test.dart` 的檔案。

Add the annotation `@GenerateMocks([http.Client])` to the main
function to generate a `MockClient` class with `mockito`.

在 main 函式上新增一個 `@GenerateMocks([http.Client])` 註解以產生含有 `mockito` 的 `MockClient` 類別。

The generated `MockClient` class implements the `http.Client` class.
This allows you to pass the `MockClient` to the `fetchAlbum` function,
and return different http responses in each test.

`MockClient` 類實現了 `http.Client` 類別。
如此一來，我們就可以把 `MockClient` 傳給 `fetchPost` 函式，
還可以在每個測試中返回不同的 http 請求結果。

The generated mocks will be located in `fetch_album_test.mocks.dart`.
Import this file to use them.

產生的 mock 檔案將會放在 `fetch_album_test.mocks.dart`，請匯入以使用它。

<?code-excerpt "test/fetch_album_test.dart (mockClient)" plaster="none"?>
```dart
import 'package:http/http.dart' as http;
import 'package:mocking/main.dart';
import 'package:mockito/annotations.dart';

// Generate a MockClient using the Mockito package.
// Create new instances of this class in each test.
@GenerateMocks([http.Client])
void main() {
}
```

Next, generate the mocks running the following command:

```terminal
$ dart run build_runner build
```

## 4. Write a test for each condition

## 4. 給每一個條件寫一個測試

The `fetchAlbum()` function does one of two things:

回過頭來看，`fetchPost()` 函式會完成下面兩件事中的一件：

  1. Returns an `Album` if the http call succeeds

     如果 http 請求成功，返回 `Post`

  2. Throws an `Exception` if the http call fails
      
     如果 http 請求失敗，丟擲 `Exception`


Therefore, you want to test these two conditions.
Use the `MockClient` class to return an "Ok" response
for the success test, and an error response for the unsuccessful test.

因此，我們要測試這兩種條件。可以使用 `MockClient`
類為成功的測試返回一個 "OK" 的請求結果，
為不成功的測試返回一個錯誤的請求結果。

Test these conditions using the `when()` function provided by
Mockito:

我們使用 Mockito 的 `when()` 函式來達到以上目的：

<?code-excerpt "test/fetch_album_test.dart"?>
```dart
import 'package:flutter_test/flutter_test.dart';
import 'package:http/http.dart' as http;
import 'package:mocking/main.dart';
import 'package:mockito/annotations.dart';
import 'package:mockito/mockito.dart';

import 'fetch_album_test.mocks.dart';

// Generate a MockClient using the Mockito package.
// Create new instances of this class in each test.
@GenerateMocks([http.Client])
void main() {
  group('fetchAlbum', () {
    test('returns an Album if the http call completes successfully', () async {
      final client = MockClient();

      // Use Mockito to return a successful response when it calls the
      // provided http.Client.
      when(client
              .get(Uri.parse('https://jsonplaceholder.typicode.com/albums/1')))
          .thenAnswer((_) async =>
              http.Response('{"userId": 1, "id": 2, "title": "mock"}', 200));

      expect(await fetchAlbum(client), isA<Album>());
    });

    test('throws an exception if the http call completes with an error', () {
      final client = MockClient();

      // Use Mockito to return an unsuccessful response when it calls the
      // provided http.Client.
      when(client
              .get(Uri.parse('https://jsonplaceholder.typicode.com/albums/1')))
          .thenAnswer((_) async => http.Response('Not Found', 404));

      expect(fetchAlbum(client), throwsException);
    });
  });
}
```

## 5. Run the tests

## 5. 執行測試

Now that you have a `fetchAlbum()` function with tests in place,
run the tests.

現在我們有了一個帶測試的 `fetchAlbum()` 函式，開始執行測試！

```terminal
$ flutter test test/fetch_album_test.dart
```

You can also run tests inside your favorite editor by following the
instructions in the [Introduction to unit testing][] recipe.

你也可以參考
[單元測試介紹][Introduction to unit testing]
章節用自己喜歡的編輯器來執行測試。

## Complete example

## 完整的範例

##### lib/main.dart

<?code-excerpt "lib/main.dart"?>
```dart
import 'dart:async';
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

Future<Album> fetchAlbum(http.Client client) async {
  final response = await client
      .get(Uri.parse('https://jsonplaceholder.typicode.com/albums/1'));

  if (response.statusCode == 200) {
    // If the server did return a 200 OK response,
    // then parse the JSON.
    return Album.fromJson(jsonDecode(response.body) as Map<String, dynamic>);
  } else {
    // If the server did not return a 200 OK response,
    // then throw an exception.
    throw Exception('Failed to load album');
  }
}

class Album {
  final int userId;
  final int id;
  final String title;

  const Album({required this.userId, required this.id, required this.title});

  factory Album.fromJson(Map<String, dynamic> json) {
    return Album(
      userId: json['userId'] as int,
      id: json['id'] as int,
      title: json['title'] as String,
    );
  }
}

void main() => runApp(const MyApp());

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  late final Future<Album> futureAlbum;

  @override
  void initState() {
    super.initState();
    futureAlbum = fetchAlbum(http.Client());
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Fetch Data Example',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Fetch Data Example'),
        ),
        body: Center(
          child: FutureBuilder<Album>(
            future: futureAlbum,
            builder: (context, snapshot) {
              if (snapshot.hasData) {
                return Text(snapshot.data!.title);
              } else if (snapshot.hasError) {
                return Text('${snapshot.error}');
              }

              // By default, show a loading spinner.
              return const CircularProgressIndicator();
            },
          ),
        ),
      ),
    );
  }
}
```

##### test/fetch_album_test.dart

<?code-excerpt "test/fetch_album_test.dart"?>
```dart
import 'package:flutter_test/flutter_test.dart';
import 'package:http/http.dart' as http;
import 'package:mocking/main.dart';
import 'package:mockito/annotations.dart';
import 'package:mockito/mockito.dart';

import 'fetch_album_test.mocks.dart';

// Generate a MockClient using the Mockito package.
// Create new instances of this class in each test.
@GenerateMocks([http.Client])
void main() {
  group('fetchAlbum', () {
    test('returns an Album if the http call completes successfully', () async {
      final client = MockClient();

      // Use Mockito to return a successful response when it calls the
      // provided http.Client.
      when(client
              .get(Uri.parse('https://jsonplaceholder.typicode.com/albums/1')))
          .thenAnswer((_) async =>
              http.Response('{"userId": 1, "id": 2, "title": "mock"}', 200));

      expect(await fetchAlbum(client), isA<Album>());
    });

    test('throws an exception if the http call completes with an error', () {
      final client = MockClient();

      // Use Mockito to return an unsuccessful response when it calls the
      // provided http.Client.
      when(client
              .get(Uri.parse('https://jsonplaceholder.typicode.com/albums/1')))
          .thenAnswer((_) async => http.Response('Not Found', 404));

      expect(fetchAlbum(client), throwsException);
    });
  });
}
```

## Summary

## 總結

In this example, you've learned how to use Mockito to test functions or classes
that depend on web services or databases. This is only a short introduction to
the Mockito library and the concept of mocking. For more information,
see the documentation provided by the [Mockito package][].

透過本例，我們已經學會了如何用 Mockito 來測試對
web 服務或資料庫有依賴的函式或類別。
這裡只是簡短地介紹了 Mockito 庫以及模擬（mocking）的概念。
更多內容請移步至 [Mockito package][]。

[Fetch data from the internet]: {{site.url}}/cookbook/networking/fetch-data
[Introduction to unit testing]: {{site.url}}/cookbook/testing/unit/introduction
[Mockito package]: {{site.pub-pkg}}/mockito
