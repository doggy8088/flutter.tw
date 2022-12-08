---
title: Debugging Flutter apps programmatically
title: 新增輸出程式碼的方式除錯 Flutter 應用
description: How to enable various debugging tools from your code and at the command line.
description: 如何透過在程式碼中加入 log 輸出。
tags: Flutter測試
keywords: Flutter除錯,Flutter加log,Flutter輸出
---

<?code-excerpt path-base="testing/code_debugging"?>

This doc describes debugging features that you can enable in code.
For a full list of debugging and profiling tools, see the
[Debugging][] page.

這篇文章描述瞭如何在程式碼中啟用除錯功能。
如果想了解整個除錯和分析工具，可參見 [Debugging][] 頁面.

## Logging

## 日誌輸出

{{site.alert.note}}

  You can view logs in DevTools' [Logging view][]
  or in your system console. This sections
  shows how to set up your logging statements.

  您可以在 DevTools 的 [Logging view][] 或系統控制檯檢視日誌。 
  本節將展示如何設定日誌的相關陳述式。

{{site.alert.end}}

You have two options for logging for your application.
The first is to use `stdout` and `stderr`.
Generally, this is done using `print()` statements,
or by importing `dart:io` and invoking methods on
`stderr` and `stdout`. For example:

在應用中有兩種日誌輸出方式。第一種方式是使用 `stdout` 和 `stderr`。
通常，我們使用 `print()` 陳述式或者透過引入 `dart:io` 
並且呼叫 `stderr` 與 `stdout` 中的方法。如下：

<?code-excerpt "lib/main.dart (stderr)"?>
```dart
stderr.writeln('print me');
```

If you output too much at once, then Android sometimes
discards some log lines. To avoid this, use [`debugPrint()`][],
from Flutter's `foundation` library. This is a wrapper around `print`
that throttles the output to a level that avoids being dropped by
Android's kernel.

如果您一次輸出太多，Android 有時可能會丟失一些日誌行。
可以使用 Flutter 的 `foundation` 套件中的 [`debugPrint()`][] 方法來避免這個問題。
它封裝了 `print` 方法，透過控制輸出的等級，從而避免輸出內容被 Android 的核心丟棄。

The other option for application logging is to use the
`dart:developer` [`log()`][] function. This allows you to include a
bit more granularity and information in the logging output.
Here's an example:

另一種應用日誌輸出的方式是使用 `dart:developer` 中的 [`log()`][] 方法。
透過這種方式，您可以在輸出日誌中包含更精細化的資訊。如下面這個範例：

<?code-excerpt "lib/main.dart (log)"?>
```dart
import 'dart:developer' as developer;

void main() {
  developer.log('log me', name: 'my.app.category');

  developer.log('log me 1', name: 'my.other.category');
  developer.log('log me 2', name: 'my.other.category');
}
```

You can also pass application data to the log call.
The convention for this is to use the `error:` named
parameter on the `log()` call, JSON encode the object
you want to send, and pass the encoded string to the
error parameter.

您也可以在列印日誌時傳入應用資料。
通常，在呼叫 `log()` 時也會使用命名引數 `error:`，
您可以透過 JSON 編碼想要傳入的物件，並將編碼後的字串傳給 error 引數。

<?code-excerpt "lib/app_data.dart (PassAppData)"?>
```dart
import 'dart:convert';
import 'dart:developer' as developer;

void main() {
  var myCustomObject = MyCustomObject();

  developer.log(
    'log me',
    name: 'my.app.category',
    error: jsonEncode(myCustomObject),
  );
}
```

If viewing the logging output in DevTool's logging view,
the JSON encoded error param is interpreted as a data object
and rendered in the details view for that log entry.

如果在 DevTool 的 logging 頁面中檢視日誌輸出情況，
JSON 編碼的錯誤引數會被解釋為一個數據物件，並呈現在該日誌條目的 details 檢視中。

## Setting breakpoints

## 設定斷點

{{site.alert.note}}

  You can set breakpoints in DevTools' [Debugger][], or
  in the built-in debugger of your IDE. If you want to
  set breakpoints programmatically, use the following
  instructions.

  您可以在 DevTools 的 [Debugger][] 頁面或在 IDE 的內建偵錯程式中設定斷點。
  如果您想要以程式設計方式設定斷點，可以使用下面的指令。

{{site.alert.end}}

You can insert programmatic breakpoints using the
`debugger()` statement. To use this, you have to
import the `dart:developer` package at the top of
the relevant file.

您可以使用 `debugger()` 陳述式插入程式設計式斷點。在此之前，
您需要在相關檔案頂部引入 `dart:developer` 套件。

The `debugger()` statement takes an optional `when`
argument that you can specify to only break when a
certain condition is true, as in the following example:

`debugger()` 陳述式有一個可選引數 `when`，
用來指定該斷點觸發的特定條件，如下這個範例：

<?code-excerpt "lib/debugger.dart"?>
```dart
import 'dart:developer';

void someFunction(double offset) {
  debugger(when: offset > 30.0);
  // ...
}
```

## Debug flags: application layers

## Debug 標識： 應用程式層

{% comment %}  DevTool's doesn't currently print the render tree.
{{site.alert.note}}

  If you use [DevTools][] and its [Flutter inspector][] to
  view a visual layout of the render tree, you probably won't
  need to use these text-based dump tools.

  如果您使用 [DevTools][] 和它的 [Flutter inspector][] 來檢視渲染樹的可視佈局，
  可能不需要使用這些基於文字的轉儲工具。

{{site.alert.end}}
{% endcomment %}

Each layer of the Flutter framework provides a function to dump its
current state or events to the console (using `debugPrint`).

Flutter 框架的每個 layer 都提供了一個函式，
用來將其當前狀態或事件轉儲到控制檯（使用 `debugPrint`）。

### Widget tree

### Widget 樹

To dump the state of the Widgets library, call [`debugDumpApp()`][].
You can call this more or less any time that the application is not in
the middle of running a build sphae (in other words, not anywhere inside a
`build()` method), if the app has built at least once and is in debug mode
(in other words, any time after calling `runApp()`).

可以透過呼叫 [`debugDumpApp()`][] 方法轉儲 widget 庫的狀態，
如果應用已至少建構了一次，並且正處於除錯模式時（`runApp()` 呼叫後的任何時間）。
只要應用不在執行建構階段，您可以呼叫隨意該方法（也就是說，不能在 `build()` 方法中使用它）。

For example, the following application:

如下面這個應用：

<?code-excerpt "lib/dump_app.dart"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    const MaterialApp(
      home: AppHome(),
    ),
  );
}

class AppHome extends StatelessWidget {
  const AppHome({super.key});

