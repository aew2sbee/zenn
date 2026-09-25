---
title: "[Jest] mockモジュールを使ってテストを行う" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["jest", "typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、Jest で**mock モジュールを活用する方法**を解説します。

:::message
Jest 30.5、ts-jest、TypeScript 5.9 で動作を確認しています。ts-jest は TypeScript 7 では動作しないため、TypeScript 5.x を使ってください。
ファイル構成は、`src/greet.ts`（テスト対象）と`test/unittest/`配下のテストファイルです。
:::

:::details 参考資料
@[card](https://www.shoeisha.co.jp/book/detail/9784798178189)
:::

## 🌱 未実装の関数をモックで定義する

### 1. テスト対象になるサンプル関数を作成する

```ts:src/greet.ts
export function greet(name: string) {
  return `Hello! ${name}.`;
}

export function sayGoodBye(name: string) {
  throw new Error("未実装");
}
```

### 2. サンプル関数のテストコードを作成する

`jest.mock`の第 2 引数に渡した関数（ファクトリ関数）が返すオブジェクトが、モジュールの中身と置き換わります。
なお、`jest.mock`はファイルの先頭に巻き上げられるため、`import`より後に書いてもモックが適用されます。ここでは`sayGoodBye`だけを定義しているため、`greet`は`undefined`になります。

```ts:test/unittest/mock01.test.ts
import { greet, sayGoodBye } from "../../src/greet";

// ../../src/greet モジュールをモック化し、sayGoodBye関数だけを定義する
jest.mock("../../src/greet", () => ({
  sayGoodBye: (name: string) => `Good bye, ${name}.`,
}));

describe("mockの動作確認1", () => {
  test("greet関数がJest.mockで定義されていないため、undefinedになる", () => {
    expect(greet).toBe(undefined);
  });

  test("sayGoodBye関数がJest.mockで定義されたため、モック化された内容に変更される", () => {
    const message = `${sayGoodBye("Taro")} See you.`;
    expect(message).toBe("Good bye, Taro. See you.");
  });
});
```

### 3. テストを実行する

```text
$ npm test test/unittest/mock01.test.ts

> test
> jest test/unittest/mock01.test.ts

 PASS  test/unittest/mock01.test.ts
  mockの動作確認1
    √ greet関数がJest.mockで定義されていないため、undefinedになる (3 ms)
    √ sayGoodBye関数がJest.mockで定義されたため、モック化された内容に変更される (1 ms)

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        2.625 s
Ran all test suites matching test/unittest/mock01.test.ts.
```

## 🌱 定義済みの関数を再定義する

### 1. テスト対象になるサンプル関数を作成する

```ts:src/greet.ts
export function greet(name: string) {
  return `Hello! ${name}.`;
}

export function sayGoodBye(name: string) {
  throw new Error("未実装");
}
```

### 2. サンプル関数のテストコードを作成する

`jest.requireActual`で元のモジュールの中身を展開し、`sayGoodBye`だけを上書きします。そのため、`greet`は元の実装のまま使えます。

```ts:test/unittest/mock02.test.ts
import { greet, sayGoodBye } from "../../src/greet";

// 元のモジュールの中身を使いつつ、sayGoodBye関数だけをモック化する
jest.mock("../../src/greet", () => ({
  ...jest.requireActual("../../src/greet"),
  sayGoodBye: (name: string) => `Good bye, ${name}.`,
}));

describe("mockの動作確認2", () => {
  test("greet関数がJest.mockで元々の関数を使用すると定義されたため、元々の関数の内容になる", () => {
    expect(greet("Taro")).toBe("Hello! Taro.");
  });

  test("sayGoodBye関数がJest.mockで定義されたため、モック化された内容に変更される", () => {
    const message = `${sayGoodBye("Taro")} See you.`;
    expect(message).toBe("Good bye, Taro. See you.");
  });
});
```

### 3. テストを実行する

```text
$ npm test test/unittest/mock02.test.ts

> test
> jest test/unittest/mock02.test.ts

 PASS  test/unittest/mock02.test.ts
  mockの動作確認2
    √ greet関数がJest.mockで元々の関数を使用すると定義されたため、元々の関数の内容になる (3 ms)
    √ sayGoodBye関数がJest.mockで定義されたため、モック化された内容に変更される

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        1.775 s, estimated 3 s
Ran all test suites matching test/unittest/mock02.test.ts.
```
