---
title: Take a picture using the camera
title: 使用 Camera 外掛實現拍照功能
description: How to use a camera plugin on mobile.
description: 如何在移動裝置上使用 camera 外掛。
tags: cookbook, 實用課程, 原生外掛
keywords: Flutter使用相機,錄影,相機預覽
prev:
  title: Play and pause a video
  title: 影片的播放和暫停
  path: /docs/cookbook/plugins/play-video
next:
  title: An introduction to integration testing
  title: 整合測試介紹
  path: /docs/cookbook/testing/integration/introduction
---

<?code-excerpt path-base="cookbook/plugins/picture_using_camera/"?>

Many apps require working with the device's cameras to
take photos and videos.  Flutter provides the [`camera`][] plugin
for this purpose. The `camera` plugin provides tools to get a list of the
available cameras, display a preview coming from a specific camera,
and take photos or videos.

很多應用都需要使用到裝置的相機模組拍攝圖片和影片。
因此，Flutter 提供了 [`camera`][] 外掛。
`camera` 外掛提供了一系列可用的相機，
並使用特定的相機展示相機預覽、拍照、錄影片。

This recipe demonstrates how to use the `camera` plugin to display a preview, 
take a photo, and display it using the following steps:

這個章節將會講解如何使用 `camera` 外掛去
展示相機預覽、拍照並顯示。

## Directions

## 步驟

  1. Add the required dependencies.

     新增所需依賴

  2. Get a list of the available cameras.

     獲取可用相機列表

  3. Create and initialize the `CameraController`.

     建立並初始化 `CameraController`

  4. Use a `CameraPreview` to display the camera's feed.

     使用 `CameraPreview` 展示相機的幀流

  5. Take a picture with the `CameraController`.

     使用 `CameraController` 拍攝一張圖片

  6. Display the picture with an `Image` widget.

     使用 `Image` 元件展示圖片

## 1. Add the required dependencies

## 1. 新增所需依賴

To complete this recipe, you need to add three dependencies to your app:

為了完成這個章節，你需要向你的應用新增三個依賴：

[`camera`][]
<br> Provides tools to work with the cameras on the device.

[`camera`][]
<br> 提供使用裝置相機模組的工具

[`path_provider`][]
<br> Finds the correct paths to store images.

[`path_provider`][]
<br>尋找儲存圖片的正確路徑

[`path`][]
<br> Creates paths that work on any platform.

[`path`][]
<br> 建立適配任何平台的路徑

```yaml
dependencies:
  flutter:
    sdk: flutter
  camera:
  path_provider:
  path:
```
{{site.alert.tip}}

  - For android, You must have to update `minSdkVersion` to 21 (or higher).

    針對 Android 來說，工程的 `minSdkVersion` 需要設定為 21 或者更高。

  - On iOS, lines below have to be added inside `ios/Runner/Info.plist` in order the access the camera and microphone.

    在 iOS 上，在 `ios/Runner/Info.plist` 中新增下面幾行以存取攝影頭和麥克風。

    ```
    <key>NSCameraUsageDescription</key>
    <string>Explanation on why the camera access is needed.</string>
    <key>NSMicrophoneUsageDescription</key>
    <string>Explanation on why the microphone access is needed.</string>
    ```

{{site.alert.end}}

## 2. Get a list of the available cameras

## 2. 獲取可用相機列表

Next, get a list of available cameras using the `camera` plugin.

接著，你可以使用 `camera` 外掛獲取可用相機列表。

<?code-excerpt "lib/main.dart (init)"?>
```dart
// Ensure that plugin services are initialized so that `availableCameras()`
// can be called before `runApp()`
WidgetsFlutterBinding.ensureInitialized();

// Obtain a list of the available cameras on the device.
final cameras = await availableCameras();

// Get a specific camera from the list of available cameras.
final firstCamera = cameras.first;
```

## 3. Create and initialize the `CameraController`

## 3. 建立並初始化 `CameraController`

Once you have a camera, use the following steps to
create and initialize a `CameraController`.
This process establishes a connection to
the device's camera that allows you to control the camera
and display a preview of the camera's feed.

在選擇了一個相機後，你需要建立並初始化 `CameraController`。
在這個過程中，與裝置相機建立了連線並允許你控制相機並展示相機的預覽幀流。

To achieve this, please:

實現這個過程，請依照以下步驟：

  1. Create a `StatefulWidget` with a companion `State` class.

     建立一個帶有 `State` 類別的 `StatefulWidget` 元件

  2. Add a variable to the `State` class to store the `CameraController`.

     新增一個變數到 `State` 類來存放 `CameraController`

  3. Add a variable to the `State` class to store the `Future`
     returned from `CameraController.initialize()`.

     新增另外一個變數到 `State` 類中來存放
     `CameraController.initialize()` 返回的 `Future`

  4. Create and initialize the controller in the `initState()` method.

     在 `initState()` 方法中建立並初始化控制器

  5. Dispose of the controller in the `dispose()` method.

     在 `dispose()` 方法中銷燬控制器

