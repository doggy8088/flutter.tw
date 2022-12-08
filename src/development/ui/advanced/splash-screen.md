---
title: Adding a splash screen to your mobile app
title: 嚮應用新增閃屏頁
short-title: Splash screens
short-title: 閃屏頁
description: Learn how to add a splash screen to your mobile app.
description: 瞭解如何向你的應用新增一個閃屏頁。
tags: 使用者介面,Flutter UI
keywords: 閃屏頁,啟動頁,Loading圖,應用商店
---

<img src='/assets/images/docs/development/ui/splash-screen/android-splash-screen/splash-screens_header.png'
class="mw-100" alt="Add Splash Screen Header">

Splash screens (also known as launch screens)
provide a simple initial experience while your
mobile app loads. They set the stage for your
application, while allowing time for the app
engine to load and your app to initialize.
This guide teaches you how to use splash screens
appropriately on iOS and Android.

閃屏頁（也稱為啟動頁）是你的應用在啟動時給使用者的第一印象。
它們就像是你的應用的基礎，同時允許你在它展示的時間裡，
載入你的引擎和初始化你的應用。
本指南將展示如何在 Flutter 編寫的移動應用中恰當地使用閃屏頁。

{{site.alert.note}}

  For more information on implementing splash screens
  on web and desktop platforms, see the
  [Customizing web app initialization guide][].

  有關在 Web 和桌面平臺上實現啟動畫面 (閃屏頁) 的更多資訊，請參閱 
  [自訂 Web 應用程式初始化指南][Customizing web app initialization guide]。

{{site.alert.end}}

## iOS launch screen

## iOS 啟動頁

All apps submitted to the Apple App Store
[must use an Xcode storyboard][] to
provide the app's launch screen.

所有應用在交付到 Apple 應用商店之前
[必須使用 Xcode storyboard][must use an Xcode storyboard]
以提供應用啟動頁面。

The default Flutter template includes an Xcode
storyboard named `LaunchScreen.storyboard`
that can be customized as you see fit with
your own assets. By default,
the storyboard displays a blank image,
but you can change this. To do so,
open the Flutter app's Xcode project
by typing `open ios/Runner.xcworkspace`
from the root of your app directory.
Then select `Runner/Assets.xcassets`
from the Project Navigator and
drop in the desired images to the `LaunchImage` image set.

預設的 Flutter 範本包括一個名為
`LaunchScreen.storyboard` 的 Xcode storyboard，
可以根據您的選擇進行客製你自己的資源。
預設情況下，storyboard 將顯示空白影象，但你可以修改它。
在專案根目錄下執行 `open ios/Runner.xcworkspace` 
開啟 Flutter 應用程式的 Xcode 專案。
然後從專案導航器中選擇 `Runner/Assets.xcassets`，
並將所需影象拖拽至 `LaunchImage` 影象集中。

Apple provides detailed guidance for launch screens as
part of the [Human Interface Guidelines][].

Apple 在 [人機介面指南][Human Interface Guidelines]
部分中為釋出啟動頁提供了詳細的指南。

## Android launch screen

## Android 啟動頁

{{site.alert.warning}}

  If you are experiencing a crash from implementing a splash screen, you
  might need to migrate your code. See detailed instructions in the
  [Deprecated Splash Screen API Migration guide][].

  如果你在實現閃屏頁的時候遇到崩潰的情況，你可能需要遷移一下程式碼了。
  請在 [這個文件][Deprecated Splash Screen API Migration guide] 裡瞭解更多。

{{site.alert.end}}

In Android, there are two separate screens that you can control:
a _launch screen_ shown while your Android app initializes,
and a _splash screen_ that displays while the Flutter experience
initializes.

在 Android 中，你有兩個可以分開控制的頁面：
在 Android 應用初始化時的 **啟動頁**，
以及在 Flutter 初始化時的 **閃屏頁**。

