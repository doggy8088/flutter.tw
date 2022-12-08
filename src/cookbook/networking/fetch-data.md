---
title: Fetch data from the internet
title: 獲取網路資料
description: How to fetch data over the internet using the http package.
description: 如何使用 http 包獲取網路資料。
prev:
  title: Delete data on the internet
  title: 刪除網路資料
  path: /docs/cookbook/networking/delete-data
next:
  title: Make authenticated requests
  title: 發起認證的請求
  path: /docs/cookbook/networking/authenticated-requests
---

<?code-excerpt path-base="cookbook/networking/fetch_data/"?>

Fetching data from the internet is necessary for most apps.
Luckily, Dart and Flutter provide tools, such as the
`http` package, for this type of work.

對於大部分應用來說，獲取網路資料都是必不可少的一個功能。
幸運的是，Dart 和 Flutter 就為我們提供了這樣的工具。

This recipe uses the following steps:

這個課程包含以下步驟：

  1. Add the `http` package.

     新增 `http` 包

  2. Make a network request using the `http` package.

     使用 `http` 套件進行網路請求

  3. Convert the response into a custom Dart object.

     將返回的響應轉換成一個自訂的 Dart 物件

  4. Fetch and display the data with Flutter.

     使用 Flutter 對資料進行獲取和展示

## 1. Add the `http` package

## 1. 新增 `http` 包

The [`http`][] package provides the
simplest way to fetch data from the internet.

[`http`][] 包為我們提供了獲取網路資料最簡單的方法。

To install the `http` package, add it to the
dependencies section of the `pubspec.yaml` file.
You can find the latest version of the
[`http` package][] the pub.dev.

安裝 `http` 包之前，你必須先把它新增到 `pubspec.yaml` 的依賴區域。
你可以在 [pub.dev 找到 http 套件的最新版本][`http` package]。

```yaml
dependencies:
  http: <latest_version>
```

Import the http package.

<?code-excerpt "lib/main.dart (Http)"?>
```dart
import 'package:http/http.dart' as http;
```

Additionally, in your AndroidManifest.xml file, 
add the Internet permission.

<!-- skip -->
```xml
<!-- Required to fetch data from the internet. -->
<uses-permission android:name="android.permission.INTERNET" />
```

## 2. Make a network request

## 2. 進行網路請求

This recipe covers how to fetch a sample album from the
[JSONPlaceholder][] using the [`http.get()`][] method.

在這裡，你可以使用 [`http.get()`][] 方法
從 [JSONPlaceholder][] 上獲取到一個樣本相簿資料。

<?code-excerpt "lib/main_step1.dart (fetchAlbum)"?>
```dart
Future<http.Response> fetchAlbum() {
  return http.get(Uri.parse('https://jsonplaceholder.typicode.com/albums/1'));
}
```

The `http.get()` method returns a `Future` that contains a `Response`.

這個 `http.get()` 方法會返回一個包含 `Response` 的 `Future`。

* [`Future`][] is a core Dart class for working with
  async operations. A Future object represents a potential
  value or error that will be available at some time in the future.
    
  [`Future`][] 是 Dart 用來處理非同步操作的一個核心類，
  它通常代表一個可能的值或者將來或許會用到的錯誤。
    
* The `http.Response` class contains the data received from a successful
  http call.
    
  `http.Response` 類包含成功的 http 請求接收到的資料。


## 3. Convert the response into a custom Dart object

## 3. 將返回的響應轉換成一個自訂的 Dart 物件

While it's easy to make a network request, working with a raw
`Future<http.Response>` isn't very convenient.
To make your life easier,
convert the `http.Response` into a Dart object.

雖然進行網路請求很容易，但是處理 `Future<http.Response>` 卻並不簡單，
為了後續處理起來更加方便，我們需要將 `http.Response` 轉換成一個 Dart 物件。

### Create an `Album` class

### 建立一個 `Album` 類

First, create an `Album` class that contains the data from the
network request. It includes a factory constructor that
creates an `Album` from JSON.

首先，建立一個包含網路請求返回資料的 `Album` 類，
而且這個類還需要一個可以利用 json 建立 `Album` 的工廠構造器。

Converting JSON by hand is only one option.
For more information, see the full article on
[JSON and serialization][].

手動轉換 JSON 是我們目前唯一的選項。想了解更多，
請檢視完整的文件 [JSON 和序列化資料][JSON and serialization]。

<?code-excerpt "lib/main.dart (Album)"?>
```dart
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
      userId: json['userId'],
      id: json['id'],
      title: json['title'],
    );
  }
}
```

### Convert the `http.Response` to an `Album`

### 將 `http.Response` 轉換成 `Album`

Now, use the following steps to update the `fetchAlbum()`
function to return a `Future<Album>`:

