---
title: Flutter 狀態管理：使用 MobX
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/04/2fbde82783576.jpg)

_文 / Paul Halliday, developer.school 創始人_

眾所周知，狀態管理是每個軟體專案都需要持續迭代更新的方向。它並不是一個「一次性」的工作，
而需要不斷確保你遵循的最佳實踐能夠讓你的工程保持良好的可維護性。

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=62886932&bvid=BV1Gt411K7JD&cid=327927635&page=1&autoplay=false" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

要在 Flutter 中高效地使用 MobX ，需要遵循以下原則：

1. 我們能存取任意狀態中的可觀察物件（即在我們應用執行過程中發生變化的變數）。
1. 我們可以在 View 中展示這些狀態，並響應 Action 意圖。
1. 我們可以修改狀態，從而更新可觀察物件以及相應的 View。

那麼它的優勢在哪呢？答案是，透過 MobX 完成這一切將會變得超級簡單！codegen 工具可以幫我們完成絕大部分模版化的工作。

## 初始化專案

讓我們從建立一個全新的 Flutter 工程開始吧：

<!--skip-->
```shell
# New Flutter project
$ flutter create f_mobx && cd f_mobx
 
# Open in VS Code
$ code .
```

下一步，我們得在 `pubspec.yaml` 中拉取一些依賴 (`dependencies` 與 `dev_dependencies`):

<!--skip-->
```yaml
dependencies: 
 flutter:
    sdk: flutter
 
  mobx:
  flutter_mobx:
 
dev_dependencies:
  flutter_test:
    sdk: flutter
 
  build_runner: ^1.3.1
  mobx_codegen:
```

之後我們可以在 `main.dart` 中建立一個全新的 `MaterialApp` 以放置我們的 `CounterPage`。

<!--skip-->
```dart
import 'package:f_mobx/pages/counter_page.dart';
import 'package:flutter/material.dart';
 
void main() => runApp(MyApp());
 
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: CounterPage(),
    );
  }
}
```

下一步，我們需要在 `lib/pages/counter_page.dart` 中建立 `CounterPage`，並完成使用者介面的建構。其中包括了一個增加按鈕和一個減少按鈕。

<!--skip-->
```dart
import 'package:flutter/material.dart';
 
class CounterPage extends StatelessWidget {
 
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
        appBar: AppBar(
          title: Text('Flutter and MobX'),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Text(
                'Counter',
                style: TextStyle(fontSize: 30.0),
              ),
              Text(
                '0', 
                style: TextStyle(fontSize: 42.0),
              ),
              Row(
                mainAxisAlignment: MainAxisAlignment.center,
                children: <Widget>[
                  FlatButton.icon(
                    icon: Icon(Icons.add),
                    label: Text('Add'),
                    onPressed: () {},
                  ),
                  FlatButton.icon(
                    icon: Icon(Icons.remove),
                    label: Text('Remove'),
                    onPressed: () {},
                    ),
                  ),
                ],
              )
            ],
          ),
        ));
  }
}
```

## 建立計數器的狀態

太棒了！我們現在已經在 `lib/store/counter/counter.dart` 建立好了我們的計數器。現在，讓我們來看看程式碼，逐行進行解釋:

<!--skip-->
```dart
import 'package:mobx/mobx.dart';
 
// This is our generated file (we'll see this soon!)
part 'counter.g.dart';
 
// We expose this to be used throughout our project
class Counter = _Counter with _$Counter;
 
// Our store class
abstract class _Counter with Store {
  @observable
  int value = 1;
 
  @action
  void increment() {
    value++;
  }
 
  @action
  void decrement() {
    value--;
  }
}
```

1. 我們匯入了 `mobx.dart`，這樣就可以存取 Store 以及其他功能了。
1. 接下來，我們使用了 `part` 語法組合此類別的自動產生的部分。我們暫時還沒使用到產生器，但是別擔心，我們將會在下一個部分進行這個操作。
1. 接下來，我們將暴露 `Counter` 類，該類將與產生的與 MobX 繫結的 `_$Counter` 類一起使用。
1. 最後，我們使用 Store 類建立一個 `_Counter`，並定一個 `@observable` 屬性和 `@actions` 以確定 Store 可以與之互動的區域。
 
