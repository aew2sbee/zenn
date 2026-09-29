---
title: '2026.09: Software Design 2026年10月号'
---

## 🌱 書籍情報
https://gihyo.jp/magazine/SD/archive/2026/202610

## 🌱 本の概要
公式ページの紹介文を要約しています。

- 第1特集「自走AIのためのハーネス設計入門」: AIエージェントに自律的にコーディングさせる時代には、人が担ってきたコードの検証やレビューを仕組みに置き換える必要がある。そのための「ハーネス」（AIを制御する縛り）の設計方法を解説している。
- 第2特集「AIによるテスト自動化のリアル」: 株式会社TOKIUMが、QAエンジニアの暗黙知に頼っていたE2Eテストの設計・実行をAIで自動化した事例。暗黙知の構造化、AIが破れないルールの仕組み化、チームへの展開で直面した落とし穴を紹介している。

## 🌱 読む目的
- 最近はコーディングだけではなくテストコードの作成もAI(CLAUDE CODE)にお願いすることが増えたが、完全にAI任せにするほどAIを使いこなせない現状のため、改善のヒントを知りたい
- 「ハーネス」という単語を初めて聞いてどんなものなのかを知りたい

## 🌱 読書メモ

### 📌 内部ハーネスと外部ハーネスとは？
ハーネスは、AIエージェントのうちモデル（LLM）以外の部分のこと。Claude Code などの製品に最初から組み込まれている部分が「内部ハーネス」、利用者が CLAUDE.md やテストなどで自分で追加する部分が「外部ハーネス」。

:::details 🤖 Claude に相談した内容
ハーネスは、AIエージェントのうちモデル（LLM）以外の部分すべてを指す。ツールの呼び出し、コンテキスト管理、権限の制御などが含まれる。ハーネスは誰が用意するかによって2層に分けられる。

| | 内部ハーネス（inner harness） | 外部ハーネス（outer harness） |
| --- | --- | --- |
| 用意する人 | エージェントの開発元 | エージェントを使う人 |
| 具体例 | Claude Code や Codex 本体に組み込まれたシステムプロンプト、ツール呼び出し、ループ制御、コンテキスト管理 | CLAUDE.md、MCPサーバー、スキル、フック、レビュー用エージェント、テストやLint |
| 変更できるか | できない（開発元のリリースを待つ） | 自分で自由に変更できる |

![モデルを内部ハーネスが包み、その外側を外部ハーネスが包む2層構造を示した図](/images/books/book-record-of-reading/book059-harness.drawio.png)

- 内部ハーネスは製品に最初から備わっている土台で、利用者は選ぶことしかできない。
- 外部ハーネスは、その上に利用者が自分のプロジェクトに合わせて組み立てる部分。AIにルールを守らせたり、出力をテストやレビューで検証したりする仕組みはここに作る。
- 自分で改善できるのは外部ハーネスなので、「ハーネス設計」で主に工夫するのは外部ハーネスになる。

このレポジトリで言えば、`CLAUDE.md` や `.claude/agents/` 配下のレビュー用エージェントが外部ハーネスにあたる。

参考: https://martinfowler.com/articles/harness-engineering.html
参考: https://codagent.beehiiv.com/p/harnesses-explained
:::

### 📌 外部ハーネスを構成する道具
外部ハーネスを構成する道具は、役割ごとに5つに分けられる。

| 役割 | 道具 | エージェントに与えるもの |
| --- | --- | --- |
| プロジェクトの前提を伝える | CLAUDE.md、スキル、設計資料 | 規約、手順、背景知識、利用可能なコマンド |
| 変更の成否を確かめる | リンター、フォーマッター、型検査、テスト、CI/CD | エラー、失敗理由、合否判定 |
| ツール実行に介入する | Hooks | 実行前後の検査、処理の追加、操作の差し戻し |
| 外部ツールへ接続する | MCP | 外部サービスの参照、操作の手段 |
| 実行範囲を限定する | 権限設定、Devコンテナ、サンドボックス | 操作可能な範囲と事故時の影響範囲 |

![外部ハーネスの5つの道具が、AIエージェントの作業の前・途中・後のどこで働くかを示した図](/images/books/book-record-of-reading/book059-harness-tools.drawio.png)

:::details 🤖 Claude に相談した内容
5つの役割は、AIへの働きかけ方で次のように整理できる。

- 作業の前に知識を渡す: 「プロジェクトの前提を伝える」「外部ツールへ接続する」
- 作業の結果を検証する: 「変更の成否を確かめる」
- 作業中の行動を制御する: 「ツール実行に介入する」「実行範囲を限定する」

**プロジェクトの前提を伝える**
CLAUDE.md は毎回読み込まれる常設のルール、スキルは必要なときだけ読み込まれる手順書にあたる。どちらもAIへのお願い（プロンプト）なので、AIが忘れたり無視したりすることがある。

**変更の成否を確かめる**
リンターやテストの結果は「合格か不合格か」がはっきりしている。エラーメッセージをAIに返せば、AIは失敗理由を読んで自分で修正を繰り返せる。人がレビューしていた検証を仕組みに置き換える中心になる道具。

**ツール実行に介入する**
Hooks は、AIがツールを使う直前・直後などの決まったタイミングで、自分で用意したコマンドを自動で実行する仕組み。詳しくは次の「Hooksとは？」にまとめた。

