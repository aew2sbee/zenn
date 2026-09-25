---
title: "[TypeScript] 配列の末尾の値を取得する" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、コードをシンプルに書ける**配列の末尾の値を取得する方法**を解説します。

@[card](https://www.oreilly.co.jp/books/9784814400362/)

## 🌱 結論

:::message
下記コードで配列の末尾の値を取得できます。

```ts
const hoge = list.at(-1);
```

※ 書き方のイメージです。`list`は配列の変数名です。

:::

## 🌱 1. `length` を使う

```ts
const numList: number[] = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(numList[numList.length - 1]);
```

:::details 実行結果を確認する

```text
10
```

:::

## 🌱 2. `.at()` を使う

`.at()`に負の数を渡すと、末尾から数えた位置の値を取得できます（`-1`は末尾、`-2`は末尾から 2 番目）。

:::message
`.at()`は、ECMAScript 2022 で追加されました。
`tsconfig.json`の`lib`（`lib`を指定していない場合は`target`）が`ES2022`より前だと、下記のエラーになります。

```text
Property 'at' does not exist on type 'number[]'. Do you need to change your target library? Try changing the 'lib' compiler option to 'es2022' or later.
```

:::

```ts
const numList: number[] = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(numList.at(-1));
```

:::details 実行結果を確認する

```text
10
```

:::

:::message alert
**`.at()`で存在しないインデックスを指定した場合**
`undefined`が返ります。そのため、`.at()`の戻り値の型は、`number | undefined`のように、常に`undefined`を含む合併型になります。
※ `numList[numList.length - 1]`の書き方では、戻り値の型は`number`になります（`noUncheckedIndexedAccess`が無効の場合）。そのため、空の配列で`undefined`が返っても、型からは気付けません。
:::

```ts
const numList: number[] = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(numList.at(11));
```

:::details 実行結果を確認する

```text
undefined
```

:::
