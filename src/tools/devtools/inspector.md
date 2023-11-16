---
title: Using the Flutter inspector
title: 使用 Flutter inspector 工具
description: Learn how to use the Flutter inspector to explore a Flutter app's widget tree.
description: 學習如何使用 Flutter inspector 來檢查 Flutter 應用的 widget 樹。
tags: Flutter開發工具,DevTools
keywords: Flutter inspector,widget 樹
---

<?code-excerpt path-base="../examples/visual_debugging/"?>

{{site.alert.note}}

  The inspector works with all Flutter applications.
  
  Flutter inspector 適用於所有 Flutter 應用。
  
{{site.alert.end}}

## What is it?

## 這是什麼？

The Flutter widget inspector is a powerful tool for visualizing and
exploring Flutter widget trees. The Flutter framework uses widgets
as the core building block for anything from controls
(such as text, buttons, and toggles),
to layout (such as centering, padding, rows, and columns).
The inspector helps you visualize and explore Flutter widget
trees, and can be used for the following:

Flutter widget inspector 是一個強大的工具，用於視覺化和檢視 widget 樹。
Flutter 框架層使用 widgets 作為核心建構模組來處理從控制項
（例如文字、按鈕和切換等）到佈局（例如居中、填充、行和列等）的所有內容。
Flutter inspector 不僅可以幫助你視覺化檢視 Flutter widget 樹，還有其他的作用：

* understanding existing layouts

  瞭解現有佈局

* diagnosing layout issues

  診斷佈局問題

![Screenshot of the Flutter inspector window]({{site.url}}/assets/images/docs/tools/devtools/inspector_screenshot.png){:width="100%"}

## Get started

## 開始使用

To debug a layout issue, run the app in [debug mode][] and
open the inspector by clicking the **Flutter Inspector**
tab on the DevTools toolbar.

要除錯佈局問題，請在 [Debug 模式][debug mode] 下執行應用程式，
然後點選 DevTools 工具欄上的 **Flutter inspector** 選項開啟除錯面板。

{{site.alert.note}}

  You can still access the Flutter inspector directly from
  Android Studio/IntelliJ, but you might prefer the
  more spacious view when running it from DevTools
  in a browser.
  
  你可以直接在 Android Studio/IntelliJ 中使用 Flutter inspector，
  但是你可能會更喜歡使用 DevTools 在瀏覽器中開啟 Flutter inspector，這樣你能得到更寬敞的檢視。
  
{{site.alert.end}}

### Debugging layout issues visually

### 視覺化地除錯佈局問題

The following is a guide to the features available in the
inspector's toolbar. When space is limited, the icon is
used as the visual version of the label.

下面是 Flutter inspector 工具欄中可用功能的指南。
當空間有限時，將直接使用圖示展示。

