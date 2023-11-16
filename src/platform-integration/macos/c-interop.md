---
title: "Binding to native macOS code using dart:ffi"
title: "在 macOS 中使用 dart:ffi 呼叫原生代碼"
description: "To use C code in your Flutter program, use the dart:ffi library."
description: "在你的 Flutter 工程中，透過 dart:ffi 來使用 C 語言程式碼"
---

<?code-excerpt path-base="development/platform_integration"?>

Flutter mobile and desktop apps can use the
[dart:ffi][] library to call native C APIs.
_FFI_ stands for [_foreign function interface._][FFI]
Other terms for similar functionality include
_native interface_ and _language bindings._

Flutter 移動版可以使用 [dart:ffi][] 庫來呼叫本地的 C API。
**FFI** 代表 [**外部功能介面**][FFI]。
類似功能的其他術語包括 **本地介面** 和 **語言繫結**。

{{site.alert.note}}

  This page describes using the `dart:ffi` library
  in macOS desktop apps.
  For information on Android, see
  [Binding to native Android code using dart:ffi][android-ffi].
  For information on iOS, see
  [Binding to native iOS code using dart:ffi][ios-ffi].
  This feature is not yet supported for web plugins.

  本文描述的是在 macOS 桌面端應用程式中使用 `dart:ffi` 庫。
  你可以閱讀 [在 Android 中使用 dart:ffi 呼叫原生代碼][android-ffi]
  或 [在 iOS 中使用 dart:ffi 呼叫原生代碼][ios-ffi]。
  Web 外掛暫不支援呼叫原生代碼。

{{site.alert.end}}


[android-ffi]: {{site.url}}/platform-integration/android/c-interop
[ios-ffi]: {{site.url}}/platform-integration/ios/c-interop
[dart:ffi]: {{site.dart.api}}/dev/dart-ffi/dart-ffi-library.html
[FFI]: https://en.wikipedia.org/wiki/Foreign_function_interface

Before your library or program can use the FFI library
to bind to native code, you must ensure that the
native code is loaded and its symbols are visible to Dart.
This page focuses on compiling, packaging,
and loading macOS native code within a Flutter plugin or app.

您必須首先確保原生代碼已載入，並且其符號對 Dart 可見，
然後才能在庫或程式使用 FFI 庫繫結原生代碼。
本頁主要介紹如何在 Flutter 外掛或應用程式中編譯、
打套件和載入 macOS 原生代碼。

This tutorial demonstrates how to bundle C/C++
sources in a Flutter plugin and bind to them using
the Dart FFI library on macOS.
In this walkthrough, you'll create a C function
that implements 32-bit addition and then
exposes it through a Dart plugin named "native_add".

本課程示範瞭如何在 Flutter 外掛中捆綁 C/C++ 原始碼，
並使用 macOS 上的 Dart FFI 庫繫結它們。
在本範例中，您將建立一個實現 32 位的加法 C 函式，
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
或 `DynamicLibrary.process` 來載入。

A dynamically linked library, by contrast, is distributed
in a separate file or folder within the app,
and loaded on-demand. On macOS, the dynamically linked
library is distributed as a `.framework` folder.

相比之下，動態連結庫則分佈在應用程式中的單獨的檔案或資料夾中，
並按需載入。在 macOS 上，它是作為 `.framework` 資料夾分發的。

A dynamically linked library can be loaded into
Dart using `DynamicLibrary.open`.

動態連結庫在 Dart 中可以透過 `DynamicLibrary.open` 載入。

API documentation is available from the Dart dev channel:
[Dart API reference documentation][].

Dart dev 頻道中的 API 已經可用：
[Dart API 參考文件][Dart API reference documentation]。

[Dart API reference documentation]: {{site.dart.api}}/dev/

## Step 1: Create a plugin

## 步驟 1：建立外掛

If you already have a plugin, skip this step.

如果您已經有一個外掛，跳過這步。

To create a plugin called "native_add",
do the following:

如果要建立一個名為 "native_add" 的外掛，
您需要這麼做：

```terminal
$ flutter create --platforms=macos --template=plugin native_add
$ cd native_add
```

