#js #mysql #db
# MySQL

## 基本指令

1. 顯示所有database => `show databases; `
2. 創建database => `create database <name>; `

3. 刪除database => `drop database <name>; `
4. 使用database => `use <database_name>; `
5. 查看當前使用database => `select database(); `

## 架構
1. database
1. table
1. row
1. column
## keys

### `primary key`

> `primary key`獨特性資料、資料不能重複、不能是NULL值、通常把id欄位當作`primary key`
>
> 一個table裡只能有一個`primary key`
>
> `foreign key`table之間串聯使用
### `unique`

> 唯一的

### `primarykey`vs`unique`

1. `primary key`不可為NULL	
2. `unique`可為NULL

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

### `ENUM`選擇型字串類型`size ENUM("M","F")`

### 資料表`constranints`
1. `primary key`一個資料表只能有一個，不能有重複的值出現並且一定要有值，不能是NULL
1. `Not NULL`限制該欄位一定要填
1. `unique`希望這值不要重複的
1. `foreign key`會去檢查是否符合條件
1. `auto_increment`會自動+1並且不重複，可以自己設定從哪個數字開始自增
## `SQL`指令
### 建立database
```sql
CREATE DATABASE`my_blog`
```
### 使用database
```sql
USE `my_blog`;
```
## 顯示當前database下所有table

`show tables;`

### 建立table

```sql
CREATE TABLE person (
    id int primary key auto_increment,
	name VARCHAR(20),
    phone VARCHAR(20),
    age INT,
    gender enum("M","F") not null,
    create_date date default "2001-01-01"
)
```
## 驗證創建table的column

`desc <table _name>;`

## 刪除table

`drop table <table_name>;`

## 增刪改查 Query

1. SELECT
1. INSERT
1. UPDATE
1. DELETE
### INSERT

> INSERT INTO 語句用於向表格中插入新的行。 

> 語法：`INSERT INTO 表名稱 VALUES (值1, 值2,....)`  

> 語法：`INSERT INTO 表名稱 (列1, 列2,...) VALUES (值1, 值2,....)` 

```sql
-- 向表 Persons 插入一條字段 LastName = JSLite 字段 Address = shanghai
INSERT INTO Persons (LastName, Address) VALUES ('JSLite', 'shanghai');
-- 向表 meeting 插入 字段 a=1 和字段 b=2
INSERT INTO meeting SET a=1,b=2;
-- 
-- SQL實現將一個表的數據插入到另外一個表的代碼
-- 如果只希望導入指定字段，可以用這種方法：
-- INSERT INTO 目標表 (字段1, 字段2, ...) SELECT 字段1, 字段2, ... FROM 來源表;
INSERT INTO orders (user_account_id, title) SELECT m.user_id, m.title FROM meeting m where m.id=1;
```
### SELECT

> SELECT 語句用於從表中選取數據。

> 語法：`SELECT 列名稱 FROM 表名稱` 

> 語法：`SELECT * FROM 表名稱` 

```sql
-- 表station取個別名叫s，表station中不包含 字段id=13或者14 的，並且id不等於4的 查詢出來，只顯示id
SELECT s.id from station s WHERE id in (13,14) and user_id not in (4);

-- 從表 Persons 選取 LastName 列的數據
SELECT LastName FROM Persons

-- 結果集中會自動去重複數據
SELECT DISTINCT Company FROM Orders 
-- 表 Persons 字段 Id_P 等於 Orders 字段 Id_P 的值，
-- 結果集顯示 Persons表的 LastName、FirstName字段，Orders表的OrderNo字段
SELECT p.LastName, p.FirstName, o.OrderNo FROM Persons p, Orders o WHERE p.Id_P = o.Id_P 

-- gbk 和 utf8 中英文混合排序最簡單的辦法 
-- ci是 case insensitive, 即 “大小寫不敏感”
SELECT tag, COUNT(tag) from news GROUP BY tag order by convert(tag using gbk) collate gbk_chinese_ci;
SELECT tag, COUNT(tag) from news GROUP BY tag order by convert(tag using utf8) collate utf8_unicode_ci;
```
### UPDATE

> Update 語句用於修改表中的數據。

> 語法：`UPDATE 表名稱 SET 列名稱 = 新值 WHERE 列名稱 = 某值` 

```sql
-- update語句設置字段值為另一個結果取出來的字段
update user set name = (select name from user1 where user1 .id = 1 )
where id = (select id from user2 where user2 .name='小蘇');
-- 更新表 orders 中 id=1 的那一行數據更新它的 title 字段
UPDATE `orders` set title='這裡是標題' WHERE id=1;
```
### DELETE

> DELETE 語句用於刪除表中的行。 

> 語法：`DELETE FROM 表名稱 WHERE 列名稱 = 值` 

