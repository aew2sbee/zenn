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

:::message
**MFA**（Multi-Factor Authentication ＝ 多要素認証）は、パスワードに加えてスマートフォンに表示される 6 桁のコードなど、別の要素を組み合わせて本人確認を行う仕組みです。
:::

|項目|内容|
|---|---|
|**対象者**|・IAM ユーザーを管理している管理者（ポリシーの作成とアタッチができる方）<br>・読み取り専用の IAM ユーザーに、本人で MFA を設定してもらいたい方|
|**伝えたい内容**|・IAM ユーザー本人が MFA を設定できるようにするポリシーの作り方<br>・MFA を設定するまで他の操作をさせないための制御方法|
|**前提条件**|・AWS のルートユーザー作成済み<br>・ポリシーの作成とアタッチができる管理者権限のユーザーがいる<br>・対象の IAM ユーザーが作成済みで、`ReadOnlyAccess`と`IAMUserChangePassword`が付与されている<br>・対象の IAM ユーザーの初回サインインとパスワード変更が完了している<br>・対象の IAM ユーザーに、アカウント ID またはサインイン URL を共有済み<br>・認証アプリをインストールするスマートフォン（iOS / Android）がある|

本記事で使う用語は次のとおりです。

|用語|意味|
|---|---|
|**IAM**|AWS の利用者と、その利用者が「何をできるか」（権限）を管理するサービス|
|**ルートユーザー**|AWS アカウントを作成したときの最上位のユーザー。日常の作業には使いません|
|**IAM ユーザー**|ルートユーザーや管理者が作成する、利用者ごとのユーザー|
|**ポリシー**|許可・拒否する操作を JSON で書いた文書。ユーザーに**アタッチ**（紐付け）すると有効になります|

IAM ユーザーがまだない場合は、下記の記事を先にご確認ください。

