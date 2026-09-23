---
title: "[Docker] Reactの環境構築" # 記事のタイトル
emoji: "🐳" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["docker", "react", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

Docker コンテナ上に Node.js の環境を作り、VS Code から接続して React アプリを起動するまでの手順をまとめます。
ローカルの PC に Node.js をインストールしなくても、コンテナの中だけで React を動かせます。

下記の画面が表示されるまでを解説します。

![ブラウザでReactアプリの初期画面が表示された状態](/images/articles/docker-react-env/React_step6.png)

:::message alert
この記事は執筆時点（2023年）の手順です。
記事で使っている Node.js 14 はサポートが終了しており、Create React App も 2025年2月に非推奨になりました。これから始める場合は、現行の LTS 版の Node.js イメージと、Vite などの公式に推奨されているツールを使うことをおすすめします。
:::

|項目|内容|
|---|---|
|**対象者**|・Docker 初学者|
|**伝えたい内容**|・Docker コンテナ上に Node.js の環境を作り、VS Code から接続して React アプリを起動する方法|
|**前提条件**|・Docker Desktop をインストール済み<br>・VS Code と拡張機能（Docker、Dev Containers）をインストール済み|
|**使用するイメージ**|・node:14.17.0（Docker Hub から取得するため、ローカルへの Node.js のインストールは不要）|

Docker Desktop のインストールは、下記の記事を参考にしてください。

@[card](https://zenn.dev/aew2sbee/articles/docker-desktop-install)

### 用語

- **イメージ**: 環境の設計図。今回は Node.js が入った `node:14.17.0` を使います。
- **コンテナ**: イメージから作った実際の実行環境。この中で React を動かします。

## 🌱 コンテナを作成する

### 1. docker run コマンド

Docker Desktop を起動した状態で、ホストのターミナル（PowerShell など）で下記のコマンドを実行します。

```bash
docker run -td -p 3000:3000 --name="React-env" node:14.17.0 /bin/bash
```

:::message
【上記のコマンドの意味について】

- `-t`: 疑似ターミナルを割り当てる（`/bin/bash` を起動したままにして、コンテナがすぐ終了しないようにする）
- `-d`: バックグラウンドで実行する
- `-p 3000:3000`: コンテナの 3000 番ポートを、ホストの 3000 番ポートに公開する
- `--name="React-env"`: コンテナの名前
- `node:14.17.0`: 使用するイメージとタグ（Node.js のバージョン）
- `/bin/bash`: コンテナの起動時に実行するコマンド

このコマンドで、コンテナが作成され、同時に起動します。
:::

### 2. 作成したコンテナを確認する

Docker Desktop を開き、先ほどのコンテナが作成できていることを確認します。

![Docker Desktopのコンテナ一覧にReact-envが表示された状態](/images/articles/docker-react-env/docker_desktop_step7.png)

## 🌱 React アプリを起動する

### 1. コンテナを起動する（停止している場合）

`docker run` を実行した直後は、コンテナはすでに起動しています。
PC を再起動したあとなど、コンテナが停止している場合は、下記の手順で起動します。

1. Visual Studio Code の左側の `Docker` のアイコンをクリックします。
2. 先ほど作成したコンテナを右クリックし、`Start` を押してコンテナを起動します。

![VS CodeのDocker拡張機能でコンテナを起動する画面](/images/articles/docker-react-env/React_step1.png)

:::message
Docker のアイコンをクリックしてもコンテナを確認できない場合は、Docker Desktop を起動してリロードしてみてください。
:::

### 2. 起動したコンテナに接続する

Visual Studio Code から、Running 状態のコンテナに接続します。

1. 先ほどのコンテナを右クリックし、`Attach Visual Studio Code` をクリックします。
2. 新しく Visual Studio Code が起動します。

![コンテナを右クリックしてAttach Visual Studio Codeを選ぶ画面](/images/articles/docker-react-env/React_step2.png)

新しく開いた Visual Studio Code では、コンテナの中のファイルやターミナルを操作します。以降のコマンドは、ローカル PC ではなくコンテナの中で実行されます。

### 3. 作成したコンテナと接続できていることを確認する

下記の画像が、新しく起動した Visual Studio Code です。
画面の左下に、接続先のコンテナが表示されていれば接続できています。

![コンテナに接続したVS Codeの画面](/images/articles/docker-react-env/React_step3.png)

### 4. React アプリを作成する

画面左側の `フォルダーを開く` をクリックし、ユーザー用の作業領域である `/home` ディレクトリを開きます。

![フォルダーを開くで/homeを選択する画面](/images/articles/docker-react-env/React_step4.png)

メニューの `ターミナル` > `新しいターミナル` でターミナルを開き、`/home` ディレクトリで下記のコマンドを実行して、React アプリのひな形を作成します。
公式の node イメージには yarn が最初から入っているため、yarn のインストールは不要です（`yarn -v` でバージョンを確認できます）。

```bash
yarn create react-app sample_app --template typescript
```

:::message
【上記のコマンドの意味について】

- `yarn create react-app`: yarn コマンドで React の Web アプリのひな形を作成する
- `sample_app`: Web アプリの名前
- `--template typescript`: TypeScript で Web アプリを構築する
:::

### 5. React を起動する

下記のコマンドを実行して、Web アプリを起動します。

```bash
cd sample_app
yarn start
```

問題なく起動したら、ホストのブラウザで `http://localhost:3000` を開くと、下記の画面が表示されます。
（VS Code からコンテナに接続している場合は、VS Code がポートを自動で転送するため、通知からブラウザを開くこともできます）

![ブラウザでReactアプリの初期画面が表示された状態](/images/articles/docker-react-env/React_step6.png)

:::message alert
アプリはコンテナの中の `/home` に作成されるため、`docker rm` でコンテナを削除するとコードも消えます。残したい場合は、ボリュームやバインドマウント（`-v` オプション）を使ってください。
:::

## 🌱 おわりに

- `docker run` で Node.js のイメージからコンテナを作成・起動する
- VS Code の `Attach Visual Studio Code` でコンテナに接続する
- コンテナの中で `yarn create react-app` を実行し、`yarn start` で起動する
