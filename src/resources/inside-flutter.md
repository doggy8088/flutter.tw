---
title: Inside Flutter
title: Flutter 工作原理
description: Learn about Flutter's inner workings from one of the founding engineers.
description: 向核心工程師瞭解 Flutter 的內部工作原理。
tags: Flutter參考資料
keywords: 組合性API,Flutter設計思路,Flutter設計架構,Flutter Android,Flutter跨平臺
---

This document describes the inner workings of the Flutter toolkit that make
Flutter’s API possible. Because Flutter widgets are built using aggressive
composition, user interfaces built with Flutter have a large number of
widgets.  To support this workload, Flutter uses sublinear algorithms for
layout and building widgets as well as data structures that make tree
surgery efficient and that have a number of constant-factor optimizations.
With some additional details, this design also makes it easy for developers
to create infinite scrolling lists using callbacks that build exactly those
widgets that are visible to the user.

本文件解釋了使 Flutter API 正常工作的 Flutter 工具套件內部工作原理。
由於 Flutter widget 是以積極組合的形式建構的，
所以使用 Flutter 建構的使用者介面含有大量 widget。
為了支撐這些負載，Flutter 使用了次線性演算法來佈局和建構 widget，
這些資料結構使樹形結構最佳化更加高效，並且具有很多常量因子最佳化。
透過一些額外的機制，該設計也允許開發者利用回呼(Callback)
（用於建構使用者可見的 widget）來輕鬆建立無限滾動列表。

## Aggressive composability

## 積極可組合性

One of the most distinctive aspects of Flutter is its _aggressive
composability_. widgets are built by composing other widgets,
which are themselves built out of progressively more basic widgets.
For example, `Padding` is a widget rather than a property of other widgets.
As a result, user interfaces built with Flutter consist of many,
many widgets.

組合性是 Flutter 最為出眾的一個特性。widget 透過組合其他 widget 的方式進行建構，
並且這些 widget 自身由更基礎的 widget 建構。
比如，`Padding` 是一個 widget 而非其他 widget 的屬性。
因此，使用 Flutter 建立的使用者介面是由多個 widget 組成的。

The widget building recursion bottoms out in `RenderObjectwidgets`,
which are widgets that create nodes in the underlying _render_ tree.
The render tree is a data structure that stores the geometry of the user
interface, which is computed during _layout_ and used during _painting_ and
_hit testing_. Most Flutter developers do not author render objects directly
but instead manipulate the render tree using widgets.

widget 遞迴建構的底層是 RenderObjectwidget，它將在渲染樹的底部建立子節點。
渲染樹是一種儲存使用者介面幾何資訊的資料結構，
該幾何資訊在 **佈局** 期間計算並在 **繪製** 及 **命中測試** 期間使用。
大多數 Flutter 開發者無需直接建立這些物件，而是使用 widget 來操縱渲染樹。

In order to support aggressive composability at the widget layer,
Flutter uses a number of efficient algorithms and optimizations at
both the widget and render tree layers, which are described in the
following subsections.

為了支援 widget 層的積極可組合性，
Flutter 在 widget 和樹渲染層使用了大量的高效演算法和最佳化措施，
這些將在下面小節中進行介紹。

### Sublinear layout

### 次線性佈局

With a large number of widgets and render objects, the key to good
performance is efficient algorithms. Of paramount importance is the
performance of _layout_, which is the algorithm that determines the
geometry (for example, the size and position) of the render objects.
Some other toolkits use layout algorithms that are O(N²) or worse
(for example, fixed-point iteration in some constraint domain).
Flutter aims for linear performance for initial layout, and _sublinear
layout performance_ in the common case of subsequently updating an
existing layout. Typically, the amount of time spent in layout should
scale more slowly than the number of render objects.

使用大量 widget 及渲染物件並保持高效能的關鍵是使用高效的演算法。
其中最重要的是確定渲染物件幾何空間（比如大小和位置）的**佈局**演算法的效能。
其他一些工具套件使用 O(N²) 或更糟糕的佈局演算法（例如，約束域中的不動點迭代）。
Flutter 的目標在於佈局初始化的線性效能，
及一般情況下更新現有佈局的`次線性佈局效能`。
通常情況下，佈局所花費的時間應該比物件渲染要多得多。

Flutter performs one layout per frame, and the layout algorithm works
in a single pass. _Constraints_ are passed down the tree by parent
objects calling the layout method on each of their children.
The children recursively perform their own layout and then return
_geometry_ up the tree by returning from their layout method. Importantly,
once a render object has returned from its layout method, that render
object will not be visited again<sup><a href="#a1">1</a></sup>
until the layout for the next frame. This approach combines what might
otherwise be separate measure and layout passes into a single pass and,
as a result, each render object is visited _at most
twice_<sup><a href="#a2">2</a></sup> during layout: once on the way
down the tree, and once on the way up the tree.

Flutter 對每一幀執行一次佈局操作，且佈局演算法僅在一次傳遞中完成。
**約束**資訊透過父節點呼叫每個子節點的佈局方法向下傳遞。
子節點遞迴執行自身的佈局操作，
並在它們的佈局方法中返回**幾何**資訊以便將其新增到渲染樹中。
需要注意的是，一旦渲染物件從佈局中返回，
該物件將不會被再次存取 <sup><a href="#a1">1</a></sup>，直到下一幀佈局的執行。
該策略將可能存在的單獨測量和佈局傳遞合併為單次傳遞，
因此，每個渲染物件在佈局過程中**最多**被存取**兩次**
<sup><a href="#a2">2</a></sup>：
一次在樹的向下傳遞過程中，一次在樹的向上傳遞過程中。

Flutter has several specializations of this general protocol.
The most common specialization is `RenderBox`, which operates in
two-dimensional, cartesian coordinates. In box layout, the constraints
are a min and max width and a min and max height. During layout,
the child determines its geometry by choosing a size within these bounds.
After the child returns from layout, the parent decides the child's
position in the parent's coordinate system<sup><a href="#a3">3</a></sup>.
Note that the child's layout cannot depend on its position,
as the position is not determined until after the child
returns from the layout. As a result, the parent is free to reposition
the child without needing to recompute its layout.

針對這個通用協議，Flutter 擁有多種實現。最常用的是 `RenderBox`，
它以二維的笛卡爾座標進行運算。在盒子佈局中，約束是最小及最大寬高。
在佈局過程中，子節點透過選擇這些邊界內的大小來確定其幾何資訊。
子節點在佈局中返回後，由父節點確定該子節點在父座標系中的位置 <sup><a href="#a3">3</a></sup>。
注意，子節點的佈局並不取決於它的位置，這是因為它的位置直到它從佈局中返回後才確定。
因此父節點可以在無需重新計算子節點佈局的情況下重新定位子節點的位置資訊。

More generally, during layout, the _only_ information that flows from
parent to child are the constraints and the _only_ information that
flows from child to parent is the geometry. These invariants can reduce
the amount of work required during layout:

更廣泛地講，在佈局期間，從父節點流向子節點的**唯一**資訊是約束資訊，
從子節點流向父節點的**唯一**資訊是幾何資訊。
透過這些不變數可減少佈局期間所需的工作量：

