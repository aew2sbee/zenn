---
title: "[React] useStateの使い方" # 記事のタイトル
emoji: "🦇" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["react", "typescript", "nodejs", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開:true / 非公開:false
---

## はじめに

`React Hooks`の`useState`を学習しました。
開発メンバーにも共有するために本記事を執筆します。

本記事では、`useState`を使って 3 つの下記の画像のような機能ページを作成します。
![react-usestate-step01](/images/articles/react-usestate/react-usestate-step01.png)
_`+1`ボタンをクリックすると、即座に`Count`が増えるページ_
![react-usestate-step03](/images/articles/react-usestate/react-usestate-step03.png)
_入力欄に文字を入力すると、即座に`Name`と`Age`に反映するページ_
![react-usestate-step05](/images/articles/react-usestate/react-usestate-step05.png)
_`Add Task`をクリックすると、即座にタスクが増えるページ_

### 前提条件

下記の作業が完了していることを想定しております。
@[card](https://zenn.dev/aew2sbee/articles/react-install)

### ファイル構成

ファイル構成は下記のとおりです。

```bash
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

## 1. useState のキホン

まずは**useState のキホン**を理解します。

:::message
`useState`のキホン構文は下記の通りになります。

```ts
const [状態, 状態更新関数] = useState(初期値);
```

:::

キホン構文を理解できたと思うので、実際にコーディングします。

1. `useState`を使ったコンポーネントを作成する

```tsx: src/components/useState01.tsx
// useStateをインポートする
import React, {useState} from "react";

export const State = () => {
    // 初期値を0とする
    const [count, setCount] = useState(0)
    return (
        // <>: React Fragmentで外側を囲む必要あり
        <>
            // 現在のステータスを表示
            <p>Count: {count}</p>
            // クリックしたらsetCountを使い、前回の値を(preCount)に`+1`をする
            <button onClick={() => {setCount(preCount=>preCount+1)} }>+ 1</button>
            // クリックしたらsetCountを使い、初期値に戻す
            <button onClick={() => {setCount(0)} }>reset</button>
        </>
    )
}
```

2. 自作コンポーネントを`App.tsx`に追加し反映する

```tsx: /src/App.tsx
import React from 'react';
import logo from './logo.svg';
import './App.css';
// 自作コンポーネントをインポート
import { State } from './components/useState01';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        // 自作コンポーネントの追加
      <State />
      </header>
    </div>
  );
}

export default App;

```

3. `package.json`があるディレクトリに移動し、下記コマンドを実行し React を起動する

```bash
npm start
```

4. 起動が完了したら`http://localhost:3000/`にアクセスする

5. `+1`ボタンでカウントが増えるか確認する
   ![react-usestate-step02](/images/articles/react-usestate/react-usestate-step02.png)
   _`+1`ボタンを 3 回クリックすると、`Count`の数が 3 になります_

以上で`useState`のキホンとなります。

## 2. useState で Object を使う

次は、`useState`を扱うデータを`Object`の場合を解説します。

1. `useState`を使い、扱うデータを`Object`でコンポーネントを作成する

```tsx: src/components/useState02.tsx
import React,  {useState} from "react";

export const SatateObject = () => {
    // name, ageのキーを持つオブジェクトを初期値にする
    const [profile, setProfile] = useState({name: '', age: ''})
    return (
        <>
            <form>
                // nameを入力する欄を作成する
                // 入力に変化がある場合(onChange)、setProfileを使い入力値(event.target.value)をprofileに設定する
                // ...profile:　変更するキー以外は現在値のままにする指定にする
                <input type='text' value={profile.name}
                    onChange={event => setProfile({...profile, name: event.target.value})}/>
                <input type='text' value={profile.age}
                    onChange={event => setProfile({...profile, age: event.target.value})}/>
            </form>
            // useStateを使ったprofileのnameを表示する
            <p>Name: {profile.name}</p>
            // useStateを使ったprofileのageを表示する
            <p>Age: {profile.age}</p>
        </>
    )
}
```

2. 自作コンポーネントを`App.tsx`に追加し反映する

