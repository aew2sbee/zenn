---
title: "[Playwright] 条件付きでテストをスキップする" # 記事のタイトル
emoji: "🎭" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["playwright", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、Playwright で**条件付きでテストをスキップする**方法をまとめております。

:::details 参考資料
@[card](https://playwright.dev/docs/test-annotations#conditionally-skip-a-test)
:::

## 🌱 結論

テストコード内に`test.skip()`を追記し、条件とコメントを記載する

:::message

```diff
test('テストで確認する内容', async ({ page }) => {
+  test.skip(Skipする条件, 'Skipする理由を記載');
  // --- 準備(Arrange) ---
  // --- 実行(Act) ---
  // --- 確認(Assert) ---
});
```

:::

## 🌱 1. スキップしない

```ts:test.spec.ts
import { test, expect } from '@playwright/test';
const ENV:string = "development"

test('DEV環境はテスト実行する', async ({ page }) => {
  test.skip(ENV === 'production', '本番環境は○○ためテスト対象外');
  // --- 準備(Arrange) ---
  // --- 実行(Act) ---
  // --- 確認(Assert) ---
});
```

実行結果を確認します

```bash
$ npx playwright test tests/e2e/test.spec.ts

Running 1 test using 1 worker

  ✓  1 [Microsoft Edge] › test.spec.ts:5:5 › DEV環境はテスト実行する (1.0s)

  1 passed (2.9s)
```

## 🌱 2. スキップする

```ts:test.spec.ts
import { test, expect } from '@playwright/test';
const ENV:string = "production"

test('DEV環境はテスト実行する', async ({ page }) => {
  test.skip(ENV === 'production', '本番環境は○○ためテスト対象外');
  // --- 準備(Arrange) ---
  // --- 実行(Act) ---
  // --- 確認(Assert) ---
});
```

```diff ts:スキップしない/するのテストコード差分
import { test, expect } from '@playwright/test';
- const ENV:string = "development"
+ const ENV:string = "production"

test('DEV環境はテスト実行する', async ({ page }) => {
  test.skip(ENV === 'production', '本番環境は○○ためテスト対象外');
  // --- 準備(Arrange) ---
  // --- 実行(Act) ---
  // --- 確認(Assert) ---
});
```

実行結果を確認します

```bash
$ npx playwright test tests/e2e/test.spec.ts

Running 1 test using 1 worker

  -  1 [Microsoft Edge] › test.spec.ts:5:5 › DEV環境はテスト実行する

  1 skipped
```

:::message
- 実行結果は、実際に実行したテストファイルのものです。そのため、行番号（`test.spec.ts:5:5`）やテスト名が、記事のコード例と一部一致しません。
- 本記事では分かりやすさのために`ENV`を直接書き換えていますが、実際の運用では`process.env.ENV === 'production'`のように環境変数で切り替え、`ENV=production npx playwright test`のように実行時に指定するのが一般的です。
- テスト本体の中の`test.skip()`は、`beforeEach`やフィクスチャ（`page`など）の準備が終わった後に評価されます。複数のテストをまとめてスキップする場合は、`test.describe`の中やファイルの先頭に`test.skip()`を書く方法もあります。
:::
