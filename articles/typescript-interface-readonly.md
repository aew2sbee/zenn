---
title: '[TypeScript] インターフェース(interface)で読み取り専用にする' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

`TypeScript`をより深く理解したく下記書籍を読みました。
`インターフェース(interface)で読み取り専用にする`について情報を整理したかったので、執筆します。
@[card](https://www.oreilly.co.jp/books/9784814400362/)

## 結論

:::message
下記がテンプレートになります。

```ts
interface インターフェース名 {
  readonly 変数名: データ型;
}
```

:::

## 1. 読み取り専用(readonly)を使用しない

let なので値の上書きが可能です

```ts
interface Writing {
  title: string;
}

let book: Writing = {
  title: '初めての',
};

book.title += 'TypeScript';
```

## 2. 読み取り専用(readonly)を使用する

```ts
interface Writing {
  readonly title: string;
}

let book: Writing = {
  title: '初めての',
};

book.title += 'TypeScript';
```

出力結果を確認する

```bash
Cannot assign to 'title' because it is a read-only property.
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