@[card](https://zenn.dev/aew2sbee/articles/aws-ec2-iam-create-user)

:::message
本記事は IAM **ユーザー**の MFA 設定に関する内容です。IAM ロールについては下記の記事で扱っています。
:::

@[card](https://zenn.dev/aew2sbee/articles/aws-ec2-iam-why-role)

## 🌱 `ReadOnlyAccess`と`IAMUserChangePassword`のポリシーだけでは設定できない

`ReadOnlyAccess`は、AWS があらかじめ用意しているポリシー（AWS 管理ポリシー）で、閲覧だけを許可します。MFA デバイスの作成は書き込みにあたる操作のため、このポリシーでは許可されません。
実際に`ReadOnlyAccess`のユーザーで MFA を設定しようとしたところ、下記のエラーメッセージが表示されました。

:::message alert

```text
User: arn:aws:iam::XXXXXXXXXXXX:user/test_user is not authorized to perform: iam:CreateVirtualMFADevice on resource: arn:aws:iam::XXXXXXXXXXXX:mfa/deviceName because no identity-based policy allows the iam:CreateVirtualMFADevice action
```

:::

![sandbooks-aws-IAM-MFA-step00](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step00.png)

`arn:aws:iam::...`の部分は **ARN**（Amazon Resource Name）と呼ばれる、AWS 上のリソースを一意に表す名前です。`XXXXXXXXXXXX`には 12 桁のアカウント ID が入ります。
エラーメッセージは、下記のように読み解けます。

|英文|意味|
|---|---|
|`User: arn:aws:iam::XXXXXXXXXXXX:user/test_user`|誰が（`test_user`というユーザーが）|
|`is not authorized to perform: iam:CreateVirtualMFADevice`|何をしようとして失敗したのか（仮想 MFA デバイスの作成）|
|`on resource: arn:aws:iam::XXXXXXXXXXXX:mfa/deviceName`|どのリソースに対してなのか（`deviceName`という名前の MFA デバイス）|
|`because no identity-based policy allows the ... action`|なぜ失敗したのか（ユーザーに付与されたポリシー（identity-based policy）の中に、この操作を許可するものがないため）|

つまり`iam:CreateVirtualMFADevice`を許可するポリシーを追加すれば解決できそうです。
ただし、MFA の登録にはデバイスの作成だけでなく、ユーザーへの有効化（`iam:EnableMFADevice`）などの操作も必要です。また、MFA を設定するまでは他の操作をさせない制御も入れたいため、本記事では AWS 公式ドキュメントのサンプルポリシーを使います。

## 🌱 ユーザー本人が MFA を設定できるようにする

### 1.【管理者】本人が MFA の設定を行えるポリシーを作成する

1. **管理者権限を持つユーザー**で AWS マネジメントコンソールにサインインし、検索欄に`IAM`と入力して**IAM**をクリックする
2. 左側の`ポリシー`をクリックする
3. `ポリシーを作成`をクリックする
   ![sandbooks-aws-IAM-MFA-step01](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step01.png)

4. 右側の`JSON`をクリックする
5. `ポリシーエディタ`に最初から入っている内容をすべて削除し、下記の JSON を貼り付ける

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
        "iam:UpdateAccessKey",
        "iam:GetAccessKeyLastUsed"
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

6. エディタの下部にエラーが表示されていないことを確認し、`次へ`をクリックする

:::message alert
下記のスクリーンショットは執筆時のもので、上記 JSON とは内容が一部異なります。**正しい内容は本文の JSON**です。
:::

![sandbooks-aws-IAM-MFA-step02](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step02.png)

上記は AWS 公式ドキュメントのサンプルポリシーをそのまま利用しています。

@[card](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/reference_policies_examples_aws_my-sec-creds-self-manage.html)

このポリシーは、大きく次の 2 つで構成されています。

- **Allow（許可）**: IAM ユーザーが**自分自身の**認証情報（パスワード・MFA デバイスなど）を管理できるようにする
- **Deny（拒否）**: MFA 認証されていない状態では、MFA の設定に必要な操作**以外**をすべて禁止する

#### ポリシーの読み方

|記述|意味|
|---|---|
|`Version`|ポリシー言語のバージョンです。`2012-10-17`は固定値なので、日付を変更する必要はありません|
|`Sid`|Statement ID（この設定の塊につける名前）です。動作には影響しません|
|`Effect`|`Allow`は許可、`Deny`は拒否を表します。**同じアクションに Allow と Deny の両方が該当する場合は、必ず Deny が優先されます**|
|`Action`|`iam:CreateVirtualMFADevice`のような、AWS への操作の種類です。`サービス名:操作名`という形式で書きます|
|`NotAction`|`Action`の逆で、ここに書かれた操作**以外**が対象になります。`Deny`と組み合わせると「リストにある操作以外をすべて拒否する」制御になります（リストの操作を実行するには、別途`Allow`で許可が必要です）|
|`Resource`|操作の対象となるリソースを ARN（`arn:aws:iam::アカウントID:user/ユーザー名`）の形式で指定します。`*`は任意の文字列を表すワイルドカードです|
|`Condition`|ステートメントを適用する条件です。本記事では`aws:MultiFactorAuthPresent`（MFA でサインインしたかどうか）が`false`のとき、つまり MFA 未認証のときだけ Deny が適用されます|
|`${aws:username}`|**サインイン中の IAM ユーザー名に AWS が自動で置き換える変数**です。自分のユーザー名に書き換えず、そのまま貼り付けてください。この記述のおかげで、同じポリシーを全員に使い回しても、ユーザー単位の操作は各自が自分のユーザーに対してしか行えません|

:::message alert
`${aws:username}`は IAM ユーザーのリクエストにだけ存在する変数です。IAM ロールやフェデレーティッドユーザーのリクエストには`aws:username`が存在しないため、この変数を使った`Resource`はどのリソースにも一致せず、正しく機能しません。
:::

@[card](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/reference_policies_variables.html)

#### 補足情報

各ステートメント（Sid）で許可・拒否している操作は次のとおりです。

|Sid|操作|
|---|---|
|**AllowViewAccountInfo**|アカウントのパスワードポリシーの取得、仮想 MFA デバイスの一覧表示<br>※`iam:ListVirtualMFADevices`は「自分のものだけ」のように対象を絞り込めない仕様のため、`Resource`は`*`にする必要があり、アカウント内の全デバイスが一覧表示されます。一覧を見られるだけで、他人のデバイスを操作することはできません|
|**AllowManageOwnPasswords**|自分のパスワードの変更、自分のユーザー情報の取得|
|**AllowManageOwnAccessKeys**|自分のアクセスキーの作成・削除・一覧表示・更新・最終使用日時の取得|
|**AllowManageOwnSigningCertificates**|自分の署名証明書の削除・一覧表示・更新・アップロード|
|**AllowManageOwnSSHPublicKeys**|自分の SSH 公開鍵の削除・取得・一覧表示・更新・アップロード|
|**AllowManageOwnGitCredentials**|自分のサービス固有の認証情報の作成・削除・一覧表示・リセット・更新|
|**AllowManageOwnVirtualMFADevice**|仮想 MFA デバイスの作成<br>※作成したデバイスは`AllowManageOwnUserMFA`により自分自身にしか割り当てられません|
|**AllowManageOwnUserMFA**|自分の MFA デバイスの有効化・無効化・一覧表示・再同期|
|**DenyAllExceptListedIfNoMFA**|**MFA 認証されていないリクエスト**の場合に、MFA の設定に必要な操作以外をすべて拒否します|

:::message alert
`DenyAllExceptListedIfNoMFA`により、MFA を設定するまでは**パスワードの変更を含むほぼすべての操作が拒否されます**。詳しくは「4.【管理者】対象ユーザーに作成したポリシーを付与する」の注意を参照してください。
:::

:::message
アクセスキーは、プログラムやコマンドラインから AWS を操作するための認証情報です。署名証明書・SSH 公開鍵・サービス固有の認証情報も、MFA の設定とは関係のない認証情報です。
MFA の設定だけが目的であれば、`AllowManageOwnAccessKeys` / `AllowManageOwnSigningCertificates` / `AllowManageOwnSSHPublicKeys` / `AllowManageOwnGitCredentials`の 4 つは削除しても動作します。
特に`iam:CreateAccessKey`は、読み取り専用ユーザーが**自分で長期のアクセスキーを発行できる**状態になるため、必要かどうか判断したうえでご利用ください。
:::

:::message
（コンソールだけを使う場合は読み飛ばして構いません）
条件に`Bool`ではなく`BoolIfExists`を使っているのは、アクセスキーによる API / CLI（コマンドで AWS を操作するツール）の実行では`aws:MultiFactorAuthPresent`というキー自体が存在せず、`Bool`では拒否がすり抜けてしまうためです。
`BoolIfExists`にすることで、アクセスキー経由の操作も`sts:GetSessionToken`で MFA 付きの一時認証情報（有効期限付きの認証情報）を取得しない限り拒否されます。
:::

@[card](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/reference_policies_condition-keys.html)

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
2. 作成したポリシー（今回は`test-MFA`）が一覧に表示されていることを確認する
   ![sandbooks-aws-IAM-MFA-step05](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step05.png)

### 4.【管理者】対象ユーザーに作成したポリシーを付与する

:::message alert
このポリシーを付与する前に、対象ユーザーの**初回サインインとパスワード変更を必ず完了させて**ください。
MFA 未設定の状態では`iam:ChangePassword`も拒否されるため、パスワードの強制変更が終わっていないユーザーはサインインできなくなります。
:::

:::message
スクリーンショットでは、管理者の画面で`test-user`、本人の画面で`test_user`という別名のユーザーを使っています。実際には同じユーザーで作業してください。
:::

1. 左側の`ユーザー`をクリックする
2. 対象のユーザーをクリックする
   ※今回は、`test-user`を選択します。
   ![sandbooks-aws-IAM-MFA-step06](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step06.png)

3. `許可を追加`の右横にある ▼ をクリックし、表示されたメニューから`許可を追加`を選択する
   ![sandbooks-aws-IAM-MFA-step07](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step07.png)

4. `ポリシーを直接アタッチする`を選択する
5. 検索欄にキーワードを入力して絞り込む
   ※ポリシー名が`test-MFA`なので、`test`というキーワードで検索しています。
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
管理者のコンソールから一度サインアウトするか、シークレットウィンドウまたは別のブラウザを使用してください。コンソールのマルチセッション機能を有効にしていない場合、同一ブラウザでは複数ユーザーで同時にサインインできません。
:::

1. 対象のユーザーでサインインする
   ※IAM ユーザーのサインインには、アカウント ID または専用のサインイン URL（`https://アカウントID.signin.aws.amazon.com/console`）が必要です。
   ![sandbooks-aws-IAM-step10](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-step10.png)
2. 検索欄に`IAM`と入力して**IAM**をクリックする
3. 左側の`ダッシュボード`をクリックし、`MFA を自分用に追加`の右にある`MFAを追加`をクリックする
   ※画面右上のユーザー名メニューから`セキュリティ認証情報`を選択しても同じ画面に到達できます。

   :::message
   MFA の設定が完了するまでは、ダッシュボードに`iam:GetAccountSummary に対する許可がありません`などのアクセス拒否が表示されます。
   `DenyAllExceptListedIfNoMFA`による**想定内の動作**なので、そのまま`MFAを追加`に進んでください。
   :::

   ![sandbooks-aws-IAM-MFA-step11](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step11.png)
4. `MFAを追加`をクリックすると`セキュリティ認証情報`ページに遷移するので、下へスクロールし`多要素認証(MFA)`を見つけて`MFAデバイスの割り当て`をクリックする
   ![sandbooks-aws-IAM-MFA-step12](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step12.png)
5. 登録するデバイス名を`デバイス名`の入力欄に記入する

   :::message alert
   【注意1】
   デバイス名は AWS アカウント内で一意である必要があり、他のメンバーと同じデバイス名は登録できません。
   **NG** : A さん=iPhone、B さん=iPhone
   **OK** : A さん=iPhone-A、B さん=iPhone-B
   :::

   :::message
   【注意2】
   途中でキャンセルすると、未割り当ての仮想 MFA デバイスが残ることがあります。未割り当てのデバイスは、コンソールから新しい仮想 MFA デバイスを追加するときに自動で削除されるため、同じデバイス名で登録し直せます。
   それでも登録できない場合、本記事のポリシーでは`iam:DeleteVirtualMFADevice`が拒否されるため**本人では削除できません**。管理者に依頼し、AWS CLI（または AWS CloudShell）で未割り当てのデバイスを削除してもらってください。

   ```bash
   # 未割り当ての仮想 MFA デバイスを一覧表示する
   aws iam list-virtual-mfa-devices --assignment-status Unassigned

   # 一覧で確認した SerialNumber を指定して削除する
   aws iam delete-virtual-mfa-device --serial-number arn:aws:iam::<アカウントID>:mfa/<デバイス名>
   ```

   :::

6. `認証アプリケーション`を選択する
   ※現在の選択肢は`パスキーまたはセキュリティキー` / `認証アプリケーション` / `ハードウェアTOTPトークン`の 3 種類です（スクリーンショットは旧画面のため表記が異なります）。AWS はフィッシング（偽サイトにコードを入力させる攻撃）に強いパスキー・セキュリティキーを推奨していますが、本記事では認証アプリケーションを使用します。
7. `次へ`をクリックする
   ![sandbooks-aws-IAM-MFA-step13](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step13.png)

8. `認証アプリケーション`をスマートフォンにダウンロードする

   :::message
   AWS 公式が案内している認証アプリケーションは下記を参照してください。

   @[card](https://aws.amazon.com/jp/iam/features/mfa/?audit=2019q1)

   :::

   自分は下記を利用しました。

   @[card](https://www.microsoft.com/ja-jp/security/mobile-authenticator-app)

   ![sandbooks-aws-IAM-MFA-step14](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step14.png)
9. `QRコードを表示`をクリックし、QR コードを表示させる
10. 先ほどインストールした`認証アプリケーション`で QR コードを読み込む

    :::message alert
    QR コードとシークレットキーは MFA の秘密情報そのものです。スクリーンショットの撮影や第三者への共有は絶対に行わないでください。
    :::

    ![sandbooks-aws-IAM-MFA-step15](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step15.png)
11. `MFAコード1`に現在表示されている 6 桁の数字を入力し、**表示が次の数字に切り替わるまで（最大 30 秒）待ってから**`MFAコード2`に次の数字を入力する
    ※同じ数字を 2 回入力するとエラーになります。連続する 2 つのコードを入力することで、時刻が同期していることを AWS が確認しています。
12. `MFAを追加`をクリックする
    ![sandbooks-aws-IAM-MFA-step16](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step16.png)
13. 成功メッセージを確認する
    ![sandbooks-aws-IAM-MFA-step17](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step17.png)
14. `多要素認証(MFA)`の一覧に先ほどのデバイス名が登録されていることを確認する
    ![sandbooks-aws-IAM-MFA-step18](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step18.png)

### 6.【本人】サインアウトして再サインインする

:::message alert
`aws:MultiFactorAuthPresent`は**サインイン時に決まるセッション（サインインしてからサインアウトするまでの状態）の属性**です。
MFA を登録しただけの既存セッションでは`false`のままで拒否が続くため、**必ずサインアウトして再サインインしてください**。MFA を使ってサインインし直すことで、初めて「MFA 認証済み」として扱われ、`ReadOnlyAccess`などの権限が使えるようになります。
:::

1. 一度サインアウトする
2. 再度サインインする
3. 下記の MFA 認証画面が表示されることを確認する
   ![sandbooks-aws-IAM-MFA-step19](/images/articles/aws-ec2-iam-role/sandbooks-aws-IAM-MFA-step19.png)
4. 認証アプリに表示されている 6 桁のコードを`MFAコード`に入力し、`送信`をクリックする
5. IAM のダッシュボードを開き、手順 5 で表示されていたアクセス拒否（`iam:GetAccountSummary`など）が表示されなくなったことを確認する

## 🌱 おわりに

本記事の要点は次の 3 つです。

- AWS 公式のサンプルポリシーを付与すると、IAM ユーザー本人が MFA を設定でき、MFA を設定するまで他の操作は拒否される
- ポリシーを付与する前に、対象ユーザーの初回パスワード変更を済ませておく
- MFA を登録した後は、サインアウトして再サインインする

なお、MFA デバイスは 1 ユーザーあたり最大 8 個まで登録できます。スマートフォンの紛失や故障でサインインできなくなることを防ぐため、複数のデバイスを登録しておくと安心です。

## 🌱 参考

@[card](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/reference_policies_examples_aws_my-sec-creds-self-manage.html)

@[card](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html)

@[card](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_credentials_mfa.html)