* If the child has not marked its own layout as dirty, the child can
  return immediately from layout, cutting off the walk, as long as the
  parent gives the child the same constraints as the child received
  during the previous layout.

  如果父節點對子節點使用與上一次佈局中相同的約束，
  且子節點沒有將自己的佈局標記為髒，
  那麼該節點可立即從佈局中返回，以切斷佈局的向下傳遞。

* Whenever a parent calls a child's layout method, the parent indicates
  whether it uses the size information returned from the child. If,
  as often happens, the parent does not use the size information,
  then the parent need not recompute its layout if the child selects
  a new size because the parent is guaranteed that the new size will
  conform to the existing constraints.

  當父節點呼叫子節點的佈局方法時，父節點會表明它是否使用從子節點返回的大小資訊。
  如果父節點經常不使用此資訊，即使子節點重新選擇了大小，
  父節點依舊無需重新計算其佈局，
  這是因為父節點需要保證新的大小符合現有約束。

* _Tight_ constraints are those that can be satisfied by exactly one
  valid geometry. For example, if the min and max widths are equal to
  each other and the min and max heights are equal to each other,
  the only size that satisfies those constraints is one with that
  width and height. If the parent provides tight constraints,
  then the parent need not recompute its layout whenever the child
  recomputes its layout, even if the parent uses the child's size
  in its layout, because the child cannot change size without new
  constraints from its parent.

  **嚴格**約束是指恰好由一個有效幾何滿足的約束。比如，如果最小最大寬度彼此相等，
  且最小最大高度彼此相等，那麼滿足這些約束的唯一大小便是具有該寬度及高度的大小。
  如果父節點提供了嚴格約束，即便父節點在佈局中使用了子節點的大小，
  在子節點重新計算佈局時，  父節點的佈局也無需重新計算，
  這是因為子節點在沒有父節點新約束的情況下無法更改其大小。

* A render object can declare that it uses the constraints provided
  by the parent only to determine its geometry. Such a declaration
  informs the framework that the parent of that render object does
  not need to recompute its layout when the child recomputes its layout
  _even if the constraints are not tight_ and _even if the parent's
  layout depends on the child's size_, because the child cannot change
  size without new constraints from its parent.

  渲染物件可以宣告僅使用父節點提供的約束來確定其幾何資訊。
  此類宣告通知框架：
  **即便約束為非嚴格約束，以及父節點的佈局取決於子節點的大小，**
  該渲染物件父節點的佈局在子節點的佈局重新計算時仍無需重新計算，
  這是因為子節點在沒有父節點新約束的情況下無法更改其大小。

As a result of these optimizations, when the render object tree contains
dirty nodes, only those nodes and a limited part of the subtree around
them are visited during layout.

這些最佳化措施的效果是，當渲染物件包含髒節點時，在佈局過程中，
只有這些節點以及它們周圍子樹的有限節點才允許被存取。

### Sublinear widget building

### 次線性 widget 建構

Similar to the layout algorithm, Flutter's widget building algorithm
is sublinear. After being built, the widgets are held by the _element
tree_, which retains the logical structure of the user interface.
The element tree is necessary because the widgets themselves are
_immutable_, which means (among other things), they cannot remember their
parent or child relationships with other widgets. The element tree also
holds the _state_ objects associated with stateful widgets.

Flutter 使用類似於佈局的次線性演算法來建構 widget。widget 建構完成後，
它們將被保留了使用者頁面邏輯結構的 **element 樹** 儲存。
Element 樹是非常有必要的，這是因為 widget 自身是**不可變的**，
這意味著（其他情況除外），它們無法記住父（或子）節點與其他 widget 的關係。
Element 還儲存了與 Stateful widget 相關聯的 **state** 物件。

In response to user input (or other stimuli), an element can become dirty,
for example if the developer calls `setState()` on the associated state
object. The framework keeps a list of dirty elements and jumps directly
to them during the _build_ phase, skipping over clean elements. During
the build phase, information flows _unidirectionally_ down the element
tree, which means each element is visited at most once during the build
phase.  Once cleaned, an element cannot become dirty again because,
by induction, all its ancestor elements are also
clean<sup><a href="#a4">4</a></sup>.

由於使用者輸入（或來自其他地方的響應），
比如開發者在關聯的 state 物件上呼叫了 `setState()` 方法，element 可能會變髒。
框架維護了一個髒 element 列表，
使得 **建構** 過程可跳過乾淨的 element，直接跳轉到髒的 element。
建構過程中，資訊在 element 樹中向下 **單向** 傳遞，
這意味著該階段中每個 element 最多會被存取一次。
一個 element 一旦被清洗，它將不會再次變髒，這是因為透過歸納，
它所有的祖先 element 也都是乾淨的 <sup><a href="#a4">4</a></sup>。

Because widgets are _immutable_, if an element has not marked itself as
dirty, the element can return immediately from build, cutting off the walk,
if the parent rebuilds the element with an identical widget. Moreover,
the element need only compare the object identity of the two widget
references in order to establish that the new widget is the same as
the old widget. Developers exploit this optimization to implement the
_reprojection_ pattern, in which a widget includes a prebuilt child
widget stored as a member variable in its build.

由於 widget 是**不可變的**，因此父節點使用相同的 widget 來重新建構
element，如果 element 沒有將自己標記為髒，那麼該 element
可立即從建構中返回，以切斷建構的向下傳遞。
另外，element 只需比較兩個 widget 所參考的物件標識來
確定新 widget 與舊 widget 是否相同。
開發者可利用該最佳化實現**投影**模式，即 widget 包含了被儲存為
成員變數、在建構過程中預先建構的子 widget

During build, Flutter also avoids walking the parent chain using
`Inheritedwidgets`. If widgets commonly walked their parent chain,
for example to determine the current theme color, the build phase
would become O(N²) in the depth of the tree, which can be quite
large due to aggressive composition. To avoid these parent walks,
the framework pushes information down the element tree by maintaining
a hash table of `Inheritedwidget`s at each element. Typically, many
elements will reference the same hash table, which changes only at
elements that introduce a new `Inheritedwidget`.

建構過程中，Flutter 同時使用 `Inheritedwidgets` 來避免父鏈的遍歷。
如果 widget 經常遍歷它們的父鏈，比如確定當前的主題顏色，
那麼建構階段樹的深底將變為 O(N²)，由於 Flutter 的積極可組合性，
其數量可能非常巨大。為了避免這些父鏈的遍歷，
框架透過在每個 element 上維護一個 `Inheritedwidget` 雜湊表來向下傳遞
element 樹中的資訊。通常情況下，多個 element 參考相同的雜湊表，
並且該表僅在 element 引入新的 `Inheritedwidget` 時改變。

### Linear reconciliation

### 線性協調

Contrary to popular belief, Flutter does not employ a tree-diffing
algorithm. Instead, the framework decides whether to reuse elements by
examining the child list for each element independently using an O(N)
algorithm. The child list reconciliation algorithm optimizes for the
following cases:

不同於傳統做法，Flutter 沒有使用樹差異比較演算法。
相反，框架透過使用 O(N) 演算法獨立地檢查每個 element 的子節點
來決定是否重用該 element。子列表協調演算法針對以下情況進行了最佳化：

* The old child list is empty.

  舊的子列表為空。

* The two lists are identical.

  兩個列表完全相同。

* There is an insertion or removal of one or more widgets in exactly
  one place in the list.

  在列表的某個位置插入或刪除一個或多個 widget。

