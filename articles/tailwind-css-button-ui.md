---
title: '[Tailwind CSS] ボタンUI' # 記事のタイトル
emoji: '🍃' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['css', 'tailwindcss', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**Tailwind CSSでよくあるボタンデザイン**を作成しました。

下記サイトを活用して表示しております。
@[card](https://play.tailwindcss.com/)

:::details 参考資料
@[card](https://gihyo.jp/book/2024/978-4-297-13943-8)
:::

## 1. 「登録」のボタン
![button01](/images/articles/tailwind-css-cheat-design/button01.png)
:::message

:::
```html
<button class="py-1 px-5 bg-sky-500 rounded-2xl text-white font-black">登録</button>
```



## 2. 「削除」のボタン
![button02](/images/articles/tailwind-css-cheat-design/button02.png)
:::message

:::
```html
<button class="py-1 px-5 bg-red-500 rounded-2xl text-white font-black">削除</button>
```


## 3. 「戻る」のボタン

![button03](/images/articles/tailwind-css-cheat-design/button03.png)
:::message

:::
```html
<button class="py-1 px-5 bg-gray-100/50 border rounded-2xl">戻る</button>
```



