---
title: Dealing with box constraints
title: 處理邊界約束 (Box constraints) 的問題
description: How box constraints might go wrong.
description: 為何框約束可能會出錯。
short-title: Box constraints
short-title: 邊界約束 (Box constraints)
tags: 使用者介面,Flutter UI,佈局
keywords: 邊界約束問題,渲染
---

{{site.alert.note}}

  You might be directed to this page if the
  framework detects a problem involving box constraints.

  如果你被引導至本頁面，
  是因為 Flutter 框架檢測到你可能遇到涉及邊界約束的問題。 

{{site.alert.end}}

{{site.alert.note}}
  If you are confused by how constraints, sizing,
  and positioning work in Flutter, see
  [Understanding constraints][].
{{site.alert.end}}

In Flutter, widgets are rendered by their underlying
[`RenderBox`][] objects. Render boxes are given
constraints by their parent, and size themselves within those
constraints. Constraints consist of minimum and maximum widths
and heights; sizes consist of a specific width and height.

Flutter 中的 widget 由在其底層的 [`RenderBox`][] 物件渲染而成。
渲染框由其父級 widget 給出約束，
並根據這些約束調整自身尺寸大小。
約束是由最小寬度、最大寬度、最小高度、最大高度四個方面構成；
尺寸大小則由特定的寬度和高度兩個方面構成 。

Generally, there are three kinds of boxes,
in terms of how they handle their constraints:

一般來說，從如何處理約束的角度來看，有以下三種類型的渲染框：

* Those that try to be as big as possible.
  For example, the boxes used by [`Center`][] and
  [`ListView`][].

  儘可能大。比如 [`Center`][] 和 [`ListView`][] 的渲染框。

* Those that try to be the same size as their children.
  For example, the boxes used by [`Transform`][] and
  [`Opacity`][].

  與子 widget 一樣大，比如 [`Transform`][] 和 [`Opacity`][] 的渲染框。

* Those that try to be a particular size.
  For example, the boxes used by [`Image`][] and
  [`Text`][].

  特定大小，比如 [`Image`][] 和 [`Text`][] 的渲染框。

Some widgets, for example [`Container`][],
vary from type to type based on their constructor arguments.
In the case of [`Container`][], it defaults
to trying to be as big as possible, but if you give it a `width`,
for instance, it tries to honor that and be that particular size.

對於一些諸如 [`Container`][] 的 widget，
其尺寸會因構造方法的引數而異，
就 [`Container`][] 來說，它預設是儘可能大的，
而一旦給它一個特定的寬度，
那麼它就會遵照這個特定的寬度來調整自身尺寸。

Others, for example [`Row`][] and  (flex boxes)
vary based on the constraints they are given,
as described below in the "Flex" section.

其它一些像 [`Row`][] and [`Column`][] (flex boxes)這樣的 widget ，
其尺寸會因給定的約束而異，具體細節見後文 "Flex" 部分；

The constraints are sometimes "tight",
meaning that they leave no room for the render box to decide on
a size (for example, if the minimum and maximum width are the same,
it is said to have a tight width). An example of this is the
`App` widget, which is contained by the [`RenderView`][]
class: the box used by the child returned by the
application's [`build`][] function is given a constraint
that forces it to exactly fill the application's content area
(typically, the entire screen).
Many of the boxes in Flutter, especially those that just take a
single child, pass their constraint on to their children.
This means that if you nest a bunch of boxes inside each other
at the root of your application's render tree,
they'll all exactly fit in each other, forced by these tight constraints.

約束有時是"緊密的"，這意味著這些約束嚴格地限定了
渲染框在定奪自身尺寸方面的空間（例如：當約束的最小寬度和最大寬度相同時，
這種情況下，我們稱這個約束有緊密寬度），
這方面的主要例子是 App Widget，
它是 [`RenderView`][] 類裡面的一個 widget: 
由應用程式的 [`build`][] 函式返回的子 widget 渲染框被指定了一個約束，
該約束強制 App Widget 精確填充應用程式的內容區域(通常是整個螢幕)。
Flutter 中的許多渲染框，特別是那些只包含單個 widget 的渲染框，
都會將自身的約束傳遞給他們的子級 widget。
這意味著如果你在應用程式渲染樹的根部嵌套了一些渲染框，
這些框將會在受到約束的影響下相互適應彼此。

Some boxes _loosen_ the constraints,
meaning the maximum is maintained but the
minimum is removed. For example, [`Center`][].

有些渲染框**放鬆**了約束，即：約束中只有最大寬度，
最大高度，但沒有最小寬度，最小高度，
例如 [`Center`][]。


## Unbounded constraints

無邊界約束
---------------------

In certain situations, the constraint that is given to a box is
_unbounded_, or infinite. This means that either the maximum width or
the maximum height is set to [`double.infinity`][].

