---
title: "[TypeScript] これ('=>')って何？" # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## 🌱 はじめに

この記事では、**TypeScript のアロー関数式(=>)** をまとめております。

:::details 参考資料
@[card](https://oukayuka.booth.pm/items/2368045)
:::

## 🌱 結論

:::message
`=>`は、TypeScript のアロー関数式という書き方になります。
function を使用せず、**`=>`を使用した関数**のことです。
さらに、記法に**省略記法**があり、**シンプルかつ少ないコード**で記述することが出来ます。

```ts
// アロー関数式
const clac_tax = (price: number) => {
  return Math.floor(price * 1.1);
};

// 省略記法
const clac_tax = (price: number) => Math.floor(price * 1.1);
```

**メリット**

- アロー関数式: **function**を省略する可能。
- アロー関数式 (省略記法): **return**と **{}** を省略する可能。

:::

## 🌱 function を使用した関数

```ts
// 機能内容: 渡された金額を消費税込みの金額に計算をする
const clac_tax = function (price: number) {
  // Math.floor: 小数点を切り捨て
  return Math.floor(price * 1.1);
};
console.log(clac_tax(100));
```

:::details 実行結果を確認する

```bash
110
```

:::

## 🌱 アロー関数式

```diff ts
 // 機能内容: 渡された金額を消費税込みの金額に計算をする。
- const clac_tax = function (price: number) {
  // Math.floor: 小数点を切り捨て
  return Math.floor(price * 1.1);
};

+ const clac_tax = (price: number) => {
  return Math.floor(price * 1.1);
};
console.log(clac_tax(100));
```

:::details 実行結果を確認する。

```bash
110
```

:::

## 🌱 アロー関数式 (省略記法)

```diff ts
- const clac_tax = (price: number) => {
-  return Math.floor(price * 1.1);
- };
+ const clac_tax = (price: number) => Math.floor(price * 1.1);
console.log(clac_tax(100));
```

:::details 実行結果を確認する。

```bash
110
```

:::
