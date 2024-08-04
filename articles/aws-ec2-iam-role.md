---
title: '[AWS]IAMで読み取り専用ユーザー本人がMFAの設定が出来るようにする方法' # 記事のタイトル
emoji: '👨‍🏭' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['aws', 'iam', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

社内の有志メンバーでの活動で本人が MFA の設定をする機会がありました。
この経験は、何度もある事ではないと思いますので、備忘録として執筆します。

## `ReadOnlyAccess`と`IAMUserChangePassword`のポリシーだけでは設定できない

`ReadOnlyAccess`のユーザーを作成後に MFA の認証を行うとしましたが、下記のエラーメッセージが表示されました。
どうやら、「権限がありません」と記載されています。
なので、ポリシーで追加する必要がありそうです。
:::message alert
User: arn:aws:iam::XXXXXXXXXXXX:user/test_user is not authorized to perform: iam:CreateVirtualMFADevice on resource: arn:aws:iam::XXXXXXXXXXXX:mfa/deviceName because no identity-based policy allows the iam:CreateVirtualMFADevice action
:::
![sandbooks-aws-IAM-MFA-step00](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step00.png)

## ユーザー本人で MFA の設定可能にする

### 1. 本人が MFA の設定できるポリシーを作成する

1. 左側の`ポリシー`をクリックする
2. `ポリシーを作成`をクリックする
   ![sandbooks-aws-IAM-MFA-step01](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step01.png)

3. 右側の`JSON`をクリックする
4. `ポリシーエディタ`に下記 JSON をコピペする
5. `次へ`をクリックする
   ![sandbooks-aws-IAM-MFA-step02](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step02.png)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowViewAccountInfo",
      "Effect": "Allow",
      "Action": ["iam:GetAccountPasswordPolicy", "iam:ListVirtualMFADevices"],
      "Resource": "*"
    },
    {
      "Sid": "AllowManageOwnPasswords",
      "Effect": "Allow",
      "Action": ["iam:ChangePassword", "iam:GetUser"],
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    },
    {
      "Sid": "AllowManageOwnAccessKeys",
      "Effect": "Allow",
      "Action": [
        "iam:CreateAccessKey",
        "iam:DeleteAccessKey",
        "iam:ListAccessKeys",
        "iam:UpdateAccessKey"
      ],
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    },
    {
      "Sid": "AllowManageOwnSigningCertificates",
      "Effect": "Allow",
      "Action": [
        "iam:DeleteSigningCertificate",
        "iam:ListSigningCertificates",
        "iam:UpdateSigningCertificate",
        "iam:UploadSigningCertificate"
      ],
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    },
    {
      "Sid": "AllowManageOwnSSHPublicKeys",
      "Effect": "Allow",
      "Action": [
        "iam:DeleteSSHPublicKey",
        "iam:GetSSHPublicKey",
        "iam:ListSSHPublicKeys",
        "iam:UpdateSSHPublicKey",
        "iam:UploadSSHPublicKey"
      ],
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    },
    {
      "Sid": "AllowManageOwnGitCredentials",
      "Effect": "Allow",
      "Action": [
        "iam:CreateServiceSpecificCredential",
        "iam:DeleteServiceSpecificCredential",
        "iam:ListServiceSpecificCredentials",
        "iam:ResetServiceSpecificCredential",
        "iam:UpdateServiceSpecificCredential"
      ],
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    },
    {
      "Sid": "AllowManageOwnVirtualMFADevice",
      "Effect": "Allow",
      "Action": ["iam:CreateVirtualMFADevice", "iam:DeleteVirtualMFADevice"],
      "Resource": "arn:aws:iam::*:mfa/*"
    },
    {
      "Sid": "AllowManageOwnUserMFA",
      "Effect": "Allow",
      "Action": [
        "iam:DeactivateMFADevice",
        "iam:EnableMFADevice",
        "iam:ListMFADevices",
        "iam:ResyncMFADevice"
      ],
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    },
    {
      "Sid": "DenyAllExceptListedIfNoMFA",
      "Effect": "Deny",
      "NotAction": [
        "iam:CreateVirtualMFADevice",
        "iam:EnableMFADevice",
        "iam:GetUser",
        "iam:ListMFADevices",
        "iam:ListVirtualMFADevices",
        "iam:ResyncMFADevice",
        "sts:GetSessionToken"
      ],
      "Resource": "*",
      "Condition": {
        "BoolIfExists": {
          "aws:MultiFactorAuthPresent": "false"
        }
      }
    }
  ]
}
```

#### 補足情報

| 単語                                  | 意味                                                                                                                                                                                                                                                                  |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **AllowViewAccountInfo**              | IAM ユーザーがアカウント情報を表示するために許可されているアクションが定義されています。具体的には、アカウントのパスワードポリシーを取得したり、アカウントのサマリー情報を取得したり、仮想多要素認証（MFA）デバイスのリストを取得することができます                   |
| **AllowManageOwnPasswords**           | IAM ユーザーが自分自身のパスワードを管理するために許可されているアクションが定義されています。具体的には、パスワードの変更、自分自身のログインプロファイルの作成と削除、ログインプロファイルの取得と更新が許可されています                                            |
| **AllowManageOwnAccessKeys**          | IAM ユーザーが自分自身のアクセスキーを管理するために許可されているアクションが定義されています。具体的には、アクセスキーの作成、削除、リストの表示、アクセスキーの更新が許可されています                                                                              |
| **AllowManageOwnSigningCertificates** | IAM ユーザーが自分自身の署名証明書を管理するために許可されているアクションが定義されています。具体的には、署名証明書の削除、リストの表示、署名証明書の更新とアップロードが許可されています                                                                            |
| **AllowManageOwnSSHPublicKeys**       | IAM ユーザーが自分自身の SSH 公開鍵を管理するために許可されているアクションが定義されています。具体的には、SSH 公開鍵の削除、取得、リストの表示、SSH 公開鍵の更新とアップロードが許可されています                                                                     |
| **AllowManageOwnGitCredentials**      | IAM ユーザーが自分自身の Git クレデンシャルを管理するために許可されているアクションが定義されています。具体的には、サービス固有のクレデンシャルの作成、削除、リストの表示、サービス固有のクレデンシャルのリセットと更新が許可されています。                           |
| **AllowManageOwnVirtualMFADevice**    | IAM ユーザーが自分自身の仮想多要素認証（MFA）デバイスを管理するために許可されているアクションが定義されています。具体的には、仮想 MFA デバイスの作成と削除が許可されています                                                                                          |
| **AllowManageOwnUserMFA**             | IAM ユーザーが自分自身の MFA デバイスを管理するために許可されているアクションが定義されています。具体的には、MFA デバイスの無効化と有効化、MFA デバイスのリストの表示、MFA デバイスの再同期が許可されています                                                         |
| **DenyAllExceptListedIfNoMFA**        | MFA が無効化されている場合に、ユーザーに許可されるアクションを制限する条件が定義されています。MFA が無効な場合、特定のアクション（仮想 MFA デバイスの作成、MFA デバイスの有効化、パスワード変更など）に対してのみ許可され、それ以外のアクションに対しては拒否されます |

### 3. 任意のポリシーを設定する

1. `ポリシー名`の欄に任意の名前を入力します。
   ※今回は、`test-MFA`という名前で作成します。
   ![sandbooks-aws-IAM-MFA-step03](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step03.png)
2. `許可`の欄に`IAM`がある事を確認する
   ※先ほどの JSON で`IAM`の内容の設定が反映されました。
3. 内容を確認し、`ポリシーの作成`をクリックする
   ![sandbooks-aws-IAM-MFA-step04](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step04.png)

### 4. 成功メッセージを確認する

1. 画面上部に`ポリシー test-MFAが作成されました。`を確認する
2. 任意で設定したポリシー名の`test-MFA`が一覧で確認出来ます。
   ![sandbooks-aws-IAM-MFA-step05](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step05.png)

### 5. 対象ユーザーに作成したポリシーを付与する

1. 左側の`ユーザー`をクリックする
2. 対象のユーザーをクリックする
   ※今回は、`test-user`を選択します。
   ![sandbooks-aws-IAM-MFA-step06](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step06.png)

3. `許可を追加`の右横の ▲ をクリックし、`許可を追加`をクリックする
   ![sandbooks-aws-IAM-MFA-step07](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step07.png)

4. `ポリシーを直接アタッチする`を選択する
5. 検索欄にキーワードを入力し調べやすくします。
   ※`test-MFA`だったので`test`というキーワードで検索しています。
6. 先ほど作成した`test-MFA`に ✅ を付ける
7. `次へ`をクリックする
   ![sandbooks-aws-IAM-MFA-step08](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step08.png)
8. 対象ユーザーと付与するポリシーを確認する
9. `許可を追加`をクリックする
   ![sandbooks-aws-IAM-MFA-step09](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step09.png)
10. `ポリシーが追加されました`という成功メッセージを確認する
11. ポリシーの一覧に`test-MFA`が追加されていることを確認する
    ![sandbooks-aws-IAM-MFA-step10](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step10.png)

### 6. 本人が MFA 設定を行う

1. 対象のユーザーでログインをする
   ![sandbooks-aws-IAM-step10](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-step10.png)
2. 左側の`ダッシュボード`から`MFAを追加`をクリックする
   ![sandbooks-aws-IAM-MFA-step11](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step11.png)
3. 少しスクロールし`多要素認証(MFA)`を見つけ、`MFAデバイスの割り当て`をクリックする
   ![sandbooks-aws-IAM-MFA-step12](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step12.png)
4. 登録するデバイス名を`デバイス名`の入力欄に記入する
   :::message alert
   【注意１】
   他のメンバーと同じデバイス名を登録する事が出来ません。
   **NG** : A さん=iPhone, B さん=iPhone
   **OK** : A さん=iPhone-A, B さん=iPhone-B
   :::
   :::message alert
   【注意２】
   一度入力したデバイス名は、登録してなくても利用する事が出来ません。
   :::
5. `認証アプリケーション`を選択する
6. `次へ`をクリックする
   ![sandbooks-aws-IAM-MFA-step13](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step13.png)

7. `認証アプリケーション`をスマホにダウンロードする
   :::message
   公式による認証アプリケーションは下記参照です。
   @[card](https://aws.amazon.com/jp/iam/features/mfa/?audit=2019q1)
   :::
   自分は下記を利用しました。
   @[card](https://www.microsoft.com/ja-jp/security/mobile-authenticator-app)
   ![sandbooks-aws-IAM-MFA-step14](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step14.png)
8. `QRコードを表示`をクリックし、QR コードさせる
9. 先ほどインストールした`認証アプリケーション`QR コードを読み込む
   ![sandbooks-aws-IAM-MFA-step15](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step15.png)
10. スマホの画面に 30 秒に 1 回 6 桁の数字が表示されるので、連続で 6 桁の数字を 2 回分入力する
11. `MFAを追加`をクリックする
    ![sandbooks-aws-IAM-MFA-step16](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step16.png)
12. 成功メッセージを確認する
    ![sandbooks-aws-IAM-MFA-step17](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step17.png)
13. `多要素認証(MFA)`の一覧に先ほどのデバイス名が登録させている事を確認する
    ![sandbooks-aws-IAM-MFA-step18](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step18.png)

### 7. MFA が機能しているのか確認する

1. 一度サインアウトする
2. 再度ログインをする
3. 下記の画面が表示されることを確認する
   ![sandbooks-aws-IAM-MFA-step19](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step19.png)

## おわりに

セキュリティ対策として大切な設定なので必ず行いたいですね。
