---
title: "[Playwright] 高度なロケーターのfilter()種類と使い方" # 記事のタイトル
emoji: '🎭' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['playwright', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに
この記事では、**Playwrightのfilter()** についてをまとめております。


> filter()メソッドを使うことで取得した要素の中からさらに条件に合致するものを絞り込むことができます。

## 1. 特定のテキストを持つ: { hasText: テキスト }
```ts
// getByRole('button') でページ上のすべてのボタンを取得
// filter({ hasText: 'Login' }) で「Login」というテキストを持つボタンをフィルタリング
// そのボタンをクリック
await page.getByRole('button').filter({ hasText: '/Login/' }).click();
```

## 1. 特定のテキストを持たない: { hasNotText: テキスト }
```ts
// getByRole('button') でページ上のすべてのボタンを取得
// filter({ hasText: 'Login' }) で「Login」というテキストを持たないボタンをフィルタリング
// そのボタンをクリック
await page.getByRole('button').filter({ hasNotText: '/Login/' }).click();
```

## 3. 特定の属性を持つ要素を絞り込む: { has: ロケーター }
```ts
// getByRole('textbox') でテキスト入力フィールドを取得
// filter({ has: page.locator('input[name="email"]') }) でname="email"属性を持つ入力フィールドをフィルタリング
await page.getByRole('textbox').filter({ has: page.locator('input[name="email"]') });
```

## 4. 特定の属性を持つ要素を絞り込む: { hasNot: ロケーター }
```ts
// getByRole('textbox') でテキスト入力フィールドを取得
// filter({ has: page.locator('input[name="email"]') }) でname="email"属性を持たない入力フィールドをフィルタリング
await page.getByRole('textbox').filter({ hasNot: page.locator('input[name="email"]') });
```

## 5. 複数の条件で絞り込む
```ts
// getByRole('button') でボタンを取得
// filter({ hasText: 'Next', has: page.locator('.enabled') }) で「Next」というテキストを持ち、かつクラス名が.enabledのボタンを絞り込む
await page.getByRole('button').filter({ hasText: '/Next/', has: page.locator('.enabled') }).click();
```

