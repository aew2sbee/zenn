---
title: "[TypeScript] var/let/constの違い" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

`TypeScript`の学習を始めたときに、サンプルコードを見て`var/let/const`の違いが分かりませんでした。
下記書籍で`var/let/const`の違いを理解できたので、その内容をまとめます。

@[card](https://oukayuka.booth.pm/items/2368045)

なお、`var/let/const`は JavaScript の仕様であり、TypeScript でも同じように動作します。

### 動作環境

- TypeScript 7.0.2
- Node.js v22.16.0

サンプルコードは 1 つずつ別のファイル（例: `sample.ts`）に書き、下記のコマンドで確認しています。
同じファイルに続けて書くと、変数`price`の宣言が重複してエラーになります。

```bash
npx tsc sample.ts
node sample.js
```

## 🌱 結論

:::message
主な違いは、**再代入**・**再宣言**ができるかどうかと、**スコープ**（変数を使える範囲）です。

- 再代入: 宣言済みの変数に、`price = 120`のように別の値を入れ直すこと
- 再宣言: `var price`のように、同じ名前の変数をもう一度宣言すること

| 変数宣言 | 意味            | 再代入 | 再宣言 | スコープ |
| :------: | :-------------- | :----: | :----: | :------: |
|   var    | variable (変数) |   〇   |   〇   |   関数   |
|   let    | let (〜とする)  |   〇   |   ×    | ブロック |
|  const   | constant (定数) |   ×    |   ×    | ブロック |

潜在的なバグを生みにくい順に並べると、**const > let > var** です。
再代入や再宣言ができるほど、またスコープが広いほど、意図しない動作が起きやすいためです。
→ 基本は **const** を使い、再代入が必要な場合だけ **let** を使います。

:::

## 🌱 var とは

`var`は、再代入も再宣言もできます。

### 再代入

```ts
// 初回宣言
var price = 100;
console.log(price);

// 再代入
price = 120;
console.log(price);
```

```text:出力結果
100
120
```

`var`の再代入が可能であることが確認できました。

### 再宣言

```ts
// 初回宣言
var price = 100;
console.log(price);

// 再宣言
var price = 200;
console.log(price);
```

```text:出力結果
100
200
```

`var`の再宣言が可能であることが確認できました。
ただし TypeScript では、`var price = "200";`のように**型が異なる**再宣言はコンパイルエラー（TS2403）になります。

## 🌱 let とは

`let`は、再代入はできますが、同じスコープ内での再宣言はできません。

### 再代入

```ts
// 初回宣言
let price = 100;
console.log(price);

// 再代入
price = 120;
console.log(price);
```

```text:出力結果
100
120
```

`let`の再代入が可能であることが確認できました。

### 再宣言

```ts
// 初回宣言
let price = 100;
console.log(price);

// 再宣言
let price = 200;
console.log(price);
```

```text:コンパイル結果（tsc）
sample.ts(2,5): error TS2451: Cannot redeclare block-scoped variable 'price'.
sample.ts(6,5): error TS2451: Cannot redeclare block-scoped variable 'price'.
```

:::message alert
`let`で同じ変数を**再宣言**しているため、コンパイルエラーになります。
:::

## 🌱 const とは

`const`は、再代入も再宣言もできません。

### 再代入

```ts
// 初回宣言
const price = 100;
console.log(price);

// 再代入
price = 120;
console.log(price);
```

```text:コンパイル結果（tsc）
sample.ts(6,1): error TS2588: Cannot assign to 'price' because it is a constant.
```

:::message alert
`const`で宣言した変数に**再代入**しているため、コンパイルエラーになります。
:::

### 再宣言

```ts
// 初回宣言
const price = 100;
console.log(price);

// 再宣言
const price = 200;
console.log(price);
```

```text:コンパイル結果（tsc）
sample.ts(2,7): error TS2451: Cannot redeclare block-scoped variable 'price'.
sample.ts(6,7): error TS2451: Cannot redeclare block-scoped variable 'price'.
```

:::message alert
`const`で同じ変数を**再宣言**しているため、コンパイルエラーになります。
:::

### 補足: 配列やオブジェクトの中身は変更できる

`const`が禁止するのは、変数への再代入だけです。配列やオブジェクトの中身は変更できます。

```ts
const prices = [100, 120];
prices.push(150);
console.log(prices);
```

```text:出力結果
[ 100, 120, 150 ]
```

## 🌱 スコープの違い

`var`のスコープは関数単位のため、`if`などのブロック内で宣言しても、ブロックの外から使えます。

```ts
if (true) {
  var price = 100;
}
console.log(price);
```

```text:出力結果
100
```

一方、`let`と`const`のスコープはブロック単位のため、ブロックの外からは使えません。

```ts
if (true) {
  let price = 100;
}
console.log(price);
```

```text:コンパイル結果（tsc）
sample.ts(4,13): error TS2304: Cannot find name 'price'.
```

:::message alert
`let`で宣言した変数を、ブロックの外から使おうとしているため、コンパイルエラーになります。
:::
