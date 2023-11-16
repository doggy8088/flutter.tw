---
title: Internationalizing Flutter apps
title: Flutter 應用裡的國際化
short-title: i18n
description: How to internationalize your Flutter app.
description: 如何實現 Flutter 應用程式的國際化。
tags: Flutter開發
keywords: 國際化
---

<?code-excerpt path-base="internationalization"?>

{{site.alert.secondary}}

  <h4 class="no_toc">What you'll learn</h4>

  <h4 class="no_toc">你將學習到</h4>

  * How to track the device's locale (the user's preferred language).

    如何去獲取裝置的語言環境（使用者首選的語言）。
  * How to enable locale-specific Material or Cupertino widgets.

    如何去管理特定語言環境下的 Material 或 Cupertino widget。

  * How to manage locale-specific app values.
  
    如何去管理特定語言環境下的 app 值。
  
  * How to define the locales an app supports.
  
    如何去定義 app 支援的語言環境。
  
{{site.alert.end}}

If your app might be deployed to users who speak another
language then you'll need to internationalize it.
That means you need to write the app in a way that makes
it possible to localize values like text and layouts
for each language or locale that the app supports.
Flutter provides widgets and classes that help with
internationalization and the Flutter libraries
themselves are internationalized.

如果你的 app 會部署給說其他語言的使用者使用，那麼你就需要對它進行國際化。
這就意味著你在編寫 app 的時候，需要採用一種容易對它進行本地化的方式進行開發，
這種方式讓你能夠為每一種語言或者 app 所支援的語言環境下的文字和佈局等進行本地化。
Flutter 提供了 widgets 和類來幫助開發者進行國際化，
當然 Flutter 庫本身就是國際化的。

This page covers concepts and workflows necessary to
localize a Flutter application using the
`MaterialApp` and `CupertinoApp` classes,
as most apps are written that way.
However, applications written using the lower level
`WidgetsApp` class can also be internationalized
using the same classes and logic.

由於大多數應用程式都是以這種方式編寫的，
因此該頁面主要介紹了使用 `MaterialApp` 和 `CupertinoApp`
對 Flutter 應用程式進行本地化所需的概念和工作流程。
但是，使用較低級別的 `WidgetsApp` 類編寫的應用程式
也可以使用相同的類和邏輯進行國際化。

## Introduction to localizations in Flutter

## Flutter 應用本地化介紹

This section provides a tutorial on how to create and
internationalize a new Flutter application,
along with any additional setup
that a target platform might require.

本節主要介紹如何對 Flutter 應用進行國際化，以及針對目標平台需要設定的其他內容。

You can find the source code for this example in
[`gen_l10n_example`][].

你可以在 [`gen_l10n_example`][] 儲存庫找到原始碼。

[`gen_l10n_example`]: {{site.repo.this}}/tree/{{site.branch}}/examples/internationalization/gen_l10n_example

### Setting up an internation&shy;alized app: the Flutter<wbr>_localizations package {#setting-up}

### 配置一個國際化的 app：flutter_localizations package {#setting-up}

By default, Flutter only provides US English localizations.
To add support for other languages,
an application must specify additional
`MaterialApp` (or `CupertinoApp`) properties,
and include a package called `flutter_localizations`.
As of June 2023, this package supports [113 languages][]
and language variants.

預設情況下，Flutter 只提供美式英語的本地化。
如果想要新增其他語言，你的應用必須指定額外的
`MaterialApp` 或者 `CupertinoApp` 屬性並且
新增一個名為 `flutter_localizations` 的 package。
截至到 2023 年 1 月份，這個 package 已經支援大約 [113 種語言][113 languages]。

To begin, start by creating a new Flutter application
in a directory of your choice with the `flutter create` command.

若要開始使用，在 Flutter 工程資料夾下執行 `flutter create` 命令:

```terminal
$ flutter create <name_of_flutter_app>
```

To use `flutter_localizations`,
add the package as a dependency to your `pubspec.yaml` file, 
as well as the `intl` package:

想要使用 `flutter_localizations` 的話，
你需要在 `pubspec.yaml` 檔案中新增它和 `intl` 作為依賴：

```terminal
$ flutter pub add flutter_localizations --sdk=flutter
$ flutter pub add intl:any
```

This creates a `pubspec.yml` file with the following entries:

最終的 `pubspec.yaml` 檔案中形如：

<?code-excerpt "gen_l10n_example/pubspec.yaml (FlutterLocalizations)"?>
```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_localizations:
    sdk: flutter
  intl: any
```

