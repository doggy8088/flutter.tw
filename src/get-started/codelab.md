---
title: Write your first Flutter app, part 1
title: 編寫第一個 Flutter 應用
description: How to write a web-based app in Flutter.
description: 如何使用 Flutter 編寫一個網頁應用。
short-title: Write your first app
short-title: 編寫第一個 Flutter 應用
tags: Flutter安裝,Flutter起步課程
keywords: Flutter 第一個應用,Flutter Hello World,codelab
prev:
  title: Test drive
  title: 開發體驗初探
  path: /docs/get-started/test-drive
next:
  title: Learn more
  title: 瞭解更多
  path: /docs/get-started/learn-more
diff2html: true
---

{{site.alert.tip}}

  This codelab walks you through writing your first Flutter
  app. You might prefer to try
  [writing your first Flutter app on the web][codelab-web].

  這篇 codelab 將帶你初體驗移動端 Flutter 應用開發。
  你也許更想嘗試 [編寫你的第一個 Flutter 網頁應用][codelab-web]。

  **Note that if you have [enabled web][],
  the completed app just works on all of these devices!**

  **提示：如果你已經 [啟用 web][enabled web]，你的應用可以在這裡面的所有裝置上執行。**

{{site.alert.end}}

If you prefer an instructor-led version of this codelab,
check out the following workshop:

如果你更喜歡由講師介紹的 codelab 版本，請檢視以下的 workshop：

<iframe width="560" height="315" src="https://player.bilibili.com/player.html?aid=846249495&bvid=BV1n54y1H7dZ&cid=354773704&page=1" title="Bilibili video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{% assign code-url = 'https://raw.githubusercontent.com/flutter/codelabs/main' -%}

<img src="/assets/images/docs/get-started/startup-namer-part-1.gif" alt="The app that you'll be building" class='site-image-right'>

