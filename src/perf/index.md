---
title: Performance
title: 效能評估
description: Evaluating the performance of your app from several angles.
description: 從多個角度評估你的應用效能..
tags: Flutter效能
keywords: 效能評估,包體積,記憶體佔用
---

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=243695231&bvid=BV1zv411B7gY&cid=207457008&page=1&autoplay=false" 
frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; 
picture-in-picture" allowfullscreen></iframe>
[Flutter performance basics]({{site.youtube-site}}/watch?v=PKGguGUwSYE)

[Flutter 效能入門](https://www.bilibili.com/video/BV1zv411B7gY/)

{{site.alert.note}}

  If your app has a performance issue and you are
  trying to debug it, check out the DevTool's page
  on [Using the Performance view][].

  如果你的應用程式存在效能問題，並且你正在嘗試進行除錯，
  可以檢視 DevTools 的頁面 [使用效能檢視][Using the Performance view]。

{{site.alert.end}}

[Using the Performance view]: {{site.url}}/tools/devtools/performance

What is performance? Why is performance important? How do I improve performance?

什麼是效能？為什麼效能很重要？如何才能提升效能？

Our goal is to answer those three questions (mainly the third one), and 
anything related to them. This document should serve as the single entry 
point or the root node of a tree of resources that addresses any questions 
that you have about performance.

我們的目標是回答這三個問題（主要是第三個）以及任何與之相關的話題。
如果你有任何關於效能方面的問題，本文件可以作為解決你疑惑的起點。

The answers to the first two questions are mostly philosophical,
and not as helpful to many developers who visit this page with specific
performance issues that need to be solved.
questions are in the [appendix]({{site.url}}/perf/appendix).

前兩個問題的答案比較哲學，對於正在閱讀這篇文章的開發者而言，當他們需要解決特定的效能問題時，並沒有什麼幫助。
所以，我們將它們放在了 [附錄]({{site.url}}/perf/appendix)。

To improve performance, you first need metrics: some measurable numbers to
verify the problems and improvements.
In the [metrics]({{site.url}}/perf/metrics) page,
you'll see which metrics are currently used,
and which tools and APIs are available to get the metrics.

為了提升效能，首先你需要一些可以量化的指標來驗證問題和效能的提升。
在 [指標]({{site.url}}/perf/metrics) 頁面，你可以看到一些現有的指標，
以及哪些工具和 API 可以用於獲取這些指標。

There is a list of [Frequently asked questions]({{site.url}}/perf/faq), 
so you can find out if the questions you have or the problems you're having 
were already answered or encountered, and whether there are existing solutions. 
(Alternatively, you can check the Flutter GitHub issue database using the
[performance][performance] label.)
 
這裡有一個 [常見問題]({{site.url}}/perf/faq) 的列表，
你可以查詢你的問題是否出現過或者已經被解答，
以及是否有現成的解決方案。
（你也可以檢視 GitHub issues 裡含有 [效能][performance] 標籤的內容。） 

Finally, the performance issues are divided into four categories. They 
correspond to the four labels that are used in the Flutter GitHub issue 
database: "[perf: speed][speed]", "[perf: memory][memory]", 
"[perf: app size][size]", "[perf: energy][energy]".

最後，效能問題可以分為四類，對應 GitHub issue 裡的四個標籤：
「[流暢度][speed]」、「[記憶體][memory]」、「[應用大小][size]」、和 「[功耗][energy]」。

The rest of the content is organized using those four categories.

其它內容均已歸納到這四個類別中。

{% comment %}
Let's put "speed" (rendering) first as it's the most popular performance issue
category.
{% endcomment -%}
## Speed

## 流暢度

Are your animations janky (not smooth)? Learn how to 
evaluate and fix rendering issues.

你的動畫是否卡頓（不流暢）？學習如何評估和修復渲染問題。

[Improving rendering performance]({{site.url}}/perf/rendering-performance)

[提高渲染效能]({{site.url}}/perf/rendering-performance)

{% comment %}
Do your apps take a long time to open? We'll also cover the startup speed issue
in some future pages.
{% endcomment -%}

{% comment %}

TODO(https://github.com/flutter/website/issues/8249): Reintroduce this article and add this link back.

## Memory

## 記憶體

[Using memory wisely]({{site.url}}/perf/memory)

[正確地管理記憶體]({{site.url}}/perf/memory)

{% endcomment -%}

## App size

## 應用大小

How to measure your app's size. The smaller the size,
the quicker it is to download.

如何測量應用的體積。體積越小，下載就越快。

[Measuring your app's size][]

[測量應用的體積][Measuring your app's size]

{% comment %}

TODO(https://github.com/flutter/website/issues/8249): Reintroduce this article and add this link back.

## Energy

## 功耗

How to ensure a longer battery life when running your app.

當執行你的應用程式時，如何確保更久的電池續航。

[Preserving your battery]({{site.url}}/perf/power)

[節省電量]({{site.url}}/perf/power)

{% endcomment -%}

[Measuring your app's size]: {{site.url}}/perf/app-size

[speed]: {{site.repo.flutter}}/issues?q=is%3Aopen+label%3A%22perf%3A+speed%22+sort%3Aupdated-asc+
[energy]: {{site.repo.flutter}}/issues?q=is%3Aopen+label%3A%22perf%3A+energy%22+sort%3Aupdated-asc+
[memory]: {{site.repo.flutter}}/issues?q=is%3Aopen+label%3A%22perf%3A+memory%22+sort%3Aupdated-asc+
[size]: {{site.repo.flutter}}/issues?q=is%3Aopen+label%3A%22perf%3A+app+size%22+sort%3Aupdated-asc+
[performance]: {{site.repo.flutter}}/issues?q=+label%3A%22severe%3A+performance%22
