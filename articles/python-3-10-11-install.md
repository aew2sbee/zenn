---
title: "【Python】Python3.10.11をインストールする" # 記事のタイトル
emoji: "🏂" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
社内の有志メンバーでdjango rest frameworkを利用すること決まりました。
Python3.10.11をインストールする方法を解説します。


:::message
※Python3.10.11を選んだ理由は、
2023/08月時点で`security`かつ`End of support`が長いものを選択しました。
![python-install-step01](/images/python-install-step01.png)
:::

## 1. インストーラーをダウンロードする
下記インストーラーのURLをクリックする
※勝手にダウンロードされます
[Python3.10.11インストーラー](https://www.python.org/ftp/python/3.10.11/python-3.10.11-amd64.exe)

## 2. ダウンロードされたインストーラーを実行する
`python-3.10.11-amd64.exe`をダブルクリックする
![python-install-step02](/images/python-install-step02.png)

## 3. インストールを開始する
1. `Add python.exe to PATH`に✅を付ける
2. `Install Now`をクリックし、インストールを開始します
![python-install-step03](/images/python-install-step03.png)
3. インストールが完了するまでしばらく待ちます。
![python-install-step04](/images/python-install-step04.png)

## 4. インストールが完了する
`Close`をクリックし閉じます。
![python-install-step05](/images/python-install-step05.png)


## 5. Pythonのversionを確認する
1. Git Bashを起動する
    検索欄に`git bash`と検索し、**Git Bash**アプリを起動する
    ![sandbooks-git-step01](/images/sandbooks-git-step01.png)
2. 下記コマンドを実行する
```git bash
python -V
```
3. 下記の画像の通り、`Python 3.10.11`である事を確認する
![python-install-step06](/images/python-install-step06.png)

### おわりに
無事にPythonをインストール出来ました。
別PJで別verのPythonを利用している場合は、お互いのPJに影響がないように
DockerやPyenvなどを活用して切り分けてください。