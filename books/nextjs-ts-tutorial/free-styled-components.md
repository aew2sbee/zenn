---
title: '[作業] styled-componentsの設定'
free: true
---

## styled-componentsとは
> Reactコンポーネントに対するスタイリングを行うためのライブラリです。<br>このライブラリを使用すると、JavaScript（またはTypeScript）内にCSSを効率的書くためのもの。

## styled-componentsのインストール
下記コマンドでデフォルトで作成する事が出来ますが、PJ作成時に作成されます。
> sa@types/styled-components
```bash
npm install styled-components
npm install --save-dev @types/styled-components
```
<!-- 
## styled-componentsの更新
```diff js
/** @type {import('next').NextConfig} */
- const nextConfig = {}

+ const nextConfig = {
+   reactStrictMode: true,
+   compiler: (() => {
+     let compilerConfig = {
+       // styledComponentsの有効化
+       styledComponents: true,
+     }
+     return compilerConfig
+   }


module.exports = nextConfig

``` -->