```sql
-- 在不刪除table_name表的情況下刪除所有的行，清空表。
DELETE FROM table_name
-- 或者
DELETE * FROM table_name
-- 刪除 Person表字段 LastName = 'JSLite' 
DELETE FROM Person WHERE LastName = 'JSLite' 
-- 刪除 表meeting id 為2和3的兩條數據
DELETE from meeting where id in (2,3);
```
aobut 關鍵字

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
### AND 和 OR
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
### NULL

```sql
// 判斷非NULL
SELECT * FROM `posts`
WHERE `category_id`=8 OR `category_id` is not null

// 判斷NULL
SELECT * FROM `posts`
WHERE `category_id`=8 OR `category_id` is null
```



## ORDER BY

>  語句默認按照升序對記錄進行排序。  

> `ORDER BY` - 語句用於根據指定的列對結果集進行排序。 

> `DESC` - 按照降序對記錄進行排序。  

> `ASC` - 按照順序對記錄進行排序。 

```sql
-- Company在表Orders中為字母，則會以字母順序顯示公司名稱
SELECT Company, OrderNumber FROM Orders ORDER BY Company

-- 後面跟上 DESC 則為降序顯示
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC

-- Company以降序顯示公司名稱，並OrderNumber以順序顯示
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC, OrderNumber ASC

-- 遇到排序浮點數時，會出現排序不正確
SELECT * from People order by age * 1 limit 30
SELECT * from People order by CAST(age as DECIMAL(10,5)) limit 30
```

## IN

> IN - 操作符允許我們在 WHERE 子句中規定多個值。

> IN - 操作符用來指定範圍，範圍中的每一條，都進行匹配。

> IN取值規律，由逗號分割，全部放置括號中。 語法：`SELECT "字段名"FROM "表格名"WHERE "字段名" IN ('值一', '值二', ...);` 

```sql
-- 從表 Persons 選取 字段 LastName 等於 Adams、Carter
SELECT * FROM Persons WHERE LastName IN ('Adams','Carter')
```

## NOT

> NOT - 操作符總是與其他操作符一起使用，用在要過濾的前面。 

```sql
SELECT vend_id, prod_name FROM Products WHERE NOT vend_id = 'DLL01' ORDER BY prod_name;
```

## UNION

> UNION - 操作符用於合併兩個或多個 SELECT 語句的結果集。 

``` sql
-- 列出所有在中國表（Employees_China）和美國（Employees_USA）的不同的僱員名
SELECT E_Name FROM Employees_China UNION SELECT E_Name FROM Employees_USA

-- 列出 meeting 表中的 pic_url，
-- station 表中的 number_station 別名設置成 pic_url 避免字段不一樣報錯
-- 按更新時間排序
SELECT id,pic_url FROM meeting UNION ALL SELECT id,number_station AS pic_url FROM station  ORDER BY update_at;
```

## AS

> as - 可理解為：用作、當成，作為；別名 

> 一般是重命名列名或者表名。 

> 語法：`select column_1 as 列1,column_2 as 列2 from table as 表` 

```sql
SELECT * FROM Employee AS emp
-- 這句意思是查找所有Employee 表裡面的數據，並把Employee表格命名為 emp。
-- 當你命名一個表之後，你可以在下面用 emp 代替 Employee.
-- 例如 SELECT * FROM emp.

SELECT MAX(OrderPrice) AS LargestOrderPrice FROM Orders
-- 列出表 Orders 字段 OrderPrice 列最大值，
-- 結果集列不顯示 OrderPrice 顯示 LargestOrderPrice

-- 顯示表 users_profile 中的 name 列
SELECT t.name from (SELECT * from users_profile a) AS t;

-- 表 user_accounts 命名別名 ua，表 users_profile 命名別名 up
-- 滿足條件 表 user_accounts 字段 id 等於 表 users_profile 字段 user_id
-- 結果集只顯示mobile、name兩列
SELECT ua.mobile,up.name FROM user_accounts as ua INNER JOIN users_profile as up ON ua.id = up.user_id;
```

JOIN

> 用於根據兩個或多個表中的列之間的關係，從這些表中查詢數據。 

* `JOIN`: 如果表中有至少一個匹配，則返回行 
* `INNER JOIN`:在表中存在至少一個匹配時，INNER JOIN 關鍵字返回行。 
* `LEFT JOIN`: 即使右表中沒有匹配，也從左表返回所有的行 
* `RIGHT JOIN`: 即使左表中沒有匹配，也從右表返回所有的行 
* `FULL JOIN`: 只要其中一個表中存在匹配，就返回行 

