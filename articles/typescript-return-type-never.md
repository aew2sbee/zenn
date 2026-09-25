---
title: "[TypeScript] 呼び出し元に戻らない関数には、戻り値の型(never)を使う" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、TypeScript の戻り値の型**never**を解説します。
`never`を指定すると、「この関数は呼び出し元に戻らない」ことを型で表せます。誤って正常に戻る処理を書いた場合は、コンパイラーがエラーで知らせてくれます。

本記事のコードは、TypeScript Playground（https://www.typescriptlang.org/play ）に貼り付けると、エラーを確認できます。

@[card](https://www.oreilly.co.jp/books/9784814400362/)
@[card](https://www.typescriptlang.org/docs/handbook/2/functions.html#never)

## 🌱 結論

:::message
`never`とは、必ず例外を投げる・無限ループするなど、**処理が最後まで到達せず、呼び出し元に戻らない関数**の戻り値に使う型です。
return 文がないだけで、最後まで実行されて終わる関数には`void`を使います。
:::

## 🌱 1. 必ず例外を投げる関数

`throw`は例外を発生させ、その場で関数の処理を中断します。
下記の`fail`関数は必ず例外を投げるため、呼び出し元に戻ることがありません。そのため、戻り値の型に`never`を指定できます。

```ts
const fail = (message: string): never => {
  throw new Error(message);
};
```

## 🌱 2. 呼び出し元に戻る経路がある関数（エラーになる例）

`never`を指定した関数に、呼び出し元に戻る`return;`を書いた例です。

```ts
const fail = (message: string): never => {
  throw new Error(message);
  return;
};
```

:::message alert
上記のコードは、下記のコンパイルエラーになります。
値を返さない`return;`は`undefined`を返すため、`never`型と矛盾します。

`Type 'undefined' is not assignable to type 'never'.`
:::

また、`throw`の後の`return;`は絶対に実行されないため、エディタ上で`Unreachable code detected.`（到達できないコード）として薄く表示されます。
これは初期設定ではエラーではなく警告で、`tsconfig.json`で`"allowUnreachableCode": false`を設定するとエラーになります。

@[card](https://www.typescriptlang.org/tsconfig/#allowUnreachableCode)

## 🌱 void との違い

|型|使う関数|例|
|---|---|---|
|`void`|処理は最後まで終わるが、値を返さない関数|`console.log`するだけの関数|
|`never`|処理が終わって戻ってくることがない関数|必ず例外を投げる関数、無限ループする関数|

## 🌱 まとめ

- `never`は、呼び出し元に戻らない関数の戻り値に使う型
- `never`を指定した関数に`return;`を書くと、コンパイルエラーになる
- 値を返さずに正常に終わる関数には`void`を使う
