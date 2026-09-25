---
title: "[AWS] IAMの読み取り専用ユーザーが自分でMFAを設定できるようにする方法" # 記事のタイトル
emoji: "☁️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["aws", "iam", "mfa", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

社内の有志メンバーでの活動で、IAM ユーザー本人に MFA（多要素認証）を設定してもらう機会がありました。
何度も経験することではないため、備忘録としてまとめます。

|項目|内容|
|---|---|
|**対象者**|・AWS の IAM について知らない方<br>・読み取り専用の IAM ユーザーに、本人自身で MFA を設定してもらいたい方|
|**伝えたい内容**|・IAM ユーザー本人が MFA を設定できるようにするポリシーの作り方<br>・MFA を設定するまで他の操作をさせないための制御方法|
|**前提条件**|・AWS のルートユーザー作成済み<br>・ポリシーの作成とアタッチができる管理者権限のユーザーがある<br>・対象の IAM ユーザーが作成済みで、`ReadOnlyAccess`と`IAMUserChangePassword`が付与されている<br>・対象の IAM ユーザーの初回サインインとパスワード変更が完了している<br>・認証アプリをインストールするスマートフォン（iOS / Android）がある|

:::message
**MFA**（Multi-Factor Authentication ＝ 多要素認証）は、パスワードに加えてスマートフォンに表示される 6 桁のコードなど、別の要素を組み合わせて本人確認を行う仕組みです。
:::

IAM ユーザーがまだ無い場合は、下記の記事を先にご確認ください。

