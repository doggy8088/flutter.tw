---
title: Return data from a screen
title: 從一個頁面回傳資料
description: How to return data from a new screen.
description: 如何從新頁面返回資料。
tags: cookbook, 實用課程, 路由
keywords: 傳參, 回傳資料
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/navigation/returning_data/"?>

In some cases, you might want to return data from a new screen.
For example, say you push a new screen that presents two options to a user.
When the user taps an option, you want to inform the first screen
of the user's selection so that it can act on that information.

在某些場景下，我們需要在回退到上一屏時同時返回一些資料。
比如，我們跳轉到新的一屏，有兩個選項讓使用者選擇，
當用戶點選某個選項後會返回到第一屏，同時在第一屏可以知道使用者選擇的資訊。

You can do this with the [`Navigator.pop()`][]
method using the following steps:

你可以使用 [`Navigator.pop()`][] 來進行以下步驟：

## Directions

## 步驟

  1. Define the home screen

     建立主屏介面

  2. Add a button that launches the selection screen

     新增按鈕，點選時跳轉到選擇介面

  3. Show the selection screen with two buttons

     在選擇介面顯示兩個按鈕

  4. When a button is tapped, close the selection screen

     當任意一個按鈕被點選，關閉選擇介面回退到主屏介面

  5. Show a snackbar on the home screen with the selection

     在主屏介面顯示 snackbar ，展示選中的專案

## 1. Define the home screen

## 1. 建立主屏介面

The home screen displays a button. When tapped,
it launches the selection screen.

主屏介面顯示一個按鈕，當點選按鈕時跳轉到選擇介面。

<?code-excerpt "lib/main_step2.dart (HomeScreen)"?>
```dart
class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Returning Data Demo'),
      ),
      // Create the SelectionButton widget in the next step.
      body: const Center(
        child: SelectionButton(),
      ),
    );
  }
}
```

## 2. Add a button that launches the selection screen

## 2. 新增按鈕，點選時跳轉到選擇介面

Now, create the SelectionButton, which does the following:

接下來，我們建立 SelectionButton 按鈕，它有兩個功能：

  * Launches the SelectionScreen when it's tapped.

    點選時跳轉到選擇介面

  * Waits for the SelectionScreen to return a result.

    等待選擇介面給它返回結果

<?code-excerpt "lib/main_step2.dart (SelectionButton)"?>
```dart
class SelectionButton extends StatefulWidget {
  const SelectionButton({super.key});

  @override
  State<SelectionButton> createState() => _SelectionButtonState();
}

class _SelectionButtonState extends State<SelectionButton> {
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: () {
        _navigateAndDisplaySelection(context);
      },
      child: const Text('Pick an option, any option!'),
    );
  }

  Future<void> _navigateAndDisplaySelection(BuildContext context) async {
    // Navigator.push returns a Future that completes after calling
    // Navigator.pop on the Selection Screen.
    final result = await Navigator.push(
      context,
      // Create the SelectionScreen in the next step.
      MaterialPageRoute(builder: (context) => const SelectionScreen()),
    );
  }
}
```

## 3. Show the selection screen with two buttons

## 3. 在選擇介面顯示兩個按鈕

Now, build a selection screen that contains two buttons.
When a user taps a button,
that app closes the selection screen and lets the home
screen know which button was tapped.

現在來建構選擇介面，它包含兩個按鈕，
當任意一個按鈕被點選的時候，關閉選擇頁面回退到主屏介面，
並讓主屏介面知道哪個按鈕被點選了。

This step defines the UI.
The next step adds code to return data.

這一步我們來定義 UI，在下一步完成資料的返回。

<?code-excerpt "lib/main_step2.dart (SelectionScreen)"?>
```dart
class SelectionScreen extends StatelessWidget {
  const SelectionScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Pick an option'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Padding(
              padding: const EdgeInsets.all(8),
              child: ElevatedButton(
                onPressed: () {
                  // Pop here with "Yep"...
                },
                child: const Text('Yep!'),
              ),
            ),
            Padding(
              padding: const EdgeInsets.all(8),
              child: ElevatedButton(
                onPressed: () {
                  // Pop here with "Nope"...
                },
                child: const Text('Nope.'),
              ),
            )
          ],
        ),
      ),
    );
  }
}
```

## 4. When a button is tapped, close the selection screen

## 4. 當任意一個按鈕被點選，關閉選擇介面回退到主屏介面

Now, update the `onPressed()` callback for both of the buttons.
To return data to the first screen,
use the [`Navigator.pop()`][] method,
which accepts an optional second argument called `result`.
Any result is returned to the `Future` in the SelectionButton.

