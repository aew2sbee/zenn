---
title: "[Next.js] page.tsxについて" # 記事のタイトル
emoji: "⚡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["nextjs", "フロントエンド", "typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

`Next.js`の学習の為に、下記書籍を読みました。
本書で Next.js のルーティング（`page.tsx`）について学びがあったので、執筆します。

@[card](https://gihyo.jp/book/2024/978-4-297-14061-8)

## 🌱 Next.js のページ構造について

Next.js には、Pages Router と App Router の2種類のルーティング方式があります。

Pages Router では、ルートディレクトリまたは`src`ディレクトリ内の`pages`フォルダにページを配置します。
このフォルダ内に配置されたファイルは、自動的に URL ルートにマッピングされます。
例えば、`pages/about.tsx` ファイルは `/about` というルートに対応します。

Next.js 13 からは、アプリディレクトリ（app ディレクトリ）を使ってページやレイアウトを構築する App Router が導入されました（13.4 で安定版）。
App Router では、フォルダが URL のパスになり、その中の`page.tsx`がページとして公開されます。
例えば、`app/sample/page.tsx` ファイルは `/sample` というルートに対応します。
現在の`create-next-app`では、App Router が既定です。

## 🌱 やり方

:::message
`npx create-next-app@latest` で、App Router と `src` ディレクトリを有効にしてプロジェクトを作成済みである前提です。
:::

### 1. page.tsx の作成

下記コマンドを実行し、`sample`ディレクトリと`page.tsx`を作成します。

```bash
mkdir -p src/app/sample
touch src/app/sample/page.tsx
```

実行結果は、下記の通りです。
（`sample`以外のファイルは、プロジェクト作成時の選択によって異なります）

```text
src
└── app
    ├── sample
    │   └── page.tsx
    ├── favicon.ico
    ├── globals.css
    ├── layout.tsx
    ├── page.module.css
    └── page.tsx
```

### 2. page.tsx のコーディング

下記のように`page.tsx`を編集します。

```tsx:src/app/sample/page.tsx
export default function Page() {
  return (
    <div>こちらは、sampleディレクトリーに配置したpage.tsxです。</div>
  )
}
```

### 3. 動作確認

下記コマンドを実行し、プロジェクトを起動します。

```bash
npm run dev
```

`http://localhost:3000/sample` にアクセスすると、下記のように表示されます。

![next-js-page](/images/articles/nextjs-page-tsx/next-js-page.png)
