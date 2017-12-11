# MySQL
## 架構
1. database
1. table
1. row
1. column
## keys
> `primary key`獨特性資料、資料不能重複、不能是NULL值、通常把id欄位當作`primary key`
>
> 一個table裡只能有一個`primary key`
>
> `foreign key`table之間串聯使用
## data type
### `INT`數字型態
> `INT`存數字型態
>
> `BIGINT`存大數據數字型態
>
> `DECIMAL`存浮點數
>
### `DATETIME`存日期、時間
* DATETIME
    * YYYY-MM-DD HH:MM:SS
* DATE
    * YYYY-MM-DD
* TIME
    * HH:MM:SS
### `VARCHAR(M)`純字串
### 資料表`constranints`
1. `primary key`一個資料表只能有一個，不能有重複的值出現並且一定要有值，不能是NULL
1. `Not NULL`限制該欄位一定要填
1. `unique`希望這值不要重複的
1. `foreign key`會去檢查是否符合條件
1. `auto_increment`會自動+1並且不重複，可以自己設定從哪個數字開始自增
## `SQL`指令
### 建立database
```sql
CREATE DATABASE IF NOT EXISTS `my_blog`
DEFAULT CHARACTER SET utf8
COLLATE utf8_general_ci;
```
### 使用database
```sql
USE `my_blog`;
```
### 建立table
```sql
CREATE TABLE IF NOT EXISTS `my_blog`.`users`(
    `id` INT NOT NULL AUTO_INCREMENT,
    `username` VARCHAR(255) NOT NULL,
    `password` VARCHAR(255) NOT NULL,
    PRIMARY KEY (`id`))
ENGINE=InnoDB
```
## Query
1. SELECT
1. INSERT
1. UPDATE
1. DELETE
### INSERT
```sql
INSERT INTO `users` (`username`,`password`)
VALUES ('taker.wu','password_demo');
```
### SELECT
```sql
SELECT * FROM `user` WHERE `username`=`taker.wu`;
SELECT `password`,`create_time` FROM `users` WHERE `username`=`taker.wu`;
```
### UPDATE
```sql
UPDATE `users`
SET `password`=`newpassword`
WHERE `username`=`taker.wu`;
```
### DELETE
```sql
DELETE FROM `users`
WHERE `username`='taker.wu';
```
### JOIN
```sql
SELECT * FROM posts
INNER JOIN usera on posts.author_id=users.id
WHERE users.username='taker.wu';
```
### aobut `backticks`
> 遇到關鍵字時就使用`反引號`
```sql
SELECT * FROM `users` WHERE `username`='taker.wu';
```
```sql
SELECT * FROM posts
INNER JOIN usera on posts.author_id=users.id
WHERE users.username='taker.wu';
```
## WHERE
### Logic
> `AND`指兩個條件都要成立
```sql
SELECT * FROM `posts`
WHERE `post_time`>`2017-10-30 16:00:00` AND `category_id`=2;
```
> `OR`只要兩個條件其中一個成立即可
```sql
SELECT * FROM `posts`
WHERE `category_id`=8 OR `category_id`=2;
```
## Comparison
```sql
-- 等於
SELECT * FROM `users`
WHERE `username`='taker.wu';

-- 不等於
SELECT * FROM `users`
WHERE `username`<>'taker.wu';
SELECT * FROM `users`
WHERE `username`!='taker.wu';

-- 大於等於
SELECT * FROM `posts`
WHERE `post_time`>='2017-10-30 16:00:00';
```
### others
> 抓取沒有被分類的文章
```sql
SELECT * FROM `posts`
WHERE `category_id` IS NULL;
```
## Functions
### MAX
```sql
SELECT max(`order`) FROM todos;
SELECT min(`order`) FROM todos;
```
### SUM & AVG
```sql
SELECT SUM(price)
FROM `orders`
WHERE order_time > '2017-10-30 16:00:00';
```
```sql
SELECT AVG(price)
FROM `orders`
WHERE order_time > '2017-10-30 16:00:00';
```
### Count
```sql
SELECT count(*)
FROM posts
WHERE category_id=2;
```