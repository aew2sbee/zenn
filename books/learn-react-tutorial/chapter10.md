---
title: "コンポーネントにpropsを渡す"
---

## 🌱 子コンポーネントに引数を渡す
React では、**親コンポーネントから子コンポーネントへデータを渡す**ために`props`（プロパティ）という仕組みを使います。

まずは、`props` を使わないシンプルなコンポーネントから見てみましょう。

```tsx: components/Avatar.tsx
// 子コンポーネント
export function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/1bX5QH6.jpg"
      alt="Lin Lanying"
      width={100}
      height={100}
    />
  );
}
```

```tsx: app/page.tsx
// 親コンポーネント
import { Avatar } from '@/components/Avatar';

export default function Page() {
  return (
    <>
      <Avatar />
    </>
  );
}
```

この時点では、`Avatar`コンポーネントの表示内容はコンポーネント自身の中に固定された値になっています。


```diff tsx: components/Avatar.tsx
// 子コンポーネント
+ type Person = {
+   name: string;
+   imageUrl: string;
+ };
+
+ type AvatarProps = {
+   person: Person;
+   size: number;
+ };

- export function Avatar() {
+ export function Avatar({ person, size }: AvatarProps) {
  return (
    <img
      className="avatar"
-      src="https://i.imgur.com/1bX5QH6.jpg"
-      alt="Lin Lanying"
-      width={100}
-      height={100}
+      src={person.imageUrl}
+      alt={person.name}
+      width={size}
+      height={size}
    />
  );
}
```


:::message
**ポイント**
- props の型を`AvatarProps`として定義する
- 関数の引数で`{ person, size }`のように分割代入で受け取る
- 表示に使う値を、すべて`props`経由にする

**「データを表示するだけの再利用しやすいコンポーネント」** になります。

:::

次に、親コンポーネント側から `Avatar` コンポーネントへ
`props` を渡すように修正します。

```diff tsx: app/page.tsx
// 親コンポーネント
import { Avatar } from '@/components/Avatar';

export default function Page() {
  return (
    <>
-    <Avatar />
+    <Avatar
+      person={{
+        name: 'Lin Lanying',
+        imageUrl: 'https://i.imgur.com/1bX5QH6.jpg',
+      }}
+      size={100}
+    />
    </>
  );
}
```

## 🌱 [おまけ] インラインで型を書く（小さい props のとき）

props の数が少なく、他で再利用しない場合は
型を別で定義せず、その場でインラインに書くこともできます。

```diff tsx: components/Avatar.tsx
// 子コンポーネント
- export function Avatar() {
+ export function Avatar({
+   name,
+   imageUrl,
+   size,
+ }: {
+   name: string;
+   imageUrl: string;
+   size: number;
+ }) {
   return (
     <img
-       src="https://i.imgur.com/1bX5QH6.jpg"
-       alt="Lin Lanying"
-       width={100}
-       height={100}
+       className="avatar"
+       src={imageUrl}
+       alt={name}
+       width={size}
+       height={size}
     />
  );
}
```

```diff tsx: app/page.tsx
// 親コンポーネント
import { Avatar } from '@/components/Avatar';

export default function Page() {
  return (
    <>
-    <Avatar />
+    <Avatar
+      name="Lin Lanying"
+      imageUrl="https://i.imgur.com/1bX5QH6.jpg"
+      size={100}
+    />
    </>
  );
}
```