{{site.alert.note}}

  As of Flutter 2.5, the launch and splash screens have been
  consolidated—Flutter now only implements the Android launch screen,
  which is displayed until the framework draws the first frame.
  This launch screen can act as both an Android launch screen and an
  Android splash screen via customization, and thus, is referred to
  as both terms. For example of such customization, see the
  [Android splash screen sample app][].

  從 Flutter 2.5 開始，啟動螢幕和閃屏頁已經被合併。
  Flutter 現在只會實現 Android 啟動螢幕，
  它會一直顯示到框架繪製的第一幀。

  If, prior to 2.5, you used `flutter create` to create an app,
  and you run the app on 2.5 or later, it can cause the app to crash.
  For more info, see the [Deprecated Splash Screen API Migration guide][].

  如果在 2.5 版本之前使用 `flutter create` 命令建立了應用，
  並且在 2.5 或 2.5 以上的版本執行這個應用，這將會導致應用崩潰。
  更多詳細資訊，請參考文件
  [已棄用的閃屏頁 API 遷移][Deprecated Splash Screen API Migration guide]。

{{site.alert.end}}

{{site.alert.note}}
  For apps that embed one or more Flutter screens within an
  existing Android app, consider
  [pre-warming a `FlutterEngine`][] and reusing the
  same engine throughout your app to minimize wait
  time associated with initialization of the Flutter engine.

  對於集成了多個 Flutter 內容的 Android 應用，可以考慮
  [預熱 `FlutterEngine`][pre-warming a `FlutterEngine`]
  以及在整個應用中複用同一個 Flutter 引擎，
  以減少初始化的等待時間。

{{site.alert.end}}

### Initializing the app

### 應用初始化

Every Android app requires initialization time while the
operating system sets up the app's process.
Android provides the concept of a [launch screen][] to
display a `Drawable` while the app is initializing.

所有 Android 應用在作業系統準備應用處理序時都需要一定的初始化時間。
因此 Android 提供了 [啟動介面][launch screen] 的概念，
在應用初始化的時候顯示 `Drawable`。

The default Flutter project template includes a definition
of a launch theme and a launch background. You can customize
this by editing `styles.xml`, where you can define a theme
whose `windowBackground` is set to the
`Drawable` that should be displayed as the launch screen.

預設的 Flutter 專案範本定義了啟動主題和啟動背景。
你可以在 `styles.xml` 中自訂一個主題，
將一個 `Drawable` 配置給該主題的 `windowBackground`，
它將作為啟動頁被展示。

```xml
<style name="LaunchTheme" parent="@android:style/Theme.Black.NoTitleBar">
    <item name="android:windowBackground">@drawable/launch_background</item>
</style>
```

### Define a normal theme

### 定義一個普通主題

In addition, `style.xml` defines a _normal theme_
to be applied to `FlutterActivity` after the launch
screen is gone. The normal theme background only shows
for a very brief moment after the splash screen disappears,
and during orientation change and `Activity` restoration.
Therefore, it's recommended that the normal theme use a
solid background color that looks similar to the primary
background color of the Flutter UI.

此外，在 `styles.xml` 中定義一個 **普通主題**，
當啟動頁消失後，它會應用在 `FlutterActivity` 上。
普通主題的背景僅僅展示非常短暫的時間，
例如，當啟動頁消失後、裝置方向改變或者 `Activity` 恢復期間。
因此建議普通主題的背景顏色使用與 Flutter UI 主要背景顏色相似的純色。

```xml
<style name="NormalTheme" parent="@android:style/Theme.Black.NoTitleBar">
    <item name="android:windowBackground">@drawable/normal_background</item>
</style>
```

### Set up the FlutterActivity in AndroidManifest.xml

### 在 AndroidManifest.xml 中配置 FlutterActivity

In `AndroidManifest.xml`, set the `theme` of
`FlutterActivity` to the launch theme. Then,
add a metadata element to the desired `FlutterActivity`
to instruct Flutter to switch from the launch theme
to the normal theme at the appropriate time.

在 `AndroidManifest.xml` 中，
將 `FlutterActivity` 的 `theme` 設定為啟動主題，
將元資料元素新增到所需的 `FlutterActivity`，
以知會 Flutter 在適當的時機從啟動主題切換到普通主題。

```xml
<activity
    android:name=".MyActivity"
    android:theme="@style/LaunchTheme"
    // ...
    >
    <meta-data
        android:name="io.flutter.embedding.android.NormalTheme"
        android:resource="@style/NormalTheme"
        />
    <intent-filter>
        <action android:name="android.intent.action.MAIN"/>
        <category android:name="android.intent.category.LAUNCHER"/>
    </intent-filter>
</activity>
```

