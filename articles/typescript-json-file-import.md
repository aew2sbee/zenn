---
title: '[TypeScript] Jsonファイルを直接インポートする' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

`TypeScript`をより深く理解したく下記書籍を読みました。
下記書籍の`Jsonファイルを直接インポートする方法`について学びがあったので、記事として記録します。
@[card](https://www.oreilly.co.jp/books/9784873119045/)

## 結論

:::message
tsconfig.json の`"resolveJsonModule": true,`を追加する

```json:tsconfig.json
{
  "compilerOptions": {
    "resolveJsonModule": true,
    // 他のオプション...
  }
}
```

:::

## 準備

### 1. ディレクトリー構成の確認

```bash
.
├── data.json
├── resolveJsonModule.ts
└── tsconfig.json
```

### 2. 読み取る対象の Json ファイルの確認

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

### 3.Json ファイルを読み取る ts ファイルの編集

```ts:resolveJsonModule.ts
import * as data from './data.json';
```

### 4.tsconfig.json の編集

```json:tsconfig.json
{
  "compilerOptions": {
    "resolveJsonModule": true,
    // 他のオプション...
  }
}
```

## 出力確認

### 1. Json ファイルの全てを出力する

```ts:resolveJsonModule.ts
import * as data from './data.json';

console.log(data);
```

実行結果を確認する

```bash
$ ts-node resolveJsonModule.ts
{
  func_name: 'get_users',
  timestamp: '2019-01-01 12:00:00',
  status: 200,
  data: [ { id: 1, name: 'Michael' }, { id: 2, name: 'John' } ]
}
```

### 2. Json ファイルのタイムスタンプを出力する

```ts:resolveJsonModule.ts
import * as data from './data.json';

console.log(data.timestamp);
```

実行結果を確認する

```bash
$ ts-node resolveJsonModule.ts
2019-01-01 12:00:00
```

### 3. Json ファイルの data の配列を出力する

```ts:resolveJsonModule.ts
import * as data from './data.json';

console.log(data.data);
```

実行結果を確認する

```bash
$ ts-node resolveJsonModule.ts
[ { id: 1, name: 'Michael' }, { id: 2, name: 'John' } ]
```

### 4. Json ファイルの data の配列の 0 番目を出力する

```ts:resolveJsonModule.ts
import * as data from './data.json';

console.log(data.data[0].name);
```

実行結果を確認する

```bash
$ ts-node resolveJsonModule.ts
Michael
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
