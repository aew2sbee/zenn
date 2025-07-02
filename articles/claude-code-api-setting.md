---
title: '[Claude Code] 任意のAPIを設定する' # 記事のタイトル
emoji: '🧠' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['claudecode', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**Claude Code の任意の`ANTHROPIC_API_KEY`を設定方法**解説します

会社から配布された API の設定方法に戸惑ったので記事にしました。
[公式ドキュメントの手順通り](https://docs.anthropic.com/ja/docs/claude-code/setup#%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%A8%E8%AA%8D%E8%A8%BC)だと勝手に API が作成、設定され任意の API を設定できなかったです。

:::details 参考資料
@[card](https://docs.anthropic.com/ja/docs/claude-code/settings#%E8%A8%AD%E5%AE%9A%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB)
:::

## 結論

1. `~/.claude/settings.json`を作成する
2. 必要な`key`, `value`を設定する

```json: .claude/settings.json
{
  "env": {
    "ANTHROPIC_API_KEY": "sk-ant-api03-XXXXXXXXXXXXXXXXXXXXX"
  }
}
```

## 1. 一度ログアウトする

`~/.claude/settings.json`を作成、設定を行うと
下記のようなメッセージが表示されると思います。

`claude /logout`でログアウトをしてください。

:::message
注意喚起のメッセージが表示される

```bash

⚠ Auth conflict: Using ANTHROPIC_API_KEY instead of Anthropic Console key.
Either unset ANTHROPIC_API_KEY, or run `claude /logout`.

--- 日本語訳 ---
認証の衝突： Anthropicコンソールのキーの代わりにANTHROPIC_API_KEYを使用しています。
ANTHROPIC_API_KEYの設定を解除するか、`claude /logout`を実行してください。
```

:::

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
