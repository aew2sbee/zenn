---
title: "[Claude Code] Anthropic公式スキル全 19 個を用途別に紹介する" # 記事のタイトル
emoji: "🧠" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["claudecode", "claude", "ai", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開:true / 非公開:false
---

## 🌱 はじめに

Claude Code を使っていると、「スキル」という言葉をよく目にします。
Anthropic は公式のスキルを GitHub で公開しています。ただ、数が多く、どれが何に使えるのか分かりにくいと感じました。

この記事では、公式リポジトリで配布されているスキル 19 個を用途別に整理し、それぞれの使いどころを紹介します。

https://github.com/anthropics/skills

:::message
この記事の内容は、2026 年 9 月 28 日時点のリポジトリ（コミット `8a1541c`）をもとにしています。
スキルは追加・変更されることがあるため、最新の情報はリポジトリを確認してください。
:::

## 🌱 結論

用途ごとに、次のスキルを使います。

| やりたいこと | スキル |
|---|---|
| Word・Excel・PowerPoint・PDF を作る、読む、編集する | `docx` / `xlsx` / `pptx` / `pdf` |
| 仕様書や提案書を Claude と一緒に書く | `doc-coauthoring` |
| Claude API を使ったアプリを作る | `claude-api` |
| MCP サーバーを作る | `mcp-builder` |
| ローカルの Web アプリを自動でテストする | `webapp-testing` |
| Claude.ai で複雑な HTML アーティファクトを作る | `web-artifacts-builder` |
| 自分用のスキルを作る、改善する | `skill-creator` |
| 画面のデザインを整える | `frontend-design` / `theme-factory` / `brand-guidelines` |
| ポスターや画像、アートを作る | `canvas-design` / `algorithmic-art` / `slack-gif-creator` |
| 社内向けの報告文やニュースレターを書く | `internal-comms` |
| Claude の使い方を学ぶ、回答を見直す | `academy-guide` / `discernment-nudge` |

最初に試すなら、日常業務で出番の多い `docx` / `xlsx` / `pptx` / `pdf` の 4 つがおすすめです。

## 🌱 スキルとは

スキルは、特定の作業のやり方を Claude に教えるための「手順書のフォルダ」です。
フォルダの中には、指示を書いた `SKILL.md` と、必要に応じてスクリプトや参考資料が入っています。

```text
pdf/
├── SKILL.md      # 指示とメタデータ（必須）
├── LICENSE.txt   # ライセンス
├── reference.md  # 参考資料
├── forms.md      # PDF フォーム入力の手順
└── scripts/      # 補助スクリプト
```

`SKILL.md` の先頭には、スキルの名前と説明を書きます。次の例は、`pdf` スキルの `SKILL.md` を簡略化したものです（実際の説明文は英語です）。

```md:pdf/SKILL.md
---
name: pdf
description: PDF の読み取り・結合・分割・フォーム入力などを行うときに使う
---

# PDF Processing Guide
（ここに Claude が従う手順を書く）
```

Claude は `description` を見て、今の依頼にそのスキルが必要かどうかを判断します。
Claude が一度に扱える情報量（コンテキスト）には上限があります。
常に読み込まれるのは各スキルの名前と説明だけで、本文は必要と判断したときだけ読み込まれます。そのため、スキルを多く入れても、コンテキストを圧迫しにくい仕組みになっています。

![インストール済みのスキルのうち、name と description は常にコンテキストに読み込まれ、本文と scripts は依頼に必要と判断された pdf スキルの分だけ読み込まれることを表した図](/images/articles/claude-code-official-skills/skill-loading.drawio.png)

## 🌱 公式リポジトリの構成

リポジトリには、スキル本体のほかに仕様とひな形が入っています。

```text
anthropics/skills/
├── skills/    # スキル本体（19 個）
├── spec/      # Agent Skills の仕様（agentskills.io へのリンク）
└── template/  # SKILL.md のひな形
```

:::message alert
README には、これらのスキルは「デモと学習を目的としたもの」と書かれています。
Claude.ai に組み込まれている機能とは動作が異なる場合があるため、重要な作業に使う前に自分の環境で試してください。
:::

## 🌱 インストール方法

### Claude Code の場合

Claude Code では、スキルのフォルダを決まった場所に置くだけで、自動で読み込まれます。

| 置き場所 | パス | 使える範囲 |
|---|---|---|
| 個人用 | `~/.claude/skills/<スキル名>/SKILL.md` | 自分のすべてのプロジェクト |
| プロジェクト用 | `.claude/skills/<スキル名>/SKILL.md` | そのリポジトリだけ |

`~` はホームディレクトリを表します。Windows では `C:\Users\<ユーザー名>` です。

たとえば `pdf` スキルを個人用に入れる場合は、作業用のフォルダで次のように実行します。
clone した `skills/` フォルダの中に、スキル本体が入った `skills/` フォルダがあるため、パスは `skills/skills/pdf` になります。

```bash:macOS / Linux
git clone --depth 1 https://github.com/anthropics/skills.git
mkdir -p ~/.claude/skills
cp -r skills/skills/pdf ~/.claude/skills/
```

```powershell:Windows（PowerShell）
git clone --depth 1 https://github.com/anthropics/skills.git
New-Item -ItemType Directory -Force "$HOME\.claude\skills"
Copy-Item -Recurse skills\skills\pdf "$HOME\.claude\skills\"
```

`SKILL.md` だけでなく、`scripts/` などの同梱ファイルも使うため、フォルダごとコピーします。
プロジェクト用のフォルダに置いてコミットすれば、チームで同じスキルを使えます。

インストール後は、依頼の内容に合えば Claude が自動でスキルを使います。
確実に使わせたいときは、次のようにスキル名を書いて依頼するか、`/pdf` のようにスラッシュコマンドで呼び出します。

```text
pdf スキルを使って、path/to/some-file.pdf のフォーム項目を抜き出して
```

:::message
スキルに同梱されたスクリプトは、手元の環境で実行されます。
スキルによっては、Python や Node.js のパッケージ、LibreOffice などが必要です。たとえば `pdf` の OCR には `pytesseract` と `pdf2image` を使い、`docx` / `xlsx` / `pptx` は LibreOffice を使います。
必要なものは、各 `SKILL.md` の記述（`Dependencies` の節など）で確認してください。
:::

コピーしたスキルは、公式リポジトリが更新されても自動では反映されません。
最新の内容を使いたいときは、clone したフォルダで最新の状態を取得し、コピーし直します。

```bash
git -C skills pull
cp -r skills/skills/pdf ~/.claude/skills/
```

:::message
`docx` / `xlsx` / `pptx` / `pdf` の 4 つは、ソースは公開されていますが、オープンソースではありません。コードは読めますが、再配布や改変は自由ではないため、コピーしてリポジトリで共有する前に各フォルダの `LICENSE.txt` を確認してください。
それ以外の多くのスキルは、改変や再配布が認められている Apache 2.0 で公開されています。
:::

https://code.claude.com/docs/en/skills

### Claude.ai の場合

Claude.ai では、設定でコード実行を有効にすると、Word・Excel・PowerPoint・PDF の公式スキル（`docx` / `xlsx` / `pptx` / `pdf`）が自動で使われます。
それ以外のスキルは、自作スキルと同じようにアップロードして使います。対象プランや設定の手順は、公式ヘルプを参照してください。

https://support.claude.com/en/articles/12512180-using-skills-in-claude

### Claude API の場合

API では、`docx` / `xlsx` / `pptx` / `pdf` の 4 つを公式スキルとして使えます。
それ以外のスキルは、自作スキルとしてアップロードして使います。どちらの場合も、リクエストでコード実行ツールを指定する必要があります。

https://platform.claude.com/docs/en/api/skills-guide

## 🌱 用途別のスキル紹介

### 📄 ドキュメント作成

#### docx

Word ファイル（`.docx`）の作成・読み取り・編集を行います。
目次やページ番号の入った文書の作成、変更履歴（赤入れ）やコメントの追加にも対応しています。

- 使いどころ: 報告書や議事録を Word 形式で納品したいとき
- 依頼例: 「この議事録メモを、見出しと目次付きの Word ファイルにして」

#### xlsx

Excel ファイル（`.xlsx`）や CSV の作成・編集・整形を行います。
数式を含むファイルについては、保存後に再計算してエラーがないかを確認する手順も組み込まれています。

- 使いどころ: 集計表の作成、崩れた CSV の整形
- 依頼例: 「sales.csv を月別に集計して、合計行付きの Excel にして」

#### pptx

PowerPoint ファイル（`.pptx`）の作成・読み取り・編集を行います。
配色や余白などのデザインの指針と、出来上がりを確認する手順が含まれています。

- 使いどころ: 説明資料のたたき台を作りたいとき、既存スライドの文字を抜き出したいとき
- 依頼例: 「この README をもとに、5 枚程度の紹介スライドを作って」

#### pdf

PDF の読み取り・結合・分割・回転・透かしの追加・フォーム入力などを行います。
スキャンした PDF から、OCR で文字を抜き出すこともできます。

- 使いどころ: 複数の PDF をまとめたいとき、PDF 内の表を取り出したいとき
- 依頼例: 「この 3 つの PDF を 1 つに結合して」

#### doc-coauthoring

仕様書・提案書・設計ドキュメントなどを、Claude と対話しながら書き上げるためのスキルです。
次の 3 段階で進めます。

1. 情報収集: 目的や読者、背景を Claude が質問して集める
2. 構成と推敲: 章立てを決め、1 節ずつ下書きと修正を繰り返す
3. 読者テスト: 前提知識のない読者の視点で読み、分かりにくい箇所を洗い出す

- 使いどころ: 頭の中にある内容を、抜け漏れなく文書にしたいとき
- 依頼例: 「新機能の設計ドキュメントを一緒に書きたい」

### 🛠️ 開発

#### claude-api

Claude API や Anthropic SDK を使ったアプリ開発を支援します。
最新のモデル ID や料金、ストリーミング、ツール利用、プロンプトキャッシュなどの情報がまとまっています。
資料は、Python・TypeScript・Go・Java など 7 言語と curl 向けに用意されています。

- 使いどころ: Claude を組み込んだアプリやエージェントを作るとき
- 依頼例: 「TypeScript で、Claude API を使った要約ツールを作って」

:::message
AI は学習時点の古い情報で API のコードを書いてしまうことがあります。
このスキルを入れておくと、Claude がスキルに書かれた情報をもとにコードを書くため、古いモデル名や書き方を使うミスを減らせます。
:::

#### mcp-builder

MCP（Model Context Protocol）サーバーを作るためのガイドです。
MCP は、Claude などの AI から外部サービスを操作するための共通の仕組みです。
Python（FastMCP）と Node / TypeScript（MCP SDK）に対応しており、調査・実装・テスト・評価の 4 段階で進めます。

- 使いどころ: 社内ツールや外部 API を Claude から使えるようにしたいとき
- 依頼例: 「GitHub の Issue を検索できる MCP サーバーを TypeScript で作って」

#### webapp-testing

Playwright を使って、ローカルで動かしている Web アプリをテストします。
Playwright は、ブラウザを自動で操作するためのツールです。
サーバーの起動から停止までを管理するスクリプト（`with_server.py`）が付いています。

- 使いどころ: 画面の動作確認やスクリーンショットの取得を自動化したいとき
- 依頼例: 「npm run dev で起動して、ログイン画面が表示されるか確認して」

#### web-artifacts-builder

Claude.ai のアーティファクト（チャット画面に表示される HTML）を、React・Tailwind CSS・shadcn/ui を使って作るためのスキルです。
複数のファイルで開発し、最後に 1 つの HTML ファイルにまとめます。

- 使いどころ: 画面遷移や状態管理がある、作り込んだアーティファクトを作りたいとき
- 依頼例: 「タスク管理のダッシュボードをアーティファクトで作って」

単純な 1 ファイルの HTML であれば、このスキルは不要です。

#### skill-creator

新しいスキルの作成や、既存スキルの改善を支援します。
スキルの下書きを作るだけでなく、テスト用の依頼文で実際に動かし、結果を評価して改善する流れまで面倒を見てくれます。
Claude がスキルを正しく呼び出せるよう、`description` の文面を調整する機能もあります。

- 使いどころ: 自分の作業手順をスキルにしたいとき
- 依頼例: 「Zenn の記事を書くときのルールをスキルにしたい」

### 🎨 デザイン・クリエイティブ

#### frontend-design

Web 画面のデザインを、ありきたりな見た目にならないよう整えるためのスキルです。
配色・フォント・レイアウトの方針を先に決め、ありきたりになっていないかを見直してから作り始めます。作っている間も、スクリーンショットで見た目を確認しながら進める手順になっています。

- 使いどころ: LP やポートフォリオなど、見た目にこだわりたい画面を作るとき
- 依頼例: 「喫茶店の紹介ページを、落ち着いた雰囲気でデザインして」

#### canvas-design

ポスターなどの静止画を、PNG や PDF で作るスキルです。
最初にデザインの考え方（コンセプト）を文章で決め、それを絵に落とし込む 2 段階で進めます。

- 使いどころ: イベントの告知ポスターや、記事のアイキャッチ画像を作りたいとき
- 依頼例: 「社内勉強会の告知ポスターを作って」

#### algorithmic-art

p5.js を使って、プログラムで描くアート（ジェネラティブアート）を作るスキルです。
乱数の種（シード）やパラメーターを画面上で変えながら、見た目を調整できる HTML を出力します。

- 使いどころ: 模様や粒子の動きなど、プログラムならではの表現を作りたいとき
- 依頼例: 「風の流れのような模様を描くアートを作って」

#### theme-factory

スライドや文書、HTML などに、配色とフォントの組み合わせ（テーマ）を適用するスキルです。
「Ocean Depths」「Modern Minimalist」など 10 種類のテーマが用意されており、新しいテーマを作ることもできます。

- 使いどころ: 資料の見た目をまとめて整えたいとき
- 依頼例: 「このスライドに Modern Minimalist のテーマを適用して」

#### brand-guidelines

Anthropic のブランドカラーとフォントを、成果物に適用するスキルです。

- 使いどころ: Anthropic 風の見た目にしたいとき
- 依頼例: 「この資料を Anthropic のブランドガイドラインに沿った配色にして」

自社のブランドガイドラインをスキルにするときの見本としても参考になります。

#### slack-gif-creator

Slack 向けのアニメーション GIF を作るスキルです。
絵文字用（128×128 px）とメッセージ用（480×480 px）の推奨サイズや、容量を抑えるためのフレームレート・色数の目安が含まれています。

- 使いどころ: チャンネルで使うリアクション用 GIF を作りたいとき
- 依頼例: 「猫が手を振る Slack 用の GIF を作って」

### 💬 社内コミュニケーション

#### internal-comms

社内向けの文章を、決まった形式で書くためのスキルです。
`examples/` フォルダに、次の 4 種類のテンプレートが入っています。

| テンプレート | 用途 |
|---|---|
| `3p-updates.md` | 進捗（Progress）・計画（Plans）・課題（Problems）の 3P 報告 |
| `company-newsletter.md` | 社内ニュースレター |
| `faq-answers.md` | よくある質問への回答 |
| `general-comms.md` | 上記に当てはまらない文章 |

- 使いどころ: 週次報告や障害報告を書くとき
- 依頼例: 「今週の作業メモから 3P 形式の週次報告を書いて」

テンプレートを自社の形式に書き換えれば、そのまま自社用のスキルとして使えます。

### 🎓 学習・回答の補助

この 2 つは、作業をこなすためのスキルではなく、Claude の回答に補足を加えるスキルです。

#### academy-guide

Claude の使い方を質問したときに、回答の最後に Anthropic の学習サイト「Claude Academy」の関連コースやチュートリアルを紹介します。
紹介するコースが質問の意図と強く一致するときだけ紹介し、作業の途中では紹介しないよう指示されています。

- 使いどころ: Claude を学び始めたばかりのとき、チームへの導入資料を探しているとき
- 依頼例: 「Claude Code のスキルの使い方を教えて」

#### discernment-nudge

Claude が助言や見積もり、分析などの回答をしたあとに、内容を見直すための短い質問を 2〜3 個付け加えます。
質問は、次の 3 つの観点から作られます。

- 事実の確認: 回答のどの部分を、何で確かめるべきか
- 推論の確認: どこで論理を飛ばしていないか
- 前提の確認: 情報が足りず、何を仮定して回答したか

質問を付け加えるのは 1 回の会話につき 1 度だけ、と指示されているため、毎回質問が増えにくい設計になっています。

- 使いどころ: AI の回答をうのみにせず、判断材料として使いたいとき
- 依頼例: 「この新規事業の売上見込みを立てて」

## 🌱 自作スキルを作るときの参考

公式スキルは、そのまま使うだけでなく、自作スキルの見本としても役立ちます。

| 作りたいスキル | 参考になる公式スキル |
|---|---|
| スクリプトを含まないシンプルなスキル | `internal-comms`（`SKILL.md` は約 30 行） |
| テンプレートを使い分けるスキル | `internal-comms` / `theme-factory` |
| スクリプトを同梱するスキル | `pdf` / `webapp-testing` |
| 対話しながら進めるスキル | `doc-coauthoring` |

一から作る場合は、`template/SKILL.md` をコピーするか、`skill-creator` に作成を手伝ってもらうのが手軽です。

```md:template/SKILL.md
---
name: template-skill
description: Replace with description of the skill and when Claude should use it.
---

# Insert instructions below
```

`name` と `description` の 2 項目があれば、スキルとして動きます。
特に `description` は、Claude がスキルを使うかどうかを決める手がかりになるため、「何をするか」と「どんなときに使うか」の両方を書くのがポイントです。

## 🌱 おわりに

公式スキル 19 個を用途別に紹介しました。
まずは `docx` / `xlsx` / `pptx` / `pdf` を入れて、普段の Word・Excel・PDF の作業を任せてみるのがおすすめです。
慣れてきたら、`skill-creator` を使って、自分の作業手順をスキルにしてみてください。

## 🌱 参考

https://github.com/anthropics/skills

https://support.claude.com/en/articles/12512176-what-are-skills

https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills
