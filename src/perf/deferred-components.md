---
title: Deferred components
title: 延遲載入元件
description: How to create deferred components for improved download performance.
description: 如何使用延遲元件來提高下載效能
---

<?code-excerpt path-base="perf/deferred_components"?>

## Introduction

## 簡介

Flutter has the capability to build apps that can
download additional Dart code and assets at runtime.
This allows apps to reduce install apk size and download
features and assets when needed by the user.

Flutter 支援建構在執行時下載額外 Dart 程式碼和靜態資源的應用程式。
這可以減少安裝應用程式 apk 的大小，並在使用者需要時下載功能和靜態資源。

We refer to each uniquely downloadable bundle of Dart
libraries and assets as a "deferred component".
This is achieved by using Dart's deferred imports,
which can be compiled into split AOT shared libraries.

我們將每個獨立的可下載的 Dart 庫和靜態資源稱為「延遲元件」。
這是透過使用 Dart 的延遲匯入來實現的，
可以將其編譯到拆分的 AOT 共享庫中。

{{site.alert.note}}

  This feature is currently only available on Android,
  taking advantage of Android and Google Play Stores'
  [dynamic feature modules][] to deliver the
  deferred components packaged as Android modules.
  Deferred code does not impact other platforms,
  which continue to build as normal with all deferred
  components and assets included at initial install time.
  
  此功能目前僅在 Android 上可用，
  利用 Android 和 Google Play 商店的 [動態功能模組][dynamic feature modules] 提供打包為 Android module 的延遲元件。
  延遲元件中的程式碼不會影響其他平台，其他平台在初始安裝時會正常建構包含所有延遲元件和資源的應用。
  
  Also, note that this is an advanced feature.
  
  另外，請注意這是一個高階功能。
  
{{site.alert.end}}

Though modules can be defer loaded,
the entire application must be completely built and
uploaded as a single Android App Bundle.
Dispatching partial updates without re-uploading
new Android App Bundles for the entire application
is not supported.

儘管模組可以延遲載入，但整個應用程式必須作為單個 App Bundle 完全建構和上傳。
不支援在沒有重新上傳整個新 Android 應用程式套件的情況下發送部分更新。

Deferred loading is only performed when the app
is compiled to [release or profile mode][].
In debug mode, all deferred components are treated
as regular imports, so they are present
at launch and load immediately. Therefore,
debug builds can still hot reload.

延遲載入僅在應用程式編譯為 [Release 或 Profile 模式][release or profile mode] 時可用。
在 Debug 模式下，所有延遲元件都被視為常規匯入，它們在啟動時立即載入。
因此，Debug 模式下仍然可以熱重載。

For a deeper dive into the technical details of
how this feature works, see [Deferred Components][]
on the [Flutter wiki][].

關於此功能的技術細節，請檢視 [Flutter wiki][] 上的 [延遲載入元件][Deferred Components]。

## How to set your project up for deferred components

## 如何讓專案支援延遲載入元件

The following instructions explain how to set up your
Android app for deferred loading.

下面的引導將介紹如何設定 Android 應用程式以支援延遲載入。

### Step 1: Dependencies and initial project setup

### 步驟 1：依賴項和初始專案設定

<ol markdown="1">
<li markdown="1"><p markdown="1">Add Play Core to the Android app's
    build.gradle dependencies.
    In `android/app/build.gradle` add the following:</p><p markdown="1">將 Play Core 新增到 Android 應用程式的 build.gradle 依賴項中。
    在 `android/app/build.gradle` 中新增以下內容：</p>

```gradle
...
dependencies {
  ...
  implementation "com.google.android.play:core:1.8.0"
  ...
}
```
</li>

<li markdown="1">
<p markdown="1">If using the Google Play Store as the
    distribution model for dynamic features,
    the app must support `SplitCompat` and provide an instance
    of a `PlayStoreDeferredComponentManager`.
    Both of these tasks can be accomplished by setting
    the `android:name` property on the application in
    `android/app/src/main/AndroidManifest.xml` to
    `io.flutter.embedding.android.FlutterPlayStoreSplitApplication`:
</p>
<p markdown="1">如果使用 Google Play 商店作為動態功能的分發模型，
    應用程式必須支援 `SplitCompat` 並手動提供 `PlayStoreDeferredComponentManager` 的例項。
    這兩個任務都可以透過設定 `android/app/src/main/AndroidManifest.xml` 中的 `android:name` 為
    `io.flutter.embedding.android.FlutterPlayStoreSplitApplication` 應用屬性來完成：
</p>

```xml
<manifest ...
  <application
     android:name="io.flutter.embedding.android.FlutterPlayStoreSplitApplication"
        ...
  </application>
</manifest>
```

`io.flutter.app.FlutterPlayStoreSplitApplication` handles
both of these tasks for you. If you use
`FlutterPlayStoreSplitApplication`,
you can skip to step 1.3.

`io.flutter.app.FlutterPlayStoreSplitApplication` 已經為你完成了這兩項任務。
如果你使用了 `FlutterPlayStoreSplitApplication`，可以跳轉至步驟 1.3。

If your Android application
is large or complex, you might want to separately support
`SplitCompat` and provide the
`PlayStoreDynamicFeatureManager` manually.

如果你的 Android 應用程式很大或很複雜，
你可能需要單獨支援 `SplitCompat` 並提供 `PlayStoreDynamicFeatureManager`。

To support `SplitCompat`, there are three methods
(as detailed in the [Android docs][]), any of which are valid:

要支援 `SplitCompat`，有三種方法（詳見 [Android docs][]），其中任何一種都是有效的：

<ul markdown="1">
<li markdown="1"><p markdown="1">Make your application class extend
    `SplitCompatApplication`:</p><p markdown="1">讓你的 application 類繼承 `SplitCompatApplication`：</p>

```java
public class MyApplication extends SplitCompatApplication {
    ...
}
```
</li>

<li markdown="1"><p markdown="1">Call `SplitCompat.install(this);`
    in the `attachBaseContext()` method:</p><p markdown="1">在 `attachBaseContext()` 中呼叫 `SplitCompat.install(this);`：</p>

```java
@Override
protected void attachBaseContext(Context base) {
    super.attachBaseContext(base);
    // Emulates installation of future on demand modules using SplitCompat.
    SplitCompat.install(this);
}
```
</li>

<li markdown="1"><p markdown="1">Declare `SplitCompatApplication` as the application
    subclass and add the Flutter compatibility code from
  `FlutterApplication` to your application class:</p><p markdown="1">將 `SplitCompatApplication` 宣告為 application 的子類別，
  並將 `FlutterApplication` 中的 flutter 相容性程式碼新增到你的 application 類中：</p>

```js
<application
    ...
    android:name="com.google.android.play.core.splitcompat.SplitCompatApplication">
</application>
```
</li>
</ul>

The embedder relies on an injected
`DeferredComponentManager` instance to handle
install requests for deferred components.
Provide a `PlayStoreDeferredComponentManager` into
the Flutter embedder by adding the following code
to your app initialization:

嵌入層依賴注入的 `DeferredComponentManager` 例項來處理延遲元件的安裝請求。
透過在應用程式的初始流程中新增以下程式碼，將 `PlayStoreDeferredComponentManager` 新增到 Flutter 嵌入層中：

```java
import io.flutter.embedding.engine.dynamicfeatures.PlayStoreDeferredComponentManager;
import io.flutter.FlutterInjector;
... 
PlayStoreDeferredComponentManager deferredComponentManager = new
  PlayStoreDeferredComponentManager(this, null);
FlutterInjector.setInstance(new FlutterInjector.Builder()
    .setDeferredComponentManager(deferredComponentManager).build());
```

</li>
    