Then import the `flutter_localizations` library and specify
`localizationsDelegates` and `supportedLocales` for
your `MaterialApp` or `CupertinoApp`:

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
    Locale('en'), // English
    Locale('es'), // Spanish
  ],
  home: MyHomePage(),
);
```

After introducing the `flutter_localizations` package
and adding the previous code,
the `Material` and `Cupertino`
packages should now be correctly localized in
one of the 113 supported locales.
Widgets should be adapted to the localized messages,
along with correct left-to-right or right-to-left layout.

引入 `flutter_localizations` package 並添加了上面的程式碼之後，
`Material` 和 `Cupertino` 包現在應該被正確地本地化為 113 個受支援的語言環境之一。
widget 應當與本地化資訊保持同步，並具有正確的從左到右或從右到左的佈局。 

Try switching the target platform's locale to
Spanish (`es`) and the messages should be localized.

你可以嘗試將目標平台的語言環境切換為西班牙語 (`es`)，
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

The elements of the `localizationsDelegates` list are
factories that produce collections of localized values.
`GlobalMaterialLocalizations.delegate` provides localized
strings and other values for the Material Components
library. `GlobalWidgetsLocalizations.delegate`
defines the default text direction,
either left-to-right or right-to-left, for the widgets library.

`localizationDelegates` 陣列是用於產生本地化值集合的工廠。
`GlobalMaterialLocalizations.delegate` 為 Material 元件庫
提供本地化的字串和一些其他的值。
`GlobalWidgetsLocalizations.delegate` 為 widgets 庫
定義了預設的文字排列方向，由左到右或者由右到左。

More information about these app properties, the types they
depend on, and how internationalized Flutter apps are typically
structured, is covered in this page.

想知道更多關於這些 app 屬性，
它們依賴的型別以及那些國際化的 Flutter app 通常是如何組織的，
可以繼續閱讀下面內容。

[113 languages]: {{site.api}}/flutter/flutter_localizations/GlobalMaterialLocalizations-class.html

<a id="overriding-locale"></a>
### Overriding the locale

### 重載語言

`Localizations.override` is a factory constructor
for the `Localizations` widget that allows for
(the typically rare) situation where a section of your application
needs to be localized to a different locale than the locale
configured for your device. 

`Localizations.override` 提供了一個工廠構造方法，
使得你可以從在某一個位置設定與應用不同的語言（非一般情況）。

To observe this behavior, add a call to `Localizations.override`
and a simple `CalendarDatePicker`:

下面的範例展示了 `Localizations.override` 與 `CalendarDatePicker` 組合使用的情況：

<?code-excerpt "gen_l10n_example/lib/examples.dart (CalendarDatePicker)"?>
```dart
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text(widget.title),
    ),
    body: Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          // Add the following code
          Localizations.override(
            context: context,
            locale: const Locale('es'),
            // Using a Builder to get the correct BuildContext.
            // Alternatively, you can create a new widget and Localizations.override
            // will pass the updated BuildContext to the new widget.
            child: Builder(
              builder: (context) {
                // A toy example for an internationalized Material widget.
                return CalendarDatePicker(
                  initialDate: DateTime.now(),
                  firstDate: DateTime(1900),
                  lastDate: DateTime(2100),
                  onDateChanged: (value) {},
                );
              },
            ),
          ),
        ],
      ),
    ),
  );
}
```

Hot reload the app and the `CalendarDatePicker`
widget should re-render in Spanish.

應用熱重載後，你將能夠發現 `CalendarDatePicker` widget 顯示為西班牙語了。

<a id="adding-localized-messages"></a>
### Adding your own localized messages

### 新增您自己的本地化資訊

After adding the `flutter_localizations` package,
you can configure localization.
To add localized text to your application,
complete the following instructions:

引入 `flutter_localizations` package 後，
請按照以下說明將本地化的文字新增到您的應用。

1. Add the `intl` package as a dependency, pulling
   in the version pinned by `flutter_localizations`:

   將 `intl` package 新增為依賴，
   使用 `any` 作為 `flutter_localizations` 的版本值:

   ```terminal
   $ flutter pub add intl:any
   ```

2. Open the `pubspec.yaml` file and enable the `generate` flag. 
   This flag is found in the `flutter` section in the pubspec file.

   另外，在 `pubspec.yaml` 檔案中，啟用 `generate` 標誌。
   該設定項新增在 pubspec 中 Flutter 部分，
   通常處在 pubspec 檔案中後面的部分。

   <?code-excerpt "gen_l10n_example/pubspec.yaml (Generate)"?>
   ```yaml
   # The following section is specific to Flutter.
   flutter:
     generate: true # Add this line
   ```

3. Add a new yaml file to the root directory of the Flutter project.
   Name this file `l10n.yaml` and include following content:

   在 Flutter 專案的根目錄中新增一個新的 yaml 檔案，
   命名為 `l10n.yaml`，其內容如下：

   <?code-excerpt "gen_l10n_example/l10n.yaml"?>
   ```yaml
   arb-dir: lib/l10n
   template-arb-file: app_en.arb
   output-localization-file: app_localizations.dart
   ```

   This file configures the localization tool.
   In this example, you've done the following:

   該檔案用於配置本地化工具。它為你的專案配置瞭如下內容：

   * Put the [App Resource Bundle][] (`.arb`) input files in
     `${FLUTTER_PROJECT}/lib/l10n`.

     將 [應用資源包][App Resource Bundle] (`.arb`)
     的輸入路徑指定為 `${FLUTTER_PROJECT}/lib/l10n`。

     The `.arb` provide localization resources for your app. 

     `.arb` 檔案提供了應用的本地化資源。

   * Set the English template as `app_en.arb`.

     將英文的語言範本設定為 `app_en.arb`。

   * Told Flutter to generate localizations in the
     `app_localizations.dart` file.

     指定 Flutter 產生本地化內容到 `app_localizations.dart` 檔案。

4. In `${FLUTTER_PROJECT}/lib/l10n`,
   add the `app_en.arb` template file. For example:

   在 `${FLUTTER_PROJECT}/lib/l10n` 中，新增 `app_en.arb` 範本檔案。如下：

   <?code-excerpt "gen_l10n_example/lib/l10n/app_en.arb" take="5" replace="/},/}\n}/g"?>
   ```json
   {
     "helloWorld": "Hello World!",
     "@helloWorld": {
       "description": "The conventional newborn programmer greeting"
     }
   }
   ```

5. Add another bundle file called `app_es.arb` in the same directory.
   In this file, add the Spanish translation of the same message.

   接下來，在同一目錄中新增一個 `app_es.arb` 檔案，
   對同一條資訊做西班牙語的翻譯：

   <?code-excerpt "gen_l10n_example/lib/l10n/app_es.arb"?>
   ```json
   {
       "helloWorld": "¡Hola Mundo!"
   }
   ```

6. Now, run `flutter pub get` or `flutter run` and codegen takes place automatically.
   You should find generated files in
   `${FLUTTER_PROJECT}/.dart_tool/flutter_gen/gen_l10n`.
   Alternatively, you can also run `flutter gen-l10n` to
   generate the same files without running the app.

   現在，執行 `flutter run` 命令，
   您將在 `${FLUTTER_PROJECT}/.dart_tool/flutter_gen/gen_l10n` 中看到產生的檔案。
   同樣的，你可以在應用沒有執行的時候執行
   `flutter gen-l10n` 來產生本地化檔案。

7. Add the import statement on `app_localizations.dart` and
   `AppLocalizations.delegate`
   in your call to the constructor for `MaterialApp`:

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
       Locale('en'), // English
       Locale('es'), // Spanish
     ],
     home: MyHomePage(),
   );
   ```

   The `AppLocalizations` class also provides auto-generated
   `localizationsDelegates` and `supportedLocales` lists.
   You can use these instead of providing them manually.

   `AppLocalizations` 類也可以自動自動產生
   `localizationsDelegates` 和 `supportedLocales` 列表，
   而無需手動提供它們。

   <?code-excerpt "gen_l10n_example/lib/examples.dart (MaterialAppExample)"?>
   ```dart
   const MaterialApp(
     title: 'Localizations Sample App',
     localizationsDelegates: AppLocalizations.localizationsDelegates,
     supportedLocales: AppLocalizations.supportedLocales,
   );
   ```

