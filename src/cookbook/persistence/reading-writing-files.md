---
title: Read and write files
title: 檔案讀寫
description: How to read from and write to files on disk.
description: 如何讀寫硬碟上的檔案。
tags: cookbook, 實用課程, 持久化
keywords: 檔案讀寫,臨時資料夾,Documents目錄
prev:
  title: Persist data with SQLite
  title: 用 SQLite 做資料持久化
  path: /docs/cookbook/persistence/sqlite
next:
  title: Store key-value data on disk
  title: 儲存鍵值對資料
  path: /docs/cookbook/persistence/key-value
---

<?code-excerpt path-base="cookbook/persistence/reading_writing_files/"?>

In some cases, you need to read and write files to disk.
For example, you may need to persist data across app launches,
or download data from the internet and save it for later offline use.

磁碟檔案的讀寫操作可能會相對方便地實現某些業務場景。
它常見於應用啟動期間產生的持久化資料，或者從網路下載資料供離線使用。

To save files to disk, combine the [`path_provider`][]
plugin with the [`dart:io`][] library.

為了將檔案儲存到磁碟，你需要結合使用
[`dart:io`][] 庫中的 [`path_provider`][] 這個 package。

## Directions

## 步驟

  1. Find the correct local path.
  
     找到正確的本地路徑
     
  2. Create a reference to the file location.
  
     建立一個指向檔案位置的參考
  
  3. Write data to the file.
  
     將資料寫入檔案
  
  4. Read data from the file.
  
     從檔案讀取資料
     

{{site.alert.note}}
To learn more, watch this Package of the Week video on the path_provider package:

<iframe class="full-width" src="{{site.youtube-site}}/embed/Ci4t-NkOY3I" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{{site.alert.end}}

## 1. Find the correct local path

## 1. 找到正確的本地路徑

This example displays a counter. When the counter changes,
write data on disk so you can read it again when the app loads.
Where should you store this data?

這個例子裡，我們將會顯示一個計數器，當計數器發生變化時，你將在磁碟中寫入資料，
以便在應用載入時重新讀取這些資料。因此，你一定想知道：我應該將這些資料儲存在哪裡？

The [`path_provider`][] package
provides a platform-agnostic way to access commonly used locations on the
device's file system. The plugin currently supports access to
two file system locations:

[`path_provider`][] package 提供一種平台無關的方式以一致的方式存取裝置的檔案位置系統。
該 plugin 當前支援存取兩種檔案位置系統：

*Temporary directory*
<br> A temporary directory (cache) that the system can
  clear at any time. On iOS, this corresponds to the
  [`NSCachesDirectory`][]. On Android, this is the value that
  [`getCacheDir()`][] returns.

*臨時資料夾：* 
<br> 這是一個系統可以隨時清空的臨時（快取）資料夾。
  在 iOS 上 對應 [`NSCachesDirectory`][] 的返回值；
  在 Android 上對應 [`getCacheDir()`][] 的返回值。

*Documents directory*
<br> A directory for the app to store files that only
  it can access. The system clears the directory only when the app
  is deleted.
  On iOS, this corresponds to the `NSDocumentDirectory`.
  On Android, this is the `AppData` directory.
  
*Documents 目錄：*
<br> 供應用使用，用於儲存只能由該應用存取的檔案。
  只有在刪除應用時，系統才會清除這個目錄。
  在 iOS 上，這個目錄對應於 `NSDocumentDirectory`。
  在 Android 上，則是 `AppData` 目錄。 

This example stores information in the documents directory.
You can find the path to the documents directory as follows:

在本範例中，你需要將資訊儲存在 Documents 目錄中。
可以按如下所示，找到 Documents 目錄路徑：

<?code-excerpt "lib/main.dart (localPath)"?>
```dart
Future<String> get _localPath async {
  final directory = await getApplicationDocumentsDirectory();

  return directory.path;
}
```

## 2. Create a reference to the file location

## 2. 建立一個指向檔案位置的參考

