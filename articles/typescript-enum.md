---
title: "[TypeScript] enum（列挙型）" # 記事のタイトル
emoji: '🧏' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに

typescript の学習を始めた時に、`enum（列挙型）`について勉強しました。
色々調べて便利である事が分かりまとめました。

### Enum（列挙型）とは？
> Enumは、一連の関連する定数をまとめたデータ型です。これにより、プログラム内で明示的に特定の値を扱うことができます。例えば、曜日や月、カラーなどが挙げられます。

### Enumのメリット
1. **可読性向上**: Enumを使用することで、コードがわかりやすくなります。例えば、Color.REDと書かれていれば、その値が赤色であることが一目瞭然です。
2. **保守性向上**: 列挙型を使用することで、特定の値の変更や追加が簡単になります。新しい値を追加する場合、既存のコードは変更せずに済みます。

## サンプルコード

### 数値型
:::message
数値を指定しなくても自動でインクリメントされる
:::

```typescript
enum Test1 {
    ONE,
    TWO,
    THREE
}

console.log('Test1.ONE', Test1.ONE)
console.log('Test1.TWO', Test1.TWO)
console.log('Test1.THREE', Test1.THREE)
```

```bash
----- 出力結果 -----
"Num.ONE"　0 
"Num.TWO"　1 
"Num.THREE"　2 
```

### 文字列
:::message
文字列は指定する事が出来る
:::
```typescript
enum Test2 {
    ONE = '1',
    TWO = '2',
    THREE = '3'
}

console.log('Test2.ONE', Test2.ONE)
console.log('Test2.TWO', Test2.TWO)
console.log('Test2.THREE', Test2.THREE)
```

```bash
----- 出力結果 -----
"Test2.ONE" "1" 
"Test2.TWO" "2" 
"Test2.THREE" "3" 
```

### 真偽値
:::message
真偽値は指定する事が出来ない
:::
```typescript
enum Test3 {
    Success = true,
    Failure = false,
}

console.log('Test3.ONE', Test3.Success)
console.log('Test3.TWO', Test3.Failure)
```

:::message alert
Type 'boolean' is not assignable to type 'number' as required for computed enum member values.
:::

### 浮動小数点数型
:::message
浮動小数点数型は指定する事が出来る
:::
```typescript
enum Test4 {
    Float = 1.1,
}

console.log('Test4.Float', Test4.Float)
```

```bash
----- 出力結果 -----
"Test4.Float" 1.1 
```

## おわりに
- 真偽値は、`const`で定数化する
- 真偽値以外は、`enum`で定数化する
