---
title: "【インフラ】AWSの初期設定をしょう！"
---

## 1. IAMユーザーを作成する

### 1. IAMに開く
@[card](https://zenn.dev/aew2sbee/articles/aws-ec2-iam-create-user)

## MFAを本人が設定できるように設定する
### 1. IAMに開く
1. **ルートユーザー**で検索欄に`IAM`と検索し検索結果から**IAM**をクリックします。
![sandbooks-aws-IAM-step01](/images/sandbooks-aws-IAM-step01.png)

### 2. MFAを本人が設定できるポリシーを作成する
1. 左側の`ポリシー`をクリックする
2. `ポリシーを作成`をクリックする
![sandbooks-aws-IAM-MFA-step01](/images/sandbooks-aws-IAM-MFA-step01.png)

### 3. MFAを本人が設定できるポリシーを作成する
1. `JSON`をクリックする
![sandbooks-aws-IAM-MFA-step02](/images/sandbooks-aws-IAM-MFA-step02.png)
2. `ポリシーエディタ`に下記JSONをコピペする
3. `次へ`をクリックする
````json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowViewAccountInfo",
            "Effect": "Allow",
            "Action": [
                "iam:GetAccountPasswordPolicy",
                "iam:GetAccountSummary",
                "iam:ListVirtualMFADevices"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AllowManageOwnPasswords",
            "Effect": "Allow",
            "Action": [
                "iam:ChangePassword",
                "iam:GetUser",
                "iam:CreateLoginProfile",
                "iam:DeleteLoginProfile",
                "iam:GetLoginProfile",
                "iam:UpdateLoginProfile"
            ],
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
            "Action": [
                "iam:CreateVirtualMFADevice",
                "iam:DeleteVirtualMFADevice"
            ],
            "Resource": "arn:aws:iam::*:mfa/${aws:username}"
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
                "iam:ChangePassword",
                "iam:GetUser",
                "iam:CreateLoginProfile",
                "iam:DeleteLoginProfile",
                "iam:GetLoginProfile",
                "iam:UpdateLoginProfile",
                "iam:ListMFADevices",
                "iam:ListVirtualMFADevices",
                "iam:ResyncMFADevice",
                "iam:DeleteVirtualMFADevice",
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
````
#### 補足情報
|  単語  | 意味  |
| ---- | ---- |
|  **AllowViewAccountInfo**  |  IAMユーザーがアカウント情報を表示するために許可されているアクションが定義されています。具体的には、アカウントのパスワードポリシーを取得したり、アカウントのサマリー情報を取得したり、仮想多要素認証（MFA）デバイスのリストを取得することができます  |
|  **AllowManageOwnPasswords**  |  IAMユーザーが自分自身のパスワードを管理するために許可されているアクションが定義されています。具体的には、パスワードの変更、自分自身のログインプロファイルの作成と削除、ログインプロファイルの取得と更新が許可されています  |
|  **AllowManageOwnAccessKeys**  |  IAMユーザーが自分自身のアクセスキーを管理するために許可されているアクションが定義されています。具体的には、アクセスキーの作成、削除、リストの表示、アクセスキーの更新が許可されています |
|  **AllowManageOwnSigningCertificates**  |  IAMユーザーが自分自身の署名証明書を管理するために許可されているアクションが定義されています。具体的には、署名証明書の削除、リストの表示、署名証明書の更新とアップロードが許可されています  |
|  **AllowManageOwnSSHPublicKeys**  |  IAMユーザーが自分自身のSSH公開鍵を管理するために許可されているアクションが定義されています。具体的には、SSH公開鍵の削除、取得、リストの表示、SSH公開鍵の更新とアップロードが許可されています  |
|  **AllowManageOwnGitCredentials**  |  IAMユーザーが自分自身のGitクレデンシャルを管理するために許可されているアクションが定義されています。具体的には、サービス固有のクレデンシャルの作成、削除、リストの表示、サービス固有のクレデンシャルのリセットと更新が許可されています。 |
|  **AllowManageOwnVirtualMFADevice**  |  IAMユーザーが自分自身の仮想多要素認証（MFA）デバイスを管理するために許可されているアクションが定義されています。具体的には、仮想MFAデバイスの作成と削除が許可されています  |
|  **AllowManageOwnUserMFA**  |  IAMユーザーが自分自身のMFAデバイスを管理するために許可されているアクションが定義されています。具体的には、MFAデバイスの無効化と有効化、MFAデバイスのリストの表示、MFAデバイスの再同期が許可されています  |
|  **DenyAllExceptListedIfNoMFA**  |  MFAが無効化されている場合に、ユーザーに許可されるアクションを制限する条件が定義されています。MFAが無効な場合、特定のアクション（仮想MFAデバイスの作成、MFAデバイスの有効化、パスワード変更など）に対してのみ許可され、それ以外のアクションに対しては拒否されます |
### 3. 任意のポリシーを設定する
1. `ポリシー名`の欄に任意の名前を入力します。
    ※今回は、`test-MFA`という名前で作成します。
![sandbooks-aws-IAM-MFA-step03](/images/sandbooks-aws-IAM-MFA-step03.png)
2. `許可`の欄に`IAM`がある事を確認する
    ※先ほどのJSONで`IAM`の内容の設定が反映されました。
3. 内容を確認し、`ポリシーの作成`をクリックする
![sandbooks-aws-IAM-MFA-step04](/images/sandbooks-aws-IAM-MFA-step04.png)

### 4. 成功メッセージを確認する
1. 画面上部に`ポリシー test-MFAが作成されました。`を確認する
2. 任意で設定したポリシー名の`test-MFA`が一覧で確認出来ます。
![sandbooks-aws-IAM-MFA-step05](/images/sandbooks-aws-IAM-MFA-step05.png)

### 5. 対象ユーザーに作成したポリシーを付与する
1. 左側の`ユーザー`をクリックする
2. 対象のユーザーをクリックする
    ※今回は、`test-user`を選択します。
![sandbooks-aws-IAM-MFA-step06](/images/sandbooks-aws-IAM-MFA-step06.png)

3. `許可を追加`の右横の▲をクリックし、`許可を追加`をクリックする
![sandbooks-aws-IAM-MFA-step07](/images/sandbooks-aws-IAM-MFA-step07.png)

4. `ポリシーを直接アタッチする`を選択する
5. 検索欄にキーワードを入力し調べやすくします。
6. 先ほど作成した`test-MFA`を選択する
7. `次へ`をクリックする
![sandbooks-aws-IAM-MFA-step08](/images/sandbooks-aws-IAM-MFA-step08.png)
8. 対象ユーザーと付与するポリシーを確認する
9. `許可を追加`をクリックする
![sandbooks-aws-IAM-MFA-step09](/images/sandbooks-aws-IAM-MFA-step09.png)
![sandbooks-aws-IAM-MFA-step10](/images/sandbooks-aws-IAM-MFA-step10.png)



````json
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": "sts:AssumeRole",
        "Resource": "arn:aws:iam::[AWSアカウント番号(13桁)]:role/Switch-Administrator-role*"
    }
}
````
#### 補足情報

