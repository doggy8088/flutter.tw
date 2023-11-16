---
title: Make authenticated requests
title: 發起 HTTP 認證授權請求
description: How to fetch authorized data from a web service.
description: 如何從 web 服務中獲取授權資訊。
tags: cookbook, 實用課程, 網路請求
keywords: HTTP 認證授權請求
---

<?code-excerpt path-base="cookbook/networking/authenticated_requests/"?>

To fetch data from most web services, you need to provide
authorization. There are many ways to do this,
but perhaps the most common uses the `Authorization` HTTP header.

為了從眾多的網路服務中獲取資料，你需要提供相應的授權認證資訊。
當然了，解決這一問題的方法有很多，
而最常見的方法或許就是使用 `Authorization` HTTP header 了。

## Add authorization headers

## 新增 Authorization Headers

The [`http`][] package provides a
convenient way to add headers to your requests.
Alternatively, use the [`HttpHeaders`][]
class from the `dart:io` library.

[`http`][] 這個 package 提供了相當實用的方法來向請求中新增 headers，
你也可以使用 `dart:io` 來使用一些常見的 [`HttpHeaders`][]。

<?code-excerpt "lib/main.dart (get)"?>
```dart
final response = await http.get(
  Uri.parse('https://jsonplaceholder.typicode.com/albums/1'),
  // Send authorization headers to the backend.
  headers: {
    HttpHeaders.authorizationHeader: 'Basic your_api_token_here',
  },
);
```

## Complete example

## 完整範例

This example builds upon the
[Fetching data from the internet][] recipe.

下面的例子是基於
[獲取網路資料][Fetching data from the internet] 中的方法編寫的。

<?code-excerpt "lib/main.dart"?>
```dart
import 'dart:async';
import 'dart:convert';
import 'dart:io';

import 'package:http/http.dart' as http;

Future<Album> fetchAlbum() async {
  final response = await http.get(
    Uri.parse('https://jsonplaceholder.typicode.com/albums/1'),
    // Send authorization headers to the backend.
    headers: {
      HttpHeaders.authorizationHeader: 'Basic your_api_token_here',
    },
  );
  final responseJson = jsonDecode(response.body) as Map<String, dynamic>;

  return Album.fromJson(responseJson);
}

class Album {
  final int userId;
  final int id;
  final String title;

  const Album({
    required this.userId,
    required this.id,
    required this.title,
  });

  factory Album.fromJson(Map<String, dynamic> json) {
    return Album(
      userId: json['userId'] as int,
      id: json['id'] as int,
      title: json['title'] as String,
    );
  }
}
```


[Fetching data from the internet]: {{site.url}}/cookbook/networking/fetch-data
[`http`]: {{site.pub-pkg}}/http
[`HttpHeaders`]: {{site.dart.api}}/stable/dart-io/HttpHeaders-class.html
