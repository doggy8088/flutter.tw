---
title: Hot reload
title: 熱重載 (Hot reload)
description: Speed up development using Flutter's hot reload feature.
description: 使用熱重載提高你的開發速度和體驗。
tags: SDK,Flutter SDK
keywords: 熱重載,效率提升,widget渲染
---

<?code-excerpt path-base="development/tools"?>

Flutter's hot reload feature helps you quickly and
easily experiment, build UIs, add features, and fix bugs.
Hot reload works by injecting updated source code files
into the running [Dart Virtual Machine (VM)][].
After the VM updates classes with the new versions of fields and functions,
the Flutter framework automatically rebuilds the widget tree,
allowing you to quickly view the effects of your changes.

Flutter 的熱重載功能可幫助你在無需重新啟動應用程式的情況下
快速、輕鬆地測試、建構使用者介面、新增功能以及修復錯誤。
透過將更新的原始碼檔案注入到正在執行的
[Dart 虛擬機器（VM）][Dart Virtual Machine (VM)] 來實現熱重載。
在虛擬機器使用新的欄位和函式更新類之後，
Flutter 框架會自動重新建構 widget 樹，以便你可以快速檢視更改的效果。

## How to perform a hot reload

## 如何進行熱重載

To hot reload a Flutter app:

想要熱重載 Flutter 應用：

1. Run the app from a supported [Flutter editor][] or a terminal window.
   Either a physical or virtual device can be the target.
   **Only Flutter apps in debug mode can be hot reloaded or hot restarted.**

   在支援 [Flutter 編輯器][Flutter editor]
   或終端視窗執行應用程式，物理機或虛擬器都可以。
   **Flutter 應用程式只有在 DEBUG 模式下才能執行熱重載或者熱重啟。**

