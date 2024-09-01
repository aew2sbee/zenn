---
title: '[デザイン] Atomic Designをデジタル庁のサイトで理解する' # 記事のタイトル
emoji: '🏋️‍♀️' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['atomicdesign', 'design', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに

Atomic Design について下記の書籍から学びました。
開発メンバーに共有するために執筆します。
@[card](https://gihyo.jp/book/2022/978-4-297-12916-3)

言葉だけでは、理解が難しいと感じましたので、
デジタル庁のサイトのフッターをサンプルとしてお借りしております。
@[card](https://www.digital.go.jp/)
:::message alert
本記事で使用しているイラストはデジタル庁から提供されている画像ではありません。
個人でサイトを画像として保存しイラストを追加した自作画像となります。
:::

## Atomic Design とは

> デザインシステムを構築するための方法論です。

:::message
【メリット】デザインを**階層的**に定義することで**一貫性**を保ち**管理しやすい**
:::

5 つの階層で構成される(最小順)
| 階層名 | 説明 | 例 |
| ---- | ---- | ---- |
| Atoms | **最小の要素** これ以上分割できない | ボタン、アイコン、テキスト |
| Molecules | **複数の Atoms**を組み合わせた要素 | テキストとアイコンを組み合わせたボタン |
| Organisms | Molecules よりもより**具体的な要素** | ヘッダー、フッター、検索フォーム、カード |
| Templates | ページ全体の**レイアウト** | ワイヤーフレーム |
| Pages | **ページそのもの** | ユーザーが見るページ |

## それぞれの役割について

### 1. Atoms

- 基本的に**状態**や**振る舞いを持たない**
- 文章、色、大きさなどの描画に必要な**パラメータは`props`から受け取る**
- CSS で親要素の大きさに**依存させない**
- 画像の Atoms なら**URL のみ**を渡して表示する

下記は、デジタル庁サイトのフッターの Atoms 要素になります
![Atomic-Design-Atoms](/images/articles/atomic-design-for-digital/Atomic-Design-Atoms.png)
_デジタル庁サイトのフッターの Atoms_

### 2. Molecules

- 基本的に**状態**や**振る舞いを持たない**
- 汎用的に使うため必要なデータは**親から**受け取る
- **1 つの役割**を持った UI にする
- 複数の Atoms を配置し、**必要なデータを子コンポーネントに渡す**
- それぞれの位置関係を CSS で指定する

下記は、デジタル庁サイトのフッターの Molecules 要素になります
![Atomic-Design-Molecules](/images/articles/atomic-design-for-digital/Atomic-Design-Molecules.png)
_デジタル庁サイトのフッターの Molecules_

### 3. Organisms

- サインフォームなどの**UI コンポーネント**
- ドメイン知識に依存したデータを受け取る
- Context の参照
- 見た目：Presentatational Component で実装
- ロジック：Container Component で実装
  ![Atomic-Design-Organisms](/images/articles/atomic-design-for-digital/Atomic-Design-Organisms.png)
  _デジタル庁サイトのフッターの Organisms_

### 4. Templates

- 複数の Organisms 以下のコンポーネントを配置する
- コンポーネントを CSS でレイアウトする

### 5. Pages

- Templates に状態管理、router 関係の処理、API コールなどの副作用の実行
- Context に値を渡す

## デジタル庁のサイトの Atomic Design はどうなっているか

#### 加工前：デジタル庁のフッター

![Atomic-Design-step00](/images/articles/atomic-design-for-digital/Atomic-Design-step00.png)

#### 加工後：デジタル庁のフッター

![Atomic-Design-step01](/images/articles/atomic-design-for-digital/Atomic-Design-step01.png)

## おわりに

Atomic-Design の概念に触れてしっかり使いこなせたら便利だなと思いました。
チーム全体で使うには、チーム力がそこそこいるなと感じました。
