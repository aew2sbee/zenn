---
title: '[Jest] mockモジュールを使ってテストを行う' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['jest', 'typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、Jest で**mock モジュールを活用する方法**を解説します。

:::details 参考資料
@[card](https://www.shoeisha.co.jp/book/detail/9784798178189)
:::

## 未定義の関数を定義する

### 1. テスト対象になるサンプル関数を作成する

```ts
export function greet(name: string) {
  return `Hello! ${name}.`;
}

export function sayGoodBye(name: string) {
  throw new Error('未実装');
}
```

### 2. サンプル関数をテストコードを作成する

```ts
import { greet, sayGoodBye } from '../../src/greet';

// ../../src/greetで実装されているgreet関数をモック化実装
jest.mock('../../src/greet', () => ({
  sayGoodBye: (name: string) => `Good bye, ${name}.`,
}));

describe('mockの動作確認1', () => {
  test('greet関数がJest.mockで定義されていない為、undefinedになる', () => {
    expect(greet).toBe(undefined);
  });

  test('sayGoodBye関数がJest.mockで定義された為、モック化された内容に変更される', () => {
    const message = `${sayGoodBye('Taro')} See you.`;
    expect(message).toBe('Good bye, Taro. See you.');
  });
});
```

### 3. テストを実行する

```bash
$ npm test test/unittest/mock01.test.ts

> test
> jest test/unittest/mock01.test.ts

 PASS  test/unittest/mock01.test.ts
  mockの動作確認1
    √ greet関数がJest.mockで定義されていない為、undefinedになる (3 ms)
    √ sayGoodBye関数がJest.mockで再定義された為、モック化された内容に変更される (1 ms)

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        2.625 s
Ran all test suites matching /test\\unittest\\mock01.test.ts/i.
```

## 定義済みの関数を再定義する

### 1. テスト対象になるサンプル関数を作成する

```ts
export function greet(name: string) {
  return `Hello! ${name}.`;
}

export function sayGoodBye(name: string) {
  throw new Error('未実装');
}
```

### 2. サンプル関数をテストコードを作成する

```ts
import { greet, sayGoodBye } from '../../src/greet';

// ../../src/greetで実装されているgreet関数をモック化実装
jest.mock('../../src/greet', () => ({
  ...jest.requireActual('../../src/greet'),
  sayGoodBye: (name: string) => `Good bye, ${name}.`,
}));

describe('mockの動作確認2', () => {
  test('greet関数がJest.mockで元々の関数を使用すると定義された為、元々の関数の内容になる', () => {
    expect(greet('Taro')).toBe('Hello! Taro.');
  });

  test('sayGoodBye関数がJest.mockで定義された為、モック化された内容に変更される', () => {
    const message = `${sayGoodBye('Taro')} See you.`;
    expect(message).toBe('Good bye, Taro. See you.');
  });
});
```

### 3. テストを実行する

```bash
$ npm test test/unittest/mock02.test.ts

> test
> jest test/unittest/mock02.test.ts

 PASS  test/unittest/mock02.test.ts
  mockの動作確認2
    √ greet関数がJest.mockで元々の関数を使用すると定義された為、元々の関数の内容になる (3 ms)
    √ sayGoodBye関数がJest.mockで定義された為、モック化された内容に変更される

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        1.775 s, estimated 3 s
Ran all test suites matching /test\\unittest\\mock02.test.ts/i.
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！

@[card](https://www.youtube.com/@aew2sbee)
