<div class="tab-pane" id="terminal" role="tabpanel" aria-labelledby="terminal-tab" markdown="1">

## Create the app {#create-app}

## 建立應用 {#create-app}

Use the `flutter create` command to create a new project:

使用 `flutter create` 命令來建立新的工程：

```terminal
$ flutter create my_app
$ cd my_app
```

It is also possible to pass other arguments to `flutter create`,
such as the project name, the organization name,
or to specify the programming language used for the native platform:

你可以在執行 `flutter create` 時傳遞其他引數，例如專案名 (**pubspec.yaml**)、
組織名或者指定原生平台使用的語言：

```terminal
$ flutter create --project-name my_app --org dev.flutter --android-language java --ios-language objc my_app
$ cd my_app
```

The command creates a Flutter project directory called `my_app` that
contains a simple demo app that uses [Material Components][].

該命令會建立一個名為 `myapp`，裡面包含一個簡單的範例應用，
裡面用到了 [Material 元件][Material Components]。

{% include_relative _main-code-note.md %}

## Run the app

## 執行應用

 1. Check that an Android device is running.
   If none are shown, follow the device-specific instructions
   on the [Install][] page for your OS.

    檢查一下 Android 裝置是否已經正常執行。
    如果應用未顯示，請在 [安裝][Install] 頁面裡，
    根據你的作業系統按照裝置相關說明進行操作。

    ```terminal
    $ flutter devices
    ```

 1. Run the app with the following command:

    使用下面指令執行應用：

   ```terminal
   $ flutter run
   ```

{% capture save_changes -%}.
{% endcapture %}

 1. Type <kbd>r</kbd> in the terminal window.

    在命令列視窗輸入 <kbd>r</kbd>

{% include_relative _try-hot-reload.md save_changes=save_changes %}
{% include docs/run-profile.md %}

</div>

