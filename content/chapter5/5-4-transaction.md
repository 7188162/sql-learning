---
title: "5-4. トランザクション制御"
weight: 54
---

**トランザクション**とは、「すべて成功するか、すべて失敗するかのどちらか」であるべき一連の処理のまとまりです。例えば、銀行振込は「Aさんの口座から引き落とし」と「Bさんの口座へ入金」の2つの`UPDATE`文で構成されますが、片方だけ成功しては困ります。

- **`COMMIT` **: トランザクション内のすべての変更をデータベースに**恒久的に反映（確定） **します。
- **`ROLLBACK` **: トランザクション内のすべての変更を**取り消し** 、トランザクション開始前の状態に戻します。

{{< highlight sql >}}
START TRANSACTION; -- トランザクション開始 (DBMSにより異なる場合がある)

UPDATE employees SET salary = salary - 50000 WHERE employee_id = 1;
UPDATE employees SET salary = salary + 50000 WHERE employee_id = 2;

-- ここで内容を確認し、問題なければCOMMIT、問題があればROLLBACKする
COMMIT;
-- もしくは ROLLBACK;
{{< /highlight >}}
