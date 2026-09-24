---
title: "[TypeScript] 配列の全ての要素を判定するevery関数" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**TypeScript の every** をまとめています。
`every`は JavaScript の配列メソッド（`Array.prototype.every`）で、TypeScript でもそのまま使えます。

:::details 参考資料
@[card](https://oukayuka.booth.pm/items/2368045)
:::

## 🌱 結論

:::message
配列の**全ての要素が条件を満たすか**を判定するメソッド

```ts
const hoge = list.every((各要素) => 条件式);
```

**メリット**

1. for ループなどを使用する場合よりも、**シンプルなコード**を書くことができます。
2. 条件を満たさない要素が見つかった時点で**false**を返し、残りの要素は判定しないため、効率的に動作します。

:::

:::message alert
空の配列に対しては、条件式にかかわらず`true`を返します。
:::

## 🌱 1. 条件を満たす

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素が全て0より大きい値であるかを確認する
const result = numbers.every((i) => i > 0);

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
// 配列の要素が全て1より大きい値であるかを確認する
const result = numbers.every((i) => i > 1);

// 期待値： false
console.log(result);
```

:::details 実行結果を確認する

```text
false
```

:::
