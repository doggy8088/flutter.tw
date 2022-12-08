---
title: 深入理解 Function & Closure
toc: true
---

文/ 鑫磊，滴滴出行實習生

# 前言

在最初設計 Dart 的時候，參考了 `JavaScript` 許多特性。無論是在非同步處理，還是在語法上，都能看到它的影子。熟悉 Dart 的同學應該明白，在 Dart 中一切皆為物件。不僅 `int`、`bool` 是透過 core library 提供的類創建出的物件，連函式也被看作是物件。（本文中可能會出現 **函式** / **方法** 二者僅叫法不同）而本文將帶你深入理解 Dart 的函式 （Function）&閉套件（Closure）以及它們的用法。

## 什麼是 Closure（閉包）

如果你從未聽說過閉套件，沒關係，本節將會從零開始引入閉包這個概念。在正式介紹閉包之前，我們需要先來了解一下 **Lexical scoping**。

### 詞法作用域 Lexical scoping

也許你對這個詞很陌生，但是它卻是最熟悉的陌生人。我們先來看下面一段程式碼。
``` dart
void main() {
  var a = 0;
  var a = 1; //  Error：The name 'a' is already defined
}
```
你肯定已經發現了，我們在該段程式碼中犯了一個明顯的錯誤。那就是定義了兩次變數 `a`，而編譯器也會提示我們，a 這個變數名已經被定義了。

這是由於，我們的變數都有它的 **詞法作用域** ，在同一個詞法作用域中僅允許存在一個名稱為 `a` 的變數，且在編譯期就能夠提示語法錯誤。

這很好理解，如果一個 **Lexical scoping** 中存在兩個同名變數 `a`，那麼我們存取的時候從語法上就無法區分到底你是想要存取哪一個 `a` 了。

 > 上述程式碼中，我們在 `main` 函式的詞法作用域中定義了兩次 a

僅需稍作修改
 ``` dart
 void main() {
  var a = 1; 
  print(a); // => 1
}

 var a = 0;
 ```
我們就能夠正常打印出 `a` 的值為 1。
簡單的解釋，` var a = 0;` 是該 **dart 檔案**的 **Lexical scoping** 中定義的變數，而 `var a = 1;` 是在 main 函式的 **Lexical scoping** 中定義的變數，二者不是一個空間，所以不會產生衝突。

### Function is Object

首先，要證明方法（函式）是一個物件這很簡單。
``` dart
print( (){} is Object ); // true
```
`(){}` 為一個匿名函式，我們可以看到輸出為 `true`。

知道了 Function is Object 還不夠，我們應該如何看待它呢。
``` dart
void main() {
  var name = 'Vadaski';
  
  var printName = (){
    print(name);
  };
}
```
可以很清楚的看到，我們可以在 `main` 函式內定義了一個新的方法，而且還能夠將這個方法賦值給一個變數 `printName`。

但是如果你執行這段程式碼，你將看不到任何輸出，這是為什麼呢。

實際上我們在這裡定義了 `printName` 之後，並沒有真正的去執行它。我們知道，要執行一個方法，需要使用 `XXX()` 才能真正執行。

``` dart
void main() {
  var name = 'Vadaski';
  
  var printName = (){
    print(name);
  };
  
  printName(); // Vadaski
}
```
上面這個例子非常常見，在 `printName` 內部存取到了外部定義的變數 `name`。也就是說，一個 Lexical scoping **內部** 是能夠存取到 **外部** Lexical scoping 中定義的變數的。

### Function + Lexical scoping

**內部**存取**外部**定義的變數是 ok 的，很容易就能夠想到，外部是否可以存取內部定義的變數呢。

如果是正常存取的話，就像下面這樣。

``` dart
void main() {
  
  var printName = (){
    var name = 'Vadaski';
  };
  printName();
  
  print(name); // Error：Undefined name 'name'
}
```

這裡出現了**未定義該變數**的錯誤警告，可以看出 `printName` 中定義的變數，對於 `main` 函式中的變數是不可見的。Dart 和 JavaScript 一樣具有鏈式作用域，也就是說，**子作用域**可以存取**父（甚至是祖先）作用域**中的變數，而反過來不行。

