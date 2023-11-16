## Profile or release runs

## 以 profile 模式執行

{{site.alert.important}}

  Do _not_ test the performance of your app with debug and
  hot reload enabled.
  
  **請勿** 在除錯模式和熱重載功能開啟的情況下做效能測試。
  
{{site.alert.end}}

So far you've been running your app in *debug* mode. Debug
mode trades performance for useful developer features such
as hot reload and step debugging. It's not unexpected to
see slow performance and janky animations in debug mode.
Once you are ready to analyze performance or release your
app, you'll want to use Flutter's "profile" or "release"
build modes. For more details, see [Flutter's build modes][].

截止目前文件所示內容，你的應用應該執行在除錯 (debug) 模式中，
這個模式意味著在更大的效能開銷下實現了更快速的開發效率，
比如熱重載功能的啟用，因此你可能要面臨較差品質的動畫效果。
當你準備分析應用效能或要打包釋出的時候，你可能需要 Flutter
的 profile 或者 release 建構，相關文件，請查閱文件：
[Flutter 的建構模式選擇]({{site.url}}/testing/build-modes)。

{{site.alert.important}}
  
  If you're concerned about the package size of your app,
  see [Measuring your app's size][].
  
  如果你關心應用大小，請參考 [這篇文件][Measuring your app's size]。
  
{{site.alert.end}}

[Flutter's build modes]: {{site.url}}/testing/build-modes
[Measuring your app's size]: {{site.url}}/perf/app-size
