---
title: "[Prisma] npx prisma db seedが実行できない" # 記事のタイトル
emoji: "⛰" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["prisma", "seed", "typescript"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

社内で`seed`を導入する話になりました。
しかし、サンプルコードを記述し、`npx prisma db seed`を実行しましたが、
下記のエラーで`seed`が実行できませんでした。

```bash
$ npx prisma db seed
Environment variables loaded from .env
Running seed command `ts-node prisma/seed/seed.ts` ...
C:\Users\YOUR_NAME\Work\xxxxxxxxxxxxxxxx\node_modules\typescript\lib\typescript.js:42537
        ts.Debug.assert(typeof typeReferenceDirectiveName === "string", "Non-string value passed to `ts.resolveTypeReferenceDirective`, likely by a wrapping package working with an outdated `resolveTypeReferenceDirectives` signature. This is probably not a problem in TS itself.");
                 ^
Error: Debug Failure. False expression: Non-string value passed to `ts.resolveTypeReferenceDirective`, likely by a wrapping package working with an outdated `resolveTypeReferenceDirectives` signature. This is probably not a problem in TS itself.
    at Object.resolveTypeReferenceDirective

~~~~~~~~~~~~~~~~~~~~~~~~ 省略 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

An error occurred while running the seed command:
Error: Command failed with exit code 1: ts-node prisma/seed/seed.ts

```

:::details エラーメッセージの和訳
.env から環境変数を読み込みました。
seed コマンド `ts-node prisma/seed/seed.ts` を実行中 ...

エラー: デバッグアサーションの失敗（式が false）。`ts.resolveTypeReferenceDirective` に文字列以外の値が渡されました。古い `resolveTypeReferenceDirectives` のシグネチャを使っているラッパーパッケージが原因と思われます。おそらく TS 自体の問題ではありません。

（中略）

seed コマンドの実行中にエラーが発生しました。
エラー: コマンドは終了コード 1 で失敗しました: ts-node prisma/seed/seed.ts
:::

:::message
つまり、`TypeScript`本体の問題ではなく、`TypeScript`を内部で使う`ts-node`が、新しい`TypeScript`の`resolveTypeReferenceDirectives`の呼び出し方に対応していないことが原因みたいですね。
:::

### 対象読者

- 上記のエラーで`npx prisma db seed`が実行できない方

### この記事でわかること

- 上記のエラーの解決方法

### 前提条件

- ts-node 10.0.0（TypeScript を 5.x 系に更新した環境で発生します）

## 🌱 結論

:::message
インストールされている"ts-node"のバージョンを最新にする
:::

## 🌱 エラーの再現方法

### 1. ファイル構成の確認

今回は、このようなファイル構成で行いました。

```bash
prisma
├── seed
│   ├── userInfoTable
│   │   ├── insertData.ts
│   │   └── insertScript.ts
│   └── seed.ts
└── schema.prisma
```

### 2. seed を実行する

`npx prisma db seed`を実行しましたが、`resolveTypeReferenceDirectives`のエラーが発生しました。

```bash
$ npx prisma db seed
Environment variables loaded from .env
Running seed command `ts-node prisma/seed/seed.ts` ...
C:\Users\YOUR_NAME\Work\xxxxxxxxxxxxxxxx\node_modules\typescript\lib\typescript.js:42537
        ts.Debug.assert(typeof typeReferenceDirectiveName === "string", "Non-string value passed to `ts.resolveTypeReferenceDirective`, likely by a wrapping package working with an outdated `resolveTypeReferenceDirectives` signature. This is probably not a problem in TS itself.");
                 ^
Error: Debug Failure. False expression: Non-string value passed to `ts.resolveTypeReferenceDirective`, likely by a wrapping package working with an outdated `resolveTypeReferenceDirectives` signature. This is probably not a problem in TS itself.
    at Object.resolveTypeReferenceDirective

~~~~~~~~~~~~~~~~~~~~~~~~ 省略 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

An error occurred while running the seed command:
Error: Command failed with exit code 1: ts-node prisma/seed/seed.ts
```

## 🌱 エラーの解決方法

### 1. "ts-node"のバージョンを最新にする

調べたら、**"ts-node"のバージョンを最新**にしたら解決するみたいなので、
`npm install ts-node@latest`を実行して**バージョンを最新**にします。
（ts-node は開発時だけ使うツールなので、新しく入れる場合は`npm install -D ts-node@latest`のように devDependencies に入れます）

```bash
$ npm install ts-node@latest

up to date, audited 929 packages in 7s

87 packages are looking for funding
  run `npm fund` for details

8 vulnerabilities (3 moderate, 2 high, 3 critical)

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
```

:::message alert
上記の脆弱性の表示は既存の依存関係に対するもので、今回のエラーとは関係ありません。
`npm audit fix --force`は破壊的な変更を含むため、内容を確認せずに実行しないでください。
:::

### 2. package.json の中身を確認する

package.json の`ts-node`のバージョンを確認します。
無事に**10.0.0 -> 10.9.1**に変更されました。

```diff json:package.json
-  "ts-node": "^10.0.0",
+  "ts-node": "^10.9.1",
```

### 3. seed を再度実行する

```bash
$ npx prisma db seed
Environment variables loaded from .env
Running seed command `ts-node prisma/seed/seed.ts` ...
Start seeding ...
Seeding finished.

The seed command has been executed.
```

先ほどの`resolveTypeReferenceDirectives`のエラーは発生せず、ローカルの DB にデータが登録されておりました。

## 🌱 おわりに

seed を初めて触り、よく分からない中
`resolveTypeReferenceDirectives`のエラーが発生して苦労しました。
開発のメンバーと一緒に解決できてよかったです。