現在，我們需要更新 `fetchPost()` 函式並返回 `Future<Album>`，
為了實現這個目標，我們需要做以下幾步：

  1. Convert the response body into a JSON `Map` with
     the `dart:convert` package.

     用 `dart:convert` 包將回應內文轉換成一個 json `Map`。

  2. If the server does return an OK response with a status code of
     200, then convert the JSON `Map` into an `Album`
     using the `fromJson()` factory method.

     如果伺服器返回了一個狀態碼為 200 的 "OK" 響應，
     那麼就使用 `fromJson` 工廠方法將 json `Map` 轉換成 `Album`。

  3. If the server does not return an OK response with a status code of 200,
     then throw an exception.
     (Even in the case of a "404 Not Found" server response,
     throw an exception. Do not return `null`.
     This is important when examining
     the data in `snapshot`, as shown below.)

     如果伺服器返回的不是我們預期的響應（返回一個OK，Http Header 是 200），
     那麼就丟擲例外。伺服器如若返回 404 Not Found 錯誤，也同樣要丟擲例外，
     而不是返回一個 `null`，
     在檢查如下所示的 `snapshot` 值的時候，這一點相當重要。

<?code-excerpt "lib/main.dart (fetchAlbum)"?>
```dart
Future<Album> fetchAlbum() async {
  final response = await http
      .get(Uri.parse('https://jsonplaceholder.typicode.com/albums/1'));

  if (response.statusCode == 200) {
    // If the server did return a 200 OK response,
    // then parse the JSON.
    return Album.fromJson(jsonDecode(response.body));
  } else {
    // If the server did not return a 200 OK response,
    // then throw an exception.
    throw Exception('Failed to load album');
  }
}
```

Hooray!
Now you've got a function that fetches an album from the internet.

太棒了！現在你就擁有了一個可以獲取網路資料的完整函式啦。

## 4. Fetch the data

## 4. 獲取資料

Call the `fetchAlbum()` method in either the
[`initState()`][] or [`didChangeDependencies()`][]
methods.

在 [`initState()`][] 或 [`didChangeDependencies()`][]
方法中呼叫獲取資料的方法 `fetch()`。

The `initState()` method is called exactly once and then never again.
If you want to have the option of reloading the API in response to an
[`InheritedWidget`][] changing, put the call into the
`didChangeDependencies()` method.
See [`State`][] for more details.

`initState()` 方法僅會被呼叫一次。
如果你想要響應 [`InheritedWidget`][] 改變以重新載入 API 的話，
請在 `didChangeDependencies()` 方法中進行呼叫，
你可以在 [`State`][] 文件裡瞭解更多。

<?code-excerpt "lib/main.dart (State)"?>
```dart
class _MyAppState extends State<MyApp> {
  late Future<Album> futureAlbum;

  @override
  void initState() {
    super.initState();
    futureAlbum = fetchAlbum();
  }
  // ···
}
```

This Future is used in the next step.

我們將會在下一步中使用這個 Future。

## 5. Display the data

## 5. 顯示資料

To display the data on screen, use the
[`FutureBuilder`][] widget.
The `FutureBuilder` widget comes with Flutter and
makes it easy to work with asynchronous data sources.

為了能夠獲取資料並在螢幕上展示它，你可以使用 [`FutureBuilder`][] widget。
這個由 Flutter 提供的 `FutureBuilder` 元件可以讓處理非同步資料源變的非常簡單。

You must provide two parameters:

此時，你必須要提供兩個引數：

  1. The `Future` you want to work with.
     In this case, the future returned from
     the `fetchAlbum()` function.

     你想要處理的 `Future`，在這個例子中就是 `fetchAlbum()` 返回的 future。

  2. A `builder` function that tells Flutter
     what to render, depending on the
     state of the `Future`: loading, success, or error.

     一個告訴 Flutter 渲染哪些內容的 `builder` 函式，
     同時這也依賴於 `Future` 的狀態：loading、success 或者是 error。
     
Note that `snapshot.hasData` only returns `true`
when the snapshot contains a non-null data value.

需要注意的是：當快照包含非空資料值，
`snapshot.hasData` 將只返回 `true`。

Because `fetchAlbum` can only return non-null values,
the function should throw an exception
even in the case of a "404 Not Found" server response.
Throwing an exception sets the `snapshot.hasError` to `true`
which can be used to display an error message.

因為 `fetchAlbum` 只能返回非空值，在伺服器響應
"404 Not Found" 的時候應該引發例外丟擲。
發生例外的時候會將 `snapshot.hasError` 設定為 `true`，
用來顯示錯誤訊息。

Otherwise, the spinner will be displayed.

其他情況下，spinner 就會正常顯示。

<?code-excerpt "lib/main.dart (FutureBuilder)" replace="/^child: //g;/,$//g"?>
```dart
FutureBuilder<Album>(
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
)
```

## Why is fetchAlbum() called in initState()?

## 為何要在 initState() 中呼叫 fetchPost()？

Although it's convenient,
it's not recommended to put an API call in a `build()` method.

