---
title: Using the app size tool
title: 使用應用體積工具
description: Learn how to use the DevTools app size tool.
description: 學習如何使用 DevTools 中的應用體積工具。
---

## What is it?

## 這是什麼?

The app size tool allows you to analyze the total size of your app.
You can view a single snapshot of "size information"
using the [Analysis tab][], or compare two different
snapshots of "size information" using the [Diff tab][].

應用程式體積工具可讓您分析應用的總體積。 
您可以使用 [Analysis 標籤][Analysis tab] 來檢視「體積資訊」的單個快照，
或使用 [Diff 標籤][Diff tab] 比較使用「體積資訊」的兩個不同快照。

### What is "size information"?

### 什麼是“體積資訊”？

"Size information" contains size data for Dart code,
native code, and non-code elements of your app,
like the application package, assets and fonts. A "size
information" file contains data for the total picture
of your application size.

「體積資訊」包含 Dart 程式碼、原生程式碼和非程式碼部分（比如應用套件，資源和字型）。
一個「體積資訊」檔案包含您應用的所有圖片資料。

### Dart size information

### Dart 體積資訊

The Dart AOT compiler performs tree-shaking on your code
when compiling your application (profile or release mode
only&mdash;the AOT compiler is not used for debug builds,
which are JIT compiled). This means that the compiler
attempts to optimize your app's size by removing
pieces of code that are unused or unreachable.

Dart AOT 編譯器會在編譯應用程式時對程式碼進行搖樹最佳化
（僅限 profile 或 release 模式 -
AOT 編譯器不用於 debug 產生，debug 模式是 JIT 編譯的）。
這意味著，編譯器會嘗試刪除未使用或無法存取的程式碼，對應用體積進行最佳化。

After the compiler optimizes your code as much as it can,
the end result can be summarized as the collection of packages,
libraries, classes, and functions that exist in the binary output,
along with their size in bytes. This is the Dart portion of
"size information" we can analyze in the app size tool to further
optimize Dart code and track down size issues.

當編譯器盡全力最佳化您的程式碼後，
產出的二進位制檔案會包含依賴、庫、類和函式的集合，
以及他們的體積（以位元組為單位）。
這是我們可以在應用體積工具中分析的 Dart 部分的「體積資訊」,
有了這些資訊，我們便可以進一步最佳化 Dart 程式碼，並且追蹤體積問題。

## How to use it

## 如何使用

If DevTools is already connected to a running application,
navigate to the "App Size" tab.

如果 DevTools 已經連線到了一個正在執行的應用，點選 "App Size" 標籤。

![Screenshot of app size tab]({{site.url}}/assets/images/docs/tools/devtools/app_size_tab.png)

If DevTools is not connected to a running application, you can
access the tool from the landing page that appears once you have launched
DevTools (see [installation instructions][]).

如果 DevTools 未連線到應用，您可以從啟動 DevTools 後出現的登入頁存取該工具
（檢視[安裝說明][installation instructions]）。

![Screenshot of app size access on landing page]({{site.url}}/assets/images/docs/tools/devtools/app_size_access_landing_page.png){:width="100%"}

## Analysis tab

## 分析標籤頁

The analysis tab allows you to inspect a single snapshot
of size information.  You can view the hierarchical structure
of the size data using the treemap and table,
and you can view code attribution data
(for example, why a piece of code is included in your compiled
application) using the dominator tree and call graph.

「分析」標籤頁允許您檢查體積資訊的單個快照。
您可以看到層次結構的樹狀圖和表格，
並且可以使用 "dominator tree" 和 "call graph" 看到程式碼的屬性資料
（例如：為什麼編譯後的應用程式中包含一段程式碼）。

![Screenshot of app size analysis]({{site.url}}/assets/images/docs/tools/devtools/app_size_analysis.png){:width="100%"}

### Loading a size file

### 讀取一個體積檔案

When you open the Analysis tab, you'll see instructions
to load an app size file. Drag and drop an app size
file into the dialog, and click "Analyze Size".

當您開啟分析標籤頁時，您可以看到載入一個體積檔案的使用說明。
拖動一個尺寸檔案到彈框中，並點選 "Analyze Size"。

![Screenshot of app size analysis loading screen]({{site.url}}/assets/images/docs/tools/devtools/app_size_load_analysis.png){:width="100%"}

See [Generating size files][] below for information on
generating size files.

檢視 [產生體積檔案][Generating size files] 可以得到有關產生尺寸檔案的資訊。

### Treemap and table

### 樹狀圖和表格

The treemap and table show the hierarchical data for your app's size.

樹狀圖和表格可以檢視您的應用體積的結構化資料。

#### Using the treemap

#### 使用樹狀圖

A treemap is a visualization for hierarchical data.
The space is broken up into rectangles,
where each rectangle is sized and ordered by some quantitative
variable (in this case, size in bytes).
The area of each rectangle is proportional to the size
the node occupies in the compiled application. Inside
of each rectangle (call one A), there are additional
rectangles that exist one level deeper in the data
hierarchy (children of A).

