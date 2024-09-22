---
title: "[JavaScript]チェックボックスによるボタンの活性/非活性の切り替え" # 記事のタイトル
emoji: "🙆‍♀️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["html", "css", "javascript", '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに
この記事では、Webサイトの「利用規約への同意」等で使用される、チェックボックスによるボタンの活性/非活性の切り替えのJavaScriptでの実装方法をまとめていきます。

## 1. HTMLファイルの編集
```html
<input type="checkbox" id="agreement-check">
<label for="agreement-check"><a href="#" target="_brank">利用規約</a>に同意する</label>
<button type="submit" value="登録" class="submit_btn" disabled=”disabled”>登録</button>
```

## 2. CSSファイルの編集
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
    opacity: .5;
}
```
## 3. JavaScriptファイルの編集

```js
$(function () {
  $("#agreement-check").click(function () {
    if ($(this).prop("checked") == false) {
      //チェックが入っていなかったら
      $(".submit_btn").attr("disabled", "disabled"); //disabledをつける
    } else {
      $(".submit_btn").removeAttr("disabled"); //disabledを外す
    }
  });
});
```

## おわり
チェックボックスのチェックの有無を判別し、ボタンタグにdisabledをつけ外しし、CSSでスタイルを変更することによって実装しました。
