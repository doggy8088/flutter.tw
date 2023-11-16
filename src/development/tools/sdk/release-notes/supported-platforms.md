---
title: Supported platforms
title: 已支援的平台
short-title: Supported platforms
short-title: 已支援的平台
description: The platforms that Flutter supports by platform version.
description: Flutter 支援的平台以及版本號
---

## Support Tiers

## 支援級別

We define three tiers of support for the platforms on
which Flutter runs:

我們為 Flutter 的平台支援定義了三個級別：

1. Google-tested platforms
   are automatically tested at every commit
   by continuous integration testing.

   谷歌公司級別的 (Google-tested) 平台，
   這些平台“歸屬”在整個公司級別的產品測試體系下、
   Flutter 團隊每次提交時都會進行整合測試的平台。

1. Best-effort platforms, supported through community
   testing, are platforms we believe we support through
   coding practices and ad-hoc testing,
   but rely on the community for testing.

   盡力支援的平台，
   由社群維護的測試平台，我們相信這些平台是
   通過了編碼實踐和特別測試支援的，
   但依賴社群（非官方）進行測試。

1. Unsupported platforms, which are platforms that
   might work, but that the development team
   doesn't directly test or support.

   不受支援的平台，
   這些平台可能是可以正常工作的，
   但開發團隊不會直接測試或提供支援。
   
## Supported platforms

## 已支援的平台

As of the current release,
Flutter supports the following platforms as part of Google-tested and best-effort platform tier:

截止目前的穩定版釋出，
Flutter 支援下列谷歌級別和盡力支援的平台：

<div class="table-wrapper" markdown="1">
|Platform|Version                       |Channels |
|平台     | 版本                         | Flutter 版本釋出渠道 |
|--------|------------------------------|---------|
|Android | API 16 (Android 4.1) & above | All     |
|Android | API 16 (Android 4.1) 及更高的版本 | 所有    |
|iOS     | iOS 11 & above                | All     |
|iOS     | iOS 11 及更高的版本             | 所有     |
|Linux   | Debian, 64-bit               | All     |
|Linux   | Debian, 64-bit               | 所有     |
|macOS   | El Capitan (10.11) & above   | All     |
|macOS   | El Capitan (10.11)及更高的版本 | 所有     |
|Web     | Chrome 84  & above           | All     |
|Web     | Chrome 84  及更高的版本        | 所有     |
|Web     | Firefox 72.0 & above         | All     |
|Web     | Firefox 72.0 及更高的版本      | 所有     |
|Web     | Safari on El Capitan & above | All     |
|Web     | Safari on El Capitan 及更高的版本| 所有   |
|Web     | Edge 1.2.0 & above           | All     |
|Web     | Edge 1.2.0 及更高的版本        | 所有     |
|Windows | Windows 7 & above, 64-bit    | All     |
|Windows | 64 位 Windows 7 及更高的版本   | 所有     |
{:.table.table-striped}
</div>

All channels include master, beta,
and stable channels.

Flutter 版本釋出渠道中的「所有」渠道，
包括 master、beta 和 stable 釋出渠道。

### Google-tested platforms

## 谷歌公司級別的平台

<div class="table-wrapper" markdown="1">
|Platform|Version               |
|平台    |版本                   |
|--------|----------------------|
|Android |Android SDK 19–30*    |
|iOS     |14-15                 |
|Linux   |Debian 10             |
|Linux   |Ubuntu 18.04 LTS      |
|macOS   |Monterey (12) & above |
|Web     |Chrome 84             |
|Web     |Firefox 72.0          |
|Web     |Safari / Catalina     |
|Web     |Edge 1.2.0            |
|Windows |Windows 10            |
{:.table.table-striped}
</div>

\* Passing tests on Android SDK 19 also confers a passing result on SDK 20.
  This is because Android SDK 20 has additional support for Android Wear,
  but otherwise no new or deprecated API.

\* 請注意，Android SDK 19 的測試涵蓋了 Android SDK 20，
因為兩個版本之間的差異很小。

### Best-effort platforms

### 盡力支援的平台

<div class="table-wrapper" markdown="1">
|Platform|Version             |
|--------|--------------------|
|Android |Android SDK 16–18   |
|iOS     |iOS 11-13           |
|Linux   |Debian 11           |
|Linux   |Debian 9 & below    |
|Linux   |Debian 9 以及更低的版本|
|Linux   |Ubuntu 20.04        |
|Linux   |Ubuntu 22.04 (Aspirational Google-tested platform)        |
|Linux   |Ubuntu 22.04 (有望成為 Google-tested 的平台)                 |
|macOS   |El Capitan (10.11) - Big Sur (11)*   |
|Windows |Windows 11 (Aspirational Google-tested platform)          |
|Windows |Windows 11 (有望成為 Google-tested 的平台)          |
|Windows |Windows 8           |
|Windows |Windows 7           |
{:.table.table-striped}
</div>

\* Flutter 3.3 is the last stable release with macOS 10.11 through 10.13
best-effort support.

\* Flutter 3.3 將會是盡力支援 macOS 10.11 和 10.12 的最後一個版本。

### Unsupported platforms

### 不受支援的平台

<div class="table-wrapper" markdown="1">
|Platform|Version                                     |
|平台     |版本                                         |
|--------|--------------------------------------------|
|Android |Android SDK 15 & below                      |
|Android |Android SDK 15 及更低的版本                   |
|iOS     |[iOS 10 & below and `arm7v` 32-bit iOS][]|
|iOS     |[iOS 10 及更低的版本 `armv7` 架構的 32 位 iOS 系統][iOS 10 & below and `arm7v` 32-bit iOS]|
|Linux   |Any 32-bit platform                         |
|Linux   |任何 32 位的平台                              |
|Windows |Windows Vista & below                       |
|Windows |Windows Vista 及更低的版本                    |
|Windows |Any 32-bit platform                         |
|Windows |任何 32 位的平台                              |
{:.table.table-striped}
</div>

[iOS 10 & below and `arm7v` 32-bit iOS]: {{site.url}}/go/rfc-32-bit-ios-unsupported
