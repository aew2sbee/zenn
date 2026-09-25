<!-- AI向け: 見出しは追加しない。事実のみを書き、所感・補足は書かない。 -->

# 要件定義

## 🌱 背景
<!-- AI向け: 作業が必要な理由を1〜2文で書く。例: 旧リポジトリの記事を本リポジトリへ移行するため -->
- Zennで管理するレポジトリをprivateからpublicに移行する非公開の記事の履歴を抹消するために新しいレポジトリに移行する

## 🌱 対象範囲
<!-- AI向け: 今回の作業で扱うものを「- 」で始まる1行ずつ書く。例: - articles/typescript-reduce.md -->
- articles/aws-ec2-iam-why-role.md
- articles/aws-ec2-vscode.md
- articles/aws-ec2-web-server.md
- articles/aws-vault-install-to-powershell.md
- articles/book-increase-productivity.md
- articles/book-leanstartup.md
- articles/book-secure-web-app-password.md
- articles/claude-code-ai-testing.md
- articles/claude-code-api-setting.md
- articles/claude-code-context-window-usage.md
- articles/docker-comprehension.md
- articles/docker-desktop-install.md
- articles/docker-react-env.md
- articles/document-write.md
- articles/domain-driven-design-tutorial.md
- articles/first-time-leader.md
- articles/gcloud-bash-sh-file.md
- articles/general-statuscode.md
- articles/github-branch-protection-ruleset.md
- articles/github-ci-cd.md
- articles/github-copilot-commit-message-japanese.md
- articles/github-reviewers-me.md
- articles/github-secret.md
- articles/github-upstream.md
- articles/good-and-bad-bug-issues.md
- articles/how-to-mermaid.md
- articles/how-to-review.md
- articles/http-accept-lang.md
- articles/http-timestamp.md
- articles/ipa-information-security-management-examination.md
- articles/ipa-information-technology-engineer-examination.md
- articles/javascript-checkbox.md
- articles/javascript-timelag-animation.md
- articles/job-change-age-30s.md
- articles/job-change-resume.md
- articles/lessons-from-my-mistakes.md
- articles/linux-command-line-interface.md
- articles/linux-ip-addr-show.md
- articles/nextjs-fetch-no-store.md
- articles/nextjs-link-userouter.md
- articles/nextjs-page-tsx.md
- articles/nextjs-prettier.md
- articles/nextjs-project-dev.md
- articles/nextjs-root-layout-file.md
- articles/nextjs-server-componet-client-component.md
- articles/nextjs-tsconfig-json.md
- articles/oreilly-web-api-the-good-parts.md
- articles/original-issue-template.md
- articles/playwright-accessibilitytree.md
- articles/playwright-advanced-locator-filter.md
- articles/playwright-advanced-locator-nth.md
- articles/playwright-ci-cd-gcp.md
- articles/playwright-implementation.md
- articles/playwright-locator.md
- articles/playwright-skip.md
- articles/playwright-storage-state.md
- articles/prisma-seed-error.md
- articles/python-3-10-11-install.md
- articles/python-csv-2pattern.md
- articles/python-error-selenium.md
- articles/python-list-comprehensions.md
- articles/python-matplotlib-45graph.md
- articles/python-matplotlib-bar.md
- articles/python-matplotlib-hist.md
- articles/python-matplotlib-pie.md
- articles/python-matplotlib-plot.md
- articles/python-pandas-mean.md
- articles/python-pandas-median.md
- articles/python-pandas-value_counts.md
- articles/python-pymodbustcp.md
- articles/python-zipcode.md
- articles/react-install.md
- articles/react-meta-tag.md
- articles/react-usestate.md
- articles/security-access-control-allow-headers.md
- articles/security-cross-sitescripting.md
- articles/security-cross-sitescripting-link.md
- articles/sql-cheat-sheet.md
- articles/tailwind-css-button-ui.md
- articles/tailwind-css-cheat-sheet.md
- articles/tailwind-css-line-clamp.md
- articles/tailwind-css-load-ui.md
- articles/tailwind-css-login-ui.md
- articles/tech-blog-for-job-change.md
- articles/terraform-aws-vault-ec2.md
- articles/test-black-white-box.md
- articles/test-design-document-template.md
- articles/test-high-performance-at-minimal-cost.md
- articles/test-how-to-become-a-good-test-engineer.md
- articles/test-how-to-use-pict.md
- articles/testing-notes.md
- articles/test-perspectives.md
- articles/test-pict-install.md
- articles/test-procedure-document.md
- articles/thinking-test-pattern.md
- articles/typescript-arrow.md
- articles/typescript-bigint-type.md
- articles/typescript-call-signature.md
- articles/typescript-coding-rule-object.md
- articles/typescript-doding-guidelines-microsoft.md
- articles/typescript-enum.md
- articles/typescript-error-handling.md
- articles/typescript-every.md
- articles/typescript-filter.md
- articles/typescript-find.md
- articles/typescript-generics-func.md
- articles/typescript-import-json.md
- articles/typescript-includes.md
- articles/typescript-interface.md
- articles/typescript-interface-readonly.md
- articles/typescript-interface-vs-type.md
- articles/typescript-jest-coverage.md
- articles/typescript-jest-mock.md
- articles/typescript-json-file-import.md
- articles/typescript-list-end.md
- articles/typescript-list-multi-type.md
- articles/typescript-map.md
- articles/book-principlesofprogramming.md
- articles/git-basiccommand.md
- articles/typescript-ver-let-const.md
- articles/python-matplotlib-vscode.md
- articles/python-pprint.md
- articles/ai-github-copilot.md
- articles/ai-qa-what-to-test.md
- articles/atomic-design-for-digital.md
- articles/aws-ec2-iam-create-user.md
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
- [x] .steering\20260925-旧リポジトリの記事を移行する\TODO.mdのチェックリストが完了すること
- [x] 対象記事がすべて articles/ 配下に存在する
- [x] 対象記事が参照する画像がすべて images/ 配下に存在する
- [ ] npx zenn preview で全記事が画像を含めて表示できる