{%- comment %}
  Code highlights in this page are green, to match diff additions.
{% endcomment -%}
<style>pre .highlight { background-color: #dfd; }</style>

This is a guide to creating your first Flutter app.
If you are familiar with object-oriented code and basic programming
concepts such as variables, loops, and conditionals,
you can complete this tutorial. You don’t need
previous experience with Dart, mobile, desktop, or web programming.

這是一個指引你完成第一個 Flutter 應用的手把手操作課程（我們也稱之為是 codelab）。
我們將會著手建立一個簡單的 Flutter 應用，無需 Dart 語言、
移動端開發、桌面端開發或 Web 開發的經驗，
只需你具備面嚮物件語言開發基礎即可（如變數，迴圈和條件陳述式）。

This codelab is part 1 of a two-part codelab.
You can find [part 2][] on [Google Developers Codelabs][]
(as well as a copy of this codelab, [part 1][]).

完整的課程分為兩部分，本頁面是第一部分的內容，
你可以在這裡檢視 [第二部分][part 2] 的內容。
(Codelabs 裡的第一部分內容與本頁內容相同)。

## What you'll build in part 1
{:.no_toc}

## 第一部分的內容概覽
{:.no_toc}

You’ll implement a simple app that generates proposed
names for a startup company. The user can select and unselect names,
saving the best ones. The code lazily generates 10 names at a time.
As the user scrolls, more names are generated.
There is no limit to how far a user can scroll.

你將完成一個簡單的應用，功能是：為一個創業公司產生建議的公司名稱。
使用者可以選擇和取消選擇的名稱、儲存喜歡的名稱。
該程式碼一次產生十個名稱，當用戶滾動時，會產生新一批名稱。

The animated GIF shows how the app works at the completion of part 1.

頁面上方的這個 GIF 可以引導你預覽本 codelab 做完之後的應用效果圖。

{{site.alert.secondary}}

  <h4 class="no_toc">What you’ll learn in part 1</h4>

  <h4 class="no_toc">第一部分，我們將共同學習：</h4>

  * How to write a Flutter app that looks natural on iOS, Android,
    desktop (Windows, for example), and the web.

    如何在 iOS、Android、桌面端和 Web 端建構具有自然體驗的 Flutter 應用。

  * How to use a Flutter app with screen readers (<a href="#talkback">TalkBack</a> and <a href="#talkback">VoiceOver</a>). These  
    technologies enable visually impaired users to get spoken feedback about app contents.

    如果使用螢幕閱讀器來使用 Flutter 應用。這一類螢幕閱讀器技術可以保障視覺障礙使用者“聽”到應用。

  * How to structure a Flutter app.

    如何去設計一個 Flutter 應用的架構

  * How to find and use packages to extend functionality.

    如何查詢和使用 package 來擴充應用功能

  * How to use hot reload for a quicker development cycle.

    如何使用熱重載來加快開發週期

  In [part 2][] of this codelab, you'll add interactivity,
  modify the app's theme, and add the ability to navigate
  to a new screen (called a _route_ in Flutter).
  
  在本 codelab 的 
  [第二部分][part 2]，
  你還將學到新增互動，
  修改應用的主題，
  以及為應用新增一個新的頁面（在 Flutter，我們稱之為 route）。
  
{{site.alert.end}}

{{site.alert.secondary}}

  <h4 class="no_toc">What you'll use</h4>

  <h4 class="no_toc">所需工具</h4>
  
  You need two pieces of software to complete this lab: the
  [Flutter SDK][] and [an editor][].

  你需要安裝兩部分內容來完成本次實驗：
  [Flutter SDK 安裝][Flutter SDK] 和 
  [編輯器 (editor)][an editor] 設定。

  You can run this codelab by using any of the following devices:
  
  你可以透過如下任何裝置完成本 codelab：

  * A physical device ([Android][] or [iOS][]) connected to your
    computer and set to developer mode
    
    開啟開發者模式 (developer mode) 的 [Android][] 和 / 或 [iOS][] 真機；

  * Screen reader enabled on the physical device (<a href="#talkback">TalkBack</a> on Android, <a href="#talkback">VoiceOver</a> on iPhone)

    在物理裝置上啟用了螢幕閱讀器 (Android 上的 <a href="#talkback">TalkBack</a> 和 iPhone 上的 <a href="#talkback">VoiceOver</a>)
    
  * The [iOS simulator][] (requires installing Xcode tools)
    
    [iOS 模擬器][iOS simulator] (需要安裝 Xcode 工具)；
  
  * The [Android emulator][] (requires setup in Android Studio)
    
    [Android 模擬器][Android emulator] (需要安裝設定 Android Studio)。
 
  * A browser (Debugging requires the Chrome browser)

    瀏覽器（如果需要 debug，則需要用 Chrome 瀏覽器）；

  * A [Windows][], [Linux][], or [macOS][] desktop application

    [Windows][]、[Linux][] 或 [macOS][] 桌面端系統。

{{site.alert.end}}

[Android]: {{site.url}}/get-started/install/macos#set-up-your-android-device
[Android emulator]: {{site.url}}/get-started/install/macos#set-up-the-android-emulator
[iOS]: {{site.url}}/get-started/install/macos#deploy-to-ios-devices
[iOS simulator]: {{site.url}}/get-started/install/macos#set-up-the-ios-simulator

Every Flutter app you
create also compiles for the web. In your IDE under
the **devices** pulldown, or at the command line
using `flutter devices`, you should now see **Chrome**
and **Web server** listed. The **Chrome** device
automatically starts Chrome. The **Web server**
starts a server that hosts the app so that you can
load it from any browser. Use the Chrome device during
development so that you can use DevTools,
and the web server when you want to test on
other browsers. For more information,
see [Building a web application with Flutter][]
and [Write your first Flutter app on the web][codelab-web].

任何一個 Flutter 的專案都可以編譯為 web 應用。
你可以在 IDE 中開啟 **devices** 選擇器，
或者是在命令列中輸入 `flutter devices`，
這樣你就可以看到 **Chrome** 以及 **Web server** 選項卡。
**Chrome** 裝置將會自動開啟 Chrome。
**Web server** 則會執行一個服務程式託管應用，這樣你就可以在任意瀏覽器載入它。
在開發時請使用 Chrome 進行除錯，以使用 DevTools。
當你想在其他瀏覽器測試時，請使用 web server。
更多詳細資訊請檢視 [使用 Flutter 建構 web 應用][Building a web application with Flutter]，
以及 [編寫你的第一個 Flutter web 應用程式][codelab-web]。

Also, Flutter apps can compile for desktop.
You should see your operating system listed
in your IDE under **devices**,
for example: **Windows (desktop)**,
or at the command line using `flutter devices`.
For more information on building apps for desktop,
see [Write a Flutter desktop application][].

同時，Flutter 應用也可以編譯到桌面端。
你會在 IDE 的 **裝置** 列表內或執行 `flutter devices`
時看到其列舉了你的作業系統，例如 **Windows (desktop)**，
想了解更多關於建構桌面端應用的資訊，參考
[編寫一個 Flutter 桌面應用][Write a Flutter desktop application]。

## Step 1: Create the starter Flutter app

## 第一步：建立初始化工程

<?code-excerpt path-base="codelabs/startup_namer/step1_base"?>

Create a simple, templated Flutter app, using the instructions in
[Getting Started with your first Flutter app][].
Name the project **startup_namer** (instead of _flutter_app_).

按照 [這個指南][Getting Started with your first Flutter app] 中所描述的步驟，
建立一個簡單的、基於範本的 Flutter 工程，然後將專案命名為 startup_namer (而不是 myapp)，
接下來你將會修改這個工程來完成最終的 App。

{{site.alert.tip}}

  If you don't see "New Flutter Project" as an option in your IDE, make
  sure you have the [plugins installed for Flutter and Dart][].
  
  如果你沒有在你的整合開發環境（IDE）裡看到"New Flutter Project"（新建一個 Flutter 工程），
  請按照本文件完成 [Dart 和 Flutter 外掛的安裝][plugins installed for Flutter and Dart]。
  
{{site.alert.end}}

You'll mostly edit **lib/main.dart**,
where the Dart code lives.

在這個範例中，你將主要編輯 Dart 程式碼所在的 **lib/main.dart** 檔案,

 1. Replace the contents of `lib/main.dart`.<br>
    Delete all of the code from **lib/main.dart**.
    Replace with the following code, which displays
    "Hello World" in the center of the screen.
    
    刪除 lib/main.dart 中的所有程式碼，然後替換為下面的程式碼，它將在螢幕的中心顯示「Hello World」。

    <?code-excerpt "lib/main.dart" title?>
    ```dart
    // Copyright 2018 The Flutter team. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    import 'package:flutter/material.dart';

    void main() {
      runApp(const MyApp());
    }

    class MyApp extends StatelessWidget {
      const MyApp({super.key});

      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          title: 'Welcome to Flutter',
          home: Scaffold(
            appBar: AppBar(
              title: const Text('Welcome to Flutter'),
            ),
            body: const Center(
              child: Text('Hello World'),
            ),
          ),
        );
      }
    }
    ```

    {{site.alert.tip}}

      When pasting code into your app, indentation can become skewed.
      You can fix this with the following formatting tool:

      在向你的工程專案中貼上程式碼的時候，縮排可能會變形。
      你可以使用下面的 Flutter 工具修復此問題：

      * VS Code: Right-click and select **Format Document**.

        VS Code: 右鍵單擊並選擇 Format Document。

      * Android Studio and IntelliJ IDEA: Right-click the code and
        select **Reformat Code with dartfmt**.

        Android Studio 和 IntelliJ IDEA: 右鍵單擊 Dart 程式碼，
        然後選擇 **Reformat Code with dartfmt**。
      * Terminal: Run `flutter format <filename>`.

        Terminal: 執行 `flutter format <filename>`。

    {{site.alert.end}}

 2. Run the app [in the way your IDE describes][].
    You should see either Android, iOS, Windows, Linux, macOS,
    or web output, depending on your chosen device.

    [執行]({{site.url}}/get-started/test-drive#androidstudio) 你的工程專案，
    根據不同的作業系統，你會看到如下執行結果介面：

    {% indent %}
      {% include docs/ios-windows-figure-pair.md image="hello-world.png" alt="Hello world app" %}
    {% endindent %}

    {{site.alert.tip}}

      The first time you run on a physical device,
      it can take a while to load.
      Afterward, you can use hot reload for quick updates.
      **Save** also performs a hot reload if the app is running.
      When running an app directly from the console using
      `flutter run`, enter `r` to perform hot reload.

      第一次真機執行的時候可能會需要更多的等待時間，但是這是值得的，
      因為接下來你就可以使用熱重載（hot reload）功能，
      真正的為下次執行時更新的預覽**節省**更多時間。
      如果你是在終端窗口裡透過 `flutter run` 執行應用的話，
      直接輸入 `r` 進行熱重載。

    {{site.alert.end}}

### Observations
{:.no_toc}

### 觀察和分析
{:.no_toc}

* This example creates a Material app.
  [Material][] is a visual design language
  that is standard on mobile and the web.
  Flutter offers a rich set of Material widgets.  
  It's a good idea to have a `uses-material-design: true` entry
  in the `flutter` section of your `pubspec.yaml` file.
  This allows you to use more features of Material,
  such as its set of predefined [Icons][].

  本範例建立了一個具有 Material Design 風格的應用，
  [Material][] 是一種移動端和網頁端通用的視覺設計語言，
  Flutter 提供了豐富的 Material 風格的 widgets。
  在 `pubspec.yaml` 檔案的 `flutter` 部分選擇加入
  `uses-material-design: true` 會是一個明智之舉，
  透過這個可以讓您使用更多 Material 的特性，
  比如其預定義好的 [圖示][Icons] 集。

* The app extends `StatelessWidget`, which makes the app itself a
  widget. In Flutter, almost everything is a widget,
  including alignment, padding, and layout.

  該應用程式繼承了 `StatelessWidget`，這將會使應用本身也成為一個 widget。
  在 Flutter 中，幾乎所有都是 widget，
  包括對齊 (alignment)、填充 (padding) 和佈局 (layout)。

* The `Scaffold` widget, from the Material library,
  provides a default app bar, and a body property that
  holds the widget tree for the home screen.
  The widget subtree can be quite complex.

  `Scaffold` 是 Material 庫中提供的一個 widget，
  它提供了預設的導航欄、標題和包含主螢幕 widget 樹的 body 屬性。
  widget 樹可以很複雜。

* A widget’s main job is to provide a `build()` method
  that describes how to display the widget in terms of other,
  lower level widgets.

  一個 widget 的主要工作是提供一個 `build()` 
  方法來描述如何根據其他較低級別的 widgets 來顯示自己。

* The body for this example consists of a `Center` widget containing
  a `Text` child widget. The Center widget aligns its widget subtree
  to the center of the screen.

  本範例中的 body 的 widget 樹中包含了一個 `Center` widget，
  Center widget 又包含一個 `Text` 子 widget，
  Center widget 可以將其子 widget 樹對齊到螢幕中心。

{{site.alert.note}}
      The app creates a semantic tree for screen readers. 
      Each node in the tree corresponds to one or several 
      widgets. These nodes can be <a href="https://api.flutter.dev/flutter/widgets/Semantics-class.html">customized</a>
      to tell screen readers how to behave with the node. 
{{site.alert.end}} 

## Step 2: Use an external package

## 第二步：使用外部 package

In this step, you’ll start using an open-source package named
[english_words][], which contains a few thousand of the most used
English words plus some utility functions.

在這一步中，你將開始使用一個名為 [english_words][]
的開源軟體套件，其中包含數千個最常用的英文單詞以及一些實用功能。

You can find the `english_words` package,
as well as many other open source packages, on [pub.dev][].

你可以在 [pub.dev][] 上找到
`english_words` package 以及其他許多開源的 package。

The `pubspec.yaml` file manages the assets and dependencies
for a Flutter app.

`pubspec.yaml` 檔案管理著 Flutter 工程中的所有資源和依賴。

 1. Add the `english_words` package to your project as follows:

    透過下面的方式將 `english_words` 這個 package 加入你的工程裡：

    In your IDE, add the line `english_words: ^4.0.0`
    just after the `cupertino_icons 1.0.4` line 
    and save the file.

    在你的 IDE 中，將 `english_words: ^4.0.0` 加到
    `cupertino_icons 1.0.4` 後面，然後儲存檔案。

    Saving the file causes the dependencies to be
    fetched. The equivalent command-line prompt is
    the following:

    檔案儲存後就會觸發依賴的獲取，這等同於執行下面的命令:

```terminal
$ `flutter pub add english_words`
```

The output will look something like the following:

輸出的結果會類似下面這些:

```terminal
Resolving dependencies...
+ english_words 4.0.0
Downloading english_words 4.0.0...

Changed 1 dependency!
```

Fetching the dependencies also auto-generates the `pubspec.lock`
file with a list of all packages pulled into the project and
their version numbers.

依賴關係的獲取也會自動產生 `pubspec.lock` 檔案，
這個檔案包含所有加入專案的 package 和版本號資訊。

The `pubspec.yaml` file manages the assets and dependencies
for a Flutter app. In `pubspec.yaml`, you will see
that the `english_words` dependency has been added:

`pubspec.yaml` 檔案管理著 Flutter 工程中的所有資源和依賴。
在這個檔案中，現在你會觀察到 `english_words` 這個依賴已經新增:
    
<?code-excerpt path-base="codelabs/startup_namer"?>
<?code-excerpt "{step1_base,step2_use_package}/pubspec.yaml" diff-u="4" from="dependencies" to="sdk"?>
```diff
--- step1_base/pubspec.yaml
+++ step2_use_package/pubspec.yaml
@@ -9,4 +9,5 @@
 dependencies:
   cupertino_icons: ^1.0.5
+  english_words: ^4.0.0
   flutter:
     sdk: flutter
```

 2. While viewing the `pubspec.yaml` file in your IDE's editor view,
    click **Pub get**. This pulls the package into
    your project. The equivalent command-line prompt is as follows:

    在 Android Studio 的編輯器檢視中檢視 `pubspec.yaml` 檔案時，
    點選 **Pub get** 會將相依套件安裝到你的專案。
    你應該會在控制檯中看到以下內容：

```terminal
$ `flutter pub get`
```

The output will look something like the following:

會大約輸出下面類似的內容:

```terminal
Running "flutter pub get" in startup_namer...
Process finished with exit code 0
```

 3. In `lib/main.dart`, import the new package:

    在 `lib/main.dart` 中引入，如下所示：

    <?code-excerpt path-base="codelabs/startup_namer/step2_use_package"?>
    <?code-excerpt "lib/main.dart" title retain="/^import/" replace="/import.*?english.*/[!$&!]/g" indent-by="2"?>
    ```dart
      [!import 'package:english_words/english_words.dart';!]
      import 'package:flutter/material.dart';
    ```

    Your IDE might provide suggestions for libraries to import
    as you type. Some IDEs render the import string in gray to let
    you know that the imported library is unused (so far).

    在你輸入時，Android Studio會為你提供有關庫匯入的建議。
    然後它將呈現灰色的匯入字串，讓你知道匯入的函式庫截至目前尚未被使用。

 4. Use the English words package to generate the text instead of
    using the string "Hello World":

    接下來，我們使用 English words 包產生文字來替換字串"Hello World"：

    <?code-excerpt path-base="codelabs/startup_namer"?>
    <?code-excerpt "{step1_base,step2_use_package}/lib/main.dart" from="class"?>
    ```diff
    --- step1_base/lib/main.dart
    +++ step2_use_package/lib/main.dart
    @@ -2,6 +2,7 @@
     // Use of this source code is governed by a BSD-style license that can be
     // found in the LICENSE file.

    +import 'package:english_words/english_words.dart';
     import 'package:flutter/material.dart';

     void main() {
    @@ -13,14 +14,15 @@

       @override
       Widget build(BuildContext context) {
    +    final wordPair = WordPair.random();
         return MaterialApp(
           title: 'Welcome to Flutter',
           home: Scaffold(
             appBar: AppBar(
               title: const Text('Welcome to Flutter'),
             ),
    -        body: const Center(
    -          child: Text('Hello World'),
    +        body: Center(
    +          child: Text(wordPair.asPascalCase),
             ),
           ),
         );
    ```

    {{site.alert.note}}

      "Pascal case" (also known as "upper camel case"),
      means that each word in the string, including the first one,
      begins with an uppercase letter. So, "uppercamelcase" becomes
      "UpperCamelCase".

      「大駝峰式命名法」也稱為 upper camel case 或 Pascal case，
      表示字串中的每個單詞（包括第一個單詞）都以大寫字母開頭。
      所以，uppercamelcase 會變成 UpperCamelCase。

    {{site.alert.end}}

    {{site.alert.important}}
    Using upper camel case also helps screen readers identify the individual words in the compound word and provides a better 
    experience to visually impaired users.
    {{site.alert.end}}

 5. If the app is running, [hot reload][]
    to update the running app. Each time you click hot reload,
    or save the project, you should see a different word pair,
    chosen at random, in the running app.
    This is because the word pairing is generated inside the build
    method, which is run each time the `MaterialApp` requires rendering,
    or when toggling the Platform in Flutter Inspector.

    如果應用程式正在執行，請使用熱重載按鈕 <i class="material-icons align-bottom">offline_bolt</i>
    更新正在執行的應用程式。每次單擊熱重載或儲存專案時，
    都會在正在執行的應用程式中隨機選擇不同的單詞對。
    這是因為單詞對是在 build 方法內部產生的。
    每次 `MaterialApp` 需要渲染時或者在 Flutter Inspector
    中切換平台時 build 都會執行。

    {% indent %}
      {% include docs/ios-windows-figure-pair.md image="step2.png" alt="App at completion of second step" %}
    {% endindent %}

    **You can try the app with a screen reader.
    To turn on the screen reader on your device, complete the following steps:** 

    {% comment %} Nav tabs {% endcomment -%}
    <ul class="nav nav-tabs" id="editor-setup" role="tablist">
      <li class="nav-item">
        <a class="nav-link active" id="talkback-tab" href="#talkback" role="tab" aria-controls="talkback" aria-selected="true">TalkBack on Android</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" id="voiceover-tab" href="#voiceover" role="tab" aria-controls="voiceover" aria-selected="false">VoiceOver on iPhone</a>
      </li>
    </ul>

    {% comment %} Tab panes {% endcomment -%}
    <div class="tab-content">
    
    <div class="tab-pane active" id="talkback" role="tabpanel" aria-labelledby="talkback-tab" markdown="1">

    <!-- **Turn on TalkBack on your Android phone to try the app with a screen reader** -->

    1. On your device, open Settings. 
    2. Select Accessibility and then TalkBack.
    3. Turn 'Use TalkBack' on or off.
    4. Select Ok.

    To learn how to find and customize Android's accessibility features, view this video.

    <iframe width="560" height="315" src="{{site.youtube-site}}/embed/FQyj_XTl01w" title="Customize accessibility features on Pixel" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
    </iframe>

    </div>

    <div class="tab-pane" id="voiceover" role="tabpanel" aria-labelledby="voiceover-tab" markdown="1">


    <!-- **Turn on VoiceOver on your iPhone to try the app with a screen reader** -->

    1. On your device, open **Settings > Accessibility > VoiceOver**
    2. Turn the VoiceOver setting on or off    

    To learn how to find and customize iOS accessibility features, view this video.


    <iframe width="560" height="315" src="{{site.youtube-site}}/embed/qDm7GiKra28" title="How to navigate your iPhone or iPad with VoiceOver" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
    </iframe>

    </div>
    </div>{% comment %} End: Tab panes. {% endcomment -%}  

### Problems?
{:.no_toc}

### 遇到問題?
{:.no_toc}

If your app is not running correctly, look for typos.
If you want to try some of Flutter's debugging tools,
check out the [DevTools][] suite of debugging and profiling tools.
If needed, use the code at the following links to get back on track.

如果你的應用程式執行不正常，請查詢是否有拼寫錯誤。
如果需要透過 Flutter 的 debug 工具，可以檢視 [開發者工具][DevTools] 頁面
來檢視 debug 和 profile 的工具。
如果需要，使用下面連結中的程式碼來對比更正。

* [pubspec.yaml]({{code-url}}/startup_namer/step2_use_package/pubspec.yaml)
* [lib/main.dart]({{code-url}}/startup_namer/step2_use_package/lib/main.dart)

## Step 3: Add a Stateful widget

## 第三步：新增一個 Stateful widget

<?code-excerpt path-base="codelabs/startup_namer/step3_stateful_widget"?>

State<em>less</em> widgets are immutable, meaning that their
properties can’t change&mdash;all values are final.

**無** 狀態的 widgets 是不可變的，這意味著它們的屬性不能改變
—— 所有的值都是 final。

State<em>ful</em> widgets maintain state that might change
during the lifetime of the widget. Implementing a stateful
widget requires at least two classes: 1) a `StatefulWidget` class
that creates an instance of 2) a `State` class. The `StatefulWidget`
class is, itself, immutable and can be thrown away and regenerated,
but the `State` class persists over the lifetime of the widget.

