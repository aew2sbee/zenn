---
title: "useState-オブジェクト"
---

## 🌱 state 内のオブジェクトの更新
`useState`はオブジェクトも state として持てます。
ただし`React`の`state`は直接書き換えず（ミューテートせず）、新しいオブジェクトを作って更新する必要があります。
直接書き換えても React は変更を検知できず、再レンダーされないためです。

新しいオブジェクトは、スプレッド構文（`...person`：`person`の中身を展開してコピーする構文）で作ります。
特にネストしたオブジェクトは、更新したい階層まで段階的にコピーしてから値を上書きします。

```tsx
"use client";
import { useState } from "react";

type Person = {
  name: string;
  artwork: {
    title: string;
    city: string;
    image: string;
  };
};

export default function Form() {
  // フォーム全体の入力値を「1つの state オブジェクト」で管理する
  const [person, setPerson] = useState<Person>({
    name: "テストA",
    artwork: {
      title: "Blue Nana",
      city: "Hamburg",
      image: "https://i.imgur.com/Sd1AgUOm.jpg",
    },
  });

  // name を更新（浅い階層なので person をコピーして name だけ上書き）
  function handleNameChange(e: React.ChangeEvent<HTMLInputElement>) {
    setPerson({
      ...person,
      name: e.target.value,
    });
  }

  // artwork.title を更新（ネストしているので「2段階」でコピー）
  function handleTitleChange(e: React.ChangeEvent<HTMLInputElement>) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        title: e.target.value,
      },
    });
  }

  // artwork.city を更新
  function handleCityChange(e: React.ChangeEvent<HTMLInputElement>) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        city: e.target.value,
      },
    });
  }

  // artwork.image を更新
  function handleImageChange(e: React.ChangeEvent<HTMLInputElement>) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        image: e.target.value,
      },
    });
  }

  return (
    <>
      {/* value に state を紐づけ、onChange で state を更新する（制御コンポーネント：入力値を state で管理するコンポーネント） */}
      <label>
        Name:
        <input value={person.name} onChange={handleNameChange} />
      </label>

      <label>
        Title:
        <input value={person.artwork.title} onChange={handleTitleChange} />
      </label>

      <label>
        City:
        <input value={person.artwork.city} onChange={handleCityChange} />
      </label>

      <label>
        Image:
        <input value={person.artwork.image} onChange={handleImageChange} />
      </label>

      {/* state の内容を表示（入力が変わるとここも再レンダーされる） */}
      <p>
        <i>{person.artwork.title}</i>
        {" by "}
        {person.name}
        <br />
        (located in {person.artwork.city})
      </p>

      <img src={person.artwork.image} alt={person.artwork.title} />
    </>
  );
}


```


## 🌱 オブジェクト更新の「コピー」を楽にする：Immer（use-immer）
ネストが深くなるほど、スプレッドでのコピーは長くなりがちです。
そこで`use-immer`を使うと、見た目は直接書き換えるように書きつつ、
内部では元のオブジェクトを変更しない更新（イミュータブル更新）として`state`を作ってくれます。

インストール（プロジェクトのルート、`package.json`がある場所で実行します。成功すると`package.json`の`dependencies`に`use-immer`が追加されます）
```bash
npm install use-immer
```

```diff tsx
"use client";
- import { useState } from 'react';
+ import { useImmer } from 'use-immer';

type Person = {
  name: string;
  artwork: {
    title: string;
    city: string;
    image: string;
  };
};

export default function Form() {
-  const [person, setPerson] = useState({
+  const [person, updatePerson] = useImmer<Person>({
    name: 'テストA',
    artwork: {
      title: 'Blue Nana',
      city: 'Hamburg',
      image: 'https://i.imgur.com/Sd1AgUOm.jpg',
    }
  });

  function handleNameChange(e: React.ChangeEvent<HTMLInputElement>) {
-    setPerson({
-      ...person,              // ← person の他のプロパティはそのまま残す
-      name: e.target.value    // ← name だけ更新
+    updatePerson(draft => {
+      draft.name = e.target.value;
    });
  }

  // artwork.title を更新するハンドラ
  // ネストされたオブジェクトも draft を直接書き換えるだけでよい
  function handleTitleChange(e: React.ChangeEvent<HTMLInputElement>) {
-    setPerson({
-      ...person,
-      artwork: {
-        ...person.artwork,
-        title: e.target.value
-      }
+    updatePerson(draft => {
+      draft.artwork.title = e.target.value;
    });
  }

  // artwork.city を更新するハンドラ
  function handleCityChange(e: React.ChangeEvent<HTMLInputElement>) {
-    setPerson({
-      ...person,
-      artwork: {
-        ...person.artwork,
-        city: e.target.value
-      }
+    updatePerson(draft => {
+      draft.artwork.city = e.target.value;
    });
  }

  // artwork.image を更新するハンドラ
  function handleImageChange(e: React.ChangeEvent<HTMLInputElement>) {
-    setPerson({
-      ...person,
-      artwork: {
-        ...person.artwork,
-        image: e.target.value
-      }
+    updatePerson(draft => {
+      draft.artwork.image = e.target.value;
    });
  }

  return (
    <>
      <label>
        Name:
        <input value={person.name} onChange={handleNameChange} />
      </label>

      <label>
        Title:
        <input value={person.artwork.title} onChange={handleTitleChange} />
      </label>

      <label>
        City:
        <input value={person.artwork.city} onChange={handleCityChange} />
      </label>

      <label>
        Image:
        <input value={person.artwork.image} onChange={handleImageChange} />
      </label>

      <p>
        <i>{person.artwork.title}</i>
        {" by "}
        {person.name}
        <br />
        (located in {person.artwork.city})
      </p>

      <img src={person.artwork.image} alt={person.artwork.title} />
    </>
  );
}


```

## 🌱 参考
- https://ja.react.dev/learn/updating-objects-in-state
