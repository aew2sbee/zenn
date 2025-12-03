---
title: "[TypeScript] 関数内で扱う型を引数のように受け取るジェネリクス関数" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**TypeScript のジェネリクス関数** をまとめております。

:::details 参考資料
@[card](https://www.oreilly.co.jp/books/9784873119045/)
:::

## 結論

:::message
**ジェネリクス関数**とは、データ型を引数のように扱う関数のこと
**型引数(type argument)**=データ型を引数のように扱う

```ts
const 関数名 = <関数内で扱うデータ型>(引数名: 関数内で扱うデータ型[]) => 処理;
```

:::

## 1. それぞれのデータ型で関数を作成する

:::message alert
デメリット：似たような関数が量産されてしまう
:::

```ts
// 数値型の配列の末尾のデータを取得する関数
const getEndElementOfNum = (list: number[]) => list.at(-1);
// 文字列型の配列の末尾のデータを取得する関数
const getEndElementOfStr = (list: string[]) => list.at(-1);

console.log(getEndElementOfNum([1, 2, 3, 4]));
console.log(getEndElementOfStr(["a", "b", "c"]));
```

:::details 実行結果を確認する

```bash
4
c
```

:::

## 2. any 型で共有化

:::message
メリット：似たような関数を量産せず済む
:::
:::message alert
デメリット：any 型の為、意図しないデータ型を処理してしまう
:::

```ts
// 配列の末尾のデータを取得する関数
const getEndElementOfAny = (list: any[]) => list.at(-1);

console.log(getEndElementOfAny([1, 2, 3, 4]));
console.log(getEndElementOfAny(["a", "b", "c"]));
```

:::details 実行結果を確認する

```bash
4
c
```

:::

## 3. ジェネリック関数

:::message
メリット：
似たような関数を量産せず済む
関数内部を確認しなくても、関数外部からデータ型を確認できる
:::

```ts
const getEndElement = <T>(list: T[]) => list.at(-1);

console.log(getEndElement<number>([1, 2, 3, 4]));
console.log(getEndElement<string>(["a", "b", "c"]));
```

:::details 実行結果を確認する

```bash
4
c
```

:::

## 4. 複数の型引数の活用

:::message
メリット：2 つのデータ型を指定できる
:::

```ts
const getEndElement1 = <T, U>(arg1: T, arg2: U) => {
  console.log(arg1, typeof arg1);
  console.log(arg2, typeof arg2);
};

getEndElement1(1, "a");
```

:::details 実行結果を確認する

```bash
1 number
a string
```

:::

## 5. コンポーネント(JSX 構文、.tsx ファイル)でのジェネリクなアロー関数

:::message alert
ジェネリック関数をそのままコンポーネント(JSX 構文、.tsx ファイル)を使用すると下記のようなエラーが発生します。

```tsx
const identity = <T,>(arg: T): T => arg;
```

```bash
JSX 要素 'T' には対応する終了タグがありません
JSX element 'T' has no corresponding closing tag.
```

:::

:::message
<関数内で扱うデータ型>を<関数内で扱うデータ型 = unknown >に変更して制約を追加する
`T = unknown`は、ジェネリック型 `T` のデフォルトの型を指定しています。`unknown`は TypeScript 2.0 以降で導入された型で、他の型に関する情報がないことを表します。つまり、`T`がどの型であるかはわからず、そのままの状態で受け入れられます。これにより、関数が受け取る引数の型と同じ型を返すという振る舞いが実現されます。

```ts
const identity = <T = unknown>(arg: T): T => arg;
```

:::
