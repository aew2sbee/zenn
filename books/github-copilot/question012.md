---
title: "GitHub Copilot からのコンテンツの除外"
---

## Q12: GitHub Copilot は、GitHub Copilot のコンテンツ除外によって無視されるファイルのセマンティック情報を使用できますか？

### 選択肢

- はい、IDE から間接的に情報が提供される場合。
- いいえ、除外されたファイルからの情報はすべて無視されます。

### 回答欄

:::details 回答を見る
[公式ドキュメント](https://docs.github.com/ja/copilot/managing-copilot/configuring-and-auditing-content-exclusion/excluding-content-from-github-copilot#limitations-of-content-exclusions)

- [x] はい、IDE から間接的に情報が提供される場合。
- [ ] いいえ、除外されたファイルからの情報はすべて無視されます。(情報が IDE から間接的に提供されている場合、Copilot は除外されたファイルのセマンティック情報を使用する可能性があります。このようなコンテンツの例としては、コードで使用されるシンボルの型情報やホバーオーバーの定義、ビルド構成情報などの一般的なプロジェクトのプロパティがあります。)

:::
