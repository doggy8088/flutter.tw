## Windows setup

## Windows 設定

{{site.alert.warning}}

  **Windows support!**
  As of Flutter 2.10, Windows support is available
  on the `stable` channel! For more information, see
  [Announcing Flutter for Windows][], a free article
  on Medium.

  **Windows 支援！**
  自 Flutter 2.10 起，Windows 的支援已進入 `stable` 渠道！
  你可以檢視
  [用 Flutter 建構 Windows 桌面應用程式][Announcing Flutter for Windows]
  文章瞭解更多資訊。

{{site.alert.end}}

[Announcing Flutter for Windows]: {{site.flutter-medium}}/announcing-flutter-for-windows-6979d0d01fed

### Additional Windows requirements

### 其他 Windows 需要的內容

{% include_relative _help-link.md location='win-desktop' %}

For Windows desktop development,
you need the following in addition to the Flutter SDK:

對於 Windows 桌面開發而言，除了 Flutter SDK 以外你還需要以下內容：

* [Visual Studio 2022][] or [Visual Studio Build Tools 2022][]
  When installing Visual Studio or only the Build Tools,
  you need the "Desktop development with C++" workload installed
  for building windows, including all of its default components. 

  [Visual Studio 2022][] 或 [Visual Studio 2022 產生工具][Visual Studio Build Tools 2022]
  在選擇安裝 Visual Studio 時或只安裝產生工具的時候，
  你需要選擇「使用 C++ 的桌面開發」，包括其所有預設元件，
  以安裝必要的 C++ 工具鏈和 Windows SDK 的標頭檔案。

{{site.alert.note}}

  **Visual Studio** is different than Visual Studio _Code_.

  請注意區分 **Visual Studio** 與 Visual Studio **Code**。

{{site.alert.end}}

For more information, see [Building Windows apps][].

更多詳情請檢視
[建構基於 Flutter 的 Windows 應用程式][Building Windows apps]。

[Building Windows apps]: {{site.url}}/platform-integration/windows/building
[Visual Studio 2022]: https://visualstudio.microsoft.com/downloads/
[Visual Studio Build Tools 2022]: https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022
