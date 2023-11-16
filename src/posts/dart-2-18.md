---
title: Dart 2.18 正式釋出
toc: true
keywords: Dart正式版, Dart2.18
description: 互操作性增強、平台特定的網路元件、最佳化型別推斷，以及空安全語言里程碑的近期更新。
image:
    path: https://files.flutter-io.cn/posts/flutter-cn/2022/dart-2-18/image3.png
    width: 2000
    height: 850
---
*文/ Michael Thomsen, Google Dart 團隊產品經理，2022 年 8 月 31 日發表於 Dart 官方部落格*

Dart 2.18 穩定版也隨著 [Flutter 3.3 穩定版]({{site.main-url}}/posts/whats-new-in-flutter-3-3) 一起釋出，本次更新帶來了 Dart 與 Objective-C \& Swift 互操作特性的預覽版，以及根據這個特性建構的 iOS / macOS 網路元件的 package。新的 Dart 還包括泛型方法的型別推斷最佳化、非同步程式碼的效能提升、pub.dev 新的功能，以及對我們工具和核心庫的一些調整。

文章最後我們也給出了最新的空安全遷移狀態情況資料，以及最終完全實現 Dart 空安全特性路線圖的一個重要更新，請務必讀到最後。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/dart-2-18/image3.png)

## Dart 與 Objective-C 和 Swift 互調

早在 2020 年的時候，我們釋出了外部功能介面 (FFI) 用於呼叫原生 C 語言介面的預覽，並在 2021 年的 Dart 2.12 中正式釋出。自那時起，大量的 package 藉助於 FFI 的優勢與現有的原生 C 語言介面 API 整合，舉一些例子，比如 `file_picker`、`printing`、`win32`、`objectbox`、`realm`、`isar`、`tflite_flutter` 以及 `dbus` 這些 package。

Dart 團隊希望主流程式語言之間的互操作能夠在所有 Dart 可以執行的平臺上都支援，2.18 正式版達到了這個目標的下一個里程碑，現在，Dart 程式碼可以直接呼叫 Objective-C 和 Swift 程式碼了，主要用於在 macOS 和 iOS 平台呼叫 API。Dart 支援“全端呼叫”——從後端的命令列程式碼，再到前端的 Flutter 介面，你可以在任何應用中使用這種互操作機制。