```sql
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons
INNER JOIN Orders
ON Persons.Id_P = Orders.Id_P
ORDER BY Persons.LastName;
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
## SQL 函數
### MAX

> MAX 函數返回一列中的最大值。NULL 值不包括在計算中。

 >語法：`SELECT MAX("字段名") FROM "表格名"` 

```sql
SELECT max(`order`) FROM todos;
SELECT min(`order`) FROM todos;

-- 列出表 Orders 字段 OrderPrice 列最大值，
-- 結果集列不顯示 OrderPrice 顯示 LargestOrderPrice
SELECT MAX(OrderPrice) AS LargestOrderPrice FROM Orders
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
### COUNT

> COUNT 讓我們能夠數出在表格中有多少筆資料被選出來。  

> 語法：`SELECT COUNT("字段名") FROM "表格名";` 

```sql
-- 表 Store_Information 有幾筆 store_name 欄不是空白的資料。
-- "IS NOT NULL" 是 "這個欄位不是空白" 的意思。
SELECT COUNT (Store_Name) FROM Store_Information WHERE Store_Name IS NOT NULL; 
-- 獲取 Persons 表的總數
SELECT COUNT(1) AS totals FROM Persons;
-- 獲取表 station 字段 user_id 相同的總數
select user_id, count(*) as totals from station group by user_id;
```
## 文字處理函數

|  名稱  |         調用示例         |  示例結果  |            描述            |
| :----: | :----------------------: | :--------: | :------------------------: |
|  LEFT  |    LEFT('abc123', 3)     |    abc     | 返回從左邊取指定長度的子串 |
| RIGHT  |    RIGHT('abc123', 3)    |    321     | 返回從左邊取指定長度的子串 |
| LENGTH |      LENGTH('abc')       |     3      |      返回字符串的長度      |
|  LOWE  |       LOWER(‘ABC’)       |    abc     |                            |
| UPPER  |       UPPER(‘abc’)       |    ABC     |                            |
| LTRIM  |      LTRIM('  abc')      |    abc     |                            |
| RTRIM  |      RTRIM(‘abc  ’)      |    abc     |                            |
| SUBSTR | SUBSTR(“HelloWorld”,2,3) |    ell     |                            |
| CONCAT | CONCAT(“Hello”,‘world’)  | Helloworld |                            |

## 隨機取得一筆數值

```mysql
select * from order by rand() limit 1
```



## 觸發器

語法：

> create trigger <觸發器名稱> 

> { before | after} # 之前或者之後出發

> insert | update | delete # 指明了激活觸發程序的語句的類型 

> on <表名> # 操作哪張表

> for each row # 觸發器的執行間隔，for each row 通知觸發器每隔一行執行一次動作，而不是對整個表執行一次。

> <觸發器SQL語句> 

```sql
DELIMITER $ -- 自定義結束符號
CREATE TRIGGER set_userdate 
BEFORE INSERT 
on `message`
for EACH ROW
BEGIN
  UPDATE `user_accounts` SET status=1 WHERE openid=NEW.openid;
END
$
DELIMITER ; -- 恢復結束符號
```

## 字符串處理

`concat(A," ",B)` => A B

`concat_ws("-",A,B)` => A-B => 分隔符號功能

`substring("Helloworld",1,4)` => 從第1個字到第4個字 => Hell

`substring("Helloworld",4)` => 從第4個字開始沒有結束 => loworld

`substring("Helloworld",-3)` => 從後第3個字開始沒有結束 => rld

`replace("Helloworld","world","Mysql")` => HelloMysql

`reverse("Helloworld")` => dlrowolleH

`char_length("Helloworld")` => 10

`UPPER("hello world")` => HELLO WORLD

`LOWER("HELLO WORLD")` => hello world

## 規則排序、模糊搜索、限制數量

`order by` =>  默認降序、desc升序

`limit` =>  `limit 5` => 返回前5個

`limit` =>  `limit 0,3` => 從0開始返回3個

`like` = > 模糊查詢 => `where last_name like "%w%"` => 查有包含有w字

`like` = > 模糊查詢 => `where last_name like "__w"` => 前有長度2個字後面有w

## 數據聚合處理

### `count()` 對統計出來的數量

```sql
select count(*) from demo.employee
```

### `distinct()` 統計唯一值，去掉重複值在統計

```sql
SELECT distinct title FROM demo.employee;
SELECT count(distinct title) FROM demo.employee;
```

### `group by`數據整合

> 篩選重複的title，並統計出數量

```sql
SELECT title,count(first_name) FROM demo.employee group by title;
```

### `MAX()&MIN()`求最大值和最小值

```sql
SELECT max(salary),title FROM demo.employee group by title ;
```

### `SUM()和AVG()求和和平均值`

```sql
SELECT sum(salary) FROM demo.employee;
SELECT sum(salary),avg(salary) FROM demo.employee group by title;
```

### `HAVING`可以對聚合的數據進行過濾

