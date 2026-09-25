---
title: "[Tailwind CSS] n行を超える文章を「...」で省略する" # 記事のタイトル
emoji: "🍃" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["css", "tailwindcss", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**長い文章を省略し、先頭の数行だけを表示する方法**を解説します。

@[card](https://gihyo.jp/book/2024/978-4-297-13943-8)

## 🌱 前提条件

1. 下記サイトで表示を確認しています。

   @[card](https://play.tailwindcss.com/)

2. 見やすいように`font-size`を`200%`にしています。

   ```css
   html {
     font-size: 200%;
   }
   ```

3. 見やすいように下記の`div`で囲っています。

   ```html
   <div class="flex flex-col justify-center min-h-screen mx-10">
     <!-- 対象のhtmlを記載 -->
   </div>
   ```

4. `line-clamp-*`は、Tailwind CSS v3.3 以降で使えます。v3.2 以前は`@tailwindcss/line-clamp`プラグインが必要です。

## 🌱 結論

:::message
**`line-clamp-n`を使うと、n行目まで表示し、それ以降を「...」で省略できます。**

1. `line-clamp-1`〜`line-clamp-6`: 表示する行数を1〜6行で指定できます（v3 の既定値）。v3 で7行以上にする場合は`line-clamp-[8]`のように任意の値で指定します。v4 では`line-clamp-10`のように任意の整数を指定できます。
2. `line-clamp-none`: 行数の制限を解除し、すべてのテキストを表示します。`md:line-clamp-none`のように、画面幅に応じて省略を解除する場合に使います。

```html
<!-- 3行目まで表示し、4行目以降を省略する場合 -->
<p class="line-clamp-3">
  テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。テキストが入ります。
</p>
```

:::

:::message alert
`line-clamp-n`は、`display: -webkit-box`と`overflow: hidden`を使って省略しています。
同じ要素に`flex`や`block`などの`display`系、`overflow`系のクラスを付けると、省略が効かなくなります。
:::

## 🌱 見本

```html
<div class="mb-3">
  <p class="font-black">1行目まで表示</p>
  <p class="line-clamp-1 pl-3">ああああああああああああああああああああああああああああああああああああああああ</p>
</div>
<div class="mb-3">
  <p class="font-black">2行目まで表示</p>
  <p class="line-clamp-2 pl-3">
    あああああああああああああああああああああああああああああああああああああああ
    いいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいい
  </p>
</div>
```

![line-clamp](/images/articles/tailwind-css-cheat-sheet/line-clamp.png)