Once you know where to store the file, create a reference to the
file's full location. You can use the [`File`][]
class from the [`dart:io`][] library to achieve this.

確定檔案的儲存位置後，需要建立對檔案完整位置的參考。
為此，你可以使用 [`dart:io`][] 庫的 [`File`][] 類來實現。

<?code-excerpt "lib/main.dart (localFile)"?>
```dart
Future<File> get _localFile async {
  final path = await _localPath;
  return File('$path/counter.txt');
}
```

## 3. Write data to the file

## 3. 將資料寫入檔案

Now that you have a `File` to work with,
use it to read and write data.
First, write some data to the file.
The counter is an integer, but is written to the
file as a string using the `'$counter'` syntax.

現在你已經有了可以使用的 `File`，接下來使用這個檔案來讀寫資料。
首先，將一些資料寫入該檔案。由於使用了計數器，
因此只需將整數儲存為字串格式，
使用 `'$counter'` 即可呼叫。

<?code-excerpt "lib/main.dart (writeCounter)"?>
```dart
Future<File> writeCounter(int counter) async {
  final file = await _localFile;

  // Write the file
  return file.writeAsString('$counter');
}
```

## 4. Read data from the file

## 4. 從檔案讀取資料

Now that you have some data on disk, you can read it.
Once again, use the `File` class.

現在，你的磁碟上已經有了一些資料可供讀取。
此時同樣需要使用 `File` 類別。

<?code-excerpt "lib/main.dart (readCounter)"?>
```dart
Future<int> readCounter() async {
  try {
    final file = await _localFile;

    // Read the file
    final contents = await file.readAsString();

    return int.parse(contents);
  } catch (e) {
    // If encountering an error, return 0
    return 0;
  }
}
```

## Complete example

## 完整範例

<?code-excerpt "lib/main.dart"?>
```dart
import 'dart:async';
import 'dart:io';

import 'package:flutter/material.dart';
import 'package:path_provider/path_provider.dart';

void main() {
  runApp(
    MaterialApp(
      title: 'Reading and Writing Files',
      home: FlutterDemo(storage: CounterStorage()),
    ),
  );
}

class CounterStorage {
  Future<String> get _localPath async {
    final directory = await getApplicationDocumentsDirectory();

    return directory.path;
  }

  Future<File> get _localFile async {
    final path = await _localPath;
    return File('$path/counter.txt');
  }

  Future<int> readCounter() async {
    try {
      final file = await _localFile;

      // Read the file
      final contents = await file.readAsString();

      return int.parse(contents);
    } catch (e) {
      // If encountering an error, return 0
      return 0;
    }
  }

  Future<File> writeCounter(int counter) async {
    final file = await _localFile;

    // Write the file
    return file.writeAsString('$counter');
  }
}

class FlutterDemo extends StatefulWidget {
  const FlutterDemo({super.key, required this.storage});

  final CounterStorage storage;

  @override
  State<FlutterDemo> createState() => _FlutterDemoState();
}

class _FlutterDemoState extends State<FlutterDemo> {
  int _counter = 0;

  @override
  void initState() {
    super.initState();
    widget.storage.readCounter().then((value) {
      setState(() {
        _counter = value;
      });
    });
  }

  Future<File> _incrementCounter() {
    setState(() {
      _counter++;
    });

    // Write the variable as a string to the file.
    return widget.storage.writeCounter(_counter);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Reading and Writing Files'),
      ),
      body: Center(
        child: Text(
          'Button tapped $_counter time${_counter == 1 ? '' : 's'}.',
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

[`dart:io`]: {{site.api}}/flutter/dart-io/dart-io-library.html
[`File`]: {{site.api}}/flutter/dart-io/File-class.html
[`getCacheDir()`]: {{site.android-dev}}/reference/android/content/Context#getCacheDir()
[`NSCachesDirectory`]: {{site.apple-dev}}/documentation/foundation/nssearchpathdirectory/nscachesdirectory
[`path_provider`]: {{site.pub-pkg}}/path_provider
