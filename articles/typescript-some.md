---
title: '[TypeScript] 便利な関数(some)の使い方' # 記事のタイトル
emoji: '🥶' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', 'some'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

配列の要素に指定された条件を満たすか判定する**some 関数**を学びましたので、執筆しました。

| 項目             | 内容                    |
| ---------------- | ----------------------- |
| **対象者**       | ・TypeScript 初学者     |
| **伝えたい内容** | ・some の使い方が分かる |
| **前提条件**     | ・TypeScript 4.8.4      |

### 結論

:::message
配列の要素に**指定された条件を満たす**か判定するメソッド

```typescript
const hoge = list.some((各要素) => 条件式);
```

:::

### メリット

#### 1. 引数として関数を直接に渡すことも可能です。

```typescript
const numbers = [1, 2, 3, 4, 5];
const isEven = (num: number) => num % 2 === 0;

const result1 = numbers.some(isEven); // true
const result2 = numbers.some((num) => num % 2 === 0); // true
```

#### 2. includes と some の違いとは？

:::message
**指定された条件**か**指定された要素**が含まれるのかの違いです。
:::

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素に3が含まれているのか
const result1 = numbers.includes(3); // true
const result2 = numbers.some((i) => i === 3); // true
```

## 1. some()で条件を満たすサンプルコード

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素で1つでも1より大きい値があるかを確認する
const result = numbers.some((i) => i > 1);

// 期待値： true
console.log(result(items));
```

出力結果を確認します。

```bash
true
```

## 2. some()で条件を満たさないサンプルコード

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素で1つでも5より大きい値があるかを確認する
const result = numbers.some((i) => i > 5);

// 期待値： false
console.log(result(items));
```

出力結果を確認します。

```bash
false
```

## おわりに

.some()といえば、サムで寒そうな 🥶 の emoji にしました。
