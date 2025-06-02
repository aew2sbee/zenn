---
title: '[TypeScript] それぞれで型定義ではなく、呼び出しシグネチャを使う' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

`TypeScript`をより深く理解したく下記書籍を読みました。
`それぞれで型定義ではなく、呼び出しシグネチャを使う`について情報を整理したかったので、執筆します。
@[card](https://www.oreilly.co.jp/books/9784814400362/)

## 結論

:::message

```ts
interface 呼び出しシグネチャ名 {
  (引数名1: データ型, 引数名2: データ型): データ型;
}
```

:::

## 1. 型を使わない

```ts
const add = (x, y) => x + y;
```

## 2. 引数と戻り値のそれぞれに型定義

:::message alert
型定義により安全性が向上しましたが、
「引数」「戻り値」に型定義をするとのは、手間であり
可読性はそんなに高くない
:::

```ts
const add = (x: number, y: number): number => x + y;
```

## 3. 呼び出しシグネチャで型定義

:::message
一か所で型定義が出来て、手間が少ない
可読性も向上しました。
:::

```ts
interface CallSignature {
  (x: number, y: number): number;
}

const add: CallSignature = (x, y) => x + y;
console.log(add(2, 5));
```

実行結果を確認する

```bash
7
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
