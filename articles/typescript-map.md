---
title: "[TypeScript] 配列の各要素を処理して新しい配列を作る map メソッド" # 記事のタイトル
emoji: "🔁" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "javascript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、配列の各要素に処理を行い、その結果を新しい配列として返す**map メソッド**を解説します。
map は JavaScript 標準の配列メソッド（`Array.prototype.map`）で、TypeScript でもそのまま使えます。

:::details 参考資料
@[card](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
:::

## 🌱 結論

:::message
map は、配列の各要素に対して**指定した処理**を行い、その結果を**新しい配列として返すメソッド**です。

```ts
const 新しい配列 = 元の配列.map((要素) => 新しい要素として返す値);
```

※ 書き方のイメージです。そのままでは動きません。

**メリット**

1. 配列を別の配列に変換するときに、for ループよりも**シンプルなコード**を書くことができます。
2. 「配列を変換している」という**処理の目的がコードから読み取りやすく**なります。
3. **元の配列を変更せず**に新しい配列を返すため、元の配列はそのまま残ります。

:::

## 🌱 基本の書き方

### アロー関数で処理を渡す

map には「各要素に対して行う処理」を関数として渡します。
`(num) => num * 2` のような書き方は**アロー関数**と呼ばれる、関数の短い書き方です。
下記の 2 つは同じ結果になります（`this`の扱いなど細かな違いはありますが、この記事の範囲では気にしなくて構いません）。

```ts
// function を使った書き方
const double1 = function (num: number) {
  return num * 2;
};

// アロー関数を使った書き方
const double2 = (num: number) => num * 2;
```

map に渡す関数では、`num` の型が配列の型から自動で推論されるため、型注釈は不要です。

### `{}` を付ける場合は return が必要

`=>` の右側に `{}` を付けない場合は、右側の式の値がそのまま戻り値になります。
`{}` を付ける場合は `return` が必要です。

```ts
const numbers = [1, 2, 3];

// {} を付けない場合は、式の値がそのまま戻り値になる
const withoutBraces = numbers.map((num) => num * 2); // [2, 4, 6]

// {} を付ける場合は return が必要
const withBraces = numbers.map((num) => {
  return num * 2;
}); // [2, 4, 6]

// return を忘れると undefined の配列になる
const forgotReturn = numbers.map((num) => {
  num * 2;
}); // [undefined, undefined, undefined]（TypeScript ではエラーにならず、void[] 型と推論される）
```

### map の中で起きていること

map は配列の要素を先頭から 1 つずつ取り出して、渡した関数を呼び出します。
関数の戻り値が、新しい配列の同じ位置に入ります。

```text
numbers : [ 1,  2,  3,  4,  5 ]
            ↓   ↓   ↓   ↓   ↓    num * 2
doubled : [ 2,  4,  6,  8, 10 ]
```

## 🌱 サンプルコード

サンプルコードは [TypeScript Playground](https://www.typescriptlang.org/play) に貼り付けて「Run」を押すと、ブラウザ上で動かせます。

### 数値の配列を 2 倍にする

数値の配列の各要素を 2 倍にした、新しい配列を作るサンプルコードです。

```ts
// 数値の配列
const numbers = [1, 2, 3, 4, 5];
// 各要素を2倍にした新しい配列を作る
const doubled = numbers.map((num) => num * 2);

console.log(doubled);
// 元の配列は変わらない
console.log(numbers);
```

:::details 実行結果を確認する

```text
[ 2, 4, 6, 8, 10 ]
[ 1, 2, 3, 4, 5 ]
```

:::

TypeScript では、`numbers` と `doubled` はどちらも `number[]` 型と自動で推論されます。

### for ループで書いた場合との比較

同じ処理を for ループで書くと、下記のようになります。

```ts
const numbers = [1, 2, 3, 4, 5];
const doubled: number[] = [];

for (let i = 0; i < numbers.length; i++) {
  doubled.push(numbers[i] * 2);
}
```

for ループでは「空の配列を用意する」「インデックスを管理する」「push で追加する」という 3 つの手間がかかります。
map を使うと、これらを書かずに 1 行で済みます。

### 戻り値の型を変える

新しい配列の型は、元の配列と同じである必要はありません。
数値の配列から文字列の配列を作ることもできます。

```ts
const numbers = [1, 2, 3];
// string[] 型と推論される
const labels = numbers.map((num) => `${num}個`);

console.log(labels);
```

:::details 実行結果を確認する

```text
[ '1個', '2個', '3個' ]
```

:::

### オブジェクトの配列から値を取り出す

オブジェクトの配列から、特定のプロパティだけを取り出した配列を作れます。

```ts
type User = {
  name: string;
  age: number;
};

const users: User[] = [
  { name: "田中", age: 20 },
  { name: "佐藤", age: 30 },
];

// 名前だけの配列を作る（string[] 型と推論される）
const names = users.map((user) => user.name);

console.log(names);
```

:::details 実行結果を確認する

```text
[ '田中', '佐藤' ]
```

:::

### インデックスを使う

map に渡す関数は、第 2 引数で要素のインデックス（0 から始まる番号）を受け取れます。

```ts
const fruits = ["りんご", "バナナ", "みかん"];
const numberedFruits = fruits.map((fruit, index) => `${index + 1}. ${fruit}`);

console.log(numberedFruits);
```

:::details 実行結果を確認する

```text
[ '1. りんご', '2. バナナ', '3. みかん' ]
```

:::

## 🌱 注意点

### オブジェクトの要素を直接書き換えない

map は新しい配列を返しますが、配列の中のオブジェクトはコピーされません。新しい配列の要素は、元の配列と同じオブジェクトを指しています。
そのため、関数の中でオブジェクトを直接書き換えると、元の配列の中身も変わってしまいます。

※ 下記のコードは、「オブジェクトの配列から値を取り出す」の`users`を使っています。

```ts
// NG: 元の users の age も書き換わってしまう
const ngUsers = users.map((user) => {
  user.age += 1;
  return user;
});
```

オブジェクトの値を変えたい場合は、スプレッド構文（`...`）で新しいオブジェクトを作って返します。
アロー関数で `{ ... }` のオブジェクトを `return` なしで返すときは、`{}` を `()` で囲みます（囲まないと、関数の処理を書く `{}` として扱われます）。

```ts
// OK: 新しいオブジェクトを作って返すので、元の users は変わらない
const okUsers = users.map((user) => ({ ...user, age: user.age + 1 }));
```

※ スプレッド構文でコピーされるのは 1 階層目だけです。オブジェクトの中にネストしたオブジェクトを変更する場合は、そのオブジェクトも同じ方法で作り直します。

### 戻り値の配列が不要なら forEach を使う

map は新しい配列を作るためのメソッドです。
ログ出力のように結果の配列を使わない処理には、`forEach` を使います。

```ts
const numbers = [1, 2, 3];

// 結果の配列を使わない場合は forEach
numbers.forEach((num) => console.log(num));
```

## 🌱 おわりに

map メソッドは、配列を別の配列に変換するときに使うメソッドです。
元の配列を書き換えずに新しい配列を作れるので、配列を変換する処理では、ぜひ for ループの代わりに使ってみてください。
