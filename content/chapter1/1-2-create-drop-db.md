---
title: "1-2. データベースの作成と削除"
weight: 12
---

{{< infobox title="SQLiteをお使いの方へ" >}}
SQLiteでは、データベースは単一のファイルです。第0章で作成した `.db` ファイルがデータベースそのものなので、`CREATE DATABASE` コマンドは通常使用しません。「DB Browser for SQLite」などのツールで「新しいデータベース」を作成する操作がこれに相当します。
{{< /infobox >}}

### サーバー型RDBMSの場合
MySQLやPostgreSQLなどのサーバー型RDBMSでは、まず作業場所となるデータベースを作成します。

#### データベースの作成
{{< highlight sql >}}
CREATE DATABASE データベース名;
{{< /highlight >}}
{{< badges names="MySQL, PostgreSQL, Db2" >}}

#### データベースの削除
{{< highlight sql >}}
DROP DATABASE データベース名;
{{< /highlight >}}
{{< badges names="MySQL, PostgreSQL, Db2" >}}

{{< infobox title="注意！" >}}
`DROP DATABASE` は、データベース内のすべてのテーブルやデータごと、完全に削除してしまう非常に強力なコマンドです。実行する際は細心の注意を払ってください。
{{< /infobox >}}
