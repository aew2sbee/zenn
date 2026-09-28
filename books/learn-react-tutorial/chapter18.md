---
title: "useState で配列を扱う（追加・削除・置換・挿入）"
---

React の `useState` では、配列も state として持てます。
ただし **state の配列は直接書き換えず**、必ず **新しい配列を作って** `setState` で差し替えるのが基本です。

## 🌱 早見表（state を直接書き換えない）

| 操作 | ❌ 使わない（配列を書き換える） | ✅ 使う（新しい配列を返す） |
| --- | --- | --- |
| 追加 | `push`, `unshift` | `concat`, `[...arr]`（スプレッド構文） |
| 削除 | `pop`, `shift`, `splice` | `filter`, `slice` |
| 要素置換 | `splice`, `arr[i] = ...`（代入） | `map` |
| 並び替え | `reverse`, `sort`（※破壊的） | `toReversed`, `toSorted`（またはコピーしてから `reverse/sort`） |

> ⚠️ `reverse()` と `sort()` は **元の配列を直接変更（破壊的）**します。
> state に対して使う場合は、`[...arr].sort()` のように **コピーしてから**使うか、`toSorted()` / `toReversed()`（対応環境なら）を使います。


## 🌱 配列に要素を追加（末尾に追加）

```diff jsx
import { useState } from 'react';

let nextId = 0;

export default function List() {
  const [name, setName] = useState('');
  const [artists, setArtists] = useState([]);

  return (
    <>
      <h1>Inspiring sculptors:</h1>
      <input
        value={name}
        onChange={e => setName(e.target.value)}
      />
      <button onClick={() => {
-        artists.push({
-          id: nextId++,
-          name: name,
-        });
+        setArtists([
+          ...artists,
+          { id: nextId++, name: name }
+        ]);
      }}>Add</button>
      <ul>
        {artists.map(artist => (
          <li key={artist.id}>{artist.name}</li>
        ))}
      </ul>
    </>
  );
}
```

:::message
**ポイント**
- `push()`は配列を直接変更するため`state`では避ける
- `setArtists([...artists, newItem])`で 新しい配列を作る
:::


## 🌱 配列から要素を削除（条件に合う要素を除外）

```diff jsx
import { useState } from 'react';

let initialArtists = [
  { id: 0, name: 'Marta Colvin Andrade' },
  { id: 1, name: 'Lamidi Olonade Fakeye'},
  { id: 2, name: 'Louise Nevelson'},
];

export default function List() {
  const [artists, setArtists] = useState(
    initialArtists
  );

  return (
    <>
      <h1>Inspiring sculptors:</h1>
      <ul>
        {artists.map(artist => (
          <li key={artist.id}>
            {artist.name}{' '}
            <button onClick={() => {
+              setArtists(
+                artists.filter(a =>
+                  a.id !== artist.id
+                )
              );
            }}>
              Delete
            </button>
          </li>
        ))}
      </ul>
    </>
  );
}

```

:::message
**ポイント**
- `filter()`は「残したい要素だけを集めた新しい配列」を返す
- 削除というより「除外して作り直す」イメージ
:::


## 🌱 配列内の要素を置換（特定の要素だけ更新）

```diff jsx
import { useState } from 'react';

let initialCounters = [0, 0, 0];

export default function CounterList() {
  const [counters, setCounters] = useState(
    initialCounters
  );

  function handleIncrementClick(index) {
+    const nextCounters = counters.map((c, i) => {
+      if (i === index) {
+        // Increment the clicked counter
+        return c + 1;
+      } else {
+        // The rest haven't changed
+        return c;
+      }
+    });
+    setCounters(nextCounters);
  }

  return (
    <ul>
      {counters.map((counter, i) => (
        <li key={i}>
          {counter}
          <button onClick={() => {
            handleIncrementClick(i);
          }}>+1</button>
        </li>
      ))}
    </ul>
  );
}

```

:::message
**ポイント**
- `map()`で 新しい配列を作りながら、更新したい要素だけ差し替える
:::

## 🌱 配列への挿入（途中に 1 件入れる）


