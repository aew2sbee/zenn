---
title: "[TypeScript] 関数内で扱う型を引数のように受け取るジェネリクス関数" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**TypeScript のジェネリクス関数** をまとめています。

:::details 参考資料
@[card](https://www.oreilly.co.jp/books/9784873119045/)
:::

## 🌱 結論

:::message
**ジェネリクス関数**とは、データ型を引数のように扱う関数のこと
関数を呼び出すときに渡すデータ型を**型引数(type argument)**、関数の定義側で受け取る`<T>`を**型パラメーター(type parameter)**と呼びます。

```ts
const 関数名 = <関数内で扱うデータ型>(引数名: 関数内で扱うデータ型[]) => 処理;
```

:::

## 🌱 1. それぞれのデータ型で関数を作成する

:::message alert
デメリット：似たような関数が量産されてしまう
:::

※`at`メソッドを使うには、`tsconfig.json`の`target`（または`lib`）を`ES2022`以上にする必要があります。

```ts
// 数値型の配列の末尾のデータを取得する関数
const getEndElementOfNum = (list: number[]) => list.at(-1);
// 文字列型の配列の末尾のデータを取得する関数
const getEndElementOfStr = (list: string[]) => list.at(-1);

console.log(getEndElementOfNum([1, 2, 3, 4]));
console.log(getEndElementOfStr(["a", "b", "c"]));
```

:::details 実行結果を確認する

```text
4
c
```

:::

## 🌱 2. any 型で共通化

:::message
メリット：似たような関数を量産せずに済む
:::
:::message alert
デメリット：any 型のため、意図しないデータ型も処理できてしまい、戻り値の型も any になる
:::

```ts
// 配列の末尾のデータを取得する関数
const getEndElementOfAny = (list: any[]) => list.at(-1);

console.log(getEndElementOfAny([1, 2, 3, 4]));
console.log(getEndElementOfAny(["a", "b", "c"]));
```

:::details 実行結果を確認する

```text
4
c
```

:::

## 🌱 3. ジェネリクス関数

:::message
メリット：
似たような関数を量産せずに済む
渡した配列の型に応じて、戻り値の型（`number | undefined`など）が決まる
:::

```ts
const getEndElement = <T>(list: T[]) => list.at(-1);

console.log(getEndElement<number>([1, 2, 3, 4]));
console.log(getEndElement<string>(["a", "b", "c"]));
```

:::details 実行結果を確認する

```text
4
c
```

:::

## 🌱 4. 複数の型パラメーターの活用

:::message
メリット：2 つのデータ型を指定できる
:::

```ts
const logArgs = <T, U>(arg1: T, arg2: U) => {
  console.log(arg1, typeof arg1);
  console.log(arg2, typeof arg2);
};

logArgs(1, "a");
```

:::details 実行結果を確認する

```text
1 number
a string
```

:::

## 🌱 5. .tsx ファイル（JSX 構文）でのジェネリクス関数（アロー関数）

:::message alert
.tsx ファイルでジェネリックなアロー関数をそのまま書くと、`<T>`が JSX のタグとして解釈され、下記のようなエラーが発生します。

```tsx
const identity = <T>(arg: T): T => arg;
```

```text
JSX 要素 'T' には対応する終了タグがありません
JSX element 'T' has no corresponding closing tag.
```

:::

:::message
`<T>`を`<T = unknown>`に変更すると、JSX のタグではなく型パラメーターとして解釈されるため、エラーを回避できます。
`T = unknown`は、型パラメーター`T`のデフォルトの型を`unknown`（TypeScript 3.0 で導入された、どんな値でも受け入れる型）にする書き方です。

```tsx
const identity = <T = unknown>(arg: T): T => arg;
```

ほかにも、`<T,>`（末尾にカンマを付ける）や`<T extends unknown>`と書いても回避できます。
:::
