---
title: Store key-value data on disk
title: 儲存鍵值對資料
description: How to use the shared_preferences package to store key-value data.
description: 如何使用 shared_preferences 包來儲存 key-value 資料。
tags: cookbook, 實用課程, 持久化
keywords: KV,SharedPreferences
prev:
  title: Read and write files
  title: 檔案讀寫
  path: /docs/cookbook/persistence/reading-writing-files
next:
  title: Play and pause a video
  title: 影片的播放和暫停
  path: /docs/cookbook/plugins/play-video
---

<?code-excerpt path-base="cookbook/persistence/key_value/"?>

If you have a relatively small collection of key-values
to save, you can use the [`shared_preferences`][] plugin.

如果你要儲存的鍵值集合相對較少，
則可以用 [`shared_preferences`][] 外掛。

Normally,
you would have to write native platform integrations for storing
data on both iOS and Android. Fortunately,
the [`shared_preferences`][] plugin can be used to persist
key-value data on disk. The shared preferences plugin
wraps `NSUserDefaults` on iOS and `SharedPreferences` on Android,
providing a persistent store for simple data.

通常你需要在兩個平台用原生的方式儲存資料。
幸運的是 [`shared_preferences`] 外掛可以把 key-value 儲存到磁碟中。
它透過封裝 iOS 上的 `NSUserDefaults` 和 Android
上的 `SharedPreferences` 為簡單資料提供持久化儲存。

This recipe uses the following steps:

這個課程包含以下步驟：

  1. Add the dependency.

     新增依賴

  2. Save Data.

     儲存資料

  3. Read Data.

     讀取資料

  4. Remove Data.

     移除資料

{{site.alert.note}}
  To learn more, watch this short Package of the Week video on the shared_preferences package:

  <iframe class="full-width" src="{{site.youtube-site}}/embed/sa_U0jffQII" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
{{site.alert.end}}

## 1. Add the dependency

## 1. 新增依賴

Before starting, add the [`shared_preferences`][]
plugin to the `pubspec.yaml` file:

在開始之前，你需要在 `pubspec.yaml` 
檔案中新增 [`shared_preferences`][] 外掛：

```yaml
dependencies:
  flutter:
    sdk: flutter
  shared_preferences: "<newest version>"
```

## 2. Save data

## 2. 儲存資料

To persist data, use the setter methods provided by the
`SharedPreferences` class. Setter methods are available for
various primitive types, such as `setInt`, `setBool`, and `setString`.

要儲存資料，請使用 `SharedPreferences` 類別的 setter 方法。
Setter方法可用於各種基本資料型別，
例如 `setInt`、`setBool` 和 `setString`。

Setter methods do two things: First, synchronously update the key-value pair
in-memory. Then, persist the data to disk.

Setter 方法做兩件事：
首先，同步更新 key-value 到記憶體中，然後儲存到磁碟中。

<?code-excerpt "lib/partial_excerpts.dart (Step2)"?>
```dart
// obtain shared preferences
final prefs = await SharedPreferences.getInstance();

// set value
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

// Try reading data from the counter key. If it doesn't exist, return 0.
final counter = prefs.getInt('counter') ?? 0;
```

## 4. Remove data

## 4. 移除資料

To delete data, use the `remove()` method.

使用 `remove()` 方法刪除資料。

<?code-excerpt "lib/partial_excerpts.dart (Step4)"?>
```dart
final prefs = await SharedPreferences.getInstance();

await prefs.remove('counter');
```

## Supported types

## 支援型別

Although key-value storage is easy and convenient to use,
it has limitations:

雖然使用 key-value 儲存非常簡單方便，
但是它也有以下侷限性：

* Only primitive types can be used: `int`, `double`, `bool`, `string`,
  and `stringList`.
  
  只能用於基本資料型別： `int`、`double`、`bool`、`string` 和 `stringList`。
  
* It's not designed to store a lot of data.

  不適用於大量資料的儲存。

For more information about shared preferences on Android,
see the [shared preferences documentation][]
on the Android developers website.

關於 Android 平臺上 Shared Preferences 的更多資訊，
請前往 Android 開發者網站上檢視
[Shared preferences 文件][shared preferences documentation] 。

## Testing support

## 測試支援

It's a good idea to test code that persists data using
`shared_preferences`. You can do this by mocking out the
`MethodChannel` used by the `shared_preferences` library.

使用 `shared_preferences` 儲存資料來測試程式碼是一個不錯的思路。
為此，你需要模擬出 `shared_preferences` 庫的 `MethodChannel` 方法。

Populate `SharedPreferences` with initial values in your tests
by running the following code in a `setupAll()` method in
your test files:

在你的測試中，你可以透過在測試檔案的 `setupAll()` 方法中新增執行以下程式碼，
對 `SharedPreferences` 的值進行初始：

<?code-excerpt "lib/partial_excerpts.dart (Testing)"?>
```dart
const MethodChannel('plugins.flutter.io/shared_preferences')
    .setMockMethodCallHandler((methodCall) async {
  if (methodCall.method == 'getAll') {
    return <String, dynamic>{}; // set initial values here if desired
  }
  return null;
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

  // This widget is the root of the application.
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

  //Loading counter value on start
  Future<void> _loadCounter() async {
    final prefs = await SharedPreferences.getInstance();
    setState(() {
      _counter = (prefs.getInt('counter') ?? 0);
    });
  }

  //Incrementing counter after click
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
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headline4,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ), // This trailing comma makes auto-formatting nicer for build methods.
    );
  }
}
```


[`shared_preferences`]: {{site.pub-pkg}}/shared_preferences
[shared preferences documentation]: {{site.android-dev}}/guide/topics/data/data-storage#pref
