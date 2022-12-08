---
title: List of state management approaches
title: 狀態 (State) 管理參考
description: A list of different approaches to managing state.
description: 透過不同的方式來進行狀態管理。
tags: Flutter狀態管理
keywords: 參考資料
prev:
  title: Simple app state management
  title: 簡單的共享 (app) 狀態管理
  path: /docs/development/data-and-backend/state-mgmt/simple
---

State management is a complex topic.
If you feel that some of your questions haven't been answered,
or that the approach described on these pages
is not viable for your use cases, you are probably right.

狀態管理是一個相當複雜的話題。
如果您在瀏覽後發現一些問題並未得到解答，或者並不適用於您的具體需求場景，
自信些，您的實現就是對的。

Learn more at the following links,
many of which have been contributed by the Flutter community:

透過下面的連結瞭解更多的資訊，
其中有很多資訊都是由社群（第三方）提供。

## General overview

## 總體概覽

Things to review before selecting an approach.

在選擇一個具體內容前，您可以先檢視以下幾項。

* [Introduction to state management][],
  which is the beginning of this very section
  (for those of you who arrived directly to this _Options_ page 
  and missed the previous pages)

  [狀態管理的介紹][Introduction to state management]。
  這是本篇內容的總起。（當您直接跳轉到了該頁面，但錯過了
  其他頁面時，可以先從這裡開始閱讀）

* [Pragmatic State Management in Flutter][],
  a video from Google I/O 2019

  [Flutter 實用狀態管理 (Pragmatic State Management in Flutter)][Pragmatic State Management in Flutter]，
  來自 Google I/O 2019 的介紹影片。

* [Flutter Architecture Samples][], by Brian Egan

  [Flutter 架構例項 (Flutter Architecture Samples)][Flutter Architecture Samples]，
  Brian Egan 著。


[Flutter Architecture Samples]: https://fluttersamples.com/
[Introduction to state management]: {{site.url}}/development/data-and-backend/state-mgmt/intro
[Pragmatic State Management in Flutter]: {{site.youtube-site}}/watch?v=d_m5csmrf7I

## Provider

A recommended approach.

推薦的管理方式。

* [Simple app state management][], the previous page in this section

  [簡易的應用狀態管理 (Simple app state management)][Simple app state management]，
  這是本節內容的上篇。

* [Provider package][]

  [使用 Provider package][Provider package]

[Provider package]: {{site.pub-pkg}}/provider
[Simple app state management]: {{site.url}}/development/data-and-backend/state-mgmt/simple

## Riverpod

Riverpod, another good choice, is
similar to Provider and is compile-safe and testable.
Riverpod doesn't have a dependency on the Flutter SDK.

Riverpod 是另一個不錯的選擇，
它類似於 Provider，並且是編譯安全和可測試的。
Riverpod 不依賴於 Flutter SDK。

* [Riverpod][] homepage

  [Riverpod][] 專案主頁

* [Getting started with Riverpod][]

  [開始上手使用 Riverpod][Getting started with Riverpod]

[Getting started with Riverpod]: https://riverpod.dev/docs/getting_started
[Riverpod]: https://riverpod.dev/

## setState

The low-level approach to use for widget-specific, ephemeral state.

適用於較小規模 widget 的暫時性狀態的基礎管理方法。

* [Adding interactivity to your Flutter app][], a Flutter tutorial

  [為你的 Flutter 應用新增互動 (Adding interactivity to your Flutter app)][Adding interactivity to your Flutter app]，一篇 Flutter 的課程。

* [Basic state management in Google Flutter][], by Agung Surya

  [Flutter 中的基礎狀態管理 (Basic state management in Google Flutter)][Basic state management in Google Flutter]，Agung Surya 著。

[Adding interactivity to your Flutter app]: {{site.url}}/development/ui/interactive
[Basic state management in Google Flutter]: {{site.medium}}/@agungsurya/basic-state-management-in-google-flutter-6ee73608f96d

## InheritedWidget &amp; InheritedModel

The low-level approach used to communicate between ancestors and children
in the widget tree. This is what `provider` and many other approaches
use under the hood.

