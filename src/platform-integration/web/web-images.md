---
title: Displaying images on the web
title: 在 Web 中展示圖片
short-title: Web images
short-title: Web 圖片
description: Learn how to load and display images on the web.
description: 學習如何在 Web 中載入和展示圖片。
---

The web supports the standard [`Image`][1] widget to display images.
However, because web browsers are built to run untrusted code safely,
there are certain limitations in what you can do with images compared
to mobile and desktop platforms. This page explains these limitations
and offers ways to work around them.

Web 支援使用標準的 [`Image`][1] 元件來展示圖片。
然而，Web 瀏覽器和手機以及桌面平台相比，在處理圖片上會有一定的侷限性，
因為它需要以安全的方式執行未信任的程式碼。
本頁面內容解釋了這些限制，並提供一些解決方法。

# Background

# 背景

This section summarizes the technologies available
across Flutter and the web,
on which the solutions below are based on.

本節概括了 Flutter Web 中可用的技術，下述的解決方案都基於此。

## Images in Flutter

Flutter offers the [`Image`][1] widget as well as the low-level
[`dart:ui/Image`][11] class for rendering images.
The `Image` widget has enough functionality for most use-cases.
The `dart:ui/Image` class can be used in
advanced situations where fine-grained control
of the image is needed.

Flutter 提供了 [`Image`][1] 元件以及底層的 [`dart:ui/Image`][11] 類來渲染圖片。
`Image` 元件的功能足夠滿足大部分使用場景。
`dart:ui/Image` 類可用於需要精細控制圖片的場景。

## Images on the web

## Web 中的圖片

The web offers several methods for displaying images.
Below are some of the common ones:

Web 提供了多種展示圖片的方式，下面是幾種常見的：

- The built-in [`<img>`][2] and [`<picture>`][3] HTML elements.
  
  內建的 [`<img>`][2] 和 [`<picture>`][3] HTML 元素。
  
- The [`drawImage`][4] method on the [`<canvas>`][5] element.

  [`<canvas>`][5] 元素中的 [`drawImage`][4] 方法。

- Custom image codec that renders to a WebGL canvas.

  可渲染至 WebGL 畫布的自訂圖片編解碼器。

Each option has its own benefits and drawbacks.
For example, the built-in elements fit nicely among
other HTML elements, and they automatically take
advantage of browser caching, and built-in image
optimization and memory management.
They allow you to safely display images from arbitrary sources
(more on than in the CORS section below).
`drawImage` is great when the image must fit within
other content rendered using the `<canvas>` element.
You also gain control over image sizing and,
when the CORS policy allows it, read the pixels
of the image back for further processing.
Finally, WebGL gives you the highest degree of
control over the image. Not only can you read the pixels and
apply custom image algorithms, but you can also use GLSL for
hardware-acceleration.

每種方式都有各自的優缺點。例如：內建的元素和其他 HTML 元素完美契合，
並且自帶瀏覽器快取、內建圖片最佳化和記憶體管理的優勢。
這使得你可以安全地展示任意來源的圖片（超越了下節中的 CORS）。
當圖片必須和 `<canvas>` 元素中渲染的其他內容適配時，`drawImage` 更合適。
當 CORS 政策允許時，你也可以獲取圖片尺寸的控制權，讀取畫素資訊用於進一步處理。
最後，WebGL 給予了你最大限度的圖片控制權。
你不僅可以讀取畫素資訊、應用自訂的圖片演算法，還可以使用 GLSL 實現硬體加速。

## Cross-Origin Resource Sharing (CORS)

## 跨域資源共享 (CORS)

[CORS][6] is a mechanism that browsers use to control
how one site accesses the resources of another site.
It is designed such that, by default, one web-site
is not allowed to make HTTP requests to another site
using [XHR][21] or [`fetch`][22].
This prevents scripts on another site from acting on behalf
of the user and from gaining access to another
site's resources without permission.

[CORS][6] 是一種瀏覽器用來控制一個站點如何獲取另一個站點的資源的機制。
預設情況下，一個網站不允許使用 [XHR][21] 或者 [`fetch`][22] 的
方式向另一個站點發送 HTTP 請求。
這樣可以阻止另一個站點代表使用者執行指令碼，
以及無需許可權就可以獲取另一個站點的資源。

