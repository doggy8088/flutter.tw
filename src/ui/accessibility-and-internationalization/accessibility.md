---
title: Accessibility
title: 無障礙
description: Information on Flutter's accessibility support.
tags: Flutter開發
keywords: 聯合國關於殘疾人權利,CRPD,無障礙
---

Ensuring apps are accessible to a broad range of users is an essential
part of building a high-quality app. Applications that are poorly
designed create barriers to people of all ages. The [UN Convention on
the Rights of Persons with Disabilities][CRPD] states the moral and legal
imperative to ensure universal access to information systems; countries
around the world enforce accessibility as a requirement; and companies
recognize the business advantages of maximizing access to their services.

確保你的應用能夠被廣泛的使用者使用是建構高品質應用程式至關重要的一點。
如果你的應用設計不佳，可能會無法覆蓋到所有年齡段的人。
[聯合國關於殘疾人權利][CRPD] 規定了道德和法律必須確保資訊系統能夠普遍使用。
世界各地也都要求提供無障礙的環境；
同樣，公司也認識到了最大限度覆蓋服務的優勢所在。

We strongly encourage you to include an accessibility checklist 
as a key criteria before shipping your app. Flutter is committed to
supporting developers in making their apps more accessible, and includes
first-class framework support for accessibility in addition to that
provided by the underlying operating system, including:

我們強烈建議你將輔助功能清單新增到釋出應用前的關鍵指標。
Flutter 始終致力於支援開發者能夠使它的應用更易於存取，
其中就包括了由底層作業系統提供的一流的無障礙支援，
包括：

[**Large fonts**][]
<br> Render text widgets with user-specified font sizes

[**大字型**][**Large fonts**]
<br> 使用使用者指定的字型大小呈現文字 widget

[**Screen readers**][]
<br> Communicate spoken feedback about UI contents

[**讀屏器**][**Screen readers**]
<br> 透過語音反饋傳達使用者介面的內容

[**Sufficient contrast**][]
<br> Render widgets with colors that have sufficient contrast

[**高對比度**][**Sufficient contrast**]
<br> 在渲染 widget 時，使用具有高對比度的顏色

Details of these features are discussed below.

詳細關於這個特性的討論，請繼續閱讀。

## Inspecting accessibility support

## 輔助功能檢測

Details of these are discussed below.
In addition to testing for these specific topics,
we recommend using automated accessibility scanners:

關於無障礙功能檢測的細節，我們將在下面討論。
除了測試這些特定主題外，我們還建議您使用自動輔助功能掃描程式：

  * For Android:

    對於 Android:

    1. Install the [Accessibility Scanner][] for Android

       在 Android 上安裝 [輔助掃描程式][Accessibility Scanner]

    1. Enable the Accessibility Scanner from
       **Android Settings > Accessibility >
       Accessibility Scanner > On**

       透過 **設定 > 輔助 > 輔助掃描器 > 開啟 在 Android 啟用輔助功能掃描程式**

    1. Navigate to the Accessibility Scanner 'checkbox'
       icon button to initiate a scan

       在導航欄找到輔助掃描器複選框按鈕啟動掃描

  * For iOS:

    對於 iOS:

    1. Open the `iOS` folder of your Flutter app in Xcode

       在 Xcode 中開啟 Flutter 應用程式的 `iOS` 資料夾

    1. Select a Simulator as the target, and click **Run** button

       找到模擬器，然後單擊 **執行** 按鈕

    1. In Xcode, select
       **Xcode > Open Developer Tools > Accessibility Inspector**

       在 Xcode 選擇 **Xcode > 開發者工具 > 輔助檢查器**

    1. In the Accessibility Inspector,
       select **Inspection > Enable Point to Inspect**,
       and then select the various user interface elements in running
       Flutter app to inspect their accessibility attributes

       在輔助檢查器中，選擇 **檢查> 啟用點檢查**，然後執行 Flutter 應用程式，
       選擇各種使用者介面元素來檢查其輔助功能

    1. In the Accessibility Inspector,
       select **Audit** in the toolbar, and then
       select **Run Audit** to get a report of potential issues

       在輔助檢查器中，選擇工具欄中的 **稽核**，
       然後選擇 **執行音訊** 來獲取潛在問題的報告
 
  * For web:

    對於 web:

    1. Open Chrome DevTools (or similar tools in other browsers)

       開啟 Chrome 開發者工具（或其他瀏覽器中的類似工具）

    2. Inspect the HTML tree containing the ARIA attributes generated by Flutter.

       檢查包含 Flutter 產生的 ARIA 屬性的 HTML 樹。

    3. In Chrome, the "Elements" tab has a "Accessibility" sub-tab 
       that can be used to inspect the data exported to semantics tree

       在 Chrome 中，「Elements」選項卡中有一個「Accessbility」子選項卡，
       可用於檢查匯出到語義樹的資料。

