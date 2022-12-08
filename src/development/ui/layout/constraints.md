---
title: Understanding constraints
title: 深入理解 Flutter 佈局約束
description: Flutter's model for widget constraints, sizing, positioning, and how they interact.
description: 理解 Flutter widget 約束模型，瞭解它是如何確定自身的大小，位置以及影響彼此的。
toc: false
tags: 使用者介面,Flutter UI,佈局
keywords: 佈局約束,必讀,佈局範例
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="layout/constraints/"?>

<img src='/assets/images/docs/ui/layout/article-hero-image.png' class="mw-100" alt="Hero image from the article">

When someone learning Flutter asks you why some widget
with `width:100` isn't 100 pixels wide,
the default answer is to tell them to put that widget
inside of a `Center`, right?

我們會經常聽到一些開發者在學習 Flutter 時的疑惑：為什麼我設定了 `width:100`，
但是看上去卻不是 100 畫素寬呢。（注意，本文中的“畫素”均指的是邏輯畫素）
通常你會回答，將這個 Widget 放進 `Center` 中，對吧？

**Don't do that.**

**別這麼幹。**

If you do, they'll come back again and again,
asking why some `FittedBox` isn't working,
why that `Column` is overflowing, or what
`IntrinsicWidth` is supposed to be doing.

如果你這樣做了，他們會不斷找你詢問這樣的問題：為什麼 `FittedBox` 又不起作用了？
為什麼 `Column` 又溢位邊界，亦或是 `IntrinsicWidth` 應該做什麼。

Instead, first tell them that Flutter layout is very different
from HTML layout (which is probably where they’re coming from),
and then make them memorize the following rule:

其實我們首先應該做的，是告訴他們 Flutter 的佈局方式與 HTML 的佈局差異相當大
（這些開發者很可能是 Web 開發），然後要讓他們熟記這條規則：

<center><font size="+2">
  <t><b>Constraints go down. Sizes go up. Parent sets position.</b></t>
  <t>首先，上層 widget 向下層 widget 傳遞約束條件；<br/>
    然後，下層 widget 向上層 widget 傳遞大小資訊。<br/>
    最後，上層 widget 決定下層 widget 的位置。<br/>
  </t>
</font></center>

Flutter layout can’t really be understood without knowing
this rule, so Flutter developers should learn it early on.

如果我們在開發時無法熟練運用這條規則，在佈局時就不能完全理解其原理，
所以越早掌握這條規則越好！

In more detail:

更多細節：

* A widget gets its own **constraints** from its **parent**.
  A _constraint_ is just a set of 4 doubles:
  a minimum and maximum width, and a minimum and maximum height.

  Widget 會透過它的 **父級** 獲得自身的約束。
  約束實際上就是 4 個浮點型別的集合：
  最大/最小寬度，以及最大/最小高度。

* Then the widget goes through its own list of **children**.
  One by one, the widget tells its children what their
  **constraints** are (which can be different for each child),
  and then asks each child what size it wants to be.

  然後，這個 widget 將會逐個遍歷它的 **children** 列表。向子級傳遞
  **約束**（子級之間的約束可能會有所不同），然後詢問它的每一個子級需要用於佈局的大小。

* Then, the widget positions its **children**
  (horizontally in the `x` axis, and vertically in the `y` axis),
  one by one.

  然後，這個 widget 就會對它子級的 **children** 逐個進行佈局。
  （水平方向是 `x` 軸，豎直是 `y` 軸）

* And, finally, the widget tells its parent about its own **size**
  (within the original constraints, of course).

  最後，widget 將會把它的大小資訊向上傳遞至父 widget（包括其原始約束條件）。

For example, if a composed widget contains a column
with some padding, and wants to lay out its two children
as follows:

例如，如果一個 widget 中包含了一個具有 padding 的 Column，
並且要對 Column 的子 widget 進行如下的佈局：

<img src='/assets/images/docs/ui/layout/children.png' class="mw-100" alt="Visual layout">

The negotiation goes something like this:

那麼談判將會像這樣：

**Widget**: "Hey parent, what are my constraints?"

**Widget**: "嘿！我的父級。我的約束是多少？"

**Parent**: "You must be from `80` to `300` pixels wide,
   and `30` to `85` tall."

**Parent**: "你的寬度必須在 `80` 到 `300` 畫素之間，高度必須在 `30` 到 `85` 之間。"

**Widget**: "Hmmm, since I want to have `5` pixels of padding,
   then my children can have at most `290` pixels of width
   and `75` pixels of height."

**Widget**: "嗯...我想要 `5` 個畫素的內邊距，這樣我的子級能最多擁有 `290` 個畫素寬度和 `75` 個畫素高度。"

**Widget**: "Hey first child, You must be from `0` to `290`
   pixels wide, and `0` to `75` tall."

**Widget**: "嘿，我的第一個子級，你的寬度必須要在 `0` 到 `290`，長度在 `0` 到 `75` 之間。"

**First child**: "OK, then I wish to be `290` pixels wide,
   and `20` pixels tall."

**First child**: "OK，那我想要 `290` 畫素的寬度，`20` 個畫素的長度。"

**Widget**: "Hmmm, since I want to put my second child below the
   first one, this leaves only `55` pixels of height for
   my second child."

**Widget**: "嗯...由於我想要將我的第二個子級放在第一個子級下面，所以我們僅剩 `55` 個畫素的高度給第二個子級了。"

**Widget**: "Hey second child, You must be from `0` to `290` wide,
   and `0` to `55` tall."

**Widget**: "嘿，我的第二個子級，你的寬度必須要在 `0` 到 `290`，長度在 `0` 到 `55` 之間。"

**Second child**: "OK, I wish to be `140` pixels wide,
   and `30` pixels tall."

**Second child**: "OK，那我想要 `140` 畫素的寬度，`30` 個畫素的長度。"

**Widget**: "Very well. My first child has position `x: 5` and `y: 5`,
   and my second child has `x: 80` and `y: 25`."

**Widget**: "很好。我的第一個子級將被放在 `x: 5` & `y: 5` 的位置，
   而我的第二個子級將在 `x: 80` & `y: 25` 的位置。"

**Widget**: "Hey parent, I’ve decided that my size is going to be `300`
   pixels wide, and `60` pixels tall."

**Widget**: "嘿，我的父級，我決定我的大小為 `300` 畫素寬度，`60` 畫素高度。"

## Limitations

## 限制

As a result of the layout rule mentioned above,
Flutter’s layout engine has a few important limitations:

正如上述所介紹的佈局規則中所說的那樣，
Flutter 的佈局引擎有一些重要限制：

* A widget can decide its own size only within the
  constraints given to it by its parent.
  This means a widget usually **can't have any
  size it wants**.

  一個 widget 僅在其父級給其約束的情況下才能決定自身的大小。
  這意味著 widget 通常情況下 **不能任意獲得其想要的大小**。

* A widget **can’t know and doesn’t decide its own position
  in the screen**, since it’s the widget’s parent who decides
  the position of the widget.

  一個 widget **無法知道，也不需要決定其在螢幕中的位置**。
  因為它的位置是由其父級決定的。

* Since the parent’s size and position, in its turn,
  also depends on its own parent, it’s impossible to
  precisely define the size and position of any widget
  without taking into consideration the tree as a whole.

  當輪到父級決定其大小和位置的時候，同樣的也取決於它自身的父級。
  所以，在不考慮整棵樹的情況下，幾乎不可能精確定義任何 widget 的大小和位置。

* If a child wants a different size from its parent and 
  the parent doesn't have enough information to align it,
  then the child's size might be ignored.
  **Be specific when defining alignment.**

  如果子級想要擁有和父級不同的大小，然而父級沒有足夠的空間對其進行佈局的話，
  子級的設定的大小可能會不生效。
  **這時請明確指定它的對齊方式**
  
## Examples

## 範例

For an interactive experience, use the following DartPad.
Use the numbered horizontal scrolling bar to switch between
29 different examples.

下面的範例由 DartPad 提供，具有良好的互動體驗。
使用下面水平捲軸的編號切換 29 個不同的範例。

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-starting_code
import 'package:flutter/material.dart';

void main() => runApp(const HomePage());

const red = Colors.red;
const green = Colors.green;
const blue = Colors.blue;
const big = TextStyle(fontSize: 30);

//////////////////////////////////////////////////

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return const FlutterLayoutArticle([
      Example1(),
      Example2(),
      Example3(),
      Example4(),
      Example5(),
      Example6(),
      Example7(),
      Example8(),
      Example9(),
      Example10(),
      Example11(),
      Example12(),
      Example13(),
      Example14(),
      Example15(),
      Example16(),
      Example17(),
      Example18(),
      Example19(),
      Example20(),
      Example21(),
      Example22(),
      Example23(),
      Example24(),
      Example25(),
      Example26(),
      Example27(),
      Example28(),
      Example29(),
    ]);
  }
}

//////////////////////////////////////////////////

abstract class Example extends StatelessWidget {
  const Example({super.key});

  String get code;

  String get explanation;
}

//////////////////////////////////////////////////

class FlutterLayoutArticle extends StatefulWidget {
  const FlutterLayoutArticle(
    this.examples, {
    super.key,
  });

  final List<Example> examples;

  @override
  State<FlutterLayoutArticle> createState() => _FlutterLayoutArticleState();
}

