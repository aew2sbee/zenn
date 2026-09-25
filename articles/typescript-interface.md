---
title: "[TypeScript] インターフェース(interface)を継承する" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、コードをシンプルに書ける**インターフェース(interface)を継承する方法**を解説します。

:::details 参考資料
@[card](https://www.oreilly.co.jp/books/9784814400362/)
:::

## 🌱 結論

:::message

```ts
interface 継承元 {
  プロパティ名1: データ型;
}

interface 継承先 extends 継承元 {
  プロパティ名2: データ型;
}
```

**メリット**

- 似たようなコードを複数書く手間を減らせる

**デメリット**

- 継承が深くなると、どこで定義されたプロパティなのかが追いにくくなり、可読性が下がる
:::

## 🌱 1. 問題がないパターン

:::message
Writing が継承され、型エラーが発生していない
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
  title: "MyNovella",
};
```

## 🌱 2. 継承先で定義したプロパティが不足

:::message alert
継承先（Novella）で定義した`page`が足りないため、下記のようなエラーが発生する

```text
Property 'page' is missing in type '{ title: string; }' but required in type 'Novella'.
```

:::

```ts
interface Writing {
  title: string;
}

interface Novella extends Writing {
  page: number;
}

const MyNovella: Novella = {
  title: "MyNovella",
};
```

## 🌱 3. 継承元で定義したプロパティが不足

:::message alert
継承元（Writing）で定義した`title`が足りないため、下記のようなエラーが発生する

```text
Property 'title' is missing in type '{ page: number; }' but required in type 'Novella'.
```

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
};
```
