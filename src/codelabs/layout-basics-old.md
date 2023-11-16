---
title: "Codelab: Basic Flutter layout"
title: "Codelab: Flutter 佈局基礎課程"
description: "A codelab that uses DartPad2 to teach Flutter layout concepts."
description: "使用 DartPad2 工具教你如何建構 Flutter 佈局"
toc: false
---

請注意，本文件已在官方文件中被移除，新的頁面正在翻譯中，本頁面作為臨時替代頁面，
將在新頁面翻譯結束之後刪除，請勿在站外參考或分享以避免不好的使用者體驗，謝謝！

{{site.alert.note}}

  The embedded editors use an experimental version of DartPad.
  If you find a DartPad bug or have suggestions for DartPad,
  please [create a DartPad
  issue](https://github.com/dart-lang/dart-pad/issues/new)
  by clicking the bug icon at the top right of this page.
  
  內建的編輯器使用的是實驗版的 DartPad。如果你發現了 DartPad 的 bug 或有任何建議，請在 [create a DartPad
  issue](https://github.com/dart-lang/dart-pad/issues/new) 頁面的右上角點選 bug 圖示。
{{site.alert.end}}

{{site.alert.note}}

  This codelab is currently being developed and tested
  with Chrome. There might be (in the short term) features that
  work in some browsers and not others. If you encounter any, please
  [create a DartPad issue](https://goo.gle/flutter_web_issue),
  labeling the issue with `platform-web`.
  
  目前 codelab 是基於 Chrome 開發測試的。可能會有一些功能（短期內）在某些瀏覽器上可用，而在另一些瀏覽器上不可用。
  如果您遇到任何問題，請 [create a DartPad issue](https://goo.gle/flutter_web_issue) ，併為這個 issue 加上
   `platform-web` 標籤。
{{site.alert.end}}

`Row` and `Column` are two very important widgets in the Flutter universe.
Want to put a `Text` widget with a label next to another `Text`
widget with the corresponding value?  Use a `Row`.
Want to present multiple pairs of labels and values?
That's a `Column` of `Row`s. Forms with several fields,
icons next to menu choices, buttons next to
search bars, these are all places where `Row`s and `Column`s are used,

`Row` and `Column` 是 Flutter 世界非常重要的兩個 widgets。想要把一個帶 label 的 `Text` widget 放到
另一個具有相應值的 `Text` widget邊上？使用一個 `Row`。想要現實多對 labels 和值？使用一個包含多個 `Row` 的 `Column`。
包含多個欄位的表單，旁邊有圖示的選單選項，旁邊有搜尋欄的按鈕，這些都是要用到 `Row`s 和 `Column` 的地方。

This codelab walks you through how `Row`s and `Column`s work.
Because they're so similar, once you're done learning about 
`Row`s, the codelab mostly shows you how all the same concepts apply
to `Column`s. There are inline editors
along the way so you can play around and test your knowledge.

codelab 將帶您瞭解 `Row`s and `Column` 如何工作。因為它們非常相似，一旦您學會了如何使用 `Row`，codelab 將會向您
展示如何把相同的概念應用於 `Column`。使用內建的編輯器，您將會邊玩邊測試您學到的知識。

### Start with a Row and some children

### 從一個 Row 和一些 children開始

The whole point of a `Row` or `Column` is to contain
other widgets, known as children. In a `Row`, children
are arranged horizontally from first to last in accordance
with text direction. If your device is set to
English or another left-to-right language, it starts from the left.
If you're using Arabic or another right-to-left language, it starts
on the right and moves left.

`Row` or `Column` 的主要功能就是包含其他的 widgets，這些 widgets 被稱為 children。
在一個 `Row` 裡，所有的 children 都會根據 text 方向從頭到尾水平排列。如果您的裝置被設定為英文或其他從左到右的語言，
就會從左開始，如果您使用阿拉伯語或者其他從右到左顯示的語言，就會從右到左排列。

#### Code example

#### 程式碼例子

Below is a widget called `MyWidget` that builds a single `Row`.
Try adding three `BlueBox` widgets to its list of children.

下面是一個叫作 `MyWidget` 的 widget，在其內部建立了一個 `Row`，然後請試著
將三個 `BlueBox` widgets 加到 `Row` 的 children中。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=76e993732820ef908ea8424744b9996d" width="100%" height="400px"></iframe>

### Main axis size

### 主軸空間

The main axis of a `Row` is the horizontal one (for
`Column`s, it's the vertical axis). Each `Row` has a
property called `mainAxisSize` that determines how much space
it should take along that axis. By default,
`mainAxisSize` is set to `MainAxisSize.max`, which
causes a `Row` to take up all the available horizontal
space. You can use `MainAxisSize.min` to direct a `Row`
widget to take up as little space as possible.

`Row` 的主軸是指水平方向的軸（`Column` 的主軸是指豎直方向的）。每一個 `Row` 都有一個
叫 `mainAxisSize` 的屬性，它決定了此 `Row` 沿著水平方向佔用空間的大小。預設情況下，`mainAxisSize` 的值為 `MainAxisSize.max`，
這意味著 `Row` 將會佔用所有可用的水平方向空間。你可以使用 `MainAxisSize.min` 實現讓一個 `Row` 佔用儘可能少的空間。

#### Code example

#### 程式碼例子

Here's the example you just finished. Try setting the `Row`'s
`mainAxisSize` property to `MainAxisSize.min` and see what happens.

這裡的例子是您剛剛完成的。試著將 `Row` 的 `mainAxisSize` 的值設為 `MainAxisSize.min`，看看會發生什麼。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=9ac4ade5961150a27d3e547b667c8037" width="100%" height="400px"></iframe>

### Main axis alignment

### 主軸對齊

If you've set the `mainAxisSize` of a `Row` to the minimum,
there won't be any extra room beyond what the children use.
If you've set it to `max`, though, the `Row` might have some
additional space lying around.  You can use the `mainAxisAlignment`
property to control how the `Row` aligns its children within that space.

如果您將一個 `Row` 的 `mainAxisSize` 設為最小值，在 children 之外就不會有更多的空間。
如果您將其設為最大值，`Row` 就會有多出來的空間。您可以使用 `mainAxisAlignment` 屬性來控制
`Row` 中的 children 對齊的方式。

There are six different values available in the `MainAxisAlignment` enum:

`MainAxisAlignment` 有六種不同的列舉值：

* `MainAxisAlignment.start`<br>
   Place all children as close to the start of the `Row` as possible
   (for left-to-right rows, this is the left side).
   
   將所有的 children 儘可能向 `Row` 的 start 方向排列（如果是從左到右，那就是靠左排列）。

* `MainAxisAlignment.end`<br>
  Place all children as close to the end of the `Row` as possible.
  
  將所有的 children 儘可能向 `Row` 的 end 方向排列。

* `MainAxisAlignment.center`<br>
  Group the children together in the center of the `Row`.
  
  將 children 聚在 `Row` 主軸的中間位置。

* `MainAxisAlignment.spaceBetween`<br>
  Any extra space is divided evenly and used to make gaps between the children.
  
  將主軸空白位置進行均分，用來在 children 之間製造間隔，首尾 children 距邊緣沒有間隙。

* `MainAxisAlignment.spaceEvenly`<br>
  Just like `spaceBetween`, except the spots before the first child
  and after the last one also count as gaps.
  
  很像 `spaceBetween`，除了讓首尾 children 距邊緣也有相同的間隙。

* `MainAxisAlignment.spaceAround`<br>
  Just like `spaceEvenly`, only the first and last gaps get 50% of the
  amount used between children.
  
  很像 `spaceEvenly`，只是首尾 children 距邊緣間距為中間 children 間距的一半。

#### Code example

#### 程式碼例子

The row below has its `mainAxisAlignment` set to start. Try changing it to the
other values and re-running the code to see how things move around.

下面的 row 的 `mainAxisAlignment` 被設為了 start。試著將其改為其他的值，然後重新執行看看會怎麼樣。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=0c97de625a9aa5c3194f9eecbd73ec1a" width="100%" height="400px"></iframe>

### Cross axis alignment

### 交叉軸對齊

The cross axis for `Row` widgets is the vertical axis,
and you can use the `crossAxisAlignment` property to
control how children are positioned vertically.
The default value is `CrossAxisAlignment.center`,
but there are five options in total:

`Row` widgets 的交叉軸是豎直方向的軸， 你可以用 `crossAxisAlignment` 屬性來控制
children 如何在垂直方向排列。預設值是 `CrossAxisAlignment.center`，一共有五種值：

* `CrossAxisAlignment.start`<br>
  Children are aligned at the start of the `Row`'s vertical space
  (by default, the top is considered to be the start,
  though you can change that via the `verticalDirection` property).

  將所有的 children 向 `Row` 豎直方向的 start 方向排列
  （如果是從上到下，你可以修改 `verticalDirection` 來改變）。

* `CrossAxisAlignment.end`<br>
  Children are aligned at the end of the `Row`'s
  vertical space (by default, that means the bottom).
  
  將所有的 children 向 `Row` 豎直方向的 end 方向排列（預設是底部）。

* `CrossAxisAlignment.center`<br>
  Children are centered with respect to the vertical axis.
  
  將 children 聚在 `Row` 豎直方向軸的中間位置。

* `CrossAxisAlignment.stretch`<br>
  Children are forced to have the same height as the
  `Row` itself, filling all the vertical space.
  
  所有的 Children 的高度會被拉伸到和 `Row` 一樣，填滿豎直方向軸的空間。

* `CrossAxisAlignment.baseline`<br>
  Children are aligned by their baselines (more on this one below).
  
  所有的 Children 的 baselines 在豎直方向對齊。

#### Code example

#### 程式碼例子

This `Row` has two small children and one big one. Its
`crossAxisAlignment` property is set to center, the default.
Try changing it to the other values and re-running the code to
see how things move around.

`Row` 有兩個小的 children 和一個大的。`crossAxisAlignment` 屬性預設為 center。
可以試著將其變為其他值然後重新執行，看看會怎樣。

A word of warning: `CrossAxisAlignment.baseline` requires
that another property be set as well, so you
will see an error if you try that one.
Don't worry, though&mdash;it's covered in the next section.

會有一個警告： `CrossAxisAlignment.baseline` requires
that another property be set as well, so you
will see an error if you try that one. 不用擔心，在下一節將會對此進行討論。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=610aa31bbd09c90b5cede790bb6c3854" width="100%" height="400px"></iframe>

### Baseline alignment
### 基線對齊

Sometimes it's handy to align widgets containing text not by their
overall bounds, but by the baselines used by their characters.
That's what `CrossAxisAlignment.baseline` is for. You
can use it in combination with a `Row`'s `textBaseline`
property (which indicates which baseline to use) to align a
`Row`'s children along their baselines.

Note that if you set a `Row`'s `crossAxisAlignment` property
to baseline without setting `textBaseline` at the same
time, your widgets will fail to build.

有時候根據包含文字的 widgets 的基線對齊是比較方便的，而不是根據它們的整體邊框對齊。
那就是 CrossAxisAlignment.baseline 的用途。你可以使用聯合使用 `Row` 的 `textBaseline`屬性（決定按照哪種基線來對齊），
來決定 `Row` 的所有 children 根據基線對齊。

#### Code example

#### 程式碼例子

This row contains three `Text` widgets with different font
sizes. Try changing the `crossAxisAlignment`
property to `baseline`, and experiment with different
values for `textBaseline` as well (there's an enum called
`TextBaseline` that contains the valid baseline values).

row 裡包含三個擁有不同字型大小的 `Text` widgets。試著將 `crossAxisAlignment` 屬性設為 `baseline`，
然後試驗 `textBaseline` 的不同值（`TextBaseline` 列舉值裡包含可用的 baseline 值）。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=8c4a0571b161755c8d9235df947d268e" width="100%" height="400px"></iframe>

### Flexible children

### 可伸縮 children

So far, all the widgets used as children in examples have
had a fixed size. It's possible, though, for a
`Row` to have children that flex,
and adapt to the available space. In order to
understand how this works,
it's best to take a look at how `Row`s size themselves and
their children:

到目前為止，例子中所有用作 children 的 widgets 都有一個固定的大小。
不過 `Row` 可以讓它的 children 可伸縮，來適應可用的空間。
為了更好的理解這是怎麼回事兒， 最好看看 `Row` 的大小和它的 children。

1. First, the `Row` asks all of its children with fixed
   sizes how big they'd like to be.
   
   首先，`Row` 首先會要求它所有的 children 想要多大的尺寸。
   
1. Next, it calculates the remaining space in its main
   axis (horizontal).
   
   然後，它會計算主軸（水平）的剩餘空間。
   
1. Then it divides up that remaining space among its
   flexible children according to their flex factors.
   The flexible children can use some or all of the space
   they're offered.
   
   然後它把剩下的空間根據 children 的 flex 值分給它的可伸縮的 children，
   這些可伸縮的 children 可以使用他們提供的部分或者全部的空間。

1. At that point, the `Row` knows how big all of its
   children are, and can align them using the same axis
   size and alignment properties you've seen so far.
   
   在那時， `Row` 知道所有的 children 的尺寸有多大，然後可以根據你之前學到的 axis
   size 和 alignment 屬性來排列它們。

Most widgets are considered to be of a fixed size.
You can change that by wrapping them in a `Flexible`
widget. `Flexibles` have two important properties:
a `flex` factor that determines how much of the remaining
space they get in comparison to other `Flexibles`,
and a `fit` property that determines whether their child
is forced to take up all the extra space it's offered.

大多數 widgets 是固定大小的。你可以將他們包裹在一個 `Flexible` widget 中來將它們變為可伸縮的。
`Flexibles` 有兩個重要屬性： `flex` 值決定與其他 children 相比可佔用剩餘空間的多少， `fit` 屬性決定
其 child 是否佔用所有額外的空間。


#### Code example

#### 程式碼例子

Try wrapping the middle box in this row with a `Flexible`
widget that has a `flex` factor of 1 and its `fit`
property set to `FlexFit.loose`. Afterward,
try changing the fit to tight and see what happens.

試著將 row 中間的 box 包裹在一個 `flex` factor 為 1 並且 `fit` 為 `FlexFit.loose` 的 Flexible` widget中。
然後試著將 `fit` 改為 `FlexFit.tight`，看看會發生什麼。

This combination (a `flex` factor of 1 and a tight `fit`)
is so popular, there's a whole widget just to make
using them easier: `Expanded`.

`flex` factor 為 1 和 `fit` 為 `FlexFit.tight` 的組合是非常常見的, 更簡單的方式是直接使用 `Expanded` widget.

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=c7ba00c50151ab2e5c0c2194686fef93" width="100%" height="400px"></iframe>

### Flex factors

If more than one child of a `Row` or `Column` has a
flexible size, the available space is allotted to them according to their
`flex` factors. Each child gets space in proportion to their flex
factor divided by the total of all the flex factors of all children:

如果 `Row` 或 `Column` 中多個 children 都是可伸縮的，那麼如何分配可用空間取決於它們的 `flex` 值。
每個 child 獲得的空間將取決於 他們的 flex 值佔所有 children 的 flex 值之和的比例。

<!-- skip -->
```dart
remainingSpace * (flex / totalOfAllFlexValues)
```

For example, if there are two children with flex factors of 1,
each gets half of the available space. If there
are two children with flex factors of 1 and another child
with a flex factor of 2, the first two
children each get a quarter of the available space,
and the other child gets half.

例如， 如果有兩個 flex 值為 1 的 children， 每個將獲得一半的可用空間。
如果有兩個  flex 值為 1 的 children， 還有一個 flex 值為 2 的 child，
那麼前兩個 children 將各獲得四分之一的可用空間，另一個 child 將獲得一半的可用空間。

#### Code example

#### 程式碼例子

In this example, all three of the `Row`'s children are `Flexible`.
Try changing their `flex` values and
re-running the code to see how the widgets' sizes adjust.

在這個例子中， `Row` 的所有三個 children 都是可伸縮的，試著改變它們的 `flex` 值然後重新執行看看它們的尺寸如何改變。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=4ab5409b566272c8f2cd28feddb0a995" width="100%" height="400px"></iframe>

### What happens if you run out of space?

### 如果沒有空間了怎麼辦？

As you just saw, when a `Row` asks one of its `Flexible`
children how big it wants to be, it gives the child
a max width based on its `flex` factor. However,
fixed-size children get no such restriction. This is so
they can determine their own intrinsic size.

正如你所看到的，當一個 `Row` 問 它其中一個可伸縮的 child 想要多大空間時， 它會根據這個 child 的 `flex` 值分配給它一個最大值。
但是固定大小的 children 沒有這個限制，它們可以自己決定大小。

One side effect is that there's nothing stopping a fixed-size
child from declaring itself to be bigger than the `Row` can support.
When that happens, a flex overflow results. You can
fix it by changing the child widget so that it chooses a smaller size,
or by using a scrolling widget.

一個副作用就是， 無法阻止一個固定大小的 child 宣告超出 `Row` 所能支援的大小。
當這種情況發生時， 就會發生溢位。你可以透過修改這個 child 的大小或者使用一個可滾動的 widget 來解決這個問題。

#### Code example

#### 程式碼例子

The `Row` below contains a single widget that's way too wide to fit. Run the
code as-is to see what happens, then try modifying the width of the
`Container` to make it fit.

下面的 `Row` 包含一個特別寬的 widget。執行程式碼看會發生什麼，然後試著修改`Container`的寬度使其適應。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=5a59d93119dc5b6eb1725235fde137cf" width="100%" height="400px"></iframe>

### Try using SizedBox to make space

### 試著使用 SizedBox 來留出空間

If you need a specific amount of space between two children of a
`Row`, an easy way to do it is by sticking a `SizedBox` of the
appropriate width in between.

如果你需要在一個 `Row` 中的兩個 children 之間指定一個特定的間隔，一個簡單的方法是在中間放一個 寬度合適的 `SizedBox`。

#### Code example

#### 程式碼例子

Trying making some space between these two list items by placing a
`SizedBox` with a `width` of 100 between them.

試著用一個寬度 100 的 `SizedBox` 在兩個 items 中間製造一些間隔。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=326b8c5774079b7a80922e11a3730f99" width="100%" height="400px"></iframe>

### Spacers expand to make space

### Spacers 留出可變空間

`Spacers` are another convenient way to make space between
items in a `Row`.  They're flexible, and expand to fill any
leftover space.

使用`Spacers` 是另一個在 `Row` 的 children 之間留出空間的方法。它們是可伸縮的，可以填滿任何剩下的空間。

#### Code example

#### 程式碼例子

Try adding a `Spacer` in between the first and second children of the
`Row` below.

試著在第一個和第二個 children 之間加一個 `Spacer`。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=dd68c1eb491e7a22a2ceb4127d78e504" width="100%" height="400px"></iframe>

### Wait, wasn't I going to learn about Columns, too?

### 等等, 我不是還要學習 Columns 嗎?

Surprise, you already have! `Row`s and `Column`s do the
same job, just in different dimensions. The main
axis of a `Row` is horizontal, and the main axis of a
`Column` is vertical, but they both size and position their
children in the same way. They even share a base class,
`Flex`, so everything you've learned about `Row`s
applies to `Column`s as well!

給你個驚喜，你已經學習了。`Row` 的所有用法和 `Column` 是一樣的，只是維度不同。
`Row` 的主軸是水平的， 而 `Column` 的主軸是豎直的， 但是它們設定其 children 的大小和位置的方式是一樣的。
它們還共用一個基本類 `Flex`。所以你已經學習的有關 `Row` 的用法，同樣適用於 `Column`。

#### Code example

#### 程式碼例子

Here's a `Column` with some children of various sizes and its most important
properties set. Try fiddling around with them and you'll see that the
`Column` works like a vertical `Row`.

這裡有一個包含不同尺寸和一些重要屬性已經設定好的 children 的 `Column`。試著擺弄以下，你會發現 `Column` 就像一個豎過來的的 `Row`。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=6cafe7beab954e72fed2fd2393a29f6c" width="100%" height="400px"></iframe>

### Putting it all together

### 將它們放在一起

Now that you're versed in `Row`s, `Column`s, and the
important properties of both, you're ready to practice
putting them together to build interfaces. The next few
examples guide you through the construction
of a business card display.

現在你已經熟悉了 `Row` 和 `Column` 的重要屬性， 你已經可以來聯絡將它門組合在一起來建構使用者介面。
下面的例子將帶你完成一個名片顯示的建構。

#### Code example

#### 程式碼例子

Every business card needs a name and a title, so start with that.

每一張名片都需要一個名字和頭銜，讓我們從這裡開始。

* Add a `Column` widget
  
  新增一個 `Column` widget
  
* Add two text widgets to the `Column`'s list of children:

  新增兩個 text widgets 到 `Column` 的 children 列表中：

  * The first should be a name (a short one is easier to
    fit into the small window) and use the `headline` style:
    
    第一個是名字（簡短一點更適合於一個小視窗），使用 `headline` 樣式：

<!-- skip -->
```dart
style: Theme.of(context).textTheme.headline
```

  * The second text widget should say `Experienced App Developer`
    and use the default style (leave the `style` property out entirely).
    
    第二個 text widget 應該是 `Experienced App Developer`，使用預設樣式（不用設定 `style` 屬性）。

* Set the `Column`'s `crossAxisAlignment` to start, so
  that the text widgets are start-aligned rather than centered.
  
  設定 `Column` 的 `crossAxisAlignment` 為 start，使得 text widgets 會開始對齊，而不是居中。

* Set the `Column`'s `mainAxisSize` to `MainAxisSize.min`,
  so the card won't expand to the full height of the window.
  
  將 `Column` 的 `mainAxisSize` 設為 `MainAxisSize.min`，這樣 card 才不會擴充到整個 window 那麼高。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=5e7e9352bca878f446d4347f324e2f63&split=60" width="100%" height="800px"></iframe>

Business cards often have an icon or logo in the top-left corner,
so the next step is to add one to yours. Start by wrapping the
`Column` you just created with a `Row` widget:

名片的左上角通常會有一個圖示或者標誌，所以下一步是加一個到你的名片上。將你剛建立的 `Column` 包裹在一個 `Row` widget 中。

<!-- skip -->
```dart
Row(
  children: [
    Column( … ), // <- This should be the Column you made in the previous step
  ],
);
```

Now you can add the `Icon`:

現在你可以新增一個圖示：

* Above your `Column` in the `Row`'s list of children,
  add a `Padding` widget.
  
  在你的 `Row` 的 `Column` 的前面，加一個 `Padding` widget。
  
  * Set its `padding` to `const EdgeInsets.all(8)`.
    
    設定 `padding` 為 `const EdgeInsets.all(8)`。
  
  * For the child of the `Padding` widget, use an `Icon`.
  
    將一個 `Icon` widget 作為 `Padding` widget 的 child。
  
    * You can use any icon resource you want, though `Icons.account_circle`
      works nicely.
      
      你可以使用任何 icon resource， `Icons.account_circle`看起來就不錯。
      
    * Set the `Icon`'s `size` to 50.
    
      把 `Icon` 的大小設定為 50。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=684e599476eef2ec4b4508e6b2186c03&split=60" width="100%" height="800px"></iframe>

Your first `Row` is now complete! There are two more to go, though,
and you need a `Column` to put them in.
Wrap your `Row` with a `Column` widget so that it looks like this:

你的第一個 `Row` 現在完成了。還有兩件事要做，你需要一個 `Column` 把它們放進去。
把你的 `Row` 包裹進一個 `Column` widget 就像這樣：

<!-- skip -->
```dart
 Column(
   children: [
     Row( … ), // <- This should be the Row with your Icon and Text widgets.
   ],
 );
```

Then, finish up your new `Column` with these steps:

然後按照以下步驟完成你的新 `Column`：

* Set the `Column`'s `mainAxisSize` to min

  設定 `Column` 的 mainAxisSize 為最小
  
  * Otherwise it'll expand to fill the screen!
  
    否則它會充滿整個螢幕！

* Set the `Column`'s `crossAxisAlignment` to stretch
  
  設定`Column` 的 `crossAxisAlignment` 為 stretch。

  * This makes all of its children full-width
  
    這使得所有 children 都會拉伸到最大寬度

* Add more widgets below your `Row` in the `Column`'s
  list of children:
  
  在 `Column` 中你的 `Row` 下面新增更多的 widgets：
  
  * A `SizedBox` with a height of 8
  
    一個高度為 8 的 `SizedBox`
  
  * An empty `Row` (no children or other properties)
  
    一個空 `Row` （沒有 children 或 其他屬性）
  
  * A `SizedBox` with a height of 16
  
    一個高度為 16 的 `SizedBox`
    
  * Another empty `Row`
  
    另一個空 `Row`

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=19ead6db4f42ce112fc0a7d2e0922466&split=60" width="100%" height="800px"></iframe>

There are just a few steps to go now. Next up is the second row.
Add the following to its list of children:

現在就差幾步了。接下來是第二個 `Row`。
新增以下的 widgets 作為它的 children：

* A `Text` widget with a street address like '123 Main Street'
  
  一個地址為 '123 Main Street' 的 `Text` widget

* A `Text` widget with a phone number like '800-123-1234'

  一個電話為 '800-123-1234' 的 `Text` widget

If you run the code at this point, you'll see that the two `Text`
widgets are placed right up against each other rather than at
opposite ends of the `Row`, which isn't right.
You can fix this by setting the `Row`'s `mainAxisAlignment`
property to `spaceBetween`, which puts any extra space between
the two `Text` widgets.

如果你現在執行程式碼，你會看到這兩個 `Text`widgets 是挨著的，而不是在 `Row` 的兩端對齊，這是不對的。
你可以將 `Row` 的 `mainAxisAlignment` 設為 `spaceBetween`，使得這兩個 `Text` widge 中間有些間隔。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=e6e07bbe96255b762163cf3e40906944&split=60" width="100%" height="800px"></iframe>

The last step is to get those icons in place at the bottom of the card:

最後一步是在名片的底部放一些圖示。

* Add four `Icon` widgets to the last `Row`'s list of
  children. You can use whichever icon resources you
  like, but these would be a good way to show that your
  imaginary developer focuses on accessibility,
  fast development, and multi-platform apps:
  
  新增四個 `Icon` widgets 到最後一個 `Row` 中。你可以使用任何你喜歡的圖示資源，
  但是以下圖示是一個很好的選擇，用來展示你想象中的關注於 accessibility,
  fast development, and multi-platform apps 的開發人員：

  * `Icons.accessibility`
  * `Icons.timer`
  * `Icons.phone_android`
  * `Icons.phone_iphone`

* Set the `Row`'s `mainAxisAlignment` property to
  `MainAxisAlignment.spaceAround`
  
  設定 `Row` 的 `mainAxisAlignment` 屬性為 `MainAxisAlignment.spaceAround`。

<iframe src="{{site.dartpad}}/experimental/embed-new-flutter.html?id=2234a5ccada200eb1e018b12fa95d57d&split=60" width="100%" height="800px"></iframe>