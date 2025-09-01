---
title: '[AI] GitHub Copilotの基本的な使い方' # 記事のタイトル
emoji: '🧠' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['ai', 'vscode', 'githubcopilot', 'contest2025ts', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## 🌱 はじめに

今回は、会社で`GitHub Copilot`が使えるようになったため、
`GitHub Copilot`の操作や使い方を解説します。
サンプルコードとして`TypeScript`を使用します。

## 🌱 拡張機能のインストール

`VScode`の拡張機能の検索窓に`GitHub Copilot`と検索し、インストールする。

![5](/images/articles/ai-github-copilot/5.png)

## 🌱 1. コード補完: コメントからコードを提案をしてもらう。

下記のようなコメントだけを書きました。

```ts
// 配列の合計値を算出する
```

しばらくすると、AI がコメントを確認してくれてコードを提案してくれます。
![2](/images/articles/ai-github-copilot/2.png)

## 🌱 2. コード補完: 提案されたコードを全部採用する。

下記のような提案をしてくれたコードを全て採用する場合は、`Tab キー`を実行します。
![2](/images/articles/ai-github-copilot/2.png)

## 🌱 3. コード補完: 提案して貰ったコードの一部を採用する。

下記コマンドで`次の単語まで採用する`
`export function sumArray`まで採用してみました。
:::message

**Win**: `Ctrl + →`
**Mac**: `command + →`

:::

![3](/images/articles/ai-github-copilot/3.png)

## 🌱 4. コード補完: 他の提案を提示して貰う。

下記コマンドで`他の提案をして貰いました。`
`if文`を追加された別の提案をしてくれました。
:::message

**Win**: `Alt + ]`
**Mac**: `option + ]`

:::

![4](/images/articles/ai-github-copilot/4.png)

## 🌱 チャット AI の使い方

1. `VSCode`の画面上段のアイコンをクリックすると、画面右側にチャット欄が表示される。

![1](/images/articles/ai-github-copilot/1.png)

2. モードを選択する

| モード    | 説明                                                        | 使い道                                                                                                     |
| --------- | ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Ack**   | AI がコードについての質問に答えてくれる                     | コードのロジックがどう動いているのかを聞く、設計の壁打ちなど。                                             |
| **Edit**  | AI がコードを編集してくれる                                 | 機能追加、バグ修正、リファクタリングなど。                                                                 |
| **Agent** | AI が自律的にコーディングやコマンド実行などを実行してくれる | 新機能の追加を自律的に実装させる。コードに問題が発見されると都度その問題を解決しながら実装を進めてくれる。 |

![7](/images/articles/ai-github-copilot/7.png)

AI のモデルを選択できます。
モデルを変更する事によって AI の賢さが変更されます。
※モデルによって利用金額等が変わります。

![6](/images/articles/ai-github-copilot/6.png)
