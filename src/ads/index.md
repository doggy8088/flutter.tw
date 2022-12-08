---
title: Ads support for Flutter
title: Flutter 對廣告的支援
description: Learn how to put ads in your Flutter apps.
description: 學習如何將廣告加入您的 Flutter 應用中。
---

Monetizing apps by using ads has been one of
the most popular requests for many Flutter developers.

在 Flutter 開發中，透過廣告獲利是開發者們非常熱門的需求之一。

Flutter ads support is available through the
[Google Mobile Ads SDK for Flutter (Beta)][plugin],
which works with both AdMob and AdManager.
This plugin supports a variety of ad formats,
including banner (inline and overlay),
interstitial, rewarded video, native ads,
and adaptive banner.

你可以藉助 [Google Flutter 版移動廣告 SDK][plugin] 在專案中整合廣告功能，
這款外掛可以與 AdMob 和 AdManager 一起使用。
支援多種廣告格式，包括橫幅廣告（內嵌和疊加）、插頁式廣告、獎勵影片廣告、原生廣告和自適應橫幅廣告。

![Pic showing different types of ads]({{site.url}}/assets/images/ads/GoogleMobileAdTypes.png){:width="100%"}

The following video tutorial,
[Monetizing apps with Flutter][],
shows how to get started with Ads:

以下的[透過 Flutter 應用獲利][Monetizing apps with Flutter]
影片課程展示瞭如何開始使用廣告功能:

<iframe width="560" height="315" src="https://player.bilibili.com/player.html?aid=289460171&bvid=BV1Vf4y147Er&cid=305747760&page=1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe><br>

The following resources can help you get started:

以下資源可以幫助你入門：

* To get the Google Mobile Ads SDK for Flutter,
  download the [google_mobile_ads plugin][plugin] from pub.dev.

  在 pub.dev 下載 [google_mobile_ads 外掛][plugin] 以獲取 Flutter 的 Google 移動廣告 SDK。

* For instructions on creating and loading an Ad with
  AdMob or AdManager, see the [detailed implementation guide][].

  有關如何使用 AdMob 或 AdManager 建立和載入廣告的相關說明，可以查閱
  [詳細實現的指導][detailed implementation guide] 文件。

* To learn how to implement overlay banner,
  interstitial, and rewarded ads, see the
  [Adding AdMob ads to a Flutter app][] codelab.

  想要了解如何實現疊加橫幅廣告、插頁式廣告和獎勵廣告，可以查閱
  [新增 AdMob 到 Flutter 應用中][Adding AdMob ads to a Flutter app] 的 Codelabs 課程。

* To learn how to implement inline banner
  and native ads, see the [Adding AdMob banner
  and native inline ads to a Flutter app][] codelab.

  想要了解如何實現內嵌橫幅廣告和本地廣告，可以查閱
  [新增 AdMob 的橫幅和資訊流廣告到 Flutter 應用中][Adding AdMob banner and native inline ads to a Flutter app]
  的 Codelabs 課程。

* If you experience any problems with the beta release,
  please [file an issue][].

  如果你在公開試用版本遇到了任何問題，請 [向我們反饋][file an issue] 。

[Adding AdMob Ads to a Flutter app]: {{site.codelabs}}/codelabs/admob-ads-in-flutter#0
[Adding AdMob banner and native inline ads to a Flutter app]: {{site.codelabs}}/codelabs/admob-inline-ads-in-flutter
[detailed implementation guide]: {{site.developers}}/admob/flutter
[file an issue]: {{site.github}}/googleads/googleads-mobile-flutter/issues
[Monetizing apps with Flutter]: {{site.youtube-site}}/watch?v=m0d_pbgeeG8&feature=youtu.be
[plugin]: {{site.pub-pkg}}/google_mobile_ads
