## Configuring web app support

## 配置 web 應用支援

On ChromeOS, you do development work in a Linux container.
However, the Chrome browser itself is part of the
parent ChromeOS operating system,
and Flutter doesn't have a means to call it with the required parameters.

在 ChromeOS 上，開發工作是在 Linux 容器中進行的。
然而，Chrome 瀏覽器本身是 ChromeOS 作業系統的一部分，
Flutter 無法使用必要的引數來呼叫它。

Track this issue at [Issue 121462: Improve the web debugging experience on Chromebooks]({{site.github}}/flutter/flutter/issues/121462).

追蹤此問題：[Issue 121462: Improve the web debugging experience on Chromebooks]({{site.github}}/flutter/flutter/issues/121462).

Instead, the best approach is to manually install a second copy of
Chrome in the Linux container. You can do that with the following steps:

最好的辦法是在 Linux 容器中手動安裝第二個 Chrome 瀏覽器副本。
具體步驟如下：

```terminal
$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
$ sudo apt install ./google-chrome-stable_current_amd64.deb
```
