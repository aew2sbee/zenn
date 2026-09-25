---
title: "[TypeScript] 関数の引数を必須ではなくオプションにする" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

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

## 🌱 やり方

### 1. オプションパラメーターに文字列の引数を渡す

オプションパラメーターに文字列を引数として渡したので、問題ありません

```ts
announceSong("Have fun !", "ayaka");
```

```bash
"Song: Have fun !"
"Singer: ayaka"
```

### 2. オプションパラメーターに undefined の引数を渡す

singer は「undefined」が追加された合併型になるので、エラーになりません

```ts
announceSong("Have fun !", undefined);
```

```bash
"Song: Have fun !"
```

### 3. オプションパラメーターは、必ず最後であること

オプションパラメーターを先頭に持ってくると、下記のようなエラーが発生します

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
