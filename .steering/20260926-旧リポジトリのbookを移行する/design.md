<!-- AI向け: 見出しは追加しない。事実のみを書き、所感・補足は書かない。requirements.md の要件をどう実現するかを書く。 -->

# 設計

## 🌱 方針
<!-- AI向け: 実現方法の全体像を1〜3文で書く。 -->
対象の6冊を、1冊につき1ブランチ・1PRで移行する。旧リポジトリからbookディレクトリと参照画像をコピーし、チャプターを約20章ずつに分けてサブエージェントでレビューし、指摘を修正してからPRを作成する。

## 🌱 作業の流れ
<!-- AI向け: 1単位の作業を番号付きリストで順に書く。 -->
1. main を最新化し、`docs/<slug>` ブランチを作成する
2. bookディレクトリと、bookが参照する画像ファイルをコピーする
3. bookの追加をコミットする
4. サブエージェントでレビューする
5. レビュー指摘を修正してコミットする
6. `npx zenn preview` で表示を確認する
7. push して PR を作成する
8. TODO.md の該当項目にチェックを付ける

## 🌱 詳細
<!-- AI向け: 流れの各ステップで必要なルール（命名規則・判断基準・使うツールなど）を書く。形式は自由。 -->

### ブランチ
- ブランチ名: `docs/<slug>`（例: `docs/learn-react-tutorial`）
- ベースブランチ: main

### コピー
- コピー元: `C:\Users\aew2s\work\zenn-bk\books\<slug>\`
- コピー先: `C:\Users\aew2s\work\zenn\books\<slug>\`
- ディレクトリ内の `config.yaml`・`cover.png`・チャプターの `.md` をすべてコピーする
- ディレクトリ名（スラッグ）とチャプターのファイル名は変更しない

### 画像
- チャプター本文から `/images/books/` で始まる参照パスを抽出し、同じパスで `C:\Users\aew2s\work\zenn-bk\images\` から `C:\Users\aew2s\work\zenn\images\` へコピーする
- 画像ディレクトリ名はスラッグから推測せず、参照パスに従う
- 外部URL（`https://` で始まるもの）の画像はコピーしない
- bookが参照していない画像はコピーしない
- 画像を参照するbookと枚数（`cover.png` を除く）

| スラッグ | 画像ディレクトリ | 枚数 |
|---|---|---|
| learn-storybook-tutorial | images/books/learn-storybook-tutorial | 8 |

### 対象bookの規模

| スラッグ | チャプター数 | レビューのバッチ数 |
|---|---|---|
| book-record-of-reading | 58 | 3 |
| github-copilot | 27 | 2 |
| github-foundations | 100 | 5 |
| github-foundations-part-2 | 46 | 3 |
| learn-react-tutorial | 20 | 1 |
| learn-storybook-tutorial | 4 | 1 |

### config.yaml
- published は旧リポジトリの値を引き継ぎ、レビュー指摘があっても変更しない
- chapters は旧リポジトリの値を引き継ぎ、レビュー指摘があっても変更しない
- github-copilot の chapters には存在しない章（question027〜099）が含まれるが、変更しない
- published・chapters 以外の項目は旧リポジトリの値を引き継ぐ。レビュー指摘がある場合のみ修正する

### チャプター
- Front Matter の title は旧リポジトリの値を引き継ぐ。レビュー指摘がある場合のみ修正する
- 「🌱 参考」セクションは削除せず残す

### コミット
- bookの追加: `docs: <bookの内容>のbookを追加`
- 指摘の修正: `docs: <修正内容>`

### レビュー
- チャプターを `config.yaml` の chapters の順に約20章ずつのバッチに分ける
- 1バッチごとに `.claude/agents/` の以下11エージェントを並列で実行する
- `config.yaml` と `cover.png` は1バッチ目に含める

| エージェント | 観点 |
|---|---|
| security-reviewer | 秘密情報・個人情報・内部情報（画像・cover.png を含む） |
| zenn-reviewer | config.yaml・Front Matter・Zenn独自記法 |
| technical-reviewer | 技術的な正確性 |
| fact-checker | バージョン・仕様・数値の最新性 |
| source-checker | 出典の有無と一致 |
| code-reviewer | コード・コマンドの動作 |
| reproducibility-reviewer | 手順の再現性 |
| structure-reviewer | 構成と論理の流れ |
| beginner-reviewer | 初学者の理解しやすさ |
| japanese-reviewer | 誤字脱字・表記ゆれ |
| ai-writing-reviewer | 定型的・冗長な表現 |

- security-reviewer の指摘に該当する記述は削除する
- 他の指摘は修正する
- `:::message` など Zenn 独自記法は CLAUDE.md の禁止事項に従い変更しない

### PR
- タイトル: bookの追加のコミットメッセージと同じ
- 本文: `.github/pull_request_template.md` の形式に従い、変更点に published の値と修正内容を書く
- 例: `- books/learn-react-tutorial（published: true）: 旧リポジトリから移行。〜を修正`

## 🌱 確認方法
<!-- AI向け: requirements.md の完了条件をどう確かめるかを書く。 -->
- TODO.md: 全項目にチェックが付いていることを目視で確認する
- bookの存在: 対象6件の `books/<slug>/` が main に存在することを `git ls-tree main books/` で確認する
- チャプターの存在: 各bookのチャプターファイル一覧が `zenn-bk` と一致することを確認する
- 画像の存在: 各bookの `/images/books/` 参照パスを抽出し、すべてのファイルが `images/` 配下に存在することを確認する
- 表示: `npx zenn preview` で対象6件を開き、本文・cover.png・画像が表示されることを確認する（github-copilot の question027〜099 は対象外）
