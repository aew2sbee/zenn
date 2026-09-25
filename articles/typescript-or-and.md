---
title: "[TypeScript] これ('||')とそれ('&&')って何？" # 記事のタイトル
emoji: "🛡️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、TypeScript（JavaScript）の`||`と`&&`の**ショートサーキット評価**を解説します。
ショートサーキット評価とは、左から順に評価し、結果が決まった時点で残りの項（演算子の左右に置かれた値）を評価しない仕組みです。`||`と`&&`の評価の仕方は、JavaScript の仕様をそのまま引き継いでいます。
本記事のコードは、TypeScript Playground（https://www.typescriptlang.org/play ）に貼り付けて`Run`を押すと試せます。

:::details 参考資料
@[card](https://oukayuka.booth.pm/items/2368045)
:::

## 🌱 結論

:::message
`||`は**OR 演算子（論理和）**、`&&`は**AND 演算子（論理積）**です。どちらも、`true`/`false`ではなく、項の値そのものを返します。

**OR 演算子**

- 左項が**truthy な値**の場合、**左項の値**が使用され、右項は評価されない
- 左項が**falsy な値**の場合、**右項の値**が使用される
**AND 演算子**

- 左項が**falsy な値**の場合、**左項の値**が使用され、右項は評価されない
- 左項が**truthy な値**の場合、**右項の値**が使用される

**falsy / truthy な値**

- **falsy な値**: 真偽値として扱うと`false`とみなされる値。主に`false`, `0n`（BigInt 型の 0）, `0`, `-0`, `undefined`, `null`, `NaN`（数値として不正な値）, `""`
- **truthy な値**: falsy 以外のすべての値。`[]`や`{}`のように、中身が空の配列やオブジェクトも truthy

:::

@[card](https://developer.mozilla.org/ja/docs/Glossary/Falsy)

### 右項が評価されない例

結果が決まった時点で、右項は評価されません。下記のコードでは、`console.log`は実行されません。

```ts
const isLoggedIn: boolean = true;
const result = isLoggedIn || console.log("実行されない");
console.log(result);
```

:::details 実行結果を確認する

```text
true
```

:::

## 🌱 1. OR 演算子(`||`)

### 評価の流れ

`undefined`〜`""`はすべて**falsy な値**なので順に読み飛ばされ、最初の**truthy な値**である`"foo"`が**最終値**として決まります。

```js
const foo = undefined || null || 0 || NaN || "" || "foo";
console.log(foo);
```

:::details 実行結果を確認する

```text
foo
```

:::

:::message alert
上記のようにリテラルを直接並べたコードは、TypeScript 5.6 以降では「This kind of expression is always falsy.」などのコンパイルエラーになります。評価の流れを確認するための JavaScript のコードとして読んでください。
:::

### よくある使い方：既定値を設定する

値が falsy な場合に、既定値を使うときによく使います。

```ts
const getName = (input?: string): string => input || "名無し";

console.log(getName("hanako"));
console.log(getName());
```

:::details 実行結果を確認する

```text
hanako
名無し
```

:::

@[card](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Logical_OR)

## 🌱 2. AND 演算子(`&&`)

### 評価の流れ

`100`、`[]`、`{}`はすべて**truthy な値**なので順に読み進められ、最後の`"hoge"`が**最終値**として決まります。

```js
const hoge = 100 && [] && {} && "hoge";
console.log(hoge);
```

:::details 実行結果を確認する

```text
hoge
```

:::

途中に falsy な値がある場合は、その時点で評価が止まり、その値が最終値になります。

```js
const piyo = 100 && 0 && "hoge";
console.log(piyo);
```

:::details 実行結果を確認する

```text
0
```

:::

:::message alert
こちらも、TypeScript 5.6 以降では「This kind of expression is always truthy.」などのコンパイルエラーになるため、JavaScript のコードとして読んでください。
:::

### よくある使い方：値があるときだけ処理する

左項が falsy な場合は右項が評価されないため、値の存在を確認してから処理するときによく使います。

```ts
type User = { name: string };

const getUserName = (user?: User) => user && user.name;

console.log(getUserName({ name: "hanako" }));
console.log(getUserName());
```

:::details 実行結果を確認する

```text
hanako
undefined
```

:::

@[card](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Logical_AND)
