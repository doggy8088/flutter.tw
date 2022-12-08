### Update your path

### 更新 PATH 環境變數

You can update your PATH variable for the current session at
the command line, as shown in [Get the Flutter SDK][].
You'll probably want to update this variable permanently,
so you can run `flutter` commands in any terminal session.

正如 [獲取 Flutter SDK][Get the Flutter SDK]
一文中所展示你可以在當前命令列會話中更新你的 PATH 環境變數。
但你也許想讓這個配置永久生效，
這樣就可以在任意一個命令列會話中使用 `flutter` 命令了。

The steps for modifying this variable permanently for
all terminal sessions are machine-specific.
Typically you add a line to a file that is executed
whenever you open a new window. For example:

以下配置環境變數讓其永久生效的方法在不同的機器上有略微的差別。
基本上方法都是在某一個檔案中增加一句命令，
讓它在每次新的命令列視窗開啟時都執行一遍。比如：

 1. Determine the path of your clone of the Flutter SDK.
    You need this in Step 3.

    首先決定你想要將 Flutter SDK 下載或者透過 `git clone` 命令到哪一個目錄內，
    獲取並記錄這個目錄的路徑，你將要在第 3 步中用到它。

 2. Open (or create) the `rc` file for your shell.
    Typing `echo $SHELL` in your Terminal tells you
    which shell you're using.
    If you're using Bash,
    edit `$HOME/.bash_profile` or `$HOME/.bashrc`.
    If you're using Z shell, edit `$HOME/.zshrc`.
    If you're using a different shell, the file path
    and filename will be different on your machine.
 
    開啟或者建立 shell 的 `rc` 檔案，
    比如，在 Linux 和 macOS Mojave 或 Mojave 之前的系統裡，是預設使用 Bash 的，
    所以需要修改 `$HOME/.bashrc` 檔案。
    macOS Catalina 作業系統預設使用 Z Shell，
    所以需要修改 `$HOME/.zshrc` 檔案。
    請知曉，如果你使用不同的 shell，檔案目錄或檔名可能會有所不同。

 3. Add the following line and change
    `[PATH_OF_FLUTTER_GIT_DIRECTORY]` to be
    the path of your clone of the Flutter git repo:

    在檔案中增加下列這行命令，並將其中的 
    `[PATH_OF_FLUTTER_GIT_DIRECTORY]` 更改為你第一步獲取到的路徑：

    ```terminal
    $ export PATH="$PATH:[PATH_OF_FLUTTER_GIT_DIRECTORY]/bin"
    ```

 4. Run `source $HOME/.<rc file>`
    to refresh the current window,
    or open a new terminal window to
    automatically source the file.

    執行 `source $HOME/.bash_profile` 來重新整理當前命令列視窗。

 5. Verify that the `flutter/bin` directory
    is now in your PATH by running:

    透過執行以下命令來驗證 `flutter/bin` 資料夾是否已經新增到 PATH 環境變數中：

    ```terminal
    $ echo $PATH
    ```

    Verify that the `flutter` command is available by running:

    驗證 `flutter` 命令是否可用，可以執行下面的命令檢測：

    ```terminal
    $ which flutter
    ```

[Get the Flutter SDK]: #get-sdk
