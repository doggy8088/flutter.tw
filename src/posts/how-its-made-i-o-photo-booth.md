---
title: 用 Flutter 和 Firebase 輕鬆建構 Web 應用
toc: true
---

![](https://devrel.andfun.cn/devrel/posts/2021/11/da90dd75700e2.png)

*作者 / Very Good Ventures Team*

我們 (Very Good Ventures 團隊) 與 Google 合作，在今年 5 月推出了 [照相亭互動體驗](https://photobooth.flutter.cn/) (Flutter Photo Booth)。您可以與深受喜愛的 Google 吉祥物合影: [Flutter 的 Dash](https://flutter.cn/dash)、Android Jetpack、Chrome 的 Dino 和 Firebase 的 Sparky，並用各種貼紙裝飾照片，包括派對帽、披薩、時髦眼鏡等。當然，您也可以透過社交媒體下載並分享，或者用作您的個人頭像！

![△ Flutter 的 Dash、Firebase 的 Sparky、Android Jetpack 和 Chrome 的 Dino](https://devrel.andfun.cn/devrel/posts/2021/06/Rjow8V.png)

△ Flutter 的 Dash、Firebase 的 Sparky、Android Jetpack 和 Chrome 的 Dino

我們使用 [Flutter web](https://flutter.cn/web) 和 [Firebase](https://firebase.google.com/) 建構了 Flutter 照相亭。因為 [Flutter 現在支援打造 Web 應用](https://flutter.cn/posts/whats-new-in-flutter-2-0)，我們認為這將是一個很好的方式，可以讓世界各地的與會者在線上輕鬆存取這一應用。Flutter web 消除了必須透過應用商店安裝應用的障礙，同時使用者還可以靈活選擇執行應用的裝置: 移動裝置、桌面裝置或平板電腦。因此，只要能使用瀏覽器，使用者便可無需下載直接使用 Flutter 照相亭。

儘管 Flutter 照相亭旨在提供 Web 體驗，但所有程式碼均採用與平台無關的架構編寫而成。當相機外掛等原生功能的支援在各個平台就緒後，這套程式碼即可在所有平台 (桌面、Web 和移動裝置) 通用。

## **使用 Flutter 建構虛擬照相亭**

**建構 Web 版 Flutter 相機外掛**

第一個挑戰即在 Web 上為 Flutter 建構攝影頭外掛。最初，我們聯絡了 [Baseflow](https://www.baseflow.com/) 團隊，因為他們負責維護現有的開源 [Flutter 攝影頭外掛](https://github.com/Baseflow/flutter-plugins)。Baseflow 致力於建構適用於 iOS 和 Android 的一流攝影頭外掛支援，我們也很樂於與其合作，使用 [聯合外掛](https://flutter.tw/development/packages-and-plugins/developing-packages#federated-plugins) 方法為外掛提供 Web 支援。我們儘可能符合官方外掛介面，以便我們可以在準備就緒時將其合併回官方外掛。

我們確定了兩個對於在 Flutter 中建構 Flutter 照相亭相體驗至關重要的 API。

* **初始化攝影頭:** 應用首先需要存取您的裝置攝影頭。對於桌面裝置，存取的可能是網路攝影頭，而對於移動裝置，我們選擇了存取前置攝影頭。我們還提供了 1080p 的預期解析度，以根據使用者裝置型別充分提高拍攝品質。
* **拍照:** 我們使用了內建的 [HtmlElementView](https://api.flutter.dev/flutter/widgets/HtmlElementView-class.html)，該控制項使用平臺視圖將原生 Web 元素渲染為 Flutter widget。在此專案中，我們將 [VideoElement](https://api.flutter.dev/flutter/dart-html/VideoElement-class.html) 渲染為原生 HTML 元素，這便是您在拍照前會在螢幕上看到的內容。我們還使用了一個 [CanvasElement](https://api.flutter.dev/flutter/dart-html/CanvasElement-class.html)，用於在您點選拍照按鈕時從媒體流中捕獲圖像。


```dart
Future<CameraImage> takePicture() async {
 final videoWidth = videoElement.videoWidth;
 final videoHeight = videoElement.videoHeight;
 final canvas = html.CanvasElement(
   width: videoWidth,
   height: videoHeight,
 );
 canvas.context2D
   ..translate(videoWidth, 0)
   ..scale(-1, 1)
   ..drawImageScaled(videoElement, 0, 0, videoWidth, videoHeight);
 final blob = await canvas.toBlob();
 return CameraImage(
   data: html.Url.createObjectUrl(blob),
   width: videoWidth,
   height: videoHeight,
 );
}
```

**攝影頭許可權**

在 Web 上完成 Flutter 攝影頭外掛後，我們建立了一個抽象佈局，以根據相機許可權顯示不同的介面。例如，在等待您允許或拒絕使用瀏覽器攝影頭時，或者如果沒有可供存取的攝影頭時，我們可以顯示一條說明性訊息。

```dart
Camera(
 controller: _controller,
 placeholder: (_) => const SizedBox(),
 preview: (context, preview) => PhotoboothPreview(
   preview: preview,
   onSnapPressed: _onSnapPressed,
 ),
 error: (context, error) => PhotoboothError(error: error),
)
```

在上面的抽象佈局中，placeholder 會在應用等待您授予攝影頭許可權時返回初始介面。Preview 則會在您授予許可權後返回真實的介面，並顯示攝影頭的即時影片流。結尾的 Error 構造陳述式則可以在錯誤發生時捕獲錯誤並顯示相應的訊息。

**產生鏡像照片**

我們的下一個挑戰是產生鏡像照片。如果我們照原樣使用攝影頭拍攝的照片，那麼您看到的內容將與您在照鏡子時所看到的內容不一樣。[某些裝置會提供專門處理這一問題的設定選項](https://9to5mac.com/2020/07/09/iphone-mirror-selfie-photos/)，所以，如果您用前置攝影頭拍照，您看到的其實是照片的鏡像版本。

在我們的第一種方法中，我們嘗試捕捉預設的攝影頭檢視，然後圍繞 y 軸對其進行 180 度翻轉。這種方法似乎有效，但後來我們遇到了 [一個問題](https://github.com/flutter/flutter/issues/79519)，即 Flutter 偶爾會覆蓋這個翻轉，導致影片恢復到未鏡像的版本。

在 Flutter 團隊的幫助下，我們將 VideoElement 放在 [DivElement](https://api.flutter.dev/flutter/dart-html/DivElement-class.html) 中，並更新 VideoElement 以填充 DivElement 的寬度和高度，解決了這個問題。這樣一來，我們能夠為影片元素應用鏡像，同時因為父元素是 div，所以不會被 Flutter 覆蓋翻轉效果。如此一來，我們便獲得了所需的鏡像攝影頭檢視！

![△ 未鏡像的檢視](https://devrel.andfun.cn/devrel/posts/2021/06/rPthpJ.png)

△ 未鏡像的檢視

![△ 鏡像檢視](https://devrel.andfun.cn/devrel/posts/2021/06/CRBd2x.png)

△ 鏡像檢視

**保持寬高比**

在大螢幕上保持 4:3 寬高比，以及在小螢幕上保持 3:4 寬高比，這個操作起來比看起來更難！保持寬高比非常重要，既要符合 Web 應用的整體設計，又要確保在社交媒體上分享照片時，令其中的畫素呈現出清晰的本色效果。這是一項具有挑戰性的任務，因為不同裝置上內建攝影頭的寬高比差異很大。

為了強制保持寬高比，應用首先使用 JavaScript [getUserMedia API](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia) 從裝置攝影頭請求可能的最大解析度。隨後，我們將此 API 傳遞到 VideoElement 流中，這便是您在攝影頭檢視中看到的內容 (當然是已鏡像的版本)。我們還應用了 [object-fit](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit) CSS 屬性來確保影片元素能蓋住其父級容器。我們使用 Flutter 自帶的 AspectRatio widget 來設定寬高比。因此，攝影頭不會對顯示的寬高比做出任何假設；它始終返回支援的最大解析度，然後遵守 Flutter 提供的約束條件 (在本例中為 4:3 或 3:4)。

```dart
final orientation = MediaQuery.of(context).orientation;
final aspectRatio = orientation == Orientation.portrait
   ? PhotoboothAspectRatio.portrait
   : PhotoboothAspectRatio.landscape;
return Scaffold(
 body: _PhotoboothBackground(
   aspectRatio: aspectRatio,
   child: Camera(
     controller: _controller,
     placeholder: (_) => const SizedBox(),
     preview: (context, preview) => PhotoboothPreview(
       preview: preview,
       onSnapPressed: () => _onSnapPressed(
         aspectRatio: aspectRatio,
       ),
     ),
     error: (context, error) => PhotoboothError(error: error),
   ),
 ),
);
```

**透過拖放新增貼紙**

Flutter 照相亭的一大重要體驗在於與您最喜歡的 Google 吉祥物合影並新增道具。您能夠在照片中拖放吉祥物和道具，以及調整大小和旋轉，直到獲得您喜歡的圖像。您也會發現，在將吉祥物新增到螢幕上時，您可以拖動它們並調整其大小。吉祥物們還是有動畫效果的——這種效果由 sprite sheet 來實現。

```dart
for (final character in state.characters)
 DraggableResizable(   
   canTransform: character.id == state.selectedAssetId,
   onUpdate: (update) {
     context.read<PhotoboothBloc>().add(
       PhotoCharacterDragged(
         character: character, 
         update: update,
       ),
     );
   },
   child: _AnimatedCharacter(name: character.asset.name),
 ),
```

為調整物件的大小，我們建立了可拖動、可調整大小且可以容納其他 Flutter widget 的 widget，在本例中，即為吉祥物和道具。該 widget 會使用 [LayoutBuilder](https://api.flutter.dev/flutter/widgets/LayoutBuilder-class.html)，根據視窗的約束條件來處理 widget 的縮放。在內部，我們使用 [GestureDetector](https://api.flutter.dev/flutter/widgets/GestureDetector-class.html) 以掛接到 onScaleStart、onScaleUpdate 和 onScaleEnd 事件。這些回呼(Callback)提供了必要的手勢詳細資訊，以反映使用者對吉祥物和道具的操作。

透過多個 GestureDetector 回饋的資料，[Transform](https://api.flutter.dev/flutter/widgets/Transform-class.html) widget 和 4D 矩陣變換即可根據使用者所做的各種手勢處理縮放，以及旋轉吉祥物和道具。

```dart
Transform(
 alignment: Alignment.center,
 transform: Matrix4.identity()
   ..scale(scale)
   ..rotateZ(angle),
 child: _DraggablePoint(...),
)
```

最後，我們建立了單獨的 package 來確定您的裝置是否支援觸控輸入。可拖動、可調整大小的 widget 會根據觸控功能做出相應的調整。在具有觸控輸入功能的裝置上，您並不能看到調整大小的錨點和旋轉圖示，因為您可以透過雙指張合和平移手勢來直接操縱圖像；而在不支援觸控輸入的裝置 (例如您的桌面裝置) 上，我們則添加了錨點和旋轉圖示，以適應單擊和拖動操作。

![](https://devrel.andfun.cn/devrel/posts/2021/06/XfyErj.png)

## **針對 Web 最佳化 Flutter**

**使用 Flutter 針對 Web 進行開發**

這是我們使用 Flutter 建構的首批純 Web 專案之一，其與移動應用具有不同的特徵。

我們需要確保該應用對任何裝置上的任何瀏覽器都具有 [響應性和自適應性](https://flutter.tw/development/ui/layout/adaptive-responsive)。也就是說，我們必須確保 Flutter 照相亭可以根據瀏覽器大小進行縮放，並且能夠處理移動裝置和 Web 端的輸入。我們透過以下幾種方式做到了這一點:

* **響應式調整大小:** 使用者能夠隨意調整瀏覽器的大小，並且介面能做出響應。如果您的瀏覽器視窗為縱向，則相機會從 4:3 的橫向檢視翻轉為 3:4 的縱向檢視。
* **響應式設計:** 針對桌面瀏覽器，我們設計為在右側顯示 Dash、Android Jetpack、Dino 和 Sparky，而對於移動裝置，這些要素則會顯示在頂部。我們針對桌面裝置，在攝影頭右側設計使用了抽屜式導航欄，而對於移動裝置，則使用了 BottomSheet 類別。
* **自適應輸入:** 如果您使用桌面裝置存取 Flutter 照相亭，則滑鼠點選操作將被視為輸入，如果您使用的是平板電腦或手機，則使用觸控輸入。在調整貼紙大小並將其放置在照片中時，這一點尤其重要。移動裝置支援雙指張合和平移手勢，桌面裝置支援點選和拖動操作。

**可擴充架構**

我們還為此應用建構了可擴充的移動應用。我們的 Flutter 照相亭在建立之初就具有穩固的基礎，包括良好的空安全性、國際化，以及從第一次提交開始就做到的 100% 單元和 widget 測試覆蓋率。我們使用 [flutter_bloc](https://pub.dev/packages/flutter_bloc) 進行狀態管理，因為它支援我們輕鬆測試業務邏輯，並觀察應用中的所有狀態變化。這對於產生開發者日誌和確保可追溯性特別有用，因為我們可以準確地觀察到從一個狀態到另一個狀態的變化，並更快地隔離問題。

我們還實現了由功能驅動的單一程式碼庫結構。例如，貼紙、分享和即時相機預覽，均在各自的資料夾中得到實現，其中每個資料夾包含其各自的介面元件和業務邏輯。這些功能也會用到外部依賴，例如位於 package 子目錄中的相機外掛。利用這種架構，我們的團隊能夠在互不干擾的情況下並行處理多個功能，最大限度地減少合併衝突，並有效地重用程式碼。例如，介面元件函式庫是名為 [photobooth_ui](https://github.com/flutter/photobooth/tree/main/packages/photobooth_ui) 的單獨 package，相機外掛也是單獨的。

透過將元件分成獨立的 package，我們可以提取未與此特定專案繫結的各個元件，並將其開源。與 [Material](https://flutter.tw/development/ui/widgets/material) 和 [Cupertino](https://flutter.tw/development/ui/widgets/cupertino) 元件庫類似，我們甚至可以將介面元件庫 package 做開源處理，以供 Flutter 社群使用。

## **Firebase + Flutter = 完美組合**

**Firebase Auth、儲存、託管等**

照相亭利用 Firebase 生態系統進行各種後端整合。[firebase_auth](https://pub.dev/packages/firebase_auth) package 支援使用者在應用啟動後立即匿名登入。每個會話都使用 Firebase Auth 建立具有唯一 ID 的匿名使用者。

當您來到共享頁面時，此設定即會開始發揮作用。您可以下載照片以儲存為個人頭像，也可以直接將其分享到社交媒體。如果您下載照片，則該照片將儲存在您的本地裝置上。如果您分享照片，我們會使用 [firebase_storage](https://pub.dev/packages/firebase_storage) package 將照片儲存在 Firebase 中，以便稍後檢索並產生帖子透過社交媒體釋出。

我們在 Firebase 的儲存分割槽上定義了 [Firebase 安全規則](https://firebase.google.cn/docs/rules)，確保照片在建立後不可變。這可以防止其他使用者修改或刪除儲存分割槽中的照片。此外，我們使用 Google Cloud 提供的 [物件生命週期管理](https://cloud.google.com/storage/docs/lifecycle)，定義了一個刪除 30 天前所有物件的規則，但您可以按照應用中列出的說明請求儘快刪除您的照片。

此應用還使用 [Firebase Hosting](https://firebase.google.cn/docs/hosting) 快速安全地進行託管。我們可以藉助 [action-hosting-deploy](https://github.com/FirebaseExtended/action-hosting-deploy) GitHub Action，根據目標分支，將應用自動部署到 Firebase Hosting。當我們將變更合併到主分支時，該操作會觸發一個工作流，用於建構應用的特定開發版本，並將其部署到 Firebase Hosting。同樣，當我們將變更合併到釋出分支時，該操作也會觸發部署生產版本。透過結合使用 GitHub Action 與 Firebase Hosting，我們的團隊能夠快速迭代，並始終得到最新版本的預覽。

最後，我們使用 [Firebase 效能監測](https://firebase.google.cn/products/performance) 來監控主要的 Web 效能指標。

**使用 Cloud Functions 進行社交**

在產生您的社交帖子之前，我們首先會確保照片內容是畫素級完美的。最終圖像包含漂亮的邊框，以呈現 Flutter 照相亭特色，並按 4:3 或 3:4 的寬高比進行裁剪，以便在社交帖子上呈現出色的效果。

我們使用 [OffscreenCanvas](https://developer.mozilla.org/en-US/docs/Web/API/OffscreenCanvas) API 或 [CanvasElement](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas) 來合成原始照片、吉祥物和道具的圖層，並產生您可以下載的單個圖像。這個處理步驟由 [image_compositor](https://github.com/flutter/photobooth/tree/main/packages/image_compositor) package 負責執行。

然後，我們利用 Firebase 強大的 [Cloud Functions](https://firebase.google.cn/docs/functions)，來將照片分享到社交媒體。當您點選分享按鈕時，系統會帶您前往新標籤頁，並在所選的社交平臺上自動產生待發布的帖子。該帖子還包含一個連結，連線到我們編寫的 Cloud Functions。瀏覽器在分析網址時，會檢測 Cloud Functions 產生的動態元資料，並據此在您的社交帖子中顯示照片的精美預覽，以及一個指向分享頁面的連結，您的粉絲們可以在該頁面上檢視照片，並導航回 Flutter 照相亭應用，以獲取他們自己的照片。

```dart
function renderSharePage(imageFileName: string, baseUrl: string): string {
 const context = Object.assign({}, BaseHTMLContext, {
   appUrl: baseUrl,
   shareUrl: `${baseUrl}/share/${imageFileName}`,
   shareImageUrl: bucketPathForFile(`${UPLOAD_PATH}/${imageFileName}`),
 });
 return renderTemplate(shareTmpl, context);
}
```

成品如下所示:

![](https://devrel.andfun.cn/devrel/posts/2021/06/6TLvkS.png)

有關如何在 Flutter 專案中使用 Firebase 的更多資訊，請檢視 [此 Codelab](https://firebase.google.cn/codelabs/firebase-get-to-know-flutter#0)。

## **最終成果**

本專案詳細地示範瞭如何針對 Web 來建構應用的方法。令我們感到驚喜的是，與使用 Flutter 建構移動應用的體驗相比，這個 Web 應用的建構工作流與之非常相似。我們必須考慮視窗大小、自適應、觸控與滑鼠輸入、圖像載入時間、瀏覽器相容性等元素，以及在建構 Web 應用時所必需考慮的其他所有因素。但是，我們仍然可以使用相同的模式、架構和編碼標準來編寫 Flutter 程式碼，這讓我們在建構 Web 應用時感到非常自在。Flutter package 提供的工具和不斷髮展的生態系統，包括 Firebase 工具套件，幫助我們實現了 Flutter 照相亭。

![△ 打造 Flutter 照相亭的 Very Good Ventures 團隊](https://devrel.andfun.cn/devrel/posts/2021/11/0219cca9b22ab.png)

△ 打造 Flutter 照相亭的 Very Good Ventures 團隊

我們已經開放了所有原始碼，歡迎大家前往 GitHub 檢視 [photo_booth](https://github.com/flutter/photobooth) 專案，也別忘了多多拍照秀出來哦！

*Flutter 照相亭中文版有部分功能刪減，您可以在 <a href="https://photobooth.flutter.dev">https://photobooth.flutter.dev</a> 體驗完整功能*