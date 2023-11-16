## Performance

## 效能

Platform views in Flutter come with performance trade-offs.

在 Flutter 中使用平臺視圖時，效能會有所取捨。

For example, in a typical Flutter app, the Flutter UI is composed
on a dedicated raster thread. This allows Flutter apps to be fast,
as the main platform thread is rarely blocked.

例如，在典型的 Flutter 應用中，Flutter 的 UI 是專門在 raster 執行緒上合成的。
由於平台的主執行緒很少被阻塞，因此 Flutter 應用程式可以快速執行。

While a platform view is rendered with hybrid composition,
the Flutter UI is composed from the platform thread,
which competes with other tasks like handling OS or plugin messages.

使用混合整合模式渲染平臺視圖時，
Flutter UI 由平台執行緒完成，與其他執行緒一起競爭，
例如：處理系統或外掛訊息等任務。

Prior to Android 10, hybrid composition copied each Flutter frame
out of the graphic memory into main memory, and then copied it back
to a GPU texture. As this copy happens per frame, the performance of
the entire Flutter UI might be impacted. In Android 10 or above, the
graphics memory is copied only once.

在 Android 10 之前，
混合整合模式將每個 Flutter 幀從視訊記憶體中複製到主記憶體中，
然後再將其複製回 GPU 紋理中。
在 Android 10 或更高版本中，視訊記憶體會被複制兩次。
由於每幀都會進行一次複製，因此可能會影響整個 Flutter UI 的效能。

Virtual display, on the other hand,
makes each pixel of the native view
flow through additional intermediate graphic buffers,
which cost graphic memory and drawing performance.

另一方面，虛擬顯示模式使平臺視圖的每個畫素
流經附加的中間圖形緩衝區，這會浪費視訊記憶體和繪圖效能。

For complex cases, there are some techniques that
can be used to mitigate these issues.

對於複雜的情況，可以使用一些技巧來緩解這些問題。

For example, you could use a placeholder texture
while an animation is happening in Dart.
In other words, if an animation is slow while a
platform view is rendered,
then consider taking a screenshot of the
native view and rendering it as a texture.

例如，當 Dart 中發生動畫時，您可以使用佔位符紋理。
換句話說，如果在渲染平臺視圖時動畫很慢，
請考慮對原生檢視進行截圖，並將其渲染為紋理。

For more information, see:

更多資訊，請檢視下面連結：

* [`TextureLayer`][]
* [`TextureRegistry`][]
* [`FlutterTextureRegistry`][]
* [`FlutterImageView`][]

[`FlutterImageView`]: {{site.api}}/javadoc/io/flutter/embedding/android/FlutterImageView.html
[`FlutterTextureRegistry`]: {{site.api}}/objcdoc/Protocols/FlutterTextureRegistry.html
[`TextureLayer`]: {{site.api}}/flutter/rendering/TextureLayer-class.html
[`TextureRegistry`]: {{site.api}}/javadoc/io/flutter/view/TextureRegistry.html
