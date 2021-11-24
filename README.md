## docker run

```
% docker compose up -d
```

※ -d --detach オプションでバックグラウンド起動

## docker bash

```
% docker compose exec postgres /bin/bash
```

## initial SQL

postgresql のサンプル SQL を流し込む

```
pg_restore -U admin -d dvdrental dvdrental.tar
```

## postgres login

```
% psql -U admin dvdrental
```

## ER diagram

![ER](ER-diagram.jpg)

## SELECT

```
select <column_name> from <table_name>;

select first_name from customer;
select * from customer;
```

#### limit

```
select * from customer limit 10;
```

#### count

```
select count (*) from customer ;
```

#### where

```
select * from customer where store_id = 1;
select * from customer where first_name = 'Julie' and store_id = 1;
select * from customer where address_id > 50;
select * from customer where address_id between 1 and 20;
select * from customer where first_name > 'y' ; // Alphabet順でy以降
```

#### like

% 0 文字以上のワイルドカード

`_` 1 文字のワイルドカード

```
select * from customer where last_name like '%son';
select * from customer where last_name like '___son';
```

#### order by

```
select * from customer order by address_id;
select * from customer order by address_id desc; //降順
```

#### group by

```
select count(*) from inventory group by film_id;

select film_id ,
count(*) from inventory
group by film_id
order by count desc;
```

#### join

```
select inventory_id,film.title,inventory.last_update from inventory
left join film on inventory.film_id = film.film_id;
```

auther_table
| id | name | book_id |
| -- | --------- |---|
| 1 | JK・ローリング | 1 |
| 2 | 太宰治 |2|
|3 | 芥川龍之介|3|

book_table
| id | name |
| -- | --------- |
| 1 | ハリポタ |
| 2 | 人間失格 |
|4 | 吾輩は猫である|

- inner join
  - 積集合
  ```
  select * from auther inner join book on auther.book_id = book.id;
  ```
  | id  | name           | book_id | id  | name     |
  | --- | -------------- | ------- | --- | -------- |
  | 1   | JK・ローリング | 1       | 1   | ハリポタ |
  | 2   | 太宰治         | 2       | 2   | 人間失格 |
- left outer join

  - 外部キーを持つテーブルをベースに抽出（外部キーのレコードが存在しなくても取得）

  ```
  select * from auther left join book on auther.book_id = book.id;
  ```

  | id  | name           | book_id | id   | name     |
  | --- | -------------- | ------- | ---- | -------- |
  | 1   | JK・ローリング | 1       | 1    | ハリポタ |
  | 2   | 太宰治         | 2       | 2    | 人間失格 |
  | 3   | 芥川龍之介     | 3       | null | null     |

- right outer join

  - 外部キーが存在しなくても子テーブルにレコードが存在すれば取得

  ```
  select * from auther left join book on auther.book_id = book.id;
  ```

  | id   | name           | book_id | id  | name           |
  | ---- | -------------- | ------- | --- | -------------- |
  | 1    | JK・ローリング | 1       | 1   | ハリポタ       |
  | 2    | 太宰治         | 2       | 2   | 人間失格       |
  | null | null           | 4       | 4   | 吾輩は猫である |

#### 各 dvd のタイトル毎に借りられた件数を表示

```
select film.title, count(film.title)
from inventory
left join film on inventory.film_id = film.film_id
group by film.title
having count(film.title) >= 7
order by count(film.title) desc;
```

inventory テーブルと film テーブルを外部結合しタイトル毎に借りられた件数を取得

件数が 7 件以上のレコードを降順ソートで表示

※where 句は集計前に実行されてしまうので`having`を活用
