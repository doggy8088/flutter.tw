---
title: Deprecated Splash Screen API Migration
title: 從已棄用的閃屏頁 API 遷移
description: How to migrate from Manifest/Activity defined splash screen.
description: 如何將 Manifest/Activity 定義的閃屏頁進行遷移。
---

Prior to Flutter 2.5, Flutter apps could add a splash
screen by defining it within the metadata of their application manifest file
(`AndroidManifest.xml`), by implementing [`provideSplashScreen`][] within
their [`FlutterActivity`][], or both. This would display momentarily in between
the time after the Android launch screen is shown and when Flutter has
drawn the first frame. This approach is now deprecated as of Flutter 2.5.
Flutter now automatically keeps the Android launch screen displayed
until it draws the first frame.

在 Flutter 2.5 版本之前，Flutter 應用程式可以透過定義 manifest 檔案 (`AndroidManifest.xml`) 
中的元資料，或在其 [`FlutterActivity`][] 中實現 [`provideSplashScreen`][]，
再或者兩者兼顧使用，來新增閃屏頁。
閃屏頁會在 Android 啟動頁顯示後，Flutter 首幀繪製前短暫顯示。
從 Flutter 2.5 版本起，這種方式已被棄用。
Flutter 現在會自動保持顯示 Android 啟動頁，直至首幀繪製。

To migrate from defining a custom splash screen to just defining a custom
launch screen for your application, follow the steps that correspond
to how your application's custom splash screen was defined
prior to the 2.5 release.

請按照 Flutter 2.5 版本之前自訂應用程式閃屏頁相應的操作步驟，
將自訂的閃屏頁遷移到只用自訂應用程式的啟動頁。

**Custom splash screen defined in [`FlutterActivity`][]**

**在 [`FlutterActivity`][] 中自訂閃屏頁**

1. Locate your application's implementation of `provideSplashScreen()`
   within its `FlutterActivity` and **delete it**. This implementation should involve
   the construction of your application's custom splash screen
   as a `Drawable`. For example:

   在應用程式的 `FlutterActivity` 中找到 `provideSplashScreen()` 方法的實現並 **將其刪除**。
   該方法應該包括了將應用程式的自訂閃屏頁建構成一個 `Drawable` 的實現。
   例如：

   ```java
   @Override
   public SplashScreen provideSplashScreen() {
       // ...
       return new DrawableSplashScreen(
           new SomeDrawable(
               ContextCompat.getDrawable(this, R.some_splash_screen)));
   }
   ```

2. Use the steps in the section directly following to ensure that your
   `Drawable` splash screen (`R.some_splash_screen` in the previous example)
   is properly configured as your application's custom launch screen.

   請按照接下來章節中的步驟，確保你的 `Drawable` 閃屏頁
   （上個步驟範例中的 `R.some_splash_screen`）
   已正確配置為應用程式的自訂啟動頁。

**Custom splash screen defined in Manifest**

**在 Manifest 中自訂閃屏頁**

1. Locate your application's `AndroidManifest.xml` file.
   Within this file, find the `activity` element.
   Within this element, identify the `android:theme` attribute
   and the `meta-data` element that defines
   a splash screen as an
   `io.flutter.embedding.android.SplashScreenDrawable`,
   and update it. For example:

   在應用程式的 `AndroidManifest.xml` 檔案中找到 `activity` 元素。
   在此元素中，找到 `android:theme` 屬性以及
   定義閃屏頁為 `io.flutter.embedding.android.SplashScreenDrawable` 
   的 `meta-data` 元素，以便在後續的步驟中進行更新。
   例如：

   ```xml
   <activity
       // ...
       android:theme="@style/SomeTheme">
     // ...
     <meta-data
         android:name="io.flutter.embedding.android.SplashScreenDrawable"
         android:resource="@drawable/some_splash_screen"
         />
   </activity>
   ```

2. If the `android:theme` attribute isn't specified, add the attribute and
   [define a launch theme][] for your application's launch screen.

   如果未指定 `android:theme` 屬性，請新增該屬性
   併為應用程式的啟動頁 [定義啟動主題][define a launch theme]

3. Delete the `meta-data` element, as Flutter no longer
   uses that, but it can cause a crash.

   刪除 `meta-data` 元素，因為 Flutter 不再使用該元素，
   如果保留它可能會導致崩潰。

4. Locate the definition of the theme specified by the `android:theme` attribute
   within your application's `style` resources. This theme specifies the
   launch theme of your application. Ensure that the `style` attribute configures the
   `android:windowBackground` attribute with your custom splash screen. For example:

   在應用程式的 `style` 資源中找到由 `android:theme` 屬性指定的主題。
   該主題指定了應用程式的啟動主題。
   請確保 `style` 將 `android:windowBackground` 屬性配置為你的自訂閃屏頁。
   例如：

   ```xml
   <resources>
       <style
           name="SomeTheme"
           // ...
           >
           <!-- Show a splash screen on the activity. Automatically removed when
                Flutter draws its first frame -->
           <item name="android:windowBackground">@drawable/some_splash_screen</item>
       </style>
   </resources>
   ```

[`provideSplashScreen`]: {{site.api}}/javadoc/io/flutter/embedding/android/SplashScreenProvider.html#provideSplashScreen--
[`FlutterActivity`]: {{site.api}}/javadoc/io/flutter/embedding/android/FlutterActivity.html
[define a launch theme]:  {{site.url}}/platform-integration/android/splash-screen
