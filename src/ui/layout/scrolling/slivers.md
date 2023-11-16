---
title: Using slivers to achieve fancy scrolling
title: 使用 sliver 實現出色的滑動效果
description: Where to find information on using slivers to implement fancy scrolling effects, like elastic scrolling, in Flutter.
description: 介紹如何透過 slivers 在 Flutter 裡實現酷炫的滑動效果。
toc: false
tags: 使用者介面,Flutter UI
keywords: 滑動效果, slivers
---

A sliver is a portion of a scrollable area that you
can define to behave in a special way.
You can use slivers to achieve custom scrolling effects,
such as elastic scrolling.

Sliver 是可滾動區域的一部分，你可以定義它以特殊的方式工作。
你可以使用 sliver 實現自訂滾動效果，
比如彈性滾動。

For a free, instructor-led video workshop that uses DartPad,
check out the following video about using slivers:

以下是一個關於使用 sliver 的免費指導影片，
由講師使用 DartPad Workshop 進行指導，
請觀看影片：

<iframe width="560" height="315" src="https://player.bilibili.com/player.html?aid=291195426&bvid=BV11f4y187gV&cid=354814353&page=1&autoplay=false" title="Bilibili video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Resources

## 資源

For more information on implementing fancy scrolling effects
in Flutter, see the following resources:

關於在 Flutter 中實現出色的滾動效果的更多資訊，
請參閱以下資源：

<dl markdown="1">
<dt markdown="1"> **[Slivers, Demystified][]**
</dt>
<dd markdown="1">
<p markdown="1">A free article on Medium that
    explains how to implement custom scrolling
    using the sliver classes.</p>
<p markdown="1">一篇 Medium 上的免費文章，
介紹瞭如何使用 sliver 元件類實現自訂滾動。</p>
</dd>

<dt markdown="1"> **[SliverAppBar][sliver-app-bar-video]**
</dt>
<dd markdown="1">
<p markdown="1">A one-minute Widget-of-the-week
    video that gives an overview of the
    `SliverAppBar` widget.</p>
<p markdown="1">一段一分鐘的「每週 Flutter Widgets」影片，
概述了 `SliverAppBar` widget。</p>

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=586378022&bvid=BV19z4y1S7K7&cid=288732722&page=1&autoplay=false" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</dd>

<dt markdown="1">
<p markdown="1">**[SliverList and SliverGrid][]**</p>
<p markdown="1">**[SliverList 和 SliverGrid][SliverList and SliverGrid]**</p>
</dt>
<dd markdown="1">
<p markdown="1">A one-minute Widget-of-the-week
    video that gives an overview of the `SliverList`
    and `SliverGrid` widgets.</p>
<p markdown="1">一段一分鐘的「每週 Flutter Widgets」影片，
概述了 `SliverList` 和 `SliverGrid` widget。</p>

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=38437526&bvid=BV1Pt411v78y&cid=67565151&page=12&autoplay=false" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</dd>

<dt markdown="1">
<p markdown="1">**[Slivers explained - Making dynamic layouts][]**</p>
<p markdown="1">**[Slivers explained - 動態佈局][Slivers explained - Making dynamic layouts]**</p>
</dt>
<dd markdown="1">
<p markdown="1">A 50-minute episode of [The Boring Show][]
    where Ian Hickson, Flutter's Tech Lead, and Filip Hracek
    discuss the power of slivers.</p>
<p markdown="1">一集 50 分鐘的 [The Boring Show][] 影片，
Flutter 的技術負責人 Ian Hickson 和 Filip Hracek 討論了 sliver 的能力。</p>

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=77325252&bvid=BV1EJ41197NB&cid=132272803&page=1&autoplay=false" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</dd>
</dl>

## API docs

## API 文件

To learn more about the available sliver APIs,
check out these related API docs:

參閱以下 Sliver 相關 API 文件瞭解更多：

* [`CustomScrollView`][]
* [`SliverAppBar`][]
* [`SliverGrid`][]
* [`SliverList`][]


[`CustomScrollView`]: {{site.api}}/flutter/widgets/CustomScrollView-class.html
[sliver-app-bar-video]: {{site.youtube-site}}/watch?v=R9C5KMJKluE
[`SliverAppBar`]: {{site.api}}/flutter/material/SliverAppBar-class.html
[`SliverGrid`]: {{site.api}}/flutter/widgets/SliverGrid-class.html
[SliverList and SliverGrid]: {{site.youtube-site}}/watch?v=ORiTTaVY6mM
[`SliverList`]: {{site.api}}/flutter/widgets/SliverList-class.html
[Slivers, DeMystified]: {{site.flutter-medium}}/slivers-demystified-6ff68ab0296f
[Slivers explained - Making dynamic layouts]: https://www.bilibili.com/video/BV1EJ41197NB/
[The Boring Show]: {{site.youtube-site}}/playlist?list=PLOU2XLYxmsIK0r_D-zWcmJ1plIcDNnRkK
