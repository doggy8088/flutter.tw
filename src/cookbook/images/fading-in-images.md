---
title: Fade in images with a placeholder
title: 佔位符和網路圖片淡入
description: How to fade images into view.
description: 如何淡入佔位符和網路圖片。
tags: cookbook, 實用課程, 使用圖片
keywords: 互動,淡入淡出,佔位符
---

<?code-excerpt path-base="cookbook/images/fading_in_images"?>

When displaying images using the default `Image` widget,
you might notice they simply pop onto the screen as they're loaded.
This might feel visually jarring to your users.

當使用預設的 `Image` widget 顯示圖片時，
你可能會注意到圖片只是在載入完後直接顯示到螢幕上，
使用者可能會覺得這看起來不舒服。

Instead, wouldn't it be nice to display a placeholder at first,
and images would fade in as they're loaded? Use the
[`FadeInImage`][] widget for exactly this purpose.

此外，如果可以先展示佔位符，待圖片載入完成後淡入顯示圖片不是很酷麼？
可以使用 Flutter 自帶的 [`FadeInImage`][] widget 來實現這個功能。

`FadeInImage` works with images of any type: in-memory, local assets,
or images from the internet.

`FadeInImage` 適用於任何型別的圖片：
記憶體中的，本地儲存的，抑或是網路上的。

## In-Memory

## 從記憶體載入佔位符

In this example, use the [`transparent_image`][]
package for a simple transparent placeholder.

本例將使用 [`transparent_image`][] 包來實現一個簡單的透明佔位符。

<?code-excerpt "lib/memory_main.dart (MemoryNetwork)" replace="/^child\: //g"?>
```dart
FadeInImage.memoryNetwork(
  placeholder: kTransparentImage,
  image: 'https://picsum.photos/250?image=9',
),
```

### Complete example

### 完整範例

<?code-excerpt "lib/memory_main.dart"?>
```dart
import 'package:flutter/material.dart';
import 'package:transparent_image/transparent_image.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    const title = 'Fade in images';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(title),
        ),
        body: Stack(
          children: <Widget>[
            const Center(child: CircularProgressIndicator()),
            Center(
              child: FadeInImage.memoryNetwork(
                placeholder: kTransparentImage,
                image: 'https://picsum.photos/250?image=9',
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

![Fading In Image Demo]({{site.url}}/assets/images/docs/cookbook/fading-in-images.gif){:.site-mobile-screenshot}

## From asset bundle

## 從本地儲存載入佔位符

You can also consider using local assets for placeholders.
First, add the asset to the project's `pubspec.yaml` file
(for more details, see [Adding assets and images][]):

也可以考慮使用本地資源作為佔位符。
首先將資源新增到專案的 `pubspec.yaml` 檔案中
（更多細節請參閱文件: [新增資源和圖片][Adding assets and images]）：

```diff
 flutter:
   assets:
+    - assets/loading.gif
```

Then, use the [`FadeInImage.assetNetwork()`][] constructor:

然後使用 [`FadeInImage.assetNetwork()`][] 建構函式：

<?code-excerpt "lib/asset_main.dart (AssetNetwork)" replace="/^child\: //g"?>
```dart
FadeInImage.assetNetwork(
  placeholder: 'assets/loading.gif',
  image: 'https://picsum.photos/250?image=9',
),
```

### Complete example

### 完整範例

<?code-excerpt "lib/asset_main.dart"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    const title = 'Fade in images';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(title),
        ),
        body: Center(
          child: FadeInImage.assetNetwork(
            placeholder: 'assets/loading.gif',
            image: 'https://picsum.photos/250?image=9',
          ),
        ),
      ),
    );
  }
}
```

![Asset fade-in]({{site.url}}/assets/images/docs/cookbook/fading-in-asset-demo.gif){:.site-mobile-screenshot}


[Adding assets and images]: {{site.url}}/ui/assets/assets-and-images
[`FadeInImage`]: {{site.api}}/flutter/widgets/FadeInImage-class.html
[`FadeInImage.assetNetwork()`]: {{site.api}}/flutter/widgets/FadeInImage/FadeInImage.assetNetwork.html
[`transparent_image`]: {{site.pub-pkg}}/transparent_image
