---
title: "2-2. 結果の並べ替えと件数制限"
weight: 22
---
`SELECT`文で取得した結果を、見やすく並べ替えたり、表示する件数を制限する方法を学びます。

### 結果を並べ替える (`ORDER BY`)
`ORDER BY`句で、結果の表示順を指定できます。`ASC`で昇順（小さい順）、`DESC`で降順（大きい順）になります。デフォルトは`ASC`です。
{{< highlight sql >}}
-- 給与が高い順に並べ替える
SELECT employee_name, salary FROM employees ORDER BY salary DESC;
{{< /highlight >}}

{{< dbmsdiff title="取得件数を制限する" >}}
結果の先頭から数件だけを取得したい場合に便利です。この構文はDBMSによる違いが大きいです。

---

**`LIMIT`句 (MySQL, PostgreSQL, SQLite)**
{{< highlight sql >}}
-- 給与が高い従業員TOP3
SELECT employee_name, salary FROM employees ORDER BY salary DESC LIMIT 3;
{{< /highlight >}}
{{< badges names="MySQL, PostgreSQL, SQLite" >}}

---

**`FETCH FIRST`句 (IBM Db2, 標準SQL)**
{{< highlight sql >}}
SELECT employee_name, salary FROM employees ORDER BY salary DESC FETCH FIRST 3 ROWS ONLY;
{{< /highlight >}}
{{< badges names="Db2, std" >}}

---

**`TOP`句 (MS Access, SQL Server)**
{{< highlight sql >}}
SELECT TOP 3 employee_name, salary FROM employees ORDER BY salary DESC;
{{< /highlight >}}
{{< badges names="Access" >}}
{{< /dbmsdiff >}}