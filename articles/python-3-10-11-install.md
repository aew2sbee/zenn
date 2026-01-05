---
title: "[Python] Python3.10.11をインストールする" # 記事のタイトル
emoji: "🐍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## はじめに

社内の有志メンバーで django rest framework を利用することに決まりました。
Python3.10.11 をインストールする方法を解説します。

:::message
※Python3.10.11 を選んだ理由は、
2023/08 月時点で`security`かつ`End of support`が長いものを選択しました。
![python-install-step01](/images/articles/python-3-10-11-install/python-install-step01.png)
:::

## 1. インストーラーをダウンロードする

下記インストーラーの URL をクリックする
※勝手にダウンロードされます
[Python3.10.11 インストーラー](https://www.python.org/ftp/python/3.10.11/python-3.10.11-amd64.exe)

## 2. ダウンロードされたインストーラーを実行する

`python-3.10.11-amd64.exe`をダブルクリックする
![python-install-step02](/images/articles/python-3-10-11-install/python-install-step02.png)

## 3. インストールを開始する

1. `Add python.exe to PATH`に ✅ を付ける
2. `Install Now`をクリックし、インストールを開始します
   ![python-install-step03](/images/articles/python-3-10-11-install/python-install-step03.png)
3. インストールが完了するまでしばらく待ちます。
   ![python-install-step04](/images/articles/python-3-10-11-install/python-install-step04.png)

## 4. インストールが完了する

`Close`をクリックし閉じます。
![python-install-step05](/images/articles/python-3-10-11-install/python-install-step05.png)

## 5. Python の version を確認する

1. Git Bash を起動する
   検索欄に`git bash`と検索し、**Git Bash**アプリを起動する
2. 下記コマンドを実行する

```git bash
python -V
```

3. 下記の画像の通り、`Python 3.10.11`である事を確認する
   ![python-install-step06](/images/articles/python-3-10-11-install/python-install-step06.png)
