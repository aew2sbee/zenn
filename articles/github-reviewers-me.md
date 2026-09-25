---
title: "[GitHub] Reviewersが自分であるPRを表示する" # 記事のタイトル
emoji: "🐙" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["github", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

PR のレビュー依頼をもらっても、あとで確認しようとすると、どの PR だったか見失うことがよくありました。
GitHub Projects のビュー（view）で管理する方法を見つけたので、その方法を解説します。

## 🌱 1. PR を管理するビューを新規作成する

対象の Project を開き、新しいビューを作成します。
自分の場合は、「Reviewer Me」という名前で作成します。

## 🌱 2. ビューのフィルターを設定する

下記のフィルター条件を追加します。

:::message

- `is:pr`: **PR のみを対象とする**
- `is:open`: **open 状態の PR のみを対象とする**
- `reviewers:@me`: **Reviewers フィールドに自分が入っている PR を対象とする**

:::

```text
is:pr is:open reviewers:@me
```

![GitHub Projectsのビューにフィルター条件を設定した画面](/images/articles/github-reviewers-me/reviewers-me.png)

:::message alert
**このビューに表示するには、対象の PR をあらかじめ Project に追加しておく必要があります。**
Project の Workflows にある「Auto-add to project」を使うと、条件に合う PR を自動で追加できます。
:::

:::message
PR 一覧の検索で使う `review-requested:@me` は、Projects のフィルターでは使えません。Projects では `reviewers:@me` を使います。
Project に追加していない PR も含めて確認したい場合は、PR 一覧で `is:open is:pr review-requested:@me` と検索する方法もあります。
:::
