---
title: Obfuscating Dart code
title: 混淆 Dart 程式碼
description: How to remove function and class names from your Dart binary.
description: 如何移除你的 Dart 庫中的方法名和類別名稱。
tags: 釋出, Dart
keywords: 程式碼混淆,保護
---

<?code-excerpt path-base="deployment/obfuscate"?>

## What is code obfuscation?

## 什麼是程式碼混淆？

[Code obfuscation][] is the process of modifying an
app's binary to make it harder for humans to understand.
Obfuscation hides function and class names in your
compiled Dart code, replacing each symbol with
another symbol, making it difficult for an attacker
to reverse engineer your proprietary app.

[程式碼混淆][Code obfuscation] 是一種將應用程式二進位制檔案轉換為功能上等價，
但人類難於閱讀和理解的行為。
在編譯 Dart 程式碼時，混淆會隱藏函式和類別的名稱，
並用其他符號替代每個符號，從而使攻擊者難以進行逆向工程。

**Flutter's code obfuscation works
only on a [release build][].**

**Flutter 的程式碼混淆功能僅在 [生產建構][release build] 上生效。**

[Code obfuscation]: https://en.wikipedia.org/wiki/Obfuscation_(software)
[release build]: {{site.url}}/testing/build-modes#release

## Limitations

## 侷限性

Note that obfuscating your code does _not_
encrypt resources nor does it protect against
reverse engineering.
It only renames symbols with more obscure names.

請注意，混淆你的程式碼並 **不會** 加密資源，
也不能防止逆向工程。
它只是用更晦澀的名稱重新命名這些符號。

{{site.alert.info}}

  It is a **poor security practice** to
  store secrets in an app.

  在應用程式中儲存重要私密的資訊（如密碼、金鑰等）
  是一種 **非常不安全的做法**。

{{site.alert.end}}

## Supported targets

## 支援的建構目標

The following build targets
support the obfuscation process
described on this page:

以下建構目標支援本篇介紹的混淆過程：

* `aar`
* `apk`
* `appbundle`
* `ios`
* `ios-framework`
* `ipa`
* `linux`
* `macos`
* `macos-framework`
* `windows`

{{site.alert.info}}

  Web apps don't support obfuscation.
  A web app can be [minified][], which provides a similar result.
  When you build a release version of a Flutter web app,
  the web compiler minifies the app. To learn more,
  see [Build and release a web app][].

  Web 應用不支援混淆。
  因為當你建構 Flutter Web 應用釋出版本時，
  Web 應用已經經過了 [壓縮][minified] 處理。
  Web 壓縮提供了與混淆相似的效果。

{{site.alert.end}}

[Build and release a web app]: {{site.url}}/deployment/web
[minified]: https://en.wikipedia.org/wiki/Minification_(programming)

## Obfuscate your app

## 混淆你的應用程式

To obfuscate your app, use the `flutter build` command
in release mode
with the `--obfuscate` and  `--split-debug-info` options.
The `--split-debug-info` option specifies the directory
where Flutter outputs debug files.
In the case of obfuscation, it outputs a symbol map.
For example:

要混淆你的應用程式，
請在 release 模式下使用 `flutter build` 命令，
並使用 `--obfuscate` 和 `--split-debug-info` 選項。
`--split-debug-info` 選項指定了 Flutter 輸出除錯檔案的目錄。
在混淆的情況下，它會輸出一個符號表。
請參考以下命令：

```terminal
$ flutter build apk --obfuscate --split-debug-info=/<project-name>/<directory>
```

Once you've obfuscated your binary, **save
the symbols file**. You need this if you later
want to de-obfuscate a stack trace.

一旦你混淆了二進位制檔案，
請務必 **儲存符號表檔案**。
如果你將來需要解析混淆後的堆疊追蹤，
你將需要該檔案。

{{site.alert.tip}}

  The `--split-debug-info` option can also be used without `--obfuscate`
  to extract Dart program symbols, reducing code size.
  To learn more about app size, see [Measuring your app's size][].

  `--split-debug-info` 選項也可以不使用 `--obfuscate` 來提取 Dart 程式符號，
  以減少程式碼體積。
  想了解更多關於應用體積的資訊，
  請查閱 [測量你的應用體積][Measuring your app's size]。

{{site.alert.end}}

[Measuring your app's size]: {{site.url}}/perf/app-size

For detailed information on these flags, run
the help command for your specific target, for example:

關於這些標誌的詳細資訊，
請執行特定建構目標型別的幫助命令，
例如：

```terminal
$ flutter build apk -h
```

If these flags are not listed in the output,
run `flutter --version` to check your version of Flutter.

如果輸出中沒有列出這些標誌，
請執行 `flutter --version` 命令，檢查你的 Flutter 版本。

## Read an obfuscated stack trace

## 讀取混淆的堆疊追蹤

To debug a stack trace created by an obfuscated app,
use the following steps to make it human readable:

如果你需要除錯被混淆的應用程式建立的堆疊追蹤，
請遵循以下步驟將其解析為人類可讀的內容：

1. Find the matching symbols file.
   For example, a crash from an Android arm64
   device would need `app.android-arm64.symbols`.

   找到與應用程式匹配的符號檔案。
   例如，在 Android arm64 裝置崩潰時，
   需要 `app.android-arm64.symbols` 檔案。

1. Provide both the stack trace (stored in a file)
   and the symbols file to the `flutter symbolize` command.
   For example:

   向 `flutter symbolize` 命令提供堆疊追蹤（儲存在檔案中）和符號檔案。
   例如：

   ```terminal
   $ flutter symbolize -i <stack trace file> -d out/android/app.android-arm64.symbols
   ```

   For more information on the `symbolize` command,
   run `flutter symbolize -h`.

   關於 `symbolize` 命令的更多資訊，
   請執行 `flutter symbolize -h` 命令。

## Read an obfuscated name

To make the name that an app obfuscated human readable,
use the following steps:

1. To save the name obfuscation map at app build time,
   use `--extra-gen-snapshot-options=--save-obfuscation-map=/<your-path>`.
   For example:

   ```terminal
   $ flutter build apk --obfuscate --split-debug-info=/<project-name>/<directory> --extra-gen-snapshot-options=--save-obfuscation-map=/<your-path>
   ```

1. To recover the name, use the generated obfuscation map.
   The obfuscation map is a flat JSON array with pairs of
   original names and obfuscated names. For example,
   `["MaterialApp", "ex", "Scaffold", "ey"]`, where `ex`
   is the obfuscated name of `MaterialApp`.

## Caveat

## 注意事項

Be aware of the following when coding an app that will
eventually be an obfuscated binary.

當你打算將二進位制的應用程式進行混淆時，
需要注意以下內容：

* Code that relies on matching specific class, function,
  or library names will fail.
  For example, the following call to `expect()` won't
  work in an obfuscated binary:

  使用匹配特定的類、函式或庫名的程式碼將會失效。
  例如，以下在混淆的二進位制檔案中對 `expect()` 的呼叫就不會工作：

<?code-excerpt "lib/main.dart (Expect)"?>
```dart
expect(foo.runtimeType.toString(), equals('Foo'));
```

* Enum names are not obfuscated currently.
