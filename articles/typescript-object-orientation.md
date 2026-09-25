---
title: "[TypeScript] メンテしやすいコードの書き方" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、TypeScript の**メンテしやすいコードの書き方**を解説します。

:::details 参考資料
@[card](https://gihyo.jp/book/2017/978-4-7741-9087-7)
:::

## 🌱 1. 小さくまとめてわかりやすくする

### わかりやすい名前を使う

:::message alert
a, b の変数ではどんな値で掛け算しているか不明

```ts
let basePrice: number = a * b;
```

何の略語なのか分からない

```ts
let basePrice: number = qty * up;
```

:::

:::message
変数を意味の分かる quantity(数量) unitPrice(単価)を単語にすることで、メソッドが理解しやすい

```ts
let basePrice: number = quantity * unitPrice;
```

:::

### 目的ごとに変数を用意する

:::message alert
`price`という変数を使いまわしているので、改修による影響範囲が大きい

```ts
let price: number = quantity * unitPrice;

if (price > 1000) price += 500;

price = price * taxRate();
```

:::

:::message
目的ごとに変数を分けているので、改修による影響範囲が小さい

```ts
let basePrice: number = quantity * unitPrice;

let shippingCost: number = 0;
if (basePrice > 1000) shippingCost = 500;

let itemPrice: number = (basePrice + shippingCost) * taxRate();
```

:::

### メソッドとして独立する

:::message alert
**if 文**の有無で可読性が下がる

```ts
let basePrice: number = quantity * unitPrice;

let shippingCost: number = 0;
if (basePrice > 1000) shippingCost = 500;

let itemPrice: number = (basePrice + shippingCost) * taxRate();
```

:::

:::message

- メソッド名から処理を推察しやすい
- 改修がメソッド内で留めることが可能

```ts
let basePrice: number = quantity * unitPrice;

let shippingCost: number = getShippingCost(basePrice);

let itemPrice: number = (basePrice + shippingCost) * taxRate();
```

:::

### 値の範囲を制限してプログラムを分かりやすく安全にする

:::message alert
引数の型を数値にしているが、`9007199254740991`(最大数)まで加算することができる
-> 無駄なテストパターンが発生する

```ts
const addNumbers = (num1: number, num2: number): number => num1 + num2;
```

:::

:::message
enum で指定した数値のみしか引数として渡せない
-> テストパターンが最小限にできる

```ts
enum Number1To5 {
  One = 1,
  Two,
  Three,
  Four,
  Five,
}

const addNumbers = (num1: Number1To5, num2: Number1To5): number => num1 + num2;
```

:::

### 複雑さを閉じ込める

:::message alert
**下記項目のコードを複雑化させる**

- for 文などのループ処理のロジック
- 配列やコレクションの要素の数が変化する(可能性がある)
- 個々の要素の内容が変化する(可能性がある)
- 0 件の場合の処理
- 要素の最大数の制限

:::

:::message
ある機能の部分メソッドは、別ファイルとかで管理せず**同じクラス内で管理する**
:::

## 🌱 2. 場合分けのロジックを整理する

### なるべく else を使わない

:::message alert
条件が 3 つしかない割には、コード量が多く読みにくい

```ts
if (isChild()) {
  return 0;
} else if (isSenior()) {
  return 1;
} else {
  return 2;
}
```

:::

:::message
条件が一覧化されて、コード量が少なくなり、読みやすい
-> 条件の変更も対応しやすい

```ts
if (isChild()) return 0;
if (isSenior()) return 1;
return 2;
```

:::

## 🌱 3. 業務ロジックを分かりやすく整理する

### メソッドをロジックの置き場にする

:::message alert
受け取った値をただ返すだけのメソッドで意味がない

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
受け取った値を結合する処理が行われており、意味のあるメソッドになる

```ts
class PersonName {
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

  getFullName(): string {
    return this.lastName + this.firstName;
  }
}
```

:::
