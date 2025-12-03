---
title: "[bash] シェルスクリプトでgcloudが実行できない" # 記事のタイトル
emoji: "🐼" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["bash", "gcloud", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

社内の作業効率化の取り組みで **.sh ファイルで gcloud が実行できない現象** が発生しました。
原因は、.env の改行コードでした。
その解消方法を紹介します。

## 再現

### 1. シェルスクリプトと.env ファイルを作成する

> .env ファイルに定義されたバケット名を読み取る
> ローカルにあるファイルを storage にアップロードする

```sh:script.sh
#!/bin/sh

# .envファイルのパス
ENV_FILE=".env"

# .envファイルの存在確認
if [ -f "$ENV_FILE" ]; then
    # .envファイルを読み込んで環境変数に設定
    export $(cat $ENV_FILE | xargs)
else
    echo "Error: $ENV_FILE が見つかりません。"
    exit 1
fi

gcloud storage cp --recursive -n files/* gs://$GCP_BUCKET_NAME/
```

```.env
GCP_BUCKET_NAME="sample_bucket"
```

### 2. シェルスクリプトを実行する

```bash
bash script.sh
```

実行結果を確認します。

```bash
$ bash script.sh
'RROR: (gcloud.storage.cp) HTTPError 400: Invalid bucket name: 'sample_bucket
```

もちろん、GCP 上に sample_bucket という bucket 名は存在するのに、
存在しないと Error になる

## 原因

:::message alert
原因：.env ファイルの改行コードが CRLF になっている事が原因でした。
CRLF を LF に変換する必要があります！
:::

## 解決

:::message
**bash script.sh** を実行する前に**dos2unix .env**を実行する
:::
dos2unix: DOS や Windows 環境で作成されたテキストファイルの CRLF を Unix/Linux 環境で使われる LF に変換する事が出来ます

```bash
dos2unix .env
bash script.sh
```

実行結果を確認します。

```bash
$ bash script.sh
Copying file://files\images\sampleimage.png to gs://sample_bucket/images/sampleimage.png

Average throughput: 1.2MiB/s
```

問題なくシェルスクリプトが実行出来ました！
