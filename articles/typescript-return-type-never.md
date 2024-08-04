---
title: '[TypeScript] 処理しない関数には、戻り値の型(never)を使う' # 記事のタイトル
emoji: '🎣' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに

今回は下記書籍で`TypeScript`について学習しました。
下記内容まで解説します。

- never について

@[card](https://www.oreilly.co.jp/books/9784814400362/)

### 結論

:::message
`never`とは、「return 文を持たない」や「処理しない関数」の場合、
戻り値の型を使う事ができる型です。
:::

## やり方

### OK: return 文を持たない

```ts
const fail = (message: string): never => {
  throw new Error(`${message}`);
};
```

### NG: return 文を持つ

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
