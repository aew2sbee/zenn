---
title: "[TypeScript] 何も返さない関数には、戻り値の型(void)を使う" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## はじめに

この記事では、コードの安全性を高める**void**を解説します。

:::details 参考資料
@[card](https://www.oreilly.co.jp/books/9784814400362/)
:::

## 結論

:::message
`void` とは、**return 文を持たない**や**値を返さない return 文**を有する関数の場合、
戻り値の型を使う事ができる型です。
:::

## 1. return 文を持たない

```ts
const log = (): void => {
  console.log("Hello Word");
};
```

## 2. 値を返さない return 文

```ts
const log = (): void => {
  console.log("Hello Word");
  return;
};
```

### 3. 値を返す return 文

:::message alert

`true` を返す関数に `void` を指定すると、下記のようなエラーが発生します
`Type 'boolean' is not assignable to type 'void'.`

:::

```ts
const log = (): void => {
  console.log("Hello Word");
  return true;
};
```
