---
title: "[GitHub Actions] Pull Request時に自動テスト/Prettier/ESLintを実行する" # 記事のタイトル
emoji: "🐙" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["githubactions", "github", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、Pull Request を作成したときに、GitHub Actions で**自動テスト/Prettier/ESLint を実行する方法**を解説します。

:::message
**前提条件**

- GitHub で管理している Node.js のプロジェクトであること
- `package-lock.json` をリポジトリにコミットしていること（`npm ci` と npm のキャッシュに必要）
- `package.json` の `scripts` に、自動テスト/Prettier/ESLint のコマンドが定義されていること

この記事は 2023 年に執筆し、2026 年にアクションと Node.js のバージョンを更新しています。
:::

## 🌱 1. `.github/workflows` 配下に `code_check.yaml` を作成する

リポジトリのルート直下に `.github/workflows` ディレクトリを作成し、その中に `code_check.yaml` を作成します（拡張子は `.yml` でも構いません）。
先頭のドット（`.`）を付け忘れると、GitHub Actions がワークフローを認識しないので注意してください。

```bash
mkdir -p .github/workflows
touch .github/workflows/code_check.yaml
```

## 🌱 2. `code_check.yaml` を記述する

以下の 3 つをチェックするワークフローを記述します。

- 自動テスト
- Prettier
- ESLint

```yaml:.github/workflows/code_check.yaml
# Actionの名前
name: Code Check

on:
  # Pull Request時
  pull_request:
    # PRのマージ先（ベースブランチ）が main の場合に実行
    branches: ["main"]

# GITHUB_TOKEN の権限を読み取りのみに制限
permissions:
  contents: read

jobs:
  Code-Check:
    runs-on: ubuntu-latest
    steps:
      # GitHubリポジトリのコードをチェックアウト
      - uses: actions/checkout@v6
      # Node.js をセットアップ
      - name: Use Node.js v24
        uses: actions/setup-node@v7
        with:
          node-version: 24
          # npmのダウンロードキャッシュ（~/.npm）を再利用して、依存関係のインストールを高速化
          cache: "npm"
      # プロジェクトの依存関係をインストール
      - run: npm ci
      # Prettierでコードのフォーマットをチェック
      - run: npm run format:check
      # ESLintでコードをチェック
      - run: npm run lint
      # 自動テストを実行し、カバレッジを出力
      - run: npm run test:cov
```

`package.json` の `scripts` は、例えば下記のようになります（Jest を使う場合）。

```json:package.json
{
  "scripts": {
    "format:check": "prettier --check .",
    "lint": "eslint .",
    "test:cov": "jest --coverage"
  }
}
```

:::message alert
**自動テスト/Prettier/ESLint** のコマンド名はプロジェクトごとに異なります。`package.json` の `scripts` で確認してください。

- CI では、Prettier は `--write`（上書き）ではなく `--check` で実行します。`--write` だと、フォーマット違反があってもジョブが成功してしまいます。
- カバレッジが低いときに CI を失敗させたい場合は、Jest の `coverageThreshold` などでしきい値を設定します。
:::

:::message
`branches` には、PR のマージ先ブランチを指定します。すべての PR で実行したい場合は、`branches` を省略します。
:::

## 🌱 3. 開発ブランチに取り込む

`code_check.yaml` をコミットして push し、開発ブランチに取り込みます。

```bash
git add .github/workflows/code_check.yaml
git commit -m "ci: PR時にコードチェックを実行する"
git push
```

以降、`main` 宛てに PR を作成・更新すると、GitHub Actions でコードチェックが自動実行されます。
結果は、PR 画面の Checks 欄や、リポジトリの Actions タブで確認できます。