> `where`是對整個數據在聚合先進行過濾
>
> `having` 則是可以對數據聚合後再進行過濾

```sql
SELECT title,count(*) FROM demo.employee group by title having title="Software Engineer";
```

## SQL邏輯運算符

### `Equal` `not equal`

```sql
select * from demo.employee where salary = 8000;
select * from demo.employee where not salary = 8000;
```

### `LIKE` `Not Like`

```sql
select * from demo.employee where first_name like "%Jack%";
select * from demo.employee where first_name not like "%Jack%";
```

### `greater than` `less than`

```sql
select * from demo.employee where salary >= 6000;
```

### `and` `or`

```sql
select * from demo.employee where salary > 6000 and first_name like "H%";
select * from demo.employee where salary > 6000 or first_name like "H%";
```

### `between`

```sql
select * from demo.employee where salary >= 6000 and salary <= 8000

select * from demo.employee where salary between 6000 and 8000;
```

### `in` `not in`

```sql
select * from demo.employee where salary = 3000 or salary = 6000 or salary = 8000;

select * from demo.employee where salary in (3000,6000,8000);
select * from demo.employee where salary not in (3000,6000,8000);
```

### `case statement`

```sql
select first_name,last_name,title,salary,
    case
        when salary >= 7000 then "height"
        else "low"
    end as tag
from demo.employee
order by salary desc

select *,
    case
        when title like "%Engineer%" then 1
        when title like "%Architect%" then 2
        else 3
    end as tag
from demo.employee
```

## 內置函數(補充)

#### `replace()`

```sql
select replace('$$mysql$$','$','');
```

##  table 對 其他 table查詢

### 透過id關聯查詢

```sql
select * from orders 
where customer_id=(select id from customers where email = "roj@gmail.com");
```

### `FOREIGN KEY()`約束關聯字段

```sql
-- 使用foreign key()限制數據
-- 如下:
-- 限制 customer_id 一定要跟 customers表的id一樣
CREATE TABLE customers(
	id INT AUTO_INCREMENT PRIMARY KEY,
    ...
);
CREATE TABLE orders(
customer_id INT,
...,
FOREIGN KEY(customer_id) REFERENCES customers(id)
);
```

## JOIN

### `inner join`

> 把 `customers`表跟`orders`表之間`重合`的部分去設置一個filter

```sql
select first_name,last_name,SUM(amount)from customers inner join orders on customers.id=orders.customer_id GROUP BY customer_id
```

### `left join`

> 把 `customers`表跟`orders`表之間`重合`的部分並包含了`左邊`的訊息去設置一個filter
>
> IFNULL(如果是NULL,為0)

```sql
// orders 沒匹配到的會顯示NULL
select first_name,last_name,
	CASE
		when SUM(amount) is NULL THEN 0
		else SUM(amount)
	end as total
from customers 
left join orders 
on customers.id=orders.customer_id 
GROUP BY customer_id
order by total desc
```

```sql
select first_name,last_name,IFNULL(SUM(amount),0) as total
from customers 
left join orders 
on customers.id=orders.customer_id 
GROUP BY customer_id
order by total desc
```

### `right join`

> 把 `customers`表跟`orders`表之間`重合`的部分並包含了`右邊`的訊息去設置一個filter

```sql
select *
from customers 
right join orders 
on customers.id=orders.customer_id 
GROUP BY customer_id
```

### `ON DELETE`

> ON DELETE CASCADE 當去刪除customer_id表數據時，他會把相關聯的orders數據也刪除

```sql
CREATE TABLE orders(
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_date DATE,
    amount DECIMAL(8,2),
    customer_id INT,
    FOREIGN KEY(customer_id)
        REFERENCES customers(id)
        ON DELETE CASCADE
);
```

## 多對多表查詢

> 有 book,reviewers,reviews

```sql
select
	first_name,
	last_name,
	count(rating) as count,
	min(ifnull(rating,0)) as min,
	max(ifnull(rating,0)) as max,
	convert(ifnull(avg(rating),0),decimal(3,2)) as AVG,
	if(count(rating) > 0,'active','inactive') as new_status,
	CASE
		when count(rating) > 0 then "active"
		else "inactive"
	end as status
from reviewers
left join reviews
	on reviewers.id = reviews.reviewer_id
GROUP BY reviews.reviewer_id
order by AVG desc
```

## 中文編碼

> 創建時使用utf8

```sql
create database test default charset=utf8 collate=uf8_general_ci
```

> 修改未使用utf8 database和table

```sql
alter database test1 charset utf8 collate utf8_general_ci
alter table user convert to character set utf8
```



## 查詢指令

```sql
查詢中用到的關鍵詞主要包含六個，並且他們的順序依次為

select--from--where--group by--having--order by
```