8. Now you can use `AppLocalizations` anywhere in your app:

   現在，你可以在應用的任意地方使用 `AppLocalizations` 了:

   <?code-excerpt "gen_l10n_example/lib/main.dart (InternationalizedTitle)"?>
   ```dart
   appBar: AppBar(
     // The [AppBar] title text should update its message
     // according to the system locale of the target platform.
     // Switching between English and Spanish locales should
     // cause this text to update.
     title: Text(AppLocalizations.of(context)!.helloWorld),
   ),
   ```

   This code generates a `Text` widget that displays "Hello World!"
   if the target device's locale is set to English,
   and "¡Hola Mundo!" if the target device's locale is set
   to Spanish. In the `arb` files,
   the key of each entry is used as the method name of the getter,
   while the value of that entry contains the localized message.
   
   如果目標裝置的語言環境設定為英語，
   此程式碼產生器的 `Text` widget 會展示「Hello World!」。
   如果目標裝置的語言環境設定為西班牙語，則展示「Hola Mundo!」，
   在 `arb` 檔案中，每個條目的鍵值都被用作 getter 的方法名稱，
   而該條目的值則表示本地化的資訊。

The [`gen_l10n_example`][] uses this tool.

要檢視使用該工具的範例 Flutter 應用，請參閱 [`gen_l10n_example`][]。

To localize your device app description,
pass the localized string to
[`MaterialApp.onGenerateTitle`][]:

如需本地化裝置應用描述，你可以將本地化後的字串傳遞給
[`MaterialApp.onGenerateTitle`][]:

<?code-excerpt "intl_example/lib/main.dart (MaterialAppTitleExample)"?>
```dart
return MaterialApp(
  onGenerateTitle: (context) => DemoLocalizations.of(context).title,
```

[App Resource Bundle]: {{site.github}}/google/app-resource-bundle
[`gen_l10n_example`]: {{site.repo.this}}/tree/{{site.branch}}/examples/internationalization/gen_l10n_example
[`MaterialApp.onGenerateTitle`]: {{site.api}}/flutter/material/MaterialApp/onGenerateTitle.html

### Placeholders, plurals, and selects

### 佔位符、複數和選項

{{site.alert.tip}}

  When using VS Code, add the [arb-editor extension][].
  This extension adds syntax highlighting, snippets, 
  diagnostics, and quick fixes to help edit `.arb` template files.

  使用 VS Code 時，新增 [arb-editor 擴充][arb-editor extension] 。
  該擴充可增添語法高亮顯示、片段、診斷和快速修復功能，
  以輔助編輯 `.arb` 範本檔案。

{{site.alert.end}}

[arb-editor extension]: https://marketplace.visualstudio.com/items?itemName=Google.arb-editor

You can also include application values in a message with
special syntax that uses a _placeholder_ to generate a method
instead of a getter.
A placeholder, which must be a valid Dart identifier name,
becomes a positional parameter in the generated method in the
`AppLocalizations` code. Define a placeholder name by wrapping
it in curly braces as follows:

你還可以使用特殊語法在資訊中包含應用程式的值，
該語法使用 _佔位符_ 產生方法（並非 getter）。
佔位符必須是有效的 Dart 識別符號名稱，
它將成為 `AppLocalizations` 程式碼中產生方法的位置引數。
用大括號定義佔位符名稱，如下所示：

```json
"{placeholderName}"
```

Define each placeholder in the `placeholders` object
in the app's `.arb` file. For example,
to define a hello message with a `userName` parameter,
add the following to `lib/l10n/app_en.arb`:

在應用程式 `.arb` 檔案內的 `placeholders` 物件中定義每個佔位符。
例如，需要定義帶有 `userName` 引數的 hello 資訊，
請在 `lib/l10n/app_en.arb` 中新增以下內容：

<?code-excerpt "gen_l10n_example/lib/l10n/app_en.arb" skip="5" take="10" replace="/},$/}/g"?>
```json
"hello": "Hello {userName}",
"@hello": {
  "description": "A message with a single parameter",
  "placeholders": {
    "userName": {
      "type": "String",
      "example": "Bob"
    }
  }
}
```

This code snippet adds a `hello` method call to
the `AppLocalizations.of(context)` object,
and the method accepts a parameter of type `String`;
the `hello` method returns a string.
Regenerate the `AppLocalizations` file.

此程式碼段為 `AppLocalizations.of(context)` 物件
添加了一個 `hello` 方法呼叫，
該方法接收一個 `String` 型別的引數；
`hello` 方法返回一個字串。
重新產生 `AppLocalizations` 檔案。

Replace the code passed into `Builder` with the following:

將 `Builder` 中的程式碼替換為以下程式碼：

<?code-excerpt "gen_l10n_example/lib/main.dart (Placeholder)" remove="/wombat|Wombats|he'|they|pronoun/"?>
```dart
// Examples of internationalized strings.
return Column(
  children: <Widget>[
    // Returns 'Hello John'
    Text(AppLocalizations.of(context)!.hello('John')),
  ],
);
```

You can also use numerical placeholders to specify multiple values.
Different languages have different ways to pluralize words.
The syntax also supports specifying _how_ a word should be pluralized.
A _pluralized_ message must include a `num` parameter indicating
how to pluralize the word in different situations.
English, for example, pluralizes "person" to "people",
but that doesn't go far enough.
The `message0` plural might be "no people" or "zero people".
The `messageFew` plural might be
"several people", "some people", or "a few people". 
The `messageMany` plural might
be  "most people" or "many people", or "a crowd". 
Only the more general `messageOther` field is required.
The following example shows what options are available:

你還可以使用數字佔位符來指定多個值。
不同的語言有不同的單詞複數化形式。
該語法還支援指定單詞的複數化形式。
一個 _複數化_ 資訊必須包含一個 `num` 引數，
指明在不同情況下該單詞的複數化形式。
例如，英語將「person」複數為「people」，但這還不夠。
`message0` 的複數可能是「no people」或「zero people」。
`messageFew` 的複數可能是「several people」、「some people」或「a few people」。
`messageMany` 的複數可能是「most people」、「many people」或「a crowd」。
只有更通用的 `messageOther` 欄位是必填的。
下面的範例顯示了可用的選項：

```json
"{countPlaceholder, plural, =0{message0} =1{message1} =2{message2} few{messageFew} many{messageMany} other{messageOther}}"
```

The previous expression is replaced by the message variation
(`message0`, `message1`, ...) corresponding to the value
of the `countPlaceholder`.
Only the `messageOther` field is required.

前面的表示式由 `countPlaceholder` 值相對應的資訊變數
（`message0`、`message1`、...）所替代。
只有 `messageOther` 欄位是必填的。

