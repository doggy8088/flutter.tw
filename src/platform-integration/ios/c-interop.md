---
title: "Binding to native iOS code using dart:ffi"
title: "在 iOS 中使用 dart:ffi 呼叫原生代碼"
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
  in iOS apps. For information on Android, see
  [Binding to native Android code using dart:ffi][android-ffi].
  For information in macOS, see
  [Binding to native macOS code using dart:ffi][macos-ffi].
  This feature is not yet supported for web plugins.

  本文描述的是在 iOS 應用中使用 `dart:ffi` 庫。
  你可以閱讀 [在 Android 中使用 dart:ffi 呼叫原生代碼][android-ffi]
  或 [在 macOS 中使用 dart:ffi 呼叫原生代碼][macos-ffi]。
  Web 外掛暫不支援呼叫原生代碼。

{{site.alert.end}}

[android-ffi]: {{site.url}}/platform-integration/android/c-interop
[macos-ffi]: {{site.url}}/platform-integration/macos/c-interop
[dart:ffi]: {{site.dart.api}}/dev/dart-ffi/dart-ffi-library.html
[FFI]: https://en.wikipedia.org/wiki/Foreign_function_interface

Before your library or program can use the FFI library
to bind to native code, you must ensure that the
native code is loaded and its symbols are visible to Dart.
This page focuses on compiling, packaging,
and loading iOS native code within a Flutter plugin or app.

你必須首先確保原生代碼已載入，並且其符號對 Dart 可見，
然後才能在庫或程式使用 FFI 庫繫結原生代碼。
本頁主要介紹如何在 Flutter 外掛或應用程式中編譯、打套件和載入 iOS 原生程式碼。

This tutorial demonstrates how to bundle C/C++
sources in a Flutter plugin and bind to them using
the Dart FFI library on iOS.

本課程示範瞭如何在 Flutter 外掛中捆綁 C/C++ 原始碼，
並在 iOS 上使用 Dart FFI 庫繫結和使用。

In this walkthrough, you'll create a C function
that implements 32-bit addition and then
exposes it through a Dart plugin named "native_add".

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
loaded using `DynamicLibrary.executable` or
`DynamicLibrary.process`.

靜態連結中的符號可以使用 `DynamicLibrary.executable`
或 `DynamicLibrary.process` 來載入.

A dynamically linked library, by contrast, is distributed
in a separate file or folder within the app,
and loaded on-demand. On iOS, the dynamically linked
library is distributed as a `.framework` folder.

相比之下，動態連結庫則分佈在應用程式中的單獨的檔案或資料夾中，
並按需載入。在 iOS 上，它是作為 `.framework` 資料夾分發的。

A dynamically linked library can be loaded into
Dart using `DynamicLibrary.open`.

動態連結庫在 Dart 中可以透過 `DynamicLibrary.open` 載入。

API documentation is available from the Dart dev channel:
[Dart API reference documentation][].

Dart dev 頻道中的 API 已經可用：
[Dart API 參考文件][Dart API reference documentation].

[Dart API reference documentation]: {{site.dart.api}}/dev/

## Create an FFI plugin

## 建立 FFI 外掛

To create an FFI plugin called "native_add",
do the following:

如果要建立一個名為 "native_add" 的外掛，
你需要這麼做：

```terminal
$ flutter create --platforms=android,ios,macos,windows,linux --template=plugin_ffi native_add
$ cd native_add
```

{{site.alert.note}}

You can exclude platforms from `--platforms` that you don't want
to build to. However, you need to include the platform of
the device you are testing on.

你可以使用 `--platforms` 來排除你不需要的平台。
但是，你仍需要包含測試裝置所需的平台。

{{site.alert.end}}

This will create a plugin with C/C++ sources in `native_add/src`.
These sources are built by the native build files in the various
os build folders.

C/C++ 原始碼會被建立至 `native_add/src`。
這些原始碼在不同平台建構時會產生在不同平台的建構資料夾。

