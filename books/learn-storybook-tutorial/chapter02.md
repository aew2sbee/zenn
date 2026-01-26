---
title: "Storybookのインストール"
---

## 🌱 インストール

```bash
npm create storybook@latest
```

## 🌱 [オプション] 案内（オンボーディング）

```bash
$ npm create storybook@latest
...
│
◆  New to Storybook?
│  ● Yes: Help me with onboarding
│  ○ No: Skip onboarding & don't ask again
└
```

:::message
**翻訳**
> ◆  New to Storybook?
> │  ● Yes: Help me with onboarding
> │  ○ No: Skip onboarding & don't ask again

Storybookは初めて使いますか？
- Yes: Storybook初心者向けの案内（オンボーディング）を表示しますか？
  ▶ Storybookの基本構造（stories、Controls、Docs）を知りたい
- No: オンボーディングは不要。今後も聞かなくてOK
  ▶ 余計なファイルが増やしたくない/プロジェクト固有のルールがある
:::


## 🌱 [オプション] Playwright（Chromium）

```bash
...
│
◆  Do you want to install Playwright with Chromium now?
│  ● Yes / ○ No
└
```


:::message
**翻訳**
> ◆  Do you want to install Playwright with Chromium now?
> │  ● Yes / ○ No

Playwright（Chromium）を今すぐインストールしますか？
- Yes: 今すぐ chromium ブラウザをダウンロード＆セットアップする
  ▶ 今後`addon-vitest`を使う予定がある
- No: 今はインストールしない
  ▶ StorybookのUI確認だけ先にやりたい

補足
1. Playwright（Chromium）: Webブラウザを自動で操作するためのツール
2. addon-vitest: Storybookに「テスト結果を表示する機能」を追加するアドオン
:::


```bash
◇  Playwright browser binaries installed successfully
│
◇  Storybook was successfully installed in your project!
│
│  To run Storybook manually, run npm run storybook. CTRL+C to stop.
│
│  Wanna know more about Storybook? Check out https://storybook.js.org/
│  Having trouble or want to chat? Join us at https://discord.gg/storybook/
No Instance(s) Available.
│
└


┌  storybook v10.2.0

```