## Large fonts

## 大字型

Both Android and iOS contain system settings to configure the desired font
sizes used by apps. Flutter text widgets respect this OS setting when
determining font sizes.

Android 和 iOS 都包含配置應用程式所需字型大小的系統設定。
在確定字型大小時，
Flutter 文字 widget 會遵循當前系統設定。

### Tips for developers

### 給開發者的提示

Font sizes are calculated automatically by Flutter based on the OS setting.
However, as a developer you should make sure your layout has enough room to
render all its contents when the font sizes are increased.
For example, you can test all parts of your app on a small-screen
device configured to use the largest font setting.

Flutter 會根據作業系統設定自動計算字型大小。
但是，作為開發人員，你應確保在增加字型大小時，
你的頁面有足夠的空間來呈現其所有內容。例如，
你可以在小螢幕上設定最大的字型來測試你應用上的全部內容。

### Example

### 例子

The following two screenshots show the standard Flutter app
template rendered with the default iOS font setting,
and with the largest font setting selected in iOS accessibility settings.

以下兩個螢幕截圖分別顯示了使用預設 iOS 字型設定呈現的標準
Flutter 應用程式，和使用 iOS 輔助功能設定中選擇的最大字型
設定呈現的 Flutter 應用程式。

<div class="row">
  <div class="col-md-6">
    {% include docs/app-figure.md image="a18n/app-regular-fonts.png" caption="Default font setting" img-class="border" %}
  </div>
  <div class="col-md-6">
    {% include docs/app-figure.md image="a18n/app-large-fonts.png" caption="Largest accessibility font setting" img-class="border" %}
  </div>
</div>

## Screen readers

## 讀屏器

For mobile, screen readers ([TalkBack][], [VoiceOver][])
enable visually impaired users to get spoken feedback about
the contents of the screen and interact with the UI by using
gestures on mobile and keyboard shortcuts on desktop. 
Turn on VoiceOver or TalkBack on your mobile device and
navigate around your app.

在移動裝置上，讀屏器（[TalkBack][]、[VoiceOver][]）可以使視障使用者能夠獲得有關螢幕內容的語音反饋，
並透過移動裝置上的手勢和桌面上的鍵盤快捷鍵與 UI 進行互動。
在你的移動裝置上開啟 VoiceOver 或 TalkBack 並瀏覽你的應用程式。

**To turn on the screen reader on your device, complete the following steps:**

**請透過以下步驟，在裝置上開啟讀屏器：**

{% comment %} Nav tabs {% endcomment -%}
<ul class="nav nav-tabs" id="editor-setup" role="tablist">
    <li class="nav-item">
        <a class="nav-link active" id="talkback-tab" href="#talkback" role="tab" aria-controls="talkback" aria-selected="true">TalkBack on Android</a>
    </li>
    <li class="nav-item">
        <a class="nav-link" id="voiceover-tab" href="#voiceover" role="tab" aria-controls="voiceover" aria-selected="false">VoiceOver on iPhone</a>
    </li>
    <li class="nav-item">
        <a class="nav-link" id="browsers-tab" href="#browsers" role="tab" aria-controls="browsers" aria-selected="false">Browsers</a>
    </li>
    <li class="nav-item">
        <a class="nav-link" id="desktop-tab" href="#desktop" role="tab" aria-controls="desktop" aria-selected="false">Desktop</a>
    </li>
