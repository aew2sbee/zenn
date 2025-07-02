---
title: '[Claude Code] 任意のAPIを設定する' # 記事のタイトル
emoji: '🧠' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['claudecode', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**Claude Code の任意の`ANTHROPIC_API_KEY`を設定方法**解説します

会社から配布された API を設定する方法に戸惑ったので記事にしました。
[公式ドキュメントの手順通り](https://docs.anthropic.com/ja/docs/claude-code/setup#%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%A8%E8%AA%8D%E8%A8%BC)だと勝手に API が作成、設定され任意の API を設定できなかったです。

:::details 参考資料
@[card](https://docs.anthropic.com/ja/docs/claude-code/settings#%E8%A8%AD%E5%AE%9A%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB)
:::

## 結論

:::message
`.claude/settings.json`に`ANTHROPIC_API_KEY`を設定する

```diff json: .claude/settings.json
{
  --- 略 ---
+  "env": {
+    "ANTHROPIC_API_KEY": "sk-ant-api03-XXXXXXXXXXXXXXXXXXXXX"
+  }
}
```

:::

## 1. `.claude/settings.json`を編集する

:::message alert
`.claude/settings.json`が存在しない場合は、新規作成する
:::

下記のように`ANTHROPIC_API_KEY`に設定したい`API KEY`を追加してください

```diff json: .claude/settings.json
{
  --- 略 ---
+  "env": {
+    "ANTHROPIC_API_KEY": "sk-ant-api03-XXXXXXXXXXXXXXXXXXXXX"
+  }
}
```

## 2. Claude Code を起動する

下記コマンドを実行して Claude Code を起動する

```bash
claude
```

Claude Code を起動すると下記画面が表示する
![step1](/images/articles/claude-code-api-setting/step1.png)

`1.Yes`を選択する

```bash
// このAPIキーを使用しますか？
Do you want to use this API key?
❯ 1.Yes
  2. No (recommended)✔
```

## 3. 設定された API を確認する

先ほどの`1.Yes`を選択すると、現在設定されている`API Key`が表示されます。
下記のように表示されているので設定が出来ていますね

```bash
// 認証の衝突： Anthropicコンソールのキーの代わりにANTHROPIC_API_KEYを使用しています。
⚠ Auth conflict: Using ANTHROPIC_API_KEY instead of Anthropic Console key.
```

![step2](/images/articles/claude-code-api-setting/step2.png)

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