**有** 狀態的 widgets 也是不可變的，
但其持有的狀態可能在 widget 生命週期中發生變化，
實現一個有狀態的 widget 至少需要兩個類：
1）一個 `StatefulWidget` 類；2）一個 `State` 類，`StatefulWidget` 類本身是不變的，
但是 `State` 類在 widget 生命週期中始終存在。

In this step, you’ll add a stateful widget, `RandomWords`,
which creates its `State` class, `_RandomWordsState`.
You'll then use `RandomWords` as
a child inside the existing `MyApp` stateless widget.

在這一步，你將新增一個有狀態的 widget —— `RandomWords`，
它會建立自己的狀態類 —— `_RandomWordsState`，
然後你需要將 `RandomWords`
內嵌到已有的無狀態的 `MyApp` widget。

<ol markdown="1">
<li markdown="1"><p markdown="1">Create the boilerplate code for a stateful widget.<br>
  In `lib/main.dart`, position your cursor after all of the code,
  enter **Return** a couple times to start on a fresh line.
  In your IDE, start typing `stful`.
  The editor asks if you want to create a
  `Stateful` widget. Press **Return** to accept.
  The boilerplate code for two classes appears,
  and the cursor is positioned for you to enter the name of
  your stateful widget.</p>
  <p markdown="1">建立有狀態 widget 的樣板程式碼。<br>
  在 `lib/main.dart` 中，將游標置於所有程式碼之後，
  輸入 **Enter** 幾次另起新行。
  在 IDE 中，輸入 `stful`，編輯器就會提示您是否要建立一個 `Stateful` widget。
  按回車鍵表示接受建議，隨後就會出現兩個類別的樣板程式碼，
  游標也會被定位在輸入有狀態 widget 的名稱處。</p>
