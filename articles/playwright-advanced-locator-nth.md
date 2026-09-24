---
title: "[Playwright] 複数の要素からn番目を取得する" # 記事のタイトル
emoji: "🎭" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["playwright", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**Playwright の.nth(index)/.first()/.last()** についてまとめています。

複数の要素に一致するロケーターをそのまま `click()` すると、strict mode violation のエラーになります。
そのようなときに、何番目の要素を操作するかを指定できるのが、これらのメソッドです。

@[card](https://gihyo.jp/book/2024/978-4-297-14220-9)

## 🌱 結論

:::message
下記メソッドで絞り込みができます。

```ts
// 最初の要素
locator.first();
// 3番目の要素（0始まり）
locator.nth(2);
// 最後の要素
locator.last();
```

:::

:::message alert
公式ドキュメントでは、位置に依存するため first/last/nth の使用は推奨されていません（画面が変わると、意図しない要素を操作してしまうため）。
まずは `getByRole("button", { name: "保存" })` や `filter({ hasText: ... })` で一意に絞り込み、それでも特定できない場合に使いましょう。
:::

## 🌱 1. 最初の要素を取得する

```ts
// 最初のボタンをクリックする
await page.getByRole("button").first().click();
```

## 🌱 2. n 番目の要素を取得する

`nth()` のインデックスは0始まりです。`nth(0)` は `first()` と同じです。
また、`nth(-1)` のように負の値を渡すと末尾から数えるため、`nth(-1)` は `last()` と同じになります。

```ts
// 3番目のボタンをクリックする
await page.getByRole("button").nth(2).click();
```

## 🌱 3. 最後の要素を取得する

```ts
// 最後のボタンをクリックする
await page.getByRole("button").last().click();
```
