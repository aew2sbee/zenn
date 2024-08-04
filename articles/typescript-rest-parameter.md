---
title: '[TypeScript] 残りの引数(...hoge)を配列に格納する' # 記事のタイトル
emoji: '🎣' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに

今回は下記書籍で`TypeScript`について学習しました。
下記内容まで解説します。

- `parameter` について

@[card](https://www.oreilly.co.jp/books/9784814400362/)

### 結論

:::message
最後の引数に「...」を追加すると、1 つの配列に全ての値が格納される

```ts
const foodList = (...foods: string[]) => foods;
```

:::

## やり方

### 全ての引数を配列にする

全ての引数(...foods)を配列に格納する

```ts
const foodList = (...foods: string[]) => foods;
console.log(foodList('たまご', '納豆', '豆腐', 'ゼリー', 'めかぶ'));
```

実行結果を確認する

```bash
["たまご", "納豆", "豆腐", "ゼリー", "めかぶ"]
```

### 最初の値は配列に含めない

第一引数(`egg:string`)以外は配列に格納する

```ts
const foodList = (egg: string, ...foods: string[]) => foods;
console.log(foodList('たまご', '納豆', '豆腐', 'ゼリー', 'めかぶ'));
```

実行結果を確認する

```bash
["納豆", "豆腐", "ゼリー", "めかぶ"]
```
