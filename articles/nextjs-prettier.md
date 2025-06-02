---
title: '[Next.js] Prettierの導入' # 記事のタイトル
emoji: '⚡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['nextjs', 'prettier', 'typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**Prettier の導入方法**をまとめております。

:::details 参考資料
@[card](https://gihyo.jp/book/2022/978-4-297-12916-3)
:::

## 1. Prettier をインストール

**Prettier（プリティア）とは**

> コードのフォーマット（整形）を自動的に行ってくれるツールです。
> 下記コマンドでインストールする

```bash
npm install prettier --save-dev
```

## 2. .prettierrc の作成

プロジェクトのディレクトリー配下に下記コマンドで作成

```bash
touch .prettierrc
```

## 3. .prettierrc の編集

```json:.prettierrc
{
  "semi": false,  // コードの末尾にセミコロンを入れるか
  "trailingComma": "none",  // オブジェクト定義の最後のカンマを無しにするか
  "singleQuote": true,  // 文字列の定義などクオートにシングルクオートを使用するか
  "printWidth": 120 // 行を改行する際の文字数
}
```

## 4. package.json を更新する

```json:package.json
{
"scripts": {
  ...
  "prettier": "prettier --config .prettierrc --write .",
  }
}
```

## 5. Prettier を適応させる

下記コマンドで src 配下のコードに適応する事が出来る

```bash
npm run prettier-format
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
