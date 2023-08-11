---
title: "【Docker】Reactの環境構築" # 記事のタイトル
emoji: "🎡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["docker", "react", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
以前、DockerコマンドでReactを立ち上げる方法の勉強をしたので、執筆します。
下記の画面が表示されるまでを解説したいと思います。
![React_step6](/images/React_step6.png)


|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・Docker初学者  |
|  **伝えたい内容**  |  ・DockerコマンドでReactを立ち上げる方法  |
|  **前提条件**  |  ・node:14.17.0 |

## Docker DeskTopのインストール
下記記事を参考にしてください。
@[card](https://zenn.dev/aew2sbee/articles/docker-desktop-install)

## コンテナを作成する
### 1. docker runコマンド
Reactを動かせる環境(Nodejs)を作成する為に下記コマンドを実行します。
`Nodejsのバージョンを14.17.0でReact-envという名前のコンテナを作成するという意味になります。`
````bash
docker run -td --name="React-env" node:14.17.0 /bin/bash
````
### 2. 作成したコンテナを確認する
Docker Desktopを起動し、さきほどのコンテナが作成出来ている事を確認します。

![docker_desktop_step7](/images/docker_desktop_step7.png)

## Reactアプリを起動する
### 1. コンテナを起動する
Webアプリを作成する準備が整ったので作成していきます。
まずは、さきほど作成したコンテナを起動します。

1. Visual Studio Codeの左側の`Dcoker`のアイコンをクリックします。
2. さきほど作成したコンテナを右クリックで`Start`を押し、コンテナを起動します。

![React_step1](/images/React_step1.png)

:::message
Dcokerのアイコンをクリックしてもコンテナを確認出来ない場合は、Dcoker Desktopを起動してリロードしてみて下さい
:::

### 2. 起動したコンテナに接続する
Visual Studio CodeからRunning状態のコンテナに接続します。

1.先ほどのコンテナを右クリックから`Attach Visual Studio Code`をクリックします。
2.新しくVisual Studio Codeが起動します。

![React_step2](/images/React_step2.png)

### 3. 作成したコンテナと接続できている事を確認する
下記の画像が新しく起動したVisual Studio Codeになります。
パソコンの右下に`チェックマーク`が付いています。

![React_step3](/images/React_step3.png)

### 4. Reactアプリを作成する
画面左側の`フォルダーを開く`をクリックし
`/home `ディレクトリを開きます

![React_step4](/images/React_step4.png)

ターミナルを起動し、`/home`ディレクトリで
下記コマンドを実行し、Webアプリに必要なコードをインストールします。

````bash
npm install -g yarn
yarn create react-app sample_app --template typescript
````

:::message
【上記のコマンドの意味について】
yarn create react-app：yarnコマンドでReactのWebアプリを作成する
sample_app：Webアプリの名前
--template typescript：typescriptでWebアプリを構築する
:::

### 4. Reactを起動する
下記コマンドを実行して、Webアプリを起動します。
````bash
cd sample_app
yarn start
````
問題なく起動したら、上記のようなに
勝手にブラウザーが起動し、表示されます。

![React_step6](/images/React_step6.png)

## 補足情報

- **Q: 接続先のコンテナの初期のファイル構成とは？**
    - A: 下記の通りです。
    ````bash
    /bin: 基本的なユーティリティやコマンドを含むバイナリファイルが格納されます。
    /boot: Linuxカーネルとブートローダーのファイルが格納されます。
    /dev: デバイスファイルが格納されます。
    /etc: システムの設定ファイルが格納されます。
    /home: ユーザーのホームディレクトリが格納されます。
    /lib: 共有ライブラリが格納されます。
    /lib64: 64ビットアーキテクチャ向けの共有ライブラリが格納されます。
    /media: 取り外し可能なメディア（CD、DVD、USBメモリなど）が自動的にマウントされる場所です。
    /mnt: 一時的にファイルシステムをマウントする場所です。
    /opt: オプションアプリケーション用に予約されています。
    /proc: カーネルとプロセスの情報が仮想ファイルシステムとして格納されます。
    /root: rootユーザーのホームディレクトリが格納されます。
    /run: 実行時に必要なファイルが格納されます。
    /sbin: システム管理者用のコマンドが格納されます。
    /srv: システム上のサービス用にデータが格納されます。
    /sys: カーネルパラメータを含むファイルシステムが格納されます。
    /tmp: 一時的なファイルが格納されます。
    /usr: 多くのアプリケーションやユーティリティが格納されます。
    /var: 変更可能なファイルが格納されます。
    ````
## おわりに
🎡のemojiがReactのロゴみたいに見える
