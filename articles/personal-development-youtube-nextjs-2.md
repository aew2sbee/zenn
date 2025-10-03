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
「データ更新日」も記載し**いつ更新された情報なのか？** を明示するようにしました。

### 2. 過去 7 日間

![weekly-image](/images/articles/personal-development-youtube-nextjs-2/weekly-image.png)

過去 7 日間の「集中日数」「7日平均」「最高記録」の概要の表示
ページネーション機能で`「←前週 」`,  `「次週→ 」`さらに過去の週次のデータを確認することできます。

### 3. 過去 30 日間

![monthly-image](/images/articles/personal-development-youtube-nextjs-2/monthly-image.png)
過去 30 日間の「集中日数」「30日平均」「最高記録」の概要の表示
ページネーション機能で`「←前月 」`,  `「次月→ 」`さらに過去の月次のデータを確認することできます。


## 🌱 コンセプト


:::message

なるべく**無料**でかつ**運用の手間がほぼ 0**になるように設計しました。

:::


### 1. 料金
YouTubeが収益化もまだなので、サーバー代を抑えるために`GitHub Page`を活用しました。
レポジトリが`public`である条件があります。

@[card](https://docs.github.com/ja/pages/getting-started-with-github-pages/creating-a-github-pages-site)


また、`GitHub Page`は静的ページ(HTML,CSS,JS)しか利用出来ません。
そのため、`Next.JS`の`Static Site Generation (SSG)`を活用してこの条件をクリアしました。

> `Static Site Generation (SSG)`とは、ビルド(`npm run build`)時にHTMLとCSSを生成する機能

@[card](https://nextjs.org/docs/pages/building-your-application/rendering/static-site-generation)


`計測アプリ`と`アナリティクスアプリ`で共通のDBを持つ必要があります。
以前までは、ローカルで`SQLite`でデータを管理しておりました。
今回`Supabase`を活用しました。

無料枠だと、**データベース容量：500 MB**らしいです。

今回のアプリの場合、**数千〜1万以上のユーザーを1年間分**が記録できそうなので、
無料枠で活用できそうです。

@[card](https://supabase.com/)


### 2. 運用の手間


`アナリティクスアプリ`にデータを追加して、毎日ビルドを必要があります。
そこで`GitHub Action`を活用しました。

`GitHub Action`の大まかな流れ
1. 日本時間の毎朝4時に`GitHub Action`が起動する
2. `GitHub Action`が`Supabase`に昨日のデータだけを取得する
3. 用意したスクリプトを通じて昨日のデータを専用のJSON形式に変更する
4. `JSONファイル`を`main`ブランチに自動pushする
5. 自動push後、SSGで静的ページを作成
6. 静的ページを`GitHub Page`にデプロイする

@[card](https://docs.github.com/ja/actions/get-started/understand-github-actions)
