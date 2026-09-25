<!-- AI向け: 見出しは追加しない。事実のみを書き、所感・補足は書かない。requirements.md の要件をどう実現するかを書く。 -->

# 設計

## 🌱 方針
<!-- AI向け: 実現方法の全体像を1〜3文で書く。 -->
対象の15記事を、1記事につき1ブランチ・1PRで移行する。旧リポジトリから記事と参照画像をコピーし、サブエージェントのレビュー指摘を修正してからPRを作成する。

## 🌱 作業の流れ
<!-- AI向け: 1単位の作業を番号付きリストで順に書く。 -->
1. main を最新化し、`docs/<slug>` ブランチを作成する
2. 記事ファイルと、記事が参照する画像ファイルをコピーする
3. 記事の追加をコミットする
4. サブエージェントでレビューする
5. レビュー指摘を修正してコミットする
6. `npx zenn preview` で表示を確認する
7. push して PR を作成する
8. tasklist.md の該当項目にチェックを付ける

## 🌱 詳細
<!-- AI向け: 流れの各ステップで必要なルール（命名規則・判断基準・使うツールなど）を書く。形式は自由。 -->

### ブランチ
- ブランチ名: `docs/<slug>`（例: `docs/typescript-reduce`）
- ベースブランチ: main

### コピー
- コピー元: `C:\Users\aew2s\work\zenn\articles\<slug>.md`
- コピー先: `C:\Users\aew2s\work\dev-zenn\articles\<slug>.md`
- ファイル名（スラッグ）は変更しない

### 画像
- 記事本文から `/images/articles/` で始まる参照パスを抽出し、同じパスで `C:\Users\aew2s\work\zenn\images\` から `C:\Users\aew2s\work\dev-zenn\images\` へコピーする
- 画像ディレクトリ名はスラッグから推測せず、参照パスに従う
- 外部URL（`https://` で始まるもの）の画像はコピーしない
- 記事が参照していない画像はコピーしない
- 画像を参照する記事と枚数

| スラッグ | 画像ディレクトリ | 枚数 |
|---|---|---|
| aws-ec2-iam-role | images/articles/aws-ec2-iam-role | 21 |
| django-install | images/articles/django-install | 1 |
| django-rest-framework-install | images/articles/django-rest-framework-install | 8 |
| yarn-error-no-such-option | images/articles/yarn-error-no-such-option | 1 |

### Front Matter
- published は旧リポジトリの値を引き継ぎ、レビュー指摘があっても変更しない
- published 以外の項目は旧リポジトリの値を引き継ぐ。レビュー指摘がある場合のみ修正する

### 本文
- 「🌱 参考」セクションは削除せず残す

### コミット
- 記事の追加: `docs: <記事の内容>の記事を追加`
- 指摘の修正: `docs: <修正内容>`

### レビュー
- `.claude/agents/` の以下11エージェントを並列で実行する

| エージェント | 観点 |
|---|---|
| security-reviewer | 秘密情報・個人情報・内部情報（画像を含む） |
| zenn-reviewer | Front Matter・Zenn独自記法 |
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
- タイトル: 記事追加のコミットメッセージと同じ
- 本文: `.github/pull_request_template.md` の形式に従い、変更点に published の値と修正内容を書く
- 例: `- articles/typescript-reduce.md（published: true）: 旧リポジトリから移行。〜を修正`

## 🌱 確認方法
<!-- AI向け: requirements.md の完了条件をどう確かめるかを書く。 -->
- tasklist.md: 全項目にチェックが付いていることを目視で確認する
- 記事の存在: 対象15件の `articles/<slug>.md` が main に存在することを `git ls-tree main articles/` で確認する
- 画像の存在: 各記事の `/images/articles/` 参照パスを抽出し、すべてのファイルが `images/` 配下に存在することを確認する
- 表示: `npx zenn preview` で対象15件を開き、本文と画像が表示されることを確認する