{{site.alert.note}}

  You can exclude platforms from `--platforms` that you don't want
  to build to. However, you need to include the platform of 
  the device you are testing on.

  您可以使用 `--platforms` 來排除您不需要的平台。
  但是，您仍需要包含測試裝置所需的平台。

{{site.alert.end}}

## Step 2: Add C/C++ sources

## 步驟 2：新增 C/C++ 原始碼

You need to inform the macOS build system about the
native code so the code can be compiled
and linked appropriately into the final application.

您需要讓 macOS 建構系統知道原生代碼的存在，
以便程式碼可以被編譯並連結到最終的應用程式中。

Add the sources to the `macos` folder,
because CocoaPods doesn't allow including sources
above the `podspec` file.

您可以將原始碼新增到 `macos` 資料夾，
因為 CocoaPods 不允許原始碼處於比 `podspec` 檔案更高的目錄層級，

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

For example,
to create a C++ file named `macos/Classes/native_add.cpp`,
use the following instructions. (Note that the template
has already created this file for you.) Start from the
root directory of your project:

作為範例，建立一個 C++ 檔案，
路徑為：`macos/Classes/native_add.cpp`。
（請注意，範本已經為您建立了此檔案。）
在專案的根目錄下中執行以下命令：

```bash
cat > macos/Classes/native_add.cpp << EOF
#include <stdint.h>

extern "C" __attribute__((visibility("default"))) __attribute__((used))
int32_t native_add(int32_t x, int32_t y) {
    return x + y;
}
EOF
```

On macOS, you need to tell Xcode to statically link the file:

在 macOS 中，您需要告訴 Xcode 如何靜態連結這個檔案：

 1. In Xcode, open `Runner.xcworkspace`.

    在 Xcode 中，開啟 `Runner.xcworkspace`。

 2. Add the C/C++/Objective-C/Swift
    source files to the Xcode project.

    新增 C/C++/Objective-C/Swift
    原始碼檔案到 Xcode 工程中。

## Step 3: Load the code using the FFI library

In this example, you can add the following code to
`lib/native_add.dart`. However the location of the
Dart binding code isn't important.

在範例中，您需要新增如下的程式碼到 `lib/native_add.dart`。
但是，Dart 在何處進行程式碼繫結並不重要。

First, you must create a `DynamicLibrary` handle to
the native code.

首先，您需要建立一個 `DynamicLibrary` 來處理原生代碼。

```dart
import 'dart:ffi'; // For FFI

final DynamicLibrary nativeAddLib = DynamicLibrary.process();
```

With a handle to the enclosing library,
you can resolve the `native_add` symbol:

您可以透過使用庫的控制代碼來解析 `native_add` 符號：

<?code-excerpt "lib/c_interop.dart (NativeAdd)"?>
```dart
final int Function(int x, int y) nativeAdd = nativeAddLib
    .lookup<NativeFunction<Int32 Function(Int32, Int32)>>('native_add')
    .asFunction();
```

Finally, you can call it. To demonstrate this within
the auto-generated "example" app (`example/lib/main.dart`):

現在，您可以呼叫它了。在自動產生的 example 專案
（`example/lib/main.dart`）中示範它。

```nocode
// Inside of _MyAppState.build:
        body: Center(
          child: Text('1 + 2 == ${nativeAdd(1, 2)}'),
        ),
```

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
您還可以使用 [`DynamicLibrary.open`][]
來限制符號解析的範圍，
但目前仍然不確定蘋果的審查程式將如何處理兩者的使用。

Symbols statically linked into the application binary
can be resolved using [`DynamicLibrary.executable`][] or
[`DynamicLibrary.process`][].

您可以使用 [`DynamicLibrary.executable`][]
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

    將以下字首新增到匯出的符號宣告中，
    以確保它們對 Dart 可見：

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

#### Compiled (dynamic) library (macOS)

#### 已編譯的（動態）庫 (macOS)

To add a closed source library to a
[Flutter macOS Desktop][] app,
use the following instructions:

