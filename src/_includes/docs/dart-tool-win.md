{{site.alert.note}}

  The Flutter SDK contains the `dart` command alongside the `flutter` command 
  so that you can more easily run Dart command-line programs. 
  Downloading the Flutter SDK also downloads the compatible version of Dart,
  but if you've downloaded the Dart SDK separately,
  make sure that the Flutter version of `dart` is
  first in your path, as the two versions might not be compatible.
  The following command tells you whether the `flutter` and `dart`
  commands originate from the same `bin` directory and are
  therefore compatible.

  Flutter SDK 在 `flutter` 命令指令碼的同級目錄下增加了 `dart` 命令，
  你可以更方便地執行 Dart 命令列程式。
  下載 Flutter SDK 時也會下載對應版本的 Dart SDK，但如果你單獨下載了 Dart SDK，
  請確保 Flutter SDK 內的 `dart` 在你的環境變數中排在首位，因為單獨的 SDK 可能並不相容 Flutter SDK。
  下面的命令展示了 `flutter` 和 `dart` 是否來自相同的 `bin` 目錄，並且是否可以相容使用。

  ```terminal
  C:\>where flutter dart
  C:\path-to-flutter-sdk\bin\flutter
  C:\path-to-flutter-sdk\bin\flutter.bat
  C:\path-to-dart-sdk\bin\dart.exe        :: this should go after `C:\path-to-flutter-sdk\bin\` commands
  C:\path-to-flutter-sdk\bin\dart
  C:\path-to-flutter-sdk\bin\dart.bat
  ```

  As shown above, the command `dart` from the Flutter SDK doesn't come first.
  Update your path to use commands from `C:\path-to-flutter-sdk\bin\` before
  commands from `C:\path-to-dart-sdk\bin\` (in this case).
  After restarting your shell for the change to take effect,
  running the `where` command again
  should show that the `flutter` and `dart` commands
  from the same directory now come first.

  如上所示，Flutter SDK 內的 `dart` 命令不在首位。
  你需要更新 PATH，將 `C:\path-to-flutter-sdk\bin\` 放在 `C:\path-to-dart-sdk\bin\` 前面（當前場景）。
  接著重啟命令列使修改生效，再次執行 `where`，此時來自相同目錄的 `flutter` 和 `dart` 已經排在前面。

  ```terminal
  C:\>where flutter dart
  C:\dev\src\flutter\bin\flutter
  C:\dev\src\flutter\bin\flutter.bat
  C:\dev\src\flutter\bin\dart
  C:\dev\src\flutter\bin\dart.bat
  C:\dev\src\dart-sdk\bin\dart.exe
  ```

  However, if you are using `PowerShell`, in it `where` is
  an alias of `Where-Object` command, so you need to use `where.exe` instead.

  然而，如果你在使用 `PowerShell`，`where` 其實是 `Where-Object` 命令的別名，
  所以實際上你需要執行 `where.exe`。

  ```terminal
  PS C:\> where.exe flutter dart
  ```

  To learn more about the `dart` command, run `dart -h`
  from the command line, or see the [dart tool][] page.

  瞭解更多 `dart` 命令的用法，可以在命令列中執行 `dart -h` 檢視，
  也可以存取 [DartVM 執行環境][dart tool]。

{{site.alert.end}}

[dart tool]: {{site.dart-site}}/tools/dart-tool