</li>

<li markdown="1"> <p markdown="1">Enter `RandomWords` as the name of your widget.<br>
  The `RandomWords` widget does little else beside creating its
  `State` class.<br><br>
  Once you've entered `RandomWords` as the name of
  the stateful widget, the IDE automatically updates
  the accompanying `State` class, naming it `_RandomWordsState`.
  By default, the name of the `State` class is prefixed
  with an underbar. Prefixing an identifier with an
  underscore [enforces privacy][] in the Dart language and
  is a recommended best practice for `State` objects.<br><br>
  The IDE also automatically updates the state class
  to extend `State<RandomWords>`, indicating
  that you're using a generic [`State`][]
  class specialized for use with `RandomWords`.
  Most of the app's logic resides here&mdash;it maintains
  the state for the `RandomWords` widget. This class saves the list
  of generated word pairs, which grows infinitely
  as the user scrolls and, in part 2 of this lab,
  favorites word pairs as the user adds or removes them from
  the list by toggling the heart icon.<br><br>
  Both classes now look as follows:</p>
  <p markdown="1">輸入 `RandomWords` 作為有狀態 widget 的名稱。<br>
  `RandomWords` widget 的主要作用就是建立其對應的 `State` 類別。<br><br>
  輸入 `RandomWords` 作為有有狀態 widget 的名稱後，
  IDE 會自動更新其對應的 `State` 類，並將其命名為 `_RandomWordsState`。
  預設情況下，`State` 類別的名稱帶有下劃線字首。
  Dart 語言中，給識別符號加上下劃線字首可以 [增強隱私性][enforces privacy]，
  並且這也是針對 `State` 物件推薦的最佳實踐寫法。<br><br>
  IDE 也會自動將狀態類繼承自 `State<RandomWords>`，
  這表示專門用於 `RandomWords` 的通用 [`State`][] 類別。
  該應用程式的大多數邏輯都位於此處&mdash;它維護 `RandomWords` widget 的狀態。
  該類會儲存產生的單詞對的列表，該列表隨使用者滾動而無限增長，
  在本實驗的第 2 部分中，使用者可以透過點選心形圖示，新增或刪除列表中收藏的單詞對。<br><br>
  這兩個類現在都如下所示：</p>

