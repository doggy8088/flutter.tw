---
title: Creating responsive and adaptive apps
title: 建立響應式和自適應的應用
description: It's important to create apps, whether for mobile or web, so that they are responsive to size and orientation changes.
description: 建立用於移動端和 Web 的應用非常重要，它們能夠響應尺寸和旋轉方向的變化。
short-title: 響應式且自適應
---

One of Flutter's primary goals is to create a framework
that allows you to develop apps from a single codebase
that look and feel great on any platform.

Flutter 的首要目標，是建構一個可以使用單一程式碼來源，
開發在所有平臺上都有著良好的視覺和體驗的應用的框架。

This means that your app may appear on screens of
many different sizes, from a watch, to a foldable
phone with two screens, to a high def monitor.

這意味著你的應用可能會在不同大小的螢幕上使用，
從智慧手錶，到可摺疊的雙屏裝置，再到高畫質顯示器。

Two terms that describe concepts for this
scenario are _adaptive_ and _responsive_. Ideally,
you'd want your app to be _both_ but what, 
exactly, does this mean?
These terms are similar, but they are not the same.

通常這樣的考量被分為兩種概念：**自適應** 和 **響應式**。
理想條件下，你的應用應該兩者兼具，但是它們究竟代表了什麼？
這兩種概念有些類似，但並不是同一種含義。

## The difference between an adaptive and a responsive app

## 自適應應用和響應式應用的區別

_Adaptive_ and _responsive_ can be viewed as separate
dimensions of an app: you can have an adaptive app
that is not responsive, or vice versa. And, of course,
an app can be both, or neither.

**自適應** 和 **響應式** 可以看作應用裡的兩種維度：
你的應用可能是自適應的，但不是響應式的，又或是反行其道。
當然，你的應用可能既自適應又為響應式，也可能兩者均未實現。

**Responsive**
<br> Typically, a _responsive_ app has had its layout
  tuned for the available screen size. Often this
  means (for example), re-laying out the UI if the
  user resizes the window, or changes the device's
  orientation. This is especially necessary when
  the same app can run on a variety of devices,
  from a watch, phone, tablet, to a laptop or
  desktop computer.

**響應式**
<br> 通常來說，一個 **響應式** 應用的佈局會根據可用的螢幕大小而調整。
  常見的場景是在使用者重新調整視窗大小或旋轉螢幕時，重新佈局 UI。
  對於需要在多種裝置（手錶、手機、平板、筆記本或桌上型電腦）上執行的應用而言，
  這是必要的要素。

**Adaptive**
<br> _Adapting_ an app to run on different device types,
  such as mobile and desktop, requires dealing
  with mouse and keyboard input, as well as
  touch input. It also means there are different
  expectations about the app's visual density,
  how component selection works
  (cascading menus vs bottom sheets, for example),
  using platform-specific features (such as
  top-level windows), and more.

**自適應**
<br> 應用以 **自適應** 的方式在不同的裝置上（移動端和桌面端）執行，
  需要同時處理滑鼠、鍵盤和觸控輸入。
  這也意味著應用的視覺密度、元件的選擇（層級選單或底部抽屜）、
  平台特定的行為（例如置頂的視窗）等內容將在不同的平臺上有一定的差異。

## Creating a responsive Flutter app

## 建構一個響應式的 Flutter 應用

Flutter allows you to create apps that self-adapt
to the device's screen size and orientation.

Flutter 讓你能夠建構自動適配螢幕大小和旋轉方向的應用。

There are two basic approaches to creating Flutter
apps with responsive design:

建構響應式設計的 Flutter 應用，有以下兩種較為基礎的方式：

**Use the [`LayoutBuilder`][] class**
<br> From its [`builder`][] property, you get a
  [`BoxConstraints`][] object.
  Examine the constraint's properties to decide what to
  display. For example, if your [`maxWidth`][] is greater than
  your width breakpoint, return a [`Scaffold`][] object with a
  row that has a list on the left. If it's narrower,
  return a [`Scaffold`][] object with a drawer containing that
  list. You can also adjust your display based on the
  device's height, the aspect ratio, or some other property.
  When the constraints change (for example,
  the user rotates the phone, or puts your app into a tile UI
  in Nougat), the build function runs.