**外部ツールへ接続する**
MCP（Model Context Protocol）は、AIと外部サービスをつなぐための共通の規格。GitHub、Slack、データベースなどの情報を読んだり操作したりする手段をAIに与える。

**実行範囲を限定する**
ほかの4つは「AIにうまく作業させる」ための道具だが、これは「AIが失敗しても被害を小さくする」ための道具。権限設定で実行できるコマンドを制限し、Devコンテナやサンドボックスで作業できる場所をPCの本体から切り離す。AIに任せる範囲を広げるほど重要になる。

このレポジトリでは、CLAUDE.md（前提を伝える）と `.claude/agents/` 配下のレビュー用エージェント（成否を確かめる）を使っているが、Hooks はまだ設定していない。

参考: https://modelcontextprotocol.io/
:::

### 📌 ワークフローとコンポーネント
コーディングエージェントが指示を受けてから応答を返すまでの流れと、その中で使われる部品（コンポーネント）の関係。

![ユーザーの指示がコマンドやスキルを通してコンテキストに入り、メインループで推論とツール実行を繰り返して、計画書や最終応答を返すまでの流れを示した図](/images/books/book-record-of-reading/book059-workflow-components.drawio.png)
*出典: Software Design 2026年10月号 第1特集「自走AIのためのハーネス設計入門」第2章 図1「ワークフローとコンポーネント」をもとに作成*

1. ユーザーがコマンドやスキルで指示を出し、セッションのコンテキストに入る。ガイドファイル（CLAUDE.md など）は開始時に読み込まれる
2. メインループが始まり、推論とツール実行を繰り返す
3. 必要になったときに、スキルを読み込む
4. ツール（ビルトインツール・MCPツール・サブエージェント）を実行する。サブエージェントは、自分専用の推論とツール実行のループを持つ
5. 計画書をまとめ、最終応答としてユーザーに返す

### 📌 Hooksとは？
AIがツールを使う直前・直後などの決まったタイミングで、自分で用意したコマンドを自動で実行する仕組み。CLAUDE.md の指示と違い、AIの判断に関係なく必ず実行される。

:::details 🤖 Claude に相談した内容
Hooks（フック）は、Claude Code の決まったタイミングで、自分で用意したシェルコマンドを自動で実行する仕組み。外部ハーネスの道具の中では「ツール実行に介入する」役割を担う。

**CLAUDE.md との違い**
Hooks を使ういちばんの理由は、AIの判断に頼らず必ず実行されること。

| | CLAUDE.md | Hooks |
| --- | --- | --- |
| 仕組み | AIへのお願い（プロンプト） | Claude Code 本体がコマンドを実行する |
| 守られるか | AIが忘れたり無視したりすることがある | 条件を満たせば必ず実行される |
| 向いている用途 | 方針や書き方のルール | 絶対に守らせたいチェックや自動処理 |

たとえば CLAUDE.md に「編集したらフォーマッターを実行して」と書いても、AIが実行し忘れることがある。PostToolUse のフックにしておけば、編集のたびに必ずフォーマッターが実行される。

**主な実行タイミング（イベント）**

| イベント | タイミング | 使い方の例 |
| --- | --- | --- |
| `PreToolUse` | AIがツールを使う直前 | 危険なコマンドや、編集禁止ファイルの変更をブロックする |
| `PostToolUse` | ツールを使った直後 | 編集したファイルにフォーマッターやリンターをかける |
| `UserPromptSubmit` | ユーザーが指示を送信したとき | 指示に補足情報を自動で追加する |
| `Stop` | AIが応答を終えようとしたとき | テストが通るまで作業を終わらせない |
| `SessionStart` | セッションを開始したとき | ブランチ名や作業中の課題を最初に読み込ませる |

**終了コードでAIに結果を返す**
フックのコマンドは、終了コードによって Claude Code の動きを変えられる。

- `0`: 成功。そのまま処理が続く。
- `2`: ブロック。`PreToolUse` ならツールの実行が止められ、標準エラー出力のメッセージがAIに渡される。AIはそのメッセージを読んでやり方を変える。
- それ以外: エラーとしてユーザーに表示されるが、処理は止まらない。

![各イベントでフックが実行される順番と、終了コード2でブロックしたときにAIへ処理が戻る流れを示した図](/images/books/book-record-of-reading/book059-hooks.drawio.png)

書籍の表にある「実行前後の検査」「処理の追加」「操作の差し戻し」は、この仕組みで実現する。終了コード `2` でブロックすれば、AIが破れないルールを作れる。

**設定例**
`.claude/settings.json` に書く。次の例は、AIがファイルを編集・作成した直後に、そのファイルを Prettier で整形する。フックには、実行されたツールの情報が JSON で標準入力に渡されるので、`jq` で編集したファイルのパスを取り出している。

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write"
          }
        ]
      }
    ]
  }
}
```

- `matcher` で、どのツールのときに実行するかを指定する（この例では `Edit` と `Write`）。
- フックは自分のPC上で、自分の権限のまま実行される。他人が書いたフックの設定をそのまま使う前に、実行されるコマンドの中身を確認する。

参考: https://docs.claude.com/en/docs/claude-code/hooks
:::

### 📌 最小限のハーネス構築
最小限のハーネスとして、次の4つのファイルを用意する。

```
project/
├── CLAUDE.md
├── .claude/
│   └── settings.json
├── .devcontainer/
│   └── devcontainer.json
└── .github/
    └── workflows/
        └── ci.yaml
