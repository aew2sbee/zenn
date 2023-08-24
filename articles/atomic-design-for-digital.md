---
title: "【イラスト付き】デジタル庁のサイトでAtomic Designを理解する" # 記事のタイトル
emoji: "🏋️‍♀️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["atomicdesign", "design", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
Atomic Designについて下記の書籍から学びました。
開発メンバーに共有するために執筆します。
@[card](https://www.amazon.co.jp/TypeScript%E3%81%A8React-Next-js%E3%81%A7%E3%81%A4%E3%81%8F%E3%82%8B%E5%AE%9F%E8%B7%B5Web%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E9%96%8B%E7%99%BA-%E6%89%8B%E5%B3%B6-%E6%8B%93%E4%B9%9F/dp/4297129167)


言葉だけでは、理解が難しいと感じましたので、
デジタル庁のサイトのフッターをサンプルとしてお借りしております。
@[card](https://www.digital.go.jp/)
:::message alert
本記事で使用しているイラストはデジタル庁から提供されている画像ではありません。
個人でサイトを画像として保存しイラストを追加した自作画像となります。
:::

## Atomic Designとは
> デザインシステムを構築するための方法論です。

:::message
【メリット】デザインを**階層的**に定義することで**一貫性**を保ち**管理しやすい**
:::

5つの階層で構成される(最小順)
|  階層名  | 説明  | 例  |
| ---- | ---- | ---- |
|  Atoms  |  **最小の要素** これ以上分割できない | ボタン、アイコン、テキスト |
|  Molecules  |  **複数のAtoms**を組み合わせた要素 | テキストとアイコンを組み合わせたボタン |
|  Organisms  |  Moleculesよりもより**具体的な要素** | ヘッダー、フッター、検索フォーム、カード |
|  Templates  |  ページ全体の**レイアウト** | ワイヤーフレーム |
|  Pages  |  **ページそのもの** | ユーザーが見るページ |

## それぞれの役割について
### 1. Atoms
- 基本的に**状態**や**振る舞いを持たない**
- 文章、色、大きさなどの描画に必要な**パラメータは`props`から受け取る**
- CSSで親要素の大きさに**依存させない**
- 画像のAtomsなら**URLのみ**を渡して表示する

下記は、デジタル庁サイトのフッターのAtoms要素になります
![Atomic-Design-Atoms](/images/Atomic-Design-Atoms.png)
*デジタル庁サイトのフッターのAtoms*


### 2. Molecules
- 基本的に**状態**や**振る舞いを持たない**
- 汎用的に使うため必要なデータは**親から**受け取る
- **1つの役割**を持ったUIにする
- 複数のAtomsを配置し、**必要なデータを子コンポーネントに渡す**
- それぞれの位置関係をCSSで指定する

下記は、デジタル庁サイトのフッターのMolecules要素になります
![Atomic-Design-Molecules](/images/Atomic-Design-Molecules.png)
*デジタル庁サイトのフッターのMolecules*

### 3. Organisms
- サインフォームなどの**UIコンポーネント**
- ドメイン知識に依存したデータを受け取る
- Contextの参照
- 見た目：Presentatational Componentで実装
- ロジック：Container Componentで実装
![Atomic-Design-Organisms](/images/Atomic-Design-Organisms.png)
*デジタル庁サイトのフッターのOrganisms*

### 4. Templates
- 複数のOrganisms以下のコンポーネントを配置する
- コンポーネントをCSSでレイアウトする
### 5. Pages
- Templatesに状態管理、router関係の処理、APIコールなどの副作用の実行
- Contextに値を渡す

## デジタル庁のサイトのAtomic Designはどうなっているか
#### 加工前：デジタル庁のフッター
![Atomic-Design-step00](/images/Atomic-Design-step00.png)
#### 加工後：デジタル庁のフッター
![Atomic-Design-step01](/images/Atomic-Design-step01.png)

## おわりに
Atomic-Designの概念に触れてしっかり使いこなせたら便利だなと思いました。
チーム全体で使うには、チーム力がそこそこいるなと感じました。


