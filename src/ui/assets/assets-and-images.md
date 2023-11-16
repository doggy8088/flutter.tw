---
title: Adding assets and images
title: 新增資源和圖片
description: How to use images (and other assets) in your Flutter app.
description: 如何在你的 Flutter 應用中使用圖片或者其他型別的資源。
short-title: Assets and images
short-title: 資源和圖片
tags: 使用者介面,Flutter UI,佈局
keywords: Flutter資源最佳化,新增圖片
---

<?code-excerpt path-base="ui/assets_and_images/lib"?>

Flutter apps can include both code and _assets_
(sometimes called resources). An asset is a file
that is bundled and deployed with your app,
and is accessible at runtime. Common types of assets include
static data (for example, JSON files),
configuration files, icons, and images
(JPEG, WebP, GIF, animated WebP/GIF, PNG, BMP, and WBMP).

Flutter 應用程式包含程式碼和 **assets**（也為資源）。資源是被打包到應用程式安裝套件中，
可以在執行時存取的一種檔案。常見的資源型別包括靜態資料（例如 JSON 檔案），配置檔案，圖示和
圖片（JPEG，WebP，GIF，動畫 WebP / GIF，PNG，BMP 和 WBMP）。

## Specifying assets

## 指定資源

Flutter uses the [`pubspec.yaml`][] file,
located at the root of your project,
to identify assets required by an app.

Flutter 使用 [`pubspec.yaml`][] 檔案，
位於專案根目錄，來識別應用程式所需的資源。

Here is an example:

下面舉個例子:

```yaml
flutter:
  assets:
    - assets/my_icon.png
    - assets/background.png
```

To include all assets under a directory,
specify the directory name with the `/` character at the end:

如果要包含一個目錄下的所有 assets，
需要在目錄名稱的結尾加上  `/`：

```yaml
flutter:
  assets:
    - directory/
    - directory/subdirectory/
```

