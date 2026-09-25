---
title: "[TypeScript] それぞれで型定義ではなく、呼び出しシグネチャを使う" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

`TypeScript`をより深く理解したく、下記の書籍（『初めてのTypeScript』）を読みました。
`それぞれで型定義ではなく、呼び出しシグネチャを使う`について情報を整理したかったので、執筆します。
@[card](https://www.oreilly.co.jp/books/9784814400362/)

## 🌱 結論

:::message

```ts
interface 型名 {
  (引数名1: データ型, 引数名2: データ型): データ型;
}
```

:::

## 🌱 1. 型を使わない

```ts
const add = (x, y) => x + y;
```

:::message alert
`tsconfig.json`で`strict`（`noImplicitAny`）を有効にしている場合は、`Parameter 'x' implicitly has an 'any' type.`というエラーになります。
:::

## 🌱 2. 引数と戻り値のそれぞれに型定義

:::message alert
型定義により安全性が向上しましたが、
同じ形の関数が複数ある場合、関数ごとに「引数」「戻り値」の型定義を書くのは手間であり、
可読性も下がる
:::

```ts
const add = (x: number, y: number): number => x + y;
```

## 🌱 3. 呼び出しシグネチャで型定義

:::message
型定義を一か所にまとめられるため、同じ形の関数に使い回せて手間が少ない
可読性も向上する
:::

```ts
interface CallSignature {
  (x: number, y: number): number;
}

const add: CallSignature = (x, y) => x + y;
console.log(add(2, 5));
```

:::details 実行結果を確認する

```text
7
```

:::

:::message
同じ関数の型は、型エイリアスと関数型の式を使って`type CallSignature = (x: number, y: number) => number;`のように短く書くこともできます（公式ドキュメントでは、この形を「Function Type Expressions」と呼び、呼び出しシグネチャとは区別しています）。

@[card](https://www.typescriptlang.org/docs/handbook/2/functions.html#call-signatures)
:::