```

| ファイル | 外部ハーネスでの役割 | 書く内容 |
| --- | --- | --- |
| `CLAUDE.md` | プロジェクトの前提を伝える | 規約、よく使うコマンド、ディレクトリ構成 |
| `.claude/settings.json` | ツール実行に介入する、実行範囲を限定する | Hooks、許可・禁止するコマンド（権限設定） |
| `.devcontainer/devcontainer.json` | 実行範囲を限定する | AIが作業する環境をコンテナに分け、PC本体から切り離す |
| `.github/workflows/ci.yaml` | 変更の成否を確かめる | push やプルリクエストのたびに、リンター・型検査・テストを実行する |

![4つのファイルが、Devコンテナ内のAIの作業とGitHubのCIのどこで働くかを示した図。同じ検査コマンドをStopフックとCIの両方で実行する](/images/books/book-record-of-reading/book059-minimal-harness.drawio.png)

:::details 🤖 4つのファイルのサンプル（TypeScript の場合）
`package.json` の `scripts` に、リンター・型検査・テストをまとめて実行する `check` がある前提のサンプル。

```json
"scripts": {
  "lint": "eslint .",
  "typecheck": "tsc --noEmit",
  "test": "vitest run",
  "check": "npm run lint && npm run typecheck && npm test"
}
```

**CLAUDE.md**
AIに毎回読ませるルール。検査コマンドと「完了の条件」を書いておくのが大事。

```md
# CLAUDE.md

## コマンド
- 検査をまとめて実行: `npm run check`（リンター・型検査・テスト）
- テストだけ実行: `npm test`

## 規約
- TypeScript の strict モードで書く。`any` は使わない
- 関数を追加・変更したら、tests/ にテストを追加する
- 作業の最後に `npm run check` を実行し、すべて通ってから完了を報告する

## ディレクトリ構成
- src/: アプリケーションのコード
- tests/: テスト（Vitest）
```

**.claude/settings.json**
権限設定で「確認なしで実行してよいコマンド」と「禁止するコマンド」を決め、Hooks で整形と検査を自動化する。

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run check)",
      "Bash(npm test:*)",
      "Bash(git diff:*)"
    ],
    "deny": [
      "Read(./.env)",
      "Bash(git push:*)",
      "Bash(rm -rf:*)"
    ]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write --ignore-unknown"
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "jq -e '.stop_hook_active' >/dev/null && exit 0; npm run check 1>&2 || exit 2"
          }
        ]
      }
    ]
  }
}
```

- `PostToolUse`: AIがファイルを編集・作成するたびに、Prettier で整形する。
- `Stop`: AIが作業を終えようとしたときに `npm run check` を実行し、失敗したら終了コード `2` で終わらせずにエラー内容をAIに返す。
- `stop_hook_active` は、Stop フックによって作業が続けられている最中なら `true` になる。これを確認しないと、検査が通らないときに Stop フックが何度も繰り返される。

**.devcontainer/devcontainer.json**
AIが作業する環境をコンテナに分ける。AIが誤ったコマンドを実行しても、影響をコンテナの中に抑えられる。

```json
{
  "name": "project",
  "image": "mcr.microsoft.com/devcontainers/typescript-node:22",
  "postCreateCommand": "sudo apt-get update && sudo apt-get install -y jq && npm install -g @anthropic-ai/claude-code && npm ci"
}
```

- `image`: Node.js 22 と TypeScript の開発ツールが入った公式イメージ。
- `postCreateCommand`: コンテナを作った直後に、Hooks で使う `jq`、Claude Code、プロジェクトの依存パッケージをインストールする。

**.github/workflows/ci.yaml**
ローカルと同じ `npm run check` を GitHub Actions でも実行する。AIが検査を飛ばしても、プルリクエストの段階で必ず止められる。

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5
      - uses: actions/setup-node@v5
        with:
          node-version: 22
          cache: npm
      - run: npm ci
      - run: npm run check
```

参考: https://docs.claude.com/en/docs/claude-code/settings
参考: https://docs.claude.com/en/docs/claude-code/hooks
参考: https://containers.dev/implementors/json_reference/
:::

:::details 🤖 4つのファイルのサンプル（Python の場合）
パッケージ管理に uv、リンターとフォーマッターに Ruff、型検査に mypy、テストに pytest を使う前提のサンプル。Python には npm の `scripts` にあたる仕組みがないため、検査は次のコマンドでまとめて実行する。

```bash
uv run ruff check . && uv run mypy src && uv run pytest
```

各ツールの設定は `pyproject.toml` にまとめて書く。

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = []

[dependency-groups]
dev = ["ruff", "mypy", "pytest"]

[tool.ruff.lint]
select = ["E", "F", "I", "B", "UP"]

[tool.mypy]
strict = true

[tool.pytest.ini_options]
testpaths = ["tests"]
pythonpath = ["src"]
```

**CLAUDE.md**

```md
# CLAUDE.md

## コマンド
- 検査をまとめて実行: `uv run ruff check . && uv run mypy src && uv run pytest`
- テストだけ実行: `uv run pytest`
- パッケージの追加: `uv add <パッケージ名>`（pip install は使わない）

## 規約
- 関数には型ヒントを付ける（mypy の strict モードで検査している）
- 関数を追加・変更したら、tests/ にテストを追加する
- 作業の最後に検査をまとめて実行し、すべて通ってから完了を報告する

## ディレクトリ構成
- src/app/: アプリケーションのコード
- tests/: テスト（pytest）
```

