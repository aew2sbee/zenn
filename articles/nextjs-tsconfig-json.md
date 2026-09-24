---
title: "[Next.js] tsconfig.jsonの設定" # 記事のタイトル
emoji: "⚡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["nextjs", "typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**tsconfig.json の設定方法** をまとめています。

@[card](https://gihyo.jp/book/2022/978-4-297-12916-3)

## 🌱 1. tsconfig.json の作成

create-next-app でプロジェクトを作成すると、自動で生成されます。
手動で作成する場合は、下記コマンドを実行します（TypeScript 汎用の既定設定が生成されます）。

```bash
npx tsc --init
```

## 🌱 2. tsconfig.json の更新

下記内容のようにファイルを更新します。

:::message
下記は、参考書籍（2022年）の時点の設定例です。現在の create-next-app が生成する tsconfig.json とは一部が異なります（例: `moduleResolution` が `bundler`、`plugins` や `paths` が追加されている）。
また、TypeScript 6.0 以降では `target: "es5"`・`moduleResolution: "node"`・`baseUrl` が非推奨になっているため、この設定例をそのまま使うと警告やエラーになる可能性があります。
:::

```json:tsconfig.json
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "baseUrl": "src"
  },
  "include": [
    "next-env.d.ts",
    "src/**/*.ts",
    "src/**/*.tsx"
  ],
  "exclude": [
    "node_modules"
  ]
}
```

**tsconfig.json**とは

> TypeScript プロジェクトの設定ファイルであり、TypeScript コンパイラに対してプロジェクトのビルド方法やコンパイルオプションを指示するためのものです

| 項目 | 役割 |
| --- | --- |
| compilerOptions | TypeScript コンパイラの設定が含まれるセクションです。 |
| target | 型チェック時に想定する JavaScript のバージョンを指定します。この場合は ECMAScript 5 (es5) です（Next.js では出力は SWC が担うため、出力には影響しません）。 |
| lib | コンパイル時に使用可能なライブラリのリストです。dom、dom.iterable、esnext が含まれています。 |
| allowJs | JavaScript ファイルもコンパイルの対象とするかどうかを指定します。 |
| skipLibCheck | 宣言ファイル（`*.d.ts`）の型チェックをスキップするかどうかを指定します。 |
| strict | 厳格な型チェックを有効にします。 |
| forceConsistentCasingInFileNames | import のパスとファイル名で、大文字・小文字の表記が一致していることを強制するかどうかを指定します。 |
| noEmit | 実際に JavaScript ファイルを生成しないようにします。 |
| esModuleInterop | CommonJS モジュールと ES6 モジュールの相互運用性を改善するための設定です。 |
| module | モジュールのコード生成方式を指定します。ここでは最新の ES モジュール構文（esnext）を指定しています。 |
| moduleResolution | モジュール解決の方法を指定します。ここでは Node.js の方式を使用しています。 |
| resolveJsonModule | JSON ファイルを import 文で読み込むことを可能にするかどうかを指定します。 |
| isolatedModules | ファイル単位でトランスパイルするツール（Next.js では SWC）で安全に変換できない書き方をエラーにします。 |
| jsx | JSX ファイルの扱いを指定します。ここでは "preserve" としています。 |
| incremental | 増分ビルドを有効にするかどうかを指定します。 |
| baseUrl | 相対パス以外でモジュールを解決するときの基準ディレクトリを指定します。ここでは `src` を指定しています。 |
| include | コンパイルの対象となるファイルやディレクトリのリストです。`next-env.d.ts`、`src/**/*.ts`、`src/**/*.tsx` が含まれています。 |
| exclude | コンパイルから除外するファイルやディレクトリのリストです。ここでは "node_modules" が除外されています。 |
