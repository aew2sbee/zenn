---
title: '[個人開発] 続編-YouTubeで集中した時間を振り返るWebアプリ' # 記事のタイトル
emoji: '🏅' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['個人開発', 'youtube', 'nextjs', 'typescript', 'claudecode'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## 🌱 はじめに

@[card](https://zenn.dev/aew2sbee/articles/personal-development-youtube-nextjs)

こちらのアプリの姉妹アプリのお話になります。

今回も個人開発で `アナリティクスWeb アプリ`を開発しました。
その内容についてご紹介します。

![Image](/images/articles/personal-development-youtube-nextjs-2/Image.png)
_実際の私のアナリティクスの内容になります_

下記リンクから アナリティクスの内容を確認できます。

@[card](https://aew2sbee.github.io/UCDV95uUZlqOmxJ0hONnoALw/)

## 🌱 サービス説明

:::message

アナリティクスとして確認できる内容は下記の通りです。

1. 全期間
2. 過去 7 日間
3. 過去 30 日間

:::

### 1. 全期間

![all-image](/images/articles/personal-development-youtube-nextjs-2/all-image.png)

シンプルに何のデータがわかるように直観的な`アイコン`や`色`,`テキスト`を選定しました。
「データ更新日」も記載し**いつ更新された情報なのか** を明示するようにしました。

### 2. 過去 7 日間

![weekly-image](/images/articles/personal-development-youtube-nextjs-2/weekly-image.png)

過去 7 日間の「集中日数」「7 日平均」「最高記録」の概要の表示
ページネーション機能で`「←前週 」`, `「次週→ 」`
さらに過去の週次のデータを確認できます。

### 3. 過去 30 日間

![monthly-image](/images/articles/personal-development-youtube-nextjs-2/monthly-image.png)
過去 30 日間の「集中日数」「30 日平均」「最高記録」の概要の表示
ページネーション機能で`「←前月 」`, `「次月→ 」`
さらに過去の月次のデータを確認できます。

## 🌱 コンセプト

:::message

下記の要件を満たせるように設計しました。

- 運用コスト: ほぼゼロ
- 運用の手間：ほぼなし
- シンプルな設計

:::

### 1. 運用コスト

YouTube が収益化もまだなので、サーバー代を抑えるために`GitHub Page`を活用しました。
レポジトリが`public`である条件があります。

@[card](https://docs.github.com/ja/pages/getting-started-with-github-pages/creating-a-github-pages-site)

また、`GitHub Page`は静的ページ(HTML,CSS,JS)しか利用出来ません。
そのため、`Next.JS`の`Static Site Generation (SSG)`を活用してこの条件をクリアしました。

> `Static Site Generation (SSG)`とは、ビルド(`npm run build`)時に HTML と CSS を生成する機能

@[card](https://nextjs.org/docs/pages/building-your-application/rendering/static-site-generation)

`計測アプリ`と`アナリティクスアプリ`で共通の DB を持つ必要があります。
以前までは、ローカルで`SQLite`でデータを管理しておりました。
今回`Supabase`を活用しました。

無料枠だと、**データベース容量：500 MB**らしいです。

今回のアプリは 1 年分で数千〜1 万ユーザー規模を記録できる見込みであり、無料枠で運用可能と判断した。

@[card](https://supabase.com/)

### 2. 運用の手間

`アナリティクスアプリ`にデータを追加して、毎日ビルドする必要があります。
そこで`GitHub Action`を活用しました。

`GitHub Action`の大まかな流れ

1. 日本時間の毎朝 4 時に`GitHub Action`が起動する
2. `GitHub Action`が`Supabase`に昨日のデータだけを取得する
3. 用意したスクリプトを通じて昨日のデータを専用の JSON 形式に変更する
4. `JSONファイル`を`main`ブランチに自動 push する
5. 自動 push 後、SSG で静的ページを作成
6. 静的ページを`GitHub Page`にデプロイする

@[card](https://docs.github.com/ja/actions/get-started/understand-github-actions)

### 3. シンプルな設計

`GitHub Page`は静的ページ(HTML,CSS,JS)なので、ログイン/ログアウト機能を実装できません。
しかし、他の視聴者の情報を簡単に閲覧できてしまうのはサービスとして不完全である。

そこで、YouTube 側で発行している`チャンネルID`を活用しました。
チャンネル ID はユニークな情報であり、私が管理する必要がないのもいいですね！

また、チャンネル ID は私と本人しか調べることができないので、認証情報として適していると判断した。

チャンネル ID はユニークであり、運営側で管理する必要がない点も利点である。

※チャンネル ID は、チャンネルのマイページから調べる事ができます。
※登録者がいるチャンネルなら、YouTube 上で検索してマイページを見つけることができる

チャンネル ID を用いることで、他人に閲覧されず自分だけが自分のデータを参照できる。
