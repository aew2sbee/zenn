---
title: '[Prisma] npx prisma db seedが実行できない' # 記事のタイトル
emoji: '⛰' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['prisma', 'seed', 'typescript'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

社内で`seed`を導入する話になりました
しかし、サンプルコードを記述し、`npx prisma db seed`を実行しましたが、
下記のエラーで`seed`が実行出来ませんでした。

```bash
$ npx prisma db seed
Environment variables loaded from .env
Running seed command `ts-node prisma/seed/seed.ts` ...
C:\Users\furutan\Work\xxxxxxxxxxxxxxxx\node_modules\typescript\lib\typescript.js:42537
        ts.Debug.assert(typeof typeReferenceDirectiveName === "string", "Non-string value passed to `ts.resolveTypeReferenceDirective`, likely by a wrapping package working with an outdated `resolveTypeReferenceDirectives` signature. This is probably not a problem in TS itself.");
                 ^
Error: Debug Failure. False expression: Non-string value passed to `ts.resolveTypeReferenceDirective`, likely by a wrapping package working with an outdated `resolveTypeReferenceDirectives` signature. This is probably not a problem in TS itself.
    at Object.resolveTypeReferenceDirective

~~~~~~~~~~~~~~~~~~~~~~~~ 省略 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

An error occurred while running the seed command:
Error: Command failed with exit code 1: ts-node prisma/seed/seed.ts

```

> 和訳<br>.env から環境変数を読み込む
> seed コマンド `ts-node prisma/seed/seed.ts` を実行中 ...
> C:¥Usersfurutan¥Work¥xxxxxxxxxxxxxxxx¥node_modules¥typescript¥lib¥typescript.js:42537

        ts.Debug.assert(typeof typeReferenceDirectiveName === "string", "`ts.resolveTypeReferenceDirective` に渡された非文字列の値は、古い `resolveTypeReferenceDirectives` 署名で動作するラッピングパッケージによる可能性があります。これはおそらくTS自体の問題ではありません」）；
                 ^

エラーです： Debug Failure. 偽の式です： これは、古い `resolveTypeReferenceDirectives` シグネチャで動作するラッピングパッケージによるものと思われます。これは、おそらく TS 自体の問題ではありません。<br>~~~ 省略 ~~~<br>seed コマンドの実行中にエラーが発生しました：
エラーです： コマンドは終了コード 1 で失敗しました: ts-node prisma/seed/seed.ts

:::message
つまり、`TypeScript`の問題ではなく、`resolveTypeReferenceDirectives`が問題みたいですね。
:::

### 対象読者

- 上記のエラーで`npx prisma db seed`が実行できない方

### この記事でわかること

- 上記のエラーの解決方法

### 前提条件

- ts-node 10.0.0

# 手順解説

## 結論

:::message
`package.json`の"ts-node"の version を最新にする
:::

## Error の再現方法

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

`npx prisma db seed`を実行しましたが、`resolveTypeReferenceDirectives`の Error が発生しました。

```bash
$ npx prisma db seed
Environment variables loaded from .env
Running seed command `ts-node prisma/seed/seed.ts` ...
C:\Users\furutan\Work\xxxxxxxxxxxxxxxx\node_modules\typescript\lib\typescript.js:42537
        ts.Debug.assert(typeof typeReferenceDirectiveName === "string", "Non-string value passed to `ts.resolveTypeReferenceDirective`, likely by a wrapping package working with an outdated `resolveTypeReferenceDirectives` signature. This is probably not a problem in TS itself.");
                 ^
Error: Debug Failure. False expression: Non-string value passed to `ts.resolveTypeReferenceDirective`, likely by a wrapping package working with an outdated `resolveTypeReferenceDirectives` signature. This is probably not a problem in TS itself.
    at Object.resolveTypeReferenceDirective

~~~~~~~~~~~~~~~~~~~~~~~~ 省略 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

An error occurred while running the seed command:
Error: Command failed with exit code 1: ts-node prisma/seed/seed.ts
```

## Error の解決方法

### 1. "ts-node"の version を最新する

調べたら、**`package.json`の"ts-node"の version を最新**にしたら解決するみたいなので
`npm install ts-node@latest`を実行して**version を最新**にします。

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

### 2. package.json の中身を確認する

package.json の`ts-node`の version を確認します。
無事に**10.0.0 -> 10.9.1**に変更されました。

```diff json: package.json
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

先ほどの`resolveTypeReferenceDirectives`の Error は発生せず、ローカルの DB にデータが登録されておりました。

## おわりに

seed を初めて触り、よく分からない中
`resolveTypeReferenceDirectives`の Error が発生して苦労しました。
開発のメンバーと一緒に解決できてよかったです。

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
