---
title: "[TypeScript] 非常に大きな整数はbigint型を使う" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**TypeScript の bigint 型** をまとめています。
:::details 参考資料
@[card](https://www.oreilly.co.jp/books/9784873119045/)
@[card](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/BigInt)
@[card](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER)
:::

※コードは TypeScript 5.9 で確認しています。

## 🌱 結論

:::message
`number`型ですべての整数を正確に（隣の整数と区別して）扱えるのは、**安全な整数**の範囲内だけです。

- 安全な整数の最小値: `-9007199254740991`（`-(2 の 53 乗 - 1)`、`Number.MIN_SAFE_INTEGER`）
- 安全な整数の最大値: `9007199254740991`（`2 の 53 乗 - 1`、`Number.MAX_SAFE_INTEGER`）

この範囲を超える整数を正確に扱いたい場合は、**`bigint`型**を使います。
`bigint`型の値は、数値の末尾に`n`を付けて書きます。

```ts
const num: bigint = 2n ** 53n;
```

:::

:::message alert
`bigint`と`number`は、算術演算（`+`、`-`、`*`など）で混在させて計算することはできません（`<`などの比較は可能です）。
また、`bigint`のリテラル（`100n`など）を使うには、`tsconfig.json`の`target`を`ES2020`以上にする必要があります。
:::

## 🌱 1. number 型で安全な整数の範囲を超えるとどうなるか

```ts
console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991

// 安全な整数の範囲を超えると、異なる値のはずが同じ値として扱われる
console.log(2 ** 53 === 2 ** 53 + 1); // true
```

## 🌱 2. bigint 型で大きな整数を扱う

```ts
const big: bigint = 2n ** 53n;

// 範囲を超えても正確に計算できる
console.log(big + 1n); // 9007199254740993n
```

## 🌱 3. bigint 型に number 型の値は代入できない

```ts
// Type 'number' is not assignable to type 'bigint'.
const num: bigint = 100;
```

小さい値でも、`bigint`型として扱う場合は`100n`のように末尾に`n`を付けます。

```ts
const num: bigint = 100n;
```

## 🌱 4. bigint と number は混在させて計算できない

```ts
// Operator '+' cannot be applied to types '1n' and '1'.
const sum = 1n + 1;
```

計算する場合は、`BigInt()`で`bigint`にそろえます（`Number()`で`number`にそろえると、安全な整数の範囲外では精度が失われることがあります）。

```ts
const sum = 1n + BigInt(1); // 2n
```