<?code-excerpt "lib/main_step3.dart (controller)"?>
```dart
// A screen that allows users to take a picture using a given camera.
class TakePictureScreen extends StatefulWidget {
  const TakePictureScreen({
    super.key,
    required this.camera,
  });

  final CameraDescription camera;

  @override
  TakePictureScreenState createState() => TakePictureScreenState();
}

class TakePictureScreenState extends State<TakePictureScreen> {
  late CameraController _controller;
  late Future<void> _initializeControllerFuture;

  @override
  void initState() {
    super.initState();
    // To display the current output from the Camera,
    // create a CameraController.
    _controller = CameraController(
      // Get a specific camera from the list of available cameras.
      widget.camera,
      // Define the resolution to use.
      ResolutionPreset.medium,
    );

    // Next, initialize the controller. This returns a Future.
    _initializeControllerFuture = _controller.initialize();
  }

  @override
  void dispose() {
    // Dispose of the controller when the widget is disposed.
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // Fill this out in the next steps.
    return Container();
  }
}
```

{{site.alert.warning}}

  If you do not initialize the `CameraController`,
  you *cannot* use the camera to display a preview and take pictures.

  如果你沒有初始化 `CameraController`，你就 *不能* 使用相機預覽和拍照。

{{site.alert.end}}

## 4. Use a `CameraPreview` to display the camera's feed

## 4. 在 `initState` 方法中建立並初始化控制器

Next, use the `CameraPreview` widget from the `camera` package to
display a preview of the camera's feed.

接著，你能夠使用 `camera` 中的 `CameraPreview` 元件來展示相機預覽幀流。

{{site.alert.secondary}}

  **Remember** You must wait until the controller has finished
  initializing before working with the camera. Therefore,
  you must wait for the `_initializeControllerFuture()` created
  in the previous step to complete before showing a `CameraPreview`.
  
  **請記住**： 在使用相機前，請確保控制器已經完成初始化。
  因此，你一定要等待前一個步驟建立 `_initializeControllerFuture()`
  執行完畢才去展示 `CameraPreview`。
  
{{site.alert.end}}

Use a [`FutureBuilder`][] for exactly this purpose.

你可以使用 [`FutureBuilder`][] 完成這個任務。

<?code-excerpt "lib/main.dart (FutureBuilder)" replace="/body: //g;/,$//g"?>
```dart
// You must wait until the controller is initialized before displaying the
// camera preview. Use a FutureBuilder to display a loading spinner until the
// controller has finished initializing.
FutureBuilder<void>(
  future: _initializeControllerFuture,
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.done) {
      // If the Future is complete, display the preview.
      return CameraPreview(_controller);
    } else {
      // Otherwise, display a loading indicator.
      return const Center(child: CircularProgressIndicator());
    }
  },
)
```

## 5. Take a picture with the `CameraController`

## 5. 使用 `CameraController` 拍照

You can use the `CameraController` to take pictures using the
[`takePicture()`][] method, which returns an [`XFile`][],
a cross-platform, simplified `File` abstraction.
On both Android and IOS, the new image is stored in their
respective cache directories,
and the `path` to that location is returned in the `XFile`.

In this example, create a `FloatingActionButton` that takes a picture
using the `CameraController` when a user taps on the button.

Taking a picture requires 2 steps:

  1. Ensure that the camera is initialized.
  2. Use the controller to take a picture and ensure that it returns a `Future<XFile>`.
  
It is good practice to wrap these operations in a `try / catch` block in order
to handle any errors that might occur.

最好把這些操作都放在 `try / catch` 方法塊中來處理可能發生的例外。

<?code-excerpt "lib/main_step5.dart (FAB)" replace="/^floatingActionButton: //g;/,$//g"?>
```dart
FloatingActionButton(
  // Provide an onPressed callback.
  onPressed: () async {
    // Take the Picture in a try / catch block. If anything goes wrong,
    // catch the error.
    try {
      // Ensure that the camera is initialized.
      await _initializeControllerFuture;

      // Attempt to take a picture and then get the location
      // where the image file is saved.
      final image = await _controller.takePicture();
    } catch (e) {
      // If an error occurs, log the error to the console.
      print(e);
    }
  },
  child: const Icon(Icons.camera_alt),
)
```
## 6. Display the picture with an `Image` widget

## 6. 在 `dispose` 方法中銷燬控制器