這種全新機制源自於 Objective-C 和 Swift 程式碼可以透過 API 繫結機制用 C 語言程式碼來呼叫。Dart 的 [ffigen](https://pub.flutter-io.cn/packages/ffigen "Dart FFI 工具: ffigen 的 pub.dev 頁面") 工具可以透過 API 標頭檔案來建立這些繫結，接下來看一個例子。

## 在 Objective-C 中操作時區的例子

macOS 上有一個查詢時區資訊的 API，可以透過 [NSTimeZone](https://developer.apple.com/documentation/foundation/nstimezone?language%3Dobjc%5D&language=objc "NSTimeZone 類別的 API 頁面") 類來呼叫，開發者們可以透過這個 API 來查詢使用者為裝置設定的時區和 UTC 時區偏移。

下面的範例 Objective-C 應用就呼叫了這個時區 API 來獲得系統時區設定和 GMT 偏移。

```
#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        NSTimeZone *timezone = [NSTimeZone systemTimeZone]; // Get current time zone.
        NSLog(@"Timezone name: %@", timezone.name);
        NSLog(@"Timezone offset GMT: %ld hours", timezone.secondsFromGMT/60/60);
    }
    return 0;
}
```

這個範例應用首先匯入了 `Foundation.h` 標頭檔案，它包含了 Apple 的基礎庫的 API 標頭檔案。在接下來的方法體中，它呼叫了 NSTimeZone 的 `systemTimeZone` 方法，這個方法會返回一個例項化之後的 `NSTimeZone` 幷包含了裝置所設定的時區資訊。

最後，這個應用會向控制檯輸出兩行內容，包含時區名稱和 UTC 的小時偏移量:

```terminal
Timezone name: Europe/Copenhagen
Timezone offset GMT: 2 hours
```

## 在 Dart 中操作時區的例子

讓我們用 Dart 和 Objective-C 的互操作來重複一遍剛剛的實現。

首先透過 Dart 命令列建立一個應用:

`$ dart create timezones`

接著，在你的 `pubspec` 檔案里加入 `ffigen` 的配置引數，這些配置會在 headers 裡設定標頭檔案路徑，並且羅列出要產生的包裝類 (wrapper) 的 Objective-C 介面:

```yaml
ffigen:
  name: TimeZoneLibrary
  language: objc
  output: "foundation_bindings.dart"
  exclude-all-by-default: true
  objc-interfaces:
    include:
      - "NSTimeZone"
  headers:
    entry-points:
      - "/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks/Foundation.framework/
         Headers/NSTimeZone.h"
```

這會為 `NSTimeZone.h` 這個標頭檔案配置 Objective-C 繫結，並且僅包含 `NSTimeZone` 介面中的 API，然後執行下面程式碼產生器包裝類:

`$ dart run ffigen`

這個命令會建立一個包含了各種 API 繫結的新 dart 檔案 `foundation_bindings.dart`，呼叫這個檔案之後，我們就可以來寫 Dart 主方法 (`main`) 了，這個方法「鏡像」了 Objective-C 的程式碼，如下:

```dart
void main(List<String> args) async {
  const dylibPath =
      '/System/Library/Frameworks/Foundation.framework/Versions/Current/Foundation';
  final lib = TimeZoneLibrary(DynamicLibrary.open(dylibPath));

  final timeZone = NSTimeZone.getLocalTimeZone(lib);
  if (timeZone != null) {
    print('Timezone name: ${timeZone.name}');
    print('Offset from GMT: ${timeZone.secondsFromGMT / 60 / 60} hours');
  }
}
```

這樣就可以啦，這個新特性從 Dart 2.18 開始以實驗性的支援開始提供，它增強了 Dart 的基礎互操作特性，可以直接在 Dart 程式碼裡或者透過 Flutter 外掛來呼叫 macOS 和 iOS API 了。

我們非常歡迎開發者們的反饋，你可以透過我們的 GitHub Issue [提出反饋建議](https://github.com/dart-lang/sdk/issues/49673 "針對 Dart 和 Objective-C 和 Swift 的互操作提出反饋建議")，讓我們知道哪些已然做的很好了、哪些地方尚有待改進，以及任何你遇到的問題。瞭解互操作性的更多資訊，可以參閱 Dart 文件: [使用 package:ffigen 來進行與 Objective-C 和 Swift 的互操作](https://dart.cn/guides/libraries/objective-c-interop "Dart 文件: 使用 package:ffigen 來進行與 Objective-C 和 Swift 的互操作")。


## 平台特定的 http 庫

Dart 自帶一個通用的、可適用於多個平台的 `http` 庫，使用這個庫進行網路請求可免於考慮各個平台的不同情況。但有些時候，開發者們可能會想在某個平台使用這個平台的網路請求 API 來進行建構。

比如，Apple 的網路請求庫 [NSURLSession](https://developer.apple.com/documentation/foundation/nsurlsession "NSURLSession 類別的 API 頁面") 可以限定僅在 Wi-Fi 下存取或需要 VPN 才能連線。為了支援這些使用案例，我們建立了一個新的網路請求的 package: `cupertino_http`，它基於上一節提到的新的 Objective-C 互操作，並從 Apple Foundation 庫中網路請求庫中「提取」了大量的 API。

## `cupertino_http` 範例

這個例子裡，Flutter 應用的 HTTP 客戶端在 macOS 和 iOS 上使用了 `cupertino_http`，在其他平臺中仍使用普通的 `dart:io` 庫:

```dart
late Client client;
if (Platform.isIOS || Platform.isMacOS) {
  final config = URLSessionConfiguration.ephemeralSessionConfiguration()
    ..allowsCellularAccess = false
    ..allowsExpensiveNetworkAccess = false;
  client = CupertinoClient.fromSessionConfiguration(config);
} else {
  client = Client(); // 使用基於 dart:io 的 HTTP 客戶端
}
```

像這樣的初始配置完成之後，應用就會在不同平臺上執行特定的網路請求，比如現在的 `get()` 請求類似於下面這樣:

```dart
final response = await get(
  Uri.https(
    'www.googleapis.com',
    '/books/v1/volumes',
    {'q': 'HTTP', 'maxResults': '40', 'printType': 'books'},
  ),
);
```

當無法使用通用的介面時，你可以透過 `cupertino_http` 來呼叫 Apple 的網路請求 API:

```dart
final session = URLSession.sessionWithConfiguration(
    URLSessionConfiguration.backgroundSession('com.example.bgdownload'),
    onFinishedDownloading: (s, t, fileUri) {
      actualContent = File.fromUri(fileUri).readAsStringSync();
    });

final task = session.downloadTaskWithRequest(
    URLRequest.fromUrl(Uri.https(...))
    ..resume();
```

## 多平臺應用中使用特定平台的網路

我們的設計目標仍舊是儘可能保持應用的多平臺通用性，因此我們為 `http` API 保留了多平臺通用的基礎網路請求的等操作，並且可以透過配置檔案在不同平台配置網路請求庫。開發者們可以使用 `package:http` 的 Client API 來減少編寫平台特定的程式碼，它可以按照平台進行配置並以獨立於平台的方式使用。

Dart 2.18 對 `package:http` [Client API](https://pub.flutter-io.cn/documentation/http/latest/http/Client-class.html "Client API 文件頁面") 提供了特定平台 http 庫的實驗性支援:

* 在 macOS / iOS 使用基於 [NSURLSession](https://developer.apple.com/documentation/foundation/nsurlsession "NSURLSession 類別的 API 頁面")
* 在 Android 上使用基於 [Cronet](https://developer.android.google.cn/guide/topics/connectivity/cronet "Android 開發者文件: 使用 Cronet 執行網路操作")，Cronet 是一個在 Android 上非常流行的網路請求庫

將一個通用的 Client API 與幾個不同的網路請求實現結合在一起可以讓你獲得兩方面的好處，既可以使用平台特定的行為，同時也仍然在維護同一組共享的網路請求資源。我們希望 [在 GitHub 上收到大家的反饋](https://github.com/dart-lang/http/issues/764 "針對「在多平臺應用中使用特定平台的網路」的反饋連線")。

## 增強型別推斷

Dart 使用了許多通用方法，試想這個可以將集合元素轉換為一個單一值的 [fold](https://api.dart.cn/stable/2.17.6/dart-core/Iterable/fold.html "Dart fold 類別的 API 頁面") 方法。下面是一個對集合中的數字進行求和的例子：

```dart
List<int> numbers = [1, 2, 3];
final sum = numbers.fold(0, (x, y) => x + y);
print(‘The sum of $numbers is $sum’);
```

在 Dart 2.17 之前這個方法會返回一個類別型錯誤：

```
line 2 • The operator ‘+’ can’t be unconditionally invoked because the receiver can be ‘null’.
```

Dart 無法結合多個引數之間的資訊進行型別推斷。這導致了 `x` 型別具有不確定性。要糾正這個潛在的錯誤，你需要指定型別：

`final sum = numbers.fold(0, (int x, int y) => x + y)`

Dart 2.18 增強了型別推斷。在前面範例中，Dart 將會進行靜態分析，並推斷出 x 和 y 都是非空的整型。這個改動能夠讓你在保留強型別推斷帶來的穩健性的同時編寫出更加簡潔的 Dart 程式碼。

## 非同步函式效能增強

這個版本的 Dart 優化了 Dart VM 執行 `async` 以及 `async*`/`sync*` 的方式。這會縮減程式碼體積：在 Google 的兩個大型應用上，我們看到 AOT snapshot 產物大小減少大約了 10% 左右。同時在我們的微基準測試上也反映出了效能的提升。

VM 中還包含了一些額外的小的行為變更，瞭解更多請檢視 [發行註記](https://github.com/dart-lang/sdk/blob/master/CHANGELOG.md%23dart-vm "Dart 程式語言發行註記")。

## Pub.dev 網站的改進

結合 2.18 版本釋出的改動，我們在 `pub.dev` 這個 package 生態網站上也帶來了兩個新的改動。

通常情況下，個人的 package 開發者會使用業餘時間維護併發布新的 package，這可能會耗費他們大量的時間和資源。為方便其他使用者進行贊助，我們在 `pubspec` 中支援了全新的 `funding` 標籤，package 開發者可以用它列出一個或多個贊助其持續開發的連結，這些連結會展示在 `pub.dev` 網站的側欄中。

![](https://files.flutter-io.cn/posts/flutter-cn/2022/dart-2-18/image2.png)

瞭解更多請存取 [pubspec 文件](https://dart.cn/tools/pub/pubspec%23funding "pubspec 規範文件中關於 funding 標籤的部分")。

此外，我們也希望促進開源 package 的豐富生態，為了突出這一點，`pub.dev` 上的自動評分系統會為使用了 [OSI 批准的許可證](https://opensource.org/licenses "OSI 批准的許可證列表") 的 package 額外獎勵 10 分。

## 一些破壞性改動

Dart 特別注重簡單性和可學習性，因此在增加新功能時，我們也一直小心翼翼。保持簡單的一種做法是移除很少被使用或已經有更好的替代品的舊功能和 API。Dart 2.18 清理了這類條目，幷包含少量的破壞性改動：

* 我們在 2020 年 10 月添加了統一的 `dart` CLI 開發者工具。在 2.18 中，我們完成了此過渡。此版本移除了最後兩個已棄用的命令列工具: `dart2js` (更換為使用 `dart compile js`) 和 `dartanalyzer` (更換為使用 `dart analyze`)。
* 隨著語言版本控制的引入，`pub` 命令會產生一個新的解析檔案: `.dart_tool/package_config.json` (之前使用的 `.packages` 格式的檔案不能包含版本)，現在我們已經停止使用 `.packages` 檔案了，如果你有任何 `.packages` 檔案，你可以刪除它們。
* 非繼承自 `Object` 的類不能再作為 Mixin 被使用 (破壞性改動 [#48167](https://github.com/dart-lang/sdk/issues/48167 "破壞性改動 #48167"))，這種行為從未有意提倡。
* `dart:io` 中 `RedirectException` 的 `uri` 屬性已更改為可為空 (nullable) (破壞性改動 [#49045](https://github.com/dart-lang/sdk/issues/49045 "破壞性改動 #49045"))。
* `dart:io` 網路請求 API 中遵循 `SCREAMING_SNAKE` 約定的常量已被移除 (破壞性改動 [#34218](https://github.com/dart-lang/sdk/issues/34218 "破壞性改動 #34218"))，請改用相應的 lowerCamelCase 常量。
* Dart VM 在退出時不再恢復初始終端設定，更改標準輸入設定 `lineMode` 和 `echoMode` 的程式現在負責在程式退出時恢復設定 (破壞性改動 [#45630](https://github.com/dart-lang/sdk/issues/45630 "破壞性改動 #45630"))。

## 空安全更新

空安全自 2020 年 11 月 Beta 版釋出、2021 年 3 月隨著 [Dart 2.12]({{site.main-url}}/posts/announcing-dart-2-12) 正式推出以來，我們很高興看到空安全已被廣泛使用。

首先，`pub.dev` 上大部分流行 package 的開發者都已遷移到了空安全。我們的分析表明，最常用的 package 前 250 已全部支援空安全，前 1,000 中也有 98% 已經支援空安全。

其次，大部分開發者已經在具有完全空安全性的程式碼庫中開發。這一點至關重要，因為在將所有程式碼和所有依賴項 (包括傳遞性) 遷移之前，[Dart 健全的空安全性](https://dart.cn/null-safety/understanding-null-safety "Dart 健全的空安全性") 並不會發揮作用，我們正在透過 `flutter run` 命令的遙測來追蹤這一點。

下圖展示了 `flutter run` 命令執行中非健全 (Unsound) 和健全 (Sound) 的空安全的對比情況。在引入空安全之前，兩者都為零。隨後非健全的空安全快速增長，此時應用開始逐漸遷移到空安全，開發者先進行了部分遷移，但有些部分仍然需要遷移。一段時間過後，我們可以看到健全的空安全曲線穩定增長，到上月底，與非健全的空安全相比，健全的空安全執行量多出了四倍。我們希望在接下來的幾個季度中，我們將看到健全空安全到達 100%！

![](https://files.flutter-io.cn/posts/flutter-cn/2022/dart-2-18/image1.png)

## 重要的空安全路線圖更新

同時支援非健全和健全的空安全性不可避免地會增加開銷和複雜性。

首先，Dart 開發者需要學習和理解這兩種模式。每當閱讀一段 Dart 程式碼時，都需要 [檢查語言版本](https://dart.cn/guides/language/evolution%23language-versioning "Dart 文件: 語言版本控制")。

其次，在編譯器和執行時同時支援這兩種模式也會減慢 Dart SDK 支援新功能的發展。

>基於非健全空安全的開銷和上一節中提到的非常可觀的統計資料，我們的目標是過渡到僅支援健全的空安全，並停止支援非空安全和非健全的空安全模式，我們暫時將其定於 2023 年年中釋出。

這將意味著停止對 Dart 2.11 及更早版本的支援。具有 SDK 約束且下限小於 2.12 的 `pubspec` 將不再在 Dart 3 及更高版本中解析。在包含語言標記的原始碼中，如果設定為小於 2.12 (例如 `//@dart=2.9`) 則會失敗。

如果你已遷移到健全的空安全，你的程式碼將在 Dart 3 中以完全的空安全工作。如果你還沒有，我們的建議是請立即著手開始遷移。瞭解有關這些更改的更多資訊，請參閱 [這個議題](https://github.com/dart-lang/sdk/issues/49530 "Dart 程式語言議題 #49530: Dart 3 開始將停止支援非空安全")。

## 總結

與 Objective-C 和 Swift 等互操作、網路請求庫、Dart 程式語言的型別推斷以及 pub.dev 的更新等已經正式可用。開始體驗，請下載最新的 Dart 2.18 正式版，或者直接在 Flutter 3.3 中體驗，也可以直接在 DartPad 中體驗 Dart 程式語言。

最後就是空安全的遷移，請即刻著手遷移，與我們共同建構和體驗擁有健全空安全特性的 Dart 程式語言！

> **原文連結**: [Dart 官方部落格](https://medium.com/dartlang/dart-2-18-f4b3101f146c)
>
> **本地化**: CFUG 團隊: @chenglu、@Vadaski、@MeandNi、@Realank
>
> **中文連結**: [flutter.cn/posts/dart-2-18](https://flutter.cn/posts/dart-2-18)