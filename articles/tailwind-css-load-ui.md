---
title: "[Tailwind CSS] ロードUI/スケルトンUI" # 記事のタイトル
emoji: "🍃" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ui", "css", "tailwindcss", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**ロード UI/スケルトン UI の実装方法**を解説します。
下記サイトを活用して表示しております。
@[card](https://play.tailwindcss.com/)

:::details 参考資料
@[card](https://gihyo.jp/book/2024/978-4-297-13943-8)
:::

## 1. iPhone で見かけそうな UI

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

## 2. Android で見かけそうな UI

![Tailwind CSS animate-spin-2](https://storage.googleapis.com/zenn-user-upload/545cea848b7a-20241201.gif)

```html
<div class="flex items-center justify-center animate-spin min-h-screen">
  <svg class="w-10 h-10 border-4 border-gray-400 border-t-transparent rounded-full animate-spin"></svg>
</div>
```

## 3. YouTube で見かけそうな UI

![Tailwind-CSS-animate-pulse](https://storage.googleapis.com/zenn-user-upload/dedf353c1f82-20241201.gif)

```html
<div class="flex items-center justify-center min-h-screen">
  <svg class="w-10 h-10 bg-gray-400 rounded-full animate-pulse"></svg>
  <svg class="w-20 h-10 bg-gray-400 rounded-lg animate-pulse mx-4"></svg>\
</div>
```

## 4. ゲームの右下で見かけそうな UI

![Tailwind-CSS-animate-bounce](https://storage.googleapis.com/zenn-user-upload/d183a29fc437-20241201.gif)

```html
<div class="flex items-center justify-center animate-spin min-h-screen">
  <svg class="w-12 h-12 bg-gradient-to-t from-gray-300 to-gray-400 rounded-full animate-bounce"></svg>
</div>
```
