---
title: Internationalizing Flutter apps
title: Flutter 應用裡的國際化
short-title: i18n
description: How to internationalize your Flutter app.
tags: Flutter開發
keywords: 國際化
---

<?code-excerpt path-base="internationalization"?>

{{site.alert.secondary}}
 
  <h4 class="no_toc">What you’ll learn</h4>
  
  <h4 class="no_toc">你將學習到</h4>

  * How to track the device's locale (the user's preferred language).
  
    如何去獲取裝置的語言環境（使用者首選的語言）。
    
  * How to manage locale-specific app values.
  
    如何去管理特定語言環境下的 app 值。
  
  * How to define the locales an app supports.
  
    如何去定義 app 支援的語言環境。
  
{{site.alert.end}}

  If your app might be deployed to users who speak another language then
  you'll need to internationalize it. That means you need to write
  the app in a way that makes it possible to localize values like text
  and layouts for each language or locale that the app
  supports. Flutter provides widgets and classes that help with
  internationalization and the Flutter libraries themselves are
  internationalized.
  
  如果你的 app 會部署給說其他語言的使用者使用，那麼你就需要對它進行國際化。
  這就意味著你在編寫 app 的時候，需要採用一種容易對它進行本地化的方式進行開發，
  這種方式讓你能夠為每一種語言或者 app 所支援的語言環境下的文字和佈局等進行本地化。
  Flutter 提供了 widgets 和類來幫助開發者進行國際化，
  當然 Flutter 庫本身就是國際化的。
  
  This page covers concepts and workflows necessary to localize a
  Flutter application using the `MaterialApp` and `CupertinoApp`
  classes, as most apps are written that way. However, applications
  written using the lower level `WidgetsApp` class can also
  be internationalized using the same classes and logic.
  
  由於大多數應用程式都是以這種方式編寫的，
  因此該頁面主要介紹了使用 `MaterialApp` 和 `CupertinoApp` 
  對 Flutter 應用程式進行本地化所需的概念和工作流程。 
  但是，使用較低級別的 `WidgetsApp` 類編寫的應用程式
  也可以使用相同的類和邏輯進行國際化。

{{site.alert.secondary}}

  <h4 class="no_toc">Sample internationalized apps</h4>
  
  <h4 class="no_toc">國際化的 app 範例</h4>

  If you'd like to start out by reading the code for an internationalized
  Flutter app, here are two small examples. The first one is intended to
  be as simple as possible, and the second one uses the APIs and tools
  provided by the [`intl`][] package.
  If Dart's intl package is new to you,
  see [Using the Dart intl tools][].
  
  如果你想透過閱讀已經國際化的 Flutter app 程式碼來開始的話，
  這裡有兩個小例子。第一個例子是一個儘可能簡單的實現。
  第二個例子使用了 [`intl`][] package 提供的 API 和工具。
  如果你還不熟悉 Dart 的 intl 套件，請檢視
  [使用 Dart intl 工具][Using the Dart intl tools]。

  * [Minimal internationalization][]
    
    [最簡單的國際化實現範例][Minimal internationalization]
    
  * [Internationalization based on the `intl` package][]
    
    [基於 `intl` package 的國際化實現範例][Internationalization based on the `intl` package]
  
{{site.alert.end}}

## Introduction to localizations in Flutter

## Flutter 應用本地化介紹

This section provides a tutorial on how to internationalize
a Flutter application, along with any additional setup that a
target platform might require.

本節主要介紹如何對 Flutter 應用進行國際化，以及針對目標平台需要設定的其他內容。

### Setting up an internation&shy;alized app: the Flutter<wbr>_localizations package {#setting-up}

### 配置一個國際化的 app：flutter_localizations package {#setting-up}

By default, Flutter only provides US English localizations.
To add support for other languages,
an application must specify additional `MaterialApp` (or `CupertinoApp`)
properties, and include a package called
`flutter_localizations`. As of November 2020,
this package supports 78 languages.

預設情況下，Flutter 只提供美式英語的本地化。
如果想要新增其他語言，你的應用必須指定額外的
`MaterialApp` 或者 `CupertinoApp` 屬性並且
新增一個名為 `flutter_localizations` 的 package。
截至到 2020 年 11 月份，這個 package 已經支援大約 78 種語言。

To use flutter_localizations,
add the package as a dependency to your `pubspec.yaml` file:

想要使用 flutter_localizations 的話，
你需要在 `pubspec.yaml` 檔案中新增它作為依賴：

<?code-excerpt "gen_l10n_example/pubspec.yaml (FlutterLocalizations)"?>
```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_localizations: # Add this line
    sdk: flutter         # Add this line
```

Next, run `pub get packages`, then import the `flutter_localizations` library and specify
`localizationsDelegates` and `supportedLocales` for `MaterialApp`:

下一步，先執行 `pub get packages`，然後引入 flutter_localizations 庫，
然後為 MaterialApp 指定 `localizationsDelegates` 和 `supportedLocales`：

<?code-excerpt "gen_l10n_example/lib/main.dart (LocalizationDelegatesImport)"?>
```dart
import 'package:flutter_localizations/flutter_localizations.dart';
```

<?code-excerpt "gen_l10n_example/lib/main.dart (MaterialApp)" remove="AppLocalizations.delegate"?>
```dart
return const MaterialApp(
  title: 'Localizations Sample App',
  localizationsDelegates: [
    GlobalMaterialLocalizations.delegate,
    GlobalWidgetsLocalizations.delegate,
    GlobalCupertinoLocalizations.delegate,
  ],
  supportedLocales: [
    Locale('en', ''), // English, no country code
    Locale('es', ''), // Spanish, no country code
  ],
  home: MyHomePage(),
);
```

After introducing the `flutter_localizations` package
and adding the code above, the `Material` and `Cupertino`
packages should now be correctly localized in
one of the 78 supported locales. Widgets should be
adapted to the localized messages, along with
correct left-to-right and right-to-left layout.

引入 `flutter_localizations` package 並添加了上面的程式碼之後，
`Material` 和 `Cupertino` 包現在應該被正確地本地化為 78 個受支援的語言環境之一。
widget 應當與本地化資訊保持同步，並具有正確的從左到右或從右到左的佈局。 

Try switching the target platform's locale to
Spanish (`es`) and notice that the messages should
be localized.

你可以嘗試將目標平台的語言環境切換為西班牙語（`es`），
然後應該可以發現資訊已經被本地化了。

Apps based on `WidgetsApp` are similar except that the
`GlobalMaterialLocalizations.delegate` isn't needed.

基於 `WidgetsApp` 建構的 app 在新增語言環境時，
除了 `GlobalMaterialLocalizations.delegate` 不需要之外，
其他的操作是類似的。

The full `Locale.fromSubtags` constructor is preferred
as it supports [`scriptCode`][], though the `Locale` default
constructor is still fully valid.

雖然 `語言環境 (Locale)` 預設的建構函式是完全沒有問題的，
但是還是建議大家使用 `Locale.fromSubtags` 的建構函式，
因為它支援設定 [文字程式碼][`scriptCode`]。

[`scriptCode`]: {{site.api}}/flutter/package-intl_locale/Locale/scriptCode.html

The elements of the `localizationsDelegates` list are factories that produce
collections of localized values. `GlobalMaterialLocalizations.delegate`
provides localized strings and other values for the Material Components
library. `GlobalWidgetsLocalizations.delegate` defines the default
text direction, either left-to-right or right-to-left, for the widgets
library.

`localizationDelegates` 陣列是用於產生本地化值集合的工廠。
`GlobalMaterialLocalizations.delegate` 為 Material 元件庫
提供本地化的字串和一些其他的值。
`GlobalWidgetsLocalizations.delegate` 為 widgets 庫
定義了預設的文字排列方向，由左到右或者由右到左。

More information about these app properties, the types they
depend on, and how internationalized Flutter apps are typically
structured, can be found below.

想知道更多關於這些 app 屬性，
它們依賴的型別以及那些國際化的 Flutter app 通常是如何組織的，
可以繼續閱讀下面內容。

<a name="adding-localized-messages"></a>
### Adding your own localized messages

### 新增您自己的本地化資訊

Once the `flutter_localizations` package is added, use the
following instructions to add localized text to your application.

引入 `flutter_localizations` package 後，
請按照以下說明將本地化的文字新增到您的應用程式。

1. Add the `intl` package to the `pubspec.yaml` file:

   將 `intl` package 新增到 `pubspec.yaml` 檔案中：

{% comment %}
RegEx removes "# Add this line" from lines "flutter_localizations:" and "sdk: flutter"
{% endcomment %}
   <?code-excerpt "gen_l10n_example/pubspec.yaml (Intl)" replace="/(?<!0) # Add this line//g" ?>
   ```yaml
   dependencies:
     flutter:
       sdk: flutter
     flutter_localizations:
       sdk: flutter
     intl: ^0.17.0 # Add this line
   ```

2. Also, in the `pubspec.yaml` file, enable the `generate`
   flag. This is added to the section of the pubspec that is
   specific to Flutter, and usually comes later in the pubspec
   file.

   另外，在 `pubspec.yaml` 檔案中，啟用 `generate` 標誌。
   該設定項新增在 pubspec 中 Flutter 部分，
   通常處在 pubspec 檔案中後面的部分。

   <?code-excerpt "gen_l10n_example/pubspec.yaml (Generate)"?>
   ```yaml
   # The following section is specific to Flutter.
   flutter:
     generate: true # Add this line
   ```

3. Add a new yaml file to the root directory of the Flutter
   project called `l10n.yaml` with the following content:
   
   在 Flutter 專案的根目錄中新增一個新的 yaml 檔案，
   命名為 `l10n.yaml`，其內容如下：

   <?code-excerpt "gen_l10n_example/l10n.yaml"?>
   ```yaml
   arb-dir: lib/l10n
   template-arb-file: app_en.arb
   output-localization-file: app_localizations.dart
   ```

   This file configures the localization tool; in this example,
   the input files are located in `${FLUTTER_PROJECT}/lib/l10n`,
   the `app_en.arb` file provides the template, and the generated
   localizations are placed in the `app_localizations.dart` file.

   該檔案用於配置本地化工具；在上面的範例中，指定輸入檔案在 
   `${FLUTTER_PROJECT}/lib/l10n` 中，`app_en.arb` 檔案提供範本，
   產生的本地化檔案在 `app_localizations.dart` 檔案中。

4. In `${FLUTTER_PROJECT}/lib/l10n`,
   add the `app_en.arb` template file. For example:
   
   在 `${FLUTTER_PROJECT}/lib/l10n` 中，新增 `app_en.arb` 範本檔案。如下：

   <?code-excerpt "gen_l10n_example/lib/l10n/app_en.arb"?>
   ```json
   {
       "helloWorld": "Hello World!",
       "@helloWorld": {
         "description": "The conventional newborn programmer greeting"
       }
   }
   ```

5. Next, add an `app_es.arb` file in the same directory for
   Spanish translation of the same message:
   
   接下來，在同一目錄中新增一個 `app_es.arb` 檔案，
   對同一條資訊做西班牙語的翻譯：

   <?code-excerpt "gen_l10n_example/lib/l10n/app_es.arb"?>
   ```json
   {
       "helloWorld": "¡Hola Mundo!"
   }
   ```

6. Now, run `flutter gen-l10n` so that codegen takes place. You should see generated files in
   `${FLUTTER_PROJECT}/.dart_tool/flutter_gen/gen_l10n`.
   
   執行 `flutter gen-l10n` 命令，
   您將在 `${FLUTTER_PROJECT}/.dart_tool/flutter_gen/gen_l10n` 中看到產生的檔案。

7. Add the import statement on `app_localizations.dart` and `AppLocalizations.delegate`
   in your call to the constructor for `MaterialApp`.

   在呼叫 `MaterialApp` 的建構函式時候，新增 `import` 陳述式，匯入
   `app_localizations.dart` 和 `AppLocalizations.delegate`。
   
   <?code-excerpt "gen_l10n_example/lib/main.dart (AppLocalizationsImport)"?>
   ```dart
   import 'package:flutter_gen/gen_l10n/app_localizations.dart';
   ```

   <?code-excerpt "gen_l10n_example/lib/main.dart (MaterialApp)"?>
   ```dart
   return const MaterialApp(
     title: 'Localizations Sample App',
     localizationsDelegates: [
       AppLocalizations.delegate, // Add this line
       GlobalMaterialLocalizations.delegate,
       GlobalWidgetsLocalizations.delegate,
       GlobalCupertinoLocalizations.delegate,
     ],
     supportedLocales: [
       Locale('en', ''), // English, no country code
       Locale('es', ''), // Spanish, no country code
     ],
     home: MyHomePage(),
   );
   ```

