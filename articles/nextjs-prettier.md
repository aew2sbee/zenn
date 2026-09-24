---
title: "[Next.js] Prettierの導入" # 記事のタイトル
emoji: "⚡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["nextjs", "prettier", "typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**Prettier の導入方法**をまとめています。

:::details 参考資料
@[card](https://gihyo.jp/book/2022/978-4-297-12916-3)
:::

## 🌱 1. Prettier のインストール

**Prettier（プリティア）とは**

> コードのフォーマット（整形）を自動的に行ってくれるツールです。

下記コマンドでインストールします。

```bash
npm install prettier --save-dev
```

## 🌱 2. .prettierrc の作成

プロジェクトのルートディレクトリで、下記コマンドを実行して作成します。

```bash
touch .prettierrc
```

## 🌱 3. .prettierrc の編集

```json:.prettierrc
{
  "semi": false,
  "trailingComma": "none",
  "singleQuote": true,
  "printWidth": 120
}
```

各設定の意味は、下記の通りです。

| 設定 | 意味 |
| --- | --- |
| semi | コードの末尾にセミコロンを入れるか |
| trailingComma | オブジェクトや配列、引数などの最後のカンマを付けるか（`none` は付けない） |
| singleQuote | 文字列の定義などのクオートにシングルクオートを使用するか |
| printWidth | 行を改行する際の文字数 |

:::message alert
JSON にはコメントを書けません。`.prettierrc` に `//` のコメントを書くと、設定が正しく読み込まれません。
:::

## 🌱 4. package.json の更新

`scripts` に下記の1行を追加します（既存のスクリプトはそのまま残します）。

```json:package.json
{
  "scripts": {
    "prettier-format": "prettier --config .prettierrc --write src"
  }
}
```

## 🌱 5. Prettier の適用

下記コマンドで src 配下のコードに適用できます。

```bash
npm run prettier-format
```
