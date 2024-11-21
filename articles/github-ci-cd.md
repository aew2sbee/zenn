---
title: "[GitHub Actions] Pull Request時に自動テスト/prettier/ESLintを実行する" # 記事のタイトル
emoji: "🐙‍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["githubactions", "github", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---


## はじめに
この記事では、GitHub Actionsで**自動テスト/prettier/ESLintを実行する方法**を解説します。


## 1. github/workflows配下にcode_check.yamlを作成する
```bash
1. github/workflows配下にcode_check.yamlを作成する
```
## 2. code_check.yamlを記述する
- 自動テスト
- prettier
- ESLint

```yaml
# Actionの名前
name: Code Check

on:
  # Pull Request時
  pull_request:
    # 対象のブランチを指定
    branches: ['test_CI/CD']

jobs:
  Code-Check:
    runs-on: ubuntu-latest
    steps:
      # GitHubリポジトリのコードをチェックアウト
      - uses: actions/checkout@v3
      # Node.js v18をセットアップ
      - name: Use Node.js v18
        # npmのキャッシュを有効
        # 以前にインストールしたnpmパッケージを再利用でき、ビルド時間を短縮
        uses: actions/setup-node@v3
        with:
          node-version: 18
          cache: 'npm'
      # プロジェクトの依存関係をインストール
      - run: npm ci
      # prettierでコードのフォーマットをチェック
      - run: npm run prettier-format
      # ESLintでコードをチェック
      - run: npm run eslint
      # 自動テストの結果/カバレッジをチェック
      - run: npm run test:cov test/
```
:::message alert
※ **自動テスト/prettier/ESLint**の各コマンドは、所属プロジェクトの`package.json`の`scripts`を確認してください
:::

## 3. 開発ブランチに取り込み
今後のPR(Pull Request)を出されたときにGitHub Actionsが実行する