* If each list contains a widget with the same
  key<sup><a href="#a5">5</a></sup>, the two widgets are matched.

  如果新舊列表都包含相同 key <sup><a href="#a5">5</a></sup> 的 widget，
  那麼這兩個 widget 就會被認為是相同的。

The general approach is to match up the beginning and end of both child
lists by comparing the runtime type and key of each widget,
potentially finding a non-empty range in the middle of each list
that contains all the unmatched children. The framework then places
the children in the range in the old child list into a hash table
based on their keys. Next, the framework walks the range in the new
child list and queries the hash table by key for matches. Unmatched
children are discarded and rebuilt from scratch whereas matched children
are rebuilt with their new widgets.

通常的做法是從新舊子列表的頭部和尾部開始對每一個 widget 的執行時型別和 key
進行匹配，這樣就可能找到在兩個列表中間所有不匹配子節點的（非空）範圍。
然後框架將舊子列表中該範圍內的子項根據它的 key 放入一個雜湊表中。
接下來，框架將會遍歷新的子列表以尋找該範圍內能夠匹配雜湊表中的 key的子項。
無法匹配的子項將會被丟棄並從頭開始重建，
匹配到的子項則使用它們新的 widget 進行重建。

### Tree surgery

### 樹結構最佳化

Reusing elements is important for performance because elements own
two critical pieces of data: the state for stateful widgets and the
underlying render objects. When the framework is able to reuse an element,
the state for that logical part of the user interface is preserved
and the layout information computed previously can be reused,
often avoiding entire subtree walks. In fact, reusing elements is
so valuable that Flutter supports _non-local_ tree mutations that
preserve state and layout information.

重用 element 對效能非常重要，
這是因為 element 擁有兩份關鍵資料：Stateful widget 的狀態物件及底層的渲染物件。
當框架能夠重用 element 時，使用者介面的邏輯狀態資訊是不變的，
並且可以重用之前計算的佈局資訊，這通常可以避免遍歷整棵子樹。
事實上，重用 element 是非常有價值的，
因為 Flutter 支援 **全域** 樹更新，以此保留狀態和佈局資訊。

Developers can perform a non-local tree mutation by associating a `GlobalKey`
with one of their widgets. Each global key is unique throughout the
entire application and is registered with a thread-specific hash table.
During the build phase, the developer can move a widget with a global
key to an arbitrary location in the element tree. Rather than building
a fresh element at that location, the framework will check the hash
table and reparent the existing element from its previous location to
its new location, preserving the entire subtree.

開發者可透過將 `GlobalKey` 與其中一個 widget 相關聯來實施全域樹更新。
每個全域 key 在整個應用中都是唯一的，並使用特定於執行緒的雜湊表進行註冊。
在建構過程中，開發者可以使用全域 key 將 widget 移動到 element 樹的任意位置。
框架將不會在該位置上重新建構 element，
而是檢查雜湊表並將現有的 element 從之前的位置移動到新的位置，從而保留整棵子樹。

The render objects in the reparented subtree are able to preserve
their layout information because the layout constraints are the only
information that flows from parent to child in the render tree.
The new parent is marked dirty for layout because its child list has
changed, but if the new parent passes the child the same layout
constraints the child received from its old parent, the child can
return immediately from layout, cutting off the walk.

重新建構的子樹中的渲染物件能夠保留它們的佈局資訊，
這是因為佈局約束是渲染樹從父節點傳遞到子節點的唯一資訊。
子列表發生變化後，父節點將會被標記為髒，
但如果新的父節點傳遞給子節點的佈局約束與該子節點從舊的父節點接收到的相同，
那麼子節點可立即從佈局中返回，從而切斷佈局的向下傳遞。

Global keys and non-local tree mutations are used extensively by
developers to achieve effects such as hero transitions and navigation.

開發者廣泛使用全域 key 和全域樹更新來實現 hero transition 及導航等效果。

### Constant-factor optimizations

### 恆定因子最佳化

In addition to these algorithmic optimizations, achieving aggressive
composability also relies on several important constant-factor
optimizations. These optimizations are most important at the leaves of
the major algorithms discussed above.

除了上述演算法最佳化，實現積極可組合還需依賴幾個重要的恆定因子最佳化。
這些最佳化對於上面所討論的主要演算法是非常重要的。

* **Child-model agnostic.** Unlike most toolkits, which use child lists,
  Flutter’s render tree does not commit to a specific child model.
  For example, the `RenderBox` class has an abstract `visitChildren()`
  method rather than a concrete `firstChild` and `nextSibling` interface.
  Many subclasses support only a single child, held directly as a member
  variable, rather than a list of children. For example, `RenderPadding`
  supports only a single child and, as a result, has a simpler layout
  method that takes less time to execute.

  **子模型無關**。與大多數使用子列表的工具套件不同，
  Flutter 渲染樹不會記住一個特定的子模型。
  比如，類 `RenderBox` 存在一個抽象的 `visitChildren()` 方法，
  而非具體的 **firstChild** 和 **nextSibling** 介面。
  許多子類別僅支援直接作為其成員變數的單個子項，而非子項列表。
  比如，由於 `RenderPadding` 僅支援單個子節點，
  因此它擁有一個更為簡單、高效的佈局方法。

* **Visual render tree, logical widget tree.** In Flutter, the render
  tree operates in a device-independent, visual coordinate system,
  which means smaller values in the x coordinate are always towards
  the left, even if the current reading direction is right-to-left.
  The widget tree typically operates in logical coordinates, meaning
  with _start_ and _end_ values whose visual interpretation depends
  on the reading direction. The transformation from logical to visual
  coordinates is done in the handoff between the widget tree and the
  render tree. This approach is more efficient because layout and
  painting calculations in the render tree happen more often than the
  widget-to-render tree handoff and can avoid repeated coordinate conversions.

  **視覺渲染樹、widget 邏輯樹**。在 Flutter 中，
  渲染樹在與裝置無關的視覺座標系中執行，這意味著即使 x 軸的讀取方向是從右到左，
  其左側的值依舊小於右側。Widget 樹通常在邏輯座標中執行，
  這意味著擁有 **開始** 和 **結束** 值的視覺解釋取決於讀取方向。
  邏輯座標到視覺座標的轉換是在 widget 樹和渲染樹之間的切換中完成的。
  這種方法更為高效的原因是，渲染樹中的佈局和繪製計算
  比 widget 到渲染樹的切換更加頻繁，並且可以避免重複的座標轉換。

* **Text handled by a specialized render object.** The vast majority
  of render objects are ignorant of the complexities of text. Instead,
  text is handled by a specialized render object, `RenderParagraph`,
  which is a leaf in the render tree. Rather than subclassing a
  text-aware render object, developers incorporate text into their
  user interface using composition. This pattern means `RenderParagraph`
  can avoid recomputing its text layout as long as its parent supplies
  the same layout constraints, which is common, even during tree surgery.

  **透過專門的渲染物件處理文字**。 大多數渲染物件都不清楚文字的複雜性。
  相反，文字是由專門的渲染物件 `RenderParagraph` 進行處理，
  它是渲染樹中的一個葉子節點。
  開發者使用組合形式將文字併入到使用者介面中，
  而非使用文字感知渲染物件進行子類別化。
  該模式意味著 `RenderParagraph` 可避免
  文字佈局在父節點提供相同佈局約束下的重複計算，
  這是非常常見的，即使在樹最佳化期間也是如此。

