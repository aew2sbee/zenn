---
title: "[Playwright] ログイン状態でテストする簡単なやり方" # 記事のタイトル
emoji: "🎭" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["playwright", "テスト", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

`Playwright`で認証のテストに苦手意識があり、避けていましたが、
業務でやる必要が発生したため、試行錯誤しながら実装しました。
その時に発見した簡単なやり方を紹介します。

紹介する方法では実現できない場合や詳細な内容を知りたい方は、
公式ドキュメントをご確認ください
@[card](https://playwright.dev/docs/auth)

## 🌱 結論

:::message

1. `playwright.config.ts`に指定の`session.json`を設定する
2. 各テストごとに指定の`session.json`を設定する

:::

## 🌱 事前準備

:::message alert
下記の`session.json`は、サンプルとして[iron-session](https://www.npmjs.com/package/iron-session)を設定にしております。
**ご参考にする場合は、PJ に応じて設定してください**
:::

```bash
# ファイル構成
tests
└── e2e
    └── .auth
        └── session.json
```

```json:session.json
// iron-sessionのサンプルです
{
  "cookies": [
    {
      "name": "iron-session/example-app",
      "value": "REDACTED_SESSION_TOKEN",
      "domain": "your-backend.example.com",
      "path": "/",
      "expires": -1,
      "httpOnly": true,
      "secure": false,
      "sameSite": "Lax"
    }
  ],
  "origins": []
}

```

## 🌱 1. `playwright.config.ts`に指定の`session.json`を設定する

:::message

- メリット
  - `playwright.config.ts`のみのコードの改修済む
  - 各ブラウザ毎で異なる認証情報を設定できる

- デメリット
  - 共通の認証情報になるので、環境依存には対応できない(STG 環境, PRD 環境等)

:::

`playwright.config.ts`を改修し、あとはテストを実行するだけです。

```diff ts:playwright.config.ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  projects: [
    {
      name: 'chromium',
      use: {
        ...devices['Desktop Chrome'],
+        storageState: './tests/e2e/.auth/session.json',
      },
    },
  ],
});
```

## 🌱 2. 各テストごとに指定の`session.json`を設定する

:::message

- メリット
  - テスト内容に併せて認証情報を切り替えられる

- デメリット
  - テスト毎に認証情報の json を用意、配置が大変

:::

テスト内容に応じて`session.json`を設定し、あとはテストを実行するだけです。

```ts:
// 指定の`session.json'を読み込む
const SESSION_JSON_PATH = `./tests/e2e/.auth/session.json`;
const context = await browser.newContext({ storageState: SESSION_JSON_PATH });
const page = await context.newPage();

// 実際のテスト処理
await page.goto('https://example.com/');
```
