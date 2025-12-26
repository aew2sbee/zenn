---
title: "[Playwright] 複数の要素からn番目を取得する" # 記事のタイトル
emoji: "🎭" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["playwright", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## はじめに

この記事では、**Playwright の.nth(index)/.first()/.last()** についてをまとめております。

:::details 参考資料
@[card](https://gihyo.jp/book/2024/978-4-297-14220-9)
:::

## 結論

:::message
下記メソッドで絞り込みができます。

```ts
// 最初の要素
first()
  // 3番目の要素
  .nth(2)
  // 最後
  .last();
```

:::

## 1. 最初の要素を取得する

```ts
// 最初のボタンをクリックする
await page.getByRole("button").first().click();
```

## 2. n 番目の要素を取得する

```ts
// 3番目のボタンをクリックする
await page.getByRole("button").nth(2).click();
```

## 3. 最後の要素を取得する

```ts
// 最後のボタンをクリックする
await page.getByRole("button").last().click();
```
