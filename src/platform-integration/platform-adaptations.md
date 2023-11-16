---
title: Automatic platform adaptations
title: 自動適配不同平台操作體驗
description: Learn more about Flutter's platform adaptiveness.
description: 瞭解更多 Flutter 的平台適配機制。
tags: Flutter參考資料
keywords: 平台適配,研究,Flutter Android,Flutter iOS,Flutter跨平臺
---

## Adaptation philosophy

## 適配哲學

In general, two cases of platform adaptiveness exist:

平台適配通常有兩種情形：

1. Things that are behaviors of the OS environment
   (such as text editing and scrolling) and that
   would be 'wrong' if a different behavior took place.

   作業系統所特有的操作體驗（例如文字編輯和滾動）。
   如果操作體驗與作業系統不一致，則通常會被認為是“錯誤的”。

2. Things that are conventionally implemented in apps using
   the OEM's SDKs (such as using parallel tabs on iOS or
   showing an [android.app.AlertDialog][] on Android).

   使用 OEM 提供的 SDK 實現的功能體驗
   （例如 iOS 常使用的選項卡，
   Android 使用 [android.app.AlertDialog][] 顯示一個提示視窗）。

This article mainly covers the automatic adaptations
provided by Flutter in case 1 on Android and iOS.

本文囊括了 Flutter 為解決情形 1 而提供的覆蓋 Android 和 iOS 的自動適配。

