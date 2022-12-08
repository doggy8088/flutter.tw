---
title: Implicit Animations
title: 隱含動畫
description: Where to find more information on using implicit animations in Flutter.
description: 在 Flutter 中尋找有關使用隱含動畫的更多資訊。
tags: 使用者介面,Flutter UI,動畫
keywords: 隱含動畫
---

With Flutter's [animation library][],
you can add motion and create visual effects
for the widgets in your UI.
One part of the library is an assortment of widgets
that manage animations for you.
These widgets are collectively referred to as _implicit animations_,
or _implicitly animated widgets_, deriving their name from the
[`ImplicitlyAnimatedWidget`][] class that they implement.
The following set of resources provide many ways to learn
about implicit animations in Flutter.

透過 Flutter 的 [動畫函式庫][animation library]，
你可以為 UI 中的 widgets 新增動作並創造視覺效果。
有些庫包含各種各樣可以幫你管理動畫的 widget。
這些 widgets 被統稱為 **隱含動畫** 或 **隱含動畫 widget**，
其名字來源於它們所實現的 [`ImplicitlyAnimatedWidget`][] 類別。
下列資源提供了許多在 Flutter 中學習使用隱含動畫的方法。

## Documentation

## 文件

[Implicit animations codelab][]
<br> Jump right into the code!
  This codelab uses interactive examples
  and step-by-step instructions to teach you
  how to use implicit animations.
  
[隱含動畫 codelab][Implicit animations codelab]
<br> 跳轉至程式碼！
  Codelab 使用互動式範例和分佈介紹來教你學會如何使用隱含動畫。

[`AnimatedContainer` sample][]
<br> A step-by-step recipe from the [Flutter cookbook][]
  for using the [`AnimatedContainer`][]
  implicitly animated widget.

[`AnimatedContainer` 範例][`AnimatedContainer` sample]
<br>[Flutter cookbook][] 中針對如何使用 [`AnimatedContainer`][] 隱含動畫 widget 進行了手把手的指導。

[`ImplicitlyAnimatedWidget`][] API page
<br> All implicit animations extend the `ImplicitlyAnimatedWidget` class.

[`ImplicitlyAnimatedWidget`][] API 頁面
<br>所有隱含動畫都擴充了 `ImplicitlyAnimatedWidget` 類別。

## Flutter in Focus videos

## 聚焦 Flutter 影片

Flutter in Focus videos feature 5-10 minute tutorials
with real code that cover techniques
that every Flutter dev needs to know from top to bottom.
The following videos cover topics
that are relevant to implicit animations.

聚焦 Flutter 影片以 5 到 10 分鐘的實戰程式碼為特點，涵蓋了每個 Flutter 開發人員都需要從頭到尾瞭解的技術。
下列影片涵蓋了所有與隱性動畫相關的話題。

{% comment %} Animation Basics with Implicit Animations {% endcomment %}

<iframe width="560" height="315" src="{{site.youtube-site}}/embed/IVTjpW3W33s" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{% comment %} Custom Implicit Animations with Tween Animation Builder {% endcomment %}
<iframe width="560" height="315" src="{{site.youtube-site}}/embed/6KiPEqzJIKQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## The Boring Show

Watch the Boring Show to follow Google Engineers build apps
from scratch in Flutter. The following episode covers
using implicit animations in a news aggregator app.

觀看《The Boring Show》，跟隨谷歌工程師用 Flutter 從零開始建構應用程式。
下面這一集涉及在一個新聞聚合器應用中使用隱含動畫。

{% comment %} Implicitly animating the Hacker News app {% endcomment %}

<iframe width="560" height="315" src="{{site.youtube-site}}/embed/8ehlWchLVlQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Widget of the Week videos

## 每週 Widget 影片

A weekly series of short animated videos each showing
the important features of one particular widget.
In about 60 seconds, you'll see real code for each
widget with a demo about how it works.
The following Widget of the Week videos cover
implicitly animated widgets:

每週都有一個系列的動畫短片，每個短片都展示了一個特定 widget 的核心功能。
在大約六十秒的時間裡，你將會看到每個 widget 的實戰程式碼，以及關於它是如何工作的示範。
下列「每週 Widget」影片涉及了隱含動畫 widget 有：

{% comment %} Animated Opacity {% endcomment %}
<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=839306372&bvid=BV1W54y1U7ma&cid=225852916&page=1" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{% comment %} Animated Padding {% endcomment %}
<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=839226956&bvid=BV1354y1U7gU&cid=223412850&page=1" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{% comment %} Animated Positioned {% endcomment %}
<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=839111170&bvid=BV1T54y1D7hk&cid=220295763&page=1" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{% comment %} Animated switcher {% endcomment %}
<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=544022725&bvid=BV1dv4y1o7BG&cid=295191533&page=1" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


[`AnimatedContainer` sample]: {{site.url}}/cookbook/animation/animated-container
[`AnimatedContainer`]: {{site.api}}/flutter/widgets/AnimatedContainer-class.html
[animation library]: {{site.api}}/flutter/animation/animation-library.html
[Flutter cookbook]: {{site.url}}/cookbook
[Implicit animations codelab]: {{site.url}}/codelabs/implicit-animations
[`ImplicitlyAnimatedWidget`]: {{site.api}}/flutter/widgets/ImplicitlyAnimatedWidget-class.html
