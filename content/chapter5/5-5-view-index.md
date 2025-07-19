---
title: "5-5. ビューとインデックス"
weight: 55
---

データベースをより効率的、安全に使うための2つの重要なオブジェクトを紹介します。

### ビュー (`VIEW`)
ビューは、`SELECT`文に名前を付けて保存した**仮想的なテーブル**です。複雑な結合や集計クエリをビューとして定義しておけば、ユーザーはそのビューに対して単純な`SELECT *`を発行するだけで済みます。セキュリティの観点から、ユーザーに見せたいカラムだけを公開するためにも使われます。
{{< highlight sql >}}
CREATE VIEW employee_department_view AS
SELECT e.employee_name, d.department_name, e.salary
FROM employees AS e
LEFT JOIN departments AS d ON e.department_id = d.department_id;
{{< /highlight >}}

### インデックス (`INDEX`)
インデックスは、本の「索引」と同じで、テーブルの特定のカラムからデータを高速に検索するための仕組みです。`WHERE`句や`JOIN`の`ON`句で頻繁に使われるカラムにインデックスを作成すると、検索パフォーマンスが劇的に向上します。
ただし、データの追加・更新・削除時にはインデックスも更新する必要があるため、これらの処理はわずかに遅くなります。
{{< highlight sql >}}
CREATE INDEX idx_employees_name ON employees (employee_name);
{{< /highlight >}}
