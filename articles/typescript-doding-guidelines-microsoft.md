---
title: '[TypeScript] 私なりのMicrosoftのコーディングガイドの要約' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け', 'コーディング規約'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、一部抜粋した**Microsoft のコーディングガイド** をまとめております。
また、**注意事項**を確認してください

@[card](https://github.com/microsoft/TypeScript/wiki/Coding-guidelines)

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

### 1. 他コンポーネントで共有しないなら export しない

```diff ts
- const usr = 'Ogura'
+ const user = 'Ogura'

- function calc(d: number)
+ function calculateTotal(price: number)
```

### 2.グローバル名前空間には追加しない

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

## 🔷 `null`と`undefined`

### 1. `undefined`を使い`null`は使わない。

:::details なぜ undefined を使って null は避けるのか？

1. 一貫性のため
   JavaScript では、初期化されていない変数や存在しないプロパティは undefined になります。

```ts
let a;
console.log(a); // undefined
```

API や内部コードで`undefined`を使うことで、デフォルトの挙動と合わせられる。

2. 二重管理の複雑さを避ける
   `null`と`undefined`の両方を使うと、「どっちが来るか」を常に意識しないといけません。

```ts
function getName(): string | null | undefined {
  // 呼び出し側は3通りの分岐が必要
}
```

`undefined`に統一すれば、分岐の数を減らし、コードがシンプルになる。

3. 型システムとの相性が良い
   TypeScript の型チェックでは、`undefined`の方が柔軟に扱えます。
   `Partial<T>`や`?`をつけたオプショナル型は`undefined`を前提としています。

```ts
interface User {
  name?: string; // name: string | undefined
}
```

:::

## 🔷 前提条件

### 1.Node や Symbol などのオブジェクトは作成元以外では変更しない

❌ 悪い例：ノードを勝手に書き換える

```ts
function tamperNode(node: ts.Node) {
  node.kind = ts.SyntaxKind.StringLiteral; // ❌ 他の処理にも影響する
}
```

✅ 良い例：読み取り専用として扱う

```ts
function tamperNode(node: ts.Node) {
  node.kind = ts.SyntaxKind.StringLiteral; // ❌ 他の処理にも影響する
}
```

### 2. 配列は基本的に不変として扱う

配列の値は部分的に更新をしない
❌ 悪い例：配列やオブジェクトを直接変更

```ts
const user = { name: 'Alice', age: 30 };
user.age = 31; // ← 直接変更（副作用の原因）

const list = [1, 2, 3];
list.push(4); // ← 元の配列を直接変更
```

✅ 良い例：コピーして変更（不変）

```ts
const user = { name: 'Alice', age: 30 };
const updatedUser = { ...user, age: 31 }; // 新しいオブジェクトを作る

const list = [1, 2, 3];
const newList = [...list, 4]; // 新しい配列を作る
```

✅ より強力に：readonly を使う

```ts
interface User {
  readonly id: number;
  readonly name: string;
}

const user: User = { id: 1, name: 'Alice' };
// user.name = "Bob"; // ❌ エラー！readonlyなので変更不可
const numbers: readonly number[] = [1, 2, 3];
// numbers.push(4); // ❌ エラー
```

## 🔷 コメント

### 1. JSDoc 形式を使用（関数・型・クラスなど）

```ts
/**
 * ユーザー情報を取得する
 * @param userId - 対象のユーザーID
 */
function fetchUser(userId: number): UserInfo {
  // ...
}
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
