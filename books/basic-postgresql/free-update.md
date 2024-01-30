---
title: '[UPDATE] データを更新する'
free: true
---

## 基本構文

:::message
下記が`基本構文`なります

```sql
UPDATE テーブル名 SET カラム名 = 設定したい値 WHERE 条件式
```

:::

### サンプル

`2024-01-01`のデータを`2024-01-15`に更新する

```sql
UPDATE sales SET date = '2024-01-15' WHERE date = '2024-01-01'
```
