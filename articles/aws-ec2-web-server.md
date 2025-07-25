---
title: '[AWS] EC2でWebサーバーを構築する' # 記事のタイトル
emoji: '☁️' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['aws', 'ec2', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

AWS 上で Web サーバーを構築する方法を学習しましたので執筆します。

| 項目             | 内容                                    |
| ---------------- | --------------------------------------- |
| **対象者**       | ・AWS の EC2 について知らない方         |
| **伝えたい内容** | ・EC2 で Web サーバーを起動したい       |
| **前提条件**     | ・AWS のアカウント作成済み<br>・Windows |

### 結論

:::message
[手順]

1. キーペアーの作成
2. プライベートキーをローカルに配置
3. サーバーの設定
4. サーバーの起動/停止

:::

- **Q: AWS の EC2 とは？**
  - A: EC2（Elastic Compute Cloud）は、仮想サーバーを簡単に作成・起動・管理することができます。
- **Q: EC2 のメリットは？**
  - A: 下記のメリットです。
  1. **需要に応じてインスタンスの数やタイプを柔軟に調整する**ことができます。需要が高まった場合は、追加のインスタンスを簡単に起動して負荷を分散できます。また、異なるインスタンスタイプを使用してアプリケーションを最適化することも可能です。
  2. 実際に使用したリソースに対してのみ料金が発生します。**インスタンスの起動と停止を自由に行うことができ、必要な時にのみ課金されます。** これにより、コストを最適化し、無駄な支出を避けることができます。
  3. さまざまなインスタンスタイプを提供しています。これには、一般的な汎用タイプから、メモリ重視、コンピュート重視、GPU インスタンスなど、さまざまな用途に特化したタイプがあります。これにより、**アプリケーションの要件に最適なインスタンス**を選択できます。
  4. 複数のアベイラビリティーゾーン（Availability Zone）にインスタンスを配置することができます。これにより、システムの可用性が向上し、**障害が発生した場合でもサービスの継続性を確保する**ことができます。また、Amazon Machine Image（AMI）やスナップショットなどのバックアップ機能も利用できます。
  5. AWS が提供するセキュリティグループ、ネットワークアクセス制御リスト（Network ACL）、仮想プライベートクラウド（VPC）などのセキュリティ機能を活用することができます。また、AWS Identity and Access Management（IAM）を使用して、適切なアクセス制御を実施することができます。さらに、AWS はセキュリティとコンプライアンスに関するさまざまな認証と規制を取得しており、**信頼性の高い環境**を提供しています。

## キーペアーの作成

### 1. EC2 のページに移動する

画面左上の`サービス>コンピューティング>EC2`をクリックし、EC2 のページに移動します。
![ec2-step1](/images/ec2-step1.png)

### 2. キーペアのページに移動する

リソースの`キーペア`をクリックします。
![ec2-step2](/images/ec2-step2.png)

### 3. キーペアを作成する

画面右上の`キーペアを作成`をクリックします
![ec2-step3](/images/ec2-step3.png)

### 4. キーペアを設定

下記の項目を設定します。

1. 名前 →`任意の名前`
2. キーペアのタイプ →`RSA`
3. プライベートキーファイル形式 →`.pem`
   ![ec2-step4](/images/ec2-step4.png)

- **Q: RSA (Rivest Shamir Adleman)とは？**
  - A: 非常に大きな数字を素因数分解する事が困難な事を利用した暗号アルゴリズム
- **Q: .pem とは？**
  - A: 証明書や鍵の情報を記載した拡張子

### 5. 作成したキーペアを確認する

問題がなければ、`キーペアが正常に作成されました。`とポップアップが表示され、
自動でダウンロードされる事を確認します。
![ec2-step5](/images/ec2-step5.png)

## プライベートキーをローカルに配置

### 1. 作成したプライベートキーを.ssh に配置する

`C:/Users/[user_name]/`配下に`.ssh`というディレクトリを作成し、
その中に先ほど作成しダウンロードした`プライベートキー`を配置します
![ec2-step6](/images/ec2-step6.png)

- **Q: 公開鍵暗号方式とは？**
  - A: 暗号化と復号に異なる鍵（公開鍵と秘密鍵）の組み合わせを使用する暗号方式です。
- **Q: パブリックキー(公開鍵)とは？**
  - A: 公開鍵暗号方式で使用される鍵の一部です。
- **Q: プライベートキー(秘密鍵)とは？**
  - A: 公開鍵暗号方式で使用される鍵の一部であり、その鍵を所有する個人またはエンティティだけが知っている情報です。

## サーバーの設定

### 1. インスタンスを起動するページに移動する

EC2 ダッシュボードから`インスタンスを起動`をクリックする
![ec2-step7](/images/ec2-step7.png)

### 2. サーバー名を設定する

今回は、`server_for_study`という名前のサーバー名にします
![ec2-step8](/images/ec2-step8.png)

### 3. OS を設定する

今回は、`Ubuntu server 22.04`を選択します。
![ec2-step9](/images/ec2-step9.png)

### 4. インスタンスを起動するページに移動する

`t2.micro`の無料利用枠を設定します
![ec2-step10](/images/ec2-step10.png)

### 5. キーペアの設定する

先ほど作成した`キーペア`を選択します。
![ec2-step11](/images/ec2-step11.png)

### 6. ネットワークの設定する

Web アプリで HTTP/HTTPS 通信を可能にする為に、下記項目にチェックを付けます。

- インターネットからの HTTPS トラフィックを許可
- インターネットからの HTTP トラフィックを許可
  ![ec2-step12](/images/ec2-step12.png)

### 7. ストレージの設定する

今回は、勉強の為（料金を抑える為に）なので`8GiB`で設定します。
![ec2-step13](/images/ec2-step13.png)

## サーバーの起動/停止

### 1. インスタンスを起動する

各種設定が完了出来たので、`インスタンスを起動`をクリックします
![ec2-step14](/images/ec2-step14.png)
ロード画面後に`成功`のポップアップが表示があれば OK です！
![ec2-step15](/images/ec2-step15.png)

### 2. インスタンスを停止する

料金を発生しないためにサーバーを停止するには、
対象のサーバーにチェックを付け、`インスタンスの状態`をインスタンスを停止"を選択します。
![ec2-step16](/images/ec2-step16.png)
ポップアップの停止ボタンをクリックすると、少し時間を経過すると
インスタンスの状態が`停止済み`に変更されます
![ec2-step17](/images/ec2-step17.png)

:::message alert
**インスタンスを起動後にサーバーが起動する=料金の発生する!!**
**必要な時だけ起動してください!**

過去に知らずにサーバー起動を放置(2 カ月)して 5 千円ほど料金が発生した経験があります。。
:::

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
