---
title: '[作業] プロジェクト作成'
---

## Next.jsとは
> Reactベースのフレームワークで、Reactを使用してウェブアプリケーションを構築するための便利なツールセットを提供します。


| メリット | 説明 |
| ---- | ---- |
| Reactとの統合 | Next.jsはReactと統合されており、Reactコンポーネントを使用してウェブアプリケーションを構築します。Reactは、ユーザーインターフェースを構築するための強力で人気のあるライブラリです。 |
| サーバーサイドレンダリング (SSR) と静的サイト生成 (SSG) | Next.jsはサーバーサイドレンダリングと静的サイト生成をサポートしています。これにより、ページを事前にサーバーでレンダリングしておくことができ、初回の読み込みが高速になります。 |
| ルーティングの簡素化 | Next.jsはファイルベースのルーティングを提供しており、`pages`フォルダ内にファイルを配置することで、自動的にURLとの対応が行われます。例えば、`pages/about.ts`は`/about`のURLにマッピングされます。 |
|自動的なコード分割 | ページごとに必要なJavaScriptコードが分割され、ページごとに必要な分だけをユーザーに送信することができます。これにより、初回読み込みを高速化し、パフォーマンスを向上させます。 |
| ホットリローディング | 開発中に行われる変更を即座に反映するホットリローディングがサポートされています。これにより、アプリケーションの開発が効率的に行えます。 |
| APIルート | `pages/api` フォルダにAPIエンドポイントを作成することができ、サーバーレス関数として簡単に利用できます。 |
| デプロイの容易性 | VercelやNetlifyなどのプラットフォームで簡単にデプロイできます。また、静的ファイルを生成することで、CDNを使用して高速かつ安定したホスティングが可能です。 |

## プロジェクト作成
下記コマンドでプロジェクト作成する事が出来ます。
※ 意味：`frontend`という名前のプロジェクトを`TypeScript(--ts)`で`最新(@latest)`で作成する
````bash
npx create-next-app@latest --ts frontend
````

### [質問1] ESLintを使用しますか？
`Yes`と回答しましょう
````bash
√ Would you like to use ESLint? ... No / Yes
（ESLintを使用しますか？... いいえ / はい）
````
#### ESLintとは？
> ESLintは、JavaScriptコードの潜在的な問題やスタイルの違反を見つけ、修正するためのツールです。コードの品質を向上させ、共通のコーディングスタイルを確立するのに役立ちます。ESLintはカスタマイズ可能であり、異なるプロジェクトに合わせてルールを設定できます。

### [質問2] Tailwind CSSを使用しますか？
`Yes`と回答しましょう
````bash
√ Would you like to use Tailwind CSS? ... No / Yes
（Tailwind CSSを使用しますか？... いいえ / はい）
````
#### Tailwind CSSとは？
> CSSを効果的かつ迅速に構築するためのユーティリティファーストのCSSフレームワークです。


### [質問3] src/ ディレクトリを使用しますか？
`Yes`と回答しましょう
````bash
√ Would you like to use `src/` directory? ... No / Yes
（src/ ディレクトリを使用しますか？... いいえ / はい）
````

### [質問4] App Routerを使用しますか？（推奨）
`Yes`と回答しましょう
````bash
√ Would you like to use App Router? (recommended) ... No / Yes
（App Routerを使用しますか？（推奨）... いいえ / はい）
````

#### App Routerとは？
> ウェブアプリケーション内でのページの遷移やルーティングを簡単かつ効果的に処理するための機能です

| メリット | 説明 |
| ---- | ---- |
| 自動的なルーティング | App Routerは、`pages`ディレクトリ内のファイル構造に基づいて自動的にルーティングを構築します。たとえば、`pages/about.ts`というファイルがあれば、それに対応するURLは`/about`になります。 |
| 動的ルーティング | ファイル名に`[...]`でくくられた部分を含むファイルを作成することで、動的なルーティングを実現できます。たとえば、`pages/posts/[slug].ts`というファイルを作成すると、`/posts/any-slug`のようなURLに対応することができます。 |
| Linkコンポーネント | `next/link`モジュールを使用した `Link`コンポーネントは、アプリケーション内でのクライアントサイドのナビゲーションを可能にします。通常の `<a>`タグとは異なり、ページ遷移がクライアントサイドで処理され、ページの再読み込みが発生しません。 |

