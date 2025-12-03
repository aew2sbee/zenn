---
title: "[TypeScript] 配列の含有を判定するincludes関数" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**TypeScript の includes** をまとめております。

:::details 参考資料
@[card](https://oukayuka.booth.pm/items/2368045)
:::

## 結論

:::message
配列の要素に**指定された要素が含まれているか**を判断するメソッド

```typescript
const hoge = list.includes(指定された要素);
```

**メリット**

1. for ループなどを使用する場合よりも、**シンプルなコード**を書くことができます
2. 第二引数として**検索を開始するインデックスを指定する**ことができる
3. 配列の要素の型を明示的に指定することができ、**型安全性**を確保できる。
   :::

## 1. 条件を満たす

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の値の中で"3"が含まれているのか
const result = numbers.includes(3);

// 期待値： true
console.log(result);
```

:::details 実行結果を確認する

```bash
true
```

:::

## 2. 条件を満たさない

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の値の中で"6"が含まれているのか
const result = numbers.includes(6);

// 期待値： false
console.log(result);
```

:::details 実行結果を確認する

```bash
false
```

:::

## 3. 第二引数を指定する

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の値の中で2番目以降に"1"が含まれているのか
const result = numbers.includes(1, 2);

// 期待値： false
console.log(result);
```

:::details 実行結果を確認する

```bash
false
```

:::
