---
title: "[TypeScript] 何も返さない関数には、戻り値の型(void)を使う" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、TypeScript の戻り値の型**void**を解説します。
`void`を指定すると、「この関数は値を返さない」ことを型で表せます。値を返さないつもりの関数で誤って値を返すと、コンパイルエラーで気付けます。

本記事のコードは、TypeScript Playground（https://www.typescriptlang.org/play ）に貼り付けると、エラーの有無を確認できます。

:::details 参考資料
@[card](https://www.oreilly.co.jp/books/9784814400362/)
@[card](https://www.typescriptlang.org/docs/handbook/2/functions.html#void)
:::

## 🌱 結論

:::message
`(): void =>`のように、引数の括弧の後ろに`: 型`を書くと、その関数が返す値（戻り値）の型を指定できます。
`void`は「値を返さない」ことを表す型で、**return 文がない関数**や、**値を返さない`return;`だけを持つ関数**の戻り値に使います。
値を返すとコンパイルエラーになります。
:::

## 🌱 1. return 文がない関数

エラーなくコンパイルできます。

```ts
const log = (): void => {
  console.log("Hello World");
};
```

## 🌱 2. 値を返さない return 文を持つ関数

エラーなくコンパイルできます。

```ts
const log = (): void => {
  console.log("Hello World");
  return;
};
```

## 🌱 3. 値を返す return 文を持つ関数（エラーになる例）

```ts
const log = (): void => {
  console.log("Hello World");
  return true;
};
```

:::message alert
戻り値の型に`void`を指定した関数で`true`を返すと、下記のコンパイルエラーになります。
`true`は boolean 型の値なので、「boolean を void に代入できない」というエラーです。

`Type 'boolean' is not assignable to type 'void'.`
:::

:::message
このエラーになるのは、関数の定義で戻り値の型に`void`を直接書いた場合です。
`type Log = () => void`のような関数の型に関数を代入する場合は、値を返す関数も代入でき、戻り値は無視されます。
@[card](https://www.typescriptlang.org/docs/handbook/2/functions.html#return-type-void)
:::

## 🌱 never との違い

`void`の関数は、処理が最後まで終わって呼び出し元に戻ります（実行時には`undefined`が返ります）。
一方、必ず例外を投げるなど、呼び出し元に戻らない関数の戻り値には`never`を使います。

## 🌱 まとめ

- `void`は、値を返さない関数の戻り値に使う型
- return 文がない関数や、`return;`だけの関数に指定できる
- 値を返すとコンパイルエラーになるので、誤って値を返すミスに気付ける
