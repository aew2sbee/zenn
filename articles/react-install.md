---
title: "[React]ローカル環境でTypeScriptのReactを動かす環境を構築" # 記事のタイトル
emoji: "❄️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["react", "typescript", "nodejs"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

社内の有志メンバー向けにローカル環境で React を動かす資料が必要だったので執筆します。
今回は、下記画像のようにローカル環境で React が実行できるように作業を行います。
![React_step6](/images/articles/react-install/React_step6.png)

:::message alert
本記事は、2023年7月時点の手順です。
- 本記事で使っている Create React App は、2025年2月に React 公式で非推奨になりました。新しく作る場合は、Vite（例: `npm create vite@latest frontend -- --template react-ts`）や Next.js などを使ってください。
- Node.js 18 はサポートが終了しています。インストールする場合は、公式サイトで推奨されている最新の LTS 版を選んでください。
:::

## 🌱 1. Node.js をローカル環境にインストールする

:::message alert
本業で`Node.js`を利用している方は、下記の作業は行わないでください。<br>本業の作業に影響が出る可能性があります。
:::

> 【Node.js とは】<br>ブラウザの外で JavaScript を実行できる環境（ランタイム）です。<br>サーバーサイドのアプリケーションだけでなく、開発ツールやスクリプトの実行にも使われます。

> 【なぜ、Node.js が必要なの？】<br>React の開発では、Node.js と、そのパッケージマネージャーである npm（または Yarn）を使ってパッケージをインストールしたり、開発サーバーを起動したりするためです。

1. 下記サイトにアクセスする

   @[card](https://nodejs.org/ja)

2. 推奨版の`18.16.1`をインストールします。（2023/07 時点）
   ![sandbooks-react-step01](/images/articles/react-install/sandbooks-react-step01.png)
3. インストーラーがダウンロードされたことを確認する
   ![sandbooks-react-step02](/images/articles/react-install/sandbooks-react-step02.png)
4. ダウンロードしたインストーラーを実行する
   ![sandbooks-react-step03](/images/articles/react-install/sandbooks-react-step03.png)

5. `Next`をクリックし事前設定を確認します。
   ![sandbooks-nodejs-step01](/images/articles/react-install/sandbooks-nodejs-step01.png)

6. ライセンスの同意に ✅ を付けて、`Next`をクリックする
   ![sandbooks-nodejs-step02](/images/articles/react-install/sandbooks-nodejs-step02.png)

7. プログラムの保存先を`デフォルト`のままで`Next`をクリックする
   ![sandbooks-nodejs-step03](/images/articles/react-install/sandbooks-nodejs-step03.png)

8. 特に操作もせず、`Next`をクリックする
   ![sandbooks-nodejs-step04](/images/articles/react-install/sandbooks-nodejs-step04.png)

9. ✅ を付けずに、`Next`をクリックする
   ネイティブモジュールのビルドに必要なツールを自動でインストールするか聞かれていますが、今回は不要なので ✅ を付けません。
   ![sandbooks-nodejs-step05](/images/articles/react-install/sandbooks-nodejs-step05.png)

10. `Install`をクリックしインストールを開始します。
    ![sandbooks-nodejs-step06](/images/articles/react-install/sandbooks-nodejs-step06.png)

11. インストールが完了するまで待ちます。
    ![sandbooks-nodejs-step07](/images/articles/react-install/sandbooks-nodejs-step07.png)

12. インストールの完了を確認して`Finish`をクリックします。
    ![sandbooks-nodejs-step08](/images/articles/react-install/sandbooks-nodejs-step08.png)

## 🌱 2. Node.js のバージョンが`18.16.1`であるか確認する

1. Git Bash を起動する
検索欄に`git bash`と検索し、**Git Bash**アプリを起動する

2. 作業する場所に移動する
下記コマンドは、ホームディレクトリに一旦移動し、`Work`配下にあるクローンした`sandbooks`に移動します。
（社内向けのリポジトリです。読者の方は、任意の作業用ディレクトリに移動してください）
```bash
cd ~
cd Work/sandbooks
```

3. 下記コマンドを実行しバージョンを確認する

```bash
node --version
```

4. バージョンが`18.16.1`であることを確認する
```bash
$ node --version
v18.16.1
```

## 🌱 3. React アプリを作成し、起動する

1. `src`ディレクトリを作成し、移動する
※既に作成済みなら、対応不要です。

```bash
mkdir src
cd src
```

2. Yarn をインストールする

```bash
npm install -g yarn
```

> 【yarn とは】
> Yarn は JavaScript のパッケージマネージャーで、プロジェクトで使用するコードやライブラリを管理するためのツールです。<br><br>パッケージマネージャーは、他の人が作ったコードを使う際に必要なファイルや依存関係を管理し、効率的にインストールしたりアップデートしたりする役割を果たします。

> 【yarn のメリットは？】<br>1. **パフォーマンス**: Yarn はパッケージのインストールやアップデートを高速化します。依存関係の解決やダウンロードを並列で行い、パッケージをキャッシュして再利用することで時間を節約します。<br><br>2. **信頼性**: Yarn は厳密な依存関係の管理を行います。パッケージのバージョンが変更されないようにロックファイルを作成し、他の開発者が同じ環境で同じバージョンのパッケージを利用できるようにします。<br><br>3. **キャッシュの再利用**: 以前にダウンロードしたパッケージをキャッシュから再利用できます（`--offline`オプションを使えば、キャッシュ済みのパッケージだけでインストールすることもできます）。<br><br>4. **セキュリティ**: ロックファイルのチェックサムでパッケージの改ざんを検知できます。また、`yarn audit`で既知の脆弱性を確認できます。<br><br>なお、現在の npm にも同様の機能があります。

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
> **--template typescript**：TypeScript で Web アプリを構築する

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
`http://localhost:3000/`をブラウザの URL に入力して Enter キーを押します。
![React_step6](/images/articles/react-install/React_step6.png)
> 【ローカルホスト（Localhost）とは】<br>自分自身のコンピュータ上で実行されているネットワークサーバーを指す言葉です。<br><br>例えば、ウェブ開発の場合、ローカルホストは自分のコンピュータ上で実行されるウェブサーバーを指します。ローカルホストを利用することで、ウェブサイトの動作をブラウザで確認したり、データベースとの連携をテストしたりすることができます。
