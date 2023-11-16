## CPU Profiler

## CPU 分析器

Start recording a CPU profile by clicking **Record**.
When you are done recording, click **Stop**. At this point,
CPU profiling data is pulled from the VM and displayed
in the profiler views (Call Tree, Bottom Up, and Flame Chart).

單擊 **Record** 開始進行記錄 CPU 資訊，
完成後點選 **Stop** 停止記錄，
CPU 分析器會把收集的資訊推送到 VM，
並分別在不同的資訊視窗進行展示呼叫樹 
（呼叫棧、從下到上的資訊以及火焰圖）。

### Profile granularity

### 分析粒度

The default rate at which the VM collects CPU samples
is 1 sample / 250 μs.  This is selected by default on
the CPU profiler view as "Profile granularity: medium".
This rate can be modified using the selector at the top
of the page. The sampling rates for low, medium,
and high granularity are 1 / 1000 μs, 1 / 250 μs, and 1 / 50 μs,
respectively. It is important to know the trade-offs
of modifying this setting.

VM 收集 CPU 樣本的預設速率為 1/250μs (即每 250 微秒收集一次資料)。
一般情況下，`Profile granularity` 的預設值為 “medium”。
可以透過頁面頂部下拉列表進行修改。抽樣率低、中、高粒度分別
順序對應 1/50μs、1/250μs 和 1/1000μs。
正確設定此值對效能分析非常重要。

A **higher granularity** profile has a higher sampling rate,
and therefore yields a fine-grained CPU profile with more samples.
This might also impact performance of your app since the VM
is being interrupted more often to collect samples. This also
causes the VM's CPU sample buffer to overflow more quickly.
The VM has limited space where it can store CPU sample information.
At a higher sampling rate, the space fills up and begins
to overflow sooner than it would have if a lower sampling
rate was used. This means that you might not have access to CPU samples
from the beginning of the recorded profile.

**高粒度** 的配置會具有更高效的取樣率，因此單元時間內採集的 CPU 資訊會更加詳細且採集範例更多。
因些 VM 會被經常中斷以收集樣本資料，所以這有可能會影響你的應用程式的執行或導致效能下降。
VM 中 CPU 範例資料資訊的儲存空間是受限制的，所以也會導致 VM 的 CPU 範例緩衝區很快地填充滿且會產生溢位。
相對低取樣率，高取樣率儲存空間會被迅速填滿並會出現溢位。一旦空間溢位，就有可能導致取樣資料丟失。

A **lower granularity** profile has a lower sampling rate,
and therefore yields a coarse-grained CPU profile with fewer samples.
However, this impacts your app's performance less.
The VM's sample buffer also fills more slowly, so you can see
CPU samples for a longer period of app run time. This means that
you have a better chance of viewing CPU samples from the beginning
of the recorded profile.

**低粒度** 的配置具有較低的取樣率，
因此單元時間內採集的 CPU 資訊會比較粗略且採集範例較少。
當然，這樣也會對你的應用程式效能影響更小。
VM 範例緩衝區填充速度也會較慢，
因此你可以採集到相當長一段時間內應用程式的 CPU 範例資料，
這也意味著你有更好的機會去檢視 CPU 範例資料。

### Flame chart

### 火焰圖表

This tab of the profiler shows CPU samples for the recorded duration.
This chart should be viewed as a top-down stack trace, where the
top-most stack frame calls the one below it. The width of each stack
frame represents the amount of time it consumed the CPU. Stack frames
that consume a lot of CPU time might be a good place to look for possible
performance improvements.

火焰圖選項卡主要用於顯示一段持續時間內 CPU 的樣本資訊。
圖表展示的是自上而下的呼叫堆疊資訊，即上面的堆疊幀呼叫下面的堆疊幀。
每一個堆疊幀的寬度代表 CPU 執行的時長。棧幀消耗 CPU 的時間越長，
就越洽有可能是我們進行效能改進的好地方。

![Screenshot of a flame chart]({{site.url}}/assets/images/docs/tools/devtools/cpu_profiler_flame_chart.png){:width="100%"}

