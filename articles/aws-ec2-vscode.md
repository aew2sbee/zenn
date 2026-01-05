---
title: "[AWS] VSCodeからEC2クラウドにSSH接続する" # 記事のタイトル
emoji: "☁️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["aws", "ec2", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## はじめに

VSCode から EC2 で立ち上げたサーバーに接続する方法を学習しましたので執筆します。

|項目|内容|
|---|---|
|**対象者**|・VSCode から EC2 に接続する方法を知らない方|
|**伝えたい内容**|・VSCode から EC2 に接続する方法|
|**前提条件**|・AWS のアカウント作成済み<br>・Windows<br>・EC2 でサーバー構築済み|

## VSCode から SSH 接続

### 1. サーバーを起動する

目的のサーバーのインスタンス状態が`実行中`にである事確認します。
次に、インスタンス ID の`リンク`をクリックしページ遷移します。
![vscode-ssh-step1](/images/articles/aws-ec2-vscode/vscode-ssh-step1.png)

### 2. 接続に関する情報の確認する

`接続`ボタンをクリックし、詳細ページに遷移します。
![vscode-ssh-step2](/images/articles/aws-ec2-vscode/vscode-ssh-step2.png)

### 3. SSH コマンドのコピーする

`SSHクライアント`をクリックし、`SSHコマンド`をコピーします
![vscode-ssh-step3](/images/articles/aws-ec2-vscode/vscode-ssh-step3.png)

- **Q: SSH コマンドの意味とは？**
  - A: ssh -i "秘密鍵のファイルパス" ユーザー名@ホスト名

### 4. 秘密鍵のファイルパスの変更する

`.ssh/配下`にある秘密鍵のパスに変更します。

:::message
今回の場合は、下記のように変更しました。

```diff bash
- ssh -i "aws_for_study.pem" ubuntu@ec2-18-183-101-70.ap-northeast-1.compute.amazonaws.com
+ ssh -i "./.ssh/aws_for_study.pem" ubuntu@ec2-18-183-101-70.ap-northeast-1.compute.amazonaws.com
```

:::
:::message alert
インスタンスが起動する度に、ホスト名が変更される為、
起動の度に新しい SSH コマンドを用意し、再び SSH 接続する必要があります。
:::

### 5. VSCode に拡張機能の追加する

拡張機能の検索欄から`ssh`と検索し`インストール`して下さい。
![vscode-ssh-step4](/images/articles/aws-ec2-vscode/vscode-ssh-step4.png)

### 6. SSH コマンドの実行する

画面左側の`パソコンマーク`をクリックし
SSH と書かれているバーの`＋`をクリックし
入力欄が表示されるため、先ほどの`SSHコマンド`を入力し実行する
![vscode-ssh-step5](/images/articles/aws-ec2-vscode/vscode-ssh-step5.png)
:::message
例: 入力欄に SSH コマンドを入力した状態
:::
![vscode-ssh-step6](/images/articles/aws-ec2-vscode/vscode-ssh-step6.png)

### 7. config ファイルの指定する

自分の名前のファイルの`config`ファイルを選択し、接続情報を記録します。
![vscode-ssh-step7](/images/articles/aws-ec2-vscode/vscode-ssh-step7.png)

### 8. SSH 接続を開始

右下のポップアップの`接続`ボタンをクリックすると、接続が開始されます。
![vscode-ssh-step8](/images/articles/aws-ec2-vscode/vscode-ssh-step8.png)

### 9. OS の選択する

今回は、`ubuntu`なので`Linux`を選択します。
選択後、少しロードが行われて接続が完了します。
![vscode-ssh-step9](/images/articles/aws-ec2-vscode/vscode-ssh-step9.png)

## SSH 接続の確認

### 1. アイコンで接続状態を確認する

画面左側の SSH アイコンのが`緑色`に変化を確認する
![vscode-ssh-step10](/images/articles/aws-ec2-vscode/vscode-ssh-step10.png)

### 2. 接続先のファイルを開く

`ファルダーを開く`をクリックし、
パスを確認し、`OK`をクリックすると、接続先のファイルを確認出来ます。
![vscode-ssh-step11](/images/articles/aws-ec2-vscode/vscode-ssh-step11.png)

`OK`をクリック後、`ファイルの作成者に信頼しますか？`と尋ねられるので
`信頼します`と選択してください。
![vscode-ssh-step12](/images/articles/aws-ec2-vscode/vscode-ssh-step12.png)
信頼を確認後、下記の画像のように接続先のサーバーのファイル構成が確認が出来ます。

![vscode-ssh-step13](/images/articles/aws-ec2-vscode/vscode-ssh-step13.png)
