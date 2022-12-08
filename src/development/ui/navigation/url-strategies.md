---
title: Configuring the URL strategy on the web
title: 配置 Web 應用的 URL 策略
description: Use hash or path URL strategies on the web
description: 在 Web 上使用路徑 URL 策略
---

Flutter web apps support two ways of configuring
URL-based navigation on the web:

Flutter Web 應用支援兩種基於 URL 的路由的配置方式：

**Hash (default)**
<br> Paths are read and written to the [hash fragment][].
For example, `flutterexample.dev/#/path/to/screen`.


**Hash（預設）**
<br> 路徑使用 [# + 錨點識別符號][hash fragment] 讀寫，
  例如：`flutterexample.dev/#/path/to/screen`。

**Path**
<br>  Paths are read and written without a hash. For example,
`flutterexample.dev/path/to/screen`.

**Path**
<br> 路徑使用非 # 讀寫，
  例如：`flutterexample.dev/path/to/screen`。

## Configuring the URL strategy

## 配置 URL 策略

To configure Flutter to use the path instead, use the
[usePathUrlStrategy][] function provided by the [flutter_web_plugins][] library
in the SDK:

讓 Flutter 使用 path 策略，請使用 [flutter_web_plugins][]
庫中提供的 [`setUrlStrategy`][] 方法。

```dart
import 'package:flutter_web_plugins/url_strategy.dart';

void main() {
  usePathUrlStrategy();
  runApp(ExampleApp());
}
```

## Configuring your web server

PathUrlStrategy uses the [History API][], which requires additional
configuration for web servers.

To configure your web server to support PathUrlStrategy, check your web server's
documentation to rewrite requests to `index.html`.Check your web server's
documentation for details on how to configure single-page apps.

If you are using Firebase Hosting, choose the "Configure as a single-page app"
option when initializing your project. For more information see Firebase's
[Configure rewrites][] documentation.

The local dev server created by running `flutter run -d chrome` is configured to
handle any path gracefully and fallback to your app's `index.html` file.

## Hosting a Flutter app at a non-root location

## 將 Flutter 應用部署在子目錄下

Update the `<base href="/">` tag in `web/index.html`
to the path where your app is hosted.
For example, to host your Flutter app at
`my_app.dev/flutter_app`, change
this tag to `<base href="/flutter_app/">`.

更新 `web/index.html` 中的 `<base href="/">` 標籤為你的應用部署路徑。
例如：如果你期望將 Flutter 應用部署在 `myapp.dev/flutter_app`，
則更改此標籤為 `<base href="/flutter_app/">`。

[hash fragment]: https://en.wikipedia.org/wiki/Uniform_Resource_Locator#Syntax
[`HashUrlStrategy`]: {{site.api}}/flutter/flutter_web_plugins/HashUrlStrategy-class.html
[`PathUrlStrategy`]: {{site.api}}/flutter/flutter_web_plugins/PathUrlStrategy-class.html
[`setUrlStrategy`]: {{site.api}}/flutter/flutter_web_plugins/setUrlStrategy.html
[`url_strategy`]: {{site.pub-pkg}}/url_strategy
[usePathUrlStrategy]: https://api.flutter.dev/flutter/flutter_web_plugins/usePathUrlStrategy.html
[flutter_web_plugins]: https://api.flutter.dev/flutter/flutter_web_plugins/flutter_web_plugins-library.html
[History API]: https://developer.mozilla.org/en-US/docs/Web/API/History_API
[Configure rewrites]: https://firebase.google.com/docs/hosting/full-config#rewrites