The FFI library can only bind against C symbols,
so in C++ these symbols are marked `extern "C"`.

FFI 庫只能繫結 C 語言的符號，所以 C++ 語言的符號會被標記為 `extern "C"`。

You should also add attributes to indicate that the
symbols are referenced from Dart,
to prevent the linker from discarding the symbols
during link-time optimization.
`__attribute__((visibility("default"))) __attribute__((used))`.

FFI 庫只能與 C 符號繫結，因此在 C++ 中，
這些符號新增 `extern C` 標記。
還應該新增屬性來表明符號是需要被 Dart 參考的，
以防止連結器在最佳化連結時會丟棄符號。
`__attribute__((visibility("default"))) __attribute__((used))`.

On iOS, the `native_add/ios/native_add.podspec` links the code.

在 iOS 上 `native_add/android/build.gradle` 負責關聯這些程式碼。

The native code is invoked from dart in `lib/native_add_bindings_generated.dart`.

原生程式碼會從 `lib/native_add_bindings_generated.dart` 被 Dart 呼叫。

The bindings are generated with [package:ffigen](https://pub.dev/packages/ffigen).

程式碼由 [package:ffigen](https://pub.dev/packages/ffigen) 產生。

## Other use cases

## 其他的使用案例

### iOS and macOS

### iOS 和 macOS

Dynamically linked libraries are automatically loaded by
the dynamic linker when the app starts. Their constituent
symbols can be resolved using [`DynamicLibrary.process`][].
You can also get a handle to the library with
[`DynamicLibrary.open`][] to restrict the scope of
symbol resolution, but it's unclear how Apple's
review process handles this.

動態連結庫在應用程式啟動時由動態連結器自動載入。
它們的組成符號可以用 [`DynamicLibrary.process`][]。
你還可以使用 [`DynamicLibrary.open`][]
來限制符號解析的範圍，
但目前仍然不確定蘋果的審查程式將如何處理兩者的使用。

Symbols statically linked into the application binary
can be resolved using [`DynamicLibrary.executable`][] or
[`DynamicLibrary.process`][].

你可以使用 [`DynamicLibrary.executable`][]
或 [`DynamicLibrary.process`][]
解析靜態連結到應用程式二進位制檔案的符號。

[`DynamicLibrary.executable`]: {{site.dart.api}}/dev/dart-ffi/DynamicLibrary/DynamicLibrary.executable.html
[`DynamicLibrary.open`]: {{site.dart.api}}/dev/dart-ffi/DynamicLibrary/DynamicLibrary.open.html
[`DynamicLibrary.process`]: {{site.dart.api}}/dev/dart-ffi/DynamicLibrary/DynamicLibrary.process.html

#### Platform library

#### 平台庫

To link against a platform library,
use the following instructions:

要連結到平台庫，
請按照如下說明：

1. In Xcode, open `Runner.xcworkspace`.

   在 Xcode 中，開啟 `Runner.xcworkspace`。

1. Select the target platform.

   選擇目標裝置。

1. Click **+** in the **Linked Frameworks and Libraries**
   section.

   在 **Linked Frameworks and Libraries** 中點選 **+**。

1. Select the system library to link against.

   選擇要連結的系統庫。

#### First-party library

#### 第一方庫

A first-party native library can be included either
as source or as a (signed) `.framework` file.
It's probably possible to include statically linked
archives as well, but it requires testing.

第一方本地庫可以作為原始檔或（已簽名的）`.framework` 檔案被包含在內。
它也可能包括靜態連結的檔案，但需要測試。

#### Source code

#### 原始碼

To link directly to source code,
use the following instructions:

要直接連結到原始碼，請按照如下說明：

 1. In Xcode, open `Runner.xcworkspace`.

    在 Xcode 中，開啟 `Runner.xcworkspace`。

 2. Add the C/C++/Objective-C/Swift
    source files to the Xcode project.

    新增 C/C++/Objective-C/Swift 原始碼到 Xcode 工程中。

 3. Add the following prefix to the
    exported symbol declarations to ensure they
    are visible to Dart:

    將以下字首新增到匯出的符號宣告中，以確保它們對 Dart 可見：

    **C/C++/Objective-C**

    ```objective-c
    extern "C" /* <= C++ only */ __attribute__((visibility("default"))) __attribute__((used))
    ```

    **Swift**

    ```swift
    @_cdecl("myFunctionName")
    ```

#### Compiled (dynamic) library

#### 已編譯的動態庫

To link to a compiled dynamic library,
use the following instructions:

要連結到已編譯過的動態庫，請按照如下說明：

1. If a properly signed `Framework` file is present,
   open `Runner.xcworkspace`.

   如果存在已進行簽名的 `Framework` 檔案，請開啟 `Runner.xcworkspace`。

1. Add the framework file to the **Embedded Binaries**
   section.

   新增 framework 檔案到 **Embedded Binaries** 區域中。

1. Also add it to the **Linked Frameworks & Libraries**
   section of the target in Xcode.
   
   同時將其新增到 Xcode 中目標的
   **Linked Frameworks & Libraries** 部分。

#### Open-source third-party library

#### 開源的三方庫

To create a Flutter plugin that includes both
C/C++/Objective-C _and_ Dart code,
use the following instructions:

要建立一個包含 C/C++/Objective-C **和** Dart 程式碼的 Flutter 外掛，
請按照如下說明：

1. In your plugin project,
   open `ios/<myproject>.podspec`.

   在你的外掛專案開啟 `ios/<myproject>.podspec`.

1. Add the native code to the `source_files`
   field.

   新增原生代碼到 `source_files` 欄位。

The native code is then statically linked into
the application binary of any app that uses
this plugin.

原生代碼會被靜態連結到任何使用這個外掛的應用二進位制中。

#### Closed-source third-party library

#### 閉源三方庫

To create a Flutter plugin that includes Dart
source code, but distribute the C/C++ library
in binary form, use the following instructions:

要建立包含 Dart 原始碼，
但 C/C++ 部分是以二進位制形式分發的函式庫的 Flutter 外掛，
請按照如下說明：

1. In your plugin project,
   open `ios/<myproject>.podspec`.

   在你的外掛目錄開啟 `ios/<myproject>.podspec`。

1. Add a `vendored_frameworks` field.
   See the [CocoaPods example][].

   新增 `vendored_frameworks` 欄位。
   參考 [CocoaPods 範例][CocoaPods example]。

{{site.alert.warning}}

  **Do not** upload this plugin
  (or any plugin containing binary code) to pub.dev.
  Instead, this plugin should be downloaded
  from a trusted third-party,
  as shown in the CocoaPods example.

  **不要**將此外掛
 （或任何包含二進位制程式碼的外掛）上載到 pub.dev。
  相反，應該從可信的第三方下載此外掛。
  如 CocoaPods 範例所示。

{{site.alert.end}}

[CocoaPods example]: {{site.github}}/CocoaPods/CocoaPods/blob/master/examples/Vendored%20Framework%20Example/Example%20Pods/VendoredFrameworkExample.podspec

## Stripping iOS symbols

## 精簡 iOS 符號表

When creating a release archive (IPA),
the symbols are stripped by Xcode.

當建立一個 release 檔案（IPA）時，符號會被 Xcode 刪除。

1. In Xcode, go to **Target Runner > Build Settings > Strip Style**.

   在 Xcode 中, 點選 **Target Runner > Build Settings > Strip Style**.

2. Change from **All Symbols** to **Non-Global Symbols**.

   將 **All Symbols** 修改為 **Non-Global Symbols**。

{% include docs/resource-links/ffi-video-resources.md %}