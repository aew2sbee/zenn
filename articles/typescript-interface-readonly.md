---
title: "[TypeScript] インターフェース(interface)で読み取り専用にする" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

`TypeScript`をより深く理解するために、下記の書籍を読みました。
`インターフェース(interface)で読み取り専用にする`について情報を整理したかったので、執筆します。

@[card](https://www.oreilly.co.jp/books/9784814400362/)

## 🌱 結論

:::message
下記がテンプレートになります。

```ts
interface インターフェース名 {
  readonly プロパティ名: データ型;
}
```

:::

## 🌱 1. 読み取り専用(readonly)を使用しない

`readonly`を付けていないため、プロパティの値を上書きできます。

```ts
interface Writing {
  title: string;
}

let book: Writing = {
  title: "初めての",
};

book.title += "TypeScript";
```

## 🌱 2. 読み取り専用(readonly)を使用する

`readonly`を付けたプロパティを上書きしようとすると、コンパイルエラーになります。

```ts
interface Writing {
  readonly title: string;
}

let book: Writing = {
  title: "初めての",
};

book.title += "TypeScript";
```

:::details コンパイル結果を確認する

```text
Cannot assign to 'title' because it is a read-only property.
```

:::

:::message
`readonly`は型チェック（コンパイル時）だけの制約です。JavaScript に変換したあとの実行時には、値の変更を防げません。
また、変数の宣言が`let`か`const`かは関係ありません。`const`で宣言しても、`readonly`を付けていないプロパティは上書きできます。
さらに、`readonly`が防ぐのはそのプロパティ自体への再代入だけで、プロパティがオブジェクトの場合、その中身は変更できます。
:::
