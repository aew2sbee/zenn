---
title: '[TypeScript] インターフェース(interface)を継承する' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、コードをシンプルに書ける**インターフェース(interface)を継承する方法**を解説します。

:::details 参考資料
@[card](https://www.oreilly.co.jp/books/9784814400362/)
:::

## 結論
:::message
```ts
interface 継承元 {
  変数名1: データ型;
}

interface 継承先 extends 継承元 {
  変数名2: データ型;
}
```
**メリット**
- 似たようなコードを複数書くことが軽減する事が可能

**デメリット**
- 注意: 継承が深い場合など可読性がバグに繋がる可能性がある
:::

## 1. 問題がないパターン
:::message
Writingが継承されて構文エラーが発生していない
:::
```ts
interface Writing {
  title: string;
}

interface Novella extends Writing {
  page: number;
}

const MyNovella: Novella = {
  page: 100,
  title: 'MyNovella',
}
```

## 2. 継承先の型が不足
:::message alert

page: 100が足りない為、下記のようなエラーが発生する
Property 'page' is missing in type '{ title: string; }' but required in type 'Novella'.

:::

```ts
interface Writing {
  title: string;
}

interface Novella extends Writing {
  page: number;
}

const MyNovella: Novella = {
  title: 'MyNovella',
}
```


## 3. 継承元の型が不足
:::message alert

title: 'MyNovella'が足りない為、下記のようなエラーが発生する
Property 'title' is missing in type '{ page: number; }' but required in type 'Novella'.
:::
```ts
interface Writing {
  title: string;
}

interface Novella extends Writing {
  page: number;
}

const MyNovella: Novella = {
  page: 100,
}
```
