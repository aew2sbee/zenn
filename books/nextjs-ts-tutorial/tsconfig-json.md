---
title: '[事前準備] tsconfig.jsonの設定'
---

## tsconfig.jsonとは
> TypeScriptプロジェクトの設定ファイルであり、TypeScriptコンパイラに対してプロジェクトのビルド方法やコンパイルオプションを指示するためのものです。

## tsconfig.jsonの作成
下記コマンドでデフォルトで作成する事が出来ます。
````bash
tsc --init
````

## tsconfig.jsonの更新
````json
{
  "compilerOptions": {
    "target": "es5",  // JavaScriptのコンパイルターゲットを指定
    "lib": [  // コンパイルに使用するライブラリを指定
      "dom",
      "dom.iterable",
      "esnext"
    ],                                        
    "allowJs": true,  // JavaScriptファイルのコンパイルを許可
    "skipLibCheck": true,  // ライブラリの型チェックをスキップ
    "strict": true,  // 厳格な型チェックを有効にする
    "forceConsistentCasingInFileNames": true,  // ファイル名の一貫性を強制
    "noEmit": true,  // JavaScriptファイルの実際の出力を抑制
    "esModuleInterop": true,  // ESモジュールとInterop可能な形式の相互運用性を有効にする
    "module": "esnext",  // モジュールの形式を指定
    "moduleResolution": "node",  // モジュール解決の方法を指定
    "resolveJsonModule": true,  // JSONモジュールの解決を有効にする
    "isolatedModules": true,  // ファイルごとにコンパイルを行う（incremental: true と一緒に使用）
    "jsx": "preserve",  // JSXの処理方法を指定
    "incremental": true,  // 増分コンパイルを有効にする
    "baseUrl": "src"  // モジュール解決の基準となるルートディレクトリを指定
  },
  "include": [  // コンパイル対象のファイルやディレクトリを指定
    "next-env.d.ts",
    "src/**/*.ts",
    "src/**/*.tsx"
  ],
  "exclude": [  // コンパイル対象から除外するファイルやディレクトリを指定
    "node_modules"
  ]
}

````