* **Observable objects.** Flutter uses both the model-observation and
  the reactive paradigms. Obviously, the reactive paradigm is dominant,
  but Flutter uses observable model objects for some leaf data structures.
  For example, `Animation`s notify an observer list when their value changes.
  Flutter hands off these observable objects from the widget tree to the
  render tree, which observes them directly and invalidates only the
  appropriate stage of the pipeline when they change. For example,
  a change to an `Animation<Color>` might trigger only the paint phase
  rather than both the build and paint phases.

  **可觀察物件**。 Flutter 使用模型觀察及響應設計模式。顯而易見，
  響應模式占主導地位，但 Flutter 在某些葉子節點的資料結構上使用了可觀察物件。
  比如 `Animation` 會在值發生變化時通知觀察者列表。
  Flutter 將這些可觀察物件從 widget 樹轉移到渲染樹中，
  渲染樹直接監聽這些物件，並在它們改變時僅重繪管道的相關階段。
  比如，更改 `Animation<Color>` 可能只觸發繪製階段，
  而非整個建構和繪製階段。

Taken together and summed over the large trees created by aggressive
composition, these optimizations have a substantial effect on performance.

總的來說，這些最佳化對透過積極組合方式產生的大型樹結構的效能產生了重大影響。

### Separation of the Element and RenderObject trees

The `RenderObject` and `Element` (Widget) trees in Flutter are isomorphic
(strictly speaking, the `RenderObject` tree is a subset of the `Element`
tree). An obvious simplification would be to combine these trees into
one tree. However, in practice there are a number of benefits to having
these trees be separate:

* **Performance.** When the layout changes, only the relevant parts of
  the layout tree need to be walked. Due to composition, the element
  tree frequently has many additional nodes that would have to be skipped.

* **Clarity.** The clearer separation of concerns allows the widget
  protocol and the render object protocol to each be specialized to
  their specific needs, simplifying the API surface and thus lowering
  the risk of bugs and the testing burden.

* **Type safety.** The render object tree can be more type safe since it
  can guarantee at runtime that children will be of the appropriate type
  (each coordinate system, e.g. has its own type of render object).
  Composition widgets can be agnostic about the coordinate system used
  during layout (for example, the same widget exposing a part of the app
  model could be used in both a box layout and a sliver layout), and thus
  in the element tree, verifying the type of render objects would require
  a tree walk.

## Infinite scrolling

## 無限滾動

Infinite scrolling lists are notoriously difficult for toolkits.
Flutter supports infinite scrolling lists with a simple interface
based on the _builder_ pattern, in which a `ListView` uses a callback
to build widgets on demand as they become visible to the user during
scrolling. Supporting this feature requires _viewport-aware layout_
and _building widgets on demand_.

對於工具套件來說，實現無限滾動列表是非常困難的。Flutter
支援基於 **構造器** 模式實現的簡單無限滾動列表介面，
其中 `ListView` 使用回呼(Callback)按需建構 widget，
即它們只在滾動過程中才對使用者可見。
該功能需要 **視窗感知佈局** 及 **按需建構 widget** 的支援。

### Viewport-aware layout

### 視窗感知佈局

Like most things in Flutter, scrollable widgets are built using
composition. The outside of a scrollable widget is a `Viewport`,
which is a box that is "bigger on the inside," meaning its children
can extend beyond the bounds of the viewport and can be scrolled into
view. However, rather than having `RenderBox` children, a viewport has
`RenderSliver` children, known as _slivers_, which have a viewport-aware
layout protocol.

同 Flutter 中的大多數東西一樣，可滾動的 widget 是基於組合模式建構的。
可滾動 widget 的外部是一個 `Viewport`，這是一個擁有更大內部空間的盒子，
這意味著它的子節點可以超出視視窗的邊界並滾動到可視區域中。
但是，視視窗沒有 `RenderBox` 子節點，而是擁有被稱為 **sliver**，
實現了視窗感知協議的`RenderSliver` 子節點。

The sliver layout protocol matches the structure of the box layout
protocol in that parents pass constraints down to their children and
receive geometry in return. However, the constraint and geometry data
differs between the two protocols. In the sliver protocol, children
are given information about the viewport, including the amount of
visible space remaining. The geometry data they return enables a
variety of scroll-linked effects, including collapsible headers and
parallax.

sliver 佈局協議中父節點向下傳遞給子節點的約束資訊
及接收到的幾何資訊的結構與盒子佈局相同。
但約束和幾何資料在兩個協議之間不同。
在 sliver 協議中，子節點接收到的是關於視視窗的資訊，
這其中包含剩餘的可見空間量。
它們返回的幾何資料支援各種滾動連結效果，包括可摺疊標題及視差。

Different slivers fill the space available in the viewport in different
ways. For example, a sliver that produces a linear list of children lays
out  each child in order until the sliver either runs out of children or
runs out of space. Similarly, a sliver that produces a two-dimensional
grid of children fills only the portion of its grid that is visible.
Because they are aware of how much space is visible, slivers can produce
a finite number of children even if they have the potential to produce
an unbounded number of children.

不同的 sliver 以不同的方式填充視視窗中的可用空間。
比如，產生線性子列表的 sliver 按順序排列每個子節點，
直到 sliver 中無任何子節點或可用空間。
同理，產生二維子節點網格的 sliver 僅填充網格中的可見區域。
由於它們知道還有多大的可見空間，sliver 可以產生有限的子節點，
即使它們可能產生無限的子節點。

Slivers can be composed to create bespoke scrollable layouts and effects.
For example, a single viewport can have a collapsible header followed
by a linear list and then a grid. All three slivers will cooperate through
the sliver layout protocol to produce only those children that are actually
visible through the viewport, regardless of whether those children belong
to the header, the list, or the grid<sup><a href="#a6">6</a></sup>.

可組合 sliver 來建立特定的滾動佈局和效果。
比如，單個視視窗可以有一個摺疊標題、一個線性列表和一個網格。
所有這些 sliver 將按照 sliver 佈局協議進行協作，
只產生那些在視視窗實際可見的子節點，而不管這些子節點
是否屬於標題、列表或網格<sup><a href="#a6">6</a></sup>。

### Building widgets on demand

### 按需建構 widget

If Flutter had a strict _build-then-layout-then-paint_ pipeline,
the foregoing would be insufficient to implement an infinite scrolling
list because the information about how much space is visible through
the viewport is available only during the layout phase. Without
additional machinery, the layout phase is too late to build the
widgets necessary to fill the space. Flutter solves this problem
by interleaving the build and layout phases of the pipeline. At any
point in the layout phase, the framework can start building new
widgets on demand _as long as those widgets are descendants of the
render object currently performing layout_.

如果 Flutter 擁有一個嚴格的**從建構到佈局，再到繪製**的管道，
那麼前面的內容將不足以實現無限滾動列表，
這是因為只有在佈局階段才能透過視視窗獲取可用的空間資訊。
如果沒有額外的機制，在佈局階段建構用於填充空間的 widget 已經太遲了。
Flutter 使用將管道的建構與佈局交叉在一起的方式來解決這個問題。
在佈局階段的任意時刻，**只要這些 widget 是當前佈局的渲染物件的子節點**，
框架就可以按需建構新的 widget。

