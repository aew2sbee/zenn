---
title: "[Next.js] LinkコンポーネントとuseRouterの違い" # 記事のタイトル
emoji: "⚡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["nextjs", "フロントエンド", "typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

`Next.js`の学習の為に、下記書籍を読みました。
下記書籍の「`Link` コンポーネントと `useRouter` の違い」について情報を整理したかったので、執筆します。

@[card](https://gihyo.jp/book/2024/978-4-297-14061-8)

:::message
本記事は、書籍と同じく **App Router** を前提にしています。
App Router の `useRouter` は `next/navigation` から import します。Pages Router の `next/router` とは使えるメソッドが異なります。
:::

## 🌱 結論

:::message

- `useRouter`は、
  プログラムによるページ遷移に使用する。
  フォームの送信後など、特定の処理の後にページ遷移を行いたい場合に適している

- `Link` コンポーネントは、
  ユーザーがクリックして遷移するリンクを作成するために使用する。
  ヘッダーやメニューなど、アプリケーション内の他のページへのリンクに適している

:::

## 🌱 Link コンポーネントについて

- ユーザーがクリックすることで別のページにナビゲートするための宣言的な方法を提供する（`<a>` 要素として描画される）
- `href` には `/posts/${id}` のような動的な値も渡せる
- 画面に表示されたリンクの遷移先を自動でプリフェッチする（本番ビルドのみ）ため、ページ遷移が高速になる
- クライアントサイドのナビゲーションを実現するため、フルページのリロードを避けられる

## 🌱 useRouter について

- Next.js のルーティング機能にアクセスするためのフックである
- Client Component（ファイルの先頭に `'use client'`）でのみ使用できる
- プログラムによるページ遷移（例えば、フォームの送信後に別のページにリダイレクトする）を行う場合に使用する
- `push`、`replace`、`back`、`refresh`、`prefetch` などのメソッドを使って、ページ遷移を動的に制御できる

:::message
App Router の `useRouter` では、現在の URL の情報は取得できません。
パスは `usePathname`、クエリパラメータは `useSearchParams`、動的ルートのパラメータは `useParams` で取得します。
（Pages Router の `next/router` の `useRouter` では、`router.query` や `router.pathname` で取得できます）
:::
