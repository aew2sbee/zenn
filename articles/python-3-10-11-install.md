---
title: "[Python] Python3.10.11をインストールする" # 記事のタイトル
emoji: "🐍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

社内の有志メンバーで Django REST framework を利用することに決まりました。
そこで、Python3.10.11 をインストールする方法を解説します。

:::message
Python3.10.11 を選んだのは、2023年8月時点でサポート状況が`security`のバージョンのうち、サポート終了日（End of support）が最も遅いためです。

![python-install-step01](/images/articles/python-3-10-11-install/python-install-step01.png)
:::

:::message alert
Python 3.10 のサポートは、2026年10月に終了します。これから新しくインストールする場合は、より新しいバージョンの利用を検討してください。
なお、3.10.11 は Windows 用のインストーラーが提供されている最後の 3.10 系のバージョンです。
:::

## 🌱 1. インストーラーをダウンロードする

下記のURLをクリックすると、インストーラーのダウンロードがすぐに始まります。

[Python3.10.11 インストーラー](https://www.python.org/ftp/python/3.10.11/python-3.10.11-amd64.exe)

## 🌱 2. ダウンロードされたインストーラーを実行する

`python-3.10.11-amd64.exe`をダブルクリックします。
![python-install-step02](/images/articles/python-3-10-11-install/python-install-step02.png)

## 🌱 3. インストールを開始する

1. `Add python.exe to PATH`に ✅ を付けます。
2. `Install Now`をクリックし、インストールを開始します。
   ![python-install-step03](/images/articles/python-3-10-11-install/python-install-step03.png)
3. インストールが完了するまでしばらく待ちます。
   ![python-install-step04](/images/articles/python-3-10-11-install/python-install-step04.png)

## 🌱 4. インストールが完了する

`Close`をクリックし閉じます。
![python-install-step05](/images/articles/python-3-10-11-install/python-install-step05.png)

## 🌱 5. Python の version を確認する

1. Git Bash を起動します。
   検索欄に`git bash`と入力し、**Git Bash**アプリを起動します（インストール前から開いていた Git Bash では PATH が反映されないため、起動し直してください）。
2. 下記コマンドを実行します。

   ```bash
   python -V
   ```

3. 下記の画像の通り、`Python 3.10.11`であることを確認します。
   ![python-install-step06](/images/articles/python-3-10-11-install/python-install-step06.png)
