---
title: "[Jest] TypeScript環境でカバレッジレポートを表示する" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["jest", "typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、TypeScript の環境で Jest を使い、カバレッジレポートを表示する方法を解説します。

:::message
Jest 30.5、ts-jest、TypeScript 5.9 で動作を確認しています。
:::

## 🌱 やり方

### 1. Jest に必要なものをインストール

Node.js 22.18 未満では、`jest.config.ts`（TypeScript で書いた Jest の設定ファイル）を読み込むために、`ts-node`も必要です。

```bash
npm install --save-dev @types/jest jest ts-jest ts-node typescript@5
```

:::message alert
ts-jest と ts-node は TypeScript 7 では動作しません（TypeScript 7.0 には、これらが使う JavaScript API がないため）。`typescript@5`のように、TypeScript 5.x を指定してインストールしてください。
:::

### 2. Jest の設定ファイルの作成

下記コマンドを実行します

```bash
npm init jest@latest
```

:::details 実行結果を確認する

```text
$ npm init jest@latest

The following questions will help Jest to create a suitable configuration for your project

√ Would you like to use Typescript for the configuration file? ... yes
√ Choose the test environment that will be used for testing » node
√ Do you want Jest to add coverage reports? ... yes
√ Which provider should be used to instrument code for coverage? » V8
√ Automatically clear mock calls, instances, contexts and results before every test? ... yes

📝  Configuration file created at C:\Users\YOUR_NAME\my-project\jest.config.ts
```

:::

#### Would you like to use Typescript for the configuration file? - Jest の設定ファイルを TypeScript で書くかどうか?

今回は、TypeScript を使用するので`Yes`と回答します

#### Choose the test environment that will be used for testing - テスト環境を何にするか

サーバー側のコードをテストするため、`node`を選択します（ブラウザの API を使うコードをテストする場合は`jsdom`を選択します）

#### Do you want Jest to add coverage reports? - Jest がカバレッジレポートを生成するかどうか?

カバレッジレポートを確認したいので、`Yes`と回答します

#### Which provider should be used to instrument code for coverage? - コードカバレッジの計測にどのプロバイダを使用するか?

パフォーマンスを優先して`V8`を選択します

**Babel**: コードを変換するときに、カバレッジを計測するためのコードを埋め込みます（インストルメント）。コードの変換が必要なため、実行が遅くなる場合があります。

**V8**: Chrome と Node.js で使われている JavaScript エンジン V8 の、組み込みのカバレッジ機能を利用します。カバレッジ計測用のコードの埋め込み（インストルメント）が不要なため、一般に Babel より高速です。

#### Automatically clear mock calls, instances, contexts and results before every test? - 各テストの前にモックの呼び出し、インスタンス、コンテキスト、結果を自動的にクリアするかどうか?

各テストを独立して実行し、他のテストのモックの呼び出し履歴の影響を受けないようにしたいため、`Yes`と回答します。

### 3. Jest の設定ファイルを更新する

下記のようにファイルを更新します。

```ts:jest.config.ts
import type { Config } from 'jest'

const config: Config = {
  // テストの詳細な結果を出力
  verbose: true,
  // 各テストの前にモックの呼び出し履歴などをクリア
  clearMocks: true,
  // テストカバレッジ情報を出力する
  collectCoverage: true,
  // カバレッジレポートを出力するディレクトリ
  coverageDirectory: 'test/coverage',
  // カバレッジの計測に V8 を使用する（省略すると babel）
  coverageProvider: 'v8',
  // Jestがテストの結果を報告するために使用するレポーターを設定
  reporters: ['default'],
  // TypeScriptのコードを理解できるようにする設定
  preset: 'ts-jest',
  // テストをNode.js環境で実行する
  testEnvironment: 'node',
}

export default config
```

### 4. 動作確認用のサンプルコードを作成する

```ts:src/sum.ts
function sum(a: number, b: number): number {
  return a + b
}
export default sum
```

```ts:test/sum.spec.ts
import sum from '../src/sum';

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

### 5. テストを実行する

`package.json`の`scripts`に`"test": "jest"`があることを確認し、下記コマンドでテストを実行します。

```json:package.json
{
  "scripts": {
    "test": "jest"
  }
}
```

```bash
npm test
```

:::details 実行結果を確認する

```text
$ npm test

> my-project@1.0.0 test
> jest

 PASS  test/sum.spec.ts
  √ adds 1 + 2 to equal 3 (6 ms)

----------|---------|----------|---------|---------|-------------------
File      | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s
----------|---------|----------|---------|---------|-------------------
All files |     100 |      100 |     100 |     100 |
 sum.ts   |     100 |      100 |     100 |     100 |
----------|---------|----------|---------|---------|-------------------
Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        4.88 s
Ran all test suites.
```

:::

### 6. カバレッジレポートも確認する

`test/coverage/lcov-report/index.html` をブラウザで表示します。

![coverage](/images/articles/jest-coverage/coverage.png)

## 🌱 カバレッジレポートの項目

### Stmts: 命令網羅率

テスト対象ファイルに含まれる「全てのステートメント(命令)」のうち、少なくとも 1 回実行されたものの割合です。

### Branch: 分岐網羅率

テスト対象ファイルに含まれる「全ての条件分岐」のうち、少なくとも 1 回通過したものの割合です。
if 文や switch 文、三項演算子、論理演算子（`&&` / `||` / `??`）などの分岐が対象になります。

### Funcs: 関数網羅率

テスト対象ファイルに含まれる「全ての関数」のうち、少なくとも 1 回呼び出されたものの割合です。
テストで一度も呼び出されていない関数を見つけるのに活用できます。

### Lines: 行網羅率

テスト対象ファイルに含まれる「実行可能なコードを含む行」のうち、少なくとも 1 回実行されたものの割合です。

:::message
カバレッジの集計対象は、テストから読み込まれたファイルだけです。テストがないファイルも集計するには、`collectCoverageFrom`を設定します。
:::
