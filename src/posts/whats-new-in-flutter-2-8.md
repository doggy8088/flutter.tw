---
title: Flutter 2.8 更新詳解
toc: true
---

北半球的冬意已至，黃葉與氣溫均隨風而落。年終的最後一個 Flutter 穩定版本 [已悄然來到你的面前](https://mp.weixin.qq.com/s/mr9F5VSRSOiV0QVxOYyT9g)。讓我們向 **Flutter 2.8** 打聲招呼～

本次更新包含了 **207 位貢獻者和 178 位稽核者** 的辛勤勞作，所有人共同產出了 **2424 個 PR**，關閉了 **2976 個 issue**。在此特別感謝本次釋出中最突出的社群貢獻者: 來自 VGV 的 Flutter 開發工程師 Bartosz Selwesiuk，他為 Web 平台的 camera 外掛並提交了 23 個 PR。

以上的所有產出讓 Flutter 引擎和開發者工具 (DevTools) 都有了非常顯著的效能提升，同時帶來的還有 Google 移動端廣告 SDK Flutter 版本的穩定版釋出、一系列針對 Firebase 的新功能和最佳化、Flutter WebView 3.0、新的 Flutter Favorite package、向桌面端穩定版邁出的一大步，以及支援更多 package 的新版 DartPad。讓我們一起來看看吧！

## 效能提升

Flutter 的首要目標是一如既往地保證其品質。我們花費了大量時間以確保 Flutter 在多種多樣的裝置上都能流暢且穩定地執行。

### 應用啟動效能

本次更新優化了應用啟動的延遲。我們在擁有一百萬行以上的程式碼量的 GPay 應用上進行了測試，以確保改動在實際生產的應用上有效。這些改動**將 GPay 在低端 Android 裝置上啟動的時間減少了約 50%**、**高階裝置上減少了約 10%**。

我們對 Flutter 呼叫 Dart VM 的 GC 策略也做了一些改進，以此避免在程式啟動期間出現不合時宜的 GC。例如，在 Android 裝置上渲染出第一幀前，Flutter [僅在 TRIM_LEVEL_RUNNING_CRITYCAL 及高於其等級的訊號出現時，通知 Dart VM 有記憶體壓力](https://github.com/flutter/flutter/issues/90551)。在本地測試中，**低端 Android 裝置的初始幀出現間隔時間最多減少了約 300ms**。

在先前的 Flutter 版本中，[出於謹慎考慮](https://github.com/flutter/engine/pull/29145#pullrequestreview-778935616)，在建立 PlatformView 時會阻塞平台執行緒。[在經過仔細的推理和測試後](https://github.com/flutter/flutter/issues/91711)，我們刪除了部分序列化的步驟，使得 GPay **在低端裝置上的啟動時間至少減少了 100ms**。

長久以來，在初始化首個 Dart isolate 前初始化預設的字型管理器會引入人為的延遲。由於它是首要的延遲瓶頸，所以 [將預設字型管理器的初始化延遲](https://github.com/flutter/engine/pull/29291) 到與首個 Dart isolate 同時執行，降低了啟動的延遲，並讓上述的所有啟動最佳化的表現更加明顯。

### 應用記憶體

由於 Flutter 會盡可能快地載入 Dart VM 的服務 isolate，並將其和繫結在應用內的 AOT 程式碼一併載入到記憶體中，這會導致 Flutter 開發人員在部分記憶體 [有限制的裝置上難以追蹤記憶體指標](https://github.com/flutter/flutter/issues/91382)。在 Flutter 2.8 版本中，Android 裝置上 Dart VM 的服務 isolate [已被拆分至單獨的 bundle 中](https://github.com/flutter/engine/pull/29245)，可以單獨載入，減少了在其載入前約 40MB 的記憶體使用。原本 Dart VM 向作業系統傳送 AOT 程式的記憶體用量的通知，已轉由一個無需多次讀取的檔案支援，後續的記憶體佔用量進一步減少了約 10%。因此，先前儲存了檔案資料複製的記憶體可以回收並用於其他用途。

### 效能分析

某些場景下，開發者希望能同時看到 Flutter 和 Android 的效能追蹤事件，又或者是在生產模式下檢視追蹤事件來更好地瞭解應用的效能問題。為了這一需求，Flutter 2.8 現在可以選擇在應用啟動後，將效能追蹤事件傳送至 Android 的事件記錄器，在生產模式下也同樣如此。

![Flutter 效能追蹤事件現在顯示在 Android systrace 記錄工具中（底部）](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/flutter-trace-event.png)

此外，一些開發人員想要更多的關於光柵快取行為的效能追蹤資訊，以減少製作動畫效果時的卡頓，這允許 Flutter 快速地對昂貴的、重複使用的圖片進行復用而不是重新繪製。效能追蹤中的新的 **流事件** 讓開發人員可以追蹤光柵快取圖片的生命週期。

### Flutter 開發者工具

對於除錯效能問題，新版的開發者工具 (DevTools) 添加了一個新的「增強追蹤」功能，用來幫助開發者診斷消耗較大的建構、佈局和繪製操作引起的 UI 卡頓。

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/enable-trace-feat.png)

啟用任何一個追蹤功能後，時間軸中將視情況展示 Widget 的建構、RenderObject 佈局和 RenderObject 繪製的事件。

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/render-draw-event.png)

此外，新版的開發者工具也增加了應用啟動效能的分析支援。該配置檔案包含了從 Dart VM 初始化到第一幀 Flutter 渲染的 CPU 樣本。在你按下「Profile app start up」按鈕並載入應用啟動配置檔案後，你將看到為配置檔案選擇了「AppStartUp」標籤。你還可以透過在可用使用者標籤列表中選擇此使用者標籤過濾器（如果存在）來載入應用啟動配置檔案。選擇此標籤會顯示你的應用啟動的個人資料資料。

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/profile-app-start.png)

### Web 平台的平臺視圖 (PlatformView)

不僅僅是 Android 和 iOS 平台獲得了效能提升，本次釋出同時包含了對 Flutter Web 平臺視圖的效能最佳化。平臺視圖是從宿主平台向 Flutter 嵌入 UI 元件的媒介。Flutter Web 使用 [HtmlElementView](https://api.flutter-io.cn/flutter/widgets/HtmlElementView-class.html) widget 實現了這一功能，讓你能在 Flutter Web 應用中嵌入 HTML 元素。如果你正在使用 `google_maps_flutter` 外掛或 `video_player` 外掛的 Web 版本，或者你正在遵循 Flutter 團隊關於 [如何最佳化網路上顯示圖像](https://flutter.cn/docs/development/platform-integration/web-images#use-img-in-a-platform-view) 的建議，那說明你已經在使用平臺視圖了。

在之前版本的 Flutter 中，嵌入平臺視圖會建立一個新的 canvas，每嵌入一個平臺視圖都會新增一個 canvas。
建立額外的 canvas 是十分消耗效能的操作，因為每個 canvas 的大小都與整個視窗相等。在 Flutter 2.8 中，將 [複用為先前的平臺視圖建立的 canvas](https://github.com/flutter/engine/pull/28087)。因此，你不會在應用的整個生命週期內產生每秒 60 倍的成本，而是隻有一次建立的成本。這意味著你可以在 Web 應用中擁有多個 `HtmlElementView` 例項而不會降低效能，同時還可以減少使用平臺視圖時的滾動卡頓。

## 生態

Flutter 不僅僅是框架、引擎和工具——pub.dev 上現有超過 2w 個與 Flutter 相容的套件和外掛，而且每天都在增加。Flutter 開發人員大量的日常操作也是龐大的生態系統的一部分，所以讓我們來看看自上一個版本以來 Flutter 生態系統中有什麼改變。

### 適用於 Flutter 廣告的 Google 廣告

首先也是最重要的是，[Google Mobile SDK for Flutter 已於 11 月正式釋出](https://medium.com/flutter/announcing-general-availability-for-the-google-mobile-ads-sdk-for-flutter-574e51ea6783)。此版本支援 5 種廣告格式，集成了 AdMob 和 Ad Manager 支援，幷包含一個新的中轉功能的測試版，可以幫助你最佳化廣告展現的效果。有關將 Google Ads 整合到 Flutter 應用以及其他貨幣化選項的更多資訊，請檢視 [Flutter 網站上的頁面](https://flutter.cn/monetization)。

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/admob-sdk-flutter-ga.png)

### WebView 3.0

這次 Flutter 附帶的另一個新版本是 [webview_flutter 外掛](https://pub.flutter-io.cn/packages/webview_flutter) 的 3.0 版本。因為新功能的數量增加，我們提升了主要版本號，但也因為 Web 檢視在 Android 上的工作方式可能發生了重大變化。在之前的 `webview_flutter` 版本中，Hybrid composition 已經可用，但不是預設的。而現在它修復了先前預設以虛擬顯示模式執行的許多問題。
根據使用者反饋和我們的問題追蹤，我們認為是時候讓 Hybrid composition 成為預設設定了。此外，`webview_flutter` 還增加了一些呼聲極高的功能: 

* 支援使用 POST 和 GET 來載入內容
* 載入檔案或字串內容為 HTML
* 支援透明背景
* 在載入內容前設定 Cookies

此外，在 3.0 版本中，`webview_flutter` 為新平台提供了初步支援: Flutter Web。已經有很多人要求能夠在 Flutter Web 應用中託管 Web 檢視，這允許開發者利用單個原始碼庫建構移動或 Web 應用。在 Flutter Web 應用中託管 Web 檢視是什麼樣的？從編寫程式碼的角度來看，其實是一樣的: 

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';
import 'package:webview_flutter_web/webview_flutter_web.dart';

void main() {
  runApp(const MaterialApp(home: HomePage()));
}

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  @override
  void initState() {
    super.initState();

    // required while web support is in preview
    if (kIsWeb) WebView.platform = WebWebViewPlatform();
  }

  @override
  Widget build(BuildContext context) => Scaffold(
    appBar: AppBar(title: const Text('Flutter WebView example')),
    body: const WebView(initialUrl: 'https://flutter.dev'),
  ;
}
```

在 Flutter Web 上執行時，它會按你的預期工作: 

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/webview_flutter-demo-in-2-8.gif)

請注意，當前 `webview_flutter` 的 web 實現有許多限制，因為它是使用 iframe 建構的，
iframe 僅支援簡單的 URL 載入，無法控制載入的內容或與載入的內容互動。
但是，由於需求呼聲太高，我們決定將 `webview_flutter_web` 作為未經認可的外掛提供。
如果你想嘗試一下，請將以下內容新增到你的 `pubspec.yaml` 中: 

```yaml
dependencies:
  webview_flutter: ^3.0.0
  webview_flutter_web: ^0.1.0 # 顯式依賴未經認可的外掛
```

如果你對 `webview_flutter` v3.0 有任何反饋，無論是否是關於 Web 平台，請 [將問題提交到 Flutter 儲存庫中](https://github.com/flutter/flutter/issues)。此外，如果你之前沒有使用過 webview 或者想複習一下，請檢視 [新的 webview codelab](https://codelabs.developers.google.com/codelabs/flutter-webview)，它將帶你逐步完成在 Flutter 應用中託管 Web 內容的過程。

### Flutter Favorites 專案

Flutter 生態系統委員會再次召開會議，將以下 package 指定為 Flutter Favorite 的 package: 

* 新路由 API (又名 Navigator 2) 的三個自訂路由 package: [beamer](https://pub.flutter-io.cn/packages/beamer)、[routemaster](https://pub.flutter-io.cn/packages/routemaster) 和 [go_router](https://pub.flutter-io.cn/packages/go_router)；
* [drift](https://pub.flutter-io.cn/packages/drift): 對 Flutter 和 Dart 已經功能強大且流行的響應式永續性庫的重新命名，基於 sqlite 建構；
* [freezed](https://pub.flutter-io.cn/packages/freezed): 一個 Dart「語言補丁」，為定義模型、複製物件、模式匹配等提供簡單的語法；
* [dart_code_metrics](https://pub.flutter-io.cn/packages/dart_code_metrics): 一個幫助您分析和提高程式碼品質的靜態分析工具；
* 以及有著漂亮介面的 package: [flex_color_scheme](https://pub.flutter-io.cn/packages/flex_color_scheme)、[flutter_svg](https://pub.flutter-io.cn/packages/flutter_svg)、[feedback](https://pub.flutter-io.cn/packages/feedback)、[toggle_switch](https://pub.flutter-io.cn/packages/toggle_switch) 和 [auto_size_text](https://pub.flutter-io.cn/packages/auto_size_text)。

![使用 flex_color_scheme 建構的可靈活摺疊的應用](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/flex_color_scheme-demo-in-2-8.gif)

祝賀這些 package 的作者，並感謝你透過你的辛勤工作支援 Flutter 社群。如果你有興趣提名你最喜歡的 Flutter package 加入 Flutter Favorite 嘉獎，請按照 [Flutter Favorite 計劃頁面](https://flutter.cn/docs/development/packages-and-plugins/favorites) 上的指南和說明進行操作。

### 特定平台的外掛

如果你是 package / 外掛作者，你需要宣告和實現支援哪些平台。如果你正在使用特定於平台的原生程式碼建構外掛，你可以 [使用專案 pubspec.yaml 中的 pluginClass 屬性](https://flutter.cn/docs/development/packages-and-plugins/developing-packages#plugin-platforms)
來實現，該屬性將指定提供原生功能的原生類別名稱: 

```yaml
flutter:
  plugin:
    platforms:
      android:
        package: com.example.hello
        pluginClass: HelloPlugin
      ios:
        pluginClass: HelloPlugin
```

然而，隨著 Dart FFI 變得更加成熟，有可能使用 100% 的 Dart 實現特定平台的功能，就像 [path_provider_windows package](https://pub.flutter-io.cn/packages/path_provider_windows) 所做的那樣。在這種情況下，你沒有任何本地類可以使用，但你仍然希望將你的外掛指定為僅支援某些平台。此時你可以改用 `dartPluginClass` 屬性: 

```yaml
flutter:
  plugin:
    implements: hello
    platforms:
      windows:
        dartPluginClass: HelloPluginWindows
```

經過這樣的設定後，即使你沒有任何本機程式碼，也可以為特定平台客製外掛。你還必須提供 Dart 外掛的類，有關詳細內容，你可以在 [Flutter 文件上閱讀 Dart 平台實現文件](https://flutter.cn/docs/development/packages-and-plugins/developing-packages#dart-only-platform-implementations) 以瞭解更多。

## Firebase 相關的更新

Flutter 生態中另一個重要組成是 FlutterFire，大約有三分之二的 Flutter 應用都在使用它。這次穩定版增加了一系列新的功能，方便開發者們更好的在 Flutter 裡使用 Firebase:

- 所有 FlutterFire 外掛都從測試版畢業，「成長」為穩定版
- DartPad 開始支援部分 Firebase 服務，方便線上使用和體驗
- 更方便建構認證和在即時查詢 Firestore 資料的 UI 介面
- Flutter 中使用 Firestore Object/Document 對映的支援進入 Alpha 版

### 生產品質

[The FlutterFire plugins](http://firebase.flutter.dev/) 幾乎已經全部從測試版轉為文穩定版，可用於生產環境。

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/flutterfire-website.png)

Android、iOS 和網頁版的外掛已轉為穩定版，包括 [Analytics](https://firebase.flutter.dev/docs/analytics/overview)、[Dynamic Links](https://firebase.flutter.dev/docs/dynamic-links/overview)、[In-App Messaging](https://firebase.flutter.dev/docs/in-app-messaging/overview/)、[Performance Monitoring](https://firebase.flutter.dev/docs/performance/overview)、[Realtime Database](https://firebase.flutter.dev/docs/database/overview)、[Remote Config](https://firebase.flutter.dev/docs/remote-config/overview) 和 [Installations](https://firebase.flutter.dev/docs/installations/overview)。有些 Firebase 庫本身在部分平臺上仍處於測試階段，所以它的 Flutter 外掛也會是測試版狀態，比如 App Check 在 macOS 平台。但類似即時資料庫 (Realtime Database)、分析 (Analytics)、遠端配置 (Remote Config) 等 FlutterFire 外掛已經在生產環境中可用了，可以選擇試試看！

### Firebase 初始化僅需在 Dart 程式碼中配置即可

因為這些 package 已經達到生產品質，現在你 [只用在 Dart 程式碼中配置](https://github.com/FirebaseExtended/flutterfire/pull/6549)，就可以完成 Firebase 的初始化了。

```dart
import 'package:firebase_core/firebase_core.dart';
import 'firebase_options.dart'; // generated via `flutterfire` CLI

Future<void> main() async {
  // initialize firebase across all supported platforms
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(options: DefaultFirebaseOptions.currentPlatform);

  runApp(MyApp());
}
```

在 `firebase_options.dart` 檔案中定義的各種配置資訊，就可以在選擇的每個支援的平台裡初始化 Firebase: 

```dart
static const FirebaseOptions web = FirebaseOptions(
  apiKey: 'AIzaSyCZFKryCEiKhD0JMPeq_weJguspf09h7Cg',
  appId: '1:111079797892:web:b9195888086158195ffed1',
  messagingSenderId: '111079797892',
  projectId: 'flutterfire-fun',
  authDomain: 'flutterfire-fun.firebaseapp.com',
  storageBucket: 'flutterfire-fun.appspot.com',
  measurementId: 'G-K029Y6KJDX',
);
```

如果你想為每個平台的初始化自訂資料結構的話，請使用這個 `flutterfire` 命令列工具完成: 

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/flutterfire-cli.png)

這個命令列工具會從每個平台的子資料夾中找到唯一的 bundle ID，進而用它來查詢以及建立匹配的特定平臺下的 Firebase 工程詳情。這意味著你將省去下載 `.json`檔案到 Android 工程、下載 `.plist` 檔案到 iOS 和 macOS 工程的時間了，當然，也無需再複製貼上程式碼到你的 Web 工程了。換句話說，無論你的應用要為哪些平台初始化 Firebase，這句程式碼都可以幫你做到。當然，這也可能不是唯一一處初始化程式碼的地方，比如你需要在 Android 或 iOS 中建立 Crashlytics 除錯符號 (dSYM) 的時候。但至少可以針對新的 Firebase 工程能夠快速跑起來。

### 在 DartPad 中使用 Firebase

由於我們可以只在 Dart 程式碼中初始化並使用 FlutterFire，那 DartPad 自然也就支援使用 Firebase 啦: 

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/dartpad-flutterfire-demo.png)

這裡有一個使用 Flutter 和 Firebase 建構的線上聊天的示範，所有這些都可以在 DartPad 中直接使用而無需安裝任何內容。DartPad 對 Firebase 的支援已經包括了核心 API、身份驗證和 Firestore，隨著時間的推進，未來 DartPad 會支援更多 Firebase 服務。

另一個支援是在 FlutterFire 文件中直接內嵌了 DartPad 例項，比如 [Firestore 的範例頁面](https://firebase.flutter.dev/docs/firestore/example/):

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/firestore-docs-with-dartpad.png)

在這個範例中，你將看到 Cloud Firestore 的文件以及 [範例應用](https://github.com/FirebaseExtended/flutterfire/tree/master/packages/cloud_firestore/cloud_firestore/example) 的程式碼，並且可以在瀏覽器中直接執行和編輯，無需安裝任何軟體。

### Firebase 使用者介面

大多數使用者都有身份驗證的流程，包括但不僅限於透過郵箱和密碼或者第三方帳號登入等。使用 Firebase 身份認證 (Authentication) 服務，你就可以完成建立新使用者、郵箱認證、重置密碼，甚至是簡訊兩步驗證、使用手機號碼登入、將多個帳號合併為一個帳號等功能。直到今天，開發者們仍需要自行來完成這些邏輯和 UI。

今天我們很希望大家嘗試一個新的 package，名為 [flutterfire_ui](https://pub.dev/packages/flutterfire_ui)。這個 package 可以用少量的程式碼建構一個基本的身份驗證體驗，例如，在 Firebase 專案中設定了使用郵箱和 Google 帳號登入:

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/firebase-console-auth-page.png)

透過這個配置你可以透過下面的程式碼建構一個身份驗證:

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'package:firebase_auth/firebase_auth.dart';
import 'package:flutterfire_ui/auth.dart';
import 'firebase_options.dart';

Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(options: DefaultFirebaseOptions.currentPlatform);
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) => MaterialApp(
        home: AuthenticationGate(),
      );
}

class AuthenticationGate extends StatelessWidget {
  const AuthenticationGate({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) => StreamBuilder<User?>(
        stream: FirebaseAuth.instance.authStateChanges(),
        builder: (context, snapshot) {
          // User is not signed in - show a sign-in screen
          if (!snapshot.hasData) {
            return SignInScreen(
              providerConfigs: [
                EmailProviderConfiguration(),
                GoogleProviderConfiguration(
                  clientId: 'xxxx-xxxx.apps.googleusercontent.com',
                ),
              ],
            );
          }

          return HomePage(); // show your app’s home page after login
        },
      );
}
```

這段程式碼將首先初始化 Firebase，然後會發現使用者尚未登入進而顯示登入介面，`SigninScreen` widget 配置了郵件和 Google 帳號登入，程式碼裡還使用了 `firebase_auth` package 來監測使用者的身份驗證狀態，因此一旦使用者登入完成，你就可以顯示接下來的應用內容。使用這個程式碼片段，你將可以在所有 Firebase 支援的平臺上完成身份驗證功能。

再加入一些其他配置的話，你還可以新增一些圖像和自訂文字 (詳情見 [本文件](https://firebase.flutter.dev/docs/ui/overview))，從而為你提供更全面的使用者身份驗證體驗: 

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/flutterfire_ui-auth-mobile.png)

上面這個截圖是移動端的身份認證，不過因為 `flutterfire_ui` 的 UI 是響應性設計，因此在桌面瀏覽器上，它會是這樣的效果:

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/flutterfire_ui-auth-desktop-browser.png)

使用者可以使用郵箱地址和密碼直接完成登入，如果他們選擇使用透過谷歌身份驗證登入，不論是在移動端、Web 端還是桌面端，則將會看到常見的 Google 身份驗證流程。如果使用者還沒有賬戶，他們可以點選註冊按鈕進入註冊流程。使用者登入之後就會有電子郵件驗證、密碼重置、登出以及社交賬戶繫結功能。透過電子郵件和密碼的身份驗證適用於所有平台，並支援使用 Google、Facebook 和 Twitter 帳號登入，以及在 iOS 系統上支援透過 Apple ID 登入。`flutterfire_ui` 的身份認證支援多種場景和導航方案以及自訂和本地化選項等。檢視 [FlutterFire UI 的文件](https://firebase.flutter.dev/docs/ui/overview/) 瞭解更多。

此外，身份認證不是 `flutterfire_ui` 唯一支援的 Flutter UI 的相關功能。它還可以向用戶展示一個來自 Firebase 資料查詢並無限滾動的資料列表，這個版本也包含了一個 `FirestoreListView` 可以使用: 

```dart
class UserListView extends StatelessWidget {
  UserListView({Key? key}) : super(key: key);

  // live Firestore query
  final usersCollection = FirebaseFirestore.instance.collection('users');

  @override
  Widget build(BuildContext context) => Scaffold(
        appBar: AppBar(title: const Text('Contacts')),
        body: FirestoreListView<Map>(
          query: usersCollection,
          pageSize: 15,
          primary: true,
          padding: const EdgeInsets.all(8),
          itemBuilder: (context, snapshot) {
            final user = snapshot.data();

            return Column(
              children: [
                Row(
                  children: [
                    CircleAvatar(
                      child: Text((user['firstName'] ?? 'Unknown')[0]),
                    ),
                    const SizedBox(width: 8),
                    Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      mainAxisAlignment: MainAxisAlignment.center,
                      mainAxisSize: MainAxisSize.min,
                      children: [
                        Text(
                          '${user['firstName'] ?? 'unknown'} '
                          '${user['lastName'] ?? 'unknown'}',
                          style: Theme.of(context).textTheme.subtitle1,
                        ),
                        Text(
                          user['number'] ?? 'unknown',
                          style: Theme.of(context).textTheme.caption,
                        ),
                      ],
                    ),
                  ],
                ),
                const Divider(),
              ],
            );
          },
        ),
      );
}
```

實際的執行效果如下: 

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/flutterfire_ui-contact-demo.gif)

或者你想為使用者提供對錶格資料的增刪改查功能，你可以使用 `FirestoreDataTable`:

```dart
class FirestoreTableStory extends StatelessWidget {
  FirestoreTableStory({Key? key}) : super(key: key);

  // live Firestore query
  final usersCollection = FirebaseFirestore.instance.collection('users');

  @override
  Widget build(BuildContext context) {
    return FirestoreDataTable(
      query: usersCollection,
      columnLabels: const {
        'firstName': Text('First name'),
        'lastName': Text('Last name'),
        'prefix': Text('Prefix'),
        'userName': Text('User name'),
        'email': Text('Email'),
        'number': Text('Phone number'),
        'streetName': Text('Street name'),
        'city': Text('City'),
        'zipCode': Text('Zip code'),
        'country': Text('Country'),
      },
    );
  }
}
```

效果是這樣的:

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/firebase-table-crud-demo.gif)

有關身份驗證、列表檢視和資料表的更多資訊，[請查閱 flutterfire_ui 文件](https://firebase.flutter.dev/docs/ui/overview/)。這個 package 仍處於預覽狀態，可能會加入更多新的特性，如果你有任何使用的問題或者新的特性需求，請 [在 GitHub repo 裡參與我們的討論](https://github.com/FirebaseExtended/flutterfire/discussions/6978)。

### Firestore Object/Document 對映 (ODM)

我們同時釋出了 [Firestore 物件 / 文件對映 (ODM)](https://firebase.flutter.dev/docs/firestore-odm/overview/) 的 Alpha 版本，Firestore ODM 的目標是讓開發者更高效的透過型別安全、結構化物件和方法來簡化 Firestore 的使用。透過產生程式碼，你可以以型別安全的方式對資料進行建模，從而改進與文件和集合互動的語法：

```dart
@JsonSerializable()
class Person {
  Person({required this.name, required this.age});

  final String name;
  final int age;
}

@Collection<Person>(‘/persons’)
final personsRef = PersonCollectionReference();
```

有了這些型別，你可以執行型別安全的查詢:

```dart
personsRef.whereName(isEqualTo: 'Bob');
personsRef.whereAge(isGreaterThan: 42);
```

ODM 還支援強型別子集合，也提供了一些內建、最佳化過的 widget 來重建其 `select` 功能，你可以在 [Firestore ODM 文件](https://firebase.flutter.dev/docs/firestore-odm/overview/) 中閱讀相關內容。因為這個還是 Alpha 版本，請儘可能 [在 GitHub repo 裡向我們提出反饋](https://github.com/FirebaseExtended/flutterfire/discussions/7475)。

## 桌面平台更新

Flutter 2.8 版本在 Windows、macOS 和 Linux 穩定版本的道路上又邁出了一大步。
我們的目標品質標準很高，包括國際化和本地化支援，例如 [新的中文輸入法支援](https://github.com/flutter/engine/pull/29620)、[韓語輸入法支援](https://github.com/flutter/engine/pull/24713) 以及剛剛合併的 [Kanji（日文）輸入法](https://github.com/flutter/engine/pull/29761) 支援。或者，就像我們在緊密建構 [Windows 輔助功能的支援](https://github.com/flutter/flutter/issues/77838) 一樣。
對於 Flutter 來說，在穩定版渠道的桌面端上“執行”是不夠的，它必須在世界各地的語言和文化以及不同能力的裝置上執行良好。我們還沒有達到我們想要的目標，但未來可期！

其中一個例子是我們重構了 Flutter 處理鍵盤事件以允許同步響應的架構。這使 widget 能夠處理按鍵並攔截它在整個 widget tree 中的其餘部分中的傳遞。我們在 Flutter 2.5 中完成了這項工作的落地，並在 Flutter 2.8 中修復了許多問題。這是對我們如何處理特定於裝置的鍵盤輸入的方式的重新設計，以及和重構 Flutter 處理文字編輯方式的持續工作的補充，所有這些都是用鍵盤這樣輸入密集型的桌面應用所必需的。

此外，我們還在繼續 [向 Flutter 擴充視覺密度的定義](https://github.com/flutter/flutter/pull/89353)，暴露對話方塊對齊方式的設定，以便開發者可以實現更加友好的桌面 UI。

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/flutter-desktop-dialog-demo.png)

最後，Flutter 團隊並不是唯一一個在為了 Flutter desktop 付出心血的團隊。舉個例子，Canonical 的桌面團隊正在與 Invertase 合作，在 Linux 和 Windows 上開發最流行的 Flutter Firebase 外掛。

你可以在 [Invertase 部落格上](https://invertase.io/blog/announcing-flutterfire-desktop) 閱讀有關預覽版的更多資訊。

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/invertase-flutterfire-desktop-preview.png)

## DartPad

如果沒有工具的改進，那麼這個 Flutter 新版本的釋出是不完整的。我們將重點介紹 DartPad 的改進，其中最大的改進是對更多軟體套件的支援。事實上，目前共有 23 個 package 可供匯入使用。除了幾個 Firebase 服務之外，該列表還包括諸如 `bloc`、`characters`、`collection`、`google_fonts` 和 `flutter_riverpod` 等流行的 package。
DartPad 團隊會繼續新增新的 package，如果你想檢視當前支援哪些 package 的話，可以單擊右下角的資訊圖示。

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/dartpad-support-packages-1211.png)

如果你想了解未來我們向 DartPad 新增新 package 的計劃，請檢視 [Dart wiki 上的這篇文章](https://github.com/dart-lang/dart-pad/wiki/Adding-support-for-a-new-package)。
還有另一個新的 DartPad 功能也非常方便，在此之前，DartPad 總是以執行最新的穩定版本執行。在新版本中，你可以使用狀態列中新的 **Channel 選單** 來切換到使用最新的 Beta 渠道版本以及先前穩定版本 (我們稱為 "old channel" 舊渠道)。

![](https://files.flutter-io.cn/posts/flutter-cn/2021/whats-new-in-flutter-2-8/dartpad-using-channel-switch.png)

DartPad 裡舊渠道的使用場景比如你正在撰寫一篇部落格文章，而最新的穩定版本還是特別流行，那這將非常有用。

## 移除 Dev 渠道

Flutter 的釋出「渠道」(也就是 channel) 決定了 Flutter 框架和引擎在你的開發機器上變化的速度，`stable` 代表最少的變更，而 `master` 代表最多。由於資源有限，我們決定最近將停止更新 `dev` 渠道。雖然我們確實收到了一些關於 `dev` 渠道的問題，但我們發現只有不到 3% 的 Flutter 開發人員使用 `dev` 渠道，因此，我們決定 **不久將正式停用 dev 渠道**。

因為雖然很少有開發人員使用 `dev` 渠道，但 Flutter 工程師仍需要花費大量時間和精力來維護它。
如果你基本都只使用 `stable` 渠道的 Flutter 版本 (超過 90% 的 Flutter 者都在這麼做)，那麼這項改動將不會影響你的日常開發。透過放棄維護這個渠道，開發者們也可以少做一個渠道選擇的決定，同時 Flutter 團隊也可以將時間和精力花在其他事情上。你可以使用 `flutter channel` 命令來決定你想要哪個渠道。以下是 Flutter 團隊對每個渠道的計劃: 

* **Stable 渠道**: 代表我們擁有的最高品質的建構。它們每季度（大致）釋出一次，並針對中間的關鍵問題進行熱修復。這是「慢」通道: 安全、成熟、長期服務；
* **Beta 渠道**: 為那些習慣於更快節奏的開發者提供一種快速調整的替代方案。目前每月釋出，穩定測試後會釋出。這是一個「快速」通道，如果我們發現 `dev` 渠道相較於 `beta` 渠道有特別的需求和需求而 `beta` 渠道無法滿足，我們可能會改變 `beta` 渠道的計劃來滿足 (比如，加速釋出節奏或降低我們對該渠道執行的測試和熱修復級別)；
* **Master 渠道**: 是我們活躍的開發渠道。我們不提供對該渠道的支援，但我們針對它運行了一套全面的單元測試。對於對不穩定的建構感到滿意的貢獻者或高階開發者而言，這是適合他們的渠道。在這個頻道上，我們跑得很快，打破了一些東西 (然後會很快地修復它們)。

當我們在未來幾個月停用 `dev` 渠道時，請考慮使用 `beta` 或 `master` 渠道，這取決於你對變更的容忍度以及對使用「最新」還是「最好」的平衡點。

## 破壞性改動 (breaking changes)

與往常一樣，我們努力減少每個版本中破壞性更改的數量。在此版本中，Flutter 2.8 除了已過期並根據我們的 [破壞性改動政策](https://github.com/flutter/flutter/wiki/Tree-hygiene#handling-breaking-changes) 被刪除的已棄用 API 之外，沒有重大變更。

* [90292](https://github.com/flutter/flutter/pull/90292) 移除已廢棄的 `autovalidate` 
* [90293](https://github.com/flutter/flutter/pull/90293) 移除已廢棄的 `FloatingHeaderSnapConfiguration.vsync`
* [90294](https://github.com/flutter/flutter/pull/90294) 移除已廢棄的 `AndroidViewController.id`
* [90295](https://github.com/flutter/flutter/pull/90295) 移除已廢棄的 `BottomNavigationBarItem.title`
* [90296](https://github.com/flutter/flutter/pull/90296) 移除已廢棄的文字輸入格式化類

如果你仍在使用這些 API 並想了解如何遷移程式碼，你可以閱讀 [Flutter 文件網站上的遷移指南](https://flutter.cn/docs/release/breaking-changes/2-5-deprecations)。
與往常一樣，非常感謝社群 [貢獻的測試使用案例](https://github.com/flutter/tests/blob/master/README.md)，幫助我們識別這些破壞性改動。

## 總結

在我們結束 2021 年並展望 2022 年之際，Flutter 團隊要對整個 Flutter 社群的工作和支援表示感謝。誠然，我們正在為世界上越來越多的開發人員建構 Flutter，但如果沒有你和每位開發者的存在，我們也無法維護並建構它。Flutter 社群與眾不同，感謝你所做的一切！