---
title: Nullable CupertinoThemeData.brightness
title: CupertinoThemeData.brightness 現可為空
description: CupertinoThemeData.brightness is now nullable, and it returns the value specified by the user (defaults to null) as is.
description: CupertinoThemeData.brightness 現可為空，並按原樣返回使用者指定的值（預設為 null）。
---

## Summary

## 概要

[`CupertinoThemeData.brightness`][] is now nullable.

[`CupertinoThemeData.brightness`][] 現可為空。

## Context

## 背景

[`CupertinoThemeData.brightness`][] is now used to
override `MediaQuery.platformBrightness` for Cupertino widgets.
Before this change, the [`CupertinoThemeData.brightness`][]
getter returned `Brightness.light` when it was set to null.

[`CupertinoThemeData.brightness`][] 現被用於覆蓋 Cupertino widgets 的 `MediaQuery.platformBrightness`。
在此之前，
[`CupertinoThemeData.brightness`][] 為空時返回 `Brightness.light`。

## Description of change

## 更改描述

Previously [`CupertinoThemeData.brightness`][]
was implemented as a getter:

此前 [`CupertinoThemeData.brightness`][] 由 getter 實現:

<!-- skip -->
```dart
Brightness get brightness => _brightness ?? Brightness.light;
final Brightness _brightness;
```

It is now a stored property:

現在它是一個儲存型別：

<!-- skip -->
```dart
final Brightness brightness;
```

## Migration guide

## 遷移指南

Generally [`CupertinoThemeData.brightness`][]
is rarely useful outside of the Flutter framework.
To retrieve the brightness for Cupertino widgets,
now use [`CupertinoTheme.brightnessOf`][] instead.

一般來說 [`CupertinoThemeData.brightness`][] 很少會在 Flutter 框架層外用到。
現在如果要檢索 Cupertino widgets 的亮度，
使用 [`CupertinoTheme.brightnessOf`][] 代替它即可。

With this change, it is now possible to override
`CupertinoThemeData.brightness` in a `CupertinoThemeData`
subclass to change the brightness override. For example:

在此更改後，
現在可以在 `CupertinoThemeData` 子類別中覆蓋 `CupertinoThemeData.brightness` 值來改變亮度。
例如：

<!-- skip -->
```dart
class AwaysDarkCupertinoThemeData extends CupertinoThemeData {
  Brightness brightness => Brightness.dark;
}
```

When a `CupertinoTheme` uses the above `CupertinoThemeData`,
dark mode is enabled for all its Cupertino descendants
that are affected by this `CupertinoTheme`.

當有一個 `CupertinoTheme` 使用上述 `CupertinoThemeData` 時，
所有受此 `CupertinoTheme` 影響的 Cupertino 類元件都將啟用深色模式。

## Timeline

## 時間表

Landed in version: 1.16.3<br>
In stable release: 1.17

實現版本：1.16.3<br>
穩定版本：1.17

## References

## 參考資料

Design doc:

設計文件：

* [Make `CupertinoThemeData.brightness nullable`][]

  [允許 `CupertinoThemeData.brightness` 為空值][Make `CupertinoThemeData.brightness nullable`]

API documentation:

API 文件：

* [`CupertinoThemeData.brightness`][]

Relevant issue:

相關 issue：

* [Issue 47255][]

Relevant PR:

相關 PR：

* [Let material `ThemeData` dictate brightness if `cupertinoOverrideTheme.brightness` is null][]

  [如果 `cupertinoOverrideTheme.brightness` 為空，則由 `ThemeData` 決定其亮度][Let material `ThemeData` dictate brightness if `cupertinoOverrideTheme.brightness` is null]


[`CupertinoTheme.brightnessOf`]: {{site.api}}/flutter/cupertino/CupertinoTheme/brightnessOf.html
[`CupertinoThemeData.brightness`]: {{site.api}}/flutter/cupertino/NoDefaultCupertinoThemeData/brightness.html
[Issue 47255]: {{site.repo.flutter}}/issues/47255
[Let material `ThemeData` dictate brightness if `cupertinoOverrideTheme.brightness` is null]: {{site.repo.flutter}}/pull/47249
[Make `CupertinoThemeData.brightness nullable`]: {{site.url}}/go/nullable-cupertinothemedata-brightness