<dl markdown="1">
<dt>
<p markdown="1">![Select widget mode icon]({{site.url}}/assets/images/docs/tools/devtools/select-widget-mode-icon.png){:width="20px"} **Select widget mode**</p>
<p markdown="1">![Select widget mode icon]({{site.url}}/assets/images/docs/tools/devtools/select-widget-mode-icon.png){:width="20px"} **選擇 widget 模式**</p>
</dt>
<dd markdown="1">
<p markdown="1">Enable this button in order to select
    a widget on the device to inspect it. For more information,
    see [Inspecting a widget](#inspecting-a-widget).</p>
<p markdown="1">啟用此按鈕以在裝置上選擇 widget 進行檢視。
    有關更多資訊，請參考 [檢視 widget](#inspecting-a-widget)。</p>
</dd>
<dt>
<p markdown="1">![Refresh tree icon]({{site.url}}/assets/images/docs/tools/devtools/refresh-tree-icon.png){:width="20px"} **Refresh tree**</p>
<p markdown="1">![Refresh tree icon]({{site.url}}/assets/images/docs/tools/devtools/refresh-tree-icon.png){:width="20px"} **重新整理樹**</p>
</dt>
<dd>
<p>Reload the current widget info</p>
<p>重新載入當前 widget 的資訊。</p>
</dd>
<dt>
<p markdown="1">![Slow animations icon]({{site.url}}/assets/images/docs/tools/devtools/slow-animations-icon.png){:width="20px"} **[Slow Animations]**</p>
<p markdown="1">![Slow animations icon]({{site.url}}/assets/images/docs/tools/devtools/slow-animations-icon.png){:width="20px"} **[慢速動畫][Slow Animations]**</p>
</dt>
<dd>
<p>Run animations 5 times slower to help fine-tune them.</p>
<p>以五分之一的速度執行動畫以便對它們進行最佳化。</p>
</dd>
<dt>
<p markdown="1">![Show guidelines mode icon]({{site.url}}/assets/images/docs/tools/devtools/debug-paint-mode-icon.png){:width="20px"} **[Show guidelines][]**</p>
<p markdown="1">![Show guidelines mode icon]({{site.url}}/assets/images/docs/tools/devtools/debug-paint-mode-icon.png){:width="20px"} **[顯示引導線][Show guidelines]**</p>
</dt>
<dd>
<p>Overlay guidelines to assist with fixing layout issues.</p>
<p>覆蓋一層引導線以幫助調整佈局問題。</p>
</dd>
<dt>
<p markdown="1">![Show baselines icon]({{site.url}}/assets/images/docs/tools/devtools/paint-baselines-icon.png){:width="20px"} **[Show baselines][]**</p>
<p markdown="1">![Show baselines icon]({{site.url}}/assets/images/docs/tools/devtools/paint-baselines-icon.png){:width="20px"} **[顯示基線][Show baselines]**</p>
</dt>
<dd>
<p>Show baselines, which are used for aligning text.
    Can be useful for checking if text is aligned.</p>
<p>針對文字對齊展示文字的基線。對檢查文字是否對齊有幫助。</p>
</dd>
<dt>
<p markdown="1">![Highlight repaints icon]({{site.url}}/assets/images/docs/tools/devtools/repaint-rainbow-icon.png){:width="20px"} **[Highlight repaints][]**</p>
<p markdown="1">![Highlight repaints icon]({{site.url}}/assets/images/docs/tools/devtools/repaint-rainbow-icon.png){:width="20px"} **[高亮重繪製內容][Highlight repaints]**</p>
</dt>
<dd>
<p>Shows rotating colors on layers when repainting.</p>
<p>重新繪製時在圖層上依次顯示不同的顏色。</p>
</dd>
<dt>
<p markdown="1">![Highlight oversized images icon]({{site.url}}/assets/images/docs/tools/devtools/invert_oversized_images_icon.png){:width="20px"} **[Highlight oversized images][]**</p>
<p markdown="1">![Highlight oversized images icon]({{site.url}}/assets/images/docs/tools/devtools/invert_oversized_images_icon.png){:width="20px"} **[高亮尺寸過大的圖片][Highlight oversized images]**</p>
</dt>
<dd>
<p>Highlights images that are using too much memory
    by inverting colors and flipping them.</p>
<p>在執行的應用程式中高亮並反轉消耗過多記憶體的圖像。</p>
</dd>

[Slow animations]: #slow-animations
[Show guidelines]: #show-guidelines
[Show baselines]: #show-baselines
[Highlight repaints]: #highlight-repaints
[Highlight oversized images]: #highlight-oversized-images

## Inspecting a widget

## 檢查一個 widget

You can browse the interactive widget tree to view nearby
widgets and see their field values.

你可以瀏覽 widget 樹並檢視其附近的 widgets 和它們的屬性值。

To locate individual UI elements in the widget tree,
click the **Select Widget Mode** button in the toolbar.
This puts the app on the device into a "widget select" mode.
Click any widget in the app's UI; this selects the widget on the
app's screen, and scrolls the widget tree to the corresponding node.
Toggle the **Select Widget Mode** button again to exit
widget select mode.

要在 widget 樹中找到單個 UI 元素，
請點選工具欄中的 **Select Widget Mode** 按鈕。
這將使裝置上的應用程式進入「widget select」模式。
點選應用介面上的任何 widget，將選中 widget 並將 widget 樹滾動到對應的節點。
再次點選 **Select Widget Mode** 按鈕則退出「widget select」模式。

When debugging layout issues, the key fields to look at are the
`size` and `constraints` fields. The constraints flow down the tree,
and the sizes flow back up. For more information on how this works,
see [Understanding constraints][].

在除錯佈局問題時，要檢視的關鍵欄位是 `size` 和 `constraints`。
其中約束沿樹結構向下傳遞，尺寸資訊則向上返回。
想要了解更多資訊，可以檢視 [深入理解 Flutter 佈局約束][Understanding constraints]。

## Flutter Layout Explorer

## Flutter 佈局瀏覽器

The Flutter Layout Explorer helps you to better understand
Flutter layouts.

Flutter 佈局瀏覽器可以幫助你更好地理解 Flutter 佈局。

For an overview of what you can do with this tool, see
the Flutter Explorer video:

有關此工具的操作概述，觀看 Flutter Explorer 的介紹影片：

<iframe width="560" height="315" src="{{site.youtube-site}}/embed/Jakrc3Tn_y4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

You might also find the following step-by-step article useful:

下面詳細介紹的文章可能對你有幫助：

* [How to debug layout issues with the Flutter Inspector][debug-article]

  [如何使用 Flutter Inspector 除錯佈局問題][debug-article]

[debug-article]: {{site.flutter-medium}}/how-to-debug-layout-issues-with-the-flutter-inspector-87460a7b9db

### Using the Layout Explorer

### 使用佈局瀏覽器

From the Flutter Inspector, select a widget. The Layout Explorer
supports both [flex layouts][] and fixed size layouts, and has
specific tooling for both kinds.

從 Flutter Inspector 中，選擇一個 widget。
佈局瀏覽器支援 [彈性佈局][flex layouts] 和固定大小的佈局，並且針對它們配備了特定的工具。

#### Flex layouts

#### 彈性佈局

When you select a flex widget (for example, [`Row`][], [`Column`][], [`Flex`][])
or a direct child of a flex widget, the flex layout tool will
appear in the Layout Explorer.

當你選擇了一個彈性佈局 widget（例如，[`Row`][]、[`Column`][]、[`Flex`][]）
或它的子 widget 時，彈性佈局工具將顯示在佈局瀏覽器中。

The Layout Explorer visualizes how [`Flex`][] widgets and their
children are laid out. The explorer identifies the main axis
and cross axis, as well as the current alignment for each
(for example, start, end, and spaceBetween).
It also shows details like flex factor, flex fit, and layout
constraints.

佈局瀏覽器會直觀的顯示 [`Flex`][] widgets 及其子元素的佈局方式。
瀏覽器中還會顯示主軸和交叉軸，以及每個軸當前的對齊方式（例如，start、end 和 spaceBetween）。
它還顯示了諸如彈性係數、彈性適配和佈局約束等詳細資訊。

Additionally, the explorer shows layout constraint violations
and render overflow errors. Violated layout constraints
are colored red, and overflow errors are presented in the
standard  "yellow-tape" pattern, as you might see on a running
device. These visualizations aim to improve understanding of
why overflow errors occur as well as how to fix them.

此外，瀏覽器中還會顯示佈局約束衝突和渲染溢位錯誤。
正如你在裝置上看到的那樣，違背佈局約束的地方會被標記成紅色，溢位錯誤以標準的「黃色條帶」顯示。
這些視覺化的錯誤是為了讓我們更好地理解溢位錯誤發生的原因，
並瞭解如何修復它們。

![The Layout Explorer showing errors and device inspector]({{site.url}}/assets/images/docs/tools/devtools/layout_explorer_errors_and_device.gif){:width="100%"}

Clicking a widget in the layout explorer mirrors
the selection on the on-device inspector. **Select Widget Mode**
needs to be enabled for this. To enable it,
click on the **Select Widget Mode** button in the inspector.

在 **Select Widget Mode** 模式下，點選佈局瀏覽器中的 widget 會同步選擇到裝置上。
啟用此模式，請點選除錯面板中的 **Select Widget Mode** 按鈕。

![The Select Widget Mode button in the inspector]({{site.url}}/assets/images/docs/tools/devtools/select_widget_mode_button.png)

For some properties, like flex factor, flex fit, and alignment,
you can modify the value via dropdown lists in the explorer.
When modifying a widget property, you see the new value reflected
not only in the Layout Explorer, but also on the
device running your Flutter app. The explorer animates
on property changes so that the effect of the change is clear.
Widget property changes made from the layout explorer don't
modify your source code and are reverted on hot reload.

你可以在佈局瀏覽器的下拉列表修改屬性值，
例如彈性係數、彈性適配和對齊方式。
當修改 widget 的屬性時，您會看到新的值同時在瀏覽器和執行 Flutter 應用程式的裝置上生效。
瀏覽器透過動畫使更改的效果清晰可見。
從佈局瀏覽器中對 widget 屬性的更改不會修改原始碼，將在熱重載時還原。

##### Interactive Properties

##### 互動屬性

Layout Explorer supports modifying [`mainAxisAlignment`][],
[`crossAxisAlignment`][], and [`FlexParentData.flex`][].
In the future, we may add support for additional properties
such as [`mainAxisSize`][], [`textDirection`][], and
[`FlexParentData.fit`][].

佈局資源管理器支援修改 [`mainAxisAlignment`][]、[`crossAxisAlignment`][] 和 [`FlexParentData.flex`][]。
將來，我們可能會新增對其他屬性的支援，例如 [`mainAxisSize`][]、[`textDirection`][] 和 [`FlexParentData.fit`][].

###### mainAxisAlignment

![The Layout Explorer changing main axis alignment]({{site.url}}/assets/images/docs/tools/devtools/layout_explorer_main_axis_alignment.gif){:width="100%"}

Supported values:

支援屬性：

* `MainAxisAlignment.start`
* `MainAxisAlignment.end`
* `MainAxisAlignment.center`
* `MainAxisAlignment.spaceBetween`
* `MainAxisAlignment.spaceAround`
* `MainAxisAlignment.spaceEvenly`

###### crossAxisAlignment

![The Layout Explorer changing cross axis alignment]({{site.url}}/assets/images/docs/tools/devtools/layout_explorer_cross_axis_alignment.gif){:width="100%"}

Supported values:

支援屬性：

* `CrossAxisAlignment.start`
* `CrossAxisAlignment.center`
* `CrossAxisAlignment.end`
* `CrossAxisAlignment.stretch`

###### FlexParentData.flex

![The Layout Explorer changing flex factor]({{site.url}}/assets/images/docs/tools/devtools/layout_explorer_flex.gif){:width="100%"}

Layout Explorer supports 7 flex options in the UI
(null, 0, 1, 2, 3, 4, 5), but technically the flex
factor of a flex widget's child can be any int.

佈局瀏覽器支援設定 7 種彈性因子（null、0、1、2、3、4、5），
但從技術上講，彈性 widget 子級的彈性因子可以是任何整數。

###### Flexible.fit

![The Layout Explorer changing fit]({{site.url}}/assets/images/docs/tools/devtools/layout_explorer_fit.gif){:width="100%"}

Layout Explorer supports the two different types of
[`FlexFit`][]: `loose` and `tight`.

佈局瀏覽器支援兩種不同型別的 [`FlexFit`][]：`loose` 和 `tight`。

#### Fixed size layouts

#### 固定大小布局

When you select a fixed size widget that is not a child
of a flex widget, fixed size layout information will appear
in the Layout Explorer. You can see size, constraint, and padding
information for both the selected widget and its nearest upstream
RenderObject.

當您選擇一個固定大小的 widget 而不是彈性 widget 時，它的佈局資訊將顯示在佈局瀏覽器中。
你可以看到所選 widget 及其最近的上一級 RenderObject 的大小、約束和填充資訊。

![The Layout Explorer fixed size tool]({{site.url}}/assets/images/docs/tools/devtools/layout_explorer_fixed_layout.png){:width="100%"}

## Visual debugging

## 除錯視覺效果

The Flutter Inspector provides several options for visually debugging your app.

![Inspector visual debugging options]({{site.url}}/assets/images/docs/tools/devtools/visual_debugging_options.png){:width="100%"}

Flutter Inspector 提供了多種以視覺化方式除錯應用的方式。
以下是在 Flutter DevTools 中的 inspector 可用的選項：

### Slow animations

### 慢速動畫

When enabled, this option runs animations 5 times slower for easier visual
inspection.
This can be useful if you want to carefully observe and tweak an animation that
doesn't look quite right.

啟用時，動畫將以約五分之一的原有速度執行，方便對視覺效果進行檢查。
當你想要仔細地觀察並除錯看起來不正常的動畫時，這個選項會非常有用。

This can also be set in code:

你也可以使用程式碼設定：

<?code-excerpt "lib/slow_animations.dart"?>
```dart
import 'package:flutter/scheduler.dart';

void setSlowAnimations() {
  timeDilation = 5.0;
}
```
 
This slows the animations by 5x.

這會讓動畫時長增加 5 倍（速度減慢 5 倍）。

#### See also

#### 更多內容

The following links provide more info.

以下的連結提供了更多細節內容。

* [Flutter documentation: timeDilation property]({{site.api}}/flutter/scheduler/timeDilation.html) 

  [Flutter 文件：timeDilation 屬性]({{site.api}}/flutter/scheduler/timeDilation.html) 

以下的錄屏展示了動畫減速前後的對比。

![Screen recording showing normal animation speed]({{site.url}}/assets/images/docs/tools/devtools/debug-toggle-slow-animations-disabled.gif)
![Screen recording showing slowed animation speed]({{site.url}}/assets/images/docs/tools/devtools/debug-toggle-slow-animations-enabled.gif)

### Show guidelines

### 顯示引導線

This feature draws guidelines over your app that display render boxes, alignments,
paddings, scroll views, clippings and spacers.

該功能會在你的應用最上層繪製引導線，展示繪製區域、對齊、間距、滾動檢視、裁剪和空位填充。

This tool can be used for better understanding your layout. For instance,
by finding unwanted padding or understanding widget alignment.

這個工具能幫助你更加了解你的佈局。例如查詢不需要的填充或者理解 widget 的對齊方式。

You can also enable this in code:

你也可以透過程式碼啟用：

<?code-excerpt "lib/layout_guidelines.dart"?>
```dart
import 'package:flutter/rendering.dart';

void showLayoutGuidelines() {
  debugPaintSizeEnabled = true;
}
```

#### Render boxes

#### RenderBox

Widgets that draw to the screen create a [render box][], the 
building blocks of Flutter layouts. They're shown with a bright blue border:

繪製在螢幕上的 widgets 會建立一個 [RenderBox][render box]，
它是 Flutter 佈局的基礎建構。
這些 RenderBox 會加上一個淺藍色的邊框：

![Screenshot of render box guidelines]({{site.url}}/assets/images/docs/tools/devtools/debug-toggle-guideline-render-box.png)

#### Alignments

#### 對齊方式

Alignments are shown with yellow arrows. These arrows show the vertical
and horizontal offsets of a widget relative to its parent.
For example, this button's icon is shown as being centered by the four arrows:

對齊方式將以黃色箭頭展示。
這些箭頭會顯示出垂直和豎屏方向上 widget 相對其父佈局的偏移。
例如，這個按鈕圖示有四個箭頭表示它被居中展示：

![Screenshot of alignment guidelines]({{site.url}}/assets/images/docs/tools/devtools/debug-toggle-guidelines-alignment.png)

#### Padding

#### 間距

Padding is shown with a semi-transparent blue background:

間距會以半透明的藍色背景顯示：

![Screenshot of padding guidelines]({{site.url}}/assets/images/docs/tools/devtools/debug-toggle-guidelines-padding.png)

#### Scroll views

#### 滾動檢視

Widgets with scrolling contents (such as list views) are shown with green arrows:

包含滾動內容的 widget（例如 ListView）會展示綠色的箭頭：

![Screenshot of scroll view guidelines]({{site.url}}/assets/images/docs/tools/devtools/debug-toggle-guidelines-scroll.png)

#### Clipping

#### 裁剪

Clipping, for example when using the [ClipRect widget][], are shown
with a dashed pink line with a scissors icon:

使用了諸如 [ClipRect Widget][] 進行裁剪的內容，會以粉紅色的虛線加一個剪刀圖示展示：

[ClipRect widget]: {{site.api}}/flutter/widgets/ClipRect-class.html

![Screenshot of clip guidelines]({{site.url}}/assets/images/docs/tools/devtools/debug-toggle-guidelines-clip.png)

#### Spacers

#### 空位填充

Spacer widgets are shown with a grey background,
such as this `SizedBox` without a child:

空位填充的 widgets 會以灰色背景展示，例如沒有 child 的 `SizedBox`：

![Screenshot of spacer guidelines]({{site.url}}/assets/images/docs/tools/devtools/debug-toggle-guidelines-spacer.png)

### Show baselines

### 顯示基線

This option makes all baselines visible.
Baselines are horizontal lines used to position text.

該選項會顯示所有的基線。
基線是水平的用來定位文字的線。

This can be useful for checking whether text is precisely aligned vertically.
For example, the text baselines in the following screenshot are slightly misaligned:

在檢查文字是否垂直對齊時，基線會非常有用。
例如，下圖中文字的基線稍微有一些錯位：

![Screenshot with show baselines enabled]({{site.url}}/assets/images/docs/tools/devtools/debug-toggle-guidelines-baseline.png)

The [Baseline][] widget can be used to adjust baselines.

[Baseline][] widget 可以用來調整基線。

[Baseline]: {{site.api}}/flutter/widgets/Baseline-class.html

A line is drawn on any [render box][] that has a baseline set;
alphabetic baselines are shown as green and ideographic as yellow.

在設定了基線的 [RenderBox][render box] 上，都會顯示一條線。
字母的基線以綠色展示，而符號的基線以黃色展示。

You can also enable this in code:

你也可以透過程式碼啟用：

<?code-excerpt "lib/show_baselines.dart"?>
```dart
import 'package:flutter/rendering.dart';

void showBaselines() {
  debugPaintBaselinesEnabled = true;
}
```

### Highlight repaints

### 高亮重繪製內容

This option draws a border around all [render boxes][]
that changes color every time that box repaints.

該選項會為所有的 [RenderBox][render boxes] 繪製一層邊框，
在它們重新繪製時改變顏色。

[render boxes]: {{site.api}}/flutter/rendering/RenderBox-class.html

This rotating rainbow of colors is useful for finding parts of your app
that are repainting too often and potentially harming performance.

以彩虹色譜迴圈的顏色，有利於你找到應用中頻繁重繪導致效能消耗過大的部分。

For example, one small animation could be causing an entire page
to repaint on every frame.
Wrapping the animation in a [RepaintBoundary widget][] limits
the repainting to just the animation.

例如，一個小動畫可能會導致整個頁面一直在重繪。
將動畫使用 [RepaintBoundary widget][] 巢狀(Nesting)，可以保證動畫只會導致其本身重繪。

[RepaintBoundary widget]: {{site.api}}/flutter/widgets/RepaintBoundary-class.html

Here the progress indicator causes its container to repaint:

下面是一個進度指示器導致其容器重繪的例子：

<?code-excerpt "lib/highlight_repaints.dart (EverythingRepaints)"?>
```dart
class EverythingRepaintsPage extends StatelessWidget {
  const EverythingRepaintsPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Repaint Example')),
      body: const Center(
        child: CircularProgressIndicator(),
      ),
    );
  }
}
```

![Screen recording of a whole screen repainting]({{site.url}}/assets/images/docs/tools/devtools/debug-toggle-guidelines-repaint-1.gif)

Wrapping the progress indicator in a `RepaintBoundary` causes
only that section of the screen to repaint:

將進度指示器使用 `RepaintBoundary` 包裹，
可以將重繪範圍縮小至它本身佔有的區域。

<?code-excerpt "lib/highlight_repaints.dart (AreaRepaints)"?>
```dart
class AreaRepaintsPage extends StatelessWidget {
  const AreaRepaintsPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Repaint Example')),
      body: const Center(
        child: RepaintBoundary(
          child: CircularProgressIndicator(),
        ),
      ),
    );
  }
}
```

![Screen recording of a just a progress indicator repainting]({{site.url}}/assets/images/docs/tools/devtools/debug-toggle-guidelines-repaint-2.gif)

`RepaintBoundary` widgets have tradeoffs. They can help with performance,
but they also have an overhead of creating a new canvas,
which uses additional memory.

`RepaintBoundary` widget 也有一些額外的消耗。
它們對效能有一定的幫助，但也會在建立額外的繪製畫布時增加一定的記憶體消耗。

You can also enable this option in code:

你也可以透過程式碼啟用：

<?code-excerpt "lib/highlight_repaints.dart (Toggle)"?>
```dart
import 'package:flutter/rendering.dart';

void highlightRepaints() {
  debugRepaintRainbowEnabled = true;
}
```

### Highlight oversized images

### 高亮尺寸過大的圖片

This option highlights images that are too large by both inverting their colors
and flipping them vertically:

該選項會將尺寸過大的圖片高亮表示，並且進行垂直翻轉及色調反轉：

![A highlighted oversized image]({{site.url}}/assets/images/docs/tools/devtools/debug-toggle-guidelines-oversized.png)

The highlighted images use more memory than is required;
for example, a large 5MB image displayed at 100 by 100 pixels.

被高亮的圖片使用了過多的記憶體。
例如一張 5MB 大小的圖片以 100x100 畫素展示。

Such images can cause poor performance, especially on lower-end devices
and when you have many images, as in a list view,
this performance hit can add up.
Information about each image is printed in the debug console:

這樣的圖片會導致效能低下，在低端裝置上尤為明顯，
而當你在諸如列表中有大量這樣的圖片時，效能的下降會疊加。
除錯控制檯視窗中會列印每個圖片的資訊：

```console
dash.png has a display size of 213×392 but a decode size of 2130×392, which uses an additional 2542KB.
```

Images are deemed too large if they use at least 128KB more than required.

超過 128KB 的圖片會被視為過大。

#### Fixing images

#### 調整圖片

Wherever possible, the best way to fix this problem is resizing
the image asset file so it's smaller.

在可能的情況下，最好的辦法是調整圖片資源的大小，讓它變得更小。

If this isn't possible, you can use the `cacheHeight` and `cacheWidth`
parameters on the `Image` constructor:

如果該方法不可行，你可以使用 `Image` 構造裡的
`cacheHeight` 和 `cacheWidth` 引數：

<?code-excerpt "lib/oversized_images.dart (ResizedImage)"?>
```dart
class ResizedImage extends StatelessWidget {
  const ResizedImage({super.key});

  @override
  Widget build(BuildContext context) {
    return Image.asset(
      'dash.png',
      cacheHeight: 213,
      cacheWidth: 392,
    );
  }
}
```

This makes the engine decode this image at the specified size,
and reduces memory usage (decoding and storage is still more expensive
than if the image asset itself was shrunk).
The image is rendered to the constraints of the layout or width and height
regardless of these parameters. 

這樣的方法可以讓引擎以指定的大小解析圖片，減少記憶體的消耗
（解析開銷和空間佔用相較圖片調整圖片本身仍然較大）。
無論如何設定引數，圖片依然會以佈局限制或大小進行渲染。

This property can also be set in code:

該屬性同樣可以使用程式碼設定：

<?code-excerpt "lib/oversized_images.dart (Toggle)"?>
```dart
void showOversizedImages() {
  debugInvertOversizedImages = true;
}
```

#### More information

#### 更多內容

You can learn more at the following link:

以下的連結提供了更多細節內容：

- [Flutter documentation: debugInvertOversizedImages]({{site.api}}/flutter/painting/debugInvertOversizedImages.html)

  [Flutter 文件：debugInvertOversizedImages]({{site.api}}/flutter/painting/debugInvertOversizedImages.html)

[render box]: {{site.api}}/flutter/rendering/RenderBox-class.html

## Details Tree

## 樹的詳細資訊

Select the **Widget Details Tree** tab to display the details tree for the
selected widget. From here, you can gather useful information about a
widget's properties, render object, and children.

選擇 **Widget Details Tree** 標籤來顯示選中 widget 的詳細資訊樹。
從這裡，你可以收集關於 widget 屬性、渲染物件和子節點的有用資訊。

![The Details Tree view]({{site.url}}/assets/images/docs/tools/devtools/inspector_details_tree.png){:width="100%"}


## Track widget creation

## 追蹤 widget 建立

Part of the functionality of the Flutter inspector is based on
instrumenting the application code in order to better understand
the source locations where widgets are created. The source
instrumentation allows the Flutter inspector to present the
widget tree in a manner similar to how the UI was defined
in your source code. Without it, the tree of nodes in the
widget tree are much deeper, and it can be more difficult to
understand how the runtime widget hierarchy corresponds to
your application's UI.

Flutter inspector 的部分功能是基於檢測應用程式的原始碼，以便更好地理解建立 widget 的源位置。
Flutter inspector 可以以類似於在原始碼中定義 UI 的方式呈現 widget 樹。
如果沒有它，widget 樹中的組成某個節點的樹結構會更深，
並且更難理解執行時 widget 的層次結構如何與應用程式的 UI 相對應。

You can disable this feature by passing `--no-track-widget-creation` to
the `flutter run` command.

你可以透過在 `flutter run` 後面新增引數 `-no track widget creation` 來禁用此功能。

Here are examples of what your widget tree might look like
with and without track widget creation enabled.

下面是 widget 樹在啟用和不啟用追蹤 widget 建立下的範例。

Track widget creation enabled (default):

啟用追蹤 widget 建立（預設）：

![The widget tree with track widget creation enabled]({{site.url}}/assets/images/docs/tools/devtools/track_widget_creation_enabled.png){:width="100%"}

Track widget creation disabled (not recommended):

關閉追蹤 widget 建立（不推薦）：

![The widget tree with track widget creation disabled]({{site.url}}/assets/images/docs/tools/devtools/track_widget_creation_disabled.png){:width="100%"}

This feature prevents otherwise-identical `const` Widgets from
being considered equal in debug builds. For more details, see
the discussion on [common problems when debugging][].

此功能可避免在除錯建構中將其他相同的 `const` 的 Widgets 視為相同。
有關更多詳細資訊，請參閱關於 [除錯時常見問題][common problems when debugging] 的討論。

## Other resources

## 其他資源

For a demonstration of what's generally possible with the inspector,
see the [DartConf 2018 talk][] demonstrating the IntelliJ version
of the Flutter inspector.

有關 Flutter inspector 常用功能的示範，請參考 [DartConf 2018 talk][]
中基於 IntelliJ 上的示範。

To learn how to visually debug layout issues
using DevTools, check out a guided
[Flutter Inspector tutorial][inspector-tutorial].

想要學習如何使用 DevTools 以視覺化的方式除錯佈局問題，
可以閱讀 [Flutter 佈局檢查器課程][inspector-tutorial]。

[`Column`]: {{site.api}}/flutter/widgets/Column-class.html
[common problems when debugging]: {{site.url}}/testing/debugging#common-problems
[`crossAxisAlignment`]: {{site.api}}/flutter/widgets/Flex/crossAxisAlignment.html
[DartConf 2018 talk]: https://www.bilibili.com/video/BV1h4411575y/
[debug mode]: {{site.url}}/testing/build-modes#debug
[Debugging Flutter apps]: {{site.url}}/testing/debugging
[DevTools written in Flutter]: {{site.url}}/tools/devtools/overview#how-do-i-try-devtools-written-in-flutter
[`Flex`]: {{site.api}}/flutter/widgets/Flex-class.html
[flex layouts]: {{site.api}}/flutter/widgets/Flex-class.html
[`FlexFit`]: {{site.api}}/flutter/rendering/FlexFit.html
[`FlexParentData.fit`]: {{site.api}}/flutter/rendering/FlexParentData/fit.html
[`FlexParentData.flex`]: {{site.api}}/flutter/rendering/FlexParentData/flex.html
[Flutter performance profiling]: {{site.url}}/perf/ui-performance
[`mainAxisAlignment`]: {{site.api}}/flutter/widgets/Flex/mainAxisAlignment.html
[`mainAxisSize`]: {{site.api}}/flutter/widgets/Flex/mainAxisSize.html
[`Row`]: {{site.api}}/flutter/widgets/Row-class.html
[`textDirection`]: {{site.api}}/flutter/widgets/Flex/textDirection.html
[the performance overlay]: {{site.url}}/perf/ui-performance#the-performance-overlay
[Understanding constraints]: {{site.url}}/ui/layout/constraints
[inspector-tutorial]: {{site.medium}}/@fluttergems/mastering-dart-flutter-devtools-flutter-inspector-part-2-of-8-bbff40692fc7
