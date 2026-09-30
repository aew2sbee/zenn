<!-- AI向け: 見出しは追加しない。事実のみを書き、所感・補足は書かない。 -->

# 要件定義

## 🌱 背景
<!-- AI向け: 作業が必要な理由を1〜2文で書く。例: 旧リポジトリの記事を本リポジトリへ移行するため -->
- Zennで公開中の記事 `github-branch-protection-ruleset` の画像7件がすべてリンク切れになっている
- 公開ページの画像URL（`https://static.zenn.studio/user-upload/deployed-images/*.png?sha=...`）は、すべて HTTP 404 を返す
- 画像URLの `sha` は、リポジトリ内の画像ファイルのgitハッシュと一致している
- リポジトリ内の画像ファイルは `images/articles/github-branch-protection-ruleset/` 配下に7件存在し、gitで管理されている

## 🌱 対象範囲
<!-- AI向け: 今回の作業で扱うものを「- 」で始まる1行ずつ書く。例: - articles/typescript-reduce.md -->
- articles/github-branch-protection-ruleset.md
- images/articles/github-branch-protection-ruleset/ 配下の画像7件

## 🌱 対象外
<!-- AI向け: 今回扱わないものを「- 」で始まる1行ずつ書く。ない場合は「なし」と書く。例: - 記事本文のリライト -->
- 他の記事の画像リンク切れ
- 記事本文のリライト
- 画像の差し替え・撮り直し

## 🌱 要件
<!-- AI向け: 成果物が満たすべき条件を「- 」で始まる1行ずつ書く。1項目は1条件にする。例: - Front Matterの published は旧リポジトリの値を引き継ぐ -->
- リンク切れの原因を特定する
- 公開ページで記事内の画像7件がすべて表示される状態にする
- 記事内の画像パスは `/images/articles/github-branch-protection-ruleset/` 配下を参照する形式を維持する
- 修正は専用ブランチで行い、PRを作成する

## 🌱 制約
<!-- AI向け: 守るべき前提・禁止事項を「- 」で始まる1行ずつ書く。ない場合は「なし」と書く。例: - 記事本文の内容は変更しない -->
- 記事のファイル名（スラッグ）は変更しない
- 記事本文の内容は変更しない
- Zenn固有の記法（`:::message` など）は変更しない

## 🌱 完了条件
<!-- AI向け: 完了とみなせる状態を「- [ ] 」で始まる1行ずつ書く。1項目は1条件にし、確認できる状態で書く。例: - [ ] 対象記事が articles/ 配下に存在し、npx zenn preview で表示できる -->
- [ ] リンク切れの原因が design.md に記載されている
- [ ] npx zenn preview で記事内の画像7件がすべて表示できる
- [ ] 公開ページの画像URL7件がすべて HTTP 200 を返す
- [ ] 公開ページで記事内の画像7件がすべて表示できる
