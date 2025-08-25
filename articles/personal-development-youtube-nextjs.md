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

### 2. 「Bot くん」が累計時間の通知と褒めてくれる

![chat-bot](/images/articles/personal-development-youtube-nextjs/chat-bot.png)

チャット欄に「end」のみを入力すると、モデレーターの「Bot くん」からコメントが貰えます

- これまでの配信での集中した **累計時間**
- **褒め言葉** (ex. 素晴らしい努力ですね, 熱心に取り組まれていますね)

### 3. 「Monthly Challenge」でみんなで 1 つの挑戦!!

![monthly-challenge](/images/articles/personal-development-youtube-nextjs/monthly-challenge.png)

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
とりあえず、動く Web アプリを作成してから色々考える → 作り直すを繰り返す

### 2. 勉強配信の中で機能的な配信が少なかった(ブルーオーシャン)

たまたま、`Twitch`でコーディング配信（HTML/CSS）をやっている海外の方がいました。
その配信には 10 人程度の視聴者が集まり、「意外とマニアックな配信を見る人もいるんだな」と衝撃を受けました。

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

### 1. 視聴者には、勉強や作業に専念できるように**ユーザーから操作を最小限にする**

Chat 欄に入力できるコマンドを増やせば、出来る事の幅が増えます。
(ex. 豆知識を教えて!, 今日の天気を教えて!)
しかし、あえてやらなかったです。

この配信は「いつもの勉強が「楽しみ!!」に変える配信」なので
コマンドで遊んで本質の勉強をしないという状態を望まない。
また、他の視聴者の邪魔にならないようにも注意を払いました!

### 2. 勉強が継続する仕組みを導入

![cycle](/images/articles/personal-development-youtube-nextjs/cycle.png)

多くの方に長く この配信を活用して欲しいと考えております。
そこで[「大人のやる気ペン」](https://www.kokuyo-st.co.jp/stationery/otonayarukipen/index.html)のサイクルを参考しました。

:::message
勉強 → データの可視化 →Bot くんから褒められる...(繰り返し)
:::

上記のサイクルを実現できるように各機能をデザインしました!


### 3. 視聴者同志の競争性を生まないようにする

## 🌱 開発工夫ポイント

## 🌱 悩んだけど、実装しなかった機能案

### 1. 24 時間配信/無人配信

「24 時間配信/無人配信」を実装すれば、いくつかメリットがあります。

:::message

- 視聴者はいつでも利用できる
- Youtube の露出が増えて登録者も伸びやすい

:::

しかし、個人的な方針と反すると思い実装しませんでした。
理由は下記の通りです。

:::message alert

- 今後 配信で求められる要素は、「無機質な配信」よりも「人間らしさな配信」
- 無人による治安悪化（チャット欄が荒れるし、監視/保守する人がいない）
- 寝ている時間も配信をすると、アプリとか配信が安定しているか不安になる

:::

### 2. 視聴者の週間/月間の時間を Bot くん通知する

その週や月間をどのぐらい頑張ったのかを視聴者に教えたいと思いましたが、
次の週や来月になるとリセットされるは、上記のサイクルから抜けるプロセルになるため
あえて実装しなかったです。

### 3. 視聴者の週間/月間のグラフ化

こちらも同様に頑張りをグラフ化し、視聴者に伝えたかったですが、

## 🌱 技術選定

### Next.js

@[card](https://nextjs.org/)

### TypeScript

@[card](https://www.typescriptlang.org/)

### Tailwind CSS

@[card](https://tailwindcss.com/)

### SQLite

@[card](https://sqlite.org/)

## 🌱 最後に

2025 年の年内で**登録者 1000 人**を目指しております!!

本チャンネルに「ご興味がある方」や「一緒に勉強をしてくれる方」はチャンネル登録をよろしくお願いいたします!!

また、収益化したら、高くてなかなか買えない技術の書籍(O'Reilly 本など)を買う経費にしたいと考えております。(私のおこづかいは 5,000 円/月)

![analytics](/images/articles/personal-development-youtube-nextjs/analytics.png)
_2025/08/25 時点の全期間のアナリティクス画像です。_

:::message
**概要**

- 視聴回数：1.7 万回
- 総再生時間：3,992 時間

:::

## 🌱 参考にしたサービス

@[card](https://www.kokuyo-st.co.jp/stationery/otonayarukipen/index.html)
@[card](https://www.nintendo.com/jp/ring/index.html)
