---
title: '[Next.js] page.tsxについて' # 記事のタイトル
emoji: '⚡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['nextjs', 'フロントエンド', 'typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

`Next.js`の学習の為に、下記書籍を読みました。
下記書籍の`ルートディレクトリ`について学びがあったので、執筆します。
@[card](https://gihyo.jp/book/2024/978-4-297-14061-8)

## Next.js のページ構造について

Next.js プロジェクトのルートディレクトリまたは`src`ディレクトリ内には`pages`というフォルダがあります。
このフォルダ内に配置されたファイルは、自動的に URL ルートにマッピングされます。
例えば、`pages/about.tsx` ファイルは `/about` というルートに対応します。

Next.js 13 からは、アプリディレクトリ（app ディレクトリ）を使ってページやレイアウトを構築する新しいアプローチが導入されました。
これにより、より柔軟で効率的なページ管理が可能になります。
ここで、`app/sample/page.tsx` ファイルは `/sample` というルートに対応します。

## やり方

### 1. page.tsx の作成

下記コマンドを実行し、プロジェクト作成する

```bash
mkdir -p src/app/sample
touch src/app/sample/page.tsx
```

実行結果は、下記の通りです

```bash
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

下記のように page.tsx を編集する

```tsx:src/app/sample/page.tsx
export default function Page() {
  return (
    <div>こちらは、sampleディレクトリーに配置したpage.tsxです。</div>
  )
}
```

### 3. 動作確認

下記コマンドを実行し、プロジェクトを起動する

```bash
npm run dev
```

実行結果は、下記の通りです
`http://localhost:3000/sample` にアクセスして確認する
![next-js-page](/images/articles/nextjs-page-tsx/next-js-page.png)

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
