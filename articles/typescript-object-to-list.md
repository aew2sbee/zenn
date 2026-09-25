---
title: "[TypeScript] オブジェクト(object)を配列に変換する" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、コードをシンプルに書ける**反復処理**を解説します。

## 🌱 結論

:::message
下記コードでオブジェクトを配列に変換することが可能です。

```ts
// オブジェクトのkeyのみを取得する
const hoge = Object.keys(オブジェクト変数);

// オブジェクトのvalueのみを取得する
const hoge = Object.values(オブジェクト変数);

// オブジェクトのkeyとvalueを取得する
const hoge = Object.entries(オブジェクト変数);
```

**メリット**

1. 1 行で処理することができる（for 文不要）
2. map()などの処理にも併せて利用することができる
   :::

## 🌱 1. オブジェクトの key のみを取得する

```ts
const user_info = {
  name: "ogura",
  age: 18,
  gender: "female",
};
console.log(Object.keys(user_info));
```

:::details 実行結果を確認する

```bash
["name", "age", "gender"]
```

:::

## 🌱 2. オブジェクトの value のみを取得する

```ts
const user_info = {
  name: "ogura",
  age: 18,
  gender: "female",
};

console.log(Object.values(user_info));
```

:::details 実行結果を確認する

```bash
["ogura", 18, "female"]
```

:::

## 🌱 3. オブジェクトの key と value を取得する

```ts
const user_info = {
  name: "ogura",
  age: 18,
  gender: "female",
};

console.log(Object.entries(user_info));
```

:::details 実行結果を確認する

```bash
[["name", "ogura"], ["age", 18], ["gender", "female"]]
```

:::
