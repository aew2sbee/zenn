---
title: "[TypeScript] インターフェース(interface)と型エイリアス(type)の違い" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

`TypeScript`をより深く理解するために、下記の書籍を読みました。
`インターフェース(interface)と型エイリアス(type)の違い`について情報を整理したかったので、執筆します。

@[card](https://www.oreilly.co.jp/books/9784814400362/)

## 🌱 結論

:::message
オブジェクトの型を定義する場合は、下記の理由から、`interface`を使える場面では`interface`の利用を推奨します（書籍の推奨に基づく）。
ユニオン型など、`interface`で表現できない型には型エイリアス(type)を使います。
:::

## 🌱 1. interface 同士で継承できる

- `extends`で継承でき、継承元と型が衝突した場合はエラーで気付けます
- 型エイリアス(type)でも、交差型（`&`）で似たことはできます。ただし、交差型では型が衝突してもその場ではエラーにならず、そのプロパティが`never`型になるため、気付きにくくなります

```ts
interface 継承元 {
  プロパティ名1: データ型;
}

interface 継承先 extends 継承元 {
  プロパティ名2: データ型;
}
```

## 🌱 2. 同じ名前の interface は自動でマージされる（宣言のマージ）

- 同じ名前の`interface`を複数回宣言すると、1 つにまとめられます
- 組み込みのグローバルインターフェースや、npm パッケージなどのサードパーティの型を拡張するときに便利です
- 型エイリアス(type)は、同じ名前で宣言するとエラーになります

```ts
interface User {
  name: string;
}

interface User {
  age: number;
}

// name と age の両方が必要になる
const user: User = { name: "Alice", age: 20 };
```

:::message
クラスの`implements`には、interface も型エイリアス(type)も使えます（型エイリアスは、オブジェクトの形を表す型の場合のみ。ユニオン型は不可）。
:::

## 🌱 3. interface の方が型チェックが速い場合がある

- interface は TypeScript が内部でキャッシュしやすいため、型チェックが高速になることがあります
- 型エイリアス(type)の交差型などは、使われるたびに型を計算し直すことがあるため、interface より遅くなる場合があります
