---
title: '[TypeScript] 残りの引数(...hoge)を配列に格納する' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、コードをシンプルに書ける**parameter**を解説します。

:::details 参考資料
@[card](https://www.oreilly.co.jp/books/9784814400362/)
:::

## 結論

:::message
最後の引数に「...」を追加すると、1 つの配列に全ての値が格納される

```ts
const foodList = (...foods: string[]) => foods;
```

:::

## 1.全ての引数を配列にする

全ての引数(...foods)を配列に格納する

```ts
const foodList = (...foods: string[]) => foods;
console.log(foodList('たまご', '納豆', '豆腐', 'ゼリー', 'めかぶ'));
```

:::details 実行結果を確認する

```bash
["たまご", "納豆", "豆腐", "ゼリー", "めかぶ"]
```

:::

## 2. 最初の値は配列に含めない

第一引数(`egg:string`)以外は配列に格納する

```ts
const foodList = (egg: string, ...foods: string[]) => foods;
console.log(foodList('たまご', '納豆', '豆腐', 'ゼリー', 'めかぶ'));
```

:::details 実行結果を確認する

```bash
["納豆", "豆腐", "ゼリー", "めかぶ"]
```

:::

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
