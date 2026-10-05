---
title: "[Claude Code] Anthropic製の公式プラグイン全 39 個を用途別に紹介する" # 記事のタイトル
emoji: "🧩" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["claudecode", "claude", "ai", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

Claude Code には「プラグイン」という仕組みがあり、Anthropic は公式のマーケットプレイス（プラグインの一覧）を GitHub で公開しています。
ただ、掲載されているプラグインは 300 個を超えていて、どれから試せばよいのか分かりにくいと感じました。

この記事では、そのうち Anthropic 自身が開発している 39 個のプラグインを用途別に整理します。
あわせて、スキルとの違い、メリットとデメリット、インストール方法も紹介します。

https://github.com/anthropics/claude-plugins-official

:::message
この記事の内容は、2026 年 10 月 3 日時点のリポジトリ（コミット `d182ca4`）をもとにしています。
コマンドの構文は Claude Code v2.1.289 で確認しました。説明はターミナルで起動する Claude Code を前提にしています。
プラグインは追加・変更されることがあるため、最新の情報はリポジトリを確認してください。
:::

## 🌱 結論

用途ごとに、次のプラグインを使います。

| やりたいこと | プラグイン |
|---|---|
| コミットや PR（プルリクエスト）の作成をまとめて任せる | `commit-commands` |
| PR やコードをレビューする | `code-review` / `pr-review-toolkit` / `code-simplifier` |
| 新機能を段階を踏んで開発する | `feature-dev` |
| 古いシステムを作り直す | `code-modernization` |
| 完了するまで同じ作業を繰り返させる | `ralph-loop` |
| 画面のデザインや、操作して試せる HTML を作る | `frontend-design` / `playground` |
| コードの脆弱性を見つける | `security-guidance` / `claude-security` |
| Claude Code の設定を整える | `claude-code-setup` / `claude-md-management` / `hookify` |
| スキルやプラグイン、MCP サーバーなどを作る | `skill-creator` / `plugin-dev` / `agent-sdk-dev` / `mcp-server-dev` |
| 社内の MCP サーバーに Claude API からつなぐ | `mcp-tunnels` |
| 定義へのジャンプや型エラーの検出を強化する | `typescript-lsp` / `pyright-lsp` など 12 個 |
| 解説を聞きながら学ぶ | `explanatory-output-style` / `learning-output-style` |
| 自分の使い方を振り返る・共有する | `session-report` / `receipts` / `project-artifact` |
| 難しい数学の問題を解く | `math-olympiad` / `math-proof` |
| 特定のハードウェアを準備する | `cwc-makers` |

最初に試すなら、`commit-commands` と、普段使う言語の LSP プラグインがおすすめです。
`commit-commands` は入れたその日から毎日のコミットに使えます。LSP プラグインを入れると、Claude が定義の場所や型エラーを正確に調べられるようになります。どちらも効果を実感しやすいプラグインです。

## 🌱 プラグインとは

プラグインは、スキル・サブエージェント・フック・MCP サーバーなどを 1 つにまとめ、一度にインストールできるようにしたフォルダです。

| 中身 | 役割 |
|---|---|
| スキル（コマンド） | 特定の作業の手順書。スラッシュコマンドで呼び出すことも、依頼の内容に合わせて Claude に自動で使わせることもできる |
| サブエージェント | 特定の役割を任せる別の Claude（例: レビュー担当）。Claude が必要に応じて呼び出す |
| フック | ファイル編集のあとなど、決まったタイミングで自動実行される処理 |
| MCP サーバー | 外部のサービスやツールを Claude から使えるようにする仕組み |
| LSP サーバー | 定義へのジャンプや型エラーの検出など、コードを解析する仕組み |

フォルダの構成は次のとおりです。必要なものだけを入れれば動きます。

```text
plugin-name/
├── .claude-plugin/
│   └── plugin.json   # プラグインの名前や説明
├── skills/           # スキル
├── commands/         # スラッシュコマンド（古い形式。新しく作るなら skills/）
├── agents/           # サブエージェントの定義
├── hooks/            # フック
├── .mcp.json         # MCP サーバーの設定
├── .lsp.json         # LSP サーバーの設定
└── README.md         # 説明書
```

### プラグインの呼び出し方

プラグインのスキルやコマンドは、`/プラグイン名:コマンド名` の形で呼び出します。たとえば `commit-commands` の `commit` は `/commit-commands:commit` です。
スキルの多くは、普通の文章で頼んでも、依頼の内容に合わせて Claude が自動で使います。

この記事の紹介では、スラッシュコマンドで呼び出すものは「使い方」、普通の文章で頼むものは「依頼例」として書いています。

## 🌱 公式マーケットプレイスの構成

プラグインは、マーケットプレイスと呼ばれる一覧から探してインストールします。
Anthropic の公式マーケットプレイス `claude-plugins-official` は、Claude Code をはじめて対話モード（`claude` と入力して起動し、会話しながら操作する通常の使い方）で起動したときに自動で登録されます。

公式マーケットプレイスには、2 種類のプラグインが掲載されています。

```text
anthropics/claude-plugins-official/
├── .claude-plugin/
│   └── marketplace.json  # 掲載しているプラグインの一覧（315 個）
├── plugins/              # Anthropic が開発しているプラグイン（掲載は 39 個。ほかに見本の example-plugin）
└── external_plugins/     # パートナーやコミュニティのプラグイン
```

315 個のうち、Anthropic が開発しているのは 39 個です。残りはパートナー企業やコミュニティのプラグインで、多くは外部のリポジトリから取得されます。

:::message alert
README には、「Anthropic はプラグインに含まれる MCP サーバーやファイルなどを管理しておらず、意図どおりに動くことや、今後変わらないことを保証できない」と書かれています。
特にパートナーやコミュニティのプラグインは、インストール前にホームページや中身を確認してください。
:::

## 🌱 スキルとの違い

スキルは「1 つの作業の手順書」で、プラグインは「スキルなどをまとめて配布するための箱」です。
スキルは、プラグインに入れなくても単体で使えます。

![マーケットプレイスの中に、コマンド・スキル・サブエージェント・フックなどの部品をまとめたプラグインが並び、/plugin install で Claude Code に入ることと、単体のスキルは ~/.claude/skills/ に置くだけで Claude Code に読み込まれることを表した図](/images/articles/claude-code-official-plugins/plugin-overview.drawio.png)

| 項目 | 単体のスキル | プラグイン |
|---|---|---|
| 中身 | `SKILL.md` と同梱ファイル | スキル・サブエージェント・フック・MCP サーバーなどの組み合わせ |
| 入れ方 | フォルダを `~/.claude/skills/` などに置く | `/plugin` コマンドでインストールする |
| 更新 | 自分でファイルを差し替える | マーケットプレイスから更新される |
| 呼び出し名 | `/スキル名` | `/プラグイン名:スキル名` |
| 止め方 | フォルダを消す、移動する | 無効化・アンインストールのコマンドがある |

自分用の小さな手順書なら単体のスキルで十分です。
複数の部品を組み合わせたいときや、チームに配りたいときはプラグインが向いています。

## 🌱 メリットとデメリット

### メリット

- **1 つのコマンドで入る**: スキルやフック、MCP サーバーなどの部品を、まとめてインストールできます。
- **更新が楽**: 公式マーケットプレイスは自動更新が最初から有効です。Claude Code の起動後に新しいバージョンが取り込まれ、次のセッション（Claude Code を起動してから終了するまで）から使われます。
- **オン・オフを切り替えやすい**: アンインストールせずに無効化できるため、試しに入れて合わなければ止めるのも簡単です。
- **チームでそろえやすい**: プロジェクト単位でインストールすると設定ファイルに記録されるため、コミットしておけばチームで同じプラグインをそろえやすくなります（各自のインストールは必要です）。

### デメリット

- **使っていなくてもコンテキストを使う**: コンテキストは、Claude が一度に扱える情報の量で、上限があります。有効なプラグインのうち、Claude が自動で呼び出せるスキルやサブエージェントは、名前と説明が毎回コンテキストに入ります。入れすぎると、その分だけ会話に使える量が減り、使用量（トークンの消費。プランの利用上限や API 料金に影響します）も増えます。
- **自分の権限でコードが動く**: フックや MCP サーバーは、自分のユーザー権限で実行されます。自動更新で中身が変わったときも同じです。インストール前に中身を確認し、気になる場合は自動更新を切ることもできます。
- **別のツールが必要なことがある**: LSP プラグインは言語サーバー本体、`security-guidance` などは Python、`commit-commands` の PR 作成や `code-review` は GitHub CLI（`gh`）が必要です。
- **中身を書き換えにくい**: 自分で書き換えても、更新で上書きされる可能性があります。手を加えたいときは、単体のスキルとしてコピーして使うほうが安全です。

## 🌱 インストール方法

### 一覧から選んでインストールする

Claude Code を起動して `/plugin` を実行すると、プラグインの管理画面が開きます。

1. **Discover** タブで、インストールしたいプラグインを探す（文字を入力すると絞り込めます）
2. プラグインを選んで **Enter** を押し、詳細を確認する
3. インストールする範囲（スコープ）を選ぶ

詳細画面の **Will install** には、そのプラグインが追加するスキル・サブエージェント・フックなどが表示されます。

**Discover** タブに何も表示されないときは、公式マーケットプレイスが登録されていない可能性があります。次のコマンドで追加してください。

```text
/plugin marketplace add anthropics/claude-plugins-official
```

### コマンドでインストールする

名前が分かっている場合は、`プラグイン名@マーケットプレイス名` の形で指定します。

```text
/plugin install commit-commands@claude-plugins-official
```

このコマンドはすぐにはインストールせず、詳細画面を開きます。内容を確認して、スコープを選ぶとインストールされます。
この方法か **Marketplaces** タブから詳細画面を開くと、公式マーケットプレイスのプラグインでは、コンテキストを使う量の目安（**Context cost**）も確認できます（**Discover** タブから開いた場合は表示されません）。

インストールが終わって `Plugin is now active.` と表示されれば、すぐに使えます。
`Run /reload-plugins to activate.` と表示された場合は、Claude Code が自動で読み込み直します。キャッシュについての警告が出て保留になったときは、`/reload-plugins --force` を実行します。

### スコープを選ぶ

スコープによって、誰がどこでプラグインを使えるかが変わります。

| スコープ | 使える範囲 | 記録される設定ファイル |
|---|---|---|
| user | 自分の、このパソコンのすべてのプロジェクト | `~/.claude/settings.json` |
| project | このリポジトリで作業する全員 | `.claude/settings.json`（コミットする） |
| local | 自分の、このリポジトリだけ | `.claude/settings.local.json` |

`~` はホームディレクトリを表します。Windows では `C:\Users\<ユーザー名>` です。

project スコープでは、設定ファイルをコミットすると、チームでプラグインを有効にできます。
ただし、プラグイン本体は各自のパソコンに自動ではダウンロードされないため、メンバーもそれぞれ同じコマンドでインストールする必要があります。
チームに配るプラグインは、中身をチームで確認してから決めると安全です。

### シェルからインストールする

Claude Code を起動せずに、ターミナルからインストールすることもできます。セットアップ用のスクリプトに書いておくときに便利です。
project スコープや local スコープで入れるときは、対象のリポジトリのフォルダに移動してから実行します。

```bash
cd path/to/your-repository
claude plugin install commit-commands@claude-plugins-official --scope project
```

`--scope` を省略すると user スコープになります。
インストールしたプラグインは、次に Claude Code を起動したとき、または起動中のセッションで `/reload-plugins` を実行したときに読み込まれます。`/plugin` の **Installed** タブに表示されていれば成功です。

### 管理する

インストール後は、`/plugin` の **Installed** タブで、有効化・無効化・更新・アンインストールができます。
シェルからは次のコマンドで操作できます。

```bash
claude plugin disable commit-commands@claude-plugins-official    # 無効化
claude plugin enable commit-commands@claude-plugins-official     # 有効化
claude plugin update commit-commands@claude-plugins-official     # 更新（再起動後に反映）
claude plugin uninstall commit-commands@claude-plugins-official  # アンインストール
```

`uninstall` は、`--scope` を省略すると user スコープが対象になります。project スコープで入れた場合は、リポジトリのフォルダで `--scope project` を付けて実行します。
公式マーケットプレイスは自動更新が有効なため、通常は `update` を実行する必要はありません。

:::message
有効なプラグインが会話に追加しているコンテキストの量は、ターミナルで `claude plugin details <プラグイン名>` を実行すると確認できます。
`/plugin` の **Installed** タブでは、最近使っていないプラグインが **Not used recently** にまとめて表示されるため、整理するときの目安になります。
:::

https://code.claude.com/docs/en/plugins/install

## 🌱 用途別のプラグイン紹介

### 🔀 Git・レビュー

#### commit-commands

Git のコミットや PR 作成をまとめて行うコマンド集です。

| コマンド | 内容 |
|---|---|
| `commit` | 変更内容と過去のコミットメッセージの書き方を見て、コミットを作る |
| `commit-push-pr` | コミット、プッシュ、PR の作成までをまとめて行う |
| `clean_gone` | リモートで削除済みのブランチを、関連するワークツリーも含めてローカルから強制削除する |

- 使いどころ: コミットメッセージを考える手間を減らしたいとき
- 使い方: `/commit-commands:commit`

`commit-push-pr` で PR を作るには、GitHub CLI（`gh`）のインストールと、`gh auth login` でのログインが必要です。
`clean_gone` は強制削除のため、ワークツリーの未コミットの変更やマージしていないコミットも消えます。実行前に `git worktree list` や `git branch -vv` で対象を確認してください。

#### code-review

PR を複数のサブエージェントで並行してレビューし、結果を PR にコメントします。
指摘ごとに確信度を 0〜100 で採点し、80 未満の指摘は捨てることで、誤検知を減らしています。
`commit-commands` と同じく、GitHub CLI（`gh`）を使います。

- 使いどころ: PR 全体をひととおり見て、コメントまで残してほしいとき（特定の観点を深く見るなら `pr-review-toolkit`）
- 使い方: `/code-review:code-review`

#### pr-review-toolkit

観点ごとに分かれた 6 個のレビュー用サブエージェントのセットです。

| サブエージェント | 見る観点 |
|---|---|
| `comment-analyzer` | コメントが正確か、古くなっていないか |
| `pr-test-analyzer` | テストが足りているか |
| `silent-failure-hunter` | エラーを握りつぶしていないか |
| `type-design-analyzer` | 型の設計が適切か |
| `code-reviewer` | 規約違反やバグがないか |
| `code-simplifier` | もっと簡単に書けないか |

`review-pr` コマンドでまとめて実行することも、「コメントが正確か確認して」のように頼んで 1 個だけ使うこともできます。

- 使いどころ: 特定の観点で深くレビューしたいとき
- 使い方: `/pr-review-toolkit:review-pr`

#### code-simplifier

動作を変えずに、コードを読みやすく整理するサブエージェントです。
`pr-review-toolkit` に入っている `code-simplifier` と同じ役割のものを、単体で入れられます。特に指定しなければ、直近で変更したコードを対象にします。

- 使いどころ: 機能ができたあと、コードを整理してからコミットしたいとき
- 依頼例: 「さっき変更したコードを整理して」

### 🛠️ 開発

#### feature-dev

新機能の開発を、7 つの段階に分けて進めるコマンドです。
いきなりコードを書くのではなく、既存コードの調査、要件の確認、設計を済ませてから実装し、最後にレビューします。
調査・設計・レビューの 3 個のサブエージェントが付いています。

- 使いどころ: 影響範囲が広い機能を、手戻りなく作りたいとき
- 使い方: `/feature-dev:feature-dev OAuth でのログイン機能を追加して`

#### code-modernization

古いシステム（レガシーコード）を作り直すための、13 個のコマンドと 8 個のサブエージェントのセットです。
現状の調査、構造の可視化、業務ルールの抽出、計画の承認、実装の順に進みます。最後に、新しいコードが元のコードと同じ動きをするかを確認します。

- 使いどころ: 古いシステムを新しい言語やフレームワークに移行したいとき
- 使い方: `/code-modernization:modernize` から始めると、質問に答えるだけで次の手順を案内してくれます

一度に多くのサブエージェントが動くため、大きなシステムでは使用量が増えます。README でも、まずは 1 つのモジュールで試すよう勧めています。

#### ralph-loop

同じ指示を、完了するまで何度も繰り返させるプラグインです。
Claude が作業を終えようとすると、フックがそれを止めて同じ指示をもう一度渡します。Claude は前回までの作業結果をファイルから読み取り、少しずつ完成に近づけます。

![ralph-loop の流れの図。作業して結果をファイルに保存したあと、完了の目印を出力したか、回数の上限に達したかを確認し、どちらでもなければフックが終了を止めて同じ指示をもう一度渡し、前回までの結果をファイルから読み取って作業を繰り返す](/images/articles/claude-code-official-plugins/ralph-loop.drawio.png)

- 使いどころ: テストが通るまで修正を続けさせたいときなど、ゴールがはっきりしている作業
- 使い方: `/ralph-loop:ralph-loop "TODO 管理の REST API を作る。完成したら <promise>COMPLETE</promise> と出力する" --completion-promise "COMPLETE" --max-iterations 50`

Claude が `<promise>COMPLETE</promise>` を出力すると、完了とみなしてループが止まります。`--completion-promise` には、その目印の文字列を指定します。
`--max-iterations` で回数の上限を決めておかないと、使用量が増え続けるおそれがあります。
フックは bash のスクリプトのため、Windows では Git for Windows（Git Bash）が必要です。

#### frontend-design

Web 画面のデザインを、ありきたりな見た目にならないよう整えるスキルです。
同じ名前のスキルが、公式スキルのリポジトリ（`anthropics/skills`）にもあります。

- 使いどころ: LP（ランディングページ）など、見た目にこだわりたい画面を作るとき
- 依頼例: 「喫茶店の紹介ページを、落ち着いた雰囲気でデザインして」

#### playground

操作しながら試せる HTML（プレイグラウンド）を作るスキルです。
片側に設定を変える操作パネル、もう片側にプレビューがあり、下に Claude に渡すための指示文が出力されます。
デザイン、データの検索条件、概念マップ、文書の添削、コードの差分レビュー、コードベースの構成図の 6 種類のテンプレートが入っています。

- 使いどころ: 余白や配色など、言葉で伝えにくい好みを Claude に伝えたいとき
- 依頼例: 「カードの余白と配色を調整できるプレイグラウンドを作って」

### 🔒 セキュリティ

#### security-guidance

Claude が書いたコードの脆弱性を、3 段階でチェックするプラグインです。

1. ファイル編集時: 危険な書き方（`innerHTML` への代入など）をパターンで見つけて警告する
2. 回答の終了時: 変更差分を AI でレビューし、重大な問題があれば Claude に直させる
3. コミット時: 関連するファイルも読んで、複数ファイルにまたがる脆弱性を探す

インジェクションや XSS（Web ページに悪意のあるスクリプトを埋め込む攻撃）、秘密情報の直書きなど、Web アプリでよくある脆弱性を対象にしています。

- 使いどころ: 普段の開発で、脆弱性の作り込みを防ぎたいとき（入れておくだけで動きます）

Claude Code v2.1.144 以上と、Python 3.8 以上が必要です。
回答が終わるたびに変更差分を最新の Opus モデルに送ってレビューするため、使用量が増えます。段階ごとに止めたいときは、`ENABLE_STOP_REVIEW=0` などの環境変数で無効にできます。

#### claude-security

複数のサブエージェントが、リポジトリ全体や変更差分の脆弱性を調べるプラグインです。
見つけた問題は、別のサブエージェントが検証してから報告します。修正のパッチを作ることもできます。

- 使いどころ: リリース前など、時間をかけて脆弱性を洗い出したいとき
- 使い方: `/claude-security:claude-security` でメニューを開き、「リポジトリ全体」「変更差分」「パッチの提案」から選びます

自分の権限でリポジトリを読むため、自分が管理するコード向けです。信頼できないリポジトリを調べるときは、sandbox-runtime の中で Claude Code を動かすよう README で勧めています。

### ⚙️ Claude Code の設定

#### claude-code-setup

コードベースを調べて、そのプロジェクトに合うフック・スキル・MCP サーバー・サブエージェントなどを提案するスキルです。
ファイルは変更せず、提案だけを行います。

- 使いどころ: Claude Code を使い始めたばかりで、何を設定すればよいか分からないとき
- 依頼例: 「このプロジェクトにおすすめの自動化を提案して」

#### claude-md-management

`CLAUDE.md`（Claude Code に読ませるプロジェクトの説明書）を整えるためのプラグインです。

| 種類 | 名前 | 内容 |
|---|---|---|
| スキル | `claude-md-improver` | 今のコードと `CLAUDE.md` を見比べて品質を評価し、承認すると古い記述を直す |
| コマンド | `revise-claude-md` | 今のセッションで分かったことを `CLAUDE.md` に追記する |

- 使いどころ: `CLAUDE.md` を定期的に見直したいとき、作業の最後に学びを残したいとき
- 使い方: `/claude-md-management:revise-claude-md`

#### hookify

「`rm -rf` を使ったら警告して」のように、言葉で伝えるだけでフックを作れるプラグインです。
設定は Markdown ファイルに保存され、再起動しなくても次のツール実行から有効になります。
引数を付けずに実行すると、会話の内容から防ぎたい動作を探して提案してくれます。

- 使いどころ: Claude に毎回同じ注意をしていて、仕組みで防ぎたいとき
- 使い方: `/hookify:hookify TypeScript ファイルで console.log を使ったら警告して`

### 🧰 拡張機能を作る

#### skill-creator

新しいスキルの作成や、既存スキルの改善を手伝うスキルです。
同じ名前のスキルが、公式スキルのリポジトリ（`anthropics/skills`）にもあります。

- 使いどころ: Claude に毎回同じ手順を説明していると気づいたとき
- 依頼例: 「Zenn の記事を書くときのルールをスキルにしたい」

#### plugin-dev

プラグインを作るためのセットです。
フック、MCP、コマンド、サブエージェントなどの作り方を説明する 7 個のスキルと、サブエージェントの作成やプラグイン・スキルの検証を行う 3 個のサブエージェントが入っています。

- 使いどころ: 自分のプラグインを作ってチームに配りたいとき
- 使い方: `/plugin-dev:create-plugin` で、設計から検証までを対話しながら進めます

#### agent-sdk-dev

Claude Agent SDK（Claude を組み込んだエージェントを作るためのライブラリ）でアプリを作るためのセットです。
新しいアプリのひな形を作るコマンドと、Python 版・TypeScript 版のコードを検証するサブエージェントが入っています。

- 使いどころ: Agent SDK で新しくエージェントを作り始めるとき
- 使い方: `/agent-sdk-dev:new-sdk-app`

#### mcp-server-dev

MCP サーバーを作るための 3 個のスキルです。
MCP サーバー本体の作り方のほか、チャット画面にフォームなどを表示する MCP アプリの作り方や、配布用のファイル（MCPB）にまとめる方法も扱います。

- 使いどころ: 社内ツールや外部 API を Claude から使えるようにしたいとき
- 依頼例: 「GitHub の Issue を検索できる MCP サーバーを作りたい」

### 🌐 MCP への接続

#### mcp-tunnels

社内ネットワークなど、外から直接つながらない場所にある MCP サーバーを、Claude API（Messages API や Managed Agents）から使えるようにするためのコマンドです。
接続には Anthropic の MCP トンネルを使い、Docker Compose での手順を、証明書の作成から動作確認まで案内します。

- 使いどころ: 社内の MCP サーバーを、Claude API で作ったエージェントから使いたいとき
- 使い方: `/mcp-tunnels:create-docker-mcp-tunnel`

MCP トンネルは、稼働の保証がないリサーチプレビューの機能です。Docker と OpenSSL も必要です。

:::message alert
社内のシステムを外部から使える経路を作ることになります。所属する組織のセキュリティポリシーを確認し、必要なら許可を得てから使ってください。
つなぐ MCP サーバーは、操作できる範囲や扱うデータを最小限にしておくと安全です。
:::

### 🔍 言語サーバー（LSP）

LSP（Language Server Protocol）は、エディタが「定義へのジャンプ」「参照の検索」「型エラーの表示」などを行うための共通の仕組みです。
LSP プラグインを入れると、Claude Code もこれらの機能を使ってコードを調べられるようになります。

| プラグイン | 言語 | 使う言語サーバー |
|---|---|---|
| `typescript-lsp` | TypeScript / JavaScript | typescript-language-server |
| `pyright-lsp` | Python | Pyright |
| `gopls-lsp` | Go | gopls |
| `rust-analyzer-lsp` | Rust | rust-analyzer |
| `jdtls-lsp` | Java | Eclipse JDT.LS |
| `kotlin-lsp` | Kotlin | kotlin-lsp |
| `csharp-lsp` | C# | csharp-ls |
| `clangd-lsp` | C / C++ | clangd |
| `swift-lsp` | Swift | SourceKit-LSP |
| `ruby-lsp` | Ruby | ruby-lsp |
| `php-lsp` | PHP | Intelephense |
| `lua-lsp` | Lua | lua-language-server |

:::message
LSP プラグインには、言語サーバーを呼び出す設定だけが入っています。言語サーバー本体は、自分でインストールする必要があります。
たとえば `typescript-lsp` は、README で次のコマンドを案内しています（Node.js が必要です）。

```bash
npm install -g typescript-language-server typescript
```

ほかの言語のインストール方法は、各プラグインの README に書かれています。
インストール後に `typescript-language-server --version` のようにコマンドが実行できるか（PATH が通っているか）を確認し、Claude Code を起動し直してください。
:::

### 🎓 学習

この 2 個は、セッションの開始時にフックで Claude への指示を追加し、回答のスタイルを変えるプラグインです。
毎回指示が追加され、回答も長くなるため、README では「使用量が増えても構わない場合だけ入れる」よう注意しています。

:::message
Claude Code 本体にも、同じ働きをする出力スタイル「Explanatory」と「Learning」が組み込まれています。`/output-style explanatory` のように切り替えられるので、まずは本体の機能を試すのがおすすめです。
プラグインは、入れている間はすべてのセッションで常に指示を追加します。
:::

#### explanatory-output-style

コードを書く前後に、実装の選び方やコードベースの書き方について、短い解説（Insight）を付け加えます。

- 使いどころ: 作業を進めながら、なぜそう書くのかも知りたいとき

#### learning-output-style

`explanatory-output-style` の解説に加えて、設計の判断が必要な部分を 5〜10 行ほど自分で書くよう求めてきます。
決まりきったコードは Claude が書き、業務ロジックやエラー処理など、判断が必要な部分だけをユーザーに任せます。

- 使いどころ: Claude に任せきりにせず、手を動かして学びたいとき

### 📊 振り返り・共有

#### session-report

手元の Claude Code の利用履歴（`~/.claude/projects`）を集計し、HTML のレポートにするスキルです。
トークン数、キャッシュの効き具合、よく使ったサブエージェントやスキル、使用量の多かった指示などを確認できます。

- 使いどころ: 使用量が多い原因を調べたいとき
- 依頼例: 「直近 7 日間のセッションレポートを作って」

#### receipts

自分が Claude Code で何を作ったかを、レシート風のレポートにまとめるスキルです。
変更したファイルや行数、コミット、PR、プロジェクトごとの使用量の割合などを集計します。
レポートは手元に保存され、どこにも公開されません。

- 使いどころ: 上司への報告や、自分の振り返りに使うとき
- 使い方: `/receipts:receipts`（直近 30 日）、`/receipts:receipts week`（直近 7 日）

`session-report` と `receipts` のレポートには、指示の内容やプロジェクト名が入ることがあります。人に渡す前に中身を確認してください。

#### project-artifact

プロジェクトの進捗をまとめたページを作り、Claude.ai のアーティファクト（Web ページ）として保存するスキルです。
作ったページは最初は自分だけが見られる状態で、Claude.ai の画面から共有すると、ほかの人も見られるようになります。
「更新して」と頼むと、最新の状態を集め直して同じ URL のページを更新します。

- 使いどころ: 複数の作業が並行する大きなプロジェクトの状況を、チームに共有したいとき
- 依頼例: 「このプロジェクトの進捗ページを作って」

Claude.ai のアカウントでログインしている必要があります。README によると、Claude Code のアーティファクトはベータ版で、Team プランと Enterprise プランで使えます。
ページにはリポジトリや PR から集めた情報が載るため、共有する前に載せてはいけない情報が入っていないかを確認してください。

### 🧮 数学

#### math-olympiad

国際数学オリンピックなどの競技数学の問題を解くスキルです。
解いたあとに、別のコンテキストで動く検証役が証明の穴を探します。確信が持てないときは、それらしい答えを作らずに解けなかったと答えるよう作られています。

- 使いどころ: 競技数学の問題を、検証付きで解かせたいとき

#### math-proof

難しい数学の問題に取り組み、証明を `proof.md` にまとめる 2 個のスキルです。

| スキル | 進め方 |
|---|---|
| `solo` | 1 つのセッションで段階的に解く。途中経過はメモに残す |
| `siege` | 審判役と解答役のサブエージェントが、何ラウンドも（数時間かけて）取り組む |

- 使いどころ: 証明が必要な難しい問題に、時間をかけて取り組ませたいとき
- 使い方: `/math-proof:solo`、`/math-proof:siege`

Claude Code v2.1.280 以上が必要で、事前に `settings.json` へ環境変数を追加する設定も必要です（手順は README にあります）。
`siege` はサブエージェントを何十回も動かすため、使用量がとても多くなります。

### 🔌 その他

#### cwc-makers

Code-with-Claude Makers キット（M5Stack Cardputer-Adv という小型のコンピューター）のセットアップ用プラグインです。
`/cwc-makers:maker-setup` を実行すると、リポジトリの取得、ファームウェアの書き込み、アプリのインストールまでをまとめて行います。

- 使いどころ: このキットを手に入れて、最初のセットアップをするとき

## 🌱 公式スキルもプラグインとして入れられる

Anthropic の公式スキル（`anthropics/skills`）も、プラグインとしてまとめてインストールできます。
このマーケットプレイスは自動では登録されないため、先に追加します。

```text
/plugin marketplace add anthropics/skills
/plugin install document-skills@anthropic-agent-skills
```

追加するときはリポジトリ名（`anthropics/skills`）を指定しますが、インストールするときの `@` の後ろには、マーケットプレイス名（`anthropic-agent-skills`）を指定します。

`document-skills` には、Word・Excel・PowerPoint・PDF を扱う 4 個のスキルが入っています。
ほかに、12 個のスキルをまとめた `example-skills` や、`claude-api` などのプラグインがあります。

https://github.com/anthropics/skills

## 🌱 おわりに

この記事では、プラグインとスキルの違い、メリットとデメリット、インストール方法を整理し、Anthropic 製の公式プラグイン 39 個を紹介しました。
プラグインは便利ですが、入れたままにするとコンテキストや使用量を消費します。ときどき `/plugin` の **Not used recently** を見て、使っていないものを整理するのがおすすめです。
何を入れればよいか迷ったら、`claude-code-setup` に自分のプロジェクトに合う設定を提案してもらうのもよいと思います。

## 🌱 参考

https://github.com/anthropics/claude-plugins-official

https://code.claude.com/docs/en/plugins

https://code.claude.com/docs/en/plugins/install

https://code.claude.com/docs/en/plugins/security
