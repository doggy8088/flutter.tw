---
title: Parse JSON in the background
title: 在後台處理 JSON 資料解析
description: How to perform a task in the background.
description: 如何在後台執行任務。
tags: cookbook, 實用課程, 網路請求
keywords: json,後臺任務,排程,fps,卡頓
prev:
  title: Make authenticated requests
  title: 發起 HTTP 認證授權請求
  path: /docs/cookbook/networking/authenticated-requests
next:
  title: Send data to the internet
  title: 傳送網路資料請求
  path: /docs/cookbook/networking/send-data
---

<?code-excerpt path-base="cookbook/networking/background_parsing/"?>

By default, Dart apps do all of their work on a single thread.
In many cases, this model simplifies coding and is fast enough
that it does not result in poor app performance or stuttering animations,
often called "jank."

Dart 應用通常只會在單執行緒中處理它們的工作。
並且在大多數情況中，
這種模式不但簡化了程式碼而且速度也夠快，
基本不會出現像動畫卡頓以及效能不足這種「不靠譜」的問題。

However, you might need to perform an expensive computation, such as parsing a
very large JSON document. If this work takes more than 16 milliseconds,
your users experience jank.

但是，當你需要進行一個非常複雜的計算時，例如解析一個巨大的 JSON 文件。
如果這項工作耗時超過了 16 毫秒，那麼你的使用者就會感受到掉幀。

To avoid jank, you need to perform expensive computations like this in the
background. On Android, this means scheduling work on a different thread.
In Flutter, you can use a separate [Isolate][].
This recipe uses the following steps:

為了避免掉幀，像上面那樣消耗效能的計算就應該放在後台處理。
在 Android 平臺上，這意味著你需要在不同的執行緒中進行排程工作。
而在 Flutter 中，你可以使用一個單獨的 [Isolate][]。

## Directions

## 使用步驟

  1. Add the `http` package.

     新增 `http` 這個 package；

  2. Make a network request using the `http` package.

     使用 `http` package 發起一個網路請求；

  3. Convert the response into a list of photos.

     將響應轉換成一列照片；

  4. Move this work to a separate isolate.

     將這個工作移交給一個單獨的 isolate。

## 1. Add the `http` package

## 1. 新增 `http` 包

First, add the [`http`][] package to your project.
The `http` package makes it easier to perform network
requests, such as fetching data from a JSON endpoint.

首先，在你的專案中新增 [`http`][] 這個 package，
`http` package 會讓網路請求變的像從 JSON 端點獲取資料一樣簡單。

```yaml
dependencies:
  http: <latest_version>
```

## 2. Make a network request

## 2. 發起一個網路請求

This example covers how to fetch a large JSON document
that contains a list of 5000 photo objects from the
[JSONPlaceholder REST API][],
using the [`http.get()`][] method.

在這個例子中，你將會使用 [`http.get()`][] 方法透過
[JSONPlaceholder REST API][] 獲取到一個包含
5000 張圖片物件的超大 JSON 文件。

<?code-excerpt "lib/main_step2.dart (fetchPhotos)"?>
```dart
Future<http.Response> fetchPhotos(http.Client client) async {
  return client.get(Uri.parse('https://jsonplaceholder.typicode.com/photos'));
}
```

{{site.alert.note}}

  You're providing an `http.Client` to the function in this example.
  This makes the function easier to test and use in different environments.

  在這個例子中你需要給方法添加了一個 `http.Client` 引數。
  這將使得該方法測試起來更容易同時也可以在不同環境中使用。
{{site.alert.end}}

## 3. Parse and convert the JSON into a list of photos

## 3. 解析並將 json 轉換成一列圖片

Next, following the guidance from the
[Fetch data from the internet][] recipe,
convert the `http.Response` into a list of Dart objects.
This makes the data easier to work with.

接下來，根據 [獲取網路資料][Fetch data from the internet] 的說明，
為了讓接下來的資料處理更簡單，
你需要將 `http.Response` 轉換成一列 Dart 物件。

