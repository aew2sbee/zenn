---
title: "[セキュリティ対策] 安全なパスワードにするには？" # 記事のタイトル
emoji: "🔐" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['セキュリティ', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）

---
## はじめに



@[card](https://www.sbcr.jp/product/4797393163/)


### 結論
:::message
下記条件を満たすと安全性が高まります。

1. パスワードの桁数を`8桁以上`にする
2. パスワードに`英字、数値、記号`を 1 文字以上含める
3. パスワードに`ユーザー情報に類似した情報`を含めない
4. パスワードに`パスワード辞書に載っていそうな単語`を含めない

:::

## 1. パスワードの桁数を`8桁以上`にする

組み合わせ数が多いと、パスワードを特定するのが困難になります。
組み合わせ数の計算方法は下記のとおりです。

> パスワードの組み合わせ数 = 文字の種類 ^ 桁数

桁が`4 桁 -> 6 桁 -> 8 桁`と増加すると、組み合わせ数も増加します。

| 文字の種類        | 4 桁 | 6 桁   | 8 桁 |
| ----------------- | ---- | ------ | ---- |
| 数字のみ(10 種類) | 1 万 | 100 万 | 1 億 |

:::message
仮に、1 秒に 1 回ログインを試みて`1億パターン`試みると、`約3.17年`かかります。
:::

![password-step00](/images/articles/secure-web-app-password/password-step00.png)

## 2. パスワードに`英字、数値、記号`を 1 文字以上含める

組み合わせ数が多いと、同様にパスワードを特定するのが困難になります。

文字の種類が`10 種類 -> 26 種類 -> 2 種類 -> 94 種類 `と増加すると、組み合わせ数も増加します。

| 文字桁 | 数字のみ(10 種類) | 英小文字(26 種類) | 英数字(62 種類) | 英数字記号(94 種類) |
| ------ | ----------------- | ----------------- | --------------- | ------------------- |
| 8 桁   | 1 億              | 約 2 千億         | 約 220 兆       | 約 6100 兆          |

:::message
仮に、1 秒に 1 回ログインを試みて`約6100兆`パターン試みると、`約1,991,341年`かかります。
:::

![password-step01](/images/articles/secure-web-app-password/password-step01.png)
![password-step02](/images/articles/secure-web-app-password/password-step02.png)
![password-step03](/images/articles/secure-web-app-password/password-step03.png)

参考テーブル
| 文字の種類 | 4 桁 | 6 桁 | 8 桁 |
| ------------------- | ---------- | ---------- | ---------- |
| 数字のみ(10 種類) | 1 万 | 100 万 | 1 億 |
| 英小文字(26 種類) | 約 46 万 | 約 3 億 | 約 2 千億 |
| 英数字(62 種類) | 約 1500 万 | 約 570 億 | 約 220 兆 |
| 英数字記号(94 種類) | 約 7800 万 | 約 6900 億 | 約 6100 兆 |

## 3. パスワードに`ユーザー情報に類似した情報`を含めない

下記情報を含まない

1. パスワードに`氏名(名前、苗字)`を含めない

- 例: パスワード: furuta1234

2. パスワードに`誕生日`を含めない

- 例: パスワード: 20231221

:::message
つまり、password に`氏名(名前、苗字)`や`誕生日`などの情報を含めない
:::

## 4. パスワードに`パスワード辞書に載っていそうな単語`を含めない

パスワード辞書とは？

> 一般的なパスワードや単語、数値、シンボルの組み合わせを含むリストです。サイバー攻撃者はこれらの辞書を使用して、ユーザーアカウントに対する不正なアクセスを試みることがあります。

パスワード辞書攻撃とは？

> 辞書中の単語や一般的なパスワードの組み合わせを試行し、アカウントにアクセスしようとする手法です。攻撃者は一般的なパスワードや単語、名前、日付などを網羅的に試行し、弱いパスワードを持つアカウントを見つけ出すことを目指します。

:::message
つまり、パスワード に`password`という推測が容易な言葉をを含めない
:::