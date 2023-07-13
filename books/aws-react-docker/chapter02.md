---
title: "作業する場所(ローカル環境)を確認しよう！"
---
:::message
作業ディレクトリを既に理解している方は、この章は読み飛ばして頂いても大丈夫です。
:::


## はじめに
下記条件が前提条件として解説します。
- GitHubアカウントが発行済み
- Git Bashがインストール済み/初期設定済み
- ホームディレクトリ配下に`Work`ディレクトリが作成済み
- `Work`配下に`sandbooks`がクローン済み

## 1. Git Bashを起動する
検索欄に`git bash`と検索し、**Git Bash**アプリを起動する
![sandbooks-git-step01](/images/sandbooks-git-step01.png)


## 2. 作業する場所に移動する
下記コマンドは、ホームディレクトリに一旦移動し、`Work`配下にあるクローンした`sandbooks`に移動します。
````bash
cd ~
cd Work/sandbooks
````
下記の画像の通り`sandbooks`まで移動する事が出来ました。
![sandbooks-git-step02](/images/sandbooks-git-step02.png)

今後は、この場所で作業を行います。

## Error対応

### Q: `bash: cd: Work/sandbooks: No such file or directory`とErrorが発生する場合は？
:::message alert
`sandbooks`を`git clone`していない可能性があります。
:::
1. `https://github.com/ユーザー名/sandbooks`をURLに入力して表示されるのか？
    - `404 This is not the web page you are looking for.`と表示される場合は、フォークされていない可能があります。
2. 下記コマンドを実行し、`git clone`します。
````bash
cd ~
cd Work/
git clone git@github.com:[ユーザー名]/sandbooks.git
````
3. 手順2のコマンドを実行する

****
### Q: `404 This is not the web page you are looking for.`と表示される場合は？
:::message alert
`sandbooks`をフォークされていない可能があります。
:::
1. `https://github.com/[会社のアカウント名]/sandbooks`にアクセスする
2. `Fork`の右横の`▼`をクリックする
![sandbooks-git-step03](/images/sandbooks-git-step03.png)
3. `Create a noe fork`をクリックする
![sandbooks-git-step04](/images/sandbooks-git-step04.png)
4. `Owner`を自分のアカウントに選択し、`Create fork`をクリックする
![sandbooks-git-step05](/images/sandbooks-git-step05.png)
5. `https://github.com/ユーザー名/sandbooks`をURLに入力して表示する事を確認する

#### 補足情報

|  単語  | 意味  |
| ---- | ---- |
|  **`cd`**  |  `cd`とは、コンピュータ上でディレクトリ（フォルダ）を変更するためのコマンドです。<br>`cd`は、Change Directory（ディレクトリを変更する）の略です。<br>コンピュータでは、ファイルやフォルダは階層構造で整理されており、ディレクトリと呼ばれる場所に格納されています。<br><br>`cd`コマンドを使うことで、現在の作業ディレクトリ（現在いる場所）を別のディレクトリに変更することができます。<br>これは、コンピュータ上で異なるディレクトリに移動したい場合に便利です。  |
|  **`cd ~`**  |  `cd ~`とは、現在の作業ディレクトリがユーザーのホームディレクトリにコマンドです。  |
|  **ホームディレクトリ**  |  ホームディレクトリとは、ユーザーアカウントごとに割り当てられる特別なディレクトリであり、そのユーザーが最初にログインしたときに配置される場所です。ユーザーの個人的なファイルや設定ファイルが格納される場所として機能します。<br><br>ホームディレクトリは、ユーザーアカウント名と同じ名前を持ち、通常はユーザー名がそのまま使用されます。例えば、ユーザー名が「alice」であれば、そのユーザーのホームディレクトリは「/home/alice」となります |