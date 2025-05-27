---
title: '[Python] Python3.10.11をインストールする' # 記事のタイトル
emoji: '🐍' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['python', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

社内の有志メンバーで django rest framework を利用すること決まりました。
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

`Close`をクリックしてインストーラーを閉じます
![python-install-step05](/images/articles/python-3-10-11-install/python-install-step05.png)

## 5. Python の version を確認する

1. **Git Bash**を起動します。
   検索欄に`git bash`と入力し、アプリを起動してください。
   ![sandbooks-git-step01](/images/articles/python-3-10-11-install/sandbooks-git-step01.png)

2. 下記コマンドを実行する

```bash
python -V
```

3. 下記の画像のように、`Python 3.10.11`と表示されることを確認します。  
   ![python-install-step06](/images/articles/python-3-10-11-install/python-install-step06.png)

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！

@[card](https://www.youtube.com/@aew2sbee)