The following example defines a message that pluralizes
the word, "wombat":

下面的範例定義了「袋熊」複數化的資訊：

{% raw %}
<?code-excerpt "gen_l10n_example/lib/l10n/app_en.arb" skip="15" take="10" replace="/},$/}/g"?>
```json
"nWombats": "{count, plural, =0{no wombats} =1{1 wombat} other{{count} wombats}}",
"@nWombats": {
  "description": "A plural message",
  "placeholders": {
    "count": {
      "type": "num",
      "format": "compact"
    }
  }
}
```
{% endraw %}

Use a plural method by passing in the `count` parameter:

透過傳遞 `count` 引數來使用複數方法：

<?code-excerpt "gen_l10n_example/lib/main.dart (Placeholder)" remove="/John|he|she|they|pronoun/" replace="/\[/[\n    .../g"?>
```dart
// Examples of internationalized strings.
return Column(
  children: <Widget>[
    ...
    // Returns 'no wombats'
    Text(AppLocalizations.of(context)!.nWombats(0)),
    // Returns '1 wombat'
    Text(AppLocalizations.of(context)!.nWombats(1)),
    // Returns '5 wombats'
    Text(AppLocalizations.of(context)!.nWombats(5)),
  ],
);
```

Similar to plurals,
you can also choose a value based on a `String` placeholder.
This is most often used to support gendered languages.
The syntax is as follows:

與複數類似，
你也可以根據 `String` 佔位符選擇一個值。
這通常用於性別。
語法如下：

```json
"{selectPlaceholder, select, case{message} ... other{messageOther}}"
```

The next example defines a message that
selects a pronoun based on gender:

下面的範例定義了一條資訊，
該資訊根據性別選擇代詞：

{% raw %}
<?code-excerpt "gen_l10n_example/lib/l10n/app_en.arb" skip="25" take="9" replace="/},$/}/g"?>
```json
"pronoun": "{gender, select, male{he} female{she} other{they}}",
"@pronoun": {
  "description": "A gendered message",
  "placeholders": {
    "gender": {
      "type": "String"
    }
  }
}
```
{% endraw %}

Use this feature by
passing the gender string as a parameter:

將性別字串作為引數傳遞，
即可使用該功能：

<?code-excerpt "gen_l10n_example/lib/main.dart (Placeholder)" remove="/'He|hello|ombat/" replace="/\[/[\n    .../g"?>
```dart
// Examples of internationalized strings.
return Column(
  children: <Widget>[
    ...
    // Returns 'he'
    Text(AppLocalizations.of(context)!.pronoun('male')),
    // Returns 'she'
    Text(AppLocalizations.of(context)!.pronoun('female')),
    // Returns 'they'
    Text(AppLocalizations.of(context)!.pronoun('other')),
  ],
);
```

Keep in mind that when using `select` statements,
comparison between the parameter and the actual
value is case-sensitive.
That is, `AppLocalizations.of(context)!.pronoun("Male")`
defaults to the "other" case, and returns "they".

請記住，在使用 `select` 陳述式時，
引數和實際值之間的比較是區分大小寫的。
也就是說，`AppLocalizations.of(context)!.pronoun("Male")` 
預設為「other」，並返回「they」。

### Escaping syntax

### 避免語法解析

Sometimes, you have to use tokens,
such as `{` and `}`, as normal characters.
To ignore such tokens from being parsed,
enable the `use-escaping` flag by adding the
following to `l10n.yaml`:

有時你會使用符號（例如 `{` 或 `}`）作為普通文字的一部分。
如果你想要讓它們不被解析為一種語法，可以在 `l10n.yaml` 中設定 `use-escaping`：

```yaml
use-escaping: true
```

The parser ignores any string of characters
wrapped with a pair of single quotes.
To use a normal single quote character,
use a pair of consecutive single quotes.
For example, the follow text is converted
to a Dart `String`:

啟用後，解析器會忽略使用一對單引號包括的文字，
如果在文字中又想使用單個單引號，需要使用成對的單引號進行轉義。
例如，下面的文字會直接轉為 Dart 的 `String`：

```json
{
  "helloWorld": "Hello! '{Isn''t}' this a wonderful day?"
}
```

The resulting string is as follows:

結果如下：

```dart
"Hello! {Isn't} this a wonderful day?"
```

### Messages with numbers and currencies

### 包含數字和貨幣的資訊

Numbers, including those that represent currency values,
are displayed very differently in different locales. 
The localizations generation tool in
`flutter_localizations` uses the
[`NumberFormat`]({{site.api}}/flutter/intl/NumberFormat-class.html)
class in the `intl` package to format
numbers based on the locale and the desired format.

數字，包括那些代表貨幣價值的數字，
在不同的本地化環境中顯示的方式大相徑庭。
在 `flutter_localizations` 中的本地化產生工具
使用了 `intl` package 中的 
[`NumberFormat`]({{site.api}}/flutter/intl/NumberFormat-class.html) 
類，根據本地化和所需的格式來格式化數字。

The `int`, `double`, and `number` types can use any of the
following `NumberFormat` constructors:

`int`、`double` 和 `number` 型別可以使用
以下任何一個 `NumberFormat` 建構函式：

<div class="table-wrapper" markdown="1">
| <t>Message "format" value</t><t>資訊「格式」值</t> | <t>Output for 1200000</t><t>輸出為 1200000</t> |
| --------------------------- | ------------------ |
| `compact`                   | "1.2M"             |
| `compactCurrency`*          | "$1.2M"            |
| `compactSimpleCurrency`*    | "$1.2M"            |
| `compactLong`               | "1.2 million"      |
| `currency`*                 | "USD1,200,000.00"  |
| `decimalPattern`            | "1,200,000"        |
| `decimalPercentPattern`*    | "120,000,000%"     |
| `percentPattern`            | "120,000,000%"     |
| `scientificPattern`         | "1E6"              |
| `simpleCurrency`*           | "$1,200,000"       |
{:.table.table-striped}
</div>

The starred `NumberFormat` constructors in the table
offer optional, named parameters.
Those parameters can be specified as the value
of the placeholder's `optionalParameters` object.
For example, to specify the optional `decimalDigits`
parameter for `compactCurrency`,
make the following changes to the `lib/l10n/app_en.arg` file:

表中帶星<sup>(*)</sup>的 `NumberFormat` 建構函式提供了可選的命名引數。
這些引數可以指定為 placeholders 中 `optionalParameters` 物件的值。
例如，要為 `compactCurrency` 指定可選的 `decimalDigits` 引數，
請對 `lib/l10n/app_en.arg` 檔案進行以下更改：