### Create a `Photo` class

### 建立一個 `Photo` 類

First, create a `Photo` class that contains data about a photo.
Include a `fromJson()` factory method to make it easy to create a
`Photo` starting with a JSON object.

首先，建立一個包含圖片資料的 `Photo` 類別。
還需要一個 `fromJson` 的工廠方法，
使得透過 json 建立 `Photo` 變的更加方便。

<?code-excerpt "lib/main_step3.dart (Photo)"?>
```dart
class Photo {
  final int albumId;
  final int id;
  final String title;
  final String url;
  final String thumbnailUrl;

  const Photo({
    required this.albumId,
    required this.id,
    required this.title,
    required this.url,
    required this.thumbnailUrl,
  });

  factory Photo.fromJson(Map<String, dynamic> json) {
    return Photo(
      albumId: json['albumId'] as int,
      id: json['id'] as int,
      title: json['title'] as String,
      url: json['url'] as String,
      thumbnailUrl: json['thumbnailUrl'] as String,
    );
  }
}
```

### Convert the response into a list of photos

### 將響應轉換成一列圖片

Now, use the following instructions to update the
`fetchPhotos()` function so that it returns a
`Future<List<Photo>>`:

現在，為了讓 `fetchPhotos()` 方法可以返回一個
`Future<List<Photo>>`，我們需要以下兩點更新：

  1. Create a `parsePhotos()` function that converts the response
     body into a `List<Photo>`.

     建立一個可以將回應內文轉換成 `List<Photo>` 的方法：`parsePhotos()` 
  
  2. Use the `parsePhotos()` function in the `fetchPhotos()` function.

     在 `fetchPhotos()` 方法中使用 `parsePhotos()` 方法

<?code-excerpt "lib/main_step3.dart (parsePhotos)"?>
```dart
// A function that converts a response body into a List<Photo>.
List<Photo> parsePhotos(String responseBody) {
  final parsed = jsonDecode(responseBody).cast<Map<String, dynamic>>();

  return parsed.map<Photo>((json) => Photo.fromJson(json)).toList();
}

Future<List<Photo>> fetchPhotos(http.Client client) async {
  final response = await client
      .get(Uri.parse('https://jsonplaceholder.typicode.com/photos'));

  // Use the compute function to run parsePhotos in a separate isolate.
  return parsePhotos(response.body);
}
```

## 4. Move this work to a separate isolate

## 4. 將這部分工作移交到單獨的 isolate 中

If you run the `fetchPhotos()` function on a slower device,
you might notice the app freezes for a brief moment as it parses and
converts the JSON. This is jank, and you want to get rid of it.

如果你在一臺很慢的手機上執行 `fetchPhotos()` 函式，
你或許會注意到應用會有點卡頓，因為它需要解析並轉換 json。
顯然這並不好，所以你要避免它。

You can remove the jank by moving the parsing and conversion
to a background isolate using the [`compute()`][]
function provided by Flutter. The `compute()` function runs expensive
functions in a background isolate and returns the result. In this case,
run the `parsePhotos()` function in the background.

那麼我們究竟可以做什麼呢？
那就是透過 Flutter 提供的 [`compute()`][] 方法
將解析和轉換的工作移交到一個後臺 isolate 中。
這個 `compute()` 函式可以在後台 isolate 中運行復雜的函式並返回結果。
在這裡，我們就需要將 `parsePhotos()` 方法放入後臺。

<?code-excerpt "lib/main.dart (fetchPhotos)"?>
```dart
Future<List<Photo>> fetchPhotos(http.Client client) async {
  final response = await client
      .get(Uri.parse('https://jsonplaceholder.typicode.com/photos'));

  // Use the compute function to run parsePhotos in a separate isolate.
  return compute(parsePhotos, response.body);
}
```

## Notes on working with isolates

## 使用 Isolates 需要注意的地方