**.claude/settings.json**

```json
{
  "permissions": {
    "allow": [
      "Bash(uv run ruff:*)",
      "Bash(uv run mypy:*)",
      "Bash(uv run pytest:*)",
      "Bash(git diff:*)"
    ],
    "deny": [
      "Read(./.env)",
      "Bash(git push:*)",
      "Bash(rm -rf:*)"
    ]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "f=$(jq -r '.tool_input.file_path'); case \"$f\" in *.py) uv run ruff format \"$f\" ;; esac"
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "jq -e '.stop_hook_active' >/dev/null && exit 0; (uv run ruff check . && uv run mypy src && uv run pytest) 1>&2 || exit 2"
          }
        ]
      }
    ]
  }
}
```

- `PostToolUse`: AIが編集・作成したファイルが `.py` なら、Ruff で整形する。
- `Stop`: TypeScript 版と同じく、検査が失敗したら終了コード `2` で作業を続けさせる。

**.devcontainer/devcontainer.json**

```json
{
  "name": "project",
  "image": "mcr.microsoft.com/devcontainers/python:3.12",
  "features": {
    "ghcr.io/devcontainers/features/node:1": {}
  },
  "postCreateCommand": "sudo apt-get update && sudo apt-get install -y jq && npm install -g @anthropic-ai/claude-code && pip install uv && uv sync"
}
```

- `image`: Python 3.12 の開発ツールが入った公式イメージ。
- `features`: Claude Code を npm でインストールするために Node.js を追加する。
- `postCreateCommand`: `jq`、Claude Code、uv をインストールし、`uv sync` で依存パッケージをインストールする。

**.github/workflows/ci.yaml**

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5
      - uses: astral-sh/setup-uv@v6
      - run: uv sync
      - run: uv run ruff check .
      - run: uv run mypy src
      - run: uv run pytest
```

検査をステップに分けておくと、GitHub の画面でどの検査が失敗したかがすぐ分かる。

参考: https://docs.astral.sh/uv/
参考: https://docs.astral.sh/ruff/
参考: https://github.com/astral-sh/setup-uv
:::

:::details 🤖 Claude に相談した内容
4つのファイルは、外部ハーネスの5つの役割のうち「外部ツールへ接続する（MCP）」以外をひととおり受け持っている。MCP は外部サービスを使うときに足せばよいので、最初はこの4つで足りる。

実際のプロジェクトでは、この4つに言語ごとの「変更の成否を確かめる」道具（リンター・フォーマッター・型検査・テスト）の設定ファイルを足すと、次のようになる。

**Python（uv + Ruff + mypy + pytest）**
```
project/
├── CLAUDE.md               # 前提を伝える: 規約・よく使うコマンド（uv run pytest など）
├── .claude/
│   └── settings.json       # 介入する: Hooks ／ 範囲を限定する: 権限設定
├── .devcontainer/
│   └── devcontainer.json   # 範囲を限定する: 作業環境をコンテナに閉じ込める
├── .github/
│   └── workflows/
│       └── ci.yaml         # 成否を確かめる: push のたびに下の検査をまとめて実行
├── pyproject.toml          # 成否を確かめる: Ruff・mypy・pytest の設定をまとめて書く
├── uv.lock                 # 依存パッケージのバージョンを固定する
├── src/
│   └── app/
│       └── __init__.py
└── tests/
    └── test_app.py         # 成否を確かめる: テスト
```

**TypeScript（npm + ESLint + Prettier + tsc + Vitest）**
```
project/
├── CLAUDE.md               # 前提を伝える: 規約・よく使うコマンド（npm test など）
├── .claude/
│   └── settings.json       # 介入する: Hooks ／ 範囲を限定する: 権限設定
├── .devcontainer/
│   └── devcontainer.json   # 範囲を限定する: 作業環境をコンテナに閉じ込める
├── .github/
│   └── workflows/
│       └── ci.yaml         # 成否を確かめる: push のたびに下の検査をまとめて実行
├── package.json            # scripts に lint・format・typecheck・test のコマンドを書く
├── package-lock.json       # 依存パッケージのバージョンを固定する
├── tsconfig.json           # 成否を確かめる: 型検査（tsc）の設定
├── eslint.config.js        # 成否を確かめる: リンター
├── .prettierrc             # 成否を確かめる: フォーマッター
├── vitest.config.ts        # 成否を確かめる: テストの設定
├── src/
│   └── index.ts
└── tests/
    └── index.test.ts       # 成否を確かめる: テスト
