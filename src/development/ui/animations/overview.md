---
title: Animations overview
title: 動畫概覽
short-title: Overview
short-title: 概覽
description: An overview of animation concepts.
description: 動畫效果的概念簡述。
tags: 使用者介面,Flutter UI,動畫
keywords: 概覽,概念
---

The animation system in Flutter is based on typed
[`Animation`][] objects. Widgets can either
incorporate these animations in their build
functions directly by reading their current value and listening to their
state changes or they can use the animations as the basis of more elaborate
animations that they pass along to other widgets.

Flutter 中的動畫系統基於 [`Animation`][]。
Widgets 可以直接將這些動畫合併到自己的 build 方法中
來讀取它們的當前值或者監聽它們的狀態變化，
或者可以將其作為的更復雜動畫的基礎傳遞給其他 widgets。

## Animation

The primary building block of the animation system is the
[`Animation`][] class. An animation represents a value
of a specific type that can change over the lifetime of
the animation. Most widgets that perform an animation
receive an `Animation` object as a parameter,
from which they read the current value of the animation
and to which they listen for changes to that value.

動畫系統的首要組成部分就是 [`Animation`][] 類別。
一個動畫表現為可在它的生命週期內發生變化的特定型別的值。
大多數需要執行動畫的 widgets 都需要接收一個
`Animation` 物件作為引數，
從而能從中獲取到動畫的當前狀態值以及
應該監聽哪些具體值的更改。

### `addListener`

Whenever the animation's value changes,
the animation notifies all the listeners added with
[`addListener`][]. Typically, a [`State`][]
object that listens to an animation calls
[`setState`][] on itself in its listener callback
to notify the widget system that it needs to
rebuild with the new value of the animation.

每當動畫的狀態值發生變化時，
動畫都會通知所有透過 [`addListener`][] 新增的監聽器。
通常，一個正在監聽動畫的 [`State`][]
物件會呼叫自身的 [`setState`][] 方法，
將自身傳入這些監聽器的回呼(Callback)函式來通知
widget 系統需要根據新狀態值進行重新建構。

This pattern is so common that there are two widgets
that help widgets rebuild when animations change value:
[`AnimatedWidget`][] and [`AnimatedBuilder`][].
The first, `AnimatedWidget`, is most useful for
stateless animated widgets. To use `AnimatedWidget`,
simply subclass it and implement the [`build`][] function.
The second, `AnimatedBuilder`, is useful for more complex widgets
that wish to include an animation as part of a larger build function.
To use `AnimatedBuilder`, simply construct the widget
and pass it a `builder` function.

這種模式非常常見，所以有兩個 widgets
可以幫助其他 widgets 在動畫改變值時進行重新建構：
[`AnimatedWidget`][] 和 [`AnimatedBuilder`][]。
第一個是 `AnimatedWidget`，
對於無狀態動畫 widgets 來說是尤其有用的。
要使用 `AnimatedWidget`，
只需繼承它並實現一個 [`build`][] 方法。
第二個是 `AnimatedBuilder`，
對於希望將動畫作為複雜 widgets 的
build 方法的其中一部分的情況非常有用。
要使用 `AnimatedBuilder`，
只需構造 widget 並將 `AnimatedBuilder`
傳遞給 widget 的 `builder` 方法。

### `addStatusListener`

Animations also provide an [`AnimationStatus`][],
which indicates how the animation will evolve over time.
Whenever the animation's status changes,
the animation notifies all the listeners added with
[`addStatusListener`][]. Typically, animations start
out in the `dismissed` status, which means they're
at the beginning of their range. For example,
animations that progress from 0.0 to 1.0
will be `dismissed` when their value is 0.0.
An animation might then run `forward` (from 0.0 to 1.0)
or perhaps in `reverse` (from 1.0 to 0.0).
Eventually, if the animation reaches the end of its range
(1.0), the animation reaches the `completed` status.

