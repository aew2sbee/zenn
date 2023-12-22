---
title: '[作業] tsconfig.jsonの設定'
free: true
---

## tsconfig.jsonとは
> TypeScriptプロジェクトの設定ファイルであり、TypeScriptコンパイラに対してプロジェクトのビルド方法やコンパイルオプションを指示するためのものです。

## tsconfig.jsonの作成
下記コマンドでデフォルトで作成する事が出来ますが、PJ作成時に作成されます。
````bash
tsc --init
````

## tsconfig.jsonの更新
````json
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "baseUrl": "src"
  },
  "include": [
    "next-env.d.ts",
    "src/**/*.ts",
    "src/**/*.tsx"
  ],
  "exclude": [
    "node_modules"
  ]
}

````

## tsconfig.jsonの解説

| 項目 | 役割 |
| ---- | ---- |
| tacompilerOptionsget | TypeScript コンパイラの設定が含まれるセクションです。 |
| target | コンパイルされる JavaScript のバージョンを指定します。この場合は ECMAScript 5 (es5) |
| lib | コンパイル時に使用可能なライブラリのリストです。dom、dom.iterable、esnext が含まれています。 |
| allowJs | JavaScript ファイルもコンパイルの対象とするかどうかを指定します。 |
| skipLibCheck | ライブラリの型チェックをスキップするかどうかを指定します。 |
| strict | 厳格な型チェックを有効にします。 |
| forceConsistentCasingInFileNames | ファイル名の一貫性を強制するかどうかを指定します。 |
| noEmit | 実際に JavaScript ファイルを生成しないようにします。 |
| esModuleInterop | CommonJS モジュールと ES6 モジュールの相互運用性を改善するための設定です。 |
| module | モジュールのコード生成方式を指定します。ここでは ES6 モジュールを指定しています。 |
| moduleResolution | モジュール解決の方法を指定します。ここでは Node.js の方式を使用しています。 |
| resolveJsonModule | JSON ファイルを import 文で読み込むことを可能にするかどうかを指定します。 |
| isolatedModules | ファイルごとに個別の型チェックとコンパイルを行うかどうかを指定します。 |
| jsx | JSX ファイルの扱いを指定します。ここでは "preserve" としています。 |
| incremental | 増分ビルドを有効にするかどうかを指定します。 |
| baseUrl | 増分ビルドを有効にするかどうかを指定します。 |
| include | コンパイルの対象となるファイルやディレクトリのリストです。next-env.d.ts、src/**/*.ts、src/**/*.tsx が含まれています。 |
| exclude | コンパイルから除外するファイルやディレクトリのリストです。ここでは "node_modules" が除外されています。 |

## tsconfig.jsonの動作確認
1. 下記コマンドで起動する
````bash
cd frontend
npm run dev
````

````bash
----- 出力結果 -----
$ npm run dev

> frontend@0.1.0 dev
> next dev

   ▲ Next.js 14.0.4
   - Local:        http://localhost:3000

 ✓ Ready in 9.6s
````
2. [ローカル環境](http://localhost:3000)にアクセスする
3. 下記画像のような画面が確認出来る
![create-project-step01](/images/books/nextjs-ts-tutorial/create-project/create-project-step01.png)