**使用 [`LayoutBuilder`][] 類**
<br> 透過它的 [`builder`][] 屬性，你可以得到一個 [`BoxConstraints`][] 物件。
  你可以檢查約束裡的屬性，來決定如何進行顯示。
  例如，如果約束裡的 [`maxWidth`][] 超過了你的寬度分界點，
  你可以返回一個 [`Scaffold`][]，它包含一列內容，左側是一個列表。
  如果約束更小，則返回一個列表在抽屜裡的 [`Scaffold`][]。
  你也可以根據你的裝置高度、螢幕的比例或者其他的屬性，來調整你的顯示。
  當約束改變時（例如使用者旋轉了手機，或是在 Android N 上將應用放置到 tile UI 中）
  建構方法會執行。

**Use the [`MediaQuery.of()`][] method in your build functions**
<br> This gives you the size, orientation, etc, of your current app.
  This is more useful if you want to make decisions based on the
  complete context rather than on just the size of your particular
  widget. Again, if you use this, then your build function automatically
  runs if the user somehow changes the app's size.

**在建構方法中使用 [`MediaQuery.of()`][] 方法**
<br> 這個方法可以獲取到當前應用（基於上下文）的尺寸、旋轉方向等資訊。
  如果你需要基於完整的上下文資訊進行佈局決策，
  而不是基於特定的 widget，這個方法將非常有用。
  同樣的，如果應用的尺寸發生了改變，建構方法也會自動執行。

Other useful widgets and classes for creating a responsive UI:

以下是其他有助於建構響應式介面的 widget：

* [`AspectRatio`][]
* [`CustomSingleChildLayout`][]
* [`CustomMultiChildLayout`][]
* [`FittedBox`][]
* [`FractionallySizedBox`][]
* [`LayoutBuilder`][]
* [`MediaQuery`][]
* [`MediaQueryData`][]
* [`OrientationBuilder`][]

### Other resources

### 更多資源

For more information, here are a few resources,
including contributions from the Flutter community:

想了解更多資訊，以下是一些來自社群貢獻的資源：

* [Developing for Multiple Screen Sizes and Orientations in
  Flutter][] by Deven Joshi

  [使用 Flutter 開發兼顧多種螢幕尺寸和旋轉的應用][Developing
  for Multiple Screen Sizes and Orientations in Flutter]，
  由 Deven Joshi 撰寫

* [Build Responsive UIs in Flutter][] by Raouf Rahiche

  [使用 Flutter 建構響應式介面][Build Responsive UIs in Flutter]，
  由 Raouf Rahiche 撰寫

* [Making Cross-platform Flutter Landing Page Responsive][]
  by Priyanka Tyagi

  [建構響應式的 Flutter 跨平臺的登入頁][Making Cross-platform
  Flutter Landing Page Responsive]，由 Priyanka Tyagi 撰寫

* [How to make flutter app responsive according to different screen
  size?][], a question on StackOverflow

  [如何根據不同的螢幕大小建構響應式的 Flutter 應用？][How to make
  flutter app responsive according to different screen size?]，
  在 StackOverflow 上的一個問題。

[`AspectRatio`]: {{site.api}}/flutter/widgets/AspectRatio-class.html
[`BoxConstraints`]: {{site.api}}/flutter/rendering/BoxConstraints-class.html
[Build Responsive UIs in Flutter]: {{site.medium}}/flutter-community/build-responsive-uis-in-flutter-fd450bd59158
[`builder`]: {{site.api}}/flutter/widgets/LayoutBuilder/builder.html
[`CustomMultiChildLayout`]: {{site.api}}/flutter/widgets/CustomMultiChildLayout-class.html
[`CustomSingleChildLayout`]: {{site.api}}/flutter/widgets/CustomSingleChildLayout-class.html
[Developing for Multiple Screen Sizes and Orientations in Flutter]: {{site.medium}}/flutter-community/developing-for-multiple-screen-sizes-and-orientations-in-flutter-fragments-in-flutter-a4c51b849434
[`FittedBox`]: {{site.api}}/flutter/widgets/FittedBox-class.html