//////////////////////////////////////////////////

class _FlutterLayoutArticleState extends State<FlutterLayoutArticle> {
  late int count;
  late Widget example;
  late String code;
  late String explanation;

  @override
  void initState() {
    count = 1;
    code = const Example1().code;
    explanation = const Example1().explanation;

    super.initState();
  }

  @override
  void didUpdateWidget(FlutterLayoutArticle oldWidget) {
    super.didUpdateWidget(oldWidget);
    var example = widget.examples[count - 1];
    code = example.code;
    explanation = example.explanation;
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Flutter Layout Article',
      home: SafeArea(
        child: Material(
          color: Colors.black,
          child: FittedBox(
            child: Container(
              width: 400,
              height: 670,
              color: const Color(0xFFCCCCCC),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.center,
                children: [
                  Expanded(
                      child: ConstrainedBox(
                          constraints: const BoxConstraints.tightFor(
                              width: double.infinity, height: double.infinity),
                          child: widget.examples[count - 1])),
                  Container(
                    height: 50,
                    width: double.infinity,
                    color: Colors.black,
                    child: SingleChildScrollView(
                      scrollDirection: Axis.horizontal,
                      child: Row(
                        mainAxisSize: MainAxisSize.min,
                        children: [
                          for (int i = 0; i < widget.examples.length; i++)
                            Container(
                              width: 58,
                              padding:
                                  const EdgeInsets.only(left: 4.0, right: 4.0),
                              child: button(i + 1),
                            ),
                        ],
                      ),
                    ),
                  ),
                  Container(
                    height: 273,
                    color: Colors.grey[50],
                    child: Scrollbar(
                      child: SingleChildScrollView(
                        key: ValueKey(count),
                        child: Padding(
                          padding: const EdgeInsets.all(10.0),
                          child: Column(
                            children: [
                              Center(child: Text(code)),
                              const SizedBox(height: 15),
                              Text(
                                explanation,
                                style: TextStyle(
                                    color: Colors.blue[900],
                                    fontStyle: FontStyle.italic),
                              ),
                            ],
                          ),
                        ),
                      ),
                    ),
                  ),
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }

  Widget button(int exampleNumber) {
    return Button(
      key: ValueKey('button$exampleNumber'),
      isSelected: count == exampleNumber,
      exampleNumber: exampleNumber,
      onPressed: () {
        showExample(
          exampleNumber,
          widget.examples[exampleNumber - 1].code,
          widget.examples[exampleNumber - 1].explanation,
        );
      },
    );
  }

  void showExample(int exampleNumber, String code, String explanation) {
    setState(() {
      count = exampleNumber;
      this.code = code;
      this.explanation = explanation;
    });
  }
}

//////////////////////////////////////////////////

class Button extends StatelessWidget {
  final bool isSelected;
  final int exampleNumber;
  final VoidCallback onPressed;

  const Button({
    super.key,
    required this.isSelected,
    required this.exampleNumber,
    required this.onPressed,
  });

  @override
  Widget build(BuildContext context) {
    return TextButton(
      style: TextButton.styleFrom(
        foregroundColor: Colors.white,
        backgroundColor: isSelected ? Colors.grey : Colors.grey[800],
      ),
      child: Text(exampleNumber.toString()),
      onPressed: () {
        Scrollable.ensureVisible(
          context,
          duration: const Duration(milliseconds: 350),
          curve: Curves.easeOut,
          alignment: 0.5,
        );
        onPressed();
      },
    );
  }
}
//////////////////////////////////////////////////

class Example1 extends Example {
  const Example1({super.key});

  @override
  final code = 'Container(color: red)';

  @override
  final explanation = 'The screen is the parent of the Container, '
      'and it forces the Container to be exactly the same size as the screen.'
      '\n\n'
      'So the Container fills the screen and paints it red.';

  @override
  Widget build(BuildContext context) {
    return Container(color: red);
  }
}

//////////////////////////////////////////////////

class Example2 extends Example {
  const Example2({super.key});

  @override
  final code = 'Container(width: 100, height: 100, color: red)';
  @override
  final String explanation =
      'The red Container wants to be 100x100, but it can\'t, '
      'because the screen forces it to be exactly the same size as the screen.'
      '\n\n'
      'So the Container fills the screen.';

  @override
  Widget build(BuildContext context) {
    return Container(width: 100, height: 100, color: red);
  }
}

//////////////////////////////////////////////////

class Example3 extends Example {
  const Example3({super.key});

  @override
  final code = 'Center(\n'
      '   child: Container(width: 100, height: 100, color: red))';
  @override
  final String explanation =
      'The screen forces the Center to be exactly the same size as the screen, '
      'so the Center fills the screen.'
      '\n\n'
      'The Center tells the Container that it can be any size it wants, but not bigger than the screen.'
      'Now the Container can indeed be 100x100.';

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Container(width: 100, height: 100, color: red),
    );
  }
}

//////////////////////////////////////////////////

class Example4 extends Example {
  const Example4({super.key});

  @override
  final code = 'Align(\n'
      '   alignment: Alignment.bottomRight,\n'
      '   child: Container(width: 100, height: 100, color: red))';
  @override
  final String explanation =
      'This is different from the previous example in that it uses Align instead of Center.'
      '\n\n'
      'Align also tells the Container that it can be any size it wants, but if there is empty space it won\'t center the Container. '
      'Instead, it aligns the Container to the bottom-right of the available space.';

  @override
  Widget build(BuildContext context) {
    return Align(
      alignment: Alignment.bottomRight,
      child: Container(width: 100, height: 100, color: red),
    );
  }
}

//////////////////////////////////////////////////

class Example5 extends Example {
  const Example5({super.key});

  @override
  final code = 'Center(\n'
      '   child: Container(\n'
      '              color: red,\n'
      '              width: double.infinity,\n'
      '              height: double.infinity))';
  @override
  final String explanation =
      'The screen forces the Center to be exactly the same size as the screen, '
      'so the Center fills the screen.'
      '\n\n'
      'The Center tells the Container that it can be any size it wants, but not bigger than the screen.'
      'The Container wants to be of infinite size, but since it can\'t be bigger than the screen, it just fills the screen.';

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Container(
          width: double.infinity, height: double.infinity, color: red),
    );
  }
}

//////////////////////////////////////////////////

class Example6 extends Example {
  const Example6({super.key});

  @override
  final code = 'Center(child: Container(color: red))';
  @override
  final String explanation =
      'The screen forces the Center to be exactly the same size as the screen, '
      'so the Center fills the screen.'
      '\n\n'
      'The Center tells the Container that it can be any size it wants, but not bigger than the screen.'
      '\n\n'
      'Since the Container has no child and no fixed size, it decides it wants to be as big as possible, so it fills the whole screen.'
      '\n\n'
      'But why does the Container decide that? '
      'Simply because that\'s a design decision by those who created the Container widget. '
      'It could have been created differently, and you have to read the Container documentation to understand how it behaves, depending on the circumstances. ';

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Container(color: red),
    );
  }
}

//////////////////////////////////////////////////

class Example7 extends Example {
  const Example7({super.key});

  @override
  final code = 'Center(\n'
      '   child: Container(color: red\n'
      '      child: Container(color: green, width: 30, height: 30)))';
  @override
  final String explanation =
      'The screen forces the Center to be exactly the same size as the screen, '
      'so the Center fills the screen.'
      '\n\n'
      'The Center tells the red Container that it can be any size it wants, but not bigger than the screen.'
      'Since the red Container has no size but has a child, it decides it wants to be the same size as its child.'
      '\n\n'
      'The red Container tells its child that it can be any size it wants, but not bigger than the screen.'
      '\n\n'
      'The child is a green Container that wants to be 30x30.'
      '\n\n'
      'Since the red `Container` has no size but has a child, it decides it wants to be the same size as its child. '
      'The red color isn\'t visible, since the green Container entirely covers all of the red Container.';

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Container(
        color: red,
        child: Container(color: green, width: 30, height: 30),
      ),
    );
  }
}

//////////////////////////////////////////////////

class Example8 extends Example {
  const Example8({super.key});

  @override
  final code = 'Center(\n'
      '   child: Container(color: red\n'
      '      padding: const EdgeInsets.all(20.0),\n'
      '      child: Container(color: green, width: 30, height: 30)))';
  @override
  final String explanation =
      'The red Container sizes itself to its children size, but it takes its own padding into consideration. '
      'So it is also 30x30 plus padding. '
      'The red color is visible because of the padding, and the green Container has the same size as in the previous example.';

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Container(
        padding: const EdgeInsets.all(20.0),
        color: red,
        child: Container(color: green, width: 30, height: 30),
      ),
    );
  }
}

//////////////////////////////////////////////////

class Example9 extends Example {
  const Example9({super.key});

  @override
  final code = 'ConstrainedBox(\n'
      '   constraints: BoxConstraints(\n'
      '              minWidth: 70, minHeight: 70,\n'
      '              maxWidth: 150, maxHeight: 150),\n'
      '      child: Container(color: red, width: 10, height: 10)))';
  @override
  final String explanation =
      'You might guess that the Container has to be between 70 and 150 pixels, but you would be wrong. '
      'The ConstrainedBox only imposes ADDITIONAL constraints from those it receives from its parent.'
      '\n\n'
      'Here, the screen forces the ConstrainedBox to be exactly the same size as the screen, '
      'so it tells its child Container to also assume the size of the screen, '
      'thus ignoring its \'constraints\' parameter.';

