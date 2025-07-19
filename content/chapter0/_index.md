---
title: "第0章 はじめに：データベースの世界へようこそ"
weight: 1
display_number: "0"
description: "SQLとデータベースの基本概念を学び、学習を始めるための環境を準備します。"
tags: ["chapter"]
---

## 0-1. この資料の目的と対象読者

この学習サイトへようこそ！
本サイトは、 **SQLを初めて学ぶ方**から、 **知識を再確認・体系化したい初中級者の方**までを対象としています。

私たちのゴールは、あなたが単にSQLの構文を覚えるだけでなく、 **「なぜそう書くのか」「実務ではどのように使われるのか」** を理解し、自信を持ってデータベースを操作できるようになることです。

各章では、以下のRDBMS（リレーショナルデータベース管理システム）間の違いも解説し、より実践的な知識の習得を目指します。

{{< badges names="std, MySQL, PostgreSQL, SQLite, Db2, Access" >}}

## 0-2. データベース(DB)とRDBMSとは？

**データベース（DB）** とは、整理されたデータの集まりのことです。身近な例では、スマートフォンの連絡先リストも一種のデータベースと言えます。

その中でも、本サイトで扱うのは **リレーショナルデータベース（RDB）** です。これは、Excelのような行と列で構成される「テーブル（表）」形式でデータを管理し、さらに **テーブル同士を関連付け（リレーション）** できる点が大きな特徴です。

このRDBを管理するためのソフトウェアが、 **RDBMS (Relational Database Management System)** です。

### Excelでのデータ管理との違い
Excelも便利なツールですが、大量のデータや複雑な関連性を持つデータを扱うのは苦手です。RDBMSは、データの整合性を保ち、複数人での同時アクセスを可能にし、高速なデータ検索を実現するために設計されています。

## 0-3. SQLとは何か？

**SQL (Structured Query Language)** とは、RDBMSと「対話」するための専門言語です。
私たちはSQLを使って、データベースに以下のような命令を出します。

- 「このデータを持ってきてください」（検索: `SELECT`）
- 「新しいデータを追加してください」（追加: `INSERT`）
- 「このデータを更新してください」（更新: `UPDATE`）
- 「このデータを削除してください」（削除: `DELETE`）

SQLは国際標準化されており、一度習得すれば多くのRDBMSで応用が利く、非常にコストパフォーマンスの高いスキルです。

## 0-4. 学習の始め方：環境構築ガイド

SQLを学ぶ最も良い方法は、実際に手を動かしてみることです。
本サイトでは、手軽に始められる**SQLite**の利用を推奨します。

### 推奨：SQLiteとGUIツールの導入
SQLiteは、サーバーのインストールが不要で、単一のファイルがデータベースそのものになるという手軽さが魅力です。

1.  **DB Browser for SQLiteのインストール:**
    以下の公式サイトから、お使いのOS（Windows, macOS）に合ったものをダウンロードしてインストールします。これは、SQLの実行やデータの中身を視覚的に確認できる無料のツールです。
    [https://sqlitebrowser.org/](https://sqlitebrowser.org/)

2.  **データベースファイルの作成:**
    DB Browser for SQLiteを起動し、「新しいデータベース」ボタンから、空のデータベースファイル（例: `learning.db`）を作成します。

これだけで準備は完了です。

{{< infobox title="その他の環境" >}}
より本格的な環境を試したい方は、Dockerを使ってMySQLやPostgreSQLを構築する方法もあります。また、Webブラウザ上で手軽にSQLを試せるオンラインサービスも多数存在します。
{{< /infobox >}}

## 0-5. サンプルデータベースの紹介とセットアップ

本サイトでは、全章を通して一貫したサンプルデータベースを使用します。
これは、ある会社の「従業員」「部署」「商品」「売上」などを管理する、実践的なデータモデルです。

### 演習用データセットのダウンロードとセットアップ

1.  **SQLファイルのダウンロード:**
    以下のリンクから、SQLite用のセットアップSQLファイルをダウンロードしてください。
    
    <!-- ※ このリンクは後ほど有効化します -->
    <a href="/downloads/setup_sqlite.sql" download>setup_sqlite.sql をダウンロード</a>

2.  **SQLの実行:**
    DB Browser for SQLiteを開き、「ファイル」メニューから「SQLファイルを開く」を選択し、ダウンロードした`setup_sqlite.sql`を実行します。
    これで、学習に必要なすべてのテーブルとデータが準備されます。

### テーブル構成
演習では、主に以下のテーブルを使用します。
- **departments** : 部署マスタ
- **employees** : 従業員マスタ
- **products** : 商品マスタ
- **customers** : 顧客マスタ
- **sales** : 売上データ
- **quarterly_sales** : 横持ちデータ（演習用）
- **audit_logs** : 監査ログ（トリガー演習用）

### テーブル関連図 (ER図)
各テーブルは、`department_id`や`customer_id`といったキーを使って互いに関連付けられています。

{{< mermaid >}}
erDiagram
    departments ||--o{ employees : "所属"
    customers   ||--o{ sales     : "購入"
    employees   ||--o{ sales     : "販売"
    products    ||--o{ sales     : "商品"

    departments {
        INTEGER department_id PK "部署ID"
        VARCHAR department_name "部署名"
    }
    employees {
        INTEGER employee_id PK "従業員ID"
        VARCHAR employee_name "従業員名"
        INTEGER department_id FK "部署ID"
    }
    products {
        INTEGER product_id PK "商品ID"
        VARCHAR product_name "商品名"
        INTEGER price "単価"
    }
    customers {
        INTEGER customer_id PK "顧客ID"
        VARCHAR customer_name "顧客名"
        VARCHAR region "地域"
    }
    sales {
        INTEGER sale_id PK "売上ID"
        DATE sale_date "売上日"
        INTEGER customer_id FK "顧客ID"
        INTEGER employee_id FK "従業員ID"
        INTEGER product_id FK "商品ID"
        INTEGER quantity "数量"
    }
{{< /mermaid >}}

---

さあ、準備は整いました。次の章から、実際にSQLを書いてデータベースを操作していきましょう！

[第1章 データベースとテーブルの基本操作 (DDL)へ進む](../chapter1)