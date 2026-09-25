---
title: "[AWS] IAMで読み取り専用ユーザーを作成する" # 記事のタイトル
emoji: "☁️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["aws", "iam", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

社内の有志メンバーによる活動で、IAM の読み取り専用ユーザーを作成する機会がありました。
何度も経験することではないと思いますので、備忘録として執筆します。

メンバーに設定を変更されるリスクなく AWS の構成を見てもらいたい場合に、読み取り専用ユーザーが役立ちます。

| 項目 | 内容 |
| --- | --- |
| **対象者** | ・AWS の IAM について知らない方 |
| **伝えたい内容** | ・IAM で読み取り専用ユーザーを作成し、初回サインインまで行う |
| **前提条件** | ・AWS アカウントを作成済みで、マネジメントコンソールにルートユーザーでサインインしていること |

:::message
本記事のスクリーンショットは執筆時点のものです。
AWS マネジメントコンソールの画面や項目名は変更されることがあるため、実際の画面と異なる場合があります。
:::

## 🌱 IAM とは

IAM（Identity and Access Management）は、AWS の「誰が・どのサービスに・何をできるか」を管理する仕組みです。
AWS アカウントに利用者ごとのユーザーを作り、必要な権限だけを与えられます。

- **ルートユーザー**: AWS アカウント作成時に自動で用意される、すべての操作ができる最上位のユーザー
- **IAM ユーザー**: アカウント内に作成する、権限を絞れるユーザー

ルートユーザーは何でもできてしまうため、日常の作業は権限を絞った IAM ユーザーで行うことが推奨されています。

権限は**ポリシー**（どのサービスに対して何ができるかを定義した設定）で与えます。
ポリシーをユーザーに紐づけることを「**アタッチする**」と呼びます。

今回使う `ReadOnlyAccess` は AWS があらかじめ用意しているポリシー（AWS 管理ポリシー）で、
各サービスの情報を「見る」ことはできますが、「作成・変更・削除」はできません。

:::message alert
`ReadOnlyAccess` は、S3 のオブジェクトなど**データの中身の読み取りも許可**します。
「読み取り専用だから何を見せても安全」とは限らないため、機密データを含む環境では注意してください。
一覧や設定の閲覧だけで十分な場合は `ViewOnlyAccess` も検討できます。

また、AWS 管理ポリシーは AWS 側で更新され、対象サービスやアクションが増えることがあります。
:::

## 🌱 IAM ユーザーを作成する

1. IAM を開く
   検索欄に`IAM`と入力し、検索結果から**IAM**をクリックします。
   ![AWSマネジメントコンソールの検索欄でIAMを検索した画面](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step01.png)

2. 新規ユーザー作成画面を開く
   左側の`ユーザー`を選択し、右側にある青色のボタン`ユーザーを追加`をクリックします。
   ![IAMのユーザー一覧画面](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step02.png)

3. ユーザー名を入力します（任意の名前で構いません。本記事ではサンプルとして`test_user`を使用します）。
4. `AWS マネジメントコンソールへのユーザーアクセスを提供する`に ✅ を付けます。
   ブラウザからサインインできるようにする設定です。
5. コンソールパスワードは`自動生成されたパスワード`を選びます。
6. `ユーザーは次回のサインイン時に新しいパスワードを作成する必要があります`に ✅ が付いていることを確認します。
   :::message
   この設定により、後述の手順で初回サインイン時にパスワード変更画面が表示されます。
   ✅ を外すとパスワード変更画面は表示されません。
   :::
7. `次へ`ボタンをクリックします。
   ![ユーザーの詳細を指定する画面](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step03.png)

8. `ポリシーを直接アタッチする`を選択します。
   :::message
   今回は 1 ユーザーのため直接アタッチしますが、複数ユーザーを運用する場合はユーザーグループ経由での権限管理が推奨されています。
   :::
   ![許可のオプションを選択する画面](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step04.png)

