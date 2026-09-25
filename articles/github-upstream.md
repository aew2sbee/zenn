---
title: "[GitHub] fork元のリポジトリをupstreamに設定する" # 記事のタイトル
emoji: "🐙" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["git", "github", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**fork元のリポジトリを upstream に設定する方法**を解説します。

fork したリポジトリには、fork元の更新が自動では反映されません。
fork元の変更を取り込むために、fork元のリポジトリを `upstream` という名前でリモートに登録します。

- `origin`: 自分の fork（clone したリポジトリ）
- `upstream`: fork元のリポジトリ

`upstream` は慣習的に使われている名前で、Git の予約語ではありません。

## 🌱 結論

:::message
下記のコマンドを実行すると設定できます。

```bash
git remote add upstream https://github.com/<fork元のオーナー名>/<fork元のリポジトリ名>.git
```

`git remote add` はリモートを登録するだけの操作です。fork元の変更を取り込むには、後述の「3. fork元の変更を取り込む」を実行します。
:::

## 🌱 1. 状況を確認する

下記のコマンドで状況を確認します。

```bash
git remote -v
```

:::details 出力結果を確認する

```text
$ git remote -v
origin	https://github.com/user-name/sample-project.git (fetch)
origin	https://github.com/user-name/sample-project.git (push)
```

:::

## 🌱 2. upstream に設定する

下記のコマンドで、fork元のリポジトリを upstream として登録します。

```bash
git remote add upstream https://github.com/company-name/sample-project.git
```

もう一度、下記のコマンドで状況を確認します。

```bash
git remote -v
```

:::details 出力結果を確認する

```text
$ git remote -v
origin	https://github.com/user-name/sample-project.git (fetch)
origin	https://github.com/user-name/sample-project.git (push)
upstream	https://github.com/company-name/sample-project.git (fetch)
upstream	https://github.com/company-name/sample-project.git (push)
```

:::

:::message
すでに upstream を登録している状態で実行すると、`error: remote upstream already exists.` と表示されます。
URL を変更したい場合は、`git remote set-url upstream <URL>` を実行します。
:::

## 🌱 3. fork元の変更を取り込む

upstream を登録したら、下記のコマンドで fork元の変更を取り込みます。
デフォルトブランチ名が `main` 以外（`master` など）の場合は、読み替えてください。

```bash
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

:::message
GitHub の Web 画面の「Sync fork」ボタンや、GitHub CLI の `gh repo sync` でも同期できます。
詳しくは公式ドキュメントの [Syncing a fork](https://docs.github.com/ja/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork) を参照してください。
:::
