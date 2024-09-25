---
title: "[Jest] Typescript環境でカバレッジレポートを表示する" # 記事のタイトル
emoji: "🎡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ['jest', 'typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---


## やり方
### 1. jestに必要な物をインストール
```bash
npm install --save-dev @types/jest jest ts-jest
```
### 2. jestの設定ファイルの作成
下記コマンドを実行します
```
npm init jest@latest
```
:::details 実行結果を確認する
```bash
$ npm init jest@latest

The following questions will help Jest to create a suitable configuration for your project

√ Would you like to use Typescript for the configuration file? ... yes
√ Choose the test environment that will be used for testing » node
√ Do you want Jest to add coverage reports? ... yes
√ Which provider should be used to instrument code for coverage? » V8
√ Automatically clear mock calls, instances, contexts and results before every test? ... yes

📝  Configuration file created at C:\Users\xxxx\work\code-lab\nextjs\jest.config.ts
```
:::


#### Would you like to use Typescript for the configuration file? -Jestの設定ファイルをTypeScriptで書くかどうか?

今回は、TypeScriptを使用するので`Yes`と回答します
#### Choose the test environment that will be used for testing - テスト環境は何にしますか？
Next.jsはNode.jsを利用しているのでnodeを選択します

#### Do you want Jest to add coverage reports? - Jestがカバレッジレポートを生成するかどうか?
カバレッジレポートを確認したいので、`Yes`と回答します

#### Which provider should be used to instrument code for coverage? - コードカバレッジの計測にどのプロバイダを使用するか?

パフォーマンスを優先して`V8`を選択します

**Babel**: BabelはJavaScriptコンパイラで、ES6以上のコードをES5以下に変換します。Babelをカバレッジプロバイダとして使用すると、Babelはコードを変換する際にカバレッジ情報を注入します。これにより、元のソースコードに対する詳細なカバレッジレポートを得ることができます。ただし、この方法はコード変換のプロセスが必要なため、パフォーマンスに影響を与える可能性があります。


**V8**: V8はChromeとNode.jsで使用されているJavaScriptエンジンです。V8をカバレッジプロバイダとして使用すると、V8の組み込みのカバレッジ機能を利用してカバレッジデータを収集します。この方法はBabelよりもパフォーマンスが良いですが、カバレッジレポートの詳細度はBabelほど高くないかもしれません。

#### Automatically clear mock calls, instances, contexts and results before every test? - 各テストの前にモックの呼び出し、インスタンス、コンテキスト、結果を自動的にクリアするかどうか?

各テストが完全に独立して実行され、他のテストからの副作用がないを優先したいため、YESと回答します。

### 3. jestの設定ファイルを更新する
下記のようにファイルを更新する
```ts:jest.config.ts
import type { Config } from 'jest'

const config: Config = {
  // テストの詳細な結果を出力
  verbose: true,
  // テストを逐次的に実行
  maxConcurrency: 1,
  // 各テストの後でモックはリセット
  clearMocks: true,
  // テストカバレッジ情報を出力する
  collectCoverage: true,
  // カバレッジレポートを出力するディレクトリ
  coverageDirectory: 'test/coverage',
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
```tsx:src/sum.ts
function sum(a: number, b: number): number {
  return a + b
}
export default sum
```

```tsx:test/sum.spec.ts
import sum from '../src/sum';

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```


### 5. テストを実行する

下記コマンドでテストを実行する
```bash
npm test
```
:::details 実行結果を確認する
```bash
$ npm test

> nextjs@0.1.0 test
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
`test/coverage/lcov-report/index.html` をブラウザーで表示する
![coverage](/images/articles/jest-coverage/coverage.png)



## カバレッジレポートの項目
### Stmts: 命令網羅率
テスト対象ファイルに含まれる「全てのステートメント(命令)」が少なくとも1回実行されたか？を示す分数です。
### Branch: 分岐網羅率
テスト対象ファイルに含まれる「全ての条件分岐」が少なくとも1回通過したか？を示す分数です。
if文やcase文、三項演算子の分岐が対象になる

### Funcs: 関数網羅率
テスト対象ファイルに含まれる「全ての関数」が少なくとも1回呼び出されたか？を示す分数です。
exportされているが使われていない関数を発見するのに活用できます。

### Lines: 行網羅率
テスト対象ファイルに含まれる「全ての行」を少なくとも1回通過したか？を示す分数です。
