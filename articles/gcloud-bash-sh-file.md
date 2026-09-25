---
title: "[bash] .envの改行コード(CRLF)でgcloud storage cpがInvalid bucket nameになる" # 記事のタイトル
emoji: "🐼" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["bash", "gcloud", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

社内の作業効率化の取り組みで、**シェルスクリプトから実行した gcloud コマンドが、バケット名のエラーで失敗する現象**が発生しました。
原因は、.env ファイルの改行コードが CRLF になっていたことでした。
その解消方法を紹介します。

## 🌱 再現

### 1. シェルスクリプトと .env ファイルを作成する

> .env ファイルに定義されたバケット名を読み取る
> ローカルにあるファイルを Cloud Storage にアップロードする

```sh:script.sh
#!/bin/bash

# .envファイルのパス
ENV_FILE=".env"

# .envファイルの存在確認
if [ -f "$ENV_FILE" ]; then
    # .envファイルを読み込んで環境変数に設定
    export $(cat "$ENV_FILE" | xargs)
else
    echo "Error: $ENV_FILE が見つかりません。"
    exit 1
fi

gcloud storage cp --recursive -n files/* "gs://${GCP_BUCKET_NAME}/"
```

```text:.env
GCP_BUCKET_NAME="sample_bucket"
```

### 2. シェルスクリプトを実行する

```bash
bash script.sh
```

実行結果を確認します。

```text
$ bash script.sh
'RROR: (gcloud.storage.cp) HTTPError 400: Invalid bucket name: 'sample_bucket
```

GCP 上に sample_bucket というバケットは存在するにもかかわらず、不正なバケット名（Invalid bucket name）としてエラーになります。
メッセージの先頭が `'RROR` と崩れているのも、後述する原因によるものです。

## 🌱 原因

:::message alert
.env ファイルの改行コードが CRLF になっていることが原因でした。
CRLF を LF に変換する必要があります！
:::

CRLF の .env は、各行の末尾に CR（`\r`）が付いています。

1. `.env` の行は `GCP_BUCKET_NAME="sample_bucket"\r` になっている
2. `xargs` はダブルクォートを取り除くが、`\r` は取り除かない
3. その結果、`GCP_BUCKET_NAME` の値が `sample_bucket\r` になり、不正なバケット名として扱われる

エラーメッセージの表示が崩れるのも、`\r` でカーソルが行の先頭に戻り、閉じ側の `'` が `ERROR` の `E` を上書きするためです。

下記のコマンドで、値の末尾に `\r` が付いているかを確認できます。

```bash
printf '%s' "$GCP_BUCKET_NAME" | od -c
```

:::message
Linux や WSL の bash では、この現象が再現します。
一方、Windows の Git Bash では、コマンド置換 `$(...)` が `\r` を取り除くため、再現しない場合があります。
:::

## 🌱 解決

:::message
`bash script.sh` を実行する前に、`dos2unix .env` を実行します。
:::

dos2unix は、DOS や Windows 環境で作成されたテキストファイルの改行コード CRLF を、Unix/Linux 環境で使われる LF に変換するコマンドです。

```bash
dos2unix .env
bash script.sh
```

dos2unix がインストールされていない場合は、`sudo apt install dos2unix` などでインストールするか、下記のコマンドで代用できます。

```bash
sed -i 's/\r$//' .env
```

実行結果を確認します。

```text
$ bash script.sh
Copying file://files\images\sampleimage.png to gs://sample_bucket/images/sampleimage.png

Average throughput: 1.2MiB/s
```

問題なくシェルスクリプトが実行できました！

## 🌱 再発を防ぐには

.env を Windows で編集するたびに CRLF に戻る可能性があります。次のような対策も有効です。

- スクリプト側で CR を取り除いてから読み込む

```bash
export $(grep -v '^#' "$ENV_FILE" | tr -d '\r' | xargs)
```

- `.gitattributes` に `.env text eol=lf` や `*.sh text eol=lf` を書き、Git で LF に固定する
- エディタの改行コードの設定を LF にする

:::message alert
`export $(cat .env | xargs)` という読み込み方は、値に空白が含まれると分割される、コメント行があるとエラーになる、などの制約があります。
また、秘密情報を含む .env はリポジトリにコミットしないように、`.gitignore` に追加してください。
:::
