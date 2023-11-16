---
title: Add a Flutter screen to an iOS app
title: 在 iOS 應用中新增 Flutter 頁面
short-title: Add a Flutter screen
short-title: 新增 Flutter 頁面
description: Learn how to add a single Flutter screen to your existing iOS app.
description: 瞭解如何在現有 iOS 應用中新增單個 Flutter 頁面。
tags: Flutter混合工程,add2app
keywords: iOS,Flutter頁面,FlutterEngine
---

This guide describes how to add a single Flutter screen to an existing iOS app.

本指南描述了怎樣在既有 iOS 應用中新增單個 Flutter 頁面。

## Start a FlutterEngine and FlutterViewController

## 啟動 FlutterEngine 和 FlutterViewController

To launch a Flutter screen from an existing iOS, you start a
[`FlutterEngine`][] and a [`FlutterViewController`][].

為了在既有 iOS 應用中展示 Flutter 頁面，
請啟動 [`FlutterEngine`][] 和 [`FlutterViewController`][]。

{{site.alert.secondary}}

  The `FlutterEngine` serves as a host to the Dart VM and your Flutter runtime,
  and the `FlutterViewController` attaches to a `FlutterEngine` to pass 
  input events into Flutter and to display frames rendered by the
  `FlutterEngine`.
  
  `FlutterEngine` 充當 Dart VM 和 Flutter 執行時的主機；
  `FlutterViewController` 依附於 `FlutterEngine`，
  給 Flutter 傳遞 UIKit 的輸入事件，並展示被 `FlutterEngine` 渲染的每一幀畫面。
{{site.alert.end}}

The `FlutterEngine` might have the same lifespan as your
`FlutterViewController` or outlive your `FlutterViewController`.

`FlutterEngine` 的壽命可能與 `FlutterViewController` 相同，也可能超過 `FlutterViewController`。

{{site.alert.tip}}

  It's generally recommended to pre-warm a long-lived
  `FlutterEngine` for your application because:

  通常建議為您的應用預熱一個“長壽”的 `FlutterEngine` 是因為:
  
  * The first frame appears faster when showing the `FlutterViewController`.
    
    當展示 `FlutterViewController` 時，第一幀畫面將會更快展現；
  
  * Your Flutter and Dart state will outlive one `FlutterViewController`.
  
    你的 Flutter 和 Dart 狀態將比一個`FlutterViewController` 存活更久；

  * Your application and your plugins can interact with Flutter and your Dart
    logic before showing the UI.

    在展示 UI 前，你的應用和 plugins 可以與 Flutter 和 Dart 邏輯互動。
{{site.alert.end}}

See [Loading sequence and performance][]
for more analysis on the latency and memory
trade-offs of pre-warming an engine.

[載入順序和效能]({{site.url}}/development/add-to-app/performance)
裡有更多關於預熱 engine 的延遲和記憶體取捨的分析。

### Create a FlutterEngine

### 建立一個 FlutterEngine

Where you create a `FlutterEngine` depends on your host app.

在哪建立 `FlutterEngine` 取決於你要用的宿主型別。

{% samplecode engine %}

{% sample SwiftUI %}
In this example, we create a `FlutterEngine` object inside a SwiftUI `ObservableObject`. 
We then pass this `FlutterEngine` into a `ContentView` using the 
 `environmentObject()` property. 

在這個例子中，我們在 SwiftUI 的 `ObservableObject` 中建立了一個 `FlutterEngine` 物件。
然後我們使用 `environmentObject()` 屬性將這個 `FlutterEngine` 傳遞給了 `ContentView`。

 <?code-excerpt title="MyApp.swift"?>
 ```swift
import SwiftUI
import Flutter
// The following library connects plugins with iOS platform code to this app.
import FlutterPluginRegistrant

class FlutterDependencies: ObservableObject {
  let flutterEngine = FlutterEngine(name: "my flutter engine")
  init(){
    // Runs the default Dart entrypoint with a default Flutter route.
    flutterEngine.run()
    // Connects plugins with iOS platform code to this app.
    GeneratedPluginRegistrant.register(with: self.flutterEngine);
  }
}

@main
struct MyApp: App {
  // flutterDependencies will be injected using EnvironmentObject.
  @StateObject var flutterDependencies = FlutterDependencies()
    var body: some Scene {
      WindowGroup {
        ContentView().environmentObject(flutterDependencies)
      }
    }
}
```

{% sample UIKit-Swift %}
As an example, we demonstrate creating a
`FlutterEngine`, exposed as a property, on app startup in
the app delegate.

這個例子中，我們在應用啟動時的 App Delegate 中建立了一個 `FlutterEngine`
並作為屬性暴露給外界。

<?code-excerpt title="AppDelegate.swift"?>
```swift
import UIKit
import Flutter
// The following library connects plugins with iOS platform code to this app.
import FlutterPluginRegistrant

@UIApplicationMain
class AppDelegate: FlutterAppDelegate { // More on the FlutterAppDelegate.
  lazy var flutterEngine = FlutterEngine(name: "my flutter engine")

  override func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    // Runs the default Dart entrypoint with a default Flutter route.
    flutterEngine.run();
    // Connects plugins with iOS platform code to this app.
    GeneratedPluginRegistrant.register(with: self.flutterEngine);
    return super.application(application, didFinishLaunchingWithOptions: launchOptions);
  }
}
```
{% sample UIKit-ObjC %}

In this example, we create a `FlutterEngine` 
object inside a SwiftUI `ObservableObject`. 
We then pass this `FlutterEngine` into a 
`ContentView` using the `environmentObject()` property.

在這個例子中，我們在 SwiftUI 的 `ObservableObject`
中建立了一個 `FlutterEngine` 物件。
然後我們使用 `environmentObject()` 屬性將這個
`FlutterEngine` 傳遞給 `ContentView`。

<?code-excerpt title="AppDelegate.h"?>
```objectivec
@import UIKit;
@import Flutter;

@interface AppDelegate : FlutterAppDelegate // More on the FlutterAppDelegate below.
@property (nonatomic,strong) FlutterEngine *flutterEngine;
@end
```

<?code-excerpt title="AppDelegate.m"?>
```objectivec
// The following library connects plugins with iOS platform code to this app.
#import <FlutterPluginRegistrant/GeneratedPluginRegistrant.h>

#import "AppDelegate.h"

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application
    didFinishLaunchingWithOptions:(NSDictionary<UIApplicationLaunchOptionsKey, id> *)launchOptions {
  self.flutterEngine = [[FlutterEngine alloc] initWithName:@"my flutter engine"];
  // Runs the default Dart entrypoint with a default Flutter route.
  [self.flutterEngine run];
  // Connects plugins with iOS platform code to this app.
  [GeneratedPluginRegistrant registerWithRegistry:self.flutterEngine];
  return [super application:application didFinishLaunchingWithOptions:launchOptions];
}

@end
```
{% endsamplecode %}

### Show a FlutterViewController with your FlutterEngine

### 使用 FlutterEngine 展示 FlutterViewController

{% samplecode vc %}

{% sample SwiftUI %}
The following example shows a generic `ContentView` with a
`Button` hooked to present a [`FlutterViewController`][].
The `FlutterViewController` constructor takes the pre-warmed 
`FlutterEngine` as an argument. `FlutterEngine` is passed in 
as an `EnvironmentObject` via `flutterDependencies`.

下面的例子中展示了在一個常見的 `ContentView`，
包含一個能跳轉到 [`FlutterViewController`][] 的 `Button`，
`FlutterViewController` 的建構函式會接收一個預熱過的
`FlutterEngine` 作為引數，`FlutterEngine` 透過
`flutterDependencies` 作為 `EnvironmentObject` 傳入。

<?code-excerpt title="ContentView.swift"?>
```swift
import SwiftUI
import Flutter

struct ContentView: View {
  // Flutter dependencies are passed in an EnvironmentObject.
  @EnvironmentObject var flutterDependencies: FlutterDependencies

  // Button is created to call the showFlutter function when pressed.
  var body: some View {
    Button("Show Flutter!") {
      showFlutter()
    }
  }

func showFlutter() {
    // Get the RootViewController.
    guard
      let windowScene = UIApplication.shared.connectedScenes
        .first(where: { $0.activationState == .foregroundActive && $0 is UIWindowScene }) as? UIWindowScene,
      let window = windowScene.windows.first(where: \.isKeyWindow),
      let rootViewController = window.rootViewController
    else { return }

    // Create the FlutterViewController.
    let flutterViewController = FlutterViewController(
      engine: flutterDependencies.flutterEngine,
      nibName: nil,
      bundle: nil)
    flutterViewController.modalPresentationStyle = .overCurrentContext
    flutterViewController.isViewOpaque = false

    rootViewController.present(flutterViewController, animated: true)
  }
}
```

{% sample UIKit-Swift %}
The following example shows a generic `ViewController` with a
`UIButton` hooked to present a [`FlutterViewController`][].
The `FlutterViewController` uses the `FlutterEngine` instance
created in the `AppDelegate`.

下面的例子展示了一個普通的 `ViewController`，
包含一個能跳轉到 [`FlutterViewController`][] 的 `UIButton`，這個
`FlutterViewController` 使用在 `AppDelegate`
中建立的 Flutter 引擎 (`FlutterEngine`)。

<?code-excerpt title="ViewController.swift"?>
```swift
import UIKit
import Flutter

class ViewController: UIViewController {
  override func viewDidLoad() {
    super.viewDidLoad()

    // Make a button to call the showFlutter function when pressed.
    let button = UIButton(type:UIButton.ButtonType.custom)
    button.addTarget(self, action: #selector(showFlutter), for: .touchUpInside)
    button.setTitle("Show Flutter!", for: UIControl.State.normal)
    button.frame = CGRect(x: 80.0, y: 210.0, width: 160.0, height: 40.0)
    button.backgroundColor = UIColor.blue
    self.view.addSubview(button)
  }

  @objc func showFlutter() {
    let flutterEngine = (UIApplication.shared.delegate as! AppDelegate).flutterEngine
    let flutterViewController =
        FlutterViewController(engine: flutterEngine, nibName: nil, bundle: nil)
    present(flutterViewController, animated: true, completion: nil)
  }
}
```

{% sample UIKit-ObjC %}
The following example shows a generic `ViewController` with a
`UIButton` hooked to present a [`FlutterViewController`][].
The `FlutterViewController` uses the `FlutterEngine` instance
created in the `AppDelegate`.

下面的例子中展示了在一個常見的 `ViewController`，
包含一個能跳轉到 [`FlutterViewController`][] 的 `UIButton`，
`FlutterViewController` 會使用在 `AppDelegate`
中建立的 `FlutterEngine` 例項。

<?code-excerpt title="ViewController.m"?>
```objectivec
@import Flutter;
#import "AppDelegate.h"
#import "ViewController.h"

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];

    // Make a button to call the showFlutter function when pressed.
    UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
    [button addTarget:self
               action:@selector(showFlutter)
     forControlEvents:UIControlEventTouchUpInside];
    [button setTitle:@"Show Flutter!" forState:UIControlStateNormal];
    button.backgroundColor = UIColor.blueColor;
    button.frame = CGRectMake(80.0, 210.0, 160.0, 40.0);
    [self.view addSubview:button];
}

- (void)showFlutter {
    FlutterEngine *flutterEngine =
        ((AppDelegate *)UIApplication.sharedApplication.delegate).flutterEngine;
    FlutterViewController *flutterViewController =
        [[FlutterViewController alloc] initWithEngine:flutterEngine nibName:nil bundle:nil];
    [self presentViewController:flutterViewController animated:YES completion:nil];
}
@end
```
{% endsamplecode %}

Now, you have a Flutter screen embedded in your iOS app.

現在，你的 iOS 應用中集成了一個 Flutter 頁面。

{{site.alert.note}}

  Using the previous example, the default `main()`
  entrypoint function of your default Dart library
  would run when calling `run` on the
  `FlutterEngine` created in the `AppDelegate`.
  
  在上一個例子中，你的預設 Dart 庫的預設入口函式 `main()`，
  將會在 `AppDelegate` 建立 `FlutterEngine` 並呼叫 `run` 方法時呼叫。
{{site.alert.end}}

### _Alternatively_ - Create a FlutterViewController with an implicit FlutterEngine

### **或者** —— 使用隱含 FlutterEngine 建立 FlutterViewController

As an alternative to the previous example, you can let the
`FlutterViewController` implicitly create its own `FlutterEngine` without
pre-warming one ahead of time.

上一個範例還有另一個選擇，你可以讓 `FlutterViewController`
隱含建立它自己的 `FlutterEngine`，而不用提前預熱 engine。

This is not usually recommended because creating a
`FlutterEngine` on-demand could introduce a noticeable
latency between when the `FlutterViewController` is
presented and when it renders its first frame. This could, however, be
useful if the Flutter screen is rarely shown, when there are no good
heuristics to determine when the Dart VM should be started, and when Flutter
doesn't need to persist state between view controllers.

不過不建議這樣做，因為按需建立`FlutterEngine` 的話，
在 `FlutterViewController` 被 present 出來之後，
第一幀圖像渲染完之前，將會引入明顯的延遲。
但是當 Flutter 頁面很少被展示時，當對決定何時啟動 Dart VM 沒有好的啟發時，
當 Flutter 無需在頁面（view controller）之間保持狀態時，
此方式可能會有用。

To let the `FlutterViewController` present without an existing
`FlutterEngine`, omit the `FlutterEngine` construction, and create the
`FlutterViewController` without an engine reference.

為了不使用已經存在的 `FlutterEngine` 來展現 `FlutterViewController`，
省略 `FlutterEngine` 的建立步驟，
並且在建立 `FlutterViewController` 時，去掉 engine 的參考。

{% samplecode no-engine-vc %}
{% sample SwiftUI %}
```swift
import SwiftUI
import Flutter

struct ContentView: View {
  var body: some View {
    Button("Show Flutter!") {
      openFlutterApp()
    }
  }

func openFlutterApp() {
    // Get the RootViewController.
    guard
      let windowScene = UIApplication.shared.connectedScenes
        .first(where: { $0.activationState == .foregroundActive && $0 is UIWindowScene }) as? UIWindowScene,
      let window = windowScene.windows.first(where: \.isKeyWindow),
      let rootViewController = window.rootViewController
    else { return }

    // Create the FlutterViewController without an existing FlutterEngine.
    let flutterViewController = FlutterViewController(
      project: nil,
      nibName: nil,
      bundle: nil)
    flutterViewController.modalPresentationStyle = .overCurrentContext
    flutterViewController.isViewOpaque = false

    rootViewController.present(flutterViewController, animated: true)
  }
}
```
{% sample UIKit-Swift %}
<?code-excerpt title="ViewController.swift"?>
```swift
// Existing code omitted.
func showFlutter() {
  let flutterViewController = FlutterViewController(project: nil, nibName: nil, bundle: nil)
  present(flutterViewController, animated: true, completion: nil)
}
```
{% sample UIKit-ObjC %}
<?code-excerpt title="ViewController.m"?>
```objectivec
// Existing code omitted.
- (void)showFlutter {
  FlutterViewController *flutterViewController =
      [[FlutterViewController alloc] initWithProject:nil nibName:nil bundle:nil];
  [self presentViewController:flutterViewController animated:YES completion:nil];
}
@end
```
{% endsamplecode %}

See [Loading sequence and performance][]
for more explorations on latency and memory usage.

檢視 [載入順序和效能][Loading sequence and performance]
瞭解更多關於延遲和記憶體使用的探索。

## Using the FlutterAppDelegate

## 使用 FlutterAppDelegate

Letting your application's `UIApplicationDelegate` subclass
`FlutterAppDelegate` is recommended but not required.

推薦讓你應用的 `UIApplicationDelegate` 繼承 `FlutterAppDelegate`，但不是必須的。

The `FlutterAppDelegate` performs functions such as:

`FlutterAppDelegate` 有這些功能：

* Forwarding application callbacks such as [`openURL`][]
  to plugins such as [local_auth][].

  傳遞應用的回呼(Callback)，例如 [`openURL`][] 到 Flutter 的外掛 —— [local_auth][]。

* Keeping the Flutter connection open 
  in debug mode when the phone screen locks.

  當手機螢幕鎖定時，在除錯模式下保持 Flutter 連線處於開啟狀態。

### Creating a FlutterAppDelegate subclass

### 建立 FlutterAppDelegate 子類別

Creating a subclass of the the `FlutterAppDelegate` in UIKit apps was shown 
in the [Start a FlutterEngine and FlutterViewController section][]. 
In a SwiftUI app, you can create a subclass of the 
`FlutterAppDelegate` that conforms to the `ObservableObject` protocol as follows:

[啟動 FlutterEngine 和 FlutterViewController][Start a FlutterEngine and FlutterViewController section]
文件中展示瞭如何在使用 UIKit 的應用中建立 `FlutterAppDelegate` 子類別。
在使用 SwiftUI 的應用中，你可以建立一個符合 `ObservableObject` 協議的
`FlutterAppDelegate` 的子類別，如下所示：

```swift
import SwiftUI
import Flutter
import FlutterPluginRegistrant

class AppDelegate: FlutterAppDelegate, ObservableObject {
  let flutterEngine = FlutterEngine(name: "my flutter engine")

  override func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
      // Runs the default Dart entrypoint with a default Flutter route.
      flutterEngine.run();
      // Used to connect plugins (only if you have plugins with iOS platform code).
      GeneratedPluginRegistrant.register(with: self.flutterEngine);
      return true;
    }
}

@main
struct MyApp: App {
//  Use this property wrapper to tell SwiftUI
//  it should use the AppDelegate class for the application delegate
  @UIApplicationDelegateAdaptor(AppDelegate.self) var appDelegate

  var body: some Scene {
      WindowGroup {
        ContentView()
      }
  }
}
```

Then, in your view, the `AppDelegate`is accessible as an `EnvironmentObject`.

```swift
import SwiftUI
import Flutter

struct ContentView: View {
  // Access the AppDelegate using an EnvironmentObject.
  @EnvironmentObject var appDelegate: AppDelegate

  var body: some View {
    Button("Show Flutter!") {
      openFlutterApp()
    }
  }

func openFlutterApp() {
    // Get the RootViewController.
    guard
      let windowScene = UIApplication.shared.connectedScenes
        .first(where: { $0.activationState == .foregroundActive && $0 is UIWindowScene }) as? UIWindowScene,
      let window = windowScene.windows.first(where: \.isKeyWindow),
      let rootViewController = window.rootViewController
    else { return }

    // Create the FlutterViewController.
    let flutterViewController = FlutterViewController(
      // Access the Flutter Engine via AppDelegate.
      engine: appDelegate.flutterEngine,
      nibName: nil,
      bundle: nil)
    flutterViewController.modalPresentationStyle = .overCurrentContext
    flutterViewController.isViewOpaque = false

    rootViewController.present(flutterViewController, animated: true)
  }
}

```

### If you can't directly make FlutterAppDelegate a subclass

### 如果不能直接讓 FlutterAppDelegate 成為子類別

If your app delegate can't directly make `FlutterAppDelegate` a subclass,
make your app delegate implement the `FlutterAppLifeCycleProvider`
protocol in order to make sure your plugins receive the necessary callbacks.
Otherwise, plugins that depend on these events might have undefined behavior.

如果你的 app delegate 不能直接繼承 `FlutterAppDelegate`，
讓你的 app delegate 實現 `FlutterAppLifeCycleProvider` 協議，
來確保 Flutter plugins 接收到必要的回呼(Callback)。
否則，依賴這些事件的 plugins 將會有無法預估的行為。

For instance:

例如：

{% samplecode app-delegate %}
{% sample Swift %}
<?code-excerpt title="AppDelegate.swift"?>
```swift
import Foundation
import Flutter

class AppDelegate: UIResponder, UIApplicationDelegate, FlutterAppLifeCycleProvider, ObservableObject {

  private let lifecycleDelegate = FlutterPluginAppLifeCycleDelegate()

  let flutterEngine = FlutterEngine(name: "flutter_nps_engine")

  override func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
  func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
    flutterEngine.run()
    return lifecycleDelegate.application(application, didFinishLaunchingWithOptions: launchOptions ?? [:])
  }

  func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    lifecycleDelegate.application(application, didRegisterForRemoteNotificationsWithDeviceToken: deviceToken)
  }

  func application(_ application: UIApplication, didFailToRegisterForRemoteNotificationsWithError error: Error) {
    lifecycleDelegate.application(application, didFailToRegisterForRemoteNotificationsWithError: error)
  }

  func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
    lifecycleDelegate.application(application, didReceiveRemoteNotification: userInfo, fetchCompletionHandler: completionHandler)
  }

  func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
    return lifecycleDelegate.application(app, open: url, options: options)
  }

  func application(_ application: UIApplication, handleOpen url: URL) -> Bool {
    return lifecycleDelegate.application(application, handleOpen: url)
  }

  func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    return lifecycleDelegate.application(application, open: url, sourceApplication: sourceApplication ?? "", annotation: annotation)
  }

  func application(_ application: UIApplication, performActionFor shortcutItem: UIApplicationShortcutItem, completionHandler: @escaping (Bool) -> Void) {
    lifecycleDelegate.application(application, performActionFor: shortcutItem, completionHandler: completionHandler)
  }

  func application(_ application: UIApplication, handleEventsForBackgroundURLSession identifier: String, completionHandler: @escaping () -> Void) {
    lifecycleDelegate.application(application, handleEventsForBackgroundURLSession: identifier, completionHandler: completionHandler)
  }

  func application(_ application: UIApplication, performFetchWithCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
    lifecycleDelegate.application(application, performFetchWithCompletionHandler: completionHandler)
  }

  func add(_ delegate: FlutterApplicationLifeCycleDelegate) {
    lifecycleDelegate.add(delegate)
  }
}
```

{% sample Objective-C %}
<?code-excerpt title="AppDelegate.h"?>
```objectivec
@import Flutter;
@import UIKit;
@import FlutterPluginRegistrant;

@interface AppDelegate : UIResponder <UIApplicationDelegate, FlutterAppLifeCycleProvider>
@property (strong, nonatomic) UIWindow *window;
@property (nonatomic,strong) FlutterEngine *flutterEngine;
@end
```

The implementation should delegate mostly to a
`FlutterPluginAppLifeCycleDelegate`:

在具體實現中，應該最大化地委託給
`FlutterPluginAppLifeCycleDelegate`：

<?code-excerpt title="AppDelegate.m"?>
```objectivec
@interface AppDelegate ()
@property (nonatomic, strong) FlutterPluginAppLifeCycleDelegate* lifeCycleDelegate;
@end

@implementation AppDelegate

- (instancetype)init {
    if (self = [super init]) {
        _lifeCycleDelegate = [[FlutterPluginAppLifeCycleDelegate alloc] init];
    }
    return self;
}

- (BOOL)application:(UIApplication*)application
didFinishLaunchingWithOptions:(NSDictionary<UIApplicationLaunchOptionsKey, id>*))launchOptions {
    self.flutterEngine = [[FlutterEngine alloc] initWithName:@"io.flutter" project:nil];
    [self.flutterEngine runWithEntrypoint:nil];
    [GeneratedPluginRegistrant registerWithRegistry:self.flutterEngine];
    return [_lifeCycleDelegate application:application didFinishLaunchingWithOptions:launchOptions];
}

// Returns the key window's rootViewController, if it's a FlutterViewController.
// Otherwise, returns nil.
- (FlutterViewController*)rootFlutterViewController {
    UIViewController* viewController = [UIApplication sharedApplication].keyWindow.rootViewController;
    if ([viewController isKindOfClass:[FlutterViewController class]]) {
        return (FlutterViewController*)viewController;
    }
    return nil;
}

- (void)application:(UIApplication*)application
didRegisterUserNotificationSettings:(UIUserNotificationSettings*)notificationSettings {
    [_lifeCycleDelegate application:application
didRegisterUserNotificationSettings:notificationSettings];
}

- (void)application:(UIApplication*)application
didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken {
    [_lifeCycleDelegate application:application
didRegisterForRemoteNotificationsWithDeviceToken:deviceToken];
}

- (void)application:(UIApplication*)application
didReceiveRemoteNotification:(NSDictionary*)userInfo
fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler {
    [_lifeCycleDelegate application:application
       didReceiveRemoteNotification:userInfo
             fetchCompletionHandler:completionHandler];
}

- (BOOL)application:(UIApplication*)application
            openURL:(NSURL*)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey, id>*)options {
    return [_lifeCycleDelegate application:application openURL:url options:options];
}

- (BOOL)application:(UIApplication*)application handleOpenURL:(NSURL*)url {
    return [_lifeCycleDelegate application:application handleOpenURL:url];
}

- (BOOL)application:(UIApplication*)application
            openURL:(NSURL*)url
  sourceApplication:(NSString*)sourceApplication
         annotation:(id)annotation {
    return [_lifeCycleDelegate application:application
                                   openURL:url
                         sourceApplication:sourceApplication
                                annotation:annotation];
}

- (void)application:(UIApplication*)application
performActionForShortcutItem:(UIApplicationShortcutItem*)shortcutItem
  completionHandler:(void (^)(BOOL succeeded))completionHandler {
    [_lifeCycleDelegate application:application
       performActionForShortcutItem:shortcutItem
                  completionHandler:completionHandler];
}

- (void)application:(UIApplication*)application
handleEventsForBackgroundURLSession:(nonnull NSString*)identifier
  completionHandler:(nonnull void (^)(void))completionHandler {
    [_lifeCycleDelegate application:application
handleEventsForBackgroundURLSession:identifier
                  completionHandler:completionHandler];
}

- (void)application:(UIApplication*)application
performFetchWithCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler {
    [_lifeCycleDelegate application:application performFetchWithCompletionHandler:completionHandler];
}

- (void)addApplicationLifeCycleDelegate:(NSObject<FlutterPlugin>*)delegate {
    [_lifeCycleDelegate addDelegate:delegate];
}
@end
```

{% endsamplecode %}
## Launch options

## 啟動選項

The examples demonstrate running Flutter using the default launch settings.

例子中展示了使用預設啟動選項執行 Flutter。

In order to customize your Flutter runtime,
you can also specify the Dart entrypoint, library, and route.

為了客製化你的 Flutter 執行時，你也可以置頂 Dart 入口、庫和路由。

### Dart entrypoint

### Dart 入口

Calling `run` on a `FlutterEngine`, by default,
runs the `main()` Dart function
of your `lib/main.dart` file.

在 `FlutterEngine` 上呼叫 `run`，
預設將會呼叫你的 `lib/main.dart`
檔案裡的 `main()` 函式。

You can also run a different entrypoint function by using
[`runWithEntrypoint`][] with an `NSString` specifying
a different Dart function.

你也可以使用另一個入口方法 [`runWithEntrypoint`][]，
並使用 `NSString` 字串指定一個不同的 Dart 入口。

{{site.alert.note}}

  Dart entrypoint functions other than `main()`
  must be annotated with the following in order to
  not be [tree-shaken][] away when compiling:
  
  使用 `main()` 以外的 Dart 入口函式，必須使用下面的註解，
  防止被 [tree-shaken][] 最佳化掉，
  而沒有編譯。

  ```dart
  @pragma('vm:entry-point')
  void myOtherEntrypoint() { ... };
  ```
{{site.alert.end}}

### Dart library

### Dart 庫

In addition to specifying a Dart function, you can specify an entrypoint
function in a specific file.

另外，在指定 Dart 函式時，你可以指定特定檔案的特定函式。

For instance the following runs `myOtherEntrypoint()`
in `lib/other_file.dart` instead of `main()` in `lib/main.dart`:

下面的例子使用 `lib/other_file.dart` 檔案的
`myOtherEntrypoint()` 函式取代 `lib/main.dart` 的 `main()` 函式：

{% samplecode entrypoint-library %}
{% sample Swift %}
```swift
flutterEngine.run(withEntrypoint: "myOtherEntrypoint", libraryURI: "other_file.dart")
```
{% sample Objective-C %}
```objectivec
[flutterEngine runWithEntrypoint:@"myOtherEntrypoint" libraryURI:@"other_file.dart"];
```
{% endsamplecode %}


### Route

### 路由

An initial route can be set for your Flutter [`WidgetsApp`][]
when constructing the engine.

當建構 engine 時，可以為你的 Flutter [`WidgetsApp`][] 設定一個初始路由。

{% samplecode initial-route %}
{% sample Swift %}
```swift
let flutterEngine = FlutterEngine()
// FlutterDefaultDartEntrypoint is the same as nil, which will run main().
engine.run(
  withEntrypoint: "main", initialRoute: "/onboarding")
```
{% sample Objective-C %}
```objectivec
FlutterEngine *flutterEngine = [[FlutterEngine alloc] init];
// FlutterDefaultDartEntrypoint is the same as nil, which will run main().
[flutterEngine runWithEntrypoint:FlutterDefaultDartEntrypoint
                    initialRoute:@"/onboarding"];
```
{% endsamplecode %}

This code sets your `dart:ui`'s [`window.defaultRouteName`][]
to `"/onboarding"` instead of `"/"`.

這段程式碼使用 `"/onboarding"` 取代 `"/"`，
作為你的 `dart:ui` 的 [`window.defaultRouteName`]({{site.api}}/flutter/dart-ui/Window/defaultRouteName.html)

Alternatively, to construct a FlutterViewController directly without pre-warming
a FlutterEngine:

你也可以直接構造 FlutterViewController 而不用提前初始化 FlutterEngine：

{% samplecode initial-route-without-pre-warming %}
{% sample Swift %}
```swift
let flutterViewController = FlutterViewController(
      project: nil, initialRoute: "/onboarding", nibName: nil, bundle: nil)
```
{% sample Objective-C %}
```objectivec
FlutterViewController* flutterViewController =
      [[FlutterViewController alloc] initWithProject:nil
                                        initialRoute:@"/onboarding"
                                             nibName:nil
                                              bundle:nil];
```
{% endsamplecode %}

{{site.alert.tip}}

  In order to imperatively change your current Flutter
  route from the platform side after the `FlutterEngine`
  is already running, use [`pushRoute()`][]
  or [`popRoute()`] on the `FlutterViewController`.

  如果在 `FlutterEngine` 啟動後，迫切得需要在平台側改變你當前的 Flutter 路由，
  可以使用 `FlutterViewController` 裡的 [`pushRoute()`][] 或者 [`popRoute()`][]。

To pop the iOS route from the Flutter side,
  call [`SystemNavigator.pop()`][].
  
在 Flutter 側推出 iOS 路由，呼叫 [`SystemNavigator.pop()`][]。
{{site.alert.end}}

See [Navigation and routing][] for more about Flutter's routes.

檢視文件：[路由和導航][Navigation and routing] 瞭解更多 Flutter 路由的內容。

### Other

### 其它

The previous example only illustrates a few ways to customize
how a Flutter instance is initiated. Using [platform channels][],
you're free to push data or prepare your Flutter environment
in any way you'd like, before presenting the Flutter UI using a
`FlutterViewController`.

之前的例子僅僅展示了怎樣客製 Flutter 例項初始化的幾種方式，
透過 [撰寫雙端平台程式碼][platform channels]，
你可以在 `FlutterViewController` 展示 Flutter UI 之前，
自由地選擇你喜歡的，推入資料和準備 Flutter 環境的方式。

[`FlutterEngine`]: {{site.api}}/objcdoc/Classes/FlutterEngine.html
[`FlutterViewController`]: {{site.api}}/objcdoc/Classes/FlutterViewController.html
[Loading sequence and performance]: {{site.url}}/add-to-app/performance
[local_auth]: {{site.pub}}/packages/local_auth
[Navigation and routing]: {{site.url}}/ui/navigation
[Navigator]: {{site.api}}/flutter/widgets/Navigator-class.html
[`NavigatorState`]: {{site.api}}/flutter/widgets/NavigatorState-class.html
[`openURL`]: {{site.apple-dev}}/documentation/uikit/uiapplicationdelegate/1623112-application
[platform channels]: {{site.url}}/platform-integration/platform-channels
[`popRoute()`]: {{site.api}}/objcdoc/Classes/FlutterViewController.html#/c:objc(cs)FlutterViewController(im)popRoute
[`pushRoute()`]: {{site.api}}/objcdoc/Classes/FlutterViewController.html#/c:objc(cs)FlutterViewController(im)pushRoute:
[`runApp`]: {{site.api}}/flutter/widgets/runApp.html
[`runWithEntrypoint`]: {{site.api}}/objcdoc/Classes/FlutterEngine.html#/c:objc(cs)FlutterEngine(im)runWithEntrypoint:
[`SystemNavigator.pop()`]: {{site.api}}/flutter/services/SystemNavigator/pop.html
[tree-shaken]: https://en.wikipedia.org/wiki/Tree_shaking
[`WidgetsApp`]: {{site.api}}/flutter/widgets/WidgetsApp-class.html
[`window.defaultRouteName`]: {{site.api}}/flutter/dart-ui/SingletonFlutterWindow/defaultRouteName.html
[Start a FlutterEngine and FlutterViewController section]:{{site.url}}/add-to-app/ios/add-flutter-screen/#start-a-flutterengine-and-flutterviewcontroller
