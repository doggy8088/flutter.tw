---
title: 使用 Flutter 與 Firebase 製作彈球遊戲
toc: true
keywords: 彈球遊戲, pinball, Firebase, Flutter 遊戲開發
description: 讓 Flutter 遊戲開發能力更上一層樓。
image:
    path: https://files.flutter-io.cn/events/gdd2022/pinball/pinball_share_image.png
---

*文/ Very Good Ventures 團隊，5 月 11 日發表於 Flutter 官方部落格*

![](https://files.flutter-io.cn/events/gdd2022/pinball/pinball_share_image.png)

Flutter 團隊使用 Flutter 以及 Firebase 建構了一款經典的彈球遊戲。
下面將會介紹我們是如何透過 Flame 遊戲引擎將 [Flutter 彈球遊戲](https://pinball.flutter.cn/ "Flutter 彈球遊戲主頁") 帶到 Web 端的。

## 遊戲開發要點

使用 Flutter 打造使用者互動型別的遊戲是一個很棒的選擇，例如拼圖或者文字遊戲這樣的遊戲。
[Flame](https://docs.flame-engine.org/ "Flame 開發文件主頁") 是一個在 Flutter 上建構的 2D 遊戲引擎，
當涉及到使用遊戲迴圈的遊戲時它會非常有用。
Flutter 彈球遊戲使用了 Flame 提供的一系列特性，例如動畫、物理引擎、碰撞檢測等等，
同時還藉助了 Flutter 框架的基礎架構。
如果你能用 Flutter 建構應用，你就獲得 Flame 建構遊戲所需的基礎。

![](https://devrel.andfun.cn/devrel/posts/2022/05/8Qcwcy.jpg)

## 遊戲迴圈

通常來說應用螢幕在沒有使用者互動事件的時候都會保持視覺靜止狀態。
遊戲中則是相反的—— UI 會持續的渲染，而且遊戲狀態會不斷變化。
Flame 提供了一個 game widget，它內部管理了一個遊戲迴圈，
所以能恆定且高效地進行渲染。`Game` 類包含了遊戲元件以及其邏輯的實現，
然後被交給 widget 樹中的 `GameWidget`。
在 Flutter 彈球遊戲中，遊戲迴圈反映了彈球在遊戲場的位置以及狀態，
然後如果球與物體碰撞或跌出比賽則需要給出必要的反饋。


```dart
@override
void update(double dt) {
  super.update(dt);
  final direction = -parent.body.linearVelocity.normalized();
  angle = math.atan2(direction.x, -direction.y);
  size = (_textureSize / 45) * 
    parent.body.fixtures.first.shape.radius;
}
```

## 使用 2D 元件渲染 3D 空間

在做 Flutter 彈球遊戲的時候，其中遇到的一個挑戰即是如何使用 2D 元素渲染一個 3D 的互動體驗。
元件需要知道在螢幕上渲染的前後順序。
例如，當小球發射到斜坡上時，它的順序會向前，這樣就會讓它看起來出現在斜坡的頂部。

![](https://devrel.andfun.cn/devrel/posts/2022/05/Ko46wg.jpg)

彈球、彈射活塞、擋板以及 Chrome 小恐龍等等這些元素都是可活動的，這意味著它應該遵循真實世界的物理規則。
而且彈球也需要根據它在板子上的位置改變其大小。當彈球滾到頂部時，
它應該越來越小，以讓它看著離使用者更遠。此外，重力還會讓彈球調整角度，讓它能在斜坡上更快地落下。

```dart
/// Scales the ball's body and sprite according to its position on the board.
class BallScalingBehavior extends Component with ParentIsA<Ball> {
  @override
  void update(double dt) {
    super.update(dt);
    final boardHeight = BoardDimensions.bounds.height;
    const maxShrinkValue = BoardDimensions.perspectiveShrinkFactor;
    final standardizedYPosition = parent.body.position.y +   (boardHeight / 2);
    final scaleFactor = maxShrinkValue +
        ((standardizedYPosition / boardHeight) * (1 - maxShrinkValue));
parent.body.fixtures.first.shape.radius = (Ball.size.x / 2) * scaleFactor;
final ballSprite = parent.descendants().whereType<SpriteComponent>();
    if (ballSprite.isNotEmpty) {
      ballSprite.single.scale.setValues(
        scaleFactor,
        scaleFactor,
      );
    }
  }
}
```

## Forge 2D 的物理引擎

Flutter 彈球遊戲很大程度依賴了 Flame 團隊維護的
[forge2d](https://pub.flutter-io.cn/packages/forge2d "Flame 團隊維護的 package: forge2d") package。
這個 package 將開源的 [Box2D 物理引擎](https://box2d.org/ "Box2D 物理引擎官網") 移植到 Dart 中，以便可以輕鬆整合到 Flutter。
我們使用 `forge2d` 增強遊戲中的物理特性，例如物體（夾板）在遊戲場上的之間的碰撞檢測。
使用 `forge2D` 能夠我們監聽夾板發生碰撞的時機。
我們就可以在這裡向夾板新增互動的回呼(Callback)，當兩個物體發生碰撞的時候我們就能收到通知。
例如，彈球（它是圓形的）與彈簧（它是橢圓形的）接觸時，我們就會增加它的得分。
在這些回呼(Callback)中，我們可以清楚地設定接觸開始和結束的位置，
以便當兩個物體相互接觸時，會發生碰撞。

```dart
@override
Body createBody() {
  final shape = CircleShape()..radius = size.x / 2;
  final bodyDef = BodyDef(
    position: initialPosition,
    type: BodyType.dynamic,
    userData: this,
  );
  return world.createBody(bodyDef)
    ..createFixtureFromShape(shape, 1);
}
```

## Sprite sheet 動畫

在彈球遊戲場中有一些小東西，
例如 Android、Dash（Dart 吉祥物）、Sparky（Firebase 吉祥物）以及 Chrome 小恐龍，這些都是動畫。
對於這些東西，我們使用了 sprite sheets，它已經包含在 Flame 引擎中了，
叫做 `SpriteAnimationComponent`。
對於每個元素，我們都有一個檔案，其中包含不同方向的圖像、檔案中的幀數以及幀之間的時間。
使用這些資料，Flame 中的 `SpriteAnimationComponent` 能夠在一個迴圈中將所有圖像編在一起，
使元素看起來在運動。

![△ Sprite sheet 範例](https://devrel.andfun.cn/devrel/posts/2022/05/sMkc3K.jpg)

△ Sprite sheet 範例

```dart
final spriteSheet = gameRef.images.fromCache(
  Assets.images.android.spaceship.animatronic.keyName,
);
const amountPerRow = 18;
const amountPerColumn = 4;
final textureSize = Vector2(
  spriteSheet.width / amountPerRow,
  spriteSheet.height / amountPerColumn,
);
size = textureSize / 10;
animation = SpriteAnimation.fromFrameData(
  spriteSheet,
  SpriteAnimationData.sequenced(
    amount: amountPerRow * amountPerColumn,
    amountPerRow: amountPerRow,
    stepTime: 1 / 24,
    textureSize: textureSize,
  ),
);
```

接下來詳細解析 Flutter 彈球遊戲程式碼。

## 來自 Firebase 的即時積分排行榜

Flutter 彈球排行榜即時地顯示世界各地玩家的最高分數。
我們使用 Firebase [Cloud Firestore](https://firebase.google.cn/docs/firestore "Firebase Cloud Firestore 文件") 記錄排名前十的分數，將其顯示在排行榜上。
當一個新的分數計入排行榜時，一個 [Cloud Function](https://firebase.google.cn/docs/functions "Firebase Cloud Function 文件") 會將分數按降序排列並刪除目前不在前十的分數。

![](https://devrel.andfun.cn/devrel/posts/2022/05/PTfsgf.png)

```dart
/// Acquires top 10 [LeaderboardEntryData]s.
Future<List<LeaderboardEntryData>> fetchTop10Leaderboard() async {
  try {
    final querySnapshot = await _firebaseFirestore
      .collection(_leaderboardCollectionName)
      .orderBy(_scoreFieldName, descending: true)
      .limit(_leaderboardLimit)
      .get();
    final documents = querySnapshot.docs;
    return documents.toLeaderboard();
  } on LeaderboardDeserializationException {
    rethrow;
  } on Exception catch (error, stackTrace) {
    throw FetchTop10LeaderboardException(error, stackTrace);
  }
}
```

## 建構 Web 應用

與傳統應用相比，製作響應式的遊戲更容易。彈球遊戲只需要根據裝置的大小進行縮放即可。
對於 Flutter 彈球遊戲，我們基於固定比例的裝置大小進行縮放。
確保了無論在何種顯示大小，座標系統總是相同的。保證元件在不同裝置之間的一致顯示和互動非常重要。

Flutter 彈球遊戲也適配了移動和桌面瀏覽器。
在移動瀏覽器上，使用者可以點選啟動按鈕開始播放，也可以點選螢幕左右兩側來控制相應的擋板。
在桌面瀏覽器上，使用者可以使用鍵盤來發射彈球和控制擋板。

## 程式碼架構

彈球程式碼遵循分層架構，每個功能都在自己的資料夾中。
在這個專案中，遊戲邏輯也與視覺元件分離。讓我們能獨立於遊戲邏輯輕鬆地更新視覺元素。
彈球遊戲的主題取決於玩家在遊戲開始前選擇的角色。主題是由 `CharacterThemeCubit` 類控制的。
根據角色的選擇，球的顏色、背景和其他元素都會更新。

![](https://devrel.andfun.cn/devrel/posts/2022/05/BPnkOM.png)

```dart
/// {@template character_theme}
/// Base class for creating character themes.
///
/// Character specific game components should have a getter specified here to
/// load their corresponding assets for the game.
/// {@endtemplate}
abstract class CharacterTheme extends Equatable {
  /// {@macro character_theme}
  const CharacterTheme();
/// Name of character.
  String get name;
/// Asset for the ball.
  AssetGenImage get ball;
/// Asset for the background.
  AssetGenImage get background;
/// Icon asset.
  AssetGenImage get icon;
/// Icon asset for the leaderboard.
  AssetGenImage get leaderboardIcon;
/// Asset for the the idle character animation.
  AssetGenImage get animation;
@override
  List<Object> get props => [
        name,
        ball,
        background,
        icon,
        leaderboardIcon,
        animation,
      ];
}
```

Flutter 彈球的遊戲狀態是用
[flam_bloc](https://pub.flutter-io.cn/packages/flame_bloc "Flutter package: flam_bloc 頁面") 這個 package 處理的，
這是一個組合了 bloc 和 Flame 元件的 package。
例如，我們使用 `flame_bloc` 來記錄剩餘的遊戲回合數、遊戲中獲得的獎勵以及當前的遊戲分數。
另外，在 wdget 樹最上層有一個 widget，它包含載入頁面的邏輯以及玩遊戲的說明。
我們還遵循 [行為型模式](http://baike.baidu.com/l/i4znnfCN "百度百科: 設計模式行為型模式") 來封裝和隔離基於元件的遊戲功能元素。
例如，保險槓在被球擊中時會發出聲音，所以我們實現了 `BumperNoiseBehavior` 類來處理這個問題。


```dart
class BumperNoiseBehavior extends ContactBehavior {
  @override
  void beginContact(Object other, Contact contact) {
    super.beginContact(other, contact);
    readProvider<PinballPlayer>().play(PinballAudio.bumper);
  }
}
```

程式碼庫還包含全面的單元測試、元件測試和黃金測試。測試遊戲會帶來一些挑戰，
因為一個元件可能具有多個職責，使得它們很難單獨地進行測試。
最終我們定義了更好的隔離和測試元件的模式，
並將其改進整合到
[flame_test](https://pub.flutter-io.cn/packages/flame_test "Flutter package: flame_test 頁面") 這個 package 中。

## 元件沙盒

這個專案高度依賴於 Flame 元件帶來的模擬彈球體驗。
程式碼附帶了一個元件沙盒，它類似於 Flutter Gallery 中展示的
[UI 元件庫](https://gallery.flutter.cn/#/ "Flutter Gallery 網頁版")。
在開發遊戲時，這是一個很有用的工具，
因為它提供了獨立的每個遊戲元件以確保它們的外觀和行為符合預期，然後再將它們整合到遊戲中。

## 接下來

邀請你來 [Flutter Pinball](https://pinball.flutter.cn/ "Flutter Pinball 彈球遊戲") 試玩並獲取高分！
關注積分排行榜並且在社交媒體上分享你的分數，
也可以在 [GitHub repo](https://github.com/flutter/pinball "彈球遊戲原始碼儲存庫") 存取並學習專案的開原始碼。

---
[原文](https://medium.com/flutter/i-o-pinball-powered-by-flutter-and-firebase-d22423f3f5d)，
本地化：CFUG 團隊，[中文連結](https://flutter.cn/posts/i-o-pinball)。