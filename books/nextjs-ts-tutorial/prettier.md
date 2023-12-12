---
title: '[事前準備] Prettierの導入'
---

## Prettier（プリティア）とは
> コードのフォーマット（整形）を自動的に行ってくれるツールです。

## Prettierをインストール
下記コマンドでインストールする事が出来ます。
````bash
npm install prettier --save-dev
````

## .prettierrcの作成
1. プロジェクトのディレクトリー配下に`.prettierrc`を作成する
2. `.prettierrc`の中身を下記にコードにする
````json: .prettierrc
{
  "semi": false,  // コードの末尾にセミコロンを入れるか
  "trailingComma": "none",  // オブジェクト定義の最後のカンマを無しにするか
  "singleQuote": true,  // 文字列の定義などクオートにシングルクオートを使用するか
  "printWidth": 120 // 行を改行する際の文字数
}
````
## package.jsonを更新する
````json: package.json
{
"scripts": {
  ...
  "prettier-format": "prettier --config .prettierrc 'src/**/*.ts'  --write",
  }
}
````

## Prettierを適応させる
下記コマンドで`src`配下のコードに適応する事が出来ます。
````bash
npm run prettier-format
````