#### 存取規則

從上面的例子我們可以看出，**Lexical scoping** 實際上是以鏈式存在的。一個 scope 中可以開一個新的 scope，而不同 scope 中是可以允許重名變數的。那麼我們在某個 scope 中存取一個變數，究竟是基於什麼規則來存取變數的呢。

``` dart
void main() {
  var a = 1;
  firstScope(){
    var a = 2;
    print('$a in firstScope'); //2 in firstScope
  }
  print('$a in mainScope'); //1 in mainScope
  firstScope();
}
```

在上面這個例子中我們可以看到，在 main 和 firstScope 中都定義了變數 a。我們在 `firstScope` 中 print，輸出了 `2 in firstScope` 而在 main 中 print 則會輸出 `1 in mainScope` 。

我們已經可以總結出規律了：**近者優先**。

如果你在某個 scope 中存取一個變數，它首先會看當前 scope 中是否已經定義該變數，如果已經定義，那麼就使用該變數。如果當前 scope 沒找到該變數，那麼它就會在它的上一層 scope 中尋找，以此類推，直到最初的 scope。如果所有 scope 鏈上都不存在該變數，則會提示 `Error：Undefined name 'name'`。

> Tip: Dart scope 中的變數是靜態確定的，如何理解呢？
>
> ``` dart
> void main() {
>   print(a); // Local variable 'a' can't be referenced before it is declared
>   var a;
> }
> var a = 0;
> ```
>
> 我們可以看到，雖然在 main 的父 scope 中存在變數 a，且已經賦值，但是我們在 main 的 scope 中也定義了變數 a。因為是靜態確定的，所以在 print 的時候會優先使用當前 scope 中定義的 a，而這時候 a 的定義在 print 之後，同樣也會導致編譯器錯誤：Local variable 'a' can't be referenced before it is declared。

### Closure 的定義

有了上面這些知識，我們現在可以來看看 Closure 的定義了。
> A closure is a function object that has access to variables in its lexical scope, even when the function is used outside of its original scope.

> 閉包 即一個函式物件，即使函式物件的呼叫在它原始作用域之外，依然能夠存取在它詞法作用域內的變數。

你可能對這段話還是很難一下就理解到它到底在說什麼。如果簡要概括 Closure 的話，它實際上就是**有狀態**的函式。

### 函式狀態

#### 無狀態函式
通常我們執行一個函式，它都是**無狀態**的。你可能會產生疑問，啥？狀態？？我們還是看一個例子。
``` dart
void main() {
  printNumber(); // 10
  printNumber(); // 10
}

void printNumber(){
  int num = 0;
  for(int i = 0; i < 10; i++){
    num++;
  }
  print(num);
}
```
上面的程式碼很好預測，它將會輸出兩次 10，我們多次呼叫一個函式的時候，它還是會得到一樣的輸出。

但是，當我們理解 Function is Object 之後，我們應該如何從 Object 的角度來看待函式的執行呢。

顯然 `printNumber();` 建立了一個 Function 物件，但是我們沒有將它賦值給任何變數，下次一個 `printNumber();` 實際上建立了一個新的 Function，兩個物件都執行了一遍方法體，所以得到了相同的輸出。

#### 有狀態函式
無狀態函式很好理解，我們現在可以來看看有狀態的函數了。
``` dart
void main() {
  var numberPrinter = (){
    int num = 0;
    return (){
      for(int i = 0; i < 10; i++){
        num++;
      }
      print(num);
    };
  };
  
  var printNumber = numberPrinter();
  printNumber(); // 10
  printNumber(); // 20
}
```
上面這段程式碼同樣執行了兩次 `printNumber();`，然而我們卻得到了不同的輸出 10，20。好像有點 **狀態** 的味道了呢。

但看上去似乎還是有些難以理解，讓我們一層一層來看。

