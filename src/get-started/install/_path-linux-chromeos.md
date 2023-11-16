### Update your path

### 更新 path

Independent of how you installed Flutter,
you need to add the Flutter SDK to your `PATH`.
You can add Flutter to your `PATH` either for the current session
or for all sessions going forward.

無論你如何安裝 Flutter，
都需要將 Flutter SDK 新增到你的 `PATH` 中。
為了當前會話或今後所有的會話能夠正常使用，
你需要將 Flutter 新增到你的 `PATH` 中。

{% include docs/dart-tool.md %}

#### Update your path for the current session only

#### 僅更新當前會話的 path

To update your `PATH` variable for the current session,
enter this command in your terminal:

請在終端中輸入以下指令，更新當前會話的 `PATH` 變數：

```terminal
$ export PATH="$PATH:[PATH_TO_FLUTTER_GIT_DIRECTORY]/flutter/bin"
```

In this command,
replace `[PATH_TO_FLUTTER_GIT_DIRECTORY]`
with the path to your Flutter SDK install.

在此指令中，
將 `[PATH_TO_FLUTTER_GIT_DIRECTORY]` 替換為 
Flutter SDK 的安裝路徑。

#### Update your path for all future sessions

#### 為今後所有會話更新 path

To add Flutter to your `PATH` for _any_ terminal session, 
follow these steps:

請按照以下步驟，將 Flutter 新增到 _所有_ 終端會話的 `PATH` 中：

1. Find your Flutter SDK installation path.

   找到 Flutter SDK 的安裝路徑。

    ```terminal
    $ find / -type d -wholename "flutter/bin" 2>/dev/null
    ```

    Response should resemble:

    回覆應該類似於：

    ```terminal
    /usr/<example>dev/flutter/bin
    ```

2. Append the following line to your `rc` shell file
   Linux reads the `rc` shell "resource" file each
   time it opens a terminal.

   將下面這一行新增到 `rc` shell 檔案中，
   Linux 在每次開啟終端時都會讀取 `rc` shell 的檔案內容。

   Replace `<path_to_flutter_directory>` with your Flutter path

   將 `<path_to_flutter_directory>` 替換為 Flutter 路徑
   
    ```terminal
    $ echo 'export PATH="$PATH:<path_to_flutter_directory>/flutter/bin"' >> $HOME/.bashrc
    ```
    
3. Reload the current shell profile.

   重新載入當前 shell 配置檔案。
   
    ```terminal
    source $HOME/.<rc file>
    ```
    
4. Verify that the `flutter/bin` directory exists in your `PATH`.

   驗證 `PATH` 中是否存在 `flutter/bin` 目錄。
   
    ```terminal
    $ echo $PATH
    ```
    
    Response should resemble:

    回覆應該類似於：
   
    ```terminal
    /usr/<example>/dev/flutter/bin:/usr/local/git/git-google/bin:/usr/local/git/current/bin:/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin:
    ```
    
5. Verify that you can now use the `flutter` command.

   驗證現在是否可用 `flutter` 指令。
   
    ```terminal
    $ which flutter
    ```

    Response should resemble:

    回覆應該類似於：
   
    ```
	  /usr/<example>/dev/flutter/bin/flutter
	  ```

{% include docs/dart-tool.md %}
