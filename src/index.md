---
title: Flutter documentation
title: Flutter 開發文件
short-title: Docs
short-title: 文件
description: Get started with Flutter. Widgets, examples, updates, and API docs to help you write your first Flutter app.
description: Flutter 上手起步，包括 widgets 介紹、範例程式碼、最新更新和 API 文件，幫助您撰寫第一個 Flutter 應用。
tags: Flutter中文文件
keywords: Flutter文件,Flutter漢語文件,Flutter開發導航
---

{% for card in site.data.docs_cards -%}
  {% capture index0Modulo3 %}{{ forloop.index0 | modulo:3 }}{% endcapture %}
  {% capture indexModulo3 %}{{ forloop.index | modulo:3 }}{% endcapture %}
  {% if index0Modulo3 == '0' %}
  <div class="card-deck mb-4">
  {% endif %}
    <a class="card" href="{{card.url}}">
      <div class="card-body">
        <header class="card-title">{{card.name}}</header>
        <p class="card-text">{{card.description}}</p>
      </div>
    </a>
  {% if indexModulo3 == '0' %}
  </div>
  {% endif %}
{% endfor -%}

**To see changes to the site since our last release,
see [What's new][].**

檢視最近網站更新的內容，請查閱
[文件網站更新內容歸檔][What's new]。

[What's new]: {{site.url}}/whats-new

## New to Flutter?

## 新接觸 Flutter 嗎

Once you've gone through [Get started][],
including [Write your first Flutter app][],
here are some next steps.

你可以從 [安裝和環境配置][Get Started] 開始，
也可以上手試試看 [第一個 Flutter 應用的開發][Write your first Flutter app]。

[Write your first Flutter app]: {{site.url}}/get-started/codelab

### Docs

### 文件

Coming from another platform? Check out Flutter for:
[Android][], [iOS][], [web][], [React Native][], and
[Xamarin.Forms][] developers.

看我們為各種已經有相關平臺開發經驗的開發者準備的文件：
- [給 Android 開發者的 Flutter 指南][Android]
- [給 iOS 開發者的 Flutter 指南][iOS]
- [給 React Native 開發者的 Flutter 指南][React Native]
- [給 Web 開發者的 Flutter 指南][Web]
- [給 Xamarin.Forms 開發者的 Flutter 指南][Xamarin.Forms]

[Building layouts][]
<br> Learn how to create layouts in Flutter,
  where everything is a widget.

[Flutter 中的佈局][Building layouts]
<br> 學習如何在 Flutter 中建立佈局，在 Flutter 裡，所有事物都是 widget。

[Understanding constraints][]
<br> Once you understand that "Constraints
  flow up. Sizes flow down. Parents set
  positions", then you are well on your
  way to understanding Flutter's layout model.

[理解佈局約束][Understanding constraints]
<br> 一旦你理解了 Constraints flow up. Sizes flow down. Parents set positions
這個思路之後，就能更好幫助你瞭解 Flutter 的佈局模型。

[Adding interactivity to your Flutter app][interactivity]
<br> Learn how to add a stateful widget to your app.

[為你的 Flutter 應用加入互動體驗][interactivity]
<br> 在 app 裡使用有狀態的 widget。

[A tour of the Flutter widget framework][]
<br> Learn more about Flutter's react-style framework.

[Widgets 介紹][A tour of the Flutter widget framework]
<br> 學習 Flutter 響應式框架的核心。

[FAQ][]
<br> Get the answers to frequently asked questions.

[常見問題][FAQ]
<br> 常見問題解答

[A tour of the Flutter widget framework]: {{site.url}}/development/ui/widgets-intro
[Android]: {{site.url}}/get-started/flutter-for/android-devs
[Building layouts]: {{site.url}}/development/ui/layout
[FAQ]: {{site.url}}/resources/faq
[Get started]: {{site.url}}/get-started/install
[interactivity]: {{site.url}}/development/ui/interactive
[iOS]: {{site.url}}/get-started/flutter-for/ios-devs
[React Native]: {{site.url}}/get-started/flutter-for/react-native-devs
[Understanding constraints]: {{site.url}}/development/ui/layout/constraints
[web]: {{site.url}}/get-started/flutter-for/web-devs
[Xamarin.Forms]: {{site.url}}/get-started/flutter-for/xamarin-forms-devs

### Videos

### 影片資源

Check out the Introducing Flutter series.
Learn Flutter basics like
[how do I make my first Flutter app?][first-app]
In Flutter, "everything is a widget"!
Learn more about `Stateless` and `Stateful`
widgets in [What is State?][]

我們在 YouTube 上有一個 [Flutter 頻道](https://www.youtube.com/c/flutterdev)，歡迎訂閱！
更多影片和播放列表介紹，以及社群製作的影片課程，
可以檢視我們的 [Flutter 技術影片資源][videos] 頁面。
同時，你可以關注 [“Google中國”的嗶哩嗶哩帳號](https://space.bilibili.com/64169458)
瞭解更多更全面的谷歌技術中文內容，
也可以關注 [“Flutter 社群”的嗶哩嗶哩帳號](https://space.bilibili.com/344928717)
瞭解更多來自社群的內容更新。

請檢視下述 Flutter 入門系列影片，透過 [建構第一個 Flutter 應用][] 學習 Flutter 基礎內容，Flutter 裡 “所有的事物都是 widget”，如果你想更好了解有狀態 `Stateful` 和無狀態 `Stateless` 的 widget，檢視影片 [什麼是狀態][]？

<div class="card-deck card-deck--responsive">
    <div class="video-card">
        <div class="card-body">
            <iframe style="max-width: 100%; width: 100%; height: 230px;" src="//player.bilibili.com/player.html?aid=557525809&bvid=BV1Se4y1Z74p&cid=818071255&page=1" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
        </div>
    </div>
    <div class="video-card">
        <div class="card-body">
            <iframe style="max-width: 100%; width: 100%; height: 230px;" src="//player.bilibili.com/player.html?aid=815087524&bvid=BV14G4y167Tu&cid=818431224&page=1" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
        </div>
    </div>
</div>

[first-app]: {{site.youtube-site}}/watch?v=xWV71C2kp38
[What is State?]: {{site.youtube-site}}/watch?v=QlwiL_yLh6E
[建構第一個 Flutter 應用]: https://www.bilibili.com/video/BV1Se4y1Z74p
[什麼是狀態]: https://www.bilibili.com/video/BV14G4y167Tu

{:.text-center}
#### Only have 60 seconds? Learn how to build and deploy a Flutter App!

#### 一分鐘！快速學習建構和部署 Flutter 應用

<div style="display: flex; align-items: center; justify-content: center; flex-direction: column;">
  <iframe style="max-width: 100%" width="560" height="315" src="{{site.youtube-site}}/embed/ZnufaryH43s" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

## Want to skill up?

## 提升內容

Dive deeper into how Flutter works under the hood!
Learn [why you write standalone widgets instead of
using helper methods][standalone-widgets] or
[what is "BuildContext" and how is it used][buildcontext]?

深入瞭解 Flutter 的工作原理，
瞭解為什麼要編寫獨立的 widget 而不是使用 help 方法，
以及 BuildContext 是什麼，以及如何使用：

<div class="card-deck card-deck--responsive">
    <div class="video-card">
        <div class="card-body">
            <iframe style="max-width: 100%; width: 100%; height: 230px;" src="{{site.youtube-site}}/embed/IOyq-eTRhvo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
        </div>
    </div>
    <div class="video-card">
        <div class="card-body">
            <iframe style="max-width: 100%; width: 100%; height: 230px;" src="{{site.youtube-site}}/embed/rIaaH87z1-g" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
        </div>
    </div>
</div>

[standalone-widgets]: {{site.youtube-site}}/watch?v=IOyq-eTRhvo   
[buildcontext]: {{site.youtube-site}}/watch?v=rIaaH87z1-g

To learn about all of the Flutter video series,
see our [videos][] page.

瞭解更多 Flutter 的影片系列內容，可以檢視 [影片學習資源][videos] 頁面。

We release new videos almost every week on the Flutter YouTube channel:

我們每週都會在 Flutter 的 YouTube 頻道更新影片，歡迎關注:

<a class="btn btn-primary" target="_blank" href="https://www.youtube.com/c/flutterdev">Flutter 的 YouTube 頻道</a>

**The documentation on this site reflects the
latest stable release of Flutter.**

本網站的文件基於 Flutter 最新的穩定版。

[videos]: {{site.url}}/resources/videos
