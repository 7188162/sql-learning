---
title: "1-1. SQLの4つの言語"
weight: 11
---

SQLは、その役割に応じて大きく4つのカテゴリに分類されます。

- **DDL (Data Definition Language): データ定義言語**
  - データベースの構造（テーブル、インデックスなど）を定義します。
  - `CREATE`, `ALTER`, `DROP`
- **DML (Data Manipulation Language): データ操作言語**
  - 実際のデータを操作（検索、追加、更新、削除）します。
  - `SELECT`, `INSERT`, `UPDATE`, `DELETE`
- **DCL (Data Control Language): データ制御言語**
  - データへのアクセス権限を管理します。
  - `GRANT`, `REVOKE`
- **TCL (Transaction Control Language): トランザクション制御言語**
  - DMLによる変更を確定または取り消します。
  - `COMMIT`, `ROLLBACK`

本章では、この中のDDLに焦点を当てます。
