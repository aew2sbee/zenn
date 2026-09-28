---
title: "リポジトリを検索する"
---

## Q133: 名前に `docker` を含み、スター数が 100 を超えるパブリックリポジトリを見つけるには、どの高度な検索演算子の組み合わせを使いますか？

### 選択肢

- `in:name docker stars:>100 is:public`
- `docker in:description stars:<100 is:public`
- `is:public name:docker stars:100`
- `topic:docker stars:>100 in:readme`

### 回答欄

:::details 回答を見る
[公式ドキュメント](https://docs.github.com/ja/search-github/searching-on-github/searching-for-repositories)

- [x] `in:name docker stars:>100 is:public`（名前に `docker` を含み、スターが 100 個を超えるパブリックリポジトリを検索します。）
- [ ] `docker in:description stars:<100 is:public`
- [ ] `is:public name:docker stars:100`（これは stars の範囲演算子(`>`)が抜けており、`name:docker`は有効な構文ではありません。）
- [ ] `topic:docker stars:>100 in:readme`（これはリポジトリ名ではなく、トピックまたは readme 内を検索します。）

:::
