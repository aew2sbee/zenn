---
title: "リーダーに「このタスクをお願い！」と言われたらやること"
---


## はじめに
下記記事の内容が完了済みとして解説します。
@[card](https://zenn.dev/aew2sbee/books/aws-react-docker/viewer/chapter02)


## 1. 作業ブランチを作成する
1. 現在のブランチをプルダウンから`develop`を選択する
2. `Sync fork`をクリックし、フォーク元の情報を取り込む
![sandbooks-github-branch-step01](/images/sandbooks-github-branch-step01.png)

3. `branches`をクリックし、ブランチの一覧画面に遷移する
![sandbooks-github-branch-step02](/images/sandbooks-github-branch-step02.png)

4. 新しいブランチを作成する為に`New branch`をクリックする
![sandbooks-github-branch-step03](/images/sandbooks-github-branch-step03.png)

5. ブランチ名を入力欄に入力する
    ※今回は、`feature-#1-login-func`とします。
6. `Source`を自分のブランチ名かつ`develop`であることを確認する
7. `Create new branch`をクリックする
    ![sandbooks-github-branch-step04](/images/sandbooks-github-branch-step04.png)
    :::message
    ブランチ名は下記参照
    ````bash
    # バグ修正ブランチ
    bug-チケット番号-チケット内容

    # ホットフィックスブランチ（緊急性の高いバグ）
    hotfix-チケット番号-チケット内容

    # 新規機能開発ブランチ
    feature-チケット番号-チケット内容
    ````
    :::


8. ブランチ一覧画面の中に先ほど作成したブランチを確認する
![sandbooks-github-branch-step05](/images/sandbooks-github-branch-step05.png)

9. 下記コマンドを実行し、ローカル環境に作成したブランチを取得する
    `[new branch] feature-#1-login-func -> origin/feature-#1-login-func`を確認出来ます。
    ````bash
    git pull
    ````
    ![sandbooks-github-branch-step06](/images/sandbooks-github-branch-step06.png)

10. 下記のコマンドを実行し作成したブランチに切り替える
    ````bash
    git checkout [作成したブランチ名]
    ````
    ※今回の場合は、`git checkout feature-#1-login-func`になります。
11. `work/sandbooks ()`の後ろの文字が`demo`(切り替え前)`feature-#1-login-func`(切り替え後)に変更されたことを確認する
    ![sandbooks-github-branch-step07](/images/sandbooks-github-branch-step07.png)


## 2. タスク内容の作業を行う

:::message
作業内容は各タスクで異なる為、割愛します。
**ただし、分かりやすいように、今回は`test.txt`を作成しました。**
:::

## 3. 作業内容をコミットする
1. 下記コマンドを実行し、作業内容を確認する
    ````bash
    git stetus
    ````
    作成した`test.txt`が赤文字で表示されていることを確認する
    ※**赤色文字**は、前回のコミットから変化があったファイルという意味
    ![sandbooks-github-branch-step08](/images/sandbooks-github-branch-step08.png)

2. 下記コマンドを実行し、作業内容を確認する
    ````bash
    git add [作業したファイル名]
    ````
    ※今回の場合は、`git add test.txt`になります。
    ※**緑色文字**は、ステージに変更されたという意味
    ![sandbooks-github-branch-step09](/images/sandbooks-github-branch-step09.png)


3. コミットメッセージを残す
    ````bash
    git commit -m "[コミットメッセージ]"
    ````
    ※今回の場合は、`git commit -m "create test.txt"`になります。

    ![sandbooks-github-branch-step10](/images/sandbooks-github-branch-step10.png)

4. 下記コマンドを実行し、先ほどのコミットがされているか確認する
    ````bash
    git log
    ````
    ログの中に`create test.txt`のメッセージがある事を確認する
    ![sandbooks-github-branch-step11](/images/sandbooks-github-branch-step11.png)

5. 下記コマンドを実行し、リモートリポジトリにアップロードする
    ````bash
    git push
    ````
    ![sandbooks-github-branch-step12](/images/sandbooks-github-branch-step12.png)


## 4. プルリクエストを出す
1. `https://github.com/[会社のアカウント名]/sandbooks`にアクセスする
2. pull requestのメッセージの中に**作業したブランチ名**を含まれている事を確認する
3. `Compare ＆ pull request`をクリックする
    ![sandbooks-github-branch-step13](/images/sandbooks-github-branch-step13.png)

4. 左側(緑色：取り込み先)が目的の`レポジトリー`と`ブランチ`である事を確認する
5. 右側(水色：取り込み元)が目的の`レポジトリー`と`ブランチ`である事を確認する
6. `✓ Able to merege`である事を確認する
    ![sandbooks-github-branch-step14](/images/sandbooks-github-branch-step14.png)

7. pull requestのタイトルを作成したブランチ名と一緒にする
    ![sandbooks-github-branch-step15](/images/sandbooks-github-branch-step15.png)

8. pull requestの作業内容を他のメンバーに分かるように内容をMD形式で書き込む
    @[card](https://zenn.dev/zenn/articles/markdown-guide)
    ![sandbooks-github-branch-step16](/images/sandbooks-github-branch-step16.png)

9. pull requestが完成したら、`Create pull request`をクリックする
    ![sandbooks-github-branch-step17](/images/sandbooks-github-branch-step17.png)

## 5. レビュー依頼する
1. レビュー者にレビュー依頼を行う
2. レビュー者からの指摘に対応する　※必要に応じて

    :::message
    今回は、内容を割愛します。
    :::

## 6. マージする
1. レビュー者からの承認を頂いたら、`Squash and merge`をクリックする
![sandbooks-github-branch-step18](/images/sandbooks-github-branch-step18.png)


## おわり
タスクの着手から完了までの流れを解説しました。
覚えることが多いですが、ゆくゆくは、この記事を見ずに作業できるようになりましょう！