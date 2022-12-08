---
title: 在 Flutter 中使用 TensorFlow Lite 外掛實現文字分類
---

![](https://devrel.andfun.cn/devrel/posts/2020/09/a21e5b12e71bb.png)

文/ Amish Garg，Google Summer of Code(GSoC) 實習生，譯/ Yuan，谷創字幕組，審校/ Xinlei、Lynn Wang，CFUG 社群。

如果您希望能有一種簡單、高效且靈活的方式把 TensorFlow 模型整合到 Flutter 應用裡，那請您一定不要錯過我們今天介紹的這個全新外掛 [tflite_flutter](https://pub.flutter-io.cn/packages/tflite_flutter)。這個外掛的開發者是 Google Summer of Code(GSoC) 的一名實習生 Amish Garg，本文來自他在 Medium 上的一篇文章[《在 Flutter 中使用 TensorFlow Lite 外掛實現文字分類》](https://medium.com/@am15hg/text-classification-using-tensorflow-lite-plugin-for-flutter-3b92f6655982)。

[tflite_flutter](https://pub.flutter-io.cn/packages/tflite_flutter) 外掛的核心特性：

* 它提供了與 TFLite Java 和 Swift API 相似的 Dart API，所以其靈活性和在這些平臺上的效果是完全一樣的
* 透過 dart:ffi 直接與 TensorFlow Lite C API 相繫結，所以它比其它平台整合方式更加高效。
* 無需編寫特定平台的程式碼。
* 透過 NNAPI 提供加速支援，在 Android 上使用 GPU Delegate，在 iOS 上使用 Metal Delegate。

本文中，我們將使用 tflite_flutter 建構一個 **文字分類 Flutter 應用** 帶您體驗 tflite_flutter 外掛，首先從新建一個 Flutter 專案 `text_classification_app` 開始。

### （很重要）初始化配置

#### Linux 和 Mac 使用者


將 [`install.sh`](https://github.com/am15h/tflite_flutter_plugin/blob/master/install.sh) 複製到您應用的根目錄，然後在根目錄執行 `sh install.sh`，本例中就是目錄 `text_classification_app/`。
  
#### Windows 使用者

將 [install.bat](https://github.com/am15h/tflite_flutter_plugin/blob/master/install.bat) 檔案複製到應用根目錄，並在根目錄執行批處理檔案 `install.bat`，本例中就是目錄 `text_classification_app/`。

它會自動從 [release assets](https://github.com/am15h/tflite_flutter_plugin/releases) 下載最新的二進位制資源，然後把它放到指定的目錄下。

請點選到 README 檔案裡檢視更多 [關於初始配置的資訊](https://github.com/am15h/tflite_flutter_plugin#important-initial-setup)。

### 獲取外掛

在 `pubspec.yaml` 新增 `tflite_flutter: ^<latest_version>` （[詳情](https://pub.flutter-io.cn/packages/tflite_flutter#-installing-tab-)）。

### 下載模型

要在移動端上執行 TensorFlow 訓練模型，我們需要使用 `.tflite` 格式。如果需要了解如何將 TensorFlow 訓練的模型轉換為 `.tflite` 格式，請參閱[官方指南](https://tensorflow.google.cn/lite/convert/python_api)。

這裡我們準備使用 TensorFlow 官方站點上預訓練的文字分類模型，可[從這裡下載](https://files.flutter-io.cn/posts/flutter-cn/2020/tensorflow-lite-plugin/text_classification.tflite)。


> 該預訓練的模型可以預測當前段落的情感是積極還是消極。它是基於來自 Mass 等人的  [Large Movie Review Dataset v1.0](http://ai.stanford.edu/~amaas/data/sentiment/) 資料集進行訓練的。資料集由基於 IMDB 電影評論所標記的積極或消極標籤組成，[點選檢視更多資訊](https://tensorflow.google.cn/lite/models/text_classification/overview)。

將 [`text_classification.tflite`](https://files.flutter-io.cn/posts/flutter-cn/2020/tensorflow-lite-plugin/text_classification.tflite) 和 [`text_classification_vocab.txt`](https://files.flutter-io.cn/posts/flutter-cn/2020/tensorflow-lite-plugin/text_classification_vocab.txt) 檔案複製到 text_classification_app/assets/ 目錄下。

在 `pubspec.yaml` 檔案中新增 `assets/`：

<!-- skip -->
```
assets:
  - assets/
```

現在萬事俱備，我們可以開始寫程式碼了。 🚀

### 實現分類器

### 預處理

正如 [文字分類模型頁面](https://tensorflow.google.cn/lite/models/text_classification/overview#how_it_works) 裡所提到的。可以按照下面的步驟使用模型對段落進行分類：

1. 對段落文字進行分詞，然後使用預定義的詞彙集將它轉換為一組詞彙 ID；
2. 將產生的這組詞彙 ID 輸入 TensorFlow Lite 模型裡；
3. 從模型的輸出裡獲取當前段落是積極或者是消極的機率值。

我們首先寫一個方法對原始字串進行分詞，其中使用 [`text_classification_vocab.txt`](https://github.com/am15h/tflite_flutter_plugin/blob/master/example/assets/text_classification_vocab.txt) 作為詞彙集。

在 `lib/` 資料夾下建立一個新檔案 `classifier.dart`。 

這裡先寫程式碼載入 `text_classification_vocab.txt` 到字典裡。

<!-- skip -->
```dart
import 'package:flutter/services.dart';

class Classifier {
  final _vocabFile = 'text_classification_vocab.txt';
  
  Map<String, int> _dict;

  Classifier() {
    _loadDictionary();
  }

  void _loadDictionary() async {
    final vocab = await rootBundle.loadString('assets/$_vocabFile');
    var dict = <String, int>{};
    final vocabList = vocab.split('\n');
    for (var i = 0; i < vocabList.length; i++) {
      var entry = vocabList[i].trim().split(' ');
      dict[entry[0]] = int.parse(entry[1]);
    }
    _dict = dict;
    print('Dictionary loaded successfully');
  }
  
}
```

載入字典

現在我們來編寫一個函式對原始字串進行分詞。

<!-- skip -->
```dart
import 'package:flutter/services.dart';

class Classifier {
  final _vocabFile = 'text_classification_vocab.txt';

  // 單句的最大長度
  final int _sentenceLen = 256;

  final String start = '<START>';
  final String pad = '<PAD>';
  final String unk = '<UNKNOWN>';

  Map<String, int> _dict;
  
  List<List<double>> tokenizeInputText(String text) {
    
    // 使用空格進行分詞
    final toks = text.split(' ');
    
    // 建立一個列表，它的長度等於 _sentenceLen，並且使用 <pad> 的對應的字典值來填充
    var vec = List<double>.filled(_sentenceLen, _dict[pad].toDouble());

    var index = 0;
    if (_dict.containsKey(start)) {
      vec[index++] = _dict[start].toDouble();
    }

    // 對於句子裡的每個單詞在 dict 裡找到相應的 index 值
    for (var tok in toks) {
      if (index > _sentenceLen) {
        break;
      }
      vec[index++] = _dict.containsKey(tok)
          ? _dict[tok].toDouble()
          : _dict[unk].toDouble();
    }

    // 按照我們的直譯器輸入 tensor 所需的形狀 [1,256] 返回 List<List<double>>
    return [vec];
  }
}


```

分詞

### 使用 tflite_flutter 進行分析

這是本文的主體部分，這裡我們會討論 tflite_flutter 外掛的用途。

這裡的分析是指基於輸入資料在裝置上使用 TensorFlow Lite 模型的處理過程。要使用 TensorFlow Lite 模型進行分析，需要透過 **直譯器** 來執行它，[瞭解更多](https://tensorflow.google.cn/lite/guide/inference)。

**建立直譯器，載入模型**

tflite_flutter 提供了一個方法直接透過資源建立直譯器。

<!-- skip -->
```
static Future<Interpreter> fromAsset(String assetName, {InterpreterOptions options})
```

由於我們的模型在 `assets/` 資料夾下，需要使用上面的方法來建立解析器。對於 InterpreterOptions 的相關說明，請 [參考這裡](https://github.com/am15h/tflite_flutter_plugin/blob/master/lib/src/interpreter_options.dart)。

<!-- skip -->
```dart
import 'package:flutter/services.dart';

// 引入 tflite_flutter
import 'package:tflite_flutter/tflite_flutter.dart';

class Classifier {
  // 模型檔案的名稱
  final _modelFile = 'text_classification.tflite';

  // TensorFlow Lite 直譯器物件
  Interpreter _interpreter;

  Classifier() {
    // 當分類器初始化以後載入模型
    _loadModel();
  }

  void _loadModel() async {
    
    // 使用 Interpreter.fromAsset 建立直譯器
    _interpreter = await Interpreter.fromAsset(_modelFile);
    print('Interpreter loaded successfully');
  }

}

```

建立直譯器的程式碼


如果您不希望將模型放在 `assets/` 目錄下，tflite_flutter 還提供了工廠建構函式建立直譯器，[更多資訊](https://github.com/am15h/tflite_flutter_plugin#creating-the-interpreter)。

**我們開始進行分析！**

現在用下面方法啟動分析：

<!-- skip -->
```
void run(Object input, Object output);
```

注意這裡的方法和 Java API 中的是一樣的。

`Object input` 和 `Object output` 必須是和 Input Tensor 與 Output Tensor 維度相同的列表。

要檢視 input tensors 和 output tensors 的維度，可以使用如下程式碼：

<!-- skip -->
```dart
_interpreter.allocateTensors();
// 列印 input tensor 列表
print(_interpreter.getInputTensors());
// 列印 output tensor 列表
print(_interpreter.getOutputTensors());
```

在本例中 text_classification 模型的輸出如下：

<!-- skip -->
```
InputTensorList:
[Tensor{_tensor: Pointer<TfLiteTensor>: address=0xbffcf280, name: embedding_input, type: TfLiteType.float32, shape: [1, 256], data:  1024]
OutputTensorList:
[Tensor{_tensor: Pointer<TfLiteTensor>: address=0xbffcf140, name: dense_1/Softmax, type: TfLiteType.float32, shape: [1, 2], data:  8]
```

現在，我們實現分類方法，該方法返回值為 1 表示積極，返回值為 0 表示消極。

<!-- skip -->
```dart
int classify(String rawText) {
    
    //  tokenizeInputText 返回形狀為 [1, 256] 的 List<List<double>>
    List<List<double>> input = tokenizeInputText(rawText);
   
    // [1,2] 形狀的輸出
    var output = List<double>(2).reshape([1, 2]);
    
    // run 方法會執行分析並且儲存輸出的值
    _interpreter.run(input, output);

    var result = 0;
    // 如果輸出中第一個元素的值比第二個大，那麼句子就是消極的
    
    if ((output[0][0] as double) > (output[0][1] as double)) {
      result = 0;
    } else {
      result = 1;
    }
    return result;
  }

```

用於分析的程式碼

在 tflite_flutter 的 extension ListShape on List 下面定義了一些使用的擴充：

<!-- skip -->
```dart
// 將提供的列表進行矩陣變形，輸入引數為元素總數 // 保持相等 
// 用法：List(400).reshape([2,10,20]) 
// 返回  List<dynamic>

List reshape(List<int> shape)
// 返回列表的形狀
List<int> get shape
// 返回列表任意形狀的元素數量
int get computeNumElements

```

最終的 `classifier.dart` 應該是這樣的：

<!-- skip -->
```dart
import 'package:flutter/services.dart';

// 引入 tflite_flutter
import 'package:tflite_flutter/tflite_flutter.dart';

class Classifier {
  // 模型檔案的名稱
  final _modelFile = 'text_classification.tflite';
  final _vocabFile = 'text_classification_vocab.txt';

  // 陳述式的最大長度
  final int _sentenceLen = 256;

  final String start = '<START>';
  final String pad = '<PAD>';
  final String unk = '<UNKNOWN>';

  Map<String, int> _dict;

  // TensorFlow Lite 直譯器物件
  Interpreter _interpreter;

  Classifier() {
    // 當分類器初始化的時候載入模型
    _loadModel();
    _loadDictionary();
  }

  void _loadModel() async {
    // 使用 Intepreter.fromAsset 建立解析器
    _interpreter = await Interpreter.fromAsset(_modelFile);
    print('Interpreter loaded successfully');
  }

  void _loadDictionary() async {
    final vocab = await rootBundle.loadString('assets/$_vocabFile');
    var dict = <String, int>{};
    final vocabList = vocab.split('\n');
    for (var i = 0; i < vocabList.length; i++) {
      var entry = vocabList[i].trim().split(' ');
      dict[entry[0]] = int.parse(entry[1]);
    }
    _dict = dict;
    print('Dictionary loaded successfully');
  }

  int classify(String rawText) {
    // tokenizeInputText  返回形狀為 [1, 256] 的 List<List<double>>
    List<List<double>> input = tokenizeInputText(rawText);

    //輸出形狀為 [1, 2] 的矩陣
    var output = List<double>(2).reshape([1, 2]);

    // run 方法會執行分析並且將結果儲存在 output 中。
    _interpreter.run(input, output);

    var result = 0;
    // 如果第一個元素的輸出比第二個大，那麼當前陳述式是消極的

    if ((output[0][0] as double) > (output[0][1] as double)) {
      result = 0;
    } else {
      result = 1;
    }
    return result;
  }

  List<List<double>> tokenizeInputText(String text) {
    // 用空格分詞
    final toks = text.split(' ');

    // 建立一個列表，它的長度等於 _sentenceLen，並且使用 <pad> 對應的字典值來填充
    var vec = List<double>.filled(_sentenceLen, _dict[pad].toDouble());

    var index = 0;
    if (_dict.containsKey(start)) {
      vec[index++] = _dict[start].toDouble();
    }

    // 對於句子中的每個單詞，在 dict 中找到相應的 index 值
    for (var tok in toks) {
      if (index > _sentenceLen) {
        break;
      }
      vec[index++] = _dict.containsKey(tok)
          ? _dict[tok].toDouble()
          : _dict[unk].toDouble();
    }

    // 按照我們的直譯器輸入 tensor 所需的形狀 [1,256] 返回 List<List<double>>
    return [vec];
  }
}

```

現在，可以根據您的喜好實現 UI 的程式碼，分類器的用法比較簡單。

<!-- skip -->
```dart
// 建立 Classifier 物件
Classifer _classifier = Classifier();
// 將目標陳述式作為引數，呼叫 classify 方法
_classifier.classify("I liked the movie");
// 返回 1 （積極的）
_classifier.classify("I didn't liked the movie");
// 返回 0 （消極的）
```

請在這裡查閱完整程式碼：[Text Classification Example app with UI](https://github.com/am15h/tflite_flutter_plugin/tree/master/example/)。

![Text Classification Example App](https://devrel.andfun.cn/devrel/posts/2020/09/3547a17bcd6eb.gif)

文字分類範例應用

瞭解更多關於 tflite_flutter 外掛的資訊，請存取 GitHub repo: [**am15h/tflite_flutter_plugin**](https://github.com/am15h/tflite_flutter_plugin)。

### 答疑

##### 問：[`tflite_flutter`](https://pub.flutter-io.cn/packages/tflite_flutter) 和 [`tflite v1.0.5`](https://pub.flutter-io.cn/packages/tflite) 有哪些區別？

`tflite v1.0.5` 側重於為特定用途的應用場景提供高階特性，比如圖片分類、物體檢測等等。而新的 tflite_flutter 則提供了與 Java API 相同的特性和靈活性，而且可以用於任何 tflite 模型中，它還支援 delegate。

由於使用 dart:ffi (dart ↔️ (ffi) ↔️ C)，tflite_flutter 非常快 (擁有低延時)。而 tflite 使用平台整合 (dart ↔️ platform-channel ↔️ (Java/Swift) ↔️ JNI ↔️ C)。

##### 問：如何使用 tflite_flutter 建立圖片分類應用？有沒有類似 [TensorFlow Lite Android Support Library](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/lite/experimental/support/java/README.md) 的相依套件？

更新（07/01/2020）: TFLite Flutter Helper 開發庫已釋出。

[TensorFlow Lite Flutter Helper Library](https://github.com/am15h/tflite_flutter_helper) 為處理和控制輸入及輸出的 TFLite 模型提供了易用的架構。它的 API 設計和文件與 TensorFlow Lite Android Support Library 是一樣的。更多資訊請 [參考這裡](https://github.com/am15h/tflite_flutter_helper#tensorflow-lite-flutter-helper-library)。

以上是本文的全部內容，歡迎大家對 tflite_flutter 外掛進行反饋，請在這裡 [上報 bug 或提出功能需求](https://github.com/am15h/tflite_flutter_plugin/issues)。

謝謝關注。

感謝 Michael Thomsen。

### 延展閱讀

如果需要關注更多 TensorFlow 和 Google AI 相關內容，請查閱下面資料
- [TensorFlow 微信公眾號](https://mp.weixin.qq.com/s/XCZ3xOZa7x1lfdoiHOLqrw)
- [TensorFlow 官方文件](https://tensorflow.google.cn)
- [最新簡單粗暴 TF 手冊](https://tf.wiki)
- [TensorFlow 交流論壇](https://discuss.tf.wiki)
- [TensorFlow Codelabs](https://codelabs.tf.wiki)