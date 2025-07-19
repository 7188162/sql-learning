---
title: "第1章 演習問題"
weight: 15
tags: ["exercise"]

---

この章で学んだ内容を使い、あなたの理解度を試してみましょう。

1.  以下の仕様で、書籍情報を管理するための `books` テーブルを作成するSQL文を書いてみましょう。
    - テーブル名: `books`
    - カラム:
        - `book_id`: 整数型、主キー、自動で番号が増えるようにする
        - `title`: 文字列型、`NULL`は許可しない
        - `author`: 文字列型
        - `published_date`: 日付型 (SQLiteの場合はTEXT型で良い)
        - `price`: 整数型

[解答・解説はこちら](/appendix/a-3-answers#第1章の解答)