Interleaving build and layout is possible only because of the strict
controls on information propagation in the build and layout algorithms.
Specifically, during the build phase, information can propagate only
down the tree. When a render object is performing layout, the layout
walk has not visited the subtree below that render object, which means
writes generated by building in that subtree cannot invalidate any
information that has entered the layout calculation thus far. Similarly,
once layout has returned from a render object, that render object will
never be visited again during this layout, which means any writes
generated by subsequent layout calculations cannot invalidate the
information used to build the render object’s subtree.

只有嚴格控制建構及佈局中訊息傳播的演算法，才能實現建構和佈局的交叉執行。
也就是說，在建構過程中，訊息只能沿建構樹向下傳遞。當渲染物件進行佈局時，
佈局遍歷過程中並沒有存取該渲染物件的子樹，
這意味透過子樹建構的寫入無法使到目前為止已進入佈局計算過程的任何資訊失效。
無獨有偶，一旦佈局從渲染物件中返回，
在當前佈局過程中，該渲染物件將永遠不會被再次存取，
這意味後續佈局計算產生的任何寫入都不會使用於建構渲染物件的子樹的資訊失效。

Additionally, linear reconciliation and tree surgery are essential
for efficiently updating elements during scrolling and for modifying
the render tree when elements are scrolled into and out of view at
the edge of the viewport.

此外，線性協調及樹結構最佳化對於在滾動過程中有效更新 element，以及當 element
在視視窗邊緣滾動進出檢視期間修改渲染樹至關重要。

## API Ergonomics

## 人機工程 API

Being fast only matters if the framework can actually be used effectively.
To guide Flutter's API design towards greater usability, Flutter has been
repeatedly tested in extensive UX studies with developers. These studies
sometimes confirmed pre-existing design decisions, sometimes helped guide
the prioritization of features, and sometimes changed the direction of the
API design. For instance, Flutter's APIs are heavily documented; UX
studies confirmed the value of such documentation, but also highlighted
the need specifically for sample code and illustrative diagrams.

速度只有在框架能夠被有效使用時才有意義。
為了引導設計更高可用性的 Flutter API，
Flutter 已經在與開發者進行的廣泛使用者體驗研究中進行了反覆測試。
這些研究有時證實了已有的設計決策，
有時有助於引導功能的優先順序，有時會改變 API 的設計方向。
比如，Flutter 的 API 文件很多，
使用者體驗的研究不僅證實了這些文件的價值，
也同時強調了範例程式碼及說明性圖表的重要性。

This section discusses some of the decisions made in Flutter's API design
in aid of usability.

本節將要討論 Flutter API 設計中為提高可用性所做的一些決策。

### Specializing APIs to match the developer's mindset

### 與開發者思維模式相匹配的專項 API

The base class for nodes in Flutter's `widget`, `Element`, and `RenderObject`
trees does not define a child model. This allows each node to be
specialized for the child model that is applicable to that node.

Flutter 中 `widget`、`Element` 和 `RenderObject`
的基底類別節點不定義子類別模型。該機制允許每個節點對適用於該節點的子模型進行客製化。

Most `widget` objects have a single child `widget`, and therefore only expose
a single `child` parameter. Some widgets support an arbitrary number of
children, and expose a `children` parameter that takes a list.
Some widgets don't have any children at all and reserve no memory,
and have no parameters for them. Similarly, `RenderObjects` expose APIs
specific to their child model. `RenderImage` is a leaf node, and has no
concept of children. `RenderPadding` takes a single child, so it has storage
for a single pointer to a single child. `RenderFlex` takes an arbitrary
number of children and manages it as a linked list.

大多數 `widget` 物件都有一個子 `widget` 物件，
因此它只暴露了一個 `child` 引數。一些 widget 支援任意數量的子節點，
並暴露了一個獲取子節點列表的 `children` 引數。
有些 widget 無任何子節點、不保留記憶體且無任何引數。
同樣的，`RenderObjects` 暴露特定於子模型的 API。
`RenderImage` 是一個沒有子節點的葉子節點。
`RenderPadding` 只持有一個子節點，
因此它有一個指向單個子節點的指標儲存空間。
`RenderFlex` 接受任意數量的子節點，並透過連結串列對其進行管理。

In some rare cases, more complicated child models are used. The
`RenderTable` render object's constructor takes an array of arrays of
children, the class exposes getters and setters that control the number
of rows and columns, and there are specific methods to replace
individual children by x,y coordinate, to add a row, to provide a
new array of arrays of children, and to replace the entire child list
with a single array and a column count. In the implementation,
the object does not use a linked list like most render objects but
instead uses an indexable array.

在一些罕見情況下，將使用更復雜的子類別模型。 
渲染物件 `RenderTable` 的建構函式需要使用二維陣列來儲存子節點，
所以該類暴露了用於控制行和列數量的 getter 及 setter 方法，
還有一些可以用 x、y 軸座標來替換單個子節點的特殊方法，
可透過提供一個新的子節點陣列來新增新行，
並用單個數組及列的個數來替換整個子節點列表。
該物件並不像大多數渲染物件那樣使用連結串列，而是使用可索引陣列來實現。

The `Chip` widgets and `InputDecoration` objects have fields that match
the slots that exist on the relevant controls. Where a one-size-fits-all
child model would force semantics to be layered on top of a list of
children, for example, defining the first child to be the prefix value
and the second to be the suffix, the dedicated child model allows for
dedicated named properties to be used instead.

`Chip` widget 和 `InputDecoration` 物件具有與其控制中的插槽相匹配的欄位。
如果一個通用子模型將強制語義定義在子列表之上，比如將第一個子節點定義為字首，
第二個子節點定義為字尾，那麼專用子模型允許使用特有的命名屬性。

This flexibility allows each node in these trees to be manipulated in
the way most idiomatic for its role. It's rare to want to insert a cell
in a table, causing all the other cells to wrap around; similarly,
it's rare to want to remove a child from a flex row by index instead
of by reference.

這種靈活性允許樹中的每個子節點以其最常用的方式操作它的角色。
很少有人想要在表格中插入一個單元格，從而導致其他所有單元格被環繞；
同樣的，很少有人想要透過索引而不是透過參考從 flex 行中刪除子項。

The `RenderParagraph` object is the most extreme case: it has a child of
an entirely different type, `TextSpan`. At the `RenderParagraph` boundary,
the `RenderObject` tree transitions into being a `TextSpan` tree.

`RenderParagraph` 物件是最極端的情況：它有一個完全不同型別的子節點，`TextSpan`。在 `RenderParagraph`
的邊界，`RenderObject` 樹會被轉換為 `TextSpan` 樹。

The overall approach of specializing APIs to meet the developer's
expectations is applied to more than just child models.

專門用於滿足開發者期望的 API 的一切方法不僅適用於子模型。

Some rather trivial widgets exist specifically so that developers
will find them when looking for a solution to a problem. Adding a
space to a row or column is easily done once one knows how, using
the `Expanded` widget and a zero-sized `SizedBox` child, but discovering
that pattern is unnecessary because searching for `space`
uncovers the `Spacer` widget, which uses `Expanded` and `SizedBox` directly
to achieve the effect.

