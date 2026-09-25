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
公式ドキュメントをご確認ください。

@[card](https://playwright.dev/docs/auth)

## 🌱 結論

:::message

1. `playwright.config.ts`に指定の`session.json`を設定する
2. 各テストごとに指定の`session.json`を設定する

:::

## 🌱 事前準備

:::message alert
下記の`session.json`は、[iron-session](https://www.npmjs.com/package/iron-session)を使った場合のサンプルです。
**参考にする場合は、プロジェクトに応じて設定してください。**
:::

```bash
# ファイル構成
tests
└── e2e
    └── .auth
        └── session.json
```

```json:session.json
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

:::message alert
**`session.json`を使う際の注意**

- `session.json`には認証情報が含まれるため、Git で管理しないでください（`.gitignore`に`tests/e2e/.auth/`を追加する）。また、本番環境のセッションは使わず、テスト用のアカウントを使ってください。
- `value`は iron-session が暗号化した値で、有効期限があります。期限切れや暗号化キー（password）の変更で無効になるため、定期的に作り直す必要があります。
- `domain`は、テストで開くページのホストに合わせてください。
- `secure: false`は、ローカルの HTTP 環境でテストするための値です。HTTPS の環境では`true`にしてください（アプリ側の Cookie 設定を変えるものではありません）。

公式ドキュメントでは、setup project で実際にログインし、`page.context().storageState({ path })`で`session.json`を自動生成する方法が紹介されています。
:::

## 🌱 1. `playwright.config.ts`に指定の`session.json`を設定する

:::message

- メリット
  - `playwright.config.ts`の改修だけで済む
  - project（ブラウザ）ごとに異なる認証情報を設定できる

- デメリット
  - 環境ごと（STG 環境, PRD 環境等）に切り替えるには、環境変数や project の追加など config 側の工夫が必要

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

:::message
`storageState`は project ごとの設定です。上記の例では`chromium`にしか適用されません。
すべての project に適用したい場合は、トップレベルの`use`に書きます。
:::

## 🌱 2. 各テストごとに指定の`session.json`を設定する

:::message

- メリット
  - テスト内容に合わせて認証情報を切り替えられる

- デメリット
  - ユーザーの種類ごとに認証情報の JSON を用意・配置する手間がかかる

:::

テスト内容に応じて`session.json`を設定し、あとはテストを実行するだけです。

```ts:tests/e2e/example.spec.ts
import { test } from '@playwright/test';

test('ログイン状態で表示できる', async ({ browser }) => {
  // 指定の session.json を読み込む
  const SESSION_JSON_PATH = './tests/e2e/.auth/session.json';
  const context = await browser.newContext({ storageState: SESSION_JSON_PATH });
  const page = await context.newPage();

  // 実際のテスト処理
  await page.goto('https://example.com/');

  await context.close();
});
```

:::message
ファイル単位や`test.describe`単位で指定する場合は、`test.use`を使う方法もあります。
この方法なら、`page`フィクスチャをそのまま使えて、後片付けも不要です。

```ts:tests/e2e/example.spec.ts
import { test } from '@playwright/test';

test.use({ storageState: './tests/e2e/.auth/session.json' });

test('ログイン状態で表示できる', async ({ page }) => {
  await page.goto('https://example.com/');
});
```

:::