```

どちらの言語でも、AIに渡す検査コマンドを1つにまとめておくと扱いやすい。Python なら `uv run ruff check && uv run mypy src && uv run pytest`、TypeScript なら `package.json` の `scripts` に `"check": "npm run lint && npm run typecheck && npm test"` のように書く。このコマンドを CLAUDE.md に書き、Hooks や CI からも同じコマンドを呼ぶ。そうすると、AIがローカルで確かめる内容と CI で確かめる内容が揃う。
:::

### 📌 ハーネスは日々の保守が必要
実質的に機能効果を失ったハーネスは、残っていると有害になる。そのため、ハーネスは作って終わりではなく、日々の保守が必要になる。

:::message
ハーネスを追加するときに「消す条件」も書いておくと、他のメンバーが保守しやすい。
:::

:::details 🤖 Claude に相談した内容
**効果を失ったハーネスが有害になる理由**
ハーネスは「AIに必ず守らせる」ための仕組みなので、古くなっても誰も気づかないまま効き続ける。

| 残っているハーネスの例 | 起きる問題 |
| --- | --- |
| もう使っていないライブラリのルールが書かれた CLAUDE.md | 毎回読み込まれてコンテキストを消費する。AIが古いルールに従って間違ったコードを書く |
| 移行済みのディレクトリを編集禁止にする PreToolUse フック | 必要な変更までブロックされ、AIが回り道をする |
| モデルの性能が上がって不要になった細かい指示 | 指示どうしが矛盾したときに、本当に大事なルールが守られにくくなる |
| 一時的に広げたまま戻していない権限設定 | 実行範囲の限定が実質的に効かなくなる |

**消す条件の書き方の例**
ルールと一緒に「なぜ追加したか」と「いつ消してよいか」を残しておくと、追加した本人以外でも消してよいか判断できる。

```md
## 規約
- API の呼び出しは src/lib/api-client.ts を経由する
  - 理由: 旧クライアント（src/legacy/）からの移行中のため（2026-09 追加）
  - 消す条件: src/legacy/ を削除したら、このルールも消す
```

JSON はコメントを書けないので、`.claude/settings.json` の Hooks や権限設定の理由と消す条件は、CLAUDE.md や README にまとめて書いておく。

**保守のしかた**
- AIが同じ失敗を繰り返したらハーネスを足し、ハーネスが原因でAIが回り道をしていたら見直す。
- モデルや Claude Code を更新したタイミングで、不要になった指示がないかを確認する。
- 消す条件を満たしたルールは、その場で消す。
:::

### 📌 AIのPlanモードでは何が起きているのか？
Planモードでは、AIはファイルの読み込みや検索だけで調査と計画を行い、ファイルの編集やコマンドの実行は仕組みとして止められている。人が計画を承認すると、通常のモードに切り替わって変更を始める。

:::details 🤖 Claude に相談した内容
Planモードは、Claude Code に「調べて計画を立てるところまで」をさせ、ファイルの変更は人が計画を承認するまで止めておくモード。`Shift + Tab` で切り替えるか、`claude --permission-mode plan` で起動する。

**Planモード中に起きていること**
Planモードは権限モードの1つで、モデル（LLM）が別のものに切り替わるわけではない。内部ハーネスが次の2つを行っている。

1. モデルに「今はPlanモードなので、調査と計画だけを行う」という指示を追加する
2. ファイル編集やコマンド実行など、変更を伴うツールを使えないようにし、読み取り系のツールだけを許可する

1だけだとAIが指示を無視して編集してしまう可能性があるが、2で仕組みとして止めているので、承認前にファイルが変更されることはない。

![Planモードでは読み取り系のツールだけで調査と計画を行い、人が承認すると通常のモードに切り替わって変更を始める流れを示した図](/images/books/book-record-of-reading/book059-plan-mode.drawio.png)

| | Planモード | 通常のモード |
| --- | --- | --- |
| 使えるツール | ファイルの読み込み・検索・Web検索などの読み取り系 | 編集・コマンド実行も含むすべて（権限設定の範囲内） |
| AIが出すもの | 調査結果と実装の計画 | コードの変更 |
| 人がすること | 計画を読んで、承認するか修正を依頼する | 変更内容を確認する |

**ハーネスとしての意味**
Planモードは、「実行範囲を限定する」仕組みと「人が確認する」仕組みを組み合わせたもの。AIが間違った方向に進んでいても、コードを変更する前に気づいて止められる。変更が大きい作業や、影響範囲が分からない作業ほど効果が大きい。

Hooks や CI が「変更した後」に検証するのに対して、Planモードは「変更する前」に確認する。担当するタイミングが違うので、組み合わせて使う。

参考: https://docs.claude.com/en/docs/claude-code/common-workflows
:::

### 📌 テストケース生成スキルの最小形
仕様書やソースコードから、テスト観点とテストケースを生成するスキル（プロンプト定義）の最小形（汎用版）。QAエンジニアの頭の中にあった観点やルールをファイルに書き出し、AIが毎回同じ基準でテストケースを作れるようにしている。

```md
---
name: testcase-generator
description: 仕様書またはソースコードからテスト観点とテストケースを生成する
---

## 必ず最初に読むファイル
- ./knowledge/perspectives.md
- ./knowledge/style-guide.md

## 入力
- 対象の仕様書、または対象コードのパス／プルリクエストURL

## 手順
1. 入力を読み、対象の機能範囲を把握する。
2. 各観点を「対象／対象外」に判定し、対象外には理由を1行添える。
3. 「対象」の観点ごとにテストケースを Gherkin 形式で書く。
4. 各テストケースに優先度タグ（@P1/@P2/@P3）と実行方法タグを付ける。
   @UI（画面操作）/@API（API経由）/@AUTO（自動テストのみ・手動対象外）

## 出力形式（Gherkin・厳守）
Feature: ＜機能名＞
  Scenario: ＜検証内容を一文で＞
    Given ＜前提＞
    When ＜操作＞
    Then ＜期待結果＞

