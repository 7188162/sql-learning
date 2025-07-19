---
title: "2-3. 【Create】データの追加 (INSERT)"
weight: 23
---
新しいデータをテーブルに追加するには `INSERT` 文を使います。
{{< highlight sql >}}
INSERT INTO テーブル名 (カラム1, カラム2, ...)
VALUES (値1, 値2, ...);
{{< /highlight >}}
`products`テーブルに新しい商品を追加してみましょう。
{{< highlight sql >}}
INSERT INTO products (product_id, product_name, category, price)
VALUES (302, 'ソフトウェア Y', 'ソフトウェア', 80000);
{{< /highlight >}}