</ul>

{% comment %} Tab panes {% endcomment -%}
<div class="tab-content">

<div class="tab-pane active" id="talkback" role="tabpanel" aria-labelledby="talkback-tab" markdown="1">

1. On your device, open **Settings**.

   開啟裝置上的 **設定**。

2. Select **Accessibility** and then **TalkBack**.

   選擇 **輔助功能/無障礙** > **TalkBack**。

3. Turn 'Use TalkBack' on or off.

   開啟或關閉 **TalkBack**。

4. Select Ok.

   選擇完成。

To learn how to find and customize Android's
accessibility features, view the following video.

請觀看以下影片，
瞭解如何查詢和自訂 Android 的輔助功能/無障礙。

<iframe width="560" height="315" src="{{site.youtube-site}}/embed/FQyj_XTl01w" title="Customize accessibility features on Pixel" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
</iframe>

</div>

<div class="tab-pane" id="voiceover" role="tabpanel" aria-labelledby="voiceover-tab" markdown="1">

1. On your device, open **Settings > Accessibility > VoiceOver**

   開啟裝置上的 **設定 > 輔助功能 > 旁白 (VoiceOver)**

2. Turn the VoiceOver setting on or off

   開啟或關閉 **旁白 (VoiceOver)** 的設定

To learn how to find and customize iOS
accessibility features, view the following video.

請觀看以下影片，
瞭解如何查詢和自訂 iOS 的輔助功能。

<iframe width="560" height="315" src="{{site.youtube-site}}/embed/qDm7GiKra28" title="How to navigate your iPhone or iPad with VoiceOver" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
</iframe>

</div>

<div class="tab-pane" id="browsers" role="tabpanel" aria-labelledby="browsers-tab" markdown="1">

For web, the following screen readers are currently supported:

對於 Web，目前支援以下螢幕閱讀器：

Mobile browsers:

移動瀏覽器：

* iOS - VoiceOver
* Android - TalkBack

Desktop browsers:

桌面瀏覽器：

* MacOS - VoiceOver
* Windows - JAWs & NVDA

Screen readers users on web must toggle the
"Enable accessibility" button to build the semantics tree.
Users can skip this step if you programmatically auto-enable
accessibility for your app using this API:

Web 上的讀屏器使用者需要點選「啟用輔助功能」按鈕來建構語義樹。
如果你使用下面這個 API 以程式設計方式為你的應用程式自動啟用輔助功能，
則使用者可以跳過此步驟：

```dart
import 'package:flutter/material.dart';
import 'package:flutter/semantics.dart';

void main() {
  runApp(const MyApp());
  SemanticsBinding.instance.ensureSemantics();
}
```

</div>

<div class="tab-pane" id="desktop" role="tabpanel" aria-labelledby="desktop-tab" markdown="1">

Windows comes with a screen reader called Narrator
but some developers recommend using the more popular
NVDA screen reader. To learn about using NVDA to test
Windows apps, check out
[Screen Readers 101 For Front-End Developers (Windows)][nvda].

在 Windows 上，你可以使用自帶的講述人 (Narrator) 螢幕閱讀器，
但一些開發者建議使用更受歡迎的 NVDA 螢幕閱讀器。
想要了解如何使用 NVDA 測試 Windows 應用程式，
請查閱 [Screen Readers 101 For Front-End Developers (Windows)][nvda]。

[nvda]: https://get-evinced.com/blog/screen-readers-101-for-front-end-developers-windows

On a Mac, you can use the desktop version of VoiceOver,
which is included in macOS.

在 Mac 上，你可以使用 macOS 自帶的桌面版旁白 (VoiceOver)。

<iframe width="560" height="315" src="{{site.youtube-site}}/embed/5R-6WvAihms" title="Screen Reader Basics: VoiceOver -- A11ycasts #07" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
</iframe>

