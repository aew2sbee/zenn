---
title: '[TypeScript] 便利な関数(reduce)の使い方' # 記事のタイトル
emoji: '💴' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', 'reduce'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに

配列の各要素に対して、結合して単一の値を生成する**reduce 関数**を学びましたので、執筆しました。

| 項目             | 内容                      |
| ---------------- | ------------------------- |
| **対象者**       | ・TypeScript 初学者       |
| **伝えたい内容** | ・reduce の使い方が分かる |
| **前提条件**     | ・TypeScript 4.8.4        |

### 結論

:::message
配列の各要素に対して、結合して単一の値を生成するメソッド

```typescript
const hoge = list.reduce((要素1, 要素2) => 処理);
```

:::

### メリット

1. for ループなどを使用する場合よりも、**シンプルなコード**を書くことができます
2. **初期値を指定することが可能**です。初期値を省略すると、配列の最初の要素が初期値として使用されます。
3. 要素の合計値や積だけでなく、平均値の計算や最大値・最小値の取得、特定の条件を満たす要素の抽出など、**様々な処理**を行うことができます。

## 1. reduce()で合計値を求めるサンプルコード

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の合計値を求める
const result = numbers.reduce((acc, value) => acc + value, 0);

// 期待値： 15
console.log(result);
```

出力結果を確認します。

```bash
15
```

## おわりに

.reduce()といえば、減らされたくないモノをイメージして 💴 の emoji にしました。
