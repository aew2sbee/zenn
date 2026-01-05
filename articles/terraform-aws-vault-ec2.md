---
title: "[AWS]AWS VaultとTerraformでEC2を作成する" # 記事のタイトル
emoji: "💨" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["aws", "awsvault", "初心者向け", "powerShell", "terraform"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## はじめに

Terraform で AWS のサービスを管理したく、`AWS Vaultコマンド`で EC2 を立ち上げる方法を執筆します

下記記事の作業が完了している前提で解説します。
@[card](https://zenn.dev/aew2sbee/articles/aws-vault-install-to-powershell)

※sandbooks の作業の一環になります。

## 1. AWS CLI をインストール

1. 下記ページにアクセスする
   @[card](https://aws.amazon.com/jp/cli/)

2. Windows の方は、`64ビット`をクリックしインストーラーをダウンロードする
   ![terraform-aws-vault-ec2-step00](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step00.png)

3. ダウンロードされたインストーラーを実行する
   ![terraform-aws-vault-ec2-step02](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step02.png)

4. ✅ を付けて`Next`をクリックする
   ![terraform-aws-vault-ec2-step08](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step09.png)

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

## 2. `~/.aws/config`を設定する

1. `エクスプローラー`から`~/.aws/config`を開きます。
   ![terraform-aws-vault-ec2-step01](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step01.png)

2. 下記のように変更します。

```config
[default]
region=ap-northeast-1 # アジアパシフィック(東京)
output=json

[profile sandbooks]
region=ap-northeast-1 # アジアパシフィック(東京)
mfa_serial=arn:aws:iam::[アカウントID]:mfa/XXXXX # MFAしている端末
role_arn=arn:aws:iam::[アカウントID]:role/XXXXX # Assume Roleを想定しているロール
source_profile=default
```

※`sandbooks`はプロジェクト名になります。
※`~/.aws/credentials`は空のままで大丈夫です。

## 3. `aws-vault`経由で`Access Keys`,`Secret Access Key`を登録する

1. 下記コマンドを実行し、`Access Keys`、`Secret Access Key`を登録する

```powerShell
aws-vault add default
```

実行結果: `Access Keys`,`Secret Access Key`の入力を求められます。

```powerShell
PS C:\Users\Users\Work\sandbooks-infra> aws-vault add default
Enter Access Key ID: AKXXXXXXXXXXXXXXXXXX
Enter Secret Access Key: ****************************************
Added credentials to profile "default" in vault
```

※事前に`Access Keys`,`Secret Access Key`を設定しておく必要があります
:::message
`credentials`への`Access Keys`、`Secret Access Key`記述が不要になる
:::

2. MFA 認証ができるのか確認するために、下記コマンドを実行する

```powerShell
aws-vault exec sandbooks -- aws s3 ls
```

実行結果: MFA 認証で登録しているデバイスに表示されている**6 桁**の番号を入力する

```powerShell
PS C:\Users\Users\Work\sandbooks-infra> aws-vault exec sandbooks -- aws s3 ls
Enter MFA code for arn:aws:iam::[アカウントID]:mfa/iPhone: 706865
```

3. `Sessions`に認証成功が記録されているか確認する

```powerShell
aws-vault ls
```

実行結果: `58m37s`まで`Sessions`が残っているみたいですね

```powerShell
PS C:\Users\Users\Work\sandbooks-infra> aws-vault ls
Profile                  Credentials              Sessions
=======                  ===========              ========
default                  default                  -
sandbooks                sandbooks                sts.AssumeRole:58m37s
```

## 4. Terraform ファイルの作成

1. ファイル構成は下記の通りにします

```bash
.
├── aws.tf
└── variables.tf
```

```tf: aws.tf
provider "aws" {
  region = var.region
}

resource "aws_instance" "example" {
  ami = lookup(var.amis, var.region)
  instance_type = var.instance_type
}
```

```tf: variables.tf
variable "region" {
  description = "AWS region to host your network"
  default     = "ap-northeast-1"
}

variable "instance_type" {
  default = "t2.micro"
}

variable "amis" {
  default = {
    "ap-northeast-1" = "ami-04beabd6a4fb6ab6f"
  }
}
```

:::message
`ami-04beabd6a4fb6ab6f`は、region やアップデートの影響で使えない場合がありますので、EC2 に直接確認してください
:::

## 5. terraform で EC2 を作成する

1. 下記コマンドを実行する

```powerShell
aws-vault exec sandbooks -- terraform init
```

実行結果

```powerShell
PS C:\Users\Users\Work\sandbooks-infra> aws-vault exec sandbooks -- terraform init
Enter MFA code for arn:aws:iam::439798504435:mfa/iPhone: 406982

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

```powerShell
aws-vault exec sandbooks -- terraform plan
```

実行結果：Error が発生しなかったので、問題がなさそうですね。

```powerShell
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

```powerShell
aws-vault exec sandbooks -- terraform apply
```

実行途中で`Only 'yes' will be accepted to approve.`と尋ねられるので、`yes`と入力する

```powerShell
Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes
```

実行結果: `i-0122846cb479c3fc6`が作成された

```powerShell
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
aws_instance.example: Creation complete after 32s [id=i-0122846cb479c3fc6]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

## 6. AWS コンソールで作成できているか確認する

1. AWS コンソールにログインする
2. EC2 を作成した`リージョン`に切り替える
3. `EC2 ダッシュボード`にアクセスし、先ほど作成した`i-0122846cb479c3fc6`がある確認する
   :::message alert
   **必要な時だけ起動してください!**
   **インスタンスを起動後にサーバーが起動する=料金の発生する!!**
   :::

![terraform-aws-vault-ec2-step10](/images/articles/terraform-aws-vault-ec2/terraform-aws-vault-ec2-step10.png)

## おわりに

Windows と Terraform の相性が悪く、環境構築に時間がかかりました。
MacOS ならもっと楽にできそうです。