Isolates communicate by passing messages back and forth. These messages can
be primitive values, such as `null`, `num`, `bool`, `double`, or `String`, or
simple objects such as the `List<Photo>` in this example.

Isolates 透過來回傳遞訊息來交流。這些訊息可以是任何值，
它們可以是 `null`、`num`、`bool`、`double` 或者 `String`，
哪怕是像這個例子中的 `List<Photo>` 這樣簡單物件都沒問題。

You might experience errors if you try to pass more complex objects,
such as a `Future` or `http.Response` between isolates.

當你試圖傳遞更復雜的物件時，你可能會遇到錯誤，
例如在 isolates 之間的 `Future` 或者 `http.Response`。

As an alternate solution, check out the [`worker_manager`][] or
[`workmanager`][] packages for background processing.

與此同時，後臺處理序的其他解決方案是使用
[`worker_manager`][] 或 [`workmanager`][] package。

[`worker_manager`]:  {{site.pub}}/packages/worker_manager
[`workmanager`]: {{site.pub}}/packages/workmanager

## Complete example

## 完整範例

<?code-excerpt "lib/main.dart"?>
```dart
import 'dart:async';
import 'dart:convert';

import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

Future<List<Photo>> fetchPhotos(http.Client client) async {
  final response = await client
      .get(Uri.parse('https://jsonplaceholder.typicode.com/photos'));

  // Use the compute function to run parsePhotos in a separate isolate.
  return compute(parsePhotos, response.body);
}

// A function that converts a response body into a List<Photo>.
List<Photo> parsePhotos(String responseBody) {
  final parsed = jsonDecode(responseBody).cast<Map<String, dynamic>>();

  return parsed.map<Photo>((json) => Photo.fromJson(json)).toList();
}

class Photo {
  final int albumId;
  final int id;
  final String title;
  final String url;
  final String thumbnailUrl;

  const Photo({
    required this.albumId,
    required this.id,
    required this.title,
    required this.url,
    required this.thumbnailUrl,
  });

  factory Photo.fromJson(Map<String, dynamic> json) {
    return Photo(
      albumId: json['albumId'] as int,
      id: json['id'] as int,
      title: json['title'] as String,
      url: json['url'] as String,
      thumbnailUrl: json['thumbnailUrl'] as String,
    );
  }
}

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    const appTitle = 'Isolate Demo';

    return const MaterialApp(
      title: appTitle,
      home: MyHomePage(title: appTitle),
    );
  }
}

class MyHomePage extends StatelessWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: FutureBuilder<List<Photo>>(
        future: fetchPhotos(http.Client()),
        builder: (context, snapshot) {
          if (snapshot.hasError) {
            return const Center(
              child: Text('An error has occurred!'),
            );
          } else if (snapshot.hasData) {
            return PhotosList(photos: snapshot.data!);
          } else {
            return const Center(
              child: CircularProgressIndicator(),
            );
          }
        },
      ),
    );
  }
}

class PhotosList extends StatelessWidget {
  const PhotosList({super.key, required this.photos});

  final List<Photo> photos;

  @override
  Widget build(BuildContext context) {
    return GridView.builder(
      gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
        crossAxisCount: 2,
      ),
      itemCount: photos.length,
      itemBuilder: (context, index) {
        return Image.network(photos[index].thumbnailUrl);
      },
    );
  }
}
```

![Isolate demo]({{site.url}}/assets/images/docs/cookbook/isolate.gif){:.site-mobile-screenshot}

[`compute()`]: {{site.api}}/flutter/foundation/compute-constant.html
[Fetch data from the internet]: {{site.url}}/cookbook/networking/fetch-data
[`http`]: {{site.pub-pkg}}/http
[`http.get()`]: {{site.pub-api}}/http/latest/http/get.html
[Isolate]: {{site.api}}/flutter/dart-isolate/Isolate-class.html
[JSONPlaceholder REST API]: https://jsonplaceholder.typicode.com
