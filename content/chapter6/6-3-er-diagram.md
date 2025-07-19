---
title: "6-3. ER図の読み方"
weight: 63
---

**ER図 (Entity-Relationship Diagram)** とは、データベースの設計図です。どのテーブル（エンティティ）があり、それらがどのように関連（リレーションシップ）しているかを視覚的に表現します。

{{< mermaid >}}
erDiagram
    departments ||--o{ employees : "所属"
    customers   ||--o{ sales     : "購入"
    employees   ||--o{ sales     : "販売"
    products    ||--o{ sales     : "商品"
{{< /mermaid >}}

- **エンティティ (Entity):** 四角で表現され、テーブルに相当します。（例：`departments`, `employees`）
- **リレーションシップ (Relationship):** エンティティ間を結ぶ線で、テーブル間の関連を示します。線の名前は関連の内容を表します（例：「所属」）。
- **カーディナリティ (Cardinality):** 線の両端の記号で、対応関係の数を示します。
  - `|`: 1を表す
  - `o`: 0を表す
  - `{`: 多を表す
  - `||--o{`: 「1対0以上（多）」の関係。`departments`の1レコードに対し、`employees`のレコードは0件以上対応する（＝1つの部署に0人以上の従業員が所属する）。

ER図を読むことで、データベース全体の構造を直感的に把握することができます。
