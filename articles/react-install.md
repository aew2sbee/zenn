---
title: "[React]ローカル環境にTypeScriptで動かす環境を構築" # 記事のタイトル
emoji: "❄" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["react", "typescript", "nodejs"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

社内の有志メンバー向けにローカル環境で React を動かす資料が必要だったので執筆します。
今回は、下記画像のようにローカル環境で React が実行できるように作業を行います。
![React_step6](/images/React_step6.png)

## 1. Node.js をローカル環境にインストールする

:::message alert
本業で`Node.js`を利用している方は、下記の作業は行わないでください。<br>本業の作業に影響が出る可能性があります。
:::

> 【Node.js とは】<br>JavaScript を使ってサーバーサイド（バックエンド）のアプリケーションを作成するためのプラットフォームです。<br>これにより、従来はクライアントサイドでしか使えなかった JavaScript を、サーバーサイドでも使えるようになります。

> 【なぜ、Node.js が必要なの？】<br>Node.js とそのパッケージマネージャーである npm（または Yarn）が非常に便利です。

1. 下記サイトにアクセスする
   @[card](https://nodejs.org/ja)
2. 推奨版の`18.16.1`をインストールします。(2023/07 時点)
   ![sandbooks-react-step01](/images/sandbooks-react-step01.png)
3. インストーラーがダウンロードされたことを確認する
   ![sandbooks-react-step02](/images/sandbooks-react-step02.png)
4. ダウンロードしたインストーラーを実行する
   ![sandbooks-react-step03](/images/sandbooks-react-step03.png)

5. `Next`をクリックし事前設定を確認します。
   ![sandbooks-nodejs-step01](/images/sandbooks-nodejs-step01.png)

6. ライセンスの同意に ✅ を付けて、`Next`をクリックする
   ![sandbooks-nodejs-step02](/images/sandbooks-nodejs-step02.png)

7. プログラムの保存先を`デフォルト`のままで`Next`をクリックする
   ![sandbooks-nodejs-step03](/images/sandbooks-nodejs-step03.png)

8. 特に操作もせず、`Next`をクリックする
   ![sandbooks-nodejs-step04](/images/sandbooks-nodejs-step04.png)

9. 特に操作もせず、`Next`をクリックする
   自動で必要なツールをインストールしますか？と聞かれていますが、余分なものを取り込みたくないので ✅ を付けません。
   ![sandbooks-nodejs-step05](/images/sandbooks-nodejs-step05.png)

10. `Install`をクリックしインストールを開始します。
    ![sandbooks-nodejs-step06](/images/sandbooks-nodejs-step06.png)

11. インストールが完了するまで待ちます。
    ![sandbooks-nodejs-step07](/images/sandbooks-nodejs-step07.png)

12. インストールの完了を確認して`Finish`をクリックします。
    ![sandbooks-nodejs-step08](/images/sandbooks-nodejs-step08.png)

## 2. Node.js の version が`18.16.1`のか確認する

1. Git Bash を起動する
   検索欄に`git bash`と検索し、**Git Bash**アプリを起動する
   ![sandbooks-git-step01](/images/sandbooks-git-step01.png)

2. 作業する場所に移動する
   下記コマンドは、ホームディレクトリに一旦移動し、`Work`配下にあるクローンした`sandbooks`に移動します。
   `bash
cd ~
cd Work/sandbooks
`

3. 下記コマンドを実行し version を確認する

   ```bash
   node --version
   ```

4. version が`18.16.1`である事を確認する
   ```bash
   $ node --version
   v18.16.1
   ```

## 3. React アプリを作成し、起動する

1. `src`ディレクトリを作成し、移動する
   ※既に作成済みなら、対応不要です。

   ```bash
   mkdir src
   cd src
   ```

2. `yarn`を利用できるようにインストール

   ```bash
   npm install -g yarn
   ```

   > 【yarn とは】
   > Yarn は JavaScript のパッケージマネージャーで、プロジェクトで使用するコードやライブラリを管理するためのツールです。<br><br>パッケージマネージャーは、他の人が作ったコードを使う際に必要なファイルや依存関係を管理し、効率的にインストールしたりアップデートしたりする役割を果たします。

   > 【yarn のメリットは？】<br>1. **パフォーマンス**: Yarn はパッケージのインストールやアップデートを高速化します。依存関係の解決やダウンロードを並列で行い、パッケージをキャッシュして再利用することで時間を節約します。<br><br>2. **信頼性**: Yarn は厳密な依存関係の管理を行います。パッケージのバージョンが変更されないようにロックファイルを作成し、他の開発者が同じ環境で同じバージョンのパッケージを利用できるようにします。<br><br>3. **オフラインモード**: インターネットに接続できない場合でも、Yarn は以前にダウンロードしたパッケージを利用することができます。これは、移動中やネットワークの遅い場所でも作業を続けることができます。<br><br>4. **セキュリティ**: Yarn はパッケージのセキュリティを重視しています。依存関係のパッケージが信頼できるものであるかどうかをチェックし、既知のセキュリティの問題がある場合に警告を出すことができます。

3. React のアプリを作成する

   ```bash
   yarn create react-app frontend --template typescript
   ```

   :::message
   create するまでに少し時間がかかりますが、完了するまで放置します。
   :::

   > 【上記のコマンドの意味について】
   > **yarn create react-app**：yarn コマンドで React の Web アプリを作成する
   > **frontend**：Web アプリの名前
   > **--template typescript**：typescript で Web アプリを構築する

4. 作成した React のアプリに移動します。

   ```bash
   cd frontend
   ```

5. React を起動する
   下記コマンドを実行して React を起動します
   ```bash
   yarn start
   ```
6. ローカルホストにアクセスする
   `http://localhost:3000/`をブラウザーの URL に入力して Enter キーを押します。
   ![React_step6](/images/React_step6.png)
   > 【ローカルホスト（Localhost）とは】<br>自分自身のコンピュータ上で実行されているネットワークサーバーを指す言葉です。<br><br>例えば、ウェブ開発の場合、ローカルホストは自分のコンピュータ上で実行されるウェブサーバーを指します。ローカルホストを利用することで、ウェブサイトの動作をブラウザで確認したり、データベースとの連携をテストしたりすることができます。