[`FractionallySizedBox`]: {{site.api}}/flutter/widgets/FractionallySizedBox-class.html
[How to make flutter app responsive according to different screen size?]: {{site.so}}/questions/49704497/how-to-make-flutter-app-responsive-according-to-different-screen-size
[`LayoutBuilder`]: {{site.api}}/flutter/widgets/LayoutBuilder-class.html
[Making Cross-platform Flutter Landing Page Responsive]: {{site.medium}}/flutter-community/making-cross-platform-flutter-landing-page-responsive-7fffe0655970
[`maxWidth`]: {{site.api}}/flutter/rendering/BoxConstraints/maxWidth.html
[`MediaQuery`]: {{site.api}}/flutter/widgets/MediaQuery-class.html
[`MediaQuery.of()`]: {{site.api}}/flutter/widgets/MediaQuery/of.html
[`MediaQueryData`]: {{site.api}}/flutter/widgets/MediaQueryData-class.html
[`OrientationBuilder`]: {{site.api}}/flutter/widgets/OrientationBuilder-class.html
[`Scaffold`]: {{site.api}}/flutter/material/Scaffold-class.html

## Creating an adaptive Flutter app

## 建立自適應的 Flutter 應用

Learn more about creating an adaptive Flutter app with
[Building adaptive apps][], written by the gskinner team.

你可以閱讀由 gskinner 團隊撰寫的 [建構自適應的應用][Building adaptive apps]
瞭解更多關於建構自適應應用的內容。

You might also check out the following episodes
of The Boring Show:

你也可以觀看下面幾期關於自適應佈局的 The Boring Show：

<iframe style="max-width: 100%" width="560" height="315" src="{{site.youtube-site}}/embed/n6Awpg1MO6M" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[Adaptive layouts][]

<iframe style="max-width: 100%" width="560" height="315" src="{{site.youtube-site}}/embed/eikOZzfc0l4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[Adaptive layouts, part 2][]

For an excellent example of an adaptive app,
check out Flutter Folio, a scrapbooking app created 
in collaboration with gskinner and the Flutter team:

想要嘗試精美的自適應應用，可以下載由 gskinner 和 Flutter
團隊共建的剪貼簿應用 Flutter Folio：

<iframe width="560" height="315" src="{{site.youtube-site}}/embed/yytBENOnF0w" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

The [Folio source code][] is also available on GitHub.
Learn more on the [gskinner blog][].

[Folio 應用的原始碼][Folio source code] 也可以在 GitHub 找到，
你可以閱讀 [gskinner 的部落格][gskinner blog] 瞭解更多內容。

### Other resources

### 更多資源

You can learn more about creating platform adaptive apps
in the following resources:

你可以在以下的資源中瞭解更多關於如何建構自適應平台應用的內容：

* [Platform-specific behaviors and adaptations][], a page on this site.

  [不同平台操作體驗的差異和適配][Platform-specific behaviors and adaptations]，
  站內的另一篇文件。

* [Designing truly adaptive user interfaces][] a blog post and video
  by Aloïs Deniel, presented at this year's FlutterViking conference.

  [設計真正能夠自適應的使用者互動][Designing truly adaptive user interfaces]，
  由 Aloïs Deniel 在今年的 FlutterViking 會議上釋出的文章和影片。

* The [Flutter gallery app][] ([repo][]) has been written as an
  adaptive app.

  以自適應應用為目標編寫的
  [Flutter Gallery 應用][Flutter gallery app]（[原始碼儲存庫][repo]）。

[Adaptive layouts]: {{site.youtube-site}}/watch?v=n6Awpg1MO6M&t=694s
[Adaptive layouts, part 2]: {{site.youtube-site}}/watch?v=eikOZzfc0l4&t=11s
[Building adaptive apps]: {{site.url}}/development/ui/layout/building-adaptive-apps

[Designing truly adaptive user interfaces]: https://aloisdeniel.com/#/posts/adaptative-ui
[Flutter gallery app]: {{site.gallery}}
[Folio source code]: {{site.github}}/gskinnerTeam/flutter-folio
[gskinner blog]: https://blog.gskinner.com/
[Platform-specific behaviors and adaptations]: {{site.url}}/resources/platform-adaptations
[repo]: {{site.repo.gallery}}
