#mysql #db
# Mysql
## 開始使用數據庫
> 每個SQL語句都使用分號或者\G結尾。
```sql
-- 查看所有的數據庫
show databases;
-- 創建數據庫 加上 字符集
create database name charset utf8;
-- 刪除一個數據庫
drop database name;
-- 查看創建這個數據庫的數據
show create database name;
-- 選取要使用的數據庫
use name;
-- 需要知道我們在哪個數據中
select database();
```
## 數據庫類型
> MySQL有27種數據類型

|所屬類型|具有數劇類型數目|功能|
|----|----|----|
|整數類型|tinyint,int等5種|存儲整數數據|
|浮點類型|float,double等3種|存儲小數數據|
|字符類型|char,varchar等12種|存儲文本信息|
|時間日期類型|date,time等5種|用於紀錄時間|
|複合類型|enum,set等2種|用於其他功能|
## 創建表
```sql
-- 創建表使用
-- create table 表名(
--   列名1 數劇類型1[修飾符],
--   列名2 數劇類型2[修飾符],
--   列名n 數劇類型n[修飾符]
-- );
create table user(
id int,
name char(20),
pwd char(32)
) charset utf8;
-- 查看建表的語句
show create table tableName;
-- 刪除一個表
drop table tableName;
```
## 增刪改查
> 增加表中數據
```sql
-- 增加表中數據
insert into tableName (列名1,列名2) values (值1,值2);
```
> 查看該表的所有數據
```sql
select * from tableName;
```
> 修改該表數據
```sql
-- update tableName set 列名1=值1,列名2=值2 where 條件;
update team set name="Mark" where id =1;
```
> 刪除該表數據
```sql
-- delete from tableName where 條件
delete from team where id=4;
```
## Table 字符集
```sql
-- 設定mysql字符集
set names utf8;
```
## PHP 使用 mysql
### 連接數數據
```php
<?php
    // 連結數據庫
    $db=mysqli_connect('localhost','root','','maoer');
    //創建table
    $sql="create table xin(id int,name char(20))";

    $re=mysqli_query($db,$sql);
    if($re){
        echo "創建成功";
    }else{
        echo "創建數據失敗";
    }
    //關閉連接
    mysqli_close($db);
?>
```
### 數據查詢
> 使用`mysqli_query`只能得到一個結果集,這個結果集無法直接被查看<br/>
> 可以使用`mysqli_num_row`得到他的所有行數，然後對於每一行，在使用`mysqli_fetch_assoc`來獲取他的具體的行的數據
```sql
-- mysqli_query 獲取結果集
-- mysqli_num_rows 獲取行數
-- mysqli_fetch_assoc 獲取每條數據
```
```php
<?php
    // 連結數據庫
    $db=mysqli_connect('localhost','root','','maoer');
    $set="set names utf8";
    mysqli_query($db,$set);
    $sql="select * from team";
    $re=mysqli_query($db,$sql);
    $num=mysqli_num_rows($re);//獲取結果集行數
    for($a=0;$a<$num;$a++){
        //獲取結果集的每一行
        $row=mysqli_fetch_assoc($re);
        var_dump($row);//打印出來
    }
    mysqli_close($db);
?>
```
###  實戰
> 創建會員登入 註冊 刪除 系統
```sql
create database usermanage charset utf8;

use usermanage;

CREATE TABLE `user`(
    `id` int UNSIGNED NOT NULL AUTO_INCREMENT,
    `name` varchar(20) NOT NULL,
    `pwd` char(32) NOT NULL,
    `level` tinyint UNSIGNED NOT NULL,
    PRIMARY KEY (`id`)
)
ENGINE=MyISAM
DEFAULT CHARACTER SET=utf8;
```
### 可查看該table內
```sql
describe name;
```