雖然這樣會比較方便，但是我們仍然不推薦將 API 呼叫置於 `build()` 方法內部。

Flutter calls the `build()` method every time it needs
to change anything in the view,
and this happens surprisingly often.
The `fetchAlbum()` method, if placed inside `build()`, is repeatedly 
called on each rebuild causing the app to slow down.

Storing the `fetchAlbum()` result in a state variable ensures that
the `Future` is executed only once and then cached for subsequent
rebuilds.

每當 Flutter 需要改變檢視中的一些內容時（這個發生的頻率非常高），
就會呼叫 `build()` 方法。因此，如果你將資料請求置於 `build()` 內部，
就會造成大量的無效呼叫，同時還會拖慢應用程式的速度。

Here are some better options so it only hits the API when the page is
initially loaded.

關於如何在頁面初始化的時候，只調用 API，下面有一些更好的選擇。

### Pass it into a `StatelessWidget`

### 傳入 `StatelessWidget`

With this strategy, the parent widget is responsible for calling the fetch
method, storing its result, and then passing it to your widget.

使用這種策略的話，相當於父元件負責呼叫資料獲取方法，
儲存結果並傳入你的元件中。

<!-- skip -->
```dart
class MyApp extends StatelessWidget {
  final Future<Post> post;

  MyApp({Key key, this.post}) : super(key: key);
```

You can see a working example of this in the complete example below.

你可以在下面看到一個關於這種策略的完整程式碼範例。

### Call it in the lifecycle of a `StatefulWidget`'s state

### 在 `StatefulWidget` 狀態的生命週期中呼叫

If your widget is stateful, call the fetch method in either the
[`initState()`][] or [`didChangeDependencies()`][] methods.

如果你的元件是有狀態的，你可以在
[`initState()`][] 或者 [`didChangeDependencies()`][] 方法中呼叫 fetch 方法。

The `initState()` method is called exactly once and then never again.
If you want to have the option of reloading the API in response to an
[`InheritedWidget`][] changing, put the call into the 
`didChangeDependencies()` method. See [`State`][] for more details.

`initState()` 只會被呼叫一次而且再也不會被呼叫。
如果你需要在 [`InheritedWidget`][] 改變的時候可以重新載入的話，
可以把資料呼叫放在 `didChangeDependencies()` 方法中。
想了解更多詳細內容請檢視 [`State`][] 文件。

<!-- skip -->
```dart
class _MyAppState extends State<MyApp> {
  Future<Post> post;

  @override
  void initState() {
    super.initState();
    post = fetchPost();
  }
```

## Testing

## 測試

For information on how to test this functionality,
see the following recipes:

關於如何測試這個功能，請檢視下面的說明：

  * [Introduction to unit testing][]
  
    [單元測試介紹][Introduction to unit testing]
    
  * [Mock dependencies using Mockito][]
 
    [使用 Mockito 模擬依賴][Mock dependencies using Mockito]


## Complete example

## 完整範例

<?code-excerpt "lib/main.dart"?>
```dart
import 'dart:async';
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

Future<Album> fetchAlbum() async {
  final response = await http
      .get(Uri.parse('https://jsonplaceholder.typicode.com/albums/1'));

  if (response.statusCode == 200) {
    // If the server did return a 200 OK response,
    // then parse the JSON.
    return Album.fromJson(jsonDecode(response.body));
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

  const Album({
    required this.userId,
    required this.id,
    required this.title,
  });

  factory Album.fromJson(Map<String, dynamic> json) {
    return Album(
      userId: json['userId'],
      id: json['id'],
      title: json['title'],
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
  late Future<Album> futureAlbum;

  @override
  void initState() {
    super.initState();
    futureAlbum = fetchAlbum();
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Fetch Data Example',
      theme: ThemeData(
        primarySwatch: Colors.blue,
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


[`didChangeDependencies()`]: {{site.api}}/flutter/widgets/State/didChangeDependencies.html
[`Future`]: {{site.api}}/flutter/dart-async/Future-class.html
[`FutureBuilder`]: {{site.api}}/flutter/widgets/FutureBuilder-class.html
[JSONPlaceholder]: https://jsonplaceholder.typicode.com/
[`http`]: {{site.pub-pkg}}/http
[`http.get()`]: {{site.pub-api}}/http/latest/http/get.html
[`http` package]: {{site.pub-pkg}}/http/install
[`InheritedWidget`]: {{site.api}}/flutter/widgets/InheritedWidget-class.html
[Introduction to unit testing]: {{site.url}}/cookbook/testing/unit/introduction
[`initState()`]: {{site.api}}/flutter/widgets/State/initState.html
[Mock dependencies using Mockito]: {{site.url}}/cookbook/testing/unit/mocking
[JSON and serialization]: {{site.url}}/development/data-and-backend/json
[`State`]: {{site.api}}/flutter/widgets/State-class.html
