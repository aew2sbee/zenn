---
title: '[API設計] 良いAPI(URL)とは' # 記事のタイトル
emoji: '🩹' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['初心者向け', 'API', 'Oreilly'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

下記の書籍を読みました。
備忘録を書きます。
@[card](https://www.oreilly.co.jp/books/9784873116860/)

## 1. 短く入力しやすい URL

### 1-1. **必要最低限**で表現する

❌ `service`, `api`という言葉があり、一見丁寧な URL だけど、**文字数が多く**て入力しにくい

```bash: Not good
https://api.example.com/service/api/search/
```

⭕ 必要最低限の単語で、**文字数が少なく**入力しやすい

```bash: good
https://api.example.com/search/
```

## 2. 人間が読んで理解できる URL

### 2-1. **省略系**の単語を使わない

❌ `sv`, `u`という言葉があるが、**意味が伝わりにくい**

```bash: Not good
https://api.example.com/sv/u/
```

⭕ 省略系を使わず、適切に伝える

```bash: good
https://api.example.com/service/users/
```

---

### 2-2. 単語は**英語**を使う

❌ `英語: product`, `スペイン語: producto`のように勘違いが発生しにくいように英語にする
❌ `ローマ字: seihin`は論外

```bash: Not good
<!-- スペイン語 -->
https://api.example.com/producto/123/
<!-- 日本語 -->
https://api.example.com/seihin/123/
```

```bash: good
<!-- 英語 -->
https://api.example.com/product/123/
```

---

### 2-3. **適切な英語**を使う

❌ `find: あるものを探す`

```bash: Not good
https://api.example.com/find/
```

⭕ `search: ある場所において探す`

```bash: good
https://api.example.com/search/
```

## 3. 大文字小文字が混在していない URL

### 3-1. **小文字**を使う

❌ 大文字小文字が混在している

```bash: Not good
https://api.example.com/User/123
```

⭕ 小文字のみにする

```bash: good
https://api.example.com/users/123
```

## 4. 改造しやすい(Hackable)URL

:::message
改造しやすい(Hackable)とは、
**API のドキュメントを熟読**しなくても、**構造が想像しやすい**という意味
:::

### 4-1. **小文字**を使う

⭕ `v1`が`version`を表し、`version 1.0`が予想できる
⭕ `itmes/123`は、`itmes`の中の`id`が`123`を取得している事が予想できる
⭕ 別の`itmes`なら`itmes/234`などにしたら取得出来そう

```bash: good
https://api.example.com/v1/itmes/123
```

## 5. サーバー側のアーキテクチャが反映されていない URL

### 5-1. **言語**を隠す

❌ 言語が PHP である事が分かる
:::message
PHP の脆弱性を突いてくる悪い人がいる
:::

```bash: Not good
https://api.example.com/cgi-bin/get-user.php?user=100
```

⭕ 言語などの情報を開示しない

```bash: good
https://api.example.com/users/123
```

## 6. ルールが統一された URL

### 6-1. **ルール**を定める

❌ `id`の指定が URL 毎でバラバラ
❌ `friends`,`friend`で複数形と単数形が混在

```bash: Not good
https://api.example.com/friends?id=100
https://api.example.com/friend/100/message
```

⭕ `friends`の複数形で統一
⭕ `id`を使わないで統一

```bash: good
https://api.example.com/friends/100
https://api.example.com/friends/100/messages
```

## HTTP メソッド

| メソッド名 | 説明                     |
| ---------- | ------------------------ |
| GET        | リソースの取得           |
| POST       | リソースの新規登録       |
| PUT        | 既存リソースの更新       |
| DELETE     | リソースの削除           |
| PATCH      | リソースの一部変更       |
| HEAD       | リソースのメタ情報の取得 |

### PUT メソッドと PATCH メソッドの違い

- PUT メソッド: もともとあるリソースを置き換える
- PATCH メソッド: もともとあるリソースの一部を置き換える

:::message
1MB のような大きなデータの一部のデータを更新したい場合は、PATCH メソッドを使う
:::

| 目的                     | メソッド  | エンドポイント                                       |
| ------------------------ | --------- | ---------------------------------------------------- |
| ユーザー一覧の取得       | GET       | https://api.example.com/v1/users/                    |
| ユーザーの新規登録       | POST      | https://api.example.com/v1/users/                    |
| 特定ユーザー情報の取得   | GET       | https://api.example.com/v1/users/:id                 |
| ユーザーの情報の更新     | PUT/PATCH | https://api.example.com/v1/users/:id                 |
| ユーザーの情報の削除     | DELETE    | https://api.example.com/v1/users/:id                 |
| ユーザーの友達一覧の取得 | GET       | https://api.example.com/v1/users/:id/friends         |
| 友達の追加               | POST      | https://api.example.com/v1/users/:id/friends         |
| 友達の削除               | DELETE    | https://api.example.com/v1/users/:id/friends/:id     |
| 近況の投稿               | POST      | https://api.example.com/v1/updates                   |
| 近況の編集               | PUT       | https://api.example.com/v1/updates/:id               |
| 近況の削除               | DELETE    | https://api.example.com/v1/updates/:id               |
| 特定ユーザー近況の取得   | GET       | https://api.example.com/v1/users/:id/updates         |
| 友達の近況一覧の取得     | GET       | https://api.example.com/v1/users/:id/friends/updates |

## ページネーションについて

:::message
**相対位置**ではなく、**絶対位置**を使って`limit`,`offset`をページネーションを行う
:::

❌ **相対位置**だと、データ数が増えるとパフォーマンスが下がる
❌ **相対位置**だと、データの更新によってデータの順番がズレると取得するデータもズレる

⭕ **絶対位置**で`id`で検索だと、データ数が増えてもパフォーマンスは下がらない
⭕ **絶対位置**で`id`で検索だと、データ数が増えても取得するデータはズレない

## Expiration Model (期限切れモデル)

> レスポンスデータに期間を決める。期限が消えたら、再度アクセスをする

## Validation Model (検証モデル)

> 現在保持しているキャッシュが最新であるかを確認する。データが更新された場合にのみ取得する

## おわりに

Oreilly を初めて読みました。
自分には難しいと思っていましたが、そこまで難しいと感じなかったです。
Oreilly は学びが多く気に入りました!

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！

@[card](https://www.youtube.com/@aew2sbee)
