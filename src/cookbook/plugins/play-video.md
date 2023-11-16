---
title: Play and pause a video
title: 影片的播放和暫停
description: How to use the video_player plugin.
description: 如何使用 video_player 外掛。
tags: cookbook, 實用課程, 原生外掛
keywords: Flutter播放影片
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/plugins/play_video/"?>

Playing videos is a common task in app development,
and Flutter apps are no exception. To play videos,
the Flutter team provides the [`video_player`][] plugin.
You can use the `video_player` plugin to play videos
stored on the file system, as an asset, or from the internet.

在任何應用開發中，影片播放都是一項常見任務，Flutter 應用也不例外。
為了支援影片播放，Flutter 團隊提供了 [`video_player`][] 外掛。
你可以使用 `video_player` 外掛播放儲存在本地檔案系統中的影片或者網路影片。

{{site.alert.warning}}

  At this time,
  the `video_player` plugin doesn't work with any desktop platform.
  To learn more, check out the [`video_player`][] package.

  目前 `video_player` 外掛不支援桌面端。
  你可以檢視 [`video_player`][] package 瞭解更多。

{{site.alert.end}}

On iOS, the `video_player` plugin makes use of
[`AVPlayer`][] to handle playback. On Android,
it uses [`ExoPlayer`][].

在 iOS 上，`video_player` 使用 [`AVPlayer`][] 進行播放控制。
在 Android 上，使用的是 [`ExoPlayer`][]。

This recipe demonstrates how to use the `video_player` package to stream a
video from the internet with basic play and pause controls using
the following steps:

這個章節講解的是如何藉助 `video_player` 包接收網路影片流，
並加入基本的播放、暫停操作。

## Directions

## 步驟

  1. Add the `video_player` dependency.
     
     新增 `video_player` 依賴

  2. Add permissions to your app.

     新增許可權

  3. Create and initialize a `VideoPlayerController`.

     建立並初始化 `VideoPlayerController`

  4. Display the video player.

     展示影片播放器

  5. Play and pause the video.

     播放影片和暫停影片

## 1. Add the `video_player` dependency

## 1. 新增 `video_player` 依賴

This recipe depends on one Flutter plugin: `video_player`. 
First, add this dependency to your project.

這個章節基於一個 Flutter 外掛： `video_player`。
首先，新增依賴到 `pubspec.yaml` 中。

To add the `video_player` package as a dependency, run `flutter pub add`:

執行 `flutter pub add` 將 `video_player` 新增為依賴：

```terminal
$ flutter pub add video_player
```

## 2. Add permissions to your app

## 2. 新增許可權

Next, update your `android` and `ios` configurations to ensure
that your app has the correct permissions to stream videos
from the internet.

然後，你需要確保你的應用擁有從網路中獲取影片流的許可權。
因此，你需要更新你的 `android` 和 `ios` 配置。

### Android

### Android 配置

Add the following permission to the `AndroidManifest.xml` file just after the
`<application>` definition. The `AndroidManifest.xml` file is found at
`<project root>/android/app/src/main/AndroidManifest.xml`.

在 `AndroidManifest.xml` 檔案中的 `<application>` 配置項下加入如下許可權。
`AndroidManifest.xml` 檔案的路徑是
`<project root>/android/app/src/main/AndroidManifest.xml`

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    <application ...>

    </application>

    <uses-permission android:name="android.permission.INTERNET"/>
</manifest>
```

### iOS

### iOS 配置

For iOS, add the following to the `Info.plist` file found at
`<project root>/ios/Runner/Info.plist`.

針對 iOS，你需要在 `<project root>/ios/Runner/Info.plist`
路徑下的 `Info.plist` 檔案中加入如下配置。

```xml
<key>NSAppTransportSecurity</key>
<dict>
  <key>NSAllowsArbitraryLoads</key>
  <true/>
