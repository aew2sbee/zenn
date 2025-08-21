---
title: '[個人開発] Youtubeでいつもの勉強が「楽しみ!!」に変える配信にする Web アプリを開発しましたた' # 記事のタイトル
emoji: '📺' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['個人開発', 'youtube', 'nextjs', 'typescript', 'claudecode'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## 🌱 はじめに

今回は、個人開発で Youtube ライブ勉強配信をちょっと楽しくなる Web アプリを開発しました。
その内容についてご紹介します。

![service-sample](/images/articles/personal-development-youtube-nextjs/service-sample.png)
_画面左下に表示されている"Focus tracker"が今回作成したアプリです_

下記リンクから Youtube ライブ配信の様子を確認できます。
@[card](https://www.youtube.com/@aew2sbee)

## 🌱 コンセプト

:::message

**いつもの勉強が「楽しみ!!」に変える配信にする Web アプリ**としております。

:::

いつもの勉強が「楽しみ!!」に変えるには、
**多くの方が抱える悩み TOP3 を解消する**必要があると考えております。

自分も社会人になってからちゃんと勉強するようになりました。
その時からこの TOP3 悩みには苦労しました...

### 1. 勉強の取り掛かりに時間がかかる

![worry-1](/images/articles/personal-development-youtube-nextjs/worry-1.png)

いざ、勉強しようと机に座り、教材を開くが...

- スマホを触っちゃう
- いつも気にならないものが気になる
- ○○ したら勉強する（後回し）

> → Youtube 配信が始めっている or 始まりそうだから

### 2. 集中力が続かない

![worry-2](/images/articles/personal-development-youtube-nextjs/worry-2.png)

いざ、始めたのがいいけど...

- 10 分で飽きたり
- 他の事が気になる

### 3. 勉強が習慣化しない

![worry-3](/images/articles/personal-development-youtube-nextjs/worry-3.png)

一昨昨日も昨日もも勉強出来たけど、3日目の勉強ができない=三日坊主になる

## 🌱 サービス説明

### 1. 勉強/作業に「集中時間」を計測できる

![chat-sample](/images/articles/personal-development-youtube-nextjs/chat-sample.png)

チャット欄に「start」のみを入力すると、約1分後に配信画面の左下に
名前, アイコン, 現在の継続時間が表示されている

## 🌱 開発動機や背景

- 以前から`勉強や教育が楽しくなる`Web アプリを作りたかった

## 🌱 仕様工夫ポイント

1. 視聴者(利用者)には、勉強や作業に専念できるように**ユーザーから操作を最小限にする**

## 🌱 開発工夫ポイント

![youtube-about](/images/articles/personal-development-youtube-nextjs/youtube-about.png)

@[card](https://www.kokuyo-st.co.jp/stationery/otonayarukipen/index.html)
@[card](https://www.nintendo.com/jp/ring/index.html)


## 🌱 悩んだけど、実装しなかった機能案

### 1. 24時間配信/無人配信
