---
title: Flutter compatibility policy
title: Flutter 相容性策略
description: How Flutter approaches the question of breaking changes.
description: Flutter 團隊如何處理破壞性改動 (breaking changes) 的問題。
---

The Flutter team tries to balance the need for API stability with the
need to keep evolving APIs to fix bugs, improve API ergonomics,
and provide new features in a coherent manner.

Flutter 團隊努力平衡對 API 穩定性的需求和對 API 持續研發以修復 bug，提升其人機工程學體驗的需求。
並且我們會透過一種連貫的方式來提供新特性。

To this end, we have created a test registry where you can provide
unit tests for your own applications or libraries that we run
on every change to help us track changes that would break
existing applications. Our commitment is that we won't make any
changes that break these tests without working with the developers of
those tests to (a) determine if the change is sufficiently valuable,
and (b) provide fixes for the code so that the tests continue to pass.

為此，我們已經建立了一個測試登記。你可以在這裡針對每個改動為你的應用或庫提供單元測試，
以幫助我們追蹤對現存應用造成破壞的那些改動。
我們承諾，與這些測試的開發者進行合作以確定以下兩點之前，將不會有任何改動破壞這些測試。
（1）決定改動是否足夠有價值；（2）提供對程式碼的修復方案使得這些測試能夠繼續透過。

If you would like to provide tests as part of this program, please
submit a PR to the [flutter/tests repository][]. 
The [README][flutter-tests-readme] on that repository describes 
the process in detail.

作為該計劃的一部分，如果你想要提供一些測試方案，
請向 [flutter/tests repository][] 提交 PR。
這個儲存庫中的 [README][flutter-tests-readme] 檔案描述了具體流程。

[flutter/tests repository]: {{site.github}}/flutter/tests
[flutter-tests-readme]: {{site.github}}/flutter/tests#adding-more-tests

## Announcements and migration guides

## 公告和遷移指南

If we do make a breaking change (defined as a change that caused one
or more of these submitted tests to require changes), we will announce
the change on our [flutter-announce][]
mailing list as well as in our release notes.

如果我們確實釋出了一項破壞性改動（定義為：會導致一個或更多已提交的測試需要變化的改動），
我們將透過 [flutter-announce][] 郵件列表公佈，並且同時寫在釋出版說明上。

We provide a list of [guides for migrating code][] affected by
breaking changes.

我們提供一個受破壞性改動影響的
[遷移程式碼指南][guides for migrating code] 列表。

[flutter-announce]: {{site.groups}}/forum/#!forum/flutter-announce
[guides for migrating code]: {{site.url}}/release/breaking-changes

## Deprecation policy

## 廢棄政策

We will, on occasion, deprecate certain APIs rather than outright
break them overnight. This is independent of our compatibility policy
which is exclusively based on whether submitted tests fail, as
described above.

我們將會不定期的廢棄一些確定的 API，而不是直接讓他們不可用。
這將獨立於我們的相容性政策，只基於已提交的測試是否失敗，就如同之前描述的那樣。

Deprecated APIs are removed after a migration grace period. This grace
period is one calendar year after being released on the stable channel,
or after 4 stable releases, whichever is longer.

已經廢棄的 API 將會在一個寬限週期後移除。
以釋出至穩定版本時開始至一個日曆年，
或是 4 個穩定版本的釋出為一個寬限週期，以時間最長的為準。

When a deprecation does reach end of life, we follow the same procedures
listed above for making breaking changes in removing the deprecated API.

當已經廢棄的 API 到達了棄用期限時，我們會依照同上的步驟移除廢棄的 API。

## Dart and other libraries used by Flutter

## Dart 和其它被 Flutter 使用的函式庫

The Dart language itself has a [separate breaking-change policy][],
with announcements on [Dart announce][].

Dart 程式語言有 [自己的破壞性改動和棄用政策][separate breaking-change policy]，
並會在 [Dart 程式語言通知郵件列表][Dart announce] 裡公佈。

In general, the Flutter team doesn't currently have any commitment
regarding breaking changes for other dependencies.
For example, it's possible that a new version of
Flutter using a new version of Skia
(the graphics engine used by some platforms on Flutter)
or Harfbuzz (the font shaping engine used by Flutter)
would have changes that affect contributed tests.
Such changes wouldn't necessarily be accompanied by a
migration guide.

總而言之，關於其它依賴的破壞性改動，Flutter 團隊目前沒有做出任何承諾。
例如，有可能 Flutter 的一個新版本使用了新版本的 Skia（Flutter 使用的圖形引擎）
或者 Harfbuzz（Flutter 使用的字型形狀引擎），將會影響到已提交測試的改動。
這一類別的改動不一定會被寫入遷移指南。

[separate breaking-change policy]: {{site.github}}/dart-lang/sdk/blob/main/docs/process/breaking-changes.md
[Dart announce]: https://groups.google.com/a/dartlang.org/g/announce
