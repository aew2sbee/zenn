---
title: "[GitHub] Reviewersが自分であるPRを表示する" # 記事のタイトル
emoji: "🐙‍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["github", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

PR 依頼の連絡をもらったけど、あとで確認する場合
どの PR なのかを見失う事がよくあったので、
`projects view`で管理する方法を見つけたので、その方法を解説します。

## 1. PR を管理する view を新規作成

自分の場合は、「Reviewer Me」という名前で作成します

## 2. view の filter を設定する

下記の filter 条件を追加します

:::message

- is:pr: **PR のみを対象とする**
- is:open: **PR が open のみを対象とする**
- reviewers:@me: **reviewers が自分を含む PR を対象とする**
  :::

```md
is:pr is:open 　 reviewers:@me
```

![reviewers-me](/images/articles/github-reviewers-me/reviewers-me.png)

:::message alert
**その PR を project に追加しておく必要があります。**
:::