樹狀圖是資料結構的視覺化表示。
在檢視中，空間被分解成矩形，
其中每個矩形的體積和順序由一些定量變數決定 (在本例中，體積以位元組為單位)。
每個矩形的面積與節點在編譯後的應用程式中所佔的大小成比例關係。
在每個矩形（稱為 A）的內部，
還有更多的矩形存在於資料層次結構的更深層（A 的子級）。

To drill into a cell in the treemap, select the cell.
This re-roots the tree so that the selected cell becomes
the visual root of the treemap.

要檢視樹狀圖中的單元格的詳情，請選擇這個單元格。
這將重新確定樹的根節點，以便選中的單元格作為樹狀圖中新的根節點。

To navigate back, or up a level, use the breadcrumb
navigator at the top of the treemap.

如果要後退或向上導航，請使用樹對映頂部的麵包屑導航。

![Screenshot of treemap breadcrumb navigator]({{site.url}}/assets/images/docs/tools/devtools/treemap_breadcrumbs.png){:width="100%"}

### Dominator tree and call graph

### 支配樹和呼叫圖

This section of the page shows code size attribution data
(for example, why a piece of code is included in your
compiled application). This data is visible
in the form of a dominator tree as well as a call graph.

這個部分顯示了程式碼的體積屬性資訊（例如：為什麼編譯後的應用程式中包含一段程式碼）。
這些資料以支配樹和呼叫圖的形式呈現。

#### Using the dominator tree

#### 使用支配樹

A [dominator tree][] is a tree where each node's
children are those nodes it immediately dominates.
A node `a` is said to "dominate" a node `b` if
every path to `b` must go through `a`.

