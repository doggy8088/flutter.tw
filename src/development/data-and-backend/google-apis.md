---
title: Google APIs
title: 使用 Google API
description: How to use Google APIs with Flutter.
description: 如何在 Flutter 中使用 Google API
---

<?code-excerpt path-base="googleapis/"?>

The [Google APIs package]({{site.pub-pkg}}/googleapis) exposes dozens of Google
services that you can use from Dart projects.

[Google APIs package]({{site.pub-pkg}}/googleapis)
提供了許多你可以從 Dart 專案中使用的 Google 服務。

This page describes how to use APIs that interact with end-user data by using
Google authentication.

本頁面描述瞭如何透過 Google 身份驗證，使用這些 API 和終端使用者資料互動。

Examples of user-data APIs include
[Calendar]({{site.pub-api}}/googleapis/latest/calendar.v3/calendar.v3-library.html),
[Gmail]({{site.pub-api}}/googleapis/latest/gmail.v1/gmail.v1-library.html), and
[YouTube]({{site.pub-api}}/googleapis/latest/youtube.v3/youtube.v3-library.html).

使用者資料 API 的例子包括 [Calendar]({{site.pub-api}}/googleapis/latest/calendar.v3/calendar.v3-library.html)、
[Gmail]({{site.pub-api}}/googleapis/latest/gmail.v1/gmail.v1-library.html) 
和 [YouTube]({{site.pub-api}}/googleapis/latest/youtube.v3/youtube.v3-library.html)。