If you take the picture successfully, you can then display the saved picture
using an `Image` widget. In this case, the picture is stored as a file on
the device.

如果你能成功拍攝圖片，你就可以使用 `Image` 元件展示被儲存的圖片。
在這個範例中，這張圖片是以檔案的形式儲存在裝置中。

Therefore, you must provide a `File` to the `Image.file` constructor.
You can create an instance of the `File` class by passing the path created in
the previous step.

因此，你需要提供一個 `File` 給 `Image.file` 建構函式。
你能夠透過傳遞你在上一步中建立的路徑來建立一個 `File` 類別的例項。

<?code-excerpt "lib/image_file.dart (ImageFile)" replace="/^return\ //g"?>
```dart
Image.file(File('path/to/my/picture.png'));
```

## Complete example

## 完整範例

<?code-excerpt "lib/main.dart"?>
```dart
import 'dart:async';
import 'dart:io';

import 'package:camera/camera.dart';
import 'package:flutter/material.dart';

Future<void> main() async {
  // Ensure that plugin services are initialized so that `availableCameras()`
  // can be called before `runApp()`
  WidgetsFlutterBinding.ensureInitialized();

  // Obtain a list of the available cameras on the device.
  final cameras = await availableCameras();

  // Get a specific camera from the list of available cameras.
  final firstCamera = cameras.first;

  runApp(
    MaterialApp(
      theme: ThemeData.dark(),
      home: TakePictureScreen(
        // Pass the appropriate camera to the TakePictureScreen widget.
        camera: firstCamera,
      ),
    ),
  );
}

// A screen that allows users to take a picture using a given camera.
class TakePictureScreen extends StatefulWidget {
  const TakePictureScreen({
    super.key,
    required this.camera,
  });

  final CameraDescription camera;

  @override
  TakePictureScreenState createState() => TakePictureScreenState();
}

class TakePictureScreenState extends State<TakePictureScreen> {
  late CameraController _controller;
  late Future<void> _initializeControllerFuture;

  @override
  void initState() {
    super.initState();
    // To display the current output from the Camera,
    // create a CameraController.
    _controller = CameraController(
      // Get a specific camera from the list of available cameras.
      widget.camera,
      // Define the resolution to use.
      ResolutionPreset.medium,
    );

    // Next, initialize the controller. This returns a Future.
    _initializeControllerFuture = _controller.initialize();
  }

  @override
  void dispose() {
    // Dispose of the controller when the widget is disposed.
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Take a picture')),
      // You must wait until the controller is initialized before displaying the
      // camera preview. Use a FutureBuilder to display a loading spinner until the
      // controller has finished initializing.
      body: FutureBuilder<void>(
        future: _initializeControllerFuture,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.done) {
            // If the Future is complete, display the preview.
            return CameraPreview(_controller);
          } else {
            // Otherwise, display a loading indicator.
            return const Center(child: CircularProgressIndicator());
          }
        },
      ),
      floatingActionButton: FloatingActionButton(
        // Provide an onPressed callback.
        onPressed: () async {
          // Take the Picture in a try / catch block. If anything goes wrong,
          // catch the error.
          try {
            // Ensure that the camera is initialized.
            await _initializeControllerFuture;

            // Attempt to take a picture and get the file `image`
            // where it was saved.
            final image = await _controller.takePicture();

            if (!mounted) return;

            // If the picture was taken, display it on a new screen.
            await Navigator.of(context).push(
              MaterialPageRoute(
                builder: (context) => DisplayPictureScreen(
                  // Pass the automatically generated path to
                  // the DisplayPictureScreen widget.
                  imagePath: image.path,
                ),
              ),
            );
          } catch (e) {
            // If an error occurs, log the error to the console.
            print(e);
          }
        },
        child: const Icon(Icons.camera_alt),
      ),
    );
  }
}

// A widget that displays the picture taken by the user.
class DisplayPictureScreen extends StatelessWidget {
  final String imagePath;

  const DisplayPictureScreen({super.key, required this.imagePath});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Display the Picture')),
      // The image is stored as a file on the device. Use the `Image.file`
      // constructor with the given path to display the image.
      body: Image.file(File(imagePath)),
    );
  }
}
```


[`camera`]: {{site.pub-pkg}}/camera
[`FutureBuilder`]: {{site.api}}/flutter/widgets/FutureBuilder-class.html
[`path`]: {{site.pub-pkg}}/path
[`path_provider`]: {{site.pub-pkg}}/path_provider
[`takePicture()`]: {{site.pub}}/documentation/camera/latest/camera/CameraController/takePicture.html
[`XFile`]: {{site.pub}}/documentation/cross_file/latest/cross_file/XFile-class.html
