---
title: Start thinking declaratively
title: 狀態管理中的宣告式程式設計思維
description: How to think about declarative programming.
description: 如何理解宣告式程式設計。
tags: Flutter狀態管理
keywords: 宣告式程式設計,程式設計思維
prev:
  title: Intro
  title: 狀態 (State) 管理介紹
  path: /docs/development/data-and-backend/state-mgmt
next:
  title: Ephemeral versus app state
  title: 短時 (ephemeral) 和共享 (app) 狀態
  path: /docs/development/data-and-backend/state-mgmt/ephemeral-vs-app
---

If you're coming to Flutter from an imperative framework
(such as Android SDK or iOS UIKit), you need to start
thinking about app development from a new perspective.

如果你是從命令式框架（例如 Android SDK 或者 iOS UIKit）轉到 Flutter 應用，
那麼，你需要開始從一個新的角度來考慮 app 開發了。

Many assumptions that you might have don't apply to Flutter. For example, in
Flutter it's okay to rebuild parts of your UI from scratch instead of modifying
it. Flutter is fast enough to do that, even on every frame if needed.

因此，很多在命令式框架下的假設可能並不適用於 Flutter。
例如，在 Flutter 應用中這是可行的，重新建構你的部分介面，而不是直接去修改它。 
如果有需要的話，Flutter 甚至可以在每一幀上都很快做到這點。

Flutter is _declarative_. This means that Flutter builds its user interface to
reflect the current state of your app:

Flutter 應用是 **宣告式** 的，
這也就意味著 Flutter 建構的使用者介面就是應用的當前狀態。

<img src='/assets/images/docs/development/data-and-backend/state-mgmt/ui-equals-function-of-state.png' width="100%" alt="A mathematical formula of UI = f(state). 'UI' is the layout on the screen. 'f' is your build methods. 'state' is the application state.">

{% comment %}
Source drawing for the png above: : https://docs.google.com/drawings/d/1RDcR5LyFtzhpmiT5-UupXBeos2Ban5cUTU0-JujS3Os/edit?usp=sharing
{% endcomment %}

When the state of your app changes
(for example, the user flips a switch in the settings screen),
you change the state, and that triggers a redraw of the user interface.
There is no imperative changing of the UI itself
(like `widget.setText`)&mdash;you change the state,
and the UI rebuilds from scratch.

當你的 Flutter 應用的狀態發生改變時
（例如，使用者在設定介面中點選了一個開關選項）
你改變了狀態，這將會觸發使用者介面的重繪。
去改變使用者介面本身是沒有必要的
（例如 widget.setText ）&mdash;你改變了狀態，那麼使用者介面將重新建構。

Read more about the declarative approach to UI programming
in the [get started guide][].

在 [宣告式 UI 介紹][get started guide]
中你可以閱讀更多有關宣告式程式設計思維的資訊。

The declarative style of UI programming has many benefits.
Remarkably, there is only one code path for any state of the UI.
You describe what the UI should look
like for any given state, once&mdash;and that is it.

宣告式的程式設計風格有許多好處。值得注意的是，使用者介面任何狀態的改變都只有一種編碼途徑。
一旦給定任意狀態，你就描述了使用者介面應該長什麼樣，並且它就是這樣。

At first,
this style of programming might not seem as intuitive as the
imperative style. This is why this section is here. Read on.

剛開始的時候，這種編碼風格可能看起來不像命令式的那麼直觀。
這也是本章為什麼出現在這的原因。

[get started guide]: {{site.url}}/get-started/flutter-for/declarative
