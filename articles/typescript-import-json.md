---
title: "[TypeScript] JSONファイルを直接インポートする" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "json", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**JSON ファイルを直接インポートする方法** をまとめています。

:::details 参考資料
@[card](https://www.oreilly.co.jp/books/9784814400362/)
:::

## 🌱 結論

:::message
tsconfig.json に **"resolveJsonModule": true** を追加する

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "esModuleInterop": true,
    "resolveJsonModule": true
  }
}
```

:::

## 🌱 0. 前提条件：ディレクトリ構成

```text
.
├── data.json
├── resolveJsonModule.ts
└── tsconfig.json
```

## 🌱 1. tsconfig.json の設定

`"resolveJsonModule": true`を追加する

```json:tsconfig.json
{
  "compilerOptions": {
    "module": "commonjs",
    "esModuleInterop": true,
    "resolveJsonModule": true
  }
}
```

## 🌱 2. 読み取る対象の JSON ファイルの作成

```json:data.json
{
  "func_name": "get_users",
  "timestamp": "2019-01-01 12:00:00",
  "status": 200,
  "data": [
    {
      "id": 1,
      "name": "Michael"
    },
    {
      "id": 2,
      "name": "John"
    }
  ]
}
```

:::message
`import data from "./data.json";`のようにデフォルトインポートするには、`esModuleInterop`の設定が必要です（上記の tsconfig.json に含めています）。
`import * as data from "./data.json";`で読み込むと、`console.log(data)`の出力に`default`というプロパティが追加されます。
:::

:::message alert
実行結果は、TypeScript 5.9 と ts-node 10.9 で確認しています。ts-node は TypeScript 7 では動作しないため、TypeScript 7 を使う場合は tsx などのツールで実行してください。
:::

## 🌱 3-1. JSON ファイル全体を出力する

```ts:resolveJsonModule.ts
import data from "./data.json";

console.log(data);
```

:::details 実行結果を確認する

```text
$ npx ts-node resolveJsonModule.ts
{
  func_name: 'get_users',
  timestamp: '2019-01-01 12:00:00',
  status: 200,
  data: [ { id: 1, name: 'Michael' }, { id: 2, name: 'John' } ]
}
```

:::

## 🌱 3-2. JSON ファイルのタイムスタンプを出力する

```ts:resolveJsonModule.ts
import data from "./data.json";

console.log(data.timestamp);
```

:::details 実行結果を確認する

```text
$ npx ts-node resolveJsonModule.ts
2019-01-01 12:00:00
```

:::

## 🌱 3-3. JSON ファイルの data の配列を出力する

```ts:resolveJsonModule.ts
import data from "./data.json";

console.log(data.data);
```

:::details 実行結果を確認する

```text
$ npx ts-node resolveJsonModule.ts
[ { id: 1, name: 'Michael' }, { id: 2, name: 'John' } ]
```

:::

## 🌱 3-4. JSON ファイルの data の配列の 0 番目の name を出力する

```ts:resolveJsonModule.ts
import data from "./data.json";

console.log(data.data[0].name);
```

:::details 実行結果を確認する

```text
$ npx ts-node resolveJsonModule.ts
Michael
```

:::