  @override
  Widget build(BuildContext context) {
    return ConstrainedBox(
      constraints: const BoxConstraints(
        minWidth: 70,
        minHeight: 70,
        maxWidth: 150,
        maxHeight: 150,
      ),
      child: Container(color: red, width: 10, height: 10),
    );
  }
}

//////////////////////////////////////////////////

class Example10 extends Example {
  const Example10({super.key});

  @override
  final code = 'Center(\n'
      '   child: ConstrainedBox(\n'
      '      constraints: BoxConstraints(\n'
      '                 minWidth: 70, minHeight: 70,\n'
      '                 maxWidth: 150, maxHeight: 150),\n'
      '        child: Container(color: red, width: 10, height: 10))))';
  @override
  final String explanation =
      'Now, Center allows ConstrainedBox to be any size up to the screen size.'
      '\n\n'
      'The ConstrainedBox imposes ADDITIONAL constraints from its \'constraints\' parameter onto its child.'
      '\n\n'
      'The Container must be between 70 and 150 pixels. It wants to have 10 pixels, so it will end up having 70 (the MINIMUM).';

  @override
  Widget build(BuildContext context) {
    return Center(
      child: ConstrainedBox(
        constraints: const BoxConstraints(
          minWidth: 70,
          minHeight: 70,
          maxWidth: 150,
          maxHeight: 150,
        ),
        child: Container(color: red, width: 10, height: 10),
      ),
    );
  }
}

//////////////////////////////////////////////////

class Example11 extends Example {
  const Example11({super.key});

  @override
  final code = 'Center(\n'
      '   child: ConstrainedBox(\n'
      '      constraints: BoxConstraints(\n'
      '                 minWidth: 70, minHeight: 70,\n'
      '                 maxWidth: 150, maxHeight: 150),\n'
      '        child: Container(color: red, width: 1000, height: 1000))))';
  @override
  final String explanation =
      'Center allows ConstrainedBox to be any size up to the screen size.'
      'The ConstrainedBox imposes ADDITIONAL constraints from its \'constraints\' parameter onto its child'
      '\n\n'
      'The Container must be between 70 and 150 pixels. It wants to have 1000 pixels, so it ends up having 150 (the MAXIMUM).';

  @override
  Widget build(BuildContext context) {
    return Center(
      child: ConstrainedBox(
        constraints: const BoxConstraints(
          minWidth: 70,
          minHeight: 70,
          maxWidth: 150,
          maxHeight: 150,
        ),
        child: Container(color: red, width: 1000, height: 1000),
      ),
    );
  }
}

//////////////////////////////////////////////////

class Example12 extends Example {
  const Example12({super.key});

  @override
  final code = 'Center(\n'
      '   child: ConstrainedBox(\n'
      '      constraints: BoxConstraints(\n'
      '                 minWidth: 70, minHeight: 70,\n'
      '                 maxWidth: 150, maxHeight: 150),\n'
      '        child: Container(color: red, width: 100, height: 100))))';
  @override
  final String explanation =
      'Center allows ConstrainedBox to be any size up to the screen size.'
      'ConstrainedBox imposes ADDITIONAL constraints from its \'constraints\' parameter onto its child.'
      '\n\n'
      'The Container must be between 70 and 150 pixels. It wants to have 100 pixels, and that\'s the size it has, since that\'s between 70 and 150.';

  @override
  Widget build(BuildContext context) {
    return Center(
      child: ConstrainedBox(
        constraints: const BoxConstraints(
          minWidth: 70,
          minHeight: 70,
          maxWidth: 150,
          maxHeight: 150,
        ),
        child: Container(color: red, width: 100, height: 100),
      ),
    );
  }
}

//////////////////////////////////////////////////

class Example13 extends Example {
  const Example13({super.key});

  @override
  final code = 'UnconstrainedBox(\n'
      '   child: Container(color: red, width: 20, height: 50));';
  @override
  final String explanation =
      'The screen forces the UnconstrainedBox to be exactly the same size as the screen.'
      'However, the UnconstrainedBox lets its child Container be any size it wants.';

  @override
  Widget build(BuildContext context) {
    return UnconstrainedBox(
      child: Container(color: red, width: 20, height: 50),
    );
  }
}

//////////////////////////////////////////////////

class Example14 extends Example {
  const Example14({super.key});

  @override
  final code = 'UnconstrainedBox(\n'
      '   child: Container(color: red, width: 4000, height: 50));';
  @override
  final String explanation =
      'The screen forces the UnconstrainedBox to be exactly the same size as the screen, '
      'and UnconstrainedBox lets its child Container be any size it wants.'
      '\n\n'
      'Unfortunately, in this case the Container has 4000 pixels of width and is too big to fit in the UnconstrainedBox, '
      'so the UnconstrainedBox displays the much dreaded "overflow warning".';

  @override
  Widget build(BuildContext context) {
    return UnconstrainedBox(
      child: Container(color: red, width: 4000, height: 50),
    );
  }
}

//////////////////////////////////////////////////

class Example15 extends Example {
  const Example15({super.key});

  @override
  final code = 'OverflowBox(\n'
      '   minWidth: 0.0,'
      '   minHeight: 0.0,'
      '   maxWidth: double.infinity,'
      '   maxHeight: double.infinity,'
      '   child: Container(color: red, width: 4000, height: 50));';
  @override
  final String explanation =
      'The screen forces the OverflowBox to be exactly the same size as the screen, '
      'and OverflowBox lets its child Container be any size it wants.'
      '\n\n'
      'OverflowBox is similar to UnconstrainedBox, and the difference is that it won\'t display any warnings if the child doesn\'t fit the space.'
      '\n\n'
      'In this case the Container is 4000 pixels wide, and is too big to fit in the OverflowBox, '
      'but the OverflowBox simply shows as much as it can, with no warnings given.';

  @override
  Widget build(BuildContext context) {
    return OverflowBox(
      minWidth: 0.0,
      minHeight: 0.0,
      maxWidth: double.infinity,
      maxHeight: double.infinity,
      child: Container(color: red, width: 4000, height: 50),
    );
  }
}

//////////////////////////////////////////////////

class Example16 extends Example {
  const Example16({super.key});

  @override
  final code = 'UnconstrainedBox(\n'
      '   child: Container(color: Colors.red, width: double.infinity, height: 100));';
  @override
  final String explanation =
      'This won\'t render anything, and you\'ll see an error in the console.'
      '\n\n'
      'The UnconstrainedBox lets its child be any size it wants, '
      'however its child is a Container with infinite size.'
      '\n\n'
      'Flutter can\'t render infinite sizes, so it throws an error with the following message: '
      '"BoxConstraints forces an infinite width."';

  @override
  Widget build(BuildContext context) {
    return UnconstrainedBox(
      child: Container(color: Colors.red, width: double.infinity, height: 100),
    );
  }
}

//////////////////////////////////////////////////

class Example17 extends Example {
  const Example17({super.key});

  @override
  final code = 'UnconstrainedBox(\n'
      '   child: LimitedBox(maxWidth: 100,\n'
      '      child: Container(color: Colors.red,\n'
      '                       width: double.infinity, height: 100));';
  @override
  final String explanation = 'Here you won\'t get an error anymore, '
      'because when the LimitedBox is given an infinite size by the UnconstrainedBox, '
      'it passes a maximum width of 100 down to its child.'
      '\n\n'
      'If you swap the UnconstrainedBox for a Center widget, '
      'the LimitedBox won\'t apply its limit anymore (since its limit is only applied when it gets infinite constraints), '
      'and the width of the Container is allowed to grow past 100.'
      '\n\n'
      'This explains the difference between a LimitedBox and a ConstrainedBox.';

  @override
  Widget build(BuildContext context) {
    return UnconstrainedBox(
      child: LimitedBox(
        maxWidth: 100,
        child: Container(
          color: Colors.red,
          width: double.infinity,
          height: 100,
        ),
      ),
    );
  }
}

//////////////////////////////////////////////////

class Example18 extends Example {
  const Example18({super.key});

  @override
  final code = 'FittedBox(\n'
      '   child: Text(\'Some Example Text.\'));';
  @override
  final String explanation =
      'The screen forces the FittedBox to be exactly the same size as the screen.'
      'The Text has some natural width (also called its intrinsic width) that depends on the amount of text, its font size, and so on.'
      '\n\n'
      'The FittedBox lets the Text be any size it wants, '
      'but after the Text tells its size to the FittedBox, '
      'the FittedBox scales the Text until it fills all of the available width.';

  @override
  Widget build(BuildContext context) {
    return const FittedBox(
      child: Text('Some Example Text.'),
    );
  }
}

//////////////////////////////////////////////////

class Example19 extends Example {
  const Example19({super.key});

  @override
  final code = 'Center(\n'
      '   child: FittedBox(\n'
      '      child: Text(\'Some Example Text.\')));';
  @override
  final String explanation =
      'But what happens if you put the FittedBox inside of a Center widget? '
      'The Center lets the FittedBox be any size it wants, up to the screen size.'
      '\n\n'
      'The FittedBox then sizes itself to the Text, and lets the Text be any size it wants.'
      '\n\n'
      'Since both FittedBox and the Text have the same size, no scaling happens.';