[支配樹](https://zh.wikipedia.org/wiki/%E6%94%AF%E9%85%8D_(%E5%9C%96%E8%AB%96)) 是一個樹形結構的圖表，
其子節點可以立刻被支配。
如果通往 `b` 的每條路徑都必經節點 `a`，
那麼我們可以說：節點 `a` 支配了節點 `b`。

[dominator tree]: https://en.wikipedia.org/wiki/Dominator_(graph_theory)

To put it in context of app size analysis,
imagine `package:a` imports both `package:b` and `package:c`,
and both `package:b` and `package:c` import `package:d`.

把它放在應用程式大小分析的上下文中，
想象一下 `package:a` 匯入了 `package:b` 和 `package:c`，
並且 `package:b` 和 `package:c` 都匯入了 `package:d`。

```
package:a
|__ package:b
|   |__ package:d
|__ package:c
    |__ package:d
```

In this example, `package:a` dominates `package:d`,
so the dominator tree for this data would look like:

在這個例子中，`package:a` 支配 `package:d`，所以這個支配樹看起來像是這樣：

```
package:a
|__ package:b
|__ package:c
|__ package:d
```

This information is helpful for understanding why certain
pieces of code are present in your compiled application.
For example, if you are analyzing your app size and find
an unexpected package included in your compiled app, you can
use the dominator tree to trace the package to its root source.

這些資訊對於您而言，可以幫助您理解編譯後的應用程式中為何出現某些程式碼片段。
例如，如果您正在分析應用程式的體積，平行處理現編譯後的應用程式中包含意外的套件，
則可以使用支配樹來追蹤包到其根源。

![Screenshot of code size dominator tree]({{site.url}}/assets/images/docs/tools/devtools/code_size_dominator_tree.png){:width="100%"}

#### Using the call graph

#### 使用呼叫圖

A call graph provides similar information to the dominator
tree in regards to helping you understand why code exists
in a compiled application. However, instead of showing
the one-to-many dominant relationships between nodes of code
size data like the dominator tree, the call graph shows the many-to-many
relationships that existing between nodes of code size data.

呼叫圖提供了與支配樹相似的資訊，
幫助您理解編譯後的應用程式中為何出現某些程式碼片段。
它並不像支配樹一樣提供了一對多的程式碼體積資料節點，
而是展示了程式碼體積資料的節點之間存在的多對多關係。

Again, using the following example:

我們再來看下面這個例子：

```
package:a
|__ package:b
|   |__ package:d
|__ package:c
    |__ package:d
```

The call graph for this data would link `package:d`
to its direct callers, `package:b` and `package:c`,
instead of its "dominator", `package:a`.

此資料的呼叫圖會將直接呼叫者 `package：b` 和 `package：c` 與 `package：d` 連結到一起，
而不是它的「支配者」 `package：a`。

```
package:a --> package:b -->
                              package:d
package:a --> package:c -->
```

This information is useful for understanding the
fine-grained dependencies of between pieces of your code
(packages, libraries, classes, functions).

這些資訊對於理解程式碼片段（包、庫、類和函式）之間的細粒度依賴關係非常有用。

![Screenshot of code size call graph]({{site.url}}/assets/images/docs/tools/devtools/code_size_call_graph.png){:width="100%"}

#### Should I use the dominator tree or the call graph?

#### 我應該使用支配樹還是呼叫圖？

Use the dominator tree if you want to understand the
*root* cause for why a piece of code is included in your
application. Use the call graph if you want to understand
all the call paths to and from a piece of code.

如果您想理解應用程式中包含一段程式碼的 **根本** 原因，請使用支配樹。
如果您想理解一段程式碼之間的所有呼叫路徑，請使用呼叫圖。

A dominator tree is an analysis or slice of call graph data,
where nodes are connected by "dominance" instead of
parent-child hierarchy. In the case where a parent node
dominates a child, the relationship in the call graph and the
dominator tree would be identical, but this is not always the case.

支配樹是呼叫圖資料的分析或切片，其中節點是透過「支配」而不是父子層次結構連線。
在父節點支配子節點的情況下，呼叫圖和支配樹中的關係是相同的，但情況並非總是如此。

In the scenario where the call graph is complete
(an edge exists between every pair of nodes),
the dominator tree would show the that `root` is the
dominator for every node in the graph.
This is an example where the call graph would give
you a better understanding around why a piece of code is
included in your application.

在呼叫圖完成的情況下（每對節點之間存在一條邊），
支配樹將顯示出 `root` 是圖中每個節點的支配者。
呼叫圖可以讓您更好地理解為什麼在應用程式中包含一段程式碼。

## Diff tab

## 差異標籤頁

The diff tab allows you to compare two snapshots of
size information. The two size information files
you are comparing should be generated from two different
versions of the same app; for example,
the size file generated before and after
changes to your code. You can visualize the
difference between the two data sets
using the treemap and table.

diff 標籤頁讓您可以比較體積資訊的兩個快照。
您要比較的兩個體積資訊檔案應該從同一個應用程式的兩個不同版本產生，
例如，在更改程式碼之前和之後產生的體積檔案。
您可以使用樹狀圖和表格視覺化兩個資料集之間的差異。

![Screenshot of app size diff]({{site.url}}/assets/images/docs/tools/devtools/app_size_diff.png){:width="100%"}

### Loading size files

### 讀取體積檔案

When you open the **Diff** tab,
you'll see instructions to load "old" and "new" size
files. Again, these files need to be generated from
the same application. Drag and drop these files into
their respective dialogs, and click **Analyze Diff**.

當您開啟 **Diff** 標籤頁時，您將看到載入「舊」和「新」大小檔案的使用說明。
同樣，這些檔案需要從同一個應用程式產生。
將這些檔案拖放到各自的對話方塊中，然後單擊 **Analyze Diff**。

![Screenshot of app size diff loading screen]({{site.url}}/assets/images/docs/tools/devtools/app_size_load_diff.png){:width="100%"}

See [Generating size files][] below for information
on generating these files.

檢視 [產生體積檔案][Generating size files] 可以得到有關產生這些檔案的資訊。

### Treemap and table

### 樹狀圖和表格

In the diff view, the treemap and tree table show
only data that differs between the two imported size files.

在差異檢視中, 這個樹狀圖和表格只會顯示匯入的兩個檔案中的差異資料。

For questions about using the treemap, see [Using the treemap][] above.

關於樹狀圖的問題，可以檢視[使用樹狀圖][Using the treemap]。

## Generating size files

## 產生尺寸檔案

To use the app size tool, you'll need to generate a
Flutter size analysis file. This file contains size
information for your entire application (native code,
Dart code, assets, fonts, etc.), and you can generate it using the
`--analyze-size` flag:

要使用應用體積工具，您需要產生一個 flutter 體積分析檔案。
此檔案包含整個應用程式的體積資訊（本機程式碼、Dart 程式碼、資源和字型等），
您可以使用 `--analyze size` 標誌產生它：

```
flutter build <your target platform> --analyze-size
```

This builds your application, prints a size summary
to the command line, and prints a line
telling you where to find the size analysis file.

這會建構您的應用並輸出尺寸的摘要到命令列，
同時告訴您在哪裡找到體積分析檔案。

```
flutter build apk --analyze-size --target-platform=android-arm64
...
▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
app-release.apk (total compressed)                               6 MB
...
▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
A summary of your APK analysis can be found at: build/apk-code-size-analysis_01.json
```

In this example, import the `build/apk-code-size-analysis_01.json`
file into the app size tool to analyze further.
For more information, see [App Size Documentation][].

在這個範例中，想要進行更進一步的分析，
可以匯入 `build/apk-code-size-analysis_01.json` 檔案到體積分析工具。
更多資訊，可以檢視 [應用體積尺寸文件][App Size Documentation]。

[Using the treemap]: #using-the-treemap
[Generating size files]: #generating-size-files
[Analysis tab]: #analysis-tab
[Diff tab]: #diff-tab
[installation instructions]: {{site.url}}/development/tools/devtools/overview#install-devtools
[App Size Documentation]: {{site.url}}/perf/app-size#breaking-down-the-size
