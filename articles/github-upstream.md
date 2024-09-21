---
title: "[GitHub] fork元のレポジトリーをupstreamに設定する" # 記事のタイトル
emoji: "🌾" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["git", "github", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---


## はじめに

## 結論
:::message
下記のコマンドを実行したら、設定できます
```bash
git remote add upstream https://github.com/<Fork元のユーザー名>/<リポジトリ名>.git
```
:::

## やり方
### 1. 状況を確認する
下記コマンドで状況を確認します
```bash
git remote -v
```

:::details 出力結果を確認する
```bash
$ git remote -v
origin  https://github.com/user-name/sample-project.git (fetch)
origin  https://github.com/user-name/sample-project.git (push)
```
:::

### 2. upstreamに設定する
下記コマンドでupstreamに設定する
```bash
git remote add upstream https://github.com/company-name/sample-project.git
```
再び、下記コマンドで状況を確認する
```bash
git remote -v
```
:::details 出力結果を確認する
```bash
$ git remote -v
origin  https://github.com/user-name/sample-project.git (fetch)
origin  https://github.com/user-name/sample-project.git (push)

upstream  https://github.com/company-name/sample-project.git (fetch)
upstream  https://github.com/company-name/sample-project.git (push)
```
:::
