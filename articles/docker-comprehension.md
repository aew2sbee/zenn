---
title: "【Docker】少しだけ分かった気がする「イラスト付き」" # 記事のタイトル
emoji: "🍧" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["Docker", "Dockerfile"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
いつかDockerを覚えたいと思い、書籍や動画を見て学習しておりましたが、
アウトプットできるぐらいに少しだけ理解できたので執筆します。
内容は、当時自分が分からなかった内容を中心に書きます。

好きな食べ物は**あんこ**です。

:::message alert
Dockerに関する説明等は行いません。
あくまでも**当時自分が分からなかった内容**になります。
:::

|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・Docker初学者  |
|  **伝えたい内容**  |  ・当時自分が分からなかった内容  |

## Dockerイメージとは？
![sweets_anko_pack](/images/sweets_anko_pack.png =250x)

**解答：Dockerコンテナを作成するためのスタンドアロンのパッケージです。**

つまり、あんこのことです。
あんこがあれば、色んなあんこのスイーツが食べれます。

- **Q: スタンドアロンとは？**
    - A: 独立して動作すること

## Dockerコンテナとは？
![sweets_taiyaki1_tsubuan](/images/sweets_taiyaki1_tsubuan.png =250x)

**解答：Dockerイメージを実行するためのランタイム環境のこと**

つまり、たい焼きのことです。
あんこ(`Dockerイメージ`)をもとに、たい焼きを作ることが出来ます。

- **Q: ランタイム環境とは？**
    - A: ソフトウェアの実行に必要なランタイムコンポーネントやリソースが提供される実行環境のこと

## Dockerfileってなんで必要なの？
![docker01](/images/docker01.png)

**解答：DockerイメージからDockerコンテナをビルドするために必要です。**

つまり、たい焼きのレシピにあたります。
あんこ(`Dockerイメージ`)を使い、レシピ(`Dockerfile`)に書かれている調理法(ライブラリーのインストールなど)を行い、たい焼き(`Dockerコンテナ`)を作成する

下記のイメージのように様々なレシピ(`Dockerfile`)があれば、**大好きなあんこのスイーツ**を作る事が出来ます。
![docker02](/images/docker02.png)

:::message
1つのDockerイメージからDockerfileによってオリジナルのDockerコンテナを作成できる
:::

### PythonのDockerイメージの場合
![docker03](/images/docker03.png)

PythonのDockerイメージから...
- `TensorFlow`に関するDockerfile -> AIを動かす環境が出来る
- `django`に関するDockerfile -> YoutubeのようなWebアプリを動かす環境が出来る
- `Pandas`に関するDockerfile -> データ分析を動かす環境が出来る

## docker-compose.ymlってなんで必要なの？
![docker04](/images/docker04.png =500x)

**解答：Docker Composeを使用して複数のコンテナを定義し、それらを組み合わせてアプリケーションの実行環境を構築するために必要です。**

つまり、**大好きなあんこのスイーツ**の詰め合わせセットを作ることが出来ます。
### Django/PostgreSQLのDockerコンテナの場合
![docker05](/images/docker05.png)

上記のイメージのように複数のDockerコンテナ(`django/PostgreSQL`)を組み合わせたWebアプリを動かす環境を作成する事が出来ます。

## おわりに
最近暑くなりましたね。🍧が食べたくなります。