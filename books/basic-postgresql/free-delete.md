---
title: '[DELETE] データを削除する'
free: true
---

## 基本構文

:::message
下記が`基本構文`なります

```sql
DELETE FROM テーブル名 WHERE 条件式
```

:::

### サンプル

`2024-01-01`のデータを削除する

```sql
DELETE FROM sales WHERE date = '2024-01-01'
```
