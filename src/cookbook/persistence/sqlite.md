---
title: Persist data with SQLite
title: 用 SQLite 做資料持久化
description: How to use SQLite to store and retrieve data.
description: 如何使用 SQLite 做資料持久化。
tags: cookbook, 實用課程, 持久化
keywords: SQLite,資料庫
---

<?code-excerpt path-base="cookbook/persistence/sqlite/"?>

{{site.alert.note}}

  This guide uses the [sqflite package][].
  This package only supports apps that run on macOS, iOS, or Android.

  本範例使用了 [sqflite package][]。
  該 package 僅支援 macOS、iOS 和 Android。

{{site.alert.end}}

[sqflite package]: {{site.pub-pkg}}/sqflite

If you are writing an app that needs to persist and query large amounts of data on
the local device, consider using a database instead of a local file or
key-value store. In general, databases provide faster inserts, updates,
and queries compared to other local persistence solutions.

如果您正在編寫一個需要持久化且查詢大量本地裝置資料的 app，
可考慮採用資料庫，而不是本地資料夾或關鍵值庫。
總的來說，相比於其他本地持久化方案來說，
資料庫能夠提供更為迅速的插入、更新、查詢功能。

Flutter apps can make use of the SQLite databases via the
[`sqflite`][] plugin available on pub.dev.
This recipe demonstrates the basics of using `sqflite`
to insert, read, update, and remove data about various Dogs.

Flutter應用程式中可以透過 [`sqflite`][] package
來使用 SQLite 資料庫。
本文將透過使用 `sqflite` 來示範插入，讀取，更新，刪除各種狗狗的資料。

If you are new to SQLite and SQL statements, review the
[SQLite Tutorial][] to learn the basics before
completing this recipe.

如果你對於 SQLite 和 SQL 的各種陳述式還不熟悉，請檢視 SQLite 官方的課程
[SQLite 課程][SQLite Tutorial]，在檢視本文之前需要掌握基本的SQL陳述式。

This recipe uses the following steps:

總共有以下的步驟：

  1. Add the dependencies.

     新增依賴；

  2. Define the `Dog` data model.

     定義 `Dog (狗)` 資料模型；

  3. Open the database.

     開啟資料庫；

  4. Create the `dogs` table.

     建立 `dogs` 資料表；

  5. Insert a `Dog` into the database.

     將一條 `Dog` 資料插入資料庫；

  6. Retrieve the list of dogs.

     查詢所有狗狗的資料；

  7. Update a `Dog` in the database.

     更新（修改）一條 `Dog` 的資料；

  7. Delete a `Dog` from the database.

     刪除一條 `Dog` 的資料。

## 1. Add the dependencies

## 1. 新增依賴

To work with SQLite databases, import the `sqflite` and `path` packages.

為了使用 SQLite 資料庫，首先需要匯入 `sqflite` 和 `path` 這兩個 package。

  * The `sqflite` package provides classes and functions to
    interact with a SQLite database.

    `sqflite` 提供了豐富的類和方法，以便你能便捷實用 SQLite 資料庫。

  * The `path` package provides functions to
    define the location for storing the database on disk.

    `path` 提供了大量方法，以便你能正確的定義資料庫在磁碟上的儲存位置。

To add the packages as a dependency,
run `flutter pub add`:

執行 `flutter pub add` 將其新增為依賴：

```terminal
$ flutter pub add sqflite path
```

Make sure to import the packages in the file you'll be working in.

確保你已將 packages 匯入要使用的檔案中。

<?code-excerpt "lib/main.dart (imports)"?>
```dart
import 'dart:async';

import 'package:flutter/widgets.dart';
import 'package:path/path.dart';
import 'package:sqflite/sqflite.dart';
```

## 2. Define the Dog data model

## 2. 定義狗狗的資料模型

Before creating the table to store information on Dogs, take a few moments to
define the data that needs to be stored. For this example, define a Dog class
that contains three pieces of data:
A unique `id`, the `name`, and the `age` of each dog.