{% raw %}
<?code-excerpt "gen_l10n_example/lib/l10n/app_en.arb" skip="34" take="13" replace="/},$/}/g"?>
```json
"numberOfDataPoints": "Number of data points: {value}",
"@numberOfDataPoints": {
  "description": "A message with a formatted int parameter",
  "placeholders": {
    "value": {
      "type": "int",
      "format": "compactCurrency",
      "optionalParameters": {
        "decimalDigits": 2
      }
    }
  }
}
```
{% endraw %}

### Messages with dates

### 帶日期的資訊

Dates strings are formatted in many different ways
depending both the locale and the app's needs.  

日期字串的格式有很多種，
取決於地區和應用程式的需求。

Placeholder values with type `DateTime` are formatted with
[`DateFormat`][] in the `intl` package.

`DateTime` 型別的佔位符使用 `intl` package 中的 
[`DateFormat`][] 格式化。

There are 41 format variations,
identified by the names of their `DateFormat` factory constructors.
In the following example, the `DateTime` value
that appears in the `helloWorldOn` message is
formatted with `DateFormat.yMd`:

格式變體共有 41 種，
由 `DateFormat` factory 建構函式的名稱標識。
在下面的範例種，
出現在 `helloWorldOn` 資訊中的 `DateTime` 值
是用 `DateFormat.yMd` 進行的格式化：

```json
"helloWorldOn": "Hello World on {date}",
"@helloWorldOn": {
  "description": "A message with a date parameter",
  "placeholders": {
    "date": {
      "type": "DateTime",
      "format": "yMd"
    }
  }
}
```

In an app where the locale is US English,
the following expression would produce  "7/9/1959".
In a Russian locale, it would produce "9.07.1959".

在語言環境為英語（美國）的應用中，以下表達式將會是 7/9/1959，
在俄羅斯語言環境中，它將是 9.07.1959。

```dart
AppLocalizations.of(context).helloWorldOn(DateTime.utc(1959, 7, 9))
```

[`DateFormat`]: {{site.api}}/flutter/intl/DateFormat-class.html

<a id="ios-specifics"></a>
### Localizing for iOS: Updating the iOS app bundle

### iOS 本地化：更新 iOS app bundle

Typically, iOS applications define key application metadata,
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
   in the [`supportedLocales`][] parameter.

   選擇並展開新建立的 `Localizations` 項。對於您的應用程式支援的每種語言環境，
   請新增一個新項，然後從 **Value** 欄位中的彈出選單中選擇要新增的語言環境。
   該列表應需要與 [`supportedLocales`][] 引數中列出的語言一致。

5. Once all supported locales have been added, save the file.

   新增所有受支援的語言環境後，儲存檔案。

<a id="advanced-customization">
## Advanced topics for further customization

## 客製的進階操作

This section covers additional ways to customize a
localized Flutter application.

本節介紹自訂本地 Flutter 應用程式的其他方法。

<a id="advanced-locale"></a>
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
If a user's preferred locale isn't specified,
Flutter selects the closest match,
which likely contains differences to what the user expects.
Flutter only resolves to locales defined in `supportedLocales`
and provides scriptCode-differentiated localized
content for commonly used languages.
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

[`Localizations`]: {{site.api}}/flutter/widgets/WidgetsApp/supportedLocales.html

<a id="tracking-locale"></a>
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

You can always look up an app's current locale with
`Localizations.localeOf()`:

你可以透過呼叫 `Localizations.localeOf()`
方法來檢視 app 當前的語言環境:

<?code-excerpt "gen_l10n_example/lib/examples.dart (MyLocale)"?>
```dart
Locale myLocale = Localizations.localeOf(context);
```

[`Locale`]: {{site.api}}/flutter/dart-ui/Locale-class.html
[`WidgetsApp`]: {{site.api}}/flutter/widgets/WidgetsApp-class.html
[widgets-global]: {{site.api}}/flutter/flutter_localizations/GlobalWidgetsLocalizations-class.html

<a id="specifying-supportedlocales"></a>
### Specifying the app's supported&shy;Locales parameter

### 指定應用程式 supported­Locales 引數

Although the `flutter_localizations` library currently supports
113 languages and language variants, only English language translations
are available by default. It's up to the developer to decide exactly
which languages to support.

儘管 `flutter_localizations` 庫目前支援 113 種語言和語言變體，
但預設情況下僅提供英語譯文。
具體支援哪些語言由開發人員決定。

The `MaterialApp` [`supportedLocales`][]
parameter limits locale changes. When the user changes the locale
setting on their device, the app's `Localizations` widget only
follows suit if the new locale is a member of this list.
If an exact match for the device locale isn't found,
then the first supported locale with a matching [`languageCode`][]
is used. If that fails, then the first element of the
`supportedLocales` list is used.

`MaterialApp` 的 [`supportedLocales`][] 引數限制了本地化設定的更改。
當用戶更改裝置上的語言設定時，
只有在 [`supportedLocales`][] 引數列表中包含了使用者更改的本地化語言設定的情況下，
應用程式的 `Localizations` widget 才會生效。
如果找不到與裝置本地化完全匹配的語言，
則會使用與 [`languageCode`][] 匹配的第一個受支援的語言。
如果仍然找不到，
則使用 `supportedLocales` 列表中的第一個元素。

An app that wants to use a different "locale resolution"
method can provide a [`localeResolutionCallback`][].
For example, to have your app unconditionally accept
whatever locale the user selects:

如果應用程式希望使用不同的「本地化解析」方法，
可以提供 [`localeResolutionCallback`][]。
例如，應用程式可以無條件接受使用者選擇的任何語言：

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

[`languageCode`]: {{site.api}}/flutter/dart-ui/Locale/languageCode.html
[`localeResolutionCallback`]: {{site.api}}/flutter/widgets/LocaleResolutionCallback.html
[`supportedLocales`]: {{site.api}}/flutter/material/MaterialApp/supportedLocales.html

### Configuring the l10n.yaml file

### 配置 l10n.yaml 檔案

The `l10n.yaml` file allows you to configure the `gen-l10n` tool
to specify the following:

透過 `l10n.yaml` 檔案，
你可以配置 `gen-l10n` 工具，
指定以下內容：

* where all the input files are located

  所有輸入檔案的位置

* where all the output files should be created

  所有輸出檔案的建立位置