專門存在一些瑣碎的 widget，以便開發者在尋找問題解決方案時能夠發現並使用它們。
一旦知道如何使用 `Expanded` 和大小為零的 `SizedBox` 子部件，
就可以輕鬆地為行或列新增空格，但你會發現這種模式是沒有必要的，
因為搜尋 `space` 所找到的 `Spacer`，
它是直接使用 `Expanded` 和 `SizedBox` 來達到同樣的效果的。

Similarly, hiding a widget subtree is easily done by not including the
widget subtree in the build at all. However, developers typically expect
there to be a widget to do this, and so the `Visibility` widget exists
to wrap this pattern in a trivial reusable widget.

同理，可以透過在建構過程中不包含 widget 子樹來輕鬆隱藏 widget 子樹。
但開發者通常希望有一個 widget 來執行該操作，
因此 `Visibility` 的存在便是將此模式封裝在一個簡單的可重用 widget 中。

### Explicit arguments

### 明確的引數

UI frameworks tend to have many properties, such that a developer is
rarely able to remember the semantic meaning of each constructor
argument of each class. As Flutter uses the reactive paradigm,
it is common for build methods in Flutter to have many calls to
constructors. By leveraging Dart's support for named arguments,
Flutter's API is able to keep such build methods clear and understandable.

UI 框架往往擁有大量的屬性，
因此很少有開發者能夠記住每個類別的每個建構函式引數的作用。
由於 Flutter 使用響應式程式設計正規化，
因此在 Flutter 中，建構方法通常會對建構函式進行多次呼叫。
透過利用 Dart 的命名引數，Flutter 中的 API 能夠使這些建構方法保持清晰易懂。

This pattern is extended to any method with multiple arguments,
and in particular is extended to any boolean argument, so that isolated
`true` or `false` literals in method calls are always self-documenting.
Furthermore, to avoid confusion commonly caused by double negatives
in APIs, boolean arguments and properties are always named in the
positive form (for example, `enabled: true` rather than `disabled: false`).

該模式已被擴充到任何具有多個引數（尤其是具有 boolean 型別引數）的方法，
因此獨立的 `true` 或 `false` 值在方法呼叫中總是自我描述的。
此外，為避免 API 中通常由雙重否定所造成的困惑，
boolean 型別的引數和屬性始終以肯定的形式命名
（比如，使用 `enabled: true` 而非 `disabled: false`）。

### Paving over pitfalls

### 引數陷阱

A technique used in a number of places in the Flutter framework is to
define the API such that error conditions don't exist. This removes
entire classes of errors from consideration.

在 Flutter 框架中被大量使用的一項技術是定義不存在錯誤條件的 API。
這樣可以避免考慮整個錯誤類別。

For example, interpolation functions allow one or both ends of the
interpolation to be null, instead of defining that as an error case:
interpolating between two null values is always null, and interpolating
from a null value or to a null value is the equivalent of interpolating
to the zero analog for the given type. This means that developers
who accidentally pass null to an interpolation function will not hit
an error case, but will instead get a reasonable result.

比如插值函式允許插值的一端或兩端為空，
而不是將其定義為錯誤：兩個空值之間的插值永遠為空，
並且從空值或空值插值等效於對指定型別進行零模擬插值。
這意味著不小心將 null 傳遞給插值函式的開發者不會遇到錯誤，
而是會得到一個合理結果。

A more subtle example is in the `Flex` layout algorithm. The concept of
this layout is that the space given to the flex render object is
divided among its children, so the size of the flex should be the
entirety of the available space. In the original design, providing
infinite space would fail: it would imply that the flex should be
infinitely sized, a useless layout configuration. Instead, the API
was adjusted so that when infinite space is allocated to the flex
render object, the render object sizes itself to fit the desired
size of the children, reducing the possible number of error cases.

一個更加微妙的例子是 `Flex` 佈局演算法。
該佈局給予 flex 渲染物件的空間被它的子節點所劃分。
因此 flex 的大小應該是整個可用空間。
在最初的設計中提供無限空間將導致失敗：
這意味著 flex 應該是無限大且無用的佈局設定。
然而，透過對 API 的改造，在為 flex 物件提供無限空間時，
渲染物件會調整自身大小來滿足所需子節點的大小，
從而減少可能出現的錯誤次數。

The approach is also used to avoid having constructors that allow
inconsistent data to be created. For instance, the `PointerDownEvent`
constructor does not allow the `down` property of `PointerEvent` to
be set to `false` (a situation that would be self-contradictory);
instead, the constructor does not have a parameter for the `down`
field and always sets it to `true`.

該方法也可用於避免使用允許建立不符合邏輯的資料的建構函式。例如，`PointerDownEvent`
的建構函式不允許將 `PointerEvent` 的 `down` 屬性設定為
`false`（這種情況是自相矛盾的）；相反，建構函式沒有關於欄位 `down`
的引數，且將值始終設定為 `true`。

In general, the approach is to define valid interpretations for all
values in the input domain. The simplest example is the `Color` constructor.
Instead of taking four integers, one for red, one for green,
one for blue, and one for alpha, each of which could be out of range,
the default constructor takes a single integer value, and defines
the meaning of each bit (for example, the bottom eight bits define the
red component), so that any input value is a valid color value.

一般情況下，該方法用於為輸入域中的所有值定義有效的解釋。
最簡單的例子是 `Color` 的建構函式。相對於接受四個整型引數
（分別用於表示紅色、綠色、藍色和 alpha），其中任何一個都可能超出範圍，
它的預設建構函式僅接受一個整數值，並定義每位的含義（例如，低八位代表紅色），
以便任何輸入都是有效的顏色值。

A more elaborate example is the `paintImage()` function. This function
takes eleven arguments, some with quite wide input domains, but they
have been carefully designed to be mostly orthogonal to each other,
such that there are very few invalid combinations.

一個更復雜的例子是 `paintImage()` 函式。該函式需要 11 個引數，其中一些具有相當寬泛的輸入域，
但它們都經過精心設計且大部分都能夠彼此相交，因此很少出現無效組合。

### Reporting error cases aggressively

### 積極報告錯誤

Not all error conditions can be designed out. For those that remain,
in debug builds, Flutter generally attempts to catch the errors very
early and immediately reports them. Asserts are widely used.
Constructor arguments are sanity checked in detail. Lifecycles are
monitored and when inconsistencies are detected they immediately
cause an exception to be thrown.

並非所有的錯誤都能被設計出來。對於那些遺漏的錯誤，
在 debug 版本中，Flutter 通常會嘗試儘早捕獲並立即報告。
它使用了大量的斷言，對建構函式引數進行了詳細的完整性檢查，
並監視其生命週期，一旦檢測到不一致，它們會立即引發例外。

In some cases, this is taken to extremes: for example, when running
unit tests, regardless of what else the test is doing, every `RenderBox`
subclass that is laid out aggressively inspects whether its intrinsic
sizing methods fulfill the intrinsic sizing contract. This helps catch
errors in APIs that might otherwise not be exercised.

這在某些情況下是極端情況：比如，在執行單元測試時，無論測試使用案例正在做什麼，
每個 `RenderBox` 子類別都會主動地檢查其內部大小調整方法是否滿足內部大小調整契約。
這有助於捕獲可能無法執行的 API 錯誤。

