---
title: "[Tailwind CSS] ロードUI/スケルトンUI" # 記事のタイトル
emoji: "🍃" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ui", "css", "tailwindcss", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**ロード UI/スケルトン UI の実装方法**を解説します。
以下のコードは、下記サイトで表示を確認しています。

@[card](https://play.tailwindcss.com/)

:::message
コードは Tailwind CSS v3 で表示を確認したものです。
:::

:::details 参考資料
@[card](https://gihyo.jp/book/2024/978-4-297-13943-8)
:::

## 🌱 1. iPhone で見かけそうな UI

![Tailwind-CSS-animate-spin](https://storage.googleapis.com/zenn-user-upload/ae6609094bb9-20241201.gif)

```html
<div class="flex items-center justify-center animate-spin min-h-screen">
  <svg class="absolute w-4 h-2 bg-gray-100/50 rounded-lg translate-x-[24px] translate-y-0 rotate-0"></svg>
  <svg class="absolute w-4 h-2 bg-gray-200/50 rounded-lg translate-x-[17px] translate-y-[17px] rotate-[45deg]"></svg>
  <svg class="absolute w-4 h-2 bg-gray-300/50 rounded-lg translate-x-0 translate-y-[24px] rotate-[90deg]"></svg>
  <svg class="absolute w-4 h-2 bg-gray-400/50 rounded-lg translate-x-[-17px] translate-y-[17px] rotate-[135deg]"></svg>
  <svg class="absolute w-4 h-2 bg-gray-500/50 rounded-lg translate-x-[-24px] translate-y-0 rotate-0"></svg>
  <svg
    class="absolute w-4 h-2 bg-gray-600/50 rounded-lg translate-x-[-17px] translate-y-[-17px] rotate-[-135deg]"
  ></svg>
  <svg class="absolute w-4 h-2 bg-gray-700/50 rounded-lg translate-x-0 translate-y-[-24px] rotate-[-90deg]"></svg>
  <svg class="absolute w-4 h-2 bg-gray-800/50 rounded-lg translate-x-[17px] translate-y-[-17px] rotate-[-45deg]"></svg>
</div>
```

## 🌱 2. Android で見かけそうな UI

![Tailwind-CSS-animate-spin-2](https://storage.googleapis.com/zenn-user-upload/545cea848b7a-20241201.gif)

```html
<div class="flex items-center justify-center min-h-screen">
  <svg class="w-10 h-10 border-4 border-gray-400 border-t-transparent rounded-full animate-spin"></svg>
</div>
```

## 🌱 3. YouTube で見かけそうな UI

![Tailwind-CSS-animate-pulse](https://storage.googleapis.com/zenn-user-upload/dedf353c1f82-20241201.gif)

```html
<div class="flex items-center justify-center min-h-screen">
  <svg class="w-10 h-10 bg-gray-400 rounded-full animate-pulse"></svg>
  <svg class="w-20 h-10 bg-gray-400 rounded-lg animate-pulse mx-4"></svg>
</div>
```

## 🌱 4. ゲームのロード画面の右下で見かけそうな UI

![Tailwind-CSS-animate-bounce](https://storage.googleapis.com/zenn-user-upload/d183a29fc437-20241201.gif)

```html
<div class="flex items-center justify-center min-h-screen">
  <svg class="w-12 h-12 bg-gradient-to-t from-gray-300 to-gray-400 rounded-full animate-bounce"></svg>
</div>
```

:::message
2 と 4 の画像は、親要素にも`animate-spin`を付けていた修正前のコードで表示したものです。修正前は、2 は回転が2倍の速さになり、4 は跳ねるボールが画面の中心を軸に回転していました。
:::

## 🌱 補足

- v4 では、`bg-gradient-to-t`が`bg-linear-to-t`に名前が変わりました。
- スクリーンリーダーの利用者に読み込み中であることを伝えるには、外側の要素に`role="status"`を付け、`<span class="sr-only">読み込み中</span>`を加えます。
- 動きを減らす設定（`prefers-reduced-motion`）をしているユーザーのために、`motion-reduce:animate-none`を併用すると、アニメーションを止められます。