* what Dart class name to give your localizations delegate

  為本地化委託賦予自訂的 Dart 類別名稱

For a full list of options, either run `flutter gen-l10n --help`
at the command line or refer to the following table:

獲取完整的選項列表，
可在命令列中執行 `flutter gen-l10n --help` 
或參考下表內容：

<div class="table-wrapper" markdown="1">
| <t>Option</t><t>可選項</t>           | <t>Description</t><t>說明</t> |
| ------------------------------------| ------------------ |
| `arb-dir`                           | The directory where the template and translated arb files are located. The default is `lib/l10n`. |
| `arb-dir`                           | 範本和翻譯 arb 檔案所在的目錄。 預設為 `lib/l10n`。 |
| `output-dir`                        | The directory where the generated localization classes are written. This option is only relevant if you want to generate the localizations code somewhere else in the Flutter project. You also need to set the `synthetic-package` flag to false.<br /><br />The app must import the file specified in the `output-localization-file` option from this directory. If unspecified, this defaults to the same directory as the input directory specified in `arb-dir`. |
| `output-dir`                        | 產生本地化類別的目錄。 只有當你想在 Flutter 專案的其他位置產生本地化程式碼時，才需要使用此選項。 你還需要將 `synthetic-package` 標誌設為 false。<br /><br /> 應用程式必須從該目錄匯入 `output-localization-file` 選項中指定的檔案。 如果未指定，則預設與 `arb-dir` 中指定的輸入目錄相同。 |
| `template-arb-file`                 | The template arb file that is used as the basis for generating the Dart localization and messages files. The default is `app_en.arb`. |
| `template-arb-file`                 | 用於產生 Dart 本地化和資訊檔案的 arb 範本檔案。 預設為 `app_en.arb`。 |
| `output-localization-file`          | The filename for the output localization and localizations delegate classes. The default is `app_localizations.dart`. |
| `output-localization-file`          | 輸出本地化和本地化委託類別的檔名。 預設為 `app_localizations.dart`。 |
| `untranslated-messages-file`        | The location of a file that describes the localization messages haven't been translated yet. Using this option creates a JSON file at the target location, in the following format: <br /> <br />`"locale": ["message_1", "message_2" ... "message_n"]`<br /><br /> If this option is not specified, a summary of the messages that haven't been translated are printed on the command line. |
| `untranslated-messages-file`        | 描述尚未翻譯的本地化資訊的檔案位置。 使用該選項會在目標位置建立一個 JSON 檔案，格式如下：<br /><br /> `"locale": ["message_1", "message_2" ... "message_n"]` <br /><br /> 如果未指定此選項，則會在命令列中列印尚未翻譯的資訊摘要。 |
| `output-class`                      | The Dart class name to use for the output localization and localizations delegate classes. The default is `AppLocalizations`. |
| `output-class`                      | 用於輸出本地化和本地化委託類別的 Dart 類別名稱。 預設為 `AppLocalizations`。 |
| `preferred-supported-locales`       | The list of preferred supported locales for the application. By default, the tool generates the supported locales list in alphabetical order. Use this flag to default to a different locale.<br /><br />For example, pass in `[ en_US ]` to default to American English if a device supports it. |
| `preferred-supported-locales`       | 應用程式首選支援的本地語言列表。 預設情況下，工具會按字母順序產生支援的本地語言列表。 使用此標記可預設為不同的本地語言。 <br /><br /> 例如，裝置支援美式英語，則輸入 `[ en_US ]` 預設為美式英語。 |
| `header`                            | The header to prepend to the generated Dart localizations files. This option takes in a string.<br /><br />For example, pass in `"/// All localized files."` to prepend this string to the generated Dart file.<br /><br />Alternatively, check out the `header-file` option to pass in a text file for longer headers. |
| `header`                            | 在產生的 Dart 本地化檔案中預置標頭檔案。 該選項包含一個字串。 <br /><br /> 例如，輸入 `"/// All localized files."`，就會在產生的 Dart 檔案中預置這個字串。 <br /><br /> 或者，還可以使用 `header-file` 選項來傳遞一個文字檔案，以獲得更長的標頭檔案。 |
| `header-file`                       | The header to prepend to the generated Dart localizations files. The value of this option is the name of the file that contains the header text that is inserted at the top of each generated Dart file. <br /><br /> Alternatively, check out the `header` option to pass in a string for a simpler header.<br /><br />This file should be placed in the directory specified in `arb-dir`. |
| `header-file`                       | 在產生的 Dart 本地化檔案中預置標頭檔案。 該選項的值是包含標頭檔案文字的檔名，標頭檔案文字將插入每個產生的 Dart 檔案的頂部。 <br /><br /> 或者，還可以使用 `header` 選項來傳遞一個字串，以獲得更簡單的標頭檔案。 <br /><br /> 該檔案應放在 `arb-dir` 中指定的目錄下。 |
| `[no-]use-deferred-loading`         | Specifies whether to generate the Dart localization file with locales imported as deferred, allowing for lazy loading of each locale in Flutter web.<br /><br />This can reduce a web app's initial startup time by decreasing the size of the JavaScript bundle. When this flag is set to true, the messages for a particular locale are only downloaded and loaded by the Flutter app as they are needed. For projects with a lot of different locales and many localization strings, it can improve performance to defer loading. For projects with a small number of locales, the difference is negligible, and might slow down the start up compared to bundling the localizations with the rest of the application.<br /><br />Note that this flag doesn't affect other platforms such as mobile or desktop. |
| `[no-]use-deferred-loading`         | 指定是否將產生的 Dart 本地化檔案延遲匯入，以便在 Flutter web 中對每個本地化進行延遲載入。 <br /><br /> 這可以減少 JavaScript 程式的大小，從而縮短 web 應用的初始啟動時間。 當此標記設定為 true 時，Flutter 應用程式只會在需要時下載和載入特定語言的資訊。 對於具有大量不同本地化字串的專案，延遲載入可以提高效能。 對於本地化字串數量較少的專案，兩者之間的差異可以忽略不計，但是將本地化字串與應用程式的其他部分捆綁在一起相比，可能會降低啟動速度。 <br /><br /> 請注意，此標記不會影響移動或桌面等其他平台。 |
| `gen-inputs-and-outputs-list`      | When specified, the tool generates a JSON file containing the tool's inputs and outputs, named `gen_l10n_inputs_and_outputs.json`.<br /><br />This can be useful for keeping track of which files of the Flutter project were used when generating the latest set of localizations.  For example, the Flutter tool's build system uses this file to keep track of when to call gen_l10n during hot reload.<br /><br />The value of this option is the directory where the JSON file is generated.  When null, the JSON file won't be generated. |
| `gen-inputs-and-outputs-list`      | 指定後，工具會產生一個 JSON 檔案，其中包含工具的輸入和輸出的內容，檔名為 `gen_l10n_inputs_and_outputs.json`。 <br /><br /> 這對於追蹤產生最新的本地化時使用了 Flutter 專案中的哪些檔案非常有用。 例如，Flutter 工具的建構系統會使用此檔案來追蹤在熱重載期間何時呼叫 gen_l10n。 <br /><br /> 該選項的值是產生 JSON 檔案的目錄。 如果為空，則不會產生 JSON 檔案。 |
| `synthetic-package`                 | Determines  whether the generated output files are generated as a synthetic package or at a specified directory in the Flutter project. This flag is `true` by default. When `synthetic-package` is set to `false`, it generates the localizations files in the directory specified by `arb-dir` by default. If `output-dir` is specified, files are generated there. |
| `synthetic-package`                 | 決定產生的輸出檔案是作為 synthetic package 還是在 Flutter 專案中指定的目錄下產生。 該標誌預設為 `true`。  `synthetic-package` 設定為 `false` 時，預設會在 `arb-dir` 指定的目錄下產生本地化檔案。 如果指定了 `output-dir` 目錄，則會在該目錄下產生檔案。 |
| `project-dir`                       | When specified, the tool uses the path passed into this option as the directory of the root Flutter project.<br /><br />When null, the relative path to the present working directory is used. |
| `project-dir`                       | 指定後，工具將使用此選項中傳遞的路徑作為 Flutter 專案的根目錄。 <br /><br /> 如果為空，則使用當前工作目錄的相對路徑。 |
| `[no-]required-resource-attributes` | Requires all resource ids to contain a corresponding resource attribute.<br /><br />By default, simple messages won't require metadata, but it's highly recommended as this provides context for the meaning of a message to readers.<br /><br />Resource attributes are still required for plural messages. |
| `[no-]required-resource-attributes` | 要求所有資源 ID 包含相應的資源屬性。 <br /><br /> 預設情況下，簡單資訊不需要元資料，但強烈建議使用元素據，因為它能為讀者提供資訊含義的上下文。 <br /><br /> 複數資訊仍然需要資源屬性。 |
| `[no-]nullable-getter`              | Specifies whether the localizations class getter is nullable.<br /><br />By default, this value is true so that `Localizations.of(context)` returns a nullable value for backwards compatibility. If this value is false, then a null check is performed on the returned value of `Localizations.of(context)`, removing the need for null checking in user code. |
| `[no-]nullable-getter`              | 指定本地化類 getter 是否可為空。 <br /><br /> 預設情況下，該值為 true，這樣 `Localizations.of(context)` 就會返回一個可歸零的值，以實現向下相容。 如果該值為 false，則會對 `Localizations.of(context)` 返回的值進行空值檢查，從而無需在使用者程式碼中進行空值檢查。 |
| `[no-]format`                       | When specified, the `dart format` command is run after generating the localization files. |
| `[no-]format`                       | 指定後，將在產生本地化檔案後執行 `dart format` 指令。 |
| `use-escaping`                      | Specifies whether to enable the use of single quotes as escaping syntax. |
| `use-escaping`                      | 指定是否啟用單引號作為轉義語法。 |
| `[no-]suppress-warnings`            | When specified, all warnings are suppressed. |
| `[no-]suppress-warnings`            | 指定後，將不會進行警告。 |
{:.table.table-striped}
</div>


