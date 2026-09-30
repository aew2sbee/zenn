<!-- AI向け: 見出しは追加しない。事実のみを書き、所感・補足は書かない。 -->

# 要件定義

## 🌱 背景
<!-- AI向け: 作業が必要な理由を1〜2文で書く。例: 旧リポジトリの記事を本リポジトリへ移行するため -->
- Zennで管理するレポジトリをprivateからpublicに移行する際、非公開の記事の履歴を抹消するために新しいレポジトリに移行する
- articles配下の移行は完了しており（.steering\20260925-旧リポジトリの記事を移行する）、books配下が未移行のため

## 🌱 対象範囲
<!-- AI向け: 今回の作業で扱うものを「- 」で始まる1行ずつ書く。例: - articles/typescript-reduce.md -->
- books/book-record-of-reading
- books/github-copilot
- books/github-foundations
- books/github-foundations-part-2
- books/learn-react-tutorial
- books/learn-storybook-tutorial
- 対象bookが参照する画像ファイル

## 🌱 対象外
<!-- AI向け: 今回扱わないものを「- 」で始まる1行ずつ書く。ない場合は「なし」と書く。例: - 記事本文のリライト -->
- articles配下のmdファイル
- published: false のbook（books/poc-public-repository）
- 移行済みのbook、およびPRがオープン中のbook

## 🌱 要件
<!-- AI向け: 成果物が満たすべき条件を「- 」で始まる1行ずつ書く。1項目は1条件にする。例: - Front Matterの published は旧リポジトリの値を引き継ぐ -->
- C:\Users\aew2s\work\zenn-bk\books配下のbookディレクトリをC:\Users\aew2s\work\zenn\books配下にコピーする
- コピーは、1bookごとで行い専用ブランチを作成する
- コピー完了後、全てサブエージェントを活用してレビューする
- レビュー指摘を修正してPRを作成する
- bookのディレクトリ名（スラッグ）とチャプターのファイル名は旧リポジトリと同一にする
- config.yaml の published は旧リポジトリの値を引き継ぐ
- bookが参照する画像は、同じパスで images/ 配下に配置する

## 🌱 制約
<!-- AI向け: 守るべき前提・禁止事項を「- 」で始まる1行ずつ書く。ない場合は「なし」と書く。例: - 記事本文の内容は変更しない -->
- 秘密情報・個人情報を含む記述は移行しない
- 旧リポジトリのgit履歴は引き継がない

## 🌱 完了条件
<!-- AI向け: 完了とみなせる状態を「- [ ] 」で始まる1行ずつ書く。1項目は1条件にし、確認できる状態で書く。例: - [ ] 対象記事が articles/ 配下に存在し、npx zenn preview で表示できる -->
- [ ] .steering\20260926-旧リポジトリのbookを移行する\TODO.mdのチェックリストが完了すること
- [ ] 対象bookがすべて books/ 配下に存在する
- [ ] 対象bookが参照する画像がすべて images/ 配下に存在する
- [ ] npx zenn preview で全bookが画像を含めて表示できる