  @override
  Widget build(BuildContext context) {
    return Material(
      child: Center(
        child: TextButton(
          onPressed: () {
            debugDumpApp();
          },
          child: const Text('Dump App'),
        ),
      ),
    );
  }
}
```

The previous app outputs something like the following
(the precise details vary by the version of the framework,
the size of the device, and so forth):

上面應用的輸出內容如下（具體細節因框架版本、裝置大小等會有所差異）：

```
I/flutter ( 6559): WidgetsFlutterBinding - CHECKED MODE
I/flutter ( 6559): RenderObjectToWidgetAdapter<RenderBox>([GlobalObjectKey RenderView(497039273)]; renderObject: RenderView)
I/flutter ( 6559): └MaterialApp(state: _MaterialAppState(1009803148))
I/flutter ( 6559):  └ScrollConfiguration()
I/flutter ( 6559):   └AnimatedTheme(duration: 200ms; state: _AnimatedThemeState(543295893; ticker inactive; ThemeDataTween(ThemeData(Brightness.light Color(0xff2196f3) etc...) → null)))
I/flutter ( 6559):    └Theme(ThemeData(Brightness.light Color(0xff2196f3) etc...))
I/flutter ( 6559):     └WidgetsApp([GlobalObjectKey _MaterialAppState(1009803148)]; state: _WidgetsAppState(552902158))
I/flutter ( 6559):      └CheckedModeBanner()
I/flutter ( 6559):       └Banner()
I/flutter ( 6559):        └CustomPaint(renderObject: RenderCustomPaint)
I/flutter ( 6559):         └DefaultTextStyle(inherit: true; color: Color(0xd0ff0000); family: "monospace"; size: 48.0; weight: 900; decoration: double Color(0xffffff00) TextDecoration.underline)
I/flutter ( 6559):          └MediaQuery(MediaQueryData(size: Size(411.4, 683.4), devicePixelRatio: 2.625, textScaleFactor: 1.0, padding: EdgeInsets(0.0, 24.0, 0.0, 0.0)))
I/flutter ( 6559):           └LocaleQuery(null)
I/flutter ( 6559):            └Title(color: Color(0xff2196f3))
I/flutter ( 6559):             └Navigator([GlobalObjectKey<NavigatorState> _WidgetsAppState(552902158)]; state: NavigatorState(240327618; tracking 1 ticker))
I/flutter ( 6559):              └Listener(listeners: down, up, cancel; behavior: defer-to-child; renderObject: RenderPointerListener)
I/flutter ( 6559):               └AbsorbPointer(renderObject: RenderAbsorbPointer)
I/flutter ( 6559):                └Focus([GlobalKey 489139594]; state: _FocusState(739584448))
I/flutter ( 6559):                 └Semantics(container: true; renderObject: RenderSemanticsAnnotations)
I/flutter ( 6559):                  └_FocusScope(this scope has focus; focused subscope: [GlobalObjectKey MaterialPageRoute<void>(875520219)])
I/flutter ( 6559):                   └Overlay([GlobalKey 199833992]; state: OverlayState(619367313; entries: [OverlayEntry@248818791(opaque: false; maintainState: false), OverlayEntry@837336156(opaque: false; maintainState: true)]))
I/flutter ( 6559):                    └_Theatre(renderObject: _RenderTheatre)
I/flutter ( 6559):                     └Stack(renderObject: RenderStack)
I/flutter ( 6559):                      ├_OverlayEntry([GlobalKey 612888877]; state: _OverlayEntryState(739137453))
I/flutter ( 6559):                      │└IgnorePointer(ignoring: false; renderObject: RenderIgnorePointer)
I/flutter ( 6559):                      │ └ModalBarrier()
I/flutter ( 6559):                      │  └Semantics(container: true; renderObject: RenderSemanticsAnnotations)
I/flutter ( 6559):                      │   └GestureDetector()
I/flutter ( 6559):                      │    └RawGestureDetector(state: RawGestureDetectorState(39068508; gestures: tap; behavior: opaque))
I/flutter ( 6559):                      │     └_GestureSemantics(renderObject: RenderSemanticsGestureHandler)
I/flutter ( 6559):                      │      └Listener(listeners: down; behavior: opaque; renderObject: RenderPointerListener)
I/flutter ( 6559):                      │       └ConstrainedBox(BoxConstraints(biggest); renderObject: RenderConstrainedBox)
I/flutter ( 6559):                      └_OverlayEntry([GlobalKey 727622716]; state: _OverlayEntryState(279971240))
I/flutter ( 6559):                       └_ModalScope([GlobalKey 816151164]; state: _ModalScopeState(875510645))
I/flutter ( 6559):                        └Focus([GlobalObjectKey MaterialPageRoute<void>(875520219)]; state: _FocusState(331487674))
I/flutter ( 6559):                         └Semantics(container: true; renderObject: RenderSemanticsAnnotations)
I/flutter ( 6559):                          └_FocusScope(this scope has focus)
I/flutter ( 6559):                           └Offstage(offstage: false; renderObject: RenderOffstage)
I/flutter ( 6559):                            └IgnorePointer(ignoring: false; renderObject: RenderIgnorePointer)
I/flutter ( 6559):                             └_MountainViewPageTransition(animation: AnimationController(⏭ 1.000; paused; for MaterialPageRoute<void>(/))➩ProxyAnimation➩Cubic(0.40, 0.00, 0.20, 1.00)➩Tween<Offset>(Offset(0.0, 1.0) → Offset(0.0, 0.0))➩Offset(0.0, 0.0); state: _AnimatedState(552160732))
I/flutter ( 6559):                              └SlideTransition(animation: AnimationController(⏭ 1.000; paused; for MaterialPageRoute<void>(/))➩ProxyAnimation➩Cubic(0.40, 0.00, 0.20, 1.00)➩Tween<Offset>(Offset(0.0, 1.0) → Offset(0.0, 0.0))➩Offset(0.0, 0.0); state: _AnimatedState(714726495))
I/flutter ( 6559):                               └FractionalTranslation(renderObject: RenderFractionalTranslation)
I/flutter ( 6559):                                └RepaintBoundary(renderObject: RenderRepaintBoundary)
I/flutter ( 6559):                                 └PageStorage([GlobalKey 619728754])
I/flutter ( 6559):                                  └_ModalScopeStatus(active)
I/flutter ( 6559):                                   └AppHome()
I/flutter ( 6559):                                    └Material(MaterialType.canvas; elevation: 0; state: _MaterialState(780114997))
I/flutter ( 6559):                                     └AnimatedContainer(duration: 200ms; has background; state: _AnimatedContainerState(616063822; ticker inactive; has background))
I/flutter ( 6559):                                      └Container(bg: BoxDecoration())
I/flutter ( 6559):                                       └DecoratedBox(renderObject: RenderDecoratedBox)
I/flutter ( 6559):                                        └Container(bg: BoxDecoration(backgroundColor: Color(0xfffafafa)))
I/flutter ( 6559):                                         └DecoratedBox(renderObject: RenderDecoratedBox)
I/flutter ( 6559):                                          └NotificationListener<LayoutChangedNotification>()
I/flutter ( 6559):                                           └_InkFeature([GlobalKey ink renderer]; renderObject: _RenderInkFeatures)
I/flutter ( 6559):                                            └AnimatedDefaultTextStyle(duration: 200ms; inherit: false; color: Color(0xdd000000); family: "Roboto"; size: 14.0; weight: 400; baseline: alphabetic; state: _AnimatedDefaultTextStyleState(427742350; ticker inactive))
I/flutter ( 6559):                                             └DefaultTextStyle(inherit: false; color: Color(0xdd000000); family: "Roboto"; size: 14.0; weight: 400; baseline: alphabetic)
I/flutter ( 6559):                                              └Center(alignment: Alignment.center; renderObject: RenderPositionedBox)
I/flutter ( 6559):                                               └TextButton()
I/flutter ( 6559):                                                └MaterialButton(state: _MaterialButtonState(398724090))
I/flutter ( 6559):                                                 └ConstrainedBox(BoxConstraints(88.0<=w<=Infinity, h=36.0); renderObject: RenderConstrainedBox relayoutBoundary=up1)
I/flutter ( 6559):                                                  └AnimatedDefaultTextStyle(duration: 200ms; inherit: false; color: Color(0xdd000000); family: "Roboto"; size: 14.0; weight: 500; baseline: alphabetic; state: _AnimatedDefaultTextStyleState(315134664; ticker inactive))
I/flutter ( 6559):                                                   └DefaultTextStyle(inherit: false; color: Color(0xdd000000); family: "Roboto"; size: 14.0; weight: 500; baseline: alphabetic)
I/flutter ( 6559):                                                    └IconTheme(color: Color(0xdd000000))
I/flutter ( 6559):                                                     └InkWell(state: _InkResponseState<InkResponse>(369160267))
I/flutter ( 6559):                                                      └GestureDetector()
I/flutter ( 6559):                                                       └RawGestureDetector(state: RawGestureDetectorState(175370983; gestures: tap; behavior: opaque))
I/flutter ( 6559):                                                        └_GestureSemantics(renderObject: RenderSemanticsGestureHandler relayoutBoundary=up2)
I/flutter ( 6559):                                                         └Listener(listeners: down; behavior: opaque; renderObject: RenderPointerListener relayoutBoundary=up3)
I/flutter ( 6559):                                                          └Container(padding: EdgeInsets(16.0, 0.0, 16.0, 0.0))
I/flutter ( 6559):                                                           └Padding(renderObject: RenderPadding relayoutBoundary=up4)
I/flutter ( 6559):                                                            └Center(alignment: Alignment.center; widthFactor: 1.0; renderObject: RenderPositionedBox relayoutBoundary=up5)
I/flutter ( 6559):                                                             └Text("Dump App")
I/flutter ( 6559):                                                              └RichText(renderObject: RenderParagraph relayoutBoundary=up6)
```

This is the "flattened" tree, showing all the widgets projected
through their various build functions. (This is the tree you obtain if
you call `toStringDeep()` on the root of the widget tree.)
You'll see a lot of widgets in there that don't appear in your
application's source, because they are inserted by the framework's
widgets' build functions. For example,
[`InkFeature`][] is an implementation detail of the [`Material`][] widget.

這是一個「被拉平的樹」，透過它們的各種 build 函式，顯示出所有 widget 資訊。
（如果您呼叫根 widget 的 `toStringDeep()` 方法，就會得到這棵樹。）
您會看到很多 widget ，雖然它們沒出現在應用的原始碼中，但卻出現在這顆樹中，
因為它們是由框架中 widget 的 build 函式插入的。
比如，[`Material`][] widget 的實現細節中就包括了 [`InkFeature`][]。

Since the `debugDumpApp()` call is invoked when the button changes
from being pressed to being released, it coincides with the
[`TextButton`][] object calling [`setState()`][]
and thus marking itself dirty. That is why, when you look at the dump you
should see that specific object marked as "dirty". You can also see what
gesture listeners have been registered; in this case, a single
GestureDetector is listed, and it is listening only to a "tap" gesture
("tap" is the output of a `TapGestureDetector`'s `toStringShort`
function).

當按鈕被點選響應時，`debugDumpApp()` 方法被呼叫，
由於該方法與 [`TextButton`][] 物件呼叫 [`setState()`][] 相一致，
因此 TextButton 對應的元素會被標記為 dirty。
這就是為什麼在檢視轉儲資訊時，您會看到被標記為「dirty」的特定物件。
您也可以看到已經被註冊的手勢監聽器；在這個案例中，列出了一個 GestureDetector，
它只監聽「tap」手勢（這裡「tap」是 `TapGestureDetector` 的 `toStringShort` 函式輸出的）。

If you write your own widgets, you can add information by overriding
[`debugFillProperties()`][widget-fill]. Add [DiagnosticsProperty][]
objects to the method's argument, and call the superclass method.
This function is what the `toString` method uses to fill in the
widget's description.

對於您自訂的 widget，可以透過重寫 [`debugFillProperties()`][widget-fill] 方法新增資訊。
為方法中的引數新增 [DiagnosticsProperty][] 物件，並呼叫父類方法。
該方法在 widget 呼叫 `toString` 方法時會被填充到其描述資訊中。

### Render tree

### Render 樹

If you are trying to debug a layout issue, then the Widgets layer's
tree might be insufficiently detailed. In that case, you can dump the
rendering tree by calling [`debugDumpRenderTree()`][].
As with `debugDumpApp()`, you can call this more or less any time
except during a layout or paint phase. As a general rule,
calling it from a [frame callback][]
or an event handler is the best solution.

如果您試圖除錯一個佈局問題，那麼 Widget 層的樹可能不夠詳細。
在這種情況下，您可以透過呼叫 [`debugDumpRenderTree()`][] 轉儲 Render 樹資訊。
和 `debugDumpApp()` 一樣，除了在佈局或繪製階段，可以在任何時候呼叫它。
一般來說，最好在 [frame callback][] 或事件處理中呼叫它。

To call `debugDumpRenderTree()`, you need to add `import
'package:flutter/rendering.dart';` to your source file.

想要呼叫 `debugDumpRenderTree()` 方法，您需要在原始碼檔案中新增 
`import 'package:flutter/rendering.dart';`。

The output for the previous tiny example would look something like
the following:

前面的小案例輸出結構如下所示：

```
I/flutter ( 6559): RenderView
I/flutter ( 6559):  │ debug mode enabled - android
I/flutter ( 6559):  │ window size: Size(1080.0, 1794.0) (in physical pixels)
I/flutter ( 6559):  │ device pixel ratio: 2.625 (physical pixels per logical pixel)
I/flutter ( 6559):  │ configuration: Size(411.4, 683.4) at 2.625x (in logical pixels)
I/flutter ( 6559):  │
I/flutter ( 6559):  └─child: RenderCustomPaint
I/flutter ( 6559):    │ creator: CustomPaint ← Banner ← CheckedModeBanner ←
I/flutter ( 6559):    │   WidgetsApp-[GlobalObjectKey _MaterialAppState(1009803148)] ←
I/flutter ( 6559):    │   Theme ← AnimatedTheme ← ScrollConfiguration ← MaterialApp ←
I/flutter ( 6559):    │   [root]
I/flutter ( 6559):    │ parentData: <none>
I/flutter ( 6559):    │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):    │ size: Size(411.4, 683.4)
I/flutter ( 6559):    │
I/flutter ( 6559):    └─child: RenderPointerListener
I/flutter ( 6559):      │ creator: Listener ← Navigator-[GlobalObjectKey<NavigatorState>
I/flutter ( 6559):      │   _WidgetsAppState(552902158)] ← Title ← LocaleQuery ← MediaQuery
I/flutter ( 6559):      │   ← DefaultTextStyle ← CustomPaint ← Banner ← CheckedModeBanner ←
I/flutter ( 6559):      │   WidgetsApp-[GlobalObjectKey _MaterialAppState(1009803148)] ←
I/flutter ( 6559):      │   Theme ← AnimatedTheme ← ⋯
I/flutter ( 6559):      │ parentData: <none>
I/flutter ( 6559):      │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):      │ size: Size(411.4, 683.4)
I/flutter ( 6559):      │ behavior: defer-to-child
I/flutter ( 6559):      │ listeners: down, up, cancel
I/flutter ( 6559):      │
I/flutter ( 6559):      └─child: RenderAbsorbPointer
I/flutter ( 6559):        │ creator: AbsorbPointer ← Listener ←
I/flutter ( 6559):        │   Navigator-[GlobalObjectKey<NavigatorState>
I/flutter ( 6559):        │   _WidgetsAppState(552902158)] ← Title ← LocaleQuery ← MediaQuery
I/flutter ( 6559):        │   ← DefaultTextStyle ← CustomPaint ← Banner ← CheckedModeBanner ←
I/flutter ( 6559):        │   WidgetsApp-[GlobalObjectKey _MaterialAppState(1009803148)] ←
I/flutter ( 6559):        │   Theme ← ⋯
I/flutter ( 6559):        │ parentData: <none>
I/flutter ( 6559):        │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):        │ size: Size(411.4, 683.4)
I/flutter ( 6559):        │ absorbing: false
I/flutter ( 6559):        │
I/flutter ( 6559):        └─child: RenderSemanticsAnnotations
I/flutter ( 6559):          │ creator: Semantics ← Focus-[GlobalKey 489139594] ← AbsorbPointer
I/flutter ( 6559):          │   ← Listener ← Navigator-[GlobalObjectKey<NavigatorState>
I/flutter ( 6559):          │   _WidgetsAppState(552902158)] ← Title ← LocaleQuery ← MediaQuery
I/flutter ( 6559):          │   ← DefaultTextStyle ← CustomPaint ← Banner ← CheckedModeBanner ←
I/flutter ( 6559):          │   ⋯
I/flutter ( 6559):          │ parentData: <none>
I/flutter ( 6559):          │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):          │ size: Size(411.4, 683.4)
I/flutter ( 6559):          │
I/flutter ( 6559):          └─child: _RenderTheatre
I/flutter ( 6559):            │ creator: _Theatre ← Overlay-[GlobalKey 199833992] ← _FocusScope ←
I/flutter ( 6559):            │   Semantics ← Focus-[GlobalKey 489139594] ← AbsorbPointer ←
I/flutter ( 6559):            │   Listener ← Navigator-[GlobalObjectKey<NavigatorState>
I/flutter ( 6559):            │   _WidgetsAppState(552902158)] ← Title ← LocaleQuery ← MediaQuery
I/flutter ( 6559):            │   ← DefaultTextStyle ← ⋯
I/flutter ( 6559):            │ parentData: <none>
I/flutter ( 6559):            │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            │ size: Size(411.4, 683.4)
I/flutter ( 6559):            │
I/flutter ( 6559):            ├─onstage: RenderStack
I/flutter ( 6559):            ╎ │ creator: Stack ← _Theatre ← Overlay-[GlobalKey 199833992] ←
I/flutter ( 6559):            ╎ │   _FocusScope ← Semantics ← Focus-[GlobalKey 489139594] ←
I/flutter ( 6559):            ╎ │   AbsorbPointer ← Listener ←
I/flutter ( 6559):            ╎ │   Navigator-[GlobalObjectKey<NavigatorState>
I/flutter ( 6559):            ╎ │   _WidgetsAppState(552902158)] ← Title ← LocaleQuery ← MediaQuery
I/flutter ( 6559):            ╎ │   ← ⋯
I/flutter ( 6559):            ╎ │ parentData: not positioned; offset=Offset(0.0, 0.0)
I/flutter ( 6559):            ╎ │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎ │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎ │
I/flutter ( 6559):            ╎ ├─child 1: RenderIgnorePointer
I/flutter ( 6559):            ╎ │ │ creator: IgnorePointer ← _OverlayEntry-[GlobalKey 612888877] ←
I/flutter ( 6559):            ╎ │ │   Stack ← _Theatre ← Overlay-[GlobalKey 199833992] ← _FocusScope
I/flutter ( 6559):            ╎ │ │   ← Semantics ← Focus-[GlobalKey 489139594] ← AbsorbPointer ←
I/flutter ( 6559):            ╎ │ │   Listener ← Navigator-[GlobalObjectKey<NavigatorState>
I/flutter ( 6559):            ╎ │ │   _WidgetsAppState(552902158)] ← Title ← ⋯
I/flutter ( 6559):            ╎ │ │ parentData: not positioned; offset=Offset(0.0, 0.0)
I/flutter ( 6559):            ╎ │ │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎ │ │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎ │ │ ignoring: false
I/flutter ( 6559):            ╎ │ │ ignoringSemantics: implicitly false
I/flutter ( 6559):            ╎ │ │
I/flutter ( 6559):            ╎ │ └─child: RenderSemanticsAnnotations
I/flutter ( 6559):            ╎ │   │ creator: Semantics ← ModalBarrier ← IgnorePointer ←
I/flutter ( 6559):            ╎ │   │   _OverlayEntry-[GlobalKey 612888877] ← Stack ← _Theatre ←
I/flutter ( 6559):            ╎ │   │   Overlay-[GlobalKey 199833992] ← _FocusScope ← Semantics ←
I/flutter ( 6559):            ╎ │   │   Focus-[GlobalKey 489139594] ← AbsorbPointer ← Listener ← ⋯
I/flutter ( 6559):            ╎ │   │ parentData: <none>
I/flutter ( 6559):            ╎ │   │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎ │   │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎ │   │
I/flutter ( 6559):            ╎ │   └─child: RenderSemanticsGestureHandler
I/flutter ( 6559):            ╎ │     │ creator: _GestureSemantics ← RawGestureDetector ← GestureDetector
I/flutter ( 6559):            ╎ │     │   ← Semantics ← ModalBarrier ← IgnorePointer ←
I/flutter ( 6559):            ╎ │     │   _OverlayEntry-[GlobalKey 612888877] ← Stack ← _Theatre ←
I/flutter ( 6559):            ╎ │     │   Overlay-[GlobalKey 199833992] ← _FocusScope ← Semantics ← ⋯
I/flutter ( 6559):            ╎ │     │ parentData: <none>
I/flutter ( 6559):            ╎ │     │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎ │     │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎ │     │
I/flutter ( 6559):            ╎ │     └─child: RenderPointerListener
I/flutter ( 6559):            ╎ │       │ creator: Listener ← _GestureSemantics ← RawGestureDetector ←
I/flutter ( 6559):            ╎ │       │   GestureDetector ← Semantics ← ModalBarrier ← IgnorePointer ←
I/flutter ( 6559):            ╎ │       │   _OverlayEntry-[GlobalKey 612888877] ← Stack ← _Theatre ←
I/flutter ( 6559):            ╎ │       │   Overlay-[GlobalKey 199833992] ← _FocusScope ← ⋯
I/flutter ( 6559):            ╎ │       │ parentData: <none>
I/flutter ( 6559):            ╎ │       │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎ │       │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎ │       │ behavior: opaque
I/flutter ( 6559):            ╎ │       │ listeners: down
I/flutter ( 6559):            ╎ │       │
I/flutter ( 6559):            ╎ │       └─child: RenderConstrainedBox
I/flutter ( 6559):            ╎ │           creator: ConstrainedBox ← Listener ← _GestureSemantics ←
I/flutter ( 6559):            ╎ │             RawGestureDetector ← GestureDetector ← Semantics ← ModalBarrier
I/flutter ( 6559):            ╎ │             ← IgnorePointer ← _OverlayEntry-[GlobalKey 612888877] ← Stack ←
I/flutter ( 6559):            ╎ │             _Theatre ← Overlay-[GlobalKey 199833992] ← ⋯
I/flutter ( 6559):            ╎ │           parentData: <none>
I/flutter ( 6559):            ╎ │           constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎ │           size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎ │           additionalConstraints: BoxConstraints(biggest)
I/flutter ( 6559):            ╎ │
I/flutter ( 6559):            ╎ └─child 2: RenderSemanticsAnnotations
I/flutter ( 6559):            ╎   │ creator: Semantics ← Focus-[GlobalObjectKey
I/flutter ( 6559):            ╎   │   MaterialPageRoute<void>(875520219)] ← _ModalScope-[GlobalKey
I/flutter ( 6559):            ╎   │   816151164] ← _OverlayEntry-[GlobalKey 727622716] ← Stack ←
I/flutter ( 6559):            ╎   │   _Theatre ← Overlay-[GlobalKey 199833992] ← _FocusScope ←
I/flutter ( 6559):            ╎   │   Semantics ← Focus-[GlobalKey 489139594] ← AbsorbPointer ←
I/flutter ( 6559):            ╎   │   Listener ← ⋯
I/flutter ( 6559):            ╎   │ parentData: not positioned; offset=Offset(0.0, 0.0)
I/flutter ( 6559):            ╎   │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎   │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎   │
I/flutter ( 6559):            ╎   └─child: RenderOffstage
I/flutter ( 6559):            ╎     │ creator: Offstage ← _FocusScope ← Semantics ←
I/flutter ( 6559):            ╎     │   Focus-[GlobalObjectKey MaterialPageRoute<void>(875520219)] ←
I/flutter ( 6559):            ╎     │   _ModalScope-[GlobalKey 816151164] ← _OverlayEntry-[GlobalKey
I/flutter ( 6559):            ╎     │   727622716] ← Stack ← _Theatre ← Overlay-[GlobalKey 199833992] ←
I/flutter ( 6559):            ╎     │   _FocusScope ← Semantics ← Focus-[GlobalKey 489139594] ← ⋯
I/flutter ( 6559):            ╎     │ parentData: <none>
I/flutter ( 6559):            ╎     │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎     │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎     │ offstage: false
I/flutter ( 6559):            ╎     │
I/flutter ( 6559):            ╎     └─child: RenderIgnorePointer
I/flutter ( 6559):            ╎       │ creator: IgnorePointer ← Offstage ← _FocusScope ← Semantics ←
I/flutter ( 6559):            ╎       │   Focus-[GlobalObjectKey MaterialPageRoute<void>(875520219)] ←
I/flutter ( 6559):            ╎       │   _ModalScope-[GlobalKey 816151164] ← _OverlayEntry-[GlobalKey
I/flutter ( 6559):            ╎       │   727622716] ← Stack ← _Theatre ← Overlay-[GlobalKey 199833992] ←
I/flutter ( 6559):            ╎       │   _FocusScope ← Semantics ← ⋯
I/flutter ( 6559):            ╎       │ parentData: <none>
I/flutter ( 6559):            ╎       │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎       │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎       │ ignoring: false
I/flutter ( 6559):            ╎       │ ignoringSemantics: implicitly false
I/flutter ( 6559):            ╎       │
I/flutter ( 6559):            ╎       └─child: RenderFractionalTranslation
I/flutter ( 6559):            ╎         │ creator: FractionalTranslation ← SlideTransition ←
I/flutter ( 6559):            ╎         │   _MountainViewPageTransition ← IgnorePointer ← Offstage ←
I/flutter ( 6559):            ╎         │   _FocusScope ← Semantics ← Focus-[GlobalObjectKey
I/flutter ( 6559):            ╎         │   MaterialPageRoute<void>(875520219)] ← _ModalScope-[GlobalKey
I/flutter ( 6559):            ╎         │   816151164] ← _OverlayEntry-[GlobalKey 727622716] ← Stack ←
I/flutter ( 6559):            ╎         │   _Theatre ← ⋯
I/flutter ( 6559):            ╎         │ parentData: <none>
I/flutter ( 6559):            ╎         │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎         │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎         │ translation: Offset(0.0, 0.0)
I/flutter ( 6559):            ╎         │ transformHitTests: true
I/flutter ( 6559):            ╎         │
I/flutter ( 6559):            ╎         └─child: RenderRepaintBoundary
I/flutter ( 6559):            ╎           │ creator: RepaintBoundary ← FractionalTranslation ←
I/flutter ( 6559):            ╎           │   SlideTransition ← _MountainViewPageTransition ← IgnorePointer ←
I/flutter ( 6559):            ╎           │   Offstage ← _FocusScope ← Semantics ← Focus-[GlobalObjectKey
I/flutter ( 6559):            ╎           │   MaterialPageRoute<void>(875520219)] ← _ModalScope-[GlobalKey
I/flutter ( 6559):            ╎           │   816151164] ← _OverlayEntry-[GlobalKey 727622716] ← Stack ← ⋯
I/flutter ( 6559):            ╎           │ parentData: <none>
I/flutter ( 6559):            ╎           │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎           │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎           │ metrics: 83.3% useful (1 bad vs 5 good)
I/flutter ( 6559):            ╎           │ diagnosis: this is a useful repaint boundary and should be kept
I/flutter ( 6559):            ╎           │
I/flutter ( 6559):            ╎           └─child: RenderDecoratedBox
I/flutter ( 6559):            ╎             │ creator: DecoratedBox ← Container ← AnimatedContainer ← Material
I/flutter ( 6559):            ╎             │   ← AppHome ← _ModalScopeStatus ← PageStorage-[GlobalKey
I/flutter ( 6559):            ╎             │   619728754] ← RepaintBoundary ← FractionalTranslation ←
I/flutter ( 6559):            ╎             │   SlideTransition ← _MountainViewPageTransition ← IgnorePointer ←
I/flutter ( 6559):            ╎             │   ⋯
I/flutter ( 6559):            ╎             │ parentData: <none>
I/flutter ( 6559):            ╎             │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎             │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎             │ decoration:
I/flutter ( 6559):            ╎             │   <no decorations specified>
I/flutter ( 6559):            ╎             │ configuration: ImageConfiguration(bundle:
I/flutter ( 6559):            ╎             │   PlatformAssetBundle@367106502(), devicePixelRatio: 2.625,
I/flutter ( 6559):            ╎             │   platform: android)
I/flutter ( 6559):            ╎             │
I/flutter ( 6559):            ╎             └─child: RenderDecoratedBox
I/flutter ( 6559):            ╎               │ creator: DecoratedBox ← Container ← DecoratedBox ← Container ←
I/flutter ( 6559):            ╎               │   AnimatedContainer ← Material ← AppHome ← _ModalScopeStatus ←
I/flutter ( 6559):            ╎               │   PageStorage-[GlobalKey 619728754] ← RepaintBoundary ←
I/flutter ( 6559):            ╎               │   FractionalTranslation ← SlideTransition ← ⋯
I/flutter ( 6559):            ╎               │ parentData: <none>
I/flutter ( 6559):            ╎               │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎               │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎               │ decoration:
I/flutter ( 6559):            ╎               │   backgroundColor: Color(0xfffafafa)
I/flutter ( 6559):            ╎               │ configuration: ImageConfiguration(bundle:
I/flutter ( 6559):            ╎               │   PlatformAssetBundle@367106502(), devicePixelRatio: 2.625,
I/flutter ( 6559):            ╎               │   platform: android)
I/flutter ( 6559):            ╎               │
I/flutter ( 6559):            ╎               └─child: _RenderInkFeatures
I/flutter ( 6559):            ╎                 │ creator: _InkFeature-[GlobalKey ink renderer] ←
I/flutter ( 6559):            ╎                 │   NotificationListener<LayoutChangedNotification> ← DecoratedBox
I/flutter ( 6559):            ╎                 │   ← Container ← DecoratedBox ← Container ← AnimatedContainer ←
I/flutter ( 6559):            ╎                 │   Material ← AppHome ← _ModalScopeStatus ← PageStorage-[GlobalKey
I/flutter ( 6559):            ╎                 │   619728754] ← RepaintBoundary ← ⋯
I/flutter ( 6559):            ╎                 │ parentData: <none>
I/flutter ( 6559):            ╎                 │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎                 │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎                 │
I/flutter ( 6559):            ╎                 └─child: RenderPositionedBox
I/flutter ( 6559):            ╎                   │ creator: Center ← DefaultTextStyle ← AnimatedDefaultTextStyle ←
I/flutter ( 6559):            ╎                   │   _InkFeature-[GlobalKey ink renderer] ←
I/flutter ( 6559):            ╎                   │   NotificationListener<LayoutChangedNotification> ← DecoratedBox
I/flutter ( 6559):            ╎                   │   ← Container ← DecoratedBox ← Container ← AnimatedContainer ←
I/flutter ( 6559):            ╎                   │   Material ← AppHome ← ⋯
I/flutter ( 6559):            ╎                   │ parentData: <none>
I/flutter ( 6559):            ╎                   │ constraints: BoxConstraints(w=411.4, h=683.4)
I/flutter ( 6559):            ╎                   │ size: Size(411.4, 683.4)
I/flutter ( 6559):            ╎                   │ alignment: Alignment.center
I/flutter ( 6559):            ╎                   │ widthFactor: expand
I/flutter ( 6559):            ╎                   │ heightFactor: expand
I/flutter ( 6559):            ╎                   │
I/flutter ( 6559):            ╎                   └─child: RenderConstrainedBox relayoutBoundary=up1
I/flutter ( 6559):            ╎                     │ creator: ConstrainedBox ← MaterialButton ← TextButton ← Center ←
I/flutter ( 6559):            ╎                     │   DefaultTextStyle ← AnimatedDefaultTextStyle ←
I/flutter ( 6559):            ╎                     │   _InkFeature-[GlobalKey ink renderer] ←
I/flutter ( 6559):            ╎                     │   NotificationListener<LayoutChangedNotification> ← DecoratedBox
I/flutter ( 6559):            ╎                     │   ← Container ← DecoratedBox ← Container ← ⋯
I/flutter ( 6559):            ╎                     │ parentData: offset=Offset(156.7, 323.7)
I/flutter ( 6559):            ╎                     │ constraints: BoxConstraints(0.0<=w<=411.4, 0.0<=h<=683.4)
I/flutter ( 6559):            ╎                     │ size: Size(98.0, 36.0)
I/flutter ( 6559):            ╎                     │ additionalConstraints: BoxConstraints(88.0<=w<=Infinity, h=36.0)
I/flutter ( 6559):            ╎                     │
I/flutter ( 6559):            ╎                     └─child: RenderSemanticsGestureHandler relayoutBoundary=up2
I/flutter ( 6559):            ╎                       │ creator: _GestureSemantics ← RawGestureDetector ← GestureDetector
I/flutter ( 6559):            ╎                       │   ← InkWell ← IconTheme ← DefaultTextStyle ←
I/flutter ( 6559):            ╎                       │   AnimatedDefaultTextStyle ← ConstrainedBox ← MaterialButton ←
I/flutter ( 6559):            ╎                       │   TextButton ← Center ← DefaultTextStyle ← ⋯
I/flutter ( 6559):            ╎                       │ parentData: <none>
I/flutter ( 6559):            ╎                       │ constraints: BoxConstraints(88.0<=w<=411.4, h=36.0)
I/flutter ( 6559):            ╎                       │ size: Size(98.0, 36.0)
I/flutter ( 6559):            ╎                       │
I/flutter ( 6559):            ╎                       └─child: RenderPointerListener relayoutBoundary=up3
I/flutter ( 6559):            ╎                         │ creator: Listener ← _GestureSemantics ← RawGestureDetector ←
I/flutter ( 6559):            ╎                         │   GestureDetector ← InkWell ← IconTheme ← DefaultTextStyle ←
I/flutter ( 6559):            ╎                         │   AnimatedDefaultTextStyle ← ConstrainedBox ← MaterialButton ←
I/flutter ( 6559):            ╎                         │   TextButton ← Center ← ⋯
I/flutter ( 6559):            ╎                         │ parentData: <none>
I/flutter ( 6559):            ╎                         │ constraints: BoxConstraints(88.0<=w<=411.4, h=36.0)
I/flutter ( 6559):            ╎                         │ size: Size(98.0, 36.0)
I/flutter ( 6559):            ╎                         │ behavior: opaque
I/flutter ( 6559):            ╎                         │ listeners: down
I/flutter ( 6559):            ╎                         │
I/flutter ( 6559):            ╎                         └─child: RenderPadding relayoutBoundary=up4
I/flutter ( 6559):            ╎                           │ creator: Padding ← Container ← Listener ← _GestureSemantics ←
I/flutter ( 6559):            ╎                           │   RawGestureDetector ← GestureDetector ← InkWell ← IconTheme ←
I/flutter ( 6559):            ╎                           │   DefaultTextStyle ← AnimatedDefaultTextStyle ← ConstrainedBox ←
I/flutter ( 6559):            ╎                           │   MaterialButton ← ⋯
I/flutter ( 6559):            ╎                           │ parentData: <none>
I/flutter ( 6559):            ╎                           │ constraints: BoxConstraints(88.0<=w<=411.4, h=36.0)
I/flutter ( 6559):            ╎                           │ size: Size(98.0, 36.0)
I/flutter ( 6559):            ╎                           │ padding: EdgeInsets(16.0, 0.0, 16.0, 0.0)
I/flutter ( 6559):            ╎                           │
I/flutter ( 6559):            ╎                           └─child: RenderPositionedBox relayoutBoundary=up5
I/flutter ( 6559):            ╎                             │ creator: Center ← Padding ← Container ← Listener ←
I/flutter ( 6559):            ╎                             │   _GestureSemantics ← RawGestureDetector ← GestureDetector ←
I/flutter ( 6559):            ╎                             │   InkWell ← IconTheme ← DefaultTextStyle ←
I/flutter ( 6559):            ╎                             │   AnimatedDefaultTextStyle ← ConstrainedBox ← ⋯
I/flutter ( 6559):            ╎                             │ parentData: offset=Offset(16.0, 0.0)
I/flutter ( 6559):            ╎                             │ constraints: BoxConstraints(56.0<=w<=379.4, h=36.0)
I/flutter ( 6559):            ╎                             │ size: Size(66.0, 36.0)
I/flutter ( 6559):            ╎                             │ alignment: Alignment.center
I/flutter ( 6559):            ╎                             │ widthFactor: 1.0
I/flutter ( 6559):            ╎                             │ heightFactor: expand
I/flutter ( 6559):            ╎                             │
I/flutter ( 6559):            ╎                             └─child: RenderParagraph relayoutBoundary=up6
I/flutter ( 6559):            ╎                               │ creator: RichText ← Text ← Center ← Padding ← Container ←
I/flutter ( 6559):            ╎                               │   Listener ← _GestureSemantics ← RawGestureDetector ←
I/flutter ( 6559):            ╎                               │   GestureDetector ← InkWell ← IconTheme ← DefaultTextStyle ← ⋯
I/flutter ( 6559):            ╎                               │ parentData: offset=Offset(0.0, 10.0)
I/flutter ( 6559):            ╎                               │ constraints: BoxConstraints(0.0<=w<=379.4, 0.0<=h<=36.0)
I/flutter ( 6559):            ╎                               │ size: Size(66.0, 16.0)
I/flutter ( 6559):            ╎                               ╘═╦══ text ═══
I/flutter ( 6559):            ╎                                 ║ TextSpan:
I/flutter ( 6559):            ╎                                 ║   inherit: false
I/flutter ( 6559):            ╎                                 ║   color: Color(0xdd000000)
I/flutter ( 6559):            ╎                                 ║   family: "Roboto"
I/flutter ( 6559):            ╎                                 ║   size: 14.0
I/flutter ( 6559):            ╎                                 ║   weight: 500
I/flutter ( 6559):            ╎                                 ║   baseline: alphabetic
I/flutter ( 6559):            ╎                                 ║   "Dump App"
I/flutter ( 6559):            ╎                                 ╚═══════════
I/flutter ( 6559):            ╎
I/flutter ( 6559):            └╌no offstage children

