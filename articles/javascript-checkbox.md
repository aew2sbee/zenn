---
title: "[JavaScript] チェックボックスによるボタンの活性/非活性の切り替え" # 記事のタイトル
emoji: "🙆‍♀️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["html", "css", "javascript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、Web サイトの「利用規約への同意」などで使われる、チェックボックスでボタンの活性/非活性を切り替える方法を、JavaScript（jQuery）で実装する手順としてまとめます。

## 🌱 1. HTML ファイルの編集

```html
<input type="checkbox" id="agreement-check" />
<label for="agreement-check"><a href="#" target="_blank" rel="noopener noreferrer">利用規約</a>に同意する</label>
<button type="submit" value="登録" class="submit_btn" disabled>登録</button>

<!-- jQuery を読み込み、その後に自作の JavaScript ファイルを読み込む -->
<script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>
<script src="main.js"></script>
```

:::message
手順 3 のコードは jQuery を使っています。jQuery を読み込まないと `$ is not defined` というエラーになり、動作しません。
:::

## 🌱 2. CSS ファイルの編集

```css
.submit_btn {
  cursor: pointer;
  background-color: #344767;
  border-radius: 5px;
  border: none;
  width: 100px;
  height: 40px;
  margin-top: 16px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-weight: 500;
  font-size: 16px;
}
.submit_btn:disabled {
  pointer-events: none;
  opacity: 0.5;
}
```

## 🌱 3. JavaScript ファイルの編集

```js
$(function () {
  // チェック状態が変わったときに実行する
  $("#agreement-check").on("change", function () {
    // チェックが入っていなければ disabled を付け、入っていれば外す
    $(".submit_btn").prop("disabled", !$(this).prop("checked"));
  });
});
```

:::message alert
`disabled` は画面上の補助です。ブラウザの開発者ツールで簡単に外せるため、同意の有無は必ずサーバー側でも確認してください。
:::

## 🌱 おわり

チェックの有無に応じて button 要素に disabled 属性を付与・削除し、CSS でスタイルを切り替えることで実装しました。
