---
title: "3-3. テーブルの結合 (JOIN)"
weight: 33
---
リレーショナルデータベースの真価は、テーブルを結合できる点にあります。ここでは、基本となる内部結合と外部結合を学びます。

### 内部結合 (`INNER JOIN`)
`INNER JOIN`は、2つのテーブルで指定したキーの値が**両方に存在する行だけ**を連結します。

- **従業員名と部署名を同時に表示する**
  `employees`テーブルには部署名がなく、`departments`テーブルには従業員名がありません。これらを`department_id`をキーに結合します。
  {{< highlight sql >}}
  SELECT
      e.employee_name,
      d.department_name
  FROM
      employees AS e -- employeesテーブルに e という別名をつける
  INNER JOIN
      departments AS d ON e.department_id = d.department_id;
  {{< /highlight >}}
  `AS`を使ってテーブルに短い別名（エイリアス）をつけると、クエリが簡潔になります。

### 外部結合 (`LEFT JOIN`, `RIGHT JOIN`)
`INNER JOIN`では、片方のテーブルにしか存在しないデータは結果から除外されてしまいます。（例：部署に所属していない従業員、従業員が一人もいない部署）
こういったデータも表示したい場合に使うのが**外部結合**です。

#### `LEFT JOIN`
`LEFT JOIN`は、左側（`FROM`句に書いたテーブル）を主軸とし、 **左側のテーブルの行はすべて残します。** 右側のテーブルに対応するデータがあれば結合し、なければ`NULL`として表示します。

- **全従業員の名前と、所属している場合は部署名を表示する**
  部署未所属の従業員（高橋さん）も結果に含まれます。
  {{< highlight sql >}}
  SELECT
      e.employee_name,
      d.department_name
  FROM
      employees AS e
  LEFT JOIN
      departments AS d ON e.department_id = d.department_id;
  {{< /highlight >}}

#### `RIGHT JOIN`
`RIGHT JOIN`は`LEFT JOIN`の逆で、右側（`JOIN`句に書いたテーブル）を主軸にします。
一般的には、テーブルの順番を入れ替えて`LEFT JOIN`を使う方が、クエリの主軸が分かりやすくなるため好まれます。

{{< dbmsdiff title="FULL OUTER JOIN と SQLite" >}}
`LEFT JOIN`と`RIGHT JOIN`の両方の性質を持つ`FULL OUTER JOIN`（完全外部結合）もありますが、これは全てのRDBMSでサポートされているわけではありません。
**特にSQLiteでは、`RIGHT JOIN`と`FULL OUTER JOIN`はサポートされていません。**

**サポートするDBMS:**
{{< badges names="PostgreSQL, Db2" >}}

**サポートしないDBMS:**
{{< badges names="MySQL" >}}
<small>※MySQLは`UNION`で代用可能です。</small>
{{< /dbmsdiff >}}
