### Update your path

### 更新你的環境變數

You can update your PATH variable for the current session at
the command line, as shown in [Get the Flutter SDK][].
You'll probably want to update this variable permanently,
so you can run `flutter` commands in any terminal session.

與 [獲取 Flutter SDK][Get the Flutter SDK] 中說的一樣，
你可以透過命令列更新當前視窗的環境變數。
但你也許會想要讓它一直生效，在任何終端中都可以執行 `flutter` 命令。

The steps for modifying this variable permanently for
all terminal sessions are machine-specific.
Typically you add a line to a file that is executed
whenever you open a new window. For example:

下面是更改環境變數的詳細步驟，這將會對所有終端生效，且僅在該機器上生效。
通常，每當你開啟一個新的視窗，都會將一行新增到執行的檔案。例如：

 1. Determine the path of your clone of the Flutter SDK.
    You need this in Step 3.

    找到透過壓縮包下載解壓或 `git clone` 命令檢出的 Flutter SDK 的資料夾，
    你需要在第三步用到它。

 2. Open (or create) the `rc` file for your shell.
    For example, Linux uses the Bash shell by default,
    so edit `$HOME/.bashrc`.
    If you are using a different shell, the file path
    and filename will be different on your machine.

    在你的 shell 中開啟（或者建立）`rc` 檔案。
    例如，Linux 預設使用 Bash shell，所以編輯 `$HOME/.bashrc` 檔案。
    如果你使用不同的 shell，那麼在你電腦上的檔案路徑以及檔名必須不同。

 3. Add the following line and change
    `[PATH_OF_FLUTTER_GIT_DIRECTORY]` to be
    the path of your clone of the Flutter git repo:

    並將下面命令列裡 `[PATH_OF_FLUTTER_GIT_DIRECTORY]`
    修改為你透過 `git clone` 命令檢出的 Flutter 儲存庫目錄地址，
    或者透過下載 Flutter SDK 壓縮包解壓之後的目錄地址。

    ```terminal
    $ export PATH="$PATH:[PATH_OF_FLUTTER_GIT_DIRECTORY]/bin"
    ```

 4. Run `source $HOME/.<rc file>`
    to refresh the current window,
    or open a new terminal window to
    automatically source the file.

    執行 `source $HOME/.<rc file>` 重新整理當前視窗或者開啟一個新的終端視窗就會自動更新這個檔案。

 5. Verify that the `flutter/bin` directory
    is now in your PATH by running:

    透過執行以下命令驗證 `flutter/bin` 確實加到環境變數中了：

    ```terminal
    $ echo $PATH
    ```

    Verify that the `flutter` command is available by running:

    透過執行以下命令驗證 `flutter` 命令是否可用：

    ```terminal
    $ which flutter
    ```

{% include docs/dart-tool.md %}

### Update path directly

### 直接更新環境變數

In some cases, your distribution may not permanently acquire
the path when using the above directions. When this occurs,
you can change the environment variables file directly.
These instructions require administrator privileges:

在某些情況下，你的分發可能不會一直使用上面提到的環境變數。
在這種情況下，你可以直接更改環境變數檔案。但這需要管理員許可權：

   1. Determine the path of your clone of the Flutter SDK.

      找到你存放 Flutter SDK 的路徑。

   2. Locate the `etc` directory at the root of the system,
      and open the `profile` file with root privileges.

      定位系統根目錄下的 `etc` 資料夾，然後用 root 許可權開啟 `profile` 檔案。

        ```terminal
        $ sudo nano /etc/profile
        ```

   3. Update the PATH string with the location of your
      Flutter SDK directory.

      更新 Flutter SDK 資料夾所在路徑的字串。

      ```shell
      if [ "`id -u`" -eq 0 ]; then
         PATH="..."
      else
         PATH="/usr/local/bin:...:[PATH_OF_FLUTTER_GIT_DIRECTORY]/bin"
      fi
      export PATH
      ```

   4. End the current session or reboot your system.

      結束當前會話並重啟系統。

   5. Once you start a new session, verify that the
      `flutter` command is available by running:

      當你重新啟動了新會話，請確認 `flutter` 命令已經可以執行：

      ```terminal
      $ which flutter
      ```

For more details on setting the path in Bash,
see [this StackExchange question][bash].
For information on setting the path in Z shell,
see [this StackOverflow question][zsh].

更多關於如何在 Bash 中配置環境變數的資訊請檢視 [這條 StackExchange 提問][bash]。
更多關於如何在 Z shell 中配置環境變數的資訊請檢視 [這條 StackExchange 提問][zsh]。


[Get the Flutter SDK]: #get-sdk
[bash]: https://unix.stackexchange.com/questions/26047/how-to-correctly-add-a-path-to-path
[zsh]: {{site.so}}/questions/11530090/adding-a-new-entry-to-the-path-variable-in-zsh