在你準備在新建的表裡儲存狗狗們的資訊的的時候，你需要先定義這些資料。
例如，定義一個狗類時，每一條狗狗的資料將包含三個欄位：
一個唯一的 `id` ；名字 `name` ；年齡 `age`。

<?code-excerpt "lib/step2.dart"?>
```dart
class Dog {
  final int id;
  final String name;
  final int age;

  const Dog({
    required this.id,
    required this.name,
    required this.age,
  });
}
```

## 3. Open the database

## 3. 開啟資料庫

Before reading and writing data to the database, open a connection
to the database. This involves two steps:

在你準備讀寫資料庫的資料之前，你要先開啟這個資料庫。
開啟一個數據庫有以下兩個步驟：

  1. Define the path to the database file using `getDatabasesPath()` from the
  `sqflite` package, combined with the `join` function from the `path` package.
  
     使用 `sqflite` package 裡的 `getDatabasesPath` 方法並配合 `path` package裡的
     `join` 方法定義資料庫的路徑。
     
  2. Open the database with the `openDatabase()` function from `sqflite`.
  
  	 使用 `sqflite` package 裡的 `openDatabase` 方法開啟資料庫。

{{site.alert.note}}
  In order to use the keyword `await`, the code must be placed
  inside an `async` function. You should place all the following
  table functions inside `void main() async {}`. 
{{site.alert.end}}

<?code-excerpt "lib/step3.dart (openDatabase)"?>
```dart
// Avoid errors caused by flutter upgrade.
// Importing 'package:flutter/widgets.dart' is required.
WidgetsFlutterBinding.ensureInitialized();
// Open the database and store the reference.
final database = openDatabase(
  // Set the path to the database. Note: Using the `join` function from the
  // `path` package is best practice to ensure the path is correctly
  // constructed for each platform.
  join(await getDatabasesPath(), 'doggie_database.db'),
);
```

## 4. Create the `dogs` table

## 4. 建立 `dogs` 表

Next, create a table to store information about various Dogs.
For this example, create a table called `dogs` that defines the data
that can be stored. Each `Dog` contains an `id`, `name`, and `age`.
Therefore, these are represented as three columns in the `dogs` table.

接下來，你需要建立一個表用以儲存各種狗狗的資訊。
在這個範例中，建立一個名為 `dogs` 資料庫表，它定義了可以被儲存的資料。
這樣，每條 `Dog` 資料就包含了一個 `id`， `name` 和 `age`。
因此，在 `dogs` 資料庫表中將有三列，分別是 `id`， `name` 和 `age`。


  1. The `id` is a Dart `int`, and is stored as an `INTEGER` SQLite
     Datatype. It is also good practice to use an `id` as the primary
     key for the table to improve query and update times.
     
     `id` 是 Dart 的 `int` 型別，在資料表中是 SQLite 的 `INTEGER` 資料型別。
     最佳實踐是將 `id` 作為資料庫表的主鍵，用以改善查詢和修改的時間。
     
  2. The `name` is a Dart `String`, and is stored as a `TEXT` SQLite
     Datatype.
     
     `name` 是Dart的 `String`型別，在資料表中是SQLite的 `TEXT` 資料型別。
     
  3. The `age` is also a Dart `int`, and is stored as an `INTEGER`
     Datatype.
     
     `age` 也是Dart的 `int` 型別，在資料表中是SQLite的 `INTEGER` 資料型別。

For more information about the available Datatypes that can be stored in a
SQLite database, see the [official SQLite Datatypes documentation][].

