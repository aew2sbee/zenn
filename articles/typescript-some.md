---
title: "[TypeScript] 配列に指定された条件を判定するsome関数" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## はじめに

この記事では、コードをシンプルに書ける**some 関数**を解説します。

:::details 参考資料
@[card](https://oukayuka.booth.pm/items/2368045)
:::

## 結論

:::message
配列の要素に**指定された条件を満たす**か判定するメソッド

```ts
const hoge = list.some((各要素) => 条件式);
```

**メリット**

1. 引数として関数を直接に渡すことも可能です。

```diff ts
const numbers = [1, 2, 3, 4, 5];
const isEven = (num: number) => num % 2 === 0;
- const result2 = numbers.some((num) => num % 2 === 0);
+ const result1 = numbers.some(isEven);
```

2. includes と some の違い
   **指定された条件**か**指定された要素**が含まれるのかの違いです。

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素に3が含まれているのか
const result1 = numbers.includes(3); // true
const result2 = numbers.some((i) => i === 3); // true
```

:::

## 1. 条件を満たす

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素で1つでも1より大きい値があるかを確認する
const result = numbers.some((i) => i > 1);

console.log(result(items));
```

:::details 実行結果を確認する

```bash
true
```

:::

## 2. 条件を満たさない

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素で1つでも5より大きい値があるかを確認する
const result = numbers.some((i) => i > 5);

console.log(result(items));
```

:::details 実行結果を確認する

```bash
false
```

:::