8. Use AppLocalizations anywhere in your app.
   Here, the translated message is used in a Text widget.

   在你應用的任何地方，都使用 `AppLocalizations`，
   這裡它被用於在 Text widget 裡展示翻譯過的訊息。

   <?code-excerpt "gen_l10n_example/lib/examples.dart (Example)"?>
   ```dart
   Text(AppLocalizations.of(context)!.helloWorld);
   ```

9. You can also use the generated `localizationsDelegates` and `supportedLocales` list
   instead of providing them manually.

   您也可以使用產生的 `localizationsDelegates` 和 `supportedLocales` 列表，
   而不是手動提供它們。

   <?code-excerpt "gen_l10n_example/lib/examples.dart (MaterialAppExample)"?>
   ```dart
   const MaterialApp(
     title: 'Localizations Sample App',
     localizationsDelegates: AppLocalizations.localizationsDelegates,
     supportedLocales: AppLocalizations.supportedLocales,
   );
   ```

   This code generates a Text widget that displays "Hello World!"
   if the target device's locale is set to English, and "¡Hola Mundo!"
   if the target device's locale is set to Spanish. In the `arb` files,
   the key of each entry is used as the method name of the getter,
   while the value of that entry contains the localized message.
   
   如果目標裝置的語言環境設定為英語，
   此程式碼產生器的 Text widget 會展示「Hello World!」。
   如果目標裝置的語言環境設定為西班牙語，則展示「Hola Mundo!」，
   在 `arb` 檔案中，每個條目的鍵值都被用作 getter 的方法名稱，
   而該條目的值則表示本地化的資訊。

To see a sample Flutter app using this tool, please see
[`gen_l10n_example`][].

要檢視使用該工具的範例 Flutter 應用，請參閱 [`gen_l10n_example`][]。

To localize your device app description, you can pass in the localized
string into [`MaterialApp.onGenerateTitle`][]:

如需本地化裝置應用描述，你可以將本地化後的字串傳遞給
[`MaterialApp.onGenerateTitle`][]:

<?code-excerpt "intl_example/lib/main.dart (MaterialAppTitleExample)"?>
```dart
return MaterialApp(
  onGenerateTitle: (context) =>
      DemoLocalizations.of(context).title,
```

