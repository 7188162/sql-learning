---
title: "1-4. テーブル定義の変更と削除"
weight: 14
---
一度作成したテーブルの定義を変更したり、不要になったテーブルを削除する方法を学びます。

### テーブル定義の変更 (`ALTER TABLE`)
`ALTER TABLE` を使って、テーブルの構造を変更します。

- **カラムの追加**
  {{< highlight sql >}}
  ALTER TABLE employees ADD COLUMN email TEXT;
  {{< /highlight >}}
- **カラムの削除** (SQLiteでは非標準。一部のRDBMSのみ)
  {{< highlight sql >}}
  ALTER TABLE employees DROP COLUMN hire_date;
  {{< /highlight >}}

{{< dbmsdiff title="SQLiteのALTER TABLEの制限" >}}
SQLiteの `ALTER TABLE` でできることは限られています。カラムの追加（`ADD COLUMN`）とテーブル名変更（`RENAME TO`）は可能ですが、カラムの削除やデータ型の変更は直接行えません。これらの操作を行いたい場合は、新しいテーブルを作成してデータを移し替える、といった手順が必要になります。
{{< /dbmsdiff >}}

### テーブルの削除 (`DROP TABLE`)
不要になったテーブルを完全に削除します。
{{< highlight sql >}}
DROP TABLE テーブル名;
{{< /highlight >}}
{{< badges names="std, MySQL, PostgreSQL, SQLite, Db2, Access" >}}
