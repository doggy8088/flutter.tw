---
title: Work with WebSockets
title: 發起 WebSockets 請求
description: How to connect to a web socket.
description: 如何建立 web socket 連線。
tags: cookbook, 實用課程, 網路請求
keywords: WebSockets
prev:
  title: Update data over the internet
  title: 透過網路更新資料
  path: /docs/cookbook/networking/update-data
next:
  title: Persist data with SQLite
  title: 用 SQLite 做資料持久化
  path: /docs/cookbook/persistence/sqlite
---

<?code-excerpt path-base="cookbook/networking/web_sockets/"?>

In addition to normal HTTP requests,
you can connect to servers using `WebSockets`.
`WebSockets` allow for two-way communication with a server
without polling.

除了普通的 HTTP 請求，你還可以透過 `WebSockets` 來連線伺服器，
`WebSockets` 可以以非輪詢的方式與伺服器進行雙向通訊。

In this example, connect to a
[test WebSocket server sponsored by Lob.com][].
The server sends back the same message you send to it.

在這裡，你可以連線一個 
[由 Lob.com 贊助的測試伺服器][test WebSocket server sponsored by Lob.com]。
該伺服器只會返回你傳送的資訊。

This recipe uses the following steps:

這個課程裡包含以下步驟：

  1. Connect to a WebSocket server.

     連線 WebSocket 伺服器

  2. Listen for messages from the server.

     監聽來自伺服器的訊息

  3. Send data to the server.

     向伺服器傳送資料
  
  4. Close the WebSocket connection.

     關閉 WebSocket 連線

## 1. Connect to a WebSocket server

## 1. 連線 WebSocket 伺服器

The [`web_socket_channel`][] package provides the
tools you need to connect to a WebSocket server.

[`web_socket_channel`][] 這個 package 提供了
連線 WebSocket 伺服器所需的一些工具。

The package provides a `WebSocketChannel`
that allows you to both listen for messages
from the server and push messages to the server.

該套件提供的 `WebSocketChannel` 不僅可以讓你監聽到來自伺服器的訊息
還可以讓你向伺服器推送訊息。

In Flutter, use the following line to
create a `WebSocketChannel` that connects to a server:

在 Flutter 中，只用一行程式碼就可以建立一個連線到伺服器的 `WebSocketChannel`。

<?code-excerpt "lib/main.dart (connect)" replace="/_channel/channel/g"?>
```dart
final channel = WebSocketChannel.connect(
  Uri.parse('wss://echo.websocket.events'),
);
```

## 2. Listen for messages from the server

## 2. 監聽來自伺服器的訊息

Now that you've established a connection,
listen to messages from the server.

建立了連線之後，你就可以監聽來自伺服器的訊息了。

After sending a message to the test server,
it sends the same message back.

當你向測試伺服器傳送一條訊息之後，它會將同樣的訊息傳送回來。

In this example, use a [`StreamBuilder`][]
widget to listen for new messages, and a
[`Text`][] widget to display them.

在這個例子中，我們用 [`StreamBuilder`][] 元件來監聽新訊息，
並使用 [`Text`][] 元件來展示它們。

<?code-excerpt "lib/main.dart (StreamBuilder)" replace="/_channel/channel/g"?>
```dart
StreamBuilder(
  stream: channel.stream,
  builder: (context, snapshot) {
    return Text(snapshot.hasData ? '${snapshot.data}' : '');
  },
)
```

### How this works

### 這樣為什麼可行？

The `WebSocketChannel` provides a
[`Stream`][] of messages from the server.

`WebSocketChannel` 提供了一個來自伺服器的 [`Stream`][] 類訊息。

The `Stream` class is a fundamental part of the `dart:async` package.
It provides a way to listen to async events from a data source.
Unlike `Future`, which returns a single async response,
the `Stream` class can deliver many events over time.

