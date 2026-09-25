---
title: "[TypeScript] オブジェクト(object)を配列に変換する" # 記事のタイトル
emoji: "🛡️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、`Object.keys()`・`Object.values()`・`Object.entries()`を使って、オブジェクトを配列に変換する方法を解説します。

オブジェクトは`{ name: "hanako" }`のように名前（key）と値（value）を組にしたデータで、配列は`["hanako", 18]`のように値を順番に並べたデータです。オブジェクトを配列に変換すると、`map()`や`forEach()`などの配列のメソッドで全項目を処理できます。

## 🌱 結論

:::message
下記コードでオブジェクトを配列に変換できます（`obj`は変換したいオブジェクトです）。

```ts
// オブジェクトのkeyのみを取得する
const keys = Object.keys(obj);

// オブジェクトのvalueのみを取得する
const values = Object.values(obj);

// オブジェクトのkeyとvalueを取得する
const entries = Object.entries(obj);
```

**メリット**

1. 1 行で書ける（for 文不要）
2. `map()`などの配列のメソッドと組み合わせて使える

:::

:::message alert
`Object.values()`と`Object.entries()`は ES2017 で追加された機能です。`tsconfig.json`の`target`（または`lib`）が ES2017 より前だと型エラーになります。
:::

## 🌱 サンプルデータ

以降の例では、下記のオブジェクトを使います。`name`・`age`・`gender`が key、`"hanako"`・`18`・`"female"`が value です。

```ts
const user_info = {
  name: "hanako",
  age: 18,
  gender: "female",
};
```

## 🌱 1. オブジェクトの key のみを取得する

`Object.keys()`は、key の一覧を配列で返します。

```ts
console.log(Object.keys(user_info));
```

:::details 実行結果を確認する

```text
[ 'name', 'age', 'gender' ]
```

:::

:::message
TypeScript では、`Object.keys()`の戻り値は`("name" | "age" | "gender")[]`ではなく`string[]`型になります。そのため、取り出した key で`user_info[key]`のようにアクセスすると、strict モードでは型エラーになります。
:::

## 🌱 2. オブジェクトの value のみを取得する

`Object.values()`は、value の一覧を配列で返します。

```ts
console.log(Object.values(user_info));
```

:::details 実行結果を確認する

```text
[ 'hanako', 18, 'female' ]
```

:::

## 🌱 3. オブジェクトの key と value を取得する

`Object.entries()`は、`[key, value]`の組を要素とする配列を返します。

```ts
console.log(Object.entries(user_info));
```

:::details 実行結果を確認する

```text
[ [ 'name', 'hanako' ], [ 'age', 18 ], [ 'gender', 'female' ] ]
```

:::

## 🌱 4. map() と組み合わせる

`Object.entries()`の結果は配列なので、`map()`でそのまま加工できます。

```ts
const lines = Object.entries(user_info).map(([key, value]) => `${key}: ${value}`);
console.log(lines);
```

:::details 実行結果を確認する

```text
[ 'name: hanako', 'age: 18', 'gender: female' ]
```

:::

※実行結果は Node.js で実行した場合の表示です。実行環境によって表示形式は異なります。

@[card](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)

## 🌱 まとめ

|メソッド|戻り値|
|---|---|
|`Object.keys()`|key の配列|
|`Object.values()`|value の配列|
|`Object.entries()`|`[key, value]`の組の配列|
