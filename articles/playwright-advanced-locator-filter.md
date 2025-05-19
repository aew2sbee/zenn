---
title: '[Playwright] 高度なロケーターのfilter()種類と使い方' # 記事のタイトル
emoji: '🎭' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['playwright', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**Playwright の filter()** についてをまとめております。

## 結論

:::message
filter()メソッドを使うこと絞り込むことができます。

```ts
// 特定のテキストを持つ
.filter({ hasText: テキスト })
// 特定のテキストを持たない
.filter({ hasNotText: テキスト })
// 特定の属性を持つ
.filter({ has: ロケーター })
// 特定の属性を持たない
.filter({ hasNot: ロケーター })
```

:::

## 1. 特定のテキストを持つ: { hasText: テキスト }

```ts
// getByRole('button') でページ上のすべてのボタンを取得
// filter({ hasText: 'Login' }) で「Login」というテキストを持つボタンをフィルタリング
// そのボタンをクリック
await page.getByRole('button').filter({ hasText: '/Login/' }).click();
```

## 2. 特定のテキストを持たない: { hasNotText: テキスト }

```ts
// getByRole('button') でページ上のすべてのボタンを取得
// filter({ hasText: 'Login' }) で「Login」というテキストを持たないボタンをフィルタリング
// そのボタンをクリック
await page.getByRole('button').filter({ hasNotText: '/Login/' }).click();
```

## 3. 特定の属性を持つ: { has: ロケーター }

```ts
// getByRole('textbox') でテキスト入力フィールドを取得
// filter({ has: page.locator('input[name="email"]') }) でname="email"属性を持つ入力フィールドをフィルタリング
await page
  .getByRole('textbox')
  .filter({ has: page.locator('input[name="email"]') });
```

## 4. 特定の属性を持たない: { hasNot: ロケーター }

```ts
// getByRole('textbox') でテキスト入力フィールドを取得
// filter({ has: page.locator('input[name="email"]') }) でname="email"属性を持たない入力フィールドをフィルタリング
await page
  .getByRole('textbox')
  .filter({ hasNot: page.locator('input[name="email"]') });
```

## 5. 複数の条件で絞り込む

```ts
// getByRole('button') でボタンを取得
// filter({ hasText: 'Next', has: page.locator('.enabled') }) で「Next」というテキストを持ち、かつクラス名が.enabledのボタンを絞り込む
await page
  .getByRole('button')
  .filter({ hasText: '/Next/', has: page.locator('.enabled') })
  .click();
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！

@[card](https://www.youtube.com/@aew2sbee)