## 禁止事項
- 曖昧語の禁止: 「正しく」「適切に」「問題なく」は使わない。
- 内部用語の禁止: APIパス・UPPER_SNAKE_CASE・変数名を残さない。
```
*出典: Software Design 2026年10月号 第2特集「AIによるテスト自動化のリアル」をもとに作成*

![観点リストとスタイルガイドを読んだAIが、仕様書やコードから観点の判定、Gherkinでのテストケース作成、タグ付けを行い、タグに応じて実行方法が分かれる流れを示した図](/images/books/book-record-of-reading/book059-testcase-skill.drawio.png)

:::details 🤖 Claude に相談した内容
**各部分の役割**

| 部分 | 役割 |
| --- | --- |
| `name` / `description` | スキルの名前と説明。Claude Code は `description` を見て、依頼内容に合うスキルを自動で選ぶ |
| 必ず最初に読むファイル | テスト観点の一覧（perspectives.md）と書き方のルール（style-guide.md）。QAエンジニアの暗黙知を文章にしたもの |
| 手順 | 観点の判定 → テストケース作成 → タグ付けの順番を固定し、毎回同じ流れで作らせる |
| 出力形式 | Gherkin 形式に揃えることで、人が読みやすく、自動テストのツールにも渡しやすくなる |
| 禁止事項 | 「正しく動く」のような検証できない書き方や、読み手に伝わらない内部用語を防ぐ |

**工夫されている点**
- 観点を「対象外」にするときも理由を書かせるので、AIが観点を黙って飛ばしたのか、意図して外したのかを人が確認できる。
- 観点やルールをスキル本体ではなく knowledge/ に分けているので、観点を追加するときにスキル本体を変更しなくてよい。
- 「曖昧語の禁止」は、期待結果を「〇〇が表示される」のように確認できる形で書かせるためのルール。AIは「正しく動作する」のような書き方をしがちなので、はっきり禁止している。

**ファイルの配置例（Claude Code のスキルとして置く場合）**

```
.claude/
└── skills/
    └── testcase-generator/
        ├── SKILL.md              # 上のスキル定義
        └── knowledge/
            ├── perspectives.md   # テスト観点の一覧（境界値、権限、エラー時の表示など）
            └── style-guide.md    # テストケースの書き方のルール
```

**出力されるテストケースの例**

```gherkin
Feature: ログイン
  @P1 @UI
  Scenario: 登録済みのメールアドレスとパスワードでログインするとホーム画面が表示される
    Given ログイン画面を開いている
    When 登録済みのメールアドレスとパスワードを入力してログインボタンを押す
    Then ホーム画面が表示される
```

参考: https://docs.claude.com/en/docs/claude-code/skills
参考: https://cucumber.io/docs/gherkin/reference/
:::

### 📌 テストにおける最小ハーネス3点セット
テストケース生成スキルが毎回参照する知識（観点のひな形・スタイルガイド）と、その知識に書かれたルールをAIが破れないようにする仕組み（検証Hook）の3点セット。観点のひな形は「どこを見るか」、スタイルガイドは「どう書くか」を決め、検証Hookがそれを守らせる。

![スキルが観点のひな形とスタイルガイドを読んでテストケースを書き、検証Hookが書き込みのたびに検証してAIに差し戻す流れと、見つかった穴を観点に足し続けるループを示した図](/images/books/book-record-of-reading/book059-test-harness-3set.drawio.png)

**① テスト観点のひな形**

```md
# ① 観点ひな形 — knowledge/perspectives.md（どこを見るか）

## 入力値テスト
- [ ] 正常値（区分ごとに最低1件）
- [ ] 境界値（最小・最大・最小-1・最大+1を分けて作成）
- [ ] 異常値（型違い・空・null・特殊文字）
- [ ] 金額・数値計算が存在する場合は最優先でテストケース化する

## 過去の失敗から得た観点（踏んだ穴をここに足し続ける）
- [ ] 画面に存在しないボタン・動線を前提にしない（UI用語ガイドと突合）
```

**② スタイルガイド**

```md
# ② スタイルガイド — knowledge/style-guide.md（どう書くか）

## Gherkin 文末ルール
- Given:「〜している」 When:「〜する」
- Then :「〜される」「〜こと」（確認内容を具体的な表示文言で）

## 禁止表現
- 曖昧語禁止:「正しく」「適切に」「問題なく」は使わない
```

**③ 検証Hook**

```bash
#!/usr/bin/env bash
# ③ 検証Hook — hooks/validate.sh（ルールを破れなくする・最小例）
set -euo pipefail
input=$(cat)  # Claude Codeは書き込みイベントの情報をstdinのJSONで渡す
target=$(printf '%s' "$input" | jq -r '.tool_input.file_path // empty')
[ -n "$target" ] || exit 0  # 対象パスが取れないイベントは素通し

# 検証1: あいまい語の混入を BLOCK（exit 2 でAIに差し戻す）
if grep -nE '正しく|適切に|問題なく' "$target"; then
  echo "BLOCK: 曖昧語が残っています。具体的な表示文言に書き換えてください。" >&2
  exit 2
fi

# 検証2: 内部用語（APIパス・大文字定数）の裸残りを WARN。
#   まず WARN で観測し、UI用語ガイドが整ったら BLOCK へ上げる。
if grep -nE '/api/|[A-Z]{2,}_[A-Z]{2,}' "$target"; then
  echo "WARN: 内部用語の可能性。UI用語ガイドと突き合わせてください。" >&2
fi

