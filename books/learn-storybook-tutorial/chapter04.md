---
title: "ブランチごとにGitHub Pagesを用意する"
---

## 🌱 このチャプターのゴール

`main` と `develop` で **それぞれ別の URL** に Storybook を公開し、ブランチごとのデザイン差分をブラウザで確認できるようにします。


![main-develop-design](/images/books/learn-storybook-tutorial/main-develop-design.png)
*左: `main`ブランチのデザイン / 右: `develop`ブランチのデザイン*

▼ `main`ブランチの内容はこちらから確認できます
@[card](https://aew2sbee.github.io/tech-storybook/)

▼ `develop`ブランチの内容はこちらから確認できます
@[card](https://aew2sbee.github.io/tech-storybook/develop)

:::message
**ポイント**
ブランチ名に `/`（スラッシュ）が入る場合
（例: `feature/button`）は、URL 用に `feature-button` のように変換されます。
この変換ルールは後述の Workflow 内で設定します。
:::

## 🌱 GitHub Actionを編集する
ここでは、ブランチごとに異なる URL で Storybook を公開するための
GitHub Actions の Workflow を作成します。

この Workflow では、次のような動きをします。

- `main` ブランチの場合
  → `gh-pages` ブランチのルート（`/`）に Storybook をデプロイ
- `main` 以外のブランチの場合
  → ブランチ名をもとにしたサブディレクトリにデプロイ
    （例: `develop` → `/develop`、`feature/button` → `/feature-button`）

これにより、**本番用・開発中・検証中**の Storybook を同時に公開できます。

```yml .github/workflows/storybook-pages.yml
# ================================
# GitHub Pages に Storybook を公開するワークフロー（ブランチ別パス）
# - main は / に公開
# - それ以外のブランチは /<branch名>/ に公開
# ================================
name: Deploy Storybook to GitHub Pages (per-branch)

on:
  push:
    # ここに並んだブランチがデプロイ対象になります（必要に応じて追加OK）
    branches:
      - main
      - develop
      - "feature/**"
  workflow_dispatch:

# gh-pages ブランチへ push するので write が必要
permissions:
  contents: write

concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      # (1) リポジトリを取得
      - name: Checkout repository
        uses: actions/checkout@v4

      # (2) Node.js セットアップ
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      # (3) 依存関係インストール
      - name: Install dependencies
        run: npm ci

      # (4) Storybook を静的ビルド
      - name: Build Storybook
        run: npm run build-storybook

      # (5) ブランチ名から公開先フォルダ名を作る（main 以外だけ使う）
      - name: Decide destination directory
        id: dest
        if: github.ref_name != 'main'
        run: |
          BRANCH="${GITHUB_REF_NAME}"
          SAFE_BRANCH="$(echo "$BRANCH" | sed 's/\//-/g')"
          echo "dir=$SAFE_BRANCH" >> $GITHUB_OUTPUT

      # main: ルートへ（destination_dir を指定しない）
      - name: Deploy main to gh-pages root
        if: github.ref_name == 'main'
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_branch: gh-pages
          publish_dir: storybook-static
          keep_files: true

      # main 以外: サブディレクトリへ
      - name: Deploy branch to subdir
        if: github.ref_name != 'main'
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_branch: gh-pages
          publish_dir: storybook-static
          destination_dir: ${{ steps.dest.outputs.dir }}
          keep_files: true

```


## 🌱 GitHub > Settings > Pagesを編集する
以下のように設定します。
- Source: `Deploy from a branch`
- Branch: `gh-pages` / `/(root)`

![setting-github-action](/images/books/learn-storybook-tutorial/setting-github-action.png)


:::message
**ポイント**
Branch に`gh-pages`が表示されない場合は、
まず Workflow（`yml`）を`push`して`GitHub Actions`を一度実行してください。
:::

