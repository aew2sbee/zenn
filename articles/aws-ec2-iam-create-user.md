---
title: '[AWS] IAMで読み取り専用ユーザーを作成する' # 記事のタイトル
emoji: '🤺' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['aws', 'iam', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

社内の有志メンバーでの活動で IAM で読み取り専用ユーザー作成する機会がありました。
この経験は、何度もある事ではないと思いますので、備忘録として執筆します。

| 項目             | 内容                                   |
| ---------------- | -------------------------------------- |
| **対象者**       | ・AWS の IAM について知らない方        |
| **伝えたい内容** | ・IAM で読み取り専用ユーザーを作成する |
| **前提条件**     | ・AWS のルートユーザー作成済み         |

## IAM ユーザーを作成する

1. IAM を開く
   **ルートユーザー**で検索欄に`IAM`と検索し検索結果から**IAM**をクリックします。
   ![sandbooks-aws-IAM-step01](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step01.png)

2. 新規ユーザー作成画面を開く
   左側の`ユーザー`を選択し、右側の青色ボタンの`ユーザーを追加`をクリックします。
   ![sandbooks-aws-IAM-step02](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step02.png)

3. ユーザー名を入力します
   ※任意の名前を入力します。今回は、`test_user`とします
4. `AWSマネジメントコンソールへのユーザーアクセスを提供する`に ✅ を付けます。
5. `次へ`ボタンをクリックします。
   ![sandbooks-aws-IAM-step03](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step03.png)

6. `ポリシーを直接アタッチする`を選択する
   ![sandbooks-aws-IAM-step04](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step04.png)
7. `ReadOnlyAccess`を検索欄に入力する
8. `8ページ`まで移動する
9. `ReadOnlyAccess`に ✅ を付けます。
10. `次へ`ボタンをクリックします。
    ![sandbooks-aws-IAM-step05](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step05.png)

11. 下記項目が指定した値であるか確認する
    - ユーザー名が指定した名前である
    - **許可の概要**に`ReadOnlyAccess`が含まれている
12. `ユーザーの作成`をクリックする
    ![sandbooks-aws-IAM-step06](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step06.png)

13. `ユーザーが正常に作成されました`のメッセージを確認する
14. `.csvファイルをダウンロード`をクリックしログイン情報を控える
15. `ユーザーリストに戻る`をクリックします。
    ![sandbooks-aws-IAM-step07](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step07.png)
    ![sandbooks-aws-IAM-step08](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step08.png)
    ![sandbooks-aws-IAM-step09](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step09.png)
    ![sandbooks-aws-IAM-step10](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step10.png)
    ![sandbooks-aws-IAM-step11](/images/articles/aws-ec2-iam-create-user/sandbooks-aws-IAM-step11.png)

## おわりに

あとは、新しい作成したユーザーでログインし、初期パスワードの変更を行えば完了です。