關於 SQLite 資料庫能夠儲存的更多的資料型別資訊請查閱官方的
[SQLite Datatypes 文件](https://www.sqlite.org/datatype3.html)。

<?code-excerpt "lib/main.dart (openDatabase)"?>
```dart
final database = openDatabase(
  // Set the path to the database. Note: Using the `join` function from the
  // `path` package is best practice to ensure the path is correctly
  // constructed for each platform.
  join(await getDatabasesPath(), 'doggie_database.db'),
  // When the database is first created, create a table to store dogs.
  onCreate: (db, version) {
    // Run the CREATE TABLE statement on the database.
    return db.execute(
      'CREATE TABLE dogs(id INTEGER PRIMARY KEY, name TEXT, age INTEGER)',
    );
  },
  // Set the version. This executes the onCreate function and provides a
  // path to perform database upgrades and downgrades.
  version: 1,
);
```

## 5. Insert a Dog into the database

## 5. 插入一條狗狗的資料

Now that you have a database with a table suitable for storing information
about various dogs, it's time to read and write data.

現在你已經準備好了一個數據庫用於儲存各種狗狗的資訊資料，現在開始讀寫資料咯。

First, insert a `Dog` into the `dogs` table. This involves two steps:

首先，在 `dogs` 資料表中插入一條 `Dog` 資料。分以下兩步：

1. Convert the `Dog` into a `Map`
  
   把 `Dog` 轉換成一個 `Map` 資料型別；
     
2. Use the [`insert()`][] method to store the
   `Map` in the `dogs` table.
  
   使用 [`insert()`][] 方法把 `Map` 儲存到 `dogs` 資料表中。

<?code-excerpt "lib/main.dart (Dog)"?>
```dart
class Dog {
  final int id;
  final String name;
  final int age;

  const Dog({
    required this.id,
    required this.name,
    required this.age,
  });

  // Convert a Dog into a Map. The keys must correspond to the names of the
  // columns in the database.
  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'name': name,
      'age': age,
    };
  }

  // Implement toString to make it easier to see information about
  // each dog when using the print statement.
  @override
  String toString() {
    return 'Dog{id: $id, name: $name, age: $age}';
  }
}
```

<?code-excerpt "lib/main.dart (insertDog)"?>
```dart
// Define a function that inserts dogs into the database
Future<void> insertDog(Dog dog) async {
  // Get a reference to the database.
  final db = await database;

  // Insert the Dog into the correct table. You might also specify the
  // `conflictAlgorithm` to use in case the same dog is inserted twice.
  //
  // In this case, replace any previous data.
  await db.insert(
    'dogs',
    dog.toMap(),
    conflictAlgorithm: ConflictAlgorithm.replace,
  );
}
```

<?code-excerpt "lib/main.dart (fido)"?>
```dart
// Create a Dog and add it to the dogs table
var fido = const Dog(
  id: 0,
  name: 'Fido',
  age: 35,
);

await insertDog(fido);
```

## 6. Retrieve the list of Dogs

## 6. 查詢狗狗列表

Now that a `Dog` is stored in the database, query the database
for a specific dog or a list of all dogs. This involves two steps:

現在已經有了一條 `Dog` 儲存在資料庫裡。
你可以透過查詢資料庫，檢索到一隻狗狗的資料或者所有狗狗的資料。分為以下兩步:

  1. Run a `query` against the `dogs` table. This returns a `List<Map>`.
  
     呼叫 `dogs` 表對像的 `query` 方法。這將返回一個`List <Map>`。
     
  2. Convert the `List<Map>` into a `List<Dog>`.
  
     將 `List<Map>` 轉換成 `List<Dog>` 資料型別。

<?code-excerpt "lib/main.dart (dogs)"?>
```dart
// A method that retrieves all the dogs from the dogs table.
Future<List<Dog>> dogs() async {
  // Get a reference to the database.
  final db = await database;

  // Query the table for all The Dogs.
  final List<Map<String, dynamic>> maps = await db.query('dogs');

  // Convert the List<Map<String, dynamic> into a List<Dog>.
  return List.generate(maps.length, (i) {
    return Dog(
      id: maps[i]['id'] as int,
      name: maps[i]['name'] as String,
      age: maps[i]['age'] as int,
    );
  });
}
```

<?code-excerpt "lib/main.dart (print)"?>
```dart
// Now, use the method above to retrieve all the dogs.
print(await dogs()); // Prints a list that include Fido.
```

## 7. Update a `Dog` in the database

## 7. 修改一條 `Dog` 資料

After inserting information into the database,
you might want to update that information at a later time.
You can do this by using the [`update()`][]
method from the `sqflite` library.

使用 `sqflite` package 中的 [`update()`][]方法，
可以對已經插入到資料庫中的資料進行修改（更新）。

This involves two steps:

修改資料操作包含以下兩步：

  1. Convert the Dog into a Map.
  
     將一條狗狗的資料轉換成 `Map` 資料型別；
  
  2. Use a `where` clause to ensure you update the correct Dog.
     
     使用  `where` 陳述式定位到具體將要被修改的資料。

<?code-excerpt "lib/main.dart (update)"?>
```dart
Future<void> updateDog(Dog dog) async {
  // Get a reference to the database.
  final db = await database;

  // Update the given Dog.
  await db.update(
    'dogs',
    dog.toMap(),
    // Ensure that the Dog has a matching id.
    where: 'id = ?',
    // Pass the Dog's id as a whereArg to prevent SQL injection.
    whereArgs: [dog.id],
  );
}
```

<?code-excerpt "lib/main.dart (update2)"?>
```dart
// Update Fido's age and save it to the database.
fido = Dog(
  id: fido.id,
  name: fido.name,
  age: fido.age + 7,
);
await updateDog(fido);

// Print the updated results.
print(await dogs()); // Prints Fido with age 42.
```

{{site.alert.warning}}

  Always use `whereArgs` to pass arguments to a `where` statement.
  This helps safeguard against SQL injection attacks.

  使用 `whereArgs` 將引數傳遞給 `where` 陳述式。有助於防止 SQL 注入攻擊。

  Do not use string interpolation, such as `where: "id = ${dog.id}"`!
  
  這裡不要使用字串範本，比如： `where: "id = ${dog.id}"`！
  
{{site.alert.end}}


## 8. Delete a `Dog` from the database

## 8. 刪除一條 `Dog` 的資料

In addition to inserting and updating information about Dogs,
you can also remove dogs from the database. To delete data,
use the [`delete()`][] method from the `sqflite` library.

除了插入和修改狗狗們的資料，你還可以從資料庫中刪除狗狗的資料。
刪除資料用到了 `sqflite` package 中的 [`delete()`][] 方法。

In this section, create a function that takes an id and deletes the dog with
a matching id from the database. To make this work, you must provide a `where`
clause to limit the records being deleted.

在這一小節，新建一個方法用來接收一個 id 並且刪除資料庫中與這個 id 匹配的那一條資料。
為了達到這個目的，你必須使用 `where` 陳述式限定哪一條才是被刪除的資料。


<?code-excerpt "lib/main.dart (deleteDog)"?>
```dart
Future<void> deleteDog(int id) async {
  // Get a reference to the database.
  final db = await database;

  // Remove the Dog from the database.
  await db.delete(
    'dogs',
    // Use a `where` clause to delete a specific dog.
    where: 'id = ?',
    // Pass the Dog's id as a whereArg to prevent SQL injection.
    whereArgs: [id],
  );
}
```

## Example

## 範例

To run the example:

執行範例需要以下幾步：

  1. Create a new Flutter project.

     建立一個新的 Flutter 工程；

  2. Add the `sqflite` and `path` packages to your `pubspec.yaml`.

     將 `sqflite` 和 `path` 包新增到 `pubspec.yaml` 檔案裡；

  3. Paste the following code into a new file called `lib/db_test.dart`.

     將以下程式碼貼上在 `lib/db_test.dart` 檔案裡（若無則新建，若有則覆蓋）；

  4. Run the code with `flutter run lib/db_test.dart`.

     執行 `flutter run lib/db_test.dart`。

<?code-excerpt "lib/main.dart"?>
```dart
import 'dart:async';

import 'package:flutter/widgets.dart';
import 'package:path/path.dart';
import 'package:sqflite/sqflite.dart';

void main() async {
  // Avoid errors caused by flutter upgrade.
  // Importing 'package:flutter/widgets.dart' is required.
  WidgetsFlutterBinding.ensureInitialized();
  // Open the database and store the reference.
  final database = openDatabase(
    // Set the path to the database. Note: Using the `join` function from the
    // `path` package is best practice to ensure the path is correctly
    // constructed for each platform.
    join(await getDatabasesPath(), 'doggie_database.db'),
    // When the database is first created, create a table to store dogs.
    onCreate: (db, version) {
      // Run the CREATE TABLE statement on the database.
      return db.execute(
        'CREATE TABLE dogs(id INTEGER PRIMARY KEY, name TEXT, age INTEGER)',
      );
    },
    // Set the version. This executes the onCreate function and provides a
    // path to perform database upgrades and downgrades.
    version: 1,
  );

  // Define a function that inserts dogs into the database
  Future<void> insertDog(Dog dog) async {
    // Get a reference to the database.
    final db = await database;

    // Insert the Dog into the correct table. You might also specify the
    // `conflictAlgorithm` to use in case the same dog is inserted twice.
    //
    // In this case, replace any previous data.
    await db.insert(
      'dogs',
      dog.toMap(),
      conflictAlgorithm: ConflictAlgorithm.replace,
    );
  }

  // A method that retrieves all the dogs from the dogs table.
  Future<List<Dog>> dogs() async {
    // Get a reference to the database.
    final db = await database;

    // Query the table for all The Dogs.
    final List<Map<String, dynamic>> maps = await db.query('dogs');

    // Convert the List<Map<String, dynamic> into a List<Dog>.
    return List.generate(maps.length, (i) {
      return Dog(
        id: maps[i]['id'] as int,
        name: maps[i]['name'] as String,
        age: maps[i]['age'] as int,
      );
    });
  }

  Future<void> updateDog(Dog dog) async {
    // Get a reference to the database.
    final db = await database;

    // Update the given Dog.
    await db.update(
      'dogs',
      dog.toMap(),
      // Ensure that the Dog has a matching id.
      where: 'id = ?',
      // Pass the Dog's id as a whereArg to prevent SQL injection.
      whereArgs: [dog.id],
    );
  }

  Future<void> deleteDog(int id) async {
    // Get a reference to the database.
    final db = await database;

    // Remove the Dog from the database.
    await db.delete(
      'dogs',
      // Use a `where` clause to delete a specific dog.
      where: 'id = ?',
      // Pass the Dog's id as a whereArg to prevent SQL injection.
      whereArgs: [id],
    );
  }

  // Create a Dog and add it to the dogs table
  var fido = const Dog(
    id: 0,
    name: 'Fido',
    age: 35,
  );

  await insertDog(fido);

  // Now, use the method above to retrieve all the dogs.
  print(await dogs()); // Prints a list that include Fido.

  // Update Fido's age and save it to the database.
  fido = Dog(
    id: fido.id,
    name: fido.name,
    age: fido.age + 7,
  );
  await updateDog(fido);

  // Print the updated results.
  print(await dogs()); // Prints Fido with age 42.

  // Delete Fido from the database.
  await deleteDog(fido.id);

  // Print the list of dogs (empty).
  print(await dogs());
}

class Dog {
  final int id;
  final String name;
  final int age;

  const Dog({
    required this.id,
    required this.name,
    required this.age,
  });

  // Convert a Dog into a Map. The keys must correspond to the names of the
  // columns in the database.
  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'name': name,
      'age': age,
    };
  }

  // Implement toString to make it easier to see information about
  // each dog when using the print statement.
  @override
  String toString() {
    return 'Dog{id: $id, name: $name, age: $age}';
  }
}
```


[`delete()`]: {{site.pub-api}}/sqflite_common/latest/sqlite_api/DatabaseExecutor/delete.html
[`insert()`]: {{site.pub-api}}/sqflite_common/latest/sqlite_api/DatabaseExecutor/insert.html
[`sqflite`]: {{site.pub-pkg}}/sqflite
[SQLite Tutorial]: http://www.sqlitetutorial.net/
[official SQLite Datatypes documentation]: https://www.sqlite.org/datatype3.html
[`update()`]: {{site.pub-api}}/sqflite_common/latest/sqlite_api/DatabaseExecutor/update.html
