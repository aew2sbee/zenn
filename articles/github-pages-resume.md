---
title: "[GitHub] 職務経歴書を公開する" # 記事のタイトル
emoji: "🐙‍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["git", "github", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---


## はじめに
この記事では、GitHub Pagesで下記の画像のような**職務経歴書を公開する方法**を解説します。
:::details 参考資料
@[card](https://zenn.dev/choimake/articles/9809c6dbfc43a4)
@[card](https://zenn.dev/ryo_f/articles/2f925f621e6d99)
:::

## 1. 必要なディレクトリーやファイルを作成する
### 1. トップページの作成
```bash
mkdir -p src/ && touch src/index.html
```

:::details src/index.htmlのコード

:::

### 2. 各プロジェクトの詳細ページの作成
:::message
参画したプロジェクトの数だけこのhtmlファイルを作成してください
:::
```bash
mkdir -p src/project && touch src/project/001.html

```

:::details src/project/001.htmlのコード

:::

### 3. Tailwind CSSに必要なファイルを作成

```bash
mkdir -p src/assets/css && touch src/assets/css/input.css
```


```css:src/assets/css/input.css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

```bash
npx tailwindcss -i ./src/assets/css/input.css -o ./src/assets/css/tailwind.css --watch
```



### さいご

:::message
私が作成したフォーマットはご自由にお使いいただけます。
ご使用の際には、この記事に **「いいね！」** や **「シェア」** をいただけると励みになります！
:::