<?code-excerpt "lib/main.dart (RandomWordsWidget)" replace="/final wordPair = WordPair.random\(\);\n *return Text\(wordPair.asPascalCase\);/return Container();/g" indent-by="2"?>
  ```dart
  class RandomWords extends StatefulWidget {
    const RandomWords({super.key});

    @override
    State<RandomWords> createState() => _RandomWordsState();
  }

  class _RandomWordsState extends State<RandomWords> {
    @override
    Widget build(BuildContext context) {
      return Container();
    }
  }
  ```
</li>

<li markdown="1"> <p markdown="1">Update the `build()` method in `_RandomWordsState`:</p>
<p markdown="1">更新 `_RandomWordsState` 中的 `build()` 方法：</p>
  <?code-excerpt "lib/main.dart (_RandomWordsState)" title replace="/(\n  )(.*)/$1[!$2!]/g"?>
  ```dart
  class _RandomWordsState extends State<RandomWords> {
    [!@override!]
    [!Widget build(BuildContext context) {!]
    [!  final wordPair = WordPair.random();!]
    [!  return Text(wordPair.asPascalCase);!]
    [!}!]
  }
  ```
</li>

<li markdown="1"> <p markdown="1">Remove the word generation code from `MyApp`
  by making the changes shown in the following diff:</p>
  <p markdown="1">透過以下差異所示的更改，刪除 `MyApp` 中單詞產生的程式碼：</p>

  <?code-excerpt path-base="codelabs/startup_namer"?>
  <?code-excerpt "{step2_use_package,step3_stateful_widget}/lib/main.dart" to="}"?>
  ```diff
  --- step2_use_package/lib/main.dart
  +++ step3_stateful_widget/lib/main.dart
  @@ -14,16 +14,15 @@

     @override
     Widget build(BuildContext context) {
  -    final wordPair = WordPair.random();
       return MaterialApp(
         title: 'Welcome to Flutter',
         home: Scaffold(
           appBar: AppBar(
             title: const Text('Welcome to Flutter'),
           ),
  -        body: Center(
  -          child: Text(wordPair.asPascalCase),
  +        body: const Center(
  +          child: RandomWords(),
           ),
         ),
       );
     }
  ```