# 検証3: 観点とテストケースの双方向整合（発展課題。対応スクリプトを用意した場合のみ実行）
bidir="${CLAUDE_PROJECT_DIR:-.}/hooks/check_bidirectional.sh"
if [ -x "$bidir" ]; then
  "$bidir" "$target" || {
    echo "BLOCK: 観点とテストケースの対応が双方向で一致していません。" >&2
    exit 2
  }
fi

echo "OK: 検証を通過しました。" >&2
```
*出典: Software Design 2026年10月号 第2特集「AIによるテスト自動化のリアル」リスト1〜3をもとに作成*

:::details 🤖 Claude に相談した内容
**3点セットの関係**
①と②は「テストケース生成スキルの最小形」で「必ず最初に読むファイル」に指定されていた knowledge/ の中身にあたる。ただし、①②はAIへのお願い（プロンプト）なので、AIが守らないことがある。そこで③の検証Hookで、ルールを破ったテストケースが書き込まれたら仕組みとして差し戻す。

| | ファイル | 外部ハーネスでの役割 | 決めること |
| --- | --- | --- | --- |
| ① | knowledge/perspectives.md | プロジェクトの前提を伝える | どこを見るか（テスト観点） |
| ② | knowledge/style-guide.md | プロジェクトの前提を伝える | どう書くか（文末・禁止表現） |
| ③ | hooks/validate.sh | ツール実行に介入する | ①②のルールを破らせない |

**検証Hookの3つの検証**

| 検証 | 見つけるもの | 見つけたとき |
| --- | --- | --- |
| 検証1 | 曖昧語（正しく・適切に・問題なく） | BLOCK: 終了コード `2` でAIに書き直させる |
| 検証2 | 内部用語（`/api/` や `UPPER_SNAKE` のような大文字定数） | WARN: 警告を出すだけで止めない |
| 検証3 | 観点とテストケースの対応のずれ | BLOCK（対応するスクリプトがあるときだけ実行） |

検証2をいきなり BLOCK にしないのは、誤検知が多いと必要な作業まで止まってしまうから。まず WARN でどれくらい引っかかるかを観察し、判断の基準になるUI用語ガイドが整ってから BLOCK に上げる。ルールを段階的に強くしていく進め方は、ほかのハーネスにも使える。

**Hookとして登録する**
`validate.sh` は、`.claude/settings.json` に PostToolUse のフックとして登録して使う。

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/hooks/validate.sh"
          }
        ]
      }
    ]
  }
}
```

- PostToolUse は書き込みの後に実行されるので、終了コード `2` でもファイルの書き込み自体は取り消されない。書き込まれた内容とエラーメッセージがAIに返され、AIが書き直す。
- このままだとソースコードなど、テストケース以外のファイルにも検証がかかる。「正しく」を含むコメントなどで止まらないように、`case "$target" in *.feature) ;; *) exit 0 ;; esac` のようにテストケースのファイルだけに絞るとよい。

**観点のひな形は育てていくもの**
①の「過去の失敗から得た観点（踏んだ穴をここに足し続ける）」がポイント。テストで見逃した不具合が見つかるたびに、その観点をひな形に足していくと、同じ見逃しをAIが繰り返さなくなる。「ハーネスは日々の保守が必要」の実例でもある。

参考: https://docs.claude.com/en/docs/claude-code/hooks
:::

### 📌 AIによるテスト自動化のポイント
基本方針は「**機械可読を先に、人間可読を後に**」。まずAIが読める形で情報を整え、人が読みやすい形は後から整える。

1. GitHubにすべての情報を集約する
2. Gherkin形式で粒度をそろえる
3. テスト観点シートは、tsv
4. ドメイン知識用の用語集の用意
5. 知識は `./knowledge/~.md` に格納＆参照
6. Playwright MCPで一度操作してもらい、そのスクリプトをコード化する

![GitHubに集約した観点シート・用語集・knowledgeをAIが読んでGherkinのテストケースを書き、Playwright MCPで画面を一度操作してからテストコードにする流れを示した図](/images/books/book-record-of-reading/book059-test-automation.drawio.png)

:::details 🤖 Claude に相談した内容
**なぜ「機械可読が先」なのか**
AIは、読める場所にあり、読める形式で書かれた情報しか使えない。スプレッドシートの画像や口頭での申し送り、社内チャットに散らばった情報は、人には読めてもAIには渡せない。先にAIが読める形に整えておけば、人が読むための資料（一覧表やレポートなど）は、そこから後で作り出せる。

| ポイント | 理由 |
| --- | --- |
| 1. GitHubに集約 | AIが参照できる場所を1か所にまとめる。変更の履歴も残る |
| 2. Gherkinで粒度をそろえる | Given/When/Then の型に当てはめると、人によって粒度がばらつかない。自動テストのコードにも変換しやすい |
| 3. 観点シートはtsv | Excel などの形式はAIが読みにくく、Gitで差分も見えない。tsv はただのテキストなのでAIが読めて差分も見える。タブ区切りなので、日本語の文中にカンマがあっても列がずれにくい |
| 4. 用語集を用意 | 画面の表示名と、コード上の名前（APIパスや定数名）の対応を書いておく。3点セットの検証2で使う「UI用語ガイド」にもなる |
| 5. knowledge/ に格納 | スキルから「必ず最初に読むファイル」として参照させる |
| 6. Playwright MCPで一度操作 | AIが画面を見ずにテストコードを書くと、存在しないボタンや動線を前提にしてしまう。先に実際の画面を操作させ、その手順をもとにコード化すると、画面と食い違わないテストになる |

