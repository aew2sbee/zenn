---
title: "インタラクティビティの追加"
---

## 🌱 イベントへの応答

`React`では、`JSX`にイベントハンドラを追加して、
ユーザの操作（クリック、ホバー、入力など）に反応させることができます。
イベントハンドラはあなたが定義する関数で、ユーザインタラクションが発生したときに呼び出されます。

`<button>`のような組み込み要素は`onClick`などのブラウザ標準イベントを受け取れます。
一方で、自作コンポーネントでは`props`としてイベントハンドラを受け取り、
`onPlayMovie`のようにアプリ固有の名前を付けることもできます。

```tsx
export default function App() {
  return (
    <Toolbar
      onPlayMovie={() => alert('Playing!')}
      onUploadImage={() => alert('Uploading!')}
    />
  );
}

function Toolbar({ onPlayMovie, onUploadImage }) {
  return (
    <div>
      <Button onClick={onPlayMovie}>
        Play Movie
      </Button>
      <Button onClick={onUploadImage}>
        Upload Image
      </Button>
    </div>
  );
}

function Button({ onClick, children }) {
  return (
    <button onClick={onClick}>
      {children}
    </button>
  );
}

```