這個 `Stream` 類是 `dart:async` 套件的基本組成部分，
它提供了一個從資料源監聽非同步事件的方法。
和 `Future` 不一樣的是，`Future` 只能返回一個單獨的非同步響應，
而 `Stream` 類可以隨著時間的推移傳遞很多事件。

The [`StreamBuilder`][] widget connects to a `Stream`
and asks Flutter to rebuild every time it
receives an event using the given `builder()` function.

[`StreamBuilder`][] widget 會和 `Stream` 建立起連線，
並且每當它接收到一個使用給定 `builder()` 函式的事件時，
就會通知 Flutter 去 rebuild。

## 3. Send data to the server

## 3. 向伺服器傳送資料

To send data to the server,
`add()` messages to the `sink` provided
by the `WebSocketChannel`.

要向伺服器傳送資料，
可以使用 `WebSocketChannel` 提供的 `sink` 下的 `add()` 方法來發送資訊。

<?code-excerpt "lib/main.dart (add)" replace="/_channel/channel/g;/_controller.text/'Hello!'/g"?>
```dart
channel.sink.add('Hello!');
```

### How this works

### 這又是如何工作的呢

The `WebSocketChannel` provides a
[`StreamSink`][] to push messages to the server.

`WebSocketChannel` 提供了一個 [`StreamSink`][] 來向伺服器推送訊息。

The `StreamSink` class provides a general way to add sync or async
events to a data source.

這個 `StreamSink` 類提供了一個可以向資料源新增同步或者非同步事件的通用方法。

## 4. Close the WebSocket connection

## 4. 關閉 WebSocket 連線

After you're done using the WebSocket, close the connection:
To do so, close the `sink`.

當你使用完 WebSocket 之後，記得關閉這個連線。
要關閉這個 WebSocket 連線，只需要關閉 `sink`。

<?code-excerpt "lib/main.dart (close)" replace="/_channel/channel/g"?>
```dart
channel.sink.close();
```

## Complete example

## 完整範例

<?code-excerpt "lib/main.dart"?>
```dart
import 'package:flutter/material.dart';
import 'package:web_socket_channel/web_socket_channel.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    const title = 'WebSocket Demo';
    return const MaterialApp(
      title: title,
      home: MyHomePage(
        title: title,
      ),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({
    super.key,
    required this.title,
  });

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  final TextEditingController _controller = TextEditingController();
  final _channel = WebSocketChannel.connect(
    Uri.parse('wss://echo.websocket.events'),
  );

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Padding(
        padding: const EdgeInsets.all(20.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Form(
              child: TextFormField(
                controller: _controller,
                decoration: const InputDecoration(labelText: 'Send a message'),
              ),
            ),
            const SizedBox(height: 24),
            StreamBuilder(
              stream: _channel.stream,
              builder: (context, snapshot) {
                return Text(snapshot.hasData ? '${snapshot.data}' : '');
              },
            )
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _sendMessage,
        tooltip: 'Send message',
        child: const Icon(Icons.send),
      ), // This trailing comma makes auto-formatting nicer for build methods.
    );
  }

  void _sendMessage() {
    if (_controller.text.isNotEmpty) {
      _channel.sink.add(_controller.text);
    }
  }

  @override
  void dispose() {
    _channel.sink.close();
    _controller.dispose();
    super.dispose();
  }
}
```
![Web sockets demo]({{site.url}}/assets/images/docs/cookbook/web-sockets.gif){:.site-mobile-screenshot}


[`Stream`]: {{site.api}}/flutter/dart-async/Stream-class.html
[`StreamBuilder`]: {{site.api}}/flutter/widgets/StreamBuilder-class.html
[`StreamSink`]: {{site.api}}/flutter/dart-async/StreamSink-class.html
[test WebSocket server sponsored by Lob.com]: https://www.lob.com/blog/websocket-org-is-down-here-is-an-alternative
[`Text`]: {{site.api}}/flutter/widgets/Text-class.html
[`web_socket_channel`]: {{site.pub-pkg}}/web_socket_channel
