# 翻譯指南

## 參見樣本

`src/docs/resources/faq.md`

## 翻譯語法

### 頁面元資料

直接把中文放在原文下方即可：

```
title: Flutter - Beautiful native apps in record time
title: Flutter - 絕妙原生應用
```

### markdown 標題、段落等

直接把中文放在原文下方即可：

```
## english

## 中文

english

中文
```

### markdown 列表（`-`、`*`、`1. ` 等）

中文要換行並且縮排要與文字部分對齊

```
- one

  一
  
- two

  二
  
- three

  三

```

注意：不要省略中間和末尾的空行。

### markdown 表格

在下一行書寫中文（可任意新增空格進行對齊），但是表頭的翻譯要放在表頭分割線的下方，如：

```
one | two | three
----|----|----
一 | 二 | 三
four | five | six
四    | 五   | 六

```

## HTML 內容 - 容器型塊元素

對於內嵌在 md 中的 html 內容，需要特殊處理

### `h\d|p|header`

可以直接在下方放中文，如：

```
<p>
english
</p>

<p>
中文
</p>

```

內容不限

### `span|a`

可以直接在緊後方放中文，如：

```
<span>english</span><span>中文</span>
```
中間換行也可以。但儘量不要用這種方式，而是優先使用整段翻譯的方式。

## 釋出

翻譯完內容之後：

1. 先按照 <https://github.com/flutter/website> 中的指示建立起原文編譯環境
1. （有許可權的帳號，且僅限於 CI 環境）執行 `tool/translator/deploy-cn.sh` 將其推送到 github pages 進行釋出

## 額外注意

因為構架指令碼使用了 Bundle ，需要確保在在本機上安裝了 Ruby 與 Bundle，推薦使用 [RVM](https://ruby-china.org/wiki/rvm-guide) 新建一個虛擬環境做隔離

## 同步更新

用 WebStorm/IntelliJ 的 git 合併功能合併遠端更新並處理合並衝突即可。