```diff jsx
import { useState } from 'react';

let nextId = 3;
const initialArtists = [
  { id: 0, name: 'Marta Colvin Andrade' },
  { id: 1, name: 'Lamidi Olonade Fakeye'},
  { id: 2, name: 'Louise Nevelson'},
];

export default function List() {
  const [name, setName] = useState('');
  const [artists, setArtists] = useState(
    initialArtists
  );

  function handleClick() {
    const insertAt = 1;
+    const nextArtists = [
+      // 挿入位置より前の要素を slice でコピー
+      ...artists.slice(0, insertAt),
+      // 新しい要素（元の配列は直接変更しない）
+      { id: nextId++, name: name },
+      // 挿入位置以降の要素を slice でコピー
+      ...artists.slice(insertAt)
+    ];
+    setArtists(nextArtists);
+    setName('');
  }

  return (
    <>
      <h1>Inspiring sculptors:</h1>
      <input
        value={name}
        onChange={e => setName(e.target.value)}
      />
      <button onClick={handleClick}>
        Insert
      </button>
      <ul>
        {artists.map(artist => (
          <li key={artist.id}>{artist.name}</li>
        ))}
      </ul>
    </>
  );
}


```

:::message
**ポイント**
- `splice()`は破壊的なので避ける
- `slice()`とスプレッドで「前半 + 新要素 + 後半」を作る
:::


## 🌱 state 内の配列を更新（オブジェクト配列の更新例）
`useState`は配列も`state`として持てます。
ただし `React`の`state`は 直接書き換えず（ミュータブルにせず）、新しい配列を作って更新する必要があります。

たとえば「特定の要素だけ更新したい」場合は、`map()`を使って**新しい配列**を作るのが定番です。
```tsx
import { useState } from "react";

/**
 * 1件のアート作品データの型
 * - id: 作品を一意に識別するためのID（keyにも使う）
 * - title: 表示するタイトル
 * - seen: チェック済みかどうか（見た/見てない）
 */
type Artwork = {
  id: number;
  title: string;
  seen: boolean;
};

/**
 * 初期表示するリスト（stateの初期値として使う）
 * ※ 配列の中身は Artwork 型のオブジェクト
 */
const initialList: Artwork[] = [
  { id: 0, title: "Big Bellies", seen: false },
  { id: 1, title: "Lunar Landscape", seen: false },
  { id: 2, title: "Terracotta Army", seen: true },
];

export default function BucketList() {
  /**
   * list: 画面に表示するリスト（state）
   * setList: list を更新するときに使う関数
   */
  const [list, setList] = useState<Artwork[]>(initialList);

  /**
   * チェックボックスのON/OFFを受け取って、該当する作品の seen を更新する
   * - artworkId: 更新対象の作品ID
   * - nextSeen: 更新後の seen（checkboxの状態）
   *
   * ✅ポイント:
   * Reactのstateは「直接書き換え」ではなく「新しい配列を作って差し替え」ます。
   */
  const handleToggle = (artworkId: Artwork["id"], nextSeen: boolean) => {
    // setList に関数を渡すと、常に最新の state(prev) を使って更新できる
    setList((prev) =>
      // mapで「新しい配列」を作る（元の配列は壊さない）
      prev.map((artwork) =>
        // 該当IDだけ seen を nextSeen に更新し、それ以外はそのまま返す
        artwork.id === artworkId ? { ...artwork, seen: nextSeen } : artwork
      )
    );
  };

  return (
    <>
      <h1>Art Bucket List</h1>
      <h2>My list of art to see:</h2>

      {/* 子コンポーネントに、表示用データ(artworks)と更新用関数(onToggle)を渡す */}
      <ItemList artworks={list} onToggle={handleToggle} />
    </>
  );
}

/**
 * ItemListコンポーネントが受け取るpropsの型
 * - artworks: 表示する作品一覧
 * - onToggle: チェック状態が変わったときに呼ぶ関数
 */
type ItemListProps = {
  artworks: Artwork[];
  onToggle: (artworkId: Artwork["id"], nextSeen: boolean) => void;
};

function ItemList({ artworks, onToggle }: ItemListProps) {
  return (
    <ul>
      {/* 配列を map して <li> を並べる */}
      {artworks.map((artwork) => (
        // key は「リストの各要素を一意に識別するため」必須
        <li key={artwork.id}>
          <label>
            <input
              type="checkbox"
              // チェック状態は state(artwork.seen) と同期させる（制御コンポーネント）
              checked={artwork.seen}
              onChange={(e: React.ChangeEvent<HTMLInputElement>) => {
                // e.target.checked は「チェックされてるか」の真偽値
                // 親から受け取った onToggle を呼んで state を更新してもらう
                onToggle(artwork.id, e.target.checked);
              }}
            />
            {/* タイトルを表示 */}
            {artwork.title}
          </label>
        </li>
      ))}
    </ul>
  );
}


```