接下來我們來更新兩個按鈕的 `onPressed()` 回呼(Callback)函式，
使用 [`Navigator.pop()`][] 回退介面並返回資料給主屏介面。
`Navigator.pop()` 方法可以接受第二個引數 `result`，它是可選的，
如果傳遞了 `result`，資料將會透過 `Future` 方法的返回值傳遞。

### Yep button

### Yep 按鈕

<?code-excerpt "lib/main.dart (Yep)" replace="/^child: //g;/,$//g"?>
```dart
ElevatedButton(
  onPressed: () {
    // Close the screen and return "Yep!" as the result.
    Navigator.pop(context, 'Yep!');
  },
  child: const Text('Yep!'),
)
```

### Nope button

### Nope 按鈕

<?code-excerpt "lib/main.dart (Nope)" replace="/^child: //g;/,$//g"?>
```dart
ElevatedButton(
  onPressed: () {
    // Close the screen and return "Nope." as the result.
    Navigator.pop(context, 'Nope.');
  },
  child: const Text('Nope.'),
)
```

## 5. Show a snackbar on the home screen with the selection

## 5. 在主屏介面顯示一個 snackbar，展示選中的專案

Now that you're launching a selection screen and awaiting the result,
you'll want to do something with the information that's returned.

現在，我們跳轉到選擇介面並等待返回結果，當結果返回時我們可以做些事情。

In this case, show a snackbar displaying the result by using the
`_navigateAndDisplaySelection()` method in `SelectionButton`:

在本例中，我們用一個 snackbar 顯示結果，
我們來更新 `SelectionButton` 類中的 `_navigateAndDisplaySelection()` 方法。

<?code-excerpt "lib/main.dart (navigateAndDisplay)"?>
```dart
// A method that launches the SelectionScreen and awaits the result from
// Navigator.pop.
Future<void> _navigateAndDisplaySelection(BuildContext context) async {
  // Navigator.push returns a Future that completes after calling
  // Navigator.pop on the Selection Screen.
  final result = await Navigator.push(
    context,
    MaterialPageRoute(builder: (context) => const SelectionScreen()),
  );

  // When a BuildContext is used from a StatefulWidget, the mounted property
  // must be checked after an asynchronous gap.
  if (!mounted) return;

  // After the Selection Screen returns a result, hide any previous snackbars
  // and show the new result.
  ScaffoldMessenger.of(context)
    ..removeCurrentSnackBar()
    ..showSnackBar(SnackBar(content: Text('$result')));
}
```

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() {
  runApp(
    const MaterialApp(
      title: 'Returning Data',
      home: HomeScreen(),
    ),
  );
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Returning Data Demo'),
      ),
      body: const Center(
        child: SelectionButton(),
      ),
    );
  }
}

class SelectionButton extends StatefulWidget {
  const SelectionButton({super.key});

  @override
  State<SelectionButton> createState() => _SelectionButtonState();
}

class _SelectionButtonState extends State<SelectionButton> {
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: () {
        _navigateAndDisplaySelection(context);
      },
      child: const Text('Pick an option, any option!'),
    );
  }

  // A method that launches the SelectionScreen and awaits the result from
  // Navigator.pop.
  Future<void> _navigateAndDisplaySelection(BuildContext context) async {
    // Navigator.push returns a Future that completes after calling
    // Navigator.pop on the Selection Screen.
    final result = await Navigator.push(
      context,
      MaterialPageRoute(builder: (context) => const SelectionScreen()),
    );

    // When a BuildContext is used from a StatefulWidget, the mounted property
    // must be checked after an asynchronous gap.
    if (!mounted) return;

    // After the Selection Screen returns a result, hide any previous snackbars
    // and show the new result.
    ScaffoldMessenger.of(context)
      ..removeCurrentSnackBar()
      ..showSnackBar(SnackBar(content: Text('$result')));
  }
}

class SelectionScreen extends StatelessWidget {
  const SelectionScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Pick an option'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Padding(
              padding: const EdgeInsets.all(8),
              child: ElevatedButton(
                onPressed: () {
                  // Close the screen and return "Yep!" as the result.
                  Navigator.pop(context, 'Yep!');
                },
                child: const Text('Yep!'),
              ),
            ),
            Padding(
              padding: const EdgeInsets.all(8),
              child: ElevatedButton(
                onPressed: () {
                  // Close the screen and return "Nope." as the result.
                  Navigator.pop(context, 'Nope.');
                },
                child: const Text('Nope.'),
              ),
            )
          ],
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/returning-data.gif" alt="Returning data demo" class="site-mobile-screenshot" />
</noscript>


[`Navigator.pop()`]: {{site.api}}/flutter/widgets/Navigator/pop.html
