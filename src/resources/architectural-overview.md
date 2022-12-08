---
title: Flutter architectural overview
title: Flutter 架構概覽
description: A high-level overview of the architecture of Flutter, including the core principles and concepts that form its design.
tags: Flutter參考資料
keywords: Flutter原理,Flutter架構指南,Flutter分層設計
---

<?code-excerpt path-base="resources/architectural_overview/"?>

This article is intended to provide a high-level overview of the architecture of
Flutter, including the core principles and concepts that form its design.

該文章旨在提供更深入的 Flutter 架構概覽，包含其設計層面的核心原則及概念。

Flutter is a cross-platform UI toolkit that is designed to allow code reuse
across operating systems such as iOS and Android, while also allowing
applications to interface directly with underlying platform services. The goal
is to enable developers to deliver high-performance apps that feel natural on
different platforms, embracing differences where they exist while sharing as
much code as possible.

Flutter 是一個跨平臺的 UI 工具集，
它的設計初衷，就是允許在各種作業系統上覆用同樣的程式碼，例如 iOS 和 Android，
同時讓應用程式可以直接與底層平台服務進行互動。
如此設計是為了讓開發者能夠在不同的平臺上，都能交付擁有原生體驗的高效能應用，
儘可能地共享複用程式碼的同時，包容不同平台的差異。

During development, Flutter apps run in a VM that offers stateful hot reload of
changes without needing a full recompile. For release, Flutter apps are compiled
directly to machine code, whether Intel x64 or ARM instructions, or to
JavaScript if targeting the web. The framework is open source, with a permissive
BSD license, and has a thriving ecosystem of third-party packages that
supplement the core library functionality.

在開發中，Flutter 應用會在一個 VM（程式虛擬機器）中執行，
從而可以在保留狀態且無需重新編譯的情況下，熱重載相關的更新。
對於發行版 (release) ，Flutter 應用程式會直接編譯為機器程式碼
（Intel x64 或 ARM 指令集），或者針對 Web 平台的 JavaScript。
Flutter 的框架程式碼是開源的，遵循 BSD 開源協議，並擁有蓬勃發展的第三方庫生態來補充核心函式庫功能。

This overview is divided into a number of sections:

概覽分為以下幾部分內容：

1. The **layer model**: The pieces from which Flutter is constructed.

   **分層模型**：Flutter 的構成要素。

1. **Reactive user interfaces**: A core concept for Flutter user interface
   development.

   **響應式使用者介面**：Flutter 使用者介面開發的核心概念。

1. An introduction to **widgets**: The fundamental building blocks of Flutter user
   interfaces.

   **widgets** 介紹：建構 Flutter 使用者介面的基石。

1. The **rendering process**: How Flutter turns UI code into pixels.

   **渲染過程**：Flutter 如何將介面佈局轉化為畫素。

1. An overview of the **platform embedders**: The code that lets mobile and
   desktop OSes execute Flutter apps.

   **平台嵌入層** 的概覽：讓 Flutter 應用可以在移動端及桌面端作業系統執行的程式碼。

1. **Integrating Flutter with other code**: Information about different techniques
   available to Flutter apps.

   **將 Flutter 與其他程式碼進行整合**：Flutter 應用可用的各項技術的更多資訊。

1. **Support for the web**: Concluding remarks about the characteristics of
   Flutter in a browser environment.

   **Web 支援**：Flutter 在瀏覽器環境中的特性的概述。

## Architectural layers

## 架構層

Flutter is designed as an extensible, layered system. It exists as a series of
independent libraries that each depend on the underlying layer. No layer has
privileged access to the layer below, and every part of the framework level is
designed to be optional and replaceable.

Flutter 被設計為一個可擴充的分層系統。
它可以被看作是各個獨立的元件的系列合集，上層元件各自依賴下層元件。
元件無法越權存取更底層的內容，並且框架層中的各個部分都是可選且可替代的。

{% comment %}
The PNG diagrams in this document were created using draw.io. The draw.io
metadata is embedded in the PNG file itself, so you can open the PNG directly
from draw.io to edit the individual components.

The following settings were used:

 - Select all (to avoid exporting the canvas itself)
 - Export as PNG, zoom 300% (for a reasonable sized output)
 - Enable _Transparent Background_
 - Enable _Selection Only_, _Crop_
 - Enable _Include a copy of my diagram_

{% endcomment %}

![Architectural
diagram]({{site.url}}/assets/images/docs/arch-overview/archdiagram.png){:width="100%"}

