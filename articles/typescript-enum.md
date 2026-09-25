---
title: "[TypeScript] enum（列挙型）" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**TypeScript の enum（列挙型）** をまとめています。

## 🌱 結論

:::message

個人的には、下記の使い分けを推奨します。

- 真偽値は、`const`で定数化する
- 真偽値以外の関連する定数の集まりは、`enum`で定数化する

Enum（列挙型）とは？

enum は、一連の関連する定数をまとめたデータ型です。例えば、曜日や月、色などを表すときに使います。

**メリット**

1. **可読性向上**: enum を使用することで、コードがわかりやすくなります。例えば、`Color.RED`と書かれていれば、その値が赤色であることが一目瞭然です。
2. **保守性向上**: 値を 1 か所で定義するため、値の変更や追加が簡単になります。値を変更しても、enum のメンバー名で参照しているコードは変更せずに済みます。
:::

:::message alert
enum は TypeScript 独自の構文で、JavaScript に変換したときにオブジェクトのコードが生成されます。そのため、`erasableSyntaxOnly`（TypeScript 5.8 以降）を有効にした環境や、Node.js の型ストリッピングでは使えません。そうした環境では、`as const`を付けたオブジェクトや、文字列リテラルのユニオン型で代わりにする方法があります。
:::

## 🌱 1. 数値型

:::message
値を指定しなくても、0 から順に自動で割り当てられる
:::

```ts
enum Test1 {
  ONE,
  TWO,
  THREE,
}

console.log("Test1.ONE", Test1.ONE);
console.log("Test1.TWO", Test1.TWO);
console.log("Test1.THREE", Test1.THREE);
```

:::details 実行結果を確認する

```text
Test1.ONE 0
Test1.TWO 1
Test1.THREE 2
```

:::

## 🌱 2. 文字列型

:::message
文字列を指定することができる（文字列の場合は自動で割り当てられないため、すべてのメンバーに値の指定が必要）
:::

```ts
enum Test2 {
  ONE = "1",
  TWO = "2",
  THREE = "3",
}

console.log("Test2.ONE", Test2.ONE);
console.log("Test2.TWO", Test2.TWO);
console.log("Test2.THREE", Test2.THREE);
```

:::details 実行結果を確認する

```text
Test2.ONE 1
Test2.TWO 2
Test2.THREE 3
```

:::

## 🌱 3. 真偽値

:::message alert
真偽値は指定することができない（コンパイルエラーになる）
:::

```ts
enum Test3 {
  Success = true,
  Failure = false,
}

console.log("Test3.Success", Test3.Success);
console.log("Test3.Failure", Test3.Failure);
```

:::details コンパイル結果を確認する

```text
Type 'boolean' is not assignable to type 'number' as required for computed enum member values.
```

:::

## 🌱 4. 小数

:::message
小数（`1.1`など）も指定することができる
:::

```ts
enum Test4 {
  Float = 1.1,
}

console.log("Test4.Float", Test4.Float);
```

:::details 実行結果を確認する

```text
Test4.Float 1.1
```

:::
