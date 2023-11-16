---
title: 使用 DartPad 製作程式碼實踐課程
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2022/06/183a339569ee9.png)

DartPad 是一個開源的、在瀏覽器中體驗和執行 Dart 程式語言的線上編輯器，目標是為了幫助開發者更好地瞭解 Dart 程式語言以及 Flutter 應用開發。

DartPad 專案起始於 2015 年，最初只是一個線上的編譯器，可以編譯、分析和顯示 Dart 程式碼執行結果。後期主要進行過幾次重要的改進：
- 2019 年 12 月，新版的 DartPad 釋出，不僅推出了獨立的存取域名 (dartpad.dev)，同時也支援執行 Flutter 應用 (透過網頁展現)
- 2020 年 4 月，CodePen 開始支援 Flutter 應用，其使用了與 DartPad 相同的後端處理
- 2020 年 10 月，DartPad 空安全版本上線，為開發者們提供空安全編譯環境的 Dart SDK
- 2021 年 5 月，DartPad 正式支援 Workshop 格式內容
- 2021 年 11 月，DartPad 支援 SDK 版本切換 (Stable / Beta / Old)，並引入一些常用的 package，比如 Google 字型 (google_fonts)、bloc、http、intl，並計劃在後期不斷引入新的 package，目前已經支援了 87 個 package

DartPad 使用者大部分是學習 Dart 程式語言的開發者，可以免於配置本地開發環境直接測試 Dart 程式語言特性。另一個場景是用於程式碼片段的分享，這可以是一個小功能，比如 Flutter API 文件裡介紹某個 widget 時就會內嵌一個 DartPad 向開發者展示這個 widget 實際執行時的效果，程式碼片段也可以是一個最小可復現的 bug 程式碼，方便在描述問題的時候，同時將程式碼分享給 debug 的開發者。

