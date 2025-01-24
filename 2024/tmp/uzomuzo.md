## ローカルでデータベースを構築する
- 管理者権限でコマンドプロンプトを開く
- psql.exe -U postgres
- -dはデータベース名
-  -Uはデータベースのユーザ名
- \qでpostgresとの接続切断
- 
```txt
Microsoft Windows [Version 10.0.22631.4602]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\System32>cd C:\Program Files\PostgreSQL\16\bin

C:\Program Files\PostgreSQL\16\bin>ls
'ls' は、内部コマンドまたは外部コマンド、
操作可能なプログラムまたはバッチ ファイルとして認識されていません。

C:\Program Files\PostgreSQL\16\bin>list
'list' は、内部コマンドまたは外部コマンド、
操作可能なプログラムまたはバッチ ファイルとして認識されていません。

C:\Program Files\PostgreSQL\16\bin>psql.exe -U postgres
ユーザー postgres のパスワード:
psql (16.3)
"help"でヘルプを表示します。

postgres=# \dt
                 リレーション一覧
 スキーマ |      名前       |  タイプ  |  所有者
----------+-----------------+----------+----------
 public   | courses         | テーブル | postgres
 public   | home            | テーブル | postgres
 public   | j               | テーブル | postgres
 public   | student_courses | テーブル | postgres
 public   | students        | テーブル | postgres
 public   | teachers        | テーブル | postgres
(6 行)


postgres=# select 1;
 ?column?
----------
        1
(1 行)


postgres=# select * from home;
    日付    |    費目    |      メモ      | 入金額 | 出金額
------------+------------+----------------+--------+--------
 2024-02-03 | 食費       | コーヒーを購入 |   9999 |    380
 2024-02-10 | 給料       | 1月の給料      |   9999 |      0
 2024-02-11 | 教養娯楽費 | 書籍を購入     |   9999 |   2800
 2024-02-14 | 交際費     | 同期会の会費   |   9999 |   5000
 2024-02-18 | 水道光熱費 | 1月の電気代    |   9999 |   7560
 2024-02-18 | 水道光熱費 | 1月の電気代    |      0 |
 2024-02-18 | 水道光熱費 |                |      0 |
 2024-02-18 | 水道光熱費 |                |      0 |      0
 2024-02-03 | 食費       | コーヒーを購入 |      0 |    380
 2024-02-10 | 給料       | 1月の給料      | 280000 |      0
 2024-02-11 | 教養娯楽費 | 書籍を購入     |      0 |   2800
 2024-02-14 | 交際費     | 同期会の会費   |      0 |   5000
 2024-02-18 | 水道光熱費 | 1月の電気代    |      0 |   7560
(13 行)


postgres=# create database shop;
ERROR:  データベース"shop"はすでに存在します
postgres=# show databases
postgres-# ;
ERROR:  設定パラメータ"databases"は不明です
postgres=# \l
                                                                      データベース一覧
   名前    |  所有者  | エンコーディング | ロケールプロバイダー |      照合順序      | Ctype(変換演算子)  | ICUロケール | ICUルール: |     アクセス権限
-----------+----------+------------------+----------------------+--------------------+--------------------+-------------+------------+-----------------------
 postgres  | postgres | UTF8             | libc                 | Japanese_Japan.932 | Japanese_Japan.932 |             |            |
 shop      | postgres | UTF8             | libc                 | Japanese_Japan.932 | Japanese_Japan.932 |             |            |
 template0 | postgres | UTF8             | libc                 | Japanese_Japan.932 | Japanese_Japan.932 |             |            | =c/postgres          +
           |          |                  |                      |                    |                    |             |            | postgres=CTc/postgres
 template1 | postgres | UTF8             | libc                 | Japanese_Japan.932 | Japanese_Japan.932 |             |            | =c/postgres          +
           |          |                  |                      |                    |                    |             |            | postgres=CTc/postgres
(4 行)


postgres=# create database shop2
postgres-# ;
CREATE DATABASE
postgres=# q;
ERROR:  "q"またはその近辺で構文エラー
行 1: q;
      ^
postgres=# \q

C:\Program Files\PostgreSQL\16\bin>psql.exe -U postgres -d shop2
ユーザー postgres のパスワード:
psql (16.3)
"help"でヘルプを表示します。

shop2=# \dt
リレーションが見つかりませんでした。
shop2=# \l
                                                                      データベース一覧
   名前    |  所有者  | エンコーディング | ロケールプロバイダー |      照合順序      | Ctype(変換演算子)  | ICUロケール | ICUルール: |     アクセス権限
-----------+----------+------------------+----------------------+--------------------+--------------------+-------------+------------+-----------------------
 postgres  | postgres | UTF8             | libc                 | Japanese_Japan.932 | Japanese_Japan.932 |             |            |
 shop      | postgres | UTF8             | libc                 | Japanese_Japan.932 | Japanese_Japan.932 |             |            |
 shop2     | postgres | UTF8             | libc                 | Japanese_Japan.932 | Japanese_Japan.932 |             |            |
 template0 | postgres | UTF8             | libc                 | Japanese_Japan.932 | Japanese_Japan.932 |             |            | =c/postgres          +
           |          |                  |                      |                    |                    |             |            | postgres=CTc/postgres
 template1 | postgres | UTF8             | libc                 | Japanese_Japan.932 | Japanese_Japan.932 |             |            | =c/postgres          +
           |          |                  |                      |                    |                    |             |            | postgres=CTc/postgres
(5 行)


shop2=#

```txt
