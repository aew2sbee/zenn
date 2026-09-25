---
title: "[TypeScript] メンテしやすいコードの書き方" # 記事のタイトル
emoji: "🛡️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、TypeScript の**メンテしやすいコード**（後から読む・直すときに間違えにくいコード）の書き方を解説します。
書籍『現場で役立つシステム設計の原則』（Java で解説されています）の内容を参考に、TypeScript のコード例に置き換えて紹介します。

:::details 参考資料
@[card](https://gihyo.jp/book/2017/978-4-7741-9087-7)
:::

:::message
各コードは説明用の抜粋です。`quantity`や`taxRate()`など、定義を省略している変数や関数があります。
:::

## 🌱 1. 小さくまとめて分かりやすくする

### 分かりやすい名前を使う

:::message alert
a, b の変数ではどんな値で掛け算しているか不明

```ts
const basePrice = a * b;
```

何の略語なのか分からない

```ts
const basePrice = qty * up;
```

:::

:::message
変数名を quantity（数量）や unitPrice（単価）のように意味の分かる単語にすることで、計算の意味が理解しやすくなる

```ts
const basePrice = quantity * unitPrice;
```

:::

### 目的ごとに変数を用意する

:::message alert
`price`という変数を使い回しているため、途中で`price`の意味（商品代金か、送料込みか、税込みか）が変わる
-> 送料の計算を直すと、後続の税計算まで影響を受ける

```ts
let price = quantity * unitPrice;

if (price > 1000) price += 500;

price = price * taxRate();
```

:::

:::message
目的ごとに変数を分けているので、送料のルールを変えても`basePrice`の計算には手を入れずに済む

```ts
const basePrice = quantity * unitPrice;

let shippingCost = 0;
if (basePrice > 1000) shippingCost = 500;

const itemPrice = (basePrice + shippingCost) * taxRate();
```

:::

### メソッドとして独立する

前のコードを、さらに改善します。

:::message alert
送料の条件分岐（**if 文**）が価格計算の流れに混ざり、何を計算しているかが一目で分からない

```ts
const basePrice = quantity * unitPrice;

let shippingCost = 0;
if (basePrice > 1000) shippingCost = 500;

const itemPrice = (basePrice + shippingCost) * taxRate();
```

:::

:::message

- 処理に名前を付けて切り出すことで、メソッド名から処理を推察しやすい
- 送料のルールを変えるときは、`getShippingCost`の中だけを直せばよい

```ts
const getShippingCost = (basePrice: number): number => {
  if (basePrice > 1000) return 500;
  return 0;
};

const basePrice = quantity * unitPrice;

const shippingCost = getShippingCost(basePrice);

const itemPrice = (basePrice + shippingCost) * taxRate();
```

:::

### 値の範囲を制限してプログラムを分かりやすく安全にする

:::message alert
引数の型を`number`にしているため、負の数・小数・`Number.MAX_SAFE_INTEGER`（`9007199254740991`、安全に扱える整数の最大値）を超える値など、あらゆる数値を受け付けてしまう
-> 確認すべき入力のパターンが増える

```ts
const addNumbers = (num1: number, num2: number): number => num1 + num2;
```

:::

:::message
リテラル型のユニオン型で、引数に渡せる値を 1〜5 に制限する
-> 範囲外の値を渡すとコンパイルエラーになるため、確認すべきパターンを減らせる

```ts
type Number1To5 = 1 | 2 | 3 | 4 | 5;

const addNumbers = (num1: Number1To5, num2: Number1To5): number => num1 + num2;

addNumbers(1, 2); // OK
addNumbers(6, 1); // コンパイルエラー
```

※型による制限はコンパイル時のみです。外部から受け取る値は、実行時にも検証が必要です。
:::

### 複雑さを閉じ込める

:::message alert
配列を直接操作すると、**下記の項目がコードを複雑化させる**

- for 文などのループ処理のロジック
- 配列やコレクション（複数の値をまとめたもの）の要素の数が変化する（可能性がある）
- 個々の要素の内容が変化する（可能性がある）
- 0 件の場合の処理
- 要素の最大数の制限

```ts
const prices: number[] = [];
prices.push(1000);

let total = 0;
for (const price of prices) {
  total += price;
}
```

:::

:::message
配列と、その配列を操作するロジックを**専用のクラスにまとめて閉じ込める**
-> 使う側は配列の中身や件数の変化を気にせず、メソッドを呼ぶだけでよい

```ts
class Prices {
  private readonly values: number[] = [];

  add(price: number): void {
    this.values.push(price);
  }

  total(): number {
    return this.values.reduce((sum, price) => sum + price, 0);
  }
}

const prices = new Prices();
prices.add(1000);
console.log(prices.total()); // 1000
```

:::

## 🌱 2. 場合分けのロジックを整理する

### なるべく else を使わない

`return`した時点で関数の処理は終わるため、条件に当てはまったらすぐに`return`すれば`else`は不要になります（早期リターン）。

:::message alert
条件が 3 つしかない割には、コード量が多く読みにくい

```ts
const getFeeType = (): number => {
  if (isChild()) {
    return 0;
  } else if (isSenior()) {
    return 1;
  } else {
    return 2;
  }
};
```

:::

:::message
条件が一覧化されて、コード量が少なくなり、読みやすい
-> 条件の変更も対応しやすい

```ts
const getFeeType = (): number => {
  if (isChild()) return 0;
  if (isSenior()) return 1;
  return 2;
};
```

:::

## 🌱 3. 業務ロジックを分かりやすく整理する

### メソッドをロジックの置き場にする

業務ロジックとは、料金計算など、アプリケーションが扱う業務上のルールを実装した処理です。
データだけを持つクラスにせず、そのデータを使うロジックもクラスの中に置きます。

:::message alert
受け取った値をただ返すだけのメソッドしかなく、名前を結合する処理などは呼び出し側ごとに書く必要がある

```ts
class Person {
  private firstName: string;
  private lastName: string;

  constructor(firstName: string, lastName: string) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  getFirstName(): string {
    return this.firstName;
  }

  getLastName(): string {
    return this.lastName;
  }
}
```

:::

:::message
名前を扱うクラス（`PersonName`）に、姓と名を結合する処理を置く
-> 呼び出し側ごとに結合処理を書かずに済む

```ts
class PersonName {
  private firstName: string;
  private lastName: string;

  constructor(firstName: string, lastName: string) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  // 日本語表記（姓名の順・区切りなし）で結合する
  getFullName(): string {
    return this.lastName + this.firstName;
  }
}

console.log(new PersonName("太郎", "山田").getFullName()); // 山田太郎
```

:::

## 🌱 まとめ

- 意味の分かる名前を付け、目的ごとに変数やメソッドを分ける
- 型やクラスを使って、値の範囲や複雑な処理を閉じ込める
- 早期リターンで場合分けを読みやすくし、ロジックはデータを持つクラスに置く