When exceptions are thrown, they include as much information as
is available. Some of Flutter's error messages proactively probe the
associated stack trace to determine the most likely location of the
actual bug. Others walk the relevant trees to determine the source
of bad data. The most common errors include detailed instructions
including in some cases sample code for avoiding the error, or links
to further documentation.

當例外丟擲時，它們會包含儘可能多的資訊。
Flutter 中的一些錯誤會主動探測相關的堆疊追蹤資訊，
以確定實際錯誤最可能發生的位置。其他錯誤則透過相關樹來確定壞資料的來源。
最常見的錯誤包含詳細說明（在某些情況下會包含避免錯誤的範例程式碼），
或指向其他文件的連結。

### Reactive paradigm

### 響應式

Mutable tree-based APIs suffer from a dichotomous access pattern:
creating the tree's original state typically uses a very different
set of operations than subsequent updates. Flutter's rendering layer
uses this paradigm, as it is an effective way to maintain a persistent tree,
which is key for efficient layout and painting. However, it means
that direct interaction with the rendering layer is awkward at best
and bug-prone at worst.

可變的基於樹結構的 API 受二元存取模式的影響：建立樹的原始狀態通常使用與後續更新完全不同的操作集。Flutter
的渲染層使用了這種正規化，因為它是維護持久樹的有效方法，是高效佈局和繪製的關鍵所在。
但這也意味著，與渲染層的直接互動是十分笨拙的，甚至極其容易出錯。

Flutter's widget layer introduces a composition mechanism using the
reactive paradigm<sup><a href="#a7">7</a></sup> to manipulate the
underlying rendering tree.
This API abstracts out the tree manipulation by combining the tree
creation and tree mutation steps into a single tree description (build)
step, where, after each change to the system state, the new configuration
of the user interface is described by the developer and the framework
computes the series of tree mutations necessary to reflect this new
configuration.

Flutter 在 widget 層引入了一個使用響應式來操作底層渲染樹的組合機制<sup><a href="#a7">7</a></sup>。
該 API 透過將樹的建立和更新步驟整合到一個單一的樹結構描述（建構）中，
從而將樹操作抽象出來，這包括：每次系統狀態更新之後，
開發者用於描述使用者介面的新配置；
框架對於新配置所需要進行的一系列樹更新計算。

### Interpolation

### 插值

Since Flutter's framework encourages developers to describe the interface
configuration matching the current application state, a mechanism exists
to implicitly animate between these configurations.

由於 Flutter 鼓勵開發者描述與當前應用狀態相匹配的介面配置，因此存在一種在這些配置之間執行隱含的動畫機制。

For example, suppose that in state S<sub>1</sub> the interface consists
of a circle, but in state S<sub>2</sub> it consists of a square.
Without an animation mechanism, the state change would have a jarring
interface change. An implicit animation allows the circle to be smoothly
squared over several frames.

例如，假設介面在狀態 S<sub>1</sub> 由一個圓形組成，在狀態 S<sub>2</sub>
時由一個正方形組成。如果沒有動畫機制，狀態更改將導致不和諧的介面更改。
隱含動畫則允許介面在幾個幀的時間裡由圓形平滑地過渡到正方形。

Each feature that can be implicitly animated has a stateful widget that
keeps a record of the current value of the input, and begins an animation
sequence whenever the input value changes, transitioning from the current
value to the new value over a specified duration.

每個可執行隱含動畫的特性都包含一個 Stateful widget，它用於記錄輸入的當前值，並在輸入值改變時開始執行動畫序列，
並在指定的持續時間內從當前值轉換為新值。

This is implemented using `lerp` (linear interpolation) functions using
immutable objects. Each state (circle and square, in this case)
is represented as an immutable object that is configured with
appropriate settings (color, stroke width, etc) and knows how to paint
itself. When it is time to draw the intermediate steps during the animation,
the start and end values are passed to the appropriate `lerp` function
along with a _t_ value representing the point along the animation,
where 0.0 represents the `start` and 1.0 represents the
`end`<sup><a href="#a8">8</a></sup>,
and the function returns a third immutable object representing the
intermediate stage.

這是使用不可變物件的 `lerp`（線性插值）函式來實現的。
每個狀態（這裡為圓形和正方形）代表一個配置中包含恰當設定（比如顏色、筆劃寬度等）
且知道如何繪製自己的不可變物件。在動畫繪製中間步驟時，
開始和結束值連同表示動畫中點的 **t** 值一併傳遞給 `lerp`函式。
其中 0.0 代表開始 `start`，1.0 代表結束 `end`<sup><a href="#a8">8</a></sup>，
並且該方法返回表示中間階段的第三個不可變物件。

For the circle-to-square transition, the `lerp` function would return
an object representing a "rounded square" with a radius described as
a fraction derived from the _t_ value, a color interpolated using the
`lerp` function for colors, and a stroke width interpolated using the
`lerp` function for doubles. That object, which implements the
same interface as circles and squares, would then be able to paint
itself when requested to.

對於從圓形到正方形的轉換，`lerp` 函式將返回一個圓角正方形物件，
其半徑被描述為從 **t** 值匯出的分數，使用 `lerp` 函式進行插值計算的顏色，
以及使用 `lerp` 函式進行雙倍插值計算的筆劃寬度。
該物件與圓形、正方形一樣具有相同的介面實現，並且可以在請求時進行自我繪製。

This technique allows the state machinery, the mapping of states to
configurations, the animation machinery, the interpolation machinery,
and the specific logic relating to how to paint each frame to be
entirely separated from each other.

該技術允許狀態機、狀態到配置的對映、動畫和插值機制
以及與如何繪製每一楨完全分離的特定邏輯。

This approach is broadly applicable. In Flutter, basic types like
`Color` and `Shape` can be interpolated, but so can much more elaborate
types such as `Decoration`, `TextStyle`, or `Theme`. These are
typically constructed from components that can themselves be interpolated,
and interpolating the more complicated objects is often as simple as
recursively interpolating all the values that describe the complicated
objects.

在 Flutter 中，該機制得到了廣泛應用，
無論是像 `Color` 和 `Shape` 這樣的基本型別，
還是像 `Decoration`，`TextStyle` 或 `Theme` 這樣更為複雜的型別，
都是可以進行插值處理的。它們通常是由可插入元件構成的，
並且插入更復雜的物件通常就像遞迴插入描述複雜物件的所有值一樣簡單。

Some interpolatable objects are defined by class hierarchies. For example,
shapes are represented by the `ShapeBorder` interface, and there exists a
variety of shapes, including `BeveledRectangleBorder`, `BoxBorder`,
`CircleBorder`, `RoundedRectangleBorder`, and `StadiumBorder`. A single
`lerp` function cannot have a priori knowledge of all the possible types,
and therefore the interface instead defines `lerpFrom` and `lerpTo` methods,
which the static `lerp` method defers to. When told to interpolate from
a shape A to a shape B, first B is asked if it can `lerpFrom` A, then,
if it cannot, A is instead asked if it can `lerpTo` B. (If neither is
possible, then the function returns A from values of `t` less than 0.5,
and returns B otherwise.)