To the underlying operating system, Flutter applications are packaged in the
same way as any other native application. A platform-specific embedder provides
an entrypoint; coordinates with the underlying operating system for access to
services like rendering surfaces, accessibility, and input; and manages the
message event loop. The embedder is written in a language that is appropriate
for the platform: currently Java and C++ for Android, Objective-C/Objective-C++
for iOS and macOS, and C++ for Windows and Linux. Using the embedder, Flutter
code can be integrated into an existing application as a module, or the code may
be the entire content of the application. Flutter includes a number of embedders
for common target platforms, but [other embedders also
exist](https://hover.build/blog/one-year-in/).

對於底層作業系統而言，Flutter 應用程式的包裝方式與其他原生應用相同。
在每一個平臺上，會包含一個特定的嵌入層，
從而提供一個程式入口，程式由此可以與底層作業系統進行協調，
存取諸如 surface 渲染、輔助功能和輸入等服務，並且管理事件迴圈佇列。
該嵌入層採用了適合當前平台的語言編寫，例如 Android 使用的是 Java 和 C++，
iOS 和 macOS 使用的是 Objective-C 和 Objective-C++，Windows 和 Linux 使用的是 C++。
Flutter 程式碼可以透過嵌入層，以模組方式整合到現有的應用中，也可以作為應用的主體。
Flutter 本身包含了各個常見平台的嵌入層，同時也
[存在一些其他的嵌入層](https://hover.build/blog/one-year-in/)。

At the core of Flutter is the **Flutter engine**, which is mostly written in C++
and supports the primitives necessary to support all Flutter applications. The
engine is responsible for rasterizing composited scenes whenever a new frame
needs to be painted. It provides the low-level implementation of Flutter's core
API, including graphics (through [Skia](https://skia.org/)), text layout, file
and network I/O, accessibility support, plugin architecture, and a Dart runtime
and compile toolchain.

**Flutter 引擎** 毫無疑問是 Flutter 的核心，
它主要使用 C++ 編寫，並提供了 Flutter 應用所需的原語。
當需要繪製新一幀的內容時，引擎將負責對需要合成的場景進行網格化。
它提供了 Flutter 核心 API 的底層實現，包括圖形（透過 [Skia](https://skia.org/)）、
文字佈局、檔案及網路 IO、輔助功能支援、外掛架構和 Dart 執行環境及編譯環境的工具鏈。

The engine is exposed to the Flutter framework through
[`dart:ui`]({{site.github}}/flutter/engine/tree/main/lib/ui),
which wraps the underlying C++ code in Dart classes. This library
exposes the lowest-level primitives, such as classes for driving input,
graphics, and text rendering subsystems.

引擎將底層 C++ 程式碼包裝成 Dart 程式碼，透過
[`dart:ui`]({{site.github}}/flutter/engine/tree/master/lib/ui)
暴露給 Flutter 框架層。
該庫暴露了最底層的原語，包括用於驅動輸入、圖形、和文字渲染的子系統的類別。

Typically, developers interact with Flutter through the **Flutter framework**,
which provides a modern, reactive framework written in the Dart language. It
includes a rich set of platform, layout, and foundational libraries, composed of
a series of layers. Working from the bottom to the top, we have:

通常，開發者可以透過 **Flutter 框架層** 與 Flutter 互動，
該框架提供了以 Dart 語言編寫的現代響應式框架。
它包括由一系列層組成的一組豐富的平台，佈局和基礎庫。從下層到上層，依次有：

- Basic **[foundational]({{site.api}}/flutter/foundation/foundation-library.html)**
  classes, and building block services such as
  **[animation]({{site.api}}/flutter/animation/animation-library.html),
  [painting]({{site.api}}/flutter/painting/painting-library.html), and
  [gestures]({{site.api}}/flutter/gestures/gestures-library.html)** that offer
  commonly used abstractions over the underlying foundation.

  基礎的 **[foundational]({{site.api}}/flutter/foundation/foundation-library.html)**
  類及一些基層之上的建構塊服務，如 **[animation]({{site.api}}/flutter/animation/animation-library.html)、
  [painting]({{site.api}}/flutter/painting/painting-library.html) 和
  [gestures]({{site.api}}/flutter/gestures/gestures-library.html)**，
 它們可以提供上層常用的抽象。

- The **[rendering
  layer]({{site.api}}/flutter/rendering/rendering-library.html)** provides an
  abstraction for dealing with layout. With this layer, you can build a tree of
  renderable objects. You can manipulate these objects dynamically, with the
  tree automatically updating the layout to reflect your changes.

  **[渲染層]({{site.api}}/flutter/rendering/rendering-library.html)**
  用於提供操作佈局的抽象。有了渲染層，你可以建構一棵可渲染物件的樹。
  在你動態更新這些物件時，渲染樹也會自動根據你的變更來更新佈局。

- The **[widgets layer]({{site.api}}/flutter/widgets/widgets-library.html)** is
  a composition abstraction. Each render object in the rendering layer has a
  corresponding class in the widgets layer. In addition, the widgets layer
  allows you to define combinations of classes that you can reuse. This is the
  layer at which the reactive programming model is introduced.

  **[widget 層]({{site.api}}/flutter/widgets/widgets-library.html)**
  是一種組合的抽象。每一個渲染層中的渲染物件，都在 widgets 層中有一個對應的類別。
  此外，widgets 層讓你可以自由組合你需要複用的各種類別。
  響應式程式設計模型就在該層級中被引入。

- The
  **[Material]({{site.api}}/flutter/material/material-library.html)**
  and
  **[Cupertino]({{site.api}}/flutter/cupertino/cupertino-library.html)**
  libraries offer comprehensive sets of controls that use the widget layer's
  composition primitives to implement the Material or iOS design languages.

  **[Material]({{site.api}}/flutter/material/material-library.html)** 和
  **[Cupertino]({{site.api}}/flutter/cupertino/cupertino-library.html)** 庫
  提供了全面的 widgets 層的原語組合，這套組合分別實現了 Material 和 iOS 設計規範。

The Flutter framework is relatively small; many higher-level features that
developers might use are implemented as packages, including platform plugins
like [camera]({{site.pub}}/packages/camera) and
[webview]({{site.pub}}/packages/webview_flutter), as well as platform-agnostic
features like [characters]({{site.pub}}/packages/characters),
[http]({{site.pub}}/packages/http), and
[animations]({{site.pub}}/packages/animations) that build upon the core Dart and
Flutter libraries. Some of these packages come from the broader ecosystem,
covering services like [in-app
payments]({{site.pub}}/packages/square_in_app_payments), [Apple
authentication]({{site.pub}}/packages/sign_in_with_apple), and
[animations]({{site.pub}}/packages/lottie).

Flutter 框架相對較小，因為一些開發者可能會使用到的更高層級的功能
已經被拆分到不同的軟體套件中，使用 Dart 和 Flutter 的核心庫實現，
其中包括平台外掛，例如
[camera]({{site.pub}}/packages/camera) 和
[webview]({{site.pub}}/packages/webview_flutter)；與平台無關的功能，例如
[characters]({{site.pub}}/packages/characters)、
[http]({{site.pub}}/packages/http) 和
[animations]({{site.pub}}/packages/animations)。
還有一些軟體包來自於更為寬泛的生態系統中，例如
[應用內支付]({{site.pub}}/packages/square_in_app_payments)、
[Apple 認證]({{site.pub}}/packages/sign_in_with_apple) 和
[Lottie 動畫]({{site.pub}}/packages/lottie)。

The rest of this overview broadly navigates down the layers, starting with the
reactive paradigm of UI development. Then, we describe how widgets are composed
together and converted into objects that can be rendered as part of an
application. We describe how Flutter interoperates with other code at a platform
level, before giving a brief summary of how Flutter’s web support differs from
other targets.

該概覽的其餘部分將從 UI 開發的響應式範例開始，瀏覽各個建構層。
而後，我們會講述 widgets 如何被組織，並轉換成應用程式的渲染物件。
同時我們也會講述 Flutter 如何在平台層面與其他程式碼進行互動，
最終，我們會對目前 Flutter 對於 Web 平台的支援與其他平台的異同做一個總結。

## Reactive user interfaces

## 響應式使用者介面

On the surface, Flutter is [a reactive, pseudo-declarative UI
framework]({{site.url}}/resources/faq#what-programming-paradigm-does-flutters-framework-use),
in which the developer provides a mapping from application state to interface
state, and the framework takes on the task of updating the interface at runtime
when the application state changes. This model is inspired by [work that came
from Facebook for their own React
framework]({{site.youtube-site}}/watch?time_continue=2&v=x7cQ3mrcKaY&feature=emb_logo),
which includes a rethinking of many traditional design principles.

粗略一看，Flutter 是
[一個響應式的且偽宣告式的 UI 框架]({{site.url}}/resources/faq#what-programming-paradigm-does-flutters-framework-use)，
開發者負責提供應用狀態與介面狀態之間的對映，框架則在執行時將應用狀態的更改更新到介面上。
這樣的模型架構的靈感來自
[Facebook 自己的 React 框架]({{site.youtube-site}}/watch?time_continue=2&v=x7cQ3mrcKaY&feature=emb_logo)
，其中包含了對傳統設計理念的再度解構。

In most traditional UI frameworks, the user interface's initial state is
described once and then separately updated by user code at runtime, in response
to events. One challenge of this approach is that, as the application grows in
complexity, the developer needs to be aware of how state changes cascade
throughout the entire UI. For example, consider the following UI:

在大部分傳統的 UI 框架中，介面的初始狀態通常會被一次性定義，
然後，在執行時根據使用者程式碼分別響應事件進行更新。
在這裡有一項大挑戰，即隨著應用程式的複雜性日益增長，
開發者需要對整個 UI 的狀態關聯有整體的認知。
讓我們來看看如下的 UI：

![Color picker dialog]({{site.url}}/assets/images/docs/arch-overview/color-picker.png){:width="66%"}

There are many places where the state can be changed: the color box, the hue
slider, the radio buttons. As the user interacts with the UI, changes must be
reflected in every other place. Worse, unless care is taken, a minor change to
one part of the user interface can cause ripple effects to seemingly unrelated
pieces of code.

很多地方都可以更改狀態：顏色框、色調滑條、單選按鈕。
在使用者與 UI 進行互動時，狀態的改變可能會影響到每一個位置。
更糟糕的是，UI 的細微變動很有可能會引發無關程式碼的連鎖反應，尤其是當開發者並未注意其關聯的時候。

One solution to this is an approach like MVC, where you push data changes to the
model via the controller, and then the model pushes the new state to the view
via the controller. However, this also is problematic, since creating and
updating UI elements are two separate steps that can easily get out of sync.

我們可以透過類似 MVC 的方式進行處理，
開發者將資料的改動透過控制器（Controller）推至模型（Model），
模型再將新的狀態透過控制器推至介面（View）。
但這樣的處理方式仍然存在問題，
因為建立和更新 UI 元素的操作被分離開了，容易造成它們的不同步。

Flutter, along with other reactive frameworks, takes an alternative approach to
this problem, by explicitly decoupling the user interface from its underlying
state. With React-style APIs, you only create the UI description, and the
framework takes care of using that one configuration to both create and/or
update the user interface as appropriate.

Flutter 與其他響應式框架類似，採用了顯式剝離基礎狀態和使用者介面的方式，來解決這一問題。
你可以透過 React 風格的 API，建立 UI 的描述，讓框架負責透過配置優雅地建立和更新使用者介面。

In Flutter, widgets (akin to components in React) are represented by immutable
classes that are used to configure a tree of objects. These widgets are used to
manage a separate tree of objects for layout, which is then used to manage a
separate tree of objects for compositing. Flutter is, at its core, a series of
mechanisms for efficiently walking the modified parts of trees, converting trees
of objects into lower-level trees of objects, and propagating changes across
these trees.

在 Flutter 裡，widgets（類似於 React 中的元件）是用來配置物件樹的不可變類別。
這些 widgets 會管理單獨的佈局物件樹，接著參與管理合成的佈局物件樹。
Flutter 的核心就是一套高效的遍歷樹的變動的機制，
它會將物件樹轉換為更底層的物件樹，並在樹與樹之間傳遞更改。

A widget declares its user interface by overriding the `build()` method, which
is a function that converts state to UI:

`build()` 是將狀態轉化為 UI 的方法，widget 透過重寫該方法來宣告 UI 的構造：

```none
UI = f(state)
```

The `build()` method is by design fast to execute and should be free of side
effects, allowing it to be called by the framework whenever needed (potentially
as often as once per rendered frame).

`build()` 方法在框架需要時都可以被呼叫（每個渲染幀可能會呼叫一次），
從設計角度來看，它應當能夠快速執行且沒有額外影響的。

This approach relies on certain characteristics of a language runtime (in
particular, fast object instantiation and deletion). Fortunately, [Dart is
particularly well suited for this
task]({{site.flutter-medium}}/flutter-dont-fear-the-garbage-collector-d69b3ff1ca30).

這樣的實現設計依賴於語言的執行時特徵（特別是物件的快速例項化和清除）。
幸運的是，[Dart 非常適合這份工作](https://medium.com/flutter/flutter-dont-fear-the-garbage-collector-d69b3ff1ca30)。

## Widgets

As mentioned, Flutter emphasizes widgets as a unit of composition. Widgets are
the building blocks of a Flutter app’s user interface, and each widget is an
immutable declaration of part of the user interface.

如前所述，Flutter 強調以 widgets 作為組成單位。
Widgets 是建構 Flutter 應用介面的基礎塊，每個 widget 都是一部分不可變的 UI 宣告。

Widgets form a hierarchy based on composition. Each widget nests inside its
parent and can receive context from the parent. This structure carries all the
way up to the root widget (the container that hosts the Flutter app, typically
`MaterialApp` or `CupertinoApp`), as this trivial example shows:

Widgets 透過佈局組合形成一種層次結構關係。
每個 Widget 都巢狀(Nesting)在其父級的內部，並可以透過父級接收上下文。
從根佈局（託管 Flutter 應用的容器，通常是 `MaterialApp` 或 `CupertinoApp`）開始，
自上而下都是這樣的結構，如下面的範例所示：

<?code-excerpt "lib/main.dart (Main)"?>
```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('My Home Page'),
        ),
        body: Center(
          child: Builder(
            builder: (context) {
              return Column(
                children: [
                  const Text('Hello World'),
                  const SizedBox(height: 20),
                  ElevatedButton(
                    onPressed: () {
                      print('Click!');
                    },
                    child: const Text('A button'),
                  ),
                ],
              );
            },
          ),
        ),
      ),
    );
  }
}
```

In the preceding code, all instantiated classes are widgets.

在上面的程式碼中，所有例項化的類都是 widgets。

Apps update their user interface in response to events (such as a user
interaction) by telling the framework to replace a widget in the hierarchy with
another widget. The framework then compares the new and old widgets, and
efficiently updates the user interface.

應用會根據事件互動（例如使用者操作），通知框架替換層級中的舊 widget 為新 widget，
然後框架會比較新舊 widgets，高效地更新使用者介面。

Flutter has its own implementations of each UI control, rather than deferring to
those provided by the system: for example, there is a pure [Dart
implementation]({{site.api}}/flutter/cupertino/CupertinoSwitch-class.html) of both the
[iOS Switch
control]({{site.apple-dev}}/design/human-interface-guidelines/ios/controls/switches/)
and the [one for]({{site.api}}/flutter/material/Switch-class.html) the
[Android equivalent]({{site.material}}/develop/android/components/switches).

Flutter 擁有其自己的 UI 控制實現，而不是由系統自帶的方法進行託管：
例如，
[iOS 的 Switch 控制項](https://developer.apple.com/design/human-interface-guidelines/ios/controls/switches/)
和 [Android 的選擇控制項](https://material.io/develop/android/components/switches)
均有一個純 [Dart 實現]({{site.api}}/flutter/material/Switch-class.html)。

This approach provides several benefits:

這樣的實現有幾個優勢：

- Provides for unlimited extensibility. A developer who wants a variant of the
  Switch control can create one in any arbitrary way, and is not limited to the
  extension points provided by the OS.

  提供了無限的擴充性。當開發者想要一個 Switch 的改裝時，他們可以以任意方式建立一個，
  而不被系統提供的擴充所限制。

- Avoids a significant performance bottleneck by allowing Flutter to composite
  the entire scene at once, without transitioning back and forth between Flutter
  code and platform code.

  Flutter 可以直接合成所有的場景，而無需在 Flutter 與原生平台之間來回切換，
  從而避免了明顯的效能瓶頸。

- Decouples the application behavior from any operating system dependencies. The
  application looks and feels the same on all versions of the OS, even if the OS
  changed the implementations of its controls.

  將應用的行為與作業系統的依賴解耦。
  在任意一種系統平臺上體驗應用，都將是一致的，就算某個系統更改了其控制項的實現，也是如此。

### Composition

### 組成

Widgets are typically composed of many other small, single-purpose widgets that
combine to produce powerful effects.

Widgets 通常由更小的且用途單一的 widgets 組合而成，提供更強大的功能。

Where possible, the number of design concepts is kept to a minimum while
allowing the total vocabulary to be large. For example, in the widgets layer,
Flutter uses the same core concept (a `Widget`) to represent drawing to the
screen, layout (positioning and sizing), user interactivity, state management,
theming, animations, and navigation. In the animation layer, a pair of concepts,
`Animation`s and `Tween`s, cover most of the design space. In the rendering
layer, `RenderObject`s are used to describe layout, painting, hit testing, and
accessibility. In each of these cases, the corresponding vocabulary ends up
being large: there are hundreds of widgets and render objects, and dozens of
animation and tween types.

在設計時，相關的設計概念已儘可能地少量存在，而透過大量的內容進行填充。
（譯者注：即以最小的原語加最多的單一實現創造出最大的價值。）
舉個例子，Flutter 在 widgets 層中使用了相同的概念（一個 `Widget`）
來表示螢幕上的繪製、佈局（位置和大小）、使用者互動、狀態管理、主題、動畫及導航。
在動畫層，`Animation` 和 `Tween` 這對概念組合，涵蓋了大部分的設計空間。
在渲染層，`RenderObject` 用來描述佈局、繪製、觸控判斷及可及性。
在這些場景中，最終對應包含的內容都很多：
有數百個 widgets 和 render objects，以及數十種動畫和補間型別。

The class hierarchy is deliberately shallow and broad to maximize the possible
number of combinations, focusing on small, composable widgets that each do one
thing well. Core features are abstract, with even basic features like padding
and alignment being implemented as separate components rather than being built
into the core. (This also contrasts with more traditional APIs where features
like padding are built in to the common core of every layout component.) So, for
example, to center a widget, rather than adjusting a notional `Align` property,
you wrap it in a [`Center`]({{site.api}}/flutter/widgets/Center-class.html)
widget.

類別的層次結構是有意的淺而廣，以最大限度地增加可能的組合數量，
重點放在小的、可組合的 widget 上，確保每個 widget 都能很好地完成一件事情。
核心功能均被抽象，甚至像邊距和對齊這樣的基礎功能，都被實現為單獨的元件，而不是內置於核心中。
（這樣的實現也與傳統的 API 形成了對比，類似邊距這樣的功能通常都內建在了每個元件的公共核心內，
Flutter 中的 widget 則不同。）因此，如果你需要將一個 widget 居中，
與其調整 `Align` 這樣的屬性，不如將它包裹在一個
[`Center`]({{site.api}}/flutter/widgets/Center-class.html) widget 內。

There are widgets for padding, alignment, rows, columns, and grids. These layout
widgets do not have a visual representation of their own. Instead, their sole
purpose is to control some aspect of another widget’s layout. Flutter also
includes utility widgets that take advantage of this compositional approach.

Flutter 中包含了邊距、對齊、行、列和網格系列的 widgets。
這些佈局型別的 widgets 自身沒有視覺內容，
而只用於控制其他 widgets 的部分佈局條件。
Flutter 也包含了以這種組合方法組成的實用型 widgets。

For example, [`Container`]({{site.api}}/flutter/widgets/Container-class.html), a
commonly used widget, is made up of several widgets responsible for layout,
painting, positioning, and sizing. Specifically, Container is made up of the
[`LimitedBox`]({{site.api}}/flutter/widgets/LimitedBox-class.html),
[`ConstrainedBox`]({{site.api}}/flutter/widgets/ConstrainedBox-class.html),
[`Align`]({{site.api}}/flutter/widgets/Align-class.html),
[`Padding`]({{site.api}}/flutter/widgets/Padding-class.html),
[`DecoratedBox`]({{site.api}}/flutter/widgets/DecoratedBox-class.html), and
[`Transform`]({{site.api}}/flutter/widgets/Transform-class.html) widgets, as you
can see by reading its source code. A defining characteristic of Flutter is that
you can drill down into the source for any widget and examine it. So, rather
than subclassing `Container` to produce a customized effect, you can compose it
and other widgets in novel ways, or just create a new widget using
`Container` as inspiration.

例如，一個常用的 widget
[`Container`]({{site.api}}/flutter/widgets/Container-class.html)，
是由幾個 widget 組合而成，包含了佈局、繪製、定位和大小的功能。
更具體地說，Container 是由
[`LimitedBox`]({{site.api}}/flutter/widgets/LimitedBox-class.html)、
[`ConstrainedBox`]({{site.api}}/flutter/widgets/ConstrainedBox-class.html)、
[`Align`]({{site.api}}/flutter/widgets/Align-class.html)、
[`Padding`]({{site.api}}/flutter/widgets/Padding-class.html)、
[`DecoratedBox`]({{site.api}}/flutter/widgets/DecoratedBox-class.html) 和
[`Transform`]({{site.api}}/flutter/widgets/Transform-class.html) 組成的，
你也可以透過檢視原始碼看到這些組合。
Flutter 有一個典型的特徵，即你可以深入到任意一個 widget，檢視其原始碼。
因此，你可以透過同樣的方式組合其他的 widgets，也可以參考 `Container` 來建立其他的 widget，
而不需要繼承 `Container` 來實現自訂的效果。

### Building widgets

### 建構 widgets

As mentioned earlier, you determine the visual representation of a widget by
overriding the
[`build()`]({{site.api}}/flutter/widgets/StatelessWidget/build.html) function to
return a new element tree. This tree represents the widget’s part of the user
interface in more concrete terms. For example, a toolbar widget might have a
build function that returns a [horizontal
layout]({{site.api}}/flutter/widgets/Row-class.html) of some
[text]({{site.api}}/flutter/widgets/Text-class.html) and
[various]({{site.api}}/flutter/material/IconButton-class.html)
[buttons]({{site.api}}/flutter/material/PopupMenuButton-class.html). As needed,
the framework recursively asks each widget to build until the tree is entirely
described by [concrete renderable
objects]({{site.api}}/flutter/widgets/RenderObjectWidget-class.html). The
framework then stitches together the renderable objects into a renderable object
tree.

先前提到，你可以透過重寫
[`build()`]({{site.api}}/flutter/widgets/StatelessWidget/build.html)
方法，返回一個新的元素樹，來定義視覺展示。
這棵樹用更為具體的術語表示了 widget 在 UI 中的部分。
例如，工具欄 widget 的 build 方法可能會返回
[水平佈局]({{site.api}}/flutter/widgets/Row-class.html)，
其中可能包含一些 [文字]({{site.api}}/flutter/widgets/Text-class.html)，
[各種各樣]({{site.api}}/flutter/material/IconButton-class.html) 的
[按鈕]({{site.api}}/flutter/material/PopupMenuButton-class.html)。
根據需要，框架會遞迴請求每個 widget 進行建構，直到整棵樹都被
[具體的可渲染物件]({{site.api}}/flutter/widgets/RenderObjectWidget-class.html)
描述為止。
然後，框架會將可渲染的物件縫合在一起，組成可渲染物件樹。

A widget’s build function should be free of side effects. Whenever the function
is asked to build, the widget should return a new tree of widgets<sup><a
href="#a1">1</a></sup>, regardless of what the widget previously returned. The
framework does the heavy lifting work to determine which build methods need to
be called based on the render object tree (described in more detail later). More
information about this process can be found in the [Inside Flutter
topic]({{site.url}}/resources/inside-flutter#linear-reconciliation).

Widget 的 build 方法應該是沒有副作用的。每當一個方法要求建構時，
widget 都應當能返回一個 widget 的元素樹<sup><a href="#a1">1</a></sup>，
與先前返回的 widget 也沒有關聯。
框架會根據渲染物件樹（稍後將進一步介紹）來確定哪些建構方法需要被呼叫，這是一項略顯繁重的工作。
有關這個過程的更多資訊，可以在
[Flutter 工作原理]({{site.url}}/resources/inside-flutter#linear-reconciliation)
中進一步瞭解。

On each rendered frame, Flutter can recreate just the parts of the UI where the
state has changed by calling that widget’s `build()` method. Therefore it is
important that build methods should return quickly, and heavy computational work
should be done in some asynchronous manner and then stored as part of the state
to be used by a build method.

每個渲染幀，Flutter 都可以根據變化的狀態，呼叫 `build()` 方法重建部分 UI。
因此，保證 build 方法輕量且能快速返回 widget 是非常關鍵的，
繁重的計算工作應該透過一些非同步方法完成，並存儲在狀態中，在 build 方法中使用。

While relatively naïve in approach, this automated comparison is quite
effective, enabling high-performance, interactive apps. And, the design of the
build function simplifies your code by focusing on declaring what a widget is
made of, rather than the complexities of updating the user interface from one
state to another.

儘管這樣的實現看起來不夠成熟，但這樣的自動對比方法非常有效，可以實現高效能的互動應用。
同時，以這種方式設計的 build 方法，將著重點放在 widget 組成的宣告上，從而簡化了你的程式碼，
而不是以一種狀態去更新另一種狀態這樣的複雜過程。

### Widget state

### Widget 的狀態

The framework introduces two major classes of widget: _stateful_ and _stateless_
widgets.

框架包含兩種核心的 widget 類：**有狀態的** 和 **無狀態的** widget。

Many widgets have no mutable state: they don’t have any properties that change
over time (for example, an icon or a label). These widgets subclass
[`StatelessWidget`]({{site.api}}/flutter/widgets/StatelessWidget-class.html).

大部分 widget 都沒有需要變更的狀態：它們並不包含隨時變化的屬性（例如圖示或者標籤）。
這些 widget 會繼承
[`StatelessWidget`]({{site.api}}/flutter/widgets/StatelessWidget-class.html)。

However, if the unique characteristics of a widget need to change based on user
interaction or other factors, that widget is _stateful_. For example, if a
widget has a counter that increments whenever the user taps a button, then the
value of the counter is the state for that widget. When that value changes, the
widget needs to be rebuilt to update its part of the UI. These widgets subclass
[`StatefulWidget`]({{site.api}}/flutter/widgets/StatefulWidget-class.html), and
(because the widget itself is immutable) they store mutable state in a separate
class that subclasses [`State`]({{site.api}}/flutter/widgets/State-class.html).
`StatefulWidget`s don’t have a build method; instead, their user interface is
built through their `State` object.

然而，當 widget 擁有需要根據使用者互動或其他因素而變化的特有屬性，它就是 **有狀態的**。
例如，計數器 widget 在使用者點選按鈕時數字遞增，那麼計數值就是計數器 widget 的狀態。
當值變化時，widget 則需要被重建以更新相關部分的 UI。
這些 widget 會繼承
[`StatefulWidget`]({{site.api}}/flutter/widgets/StatefulWidget-class.html)，
並且「可變的」狀態會儲存在繼承
[`State`]({{site.api}}/flutter/widgets/State-class.html) 的另一個子類中
（因為 widget 本身是不可變的）。
`StatefulWidget` 自身沒有 build 方法，而在其對應的 `State` 物件中。

Whenever you mutate a `State` object (for example, by incrementing the counter),
you must call [`setState()`]({{site.api}}/flutter/widgets/State/setState.html)
to signal the framework to update the user interface by calling the `State`’s
build method again.

每當你更改 `State` 物件時（例如計數增加），你需要呼叫
[`setState()`]({{site.api}}/flutter/widgets/State/setState.html)
來告知框架，再次呼叫 `State` 的建構方法來更新 UI。

Having separate state and widget objects lets other widgets treat both stateless
and stateful widgets in exactly the same way, without being concerned about
losing state. Instead of needing to hold on to a child to preserve its state,
the parent can create a new instance of the child at any time without losing the
child’s persistent state. The framework does all the work of finding and reusing
existing state objects when appropriate.

將狀態和 widget 物件分離，可以使其他 widget 無差異地看待無狀態和有狀態 widget，
而不必擔心丟失狀態。父級無需擔心狀態的丟失，可以隨時建立新的例項，
並不需要透過子級關係保持其狀態。
框架也會在合適的時間，複用已存在的狀態物件。

### State management Available

### 狀態管理

So, if many widgets can contain state, how is state managed and passed around
the system?

那麼，在眾多 widget 都持有狀態的情況下，系統中的狀態是如何被傳遞和管理的呢？

As with any other class, you can use a constructor in a widget to initialize its
data, so a `build()` method can ensure that any child widget is instantiated
with the data it needs:

與其他類相同，你可以透過 widget 的建構函式來初始化資料，
如此一來 `build()` 方法可以確保子 widget 使用其所需的資料進行例項化：

<!-- skip -->
```dart
@override
Widget build(BuildContext context) {
   return ContentWidget(importantState);
}
```

As widget trees get deeper, however, passing state information up and down the
tree hierarchy becomes cumbersome. So, a third widget type,
[`InheritedWidget`]({{site.api}}/flutter/widgets/InheritedWidget-class.html),
provides an easy way to grab data from a shared ancestor. You can use
`InheritedWidget` to create a state widget that wraps a common ancestor in the
widget tree, as shown in this example:

然而，隨著 widget 樹層級逐漸加深，依賴樹形結構上下傳遞狀態資訊會變得十分麻煩。
這時，第三種類型的 widget&mdash;&mdash;
[`InheritedWidget`]({{site.api}}/flutter/widgets/InheritedWidget-class.html)，
提供了一種從共同的祖先節點獲取資料的簡易方法。
你可以使用 `InheritedWidget` 建立包含狀態的 widget，
該 widget 會將一個共同的祖先節點包裹在 widget 樹中，如下面的例子所示：

![Inherited widgets]({{site.url}}/assets/images/docs/arch-overview/inherited-widget.png){:width="50%"}

Whenever one of the `ExamWidget` or `GradeWidget` objects needs data from
`StudentState`, it can now access it with a command such as:

現在，當 `ExamWidget` 或 `GradeWidget` 物件需要獲取 `StudentState` 的資料時，
可以直接使用以下方式：

<!-- skip -->
```dart
final studentState = StudentState.of(context);
```

The `of(context)` call takes the build context (a handle to the current widget
location), and returns [the nearest ancestor in the
tree]({{site.api}}/flutter/widgets/BuildContext/dependOnInheritedWidgetOfExactType.html)
that matches the `StudentState` type. `InheritedWidget`s also offer an
`updateShouldNotify()` method, which Flutter calls to determine whether a state
change should trigger a rebuild of child widgets that use it.

呼叫 `of(context)` 會根據當前建構的上下文（即當前 widget 位置的控制代碼），
並返回型別為 `StudentState` 的
[在樹中距離最近的祖先節點]({{site.api}}/flutter/flutter/widgets/BuildContext/dependOnInheritedWidgetOfExactType.html)。
`InheritedWidget` 同時也包含了 `updateShouldNotify()` 方法，
Flutter 會呼叫它來判斷依賴了某個狀態的 widget 是否需要重建。

Flutter itself uses `InheritedWidget` extensively as part of the framework for
shared state, such as the application’s _visual theme_, which includes
[properties like color and type
styles]({{site.api}}/flutter/material/ThemeData-class.html) that are
pervasive throughout an application. The `MaterialApp` `build()` method inserts
a theme in the tree when it builds, and then deeper in the hierarchy a widget
can use the `.of()` method to look up the relevant theme data, for example:

`InheritedWidget` 在 Flutter 框架中被大量用於共享狀態，例如應用的 **視覺主題**，
包含了應用於整個應用的
[顏色和字型樣式等屬性]({{site.api}}/flutter/material/ThemeData-class.html)。
`MaterialApp` 的 `build()` 方法會在建構時在樹中插入一個主題，
更深層級的 widget 便可以使用 `.of()` 方法來查詢相關的主題資料，例如：

<?code-excerpt "lib/main.dart (Container)"?>
```dart
Container(
  color: Theme.of(context).secondaryHeaderColor,
  child: Text(
    'Text with a background color',
    style: Theme.of(context).textTheme.headline6,
  ),
);
```

This approach is also used for
[Navigator]({{site.api}}/flutter/widgets/Navigator-class.html), which provides
page routing; and
[MediaQuery]({{site.api}}/flutter/widgets/MediaQuery-class.html), which provides
access to screen metrics such as orientation, dimensions, and brightness.

類似地，以該方法實現的還有
提供了頁面路由的 [Navigator]({{site.api}}/flutter/widgets/Navigator-class.html)、
提供了螢幕資訊指標，包括方向、尺寸和亮度的 [MediaQuery]({{site.api}}/flutter/widgets/MediaQuery-class.html) 等。

As applications grow, more advanced state management approaches that reduce the
ceremony of creating and using stateful widgets become more attractive. Many
Flutter apps use utility packages like
[provider]({{site.pub}}/packages/provider), which provides a wrapper around
`InheritedWidget`. Flutter’s layered architecture also enables alternative
approaches to implement the transformation of state into UI, such as the
[flutter_hooks]({{site.pub}}/packages/flutter_hooks) package.

隨著應用程式的不斷迭代，更進階的狀態管理方法變得更有吸引力，
它們可以減少有狀態的 widget 的建立。
許多 Flutter 應用使用了 [provider]({{site.pub}}/packages/provider) 用於狀態管理，
它對 `InheritedWidget` 進行了進一步的包裝。
Flutter 的分層架構也允許使用其他實現來替換狀態至 UI 的方案，例如
[flutter_hooks]({{site.pub}}/packages/flutter_hooks)。

## Rendering and layout

## 渲染和佈局

This section describes the rendering pipeline, which is the series of steps that
Flutter takes to convert a hierarchy of widgets into the actual pixels painted
onto a screen.

本節介紹 Flutter 的渲染機制，
包括將 widget 層級結構轉換成螢幕上繪製的實際畫素的一系列步驟。

### Flutter’s rendering model

### Flutter 的渲染模型

You may be wondering: if Flutter is a cross-platform framework, then how can it
offer comparable performance to single-platform frameworks?

你可能思考過：既然 Flutter 是一個跨平臺的框架，那麼它如何提供與原生平台框架相當的效能？

It’s useful to start by thinking about how traditional Android apps work. When
drawing, you first call the Java code of the Android framework. The Android
system libraries provide the components responsible for drawing themselves to a
Canvas object, which Android can then render with [Skia](https://skia.org/), a
graphics engine written in C/C++ that calls the CPU or GPU to complete the
drawing on the device.

讓我們從安卓原生應用的角度開始思考。
當你在編寫繪製的內容時，你需要呼叫 Android 框架的 Java 程式碼。
Android 的系統庫提供了可以將自身繪製到 Canvas 物件的元件，
接下來 Android 就可以使用由 C/C++ 編寫的 [Skia](https://skia.org/) 影象引擎，
呼叫 CPU 和 GPU 完成在裝置上的繪製。

Cross-platform frameworks _typically_ work by creating an abstraction layer over
the underlying native Android and iOS UI libraries, attempting to smooth out the
inconsistencies of each platform representation. App code is often written in an
interpreted language like JavaScript, which must in turn interact with the
Java-based Android or Objective-C-based iOS system libraries to display UI. All
this adds overhead that can be significant, particularly where there is a lot of
interaction between the UI and the app logic.

**通常來說**，跨平臺框架都會在 Android 和 iOS 的 UI 底層庫上建立一層抽象，
該抽象層嘗試抹平各個系統之間的差異。
這時，應用程式的程式碼常常使用 JavaScript 等解釋型語言來進行編寫，
這些程式碼會與基於 Java 的 Android 和基於 Objective-C 的 iOS 系統進行互動，
最終顯示 UI 介面。
所有的流程都增加了顯著的開銷，在 UI 和應用邏輯有繁雜的互動時更為如此。

By contrast, Flutter minimizes those abstractions, bypassing the system UI
widget libraries in favor of its own widget set. The Dart code that paints
Flutter’s visuals is compiled into native code, which uses Skia for rendering.
Flutter also embeds its own copy of Skia as part of the engine, allowing the
developer to upgrade their app to stay updated with the latest performance
improvements even if the phone hasn’t been updated with a new Android version.
The same is true for Flutter on other native platforms, such as iOS, Windows, or
macOS.

相比之下，Flutter 透過繞過系統 UI 元件庫，使用自己的 widget 內容集，削減了抽象層的開銷。
用於繪製 Flutter 影象內容的 Dart 程式碼被編譯為機器碼，並使用 Skia 進行渲染。
Flutter 同時也嵌入了自己的 Skia 副本，
讓開發者能在裝置未更新到最新的系統時，
也能跟進升級自己的應用，保證穩定性並提升效能。

### From user input to the GPU

### 從使用者操作到 GPU

The overriding principle that Flutter applies to its rendering pipeline is that
**simple is fast**. Flutter has a straightforward pipeline for how data flows to
the system, as shown in the following sequencing diagram:

對於 Flutter 的渲染機制而言，首要原則是 **簡單快速**。
Flutter 為資料流向系統提供了直通的管道，如以下的流程圖所示：

![Render pipeline sequencing
diagram]({{site.url}}/assets/images/docs/arch-overview/render-pipeline.png){:width="100%"}

Let’s take a look at some of these phases in greater detail.

接下來，讓我們更加深入瞭解其中的一些階段。

### Build: from Widget to Element

### 建構：從 Widget 到 Element

Consider this code fragment that demonstrates a widget hierarchy:

首先觀察以下的程式碼片段，它代表了一個簡單的 widget 層次結構：

<?code-excerpt "lib/main.dart (Container2)"?>
```dart
Container(
  color: Colors.blue,
  child: Row(
    children: [
      Image.network('https://www.example.com/1.png'),
      const Text('A'),
    ],
  ),
);
```

When Flutter needs to render this fragment, it calls the `build()` method, which
returns a subtree of widgets that renders UI based on the current app state.
During this process, the `build()` method can introduce new widgets, as
necessary, based on its state. As an example, in the preceding code
fragment, `Container` has `color` and `child` properties. From looking at the
[source
code]({{site.repo.flutter}}/blob/02efffc134ab4ce4ff50a9ddd86c832efdb80462/packages/flutter/lib/src/widgets/container.dart#L401)
for `Container`, you can see that if the color is not null, it inserts a
`ColoredBox` representing the color:

當 Flutter 需要繪製這段程式碼片段時，框架會呼叫 `build()` 方法，
返回一棵基於當前應用狀態來繪製 UI 的 widget 子樹。
在這個過程中，`build()` 方法可能會在必要時，根據狀態引入新的 widget。
在上面的例子中，`Container` 的 `color` 和 `child` 就是典型的例子。
我們可以檢視 `Container` 的
[原始碼]({{site.repo.flutter}}/blob/02efffc134ab4ce4ff50a9ddd86c832efdb80462/packages/flutter/lib/src/widgets/container.dart#L401)，
你會看到當 `color` 屬性不為空時，`ColoredBox` 會被加入用於顏色佈局。

<!-- skip -->
```dart
if (color != null)
  current = ColoredBox(color: color!, child: current);
```

Correspondingly, the `Image` and `Text` widgets might insert child widgets such
as `RawImage` and `RichText` during the build process. The eventual widget
hierarchy may therefore be deeper than what the code represents, as in this
case<sup><a href="#a2">2</a></sup>:

與之對應的，`Image` 和 `Text` 在建構過程中也會引入 `RawImage` 和 `RichText`。
如此一來，最終產生的 widget 結構比程式碼表示的層級更深，
在該場景中如下圖<sup><a href="#a2">2</a></sup>：

![Render pipeline sequencing
diagram]({{site.url}}/assets/images/docs/arch-overview/widgets.png){:width="35%"}

This explains why, when you examine the tree through a debug tool such as the
[Flutter inspector]({{site.url}}/development/tools/devtools/inspector), part of the
Dart DevTools, you might see a structure that is considerably deeper than what
is in your original code.

這就是為什麼你在使用 Dart DevTools 的
[Flutter inspector]({{site.url}}/development/tools/devtools/inspector)
除錯 widget 樹結構時，會發現實際的結構比你原本程式碼中的結構層級更深。

During the build phase, Flutter translates the widgets expressed in code into a
corresponding **element tree**, with one element for every widget. Each element
represents a specific instance of a widget in a given location of the tree
hierarchy. There are two basic types of elements:

在建構的階段，Flutter 會將程式碼中描述的 widgets 轉換成對應的 **Element 樹**，
每一個 Widget 都有一個對應的 Element。
每一個 Element 代表了樹狀層級結構中特定位置的 widget 例項。
目前有兩種 Element 的基本型別：

- `ComponentElement`, a host for other elements.

  `ComponentElement`，其他 Element 的宿主。

- `RenderObjectElement`, an element that participates in the layout or paint
  phases.

  `RenderObjectElement`，參與佈局或繪製階段的 Element。

![Render pipeline sequencing
diagram]({{site.url}}/assets/images/docs/arch-overview/widget-element.png){:width="85%"}

`RenderObjectElement`s are an intermediary between their widget analog and the
underlying `RenderObject`, which we’ll come to later.

`RenderObjectElement` 是底層 `RenderObject` 與對應的 widget 之間的橋樑，
我們晚些會介紹它。

The element for any widget can be referenced through its `BuildContext`, which
is a handle to the location of a widget in the tree. This is the `context` in a
function call such as `Theme.of(context)`, and is supplied to the `build()`
method as a parameter.

任何 widget 都可以透過其 `BuildContext` 參考到 Element，
它是該 widget 在樹中的位置的控制代碼。
類似 `Theme.of(context)` 方法呼叫中的 `context`，
它作為 `build()` 方法的引數被傳遞。

Because widgets are immutable, including the parent/child relationship between
nodes, any change to the widget tree (such as changing `Text('A')` to
`Text('B')` in the preceding example) causes a new set of widget objects to be
returned. But that doesn’t mean the underlying representation must be rebuilt.
The element tree is persistent from frame to frame, and therefore plays a
critical performance role, allowing Flutter to act as if the widget hierarchy is
fully disposable while caching its underlying representation. By only walking
through the widgets that changed, Flutter can rebuild just the parts of the
element tree that require reconfiguration.

由於 widgets 以及它上下節點的關係都是不可變的，
因此，對 widget 樹做的任何操作
（例如將 `Text('A')` 替換成 `Text('B')`）
都會返回一個新的 widget 物件集合。
但這並不意味著底層呈現的內容必須要重新建構。
Element 樹每一幀之間都是持久化的，因此起著至關重要的效能作用，
Flutter 依靠該優勢，實現了一種好似 widget 樹被完全拋棄，而快取了底層表示的機制。
Flutter 可以根據發生變化的 widget，來重建需要重新配置的 Element 樹的部分。

### Layout and rendering

### 佈局和渲染

It would be a rare application that drew only a single widget. An important part
of any UI framework is therefore the ability to efficiently lay out a hierarchy
of widgets, determining the size and position of each element before they are
rendered on the screen.

很少有應用只繪製單個 widget。
因此，有效地排布 widget 的結構及在渲染完成前決定每個 Element 的大小和位置，
是所有 UI 框架的重點之一。

The base class for every node in the render tree is
[`RenderObject`]({{site.api}}/flutter/rendering/RenderObject-class.html), which
defines an abstract model for layout and painting. This is extremely general: it
does not commit to a fixed number of dimensions or even a Cartesian coordinate
system (demonstrated by [this example of a polar coordinate
system]({{site.dartpad}}/?id=0f020197a5d4c980342d5c7d9e935cee&null_safety=true)). Each
`RenderObject` knows its parent, but knows little about its children other than
how to _visit_ them and their constraints. This provides `RenderObject` with
sufficient abstraction to be able to handle a variety of use cases.

在渲染樹中，每個節點的基底類別都是
[`RenderObject`]({{site.api}}/flutter/rendering/RenderObject-class.html)，
該基底類別為佈局和繪製定義了一個抽象模型。
這是再平凡不過的事情：它並不總是一個固定的大小，甚至不遵循笛卡爾座標規律
（根據該 [極座標系的範例]({{site.dartpad}}/?id=0f020197a5d4c980342d5c7d9e935cee&null_safety=true) 所示）。
每一個 `RenderObject` 都瞭解其父節點的資訊，
但對於其子節點，除了如何 **存取** 和獲得他們的佈局約束，並沒有更多的資訊。
這樣的設計讓 `RenderObject` 擁有高效的抽象能力，能夠處理各種各樣的使用場景。

During the build phase, Flutter creates or updates an object that inherits from
`RenderObject` for each `RenderObjectElement` in the element tree.
`RenderObject`s are primitives:
[`RenderParagraph`]({{site.api}}/flutter/rendering/RenderParagraph-class.html)
renders text,
[`RenderImage`]({{site.api}}/flutter/rendering/RenderImage-class.html) renders
an image, and
[`RenderTransform`]({{site.api}}/flutter/rendering/RenderTransform-class.html)
applies a transformation before painting its child.

在建構階段，Flutter 會為 Element 樹中的每個 `RenderObjectElement` 建立
或更新其對應的一個從 `RenderObject` 繼承的物件。
`RenderObject` 實際上是原語：
渲染文字的
[`RenderParagraph`]({{site.api}}/flutter/rendering/RenderParagraph-class.html)、
渲染圖片的
[`RenderImage`]({{site.api}}/flutter/rendering/RenderImage-class.html)
以及在繪製子節點內容前應用變換的
[`RenderTransform`]({{site.api}}/flutter/rendering/RenderTransform-class.html)
是更為上層的實現。

![Differences between the widgets hierarchy and the element and render
trees]({{site.url}}/assets/images/docs/arch-overview/trees.png){:width="100%"}

Most Flutter widgets are rendered by an object that inherits from the
`RenderBox` subclass, which represents a `RenderObject` of fixed size in a 2D
Cartesian space. `RenderBox` provides the basis of a _box constraint model_,
establishing a minimum and maximum width and height for each widget to be
rendered.

大部分的 Flutter widget 是由一個繼承了 `RenderBox` 的子類別的物件渲染的，
它們呈現出的 `RenderObject` 會在二維笛卡爾空間中擁有固定的大小。
`RenderBox` 提供了 **盒子限制模型**，為每個 widget 關聯了渲染的最小和最大的寬度和高度。

To perform layout, Flutter walks the render tree in a depth-first traversal and
**passes down size constraints** from parent to child. In determining its size,
the child _must_ respect the constraints given to it by its parent. Children
respond by **passing up a size** to their parent object within the constraints
the parent established.

在進行佈局的時候，Flutter 會以 DFS（深度優先遍歷）方式遍歷渲染樹，
並 **將限制以自上而下的方式** 從父節點傳遞給子節點。
子節點若要確定自己的大小，則 **必須** 遵循父節點傳遞的限制。
子節點的響應方式是在父節點建立的約束內 **將大小以自下而上的方式** 傳遞給父節點。

![Constraints go down, sizes go
up]({{site.url}}/assets/images/docs/arch-overview/constraints-sizes.png){:width="80%"}

At the end of this single walk through the tree, every object has a defined size
within its parent’s constraints and is ready to be painted by calling the
[`paint()`]({{site.api}}/flutter/rendering/RenderObject/paint.html)
method.

在遍歷完一次樹後，每個物件都透過父級約束而擁有了明確的大小，隨時可以透過呼叫
[`paint()`]({{site.api}}/flutter/rendering/RenderObject/paint.html)
進行渲染。

The box constraint model is very powerful as a way to layout objects in _O(n)_
time:

盒子限制模型十分強大，它的物件佈局的時間複雜度是 **O(n)**：

- Parents can dictate the size of a child object by setting maximum and minimum
  constraints to the same value. For example, the topmost render object in a
  phone app constrains its child to be the size of the screen. (Children can
  choose how to use that space. For example, they might just center what they
  want to render within the dictated constraints.)

  父節點可以透過設定最大和最小的尺寸限制，決定其子節點物件的大小。
  例如，在一個手機應用中，最高層級的渲染物件將會限制其子節點的大小為螢幕的尺寸。
  （子節點可以選擇如何佔用空間。例如，它們可能在設定的限制中以居中的方式佈局。）

- A parent can dictate the child’s width but give the child flexibility over
  height (or dictate height but offer flexible over width). A real-world example
  is flow text, which might have to fit a horizontal constraint but vary
  vertically depending on the quantity of text.

  父節點可以決定子節點的寬度，而讓子節點靈活地自適應佈局高度（或決定高度而自適應寬度）。
  現實中有一種例子就是流式佈局的文字，它們常常會填充橫向限制，再根據文字內容的多少決定高度。

This model works even when a child object needs to know how much space it has
available to decide how it will render its content. By using a
[`LayoutBuilder`]({{site.api}}/flutter/widgets/LayoutBuilder-class.html) widget,
the child object can examine the passed-down constraints and use those to
determine how it will use them, for example:

這樣的盒子約束模型，同樣也適用於子節點物件需要知道有多少可用空間渲染其內容的場景，
透過使用 [`LayoutBuilder`]({{site.api}}/flutter/widgets/LayoutBuilder-class.html) widget，
子節點可以得到從上層傳遞下來的約束，併合理利用該約束物件，使用方法如下：

<?code-excerpt "lib/main.dart (LayoutBuilder)"?>
```dart
Widget build(BuildContext context) {
  return LayoutBuilder(
    builder: (context, constraints) {
      if (constraints.maxWidth < 600) {
        return const OneColumnLayout();
      } else {
        return const TwoColumnLayout();
      }
    },
  );
}
```

More information about the constraint and layout system, along with worked
examples, can be found in the [Understanding
constraints]({{site.url}}/development/ui/layout/constraints) topic.

更多有關約束和佈局系統的資訊，及可參考的例子，可以在
[深入理解 Flutter 佈局約束]({{site.url}}/development/ui/layout/constraints)
文章中檢視。

The root of all `RenderObject`s is the `RenderView`, which represents the total
output of the render tree. When the platform demands a new frame to be rendered
(for example, because of a
[vsync](https://source.android.com/devices/graphics/implement-vsync) or because
a texture decompression/upload is complete), a call is made to the
`compositeFrame()` method, which is part of the `RenderView` object at the root
of the render tree. This creates a `SceneBuilder` to trigger an update of the
scene. When the scene is complete, the `RenderView` object passes the composited
scene to the `Window.render()` method in `dart:ui`, which passes control to the
GPU to render it.

所有 `RenderObject` 的根節點是 `RenderView`，代表了渲染樹的總體輸出。
當平台需要渲染新的一幀內容時
（例如一個 [vsync](https://source.android.com/devices/graphics/implement-vsync)
訊號或者一個紋理的更新完成），會呼叫一次 `compositeFrame()` 方法，
它是 `RenderView` 的一部分。
該方法會建立一個 `SceneBuilder` 來觸發當前畫面的更新。
當畫面更新完畢，`RenderView` 會將合成的畫面傳遞給 `dart:ui` 中的 `Window.render()` 方法，
控制 GPU 進行渲染。

Further details of the composition and rasterization stages of the pipeline are
beyond the scope of this high-level article, but more information can be found
[in this talk on the Flutter rendering
pipeline]({{site.youtube-site}}/watch?v=UUfXWzp0-DU).

有關渲染流程的合成和網格化階段的更多細節，將不在本篇深入文章中討論，
但可以在
[關於 Flutter 渲染流程的討論](https://www.bilibili.com/video/BV1Zx411o7tq/)
中瞭解更多。

## Platform embedding

As we’ve seen, rather than being translated into the equivalent OS widgets,
Flutter user interfaces are built, laid out, composited, and painted by Flutter
itself. The mechanism for obtaining the texture and participating in the app
lifecycle of the underlying operating system inevitably varies depending on the
unique concerns of that platform. The engine is platform-agnostic, presenting a
[stable ABI (Application Binary
Interface)]({{site.github}}/flutter/engine/blob/master/shell/platform/embedder/embedder.h)
that provides a _platform embedder_ with a way to set up and use Flutter.

我們都知道，Flutter 的介面建構、佈局、合成和繪製全都由 Flutter 自己完成，
而不是轉換為對應平台系統的原生元件。
獲取紋理和聯動應用底層的生命週期的方法，不可避免地會根據平台特性而改變。
Flutter 引擎本身是與平台無關的，它提供了一個穩定的 ABI（應用二進位制介面），
包含一個 **平台嵌入層**，可以透過其方法設定並使用 Flutter。

The platform embedder is the native OS application that hosts all Flutter
content, and acts as the glue between the host operating system and Flutter.
When you start a Flutter app, the embedder provides the entrypoint, initializes
the Flutter engine, obtains threads for UI and rastering, and creates a texture
that Flutter can write to. The embedder is also responsible for the app
lifecycle, including input gestures (such as mouse, keyboard, touch), window
sizing, thread management, and platform messages. Flutter includes platform
embedders for Android, iOS, Windows, macOS, and Linux; you can also create a
custom platform embedder, as in [this worked
example]({{site.github}}/chinmaygarde/fluttercast) that supports remoting
Flutter sessions through a VNC-style framebuffer or [this worked example for
Raspberry Pi]({{site.github}}/ardera/flutter-pi).

平台嵌入層是用於呈現所有 Flutter 內容的原生系統應用，
它充當著宿主作業系統和 Flutter 之間的粘合劑的角色。
當你啟動一個 Flutter 應用時，嵌入層會提供一個入口，初始化 Flutter 引擎，
獲取 UI 和網格化執行緒，建立 Flutter 可以寫入的紋理。
嵌入層同時負責管理應用的生命週期，包括輸入的操作（例如滑鼠、鍵盤和觸控）、
視窗大小的變化、執行緒管理和平台訊息的傳遞。
Flutter 擁有 Android、iOS、Windows、macOS 和 Linux 的平台嵌入層，
當然，開發者可以建立自訂的嵌入層，正如這個
[可用的例子]({{site.github}}/chinmaygarde/fluttercast)
以 VNC 風格的幀緩衝區支援了遠端 Flutter，還有
[支援樹莓派執行的例子]{{site.github}}/ardera/flutter-pi)。

Each platform has its own set of APIs and constraints. Some brief
platform-specific notes:

每一個平台都有各自的一套 API 和限制。以下是一些關於平台簡短的說明：

- On iOS and macOS, Flutter is loaded into the embedder as a `UIViewController`
  or `NSViewController`, respectively. The platform embedder creates a
  `FlutterEngine`, which serves as a host to the Dart VM and your Flutter
  runtime, and a `FlutterViewController`, which attaches to the `FlutterEngine`
  to pass UIKit or Cocoa input events into Flutter and to display frames
  rendered by the `FlutterEngine` using Metal or OpenGL.

  在 iOS 和 macOS 上，
  Flutter 分別透過 `UIViewController` 和 `NSViewController` 載入到嵌入層。
  這些嵌入層會建立一個 `FlutterEngine`，作為 Dart VM 和您的 Flutter 執行時的宿主，
  還有一個 `FlutterViewController`，關聯對應的 `FlutterEngine`，
  傳遞 UIKit 或者 Cocoa 的輸入事件到 Flutter，
  並將 `FlutterEngine` 渲染的幀內容透過 Metal 或 OpenGL 進行展示。

- On Android, Flutter is, by default, loaded into the embedder as an `Activity`.
  The view is controlled by a
  [`FlutterView`]({{site.api}}/javadoc/io/flutter/embedding/android/FlutterView.html),
  which renders Flutter content either as a view or a texture, depending on the
  composition and z-ordering requirements of the Flutter content.

  在 Android 上，Flutter 預設作為一個 `Activity` 載入到嵌入層中。
  此時檢視是透過一個
  [`FlutterView`]({{site.api}}/javadoc/io/flutter/embedding/android/FlutterView.html)
  進行控制的，基於 Flutter 內容的合成和 z 排列 (z-ordering) 的要求，
  將 Flutter 的內容以檢視模式或紋理模式進行呈現。

- On Windows, Flutter is hosted in a traditional Win32 app, and content is
  rendered using
  [ANGLE](https://chromium.googlesource.com/angle/angle/+/master/README.md), a
  library that translates OpenGL API calls to the DirectX 11 equivalents.

  在 Windows 上，Flutter 的宿主是一個傳統的 Win32 應用，內容是透過一個將 OpenGL API
  呼叫轉換成 DirectX 11 的等價呼叫的函式庫
  [ANGLE](https://chromium.googlesource.com/angle/angle/+/master/README.md)
  進行渲染的。目前正在嘗試將 UWP 應用作為 Windows 的一種嵌入層，並將 ANGLE 替換為
  透過 DirectX 12 直接呼叫 GPU 的方式。

## Integrating with other code

## 與其他程式碼進行整合

Flutter provides a variety of interoperability mechanisms, whether you’re
accessing code or APIs written in a language like Kotlin or Swift, calling a
native C-based API, embedding native controls in a Flutter app, or embedding
Flutter in an existing application.

Flutter 提供了多種程式碼互動機制，無論你是在呼叫 Kotlin 或者 Swift 這些語言編寫的程式碼或 API，
或是呼叫 C 語言基礎的 API，或是將原生程式碼能力嵌入 Flutter 應用，
又或是將 Flutter 嵌入現有的應用。

### Platform channels

### 平台通道

For mobile and desktop apps, Flutter allows you to call into custom code through
a _platform channel_, which is a mechanism for communicating between your
Dart code and the platform-specific code of your host app. By creating a common
channel (encapsulating a name and a codec), you can send and receive messages
between Dart and a platform component written in a language like Kotlin or
Swift. Data is serialized from a Dart type like `Map` into a standard format,
and then deserialized into an equivalent representation in Kotlin (such as
`HashMap`) or Swift (such as `Dictionary`).

對於移動端和桌面端應用而言，Flutter 提供了透過 **平台通道** 呼叫自訂程式碼的能力，
這是一種非常簡單的在宿主應用之間讓 Dart 程式碼與平台程式碼通訊的機制。
透過建立一個常用的通道（封裝通道名稱和編碼），開發者可以在
Dart 與使用 Kotlin 和 Swift 等語言編寫的平台元件之間傳送和接收訊息。
資料會由 Dart 型別（例如 Map）序列化為一種標準格式，
然後反序列化為 Kotlin（例如 `HashMap`）或者 Swift（例如 `Dictionary`）中的等效型別。 

![How platform channels allow Flutter to communicate with host
code]({{site.url}}/assets/images/docs/arch-overview/platform-channels.png){:width="70%"}

The following is a short platform channel example of a Dart call to a receiving
event handler in Kotlin (Android) or Swift (iOS):

下方的範例是在 Kotlin (Android) 或 Swift (iOS) 中處理 Dart 呼叫平台通道事件
的簡單接收處理：

<?code-excerpt "lib/main.dart (MethodChannel)"?>
```dart
// Dart side
const channel = MethodChannel('foo');
final String greeting = await channel.invokeMethod('bar', 'world');
print(greeting);
```

```kotlin
// Android (Kotlin)
val channel = MethodChannel(flutterView, "foo")
channel.setMethodCallHandler { call, result ->
  when (call.method) {
    "bar" -> result.success("Hello, ${call.arguments}")
    else -> result.notImplemented()
  }
}
```

```swift
// iOS (Swift)
let channel = FlutterMethodChannel(name: "foo", binaryMessenger: flutterView)
channel.setMethodCallHandler {
  (call: FlutterMethodCall, result: FlutterResult) -> Void in
  switch (call.method) {
    case "bar": result("Hello, \(call.arguments as! String)")
    default: result(FlutterMethodNotImplemented)
  }
}
```

Further examples of using platform channels, including examples for macOS, can
be found in the [flutter/plugins]({{site.repo.plugins}})
repository<sup><a href="#a3">3</a></sup>. There are also [thousands of plugins
already available]({{site.pub}}/flutter) for Flutter that cover many common
scenarios, ranging from Firebase to ads to device hardware like camera and
Bluetooth.

更多關於如何使用平台通道的例子，包括 macOS 平台的範例，可以在
[flutter/plugins]({{site.github}}/flutter/plugins) 程式碼儲存庫
<sup><a href="#a3">3</a></sup>找到。

### Foreign Function Interface

### 外部函式介面

For C-based APIs, including those that can be generated for code written in
modern languages like Rust or Go, Dart provides a direct mechanism for binding
to native code using the `dart:ffi` library. The foreign function interface
(FFI) model can be considerably faster than platform channels, because no
serialization is required to pass data. Instead, the Dart runtime provides the
ability to allocate memory on the heap that is backed by a Dart object and make
calls to statically or dynamically linked libraries. FFI is available for all
platforms other than web, where the [js package]({{site.pub}}/packages/js)
serves an equivalent purpose.

對於基於 C 語言的 API，包括使用現代語言 Rust 或 Go 產生的程式碼，
Dart 也提供了 `dart:ffi` 庫，一套直接繫結原生程式碼的機制。
外部函式介面 (foreign function interface，FFI) 比平台通道更快，因為不需要序列化即可傳遞資料。
實際上，Dart 的執行時提供了在堆上分配 Dart 物件記憶體的支援，以及呼叫靜態或動態連結庫的能力。
除了 Web 平台外，FFI 在其他平台均可以使用，因為 Web 平臺上的
[js 包]({{site.pub}}/packages/js) 已經具有相同的用途。

To use FFI, you create a `typedef` for each of the Dart and unmanaged method
signatures, and instruct the Dart VM to map between them. As an example,
here’s a fragment of code to call the traditional Win32 `MessageBox()` API:

若您需要使用 FFI，請為每一個 Dart 和未經管理的函式的簽名建立一個 `typedef`，
並且指示 Dart VM 為它們建立關聯。
下面這段程式碼片段是呼叫 Win32 的 `MessageBox()` API 的簡單範例：

<?code-excerpt "lib/ffi.dart (FFI)"?>
```dart
import 'dart:ffi';
import 'package:ffi/ffi.dart'; // contains .toNativeUtf16() extension method

typedef MessageBoxNative = Int32 Function(
  IntPtr hWnd,
  Pointer<Utf16> lpText,
  Pointer<Utf16> lpCaption,
  Int32 uType,
);

typedef MessageBoxDart = int Function(
  int hWnd,
  Pointer<Utf16> lpText,
  Pointer<Utf16> lpCaption,
  int uType,
);

void exampleFfi() {
  final user32 = DynamicLibrary.open('user32.dll');
  final messageBox =
      user32.lookupFunction<MessageBoxNative, MessageBoxDart>('MessageBoxW');

  final result = messageBox(
    0, // No owner window
    'Test message'.toNativeUtf16(), // Message
    'Window caption'.toNativeUtf16(), // Window title
    0, // OK button only
  );
}
```

### Rendering native controls in a Flutter app

### 在 Flutter 應用中渲染原生內容

Because Flutter content is drawn to a texture and its widget tree is entirely
internal, there's no place for something like an Android view to exist within
Flutter's internal model or render interleaved within Flutter widgets. That’s a
problem for developers that would like to include existing platform components
in their Flutter apps, such as a browser control.

由於 Flutter 的內容會繪製在單一的紋理內，並且 widget 樹是完全在內部的，
因此在 Flutter 的內部模型中無法存在 Android 檢視之類別的內容，也無法與 Flutter 的 widget 交錯渲染
對於需要在 Flutter 應用中展示原生元件（例如內建瀏覽器）的開發者來說，這是一個問題。

Flutter solves this by introducing platform view widgets
([`AndroidView`]({{site.api}}/flutter/widgets/AndroidView-class.html)
and [`UiKitView`]({{site.api}}/flutter/widgets/UiKitView-class.html))
that let you embed this kind of content on each platform. Platform views can be
integrated with other Flutter content<sup><a href="#a4">4</a></sup>. Each of
these widgets acts as an intermediary to the underlying operating system. For
example, on Android, `AndroidView` serves three primary functions:

Flutter 透過引入了平台 widget
([`AndroidView`]({{site.api}}/flutter/widgets/AndroidView-class.html) 和
[`UiKitView`]({{site.api}}/flutter/widgets/UiKitView-class.html))
解決了這個問題，開發者可以在每一種平臺上嵌入此類內容。
平臺視圖可以與其他的 Flutter 內容整合<sup><a href="#a4">4</a></sup>。
這些 widget 充當了底層作業系統與 Flutter 之間的橋樑。
例如在 Android 上，`AndroidView` 主要提供了三項功能：

- Making a copy of the graphics texture rendered by the native view and
  presenting it to Flutter for composition as part of a Flutter-rendered surface
  each time the frame is painted.

  複製原生檢視渲染的圖形紋理，在 Flutter 每幀渲染時提交給 Flutter 渲染層進行合成。

- Responding to hit testing and input gestures, and translating those into the
  equivalent native input.

  響應命中測試和輸入手勢，將其轉換為等效的原生輸入事件。

- Creating an analog of the accessibility tree, and passing commands and
  responses between the native and Flutter layers.

  建立類似的可及性樹，並在原生層與 Flutter 層之間傳遞命令和響應。

Inevitably, there is a certain amount of overhead associated with this
synchronization. In general, therefore, this approach is best suited for complex
controls like Google Maps where reimplementing in Flutter isn’t practical.

但不可避免的是，這樣的同步操作必然會帶來相應的開銷。
因此該方法通常更適合複雜的控制項，例如谷歌地圖這種不適合在 Flutter 中重新實現的。

Typically, a Flutter app instantiates these widgets in a `build()` method based
on a platform test. As an example, from the
[google_maps_flutter]({{site.pub}}/packages/google_maps_flutter) plugin:

通常 Flutter 應用會在 `build()` 方法中基於平台判斷來例項化這些 widget。
例如在 [google_maps_flutter]({{site.pub}}/packages/google_maps_flutter)
外掛中：

<!-- skip -->
```dart
if (defaultTargetPlatform == TargetPlatform.android) {
  return AndroidView(
    viewType: 'plugins.flutter.io/google_maps',
    onPlatformViewCreated: onPlatformViewCreated,
    gestureRecognizers: gestureRecognizers,
    creationParams: creationParams,
    creationParamsCodec: const StandardMessageCodec(),
  );
} else if (defaultTargetPlatform == TargetPlatform.iOS) {
  return UiKitView(
    viewType: 'plugins.flutter.io/google_maps',
    onPlatformViewCreated: onPlatformViewCreated,
    gestureRecognizers: gestureRecognizers,
    creationParams: creationParams,
    creationParamsCodec: const StandardMessageCodec(),
  );
}
return Text(
    '$defaultTargetPlatform is not yet supported by the maps plugin');
```

Communicating with the native code underlying the `AndroidView` or `UiKitView`
typically occurs using the platform channels mechanism, as previously described.

如上文所述，`AndroidView` 和 `UiKitView` 通常是利用平台通道的機制與原生進行通訊。

At present, platform views aren’t available for desktop platforms, but this is
not an architectural limitation; support might be added in the future.

目前桌面平台尚未支援平臺視圖，但這並不是一個架構層面的限制。
未來可能將增加對桌面平台的支援。

### Hosting Flutter content in a parent app

### 在上層應用中託管 Flutter 內容

The converse of the preceding scenario is embedding a Flutter widget in an
existing Android or iOS app. As described in an earlier section, a newly created
Flutter app running on a mobile device is hosted in an Android activity or iOS
`UIViewController`. Flutter content can be embedded into an existing Android or
iOS app using the same embedding API.

與上一個場景相反的是，將 Flutter widget 整合至現有的 Android 或 iOS 應用中。
先前提到，新建立的 Flutter 應用，在移動裝置上是在一個 Android 的 Activity 或
iOS 的 `UIViewController` 中執行。
開發者可以使用相同的嵌入 API 將 Flutter 內容整合至現有的 Android 或 iOS 應用中。

The Flutter module template is designed for easy embedding; you can either embed
it as a source dependency into an existing Gradle or Xcode build definition, or
you can compile it into an Android Archive or iOS Framework binary for use
without requiring every developer to have Flutter installed.

Flutter 模組範本設計簡單，易於嵌入。
開發者可以將其作為原始碼依賴項整合到 Gradle 或 Xcode 建構定義中，
或者將其打包成 Android Archive (AAR) 或 iOS Framework 二進位制供其他開發者使用，
而無需安裝 Flutter。

The Flutter engine takes a short while to initialize, because it needs to load
Flutter shared libraries, initialize the Dart runtime, create and run a Dart
isolate, and attach a rendering surface to the UI. To minimize any UI delays
when presenting Flutter content, it’s best to initialize the Flutter engine
during the overall app initialization sequence, or at least ahead of the first
Flutter screen, so that users don’t experience a sudden pause while the first
Flutter code is loaded. In addition, separating the Flutter engine allows it to
be reused across multiple Flutter screens and share the memory overhead involved
with loading the necessary libraries.

Flutter 引擎需要一段短暫的時間做初始化，用於載入 Flutter 的共享庫、初始化 Dart 的執行時、
建立並執行 Dart isolate 執行緒並將渲染層與 UI 進行繫結。
為了最大限度地減少呈現 Flutter 介面時的延遲，
最好是在應用初始化時或至少在第一個 Flutter 頁面展示前，一併初始化 Flutter 引擎，
如此一來使用者不會在首個 Flutter 頁面載入時感到突然地卡頓。
另外，Flutter 的引擎分離使得多個 Flutter 頁面可以複用引擎，共享必要庫載入時的記憶體消耗。

More information about how Flutter is loaded into an existing Android or iOS app
can be found at the [Load sequence, performance and memory
topic]({{site.url}}/development/add-to-app/performance).

更多將 Flutter 整合至現有的 Android 和 iOS 應用的內容，可在
[控制載入順序，最佳化效能與記憶體]({{site.url}}/development/add-to-app/performance)
文章中檢視。

## Flutter web support

## Flutter 對 Web 的支援

While the general architectural concepts apply to all platforms that Flutter
supports, there are some unique characteristics of Flutter’s web support that
are worthy of comment.

雖然 Flutter 支援的所有平台的都適用於同一個架構概念，
但是在 Web 平台的支援上有一些獨特的特徵值得說明。

Dart has been compiling to JavaScript for as long as the language has existed,
with a toolchain optimized for both development and production purposes. Many
important apps compile from Dart to JavaScript and run in production today,
including the [advertiser tooling for Google Ads](https://ads.google.com/home/).
Because the Flutter framework is written in Dart, compiling it to JavaScript was
relatively straightforward.

Dart 語言存在之初就已經支援直接編譯成 JavaScript，並且針對開發和生產目的對其工具鏈進行了最佳化。
許多重要的應用已經使用 Dart 編譯成的 JavaScript 在生產環境上執行，
包括 [Google Ads 的廣告商工具](https://ads.google.cn/home/)。
由於 Flutter 框架是 Dart 編寫的，將其編譯成 JavaScript 相對而言更為簡單。

However, the Flutter engine, written in C++, is designed to interface with the
underlying operating system rather than a web browser. A different approach is
therefore required. On the web, Flutter provides a reimplementation of the
engine on top of standard browser APIs. We currently have two options for
rendering Flutter content on the web: HTML and WebGL. In HTML mode, Flutter uses
HTML, CSS, Canvas, and SVG. To render to WebGL, Flutter uses a version of Skia
compiled to WebAssembly called
[CanvasKit](https://skia.org/user/modules/canvaskit). While HTML mode offers the
best code size characteristics, CanvasKit provides the fastest path to the
browser's graphics stack, and offers somewhat higher graphical fidelity with the
native mobile targets<sup><a href="#a5">5</a></sup>.

然而，使用 C++ 編寫的 Flutter 引擎是為了與底層作業系統進行互動的，而不是 Web 瀏覽器。
因此我們需要另闢蹊徑。Flutter 在 Web 平臺上以瀏覽器的標準 API 重新實現了引擎。
目前我們有兩種在 Web 上呈現內容的選項：HTML 和 WebGL。
在 HTML 模式下，Flutter 使用 HTML、CSS、Canvas 和 SVG 進行渲染。
而在 WebGL 模式下，Flutter 使用了一個編譯為 WebAssembly 的 Skia 版本，
名為 [CanvasKit](https://skia.org/user/modules/canvaskit)。
HTML 模式提供了最佳的程式碼大小，CanvasKit 則提供了瀏覽器圖形堆疊渲染的最快途徑，
併為原生平台的內容<sup><a href="#a5">5</a></sup>提供了更高的圖形保真度。

The web version of the architectural layer diagram is as follows:

Web 版本的分層架構圖如下所示：

![Flutter web
architecture]({{site.url}}/assets/images/docs/arch-overview/web-arch.png){:width="100%"}

Perhaps the most notable difference compared to other platforms on which Flutter
runs is that there is no need for Flutter to provide a Dart runtime. Instead,
the Flutter framework (along with any code you write) is compiled to JavaScript.
It’s also worthy to note that Dart has very few language semantic differences
across all its modes (JIT versus AOT, native versus web compilation), and most
developers will never write a line of code that runs into such a difference.

與其他執行 Flutter 的平台相比，最明顯的區別也許是 Flutter 不再需要提供 Dart 的執行時。
取而代之的是 Flutter 框架本身（和你寫的程式碼）一併編譯成 JavaScript。
另外值得注意的是，Dart 在不同模式下（JIT 和 AOT、平台原生和 Web 編譯）的語義幾乎沒有差異，
大部分開發者絕對可以無差異地編寫這兩種模式下的程式碼。

During development time, Flutter web uses
[`dartdevc`]({{site.dart-site}}/tools/dartdevc), a compiler that supports
incremental compilation and therefore allows hot restart (although not currently
hot reload) for apps. Conversely, when you are ready to create a production app
for the web, [`dart2js`]({{site.dart-site}}/tools/dart2js), Dart’s
highly-optimized production JavaScript compiler is used, packaging the Flutter
core and framework along with your application into a minified source file that
can be deployed to any web server. Code can be offered in a single file or split
into multiple files through [deferred
imports]({{site.dart-site}}/guides/language/language-tour#lazily-loading-a-library).

在進行開發時，Web 版本的 Flutter 使用支援增量編譯的編譯器
[`dartdevc`]({{site.dart-site}}/tools/dartdevc) 進行編譯，
以支援應用熱重啟（儘管目前尚未支援熱重載）。
相反，當你準備好建立一個生產環境的 Web 應用時，Dart 深度最佳化的編譯器
[`dart2js`]({{site.dart-site}}/tools/dart2js) 將會用於編譯，
將 Flutter 核心框架和你的應用打包至縮小的原始檔中，可部署在任何伺服器上。
程式碼可以在單個檔案中提供，也可拆分至多個檔案以
[延遲載入庫]({{site.dart-site}}/guides/language/language-tour#lazily-loading-a-library)
提供。


## Further information

## 更多資訊

For those interested in more information about the internals of Flutter, the
[Inside Flutter]({{site.url}}/resources/inside-flutter) whitepaper
provides a useful guide to the framework’s design philosophy.

若你對 Flutter 的更多內部細節感興趣
[Flutter 工作原理]({{site.url}}/resources/inside-flutter)
白皮書為框架的設計理念提供了很好的入門途徑。

---

**Footnotes:**

**腳註：**

<sup><a name="a1">1</a></sup> While the `build` function returns a fresh tree,
you only need to return something _different_ if there's some new
configuration to incorporate. If the configuration is in fact the same, you can
just return the same widget.

<sup><a name="a1">1</a></sup> 在 `build` 方法返回一個全新的結構樹時，
你只需要返回不同的內容，就可以合併一些新的配置。
如果配置實際上是相同的，完全可以返回同樣的 widget。

<sup><a name="a2">2</a></sup> This is a slight simplification for ease of
reading. In practice, the tree might be more complex.

<sup><a name="a2">2</a></sup> 為了便於閱讀，該圖已進行簡化。實際上的結構可能更為複雜。

<sup><a name="a3">3</a></sup> While work is underway on Linux and Windows,
examples for those platforms can be found in the [Flutter desktop embedding
repository]({{site.github}}/google/flutter-desktop-embedding/tree/master/plugins).
As development on those platforms reaches maturity, this content will be
gradually migrated into the main Flutter repository.

<sup><a name="a3">3</a></sup> 在 Linux 和 Windows 平台的開發處理序中，平台對應的範例可以在
[Flutter 桌面整合程式碼儲存庫]({{site.github}}/google/flutter-desktop-embedding/tree/master/plugins)
中找到。隨著這些平台的開發愈發成熟，這些內容會逐步遷移到 Flutter 主程式碼儲存庫中。

<sup><a name="a4">4</a></sup> There are some limitations with this approach, for
example, transparency doesn’t composite the same way for a platform view as it
would for other Flutter widgets.

<sup><a name="a4">4</a></sup> 該方法有一些侷限性，例如，
平臺視圖的透明度計算與其他 Flutter widget 的計算不同。

<sup><a name="a5">5</a></sup> One example is shadows, which have to be
approximated with DOM-equivalent primitives at the cost of some fidelity.

<sup><a name="a5">5</a></sup> 其中一個例子便是陰影，
它必須以等效於 DOM 原語的內容來實現，並且需要丟失一定的保真度。