## 🌱 オブジェクト更新の「コピー」を楽にする：Immer（use-immer）
配列やオブジェクトが深くなると、`...`のコピーが増えてコードが読みにくくなりがちです。
そこで`Immer`を使うと、見た目は「直接書き換え」に近い書き方のまま、内部でイミュータブルな更新を作ってくれます。

```diff tsx
- import { useState } from "react";
+ import { useImmer } from 'use-immer';

/**
 * 1件のアート作品データの型
 * - id: 作品を一意に識別するためのID（keyにも使う）
 * - title: 表示するタイトル
 * - seen: チェック済みかどうか（見た/見てない）
 */
type Artwork = {
  id: number;
  title: string;
  seen: boolean;
};

/**
 * 初期表示するリスト（stateの初期値として使う）
 * ※ 配列の中身は Artwork 型のオブジェクト
 */
const initialList: Artwork[] = [
  { id: 0, title: "Big Bellies", seen: false },
  { id: 1, title: "Lunar Landscape", seen: false },
  { id: 2, title: "Terracotta Army", seen: true },
];

export default function BucketList() {
  /**
   * list: 画面に表示するリスト（state）
   * setList: list を更新するときに使う関数
   */
-  const [list, setList] = useState<Artwork[]>(initialList);
+  const [list, updateList] = useImmer<Artwork[]>(initialList);

  /**
   * チェックボックスのON/OFFを受け取って、該当する作品の seen を更新する
   * - artworkId: 更新対象の作品ID
   * - nextSeen: 更新後の seen（checkboxの状態）
   *
   * ✅ポイント:
   * Reactのstateは「直接書き換え」ではなく「新しい配列を作って差し替え」ます。
   */
  const handleToggle = (artworkId: Artwork["id"], nextSeen: boolean) => {
    // setList に関数を渡すと、常に最新の state(prev) を使って更新できる
-    setList((prev) =>
-      // mapで「新しい配列」を作る（元の配列は壊さない）
-      prev.map((artwork) =>
-        // 該当IDだけ seen を nextSeen に更新し、それ以外はそのまま返す
-        artwork.id === artworkId ? { ...artwork, seen: nextSeen } : artwork
-      )
-    );
+    updateList(draft => {
+      const artwork = draft.find(a =>
+        a.id === artworkId
+      );
+      artwork.seen = nextSeen;
+    });
  };

  return (
    <>
      <h1>Art Bucket List</h1>
      <h2>My list of art to see:</h2>

      {/* 子コンポーネントに、表示用データ(artworks)と更新用関数(onToggle)を渡す */}
      <ItemList artworks={list} onToggle={handleToggle} />
    </>
  );
}

/**
 * ItemListコンポーネントが受け取るpropsの型
 * - artworks: 表示する作品一覧
 * - onToggle: チェック状態が変わったときに呼ぶ関数
 */
type ItemListProps = {
  artworks: Artwork[];
  onToggle: (artworkId: Artwork["id"], nextSeen: boolean) => void;
};

function ItemList({ artworks, onToggle }: ItemListProps) {
  return (
    <ul>
      {/* 配列を map して <li> を並べる */}
      {artworks.map((artwork) => (
        // key は「リストの各要素を一意に識別するため」必須
        <li key={artwork.id}>
          <label>
            <input
              type="checkbox"
              // チェック状態は state(artwork.seen) と同期させる（制御コンポーネント）
              checked={artwork.seen}
              onChange={(e: React.ChangeEvent<HTMLInputElement>) => {
                // e.target.checked は「チェックされてるか」の真偽値
                // 親から受け取った onToggle を呼んで state を更新してもらう
                onToggle(artwork.id, e.target.checked);
              }}
            />
            {/* タイトルを表示 */}
            {artwork.title}
          </label>
        </li>
      ))}
    </ul>
  );
}

```