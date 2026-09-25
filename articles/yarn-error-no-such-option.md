---
title: "[yarn] yarn: error: no such option: XXXを解決" # 記事のタイトル
emoji: "🧵" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["yarn", "typescript"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

anyenv で Node.js の環境を構築し、Yarn で Vite のアプリを作成しようと`yarn create vite hello-world --template=react-ts`を実行したところ、下記のエラーが発生しました。

```text
yarn: error: no such option: --template
```

解決できたので、その方法を紹介します。

| 項目             | 内容                                                                                      |
| ---------------- | ----------------------------------------------------------------------------------------- |
| **対象者**       | ・`yarn create vite`の実行時に`yarn: error: no such option: --template`が発生する方        |
| **実行環境**     | ・WSL 上の Ubuntu（`apt`を使う Linux）                                                     |
| **確認バージョン** | ・Yarn 1.22.19（Yarn Classic）、create-vite 4.1.0（記事執筆時点）                        |

各ツールの役割は次のとおりです。

- **anyenv / nodenv**: Node.js のバージョンを切り替えて使うためのツール
- **Yarn**: npm と同じ JavaScript のパッケージマネージャー
- **Vite**: フロントエンドの開発環境を作るツール

## 🌱 結論

:::message
`yarn`コマンドの実体が、Yarn ではなく Ubuntu の`cmdtest`パッケージに含まれる同名の別ツールでした。
`cmdtest`を削除し、Yarn の公式リポジトリから Yarn をインストールし直したら解決しました。
:::

## 🌱 原因

Ubuntu の`cmdtest`パッケージには、JavaScript の Yarn とは別物の`yarn`というコマンドが含まれています。
このコマンドが実行されると、Yarn のオプション（`--template`など）を解釈できないため、`no such option`のエラーになります。
Yarn の公式ドキュメントにも、`cmdtest`が入っている場合は先に削除するよう案内があります。

@[card](https://classic.yarnpkg.com/lang/en/docs/install/)

`yarn --version`を実行して`1.22.19`のような Yarn のバージョンが表示されない場合や、`which yarn`の結果が`/usr/bin/yarn`になっている場合は、`cmdtest`の`yarn`が使われている可能性があります。

```bash
which yarn
yarn --version
```

## 🌱 エラーの再現方法

### 1. anyenv をインストールする

下記コマンドで anyenv のソースを取得し、シェルの設定ファイルに PATH と初期化の設定を追記します。
初回は`anyenv install --init`で、インストールに使う定義ファイル（manifest）を初期化します。

```bash
git clone https://github.com/anyenv/anyenv ~/.anyenv
echo 'export PATH="$HOME/.anyenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(anyenv init -)"' >> ~/.bashrc
exec $SHELL -l
anyenv install --init
```

@[card](https://github.com/anyenv/anyenv)

#### 補足

- **Q: `exec $SHELL -l`とは？**
- A: シェルを再起動して設定ファイル（`~/.bashrc`など）を読み直し、追加したコマンドを使えるようにします。

### 2. nodenv と Node.js をインストールする

下記コマンドで`nodenv`をインストールし、Node.js をインストールします。
`<バージョン>`には、`nodenv install -l`で表示される一覧から使いたいバージョンを指定します。

```bash
anyenv install nodenv
exec $SHELL -l
nodenv install <バージョン>
nodenv global <バージョン>
```

### 3. プロジェクトを作成する

`cmdtest`が入っている環境で、下記コマンドを実行し、Vite でプロジェクトを作成しようと試みました。

```console
$ yarn create vite hello-world --template=react-ts

Usage: yarn [options]

yarn: error: no such option: --template
```

## 🌱 エラーの解決方法

### 1. cmdtest を削除する

`cmdtest`を削除します。

```bash
sudo apt remove cmdtest
```

### 2. Yarn の公式リポジトリを追加する

下記コマンドで、APT（Ubuntu のパッケージ管理ツール）が Yarn の公式リポジトリから Yarn をダウンロードできるように登録し、パッケージ情報を更新します。
`/etc/apt/keyrings`ディレクトリがない場合は、先に`sudo mkdir -p /etc/apt/keyrings`で作成してください。

```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | sudo tee /etc/apt/keyrings/yarn-archive-keyring.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/yarn-archive-keyring.gpg] https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
```

#### 補足

- **Q: 1 行目のコマンドは何をしている？**
- A: Yarn の公開鍵をダウンロードし、`/etc/apt/keyrings/yarn-archive-keyring.gpg`に保存しています。公開鍵は、ダウンロードしたパッケージが本物かを検証するために使います。`curl`の`-sS`オプションは、進捗を表示せず、エラーが起きた場合だけメッセージを表示するためのものです。
- **Q: 2 行目のコマンドは何をしている？**
- A: Yarn の公式リポジトリの場所を`/etc/apt/sources.list.d/yarn.list`に書き込んでいます。`signed-by`で、このリポジトリの検証には 1 行目で保存した公開鍵だけを使うよう指定しています。

:::message
以前は`sudo apt-key add -`で公開鍵を登録する方法が使われていましたが、`apt-key`は非推奨になっています。
:::

### 3. Yarn を再インストールする

下記コマンドで Yarn をインストールします。
`--no-install-recommends`を付けると、推奨パッケージとして Ubuntu 標準の Node.js が一緒にインストールされるのを防げます。
ここでインストールされるのは Yarn Classic（1.x 系）です。

```bash
sudo apt-get install --no-install-recommends yarn
```

:::message
Yarn の公式ドキュメントでは、現在は Node.js に付属する Corepack（`corepack enable`）で Yarn を使えるようにする方法が推奨されています。
@[card](https://yarnpkg.com/getting-started/install)
:::

### 4. プロジェクトを再作成する

Yarn が正しくインストールできたかを確認するため、もう一度同じコマンドを実行します。
下記は記事執筆時点（Yarn 1.22.19、create-vite 4.1.0）の出力です。バージョンによっては、途中で質問が表示される場合があります。

```console
$ yarn create vite hello-world --template=react-ts
yarn create v1.22.19
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Installed "create-vite@4.1.0" with binaries:
      - create-vite
      - cva
[##] 2/2
Scaffolding project in /mnt/c/Users/User/work/study/hello-world...

Done. Now run:

  cd hello-world
  yarn
  yarn dev

Done in 1.45s.
```

指示に従い、プロジェクト作成の続きを行います。

```bash
cd hello-world
yarn      # package.json に書かれたパッケージをインストールする
yarn dev  # 開発サーバーを起動する
```

ターミナルに表示された URL（`http://localhost:5173/`など）をブラウザで開くと、下記の画面が表示されます。開発サーバーは`Ctrl + C`で停止できます。

![vite+Reactの画面](/images/articles/yarn-error-no-such-option/vitereact.png)

## 🌱 おわりに

`yarn`で`no such option`のエラーが出た場合は、まず`which yarn`と`yarn --version`で、`cmdtest`の`yarn`が使われていないかを確認するとよいです。
