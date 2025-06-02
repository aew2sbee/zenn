---
title: '[Next.js] tsconfig.jsonの設定' # 記事のタイトル
emoji: '⚡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['nextjs', 'typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**tsconfig.json の設定方法** をまとめております。

:::details 参考資料
@[card](https://gihyo.jp/book/2022/978-4-297-12916-3)
:::

## 1. tsconfig.json の作成

下記コマンドでデフォルトで作成する事が出来ますが、PJ 作成時に作成されます。

```bash
tsc --init
```

## 2. tsconfig.json の更新

下記内容ようにファイルを更新します。

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

> TypeScript プロジェクトの設定ファイルであり、 TypeScript コンパイラに対してプロジェクトのビルド方法やコンパイルオプションを指示するためのものです

| 項目                             | 役割                                                                                                              |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| tacompilerOptionsget             | TypeScript コンパイラの設定が含まれるセクションです。                                                             |
| target                           | コンパイルされる JavaScript のバージョンを指定します。この場合は ECMAScript 5 (es5)                               |
| lib                              | コンパイル時に使用可能なライブラリのリストです。dom、dom.iterable、esnext が含まれています。                      |
| allowJs                          | JavaScript ファイルもコンパイルの対象とするかどうかを指定します。                                                 |
| skipLibCheck                     | ライブラリの型チェックをスキップするかどうかを指定します。                                                        |
| strict                           | 厳格な型チェックを有効にします。                                                                                  |
| forceConsistentCasingInFileNames | ファイル名の一貫性を強制するかどうかを指定します。                                                                |
| noEmit                           | 実際に JavaScript ファイルを生成しないようにします。                                                              |
| esModuleInterop                  | CommonJS モジュールと ES6 モジュールの相互運用性を改善するための設定です。                                        |
| module                           | モジュールのコード生成方式を指定します。ここでは ES6 モジュールを指定しています。                                 |
| moduleResolution                 | モジュール解決の方法を指定します。ここでは Node.js の方式を使用しています。                                       |
| resolveJsonModule                | JSON ファイルを import 文で読み込むことを可能にするかどうかを指定します。                                         |
| isolatedModules                  | ファイルごとに個別の型チェックとコンパイルを行うかどうかを指定します。                                            |
| jsx                              | JSX ファイルの扱いを指定します。ここでは "preserve" としています。                                                |
| incremental                      | 増分ビルドを有効にするかどうかを指定します。                                                                      |
| baseUrl                          | 増分ビルドを有効にするかどうかを指定します。                                                                      |
| include                          | コンパイルの対象となるファイルやディレクトリのリストです。next-env.d.ts、src//_.ts、src//_.tsx が含まれています。 |
| exclude                          | コンパイルから除外するファイルやディレクトリのリストです。ここでは "node_modules" が除外されています。            |
|                                  |

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