```

This is the output of the root `RenderObject` object's
`toStringDeep()` function.

這是根節點 `RenderObject` 物件的 `toStringDeep()` 方法的輸出結果。

When debugging layout issues, the key fields to look at are the
`size` and `constraints` fields. The constraints flow down the tree,
and the sizes flow back up.

在除錯佈局問題時，主要需要關注 `size` 和 `constraints` 兩個欄位。
constraint 沿樹向下傳遞，而 size 則向上追溯。

For example, in the previous dump you can see that the window size,
`Size(411.4, 683.4)`, is used to force all the boxes down to the
[`RenderPositionedBox`][] to be the size of the screen, with
constraints of `BoxConstraints(w=411.4, h=683.4)`.
The `RenderPositionedBox`, which the dump says was created by a
[`Center`][] widget (as described by the `creator` field),
sets its child's constraints to a loose version of this:
`BoxConstraints(0.0<=w<=411.4, 0.0<=h<=683.4)`. The child, a
[`RenderPadding`][], further inserts these constraints to ensure
there is room for the padding, and thus the [`RenderConstrainedBox`][]
has a loose constraint of `BoxConstraints(0.0<=w<=395.4,
0.0<=h<=667.4)`. This object, which the `creator` field tells us is
probably part of the [`TextButton`][]'s definition,
sets a minimum width of 88 pixels on its contents and a
specific height of 36.0. (This is the `TextButton` class implementing
the Material Design guidelines regarding button dimensions.)

比如，從上面轉儲資訊中可以看出視窗尺寸是 `Size(411.4, 683.4)`，
它用於強制 [`RenderPositionedBox`][] 之前的所有 box 為螢幕尺寸，
其約束為 `BoxConstraints(w=411.4, h=683.4)`。
從轉儲檔案可以看出 `RenderPositionedBox` 是由 [`Center`][] widget 建立的
（可以從 `creator` 欄位的描述看出來），並將其 child 的約束條件變得鬆散：
約束範圍是 `BoxConstraints(0.0<=w<=411.4, 0.0<=h<=683.4)`。
其後代的 [`RenderPadding`][] 進一步插入這些約束來確保留出空間作為內邊距，
因此 [`RenderConstrainedBox`][] 有一個寬鬆的約束，該約束為：
`BoxConstraints(0.0<=w<=395.4,0.0<=h<=667.4)`。
`creator` 欄位告訴我們，這個物件很可能是 [`TextButton`][] 定義的一部分，
它內容的最小寬度為 88 畫素，具體高度為 36.0。（ `TextButton` 是 Material Design 中按鈕尺寸標準的實現。）

The inner-most `RenderPositionedBox` loosens the constraints again,
this time to center the text within the button. The
[`RenderParagraph`][] picks its size based on its contents.
If you now follow the sizes back up the chain,
you'll see how the text's size is what influences the
width of all the boxes that form the button, as they all take their
child's dimensions to size themselves.

最內部的 `RenderPositionedBox` 再次放鬆了約束，這次是把文字放在了按鈕的中間。
[`RenderParagraph`][] 可以根據其內容確定自身大小。
如果您現在沿著這條鏈路往回追溯渲染物件的尺寸大小，
您就會看到在文字的大小是如何影響按鈕邊框大小的形成過程，
因為它們都會根據子元件的尺寸自行調整大小。

Another way to notice this is by looking at the "relayoutSubtreeRoot"
part of the descriptions of each box, which essentially tells you how
many ancestors depend on this element's size in some way.
Thus the `RenderParagraph` has a `relayoutSubtreeRoot=up8`,
meaning that when the `RenderParagraph` is dirtied,
eight ancestors also have to be dirtied because they might be
affected by the new dimensions.

注意到這點的另一種方式為：檢視每個 box 的「relayoutSubtreeRoot」部分，
它本質上在告訴您，在某種程度上有多少祖先在依賴於這個元素的尺寸。
因此，`RenderParagraph` 有 `relayoutSubtreeRoot=up8`，
這意味著當 `RenderParagraph` 被標為 dirty 時，8 個祖先也會被標為 dirty，
因為它們可能會受到新尺寸的影響。

If you write your own render objects, you can add information to the
dump by overriding [`debugFillProperties()`][render-fill].
Add [DiagnosticsProperty][]
objects to the method's argument, and call the superclass method.

對於您自己寫的 render 物件，可以透過重寫 [`debugFillProperties()`][render-fill] 
方法為轉儲資料新增資訊。在方法中的引數中新增 [DiagnosticsProperty][] 物件，並呼叫父類方法即可。

### Layer tree

### Layer 樹

If you are trying to debug a compositing issue, you can use
[`debugDumpLayerTree()`][].
For the previous example, it would output:

如果您在嘗試除錯一個合成問題，您可以使用 [`debugDumpLayerTree()`][]。
在前面案例中呼叫這個方法，會輸出如下結果：

```
I/flutter : TransformLayer
I/flutter :  │ creator: [root]
I/flutter :  │ offset: Offset(0.0, 0.0)
I/flutter :  │ transform:
I/flutter :  │   [0] 3.5,0.0,0.0,0.0
I/flutter :  │   [1] 0.0,3.5,0.0,0.0
I/flutter :  │   [2] 0.0,0.0,1.0,0.0
I/flutter :  │   [3] 0.0,0.0,0.0,1.0
I/flutter :  │
I/flutter :  ├─child 1: OffsetLayer
I/flutter :  │ │ creator: RepaintBoundary ← _FocusScope ← Semantics ← Focus-[GlobalObjectKey MaterialPageRoute(560156430)] ← _ModalScope-[GlobalKey 328026813] ← _OverlayEntry-[GlobalKey 388965355] ← Stack ← Overlay-[GlobalKey 625702218] ← Navigator-[GlobalObjectKey _MaterialAppState(859106034)] ← Title ← ⋯
I/flutter :  │ │ offset: Offset(0.0, 0.0)
I/flutter :  │ │
I/flutter :  │ └─child 1: PictureLayer
I/flutter :  │
I/flutter :  └─child 2: PictureLayer
```

This is the output of calling `toStringDeep` on the root `Layer` object.

這是根 `Layer` 物件呼叫 `toStringDeep` 方法時的輸出結果。

The transform at the root is the transform that applies the device
pixel ratio; in this case, a ratio of 3.5 device pixels for every
logical pixel.

根結點的 transform 是裝置畫素比率的變換；在該範例中，每個邏輯畫素對應 3.5 個裝置畫素。

The `RepaintBoundary` widget, which creates a `RenderRepaintBoundary`
in the render tree, creates a new layer in the layer tree. This is
used to reduce how much needs to be repainted.

`RepaintBoundary` widget 在 render 樹中建立了一個 `RenderRepaintBoundary`，
並在 layer 樹中建立了一個新的層。這可以用來減少需要重繪的次數。

### Semantics tree

### Semantics 樹

You can also obtain a dump of the Semantics tree
(the tree presented to the system accessibility APIs) using
[`debugDumpSemanticsTree()`][].  To use this,
you have to have first enable accessibility, for example, by
enabling a system accessibility tool or the `SemanticsDebugger`.

您也可以使用 [`debugDumpSemanticsTree()`][] 獲得 Semantics 樹
（該樹提供了系統的 accessibility API）的轉儲資訊。想要使用它，
首先必須啟用 accessibility，例如，透過啟用系統 accessibility 工具或 `SemanticsDebugger`。

For the previous example, it would output the following:

在前面案例中呼叫這個方法，會輸出如下結果：

```
I/flutter : SemanticsNode(0; Rect.fromLTRB(0.0, 0.0, 411.4, 683.4))
I/flutter :  ├SemanticsNode(1; Rect.fromLTRB(0.0, 0.0, 411.4, 683.4))
I/flutter :  │ └SemanticsNode(2; Rect.fromLTRB(0.0, 0.0, 411.4, 683.4); canBeTapped)
I/flutter :  └SemanticsNode(3; Rect.fromLTRB(0.0, 0.0, 411.4, 683.4))
I/flutter :    └SemanticsNode(4; Rect.fromLTRB(0.0, 0.0, 82.0, 36.0); canBeTapped; "Dump App")
```

<!-- this tree is bad, see {{site.repo.flutter}}/issues/2476 -->

### Scheduling

To find out where your events happen relative to the frame's
begin/end, you can toggle the [`debugPrintBeginFrameBanner`][]
and the [`debugPrintEndFrameBanner`][] booleans to print the
beginning and end of the frames to the console.

如果您想要找到事件觸發對應的開始或結束幀，可以將 [`debugPrintBeginFrameBanner`][] 
和 [`debugPrintEndFrameBanner`][] 這兩個布林值切換為 true，在控制檯中列印開始和結束幀的資訊。

For example:

比如：

```
I/flutter : ▄▄▄▄▄▄▄▄ Frame 12         30s 437.086ms ▄▄▄▄▄▄▄▄
I/flutter : Debug print: Am I performing this work more than once per frame?
I/flutter : Debug print: Am I performing this work more than once per frame?
I/flutter : ▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀
```

The [`debugPrintScheduleFrameStacks`][] flag can also be used
to print the call stack causing the current frame to be scheduled.

當前幀被排程時，[`debugPrintScheduleFrameStacks`][] 標誌也可以用來列印呼叫堆疊資訊。


## Debug flags: layout

## 除錯標誌：佈局

You can also debug a layout problem visually, by setting
[`debugPaintSizeEnabled`][] to `true`.
This is a boolean from the `rendering` library. It can be
enabled at any time and affects all painting while it is true.
The easiest way to set it is at the top of your `void main()`
entry point. See an example in the following code:

透過將 [`debugPaintSizeEnabled`][] 設定為 true，您也可以可視地除錯佈局問題。
該布林值在 `rendering` 庫中，可以在任何時候被啟用，
並且當其為 true 時，會影響介面上所有的繪製。
最簡單的方式是在程式頂部入口 `void main()`中設定它，如下案例程式碼所示：

<?code-excerpt "lib/debug_flags.dart (debugPaintSizeEnabled)"?>
```dart
//add import to rendering library
import 'package:flutter/rendering.dart';