</dict>
```

{{site.alert.warning}}

  The `video_player` plugin can only play asset videos in iOS simulators.
  You must test network-hosted videos on physical iOS devices.

  `video_player` 外掛在 iOS 模擬器上不能使用，必須要在 iOS 真機上進行測試。

{{site.alert.end}}

## 3. Create and initialize a `VideoPlayerController`

## 3. 建立並初始化 `VideoPlayerController`

Now that you have the `video_player` plugin installed with the correct
permissions, create a `VideoPlayerController`. The
`VideoPlayerController` class allows you to connect to different types of
videos and control playback.

`video_player` 外掛成功安裝且許可權設定完成後，
需要建立一個 `VideoPlayerController`。
`VideoPlayerController` 類允許你播放不同型別的影片並進行播放控制。

Before you can play videos, you must also `initialize` the controller.
This establishes the connection to the video and prepare the
controller for playback.

在播放影片前，需要對播放控制器進行初始化。
初始化過程主要是與影片源建立連線和播放控制的準備。

To create and initialize the `VideoPlayerController` do the following:

建立和初始化 `VideoPlayerController` 時，請遵循以下步驟：

  1. Create a `StatefulWidget` with a companion `State` class

     建立一個 `StatefulWidget` 元件和 `State` 類

  2. Add a variable to the `State` class to store the `VideoPlayerController`

     在 `State` 類中增加一個變數來存放 `VideoPlayerController`

  3. Add a variable to the `State` class to store the `Future` returned from
  `VideoPlayerController.initialize`

     在 `State` 類中增加另外一個變數來存放 `VideoPlayerController.initialize` 返回的 `Future`

  4. Create and initialize the controller in the `initState` method

     在 `initState` 方法裡建立和初始化控制器

  5. Dispose of the controller in the `dispose` method

     在 `dispose` 方法裡銷燬控制器

<?code-excerpt "lib/main_step3.dart (VideoPlayerScreen)"?>
```dart
class VideoPlayerScreen extends StatefulWidget {
  const VideoPlayerScreen({super.key});

  @override
  State<VideoPlayerScreen> createState() => _VideoPlayerScreenState();
}

class _VideoPlayerScreenState extends State<VideoPlayerScreen> {
  late VideoPlayerController _controller;
  late Future<void> _initializeVideoPlayerFuture;

  @override
  void initState() {
    super.initState();

    // Create and store the VideoPlayerController. The VideoPlayerController
    // offers several different constructors to play videos from assets, files,
    // or the internet.
    _controller = VideoPlayerController.networkUrl(
      Uri.parse(
        'https://flutter.github.io/assets-for-api-docs/assets/videos/butterfly.mp4',
      ),
    );

    _initializeVideoPlayerFuture = _controller.initialize();
  }

