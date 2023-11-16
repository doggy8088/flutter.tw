<div class="tab-pane" id="androidstudio" role="tabpanel" aria-labelledby="androidstudio-tab" markdown="1">

## Create the app {#create-app}

## 建立應用 {#create-app}

1. Open the IDE and select **New Flutter Project**.

   開啟 IDE 並選中 **New Flutter Project**。

2. Select **Flutter**, verify the Flutter SDK path with the SDK's location.
   Then click **Next**.

   選擇 **Flutter**，驗證 Flutter SDK 的路徑。完成後選擇 **Next**。

3. Enter a project name (for example, `my_app`).

   輸入專案名稱（例如 `my_app`）。

4. Select **Application** as the project type.
   Then click **Next**.

   選擇 **Application** 的專案型別，
   完成後選擇 **Next**。

5. Click **Finish**.

   點選 **完成**。

6. Wait for Android Studio to create the project.

   等待 Android Studio 完成專案的建立。

{% include_relative _package-name.md  %}

The above commands create a Flutter project directory
called `my_app` that contains a simple demo app that
uses [Material Components][].

上述步驟會建立名為 `my_app` 的 Flutter 專案的資料夾，
它是一個使用了 [Material 元件][Material Components] 的簡單 demo。

{% include_relative _main-code-note.md  %}

## Run the app

## 執行應用

 1. Locate the main Android Studio toolbar:<br>
    ![Main IntelliJ toolbar][]{:.mw-100}

    定位到 Android Studio 的工具欄：<br>
    ![Main IntelliJ toolbar][]{:.mw-100}

 1. In the **target selector**, select an Android device for running the app.
    If none are listed as available,
    select **Tools > AVD Manager** and create one there.
    For details, see [Managing AVDs][].

    在 **target selector** 中，選擇一個用於執行應用的 Android 裝置。
    如果列表為空，選擇 **Tools > AVD Manager** 建立一個虛擬機器。
    更多細節可以參考 [管理 AVD 虛擬機器][Managing AVDs]。

 1. Click the run icon in the toolbar, or invoke the menu item **Run > Run**.

    點選工具欄中的執行按鈕，或者點選選單欄中的 **Run > Run**。

{% capture save_changes -%}
  : invoke **Save All**, or click **Hot Reload**
  {% include_relative _hot-reload-icon.md %}.
{% endcapture %}

{% capture ide_profile -%}
  to invoke the menu item **Run > Profile** in the IDE, or
{% endcapture %}

{% include_relative _try-hot-reload.md save_changes=save_changes %}
{% include docs/run-profile.md ide_profile=ide_profile %}

[Main IntelliJ toolbar]: {{site.url}}/assets/images/docs/tools/android-studio/main-toolbar.png
[Managing AVDs]: {{site.android-dev}}/studio/run/managing-avds
[Material Components]: {{site.material}}/components

</div>
