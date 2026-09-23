---
title: "[Docker] イメージ・コンテナ・Dockerfile・Composeの違いが少しだけ分かった気がする「イラスト付き」" # 記事のタイトル
emoji: "🐳" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["Docker", "Dockerfile", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

いつか Docker を覚えたいと思い、書籍や動画で学習していましたが、アウトプットできるぐらいに少しだけ理解できたので執筆します。
当時の自分が分からなかった「Docker イメージ」「Docker コンテナ」「Dockerfile」「docker-compose.yml」の違いを中心に書きます。

好きな食べ物は**あんこ**なので、あんこのスイーツに例えて説明します。

Docker は、アプリとその実行環境をまとめて、どの PC でも同じように動かせるようにする仕組みです。

:::message alert
Docker の網羅的な解説やコマンドの使い方は扱いません。
:::

|項目|内容|
|---|---|
|**対象者**|・Docker を触り始めたが、用語の関係があいまいな方|
|**伝えたい内容**|・Docker イメージ／コンテナ／Dockerfile／docker-compose.yml の役割と関係|

## 🌱 Docker イメージとは？

![あんこのパックのイラスト](/images/articles/docker-comprehension/sweets_anko_pack.png =250x)

**回答：Docker コンテナを作成するための、アプリの実行に必要なもの一式（OS の最小構成、言語、ライブラリなど）をまとめたパッケージです。**

つまり、あんこのことです。
あんこがあれば、色んなあんこのスイーツが食べられます。

- **Q: スタンドアロンとは？**
  - A: ほかに頼らず独立して動作すること。Docker イメージは、ホスト PC に何がインストールされているかに関係なく動きます。

## 🌱 Docker コンテナとは？

![たい焼きのイラスト](/images/articles/docker-comprehension/sweets_taiyaki1_tsubuan.png =250x)

**回答：Docker イメージを実際に起動して、プログラムが動いている状態（実行環境）のことです。**

つまり、たい焼きのことです。
あんこ（`Docker イメージ`）をもとに、たい焼きを作れます。
1 つのあんこから何個でもたい焼きを作れるように、1 つのイメージから複数のコンテナを起動できます。

## 🌱 Dockerfile ってなんで必要なの？

![Dockerイメージ・Dockerfile・Dockerコンテナの関係を表した図](/images/articles/docker-comprehension/docker01.png)

**回答：ベースとなる Docker イメージをもとに、独自の Docker イメージをビルド（作成）するために必要です。**

つまり、たい焼きのレシピにあたります。
あんこ（ベースの `Docker イメージ`）を使い、レシピ（`Dockerfile`）に書かれている調理法（ライブラリのインストールなど）を行って、自分用のイメージを作ります。
そのイメージを起動すると、たい焼き（`Docker コンテナ`）になります。

:::message
正確な流れは次のとおりです（上の図では、途中のイメージのビルドを省略しています）。

ベースイメージ ＋ Dockerfile →（`docker build`）→ 独自のイメージ →（`docker run`）→ コンテナ
:::

下記の図のように様々なレシピ（`Dockerfile`）があれば、**大好きなあんこのスイーツ**を作れます。

![1つのあんこから複数のレシピで色々なスイーツを作る様子を表した図](/images/articles/docker-comprehension/docker02.png)

### Python の Docker イメージの場合

![PythonのDockerイメージから3種類のDockerfileで異なる環境を作る様子を表した図](/images/articles/docker-comprehension/docker03.png)

Python の Docker イメージをベースに、Dockerfile で Python 用のライブラリを追加すると...

- `TensorFlow` を入れる Dockerfile → AI を動かす環境ができる
- `Django` を入れる Dockerfile → ブログや管理画面のような Web アプリを動かす環境ができる
- `Pandas` を入れる Dockerfile → データ分析を行う環境ができる

例えば、Pandas を入れる Dockerfile は下記のようになります。

```dockerfile:Dockerfile
FROM python:3.12
RUN pip install pandas
```

## 🌱 docker-compose.yml ってなんで必要なの？

![あんこスイーツの詰め合わせセットのイラスト](/images/articles/docker-comprehension/docker04.png =500x)

**回答：複数のコンテナの設定を 1 つのファイルにまとめ、まとめて起動するために必要です。**

docker-compose.yml は、Docker Compose（複数のコンテナをまとめて起動・管理するツール）で使う YAML 形式の設定ファイルです。
現在は `compose.yaml` というファイル名が推奨されていますが、`docker-compose.yml` も引き続き使えます。
各サービスには、既存のイメージか、Dockerfile からビルドしたイメージを指定します。
つまり、**大好きなあんこのスイーツ**の詰め合わせセットにあたります。

### Django/PostgreSQL の Docker コンテナの場合

![DjangoとPostgreSQLのコンテナを組み合わせてWebアプリを動かす様子を表した図](/images/articles/docker-comprehension/docker05.png)

上記の図のように、複数の Docker コンテナ（Django と、データを保存するデータベースの PostgreSQL）を組み合わせて、Web アプリを動かす環境を作れます。
図では 1 つの書類にまとめて描いていますが、実際は Django と PostgreSQL は別々のコンテナです。
1 つのコンテナに 1 つの役割を持たせると、入れ替えや管理がしやすくなります。

## 🌱 まとめ

| 用語 | 例え | 役割 |
| --- | --- | --- |
| Docker イメージ | あんこ | コンテナを作るためのパッケージ |
| Dockerfile | レシピ | ベースイメージから独自のイメージを作る手順書 |
| Docker コンテナ | たい焼き | イメージを起動して動いている実行環境 |
| docker-compose.yml | 詰め合わせセット | 複数のコンテナをまとめて起動するための設定ファイル |
