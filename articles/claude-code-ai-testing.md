---
title: "[Claude Code] AIにテストを任せても大丈夫なのか？" # 記事のタイトル
emoji: "🧠" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["claudecode", "jest", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

会社で、Claude Code を活用して生産効率を上げる取り組みに参加しています。

この記事では、Claude Code に`フィボナッチ数を求めるプログラム`とそのテストコードを書かせ、テストコードが信頼できるかを検証しました。

Claude Code は、Anthropic が提供する AI コーディングツールです。ターミナルから自然言語で指示すると、コードの作成や修正、コマンドの実行を行ってくれます。

フィボナッチ数は、0, 1, 1, 2, 3, 5, 8, … のように、直前の 2 つの数を足していく数列の値です。

検証に使ったコードは、下記のリポジトリで公開しています。

@[card](https://github.com/aew2sbee/claude-code-ai-testing)

## 🌱 結論

:::message
**30 分程度で`コーディング`と`テスト`のタスクを完了できました。**
**フィボナッチ数のような仕様が明確な関数であれば、任せてもよいと感じました。**
ただし、AI が書いたテストにも見落としがありました（後述の「テストコードの評価」を参照）。
実際の業務では、業務知識やより複雑な条件があるため、ここまでうまくはいかないと考えています。
:::

## 🌱 環境

検証時（2025年）のバージョンは、package.json に記載のとおりです（Jest 30、ts-jest 29.4、TypeScript 5.8）。

:::message
ts-jest 29.4 系が対応する TypeScript は 7 未満です。最新の TypeScript を入れると対応範囲から外れる場合があるため、バージョンを合わせてください。
:::

リポジトリを使う場合は、下記のコマンドで準備できます。

```bash
git clone https://github.com/aew2sbee/claude-code-ai-testing.git
cd claude-code-ai-testing
npm install
```

ディレクトリ構成は下記のとおりです。

```text
claude-code-ai-testing/
├── src/fibonacci.ts
├── test/fibonacci.test.ts
├── jest.config.js
├── package.json
└── tsconfig.json
```

## 🌱 Claude Code が書いたコード

### 1. フィボナッチ数を求めるプログラム

````ts:src/fibonacci.ts
/**
 * 反復的なアプローチを使用してn番目のフィボナッチ数を計算します。
 *
 * フィボナッチ数列は以下のように定義されます：
 * - F(0) = 0
 * - F(1) = 1
 * - F(n) = F(n-1) + F(n-2) (n > 1の場合)
 *
 * この実装は空間計算量O(1)、時間計算量O(n)を使用します。
 *
 * @param n - フィボナッチ数列の位置（非負の整数である必要があります）
 * @returns n番目のフィボナッチ数
 * @throws {Error} nが負の数の場合
 *
 * @example
 * ```typescript
 * fibonacci(0); // 0を返す
 * fibonacci(1); // 1を返す
 * fibonacci(5); // 5を返す
 * fibonacci(10); // 55を返す
 * ```
 */
export function fibonacci(n: number): number {
  if (n < 0) {
    throw new Error('Fibonacci sequence is not defined for negative numbers');
  }

  if (n <= 1) {
    return n;
  }

  let a = 0;
  let b = 1;

  for (let i = 2; i <= n; i++) {
    const temp = a + b;
    a = b;
    b = temp;
  }
  return b;
}

````

### 2. テストコードの作成

```ts:test/fibonacci.test.ts
import { fibonacci } from '../src/fibonacci';

describe('fibonacci', () => {
  describe('エラーハンドリング', () => {
    test('負の数に対してエラーを投げる', () => {
      expect(() => fibonacci(-1)).toThrow('Fibonacci sequence is not defined for negative numbers');
      expect(() => fibonacci(-5)).toThrow('Fibonacci sequence is not defined for negative numbers');
    });
  });

  describe('基本ケース', () => {
    test('fibonacci(0)は0を返す', () => {
      expect(fibonacci(0)).toBe(0);
    });

    test('fibonacci(1)は1を返す', () => {
      expect(fibonacci(1)).toBe(1);
    });
  });

  describe('反復的な計算', () => {
    test('フィボナッチ数を正しく計算する', () => {
      expect(fibonacci(2)).toBe(1);
      expect(fibonacci(3)).toBe(2);
      expect(fibonacci(4)).toBe(3);
      expect(fibonacci(5)).toBe(5);
      expect(fibonacci(6)).toBe(8);
      expect(fibonacci(7)).toBe(13);
      expect(fibonacci(8)).toBe(21);
      expect(fibonacci(9)).toBe(34);
      expect(fibonacci(10)).toBe(55);
    });

    test('大きなフィボナッチ数を処理する', () => {
      expect(fibonacci(15)).toBe(610);
      expect(fibonacci(20)).toBe(6765);
    });
  });
});
```

### 3. jest.config.js

```js:jest.config.js
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  testMatch: ['<rootDir>/test/**/*.test.ts'],
  collectCoverageFrom: [
    'src/**/*.ts',
    '!src/**/*.d.ts',
  ],
  coverageDirectory: 'coverage',
  coverageReporters: ['text', 'lcov', 'html'],
};

```

### 4. package.json

```json:package.json
{
  "name": "claude-code-ai-testing",
  "version": "1.0.0",
  "main": "index.js",
  "directories": {
    "test": "test"
  },
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "build": "tsc",
    "build:watch": "tsc --watch"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/aew2sbee/claude-code-ai-testing.git"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/aew2sbee/claude-code-ai-testing/issues"
  },
  "homepage": "https://github.com/aew2sbee/claude-code-ai-testing#readme",
  "description": "",
  "devDependencies": {
    "@types/jest": "^30.0.0",
    "jest": "^30.0.4",
    "ts-jest": "^29.4.0",
    "typescript": "^5.8.3"
  }
}

```

### 5. tsconfig.json

```json:tsconfig.json
{
  "compilerOptions": {
    "target": "es2020",
    "module": "commonjs",
    "lib": ["es2020"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true
  },
  "include": [
    "src/**/*"
  ],
  "exclude": [
    "node_modules",
    "dist",
    "test"
  ]
}

```

## 🌱 テスト結果

### 1. 単体テストの結果

プロジェクトのルート（package.json があるディレクトリ）で `npm test` を実行すると、`test(...)` ブロック単位で 5 つのテストがすべて成功（`passed`）しました！

```text
$ npm test

> claude-code-ai-testing@1.0.0 test
> jest

 PASS  test/fibonacci.test.ts
  fibonacci
    エラーハンドリング
      √ 負の数に対してエラーを投げる (12 ms)
    基本ケース
      √ fibonacci(0)は0を返す
      √ fibonacci(1)は1を返す
    反復的な計算
      √ フィボナッチ数を正しく計算する (1 ms)
      √ 大きなフィボナッチ数を処理する (1 ms)

Test Suites: 1 passed, 1 total
Tests:       5 passed, 5 total
Snapshots:   0 total
Time:        2.785 s, estimated 3 s
Ran all test suites.
```

### 2. カバレッジ結果

カバレッジは、テストでコードのどの部分が実行されたかを示す指標です。
`npm run test:coverage` を実行し、`coverage/` に出力された HTML レポートを開くと、Statements・Branches・Functions・Lines のカバレッジがすべて`100%`になりました。

![Jestのカバレッジレポートで、fibonacci.tsのカバレッジがすべて100%になっている画面](/images/articles/claude-code-ai-testing/index_html.png)

## 🌱 テストコードの評価

テストはすべて成功し、カバレッジも 100% でした。
しかし、カバレッジ 100% は「すべてのコードが実行された」ことを示すだけで、テストが十分であることは保証しません。

生成されたテストを見直すと、次の観点が抜けていました。

| 観点 | 実装の挙動 | テストの有無 |
| --- | --- | --- |
| 負の数（`-1` など） | エラーを投げる | あり |
| 境界値（`0`、`1`） | `0`、`1` を返す | あり |
| 小数（`2.5` など） | エラーにならず `1` を返す | なし |
| `NaN` | エラーにならず `1` を返す | なし |
| `Infinity` | ループが終わらない | なし |
| 大きな n（79 以上） | `Number.MAX_SAFE_INTEGER` を超え、正確な整数にならない | なし |

JSDoc には「非負の整数である必要があります」と書かれていますが、実装もテストも「整数であること」を確認していません。
実装とテストを同じ AI が書いたため、同じ観点が両方から抜け落ちたと考えられます。

:::message
AI にテストを任せる場合でも、入力の種類（小数、`NaN`、極端に大きな値など）が網羅されているかは、人が確認する必要があると感じました。
:::

## 🌱 まとめ

- 仕様が明確なフィボナッチ数の関数であれば、Claude Code は短時間で実装とテストを書けた
- テストはすべて成功し、カバレッジも 100% だった
- ただし、小数や `NaN` などの入力に対するテストは抜けており、カバレッジだけでは気づけない
- AI が書いたテストは、観点の漏れがないかを人がレビューしてから使う
