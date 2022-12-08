{{site.alert.warning}}

  The Flutter tool may occasionally download resources from Google servers.
  By downloading or using the Flutter SDK you agree to the [Google Terms of Service][].

  Flutter 工具可能會偶爾從 Google 的伺服器上下載資源，
  下載了或使用 Flutter SDK 代表您同意了
  [Google 服務條款][Google Terms of Service]。

  For example, when installed from GitHub (as opposed to from a prepackaged archive),
  the Flutter tool will download the Dart SDK from Google servers immediately when
  first run, as it is used to execute the `flutter` tool itself. This will also
  occur when Flutter is upgraded (e.g. by running the `flutter upgrade` command).

  舉個例子，當開發者們從 GitHub 安裝（而不是從預打套件的歸檔檔案中安裝），
  為了要執行 `flutter` 命令，Flutter 工具將會在首次執行時從谷歌伺服器下載 Dart SDK。
  升級 Flutter 時亦會發生 (比如執行 `flutter upgrade` 命令) 。

  The `flutter` tool uses Google Analytics to report feature usage
  statistics and send [crash reports][]. This data is used to help improve Flutter
  tools over time.

  `flutter` 工具使用了 Google Analytics
  來對基本使用情況和 [崩潰報告][crash reports] 進行匿名的統計。
  這些資料用來幫助改善 Flutter 工具。

  Flutter tool analytics are not sent on the very first run. To disable
  reporting, run `flutter config --no-analytics`. To display the current
  setting, use `flutter config`. If you opt out of analytics, an opt-out
  event is sent, and then no further information is sent by the
  Flutter tool.

  在第一次執行或者任何涉及到 `flutter config` 的資訊都不會進行傳送，
  所以你可以在傳送分析資料之前選擇禁止分析資料的統計。
  要禁用這一功能，只需要輸入 `flutter config --no-analytics` 即可，
  想要檢視當前設定使用命令 `flutter config`。
  如果你禁用了統計資訊傳送，這次的禁用行為會被記錄傳送，
  其他任何資訊，以及未來都不會再有任何資料會被記錄。
  
  Dart tools may also send usage metrics and crash reports to Google.
  To control the submission of these metrics, use the following options on the
  [`dart` tool][]:

  Dart 工具同樣可能會發送使用指標資料和崩潰報告給 Google。
  控制這些傳送的資料，請參考下面的
  [`dart` 命令列][[`dart` tool] 引數：

   * `--enable-analytics`: 啟用匿名分析；
   * `--disable-analytics`: 禁用匿名分析。

  The Google [Privacy Policy][] describes how data is handled by these services.

  [Google 的隱私權政策][Privacy Policy] 裡詳細描述了這些服務會如何控制這些資料。

  [Google Terms of Service]: https://policies.google.com/terms
  [Privacy Policy]: https://policies.google.com/privacy
  [crash reports]: {{site.repo.flutter}}/wiki/Flutter-CLI-crash-reporting
  [`dart` tool]: {{site.dart-site}}/tools/dart-tool

{{site.alert.end}}
