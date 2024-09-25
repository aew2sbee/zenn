---
title: "[JavaScript] 一文字ずつ時間差で表示するアニメーション" # 記事のタイトル
emoji: "🍧" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["html", "css", "javascript", '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに
この記事では、`Webページ上でテキストを一文字ずつ表示するアニメーション`の実装方法をまとめます。

## 結論
`JavaScript`によって指定した要素を一文字ずつspanで囲い、**styleを変更(opacityの値を0→1に)すること**で、一文字ずつ表示するアニメーションを実装しました。

この記事では、Webページ上でテキストを一文字ずつ表示するアニメーションの実装方法をまとめます。

## やり方
### 1. HTMLファイルの編集
```html
<div>
    <p class="js-text">Welcome to TECHLOG.</p>
</div>
```

### 2. CSSファイルの編集
次のJavaScriptで一文字ずつspanタグで囲うので、spanの透明度を0にして見えないようにしておきます。
```css
.js-text span {
    opacity: 0;
}
```
### 3. JavaScriptファイルの編集

```js
document.addEventListener("DOMContentLoaded", function (event) {
    // js-textというクラスを取得し、elementsという変数を宣言
    let elements = document.getElementsByClassName("js-text");
    // animateTextという関数を、elementsの文字数の回数繰り返す
    for (var i = 0; i < elements.length; i++) {
        animateText(elements[i]);
    }
});

function animateText(element) {
    // elementの文字情報のみをtextという変数に代入
    let text = element.innerText;
    element.innerText = "";

    // textの一文字ずつをspanタグで囲う
    for (var i = 0; i < text.length; i++) {
        var span = document.createElement("span");
        span.innerText = text[i];
        element.appendChild(span);
    }

    // spanで囲った文字をspansに代入
    let spans = element.getElementsByTagName("span");
    // spansのstyleをopacity=1にする処理を、文字数分繰り返す
    for (var j = 0; j < spans.length; j++) {
        (function (index) {
            setTimeout(function () {
                spans[index].style.opacity = 1;
          }, index * 200); // 遅延時間を調整
        })(j);
    }
}
```
