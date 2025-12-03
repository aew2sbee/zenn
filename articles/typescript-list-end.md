---
title: "[TypeScript] 配列の末尾の値を取得する" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、コードをシンプルに書ける**配列の末尾の値を取得する方法**を解説します。

:::details 参考資料
@[card](https://www.oreilly.co.jp/books/9784814400362/)
:::

## 結論

:::message
下記コードで配列の末尾の値を取得する可能です。

```ts
const hoge = list.at(-1);
```

:::

## 1. length を使う

```ts
const numList: number[] = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(numList[numList.length - 1]);
```

:::details 実行結果を確認する

```bash
10
```

:::

## 2. .at()を使う

:::message
`.at()`は、ECMAScript 2022 に追加されてました。

Property 'at' does not exist on type 'number[]'. Do you need to change your target library? Try changing the 'lib' compiler option to 'es2022' or later.
:::

```ts
const numList: number[] = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(numList.at(-1));
```

:::details 実行結果を確認する

```bash
10
```

:::

:::message alert
**.at()で存在しない index 番号を指定した場合**
.at()の戻り値の型は、「| undefined」の合併型になります。
:::

```ts
const numList: number[] = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(numList.at(11));
```

:::details 実行結果を確認する

```bash
undefined
```

:::