<li markdown="1"> <p markdown="1">Restart the app.
  The app should behave as before, displaying a word
  pairing each time you hot reload or save the app.</p>
  <p markdown="1">重啟應用。
    應用應該像之前一樣執行，每次熱重載或儲存應用程式時都會顯示一個單詞對。</p>
</li>


{{site.alert.tip}}

  If you see a warning on a hot reload that you might
  need to restart the app, consider restarting it.
  The warning might be a false positive, but
  restarting your app ensures that
  your changes are reflected in the app's UI.

  如果在熱重載時看到需要您重啟應用程式的警告，請考慮重新啟動。
  該警告可能是誤報，但重新啟動您的應用程式可確保您的更改真實反映在應用程式的 UI 中。

{{site.alert.end}}

### Problems?
{:.no_toc}

### 遇到問題？
{:.no_toc}

If your app is not running correctly, look for typos.
If you want to try some of Flutter's debugging tools,
check out the [DevTools][] suite of debugging and profiling tools.
If needed, use the code at the following link to get back on track.

如果你的應用程式執行不正常，請查詢是否有拼寫錯誤。
如果需要透過 Flutter 的 debug 工具，可以檢視 [開發者工具][DevTools] 頁面
來檢視 debug 和 profile 的工具。
如果需要，使用下面連結中的程式碼來對比更正。

* [lib/main.dart]({{code-url}}/startup_namer/step3_stateful_widget/lib/main.dart)

## Step 4: Create an infinite scrolling ListView

## 第四步：建立一個無限滾動的 ListView

<?code-excerpt path-base="codelabs/startup_namer/step4_infinite_list"?>

In this step, you'll expand `_RandomWordsState` to generate
and display a list of word pairings. As the user scrolls the list
(displayed in a `ListView` widget) grows infinitely. `ListView`'s
`builder` factory constructor allows you to build a list view
lazily, on demand.