<li markdown="1"><p markdown="1">Opt into deferred components by adding
    the `deferred-components` entry to the app's `pubspec.yaml`
    under the `flutter` entry:</p><p markdown="1">透過將 `deferred-components` 依賴新增到應用程式的 `pubspec.yaml` 中的 `flutter` 下，並選擇延遲元件：</p>

  ```yaml
  ...
  flutter:
    ...
    deferred-components:
    ...
  ```
  The `flutter` tool looks for the `deferred-components`
  entry in the `pubspec.yaml` to determine whether the
  app should be built as deferred or not.
  This can be left empty for now unless you already
  know the components desired and the Dart deferred libraries
  that go into each. You will fill in this section later
  in [step 3.3][] once `gen_snapshot` produces the loading units.
  
  `flutter` 工具會在 `pubspec.yaml` 中查詢 `deferred-components`，
  來確定是否應將應用程式建構為延遲載入。
  除非你已經知道所需的元件和每個元件中的 Dart 延遲庫，否則可以暫時將其留空。
  當 `gen_snapshot` 產生載入單元后，你可以在後面的 [步驟 3.3][step 3.3] 中完善這部分內容。
  
</li>
</ol>

### Step 2: Implementing deferred Dart libraries

### 步驟 2：實現延遲載入的 Dart 庫

Next, implement deferred loaded Dart libraries in your
app's Dart code. The implementation does not need
to be feature complete yet. The example in the
rest of this page adds a new simple deferred widget
as a placeholder. You can also convert existing code
to be deferred by modifying the imports and
guarding usages of deferred code behind `loadLibrary()`
`Futures`.

接下來，在 Dart 程式碼中實現延遲載入的 Dart 庫。實現並非立刻需要的功能。
文章剩餘部分中的範例添加了一個簡單的延遲 widget 作為佔位。
你還可以透過修改 `loadLibrary()` 和 `Futures` 後面的延遲載入程式碼的匯入和保護用法，將現有程式碼轉換為延遲程式碼。

<ol markdown="1">
<li markdown="1"><p markdown="1">Create a new Dart library.
    For example, create a new `DeferredBox` widget that
    can be downloaded at runtime.
    This widget can be of any complexity but,
    for the purposes of this guide,
    create a simple box as a stand-in.
    To create a simple blue box widget,
    create `box.dart` with the following contents:</p><p markdown="1">建立新的 Dart 庫。
    例如，建立一個可以在執行時下載的 `DeferredBox` widget。
    這個 widget 可以是任意複雜的，本指南使用以下內容建立了一個簡單的框。</p>
    
<?code-excerpt "lib/box.dart"?>
```dart
// box.dart
import 'package:flutter/material.dart';

/// A simple blue 30x30 box.
class DeferredBox extends StatelessWidget {
  const DeferredBox({super.key});

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 30,
      width: 30,
      color: Colors.blue,
    );
  }
}
```
</li>

<li markdown="1"><p markdown="1">Import the new Dart library
    with the `deferred` keyword in your app and
    call `loadLibrary()` (see [lazily loading a library][]).
    The following example uses `FutureBuilder`
    to wait for the `loadLibrary` `Future` (created in
    `initState`) to complete and display a
    `CircularProgressIndicator` as a placeholder.
    When the `Future` completes, it returns the `DeferredBox` widget.
    `SomeWidget` can then be used in the app as normal and
    won't ever attempt to access the deferred Dart code until
    it has successfully loaded.</p><p markdown="1">在應用中使用 `deferred` 關鍵字匯入新的 Dart 庫，並呼叫 `loadLibrary()`（請參見 [延遲載入庫][lazily loading a library]）。
    下面的範例使用 `FutureBuilder` 等待 `loadLibrary` 的 `Future` 物件（在 `initState` 中建立）完成，
    並將 `CircularProgressIndicator` 做為佔位。
    當 `Future` 完成時，會返回 `DeferredBox`。
    `SomeWidget` 便可在應用程式中正常使用，在成功載入之前不會嘗試存取延遲的 Dart 程式碼。</p>
    
<?code-excerpt "lib/use_deferred_box.dart"?>
```dart
import 'package:flutter/material.dart';
import 'box.dart' deferred as box;

class SomeWidget extends StatefulWidget {
  const SomeWidget({super.key});

  @override
  State<SomeWidget> createState() => _SomeWidgetState();
}

class _SomeWidgetState extends State<SomeWidget> {
  late Future<void> _libraryFuture;

  @override
  void initState() {
    _libraryFuture = box.loadLibrary();
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return FutureBuilder<void>(
      future: _libraryFuture,
      builder: (context, snapshot) {
        if (snapshot.connectionState == ConnectionState.done) {
          if (snapshot.hasError) {
            return Text('Error: ${snapshot.error}');
          }
          return box.DeferredBox();
        }
        return const CircularProgressIndicator();
      },
    );
  }
}
```

