---
title: "[Next.js] プロジェクトの始め方-開発モード起動" # 記事のタイトル
emoji: "⚡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["nextjs", "フロントエンド", "typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

`Next.js`の学習の為に、下記書籍を読みました。
下記書籍の`プロジェクト作成`について情報を整理したかったので、執筆します。
@[card](https://gihyo.jp/book/2024/978-4-297-14061-8)

## やり方

### 1. プロジェクト作成

下記コマンドを実行し、プロジェクト作成する

```bash
npx create-next-app
```

実行結果は、下記の通りです

```bash
$ npx create-next-app
# プロジェクトの名前は何にしますか？
√ What is your project named? ... next-js
# TypeScriptを使いますか？
√ Would you like to use TypeScript? ... No / Yes
# ESLintを使いますか？
√ Would you like to use ESLint? ... No / Yes
# Tailwind CSSを使いますか？
√ Would you like to use Tailwind CSS? ... No / Yes
# src/を使いますか？
√ Would you like to use `src/` directory? ... No / Yes
# App Routerを使いますか？(推奨)
√ Would you like to use App Router? (recommended) ... No / Yes
# デフォルトのimpotr alias をカスタマイズしますか？
√ Would you like to customize the default import alias (@/*)? ... No / Yes
```

---

#### What is your project named? … next-js - プロジェクトの名前は何にしますか？

A. 例えば、`next-js`というプロジェクトを作成したら、下記のようなディレクトリー構成になります。

```bash
next-js
├── .next
├── node_modules
├── public
├── src
│   └── app
└── next-env.d.ts
```

---

#### Would you like to use TypeScript? … No / Yes - TypeScript を使いますか？

TypeScript は、静的解析によってコードバグを防ぐ役割を果たすため、`Yes`と回答します。

---

#### Would you like to use ESLint? … No / Yes - ESLint を使いますか？

ESLint は、コードに規約を設ける役割があります。
例えば

- コード品質を保つ：<img>要素に alt 属性を設定し忘れるなど、セマンティクス観点で望ましくないコードを検知します。
- 潜在的なバグの混入を防ぐ：ライブラリ API を使用している箇所で非推奨な書き方をしているコードを検知します。
- 開発者同士のレビュー指摘を減らす：import 文の並びなど、コーディングガイドラインにまつわる細かい指摘が不要になる

---

#### Would you like to use Tailwind CSS? … No / Yes - Tailwind CSS を使いますか？

Tailwind CSS は、CSS のフレームワークです。目的のプロジェクトに併せて回答してください。

---

#### Would you like to use src/ directory? … No / Yes - src/を使いますか？

`Yes`と回答すると、下記のディレクトリー構成のように`src`というディレクトリーが生成されます。

```bash
next-js
├── src
│   └── app
└── next-env.d.ts
```

---

#### Would you like to use App Router? (recommended) … No / Yes - App Router を使いますか？(推奨)

ルーティング機能について古い機能を使うか？新しい機能(App Router)を使うのか？を訪ねています。推奨事項なので`Yes`と回答します

---

#### Would you like to customize the default import alias (@/\*)? … No / Yes - デフォルトの impotr alias をカスタマイズしますか？

こだわりがない場合は、`No`と回答します。
ちなみに下記のような import 文をパスを省略する事が出来ます。

```ts
// before
import { Button } from "../../../components/button";

// after
import { Button } from "@/components/button";
```

### 2. プロジェクト起動(開発モード)

下記コマンドを実行し、プロジェクトを起動する

```bash
cd next-js/
npm run dev
```

実行結果は、下記の通りです

```bash
$ npm run dev

> next-js@0.1.0 dev
> next dev

  ▲ Next.js 14.2.4
  - Local:        http://localhost:3000

 ✓ Starting...
 ✓ Ready in 6.1s
```

`http://localhost:3000` にアクセスして確認する
![next-js-page](/images/articles/nextjs-project-dev/next-js-local.png)
