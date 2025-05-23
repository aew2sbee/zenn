---
title: '[TypeScript] 処理しない関数には、戻り値の型(never)を使う' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、コードの安全性を高める**never**を解説します。

:::details 参考資料
@[card](https://www.oreilly.co.jp/books/9784814400362/)
:::

## 結論

:::message
`never`とは、**「return 文を持たない」** や **「処理しない関数」** の場合、
戻り値の型を使う事ができる型です。
:::

## 1. return 文を持たない

```ts
const fail = (message: string): never => {
  throw new Error(`${message}`);
};
```

## 2. return 文を持つ

:::message alert

return 文を持つ関数(何かしらの処理を有する)ような関数では、下記のようなエラーが発生する
(※ 値を返さない return 文は、`undefined` を返します)
`Type 'undefined' is not assignable to type 'never'.`
`Unreachable code detected.`
:::

```ts
const fail = (message: string): never => {
  throw new Error(`${message}`);
  return;
};
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！

@[card](https://www.youtube.com/@aew2sbee)
