---
title: 在 Pixelbook 上開發 Flutter 應用
description: Chrome OS 的新特效能夠讓您無需模擬器就能開發並測試 Flutter 應用程式。
toc: true
---

文 / Tim Sneath，谷歌 Dart & Flutter 產品組產品經理

譯 / 王鑫磊，CFUG 社群譯者，貢獻者，部落格 [xinlei.dev](https://xinlei.dev)

原文：[ProAndroidDev Medium 專欄](https://proandroiddev.com/flutter-development-on-a-pixelbook-dde984a3fc1e)

> 提示：本文已在 2019 年 1 月更新以匹配較新版本的 ChromeOS。然而，隨著 Google 在持續不斷對其最佳化，你可能會看到一些部分與本文描述內容有微小的差異。如果你在本文的最新版本中看到任何不清楚或錯誤的內容，請留下備註或評論，我會修正它。

在 Google I/O 2018 演講中，[Emilie Roberts](https://medium.com/@emilieroberts) 介紹了 Chrome OS 中一些很酷的新特性，主題是[在 Pixelbook 上進行 Android 開發](https://developer.android.google.cn/topic/arc/studio) 。藉助 [Linux on Chromebooks](https://blog.google/products/chromebooks/linux-on-chromebooks/) 這一新功能，你現在可以[直接在 Pixelbook 上執行 Android Studio](https://developer.android.google.cn/topic/arc/studio)，並[透過 Wi-Fi 連線除錯 Android 裝置](https://developer.android.google.cn/studio/command-line/adb#wireless)，甚至可以在 Pixelbook 上直接執行產生的 Android 應用程式。這一切都得感謝  [Crostini](https://chromium.googlesource.com/chromiumos/docs/+/master/containers_and_vms.md) 計劃，它為裝置提供了一個 Linux 容器例項。

更酷的是這一切都能在 Flutter 上表現得同樣出色。這意味著你可以直接在裝置上進行所有開發，甚至不需要模擬器來測試你的應用程式。雖然這有一點實驗性質，但它對於那些不需要加入蘋果生態系統的 Flutter 開發者來說變得相當有吸引力。

下面的截圖來自一臺正在執行 Visual Studio Code 的 Pixelbook，而且透過 [Android Device Bridge](https://developer.android.google.cn/studio/command-line/adb) 連線的 Flutter 應用程式同樣也執行在本地。這可不是什麼模擬器——Flutter 應用能夠在 Pixelbook 上直接執行（請看 Visual Studio程式碼狀態列右下角的目標平台）。

![Flutter running locally on a Pixelbook, connected with hot reload to Visual Studio Code](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-development-on-a-pixelbook/flutter_dev_env_on_pixelbook.png){:width="85%"}

接下來我將在這篇文章中分步說明如何在你自己的裝置上執行它。Linux on Chromebook 支援現在已經進入 beta 階段，但它需要[一臺支援 Crostini 的裝置](https://chromium.googlesource.com/chromiumos/docs/+/master/containers_and_vms.md#Supported-Now)，包括 [Pixelbook](https://store.google.com/product/google_pixelbook)。 但也可能會出現一些失靈的情況，這都因人而異。當然，ChromeOS 裝置的效能特徵將與典型的 iPhone 或 Android 裝置的效能特徵相差較大。我們也不能保證每個 Flutter 場景都能在這個平臺上正確執行。這算是一個足夠的警告了吧 ；）

### 基礎的環境安裝

要獲得 Flutter 完整的除錯功能，目前你需要將你的裝置設為[開發者模式](http://www.chromium.org/chromium-os/chromiumos-design-docs/developer-mode)，這樣就可以移除一些為典型消費者使用的沙箱保護措施，並讓你的裝置支援不受 Play 商店信任的應用程式。在 Pixelbook 上，你可以在按下電源（Esc）按鈕的同時按住 Refresh 鍵進入開發者模式。系統將重新啟動並進入恢復頁面（他會提示你插入隨身碟）。你只需要忽略這段文字並按住 Ctrl + D 即可，這將會在一段警告後進入該模式。

由於這種支援正在不斷髮展，[將裝置切換到 Chrome OS 的 beta 頻道](https://support.google.com/chromebook/answer/1086915?hl=en) 並不是一個壞主意，它將會為你提供最新的資訊（在 2019 年 1 月時它是 71.0.3578.94，但它會定期更新）。當你切換頻道後，你需要讓 Chrome OS 下載並安裝該頻道的最新版本，然後重啟。

當你使用最新的頻道重啟後，你會在"設定"中看到一個新的條目，該條目可以讓你在 Chromebook 上安裝 Linux 支援。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-development-on-a-pixelbook/turn_on_linux_env_on_cb.png){:width="85%"}

在你開啟開啟它之後，ChromeOS 將會彈出下載並激活 Linux 容器的視窗。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-development-on-a-pixelbook/turn_on_linux_env_on_cb_processing.png){:width="85%"}

我的機器花費了幾分鐘的時間來完成它。完成後，你會看到應用程式啟動器上有一個閃亮的新終端應用程式圖示，我立即把它固定到工作列。

### 安裝 Flutter

現在該來講點有趣的事了。開啟終端，然後使用 apt 進行檢查，確保你正在使用較新版本的 Linux。你可以執行 `sudo apt update && sudo apt upgrade` 這個命令來完成這項操作。

我們還將在接下來的步驟中安裝一些您需要的其他工具套件和依賴項。

`$ sudo apt install libglu1-mesa lib32stdc++6`

由於你正在執行一個容器，在當前版本中下載到容器例項的最簡單方法是使用 `wget`（你也可以使用 Chrome 下載它們並透過 SSH 隧道傳送它們，但那會更加複雜）。你能夠透過以下命令很輕鬆地下載並安裝 Flutter。

```
$ curl -O https://storage.googleapis.com/flutter_infra/releases/stable/linux/flutter_linux_v1.5.4-hotfix.2-stable.tar.xz 
$ tar xf flutter_linux_v1.5.4-hotfix.2-stable.tar.xz
$ export PATH=`pwd`/flutter/bin:$PATH
```

現在你應該可以執行 `flutter doctor` 命令了，它會為 Flutter 提供一個乾淨的執行環境，即使它會在這個階段抱怨缺乏必要的工具或裝置。

### 安裝 Visual Studio Code 和 Android Studio

現在，是時候安裝一兩個 IDE 了。Flutter 主要支援的兩個 IDE （Android Studio 和 Visual Studio Code）都在 Chrome OS Linux 支援版上執行得相當完美。但是，無論你是否計劃將Android Studio 作為 IDE，你都需要安裝它以便擁有 Android 的建構工具。

你能夠在官網上直接下載 [Android Studio](https://developer.android.google.cn/studio/)，但是由於 Linux 執行在與 Chrome 瀏覽器不同的容器中，所以你必須手動移動它才能在終端進行存取。拖住時同時按住 Shift 健，將其從 Download 目錄移動到 Linux 檔案節點中（它在 Linux 容器中代表了 home `~` 目錄）。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-development-on-a-pixelbook/download_android_sdk_to_linux_env.png){:width="85%"}

然後，你就可以執行以下內容來解壓縮並安裝 Android Studio：

```
$ unzip android-studio.zip
$ sudo mv ~/android-studio /usr/local/
$ cd /usr/local/android-studio/bin
$ ./studio.sh
```

你將會看到下面這樣的彈窗：

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-development-on-a-pixelbook/start_page_as_on_cb.png)

點選剩下的設定步驟（我沒有為選擇 Android 虛擬裝置而煩惱，因為你知道的，我們不再需要它了！）。當 IDE 啟動時，建立一個新的空白專案（使用預設設定即可）。為了更加輕鬆地返回 Android Studio，我們將新增一個啟動畫標，所以請轉到 Tools / Create Desktop 項。

現在我們能夠為 Android Studio 安裝 Flutter 外掛了。要安裝這些工具和整合，請跳到 File / Settings 並選擇 Plugin 節點，然後點選 **Browse repositories** 。在這裡搜尋 Flutter 外掛，如下所示，然後點選 Install 按鈕。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-development-on-a-pixelbook/install_flutter_plugin_on_as.png){:width="85%"}

這部分的最後一步——透過執行以下命令以確保你已同意所有 Android 許可證：

`$ flutter doctor --android-licenses`

對於 Visual Studio Code，你可以按照以下說明下載並安裝。這將會下載 Visual Studio Code 套件及必要的依賴並安裝（go.microsoft.com URL會自動重新導向到最新版本）。

```
$ curl -L -o vscode.deb https://go.microsoft.com/fwlink/?LinkID=760868
$ sudo apt install ./vscode.deb
```

最後開啟 Visual Studio Code 安裝 [Flutter 擴充](https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter)。

你馬上就要完成了！執行 `flutter doctor -v`，現在應該顯示所有設定都已完成，除了 ` no connected devices` 這項，我們將會在最後解決這個問題。

### 將 Pixelbook 配置為開發者裝置

我們需要處理一些事情，以便讓 Pixelbook 被 Android 識別為開發裝置。


首先是修改 Chrome OS 的防火牆，以 [允許來自 Linux 端傳入 ADB 連結](https://developer.android.google.cn/topic/arc/#configure-the-firewall)。在一個 Chrome 視窗中點選 Control+Alt+T 來開啟 Chrome OS 的終端，然後輸入 `shell` 來啟動一個終端中的 shell。如果 `shell` 不起作用，那麼你可能是忘記了將裝置設為開發者模式，我們前面提到過這一點。

現在，執行一些 `sudo` 命令： 

```
chronos@localhost / $ sudo crossystem dev_boot_signed_only=0
chronos@localhost / $ sudo /usr/libexec/debugd/helpers/dev_features_rootfs_verification
chronos@localhost / $ sudo reboot
```

在系統重啟後，你只需要執行這最後一個命令：

``` 
chronos@localhost / $ sudo /usr/libexec/debugd/helpers/dev_features_ssh
```

接下來是開啟 Android 開發者模式。你也許會想，我們最開始不是已經完成這一步了嗎？但是我們僅僅是設定 Pixelbook 自身為開發者模式：安卓端需要使用你也許用過的手機上相同的魔術開關。這個設定隱藏的有點深：從設定應用中選擇 Google Play Store 選單，然後選擇管理 Android 偏好設定（Manage Android preferences）：

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-development-on-a-pixelbook/manage_android_pref_on_cb.png){:width="85%"}

這將會為 Android 託管環境開啟另一個設定應用，你可以在其中找到"關於裝置"（**About device**）頁面。

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-development-on-a-pixelbook/android_env_on_cb_about_page.png)

連續點選 build number 7 次：這是解鎖開發者選項螢幕的神奇咒語，可讓你開啟 ADB 除錯：

![](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-development-on-a-pixelbook/turn_on_dev_mode_android_on_cb.png){:width="85%"}

現在，你終於可以建立 Flutter 能夠存取的本地裝置的 ADB 連線。假設你是在主目錄下解壓 Android Studio，那麼你會在以下位置找到 `adb` ：`$/Android/Sdk/platform-tools/adb`

執行以下命令，以連線到本地 Android 例項：

`$ adb connect 100.115.92.2:5555`

### 彙總

幸運的話，`flutter doctor` 會給你一個乾淨健康的環境。你現在可以建立並執行 Flutter 應用了！例如：

```
$ flutter create testapp
$ cd testapp
$ flutter run
```

等 Gradle 編譯完 APK 並將其安裝後，你將會看到一個全屏的正在執行範例應用程式碼的視窗跳出來。

![A simple Flutter app running in full-screen mode on a Pixelbook.](https://files.flutter-io.cn/posts/flutter-cn/2019/flutter-development-on-a-pixelbook/flutter_starter_app_on_cb.png){:width="85%"}

與其他在 Pixelbook 上執行的 Android 應用程式一樣，你可以從最大化的視窗恢復為正常大小，這時候應用程式將會看上去和手機上執行的外觀非常相似：除了它是直接執行在你的 Pixelbook 上的！

### 建議及注意事項

您可能希望稍微調整螢幕 DPI 設定以獲得更多空間。有些應用程式在這方面上比其他應用程式響應更好;特別是我發現一些 Android Studio 對話方塊對於給定的 DPI 設定來說太小了。

然而總的來說，這似乎非常有用，特別是如果你願意使用這些由各個團隊完成的最新特性的話。如果你有 Pixelbook 並一直在等待一個學習 Flutter 或嘗試建構應用的方法的話，它絕對值得你試一試！祝你玩得開心，歡迎告訴我們你的故事：你可以在 Twitter 上的 [@flutterio](https://twitter.com/flutterio) 中找到我們團隊。
