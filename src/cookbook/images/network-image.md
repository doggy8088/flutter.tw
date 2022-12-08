---
title: Display images from the internet
title: 顯示網路上的遠端圖片
description: How to display images from the internet.
description: 如何顯示網路上的圖片。
tags: cookbook, 實用課程, 使用圖片
keywords: 網路圖片,使用gif
prev:
  title: Implement swipe to dismiss
  title: 實現「滑動清除」效果
  path: /docs/cookbook/gestures/dismissible
next:
  title: Fade in images with a placeholder
  title: 佔位符和網路圖片淡入
  path: /docs/cookbook/images/fading-in-images
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/images/network_image"?>

Displaying images is fundamental for most mobile apps.
Flutter provides the [`Image`][] widget to
display different types of images.

對大多數移動應用來說，圖片顯示是一項基礎功能。
Flutter 提供了 [`Image`][] 來顯示不同型別的圖片。

To work with images from a URL, use the
[`Image.network()`][] constructor.

使用 [`Image.network()`][] 建構函式來處理來自 URL 的圖片。

<?code-excerpt "lib/main.dart (ImageNetwork)" replace="/^body\: //g"?>
```dart
Image.network('https://picsum.photos/250?image=9'),
```

## Bonus: animated gifs

## 意外之喜：Gif 動畫

One useful thing about the `Image` widget:
It supports animated gifs.

`Image` widget 令人興奮的特性之一：提供了開箱即用的 gif 動畫支援！

<?code-excerpt "lib/gif.dart (Gif)" replace="/^return\ //g"?>
```dart
Image.network(
    'https://docs.flutter.dev/assets/images/dash/dash-fainting.gif');
```

## Placeholders and caching

## 佔位符與快取

The default `Image.network` constructor doesn't handle more advanced
functionality, such as fading images in after loading, or caching images
to the device after they're downloaded. To accomplish these tasks, see
the following recipes:

預設的 `Image.network` 建構函式並沒有提供諸如載入後的圖片淡入或
下載之後將圖片快取到裝置等更進一步的功能。
請參閱以下連結來實現這些功能：

* [Fade in images with a placeholder][]
  
  [佔位符和網路圖片淡入][Fade in images with a placeholder]
  
* [Work with cached images][]
  
  [使用快取圖片][Work with cached images]

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    var title = 'Web Images';

    return MaterialApp(
      title: title,
      home: Scaffold(
        appBar: AppBar(
          title: Text(title),
        ),
        body: Image.network('https://picsum.photos/250?image=9'),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/network-image.png" alt="Network image demo" class="site-mobile-screenshot" />
</noscript>


[Fade in images with a placeholder]: {{site.url}}/cookbook/images/fading-in-images
[`Image`]: {{site.api}}/flutter/widgets/Image-class.html
[`Image.network()`]: {{site.api}}/flutter/widgets/Image/Image.network.html
[Work with cached images]: {{site.url}}/cookbook/images/cached-images
