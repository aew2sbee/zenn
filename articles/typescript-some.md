---
title: "[TypeScript] 配列の要素が条件を満たすかを判定するsomeメソッド" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、配列の中に条件を満たす要素が 1 つでもあるかを`true`/`false`で判定する**some メソッド**を解説します。
`some`は JavaScript 標準の配列のメソッド（`Array.prototype.some()`）で、TypeScript でもそのまま使えます。

本記事の実行結果は、Node.js で実行したときの表示です。TypeScript Playground（https://www.typescriptlang.org/play ）に貼り付けて`Run`を押しても試せます。

:::details 参考資料
@[card](https://oukayuka.booth.pm/items/2368045)
@[card](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/some)
:::

## 🌱 結論

:::message
配列の要素のうち**1 つでも**条件を満たすものがあれば`true`、1 つもなければ`false`を返すメソッドです。

```ts
// 構文のイメージ（list は配列）
const result = list.some((要素) => 条件式);
```

`some`には「要素を受け取って`true`/`false`を返す関数」を渡します。`some`は配列の先頭から要素を 1 つずつこの関数に渡して判定し、条件を満たす要素が見つかった時点で判定を終了します。
:::

## 🌱 1. 条件を満たす

```ts
// 数値の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素に1つでも1より大きい値があるかを確認する
const result = numbers.some((i) => i > 1);

console.log(result);
```

:::details 実行結果を確認する

```text
true
```

:::

## 🌱 2. 条件を満たさない

```ts
// 数値の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素に1つでも5より大きい値があるかを確認する
const result = numbers.some((i) => i > 5);

console.log(result);
```

:::details 実行結果を確認する

```text
false
```

:::

## 🌱 3. 判定用の関数を別に定義して渡す

判定用の関数を別に定義して、`some`にそのまま渡すこともできます。

```diff ts
const numbers = [1, 2, 3, 4, 5];
const isEven = (num: number) => num % 2 === 0;
- const result = numbers.some((num) => num % 2 === 0);
+ const result = numbers.some(isEven);
```

## 🌱 4. includes との違い

`includes`は**指定した要素**が含まれるかを、`some`は**指定した条件を満たす要素**があるかを判定します。
特定の値が含まれるかを調べるだけなら、どちらでも同じ結果になります。

```ts
// 数値の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素に3が含まれているか
const result1 = numbers.includes(3); // true
const result2 = numbers.some((i) => i === 3); // true
```

「3 より大きい値があるか」のような条件は、`includes`では書けず、`some`で判定します。

```ts
const numbers = [1, 2, 3, 4, 5];
const result = numbers.some((i) => i > 3); // true
```

## 🌱 まとめ

- `some`は、配列の要素のうち 1 つでも条件を満たせば`true`を返すメソッド
- 条件は「要素を受け取って`true`/`false`を返す関数」で渡す
- 特定の値を探すだけなら`includes`、条件で判定するなら`some`を使う
