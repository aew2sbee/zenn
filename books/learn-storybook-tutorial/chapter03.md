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

- GitHub Pages: GitHubが提供する、静的サイトを無料で公開できるサービスです
- GitHub Actions: `push`などをきっかけに、ビルドやデプロイを自動で実行する仕組みです

:::message
**前提**
GitHubで空のリポジトリを作成し、ここまでのプロジェクトを登録しておきます。
`create-next-app`で`git init`は済んでいるため、次のコマンドで登録できます。

```bash
git remote add origin https://github.com/<ユーザー名>/<リポジトリ名>.git
git add .
git commit -m "Storybookを追加"
git push -u origin main
```
:::

## 🌱 Storybookの設定ファイルを変更
`staticDirs`は、画像などの静的ファイルを置いたフォルダを`Storybook`に知らせる設定です。
Windowsでインストールした場合、`staticDirs` のパスは **Windows形式（バックスラッシュ）** で生成されます。
GitHub Actions（Linux環境）では`\`がパスの区切りとして解釈されず、フォルダが見つからないためビルドに失敗する可能性があります。
そのため、**URL/Unix形式（スラッシュ）** に変更します（Windowsでもスラッシュ形式で動作します）。

```diff ts:.storybook/main.ts
import type { StorybookConfig } from '@storybook/nextjs-vite';

const config: StorybookConfig = {
  "stories": [
    "../src/**/*.stories.@(js|jsx|ts|tsx)"
  ],
  "addons": ["@storybook/addon-docs"],
  "framework": "@storybook/nextjs-vite",
  "staticDirs": [
-    "..\\public"
+    "../public"
  ]
};
export default config;

```

:::message
**ポイント**
`staticDirs`で指定した`public/`がリポジトリに無いと、ビルドが失敗します。
`create-next-app`で作成した`public/`には最初からsvgファイルがあるため、通常は対応不要です。
svgファイルを削除して`public/`が空になった場合は、Gitは空のフォルダを管理できないため、`.gitkeep`という空ファイルを置いてフォルダを残します。

```bash
# macOS / Linux
touch public/.gitkeep
# Windows（PowerShell）
New-Item public/.gitkeep -ItemType File
```
:::

## 🌱 GitHub Actions専用ファイルを作成する
`main`ブランチへの`push`をトリガーに自動デプロイし、さらに「Run workflow」から手動実行もできるように`yml`を作成します。
なお、`secrets.GITHUB_TOKEN`などの認証情報はGitHubが自動で用意するため、自分で作成する必要はありません。

```yml:.github/workflows/storybook-pages.yml
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

# 同時に複数デプロイが走ると競合しやすいので、1つずつ順番に実行する設定
# （実行中のデプロイはキャンセルせず、完了を待つ）
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  # ----------------------------
  # 1) build: Storybook をビルドして成果物（静的ファイル）を用意する
  # ----------------------------
  build:
    runs-on: ubuntu-latest

    steps:
      # (1) リポジトリのコードを Actions の実行環境に取得する
      - name: Checkout repository
        uses: actions/checkout@v7

      # (2) Node.js をセットアップする（Storybook を動かすため）
      # cache: npm にすると npm の依存関係キャッシュが効いて速くなりやすい
      - name: Setup Node.js
        uses: actions/setup-node@v7
        with:
          node-version: 22
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
        uses: actions/upload-pages-artifact@v5
        with:
          path: storybook-static

  # ----------------------------
  # 2) deploy: build の成果物を GitHub Pages にデプロイする
  # ----------------------------
  deploy:
    needs: build            # build が成功してから deploy を実行する
    runs-on: ubuntu-latest

    # GitHub Pages 用の environment を使う（Actions の画面に公開URLが表示される）
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    steps:
      # artifact を GitHub Pages にデプロイする公式アクション
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v5
```

:::message
**ポイント**
`node-version`は、ローカル環境と同じメジャーバージョンを指定してください。
アクションのバージョン（`@v7`など）は執筆時点のものです。
:::

## 🌱 GitHub > Settings > Pagesを編集する
`actions/deploy-pages`でデプロイするため、公開元をGitHub Actionsに設定します。
- Settings > Pages > Build and deployment > Source: `GitHub Actions`

詳細は公式ドキュメントを参照してください。
@[card](https://docs.github.com/ja/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)

## 🌱 mainブランチへpushする
作成したファイルを`main`ブランチに`push`します。

```bash
git add .
git commit -m "GitHub Pagesへのデプロイを追加"
git push
```

## 🌱 GitHub > Actionsタブを確認する

`GitHub`の`Actions`タブで、ワークフローが成功していることを確認します。

![success-github-action](/images/books/learn-storybook-tutorial/success-github-action.png)

成功していれば、冒頭のURLで公開された`Storybook`を確認できます。