### [質問5] デフォルトのインポートエイリアス（@/）をカスタマイズしますか？
`Yes`と回答しましょう
````bash
√ Would you like to customize the default import alias (@/*)? ... No / Yes
（デフォルトのインポートエイリアス（@/）をカスタマイズしますか？... いいえ / はい）
````
#### import aliasとは？
> プロジェクト内のファイルパスを簡略化する



### [質問6] 設定するインポートエイリアスは何ですか？
`Yes`と回答しましょう
````bash
√ What import alias would you like configured? ... @/*
（設定するインポートエイリアスは何ですか？... @/*）
````
下記サンプルコード
```ts: Header.tsx
import Header from '@/components/Header';
```

## Next.jsの起動
### 起動コマンド
下記コマンドで起動する
````bash
cd frontend
npm run dev
````

````bash
----- 出力結果 -----
$ npm run dev

> frontend@0.1.0 dev
> next dev

   ▲ Next.js 14.0.4
   - Local:        http://localhost:3000

 ✓ Ready in 9.6s
````
### 起動したNext.jsにアクセスする
http://localhost:3000
下記画像のような画面が確認出来ます。
![create-project-step01](/images/books/nextjs-ts-tutorial/create-project/create-project-step01.png)

## Next.jsのファイル構成(初期値)
````bash
.
├── node_modules  # このディレクトリには、プロジェクトで使用されているパッケージやライブラリが格納されています。通常、開発者は手動でこのディレクトリを管理する必要はありません。
├── public  # このディレクトリには、静的なファイル（画像、アイコンなど）が格納されています。
│   ├── next.svg
│   └── vercel.svg
├── src  # このディレクトリには、アプリケーションのソースコードが格納されています。
│   └── app
│       ├── favicon.ico  # ウェブサイトのタブやブックマークなどに表示されるアイコンです。
│       ├── globals.css  # アプリケーション全体で使用されるグローバルなスタイルが定義されています。
│       ├── layout.tsx  # アプリケーションの全体的なレイアウトに関するコードがここに配置されます。
│       └── page.tsx  # アプリケーションのメインとなるページのコードがここに配置されます。
├── .eslintrc.json  # ESLintの設定ファイル。コードの品質やスタイルを検証するためのツールです。
├── .gitignore  # Gitが無視するべきファイルやディレクトリが定義されています。
├── next-env.d.ts  # Next.jsの型定義ファイル。TypeScriptを使用している場合、これは自動生成されるファイルです。
├── next.config.js  # Next.jsの設定ファイル。ビルドや開発時の動作など、プロジェクト全体の設定が記述されています。
├── package-lock.json  # npmパッケージの依存関係の正確なバージョン情報が含まれています。
├── package.json  # プロジェクトの設定、依存関係、スクリプトなどが記述されたnpmパッケージの設定ファイル。
├── postcss.config.js  # PostCSSの設定ファイル。CSSを変換するためのツールです。
├── README.md  # プロジェクトの説明や使用法に関するドキュメントが格納されたMarkdownファイル。
├── tailwind.config.ts  # Tailwind CSSの設定ファイル。Tailwind CSSは、CSSフレームワークであり、クラスベースのスタイルを提供します。
└── tsconfig.json  # TypeScriptの設定ファイル。TypeScriptコンパイラの動作やプロジェクトの型定義などが定義されています。
````

| ファイル名 | 役割 |
| ---- | ---- |
| node_modules | プロジェクトで使用されているパッケージやライブラリが格納されています。通常、開発者は手動でこのディレクトリを管理する必要はありません。 |
| public | 静的なファイル（画像、アイコンなど）が格納されています。 |
| src | アプリケーションのソースコードが格納されています。 |
| .eslintrc.json | ESLintの設定ファイル。コードの品質やスタイルを検証するためのツールです。 |
| .gitignore | Gitが無視するべきファイルやディレクトリが定義されています。 |
| next-env.d.ts | Next.jsの型定義ファイル。TypeScriptを使用している場合、これは自動生成されるファイルです。 |
| next.config.js | Next.jsの設定ファイル。ビルドや開発時の動作など、プロジェクト全体の設定が記述されています。 |
| package-lock.json | npmパッケージの依存関係の正確なバージョン情報が含まれています。 |
| package.json | プロジェクトの設定、依存関係、スクリプトなどが記述されたnpmパッケージの設定ファイル。 |
| postcss.config.js | PostCSSの設定ファイル。CSSを変換するためのツールです。 |
| README.md | プロジェクトの説明や使用法に関するドキュメントが格納されたMarkdownファイル。 |
| tailwind.config.ts | Tailwind CSSの設定ファイル。<br>Tailwind CSSは、CSSフレームワークであり、クラスベースのスタイルを提供します。 |
| tsconfig.json | TypeScriptの設定ファイル。<br>TypeScriptコンパイラの動作やプロジェクトの型定義などが定義されています。 |
