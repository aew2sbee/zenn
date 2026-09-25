---
title: "[TypeScript] これ('=>')って何？" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**TypeScript のアロー関数式(=>)** をまとめています。

@[card](https://oukayuka.booth.pm/items/2368045)

## 🌱 結論

:::message
`=>`は、JavaScript（ES2015）で導入されたアロー関数式という書き方で、TypeScript でも使えます。
`function`を使用せず、**`=>`を使用した関数**のことです。
さらに、**省略記法**を使うと、**シンプルかつ少ないコード**で記述することができます。

```ts
// アロー関数式
const calcTax = (price: number) => {
  return Math.floor(price * 1.1);
};

// 省略記法（上と同じ関数を省略して書いたもの。同じ名前で2回は宣言できないため、どちらか一方を使う）
const calcTax = (price: number) => Math.floor(price * 1.1);
```

**メリット**

- アロー関数式: **function**を省略できる。
- アロー関数式 (省略記法): 処理が 1 つの式だけの場合は、**return**と**{}**を省略できる（オブジェクトを返す場合は、`() => ({ a: 1 })`のように丸括弧で囲む）。

:::

:::message alert
アロー関数式は、`function`を使った関数とは`this`の扱いが異なります。アロー関数式の`this`は、定義した場所の`this`を引き継ぎます。そのため、オブジェクトのメソッドなど、`this`を使う場面では置き換えに注意が必要です。
また、アロー関数式は`new`でコンストラクタとして呼び出すことはできません。
:::

:::message
`(x: number) => number`のように、型を書く位置の`=>`は、関数の型を表す別の記法です。
:::

## 🌱 function を使用した関数

```ts
// 機能内容: 渡された金額から消費税込みの金額を計算する
const calcTax = function (price: number) {
  // Math.floor: 小数点を切り捨て
  return Math.floor(price * 1.1);
};
console.log(calcTax(100));
```

:::details 実行結果を確認する

```text
110
```

:::

## 🌱 アロー関数式

```diff ts
 // 機能内容: 渡された金額から消費税込みの金額を計算する
- const calcTax = function (price: number) {
+ const calcTax = (price: number) => {
   // Math.floor: 小数点を切り捨て
   return Math.floor(price * 1.1);
 };
 console.log(calcTax(100));
```

:::details 実行結果を確認する

```text
110
```

:::

## 🌱 アロー関数式 (省略記法)

```diff ts
- const calcTax = (price: number) => {
-  return Math.floor(price * 1.1);
- };
+ const calcTax = (price: number) => Math.floor(price * 1.1);
 console.log(calcTax(100));
```

:::details 実行結果を確認する

```text
110
```

:::
