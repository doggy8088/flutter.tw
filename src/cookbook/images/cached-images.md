---
title: Work with cached images
title: 使用快取圖片
description: How to work with cached images.
description: 如何操作快取的圖片。
tags: cookbook, 實用課程, 使用圖片
keywords: Flutter使用圖片,圖片快取,網路圖片,API
prev:
  title: Fade in images with a placeholder
  title: 佔位符和網路圖片淡入
  path: /docs/cookbook/images/fading-in-images
next:
  title: Use lists
  title: 基礎列表
  path: /docs/cookbook/lists/basic-list
---

<?code-excerpt path-base="cookbook/images/cached_images"?>

In some cases, it's handy to cache images as they're downloaded from the
web, so they can be used offline. For this purpose,
use the [`cached_network_image`][] package.

在一些情況下，快取從網路下載的圖片用於離線顯示是十分方便的，
你需要引入 [`cached_network_image`][] 這個 package 來實現這項功能。

{{site.alert.note}}

  To learn more, watch this short Package of the Week video on the cached_network_image package:

  瞭解更多，請參考下方「每週 Widget」的裡關於 cached_network_image 的短影片：

  <iframe class="full-width" src="{{site.youtube-site}}/embed/fnHr_rsQwDA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{{site.alert.end}}

In addition to caching, the `cached_network_image`
package also supports placeholders and fading images
in as they're loaded.

除了快取，`cached_image_network` 包也支援佔位符和載入後的圖片淡入。

<?code-excerpt "lib/simple.dart (SimpleCachedImage)" replace="/^return //g"?>
```dart
CachedNetworkImage(
  imageUrl: 'https://picsum.photos/250?image=9',
);
```

## Adding a placeholder

## 新增佔位符

The `cached_network_image` package allows you to use any widget as a
placeholder. In this example, display a spinner while the image loads.

`cached_network_image` 包允許任何 widget 充當佔位符。
在本例中，載入圖片時會展示一個旋轉載入的效果（spinner）作為佔位符。

<?code-excerpt "lib/main.dart (CachedNetworkImage)" replace="/^child\: //g"?>
```dart
CachedNetworkImage(
  placeholder: (context, url) => const CircularProgressIndicator(),
  imageUrl: 'https://picsum.photos/250?image=9',
),
```

## Complete example

## 完整範例

<?code-excerpt "lib/main.dart"?>
```dart
import 'package:cached_network_image/cached_network_image.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    const title = 'Cached Images';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(title),
        ),
        body: Center(
          child: CachedNetworkImage(
            placeholder: (context, url) => const CircularProgressIndicator(),
            imageUrl: 'https://picsum.photos/250?image=9',
          ),
        ),
      ),
    );
  }
}
```


[`cached_network_image`]: {{site.pub-pkg}}/cached_network_image
