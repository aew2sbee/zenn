---
title: "[TypeScript] enum（列挙型）" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## はじめに

この記事では、**TypeScript の enum（列挙型）** をまとめております。

## 結論

:::message

個人的に下記で使い分けてを推奨します

- 真偽値は、`const`で定数化する
- 真偽値以外は、`enum`で定数化する

Enum（列挙型）とは？

> Enum は、一連の関連する定数をまとめたデータ型です。これにより、プログラム内で明示的に特定の値を扱うことができます。例えば、曜日や月、カラーなどが挙げられます。

**メリット**

1. **可読性向上**: Enum を使用することで、コードがわかりやすくなります。例えば、Color.RED と書かれていれば、その値が赤色であることが一目瞭然です。
2. **保守性向上**: 列挙型を使用することで、特定の値の変更や追加が簡単になります。新しい値を追加する場合、既存のコードは変更せずに済みます。
   :::

## 1. 数値型

:::message
数値を指定しなくても自動でインクリメントされる
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

```bash
"Num.ONE"　0
"Num.TWO"　1
"Num.THREE"　2
```

:::

## 2. 文字列

:::message
文字列は指定する事が出来る
:::

```typescript
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

```bash
"Test2.ONE" "1"
"Test2.TWO" "2"
"Test2.THREE" "3"
```

:::

## 3. 真偽値

:::message alert
真偽値は指定する事が出来ない

:::

```typescript
enum Test3 {
  Success = true,
  Failure = false,
}

console.log("Test3.ONE", Test3.Success);
console.log("Test3.TWO", Test3.Failure);
```

:::details 実行結果を確認する

```bash
Type 'boolean' is not assignable to type 'number' as required for computed enum member values.
```

:::

### 浮動小数点数型

:::message
浮動小数点数型は指定する事が出来る
:::

```typescript
enum Test4 {
  Float = 1.1,
}

console.log("Test4.Float", Test4.Float);
```

:::details 実行結果を確認する

```bash
"Test4.Float" 1.1
```

:::