On Linux, a popular screen reader is called Orca.
It comes pre-installed with some distributions
and is available on package repositories such as `apt`.
To learn about using Orca, check out
[Getting started with Orca screen reader on Gnome desktop][orca].

在 Linux 上，有一種流行的螢幕閱讀器，名為 Orca。
它預裝在一些發行版中，
也可以在 package 儲存庫（如 `apt`）中找到。
想要了解如何使用 Orca，
請查閱 [Getting started with Orca screen reader on Gnome desktop][orca]。

[orca]: https://www.a11yproject.com/posts/getting-started-with-orca

</div>
</div>{% comment %} End: Tab panes. {% endcomment -%}

<br/>

Check out the following [video demo][] to see 
Victor Tsaran, who leads the Accessibility program for Material Design, 
using VoiceOver with the Flutter Gallery web app.

檢視此 [影片示範][video demo]，瞭解 Victor Tsaran，
他領導了 Material Design 的輔助功能計劃，
並在 Flutter Gallery Web 應用程式中使用了 VoiceOver。

Flutter's standard widgets generate an accessibility tree automatically. 
However, if your app needs something different,
it can be customized using the [`Semantics` widget][].

Flutter 的標準 widget 會自動產生無障礙樹。
但是，如果你的應用需要不同的東西，則可以使用
[語義 小部件][`Semantics` widget] 來自訂您應用程式的無障礙體驗。

When there is text in your app that should be voiced
with a specific voice, inform the screen reader
which voice to use by calling [`TextSpan.locale`][].
Note that `MaterialApp.locale` and `Localizations.override`
don't affect which voice the screen reader uses.
Usually, the screen reader uses the system voice
except where you explicitly set it with `TextSpan.locale`.

當你的應用中的某段文字需要以特定的語言進行播報時，
可以透過 [`TextSpan.locale`][] 來通知螢幕閱讀器對應的語言。
請注意 `MaterialApp.locale` 和 `Localizations.override`
並不會影響螢幕閱讀器對語言的選擇。
通常來說，除非你設定了 `TextSpan.locale`，否則螢幕閱讀器都會使用系統語言。

[`TextSpan.locale`]: {{site.api}}/flutter/painting/TextSpan/locale.html

## Sufficient contrast

## 高對比度

Sufficient color contrast makes text and images easier to read.
Along with benefitting users with various visual impairments,
sufficient color contrast helps all users when viewing an interface
on devices in extreme lighting conditions,
such as when exposed to direct sunlight or on a display with low
brightness.

高對比度能夠使文字和圖像更易於閱讀。除了使具有各種視覺障礙的使用者受益外，
高對比度也能夠幫助所有使用者在極端光照條件下
(例如在直射陽光下或在低亮度顯示器上) 觀看裝置上的介面。

The [W3C recommends][]: 

[W3C 建議][W3C recommends]:

* At least 4.5:1 for small text (below 18 point regular or 14 point bold)

  小文字至少 4.5:1 (低於 18 畫素常規或 14 畫素粗體) 
  
* At least 3.0:1 for large text (18 point and above regular or 14 point and
  above bold)

  大文字至少 3.0:1 (18 畫素及以上常規或 14 畫素及以上粗體) 

### Tips for developers

### 給開發者的提示

Make sure any images you include have sufficient contrast.

確保你包含的任何圖像都具有較高的對比度。

When specifying colors on widgets,
make sure sufficient contrast is used between
foreground and background color selections.

在 widget 上指定顏色時，請確保在前景色和背景色之間具備足夠的對比度。

## Building with accessibility in mind

## 思考如何建構無障礙應用

Ensuring your app can be used by everyone means building accessibility
into it from the start. For some apps, that's easier said than done.
In the video below, two of our engineers take a mobile app from a dire
accessibility state to one that takes advantage of Flutter's built-in
widgets to offer a dramatically more accessible experience.

確保你的應用能夠被所有人使用，
這意味著你需要從一開始就考慮到無障礙。
對於一些應用，說起來容易做起來難。
在下面的影片中，我們的兩名工程師
從一個無障礙狀態中獲取了一個
Flutter 內建的 widget，
以提供更加便捷的體驗。