void main() {
  debugPaintSizeEnabled = true;
  runApp(const MyApp());
}
```

When it is enabled, all boxes get a bright teal border,
padding (from widgets like `Padding`) is shown in faded
blue with a darker blue box around the child, alignment
(from widgets like `Center` and `Align`) is shown with
yellow arrows, and spacers (from widgets like
`Container` when they have no child) are shown in gray.

當它被啟用時，所有的 box 都會有明亮的藍綠色邊框，內邊距（來自於 widgets，比如 `Padding`）
顯示為淡藍色，並在 child 周圍有一個深藍色的 box，對齊方式（來自於 widgets，比如 `Center` 和 `Align`）
顯示為黃色箭頭，還有間隔（來自於 widgets，比如當 `Container` 沒有 child 時）顯示灰色。

The [`debugPaintBaselinesEnabled`][] flag
does something similar but for objects with baselines.
The alphabetic baseline is shown in bright green and the
ideographic baseline in orange.

`debugPaintBaselinesEnabled`][] 標誌和它類似，但只針對於帶有基線的物件。
alphabetic 基線用亮綠色顯示，ideographic 基線用橙色顯示。

The [`debugPaintPointersEnabled`][] flag turns on a
special mode whereby any objects that are being tapped
get highlighted in teal. This can help you determine
whether an object is somehow failing to correctly hit
test (which might happen if, for instance, it is actually
outside the bounds of its parent and thus not
being considered for hit testing in the first place).

[`debugPaintPointersEnabled`][] 標誌會開啟一個特殊模式，任何被選中的物件都會以藍綠色高亮顯示。
這可以幫助您確定物件是否會以某種方式未能正確命中測試（這是可能會發生的，例如，
實際上它在父節點的邊界之外，因此一開始就不用考慮進行命中測試）。

If you're trying to debug compositor layers, for example
to determine whether and where to add `RepaintBoundary`
widgets, you can use the [`debugPaintLayerBordersEnabled`][]
flag, which outlines each layer's bounds in orange,
or the [`debugRepaintRainbowEnabled`][] flag,
which causes layers to be overlayed with a rotating set of
colors whenever they are repainted.

如果您試圖除錯合成層，比如要確定是否應該在某處新增 `RepaintBoundary` widget，
您可以使用 [`debugPaintLayerBordersEnabled`][] 標誌，來用為每個 layer 的邊界顯示橙色邊框，
或使用 [`debugRepaintRainbowEnabled`][] 標誌，這會使得每當重新繪製圖層時，
邊框的顏色就會被一組輪轉的顏色覆蓋。

All of these flags only work in [debug mode][].
In general, anything in the Flutter framework that starts with
"`debug...`" only works in debug mode.

上面所有的標誌都只在 [除錯模式][debug mode] 下生效。一般來說，Flutter 
框架中以「`debug...`」開頭的都只能在除錯模式下工作。

## Debugging animations

## 除錯動畫

{{site.alert.note}}

  The easiest way to debug animations is to slow them down.
  You can do this using the **Slow Animations** button in
  DevTools' [Inspector view][], which slows down the
  animation by 5x. If you want more control over the
  amount of slowness, use the following instructions.

  除錯動畫最簡單的方法是放慢它們的速度。您可以在 DevTools 的 [Inspector view][] 
  透過 **Slow Animations** 按鈕來實現動畫慢放，這會使動畫速度降低 5 倍。
  如果您希望控制更多的慢速程度，請參考下面的說明。

{{site.alert.end}}

Set the [`timeDilation`][] variable (from the `scheduler`
library) to a number greater than 1.0, for instance, 50.0.
It's best to only set this once on app startup. If you
change it on the fly, especially if you reduce it while
animations are running, it's possible that the framework
will observe time going backwards, which will probably
result in asserts and generally interfere with your efforts.

將 [`timeDilation`][] 變數（來自 `scheduler` 庫）設定為大於 1.0 的數字，例如，50.0。
該操作最好在應用啟動時只執行一次。如果您動態地改變，尤其是在動畫執行時減少它時，
框架可能會觀察到時間倒退，這可能會導致斷言失敗，通常這會讓您徒勞無功。

## Debug flags: performance

## 除錯標誌：效能

{{site.alert.note}}

  You can achieve similar results to some of these debug
  flags using [DevTools][]. Some of the debug flags aren't
  particularly useful. If you find a flag that has
  functionality you would like to see added to [DevTools][],
  please [file an issue][].

  您可以使用 [DevTools][] 實現和這些除錯標誌類似的結果。有些除錯標記不是特別有用。
  如果您發現一個標誌，並想把該功能新增到 [DevTools][]，請 [提出一個 issue][file an issue]。

{{site.alert.end}}

Flutter provides a wide variety of debug flags and functions
to help you debug your app at various points along the
development cycle. To use these features, you must compile
in debug mode.  The following list, while not complete,
highlights some of flags (and one function) from the
[rendering library][] for debugging performance issues.

Flutter 提供了各種各樣的除錯標誌和功能，來幫助您在開發週期的不同階段除錯應用。
想要使用這些特性，必須在除錯模式下編譯。下面的列表雖然不完整，
但是突出顯示了 [rendering library][] 中用於除錯效能問題的一些標誌（以及一個函式）。

You can set these flags either by editing the framework code,
or by importing the module and setting the value in your
`main()` method, following by a hot restart.

您可以透過修改框架的程式碼來設定這些標誌，或者將模組匯入，
並在 `main()` 方法中設定標誌值，然後熱重啟。

<dl markdown="1">
<dt markdown="1">[`debugDumpRenderTree()`][]</dt>
<dd markdown="1">
<p markdown="1">Call this function when not in a layout or repaint
   phase to dump the rendering tree to the console.
   (Pressing **t** from `flutter run` calls this command.)
   Search for "RepaintBoundary" to see diagnostics
   on how useful a boundary is.</p>
<p markdown="1">當不在佈局或重新繪製階段時，呼叫此函式將 render 樹轉儲到控制檯。
  （可以從 `flutter run`  按下 **t** 呼叫此命令。）
  透過搜尋其中的「RepaintBoundary」可以檢視關於邊界的有用診斷資訊。</p>
<dt markdown="1">[`debugPaintLayerBordersEnabled`][]</dt>
<dd markdown="1">PENDING
<dt markdown="1">[`debugRepaintRainbowEnabled`][]</dt>
<dd markdown="1">
<p markdown="1">You can enable this flag in the Flutter
    inspector by selecting the **Highlight Repaints** button.
    If any static widgets are rotating through the colors of the rainbow
    (for example, a static header), those areas are candidates for adding
    repaint boundaries.</p>
<p markdown="1">您可以透過點選 **Highlight Repaints** 按鈕，
    在 Flutter inspector 中啟用此標誌。
    如果任何靜態 widget 在彩虹七顏色之間輪轉（比如一個靜態標題），
    那麼這些區域就可能需要新增重新繪製邊界進行最佳化。</p>
<dt markdown="1">[`debugPrintMarkNeedsLayoutStacks`][]</dt>
<dd markdown="1">
<p markdown="1">Enable this flag if you're seeing more layouts
    than you expect (for example, on the timeline, on a profile,
    or from a `print` statement inside a layout method).
    Once enabled, the console is flooded with stack traces
    showing why each render object is being marked dirty for
    layout. You can use the `debugPrintStack()` method from the
    `services` library to print your own stack traces on demand,
    if this kind of approach is useful to you.</p>
<p markdown="1">如果您看到的佈局比預期的要多
    （比如，在 timeline 、profile 或者一個佈局方法中的 `print` 陳述式中)，
    可以啟用這個標誌。一旦啟用，控制檯將會充滿堆疊追蹤，
    來顯示在佈局時每個渲染物件被標記為 dirty 的原因。
    如果有需要的話，您可以使用 `services` 庫中的 `debugPrintStack()` 方法
    按需打印出堆疊的追蹤資訊。</p>
<dt markdown="1">[`debugPrintMarkNeedsPaintStacks`][]</dt>
<dd markdown="1">
<p markdown="1">Similar to `debugPrintMarkNeedsLayoutStacks`,
    but for excess painting. You can use the `debugPrintStack()`
    method from the `services` library to print your own stack
    traces on demand, if this kind of approach is useful to you.</p>
<p markdown="1">它和 `debugPrintMarkNeedsLayoutStacks` 類似，
    但用於多餘的繪製。如果有需要的話，您可以使用 `services` 庫中的
    `debugPrintStack()` 方法按需打印出堆疊的追蹤資訊。</p>

### Tracing Dart code performance

### 追蹤 Dart 程式碼效能

{{site.alert.note}}

  You can use the DevTools [Timeline view][] to perform traces.
  You can also import and export trace files into the Timeline view,
  but only files generated by DevTools.

  您可以使用 DevTools 中的 [Timeline view][] 來執行追蹤。
  您還可以將追蹤檔案匯入和匯出到 Timeline view 中，但這隻支援由 DevTools 產生的檔案。

{{site.alert.end}}

To perform custom performance traces programmatically and
measure wall/CPU time of arbitrary segments of Dart code
similar to what would be done on Android with [systrace][],
use `dart:developer` [Timeline][] utilities to wrap the
code you want to measure such as:

想要以程式設計方式執行自訂效能追蹤和測量任意程式碼片段的 wall/CPU 時間，
這類似於在 Android 上使用 [systrace][]，您可以使用 `dart:developer` 套件中的 
[Timeline][] 類提供的一些靜態方法包裹您想測量的程式碼，比如:

<?code-excerpt "lib/perf_trace.dart"?>
```dart
import 'dart:developer';