Widget tree 中不同層級間的 widget 通訊的基礎方法。這是諸如 `provider` 等
眾多方法的底層實現。

The following instructor-led video workshop covers how to
use `InheritedWidget`:

以下講師指導的影片 workshop 介紹瞭如何使用 `InheritedWidget`：

<iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=248744553&bvid=BV1Wv411W7yF&cid=354736130&page=1" title="Bilibili video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Other useful docs include:

其他有用的文件包括：

* [InheritedWidget docs][]

  [InheritedWidget 文件 (InheritedWidget docs)][InheritedWidget docs]

* [Managing Flutter Application State With InheritedWidgets][],
  by Hans Muller

  [使用 InheritedWidgets 管理 Flutter 應用狀態 (Managing Flutter Application State With InheritedWidgets)][Managing Flutter Application State With InheritedWidgets]，
  Hans Muller 著。

* [Inheriting Widgets][], by Mehmet Fidanboylu

  [繼承 Widgets (Inheriting Widgets)][Inheriting Widgets]，
  Mehmet Fidanboyly 著。

* [Using Flutter Inherited Widgets Effectively][], by Eric Windmill

  [高效地使用 Flutter 繼承 Widgets (Using Flutter Inherited Widgets Effectively)][Using Flutter Inherited Widgets Effectively]，Eric Windmill 著。

* [Widget - State - Context - InheritedWidget][], by Didier Bolelens

  [Widget - State - Context - InheritedWidget][]，Didier Bolelens 著。


[InheritedWidget docs]: {{site.api}}/flutter/widgets/InheritedWidget-class.html
[Inheriting Widgets]: {{site.medium}}/@mehmetf_71205/inheriting-widgets-b7ac56dbbeb1
[Managing Flutter Application State With InheritedWidgets]: {{site.flutter-medium}}/managing-flutter-application-state-with-inheritedwidgets-1140452befe1
[Using Flutter Inherited Widgets Effectively]: https://ericwindmill.com/articles/inherited_widget/
[Widget - State - Context - InheritedWidget]: https://www.didierboelens.com/2018/06/widget---state---context---inheritedwidget/

## Redux

A state container approach familiar to many web developers.

前端開發者較為熟悉的狀態容器實現。

* [Animation Management with Redux and Flutter][],
  a video from DartConf 2018 [Accompanying article on Medium][]

  [使用 Redux 在 Flutter 中管理動畫 (Animation Management with Redux and Flutter)][Animation Management with Redux and Flutter]，來自 DartConf 2018 的影片，
  [以及 Medium 的配套文章 (Accompanying article on Medium)][Accompanying article on Medium]。

* [Flutter Redux package][]

  [Flutter Redux 相依套件][Flutter Redux package]

* [Redux Saga Middleware Dart and Flutter][], by Bilal Uslu

  [Dart 與 Flutter 中的 Redux 中介軟體 Saga][Redux Saga Middleware Dart and Flutter]，Bilal Uslu 著

* [Introduction to Redux in Flutter][], by Xavi Rigau

  [Flutter 中的 Redux 介紹][Introduction to Redux in Flutter]，
  Xavi Rigau 著。

* [Flutter + Redux&mdash;How to make a shopping list app][],
  by Paulina Szklarska on Hackernoon

  [Flutter + Redux&mdash;建構一個購物列表 APP (Flutter + Redux&mdash;How to make a shopping list app)][Flutter + Redux&mdash;How to make a shopping list app]，
  釋出於 Hackernoon，Paulina Szklarska 著。

* [Building a TODO application (CRUD) in Flutter with Redux&mdash;Part 1][],
  a video by Tensor Programming

  [用 Flutter Redux 建構一個任務應用 (CRUD) &mdash;第一部分 (Building a TODO application (CRUD) in Flutter with Redux&mdash;Part 1)][Building a TODO application (CRUD) in Flutter with Redux&mdash;Part 1]，由 Tensor Programming 製作的影片。

