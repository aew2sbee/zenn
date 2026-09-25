---
title: "[Claude Code] 任意のAPIキーを設定する" # 記事のタイトル
emoji: "🧠" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["claudecode", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、Claude Code に任意の`ANTHROPIC_API_KEY`（API キー）を設定する方法を解説します。
`ANTHROPIC_API_KEY` は、Claude Code が Anthropic の API を呼び出すときに使う認証キーで、利用料金はこのキーの発行元に請求されます。

会社から配布された API キーを設定する方法に戸惑ったので記事にしました。
執筆時点（2025年）では、[公式ドキュメント](https://code.claude.com/docs/ja/setup#authenticate)の手順どおりにログインすると、API キーが自動で作成・設定されてしまい、任意の API キーを設定できませんでした。

:::message
前提: Claude Code をインストール済みであること。
スクリーンショットは執筆時点（2025年、WSL 環境）のものです。
:::

@[card](https://code.claude.com/docs/ja/settings)
@[card](https://code.claude.com/docs/ja/authentication)

## 🌱 結論

:::message
自分だけが使う設定ファイル（`~/.claude/settings.json` など）の `env` に `ANTHROPIC_API_KEY` を設定し、Claude Code の起動時に表示される確認で `1. Yes` を選ぶ
:::

## 🌱 1. 設定ファイルに API キーを書く

設定ファイルは、置き場所によって適用範囲が変わります。

| 置き場所 | 適用範囲 |
| --- | --- |
| `~/.claude/settings.json` | 全プロジェクト（自分だけ） |
| `<プロジェクト>/.claude/settings.local.json` | そのプロジェクト（自分だけ、Git 管理外） |
| `<プロジェクト>/.claude/settings.json` | そのプロジェクト（Git で共有される） |

:::message alert
`<プロジェクト>/.claude/settings.json` は、Git にコミットしてチームで共有するための設定ファイルです。ここに API キーを書くと、リポジトリ経由でキーが漏えいするおそれがあります。
API キーは `~/.claude/settings.json` か `.claude/settings.local.json` に書いてください。`.claude/settings.local.json` を手動で作った場合は、`.gitignore` に追加されているかも確認してください。
誤ってコミットした場合は、すぐにキーを無効化して再発行してください。
:::

ファイルが存在しない場合は、下記の内容で新規作成してください。`YOUR_API_KEY` は自分の API キーに置き換えます。

```json:~/.claude/settings.json
{
  "env": {
    "ANTHROPIC_API_KEY": "YOUR_API_KEY"
  }
}
```

すでにファイルがある場合は、下記のように `env` を追加してください（直前の項目の末尾にカンマを付けます）。
`env` に書いた値は、Claude Code の起動時に環境変数として読み込まれます。

```diff json:~/.claude/settings.json
{
  --- 略 ---
+  "env": {
+    "ANTHROPIC_API_KEY": "YOUR_API_KEY"
+  }
}
```

## 🌱 2. Claude Code を起動する

下記のコマンドを実行して Claude Code を起動します。
`.claude/settings.local.json` に書いた場合は、そのプロジェクトのディレクトリで実行してください。

```bash
claude
```

Claude Code を起動すると、下記の画面が表示されます。

![環境変数のAPIキーを使うかどうかを確認する画面](/images/articles/claude-code-api-setting/step1.png)

```text
Do you want to use this API key?
❯ 1. Yes
  2. No (recommended)✔
```

「このAPIキーを使用しますか？」という確認です。推奨は `No` になっていますが、これは意図せず設定されたキーを使わないための安全策です。
今回は自分で設定したキーを使いたいので、↑キーで `1. Yes` に合わせて Enter を押します。

:::message
この選択は記憶され、次回からは表示されません。後から変更したい場合は、`/config` の「Use custom API key」で切り替えられます。
:::

## 🌱 3. 設定された API キーを確認する

`1. Yes` を選択すると、下記の画面が表示されます。
「Overrides (via env)」の欄に `API Key: sk-ant-…` と表示されていれば、設定した API キーが使われています。

![設定したAPIキーが使われていることを示す起動画面](/images/articles/claude-code-api-setting/step2.png)

以前に公式手順でログインしている場合は、下記の警告も表示されます。

```text
⚠ Auth conflict: Using ANTHROPIC_API_KEY instead of Anthropic Console key. Either unset ANTHROPIC_API_KEY, or run `claude /logout`.
```

「ログインで作られたキー（Console のキー）ではなく、`ANTHROPIC_API_KEY` を使っている」という意味なので、意図どおりの動作です。警告を消したい場合は `/logout` を実行します。

現在のバージョンでは、`/status` を実行すると、どの認証方式が使われているかを確認できます。

## 🌱 おわりに

- API キーは `~/.claude/settings.json` などの `env` に `ANTHROPIC_API_KEY` として設定する
- チームで共有する `.claude/settings.json` には API キーを書かない
- 起動時の確認で `1. Yes` を選び、`/status` などで使われている認証方式を確認する