The Android app now displays the desired launch screen
while the app initializes.

如此一來，Android 應用程式就會在在初始化時展示對應的啟動頁面。

### Android S

See [Android Splash Screens][] first on how to configure your launch screen on
Android S.

請先檢視 [Android 閃屏頁面][Android Splash Screens]
瞭解如何在 Android S 上配置閃屏頁。

Make sure neither `io.flutter.embedding.android.SplashScreenDrawable` is set in
your manifest, nor is `provideSplashScreen` implemented, as these APIs are
deprecated. Doing so will cause the Android launch screen to fade smoothly into
the Flutter when the app is launched and you may experience a crash.

確保 `io.flutter.embedding.android.SplashScreenDrawable`
未在 manifest 中設定，且 `provideSplashScreen` 也沒有具體實現，
這些 API 已被廢棄。
如此一來 Android 的閃屏頁可以在應用啟動時平滑過渡到 Flutter。

Some apps may want to continue showing the last frame of the Android launch
screen in Flutter. For example, this preserves the illusion of a single frame
while additional loading continues in Dart. To achieve this, the following
Android APIs may be helpful:

某些應用可能希望在 Flutter 中繼續顯示 Android 閃屏頁的最後一幀。
例如，保持一幀的展示，同時 Dart 繼續載入其他內容。
想達到這樣的效果，以下 API 可能有幫助：

{% samplecode android-splash-alignment %}
{% sample Java %}
<!--code-excerpt "MainActivity.java" title-->
```java
import android.os.Build;
import android.os.Bundle;
import android.window.SplashScreenView;
import androidx.core.view.WindowCompat;
import io.flutter.embedding.android.FlutterActivity;

public class MainActivity extends FlutterActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // Aligns the Flutter view vertically with the window.
        WindowCompat.setDecorFitsSystemWindows(getWindow(), false);

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
            // Disable the Android splash screen fade out animation to avoid
            // a flicker before the similar frame is drawn in Flutter.
            getSplashScreen()
                .setOnExitAnimationListener(
                    (SplashScreenView splashScreenView) -> {
                        splashScreenView.remove();
                    });
        }

        super.onCreate(savedInstanceState);
    }
}
```
{% sample Kotlin %}
<!--code-excerpt "MainActivity.kt" title-->
```kotlin
import android.os.Build
import android.os.Bundle
import androidx.core.view.WindowCompat
import io.flutter.embedding.android.FlutterActivity

class MainActivity : FlutterActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    // Aligns the Flutter view vertically with the window.
    WindowCompat.setDecorFitsSystemWindows(getWindow(), false)

    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
      // Disable the Android splash screen fade out animation to avoid
      // a flicker before the similar frame is drawn in Flutter.
      splashScreen.setOnExitAnimationListener { splashScreenView -> splashScreenView.remove() }
    }

    super.onCreate(savedInstanceState)
  }
}
```
{% endsamplecode %}

Then, you can reimplement the first frame in Flutter that shows elements of your
Android launch screen in the same positions on screen.
For an example of this, see the [Android splash screen sample app][].

然後你可以重新實現 Flutter 的第一幀，將元素擺放在與 Android 閃屏頁相同的位置。
關於這個的範例，請參考 [Android 閃屏頁範例應用][Android splash screen sample app]。

[Android Splash Screens]: {{site.android-dev}}/about/versions/12/features/splash-screen
[launch screen]: {{site.android-dev}}/topic/performance/vitals/launch-time#themed
[pre-warming a `FlutterEngine`]: {{site.url}}/development/add-to-app/android/add-flutter-fragment#using-a-pre-warmed-flutterengine
[`provideSplashScreen`]: {{site.api}}/javadoc/io/flutter/embedding/android/SplashScreenProvider.html#provideSplashScreen--
[must use an Xcode storyboard]: {{site.apple-dev}}/news/?id=03042020b
[Human Interface Guidelines]: {{site.apple-dev}}/design/human-interface-guidelines/ios/visual-design/launch-screen/
[Android splash screen sample app]: {{site.github}}/flutter/samples/tree/main/android_splash_screen
[Deprecated Splash Screen API Migration guide]: {{site.url}}/development/platform-integration/android/splash-screen-migration
[Customizing web app initialization guide]: {{site.url}}/development/platform-integration/web/initialization
