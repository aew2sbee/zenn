---
title: "[JavaScript] 一文字ずつ時間差で表示するアニメーション" # 記事のタイトル
emoji: "🍧" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["html", "css", "javascript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、Web ページ上でテキストを一文字ずつ表示するアニメーションの実装方法をまとめます。

## 🌱 結論

JavaScript で指定した要素の文字を一文字ずつ span で囲み、**opacity の値を時間差で 0 から 1 に変更すること**で、一文字ずつ表示するアニメーションを実装しました。

## 🌱 やり方

### 1. HTML ファイルの編集

```html
<div>
  <p class="js-text">Welcome to TECHLOG.</p>
</div>
```

### 2. CSS ファイルの編集

後述の JavaScript で一文字ずつ span タグで囲うので、span の透明度を 0 にして見えないようにしておきます。

```css
.js-text span {
  opacity: 0;
}
```

### 3. JavaScript ファイルの編集

```js
document.addEventListener("DOMContentLoaded", function () {
  // js-textクラスを持つ要素をすべて取得する
  const elements = document.getElementsByClassName("js-text");
  // 取得した要素の数だけ、animateText関数を実行する
  for (let i = 0; i < elements.length; i++) {
    animateText(elements[i]);
  }
});

function animateText(element) {
  // 要素の文字列を取り出し、要素の中身を空にする
  const text = element.textContent;
  element.textContent = "";

  // 一文字ずつspanタグで囲う（for...of を使うと絵文字なども1文字として扱える）
  for (const char of text) {
    const span = document.createElement("span");
    span.textContent = char;
    element.appendChild(span);
  }

  // spanで囲った文字を取得する
  const spans = element.getElementsByTagName("span");
  // 1文字ごとに200ミリ秒ずつ遅らせて、opacityを1にする
  for (let j = 0; j < spans.length; j++) {
    setTimeout(function () {
      spans[j].style.opacity = 1;
    }, j * 200); // 遅延時間を調整
  }
}
```

:::message
- 要素の中身を `textContent` で置き換えるため、`<a>` や `<strong>` などの子要素は消えます。文字だけの要素に使ってください。
- CSS に `transition` を指定していないため、各文字はフェードせずにパッと表示されます。ふわっと表示したい場合は `.js-text span { opacity: 0; transition: opacity 0.3s; }` のように指定します。
:::
