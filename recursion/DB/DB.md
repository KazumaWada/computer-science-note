# DBMS(Database Management System)

- 全ての DB の機能を抽象化したシステム
- アプリのクライアント、開発者、データサイエンティスト、DB 管理者が使いやすいように作る
  ///////////////////DB のシステムの主要な要素////////////////////////////

### 特性

- transaction: 情報の状態を変化させる処理
- ソフトウェアはデータと関数。データを保存するシステムはむちゃくちゃ重要。
- query(照会): DB からデータを照会するときに使う言語。

#### 一貫性と統合性(consistency and integrity)

user の email が更新された場合、user のクレジットカード、住所などに保存されている mail も全て更新される必要がある。更新されるときに、その変更は有効であるかどうかが判断された後に更新される。これが一貫性を保証する。

- データエントリ: 銀行口座に入力されたデータが int ではなく、string の場合、transaction は実行されるべきではない。(=一貫性を保障するため、"制約"と呼ばれるルールに従う必要がある。)

#### 永続性(durability)

- もし DB に障害が起こった場合でも、一度 transaction(または commit)された情報は維持される。ハードディスクに直接保存するなどの処理が適応される。DB のデータは、ソフトウェアに保存されているが、ハードウェアにもバックアップを保持している

#### 独立性と並行性(isolation and concurrency)

- DB はデータを独立、分離することができる.
  ↓ どういうことか。
  ネットオークションで異なるパソコン A,B がそれぞれ 200 枚売られているジーンズを 10 枚買おうと試みる、
  ↓
  データが独立していない場合、A が購入したときに B も数秒遅れて購入した場合、B は A の購入が反映されるまで、間違った情報をパソコン越しにみることになる。既に A は 10 枚購入しているのですから。しかし B は 10 枚まだ買われていない枚数をみることになる。
  ↓
  データが分離されていた場合、A,B どちらが先に処理されても構わない。先に実行された方が終わってから、後の人の処理が実行されるから。

#### 原子性(Atomicity)全体的に起こるか、全体的に全く起こらないか。

- 「原子」＝　分割できないという意味から来ている。
- all or nothing(transaction が全体的に起こるか、全く起こらないか)が適応されている
  ↓
  大学のシステムに、学生が授業料を払ったとする。
  ↓
  何らかのシステム障害が起こって学生の授業料が払われていないと表示されてしまう
  ↑
  これを防ぐには。。
  ↓
  「DB が原子性を持つ」all or nothing. つまり、授業料が支払われているか or 何も起こらないという 2 つのみの選択肢を持つ。つまり、障害が起こった場合、「何も起こらない」が適応され、rollback(コードが start に戻りまた同じコードが再び実行される。)
  ///////////////////////////////////////////////////////////////////

#### セキュリティ

- データのアクセスや更新に対して行われる安全対策
  ↓
  例: a さんがアプリ内で障害を起こし、サポートセンターに連絡した。サポートセンターは、a さんのサポートに必要なデータを取得することはできるが、それ以外のデータにはアクセスできないようになっている。
  ↓
  a <-> user データ(全てのデータ取得)
  people support a さん <-> support_user(サポートに必要なデータのみ取得

### DB の内部

- query processor: SQL 言語を解釈して低層に命令するために使われる
- SQL 言語は DDL(data-definition language == データ定義言語)//データの集合に紐づけられている規則(userDB と productDB を繋げるって ruby でやったじゃん。)
  DML(data-manipulation language == データ操作言語)//データを追加、アクセス、操作ができる

##################################
#########"DBのセットアップ"###############
##################################
% mysql -u root -p
% CREATE USER 'your_user_name'@'localhost' IDENTIFIED BY 'your_password';
% GRANT ALL PRIVILEGES ON %.* TO 'your_user_name'@'localhost';
または、
mysql> GRANT ALL PRIVILEGES ON *.* TO 'user1'@'localhost';


mysql> CREATE DATABASE db01
    -> ;
Query OK, 1 row affected (0.01 sec)

mysql> SHOW DATABASE;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'DATABASE' at line 1
mysql> SHOW DATABASEs;
+--------------------+
| Database           |
+--------------------+
| db01               |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.01 sec)