## How internationalization in Flutter works

## Flutter 裡的國際化是如何工作的

This section covers the technical details of how localizations work
in Flutter. If you're planning on supporting your own set of localized
messages, the following content would be helpful.
Otherwise, you can skip this section.

本節涵蓋了 Flutter 中本地化工作的技術細節，
如果你計劃使用自定的一套本地化資訊，下面的內容會很有幫助。
反之則可以跳過本節。

<a id="loading-and-retrieving"></a>
### Loading and retrieving localized values

### 載入和獲取本地化值

The `Localizations` widget is used to load and
look up objects that contain collections of localized values.
Apps refer to these objects with [`Localizations.of(context,type)`][].
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
`load` methods, specify a `BuildContext` and the object's type.

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

```dart
Localizations.of<MaterialLocalizations>(context, MaterialLocalizations);
```

This particular `Localizations.of()` expression is used frequently,
so the `MaterialLocalizations` class provides a convenient shorthand:

因為這個特定的 `Localizations.of()` 表示式經常使用，
所以 `MaterialLocalizations` 類提供了一個快捷存取：

```dart
static MaterialLocalizations of(BuildContext context) {
  return Localizations.of<MaterialLocalizations>(context, MaterialLocalizations);
}

/// References to the localized values defined by MaterialLocalizations
/// are typically written like this:

tooltip: MaterialLocalizations.of(context).backButtonTooltip,
```

[`InheritedWidget`]: {{site.api}}/flutter/widgets/InheritedWidget-class.html
[`load()`]: {{site.api}}/flutter/widgets/LocalizationsDelegate/load.html
[`LocalizationsDelegate`]: {{site.api}}/flutter/widgets/LocalizationsDelegate-class.html
[`Localizations.of(context,type)`]: {{site.api}}/flutter/widgets/Localizations/of.html
[`MaterialApp`]: {{site.api}}/flutter/material/MaterialApp-class.html
[`MaterialLocalizations`]: {{site.api}}/flutter/material/MaterialLocalizations-class.html

