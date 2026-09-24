---
title: "[React] useStateの使い方" # 記事のタイトル
emoji: "🦇" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["react", "typescript", "nodejs", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開:true / 非公開:false
---

## 🌱 はじめに

`React Hooks`の`useState`を学習しました。
開発メンバーにも共有するために本記事を執筆します。

本記事では、`useState`を使って下記の画像のような3つのページを作成します。

![react-usestate-step01](/images/articles/react-usestate/react-usestate-step01.png)
_`+ 1`ボタンをクリックすると、即座に`Count`が増えるページ_

![react-usestate-step03](/images/articles/react-usestate/react-usestate-step03.png)
_入力欄に入力した文字が、即座に`Name`と`Age`に反映されるページ_

![react-usestate-step05](/images/articles/react-usestate/react-usestate-step05.png)
_`Add Task`をクリックすると、即座にタスクが増えるページ_

### 前提条件

下記の作業が完了していることを想定しています。

@[card](https://zenn.dev/aew2sbee/articles/react-install)

:::message alert
本記事は、Create React App（TypeScript テンプレート）で作成したプロジェクトを前提にしています。Create React App は、2025年2月に React 公式で非推奨になりました。新しく作る場合は、Vite や Next.js などを使ってください。その場合、起動コマンドやファイル構成は本記事と異なります。
:::

### ファイル構成

ファイル構成は下記のとおりです。

```text
src
├── components
│   ├── useState01.tsx
│   ├── useState02.tsx
│   └── useState03.tsx
├── App.css
├── App.test.tsx
├── App.tsx
├── index.css
├── index.tsx
├── logo.svg
├── react-app-env.d.ts
├── reportWebVitals.ts
└── setupTests.ts
```

## 🌱 1. useStateのキホン

まずは`useState`のキホンを理解します。

:::message
`useState`のキホン構文は下記のとおりです。

```ts
const [状態, 状態更新関数] = useState(初期値);
```

:::

キホン構文を理解できたと思うので、実際にコーディングします。

1. `useState`を使ったコンポーネントを作成する

   ```tsx:src/components/useState01.tsx
   // useStateをインポートする
   import React, { useState } from "react";

   export const State = () => {
     // 初期値を0とする
     const [count, setCount] = useState(0);
     // JSXは1つの要素にまとめて返す必要があるため、<>（React Fragment）で囲む
     return (
       <>
         {/* 現在の状態を表示する */}
         <p>Count: {count}</p>
         {/* クリックしたら、setCountで直前の値（preCount）に1を足す */}
         <button onClick={() => setCount((preCount) => preCount + 1)}>+ 1</button>
         {/* クリックしたら、setCountで初期値に戻す */}
         <button onClick={() => setCount(0)}>reset</button>
       </>
     );
   };
   ```

2. 作成したコンポーネントを`App.tsx`に追加する

   ```tsx:src/App.tsx
   import React from "react";
   import logo from "./logo.svg";
   import "./App.css";
   // 作成したコンポーネントをインポートする
   import { State } from "./components/useState01";

   function App() {
     return (
       <div className="App">
         <header className="App-header">
           <img src={logo} className="App-logo" alt="logo" />
           {/* 作成したコンポーネントを追加する */}
           <State />
         </header>
       </div>
     );
   }

   export default App;
   ```

3. `package.json`があるディレクトリに移動して、下記コマンドでReactを起動する

   ```bash
   npm start
   ```

4. 起動が完了したら`http://localhost:3000/`にアクセスする

5. `+ 1`ボタンでカウントが増えることを確認する

   ![react-usestate-step02](/images/articles/react-usestate/react-usestate-step02.png)
   _`+ 1`ボタンを3回クリックすると、`Count`の数が3になります_

以上で、`useState`のキホンの解説は終わりです。

## 🌱 2. useStateでオブジェクトを使う

次は、`useState`で扱うデータがオブジェクトの場合を解説します。

:::message
`useState`の状態更新関数は、オブジェクトの一部だけを更新（マージ）するのではなく、渡した値で状態を丸ごと置き換えます。
そのため、`...profile`（スプレッド構文）で、変更しないキーの値を引き継ぎます。
:::

1. `useState`でオブジェクトを扱うコンポーネントを作成する

   ```tsx:src/components/useState02.tsx
   import React, { useState } from "react";

   export const StateObject = () => {
     // name, ageのキーを持つオブジェクトを初期値にする
     const [profile, setProfile] = useState({ name: "", age: "" });
     return (
       <>
         <form>
           {/* 入力に変化があったら（onChange）、setProfileで入力値（event.target.value）をnameに設定する */}
           {/* ...profileで、変更するキー以外は現在の値のままにする */}
           <input
             type="text"
             value={profile.name}
             onChange={(event) => setProfile({ ...profile, name: event.target.value })}
           />
           <input
             type="text"
             value={profile.age}
             onChange={(event) => setProfile({ ...profile, age: event.target.value })}
           />
         </form>
         {/* profileのnameを表示する */}
         <p>Name: {profile.name}</p>
         {/* profileのageを表示する */}
         <p>Age: {profile.age}</p>
       </>
     );
   };
   ```

2. 作成したコンポーネントを`App.tsx`に追加する

   ```tsx:src/App.tsx
   import React from "react";
   import logo from "./logo.svg";
   import "./App.css";
   // 作成したコンポーネントをインポートする
   import { StateObject } from "./components/useState02";

   function App() {
     return (
       <div className="App">
         <header className="App-header">
           <img src={logo} className="App-logo" alt="logo" />
           {/* 作成したコンポーネントを追加する */}
           <StateObject />
         </header>
       </div>
     );
   }

   export default App;
   ```

3. `package.json`があるディレクトリに移動して、下記コマンドでReactを起動する

   ```bash
   npm start
   ```

4. 起動が完了したら`http://localhost:3000/`にアクセスする

5. 左の入力欄（Name）に適当な文字を入力し、`Name:`の右隣に即座に文字が表示されることを確認する

6. 右の入力欄（Age）に適当な文字を入力し、`Age:`の右隣に即座に文字が表示され、`Name:`の右隣の文字はそのままであることを確認する

   ![react-usestate-step03](/images/articles/react-usestate/react-usestate-step03.png)
   _左の入力欄（Name）に`ふるた`、右の入力欄（Age）に`18`と入力したページ_

以上で、`useState`でオブジェクトを使う方法の解説は終わりです。

## 🌱 3. useStateで配列を使う

次は、`useState`で扱うデータが配列の場合を解説します。

:::message
初期値が空の配列（`[]`）の場合、TypeScript は要素の型を推論できず、`never[]`型になります。
そのため、`useState<Task[]>([])`のように型を指定します。型の指定方法は「4. useStateで型を指定する」で解説します。
:::

1. `useState`で配列を扱うコンポーネントを作成する

   ```tsx:src/components/useState03.tsx
   import React, { useState } from "react";

   // タスクの型
   type Task = {
     id: number;
   };

   export const StateList = () => {
     // 空の配列を初期値にする
     const [taskList, setTaskList] = useState<Task[]>([]);
     // ボタンがクリックされたら、配列に要素を1つ追加する
     const addTask = () => {
       setTaskList([...taskList, { id: taskList.length }]);
     };
     return (
       <>
         <button onClick={addTask}>Add Task</button>
         <ul>
           {/* mapを使い、<li>要素を作成する */}
           {taskList.map((task) => (
             <li key={task.id}>Task{task.id}</li>
           ))}
         </ul>
       </>
     );
   };
   ```

2. 作成したコンポーネントを`App.tsx`に追加する

   ```tsx:src/App.tsx
   import React from "react";
   import logo from "./logo.svg";
   import "./App.css";
   // 作成したコンポーネントをインポートする
   import { StateList } from "./components/useState03";

   function App() {
     return (
       <div className="App">
         <header className="App-header">
           <img src={logo} className="App-logo" alt="logo" />
           {/* 作成したコンポーネントを追加する */}
           <StateList />
         </header>
       </div>
     );
   }

   export default App;
   ```

3. `package.json`があるディレクトリに移動して、下記コマンドでReactを起動する

   ```bash
   npm start
   ```

4. 起動が完了したら`http://localhost:3000/`にアクセスする

5. 下記の画像のように表示されることを確認する

   ![react-usestate-step04](/images/articles/react-usestate/react-usestate-step04.png)

6. `Add Task`を3回クリックし、下記の画像のように表示されることを確認する

   ![react-usestate-step05](/images/articles/react-usestate/react-usestate-step05.png)

以上で、`useState`で配列を使う方法の解説は終わりです。

## 🌱 4. useStateで型を指定する

:::message
型を指定する場合の`useState`の構文は下記のとおりです。

```ts
const [状態, 状態更新関数] = useState<型>(初期値);
```

:::

`useState(0)`のように初期値から型がわかる場合は、TypeScript が型を推論するため、型の指定は省略できます。
下記のコードは、明示的に数値型を指定した例です。

```tsx:src/components/useState01.tsx
import React, { useState } from "react";

export const State = () => {
  // 数値型を明示的に指定する（useState(0)でもnumber型と推論される）
  const [count, setCount] = useState<number>(0);
  return (
    <>
      <p>Count: {count}</p>
      <button onClick={() => setCount((preCount) => preCount + 1)}>+ 1</button>
      <button onClick={() => setCount(0)}>reset</button>
    </>
  );
};
```

型の指定が必要になるのは、下記のように初期値だけでは型が決まらない場合です。

- 空の配列を初期値にする場合（例: `useState<Task[]>([])`）
- `null`を初期値にする場合（例: `useState<User | null>(null)`）

## 🌱 5. useStateで自作の型を指定する

`age`を数値型にするため、入力欄を`type="number"`にし、`Number()`で入力値（文字列）を数値に変換しています。

```tsx:src/components/useState02.tsx
import React, { useState } from "react";

// 自作の型を作成する
type TypeProfile = {
  name: string;
  age: number;
};

export const StateObject = () => {
  // 作成した型をuseStateの型として指定する
  const [profile, setProfile] = useState<TypeProfile>({ name: "", age: 0 });
  return (
    <>
      <form>
        <input
          type="text"
          value={profile.name}
          onChange={(event) => setProfile({ ...profile, name: event.target.value })}
        />
        {/* event.target.valueは文字列のため、Number()で数値に変換する */}
        <input
          type="number"
          value={profile.age}
          onChange={(event) => setProfile({ ...profile, age: Number(event.target.value) })}
        />
      </form>
      <p>Name: {profile.name}</p>
      <p>Age: {profile.age}</p>
    </>
  );
};
```
