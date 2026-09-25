---
title: "[TypeScript] 関数の引数を必須ではなくオプションにする" # 記事のタイトル
emoji: "🛡️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

今回は下記書籍『初めての TypeScript』で`TypeScript`について学習しました。
本記事では、関数の引数を省略可能にするオプションパラメーターの書き方と注意点を解説します。

@[card](https://www.oreilly.co.jp/books/9784814400362/)

### 結論

:::message

- 型アノテーション（`x: number`のように、引数に型を書く記法）の中で、`:`の前に`?`を追加する
- オプションパラメーターにした引数は、`| undefined`が追加された合併型（ユニオン型）になる（`strictNullChecks`が有効な場合）
- オプションパラメーターは、必須パラメーターより後ろに置く

```ts
// y を省略した場合は undefined になるため、?? で 0 に置き換えてから計算する
const add = (x: number, y?: number) => x + (y ?? 0);
```

:::

@[card](https://www.typescriptlang.org/docs/handbook/2/functions.html#optional-parameters)

### サンプルコード

以降の例では、下記の`announceSong`関数を使います。
オプションパラメーターの`singer`は`undefined`になる可能性があるため、`if`で値があるかを確認してから使っています。

```ts
const announceSong = (song: string, singer?: string) => {
  console.log(`Song: ${song}`);

  if (singer) {
    console.log(`Singer: ${singer}`);
  }
};
```

## 🌱 使い方

### 1. オプションパラメーターに文字列の引数を渡す

`string`は`string | undefined`に代入できるため、エラーになりません。

```ts
announceSong("Have fun !", "ayaka");
```

```text
Song: Have fun !
Singer: ayaka
```

### 2. オプションパラメーターの引数を省略する

オプションパラメーターは、引数自体を省略できます。省略した引数には`undefined`が入ります。

```ts
announceSong("Have fun !");
```

```text
Song: Have fun !
```

### 3. オプションパラメーターに undefined の引数を渡す

`singer`は`undefined`が追加された合併型になるので、`undefined`を明示的に渡してもエラーになりません。

```ts
announceSong("Have fun !", undefined);
```

```text
Song: Have fun !
```

:::message
`singer: string | undefined`と書いた場合も`undefined`を渡せますが、引数自体は省略できません。引数を省略できるのは、`?`を付けた場合だけです。
:::

## 🌱 注意点：オプションパラメーターは必須パラメーターより後ろに置く

オプションパラメーターの後ろに必須パラメーターを置くと、コンパイル時に下記のエラー（TS1016）が発生します。
※上記の`announceSong`と同じファイルに書く場合は、関数名を変えてください。

```ts
const announceSongNG = (song?: string, singer: string) => {
  console.log(`Song: ${song}`);

  if (singer) {
    console.log(`Singer: ${singer}`);
  }
};
```

```text
error TS1016: A required parameter cannot follow an optional parameter.
```

## 🌱 まとめ

- `?`を付けると、引数を省略できるオプションパラメーターになる
- オプションパラメーターの型は`| undefined`が追加された合併型になるため、使う前に`undefined`のチェックが必要
- オプションパラメーターは、必須パラメーターより後ろに置く
