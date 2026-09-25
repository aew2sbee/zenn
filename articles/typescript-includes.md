---
title: "[TypeScript] 配列の含有を判定するincludes関数" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**TypeScript の includes** をまとめています。
`includes`は JavaScript の配列メソッド（`Array.prototype.includes`）で、TypeScript でもそのまま使えます。

@[card](https://oukayuka.booth.pm/items/2368045)

## 🌱 結論

:::message
配列に**指定された要素が含まれているか**を判定するメソッド

```ts
const hoge = list.includes(指定された要素);
```

**メリット**

1. for ループなどを使用する場合よりも、**シンプルなコード**を書くことができます。
2. 第二引数に**検索を開始するインデックスを指定する**ことができます。
3. TypeScript では、配列の要素の型と異なる型の値を渡すとコンパイルエラーになるため、**型の間違い**に気付けます。
:::

## 🌱 1. 条件を満たす

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の値の中に3が含まれているか
const result = numbers.includes(3);

// 期待値： true
console.log(result);
```

:::details 実行結果を確認する

```text
true
```

:::

## 🌱 2. 条件を満たさない

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の値の中に6が含まれているか
const result = numbers.includes(6);

// 期待値： false
console.log(result);
```

:::details 実行結果を確認する

```text
false
```

:::

## 🌱 3. 第二引数を指定する

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// インデックス2（3番目の要素）以降に1が含まれているか
const result = numbers.includes(1, 2);

// 期待値： false
console.log(result);
```

:::details 実行結果を確認する

```text
false
```

:::