When using `<img>`, `<picture>`, or `<canvas>`,
the browser automatically blocks access to pixels
when it knows that an image is coming from another site
and the CORS policy disallows access to data.

當使用 `<img>`、`<picture>` 或者 `<canvas>` 時，
如果瀏覽器發現圖片來源於另一個站點，且 CORS 政策不允許存取資料時，
瀏覽器會自動阻止資訊的存取許可權。

WebGL requires access to the image data in order
to be able to render the image. Therefore,
images to be rendered using WebGL must only come from servers
that have a CORS policy configured to work with
the domain that serves your application.

WebGL 需要存取圖片資訊以用於渲染圖片。
因此要想使用 WebGL 渲染圖片，該圖片的來源服務所在的域名必須配置有效且可用的 CORS 策略。

## Flutter renderers on the web

## Web 中的 Flutter 渲染器

Flutter offers a choice of two renderers on the web:

Flutter 在 Web 中提供了兩種可供選擇的渲染器：

* **HTML**: this renderer uses a combination of HTML,
  CSS, Canvas 2D, and SVG to render UI.
  It uses the `<img>` element to render images.
  
  **HTML**：該渲染器使用了 HTML、CSS、Canvas 2D 和 SVG 組合的方式渲染 UI。
  使用 `<img>` 元素渲染圖片。
  
* **CanvasKit**: this renderer uses WebGL to render UI,
  and therefore requires
  access to the pixels of the image.
  
  **CanvasKit**：該渲染器使用了 WebGL 渲染 UI，
  因此需要存取圖片的畫素資訊。

Because the HTML renderer uses the `<img>`
element it can display images from
arbitrary sources. However,
this places the following limitations on what you
can do with them:

因為 HTML 渲染器使用了 `<img>` 元素，所以可以展示任意來源的圖片。
但是，這也給你帶來了以下限制：

* Limited support for [`Image.toByteData`][7].
  
  對 [`Image.toByteData`][7] 的支援有限。
  
* No support for [`OffsetLayer.toImage`][8] and
  [`Scene.toImage`][10].
  
  不支援 [`OffsetLayer.toImage`][8] 和 [`Scene.toImage`][10]。
  
* No access to frame data in animated images
  ([`Codec.getNextFrame`][9],
  `frameCount` is always 1, `repetitionCount` is always 0).
  
  無法獲取動畫中的幀資訊（[`Codec.getNextFrame`][9]，
  `frameCount` 永遠為 1，`repetitionCount` 永遠為 0）。
  
* No support for `ImageShader`.

  不支援 `ImageShader`。
  
* Limited support for shader effects that can be applied to images.

  應用於圖片的著色器效果的支援有限。

* No control over image memory (`Image.dispose` has no effect).
  The memory is managed by the browser behind-the-scenes.
  
  不可控制圖片記憶體（`Image.dispose` 無效）。記憶體由瀏覽器自行控制。

The CanvasKit renderer implements Flutter's image API fully.
However, it requires access to image pixels to do so,
and is therefore subject to the CORS policy.

CanvasKit 完全實現了 Flutter 中的圖片 API。
但它需要存取圖片的畫素資訊，因此受制於 CORS 政策。

# Solutions

# 解決方案

## In-memory, asset, and same-origin network images

## 記憶體、asset 和同源網路圖片

If the app has the bytes of the encoded image in memory,
provided as an [asset][12], or stored on the
same server that serves the application
(also known as _same-origin_), no extra effort is necessary.
The image can be displayed using
[`Image.memory`][13], [`Image.asset`][14], and [`Image.network`][15]
in both HTML and CanvasKit modes.

如果圖片在應用記憶體中有編碼後的位元組資訊、或者以 [asset][12] 的方式提供、
或者和應用儲存在同一伺服器上（也就是同源），則不需要做額外工作。
圖片既可以在 HTML 也可以在 CanvasKit 模式下，使用 [`Image.memory`][13]、
[`Image.asset`][14] 和 [`Image.network`][15] 來展示。

## Cross-origin images

## 跨域圖片

The HTML renderer can load cross-origin images
without extra configuration.

HTML 渲染器可以載入跨域圖片，無需額外配置。