{{site.alert.info}} 

  Note: The only APIs you should use directly from your Flutter
  project are those that access user data via Google authentication.
  
  注意：你直接使用自 Flutter 專案的 API，應該只有那些透過 Google 身份驗證存取使用者資料的 API。

  APIs that require
  [service accounts](https://cloud.google.com/iam/docs/service-accounts) **should
  not** be used directly from a Flutter application. Doing so requires shipping
  service credentials as part of your application, which is not secure. To use
  these APIs, we recommend creating an intermediate service.
  
  那些需要 [服務帳號](https://cloud.google.com/iam/docs/service-accounts) 的 API，
  不應該直接使用自 Flutter 應用。直接使用需要將服務證書作為應用程式的一部分，這是不安全的。
  我們推薦建立一箇中間服務，來使用這些 API。

<!-- TODO(kevmoo): Add link to public user guide when available. -->

{{site.alert.end}}

## Overview

## 概覽

To use Google APIs, follow these steps.

請遵循以下步驟使用 Google API。

1. Pick the desired API

   選擇所需的 API

1. Enable the API

   啟用 API 服務

1. Authenticate user with the required scopes

   使用所需的作用域對使用者進行身份驗證

1. Obtain an authenticated HTTP client

   獲取身份驗證後的 HTTP 客戶端

1. Create and use the desired API class

   建立並使用所需的 API 類

## 1. Pick the desired API

## 1. 選擇所需的 API

The documentation for [package:googleapis]({{site.pub-api}}/googleapis) lists
each API as a separate Dart library – in a `name.version` format. Let's look at
[`youtube.v3`]({{site.pub-api}}/googleapis/latest/youtube.v3/youtube.v3-library.html)
as an example.

文件 [package:googleapis]({{site.pub-api}}/googleapis) 採用 `name.version` 的形式，
列舉了每一個可以單獨作為 Dart 庫的 API。
一起看看 [`youtube.v3`]({{site.pub-api}}/googleapis/latest/youtube.v3/youtube.v3-library.html) 這個例子。

Each library may provide many types, but there is one _root_ class that ends in
`Api`. For YouTube, it's
[`YouTubeApi`]({{site.pub-api}}/googleapis/latest/youtube.v3/YouTubeApi-class.html).

每一個函式庫都可以提供多種型別，但是一定會有一個以 `Api` 結尾的 **根** 類別。
在 YouTube 中，根類就是 [`YouTubeApi`]({{site.pub-api}}/googleapis/latest/youtube.v3/YouTubeApi-class.html)。

Not only is the `Api` class the one you need to instantiate – see step 3 – but
it also exposes the scopes that represent the permissions needed to use the API.
Look under the
[Constants section]({{site.pub-api}}/googleapis/latest/youtube.v3/YouTubeApi-class.html#constants)
of the `YouTubeApi` class and you'll see the available scopes. To request access
to simply read (but not write) an end-users YouTube data, use the
[`youtubeReadonlyScope`]({{site.pub-api}}/googleapis/latest/youtube.v3/YouTubeApi/youtubeReadonlyScope-constant.html)
when authenticating the user.

`Api` 類不僅是你需要初始化的類——詳見步驟 3 ——它還暴露了使用該 API 所需許可權的作用域。
請看 `YouTubeApi` 類中 [常量]({{site.pub-api}}/googleapis/latest/youtube.v3/YouTubeApi-class.html#constants) 這一節，
你會看到可用的作用域有哪些。為了獲取終端使用者的 YouTube 資料的讀取（並非寫入）許可權，
使用者驗證時使用 [`youtubeReadonlyScope`]({{site.pub-api}}/googleapis/latest/youtube.v3/YouTubeApi/youtubeReadonlyScope-constant.html)。

<?code-excerpt "lib/main.dart (youtubeImport)"?>
```dart
/// Provides the `YouTubeApi` class.
import 'package:googleapis/youtube/v3.dart';
```

## 2. Enable the API

## 2. 啟用 API 服務

To use Google APIs you must have a Google account and a Google project. You also
need to enable your desired API.

使用 Google API，你必須有一個 Google 賬戶和一個 Google 專案。你還需要啟用所需的 API 服務。

In this example, you'd enable
[YouTube Data API v3](https://console.cloud.google.com/apis/api/youtube.googleapis.com).

在本範例中，你將需要啟用 [YouTube Data API v3](https://console.cloud.google.com/apis/api/youtube.googleapis.com) 服務。

For details, see the
[getting started instructions](https://cloud.google.com/apis/docs/getting-started).

詳情請看 [入門指南](https://cloud.google.com/apis/docs/getting-started)。

## 3. Authenticate the user with the required scopes

## 3. 使用所需的作用域對使用者進行身份驗證

Use the [google_sign_in]({{site.pub-pkg}}/google_sign_in) package to
authenticate users with their Google identity. You will have to configure signin
for each platform you want to support.

使用 [google_sign_in]({{site.pub-pkg}}/google_sign_in) 包對使用者進行 Google 身份驗證。
你必須為每一種想要支援的平台配置登入。

<?code-excerpt "lib/main.dart (googleImport)"?>
```dart
/// Provides the `GoogleSignIn` class
import 'package:google_sign_in/google_sign_in.dart';
```

When you instantiate the
[`GoogleSignIn`]({{site.pub-api}}/google_sign_in/latest/google_sign_in/GoogleSignIn-class.html)
class, you provide the desired scopes as discussed in the previous section.

當你初始化 [`GoogleSignIn`]({{site.pub-api}}/google_sign_in/latest/google_sign_in/GoogleSignIn-class.html) 類時，
你需要提供前面的小節中提到的所需的作用域。

<?code-excerpt "lib/main.dart (init)"?>
```dart
final _googleSignIn = GoogleSignIn(
  scopes: <String>[YouTubeApi.youtubeReadonlyScope],
);
```

Follow the instructions provided by
[package:google_sign_in]({{site.pub-pkg}}/google_sign_in) to allow a user to
authenticate.

按照 [package:google_sign_in]({{site.pub-pkg}}/google_sign_in) 中的介紹來進行使用者驗證。

Once authenticated, you must obtain an authenticated HTTP client.

一旦驗證完畢，你必須獲取一個驗證後的 HTTP 客戶端。

## 4. Obtain an authenticated HTTP client

## 4. 獲取身份驗證後的 HTTP 客戶端

The
[extension_google_sign_in_as_googleapis_auth]({{site.pub-pkg}}/extension_google_sign_in_as_googleapis_auth)
package provides an
[extension method]({{site.dart-site}}/guides/language/extension-methods) on
`GoogleSignIn`:
[`authenticatedClient`]({{site.pub-api}}/extension_google_sign_in_as_googleapis_auth/latest/extension_google_sign_in_as_googleapis_auth/GoogleApisGoogleSignInAuth/authenticatedClient.html).

[extension_google_sign_in_as_googleapis_auth]({{site.pub-pkg}}/extension_google_sign_in_as_googleapis_auth) 包
在 `GoogleSignIn` 中提供了一個 [擴充方法]({{site.dart-site}}/guides/language/extension-methods)：
[`authenticatedClient`]({{site.pub-api}}/extension_google_sign_in_as_googleapis_auth/latest/extension_google_sign_in_as_googleapis_auth/GoogleApisGoogleSignInAuth/authenticatedClient.html)。

<?code-excerpt "lib/main.dart (authImport)"?>
```dart
import 'package:extension_google_sign_in_as_googleapis_auth/extension_google_sign_in_as_googleapis_auth.dart';
```

You can listen to
[`onCurrentUserChanged`]({{site.pub-api}}/google_sign_in/latest/google_sign_in/GoogleSignIn/onCurrentUserChanged.html).
When event value is not `null`, you can create an authenticated client.

你可以監聽 [`onCurrentUserChanged`]({{site.pub-api}}/google_sign_in/latest/google_sign_in/GoogleSignIn/onCurrentUserChanged.html)。
當事件值不是 `null` 時，你可以建立一個身份驗證後的客戶端。

<?code-excerpt "lib/main.dart (signinCall)"?>
```dart
var httpClient = (await _googleSignIn.authenticatedClient())!;
```

This [`Client`]({{site.pub-api}}/http/latest/http/Client-class.html) instance
includes the nessesary credentials when invoking Google API classes.

[`Client`]({{site.pub-api}}/http/latest/http/Client-class.html) 例項
包含了呼叫 Google API 類時所需的憑證。

## 5. Create and use the desired API class

## 5. 建立並使用所需的 API 類

Use the API to create the desired API type and call methods, for instance:

使用 API 來建立所需的 API 型別和呼叫方法，例如：

<?code-excerpt "lib/main.dart (playlist)"?>
```dart
var youTubeApi = YouTubeApi(httpClient);

var favorites = await youTubeApi.playlistItems.list(
  ['snippet'],
  playlistId: 'LL', // Liked List
);
```

## More information

## 更多資訊

- The
  [`extension_google_sign_in_as_googleapis_auth` example]({{site.pub-pkg}}/extension_google_sign_in_as_googleapis_auth/example)
  is a working implementation of the concepts described on this page.
  
  範例 [`extension_google_sign_in_as_googleapis_auth`]({{site.pub-pkg}}/extension_google_sign_in_as_googleapis_auth/example) 
  是本頁面所述概念的一個可行的實現。
