---
title: '[GitHub] fork元のレポジトリーをupstreamに設定する' # 記事のタイトル
emoji: '🐙‍' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['git', 'github', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**fork 元のレポジトリーを upstream に設定する方法**を解説します。

## 結論

:::message
下記のコマンドを実行したら、設定できます

```bash
git remote add upstream https://github.com/<Fork元のユーザー名>/<リポジトリ名>.git
```

:::

## 1. 状況を確認する

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

## 2. upstream に設定する

下記コマンドで upstream に設定する

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

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