一些插值物件由類層次結構定義。比如，形狀由 `ShapeBorder` 介面表示，
並且存在多種形狀型別，包括：
`BeveledRectangleBorder`、`BoxBorder`、`CircleBorder`、`RoundedRectangleBorder`
和 `StadiumBorder`。單一的 `lerp` 函式並不能瞭解所有可能的型別資訊，
因此介面定義了 `lerpFrom` 和 `lerpTo` 方法以替代靜態的 `lerp` 方法。
當被告知從形狀 A 切換到 B 時，將首選詢問 B 是否 `lerpFrom` A，
如其答案為否，則詢問 A 是否可以 `lerpTo` B
（如兩者的答案均為否，如果 `t` 的值小於 0.5 則返回 A，否則返回 B）。

This allows the class hierarchy to be arbitrarily extended, with later
additions being able to interpolate between previously-known values
and themselves.

這允許類層次結構的任意擴充，後續新增的能夠在先前已知值與它們之間進行插值處理。

In some cases, the interpolation itself cannot be described by any of
the available classes, and a private class is defined to describe the
intermediate stage. This is the case, for instance, when interpolating
between a `CircleBorder` and a `RoundedRectangleBorder`.

在某些情況下，插值本身不能被任何可用的類描述，
並且定義一個私有類來描述中間狀態。
比如在 `CircleBorder` 和 `RoundedRectangleBorder` 之間進行插值時就是如此。

This mechanism has one further added advantage: it can handle interpolation
from intermediate stages to new values. For example, half-way through
a circle-to-square transition, the shape could be changed once more,
causing the animation to need to interpolate to a triangle. So long as
the triangle class can `lerpFrom` the rounded-square intermediate class,
the transition can be seamlessly performed.

該機制的另外一個優點是：它可以處理從中間態到新值的插值。
比如，在圓形到正方形過渡的中途，形狀可能再次改變，
導致動畫需要插值到一個三角形。只要該三角形類是 `lerpFrom` 圓形到正方形的中間類，
就可以無縫進行轉換。

## Conclusion

## 結論

Flutter’s slogan, "everything is a widget," revolves around building
user interfaces by composing widgets that are, in turn, composed of
progressively more basic widgets. The result of this aggressive
composition is a large number of widgets that require carefully
designed algorithms and data structures to process efficiently.
With some additional design, these data structures also make it
easy for developers to create infinite scrolling lists that build
widgets on demand when they become visible.

Flutter 一切都是 widget 的口號是圍繞著透過組合 widget 來建構使用者介面，
widget 又由更為基礎的 widget 構成。
這種積極組合的結果是需要精心設計的演算法和資料結構才能有效處理大量的 widget。
透過一些額外的機制，這些資料結構還能使開發者輕鬆建構無限滾動列表，
以便在 widget 可見時進行按需建構。

---
**Footnotes:**

**腳註：**

<sup><a name="a1">1</a></sup> For layout, at least. It might be revisited
  for painting, for building the accessibility tree if necessary,
  and for hit testing if necessary.

<sup><a name="a1">1</a></sup> 至少對於佈局來說。它可能會重新審視繪製、在必要時建構輔助功能樹、
  以及必要時的命中測試。

<sup><a name="a2">2</a></sup> Reality, of course, is a bit more
  complicated. Some layouts involve intrinsic dimensions or baseline
  measurements, which do involve an additional walk of the relevant subtree
  (aggressive caching is used to mitigate the potential for quadratic
  performance in the worst case). These cases, however, are surprisingly
  rare. In particular, intrinsic dimensions are not required for the
  common case of shrink-wrapping.

<sup><a name="a2">2</a></sup> 現實情況當然更復雜一些。有些佈局涉及內部維度及基線測量，
  這涉及到相關子樹的額外遍歷���在最壞的情況下，使用積極快取來降低潛在的二次效能）。
  但是，這些情況非常罕見。特別是在常見的 shrink-wrapping 情況下，根本不需要內部尺寸。

<sup><a name="a3">3</a></sup> Technically, the child's position is not
  part of its RenderBox geometry and therefore need not actually be
  calculated during layout. Many render objects implicitly position
  their single child at 0,0 relative to their own origin, which
  requires no computation or storage at all. Some render objects
  avoid computing the position of their children until the last
  possible moment (for example, during the paint phase), to avoid
  the computation entirely if they are not subsequently painted.

<sup><a name="a3">3</a></sup> 嚴格來說，子節點的位置不是其 RenderBox
  幾何體的一部分，因此無需在佈局期間進行實際計算。
  許多渲染物件隱含地將它們的單個子節點相對於它們自身的原點定位在 0,0 處，
  這根本不需要進行計算或儲存。
  一些渲染物件避免計算它們子節點的位置直到最後可能需要的時刻
  （比如，在繪製過程中），以避免以後沒有被繪製時的計算。

<sup><a name="a4">4</a></sup>  There exists one exception to this rule.
  As discussed in the [Building widgets on demand](#building-widgets-on-demand)
  section, some widgets can be rebuilt as a result of a change in layout
  constraints. If a widget marked itself dirty for unrelated reasons in
  the same frame that it also is affected by a change in layout constraints,
  it will be updated twice. This redundant build is limited to the
  widget itself and does not impact its descendants.

<sup><a name="a4">4</a></sup>  該規則有一個例外。正如 [按需建構 widget](#building-widgets-on-demand)
  中所描述的，由於佈局約束的變化，一些 widget 可以被重建。如果 widget
  在同一幀中因與此無關的原因被標記為髒，同時也由於它受佈局約束的影響，該 widget
  將會被建構兩次。該次冗餘建構僅限於 widget 自身，並不會影響其後代節點。

<sup><a name="a5">5</a></sup> A key is an opaque object optionally
  associated with a widget whose equality operator is used to influence
  the reconciliation algorithm.

<sup><a name="a5">5</a></sup> 鍵是一個可選的與 widget 相關聯的不透明物件，
  它的相等運運算元用於影響協調演算法。

<sup><a name="a6">6</a></sup>  For accessibility, and to give applications
  a few extra milliseconds between when a widget is built and when it
  appears on the screen, the viewport creates (but does not paint)
  widgets for a few hundred pixels before and after the visible widgets.

<sup><a name="a6">6</a></sup>  對於可及性，並在 widget
  建構及在視窗顯示的過程中為應用提供幾毫米的時間，視視窗會在可見 widget
  的前後為幾百個畫素建構（但不進行繪製）widget。

<sup><a name="a7">7</a></sup>  This approach was first made popular by
  Facebook's React library.

<sup><a name="a7">7</a></sup>  該方法首次在 Facebook 的 React 框架中得到了廣泛使用。

<sup><a name="a8">8</a></sup>  In practice, the _t_ value is allowed
  to extend past the 0.0-1.0 range, and does so for some curves. For
  example, the "elastic" curves overshoot briefly in order to represent
  a bouncing effect. The interpolation logic typically can extrapolate
  past the start or end as appropriate. For some types, for example,
  when interpolating colors, the _t_ value is effectively clamped to
  the 0.0-1.0 range.

<sup><a name="a8">8</a></sup>  實際上，允許 **t** 值超過 0.0-1.0
  的範圍，這同樣適用於某些曲線。比如 elastic 緩動曲線透過短暫的過沖來表示彈跳效應。
  插值邏輯通常可以在適當情況下推算出起始或結束點。對於某些型別，
  比如在插入顏色時，**t** 值被有效地固定到 0.0-1.0 的範圍。