動畫還提供了一個 [`AnimationStatus`][]，
表示動畫將如何隨時間進行變化。每當動畫的狀態發生變化時，
動畫都會通知所有透過 [`addStatusListener`][] 新增的監聽器。
通常情況下，動畫會從 `dismissed` 狀態開始，
表示它處於變化區間的開始點。舉例來說，
從 0.0 到 1.0 的動畫在 `dismissed` 狀態時的值應該是 0.0。
動畫進行的下一狀態可能是 `forward`（比如從 0.0 到 1.0）
或者 `reverse`（比如從 1.0 到 0.0）。
最終，如果動畫到達其區間的結束點（比如 1.0），
則動畫會變成 `completed` 狀態。

## Animation&shy;Controller

To create an animation, first create an [`AnimationController`][].
As well as being an animation itself, an `AnimationController`
lets you control the animation. For example,
you can tell the controller to play the animation
[`forward`][] or [`stop`][] the animation.
You can also [`fling`][] animations,
which uses a physical simulation, such as a spring,
to drive the animation.

要建立動畫，首先要建立一個 [`AnimationController`][]。
除了作為動畫本身，`AnimationController` 還可以用來控制動畫。
例如，你可以透過控制器讓動畫
正向播放 [`forward`][] 或停止動畫 [`stop`][]。
你還可以新增物理模擬效果 [`fling`][]（例如彈簧效果）來驅動動畫。

Once you've created an animation controller,
you can start building other animations based on it.
For example, you can create a [`ReverseAnimation`][]
that mirrors the original animation but runs in the
opposite direction (from 1.0 to 0.0).
Similarly, you can create a [`CurvedAnimation`][]
whose value is adjusted by a [`Curve`][].

一旦建立了一個動畫控制器，你可以基於它來建構其他動畫。
例如，你可以建立一個 [`ReverseAnimation`][]，
效果是複製一個動畫但是將其反向執行（比如從 1.0 到 0.0）。
同樣，你可以建立一個 [`CurvedAnimation`][]，
效果是用 [`Curve`][] 來調整動畫的值。

## Tweens

## 補間動畫

To animate beyond the 0.0 to 1.0 interval, you can use a
[`Tween<T>`][], which interpolates between its
[`begin`][] and [`end`][] values. Many types have specific
`Tween` subclasses that provide type-specific interpolation.
For example, [`ColorTween`][] interpolates between colors and
[`RectTween`][] interpolates between rects.
You can define your own interpolations by creating
your own subclass of `Tween` and overriding its
[`lerp`][] function.

如果想要在 0.0 到 1.0 的區間之外設定動畫，
可以使用 [`Tween<T>`][]，它可以在它的 [`begin`][] 值和
[`end`][] 值之間進行插值補間。
許多類都有特定的 `Tween` 子類別，
它們能提供基於特定型別的插值行為。
例如， [`ColorTween`][] 可以在顏色間進行插值，
[`RectTween`][] 可以在矩形之間進行插值。
你可以透過建立自己的 `Tween` 子類別並覆蓋其
[`lerp`][] 方法來定義自己的補間動畫。

By itself, a tween just defines how to interpolate
between two values. To get a concrete value for the
current frame of an animation, you also need an
animation to determine the current state.
There are two ways to combine a tween
with an animation to get a concrete value:

補間動畫本身只定義瞭如何在兩個值之間進行插值。
要獲取動畫當前幀的具體值，
還需要一個動畫來確定當前狀態。
有兩種方法可以將補間動畫與動畫組合在一起以獲得動畫的具體值：

1. You can [`evaluate`][] the tween at the current
   value of an animation. This approach is most useful
   for widgets that are already listening to the animation and hence
   rebuilding whenever the animation changes value.

   你可以用 [`evaluate`][] 方法處理動畫的當前值從而得到對應的插值。
   這種方法對於已經監聽動畫並因此在動畫改變值時
   重新建構的 widgets 是最有效的。

