---
title: "GitHub Pagesにデプロイする"
---

## 🌱 このチャプターのゴール
ローカル環境で確認していた `Storybook` を、GitHub Pages に公開します。
公開URLは次の形式になります。

`https://<ユーザー名>.github.io/<リポジトリ名>/`

![original-button](/images/books/learn-storybook-tutorial/original-button.png)

▼ 私の場合はこちらから確認できます
@[card](https://aew2sbee.github.io/tech-storybook/)


## 🌱 Storybookの設定ファイルを変更
`staticDirs` のパスを **Windows形式（バックスラッシュ）から、URL/Unix形式（スラッシュ）** に変更します。
GitHub Actions（Linux環境）でビルドする場合、スラッシュ形式に統一しておくと安全です。

```diff ts .storybook/main.ts
import type { StorybookConfig } from '@storybook/nextjs-vite';

const config: StorybookConfig = {
  "stories": [
    "../src/**/*.stories.@(js|jsx|ts|tsx)"
  ],
  "addons": ["@storybook/addon-docs",],
  "framework": "@storybook/nextjs-vite",
  "staticDirs": [
-    "..\\public"
+    "../public"
  ]
};
export default config;

```

## 🌱 GitHub上にpublic/を作る
`public/`をリポジトリに含めるため、空フォルダにならないよう`.gitkeep`を置きます。

```bash
touch public/.gitkeep
```

## 🌱 GitHub Action専用ファイルを作成する
`main`ブランチへの`push`をトリガーに自動デプロイし、さらに「Run workflow」から手動実行もできるように`yml`を作成します。

```yml .github/workflows/storybook-pages.yml
# ================================
# GitHub Pages に Storybook を公開するワークフロー
# - main ブランチに push されたら自動デプロイ
# - 手動実行（workflow_dispatch）も可能
# ================================
name: Deploy Storybook to GitHub Pages

# いつ実行するか（トリガー）
on:
  # main ブランチに push されたら実行
  push:
    branches: [main]

  # GitHub 画面から「Run workflow」で手動実行もできる
  workflow_dispatch:

# GitHub Pages にデプロイするために必要な権限
# （GitHub Actions から Pages に書き込むため）
permissions:
  contents: read   # リポジトリの中身を読み取る（checkout に必要）
  pages: write     # GitHub Pages へデプロイする権限
  id-token: write  # OIDC で deploy-pages が認証するために必要

# 同時に複数デプロイが走ると競合しやすいので、1つにまとめる設定
concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  # ----------------------------
  # 1) build: Storybook をビルドして成果物（静的ファイル）を用意する
  # ----------------------------
  build:
    runs-on: ubuntu-latest

    steps:
      # (1) リポジトリのコードを Actions の実行環境に取得する
      - name: Checkout repository
        uses: actions/checkout@v4

      # (2) Node.js をセットアップする（Storybook を動かすため）
      # cache: npm にすると npm の依存関係キャッシュが効いて速くなりやすい
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      # (3) 依存関係をインストールする
      # npm ci は package-lock.json を基準に「再現性高く」インストールするコマンド
      - name: Install dependencies
        run: npm ci

      # (4) Storybook を「静的サイト」としてビルドする
      # 通常、storybook-static/ というフォルダにHTML/CSS/JSが出力される
      - name: Build Storybook
        run: npm run build-storybook

      # (5) build の成果物（storybook-static）を GitHub Pages 用の artifact としてアップロード
      # 後続の deploy ジョブがこの artifact を使ってデプロイする
      - name: Upload artifact for GitHub Pages
        uses: actions/upload-pages-artifact@v4
        with:
          path: storybook-static

  # ----------------------------
  # 2) deploy: build の成果物を GitHub Pages にデプロイする
  # ----------------------------
  deploy:
    needs: build            # build が成功してから deploy を実行する
    runs-on: ubuntu-latest

    # GitHub Pages 用の environment を使う（Pages のURLなどが紐づく）
    environment:
      name: github-pages

    steps:
      # artifact を GitHub Pages にデプロイする公式アクション
      - name: Deploy to GitHub Pages
        uses: actions/deploy-pages@v4
```

## 🌱 GitHub > Actionsタブを確認する

`GitHub`の`Action`タブで、ワークフローが成功していることを確認します。

![success-github-action](/images/books/learn-storybook-tutorial/success-github-action.png)

成功していれば、次のURLで公開された`Storybook`を確認できます。

`https://<ユーザー名>.github.io/<リポジトリ名>/`
