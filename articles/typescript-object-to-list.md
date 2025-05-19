---
title: '[TypeScript] オブジェクト(object)を配列に変換する' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、コードをシンプルに書ける**反復処理**を解説します。

## 結論

:::message
下記コードで配列の末尾の値を取得する可能です。

```ts
// オブジェクトのkeyのみを取得する
const hoge = Object.keys(オブジェクト変数);

// オブジェクトのvalueのみを取得する
const hoge = Object.values(オブジェクト変数);

// オブジェクトのkeyとvalueのみを取得する
const hoge = Object.entries(オブジェクト変数);
```

**メリット**

1. 1 行で処理する事が出来る(for 文不要)
2. map()などの処理にも併せて利用する事が出来る
   :::

## 1. オブジェクトの key のみを取得する

```ts
const user_info = {
  name: 'ogura',
  age: 18,
  gender: 'female',
};
console.log(Object.keys(user_info));
```

:::details 実行結果を確認する

```bash
["name", "age", "gender"]
```

:::

## 2. オブジェクトの value のみを取得する

```ts
const user_info = {
  name: 'ogura',
  age: 18,
  gender: 'female',
};

console.log(Object.values(user_info));
```

:::details 実行結果を確認する

```bash
["ogura", 18, "female"]
```

:::

## 3. オブジェクトの key と value のみを取得する

```ts
const user_info = {
  name: 'ogura',
  age: 18,
  gender: 'female',
};

console.log(Object.entries(user_info));
```

:::details 実行結果を確認する

```bash
[["name", "ogura"], ["age", 18], ["gender", "female"]]
```

:::

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！

@[card](https://www.youtube.com/@aew2sbee)
