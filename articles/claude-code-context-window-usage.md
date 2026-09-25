---
title: "[Claude Code] コンテキストウィンドウ使用量をリアルタイムで把握する方法" # 記事のタイトル
emoji: "🧠" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["claudecode", "初心者向け", "コンテキストウィンドウ"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、Claude Code で会話中に随時**コンテキストウィンドウの使用量を把握する**方法を解説します。

コンテキストウィンドウとは、Claude が一度に参照できる会話やファイル内容の上限量で、トークン（AI が文章を処理する単位）の数で数えます。
上限に近づくと古い内容が要約されるため、使用量を見ながら `/compact` や `/clear` を使うタイミングを判断できると便利です。

@[card](https://code.claude.com/docs/ja/statusline)
@[card](https://ccusage.com/guide/statusline)

## 🌱 結論

:::message
Claude Code の設定ファイルに `statusLine` を設定し、`ccusage` の出力を表示します。
下記のように、**コンテキストウィンドウ使用量**を含めた情報がステータスラインに表示されます。
:::

```text
🤖 Opus 4.5 | 💰 $0.36 session / $0.35 today / $0.35 block (4h 41m left) | 🔥 $3.13/hr | 🧠 26,763 (13%)
```

## 🌱 前提条件

- Claude Code がインストール済みであること
- Node.js（`npx` コマンド）が使えること

`ccusage` は、Claude Code のローカルのログから使用量やコストを集計するサードパーティ製の CLI ツールです（Anthropic の公式ツールではありません）。
`npx -y` は、パッケージをインストールせず、確認を省略してその場で実行するオプションです。

## 🌱 1. 設定ファイルに `statusLine` を追加する

`statusLine` は、指定したコマンドの出力を入力欄の下に常時表示する機能です。
設定ファイルは、置き場所によって適用範囲が変わります。

| 置き場所 | 適用範囲 |
| --- | --- |
| `~/.claude/settings.json` | 全プロジェクト（自分だけ） |
| `<プロジェクト>/.claude/settings.local.json` | そのプロジェクト（自分だけ、Git 管理外） |
| `<プロジェクト>/.claude/settings.json` | そのプロジェクト（Git で共有され、他のメンバーにも適用される） |

今回は個人の表示設定なので、`~/.claude/settings.json` に書くことをおすすめします。

ファイルが存在しない場合は、下記の内容で新規作成してください。

```json:~/.claude/settings.json
{
  "statusLine": {
    "type": "command",
    "command": "npx -y ccusage statusline",
    "padding": 0
  }
}
```

すでにファイルがある場合は、下記のように `statusLine` を追加してください（直前の項目の末尾にカンマを付けます）。

```diff json:~/.claude/settings.json
{
  --- 略 ---
+  "statusLine": {
+    "type": "command",
+    "command": "npx -y ccusage statusline",
+    "padding": 0
+  }
}
```

:::message alert
`npx -y` は、ステータスラインを更新するたびに npm からパッケージを取得して実行します。バージョンを固定していないため、悪意のある版が公開された場合もそのまま実行されてしまいます。
気になる場合は、`npx -y ccusage@<バージョン> statusline` のようにバージョンを固定するか、`npm install -g ccusage` でインストールしてから `ccusage statusline` を指定してください。
:::

## 🌱 2. Claude Code を起動（再起動）する

下記のコマンドを実行して Claude Code を起動します。
起動中の場合は、`/exit` で一度終了してから再度起動してください。

```bash
claude
```

初回は `npx` がパッケージをダウンロードするため、表示まで少し時間がかかります。
表示されない場合は、メッセージを 1 回送るか、`npx -y ccusage statusline` を単体で実行してエラーが出ないか確認してください。

## 🌱 各項目の説明

```text
🤖 Opus 4.5 | 💰 $0.36 session / $0.35 today / $0.35 block (4h 41m left) | 🔥 $3.13/hr | 🧠 26,763 (13%)
```

| 項目 | 表示例 | 説明 |
|------|--------|------|
| 🤖 **Model** | `Opus 4.5` | 現在使用中の Claude モデル（使用中のモデルによって変わる） |
| 💰 **Session** | `$0.36` | 現在のセッション（会話）での累計コスト |
| 💰 **Today** | `$0.35` | 本日の累計コスト |
| 💰 **Block** | `$0.35 (4h 41m left)` | 現在の 5 時間ブロック（利用枠の区切り）での使用額と、リセットまでの残り時間 |
| 🔥 **Burn rate** | `$3.13/hr` | 現在の消費ペース（時間あたりのコスト） |
| 🧠 **Tokens** | `26,763 (13%)` | 現在のコンテキストウィンドウ使用量（トークン数と、モデルのコンテキスト上限に対する割合） |

:::message
コストは、ローカルのログから API 料金に換算した推定値です。Pro/Max などのサブスクリプションプランを使っている場合、実際の請求額ではありません。
:::

## 🌱 まとめ

- `statusLine` に `npx -y ccusage statusline` を設定すると、🧠 欄でコンテキストウィンドウの使用量を確認できる
- 個人の設定は `~/.claude/settings.json` に書く
- 🧠 欄の割合が高くなってきたら、`/compact` や `/clear` を検討する
