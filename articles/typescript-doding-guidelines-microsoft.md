---
title: "[TypeScript] 私なりのMicrosoftのコーディングガイドラインの要約" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け", "コーディング規約"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**TypeScript（Microsoft）のコーディングガイドライン**から一部を抜粋してまとめています。
下記の**注意事項**も確認してください。
見出しはガイドラインの項目を筆者が訳したもの、コード例と解説は筆者が作成したものです。
また、ガイドラインのうち Classes、Flags、Strings、Diagnostic Messages、General Constructs、Style の項目は省略しています。

@[card](https://github.com/microsoft/TypeScript/wiki/Coding-guidelines)

:::message alert
**注意事項**

（以下、筆者訳）

すぐに読むのをやめてください。このページは、おそらくあなたには関係ありません。

これらは TypeScript への貢献者のためのコーディングガイドラインです。
これは TypeScript コミュニティのための規範的なガイドラインではありません。
これらのガイドラインは、TypeScript プロジェクトのコードベースへの貢献者のためのものです。
その多くは、チームの一貫性のために選んだものです。自分たちのチームで自由に採用してください。

繰り返しますが、これは TypeScript コミュニティのための規範的なガイドラインではありません。
これらのガイドラインについて Issue を作成しないでください。

> --- 原文（抜粋） ---
> STOP READING IMMEDIATELY
>
> THIS PAGE PROBABLY DOES NOT PERTAIN TO YOU
>
> These are Coding Guidelines for Contributors to TypeScript.
> This is NOT a prescriptive guideline for the TypeScript community.
> These guidelines are meant for contributors to the TypeScript project's codebase.
> We have chosen many of them for team consistency. Feel free to adopt them for your own team.
>
> AGAIN: This is NOT a prescriptive guideline for the TypeScript community.
> Please do not file issues about these guidelines.

:::

## 🌱 命名規則

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

### 3. 列挙型（enum）の値の名前はパスカルケース（PascalCase）を使う

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

### 6. private プロパティ名の接頭辞に`_`を使わない

```diff ts
 class User {
-  private _userId: number;
+  private userId: number;
 }
```

### 7. 名前には可能な限り省略しない単語を使う

:::message

- `usr`: 何の略語か瞬時に分からない
- `calc`: calculate の省略形で、何を計算しているのかも分からない
:::

```diff ts
- const usr = "Alice"
+ const user = "Alice"

- function calc(d: number)
+ function calculateTotal(price: number)
```

## 🌱 コンポーネント

### 1. 1 つのファイルには 1 つの役割（コンポーネント）だけを持たせる

❌ 悪い例

```ts:index.ts
function scan() { ... }
function parse() { ... }
function emit() { ... }
function checkTypes() { ... }
```

✅ 良い例

```text
src/
├── parser.ts      // パース処理
├── scanner.ts     // スキャナ処理
├── emitter.ts     // 出力処理
└── checker.ts     // 型チェック処理
```

### 2. 新しいファイルを追加しない（原文: "Do not add new files. :)"）

### 3. 自動生成されたファイル（`.generated.*`）は編集しない

## 🌱 型

### 1. 他コンポーネントで共有しないなら export しない

```diff ts
- export function formatDate(date: Date) { ... } // このファイル内でしか使わない
+ function formatDate(date: Date) { ... }
```

### 2. グローバル名前空間には追加しない

❌ 悪い例（グローバルを汚染する）
（筆者の補足）グローバルに定義した型は、他のコードやライブラリと名前がかぶるリスクがあり、予期せぬバグの温床になります。

```ts:global.d.ts
// どこでも User 型が使えるが、名前の衝突や意図しない宣言マージの原因に
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

❌ 悪い例

```ts:parser.ts
// 複数のファイルで使う型を、各ファイルで個別に定義している
interface Config {
  mode: string;
  verbose: boolean;
}

function parseConfig(config: Config) {
  // ...
}
```

✅ 良い例

```text
src/
├── types.ts      // 共通の型
└── parser.ts     // パース処理
```

```ts:types.ts
export interface Config {
  mode: string;
  verbose: boolean;
}
```

```ts:parser.ts
import { Config } from "./types";

function parseConfig(config: Config) {
  // ...
}
```

### 4. 型定義はファイルの最初に記載する

```diff ts
- function scan() { ... }
- function parse() { ... }
- interface Scan { ... }
- interface Parse { ... }

+ interface Scan { ... }
+ interface Parse { ... }
+ function scan() { ... }
+ function parse() { ... }
```

## 🌱 `null`と`undefined`

### 1. `undefined`を使い`null`は使わない

:::details なぜ undefined を使って null は避けるのか？（筆者の補足）

※ガイドラインには理由が書かれていないため、筆者の考えをまとめたものです。

1. 一貫性のため
   JavaScript では、初期化されていない変数や存在しないプロパティは undefined になります。

```ts
let a;
console.log(a); // undefined
```

API や内部コードで`undefined`を使うことで、デフォルトの挙動と合わせられます。

2. 二重管理の複雑さを避ける
   `null`と`undefined`の両方を使うと、「どっちが来るか」を常に意識しないといけません。

```ts
function getName(): string | null | undefined {
  // 呼び出し側は3通りの分岐が必要
}
```

`undefined`に統一すれば、分岐の数を減らし、コードがシンプルになります。

3. 型システムとの相性が良い
   TypeScript の型チェックでは、`undefined`の方が柔軟に扱えます。
   `Partial<T>`や`?`を付けたオプショナルプロパティは`undefined`を前提としています。

```ts
interface User {
  name?: string; // 省略可能。値の型は string | undefined として扱われる
}
```

:::

## 🌱 一般的な前提

### 1. Node や Symbol などのオブジェクトは作成元以外では変更しない

❌ 悪い例：ノードを勝手に書き換える

※`ts.Node`の`kind`は`readonly`のため、実際には下記のコードは型エラーになります。型で守られていないプロパティでも、作成元以外では書き換えないようにします。

```ts
function tamperNode(node: ts.Node) {
  node.kind = ts.SyntaxKind.StringLiteral; // ❌ 他の処理にも影響する
}
```

✅ 良い例：読み取り専用として扱う

```ts
function getNodeKind(node: ts.Node): ts.SyntaxKind {
  return node.kind; // 読み取るだけで、書き換えない
}
```

### 2. 配列は基本的に不変として扱う

作成した配列は、あとから部分的に更新しません（下記はオブジェクトの例もあわせて載せています）。

❌ 悪い例：配列やオブジェクトを直接変更

```ts
const user = { name: "Alice", age: 30 };
user.age = 31; // ← 直接変更（副作用の原因）

const list = [1, 2, 3];
list.push(4); // ← 元の配列を直接変更
```

✅ 良い例：コピーして変更（不変）

```ts
const user = { name: "Alice", age: 30 };
const updatedUser = { ...user, age: 31 }; // 新しいオブジェクトを作る

const list = [1, 2, 3];
const newList = [...list, 4]; // 新しい配列を作る
```

✅ より強力に：readonly を使う（筆者の補足）

※`readonly`は型チェック上の制約で、実行時の変更は防げません。また、ネストしたプロパティまでは及びません。

```ts
interface User {
  readonly id: number;
  readonly name: string;
}

const user: User = { id: 1, name: "Alice" };
// user.name = "Bob"; // ❌ エラー！readonlyなので変更不可
const numbers: readonly number[] = [1, 2, 3];
// numbers.push(4); // ❌ エラー
```

## 🌱 コメント

### 1. JSDoc 形式を使用（関数・インタフェース・列挙型・クラス）

```ts
/**
 * ユーザー情報を取得する
 * @param userId - 対象のユーザーID
 */
function fetchUser(userId: number): UserInfo {
  // ...
}
```
