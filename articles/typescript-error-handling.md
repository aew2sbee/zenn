---
title: "[TypeScript] エラー処理/例外処理の3つの方法" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

`TypeScript`をより深く理解したく、下記の書籍（『プログラミングTypeScript』）を読みました。
この書籍の`エラー処理/例外処理`について学びがあったので、記事として記録します。
書籍では Option 型を使う方法も紹介されていますが、本記事では 3 つの方法を扱います。コード例は、書籍のサンプルコードをもとに簡略化したものです。
@[card](https://www.oreilly.co.jp/books/9784873119045/)

## 🌱 結論

:::message

- 単純にエラーを知らせることを求めるなら = **null を返す**
- なぜ失敗したのかについてより多くの情報を求めるなら = **例外をスローする/例外を返す**
- 起こり得るすべての例外を呼び出し側に処理させたいなら = **例外を返す**
- 少ない記述で例外処理を済ませたいなら = **例外をスローする**

:::

以降の例では、誕生日の文字列を`Date`に変換する`parse`関数を使います。
`isValid`は、日付として正しいかを判定する関数です。

```ts
const isValid = (date: Date): boolean => !Number.isNaN(date.getTime());
```

## 🌱 1. null を返す

:::message
型安全にエラーを処理する、**最も軽量な方法**
:::

:::message alert
エラーの有無しかわからない（なぜ失敗したのかは、追加で調査が必要）
呼び出すたびに`null`かどうかを確認する必要があり、複数の処理を組み合わせると**冗長**になりやすい
:::

```ts
const parse = (birthday: string): Date | null => {
  const date = new Date(birthday);
  if (!isValid(date)) {
    return null;
  }
  return date;
};
```

## 🌱 2. 例外をスローする

:::message
null を返す場合と比べて、失敗した理由がわかるため、**デバッグが簡単**になる
`catch`側で`instanceof`を使って、エラーの種類ごとに処理を分けられる
:::

:::message alert
関数の型（シグネチャ）には、どの例外がスローされるかが現れない
そのため、呼び出し側が例外の処理を忘れても、TypeScript はエラーにしない
:::

```ts
/**
 * @throws {RangeError} ユーザーが誕生日を誤った形式で入力した
 */
const parse = (birthday: string): Date => {
  const date = new Date(birthday);
  if (!isValid(date)) {
    throw new RangeError("Enter a date in the form YYYY/MM/DD");
  }
  return date;
};
```

## 🌱 3. 例外を返す

:::message
起こり得る例外が戻り値の型に現れるため、呼び出し側に例外の処理を促せる
例外のクラスを自作すれば、開発者が求めている情報を持たせやすい
:::

:::message alert
例外のクラスを自作し、呼び出し側でも例外ごとの処理を書くため、記述量が増える
:::

```ts
class InvalidDateFormatError extends RangeError {}

const parse = (birthday: string): Date | InvalidDateFormatError => {
  const date = new Date(birthday);
  if (!isValid(date)) {
    return new InvalidDateFormatError("Enter a date in the form YYYY/MM/DD");
  }
  return date;
};

// 呼び出し側
const result = parse("2000/01/01");
if (result instanceof InvalidDateFormatError) {
  console.error(result.message);
} else {
  console.log(result.toISOString());
}
```
