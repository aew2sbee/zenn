---
title: "【TypeScript】便利な関数(includes)の使い方" # 記事のタイトル
emoji: "🚽" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "includes"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
配列の各要素に対して指定された条件に一致する最初の要素を返す**includes関数**を学びましたので、執筆しました。

|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・TypeScript初学者  |
|  **伝えたい内容**  |  ・includesの使い方が分かる  |
|  **前提条件**  |  ・TypeScript 4.8.4 |

### 結論
:::message
配列の要素に**指定された要素が含まれているか**を判断するメソッド
```typescript
const hoge = list.includes(指定された要素);
```
:::

### メリット
1. forループなどを使用する場合よりも、**シンプルなコード**を書くことができます
2. 第二引数として**検索を開始するインデックスを指定する**ことができる
3. 配列の要素の型を明示的に指定することができ、**型安全性**を確保できる。

## 1. includes()で条件を満たすサンプルコード
```typescript
// 数字の配列
const numbers = [1,2,3,4,5];
// 配列の値の中で"3"が含まれているのか
const result = numbers.includes(3);

// 期待値： true
console.log(result);
```
出力結果を確認します。
```bash
true
```

## 2. includes()で条件を満たさないサンプルコード
```typescript
// 数字の配列
const numbers = [1,2,3,4,5];
// 配列の値の中で"6"が含まれているのか
const result = numbers.includes(6);

// 期待値： false
console.log(result);
```
出力結果を確認します。
```bash
false
```

## 3. includes()で第二引数を指定するサンプルコード
```typescript
// 数字の配列
const numbers = [1,2,3,4,5];
// 配列の値の中で2番目以降に"1"が含まれているのか
const result = numbers.includes(1,2);

// 期待値： false
console.log(result);
```
出力結果を確認します。
```bash
false
```

## おわりに
.includes()といえば、"含まれる"なのでベン図をイメージして🚽のemojiにしました。

