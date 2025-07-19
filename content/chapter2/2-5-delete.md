---
title: "2-5. 【Delete】データの削除 (DELETE)"
weight: 25
---
データを削除するには `DELETE` 文を使います。
{{< highlight sql >}}
DELETE FROM テーブル名 WHERE 条件;
{{< /highlight >}}

{{< infobox title="重要：DELETE文とWHERE句" >}}
`UPDATE`と同様、`WHERE`句を書き忘れると**テーブルの全データが削除されます！** こちらも実行前には細心の注意が必要です。
{{< /infobox >}}

先ほど追加した商品（`product_id`が302）を削除してみましょう。
{{< highlight sql >}}
DELETE FROM products WHERE product_id = 302;
{{< /highlight >}}

{{< infobox title="コラム: DELETE と TRUNCATE の違い" >}}
`TRUNCATE TABLE テーブル名;` というコマンドもテーブルの全データを削除しますが、`DELETE`とは以下の違いがあります。
- `TRUNCATE`は`WHERE`句が使えず、全件削除しかできない。
- `TRUNCATE`の方が高速に動作する（行を1つずつ削除するのではなく、テーブルを再作成するようなイメージ）。
- `TRUNCATE`はDMLではなくDDLに分類され、トランザクション制御（ROLLBACK）が効かない場合がある。
- **SQLiteには`TRUNCATE`コマンドはありません。**
{{< /infobox >}}