<iframe width="560" height="315" src="https://player.bilibili.com/player.html?aid=630361140&bvid=BV1N84y1c7hW&cid=331074861&page=1&autoplay=false" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Testing accessibility on mobile

## 測試移動裝置的無障礙

Test your app using Flutter's [Accessibility Guideline API][].
This API checks if your app's UI meets Flutter's accessibility recommendations.
These cover recommendations for text contrast, target size, and target labels.

使用 Flutter 的 [Accessibility Guideline API][] 測試你的應用程式。
該 API 可以檢查應用程式的使用者介面是否符合 Flutter 的無障礙建議。
這些建議包括文字對比度、目標尺寸和目標標籤。

The following example shows how to use the Guideline API on Name Generator.
You created this app as part of the
[Write your first Flutter app]({{site.url}}/get-started/codelab) codelab.
Each button on the app's main screen serves as a tappable target
with text represented in 18 point.

下面的範例展示瞭如何在名稱產生器中使用 Guideline API。
這個應用程式是 [建構你的第一個 Flutter 應用]({{site.url}}/get-started/codelab) codelab 中的內容。
應用程式主螢幕上的每個按鈕（文字為 18 畫素）都是一個可點選的目標。

<?code-excerpt path-base="codelabs/namer/step_08"?>
<?code-excerpt "test/a11y_test.dart (insideTest)" indent-by="2"?>
```dart
  final SemanticsHandle handle = tester.ensureSemantics();
  await tester.pumpWidget(MyApp());

  // Checks that tappable nodes have a minimum size of 48 by 48 pixels
  // for Android.
  await expectLater(tester, meetsGuideline(androidTapTargetGuideline));

  // Checks that tappable nodes have a minimum size of 44 by 44 pixels
  // for iOS.
  await expectLater(tester, meetsGuideline(iOSTapTargetGuideline));

  // Checks that touch targets with a tap or long press action are labeled.
  await expectLater(tester, meetsGuideline(labeledTapTargetGuideline));

  // Checks whether semantic nodes meet the minimum text contrast levels.
  // The recommended text contrast is 3:1 for larger text
  // (18 point and above regular).
  await expectLater(tester, meetsGuideline(textContrastGuideline));
  handle.dispose();
```

You can add Guideline API tests
in `test/widget_test.dart` of your app directory, or as a separate test
file (such as `test/a11y_test.dart` in the case of the Name Generator).

你可以在應用程式目錄的 `test/widget_test.dart` 中新增 Guideline API 測試，
也可以將其作為單獨的測試檔案（如名稱產生器中的 `test/a11y_test.dart`）。

[Accessibility Guideline API]: {{site.api}}/flutter/flutter_test/AccessibilityGuideline-class.html

## Testing accessibility on web

## 在 Web 上測試無障礙：

You can debug accessibility by visualizing the semantic nodes created for your web app
using the following command line flag in profile and release modes:

您可以透過在配置檔案和釋出模式下
使用以下命令列標誌視覺化為你的 Web 應用程式建立的語義節點來除錯無障礙：

```terminal
$ flutter run -d chrome --profile --dart-define=FLUTTER_WEB_DEBUG_SHOW_SEMANTICS=true
```
 
With the flag activated, the semantic nodes appear on top of the widgets;
you can verify that the semantic elements are placed where they should be.
If the semantic nodes are incorrectly placed, please [file a bug report][]. 

標記啟用後，語義節點出現在 widget 的頂部；
你可以驗證語義元素是否放置在應有的位置。
如果語義節點放置不正確，請 [提交錯誤報告][file a bug report]。

## Accessibility release checklist

## 無障礙釋出清單

Here is a non-exhaustive list of things to consider as you prepare your
app for release.

這裡是一些應用釋出前的你需要考慮的部分清單。