  @override
  Widget build(BuildContext context) {
    return const Center(
      child: FittedBox(
        child: Text('Some Example Text.'),
      ),
    );
  }
}

////////////////////////////////////////////////////

class Example20 extends Example {
  const Example20({super.key});

  @override
  final code = 'Center(\n'
      '   child: FittedBox(\n'
      '      child: Text(\'…\')));';
  @override
  final String explanation =
      'However, what happens if FittedBox is inside of a Center widget, but the Text is too large to fit the screen?'
      '\n\n'
      'FittedBox tries to size itself to the Text, but it can\'t be bigger than the screen. '
      'It then assumes the screen size, and resizes Text so that it fits the screen, too.';

  @override
  Widget build(BuildContext context) {
    return const Center(
      child: FittedBox(
        child: Text(
            'This is some very very very large text that is too big to fit a regular screen in a single line.'),
      ),
    );
  }
}

//////////////////////////////////////////////////

class Example21 extends Example {
  const Example21({super.key});

  @override
  final code = 'Center(\n'
      '   child: Text(\'…\'));';
  @override
  final String explanation = 'If, however, you remove the FittedBox, '
      'the Text gets its maximum width from the screen, '
      'and breaks the line so that it fits the screen.';

  @override
  Widget build(BuildContext context) {
    return const Center(
      child: Text(
          'This is some very very very large text that is too big to fit a regular screen in a single line.'),
    );
  }
}

//////////////////////////////////////////////////

class Example22 extends Example {
  const Example22({super.key});

  @override
  final code = 'FittedBox(\n'
      '   child: Container(\n'
      '      height: 20.0, width: double.infinity));';
  @override
  final String explanation =
      'FittedBox can only scale a widget that is BOUNDED (has non-infinite width and height).'
      'Otherwise, it won\'t render anything, and you\'ll see an error in the console.';

  @override
  Widget build(BuildContext context) {
    return FittedBox(
      child: Container(
        height: 20.0,
        width: double.infinity,
        color: Colors.red,
      ),
    );
  }
}

//////////////////////////////////////////////////

class Example23 extends Example {
  const Example23({super.key});

  @override
  final code = 'Row(children:[\n'
      '   Container(color: red, child: Text(\'Hello!\'))\n'
      '   Container(color: green, child: Text(\'Goodbye!\'))]';
  @override
  final String explanation =
      'The screen forces the Row to be exactly the same size as the screen.'
      '\n\n'
      'Just like an UnconstrainedBox, the Row won\'t impose any constraints onto its children, '
      'and instead lets them be any size they want.'
      '\n\n'
      'The Row then puts them side-by-side, and any extra space remains empty.';

  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        Container(color: red, child: const Text('Hello!', style: big)),
        Container(color: green, child: const Text('Goodbye!', style: big)),
      ],
    );
  }
}

//////////////////////////////////////////////////

class Example24 extends Example {
  const Example24({super.key});

  @override
  final code = 'Row(children:[\n'
      '   Container(color: red, child: Text(\'…\'))\n'
      '   Container(color: green, child: Text(\'Goodbye!\'))]';
  @override
  final String explanation =
      'Since the Row won\'t impose any constraints onto its children, '
      'it\'s quite possible that the children might be too big to fit the available width of the Row.'
      'In this case, just like an UnconstrainedBox, the Row displays the "overflow warning".';

  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        Container(
          color: red,
          child: const Text(
            'This is a very long text that '
            'won\'t fit the line.',
            style: big,
          ),
        ),
        Container(color: green, child: const Text('Goodbye!', style: big)),
      ],
    );
  }
}

//////////////////////////////////////////////////

class Example25 extends Example {
  const Example25({super.key});

  @override
  final code = 'Row(children:[\n'
      '   Expanded(\n'
      '       child: Container(color: red, child: Text(\'…\')))\n'
      '   Container(color: green, child: Text(\'Goodbye!\'))]';
  @override
  final String explanation =
      'When a Row\'s child is wrapped in an Expanded widget, the Row won\'t let this child define its own width anymore.'
      '\n\n'
      'Instead, it defines the Expanded width according to the other children, and only then the Expanded widget forces the original child to have the Expanded\'s width.'
      '\n\n'
      'In other words, once you use Expanded, the original child\'s width becomes irrelevant, and is ignored.';

  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        Expanded(
          child: Center(
            child: Container(
              color: red,
              child: const Text(
                'This is a very long text that won\'t fit the line.',
                style: big,
              ),
            ),
          ),
        ),
        Container(color: green, child: const Text('Goodbye!', style: big)),
      ],
    );
  }
}

//////////////////////////////////////////////////

class Example26 extends Example {
  const Example26({super.key});

  @override
  final code = 'Row(children:[\n'
      '   Expanded(\n'
      '       child: Container(color: red, child: Text(\'…\')))\n'
      '   Expanded(\n'
      '       child: Container(color: green, child: Text(\'Goodbye!\'))]';
  @override
  final String explanation =
      'If all of Row\'s children are wrapped in Expanded widgets, each Expanded has a size proportional to its flex parameter, '
      'and only then each Expanded widget forces its child to have the Expanded\'s width.'
      '\n\n'
      'In other words, Expanded ignores the preffered width of its children.';

  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        Expanded(
          child: Container(
            color: red,
            child: const Text(
              'This is a very long text that won\'t fit the line.',
              style: big,
            ),
          ),
        ),
        Expanded(
          child: Container(
            color: green,
            child: const Text(
              'Goodbye!',
              style: big,
            ),
          ),
        ),
      ],
    );
  }
}

//////////////////////////////////////////////////

class Example27 extends Example {
  const Example27({super.key});

  @override
  final code = 'Row(children:[\n'
      '   Flexible(\n'
      '       child: Container(color: red, child: Text(\'…\')))\n'
      '   Flexible(\n'
      '       child: Container(color: green, child: Text(\'Goodbye!\'))]';
  @override
  final String explanation =
      'The only difference if you use Flexible instead of Expanded, '
      'is that Flexible lets its child be SMALLER than the Flexible width, '
      'while Expanded forces its child to have the same width of the Expanded.'
      '\n\n'
      'But both Expanded and Flexible ignore their children\'s width when sizing themselves.'
      '\n\n'
      'This means that it\'s IMPOSSIBLE to expand Row children proportionally to their sizes. '
      'The Row either uses the exact child\'s width, or ignores it completely when you use Expanded or Flexible.';

  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        Flexible(
          child: Container(
            color: red,
            child: const Text(
              'This is a very long text that won\'t fit the line.',
              style: big,
            ),
          ),
        ),
        Flexible(
          child: Container(
            color: green,
            child: const Text(
              'Goodbye!',
              style: big,
            ),
          ),
        ),
      ],
    );
  }
}

//////////////////////////////////////////////////

class Example28 extends Example {
  const Example28({super.key});

  @override
  final code = 'Scaffold(\n'
      '   body: Container(color: blue,\n'
      '   child: Column(\n'
      '      children: [\n'
      '         Text(\'Hello!\'),\n'
      '         Text(\'Goodbye!\')])))';

  @override
  final String explanation =
      'The screen forces the Scaffold to be exactly the same size as the screen, '
      'so the Scaffold fills the screen.'
      '\n\n'
      'The Scaffold tells the Container that it can be any size it wants, but not bigger than the screen.'
      '\n\n'
      'When a widget tells its child that it can be smaller than a certain size, '
      'we say the widget supplies "loose" constraints to its child. More on that later.';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        color: blue,
        child: Column(
          children: const [
            Text('Hello!'),
            Text('Goodbye!'),
          ],
        ),
      ),
    );
  }
}

//////////////////////////////////////////////////

class Example29 extends Example {
  const Example29({super.key});

  @override
  final code = 'Scaffold(\n'
      '   body: Container(color: blue,\n'
      '   child: SizedBox.expand(\n'
      '      child: Column(\n'
      '         children: [\n'
      '            Text(\'Hello!\'),\n'
      '            Text(\'Goodbye!\')]))))';

  @override
  final String explanation =
      'If you want the Scaffold\'s child to be exactly the same size as the Scaffold itself, '
      'you can wrap its child with SizedBox.expand.'
      '\n\n'
      'When a widget tells its child that it must be of a certain size, '
      'we say the widget supplies "tight" constraints to its child. More on that later.';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SizedBox.expand(
        child: Container(
          color: blue,
          child: Column(
            children: const [
              Text('Hello!'),
              Text('Goodbye!'),
            ],
          ),
        ),
      ),
    );
  }
}