9. 検索欄に`ReadOnlyAccess`と入力し、検索結果に表示された`ReadOnlyAccess`に ✅ を付けます。
   :::message
   `AmazonEC2ReadOnlyAccess`や`IAMReadOnlyAccess`など、名前の似たポリシーが多数ヒットします。
   名前が完全一致する`ReadOnlyAccess`を選んでください。
   :::
10. `次へ`ボタンをクリックします。
    ![許可ポリシーでReadOnlyAccessを選択した画面](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step05.png)

11. 下記の項目が指定したとおりになっているか確認します。
    - ユーザー名が指定した名前である
    - **許可の概要**に`ReadOnlyAccess`が含まれている
    :::message
    手順 6 に ✅ を付けた場合、`IAMUserChangePassword`ポリシーも併せて付与されます。
    これはユーザー自身がパスワードを変更するために必要な権限です。
    :::
12. `ユーザーの作成`をクリックします。
    ![設定内容を確認して作成する画面](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step06.png)

13. `ユーザーが正常に作成されました`のメッセージを確認します。
14. `.csv ファイルをダウンロード`をクリックし、サインイン情報を控えます。
    :::message alert
    .csv には**ユーザー名・初期パスワード・コンソールサインイン URL**が記載されています。
    初期パスワードはこの画面を離れると再表示できないため、必ずこのタイミングでダウンロードしてください。

    また、.csv には初期パスワードが平文で記載されています。
    この後の手順で初回サインインとパスワード変更を行いますので、完了したら速やかに削除してください。
    :::
15. `ユーザーリストに戻る`をクリックします。
    ![パスワードを取得する画面](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step07.png)

## 🌱 作成したユーザーでサインインを確認する

:::message
ルートユーザーのセッションを維持したい場合は、サインアウトせずにシークレットウィンドウでも確認できます。
:::

1. 画面右上のアカウントメニューから`サインアウト`をクリックします。
   :::message
   このとき、アカウントメニューに表示される**アカウント ID（12 桁の数字）**を控えておくと、後の手順で入力できます。
   手順 14 でダウンロードした .csv のコンソールサインイン URL を開けば、アカウント ID は自動で入力されます。
   :::
   ![アカウントメニューからサインアウトする画面](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step08.png)

2. `もう一度ログインする`をクリックします。
   ![サインアウト後の画面](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step09.png)

3. サインイン画面で`IAM ユーザー`を選択し、アカウント ID（またはエイリアス）・ユーザー名・控えておいた初期パスワードを入力してサインインします。
   ![IAMユーザーとしてサインインする画面](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step10.png)

4. 初回サインイン時に表示されるパスワード変更画面で、新しいパスワードを設定します。
   :::message
   パスワードはアカウントのパスワードポリシー（既定では 8 文字以上）を満たす必要があります。
   :::
   ![初回サインイン時のパスワード変更画面](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step11.png)

パスワード変更後、マネジメントコンソールのホーム画面が表示されれば成功です。
リソースの作成や削除を試すと権限エラーになることで、読み取り専用になっていることを確認できます。

## 🌱 おわりに

本記事でやったことは次の 3 点です。

- IAM ユーザーを作成し、`ReadOnlyAccess`ポリシーをアタッチした
- 初期パスワードを .csv で受け取った
- 作成したユーザーでサインインし、パスワードを変更した

運用する場合は、次の点もあわせて検討してください。

- 作成した IAM ユーザーに **MFA（多要素認証）** を設定する
- 控えた .csv ファイルを削除する
- ルートユーザーは初期設定など必要最小限の場面に限って使う
- ユーザーが増えたら、ユーザーグループでの権限管理に切り替える

## 🌱 参考

https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_users_create.html
https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/best-practices.html
https://docs.aws.amazon.com/aws-managed-policy/latest/reference/ReadOnlyAccess.html
https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_users_sign-in.html
https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_credentials_mfa.html
