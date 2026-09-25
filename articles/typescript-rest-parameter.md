---
title: "[TypeScript] 残りの引数(...hoge)を配列に格納する" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、引数の数が決まっていない関数を書くときに使う**残余引数（rest parameters）**を解説します。
残余引数は、関数に渡された引数のうち「残り」を 1 つの配列にまとめて受け取る書き方です。JavaScript の機能で、TypeScript でもそのまま使えます。

本記事の実行結果は、Node.js で実行したときの表示です。TypeScript Playground（https://www.typescriptlang.org/play ）に貼り付けて`Run`を押しても試せます（表示の形式は少し異なります）。

@[card](https://www.oreilly.co.jp/books/9784814400362/)
@[card](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Functions/rest_parameters)
@[card](https://www.typescriptlang.org/docs/handbook/2/functions.html#rest-parameters-and-arguments)

## 🌱 結論

:::message
最後の引数名の前に`...`を付けると、それより前の引数に割り当てられなかった**残りの引数**が、1 つの配列にまとめて格納される。

- 残余引数は、1 つの関数に 1 つだけ、しかも最後の引数にしか書けない
- `...foods: string[]`の`string[]`は「文字列の配列」を表す型
:::

## 🌱 1. 全ての引数を配列にする

引数が残余引数（`...foods`）だけの場合、渡した引数がすべて配列に格納されます。

```ts
const foodList = (...foods: string[]) => foods;
console.log(foodList("たまご", "納豆", "豆腐", "ゼリー", "めかぶ"));
```

:::details 実行結果を確認する

```text
[ 'たまご', '納豆', '豆腐', 'ゼリー', 'めかぶ' ]
```

:::

## 🌱 2. 最初の値は配列に含めない

第一引数（`egg: string`）に 1 つ目の値が入り、残りの値だけが配列に格納されます。

```ts
const foodList = (egg: string, ...foods: string[]) => {
  console.log(egg);
  return foods;
};
console.log(foodList("たまご", "納豆", "豆腐", "ゼリー", "めかぶ"));
```

:::details 実行結果を確認する

```text
たまご
[ '納豆', '豆腐', 'ゼリー', 'めかぶ' ]
```

:::

## 🌱 3. 残余引数を最後以外に書くとエラーになる

残余引数を最後以外の位置に書くと、コンパイルエラーになります。

```ts
const foodList = (...foods: string[], egg: string) => foods;
```

```text
A rest parameter must be last in a parameter list.
```