//////////////////////////////////////////////////
```

If you prefer, you can grab the code from
[this GitHub repo][].

如果你願意的話，你還可以在
[這個 Github 儲存庫中][this GitHub repo] 獲取其程式碼。

The examples are explained in the following sections.

以下各節將介紹這些範例。

### Example 1

### 範例 1

<img src='/assets/images/docs/ui/layout/layout-1.png' class="mw-100" alt="Example 1 layout">

<?code-excerpt "lib/main.dart (Example1)" replace="/(return |;)//g"?>
```dart
Container(color: red)
```

The screen is the parent of the `Container`, and it
forces the `Container` to be exactly the same size as the screen.

整個螢幕作為 `Container` 的父級，並且強制 `Container` 變成和螢幕一樣的大小。

So the `Container` fills the screen and paints it red.

所以這個 `Container` 充滿了整個螢幕，並繪製成紅色。

### Example 2

### 範例 2

<img src='/assets/images/docs/ui/layout/layout-2.png' class="mw-100" alt="Example 2 layout">

<?code-excerpt "lib/main.dart (Example2)" replace="/(return |;)//g"?>
```dart
Container(width: 100, height: 100, color: red)
```

The red `Container` wants to be 100 × 100,
but it can’t, because the screen forces it to be
exactly the same size as the screen.

紅色的 `Container` 想要變成 100 x 100 的大小，
但是它無法變成，因為螢幕強制它變成和螢幕一樣的大小。

So the `Container` fills the screen.

所以 `Container` 充滿了整個螢幕。

### Example 3

### 範例 3

<img src='/assets/images/docs/ui/layout/layout-3.png' class="mw-100" alt="Example 3 layout">

<?code-excerpt "lib/main.dart (Example3)" replace="/(return |;)//g"?>
```dart
Center(
  child: Container(width: 100, height: 100, color: red),
)
```

The screen forces the `Center` to be exactly the same size
as the screen, so the `Center` fills the screen.

螢幕強制 `Center` 變得和螢幕一樣大，所以 `Center` 充滿了螢幕。

The `Center` tells the `Container` that it can be any size it
wants, but not bigger than the screen. Now the `Container`
can indeed be 100 × 100.

然後 `Center` 告訴 `Container` 可以變成任意大小，但是不能超出螢幕。
現在，`Container` 可以真正變成 100 × 100 大小了。

### Example 4

### 範例 4

<img src='/assets/images/docs/ui/layout/layout-4.png' class="mw-100" alt="Example 4 layout">

<?code-excerpt "lib/main.dart (Example4)" replace="/(return |;)//g"?>
```dart
Align(
  alignment: Alignment.bottomRight,
  child: Container(width: 100, height: 100, color: red),
)
```

This is different from the previous example in that it uses
`Align` instead of `Center`.

與上一個範例不同的是，我們使用了 `Align` 而不是 `Center`。

`Align` also tells the `Container` that it can be any size it
wants, but if there is empty space it won't center the `Container`.
Instead, it aligns the container to the bottom-right of the
available space.

`Align` 同樣也告訴 `Container`，你可以變成任意大小。
但是，如果還留有空白空間的話，它不會居中 `Container`。
相反，它將會在允許的空間內，把 `Container` 放在右下角（bottomRight）。

### Example 5

### 範例 5

<img src='/assets/images/docs/ui/layout/layout-5.png' class="mw-100" alt="Example 5 layout">

<?code-excerpt "lib/main.dart (Example5)" replace="/(return |;)//g"?>
```dart
Center(
  child: Container(
      width: double.infinity, height: double.infinity, color: red),
)
```

The screen forces the `Center` to be exactly the
same size as the screen, so the `Center` fills the screen.

螢幕強制 `Center` 變得和螢幕一樣大，所以 `Center` 充滿螢幕。

The `Center` tells the `Container` that it can be any size it wants,
but not bigger than the screen. The `Container` wants to be
of infinite size, but since it can’t be bigger than the screen,
it just fills the screen.

然後 `Center` 告訴 `Container` 可以變成任意大小，但是不能超出螢幕。
現在，`Container` 想要無限的大小，但是由於它不能比螢幕更大，
所以就僅充滿螢幕。

### Example 6

### 範例 6

<img src='/assets/images/docs/ui/layout/layout-6.png' class="mw-100" alt="Example 6 layout">

<?code-excerpt "lib/main.dart (Example6)" replace="/(return |;)//g"?>
```dart
Center(
  child: Container(color: red),
)
```

The screen forces the `Center` to be exactly the
same size as the screen, so the `Center` fills the screen.

螢幕強制 `Center` 變得和螢幕一樣大，所以 `Center` 充滿螢幕。

The `Center` tells the `Container` that it can be any
size it wants, but not bigger than the screen.
Since the `Container` has no child and no fixed size,
it decides it wants to be as big as possible,
so it fills the whole screen.

然後 `Center` 告訴 `Container` 可以變成任意大小，但是不能超出螢幕。
由於 `Container` 沒有子級而且沒有固定大小，所以它決定能有多大就有多大，
所以它充滿了整個螢幕。

But why does the `Container` decide that?
Simply because that’s a design decision by those who
created the `Container` widget. It could have been
created differently, and you have to read the
[`Container` documentation][] to understand how it
behaves, depending on the circumstances.

但是，為什麼 `Container` 做出了這個決定？
非常簡單，因為這個決定是由 `Container` widget 的建立者決定的。
可能會因創造者而異，而且你還得閱讀 
[`Container` 文件][`Container` documentation]
來理解不同場景下它的行為。

### Example 7

### 範例 7

<img src='/assets/images/docs/ui/layout/layout-7.png' class="mw-100" alt="Example 7 layout">

<?code-excerpt "lib/main.dart (Example7)" replace="/(return |;)//g"?>
```dart
Center(
  child: Container(
    color: red,
    child: Container(color: green, width: 30, height: 30),
  ),
)
```

The screen forces the `Center` to be exactly the same
size as the screen, so the `Center` fills the screen.

螢幕強制 `Center` 變得和螢幕一樣大，所以 `Center` 充滿螢幕。

The `Center` tells the red `Container` that it can be any size
it wants, but not bigger than the screen. Since the red
`Container` has no size but has a child,
it decides it wants to be the same size as its child.

然後 `Center` 告訴紅色的 `Container` 可以變成任意大小，但是不能超出螢幕。
由於 `Container` 沒有固定大小但是有子級，所以它決定變成它 child 的大小。

The red `Container` tells its child that it can be any size
it wants, but not bigger than the screen.

然後紅色的 `Container` 告訴它的 child 可以變成任意大小，但是不能超出螢幕。

The child is a green `Container` that wants to
be 30 × 30. Given that the red `Container` sizes itself to
the size of its child, it is also 30 × 30.
The red color isn't visible because the green `Container`
entirely covers the red `Container`.

而它的 child 是一個想要 30 × 30 大小綠色的 `Container`。由於紅色的 `Container`
和其子級一樣大，所以也變為 30 × 30。由於綠色的 `Container` 完全覆蓋了紅色 `Container`，
所以你看不見它了。

### Example 8

### 範例 8

<img src='/assets/images/docs/ui/layout/layout-8.png' class="mw-100" alt="Example 8 layout">

<?code-excerpt "lib/main.dart (Example8)" replace="/(return |;)//g"?>
```dart
Center(
  child: Container(
    padding: const EdgeInsets.all(20.0),
    color: red,
    child: Container(color: green, width: 30, height: 30),
  ),
)
```

The red `Container` sizes itself to its children's size,
but it takes its own padding into consideration.
So it is also 30 × 30 plus padding.
The red color is visible because of the padding,
and the green `Container` has the same size as
in the previous example.

紅色 `Container` 變為其子級的大小，但是它將其 padding 帶入了約束的計算中。
所以它有一個 30 x 30 的外邊距。由於這個外邊距，所以現在你能看見紅色了。
而綠色的 `Container` 則還是和之前一樣。

### Example 9

### 範例 9

<img src='/assets/images/docs/ui/layout/layout-9.png' class="mw-100" alt="Example 9 layout">

<?code-excerpt "lib/main.dart (Example9)" replace="/(return |;)//g"?>
```dart
ConstrainedBox(
  constraints: const BoxConstraints(
    minWidth: 70,
    minHeight: 70,
    maxWidth: 150,
    maxHeight: 150,
  ),
  child: Container(color: red, width: 10, height: 10),
)
```

You might guess that the `Container` has to be
between 70 and 150 pixels, but you would be wrong.
The `ConstrainedBox` only imposes **additional** constraints
from those it receives from its parent.

你可能會猜想 `Container` 的尺寸會在 70 到 150 畫素之間，但並不是這樣。
`ConstrainedBox` 僅對其從其父級接收到的約束下施加其他約束。

Here, the screen forces the `ConstrainedBox` to be exactly
the same size as the screen, so it tells its child `Container`
to also assume the size of the screen, thus ignoring its
`constraints` parameter.

在這裡，螢幕迫使 `ConstrainedBox` 與螢幕大小完全相同，
因此它告訴其子 `Widget` 也以螢幕大小作為約束，
從而忽略了其 `constraints` 引數帶來的影響。

### Example 10

### 範例 10

<img src='/assets/images/docs/ui/layout/layout-10.png' class="mw-100" alt="Example 10 layout">

<?code-excerpt "lib/main.dart (Example10)" replace="/(return |;)//g"?>
```dart
Center(
  child: ConstrainedBox(
    constraints: const BoxConstraints(
      minWidth: 70,
      minHeight: 70,
      maxWidth: 150,
      maxHeight: 150,
    ),
    child: Container(color: red, width: 10, height: 10),
  ),
)
```

Now, `Center` allows `ConstrainedBox` to be any size up to
the screen size. The `ConstrainedBox` imposes **additional**
constraints from its `constraints` parameter onto its child.

現在，`Center` 允許 `ConstrainedBox` 達到螢幕可允許的任意大小。
`ConstrainedBox` 將 `constraints` 引數帶來的約束附加到其子物件上。

The Container must be between 70 and 150 pixels.
It wants to have 10 pixels,
so it ends up having 70 (the minimum).

Container 必須介於 70 到 150 畫素之間。雖然它希望自己有 10 個畫素大小，
但最終獲得了 70 個畫素（最小為 70）。

### Example 11

### 範例 11

<img src='/assets/images/docs/ui/layout/layout-11.png' class="mw-100" alt="Example 11 layout">

<?code-excerpt "lib/main.dart (Example11)" replace="/(return |;)//g"?>
```dart
Center(
  child: ConstrainedBox(
    constraints: const BoxConstraints(
      minWidth: 70,
      minHeight: 70,
      maxWidth: 150,
      maxHeight: 150,
    ),
    child: Container(color: red, width: 1000, height: 1000),
  ),
)
```

`Center` allows `ConstrainedBox` to be any size up to the
screen size. The `ConstrainedBox` imposes **additional**
constraints from its `constraints` parameter onto its child.

現在，`Center` 允許 `ConstrainedBox` 達到螢幕可允許的任意大小。
`ConstrainedBox` 將 `constraints` 引數帶來的約束附加到其子物件上。

The `Container` must be between 70 and 150 pixels.
It wants to have 1000 pixels,
so it ends up having 150 (the maximum).

`Container` 必須介於 70 到 150 畫素之間。
雖然它希望自己有 1000 個畫素大小，
但最終獲得了 150 個畫素（最大為 150）。

### Example 12

### 範例 12

<img src='/assets/images/docs/ui/layout/layout-12.png' class="mw-100" alt="Example 12 layout">

<?code-excerpt "lib/main.dart (Example12)" replace="/(return |;)//g"?>
```dart
Center(
  child: ConstrainedBox(
    constraints: const BoxConstraints(
      minWidth: 70,
      minHeight: 70,
      maxWidth: 150,
      maxHeight: 150,
    ),
    child: Container(color: red, width: 100, height: 100),
  ),
)
```

`Center` allows `ConstrainedBox` to be any size up to the
screen size. The `ConstrainedBox` imposes **additional**
constraints from its `constraints` parameter onto its child.

現在，`Center` 允許 `ConstrainedBox` 達到螢幕可允許的任意大小。
`ConstrainedBox` 將 `constraints` 引數帶來的約束附加到其子物件上。

The `Container` must be between 70 and 150 pixels.
It wants to have 100 pixels, and that’s the size it has,
since that’s between 70 and 150.

`Container` 必須介於 70 到 150 畫素之間。
雖然它希望自己有 100 個畫素大小，
因為 100 介於 70 至 150 的範圍內，所以最終獲得了 100 個畫素。

### Example 13

### 範例 13

<img src='/assets/images/docs/ui/layout/layout-13.png' class="mw-100" alt="Example 13 layout">

<?code-excerpt "lib/main.dart (Example13)" replace="/(return |;)//g"?>
```dart
UnconstrainedBox(
  child: Container(color: red, width: 20, height: 50),
)
```

The screen forces the `UnconstrainedBox` to be exactly
the same size as the screen. However, the `UnconstrainedBox`
lets its child `Container` be any size it wants.

螢幕強制 `UnconstrainedBox` 變得和螢幕一樣大，而 `UnconstrainedBox` 允許其子級的 `Container` 可以變為任意大小。

### Example 14

### 範例 14

<img src='/assets/images/docs/ui/layout/layout-14.png' class="mw-100" alt="Example 14 layout">

<?code-excerpt "lib/main.dart (Example14)" replace="/(return |;)//g"?>
```dart
UnconstrainedBox(
  child: Container(color: red, width: 4000, height: 50),
)
```

The screen forces the `UnconstrainedBox` to be exactly
the same size as the screen, and `UnconstrainedBox`
lets its child `Container` be any size it wants.

螢幕強制 `UnconstrainedBox` 變得和螢幕一樣大，
而 `UnconstrainedBox` 允許其子級的 `Container` 可以變為任意大小。

Unfortunately, in this case the `Container` is
4000 pixels wide and is too big to fit in
the `UnconstrainedBox`, so the `UnconstrainedBox` displays
the much dreaded "overflow warning".

不幸的是，在這種情況下，容器的寬度為 4000 畫素，
這實在是太大，以至於無法容納在 `UnconstrainedBox` 中，
因此 `UnconstrainedBox` 將顯示溢位警告（overflow warning）。

### Example 15

### 範例 15

<img src='/assets/images/docs/ui/layout/layout-15.png' class="mw-100" alt="Example 15 layout">

<?code-excerpt "lib/main.dart (Example15)" replace="/(return |;)//g"?>
```dart
OverflowBox(
  minWidth: 0.0,
  minHeight: 0.0,
  maxWidth: double.infinity,
  maxHeight: double.infinity,
  child: Container(color: red, width: 4000, height: 50),
)
```

The screen forces the `OverflowBox` to be exactly the same
size as the screen, and `OverflowBox` lets its child `Container`
be any size it wants.

螢幕強制 `OverflowBox` 變得和螢幕一樣大，
並且 `OverflowBox` 允許其子容器設定為任意大小。

`OverflowBox` is similar to `UnconstrainedBox`;
the difference is that it won’t display any warnings
if the child doesn’t fit the space.

`OverflowBox` 與 `UnconstrainedBox` 類似，但不同的是，
如果其子級超出該空間，它將不會顯示任何警告。

In this case, the `Container` has 4000 pixels of width,
and is too big to fit in the `OverflowBox`,
but the `OverflowBox` simply shows as much as it can,
with no warnings given.

在這種情況下，容器的寬度為 4000 畫素，並且太大而無法容納在 `OverflowBox` 中，
但是 `OverflowBox` 會全部顯示，而不會發出警告。

### Example 16

### 範例 16

<img src='/assets/images/docs/ui/layout/layout-16.png' class="mw-100" alt="Example 16 layout">

<?code-excerpt "lib/main.dart (Example16)" replace="/(return |;)//g"?>
```dart
UnconstrainedBox(
  child: Container(color: Colors.red, width: double.infinity, height: 100),
)
```

This won’t render anything, and you'll see an error in the console.

這將不會渲染任何東西，而且你能在控制檯看到錯誤資訊。

The `UnconstrainedBox` lets its child be any size it wants,
however its child is a `Container` with infinite size.

`UnconstrainedBox` 讓它的子級決定成為任何大小，
但是其子級是一個具有無限大小的 `Container`。

Flutter can’t render infinite sizes, so it throws an error with
the following message: `BoxConstraints forces an infinite width.`

Flutter 無法渲染無限大的東西，所以它丟擲以下錯誤：
`BoxConstraints forces an infinite width.`（盒子約束強制使用了無限的寬度）

### Example 17

### 範例 17

<img src='/assets/images/docs/ui/layout/layout-17.png' class="mw-100" alt="Example 17 layout">

<?code-excerpt "lib/main.dart (Example17)" replace="/(return |;)//g"?>
```dart
UnconstrainedBox(
  child: LimitedBox(
    maxWidth: 100,
    child: Container(
      color: Colors.red,
      width: double.infinity,
      height: 100,
    ),
  ),
)
```

Here you won’t get an error anymore,
because when the `LimitedBox` is given an
infinite size by the `UnconstrainedBox`;
it passes a maximum width of 100 down to its child.

這次你就不會遇到報錯了。
`UnconstrainedBox` 給 `LimitedBox` 一個無限的大小；
但它向其子級傳遞了最大為 100 的約束。

If you swap the `UnconstrainedBox` for a `Center` widget,
the `LimitedBox` won't apply its limit anymore
(since its limit is only applied when it gets infinite
constraints), and the width of the `Container`
is allowed to grow past 100.

如果你將 `UnconstrainedBox` 替換為 `Center`，
則`LimitedBox` 將不再應用其限制（因為其限制僅在獲得無限約束時才適用），
並且容器的寬度允許超過 100。

This explains the difference between a `LimitedBox`
and a `ConstrainedBox`.

上面的範例解釋了 `LimitedBox` 和 `ConstrainedBox` 之間的區別。

### Example 18

### 範例 18

<img src='/assets/images/docs/ui/layout/layout-18.png' class="mw-100" alt="Example 18 layout">

<?code-excerpt "lib/main.dart (Example18)" replace="/(return |;)//g"?>
```dart
const FittedBox(
  child: Text('Some Example Text.'),
)
```

The screen forces the `FittedBox` to be exactly the same
size as the screen. The `Text` has some natural width
(also called its intrinsic width) that depends on the
amount of text, its font size, and so on.

螢幕強制 `FittedBox` 變得和螢幕一樣大，
而 `Text` 則是有一個自然寬度（也被稱作 intrinsic 寬度），
它取決於文字數量，字型大小等因素。

The `FittedBox` lets the `Text` be any size it wants,
but after the `Text` tells its size to the `FittedBox`,
the `FittedBox` scales the Text until it fills all of
the available width.

`FittedBox` 讓 `Text` 可以變為任意大小。
但是在 `Text` 告訴 `FittedBox` 其大小後，
`FittedBox` 縮放文字直到填滿所有可用寬度。

### Example 19

### 範例 19

<img src='/assets/images/docs/ui/layout/layout-19.png' class="mw-100" alt="Example 19 layout">

<?code-excerpt "lib/main.dart (Example19)" replace="/(return |;)//g"?>
```dart
const Center(
  child: FittedBox(
    child: Text('Some Example Text.'),
  ),
)
```

But what happens if you put the `FittedBox` inside of a
`Center` widget? The `Center` lets the `FittedBox`
be any size it wants, up to the screen size.

但如果你將 `FittedBox` 放進 `Center` widget 中會發生什麼？
`Center` 將會讓 `FittedBox` 能夠變為任意大小，
取決於螢幕大小。

The `FittedBox` then sizes itself to the `Text`,
and lets the `Text` be any size it wants.
Since both `FittedBox` and the `Text` have the same size,
no scaling happens.

`FittedBox` 然後會根據 `Text` 調整自己的大小，
然後讓 `Text` 可以變為所需的任意大小，
由於二者具有同一大小，因此不會發生縮放。

### Example 20

### 範例 20

<img src='/assets/images/docs/ui/layout/layout-20.png' class="mw-100" alt="Example 20 layout">

<?code-excerpt "lib/main.dart (Example20)" replace="/(return |;)//g"?>
```dart
const Center(
  child: FittedBox(
    child: Text(
        'This is some very very very large text that is too big to fit a regular screen in a single line.'),
  ),
)
```

However, what happens if `FittedBox` is inside of a `Center`
widget, but the `Text` is too large to fit the screen?

然而，如果 `FittedBox` 位於 `Center` 中，
但 `Text` 太大而超出螢幕，會發生什麼？

`FittedBox` tries to size itself to the `Text`,
but it can't be bigger than the screen.
It then assumes the screen size,
and resizes `Text` so that it fits the screen, too.

FittedBox 會嘗試根據 `Text` 大小調整大小，
但不能大於螢幕大小。然後假定螢幕大小，
並調整 `Text` 的大小以使其也適合螢幕。

### Example 21

### 範例 21

<img src='/assets/images/docs/ui/layout/layout-21.png' class="mw-100" alt="Example 21 layout">

<?code-excerpt "lib/main.dart (Example21)" replace="/(return |;)//g"?>
```dart
const Center(
  child: Text(
      'This is some very very very large text that is too big to fit a regular screen in a single line.'),
)
```

If, however, you remove the `FittedBox`, the `Text`
gets its maximum width from the screen,
and breaks the line so that it fits the screen.

然而，如果你刪除了 `FittedBox`，
`Text` 則會從螢幕上獲取其最大寬度，
並在合適的地方換行。

### Example 22

### 範例 22

<img src='/assets/images/docs/ui/layout/layout-22.png' class="mw-100" alt="Example 22 layout">

<?code-excerpt "lib/main.dart (Example22)" replace="/(return |;)//g"?>
```dart
FittedBox(
  child: Container(
    height: 20.0,
    width: double.infinity,
    color: Colors.red,
  ),
)
```

`FittedBox` can only scale a widget that is bounded
(has non-infinite width and height). Otherwise,
it won’t render anything,
and you'll see an error in the console.

`FittedBox` 只能在有限制的寬高中
對子 widget 進行縮放（寬度和高度不會變得無限大）。
否則，它將無法渲染任何內容，並且你會在控制檯中看到錯誤。

### Example 23

### 範例 23

<img src='/assets/images/docs/ui/layout/layout-23.png' class="mw-100" alt="Example 23 layout">

<?code-excerpt "lib/main.dart (Example23)" replace="/(return |;)//g"?>
```dart
Row(
  children: [
    Container(color: red, child: const Text('Hello!', style: big)),
    Container(color: green, child: const Text('Goodbye!', style: big)),
  ],
)
```

The screen forces the `Row` to be exactly the same size
as the screen.

螢幕強制 `Row` 變得和螢幕一樣大，所以 `Row` 充滿螢幕。

Just like an `UnconstrainedBox`, the `Row` won’t
impose any constraints onto its children,
and instead lets them be any size they want.
The `Row` then puts them side-by-side,
and any extra space remains empty.

和 `UnconstrainedBox` 一樣，
`Row` 也不會對其子代施加任何約束，
而是讓它們成為所需的任意大小。
`Row` 然後將它們並排放置，
任何多餘的空間都將保持空白。

### Example 24

### 範例 24

<img src='/assets/images/docs/ui/layout/layout-24.png' class="mw-100" alt="Example 24 layout">

<?code-excerpt "lib/main.dart (Example24)" replace="/(return |;)//g"?>
```dart
Row(
  children: [
    Container(
      color: red,
      child: const Text(
        'This is a very long text that '
        'won\'t fit the line.',
        style: big,
      ),
    ),
    Container(color: green, child: const Text('Goodbye!', style: big)),
  ],
)
```

Since `Row` won’t impose any constraints onto its children,
it’s quite possible that the children might be too big to fit
the available width of the `Row`. In this case, just like an
`UnconstrainedBox`, the `Row` displays the "overflow warning".

由於 `Row` 不會對其子級施加任何約束，
因此它的 children 很有可能太大
而超出 `Row` 的可用寬度。在這種情況下，
`Row` 會和 `UnconstrainedBox` 一樣顯示溢位警告。

### Example 25

### 範例 25

<img src='/assets/images/docs/ui/layout/layout-25.png' class="mw-100" alt="Example 25 layout">

<?code-excerpt "lib/main.dart (Example25)" replace="/(return |;)//g"?>
```dart
Row(
  children: [
    Expanded(
      child: Center(
        child: Container(
          color: red,
          child: const Text(
            'This is a very long text that won\'t fit the line.',
            style: big,
          ),
        ),
      ),
    ),
    Container(color: green, child: const Text('Goodbye!', style: big)),
  ],
)
```

When a `Row`'s child is wrapped in an `Expanded` widget,
the `Row` won't let this child define its own width anymore.

當 `Row` 的子級被包裹在了 `Expanded` widget 之後，
`Row` 將不會再讓其決定自身的寬度了。

Instead, it defines the `Expanded` width according to the
other children, and only then the `Expanded` widget forces
the original child to have the `Expanded`'s width.

取而代之的是，`Row` 會根據所有 `Expanded` 的子級
來計算其該有的寬度。

In other words, once you use `Expanded`,
the original child’s width becomes irrelevant, and is ignored.

換句話說，一旦你使用 `Expanded`，
子級自身的寬度就變得無關緊要，直接會被忽略掉。

### Example 26

### 範例 26

<img src='/assets/images/docs/ui/layout/layout-26.png' class="mw-100" alt="Example 26 layout">

<?code-excerpt "lib/main.dart (Example26)" replace="/(return |;)//g"?>
```dart
Row(
  children: [
    Expanded(
      child: Container(
        color: red,
        child: const Text(
          'This is a very long text that won\'t fit the line.',
          style: big,
        ),
      ),
    ),
    Expanded(
      child: Container(
        color: green,
        child: const Text(
          'Goodbye!',
          style: big,
        ),
      ),
    ),
  ],
)
```

If all of `Row`'s children are wrapped in `Expanded` widgets,
each `Expanded` has a size proportional to its flex parameter,
and only then each `Expanded` widget forces its child to have
the `Expanded`'s width.

如果所有 `Row` 的子級都被包裹了 `Expanded` widget，
每一個 `Expanded` 大小都會與其 flex 因子成比例，
並且 `Expanded` widget 將會強制其子級具有與 `Expanded` 相同的寬度。

In other words, `Expanded` ignores the preferred width of
its children.

換句話說，`Expanded` 忽略了其子 `Widget` 想要的寬度。

### Example 27

### 範例 27

<img src='/assets/images/docs/ui/layout/layout-27.png' class="mw-100" alt="Example 27 layout">

<?code-excerpt "lib/main.dart (Example27)" replace="/(return |;)//g"?>
```dart
Row(
  children: [
    Flexible(
      child: Container(
        color: red,
        child: const Text(
          'This is a very long text that won\'t fit the line.',
          style: big,
        ),
      ),
    ),
    Flexible(
      child: Container(
        color: green,
        child: const Text(
          'Goodbye!',
          style: big,
        ),
      ),
    ),
  ],
)
```

The only difference if you use `Flexible` instead of `Expanded`,
is that `Flexible` lets its child have the same or smaller
width than the `Flexible` itself, while `Expanded` forces
its child to have the exact same width of the `Expanded`.
But both `Expanded` and `Flexible` ignore their children's width
when sizing themselves.

如果你使用 `Flexible` 而不是 `Expanded` 的話，
唯一的區別是，`Flexible` 會讓其子級具有與
`Flexible` 相同或者更小的寬度。
而 `Expanded` 將會強制其子級具有和
`Expanded` 相同的寬度。
但無論是 `Expanded` 還是 `Flexible`
在它們決定子級大小時都會忽略其寬度。

{{site.alert.note}}

  This means that it’s impossible to expand `Row` children
  proportionally to their sizes. The `Row` either uses
  the exact child’s width, or ignores it completely
  when you use `Expanded` or `Flexible`.

  這意味著，`Row` 要麼使用子級的寬度，
  要麼使用`Expanded` 和 `Flexible` 從而忽略子級的寬度。

{{site.alert.end}}

### Example 28

### 範例 28

<img src='/assets/images/docs/ui/layout/layout-28.png' class="mw-100" alt="Example 28 layout">

<?code-excerpt "lib/main.dart (Example28)" replace="/(return |;)//g"?>
```dart
Scaffold(
  body: Container(
    color: blue,
    child: Column(
      children: const [
        Text('Hello!'),
        Text('Goodbye!'),
      ],
    ),
  ),
)
```

The screen forces the `Scaffold` to be exactly the same size
as the screen, so the `Scaffold` fills the screen.
The `Scaffold` tells the `Container` that it can be any size it wants,
but not bigger than the screen.

螢幕強制 `Scaffold` 變得和螢幕一樣大，
所以 `Scaffold` 充滿螢幕。
然後 `Scaffold` 告訴 `Container` 可以變為任意大小，
但不能超出螢幕。

{{site.alert.note}}

  When a widget tells its child that it can be smaller than a
  certain size, we say the widget supplies _loose_ constraints
  to its child. More on that later.

  當一個 widget 告訴其子級可以比自身更小的話，
  我們通常稱這個 widget 對其子級使用 **寬鬆約束（loose）**。

{{site.alert.end}}

### Example 29

### 範例 29

<img src='/assets/images/docs/ui/layout/layout-29.png' class="mw-100" alt="Example 29 layout">

<?code-excerpt "lib/main.dart (Example29)" replace="/(return |;)//g"?>
```dart
Scaffold(
  body: SizedBox.expand(
    child: Container(
      color: blue,
      child: Column(
        children: const [
          Text('Hello!'),
          Text('Goodbye!'),
        ],
      ),
    ),
  ),
)
```

If you want the `Scaffold`'s child to be exactly the same size
as the `Scaffold` itself, you can wrap its child with
`SizedBox.expand`.

如果你想要 `Scaffold` 的子級變得和 `Scaffold` 本身一樣大的話，
你可以將這個子級外包裹一個 `SizedBox.expand`。

{{site.alert.note}}

  When a widget tells its child that it must be of
  a certain size, we say the widget supplies _tight_
  constraints to its child.

  當一個 widget 告訴它的子級必須變成某個大小的時候，
  我們通常稱這個 widget 對其子級使用 **嚴格約束（tight）**。

{{site.alert.end}}


## Tight vs. loose constraints

## 嚴格約束（Tight） vs 寬鬆約束（loose）

It’s very common to hear that some constraint is
"tight" or "loose", so it’s worth knowing what that means.

以後你經常會聽到一些約束為嚴格約束或寬鬆約束，
你花點時間來弄明白它們是值得的。

A _tight_ constraint offers a single possibility,
an exact size. In other words, a tight constraint
has its maximum width equal to its minimum width;
and has its maximum height equal to its minimum height.

嚴格約束給你了一種獲得確切大小的選擇。
換句話來說就是，它的最大/最小寬度是一致的，高度也一樣。

If you go to Flutter’s `box.dart` file and search for
the `BoxConstraints` constructors, you'll find the
following:

如果你到 Flutter 的 `box.dart` 檔案中搜索
`BoxConstraints` 構造器，你會發現以下內容：

<!-- skip -->
```dart
BoxConstraints.tight(Size size)
   : minWidth = size.width,
     maxWidth = size.width,
     minHeight = size.height,
     maxHeight = size.height;