本文將帶你瞭解 DartPad 的另一種用法，我們稱之為 DartPad Workshop，我們稱之為 DartPad 實踐課程，本文由 Flutter GDE Alex [撰寫發表](https://juejin.cn/post/7098544316296986638)。

## Workshop 是什麼？

2021 年 5 月 Dart 2.13 (Flutter 2.2) 釋出之時，Flutter 團隊為 DartPad 新增了 Workshop 格式的支援，目標是支援和改善 Flutter 講師在分享和進行課程講解時的實踐體驗，當年的 I/O 大會上也正式測試了很多個 Workshop，開發者們的反應也非常積極。

以往的 DartPad 內容基於 GitHub Gist 分享，僅能分享一份程式碼，並且沒有引導式的步驟和互動。而 Workshop 是一套可互動、多步驟、引導式且基於 Web Server 的內容架構。作者可以自訂 workshop 名稱，定義每一個步驟的內容和解法，使用 Markdown 展示步驟的介紹，最後使用多種方式託管 workshop 原始檔以進行使用。

![DartPad Workshop 架構](https://pic.alexv525.com/202205112305930.png)

接下來，我將帶來大家從使用、建立、撰寫到釋出 workshop 的所有必要步驟，成為一名教學大師！

##  Workshop 怎麼開始？

**工欲善其事，必先利其器**。想要寫好一個 Workshop，首先要了解 Workshop 最終會呈現什麼效果。

此處推薦瀏覽官方的 [Sliver Workshop](https://dartpad.cn/workshops.html?webserver=https://dartpad-workshops-io2021.web.app/getting_started_with_slivers)，簡單體驗一番。

![Flutter Sliver Workshop 介面](https://pic.alexv525.com/202205161642752.png)

從上圖中我們不難看出，整個 Workshop 被一分為三：
- 左側為介紹內容塊，同時可以進行步驟的切換；
- 右上為程式碼編輯器，可以點選 Run 執行程式碼；
- 右下則為介面輸出、控制檯以及文件介面。

同時，將滑鼠放在左側下方的 **Step 1** 上，將出現步驟切換列表，展示出所有的可切換步驟。

![Workshop 步驟切換](https://pic.alexv525.com/202205112348483.png)

當某個步驟包含解答（有 `solution.dart` 且 `has_solution` 為 `true`）時，左側介紹內容塊的右下角將顯示 **Show Solution** 按鈕，點選後右側的程式碼內容將使用解答程式碼完整替換。

![Workshop 顯示解答](https://pic.alexv525.com/202205161644944.png)

整個過程中，無論是 Workshop 為你準備好的程式碼，還是點選 **Show Solution** 替換的解答程式碼，**都可以在右側的程式碼編輯器中進行修改並執行**。

## Workshop 怎麼寫？

新建一個目錄，名字隨意（例如 `my_workshop`），在 Workshop 完成時，結構應當如下：

```
my_workshop/
|---meta.yaml          # 元資料宣告檔案
|---step_01/           # 步驟目錄（任意名稱）
   |---instructions.md    # 步驟介紹 Markdown 文件
   |---snippet.dart       # 程式碼檔案
   |---solution.dart      # 解答檔案（可選）
|---step_02/
   |---instructions.md
   |---snippet.dart
```

### meta.yml

在元資料宣告檔案中，你需要宣告如下結構的內容：

```yaml
name: My workshop      # Workshop 的名稱
type: dart             # Workshop 的型別，可為 "dart" 或 "flutter"
steps:                 # 宣告步驟
  - name: Step 1         # 步驟的名稱
    directory: step_01   # 步驟對應的目錄
    has_solution: true   # 步驟是否有解答
  - name: Step 2
    directory: step_02
    has_solution: false
```

如果你的某一個步驟需要開發者動手修改，最後提供一份解答，你可以將 `has_solution` 設定為 `true` 並且建立 `solution.dart` 檔案進行互動。

每個步驟包含的內容大同小異，但在開始寫一個具體的步驟前，我們還有一項有利於我們開發 Workshop 的操作，那就是 **在專案的同級目錄新建一個 `pubspec.yaml`** 。`pubspec.yaml` 有助於 CLI / IDE 識別並配置你的 Dart / Flutter 專案。如此你便可以像正常的 Dart / Flutter 專案一樣編寫你的 Workshop。

### 編寫步驟內容

**instructions.md**

`instructions.md` 是每一個步驟對應的介紹文件，支援以 Markdown 的形式編寫。實測在 DartPad 環境中可以支援 `<iframe>` 標籤進行嵌入，你可以嵌入影片或者其他互動式內容來輔助你的介紹。

![instructions 介面預覽](https://pic.alexv525.com/202205162308673.png)

**snippet.dart**

該檔案是每個步驟對應的必需 Dart 程式碼片段，
在切換步驟時對應的程式碼將直接替換到程式碼編輯器內。
程式碼檔案有時候並不需要開發者關注所有的內容，
在這樣的情況，推薦在檔案中使用 **TODO** 標記需要開發者修改的內容。

![使用 TODO 標記程式碼](https://pic.alexv525.com/202205162304888.png)

**solution.dart**

該檔案是可選的步驟解答。在使用它前，你需要在 `meta.yml` 中把對應步驟的 `has_solution` 設定為 `true` 方可生效。
它和 `snippet.dart` 格式基本相同，
在前文中我們介紹到，點選 **Show Solution** 按鈕，
可以將已有的程式碼替換為你編寫的解答內容。
當你想教會開發者一種特定的實現方式時，解法檔案可以發揮它的作用。
但如果對應的步驟是開放性的體驗嘗試，就無需提供特定的解法。

文末可體驗截圖裡的 Workshop，並參考其建構原始碼 (包含 `instructions.md`、`snippet.dart`、`solution.dart` 等檔案)。

## Workshop 如何部署？

**Workshop 支援以 HTTP 伺服器的方式被託管**。總的來說有三種主要的託管方式：

* 使用你的個人伺服器進行部署，也可以託管在本地或區域網給同事或者學生分享。此時的存取網址應當是: `https://dartpad.cn/workshops.html?webserver=https://www.example.com/my_workshop`
* 使用 Web 應用服務商提供的服務，類似於 Firebase、web.app、Web+、Webify 等 PaaS 服務；此時的存取網址應當是: `https://dartpad.cn/workshops.html?webserver=https://your_app.web.app/path/to/my_workshop`
* 直接將程式碼上傳至 GitHub，透過 GitHub API 或原始儲存地址進行存取。此時的存取網址可以是: `https://dartpad.cn/workshops.html?webserver=https://raw.githubusercontent.com/your_name/my_workshop` 也可以是: `https://dartpad.cn/workshops.html?gh_owner=your_name&gh_repo=my_workshop`

需要注意的是，在實際執行的過程中，Workshop 的讀者會透過自己的瀏覽器從部署頁面讀取相應的內容，並展示和執行在 DartPad 頁面，因此需要作者確保部署後的 Workshop 地址可以被正確存取。

## 體驗現有的 Workshop

今年的 I/O Adventure，在 Flutter 產品分割槽透過 15 個虛擬展位展示了由 Flutter GDE 製作的 Workshop，大家可以到 https://io.google/ 線上體驗:

![DartPad Workshop 展區](https://pic.alexv525.com/202205170011031.png)

推薦大家體驗 Alex 製作的 [LazyIndexedStack Worshop](https://flutter.cn/urls/lazyindexedstack)，[程式碼儲存庫地址](https://github.com/AlexV525/dartpad_workshops/tree/main/implement_lazy_indexed_stack)。
