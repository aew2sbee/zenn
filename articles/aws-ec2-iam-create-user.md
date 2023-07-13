---
title: "【AWS】IAMで読み取り専用ユーザーを作成する" # 記事のタイトル
emoji: "🤺" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["aws", "iam"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
社内の有志メンバーでの活動でIAMで読み取り専用ユーザー作成する機会がありました。
この経験は、何度もある事ではないと思いますので、備忘録として執筆します。

|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・AWSのIAMについて知らない方  |
|  **伝えたい内容**  |  ・IAMで読み取り専用ユーザーを作成する  |
|  **前提条件**  |  ・AWSのルートユーザー作成済み |


## 1. IAMユーザーを作成する

### 1. IAMに開く
**ルートユーザー**で検索欄に`IAM`と検索し検索結果から**IAM**をクリックします。
![sandbooks-aws-IAM-step01](/images/sandbooks-aws-IAM-step01.png)

### 2. 新規ユーザー作成画面を開く
左側の`ユーザー`を選択し、右側の青色ボタンの`ユーザーを追加`をクリックします。
![sandbooks-aws-IAM-step02](/images/sandbooks-aws-IAM-step02.png)

###  3. ユーザー名と初期パスワードを設定する
1. ユーザー名を入力します
    ※任意の名前を入力します。今回は、`test_user`とします
2. `AWSマネジメントコンソールへのユーザーアクセスを提供する`に✅を付けます。
3. `次へ`ボタンをクリックします。
![sandbooks-aws-IAM-step03](/images/sandbooks-aws-IAM-step03.png)

### 4. ユーザー権限を設定する
1. ポリシーを直接アタッチする
![sandbooks-aws-IAM-step04](/images/sandbooks-aws-IAM-step04.png)
2. `ReadOnlyAccess`を検索欄に入力する
3. `8ページ`まで移動する
4. `ReadOnlyAccess`に✅を付けます。
5. `次へ`ボタンをクリックします。
![sandbooks-aws-IAM-step05](/images/sandbooks-aws-IAM-step05.png)

### 5. 設定した値を確認する
1. 下記項目が指定した値であるか確認する
    - ユーザー名が指定した名前である
    - **許可の概要**に`ReadOnlyAccess`が含まれている
2. `ユーザーの作成`をクリックする
![sandbooks-aws-IAM-step06](/images/sandbooks-aws-IAM-step06.png)

### 6. ユーザー名とパスワードを控える
1. `ユーザーが正常に作成されました`のメッセージを確認する
2. 下記項目をコピーし、ログインのために控える
    - コンソールサインインURL
    - ユーザー名
    - コンソールパスワード
3. `ユーザーリストに戻る`をクリックします。
![sandbooks-aws-IAM-step07](/images/sandbooks-aws-IAM-step07.png)


## おわりに
あとは、新しい作成したユーザーでログインし、初期パスワードの変更を行えば完了です。