2. You can [`animate`][] the tween based on the animation.
   Rather than returning a single value, the animate function
   returns a new `Animation` that incorporates the tween.
   This approach is most useful when you want to give the
   newly created animation to another widget,
   which can then read the current value that incorporates
   the tween as well as listen for changes to the value.

   你可以用 [`animate`][] 方法處理一個動畫。
   相對於返回單個值，animate 方法返回一個包含補間動畫插值的
   新的 `Animation`。這種方法對於當你想要將新建立的動畫提供給
   另一個 widget 時最有效，它可以直接讀取包含補間動畫的插值
   以及監聽對應插值的更改。

## Architecture

## 架構

Animations are actually built from a number of core building blocks.

動畫實際上是由許多核心模組共同建構的。

### Scheduler

### 排程器

The [`SchedulerBinding`][] is a singleton class
that exposes the Flutter scheduling primitives.

[`SchedulerBinding`][] 是一個暴露出 Flutter 排程原語的單例類別。

For this discussion, the key primitive is the frame callbacks.
Each time a frame needs to be shown on the screen,
Flutter's engine triggers a "begin frame" callback that
the scheduler multiplexes to all the listeners registered using
[`scheduleFrameCallback()`][]. All these callbacks are
given the official time stamp of the frame, in
the form of a `Duration` from some arbitrary epoch. Since all the
callbacks have the same time, any animations triggered from these
callbacks will appear to be exactly synchronised even
if they take a few milliseconds to be executed.

在這一節，關鍵原語是幀回呼(Callback)。每當一幀需要在螢幕上顯示時，
Flutter 的引擎會觸發一個 “開始幀” 回呼(Callback)，
排程程式會將其多路傳輸給所有使用
[`scheduleFrameCallback()`][] 註冊的監聽器。
所有這些回呼(Callback)不管在任意狀態或任意時刻都可以
收到這一幀的絕對時間戳。由於所有回呼(Callback)收到時間戳都相同，
因此這些回呼(Callback)觸發的任何動畫看起來都是完全同步的，
即使它們需要幾毫秒才能執行。

### Tickers

### 執行器

The [`Ticker`][] class hooks into the scheduler's
[`scheduleFrameCallback()`][]
mechanism to invoke a callback every tick.

[`Ticker`][] 類掛載在排程器的 [`scheduleFrameCallback()`][] 的機制上，
來達到每次執行都會觸發回呼(Callback)的效果。

A `Ticker` can be started and stopped. When started, it returns a
`Future` that will resolve when it is stopped.

一個 `Ticker` 可以被啟動和停止，啟動時，
它會返回一個 `Future`，這個 `Future` 在 `Ticker` 停止時會被改為完成狀態。

Each tick, the `Ticker` provides the callback with the
duration since the first tick after it was started. 

每次執行, `Ticker` 都會為回呼(Callback)函式提供從
`Ticker` 開始執行到現在的持續時間。

Because tickers always give their elapsed time relative to the first
tick after they were started; tickers are all synchronised. If you
start three tickers at different times between two ticks, they will all
nonetheless be synchronised with the same starting time, and will
subsequently tick in lockstep. Like people at a bus-stop,
all the tickers wait for a regularly occurring event
(the tick) to begin moving (counting time).

因為執行器總是會提供在自它們開始執行以來的持續時間，
所以所有執行器都是同步的。
如果你在兩幀之間的不同時刻啟動三個執行器，
它們都會被同步到相同的開始時間，並隨後同步執行。

### Simulations

### 模擬器

The [`Simulation`][] abstract class maps a
relative time value (an elapsed time) to a
double value, and has a notion of completion.

[`Simulation`][] 抽象類別將相對時間值（執行時間）對映為雙精度值，
並且有完成的概念。

In principle simulations are stateless but in practice
some simulations (for example,
[`BouncingScrollSimulation`][] and
[`ClampingScrollSimulation`][])
change state irreversibly when queried.

原則上，模擬器是無狀態的，但在實踐中，
一些模擬器（例如 [`BouncingScrollSimulation`][]
和 [`ClampingScrollSimulation`][]）
在查詢時會不可逆地被改變狀態。

There are [various concrete implementations][]
of the `Simulation` class for different effects.

針對不同的效果，`Simulation` 類有
[各種具體實現][various concrete implementations]。

### Animatables

The [`Animatable`][] abstract class maps a
double to a value of a particular type.

