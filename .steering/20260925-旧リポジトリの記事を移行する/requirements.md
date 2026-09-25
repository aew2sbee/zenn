<!-- AI向け: 見出しは追加しない。事実のみを書き、所感・補足は書かない。 -->

# 要件定義

## 🌱 背景
<!-- AI向け: 作業が必要な理由を1〜2文で書く。例: 旧リポジトリの記事を本リポジトリへ移行するため -->
- Zennで管理するレポジトリをprivateからpublicに移行する非公開の記事の履歴を抹消するために新しいレポジトリに移行する

## 🌱 対象範囲
<!-- AI向け: 今回の作業で扱うものを「- 」で始まる1行ずつ書く。例: - articles/typescript-reduce.md -->
- articles/aws-ec2-iam-role.md
- articles/django-install.md
- articles/django-rest-framework-install.md
- articles/django-rest-framework-models.md
- articles/django-rest-framework-postgres.md
- articles/typescript-object-orientation.md
- articles/typescript-object-to-list.md
- articles/typescript-option-parameter.md
- articles/typescript-or-and.md
- articles/typescript-reduce.md
- articles/typescript-rest-parameter.md
- articles/typescript-return-type-never.md
- articles/typescript-return-type-void.md
- articles/typescript-some.md
- articles/yarn-error-no-such-option.md
- 対象記事が参照する画像ファイル

## 🌱 対象外
<!-- AI向け: 今回扱わないものを「- 」で始まる1行ずつ書く。ない場合は「なし」と書く。例: - 記事本文のリライト -->
- books配下のmdファイル
- published: false の記事
- 移行済みの記事、およびPRがオープン中の記事

## 🌱 要件
<!-- AI向け: 成果物が満たすべき条件を「- 」で始まる1行ずつ書く。1項目は1条件にする。例: - Front Matterの published は旧リポジトリの値を引き継ぐ -->
- C:\Users\aew2s\work\zenn\articles配下のmdファイルをC:\Users\aew2s\work\dev-zenn\articles配下にコピーする
- コピーは、1ファイルごとで行い専用ブランチを作成する
- コピー完了後、全てサブエージェントを活用してレビューする
- レビュー指摘を修正してPRを作成する
- 記事のファイル名（スラッグ）は旧リポジトリと同一にする
- Front Matterの published は旧リポジトリの値を引き継ぐ
- 記事が参照する画像は、同じパスで images/ 配下に配置する

## 🌱 制約
<!-- AI向け: 守るべき前提・禁止事項を「- 」で始まる1行ずつ書く。ない場合は「なし」と書く。例: - 記事本文の内容は変更しない -->
- 秘密情報・個人情報を含む記述は移行しない
- 旧リポジトリのgit履歴は引き継がない

## 🌱 完了条件
<!-- AI向け: 完了とみなせる状態を「- [ ] 」で始まる1行ずつ書く。1項目は1条件にし、確認できる状態で書く。例: - [ ] 対象記事が articles/ 配下に存在し、npx zenn preview で表示できる -->
- [ ] .steering\20260925-旧リポジトリの記事を移行する\TODO.mdのチェックリストが完了すること
- [ ] 対象記事がすべて articles/ 配下に存在する
- [ ] 対象記事が参照する画像がすべて images/ 配下に存在する
- [ ] npx zenn preview で全記事が画像を含めて表示できる