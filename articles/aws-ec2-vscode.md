---
title: "[AWS] VSCodeからEC2インスタンスにSSH接続する" # 記事のタイトル
emoji: "☁️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["aws", "ec2", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

VSCode（Visual Studio Code）から、EC2 で立ち上げたサーバーに接続する方法を学習したので執筆します。
VSCode からサーバーに接続すると、サーバー上のファイルを手元のエディタで直接編集できます。

接続には SSH を使います。SSH は、ネットワーク越しに別のコンピューターへ安全にログインして操作するための仕組みで、認証には鍵ファイル（秘密鍵）を使います。

|項目|内容|
|---|---|
|**対象者**|・ターミナルではなく VSCode から EC2 のファイルを編集したい方|
|**伝えたい内容**|・Remote - SSH 拡張機能で EC2 に接続し、サーバー上のフォルダーを開くまでの手順|
|**前提条件**|・AWS のアカウント作成済み<br>・Windows（OpenSSH クライアントが使えること）<br>・EC2 でインスタンス（仮想サーバー）構築済み<br>・キーペア（.pem ファイル）をダウンロード済み<br>・セキュリティグループで SSH（22番ポート）を自分の IP アドレスに許可済み|

:::message
画面のスクリーンショットは、執筆時点（2023年）のものです。現在の AWS コンソールや VSCode とは表示が異なる場合があります。
:::

## 🌱 事前準備

### 1. VSCode に拡張機能を追加する

拡張機能の検索欄に `ssh` と入力し、Microsoft 製の `Remote - SSH`（ID: `ms-vscode-remote.remote-ssh`）をインストールしてください。
似た名前の拡張機能が複数表示されるので、名前と提供元を確認してください。

![VSCodeの拡張機能の検索結果にRemote - SSHが表示されている画面](/images/articles/aws-ec2-vscode/vscode-ssh-step4.png)

### 2. 秘密鍵を配置する