void main() {
  Timeline.startSync('interesting function');
  // iWonderHowLongThisTakes();
  Timeline.finishSync();
}
```

Then open your app's Observatory's timeline page, check the 'Dart'
recording option and perform the function you want to measure.

然後開啟您應用觀測臺的 timeline 頁面，勾選 “Dart” 記錄選項，並執行您想要測量的方法。

Refreshing the page displays the chronological timeline records
of your app in Chrome's [tracing tool][].

在 Chrome 的 [追蹤工具][tracing tool] 中重新整理頁面，展示您應用的 timeline 時序記錄。

Be sure to run your app in [profile mode][] to ensure that the
runtime performance characteristics closely match that of your
final product.

確保以 [效能模式][profile mode] 執行您的應用，來確保執行時的效能表現與您的最終產品相近。

## Performance overlay

## 效能圖層

{{site.alert.note}}

  You can toggle display of the performance overlay on
  your app using the **Performance Overlay** button in the
  [Flutter inspector][]. If you prefer to do it in code,
  use the following instructions.

  您可以使用 [Flutter inspector][] 中的 **Performance Overlay** 按鈕，
  來切換顯示的應用的效能圖層。
  如果您更喜歡用程式碼來完成它，請參考下面的說明。

{{site.alert.end}}

You can programmatically enable the PerformanceOverlay widget by
setting the `showPerformanceOverlay` property to `true` on the
[`MaterialApp`][], [`CupertinoApp`][], or [`WidgetsApp`][]
constructor:

您可以透過程式設計方式啟用 PerformanceOverlay widget，
在 [`MaterialApp`][]、[`CupertinoApp`][]或 [`WidgetsApp`][] 建構函式中，
將 `showPerformanceOverlay` 屬性設定為 `true` 即可。

<?code-excerpt "lib/performance_overlay.dart (PerfOverlay)" replace="/showPerformanceOverlay: true,/[[highlight]]$&[[\/highlight]]/g"?>
{% prettify dart %}
import 'package:flutter/material.dart';

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      [[highlight]]showPerformanceOverlay: true,[[/highlight]]
      title: 'My Awesome App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const MyHomePage(title: 'My Awesome App'),
    );
  }
}
{% endprettify %}

(If you're not using `MaterialApp`, `CupertinoApp`,
or `WidgetsApp`, you can get the same effect by wrapping your
application in a stack and putting a widget on your stack that was
created by calling [`PerformanceOverlay.allEnabled()`][].)

（如果您沒有使用 `MaterialApp`、`CupertinoApp` 或 `WidgetsApp`，可以透過將應用包裝在一個 Stack 中，
並透過呼叫 [`PerformanceOverlay.allEnabled()`][] 來建立一個 widget，來獲得相同的效果。）

For information on how to interpret the graphs in the overlay,
see [The performance overlay][] in
[Profiling Flutter performance][].

有關如何解釋浮層中的圖形的資訊，可以參見
[Flutter 效能分析][Profiling Flutter performance]
中的 [效能圖層][The performance overlay]。

## Widget alignment grid

## Widget 對齊網格

You can programmatically overlay a
[Material Design baseline grid][] on top of your app to
help verify alignments by using the
`debugShowMaterialGrid` argument in the
[`MaterialApp` constructor][].

您可以透過程式設計的方式將 [Material Design 基線網格][Material Design baseline grid]
覆蓋在應用的最上層來輔助對齊校驗，透過使用 [`MaterialApp` 建構函式][`MaterialApp` constructor]
中的 `debugShowMaterialGrid` 引數進行設定。

In non-Material applications, you can achieve a similar
effect by using a [`GridPaper`][] widget directly.

在非 Material 應用中，您可以透過直接使用 [`GridPaper`][] widget 來達到類似的效果。

[`GridPaper`]: {{site.api}}/flutter/widgets/GridPaper-class.html
[Material Design]: {{site.material}}/design/introduction
[`MaterialApp` constructor]: {{site.api}}/flutter/material/MaterialApp/MaterialApp.html
[Material Design baseline grid]: {{site.material}}/design/layout/spacing-methods.html#baseline
[`MaterialApp`]: {{site.api}}/flutter/material/MaterialApp/MaterialApp.html
[`WidgetsApp`]: {{site.api}}/flutter/widgets/WidgetsApp-class.html
[`CupertinoApp`]: {{site.api}}/flutter/cupertino/CupertinoApp-class.html
[`PerformanceOverlay.allEnabled()`]: {{site.api}}/flutter/widgets/PerformanceOverlay/PerformanceOverlay.allEnabled.html
[tracing tool]: https://www.chromium.org/developers/how-tos/trace-event-profiling-tool
[systrace]: {{site.android-dev}}/studio/profile/systrace
[Timeline]: {{site.dart.api}}/stable/dart-developer/Timeline-class.html
[`timeDilation`]: {{site.api}}/flutter/scheduler/timeDilation.html
[`debugPrintMarkNeedsLayoutStacks`]: {{site.api}}/flutter/rendering/debugPrintMarkNeedsLayoutStacks.html
[`debugPrintMarkNeedsPaintStacks`]: {{site.api}}/flutter/rendering/debugPrintMarkNeedsPaintStacks.html
[`debugRepaintRainbowEnabled`]: {{site.api}}/flutter/rendering/debugRepaintRainbowEnabled.html
[`debugPaintLayerBordersEnabled`]: {{site.api}}/flutter/rendering/debugPaintLayerBordersEnabled.html
[`debugPaintPointersEnabled`]: {{site.api}}/flutter/rendering/debugPaintPointersEnabled.html
[`debugPaintBaselinesEnabled`]: {{site.api}}/flutter/rendering/debugPaintBaselinesEnabled.html
[`debugPaintSizeEnabled`]: {{site.api}}/flutter/rendering/debugPaintSizeEnabled.html
[`debugPrintScheduleFrameStacks`]: {{site.api}}/flutter/scheduler/debugPrintScheduleFrameStacks.html
[`debugPrintBeginFrameBanner`]: {{site.api}}/flutter/scheduler/debugPrintBeginFrameBanner.html
[`debugPrintEndFrameBanner`]: {{site.api}}/flutter/scheduler/debugPrintEndFrameBanner.html
[`debugDumpSemanticsTree()`]: {{site.api}}/flutter/rendering/debugDumpSemanticsTree.html
[`debugDumpRenderTree()`]: {{site.api}}/flutter/rendering/debugDumpRenderTree.html
[`debugDumpLayerTree()`]: {{site.api}}/flutter/rendering/debugDumpLayerTree.html
[`debugDumpApp()`]: {{site.api}}/flutter/widgets/debugDumpApp.html
[`debugPrint()`]: {{site.api}}/flutter/foundation/debugPrint.html
[`RenderParagraph`]: {{site.api}}/flutter/rendering/RenderParagraph-class.html
[`RenderPositionedBox`]: {{site.api}}/flutter/rendering/RenderPositionedBox-class.html
[`Center`]: {{site.api}}/flutter/widgets/Center-class.html
[`RenderPadding`]: {{site.api}}/flutter/rendering/RenderPadding-class.html
[`RenderConstrainedBox`]: {{site.api}}/flutter/rendering/RenderConstrainedBox-class.html
[`TextButton`]: {{site.api}}/flutter/material/TextButton-class.html
[frame callback]: {{site.api}}/flutter/scheduler/SchedulerBinding/addPersistentFrameCallback.html
[render-fill]: {{site.api}}/flutter/rendering/Layer/debugFillProperties.html
[widget-fill]: {{site.api}}/flutter/widgets/Widget/debugFillProperties.html
[DiagnosticsProperty]: {{site.api}}/flutter/foundation/DiagnosticsProperty-class.html
[`setState()`]: {{site.api}}/flutter/widgets/State/setState.html
[`InkFeature`]: {{site.api}}/flutter/material/InkFeature-class.html
[`Material`]: {{site.api}}/flutter/material/Material-class.html
[Flutter's modes]: {{site.url}}/testing/build-modes
[profile mode]: {{site.url}}/testing/build-modes#profile
[debug mode]: {{site.url}}/testing/build-modes#debug
[release mode]: {{site.url}}/testing/build-modes#release
[DevTools]: {{site.url}}/development/tools/devtools
[Flutter inspector]: {{site.url}}/development/tools/devtools/inspector
[Logging view]: {{site.url}}/development/tools/devtools/logging
[Flutter enabled IDE/editor]: {{site.url}}/get-started/editor
[`log()`]: {{site.api}}/flutter/dart-developer/log.html
[Timeline view]: {{site.url}}/development/tools/devtools/performance
[Debugger]: {{site.url}}/development/tools/devtools/debugger
[Inspector view]: {{site.url}}/development/tools/devtools/inspector
[The performance overlay]: {{site.url}}/perf/ui-performance#the-performance-overlay
[Profiling Flutter performance]: {{site.url}}/perf/ui-performance
[Debugging]: {{site.url}}/testing/debugging
[file an issue]: {{site.github}}/flutter/devtools/issues
[rendering library]: {{site.api}}/flutter/rendering/rendering-library.html
