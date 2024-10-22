---
title: '[TypeScript] Jsonファイルを直接インポートする' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', 'json', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**Jsonファイルを直接インポートする方法** をまとめております。
:::details 参考資料
@[card](https://www.oreilly.co.jp/books/9784814400362/)
:::

## 結論

:::message
tsconfig.json の **"resolveJsonModule": true,** を追加する

```json
{
  "compilerOptions": {
    "resolveJsonModule": true
    // 他のオプション...
  }
}
```

:::


## 0. 前提条件：ディレクトリー構成

```bash
.
├── data.json
├── resolveJsonModule.ts
└── tsconfig.json

```

## 1. tsconfig.json の設定

`"resolveJsonModule": true,`を追加する

```json:tsconfig.json
{
  "compilerOptions": {
    "resolveJsonModule": true,
    // 他のオプション...
  }
}

```

## 2. 読み取る対象の Json ファイルの作成

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

## 3-1. Json ファイルの全てを出力する

```ts:resolveJsonModule.ts
import * as data from './data.json';

console.log(data);

```
:::details 実行結果を確認する
```bash
$ ts-node resolveJsonModule.ts
{
  func_name: 'get_users',
  timestamp: '2019-01-01 12:00:00',
  status: 200,
  data: [ { id: 1, name: 'Michael' }, { id: 2, name: 'John' } ]
}

```
:::

## 3-2. Json ファイルの全てを出力する

```ts:resolveJsonModule.ts
import * as data from './data.json';

console.log(data.timestamp);
```

:::details 実行結果を確認する
```bash
$ ts-node resolveJsonModule.ts
2019-01-01 12:00:00
```
:::

## 3-3. Json ファイルの data の配列を出力する

```ts:resolveJsonModule.ts
import * as data from './data.json';

console.log(data.data);

```

:::details 実行結果を確認する
```bash
$ ts-node resolveJsonModule.ts
[ { id: 1, name: 'Michael' }, { id: 2, name: 'John' } ]
```
:::

## 3-4. Json ファイルの data の配列の 0 番目を出力する

```ts:resolveJsonModule.ts
import * as data from './data.json';

console.log(data.data[0].name);

```
:::details 実行結果を確認する
```bash
$ ts-node resolveJsonModule.ts
Michael
```
:::