[`Animatable`][] 抽象類別將雙精度值對映為特定型別的值。

`Animatable` classes are stateless and immutable.

`Animatable` 類是無狀態和不可變的。

#### Tweens

#### 補間動畫

The [`Tween<T>`][] abstract class maps a double
value nominally in the range 0.0-1.0 to a typed value
(for example, a `Color`, or another double).
It is an `Animatable`.

[`Tween<T>`][] 抽象類別將名義範圍為 0.0-1.0
的雙精度值對映到某個型別值（例如 `Color` 或其他雙精度值)。
它屬於 `Animatable`。

It has a notion of an output type (`T`),
a `begin` value and an `end` value of that type,
and a way to interpolate (`lerp`) between the begin
and end values for a given input value (the double nominally in
the range 0.0-1.0).

它有一個輸出型別（`T`）的概念，
這個輸出型別有一個 `begin` 值和一個`end` 值，
以及在給定輸入值的起始值和結束值
（名義範圍為 0.0-1.0 的雙精度值）之間
插值（`lerp`）的方法。

`Tween` classes are stateless and immutable.

`Tween` 類是無狀態和不可變的。

#### Composing animatables

#### 組合 animatables

Passing an `Animatable<double>` (the parent) to an `Animatable`'s
`chain()` method creates a new `Animatable` subclass that applies the
parent's mapping then the child's mapping.

將 `Animatable<double>`（父類）傳遞給一個
`Animatable` 的 `chain()` 方法會建立一個新的
`Animatable` 子類別，這個子類別會先應用父類別的對映，
然後應用子類別的對映。

### Curves

### 曲線

The [`Curve`][] abstract class maps doubles
nominally in the range 0.0-1.0 to doubles
nominally in the range 0.0-1.0.

[`Curve`][] 抽象類別將名義範圍為 0.0-1.0 的雙精度值
對映到名義範圍為 0.0-1.0 的雙精度值。

`Curve` classes are stateless and immutable.

`Curve` 類是無狀態和不可變的。

### Animations

### 動畫

The [`Animation`][] abstract class provides a
value of a given type, a concept of animation
direction and animation status, and a listener interface to
register callbacks that get invoked when the value or status change.

[`Animation`][] 抽象類別提供給定型別的
值、動畫方向的概念和動畫狀態和一個監聽器介面，
這個監聽器介面用來註冊值或狀態的改變時被呼叫的回呼(Callback)。

Some subclasses of `Animation` have values that never change
([`kAlwaysCompleteAnimation`][], [`kAlwaysDismissedAnimation`][],
[`AlwaysStoppedAnimation`][]); registering callbacks on
these has no effect as the callbacks are never called.

有些 `Animation` 的子類別值是永遠不變的
（[`kAlwaysCompleteAnimation`][]、[`kAlwaysDismissedAnimation`][] 和
[`AlwaysStoppedAnimation`][]），
在這些子類別上註冊回呼(Callback)沒有任何效果，因為這些回呼(Callback)永遠不會被呼叫。

The `Animation<double>` variant is special because it can be used to
represent a double nominally in the range 0.0-1.0, which is the input
expected by `Curve` and `Tween` classes, as well as some further
subclasses of `Animation`.

`Animation<double>` 變數很特殊，
因為它可以被用來表示名義範圍為 0.0-1.0 的雙精度值，
也就是 `Curve` 和 `Tween` 類以及動畫的一些其他子類別所期望的輸入。

Some `Animation` subclasses are stateless,
merely forwarding listeners to their parents.
Some are very stateful.

有些 `Animation` 的子類別是無狀態的，
只是將監聽器轉發給其父級；另外有些是有狀態的。

#### Composable animations

#### 組合動畫

Most `Animation` subclasses take an explicit "parent"
`Animation<double>`. They are driven by that parent.

大多數 `Animation` 子類別都採用明確的
“父級提供的” `Animation<double>`。可以說它們是由父級驅動的。

