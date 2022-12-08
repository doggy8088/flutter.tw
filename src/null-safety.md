---
title: Null safety in Flutter
title: Flutter 中的空安全
description: Find out how to use non-nullable types in your Flutter code.
description: 學習如何在你的 Flutter 程式碼中使用非空型別
---

Flutter 2 supports null safety.
You can migrate your Flutter packages to use non-nullable types like this:

自 Flutter 2 起，Flutter 開始支援空安全。
你可以將你的 Flutter package 程式碼遷移，以使用非空型別，如下所示：

<?code-excerpt "basics/lib/main.dart (MyApp)"?>
```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  final int anInt = 3; // Cannot be null.
  final int? aNullableInt = null; // Can be null.

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Nullable Fields Demo',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Nullable Fields Demo'),
        ),
        body: Center(
          child: Column(
            children: [
              Text('anInt is $anInt.'),
              Text('aNullableInt is $aNullableInt.'),
            ],
          ),
        ),
      ),
    );
  }
}
```

For an instructor-led, free video workshop, check out the
following:

你可以觀看由講師帶來的免費學習工坊的影片：

<iframe width="560" height="315" src="https://player.bilibili.com/player.html?aid=888693780&bvid=BV1tK4y1u76N&cid=354814166&page=1" title="Bilibili video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Resources

## 資源

To learn more about null safety,
read these pages:

你可以閱讀以下內容，進一步學習空安全：

* [Sound null safety][]:
  The null safety homepage.
  Has basic information, with links to everything else you might need.

  [健全的空安全][Sound null safety]：
  空安全的首頁。它包括空安全的基礎資訊，以及你可能需要的其他所有內容的連結。

* [Understanding null safety][]:
  A deep dive into null safety, written by Dart engineer Bob Nystrom.

  [深入理解空安全][Understanding null safety]：
  對於空安全的深入解析，由 Dart 工程師 Bob Nystrom 撰寫。

When you're ready to start migrating your packages to null safety,
check out these pages:

如果你即將開始遷移你的 package 至空安全，你可以閱讀以下內容：

* [Migrating to null safety][]:
  Instructions for converting your packages to null safety.

  [遷移至空安全][Migrating to null safety]：
  將你的 package 轉換為空安全的指南。

* [Unsound null safety][]:
  Conceptual and practical information about mixed-mode programs,
  in which only some libraries are null safe.

  [非健全的空安全][Unsound null safety]：
  混合空安全模式的概念性和實質性內容，其中只有部分依賴處於空安全模式。

* [FAQ][]:
  Questions that have come up during migration to null safety.

  [空安全常見問題][FAQ]：
  遷移至空安全時遇到的問題。

## Known issues

## 已知的問題

Not all parts of the Flutter SDK support null safety yet,
as some parts still need additional work to
[migrate to null safety]({{site.dart-site}}/null-safety/migration-guide).

Flutter SDK 中的部分內容尚未支援空安全，
它們仍然需要一些 [遷移至空安全]({{site.dart-site}}/null-safety/migration-guide)
的額外工作。

We're currently aware of the following issues:
目前我們已經明確了以下的問題：

  * Migration of the pub.dev packages owned by the Flutter team
    is in progress. See pub.dev for
    [the current list]({{site.pub}}/packages?q=publisher%3Aflutter.dev&null-safe=1).
    We expect the majority of packages to be migrated in the coming weeks;
    the only exceptions we expect are the legacy `integration_tests` package,
    which is deprecated in favor of the version in the Flutter SDK itself
    (though see below), and some of the packages in
    <https://github.com/flutter/packages/tree/master/packages>
    which might take longer to be migrated.

    pub.dev 上 Flutter 團隊的 packages 正在進行遷移。你可以檢視
    [已遷移的 packages 列表]({{site.pub}}/packages?q=publisher%3Aflutter.dev&null-safe=1)。
    我們的計劃是在未來幾周將主要的 packages 遷移完成，唯一遺留的例外是 `integration_tests`，
    該 package 已被廢棄，取而代之的是 Flutter SDK 本身的工具（見下文）。
    部分在 <https://github.com/flutter/packages/tree/master/packages> 的 package
    可能需要更長的時間進行遷移。

  * Integration testing with
    [`flutter_driver`]({{site.url}}/cookbook/testing/integration/introduction) and 
    the version of `integration_test` in the Flutter SDK.

    使用 [`flutter_driver`]({{site.url}}/cookbook/testing/integration/introduction)
    以及 Flutter SDK 中對應版本的 `integration_test` 進行整合測試。

      * Currently, these methods do not support null safety on the host
        side of the test. You can drive a null-safe application, but the test
        itself won't use null-safe Dart.

        目前這些測試的方式，均未支援以空安全的方式在測試環境上執行。
        你可以執行空安全的應用，但測試本身不會使用 Dart 的空安全。

      * Depending on the `flutter_driver` and `integration_test` packages
        will limit your ability to pick up some already migrated dependencies
        such as `args`, `archive`, and `crypto`,
        since these packages themselves depend on the non-migrated versions.

        當你的專案依賴 `flutter_driver` 和 `integration_test` 時，
        你可能無法升級 `args`、`archive` 和 `crypto` 至已經遷移為空安全的版本，
        因為它們本身已經依賴了非空安全的版本（將導致版本衝突）。

    We expect to have this resolved in the first half of 2021.

    我們計劃在 2021 年上半年解決該問題。

If you are interested in seeing the latest updates before they
reach the beta and stable branches,
and are willing to live on the bleeding edge,
consider using dev builds (`flutter channel dev`).

如果你對還未合併到 beta 和 stable 渠道的新改動感興趣，
並且願意走在開發的前沿，可以考慮使用 dev 渠道 (`flutter channel dev`)。

[Migrating to null safety]: {{site.dart-site}}/null-safety/migration-guide
[FAQ]: {{site.dart-site}}/null-safety/faq
[Sound null safety]: {{site.dart-site}}/null-safety
[Understanding null safety]: {{site.dart-site}}/null-safety/understanding-null-safety
[Unsound null safety]: {{site.dart-site}}/null-safety/unsound-null-safety
