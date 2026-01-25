---
title: "useState-オブジェクト"
---

## 🌱 state 内のオブジェクトの更新
`useState`はオブジェクトも state として持てます。
ただし `React`の`state`は 直接書き換えず（ミュータブルにせず）、新しいオブジェクトを作って更新する必要があります。

特にネストしたオブジェクトは、更新したい階層まで 段階的にコピーしてから値を上書きします。

```tsx
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
      {/* value に state を紐づけ、onChange で state を更新する（制御コンポーネント） */}
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
そこで`use-immer`を使うと、見た目はミュータブルに書きつつ、
内部では イミュータブル更新として`state`を作ってくれます。

インストール
```bash
npm install use-immer
```

```diff tsx
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


  // 3) artwork.title を更新するハンドラ
  // ネストされたオブジェクトは「2段階」でコピーしてから更新する
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

  // 4) artwork.city を更新するハンドラ
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

  // 5) artwork.image を更新するハンドラ
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