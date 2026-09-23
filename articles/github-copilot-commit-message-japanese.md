---
title: "[GitHub Copilot] コミットメッセージの生成を日本語にする" # 記事のタイトル
emoji: "🐙" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["githubcopilot", "vscode", "github", "git", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

次の画像のとおり、`GitHub Copilot` がコミットメッセージを生成してくれるようになりました。
とても便利なのですが、たまに英語で生成されることがあります。
コミットメッセージはあとで見返すときに重要なので、他のメンバーにもわかりやすいよう日本語で残しておきたいです。
そこで、日本語で生成する設定を見つけたので解説します。

![VS Codeのソース管理パネルにあるコミットメッセージ生成ボタン](/images/articles/github-copilot-commit-message-japanese/generate-commit-message-button.png)

## 🌱 結論

:::message
`settings.json` に `github.copilot.chat.commitMessageGeneration.instructions` を追加します。
日本語で生成するよう指示できます（AI への指示なので、必ず日本語になるとは限りません。生成後に内容を確認してください）。
:::

```json:settings.json
{
  "github.copilot.chat.commitMessageGeneration.instructions": [
    { "text": "コミットメッセージは日本語で記述してください。" }
  ]
}
```

## 🌱 手順

### 1. settings.json を開く

設定を書く場所は、適用したい範囲によって選びます。

| 設定 | ファイル | 適用範囲 |
| --- | --- | --- |
| ワークスペース設定 | `<リポジトリのルート>/.vscode/settings.json` | そのリポジトリだけ（コミットすればチームで共有できる） |
| ユーザー設定 | コマンドパレットの「Preferences: Open User Settings (JSON)」で開く | すべてのリポジトリ |

両方に書いた場合は、ワークスペース設定が優先されます。

ワークスペース設定は、コマンドパレットの「Preferences: Open Workspace Settings (JSON)」で開けます（ファイルがない場合は作成されます）。

```text
<リポジトリのルート>/
└── .vscode/
    └── settings.json
```

### 2. settings.json に設定を追加する

下記の内容を追加します。
すでに設定が書かれている場合は、既存の `{ }` の中にキーと値だけを追記してください（直前の項目の末尾にカンマが必要です）。

```json:settings.json
{
  "github.copilot.chat.commitMessageGeneration.instructions": [
    { "text": "コミットメッセージは必ず日本語で、Conventional Commits形式（型は feat/fix/docs/refactor/test/chore を使用）で記述してください。" }
  ]
}
```

:::message
内容はあくまでもサンプルです。
上記では、日本語に加えて Conventional Commits 形式も指定しています。ご自身の好きな内容にカスタマイズしてください。
:::

:::message alert
2026年5月以降、この設定を入れるとコミットメッセージが生成されない、または 422 エラーになるという不具合が報告されています（[microsoft/vscode#316204](https://github.com/microsoft/vscode/issues/316204)）。
同じ症状が出る場合は、一時的にこの設定を外してください。
:::

## 🌱 参考資料

https://code.visualstudio.com/docs/agent-customization/custom-instructions
