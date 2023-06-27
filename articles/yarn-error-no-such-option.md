---
title: "【yarn】yarn: error: no such option: XXXを解決" # 記事のタイトル
emoji: "🧵" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["yarn", "typescript"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
nyenvのyarnでviteのアプリを作成しようとした時に
`yarn: error: no such option: --template`を実行して下記のErrorが発生しました。
```bash
$ yarn create vite hello-world --template=react-ts
Usage: yarn [options]

yarn: error: no such option: --template
```

色々調べて無事に解決出来ましたので、その方法をご紹介します。

|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・上記のエラーで`yarn: error: no such option: --template`が実行できない方  |
|  **伝えたい内容**  |  ・上記のエラーの解決方法  |

## 結論
:::message
`yarn`のバージョンを最新にしたら解決しました
:::
## Errorの再現方法
### 1. anyenvのソースを取得する
下記コマンドでanyenvのソースを取得します。
```bash
git clone https://github.com/anyenv/anyenv ~/.anyenv
```

### 2. nodenv のインストールする
下記コマンドで`nodenv`をインストールします。
```bash
exec $SHELL -l
anyenv install nodenv
exec $SHELL -l
```
#### 補足
- **Q: exec $SHELL -lとは？**
- A: exec $SHELL -lを実行することで、現在のシェルがログインシェルとして再起動され、環境の初期化が行われます。

### 3. プロジェクト作成する
下記コマンドを実行し、Viteでプロジェクトを作成しようと試みました。
```bash
$ yarn create vite hello-world --template=react-ts

Usage: yarn [options]

yarn: error: no such option: --template
```

## Errorの解決方法
### 1. yarnを削除する
yarnの最新バージョンする為に、先ほどインストールした`cmdtest/yarn`を削除します。
```bash
sudo apt remove cmdtest
sudo apt remove yarn
```
### 2. パッケージ情報を更新する
下記コマンドでarnの公開鍵を取得し、それをAPTのキーリングに追加し、
Yarnのリポジトリエントリを作成しています。
これにより、Yarnを使用してパッケージをインストールできるようになります。
```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update 
```

#### 補足
- **Q: curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg　とは？**
- A: Yarnの公開鍵をダウンロードしています。
curlはURLからデータを取得するコマンドであり、-sSオプションは進捗情報を非表示にするためのものです。ダウンロードした公開鍵は、パイプ（|）を使用して次のコマンドに渡されます。
- **Q: sudo apt-key add -　とは？**
- A: 標準入力から受け取ったデータをAPTのキーリングに追加します。
具体的には、公開鍵をキーリングに追加して信頼できるリポジトリとしてマークします。
これにより、Yarnのパッケージを正しく検証できます。
- **Q: echo "deb https://dl.yarnpkg.com/debian/ stable main　とは？**
- A: debフォーマットのAPTリポジトリのエントリを作成します。
Yarnのパッケージを含むリポジトリの場所とリポジトリ名（stable main）が指定されています。
また、パイプ（|）を使用して次のコマンドに渡されます。
- **Q: sudo tee /etc/apt/sources.list.d/yarn.list　とは？**
- A: 標準入力から受け取ったデータを指定されたファイルに書き込みます。
この場合、/etc/apt/sources.list.d/yarn.listというファイルにYarnのリポジトリエントリが書き込まれます。


### 3. yarnの再インストールする
下記コマンドで再インストールします
```bash
sudo apt-get install yarn
```

### 4. プロジェクト再作成する
yarnが正しくインストールできたか
もう一度同じコマンドを実施します
```bash
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
指示に従いプロジェクト作成の続きを行います。
```bash
cd hello-world
yarn
yarn dev
```
ローカル環境にサーバーが起動しました。
![vite+Reactの画面](/images/vitereact.png)

## おわりに
学習しているときにこのエラーで躓いたので、
解決できてよかったです！
