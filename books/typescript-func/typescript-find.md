---
title: '配列の各要素に対して "指定された条件に一致する最初の要素" を返すメソッド'
free: true
---

## はじめに

配列の各要素に対して指定された条件に一致する最初の要素を返す**find 関数**を学びましたので、執筆しました。

| 項目             | 内容                    |
| ---------------- | ----------------------- |
| **対象者**       | ・TypeScript 初学者     |
| **伝えたい内容** | ・find の使い方が分かる |
| **前提条件**     | ・TypeScript 4.8.4      |

### 結論

:::message
配列の各要素に対して**指定された条件に一致する最初の要素**を返すメソッド

※要素が存在しない場合は`undefined`を返します。
しかし、文末に**?? 任意の値**を記述すると任意の値を返す事が出来る

```typescript
const hoge = list.find((各要素) => 条件式);
```

:::

### メリット

1. for ループなどを使用する場合よりも、**シンプルなコード**を書くことができます
2. 要素が見つからなかった場合は undefined を返すため、**エラー処理**が簡単に行えます。
3. 配列内の要素が多数ある場合でも、検索が*高速*に行えます。

## 1. find()で条件を満たすサンプルコード

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 2より大きい値を取得する(最初にヒットした値を取得する)
const result = numbers.find((i) => i > 2);

// 期待値： 3
console.log(result);
```

出力結果を確認します。

```bash
3
```

## 2. find()で条件を満たさないサンプルコード

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 10より大きい値を取得する
const result = numbers.find((i) => i > 10);

// 期待値： undefined
console.log(result);
```

出力結果を確認します。

```bash
undefined
```

## 3. find()で条件を満たさないかつ undefined を返さないサンプルコード

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 10より大きい値を取得する
const result = numbers.find((i) => i > 10) ?? false;

// 期待値： false
console.log(result);
```

出力結果を確認します。

```bash
false
```