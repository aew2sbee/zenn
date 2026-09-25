---
title: "[TypeScript] 数値型配列の合計値を求めるreduceメソッド" # 記事のタイトル
emoji: "🛡️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

下記書籍で初めて`TypeScript`を学習し、その中で`reduce`メソッドについて学んだ内容を、合計値の例でまとめます。
`reduce`は JavaScript の配列のメソッドで、TypeScript でもそのまま使えます。

@[card](https://oukayuka.booth.pm/items/2368045)

## 🌱 結論

:::message
配列の要素を先頭から順に処理し、結果を 1 つの値にまとめる（畳み込む）メソッド

```ts
// 構文のイメージ（list は配列）
const result = list.reduce((累積値, 現在の要素) => 処理, 初期値);
```

- **累積値**: 前回までの処理結果。1 回目は初期値が入る
- **現在の要素**: 配列から順に取り出した要素
- 処理（コールバック関数）の戻り値が、次の呼び出しの累積値になる

:::

@[card](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

## 🌱 1. reduce()で合計値を求めるサンプルコード

```ts
// 数値の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の合計値を求める（0 は初期値）
const result = numbers.reduce((acc, value) => acc + value, 0);

console.log(result);
```

実行すると、次のように出力されます。

```text
15
```

### 処理の流れ

`acc`は accumulator（累積値）の略で、`value`は現在の要素です。

|回数|acc（累積値）|value（現在の要素）|戻り値|
|---|---|---|---|
|1|0（初期値）|1|1|
|2|1|2|3|
|3|3|3|6|
|4|6|4|10|
|5|10|5|15|

### 初期値を省略した場合

初期値を省略すると、配列の最初の要素が初期値になり、2 番目の要素から処理します。
ただし、空配列に対して初期値を省略すると`TypeError`になるため、合計値を求める場合は初期値`0`を指定するのが安全です。

## 🌱 2. for ループとの比較

同じ合計値を`for`ループで書くと、次のようになります。
合計値のような単純な集計では、`reduce`を使うと 1 行で書けます。

```ts
const numbers = [1, 2, 3, 4, 5];

let total = 0;
for (const value of numbers) {
  total += value;
}

console.log(total); // 15
```

## 🌱 まとめ

- `reduce`は、配列の要素を順に処理して 1 つの値にまとめるメソッド
- コールバック関数の第 1 引数は累積値、第 2 引数は現在の要素
- 空配列でエラーにならないよう、初期値を指定するのが安全
- 合計値以外に、最大値の取得（`numbers.reduce((acc, value) => Math.max(acc, value))`）などにも使える
