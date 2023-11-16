---
title: Taps, drags, and other gestures
title: 點選、拖動和其他手勢
short-title: Gestures
description: How gestures, such as taps and drags, work in Flutter.
description: Flutter 是如何實現點選和拖動這樣類似的手勢識別的。
tags: 使用者介面,Flutter UI
keywords: 進階,手勢,人機互動
---

This document explains how to listen for, and respond to,
_gestures_ in Flutter.
Examples of gestures include taps, drags, and scaling.

這個章節將會講解如何監聽和響應 Flutter 的手勢操作 **gestures**。 
典型的手勢操作包括點選、拖動和縮放。

The gesture system in Flutter has two separate layers.
The first layer has raw pointer events that describe
the location and movement of pointers (for example,
touches, mice, and styli) across the screen.
The second layer has _gestures_ that describe semantic
actions that consist of one or more pointer movements.

Flutter 中的手勢有兩個不同的層次：第一層是原始的指針指向事件，
描述了螢幕上由觸控板、滑鼠、指示筆等觸發的指標的位置和移動。
第二層包含 **gestures**，描述了由上述一個或多個指標移動組成的具有特殊語義的操作。


## Pointers

## 指標

Pointers represent raw data about the user's interaction
with the device's screen.
There are four types of pointer events:

Pointer 代表的是人機介面互動的原始資料。
一共有四種指標事件：

[`PointerDownEvent`][]
<br> The pointer has contacted the screen at a particular location.

[`PointerDownEvent`][]
<br>指標在特定位置與螢幕接觸

[`PointerMoveEvent`][]
<br>The pointer has moved from one location on the screen to another.
 
[`PointerMoveEvent`][]
<br>指標從螢幕的一個位置移動到另外一個位置 

[`PointerUpEvent`][]
<br>The pointer has stopped contacting the screen.

[`PointerUpEvent`][]
<br>指標與螢幕停止接觸

[`PointerCancelEvent`][]
<br>Input from this pointer is no longer directed towards this app.

[`PointerCancelEvent`][]
<br>指標的輸入已經不再指向此應用

On pointer down, the framework does a _hit test_ on your app
to determine which widget exists at the location where the
pointer contacted the screen. The pointer down event
(and subsequent events for that pointer) are then dispatched
to the innermost widget found by the hit test.
From there, the events bubble up the tree and are dispatched
to all the widgets on the path from the innermost
widget to the root of the tree. There is no mechanism for
canceling or stopping pointer events from being dispatched further.

在指標下落事件中，框架做了一個 **hit test** 
的操作確定與螢幕發生接觸的位置上有哪些元件以及分發給最內部的元件去響應。
事件會沿著元件樹從這個最內部的元件向元件樹的根部冒泡分發。
並且不存在用於取消或停止指標事件進行進一步分發的機制。

To listen to pointer events directly from the widgets layer, use a
[`Listener`][] widget. However, generally,
consider using gestures (as discussed below) instead.

使用 [`Listener`]({{site.api}}/flutter/widgets/Listener-class.html) 
可以在元件層直接監聽指標事件。然而，一般情況下，請考慮使用下面的 gestures 替代。

[`Listener`]: {{site.api}}/flutter/widgets/Listener-class.html
[`PointerCancelEvent`]: {{site.api}}/flutter/gestures/PointerCancelEvent-class.html
[`PointerDownEvent`]: {{site.api}}/flutter/gestures/PointerDownEvent-class.html
[`PointerMoveEvent`]: {{site.api}}/flutter/gestures/PointerMoveEvent-class.html
[`PointerUpEvent`]: {{site.api}}/flutter/gestures/PointerUpEvent-class.html

## Gestures

## 手勢

Gestures represent semantic actions (for example, tap, drag,
and scale) that are recognized from multiple individual pointer
events, potentially even multiple individual pointers.
Gestures can dispatch multiple events, corresponding to the
lifecycle of the gesture (for example, drag start,
drag update, and drag end):

Gesture 代表的是語義操作（比如點選、拖動、縮放）。
通常由一系列單獨的指標事件組成，甚至是一系列單獨的指標組成。
Gesture 可以分發多種事件，
對應著手勢的生命週期（比如開始拖動、拖動更新、結束拖動）。

**Tap**

**點選**

`onTapDown`
<br> A pointer that might cause a tap has contacted the screen at a
 particular location.

`onTapDown`
<br> 指標在發生接觸的螢幕的特定位置可能引發點選事件。

`onTapUp`
<br> A pointer that triggers a tap has stopped contacting
  the screen at a particular location.

