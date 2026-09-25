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

## 🌱 chapter03の方式との違い
chapter03で使った`actions/deploy-pages`は、デプロイのたびにサイト全体を置き換えます。
そのため、ブランチごとに別のフォルダへ公開することができません。

そこで、公開用のファイルを置く専用ブランチ`gh-pages`を用意し、ブランチごとのサブディレクトリにファイルを追加していく方式に切り替えます。
`gh-pages`ブランチへのデプロイには、[peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)を使います。

## 🌱 GitHub Actionsのワークフローを置き換える
chapter03で作成した`.github/workflows/storybook-pages.yml`の中身を、すべて以下の内容に置き換えます。

この Workflow では、次のような動きをします。

- `main` ブランチの場合
  → `gh-pages` ブランチのルート（`/`）に Storybook をデプロイ
- `main` 以外のブランチの場合
  → ブランチ名をもとにしたサブディレクトリにデプロイ
    （例: `develop` → `/develop`、`feature/button` → `/feature-button`）

```yml:.github/workflows/storybook-pages.yml
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

# gh-pages ブランチへの push が競合しないよう、1つずつ順番に実行する
# （実行中のデプロイはキャンセルせず、完了を待つ）
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      # (1) リポジトリを取得
      - name: Checkout repository
        uses: actions/checkout@v7

      # (2) Node.js セットアップ
      - name: Setup Node.js
        uses: actions/setup-node@v7
        with:
          node-version: 22
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

- `sed 's/\//-/g'`: ブランチ名の`/`をすべて`-`に置き換えます
- `echo "dir=..." >> $GITHUB_OUTPUT`: 置き換えた名前を、このステップの出力値`dir`として保存します
- `steps.dest.outputs.dir`: 上のステップ（`id: dest`）が出力した値を参照しています
- `keep_files: true`: `gh-pages`ブランチにある既存のファイルを消さずに上書きします。これがないと、他のブランチ用に公開したフォルダが削除されます

:::message
**ポイント**
ブランチ名に`/`（スラッシュ）が入る場合（例: `feature/button`）は、URL 用に`feature-button`のように変換されます。
ただし、`feature/a-b`と`feature-a/b`のように、別のブランチが同じフォルダ名になると互いに上書きされます。
:::

:::message alert
**注意**
対象のブランチに`push`すると、その内容は誰でも見られるURLで公開されます。
未公開の機能や機密データを含むストーリーは、対象のブランチに`push`しないでください。
また、`keep_files: true`のため、ブランチを削除しても`gh-pages`ブランチ上のフォルダは残ります。
不要になったフォルダは、`gh-pages`ブランチから手動で削除してください。
:::

## 🌱 mainとdevelopにpushする
置き換えたワークフローを`main`ブランチに`push`します。

```bash
git add .
git commit -m "ブランチごとにGitHub Pagesへ公開する"
git push
```

続いて`develop`ブランチを作成し、Buttonの色などを変更して`push`します。

```bash
git switch -c develop
# Button.tsx の色などを変更する
git add .
git commit -m "Buttonのデザインを変更"
git push -u origin develop
```

`GitHub`の`Actions`タブで、両方のワークフローが成功していることを確認します。
成功すると`gh-pages`ブランチが作成されます。

## 🌱 GitHub > Settings > Pagesを編集する
本章では公開用のファイルを`gh-pages`ブランチに置く方式に変えたため、Pagesの公開元もそのブランチに切り替えます。
chapter03で設定した Source を`GitHub Actions`から次のように変更します。
- Source: `Deploy from a branch`
- Branch: `gh-pages` / `/(root)`

![setting-github-action](/images/books/learn-storybook-tutorial/setting-github-action.png)

:::message
**ポイント**
Branch に`gh-pages`が表示されない場合は、前の手順のワークフローが成功しているかを確認してください。
:::

## 🌱 公開URLを確認する
次のURLで、ブランチごとの`Storybook`が表示されることを確認します。

- `main`: `https://<ユーザー名>.github.io/<リポジトリ名>/`
- `develop`: `https://<ユーザー名>.github.io/<リポジトリ名>/develop/`

## 🌱 おわりに
本書では、次の内容を扱いました。

- `Next.js`と`Storybook`の環境構築
- 自作のUIコンポーネントを`Storybook`に登録する
- `Storybook`をGitHub Pagesに公開する
- ブランチごとに別のURLで公開する

さらに学ぶ場合は、[Storybook公式チュートリアル](https://storybook.js.org/tutorials/intro-to-storybook/react/ja/get-started/)の続きを参照してください。
