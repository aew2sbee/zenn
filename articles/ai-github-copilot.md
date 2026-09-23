---
title: "[AI] GitHub Copilotの基本的な使い方" # 記事のタイトル
emoji: "🧠" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["ai", "vscode", "githubcopilot", "contest2025ts", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

今回は、会社で`GitHub Copilot`が使えるようになったため、その基本的な使い方を解説します。

`GitHub Copilot`は、GitHub が提供する AI コーディング支援ツールです。
この記事では、下記の 2 つの機能を扱います。

- **コード補完**: 書いている途中のコードの続きを AI が提案する機能
- **チャット**: AI に質問したり、コードの編集を依頼したりする機能

サンプルコードとして`TypeScript`を使用します。

:::message alert
`GitHub Copilot`は更新が非常に速く、掲載しているスクリーンショットは執筆時点のものです。
その後、拡張機能の提供形態・チャットのモード構成・課金方式がいずれも変更されています。
操作の考え方は大きく変わっていませんが、実際の画面や名称は[公式ドキュメント](https://code.visualstudio.com/docs/copilot/overview)で確認してください。
:::

## 🌱 前提条件

:::message
- `VSCode`がインストール済みであること
- GitHub アカウントでサインインしていること

サインインすれば無料プラン（GitHub Copilot Free）でも利用できますが、月あたりの利用回数に上限があります。
会社のアカウントで使う場合は、組織側での利用者の割り当てが必要です。
詳細は[GitHub Copilot のプラン](https://docs.github.com/ja/copilot/get-started/plans)を確認してください。
:::

## 🌱 拡張機能のインストール

`VSCode`の拡張機能の検索窓に`GitHub Copilot`と入力して検索し、インストールします。

![GitHub Copilot拡張機能のインストール画面](/images/articles/ai-github-copilot/5.png)

:::message alert
現在の`VSCode`では`GitHub Copilot`が標準搭載されているため、**この手順は不要**です。
拡張機能を個別にインストールするのではなく、ステータスバーの Copilot アイコンからサインインして利用を開始します。
最新の手順は[VS Code のセットアップ手順](https://code.visualstudio.com/docs/copilot/setup)を確認してください。
:::

## 🌱 コード補完の使い方

### コメントからコードを提案してもらう

下記のようなコメントだけを書いて、`Enter`で改行します。

```ts
// 配列の合計値を算出する
```

しばらくすると、AI がコードを提案します。
このとき、AI はコメントだけでなく、編集中のファイルや開いている他のファイルの内容も参考にしています。

![コメントからコードが提案された画面](/images/articles/ai-github-copilot/2.png)

提案は**グレーの文字**（ゴーストテキスト）で表示されます。
この時点ではまだファイルに入力されていません。

提案されたコードをすべて採用する場合は、`Tab`キーを押します。
グレーの文字が通常の文字色に変われば採用完了です。

今回は下記のようなコードが提案されました。

```ts
// 配列の合計値を算出する
export function sumArray(numbers: number[]): number {
  return numbers.reduce((sum, n) => sum + n, 0);
}
```

:::message
AI の提案は毎回同じとは限らないため、上記とは異なるコードが表示されることがあります。
提案が表示されない場合は、サインイン状態と、画面右下のステータスバーにある Copilot アイコンの状態を確認してください。
:::

### 提案の一部だけを採用する

下記のキー操作で、次の単語まで採用できます。
1 回押すごとに 1 単語ずつ進むため、複数回押して`export function sumArray`まで採用しました。

:::message
**Win**: `Ctrl + →`
**Mac**: `command + →`
:::

![提案の一部だけを採用した画面](/images/articles/ai-github-copilot/3.png)

### 他の提案を表示する

下記のキー操作で、他の提案を表示できます。
`if文`が追加された別の提案が表示されました。

:::message
**次の提案**
**Win**: `Alt + ]` / **Mac**: `option + ]`

**前の提案**
**Win**: `Alt + [` / **Mac**: `option + [`
:::

![if文が追加された別の提案が表示された画面](/images/articles/ai-github-copilot/4.png)

:::message alert
提案されたコードが必ず正しいとは限りません。採用する前に、自分で内容を確認しましょう。
:::

## 🌱 チャット AI の使い方

### チャット欄を開く

`VSCode`の画面上部にある Copilot のチャットアイコンをクリックすると、画面右側にチャット欄が表示されます。

![チャット欄を開いた画面](/images/articles/ai-github-copilot/1.png)

### モードを選択する

チャット入力欄にあるモード切り替えから、用途に応じたモードを選択します。

![モードを選択する画面](/images/articles/ai-github-copilot/7.png)

| モード | 説明 | 使い道 |
| --- | --- | --- |
| **Ask** | AI がコードに関する質問に回答する | コードのロジックがどう動いているのかを聞く、設計の壁打ち |
| **Edit** | 指示した内容に沿って AI がコードを書き換える。変更内容は採用・却下を選べる | 機能追加、バグ修正、リファクタリング（動作を変えずにコードを整理すること） |
| **Agent** | 必要なファイルの特定やコマンド実行まで AI が判断して進める。エラーが発生した場合は、内容を読み取って修正を試みる | 新機能を自律的に実装させる |

:::message
Agent モードでも、ファイルの編集やコマンドの実行は、原則としてユーザーの承認を挟んだうえで実行されます。
:::

:::message alert
上記は執筆時点のモード構成です。
その後 Edit モードは廃止され、差分編集は Agent に統合されました。現在は計画を立てる Plan モードが加わっています。
最新のモード構成は[チャットモードのドキュメント](https://code.visualstudio.com/docs/copilot/chat/chat-modes)を確認してください。
:::

### モデルを選択する

AI のモデルを選択します。
モデルとは AI の種類のことで、得意分野や応答速度が異なります。

![モデルを選択する画面](/images/articles/ai-github-copilot/6.png)

:::message
モデルによって、月ごとの利用枠の消費量が変わります。
課金の仕組みは変更されているため、最新の内容は[GitHub Copilot の課金ドキュメント](https://docs.github.com/ja/copilot/concepts/billing)を確認してください。
なお、コード補完はこの利用枠を消費せず、すべてのプランに含まれています。

また、表示されるモデルは契約プランや組織の設定によって異なります。無料プランではモデルを選べない場合があります。
:::

## 🌱 おわりに

コード補完のキー操作をまとめます。

| 操作 | Win | Mac |
| --- | --- | --- |
| 提案をすべて採用する | `Tab` | `Tab` |
| 次の単語まで採用する | `Ctrl + →` | `command + →` |
| 次の提案を表示する | `Alt + ]` | `option + ]` |
| 前の提案を表示する | `Alt + [` | `option + [` |

チャットは、コードについて聞きたいときは **Ask**、実装そのものを任せたいときは **Agent** と使い分けます。

## 🌱 参考

- [GitHub Copilot のドキュメント](https://docs.github.com/ja/copilot)
- [GitHub Copilot in VS Code](https://code.visualstudio.com/docs/copilot/overview)
- [GitHub Copilot のキーボードショートカット](https://docs.github.com/ja/copilot/reference/keyboard-shortcuts)
