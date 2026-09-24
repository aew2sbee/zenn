---
title: "[Playwright] ロケーターのfilter()の種類と使い方" # 記事のタイトル
emoji: "🎭" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["playwright", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**Playwright の filter()** についてまとめています。

## 🌱 結論

:::message
filter()メソッドを使うと、取得したロケーターを条件で絞り込めます。

```ts
// 特定のテキストを含む
.filter({ hasText: テキスト })
// 特定のテキストを含まない
.filter({ hasNotText: テキスト })
// 特定の要素を含む（子孫要素がロケーターに一致する）
.filter({ has: ロケーター })
// 特定の要素を含まない
.filter({ hasNot: ロケーター })
```

:::

:::message alert
`hasText` には、文字列（部分一致・大文字小文字を区別しない）か、正規表現（`/Login/`）を渡します。
`"/Login/"` のように正規表現を文字列で書くと、「/Login/」という文字列そのものを探すため一致しません。
:::

## 🌱 1. 特定のテキストを含む: { hasText: テキスト }

```ts
// getByRole('button') でページ上のすべてのボタンを取得
// filter({ hasText: 'Login' }) で「Login」というテキストを含むボタンに絞り込む
// そのボタンをクリック
await page.getByRole("button").filter({ hasText: "Login" }).click();
```

## 🌱 2. 特定のテキストを含まない: { hasNotText: テキスト }

```ts
// getByRole('button') でページ上のすべてのボタンを取得
// filter({ hasNotText: 'Login' }) で「Login」というテキストを含まないボタンに絞り込む
// そのボタンをクリック
await page.getByRole("button").filter({ hasNotText: "Login" }).click();
```

## 🌱 3. 特定の要素を含む: { has: ロケーター }

`has` は、要素の属性ではなく、**子孫要素**がロケーターに一致するかで絞り込みます。

```ts
// getByRole('listitem') でリストの項目を取得
// filter({ has: page.getByRole('button', { name: '削除' }) }) で「削除」ボタンを含む項目に絞り込む
await page
  .getByRole("listitem")
  .filter({ has: page.getByRole("button", { name: "削除" }) });
```

## 🌱 4. 特定の要素を含まない: { hasNot: ロケーター }

```ts
// getByRole('listitem') でリストの項目を取得
// filter({ hasNot: page.getByRole('button', { name: '削除' }) }) で「削除」ボタンを含まない項目に絞り込む
await page
  .getByRole("listitem")
  .filter({ hasNot: page.getByRole("button", { name: "削除" }) });
```

## 🌱 5. 複数の条件で絞り込む

```ts
// getByRole('listitem') でリストの項目を取得
// filter({ hasText: 'Next', has: page.locator('.enabled') }) で「Next」というテキストを含み、
// かつ enabled クラスを持つ子孫要素を含む項目に絞り込む
await page
  .getByRole("listitem")
  .filter({ hasText: "Next", has: page.locator(".enabled") })
  .click();
```
