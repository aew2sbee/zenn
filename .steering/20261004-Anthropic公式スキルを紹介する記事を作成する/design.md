<!-- AI向け: 見出しは追加しない。事実のみを書き、所感・補足は書かない。requirements.md の要件をどう実現するかを書く。 -->

# 設計

## 🌱 方針
<!-- AI向け: 実現方法の全体像を1〜3文で書く。 -->
公式リポジトリをクローンし、各スキルの SKILL.md と `.claude-plugin/marketplace.json` を一次情報として記事を書く。スキルは用途別の5カテゴリに分類し、冒頭に早見表を置く。

## 🌱 作業の流れ
<!-- AI向け: 1単位の作業を番号付きリストで順に書く。 -->
1. main から `docs/claude-code-official-skills` ブランチを作成する
2. 公式リポジトリをスクラッチパッドにクローンし、各スキルの内容を確認する
3. 記事を作成する
4. レビュー用サブエージェントでレビューし、指摘を反映する
5. コミットし、push して PR を作成する

## 🌱 詳細
<!-- AI向け: 流れの各ステップで必要なルール（命名規則・判断基準・使うツールなど）を書く。形式は自由。 -->

### 調査対象
- リポジトリ: https://github.com/anthropics/skills
- コミット: `8a1541c`（2026-09-28）

### 記事の構成
1. はじめに
2. 結論（用途別の早見表）
3. スキルとは
4. 公式リポジトリの構成（プラグイン5種）
5. インストール方法（Claude Code / Claude.ai / API）
6. 用途別のスキル紹介
7. 自作スキルを作るときの参考
8. おわりに

### スキルの分類

| カテゴリ | スキル |
|---|---|
| ドキュメント作成 | docx / xlsx / pptx / pdf / doc-coauthoring |
| 開発 | claude-api / mcp-builder / webapp-testing / web-artifacts-builder / skill-creator |
| デザイン・クリエイティブ | frontend-design / canvas-design / algorithmic-art / theme-factory / brand-guidelines / slack-gif-creator |
| 社内コミュニケーション | internal-comms |
| 学習・回答の補助 | academy-guide / discernment-nudge |

### Front Matter
- title: `[Claude Code] Anthropic公式スキル全19個を用途別に紹介する`
- topics: `claudecode`, `claude`, `ai`, `初心者向け`
- published: false（ユーザー確認後に true へ変更する）

### コミット
- `docs: Anthropic公式スキルを紹介する記事を追加`

## 🌱 確認方法
<!-- AI向け: requirements.md の完了条件をどう確かめるかを書く。 -->
- プレビュー: `npx zenn preview` で記事を開き、表示崩れがないことを確認する
- 網羅性: 記事内のスキル名と `skills/` 配下のディレクトリ名を突き合わせ、19個すべてが含まれることを確認する
- レビュー: fact-checker・beginner-reviewer・zenn-reviewer・japanese-reviewer で確認する
