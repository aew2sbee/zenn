---
title: "ブランチごとにGitHub Pagesを用意する"
---

## 🌱 このチャプターのゴール

`main`と`develop`で **それぞれ別のURL** に`Storybook`を公開し、ブランチごとのデザイン差分をブラウザで確認できるようにします。


![main-develop-design](/images/books/learn-storybook-tutorial/main-develop-design.png)
*左: `main`ブランチのデザイン / 右: `develop`ブランチのデザイン*

▼ `main`ブランチの内容はこちらから確認できます

@[card](https://aew2sbee.github.io/poc-storybook/)

▼ `develop`ブランチの内容はこちらから確認できます

@[card](https://aew2sbee.github.io/poc-storybook/develop/)

## 🌱 前のチャプターの方式との違い
チャプター「GitHub Pagesにデプロイする」で使った`actions/deploy-pages`は、デプロイのたびにサイト全体を置き換えます。
そのため、ブランチごとに別のフォルダへは公開できません。
詳細は公式ドキュメントを参照してください。

@[card](https://docs.github.com/ja/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages)

そこで、公開用のファイルを置く専用ブランチ`gh-pages`を用意し、ブランチごとのフォルダにファイルを追加していく方式に切り替えます。
`gh-pages`ブランチへのデプロイには、[peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)を使います。

## 🌱 GitHub Actionsのワークフローを置き換える
チャプター「GitHub Pagesにデプロイする」で作成した`.github/workflows/storybook-pages.yml`の中身を、すべて以下の内容に置き換えます。

このワークフローは、次のように動作します。

- `main`ブランチの場合
  → `gh-pages`ブランチのルート（`/`）に`Storybook`をデプロイ
- `main`以外のブランチの場合
  → ブランチ名をもとにしたフォルダにデプロイ
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
    # "feature/**" は feature/button のように feature/ から始まるすべてのブランチが対象
    # ここにないブランチは公開されません
    branches:
      - main
      - develop
      - "feature/**"
  workflow_dispatch:

# gh-pages ブランチへ push するので write が必要
permissions:
  contents: write

# gh-pages ブランチへの push が競合しないよう、同時に実行するデプロイを1つだけにする
# （実行中のものはキャンセルしない。待機中のものは最新の1件だけが残る）
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

      # main 以外: ブランチ名のフォルダへ
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

- `github.ref_name` / `GITHUB_REF_NAME`: `push`されたブランチ名（例: `main`、`develop`、`feature/button`）が入ります
- `sed 's/\//-/g'`: ブランチ名の`/`をすべて`-`に置き換えます
- `echo "dir=..." >> $GITHUB_OUTPUT`: 置き換えた名前を、このステップの出力値`dir`として保存します
- `steps.dest.outputs.dir`: 上のステップ（`id: dest`）が出力した値を参照しています
- `secrets.GITHUB_TOKEN`: `gh-pages`ブランチへ`push`するための認証情報です。GitHubが自動で用意するため、自分で作成する必要はありません
- `keep_files: true`: `gh-pages`ブランチにある既存のファイルを残したまま、新しいファイルを追加・上書きします。`main`のステップは`destination_dir`を指定せずルートに公開するため、これがないと他のブランチ用のフォルダまで削除されます（[公式README](https://github.com/peaceiris/actions-gh-pages#%EF%B8%8F-keeping-existing-files-keep_files)）

:::message
**ポイント**
ブランチ名に`/`（スラッシュ）が入る場合（例: `feature/button`）は、URL用に`feature-button`のように変換されます。
ただし、`feature/a-b`と`feature/a/b`のように、別のブランチが同じフォルダ名（`feature-a-b`）になると互いに上書きされます。
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

続いて`develop`ブランチを作成し、Buttonの色を変更します。
ここでは、`primary`の色を水色（`sky`）からピンク（`pink`）に変更します。

```bash
git switch -c develop
```

```diff tsx:src/client/components/ui/Button/Button.tsx
const colorMap = {
-  primary: "bg-sky-400 text-white hover:bg-sky-500 active:bg-sky-600",
+  primary: "bg-pink-400 text-white hover:bg-pink-500 active:bg-pink-600",
  secondary: "border border-slate-300 bg-white text-slate-900 hover:bg-slate-50 active:bg-slate-100",
} as const;
```

変更を`develop`ブランチに`push`します。

```bash
git add .
git commit -m "Buttonのデザインを変更"
git push -u origin develop
```

`GitHub`の`Actions`タブで、両方のワークフローが成功していることを確認します。
成功すると`gh-pages`ブランチが作成されます。

## 🌱 GitHub > Settings > Pagesを編集する
このチャプターでは公開用のファイルを`gh-pages`ブランチに置く方式に変えたため、Pagesの公開元もそのブランチに切り替えます。
チャプター「GitHub Pagesにデプロイする」で設定したSourceを、`GitHub Actions`から次のように変更します。
- Source: `Deploy from a branch`
- Branch: `gh-pages` / `/(root)`

![setting-github-action](/images/books/learn-storybook-tutorial/setting-github-action.png)

詳細は公式ドキュメントを参照してください。

@[card](https://docs.github.com/ja/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)

:::message
**ポイント**
Branchに`gh-pages`が表示されない場合は、前の手順のワークフローが成功しているかを確認してください。
:::

## 🌱 公開URLを確認する
Sourceを切り替えると、`Actions`タブで`pages build and deployment`というワークフローが実行されます。
完了するまで数分かかり、その間は404や古い内容が表示されます。
完了後、次のURLでブランチごとの`Storybook`が表示されることを確認します。

- `main`: `https://<ユーザー名>.github.io/<リポジトリ名>/`
- `develop`: `https://<ユーザー名>.github.io/<リポジトリ名>/develop/`

## 🌱 おわりに
本書では、次の内容を扱いました。

- `Next.js`と`Storybook`の環境構築
- 自作のUIコンポーネントを`Storybook`に登録する
- `Storybook`をGitHub Pagesに公開する
- ブランチごとに別のURLで公開する

さらに学ぶ場合は、[Storybook公式チュートリアル](https://storybook.js.org/tutorials/intro-to-storybook/react/ja/get-started/)の続きを参照してください。
