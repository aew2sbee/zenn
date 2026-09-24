---
title: "[Playwright] アクセシビリティツリー(accessibility)を触ってみた" # 記事のタイトル
emoji: "🎭" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["playwright", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

`Playwright MCP`がすごい！って聞いたので`Playwright`の`accessibility`を検証してみました。
その検証結果を解説します。

@[card](https://github.com/microsoft/playwright-mcp)

### 結論

Zenn のトップページでアクセシビリティツリーを可視化した結果はこちらになります。

:::message

```ts
{
  "role": "WebArea",
  "name": "Zenn｜エンジニアのための情報共有コミュニティ",
  "children": [
    {
      "role": "link",
      "name": "Zenn | エンジニアのための情報共有コミュニティ",
      "value": undefined,
      "checked": undefined,
      "pressed": undefined,
      "children": undefined
    },
    {
      "role": "link",
      "name": "検索",
      "value": undefined,
      "checked": undefined,
      "pressed": undefined,
      "children": undefined
    }
    ... more
  ],
  "value": undefined,
  "checked": undefined,
  "pressed": undefined
}
```

:::

### 前提

検証時のバージョンは下記の通りです。

```bash
"playwright": "^1.51.1"
"typescript": "^5.8.2"
```

:::message alert
本記事で使っている `page.accessibility.snapshot()` は、Playwright の公式ドキュメントで非推奨（Deprecated）とされています。
現在は、ARIA スナップショット（`locator.ariaSnapshot()`）を使う方法が推奨されています。
:::

:::message
実行結果の `role` は要素の役割（ARIA ロール。`WebArea` などの一部はブラウザ固有）、`name` はアクセシブルネーム（スクリーンリーダーなどが読み上げる名前）です。
実行結果に含まれる Zenn のユーザー名は、`user_a` のようなダミーに置き換えています。
:::

## 🌱 やり方

下記コードを実行します。

### 1. accessibilityTree を取得

```ts
import { chromium } from "playwright";

(async () => {
  const browser = await chromium.launch();
  const page = await browser.newPage();
  await page.goto("https://zenn.dev/");
  await page.waitForTimeout(1 * 1000);

  const accessibilityTree = await page.accessibility.snapshot();
  console.log(accessibilityTree);

  await browser.close();
})();
```

Playwright MCP も、スクリーンショット（画像）ではなくアクセシビリティ情報をテキストとして扱います。確かに、ブラウザ情報をデータ化すれば、画像認識に頼るより処理速度も正確性も向上しますね。
（なお、Playwright MCP 自体は ARIA スナップショットの仕組みを使っており、本記事の API とは別物です）

:::details 実行結果を確認する

```bash
$ node playwright-mcp.js
{
  role: 'WebArea',
  name: 'Zenn｜エンジニアのための情報共有コミュニティ',
  children: [
    {
      role: 'link',
      name: 'Zenn | エンジニアのための情報共有コミュニティ',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '検索',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'button',
      name: 'Log in',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'Trending',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'Explore',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '第２回 AI Agent Hackathon with Google Cloud',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: 'ハッカソン',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '第２回 AI Agent Hackathon with Google Cloud',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '期間: 2025/04/14 ~ 2025/06/30',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'heading',
      name: 'Tech',
      level: 3,
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'generic',
      name: '詳細を確認する',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: 'プログラミングなどの技術についての知見',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'REST API 設計指針・セキュリティ編',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_a',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_a',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '3日前',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'image',
      name: 'いいねされた数',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '84',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'あなたのUI/UXを上げる "アニメーション" の基礎',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'SODA Engineering Blog',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_b',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_b',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: 'in',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'SODA Engineering Blog',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '2日前',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'image',
      name: 'いいねされた数',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '97',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'MCP サーバーを自作して GitHub Copilot の Agent に可読性の低いクラス名を作ってもらう',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'Microsoft (有志)',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_c',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_c',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: 'in',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'Microsoft (有志)',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '2日前',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'image',
      name: 'いいねされた数',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '21',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '「AIワークフロー」で実現する論文調査の自動化',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '松尾研究所テックブログ',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_d',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_d',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: 'in',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '松尾研究所テックブログ',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '2日前',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'image',
      name: 'いいねされた数',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '23',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'Cline(Roo Code)を暴走列車にしたら4日間で数ヶ月分のコードが生成できた',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_e',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_e',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '4日前',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'image',
      name: 'いいねされた数',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '398',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '簡易な自作MCPサーバーをお試しで実装する方法',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'スマートラウンド テックブログ',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_f',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_f',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: 'in',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'スマートラウンド テックブログ',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '23時間前',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'image',
      name: 'いいねされた数',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '22',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '【Gemini】GPU不要！超軽量TTSとLLMを使ったチャットWebサービスの構築 ～ UTAU収録音声を用いたTTS ～',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_g',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_g',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '3日前',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'image',
      name: 'いいねされた数',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '57',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'KANNAのAWSインフラ基盤リプレースの舞台裏',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'アルダグラム Tech Blog',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_h',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_h',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: 'in',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'アルダグラム Tech Blog',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '1日前',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'image',
      name: 'いいねされた数',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '19',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '実行計画を元にした Spanner クエリ最適化の実践',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_i',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_i',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '2日前',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'image',
      name: 'いいねされた数',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '31',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'Mastraで実装するマルチエージェントAIワークフロー ─ ブログ生成プロセスを3ステップで自動化�',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_j',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_j',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '2日前',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'image',
      name: 'いいねされた数',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'text',
      name: '42',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: '',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'tj-actions のインシデントレポートを読んだ',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    {
      role: 'link',
      name: 'user_k',
      value: undefined,
      checked: undefined,
      pressed: undefined,
      children: undefined
    },
    ... 330 more items
  ],
  value: undefined,
  checked: undefined,
  pressed: undefined
}
```

:::

### 2. データ加工

```ts
// roleがlinkでフィルターをかける
console.log(accessibilityTree.children.filter((i) => i.role === "link"));
```

:::details 実行結果を確認する

```bash
[
  {
    "role": "link",
    "name": "Zenn | エンジニアのための情報共有コミュニティ",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "検索",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "Trending",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "Explore",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "第２回 AI Agent Hackathon with Google Cloud",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "第２回 AI Agent Hackathon with Google Cloud",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "REST API 設計指針・セキュリティ編",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_a",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_a",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "あなたのUI/UXを上げる \"アニメーション\" の基礎",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "SODA Engineering Blog",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_b",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_b",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "SODA Engineering Blog",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "MCP サーバーを自作して GitHub Copilot の Agent に可読性の低いクラス名を作ってもらう",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "Microsoft (有志)",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_c",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_c",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "Microsoft (有志)",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "「AIワークフロー」で実現する論文調査の自動化",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "松尾研究所テックブログ",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_d",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_d",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "松尾研究所テックブログ",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "Cline(Roo Code)を暴走列車にしたら4日間で数ヶ月分のコードが生成できた",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_e",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_e",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "簡易な自作MCPサーバーをお試しで実装する方法",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "スマートラウンド テックブログ",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_f",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_f",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "スマートラウンド テックブログ",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "【Gemini】GPU不要！超軽量TTSとLLMを使ったチャットWebサービスの構築 ～ UTAU収録音声を用いたTTS ～",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_g",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_g",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "KANNAのAWSインフラ基盤リプレースの舞台裏",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "アルダグラム Tech Blog",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_h",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_h",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "アルダグラム Tech Blog",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "実行計画を元にした Spanner クエリ最適化の実践",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_i",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_i",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "Mastraで実装するマルチエージェントAIワークフロー ─ ブログ生成プロセスを3ステップで自動化�",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_j",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_j",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "tj-actions のインシデントレポートを読んだ",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  },
  {
    "role": "link",
    "name": "user_k",
    "value": undefined,
    "checked": undefined,
    "pressed": undefined,
    "children": undefined
  }
]

```

:::

## 🌱 備考

各 OS には独自のアクセシビリティ API があり、ブラウザは構築したアクセシビリティツリーを、この API を通じてスクリーンリーダーなどの支援技術に提供します。
なお、Playwright は OS の API ではなく、ブラウザのプロトコル（Chromium なら CDP）経由でこの情報を取得しています。

| OS / プラットフォーム | アクセシビリティ API |
| ---- | ---- |
| Windows | UI Automation (UIA) / MSAA・IAccessible2 |
| macOS | NSAccessibility |
| Linux | AT-SPI |
| Android | AccessibilityNodeInfo |
| iOS | UIAccessibility |
