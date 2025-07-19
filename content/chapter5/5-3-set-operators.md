---
title: "5-3. 集合演算子"
weight: 53
---

複数の`SELECT`文の結果を、集合として足したり引いたりすることができます。

- **`UNION` / `UNION ALL` **: 2つの結果セットを縦に連結します。
  - `UNION`: 重複する行は1つにまとめられます。
  - `UNION ALL`: 重複を気にせず、すべての行をそのまま連結します（高速）。
- **`INTERSECT` **: 両方の結果セットに共通して存在する行だけを返します。
- **`EXCEPT` / `MINUS` **: 最初の結果セットに存在し、2番目の結果セットには存在しない行を返します。

{{< dbmsdiff title="集合演算子のサポート" >}}
`UNION`と`UNION ALL`はほとんどのDBMSでサポートされていますが、`INTERSECT`と`EXCEPT`はサポートされていない場合があります。

**`INTERSECT`, `EXCEPT` をサポートする主なDBMS:**
{{< badges names="PostgreSQL, SQLite, Db2, Access" >}}

**`INTERSECT`, `EXCEPT` をサポートしない主なDBMS:**
{{< badges names="MySQL" >}}
{{< /dbmsdiff >}}