The `CurvedAnimation` subclass takes an `Animation<double>` class (the
parent) and a couple of `Curve` classes (the forward and reverse
curves) as input, and uses the value of the parent as input to the
curves to determine its output. `CurvedAnimation` is immutable and
stateless.

`CurvedAnimation` 子類別接收一個 `Animation<double>`
類（父級）和幾個 `Curve` 類（正向和反向曲線）作為輸入，
並使用父級的值作為輸入提供給曲線來確定它的輸出。
`CurvedAnimation` 是不可變和無狀態的。

The `ReverseAnimation` subclass takes an
`Animation<double>` class as its parent and reverses
all the values of the animation. It assumes the parent
is using a value nominally in the range 0.0-1.0 and returns
a value in the range 1.0-0.0. The status and direction of the parent
animation are also reversed. `ReverseAnimation` is immutable and
stateless.

`ReverseAnimation` 子類別接收一個 `Animation<double>` 類作為它的父級，
但反轉動畫所有的值。它假定父級使用名義範圍為 0.0-1.0 的雙精度值，
並返回範圍為 1.0-0.0 的值。父級動畫的狀態和方向也會被反轉。
`ReverseAnimation` 是不可變和無狀態的。

The `ProxyAnimation` subclass takes an `Animation<double>` class as
its parent and merely forwards the current state of that parent.
However, the parent is mutable.

`ProxyAnimation` 子類別接收一個 `Animation<double>`
類作為其父級，並僅轉發該父級的當前狀態。
然而，父級是可變的。

The `TrainHoppingAnimation` subclass takes two parents, and switches
between them when their values cross.

`TrainHoppingAnimation` 子類別接收兩個父類，
並在它們的值交叉時在它們之間切換。

#### Animation Controllers

#### 動畫控制器

The [`AnimationController`][] is a stateful
`Animation<double>` that uses a `Ticker` to give itself life.
It can be started and stopped. At each tick, it takes the time
elapsed since it was started and passes it to a `Simulation` to obtain
a value. That is then the value it reports. If the `Simulation`
reports that at that time it has ended, then the controller stops
itself.

[`AnimationController`][] 是一個有狀態的
`Animation<double>`，並使用一個 `Ticker` 來提供生命週期，
它可以被啟動和停止。每次執行，它會收集從啟動開始經過的時間，
並將其傳遞給 `Simulation` 來獲得一個值，
這就是在當前時間戳下它應該傳遞的值。
如果 `Simulation` 反饋此時動畫已經結束了，
則控制器就會自行停止。

The animation controller can be given a lower and upper bound to
animate between, and a duration.

可以給動畫控制器設定動畫執行的下限和上限，
還有動畫的持續時間。

In the simple case (using `forward()` or `reverse()`), the animation controller simply does a linear
interpolation from the lower bound to the upper bound (or vice versa,
for the reverse direction) over the given duration.

在一般情況下
（使用 `forward()`、`reverse()`、`play()` 或者 `resume()`），
動畫控制器只是簡單地在持續時間內線性地從
下限至上限（反之亦然，用於在反向方向）進行插值補間。

When using `repeat()`, the animation controller uses a linear
interpolation between the given bounds over the given duration, but
does not stop.

當使用 `repeat()` 時，動畫控制器會在持續時間內
線性地在上下邊界之間進行插值補間，
但會一直重複，不會停止。

When using `animateTo()`, the animation controller does a linear
interpolation over the given duration from the current value to the
given target. If no duration is given to the method, the default
duration of the controller and the range described by the controller's
lower bound and upper bound is used to determine the velocity of the
animation.

當使用 `animateTo()` 時，動畫控制器會在持續時間內
線性地從當前值到給定目標值進行插值補間。
如果方法沒有指定持續時間，
則使用控制器的預設持續時間和控制器的上下限範圍來確定動畫的速度。

When using `fling()`, a `Force` is used to create a specific
simulation which is then used to drive the controller.

當使用 `fling()` 時，一個 `Force` 被用來建立一個特定的模擬器，
然後用來驅動控制器。

When using `animateWith()`, the given simulation is used to drive the
controller.

當使用 `animateWith()` 時，給定的模擬器會被用於驅動控制器。

