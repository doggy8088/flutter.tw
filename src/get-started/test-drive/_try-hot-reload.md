After the app build completes, you'll see the starter app on your device.

當應用編譯完成後，就可以在裝置上執行這個起步應用了。

{% include docs/app-figure.md img-class="site-mobile-screenshot border"
    path-prefix="get-started" platform="iOS" image="starter-app.png"
    caption="Starter app" %}

## Try hot reload

## 嘗試熱重載 (hot reload)

Flutter offers a fast development cycle with _Stateful Hot Reload_,
the ability to reload the code of a live running app without
restarting or losing app state.
Make a change to app source,
tell your IDE or command-line tool that you want to hot reload,
and see the change in your simulator, emulator, or device.

Flutter 透過 **熱重載** 提供快速開發週期，
該功能支援應用程式在執行狀態下重載程式碼，
無需重新啟動應用程式或者丟失程式執行狀態。
修改一下程式碼，然後告訴 IDE 或者命令列工具你需要熱重載，
然後看一下模擬器或者裝置上應用的變化。

 1. Open `lib/main.dart`.
 
    開啟 `lib/main.dart`。
    
 1. Change the string

    修改字串

    {% prettify dart %}
      'You have [[strike]]pushed[[/strike]] the button this many times'
    {% endprettify %}

    to
    
    改為
    
    {% prettify dart %}
      'You have [!clicked!] the button this many times'
    {% endprettify %}

    {{site.alert.important}}
    
      Do _not_ stop your app. Let your app run.
      
      **不要** 停止應用。保持應用處於執行狀態。
      
    {{site.alert.end}}

 1. Save your changes{{include.save_changes}}
    
    儲存修改{{include.save_changes}}

You'll see the updated string in the running app almost immediately.

你會發現修改後的字串幾乎馬上出現在正在執行的應用程式上。
