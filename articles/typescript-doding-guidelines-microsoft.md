---
title: '[TypeScript] MicrosoftのCoding guidelinesの要約' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け', 'コーディング規約'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、\*\*\*\* をまとめております。

:::details 参考資料
@[card](https://github.com/microsoft/TypeScript/wiki/Coding-guidelines)
:::

:::message alert
**注意事項**

これらは TypeScript への貢献者のためのコーディングガイドラインです。
これは TypeScript コミュニティのための指示的なガイドラインではありません。
これらのガイドラインは、TypeScript プロジェクトのコードベースへの貢献者のためのものです。
私たちはチームの一貫性のために多くのガイドラインを選びました。
あなた自身のチームのためにこれらを採用することは自由です。再度言いますが、

これは TypeScript コミュニティのための指示的なガイドラインではありません。

> --- 原文 ---
> These are Coding Guidelines for Contributors to TypeScript.
> This is NOT a prescriptive guideline for the TypeScript community.
> These guidelines are meant for contributors to the TypeScript project's codebase.
> We have chosen many of them for team consistency. Feel free to adopt them for your own team.
>
> AGAIN: This is NOT a prescriptive guideline for the TypeScript community
> :::

## 結論

:::message
**ジェネリクス関数**とは、データ型を引数のように扱う関数のこと
**型引数(type argument)**=データ型を引数のように扱う

```ts
const 関数名 = <関数内で扱うデータ型>(引数名: 関数内で扱うデータ型[]) => 処理;
```

:::

## 🔷 命名規則

### 1. 型名はパスカルケース（PascalCase）を使う

```diff ts
- type userInfo
+ type UserInfo
```

### 2. インタフェース名に`I`を使わない

```diff ts
- interface IUserInfo
+ interface UserInfo
```

### 3. 列挙値（enum)の名はパスカルケース（PascalCase）を使う

```diff ts
enum LogLevel {
-   info,  // camelCase
-   warning,  // camelCase
-   error  // camelCase
+   Info,  // PascalCase
+   Warning,  // PascalCase
+   Error  // PascalCase
}
```

### 4. 関数名はキャメルケース（camelCase）を使う

```diff ts
- function GetUserData(userId: number)  // パスカルケース
+ function getUserData(userId: number)  // キャメルケース
```

### 5. プロパティ名・ローカル変数名はキャメルケース（camelCase）を使う

```diff ts
- const UserId = 1  // パスカルケース
+ const userId = 1  // キャメルケース
```

### 6. private 変数名のの接頭辞に`_`を使わない

```diff ts
- const _userId = 1
+ const userId = 1
```

### 7. 意味のある単語を使うこと

:::message

- `usr`: 何の略語から瞬時に分からない
- `calc`: 何を計算しているのか分からない
  :::

```diff ts
- const usr = 'Ogura'
+ const user = 'Ogura'

- function calc(d: number)
+ function calculateTotal(price: number)
```

## 🔷 コンポーネント

### 1. 1 つのファイルで複数の役割を持たせない

🔧 よくある NG パターン

```ts: index.ts
function scan() { ... }
function parse() { ... }
function emit() { ... }
function checkTypes() { ... }

```

✅ よい構成の例

```bash
src/
├── parser.ts      // パース処理
├── scanner.ts     // スキャナ処理
├── emitter.ts     // 出力処理
├── checker.ts     // 型チェック処理

```

### 2. むやみにファイル追加しない

### 3. 自動生成されたファイル(`.generated.*`)は編集しない

## 🔷 型
### 1. 他コンポーネントで共有しないならexportしない
```diff ts
- const usr = 'Ogura'
+ const user = 'Ogura'

- function calc(d: number)
+ function calculateTotal(price: number)
```

### 2.


❌ ダメな例（グローバルを汚染する）
これらは他のコードやライブラリと名前がかぶるリスクがあり、予期せぬバグの温床になります。
```ts: global.d.ts
// どこでも User 型が使えるが、衝突や上書きの原因に
interface User {
  name: string;
  age: number;
}
```
✅ 良い例（スコープを限定する）
```ts:types/user.ts
// types/user.ts に定義し、必要なファイルでインポート
export interface User {
  name: string;
  age: number;
}

```

### 3. 共通の型は types.ts にまとめる
🔧 よくある NG パターン
```ts: parser.ts
interface Config {
  mode: string;
  verbose: boolean;
}

import { Config } from "./types";

function parseConfig(config: Config) {
  // ...
}
```

✅ よい構成の例
```bash
src/
├── types.ts      // 共通の型
├── parser.ts     // パース処理

```


```ts: types.ts
export interface Config {
  mode: string;
  verbose: boolean;
}
```
```ts: parser.ts
import { Config } from "./types";

function parseConfig(config: Config) {
  // ...
}
```


### 4. 型定義はファイルの最初に記載する
```diff ts
- function scan() { ... }
- function parse() { ... }
- type Scan { ... }
- type Parse { ... }

+ type Scan { ... }
+ type Parse { ... }
+ function scan() { ... }
+ function parse() { ... }
```