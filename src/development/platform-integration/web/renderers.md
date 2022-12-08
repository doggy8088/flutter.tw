---
title: Web renderers
title: Web 渲染器
description: How to choose a web renderer for running and building a web app.
description: 選擇合適的渲染器來執行和建構 Web 應用。
---

When running and building apps for the web, you can choose between two different
renderers. This page describes both renderers and how to choose the best one for
your needs. The two renderers are:

你可以選擇兩種不同的渲染器來執行和建構 Web 應用。
下文介紹兩種渲染器以及它們的適用場景:

你可以選擇兩種不同的渲染器來執行和建構 Web 應用。下文介紹兩種渲染器以及它們的適用場景。

**HTML renderer**
<br> Uses a combination of HTML elements, CSS, Canvas elements, and SVG elements.
This renderer has a smaller download size.

**使用 HTML 渲染**
<br> 使用 HTML，CSS，Canvas 和 SVG 元素來渲染，應用的大小相對較小。

**CanvasKit renderer**
<br> This renderer is fully consistent with Flutter mobile and desktop, has
  faster performance with higher widget density, but adds about 2MB in
  download size.

**使用 CanvasKit 渲染**
<br> 應用在移動和桌面端保持一致，
有更好的效能，以及降低不同瀏覽器渲染效果不一致的風險。但是應用的大小會增加大約 2MB。
  
## Command line options

## 命令列引數

The `--web-renderer` command line option takes one of three values, `auto`,
`html`, or `canvaskit`.

`--web-renderer` 可選引數值為 `auto`、`html` 或 `canvaskit`。

* `auto` (default) - automatically chooses which renderer to use. This option
  chooses the HTML renderer when the app is running in a mobile browser, and
  CanvasKit renderer when the app is running in a desktop browser.
  
  `auto`（預設）- 自動選擇渲染器。移動端瀏覽器選擇 HTML，桌面端瀏覽器選擇 CanvasKit。
  
* `html` - always use the HTML renderer

  `html` - 強制使用 HTML 渲染器。

* `canvaskit` - always use the CanvasKit renderer

  `canvaskit` - 強制使用 CanvasKit 渲染器。

This flag can be used with the `run` or `build` subcommands. For example:

此選項適用於 `run` 和 `build` 命令。例如：

```
flutter run -d chrome --web-renderer html
flutter build web --web-renderer canvaskit
```

This flag is ignored when a non-browser (mobile or desktop) device
target is selected.

如果執行/建構目標是非瀏覽器裝置（即移動裝置或桌面裝置），這個選項會被忽略。

## Runtime configuration

## 配置執行時

To override the web renderer at runtime:

要覆寫 web 即時渲染器請執行以下操作：

* Build the app with the `auto` option.

  使用 auto 選項建構應用。

* Insert a `<script>` tag  in `web/index.html` file before the `main.dart.js`
  script.
 
  在 `web/index.html` 檔案的 `main.dart.js` 前插入 `<script>` 標籤。

* Set `window.flutterWebRenderer` to `"canvaskit"` or `"html"`:

  配置 `window.flutterWebRenderer` 為 `"canvaskit"` 或者 `"html"`：

```html
  <script type="text/javascript">
    let useHtml = // ...
    if(useHtml) {
      window.flutterWebRenderer = "html";
    } else {
      window.flutterWebRenderer = "canvaskit";
    }
  </script>
  <script src="main.dart.js" type="application/javascript"></script>
```

The web renderer can't be changed after the Flutter engine startup process
begins in `main.dart.js`.

Flutter engine 啟動之後無法再在 `main.dart.js` 更換 web 渲染器。

## Choosing which option to use

## 選擇合適的渲染器

Choose the `auto` option (default) if you are optimizing for download size on
mobile browsers and optimizing for performance on desktop browsers.

如果您在移動端瀏覽器平臺上更關心應用大小，而桌面端瀏覽器更關心效能，請選擇 `auto` 選項（預設）。

Choose the `html` option if you are optimizing download size over performance on
both desktop and mobile browsers.

如果您在移動端和桌面端都更關心應用大小，請選擇 `html` 選項。

Choose the `canvaskit` option if you are prioritizing performance and
pixel-perfect consistency on both desktop and mobile browsers.

`canvaskit`：移動端和桌面端都更關心效能，和跨瀏覽器的畫素級一致性。

## Examples

## 範例

Run in Chrome using the default renderer option (`auto`):

在 Chrome 瀏覽器上使用預設 (`auto`) 渲染器執行：

```
flutter run -d chrome
```

Build your app in release mode, using the default (auto) option:

使用預設 (`auto`) 渲染器建構應用（釋出模式）：

```
flutter build web --release
```

Build your app in release mode, using just the CanvasKit renderer:

使用 CanvasKit 渲染器建構應用（釋出模式）：

```
flutter build web --web-renderer canvaskit --release
```

Run your app in profile mode using the HTML renderer:

使用 HTML 渲染器建構應用（釋出模式）：

```
flutter run -d chrome --web-renderer html --profile
```

[file an issue]: {{site.repo.flutter}}/issues/new?title=[web]:+%3Cdescribe+issue+here%3E&labels=%E2%98%B8+platform-web&body=Describe+your+issue+and+include+the+command+you%27re+running,+flutter_web%20version,+browser+version