MobX 已經幫我們做了絕大部分繁瑣的事情，所以我們不需要關心底層是如何實現的。

現在我們已經有了 `Counter` 類，讓我們在終端的該工程目錄下透過下面的命令執行 `build_runner` 和 `mobx_codegen`：

<!--skip-->
```shell
$ flutter packages pub run build_runner watch
```
 
我們現在應該可以看到產生的 `counter.g.dart` 檔案。它看上去類似下面這樣：

<!--skip-->
```dart
part of 'counter.dart';
mixin _$Counter on _Counter, Store {
  final _$valueAtom = Atom(name: '_Counter.value');
 
  @override
  int get value {
    _$valueAtom.reportObserved();
    return super.value;
  }
 
  @override
  set value(int value) {
    _$valueAtom.context.checkIfStateModificationsAreAllowed(_$valueAtom);
    super.value = value;
    _$valueAtom.reportChanged();
  }
 
  final _$_CounterActionController = ActionController(name: '_Counter');
 
  @override
  void increment() {
    final _$actionInfo = _$_CounterActionController.startAction();
    try {
      return super.increment();
    } finally {
      _$_CounterActionController.endAction(_$actionInfo);
    }
  }
 
  @override
  void decrement() {
    final _$actionInfo = _$_CounterActionController.startAction();
    try {
      return super.decrement();
    } finally {
      _$_CounterActionController.endAction(_$actionInfo);
    }
  }
}
```

這些東西，我們都不需要自己來實現！是不是很棒呀？

## 與 Store 進行繫結

接下來，我們需要讓 `counter_page.dart` 繫結到 Counter store。讓我們再次看看它長什麼樣，然後進行深入探索：

<!--skip-->
```dart
import 'package:flut_mobx/store/counter/counter.dart';
import 'package:flutter/material.dart';
import 'package:flutter_mobx/flutter_mobx.dart';
 
class CounterPage extends StatelessWidget {
  final Counter counter = Counter();
 
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
        appBar: AppBar(
          title: Text('Flutter and MobX'),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Text(
                'Counter',
                style: TextStyle(fontSize: 30.0),
              ),
              Observer(
                builder: (_) =>
                    Text('${counter.value}', style: TextStyle(fontSize: 42.0)),
              ),
              Row(
                mainAxisAlignment: MainAxisAlignment.center,
                children: <Widget>[
                  FlatButton.icon(
                    icon: Icon(Icons.add),
                    label: Text('Add'),
                    onPressed: counter.increment,
                  ),
                  FlatButton.icon(
                    icon: Icon(Icons.remove),
                    label: Text('Remove'),
                    onPressed: counter.decrement,
                  ),
                ],
              )
            ],
          ),
        ));
  }
}
```

讓我們深入研究一下：

1. 我們匯入了 `flutter_mobx` 以及我們的 Counter store，所以之後我們可以用到他們。
1. 接下來，我們初始化了 `Counter`，並將其命名為 `counter`，之後我們就可以輕鬆監聽這個可觀察物件的值，或是發出 `actions：final Counter counter = Counter()`;
1. 我們使用 Observer 監聽 `counter.value` 的值。
1. 我們將 onPressed 事件繫結到 `counter.increment` 和 `counter.decrement`，它們會將 `action` 傳送到 Store。
 
上面這些程式碼結合起來就完成了我們小型的計數器應用！
 
## 總結

希望這篇 MobX 的介紹能夠幫到你。我目前仍在持續探索 Flutter 狀態管理的最佳實踐，所以我也非常期待將來能對該系列進一步的更新。
 
原文：[developer.school](https://developer.school/flutter-state-management-with-mobx/)，文章 & 影片本地化和釋出已獲得作者本人授權

## 致謝

- 本文作者：Paul Halliday
- 中文字幕翻譯: Alex、鑫磊
- 文章翻譯：加康、鑫磊
- 頭圖：Lynn
