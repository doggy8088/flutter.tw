---
title: Animate a widget using a physics simulation
title: Widget 的物理模擬動畫效果
description: How to implement a physics animation.
diff2html: true
description: 如何實現物理動畫。
tags: cookbook, 實用課程, 動畫效果
keywords: 物理動畫效果,重力效果,互動
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/animation/physics_simulation/"?>

Physics simulations can make app interactions feel realistic and interactive.
For example, you might want to animate a widget to act as if it were attached to
a spring or falling with gravity.

物理模擬能夠讓應用富有真實感和更好的互動性。
例如，你可能會為一個 widget 新增動畫，
讓它看起來就像安著彈簧，或是在隨重力下落。

This recipe demonstrates how to move a widget from a dragged point back to the
center using a spring simulation.

這個指南示範瞭如何將 widget 從拖動的點移回到中心，
並使用彈簧模擬效果。

This recipe uses these steps:

這個示範將進行下面幾步：

1. Set up an animation controller

   建立一個動畫控制器

2. Move the widget using gestures

   使用手勢移動 widget

3. Animate the widget

   對 widget 進行動畫

4. Calculate the velocity to simulate a springing motion

   計算速度以模擬彈跳運動

## Step 1: Set up an animation controller

## 第一步：建立一個動畫控制器

Start with a stateful widget called `DraggableCard`:

首先，建立一個叫做 `DraggableCard` 的 stateful widget：

<?code-excerpt "lib/starter.dart"?>
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MaterialApp(home: PhysicsCardDragDemo()));
}

class PhysicsCardDragDemo extends StatelessWidget {
  const PhysicsCardDragDemo({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: const DraggableCard(
        child: FlutterLogo(
          size: 128,
        ),
      ),
    );
  }
}

class DraggableCard extends StatefulWidget {
  const DraggableCard({required this.child, super.key});

  final Widget child;

  @override
  State<DraggableCard> createState() => _DraggableCardState();
}

class _DraggableCardState extends State<DraggableCard> {
  @override
  void initState() {
    super.initState();
  }

  @override
  void dispose() {
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Align(
      child: Card(
        child: widget.child,
      ),
    );
  }
}
```

Make the `_DraggableCardState` class extend from
[SingleTickerProviderStateMixin][].
Then construct an [AnimationController][] in
`initState` and set `vsync` to `this`.

讓 `_DraggableCardState` 類繼承至 [SingleTickerProviderStateMixin][]。
然後在 `initState` 中構造一個 [AnimationController][]，並將其 `vsync` 屬性設為 `this`。

{{site.alert.note}}

  Extending `SingleTickerProviderStateMixin` allows the state object to be a
  `TickerProvider` for the `AnimationController`. For more information, see the
  documentation for [TickerProvider][].
  
  繼承的 `SingleTickerProviderStateMixin` 讓 state 物件為 `AnimationController` 
  提供了 `TickerProvider` 的能力。要獲得更多資訊，請檢視 [TickerProvider][] 文件。
  
{{site.alert.end}}

<?code-excerpt "lib/{starter,step1}.dart"?>
```diff
--- lib/starter.dart
+++ lib/step1.dart
@@ -29,14 +29,20 @@
   State<DraggableCard> createState() => _DraggableCardState();
 }

