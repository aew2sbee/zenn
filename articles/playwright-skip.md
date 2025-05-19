---
title: '[Playwright] 条件付きでテストをスキップする' # 記事のタイトル
emoji: '🎭' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['playwright', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、Playwright で**条件でスキップする** 方法ををまとめております。

:::details 参考資料
@[card](https://playwright.dev/docs/test-annotations#conditionally-skip-a-test)
:::

## 結論

テストコード内に`test.skip()`を追記し、条件とコメントを記載する
:::message

```diff
test('テストで確認内容', async ({ page }) => {
+  test.skip(Skipする条件, 'Skipする理由を記載');
  // --- 準備(Arrange) ---
  // --- 実行(Act) ---
  // --- 確認(Assert) ---
});
```

:::

## 1.Skip しない

```ts: test.spec.ts
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

## 2. Skip する

```ts: test.spec.ts
import { test, expect } from '@playwright/test';
const ENV:string = "production"

test('DEV環境はテスト実行する', async ({ page }) => {
  test.skip(ENV === 'production', '本番環境は○○ためテスト対象外');
  // --- 準備(Arrange) ---
  // --- 実行(Act) ---
  // --- 確認(Assert) ---
});
```

```diff ts: Skipしない/するのテストコード差分
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

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！

@[card](https://www.youtube.com/@aew2sbee)