``` dart
var numberPrinter = (){
    int num = 0;
    /// execute function
  };
```
首先我們定義了一個 Function 物件，然後把交給 `numberPrinter` 管理。在創建出來的這個 Function 的 **Lexical scoping** 中定義了一個 num 變數，並賦值為 0。
> 注意：這時候該方法並不會立刻執行，而是等呼叫了 `numberPrinter()` 的時候才執行。所以這時候 num 是不存在的。

``` dart
return (){
    for(int i = 0; i < 10; i++){
        num++;
    }
    print(num);
};
```
然後返回了一個 Function。這個 Function 能夠拿到其父級 scope 中的 num ，並讓其增加 10，然後列印 `num` 的值。

``` dart
var printNumber = numberPrinter();
```
然後我們透過呼叫 numberPrinter()，建立了該 Function 物件，**這就是一個 Closure！** 這個物件**真正執行**我們剛才定義的 `numberPrinter`，並且在它的內部的 scope 中就定義了一個 int 型別的 `num`。然後返回了一個方法給 `printNumber`。

> 實際上返回的 匿名 Function 又是另一個閉包了。

然後我們執行第一次 `printNumber()`，這時候將會獲得閉包儲存的 num 變數，執行下面的內容。
``` dart
// num: 0
for(int i = 0; i < 10; i++){
    num++; 
}
print(num);
```
最開始 printNumber 的 scope 中儲存的 num 為 0，所以經過 10 次自增，num 的值為 10，最後 `print` 列印了 10。

而第二次執行 `printNumber()` 我們使用的還是同一個 `numberPrinter` 物件，這個物件在第一次執行完畢後，其 num 已經為 10，所以第二次執行後，是從 10 開始自增，那麼最後 `print` 的結果自然就是 20 了。

在整個呼叫過程中，printNumber 作為一個 closure，它儲存了內部 num 的狀態，只要 printNumber 不被回收，那麼其內部的所有物件都不會被 GC 掉。

> 所以我們也需要注意到閉包可能會造成記憶體洩漏，或帶來記憶體壓力問題。

### 到底啥是閉包
再回過頭來理解一下，我們對於閉套件的定義就應該好理解了。

> 閉包 即一個函式物件，即使函式物件的呼叫在它原始作用域之外，依然能夠存取在它詞法作用域內的變數。

在剛才的例子中，我們的 num 是在 `numberPrinter` 內部定義的，可是我們可以透過返回的 Function 在外部存取到了這個變數。而我們的 `printNumber` 則一直儲存了  `num`。

## 分階段看閉包
在我們使用閉套件的時候，我將它看為三個階段。
### 定義階段
這個階段，我們定義了 Function 作為閉套件，但是卻沒有真正執行它。
``` dart
void main() {
  var numberPrinter = (){
    int num = 0;
    return (){
      print(num);
    };
  };
```
這時候，由於我們只是定義了閉套件，而沒有執行，所以 num 物件是不存在的。

### 建立階段
``` dart
var printNumber = numberPrinter();
```
這時候，我們真正執行了 nu mberPrinter 閉套件的內容，並返回執行結果，num 被創建出來。這時候，只要 printNumber 不被 GC，那麼 num 也會一直存在。

### 存取階段
``` dart
printNumber(); 
printNumber();
```
然後我們可以透過某種方式存取 numberPrinter 閉套件中的內容。(本例中間接訪問了 num)

以上三個階段僅方便理解，不是嚴謹描述。
## Closure 的應用
如果僅是理解概念，那麼我們看了可能也就忘了。來點實在的，到底 Closure 可以怎麼用？

### 在傳遞物件的位置執行方法
比如說我們有一個 Text Widget 的內容有些問題，直接給我們 show 了一個 Error Widget。這時候，我想列印一下這個內容看看到底發生了啥，你可以這樣做。

``` dart
Text((){
    print(data);
    return data;
}())
```
是不是很神奇，竟然還有這種操作。
> Tip 立即執行閉包內容：我們這裡透過閉套件的語法 `(){}()` 立刻執行閉套件的內容，並把我們的 data 返回。