-class _DraggableCardState extends State<DraggableCard> {
+class _DraggableCardState extends State<DraggableCard>
+    with SingleTickerProviderStateMixin {
+  late AnimationController _controller;
+
   @override
   void initState() {
     super.initState();
+    _controller =
+        AnimationController(vsync: this, duration: const Duration(seconds: 1));
   }

   @override
   void dispose() {
+    _controller.dispose();
     super.dispose();
   }
```

## Step 2: Move the widget using gestures

## 第二步：使用手勢移動 widget

Make the widget move when it's dragged, and add an [Alignment][] field to the
`_DraggableCardState` class:

讓 widget 可以被拖拽，併為 `_DraggableCardState`
類新增一個 [Alignment][] 範圍。

<?code-excerpt "lib/{step1,step2}.dart (alignment)"?>
```diff
--- lib/step1.dart (alignment)
+++ lib/step2.dart (alignment)
@@ -1,3 +1,4 @@
 class _DraggableCardState extends State<DraggableCard>
     with SingleTickerProviderStateMixin {
   late AnimationController _controller;
+  Alignment _dragAlignment = Alignment.center;
```

Add a [GestureDetector][] that handles the `onPanDown`, `onPanUpdate`, and
`onPanEnd` callbacks. To adjust the alignment, use a [MediaQuery][] to get the
size of the widget, and divide by 2. (This converts units of "pixels dragged" to
coordinates that [Align][] uses.) Then, set the `Align` widget's `alignment` to
`_dragAlignment`:

新增一個 [GestureDetector][] 來捕獲 `onPanDown`、`onPanUpdate`，
以及 `onPanEnd` 回呼(Callback)。為了調整對齊方式，請使用 [MediaQuery][]
來獲得 widget 的大小，然後除以 2。
（這會將「拖動的畫素」單位轉為 [Align][] 使用的座標。）
然後，將 `Align` widget 的 `alignmnt` 屬性設為 `_dragAlignment`。

<?code-excerpt "lib/{step1,step2}.dart (build)"?>
```diff
--- lib/step1.dart (build)
+++ lib/step2.dart (build)
@@ -1,8 +1,22 @@
 @override
 Widget build(BuildContext context) {
-  return Align(
-    child: Card(
-      child: widget.child,
+  var size = MediaQuery.of(context).size;
+  return GestureDetector(
+    onPanDown: (details) {},
+    onPanUpdate: (details) {
+      setState(() {
+        _dragAlignment += Alignment(
+          details.delta.dx / (size.width / 2),
+          details.delta.dy / (size.height / 2),
+        );
+      });
+    },
+    onPanEnd: (details) {},
+    child: Align(
+      alignment: _dragAlignment,
+      child: Card(
+        child: widget.child,
+      ),
     ),
   );
 }
```

## Step 3: Animate the widget

## 第三步：對 widget 進行動畫

When the widget is released, it should spring back to the center.

當一個 widget 被釋放，它應該就會彈回中心。

Add an `Animation<Alignment>` field and an `_runAnimation` method. This
method defines a `Tween` that interpolates between the point the widget was
dragged to, to the point in the center.

新增一個 `Animation<Alignment>`，以及 `_runAnimation` 方法。
此方法定義了一個 `Tween`，它在 widget 被拖動到的點之間插入到中心點。

<?code-excerpt "lib/{step2,step3}.dart (animation)"?>
```diff
--- lib/step2.dart (animation)
+++ lib/step3.dart (animation)
@@ -1,4 +1,5 @@
 class _DraggableCardState extends State<DraggableCard>
     with SingleTickerProviderStateMixin {
   late AnimationController _controller;
+  late Animation<Alignment> _animation;
   Alignment _dragAlignment = Alignment.center;
```

<?code-excerpt "lib/step3.dart (runAnimation)"?>
```dart
void _runAnimation() {
  _animation = _controller.drive(
    AlignmentTween(
      begin: _dragAlignment,
      end: Alignment.center,
    ),
  );
  _controller.reset();
  _controller.forward();
}
```

Next, update `_dragAlignment` when the `AnimationController` produces a
value:

接下來，當 `AnimationController` 產生一個值時，更新 `_dragAlignment`：

<?code-excerpt "lib/{step2,step3}.dart (initState)"?>
```diff
--- lib/step2.dart (initState)
+++ lib/step3.dart (initState)
@@ -3,4 +3,9 @@
   super.initState();
   _controller =
       AnimationController(vsync: this, duration: const Duration(seconds: 1));
+  _controller.addListener(() {
+    setState(() {
+      _dragAlignment = _animation.value;
+    });
+  });
 }
```

Next, make the `Align` widget use the `_dragAlignment` field:

下一步，讓 `Align` widget 使用 `_dragAlignment` 欄位：

<?code-excerpt "lib/step3.dart (align)"?>
```dart
child: Align(
  alignment: _dragAlignment,
  child: Card(
    child: widget.child,
  ),
),
```

Finally, update the `GestureDetector` to manage the animation controller:

最後，更新 `GestureDetector` 來管理動畫控制器：

<?code-excerpt "lib/{step2,step3}.dart (gesture)"?>
```diff
--- lib/step2.dart (gesture)
+++ lib/step3.dart (gesture)
@@ -1,5 +1,7 @@
 return GestureDetector(
-  onPanDown: (details) {},
+  onPanDown: (details) {
+    _controller.stop();
+  },
   onPanUpdate: (details) {
     setState(() {
       _dragAlignment += Alignment(
@@ -8,7 +10,9 @@
       );
     });
   },
-  onPanEnd: (details) {},
+  onPanEnd: (details) {
+    _runAnimation();
+  },
   child: Align(
     alignment: _dragAlignment,
     child: Card(
```

## Step 4: Calculate the velocity to simulate a springing motion

## 第四步：計算速度以模擬彈跳運動

The last step is to do a little math, to calculate the velocity of the widget
after it's finished being dragged. This is so that the widget realistically
continues at that speed before being snapped back. (The `_runAnimation` method
already sets the direction by setting the animation's start and end alignment.)

最後一步時做一些簡單的數學計算，計算小部件被拖動完成之後的速度。
這樣小部件在被快速恢復之前實際上以該速度繼續運動。
（`_runAnimation` 方法已經透過設定動畫的開始和結束對齊方式來設定方向。）

First, import the `physics` package:

首先，引入 `physics` 這個 package:

<?code-excerpt "lib/main.dart (import)"?>
```dart
import 'package:flutter/physics.dart';
```

The `onPanEnd` callback provides a [DragEndDetails][] object. This object
provides the velocity of the pointer when it stopped contacting the screen. The
velocity is in pixels per second, but the `Align` widget doesn't use pixels. It
uses coordinate values between [-1.0, -1.0] and [1.0, 1.0], where [0.0, 0.0]
represents the center. The `size` calculated in step 2 is used to convert pixels
to coordinate values in this range.

`onPanEnd` 回呼(Callback)提供了一個 [DragEndDetails][] 物件。
此物件提供指標停止接觸螢幕時的速度。速度以每秒畫素為單位，
但 `Align` widget 不使用畫素。
它使用 [-1.0，-1.0] 和 [1.0,1.0] 之間的座標值，其中 [0.0,0.0] 表示中心。
在步驟 2 中計算的 `size` 用於將畫素轉換為該範圍內的座標值。

Finally, `AnimationController` has an `animateWith()` method that can be given a
[SpringSimulation][]:

最後，`AnimationController` 有一個 `animateWith()` 
方法可以產生 [SpringSimulation][]:

<?code-excerpt "lib/main.dart (runAnimation)"?>
```dart
/// Calculates and runs a [SpringSimulation].
void _runAnimation(Offset pixelsPerSecond, Size size) {
  _animation = _controller.drive(
    AlignmentTween(
      begin: _dragAlignment,
      end: Alignment.center,
    ),
  );
  // Calculate the velocity relative to the unit interval, [0,1],
  // used by the animation controller.
  final unitsPerSecondX = pixelsPerSecond.dx / size.width;
  final unitsPerSecondY = pixelsPerSecond.dy / size.height;
  final unitsPerSecond = Offset(unitsPerSecondX, unitsPerSecondY);
  final unitVelocity = unitsPerSecond.distance;

  const spring = SpringDescription(
    mass: 30,
    stiffness: 1,
    damping: 1,
  );

  final simulation = SpringSimulation(spring, 0, 1, -unitVelocity);

  _controller.animateWith(simulation);
}
```

Don't forget to call `_runAnimation()`  with the velocity and size:

不要忘記呼叫 `_runAnimation()`，並傳入速度和大小：

<?code-excerpt "lib/main.dart (onPanEnd)"?>
```dart
onPanEnd: (details) {
  _runAnimation(details.velocity.pixelsPerSecond, size);
},
```

{{site.alert.note}}

  Now that the animation controller uses a simulation it's `duration` argument
  is no longer required.
  
  既然動畫控制器使用了模擬，就不再需要指定 `duration` 引數。
  
{{site.alert.end}}

## Interactive Example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';
import 'package:flutter/physics.dart';

void main() {
  runApp(const MaterialApp(home: PhysicsCardDragDemo()));
}

class PhysicsCardDragDemo extends StatelessWidget {
  const PhysicsCardDragDemo({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: const DraggableCard(
        child: FlutterLogo(
          size: 128,
        ),
      ),
    );
  }
}

/// A draggable card that moves back to [Alignment.center] when it's
/// released.
class DraggableCard extends StatefulWidget {
  const DraggableCard({required this.child, super.key});

  final Widget child;

  @override
  State<DraggableCard> createState() => _DraggableCardState();
}

class _DraggableCardState extends State<DraggableCard>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;

  /// The alignment of the card as it is dragged or being animated.
  ///
  /// While the card is being dragged, this value is set to the values computed
  /// in the GestureDetector onPanUpdate callback. If the animation is running,
  /// this value is set to the value of the [_animation].
  Alignment _dragAlignment = Alignment.center;

  late Animation<Alignment> _animation;

  /// Calculates and runs a [SpringSimulation].
  void _runAnimation(Offset pixelsPerSecond, Size size) {
    _animation = _controller.drive(
      AlignmentTween(
        begin: _dragAlignment,
        end: Alignment.center,
      ),
    );
    // Calculate the velocity relative to the unit interval, [0,1],
    // used by the animation controller.
    final unitsPerSecondX = pixelsPerSecond.dx / size.width;
    final unitsPerSecondY = pixelsPerSecond.dy / size.height;
    final unitsPerSecond = Offset(unitsPerSecondX, unitsPerSecondY);
    final unitVelocity = unitsPerSecond.distance;

    const spring = SpringDescription(
      mass: 30,
      stiffness: 1,
      damping: 1,
    );

    final simulation = SpringSimulation(spring, 0, 1, -unitVelocity);

    _controller.animateWith(simulation);
  }

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(vsync: this);

    _controller.addListener(() {
      setState(() {
        _dragAlignment = _animation.value;
      });
    });
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final size = MediaQuery.of(context).size;
    return GestureDetector(
      onPanDown: (details) {
        _controller.stop();
      },
      onPanUpdate: (details) {
        setState(() {
          _dragAlignment += Alignment(
            details.delta.dx / (size.width / 2),
            details.delta.dy / (size.height / 2),
          );
        });
      },
      onPanEnd: (details) {
        _runAnimation(details.velocity.pixelsPerSecond, size);
      },
      child: Align(
        alignment: _dragAlignment,
        child: Card(
          child: widget.child,
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/animation-physics-card-drag.gif" alt="Demo showing a widget being dragged and snapped back to the center" class="site-mobile-screenshot" />
</noscript>

[Align]: {{site.api}}/flutter/widgets/Align-class.html
[Alignment]: {{site.api}}/flutter/painting/Alignment-class.html
[AnimationController]: {{site.api}}/flutter/animation/AnimationController-class.html
[GestureDetector]: {{site.api}}/flutter/widgets/GestureDetector-class.html
[SingleTickerProviderStateMixin]: {{site.api}}/flutter/widgets/SingleTickerProviderStateMixin-mixin.html
[TickerProvider]: {{site.api}}/flutter/scheduler/TickerProvider-class.html
[MediaQuery]: {{site.api}}/flutter/widgets/MediaQuery-class.html
[DragEndDetails]: {{site.api}}/flutter/gestures/DragEndDetails-class.html
[SpringSimulation]: {{site.api}}/flutter/physics/SpringSimulation-class.html