* [Flutter Redux Thunk, an example][], by Jack Wong

  [Flutter Redux Thunk 範例 (Flutter Redux Thunk, an example)][Flutter Redux Thunk, an example]，Jack Wong 著。

* [Building a (large) Flutter app with Redux][], by Hillel Coren

  [使用 Redux 建構（大型）Flutter 應用 (Building a (large) Flutter app with Redux)][Building a (large) Flutter app with Redux]，Hillel Coren 著。

* [Fish-Redux–An assembled flutter application framework based on Redux][],
  by Alibaba

  [Fish-Redux&mdash;基於 Redux 封裝的 Flutter 應用框架 (Fish-Redux–An assembled flutter application framework based on Redux)][Fish-Redux–An assembled flutter application framework based on Redux]，阿里巴巴著。

* [Async Redux–Redux without boilerplate. Allows for both sync and async reducers][],
  by Marcelo Glasberg

  [非同步 Redux&mdash;沒有範本的 Redux，允許同步和非同步 reducers (Async Redux–Redux without boilerplate. Allows for both sync and async reducers)][Async Redux–Redux without boilerplate. Allows for both sync and async reducers]，Marcelo Glasberg 著。

* [Flutter meets Redux: The Redux way of managing Flutter applications state][],
  by Amir Ghezelbash

  [當 Flutter 遇見 Redux：以 Redux 的風格管理 Flutter 應用的狀態 (Flutter meets Redux: The Redux way of managing Flutter applications state)][Flutter meets Redux: The Redux way of managing Flutter applications state]，Amir Ghezelbash 著。

* [Redux and epics for better-organized code in Flutter apps][], by Nihad Delic

  [使用 Redux 和 redux_epics 更好的管理 Flutter 程式碼][Redux and epics for better-organized code in Flutter apps]，Nihad Delic 著。

* [Flutter_Redux_Gen - VS Code Plugin to generate boiler plate code][], by Balamurugan Muthusamy (BalaDhruv)

  [使用 Redux 更好地組織 Flutter 應用的程式碼以管理專案目標 (Redux and epics for better-organized code in Flutter apps)][Redux and epics for better-organized code in Flutter apps]，Nihad Delic 著。


[Accompanying article on Medium]: {{site.flutter-medium}}/animation-management-with-flutter-and-flux-redux-94729e6585fa
[Animation Management with Redux and Flutter]: {{site.youtube-site}}/watch?v=9ZkLtr0Fbgk
[Async Redux–Redux without boilerplate. Allows for both sync and async reducers]: {{site.pub}}/packages/async_redux
[Building a (large) Flutter app with Redux]: https://hillelcoren.com/2018/06/01/building-a-large-flutter-app-with-redux/
[Building a TODO application (CRUD) in Flutter with Redux&mdash;Part 1]: {{site.youtube-site}}/watch?v=Wj216eSBBWs
[Fish-Redux–An assembled flutter application framework based on Redux]: {{site.github}}/alibaba/fish-redux/
[Flutter Redux Thunk, an example]: {{site.medium}}/flutterpub/flutter-redux-thunk-27c2f2b80a3b
[Flutter meets Redux: The Redux way of managing Flutter applications state]: {{site.medium}}/@thisisamir98/flutter-meets-redux-the-redux-way-of-managing-flutter-applications-state-f60ef693b509
[Flutter Redux package]: {{site.pub-pkg}}/flutter_redux
[Flutter + Redux&mdash;How to make a shopping list app]: https://hackernoon.com/flutter-redux-how-to-make-shopping-list-app-1cd315e79b65
[Introduction to Redux in Flutter]: https://blog.novoda.com/introduction-to-redux-in-flutter/
[Redux and epics for better-organized code in Flutter apps]: {{site.medium}}/upday-devs/reduce-duplication-achieve-flexibility-means-success-for-the-flutter-app-e5e432839e61
[Redux Saga Middleware Dart and Flutter]: {{site.pub-pkg}}/redux_saga
[Flutter_Redux_Gen - VS Code Plugin to generate boiler plate code]: https://marketplace.visualstudio.com/items?itemName=BalaDhruv.flutter-redux-gen
  
## Fish-Redux