在該步驟中，您會拓展 `_RandomWordsState` 以產生並顯示單詞對列表。
隨著使用者滾動，列表（顯示在 `ListView` widget 中）將無限增長。
`ListView` 的 `builder` 工廠建構函式使您可以按需延遲建構列表檢視。

 1. Add a `_suggestions` list to the `_RandomWordsState`
    class for saving suggested word pairings. Also,
    add a `_biggerFont` variable for making the font size larger.

    向 `_RandomWordsState` 類中新增一個 `_suggestions` 列表以儲存建議的單詞對，
    同時，新增一個 `_biggerFont` 變數來增大字型大小。

    <?code-excerpt "lib/main.dart" title region="RWS-var" indent-by="2" replace="/final .*/[!$&!]/g"?>
    ```dart
      class _RandomWordsState extends State<RandomWords> {
        [!final _suggestions = <WordPair>[];!]
        [!final _biggerFont = const TextStyle(fontSize: 18);!]
        // ···
      }
    ```

 2. Next, you'll add a `ListView` widget to the
    `_RandomWordsState` class with the `ListView.builder` constructor.
    This method creates the `ListView` that displays the suggested word pairing.

    接下來，我們將向 `_RandomWordsState` 類新增一個 `_buildSuggestions()` 方法，
    此方法建構顯示建議單詞對的 `ListView`。

    The `ListView` class provides a builder property, `itemBuilder`,
    that's a factory builder and callback function specified as an
    anonymous function. Two parameters are passed to the
    function&mdash;the `BuildContext`, and the row iterator, `i`.
    The iterator begins at 0 and increments each time the function
    is called. It increments twice for every suggested word pairing:
    once for the ListTile, and once for the Divider.
    This model allows the suggested list to continue growing
    as the user scrolls.

    `ListView` 類提供了一個名為 `itemBuilder` 的 builder 屬性，這是一個工廠匿名回呼(Callback)函式，
    接受兩個引數 `BuildContext` 和行迭代器 `i`。迭代器從 0 開始，每呼叫一次該函式 `i` 就會自增，
    每次建議的單詞對都會讓其遞增兩次，一次是 ListTile，另一次是 Divider。
    它用於建立一個在使用者滾動時候無限增長的列表。

 2. Return a `ListView` widget from the `build` method
    of the `_RandomWordsState` class using the `ListView.builder` constructor:
 
    使用 `ListView.builder` 建構函式，從 `_RandomWordsState`
    類別的 `build` 方法中返回一個 `ListView` widget。

    <?code-excerpt "lib/main.dart (itemBuilder)" title replace="/ListTile([\S\s]*?)\);/Text(_suggestions[index].asPascalCase);/g" indent-by="2"?>
    ```dart
      return ListView.builder(
        padding: const EdgeInsets.all(16.0),
        itemBuilder: /*1*/ (context, i) {
          if (i.isOdd) return const Divider(); /*2*/

          final index = i ~/ 2; /*3*/
          if (index >= _suggestions.length) {
            _suggestions.addAll(generateWordPairs().take(10)); /*4*/
          }
          return Text(_suggestions[index].asPascalCase);
        },
      );
    ```

    {:.numbered-code-notes}
     1. The `itemBuilder` callback is called once per row,
        and places each suggestion into a `ListTile` row. For even rows, the
        function adds a `ListTile` row for the word pairing. For odd rows, the
        function adds a `Divider` widget to visually separate the entries. Note
        that the divider might be difficult to see on smaller devices.

        對於每個建議的單詞對都會呼叫一次 `itemBuilder`，然後將單詞對新增到 `ListTile` 行中。
        在偶數行，該函式會為單詞對新增一個 `ListTile` row，
        在奇數行，該函式會新增一個分割線的 widget，來分隔相鄰的詞對。
        注意，在小螢幕上，分割線可能較難辨別。

     2. Add a one-pixel-high divider widget before each row in the `ListView`.

        在 `ListView` 裡的每一行之前，新增一個 1 畫素高的分隔線 widget。

     3. The expression `i ~/ 2` divides `i` by 2 and returns an integer result.
        For example: 1, 2, 3, 4, 5 becomes 0, 1, 1, 2, 2. This calculates the
        actual number of word pairings in the `ListView`, minus the divider
        widgets.

        語法 `i ~/ 2` 表示 `i` 除以 2，但返回值是整型（向下取整），
        比如 i 為：1, 2, 3, 4, 5 時，結果為 0, 1, 1, 2, 2，
        這個可以計算出 `ListView` 中減去分隔線後的實際單詞對數量。

     4. If you've reached the end of the available word pairings,
        then generate 10 more and add them to the suggestions list.

        如果是建議列表中最後一個單詞對，接著再產生 10 個單詞對，然後新增到建議列表。

    The `ListView.builder` constructor creates and displays
    a `Text` widget once per word pairing.
    In the next step, you'll instead return each new pair as a `ListTile`,
    which allows you to make the rows more attractive in the next step.

    `ListView.builder` 的建構函式會為每個單詞對建立並顯示一個 `Text` widget，
    下一步裡，你將可以為每個單詞對返回一個 `ListTile` widget，這可以讓每一行的顯示更漂亮。

 3. Replace the returned `Text` in the `itemBuilder` body
    of the `ListView.builder` in `_RandomWordsState`
    with a `ListTile` displaying the suggestion:

    在 `_RandomWordsState` 類別的 `ListView.builder` 裡的
    `itemBuilder` 屬性體裡，將 `Text` 替換為 `ListTile` widget：

    <?code-excerpt "lib/main.dart (listTile)" title indent-by="2"?>
    ```dart
      return ListTile(
        title: Text(
          _suggestions[index].asPascalCase,
          style: _biggerFont,
        ),
      );
    ```

    A `ListTile` is a fixed height row that contains text
    as well as leading or trailing icons or other widgets.

    `ListTile` 是包含了文字以及前後點陣圖標 widget 的一行。

 4. Once complete, the `build()` method in the `_RandomWordsState` class
    should match the following highlighted code:

    更新 `_RandomWordsState` 的 `build()` 方法以使用 _buildSuggestions()，
    而不是直接呼叫單詞產生庫，程式碼更改後如下：

    <?code-excerpt "lib/main.dart (build)" title region="RWS-build" replace="/(\n  )(return.*|  .*|\);)/$1[!$2!]/g" indent-by="2"?>
    ```dart
      @override
      Widget build(BuildContext context) {
        [!return ListView.builder(!]
        [!  padding: const EdgeInsets.all(16.0),!]
        [!  itemBuilder: /*1*/ (context, i) {!]
        [!    if (i.isOdd) return const Divider(); /*2*/!]

        [!    final index = i ~/ 2; /*3*/!]
        [!    if (index >= _suggestions.length) {!]
        [!      _suggestions.addAll(generateWordPairs().take(10)); /*4*/!]
        [!    }!]
        [!    return ListTile(!]
        [!      title: Text(!]
        [!        _suggestions[index].asPascalCase,!]
        [!        style: _biggerFont,!]
        [!      ),!]
        [!    );!]
        [!  },!]
        [!);!]
      }
    ```

 5. To put it all together, update the displayed title of the app
    by updating the `build()` method in the `MyApp` class
    and changing the title of the `AppBar`:

    更新 `MyApp` 的 `build()` 方法，修改 `title` 的值來改變標題，
    修改 `home` 的值為 `RandomWords` widget。

    <?code-excerpt path-base="codelabs/startup_namer"?>
    <?code-excerpt "{step3_stateful_widget,step4_infinite_list}/lib/main.dart" diff-u="4" from="class MyApp" to="}"?>
    ```diff
    --- step3_stateful_widget/lib/main.dart
    +++ step4_infinite_list/lib/main.dart
    @@ -14,12 +14,12 @@

       @override
       Widget build(BuildContext context) {
         return MaterialApp(
    -      title: 'Welcome to Flutter',
    +      title: 'Startup Name Generator',
           home: Scaffold(
             appBar: AppBar(
    -          title: const Text('Welcome to Flutter'),
    +          title: const Text('Startup Name Generator'),
             ),
             body: const Center(
               child: RandomWords(),
             ),
    @@ -27,18 +27,36 @@
         );
       }
     }

    -class RandomWords extends StatefulWidget {
    -  const RandomWords({super.key});
    +class _RandomWordsState extends State<RandomWords> {
    +  final _suggestions = <WordPair>[];
    +  final _biggerFont = const TextStyle(fontSize: 18);

       @override
    -  State<RandomWords> createState() => _RandomWordsState();
    +  Widget build(BuildContext context) {
    +    return ListView.builder(
    +      padding: const EdgeInsets.all(16.0),
    +      itemBuilder: /*1*/ (context, i) {
    +        if (i.isOdd) return const Divider(); /*2*/
    +
    +        final index = i ~/ 2; /*3*/
    +        if (index >= _suggestions.length) {
    +          _suggestions.addAll(generateWordPairs().take(10)); /*4*/
    +        }
    +        return ListTile(
    +          title: Text(
    +            _suggestions[index].asPascalCase,
    +            style: _biggerFont,
    +          ),
    +        );
    +      },
    +    );
    +  }
     }

    -class _RandomWordsState extends State<RandomWords> {
    +class RandomWords extends StatefulWidget {
    +  const RandomWords({super.key});
    +
       @override
    -  Widget build(BuildContext context) {
    -    final wordPair = WordPair.random();
    -    return Text(wordPair.asPascalCase);
    -  }
    +  State<RandomWords> createState() => _RandomWordsState();
     }
    ```

 6. Restart the app. You should see a list of word pairings no matter how far
    you scroll.

    重新啟動你的專案工程應用，你應該看到一個單詞對列表。
    儘可能地向下滾動，你將繼續看到新的單詞對。

    {% indent %}
      {% include docs/ios-windows-figure-pair.md image="step4-infinite-list.png" alt="App at completion of fourth step" %}
    {% endindent %}

