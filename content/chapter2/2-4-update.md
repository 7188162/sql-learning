---
title: "2-4. 【Update】データの更新 (UPDATE)"
weight: 24
---
既存のデータを更新するには `UPDATE` 文を使います。
{{< highlight sql >}}
UPDATE テーブル名
SET カラム1 = 新しい値1, カラム2 = 新しい値2, ...
WHERE 条件;
{{< /highlight >}}

{{< infobox title="重要：UPDATE文とWHERE句" >}}
`WHERE`句を書き忘れると、 **テーブルの全行が更新されてしまいます！** `UPDATE`文を実行する前は、必ず`WHERE`句を確認する癖をつけましょう。
{{< /infobox >}}

`employees`テーブルの山田さんの給与を更新してみます。
{{< highlight sql >}}
UPDATE employees
SET salary = 310000
WHERE employee_id = 1;
{{< /highlight >}}