@[card](https://zenn.dev/aew2sbee/articles/aws-ec2-iam-create-user)

:::message
本記事は IAM **ユーザー**の MFA 設定に関する内容です。IAM ロールについては別記事で扱っています。
:::

## 🌱 `ReadOnlyAccess`と`IAMUserChangePassword`のポリシーだけでは設定できない

`ReadOnlyAccess`のユーザーを作成し、MFA を設定しようとしたところ、下記のエラーメッセージが表示されました。

:::message alert

```text
User: arn:aws:iam::XXXXXXXXXXXX:user/test_user is not authorized to perform: iam:CreateVirtualMFADevice on resource: arn:aws:iam::XXXXXXXXXXXX:mfa/deviceName because no identity-based policy allows the iam:CreateVirtualMFADevice action
```

:::

![sandbooks-aws-IAM-MFA-step00](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step00.png)

エラーメッセージは、下記のように読み解けます。

|英文|意味|
|---|---|
|`User: arn:aws:iam::XXXXXXXXXXXX:user/test_user`|誰が（`test_user`というユーザーが）|
|`is not authorized to perform: iam:CreateVirtualMFADevice`|何をしようとして失敗したのか（仮想 MFA デバイスの作成）|
|`on resource: arn:aws:iam::XXXXXXXXXXXX:mfa/deviceName`|どのリソースに対してなのか（`deviceName`という名前の MFA デバイス）|
|`because no identity-based policy allows the ... action`|なぜ失敗したのか（許可するポリシーが 1 つも付いていないため）|

つまり`iam:CreateVirtualMFADevice`を許可するポリシーを追加すれば解決できそうです。

## 🌱 ユーザー本人で MFA の設定を可能にする

### 1.【管理者】本人が MFA の設定を行えるポリシーを作成する

1. **管理者権限を持つユーザー**で AWS マネジメントコンソールにサインインし、検索欄に`IAM`と入力して**IAM**をクリックする
2. 左側の`ポリシー`をクリックする
3. `ポリシーを作成`をクリックする
   ![sandbooks-aws-IAM-MFA-step01](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step01.png)

4. 右側の`JSON`をクリックする
5. `ポリシーエディタ`に下記 JSON をコピペする
6. `次へ`をクリックする
   ![sandbooks-aws-IAM-MFA-step02](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step02.png)

このポリシーは、大きく次の 2 つで構成されています。

- **Allow（許可）**: IAM ユーザーが**自分自身の**認証情報（パスワード・MFA デバイスなど）を管理できるようにする
- **Deny（拒否）**: MFA 認証されていない状態では、MFA の設定に必要な操作**以外**をすべて禁止する

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
        "iam:GetAccessKeyLastUsed",
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
      "Action": ["iam:CreateVirtualMFADevice"],
      "Resource": "arn:aws:iam::*:mfa/*"
    },
    {
      "Sid": "AllowManageOwnUserMFA",
      "Effect": "Allow",
      "Action": [
        "iam:DeactivateMFADevice",
        "iam:EnableMFADevice",
        "iam:GetMFADevice",
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
        "iam:GetMFADevice",
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

上記は AWS 公式ドキュメントのサンプルポリシーをそのまま利用しています。

@[card](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/reference_policies_examples_aws_my-sec-creds-self-manage.html)

:::message alert
掲載しているスクリーンショットは執筆時のもので、上記 JSON とは内容が一部異なります。**正しい内容は本文の JSON**です。
:::

#### ポリシーの読み方

|記述|意味|
|---|---|
|`Version`|ポリシー言語のバージョンです。`2012-10-17`は固定値なので、日付を変更する必要はありません|
|`Sid`|Statement ID（この設定の塊につける名前）です。動作には影響しません|
|`Effect`|`Allow`は許可、`Deny`は拒否を表します。**同じアクションに Allow と Deny の両方が該当する場合は、必ず Deny が優先されます**|
|`Action`|`iam:CreateVirtualMFADevice`のような、AWS への操作の種類です|
|`NotAction`|`Action`の逆で、**ここに書かれた操作「以外」**が対象になります。`Deny`と組み合わせることで「リストにある操作しか実行できない」という制御になります|
|`Resource`|操作の対象となるリソースを`arn:aws:iam::アカウントID:user/ユーザー名`という形式（ARN）で指定します。`*`は任意の文字列を表すワイルドカードです|
|`${aws:username}`|**サインイン中の IAM ユーザー名に AWS が自動で置き換える変数**です。自分のユーザー名に書き換えず、そのままコピペしてください。この記述のおかげで、同じポリシーを全員に使い回しても各自が自分のリソースしか操作できません|

:::message alert
`${aws:username}`は IAM ユーザーにのみ展開される変数です。IAM ロールやフェデレーティッドユーザーでは空になり、正しく機能しません。
:::

#### 補足情報

各ステートメントは、IAM ユーザーが自分自身のリソースのみを操作できるように許可範囲を定義しています。

|Sid|許可する操作|
|---|---|
|**AllowViewAccountInfo**|アカウントのパスワードポリシーの取得、仮想 MFA デバイスの一覧表示<br>※`iam:ListVirtualMFADevices`はリソース単位の制限に対応していないため`Resource`は`*`にする必要があり、アカウント内の全デバイスが一覧されます|
|**AllowManageOwnPasswords**|自分のパスワードの変更、自分のユーザー情報の取得|
|**AllowManageOwnAccessKeys**|自分のアクセスキーの作成・削除・一覧表示・更新・最終使用日時の取得|
|**AllowManageOwnSigningCertificates**|自分の署名証明書の削除・一覧表示・更新・アップロード|
|**AllowManageOwnSSHPublicKeys**|自分の SSH 公開鍵の削除・取得・一覧表示・更新・アップロード|
|**AllowManageOwnGitCredentials**|自分のサービス固有のクレデンシャルの作成・削除・一覧表示・リセット・更新|
|**AllowManageOwnVirtualMFADevice**|仮想 MFA デバイスの作成<br>※作成したデバイスは`AllowManageOwnUserMFA`により自分自身にしか割り当てられません|
|**AllowManageOwnUserMFA**|自分の MFA デバイスの有効化・無効化・取得・一覧表示・再同期|
|**DenyAllExceptListedIfNoMFA**|**MFA 認証されていないリクエスト**の場合に、MFA の設定に必要な操作以外をすべて拒否します|

:::message alert
`DenyAllExceptListedIfNoMFA`により、MFA を設定するまでは**パスワードの変更を含むほぼすべての操作が拒否されます**（MFA を設定するまで他の操作をさせないための制御です）。
そのため、**初回サインイン時のパスワード変更が済んでいないユーザーにこのポリシーを付与すると、パスワードを変更できずサインインを完了できなくなります**。手順 4 の前に、対象ユーザーの初回パスワード変更を必ず終わらせてください。
:::

:::message
条件に`Bool`ではなく`BoolIfExists`を使っているのは、アクセスキーによる API / CLI 実行では`aws:MultiFactorAuthPresent`というキー自体が存在せず、`Bool`では拒否がすり抜けてしまうためです。
`BoolIfExists`にすることで、アクセスキー経由の操作も`sts:GetSessionToken`で MFA 付きの一時認証情報を取得しない限り拒否されます。
:::

:::message
MFA の設定だけが目的であれば、`AllowManageOwnAccessKeys` / `AllowManageOwnSigningCertificates` / `AllowManageOwnSSHPublicKeys` / `AllowManageOwnGitCredentials`の 4 つは削除しても動作します。
特に`iam:CreateAccessKey`は、読み取り専用ユーザーが**自分で長期のアクセスキーを発行できる**状態になるため、必要かどうか判断したうえでご利用ください。
:::

### 2.【管理者】ポリシー名を入力して作成する

1. `ポリシー名`の欄に任意の名前を入力する
   ※今回は、`test-MFA`という名前で作成します。
   ![sandbooks-aws-IAM-MFA-step03](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step03.png)
2. `許可`の欄に`IAM`があることを確認する
   ※先ほど貼り付けた JSON の設定が反映されています。
3. 内容を確認し、`ポリシーの作成`をクリックする
   ![sandbooks-aws-IAM-MFA-step04](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step04.png)

### 3.【管理者】成功メッセージを確認する

1. 画面上部に`ポリシー test-MFAが作成されました。`と表示されることを確認する
2. 任意で設定したポリシー名の`test-MFA`が一覧に表示されていることを確認する
   ![sandbooks-aws-IAM-MFA-step05](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step05.png)

### 4.【管理者】対象ユーザーに作成したポリシーを付与する

:::message alert
このポリシーを付与する前に、対象ユーザーの**初回サインインとパスワード変更を必ず完了させて**ください。
MFA 未設定の状態では`iam:ChangePassword`も拒否されるため、パスワードの強制変更が終わっていないユーザーはサインインできなくなります。
:::

1. 左側の`ユーザー`をクリックする
2. 対象のユーザーをクリックする
   ※今回は、`test-user`を選択します。
   ![sandbooks-aws-IAM-MFA-step06](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step06.png)

3. `許可を追加`の右横にある ▲ をクリックし、表示されたメニューから`許可を追加`を選択する
   ![sandbooks-aws-IAM-MFA-step07](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step07.png)

4. `ポリシーを直接アタッチする`を選択する
5. 検索欄にキーワードを入力して絞り込む
   ※`test-MFA`だったので`test`というキーワードで検索しています。
6. 先ほど作成した`test-MFA`にチェックを入れる
7. `次へ`をクリックする
   ![sandbooks-aws-IAM-MFA-step08](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step08.png)
8. 対象ユーザーと付与するポリシーを確認する
9. `許可を追加`をクリックする
   ![sandbooks-aws-IAM-MFA-step09](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step09.png)
10. `ポリシーが追加されました`という成功メッセージを確認する
11. ポリシーの一覧に`test-MFA`が追加されていることを確認する
    ![sandbooks-aws-IAM-MFA-step10](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step10.png)

### 5.【本人】MFA 設定を行う

:::message alert
ここからは**MFA を設定する本人**の作業です。
管理者のコンソールから一度サインアウトするか、シークレットウィンドウ／別のブラウザを使用してください。同一ブラウザでは AWS マネジメントコンソールに複数ユーザーで同時にサインインできません。
:::

1. 対象のユーザーでサインインする
   ※IAM ユーザーのサインインには、アカウント ID または専用のサインイン URL（`https://アカウントID.signin.aws.amazon.com/console`）が必要です。
   ![sandbooks-aws-IAM-step10](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-step10.png)
2. 左側の`ダッシュボード`から`MFAを追加`をクリックする
   ※画面右上のユーザー名メニューから`セキュリティ認証情報`を選択しても同じ画面に到達できます。
   :::message
   MFA の設定が完了するまでは、ダッシュボードに`iam:GetAccountSummary に対する許可がありません`などのアクセス拒否が表示されます。
   `DenyAllExceptListedIfNoMFA`による**想定内の動作**なので、そのまま`MFAを追加`に進んでください。
   :::
   ![sandbooks-aws-IAM-MFA-step11](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step11.png)
3. `MFAを追加`をクリックすると`セキュリティ認証情報`ページに遷移するので、下へスクロールし`多要素認証(MFA)`を見つけて`MFAデバイスの割り当て`をクリックする
   ![sandbooks-aws-IAM-MFA-step12](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step12.png)
4. 登録するデバイス名を`デバイス名`の入力欄に記入する
   :::message alert
   【注意１】
   デバイス名は AWS アカウント内で一意である必要があり、他のメンバーと同じデバイス名は登録できません。
   **NG** : A さん=iPhone, B さん=iPhone
   **OK** : A さん=iPhone-A, B さん=iPhone-B
   :::
   :::message alert
   【注意２】
   途中でキャンセルすると未割り当ての仮想 MFA デバイスが残り、同じデバイス名で再登録できないことがあります。
   本記事のポリシーでは`iam:DeleteVirtualMFADevice`が拒否されるため**本人では削除できません**。その場合は管理者に依頼し、AWS CLI の`aws iam list-virtual-mfa-devices`と`aws iam delete-virtual-mfa-device`で未割り当てのデバイスを削除してもらってください。
   :::
5. `認証アプリケーション`を選択する
   ※選択肢は`パスキーまたはセキュリティキー` / `認証アプリケーション` / `ハードウェアTOTPトークン`の 3 種類です。AWS はフィッシング耐性のあるパスキー・セキュリティキーを推奨していますが、本記事では認証アプリケーションを使用します。
6. `次へ`をクリックする
   ![sandbooks-aws-IAM-MFA-step13](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step13.png)

7. `認証アプリケーション`をスマホにダウンロードする
   :::message
   AWS 公式が案内している認証アプリケーションは下記を参照してください。
   @[card](https://aws.amazon.com/jp/iam/features/mfa/?audit=2019q1)
   :::
   自分は下記を利用しました。
   @[card](https://www.microsoft.com/ja-jp/security/mobile-authenticator-app)
   ![sandbooks-aws-IAM-MFA-step14](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step14.png)
8. `QRコードを表示`をクリックし、QR コードを表示させる
9. 先ほどインストールした`認証アプリケーション`で QR コードを読み込む
   :::message alert
   QR コードとシークレットキーは MFA の秘密情報そのものです。スクリーンショットの撮影や第三者への共有は絶対に行わないでください。
   :::
   ![sandbooks-aws-IAM-MFA-step15](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step15.png)
10. `MFAコード1`に現在表示されている 6 桁の数字を入力し、**表示が次の数字に切り替わるまで（最大 30 秒）待ってから**`MFAコード2`に次の数字を入力する
    ※同じ数字を 2 回入力するとエラーになります。連続する 2 つのコードを入力することで、時刻が同期していることを AWS が確認しています。
11. `MFAを追加`をクリックする
    ![sandbooks-aws-IAM-MFA-step16](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step16.png)
12. 成功メッセージを確認する
    ![sandbooks-aws-IAM-MFA-step17](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step17.png)
13. `多要素認証(MFA)`の一覧に先ほどのデバイス名が登録されていることを確認する
    ![sandbooks-aws-IAM-MFA-step18](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step18.png)

### 6.【本人】サインアウトして再サインインする

:::message alert
`aws:MultiFactorAuthPresent`は**サインイン時に決まるセッションの属性**です。
MFA を登録しただけの既存セッションでは`false`のままで拒否が続くため、**必ずサインアウトして再サインインしてください**。再サインイン後に`ReadOnlyAccess`などの権限が使えるようになります。
:::

1. 一度サインアウトする
2. 再度サインインする
3. 下記の画面が表示されることを確認する
   ![sandbooks-aws-IAM-MFA-step19](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step19.png)

## 🌱 おわりに

MFA はセキュリティ対策の基本なので、読み取り専用のユーザーであっても必ず設定しておきたいところです。
なお、MFA デバイスは 1 ユーザーあたり最大 8 個まで登録できます。スマートフォンの紛失や故障でサインインできなくなることを防ぐため、複数のデバイスを登録しておくと安心です。
同じようにポリシー不足でつまずいた方の参考になれば幸いです。

## 🌱 参考

@[card](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/reference_policies_examples_aws_my-sec-creds-self-manage.html)
@[card](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html)
@[card](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_credentials_mfa.html)
