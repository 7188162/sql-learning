---
title: "5-6. トリガー (TRIGGER)"
weight: 56
---

トリガーは、特定のテーブルに対して`INSERT`, `UPDATE`, `DELETE`が行われたことを**きっかけ（トリガー） **として、 **自動的に実行される**処理です。
例えば、「`employees`テーブルの`salary`が更新されたら、その変更履歴を`audit_logs`テーブルに自動的に記録する」といった用途で使われます。

### SQLiteでのトリガー作成例
{{< highlight sql >}}
CREATE TRIGGER salary_update_log
AFTER UPDATE OF salary ON employees
FOR EACH ROW
BEGIN
    INSERT INTO audit_logs (event_time, event_description)
    VALUES (
        DATETIME('now', 'localtime'),
        'Employee ' || OLD.employee_id || ' salary changed from ' || OLD.salary || ' to ' || NEW.salary
    );
END;
{{< /highlight >}}

{{< infobox title="トリガーの注意点" >}}
トリガーは便利ですが、多用するとデータベースの裏側で何が起きているのか分かりにくくなり、デバッグが困難になることがあります。計画的に利用しましょう。
{{< /infobox >}}