Fish Redux is an assembled flutter application framework
based on Redux state management.
It is suitable for building medium and large applications.

Fish Redux 是一個基於 Redux 狀態管理的組合式 Flutter 應用框架，
適用於建構中型和大型應用。

* [Fish-Redux-Library][] package, by Alibaba

  由阿里巴巴開發的 [Fish-Redux-Library][] package

* [Fish-Redux-Source][], project code

  [Fish-Redux-Source][]，工程程式碼

* [Flutter-Movie][], A non-trivial example demonstrating how
  to use Fish Redux, with more than 30 screens, graphql,
  payment api, and media player.

  [Flutter-Movie][] 展示如何使用 Fish Redux 的簡單範例應用，
  包含 30 多個頁面、graphql、支付 api 和媒體播放器等。


[Fish-Redux-Library]: {{site.pub-pkg}}/fish_redux
[Fish-Redux-Source]: {{site.github}}/alibaba/fish-redux
[Flutter-Movie]: {{site.github}}/o1298098/Flutter-Movie

## BLoC / Rx

A family of stream/observable based patterns. 

基於流/觀察者模式的系列。

* [Architect your Flutter project using BLoC pattern][],
  by Sagar Suri

  [使用 BLoC 模式建構你的 Flutter 專案 (Architect your Flutter project using BLoC pattern)][Architect your Flutter project using BLoC pattern]，
  Sagar Suri 著。

* [BloC Library][], by Felix Angelov

  [BLoC 庫 (BLoC Library)][BloC Library]，Felix Angelov 著。

* [Reactive Programming - Streams - BLoC - Practical Use Cases][],
  by Didier Boelens

  [響應式程式設計 - 流 - BLoC - 使用案例 (Reactive Programming - Streams - BLoC - Practical Use Cases)][Reactive Programming - Streams - BLoC - Practical Use Cases]，
    Didier Boelens 著。


[Architect your Flutter project using BLoC pattern]: {{site.medium}}/flutterpub/architecting-your-flutter-project-bd04e144a8f1
[BloC Library]: https://felangel.github.io/bloc
[Reactive Programming - Streams - BLoC - Practical Use Cases]: https://www.didierboelens.com/2018/12/reactive-programming---streams---bloc---practical-use-cases

## GetIt

A service locator based state management approach that
doesn't need a `BuildContext`.

* [GetIt package][], the service locator.
  It can also be used together with BloCs.
* [GetIt Mixin package][], a mixin that completes
  `GetIt` to a full state management solution.
* [GetIt Hooks package][], same as the mixin in
  case you already use `flutter_hooks`.
* [Flutter state management for minimalists][], by Suragch

{{site.alert.note}}
  To learn more, watch this short Package of the Week video on the GetIt package:

  <iframe class="full-width" src="{{site.youtube-site}}/embed/f9XQD5mf6FY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{{site.alert.end}}

[Flutter state management for minimalists]: {{site.medium}}/flutter-community/flutter-state-management-for-minimalists-4c71a2f2f0c1?sk=6f9cedfb550ca9cc7f88317e2e7055a0
[GetIt package]: {{site.pub-pkg}}/get_it
[GetIt Hooks package]: {{site.pub-pkg}}/get_it_hooks
[GetIt Mixin package]: {{site.pub-pkg}}/get_it_mixin

## MobX

A popular library based on observables and reactions.

一個基於觀察及響應的狀態管理常用庫。

* [MobX.dart, Hassle free state-management for your Dart and Flutter apps][]

  [MobX.dart 輕鬆管理你的 Dart 及 Flutter 應用狀態 (MobX.dart, Hassle free state-management for your Dart and Flutter apps)][MobX.dart, Hassle free state-management for your Dart and Flutter apps]

* [Getting started with MobX.dart][]

  [開始使用 MobX.dart (Getting started with MobX.dart)][Getting started with MobX.dart]

* [Flutter: State Management with Mobx][], a video by Paul Halliday

  [Flutter：使用 MobX 進行狀態管理 (Flutter: State Management with Mobx)][Flutter: State Management with Mobx]

