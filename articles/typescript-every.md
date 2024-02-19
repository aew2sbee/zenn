---
title: '[TypeScript] 便利な関数(every)の使い方' # 記事のタイトル
emoji: '📰' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', 'every'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに

配列の各要素を対して全ての要素が条件を満たすかを判定する**every 関数**を学びましたので、執筆しました。

| 項目             | 内容                     |
| ---------------- | ------------------------ |
| **対象者**       | ・TypeScript 初学者      |
| **伝えたい内容** | ・every の使い方が分かる |
| **前提条件**     | ・TypeScript 4.8.4       |

### 結論

:::message
配列の各要素を対して**全ての要素が条件を満たすか**を判定するメソッド

```typescript
const hoge = list.every((各要素) => 条件式);
```

:::

### メリット

1. for ループなどを使用する場合よりも、**シンプルなコード**を書くことができます
2. 最初の条件を満たさない要素が見つかった時点で**false**を返すため、効率的に動作します。

## 1. every()で条件を満たすサンプルコード

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素が全て0より大きい値であるかを確認する
const result = numbers.every((i) => i > 0);

// 期待値： true
console.log(result);
```

出力結果を確認します。

```bash
true
```

## 2. every()で条件を満たさないサンプルコード

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素が全て1より大きい値であるかを確認する
const result = numbers.every((i) => i > 1);

// 期待値： false
console.log(result);
```

出力結果を確認します。

```bash
false
```

## おわりに

.every()といえば、news every.をイメージして 📰 の emoji にしました。
