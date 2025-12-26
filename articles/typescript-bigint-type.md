---
title: "[TypeScript] 非常に大きな整数はbigint型を使う" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## はじめに

この記事では、**TypeScript の bigint 型** をまとめております。
:::details 参考資料
@[card](https://www.oreilly.co.jp/books/9784873119045/)
:::

## 結論

:::message
この範囲内の整数は、**正確に表現され、計算も正確に行われます**
※安全な整数の最小値(-9007199254740991=-2 の 53 乗+1) 安全な整数の最大値(9007199254740991=2 の 53 乗-1)

```ts
const num: bigint = 2 ** 53;
```

:::

:::message alert
※この範囲を超える整数は**正確に表現されず、計算結果も信頼できません**
安全な整数の最小値(-9007199254740991=-2 の 53 乗+1) 安全な整数の最大(9007199254740991=2 の 53 乗-1)
`BigInt` と `number` は互換性がないため、混在させて計算することはできません。
:::

## 1. 安全な整数の最小値を扱う

```ts
// 安全な整数の最小値
const num:bigint = -2 ** 53
```

## 2. 安全な整数の最大値を扱う

```ts
// 安全な整数の最大値
const num: bigint = 2 ** 53;
```

## 3. bigint で小さい値を扱う

```ts
// Type 'number' is not assignable to type 'bigint'.
const num: bigint = 100;
```
