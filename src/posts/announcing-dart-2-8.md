---
title: Dart 2.8 同期釋出
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/05/w7DjVe.png)

*作者 / Michael Thomsen, Dart & Flutter Product Manager, Google*

我們同樣為大家帶來了 Dart SDK 的新版本: Dart 2.8。Dart 社群保持著驚人的增長，如今有 [數百萬 Flutter 開發者](https://flutter.cn/posts/flutter-spring-2020-update) 使用 Dart 作為針對客戶端最佳化的開發語言，在各個平臺上建構高速流暢的應用。我們仍在努力完成 [即將推出的空安全 (null safety) 功能](https://flutter.cn/posts/dart-2-7)，使 Dart 成為一種更最佳化的語言，打造高速且穩定的使用者介面。我們準備了一些激動人心的新功能，讓開發者在管理依賴關係時更加高效。

Dart 平台透過 [pub 客戶端工具](https://dart.dev/tools/pub/cmd) 和 [pub.dev](https://pub.dev/) package 庫內建了程式碼包管理功能。在過去的一年裡，pub.dev package 庫增長了 200%，現在已經擁有近 10,000 個 package。作為持續改進 Dart 生態系統的一環，Dart 2.8 SDK 為 pub 客戶端工具帶來了兩個改進: 更強的 pub get 效能，以及一款新工具，可確保您的 package 依賴始終保持最新。

Dart 2.8 還在 Dart 語言和程式碼庫中引入了一些小幅度的重要改動 (Breaking Changes)。這些變更為我們的第一版空安全功能奠定了基礎。

## **為空安全奠基**

應用崩潰的一個常見原因是程式碼試圖使用一個恰好為空 (null) 的變數。Tony Hoare 爵士於 1965 年在 ALGOL 程式語言中引入了空參考，他在 [2009 年 QCon 演講](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare/) 中，把空參考稱為 "價值十億美元的錯誤"。在某些情況下，空值是有用的；但難點在於如何把這些有用的情況甄別出來。在過去的一年中，我們一直忙於在 Dart 中打造 [健全的空安全支援](https://github.com/dart-lang/language/issues/110)。這種支援將擴充型別系統，以表達始終不可空的變數；型別系統也會是完全健全的: Dart 編譯器和執行時都將信任這些型別，並能夠在型別系統保證變數不為空的情況下產生最佳化過的程式碼。

如您所想，這是一項浩大的工程，牽涉到的內容很多。為了確保用簡潔的語義建構空安全，我們決定在 Dart 語言和庫中引入一些小幅的重要改動。這些重要改動是對 Dart 型別系統邊界情況和一些 Dart 核心函式庫所做的小調整，以確保可空性相關的可用性和效能。我們已經在 Dart 公告列表中 [預先宣佈](https://groups.google.com/a/dartlang.org/g/announce/c/JwPWiC0jTiU) 了這些改動，而且我們估計這些重要改動對普通應用的程式碼影響很小。如果您在使用 Dart 2.8 時遇到任何問題，建議您檢視這些 [重要改動](https://github.com/dart-lang/sdk/issues/40686)，瞭解其詳情以及應對方法。如果這樣依然不能解決問題，請在我們的 [問題反饋頁](https://github.com/dart-lang/sdk/issues) 提交報告。想提前體驗空安全，請 [透過 DartPad 一睹為快](https://nullsafety.dartpad.cn/53257b6da4cb128dc1e069df64748ed1)。

## **更優質的 package 生態**

Pub package 管理器和 pub.dev 網站為 Dart 和 Flutter 提供了欣欣向榮的生態系統。在 pub.dev 上有近 10,000 個 package，許多有趣的 package 更是讓我們拍案叫絕。我們的核心任務之一就是幫助眾多作者打造高品質的 package，並幫助開發者找到它們。在這方面我們已經做了一些工作，比如 [改進 pub.dev 的可發現性](https://medium.com/dartlang/improved-discovery-on-the-dart-package-site-9bfe24c3d7d3)，[Verified Publishers](https://medium.com/dartlang/verified-publishers-98f05466558a) (釋出者認證)，以及 [Flutter Favorite](https://flutter.dev/docs/development/packages-and-plugins/favorites) 專案。

現在您從 pub.dev 獲取 package 的速度會快上許多。我們還提供了一款新工具來幫助您更新所有 package 的依賴關係。現在的應用頗為複雜，很可能依賴幾十個 package。如何把它們都更新到最新版本，確保滿足最新的依賴關係，並獲得所有的 bug 修復和效能最佳化？從 Dart 2.8 開始，您可以使用 pub outdated，這款新工具將自動判斷哪些依賴項需要更新到最新、最合適的版本。

## **Dart 2.8 釋出工具效能改進**

我們優化了 pub 工具的效能，在執行 pub get 時會並行獲取 package，並且推遲了 pub run 預編譯的執行時機。

在使用 flutter create 建立的新專案上執行 flutter pub get 的非正式基準測試，使用 Flutter 1.12 (使用 Dart 2.7) 總執行時間約為 6.5 秒，而在 Flutter 1.17 (使用 Dart 2.8) 中下降至 2.5 秒。在 [Flutter Gallery](https://github.com/flutter/gallery) 這樣更大型的應用中，時間則從約 15 秒降到了 3 秒左右！

## **使用 pub outdated 管理依賴**

![](https://devrel.andfun.cn/devrel/posts/2021/05/LpzhUs.gif)

Dart 程式碼中的依賴關係會被收集到 [pubspec 檔案](https://kw-staging-dartlang-2.firebaseapp.com/tools/pub/pubspec) 中。當您執行 pub get 命令，從 pub.dev 中獲取 package 時，pub 版本求解器 (使用 [PubGrub 演算法](https://medium.com/@nex3/pubgrub-2fb6470504f)) 會執行一個處理序來得出滿足 pubspec 中所有約束條件的所有依賴項的最新版本。請注意，pub 使用的是單版本方案，您的應用中只包含每個 package 的單一版本，這個方案可以確保您的應用獲得儘可能小的體積。

始終使用最新的穩定版 package 是 [最佳開發實踐](http://dart.dev/tools/pub/dependencies#best-practices) 之一，但這樣做會很費力。Dart 支援使用 [pub upgrade](https://dart.dev/tools/pub/cmd/pub-upgrade) 升級到 [語義上相容](https://dart.dev/tools/pub/dependencies#version-constraints) 的最新版本，但如果不更新 pubspec，就不能包含 package 最新的大版本。pub outdated 命令透過比較當前使用的版本和 pub.dev 上的最新版本，讓您知曉小版本和大版本何時可用。

我們來看一個例子。假如您正在建構一款應用，這款應用的 pubspec.yaml 包含下列內容:

![](https://devrel.andfun.cn/devrel/posts/2021/05/9DxuuV.png)

執行 pub get，這款工具會建立一個 pubspec.lock 檔案，包含如下版本資訊:

![](https://devrel.andfun.cn/devrel/posts/2021/05/FYXC5g.png)

幾個月過去了，pub.dev 現在有了新版本的 foo (1.3.1) 和 bar (2.1.0 和 3.0.3)。那麼我們如何才能知道這些新版本是可用的？對於小版本升級 (foo 1.4.0 和 bar 2.1.0)，您可以執行 pub upgrade，但這樣您不會得到 bar 3.0.0。為了版本更新您不得不存取 pub.dev 上對應 package 的頁面來了解詳情。或者您也可以嘗試社群提供的解決方案，比如 Paulina Szklarska 的 [version checker](https://plugins.jetbrains.com/plugin/12400-flutter-pub-version-checker) 或者 Jeroen Meijer 的 [pubspec assist](https://marketplace.visualstudio.com/items?itemName=jeroen-meijer.pubspec-assist)。

Dart SDK 現在透過 `pub outdated` 支援發現新版本。如果您使用的是支援 Dart 或 Flutter 的 IDE，請使用 **Pub outdated** 指令，這個指令在 `pubspec.yaml` 檔案被開啟時會顯示。或者透過終端執行 `pub outdated` 或者 f`lutter pub outdated` 命令:

![](https://devrel.andfun.cn/devrel/posts/2021/05/B9BoA1.png)

上面的輸出結果表明，我們可以使用 pub upgrade 自動升級到 foo 1.3.1，也就是 foo 的最新可用版本。與此同時，雖然我們可以自動升級到 bar 2.1.0，但最新可用的版本其實是 3.0.3。升級到 bar 3.0.3 屬於大版本升級，所以我們需要編輯 pubspec.yaml 檔案，才能完成升級:

![](https://devrel.andfun.cn/devrel/posts/2021/05/oGr7Qc.png)

在編輯完 pubspec 並執行 pub upgrade 之後，pub outdated 會報告所有的依賴均已是最新版本:

![](https://devrel.andfun.cn/devrel/posts/2021/05/RwkudP.png)

成功更新到最新版本了！由於我們剛才獲取了最新的版本，包括一次大版本升級，現在我們需要檢視一下這些版本中是否存在重要改動。然後執行各種測試，確保我們的應用可以正常執行。

## **下一步**

效能改善、[重要改動](https://github.com/dart-lang/sdk/issues/40686)，以及全新的 `pub outdated` 命令，現已加入穩定版 [Dart 2.8 SDK](https://dart.dev/get-dart) 和穩定版 [Flutter 1.17 SDK](https://flutter.dev/docs/get-started/install)。我們建議您儘快開始使用 [pub outdated](https://dart.dev/tools/pub/cmd/pub-outdated)，掌握自己專案中依賴的健康狀況！

如果您遇到了問題，請前往 [pub 問題反饋頁](https://github.com/dart-lang/pub/issues) 報告給我們；如果您遇到了通用問題，請前往 [SDK 問題反饋頁](https://github.com/dart-lang/sdk/issues) 進行上報。我們非常期待大家分享 `pub outdated` 的使用體驗。
