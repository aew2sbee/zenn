---
title: "Issue およびプルリクエストを検索する"
---

## Q125: 本文中に「fix」と記述されている、「test」とラベル付けされたすべてのオープンな issue を検索するクエリはどれですか？

### 選択肢

- `is:pr is:open label:test "fix"`
- `is:issue in:comments label:test "fix"`
- `is:issue is:open label:test "fix"`
- `type:issue label:test is:open body:"fix"`

### 回答欄

:::details 回答を見る
[公式ドキュメント](https://docs.github.com/ja/search-github/searching-on-github/searching-issues-and-pull-requests)

- [ ] `is:pr is:open label:test "fix"`
- [ ] `is:issue in:comments label:test "fix"`
- [x] `is:issue is:open label:test "fix"`（`in:` 修飾子を省略すると、タイトル・本文・コメントが検索対象になります。）
- [ ] `type:issue label:test is:open body:"fix"`

:::