- **Active interactions**. Ensure that all active interactions do
something. Any button that can
be pushed should do something when pushed. For example, if you have a
no-op callback for an `onPressed` event, change it to show a `SnackBar`
on the screen explaining which control you just pushed.

  **主動互動**。 確保所有可以互動的地方都會給予反饋。
  任何按鈕在按下之後都會做點什麼。
  例如，如果你有一個“onPressed”事件的無操作回呼(Callback)，
  請改為顯示一個“SnackBar”，並告訴使用者剛才按下了哪個控制項。

- **Screen reader testing**. The screen reader should be able to
describe all controls on the page when you tap on them, and the
descriptions should be intelligible. Test your app with [TalkBack][]
(Android) and [VoiceOver][] (iOS).

  **螢幕閱讀測試**。
  螢幕閱讀器應該能夠讓你在點選控制項時描述頁面上所有的控制項，
  並且描述應易於理解。請使用 [TalkBack][]（Android）
  以及 [VoiceOver][] (iOS) 測試你的應用。

  **Contrast ratios**. We encourage you to have a contrast ratio of at
least 4.5:1 between controls or text and the background, with the
exception of disabled components. Images should also be vetted for
sufficient contrast. 

  **對比度**。我們建議你至少將控制項或文字與背景之間的比例
  設為 4.5 : 1，禁用的元件除外。圖片也應該經過稽核足夠的對比度。

- **Context switching**. Nothing should change the user's context
automatically while typing in information. Generally, the widgets
should avoid changing the user's context without some sort of
confirmation action.

  **上下文切換**。當用戶輸入資訊時你不應改變其資訊。
  通常來說，widget 應該避免在沒有任何確認動作的情況下
  更改使用者的上下文。

- **Tappable targets**. All tappable targets should be at least 48x48
pixels.

  **可點選的目標**。所有可點選的目標平均
  至少應為 48x48 畫素。

- **Errors**. Important actions should be able to be undone. In fields
that show errors, suggest a correction if possible.

  **錯誤**。所有重要動作應該能夠被撤銷。
  在有限範圍內顯示錯誤原因，
  如果可能的話，提供訂正建議。

- **Color vision deficiency testing**. Controls should be usable and
legible in colorblind and grayscale modes.

  **色覺不足測試**。控制項應該可用並且在色盲和灰度模式下清晰可見。

- **Scale factors**. The UI should remain legible and usable at very
large scale factors for text size and display scaling.

  **比例係數**。 文字大小和顯示比例的使用者介面應保持清晰易用。

## Learn more

## 更多資訊

To learn more about Flutter and accessibility, check out
the following articles written by community members:

如果你希望瞭解更多，尤其是如何配置 semantics tree，
請檢視如下社群成員貢獻的文章：

* [A deep dive into Flutter's accessibility widgets][]
* [Semantics in Flutter][]
* [Flutter: Crafting a great experience for screen readers][]

[CRPD]: https://www.un.org/development/desa/disabilities/convention-on-the-rights-of-persons-with-disabilities/article-9-accessibility.html
[A deep dive into Flutter's accessibility widgets]: {{site.medium}}/flutter-community/a-deep-dive-into-flutters-accessibility-widgets-eb0ef9455bc
[Flutter: Crafting a great experience for screen readers]: https://blog.gskinner.com/archives/2022/09/flutter-crafting-a-great-experience-for-screen-readers.html
[Accessibility Scanner]: https://play.google.com/store/apps/details?id=com.google.android.apps.accessibility.auditor&hl=en
[**Large fonts**]: #large-fonts
[**Screen readers**]: #screen-readers
[Semantics in Flutter]: https://www.didierboelens.com/2018/07/semantics/
[`Semantics` widget]: {{site.api}}/flutter/widgets/Semantics-class.html
[**Sufficient contrast**]: #sufficient-contrast
[TalkBack]: https://support.google.com/accessibility/android/answer/6283677?hl=en
[W3C recommends]: https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-contrast.html
[VoiceOver]: https://www.apple.com/lae/accessibility/iphone/vision/
[video demo]: {{site.youtube-site}}/watch?v=A6Sx0lBP8PI
[file a bug report]: https://goo.gle/flutter_web_issue
