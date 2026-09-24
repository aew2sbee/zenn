---
title: "[デザイン] Atomic Designをデジタル庁のサイトで理解する" # 記事のタイトル
emoji: "🎨" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["atomicdesign", "design", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

Atomic Design について『TypeScript と React/Next.js でつくる実践 Web アプリケーション開発』（手島拓也、吉田健人、高林佳稀 著／技術評論社／2022年）から学びました。
開発メンバーへ共有するため執筆します。

@[card](https://gihyo.jp/book/2022/978-4-297-12916-3)

言葉だけでは理解が難しいと感じましたので、デジタル庁のサイトのフッターをサンプルとしてお借りしております。

@[card](https://www.digital.go.jp/)

:::message alert
本記事で使用している画像は、デジタル庁から提供されているものではありません。
デジタル庁ウェブサイト（https://www.digital.go.jp/ ）のフッターを個人で画像として保存し、筆者が枠線と文字を追加した加工画像です。
2023年8月時点のもののため、現在のサイトとは構成が異なります。
:::

### この記事について

- **ゴール**: Atomic Design の階層を、実際のサイトの画面で見分けられるようになる
- **対象読者**: コンポーネント単位で UI を作るフロントエンド開発者、デザインシステムに興味のある方
- **前提知識**: React のコンポーネントと `props` を知っていること

:::message
記事の後半に出てくる実装の指針は、参考書籍で紹介されている **React での実装例**です。
Atomic Design そのものの定義ではありません。
:::

## 🌱 Atomic Design とは

Brad Frost 氏が提唱した、化学の構造（原子 → 分子 → 有機体）になぞらえて **デザインシステム**を構築するための方法論です。

デザインシステムとは、ボタンや色、余白などの UI 部品とその使い方のルールをまとめ、チーム全体で共有する仕組みのことです。
Atomic Design は、その部品を「どの粒度で分けるか」を決めるための考え方にあたります。

:::message
【メリット】デザインを**階層的**に定義することで**一貫性**を保ち**管理しやすい**

たとえばボタンの色を変えたいとき、Atoms のボタンを 1 か所直すだけでサイト全体に反映できます。
:::

次の 5 つの段階で構成されます。

| 階層名 | 説明 | 例 |
| ---- | ---- | ---- |
| Atoms | それ以上分割すると UI として機能しなくなる**最小の要素** | ボタン、アイコン、テキスト |
| Molecules | **複数の Atoms**を組み合わせた、1 つの役割を持つ要素 | テキストとアイコンを組み合わせたボタン |
| Organisms | **Atoms や Molecules**を組み合わせた、それ単体で意味が成立するまとまり | ヘッダー、フッター |
| Templates | ページ全体の**レイアウト**（配置だけを示した設計図） | ワイヤーフレーム |
| Pages | Templates に**実際のコンテンツを流し込んだもの** | ユーザーが見るページ |

:::message
Atoms → Molecules → Organisms は粒度の小さい順ですが、Templates と Pages は大きさの違いではありません。
Pages は Templates に実データを適用したもので、実際のコンテンツでデザインが成立するかを検証する段階です。

また、階層の境界はプロダクトによって変わります（同じ「検索フォーム」でも、原典では Molecules の例として挙げられています）。
正解が 1 つあるわけではないため、チームで基準を合わせることが重要です。
:::

## 🌱 それぞれの役割について

ここからは、各階層をコンポーネントとして実装するときの指針です。

### 1. Atoms

- **内部に状態を持たせない**（自分ではデータを覚えず、渡された内容を表示するだけにする）。
- 文章、色、大きさなどの描画に必要な**パラメータは`props`から受け取る**（`props` は親の部品から子の部品へ渡す値）。
- CSS で親要素の大きさに**依存させない**。
- 画像の Atoms なら、**画像の URL と代替テキスト（alt）**を受け取って表示し、加工や取得の処理は持たせない。

下記は、デジタル庁サイトのフッターの Atoms 要素です。
サイト名、ナビゲーションのリンクテキスト、見出し、SNS アイコンなどが該当します。

![デジタル庁サイトのフッターのAtoms要素](/images/articles/atomic-design-for-digital/Atomic-Design-Atoms.png)
*デジタル庁サイトのフッターの Atoms*

### 2. Molecules

- 基本的に**内部に状態を持たない**。
- 汎用的に使うため必要なデータは**親から**受け取る。
- **1 つの役割**を持った UI にする。
- 複数の Atoms を配置し、**必要なデータを子コンポーネントに渡す**。
- それぞれの位置関係を CSS で指定する。

下記は、デジタル庁サイトのフッターの Molecules 要素です。
「見出し＋日付」で 1 件のトピックを表すまとまりや、ページ上部へ戻るボタンが該当します。

![デジタル庁サイトのフッターのMolecules要素](/images/articles/atomic-design-for-digital/Atomic-Design-Molecules.png)
*デジタル庁サイトのフッターの Molecules*

### 3. Organisms

- サインインフォームなどの**UI コンポーネント**。
- ドメイン知識（そのサービス固有の情報）に依存したデータを受け取る。
  - 例: 「デジタル庁へのアクセス」「新しいトピック一覧」のような、このサイトでしか使わない情報。
- Context（離れた階層の部品からも参照できる共有の値。ログイン状態やテーマ色など）を参照する。
- 見た目：Presentational Component（見た目だけを担当する部品）で実装。
- ロジック：Container Component（データ取得や処理を担当する部品）で実装。

下記は、デジタル庁サイトのフッターの Organisms 要素です。
フッター全体が 1 つの Organisms にあたります。

![デジタル庁サイトのフッターのOrganisms要素](/images/articles/atomic-design-for-digital/Atomic-Design-Organisms.png)
*デジタル庁サイトのフッターの Organisms*

:::message
Presentational Component / Container Component への分割は Atomic Design 自体の定義には含まれず、React での実装例（著者独自の適用方法）です。
また、このパターンを広めた Dan Abramov 氏は React Hooks の登場を受けて、2019 年に元記事へ「この分割方法はもう勧めない」と追記しています。
https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0
:::

### 4. Templates

- Organisms 以下（Organisms・Molecules・Atoms）のコンポーネントを複数配置する。
- コンポーネントを CSS でレイアウトする。

### 5. Pages

- 状態管理や router 関連の処理を行い、API コールなどの副作用（画面を描く以外の処理）を実行する。
- 取得した結果や Context の値を Templates に渡す。

:::message
本記事はフッター（Organisms まで）を題材にしているため、Templates と Pages の図は扱いません。
デジタル庁サイトで言えば、ヘッダー・本文・フッターの配置枠が Templates、そこに実際の記事データを流し込んだものが Pages にあたります。
:::

## 🌱 デジタル庁サイトのフッターを分解してみる

### 元のフッター

![加工前のデジタル庁サイトのフッター](/images/articles/atomic-design-for-digital/Atomic-Design-step00.png)
*加工前：デジタル庁サイトのフッター*

### 階層ごとに囲んだフッター

同じフッターを、Atomic Design の階層で囲んだものです。枠線の色が階層に対応しています。

| 枠線の色 | 階層 | 該当する要素 |
| ---- | ---- | ---- |
| 赤 | Atoms | サイト名、各リンクテキスト、見出し、SNS アイコン、住所 |
| 水色 | Molecules | 「見出し＋日付」で 1 件を表すトピック、ページ上部へ戻るボタン |
| 緑 | Organisms | フッター全体 |

![階層ごとに色分けしたデジタル庁サイトのフッター](/images/articles/atomic-design-for-digital/Atomic-Design-step01.png)
*加工後：階層ごとに囲んだデジタル庁サイトのフッター*

## 🌱 おわりに

Atomic Design の概念に触れ、しっかり使いこなせたら便利だなと思いました。

一方で、フッターを分解するだけでも「どこまでを Molecules とするか」の線引きには判断が入ります。
前述のとおり階層の境界はプロダクトによって変わるため、基準をチームで先に決めておかないと、同じ画面を見ても人によって分類がずれます。
チーム全体で使うには、チーム力がそこそこいるなと感じました。

## 🌱 参考

https://gihyo.jp/book/2022/978-4-297-12916-3
https://atomicdesign.bradfrost.com/chapter-2/
https://www.digital.go.jp/
