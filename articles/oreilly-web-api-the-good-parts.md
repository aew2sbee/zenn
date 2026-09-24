---
title: "[API設計] 良いAPI(URL)とは" # 記事のタイトル
emoji: "🩹" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["初心者向け", "API", "Oreilly"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

下記の書籍（水野貴明『Web API: The Good Parts』オライリー・ジャパン、2014年）を読みました。
URL 設計を中心に、HTTP メソッド・ページネーション・キャッシュの要点を備忘録として書きます。

@[card](https://www.oreilly.co.jp/books/9784873116860/)

## 🌱 1. 短く入力しやすい URL

### 1-1. **必要最低限**で表現する

❌ `service`, `api`という言葉があり、一見丁寧な URL だけど、**文字数が多く**て入力しにくい

```text:Not good
https://api.example.com/service/api/search/
```

⭕ 必要最低限の単語で、**文字数が少なく**入力しやすい

```text:good
https://api.example.com/search/
```

## 🌱 2. 人間が読んで理解できる URL

### 2-1. **省略形**の単語を使わない

❌ `sv`, `u`という言葉があるが、**意味が伝わりにくい**

```text:Not good
https://api.example.com/sv/u/
```

⭕ 省略形を使わず、適切に伝える

```text:good
https://api.example.com/service/users/
```

---

### 2-2. 単語は**英語**を使う

❌ `producto`（スペイン語）は英語の`product`と紛らわしく、勘違いが起きやすい
❌ `seihin`（ローマ字）は論外

```text:Not good
https://api.example.com/producto/123/
https://api.example.com/seihin/123/
```

```text:good
https://api.example.com/product/123/
```

---

### 2-3. **適切な英語**を使う

❌ `find: あるもの（特定のもの）を探す`

```text:Not good
https://api.example.com/find/
```

⭕ `search: ある場所の中を検索する`（検索 API には search が適している）

```text:good
https://api.example.com/search/
```

## 🌱 3. 大文字小文字が混在していない URL

### 3-1. **小文字**を使う

❌ 大文字小文字が混在している

```text:Not good
https://api.example.com/User/123
```

⭕ 小文字のみにする

```text:good
https://api.example.com/users/123
```

## 🌱 4. 改造しやすい(Hackable)URL

:::message
改造しやすい(Hackable)とは、
**API のドキュメントを熟読**しなくても、**構造が想像しやすい**という意味
:::

### 4-1. **構造を予想できる**階層にする

⭕ `v1`が`version`を表し、`version 1.0`が予想できる
⭕ `items/123`は、`items`のうち`id`が`123`のものを取得していると予想できる
⭕ 別の`items`なら`items/234`などにしたら取得できそう

```text:good
https://api.example.com/v1/items/123
```

## 🌱 5. サーバー側のアーキテクチャが反映されていない URL

### 5-1. **言語**を隠す

❌ 言語が PHP である事が分かる

:::message
PHP の脆弱性を突いてくる悪い人がいる
:::

```text:Not good
https://api.example.com/cgi-bin/get-user.php?user=100
```

⭕ 言語などの情報を開示しない

```text:good
https://api.example.com/users/123
```

## 🌱 6. ルールが統一された URL

### 6-1. **ルール**を定める

❌ `id`の指定が URL 毎でバラバラ
❌ `friends`,`friend`で複数形と単数形が混在

```text:Not good
https://api.example.com/friends?id=100
https://api.example.com/friend/100/message
```

⭕ `friends`の複数形で統一
⭕ ID はクエリパラメータ（`?id=`）ではなく、パスで指定する形に統一

```text:good
https://api.example.com/friends/100
https://api.example.com/friends/100/messages
```

## 🌱 HTTP メソッド

| メソッド名 | 説明                     |
| ---------- | ------------------------ |
| GET        | リソースの取得           |
| POST       | リソースの新規登録       |
| PUT        | リソースの置き換え（更新） |
| DELETE     | リソースの削除           |
| PATCH      | リソースの一部変更       |
| HEAD       | リソースのメタ情報の取得 |

### PUT メソッドと PATCH メソッドの違い

- PUT メソッド: 指定した URI のリソースを、送った内容で置き換える（リソースがなければ作成することもある）
- PATCH メソッド: もともとあるリソースの一部を置き換える

:::message
リソースの一部だけを更新したい場合は、PATCH メソッドを使う（PUT は全体の置き換えなので全項目を送る必要がある。1MB のような大きなデータでは、PATCH の方が通信量を抑えられる）
:::

ここまでの URL のルールと HTTP メソッドを組み合わせた、エンドポイントの例です。

| 目的                     | メソッド  | エンドポイント                                       |
| ------------------------ | --------- | ---------------------------------------------------- |
| ユーザー一覧の取得       | GET       | https://api.example.com/v1/users/                    |
| ユーザーの新規登録       | POST      | https://api.example.com/v1/users/                    |
| 特定ユーザー情報の取得   | GET       | https://api.example.com/v1/users/:id                 |
| ユーザーの情報の更新     | PUT/PATCH | https://api.example.com/v1/users/:id                 |
| ユーザーの情報の削除     | DELETE    | https://api.example.com/v1/users/:id                 |
| ユーザーの友達一覧の取得 | GET       | https://api.example.com/v1/users/:id/friends         |
| 友達の追加               | POST      | https://api.example.com/v1/users/:id/friends         |
| 友達の削除               | DELETE    | https://api.example.com/v1/users/:id/friends/:friend_id |
| 近況の投稿               | POST      | https://api.example.com/v1/updates                   |
| 近況の編集               | PUT/PATCH | https://api.example.com/v1/updates/:id               |
| 近況の削除               | DELETE    | https://api.example.com/v1/updates/:id               |
| 特定ユーザー近況の取得   | GET       | https://api.example.com/v1/users/:id/updates         |
| 友達の近況一覧の取得     | GET       | https://api.example.com/v1/users/:id/friends/updates |

## 🌱 ページネーションについて

:::message
`limit`/`offset`のような**相対位置**ではなく、`id`やタイムスタンプを基準にした**絶対位置**（例: `?max_id=12345&limit=20`）でページネーションを行う
:::

❌ **相対位置**だと、データ数が増えるとパフォーマンスが下がる
❌ **相対位置**だと、データの更新によってデータの順番がズレると取得するデータもズレる

⭕ **絶対位置**で`id`を基準にすると、基準の列にインデックスがあれば、データ数が増えてもパフォーマンスが下がりにくい
⭕ **絶対位置**で`id`を基準にすると、途中でデータが追加・削除されても取得するデータがズレにくい

## 🌱 キャッシュについて

HTTP のキャッシュには、下記の2つの方式があります。

### Expiration Model (期限切れモデル)

> レスポンスデータに有効期限（`Cache-Control: max-age`、`Expires`）を設定する。期限内はサーバーにアクセスせずキャッシュを使い、期限が切れたら再度アクセスする

### Validation Model (検証モデル)

> 保持しているキャッシュが最新であるかを、条件付きリクエスト（`If-None-Match`、`If-Modified-Since`）でサーバーに確認する。更新がなければ`304 Not Modified`が返り、キャッシュを使う。更新があれば新しいデータを受け取る

## 🌱 おわりに

オライリーの本を初めて読みました。
自分には難しいと思っていましたが、そこまで難しいと感じなかったです。
オライリーの本は学びが多く、気に入りました!
