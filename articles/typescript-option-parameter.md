---
title: '[TypeScript] 関数の引数を必須ではなくオプションにする' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

今回は下記書籍で`TypeScript`について学習しました。
下記内容まで解説します。

- `parameter` について

@[card](https://www.oreilly.co.jp/books/9784814400362/)

### 結論

:::message
型アノテーションの中で「:」の前に「?」を追加する
オプションパラメーターにした引数は、「 | undefined」が追加された合併型になる

```ts
const add = (x: number, y?: number) => x + y;
```

:::

### 前提

オプションパラメーターは、常に暗黙で「`undefined`」になる可能性がある

```ts
const announceSong = (song: string, singer?: string) => {
  console.log(`Song: ${song}`);

  if (singer) {
    console.log(`Singer: ${singer}`);
  }
};
```

## やり方

### 1. オプションパラメーターに文字列の引数を渡す

指定した文字列に文字列を引数として渡したので、問題がない

```ts
announceSong('Have fun !', 'ayaka');
```

```bash
"Song: Have fun !"
"Singer: ayaka"
```

### 2. オプションパラメーターに undefined の引数を渡す

singer は「undefined」が追加された合併型になるので、Error にならない

```ts
announceSong('Have fun !', undefined);
```

```bash
"Song: Have fun !"
```

### 3. オプションパラメーターは、必ず最後であること

オプションパラメーターを先頭に持ってくると、下記のような Error が発生する

```ts
const announceSong = (song?: string, singer: string) => {
  console.log(`Song: ${song}`);

  if (singer) {
    console.log(`Singer: ${singer}`);
  }
};
```

```bash
A required parameter cannot follow an optional parameter.
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