For more information about the localization tool,
such as dealing with DateTime and handling plurals,
see the [Internationalization User's Guide][].

有關本地化工具的更多資訊，例如處理 DateTime 和複數，
請參見 [國際化使用者指南][Internationalization User's Guide]。

<a name="ios-specifics"></a>
### Localizing for iOS: Updating the iOS app bundle

### iOS 本地化：更新 iOS app bundle

iOS applications define key application metadata,
including supported locales, in an `Info.plist` file
that is built into the application bundle.
To configure the locales supported by your app,
use the following instructions:

iOS 應用在內置於應用程式套件中的 `Info.plist` 檔案中
定義了關鍵的應用程式元資料，其中包括了受支援的語言環境，
要配置您的應用支援的語言環境，請按照以下步驟進行操作：

1. Open your project's `ios/Runner.xcworkspace` Xcode file.

   開啟專案的 `ios/Runner.xcworkspace` Xcode 檔案。

2. In the **Project Navigator**, open the `Info.plist` file
   under the `Runner` project's `Runner` folder.
   
   在 **Project Navigator** 中，開啟 `Runner` 專案的 `Runner` 資料夾
   下的 `Info.plist` 檔案。

3. Select the **Information Property List** item.
   Then select **Add Item** from the **Editor** menu,
   and select **Localizations** from the pop-up menu.
   
   選擇 **Information Property List** 項。然後從 **Editor** 選單中
   選擇 **Add Item**，接著從彈出選單中選擇 **Localizations**。

4. Select and expand the newly-created `Localizations` item.
   For each locale your application supports,
   add a new item and select the locale you wish to add
   from the pop-up menu in the **Value** field.
   This list should be consistent with the languages listed
   in the [supportedLocales][] parameter.
   
   選擇並展開新建立的 `Localizations` 項。對於您的應用程式支援的每種語言環境，
   請新增一個新項，然後從 **Value** 欄位中的彈出選單中選擇要新增的語言環境。
   該列表應需要與 [supportedLocales][] 引數中列出的語言一致。

5. Once all supported locales have been added, save the file.

   新增所有受支援的語言環境後，儲存檔案。

<a name="advanced-customization">
## Advanced topics for further customization

## 客製的進階操作

This section covers additional ways to customize a
localized Flutter application.

本節介紹自訂本地 Flutter 應用程式的其他方法。

<a name="advanced-locale"></a>
### Advanced locale definition

### 高階語言環境定義

Some languages with multiple variants require more than just a
language code to properly differentiate.

一些具有著多個變體的語言僅用 `languageCode` 來區分是不夠充分的。

For example, fully differentiating all variants of
Chinese requires specifying the language code, script code,
and country code. This is due to the existence
of simplified and traditional script, as well as regional
differences in the way characters are written within the same script type.

例如，在多語言應用開發這個話題裡，如果要更好的區分具有多種語言變體的中文，
則需要指定其 `languageCode`、`scriptCode` 和 `countryCode`。
因為目前有兩種主要的，且存在地區使用差異的中文書寫系統：簡體和繁體。

In order to fully express every variant of Chinese for the
country codes `CN`, `TW`, and `HK`, the list of supported
locales should include:

為了讓 `CN`、`TW` 和 `HK` 能夠更充分地表示到每個中文變體，
建構應用時，設定支援的語言列表可以參考如下程式碼：

<?code-excerpt "gen_l10n_example/lib/examples.dart (SupportedLocales)"?>
```dart
supportedLocales: [
  Locale.fromSubtags(languageCode: 'zh'), // generic Chinese 'zh'
  Locale.fromSubtags(
      languageCode: 'zh',
      scriptCode: 'Hans'), // generic simplified Chinese 'zh_Hans'
  Locale.fromSubtags(
      languageCode: 'zh',
      scriptCode: 'Hant'), // generic traditional Chinese 'zh_Hant'
  Locale.fromSubtags(
      languageCode: 'zh',
      scriptCode: 'Hans',
      countryCode: 'CN'), // 'zh_Hans_CN'
  Locale.fromSubtags(
      languageCode: 'zh',
      scriptCode: 'Hant',
      countryCode: 'TW'), // 'zh_Hant_TW'
  Locale.fromSubtags(
      languageCode: 'zh',
      scriptCode: 'Hant',
      countryCode: 'HK'), // 'zh_Hant_HK'
],
```

This explicit full definition ensures that your app can
distinguish between and provide the fully nuanced localized
content to all combinations of these country codes.
If a user's preferred locale is not specified,
then the closest match is used instead,
which likely contains differences to what the user expects.
Flutter only resolves to locales defined in `supportedLocales`.
Flutter provides scriptCode-differentiated
localized content for commonly used languages.
See [`Localizations`][] for information on how the supported
locales and the preferred locales are resolved.

程式碼裡這幾組明確和完整的定義，
可以確保你的應用為各種不同首選語言環境的使用者提供更加精細化的本地化內容。
如果使用者沒有指定首選的語言環境，那麼我們就會使用最近的匹配，這可能與使用者的期望會有差異。
Flutter 只會解析定義在 `supportedLocales` 裡面的語言環境。
對於那些常用語言，Flutter 為本地化內容提供了文字程式碼級別的區分。
檢視 [`Localizations`][] 瞭解 Flutter 是如何
解析支援的語言環境和首選的語言環境的。

Although Chinese is a primary example,
other languages like French (`fr_FR`, `fr_CA`)
should also be fully differentiated for more nuanced localization.

雖然中文是最主要的一個範例，
但是其他語言如法語（`fr_FR`，`fr_CA` 等等）
也應該為了更細緻的本地化而做完全的區分。

{% comment %}
Issue: cfug#942
參考內容和拓展閱讀：
- [瞭解“漢字”](https://zxxy.nwnu.edu.cn/yywzgzw/newsshow-819-12687-1.html)
- [知網百科——語言變體](https://xuewen.cnki.net/R2006091330000010.html)
- [國家標準計劃《世界各國和地區及其行政區劃名稱程式碼 第1部分：國家和地區程式碼》](http://std.samr.gov.cn/gb/search/gbDetailed?id=5DDA8BA23A9018DEE05397BE0A0A95A7)
{% endcomment %}

<a name="tracking-locale"></a>

### Tracking the locale: The Locale class and the Localizations widget

### 獲取語言環境：Locale 類和 Localizations Widget

The [`Locale`][] class identifies the user's language.
Mobile devices support setting the locale for all applications,
usually using a system settings menu.
Internationalized apps respond by displaying values that are
locale-specific. For example, if the user switches the device's locale
from English to French, then a `Text` widget that originally
displayed "Hello World" would be rebuilt with "Bonjour le monde".

[`Locale`][] 類用來識別使用者的語言。
移動裝置支援為所有的應用設定語言環境，經常是透過系統設定選單來進行操作。
設定完之後，國際化的 app 就會展示成對應特定語言環境的值。
例如，如果使用者把裝置的語言環境從英語切換到法語，
顯示 "Hello World" 的文字 widget 會使用 "Bonjour le monde" 進行重建。

The [`Localizations`][widgets-global] widget defines the locale
for its child and the localized resources that the child depends on.
The [`WidgetsApp`][] widget creates a `Localizations` widget
and rebuilds it if the system's locale changes.

[`Localizations`][widgets-global]
widget 定義了它的子節點的語言環境和依賴的本地化的資源。
[`WidgetsApp`][] 建立了一個本地化的 widget，
如果系統的語言環境變化了，它會重建這個 widget。

You can always lookup an app's current locale with
`Localizations.localeOf()`:

你可以透過呼叫 `Localizations.localeOf()` 方法來檢視 app 當前的語言環境。 

<?code-excerpt "gen_l10n_example/lib/examples.dart (MyLocale)"?>
```dart
Locale myLocale = Localizations.localeOf(context);
```

<a name="specifying-supportedlocales"></a>
### Specifying the app's supported&shy;Locales parameter

Although the `flutter_localizations` library currently supports 78
languages and language variants, only English language translations
are available by default. It's up to the developer to decide exactly
which languages to support.

The `MaterialApp` [`supportedLocales`][]
parameter limits locale changes. When the user changes the locale
setting on their device, the app's `Localizations` widget only
follows suit if the new locale is a member of this list.
If an exact match for the device locale isn't found,
then the first supported locale with a matching [`languageCode`][]
is used. If that fails, then the first element of the
`supportedLocales` list is used.

An app that wants to use a different "locale resolution"
method can provide a [`localeResolutionCallback`][].
For example, to have your app unconditionally accept
whatever locale the user selects:

<?code-excerpt "gen_l10n_example/lib/examples.dart (LocaleResolution)"?>
```dart
MaterialApp(
  localeResolutionCallback: (
    locale,
    supportedLocales,
  ) {
    return locale;
  },
);
```

## How internationalization in Flutter works

## Flutter 裡的國際化是如何工作的

This section covers the technical details of how localizations work
in Flutter. If you're planning on supporting your own set of localized
messages, the following content would be helpful. Otherwise, you can
skip this section.

本節涵蓋了 Flutter 中本地化工作的技術細節，
如果你計劃使用自定的一套本地化訊息，下面的內容會很有幫助。
反之則可以跳過本節。

<a name="loading-and-retrieving"></a>
### Loading and retrieving localized values

### 載入和獲取本地化值

The `Localizations` widget is used to load and lookup objects that
contain collections of localized values. Apps refer to these objects
with [`Localizations.of(context,type)`][].
If the device's locale changes,
the `Localizations` widget automatically loads values for
the new locale and then rebuilds widgets that used it.
This happens because `Localizations` works like an
[`InheritedWidget`][].
When a build function refers to an inherited widget,
an implicit dependency on the inherited widget is created.
When an inherited widget changes
(when the `Localizations` widget's locale changes),
its dependent contexts are rebuilt.

我們使用 `Localizations` widget 來載入和查詢那些包含本地化值集合的物件。
app 透過呼叫 [`Localizations.of(context,type)`][] 來參考這些物件。
如果裝置的語言環境變化了，Localizations widget 會自動地載入新的語言環境的值，
然後重建那些使用了語言環境的 widget。
這是因為 `Localizations` 像 [繼承 widget][`InheritedWidget`] 一樣執行。
當一個建構過程涉及到繼承 widget，對繼承 widget 的隱含依賴就建立了。
當一個繼承 widget 變化了（即 Localizations widget 的語言環境變化），
它的依賴上下文就會被重建。

Localized values are loaded by the `Localizations` widget's
list of [`LocalizationsDelegate`][]s.
Each delegate must define an asynchronous [`load()`][]
method that produces an object that encapsulates a
collection of localized values.
Typically these objects define one method per localized value.

本地化的值是透過使用 `Localizations` widget 的 [`LocalizationsDelegate`] 載入的。
每一個 delegate 必須定義一個非同步的 [`load()`][] 方法。
這個方法生成了一個封裝本地化值的物件，
通常這些物件為每個本地化的值定義了一個方法。

In a large app, different modules or packages might be bundled with
their own localizations. That's why the `Localizations` widget
manages a table of objects, one per `LocalizationsDelegate`.
To retrieve the object produced by one of the `LocalizationsDelegate`'s
`load` methods, you specify a `BuildContext` and the object's type.

在一個大型的 app 中，
不同的模組或者 package 需要和它們對應的本地化資源打包在一起。
這就是為什麼 `Localizations` widget 管理著物件的一個對應表，
每個 `LocalizationsDelegate` 對應一個物件。
為了獲得由 `LocalizationsDelegate` 的 `load` 方法產生的物件，
你需要指定一個建構上下文 (`BuildContext`) 和物件的型別。

For example,
the localized strings for the Material Components widgets
are defined by the [`MaterialLocalizations`][] class.
Instances of this class are created by a `LocalizationDelegate`
provided by the [`MaterialApp`][] class.
They can be retrieved with `Localizations.of()`:

例如，Material 元件 widget 的本地化字串是由 [`MaterialLocalizations`][]
類定義的。這個類別的例項是由 [[`MaterialApp`][] 
類提供的一個 `LocalizationDelegate` 方法建立的，
它們可以透過 `Localizations.of` 方法獲得。

<!-- skip -->
```dart
Localizations.of<MaterialLocalizations>(context, MaterialLocalizations);
```

This particular `Localizations.of()` expression is used frequently,
so the `MaterialLocalizations` class provides a convenient shorthand:

因為這個特定的 `Localizations.of()` 表示式經常使用，
所以 `MaterialLocalizations` 類提供了一個快捷存取：

<!-- skip -->
```dart
static MaterialLocalizations of(BuildContext context) {
  return Localizations.of<MaterialLocalizations>(context, MaterialLocalizations);
}

/// References to the localized values defined by MaterialLocalizations
/// are typically written like this:

tooltip: MaterialLocalizations.of(context).backButtonTooltip,
```


<a name="defining-class"></a>
### Defining a class for the app's localized resources

### 為 app 的本地化資源定義一個類別

Putting together an internationalized Flutter app usually
starts with the class that encapsulates the app's localized values.
The example that follows is typical of such classes.

綜合所有這些在一起，一個需要國際化的 app 經常以一個封裝 app 本地化值的類開始的。
下面是使用這種類別的典型範例。

Complete source code for the [`intl_example`][] for this app.

此範例 app 的 [完整的原始碼][`intl_example`]。

This example is based on the APIs and tools provided by the
[`intl`][] package.
[An alternative class for the app's localized resources][]
describes [an example][] that doesn't depend on the `intl` package.

這個範例是基於 [`intl`][] package 提供的 API 和 工具開發的，
[app 本地化資源的替代方法][An alternative class for the app's localized resources]
裡面講解了一個不依賴於 intl package 的 [範例][an example]。

The `DemoLocalizations` class defined below
contains the app's strings (just one for the example)
translated into the locales that the app supports.
It uses the `initializeMessages()` function
generated by Dart's [`intl`][] package,
[`Intl.message()`][], to look them up.

`DemoLocalizations` 類包含了 app 語言環境內支援的
已經翻譯成了本地化語言的字串（本例子只有一個）。
它透過呼叫由 Dart 的 [`intl`][] package 產生的
`initializeMessages()` 方法來載入翻譯好的字串，
然後使用 [`Intl.message()`][] 來查閱它們。

<?code-excerpt "intl_example/lib/main.dart (DemoLocalizations)"?>
```dart
class DemoLocalizations {
  DemoLocalizations(this.localeName);

  static Future<DemoLocalizations> load(Locale locale) {
    final String name =
        locale.countryCode == null || locale.countryCode!.isEmpty
            ? locale.languageCode
            : locale.toString();
    final String localeName = Intl.canonicalizedLocale(name);

    return initializeMessages(localeName).then((_) {
      return DemoLocalizations(localeName);
    });
  }

  static DemoLocalizations of(BuildContext context) {
    return Localizations.of<DemoLocalizations>(context, DemoLocalizations)!;
  }

  final String localeName;

  String get title {
    return Intl.message(
      'Hello World',
      name: 'title',
      desc: 'Title for the Demo application',
      locale: localeName,
    );
  }
}
```

A class based on the `intl` package imports a generated
message catalog that provides the `initializeMessages()`
function and the per-locale backing store for `Intl.message()`.
The message catalog is produced by an [`intl` tool][]
that analyzes the source code for classes that contain
`Intl.message()` calls.  In this case that would just be the
`DemoLocalizations` class.

基於 `intl` package 的類引入了一個產生好的資訊目錄，
它提供了 `initializeMessage()` 方法和 `Intl.message()` 方法的
每個語言環境的備份儲存。
[`intl` 工具][`intl` tool] 透過分析包含 `Intl.message()` 
呼叫類別的原始碼產生這個資訊目錄。在當前情況下，
就是 DemoLocalizations 的類（包含了 `Intl.message()` 呼叫）。


<a name="adding-language"></a>
### Adding support for a new language

### 新增支援新的語言

An app that needs to support a language that's not included in
[`GlobalMaterialLocalizations`][] has to do some extra work:
it must provide about 70 translations ("localizations")
for words or phrases and the date patterns and symbols for the
locale.

如果你要開發一個 app 需要支援的語言不在 [`GlobalMaterialLocalizations`][] 當中，
那就需要做一些額外的工作：它必須提供大概 70 個字和詞還有日期以及符號的翻譯（本地化）。

See the following for an example of how to add
support for the Norwegian Nynorsk language.

舉個例子，我們將給大家展示如何支援挪威尼諾斯克語。

A new `GlobalMaterialLocalizations` subclass defines the
localizations that the Material library depends on.
A new `LocalizationsDelegate` subclass, which serves
as factory for the `GlobalMaterialLocalizations` subclass,
must also be defined.

我們需要定義一個新的 `GlobalMaterialLocalizations` 子類別，
它定義了 Material 庫依賴的本地化資源。
同時，我們也必須定義一個新的 `LocalizationsDelegate` 子類別，
它是給 `GlobalMaterialLocalizations` 子類別作為一個工廠使用的。

Here's the source code for the complete [`add_language`][] example,
minus the actual Nynorsk translations.

這是支援新增一種新語言的[一個完整例子的原始碼][`add_language`]，
相對實際上要翻譯的尼諾斯克語數量，我們只翻譯了一小部分。

The locale-specific `GlobalMaterialLocalizations` subclass
is called `NnMaterialLocalizations`,
and the `LocalizationsDelegate` subclass is
`_NnMaterialLocalizationsDelegate`.
The value of `NnMaterialLocalizations.delegate`
is an instance of the delegate, and is all
that's needed by an app that uses these localizations.

這個特定語言環境的 `GlobalMaterialLocalizations`
子類別被稱為 `NnMaterialLocalizations`，
`LocalizationsDelegate` 子類別被稱為 `_NnMaterialLocalizationsDelegate`。
`BeMaterialLocalizations.delegate` 是 delegate 的一個例項，
這就是 app 使用這些本地化所需要的全部。

The delegate class includes basic date and number format
localizations. All of the other localizations are defined by `String`
valued property getters in `NnMaterialLocalizations`, like this:

delegate 類包括基本的日期和數字格式的本地化。
其他所有的本地化是由 `BeMaterialLocalizations`
裡面的 `String` 字串值屬性的 getters 所定義的，
像下面這樣：

<?code-excerpt "add_language/lib/nn_intl.dart (Getters)"?>
```dart
@override
String get moreButtonTooltip => r'More';

@override
String get aboutListTileTitleRaw => r'About $applicationName';

@override
String get alertDialogLabel => r'Alert';
```

These are the English translations, of course.
To complete the job you need to change the return
value of each getter to an appropriate Nynorsk string.

當然，這些都是英語翻譯。為了完成本地化操作，
你需要把每一個 getter 的返回值翻譯成合適的
新挪威語 (Nynorsk) 字串。

The getters return "raw" Dart strings that have an r prefix,
like `r'About $applicationName'`,
because sometimes the strings contain variables with a `$` prefix.
The variables are expanded by parameterized localization methods:

像 `r'About $applicationName'` 一樣，
這些帶 r 字首的 getters 返回的是原始的字串，
因為有一些時候這些字串會包含一些帶有 `$` 字首的變數。
透過呼叫帶引數的本地化方法，這些變數會被替換：

<?code-excerpt "add_language/lib/nn_intl.dart (Raw)"?>
```dart
@override
String get pageRowsInfoTitleRaw => r'$firstRow–$lastRow of $rowCount';

@override
String get pageRowsInfoTitleApproximateRaw =>
    r'$firstRow–$lastRow of about $rowCount';
```

The date patterns and symbols of the locale will also need to
be specified. In the source code, the date patterns and symbols
are defined like this:

語言對應的日期格式和符號需要一併指定。
在原始碼中，它們會以下列形式進行定義：

{% comment %}
RegEx adds last two lines with commented out code and closing bracket.
{% endcomment %}
<?code-excerpt "add_language/lib/nn_intl.dart (Date)" replace="/  'LLL': 'LLL',/  'LLL': 'LLL',\n  \/\/ ...\n}/g"?>
```dart
const nnLocaleDatePatterns = {
  'd': 'd.',
  'E': 'ccc',
  'EEEE': 'cccc',
  'LLL': 'LLL',
  // ...
}
```

{% comment %}
RegEx adds last two lines with commented out code and closing bracket.
{% endcomment %}
<?code-excerpt "add_language/lib/nn_intl.dart (Date2)" replace="/  ],/  ],\n  \/\/ ...\n}/g"?>
```dart
const nnDateSymbols = {
  'NAME': 'nn',
  'ERAS': <dynamic>[
    'f.Kr.',
    'e.Kr.',
  ],
  // ...
}
```

These will need to be modified for the locale to use the correct
date formatting. Unfortunately, since the `intl` library does
not share the same flexibility for number formatting, the formatting
for an existing locale will have to be used as a substitute in
`_NnMaterialLocalizationsDelegate`:

上列內容需要修改以匹配語言的正確日期格式。
可惜的是，`intl` 並不具備數字格式的靈活性，
以至於 `_NnMaterialLocalizationsDelegate` 需要使用
現有的語言的格式作為替代方法：

<?code-excerpt "add_language/lib/nn_intl.dart (Delegate)"?>
```dart
class _NnMaterialLocalizationsDelegate
    extends LocalizationsDelegate<MaterialLocalizations> {
  const _NnMaterialLocalizationsDelegate();

  @override
  bool isSupported(Locale locale) => locale.languageCode == 'nn';

  @override
  Future<MaterialLocalizations> load(Locale locale) async {
    final String localeName = intl.Intl.canonicalizedLocale(locale.toString());

    // The locale (in this case `nn`) needs to be initialized into the custom
    // date symbols and patterns setup that Flutter uses.
    date_symbol_data_custom.initializeDateFormattingCustom(
      locale: localeName,
      patterns: nnLocaleDatePatterns,
      symbols: intl.DateSymbols.deserializeFromMap(nnDateSymbols),
    );

    return SynchronousFuture<MaterialLocalizations>(
      NnMaterialLocalizations(
        localeName: localeName,
        // The `intl` library's NumberFormat class is generated from CLDR data
        // (see https://github.com/dart-lang/intl/blob/master/lib/number_symbols_data.dart).
        // Unfortunately, there is no way to use a locale that isn't defined in
        // this map and the only way to work around this is to use a listed
        // locale's NumberFormat symbols. So, here we use the number formats
        // for 'en_US' instead.
        decimalFormat: intl.NumberFormat('#,##0.###', 'en_US'),
        twoDigitZeroPaddedFormat: intl.NumberFormat('00', 'en_US'),
        // DateFormat here will use the symbols and patterns provided in the
        // `date_symbol_data_custom.initializeDateFormattingCustom` call above.
        // However, an alternative is to simply use a supported locale's
        // DateFormat symbols, similar to NumberFormat above.
        fullYearFormat: intl.DateFormat('y', localeName),
        compactDateFormat: intl.DateFormat('yMd', localeName),
        shortDateFormat: intl.DateFormat('yMMMd', localeName),
        mediumDateFormat: intl.DateFormat('EEE, MMM d', localeName),
        longDateFormat: intl.DateFormat('EEEE, MMMM d, y', localeName),
        yearMonthFormat: intl.DateFormat('MMMM y', localeName),
        shortMonthDayFormat: intl.DateFormat('MMM d'),
      ),
    );
  }

  @override
  bool shouldReload(_NnMaterialLocalizationsDelegate old) => false;
}
```

For more information about localization strings, see the
[flutter_localizations README][].

需要了解更多關於本地化字串的內容，可以檢視 [flutter_localizations README][]。

Once you've implemented your language-specific subclasses of
`GlobalMaterialLocalizations` and `LocalizationsDelegate`,
you just need to add the language and a delegate instance to your app.
Here's some code that sets the app's language to Nynorsk and
adds the `NnMaterialLocalizations` delegate instance to the app's
`localizationsDelegates` list:

一旦你實現了指定語言的
`GlobalMaterialLocalizations` 和 `LocalizationsDelegate` 的子類別，
你只需要給你的 app 新增此語言以及一個 delegate 的例項。
這裡有一些程式碼展示瞭如何設定 app 的語言為尼諾斯克語以及如何給 app 的
`localizationsDelegates` 列表新增 `NnMaterialLocalizations` delegate 例項。

<?code-excerpt "add_language/lib/main.dart (MaterialApp)"?>
```dart
const MaterialApp(
  localizationsDelegates: [
    GlobalWidgetsLocalizations.delegate,
    GlobalMaterialLocalizations.delegate,
    NnMaterialLocalizations.delegate, // Add the newly created delegate
  ],
  supportedLocales: [
    Locale('en', 'US'),
    Locale('nn'),
  ],
  home: Home(),
),
```

<a name="alternative-internationalization-workflows">
## Alternative internationalization workflows
  
## 其他的國際化方法

This section describes different approaches to internationalize
your Flutter application.

本節主要介紹國際化 Flutter 應用的不同方法。

<a name="alternative-class"></a>
### An alternative class for the app's localized resources

### 應用程式本地化資源的替代類

The previous example was defined in terms of the Dart `intl`
package. Developers can choose their own approach for managing
localized values for the sake of simplicity or perhaps to integrate
with a different i18n framework.

之前的範例應用主要根據 Dart `intl` package 定義，為了簡單起見，
或者可能想要與不同的 i18n 框架整合，開發者也可以選擇自己的方法來管理本地化的值。

Complete source code for the [`minimal`][] app.

點選檢視 [`minimal`][] 應用的完整原始碼。

In the below example, the `DemoLocalizations` class 
includes all of its translations directly in per language Maps.

在下面這個範例中，包含應用本地化版本的類 `DemoLocalizations`
直接在每種語言的 Map 中包括了所有的翻譯。

<?code-excerpt "minimal/lib/main.dart (Demo)"?>
```dart
class DemoLocalizations {
  DemoLocalizations(this.locale);

  final Locale locale;

  static DemoLocalizations of(BuildContext context) {
    return Localizations.of<DemoLocalizations>(context, DemoLocalizations)!;
  }

  static const _localizedValues = <String, Map<String, String>>{
    'en': {
      'title': 'Hello World',
    },
    'es': {
      'title': 'Hola Mundo',
    },
  };

  static List<String> languages ()=> _localizedValues.keys.toList();

  String get title {
    return _localizedValues[locale.languageCode]!['title']!;
  }
}
```

In the minimal app the `DemoLocalizationsDelegate` is slightly
different. Its `load` method returns a [`SynchronousFuture`][]
because no asynchronous loading needs to take place.

在 minimal 應用中，`DemoLocalizationsDelegate` 略有不同，
它的 `load` 方法返回一個 [`SynchronousFuture`][]，因為不需要進行非同步載入。

<?code-excerpt "minimal/lib/main.dart (Delegate)"?>
```dart
class DemoLocalizationsDelegate
    extends LocalizationsDelegate<DemoLocalizations> {
  const DemoLocalizationsDelegate();

  @override
  bool isSupported(Locale locale) => DemoLocalizations.languages().contains(locale.languageCode);


  @override
  Future<DemoLocalizations> load(Locale locale) {
    // Returning a SynchronousFuture here because an async "load" operation
    // isn't needed to produce an instance of DemoLocalizations.
    return SynchronousFuture<DemoLocalizations>(DemoLocalizations(locale));
  }

  @override
  bool shouldReload(DemoLocalizationsDelegate old) => false;
}
```

<a name="dart-tools"></a>
### Using the Dart intl tools

### 附錄：使用 Dart intl 工具

Before building an API using the Dart [`intl`][] package
you'll want to review the `intl` package's documentation.
Here's a summary of the process
for localizing an app that depends on the `intl` package.

在你使用 Dart [`intl`][] package 進行建構 API 之前，
你應該想要了解一下 `intl` package 的文件。

The demo app depends on a generated source file called
`l10n/messages_all.dart`, which defines all of the
localizable strings used by the app.

這個 demo app 依賴於一個產生的原始檔，叫做 `l10n/messages_all.dart`，
這個檔案定義了 app 使用的所有本地化的字串。

Rebuilding `l10n/messages_all.dart` requires two steps.

重建 `l10n/messages_all.dart` 需要 2 步。

 1. With the app's root directory as the current directory,
    generate `l10n/intl_messages.arb` from `lib/main.dart`:

    在 app 的根目錄，使用 `lib/main.dart` 產生 `l10n/intl_messages.arb`：

    ```terminal
    $ flutter pub run intl_translation:extract_to_arb --output-dir=lib/l10n lib/main.dart
    ```

    The `intl_messages.arb` file is a JSON format map with one entry for
    each `Intl.message()` function defined in `main.dart`. This
    file serves as a template for the English and Spanish translations,
    `intl_en.arb` and `intl_es.arb`.
    These translations are created by you, the developer.

    `intl_messages.arb` 是一個 JSON 格式的檔案，
    每一個入口代表定義在 `main.dart` 裡面的 `Intl.message()` 方法。
    `intl_en.arb` 和 `intl_es.arb` 分別作為英語和西班牙語翻譯的範本。
    這些翻譯是由你（開發者）來建立的。

 2. With the app's root directory as the current directory, generate
    `intl_messages_<locale>.dart` for each `intl_<locale>.arb` file and
    `intl_messages_all.dart`, which imports all of the messages files:

    在 app 的根目錄，產生每個 `intl_<locale>.arb` 
    檔案對應的 `intl_messages_<locale>.dart` 檔案，
    以及 `intl_messages_all.dart` 檔案，它引入了所有的資訊檔案。

    ```terminal
    $ flutter pub run intl_translation:generate_from_arb \
        --output-dir=lib/l10n --no-use-deferred-loading \
        lib/main.dart lib/l10n/intl_*.arb
    ```

    ***Windows does not support file name wildcarding.***
    Instead, list the .arb files that were generated by the
    `intl_translation:extract_to_arb` command.

    **Windows 系統不支援檔名萬用字元**。
    列出的 `.arb` 檔案是由 `intl_translation:extract_to_arb` 命令產生的。

    ```terminal
    $ flutter pub run intl_translation:generate_from_arb \
        --output-dir=lib/l10n --no-use-deferred-loading \
        lib/main.dart \
        lib/l10n/intl_en.arb lib/l10n/intl_fr.arb lib/l10n/intl_messages.arb
    ```

    The `DemoLocalizations` class uses the generated `initializeMessages()`
    function (defined in `intl_messages_all.dart`)
    to load the localized messages and `Intl.message()` to look them up.
    
    `DemoLocalizations` 類使用產生的 `initializeMessages()` 方法
    （該方法定義在 `intl_messages_all.dart` 檔案）
    來載入本地化的資訊，然後使用 `Intl.message()` 來查閱這些本地化的資訊。

[78 languages]: {{site.api}}/flutter/flutter_localizations/GlobalMaterialLocalizations-class.html
[`add_language`]: {{site.repo.this}}/tree/{{site.branch}}/examples/internationalization/add_language/lib/main.dart
[An alternative class for the app's localized resources]: #alternative-class
[an example]: {{site.repo.this}}/tree/{{site.branch}}/examples/internationalization/minimal
[`intl_example`]: {{site.repo.this}}/tree/{{site.branch}}/examples/internationalization/intl_example
[`gen_l10n_example`]: {{site.repo.this}}/tree/{{site.branch}}/examples/internationalization/gen_l10n_example
[flutter_localizations README]: {{site.repo.flutter}}/blob/master/packages/flutter_localizations/lib/src/l10n/README.md
[`GlobalMaterialLocalizations`]: {{site.api}}/flutter/flutter_localizations/GlobalMaterialLocalizations-class.html
[`InheritedWidget`]: {{site.api}}/flutter/widgets/InheritedWidget-class.html
[Internationalization based on the `intl` package]: {{site.repo.this}}/tree/{{site.branch}}/examples/internationalization/intl_example
[Internationalization User's Guide]: {{site.url}}/go/i18n-user-guide
[`intl`]: {{site.pub-pkg}}/intl
[`intl` tool]: #dart-tools
[`Intl.message()`]: {{site.pub-api}}/intl/latest/intl/Intl/message.html
[`languageCode`]: {{site.api}}/flutter/dart-ui/Locale/languageCode.html
[`load()`]: {{site.api}}/flutter/widgets/LocalizationsDelegate/load.html
[`Locale`]: {{site.api}}/flutter/dart-ui/Locale-class.html
[`localeResolutionCallback`]: {{site.api}}/flutter/widgets/LocaleResolutionCallback.html
[`Localizations`]: {{site.api}}/flutter/widgets/WidgetsApp/supportedLocales.html
[`Localizations.of(context,type)`]: {{site.api}}/flutter/widgets/Localizations/of.html
[`LocalizationsDelegate`]: {{site.api}}/flutter/widgets/LocalizationsDelegate-class.html
[material-global]: {{site.api}}/flutter/flutter_localizations/GlobalMaterialLocalizations-class.html
[`MaterialApp`]: {{site.api}}/flutter/material/MaterialApp-class.html
[`MaterialApp.onGenerateTitle`]: {{site.api}}/flutter/material/MaterialApp/onGenerateTitle.html
[`MaterialLocalizations`]: {{site.api}}/flutter/material/MaterialLocalizations-class.html
[`minimal`]: {{site.repo.this}}/tree/{{site.branch}}/examples/internationalization/minimal
[Minimal internationalization]: {{site.repo.this}}/tree/{{site.branch}}/examples/internationalization/minimal
[Setting up an internationalized app]: #setting-up
[`SynchronousFuture`]: {{site.api}}/flutter/foundation/SynchronousFuture-class.html
[`supportedLocales`]: {{site.api}}/flutter/material/MaterialApp/supportedLocales.html
[supportedLocales]: #specifying-supportedlocales
[Using the Dart intl tools]: #dart-tools
[widgets-local]: {{site.api}}/flutter/widgets/Localizations-class.html
[widgets-global]: {{site.api}}/flutter/flutter_localizations/GlobalWidgetsLocalizations-class.html
[`WidgetsApp`]: {{site.api}}/flutter/widgets/WidgetsApp-class.html