For case 2, Flutter bundles the means to produce the
appropriate effects of the platform conventions but doesn't
adapt automatically when app design choices are needed.
For a discussion, see [issue #8410][] and the
[Material/Cupertino adaptive widget problem definition][].

對於情形 2，Flutter 提供了一些工具可以產生符合平台習慣的體驗，
但是不會根據平台自動適配，需要根據 App 設計來手工選擇。
更多有關的討論，請存取 [issue #8410][] 和這個文件
[定義 Material/Cupertino widget 適配問題][Material/Cupertino adaptive widget problem definition]

For an example of an app using different information
architecture structures on Android and iOS but sharing
the same content code, see the [platform_design code samples][].

如果一個應用需要在 Android 和 iOS 不同架構上使用相同的程式碼，
請參閱 [platform_design 這份程式碼範例][platform_design code samples]。

{{site.alert.info}}

Preliminary guides addressing case 2 
are being added to the UI components section. 
You can request additional guides by commenting on [issue #8427][].

本文件的 UI 元件部分正在新增有關情形 2 的指南。
如果你有更多的需求，請在 [issue #8427][] 中留言讓我們知道。

{{site.alert.end}}

{{site.alert.end}}

## Page navigation

## 頁面導航

Flutter provides the navigation patterns seen on Android
and iOS and also automatically adapts the navigation animation
to the current platform.

Flutter 分別為 Android 和 iOS 提供了各自平台的導航模式，
並根據當前平台自動適配導航轉場動畫。

### Navigation transitions

### 導航轉場動畫

On **Android**, the default [`Navigator.push()`][] transition
is modeled after [`startActivity()`][],
which generally has one bottom-up animation variant.

**Android** 平台，預設提供的 [`Navigator.push()`][] 
轉場動畫模仿了 [`startActivity()`][] 的動畫，
即一種自下而上的動畫效果。

On **iOS**:

**iOS** 平台：

* The default [`Navigator.push()`][] API produces an
  iOS Show/Push style transition that animates from
  end-to-start depending on the locale's RTL setting.
  The page behind the new route also parallax-slides
  in the same direction as in iOS.

  iOS 的 [`Navigator.push()`][] API 提供了 iOS 上的
  Show 轉場動畫（也被稱為 Push 轉場動畫），
  即根據語言的方向設定，執行一種從後到前的滾動動畫效果。
  在顯示新頁面的時候，原來的頁面也會沿著相同的方向進行視差滾動。

* A separate bottom-up transition style exists when
  pushing a page route where [`PageRoute.fullscreenDialog`][]
  is true. This represents iOS's Present/Modal style
  transition and is typically used on fullscreen modal pages.

  當顯示一個頁面，且 [`PageRoute.fullscreenDialog`][] 是 true 的時候，
  iOS 提供了另外一種自下而上的動畫效果。
  這個動畫通常被用在展示全屏模態頁，
  也被稱為 iOS 上的 Present 轉場動畫或 Modal 轉場動畫。


<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img style="border-radius: 12px;" src="/assets/images/docs/platform-adaptations/navigation-android.gif" class="figure-img img-fluid" alt="An animation of the bottom-up page transition on Android" />
        <figcaption class="figure-caption">
<!--           <span><t>Android page transition</t><t>Android 轉場動畫</t></span> -->
           <span>Android 轉場動畫</span>
        </figcaption>
      </figure>
    </div>
    <div class="col-sm text-center">
      <figure class="figure">
        <img style="border-radius: 22px;" src="/assets/images/docs/platform-adaptations/navigation-ios.gif" class="figure-img img-fluid" alt="An animation of the end-start style push page transition on iOS" />
        <figcaption class="figure-caption">
<!--           <span><t>iOS push transition</t><t>iOS Push 轉場動畫</t></span> -->
          <span>iOS Push 轉場動畫</span>
        </figcaption>
      </figure>
    </div>
    <div class="col-sm text-center">
      <figure class="figure">
        <img style="border-radius: 22px;" src="/assets/images/docs/platform-adaptations/navigation-ios-modal.gif" class="figure-img img-fluid" alt="An animation of the bottom-up style present page transition on iOS" />
        <figcaption class="figure-caption">
<!--           <span><t>iOS present transition</t><t>iOS Present 轉場動畫</t></span> -->
          <span>iOS Present 轉場動畫</span>
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Platform-specific transition details

### 不同平台的轉場動畫細節

On **Android**, Flutter uses the [`ZoomPageTransitionsBuilder`][] animation.
When the user taps on an item, the UI zooms in to a screen that features that item.
When the user taps to go back, the UI zooms out to the previous screen.

**Android** 平臺上，Flutter 使用 [`ZoomPageTransitionsBuilder`][] 轉場動畫。
當用戶進行了路由跳轉，介面會縮放至下一個頁面。
當用戶返回上一頁時，介面會縮放回上一個頁面。

On **iOS** when the push style transition is used,
Flutter's bundled [`CupertinoNavigationBar`][]
and [`CupertinoSliverNavigationBar`][] nav bars
automatically animate each subcomponent to its corresponding
subcomponent on the next or previous page's
`CupertinoNavigationBar` or `CupertinoSliverNavigationBar`.

當在 **iOS** 平臺上使用 Push 轉場特效的時候，
Flutter 內建的 [`CupertinoNavigationBar`][]
和 [`CupertinoSliverNavigationBar`][]
會自動的給當前頁下一頁的子元件使用正確的動畫效果
（`CupertinoNavigationBar` 或者 `CupertinoSliverNavigationBar`）。

<div class="container">
  <div class="row">
    <div class="col-sm">
      <figure class="figure text-center">
      <object style="border-radius: 12px; height: 400px;" class="figure-img img-fluid" height="400" width="185" alt="An animation of the page transition on Android" data="/assets/images/docs/platform-adaptations/android-zoom-animation.png"></object>
        <figcaption class="figure-caption">
          Android
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img style="border-radius: 22px;" src="/assets/images/docs/platform-adaptations/navigation-ios-nav-bar.gif" class="figure-img img-fluid" alt="An animation of the nav bar transitions during a page transition on iOS" />
        <figcaption class="figure-caption">
          iOS Nav Bar
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Back navigation

### 返回導航

On **Android**,
the OS back button, by default, is sent to Flutter
and pops the top route of the [`WidgetsApp`][]'s Navigator.

**Android** 平台，通常作業系統的返回按鈕觸發的事件會發給 Flutter，
並彈出  [`WidgetsApp`][] 路由的最頂端。

On **iOS**,
an edge swipe gesture can be used to pop the top route.

**iOS** 平台，從螢幕邊緣的輕掃手勢會彈出路由的最頂端。

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img style="border-radius: 12px;" src="/assets/images/docs/platform-adaptations/navigation-android-back.gif" class="figure-img img-fluid" alt="A page transition triggered by the Android back button" />
        <figcaption class="figure-caption">
          Android back button
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img style="border-radius: 22px;" src="/assets/images/docs/platform-adaptations/navigation-ios-back.gif" class="figure-img img-fluid" alt="A page transition triggered by an iOS back swipe gesture" />
        <figcaption class="figure-caption">
          iOS back swipe gesture
        </figcaption>
      </figure>
    </div>
  </div>
</div>

## Scrolling

## 滾動

Scrolling is an important part of the platform's
look and feel, and Flutter automatically adjusts
the scrolling behavior to match the current platform.

滾動是不同平台提供獨有體驗非常重要的一環，
Flutter 會根據當前的平台自動適配滾動體驗。

### Physics simulation

### 物理模擬

Android and iOS both have complex scrolling physics
simulations that are difficult to describe verbally.
Generally, iOS's scrollable has more weight and
dynamic friction but Android has more static friction.
Therefore iOS gains high speed more gradually but stops
less abruptly and is more slippery at slow speeds.

Android 和 iOS 平台都提供了非常複雜的滾動物理模擬，
因而很難用語言來描述。通常來說，
iOS 的滾動通常提供更多的分量和動態的阻力；
而 Android 則更多的使用靜態的阻力。
所以，iOS 隨著滾動慢慢的達到高速，且不會突然的停止，
而且在慢速的時候顯得更順滑。

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/scroll-soft.gif" class="figure-img img-fluid rounded" alt="A soft fling where the iOS scrollable slid longer at lower speed than Android" />
        <figcaption class="figure-caption">
          Soft fling comparison
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/scroll-medium.gif" class="figure-img img-fluid rounded" alt="A medium force fling where the Android scrollable reached speed faster and stopped more abruptly after reaching a longer distance" />
        <figcaption class="figure-caption">
          Medium fling comparison
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/scroll-strong.gif" class="figure-img img-fluid rounded" alt="A strong fling where the Android scrollable reach speed faster and reached significantly more distance" />
        <figcaption class="figure-caption">
          Strong fling comparison
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Overscroll behavior

### 滾動邊界行為

On **Android**,
scrolling past the edge of a scrollable shows an
[overscroll glow indicator][] (based on the color
of the current Material theme).

**Android** 平台，滾動達到邊界的時候，會顯示
[滾動灰色指示][overscroll glow indicator]
（具體顏色根據 Material 主題而有所不同）。

On **iOS**, scrolling past the edge of a scrollable
[overscrolls][] with increasing resistance and snaps back.

**iOS** 平台，滾動達到邊界的時候，會顯示一個 [滾動邊界][overscrolls] 的彈簧效果。

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/scroll-overscroll.gif" class="figure-img img-fluid rounded" alt="Android and iOS scrollables being flung past their edge and exhibiting platform specific overscroll behavior" />
        <figcaption class="figure-caption">
          Dynamic overscroll comparison
        </figcaption>
      </figure>
    </div>
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/scroll-static-overscroll.gif" class="figure-img img-fluid rounded" alt="Android and iOS scrollables being overscrolled from a resting position and exhibiting platform specific overscroll behavior" />
        <figcaption class="figure-caption">
          Static overscroll comparison
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Momentum

### 動量

On **iOS**,
repeated flings in the same direction stacks momentum
and builds more speed with each successive fling.
There is no equivalent behavior on *Android*.

**iOS** 平台，不停的按相同方向滾動會產生動量疊加，
從而連續滾動速度會越來越快。在 *Android* 平臺上沒有對應的行為。

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/scroll-momentum-ios.gif" class="figure-img img-fluid rounded" alt="Repeated scroll flings building momentum on iOS" />
        <figcaption class="figure-caption">
          iOS scroll momentum
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Return to top

### 返回頂部

On **iOS**,
tapping the OS status bar scrolls the primary
scroll controller to the top position.
There is no equivalent behavior on **Android**.

**iOS** 平台，點選作業系統的狀態列，主要的捲軸控制器會滾動到頂部。 **Android** 沒有對應的行為。

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img style="border-radius: 22px;" src="/assets/images/docs/platform-adaptations/scroll-tap-to-top-ios.gif" class="figure-img img-fluid" alt="Tapping the status bar scrolls the primary scrollable back to the top" />
        <figcaption class="figure-caption">
          iOS status bar tap to top
        </figcaption>
      </figure>
    </div>
  </div>
</div>

## Typography

## 排版

When using the Material package,
the typography automatically defaults to the
font family appropriate for the platform.
Android uses the Roboto font.
iOS uses the San Francisco font.

當使用 Material package 的時候，排版會根據平台自動使用對應的字型。
Android 平台會使用 Roboto 字型，
而 iOS 則會使用系統自帶的 San Francisco 字型。

When using the Cupertino package, the [default theme][]
uses the San Francisco font.

當使用 Cupertino 套件的時候，[預設主題][default theme] 
會使用 San Francisco 字型。

The San Francisco font license limits its usage to
software running on iOS, macOS, or tvOS only.
Therefore a fallback font is used when running on Android
if the platform is debug-overridden to iOS or the
default Cupertino theme is used.

San Francisco 字型的授權限制了它只能被用在運行於
iOS、macOS 和 tvOS 平臺上的軟體。
因此當執行在 Android 平台的時候，
即使強制覆蓋系統平台為 iOS 或者使用 Cupertino 預設主題，
都會使用對應的替代字型。

You might choose to adapt the text styling of Material 
widgets to match the default text styling on iOS. 
You can see widget-specific examples in the 
[UI Component section][].

你可以選擇將 Material widgets 的文字樣式適配到 iOS 的預設文字樣式。
你可以在 [UI 元件部分][UI Component section] 看到特定元件的例子。

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/typography-android.png" class="figure-img img-fluid rounded" alt="Roboto font on Android" />
        <figcaption class="figure-caption">
          Roboto on Android
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/typography-ios.png" class="figure-img img-fluid rounded" alt="San Francisco font on iOS" />
        <figcaption class="figure-caption">
          San Francisco on iOS
        </figcaption>
      </figure>
    </div>
  </div>
</div>

## Iconography

## 圖示

When using the Material package,
certain icons automatically show different
graphics depending on the platform.
For instance, the overflow button's three dots
are horizontal on iOS and vertical on Android.
The back button is a simple chevron on iOS and
has a stem/shaft on Android.

當使用 Material 套件的時候，根據平台不同，圖示的具體樣式會有差別。
舉例來說，更多按鈕的圖示，Android 上是豎直的三個點而 iOS 是橫著的三個點；
退回按鈕，iOS 是一個簡單的 V 型標記，而 Android 平台，V 型標記有個短橫線。

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/iconography-android.png" class="figure-img img-fluid rounded" alt="Android appropriate icons" />
        <figcaption class="figure-caption">
          Icons on Android
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/iconography-ios.png" class="figure-img img-fluid rounded" alt="iOS appropriate icons" />
        <figcaption class="figure-caption">
          Icons on iOS
        </figcaption>
      </figure>
    </div>
  </div>
</div>

The material library also provides a set of platform-adaptive icons through [`Icons.adaptive`].

## Haptic feedback

## 觸控反饋

The Material and Cupertino packages automatically
trigger the platform appropriate haptic feedback in
certain scenarios.

Material 和 Cupertino 包在特定場景下都會自動觸發符合平台特點的觸控反饋。

For instance,
a word selection via text field long-press triggers a 'buzz'
vibrate on Android and not on iOS.

例如，在文字輸入框控制項裡面長按選中單詞會在 Android 裝置上會觸發震動，而 iOS 不會。

Scrolling through picker items on iOS triggers a
'light impact' knock and no feedback on Android.

在 iOS 滾動選擇器專案列表，會觸發一個很輕的敲擊音效，而 Android 則不會。

## Text editing

## 文字編輯

Flutter also makes the below adaptations while editing
the content of text fields to match the current platform.

Flutter 會根據當前平台來適配正確的文字編輯體驗。

### Keyboard gesture navigation

### 鍵盤手勢導航

On **Android**,
horizontal swipes can be made on the soft keyboard's <kbd>space</kbd> key
to move the cursor in Material and Cupertino text fields.

**Android** 平台，
在虛擬鍵盤空格鍵上可以透過左右輕掃來移動游標，
Material 和 Cupertino 的文字輸入框控制項都支援該特性。

On **iOS** devices with 3D Touch capabilities,
a force-press-drag gesture could be made on the soft
keyboard to move the cursor in 2D via a floating cursor.
This works on both Material and Cupertino text fields.

**iOS** 裝置提供了 3D Touch 相容，
透過在虛擬鍵盤上使用長按並拖拽手勢可以任意方向移動游標。
Material 和 Cupertino 都對這個功能提供了支援。

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/text-keyboard-move-android.gif" class="figure-img img-fluid rounded" alt="Moving the cursor via the space key on Android" />
        <figcaption class="figure-caption">
          Android space key cursor move
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/text-keyboard-move-ios.gif" class="figure-img img-fluid rounded" alt="Moving the cursor via 3D Touch drag on the keyboard on iOS" />
        <figcaption class="figure-caption">
          iOS 3D Touch drag cursor move
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Text selection toolbar

### 文字選中工具欄

With **Material on Android**,
the Android style selection toolbar is shown when
a text selection is made in a text field.

在 **Android** 平臺上使用 **Material**，在文字輸入框裡面選中文字會顯示一個 Android 風格的文字選中工具欄。

With **Material on iOS** or when using **Cupertino**,
the iOS style selection toolbar is shown when a text
selection is made in a text field.

在 **iOS** 平臺上使用 **Material** 或者在兩個平臺上都使用 **Cupertino**，在文字輸入框裡面選中文字會展示一個 iOS 風格的文字選中工具欄。

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/text-toolbar-android.png" class="figure-img img-fluid rounded" alt="Android appropriate text toolbar" />
        <figcaption class="figure-caption">
          Android text selection toolbar
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/text-toolbar-ios.png" class="figure-img img-fluid rounded" alt="iOS appropriate text toolbar" />
        <figcaption class="figure-caption">
          iOS text selection toolbar
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Single tap gesture

### 點選手勢

With **Material on Android**,
a single tap in a text field puts the cursor at the
location of the tap.

在 **Android** 平台使用 **Material**，在文字控制項中點選會移動游標到點選處。

A collapsed text selection also shows a draggable
handle to subsequently move the cursor.

同時，游標會有一個可移動的把手，隨後可以透過這個把手移動游標。

With **Material on iOS** or when using **Cupertino**,
a single tap in a text field puts the cursor at the
nearest edge of the word tapped.

在 **iOS** 平台使用 **Material** 或者在兩個平台都使用 **Cupertino**，在文字空間中點選，會把游標移動到點選處最近的單詞末尾。

Collapsed text selections don't have draggable handles on iOS.

在 iOS 平臺上，游標是沒有把手的。

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/text-single-tap-android.gif" class="figure-img img-fluid rounded" alt="Moving the cursor to the tapped position on Android" />
        <figcaption class="figure-caption">
          Android tap
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/text-single-tap-ios.gif" class="figure-img img-fluid rounded" alt="Moving the cursor to the nearest edge of the tapped word on iOS" />
        <figcaption class="figure-caption">
          iOS tap
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Long-press gesture

### 長按手勢

With **Material on Android**,
a long press selects the word under the long press.
The selection toolbar is shown upon release.

在 **Android** 平台使用 **Material**，在單詞上長按會選中單詞，並在釋放長按的時候顯示文字選中工具欄。

With **Material on iOS** or when using **Cupertino**,
a long press places the cursor at the location of the
long press. The selection toolbar is shown upon release.

在 **iOS** 平台使用 **Material** 或者在兩個平台都使用 **Cupertino**，長按會把游標放置到長按的位置，並在釋放長按的時候顯示文字選中工具欄。

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/text-long-press-android.gif" class="figure-img img-fluid rounded" alt="Selecting a word via long press on Android" />
        <figcaption class="figure-caption">
          Android long press
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/text-long-press-ios.gif" class="figure-img img-fluid rounded" alt="Selecting a position via long press on iOS" />
        <figcaption class="figure-caption">
          iOS long press
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Long-press drag gesture

### 長按並拖放手勢

With **Material on Android**,
dragging while holding the long press expands the words selected.

在 **Android** 平臺上使用 **Material**，長按並拖拽會選中更多單詞。

With **Material on iOS** or when using **Cupertino**,
dragging while holding the long press moves the cursor.

在 **iOS** 平台使用 **Material** 或者在兩個平台都使用 **Cupertino**，長按並拖拽會移動游標。

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/text-long-press-drag-android.gif" class="figure-img img-fluid rounded" alt="Expanding word selection via long press drag on Android" />
        <figcaption class="figure-caption">
          Android long press drag
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/text-long-press-drag-ios.gif" class="figure-img img-fluid rounded" alt="Moving the cursor via long press drag on iOS" />
        <figcaption class="figure-caption">
          iOS long press drag
        </figcaption>
      </figure>
    </div>
  </div>
</div>

### Double tap gesture

### 雙擊手勢

On both Android and iOS,
a double tap selects the word receiving the
double tap and shows the selection toolbar.

Android 和 iOS 平臺上，
雙擊選中一個單詞都會收到雙擊手勢事件，
並顯示文字選中工具欄。

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/text-double-tap-android.gif" class="figure-img img-fluid rounded" alt="Selecting a word via double tap on Android" />
        <figcaption class="figure-caption">
          Android double tap
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/text-double-tap-ios.gif" class="figure-img img-fluid rounded" alt="Selecting a word via double tap on iOS" />
        <figcaption class="figure-caption">
          iOS double tap
        </figcaption>
      </figure>
    </div>
  </div>
</div>

## UI components

This section includes preliminary recommendations on how to adapt 
Material widgets to deliver a natural and compelling experience on iOS. 
Your feedback is welcomed on [issue #8427][]. 

### Widgets with .adaptive() constructors

Several widgets support `.adaptive()` constructors. 
The following table lists these widgets.
Adaptive constructors substitute the corresponding Cupertino components 
when the app is run on an iOS device. 

Widgets in the following table are used primarily for input, 
selection, and to display system information. 
Because these controls are tightly integrated with the operating system,
users have been trained to recognize and respond to them.
Therefore, we recommend that you follow platform conventions. 


| Material Widget | Cupertino Widget | Adaptive Constructor |
|---|---|---|---|---|
|<img width=160 src="/assets/images/docs/platform-adaptations/m3-switch.png" class="figure-img img-fluid rounded" alt="Switch in Material 3" /><br/>`Switch`|<img src="/assets/images/docs/platform-adaptations/hig-switch.png" class="figure-img img-fluid rounded" alt="Switch in HIG" /><br/>`CupertinoSwitch`|[`Switch.adaptive()`][]|
|<img src="/assets/images/docs/platform-adaptations/m3-slider.png" width =160 class="figure-img img-fluid rounded" alt="Slider in Material 3" /><br/>`Slider`|<img src="/assets/images/docs/platform-adaptations/hig-slider.png"  width =160  class="figure-img img-fluid rounded" alt="Slider in HIG" /><br/>`CupertinoSlider`|[`Slider.adaptive()`][]|
|<img src="/assets/images/docs/platform-adaptations/m3-progress.png" width = 100 class="figure-img img-fluid rounded" alt="Circular progress indicator in Material 3" /><br/>`CircularProgressIndicator`|<img src="/assets/images/docs/platform-adaptations/hig-progress.png" class="figure-img img-fluid rounded" alt="Activity indicator in HIG" /><br/>`CupertinoActivityIndicator`|[`CircularProgressIndicator.adaptive()`][]|
| <img src="/assets/images/docs/platform-adaptations/m3-checkbox.png" class="figure-img img-fluid rounded" alt=" Checkbox in Material 3" /> <br/>`Checkbox`| <img src="/assets/images/docs/platform-adaptations/hig-checkbox.png" class="figure-img img-fluid rounded" alt="Checkbox in HIG" /> <br/> `CupertinoCheckbox`|[`Checkbox.adaptive()`][]|
|<img src="/assets/images/docs/platform-adaptations/m3-radio.png" class="figure-img img-fluid rounded" alt="Radio in Material 3" /> <br/>`Radio`|<img src="/assets/images/docs/platform-adaptations/hig-radio.png" class="figure-img img-fluid rounded" alt="Radio in HIG" /><br/>`CupertinoRadio`|[`Radio.adaptive()`][]|

### Top app bar and navigation bar

Since Android 12, the default UI for top app 
bars follows the design guidelines defined in [Material 3][mat-appbar]. 
On iOS, an equivalent component called "Navigation Bars" 
is defined in [Apple's Human Interface Guidelines][hig-appbar] (HIG). 

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/mat-appbar.png" 
        class="figure-img img-fluid rounded" alt=" Top App Bar in Material 3 " />
        <figcaption class="figure-caption">
          Top App Bar in Material 3 
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/hig-appbar.png" 
        class="figure-img img-fluid rounded" alt="Navigation Bar in Human Interface Guidelines" />
        <figcaption class="figure-caption">
          Navigation Bar in Human Interface Guidelines
        </figcaption>
      </figure>
    </div>
  </div>
</div>

Certain properties of app bars in Flutter apps should be adapted, 
like system icons and page transitions. 
These are already automatically adapted when using 
the Material `AppBar` and `SliverAppBar` widgets. 
You can also further customize the properties of these widgets to better 
match iOS platform styles, as shown below. 

```dart
// Map the text theme to iOS styles
TextTheme cupertinoTextTheme = TextTheme(
    headlineMedium: CupertinoThemeData()
        .textTheme
        .navLargeTitleTextStyle
         // fixes a small bug with spacing
        .copyWith(letterSpacing: -1.5),
    titleLarge: CupertinoThemeData().textTheme.navTitleTextStyle)
...

// Use iOS text theme on iOS devices
ThemeData(
      textTheme: Platform.isIOS ? cupertinoTextTheme : null,
      ...
)
...

// Modify AppBar properties
AppBar(
        surfaceTintColor: Platform.isIOS ? Colors.transparent : null,
        shadowColor: Platform.isIOS ? CupertinoColors.darkBackgroundGray : null,
        scrolledUnderElevation: Platform.isIOS ? .1 : null,
        toolbarHeight: Platform.isIOS ? 44 : null,
        ...
      ),

```

But, because app bars are displayed alongside 
other content in your page, it's only recommended to adapt the styling 
so long as its cohesive with the rest of your application. You can see 
additional code samples and a further explanation in 
[the GitHub discussion on app bar adaptations][appbar-post]. 

### Bottom navigation bars

Since Android 12, the default UI for bottom navigation 
bars follow the design guidelines defined in [Material 3][mat-navbar]. 
On iOS, an equivalent component called "Tab Bars" 
is defined in [Apple's Human Interface Guidelines][hig-tabbar] (HIG). 

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/mat-navbar.png" 
        class="figure-img img-fluid rounded" alt="Bottom Navigation Bar in Material 3 " />
        <figcaption class="figure-caption">
          Bottom Navigation Bar in Material 3 
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/hig-tabbar.png" 
        class="figure-img img-fluid rounded" alt="Tab Bar in Human Interface Guidelines" />
        <figcaption class="figure-caption">
         Tab Bar in Human Interface Guidelines
        </figcaption>
      </figure>
    </div>
  </div>
</div>

Since tab bars are persistent across your app, they should match your
own branding. However, if you choose to use Material's default 
styling on Android, you might consider adapting to the default iOS
tab bars.

To implement platform-specific bottom navigation bars, 
you can use Flutter's `NavigationBar` widget on Android 
and the `CupertinoTabBar` widget on iOS. 
Below is a code snippet you can 
adapt to show a platform-specific navigation bars.

```dart
final Map<String, Icon> _navigationItems = {
    'Menu': Platform.isIOS ? Icon(CupertinoIcons.house_fill) : Icon(Icons.home),
    'Order': Icon(Icons.adaptive.share),
  };

...

Scaffold(
  body: _currentWidget,
  bottomNavigationBar: Platform.isIOS
          ? CupertinoTabBar(
              currentIndex: _currentIndex,
              onTap: (index) {
                setState(() => _currentIndex = index);
                _loadScreen();
              },
              items: _navigationItems.entries
                  .map<BottomNavigationBarItem>(
                      (entry) => BottomNavigationBarItem(
                            icon: entry.value,
                            label: entry.key,
                          ))
                  .toList(),
            )
          : NavigationBar(
              selectedIndex: _currentIndex,
              onDestinationSelected: (index) {
                setState(() => _currentIndex = index);
                _loadScreen();
              },
              destinations: _navigationItems.entries
                  .map<Widget>((entry) => NavigationDestination(
                        icon: entry.value,
                        label: entry.key,
                      ))
                  .toList(),
            ));
```
### Text fields

Since Android 12, text fields follow the
[Material 3][m3-text-field] (M3) design guidelines. 
On iOS, Apple's [Human Interface Guidelines][hig-text-field] (HIG) define
an equivalent component. 

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/m3-text-field.png" 
        class="figure-img img-fluid rounded" alt="Text Field in Material 3" />
        <figcaption class="figure-caption">
          Text Field in Material 3
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/hig-text-field.png" 
        class="figure-img img-fluid rounded" alt="Text Field in Human Interface Guidelines" />
        <figcaption class="figure-caption">
          Text Field in HIG
        </figcaption>
      </figure>
    </div>
  </div>
</div>

Since text fields require user input,  
their design should follow platform conventions. 

To implement a platform-specific `TextField` 
in Flutter, you can adapt the styling of the 
Material `TextField`.

```dart
Widget _createAdaptiveTextField() {
  final _border = OutlineInputBorder(
    borderSide: BorderSide(color: CupertinoColors.lightBackgroundGray),
  );

  final iOSDecoration = InputDecoration(
    border: _border,
    enabledBorder: _border,
    focusedBorder: _border,
    filled: true,
    fillColor: CupertinoColors.white,
    hoverColor: CupertinoColors.white,
    contentPadding: EdgeInsets.fromLTRB(10, 0, 0, 0),
  );

  return Platform.isIOS
      ? SizedBox(
          height: 36.0,
          child: TextField(
            decoration: iOSDecoration,
          ),
        )
      : TextField();
}
```

To learn more about adapting text fields, check out 
[the GitHub discussion on text fields][text-field-post].
You can leave feedback or ask questions in the discussion.

### Alert dialog

Since Android 12, the default UI of alert dialogs 
(also known as a "basic dialog") follows the design guidelines 
defined in [Material 3][m3-dialog] (M3). 
On iOS, an equivalent component called "alert" is defined in Apple's 
[Human Interface Guidelines][hig-alert] (HIG).

<div class="container">
  <div class="row">
    <div class="col-sm text-center">
      <figure class="figure">
        <img src="/assets/images/docs/platform-adaptations/m3-alert.png" 
        class="figure-img img-fluid rounded" alt="Basic Dialog in Material 3" />
        <figcaption class="figure-caption">
          Basic Dialog in M3
        </figcaption>
      </figure>
    </div>
    <div class="col-sm">
      <figure class="figure text-center">
        <img src="/assets/images/docs/platform-adaptations/cupertino-alert.png" 
        class="figure-img img-fluid rounded" alt="Alert in Human Interface Guidelines" />
        <figcaption class="figure-caption">
          Alert in HIG
        </figcaption>
      </figure>
    </div>
  </div>
</div>

Since alert dialogs are often tightly integrated with the operating system, 
their design generally needs to follow the platform conventions. 
This is especially important when a dialog is used to request user input 
about security, privacy, and destructive operations (e.g., deleting files 
permanently). As an exception, a branded alert dialog design can be used on 
non-critical user flows to highlight specific information or messages.

To implement platform-specific alert dialogs, 
you can use Flutter's `AlertDialog` widget on Android 
and the `CupertinoAlertDialog` widget on iOS. Below is a code snippet you can 
adapt to show a platform-specific alert dialog.

```dart
void _showAdaptiveDialog(
  context, {
  required Text title,
  required Text content,
  required List<Widget> actions,
}) {
  Platform.isIOS || Platform.isMacOS
      ? showCupertinoDialog<String>(
          context: context,
          builder: (BuildContext context) => CupertinoAlertDialog(
            title: title,
            content: content,
            actions: actions,
          ),
        )
      : showDialog(
          context: context,
          builder: (BuildContext context) => AlertDialog(
            title: title,
            content: content,
            actions: actions,
          ),
        );
}
```

To learn more about adapting alert dialogs, check out 
[the GitHub discussion on dialog adaptations][alert-post].
You can leave feedback or ask questions in the discussion.


[issue #8410]: {{site.repo.flutter}}/issues/8410#issuecomment-468034023
[android.app.AlertDialog]: {{site.android-dev}}/reference/android/app/AlertDialog.html
[`ZoomPageTransitionsBuilder`]: {{site.api}}/flutter/material/ZoomPageTransitionsBuilder-class.html
[`CupertinoNavigationBar`]: {{site.api}}/flutter/cupertino/CupertinoNavigationBar-class.html
[`CupertinoSliverNavigationBar`]: {{site.api}}/flutter/cupertino/CupertinoSliverNavigationBar-class.html
[default theme]: {{site.repo.flutter}}/blob/master/packages/flutter/lib/src/cupertino/text_theme.dart
[Material/Cupertino adaptive widget problem definition]: http://bit.ly/flutter-adaptive-widget-problem
[`Navigator.push()`]: {{site.api}}/flutter/widgets/Navigator/push.html
[overscroll glow indicator]: {{site.api}}/flutter/widgets/GlowingOverscrollIndicator-class.html
[overscrolls]: {{site.api}}/flutter/widgets/BouncingScrollPhysics-class.html
[`PageRoute.fullscreenDialog`]: {{site.api}}/flutter/widgets/PageRoute-class.html
[platform_design code samples]: {{site.github}}/flutter/samples/tree/main/platform_design
[`Icons.adaptive`]: {{site.api}}/flutter/material/PlatformAdaptiveIcons-class.html
[slides and clip-reveals up]: {{site.api}}/flutter/material/OpenUpwardsPageTransitionsBuilder-class.html
[slides up and fades in]: {{site.api}}/flutter/material/FadeUpwardsPageTransitionsBuilder-class.html
[`startActivity()`]: {{site.android-dev}}/reference/android/app/Activity.html#startActivity(android.content.Intent
[`WidgetsApp`]: {{site.api}}/flutter/widgets/WidgetsApp-class.html
[issue #8427]: {{site.repo.this}}/issues/8427
[m3-dialog]: https://m3.material.io/components/dialogs/overview
[hig-alert]: https://developer.apple.com/design/human-interface-guidelines/components/presentation/alerts/
[alert-post]: {{site.repo.uxr}}/discussions/92
[appbar-post]: {{site.repo.uxr}}/discussions/93
[mat-appbar]: https://m3.material.io/components/top-app-bar/overview
[hig-appbar]: https://developer.apple.com/design/human-interface-guidelines/components/navigation-and-search/navigation-bars/
[`Checkbox.adaptive()`]: {{site.api}}/flutter/material/Checkbox/Checkbox.adaptive.html
[`Radio.adaptive()`]: {{site.api}}/flutter/material/Radio/Radio.adaptive.html
[`Switch.adaptive()`]: {{site.api}}/flutter/material/Switch/Switch.adaptive.html
[`Slider.adaptive()`]: {{site.api}}/flutter/material/Slider/Slider.adaptive.html
[`CircularProgressIndicator.adaptive()`]: {{site.api}}/flutter/material/CircularProgressIndicator/CircularProgressIndicator.adaptive.html
[UI Component section]: {{site.api}}/platform-integration/platform-adaptations/#ui-components
[mat-navbar]: https://m3.material.io/components/navigation-bar/overview
[hig-tabbar]: https://developer.apple.com/design/human-interface-guidelines/components/navigation-and-search/tab-bars/
[text-field-post]: {{site.repo.uxr}}/discussions/95 
[m3-text-field]: https://m3.material.io/components/text-fields/overview
[hig-text-field]: https://developer.apple.com/design/human-interface-guidelines/text-fields