These methods all return the future that the `Ticker` provides and
which will resolve when the controller next stops or changes
simulation.

這些方法都會返回 `Ticker` 提供的將來值，
交由控制器下一次停止或改變模擬器時來完成。

#### Attaching animatables to animations

#### 將 animatables 附加到動畫上

Passing an `Animation<double>` (the new parent) to an `Animatable`'s
`animate()` method creates a new `Animation` subclass that acts like
the `Animatable` but is driven from the given parent.

將 `Animation<double>`（新父級）傳遞給一個
`Animatable` 類別的 `animate()` 方法將建立一個
新的 `Animation` 子類別，它的作用類似於 `Animatable`，
但是由給定的父級驅動。

[`addListener`]: {{site.api}}/flutter/animation/Animation/addListener.html
[`addStatusListener`]: {{site.api}}/flutter/animation/Animation/addStatusListener.html
[`AlwaysStoppedAnimation`]: {{site.api}}/flutter/animation/AlwaysStoppedAnimation-class.html
[`Animatable`]: {{site.api}}/flutter/animation/Animatable-class.html
[`animate`]: {{site.api}}/flutter/animation/Animatable/animate.html
[`AnimatedBuilder`]: {{site.api}}/flutter/widgets/AnimatedBuilder-class.html
[`AnimationController`]: {{site.api}}/flutter/animation/AnimationController-class.html
[`AnimatedWidget`]: {{site.api}}/flutter/widgets/AnimatedWidget-class.html
[`Animation`]: {{site.api}}/flutter/animation/Animation-class.html
[`AnimationStatus`]: {{site.api}}/flutter/animation/AnimationStatus-class.html
[`begin`]: {{site.api}}/flutter/animation/Tween/begin.html
[`BouncingScrollSimulation`]: {{site.api}}/flutter/widgets/BouncingScrollSimulation-class.html
[`build`]: {{site.api}}/flutter/widgets/AnimatedWidget/build.html
[`ClampingScrollSimulation`]: {{site.api}}/flutter/widgets/ClampingScrollSimulation-class.html
[`ColorTween`]: {{site.api}}/flutter/animation/ColorTween-class.html
[`Curve`]: {{site.api}}/flutter/animation/Curves-class.html
[`CurvedAnimation`]: {{site.api}}/flutter/animation/CurvedAnimation-class.html
[`end`]: {{site.api}}/flutter/animation/Tween/end.html
[`evaluate`]: {{site.api}}/flutter/animation/Animatable/evaluate.html
[`fling`]: {{site.api}}/flutter/animation/AnimationController/fling.html
[`forward`]: {{site.api}}/flutter/animation/AnimationController/forward.html
[`kAlwaysCompleteAnimation`]: {{site.api}}/flutter/animation/kAlwaysCompleteAnimation-constant.html
[`kAlwaysDismissedAnimation`]: {{site.api}}/flutter/animation/kAlwaysDismissedAnimation-constant.html
[`lerp`]: {{site.api}}/flutter/animation/Tween/lerp.html
[`RectTween`]: {{site.api}}/flutter/animation/RectTween-class.html
[`ReverseAnimation`]: {{site.api}}/flutter/animation/ReverseAnimation-class.html
[`scheduleFrameCallback()`]: {{site.api}}/flutter/scheduler/SchedulerBinding/scheduleFrameCallback.html
[`SchedulerBinding`]: {{site.api}}/flutter/scheduler/SchedulerBinding-mixin.html
[`setState`]: {{site.api}}/flutter/widgets/State/setState.html
[`Simulation`]: {{site.api}}/flutter/physics/Simulation-class.html
[`State`]: {{site.api}}/flutter/widgets/State-class.html
[`stop`]: {{site.api}}/flutter/animation/AnimationController/stop.html
[`Ticker`]: {{site.api}}/flutter/scheduler/Ticker-class.html
[`Tween<T>`]: {{site.api}}/flutter/animation/Tween-class.html
[various concrete implementations]: {{site.api}}/flutter/physics/physics-library.html
