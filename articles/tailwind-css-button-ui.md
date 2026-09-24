---
title: "[Tailwind CSS] ボタンUI" # 記事のタイトル
emoji: "🍃" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["css", "tailwindcss", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**Tailwind CSS でよく使うボタン（登録・削除・戻る）のデザイン**を紹介します。

各ボタンは、下記サイト（Tailwind Play）で表示を確認しています。

@[card](https://play.tailwindcss.com/)

:::message
画像は Tailwind CSS v3 で表示したものです。v4 では、ボタンにマウスを乗せたときのカーソルが指の形（`pointer`）から矢印（`default`）に変わりました。指の形にしたい場合は、`cursor-pointer`を追加してください。
:::

:::details 参考資料
@[card](https://gihyo.jp/book/2024/978-4-297-13943-8)
:::

## 🌱 1. 「登録」のボタン

![登録ボタン](/images/articles/tailwind-css-cheat-design/button01.png)

```html
<button type="button" class="py-1 px-5 bg-sky-500 rounded-2xl text-white font-black">登録</button>
```

:::message
**工夫ポイント**

1. プライマリーカラーなどの色を使用することで、この後にイベントが発生することをイメージさせる
2. 文字を白色にすることで、背景色との対比で文字を目立たせ、テキストではなくボタンであることを強調する
3. ボタンの角を丸くすることで、ボタンらしさを強調する
:::

## 🌱 2. 「削除」のボタン

![削除ボタン](/images/articles/tailwind-css-cheat-design/button02.png)

```html
<button type="button" class="py-1 px-5 bg-red-500 rounded-2xl text-white font-black">削除</button>
```

:::message
**工夫ポイント**

1. 警告カラーを使用することで、この後に重大なイベントが発生することをイメージさせる
2. 文字を白色にすることで、背景色との対比で文字を目立たせ、テキストではなくボタンであることを強調する
3. ボタンの角を丸くすることで、ボタンらしさを強調する
:::

## 🌱 3. 「戻る」のボタン

![戻るボタン](/images/articles/tailwind-css-cheat-design/button03.png)

```html
<button type="button" class="py-1 px-5 bg-gray-100/50 border border-gray-200 rounded-2xl">戻る</button>
```

:::message
**工夫ポイント**

1. これまでのボタンと異なり、グレーを使用することで、この後に影響の小さい操作が行われることをイメージさせる
2. ボタンの角を丸くすることで、ボタンらしさを強調する
:::

## 🌱 補足

- `<button>`は、`<form>`の中に置くと、`type`を省略した場合に`type="submit"`として動きます。フォームを送信しないボタンには`type="button"`を付けます。
- 白い文字と`bg-sky-500`・`bg-red-500`の組み合わせは、WCAG が本文に求めるコントラスト比（4.5:1）を満たしません。読みやすさを重視する場合は、`bg-sky-700`や`bg-red-600`のように濃い色を使います。
- 枠線の色を指定しない`border`は、v3 では`gray-200`、v4 では文字色（`currentColor`）になります。バージョンによらず同じ見た目にするため、`border-gray-200`を指定しています。
