---
title: Store key-value data on disk
title: 儲存鍵值對資料
description: >-
  Learn how to use the shared_preferences package to store key-value data.
description: 學習如何使用 shared_preferences 包來儲存鍵值對資料。
tags: cookbook, 實用課程, 持久化
keywords: KV,SharedPreferences
---

<?code-excerpt path-base="cookbook/persistence/key_value/"?>

If you have a relatively small collection of key-values
to save, you can use the [`shared_preferences`][] plugin.

如果你要儲存的鍵值集合相對較少，
則可以用 [`shared_preferences`][] 外掛。

Normally, you would have to
write native platform integrations for storing data on each platform.
Fortunately, the [`shared_preferences`][] plugin can be used to
persist key-value data to disk on each platform Flutter supports.

通常你需要在兩個平台用原生的方式儲存資料。
幸運的是 [`shared_preferences`] 外掛可以把 key-value 儲存到磁碟中。

This recipe uses the following steps:

這個課程包含以下步驟：

  1. Add the dependency.

     新增依賴

  2. Save data.

     儲存資料

  3. Read data.

     讀取資料

  4. Remove data.

     移除資料

{{site.alert.note}}
  To learn more, watch this short Package of the Week video
  on the `shared_preferences` package:

  <iframe class="full-width" src="{{site.youtube-site}}/embed/sa_U0jffQII" title="YouTube video player - shared_preferences (Package of the Week)" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
{{site.alert.end}}

## 1. Add the dependency

## 1. 新增依賴

Before starting, add the [`shared_preferences`][] package as a dependency.

在開始之前，你需要新增 [`shared_preferences`][] 為依賴：

To add the `shared_preferences` package as a dependency,
run `flutter pub add`:

執行 `flutter pub add` 將 `shared_preferences` 新增為依賴：

```terminal
$ flutter pub add shared_preferences
```

## 2. Save data

## 2. 儲存資料

To persist data, use the setter methods provided by the
`SharedPreferences` class. Setter methods are available for
various primitive types, such as `setInt`, `setBool`, and `setString`.

要儲存資料，請使用 `SharedPreferences` 類別的 setter 方法。
Setter方法可用於各種基本資料型別，
例如 `setInt`、`setBool` 和 `setString`。

Setter methods do two things: First, synchronously update the
key-value pair in memory. Then, persist the data to disk.

Setter 方法做兩件事：
首先，同步更新 key-value 到記憶體中，然後儲存到磁碟中。

<?code-excerpt "lib/partial_excerpts.dart (Step2)"?>
```dart
// Load and obtain the shared preferences for this app.
final prefs = await SharedPreferences.getInstance();

// Save the counter value to persistent storage under the 'counter' key.
await prefs.setInt('counter', counter);
```

## 3. Read data

## 3. 讀取資料

To read data, use the appropriate getter method provided by the
`SharedPreferences` class. For each setter there is a corresponding getter.
For example, you can use the `getInt`, `getBool`, and `getString` methods.

要讀取資料，請使用 `SharedPreferences` 類相應的 getter 方法。
對於每一個 setter 方法都有對應的 getter 方法。
例如，你可以使用 `getInt`、`getBool` 和 `getString` 方法。

<?code-excerpt "lib/partial_excerpts.dart (Step3)"?>
```dart
final prefs = await SharedPreferences.getInstance();

// Try reading the counter value from persistent storage.
// If not present, null is returned, so default to 0.
final counter = prefs.getInt('counter') ?? 0;
```

Note that the getter methods throw an exception if the persisted value
has a different type than the getter method expects.

## 4. Remove data

## 4. 移除資料

To delete data, use the `remove()` method.

使用 `remove()` 方法刪除資料。

<?code-excerpt "lib/partial_excerpts.dart (Step4)"?>
```dart
final prefs = await SharedPreferences.getInstance();

// Remove the counter key-value pair from persistent storage.
await prefs.remove('counter');
```

## Supported types

## 支援型別

Although the key-value storage provided by `shared_preferences` is
easy and convenient to use, it has limitations:

雖然使用 `shared_preferences` 提供的鍵值對儲存非常簡單方便，
但是它也有以下侷限性：

* Only primitive types can be used: `int`, `double`, `bool`, `String`,
  and `List<String>`.

  只能用於基本資料型別： `int`、`double`、`bool`、`string` 和 `List<String>`。

* It's not designed to store large amounts of data.

  不是為儲存大量資料而設計的。

* There is no guarantee that data will be persisted across app restarts.

  不能確保應用重啟後資料仍然存在。

## Testing support

## 測試支援

It's a good idea to test code that persists data using `shared_preferences`.
To enable this, the package provides an
in-memory mock implementation of the preference store.

使用 `shared_preferences` 來儲存測試程式碼的資料是一個不錯的思路。
為此，你需要使用 package 自帶的基於記憶體的模擬持久化儲存。

To set up your tests to use the mock implementation,
call the `setMockInitialValues` static method in
a `setUpAll()` method in your test files.
Pass in a map of key-value pairs to use as the initial values.

在你的測試中，你可以透過在測試檔案的 `setupAll()` 方法中
呼叫 `setMockInitialValues` 靜態方法來使用對應的模擬儲存。
同時你還可以設定初始值：

<?code-excerpt "test/prefs_test.dart (setup)"?>
```dart
SharedPreferences.setMockInitialValues(<String, Object>{
  'counter': 2,
});
```

## Complete example

## 完整範例

<?code-excerpt "lib/main.dart"?>
```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Shared preferences demo',
      home: MyHomePage(title: 'Shared preferences demo'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  @override
  void initState() {
    super.initState();
    _loadCounter();
  }

  /// Load the initial counter value from persistent storage on start,
  /// or fallback to 0 if it doesn't exist.
  Future<void> _loadCounter() async {
    final prefs = await SharedPreferences.getInstance();
    setState(() {
      _counter = prefs.getInt('counter') ?? 0;
    });
  }

  /// After a click, increment the counter state and
  /// asynchronously save it to persistent storage.
  Future<void> _incrementCounter() async {
    final prefs = await SharedPreferences.getInstance();
    setState(() {
      _counter = (prefs.getInt('counter') ?? 0) + 1;
      prefs.setInt('counter', _counter);
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'You have pushed the button this many times: ',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
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


[`shared_preferences`]: {{site.pub-pkg}}/shared_preferences