要新增一個閉源的函式庫到
[Flutter macOS 桌面][Flutter macOS Desktop] 應用，
請按照如下說明：

1. Follow the instructions for Flutter desktop to create
   a Flutter desktop app.

   按照 Flutter 桌面的使用說明來建立 Flutter 桌面應用程式。

1. Open the `yourapp/macos/Runner.xcworkspace` in Xcode.

   在 Xcode 中開啟 `yourapp/macos/Runner.xcworkspace`。

   1. Drag your precompiled library (`libyourlibrary.dylib`)
      into `Runner/Frameworks`.

      拖動您已經預編譯的 `libyourlibrary.dylib`
      到您的 `Runner/Frameworks`。

   1. Click `Runner` and go to the `Build Phases` tab.

      點選 `Runner` 然後進入 `Build Phases` 標籤。

      1. Drag `libyourlibrary.dylib` into the
         `Copy Bundle Resources` list.

         拖動 `libyourlibrary.dylib` 到
         `Copy Bundle Resources` 列表。

      1. Under `Embed Libraries`, check `Code Sign on Copy`.

         在 `Embed Libararies` 下，檢查 `Code Sign on Copy`。

      1. Under `Link Binary With Libraries`,
         set status to `Optional`. (We use dynamic linking,
         no need to statically link.)

         在 `Link Binary With Libraries` 下，
         設定狀態為 `Optional`。
         （我們使用動態連結，不需要靜態連結）

   1. Click `Runner` and go to the `General` tab.

      點選 `Runner` 然後進入 `General` 標籤頁。

      1. Drag `libyourlibrary.dylib` into the **Frameworks,
         Libraries and Embedded Content** list.

         拖動 `libyourlibrary.dylib` 到
         **Frameworks, Libararies and Embedded Content** 列表中。

      1. Select **Embed & Sign**.

         選擇 **Embed & Sign**。

   1. Click **Runner** and go to the **Build Settings** tab.

      點選 `Runner` 然後進入 `Build Settings` 標籤頁。

      1. In the **Search Paths** section configure the
         **Library Search Paths** to include the path
         where `libyourlibrary.dylib` is located.

         在 `Search Paths` 部分，
         配置 `Library Search Paths` 確保
         `libyourlibrary.dylib` 的路徑包括在內。

1. Edit `lib/main.dart`.

   編輯  `lib/main.dart` 檔案。

   1. Use `DynamicLibrary.open('libyourlibrary.dylib')`
      to dynamically link to the symbols.

      使用 `DynamicLibrary.open('libyourlibrary.dylib')`
      來動態連結符號表。

   1. Call your native function somewhere in a widget.

      在 widget 的某個地方呼叫您的原生代碼。

1. Run `flutter run` and check that your native function gets called.

   執行 `flutter run` 然後檢查您的本地方法的呼叫結果。

1. Run `flutter build macos` to build a self-contained release
   version of your app.

   執行 `flutter build macos`
   去建構一個自包含的 release 版本的應用。

[Flutter macOS Desktop]: {{site.url}}/platform-integration/macos/building

{% comment %}

#### Open-source third-party library

#### 開源的三方庫

To create a Flutter plugin that includes both
C/C++/Objective-C _and_ Dart code,
use the following instructions:

要建立一個包含 C/C++/Objective-C **和**
Dart 程式碼的 Flutter 外掛，請按照如下說明：

1. In your plugin project,
   open `macos/<myproject>.podspec`.

   在您的外掛專案開啟 `macos/<myproject>.podspec`。

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

要建立包含 Dart 原始碼，但 C/C++ 部分是
以二進位制形式分發的函式庫的 Flutter 外掛，
請按照如下說明：

1. In your plugin project,
   open `macos/<myproject>.podspec`.

   在您的外掛目錄開啟 `macos/<myproject>.podspec`。

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

## Stripping macOS symbols

When creating a release archive (IPA),
the symbols are stripped by Xcode.

1. In Xcode, go to **Target Runner > Build Settings > Strip Style**.
2. Change from **All Symbols** to **Non-Global Symbols**.

{% endcomment %}

{% include docs/resource-links/ffi-video-resources.md %}