---
title: '[TypeScript] 合併型の配列(複数の型が含まれているを配列)の型定義' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

今回は下記書籍で`TypeScript`について学習しました。
下記内容まで解説します。

- 合併型の配列(複数の型が含まれているを配列)の型定義方法について

@[card](https://www.oreilly.co.jp/books/9784814400362/)

### 結論

:::message
文字列と数値の混合の配列のあ場合、`(number | string)[]`を指定する
:::

## 1. 文字列のみの配列

```ts
const stgList: string[] = ['1', '2', '3', '4', '5'];

console.log(stgList);
```

実行結果を確認する

```bash
["1", "2", "3", "4", "5"]
```

### 2. 数値のみの配列

```ts
const numList: number[] = [1, 2, 3, 4, 5];

console.log(numList);
```

実行結果を確認する

```bash
[1, 2, 3, 4, 5]
```

### 3. 数値のみの配列に文字列が含まれている

```ts
const numList: number[] = [1, 2, 3, '4', '5'];

console.log(numList);
```

実行結果を確認する

```bash
Type 'string' is not assignable to type 'number'.
```

### 4. 合併型(文字列/数値)の配列

```ts
const numOrStgList: (number | string)[] = [1, 2, 3, '4', '5'];

console.log(numOrStgList);
```

実行結果を確認する

```bash
[1, 2, 3, "4", "5"]
```

### 5. 数値のみの多次元配列

```ts
const numMultiList: number[][] = [
  [1, 2, 3, 4, 5],
  [6, 7, 8, 9, 10],
];

console.log(numMultiList);
```

実行結果を確認する

```bash
[[1, 2, 3, 4, 5], [6, 7, 8, 9, 10]]
```

### 6. 合併型(文字列/数値)の多次元配列

```ts
const numOrStgListMulti: (number | string)[][] = [
  ['1', '2', '3', '4', '5'],
  [6, 7, 8, 9, 10],
];

console.log(numMultiList);
```

実行結果を確認する

```bash
[["1", "2", "3", "4", "5"], [6, 7, 8, 9, 10]]
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
