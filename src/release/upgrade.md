---
title: Upgrading Flutter
title: 升級你的 Flutter 版本
short-title: Upgrading
short-title: 升級
description: How to upgrade Flutter.
description: 如何升級你的 Flutter 版本
tags: 升級,下載,Flutter版本
keywords: 中國鏡像,升級Flutter
---

No matter which one of the Flutter release channels
you follow, you can use the `flutter` command to upgrade your
Flutter SDK or the packages that your app depends on.

不管你使用的是哪個 Flutter 釋出渠道，
你都可以使用 `flutter` 命令來升級你的
Flutter SDK 或者你的應用所依賴的 packages。

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

If you are using the **stable** channel
and want an even more recent version of the Flutter SDK,
switch to the **beta** channel using `flutter channel beta`,
and then run `flutter upgrade`.

如果你使用的是 **stable** 渠道，
並且想要一個更新的 Flutter SDK 版本，
可以使用 `flutter channel beta` 命令切換到 **beta** 渠道，
然後再執行 `flutter upgrade` 命令。

### Keeping informed

### 保持關注

We publish [migration guides][] for known breaking changes.

我們會發布 [遷移指南][migration guides] 來告知你已知的破壞性變更。

We send announcements regarding these changes to the
[Flutter announcements mailing list][flutter-announce].

我們會將這些變更的公告發送到 [Flutter 公告郵件列表][flutter-announce]。

To avoid being broken by future versions of Flutter,
consider submitting your tests to our [test registry][].

為了避免你的應用被未來的 Flutter 版本破壞，
可以考慮將你的測試提交到我們的 [測試登錄檔][test registry]。


## Switching Flutter channels

## 切換 Flutter 渠道

Flutter has two release channels:
**stable** and **beta**.

Flutter 有兩個釋出渠道：**stable** 和 **beta**。

### The **stable** channel

### **stable** 渠道

We recommend the **stable** channel for new users
and for production app releases.
The team updates this channel about every three months.
The channel might receive occasional hot fixes
for high-severity or high-impact issues.

我們推薦新使用者和生產環境使用 **stable** 渠道。
Flutter 團隊會每三個月更新一次這個渠道。
這個渠道可能會偶爾收到高優先順序或者高影響力問題的熱修復。

The continuous integration for the Flutter team's plugins and packages
includes testing against the latest **stable** release.

Flutter 團隊的外掛和 packages 的持續整合包括
針對最新的 **stable** 版本的測試。

The latest documentation for the **stable** branch
is at: <https://api.flutter.dev>

**stable** 分支的最新文件在： <https://api.flutter.dev>。

### The **beta** channel

### **beta** 渠道

The **beta** channel has the latest stable release.
This is the most recent version of Flutter that we have heavily tested.
This channel has passed all our public testing,
has been verified against test suites for Google products that use Flutter,
and has been vetted against [contributed private test suites][test registry].
The **beta** channel receives regular hot fixes
to address newly discovered important issues.

**beta** 渠道有最新的穩定版本。
這是我們最近測試過的 Flutter 版本。
這個渠道已經通過了我們所有的公開測試，
已經通過了使用 Flutter 的 Google 產品的測試套件的驗證，
並且已經通過了 [貢獻的私有測試套件][test registry] 的稽核。
**beta** 渠道會定期收到熱修復來解決新發現的重要問題。

The **beta** channel is essentially the same as the **stable** channel
but updated monthly instead of quarterly.
Indeed, when the **stable** channel is updated,
it is updated to the latest **beta** release.

**beta** 渠道本質上和 **stable** 渠道是一樣的，
只是更新頻率是每月一次，而不是每季度一次。
實際上，當 **stable** 渠道更新時，
它會更新到最新的 **beta** 版本。

### Other channels

### 其他渠道

We currently have one other channel, **master**.
People who [contribute to Flutter][] use this channel.

我們目前還有一個渠道，**master**。
[所有的 Flutter 貢獻者][contribute to Flutter] 都會將程式碼交到這個渠道。

This channel is not as thoroughly tested as
the **beta** and **stable** channels.

這個渠道沒有 **beta** 和 **stable** 渠道測試得那麼徹底。

We do not recommend using this channel as
it is more likely to contain serious regressions.

我們不推薦使用這個渠道，因為它更有可能包含嚴重的迴歸問題。

The latest documentation for the **master** branch
is at: <https://main-api.flutter.dev>

**master** 分支的最新文件在： <https://master-api.flutter.dev>。

### Changing channels

### 切換渠道

To view your current channel, use the following command:

要檢視你當前使用的哪個渠道，使用下面的命令：

```terminal
$ flutter channel
```

To change to another channel, use `flutter channel <channel-name>`.
Once you've changed your channel, use `flutter upgrade`
to download the latest Flutter SDK and dependent packages for that channel.
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
  you can download it from the [Flutter SDK archive][].

  如果你需要某個特定的 Flutter SDK 版本,
  你可以從 [SDK 版本][Flutter SDK archive] 頁面下載.

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

To update to the _latest possible version_ of
all the dependencies listed in the `pubspec.yaml` file,
use the `upgrade --major-versions` command:

為了把 `pubspec.yaml` 檔案裡列出的所有依賴
更新到 **最新的版本** ，可以使用使用 `upgrade --major-versions` 命令：

```terminal
$ flutter pub upgrade --major-versions
```

This also automatically update the constraints
in the `pubspec.yaml` file.

這個命令也會自動更新 `pubspec.yaml` 檔案中的約束條件。

To identify out-of-date package dependencies and get advice
on how to update them, use the `outdated` command. For details, see
the Dart [`pub outdated` documentation]({{site.dart-site}}/tools/pub/cmd/pub-outdated).

如果需要自動判斷那些過時了的 package 依賴以及獲取更新建議，
現在你可以使用 `outdated` 命令。更多相關的資訊，
請參考 Dart 文件中關於 [`pub outdated`](https://dart.tw.gh.miniasp.com/tools/pub/cmd/pub-outdated) 的說明。

```terminal
$ flutter pub outdated
```

[Flutter SDK archive]: {{site.url}}/release/archive
[flutter-announce]: {{site.groups}}/forum/#!forum/flutter-announce
[pubspec.yaml]: {{site.dart-site}}/tools/pub/pubspec
[test registry]: https://github.com/flutter/tests
[contribute to Flutter]: https://github.com/flutter/flutter/blob/main/CONTRIBUTING.md
[migration guides]: {{site.url}}/release/breaking-changes
