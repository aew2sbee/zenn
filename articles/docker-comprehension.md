---
title: "[Docker] 少しだけ分かった気がする「イラスト付き」" # 記事のタイトル
emoji: "🐳" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["Docker", "Dockerfile", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

いつか Docker を覚えたいと思い、書籍や動画を見て学習しておりましたが、
アウトプットできるぐらいに少しだけ理解できたので執筆します。
内容は、当時自分が分からなかった内容を中心に書きます。

好きな食べ物は**あんこ**です。

:::message alert
Docker に関する説明等は行いません。
あくまでも**当時自分が分からなかった内容**になります。
:::

| 項目             | 内容                           |
| ---------------- | ------------------------------ |
| **対象者**       | ・Docker 初学者                |
| **伝えたい内容** | ・当時自分が分からなかった内容 |

## Docker イメージとは？

![sweets_anko_pack](/images/sweets_anko_pack.png =250x)

**解答：Docker コンテナを作成するためのスタンドアロンのパッケージです。**

つまり、あんこのことです。
あんこがあれば、色んなあんこのスイーツが食べれます。

- **Q: スタンドアロンとは？**
  - A: 独立して動作すること

## Docker コンテナとは？

![sweets_taiyaki1_tsubuan](/images/sweets_taiyaki1_tsubuan.png =250x)

**解答：Docker イメージを実行するためのランタイム環境のこと**

つまり、たい焼きのことです。
あんこ(`Dockerイメージ`)をもとに、たい焼きを作ることが出来ます。

- **Q: ランタイム環境とは？**
  - A: ソフトウェアの実行に必要なランタイムコンポーネントやリソースが提供される実行環境のこと

## Dockerfile ってなんで必要なの？

![docker01](/images/articles/docker-comprehension/docker01.png)

**解答：Docker イメージから Docker コンテナをビルドするために必要です。**

つまり、たい焼きのレシピにあたります。
あんこ(`Dockerイメージ`)を使い、レシピ(`Dockerfile`)に書かれている調理法(ライブラリーのインストールなど)を行い、たい焼き(`Dockerコンテナ`)を作成する

下記のイメージのように様々なレシピ(`Dockerfile`)があれば、**大好きなあんこのスイーツ**を作る事が出来ます。
![docker02](/images/articles/docker-comprehension/docker02.png)

:::message
1 つの Docker イメージから Dockerfile によってオリジナルの Docker コンテナを作成できる
:::

### Python の Docker イメージの場合

![docker03](/images/articles/docker-comprehension/docker03.png)

Python の Docker イメージから...

- `TensorFlow`に関する Dockerfile -> AI を動かす環境が出来る
- `django`に関する Dockerfile -> Youtube のような Web アプリを動かす環境が出来る
- `Pandas`に関する Dockerfile -> データ分析を動かす環境が出来る

## docker-compose.yml ってなんで必要なの？

![docker04](/images/articles/docker-comprehension/docker04.png =500x)

**解答：Docker Compose を使用して複数のコンテナを定義し、それらを組み合わせてアプリケーションの実行環境を構築するために必要です。**

つまり、**大好きなあんこのスイーツ**の詰め合わせセットを作ることが出来ます。

### Django/PostgreSQL の Docker コンテナの場合

![docker05](/images/articles/docker-comprehension/docker05.png)

上記のイメージのように複数の Docker コンテナ(`django/PostgreSQL`)を組み合わせた Web アプリを動かす環境を作成する事が出来ます。