1. Modify one of the Dart files in your project.
   Most types of code changes can be hot reloaded;
   for a list of changes that require a hot restart,
   see [Special cases](#special-cases).

   修改專案中的一個 Dart 檔案。
   大多數型別的程式碼更改可以熱重載，然而一些 [特別情況](#special-cases)
   需要熱重啟應用程式以生效。

1. If you're working in an IDE/editor that supports Flutter's IDE tools,
   select **Save All** (`cmd-s`/`ctrl-s`),
   or click the hot reload button on the toolbar.

   如果你在支援 Flutter 的 IDE 或編輯器中工作，
   請選擇 **Save All** (`Command + S`/`Ctrl + S`)，
   或單擊工具欄上的 Hot Reload 按鈕。

   If you're running the app at the command line using `flutter run`,
   enter `r` in the terminal window.

   如果你正在使用命令列 `flutter run` 執行應用程式，請在終端視窗輸入 `r`。

After a successful hot reload operation,
you'll see a message in the console similar to:

成功執行熱重載後，你將在控制檯中看到類似於以下內容的訊息：

```
Performing hot reload...
Reloaded 1 of 448 libraries in 978ms.
```

The app updates to reflect your change,
and the current state of the app is preserved.
Your app continues to execute from where it was prior
to run the hot reload command.
The code updates and execution continues.

應用程式將以你的更改進行更新，並保留應用程式當前的狀態。
你的應用程式將繼續從之前執行熱重載命令的位置開始執行。程式碼被更新並繼續執行。

{{site.alert.secondary}}

  **What is the difference between hot reload, hot restart,
  and full restart?**

  **熱重載、熱重啟和完全重啟之間有什麼區別？**<br>

  * **Hot reload** loads code changes into the VM and re-builds
    the widget tree, preserving the app state;
    it doesn't rerun `main()` or `initState()`.
    (`⌘\` in Intellij and Android Studio, `⌃F5` in VSCode)

    **熱重載** 會將程式碼更改轉入 VM，重建 widget 樹並保持應用的狀態，
    整個過程不會重新執行 `main()` 或者 `initState()`。
    （在 IDEA 中的快捷鍵是 `⌘\`，在 VSCode 中是 `⌃F5`）

  * **Hot restart** loads code changes into the VM,
    and restarts the Flutter app, losing the app state.
    (`⇧⌘\` in IntelliJ and Android Studio, `⇧⌘F5` in VSCode)

    **熱重啟** 會將程式碼更改轉入 VM，重啟 Flutter 應用，不保留應用狀態。
    （在 IDEA 中的快捷鍵是 `⇧⌘\`，在 VSCode 中是 `⇧⌘F5`）

  * **Full restart** restarts the iOS, Android, or web app.
    This takes longer because it also recompiles the
    Java / Kotlin / ObjC / Swift code. On the web,
    it also restarts the Dart Development Compiler.
    There is no specific keyboard shortcut for this;
    you need to stop and start the run configuration.

    **完全重啟** 將會完全重新執行應用。
    該處理序較為耗時，因為它會重新編譯原生部分程式碼。
    在 Web 平臺上，它同時會重啟 Dart 開發編譯器。
    完全重啟並沒有既定的快捷鍵，你需要手動停止後重新執行。

  Flutter web currently supports hot restart but not
  hot reload.

  Flutter web 目前僅支援熱重啟，不支援熱重載。

{{site.alert.end}}

![Android Studio UI]({{site.url}}/assets/images/docs/development/tools/android-studio-run-controls.png){:width="100%"}<br>
<p>Controls for run, run debug, hot reload, and hot restart in Android Studio</p>
<p>Android Studio 中的執行、執行除錯、熱重載和熱重啟的控制項位置</p>

A code change has a visible effect only if the modified
Dart code is run again after the change. Specifically,
a hot reload causes all the existing widgets to rebuild.
Only code involved in the rebuilding of the widgets
is automatically re-executed. The `main()` and `initState()`
functions, for example, are not run again.

只有修改後的 Dart 程式碼再次執行時，程式碼更改才會產生可見效果。
具體來說，熱重載會導致所有現有的 widgets 重新建構。
只有與 widgets 重新建構相關的程式碼才會自動重新執行。
`main()` and `initState()` 方法則不會再次執行。

## Special cases

## 特別情況

The next sections describe specific scenarios that involve
hot reload. In some cases, small changes to the Dart code
enable you to continue using hot reload for your app.
In other cases, a hot restart, or a full restart is needed.

下面的部分會描述一些熱重載的特別的情況。
在某些情況下，對 Dart 程式碼的小改動將確保你能夠繼續使用熱重載。
在其他情況下，需要熱重啟或完全重啟。

### An app is killed

### 應用被強制停止

Hot reload can break when the app is killed.
For example, if the app was in the background for too long.

熱重載會在應用被強制停止之後斷開連線。
比如一直在後台執行的應用（會被系統強制停止）。

### Compilation errors

### 編譯錯誤

When a code change introduces a compilation error,
hot reload generates an error message similar to:

當代碼更改導致編譯錯誤時，熱重載會產生類似於以下內容的錯誤訊息：

```nocode
Hot reload was rejected:
'/path/to/project/lib/main.dart': warning: line 16 pos 38: unbalanced '{' opens here
  Widget build(BuildContext context) {
                                     ^
'/path/to/project/lib/main.dart': error: line 33 pos 5: unbalanced ')'
    );
    ^
```

In this situation, simply correct the errors on the
specified lines of Dart code to keep using hot reload.

在這種情況下，只需更正上述程式碼的錯誤，即可以繼續使用熱重載。

### CupertinoTabView's builder

Hot reload won't apply changes made to
a `builder` of a `CupertinoTabView`.
For more information, see [Issue 43574][].

熱重載對 `CupertinoTabView` 的 `builder` 不起作用。
你可以檢視 [Issue 43574][] 瞭解更多細節。

### Enumerated types

### 列舉型別

Hot reload doesn't work when enumerated types are
changed to regular classes or regular classes are
changed to enumerated types.

在列舉型別與普通的類定義互相轉換時，熱重載無法生效。

For example:

例如：

Before the change:

更改前：

<?code-excerpt "lib/hot-reload/before.dart (Enum)"?>
```dart
enum Color {
  red,
  green,
  blue,
}
```

After the change:

更改後：

<?code-excerpt "lib/hot-reload/after.dart (Enum)"?>
```dart
class Color {
  Color(this.i, this.j);
  final int i;
  final int j;
}
```

### Generic types

### 泛型

Hot reload won't work when generic type declarations
are modified. For example, the following won't work:

在泛型發生改變時，熱重載不會生效。下面的例子將不會有效果：

Before the change:

更改前：

<?code-excerpt "lib/hot-reload/before.dart (Class)"?>
```dart
class A<T> {
  T? i;
}
```

After the change:

更改後：

<?code-excerpt "lib/hot-reload/after.dart (Class)"?>
```dart
class A<T, V> {
  T? i;
  V? v;
}
```

### Native code

### 原生程式碼

If you've changed native code (such as Kotlin, Java, Swift,
or Objective-C), you must perform a full restart (stop and
restart the app) to see the changes take effect.

如果你更改了原生程式碼（例如 Kotlin、Java、Swift 或 Objective-C），
你必須要進行完全重啟（停止後重新執行應用）才能讓更改生效。

### Previous state is combined with new code

### 新的程式碼與舊的狀態結合

Flutter's stateful hot reload preserves the state of your app.
This approach enables you to view the effect of the most
recent change only, without throwing away the current state.
For example, if your app requires a user to log in,
you can modify and hot reload a page several levels down in
the navigation hierarchy, without re-entering your login credentials.
State is kept, which is usually the desired behavior.

Flutter 有狀態的熱重載將保持你的應用的狀態。
這項特性讓你能夠在不丟失狀態的情況下，預覽程式碼作出的改動。
例如，如果你的應用需要使用者登入，你可以調整路由相關的內容重載幾次，
而不需要重新進入登入流程。過程中狀態是保持的，一般與預期相符。

If code changes affect the state of your app (or its dependencies),
the data your app has to work with might not be fully consistent
with the data it would have if it executed from scratch.
The result might be different behavior after a hot reload
versus a hot restart.

如果程式碼改動會影響你的應用的狀態（或應用的依賴），
則應用里正在使用的資料可能與從一開始執行的資料不完全一致。
熱重載和熱重啟的結果可能不一致。

### Recent code change is included but app state is excluded

In Dart, [static fields are lazily initialized][static-variables].
This means that the first time you run a Flutter app and a
static field is read, it's set to whatever value its
initializer was evaluated to.
Global variables and static fields are treated as state,
and are therefore not reinitialized during hot reload.

在 Dart 中，[靜態欄位是延遲初始化的][static-variables]。
這意味著第一次執行 Flutter 應用程式並讀取靜態欄位時，
會將靜態欄位的值設為其初始表示式的結果。
全域變數和靜態欄位都被視為狀態，因此在熱重載期間不會重新初始化。

If you change initializers of global variables and static fields,
a hot restart or restart the state where the initializers are hold
is necessary to see the changes.
For example, consider the following code:

如果你改變了全域變數或靜態欄位的初始化內容，你需要重新

如果更改全域變數和靜態欄位的初始化陳述式，則需要完全重啟以檢視更改。
例如，參考以下程式碼：

<?code-excerpt "lib/hot-reload/before.dart (SampleTable)"?>
```dart
final sampleTable = [
  Table(
    children: const [
      TableRow(
        children: [Text('T1')],
      )
    ],
  ),
  Table(
    children: const [
      TableRow(
        children: [Text('T2')],
      )
    ],
  ),
  Table(
    children: const [
      TableRow(
        children: [Text('T3')],
      )
    ],
  ),
  Table(
    children: const [
      TableRow(
        children: [Text('T4')],
      )
    ],
  ),
];
```

After running the app, you make the following change:

執行應用程式後，如果進行以下更改:

<?code-excerpt "lib/hot-reload/after.dart (SampleTable)"?>
```dart
final sampleTable = [
  Table(
    children: const [
      TableRow(
        children: [Text('T1')],
      )
    ],
  ),
  Table(
    children: const [
      TableRow(
        children: [Text('T2')],
      )
    ],
  ),
  Table(
    children: const [
      TableRow(
        children: [Text('T3')],
      )
    ],
  ),
  Table(
    children: const [
      TableRow(
        children: [Text('T10')], // modified
      )
    ],
  ),
];
```

You hot reload, but the change is not reflected.

熱重載後，這個改變並沒有產生效果。

Conversely, in the following example:

相反，在下面範例中：

<?code-excerpt "lib/hot-reload/before.dart (Const)"?>
```dart
const foo = 1;
final bar = foo;
void onClick() {
  print(foo);
  print(bar);
}
```

Running the app for the first time prints `1` and `1`.
Then, you make the following change:

第一次執行應用程式會列印 `1` 和 `1`。然後，如果你進行以下更改：

<?code-excerpt "lib/hot-reload/after.dart (Const)"?>
```dart
const foo = 2; // modified
final bar = foo;
void onClick() {
  print(foo);
  print(bar);
}
```

While changes to `const` field values are always hot reloaded,
the static field initializer is not rerun. Conceptually,
`const` fields are treated like aliases instead of state.

雖然對 `const` 定義的欄位值的更改始終會重新載入，
但不會重新執行靜態欄位的初始化陳述式。
從概念上講，`const` 欄位被視為別名而不是狀態。

The Dart VM detects initializer changes and flags when a set
of changes needs a hot restart to take effect.
The flagging mechanism is triggered for
most of the initialization work in the above example,
but not for cases like the following:

Dart VM 在一組更改需要完全重啟才能生效時，會檢測初始化程式更改和標誌。
在上面的範例中，大部分初始化工作都會觸發標記機制，但不適用於以下情況：

<?code-excerpt "lib/hot-reload/after.dart (FinalFoo)"?>
```dart
final bar = foo;
```

To update `foo` and view the change after hot reload,
consider redefining the field as `const` or using a getter to
return the value, rather than using `final`.
For example, either of the following solutions work:

為了能夠更改 `foo` 並在熱重載後檢視更改，
應該將欄位重新用 `const` 定義或使用 getter 來返回值，而不是使用 `final`。
下面的解決方案均可使用：

<!-- skip -->
```dart
const bar = foo;
```

or:

或者：

<?code-excerpt "lib/hot-reload/foo_const.dart (Const)"?>
```dart
const foo = 1;
const bar = foo; // Convert foo to a const...
void onClick() {
  print(foo);
  print(bar);
}
```

<?code-excerpt "lib/hot-reload/getter.dart (Const)"?>
```dart
const foo = 1;
int get bar => foo; // ...or provide a getter.
void onClick() {
  print(foo);
  print(bar);
}
```

For more information, read about the [differences
between the `const` and `final` keywords][const-new] in Dart.

你可以閱讀 Dart 中 [`const` 和 `final` 關鍵字的區別][const-new] 瞭解更多。

### Recent UI change is excluded

### 使用者介面沒有改變

Even when a hot reload operation appears successful and generates no
exceptions, some code changes might not be visible in the refreshed UI.
This behavior is common after changes to the app's `main()` or
`initState()` methods.

即使熱重載操作看起來成功了並且沒有丟擲例外，
但某些程式碼更改可能在更新的 UI 中不可見。
這種行為在更改應用程式的 `main()` 方法後很常見。

As a general rule, if the modified code is downstream of the root
widget's `build()` method, then hot reload behaves as expected.
However, if the modified code won't be re-executed as a result
of rebuilding the widget tree, then you won't
see its effects after hot reload.

作為一般規則，如果修改後的程式碼位於根 widget 的
`build()` 方法的下游，則熱重載將按預期執行。
但是，如果修改後的程式碼不會因重新建構 widget 樹而重新執行的話，
那麼在熱重載後你將看不到它更改後的效果。

For example, consider the following code:

例如，參考以下程式碼：

<?code-excerpt "lib/hot-reload/before.dart (Build)"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return GestureDetector(onTap: () => print('tapped'));
  }
}
```

After running this app, change the code as follows:

執行應用程式後，你可能會像如下範例更改程式碼:

<?code-excerpt "lib/hot-reload/after.dart (Main)"?>
```dart
import 'package:flutter/widgets.dart';

void main() {
  runApp(const Center(child: Text('Hello', textDirection: TextDirection.ltr)));
}
```

With a hot restart, the program starts from the beginning,
executes the new version of `main()`,
and builds a widget tree that displays the text `Hello`.

如果你進行了完全重啟，程式會從頭開始執行新的 `main()` 方法，
並建構一個 widget 樹來顯示文字 `Hello`。

However, if you hot reload the app after this change,
`main()` and `initState()` are not re-executed,
and the widget tree is rebuilt with the unchanged instance
of `MyApp` as the root widget.
This results in no visible change after hot reload.

但是，如果你在更改後是透過熱重載執行，
`main()` 和 `initState()` 方法不會重新執行，
並且會使用未修改的 `MyApp` 例項作為根 widget 樹來建構新的 widget 樹，
熱重載後結果沒有變化。

## How it works

## 熱重載的原理

When hot reload is invoked, the host machine looks
at the edited code since the last compilation.
The following libraries are recompiled:

呼叫熱重載時，主機會檢視自上次編譯以來編輯的程式碼。重新編譯以下檔案：

* Any libraries with changed code
 
  任何有程式碼更改的檔案

* The application's main library

  應用程式的主入口檔案

* The libraries from the main library leading
  to affected libraries

  受主入口檔案影響的檔案

The source code from those libraries is compiled into
[kernel files][] and sent to the mobile device's Dart VM.

這些庫中的原始碼被編譯為 [核心檔案][kernel files]，
併發送到移動裝置的 Dart VM 中。

The Dart VM re-loads all libraries from the new kernel file.
So far no code is re-executed.

Dart VM 重新載入新核心檔案中的所有檔案。
到這一步為止，沒有重新執行任何程式碼。

The hot reload mechanism then causes the Flutter framework
to trigger a rebuild/re-layout/repaint of all existing
widgets and render objects.

最後，熱重載機制在 Flutter 框架中觸發所有現有的
widget 和渲染物件的重建/重新佈局/重繪 (reassemble)。

[static-variables]: {{site.dart-site}}/language/classes#static-variables
[const-new]: {{site.dart-site}}/language/variables#final-and-const
[Dart Virtual Machine (VM)]: {{site.dart-site}}/overview#platform
[Flutter editor]: {{site.url}}/get-started/editor
[Issue 43574]: {{site.repo.flutter}}/issues/43574
[kernel files]: {{site.github}}/dart-lang/sdk/tree/main/pkg/kernel