在某些情況下，傳遞給框的約束是 **無邊界** 的或無限的。這意味著約束的最大寬度或最大高度為 [`double.infinity`][]。

A box that tries to be as big as possible won't function usefully when
given an unbounded constraint and, in debug mode, such a combination
throws an exception that points to this file.

當傳遞無邊界約束給型別為儘可能大的框時會失效，在 debug 模式下，則會丟擲例外，該例外資訊會把你引導到本頁面。

The most common cases where a render box finds itself with unbounded
constraints are within flex boxes
([`Row`][] and [`Column`][]),
and **within scrollable regions**
([`ListView`][] and other [`ScrollView`][] subclasses).

渲染框具有無邊界約束的最常見情況是：
當其被置於 flex boxes ([`Row`][] 和 [`Column`][])內以及
**可滾動區域**([`ListView`][] 和其它 [`ScrollView`][] 的子類別)內時。


In particular, [`ListView`][]
tries to expand to fit the space available
in its cross-direction (for example,
if it's a vertically-scrolling block,
it tries to be as wide as its parent).
If you nest a vertically scrolling [`ListView`][]
inside a horizontally scrolling `ListView`,
the inner one tries to be as wide as possible,
which is infinitely wide,
since the outer one is scrollable in that direction.

特別是 [`ListView`][] 會試圖擴充以適應其交叉方向可用空間
(比如說，如果它是一個垂直滾動塊，
它將試圖擴充到與其父 widget 一樣寬)。
如果讓垂直滾動的 [`ListView`][] 
巢狀(Nesting)在水平滾動的 [`ListView`][]
內，那麼被巢狀(Nesting)在裡面的垂直滾動的 [`ListView`][]
將會試圖儘可能寬，直到無限寬，
因為將其巢狀(Nesting)的是一個水平滾動的[`ListView`][]，
它可以在水平方向上一直滾動。

## Flex

Flex boxes themselves ([`Row`][] and [`Column`][])
behave differently based on whether they are in
bounded constraints or unbounded constraints in
their given direction.

Flex 框本身([`Row`][] 和 [`Column`][])的行為會有所不同，
這取決於其在給定方向上是處於有邊界約束還是無邊界約束。

In bounded constraints,
they try to be as big as possible in that direction.

在有邊界約束條件下，它們在給定方向上會盡可能大。

In unbounded constraints,
they try to fit their children in that direction.
In this case, you cannot set `flex` on the children to
anything other than 0.
In the widget library, this means that you cannot use
[`Expanded`][] when the flex box is inside
another flex box or inside a scrollable. If you do,
you'll get an exception message pointing you at this document.

在無邊界約束條件下，它們試圖讓其子 widget 自適應這個給定的方向。
在這種情況下，不能將子 widget 的`flex`屬性設定為 0（預設值）以外的任何值。
這意味著在 widget 庫中，
當一個 flex 框巢狀(Nesting)在另外一個 flex 框或者巢狀(Nesting)在可滾動區域內時，
不能使用 [`Expanded`][]。
如果這樣做了，就會收到例外，該例外資訊會把你引導到本頁面。

In the _cross_ direction, for example, in the width for
[`Column`][] (vertical flex) or in the height for
[`Row`][] (horizontal flex), they must never be unbounded,
otherwise they would not be able to reasonably align their children.

在 **交叉** 方向上，如 [`Column`][]（垂直的 flex）的寬度和 
[`Row`][]（水平的 flex）的高度，它們必將不能是無界的，
否則它們將無法合理地對齊它們的子 widget。


[`build`]: {{site.api}}/flutter/widgets/State/build.html
[`Center`]: {{site.api}}/flutter/widgets/Center-class.html
[`Column`]: {{site.api}}/flutter/widgets/Column-class.html
[`Container`]: {{site.api}}/flutter/widgets/Container-class.html
[`Expanded`]: {{site.api}}/flutter/widgets/Expanded-class.html
[`Image`]: {{site.api}}/flutter/dart-ui/Image-class.html
[`ListView`]: {{site.api}}/flutter/widgets/ListView-class.html
[`Opacity`]: {{site.api}}/flutter/widgets/Opacity-class.html
[`RenderBox`]: {{site.api}}/flutter/rendering/RenderBox-class.html
[`RenderView`]: {{site.api}}/flutter/rendering/RenderView-class.html
[`Row`]: {{site.api}}/flutter/widgets/Row-class.html
[`ScrollView`]: {{site.api}}/flutter/widgets/ScrollView-class.html
[`Text`]: {{site.api}}/flutter/widgets/Text-class.html
[`Transform`]: {{site.api}}/flutter/widgets/Transform-class.html
[Understanding constraints]: {{site.url}}/development/ui/layout/constraints
[`double.infinity`]: {{site.api}}/flutter/dart-core/double/infinity-constant.html
