---
title: "【AWS】VSCodeからEC2クラウドにSSH接続する" # 記事のタイトル
emoji: "🌇" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["aws", "ec2", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
VSCodeからEC2で立ち上げたサーバーに接続する方法を学習しましたので執筆します。

|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・VSCodeからEC2に接続する方法を知らない方  |
|  **伝えたい内容**  |  ・VSCodeからEC2に接続する方法  |
|  **前提条件**  |  ・AWSのアカウント作成済み<br>・Windows<br>・EC2でサーバー構築済み |
     
## VSCodeからSSH接続
### 1. サーバーを起動する
目的のサーバーのインスタンス状態が`実行中`にである事確認します。
次に、インスタンスIDの`リンク`をクリックしページ遷移します。
![vscode-ssh-step1](/images/vscode-ssh-step1.png)

### 2. 接続に関する情報の確認する
`接続`ボタンをクリックし、詳細ページに遷移します。
![vscode-ssh-step2](/images/vscode-ssh-step2.png)

### 3. SSHコマンドのコピーする
`SSHクライアント`をクリックし、`SSHコマンド`をコピーします
![vscode-ssh-step3](/images/vscode-ssh-step3.png)

- **Q: SSHコマンドの意味とは？**
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
起動の度に新しいSSHコマンドを用意し、再びSSH接続する必要があります。
:::

### 5. VSCodeに拡張機能の追加する
拡張機能の検索欄から`ssh`と検索し`インストール`して下さい。
![vscode-ssh-step4](/images/vscode-ssh-step4.png)


### 6. SSHコマンドの実行する
画面左側の`パソコンマーク`をクリックし
SSHと書かれているバーの`＋`をクリックし
入力欄が表示されるため、先ほどの`SSHコマンド`を入力し実行する
![vscode-ssh-step5](/images/vscode-ssh-step5.png)
:::message
例: 入力欄にSSHコマンドを入力した状態
:::
![vscode-ssh-step6](/images/vscode-ssh-step6.png)


### 7. configファイルの指定する
自分の名前のファイルの`config`ファイルを選択し、接続情報を記録します。
![vscode-ssh-step7](/images/vscode-ssh-step7.png)

### 8. SSH接続を開始
右下のポップアップの`接続`ボタンをクリックすると、接続が開始されます。
![vscode-ssh-step8](/images/vscode-ssh-step8.png)

### 9. OSの選択する
今回は、`ubuntu`なので`Linux`を選択します。
選択後、少しロードが行われて接続が完了します。
![vscode-ssh-step9](/images/vscode-ssh-step9.png)


## SSH接続の確認

### 1. アイコンで接続状態を確認する
画面左側のSSHアイコンのが`緑色`に変化を確認する
![vscode-ssh-step10](/images/vscode-ssh-step10.png)

### 2. 接続先のファイルを開く
`ファルダーを開く`をクリックし、
パスを確認し、`OK`をクリックすると、接続先のファイルを確認出来ます。
![vscode-ssh-step11](/images/vscode-ssh-step11.png)

`OK`をクリック後、`ファイルの作成者に信頼しますか？`と尋ねられるので
`信頼します`と選択してください。
![vscode-ssh-step12](/images/vscode-ssh-step12.png)
信頼を確認後、下記の画像のように接続先のサーバーのファイル構成が確認が出来ます。

![vscode-ssh-step13](/images/vscode-ssh-step13.png)

## おわりに
EC2のロゴを見たらこの(🌇)emojiにも似ているなと思いました。





