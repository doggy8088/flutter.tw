---
title: Upgrading Flutter
title: 升級你的 Flutter 版本
short-title: Upgrading
short-title: 升級
description: How to upgrade Flutter.
description: 如何升級你的 Flutter 版本
tags: 升級,下載,Flutter版本
keywords: 中國映象,升級Flutter
---

No matter which one of the Flutter [release channels][]
you follow, you can use the `flutter` command to upgrade your
Flutter SDK or the packages that your app depends on.

無論你使用哪個 [Flutter 釋出渠道][release channels]，
你都可以使用 `flutter` 命令來更新 Flutter SDK 和應用所依賴的 packages。

## Upgrading the Flutter SDK

## 升級 Flutter SDK

To update the Flutter SDK use the `flutter upgrade` command:

如果要升級 Flutter SDK 的話，請使用 `flutter upgrade` 命令：

```terminal
$ flutter upgrade
```

This command gets the most recent version of the Flutter SDK
that's available on your current Flutter channel.

這個命令首先獲取你的 Flutter 渠道可用的最新的 Flutter SDK 版本。
接著這個命令更新你 app 依賴的每一個 package，到最新的相容版本。

If you want an even more recent version of the Flutter SDK,
switch to a less stable Flutter channel
and then run `flutter upgrade`.

如果你想使用一個更加新的 Flutter SDK 版本，按照下面的步驟切換
到相應的渠道 (channel)，接著再執行 `flutter upgrade`。

## Switching Flutter channels

## 切換 Flutter 釋出渠道

Flutter has three [release channels][]:
**stable**, **beta** and **master**.

{{site.alert.info}}
  The `dev` channel was retired as of Flutter 2.8.
{{site.alert.end}}

We recommend using the **{{site.sdk.channel}}** channel
unless you need a more recent release.

Flutter 有 [3 個釋出渠道][release channels]，分別是
**stable**、**beta** 和 **master**。
我們推薦使用 **{{site.sdk.channel}}** 渠道
除非你需要體驗最新更新的 Flutter 特性。

To view your current channel, use the following command:

要檢視你當前使用的哪個渠道，使用下面的命令：

```terminal
$ flutter channel
```

To change to another channel, use `flutter channel <channel-name>`.
Once you've changed your channel, use `flutter upgrade`
to download the Flutter SDK and dependent packages.
For example:

要切換到其它渠道，使用 `flutter channel <channel-name>`。
當你切換了渠道以後，使用 `flutter upgrade` 下載 Flutter SDK 和依賴的 packages。
例如：

```terminal
$ flutter channel beta
$ flutter upgrade
```

{{site.alert.note}}

  If you need a specific version of the Flutter SDK,
  you can download it from the [Flutter SDK releases][].

  如果你需要某個特定的 Flutter SDK 版本,
  你可以從 [SDK 版本][Flutter SDK releases] 頁面下載.

{{site.alert.end}}


## Upgrading packages

## 僅更新 packages

If you've modified your `pubspec.yaml` file, or you want to update
only the packages that your app depends upon
(instead of both the packages and Flutter itself),
then use one of the `flutter pub` commands.

如果你修改了 `pubspec.yaml` 檔案，
或者想僅更新專案依賴的 packages，
而不是同時更新 packages 和 Flutter SDK，
可以選擇使用下面提到的 `flutter pub` 命令。

To update to the _latest compatible versions_ of
all the dependencies listed in the `pubspec.yaml` file,
use the `upgrade` command:

為了把 `pubspec.yaml` 檔案裡列出的所有依賴
更新到 **最新的相容版本** ，可以使用使用 `upgrade` 命令：

```terminal
$ flutter pub upgrade
```

To identify out-of-date package dependencies and get advice
on how to update them, use the `outdated` command. For details, see
the Dart [`pub outdated` documentation]({{site.dart-site}}/tools/pub/cmd/pub-outdated).

如果需要自動判斷那些過時了的 package 依賴以及獲取更新建議，
現在你可以使用 `outdated` 命令。更多相關的資訊，
請參考 Dart 文件中關於 [`pub outdated`](https://dart.cn/tools/pub/cmd/pub-outdated) 的說明。

```terminal
$ flutter pub outdated
```

## Keeping informed

## 獲得最新通知

We publish breaking change announcements to the
[Flutter announcements mailing list][flutter-announce].
You can also ask questions on the [Flutter dev mailing list][flutter-dev].
Aside from subscribing to receive announcements,
we'd love to hear from you!

我們將在 [Flutter 通知郵件列表][flutter-announce] 上釋出重大更改的公告。
你也可以在 [Flutter 開發郵件列表][flutter-dev] 上提問！
除了訂閱接收公告外我們很樂意聽取您的意見！

[Flutter SDK releases]: {{site.url}}/development/tools/sdk/releases
[release channels]: {{site.repo.flutter}}/wiki/Flutter-build-release-channels
[flutter-announce]: {{site.groups}}/forum/#!forum/flutter-announce
[flutter-dev]: {{site.groups}}/forum/#!forum/flutter-dev
[pubspec.yaml]: {{site.dart-site}}/tools/pub/pubspec