<a id="defining-class"></a>
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
[`intl`][] package. The [An alternative class for the app's
localized resources](#alternative-class) section
describes [an example][] that doesn't depend on the `intl` package.

這個範例是基於 [`intl`][] package 提供的 API 和 工具開發的，
[app 本地化資源的替代方法](#alternative-class)
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
The message catalog is produced by an [`intl` tool](#dart-tools)
that analyzes the source code for classes that contain
`Intl.message()` calls.
In this case that would just be the `DemoLocalizations` class.

基於 `intl` package 的類引入了一個產生好的資訊目錄，
它提供了 `initializeMessage()` 方法和 `Intl.message()` 方法的
每個語言環境的備份儲存。
[`intl` 工具](#dart-tools) 透過分析包含 `Intl.message()`
呼叫類別的原始碼產生這個資訊目錄。在當前情況下，
就是 DemoLocalizations 的類（包含了 `Intl.message()` 呼叫）。

[an example]: {{site.repo.this}}/tree/{{site.branch}}/examples/internationalization/minimal
[`intl`]: {{site.pub-pkg}}/intl
[`Intl.message()`]: {{site.pub-api}}/intl/latest/intl/Intl/message.html

<a id="adding-language"></a>
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

The getters return "raw" Dart strings that have an `r` prefix,
such as `r'About $applicationName'`,
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

The date patterns and symbols of the locale also need to
be specified, which are defined in the source code as follows:

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

These values need to be modified for the locale to use the correct
date formatting. Unfortunately, since the `intl` library doesn't
share the same flexibility for number formatting,
the formatting for an existing locale must be used
as a substitute in `_NnMaterialLocalizationsDelegate`:

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
        // (see https://github.com/dart-lang/i18n/blob/main/pkgs/intl/lib/number_symbols_data.dart).
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

For more information about localization strings,
check out the [flutter_localizations README][].

需要了解更多關於本地化字串的內容，可以檢視 [flutter_localizations README][]。

Once you've implemented your language-specific subclasses of
`GlobalMaterialLocalizations` and `LocalizationsDelegate`,
you  need to add the language and a delegate instance to your app.
The following code sets the app's language to Nynorsk and
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

[`add_language`]: {{site.repo.this}}/tree/{{site.branch}}/examples/internationalization/add_language/lib/main.dart

[flutter_localizations README]: {{site.repo.flutter}}/blob/master/packages/flutter_localizations/lib/src/l10n/README.md
[`GlobalMaterialLocalizations`]: {{site.api}}/flutter/flutter_localizations/GlobalMaterialLocalizations-class.html

<a id="alternative-internationalization-workflows">
## Alternative internationalization workflows
  
## 其他的國際化方法

This section describes different approaches to internationalize
your Flutter application.

本節主要介紹國際化 Flutter 應用的不同方法。

<a id="alternative-class"></a>
### An alternative class for the app's localized resources

### 應用程式本地化資源的替代類

The previous example was defined in terms of the Dart `intl`
package. You can choose your own approach for managing
localized values for the sake of simplicity or perhaps to integrate
with a different i18n framework.

之前的範例應用主要根據 Dart `intl` package 定義，為了簡單起見，
或者可能想要與不同的 i18n 框架整合，開發者也可以選擇自己的方法來管理本地化的值。

Complete source code for the [`minimal`][] app.

點選檢視 [`minimal`][] 應用的完整原始碼。

In the following example, the `DemoLocalizations` class 
includes all of its translations directly in per language Maps:

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

  static List<String> languages() => _localizedValues.keys.toList();

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
  bool isSupported(Locale locale) =>
      DemoLocalizations.languages().contains(locale.languageCode);

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

[`SynchronousFuture`]: {{site.api}}/flutter/foundation/SynchronousFuture-class.html

<a id="dart-tools"></a>
### Using the Dart intl tools

### 附錄：使用 Dart intl 工具

Before building an API using the Dart [`intl`][] package,
review the `intl` package's documentation.
The following list summarizes the process for
localizing an app that depends on the `intl` package:

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
    $ dart run intl_translation:extract_to_arb --output-dir=lib/l10n lib/main.dart
    ```

    The `intl_messages.arb` file is a JSON format map with one entry for
    each `Intl.message()` function defined in `main.dart`.
    This file serves as a template for the English and Spanish translations,
    `intl_en.arb` and `intl_es.arb`.
    These translations are created by you, the developer.

    `intl_messages.arb` 是一個 JSON 格式的檔案，
    每一個入口代表定義在 `main.dart` 裡面的 `Intl.message()` 方法。
    `intl_en.arb` 和 `intl_es.arb` 分別作為英語和西班牙語翻譯的範本。
    這些翻譯是由你（開發者）來建立的。

 2. With the app's root directory as the current directory,
    generate `intl_messages_<locale>.dart` for each
    `intl_<locale>.arb` file and `intl_messages_all.dart`,
    which imports all of the messages files:

    在 app 的根目錄，產生每個 `intl_<locale>.arb` 
    檔案對應的 `intl_messages_<locale>.dart` 檔案，
    以及 `intl_messages_all.dart` 檔案，它引入了所有的資訊檔案。

    ```terminal
    $ dart run intl_translation:generate_from_arb \
        --output-dir=lib/l10n --no-use-deferred-loading \
        lib/main.dart lib/l10n/intl_*.arb
    ```

    ***Windows doesn't support file name wildcarding.***
    Instead, list the .arb files that were generated by the
    `intl_translation:extract_to_arb` command.

    **Windows 系統不支援檔名萬用字元**。
    列出的 `.arb` 檔案是由 `intl_translation:extract_to_arb` 命令產生的。

    ```terminal
    $ dart run intl_translation:generate_from_arb \
        --output-dir=lib/l10n --no-use-deferred-loading \
        lib/main.dart \
        lib/l10n/intl_en.arb lib/l10n/intl_fr.arb lib/l10n/intl_messages.arb
    ```

    The `DemoLocalizations` class uses the generated
    `initializeMessages()` function
    (defined in `intl_messages_all.dart`)
    to load the localized messages and `Intl.message()`
    to look them up.

    `DemoLocalizations` 類使用產生的 `initializeMessages()` 方法
    （該方法定義在 `intl_messages_all.dart` 檔案）
    來載入本地化的資訊，然後使用 `Intl.message()` 來查閱這些本地化的資訊。

## More information

## 更多資訊

If you learn best by reading code,
check out the following examples.

如果你希望透過程式碼進行學習，你可以檢視以下的範例。

* [`minimal`][]<br>
  The `minimal` example is designed to be as
  simple as possible.

  [最簡範例][`minimal`]<br>
  最簡展示瞭如果以最簡單的方式實現國際化。

* [`intl_example`][]<br>
  uses APIs and tools provided by the [`intl`][] package.

  [`intl` 範例][`intl_example`]<br>
  利用 [`intl`][] package 實現國際化的範例。

If Dart's `intl` package is new to you,
check out [Using the Dart intl tools](#dart-tools).

如果你還未使用過 `intl` package，你可以閱讀 [如何使用 Dart 的 intl 工具](#dart-tools)。

[`intl_example`]: {{site.repo.this}}/tree/{{site.branch}}/examples/internationalization/intl_example
[`minimal`]: {{site.repo.this}}/tree/{{site.branch}}/examples/internationalization/minimal

