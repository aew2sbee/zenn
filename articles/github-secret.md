---
title: '[GitHub Actions] secrets情報の中身を出力する' # 記事のタイトル
emoji: '🐙‍' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['githubactions', 'github', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## 🌱 はじめに

この記事では、**secrets 情報をマスクされず、出力するを確認方法** を解説します。
下記書籍を参考にしながら調査をしました。

:::details 参考資料
@[card](https://gihyo.jp/book/2024/978-4-297-14173-8)
:::

## 🌱 結論

:::message

`run: echo "${SECRET_INFO:0:1} ${SECRET_INFO#?}"`で出力する

```diff:yaml
--- 略 ---
    env:
      SECRET_INFO: ${{ secrets.SECRET_INFO }}
    steps:
+      # マスクされずに出力する
+      - run: echo "${SECRET_INFO:0:1} ${SECRET_INFO#?}"

--- 略 ---
```

:::

## 🌱 注意事項

:::message alert

1. 不要になったら、コードなどを削除して実運用のコードに含めないようにお願い致します
2. GitHub Actions から GCP に認証するために必要なサービスアカウント鍵はマスクされたままでした。(体験談)

:::

## 🌱 やり方

### 0. SECRET_INFO に適当な値を設定する

今回は、検証のため、`ABCDEFG`を設定します

```bash
ABCDEFG
```

### 1. GitHub Action の yaml ファイルに`echo`を追加

```diff:yaml
--- 略 ---
    env:
      SECRET_INFO: ${{ secrets.SECRET_INFO }}
    steps:
+    # マスクされずに出力する
+      - run: echo "${SECRET_INFO:0:1} ${SECRET_INFO#?}"

--- 略 ---
```

## 2. GitHub Action を実行

:::message
**なぜ 表示されるのか？**

ログマスクのアルゴリズムは完全一致になります。
1 文字変更すれば、完全一致ではなくなり表示される

:::

```bash
▶ Rub echo "${SECRET_INFO:0:1} ${SECRET_INFO#?}"
A BCDEFG
```