{{site.alert.note}}

  Only files located directly in the directory are included.
  [Resolution-aware asset image variants](#resolution-aware) are the only exception.
  To add files located in subdirectories, create an entry per directory.

  僅包含當前目錄下的所有檔案，以及子目錄下（與主目錄中的檔案）的同名檔案
  （請參閱 [Asset 變體](#resolution-aware)）。如果想要新增子資料夾中的檔案，
  請為每個目錄建立一個條目。

{{site.alert.end}}

### Asset bundling

### Asset bundling (應用打包資源)

The `assets` subsection of the `flutter` section
specifies files that should be included with the app.
Each asset is identified by an explicit path
(relative to the `pubspec.yaml` file) where the asset
file is located. The order in which the assets are
declared doesn't matter. The actual directory name used
(`assets` in first example or `directory` in the above
example) doesn't matter.

`yaml` 檔案 `flutter` 下面的 `assets` 部分指定了需要包含在應用中的檔案。
每個資源都透過相對於 `pubspec.yaml` 檔案所在位置的路徑進行標識。
資源的宣告順序是無關緊要的。
資源的實際目錄可以是任意資料夾（在第一個範例中是 `assets`，其他的是 `directory`）

During a build, Flutter places assets into a special
archive called the _asset bundle_ that apps read
from at runtime.

在一次建構中，Flutter 將 assets 放到 **asset bundle**
的特殊歸檔中，以便應用在執行時讀取它們。

## Loading assets

## 載入 assets

Your app can access its assets through an
[`AssetBundle`][] object.

你的應用程式可以透過 [`AssetBundle`][] 物件存取其資源。

The two main methods on an asset bundle allow you to load a
string/text asset (`loadString()`) or an image/binary asset (`load()`)
out of the bundle, given a logical key. The logical key maps to the path
to the asset specified in the `pubspec.yaml` file at build time.

Asset bundle 透過指定一個邏輯鍵（key），允許你讀取 string/text（`loadString`）
和 image/binary（`load`）。在編譯期間，
這個邏輯鍵（key）會對映在 `pubspec.yaml` 中指定的資源路徑。

### Loading text assets

### 載入文字 assets

Each Flutter app has a [`rootBundle`][]
object for easy access to the main asset bundle.
It is possible to load assets directly using the
`rootBundle` global static from
`package:flutter/services.dart`.

每個 Flutter 應用程式都有一個 [`rootBundle`][] 物件， 
可以輕鬆存取主資源 bundle 。還可以直接使用
`package:flutter/services.dart` 中
全域靜態的 `rootBundle` 來載入資源。

However, it's recommended to obtain the `AssetBundle`
for the current `BuildContext` using
[`DefaultAssetBundle`][], rather than the default
asset bundle that was built with the app; this
approach enables a parent widget to substitute a
different `AssetBundle` at run time,
which can be useful for localization or testing
scenarios.

但是，如果獲取當前 `BuildContext` 的 `AssetBundle`，建議
使用 [`DefaultAssetBundle`][]。
這種方式不是使用應用程式建構的預設資源 bundle，而是讓父級 widget 在
執行時替換的不同的 AssetBundle，這對於本地化或測試場景很有用。

Typically, you'll use `DefaultAssetBundle.of()`
to indirectly load an asset, for example a JSON file,
from the app's runtime `rootBundle`.

通常，你可以從應用程式執行時的 `rootBundle` 中，間接使用 `DefaultAssetBundle.of()` 
來載入資源（例如 JSON 檔案）。

{% comment %}
  Need example here to show obtaining the AssetBundle for the current
  BuildContext using DefaultAssetBundle.of
{% endcomment %}

Outside of a `Widget` context, or when a handle
to an `AssetBundle` is not available,
you can use `rootBundle` to directly load such assets.
For example:

在 Widget 上下文之外，或 AssetBundle 的控制代碼不可用時，你可以使用 `rootBundle` 
直接載入這些 assets，例如：

<?code-excerpt "main.dart (RootBundle)"?>
```dart
import 'package:flutter/services.dart' show rootBundle;

Future<String> loadAsset() async {
  return await rootBundle.loadString('assets/config.json');
}
```

### Loading images

### 載入圖片

To load an image, use the [`AssetImage`][]
class in a widget's `build()` method.

你可以在 `build()` 方法中使用 [`AssetImage`][] 載入圖片。

For example, your app can load the background
image from the asset declarations in the previous example:

舉個例子，下面的程式碼載入了先前宣告的背景圖片：

<?code-excerpt "main.dart (BackgroundImage)"?>
```dart
return const Image(image: AssetImage('assets/background.png'));
```

### Resolution-aware image assets {#resolution-aware}

### 解析度自適應圖片資源 {#resolution-aware}

Flutter can load resolution-appropriate images for
the current [device pixel ratio][].

Flutter 可以為當前裝置載入適合其
[裝置畫素比][device pixel ratio] 的圖像。

[`AssetImage`][] will map a logical requested
asset onto one that most closely matches the current
[device pixel ratio][].

[`AssetImage`][] 可以將請求資源對映到最接近當前
[裝置畫素比][device pixel ratio] 的資源。

For this mapping to work, assets should be arranged
according to a particular directory structure:

為了使這種對映起作用，資源應該根據特定的目錄結構來儲存：

```text
.../image.png
.../Mx/image.png
.../Nx/image.png
...etc.
```

Where _M_ and _N_ are numeric identifiers that correspond
to the nominal resolution of the images contained within.
In other words, they specify the device pixel ratio that
the images are intended for.

其中 _M_ 和 _N_  是數字識別符號，對應於其中包含的圖像的解析度，換句話說，
它們指定不同裝置畫素比例的圖片。

In this example, `image.png` is considered the *main asset*,
while `Mx/image.png` and `Nx/image.png` are considered to be
*variants*.

在範例中，`image.png` 是 **主資源**，
而 `Mx/image.png` 和 `Nx/image.png` 則被認為是 **變體**。

The main asset is assumed to correspond to a resolution of 1.0.
For example, consider the following asset layout for an
image named `my_icon.png`:

主資源預設對應於 1.0 倍的解析度圖片。比如下面的圖片 `my_icon.png`：

```text
.../my_icon.png       (mdpi baseline)
.../1.5x/my_icon.png  (hdpi)
.../2.0x/my_icon.png  (xhdpi)
.../3.0x/my_icon.png  (xxhdpi)
.../4.0x/my_icon.png  (xxxhdpi)
```

On devices with a device pixel ratio of 1.8, the asset
`.../2.0x/my_icon.png` is chosen.
For a device pixel ratio of 2.7, the asset
`.../3.0x/my_icon.png` is chosen.

而在裝置畫素比率為 1.8 的裝置上，對應是 `.../2.0x/my_icon.png` 。
如果是 2.7 的裝置畫素比，對應是 `.../3.0x/my_icon.png` 。

If the width and height of the rendered image are not specified
on the `Image` widget, the nominal resolution is used to scale
the asset so that it occupies the same amount of screen space
as the main asset would have, just with a higher resolution.
That is, if `.../my_icon.png` is 72px by 72px, then
`.../3.0x/my_icon.png` should be 216px by 216px;
but they both render into 72px by 72px (in logical pixels),
if width and height are not specified.

如果在 `Image` widget 上未指定渲染圖像的寬度和高度，
通常會擴充資源來保證與主資源相同的螢幕空間量，
並不是相同的物理畫素，只是解析度更高。
換句話說，`.../my_icon.png` 是 72 px 乘 72 px，
那麼 `.../3.0x/my_icon.png` 應該是 216 px 乘 216 px；
但如果未指定寬度和高度，
它們都將渲染為 72 px 乘 72 px（以邏輯畫素為單位）。

{{site.alert.note}}

  [Device pixel ratio][] depends on [MediaQueryData.size][], which requires having either
  [MaterialApp][] or [CupertinoApp][] as an ancestor of your [`AssetImage`][].

  [裝置畫素比][device pixel ratio] 依賴於 [MediaQueryData.size][]，
  它們需要你的 [`AssetImage`][] 的上層節點中存在 [MaterialApp][] 或者 [CupertinoApp][]。 

{{site.alert.end}}

#### Bundling of resolution-aware image assets {#resolution-aware-bundling}

You only need to specify the main asset or its parent directory
in the `assets` section of `pubspec.yaml`.
Flutter bundles the variants for you.
Each entry should correspond to a real file, with the exception of
the main asset entry. If the main asset entry doesn't correspond
to a real file, then the asset with the lowest resolution
is used as the fallback for devices with device pixel
ratios below that resolution. The entry should still
be included in the `pubspec.yaml` manifest, however.

你只需要在 `pubspec.yaml` 的 `assets` 部分指定主要資源，
Flutter 會自動幫你繫結其他變體。
在 `pubspec.yaml` 中資源部分的每一項都應與實際檔案相對應，
除過主資源節點。當主資源缺少某個檔案時，會按解析度從低到高的順序去選擇，
也就是說 1x 中沒有的話會在 2x 中找，2x 中還沒有的話就在 3x 中找。
該條目需要在 `pubspec.yaml` 中指定。

Anything using the default asset bundle inherits resolution
awareness when loading images. (If you work with some of the lower
level classes, like [`ImageStream`][] or [`ImageCache`][],
you'll also notice parameters related to scale.)

使用預設的資源 bundle 載入資源時，系統會自動處理解析度等。
（如果你使用一些更低級別的類，如 [`ImageStream`][] 或
[`ImageCache`][]，你需要注意 scale 相關的引數)。

### Asset images in package dependencies {#from-packages}

### 相依套件中的資源圖片

To load an image from a [package][] dependency,
the `package` argument must be provided to [`AssetImage`][].

載入依賴 [package][] 中的圖像，
必須給 [`AssetImage`][] 提供 `package` 引數。

For instance, suppose your application depends on a package
called `my_icons`, which has the following directory structure:

例如，你的應用程式依賴於一個名為 `my_icons` 的 package，它的目錄結構如下：

```text
.../pubspec.yaml
.../icons/heart.png
.../icons/1.5x/heart.png
.../icons/2.0x/heart.png
...etc.
```

To load the image, use:

然後載入 image, 使用：

<?code-excerpt "main.dart (PackageImage)"?>
```dart
return const AssetImage('icons/heart.png', package: 'my_icons');
```

Assets used by the package itself should also be fetched
using the `package` argument as above.

package 使用本身的 Assets 也需要加上 `package` 引數來獲取。

#### Bundling of package assets

#### 打包 assets

If the desired asset is specified in the `pubspec.yaml`
file of the package, it's bundled automatically with the
application. In particular, assets used by the package
itself must be specified in its `pubspec.yaml`.

如果期望的資原始檔被指定在 package 的 `pubspec.yaml` 檔案中，它會被自動打包到應用程式中。
特別是，package 本身使用的資源必須在 `pubspec.yaml` 中指定。

A package can also choose to have assets in its `lib/`
folder that are not specified in its `pubspec.yaml` file.
In this case, for those images to be bundled,
the application has to specify which ones to include in its
`pubspec.yaml`. For instance, a package named `fancy_backgrounds`
could have the following files:

package 也可以選擇在其 `lib/`
資料夾中包含未在 `pubspec.yaml` 檔案中宣告的資源。
在這種情況下，對於要打套件的圖片，
應用程式必須在 `pubspec.yaml` 中指定包含哪些圖像。 
例如，一個名為 `fancy_backgrounds` 的套件，
可能包含以下檔案：

```text
.../lib/backgrounds/background1.png
.../lib/backgrounds/background2.png
.../lib/backgrounds/background3.png
```

To include, say, the first image, the `pubspec.yaml` of the
application should specify it in the `assets` section:

總而言之，要包含第一張圖像，必須在 `pubspec.yaml` 的 `assets` 部分中宣告它：

```yaml
flutter:
  assets:
    - packages/fancy_backgrounds/backgrounds/background1.png
```

The `lib/` is implied,
so it should not be included in the asset path.

`lib/` 是隱含的，所以它不應該包含在資源路徑中。

If you are developing a package, to load an asset within the package, specify it in the `pubspec.yaml` of the package:

如果你正在開發 package，想要從 package 中載入資源，首先要在 `pubspec.yaml` 中定義：

```yaml
flutter:
  assets:
    - assets/images/
```

To load the image within your package, use:

在 package 中載入圖片，按以下方式：

```dart
return const AssetImage('packages/fancy_backgrounds/backgrounds/background1.png');
```

## Sharing assets with the underlying platform

## 平台共享 assets

Flutter assets are readily available to platform code
using the `AssetManager` on Android and `NSBundle` on iOS.

在不同平台讀取 Flutter assets，
Android 是透過 `AssetManager`，iOS 是 `NSBundle`。

### Loading Flutter assets in Android

### 在 Android 中載入 Flutter 資原始檔

On Android the assets are available through the
[`AssetManager`][] API.  The lookup key used in,
for instance [`openFd`][], is obtained from
`lookupKeyForAsset` on [`PluginRegistry.Registrar`][] or
`getLookupKeyForAsset` on [`FlutterView`][].
`PluginRegistry.Registrar` is available when developing a plugin
while `FlutterView` would be the choice when developing an
app including a platform view.

在 Android 平臺上，assets 透過 [`AssetManager`][] API 讀取。
透過 [`PluginRegistry.Registrar`][] 的 `lookupKeyForAsset` 方法，
或者 [`FlutterView`][] 的 `getLookupKeyForAsset` 方法來獲取檔案路徑，
然後 [`AssetManager`][] 的 [`openFd`][] 根據檔案路徑得到檔案描述符。
開發外掛時可以使用 `PluginRegistry.Registrar`，
而開發應用程式使用平臺視圖時，[`FlutterView`][] 是最好的選擇。

As an example, suppose you have specified the following
in your pubspec.yaml

舉個例子，假設你在 pubspec.yaml 中這樣指定：

```yaml
flutter:
  assets:
    - icons/heart.png
```

This reflects the following structure in your Flutter app.

在你的 Flutter 應用程式對應以下結構。

```text
.../pubspec.yaml
.../icons/heart.png
...etc.
```

To access `icons/heart.png` from your Java plugin code,
do the following:

想要在 Java 外掛中存取 `icons/heart.png`；

```java
AssetManager assetManager = registrar.context().getAssets();
String key = registrar.lookupKeyForAsset("icons/heart.png");
AssetFileDescriptor fd = assetManager.openFd(key);
```

### Loading Flutter assets in iOS

### 在 iOS 中載入 Flutter 資原始檔

On iOS the assets are available through the [`mainBundle`][].
The lookup key used in, for instance [`pathForResource:ofType:`][],
is obtained from `lookupKeyForAsset` or `lookupKeyForAsset:fromPackage:`
on [`FlutterPluginRegistrar`][], or `lookupKeyForAsset:` or
`lookupKeyForAsset:fromPackage:` on [`FlutterViewController`][].
`FlutterPluginRegistrar` is available when developing
a plugin while `FlutterViewController` would be the choice
when developing an app including a platform view.

在 iOS 平臺上，assets 資原始檔透過 [`mainBundle`][] 讀取。
透過 [`pathForResource:ofType:`][] 的 `lookupKeyForAsset` 
或者 `lookupKeyForAsset:fromPackage:` 方法獲取檔案路徑，
同樣，[`FlutterViewController`][] 的 `lookupKeyForAsset:` 
或者 `lookupKeyForAsset:fromPackage:` 方法也可以獲取檔案路徑。
開發外掛時可以使用 `FlutterPluginRegistrar`，
而開發應用程式使用平臺視圖時， `FlutterViewController` 是最好的選擇。

As an example, suppose you have the Flutter setting from above.

舉個例子，假設你的 Flutter 配置和上面一樣。

To access `icons/heart.png` from your Objective-C plugin code you
would do the following:

要在 Objective-C 外掛中存取 `icons/heart.png`：

```objective-c
NSString* key = [registrar lookupKeyForAsset:@"icons/heart.png"];
NSString* path = [[NSBundle mainBundle] pathForResource:key ofType:nil];
```

To access `icons/heart.png` from your Swift app you
would do the following:

要在 Swift 應用程式中存取 `icons/heart.png`：

```swift
let key = controller.lookupKey(forAsset: "icons/heart.png")
let mainBundle = Bundle.main
let path = mainBundle.path(forResource: key, ofType: nil)
```

For a more complete example, see the implementation of the
Flutter [`video_player` plugin][] on pub.dev.

這有一個更完整的例項可以理解 Flutter 的應用：
[`video_player` plugin][]。

The [`ios_platform_images`][] plugin on pub.dev wraps
up this logic in a convenient category. You fetch
an image as follows:

pub.dev 上的 [`ios_platform_images`][] plugin 將這些邏輯封裝成方便的類別。
它允許編寫：

**Objective-C:**
```objective-c
[UIImage flutterImageWithName:@"icons/heart.png"];
```

**Swift:**
```swift
UIImage.flutterImageNamed("icons/heart.png")
```

### Loading iOS images in Flutter

### 在 Flutter 中載入 iOS 的圖片

When implementing Flutter by
[adding it to an existing iOS app][add-to-app],
you might have images hosted in iOS that you
want to use in Flutter. To accomplish
that, use the [`ios_platform_images`][] plugin
available on pub.dev.

當你在 iOS 應用程式中新增 Flutter 時，
你可能希望在 Flutter 中使用 iOS 中的圖片。
為了實現這一點，
可以使用 pub.dev 上的 [`ios_platform_images`][] 外掛。

## Platform assets

## 平台 assets

There are other occasions to work with assets in the
platform projects directly. Below are two common cases
where assets are used before the Flutter framework is
loaded and running.

某些場景可以直接在平台專案中使用 assets。
以下是在 Flutter 框架載入並執行之前使用資源的兩種常見情況。

### Updating the app icon

### 更新桌面圖示

Updating a Flutter application's launch icon works
the same way as updating launch icons in native
Android or iOS applications.

更新你的 Flutter 應用程式啟動畫標，
和原生 Android 或 iOS 應用程式中更新啟動畫標的方法相同。

![Launch icon]({{site.url}}/assets/images/docs/assets-and-images/icon.png)

#### Android

In your Flutter project's root directory, navigate to
`.../android/app/src/main/res`. The various bitmap resource
folders such as `mipmap-hdpi` already contain placeholder
images named `ic_launcher.png`. Replace them with your
desired assets respecting the recommended icon size per
screen density as indicated by the [Android Developer Guide][].

在 Flutter 專案的根目錄中，導航到 `.../android/app/src/main/res` 路徑。
各種點陣圖資原始檔夾，比如 `mipmap-hdpi`，已包含佔位符圖像 `ic_launcher.png`。 
只需按照 [Android 開發者指南][Android Developer Guide] 中的說明，
將其替換為所需的資源，並遵守每種螢幕解析度的建議圖示大小標準。

![Android icon location]({{site.url}}/assets/images/docs/assets-and-images/android-icon-path.png)

{{site.alert.note}}

  If you rename the `.png` files, you must also update the
  corresponding name in your `AndroidManifest.xml`'s
  `<application>` tag's `android:icon` attribute.

  如果你重新命名了 `.png` 檔案，則還必須在 `AndroidManifest.xml` 
  中 `<application>` 標籤的 `android:icon` 屬性中更新名稱。

{{site.alert.end}}

#### iOS

In your Flutter project's root directory,
navigate to `.../ios/Runner`. The
`Assets.xcassets/AppIcon.appiconset` directory already contains
placeholder images. Replace them with the appropriately
sized images as indicated by their filename as dictated by the
Apple [Human Interface Guidelines][].
Keep the original file names.

在你的 Flutter 專案的根目錄中，導航到 `.../ios/Runner` 路徑。
該目錄中 `Assets.xcassets/AppIcon.appiconset`已經包含佔位符圖片，
只需將它們替換為適當大小的圖片，
並且根據 [iOS 開發指南][Human Interface Guidelines]，檔名稱保持不變。

![iOS icon location]({{site.url}}/assets/images/docs/assets-and-images/ios-icon-path.png)

### Updating the launch screen

### 更新啟動畫

<p align="center">
  <img src="/assets/images/docs/assets-and-images/launch-screen.png" alt="Launch screen" />
</p>

Flutter also uses native platform mechanisms to draw
transitional launch screens to your Flutter app while the
Flutter framework loads. This launch screen persists until
Flutter renders the first frame of your application.

在 Flutter 框架載入時，Flutter 會使用原生平臺機制繪製啟動頁。
此啟動頁將持續到 Flutter 渲染應用程式的第一幀。

{{site.alert.note}}

  This implies that if you don't call [`runApp()`][] in the
  `main()` function of your app (or more specifically,
  if you don't call [`FlutterView.render()`][] in response to
  [`PlatformDispatcher.onDrawFrame`][]),
  the launch screen persists forever.

  這意味著如果你不在應用程式的 `main()` 方法中呼叫 [`runApp()`][] 函式
  （或者更具體地說，如果你不呼叫 [`FlutterView.render()`][] 去響應 
  [`PlatformDispatcher.onDrawFrame`][] 的話， 啟動頁將永遠持續顯示。

{{site.alert.end}}

[`FlutterView.render()`]: {{site.api}}/flutter/dart-ui/FlutterView/render.html
[`PlatformDispatcher.onDrawFrame`]: {{site.api}}/flutter/dart-ui/PlatformDispatcher/onDrawFrame.html

#### Android

To add a launch screen (also known as "splash screen") to your
Flutter application, navigate to `.../android/app/src/main`.
In `res/drawable/launch_background.xml`,
use this [layer list drawable][] XML to customize
the look of your launch screen. The existing template provides
an example of adding an image to the middle of a white splash
screen in commented code. You can uncomment it or use other
[drawables][] to achieve the intended effect.

將啟動螢幕「splash screen」新增到你的 Flutter 應用程式， 
請導航至 `.../android/app/src/main` 路徑。
在 `res/drawable/launch_background.xml` 檔案中 ，透過使用
[圖層列表][layer list drawable]  XML 來實現自訂啟動頁。
現有範本提供了一個範例，用於將圖片新增到白色啟動頁的中間（註釋程式碼中）。
你也可以取消註釋使用 [可繪製物件資源][drawables] 來實現預期效果。

For more details, see
[Adding a splash screen to your Android app][].

更多詳細資訊，請檢視
[在 Android 應用中新增閃屏頁與啟動頁][Adding a splash screen to your Android app]。

#### iOS

To add an image to the center of your "splash screen",
navigate to `.../ios/Runner`.
In `Assets.xcassets/LaunchImage.imageset`,
drop in images named `LaunchImage.png`,
`LaunchImage@2x.png`, `LaunchImage@3x.png`.
If you use different filenames,
update the `Contents.json` file in the same directory.

將圖片新增到啟動螢幕「splash screen」的中心，請導航至 `.../ios/Runner` 路徑。
在 `Assets.xcassets/LaunchImage.imageset` ，拖入圖片，
並命名為 `LaunchImage.png`， `LaunchImage@2x.png`，`LaunchImage@3x.png`。 
如果你使用不同的檔名，
那你還必須更新同一目錄中的 `Contents.json` 檔案中對應的名稱。

You can also fully customize your launch screen storyboard
in Xcode by opening `.../ios/Runner.xcworkspace`.
Navigate to `Runner/Runner` in the Project Navigator and
drop in images by opening `Assets.xcassets` or do any
customization using the Interface Builder in
`LaunchScreen.storyboard`.

你也可以透過開啟 `.../ios/Runner.xcworkspace` ，完全自訂 storyboard。
在 Project Navigator 中導航到 `Runner/Runner` ，然後開啟 `Assets.xcassets` 拖入圖片，或者
在 `LaunchScreen.storyboard` 中使用 Interface Builder 進行自訂。

![Adding launch icons in Xcode]({{site.url}}/assets/images/docs/assets-and-images/ios-launchscreen-xcode.png){:width="100%"}

For more details, see
[Adding a splash screen to your iOS app][].

更多詳細資訊，請檢視 
[在 iOS 應用中新增閃屏頁與啟動頁][Adding a splash screen to your iOS app]。

[add-to-app]: {{site.url}}/add-to-app/ios
[Adding a splash screen to your Android app]: {{site.url}}/platform-integration/android/splash-screen
[Adding a splash screen to your iOS app]: {{site.url}}/platform-integration/ios/splash-screen
[`AssetBundle`]: {{site.api}}/flutter/services/AssetBundle-class.html
[`AssetImage`]: {{site.api}}/flutter/painting/AssetImage-class.html
[`DefaultAssetBundle`]: {{site.api}}/flutter/widgets/DefaultAssetBundle-class.html
[`ImageCache`]: {{site.api}}/flutter/painting/ImageCache-class.html
[`ImageStream`]: {{site.api}}/flutter/painting/ImageStream-class.html
[Android Developer Guide]: {{site.android-dev}}/training/multiscreen/screendensities
[`AssetManager`]: {{site.android-dev}}/reference/android/content/res/AssetManager
[device pixel ratio]: {{site.api}}/flutter/dart-ui/FlutterView/devicePixelRatio.html
[Device pixel ratio]: {{site.api}}/flutter/dart-ui/FlutterView/devicePixelRatio.html
[drawables]: {{site.android-dev}}/guide/topics/resources/drawable-resource
[`FlutterPluginRegistrar`]: {{site.api}}/objcdoc/Protocols/FlutterPluginRegistrar.html
[`FlutterView`]: {{site.api}}/javadoc/io/flutter/view/FlutterView.html
[`FlutterViewController`]: {{site.api}}/objcdoc/Classes/FlutterViewController.html
[Human Interface Guidelines]: {{site.apple-dev}}/design/human-interface-guidelines/app-icons
[`ios_platform_images`]: {{site.pub}}/packages/ios_platform_images
[layer list drawable]: {{site.android-dev}}/guide/topics/resources/drawable-resource#LayerList
[`mainBundle`]: {{site.apple-dev}}/documentation/foundation/nsbundle/1410786-mainbundle
[`openFd`]: {{site.android-dev}}/reference/android/content/res/AssetManager#openFd(java.lang.String)
[package]: {{site.url}}/packages-and-plugins/using-packages
[`pathForResource:ofType:`]: {{site.apple-dev}}/documentation/foundation/nsbundle/1410989-pathforresource
[`PluginRegistry.Registrar`]: {{site.api}}/javadoc/io/flutter/plugin/common/PluginRegistry.Registrar.html
[`pubspec.yaml`]: {{site.dart-site}}/tools/pub/pubspec
[`rootBundle`]: {{site.api}}/flutter/services/rootBundle.html
[`runApp()`]: {{site.api}}/flutter/widgets/runApp.html
[`video_player` plugin]: {{site.pub}}/packages/video_player
[MediaQueryData.size]: {{site.api}}/flutter/widgets/MediaQueryData/size.html
[MaterialApp]: {{site.api}}/flutter/material/MaterialApp-class.html
[CupertinoApp]: {{site.api}}/flutter/cupertino/CupertinoApp-class.html