**Playwright MCP とは**
ブラウザ自動操作ツールの Playwright を、MCP（外部ツールへ接続する仕組み）経由でAIから使えるようにしたもの。AIがページを開く、クリックする、入力するといった操作を実際に行える。

- 6は「テスト観点のひな形」の「画面に存在しないボタン・動線を前提にしない」に対する対策にもなっている。
- 一度操作したあとは、Playwright のテストコードとして保存して CI で繰り返し実行する。毎回AIにブラウザを操作させるより速く、結果も安定する。

参考: https://github.com/microsoft/playwright-mcp
参考: https://cucumber.io/docs/gherkin/reference/
:::

### 📌 デカルト積問題とは？
ORMで複数の1対多の関連をまとめてJOINで読み込むと、関連の件数どうしが掛け算になって、取得する行数が爆発的に増える問題。

:::details 🤖 Claude に相談した内容
デカルト積（直積）は、2つの集合の要素をすべての組み合わせで並べたもの。要素数が2個と3個なら、2 × 3 = 6通りになる。

ORMで1対多の関連を複数まとめて読み込む（eager loading）と、SQLでは複数の関連を1つのクエリでJOINする。このとき、関連どうしに関係がないと行数が掛け算で増える。これがデカルト積問題。

```sql
-- ユーザー1人に、注文が10件、住所が5件あるとき
SELECT *
FROM users u
JOIN orders    o ON o.user_id = u.id
JOIN addresses a ON a.user_id = u.id;
-- 欲しいのは注文10件と住所5件の計15件だが、1人あたり 10 × 5 = 50行 が返る
```

![JOINでまとめて読み込むと行数が掛け算で増え、関連ごとにクエリを分けると足し算で済むことを示した図](/images/books/book-record-of-reading/book059-cartesian.drawio.png)

- ユーザーや関連の件数が増えるほど、転送量とメモリ使用量が急増して遅くなる。
- 同じユーザーの列が何度も重複して返るため、無駄なデータも多い。
- 対策は、関連ごとにクエリを分けて読み込むこと。EF Coreの `AsSplitQuery()` のように、ORMに分割クエリの機能が用意されていることが多い。ただし、クエリの回数は増える。

ロード戦略では、関連を必要になった時点で読み込むとクエリが大量に発行される「N+1問題」と、まとめてJOINで読み込むと行数が爆発する「デカルト積問題」のバランスを取ることになる。

参考: https://learn.microsoft.com/ja-jp/ef/core/querying/single-split-queries
:::

### 📌 N+1問題とは？
一覧を取得するクエリを1回実行したあと、一覧の件数（N件）の分だけ関連データを取得するクエリが1件ずつ実行され、合計 1 + N 回のクエリが発行される問題。

:::details 🤖 Claude に相談した内容
ORMで関連データを「必要になった時点で読み込む」（lazy loading、遅延読み込み）設定にしていると、ループの中で関連データにアクセスするたびにクエリが発行される。

```csharp
// 遅延読み込みが有効なとき
var users = context.Users.ToList();        // 1回目: ユーザー一覧を取得
foreach (var user in users)
{
    Console.WriteLine(user.Orders.Count);  // ユーザーごとに注文を取得（N回）
}
```

実際に発行されるSQLは次のようになる。

```sql
SELECT * FROM users;                        -- 1回
SELECT * FROM orders WHERE user_id = 1;     -- ユーザー1人目
SELECT * FROM orders WHERE user_id = 2;     -- ユーザー2人目
SELECT * FROM orders WHERE user_id = 3;     -- ユーザー3人目
-- ユーザーが1,000人いれば、合計 1 + 1,000 = 1,001回
```

![N+1問題ではユーザーの人数分だけ注文を取得するクエリが発行され、まとめて読み込むとクエリが2回で済むことを示した図](/images/books/book-record-of-reading/book059-n-plus-1.drawio.png)

- 1回ごとのクエリは速くても、DBとの往復が件数の分だけ発生するので、全体では遅くなる。
- コードを見ただけでは、ループの中でクエリが発行されていることに気づきにくい。開発中はデータが少なく問題にならず、本番のデータ量で初めて遅くなることが多い。
- 対策は、関連データを最初にまとめて読み込むこと（eager loading）。EF Core なら `Include()` を使う。

```csharp
var users = context.Users
    .Include(u => u.Orders)  // 注文もまとめて読み込む
    .ToList();
```

**デカルト積問題との関係**
`Include()` で関連をまとめて読み込むと、今度はJOINによる「デカルト積問題」が起きる可能性がある。とくに1対多の関連を複数まとめて読み込むときに起きやすい。

| 読み込み方 | クエリの回数 | 起きやすい問題 |
| --- | --- | --- |
| 必要になった時点で読み込む（遅延読み込み） | 1 + N 回 | N+1問題 |
| まとめてJOINで読み込む（`Include()`） | 1回 | デカルト積問題 |
| 関連ごとに分けて読み込む（`Include()` + `AsSplitQuery()`） | 関連の数 + 1 回 | 大きな問題は起きにくいが、クエリの回数は少し増える |

参考: https://learn.microsoft.com/ja-jp/ef/core/querying/related-data/
参考: https://learn.microsoft.com/ja-jp/ef/core/performance/efficient-querying
:::



