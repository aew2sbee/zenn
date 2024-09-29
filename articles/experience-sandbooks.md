---
title: "[体験談] 未完成な書籍貸出Webアプリ" # 記事のタイトル
emoji: "🥝" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["初心者向け", "個人開発", "nextjs", "react", "django"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに
この記事では、社内の有志メンバーで**社内図書館アプリ**を開発しそこで得た体験談をまとめております。

![top-page](/images/articles/experience-sandbooks/top-page.png)
![book-deteils](/images/articles/experience-sandbooks/book-deteils.png)

## 背景
なぜ？このPJが発足された理由は、`社員のスキルアップ`と`社内図書館の運用管理コストダウン`

|  ツール名  |  用途  |
| ---- | ---- |
|  Microsoft Teams  |  貸出者同士のコミュニケーションツール  |
|  Microsoft Form  |  借用者からの貸し出し申請フォーム  |
|  Microsoft Outlook  |  貸出者と借用者のコミュニケーションと[クリックポスト](https://clickpost.jp/)の共有  |
|  Google カレンダー  |  貸出書籍の返却管理  |
|  [リブライズ](https://librize.com/ja)  |  書籍情報管理アプリ  |
|  クリックポスト  |  書籍輸送  |


## 結論
:::message
メッセージをここに
:::


## どんなことができるWebアプリ？

## 技術選定
### フロントエンド
|  技術名  |  選定理由  |
| ---- | ---- |
| [TypeScript](https://www.typescriptlang.org/)  |  現職で使用言語のためスキルアップ  |
| [Redux](https://redux.js.org/)   |  TD  |
| [Next.js](https://nextjs.org)   |  TD  |
| [tailwindcss](https://tailwindcss.com/)   |  `Next.js`をインストール時に  |

### バックエンド

|  技術名  |  選定理由  |
| ---- | ---- |
| [Python](https://www.python.org/)  |  - 前職で`Python`の実務経験(2年)<br>- 他のメンバーにも経験者あり  |
| [Django REST framework](https://www.django-rest-framework.org/)   |  - `Django`で一度実装したかった  |

### その他


## ページ遷移図

:::details 書籍情報追加フロー
```mermaid
graph TB
    V[開始] -->A(バーコードリーダーで書籍のISBNコードを取得)
    A --> B(ISBNコードからGoogle Books APIsで書籍情報を取得)
    B --> C(Webアプリ上に書籍情報を表示)
    C --> D{書籍情報を登録}
    D -->|登録する| F{書籍マスターに既にデータが存在するか確認}
    D -->|登録しない| Z[終了]
    F -->|存在しない| H(書籍マスターに登録)
    F -->|存在する| I(Webアプリ上に2冊目以降として追加するか設問)
    I -->|登録する| L(書籍情報のみ追加)
    I -->|登録しない| Y[終了]
    H --> K(書籍情報に登録)
    K --> W[終了]
    L --> X[終了]
```
:::



## このWebアプリで力を入れた箇所

### 1. アイコンを活用し視覚的に操作方法をイメージしやく
### 2. 貸出状態がわかりやすいように表示する
`現在 貸出されている書籍はありません`

![mypage](/images/articles/experience-sandbooks/mypage.png)
![admin-page](/images/articles/experience-sandbooks/admin-page.png)

### 3. JWT認証システムでログイン機能を実装
### 4. Atomic Designの導入
### 5. ページネーションの導入
### 6. 冗長なテーブル構造にならないような工夫
### 7. 実装コストの軽減案の検討


## テーブル設計
### 書籍マスターテーブル
|  カラム名  |  用途  |
| ---- | ---- |
|  ID  |  TD  |
|  登録日  |  TD  |
|  更新日  |  TD  |
|  書籍タイトル  |  TD  |
|  書籍概要  |  TD  |
|  出版社  |  TD  |
|  発売日  |  TD  |
|  ISBNコード  |  TD  |
|  書影  |  TD  |

### 書籍情報テーブル
|  カラム名  |  用途  |
| ---- | ---- |
|  ID  |  TD  |
|  登録日  |  TD  |
|  更新日  |  TD  |
|  書籍マスターID  |  TD  |

### 貸出管理テーブル

|  カラム名  |  用途  |
| ---- | ---- |
|  ID  |  TD  |
|  登録日  |  TD  |
|  更新日  |  TD  |
|  利用者  |  TD  |
|  貸出対象  |  TD  |
|  貸出日  |  TD  |
|  返却日  |  TD  |
|  返却予定日  |  TD  |
|  貸出状態  |  TD  |
|  手段  |  TD  |

### ユーザーテーブル
|  カラム名  |  用途  |
| ---- | ---- |
|  ID  |  TD  |
|  登録日  |  TD  |
|  更新日  |  TD  |
|  氏名  |  TD  |
|  メールアドレス  |  TD  |
|  住所  |  TD  |
|  郵便番号  |  TD  |

## 今回の学び/反省
### 1. カタチにするを優先した
### 2. 


### 苦労


:::details 参考資料
[[基礎編]React Hooks + Django REST Framework API でフルスタックWeb開発
](https://www.udemy.com/share/103nIU3@Qctp7EIJRFkDrweoOxhKeg1QWpJ7x5UocVsa983X_7v5nbzgUclAMRzEW82N9TiXLA==/)
[[Instagramクローン編] React Hooks + Django Restframework
](https://www.udemy.com/share/104jEo3@0zqcHITGuuVkMMj7aH-z_DK_jTjJopm4A_6U-HoAUoL_jrR6Ol5bMdJTF2yYCbDMeQ==/)
[Nextjs + Tailwind CSS + Django REST Framework で学ぶモダンReact開発](https://www.udemy.com/share/1046vI3@VJq_k1x_RTTI5o3mpX8tqOUxAvZirkcibpfV3Z34wEEsGtsGRqRqvr4bEfmyOadXgA==/)
@[card](https://gihyo.jp/book/2022/978-4-297-12916-3)
[Django REST FrameworkでJWT認証システム構築](https://zenn.dev/hathle/books/drf-auth-book)
[Django REST Framework + NextJS + Stripeサブスク有料会員サイト構築](https://zenn.dev/hathle/books/next-drf-membership-book)
:::
