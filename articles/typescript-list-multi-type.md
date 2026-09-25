---
title: "[TypeScript] 合併型の配列（複数の型が含まれている配列）の型定義" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

今回は下記の書籍で TypeScript について学習しました。
この記事では、合併型の配列（複数の型が含まれている配列）の型定義の方法を解説します。

@[card](https://www.oreilly.co.jp/books/9784814400362/)

## 🌱 結論

:::message
文字列と数値が混ざった配列の場合は、`(number | string)[]`を指定します。

※ 括弧を付けずに`number | string[]`と書くと、「数値」または「文字列の配列」という別の意味になります。
※ `Array<number | string>`と書くこともできます。
:::

## 🌱 1. 文字列のみの配列

```ts
const stgList: string[] = ["1", "2", "3", "4", "5"];

console.log(stgList);
```

:::details 実行結果を確認する

```text
[ '1', '2', '3', '4', '5' ]
```

:::

## 🌱 2. 数値のみの配列

```ts
const numList: number[] = [1, 2, 3, 4, 5];

console.log(numList);
```

:::details 実行結果を確認する

```text
[ 1, 2, 3, 4, 5 ]
```

:::

## 🌱 3. 数値のみの配列に文字列が含まれている

```ts
const numList: number[] = [1, 2, 3, "4", "5"];

console.log(numList);
```

:::details コンパイル結果を確認する

```text
Type 'string' is not assignable to type 'number'.
```

:::

## 🌱 4. 合併型（文字列/数値）の配列

```ts
const numOrStgList: (number | string)[] = [1, 2, 3, "4", "5"];

console.log(numOrStgList);
```

:::details 実行結果を確認する

```text
[ 1, 2, 3, '4', '5' ]
```

:::

## 🌱 5. 数値のみの多次元配列

```ts
const numMultiList: number[][] = [
  [1, 2, 3, 4, 5],
  [6, 7, 8, 9, 10],
];

console.log(numMultiList);
```

:::details 実行結果を確認する

```text
[ [ 1, 2, 3, 4, 5 ], [ 6, 7, 8, 9, 10 ] ]
```

:::

## 🌱 6. 合併型（文字列/数値）の多次元配列

```ts
const numOrStgListMulti: (number | string)[][] = [
  ["1", "2", "3", "4", "5"],
  [6, 7, 8, 9, 10],
];

console.log(numOrStgListMulti);
```

:::details 実行結果を確認する

```text
[ [ '1', '2', '3', '4', '5' ], [ 6, 7, 8, 9, 10 ] ]
```

:::

`(number | string)[][]`は、内側の配列の中でも数値と文字列を混在できる型です。内側の配列ごとに型をそろえたい場合は、`(number[] | string[])[]`と書きます。
