---
title: '[TypeScript] MicrosoftのCoding guidelinesの要約' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け', 'コーディング規約'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**** をまとめております。

:::details 参考資料
@[card](https://github.com/microsoft/TypeScript/wiki/Coding-guidelines)
:::

:::message alert
**注意事項**

これらはTypeScriptへの貢献者のためのコーディングガイドラインです。
これはTypeScriptコミュニティのための指示的なガイドラインではありません。
これらのガイドラインは、TypeScriptプロジェクトのコードベースへの貢献者のためのものです。
私たちはチームの一貫性のために多くのガイドラインを選びました。
あなた自身のチームのためにこれらを採用することは自由です。再度言いますが、

これはTypeScriptコミュニティのための指示的なガイドラインではありません。


> --- 原文 ---
> These are Coding Guidelines for Contributors to TypeScript.
> This is NOT a prescriptive guideline for the TypeScript community.
> These guidelines are meant for contributors to the TypeScript project's codebase.
> We have chosen many of them for team consistency. Feel free to adopt them for your own team.
>
> AGAIN: This is NOT a prescriptive guideline for the TypeScript community
:::

## 結論

:::message
**ジェネリクス関数**とは、データ型を引数のように扱う関数のこと
**型引数(type argument)**=データ型を引数のように扱う

```ts
const 関数名 = <関数内で扱うデータ型>(引数名: 関数内で扱うデータ型[]) => 処理;
```

:::



## 🔷命名規則
### 1. 型名はパスカルケース（PascalCase）を使う
```diff ts
- type userInfo
+ type UserInfo
```













### ✅ ブランチ名候補

1. `feature/ts-coding-guidelines-summary`
2. `feature/typescript-style-guide-article`
3. `feature/ts-team-contribution-guide`
4. `feature/write-ts-coding-rules-post`
5. `feature/article-typescript-coding-practices`
6. `feature/blog-ts-coding-conventions`

もし日本語寄りの命名が良ければ、以下のような案もあります：

- `feature/ts-コーディング規約まとめ`
- `feature/記事作成-ts規約`

どのような記事プラットフォーム（Qiita、Zenn、社内ブログなど）を想定していますか？