---
title: "[AWS]AWS VaultとTerraformでEC2を作成する" # 記事のタイトル
emoji: "💨" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["aws", "awsvault", "初心者向け", "powershell", "terraform"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

Terraform で AWS のリソースを管理するために、AWS Vault で取得した一時的な認証情報を使って、Terraform で EC2 を作成する手順を解説します。

※個人プロジェクト「sandbooks」の作業の一環です。以降の`sandbooks`は、プロジェクト名（AWS CLI のプロファイル名）です。

### 前提条件

- 下記記事の作業（AWS Vault のインストール）が完了していること

  @[card](https://zenn.dev/aew2sbee/articles/aws-vault-install-to-powershell)

- Terraform がインストールされていること（`terraform -version`でバージョンが表示されること）

  @[card](https://developer.hashicorp.com/terraform/install)

- IAM ユーザーのアクセスキーを発行し、MFA デバイスを登録していること
- IAM ユーザーが Assume Role するロールに、EC2 を作成・削除する権限があること
- 東京リージョンにデフォルト VPC があること（本記事のコードはサブネットやセキュリティグループを指定しないため、デフォルト VPC に作成されます）

:::message alert
本記事は、2023年8月時点の手順です。

- 本家の aws-vault（99designs/aws-vault）は開発が終了しています。今後も更新を受けたい場合は、フォーク版の[ByteNess/aws-vault](https://github.com/ByteNess/aws-vault)を検討してください。
- 実行結果は、AWS プロバイダー v5.12.0 のときの出力です。v6 系では plan の出力が一部異なるため、本記事のコードでは`version = "~> 5.0"`で v5 系に固定しています。
- 2025年7月15日以降に作成した AWS アカウントの無料プランでは、`t2.micro`は対象外です（`t3.micro`などが対象）。
:::

## 🌱 1. AWS CLI をインストール

1. 下記ページにアクセスする

   @[card](https://aws.amazon.com/jp/cli/)

2. Windows の方は、`64ビット`をクリックしインストーラーをダウンロードする（現在は、ページの構成が変わっています。インストーラーは[インストールガイド](https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/getting-started-install.html)から入手してください）
   ![terraform-aws-vault-ec2-step00](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step00.png)

3. ダウンロードされたインストーラーを実行する
   ![terraform-aws-vault-ec2-step02](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step02.png)

4. ✅ を付けて`Next`をクリックする
   ![terraform-aws-vault-ec2-step09](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step09.png)

5. インストールを開始するために、`Next`をクリックする
   ![terraform-aws-vault-ec2-step03](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step03.png)

6. `Next`をクリックする
   ![terraform-aws-vault-ec2-step04](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step04.png)

7. `Next`をクリックする
   ![terraform-aws-vault-ec2-step05](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step05.png)

8. `Install`をクリックする
   ![terraform-aws-vault-ec2-step06](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step06.png)

9. インストールが完了するまで待つ
   ![terraform-aws-vault-ec2-step07](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step07.png)

10. 完了したら`Finish`をクリックする
    ![terraform-aws-vault-ec2-step08](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step08.png)

11. PowerShell を開き直し、`aws --version`でバージョンが表示されることを確認する

## 🌱 2. `~/.aws/config`を設定する

1. エクスプローラーで`C:\Users\<ユーザー名>\.aws\config`を開く（ファイルがない場合は、拡張子なしの`config`というファイルを作成する）
   ![terraform-aws-vault-ec2-step01](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step01.png)

2. 下記のように変更する

   ```ini:~/.aws/config
   [default]
   # アジアパシフィック(東京)
   region=ap-northeast-1
   output=json

   [profile sandbooks]
   # アジアパシフィック(東京)
   region=ap-northeast-1
   # MFA デバイスの ARN
   mfa_serial=arn:aws:iam::[アカウントID]:mfa/XXXXX
   # Assume Role するロールの ARN
   role_arn=arn:aws:iam::[アカウントID]:role/XXXXX
   source_profile=default
   ```

   :::message
   コメントは、値と同じ行ではなく、独立した行に書きます。値と同じ行に書くと、ツールによってはコメントまで値として読み込まれます。
   :::

※`~/.aws/credentials`は空のままで大丈夫です。

## 🌱 3. `aws-vault`経由で`Access Key ID`、`Secret Access Key`を登録する

1. 下記コマンドを実行し、`Access Key ID`、`Secret Access Key`を登録する

```powershell
aws-vault add default
```

実行結果: `Access Key ID`、`Secret Access Key`の入力を求められます。

```powershell
PS C:\Users\Users\Work\sandbooks-infra> aws-vault add default
Enter Access Key ID: AKXXXXXXXXXXXXXXXXXX
Enter Secret Access Key: ****************************************
Added credentials to profile "default" in vault
```

※事前に IAM でアクセスキーを発行しておく必要があります。

:::message
aws-vault を使うと、`~/.aws/credentials`にアクセスキーを記述する必要がなくなります。
:::

2. MFA 認証ができるのか確認するために、下記コマンドを実行する

```powershell
aws-vault exec sandbooks -- aws s3 ls
```

実行結果: MFA デバイスとして登録した端末に表示される**6 桁**のコードを入力する（バケットがなければ、何も表示されずに終了します）

```powershell
PS C:\Users\Users\Work\sandbooks-infra> aws-vault exec sandbooks -- aws s3 ls
Enter MFA code for arn:aws:iam::[アカウントID]:mfa/iPhone: 123456
```

3. 一時的な認証情報がキャッシュされているか確認する

```powershell
aws-vault ls
```

実行結果: MFA 付きの Assume Role で取得した一時的な認証情報がキャッシュされ、残り`58m37s`の間は MFA コードを再入力せずに使えます。

```powershell
PS C:\Users\Users\Work\sandbooks-infra> aws-vault ls
Profile                  Credentials              Sessions
=======                  ===========              ========
default                  default                  -
sandbooks                sandbooks                sts.AssumeRole:58m37s
```

## 🌱 4. Terraform ファイルの作成

1. 作業用のフォルダを作成し、移動する（以降のコマンドは、すべてこのフォルダで実行する）

   ```powershell
   mkdir sandbooks-infra
   cd sandbooks-infra
   ```

2. ファイル構成は下記のとおりにする

   ```text
   .
   ├── aws.tf
   └── variables.tf
   ```

```hcl:aws.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.region
}

resource "aws_instance" "example" {
  ami           = var.amis[var.region]
  instance_type = var.instance_type
}
```

```hcl:variables.tf
variable "region" {
  description = "AWS region to host your network"
  type        = string
  default     = "ap-northeast-1"
}

variable "instance_type" {
  type    = string
  default = "t2.micro"
}

variable "amis" {
  type = map(string)
  default = {
    "ap-northeast-1" = "ami-04beabd6a4fb6ab6f"
  }
}
```

:::message
`ami-04beabd6a4fb6ab6f`は、執筆時点の AMI ID です。リージョンやアップデートの影響で使えない場合があるため、EC2 コンソールの「AMI カタログ」で最新の AMI ID を確認し、書き換えてください。
:::

## 🌱 5. Terraform で EC2 を作成する

1. 下記コマンドを実行する

```powershell
aws-vault exec sandbooks -- terraform init
```

実行結果

下記は、2 回目以降に実行したときの出力です。初めて実行する場合は、`Finding latest version of hashicorp/aws...`、`Installing hashicorp/aws ...`のように表示されます。

```text
PS C:\Users\Users\Work\sandbooks-infra> aws-vault exec sandbooks -- terraform init
Enter MFA code for arn:aws:iam::[アカウントID]:mfa/iPhone: 123456

Initializing the backend...

Initializing provider plugins...
- Reusing previous version of hashicorp/aws from the dependency lock file
- Using previously-installed hashicorp/aws v5.12.0

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

2. 下記コマンドを実行する

```powershell
aws-vault exec sandbooks -- terraform plan
```

実行結果: 作成されるリソースの内容（`aws_instance.example`が 1 つ作成される）を確認します。

```text
PS C:\Users\Users\Work\sandbooks-infra> aws-vault exec sandbooks -- terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the
following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.example will be created
  + resource "aws_instance" "example" {
      + ami                                  = "ami-04beabd6a4fb6ab6f"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + cpu_core_count                       = (known after apply)
      + cpu_threads_per_core                 = (known after apply)
      + disable_api_stop                     = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + host_resource_group_arn              = (known after apply)
      + iam_instance_profile                 = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_lifecycle                   = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t2.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      + key_name                             = (known after apply)
      + monitoring                           = (known after apply)
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      + placement_partition_number           = (known after apply)
      + primary_network_interface_id         = (known after apply)
      + private_dns                          = (known after apply)
      + private_ip                           = (known after apply)
      + public_dns                           = (known after apply)
      + public_ip                            = (known after apply)
      + secondary_private_ips                = (known after apply)
      + security_groups                      = (known after apply)
      + source_dest_check                    = true
      + spot_instance_request_id             = (known after apply)
      + subnet_id                            = (known after apply)
      + tags_all                             = (known after apply)
      + tenancy                              = (known after apply)
      + user_data                            = (known after apply)
      + user_data_base64                     = (known after apply)
      + user_data_replace_on_change          = false
      + vpc_security_group_ids               = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if
you run "terraform apply" now.
```

3. 下記コマンドを実行する

```powershell
aws-vault exec sandbooks -- terraform apply
```

実行途中で`Only 'yes' will be accepted to approve.`と尋ねられるので、`yes`と入力する

```text
Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes
```

実行結果: `i-0123456789abcdef0`が作成された（インスタンス ID はダミー値に置き換えています）

```text
PS C:\Users\Users\Work\sandbooks-infra> aws-vault exec sandbooks -- terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.example will be created
  + resource "aws_instance" "example" {
      + ami                                  = "ami-04beabd6a4fb6ab6f"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + cpu_core_count                       = (known after apply)
      + cpu_threads_per_core                 = (known after apply)
      + disable_api_stop                     = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + host_resource_group_arn              = (known after apply)
      + iam_instance_profile                 = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_lifecycle                   = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t2.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      + key_name                             = (known after apply)
      + monitoring                           = (known after apply)
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      + placement_partition_number           = (known after apply)
      + primary_network_interface_id         = (known after apply)
      + private_dns                          = (known after apply)
      + private_ip                           = (known after apply)
      + public_dns                           = (known after apply)
      + public_ip                            = (known after apply)
      + secondary_private_ips                = (known after apply)
      + security_groups                      = (known after apply)
      + source_dest_check                    = true
      + spot_instance_request_id             = (known after apply)
      + subnet_id                            = (known after apply)
      + tags_all                             = (known after apply)
      + tenancy                              = (known after apply)
      + user_data                            = (known after apply)
      + user_data_base64                     = (known after apply)
      + user_data_replace_on_change          = false
      + vpc_security_group_ids               = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_instance.example: Creating...
aws_instance.example: Still creating... [10s elapsed]
aws_instance.example: Still creating... [20s elapsed]
aws_instance.example: Still creating... [31s elapsed]
aws_instance.example: Creation complete after 32s [id=i-0123456789abcdef0]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

## 🌱 6. AWS コンソールで作成できているか確認する

1. AWS コンソールにログインする
2. EC2 を作成した`リージョン`に切り替える
3. `EC2 ダッシュボード`にアクセスし、先ほど作成したインスタンスがあるか確認する

   ![terraform-aws-vault-ec2-step10](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step10.png)

:::message alert
`terraform apply`が完了した時点で、インスタンスは起動しており、料金が発生します。
停止してもストレージ（EBS）の料金は発生し続けます。また、デフォルト VPC では通常パブリック IPv4 アドレスが割り当てられ、その料金も発生します。そのため、不要になったら次の手順で削除してください。
:::

## 🌱 7. 作成したリソースを削除する

1. 下記コマンドを実行し、`yes`と入力する

   ```powershell
   aws-vault exec sandbooks -- terraform destroy
   ```

2. `Destroy complete! Resources: 1 destroyed.`と表示されたら、EC2 コンソールでインスタンスの状態が「終了済み」になっていることを確認する

## 🌱 補足

- 本記事のコードは、手順を確認するための最小構成です。実際に使う場合は、`metadata_options { http_tokens = "required" }`で IMDSv2 を必須にするなど、セキュリティの設定も検討してください。
- 画像では AWS コンソールにルートユーザーでサインインしていますが、普段の作業には IAM ユーザーを使うことを推奨します。

## 🌱 おわりに

Windows と Terraform の相性が悪く、環境構築に時間がかかりました。
macOS ならもっと楽にできそうです。
