---
title: '[yarn] yarn: error: no such option: XXXを解決' # 記事のタイトル
emoji: '🧵' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['yarn', 'typescript'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

nyenv の yarn で vite のアプリを作成しようとした時に
`yarn: error: no such option: --template`を実行して下記の Error が発生しました。

```bash
$ yarn create vite hello-world --template=react-ts
Usage: yarn [options]

yarn: error: no such option: --template
```

色々調べて無事に解決出来ましたので、その方法をご紹介します。

| 項目             | 内容                                                                      |
| ---------------- | ------------------------------------------------------------------------- |
| **対象者**       | ・上記のエラーで`yarn: error: no such option: --template`が実行できない方 |
| **伝えたい内容** | ・上記のエラーの解決方法                                                  |

## 結論

:::message
`yarn`のバージョンを最新にしたら解決しました
:::

## Error の再現方法

### 1. anyenv のソースを取得する

下記コマンドで anyenv のソースを取得します。

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

- **Q: exec $SHELL -l とは？**
- A: exec $SHELL -l を実行することで、現在のシェルがログインシェルとして再起動され、環境の初期化が行われます。

### 3. プロジェクト作成する

下記コマンドを実行し、Vite でプロジェクトを作成しようと試みました。

```bash
$ yarn create vite hello-world --template=react-ts

Usage: yarn [options]

yarn: error: no such option: --template
```

## Error の解決方法

### 1. yarn を削除する

yarn の最新バージョンする為に、先ほどインストールした`cmdtest/yarn`を削除します。

```bash
sudo apt remove cmdtest
sudo apt remove yarn
```

### 2. パッケージ情報を更新する

下記コマンドで arn の公開鍵を取得し、それを APT のキーリングに追加し、
Yarn のリポジトリエントリを作成しています。
これにより、Yarn を使用してパッケージをインストールできるようになります。

```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
```

#### 補足

- **Q: curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg　とは？**
- A: Yarn の公開鍵をダウンロードしています。
  curl は URL からデータを取得するコマンドであり、-sS オプションは進捗情報を非表示にするためのものです。ダウンロードした公開鍵は、パイプ（|）を使用して次のコマンドに渡されます。
- **Q: sudo apt-key add -　とは？**
- A: 標準入力から受け取ったデータを APT のキーリングに追加します。
  具体的には、公開鍵をキーリングに追加して信頼できるリポジトリとしてマークします。
  これにより、Yarn のパッケージを正しく検証できます。
- **Q: echo "deb https://dl.yarnpkg.com/debian/ stable main 　とは？**
- A: deb フォーマットの APT リポジトリのエントリを作成します。
  Yarn のパッケージを含むリポジトリの場所とリポジトリ名（stable main）が指定されています。
  また、パイプ（|）を使用して次のコマンドに渡されます。
- **Q: sudo tee /etc/apt/sources.list.d/yarn.list 　とは？**
- A: 標準入力から受け取ったデータを指定されたファイルに書き込みます。
  この場合、/etc/apt/sources.list.d/yarn.list というファイルに Yarn のリポジトリエントリが書き込まれます。

### 3. yarn の再インストールする

下記コマンドで再インストールします

```bash
sudo apt-get install yarn
```

### 4. プロジェクト再作成する

yarn が正しくインストールできたか
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

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
