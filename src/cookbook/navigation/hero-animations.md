---
title: Animate a widget across screens
title: 跨頁面切換的動效 Widget (Hero animations)
description: How to animate a widget from one screen to another
description: 如何讓一個 widget 跨頁面進行動畫。
tags: cookbook, 實用課程, 導航
keywords: 頁面切換,動畫效果
js:
  - defer: true
    url: https://dartpad.cn/inject_embed.dart.js
---

<?code-excerpt path-base="cookbook/navigation/hero_animations"?>

It's often helpful to guide users through an app as they navigate from screen
to screen. A common technique to lead users through an app is to animate a
widget from one screen to the next. This creates a visual anchor connecting
the two screens.

在頁面跳轉過程中給使用者加以引導是非常有用的。
實現引導的一種通用做法是在頁面切換時為某個元件加上轉場動畫，
從而在兩個頁面間建立視覺上的錨定關聯。

Use the [`Hero`][] widget
to animate a widget from one screen to the next.

在 Flutter 中，可以透過 [`Hero`][] widget 實現頁面切換時元件的轉場動畫。

This recipe uses the following steps:

這個課程將包含以下步驟：

  1. Create two screens showing the same image.

     建立兩個頁面，展示相同的圖片

  2. Add a `Hero` widget to the first screen.

     在第一個頁面中加入 `Hero` 元件

  3. Add a `Hero` widget to the second screen.

     在第二個頁面中加入 `Hero` 元件

## 1. Create two screens showing the same image

## 1. 建立兩個頁面，展示相同的圖片

In this example, display the same image on both screens.
Animate the image from the first screen to the second screen when
the user taps the image. For now, create the visual structure;
handle animations in the next steps.

在這個範例中，將在兩個頁面中展示相同的圖片。
當用戶在第一個頁面點選圖片，會透過一個轉場動畫切換到第二個頁面。
現在，我們將會建立頁面的視覺結構，並在後續步驟中處理動畫。

{{site.alert.note}}

  This example builds upon the
  [Navigate to a new screen and back][]
  and [Handle taps][] recipes.
  
  這個範例建立在 [導航到一個新頁面和返回][Navigate to a new screen and back] 和 
  [處理點選事件][Handle taps] 這兩個章節的基礎上。
  
{{site.alert.end}}

<?code-excerpt "lib/main_original.dart"?>
```dart
import 'package:flutter/material.dart';

class MainScreen extends StatelessWidget {
  const MainScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Main Screen'),
      ),
      body: GestureDetector(
        onTap: () {
          Navigator.push(context, MaterialPageRoute(builder: (context) {
            return const DetailScreen();
          }));
        },
        child: Image.network(
          'https://picsum.photos/250?image=9',
        ),
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  const DetailScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        onTap: () {
          Navigator.pop(context);
        },
        child: Center(
          child: Image.network(
            'https://picsum.photos/250?image=9',
          ),
        ),
      ),
    );
  }
}
```

## 2. Add a `Hero` widget to the first screen

## 2. 在第一個頁面中加入 `Hero` 元件

To connect the two screens together with an animation, wrap
the `Image` widget on both screens in a `Hero` widget.
The `Hero` widget requires two arguments:

為了透過動畫在兩個頁面間建立聯絡，
需要把每個頁面的 `Image` 元件都包裹進 `Hero` 元件裡面。
`Hero` 元件有兩個引數：

<dl>
  <dt>`tag`</dt>
  
  <dd><p>An object that identifies the `Hero`. It must be the same on both screens.</p><p>作為 `Hero` 元件的標識，在這兩個頁面中必須相同。</p></dd>

  <dt>`child`</dt>
  
  <dd><p>The widget to animate across screens.</p><p>在兩個螢幕直接跨越的那個 widget。</p></dd>
  
</dl>

{% comment %}
RegEx removes the first "child" property name and removed the trailing comma at the end
{% endcomment %}
<?code-excerpt "lib/main.dart (Hero1)" replace="/^child: //g;/,$//g"?>
```dart
Hero(
  tag: 'imageHero',
  child: Image.network(
    'https://picsum.photos/250?image=9',
  ),
)
```

## 3. Add a `Hero` widget to the second screen

## 3. 在第二個頁面中加入 `Hero` 元件

To complete the connection with the first screen,
wrap the `Image` on the second screen with a `Hero`
widget that has the same `tag` as the `Hero` in the first screen.

為了完善與第一個頁面的關聯，
同樣需要把第二個頁面中的 `Image` 元件包裹進 `Hero` 元件裡面。
它的 `tag` 也必須和第一個頁面相同。

After applying the `Hero` widget to the second screen,
the animation between screens just works.

當 `Hero` 元件被應用到第二個頁面後，頁面的轉場動畫就生效了。

{% comment %}
RegEx removes the first "child" property name and removed the trailing comma at the end
{% endcomment %}
<?code-excerpt "lib/main.dart (Hero2)" replace="/^child: //g;/,$//g"?>
```dart
Hero(
  tag: 'imageHero',
  child: Image.network(
    'https://picsum.photos/250?image=9',
  ),
)
```


{{site.alert.note}}

  This code is identical to what you have on the first screen.
  As a best practice, create a reusable widget instead of
  repeating code. This example uses identical code for both
  widgets, for simplicity.

  這份程式碼和第一個頁面中的程式碼是相同的。
  實際上，可以建立一個可複用的元件來代替這些重複的程式碼。
  但是在這個範例中，重複的程式碼會更易於講解和示範。
  
{{site.alert.end}}

## Interactive example

## 互動式範例

<?code-excerpt "lib/main.dart"?>
```run-dartpad:theme-light:mode-flutter:run-true:width-100%:height-600px:split-60:ga_id-interactive_example
import 'package:flutter/material.dart';

void main() => runApp(const HeroApp());

class HeroApp extends StatelessWidget {
  const HeroApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Transition Demo',
      home: MainScreen(),
    );
  }
}

class MainScreen extends StatelessWidget {
  const MainScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Main Screen'),
      ),
      body: GestureDetector(
        onTap: () {
          Navigator.push(context, MaterialPageRoute(builder: (context) {
            return const DetailScreen();
          }));
        },
        child: Hero(
          tag: 'imageHero',
          child: Image.network(
            'https://picsum.photos/250?image=9',
          ),
        ),
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  const DetailScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        onTap: () {
          Navigator.pop(context);
        },
        child: Center(
          child: Hero(
            tag: 'imageHero',
            child: Image.network(
              'https://picsum.photos/250?image=9',
            ),
          ),
        ),
      ),
    );
  }
}
```

<noscript>
  <img src="/assets/images/docs/cookbook/hero.gif" alt="Hero demo" class="site-mobile-screenshot" />
</noscript>


[Handle taps]: {{site.url}}/cookbook/gestures/handling-taps
[`Hero`]: {{site.api}}/flutter/widgets/Hero-class.html
[Navigate to a new screen and back]: {{site.url}}/cookbook/navigation/navigation-basics
