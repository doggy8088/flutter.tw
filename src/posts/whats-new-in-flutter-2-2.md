---
title: Flutter 2.2 更新詳解
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/05/fHLvPr.jpg)

[Flutter 2.2 版](https://flutter.cn/posts/announcing-flutter-2-2?t=1)已正式釋出！要獲取新版本，您只需切換到 stable 渠道並更新目前安裝的 Flutter，或前往 [flutter.cn/docs/get-started](https://flutter.tw/get-started) 從頭開始安裝。

雖然與 2.0 版只相隔數月，2.2 版還是滿載大量改進。此版本共計納入了框架、引擎和外掛庫等方面的 2,456 個 PR 並解決了 3,105 個問題。我們要特此鳴謝為此版本貢獻了大量 PR 和 PR review 的整個 Flutter 社群，包括 PR 貢獻最多 (17) 的 [Abhishek01039](https://github.com/Abhishek01039) 和 PR review 貢獻最多 (9) 的 [xu-baolin](https://github.com/xu-baolin)。衷心感謝所有開發者對 Flutter 2.2 順利釋出穩定版做出的貢獻。沒有大家的支援，我們將無法做到。

每個新的 Flutter 穩定版都會帶來諸如效能提升、新功能、bug 修復等一系列變化，並會提供一些尚未試用於生產環境的試驗性功能，希望您能幫助我們驗證這些功能能否正常工作並滿足您的需求。此外，新版本還會包含一系列相關工具的更新和來自 Flutter 社群的更新。坦白講，如今 Flutter 每個新版本的內容都非常豐富，不可能在一篇文章中鉅細無遺地詳述，因此下面我們將著重為您介紹一些主要亮點。

## **Flutter 2.2 穩定版更新**

此版本在 Flutter 2 的基礎上做了諸多改進，其中不但有面向 Android、iOS 和 web 平台的更新，還有新的 Material 圖示、文字處理方式的改進、捲軸行為的變化、TextSpan widget 的滑鼠游標支援，以及用一份程式碼適配多個平台方面的新指南。這些功能皆已釋出穩定版，可供您在正式版應用中使用。同時，所有這些功能都是在 [新版 Dart](https://flutter.cn/posts/announcing-dart-2-13) 的基礎上建構而成。

**Dart 2.13**

隨 Flutter 2.2 版一起釋出的還有 [Dart 2.13 版](https://flutter.cn/posts/announcing-dart-2-13)。此 Dart 版本包含眾多新功能，其中之一是新的類型別名，該功能讓您可為型別建立別名，就像為函式建立別名一樣:

```Dart
// Type alias for functions (existing)
typedef ValueChanged<T> = void Function(T value);

// Type alias for classes (new!)
typedef StringList = List<String>;

// Rename classes in a non-breaking way (new!)
@Deprecated("Use NewClassName instead")
typedef OldClassName<T> = NewClassName<T>;
```

有了類型別名，您可為長而複雜的型別賦予簡短易懂的名稱，並以不會破壞程式碼的方式重新命名您的類別。Dart 2.13 版還包含其他多項更新，您可以在我們之前的推文 [Dart 2.13 版本釋出](https://flutter.cn/posts/announcing-dart-2-13) 中瞭解詳情。

**Flutter web 更新**

作為 Flutter 穩定版最新支援的平台，web 平台在此版本也有多項改進。

首先，我們藉助新的 service worker 載入機制優化了快取行為，並修復了 `main.dart.js` 的重複下載問題。在舊版 Flutter web 中，service worker 會在後臺下載應用更新，使用者在此期間可照常使用應用的舊版本。更新下載完畢後，使用者要多次重新整理瀏覽器頁面後才會看到相應更新。在 Flutter 2.2 版中，當新的 service worker 檢測到更新後，使用者需要先等待更新下載完畢才能使用應用，但屆時他們無需再次手動重新整理頁面即可看到更新內容。

要啟用此項變更，您需要重新產生您的 Flutter 應用的 `index.html`。具體來說，請儲存您的修改、刪除 `index.html` 檔案，然後在專案目錄中執行 `flutter create` . 以重新產生該檔案。

我們還對兩個 web 渲染器都做了改進。在 HTML 渲染器上，我們添加了對 [字型特性](https://developer.mozilla.org/en-US/docs/Web/CSS/font-feature-settings) 的支援，以啟用 [`FontFeature`](https://api.flutter.dev/flutter/dart-ui/FontFeature-class.html) 設定並使用 canvas API 渲染文字，從而使滑鼠懸停處的文字能夠顯示在適當的位置。在 HTML 和 CanvasKit 渲染器上，我們新增了對著色器遮罩 (shader masks) 和 [`computeLineMetrics`](https://api.flutter.dev/flutter/painting/TextPainter/computeLineMetrics.html) 的支援，以解決 Flutter web 應用和移動應用兩者不一致的問題。例如，開發者現在可以透過 [不透明度遮罩](https://api.flutter.dev/flutter/widgets/Opacity-class.html) 使用著色器遮罩實現淡出轉場，並像在移動應用中一樣使用 `computeLineMetrics`。

無障礙功能不但是 Flutter web 的一大重點，也是整個 Flutter 的一個重心所在。按照設計，是透過建構 `SemanticsNode` 樹來實現無障礙功能。Flutter web 應用的使用者啟用無障礙功能後，我們會產生一個與 `RenderObject` DOM 樹並行的 DOM 樹，並將語義屬性轉換為 Aira。在此版本中，我們改進了語義節點位置，消除了移動應用和 web 應用在使用轉換 (transform) 時的不一致，這意味著在使用轉換對 widget 進行樣式設定時，焦點框會正確地顯示在元素上方。如需直觀瞭解其實際效果，您可以觀看 Material Design 無障礙專案負責人 Victor Tsaran 的影片，瞭解他如何對 [Flutter Gallery App 使用 VoiceOver](https://devrel.andfun.cn/devrel/posts/2021/05/yFhscX.mp4)。

<video controls width="640px" height="480px" src="https://devrel.andfun.cn/devrel/posts/2021/05/yFhscX.mp4"></video>

我們現在還提供一個適用於效能分析 (profile) 和釋出 (release) 模式的命令列 flag，以供開發者存取語義節點除錯樹，並直觀檢視系統為其 web 應用建立的語義節點，從而對應用的無障礙功能進行除錯。

要為您的 Flutter web 應用啟用此功能，請執行以下命令:

```
$ flutter run -d chrome --profile \
  --dart-define=FLUTTER_WEB_DEBUG_SHOW_SEMANTICS=true
```

啟用該 flag 後，您將能夠在各個 widget 上方看到您的語義節點，從而進行除錯並檢查語義元素的位置是否符合預期。如果您發現這類問題，歡迎向我們 [提交 bug 報告](https://goo.gle/flutter_web_issue)。

雖然在核心無障礙功能的支援方面已經取得了長足進步，但我們仍在這個領域不斷改進。在 [2.2 穩定版](https://flutter.cn/posts/announcing-flutter-2-2?t=1) 之後的 master 和 dev 渠道 build 中，我們新增了一個 API，供開發者以程式設計方式為其應用 [自動啟用無障礙功能](https://github.com/flutter/engine/pull/25830)，同時我們正在著手解決 [在螢幕閱讀器中使用 Tab 鍵](https://github.com/flutter/engine/pull/25797) 的問題。

最後但也同樣重要的是，最新版的 Flutter DevTools 現在還支援為您的 Flutter web 應用使用佈局瀏覽器。

![](https://devrel.andfun.cn/devrel/posts/2021/05/5Ux1TW.png)

您現在可以在 web 應用中使用您所熟悉的佈局除錯工具，這與移動應用和桌面應用別無二致。

**iOS 頁面轉場和增量式安裝**

對於 iOS 平台，此版本將動畫幀的渲染時間縮減了 75%，[使 Cupertino 主題中頁面轉場更流暢](https://github.com/flutter/flutter/pull/75670)。在低端手機上的縮減比例甚至可能更高。我們不僅改善了終端使用者可感知到的效能，還一直在想方設法提升開發效能。

在此版本中，我們實現了開發過程中的 [增量式 iOS 安裝](https://github.com/flutter/flutter/pull/77756)。基準測試結果顯示，iOS 應用的更新版本安裝時間縮短了 40%，這也就縮短了您的應用變更測試周期。

**使用 Flutter 建構平台自適應應用**

隨著 Flutter 穩定版逐步支援更多平臺，您在設計應用時不僅需要考慮相容不同的裝置型別 (如手機、平板電腦和桌面電腦)，還需要考慮支援不同的輸入方式 (觸控與鍵鼠)，以及適配各個平臺上的不同使用習慣 (例如在導航時是使用抽屜式導航欄還是系統選單)。如果應用能夠根據不同目標平台的細節差異做出相應調整，我們就稱之為平台自適應應用。

如果您想初步瞭解在建構平台自適應應用時要考慮哪些因素，請觀看 Kevin Moore 關於 "[建構平台自適應應用](https://events.google.com/io/session/868dfd56-7f8c-49ee-84ad-ac69a23ba19d?lng=en)" 的影片。如需詳細瞭解，您可以閱讀 [Flutter 文件中關於平台自適應應用的指南](https://flutter.tw/development/ui/layout/building-adaptive-apps)。

最後，如需參考遵循這些指南編寫出的多平臺應用範例。我們建議您看看 gSkinner 打造的 [Flokk](https://flutter.gskinner.com/flokk) 和 [Flutter Folio](https://flutter.gskinner.com/folio) 應用。您既可以下載 [Flokk](https://github.com/gskinnerTeam/flokk) 和 [Folio](https://github.com/gskinnerTeam/flutter-folio) 的原始碼，也可以從各個應用商店下載 [Flokk](https://flutter.gskinner.com/flokk/#g-download) 和 [Folio](https://flutter.gskinner.com/folio/#g-download) 應用，還可以直接在瀏覽器中執行它們。另一個優秀範例是 [用於建立指南本身的應用](https://www.youtube.com/watch?v=8YUIrIGGc3Y)。

Flutter 平台自適應應用指南的 UX 部分以新的 [Material 大螢幕指南](https://material.io/blog/material-design-for-large-screens) 為基礎。Material 團隊在新發布的這一指南中，根據大螢幕的特性，修訂了多篇主要的佈局文章，調整了多個元件，並更新了 Design Kit。

![](https://devrel.andfun.cn/devrel/posts/2021/05/oXdq99.png)

Flutter 的目標始終是讓應用可以走得更高更遠，而不僅是能夠在多個平臺上正常執行。不做到讓您的應用能夠良好適配所有目標平台，我們不會停下腳步。Flutter 能夠為您提供所需支援，讓您的應用不但能夠覆蓋多個目標平台，而且能夠針對不同的螢幕尺寸、輸入方式以及各平台的不同使用習慣而做出適當的調整。

[**更多 Material 圖示**](https://github.com/flutter/flutter/pull/76607)

說起 Material 指南，此版本還納入了兩個獨立的 PR，向 Flutter 中添加了更多 [Material 圖示](https://github.com/flutter/flutter/pull/78311)。我們可愛的吉祥物 Dash 現在也有專屬的圖示了！

![](https://devrel.andfun.cn/devrel/posts/2021/05/U8KghW.png)
![](https://devrel.andfun.cn/devrel/posts/2021/05/3nWtj6.png)

加上這些新圖示，可供您的應用使用的 Material 圖示總數現已突破 7,000 大關。如果您在這海量圖示中尋找所需圖示時遇到了困難，可以存取 [fonts.google.com/icons](https://fonts.google.com/icons)，按類別和名稱來搜尋圖示。

找到理想的圖示後，您可以在新的 "Flutter" 標籤頁中檢視有關如何使用該圖示的說明。您也可以單獨下載這個圖示，將其用作應用中的獨立資源。在您的 Flutter 應用中加入 Dash 的可愛形象從未如此簡單。

![透過名稱搜尋 Material 圖示](https://devrel.andfun.cn/devrel/posts/2021/05/BmOCBD.png)

透過名稱搜尋 Material 圖示

**改進文字處理方式**

隨著我們持續改進 Flutter 以使其能夠更好地適配各個平台的具體特性，我們正逐步涉足一些在移動裝置上不突出但在桌面裝置上很重要的新領域，其中之一就是文字處理。在此版本中，我們著手對文字輸入的處理方式進行重構，以實現下述功能: 在按鍵事件於 widget 層次結構內傳播的過程中將其取消，以及能夠完全自訂與文字操作相關聯的按鍵。

由於現在能夠取消按鍵事件，Flutter 按下空格鍵和箭頭鍵時不會再觸發滾動事件，從而讓使用者獲享更直觀的體驗。您還可以使用此功能來實現在一個按鍵事件傳播到您應用中的父級 widget 之前對其進行處理。另一個例子是，您可以在您的 Flutter 應用中實現用 Tab 鍵在 TextField 和按鈕之間跳轉，一切都可按預期正常運作:

```Dart
import 'package:flutter/material.dart';

void main() => runApp(App());

class App extends StatelessWidget {
 @override
 Widget build(BuildContext context) => MaterialApp(
       title: 'Flutter Text Editing Fun',
       home: HomePage(),
     );
}

class HomePage extends StatelessWidget {
 @override
 Widget build(BuildContext context) => Scaffold(
       body: Column(
         children: [
           TextField(),
           OutlinedButton(onPressed: () {}, child: const Text('Press Me')),
         ],
       ),
     );
}
```

![Flutter 2.2 可在按鍵事件於 widget 層次結構內向上傳播的過程中將其取消；例如，您可以將 TAB 鍵用於將焦點從 TextField 切換到其他元素](https://devrel.andfun.cn/devrel/posts/2021/05/0YgCDW.png)

Flutter 2.2 可在按鍵事件於 widget 層次結構內向上傳播的過程中將其取消；例如，您可以將 TAB 鍵用於將焦點從 TextField 切換到其他元素

自訂文字操作讓您可以實現對 TextField 中的 Enter 按鍵事件進行特殊處理，例如，使 Enter 鍵在聊天客戶端中觸發訊息傳送操作，同時讓使用者可以按 Ctrl+Enter 鍵來換行。此類文字操作 [讓 Flutter 本身也能夠提供不同的按鍵設定](https://github.com/flutter/flutter/pull/75032)，從而使文字編輯行為與主機作業系統相一致，例如在 Windows 和 Linux 上使用 Ctrl+C 複製文字，而在 macOS 上使用 Cmd+C。

下面就是一個這樣的例子，它覆蓋了預設的向左箭頭按鍵操作，併為退格鍵和刪除鍵設定了新的操作:

```Dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
 @override
 Widget build(BuildContext context) => MaterialApp(
       title: 'Flutter TextField Key Binding Demo',
       home: Scaffold(body: UnforgivingTextField()),
     );
}

/// A text field that clears itself if the user tries to back up or correct
/// something.
class UnforgivingTextField extends StatefulWidget {
 @override
 State<UnforgivingTextField> createState() => _UnforgivingTextFieldState();
}

class _UnforgivingTextFieldState extends State<UnforgivingTextField> {
 // The text editing controller used to clear the text field.
 late TextEditingController controller;

 @override
 void initState() {
   super.initState();
   controller = TextEditingController();
 }

 @override
 Widget build(BuildContext context) => Shortcuts(
       shortcuts: <LogicalKeySet, Intent>{
         // This overrides the left arrow key binding that the text field normally
         // has in order to move the cursor back by a character. The default is
         // created by the MaterialApp, which has a DefaultTextEditingShortcuts
         // widget in it.
         LogicalKeySet(LogicalKeyboardKey.arrowLeft): const ClearIntent(),

         // This binds the delete and backspace keys to also clear the text field.
         // You can bind any key, not just those already bound in
         // DefaultTextEditingShortcuts.
         LogicalKeySet(LogicalKeyboardKey.delete): const ClearIntent(),
         LogicalKeySet(LogicalKeyboardKey.backspace): const ClearIntent(),
       },
       child: Actions(
         actions: <Type, Action<Intent>>{
           // This binds the intent that indicates clearing a text field to the
           // action that does the clearing.
           ClearIntent: ClearAction(controller: controller),
         },
         child: Center(child: TextField(controller: controller)),
       ),
     );
}

/// An intent that is bound to ClearAction.
class ClearIntent extends Intent {
 const ClearIntent();
}

/// An action that is bound to ClearIntent that clears the TextEditingController
/// passed to it.
class ClearAction extends Action<ClearIntent> {
 ClearAction({required this.controller});

 final TextEditingController controller;

 @override
 Object? invoke(covariant ClearIntent intent) {
   controller.clear();
 }
}
```

![一個糟糕的 TextField 範例，按下左箭頭鍵或 ESC 鍵會清除文字](https://devrel.andfun.cn/devrel/posts/2021/05/hpCWg7.png)

一個糟糕的 TextField 範例，按下左箭頭鍵或 ESC 鍵會清除文字

在這方面，我們的確還有很多工作要做，但我們將不遺餘力賦予開發者對文字編輯操作的完全控制權。目標是在 Flutter 桌面版達到穩定時，終端使用者將無法區分 Flutter 應用中的文字編輯功能與主機作業系統中的其他應用有什麼差別。

**自動滾動行為**

我們一直致力於讓 Flutter 應用的表現與各個平臺上最優秀的應用如出一轍，為此，我們在這個版本中對捲軸又做了進一步最佳化。在捲軸的顯示方式上，Android 和 iOS 將完全相同，即預設不顯示捲軸。但在桌面應用中，只要內容大於容器，通常就會自動顯示捲軸。這就要求您新增一個類別型為 `Scrollbar` 的父級 widget。為了在移動裝置和桌面裝置上都實現正確的行為，此版本會在需要時自動新增一個 `Scrollbar`。

以下是一段不含 Scrollbar 的程式碼:

```Dart
import 'package:flutter/material.dart';

void main() => runApp(App());

class App extends StatelessWidget {
 @override
 Widget build(BuildContext context) => MaterialApp(
       title: 'Automatic Scrollbars',
       home: HomePage(),
     );
}

class HomePage extends StatelessWidget {
 @override
 Widget build(BuildContext context) => Scaffold(
       body: ListView.builder(
         itemCount: 100,
         itemBuilder: (context, index) => Text('Item $index'),
       ),
     );
}
```

上述程式碼在桌面平臺上執行時，將會顯示一個捲軸:

![](https://devrel.andfun.cn/devrel/posts/2021/05/t8U8U6.png)

如果您不喜歡捲軸的外觀或不想讓捲軸一直顯示，可以設定 [`ScrollBarTheme`](https://api.flutter.dev/flutter/material/ThemeData/scrollbarTheme.html)。如果您不喜歡該預設行為，可以在應用層級或具體例項層級透過設定 [`ScrollBehavior`](https://api.flutter.dev/flutter/widgets/ScrollBehavior-class.html) 來對其進行更改。如需詳細瞭解新的預設捲軸行為以及如何遷移程式碼以遵循新的最佳實踐，請參閱 [Flutter 官方文件](https://flutter.tw/release/breaking-changes/default-desktop-scrollbars)。

**TextSpan 上的滑鼠指標**

在之前的 Flutter 版本中，您可以在任何 widget 上新增一個滑鼠指標 (例如用手型指標來表示元素可供點選)。實際上，Flutter 會在大部分情況下替您新增此類滑鼠指標，例如在所有按鈕上新增手型滑鼠指標。不過，如果您是用多個不同樣式的 TextSpan 來實現一段富文字，而且其長度可能長到需要換行，那就沒辦法給它新增滑鼠指標了，這是因為 `TextSpan` 不屬於 `widget`，不能用於界定滑鼠指標的視效範圍。但現在不一樣了！在新版本中，如果您的 `TextSpan` 具備手勢識別器 (GestureRecognizer)，系統將自動為其設定滑鼠指標:

```Dart
import 'package:flutter/gestures.dart';
import 'package:flutter/material.dart';
import 'package:url_launcher/url_launcher.dart' as urlLauncher;

void main() => runApp(App());

class App extends StatelessWidget {
 static const title = 'Flutter App';
 @override
 Widget build(BuildContext context) => MaterialApp(
       title: title,
       home: HomePage(),
     );
}

class HomePage extends StatelessWidget {
 @override
 Widget build(BuildContext context) => Scaffold(
       appBar: AppBar(title: Text(App.title)),
       body: Center(
         child: RichText(
           text: TextSpan(
             style: TextStyle(fontSize: 48),
             children: [
               TextSpan(
                 text: 'This is not a link, ',
                 style: TextStyle(color: Colors.black),
               ),
               TextSpan(
                 text: 'but this is',
                 style: TextStyle(color: Colors.blue),
                 recognizer: TapGestureRecognizer()
                   ..onTap = () {
                     urlLauncher.launch('https://flutter.dev');
                   },
               ),
             ],
           ),
         ),
       ),
     );
}
```

現在您可以自由使用任意換行的 TextSpan，只要其具備手勢識別器，系統即會為其設定適當的滑鼠指標。

![](https://devrel.andfun.cn/devrel/posts/2021/05/fnraQI.png)

在此版本中，`TextSpan` 除了支援 `mouseCursor`，還支援 `onEnter` 和 `onExit`。這些改進看似細小，對使用者體驗的影響卻大，能讓 Flutter 應用提供更貼近使用者預期的使用體驗。

## **Flutter 2.2 中新的預覽版功能**

除了適用於生產環境的新功能，Flutter 2.2 還搭載了多項新的預覽版功能，包括 iOS 著色器編譯效能改進、Android 延遲載入元件支援、Flutter 桌面平台更新以及 Sony 貢獻的 ARM64 Linux 主機支援。歡迎您體驗這些功能，並 [在遇到問題時向我們報告](http://github.com/flutter/flutter/issues)。

**iOS 著色器編譯改進**

在圖形渲染領域，"著色器" 指的是一個在終端使用者裝置可用的 GPU 上編譯並執行的程式。Flutter 自誕生伊始就在底層的 Skia 圖形庫中使用著色器，從而在色彩、陰影、動畫等方面實現高品質圖形效果的同時達成可媲美原生程式碼的優越效能。得益於 Flutter API 的靈活性，著色器能夠以 JIT 方式即時產生和編譯，並與需要它們的幀工作負載同步。不過，當著色器的編譯耗時超出相應幀的時間預算，就會導致使用者感受到卡頓。

為了避免這類卡頓，Flutter 讓您可以在訓練執行期間快取著色器，並將其打包到應用中，然後於 Flutter 引擎啟動時的第一幀之前對其進行編譯。這樣一來，這些預先編譯的著色器就不必在幀工作負載期間進行編譯，也就不會造成卡頓。不過，Skia 一開始只針對 OpenGL 實現了此功能。

這就導致，我們因 Apple 廢棄 OpenGL 而在 iOS 上預設啟用 Metal 後端後，基準測試測得的最差幀時間數值有所增長，使用者報告卡頓的情況也有所增多。經過調查，我們發現此類卡頓往往由下列因素造成: 著色器編譯時間增加、Skia 為 Metal 後端產生的著色器數量增多，以及未能在多次執行之間對編譯後的著色器進行快取並因而導致應用在後續執行時仍然出現卡頓。

到目前為止，在 iOS 上杜絕此類卡頓的唯一方式就是對場景和動畫進行簡化，而這不是一個理想的解決方案。

為解決這個問題，我們已在 dev 渠道中釋出 [Skia 針對 Metal 新增對著色器預熱的支援](https://github.com/flutter/flutter/issues/79298) 這一預覽版功能。Flutter 現在會透過 Skia 在第一個幀工作負載開始之前對捆綁的著色器進行編譯。

![應用啟動期間的預編譯操作的追蹤記錄](https://devrel.andfun.cn/devrel/posts/2021/05/6E40Xj.png)

應用啟動期間的預編譯操作的追蹤記錄

不過，此解決方案存在一些不足之處:

* 相比 OpenGL 後端，Skia 仍會為 Metal 後端產生更多的著色器。
* 最終將著色器編譯為機器碼的操作仍然與幀工作負載同步發生，但這好過在幀渲染時段內執行所有著色器產生和編譯操作。
* 最終產生的機器碼會在應用首次執行後得到快取，直到裝置被重新啟動。

如果您希望在自己的應用中使用此新功能，可以參考
[Flutter 官方文件](https://flutter.tw/perf/shader#how-to-use-sksl-warmup) 上的說明進行操作。

我們將持續改進此實現方案。在 Android 和 iOS 上，它目前有幾個缺點:

* 由於捆綁了著色器，應用的部署體積更大。
* 由於需要預編譯捆綁的著色器，應用啟動延時更長。
* 對開發者體驗可能存在負面影響。

其中，我們最為重視最後一點。我們認為開發者不應該需要執行訓練執行，也不應該在應用體積和啟動延時方面承擔代價。因此，我們還在繼續探索是否有不依賴此實現方案的其他方法來消除著色器編譯卡頓以及其他型別的卡頓。我們正在與 Skia 團隊攜手合作，降低 Flutter 請求產生的著色器數量，並探討在 Flutter 引擎中捆綁少量靜態定義的著色器的可行性。

您可以關注 [Flutter 程式碼庫中的相關專案](https://github.com/flutter/flutter/projects/188) 瞭解相關進展。

**Android 延遲載入元件: 可下載的 AOT 程式碼和資源**

在 Android 平臺上，此版本利用 Dart 的分拆式 AOT 編譯功能來 [讓 Flutter 應用能夠在執行時下載內含預先編譯的程式碼和資源的模組](https://github.com/flutter/flutter/pull/76192)。我們將這種可安裝的分拆模組稱為 "延遲載入元件"。由於可在需要時才下載程式碼和資源，應用的初始安裝體積顯著縮小。例如，我們實現了一個特殊版本的 Flutter Gallery 應用，將其中所有案例和 demo 都設為延遲載入，從而將初始安裝體積縮減了 46%。

![在 Flutter Gallery 應用中下載 Crane 案例](https://devrel.andfun.cn/devrel/posts/2021/05/TJQbmZ.gif)

在 Flutter Gallery 應用中下載 Crane 案例

如果在建構應用時啟用延遲載入元件，Dart 會將那些單獨用 `deferred` 關鍵字匯入的程式碼編譯到獨立的共享庫中，並將這些庫與相關資源一起打包為延遲載入元件。

該功能還處於早期預覽階段，目前只支援 Android。您可以在 Flutter 官方文件上新的 [延遲載入元件](https://flutter.tw/perf/deferred-components) 頁面中瞭解如何實現此類元件。此頁面還連結到了 Flutter wiki 上的一個頁面，後者詳細介紹了此功能的工作原理。如果發現問題，請透過 [Flutter 問題追蹤器](http://github.com/flutter/flutter/issues) 告知我們。

**Flutter Windows UWP alpha 版**

此版本 Flutter 的另一項更新針對的是桌面平台: 對 Windows UWP 的支援現已在 dev 渠道中進入 alpha 版階段 (2.2 穩定版之後)。藉助 UWP，您的 Flutter 應用將可以覆蓋 Xbox 等無法執行標準 Windows 應用的裝置。如需試用此功能，請先 [滿足 UWP 前置條件](https://flutter.cn/desktop#windows-uwp)，然後切換到 dev 渠道並啟用 UWP 支援。

```
$ flutter channel dev
$ flutter upgrade
$ flutter config — enable-windows-uwp-desktop
```

啟用後，建立的 Flutter 應用會包含一個新的 `winuwp` 資料夾，以便您在 UWP 容器中建構和執行您的應用:

```
$ flutter create uwp_fun
$ cd uwp_fun
$ flutter pub get
$ flutter run -d winuwp
```

由於建構的是在 Windows 沙盒環境中執行的 Windows UWP 應用，您需要在開發期間對應用的防火牆進行 localhost 穿透設定，以實現熱重載和偵錯程式斷點等功能。為此，您可以按照 [Flutter 桌面文件頁面](http://flutter.cn/desktop/#windows-uwp) 上的說明使用 *checknetisolation* 進行操作。完成相關操作後，您就可以在 Windows 上以 UWP 應用的形式執行您喜愛的 Flutter 應用了。

![在 Windows UWP 容器中執行您喜愛的 Flutter 應用](https://devrel.andfun.cn/devrel/posts/2021/05/KxCQcf.png)

在 Windows UWP 容器中執行您喜愛的 Flutter 應用

當然，您也可以執行一些更豐富有趣的 UWP 應用，例如這個 [影片](https://devrel.andfun.cn/devrel/posts/2021/05/q5hepO.mp4) 中在 Xbox 上執行的 Flutter 應用。

<video controls width="640px" height="480px" src="https://devrel.andfun.cn/devrel/posts/2021/05/q5hepO.mp4"></video>

我們要特此致謝 [clarkezone](https://github.com/clarkezone)，從我加入 Flutter 團隊時起，他就一直投身於實現這項功能。如需詳細瞭解 Windows UWP alpha 版支援，請參閱 [官方文件](http://flutter.cn/desktop/#windows-uwp)。

**Sony 貢獻的 ARM64 Linux 主機支援**

這項傑出貢獻來自於 Flutter 社群的 Sony 軟體工程師 [HidenoriMatsubayashi](https://github.com/HidenoriMatsubayashi)。他提交的 [ARM64 Linux 目標支援](https://github.com/flutter/flutter/pull/61221) 這條 PR 讓您可以在 ARM64 Linux 機器上建構和執行 Flutter 應用。

![在 ARM64 Linux 機器上執行您喜愛的 Flutter 應用](https://devrel.andfun.cn/devrel/posts/2021/05/tpaxju.png)

在 ARM64 Linux 機器上執行您喜愛的 Flutter 應用

我們很高興看到 Flutter 社群將 Flutter 移植到超乎 Google 團隊想象的平台。HidenoriMatsubayashi，感激不盡！

## **Flutter 生態系統和工具更新**

Flutter 引擎和框架只是整體體驗的一部分，package 生態系統和工具方面的更新對 Flutter 開發者的體驗來說同樣重要。我們在這個領域也有幾項很棒的更新與您分享。

在生態系統端，我們釋出了多個 Flutter Favorite package，並更新了 Flutter 的 Firebase 支援外掛集 FlutterFire。FlutterFire 現在還支援新的 Firebase App Check 預覽版，讓 Flutter 開發者自第一天起就可受益於這個產品。

在工具端，Flutter DevTools 做了多項更新以方便您最佳化應用的記憶體使用方式，還為 Provider package 提供了一個標籤頁。適用於 VS Code 和 Android Studio/IntelliJ 的 IDE 外掛也有重大更新。此外，如果您是 Flutter 相關內容的創作者，我們還提供了一種全新方式讓您將 DartPad 整合到您的創作流程中。

最後，Flutter 現在有了一個新的低程式碼 (low-code) 應用設計和建構工具，名為 FlutterFlow。該工具在 web 上執行，本身也是使用 Flutter 建構而成。

**Flutter Favorite 更新**

在此版本中，得益於 Flutter 生態系統委員會 (FEC) 的辛勤工作，我們增加了 24 個 Flutter Favorite 認證 package，堪稱有史以來步伐最大的一次擴張。這些新出爐的 Flutter Favorites package 包括:

* [**FlutterFire package**](http://firebase.flutter.dev) **(正式版)** : `cloud_firestore`、`cloud_functions`、`firebase_auth`、`firebase_core`、`firebase_crashlytics`、`firebase_messaging` 和 `firebase_storage` 
* [**Flutter Community "plus"**](http://plus.fluttercommunity.dev) package: `android_alarm_manager_plus`、`android_intent_plus`、`battery_plus`、`connectivity_plus`、`device_info_plus`、`network_info_plus`、`package_info_plus`、`sensors_plus` 和 `share_plus`
* [**googleapis**](https://pub.dev/packages/googleapis) **package**
* [**win32** **package**](https://pub.dev/packages/win32)
* [**Intl**](https://pub.dev/packages/intl) 和 [**characters**](https://pub.dev/packages/characters) **package**
* [**Sentry**](https://pub.dev/packages/sentry_flutter) **package**: sentry 和 sentry_flutter 
* [**infinite_scroll_pagination**](https://pub.dev/packages/infinite_scroll_pagination) 和 [**flutter_native_splash**](https://pub.dev/packages/flutter_native_splash) **package**

所有這些 package 都已支援空 (null) 安全特性，並儘可能支援 Android、iOS 和 web 平台。其中有一些例外，例如 `firebase_crashlytics` 沒有適用於 web 平台的底層 SDK，而 `android_alarm_manager_plus` 則是專為 Android 平台設計。

Flutter Community "plus" package 在 Flutter 團隊官方 package 的基礎上提供了更多特性。例如，Google 的 Flutter 團隊提供的 [battery package](https://pub.dev/packages/battery) 可以追溯到初版 Flutter 釋出之前，它現在具備空安全特性，但只支援 Android 和 iOS 平台。與此相對，[Flutter Community battery_plus package](https://pub.dev/packages/battery_plus) 則支援全部 6 個 Flutter 平台，包括 web、Windows、macOS 和 Linux。該套件內建的全部 9 個 "plus" package 都獲得了 Flutter Favorite 認可，這是標誌著整個 Flutter 社群邁向成熟的重大一步。Flutter 專案已經遠遠不止是 Google 工程團隊獨力奮戰。您應儘快將您的程式碼遷移至 "plus" package。在未來幾週中，Google 的相關 package 將會更新，建議您進行遷移。

googleapis 外掛為 185 個 [Google API](https://developers.google.cn/api-client-library) 提供了自動產生的 Dart 封裝，供您在客戶端或伺服器端的 Dart 應用 (包括您的 Flutter 應用) 中使用。如需進一步瞭解此 package，您可以觀看其作者關於使用 Google API 為您的 Flutter 應用賦能的 [I/O 大會演講](https://events.google.com/io/session/7f706716-0de0-4a9e-bad3-581afe8ef360)。

win32 package 堪稱一件工程傑作，它使用 [Dart FFI](https://dart.tw.gh.miniasp.com/guides/libraries/c-interop) 封裝了大部分常用 Win32 API 介面，讓 Dart 程式碼無需依賴 C 編譯器或 Windows SDK 即可使用這些 API。隨著 Flutter 在 Windows 平臺上越來越受關注，win32 package 成為了許多熱門外掛的關鍵依賴項，其中包括最熱門 Flutter package 之一的 [path_provider](https://pub.dev/packages/path_provider)。為了挑戰極限，其作者 [timsneath](https://github.com/timsneath) 創下了一些驚人壯舉，例如使用純 Win32 和純 Dart 來實現 [notepad](https://github.com/timsneath/win32/tree/main/example/notepad) (記事本)、[snake](https://github.com/timsneath/win32/blob/main/example/snake.dart) (貪吃蛇) 和 [tetris](https://github.com/timsneath/win32/tree/main/example/tetris) (俄羅斯方塊)。

![僅使用 Dart FFI 和 Win32 介面實現的在 Windows 平台執行的俄羅斯方塊遊戲](https://devrel.andfun.cn/devrel/posts/2021/05/86UACo.png)

僅使用 Dart FFI 和 Win32 介面實現的在 Windows 平台執行的俄羅斯方塊遊戲

如果您有在 Windows 上進行任何 Dart 或 Flutter 開發，win32 package 值得您一探究竟。

**FlutterFire 更新和 Firebase App Check**

作為 Flutter 的 Firebase 支援外掛集，FlutterFire 已躋身最熱門 Flutter 外掛之列。為了讓該外掛集的正式版能夠與 Flutter 2 一道釋出並在隨後對其持續進行改進，其主要貢獻者 Invertase 團隊做了許多出色的工作。自 FlutterFire 的首個正式版釋出之後，Invertase 將未解決問題的數量降低了 79%，並將待處理 PR 的數量降低了 88%。他們不僅在正式版外掛的開發上成績斐然，還成功將所有 Beta 版外掛都支援了空安全機制，同時讓這些外掛在您可能遇到的各種核心上都能正常建構和執行。

Invertase 還在繼續為 FlutterFire 外掛增添更多功能，包括在新版 Flutter 中對 Cloud Firebase 整合做的多項更新:

* 用於讀寫資料的 [Typesafe API](https://firebase.flutter.dev/docs/firestore/usage/#typing-collectionreference-and-documentreference)
* 支援 [Firebase Local Emulator Suite](https://firebase.flutter.dev/docs/storage/usage/#emulator-usage)
* 利用 [data bundles](https://firebase.flutter.dev/docs/firestore/usage#data-bundles) 最佳化您的資料查詢

FlutterFire 現在還支援 Firebase 新推出的 beta 版產品 [Firebase App Check](https://firebase.google.com/docs/app-check)。Firebase App Check 可幫助您保護 Cloud Storage 等後端資源，使其不受付費欺詐或釣魚攻擊等濫用行為的侵害。有了 App Check，執行您的 Flutter 應用的裝置會透過一個應用身份證明提供方來證明應用例項的正當身份，並可檢查應用是否執行在未受篡改的可信裝置上。一旦您啟用 App Check，相應證明資訊會附加到您的應用向 Firebase 後端資源發出的每一個請求中。要了解詳情，請參閱 [FlutterFire App Check 文件](https://firebase.flutter.dev/docs/app-check/overview)。

**Flutter DevTools 更新**

此版本的 Flutter DevTools 帶來了多項重大更新，包括記憶體追蹤方面的兩項改進和一個 Provider 外掛專屬的新標籤頁。

記憶體追蹤方面的第一項改進: 追蹤物件的記憶體分配位置。這有助於找出導致記憶體洩漏的程式碼。

![Flutter DevTools 記憶體標籤頁分配棧軌跡](https://devrel.andfun.cn/devrel/posts/2021/05/bi1jDH.png)

Flutter DevTools 記憶體標籤頁分配棧軌跡

第二項改進: 向記憶體時間軸中注入自訂資訊。這讓您可以根據應用的具體情況新增標註，例如標出記憶體密集型工作的開始點和結束點，以驗證是否適當地執行了清理。

![Flutter DevTools 時間軸標籤頁自訂記憶體事件](https://devrel.andfun.cn/devrel/posts/2021/05/xQYYra.png)

Flutter DevTools 時間軸標籤頁自訂記憶體事件

隨著 Flutter 應用的體積逐步增長，我們將繼續努力為 Flutter 開發者提供所需的工具，助其追蹤和修復記憶體洩漏和其他各種執行時問題。

您需要排查的問題並不僅僅是那些與 Flutter 框架相關的執行時問題，有時還有與所依賴 package 相關的問題。隨著 pub.dev 上的 Flutter 相容 package 數量突破 15,000 大關，您的應用使用的 package 數量也很可能隨之增長。有鑑於此，Flutter DevTools 中新加入了一個實驗性的 **Provider** 標籤頁，由 [provider package](http://pub.dev/packages/provider) 本身及其他很多出色軟體的開發者 [Remi Roussel](https://github.com/rrousselGit) 傾力打造。如果您執行的是最新版 Flutter，在您對一個包含 provider 外掛的 Flutter 應用進行除錯時，**Provider** 標籤頁就會自動顯示。

![實戰示範: Flutter DevTools Provider 標籤頁](https://devrel.andfun.cn/devrel/posts/2021/05/Q04qlm.gif)

實戰示範: Flutter DevTools Provider 標籤頁

Provider 標籤頁會向您展示每個 provider 的相關資料，並會即時反映您在應用執行過程中做出的更改。很棒吧？不僅如此，它還可以讓您直接更改這些資料，從而對應用的邊角案例進行測試。太讚了！

在協力 Remi 開發此標籤頁的過程中，我們也學到了一些寶貴經驗，知道了該如何更好地為具有類似需求的 package 作者服務。您可以瞭解 [Remi 是如何構建出 Provider 標籤頁的](https://invertase.io/blog/how-to-flutter-devtool-plugin)，還可以透過 [Flutter DevTools](https://docs.google.com/document/d/1BWX8YQ962Vsx-EUDuDHRG7RX94fJlZSDRu299YRwasE/) 外掛提案瞭解我們當前的改進思路。歡迎向我們提供反饋；同時，如果您在 Flutter DevTools 新增更多標籤頁方面也有自己的想法，我們也很期待聽到您的高見。

上述改進只是此版本 Flutter DevTools 眾多改進中的幾項。如需完整列表，請檢視下列公告:

* [Flutter DevTools 2.1 版本說明](https://groups.google.com/g/flutter-announce/c/tCreMfJaJFU/m/38p1BBeiCAAJ)
* [Flutter DevTools 2.2.1 版本說明](https://groups.google.com/g/flutter-announce/c/t8opLnUyiFQ/m/dJth-jKxAAAJ)
* [Flutter DevTools 2.2.3 版本說明](https://groups.google.com/g/flutter-announce/c/t8opLnUyiFQ/m/YX5Ds_q0AgAJ)

**IDE 外掛更新**

在此版本中，針對 Flutter 的 Visual Studio Code 和 IntelliJ/Android Studio IDE 擴充也得到了更新。例如，Visual Studio Code 擴充現在支援兩項額外的 Dart 程式碼重構: 行內函數和內聯本地變數。

![實戰示範: 新的 "行內函數" Dart 重構](https://devrel.andfun.cn/devrel/posts/2021/05/bBJcfx.gif)

實戰示範: 新的 "行內函數" Dart 重構

在 Android Studio/IntelliJ 擴充中，我們新增了一個在控制檯中輸出所有堆疊軌跡的選項。

![您現在可以輸出所有堆疊軌跡，而不僅限於第一條](https://devrel.andfun.cn/devrel/posts/2021/05/KKLGup.png)

您現在可以輸出所有堆疊軌跡，而不僅限於第一條

如果專案中某個問題的根源是在另一個 package，以前相關軌跡不會納入到輸出中，這項新功能就可以派上大用場。但隨之而來的一個問題是，該如何讓軌跡資訊更易於梳理解讀？我們已經有了一些想法，歡迎關注後續相關改動。

如需此版本中 IDE 擴充的完整變更列表，請檢視下列公告:

* [VS Code 擴充 v3.21](https://groups.google.com/g/flutter-announce/c/gNtKp9c1glU/m/SZYTuwcQBwAJ)
* [VS Code 擴充 v3.22](https://groups.google.com/g/flutter-announce/c/1XR7baYZOVI/m/y6MGYrGhAAAJ)
* [Flutter IntelliJ 外掛 M55 版本](https://groups.google.com/g/flutter-announce/c/tYwFDPAtLu0/m/FrsntcNNBwAJ)
* [Flutter IntelliJ 外掛 M56 版本](https://groups.google.com/g/flutter-announce/c/EkgiAO4p3UM/m/P32ZXXKfAAAJ)

**DartPad 隨堂實踐**

為了確保 Flutter 開發者社群蓬勃發展的同時讓相關文件也能跟上腳步，Dart 和 Flutter 團隊始終在想方設法改進和拓展相關培訓內容的製作方法。在此版本中，我們為 DartPad 添加了一個新的分步式介面，讓開發者在學習講師主導的課程/講座時可以方便地隨堂動手實踐。

![實戰示範: DartPad 課程/講座隨堂實踐](https://devrel.andfun.cn/devrel/posts/2021/05/rOt9q7.png)

實戰示範: DartPad 課程/講座隨堂實踐

透過將實踐指引直接加入到 DartPad 中，我們在今年的 I/O 大會實現了 [引導式講座體驗](https://events.google.com/io/program/content?4=topic_flutter&5=type_workshop)。不過，此功能並非專為我們自己的需求而開發，如果您要在自己的 Dart 或 Flutter 課程/講座中使用該工具，可以按 [DartPad 培訓內容創作指南](https://github.com/dart-lang/dart-pad/wiki/Workshop-Authoring-Guide) 中的說明操作。請勿將此功能與 [在 Gist 中透過 DartPad 分享程式碼](https://github.com/dart-lang/dart-pad/wiki/Sharing-Guide) 和 [在您的網站上嵌入 DartPad](https://github.com/dart-lang/dart-pad/wiki/Embedding-Guide) 這兩項功能相混淆，後兩者已經推出了一段時間。

我們希望所有 Dart 和 Flutter 內容作者都能夠為其受眾帶去豐富的互動式體驗。歡迎試用並 [向我們提供反饋](https://github.com/dart-lang/dart-pad/issues)。

**社群聚焦: FlutterFlow**

FlutterFlow 是一個 "低程式碼" (low code) 應用設計和開發工具，用於在瀏覽器中建構應用。它提供了一個 "所見即所得" 環境，讓您可根據實際 Firebase 資料來設計橫跨多個頁面的應用佈局。此工具的目標是讓您能夠輕鬆執行大部分常見操作，並儘量減少您自己需要編寫的程式碼量。實際上，在一次示範中，示範者不到一個小時就使用此工具構建出了一個包含多個頁面、可幫助使用者遊覽大都會藝術博物館的完整移動應用，整個過程沒有寫一行程式碼。您可以在 YouTube 上觀看整個過程的 [影片記錄](https://devrel.andfun.cn/devrel/posts/2021/05/ilIETB.mp4)。

<video controls width="640px" height="480px" src="https://devrel.andfun.cn/devrel/posts/2021/05/ilIETB.mp4"></video>

## **對相容性有影響的重大變更**

一如既往，我們會盡量避免進行影響相容性的重大變更。在此版本中，此類變更僅限於移除下面這些已廢棄的元件:

* [73750](https://github.com/flutter/flutter/pull/73750) 移除已廢棄的 BinaryMessages
* [73751](https://github.com/flutter/flutter/pull/73751) 移除已廢棄的 TypeMatcher 類

您可以 [在 flutter.cn 上找到這些變更對應的遷移指南](https://flutter.tw/release/breaking-changes/1-22-deprecations).

## **結語**

Google Flutter 團隊全體同仁謹此向大家致以誠摯謝意！我們要感謝 Flutter 社群的每一個人，Flutter 取得的成績離不開大家的每一份貢獻。如今，Play 商店中有超過八分之一的新應用是採用 Flutter 建構而成，僅在 Play 商店就收錄了多達 20 萬款 Flutter 應用。Flutter 的迅猛發展勢頭出人意表。世界各地大大小小的應用團隊都使用 Flutter 進行開發，為諸多平臺上的使用者打造優質體驗。謝謝大家選擇 Flutter！

![](https://devrel.andfun.cn/devrel/posts/2021/05/KTpWHI.png)

最後，以免您錯過，不要忘了體驗下在本次 I/O 大會推出的 [I/O Photo Booth web 應用](https://photobooth.flutter.cn/#/)，它是由 Flutter 和 Firebase 建構而成的 web 應用，讓您可以和 Dash 合拍萌萌的大頭照。我們還 [開放了其原始碼](https://github.com/flutter/photobooth)，方便您進一步瞭解 Flutter web 最佳實踐、相機外掛 web 支援以及如何使用 Cloud Functions 函式來產生自訂社交內容。機不可失，趕緊一睹為快吧！