`onTapUp`
<br> 觸發點選事件的觸點已經在某個點停止與螢幕互動。

`onTap`
<br> The pointer that previously triggered the `onTapDown`
  has also triggered `onTapUp` which ends up causing a tap.

`onTap`
<br> 觸發 `onTapDown` 的觸點此時已觸發了 `onTapUp`，即產生了點選事件。

`onTapCancel`
<br> The pointer that previously triggered the `onTapDown`
  won't end up causing a tap.

`onTapCancel`
<br> 指標已經觸發了 `onTapDown`，但是最終不會形成一個點選事件。

**Double tap**

**雙擊** 

`onDoubleTap`
<br> The user has tapped the screen at the same location twice in
  quick succession.

`onDoubleTap`
<br> 使用者在螢幕的相同位置上快速點選了兩次。

**Long press**

**長按**

`onLongPress`
<br> A pointer has remained in contact with the
  screen at the same location for a long period of time.

`onLongPress`
<br> 指標在螢幕的相同位置上保持接觸持續一長段時間。

**Vertical drag**

**縱向拖動**

`onVerticalDragStart`
<br> A pointer has contacted the screen and might begin to
  move vertically.

`onVerticalDragStart`
<br> 指標和螢幕產生接觸並可能開始縱向移動。

`onVerticalDragUpdate`
<br> A pointer that is in contact with the screen and
    moving vertically has moved in the vertical direction.

`onVerticalDragUpdate`
<br> 指標和螢幕產生接觸，在縱向上發生移動並保持移動。

`onVerticalDragEnd`
<br> A pointer that was previously in contact with the screen
    and moving vertically is no longer in contact with the
    screen and was moving at a specific velocity when it
    stopped contacting the screen.

`onVerticalDragEnd`
<br> 指標先前和螢幕產生了接觸，以特定速度縱向移動，並且此後不會在螢幕接觸上發生縱向移動。

**Horizontal drag**

**橫向拖動**

`onHorizontalDragStart`
<br> A pointer has contacted the screen and might begin to
  move horizontally.

`onHorizontalDragStart`
<br> 指標和螢幕產生接觸並可能開始橫向移動。

`onHorizontalDragUpdate`
<br> A pointer that is in contact with the screen and
  moving horizontally has moved in the horizontal direction.

`onHorizontalDragUpdate`
<br> 指標和螢幕產生接觸，在橫向上發生移動並保持移動。

`onHorizontalDragEnd`
<br> A pointer that was previously in contact with the
  screen and moving horizontally is no longer in contact
  with the screen and was moving at a specific velocity
  when it stopped contacting the screen.

`onHorizontalDragEnd`
<br> 指標先前和螢幕產生了接觸，以特定速度橫向移動，並且此後不會在螢幕接觸上發生橫向移動。

**Pan**

**移動**

`onPanStart`
<br> A pointer has contacted the screen and might begin to move
  horizontally or vertically. This callback crashes if
  `onHorizontalDragStart` or `onVerticalDragStart` is set.

`onPanStart`
<br> 指標和螢幕產生接觸並可能開始橫向移動或者縱向移動。
  如果設定了 `onHorizontalDragStart` 或者 `onVerticalDragStart`，
  該回調方法會引發崩潰。

`onPanUpdate`
<br> A pointer that is in contact with the screen and is moving
  in the vertical or horizontal direction. This callback
  crashes if `onHorizontalDragUpdate` or `onVerticalDragUpdate`
  is set.

`onPanUpdate`
<br> 指標和螢幕產生接觸，在橫向或者縱向上發生移動並保持移動。
  如果設定了 `onHorizontalDragUpdate` 或者 `onVerticalDragUpdate`，
  該回調方法會引發崩潰。

`onPanEnd`
<br> A pointer that was previously in contact with screen
  is no longer in contact with the screen and is moving
  at a specific velocity when it stopped contacting the screen.
  This callback crashes if `onHorizontalDragEnd` or
  `onVerticalDragEnd` is set.

`onPanEnd`
<br> 指標先前和螢幕產生了接觸，並且以特定速度移動，
  此後不再在螢幕接觸上發生移動。
  如果設定了 `onHorizontalDragEnd` 或者 `onVerticalDragEnd`，
  該回調方法會引發崩潰。

### Adding gesture detection to widgets

### 為 widgets 新增手勢檢測

To listen to gestures from the widgets layer,
use a [`GestureDetector`][].

從元件層監聽手勢，需要用到 [`GestureDetector`][]。