### Problems?
{:.no_toc}

### 遇到問題？
{:.no_toc}

If your app is not running correctly, look for typos.
If you want to try some of Flutter's debugging tools,
check out the [DevTools][] suite of debugging and profiling tools.
If needed, use the code at the following link to get back on track.

如果你的應用程式執行不正常，請查詢是否有拼寫錯誤。
如果需要透過 Flutter 的 debug 工具，可以檢視 [開發者工具][DevTools] 頁面
來檢視 debug 和 profile 的工具。
如果需要，使用下面連結中的程式碼來對比更正。

* [lib/main.dart]({{code-url}}/startup_namer/step4_infinite_list/lib/main.dart)

{% include docs/run-profile.md %}

## Next steps
{:.no_toc}

## 下一步
{:.no_toc}

<container class="site-image-right">
<iframe width="auto" height="502" src="{{site.youtube-site}}/embed/TSb8fjmKY1I" title="FOO040 talkback flutter app draft2" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  The app from part 2 on TalkBack screen reader
</container>

{% include docs/app-figure.md class="site-image-right" img-class="border"
    image="get-started/startup-namer.gif" caption="The app from part 2" %}

Congratulations!

祝賀你！

You've written an interactive Flutter app that runs on iOS, Android, Windows and web.
In this codelab, you've:

你已經完成了一個可以同時執行在
iOS、Android、Windows 和 Web 平台的 Flutter 應用！
同時學習瞭如下內容：

* Created a Flutter app from the ground up.

  從零開始建立了一個 Flutter 應用；

* Written Dart code.

  編寫 Dart 程式碼；

* Leveraged an external, third-party library.

  使用外部的第三方庫（package）；

* Used hot reload for a faster development cycle.

  在開發過程中試用了熱重載 (hot reload)；

* Implemented a stateful widget.

  實現了一個有狀態的 widget；

* Created a lazily loaded, infinite scrolling list.

  建立了一個延遲載入的，無限滾動的列表。

* Learned about basic accessibility terms such as screen readers.

  瞭解基本的輔助功能術語，例如螢幕閱讀器。

If you would like to extend this app, proceed to
[part 2][] on the
[Google Developers Codelabs][] site,
where you add the following functionality:

如果你想繼續擴充你的應用，在這裡進行 
[第二部分](https://codelabs.flutter-io.cn/codelabs/first-flutter-app-pt2-cn/index.html)，
你將會從以下方面修改你的應用：

* Implement interactivity by adding a clickable heart icon to save
  favorite pairings.

  為應用新增互動功能，一個能點選的小心心，來儲存喜歡的公司名字；

* Implement navigation to a new route by adding a new screen
  containing the saved favorites.

  為應用新增一個新的頁面（Route），檢視收藏列表；

* Modify the theme color, making an all-white app.

  修改應用的主題，變成一個白色系的應用。

## Explore the Flutter SDK

## 瞭解 Flutter SDK 的更多內容

* <a href="https://docs.flutter.dev/get-started/flutter-for/react-native-devs">Flutter for React Native developers</a>
* <a href="https://docs.flutter.dev/development/accessibility-and-localization/accessibility//#testing-accessibility-on-mobile">Testing accessibility in Flutter mobile apps</a>
* <a href="https://docs.flutter.dev/development/ui/layout">Building layouts with Flutter</a>
* <a href="https://flutter.dev/docs/development/ui/widgets-intro">Introduction to widgets</a>

[an editor]: {{site.url}}/get-started/editor
[Building a web application with Flutter]: {{site.url}}/web
[DevTools]: {{site.url}}/development/tools/devtools
[enabled web]: {{site.url}}/get-started/web
[enforces privacy]: {{site.dart-site}}/guides/language/language-tour#libraries-and-visibility
[english_words]: {{site.pub}}/packages/english_words
[Flutter SDK]: {{site.url}}/get-started/install
[Getting Started with your first Flutter app]: {{site.url}}/get-started/test-drive#create-app
[Google Developers Codelabs]: {{site.codelabs}}
[hot reload]: {{site.url}}/get-started/test-drive
[in the way your IDE describes]: {{site.url}}/get-started/test-drive
[Icons]: https://fonts.google.com/icons
[Material]: {{site.material}}/guidelines
[Part 1]: {{site.codelabs}}/codelabs/first-flutter-app-pt1
[part 2]: {{site.codelabs}}/codelabs/first-flutter-app-pt2
[plugins installed for Flutter and Dart]: {{site.url}}/get-started/editor
[pub.dev]: {{site.pub}}
[`Scaffold`]: {{site.api}}/flutter/material/Scaffold-class.html
[`State`]: {{site.api}}/flutter/widgets/State-class.html
[codelab-web]: {{site.url}}/get-started/codelab-web
[Windows]: {{site.url}}/get-started/install/windows#windows-setup
[Linux]: {{site.url}}/get-started/install/linux#linux-setup
[macOS]: {{site.url}}/get-started/install/macos#macos-setup
[Write a Flutter desktop application]: {{site.codelabs}}/codelabs/flutter-github-client
