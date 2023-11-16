---
title: Flutter widget index
title: Flutter Widget 目錄
description: An alphabetical list of Flutter widgets.
description: 按照字母順序排序的 Flutter widgets 列表。
short-title: Widgets
short-title: Widget 目錄
show_breadcrumbs: false
---

{% assign sorted = site.data.catalog.widgets | sort:'name' -%}

This is an alphabetical list of nearly every widget that is bundled with
Flutter. You can also [browse widgets by category][catalog].

你可以在下方以字母順序檢視各個 Widget 的使用方法，幾乎包括了所有與 Flutter 相關的 widget。除此之外你還可以查閱 [核心 Widget 目錄][catalog]。

You might also want to check out our Widget of the Week video series
on the [Flutter YouTube channel]({{site.social.youtube}}). Each short
episode features a different Flutter widget. For more video series, see
our [videos]({{site.url}}/resources/videos) page.

我們每週都會在 [Youtube Flutter 頻道]({{site.social.youtube}})
釋出關於 Widget 的系列影片，你可以前去觀看學習。
每一個短影片都介紹了一個不同的 Flutter Widget。
關於更多系列影片，也歡迎檢視我們的
[學習 Flutter 的影片列表]({{site.url}}/resources/videos)。

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=55795672&cid=97539385&page=1&autoplay=false" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[Widget of the Week playlist]({{site.youtube-site}}/playlist?list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG)

[Widget 影片的每週播放列表]({{site.youtube-site}}/playlist?list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG)

<div class="card-deck card-deck--responsive">
{% for comp in sorted %}
    <div class="card">
        <a href="{{comp.link}}">
            <div class="card-image-holder">
                {{comp.image}}
            </div>
        </a>
        <div class="card-body">
            <a href="{{comp.link}}"><header class="card-title">{{comp.name}}</header></a>
            <p class="card-text">{{ comp.description | truncatewords: 25 }}</p>
        </div>
    </div>
{% endfor %}
</div>

[catalog]: {{site.url}}/ui/widgets