CanvasKit requires that the app gets the bytes of the encoded image.
There are several ways to do this, discussed below.

CanvasKit 需要應用獲取編碼後的圖片的位元組資訊。
有多種獲取方式，如下文所述。

### Host your images in a CORS-enabled CDN.

### 在支援 CORS 的 CDN 中部署你的圖片

Typically, content delivery networks (CDN)
can be configured to customize what domains
are allowed to access your content.
For example, Firebase site hosting allows
[specifying a custom][16] `Access-Control-Allow-Origin`
header in the `firebase.json` file.

通常，可以配置內容分發網路 (CDN) 來自訂哪些域名可以存取你的內容。
例如：Firebase 站點託管允許在 `firebase.json` 檔案中，
[指定一個自訂的][16] `Access-Control-Allow-Origin` 頭。

### Lack control over the image server? Use a CORS proxy.

### 沒有圖片伺服器控制權？使用一個 CORS 代理

If the image server cannot be configured to allow CORS
requests from your application,
you might still be able to load images by proxying
the requests through another server. This requires that the
intermediate server has sufficient access to load the images.

如果無法從你的應用層面去配置圖片伺服器的 CORS，
你依然可以透過另一個伺服器代理請求，從而載入圖片。
這要求中轉伺服器對圖片載入有充分的存取權。

This method can be used in situations when the original
image server serves images publicly,
but is not configured with the correct CORS headers.

此方法適用於源圖片伺服器公開提供了圖片，卻沒有正確配置 CORS 頭的情況。

Examples:

例子：

* Using [CloudFlare Workers][18].

  使用 [CloudFlare Workers][18]。
  
* Using [Firebase Functions][19].

  使用 [Firebase Functions][19]。

### Use `<img>` in a platform view.

### 在平臺視圖中使用 `<img>`

Flutter supports embedding HTML inside the app using
[`HtmlElementView`][17].  Use it to create an `<img>`
element to render the image from another domain.
However, do keep in mind that this comes with the
limitations explained in the section
"Flutter renderers on the web" above.

Flutter 支援在應用中使用 [`HtmlElementView`][17] 嵌入 HTML。
透過它可以建立一個 `<img>` 元素來渲染另一個域名的圖片。
但是，一定要記住，此方法也附帶了
「Web 中的 Flutter 渲染器」一節中提到的限制。

[As of today][20], using too many HTML elements
with the CanvasKit renderer might hurt performance.
If images interleave non-image content Flutter needs to
create extra WebGL contexts between the `<img>` elements.
If your application needs to display a lot of images
on the same screen all at once, consider using
the HTML renderer instead of CanvasKit.

[就目前而言][20]，在 CanvasKit 渲染器中過多地使用 HTML 元素，
可能較為影響效能。如果圖片和非圖片內容交替出現，
Flutter 需要在 `<img>` 元素之間建立額外的 WebGL 上下文。
如果你的應用需要一次性在同一螢幕中展示大量的圖片，
請考慮使用 HTML 渲染器替代 CanvasKit。


[1]: {{site.api}}/flutter/widgets/Image-class.html
[2]: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img
[3]: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture
[4]: https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/drawImage
[5]: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas
[6]: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
[7]: {{site.api}}/flutter/dart-ui/Image/toByteData.html
[8]: {{site.api}}/flutter/rendering/OffsetLayer/toImage.html
[9]: {{site.api}}/flutter/dart-ui/Codec/getNextFrame.html
[10]: {{site.api}}/flutter/dart-ui/Scene/toImage.html
[11]: {{site.api}}/flutter/dart-ui/Image-class.html
[12]: {{site.url}}/ui/assets/assets-and-images
[13]: {{site.api}}/flutter/widgets/Image/Image.memory.html
[14]: {{site.api}}/flutter/widgets/Image/Image.asset.html
[15]: {{site.api}}/flutter/widgets/Image/Image.network.html
[16]: {{site.firebase}}/docs/hosting/full-config#headers
[17]: {{site.api}}/flutter/widgets/HtmlElementView-class.html
[18]: https://developers.cloudflare.com/workers/examples/cors-header-proxy
[19]: {{site.github}}/7kfpun/cors-proxy
[20]: {{site.repo.flutter}}/issues/71884
[21]: https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest
[22]: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch
