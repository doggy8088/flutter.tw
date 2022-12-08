<div class="tab-pane active" id="vscode" role="tabpanel" aria-labelledby="vscode-tab" markdown="1">

## Create the app {#create-app}

## 建立應用 {#create-app}

  1. Invoke **View > Command Palette**.

     開啟 **View > Command Palette**。

  1. Type "flutter", and select the **Flutter: New Project**.

     輸入「flutter」，選擇 **Flutter: New Project**。

  1. Select **Application**.

     選擇 **Application**。

  1. Create or select the parent directory for the new project folder.

     新建或選擇新專案將存放的上層目錄。

  1. Enter a project name, such as `my_app`, and press **Enter**.

     輸入專案名稱，例如 `my_app`，並點選 **Enter**。

  1. Wait for project creation to complete and the `main.dart`
     file to appear.

     等待專案建立完成，並且 `main.dart` 檔案展現在編輯器中。

The above commands create a Flutter project directory called `my_app` that
contains a simple demo app that uses [Material Components][].

該命令會建立一個名為 `myapp`，裡面包含一個簡單的範例程式，
裡面用到了 [Material 元件][Material Components]。

{% include_relative _package-name.md  %}

{% include_relative _restart-vscode.md %}

{% include_relative _main-code-note.md  %}

## Run the app

1. Locate the VS Code status bar
   (the blue bar at the bottom of the window):<br>

   定位到 VS Code 的狀態列（視窗底部的藍色欄）：<br> 
    ![status bar][]{:.mw-100.pt-1.pb-2}

1. Select a device from the **Device Selector** area.
   For details, see [Quickly switching between Flutter devices][].

   從 **Device Selector** 區域選擇一個裝置。
   更多資訊，參考 [快速切換用於 Flutter 的裝置][Quickly switching between Flutter devices]。

   - If no device is available, and you want to use a device simulator,
     click **No Devices** and click
     **Start iOS Simulator** to launch a simulator.

     如果沒有可用的裝置，而同時你想使用模擬器，點選 **No Devices**
     並點選 **Start iOS Simulator** 啟動一個模擬器。

     {{site.alert.warning}}

        You might not see **Start iOS Simulator** option
        when you click **No Devices** in VS Code.
        If you're on Mac, then you might have to run following command
        in terminal to launch a simulator.

        點選 VS Code 的 **No Devices** 時，
        你可能不會看到 **Start iOS Simulator** 的選項。
        如果你在使用 macOS，請在終端中執行以下命令：

        ```terminal
        open -a simulator
        ```

        On Windows or Linux, it's not possible to launch an iOS simulator.

        在 Windows 或 Linux 上你無法啟動 iOS 模擬器。

      {{site.alert.end}}

    - To setup a real device, follow the device-specific instructions
      on the [Install][] page for your OS.

      想要配置真機用於除錯，請檢視你正在使用的系統的對應
      [安裝][Install] 裝置指導。

1. Invoke **Run > Start Debugging** or press <kbd>F5</kbd>.

   執行 **Run > Start Debugging** 或按下 <kbd>F5</kbd>。

1. Wait for the app to launch&mdash;progress is printed
   in the **Debug Console** view.

   等待應用啟動&mdash;&mdash;啟動進度會在 **Debug Console** 中展示。

{% capture save_changes -%}
  : invoke **Save All**, or click **Hot Reload**
  {% include_relative _hot-reload-icon.md %}.
{% endcapture %}

{% include_relative _try-hot-reload.md save_changes=save_changes %}
{% include docs/run-profile.md %}

[Install]: {{site.url}}/get-started/install
[Material Components]: {{site.material}}/guidelines
[Quickly switching between Flutter devices]: https://dartcode.org/docs/quickly-switching-between-flutter-devices
[status bar]: {{site.url}}/assets/images/docs/tools/vs-code/device_status_bar.png
[trusted your computer]: {{site.url}}/get-started/install/macos#trust

</div>
