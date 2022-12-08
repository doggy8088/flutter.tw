---
title: "Binding to native Android code using dart:ffi"
title: "Android 上使用 dart:ffi 呼叫原生代碼"
description: "To use C code in your Flutter program, use the dart:ffi library."
description: "在你的 Flutter 工程中，透過 dart:ffi 來使用 C 語言程式碼"
tags: "平台整合"
keywords: "dartffi"
---

<?code-excerpt path-base="development/platform_integration"?>

Flutter mobile and desktop apps can use the
[dart:ffi][] library to call native C APIs.
_FFI_ stands for [_foreign function interface._][FFI]
Other terms for similar functionality include
_native interface_ and _language bindings._

Flutter 移動版可以使用 [dart:ffi][] 庫來呼叫本地的 C API。
**FFI** 代表 [**外部功能介面**][FFI]。
類似功能的其他術語包括**本地介面**和**語言繫結**。

{{site.alert.note}}

  This page describes using the `dart:ffi` library
  in Android apps. For information on iOS, see
  [Binding to native iOS code using dart:ffi][ios-ffi].
  For information in macOS, see
  [Binding to native macOS code using dart:ffi][macos-ffi].
  This feature is not yet supported for web plugins.

  本文描述的是在 Android 應用中使用 `dart:ffi` 庫。
  你可以閱讀 [在 iOS 中使用 dart:ffi 呼叫原生代碼][ios-ffi]
  或 [在 macOS 中使用 dart:ffi 呼叫原生代碼][macos-ffi]。
  Web 外掛暫不支援呼叫原生代碼。

{{site.alert.end}}


[ios-ffi]: {{site.url}}/development/platform-integration/ios/c-interop
[dart:ffi]: {{site.dart.api}}/dev/dart-ffi/dart-ffi-library.html
[macos-ffi]: {{site.url}}/development/platform-integration/macos/c-interop
[FFI]: https://en.wikipedia.org/wiki/Foreign_function_interface

Before your library or program can use the FFI library
to bind to native code, you must ensure that the
native code is loaded and its symbols are visible to Dart.
This page focuses on compiling, packaging,
and loading Android native code within a Flutter plugin or app.

你必須首先確保原生代碼已載入，並且其符號對 Dart 可見，
然後才能在庫或程式使用 FFI 庫繫結原生代碼。
本頁主要介紹如何在 Flutter 外掛或應用程式中編譯、打套件和載入原生代碼。

This tutorial demonstrates how to bundle C/C++
sources in a Flutter plugin and bind to them using
the Dart FFI library on both Android and iOS.
In this walkthrough, you'll create a C function
that implements 32-bit addition and then
exposes it through a Dart plugin named "native_add".

本課程示範瞭如何在 Flutter 外掛中捆綁 C/C++ 原始碼，
並使用 Android 和 iOS 上的 Dart FFI 庫繫結它們。
在本範例中，你將建立一個實現 32 位的加法 C 函式，
然後透過名為 "native_add" 的 Dart 外掛暴露它。

### Dynamic vs static linking

### 動態連結 vs 靜態連結

A native library can be linked into an app either
dynamically or statically. A statically linked library
is embedded into the app's executable image,
and is loaded when the app starts.

本地庫可以動態或靜態地連結到應用程式中。
一個靜態連結庫會被嵌入到應用程式的可執行映像中，
並在應用程式啟動時載入。

Symbols from a statically linked library can be
loaded using [`DynamicLibrary.executable`][] or
[`DynamicLibrary.process`][].

靜態連結中的符號可以使用 [`DynamicLibrary.executable`][]
或 [`DynamicLibrary.process`][] 來載入。

A dynamically linked library, by contrast, is distributed
in a separate file or folder within the app,
and loaded on-demand. On Android, a dynamically
linked library is distributed as a set of `.so` (ELF)
files, one for each architecture.

相比之下，動態連結庫則分佈在應用程式中的單獨的檔案或資料夾中，
並按需載入。在 Android 上，動態連結庫作為一組
`.so`（ELF 可執行與可連結格式）檔案分發，每個架構各有一個。

A dynamically linked library can be loaded into
Dart via [`DynamicLibrary.open`][].

動態連結庫在 Dart 中可以透過 [`DynamicLibrary.open`][] 載入。

API documentation is available from the Dart dev channel:
[Dart API reference documentation][].

Dart dev 頻道中的 API 已經可用：
[Dart API 參考文件][Dart API reference documentation].

[Dart API reference documentation]: {{site.dart.api}}/dev/
[`DynamicLibrary.executable`]: {{site.dart.api}}/dev/dart-ffi/DynamicLibrary/DynamicLibrary.executable.html
[`DynamicLibrary.open`]: {{site.dart.api}}/dev/dart-ffi/DynamicLibrary/DynamicLibrary.open.html
[`DynamicLibrary.process`]: {{site.dart.api}}/dev/dart-ffi/DynamicLibrary/DynamicLibrary.process.html

## Step 1: Create a plugin

## 步驟 1：建立外掛

If you already have a plugin, skip this step.