### Call tree

### 呼叫樹 (也叫追蹤樹)

The call tree view shows the method trace for the CPU profile.
This table is a top-down representation of the profile,
meaning that a method can be expanded to show its _callees_.

呼叫樹檢視是一種自上而下展示 CPU 中的呼叫堆疊資訊方法。
在下圖中的表格中可以看出，展開其中的一個方法可以檢視它所有的 **呼叫者**。

<dl markdown="1">
<dt markdown="1">**<t>Total time</t><t>總時間</t>**</dt>
<dd markdown="1"><p><t>Time the method spent executing its own code as well as
    the code for its callees.</t><t>此方法執行的總時間，
    包括了呼叫者的執行時間(即呼叫此方法整個的生命週期時長)。</t></p></dd>
<dt markdown="1">**<t>Self time</t><t>自執行時間</t>**</dt>
<dd markdown="1"><t>Time the method spent executing only its own code.</t><t>僅表示執行當前方法把花費的時長。</t></dd>
<dt markdown="1">**<t>Method</b></t><t>方法</t>**</dt>
<dd markdown="1"><t>Name of the called method.</t><t>呼叫的方法名稱。</t></dd>
<dt markdown="1">**<t>Source</t><t>原始碼</t>**</dt>
<dd markdown="1"><t>File path for the method call site.</t><t>方法所在的檔案路徑。</t></dd>
</dl>

![Screenshot of a call tree table]({{site.url}}/assets/images/docs/tools/devtools/cpu_profiler_call_tree.png){:width="100%"}

### Bottom up

### 自下而上

The bottom up view shows the method trace for the CPU profile but,
as the name suggests, it's a bottom-up representation of the profile.
This means that each top-level method in the table is actually the
last method in the call stack for a given CPU sample (in other words,
it's the leaf node for the sample).

**自下而上** 檢視也是用於顯示方法呼叫堆疊，
但顧名思義，它是一個自下而上的表示方式。
這意味著表格中的每個最上方的方法實際上是
給定 CPU 樣本的呼叫堆疊中的最後一個方法
(換句話說，這是樣本的葉節點)。

In this table, a method can be expanded to show its _callers_.

在這張表中，可以展開一個方法檢視它的所有 **呼叫者**。

<dl markdown="1">
<dt markdown="1">**<t>Total time</t><t>總時間</t>**</dt>
<dd markdown="1"><p><t>Time the method spent executing its own code
    as well as the code for its callee.</t><t>此方法執行的總時間，
    包括了呼叫者的執行時間(即呼叫此方法整個的生命週期時長)。</t></p></dd>

<dt markdown="1">**<t>Self time</t><t>自執行時間</t>**</dt>
<dd markdown="1"><p><t>For top-level methods in the bottom-up tree
    (leaf stack frames in the profile), this is the time the
    method spent executing only its own code. For sub nodes
    (the callers in the CPU profile), this is the self time
    of the callee when being called by the caller.
    In the following example, the self time of the caller
    `createRenderObject` is equal to the self time of
    the callee `debugCheckHasDirectionality` when being called by
    the caller.</t><t>在自下而上呼叫樹中對於最最上層的方法（葉堆疊幀），
    它表示執行自己的程式碼所需要的時間。
    對於子節點(呼叫者)，它表示呼叫者執行被呼叫者的時間。
    在下面的這個例子中，呼叫者 `createRenderObject` 
    的執行時間等於被呼叫者 `debugCheckHasDirectionality` 的執行時間。</t></p></dd>

<dt markdown="1">**<t>Method</t><t>方法</t>**</dt>
<dd markdown="1"><t>Name of the called method.</t><t>呼叫方法的名稱。</t></dd>

<dt markdown="1">**<t>Source</t><t>原始碼</t>**</dt>
<dd markdown="1"><t>File path for the method call site.</t><t>方法所在的檔案路徑。</t></dd>
</dl>

![Screenshot of a bottom up table]({{site.url}}/assets/images/docs/tools/devtools/cpu_profiler_bottom_up.png){:width="100%"}