[Flutter: State Management with Mobx]: https://www.bilibili.com/video/BV1Gt411K7JD/
[Getting started with MobX.dart]: https://mobx.netlify.com/getting-started
[MobX.dart, Hassle free state-management for your Dart and Flutter apps]: {{site.github}}/mobxjs/mobx.dart

## Flutter Commands

Reactive state management that uses the Command Pattern
and is based on `ValueNotifiers`. Best in combination with
[GetIt](#getit), but can be used with `Provider` or other
locators too.

基於 `ValueNotifiers` 的命令式的狀態管理，能與 [GetIt](#getit) 完美結合使用，
也可以與 `Provider` 或者其他 locators 配合使用。

* [Flutter Command package][]
* [RxCommand package][], 基於 `Stream` 的實現.


[Flutter Command package]: {{site.pub-pkg}}/flutter_command
[RxCommand package]: {{site.pub-pkg}}/rx_command

## Binder

A state management package that uses `InheritedWidget`
at its core. Inspired in part by recoil.
This package promotes the separation of concerns.

一個使用 `InheritedWidget` 作為核心實現的狀態管理庫。受到 recoil 的啟發，該庫提供了分治的解決方式。

* [Binder package][]

  [Binder 包][Binder package]

* [Binder examples][]

  [Binder 範例][Binder examples]

* [Binder snippets][], vscode snippets to be even more
  productive with Binder

  [Binder snippets][] 是一個 vscode 外掛，能夠將程式碼拆分以獲得更高的生產力


[Binder examples]: {{site.github}}/letsar/binder/tree/main/examples
[Binder package]: {{site.pub-pkg}}/binder
[Binder snippets]: https://marketplace.visualstudio.com/items?itemName=romain-rastel.flutter-binder-snippets

## GetX

A simplified reactive state management solution.

一個簡單的響應式狀態管理解決方案。

* [GetX package][]
* [Complete GetX State Management][], a video by Tadas Petra
* [GetX Flutter Firebase Auth Example][], by Jeff McMorris

[Complete GetX State Management]: {{site.youtube-site}}/watch?v=CNpXbeI_slw
[GetX package]: {{site.pub-pkg}}/get
[GetX Flutter Firebase Auth Example]: {{site.medium}}/@jeffmcmorris/getx-flutter-firebase-auth-example-b383c1dd1de2

## states_rebuilder

An approach that combines state management with a
dependency injection solution and an integrated router.
For more information, see the following info:

一種將狀態管理與依賴注入解決方案和整合路由器相結合的方法。
更多資訊，請參閱以下資訊：

* [States Rebuilder][] project code

  [States Rebuilder][] 專案程式碼

* [States Rebuilder documentation][]


[States Rebuilder]: {{site.github}}/GIfatahTH/states_rebuilder
[States Rebuilder documentation]: {{site.github}}/GIfatahTH/states_rebuilder/wiki

## Triple Pattern (Segmented State Pattern)

Triple is a pattern for state management that uses `Streams` or `ValueNotifier`.
This mechanism (nicknamed _triple_ because the stream always uses three
values: `Error`, `Loading`, and `State`), is based on the
[Segmented State pattern][].

For more information, refer to the following resources:

* [Triple documentation][]
* [Flutter Triple package][]
* [Triple Pattern: A new pattern for state management in Flutter][]
  (blog post written in Portuguese but can be auto-translated)
* [VIDEO: Flutter Triple Pattern by Kevlin Ossada][] (recorded in English)


[Triple documentation]: https://triple.flutterando.com.br/
[Flutter Triple package]: {{site.pub-pkg}}/flutter_triple
[Segmented State pattern]: https://triple.flutterando.com.br/docs/intro/
[Triple Pattern: A new pattern for state management in Flutter]: https://blog.flutterando.com.br/triple-pattern-um-novo-padr%C3%A3o-para-ger%C3%AAncia-de-estado-no-flutter-2e693a0f4c3e
[VIDEO: Flutter Triple Pattern by Kevlin Ossada]: {{site.youtube-site}}/watch?v=dXc3tR15AoA