雖然 Text 這裡僅允許我們傳一個 String，但是我依然可以執行 `print` 方法。

另一個 case 是，如果我們想要僅在 debug 模式下執行某些陳述式，也可以透過 closure  配合斷言來實現。

``` dart
assert(() {
   child.owner._debugElementWasRebuilt(child);// execute some code
   return true;
}());
```

解釋一下，首先 assert 斷言僅在 debug 模式下才會開啟，所以斷言內的內容可以僅在 debug 模式才得以執行。

然後我們知道，Function( ) 呼叫就會執行，所以這裡我們透過匿名閉包 `(){}()` 立刻執行了閉套件中的內容，並返回 true 給斷言，讓它不會掛掉。從而達到了僅在 debug 模式下執行該閉包內的陳述式。

### 實現策略模式
透過 closure 我們可以很方便實現策略模式。
``` dart
void main(){
  var res = exec(select('sum'),1 ,2);
  print(res);
}

Function select(String opType){
  if(opType == 'sum') return sum;
  if(opType == 'sub') return sub;
  return (a, b) => 0;
}

int exec(NumberOp op, int a, int b){
  return op(a,b);
}

int sum(int a, int b) => a + b;
int sub(int a, int b) => a - b;

typedef NumberOp = Function (int a, int b);
```
透過 select 方法，可以動態選擇我們要執行的具體方法。你可以在 https://dartpad.cn/143c33897a0eac7e2d627b01983b7307 執行這段程式碼。

### 實現 Builder 模式 / 延遲載入
如果你有 Flutter 經驗，那麼你應該使用過 `ListView.builder`，它很好用對不對。我們只向 builder 屬性傳一個方法，`ListView` 就可以根據這個 `builder` 來建構它的每一個 item。實際上，這也是 closure 的一種體現。

``` dart
ListView.builder({
//...
    @required IndexedWidgetBuilder itemBuilder,
//...
  })
  
typedef IndexedWidgetBuilder = Widget Function(BuildContext context, int index);
```
Flutter 透過 typedef 定義了一種 Function，它接收 `BuildContext` 和 `int` 作為引數，然後會返回一個 Widget。對這樣的 Function 我們將它定義為 `IndexedWidgetBuilder` 然後將它內部的 Widget 返回出來。這樣外部的 scope 也能夠存取 `IndexedWidgetBuilder` 的 scope 內部定義的 Widget，從而實現了 builder 模式。

> 同樣，ListView 的延遲載入（延遲執行）也是閉包很重要的一個特性哦～

## 牛刀小試
在學習了 closure 以後，我們來道題檢驗一下你是否真正理解了吧～
``` dart
main(){
  var counter = Counter(0);
  fun1(){
    var innerCounter = counter;
    Counter incrementCounter(){
      print(innerCounter.value);
      innerCounter.increment();
      return innerCounter;
    }
    return incrementCounter;
  }

  var myFun = fun1();
  print(myFun() == counter);
  print(myFun() == counter);
}

class Counter{
  int value;
  Counter(int value) 
  : this.value = value;

  increment(){
    value++;
  }
}
```

上面這段程式碼會輸出什麼呢？

如果你已經想好了答案，就來 <a href="https://dartpad.cn/75e338c727ae608cd31d389f7557a0f1">DartPad 線上執行</a> 看看是否正確吧！

## 寫在最後

本文非常感謝 [@Realank Liu](https://juejin.im/user/5b5ae02df265da0f9e58a9a7) 的 Review 以及寶貴的建議～

時隔半年來遲遲的更新，不知道是否對大家有點幫助呢～ Closure 在實現 Flutter 的諸多功能上都發揮著重要的作用，可以說它已經深入你程式設計的日常，默默幫助我們更好地編寫 Dart 程式碼，作為一名不斷精進的 Dart 開發者，是時候用起來啦～之後的文章中，我會逐漸轉向 Dart，給大家帶來更深入的內容，敬請期待！

如果您對本文還有任何疑問或者文章的建議，歡迎透過我的郵箱 xinlei966@gmail.com 與我聯絡，我會及時回覆！