  @override
  void dispose() {
    // Ensure disposing of the VideoPlayerController to free up resources.
    _controller.dispose();

    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // Complete the code in the next step.
    return Container();
  }
}
```

## 4. Display the video player

## 4. 展示影片播放器

Now, display the video. The `video_player` plugin provides the
[`VideoPlayer`][] widget to display the video initialized by
the `VideoPlayerController`.
By default, the `VideoPlayer` widget takes up as much space as possible.
This often isn't ideal for videos because they are meant
to be displayed in a specific aspect ratio, such as 16x9 or 4x3.

現在到了展示播放器的時候。`video_player` 外掛提供了 [`VideoPlayer`][] 元件
來展示已經被 `VideoPlayerController` 初始化完成的影片。
預設情況下，`VideoPlayer` 元件會盡可能撐滿整個空間。
但是這通常不會太理想，因為很多時候影片需要在特定的寬高比下展示，
比如 16x9 或者 4x3。

Therefore, wrap the `VideoPlayer` widget in an [`AspectRatio`][]
widget to ensure that the video has the correct proportions.

因此，你可以把 `VideoPlayer` 元件嵌進一個
[`AspectRatio`][] 元件中，保證影片播放保持正確的比例。

Furthermore, you must display the `VideoPlayer` widget after the
`_initializeVideoPlayerFuture()` completes. Use `FutureBuilder` to
display a loading spinner until the controller finishes initializing.
Note: initializing the controller does not begin playback.

此外，你必須在 `_initializeVideoPlayerFuture` 完成後才展示 `VideoPlayer` 元件。
你可以使用 `FutureBuilder` 來展示一個旋轉的載入圖示直到初始化完成。
請注意：控制器初始化完成並不會立即開始播放。

<?code-excerpt "lib/main.dart (FutureBuilder)" replace="/body: //g;/,$//g"?>
```dart
// Use a FutureBuilder to display a loading spinner while waiting for the
// VideoPlayerController to finish initializing.
FutureBuilder(
  future: _initializeVideoPlayerFuture,
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.done) {
      // If the VideoPlayerController has finished initialization, use
      // the data it provides to limit the aspect ratio of the video.
      return AspectRatio(
        aspectRatio: _controller.value.aspectRatio,
        // Use the VideoPlayer widget to display the video.
        child: VideoPlayer(_controller),
      );
    } else {
      // If the VideoPlayerController is still initializing, show a
      // loading spinner.
      return const Center(
        child: CircularProgressIndicator(),
      );
    }
  },
)
```

## 5. Play and pause the video

## 5. 播放影片和暫停影片

By default, the video starts in a paused state. To begin playback,
call the [`play()`][] method provided by the `VideoPlayerController`.
To pause playback, call the [`pause()`][] method.

預設情況下，播放器啟動時會處於暫停狀態。開始播放，
需要呼叫 `VideoPlayerController` 提供的[`play()`][] 方法。
停止播放，需要呼叫 [`pause()`][] 方法。

For this example,
add a `FloatingActionButton` to your app that displays a play
or pause icon depending on the situation.
When the user taps the button,
play the video if it's currently paused,
or pause the video if it's playing.

在這個範例中，嚮應用加入了一個 `FloatingActionButton`，
這個按鈕會根據播放狀態展示播放或者暫停的圖示。
當用戶點選按鈕，會切換播放狀態。
如果當前是暫停狀態，就開始播放。如果當前是播放狀態，就暫停播放。

<?code-excerpt "lib/main.dart (FAB)" replace="/^floatingActionButton: //g;/,$//g"?>
```dart
FloatingActionButton(
  onPressed: () {
    // Wrap the play or pause in a call to `setState`. This ensures the
    // correct icon is shown.
    setState(() {
      // If the video is playing, pause it.
      if (_controller.value.isPlaying) {
        _controller.pause();
      } else {
        // If the video is paused, play it.
        _controller.play();
      }
    });
  },
  // Display the correct icon depending on the state of the player.
  child: Icon(
    _controller.value.isPlaying ? Icons.pause : Icons.play_arrow,
  ),
)
```

## Complete example

## 完整範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'dart:async';

import 'package:flutter/material.dart';
import 'package:video_player/video_player.dart';

void main() => runApp(const VideoPlayerApp());

class VideoPlayerApp extends StatelessWidget {
  const VideoPlayerApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Video Player Demo',
      home: VideoPlayerScreen(),
    );
  }
}

class VideoPlayerScreen extends StatefulWidget {
  const VideoPlayerScreen({super.key});

  @override
  State<VideoPlayerScreen> createState() => _VideoPlayerScreenState();
}

class _VideoPlayerScreenState extends State<VideoPlayerScreen> {
  late VideoPlayerController _controller;
  late Future<void> _initializeVideoPlayerFuture;

  @override
  void initState() {
    super.initState();

    // Create and store the VideoPlayerController. The VideoPlayerController
    // offers several different constructors to play videos from assets, files,
    // or the internet.
    _controller = VideoPlayerController.networkUrl(
      Uri.parse(
        'https://flutter.github.io/assets-for-api-docs/assets/videos/butterfly.mp4',
      ),
    );

    // Initialize the controller and store the Future for later use.
    _initializeVideoPlayerFuture = _controller.initialize();

    // Use the controller to loop the video.
    _controller.setLooping(true);
  }

  @override
  void dispose() {
    // Ensure disposing of the VideoPlayerController to free up resources.
    _controller.dispose();

    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Butterfly Video'),
      ),
      // Use a FutureBuilder to display a loading spinner while waiting for the
      // VideoPlayerController to finish initializing.
      body: FutureBuilder(
        future: _initializeVideoPlayerFuture,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.done) {
            // If the VideoPlayerController has finished initialization, use
            // the data it provides to limit the aspect ratio of the video.
            return AspectRatio(
              aspectRatio: _controller.value.aspectRatio,
              // Use the VideoPlayer widget to display the video.
              child: VideoPlayer(_controller),
            );
          } else {
            // If the VideoPlayerController is still initializing, show a
            // loading spinner.
            return const Center(
              child: CircularProgressIndicator(),
            );
          }
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          // Wrap the play or pause in a call to `setState`. This ensures the
          // correct icon is shown.
          setState(() {
            // If the video is playing, pause it.
            if (_controller.value.isPlaying) {
              _controller.pause();
            } else {
              // If the video is paused, play it.
              _controller.play();
            }
          });
        },
        // Display the correct icon depending on the state of the player.
        child: Icon(
          _controller.value.isPlaying ? Icons.pause : Icons.play_arrow,
        ),
      ),
    );
  }
}
```


[`AspectRatio`]: {{site.api}}/flutter/widgets/AspectRatio-class.html
[`AVPlayer`]: {{site.apple-dev}}/documentation/avfoundation/avplayer
[`ExoPlayer`]: https://google.github.io/ExoPlayer/
[`pause()`]: {{site.pub-api}}/video_player/latest/video_player/VideoPlayerController/pause.html
[`play()`]: {{site.pub-api}}/video_player/latest/video_player/VideoPlayerController/play.html
[`video_player`]: {{site.pub-pkg}}/video_player
[`VideoPlayer`]: {{site.pub-api}}/video_player/latest/video_player/VideoPlayer-class.html
