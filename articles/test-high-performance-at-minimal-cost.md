---
title: "[テスト] ？" # 記事のタイトル
emoji: '➡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['テスト', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---



## 1. 網羅率(coverage)のみを強く意識しない
テストパターンを作成した際に、テストパターンが十分であるかの指標の為に`網羅率(coverage)`を意識しているPJは多いと思います。
しかし、網羅率(coverage)を強く意識すると逆に開発効率が悪くなり、いつまでたってもリリースができない悪循環になる可能性があります。

例えば、下記コードに対して**入力値を5として返り値が`true`であること**を確認するテストパターンの場合
もちろん、網羅率は`50%`になります。(if分岐を通過した為)
```ts
const isGreaterThanOrEqualToFive = (value: number): boolean => {
  if (value >= 5)
    return true;
  else
    return false;
}
```

しかし、下記コードに対して同様なテストパターンの場合、網羅率は`100%`になります。(if分岐を省略して1行の為)
if文を使わずにシンプル
```ts
const isGreaterThanOrEqualToFive = (value: number): boolean => value >= 5;
```

:::message
つまり、コードの書き方によって`網羅率`を操作する事が可能である

:::

網羅率(coverage)が意味がないのか？
分岐網羅率(branch coverage)の目線で考える

### 2. AAAパターンでテストコードを実装する
:::message
下記英単語の頭文字を取得し`AAAパターン`とする
1. **準備(Arrange)**
2. **実行(Act)**
3. **確認(Assert)**
:::