mysql> USE db01;
Database changed
mysql> SHOW DATABASEs;
+--------------------+
| Database           |
+--------------------+
| db01               |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql> DROP DATABASE db01
    -> ;
Query OK, 0 rows affected (0.02 sec)

mysql> SHOW DATABASEs;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql> CREATE DATABASE test_db;
Query OK, 1 row affected (0.01 sec)

mysql> SHOW DATABASEs;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test_db            |
+--------------------+
5 rows in set (0.00 sec)

mysql> USE test_db;
Database changed
mysql> SHOW DATABASEs;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test_db            |
+--------------------+
5 rows in set (0.00 sec)

mysql> CREATE TABLE departments (id INT, name VARCHAR(255), start_date DATE);
mysql> SHOW TABLES FROM test_db;
+-------------------+
| Tables_in_test_db |
+-------------------+
| departments       |
+-------------------+
1 row in set (0.00 sec)
mysql> show create table departments;
+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table       | Create Table                                                                                                                                                                                  |
+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| departments | CREATE TABLE `departments` (
  `id` int DEFAULT NULL,
  `name` varchar(255) DEFAULT NULL,
  `start_date` date DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
##################################
#########"indexを設定"###############
##################################
mysql> mysql> ALTER TABLE departments ADD INDEX id_index(id);
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'departments ADD INDEX id_index(id)' at line 1
mysql> ALTER TABLE departments ADD INDEX id_index(id);
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> SHOW INDEX FROM departments;
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table       | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| departments |          1 | id_index |            1 | id          | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
1 row in set (0.01 sec)
##################################
############"制約"####################
##################################
##"↑これによってid_indexを一意の保証する主キーとして設定する事ができる"<-制約という。#####################
- 主キー(メインのDBのid)と外部キーのidのindexは必ず同じ値である必要がある。
- 主キーのデータは、外部キーのtableにあるデータを先に削除しない限り、削除することはできない。
↓
実際のテーブルの繋げ方。
% CREATE TABLE authors (id INT PRIMARY KEY);
% CREATE TABLE books (id INT, author_id INT, FOREIGN KEY 
% author_fk(author_id) REFERENCES authors(id));
##################################
############"TABLEの更新/削除"####################
##################################
% ALTER TABLE current_name change_name
(change_name: -table名,culumn名,index名,add culumn,delete culumnなど色々当てはまるここに。);
- table名の変更
% ALTER TABLE table_name RENAME TO new_table_name;
- table内のidカラムをBIGINTへ&それを一意制約にする
% ALTER TABLE new_departments MODIFY id BIGINT
- カラムの削除
% ALTER TABLE table_name DROP column_name;

##################################
############"データをTABLEに挿入"####################
##################################
mysql> CREATE TABLE products(id INT, name VARCHAR(255) NOT NULL);
Query OK, 0 rows affected (0.02 sec)
mysql> INSERT INTO products (id, name) VALUES (1, 'バナソニックレンジ'), (2, 'ゾニーカメラ'), (3, 'とうじばエアコン'), (4, 'シャーブテレビ');
mysql> SELECT products.id, products.name FROM products;
↑"これでもいいけど、全部のカラムを出力したい時は*を使用して書く事ができる"↓
mysql> SELECT products.* FROM products;
+------+-----------------------------+
| id   | name                        |
+------+-----------------------------+
|    1 | バナソニックレンジ          |
|    2 | ゾニーカメラ                |
|    3 | とうじばエアコン            |
|    4 | シャーブテレビ              |
+------+-----------------------------+
4 rows in set (0.00 sec)


















  