如果你已經有一個外掛，跳過這步。

To create a plugin called "native_add",
do the following:

如果要建立一個名為 "native_add" 的外掛，
你需要這麼做：

```terminal
$ flutter create --platforms=android,ios --template=plugin native_add
$ cd native_add
```

{{ site.alert.note }}

  You can exclude platforms from --platforms that you don't want
  to build to. However, you need to include the platform of 
  the device you are testing on.

  你可以使用 --platforms 來排除你不需要的平台。
  但是，你仍需要包含測試裝置所需的平台。

{{ site.alert.end }}

## Step 2: Add C/C++ sources

## 步驟 2：新增 C/C++ 原始碼

You need to inform the Android build system about
the native code so the code can be compiled
and linked appropriately into the final application.

你需要讓 Android 和 iOS 建構系統知道原生代碼的存在，
以便程式碼可以被編譯並連結到最終的應用程式中。

You can add Android-specific sources
to the `android` folder and modify `CMakeLists.txt`
appropriately.
Also, Gradle allows you to point to the `ios` folder,
if that helps, but it's not required to use the same
sources for both iOS and Android;

你可以將特定於 Android 的原始碼新增到 `android` 資料夾
並修改 `CMakeLists.txt` 檔案。
同時，你可以在 Gradle 中指向 `ios` 資料夾，這樣的話就
可以為 iOS 和 Android 設定不同的資源。

The FFI library can only bind against C symbols,
so in C++ these symbols must be marked `extern C`.
You should also add attributes to indicate that the
symbols are referenced from Dart,
to prevent the linker from discarding the symbols
during link-time optimization.

FFI 庫只能與 C 符號繫結，因此在 C++ 中，
這些符號新增 `extern C` 標記。
還應該新增屬性來表明符號是需要被 Dart 參考的，
以防止連結器在最佳化連結時會丟棄符號。

On Android, you need to create a `CMakeLists.txt` file
to define how the sources should be compiled and point
Gradle to it. From the root of your project directory,
use the following instructions:

在 Android 中，你需要建立一個 `CMakeLists.txt` 
檔案用來定義如何編譯原始檔，同時告訴 Gradle 如何去定位它們。
在專案根目錄下，執行如下程式碼：

```bash
cat > android/CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.4.1)  # for example

add_library( native_add

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             ../ios/Classes/native_add.cpp )
EOF
```

Finally, add an `externalNativeBuild` section to
`android/build.gradle`. For example:

最後，新增一個 `externalNativeBuild` 到你的
`android/build.gradle` 檔案中。範例如下：

```nocode
android {
  // ...
  externalNativeBuild {
    // Encapsulates your CMake build configurations.
    cmake {
      // Provides a relative path to your CMake build script.
      path "CMakeLists.txt"
    }
  }
  // ...
}
```

## Step 3: Load the code using the FFI library

## 步驟 3：在 FFI 庫中讀取程式碼

In this example, you can add the following code to
`lib/native_add.dart`. However the location of the
Dart binding code is not important.

在範例中，你需要新增如下的程式碼到 `lib/native_add.dart`。
但是，Dart 在何處進行程式碼繫結並不重要。

First, you must create a `DynamicLibrary` handle to
the native code. The following example shows
how to create a handle for an iOS app OR an Android app:

首先，你需要建立一個 `DynamicLibrary` 來處理原生代碼。
下面的例子為你展示瞭如何在 iOS 和 Android 上操作：

<?code-excerpt "lib/c_interop.dart (DynamicLibrary)"?>
```dart
import 'dart:ffi'; // For FFI
import 'dart:io'; // For Platform.isX

final DynamicLibrary nativeAddLib = Platform.isAndroid
    ? DynamicLibrary.open('libnative_add.so')
    : DynamicLibrary.process();
```

Note that on Android the native library is named
in `CMakeLists.txt` (see above),
but on iOS it takes the plugin's name.

請注意，在 Android 上，
本地庫的名稱是定義在 `CMakeLists.txt` 中的（見上文），
但在 iOS 上，它將使用外掛的名稱。

With a handle to the enclosing library,
you can resolve the `native_add` symbol:

你可以透過使用庫的控制代碼來解析 `native_add` 符號：

<?code-excerpt "lib/c_interop.dart (NativeAdd)"?>
```dart
final int Function(int x, int y) nativeAdd = nativeAddLib
    .lookup<NativeFunction<Int32 Function(Int32, Int32)>>('native_add')
    .asFunction();
```

Finally, you can call it. To demonstrate this within
the auto-generated "example" app (`example/lib/main.dart`):

現在，你可以呼叫它了。在自動產生的 "example" 專案
（`example/lib/main.dart`）中示範它。

```nocode
// Inside of _MyAppState.build:
        body: Center(
          child: Text('1 + 2 == ${nativeAdd(1, 2)}'),
        ),
```

## Other use cases

## 其他的使用案例

### Platform library

#### 平台庫

To link against a platform library,
use the following instructions:

要連結到平台庫，請按照如下說明：

 1. Find the desired library in the [Android NDK Native APIs][]
    list in the Android docs. This lists stable native APIs.

    在 Android 文件的 [Android NDK Native APIs][] 列表中找到所需的函式庫。
    它列出了穩定的本地 API。

 1. Load the library using [`DynamicLibrary.open`][].
    For example, to load OpenGL ES (v3):

    使用 [`DynamicLibrary.open`][] 載入庫。
    範例：載入 OpenGL ES (v3)：

    <!-- skip -->
    ```dart
    DynamicLibrary.open('libGLES_v3.so');
    ```

You might need to update the Android manifest
file of the app or plugin if indicated by
the documentation.

如果文件中有說明，
你還需要根據說明更新 Android 應用程式或外掛的清單檔案。

[Android NDK Native APIs]: {{site.android-dev}}/ndk/guides/stable_apis

#### First-party library

#### 第一方庫

The process for including native code in source
code or binary form is the same for an app or
plugin.

對於應用程式或外掛，以原始碼或二進位制形式包含本機程式碼的過程是相同的。

#### Open-source third-party

#### 開源三方庫

Follow the [Add C and C++ code to your project][]
instructions in the Android docs to
add native code and support for the native
code toolchain (either CMake or `ndk-build`).

遵循 Android 文件中的
[新增 C 和 C++ 程式碼到專案][Add C and C++ code to your project]
來新增原生代碼和對原生代碼工具鏈的支援（CMake 或 `ndk-build`）。

[Add C and C++ code to your project]: {{site.android-dev}}/studio/projects/add-native-code

#### Closed-source third-party library

#### 閉源三方庫

To create a Flutter plugin that includes Dart
source code, but distribute the C/C++ library
in binary form, use the following instructions:

要建立包含 Dart 原始碼，但以二進位制形式分發 C/C++ 庫的 Flutter 外掛，
請按照如下說明：

1. Open the `android/build.gradle` file for your
   project.

   開啟你專案的 `android/build.gradle` 檔案。

1. Add the AAR artifact as a dependency.
   **Don't** include the artifact in your
   Flutter package. Instead, it should be
   downloaded from a repository, such as
   JCenter.

   新增 aar 工件新增為依賴。
   **不要**在你的 Flutter package 中匯入工件。
   對應的，它需要在一個儲存庫中下載，比如 JCenter。

## Android APK size (shared object compression)

## Android APK 尺寸（共享物件壓縮）

[Android guidelines][] in general recommend
distributing native shared objects uncompressed
because that actually saves on device space.
Shared objects can be directly loaded from the APK
instead of unpacking them on device into a
temporary location and then loading.
APKs are additionally packed in transit&mdash;that's
why you should be looking at download size.

[Android 指南][Android guidelines] 通常建議分發未壓縮的本地共享物件，
因為這種做法實際上可以節省裝置空間。
共享物件可以直接從 APK 載入，
而不是將它們解壓到裝置上的臨時位置然後再載入。
APK 是在傳輸過程中額外打套件的 -
這就是為什麼你應該檢視下載的檔案尺寸。

Flutter APKs by default don't follow these guidelines
and compress `libflutter.so` and `libapp.so`&mdash;this
leads to smaller APK size but larger on device size.

Flutter APK 檔案預設情況下
不遵循這些指導原則來壓縮 `libflutter.so` 和 `libapp.so`，
這會導致 APK 體積更小，但在裝置上體積更大。

Shared objects from third parties can change this default
setting with `android:extractNativeLibs="true"` in their
`AndroidManifest.xml` and stop the compression of `libflutter.so`,
`libapp.so`, and any user-added shared objects.
To re-enable compression, override the setting in
`your_app_name/android/app/src/main/AndroidManifest.xml`
in the following way.

來自第三方的共享庫可以使用其 `AndroidManifest.xml`
中的 `android:extractNativeLibs="true"` 更改此預設設定，
來停止壓縮 `libflutter.so`、`libapp.so` 和任何使用者新增的共享庫。
要重新啟用壓縮，請按照如下方式重寫你的
`your_app_name/android/app/src/main/AndroidManifest.xml` 檔案。

```diff
@@ -1,5 +1,6 @@
 <manifest xmlns:android="http://schemas.android.com/apk/res/android"
-    package="com.example.your_app_name">
+    xmlns:tools="http://schemas.android.com/tools"
+    package="com.example.your_app_name" >
     <!-- io.flutter.app.FlutterApplication is an android.app.Application that
          calls FlutterMain.startInitialization(this); in its onCreate method.
          In most cases you can leave this as-is, but you if you want to provide
          additional functionality it is fine to subclass or reimplement
          FlutterApplication and put your custom class here. -->
@@ -8,7 +9,9 @@
     <application
         android:name="io.flutter.app.FlutterApplication"
         android:label="your_app_name"
-        android:icon="@mipmap/ic_launcher">
+        android:icon="@mipmap/ic_launcher"
+        android:extractNativeLibs="true"
+        tools:replace="android:extractNativeLibs">
```

[Android guidelines]: {{site.android-dev}}/topic/performance/reduce-apk-size#extract-false