EC2 のキーペア作成時にダウンロードした `.pem` ファイルを、`C:\Users\<ユーザー名>\.ssh\` に移動します。
`.ssh` フォルダーがなければ作成してください。

## 🌱 AWS コンソールで SSH コマンドを取得する

### 1. インスタンスが実行中か確認する

目的のインスタンスのインスタンス状態が `実行中` であることを確認します。
停止中の場合は、`インスタンスの状態` から `インスタンスを開始` を選択します。
次に、インスタンス ID のリンクをクリックし、詳細ページに移動します。

![EC2のインスタンス一覧でインスタンス状態が実行中になっている画面](/images/articles/aws-ec2-vscode/vscode-ssh-step1.png)

### 2. 接続画面を開く

`接続` ボタンをクリックし、接続画面に移動します。

![インスタンス概要ページの接続ボタン](/images/articles/aws-ec2-vscode/vscode-ssh-step2.png)

### 3. SSH コマンドをコピーする

`SSH クライアント` タブをクリックし、SSH コマンドをコピーします。

![SSHクライアントタブに表示されたSSHコマンドの例](/images/articles/aws-ec2-vscode/vscode-ssh-step3.png)

コピーしたコマンドは `ssh -i "秘密鍵のファイルパス" ユーザー名@ホスト名` という形式です。

- `-i`: 認証に使う秘密鍵のファイルを指定するオプション
- ユーザー名: AMI（OS イメージ）ごとに決まっています。Ubuntu なら `ubuntu`、Amazon Linux なら `ec2-user` です
- ホスト名: インスタンスのパブリック DNS 名

### 4. 秘密鍵のファイルパスを変更する

コマンド内の秘密鍵のパスを、`.ssh` フォルダーに置いた秘密鍵のパスに変更します。
VSCode から実行したときにも鍵ファイルが見つかるように、`~/.ssh/` から始まる形式で指定します。

:::message
今回の場合は、下記のように変更しました（ホスト名はダミーの値です）。

```diff bash
- ssh -i "aws_for_study.pem" ubuntu@ec2-192-0-2-1.ap-northeast-1.compute.amazonaws.com
+ ssh -i "~/.ssh/aws_for_study.pem" ubuntu@ec2-192-0-2-1.ap-northeast-1.compute.amazonaws.com
```

:::

## 🌱 VSCode から SSH 接続する

### 1. SSH コマンドを実行する

画面左側のアクティビティバーにある `リモート エクスプローラー`（パソコンのアイコン）をクリックします。
次に、SSH と書かれているバーの `＋` をクリックします。
入力欄が表示されたら、先ほどの SSH コマンドを入力して実行します。

![リモートエクスプローラーのSSHの横にある＋ボタン](/images/articles/aws-ec2-vscode/vscode-ssh-step5.png)

![入力欄にSSHコマンドを入力した状態](/images/articles/aws-ec2-vscode/vscode-ssh-step6.png)
*入力欄に SSH コマンドを入力した状態*

### 2. config ファイルを指定する

接続情報を保存する SSH の設定ファイルとして、`C:\Users\<ユーザー名>\.ssh\config` を選択します。
ここに保存しておくと、次回からはリモートエクスプローラーの一覧から選ぶだけで接続できます。

![接続情報を保存するconfigファイルの選択画面](/images/articles/aws-ec2-vscode/vscode-ssh-step7.png)

### 3. SSH 接続を開始する

右下のポップアップの `接続` ボタンをクリックすると、接続が開始されます。

![右下に表示された接続ボタンのポップアップ](/images/articles/aws-ec2-vscode/vscode-ssh-step8.png)

### 4. OS を選択する

接続先のサーバーの OS を選択します。今回は Ubuntu なので `Linux` を選択します。
選択後、サーバー側に VSCode Server がインストールされ、接続が完了します。

![接続先のOSとしてLinuxを選択する画面](/images/articles/aws-ec2-vscode/vscode-ssh-step9.png)

## 🌱 SSH 接続の確認

### 1. 接続状態を確認する

リモートエクスプローラーで、接続先のアイコンにチェックマークが付いていることを確認します。
画面左下のリモートインジケーターにも、接続先のホスト名（`SSH: ホスト名`）が表示されます。

![リモートエクスプローラーの接続先にチェックマークが付き、左下に接続先が表示された状態](/images/articles/aws-ec2-vscode/vscode-ssh-step10.png)

### 2. 接続先のフォルダーを開く

`フォルダーを開く` をクリックし、パス（既定は `/home/ubuntu/`）を確認して `OK` をクリックします。

![接続先のフォルダーを開くためのパス入力画面](/images/articles/aws-ec2-vscode/vscode-ssh-step11.png)

`OK` をクリックすると、フォルダー内のファイルの作成者を信頼するかどうかを尋ねられます。

![ワークスペースの信頼を確認するダイアログ](/images/articles/aws-ec2-vscode/vscode-ssh-step12.png)

:::message alert
信頼すると、フォルダー内のファイルによってコードが自動実行される場合があります。
自分で構築したサーバーなど、中身を把握しているフォルダーの場合だけ信頼してください。
:::

信頼すると、下記の画像のように接続先のサーバーのファイル構成を確認できます。

![VSCodeのエクスプローラーに接続先サーバーのファイルが表示された状態](/images/articles/aws-ec2-vscode/vscode-ssh-step13.png)

## 🌱 再接続時の注意

:::message alert
インスタンスを停止して再び開始すると、パブリック IP アドレスとパブリック DNS 名が変わります（再起動では変わりません）。
そのたびに、`C:\Users\<ユーザー名>\.ssh\config` の `HostName` を新しいパブリック DNS 名に書き換えてください。
ホスト名を固定したい場合は Elastic IP を使います。ただし、パブリック IPv4 アドレスは有料です。
:::

## 🌱 まとめ

- 事前に Remote - SSH 拡張機能を入れ、秘密鍵を `.ssh` フォルダーに置く
- AWS コンソールで SSH コマンドをコピーし、秘密鍵のパスを書き換える
- VSCode のリモートエクスプローラーから SSH コマンドを実行し、config に接続情報を保存する
- インスタンスを停止・開始したら、config の `HostName` を更新する
