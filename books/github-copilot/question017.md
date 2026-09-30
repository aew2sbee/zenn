---
title: "GitHub Copilot のデータ処理（提案の保持）"
---

## Q17: GitHub Copilot は、IDE でのコード提案に使ったデータをどのように保持しますか？

### 選択肢

- すべての提案を、後で参照できるようにローカルデータベースに永久保存する
- 一時的にメモリに保持し、提案を返した後に破棄する
- 提案内容を、バージョン管理のために GitHub リポジトリに自動的に保存する
- コードスニペットを 30 日間ディスクにキャッシュしてから削除する

### 回答欄

:::details 回答を見る
[公式ドキュメント](https://learn.microsoft.com/ja-jp/training/modules/introduction-prompt-engineering-with-github-copilot/4-github-copilot-data)

- [ ] すべての提案を、後で参照できるようにローカルデータベースに永久保存する
- [x] 一時的にメモリに保持し、提案を返した後に破棄する
- [ ] 提案内容を、バージョン管理のために GitHub リポジトリに自動的に保存する
- [ ] コードスニペットを 30 日間ディスクにキャッシュしてから削除する

:::