```

If you revisit [Example 2](#example-2) above,
it tells us that the screen forces the red
`Container` to be exactly the same size as the screen.
The screen does that, of course, by passing tight
constraints to the `Container`.

如果你重新閱讀 [範例 2](#example-2)，
它告訴我們螢幕強制 `Container` 變得和螢幕一樣大。
為何螢幕能夠做到這一點，
原因就是給 `Container` 傳遞了嚴格約束。

A _loose_ constraint, on the other hand,
sets the **maximum** width and height, but lets the widget
be as small as it wants. In other words,
a loose constraint has a **minimum** width and height
both equal to **zero**:

一個 **寬鬆** 約束，換句話來說就是設定了最大寬度/高度，
但是讓允許其子 widget 獲得比它更小的任意大小。
換句話來說，寬鬆約束的最小寬度/高度為 **0**。

<!-- skip -->
```dart
BoxConstraints.loose(Size size)
   : minWidth = 0.0,
     maxWidth = size.width,
     minHeight = 0.0,
     maxHeight = size.height;
```

If you revisit [Example 3](#example-3), it tells us that the
`Center` lets the red `Container` be smaller,
but not bigger than the screen. The `Center` does that,
of course, by passing loose constraints to the `Container`.
Ultimately, the `Center`'s very purpose is to transform
the tight constraints it got from its parent
(the screen) to loose constraints for its child
(the `Container`).

如果你存取 [範例 3](#example-3)，
它將會告訴我們 `Center` 讓紅色的 `Container` 變得更小，
但是不能超出螢幕。`Center` 能夠做到這一點的原因就在於
給 `Container` 的是一個寬鬆約束。
總的來說，`Center` 起的作用就是從其父級（螢幕）那裡獲得的嚴格約束，
為其子級（`Container`）轉換為寬鬆約束。

## Learning the layout rules for specific widgets

## 瞭解如何為特定 widget 制定佈局規則

Knowing the general layout rule is necessary, but it’s not enough.

掌握通用佈局是非常重要的，但這還不夠。

Each widget has a lot of freedom when applying the general rule,
so there is no way of knowing what it will do by just reading
the widget’s name.

應用一般規則時，每個 widget 都具有很大的自由度，
所以沒有辦法只看 widget 的名稱就知道可能它長什麼樣。

If you try to guess, you’ll probably guess wrong.
You can’t know exactly how a widget behaves unless
you’ve read its documentation, or studied its source-code.

如果你嘗試推測，可能就會猜錯。
除非你已閱讀 widget 的文件或研究了其原始碼，
否則你無法確切知道 widget 的行為。

The layout source-code is usually complex,
so it’s probably better to just read the documentation.
However, if you decide to study the layout source-code,
you can easily find it by using the navigating capabilities
of your IDE.

佈局原始碼通常很複雜，因此閱讀文件是更好的選擇。
但是當你在研究佈局原始碼時，可以使用 IDE 的導航功能輕鬆找到它。

Here is an example:

下面是一個例子：

* Find a `Column` in your code and navigate to its
  source code. To do this, use `command+B` (macOS)
  or `control+B` (Windows/Linux) in Android Studio or IntelliJ.
  You’ll be taken to the `basic.dart` file.
  Since `Column` extends `Flex`, navigate to the `Flex`
  source code (also in `basic.dart`).

  在你的程式碼中找到一個 `Column` 並跟進到它的原始碼。
  為此，請在 (Android Studio/IntelliJ) 中使用
  `command+B`（macOS）或 `control+B`（Windows/Linux）。
  你將跳到 `basic.dart` 檔案中。由於 `Column` 擴充了 `Flex`，
  請導航至 `Flex` 原始碼（也位於 `basic.dart` 中）。

* Scroll down until you find a method called
  `createRenderObject()`. As you can see,
  this method returns a `RenderFlex`.
  This is the render-object for the `Column`.
  Now navigate to the source-code of `RenderFlex`,
  which takes you to the `flex.dart` file.

  向下滾動直到找到一個名為 `createRenderObject()` 的方法。
  如你所見，此方法返回一個 `RenderFlex`。
  它是 `Column` 的渲染物件，
  現在導航到 `flex.dart` 檔案中的 `RenderFlex` 的原始碼。

* Scroll down until you find a method called
  `performLayout()`. This is the method that does
  the layout for the `Column`.

  向下滾動，直到找到 `performLayout()` 方法，
  由該方法執行列布局。

<img src='/assets/images/docs/ui/layout/layout-final.png' class="mw-100" alt="A goodbye layout">


---

Article by Marcelo Glasberg

本文作者：Marcelo Glasberg

Marcelo originally published this content as
[Flutter: The Advanced Layout Rule Even Beginners Must Know][]
on Medium. We loved it and asked that he allow us to publish
in on docs.flutter.dev, to which he graciously agreed. Thanks, Marcelo!
You can find Marcelo on [GitHub][] and [pub.dev][].

Marcelo Glasberg 最初在 Medium 發表 [Flutter: The Advanced Layout Rule Even Beginners Must Know][] 本文。
我們十分喜歡這篇文章，在徵得他的允許後釋出在 flutter.dev。再次感謝你，Marcelo!
你可以找到 [GitHub][] 以及 [pub.dev][] 找到 Marcelo。

Also, thanks to [Simon Lightfoot][] for creating the
header image at the top of the article.

同時，還要感謝 [Simon Lightfoot][] 創造了本文的標題圖片。

[`Container` documentation]: {{site.api}}/flutter/widgets/Container-class.html
[DartPad instance]: {{site.dartpad}}/60174a95879612e500203084a0588f94
[Flutter: The Advanced Layout Rule Even Beginners Must Know]: {{site.medium}}/flutter-community/flutter-the-advanced-layout-rule-even-beginners-must-know-edc9516d1a2
[GitHub]: {{site.github}}/marcglasberg
[pub.dev]: {{site.pub}}/publishers/glasberg.dev/packages
[Simon Lightfoot]: {{site.github}}/slightfoot
[this GitHub repo]: {{site.github}}/marcglasberg/flutter_layout_article
