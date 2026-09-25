# zenn

- [Zenn](https://zenn.dev/) に投稿するブログ記事を管理するリポジトリです。
- [Zenn CLI](https://zenn.dev/zenn/articles/zenn-cli-guide) を使ってローカルで記事を書き、GitHub 連携で Zenn に公開します。

## 技術スタック

- Node.js v22.16.0
- npm
- zenn-cli

## セットアップ

```bash
npm install
```

## 使い方

### 記事を作成する

```bash
npx zenn new:article --slug <slug>
```

`articles/<slug>.md` が作成されます。スラッグは `python-pandas-mean` のように、`技術名-内容` の形式で付けています。

### プレビューする

```bash
npx zenn preview
```

ブラウザで `http://localhost:8000` を開くと、Zenn と同じ見た目で記事を確認できます。


## 参考

- [Zenn CLI で記事・本を管理する方法](https://zenn.dev/zenn/articles/zenn-cli-guide)
- [Zenn のMarkdown記法一覧](https://zenn.dev/zenn/articles/markdown-guide)
- [GitHubリポジトリ連携で Zenn のコンテンツを管理する](https://zenn.dev/zenn/articles/connect-to-github)