{{site.alert.note}}

  To learn more, watch this short
  Widget of the Week video on the `GestureDetector` widget:

  瞭解更多，請參考下方「每週 Widget」的裡關於 GestureDetector 的短影片：

  <iframe class="full-width" src="{{site.youtube-site}}/embed/WhVXkCFPmK4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{{site.alert.end}}

If you're using Material Components,
many of those widgets already respond to taps or gestures.
For example, [`IconButton`][] and [`TextButton`][]
respond to presses (taps), and [`ListView`][]
responds to swipes to trigger scrolling.
If you aren't using those widgets, but you want the
"ink splash" effect on a tap, you can use [`InkWell`][].

如果使用 Material 風格的元件，其中的許多元件都能夠支援響應點選或者手勢事件。
比如 [`IconButton`][] 和 [`TextButton`][] 響應了按壓事件（點選事件），
[`ListView`][] 響應了滾動事件。如果使用了上述元件，
你也可以使用 [`InkWell`][] 來實現點選的“水波紋”效果。

[`GestureDetector`]: {{site.api}}/flutter/widgets/GestureDetector-class.html
[`IconButton`]: {{site.api}}/flutter/material/IconButton-class.html
[`InkWell`]: {{site.api}}/flutter/material/InkWell-class.html
[`ListView`]: {{site.api}}/flutter/widgets/ListView-class.html
[`TextButton`]: {{site.api}}/flutter/material/TextButton-class.html

### Gesture disambiguation

### 手勢消歧處理

At a given location on screen,
there might be multiple gesture detectors.
For example:

在螢幕的指定位置上，可能有多個手勢捕捉器。例如：

* A `ListTile` has a tap recognizer that responds
  to the entire `ListTile`, and a nested one around
  a trailing icon button. The screen rect of the
  trailing icon is now covered by two gesture
  recognizers that need to negotiate for who handles
  the gesture if it turns out to be a tap.
* A single `GestureDetector` covers a screen area
  configured to handle multiple gestures,
  such as a long press and a tap.
  The `tap` recognizer must now negotiate
  with the `long press` recognizer when
  the user touches that part of the screen.
  Depending on what happens next with that pointer,
  one of the two recognizers receives the gesture,
  or neither receives the gesture if the user
  performs something that's neither a tap nor a long press.

All of these gesture detectors listen to the stream
of pointer events as they flow past and attempt to recognize
specific gestures. The [`GestureDetector`] widget decides
which gestures to attempt to recognize based on which of its
callbacks are non-null.

所有的手勢捕捉器監聽了指標輸入流事件並判斷出特定手勢。
[`GestureDetector`][] widget
能夠基於手勢的回呼(Callback)是否非空決定是否應該嘗試去識別該手勢。

When there is more than one gesture recognizer for a given
pointer on the screen, the framework disambiguates which
gesture the user intends by having each recognizer join
the _gesture arena_. The gesture arena determines which
gesture wins using the following rules:

當螢幕上的指定指標有多個手勢識別器時，
框架會透過給每個手勢識別器加入 **gesture arena** 來處理手勢消歧。
gesture arena，也稱作手勢競技場，會利用下述規則確定哪個手勢在競爭中勝出：

* At any time, a recognizer can eliminate itself and leave the
  arena. If there's only one recognizer left in the arena,
  that recognizer wins.

  在任何時候，識別器都可以宣告失敗並離開競技場。
  如果競技場中只有一個識別器，那麼這個識別器就是勝者。

* At any time, a recognizer can declare itself the winner,
  causing all of the remaining recognizers to lose.

  在任何時候，任何識別器都可以宣告勝利，
  這將導致這個識別器勝出，其他識別器失敗。

For example, when disambiguating horizontal and vertical dragging,
both recognizers enter the arena when they receive the pointer
down event. The recognizers observe the pointer move events.
If the user moves the pointer more than a certain number of
logical pixels horizontally, the horizontal recognizer declares
the win and the gesture is interpreted as a horizontal drag.
Similarly, if the user moves more than a certain number of logical
pixels vertically, the vertical recognizer declares itself the winner.

比如，當縱向拖動和橫向拖動需要處理消歧，
當指標下落事件發生時，縱向和橫向識別器都會進入競技場，
觀測指標移動事件。如果使用者在橫向上移動超過了特定畫素，
橫向識別器會宣告勝利，手勢也會被當作橫向拖動處理。
同樣的，如果使用者在縱向上移動超過了特定的畫素，縱向識別器會宣告勝利。

The gesture arena is beneficial when there is only a horizontal
(or vertical) drag recognizer. In that case, there is only one
recognizer in the arena and the horizontal drag is recognized
immediately, which means the first pixel of horizontal movement
can be treated as a drag and the user won't need to wait for
further gesture disambiguation.