```tsx: /src/App.tsx
import React from 'react';
import logo from './logo.svg';
import './App.css';
// 自作コンポーネントをインポート
import { SatateObject } from './components/useState02';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        // 自作コンポーネントの追加
      <SatateObject />
      </header>
    </div>
  );
}

export default App;

```

3. `package.json`があるディレクトリに移動し、下記コマンドを実行し React を起動する

```bash
npm start
```

4. 起動が完了したら`http://localhost:3000/`にアクセスする

5. 左の入力欄(Name)に適当な文字を入力し、`Name:`の右隣に即座に文字が表示される事を確認する
6. 右の入力欄(Age)に適当な文字を入力し、`Age:`の右隣に即座に文字が表示され、`Name:`の右隣に表示されている文字はそのままであることを確認する
   ![react-usestate-step03](/images/articles/react-usestate/react-usestate-step03.png)
   _左の入力欄(Name)に`ふるた`を入力し、右の入力欄(Age)に`18`と入力したページ_

以上で`useState`で Object を使う解説になります。

## 3. useState で配列を使う

次は、`useState`を扱うデータを`List`の場合を解説します。

1. `useState`を使い、扱うデータを`List`でコンポーネントを作成する

```tsx: src/components/useState03.tsx
import React, {useState} from "react";

export const SatateList = () => {
    // 空の配列を初期値にする
    const [taskList, setTaskList] = useState([])
    // ボタンをクリックされた配列に要素を1足す関数を作成する
    const addTask = () => {
        setTaskList([...taskList, {
            id: taskList.length
        }])
    }
    return (
        <>
            <button onClick={addTask}>Add Task</button>
            <ul>
                // mapを使い、<li>の要素を作成する
                {taskList.map(task => (
                    <li key={task.id}>Task{task.id}</li>
                ))}
            </ul>
        </>
    )
}

```

2. 自作コンポーネントを`App.tsx`に追加し反映する

```tsx: /src/App.tsx
import React from 'react';
import logo from './logo.svg';
import './App.css';
// 自作コンポーネントをインポート
import { SatateList } from './components/useState03';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        // 自作コンポーネントの追加
      <SatateList />
      </header>
    </div>
  );
}

export default App;

```

3. `package.json`があるディレクトリに移動し、下記コマンドを実行し React を起動する

```bash
npm start
```

4. 起動が完了したら`http://localhost:3000/`にアクセスする

5. 下記の画像のように表示される事を確認する
   ![react-usestate-step04](/images/articles/react-usestate/react-usestate-step04.png)
6. `Add Task`を 3 回クリックして 下記の画像のように表示される事を確認する
   ![react-usestate-step05](/images/articles/react-usestate/react-usestate-step05.png)

以上で`useState`で List を使う解説になります。

## 4. useState で型を指定する

:::message
`useState`のキホン構文は下記の通りになります。

```ts
const [状態, 状態更新関数] = useState<型>(初期値);
```

:::

```tsx: src/components/useState01.tsx
// useStateをインポートする
import React,  {useState} from "react";

export const State = () => {
    // 初期値が0のため、数値型を指定する
    const [count, setCount] = useState<number>(0)
    return (
        <>
            <p>Count: {count}</p>
            <button onClick={() => {setCount(preCount=>preCount+1)} }>+ 1</button>
            <button onClick={() => {setCount(0)} }>reset</button>
        </>
    )
}
```

## 5. useState で自作の型を指定する

```tsx: src/components/useState02.tsx
import React,  {useState} from "react";

// 自作で型を作成する
type TypeProfile = {
  name: string
  age: number
}

export const SatateObject = () => {
    // 作成した型をuseStateで使う型として利用する
    const [profile, setProfile] = useState<TypeProfile>({name: '', age: 0})
    return (
        <>
            <form>
                <input type='text' value={profile.name}
                    onChange={event => setProfile({...profile, name: event.target.value})}/>
                <input type='text' value={profile.age}
                    onChange={event => setProfile({...profile, age: event.target.value})}/>
            </form>
            <p>Name: {profile.name}</p>
            <p>Age: {profile.age}</p>
        </>
    )
}
```