The `loadLibrary()` function returns a `Future<void>`
that completes successfully when the code in the library
is available for use and completes with an error otherwise.
All usage of symbols from the deferred library should be
guarded behind a completed `loadLibrary()` call. All imports
of the library must be marked as `deferred` for it to be
compiled appropriately to be used in a deferred component.
If a component has already been loaded, additional calls
to `loadLibrary()` complete quickly (but not synchronously).
The `loadLibrary()` function can also be called early to
trigger a pre-load to help mask the loading time.

`loadLibrary()` 函式返回一個 `Future<void>` 物件，
該物件會在延遲庫中的程式碼可用時成功返回，否則返回一個錯誤。
延遲庫中所有的符號在使用之前都應確保 `loadLibrary()` 已經完成。
所有匯入的函式庫都必須透過 `deferred` 標記，以便對其進行適當的編譯以及在延遲元件中使用。
如果元件已經被載入，再次呼叫 `loadLibrary` 將快速返回（但不是同步完成）。
也可以提前呼叫 `loadLibrary()` 函式進行預載入，以幫助遮蔽載入時間。

You can find another example of deferred import loading in
[Flutter Gallery's lib/deferred_widget.dart][].

你可以在
[Flutter Gallery 的 lib/deferred_widget.dart 檔案][Flutter Gallery's lib/deferred_widget.dart]
中找到其他延遲載入元件的範例。

</li>
</ol>

### Step 3: Building the app

### 步驟 3：建構應用程式

Use the following `flutter` command to build a deferred
components app:

使用以下 `flutter` 命令建構延遲元件應用：

```terminal
$ flutter build appbundle
```

This command assists you by validating that your project
is properly set up to build deferred components apps.
By default, the build fails if the validator detects
any issues and guides you through suggested changes to fix them.

此命令會幫助你檢查專案是否正確設定為建構延遲元件應用。
預設情況下，驗證程式檢測到任何問題都會導致建構失敗，你可以透過系統建議的更改來修復這些問題。

{{site.alert.note}}

  You can opt out of building deferred components
  with the `--no-deferred-components` flag.
  This flag causes all assets defined under
  deferred components to be treated as if they were
  defined under the assets section of `pubspec.yaml`.
  All Dart code is compiled into a single shared library
  and `loadLibrary()` calls complete in the next event
  loop boundary (as soon as possible while being asynchronous).
  This flag is also equivalent to omitting the `deferred-components:`
  entry in `pubspec.yaml`.
  
  你可以使用 `--no-deferred-components` 標誌禁用建構延遲元件。
  這個標誌會讓 `pubspec.yaml` 中定義的所有延遲元件，被視為定義在 assets 部分的普通元件。
  所有 Dart 程式碼會被編譯到一個共享庫中，`loadLibrary()` 呼叫會在下一個事件迴圈中完成（非同步時儘快完成）。
  此標誌也等效於移除 `pubspec.yaml` 中的 `deferred-components:`。
  
{{site.alert.end}}

<ol markdown="1">
<li markdown="1"><a id="step-3.1"></a>
<p markdown="1">The
    `flutter build appbundle` command
    runs the validator and attempts to build the app with
    `gen_snapshot` instructed to produce split AOT shared libraries
    as separate `.so` files. On the first run, the validator will
    likely fail as it detects issues; the tool makes
    recommendations for how to set up the project and fix these issues.</p><p markdown="1">`flutter build appbundle` 命令會嘗試建構應用，
    透過 `gen_snapshot` 將應用中拆分的 AOT 共享庫分割為單獨的 `.so` 檔案。
    第一次執行時，驗證程式可能會在檢測到問題時失敗，
    該工具會為如何設定專案和解決這些問題提供建議。</p>
    
The validator is split into two sections: prebuild
and post-gen_snapshot validation. This is because any
validation referencing loading units cannot be performed
until `gen_snapshot` completes and produces a final set
of loading units.

驗證程式分為兩個部分：預建構和產生快照後的驗證。
這是因為在 `gen_snapshot` 完成並產生最後一組載入單元之前，無法執行任何參考載入單元的驗證。

{{site.alert.note}}

  You can opt to have the tool attempt to build your
  app without the validator by passing the
  `--no-validate-deferred-components` flag.
  This can result in unexpected and confusing
  instructions to resolve failures.
  This flag is meant to be used in
  custom implementations that do not rely on the default
  Play-store-based implementation that the validator checks for.
  
  你可以透過 `--no-validate-deferred-components` 標誌，來讓工具嘗試在不執行驗證程式下建構應用。
  這可能導致由意外和錯誤的指令而引起的故障。
  此標誌應當僅在不需要依賴驗證程式檢查的預設 Play-store-based 的自訂實現時使用。
  
{{site.alert.end}}

The validator detects any new, changed, or removed
loading units generated by `gen_snapshot`.
The current generated loading units are tracked in your
`<projectDirectory>/deferred_components_loading_units.yaml` file.
This file should be checked into source control to ensure that
changes to the loading units by other developers can be caught.

驗證程式會檢測 `gen_snapshot` 產生的所有新增、修改或者刪除的載入單元。
當前產生的載入單元記錄在 `<projectDirectory>/deferred_components_loading_units.yaml` 檔案中。
這個檔案應該加入到版本管理中，以確保其他開發人員對載入單元所做的更改可被追蹤。

The validator also checks for the following in the
`android` directory:

驗證程式還會檢查 `android` 目錄中的以下內容：

<ul markdown="1">
<li markdown="1"><p markdown="1">**`<projectDir>/android/app/src/main/res/values/strings.xml`**<br>
    An entry for every deferred component mapping the key
    `${componentName}Name` to `${componentName}`.
    This string resource is used by the `AndroidManifest.xml`
    of each feature module to define the `dist:title property`.
    For example:</p><p markdown="1">每個延遲元件名稱的鍵值對對映 `${componentName}Name`：`${componentName}`。
    每個功能模組的 `AndroidManifest.xml` 使用此字串資源來定義 `dist:title property`。例如：</p>

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  ...
  <string name="boxComponentName">boxComponent</string>
</resources>
```
</li>

<li markdown="1"><p markdown="1">**`<projectDir>/android/<componentName>`**<br>
    An Android dynamic feature module for
    each deferred component exists and contains a `build.gradle`
    and `src/main/AndroidManifest.xml` file.
    This only checks for existence and does not validate
    the contents of these files. If a file does not exist,
    it generates a default recommended one.</p><p markdown="1">每個延遲元件都有一個 Android 動態功能模組，它包含一個 `build.gradle` 和 `src/main/AndroidManifest.xml` 檔案。
    驗證程式只檢查檔案是否存在，不驗證檔案內容。如果檔案不存在，它將產生一個預設的推薦檔案。</p>
    
</li>

<li markdown="1"><p markdown="1">**`<projectDir>/android/app/src/main/res/values/AndroidManifest.xml`**<br>
    Contains a meta-data entry that encodes
    the mapping between loading units and component name the
    loading unit is associated with. This mapping is used by the
    embedder to convert Dart's internal loading unit id
    to the name of a deferred component to install. For example:</p><p markdown="1">包含一個 meta-data 鍵值對，對載入單元與其關聯的元件名稱之間的對映進行編碼。
    嵌入程式使用此對映將 Dart 的內部載入單元 id 轉換為要安裝的延遲元件的名稱。例如：</p>
    
```js
    ...
    <application
        android:label="MyApp"
        android:name="io.flutter.app.FlutterPlayStoreSplitApplication"
        android:icon="@mipmap/ic_launcher">
        ...
        <meta-data android:name="io.flutter.embedding.engine.deferredcomponents.DeferredComponentManager.loadingUnitMapping" android:value="2:boxComponent"/>
    </application>
    ...
```
</li>
</ul>

The `gen_snapshot` validator won't run until the prebuild
validator passes.

`gen_snapshot` 驗證程式在預建構驗證透過之前不會執行。

</li>

<li markdown="1"><p markdown="1">For each of these checks,
    the tool produces the modified or new files
    needed to pass the check.
    These files are placed in the
    `<projectDir>/build/android_deferred_components_setup_files` directory.
    It is recommended that the changes be applied by
    copying and overwriting the same files in the
    project's `android` directory. Before overwriting,
    the current project state should be committed to
    source control and the recommended changes should be
    reviewed to be appropriate. The tool won't make any
    changes to your `android/` directory automatically.</p><p markdown="1">對於每個檢查，該工具會建立或者修改需要的檔案。
    這些檔案放在 `<projectDir>/build/android_deferred_components_setup_files` 目錄下。
    建議透過複製和覆蓋專案 `android` 目錄中的相同檔案來應用更改。
    在覆蓋之前，當前的專案狀態應該被提交到原始碼管理中，並檢查建議的改動。
    該工具不會自動更改 `android` 目錄。</p>
</li>

<li markdown="1"><a id="step-3.3"></a>
<p markdown="1">Once the available
    loading units are generated and logged in
    `<projectDirectory>/deferred_components_loading_units.yaml`,
    it is possible to fully configure the pubspec's
    `deferred-components` section so that the loading units
    are assigned to deferred components as desired.
    To continue with the box example, the generated
    `deferred_components_loading_units.yaml` file would contain:</p><p markdown="1">一旦產生可用的載入單元並將其記錄到 `<projectDirectory>deferred_components_loading_units.yaml` 中，
    便可完善 pubspec 的 `deferred-components` 配置，將載入單元分配給延遲的元件。
    在上面的案例中，產生的 `deferred_components_loading_units.yaml` 檔案將包含：</p>
    
```yaml
loading-units:
  - id: 2
    libraries:
      - package:MyAppName/box.Dart
```

The loading unit id ('2' in this case) is used
internally by Dart, and can be ignored.
The base loading unit (id '1') is not listed
and contains everything not explicitly contained
in another loading unit.

載入單元 id（在本例中為「2」）由 Dart 內部使用，可以忽略。
基本載入單元（id 為「1」）包含了其他載入單元中未顯式列出的所有內容，在這裡沒有列出。

You can now add the following to `pubspec.yaml`:

現在可以將以下內容新增到 `pubspec.yaml` 中：

```yaml
...
flutter:
  ...
  deferred-components:
    - name: boxComponent
      libraries:
        - package:MyAppName/box.Dart
  ...
```
To assign a loading unit to a deferred component,
add any Dart lib in the loading unit into the
libraries section of the feature module.
Keep the following guidelines in mind:

將載入單元分配到延遲元件，把載入單元中的任何 Dart 庫新增到功能模組的 libraries 部分。
請記住以下準則：

<ul markdown="1">

<li markdown="1"><p markdown="1">Loading units should not be included
    in more than one component.</p><p markdown="1">一個載入單元只能包含在一個延遲元件中</p>
</li>
<li markdown="1"><p markdown="1">Including one Dart library from a
    loading unit indicates that the entire loading
    unit is assigned to the deferred component.</p><p markdown="1">參考載入單元中的一個 Dart 庫意味著整個載入單元都被包含在延遲元件中。</p>
</li>
<li markdown="1"><p markdown="1">All loading units not assigned to
    a deferred component are included in the base component,
    which always exists implicitly.</p><p markdown="1">所有未被分配給延遲元件的載入單元都包含在基本元件中，基本元件始終隱含存在。</p>
</li>
<li markdown="1"><p markdown="1">Loading units assigned to the same
    deferred component are downloaded, installed,
    and shipped together.</p><p>分配給同一延遲元件的載入單元將一起下載、安裝和執行。</p>
</li>
<li markdown="1"><p markdown="1">The base component is implicit and
    need not be defined in the pubspec.</p><p>基本元件是隱含的，不需要在 pubspec 中定義。</p>
</li>
</ul>
</li>

<li markdown="1"><p markdown="1">Assets can also be included by adding
    an assets section in the deferred component configuration:</p><p>靜態資源也可以透過在延遲元件中配置 assets 進行新增 ：</p>
    
```yaml
  deferred-components:
    - name: boxComponent
      libraries:
        - package:MyAppName/box.Dart
      assets:
        - assets/image.jpg
        - assets/picture.png
          # wildcard directory
        - assets/gallery/
```

An asset can be included in multiple deferred components,
but installing both components results in a replicated asset.
Assets-only components can also be defined by omitting the
libraries section. These assets-only components must be
installed with the [`DeferredComponent`][] utility class in
services rather than `loadLibrary()`.
Since Dart libs are packaged together with assets,
if a Dart library is loaded with `loadLibrary()`,
any assets in the component are loaded as well.
However, installing by component name and the services utility
won't load any dart libraries in the component.

一個靜態資源可以包含在多個延遲元件中，但是安裝這兩個元件會導致資源的重複。
也可以透過省略 libraries 來定義純靜態資源的延遲元件。
這些靜態資源的元件必須與服務中的 [`DeferredComponent`][] 實用程式類一起安裝，而不是 `loadLibrary()`。
由於 Dart 函式庫是與靜態資源打包在一起的，因此如果用 `loadLibrary()` 載入 Dart 庫，則也會載入元件中的所有資源。
但是，按元件名稱和服務實用程式來安裝不會載入元件中的任何 Dart 庫。

You are free to include assets in any component,
as long as they are installed and loaded when they
are first referenced, though typically,
assets and the Dart code that uses those assets
are best packed in the same component.

你可以自由選擇將資源包含在任何元件中，只要它們是在首次參考時安裝和載入的，
但通常情況下，靜態資源和使用這些資源的 Dart 程式碼最好打包在同一組件中。

</li>

<li markdown="1"><p markdown="1">Manually add all deferred components
    that you defined in `pubspec.yaml` into the
    `android/settings.gradle` file as includes.
    For example, if there are three deferred components
    defined in the pubspec named, `boxComponent`, `circleComponent`,
    and `assetComponent`, ensure that `android/settings.gradle`
    contains the following:</p><p markdown="1">將在 `pubspec.yaml` 中定義的所有延遲元件手動新增到 `android/settings.gradle` 檔案中的 includes 部分。
    例如，如果 pubspec 中定義了三個名為 `boxComponent`、 `circleComponent` 和 `assetComponent` 的延遲元件，
    請確保 `android/settings.gradle` 中包含以下內容：</p>
    
```gradle
include ':app', ':boxComponent', ':circleComponent', ':assetComponent'
...
```
</li>

<li markdown="1"><p markdown="1">Repeat steps [3.1][] through 3.6 (this step)
    until all validator recommendations are handled and the tool
    runs without further recommendations.</p><p markdown="1">重複步驟 [3.1][] 到 3.6（此步驟），
    直到處理了所有驗證程式的建議，並且該工具在沒有更多建議的情況下執行。</p>

When successful, this command outputs an `app-release.aab`
file in `build/app/outputs/bundle/release`.

成功時，此命令將在 `build/app/outputs/bundle/release` 目錄下輸出 `app-release.aab` 檔案。

A successful build does not always mean the app was
built as intended. It is up to you to ensure that all loading
units and Dart libraries are included in the way you intended.
For example, a common mistake is accidentally importing a
Dart library without the `deferred` keyword,
resulting in a deferred library being compiled as part of
the base loading unit. In this case, the Dart lib would
load properly because it is always present in the base,
and the lib would not be split off. This can be checked
by examining the `deferred_components_loading_units.yaml`
file to verify that the generated loading units are described
as intended.

建構成功並非總是意味著應用是按預期建構的。
你需要確保所有的載入單元和 Dart 庫都以你想要的方式包含在內。
例如，一個常見的錯誤是不小心匯入了一個沒有 `deferred` 關鍵字的 Dart 庫，
導致一個延遲載入庫被編譯為基本載入單元的一部分。
在這種情況下，Dart 庫將正確載入，因為它始終存在於基本元件中，並且庫不會被拆分。
可以透過檢查 `deferred_components_loading_units.yaml` 檔案，
驗證預期的載入單元是否產生描述。

When adjusting the deferred components configurations,
or making Dart changes that add, modify, or remove loading units,
you should expect the validator to fail.
Follow steps [3.1][] through 3.6 (this step) to apply any
recommended changes to continue the build.

當調整延遲元件配置，或者進行新增、修改、刪除載入單元的更改時，
你應該預料到驗證程式會失敗。按照步驟 [3.1][] 到 3.6（此步驟）中的所有建議繼續建構。

</li>
</ol>

### Running the app locally

### 在本地執行應用

Once your app has successfully built an `.aab` file,
use Android's [`bundletool`][] to perform
local testing with the `--local-testing` flag.

一旦你的應用程式成功建構了一個 `.aab` 檔案，
就可以使用 Android 的 [`bundletool`][] 來執行帶有 `--local testing` 標誌的本地測試。

To run the `.aab` file on a test device,
download the bundletool jar executable from
[github.com/google/bundletool/releases][] and run:

要在測試裝置上執行 `.aab` 檔案，請從
[github.com/google/bundletool/releases][] 下載
bundletool jar 可執行檔案，然後執行：

```terminal
$ java -jar bundletool.jar build-apks --bundle=<your_app_project_dir>/build/app/outputs/bundle/release/app-release.aab --output=<your_temp_dir>/app.apks --local-testing

$ java -jar bundletool.jar install-apks --apks=<your_temp_dir>/app.apks
```

Where `<your_app_project_dir>` is the path to your app's
project directory and `<your_temp_dir>` is any temporary
directory used to store the outputs of bundletool.
This unpacks your `.aab` file into an `.apks` file and
installs it on the device. All available Android dynamic
features are loaded onto the device locally and
installation of deferred components is emulated.

`<your_app_project_dir>` 是應用程式對應專案的目錄位置，
 `<your_temp_dir>` 用於儲存 bundletool 輸出的所有臨時目錄。
這會將你的 `.aab` 檔案解壓為 `.apks` 檔案並將其安裝到裝置上。
所有可用的 Android 動態特性都已在本地裝置上載入，並模擬了延遲元件的安裝。

Before running `build-apks` again,
remove the existing app .apks file:

再次執行 `build-apks` 之前，
請刪除已存在的 .apks 檔案：

```terminal
$ rm <your_temp_dir>/app.apks
```

Changes to the Dart codebase require either incrementing
the Android build ID or uninstalling and reinstalling
the app, as Android won't update the feature modules
unless it detects a new version number.

對 Dart 程式碼庫的更改需要增加 Android 建構 ID，或者解除安裝並重新安裝應用程式。
因為只有檢測到新的版本號，Android 才會去更新功能模組。

### Releasing to the Google Play store

### 釋出到 Google Play 商店

The built `.aab` file can be uploaded directly to
the Play store as normal. When `loadLibrary()` is called,
the needed Android module containing the Dart AOT lib and
assets is downloaded by the Flutter engine using the
Play store's delivery feature.

產生的 `.aab` 檔案可以像平常一樣直接上傳到 Google Play 商店。
呼叫 `loadLibrary()` 時，Flutter 引擎將會使用從商店下載的包含 Dart AOT 庫和資源的 Android 模組。

[3.1]: #step-3.1
[Android docs]: {{site.android-dev}}/guide/playcore/feature-delivery#declare_splitcompatapplication_in_the_manifest
[`bundletool`]: {{site.android-dev}}/studio/command-line/bundletool
[Deferred Components]: {{site.repo.flutter}}/wiki/Deferred-Components
[`DeferredComponent`]: {{site.api}}/flutter/services/DeferredComponent-class.html
[dynamic feature modules]: {{site.android-dev}}/guide/playcore/feature-delivery
[Flutter Gallery's lib/deferred_widget.dart]: {{site.repo.gallery}}/blob/main/lib/deferred_widget.dart
[Flutter wiki]: {{site.repo.flutter}}/wiki
[github.com/google/bundletool/releases]: {{site.github}}/google/bundletool/releases
[lazily loading a library]: {{site.dart-site}}/language/libraries#lazily-loading-a-library
[release or profile mode]: {{site.url}}/testing/build-modes
[step 3.3]: #step-3.3
