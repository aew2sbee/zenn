---
title: '[個人開発] Youtubeでいつもの勉強が「楽しみ!!」に変える配信にする Web アプリ' # 記事のタイトル
emoji: '📺' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['個人開発', 'youtube', 'nextjs', 'typescript', 'claudecode'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## 🌱 はじめに

今回は、個人開発で Youtube ライブ勉強配信をちょっと楽しくなる Web アプリを開発しました。
その内容についてご紹介します。

こちらは、Youtube の実際の配信画面です。

![service-sample](/images/articles/personal-development-youtube-nextjs/service-sample.png)
_画面左下に表示されている"Focus tracker"が今回作成したアプリです_

下記リンクから Youtube ライブ配信の様子を確認できます。
@[card](https://www.youtube.com/@aew2sbee)

## 🌱 サービスコンセプト

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

:::message
解決案として

- Youtube 配信が始めっているから → 勉強開始に切り替えやすい
  (取り掛かりを自発的 → 受動的にする)
- スマホで Youtube 配信を開いたら、スマホは触れない
  :::

### 2. 集中力が続かない

![worry-2](/images/articles/personal-development-youtube-nextjs/worry-2.png)

いざ、始めたのがいいけど...

- 10 分で飽きたり
- 他の事が気になる

:::message
解決案として

- Youtube 配信では環境音(雨の音、焚き火)と一緒にポモドーロタイマーも併せて起動
  (適度な雑音が集中力を高めてくれます)
- 2 時間も勉強するのではなく、まずは 25 分だけ！ときめて集中してやすい環境の整備
  :::

### 3. 勉強が習慣化しない

![worry-3](/images/articles/personal-development-youtube-nextjs/worry-3.png)

一昨昨日も昨日もも勉強出来たけど、3 日目の勉強ができない= **三日坊主になる**
2 日間勉強しても...

- 誰からも褒めてくれない
- 学校みたいに仲間がいるわけではないため、孤独感を感じやすい

:::message
解決案として

- 今回のアプリでは、計測終了後に「Bot くん」が誰かの代わりに褒めてくれます!!
- 配信画面上には、今集中している仲間が見えるため、孤独感を感じにくい仕組み!!

:::

## 🌱 サービス説明

### 1. 勉強/作業に「集中した時間」を計測できる

![chat-start](/images/articles/personal-development-youtube-nextjs/chat-start.png)

チャット欄に「start」のみを入力すると、約 1 分後に配信画面の左下に
名前, アイコン, 現在の継続時間が表示されています

![chat-end](/images/articles/personal-development-youtube-nextjs/chat-end.png)

勉強/作業が**終了**や**一時休憩**の場合は、同様に
チャット欄に「end」のみを入力すると、約 1 分後に配信画面の左下に
**Focusing => Finished** に切り替わり、計測が終了します。

## 🌱 開発動機や背景

### 1. 以前から`勉強や教育が楽しくなる`Web アプリを作りたい思いがあった

個人開発をすると、下記を 1 から考える必要があります。

- インフラの知識
- DB 周りの知識
- アプリのデザイン案
- 運用コストや料金
- 詳細設計
- データの管理(セキュリティ)など

実際に作業に着手するのにとても億劫になりました。
結局、「作りたいな～。こうゆうのがあればいいのかな～」と思いを時間が過ぎました。

2025 年に`Claude Code`の AI コーディングが登場しました。
(日本語で指示したら Web アプリを作成してくれるようになりました。)

`Claude Code`の登場により、重かった腰が上がりました!!
とりあえず、動く Web アプリを作成してから色々考える→作り直すを繰り返す

### 2. 勉強配信の中で機能的な配信が少なかった(ブルーオーシャン)

たまたま、`Twitch`でコーディング配信（HTML/CSS）をやっている海外の方がいました。
その配信には10人程度の視聴者が集まり、「意外とマニアックな配信を見る人もいるんだな」と衝撃を受けました。

`Youtube`で「勉強配信」って調べると意外と配信されている方もいて
「Study With Me」という文化もそこで知りました。

いくつかの「勉強配信」や「Study With Me」を観ると、`Night bot`や`ポモドーロタイマー`
を表示するだけの配信ばかりで機能的な配信が少ない事を気づきました
※1 `Night bot`は、定数なコメントに対して返答やクーロン処理が主な機能でした。

:::message
ここで`YouTube API`を活用した機能を実装したら、頭一つ抜けた配信になる!!
:::

![victory](/images/articles/personal-development-youtube-nextjs/victory.png)



## 🌱 仕様工夫ポイント

1. 視聴者(利用者)には、勉強や作業に専念できるように**ユーザーから操作を最小限にする**

## 🌱 開発工夫ポイント

![youtube-about](/images/articles/personal-development-youtube-nextjs/youtube-about.png)

@[card](https://www.kokuyo-st.co.jp/stationery/otonayarukipen/index.html)
@[card](https://www.nintendo.com/jp/ring/index.html)

## 🌱 悩んだけど、実装しなかった機能案

### 1. 24 時間配信/無人配信
